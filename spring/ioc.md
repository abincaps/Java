
## IoC 容器

- IoC (Inversion of Control) 控制反转，也是依赖注入 DI（Dependency Injection）
- 实现对象之间的依赖和解耦
- IoC容器负责对象的创建、初始化等完整生命周期
- IoC容器自动装配对象, 统一了对象管理和配置
- 对象A直接引用和操作new出来对象B，变成对象A只依赖接口B，系统启动和装配阶段，把B接口的实例对象注入到对象A中， 对象A就无需依赖B接口的具体实现

## IOC 流程

## getBean流程

- XML 配置文件一次性加载， 通过配置文件的地址以流的方式读取到内存
- XML 配置文件解析， 进行语义处理， 按照bean标签为单位解析，通过集合统一存储bean标签数据
- bean标签对应一个存储类 BeanDefinition 进行单独存储
- 使用 PropertyValue 表示 bean 标签中的一个 property 标签
- TypedStringValue 基本类型的值
- RuntimeBeanReference 引用类型的值
- 通过反射且使用单例模式创建对象，使用 map 集合来实现单例管理
- `Class clazz = Class.forName(类的全限定名);`
- `Object bean = clazz.newInstance;`
- 遍历设置属性
- `Field field = clazz.getField(属性名);`
- `field.set(bean, 属性值);`


