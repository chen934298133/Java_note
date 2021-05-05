[](https://blog.csdn.net/kunug/article/details/111315262)
[](https://www.cnblogs.com/suizhikuo/p/8409153.html)

# 一、 包命名规范概述说明(DO、DTO、VO、PO)
## 命名
- 领域模型命名规约：
  - 数据对象：`xxxDO`，`xxx`即为数据表名。
  - 数据传输对象：`xxxDTO`，`xxx`为业务领域相关的名称。
  - 展示对象：`xxxVO`，`xxx`一般为网页名称。
  - `POJO`是`DO/DTO/BO/VO`的统称，禁止命名成`xxxPOJO`。

- 分层领域模型规约：
  - DO（ Data Object）：与数据库表结构一一对应，通过DAO层向上传输数据源对象。
  - DTO（ Data Transfer Object）：数据传输对象，Service或Manager向外传输的对象。
  - BO（ Business Object）：业务对象。 由Service层输出的封装业务逻辑的对象。
  - AO（ Application Object）：应用对象。 在Web层与Service层之间抽象的复用对象模型，极为贴近展示层，复用度不高。
  - VO（ View Object）：显示层对象，通常是Web向模板渲染引擎层传输的对象。
  - POJO（ Plain Ordinary Java Object）：在本手册中， POJO专指只有setter/getter/toString的简单类，包括DO/DTO/BO/VO等。
  - Query：数据查询对象，各层接收上层的查询请求。 注意超过2个参数的查询封装，禁止使用Map类来传输。
  
## DO(Data Object): —包命名为DO
- 简单一点理解就是里面的字段一一与数据库表字段对应，需与数据库打交道的就使用DO对象
  - 视图对象，用于展示层，它的作用是把某个指定页面（或组件）的所有数据封装起来。
<details>
<summary> &#127809; DTO -> DO &#127809; </summary>
  
- 需要进行商品的更新操作，这里可以看到对数据库进行打交道的时候，**是将传递过来的DTO对象转化为了DO对象**，然后才进行更新操作的
```java
public Integer updateProduct(ProductDTO productDTO) {
    ProductDO productDO = new ProductDO();
    BeanUtils.copyProperties(productDTO, productDO);
    return productMapper.updateById(productDO);

}
```
</details>

## DTO(Data Transfer Object): —包命名为DTO
- 简单一点理解就是在数据传输的时候用到的
  - 数据传输对象，这个概念来源于`J2EE`的设计模式，原来的目的是为了`EJB`的分布式应用提供粗粒度的数据实体，以减少分布式调用的次数，从而提高分布式调用的性能和降低网络负载
  - 但在这里，我泛指用于展示层与服务层之间的数据传输对象。

<details>
<summary> &#127809; DO -> DTO &#127809; </summary>
  
- 根据id对商品进行查询，这里可以看到数据库中查询出来返回的结果是DO对象，**要将数据进行传递就转化为DTO对象**
```java
   	public ProductDTO selectById(Long id) {
        ProductDO productDO = productMapper.selectProductById(id);
        if (ObjectUtils.isEmpty(productDO)) {
            return null;
        }
        ProductDTO productDTO = new ProductDTO();
        BeanUtils.copyProperties(productDO, productDTO);
        return productDTO;
    }
```
</details>

## VO(View Object) —包命名为VO
- 简单一点理解就是用于给页面显示使用的
  - 领域对象，就是从现实世界中抽象出来的有形或无形的业务实体。

<details>
<summary> &#127809; DTO -> VO &#127809; </summary>
  
- 根据id进行商品查询，**这里可以看到通过将DTO对象转化为VO对象**，返给前端的是VO对象
```java
@GetMapping("/pid/{pid}")
@ApiOperation("根据商品id查询商品")
@Log("查询商品")
ApiResult<ProductVO> selectById(@PathVariable("pid") Long pid) {
    ProductDTO productDTO = productService.selectById(pid);
    ProductVO productVO=new ProductVO()
    BeanUtils.copyProperties(productDTO, productVO);
    return ApiResult.ok(productVO);
}
```
</details>

## QueryParam —包命名为param
- 简单一点理解就是对请求参数使用的，也可以使用DTO作为请求参数（可能每个公司要求的不一样）
  - **持久化对象**，它跟持久层（通常是关系型数据库）的数据结构形成一一对应的映射关系
  - 如果持久层是关系型数据库，那么，数据表中的每个字段（或若干个）就对应PO的一个（或若干个）属性。

<details>
<summary> &#127809; DTO -> PO &#127809; </summary>
  
- 根据商品一些不同的属性进行分页展示，这里可以看到**接收请求的参数使用的是`QueryParam`作为请求参数**
- (可以看到这里往前端返回的对象是有问题的，应该返回的是`VO`对象，这里我就不进行调整了，要注意深拷贝浅拷贝问题，可能有些公司不是那么规范，甚至不使用`VO`，直接给前端返回`DTO`）
```java
@PostMapping("/products")
@ApiOperation("商品分页列表")
ApiResult<Paging<ProductDTO>> pageData(@Valid @RequestBody ProductQueryParam query) {
     Paging<ProductDTO> productPageInfo = productService.selectPage(query);
     return ApiResult.ok(productPageInfo);
}

```
</details>
  
  
  
  
# 二、(DO、DTO、VO、PO)区别、用处
  
![三层架构应用时序图](http://lc-dDwI9S44.cn-n1.lcfile.com/zQTl2BiFcAzBBi87w1zATR3yvE9NMNSy/DTO%E3%80%81DO%E3%80%81VO%E3%80%81PO.png)
  
- 用户发出请求（可能是填写表单），表单的数据在展示层被匹配为VO。
- **展示层**把VO转换为**服务层**对应方法所要求的DTO，传送给**服务层**。
  - VO -> DTO
- **服务层**首先根据DTO的数据构造（或重建）一个DO，调用DO的业务方法完成具体业务。
  - DTO -> DO
- **服务层**把DO转换为**持久层**对应的PO（可以使用ORM工具，也可以不用），调用**持久层**的持久化方法，把PO传递给它，完成持久化操作。
  - DO -> PO
- 对于一个逆向操作，如读取数据，也是用类似的方式转换和传递，略。
  
## VO -> DTO 区别应用
  
<details>
<summary> &#127809; VO -> DTO 必要性 &#127809; </summary>
  
## 1. 区别
  
- 对于绝大部分的应用场景来说，`DTO`和`VO`的属性值**基本**是一致的，而且他们通常都是`POJO`，因此似乎没必要多此一举。
  - 但不要忘记**这是实现层面的思维**，对于**设计层面**来说，概念上还是应该存在`VO`和`DTO`，因为**两者有着本质的区别**，
    - `DTO`代表**服务层**需要接收的数据和返回的数据
    - `VO`代表**展示层**需要显示的数据。
  
## 2. 例子

- 例如服务层有一个getUser的方法返回一个系统用户，其中有一个属性是gender(性别)，
  - 对于服务层来说，它只从语义上定义：1-男性，2-女性，0-未指定，
  - 而对于展示层来说，它可能需要用“帅哥”代表男性，用“美女”代表女性，用“秘密”代表未指定。
- 说到这里，可能你还会反驳，在服务层直接就返回“帅哥美女”不就行了吗？对于大部分应用来说，这不是问题，但设想一下，如果需求允许客户可以定制风格，而不同风格对于“性别”的表现方式不一样，又或者这个服务同时供多个客户端使用（不同门户），而不同的客户端对于表现层的要求有所不同，那么，问题就来了。
- 再者，回到设计层面上分析，从职责单一原则来看，服务层只负责业务，与具体的表现形式无关，因此，它返回的DTO，不应该出现与表现形式的耦合。
  
## 3. 应用：如何在应用中做出正确的选择。
  
1. 在以下才场景中，我们可以考虑把`VO`与`DTO`二合为一（注意：是实现层面）：
    - 当需求非常清晰稳定，而且客户端很明确只有一个的时候，没有必要把`VO`和`DTO`区分开来，这时候`VO`可以退隐，用一个`DTO`即可，
    - 为什么是`VO`退隐而不是`DTO`？
      - 回到设计层面，**服务层** 的职责依然不应该与 **展示层** 耦合，所以，对于前面的例子，你很容易理解，`DTO`对于“性别”来说，依然不能用“帅哥美女”，这个转换应该依赖于页面的脚本（如`JavaScript`）或其他机制（`JSTL、EL、CSS`）。
    - 即使客户端可以进行定制，或者存在多个不同的客户端，如果客户端能够用某种技术（脚本或其他机制）实现转换，同样可以让`VO`退隐。

2. 以下场景需要优先考虑VO、DTO并存：
    1. 当需求not清晰稳定，而且客户端很明确not只有一个的时候
    2. 因为某种技术原因，比如某个框架（如`Flex`）提供自动把`POJO`转换为`UI`中某些`Field`时，可以考虑在实现层面定义出`VO`，这个权衡完全取决于使用框架的自动转换能力带来的开发和维护效率提升与设计多一个`VO`所多做的事情带来的开发和维护效率的下降之间的比对。
    3. 如果页面出现一个“大视图”，而组成这个大视图的所有数据需要调用多个服务，返回多个`DTO`来组装（当然，这同样可以通过服务层提供一次性返回一个大视图的`DTO`来取代，但在服务层提供一个这样的方法是否合适，需要在设计层面进行权衡）。
  
</details>
  

## DTO -> DO 区别应用
  
<details>
<summary> &#127809; DTO -> DO 必要性 &#127809; </summary>
  
## 1. 区别
  
- 首先是概念上的区别，
  - `DTO`是**展示层**和**服务层**之间的数据传输对象（可以认为是两者之间的协议）
  - `DO`是对**现实世界各种业务角色的抽象**，这就引出了两者在数据上的区别，
  
## 2. 例子

  - 例如`UserInfoDTO`和`UserDO`（对于`DTO`和`DO`的命名规则，参见第一部分），对于一个`getUser`方法来说，本质上它永远不应该返回用户的密码，因此`UserInfoDTO`至少比`UserDO`少一个`password`的数据。而在领域驱动设计中，正如第一部分所说，`DO`不是简单的`POJO`，它具有领域业务逻辑。
  
## 3. 应用
  
1. 从上一例子中，可能会发现问题：既然`getUser`方法返回的`UserInfoDTO`不应该包含`password`，那么就不应该存在`password`这个属性定义，但如果同时有一个`createUser`的方法，传入的`UserInfoDTO`需要包含用户的`password`，怎么办？
    - 在设计层面，**展示层**向**服务层**传递的`DTO`与**服务层**返回给**展示层**的`DTO`在概念上是不同的，但在实现层面，我们通常很少会这样做（定义两个`UserInfoDTO`，甚至更多），因为这样做并不见得很明智，我们完全可以设计一个完全兼容的`DTO`
    - 在**服务层**接收数据的时候，不该由**展示层**设置的属性（如订单的总价应该由其单价、数量、折扣等决定），无论**展示层**是否设置，**服务层**都一概忽略，而在**服务层**返回数据时，不该返回的数据（如用户密码），就不设置对应的属性。
  
2. 对于DO来说，还有一点需要说明：为什么不在服务层中直接返回DO呢？这样可以省去DTO的编码和转换工作，原因如下：
  - 两者在本质上的区别可能导致彼此并不一一对应，一个`DTO`可能对应多个`DO`，反之亦然，甚至两者存在多对多的关系。
  - **`DO`具有一些不应该让展示层知道的数据**
  - 对于某些`ORM`框架（如`Hibernate`）来说，通常会使用“延迟加载”技术，如果直接把`DO`暴露给展示层，对于大部分情况，**展示层**不在事务范围之内（`Open session in view`在大部分情况下不是一种值得推崇的设计），如果其尝试在`Session`关闭的情况下获取一个未加载的关联对象，会出现运行时异常（对于`Hibernate`来说，就是`LazyInitiliaztionException`）。
  - 从设计层面来说，**展示层**依赖于**服务层**，**服务层**依赖于**领域层**，如果把`DO`暴露出去，就会导致**展示层**直接依赖于**领域层**，这虽然依然是**单向依赖**，但这种跨层依赖会**导致不必要的耦合**。

3. 对于`DTO`来说，也有一点必须进行说明，就是`DTO`应该是一个“扁平的二维对象”，举个例子来说明：如果`User`会关联若干个其他实体（例如`Address、Account、Region`等），那么`getUser()`返回的`UserInfo`，是否就需要把其关联的对象的`DTO`都一并返回呢？如果这样的话，必然导致数据传输量的大增，对于分布式应用来说，由于涉及数据在网络上的传输、序列化和反序列化，这种设计更不可接受。如果`getUser`除了要返回`User`的基本信息外，还需要返回一个`AccountId、AccountName、RegionId、RegionName`，那么，请把这些属性定义到`UserInfo`中，把一个“立体”的对象树“压扁”成一个“扁平的二维对象”。笔者目前参与的项目是一个分布式系统，**该系统不管三七二十一**，把一个对象的所有关联对象都转换为相同结构的`DTO`对象树并返回，导致性能非常的慢。
  
</details>
  
  

## DO -> PO 区别应用
  
<details>
<summary> &#127809; DO -> PO 必要性 &#127809; </summary>
  
## 1. 区别
  
- `DO`和`PO`在**绝大部分情况下是一一对应的**，`PO`是只含有`get/set`方法的`POJO`，但某些场景还是能反映出两者在概念上存在本质的区别：
  - `DO`在某些场景下不需要进行显式的持久化，例如利用策略模式设计的商品折扣策略，会衍生出折扣策略的接口和不同折扣策略实现类，这些折扣策略实现类可以算是DO，但它们只驻留在静态内存，不需要持久化到持久层，因此，这类DO是不存在对应的PO的。

- PO的某些属性值对于DO没有任何意义，这些属性值可能是为了解决某些持久化策略而存在的数据，例如为了实现“乐观锁”，PO存在一个version的属性，这个version对于DO来说是没有任何业务意义的，它不应该在DO中存在。同理，DO中也可能存在不需要持久化的属性。
  
  
  
## 2. 例子

  - 同样的道理，某些场景下，PO也没有对应的DO，例如老师Teacher和学生Student存在多对多的关系，在关系数据库中，这种关系需要表现为一个中间表，也就对应有一个TeacherAndStudentPO的PO，但这个PO在业务领域没有任何现实的意义，它完全不能与任何DO对应上。这里要特别声明，并不是所有多对多关系都没有业务含义，这跟具体业务场景有关，例如：两个PO之间的关系会影响具体业务，并且这种关系存在多种类型，那么这种多对多关系也应该表现为一个DO，又如：“角色”与“资源”之间存在多对多关系，而这种关系很明显会表现为一个DO——“权限”。
  - 某些情况下，为了某种持久化策略或者性能的考虑，一个PO可能对应多个DO，反之亦然。例如客户Customer有其联系信息Contacts，这里是两个一对一关系的DO，但可能出于性能的考虑（极端情况，权作举例），为了减少数据库的连接查询操作，把Customer和Contacts两个DO数据合并到一张数据表中。反过来，如果一本图书Book，有一个属性是封面cover，但该属性是一副图片的二进制数据，而某些查询操作不希望把cover一并加载，从而减轻磁盘IO开销，同时假设ORM框架不支持属性级别的延迟加载，那么就需要考虑把cover独立到一张数据表中去，这样就形成一个DO对应多个PO的情况。
## 3. 应用
  
- 由于ORM框架的功能非常强大而大行其道，而且JavaEE也推出了JPA规范，现在的业务应用开发，基本上不需要区分DO与PO，PO完全可以通过JPA，Hibernate Annotations/hbm隐藏在DO之中。虽然如此，但有些问题我们还必须注意：

  - 对于DO中不需要持久化的属性，需要通过ORM显式的声明，如：在JPA中，可以利用@Transient声明。
  - 对于PO中为了某种持久化策略而存在的属性，例如version，由于DO、PO合并了，必须在DO中声明，但由于这个属性对DO是没有任何业务意义的，需要让该属性对外隐藏起来，最常见的做法是把该属性的get/set方法私有化，甚至不提供get/set方法。但对于Hibernate来说，这需要特别注意，由于Hibernate从数据库读取数据转换为DO时，是利用反射机制先调用DO的空参数构造函数构造DO实例，然后再利用JavaBean的规范反射出set方法来为每个属性设值，如果不显式声明set方法，或把set方法设置为private，都会导致Hibernate无法初始化DO，从而出现运行时异常，可行的做法是把属性的set方法设置为protected。
  - 对于一个DO对应多个PO，或者一个PO对应多个DO的场景，以及属性级别的延迟加载，Hibernate都提供了很好的支持，请参考Hibnate的相关资料。
  
</details>