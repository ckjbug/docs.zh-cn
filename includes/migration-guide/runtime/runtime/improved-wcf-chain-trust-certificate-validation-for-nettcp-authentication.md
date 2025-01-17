---
ms.openlocfilehash: ae9ba8aaf842c6607020abf76c981266f5c4dd61
ms.sourcegitcommit: d55e14eb63588830c0ba1ea95a24ce6c57ef8c8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67856960"
---
### <a name="improved-wcf-chain-trust-certificate-validation-for-nettcp-certificate-authentication"></a>改进了 Net.Tcp 证书身份验证的 WCF 链信任证书验证

|   |   |
|---|---|
|详细信息|.NET Framework 4.7.2 改进了通过 WCF 对传输安全性使用证书身份验证时的链信任证书验证。 由于此改进，用于对服务器进行身份验证的客户端证书必须为客户端身份验证进行配置。  同样，用于对服务器进行身份验证的服务器证书必须为服务器身份验证进行配置。 由于此更改，如果禁用根证书，证书链验证将失败。 相同的更改还通过 Windows 安全汇总应用于 .NET Framework 3.5 及更高版本。 可在[此处](https://support.microsoft.com/en-us/help/4055269/security-only-update-for-net-framework-3-5-1-4-5-2-4-6-4-6-1-4-6-2-4-7)了解详细信息。此更改默认开启，可通过配置设置关闭。|
|建议|<ul><li>验证服务器和客户端证书是否具有所需的 EKU OID。 如果没有，请更新证书。</li><li>验证根证书是否无效。 如果是这样，请更新根证书。</li><li>如何选择退出更改：如果不能更新证书，可以暂时使用以下配置设置来躲开重大更改。但是，选择弃用更改将导致系统易遭受安全性问题。</li></ul><pre><code class="lang-xml">&lt;appSettings&gt;&#13;&#10;&lt;add key=&quot;wcf:useLegacyCertificateUsagePolicy&quot; value=&quot;true&quot; /&gt;&#13;&#10;&lt;/appSettings&gt;&#13;&#10;</code></pre>|
|范围|次要|
|版本|4.7.2|
|类型|运行时|

