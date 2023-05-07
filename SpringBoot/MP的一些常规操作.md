# MP的一些常规操作

​	在mapper接口中是扩展BaseMapper接口，里面帮我们封装了很多方法

![image-20220429094721886](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204290947029.png)

> 下面是几种CRUD的操作方法

## 1、添加数据：

使用的，insert(T), 这里面的 **T** 是一个泛型。

```java
/**
 * 使用MP进行添加数据
 */
@Test
public void insert(){
    Book book = new Book();
    book.setSn("JTwo");
    book.setName("test1");
    book.setAuthor("Jim");

    book.setPrice(new BigDecimal(50.55));
    book.setDirId(15L);
    bookMapper.insert(book);
}
```

![image-20220429095804084](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204290958114.png)

## 2、更新数据：

### 1> 根据id更新

使用updateById（T）:

```java
/**
 * 根据id进行修改数据
 */
@Test
public void updateById(){
    Book book = new Book();
    book.setId(42L); // 传入需要修改的id序号
    book.setSn("JTwo");
    book.setName("test1");
    book.setAuthor("Jim");
    book.setPrice(new BigDecimal(50.00));
    book.setDirId(15L);
    bookMapper.updateById(book);
}
```

![image-20220429095816935](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204290958962.png)

### 2>根据条件进行修改

#### 1、使用update（T ,wrapper）

```java
/**
* 根据 whereEntity 条件，更新记录 
* @param entity 实体对象 (set 条件值,可以为 null) 
* @param updateWrapper 实体对象封装操作类（可以为 null,里面的 entity 用于生成 where 语句） 
*/ 

int update(@Param(Constants.ENTITY) T entity, @Param(Constants.WRAPPER) Wrapper<T> 
updateWrapper); 
```

```java
@Test
public void update1(){
    Book book = new Book();
    book.setSn("Ji");
    book.setName("test2");
    book.setAuthor("JOM");
    book.setPrice(new BigDecimal(50.00));
    book.setDirId(15L); // 更改的字段
    QueryWrapper<Book> wrapper= new QueryWrapper<Book>();
    wrapper.eq("id",42L); // 更改的条件
    bookMapper.update(book,wrapper);
}
```

![image-20220429142406232](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291424265.png)

#### 2、使用updateWrapper

```java
/**
 * 使用updateWrapper
 */
@Test
public void update2(){
    Book book = new Book();
    UpdateWrapper<Book> wrapper= new UpdateWrapper<>();
    wrapper.eq("id",42L); // 更改的条件
    wrapper.set("name","Tom");
    bookMapper.update(null,wrapper);
}
```

![image-20220429142453846](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291424880.png)

## 3、删除数据

### 1>使用deleteById

```java
/**
* 根据 ID 删除 
* @param id 主键ID 
*/ 
int deleteById(Serializable id);
```

```java
@Test
public void deleteById(){
    int i = bookMapper.deleteById(42L);
    if (i==1){
        System.out.println("成功删除");
    }
}
```

![image-20220429142859025](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291428056.png)

查看数据库可以发现数据以被删除。

### 2>使用deleteByMap

```java
/**
* 根据 columnMap 条件，删除记录 
* @param columnMap 表字段 map 对象 
*/ 
int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);
```

```java
/**
 * 使用deleteByMap
 */
@Test
public void deleteByMap(){
    Map<String, Object> map = new HashMap<>();
    map.put("name","JavaEE");
    map.put("sn",101);
    int i = bookMapper.deleteByMap(map);
    if (i==1){
        System.out.println("成功删除");
    }
}
```

![image-20220429143456155](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291434186.png)

> 使用这个在删除时，他们的删除条件都是  使用and进行连接起来的。

### 3>使用delete

```java
/**
* 根据 entity 条件，删除记录 
* @param wrapper 实体对象封装操作类（可以为 null） 
*/ 
int delete(@Param(Constants.WRAPPER) Wrapper<T> wrapper);
```

```java
/**
 * 使用delete进行删除
 * 同时把删除条件使用实体类进行包装起来
 */
@Test
public void delete(){
    Book book = new Book();
    book.setName("t");
    book.setSn("J");
    // 把实体类对象包装，包装成操作条件
    QueryWrapper<Book> wrapper=new QueryWrapper<Book>(book);
    int i = bookMapper.delete(wrapper);
    if (i==1){
        System.out.println("成功删除");
    }
}
```

![image-20220429144411416](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291444453.png)

> 查看数据库数据已经被删除

### 4>批量删除

```java
/**
* 删除（根据ID 批量删除） 
* @param idList 主键ID列表(不能为 null 以及 empty) 
*/ 
int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);
```

