---
title: C
linktitle: C
toc: true
type: book
date: "2018-09-09T00:00:00Z"
weight: 100
tags:
  - C
---

## malloc vs calloc

Both allocates a block of memory, where the primary difference is if block needs to be initialized and if multiple blocks need to be allocated. If initialization isn't important, use malloc

```
typedef struct node {
    int val;
    struct node * next;
} node_t;

//create a char pointer ptr 
//using malloc for the allocation of memory
node_t *ptr = (node_t*) malloc(sizeof (node_t)); 

//create a pointer to allocate the memory block using calloc
//assign 10 elements that can hold 10 byte size each
node_t *ptr = (node_t*) calloc(10 ,sizeof (node_t));
```

If memset is needed after a malloc, use calloc.
```
memset(ptr, 0, count * sizeof (node_t));
```

Resource :
- [Why does calloc exist](https://vorpus.org/blog/why-does-calloc-exist/)

## Link list

Link list is a dynamically allocated nodes (a dynamic array).

Advantages over array :
- Items can be added or removed from the middle of the list.
- No need to define an initial size.

Disadvantages :
- No random access. With an array, you can store the index to be able to access the information where as with a linked list it needs to be iterated.
- Dynamic memory and pointers adds a level of complexity.
- Adds a layer of complexity of arrays due to dynamic allocation and pointer storage.

```
node_t * head = NULL;
head = (node_t *) malloc(sizeof(node_t));
if (head == NULL) {
    return 1;
}

head->val = 1;
head->next = NULL;

void print_list(node_t * head) {
    node_t * current = head;

    while (current != NULL) {
        printf("%d\n", current->val);
        current = current->next;
    }
}
```

Resources : 
- [Linked lists](https://www.learn-c.org/en/Linked_lists)