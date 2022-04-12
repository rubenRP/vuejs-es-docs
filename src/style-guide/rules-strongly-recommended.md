# Reglas de Prioridad B: Muy Recomendado

Se ha encontrado que estas reglas mejoran la legibilidad y/o la experiencia del desarrollador en la mayoría de los proyectos. Tu código se ejecutará aún si no las respetas, pero las violaciones deberían ser raras y estar bien justificadas.

## Archivos de Componentes

**Siempre que haya un sistema de compilación disponible para concatenar archivos, cada componente debe estar en su propio archivo.**

Esto te ayudará a encontrar más rápidamente un componente cuando necesites editarlo o revisar cómo usarlo.

<div class="style-example style-example-bad">
<h3>Incorrecto</h3>

```js
app.component('TodoList', {
  // ...
})

app.component('TodoItem', {
  // ...
})
```

</div>

<div class="style-example style-example-good">
<h3>Correcto</h3>

```
components/
|- TodoList.js
|- TodoItem.js
```

```
components/
|- TodoList.vue
|- TodoItem.vue
```

</div>

## Notación de nombre de los Componente de un Solo Archivo (SFC)

**Los nombres de archivo de los [Componentes de un Solo Archivo](/guide/scaling-up/sfc.html) deben ser siempre PascalCase o siempre kebab-case.**

El autocompletado de los editores de código funciona mejor cuando se utiliza PascalCase ya que este es consistente con la forma en la que referenciamos componentes en JS(X) y las templates, siempre que sea posible. Sin embargo, este tipo de nombres en los archivos puede crear problemas en los sistemas de archivos que no distinguen entre mayúsculas y minúsculas, razón por la cual kebab-case también es perfectamente aceptable.

<div class="style-example style-example-bad">
<h3>Incorrecto</h3>

```
components/
|- mycomponent.vue
```

```
components/
|- myComponent.vue
```

</div>

<div class="style-example style-example-good">
<h3>Correcto</h3>

```
components/
|- MyComponent.vue
```

```
components/
|- my-component.vue
```

</div>

## Nombres de los componentes base

