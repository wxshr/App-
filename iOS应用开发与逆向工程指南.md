# iOS应用开发与逆向工程指南

---

## 目录
1. [iOS逆向](#iOS逆向)  
2. [认识Xcode](#认识xcode)  
3. [Objective-C基础](#objective-c基础)  
4. [常用组件](#常用组件)  
5. [网络请求](#网络请求)  
6. [加密与解密](#加密与解密)  
7. [App生命周期](#app生命周期)  
8. [Runtime](#runtime)  
9. [内存管理](#内存管理)  
10. [打包](#打包)  

### 什么是iOS逆向？
从iOS应用的功能或行为入手，通过**脱壳、运行时分析、静态分析、动态调试、Hook、注入**等技术手段，推导出目标文件的结构、流程、算法及代码的过程。

### 逆向工程的价值
- 深入理解应用内部机制  
- 发现安全漏洞并加强防御  
- 创新功能扩展（如开发插件）  
- 学习隐藏的算法与设计逻辑  

---

## 认识Xcode
### Xcode简介
- 苹果官方集成开发工具，仅支持macOS系统。  
- 支持开发iOS、macOS、watchOS等应用。  

### 基本使用
1. **创建项目**：  
   - 打开Xcode → `File` → `New` → `Project` → 选择模板（如Single View App）。  
2. **项目结构**：  
   - `.h`文件：头文件，声明类与公共方法。  
   - `.m`文件：实现文件，包含具体逻辑。  

---

## Objective-C基础
### 数据类型
- **基本类型**：`int`, `bool`, `float`, `double`, `char`等。  
- **指针类型**：`void *`, `id`（通用对象指针）。  
- **常用对象类型**：  
  ```objectivec
  NSString *str = @"Hello";
  NSArray *array = @[@"A", @"B"];
  NSDictionary *dict = @{@"key": @"value"};

### 流程控制
- `if-else`, `switch-case`, `for`, `while`, `do-while`。  
- **快速枚举**：  
  ```objectivec
  for (NSString *item in array) {
      NSLog(@"%@", item);
  }
  ```

### 类与函数
- **类定义**：  
  ```objectivec
  @interface MyClass : NSObject
  @property (nonatomic, strong) NSString *name;
  - (void)printName;
  @end
  ```
- **函数类型**：  
  - `+`类方法：`+ (void)staticMethod;`  
  - `-`实例方法：`- (void)instanceMethod;`  

### Block语法
- **定义与使用**：  
  ```objectivec
  void (^myBlock)(int) = ^(int num) {
      NSLog(@"Number: %d", num);
  };
  myBlock(10);
  ```

---

## 常用组件
### UIKit核心组件
- `UIWindow`：应用的窗口容器，单窗口模式。  
- `UIViewController`：视图控制器，管理页面生命周期。  
- `UILabel`、`UITextField`、`UIButton`：基础UI控件。  
- `UINavigationController`：导航控制器，管理页面栈。  

### 定时器（NSTimer）
```objectivec
NSTimer *timer = [NSTimer timerWithTimeInterval:1.0 repeats:YES block:^(NSTimer *timer) {
    NSLog(@"Tick");
}];
[[NSRunLoop currentRunLoop] addTimer:timer forMode:NSDefaultRunLoopMode];
```

### 通知（NSNotification）
- **注册通知**：  
  ```objectivec
  [[NSNotificationCenter defaultCenter] addObserver:self 
                                        selector:@selector(handleNotification:) 
                                            name:@"MyNotification" 
                                          object:nil];
  ```
- **发送通知**：  
  ```objectivec
  [[NSNotificationCenter defaultCenter] postNotificationName:@"MyNotification" 
                                                    object:nil 
                                                  userInfo:@{@"key": @"value"}];
  ```

---

## 网络请求
### NSURLSession
```objectivec
NSURL *url = [NSURL URLWithString:@"https://api.example.com/data"];
NSURLRequest *request = [NSURLRequest requestWithURL:url];
NSURLSessionDataTask *task = [[NSURLSession sharedSession] dataTaskWithRequest:request 
    completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
        // 处理响应数据
}];
[task resume];
```

### AFNetworking（第三方库）
```objectivec
AFHTTPSessionManager *manager = [AFHTTPSessionManager manager];
[manager GET:@"https://api.example.com/data" parameters:nil progress:nil 
    success:^(NSURLSessionDataTask *task, id responseObject) {
        // 请求成功
    } failure:^(NSURLSessionDataTask *task, NSError *error) {
        // 请求失败
}];
```

---

## 加密与解密
### MD5加密
```objectivec
#import <CommonCrypto/CommonDigest.h>
- (NSString *)md5Encrypt:(NSString *)input {
    const char *cStr = [input UTF8String];
    unsigned char digest[CC_MD5_DIGEST_LENGTH];
    CC_MD5(cStr, (CC_LONG)strlen(cStr), digest);
    NSMutableString *output = [NSMutableString stringWithCapacity:CC_MD5_DIGEST_LENGTH * 2];
    for (int i=0; i<CC_MD5_DIGEST_LENGTH; i++) {
        [output appendFormat:@"%02x", digest[i]];
    }
    return output;
}
```

### AES加解密
```objectivec
// 加密
NSData *encryptedData = [self aesEncrypt:data key:@"key" iv:@"iv"];
// 解密
NSData *decryptedData = [self aesDecrypt:encryptedData key:@"key" iv:@"iv"];
```

---

## App生命周期
### 入口函数
```objectivec
int main(int argc, char * argv[]) {
    @autoreleasepool {
        return UIApplicationMain(argc, argv, nil, NSStringFromClass([AppDelegate class]));
    }
}
```

### UIViewController生命周期
1. `viewDidLoad`：视图加载完成。  
2. `viewWillAppear`：视图即将显示。  
3. `viewDidAppear`：视图已显示。  
4. `viewWillDisappear`：视图即将消失。  
5. `viewDidDisappear`：视图已消失。  

---

## Runtime
### KVC（键值编码）
```objectivec
[object setValue:@"value" forKey:@"propertyName"];
NSString *value = [object valueForKey:@"propertyName"];
```

### KVO（键值观察）
```objectivec
// 添加观察者
[object addObserver:self 
         forKeyPath:@"property" 
            options:NSKeyValueObservingOptionNew 
            context:nil];
// 回调方法
- (void)observeValueForKeyPath:(NSString *)keyPath 
                      ofObject:(id)object 
                        change:(NSDictionary *)change 
                       context:(void *)context {
    // 处理变化
}
```

### Method Swizzling
```objectivec
Method originalMethod = class_getInstanceMethod([self class], @selector(originalMethod));
Method swizzledMethod = class_getInstanceMethod([self class], @selector(swizzledMethod));
method_exchangeImplementations(originalMethod, swizzledMethod);
```

---

## 内存管理
### 引用计数机制
- `retain`：引用计数+1。  
- `release`：引用计数-1。  
- `autorelease`：延迟释放。  

### 自动释放池
```objectivec
@autoreleasepool {
    // 临时对象在此作用域结束后自动释放
}
```

### 弱引用
```objectivec
__weak typeof(self) weakSelf = self;
```

---

## 打包
### 有证书打包
1. 点击Xcode菜单栏 `Product` → `Archive`。  
2. 选择证书并导出`.ipa`文件。  

### 无证书打包（仅调试）
1. 修改`Scheme`的`Build Configuration`为`Debug`。  
2. 编译后，在Finder中找到`.app`文件。  
3. 创建`Payload`文件夹，将`.app`放入其中。  
4. 压缩为`Payload.zip`并重命名为`.ipa`。  

---

