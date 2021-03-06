Čím jsou naše programy větší a užitečnější v reálném životě, tím větší je objem a složitost informací, se kterými potřebují pracovat. Informacím, se kterými program pracuje říkáme data. Jednoduchá data v naších programech reprezentujeme pomocí hodnot jako čísla, řetězce, pravdivostní hodnoty apod. Brzy ale narazíme na komplexnější data, která mají nějakou složitější vnitřní strukturu. K reprezentaci takových dat používáme různé <term cs="datové struktury" en="data structures">. V tomto kurzu jsme zatím viděli pouze jednu takovou strukturu a tou je pole. Dnes si ukážeme další strukturu zvanou <term cs="objekt" en="object">, která na začátku vypadá zcela nevinně, nakonec však navždy změní naše životy a programování už nikdy nebude to, co bývalo dříve.

## Objekty

Pokud chceme reprezentovat složitější data, i obyčejná pole nám nabízejí dostatak prostoru. Vzpomeňte si například na naši tabulku útrat.

```js
const expenses = [
  ['Petr', 'Prací prášek', 240],
  ['Ondra', 'Savo', 80],
  ['Pavla', 'Toaleťák', 65],
  ['Zuzka', 'Mýdlo', 50],
  ['Pavla', 'Závěs do koupelny', 350],
  ['Libor', 'Pivka na kolaudačku', 124],
  ['Petr', 'Pytle na odpadky', 75],
  ['Míša', 'Utěrky na nádobí', 130],
  ['Ondra', 'Toaleťák', 120],
  ['Míša', 'Pečící papír', 30],
  ['Zuzka', 'Savo', 80],
  ['Petr', 'Tapeta na záchod', 315],
  ['Ondra', 'Toaleťák', 64]
];
```

Představme si však, že strukturovanost těchto dat ještě naroste. Každý člověk v tabulce bude mít nejen jméno, ale i příjmení, u zakoupené věci budeme zaznamenávat také množství a jednotky. Pole můžeme libovolně vnořovat, není tedy problém naší strukturu předělat takto.

```js
const expenses = [
  [['Petr', 'Bílek'], ['Prací prášek', 1.5, 'kg'], 240],
  [['Ondřej', 'Zvěřina'], ['Savo', 1, 'ks'], 80],
  [['Pavla', 'Durdíková'], ['Toaleťák', 1, 'balení'], 65],
  [['Zuzana', 'Kaczynská'], ['Mýdlo', 200, 'ml'], 50],
  [['Pavla', 'Durdíková'], ['Závěs do koupelny', 1, 'ks'], 350],
  [['Libor', 'Krejčí'], ['Pivka na kolaudačku', 40, 'ks'], 124],
  [['Petr', 'Bílek'], ['Pytle na odpadky', 3, 'balení'], 75],
  [['Michaela', 'Reischlová'], ['Utěrky na nádobí', 1, 'balení'], 130],
  [['Ondřej', 'Zvěřina'], ['Toaleťák', 2, 'balení'], 120],
  [['Michaela', 'Reischlová'], ['Pečící papír', 1, 'balení'], 30],
  [['Zuzana', 'Kaczynská'], ['Savo', 1, 'ks'], 80],
  [['Petr', 'Bílek'], ['Tapeta na záchod', 1, 'ks'], 315],
  [['Ondřej', 'Zvěřina'], ['Toaleťák', 1, 'balení'], 64]
];
```

Taková struktura už ale pomalu začíná být malinko nepřehledná. Pokud se chceme dostat k názvu zakoupené věci, musíme napsat například.

```jscon
> expenses[3][1][0]
'Mýdlo'
```

Z tohoto výrazu už je dost těžké vyčíst, co vlastně z tabulky vytahujeme, musíme počítat indexy na prstech ruky a je velmi snadné se splést. Musíme si totiž pamatovat, že jméno produktu je na indexu 0, že produkt samotný je na indexu 1 a tak dále.

Abychom měli život o kus jednodušší, použijeme k reprezentaci řádku v tabulce místo pole objekt. Objekty ukládají data ve formátu klíč - hodnota. Jeden řádek naší tabulky (jednoduchá verze) pak bude vypadat takto.

```js
const row = {
  name: 'Petr',
  product: 'Prací prášek',
  price: 240
};
```

Výhoda objektů spočívá v tom, že ke každé hodnotě se nyní dostaneme pomocí klíče a nemusíme řešit indexy.

```jscon
> row.name
'Petr'
> row.product
'Prací prášek'
> row.price
240
```

Objekt je nový typ hodnoty. Můžeme s ním tedy jako vždy provádět cokoliv, co děláme s ostatními hodnotami - ukládat do proměnné, používat ve výrazech, předávat jako vstupy funkcím apod. Díky tomu mohou objekty být součástí polí. Celá naše tabulka pak bude vypadat takto.

