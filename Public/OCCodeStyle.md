### 待补充
````
1. 使用NSNotificationCenter的object字段时请注意：如果object是字符串对象，请保证接收方和发送方使用的是同一个类型对象，因为方法内部是通过 == 判断而不是hash判断，例如以下的通知是有问题的： 
```
[NSNotificationCenter.defaultCenter addObserver:self selector:@selector(test:) name:@"testName" object:self.bookName];
[NSNotificationCenter.defaultCenter postNotificationName:@"testName" object:self.bookName];

假设self.bookName的值都是@"34"，也有可能收不到通知，因为NSString是一个类簇对象，它底下拥有 `__NSCFConstantString` 、`__NSCFString`、`NSTaggedPointerString`甚至更多的子类对象，虽然它们的值都是34，但是它们并不相等，这会导致收不到通知
```

1. 不要实现一些现在还不需要的功能，例如 `m_dynamicHeight`，等需要它的时候再去实现
2. 新增一个功能时问一下自己这个功能是解决现在的问题还是未来的问题？如果是未来的问题那它是必要的吗？如果不是百分百必要请在未来实现它，如果是百分百必要的请注意考虑这个功能所有的情况和细节，而不是开发一个现在用不到而且漏洞百出的功能。
3. 不要过度的去实现自动化，有时候让开发者知道他现在在干什么比自动帮他干什么要更好，例如 `m_dynamicHeight`，本意是想在宽度发生变化时自动更新高度，但是不熟悉的开发者看到代码时会很懵，这里为什么会自动变化高度？明明我没设置它的高度。
4. 重写父类方法请调用super，哪怕super的实现是空也尽量写上，这样能让别人看到这段代码时就知道是从父类继承来的，除非父类的实现是不想要的除外。
5. @property声明同一类别放在一起，不同类别换2行写，开头和末尾都要有一行空格
6. 尽量提供一下全能初始化方法，然后其他所有的方法都调用全能初始化方法，这样以后要改默认行为的话只需要修改全能初始化方法就行了
7. @property声明，数据类型类型的放最上面，然后是UI控件
8. 一般不要使用成员变量，而是使用属性，成员变量在Block内部使用时编译器会自动加上self->，如果不注意就会形成相互引用，而且在Block内部要使用成员变量需要在外面声明weakSelf，然后里面又要使用strongSelf并且判空调用，另外成品变量也不方便使用weak等关键字修饰。
9. 尽量规避代码问题，但是BUG不可能完全没有，那么就让BUG尽量容易排查，例如 nameString 就不好排查，而self->nameString就比较好排查
````


### 前言
&nbsp;&nbsp;&nbsp; 适当的代码规范和标准不是消灭代码内容的创造性、优雅性，而是限制过度个性化，以一种普遍认可的统一方式一起做事，进而提高工作效率，降低沟通成本。代码的字里行间流淌着的是软件的血液，质量的提升是尽可能少踩坑、杜绝踩重复的坑，切实提升系统稳定性，码出质量(`摘抄自《阿里巴巴Java代码规范》`)。

&nbsp;&nbsp;&nbsp; 在整个代码库中一致地使用一种风格可以让工程师专注于其他(更重要的)问题。一致性还可以实现更好的自动化，因为一致的代码允许更有效地开发和操作格式化或重构代码的工具。

&nbsp;&nbsp;&nbsp;&nbsp;根据约束力度，暂时把规范约定为2个等级，分别是 __[必须]__ 和 __[建议]__。

## (一)命名规范

#### 1. 通用命名规范
*
    ```
    Tips:
    所有的命名都应该遵循3个基本原则，即“清晰性”、“一致性”、“不要自我指涉”。
    ```

* __[必须]__ 清晰性：好的命名应该是能自我描述的。

    ```
    正例:
    removeObject:、
    [string stringByReplacingOccurrencesOfString:@"1" withString:@"2"]
    反例:
    remove:(要删除什么？)、
    string.replace("1", "2")(是将"1"替换成"2"还是将"2"替换成"1"？是替换首个匹配的字符还是所有匹配的字符?)
    ```

* __[必须]__ 一致性：命名应该和上下文乃至全局保持一致性，相同类型或者具有相同作用的变量的命名方式应该相同或类似。

    ```
    正例:
    @property (readonly) NSUInteger count;
    // NSDictionary、NSArray、NSSet这些集合类都是用count来表示集合数量，
    这就是命名的一致性。
    ```

