---
title: "UML Class"
date: 2022-09-27T11:38:59+08:00
draft: false
tags: ["uml"]
author: "toxi"
category: ["code"]
---

```mermaid
classDiagram
class 氧气 
class 水 
class 动物 {
    - 生命
    + 新陈代谢(氧气, 水)
    + 繁殖()
}
class 鸟 {
    - 羽毛
    + 下蛋()
}
class 翅膀
class 大雁 {
    + 下蛋()
    + 飞()
}
class 鸭子 {
    + 下蛋()
}
class 企鹅 {
    + 下蛋()
}

class 飞翔 {
    + 飞()
}

class 雁群

class 讲话 {
    + 说话()
}

class 唐老鸭 {
    + 说话()
}

class 气候 {
    - List~企鹅~
}

氧气 <.. 动物: 依赖
水 <.. 动物: 依赖
动物 <|-- 鸟: 继承
鸟 *--> 翅膀: 组合
鸟 <|-- 大雁: 继承
鸟 <|-- 鸭子: 继承
鸟 <|-- 企鹅: 继承
飞翔 <|.. 大雁: 实现
大雁 <--o 雁群: 聚合
鸭子 <|-- 唐老鸭: 继承
讲话 <|.. 唐老鸭: 实现
企鹅 --> 气候: 关联
```

## 继承(泛化)

继承关系，表示子类如何特化父类的所有特征和行为

## 实现

类与接口的关系，表示类是接口所有特征和行为的实现

## 关联

拥有关系，一个类知道另一个类的属性和方法

## 聚合

整体与部分的关系

## 组合

整体与部分的关系，比聚合的关系还要强，要求聚合关系中代表整体的对象负责代表部分的对象的生命周期，没有整体就不存在部分

## 依赖

使用关系，一般是局部变量，方法的参数或者静态方法的调用

```mermaid
classDiagram
class AggFunction {
    - acc: Accumulator
    + accumulate()
    + retract()
    + getValue()
}

class Accumulator {
    + acc()
    + ret()
}

class MapAccumulator {
    + acc()
    + ret()
}

class LongAccumulator {
    + acc()
    + ret()
}

AggFunction o--> Accumulator
Accumulator <|-- MapAccumulator
Accumulator <|-- LongAccumulator

```

```mermaid
classDiagram
class FusionAggregation {
    + FusionAccumulatorContainer createAccumulator()
    - void accumulateInternal(FusionAccumulatorContainer accumulator, FusionRawData data, FusionMetaData meta)
    - boolean checkNeedUpdate(FusionAccumulatorContainer accumulator, FusionMetaData meta)
    - FusionAccumulatorContainer updateAccumulator(FusionAccumulatorContainer container, FusionMetaData meta)
    - void accumulateData(FusionAccumulatorContainer accumulatorContainer, FusionRawData data) 
    - void retractInternal(FusionAccumulatorContainer accumulator, FusionRawData data, FusionMetaData meta)
    - void retractData(FusionAccumulatorContainer accumulatorContainer, FusionRawData data)
    + Object getValue(FusionAccumulatorContainer fusionAccumulatorContainer)
}
class FusionAccumulatorContainer{
    + Map~String, IFusionAccumulator~ valueMap
    + FusionMetaData meta
}

class IFusionAccumulator {
    + acceptAccumulate(IFusionAccumulatorVistor vistor)
    + acceptRetract(IFusionAccumulatorVistor vistor)

}

class FusionMetaData {
    - Map~String, FusionMetaCol~ map
    - Long version
}

class FusionMetaCol {
    - String agg
    - String field
    - boolean isDistinct
}

class AFusionAccumulator
class NumericalFusionAccumulator {
    - Object value
}
class NumbericalDistinctFusionAccumulator {
    - Object value
    - Map~Object, Object~ valueMap
}
class IFusionAccumulatorVistor {
    + vistAccumulate()
    + vistRetract()
}
class CountFusionVistor 
class CountDistinctFusionVistor
class SumFusionVistor

FusionAccumulatorContainer <.. FusionAggregation
IFusionAccumulator o-- FusionAccumulatorContainer
FusionMetaData <-- FusionAccumulatorContainer
FusionMetaCol o-- FusionMetaData 
IFusionAccumulator <|.. AFusionAccumulator 
AFusionAccumulator <|-- NumericalFusionAccumulator
AFusionAccumulator <|-- NumbericalDistinctFusionAccumulator
IFusionAccumulator <.. IFusionAccumulatorVistor
IFusionAccumulatorVistor <|.. CountFusionVistor 
IFusionAccumulatorVistor <|.. CountDistinctFusionVistor 
IFusionAccumulatorVistor <|.. SumFusionVistor 
```