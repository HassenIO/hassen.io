---
title: "[Python] Tuple vs. List"
date: 2022-03-23T00:42:00+01:00
draft: true
summary: This is a short article to compare pythonic tuples vs lists.
---

![A Stack of books](/2022/03/hero-stack-of-books.jpg "A stack of books")

Let's not make this long and go straight to the point: How do you choose between storing a set of data as a tuple vs a list?

# Immutability

Suppose we have the following code:

```python
l = [1, 2, 3, 4]
print(l)  # Shows [1, 2, 3, 4]

l[0] = 100
print(l)  # Shows [100, 2, 3, 4]
```

As you can see, you can easily alterate a value in the list by just assigning it a new value. Similarily, you can alterate the original list by appending a new item with `l.append(5)` or deleting items with `.pop()` and `.remove()`.

However, the same experience with tuples is a bit more "dramatic":

```python
t = (1, 2, 3, 4)
print(t)  # Shows (1, 2, 3, 4)

t[0] = 100
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
```

As described, altering a tuple is forbidden. A tuple is an **immutable** object: When created, It can't be altered by any mean.

This is probably the most important difference between a tuple and a list, it should help you decide which data type you should take. But it's not the only one criteria.

# Memory

To check memory size of a tuple, consider the following example in a `tuple.py` file:

```python
import sys


def test_tuple():
    t = ()
    current_size = sys.getsizeof(t)

    print(f"len(t)={len(t)}, size={current_size}, diff=0")

    for i in range(1, 17):
        t = (*t, i)
        diff, current_size = sys.getsizeof(t) - current_size, sys.getsizeof(t)

        print(f"len(t)={len(t)}, size={current_size}, diff={diff}")


if __name__ == "__main__":
    test_tuple()
```

tl;dr: We create an empty tuple, and then add items in new tuples while printing the current size of the tuple, and the difference with the previous size.

Because tuples are immutable, there is no `.append` method to add a new element into it. We can however create a new tuple that contains previous tuple values AND the new item. We call this tuple unpacking, and it's what line 11 is: `t = (*t, i)`

The result is the following:

```text
len(t)=0, size=40, diff=0
len(t)=1, size=48, diff=8
len(t)=2, size=56, diff=8
len(t)=3, size=64, diff=8
len(t)=4, size=72, diff=8
len(t)=5, size=80, diff=8
len(t)=6, size=88, diff=8
len(t)=7, size=96, diff=8
len(t)=8, size=104, diff=8
len(t)=9, size=112, diff=8
len(t)=10, size=120, diff=8
len(t)=11, size=128, diff=8
len(t)=12, size=136, diff=8
len(t)=13, size=144, diff=8
len(t)=14, size=152, diff=8
len(t)=15, size=160, diff=8
len(t)=16, size=168, diff=8
```

Note that an empty tuple is of size 40 bytes, and that every new item in a tuple increases the size of the whole set by 8 bytes.

> Why is the size of an empty tuple 40 bytes? Shouldn't be 0?
>
> The reason is that python internals not only stores the value of your variables, but also information about your variables, like links to their types and a reference counts for managing memory. If you substract 40 bytes to the sizes of your current tuple and divide by 8 bytes (the diff), you'll end up with the count of elements in your tuple. This is not true with lists as we will see shortly.

Another thing to notice is that every integer added to a tuple is the same 8 bytes increase.

If we do the same exercice with lists, we get the following code in a `list.py` file:

```python
import sys


def test_list():
    l = []
    current_size = sys.getsizeof(l)

    print(f"len(l)={len(l)}, size={current_size}, diff=0")

    for i in range(1, 17):
        l.append(i)
        diff, current_size = sys.getsizeof(l) - current_size, sys.getsizeof(l)

        print(f"len(l)={len(l)}, size={current_size}, diff={diff}")


if __name__ == "__main__":
    test_list()
```

We get the following result:

```text
len(l)=0, size=56, diff=0
len(l)=1, size=88, diff=32
len(l)=2, size=88, diff=0
len(l)=3, size=88, diff=0
len(l)=4, size=88, diff=0
len(l)=5, size=120, diff=32
len(l)=6, size=120, diff=0
len(l)=7, size=120, diff=0
len(l)=8, size=120, diff=0
len(l)=9, size=184, diff=64
len(l)=10, size=184, diff=0
len(l)=11, size=184, diff=0
len(l)=12, size=184, diff=0
len(l)=13, size=184, diff=0
len(l)=14, size=184, diff=0
len(l)=15, size=184, diff=0
len(l)=16, size=184, diff=0
```

First, notice that the size of an empty list is greater than the size of an empty tuple. Second, the diff increase is not linear, but looks to be increamental when a size is reached. If we add a new element, we get a new substantial increase:

```text
len(l)=17, size=256, diff=72
```

This increase is algorithmically decided and implemented; When adding the first element, Python pre-allocated 4 memory slots for integers (4*8=32) which makes things fast for the next 3 elements to be appended. However, when the 5-th element was added, it pre-allocated 4 more slots for future elements.

After 9 elements, Python pre-allocated 8 slots, and 9 slots from element 17.

This means that no matter how many elements are in the list, the allocated total size is higher than a tuple equivalent. This is because lists internals require more information than tuples internales (see previous note).

# Speed

To make significant timing, we increase the loop to `2^15 + 1` iterations, aka 32769.

We get the following results:

```text
$ time python3 tuple.py
python3 tuple.py  2.15s user 0.01s system 99% cpu 2.170 total

$ time python3 list.py
python3 list.py   0.03s user 0.01s system 84% cpu 0.042 total
```

Appending lists is significantly faster than appending tuples. This is expected, as we have seen in the previous section, that appending to lists pre-allocates slots for data, rather than creating a whole new variable like tuples do.

Please note that timing results might change depending on your system and your use case. This example was keps simple for demonstration purpose only.

# Conclusion

What do you choose? It depends:

- If you need mutability (like creating your dataset on the run), go for lists.
- If you absolutely need immutability, go for tuples.
- If you don't care mutability, but care about speed in the manipulation of your data, go for lists.
- If you care about memory, go for tuples.

Hope it helps.

### Credits:

- Stack of books photo by [Robert Anasch](https://unsplash.com/@diesektion?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/stack).
