---
title: YYModel, MJExtension, JSONModel 对比
tags: [JSON, MODEL, 模型框架]
categories: iOS
---

JSON 模型

<!-- more -->

### 使用区别

> JSONModel 要求所有模型类必须继承自 JSONModel 基类
> YYModel 和 MJExtension 不需要你的模型类继承任何特殊基类，毫无污染，毫无侵入性

### 量级上

> YYModel 和 MJExtension 都是超轻量级框架
> YYModel 是一个更轻量级高性能 iOS/OSX 模型转换框架,模型转换性能接近手写解析代码,比 MJExtension 和 JSONModel 性能更高.

### 字典和模型之间互相转换

> MJExtension 支持:
> Plist --> Model Array

```objectivec
模型数组 = [模型类名 objectArrayWithFilename:@"文件名.plist"];
```

> JSON Array --> Model Array

```objectivec
NSArray *modelArray = [模型类名 objectArrayWithKeyValuesArray:字段数组];
```

> JSONString --> Model Array
> Model Array --> JSON Array
> YYModel 没有看到对字典数组和模型数组相互转换的支持

### 容器类属性

> YYModel 需要在 @implementation 和 @end 之间 写上

```objectivec
+ (NSDictionary *)modelContainerPropertyGenericClass {
    return @{@"arrayName" : [模型类名 class]};
}
```

> MJExtension 需要在 @implementation 和 @end 之间 写上

```objectivec
+ (NSDictionary *)objectClassInArray {
    return @{@"arrayName" : [模型类名 class]};
}
```

`MJExtension可以配合ESJsonFormat模型插件使用,插件自动实现容器方法.YYModel也可以配合ESJsonFormat模型插件使用,需要修改容器实现的方法名.`

> JSONModel 是最简单的,只需要参照以下写法即可

```objectivec
@property (nonatomic) NSArray <ProductModel> *products;
```

### 归档和解档

> 三者性能相当
> MJExtension 只需要一行代码调用写好的宏 MJExtensionCodingImplementation 就可以实现
> YYModel 需要遵守 NSCoding 协议,并重写下面方法

```objectivec
- (void)encodeWithCoder:(NSCoder *)aCoder { [self yy_modelEncodeWithCoder:aCoder]; }
- (id)initWithCoder:(NSCoder *)aDecoder { self = [super init]; return [self yy_modelInitWithCoder:aDecoder]; }
```

### 日志

> MJExtension 只需要在@implementation 和 @end 之间写上宏 MJLogAllIvrs,就能解决调试时，打印模型，只打印出内存地址的问题
> YYModel 需要重写 description 方法

```objectivec
- (NSString *)description { return [self yy_modelDescription]; }
```

### 模型中的属性名和字典中的 key 不相同(或者需要多级映射)

> YYModel 需要在 @implementation 和 @end 之间 写上

```objectivec
+ (NSDictionary *)modelCustomPropertyMapper {
    return @{@"name" : @"n",
             @"page" : @"p",
             @"desc" : @"ext.desc",
             @"bookID" : @[@"id",@"ID",@"book_id"]
             };
}
```

> MJExtension 需要在转换代码之前实现

```objectivec
[Student mj_setupReplacedKeyFromPropertyName:^NSDictionary *{
    return @{
               @"ID" : @"id",
               @"desc" : @"desciption",
               @"oldName" : @"name.oldName",
               @"nowName" : @"name.newName",
               @"nameChangedTime" : @"name.info[1].nameChangedTime",
               @"bag" : @"other.bag"
           };
}];
```
