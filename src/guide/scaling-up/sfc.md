# Componentes de un Solo Archivo (Single File Components)

## Introducción

Los componentes de un Solo Archivo de Vue (también conocidos como archivos `*.vue`, abreviados como **SFC**) son un formato de archivo especial que nos permite encapsular la plantilla, la lógica, **y** los estilos de un componente de Vue en un único archivo. Aquí tenemos un ejemplo de SFC:

```vue
<script>
export default {
  data() {
    return {
      greeting: 'Hello World!'
    }
  }
}
</script>

<template>
  <p class="greeting">{{ greeting }}</p>
</template>

<style>
.greeting {
  color: red;
  font-weight: bold;
}
</style>
```

Como podemos ver, Vue SFC es una extensión natural del trío clásico de HTML, CSS y JavaScript. Los bloques `<template>`, `<script>` y `<style>` encapsulan y ubican la vista, la lógica y los estilos de un componente en el mismo archivo. La sintaxis completa se define en la [Especificación de sintaxis SFC](/api/sfc-spec).

## ¿Por qué SFC?

Si bien los archivos SFC requieren un paso de compilación, tiene numerosos beneficios:

- Creación componentes modularizados utilizando la sintaxis familiar de HTML, CSS y JavaScript
- [Ubicación de intereses inherentemente acoplados](#que-sucede-con-la-separacion-de-intereses)
- Plantillas pre-compiladas
- [CSS con alcance de componente](/api/sfc-css-features)
- [Sintaxis más ergonómica cuando se trabaja con la API de composición](/api/sfc-script-setup)
- Más optimizaciones en tiempo de compilación mediante el análisis cruzado de plantillas y scripts
- [Soporte para IDE](/guide/scaling-up/tooling.html#ide-support) con autocompletado y verificación de tipos para expresiones de plantilla
- Soporte de reemplazo de módulos en caliente (HMR)

SFC es una característica principal de Vue como framework y es el enfoque recomendado para usar Vue en los siguientes escenarios:

- Aplicaciones de página única (SPA)
- Generación de sitios estáticos (SSG)
- Cualquier interfaz no trivial en la que se pueda justificar un paso de compilación para una mejor experiencia de desarrollo (DX).

Dicho esto, nos damos cuenta de que hay escenarios en los que los SFC pueden parecer excesivos. Esta es la razón por la que Vue aún se puede usar a través de JavaScript puro sin un paso de compilación. Si solo busca mejorar un gran HTML estático con interacciones ligeras, también puedes consultar [petite-vue](https://github.com/vuejs/petite-vue), un subconjunto de 6kb de Vue optimizado para mejoras progresivas.

## ¿Cómo funciona?

Vue SFC es un formato de archivo específico del framework y debe ser precompilado por [@vue/compiler-sfc](https://github.com/vuejs/core/tree/main/packages/compiler-sfc) en JavaScript estándar y CSS. Un SFC compilado es un módulo JavaScript (ES) estándar, lo que significa que con una configuración de compilación adecuada puede importar un SFC como un módulo:

```js
import MyComponent from './MyComponent.vue'

export default {
  components: {
    MyComponent
  }
}
```

Las etiquetas `<style>` dentro de los SFC generalmente se inyectan como etiquetas `<style>` nativas durante el desarrollo para admitir actualizaciones en caliente. Para entornos de producción, se pueden extraer y fusionar en un solo archivo CSS

Puedes jugar con SFC y explorar cómo se compilan en [Vue SFC Playground](https://sfc.vuejs.org/).

En proyectos reales, normalmente integramos el compilador SFC con una herramienta de compilación como [Vite](https://vitejs.dev/) o [Vue CLI](http://cli.vuejs.org/) (que se basa en [webpack](https://webpack.js.org/)), y Vue proporciona herramientas oficiales de construcción de proyectos para que puedas comenzar con SFC lo más rápido posible. Consulta más detalles en la sección [Herramientas SFC](/guide/scaling-up/tooling).

## ¿Qué sucede con la separación de intereses?

Algunos usuarios que provienen de un entorno de desarrollo web tradicional pueden tener la preocupación de que los SFC mezclen diferentes intereses en el mismo lugar, ¡se suponía que HTML/CSS/JS se debían separar!

Para responder a esta pregunta, es importante que estemos de acuerdo en que **la separación de intereses no es igual a la separación de tipos de archivos**. El objetivo final de los principios de ingeniería es mejorar la capacidad de mantenimiento del código base. La separación de preocupaciones, cuando se aplica dogmáticamente como separación de tipos de archivos, no nos ayuda a alcanzar ese objetivo en el contexto de aplicaciones frontend cada vez más complejas.

En el desarrollo de interfaces de usuario moderna, hemos descubierto que, en lugar de dividir el código base en tres capas enormes que se entrelazan entre sí, tiene mucho más sentido dividirlas en componentes poco acoplados y componerlos. Dentro de un componente, su plantilla, lógica y estilos están inherentemente acoplados, y ubicarlos juntos hace que el componente sea más cohesivo y mantenible.

Ten en cuenta que incluso si no te gusta la idea de los componentes de un solo archivo, aún puedes aprovechar sus funciones de precompilación y recarga en caliente separando tu JavaScript y CSS en archivos separados usando [importaciones src](/api/sfc-spec.html#src-imports).