**Los componentes base (también conocidos como componentes de presentación, "silentes"" o puros) que aplican estilos y convenciones específicos de la aplicación deben comenzar todos con un prefijo específico, como `Base`, `App` o `V`.**

::: details Explicación detallada
Estos componentes sientan las bases para un estilo y un comportamiento consistentes en tu aplicación. Pueden contener **solo**:

- Elementos HTML,
- Otros componentes base, y
- Componentes de UI de terceros.

Pero **nunca** contendrán un estado global (por ejemplo, de una store de Vue como [Pinia](https://pinia.vuejs.org/)).

Sus nombres a menudo incluyen el nombre del elemento que envuelven (por ejemplo, `BaseButton`, `BaseTable`), a menos que no exista ningún elemento para tu propósito específico (por ejemplo, `BaseIcon`). Si creas componentes similares para un contexto más específico, casi siempre consumirán esos componentes (por ejemplo, `BaseButton` se puede usar en `ButtonSubmit`).

Algunas ventajas de esta convención:

- Cuando se organizan alfabéticamente en los editores, los componentes básicos de tu aplicación se enumeran juntos, lo que los hace más fáciles de identificar.

- Dado que los nombres de los componentes siempre deben tener varias palabras, esta convención evita que tengas que elegir un prefijo arbitrario para paquetes de componentes simples (por ejemplo, `MyButton`, `VueButton`).

- Dado que estos componentes se usan con tanta frecuencia, es posible que desees simplemente hacerlos globales en lugar de importarlos en todas partes. Un prefijo hace esto posible usando Webpack:

  ```js
  const requireComponent = require.context(
    './src',
    true,
    /Base[A-Z]\w+\.(vue|js)$/
  )
  requireComponent.keys().forEach(function (fileName) {
    let baseComponentConfig = requireComponent(fileName)
    baseComponentConfig =
      baseComponentConfig.default || baseComponentConfig
    const baseComponentName =
      baseComponentConfig.name ||
      fileName.replace(/^.+\//, '').replace(/\.\w+$/, '')
    app.component(baseComponentName, baseComponentConfig)
  })
  ```

  :::

<div class="style-example style-example-bad">
<h3>Incorrecto</h3>

```
components/
|- MyButton.vue
|- VueTable.vue
|- Icon.vue
```

</div>

<div class="style-example style-example-good">
<h3>Correcto</h3>

```
components/
|- BaseButton.vue
|- BaseTable.vue
|- BaseIcon.vue
```

```
components/
|- AppButton.vue
|- AppTable.vue
|- AppIcon.vue
```

```
components/
|- VButton.vue
|- VTable.vue
|- VIcon.vue
```

</div>

## Nombres de componentes de instancia única

**Los componentes que solo deben tener una única instancia activa deben comenzar con el prefijo `The`, para indicar que solo puede haber uno.**

Esto no significa que el componente solo se use en una sola página, sino que solo se usará una vez _por página_. Estos componentes no aceptan props, ya que son específicos para tu aplicación, no del contexto de la misma. Si encuentras la necesidad de agregar props, es buen indicio de que este es en realidad un componente reutilizable que solo se usa una vez por página _por ahora_.

<div class="style-example style-example-bad">
<h3>Incorrecto</h3>

```
components/
|- Heading.vue
|- MySidebar.vue
```

</div>

<div class="style-example style-example-good">
<h3>Correcto</h3>

```
components/
|- TheHeading.vue
|- TheSidebar.vue
```

</div>

## Nombres de componentes fuertemente acoplados

**Los componentes hijos que están fuertemente acoplados con su padre deben incluir el nombre del componente padre como prefijo.**

Si un componente solo tiene sentido en el contexto de un único componente padre, dicha relación debe ser evidente en su nombre. Dado que usualmente los editores organizar los archivos alfabéticamente, esto también mantiene a estos archivos relacionados cercanos visualmente.

::: details Explicación detallada
Puede que tengas la tentación de resolver este problema anidando componentes hijos en directorios nombrados como su componente padre. Por ejemplo:

```
components/
|- TodoList/
   |- Item/
      |- index.vue
      |- Button.vue
   |- index.vue
```

o:

```
components/
|- TodoList/
   |- Item/
      |- Button.vue
   |- Item.vue
|- TodoList.vue
```

Esto no es recomendado ya que da como resultado:

- Muchos archivos con nombres similares, haciendo difícil el cambio rápido entre archivos en los editores de código.
- Muchos subdirectorios anidados, lo que aumenta el tiempo que lleva buscar componentes en la barra lateral de un editor.
  :::

<div class="style-example style-example-bad">
<h3>Incorrecto</h3>

```
components/
|- TodoList.vue
|- TodoItem.vue
|- TodoButton.vue
```

```
components/
|- SearchSidebar.vue
|- NavigationForSearchSidebar.vue
```

</div>

<div class="style-example style-example-good">
<h3>Correcto</h3>

```
components/
|- TodoList.vue
|- TodoListItem.vue
|- TodoListItemButton.vue
```

```
components/
|- SearchSidebar.vue
|- SearchSidebarNavigation.vue
```

</div>

## Orden de las palabras en los nombres de los componentes

**Los nombres de los componentes deben comenzar con las palabras del nivel más alto (a menudo el más general) y terminar con palabras descriptivas.**

::: details Explicación detallada
Te estarás preguntando:

> "¿Por qué forzar un lenguaje poco natural al nombrar componentes?"

En inglés natural, los adjetivos y otros descriptores suelen aparecer antes de los sustantivos, mientras que las excepciones requieren palabras conectoras. Por ejemplo:

- Café _con_ leche
- Sopa _del_ día
- Visitante _del_ museo

Definitivamente, puedes incluir estas palabras conectoras en los nombres de los componentes si lo deseas, pero el orden sigue siendo importante.

También ten en cuenta que **lo que se considera "el nivel más alto" será contextual a tu aplicación**. Por ejemplo, imagina una aplicación con un formulario de búsqueda. Puedes incluir componentes como este:

```
components/
|- ClearSearchButton.vue
|- ExcludeFromSearchInput.vue
|- LaunchOnStartupCheckbox.vue
|- RunSearchButton.vue
|- SearchInput.vue
|- TermsCheckbox.vue
```

Como puedes notar, es bastante difícil ver qué componentes son específicos de la búsqueda. Ahora cambiemos el nombre de los componentes de acuerdo con la regla:

```
components/
|- SearchButtonClear.vue
|- SearchButtonRun.vue
|- SearchInputExcludeGlob.vue
|- SearchInputQuery.vue
|- SettingsCheckboxLaunchOnStartup.vue
|- SettingsCheckboxTerms.vue
```

Puesto que los editores normalmente organizan los archivos alfabéticamente, todas las relaciones importantes entre los componentes ahora son evidentes de un vistazo.

Pudieras verte tentado a resolver este problema de manera diferente, anidando todos los componentes de búsqueda en un directorio de "búsqueda", luego todos los componentes de configuración en un directorio de "configuración". Recomendamos considerar este enfoque solo en aplicaciones muy grandes (por ejemplo, más de 100 componentes), por las siguientes razones:

- Por lo general, lleva más tiempo navegar a través de subdirectorios anidados que desplazarse por un solo directorio de "componentes".
- Los conflictos de nombres (por ejemplo, múltiples componentes `ButtonDelete.vue`) dificultan la navegación rápida a un componente específico en un editor de código.
- La refactorización se vuelve más difícil, porque buscar y reemplazar a menudo no es suficiente para actualizar las referencias relativas a un componente movido.
  :::

<div class="style-example style-example-bad">
<h3>Incorrecto</h3>

```
components/
|- ClearSearchButton.vue
|- ExcludeFromSearchInput.vue
|- LaunchOnStartupCheckbox.vue
|- RunSearchButton.vue
|- SearchInput.vue
|- TermsCheckbox.vue
```

</div>

<div class="style-example style-example-good">
<h3>Correcto</h3>

```
components/
|- SearchButtonClear.vue
|- SearchButtonRun.vue
|- SearchInputQuery.vue
|- SearchInputExcludeGlob.vue
|- SettingsCheckboxTerms.vue
|- SettingsCheckboxLaunchOnStartup.vue
```

</div>

## Componentes de cierre automático

**Los componentes sin contenido deben cerrarse automáticamente en [Componentes de un Solo Archivo](/guide/scaling-up/sfc.html), templates de cadena y [JSX](/guide/extras/render-function.html#jsx-tsx), pero nunca en templates del DOM.**

Los componentes de cierre automático comunican que no solo no tienen contenido, sino que están **destinados** a no tener contenido. Es la diferencia entre una página en blanco en un libro y una etiquetada como "Esta página se dejó en blanco intencionalmente". Tu código también es más limpio sin la etiqueta de cierre innecesaria.

Desafortunadamente, HTML no permite que los elementos personalizados se cierren automáticamente, solo los [elementos "vacíos" oficiales](https://www.w3.org/TR/html/syntax.html#void-elements). Es por eso que la estrategia solo es posible cuando el compilador de templates de Vue puede alcanzar la template antes del DOM y luego servir el HTML compatible con las especificaciones del DOM.

<div class="style-example style-example-bad">
<h3>Incorrecto</h3>

```vue-html
<!-- En Componentes de un Solo Archivo, templates de cadenas y JSX -->
<MyComponent></MyComponent>
```

```vue-html
<!-- En templates del DOM -->
<my-component/>
```

</div>

<div class="style-example style-example-good">
<h3>Correcto</h3>

```vue-html
<!-- En Componentes de un Solo Archivo, templates de cadenas y JSX -->
<MyComponent/>
```

```vue-html
<!-- En templates del DOM -->
<my-component></my-component>
```

</div>

## Nombre de componente enmarcados en templates

**En la mayoría de los proyectos, los nombres de los componentes siempre deben estar en PascalCase en [Componentes de un Solo Archivo](/guide/scaling-up/sfc.html) y templates de cadenas, pero en kebab-case en las templates del DOM.**

PascalCase tiene algunas ventajas sobre kebab-case:

- Los editores pueden autocompletar los nombres de los componentes en las templates, porque PascalCase también se usa en JavaScript.
- `<MyComponent>` es visualmente más diferente de un elemento HTML de una sola palabra que `<my-component>`, porque hay dos caracteres diferentes (las dos mayúsculas), en lugar de solo uno (el guión).
- Si usas elementos personalizados que no son de Vue en tus templates, como un componente web, PascalCase garantiza que tus componentes de Vue permanezcan claramente visibles.

Desafortunadamente, debido a la insensibilidad de mayúsculas y minúsculas de HTML, las templates del DOM deben seguir usando kebab-case.

También ten en cuenta que si ya has invertido mucho en kebab-case, la coherencia con las convenciones de HTML y poder usar el mismo enmarcado en todos tus proyectos puede ser más importante que las ventajas enumeradas anteriormente. En esos casos, **también es aceptable usar kebab-case en todas partes.**

<div class="style-example style-example-bad">
<h3>Incorrecto</h3>

```vue-html
<!-- En Componentes de un Solo Archivo y templates de cadenas -->
<mycomponent/>
```

```vue-html
<!-- En Componentes de un Solo Archivo y templates de cadenas -->
<myComponent/>
```

```vue-html
<!-- En templates del DOM -->
<MyComponent></MyComponent>
```

</div>

<div class="style-example style-example-good">
<h3>Correcto</h3>

```vue-html
<!-- En Componentes de un Solo Archivo y templates de cadenas -->
<MyComponent/>
```

```vue-html
<!-- En templates del DOM -->
<my-component></my-component>
```

O

```vue-html
<!-- En todas partes -->
<my-component></my-component>
```

</div>

## Component name casing in JS/JSX

**Component names in JS/[JSX](/guide/extras/render-function.html#jsx-tsx) should always be PascalCase, though they may be kebab-case inside strings for simpler applications that only use global component registration through `app.component`.**

::: details Detailed Explanation
In JavaScript, PascalCase is the convention for classes and prototype constructors - essentially, anything that can have distinct instances. Vue components also have instances, so it makes sense to also use PascalCase. As an added benefit, using PascalCase within JSX (and templates) allows readers of the code to more easily distinguish between components and HTML elements.

However, for applications that use **only** global component definitions via `app.component`, we recommend kebab-case instead. The reasons are:

- It's rare that global components are ever referenced in JavaScript, so following a convention for JavaScript makes less sense.
- These applications always include many in-DOM templates, where [kebab-case **must** be used](#component-name-casing-in-templates).
  :::

<div class="style-example style-example-bad">
<h3>Incorrecto</h3>

```js
app.component('myComponent', {
  // ...
})
```

```js
import myComponent from './MyComponent.vue'
```

```js
export default {
  name: 'myComponent'
  // ...
}
```

```js
export default {
  name: 'my-component'
  // ...
}
```

</div>

<div class="style-example style-example-good">
<h3>Correcto</h3>

```js
app.component('MyComponent', {
  // ...
})
```

```js
app.component('my-component', {
  // ...
})
```

```js
import MyComponent from './MyComponent.vue'
```

```js
export default {
  name: 'MyComponent'
  // ...
}
```

</div>

## Full-word component names

**Component names should prefer full words over abbreviations.**

The autocompletion in editors make the cost of writing longer names very low, while the clarity they provide is invaluable. Uncommon abbreviations, in particular, should always be avoided.

<div class="style-example style-example-bad">
<h3>Incorrecto</h3>

```
components/
|- SdSettings.vue
|- UProfOpts.vue
```

</div>

<div class="style-example style-example-good">
<h3>Correcto</h3>

```
components/
|- StudentDashboardSettings.vue
|- UserProfileOptions.vue
```

</div>

## Prop name casing

**Prop names should always use camelCase during declaration, but kebab-case in templates and [JSX](/guide/extras/render-function.html#jsx-tsx).**

We're simply following the conventions of each language. Within JavaScript, camelCase is more natural. Within HTML, kebab-case is.

<div class="style-example style-example-bad">
<h3>Incorrecto</h3>

```js
props: {
  'greeting-text': String
}
```

```vue-html
<WelcomeMessage greetingText="hi"/>
```

</div>

<div class="style-example style-example-good">
<h3>Correcto</h3>

```js
props: {
  greetingText: String
}
```

```vue-html
<WelcomeMessage greeting-text="hi"/>
```

</div>

## Multi-attribute elements

**Elements with multiple attributes should span multiple lines, with one attribute per line.**

In JavaScript, splitting objects with multiple properties over multiple lines is widely considered a good convention, because it's much easier to read. Our templates and [JSX](/guide/extras/render-function.html#jsx-tsx) deserve the same consideration.

<div class="style-example style-example-bad">
<h3>Incorrecto</h3>

```vue-html
<img src="https://vuejs.org/images/logo.png" alt="Vue Logo">
```

```vue-html
<MyComponent foo="a" bar="b" baz="c"/>
```

</div>

<div class="style-example style-example-good">
<h3>Correcto</h3>

```vue-html
<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo"
>
```

```vue-html
<MyComponent
  foo="a"
  bar="b"
  baz="c"
/>
```

</div>

## Simple expressions in templates

**Component templates should only include simple expressions, with more complex expressions refactored into computed properties or methods.**

Complex expressions in your templates make them less declarative. We should strive to describe _what_ should appear, not _how_ we're computing that value. Computed properties and methods also allow the code to be reused.

<div class="style-example style-example-bad">
<h3>Incorrecto</h3>

```vue-html
{{
  fullName.split(' ').map((word) => {
    return word[0].toUpperCase() + word.slice(1)
  }).join(' ')
}}
```

</div>

<div class="style-example style-example-good">
<h3>Correcto</h3>

```vue-html
<!-- In a template -->
{{ normalizedFullName }}
```

```js
// The complex expression has been moved to a computed property
computed: {
  normalizedFullName() {
    return this.fullName.split(' ')
      .map(word => word[0].toUpperCase() + word.slice(1))
      .join(' ')
  }
}
```

</div>

## Simple computed properties

**Complex computed properties should be split into as many simpler properties as possible.**

::: details Detailed Explanation
Simpler, well-named computed properties are:

- **Easier to test**

  When each computed property contains only a very simple expression, with very few dependencies, it's much easier to write tests confirming that it works correctly.

- **Easier to read**

  Simplifying computed properties forces you to give each value a descriptive name, even if it's not reused. This makes it much easier for other developers (and future you) to focus in on the code they care about and figure out what's going on.

- **More adaptable to changing requirements**

  Any value that can be named might be useful to the view. For example, we might decide to display a message telling the user how much money they saved. We might also decide to calculate sales tax, but perhaps display it separately, rather than as part of the final price.

  Small, focused computed properties make fewer assumptions about how information will be used, so require less refactoring as requirements change.
  :::

<div class="style-example style-example-bad">
<h3>Incorrecto</h3>

```js
computed: {
  price() {
    const basePrice = this.manufactureCost / (1 - this.profitMargin)
    return (
      basePrice -
      basePrice * (this.discountPercent || 0)
    )
  }
}
```

</div>

<div class="style-example style-example-good">
<h3>Correcto</h3>

```js
computed: {
  basePrice() {
    return this.manufactureCost / (1 - this.profitMargin)
  },

  discount() {
    return this.basePrice * (this.discountPercent || 0)
  },

  finalPrice() {
    return this.basePrice - this.discount
  }
}
```

</div>

## Quoted attribute values

**Non-empty HTML attribute values should always be inside quotes (single or double, whichever is not used in JS).**

While attribute values without any spaces are not required to have quotes in HTML, this practice often leads to _avoiding_ spaces, making attribute values less readable.

<div class="style-example style-example-bad">
<h3>Incorrecto</h3>

```vue-html
<input type=text>
```

```vue-html
<AppSidebar :style={width:sidebarWidth+'px'}>
```

</div>

<div class="style-example style-example-good">
<h3>Correcto</h3>

```vue-html
<input type="text">
```

```vue-html
<AppSidebar :style="{ width: sidebarWidth + 'px' }">
```

</div>

## Directive shorthands

**Directive shorthands (`:` for `v-bind:`, `@` for `v-on:` and `#` for `v-slot`) should be used always or never.**

<div class="style-example style-example-bad">
<h3>Incorrecto</h3>

```vue-html
<input
  v-bind:value="newTodoText"
  :placeholder="newTodoInstructions"
>
```

```vue-html
<input
  v-on:input="onInput"
  @focus="onFocus"
>
```

```vue-html
<template v-slot:header>
  <h1>Here might be a page title</h1>
</template>

<template #footer>
  <p>Here's some contact info</p>
</template>
```

</div>

<div class="style-example style-example-good">
<h3>Correcto</h3>

```vue-html
<input
  :value="newTodoText"
  :placeholder="newTodoInstructions"
>
```

```vue-html
<input
  v-bind:value="newTodoText"
  v-bind:placeholder="newTodoInstructions"
>
```

```vue-html
<input
  @input="onInput"
  @focus="onFocus"
>
```

```vue-html
<input
  v-on:input="onInput"
  v-on:focus="onFocus"
>
```

```vue-html
<template v-slot:header>
  <h1>Here might be a page title</h1>
</template>

<template v-slot:footer>
  <p>Here's some contact info</p>
</template>
```

```vue-html
<template #header>
  <h1>Here might be a page title</h1>
</template>

<template #footer>
  <p>Here's some contact info</p>
</template>
```

</div>
