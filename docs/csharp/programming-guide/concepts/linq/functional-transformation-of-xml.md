---
title: XML 的功能转换 (C#)
ms.date: 07/20/2015
ms.assetid: 0ccb9251-38d7-44e3-9b84-1b5fe25e4b59
ms.openlocfilehash: b1325644873db29b2c40901ded3eb254b3a31073
ms.sourcegitcommit: 155012a8a826ee8ab6aa49b1b3a3b532e7b7d9bd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2019
ms.locfileid: "66485956"
---
# <a name="functional-transformation-of-xml-c"></a>XML 的功能转换 (C#)
本主题讨论用于修改 XML 文档的纯函数转换方法，并将该方法与过程方法进行比较。  
  
## <a name="modifying-an-xml-document"></a>修改 XML 文档  
 对 XML 程序员来说，最常见的任务之一就是将 XML 从一种形状转换为另一种形状。 XML 文档的形状就是文档的结构，包括下列内容：  
  
- 文档所表达的层次结构。  
  
- 元素和属性的名称。  
  
- 元素和属性的数据类型。  
  
 通常，将 XML 从一种形状转换为另一种形状的最有效方法是纯函数转换方法。 在这种方法中，程序员的主要任务是创建一个转换，该转换将应用到整个 XML 文档（或应用到一个或多个严格定义的节点）。 可以说，函数转换最容易进行编码（如果程序员熟悉这种方法），生成的代码最容易维护，并且相比其他方法通常更加简洁。  
  
### <a name="xml-functional-transformational-technologies"></a>XML 函数转换技术  
 Microsoft 提供两种用于 XML 文档的功能转换技术：XSLT 和 LINQ to XML。 在 <xref:System.Xml.Xsl> 托管命名空间和 MSXML 的本机 COM 实现中都支持 XSLT。 尽管 XSLT 是操作 XML 文档的可靠技术，但它要求专门领域的专业知识，即 XSLT 语言和支持它的 API。  
  
 LINQ to XML 提供了必要的工具，使用这些工具可以在 C# 或 Visual Basic 代码中以富于表现力而又强有力的方式编写纯函数转换。 例如，LINQ to XML 文档中的很多示例都使用纯函数方法。 此外，在[教程：操作 WordprocessingML 文档中的内容 (C#)](../../../../csharp/programming-guide/concepts/linq/shape-of-wordprocessingml-documents.md) 教程中，我们在函数方法中使用 LINQ to XML 操作 Microsoft Word 文档中的信息。  
  
 有关 LINQ to XML 与其他 Microsoft XML 技术的更全面比较，请参见 [LINQ to XML 和其他 XML 技术](../../../../csharp/programming-guide/concepts/linq/linq-to-xml-vs-other-xml-technologies.md)。  
  
 如果源文档具有不规则的结构，则推荐使用 XSLT 工具进行以文档为中心的转换。 但是 LINQ to XML 也可以执行以文档为中心的转换。 有关详细信息，请参阅[如何：使用批注转换 XSLT 样式中的 LINQ to XML 树 (C#)](../../../../csharp/programming-guide/concepts/linq/how-to-use-annotations-to-transform-linq-to-xml-trees-in-an-xslt-style.md)。  
  
## <a name="see-also"></a>请参阅

- [纯函数转换简介 (C#)](../../../../csharp/programming-guide/concepts/linq/introduction-to-pure-functional-transformations.md)
- [教程：操作 WordprocessingML 文档中的内容 (C#)](../../../../csharp/programming-guide/concepts/linq/shape-of-wordprocessingml-documents.md)
- [LINQ to XML 与其他 XML 技术](../../../../csharp/programming-guide/concepts/linq/linq-to-xml-vs-other-xml-technologies.md)
