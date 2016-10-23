#Структуры данных

##TOC

##1. Односвязный список

Он же Singly linked list.

###1.1 Структура односвязного списка

```
     ключ    указатель на следующий элемент
      |       |
      v       v
    ┌──────┬──────┐
    | key  | next |
    └──────┴──────┘

    head                     tail
      |                       |
      v                       v
    ┌───┬───┐   ┌───┬───┐   ┌───┬───┐
    | 1 |  ─┼─> | 2 |  ─┼─> | 3 | X │
    └───┴───┘   └───┴───┘   └───┴───┘
```

###1.2 Операции над односвязным списком

####1.2.1 Добавление в начало (pushFront)

```js
/**
 * Добавление элемента в начало O(1)
 * @param {*} key
 */
pushFront(key) {
    // Создание нового элемента O(1)
    const newNode = new ListNode(key);

    // Если список пуст -- присваивание head и tail ссылки на новый элемент O(1)
    if (this.head === null && this.tail === null) {
        this.head = newNode;
        this.tail = newNode;
        return;
    }

    // Выставление новому элементу указатель на текущий head O(1)
    newNode.setNext(this.head);

    // Присваивание head ссылки на новый элемент O(1)
    this.head = newNode;
}
```

####1.2.2 Возврат первого элемента (topFront)

```js
/**
 * Получение первого элемента O(1)
 * @returns {*}
 */
topFront() {
    // Если список пуст -- ошибка O(1)
    if (this.head === null) {
        throw new Error('List is empty');
    }

    // Возврат значения первого элмемента O(1)
    return this.head.key;
}
```

####1.2.3 Удаление первого элемента (popFront)

```js
/**
 * Удаление первого элемента O(1)
 */
popFront() {
    // Если список пуст -- ошибка O(1)
    if (this.head ===  null) {
        throw new Error('List is empty');

    // Если список состоит из одного элемента -- сброс head и tail O(1)
    } else if (this.head === this.tail) {
        this.head = null;
        this.tail = null;
    }

    // Присваивание в head ссылки на второй элемент O(1)
    this.head = this.head.next;
}
```

####1.2.4 Добавление в конец (pushBack(key)/append)

```js
/**
 * Добавление элемента в конец O(1)
 */
pushBack(key) {
    // Создание нового элемента O(1)
    const newNode = new ListNode(key);

    // Если список пуст -- присваивание head и tail ссылки на новый элемент O(1)
    if (this.tail === null && this.head === null) {
        this.tail = newNode;
        this.head = newNode;
        return;
    }

    // Добавление сслыки в текущий tail на новый элемент O(1)
    this.tail.setNext(newNode);

    // Присываивание в tail ссылки на новый элемент O(1)
    this.tail = newNode;
}
```

####1.2.5 Возврат последнего элемента (topBack)

```js
/**
 * Получение последнего элемента O(1)
 */
topBack() {
    // Если список пуст -- ошибка O(1)
    if (this.tail === null) {
        throw new Error('List is empty');
    }

    // Возврат значения последнего элмемента O(1)
    return this.tail.key;
}
```

####1.2.6 Удаление последнего элемента (popBack)

```js
/**
 * Удаление последнего элемента O(n)
 */
popBack() {
    // Если список пуст -- ошибка O(1)
    if (this.head === null) {
        throw new Error('List is empty');

    // Если список состоит из одного элемента -- сброс head и tail O(1)
    } else if (this.head === this.tail) {
        this.head = null;
        this.tail = null;
    }

    // Итерирование по списке до предпоследнего элемента O(n)
    let node = this.head;
    while (node.next !== this.tail) {
        node = node.next;
    }

    // Сброс указателя на последний элемент в предпоследнем O(1)
    node.setNext(null);

    // Присываивание в tail ссылки на предпоследний элемент O(1)
    this.tail = node;
}
```

####1.2.7 Поиск элемента в списке (find)

