title: JS tips and tricks
author:
  name: Raúl Hernández
  twitter: raulghm
  url: https://github.com/raulghm
output: index.html
controls: true
style: style.css
theme: juanbrujo/cleaver-beerjs

--

# JS tips and tricks
## Cosas feas y otras peor, y como lidiar con ellas

--

<img src="img/me.png" alt="me">

--

# 1. Array.from()
## Convirtiendo a verdaderos array

--

# 1.1 Array.from()
## Generar una lista de selectores html

```html
<ul>
  <li>uno</li>
  <li>dos</li>
  <li>tres</li>
  <li>cuatro</li>
  <li>cinco</li>
</ul>
```

-- 

# 1.2 Array.from()
## Buscar todos los nodos con el selector li del documento

```javascript
let lis = document.querySelectorAll('li')
```

--

# 1.3 Hacer algo con estos nodos (?)
## funciones, ES5... (?)

```javascript
lis.length
lis.values
lis.keys

lis.forEach((li, k) => {
  console.log(k, li.textContent)
})

for (li of lis) {
  li.addEventListener...
}
```

--

# 1.4 Usando Array.prototype.slice.call()
## El viejo slice y call() de ES5

```javascript
let lis = Array.prototype.slice.call(lis)
```

--

# 1.5 Aplicando Array.from() ✨
## Convirtiendo a un array de verdad :huemul-trusted:

```javascript
let lis = Array.from(lis) // la magia
```
--

# 1.6 Aplicar metodos de un array normal
## map, find, filter, reduce... 200, 100 lo que...

```javascript
let lis = Array.from(lis) // la magia

const find = lis.find(li => li.textContent == 'dos')
const map = lis.map(li => li.textContent + ' JS es feo')

console.log(find)
console.log(map)
...
```

--

# 2. new Set() 🌈
## Usando el nuevo objeto Set() para filtrar

--

# 2.1. Filtrando un arreglo simple a la antigua
## Removiendo items con filter + indexOf

```javascript
uniq = (data) => {
  return data.filter((elem, pos, arr) => {
    return arr.indexOf(elem) == pos
  })
}

uniq([1,2,3,3]) // [1,2,3]
```

--

# 2.2. Ahora con Set() ✨
## new Set([...])

```javascript
[...new Set([1,2,3,3])]

// (3) [1, 2, 3]
```
--

# 2.3. Filtrando un arreglo de objetos? 🤔
## Mapear y filtrar un arreglo normal

```javascript
const modas = [
  {id:0, name:'BEM'},
  {id:1, name:'MutableCSS'},
  {id:2, name:'Styled Components'},
  {id:3, name:'MutableCSS'},
  {id:4, name:'BEM'},
]
```
--

# 2.3. Filtrando a la antigua de nuevo
## filter() + indexOf()

```javascript
const filterNormal = (data) => data
            .filter((obj, pos, arr) => {
              return arr.map(mapObj => mapObj.name).indexOf(obj.name) === pos
            })

console.time()
const res = filterNormal(modas)
console.timeEnd()

```
--

# 2.3. Ahora filtrando y construyendo con Set
## A ver, a ver...

```javascript
const filterSet = (data) => [...new Set(data.map(item => {
    return JSON.stringify({ name: item.name })
  })
)].map(s => JSON.parse(s))

console.time()
const res = filterSet(modas)
console.timeEnd()

```
--

# 2.4 FilterNormal VS FilterSet
## console.time(😱)

--

# 2.5 FilterNormal VS FilterSet
## Resultados filtrando 1000 registros ⏰

--

# 2.6 Resultados

```javascript

filterNormal(modas)
// 25.985107421875ms 

filterSet(modas)
// 1.395263671875ms
```

--

# Dynamic import
## import + promise ✨

> https://github.com/tc39/proposal-dynamic-import

> This repository contains a proposal for adding a "function-like" import() module loading syntactic form to JavaScript. It is currently in stage 3 of the TC39 process.

--

# Que puedo hacer con dynamic import? 🤔

--

# Dynamic import
## Modulos que son pages, bajo demanda

```javascript
const page = await import('./pages/about');

// render page
page.render();
```

### Para usar async/await
babel-polyfill o babel-runtime + babel-plugin-transform-runtime

--

# Dynamic import
## Ejemplo un poco mas cientifico 👨‍🔬
```html
<!DOCTYPE html>
<nav>
  <a href="books.html" data-entry-module="books">Books</a>
  <a href="movies.html" data-entry-module="movies">Movies</a>
  <a href="video-games.html" data-entry-module="video-games">Video Games</a>
</nav>

<main>Content will load here!</main>

<script>
  const main = document.querySelector("main");
  for (const link of document.querySelectorAll("nav > a")) {
    link.addEventListener("click", e => {
      e.preventDefault();

      import(`./section-modules/${link.dataset.entryModule}.js`)
        .then(module => {
          module.loadPageInto(main);
        })
        .catch(err => {
          main.textContent = err.message;
        });
    });
  }
</script>
```
--

# Dynamic import
## En la actualidad

### Code Splitting
Es parte del core de parcel
https://parceljs.org/code_splitting.html

### Vue Router
https://router.vuejs.org/en/advanced/lazy-loading.html
https://babeljs.io/docs/plugins/syntax-dynamic-import

```js
const Home = () => import(/* webpackChunkName: "home" */ './Home.vue');
const routes = [
  { path: '/', name: 'home', component: Home },
];
```
--

# PostCSS tips

* Evitar SASS u otros pre-procesadores, (apoyar la nueva especificación )
* PostCSS es como el Babel de CSS
* Compila a CSS3 o CSS4 nativo
* Puedes utilizar cosas experimentales de forma segura
* Todos se convierte a través de modulos JS


- 👉 https://github.com/postcss/postcss-cli **CLI**
- 👉 http://cssnext.io **- Use tomorrow’s CSS syntax, today.**

-- 

# PostCSS
## Testear componentes

https://raulghm.github.io/cata-variables/test

colors.css
```css
:root {

  /* Colors */

  --color-white: #fff;
  --color-black: #111;
}
```

--

# PostCSS
## Testear componentes

Config test (config.json)
```javascript
{
  "lint": true,
  "postcss-reporter": {
    "plugins": ["stylelint", "postcss-bem-linter"],
    "throwError": true
  }
}
```

```
npm run build-test
```

--

# Vue Tips
## 👉 http://www.vuemastery.com

```html
<input v-model.lazy="..." />
// Sincroniza el model solo en el evento `change` no con el evento `input`

<input v-model.number="..." />
// Siempre devuelve un numero

<input v-model.trim="..." />
// Remueve los espacio en blanco

<form @submit.prevent="foo()">...
Prevenir que la pagina recarge

<img @mouseover.once="showImage">
Solo gatilla una vez
```

--

# Gracias ❤️
