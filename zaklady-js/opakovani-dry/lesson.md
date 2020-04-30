Díky JavaScritpu a práci s DOMem už dokážete vašim stránkám vdechnout hodně života a zprostředkovat tak uživateli interaktivní zážitek. Zatím jsme však pracovali pouze s elementy, které už na stránce existují. Dnes si ukážeme, jak zapojit do stránky elementy vytvořené celé přímo v JavaScriptu. Tímto postupně nastupujeme cestu, která skončí až u toho, kdy budeme vytvářet v JavaScriptu úplně celou stránku a žádné HTML soubory už vytvářet nebudeme. To však až za několik lekcí.

## Interpolace řetězců

Chceme se vyhnout nepřehledným konstrukcím typu

```js
'Spolubydlící ' + name + ' utratil ' + amount + ' kč za ' + product + '.';
```

Proto napíšeme řaději

```js
`Spolubydlící ${name} utratil ${amount} kč za ${product}.`;
```

Ve složených závorkách máme jakési JavaScriptové okno uvnitř řetězce, do kterého můžeme vepsat nejen proměnnou, ale zcela libovolný výraz, jehož výsledek bude automaticky zkonvertován na řetězec.

## Tvorba HTML pomocí JavaScriptu

Z předchozích lekcí už dávno umíme změnit obsah nějakého DOM elementu pomocí vlastnosti `textContent`. U složitějších aplikací však narazíme na situace, kdy nechceme měnit jen text už existujícího elementu, ale chceme vytvořit úplně nový element. Na začátku nám k tomu poslouží vlastnost `innerHTML`. Pomocí této vlastnosti můžeme nějakému elementu nastavit obsah jako HTML. Můžeme tak uvnitř elementu vyrobit úplně novou HTML strukturu.

Mějme následující jednoduchou HTML strukturu.

```html
<body>
  <div class="card">Produkt</div>
</body>
```

Nyní vybereme element s třídou `card` a vložíme do něj další HTML kód.

```js
const cardElm = document.querySelector('.card');
cardElm.innerHTML =
  '<div class="card__title">Lednička</div>' +
  '<div class="card__body">Cena: 12 000 kč</div>';
```

Můžeme si ověřit, že se nám a stránce skutečně objeví tato nově vytvořené struktura. Takto můžeme dovnitř libovolného elementu vložit libobolně komplikované HTML. Jen nás za chvíli začne dost obtěžovat neustálé sčítání řetězců. V JavaScriptu bohužel nelze udělat běžný řetězec na více řádků. Následující kód by bohužel nefungoval.

```js
const name = '
  petr
';
```

Pokud však použijeme nové cool řetězce se zpatnými apostrofy, takovýto trik si dovolit můžeme.

```js
const name = `
  petr
`;
```

Navíc můžeme do takovýchto řetězců vkádat proměnné. Není tedy problém sestavit naše HTML s použitím nějakých dat.

```js
const product = ['Lednička', 12000];
const cardElm = document.querySelector('.card');
cardElm.innerHTML = `
  <div class="card__title">${product[0]}</div>
  <div class="card__body">${product[1]}</div>
`;
```

Pomocí `innerHTML` získáváme mnohem větší kontrolu nad obsahem stránky než jsme měli pouze s použitím `textContent`.

### Ukázkový příklad - nákupní seznam

Mějme následující pole obsahující položky, které zítra nechceme zapomenout koupit při návštěvě supermarketu.

```js
const shoppingList = [
  'mrkev',
  'paprika',
  'cibule',
  'čínské zelí',
  'arašídy',
  'sojová omáčka',
];
```

Díky použití `innerHTML` můžeme snadno naše pole převést na hezký HTML seznam.

```html
<body>
  <ul id="shopping-list"></ul>
</body>
```

```js
const listElm = document.querySelector('#shopping-ling');
for (let i = 0; i < shoppingList.length; i += 1) {
  listElm.innerHTML += `<li>${shoppingList[i]}</li>`;
}
```

Každá obrátka tohoto cyklu nám tak přidá do HTML seznamu jeden další `li` element.

## Aktualizování stránky

Pohlédněme na celý kód naší stránky s nákupním seznamem.

```js
const shoppingList = [
  'mrkev',
  'paprika',
  'cibule',
  'čínské zelí',
  'arašídy',
  'sojová omáčka',
];

const listElm = document.querySelector('#shopping-ling');
for (let i = 0; i < shoppingList.length; i += 1) {
  listElm.innerHTML += `<li>${shoppingList[i]}</li>`;
}
```

