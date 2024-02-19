# note of Cherno C++

### Heap vs Stack

Stack allocation is fast because when we want to allocate some memory, the stack pointer will move and return the memory address like a real stack

when the variable in stack out of its range(''{}''), all of the memory in stack just got pop off.







# STL

* **`remove_if`**:
  - 它接受三个参数：开始迭代器、结束迭代器和一个谓词函数。
  - 谓词函数用于决定哪些元素应该被“移除”。在这个例子中，谓词是一个lambda函数`[](auto p) { return !p; }`，它检查元素`p`是否为`nullptr`。
  - `remove_if`并不实际从容器中移除元素，而是将不需要移除的元素移动到容器的开始部分，并返回一个指向“新的”结束位置的迭代器。
