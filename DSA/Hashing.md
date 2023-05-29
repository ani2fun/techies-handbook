# Hashing

## Hashing Technique

**Why we need Hashing ?**

To achieve faster search than Linear search O(n) and Binary Search O(logn), idea of Hashing Technique was introduced. Using hashing technique the searching time can be achieved with O(1).

![Hashing-1](/resources/Hashing/jpg/Hashing-1.jpeg)

**Ideal Hashing** 

![Hashing-2](/resources/Hashing/jpg/Hashing-2.jpeg)

Drawback of Ideal hashing is Space required is too Large.

Who is responsible for this → Ideal Hash function. 

Hence To improve hashing → Improve Hash Function.

**Modulous Hash Function and Drawbacks**

It’s non Ideal Hash function.

h(x) = x % 10 → [ Many to One ]

![Hashing-3](/resources/Hashing/jpg/Hashing-3.jpeg)

**Solutions**

![Hashing-4](/resources/Hashing/jpg/Hashing-4.jpeg)

**Open Hashing** 

Consume Extra Space

**Chaining Method**

![Hashing-5](/resources/Hashing/jpg/Hashing-5.jpeg)

**Problem with Hashing is choosing improper hash function.**
![Hashing-6](/resources/Hashing/jpg/Hashing-6.jpeg)

**Code for Chaining Method**

```c
#include <cstdio>
#include <cstdlib>

struct Node {
    int data;
    struct Node *next;
};

void SortedInsert(struct Node **H, int x) {
    struct Node *t, *q = NULL, *p = *H;

    t = (struct Node *) malloc(sizeof(struct Node));
    t->data = x;
    t->next = NULL;

    if (*H == NULL)
        *H = t;
    else {

        while (p && (p->data < x)) {
            q = p;
            p = p->next;
        }

        if (p == *H) {
            t->next = *H;
            *H = t;
        } else {
            t->next = q->next;
            q->next = t;
        }
    }
}

struct Node *Search(struct Node *p, int key) {
    while (p != NULL) {
        if (key == p->data)
            return p;
        p = p->next;
    }
    return NULL;
}

int hash(int key) {
    return key % 10;
}

void Insert(struct Node *H[], int key) {
    int index = hash(key);
    SortedInsert(&H[index], key);
}

int main() {
    struct Node *HT[10];

    for (int i = 0; i < 10; i++)
        HT[i] = NULL;

    Insert(HT, 12);
    Insert(HT, 22);
    Insert(HT, 42);

    int key = 42;
    struct Node *temp = Search(HT[hash(key)], key);
    printf("%d ", temp->data);

		return 0;

}
```

**Closed hashing**

Use only Given Space - No Extra Space.

- Linear Probing
- Quadratic Probing
- Double Hashing