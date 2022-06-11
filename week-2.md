# Конспект для Week2

## Объекты и массивы

### Строковая интерполяция

Подстановка переменных в шаблонную строку, без дополнительной конкатенации.

Шаблонные строки обрамляются косыми кавычками (знак гравис).

```js
const name = 'Roma'
const lastName = 'Pupkin'

const str = `${name} ${lastName}`
```

### Объект (Object)

```js
const person = {
    name: 'Roma',
    lastName: 'Pupkin',
    age: 30,
}

// dot notation
console.log(`${person.name} ${person.lastName} ${person.age}`)

// assignment
person.age = 31

// add property
person.profession = 'Programmer'

// bracket notation
console.log(`${person['name']} ${person['lastName']} ${person['age']}`)

// destructuring
const {name: firstName, lastName, age, profession} = person

console.log(`${firstName} ${lastName} ${age} ${profession}`)
```

### Массив (Array)

```js
const persons = ['Roma', 'Tany', 'Serg']

// request
persons[1] // <-- Tany

// assign
persons[1] = 'Sveta'

// request updated
persons[1] // <-- Sveta

// add to array end
persons.push('Vera')
console.log(persons) // <-- ['Roma', 'Sveta', 'Serg', 'Vera']

// add to array start
persons.unshift('Igor')
console.log(persons) // <-- ['Igor', 'Roma', 'Sveta', 'Serg', 'Vera']

// remove first element
persons.shift()
console.log(persons) // <-- ['Roma', 'Sveta', 'Serg', 'Vera']

// iterate elements of an array
persons.forEach(person => console.log(person))
/* <--
* Roma
* Sveta
* Serg
* Vera
* */

// iterate elements of an array with FOR
for (let i = 0; i < persons.length; i++) {
    console.log(persons[i])
}
/* <--
* Roma
* Sveta
* Serg
* Vera
* */

// create new array based on an existing one
const updatePersons = persons.map(person => person + 1)
console.log(updatePersons) // <-- ['Roma1', 'Sveta2', 'Serg3', 'Vera']

// destructuring
const [programmer, counter, designer, director] = updatePersons;

// split
console.log(programmer.split('m')) // <-- ['Ro', 'a1']
console.log(counter) // <-- Sveta2
console.log(designer) // <-- Serge3
console.log(director) // <-- Vera4

// reverse 1
;[persons[3], persons[2], persons[1], persons[0]] = [persons[0], persons[1], persons[2], persons[3]]
console.log(persons)

// reverse 2
persons.reverse()
console.log(persons)

```

### Spread и Rest

```js
// rest
function sum(...nums) {
    return nums.reduce((sum, num) => sum + num, 0);
}

const numbers1 = [1, 2, 3];
const numbers2 = [4, 5, 6];

// spread
const res1 = sum(...numbers1, ...numbers2);
const res2 = sum(...(numbers1.concat(numbers2)));

console.log(res1);
console.log(res2);
```

### FOR-OF

```js
const numbers = [1, 2, 3]

for (const num of numbers) {
    console.log(num)
}
```

### Symbol

Нужны, чтобы исключить конфликты имён у объектов.

Все символы уникальны и они не видны при переборе объекта.

```js
const mySymbol1 = Symbol();
const mySymbol2 = Symbol();

console.log(mySymbol1 === mySymbol2); // <-- false
```

### Итератор

Объект у которого есть метод `next()`.

```js
const iterator = {
    next() {
        return {
            value:'', // <-- 1 <-- 2 <-- 3 <-- 4
            done: '', // <-- false <-- false <-- false <-- false <-- true
        }
    }
}

const numbers = [1, 2, 3]

iterator.next()
```

## Обработка ошибок и отладка

Процесс пошаговой проверки правильности выполнения кода, с точки зрения логики алгоритма.

Отладка, позволяет остановить работу интерпретатора кода, на любом шаге его выполнения.

Для этого, нужно в программе отладчике, указать одну или несколько **Точек Останова**, которыми отмечаются строки где нужно сделать остановку.

В качестве отладчика может выступать панель **Инструментов разработчика**, в браузере, или сам редактор кода.

## Proxy

[MDN](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

Объект, обёртка для целевого объекта, изменяющий его стандартный метод, путём добавления ловушек (traps), перехватывающих использование методов.

```js
const obj = {} // создаём объект
    
// создаём Proxy
const objProxy = new Proxy(obj, {
    set(obj, key, value) { // перехватываем стандартный метод, присвоения свойств, объекта
        obj[key] = value ** 2; // при присвоении значения, умножаем его на 2
        
        return true; // подтверждаем совершение действия
    }
})

obj.num1 = 1; // добавляем свойство и значение обычным способом
console.log(obj) // {num1: 1}

objProxy.num2 = 2; // добавляем свойство и значение, с использованием перехватчика (Proxy)

// ввводим результат
console.log(obj) // {num1: 1, num2: 4}
console.log(objProxy) // {num1: 1, num2: 4}
```

Как видно, Proxy перехватил присвоенное объекту значение, обработал его и отправил по назначению.