Stránka je zatím poměrně statícká. Zobrazuje pořád tentýž seznam. Určitě bychom chtěli uživateli umožnit přidat do seznamu nějakou položku. Naše pole je globální, můžeme to tedy zatím zkusit udělat programátorsky přímo z konzole.

```js
> shoppingList.push('koriandr');
7
```

Naše pole se tedy rozrostlo o jeden prvek. K našemu zklamání však obsah stránky zůstává pořád stejný. Je to logické, protože obsah seznamu `ul` jsme v JavaScriptu vytvořili hned po načtení stránky. Změna našeho pole tento kód znovu magicky nespustí. Musíme jej spustit sami ve chvíli, kdy chceme říct, že se má obsah seznamu `ul` vytvořit znova podle nového obsahu pole `shoppingList`. Abychom mohli náš kód spouštět opakovaně, bude šikovné si jej zabalit do funkce.

```js
const updateShoppingList = () => {
  const listElm = document.querySelector('#shopping-ling');
  listElm.innerHTML = '';
  for (let i = 0; i < shoppingList.length; i += 1) {
    listElm.innerHTML += `<li>${shoppingList[i]}</li>`;
  }
};
```

Všimněte si, že na začátku funkce vymažeme `innerHTML` našeho `ul` seznamu, abychom celou HTML strukturu vytvořili úplně znova. Máme tak k dispozici funkci, kterou můžeme zavolat pokaždé, když chceme, aby naše stránka zobrazila aktuální obsah našeho pole `shoppingList`. To nám dává svobodu si s polem dělat co chceme, přidávat položky, měnit položky, mazat položky a tak dále. Vždy jen pak musíme zavolat funkci `updateShoppingList`, aby se změny projevily i v našem HTML. Můžete si to vyzkoušet rovnou z konzole a sledovat, jak stránka reaguje.

```js
> shoppingList.push('zázvor');
8
> updateShoppingList()
undefined
> shoppingList.shift();
'mrkev',
> updateShoppingList()
undefined
> shoppingList[0] = 'klíčky';
'klíčky',
> updateShoppingList()
undefined
```

V tomto příkladu jsme poprvé narazili na velice důležitý postup při tvorbě větších aplikací. V následující části si o něm řeknemě něco více, abychom jej dokázali rozpoznat a použít i v dalších našich programech.

## Dvouvrstvá architektura

Čím zkušenější a mocnější se v JavaScriptu stáváme, tím větší a složitější budou naše programy. Tím, jak skládat dohromady větší aplikace a udržet jejich komplikovanost na uzdě, se zabývá celý velký obor, kterému se říká <i>softwarový návrh a architektura</i>. Podobně jako když stavíme most nebo budovu, nemůžeme rovnou začít skládat cihlu k cihle. Musíme si nejdříve dopředu navrhnout strukturu celé stavby, aby se nám nestalo, že začneme stavět střechu aniž bychom si rozmysleli, kde budou stát nosné zdi.

Vývoj aplikací už má za dobu existence počítačů dlouhou historii. Tisíce a tisíce programátorů před námi postupně ze svých zkušeností vydestilovali mnoho různých postupů jak zajistit, aby se nám větší programy nerozpadly pod rukama. Těmto postupům se řiká návrhové a architektonické vzory. My si zde představíme vzor, na který jsme před chvíli narazili v naší aplikaci s nákupním seznamem. Tomuto zvoru se odborně říká <term cs="dvouvrstvá architektura" en="two tier architecture">.

V principu nejde o nic zvlášť komplikovaného. Podstatou celé věci je si uvědomit, že téměř každá aplikace obsahuje dvě zcela základních části: data a prezentaci. Data jsou naše JavaScriptové hodnoty jako čísla, řetězce, pole a podobně. Prezentace je způsob, jakým tato data zobrazujeme uživateli. V našem případě jako prezentaci používáme HTML a CSS.

## Vlastní DOM elementy

Při práci s `innerHTML` narazíme na jeden velký problém. Při každé změně této vlastnosti se celý DOM uvnitř elementu zkonstruuje zcela znova. Tím přijdeme o všechny posluchače událostí. Způsob, jak toto obejít, je vytvořit si DOM element mimo dokument.

`document.createElement`

```

```