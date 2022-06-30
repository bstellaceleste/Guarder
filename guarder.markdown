Guarder organizes the heap into **128** *Threads*

Each thread is divided in 16 *bags* (or classes) of sizes 2<sup>4</sup>
to 2<sup>19</sup>, that are randomly placed within the thread (i.e., the
1st bag is not necessarily that of size 2<sup>4</sup>)

A l’initialisation, tout le heap est initialisé: 128 \* (16 \*
\_bibopBagSize)

Class BibopHeap:

        (classes)
        -> BibopObjCache
        -> PerThreadMap
        -> PerThreadBag
        ... 
        (and arrays)
        ...
        -> _threadMap
        -> _threadBag
        -> _bagCache
        -> _freeCache

Guarder allocates a proportion of guard pages inside each bag, define in
`xdefines.hh: default_rand_guard_prop`

Function call stack from malloc (methods of the BibopObjCache class):

    xxmalloc
        |
    allocateSmallObject
        |
    malloc
        |
    getRandomObject
        |
    getNext
        |
    TryRandomGuardPage

In class BibopHeap::PerThreadBag, I had a field frequency