* __[必须]__ 禁止自我指涉：命名不要自我指涉(通知、掩码常量等除外)。

    ```
    正例:
    NSString
    反例:
    NSStringObject
    ```

* __[必须]__ 杜绝过度缩写，国际通用缩写名称除外(例如ATM、GPS等)。

    ```
    Tips:
    你的代码可能会被任何人阅读，
    而阅读的人来自不同的地方接受不同的教育不同的文化，
    他们不一定会明白你的意思。
    正例:
    destinationSelection、setBackgroundColor
    反例:
    destSel、setBgColor
    ```
    
* __[必须]__ 严禁自创缩写(例如把button缩写为btn)。

* __[必须]__ 杜绝无意义的拼音，国际通用名和地名人名除外。

    ```
    正例:
    alibaba、hangzhou
    反例:
    DaZhePromotion(打折)
    ```
    
* __[必须]__ 代码和注释都要避免使用任何语言的种族歧视性词语。

    ```
    正例:
    secondary
    反例:
    slave
    ```
    
* __[必须]__ 所有名称禁止以new、alloc、copy、mutableCopy等关键字开始。

* __[必须]__ 命名要尽可能清晰并简洁，如果两者不能兼得，则以清晰为主。

    ```
    正例:
    insertObject:atIndex:
    反例:
    insert:at:(插入什么？at代表什么?)
    ```

* __[必须]__ 类名、协议名、常量名、枚举名等全局名称需要添加前缀，前缀需要大于2个字符且全部大写。

    ```
    Tips: 系统保留任意两个字符作为前缀的使用权，
    包括但不限于NS、UI、CG、CF、CA、WK、MK、CI、NC，
    前缀若等于2个字符可以考虑添加_。
    正例:
    ZT_LoginViewController
    反例:
    ZTLoginViewController
    ```

* __[必须]__ 类名、协议名、常量名、枚举名等全局命名需遵循首字母大写的驼峰命名方式(首个单词是HTTP这种特殊词除外)。
    
    ```
    正例:
    WXYZLoginViewController
    反例:
    WXYZloginViewController
    ```

* __[必须]__ 函数名也应该遵循首字母大写的驼峰命名方式

    ```
    正例:
    static void AddTableEntry(NSString *tableEntry);
    static BOOL DeleteFile(const char *filename);
    ```
    
* __[必须]__ 非静态函数或某个类的函数名称前应添加前缀，如果函数和某个类相关联则应该在函数前面添加类名前缀。

    ```
    正例:
    extern NSTimeZone *GTMGetDefaultTimeZone(void);
    extern NSString *GTMGetURLScheme(NSURL *URL);
    ```

* __[必须]__ 方法名、属性名等非全局名称应遵循首字母小写的驼峰命名方式(首个单词是HTTP这种特殊词除外)。

    ```
    正例:
    nameLabel
    反例:
    NameLabel
    ```

* __[必须]__ 成员变量需要以_开头。

    ```
    正例:
    _nameString;
    反例:
    nameString;
    ```

* __[必须]__ 在给常量或变量命名时，请将表示类型的名词放在词尾，以提高辨识度。

    ```
    正例:
    nameLabel、nameString
    反例:
    name(name是字符串还是什么?)
    ```

* __[建议]__ 如果模块、接口、类、方法使用了模式，在命名时尽量体现出具体模式。

    ```
    正例:
    OrderFactory、LoginProxy
    ```

* __[建议]__ 建议给临时变量名称添加前缀，以提高辨识度(特殊情况除外，例如for循环里面的i)。

    ```
    正例:
    t_label(t在这里表示temp)
    反例:
    label
    ```


#### 2. 类命名规范

* __[必须]__ 类名由"前缀+类的名称+类的类型"3个部分组成，'前缀'必须大于2个字符且全部大写(如果等于2个字符可以在末尾添加_)，类名需遵循首字母大写的驼峰式命名方式，'类的名称'要能表达出该类的功能，类的类型必须使用全称，严禁使用缩写(例如使用vc代替viewController等)。
    
    ```
    正例:
    WXYZ_LoginViewControler
    前缀：WXYZ_，
    类的名称：Login(表示该类跟登录相关)，
    类的类型：ViewController(表示该类是一个视图控制器而不是View)。
    ```


#### 3. 方法命名规范

