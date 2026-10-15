# Café Raíz

## Propuesta del emprendimiento

- **Nombre:** Café Raíz
- **Problema que resuelve:** Es difícil encontrar café artesanal de calidad y con origen conocido en un solo lugar. La mayoría del café comercial viene mezclado o muy quemado, lo que no deja sentir el verdadero sabor del grano.
- **Cliente objetivo:** Personas de 20 a 40 años que toman café seguido, valoran el café de especialidad y apoyan marcas artesanales.

---

## Registro PASO 1 — Módulo y etiqueta

- **Etiqueta registrada:** `<tarjeta-producto>`
- **¿Por qué lleva guion en el nombre?**  
  Se le puso guion porque el estándar de Custom Elements pide obligatoriamente que lleve al menos un guion (`-`). De esa manera el navegador sabe que es una etiqueta propia creada por nosotros y no una etiqueta nativa de HTML (como `div` o `p`), evitando problemas si a futuro salen etiquetas nuevas con el mismo nombre.
- **¿Cómo importa la página el módulo?**  
  Se importó al final del `index.html` con esta línea:
  ```html
  <script type="module" src="./componentes/tarjeta-producto.js"></script>
  ```
  Se usó `type="module"` porque así el navegador reconoce que es código moderno en módulos, lo corre en modo estricto y espera a que el documento termine de cargar antes de ejecutarlo.

---

## Registro PASO 2 — Plantilla, Shadow DOM y slots

- **Uso de la plantilla:**  
  Se creó una sola etiqueta `<template>` afuera de la clase para no gastar memoria volviendo a crear el HTML cada vez que se usa una tarjeta en la página.
- **Slots que se configuraron:**
  - `foto`: recibe la imagen `<img slot="foto">`. Si no se pasa ninguna foto, se dejó un emoji ☕ como respaldo.
  - `nombre`: recibe el nombre del producto dentro de `<h3 slot="nombre">`.
  - Sin nombre (por defecto): recibe la descripción del producto. Si se deja vacío, se dejó como respaldo el texto `"Sin descripción"`.
- **¿Qué pasaría si no se clonara la plantilla?**  
  Si se agregara la plantilla con `appendChild(template.content)` sin clonar, los elementos se moverían directo a la primera tarjeta y la plantilla se quedaría vacía. Por eso, la primera tarjeta saldría bien pero las otras dos saldrían en blanco. Al usar `cloneNode(true)`, se generó una copia nueva para cada tarjeta.

---

## Registro PASO 3 — Atributo precio y estilos con :host

- **Método que reacciona al cambio de precio:**  
  Se usó el método `attributeChangedCallback`. Se declaró `"precio"` dentro de `observedAttributes` para que el navegador avise cada vez que el valor cambie. Cuando cambia, se buscó `#precio-pantalla` en el Shadow DOM y se actualizó su texto en pantalla.
- **Sincronización con getter y setter:**  
  Se crearon el get y set en JavaScript para que la propiedad y el atributo queden sincronizados:
  ```js
  get precio() { return Number(this.getAttribute("precio")) || 0; }
  set precio(v) { this.setAttribute("precio", v); }
  ```
- **Color de marca y variables CSS:**
  - Se eligió el color café `#6f4e37`.
  - Se usó la variable `--color-marca`.
  - En el componente se colocó con respaldo: `var(--color-marca, #6f4e37)`. Si la página no define la variable, el componente no se rompe y usa ese color café por defecto.
  - En la página (`estilos.css`), se definió la variable en `:root`:
    ```css
    :root {
      --color-marca: #6f4e37;
    }
    ```
  - La página no cambia nada más del componente por dentro, respetando el encapsulamiento con `:host`.

---

## Registro PASO 4 — Tres productos del emprendimiento

Se colocaron tres productos reales en `index.html`:

1. **Café Negro Tostado:** Grano entero 250 g, notas de chocolate y caramelo — **$18**
2. **Latte Artesanal:** Espresso doble con leche vaporizada y arte latte — **$12**
3. **Cold Brew Artesanal:** Macerado en frío por 18 horas, botella de 355 ml — **$10**

A cada tarjeta se le colocó su foto con texto en `alt`, su nombre, descripción y precio.

---

## Reto extra (opcional)

Se implementó el atributo `destacado`. Cuando se le agrega a una tarjeta (`<tarjeta-producto precio="18" destacado>`), con el selector `:host([destacado])` se le colocó un borde dorado con sombra y se activó el cartel `"⭐ Destacado"`.

---

## Declaración de uso de IA

Se utilizó IA como apoyo para consultar dudas sobre la sintaxis de Web Components y estructurar los archivos iniciales. Se revisó y probó todo el código para asegurar que cumpla con los requisitos pedidos en clase.
