---
title: _AxlRSAKeyValueToPublicKeyToken 函数
ms.date: 03/30/2017
api_name:
- _AxlRSAKeyValueToPublicKeyToken
api_location:
- clr.dll
api_type:
- DLLExport
ms.assetid: d60f19fe-7bec-47ba-b60e-ba9ce66abf8c
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 4f9981c4cf2e45795576024b797f93831324dbc9
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2019
ms.locfileid: "67741263"
---
# <a name="axlrsakeyvaluetopublickeytoken-function"></a>\_AxlRSAKeyValueToPublicKeyToken 函数

将模数和指数转换为强名称公钥标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT _AxlRSAKeyValueToPublicKeyToken (  
    [in]  PCRYPT_DATA_BLOB pModulusBlob,  
    [in]  PCRYPT_DATA_BLOB pExponentBlob,  
    [out] LPWSTR           *ppwszPublicKeyToken  
);  
```  
  
## <a name="parameters"></a>参数  
 `pModulusBlob`  
 [in]Base64 编码的模数 blob (从\<取模 > 元素)。  请参阅[CRYPTOAPI_BLOB](/windows/desktop/api/dpapi/ns-dpapi-_cryptoapi_blob)结构。  
  
 `pExponentBlob`  
 [in]Base64 编码的指数 blob (从\<指数 > 元素)。 请参阅[CRYPTOAPI_BLOB](/windows/desktop/api/dpapi/ns-dpapi-_cryptoapi_blob)结构。  
  
 `ppwszPublicKeyToken`  
 [out] 指向 WCHAR * 的指针，用于接收十六进制编码的公钥标记。  
  
## <a name="return-value"></a>返回值  
 如果此函数成功，则返回 `S_OK`。 否则，返回错误代码。  
  
## <a name="see-also"></a>请参阅

- [验证码](../../../../docs/framework/unmanaged-api/authenticode/index.md)
