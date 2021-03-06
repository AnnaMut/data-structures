##3. Очередь (Queue)

Абстрактный тип данных, представляющий собой список элементов, организованных по принципу FIFO (first in &mdash; first out).

###3.1 Стуктура очереди

Очередь, реализованная с помощью массива выглядит так.

```
    ArrayBasedQueue

     readIndex       writeIndex
      |               |
      v               v
    ┌───┬───┬───┬───┬───┐
    | 1 | 2 | 3 | 4 |   |
    └───┴───┴───┴───┴───┘
```

Возрат элемента из очереди осуществляется по индексу `readIndex`. Индекс `writeIndex` нужен для добавления элементов. И чтение, и запись двигает соотетствующий индекс вправо на единицу. Если индекс выходит за пределы массива &mdash; он сбрасывается в ноль. Если `readIndex === writeIndex`, то очередь пуста. Это условие вносит ограничение на добавление элемента: если после него `writeIndex` будет равен `readIndex`, то такое добавление невозможно.

###3.2 Операции над очередью

####3.2.1 Добавление элемента в очередь (enqueue)

```js
/**
 * Добавление элемента в очередь O(1)
 * @param {*} key
 */
enqueue(key) {
    // Если после добавления элемента индекс записи
    // окажется равен индексу чтения -- ошибка O(1)
    if (this._readIndex === (this._writeIndex + 1) % this._length) {
        throw new Error('Queue is full');
    }

    // Запись в массив по индексу O(1)
    this._array[this._writeIndex] = key;

    // Сдвиг индекса записи вправо с учетом того,
    // что он может выйти за пределы массива O(1)
    this._writeIndex = (this._writeIndex + 1) % this._length;
}
```

####3.2.2 Удаление и возврат элемента из очереди (dequeue)

```js
/**
 * Удаление и возврат элемента из очереди O(1)
 * @returns {*}
 */
dequeue() {
    // Проверка очереди на пустоту O(1)
    if (this._readIndex === this._writeIndex) {
        throw new Error('Queue is empty');
    }

    // Чтение из массива по индексу O(1)
    const result = this._array[this._readIndex];

    // Сдвиг индекса чтения вправо с учетом того,
    // что он может выйти за пределы массива O(1)
    this._readIndex = (this._readIndex + 1) % this._length;

    // Возврат удаленного элемента O(1)
    return result;
}
```

####3.2.3 Проверка на пустоту (empty)

```js
/**
 * Проверка на пустоту O(1)
 * @returns {boolean}
 */
empty() {
    // Проверка на равенство индексов записи и чтения O(1)
    return this._readIndex === this._writeIndex;
}
```
