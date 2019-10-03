# код airbnb : : Aphrodite bindings for Hypernova for rendering AMP pages.


Надеемся, опыт компании Airbnb пригодится всем, кто использует Node.js для решения задач серверного рендеринга.

Использование серверного рендеринга на Node.js означает повышенную вычислительную нагрузку на систему. Такая нагрузка отличается от традиционной, связанной, преимущественно, с обработкой операций ввода-вывода. Именно в таких ситуациях платформа Node.js показывает себя наилучшим образом. В нашем случае, мы, попытавшись добиться высокой производительности серверной части приложения, столкнулись с рядом проблем, которые удалось решить, поработав над архитектурой системы и воспользовавшись вспомогательными механизмами, такими, как nginx и HAProxy.

As Airbnb builds more of its Frontend around Server Side Rendering, we took a look at how to optimize our server configurations to support it.  
https://medium.com/airbnb-engineering/operationalizing-node-js-for-server-side-rendering-c5ba718acfc9  

[01.12.2014] Использование Node.js и Redis при построении приложений с высокой степенью масштабируемости https://drive.google.com/drive/folders/1l0_dWRGqlEHWwGyaJFD_0b7NTsIA7K5d  

[24 июля 2018] Node.js для решения задач серверного рендеринга в Airbnb  
https://docs.google.com/document/d/1g5DDrJBhl-9PyNFsX5NVpmAX_uX_9X1V4-SzjM6Hyvs/  

[28 марта 2019] Как мы пилили серверный рендеринг и что из этого вышло  
https://docs.google.com/document/d/1Stgohn-I9WZ6aLFiYSvsHzKZfVEiHecLgShlyjpAlew/  

AMPHTML ads — это прекрасный способ создания рекламы, которая быстро грузится. Ведь если на AMP страницах, открывающихся моментально, будут располагаться медленно загружаемые баннеры, пользователи такую рекламу скорее всего просто не увидят. Да и на обычных страницах — медленно загружаемая реклама раздражает пользователей. Поэтому логично саму рекламу (рекламные креативы) делать с помощью AMP.

Пользователи по-разному потребляют контент на десктопе и на мобильных устройствах. Например, при чтении с телефона пользователи бросают чтение длинной статьи гораздо раньше, чем при чтении с десктопа (а вы дочитали до этого момента?:)). На мобильных устройствах в последнее время все более популярен формат историй (stories), и AMP stories как раз и являются способом делать такие истории быстро и без лишних усилий.

Кроме того, сейчас разрабатывается версия AMP для электронной почты (AMPHTML for email), которая позволит создавать красивые интерактивные письма с помощью AMP.

Не забывайте также, что AMP можно использовать просто как формат вставки контента на вашем сайте. Например если в React или Angular приложении требуется отображать новости, статьи или карточки товара, то их можно хранить в AMP формате, предзагружать и потом мгновенно показывать в web приложении (или даже в нативном приложении). Не обязательно использовать AMP для всей страницы целиком — AMP содержимым могут быть и маленькие кусочки контента.

С помощью AMP вы можете создавать как сайты целиком, так и отдельные страницы, баннеры, истории, а также использовать AMP как быстрый и компактный формат вставки контента.

[8 августа 2018] Используем AMP как библиотеку общего назначения для создания быстрых динамических сайтов  
https://docs.google.com/document/d/1wIODuaLtwXprmXLfkIIw8YVEcw4vGtmE8jtP2OiVHqo/edit  

[Aphrodite](https://github.com/Khan/aphrodite) bindings for [Hypernova](https://github.com/airbnb/hypernova)
for rendering AMP pages.

## Install

```sh
npm install hypernova-amp
```

## Usage

Here's how use use it in your module:

```js
import { renderReactAmpWithAphrodite } from 'hypernova-amp';
import MyComponent from './src/MyComponent.jsx';

export default renderReactAmpWithAphrodite(
  'MyComponent.hypernova.js', // this file's name (or really any unique name)
  MyComponent,
  {/* Options */},
);
```


## Options

### `prependCSS` (string)
Inserts css before the generated CSS.

### `appendCSS` (string)
Inserts css after the generated CSS.

### `enableAmpBind` (bool)
Set to `true` to enable [amp-bind](https://www.ampproject.org/docs/reference/components/amp-bind).
Default: `false`.
This will transform any attribute named `amp-bind-x` into an attribute named `[x]`. This is necessary
because react does not support `[x]` prop even in conjunction with `is` (see next section).

### `enableRemoveIs` (bool)
Utilizes regex to remove `is` attribute. React has an undocumented `is` prop that when added to an element,
prevents React from transforming/filtering props that you add to the element. Any prop that you
add will be added as an attribute with the same name. For example, adding a `class` 
prop to a `<div>` will add a `class` attribute to the `<div>`. This also means that adding a 
`className` prop will not do what you normally would expect it to do. You'll want to add the `is`
prop when using `amp-bind-class` together with `class`, for example. However, the problem arises
where the `is` attribute is also added to the element which causes an AMP validation error. Enabling
the `enableRemoveIs` option will remove the `is` attribute and thus eliminate the validation error.

Here's an example which utilizes `enableAmpBind` and `enableRemoveIs`:

```
<div
  is
  amp-bind-class={`showThing ? '${thingClass} ${thingOpenClass}' : '${thingClass}'`}
  class={ampSearchBarClass}
>
  ...
</div>
```

... elsewhere you might have a button like this:

```
<div
  is
  role="button"
  on="tap:AMP.setState({ showThing: !showThing })"
  tabindex="0"
>
  Toggle Thing Component
</div>
 ```

### `scripts` (array)
Each element of the array is an object with any of the following values:
- `scripts.customElement` (string)
- `scripts.customTemplate` (string)
- `scripts.src` (string): 

### `ampExperimentToken` (string)
Adds `amp-experiment-token` meta tag with token.

### `title` (string)
Page title

### `canonicalUrl` (string)
Adds meta tag with canonical URL.

### `jsonSchema` (object)
Adds json schema meta tag.

### `ampState` (object)
Adds [amp-state tag](https://www.ampproject.org/docs/reference/components/amp-bind#initializing-state-with-amp-state).

### `ampAnalytics` (object)
Adds [amp-analytics tag](https://developers.google.com/analytics/devguides/collection/amp-analytics/)

### `ampGoogleAnalytics` (object)
Adds [amp-analytics (google type) tag](https://developers.google.com/analytics/devguides/collection/amp-analytics/)


## Publishing a new release

- Add entry to CHANGELOG.md
- Make a commit, directly onto master, that does nothing but adds/updates the changelog and bumps package.json, with a commit message of “vX.Y.Z"
- `git tag vX.Y.Z`
- `git push --tags`
- wait for travis to pass
- `npm install && npm test && npm publish`
