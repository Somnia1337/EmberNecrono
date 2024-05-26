@2024.04.25 | Week 09

卢剑歌 2022141461145

```c
typedef struct stack_s
{
    /* You can define the stack data type in any way that you like.
     * You are allowed to use the standard libc malloc/free
     * routines for the stack management, if you wish.
     * DO NOT use gc_malloc!
     */
    struct stack_s *pre;
    void *reach_ptr;
} stack_t;

void push_stack(stack_t **my_stack, stack_t *next_one)
{
    if (*my_stack == NULL)
    {
        *my_stack = next_one;
        return;
    }
    next_one->pre = (*my_stack);
    *my_stack = next_one;
}

stack_t *pop_stack(stack_t **my_stack)
{
    if (my_stack == NULL)
    {
        return NULL;
    }
    stack_t *poped_one = *my_stack;
    *my_stack = poped_one->pre;
    return poped_one;
}
```

```c
/* 0. Make sure there is already allocated memory to
 *    garbage collect.
 */
alloc_tree = *(Tree **)(dseg_lo + sizeof(long long));
if (alloc_tree == NULL)
{
	return;
}
```

```c
/* 1. collect root pointers by scanning regs, stack, and global data */
stack_t *pointer_stack = NULL;

for (int i = 0; i < 3; i++)
{
	if (is_ptr((void *)(regs[i])))
	{
		stack_t *sp = malloc(sizeof(stack_t));
		sp->pre = NULL;
		sp->reach_ptr = (void *)(regs[i]);
		push_stack(&pointer_stack, sp);
	}
	if (is_ptr((void *)(registers[i])))
	{
		stack_t *sp = malloc(sizeof(stack_t));
		sp->pre = NULL;
		sp->reach_ptr = (void *)(registers[i]);
		push_stack(&pointer_stack, sp);
	}
}

for (int i = pgm_stack; i < top; i++)
{
	void *stack_addr = (void *)i;
	if (is_ptr(stack_addr))
	{
		push_stack(&pointer_stack, stack_addr);
	}
}
```

<div style="page-break-after: always"></div>

```c
/* 2. mark nodes that are reachable from the roots and add any pointers
 *    found in reachable heap memory
 */
mark_all(alloc_tree);
while (pointer_stack != NULL)
{
	stack_t *elem = pop_stack(&pointer_stack);
	void *reachable = elem->reach_ptr;
	Tree *node = contains((ptr_t)reachable, &alloc_tree);
	if (node != NULL)
	{
		node->size &= ~0x1;
	}
	free(elem);
}
```

```c
/* 3. collect pointers to all the unmarked (i.e. unreachable) blocks */
collect_unmarked(alloc_tree, stack);
```

```c
/* 4. delete each unmarked block from the allocated tree and
 *    then place the block on the free list.
 *    Call verify_garbage with the address previously returned by malloc
 *    for each block.
 */
while (stack != NULL)
{
	stack_t *one_s = pop_stack(&stack);
	Tree *garbage = *((Tree **)(one_s->reach_ptr));
	verify_garbage(garbage + sizeof(Tree));
	*(Tree **)(dseg_lo + sizeof(long long)) = delete (garbage, alloc_tree);
	alloc_tree = *(Tree **)(dseg_lo + sizeof(long long));
	gc_add_free((char *)dseg_lo, (list_t *)garbage);
	free(one_s);
}
```