---
title: 'org.hibernate.QueryException: Ordinal parameter not bound'
date: 2019-07-10 21:20:10
tags: [Java, JPA]
---

使用的代码为

```java
// class PhotoRepository
@Component
public interface PhotoRepository extends CrudRepository<Photo, Long> {
    @Query("select count(cosId) from Photo p where p.cosId = ?2")
    int fixPartialCheckCount(Integer price, String cosId);        // 第一参数没有用到，只使用第二参数
}


// api class
@RequestMapping(value = "photo/data-import-fix-partial-check", method = RequestMethod.POST)
public @ResponseBody
Integer fixPartialCheckCount(@RequestBody Photo req) {
    int count = 0;
    try {
        count = photoRepository.fixPartialCheckCount(req.getPrice(), req.getCosId());
    } catch (Exception e) {
        System.out.println("duplicate " + e.getMessage());
    }
    return count;
}
```

得到了题目上的这个异常。尝试将`fixPartialCheckCount`改为

```java
// PhotoRepository
@Query("select count(cosId) from Photo p where p.cosId = ?1")
int fixPartialCheckCount(String cosId);


// api class
count = photoRepository.fixPartialCheckCount(req.getCosId());
```

则正常运行并符合预期。

JPA要求输入参数列表必须全都使用吗？

继续尝试下面的更改：

```java
// PhotoRepository
@Query("select count(cosId) from Photo p where p.cosId = ?1")
int fixPartialCheckCount(String cosId, Integer price); // 对调最开始的例子的两个参数位置，保证上面的Query注解是`?1`起始


// api class
count = photoRepository.fixPartialCheckCount(req.getCosId(), req.getPrice());
```

也正常运行。

所以猜测，`@Query`必须要保证至少`?1`要有。
