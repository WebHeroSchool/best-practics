# Руководства по стилю (JS)

**1) Избегайте forEach**
    Избегайте forEach при изменении данных. Используйте map, reduce или filter вместо forEach для изменять данные.
   
    // bad
    users.forEach((user, index) => {
      user.id = index;
    });
    
    // good
    const usersWithId = users.map((user, index) => {
      return Object.assign({}, user, { id: index });
    });
    
-следуя этому правилу вы минимизируете мутации в функиях
    
    
**2) Ограничить количество параметров**
    Если ваша функция или метод имеет более 3 параметров, тогда используйте объект в качестве параметра.
    
    // bad
    function a(p1, p2, p3) {
      // ...
    };
    
    // good
    function a(p) {
      // ...
    };
    
 -следуя этому правилу код будет чище и с функцией будет проще работать
    
**3) Избегайте побочных эффектов в конструкторах**
Избегайте асинхронных вызовов, запросов API или манипуляций с DOM в constructor. Вместо этого переместите их в отдельные функции. 
   
    // bad
    class myClass {
      constructor(config) {
        this.config = config;
        axios.get(this.config.endpoint)
      }
    }
    
    // good
    class myClass {
      constructor(config) {
        this.config = config;
      }
    
      makeRequest() {
        axios.get(this.config.endpoint)
      }
    }
    const instance = new myClass();
    instance.makeRequest();    
- Это облегчит написание тестов и поддержку кода.

**4) Избегайте классов для обработки событий DOM**
Если единственная цель класса - связать событие DOM и обработать обратный вызов, используйте функцию.

    // bad
    class myClass {
      constructor(config) {
        this.config = config;
      }
    
      init() {
        document.addEventListener('click', () => {});
      }
    }
    
    // good
    
    const myFunction = () => {
      document.addEventListener('click', () => {
        // handle callback here
      });
    }
- не нужно усложнать то что можно сделать легче
    
**5) Передать контейнер элемента в конструктор**
Когда ваш класс манипулирует DOM, получите контейнер элемента в качестве параметра. 

    // bad
    class a {
      constructor() {
        document.querySelector('.b');
      }
    }
    
    // good
    class a {
      constructor(options) {
        options.container.querySelector('.b');
      }
    }
- Это легче поддерживать и производительность кода будет выше.
    
**6) Используйте ParseInt**
Используется ParseInt при преобразовании числовой строки в число.

    // bad
    Number('10')
    
    // good
    parseInt('10', 10);    
    
**7) CSS селекторы - использовать js-префикс**
    Если CSS-класс используется только в JavaScript как ссылка на элемент, добавьте префикс имени класса js-.
    
    // bad
    <button class="add-user"></button>
    
    // good
    <button class="js-add-user"></button>
    
- разметка станет понятнее и будет проще стилизовать и обращаться к элементам 
    
**8) Абсолютные и относительные пути для модулей**

Используйте относительные пути, если модуль, который вы импортируете, находится ниже двух уровней.

    // bad
    import GitLabStyleGuide from '~/guides/GitLabStyleGuide';
    
    // good
    import GitLabStyleGuide from '../GitLabStyleGuide';

Если модуль, который вы импортируете, находится на два или более уровней выше, используйте вместо него абсолютный путь:

    // bad
    import GitLabStyleGuide from '../../../guides/GitLabStyleGuide';
    
    // good
    import GitLabStyleGuide from '~/GitLabStyleGuide';

Кроме того, не добавляйте их в глобальное пространство имен.
 
**9) Защита XSS**
  Не используйте innerHTML, append() или html() для установки контента. Это открывает слишком много уязвимостей
  
**10) Функции**
Стремитесь написать много маленьких чистых функций и минимизируйте, где происходят мутации.
   
    // bad
    const values = {foo: 1};
    
    function impureFunction(items) {
      const bar = 1;
    
      items.foo = items.a * bar + 2;
    
      return items.a;
    }
    
    const c = impureFunction(values);
    
    // good
    var values = {foo: 1};
    
    function pureFunction (foo) {
      var bar = 1;
    
      foo = foo * bar + 2;
    
      return foo;
    }
    
    var c = pureFunction(values.foo); 
 
-меньше мутаций легче поддержка    
    
**11) Длинный список переменных?** 

Опустите „let“ и используйте запятые
     
    Bad
    let someItem = 'some string';  
    let anotherItem = 'another string';  
    let oneMoreItem = 'one more string';  
   
    Good
    let someItem = 'some string',  
        anotherItem = 'another string',  
        oneMoreItem = 'one more string';  

Этот код должен говорить сам за себя. Я сомневаюсь что тут есть какие-то подвижки в скорости, но код выглядит чище.

**12) Используйте === Сравнение**

Оператор == сравнения всегда преобразует (в соответствующие типы) перед сравнением.
пример

    bad
    0 == "";        // true
    1 == "1";       // true
    1 == true;      // true
    good
    0 === "";       // false
    1 === "1";      // false
    1 === true;     // false

- если не слудовать можно получить ошибку там где не ожидаешь
    
 **13) Не используйте новый объект ()**
 
     Используйте {} вместо new Object()
     Используйте "" вместо new String()
     Используйте 0 вместо new Number()
     Используйте false вместо new Boolean()
     Используйте [] вместо new Array()
     Используйте /()/ вместо new RegExp()
     Используйте function (){} вместо new Function()
 - не нужно делать лишние действия     