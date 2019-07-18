---
title: JPA分页
date: 2019-07-18 20:08:42
tags: [Java]
---

参考下面的例子

```java
// extens CrudRepository 也没报错，可能也行
public interface UserRepository extends PagingAndSortingRepository<User, Long> {
    @Query("select u from User u")
    Page<User> getUserList(Pageable pageable);
}
```

```java
@RestController
@RequestMapping("/user")
public class UserResource  extends BaseResource{
    @GetMapping(path = "/list")
    public @ResponseBody
    Page<User> list(@RequestParam("pageNum") Integer pageNum,
                    @RequestParam("pageSize") Integer pageSize) {
            return userRepository.getUserList(PageRequest.of(pageNum, pageSize,
                    new Sort(Sort.Direction.DESC, "soldTotal", "soldCnt")));
    }
}
```
