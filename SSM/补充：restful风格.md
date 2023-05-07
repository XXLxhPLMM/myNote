## 补充：restful风格

​	restful的使用：在进行表现层中的行为有

- GetMapping  查询
- post~  添加
- put ~  修改
- delet~  删除

示例：

```java
/**
 * 下面一个是Restful风格的参数
 * http://localhost:8080/quick13/sdf
 *
 * @param userName
 * @throws JsonProcessingException
 * @RequestMapping("/quick13/{username}")这里面的{username} 是来获取http://localhost:8080/quick13/sdf后面传来的参数sdf
 * 然后我们需要使用@PathVariable(value = "username")来得到
 * "/quick13/{username}"中的{username}
 * 最后@PathVariable(value = "username")中获取到值传给
 * 我们的参数
 */
@RequestMapping("/quick13/{username}")
@ResponseBody
public void save7(@PathVariable(value = "username") String userName) throws JsonProcessingException {
    System.out.println(userName);
}
```

在上面的例中，我们要明白`@PathVariable(value = "username")`和`@RequestParam("name")`的区别；

当我们使用restful风格在访问路径上是使用  /quick13/sdf  我们需要进行获取sdf这个参数，需要在使用@RequestMapping("/quick13/{username}")中使用{username}进行获取参数，接着就是把@@PathVariable(value = "username")来获取参数把值传传给String username中username；



在使用**`@RequestParam`**是获取路径上传来的name对象传来的参数http://localhost:8080/quick13?name=sdf

然后再把获取的值

```java
    @RequestMapping("/quick12")
    @ResponseBody
//    使用@RequestParam
//    value:与请求参数名称
//    required：在此指的是请求参数是否必须包括，
//    默认是true，提交时如果没有此参数则报错
//    defaultValue：当没有指定请求参数，则使用指定的默认值赋值
    public void save6(@RequestParam("name") String username) throws JsonProcessingException {
        System.out.println(username);
    }
```



### 1.概念

​	简单来说就是一种互联网软件框架，使用后可以使表现层结构更加清晰，符号标准，易于理解，扩展时比较方便。

## 2.使用时的HTTP动词

​	有以下几种：

- GET (用于查询)：从服务器取出全部或者一条|多条数据

  下面是进行两个使用GET方法的

```java
 /**
     * 查找通过id
     * @param id
     * @return
     */
    @GetMapping("{id}")
    public R findById(@PathVariable(value = "id") Integer id){
        Book book = bookService.getById(id);
//        bookService.findById(id);
        return new R(true,book);
    }

    /**
     * 查询全部
     * @return
     */
    @GetMapping
    public R all(){
//        List<Book> list = bookService.allList();
        List<Book> list = bookService.list();
        return new R(true,list);
    }
```

使用rest风格的路径展示：下面展示的是根据id查询出来的数据

![image-20220415160205047](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204151602119.png)

进行查询全部数据：

![image-20220415160433534](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204151604607.png)

- POST（用于保存）：在服务器上新建一个资源

```java
/**
 * 添加数据
 * @param book
 * @return
 */
@PostMapping
public R save(@RequestBody Book book){
    boolean flag = bookService.save(book);
    return new R(flag,flag ? "添加成功^_^" : "添加失败-_-!");
}
```

进行添加数据：

![image-20220415160551493](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204151605532.png)

- PUT（用于修改）：用于更新资源

```java
/**
 * 修改全部
 * @param book
 * @return
 */
@PutMapping
public R update(@RequestBody Book book){
    boolean flag = bookService.modify(book);
    return new R(flag,flag?"修改成功^_^" : "数据异常-_-!");
}
```

![image-20220415162212907](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204151622953.png)



- DELETE（用于删除）：删除服务器上面的资源

```
/**
 * 删除
 * @param id
 * @return
 */
@DeleteMapping("{id}")
public R delete(@PathVariable("id") Integer id){
    boolean flag = bookService.delete(id);
    return new R(flag,flag ? "删除成功^~^" :"数据异常，自动刷新");
}
```

删除后资源显示：

![image-20220415162546599](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204151625635.png)