* __[必须]__ 所有方法名称禁止使用_开始。

    ```
    Tips: 系统的私有方法会使用_开头，为了避免不小心覆盖系统方法，严禁使用_开头给方法命名。
    ```

* __[必须]__ 私有方法需要增加前缀，前缀需要保持唯一性(例如p_)。

    ```
    Tips: 给私有方法加前缀有2个好处: 
    1. 增加辨识度，提高代码可读性。
    2. 避免自己的方法无意间覆盖了系统/框架同名的私有方法。
    ```

* __[必须]__ 如果方法返回接收者的某个属性值，那么请直接使用属性名作为方法名。

    ```
    正例:
    - (CGSize)cellSize;
    反例:
    - (CGSize)getCellSize;
    ```

* __[必须]__ 如果方法间接返回一个或多个值，那么请用"getXXX"命名，这种命名只适用于返回多个数据项的情况。

    ```
    正例:
    - (void)getCachedResponseForDataTask:(NSURLSessionDataTask *)dataTask 
                       completionHandler:(void (^) (NSCachedURLResponse * __nullable cachedResponse))completionHandler;
    ```

* __[必须]__ 方法的每个参数前都必须添加关键字。

    ```
    正例:
    - (void)sendAction:(SEL)aSelector toObject:(id)anObject forAllCells:(BOOL)flag;
    反例:
    - (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
    ```
    
* __[必须]__ 如果方法是由通知触发的，那么请添加Notification关键字作为后缀。

    ```
    正例:
    - (void)appDidBecomeActiveNotification;
    反例:
    - (void)appDidBecomeActive;
    ```

* __[建议]__ 参数之前的单词尽量能描述参数的意义。

    ```
    正例:
    - (id)viewWithTag:(NSInteger)aTag;
    反例:
    - (id)taggedView:(int)aTag;
    ```

* __[建议]__ 请不要使用“and”连接接收者属性，尽管and读起来还算顺口，但随着你创建的方法参数的增加，这将会带来一系列的问题。

    ```
    正例:
    - (int)runModalForDirectory:(NSString *)path file:(NSString *)name types:(NSArray *)fileTypes;
    反例:
    - (int)runModalForDirectory:(NSString *)path andFile:(NSString *)name andTypes:(NSArray *)fileTypes;
    ```

* __[建议]__ 如果方法描述了两个独立的动作，则可以使用"and"连接起来。

    ```
    正例:
    - (BOOL)openFile:(NSString *)fullPath withApplication:(NSString *)appName andDeactivate:(BOOL)flag;
    ```

#### 4. Protocol命名规范

* __[必须]__ Protocol中的方法命名以触发消息的对象名开头，省略类名前缀并首字母小写，如果它没有关联任何类则可以忽略这个规则。

    ```
    正例:
    - (BOOL)tableView:(NSTableView *)tableView shouldSelectRow:(int)row;
    ```

* __[必须]__ 除非Protocol方法只有一个参数，否则冒号需紧跟在类名后面。

    ```
    正例:
    - (BOOL)tableView:(NSTableView *)tableView shouldSelectRow:(int)row;
    - (BOOL)applicationOpenUntitledFile:(NSApplication *)sender;
    ```

#### 5. Category命名规范

* __[必须]__ 分类名也要和类名一样添加前缀。

    ```
    正例:
    UIView (YYAdd)
    反例:
    UIView (Add)
    ```

* __[必须]__ 分类中声明的方法名必须要添加前缀，防止覆盖系统的私有方法。

    ```
    正例:
    mp_start
    反例:
    start
    ```

* __[建议]__ 分类中尽量不要声明属性，能挪尽量挪到主类中声明。

    ```
    Tips: 尽管从技术上来讲可以在分类中声明属性，但是这么做需要格外小心，
    因为它很容易出现内存上或其他一些问题，而且出现问题很难排查。
    ```

* __[建议]__ 如果一个类比较复杂，那么建议使用分类组织代码(可以参考系统的UIView)。


#### 6. Notification命名规范

* __[必须]__ Notification的命名风格由"类名前缀" + "通知事件名称" + "Notification"3个部分组成。

    ```
    正例:
    UIApplicationDidBecomeActiveNotification
    UIApplication表示该通知属于谁，
    DidBecomeActive表示该通知的作用，
    Notification表示它是一个通知。
    ```

