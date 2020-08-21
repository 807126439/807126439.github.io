## spring 源码学习



常用注解：

@configuration 指名当前类为配置类

@bean	生成bean

@ComponentScan

	包扫描：1.扫描范围指定。2.扫描过滤器。3.自定义过滤规则

	includeFilters = {@Filter(FilterType,class= )}

	注意：使用会取消默认的规则

@scope

	1.单实例 创建IOC时即创建@singleton

	2.多实例 使用时才创建 @prototype

	3.request

	4.session

@lazy	单实例使用时再实例化

@controller,@service,@Reposity,@component	组件注解

@Conditional(XXConditional.class)	-> implement condition	符合条件才注入bean

@Import(Value={XXX.class})	注入bean的方式



四种bean注册方式

1.@bean

2.@controller.........X4

3.@import

	1).注入的bean的id为全类名

	2).实现importSelector接口类

	3).importBeanDefinitionRegistar 手动添加组件到IOC

4.factoryBean实现再@bean注入，实际会注入FactoryBean的泛型



bean的生命周期

constructor      init            destroy

三种实现bean初始化操作

1.@bean的initMethod

2.@PostConstruct    @PreDestroy

3.实现init??的接口和Destroy的接口

	组件实现BeanPostProcessor接口

	可以生成Bean前后操作

	所有注解都有对应Processor处理



@Value基本属性赋值

1.基本字符

2.SpringEL#{20-2}

3.properies文件属性@propertySource	${}



注入注解

@Autowired

	@Qualifier("Name")指定注入名，@Primary修饰Bean注入首选项

@Resource	1.不支持Primary

			2.不支持required = false

@Inject	第三方javax.Inject包，不支持required = false



AOP

@EnableAspectAutoProxy启动

@Aspect指定切口方法

切口位置：

	1.前置	@Before

	2.后置,无论有无异常	@After

	3.返回，正常返回	@AfterReturning

	4.异常	@AfterThrowing

	5.环绕	@Around

原理：使用动态代理增强对象，方法中要手动执行joinPoint.procced()



@After(execution(public int com.XXXX.dev(int,int)))

可抽出@PointCut

Aop获取参数

（throwing="exception",returning = "result"）也可以通过joinPoint获取所有相关



事务使用

	非springboot下：

	1.@EnableTransactionManagement,注入PlatfromTransactionManager



BeanFactory:	IOC容器，用来获取Bean

FactoryBean:工厂模式接口，实现生成Bean并添加入容器



Bean在spring管理中的过程

定义	->	创建对象		->	赋值	-》	初始化（前置处理器）	-》	后置处理器	-》	注销



组件：

	1.BeanFactoryPostProcessor:	beanFactory的后置处理器

	2.BeanDefinitionRegistoryPostProcessor:	bean定义后置处理器

		2继承1	先2后1



Spring流程：refresh（）方法

	1.准备

	2.创建BeanFactory实例

	3.beanFactory预准备工作

	4.PostProcessBeanFactory	目前为空，可由用户修改

	5.执行BeanFactoryPostProcessor	后置处理器

		两个接口，先2后1，其中2按实现接口类型排序实现

	6.注册Bean后置处理器，BeanPostProcessor类

	7.标签国际化，创建MessageSource组件

	8.事件派发器注册

	9.自定义

	10.注册监听器，事件派发

	11.！！！！将容器中所有项目的单实例bean初始化

		获取定义信息，缓存中获取，标记创建

		AOP:让BeanPostProcessor尝试创建代理增强对象，或者直接创建bean实例

		属性赋值。

		initializeBean:Bean的属性赋值，bean前后处理器，Aop增强入口

	12.完成，finishFlash()



Aop源码：

@Enable标签注入，bean注册	BeanRootDefinition

AnnotationAwareAspectJAutoProxyCreator实现BeanPostProcessor	在6中注册

在尝试生成代理对象执行前后置处理器，在6先创建bean,在11包装返回代理对象