```java
/**
 * 使用deleteBatchIds进行批量删除
 */
@Test
public void deleteBatchIds(){
    int i = bookMapper.deleteBatchIds(Arrays.asList(41L,43L));
    if (i!=0){
        System.out.println("成功删除");
    }
}
```

![image-20220429144857466](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291448524.png)

## 4、查询操作 select

### 1> 根据ID进行查询 selectById  

```
/**
* 根据 ID 查询 
* @param id 主键ID 
*/ 
T selectById(Serializable id);
```

```java
/**
 * 通过使用selectById查询单条结果
 */
@Test
public void selectById(){
    Book book = bookMapper.selectById(39L);
    System.out.println(book);
}
```

![image-20220429145358722](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291453766.png)

### 2> 根据id批量查询 selectBatchIds

```java
/**
* 查询（根据ID 批量查询） 
* @param idList 主键ID列表(不能为 null 以及 empty) 
*/
List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);
```

```java
/**
 * 使用selectBatchIds来查询都条数据
 */
@Test
public void selectBatchIds(){
    List<Book> list = bookMapper.selectBatchIds(Arrays.asList(31L, 32L));
    for (Book b : list) {
        System.out.println(b);
    }
}
```

![image-20220429145823075](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291458141.png)

### 3> 根据条件进行单条查询 seleteOne

```java
/**
* 根据 entity 条件，查询一条记录 
* @param queryWrapper 实体对象封装操作类（可以为 null） 
*/ 
T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
```

```java
    @Test
    public void selectOne(){
        Book book = new Book();
        book.setName("H5");
        // 实体类把包装成查询条件。
        QueryWrapper<Book> wrapper=new QueryWrapper<>(book);
        Book book1 = bookMapper.selectOne(wrapper);
        System.out.println(book1);
    }
}
```

![image-20220429150446078](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291504111.png)

> 可以看到我们是查询出来两个结果，这是因为使用selectOne的还回结果只有一条，但是大于1就会报错
>
> ![image-20220429150617881](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291506916.png)

> 下面通过id进行查询
>
> ```java
> /**
>      * 使用selectOne进行单条语句查询，要是用查出来大于1条会报错
>      */
> @Test
> public void selectOne(){
>     QueryWrapper<Book> wrapper=new QueryWrapper<>();
>     wrapper.eq("id",31L);
>     Book book1 = bookMapper.selectOne(wrapper);
>     System.out.println(book1);
> }
> ```

![image-20220429150802685](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291508723.png)

### 4>查询总记录数selectCount

```java
/**
* 根据 Wrapper 条件，查询总记录数 
* @param queryWrapper 实体对象封装操作类（可以为 null） 
*/ 
Integer selectCount(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper); 
```

```java
/**
 * 使用selectCount进行条件查询的总记录数
 */
@Test
public void selectCount(){
    QueryWrapper<Book> wrapper=new QueryWrapper<>();
    wrapper.eq("name","H5");
    Long aLong = bookMapper.selectCount(wrapper);
    System.out.println(aLong);
}
```

![image-20220429151833556](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291518604.png)

> 记录全部数据的数量

```java
    @Test
    public void selectCount(){
        QueryWrapper<Book> wrapper=new QueryWrapper<>();
//        wrapper.eq("name","H5");
        
        Long aLong = bookMapper.selectCount(wrapper);
        System.out.println(aLong);
    }
```

![image-20220429151938222](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291519262.png)

#### gt 大于

> 使用 gt 查询出大于多少的值

```java
 @Test
    public void selectCount(){
        QueryWrapper<Book> wrapper=new QueryWrapper<>();
//        wrapper.eq("name","H5");
        wrapper.gt("id",32L);//id 大于32的
        Long aLong = bookMapper.selectCount(wrapper);
        System.out.println(aLong);
    }
```

![image-20220429152223548](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291522590.png)

### 5> 查询全部数据 selectList

```java
/**
* 根据 entity 条件，查询全部记录 
* @param queryWrapper 实体对象封装操作类（可以为 null） 
*/ 
List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper); 
```

```java
/**
 * 使用selectList进行查询全部数据
 */
@Test
public void selectList(){
    List<Book> list = bookMapper.selectList(null);
    for (Book book:list){
        System.out.println(book);
    }
}
```

![image-20220429152616491](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291526543.png)

### 6>分页查询selectPage

```java
/**
* 根据 entity 条件，查询全部记录（并翻页） 
* @param page 分页查询条件（可以为 RowBounds.DEFAULT） 
* @param queryWrapper 实体对象封装操作类（可以为 null） 
*/ 
IPage<T> selectPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
```

