---
title: 技术区概述
ms.date: 03/30/2017
helpviewer_keywords:
- window regions [WPF]
- Win32 code [WPF], WPF interoperation
- Win32 code [WPF], airspace
- airspace [WPF]
- interoperability [WPF], airspace
- Win32 code [WPF], window regions
ms.assetid: b7cc350f-b9e2-48b1-be14-60f3d853222e
ms.openlocfilehash: 6fd924f5ccb07d92c0952526a185ffc007ed23a1
ms.sourcegitcommit: 83ecdf731dc1920bca31f017b1556c917aafd7a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "67860034"
---
# <a name="technology-regions-overview"></a>技术区概述
如果在应用程序中使用多种呈现技术（例如 WPF、Win32 或 DirectX），则这些呈现技术必须共享公共顶级窗口中的呈现区域。 本主题介绍可能会对 WPF 互操作应用程序的呈现和输入造成影响的问题。  
  
## <a name="regions"></a>区域  
 在顶级窗口内，可以这么想：每个包含互操作应用程序某个技术的 HWND 都具有自己的区域（也称为“空域”）。 窗口内的每个像素都只属于一个特定 HWND，这构成了该 HWND 的区域。 （严格地说，如果有多个 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] HWND，便会有多个 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 区域，但为了便于讨论，可以假定只有一个区域。） 区域暗含了这样一层含义：应用程序生存期内尝试在该像素之上呈现的所有层或其他窗口都必须是同一呈现级技术的一部分。 尝试呈现[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]转移像素[!INCLUDE[TLA2#tla_win32](../../../../includes/tla2sharptla-win32-md.md)]会导致意外结果，并且不允许的尽可能多地通过互操作 Api。  
  
### <a name="region-examples"></a>区域示例  
 下图显示一个混合使用 [!INCLUDE[TLA2#tla_win32](../../../../includes/tla2sharptla-win32-md.md)]、[!INCLUDE[TLA2#tla_dx](../../../../includes/tla2sharptla-dx-md.md)] 和 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 的应用程序。 每种技术都使用属于自己的且互不重叠的一组像素，因此不存在区域问题。  
  
 ![混合使用 Win32、 DirectX 和 WPF 应用程序示例。](./media/technology-regions-overview/win32-directx-windows-presentation-foundation-application.png)  
  
 假设此应用程序使用鼠标指针位置来创建想要在这三个区域中任一区域上方呈现的动画。 无论动画本身采用哪一种技术，该技术都会与其他两种技术的区域发生冲突。 下图演示在一个 Win32 区域上呈现 WPF 圆形的尝试。  
  
 ![尝试在一个 Win32 区域上呈现 WPF 圆形。](./media/technology-regions-overview/render-windows-presentation-foundation-circle-over-win32-region.png)  
  
 如果尝试在不同技术间使用透明度/Alpha 混合，也会发生冲突。  在下图中，[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 框与 [!INCLUDE[TLA2#tla_win32](../../../../includes/tla2sharptla-win32-md.md)] 和 [!INCLUDE[TLA2#tla_dx](../../../../includes/tla2sharptla-dx-md.md)] 区域存在冲突。 因为该 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 框中的像素是半透明的，所以它们必须由 [!INCLUDE[TLA2#tla_dx](../../../../includes/tla2sharptla-dx-md.md)] 和 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 共同拥有，但这是不可能的。  因此，这是另一种冲突情况，且不可生成。  
  
 ![违反 Win32 和 DirectX 区域内的一个 WPF 框的图示。](./media/technology-regions-overview/windows-foundation-presentation-box-violate-win32-directx-region.png)  
  
 前面三个示例使用矩形区域，但也可以使用其他形状。  例如，区域可以具有一个孔。 下图显示了一个带有矩形孔的 [!INCLUDE[TLA2#tla_win32](../../../../includes/tla2sharptla-win32-md.md)] 区域，其大小为 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 和 [!INCLUDE[TLA2#tla_dx](../../../../includes/tla2sharptla-dx-md.md)] 区域的总大小。  
  
 ![图，显示带有矩形孔的 Win32 区域。](./media/technology-regions-overview/win32-region-rectangular-hole.png)  
  
 区域也可以完全不是矩形，或可以是可由 [!INCLUDE[TLA2#tla_win32](../../../../includes/tla2sharptla-win32-md.md)] HRGN（区域）描述的任何形状。  
  
 ![显示非矩形区域的关系图。](./media/technology-regions-overview/nonrectangular-win32-region.png)  
  
## <a name="transparency-and-top-level-windows"></a>透明度和顶级窗口  
 Windows 中的窗口管理器实际上仅处理 [!INCLUDE[TLA2#tla_win32](../../../../includes/tla2sharptla-win32-md.md)] HWND。 因此，每个[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]<xref:System.Windows.Window>都是 HWND。 <xref:System.Windows.Window> HWND 必须遵守适用于任何 HWND 的一般规则。 在该 HWND 内[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]代码可以执行任何操作整体[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]Api 支持。 但是，为实现与桌面上其他 HWND 的交互，[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 必须遵循 [!INCLUDE[TLA2#tla_win32](../../../../includes/tla2sharptla-win32-md.md)] 处理和呈现规则。  [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 通过使用支持非矩形窗口[!INCLUDE[TLA2#tla_win32](../../../../includes/tla2sharptla-win32-md.md)]Api — Hrgn 用于非矩形窗口，并为每像素 alpha 分层的窗口。  
  
 不支持常量 Alpha 和颜色键。  [!INCLUDE[TLA2#tla_win32](../../../../includes/tla2sharptla-win32-md.md)] 分层窗口的功能因平台而异。  
  
 分层窗口可通过指定要应用于窗口中每个像素的 Alpha 值来使整个窗口呈现为半透明状。  （[!INCLUDE[TLA2#tla_win32](../../../../includes/tla2sharptla-win32-md.md)] 实际上支持每像素 Alpha，但这在实际的程序中很难应用，因为在此模式下需要自行绘制任何子 HWND，包括对话框和下拉列表）。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 支持 Hrgn;但是，在此功能没有托管的 Api。 您可以使用平台调用和<xref:System.Windows.Interop.HwndSource>来调用相关[!INCLUDE[TLA2#tla_win32](../../../../includes/tla2sharptla-win32-md.md)]Api。 有关详细信息，请参阅[从托管代码调用本机函数](/cpp/dotnet/calling-native-functions-from-managed-code)。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 分层窗口在不同操作系统上具有不同的功能。 这是因为 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 使用 [!INCLUDE[TLA2#tla_dx](../../../../includes/tla2sharptla-dx-md.md)] 进行呈现，而分层窗口主要用于 [!INCLUDE[TLA2#tla_gdi](../../../../includes/tla2sharptla-gdi-md.md)] 呈现，而非 [!INCLUDE[TLA2#tla_dx](../../../../includes/tla2sharptla-dx-md.md)] 呈现。  
  
- [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 支持 [!INCLUDE[TLA#tla_longhorn](../../../../includes/tlasharptla-longhorn-md.md)] 及更高版本上的硬件加速分层窗口。 [!INCLUDE[TLA2#tla_winxp](../../../../includes/tla2sharptla-winxp-md.md)] 上的硬件加速分层窗口需要 [!INCLUDE[TLA#tla_dx](../../../../includes/tlasharptla-dx-md.md)] 的支持，所以该功能将取决于计算机上的 [!INCLUDE[TLA#tla_dx](../../../../includes/tlasharptla-dx-md.md)] 的版本。  
  
- [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 不支持透明度颜色键，因为 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 无法保证准确呈现所请求的颜色，尤其当呈现采用了硬件加速时更是如此。  
  
- 如果应用程序在 [!INCLUDE[TLA2#tla_winxp](../../../../includes/tla2sharptla-winxp-md.md)] 上运行，则 [!INCLUDE[TLA2#tla_dx](../../../../includes/tla2sharptla-dx-md.md)] 图面之上的分层窗口会在 [!INCLUDE[TLA2#tla_dx](../../../../includes/tla2sharptla-dx-md.md)] 应用程序呈现时闪烁。  （实际的呈现顺序是：[!INCLUDE[TLA#tla_gdi](../../../../includes/tlasharptla-gdi-md.md)] 隐藏分层窗口，然后 [!INCLUDE[TLA2#tla_dx](../../../../includes/tla2sharptla-dx-md.md)] 进行绘制，之后 [!INCLUDE[TLA#tla_gdi](../../../../includes/tlasharptla-gdi-md.md)] 重新显示分层窗口。）  非 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 分层窗口也有此限制。  
  
## <a name="see-also"></a>请参阅

- [WPF 和 Win32 互操作](wpf-and-win32-interoperation.md)
- [演练：承载 WPF 时钟在 Win32 中](walkthrough-hosting-a-wpf-clock-in-win32.md)
- [在 WPF 中承载 Win32 内容](hosting-win32-content-in-wpf.md)
