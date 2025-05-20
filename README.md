# iOS开发与逆向工程学习仓库

📚 本仓库汇集了iOS应用开发与逆向工程的学习资料、示例代码及实战项目，涵盖Objective-C核心语法、内存管理、协议委托、Runtime机制等关键知识点，并包含相应的项目演示。

---

## 项目结构

iOS-Study/
├── Demo/                          # 基础示例项目
│   ├── Person.[h/m]               # 人员基类（属性、方法、类别扩展）
│   ├── Student.[h/m]              # 学生子类（任务管理、块操作）
│   ├── TaskManager.[h/m]          # 任务调度器（协议与委托）
│   └── main.m                     # 程序入口
├── TaskSystem/                    # 任务管理系统项目
│   ├── TaskHandler.[h/m]          # 任务处理器（数据源实现）
│   ├── TaskProtocol.h             # 协议定义
│   ├── TaskProcessor.[h/m]        # 任务处理器（状态管理）
│   └── main.m                     # 命令行交互入口
└── Docs/                          # 学习文档
    └── iOS应用开发与逆向工程指南.md
---

## 项目说明

### 1. 基础示例项目（Demo）

#### 功能概述
- **类继承与扩展**：`Student`继承`Person`，通过类别`Person+Extension`添加游戏方法。
- **协议与委托**：`TaskManager`通过`TaskManagerDelegate`和`TaskManagerDataSource`管理任务执行流程。
- **块(Block)操作**：在`Student`的`study`方法中演示块的定义与调用。

#### 核心代码示例
```objective-c
// Block定义与使用
void (^block1)(int) = ^(int num) {
    NSLog(@"block1 num = %d", num);
};
block1(1);

// 协议方法实现（Student中）
- (NSUInteger)taskCounts {
    return sizeof(_allTask)/sizeof(MyTask *);
}
```

#### 运行结果
```plaintext
name=Tom sex=men
block1 num = 1
block2(2) retval = 3
学习任务将要执行
正在执行学习任务
学习任务执行完毕
```

---

### 2. 任务管理系统（TaskSystem）

#### 功能概述
- **命令行交互**：支持`add`、`switch`、`show-all`等指令管理任务。
- **状态持久化**：使用`NSMutableArray`存储任务列表及完成状态。
- **协议分层**：通过`TaskDataProvider`和`TaskOperationDelegate`解耦数据与操作。

#### 核心代码示例
```objective-c
// 添加新任务（TaskProcessor.m）
- (void)insertNewTask:(NSString *)taskName {
    [self.taskList addObject:taskName];
    [self.completionStatus addObject:@NO];
    NSLog(@"[操作日志] 成功添加：%@", taskName);
}
```

#### 运行示例
```plaintext
🌟 任务管理系统已启动（输入 help 查看指引）

➜ create 写单元测试
[操作日志] 成功添加：写单元测试

➜ show-all
当前事项清单：
01. ◻️ 复习iOS基础
02. ◻️ 调试项目代码
03. ◻️ 写单元测试

➜ switch 3
[操作日志] 写单元测试 状态变更为：已完成
01. ◻️ 复习iOS基础
02. ◻️ 调试项目代码
03. ✔️ 写单元测试
```

---

## 关键技术与知识点

### 📌 Objective-C核心
- **内存管理**：`strong`/`weak`/`copy`修饰符使用场景
- **协议与委托**：`@protocol`定义接口规范，实现松耦合
- **Runtime机制**：通过`Method Swizzling`动态替换方法

### 📌 逆向工程基础
- **静态分析**：使用Hopper/IDA解析二进制文件
- **动态调试**：LLDB指令断点、寄存器监控
- **Hook技术**：通过`fishhook`劫持C函数

---

## 快速开始

### 环境要求
- macOS系统 + Xcode 12+
- 命令行工具（如Terminal）

### 编译与运行
```bash
# 编译Demo项目
cd Demo
clang -framework Foundation *.m -o demo
./demo

# 运行任务管理系统
cd TaskSystem
clang -framework Foundation *.m -o tasks
./tasks
```

---

## 贡献与许可
- **贡献指南**：欢迎提交Issue或PR，需附带详细注释和测试用例。
- **许可证**：MIT License，详见仓库根目录LICENSE文件。

🚀 通过本仓库代码，你将系统掌握iOS开发与逆向工程的核心技能，持续更新中！