```js
const expenses = [
  {name: 'Petr', product: 'Prací prášek', price: 240},
  {name: 'Ondra', product: 'Savo', price: 80},
  {name: 'Pavla', product: 'Toaleťák', price: 65},
  {name: 'Zuzka', product: 'Mýdlo', price: 50},
  {name: 'Pavla', product: 'Závěs do koupelny', price: 350},
  {name: 'Libor', product: 'Pivka na kolaudačku', price: 124},
  {name: 'Petr', product: 'Pytle na odpadky', price: 75},
  {name: 'Míša', product: 'Utěrky na nádobí', price: 130},
  {name: 'Ondra', product: 'Toaleťák', price: 120},
  {name: 'Míša', product: 'Pečící papír', price: 30},
  {name: 'Zuzka', product: 'Savo', price: 80},
  {name: 'Petr', product: 'Tapeta na záchod', price: 315},
  {name: 'Ondra', product: 'Toaleťák', price: 6}]
];
```

Navíc, každý klíč v objektu může odkazovat na libovolnou hodnotu. V řádku tabulky zatím máme jen řetězce a číslo. Není však problém použít seznam nebo rovnou další objekt. Můžeme tak vytvářen zanořené struktury jako například tuto.

```js
const row = {
  name: {
    first: 'Petr',
    last: 'Bílek'
  },
  product: {
    name: 'Prací prášek',
    amount: 1.5,
    unit: 'kg'
  },
  price: 240
};
```

K názvu produktu se pak snadno dostaneme takto.

```jscon
> row.product.name
'Prací prášek'
```

To je jistě mnohem srozumitelnější, než používat indexu u polí. Díky objektům a polím můžeme vytvářet mnohonásobně složitější a komplikovanější struktury, které mohou reprezentovat téměř jakákoliv strukturovaná data.

**Tajemství**: Pokud může být pod klíčem v objektu livobolná hodnota, zvídavého člověka možná napade co by se tak stalo, kdybychom jako hodnotu v objektu použitli funkci. Funkce jsou přece také právoplatní hodnoty. Toto je ale právě ta věc, která navždy změní naše programátorské životy a na takovou změnu je třeba se duševně připravit. Odložíme ji proto zatím do některé z dalších lekci.

### Tečková notace

Pokud objekty zapisujeme způsobem jako výše, názvy klíčů se musí řídit stejnými pravidly jako názvy proměnných. Nemůžeme tak mít uvnitř názvu klíče mezeru nebo třeba pomlčku. Většinou se nám podaří takovým znakům vyhnout. Ne vždy je to ale možné. Pokud potřebujeme klíč s názvem, který porušuje pravidla JavaScriptových proměnných, stačí jej uzavřít do uvozovek.

```js
const person = {
  'first-name': 'Petr',
  'last-name': 'Bílek'
};
```

V takovémto případě ale nemůžeme k hodnotám v objektu přistupovat jako obvykle.

```jscon
> person.first-name;
```

JavaScript by si myslel, že od hodnoty `person.first` odečítáme hodnotu `name`. Musíme tedy použít alternativní způsob přistupu ke klíčům.

```jscon
> person['first-name'];
'Petr'
```

Tento způsob zdaleka není tak elegantní jako tečková notace. Proto se snažíme v názvech klíčů vyhýbat znakům, které nepatří do názvů proměnných a můžeme tak používat tečkocou notaci.

## Formát JSON

Podle většiny moderních doporučení je lepší v JavaScriptu používat v řetězcích jednoduché uvozovky. V počátcích JavaScriptu však bylo běžné používat spíše dvojité. Pokud v našich objektech schválně uzavřeme všechny klíče a řetězce do dvojitých uvozovek i tam, kde by to jinak nebylo potřeba, dostaneme reprezentaci zapsanou takto.

<!-- prettier-ignore-start -->
```js
const row = {
  "name": {
    "first": "Petr",
    "last": "Bílek"
  },
  "product": {
    "name": "Prací prášek",
    "amount": 1.5,
    "unit": "kg"
  },
  "price": 240
};
```
<!-- prettier-ignore-end -->

Toto je z hlediska JavaSriptu naprosto validní zápis. Vznikne tak zcela stejný objekt, jak ten, který by vznikl bez použití uvozovek kolem klíčů. Tento způsob zápisu má své speciální jméno - JavaScript Object Notation, nebo-li JSON. Za dobu existence JavaScriptu se tento zápis tak rozšířil, že se stal jedním z hlavních formátů pro výměnu dat na internetu.

## Mandatory home reading

To better prepare you for the life of a professional programmer, from this lesson onwards we are going to write the mandatory home readings in English. Even if you do not aspire for a professional career in programming, even if you just want to be a hobbyist, you will not be able to avoid English for long. It slowly sneaks in with variable names but the road goes much further.

- Most of good courses, articles and video tutorials are in English.
- Most helpful books about programming are written in English.
- All the documentation for HTML, CSS, JavaScript and other tools and technologies is in English.
- If you have a problem and decide to take it to Stack Overflow, guess what? Your communication will be in English.

Switching to English may seem daunting at first. But in a while you will find that technical English is rather straightforward. Reading technical manuals and articles is not like reading a novel or, God forbid, reading the newspapers. The vocabulary in this particular area is quite limited and most of the time the hardest part is understanding the technical terms. If at first you feel a bit frightened by English, do not hesitate to chuck this paragraph into [Google Translator](https://translate.google.com/?sl=en&tl=cs). It is quite capable these days. In fact, with these simple texts, most of the time the translation is almost 100% acurate.

Once you get the hang of it, reading IT English will be a breeze. Actually, reading IT articles and books is a good way to get better in English in general. When at last you are smoothly reading through all the discussions on Stack Overflow, you might even get excited to read some real English literature.
