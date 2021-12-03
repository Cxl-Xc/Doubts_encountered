使用通用接口（IService<T>）快速开发Service

使用通用实现类（ServiceImpl<M,T>）快速开发ServiceImpl

可以在通用接口基础上做功能重载或功能追加

注意重载时不要覆盖原始的操作，避免原始提供的功能丢失



操作步骤

新建一个service包 在其报下新建接口IBookService

IBookService：

```java
public interface IBookService extends IService<Book> {

}
```

在service包的子包Impl下新建IBookService的实现类IBookServiceImpl

IBookServiceImpl：

```java
@Service
public class IBookServiceImpl extends ServiceImpl<BookDao, Book> implements IBookService {

}
```