#### 使用MybatisPlusInterceptor接口进行配置分页操作的page

在进行使用selectPage中我们需要对分页的插件进行配置：

```java
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;


@Configuration
public class MyIPage {
    /**
     * 一个IPage的拦截器，
     * @return
     */
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
//        定义拦截器
        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
//        添加拦截器的方法
        mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return mybatisPlusInterceptor;
    }
}
```

> 进行对分页操作中的page进行配置。

```java
/**
 * 使用selectPage进行分页查询，
 * 也可以带上查询条件使用QueryWrapper<Book> wrapper
 */
@Test
public void selectPage(){
    Page<Book> page=new Page<>(1,2);
    QueryWrapper<Book> wrapper=new QueryWrapper<Book>();
    IPage<Book> p = bookMapper.selectPage(page, wrapper);
    System.out.println("当前页数："+p.getCurrent());
    System.out.println("总页数："+p.getPages());
    System.out.println("当前页的数据："+p.getRecords());
    System.out.println("当前页的可展示的条目："+p.getSize());
    System.out.println("总条数："+p.getTotal());
}
```

![image-20220429155231719](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291552779.png)

> 下面进行条件查询分页，和上面的区别就是QueryWrapper<Book> wrapper带上了需要操作的条件。

```java
@Test
public void selectPage(){
    Page<Book> page=new Page<>(1,2);
    QueryWrapper<Book> wrapper=new QueryWrapper<Book>();
    wrapper.eq("sn","002");
    wrapper.or().eq("name","Vue");
    IPage<Book> p = bookMapper.selectPage(page, wrapper);

    System.out.println("当前页数："+p.getCurrent());
    System.out.println("总页数："+p.getPages());
    System.out.println("当前页的数据："+p.getRecords());
    System.out.println("当前页的可展示的条目："+p.getSize());
    System.out.println("总条数："+p.getTotal());
}
```

![image-20220429155921894](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291559952.png)

> 有上面的不难看出我们在操作selectPage时，我们是先去进行操作wrapper，在去处理进行分页的条件。



## 5、MP的一些配置

### 1> 字段对应（实体类与表--驼峰命名法）

mapUnderscoreToCamelCase:是否开启自动驼峰命名规则（camel case）映射，即从经典数据库列名 A_COLUMN（下划线命名） 

到经典 Java 属性名 aColumn（驼峰命名） 的类似映射。

```yml
mybatis-plus:
  configuration:
    #在映射实体或者属性时，将数据库中表名和字段名中的下划线去掉，按照驼峰命名法映射
    map-underscore-to-camel-case: true
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl # 开启日志打印操作时的SQL语句
  global-config:
    db-config:
      id-type: assign_id # id使用雪花命名法
```

如数据库里面的字段：

![image-20220429161336450](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291613507.png)

进行封装的实体类：

![image-20220429161428177](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291614230.png)

> 若是我们在未使用map-underscore-to-camel-case: true时，它的默认为true，要是我们给它设置为false，那样实体类封装的数据就无法进行匹配到数据库里面的字段。

### 2> 开启缓存

```yml
mybatis-plus:
  configuration:
    #在映射实体或者属性时，将数据库中表名和字段名中的下划线去掉，按照驼峰命名法映射
    map-underscore-to-camel-case: true
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
    cache-enabled: true # 全局地开启或关闭配置文件中的所有映射器已经配置的任何缓存，默认为 true
```

### 3>QueryWrapper的一些操作

- eq  等于 =  

- ne  不等于 <>  

- gt   大于 >  

- ge  大于等于 >=  

- lt    小于 < 

- le  小于等于 <=  

- between   BETWEEN 值1 AND 值2   

- notBetween   NOT BETWEEN 值1 AND 值2   

- in   字段 IN (value.get(0), value.get(1), ...)

- notIn 字段 NOT IN (v0, v1, ...)

### 4>模糊查询

- like   LIKE '%值%'

例: like("name", "王") ---> name like '%王%'

- notLike  NOT LIKE '%值%'

例: notLike("name", "王") ---> name not like '%王%'

- likeLeft  LIKE '%值' 

例: likeLeft("name", "王") ---> name like '%王'

- likeRight  LIKE '值%'

例: likeRight("name", "王") ---> name like '王%'

