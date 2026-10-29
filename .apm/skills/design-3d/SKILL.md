---
name: "design-3d"
description: "Cria elementos 3D para sites com Three.js ou React Three Fiber: objeto central, iluminação, partículas, ligação ao scroll, desempenho e fallback. Use ao pedir objeto 3D, hero com WebGL, partículas ou cena 3D em landing page."
---

# Design 3D para Web

Planeja e constrói um elemento 3D que serve à página e continua rápido e acessível. Entregue o código completo e a lista de ativos necessários, sem explicar o que é WebGL. Para o scroll, use `design-scroll`; para a base visual, `design-frontend`.

## Modos de entrada

**Modo Pedido direto**: o usuário disse o que o objeto é e o que ele faz. Construa e entregue; pergunte depois.

**Modo Briefing**: o pedido é vago. Perguntas em **uma única mensagem**, numeradas e curtas:
1. Qual é o objeto ou a ideia visual e qual papel tem na história da página?
2. Existe um modelo 3D (arquivo GLB, com licença) ou é para criar de forma procedural?
3. O objeto reage a quê (scroll, mouse, tempo)?
4. Qual é a stack (arquivo único, React ou Next.js)?
5. Há imagem de fallback para quem não tem WebGL?

Nunca invente licença ou origem de modelo. Marque `[PENDENTE: ...]`.

## Vale a pena usar 3D?

Use quando o objeto é o centro da história e o público aceita o custo. Evite quando:
- a página precisa carregar muito rápido em celular simples;
- o objeto só decora e não explica nada;
- não há modelo de qualidade nem tempo para ajustar luz e materiais.

Alternativas leves: SVG animado, canvas 2D, vídeo curto em loop, CSS 3D com `transform` e sequência de quadros pré-renderizados (seção abaixo).

## Fluxo de entrega

1. **Definir o papel do objeto** e o comportamento ao scroll, mouse e tempo.
2. **Escolher a origem:** modelo GLB com licença clara, geometria procedural (esferas, torus, malhas de pontos, partículas) ou forma criada em código.
3. **Montar cena:** câmera, luz, ambiente, material, animação.
4. **Ligar ao scroll** por progresso normalizado de 0 a 1.
5. **Otimizar e criar fallback.**
6. **Checklist e entrega.**

## Base com Three.js (módulos ES)

```js
import * as THREE from "three";

const canvas = document.querySelector("#hero3d");
const renderer = new THREE.WebGLRenderer({ canvas, antialias: true, alpha: true });
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
renderer.outputColorSpace = THREE.SRGBColorSpace;
renderer.toneMapping = THREE.ACESFilmicToneMapping;

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(35, 1, 0.1, 100);
camera.position.set(0, 0, 6);

scene.add(new THREE.HemisphereLight(0xffffff, 0x223355, 0.8));
const key = new THREE.DirectionalLight(0xffffff, 2);
key.position.set(3, 4, 5);
scene.add(key);

function resize() {
  const { clientWidth: w, clientHeight: h } = canvas;
  renderer.setSize(w, h, false);
  camera.aspect = w / h;
  camera.updateProjectionMatrix();
}
new ResizeObserver(resize).observe(canvas);
resize();
```

Carregue o `three` por importmap com versão fixa, ou pelo gerenciador de pacotes em projetos com build.

## Ligação ao scroll

Mantenha um valor `progress` (0 a 1) e suavize antes de aplicar:
```js
let target = 0, current = 0;
ScrollTrigger.create({ trigger: ".story", start: "top top", end: "bottom bottom",
  onUpdate: s => (target = s.progress) });

renderer.setAnimationLoop(() => {
  current += (target - current) * 0.08;
  object.rotation.y = current * Math.PI * 2;
  object.position.y = -current * 1.2;
  renderer.render(scene, camera);
});
```

## React Three Fiber

- `<Canvas dpr={[1, 2]}>` limita a densidade de pixels.
- `useFrame` para animação, `useGLTF` do drei para modelos, `Environment` para luz de ambiente.
- `ScrollControls` do drei liga a cena ao scroll, ou use ScrollTrigger e passe o progresso por uma ref.
- Carregue o canvas com `Suspense` e um fallback visível.

## Luz, materiais e partículas

- **Luz:** ambiente (HDRI ou hemisfério) mais uma luz principal e um contraluz. Ajuste exposição antes de mexer em cores.
- **Materiais:** `MeshStandardMaterial` resolve a maioria. `MeshPhysicalMaterial` com transmissão simula vidro, mas custa caro. Use só em objetos pequenos e teste em celular.
- **Partículas:** use `Points` ou `InstancedMesh`. Milhares são aceitáveis; centenas de milhares exigem teste.
- **Reflexo e vapor:** simule com mapa de ambiente e sprites translúcidos antes de pensar em efeitos pesados.