* __[建议]__ 如果一个类声明了delegate属性，通常情况下，这个类的delegate对象应该可以通过实现的delegate方法收到大部分通知消息。

    ```
    Tips: 
    例如applicationDidBecomeActive:代理方法和NSApplicationDidBecomeActiveNotification通知
    (注意这里的方法名和通知名遵守了命名规范的基本原则"一致性")。
    ```


#### 7. 常量命名规范

* __[必须]__ 如果常量局限于某"编译单元"之内，通常在前面加小写字母k作为前缀，若常量在全局可见，通常以类名作为前缀，然后采用首字母大写驼峰命名方式。

     ```
     正例:
     // 局部可见
     const CGFloat kAnimationDuration = 2.0;
     // 全局可见
     const CGFloat UIActivityIndicatorViewAnimationDuration = 2.0;
     ```
  

#### 8. Exception命名规范

* __[必须]__ 命令规范和Notification一样，把后缀改为"Exception"即可。


#### 9. 文件命名规范

* __[必须]__ 文件名全部小写。

* __[必须]__ 采用_连接单词，不要使用驼峰式命名。

* __[必须]__ 命名的风格："模块_属性描述"，可根据项目适当增加描述。

    ```
    正例:
    public_back@2x.png
    ```


## (二)编码规范

#### 1. 通用编码规范

* __[必须]__ 如果有使用到CF(Core Foundation)等框架时，或者是在iOS10及以下系统使用通知或KVO时，切记在dealloc方法中释放对象和移除通知或监听。

* __[必须]__ 在dealloc方法内禁止将self传递出去，如果self被retain，到下个runloop周期再释放则会多次释放导致crash。

    ```
    反例:
    - (void)dealloc {
        [self unsafeMethod:self];
    }
    ```

* __[必须]__ 禁止使用过时的方法或类，应该及时去了解和使用新方法或类。

    ```
    Tips:
    对于过时的方法或类，大都是因为其自身有缺陷或BUG，
    使用新方法前建议了解一下旧方法/类废弃的原因。
    ```

* __[必须]__ 对剪切板的读取操作必须放在子线程中进行，因为用户可能在Mac上复制大量数据然后通过iCloud同步到iPhone上。

* __[必须]__ if、else、for、while、case等后面必须要有{}(除非后面是简单的return类型语句，例如`if (xxx) return;`)。

* __[必须]__ 当方法可能会提前return时，需要注意对象的释放问题，避免内存泄漏。

    ```
    反例:
    CFArrayRef arrayRef = (__bridge CFArrayRef)array;
    
    if (x == YES) return;
     
    CFRelease(arrayRef);
    
    如果if条件成立那么arrayRef对象就会内存泄漏。
    ```

* __[必须]__ 当使用@try处理异常时，需要注意对象的释放问题，避免内存泄漏。

    ```
    反例:
    @try {
    CFArrayRef arrayRef = (__bridge CFArrayRef)array;
        
    do some thing……
    
    CFRelease(arrayRef);
            
    } @catch (NSException *exception) {
        
    }
    
    如果do some thing……出现异常的话那么arrayRef就会出现内存泄漏。
    ```

* __[必须]__ 声明常量请使用const类型声明，禁止使用#define。

    ```
    Tips: 
    宏定义声明常量的缺点:
    1. 宏定义只是简单的替换，缺少编译检查，运行期可能会出现溢出或数据错误等问题。
    2. 宏定义缺少类型，不方便编写文档用例。
    3. 宏定义可能会被不小心替换。
    4. 宏定义无法编写注释。
    
    反例:
    #define kTime @"10"
        
    if (NO) {
    #define kTime @"20"
    }
        
    NSLog(@"time = %@", kTime);
    
    即使if永远不会执行，但是编译器也会将kTime替换为@"20"
    ```

* __[必须]__ 只在必要的时刻使用懒加载。

    ```
    只在以下三种情况下才能使用懒加载:
    1. 对象的创建需要依赖其他对象
    2. 对象可能被使用，也可能不被使用
    3. 对象创建比较消耗性能
    ```

* __[必须]__ 使用一目运算符时左右两边不要有空格。

    ```
    正例:
    i++、++i、
    反例:
    i ++、++ i
    ```

* __[必须]__ 使用二目、三目运算符时左右两边必须有且仅有一个空格。

    ```
    正例:
    1 + 2
    反例:
    1+2
    ```

* __[必须]__ 采用4个空格缩进，如果要使用Tab字符，请将1个Tab设置成4个空格。

