---
title: Iterable转Stream
date: 2019-07-18 20:16:24
tags: [Java]
---

例子

```java
List<Map<String, String>> list() {
    // Iterable<Album> list = albumRepository.findAll();
    return StreamSupport.stream(albumRepository.findAll().spliterator(), false)
            .map(album -> {
                Map<String, String> map = new HashMap<>();
                map.put("uuid", album.getUuid());
                map.put("name", album.getName());
                return map;
            })
            .collect(Collectors.toList());
}
```