> #### QueryWrapper
>
> 最基础的使用方式是这样
>
> ```java
> // 查询条件构造器
> QueryWrapper<BannerItem> wrapper = new QueryWrapper<>();
> wrapper.eq("banner_id", id);
> // 查询操作
> List<BannerItem> bannerItems = bannerItemMapper.selectList(wrapper);
> ```
>
> 然后我们可以**引入lambda**，避免我们在代码中写类似的于**banner_id**的硬编码
>
> ```java
> QueryWrapper<BannerItem> wrapper = new QueryWrapper<>();
> wrapper.lambda().eq(BannerItem::getBannerId, id);
> List<BannerItem> bannerItems = bannerItemMapper.selectList(wrapper);
> ```
>
> #### LambdaQueryWrapper
>
> 为了简化lambda的使用，我们可以改写成LambdaQueryWrapper构造器，语法如下：
>
> ```java
>LambdaQueryWrapper<BannerItem> wrapper = new QueryWrapper<BannerItem>().lambda();
> wrapper.eq(BannerItem::getBannerId, id);
> List<BannerItem> bannerItems = bannerItemMapper.selectList(wrapper);
> ```
> 
> 我们可以再次将QueryWrapper<BannerItem>.lambda()简化，变成这个样子
>
> ```java
>LambdaQueryWrapper<BannerItem> wrapper = new LambdaQueryWrapper<>();
> wrapper.eq(BannerItem::getBannerId, id);
> List<BannerItem> bannerItems = bannerItemMapper.selectList(wrapper); 
> ```

```java
/**
 * 使用like进行模糊查询
 */
@Test
public void like(){
    String name="V";
    LambdaQueryWrapper<Book> queryWrapper=new LambdaQueryWrapper<Book>();
    // Strings.isNotEmpty(name) 进行判断是否为空
    // Book::getName Book类的成语函数，获得类中成员--name
    queryWrapper.like(Strings.isNotEmpty(name),Book::getName,name);
    queryWrapper.orderByDesc(Book::getId);
    List<Book> list = bookMapper.selectList(queryWrapper);
    System.out.println(list);
}
```

![image-20220429165732669](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291657726.png)

### 5> 排序

- orderBy   排序：ORDER BY 字段, ...

例: orderBy(true, true, "id", "name") ---> order by id ASC,name ASC

- orderByAsc    排序：ORDER BY 字段, ... ASC

例: orderByAsc("id", "name") ---> order by id ASC,name ASC

- orderByDesc   排序：ORDER BY 字段, ... DESC

例: orderByDesc("id", "name") ---> order by id DESC,name DESC

```java
@Test
public void selectList(){
    QueryWrapper<Book> wrapper=new QueryWrapper<>();
    wrapper.orderByDesc("id",null);
    List<Book> list = bookMapper.selectList(wrapper);
    for (Book book:list){
        System.out.println(book);
    }
```

查询全部的同时，并且进行排序：

![image-20220429163041291](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291630346.png)

### 6> 逻辑查询

- or  

拼接 OR  主动调用 or 表示紧接着下一个**方法**不是用 and 连接!(不调用 or 则默认为使用 and 连接) 

- and

AND 嵌套   例: and(i -> i.eq("name", "李白").ne("status", "活着")) ---> and (name = '李白' and status <> '活着')

```java
@Test
public void selectPage(){
    Page<Book> page=new Page<>(1,2);
    QueryWrapper<Book> wrapper=new QueryWrapper<Book>();
    wrapper.eq("sn","002");
    wrapper.or().eq("name","Vue");
    IPage<Book> p = bookMapper.selectPage(page, wrapper);

    System.out.println("当前页数："+p.getCurrent());
    System.out.println("总页数："+p.getPages());
    System.out.println("当前页的数据："+p.getRecords());
    System.out.println("当前页的可展示的条目："+p.getSize());
    System.out.println("总条数："+p.getTotal());
}
```

![image-20220429163315617](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291633687.png)

### 7> 查询指定字段select

- 在MP查询中，默认查询所有的字段，如果有需要也可以通过select方法进行指定字段

```jvaj
@Test
public void selectPage(){
    Page<Book> page=new Page<>(1,2);
    QueryWrapper<Book> wrapper=new QueryWrapper<Book>();
    wrapper.eq("sn","002");
    wrapper.or().eq("name","Vue");
    wrapper.select("id","name","author");
    IPage<Book> p = bookMapper.selectPage(page, wrapper);

    System.out.println("当前页数："+p.getCurrent());
    System.out.println("总页数："+p.getPages());
    System.out.println("当前页的数据："+p.getRecords());
    System.out.println("当前页的可展示的条目："+p.getSize());
    System.out.println("总条数："+p.getTotal());
}
```

![image-20220429164223133](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204291642224.png)

