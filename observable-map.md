# Observable Map

Реализуем класс `ObservableMap`, который делает то же, что и Map, но при этом оповещает всех, кто того пожелает об изменениях.

```js
class ObservableMap extends Map {
   listeners = []
   subscribe(callback) {
     this.listeners.push(callback)
   }
   
   set(key, value) {
    super.set(key, value)
    this.listeners.forEach(listener => listener(key, value))
   }
}
```

В данном случае мы используем готовый класс Map чтобы реализовать всё хранение данных.

В него мы добавляем новую функцию subscribe, которая сохраняет "подписчика" в специальный массив.
Далее мы заменяем функцию set своей реализацией, которая, помимо установки значения (с помощью метода `super.set`)
оповещает всех подписчиков (установленных в функции subscribe) об изменении.

У нашей реализации есть одна проблема: после того как мы подписались на изменение, мы никак не сможем избавиться от подписки.
Давайте исправим это:

```js
subscribe(callback) {
   this.listeners.push(callback)
   return () => this.listeners = this.listeners.filter(c => c != callback);
}
```

Новая версия метода subscribe возвращает функцию, которая убирает нашего подписчика из списка. 
Достаточно вызвать её, и нужный результат будет достигнут.
