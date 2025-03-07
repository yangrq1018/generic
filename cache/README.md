<!-- Code generated by gomarkdoc. DO NOT EDIT -->

# cache

```go
import "github.com/zyedidia/generic/cache"
```

Package cache provides an implementation of a key\-value store with a maximum size\. Once the maximum size is reached\, the cache uses a least\-recently\-used policy to evict old entries\. The cache is implemented as a combined hashmap and linked list\. This ensures all operations are constant\-time\.

<details><summary>Example</summary>
<p>

```go
package main

import (
	"fmt"
	"github.com/zyedidia/generic/cache"
)

func main() {
	c := cache.New[int, int](2)

	c.Put(42, 42)
	c.Put(10, 10)
	c.Get(42)
	c.Put(0, 0) // evicts 10

	c.Each(func(key, val int) {
		fmt.Println("each", key)
	})

	c.Resize(3)
	fmt.Println("size", c.Size())
	fmt.Println("capacity", c.Capacity())

	c.SetEvictCallback(func(key, val int) {
		fmt.Println("evict", key)
	})
	c.Put(1, 1)
	c.Put(2, 2) // evicts 42
	c.Remove(3) // no effect
	c.Resize(1) // evicts 0 and 1

	c.Each(func(key, val int) {
		fmt.Println("each", key)
	})

}
```

#### Output

```
each 0
each 42
size 2
capacity 3
evict 42
evict 0
evict 1
each 2
```

</p>
</details>

## Index

- [type Cache](<#type-cache>)
  - [func New[K comparable, V any](capacity int) *Cache[K, V]](<#func-new>)
  - [func (t *Cache[K, V]) Capacity() int](<#func-cachek-v-capacity>)
  - [func (t *Cache[K, V]) Each(fn func(key K, val V))](<#func-cachek-v-each>)
  - [func (t *Cache[K, V]) Get(k K) (V, bool)](<#func-cachek-v-get>)
  - [func (t *Cache[K, V]) Put(k K, e V)](<#func-cachek-v-put>)
  - [func (t *Cache[K, V]) Remove(k K)](<#func-cachek-v-remove>)
  - [func (t *Cache[K, V]) Resize(capacity int)](<#func-cachek-v-resize>)
  - [func (t *Cache[K, V]) SetEvictCallback(fn func(key K, val V))](<#func-cachek-v-setevictcallback>)
  - [func (t *Cache[K, V]) Size() int](<#func-cachek-v-size>)
- [type KV](<#type-kv>)


## type [Cache](<https://github.com/zyedidia/generic/blob/master/cache/cache.go#L15-L20>)

A Cache is an LRU cache for keys and values\. Each entry is put into the table with an associated key used for looking up the entry\. The cache has a maximum size\, and uses a least\-recently\-used eviction policy when there is not space for a new entry\.

```go
type Cache[K comparable, V any] struct {
    // contains filtered or unexported fields
}
```

### func [New](<https://github.com/zyedidia/generic/blob/master/cache/cache.go#L28>)

```go
func New[K comparable, V any](capacity int) *Cache[K, V]
```

New returns a new Cache with the given capacity\.

### func \(\*Cache\[K\, V\]\) [Capacity](<https://github.com/zyedidia/generic/blob/master/cache/cache.go#L102>)

```go
func (t *Cache[K, V]) Capacity() int
```

Capacity returns the maximum capacity of the cache\.

### func \(\*Cache\[K\, V\]\) [Each](<https://github.com/zyedidia/generic/blob/master/cache/cache.go#L108>)

```go
func (t *Cache[K, V]) Each(fn func(key K, val V))
```

Each calls 'fn' on every value in the cache\, from most recently used to least recently used\.

### func \(\*Cache\[K\, V\]\) [Get](<https://github.com/zyedidia/generic/blob/master/cache/cache.go#L38>)

```go
func (t *Cache[K, V]) Get(k K) (V, bool)
```

Get returns the entry associated with a given key\, and a boolean indicating whether the key exists in the table\.

### func \(\*Cache\[K\, V\]\) [Put](<https://github.com/zyedidia/generic/blob/master/cache/cache.go#L49>)

```go
func (t *Cache[K, V]) Put(k K, e V)
```

Put adds a new key\-entry pair to the table\.

### func \(\*Cache\[K\, V\]\) [Remove](<https://github.com/zyedidia/generic/blob/master/cache/cache.go#L81>)

```go
func (t *Cache[K, V]) Remove(k K)
```

Remove causes the entry associated with the given key to be immediately evicted from the cache\.

### func \(\*Cache\[K\, V\]\) [Resize](<https://github.com/zyedidia/generic/blob/master/cache/cache.go#L89>)

```go
func (t *Cache[K, V]) Resize(capacity int)
```

Resize changes the maximum capacity for this cache to 'capacity'\.

### func \(\*Cache\[K\, V\]\) [SetEvictCallback](<https://github.com/zyedidia/generic/blob/master/cache/cache.go#L116>)

```go
func (t *Cache[K, V]) SetEvictCallback(fn func(key K, val V))
```

SetEvictCallback sets a callback to be invoked before an entry is evicted\. This replaces any prior callback set by this method\.

### func \(\*Cache\[K\, V\]\) [Size](<https://github.com/zyedidia/generic/blob/master/cache/cache.go#L97>)

```go
func (t *Cache[K, V]) Size() int
```

Size returns the number of active elements in the cache\.

## type [KV](<https://github.com/zyedidia/generic/blob/master/cache/cache.go#L22-L25>)

```go
type KV[K comparable, V any] struct {
    Key K
    Val V
}
```



Generated by [gomarkdoc](<https://github.com/princjef/gomarkdoc>)
