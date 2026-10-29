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

Alternativas leves: SVG animado, canvas 2D, vídeo curto em loop, CSS 3D com `transform`.

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
