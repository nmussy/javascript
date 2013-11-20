# Airbnb JavaScript Guide de Style() {

*Une approche plus ou moins raisonable à Javascript*


## <a name='TOC'>Table des Matières</a>

  1. [Types](#types)
  1. [Objets](#objects)
  1. [Tableaux](#arrays)
  1. [Strings](#strings)
  1. [Fonctions](#functions)
  1. [Propriétés](#properties)
  1. [Variables](#variables)
  1. [Hissage](#hoisting)
  1. [Expressions conditionnelles & Égalité](#conditionals)
  1. [Blocs](#blocks)
  1. [Commentaires](#comments)
  1. [Espaces](#whitespace)
  1. [Virgules](#commas)
  1. [Point-virgules](#semicolons)
  1. [Conversion des types & Contraintes](#type-coercion)
  1. [Conventions de nommage](#nantions)
  1. [Accesseurs](#accessors)
  1. [Constructeurs](#constructors)
  1. [Évènements](#events)
  1. [Modules](#modules)
  1. [jQuery](#jquery)
  1. [Compatibilité ES5](#es5)
  1. [Test](#testing)
  1. [Performances](#performance)
  1. [Sources](#resources)
  1. [Dans la Nature](#in-the-wild)
  1. [Traductions](#translation)
  1. [Le Guide au Guide de Style Javascript](#guide-guide)
  1. [Contributeurs](#contributors)
  1. [License](#license)

## <a name='types'>Types</a>

  - **Primitifs**: Quand vous accédez à un type primitif, travaillez directement avec sa valeur

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1,
        bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **Complexe**: Quand vous accédez à un type complexe, travaillez avec une référence de sa valeur

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2],
        bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

    **[[⬆]](#TOC)**

## <a name='objects'>Objets</a>

  - Utilisez la syntaxe littérale pour la création d'un objet

    ```javascript
    // pas bien
    var objet = new Object();

    // bien
    var objet = {};
    ```

  - N'utilisez pas les [mots réservés](http://es5.github.io/#x7.6.1) comme clés. Cela ne marchera pas sur IE8. [Plus d'informations](https://github.com/airbnb/javascript/issues/61)

    ```javascript
    // pas bien
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // bien
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - Utilisez des synonymes lisibles à la place des mots réservés.

    ```javascript
    // pas bien
    var superman = {
      class: 'alien'
    };

    // pas bien
    var superman = {
      klass: 'alien'
    };

    // bien
    var superman = {
      type: 'alien'
    };
    ```
    **[[⬆]](#TOC)**

## <a name='arrays'>Tableaux</a>

  - Utilisez la syntaxe littérale pour la création d'un objet

    ```javascript
    // pas bien
    var objets = new Array();

    // bien
    var objets = [];
    ```

  - Si vous ne connaissez pas la taille du tableau, utilisez Array#push.

    ```javascript
    var unTableau = [];


    // pas bien
    unTableau[unTableau.length] = 'abracadabra';

    // bien
    unTableau.push('abracadabra');
    ```

  - Quand vous devez copier un tableau, utilisez Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = objets.length,
        objetsCopie = [],
        i;

    // pas bien
    for (i = 0; i < len; i++) {
      objetsCopie[i] = objets[i];
    }

    // bien
    objetsCopie = objets.slice();
    ```

  - Pour convertir un objet semblable à un tableau en un tableau, utilisez Array#slice.

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

    **[[⬆]](#TOC)**


## <a name='strings'>Strings</a>

  - Utilisez les apostrophes (single quotes) `''` pour les strings

    ```javascript
    // pas bien
    var nom = "Bob Parr";

    // bien
    var nom = 'Bob Parr';

    // pas bien
    var nomComplet = "Bob " + this.nomDeFamille;

    // bien
    var nomComplet = 'Bob ' + this.nomDeFamille;
    ```

  - Les strings faisant plus de 80 caractères devraient être écrites sur plusieurs lignes, en utilisant la concaténation des chaînes de caractères.
  - Note : Si trop utilisée, la concaténation de strings trop longues peut influencer les performances. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40)

    ```javascript
    // pas bien
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // pas bien
    var errorMessage = 'This is a super long error that \
    was thrown because of Batman. \
    When you stop to think about \
    how Batman had anything to do \
    with this, you would get nowhere \
    fast.';


    // bien
    var errorMessage = 'This is a super long error that ' +
      'was thrown because of Batman.' +
      'When you stop to think about ' +
      'how Batman had anything to do ' +
      'with this, you would get nowhere ' +
      'fast.';
    ```

  - Quand vous contruisez programmatiquement une string, utilisez Array#join à la place de l'opérateur de concaténation. Principalement pour IE : [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var objets,
        messages,
        longueur,
        i;

    messages = [{
        state: 'success',
        message: 'This one worked.'
    },{
        state: 'success',
        message: 'This one worked as well.'
    },{
        state: 'error',
        message: 'This one did not work.'
    }];

    longueur = messages.length;

    // pas bien
    function inbox(messages) {
      objets = '<ul>';

      for (i = 0; i < longueur; i++) {
        objets += '<li>' + messages[i].message + '</li>';
      }

      return objets + '</ul>';
    }

    // bien
    function inbox(messages) {
      objets = [];

      for (i = 0; i < longueur; i++) {
        objets[i] = messages[i].message;
      }

      return '<ul><li>' + objets.join('</li><li>') + '</li></ul>';
    }
    ```

    **[[⬆]](#TOC)**


## <a name='functions'>Fonctions</a>

  - Expressions de fonction :

    ```javascript
    // expression de fonction anonyme
    var anonymous = function() {
      return true;
    };

    // expression de fonction nommée
    var named = function named() {
      return true;
    };

    // expression de fonction immédiatement appelée (IIFE)
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - Ne déclarez jamais une fonction dans un bloc non-foncition (if, while, etc). Assignez plutôt la fonction à une variable. Les navigateurs vous permettront de le faire, mais ils l'interprèteront tous différemment, et là c'est la caca, c'est la cata, c'est la catastrophe.
  - **Note :** ECMA-262 définit un `bloc` comme une série d'instructions. La déclaration d'une fonction n'est pas une instruction. [Lisez la note d'ECMA-262 sur ce problème](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // pas bien
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // bien
    var test;
    if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - Ne nommez jamais un paramètre `arguments`, cela prendra précédent sur l'objet `arguments` qui est donné dans la portée de toutes les fonctions.

    ```javascript
    // pas bien
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // bien
    function yup(name, options, params) {
      // ...stuff...
    }
    ```

    **[[⬆]](#TOC)**



## <a name='properties'>Propriétés</a>

  - Utilisez la notation point lorsque vous accédez aux propriétés.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // pas bien
    var isJedi = luke['jedi'];

    // bien
    var isJedi = luke.jedi;
    ```

  - Utilisez la notation `[]` lorsque vous accédez à des propriétés à l'aide d'une variable.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

    **[[⬆]](#TOC)**


## <a name='variables'>Variables</a>

  - Utilisez toujours `var` pour déclarer des variables. Ne par le faire entaîne dans la création de variables globales. Nous préfèrons éviter de poluer l'espace de noms global. Capitaine Planète nous a prévenu de ces dangers.

    ```javascript
    // pas bien
    superPower = new SuperPower();

    // bien
    var superPower = new SuperPower();
    ```

  - N'utiliez qu'une seule déclaration `var` pour de multiples variables et déclarez chacune d'entre elles sur une nouvelle ligne.

    ```javascript
    // pas bien
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';

    // bien
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';
    ```

  - Déclarez les varibles indéfinies en dernier. Cela s'avère pratique lorsque plus tard vous aurez besoin d'assigner une variable en fonction d'une autre précédemment assignée.

    ```javascript
    // pas bien
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // pas bien
    var i, items = getItems(),
        dragonball,
        goSportsTeam = true,
        len;

    // bien
    var items = getItems(),
        goSportsTeam = true,
        dragonball,
        length,
        i;
    ```

  - Assignez vos variables au début de leur portée. Cela vous aide à éviter des problèmes avec la déclaration des variables et ceux liés au hissage de leur affectation.

    ```javascript
    // pas bien
    function() {
      test();
      console.log('doing stuff..');

      //..d'autre trucs..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // bien
    function() {
      var name = getName();

      test();
      console.log('doing stuff..');

      //..d'autre trucs..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // pas bien
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // bien
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```

    **[[⬆]](#TOC)**


## <a name='hoisting'>Hissage</a>

  - Les déclarations de variables se font hisser jusqu'au début de leur portée, mais pas leur affectation.

    ```javascript
    // nous savons que cela ne marchera pas (en supposant
    // qu'il n'existe pas de variable globale notDefined)
    function example() {
      console.log(notDefined); // => déclanche une ReferenceError
    }

    // Déclarer une variable après que vous
    // y référenciez marchera grâce au
    // hissage de variable. Note : l'affectation
    // de la valeur `true` n'est pas hissée.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // L'interpréteur hisse la déclaration
    // de variable au début de la portée.
    // Ce qui veut dire que notre exemple pourrait être écrit :
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Les expressions de fonctions anonymes hissent leur nom de variable, mais pas leur assignement.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - Les expressions de fonctions nommées hissent leur nom de variable, pas le nom de la fonction ou son corps.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // il en est de même lorsque le nom de la fonction
    // est le même que celui de la variable.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - Les déclarations de fonctions hissent leur nom et leur corps.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - Pour plus d'informations, référrez-vous à [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) par [Ben Cherry](http://www.adequatelygood.com/)

    **[[⬆]](#TOC)**



## <a name='conditionals'>Expressions conditionnelles & Égalité</a>

  - Préférez `===` et `!==` à `==` et `!=`.
  - Les expressions condtionnelles sont évaluées en utilisant les contrainte imposées par la méthode `ToBoolean` et suivent toujours ces simples rêgles :

    + Les **Objets** valent **true**
    + **Undefined** vaut **false**
    + **Null** vaut **false**
    + Les **Booleéns** valent **la valeur du booléen**
    + Les **Nombres** valent **false** si **+0, -0, ou NaN**, sinon **true**
    + Les **Strings** valent **false** si la string est vide `''`, sinon **true**

    ```javascript
    if ([0]) {
      // true
      // Un tableau est un objet, les objets valent true
    }
    ```

  - Utilisez des raccourcis.

    ```javascript
    // pas bien
    if (name !== '') {
      // ...stuff...
    }

    // bien
    if (name) {
      // ...stuff...
    }

    // pas bien
    if (collection.length > 0) {
      // ...stuff...
    }

    // bien
    if (collection.length) {
      // ...stuff...
    }
    ```

  - Pour plus d'information, lisez [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) par Angus Croll

    **[[⬆]](#TOC)**


## <a name='blocks'>Blocs</a>

  - Entourrez d'accollades tous vos blocs contenus sur plusieurs lignes.

    ```javascript
    // pas bien
    if (test)
      return false;

    // bien
    if (test) return false;

    // bien
    if (test) {
      return false;
    }

    // pas bien
    function() { return false; }

    // bien
    function() {
      return false;
    }
    ```

    **[[⬆]](#TOC)**


## <a name='comments'>Commentaires</a>

  - Utilisez `/** ... */` pour des commentaires qui s'étendent sur plusieurs lignes. Insérez une description, spécifiez les types et valeurs par défaut pour tous les paramètres et les valeurs de retour.

    ```javascript
    // pas bien
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param <String> tag
    // @return <Element> element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // bien
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param <String> tag
     * @return <Element> element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - Utilisez `//` pour les commentaires d'une seule ligne. Placez-les sur une nouvelle ligne au-dessus du sujet du commentaire. Ajoutez une ligne vide au-dessus du commentaire.

    ```javascript
    // pas bien
    var active = true;  // is current tab

    // bien
    // is current tab
    var active = true;

    // pas bien
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // bien
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  - Préfixer vos commentaire avec `FIXME` ou `TODO` (et pas `ACORRIGER` ou `AFAIRE`, par pitié...) aide d'autres développeurs à comprendre rapidement si vous indiquez un problème qui doit être retravaillé, ou permet de suggérer une solution au problème qui devra être implémentée. Ceux-ci sont différents des commentaires classiques car ils peuvent entraîner une action. Ces actions sont `FIXME -- need to figure this out` ou `TODO -- need to implement`.

  - Utilisez `// FIXME:` pour annoter des problèmes

    ```javascript
    function Calculator() {

      // FIXME: shouldn't use a global here
      total = 0;

      return this;
    }
    ```

  - Utilisez `// TODO:` pour annoter des solutions aux problèmes

    ```javascript
    function Calculator() {

      // TODO: total should be configurable by an options param
      this.total = 0;

      return this;
    }
  ```

    **[[⬆]](#TOC)**


## <a name='whitespace'>Espaces</a>

  - Utilisez deux espaces pour des tabulations "douces"

    ```javascript
    // pas bien
    function() {
    ∙∙∙∙var name;
    }

    // pas bien
    function() {
    ∙var name;
    }

    // bien
    function() {
    ∙∙var name;
    }
    ```
  - Placez un espace avant une accolade ouvrante.

    ```javascript
    // pas bien
    function test(){
      console.log('test');
    }

    // bien
    function test() {
      console.log('test');
    }

    // pas bien
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // bien
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```
  - Ajoutez une nouvelle ligne vide à la fin du fichier.

    ```javascript
    // pas bien
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bien
    (function(global) {
      // ...stuff...
    })(this);

    ```

  - Indentez vos longues chaînes de méthodes.

    ```javascript
    // pas bien
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bien
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // pas bien
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // bien
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .class('led', true)
        .attr('width',  (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

    **[[⬆]](#TOC)**

## <a name='commas'>Virgules</a>

  - Virgules en début de ligne : **Nope.**

    ```javascript
    // pas bien
    var once
      , upon
      , aTime;

    // bien
    var once,
        upon,
        aTime;

    // pas bien
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // bien
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - Virgule finale supplémentaire : **Nope.** Cela peut poser des problèmes avec IE6/7 et IE9 en mode Quirks. De plus, certaines implémentations de ES3 aujoutaient sa longueur à un tableu s'il avait une virgule finale supplémentaire. Cela fut clarifié dans ES5 ([source](http://es5.github.io/#D)):

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

    ```javascript
    // pas bien
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // bien
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

    **[[⬆]](#TOC)**


## <a name='semicolons'>Point-virgules</a>

  - **Yup.**

    ```javascript
    // pas bien
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // bien
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // bien
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    **[[⬆]](#TOC)**


## <a name='type-coercion'>Conversion de types & Contraintes</a>

  - Faîtes vos contraintes de type au début de l'instruction.
  - Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // pas bien
    var totalScore = this.reviewScore + '';

    // bien
    var totalScore = '' + this.reviewScore;

    // pas bien
    var totalScore = '' + this.reviewScore + ' total score';

    // bien
    var totalScore = this.reviewScore + ' total score';
    ```

  - Utilisez `parseInt` pour les Nombres et toujours avec la base numérique utilisée lors de la conversion.

    ```javascript
    var inputValue = '4';

    // pas bien
    var val = new Number(inputValue);

    // pas bien
    var val = +inputValue;

    // pas bien
    var val = inputValue >> 0;

    // pas bien
    var val = parseInt(inputValue);

    // bien
    var val = Number(inputValue);

    // bien
    var val = parseInt(inputValue, 10);
    ```

  - Si pour quelque raison que ce soit vous faîtes quelque chose de fou-fou, que `parseInt` vous ralentit et que vous devez utiliser le décallage de bits pour des [raisons de performances](http://jsperf.com/coercion-vs-casting/3), ajoutez un commentaire expliquant ce et pourquoi que vous le faîtes.
  - **Note :**  Soyez prudent lorsque vous utilisez les opérations de décallage de bits. Les Nombres sont représentés comme des [valeurs sur 64 bits](http://es5.github.io/#x4.3.19), mais les opérations de décallage de bits renvoient toujours des entiers sur 32 bits ([source](http://es5.github.io/#x11.7)). Les décallages de bits peuvent entraîner des comportements innatendus pour des valeurs entières stockées sur plus de 32 bits. [Discussion](https://github.com/airbnb/javascript/issues/109)

    ```javascript
    // bien
    /**
     * parseInt était la rasion pour laquelle mon code était lent.
     * Faire un décallage de bits sur la string pour la contraindre
     * à un Number l'a rendu beaucoup plus rapide.
     */
    var val = inputValue >> 0;
    ```

  - Booléens:

    ```javascript
    var age = 0;

    // pas bien
    var hasAge = new Boolean(age);

    // bien
    var hasAge = Boolean(age);

    // bien
    var hasAge = !!age;
    ```

    **[[⬆]](#TOC)**


## <a name='nantions'>Conventions de nommage</a>

  - Évitez les noms ne faisant qu'une seule lettre. Soyez descriptifs dans votre déclaration.

    ```javascript
    // pas bien
    function q() {
      // ...stuff...
    }

    // bien
    function query() {
      // ..stuff..
    }
    ```

  - Utilisez la camelCase lorsque vous nomez vos objets, fonctions et instances

    ```javascript
    // pas bien
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {};
    var u = new user({
      name: 'Bob Parr'
    });

    // bien
    var thisIsMyObject = {};
    function thisIsMyFunction() {};
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - Utilisez la PascalCase lorsque vous nommez vos constructeurs ou vos classes

    ```javascript
    // pas bien
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // bien
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - Ajoutez un underscore `_` au début du nom de vos propriétés privées

    ```javascript
    // pas bien
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // bien
    this._firstName = 'Panda';
    ```

  - Lorsque vous sauvegardez une référence à `this`, utilisez `_this`.

    ```javascript
    // pas bien
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }

    // pas bien
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // bien
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }
    ```

  - Nommez vos fonctions. Cela s'avère pratique lorsque vous étudiez la pile d'exécution.

    ```javascript
    // pas bien
    var log = function(msg) {
      console.log(msg);
    };

    // bien
    var log = function log(msg) {
      console.log(msg);
    };
    ```

    **[[⬆]](#TOC)**


## <a name='accessors'>Accesseurs</a>

  - Les fonctions d'accesseur pour les propriétés ne sont pas obligatoires
  - Si vous faîtes des fonctions d'accès, utilisez getVal() et setVal('salut')

    ```javascript
    // pas bien
    dragon.age();

    // bien
    dragon.getAge();

    // pas bien
    dragon.age(25);

    // bien
    dragon.setAge(25);
    ```

  - Si la propriété est un booléen, utilisez isVal() ou hasVal()

    ```javascript
    // pas bien
    if (!dragon.age()) {
      return false;
    }

    // bien
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - Vous pouvez créez des fonctions get() et set(), mais restez cohérents.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```

    **[[⬆]](#TOC)**


## <a name='constructors'>Constructeurs</a>

  - Assignez des méthodes à l'objet prototype, au lieu de l'écraser avec un nouvel objet. L'écraser rend l'héritage impossible : en réininitialisant le protoype, vous effacez le prototype père !

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // pas bien
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // bien
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - Les méthodes peuvent renvoyer `this` pour permettre le chaînage de méthodes.

    ```javascript
    // pas bien
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20) // => undefined

    // bien
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - Vous pouvez créer une méthode toString() personalisée, mais soyez sûr qu'elle marche correctement et qu'elle ne cause aucun effet secondaire.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

    **[[⬆]](#TOC)**


## <a name='events'>Évènements</a>

  - Lorsque vous attachez des données utiles à vos évènements (qu'il s'agisse d'évènements du DOM ou quelque chose de plus propriétaire comme les évènements de Backbone), transmettez plutôt un object "hash" au lieu de données brutes. Cela permet au contributeurs suivants d'ajouter plus de données à l'évènement sans rechercher et modifier tous les gestionnaires de l'évènement. Par exemple :

    ```js
    // pas bien
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    prefer:

    ```js
    // bien
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[[⬆]](#TOC)**


## <a name='modules'>Modules</a>

  - Le module devrait commencer avec un `!`. Cela vous assure que, si un module malformé oublie d'ajouter un point virgule final, il n'y aura pas d'erreur en production lorsque les scripts seront concaténnés. [Explication](https://github.com/airbnb/javascript/issues/44#issuecomment-13063933)
  - Le fichier devrait être nommé avec la camelCase, se situer dans un dossier avec le même nom, et correspondre au nom du seul élément exporté.
  - Ajoutez une méthode nommée noConflict() qui restaure le module exporté à sa version précédente et le renvoit.
  - Déclarez toujours `'use strict';` au début du module.

    ```javascript
    // fancyInput/fancyInput.js

    !function(global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        this.options = options || {};
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```

    **[[⬆]](#TOC)**


## <a name='jquery'>jQuery</a>

  - Prefixez vos variables d'objets jQuery avec un `$`.

    ```javascript
    // pas bien
    var sidebar = $('.sidebar');

    // bien
    var $sidebar = $('.sidebar');
    ```

  - Mettez en cache vos requêtes jQuery.

    ```javascript
    // pas bien
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // bien
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - Pour les requêtes du DOM, utilisez le sélecteur en cascades `$('.sidebar ul')` ou le parent > enfant `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - Utilisez `find` avec des objets de requêtes jQuery apparetenant à la portée.

    ```javascript
    // pas bien
    $('ul', '.sidebar').hide();

    // pas bien
    $('.sidebar').find('ul').hide();

    // bien
    $('.sidebar ul').hide();

    // bien
    $('.sidebar > ul').hide();

    // bien
    $sidebar.find('ul');
    ```

    **[[⬆]](#TOC)**


## <a name='es5'>Compatibilité ECMAScript 5</a>

  - Réferrez vous à la [table de compatibilité](http://kangax.github.com/es5-compat-table/) ES5 de [Kangax](https://twitter.com/kangax/)

  **[[⬆]](#TOC)**


## <a name='testing'>Test</a>

  - **Yup.**

    ```javascript
    function() {
      return true;
    }
    ```

    **[[⬆]](#TOC)**


## <a name='performance'>Performances</a>

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Chargement en cours...

  **[[⬆]](#TOC)**


## <a name='resources'>Sources</a>


**Lisez ceci**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**Autres Guides de Style**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)

**Autres Styles**

  - [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52)
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript)

**Pour en savoir Plus**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer

**Livres**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/)

**Blogs**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

  **[[⬆]](#TOC)**

## <a name='in-the-wild'>Dans la Nature</a>

  Ceci est une liste de toutes les organisations qui utilisent ce guilde de style. Envoyez-nous une pull request ou ouvrez une issue (sur le repo original) et nous vous ajouterons à la liste.

  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **American Insitutes for Research**: [AIRAST/javascript](https://github.com/AIRAST/javascript)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **GeneralElectric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **Userify**: [userify/javascript](https://github.com/userify/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

## <a name='translation'>Traductions</a>

  Ce guide de style dans sa version originale :

  - :us: **Anglais** : [airbnb/javascript](https://github.com/airbnb/javascript)

  Et dans d'autres langues :

  - :de: **Allemand** : [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - :jp: **Japonais** : [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
  - :br: **Portugais** : [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - :cn: **Chinois** : [adamlu/javascript-style-guide](https://github.com/adamlu/javascript-style-guide)
  - :es: **Espagnol** : [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - :kr: **Coréen**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)

## <a name='guide-guide'>Le Guide au Guide de Style Javascript</a>

  - [Référence](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## <a name='authors'>Contributeurs</a>

  - [Voir les Contributeurs](https://github.com/airbnb/javascript/graphs/contributors)


## <a name='license'>License</a>

(The MIT License)

Copyright (c) 2012 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[[⬆]](#TOC)**

# };