* __[必须]__ 使用NSUserDefaults存储数据时禁止调用synchronize方法，因为系统会在合适的时机将数据保存到本地(即使程序闪退等极端情况)。

* __[必须]__ 添加到集合中的对象应该是不可变的，或者在加入之后其哈希值是不可变的。

    ```
    反例:
    NSMutableSet *sets = [NSMutableSet set];
    NSMutableString *string1 = [NSMutableString stringWithString:@"1"];
    [sets addObject:string1];
    [sets addObject:@"12"];
        
    [string1 appendString:@"2"];
    
    当 [string1 appendString:@"2"] 执行完以后sets对象内会包含2个@"12"。
    ```
    
* __[必须]__ 不可变对象请使用copy修饰，如果重写使用copy修饰的set方法，请注意调用copy方法。

* __[必须]__ 请使用CGRectGet函数获取Frame的各种值，而不是通过frame.的方式获取。

    ```
    Tips: 
    CGRect t_frame = CGRectMake(-10, -10, -10, -10);
    当一个view的frame设置成t_frame后，其坐标会隐式的转换为CGRectMake(-20, -20, 10, 10)，因为宽高不可能出现负值；这时通过t_frame.的方式获取的值都是错误的，而CGRectGet会自动帮您处理这些隐式转换。
    
    正例:
    CGRectGetWidth(frame)、CGRectGetMinX(frame)、CGRectGetMaxX(frame)
    反例:
    frame.size.width、frame.origin.x、frame.size.width + frame.origin.x
    ```

* __[建议]__ 在写一些工具类方法时，建议使用内联函数或全局函数而不是宏定义。

    ```
    Tips:
    函数不通过对象调用，所以不会走OC的消息转发流程，效率高于方法调用，
    而且函数会有返回值和参数类型以及参数检查，这些都是宏定义没有的。
    
    正例:
    UIKIT_STATIC_INLINE NSString * kNSStringFromInteger(NSInteger a) {
        return [NSString stringWithFormat:@"%zd", a];
    }
    反例:
    #define kNSStringFromInteger(a) [NSString stringWithFormat:@"%zd", a]
    ```

* __[建议]__ 单行字符数建议不超过150个，超过请换行(空格除外)，换行时遵循如下原则:

    ```
    Tips:
    1. 第二行相对第一行缩进4个空格，从第三行起不再继续缩进。
    2. 运算符与下文一起换行。
    3. 方法调用的点符号与下文一起换行。
    
    正例:
    - (void)setImageWithURL:(nullable NSURL *)imageURL
                placeholder:(nullable UIImage *)placeholder
                    options:(YYWebImageOptions)options
                   progress:(nullable YYWebImageProgressBlock)progress
                   ransform:(nullable YYWebImageTransformBlock)transform
                 completion:(nullable YYWebImageCompletionBlock)completion;
    ```

* __[建议]__ 对于一些体积小的信息，不要频繁的存储到本地，可以使用NSUserDefaults。它会在合适的时机存储到本地，这避免了频繁的写入操作，而且在某些极端情况下它也能保证数据存储到本地(例如程序闪退等情况)。

* __[建议]__ 在多线程环境下谨慎使用可变集合，必要时候可以采用加锁或GCD的同步线程进行保护，或者在访问可变集合时先将其copy为不可变对象然后再对其访问。

* __[建议]__ 头文件中尽量不要声明成员变量而是使用属性代替。

* __[建议]__ 头文件中的属性尽量声明为只读，可以在实现文件中再将属性声明为可读可写。

    ```
    正例:
    @interface WXYZModel : NSObject
    
    @property (nonatomic, readonly) NSString *name;
    
    @end
    
    @interface WXYZModel ()
    
    @property (nonatomic, strong) NSString *name;
    
    @end
    ```

* __[建议]__ 不要使用一个类去维护多个类的内容，例如一个常量类维护所有的常量类，要按功能进行归类，分开维护。

    ```
    Tips: 大而全的类，杂乱无章，使用查找功能才能定位到具体位置，不利于理解也不利于维护。
    
    正例:
    缓存相关常量类放在CacheCosts下，系统配置相关常量类放在SystemConfigConsts下。
    ```

* __[建议]__ 如果大括号内为空，则简洁的写成{}就行。