```js
/**
 * Поиск элемента в списке
 * В худшем случае -- O(n)
 * @returns {boolean}
 */
find(key) {
    // Итерирование до тех пор, пока не будет найден нужный элемент 
    // В худшем случае -- O(n)
    let node = this.head;
    while (node !== null) {
        // Если текущий элемент имеет искомое значение -- возврат true O(1)
        if (node.key === key) {
            return true;
        }
        node = node.next;
    }

    // Если элемент не найден -- возврат false O(1)
    return false;
}
```

####1.2.8 Удаление элемента по значению (erase)

```js
/**
 * Удаление элемента из списка по значению
 * В худшем случае -- O(n)
 */
erase(key) {
    let node = this.head;
    // Итерирование до тех пор, пока не будет найден нужный элемент 
    // В худшем случае -- O(n)
    while (node !== null) {
        // Если есть следующий элемент и его значение равно искомому --
        // выставление ссылки на элемент, который следует за следующим, в текущий элемент O(1)
        if (node.next !== null && node.next.key === key) {
            node.setNext(node.next.next);
            return;
        }
        node = node.next;
    }
}
```

####1.2.9 Проверка на пустоту (empty)

```js
/**
 * Проверка списка на пустоту O(1)
 * @returns {boolean}
 */
empty() {
    // Проверка на наличие head O(1)
    return this.head === null;
}
```

####1.2.10 Получение длины списка (size)

```js
/**
 * Получение длины списка O(n)
 * @todo Можно за O(1), если хранить длину
 * @returns {number}
 */
size() {
    let size = 0;
    let node = this.head;

    // Итерирование до конца списка. На каждом шагу счетчик
    // увеличивается на единицу O(n)
    while (node !== null) {
        size += 1;
    }

    return size;
}
```

####1.2.11 Добавление по индексу (insert)

```js
/**
 * Добавление элемента по индексу O(n)
 * @param {number} index
 * @param {*} key
 */
insert(index, key) {
    // Если индекс равен нулю -- добавление в начало O(1)
    if (index === 0) {
        this.pushFront(key);
        return;
    }

    // Создание нового элемента O(1)
    const newNode = new ListNode(key);

    // Итерирование до нужного индекса O(n)
    let node = this.head;
    for (let i = 0; i < index - 1; i++) {
        node = node.next;
    }

    // Присваивание указателя в новом элементе O(1)
    newNode.setNext(node.next);

    // Присваивание указателя на новый элемент в текущем O(1)
    node.setNext(newNode);
}
```

####1.2.12 Возврат элемента по индексу с конца (nthFromEnd)

```js
/**
 * Поиск n-го элемента с конца O(n)
 * @param {number} index
 * @returns {number}
 */
nthFromEnd(index) {
    // Установка значения смещения на ноль O(1)
    let offset = 0;

    // Установка главного указателя итерации на head O(1)
    let node = this.head;

    // Установка указателя искомого элемента в null O(1)
    let resultNode = null;

    // Интерирование до конца списка O(n)
    while (node !== null) {
        // Если текущий смещение от начало равно искомому индексу,
        // то необходимо выставить указатель искомого элемента на head O(1)
        if (offset === index) {
            resultNode = this.head;

        // Если указатель на искомый элемент выставлен -- смещение его на следующий элемент O(1)
        } else if (resultNode !== null) {
            resultNode = resultNode.next;
        }

        // Смещение главного указателя итерации и увеличение смещения на единицу O(1)
        node = node.next;
        offset++;
    }

    // В конце итерации в указателе искомого элемента будет n-ый элемент с конца
    // Возврат значения искомого элемента O(1)
    return resultNode.key;
}
```

####1.2.13 Разворот списка (reverse)

