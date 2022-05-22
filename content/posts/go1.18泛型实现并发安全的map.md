---
title: "Go1.18泛型实现并发安全的map"
date: 2022-04-26T00:03:52+08:00
lastmod: 2022-04-26T00:03:52+08:00
description: "使用go泛型失效并发安全的map结构"
tags: ["golang", "泛型"]
featured_image: "https://cdn.yearnfar.com/blog/22/05/93c5a7f602430ecdebe318fdcf5b3bc3.webp"
# images is optional, but needed for showing Twitter Card
images: []
categories: ""
comment: false
draft: false
author: "yearnfar"
---

使用go泛型失效并发安全的map结构

```go
package mut

import (
	"sync"
)

type Map[K, V comparable] struct {
	mu sync.RWMutex
	values map[K]V
}

func (m *Map[K, V]) Put(key K, value V) {
	m.mu.Lock()
	m.values[key] = value
	m.mu.Unlock()
}

func (m *Map[K, V]) Get(key K) V {
	var v V
	m.mu.RLock()
	v = m.values[key]
	m.mu.RUnlock()
	return v
}

func NewMap[K, V comparable]() *Map[K, V] {
	m := &Map[K, V]{}
	m.values = make(map[K]V)
	return m
}

```