* __[建议]__ 没有必要增加多余空格来使上下代码的等号对齐。

    ```
    正例:
    int a1 = 1;
    long a2 = 3;
    NSString *a3 = @"";
    
    反例:
    int a1       = 1;
    long a2      = 3;
    NSString *a3 = @"";
    ```

* __[建议]__ 少用if else，可以使用 if return 替换，if 嵌套最好不超过5层。

    ```
    正例:
    if (x == 1) {
    ……
    return;
    }
    
    if (x == 2) {
    ……
    return;
    }
    
    反例:
    if (x == 1) {
    ……
    } else if (x == 2) {
    ……
    }
    ```

* __[建议]__ 尽量避免采用取反逻辑运算符，因为取反逻辑不利于快速理解。

    ```
    正例:
    if (array == nil) {
    ……
    }
    反例:
    if (!array) {
    ……
    }
    ```

* __[建议]__ 如果用到了很多协议，必要时可以把协议封装到一个单独的头文件中，这样做不仅可以减小编译时间，还能避免循环引用。

* __[建议]__ 使用switch枚举时尽量将所有枚举类型都列举出来而不使用default，这样下次增加枚举类型时如果switch没有处理编译器会有警告信息。

* __[建议]__ 尽量使用字面量语法创建对象，少用与之等价的方法。

    ```
    Tips:
    OC中的NSArray、NSString、NSDictionay、NSNumber都有与之对应的字面量语法: @[]、@""、@{}、@()；使用它们有以下优点:
    1. 简单易读，提高代码的可读性和可维护性。
    2. 使用字面量创建数组、字典时如果元素里在nil则会抛出异常，而使用arrayWithObjects:这些等价方法创建则会丢失nil后的数据，抛出异常能让你知道这里有问题及时修改防止问题在线上发生。
    
    缺点:
    1. 使用字面量创建的对象默认是不可变的，如果要创建可变对象需要进行mutableCopy操作。
    2. 不支持子类，如果你创建了一个NSString的子类，@""并不会返回你想要的子类对象。
    ```

* __[建议]__ 头文件中尽量少引用其他头文件，尽量使用@class向前声明，每次引入其他头文件时问问自己是否必须要这样做。

* __[建议]__ UI控件建议使用weak修饰而不是strong修饰。

#### 2. 类编码规范

* __[必须]__ 如果超类的某个初始化方法不适用于子类，那么子类一定要覆写超类的这个方法并解决该问题或抛出异常。

* __[建议]__ 尽量不要使用+load方法，如果必须要使用不要在方法内实现复杂逻辑或堵塞线程。

* __[建议]__ 尽量减少继承，类的继承建议不要超过3层，必要时刻可以考虑用分类、协议来代替继承。

* __[建议]__ 把一些稳定的、公共的变量或者方法抽取到父类中。子类尽量只维持父类所不具备的特性和功能。

#### 3. 方法编码规范

* __[必须]__ 禁止在init等初始化方法内部、getter、setter、dealloc或其他特殊地方使用.语法访问属性。

    ```
    Tips: 当存在继承关系时使用.语法访问会因为多态关系调用子类的实现方法，
    而如果这个时候子类还没有初始化或者已经释放了那么可能会出现一些奇怪的问题。
    
    正例:
    - (instancetype)init {
        if (self = [super init]) {
            _name = @"";
        }
    }
    反例:
    - (instancetype)init {
        if (self = [super init]) {
            self.name = @"";
        }
    }
    ```

* __[必须]__ 方法参数在定义和传入时，逗号后面必须添加一个空格。

    ```
    正例:
    method(a1, a2, a3);
    ```
    
* __[建议]__ 重写父类方法时建议调用super的同名方法，即使父类这个方法什么都没做(特殊情况除外)。

    ```
    Tips:
    这么做能让别人阅读代码时就知道这个方法是来自于父类
    ```

* __[建议]__ 单个方法的行数建议不超过80行，注释、左右大括号、空行、回车等除外。

#### 4. Block编码规范

* __[必须]__ 调用Block必须判空。

    ```
    Tips: 
    对于简单的Block可以使用三目运算进行判空处理，
    例如 !self.block ?: self.block();
    ```

