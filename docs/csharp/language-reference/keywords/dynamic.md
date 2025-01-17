---
title: dynamic - C# 参考
ms.custom: seodec18
ms.date: 07/20/2015
f1_keywords:
- dynamic_CSharpKeyword
helpviewer_keywords:
- dynamic [C#]
- dynamic keyword [C#]
ms.assetid: 9e797102-cc83-4964-bf58-afe4f54d16bc
ms.openlocfilehash: c8748f1869e8e2d5246910fac0100a6c70790579
ms.sourcegitcommit: a970268118ea61ce14207e0916e17243546a491f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67306640"
---
# <a name="dynamic-c-reference"></a>dynamic（C# 参考）

在通过 `dynamic` 类型实现的操作中，该类型的作用是绕过编译时类型检查。 改为在运行时解析这些操作。 `dynamic` 类型简化了对 COM API（例如 Office Automation API）、动态 API（例如 IronPython 库）和 HTML 文档对象模型 (DOM) 的访问。

在大多数情况下，`dynamic` 类型与 `object` 类型的行为类似。 但是，如果操作包含 `dynamic` 类型的表达式，那么不会通过编译器对该操作进行解析或类型检查。 编译器将有关该操作信息打包在一起，之后这些信息会用于在运行时评估操作。 在此过程中，`dynamic` 类型的变量会编译为 `object` 类型的变量。 因此，`dynamic` 类型只在编译时存在，在运行时则不存在。

下面的示例将 `dynamic` 类型的变量与 `object` 类型的变量进行对比。 若要在编译时验证每个变量的类型，请将鼠标指针放在 `WriteLine` 语句中的 `dyn` 或 `obj` 上。 IntelliSense 对 `dyn` 显示“dynamic”  ，对 `obj` 显示“object”  。

[!code-csharp[csrefKeywordsTypes#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/dynamic1.cs#21)]

`WriteLine` 语句显示 `dyn` 和 `obj` 的运行时类型。 此时，两者的类型均为整数。 将生成以下输出：

`System.Int32`

`System.Int32`

若要查看编译时 `dyn` 与 `obj` 之间的区别，请在前面示例的声明和 `WriteLine` 语句之间添加下列两行。

```csharp
dyn = dyn + 3;
obj = obj + 3;
```

 尝试在表达式 `obj + 3` 中添加整数和对象时，将报告编译器错误。 但是，对于 `dyn + 3`，不会报告任何错误。 在编译时不会检查包含 `dyn` 的表达式，原因是 `dyn` 的类型为 `dynamic`。

## <a name="context"></a>上下文

`dynamic` 关键字可以直接出现，也可以作为构造类型的组件在下列情况中出现：

- 在声明中，作为属性、字段、索引器、参数、返回值、本地变量或类型约束的类型。 下面的类定义在多个不同的声明中使用 `dynamic`。

    [!code-csharp[csrefKeywordsTypes#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/dynamic1.cs#22)]

- 在显式类型转换中，作为转换的目标类型。

    [!code-csharp[csrefKeywordsTypes#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/dynamic1.cs#23)]

- 在以下任何情况下：类型用作值（如 `is` 运算符或 `as` 运算符右侧），或者用作构造类型中 `typeof` 的参数。 例如，可以在下列表达式中使用 `dynamic`。

    [!code-csharp[csrefKeywordsTypes#24](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/dynamic1.cs#24)]

## <a name="example"></a>示例

下面的示例在多个声明中使用 `dynamic`。 `Main` 方法也将编译时类型检查与运行时类型检查进行了对比。

[!code-csharp[csrefKeywordsTypes#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/dynamic2.cs#25)]

有关详细信息和示例，请参阅[使用类型 dynamic](../../../csharp/programming-guide/types/using-type-dynamic.md)。

## <a name="see-also"></a>请参阅

- <xref:System.Dynamic.ExpandoObject?displayProperty=nameWithType>
- <xref:System.Dynamic.DynamicObject?displayProperty=nameWithType>
- [使用类型 dynamic](../../programming-guide/types/using-type-dynamic.md)
- [object](object.md)
- [is](../operators/type-testing-and-conversion-operators.md#is-operator)
- [as](../operators/type-testing-and-conversion-operators.md#as-operator)
- [typeof](../operators/type-testing-and-conversion-operators.md#typeof-operator)
- [如何：使用模式匹配以及 is 和 as 运算符安全地进行强制转换](../../how-to/safely-cast-using-pattern-matching-is-and-as-operators.md)
- [演练：创建和使用动态对象](../../programming-guide/types/walkthrough-creating-and-using-dynamic-objects.md)