## Alternativa fotorrealista: sequência de quadros

Em vez de renderizar em tempo real, renderize o objeto uma vez (em Blender ou outra ferramenta), exporte de 60 a 120 quadros em WebP ou AVIF e desenhe o quadro certo em um canvas 2D conforme o progresso do scroll. O resultado parece fotografia e roda em qualquer dispositivo.
```js
const total = 90;
const frames = Array.from({ length: total }, (_, i) => {
  const img = new Image();
  img.src = `frames/objeto-${String(i).padStart(3, "0")}.webp`;
  return img;
});
const ctx = canvas.getContext("2d");
ScrollTrigger.create({
  trigger: ".story", start: "top top", end: "bottom bottom",
  onUpdate: s => {
    const i = Math.min(total - 1, Math.round(s.progress * (total - 1)));
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.drawImage(frames[i], 0, 0, canvas.width, canvas.height);
  }
});
```
- Mostre o primeiro quadro como imagem comum enquanto os demais carregam.
- O peso cresce com a quantidade de quadros e a resolução. Meça o total e defina um orçamento.

**Poses por faixa de quadros.** Uma única sequência serve a várias cenas: cada cena usa uma faixa (por exemplo, 0 a 45% em pé, 45 a 70% virando, 70 a 100% tombando). Some escala, rotação e deslocamento em keyframes ligados ao progresso da seção. Não há troca de modelo.
```js
const poses = [
  { at: 0,   frame: 0,    scale: 1.5,  y: "20em" },
  { at: 0.3, frame: 0.45, scale: 1.4,  y: "0" },
  { at: 0.7, frame: 0.7,  scale: 1.54, y: "3em", rotate: -10 },
  { at: 1,   frame: 0.99 }
];
// ScrollTrigger com scrub na seção alta: interpole entre poses vizinhas e
// use gsap.utils.interpolate para escala, rotação e deslocamento.
```
Na saída, reduza a escala a 0 e a opacidade a 0 e deixe as próximas seções cobrirem a camada (`z-index` maior).

**Via Lottie (canvas).** Exportar os quadros em um Lottie com WebP embutido funciona, mas o JSON vira um arquivo único e pesado.
- Referência medida: 342 quadros a 1300×1350 px renderam 15,9 MB brutos e cerca de 11 MB em brotli. Carregar o arquivo antes de mostrar o objeto atrasa a primeira imagem.
- Em portfólio, prefira de 60 a 120 quadros, 800 a 1000 px de lado e WebP ou AVIF em arquivos separados, com carga progressiva (quadro 0 primeiro, depois o resto em segundo plano).
- Para o canvas 2D, o `drawImage` acima dispensa a biblioteca Lottie.
- Teste em rede lenta e tenha um poster estático para `prefers-reduced-motion`.
- A rotação fica fixa em um eixo. Para vários ângulos, use WebGL.
- Use ativos próprios. Registre a origem e a licença de cada quadro.

## Desempenho e fallback

- Limite `devicePixelRatio` a 2.
- Pause o laço de render quando o canvas sai da tela (`IntersectionObserver`) e quando a aba fica oculta.
- Libere recursos que saem de uso (`geometry.dispose()`, `material.dispose()`, `texture.dispose()`).
- Modelos: GLB comprimido (Draco ou meshopt), texturas em tamanho adequado, meta de poucos MB no total.
- Detecte falta de WebGL e `prefers-reduced-motion`: mostre imagem estática (poster) no lugar do canvas.
- O canvas recebe `aria-hidden="true"` se for decorativo, e o conteúdo importante vive no HTML.
- Em celular, reduza partículas, desligue antialias se necessário e simplifique materiais.

## Ativos

- Todo modelo, textura ou HDRI tem origem e licença registradas. Sem licença clara, não use.
- Prefira criar o objeto em código ou usar ativos próprios.
- Não reproduza produtos ou personagens de marcas.

## Checklist antes de entregar

- O objeto tem papel claro na história da página.
- Canvas redimensiona sem distorcer.
- Laço de render pausa fora da tela e com a aba oculta.
- Fallback estático existe para sem WebGL e `prefers-reduced-motion`.
- Densidade de pixels limitada, recursos liberados.
- Licença dos ativos registrada.
- Nada inventado: lacunas marcadas com `[PENDENTE: ...]`.