* __[必须]__ 在Block内部使用外部变量时要注意相互引用的问题(不一定要在Block内引用self才会导致相互引用)。

    ```
    Tips: 
    1. 不一定在Block内使用self才会相互引用，如下情况也会造成循环引用:
    - (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
        WXYZ_TitleTableViewCell *cell = ………
        
        cell.refreshTableViewBlock = ^{
            [tableView reloadData];
        };
        
        return cell;
    }
    
    2. Block内部是否要使用weak需要看Block本身和weak的这个对象是否存在直接或间接的相互引用，若无相互引用则不需要使用weak。
    
    3. 如果Block内部使用了strong修饰了外部的weak变量，那么当使用strong指向成员变量时需要进行判空，否则会崩溃，参考以下代码:
    __weak typeof(self) weakSelf = self;
        cell.refreshTableViewBlock = ^{
            __strong typeof(weakSelf) strongSelf = weakSelf;
            if (strongSelf != nil) {
                strongSelf->_name = @"name";
            }
        };
        
    如果把(strongSelf != nil)的判断去掉那么可能会崩溃。
    ```


#### 5. 通知编码规范

* __[必须]__ 在发送通知时，请使用`userInfo`进行传值，而不是`object`。

* __[必须]__ 通知中心是以同步的方式发送请求的，所以不要在通知方法里做一些复杂的计算(特别是当它处于主线程的时候)，如果想发送异步通知可以使用`NSNotificationQueue`。

* __[建议]__ 在工程里能不用通知尽量不用通知，通知虽然灵活强大，但是如果滥用会导致工程质量下降，出现问题时也比较难排查。


#### 6. 注释编码规范

* __[必须]__ 与其绞尽脑汁写注释，不如想想怎么命名；注释是起辅助作用的，好的命名应该是能自我解释的，如果命名可以解释其作用，并且方法没有任何副作用或者注意事项，那么就不用写注释；注释应该帮助别人更快的理解该方法的使用和注意事项，如果该方法有需要注意的地方一定要在注释中体现出来。请记住：虽然注释很重要，但最好的代码都是可以自我解释的。提供一个合理的名称比使用晦涩的名称然后试图通注释来解释它们要好的多。

* __[必须]__ 当修改了方法实现时请同步修改注释内容。

* __[必须]__ 注释不要写的太冗长，要简单易读容易理解。

* __[必须]__ 注释的双斜线和内容之间有且仅有一个空格。

    ```
    正例:
    // 这是示例注释，请注意在双斜线后有一个空格
    - (void)testFunction;
    ```

* __[必须]__ 对于代码注释需谨慎，代码被注释一般有2种可能，1) 后续会恢复此段代码逻辑； 2) 永久不用；对于第1种情况需添加相应注释，如果没有注释信息难以知晓注释动机，后者建议直接删除。如果有需要可以通过代码仓库查阅历史代码。

* __[必须]__ 使用特殊注释标记时，请注明标记人和标记时间，注意及时处理这些标记。

    ```
    正例:
    /**
    * @brief 简要描述
    * @author 标明开发该类模块的作者
    */
    // FIXME: 有bug，需要修改
    - (void)testFunction;
    ```
    
* __[必须]__ 行尾注解和代码保持2个空格，如果后续行有多个注释，将它们排列起来通常会更具可读性。

    ```
    正例:
    [self doSomethingWithALongName];  // Two spaces before the comment.
    [self doSomethingShort];          // More spacing to align the comment.
    ```

* __[建议]__ 别给糟糕的代码加注释，重构它。

    ```
    Tips: 注释不能美化糟糕的代码。当企图使用注释前，先考虑是否可以通过调整结构，命名等操作，消除写注释的必要。
    ```

## (三)工程结构规范

* __[必须]__ 局部使用的常量、静态变量在@interface之前声明，如果没有@interface就在@implementation前声明。

* __[必须]__ 同一类型的属性声明放在一块，不同类型的声明用2行空格隔开，属性声明的开始和末尾都要添加一行空格。

    ```
    正例:
    @interface MineViewController ()
    
    @property (nonatomic, weak) UIView *headView;
    
    @property (nonatomic, weak) UITableView *tableView;
    
    
    @property (nonatomic, copy) NSArray *dataSourceArray;
    
    @end
    ```

* __[必须]__ 方法归类。

    ```
    #pragma mark - LifeCycle(生命周期相关的代码放在最上面)
    
    - (void)dealloc {}
    
    - (void)viewDidLoad {}
    
    - (void)viewWillAppear:(BOOL)animated {}
    
    
    #pragma mark - Public(公开方法)
    
    // code...
    // 上空一行
    // 下空两行
    
    
    #pragma mark - Private(私有方法)
    
    
    #pragma mark - Override(需要覆盖父类的方法)
    
    
    #pragma mark - Notification(通知方法)
    
    
    #pragma mark - Delegate(Delegate需要实现的方法)
    
    
    #pragma mark - Getter/Setter
    ```
    
