.. SPDX-License-Identifier: GPL-2.0

.. _zblock:

======
zblock
======

Zblock stores integer number of compressed objects per block. These blocks
consist of several consecutive physical pages (from 1 to 8) and are arranged
in lists. The range from 0 to PAGE_SIZE is divided into the number of intervals
corresponding to the number of lists and these only operate on objects of size
from its interval. Thus the block lists are isolated from each other, which
makes it possible to simultaneously perform actions with several objects
from different lists.

With zlock, it is possible to densely arrange objects of various sizes,
resulting in low internal fragmentation. Also this allocator tries to fill
incomplete blocks instead of adding new ones. As a result, in many cases it
provides a compression ratio substantially higher than z3fold and zbud. Zblock
does not require MMU and also is superior to zsmalloc with regard to the worst
execution times, thus allowing for better response time and real-time
characteristics of the whole system.

Like similar allocation method from z3fold and zsmalloc, zblock_alloc() does
not return a dereferenceable pointer. Instead, it returns an unsigned long
handle which encodes actual location of the allocated object.

Unlike zbud and z3fold, zblock works well with objects of various sizes -
including but not limited to highly compressed and poorly compressed, as well
as cases where both object types exist.
