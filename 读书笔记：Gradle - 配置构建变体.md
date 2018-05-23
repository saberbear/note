# 		读书笔记：Gradle - 配置构建变体 

博客 ：[配置构建变体](https://developer.android.google.cn/studio/build/build-variants#build-types) 



## 配置构建变体

*对象-概念*  构建变体：代表为应用构建的一个不同版本，可能需要根据不同的设备、根据API级别或其他设备变体构建不同的应用版本

 *关系-组成*  构建变体是合并**构建类型**和**产品风味**中配置的设置，代码和资源的结果。不能直接配置构建变体，只能通过配置构建类型和产品风味来配置构建变体。

标签：概念理解



## 配置构建类型 Build Type



*特性-使用/作用*   可以在模块级`build.gradle`文件的`android{}`代码块内部创建和配置构建类型,使用`buildTypes{}`代码块。一般是debug和release两种类型。

*查阅*   如果想要了解对于构建类型可以配置的所有属性，可以查阅[构架类型BuildTypes的DSL参考](http://google.github.io/android-gradle-dsl/current/com.android.build.gradle.internal.dsl.BuildType.html)



## 配置产品风味 Product Falvors

产品风味和构建类型相似，使用`productFlavors{}`代码块配置想要的设置。

`defaultConfig`实际上属于[ProductFalvor](http://google.github.io/android-gradle-dsl/current/com.android.build.gradle.internal.dsl.ProductFlavor.html)类。这意味着，可以在`defaultConfig{}`代码块中提供所有的风味设置，例如`applicationId`

*查看*：配置完构建类型和产品风味后会以`<product-flavor><build-type>`的命名格式产生构建变体，通过工具栏**Build>Select Build Variant**或者侧边栏的**Build Variants**视图查看构建变体，也可以通过工具栏**Build**查看构建类型和产品风味。

#### 组合多个产品风味 (特性，由本质衍生，也可与构建类型比较)

产品风味是从不同的属性描述构建版本的要求，各个属性描述可以归为不同的维度dimension，比如sdk版本维度、版本version维度。一个构建版本可以从不同的维度进行描述，所以可以使用具体产品风味的`dimension`属性定义产品风味的维度，然后通过`android{}`代码块的`flavorDimensions`属性组合不同维度的产品风味对构建版本的进行描述。

Gradle 创建的构建变体数量等于每个风味维度中的风味数量与您配置的构建类型数量的乘积。在 Gradle 为每个构建变体或对应 APK 命名时，属于较高优先级风味维度的产品风味首先显示，之后是较低优先级维度的产品风味，再之后是构建类型。 

标签：理解，使用，构造变体命名

#### 过滤变体

配置一个变体过滤器，移除某些变体设置，被过滤掉的变体将不会出现在变体列表中。

使用`variantFilter{}`代码块，具体例子查看博客。

标签：理解，使用



## 创建源集



源集：Android Studio按逻辑关系将每个**模块的源代码和资源**分组为*源集* 。便于构建不同的构建变体、构建类型和产品风味。

模块的`main/`源集即**主源集**包括所有的构建变体所**共用**的代码和资源。其他源集目录为可选项，Android Studio不会为新构建变体创建源集目录。

创建源集，执行以下操作：

1. 导航至 **MyApplication > Tasks > android** 并双击 **sourceSets** 。查看查看如何根据不同的构建变体（构建类型、产品风味）组织文件。
2. 使用**Project视图**结合**Target Source Set**创建文件和文件夹。

#### 更改默认源集设置（影响范围，涉及范围）

Gradle让不同的构建变体去默认的文件目录位置寻找相应文件组件，使用`sourceSets`属性结合[AndroidSourceSet](http://google.github.io/android-gradle-dsl/current/com.android.build.gradle.api.AndroidSourceSet.html)属性配置gradle去哪儿找对应文件。

#### 使用源集构建

使用不同源集构建构建变体。**这些源集并不是互斥的，一个构建变体适用多个源集**。**（分类之间的关系）**

Gradle 会查看下面目录并赋予以下优先级顺序： 

1. `src/demoDebug/`*（构建变体源集）*
2. `src/debug/`*（构建类型源集）*
3. `src/demo/`*（产品风味源集）*
4. `src/main/`*（主源集）*

> 说明：
>
> - 构建类型和产品风味的优先级与它们在构建变体命名时的顺序相反。
> - 同一java类出现于多个源集会导致"duplicate class"错误，其他的将会进行合并，产生冲突以上述优先级顺序为准。
> - 如果您组合多个产品风味，**产品风味之间的优先级将由它们所属的风味维度决定**。在列示具有 android.flavorDimensions 属性的风味维度时，所列示的第一个风味维度中的产品风味比第二个维度中的产品风味拥有更高的优先级，以此类推。此外，与属于各个产品风味的源集相比，您为**产品风味组合创建的源集拥有更高的优先级**。 
> - 最后，在构建 APK 时，Gradle 会为随**库模块依赖项包含的资源和清单分配最低的优先级**。

标签：源集优先级

##配置依赖项

依赖配置项：compile、apk、provided；

Gradle3.0引入新的依赖配置：implementation、api、compileOnly、runtimeOnly

依赖配置项的使用场景

结合构造变体的依赖配置项







