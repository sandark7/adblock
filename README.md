# adblock

[Технические требования подключения контентных площадок к системе AdBlock.Rambler](https://confluence.rambler-co.ru/pages/viewpage.action?pageId=22874571)

## Node.js

To install:

```sh
npm i git+ssh://git@github.com:arezakov/adblock.git --save
```

To use:

```js
'use strict';

import * as adblock from 'adblock';

const HOST_NAME = location.hostname;
const PLATE_TEXT = 'Воспользуйтесь опцией отключения рекламы. Если у&nbsp;вас она уже активирована, то нажмите сюда.';
const PLATE_URL = 'http://noadblock.rambler.ru/verify?content=' + HOST_NAME;
const DEBUG = document.body.getAttribute('data-prod');

export function init() {
  adblock.init(DEBUG)
    .then(start)
    .catch(start);

  function start(isAdblock) {
    if (isAdblock.ad) {
        console.log('Показать рекламу');
    }
    if (isAdblock.plate) {
      showPlate();
    }
  }
}

function showPlate() {
  var plate = document.createElement('div');
  plate.className = 'adblock-plate';
  plate.innerHTML = `<a href="${PLATE_URL}" class="adblock-plate__link">${PLATE_TEXT}</a><span class="adblock-plate__close"></span>`;
  document.body
    .insertBefore(plate, document.body.firstChild)
    .querySelector('.adblock-plate__close')
    .addEventListener('click', function(e) {
      adblock.delaySubscribe();
      this.parentNode.parentNode.removeChild(plate);
      e.preventDefault();
    });
}
```