* __[必须]__ #pragma mark与下面的代码之间不要有空行。

    ```
    正例:
    #pragma mark - Setter
    - (void)setName:(NSString *)name {
        _name = name;
    }
    ```
    
* __[建议]__ 不同逻辑、不同语义、不同业务的代码之间插入一个空行分隔开以提升可读性。

    ```
    正例:
    [self createSubviews];
    [self createTableview];
    
    [self netRequest];
    ```
    
* __[建议]__ 头文件包含顺序应该是相关头文件、操作系统头文件、语言库头文件、最后是其他依赖项的头文件，用空行分隔逻辑上不同的头文件，在每个组中，包含的内容应按首字母顺序排列。

    ```
    正例:
    #import "ProjectX/BazViewController.h"

    #import <Foundation/Foundation.h>
    
    #include <unistd.h>
    #include <vector>
    
    #include "base/basictypes.h"
    #include "base/integral_types.h"
    #include "util/math/mathutil.h"
    
    #import "ProjectX/BazModel.h"
    #import "Shared/Util/Foo.h"
    ```
    
* __[建议]__ 一个好的例子
    ```
    #import <Foundation/Foundation.h>

    @class Bar;

    /**
     * A sample class demonstrating good Objective-C style. All interfaces,
     * categories, and protocols (read: all non-trivial top-level declarations
     * in a header) MUST be commented. Comments must also be adjacent to the
     * object they're documenting.
     */
    @interface Foo : NSObject
        
    /** The retained Bar. */
    @property(nonatomic) Bar *bar;
        
    /** The current drawing attributes. */
    @property(nonatomic, copy) NSDictionary<NSString *, NSNumber *> *attributes;
        
    /**
     * Convenience creation method.
     * See -initWithBar: for details about @c bar.
     *
     * @param bar The string for fooing.
     * @return An instance of Foo.
     */
    + (instancetype)fooWithBar:(Bar *)bar;
        
    /**
     * Initializes and returns a Foo object using the provided Bar instance.
     *
     * @param bar A string that represents a thing that does a thing.
     */
    - (instancetype)initWithBar:(Bar *)bar NS_DESIGNATED_INITIALIZER;
        
    /**
     * Does some work with @c blah.
     *
     * @param blah
     * @return YES if the work was completed; NO otherwise.
     */
    - (BOOL)doWorkWithBlah:(NSString *)blah;
    
    @end
    
    
    #import "Shared/Util/Foo.h"

    @implementation Foo {
      /** The string used for displaying "hi". */
      NSString *_string;
    }
    
    + (instancetype)fooWithBar:(Bar *)bar {
      return [[self alloc] initWithBar:bar];
    }
    
    - (instancetype)init {
      // Classes with a custom designated initializer should always override
      // the superclass's designated initializer.
      return [self initWithBar:nil];
    }
    
    - (instancetype)initWithBar:(Bar *)bar {
      self = [super init];
      if (self) {
        _bar = [bar copy];
        _string = [[NSString alloc] initWithFormat:@"hi %d", 3];
        _attributes = @{
          @"color" : [UIColor blueColor],
          @"hidden" : @NO
        };
      }
      return self;
    }
    
    - (BOOL)doWorkWithBlah:(NSString *)blah {
      // Work should be done here.
      return NO;
    }
    ```

## 结语

这是一篇关于OC的代码规范，所以大部分的规范和约束都是为OC所编写的；欢迎大家提出更好的建议或改进，我会不断更新完善；最后祝大家码出开心，码出质量。

## 参考

1. [Cocoa编码规范](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
2. [阿里巴巴Java代码规范](https://github.com/alibaba/Alibaba-Java-Coding-Guidelines)
3. [Effective Objective-C 2.0  编写高质量iOS与OS X代码的52个有效方法](https://book.douban.com/subject/25829244/)
4. [Google的Objective-C风格指南](https://google.github.io/styleguide/objcguide.html)
5. 网上其他人发布的有关代码规范的文章

## 下载地址
1. [国内下载点我](https://wwa.lanzoui.com/iX926vabjsj)
2. [国际下载点我]()