```js
/**
 * Разворот списка O(n)
 */
reverse() {
    // Если длина массива равна единице -- возврат никакие действия не нужны O(1)
    if (this.head === this.tail) {
        return;
    }

    // Присваивание в tail ссылки на head O(1)
    this.tail = this.head;

    // Итерирование со второго элемента O(1)
    let node = this.head.next;

    // Сохранение ссылки на элемент, предшествующий главному указателю итерации O(1)
    let prev = this.head;

    // Итерирование до конца списка O(n)
    while (node !== null) {
        // Если ссылка на следующий элемент пуста -- присваивание текущего элемента в head O(1)
        if (node.next === null) {
            this.head = node;
        }

        // Сохранение ссылки на текущий элемент O(1)
        var current = node;

        // Перенос главного указателя итерации на следующий элемент O(1)
        node = node.next;

        // Разворот ссылки для текущего элемента. Выставление в качестве next ссылки на предыдущий элемент O(1)
        current.setNext(prev);

        // Сохранение ссылки на текущий элемент в качестве предыдущего O(1)
        prev = current;
    }

    // В конце -- обнуление ссылки на следующий элемент в tail O(1)
    this.tail.setNext(null);
}
```

##2. Стек (Stack)

Абстрактный тип данных, представляющий собой список элементов, организованных по принципу LIFO (last in -- first out).

###2.1 Структура стека

Стек можно реализовать как с помощью массива, так и с помощью связанного списка. Вот структура стека, в основе которого лежит массив.

```
    ArrayBasedStack

             lastElementIndex
              |
              v
    ┌───┬───┬───┬───┬───┐
    | 1 | 2 | 3 |   |   |
    └───┴───┴───┴───┴───┘
```

###2.2 Операции над стеком

####2.2.1 Добавление элемента в стек (push)

```js
/**
 * Добавление элемента в стек O(1)
 * @param {*} key
 */
push(key) {
    // Если стек полон -- ошибка O(1)
    if (this._length === this._lastElementIndex + 1) {
        throw new Error('Stack is full');
    }

    // Добавление элемента в конец массива O(1)
    this._array[this._lastElementIndex + 1] = key;

    // Увеличение индекса последнего элемента O(1)
    this._lastElementIndex++;
}
```

####2.2.2 Возрат элемента из стека (top)

```js
/**
 * Возврат элемента из стека O(1)
 * @returns {*}
 */
top() {
    // Возврат последнего элемента из массива O(1)
    return this._array[this._lastElementIndex];
}
```

####2.2.3 Удаление элемента из стека (pop)

```js
/**
 * Удаление элемента из стека O(1)
 */
pop() {
    // Уменьшение индекса последнего элемента O(1)
    this._lastElementIndex--;
}
```

####2.2.3 Проверка на пустоту (empty)

```js
/**
 * Проверка стека на пустоту O(1)
 * @returns {boolean}
 */
empty() {
    // Проверка на равенство индекса последнего элемента -1 O(1)
    return this._lastElementIndex === -1;
}
```

###2.3 Примеры использования

####2.3.1 Проверка на последовательности скобок на сбалансированность

Сбалансированная последовательность: `(([])())`

Несбаланированная последовательность: `([)`, `)(`

```js
/**
 * Проверка последовательности скобок на сбалансированность O(n)
 * @param {string} sequence
 */
function isBalanced(sequence) {
    const stack = new ArrayBasedStack(100);

    // Итерирование по последовательности O(n)
    for (let i = 0; i < sequence.length; i++) {
        const parenthesis = sequence[i];

        // Если текущая скобка открывающая -- добавление в стек O(1)
        if (parenthesis === '(' || parenthesis === '[') {
            stack.push(parenthesis);
        } else if (parenthesis === ')' || parenthesis === ']') {

            // Если текущая скобка закрывающая, и стек пуст -- возврат false O(1)
            if (stack.empty()) {
                return false;

            // Если текущая скобок закрывающая, и на вершине стека
            // находится соответствующая ей открывающая скобка -- удаление
            // открывающей скобки из стека O(1)
            } else if (parenthesis ===  ')' && stack.top() === '(') {
                stack.pop();
            } else if (parenthesis ===  ']' && stack.top() === '[') {
                stack.pop();

            // Во всех остальных случаях -- возврат false O(1)
            } else {
                return false;
            }
        }
    }

    // Если после итерирования по последовательности в стеке остались
    // скобки -- последовательность несбалансированна. Иначе -- сбалансированна. O(1)
    return stack.empty();
};
```
