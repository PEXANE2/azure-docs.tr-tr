---
title: Azure API Yönetimi'nde protokolleri ve şifreleri yönetme | Microsoft Dokümanlar
description: Azure API Yönetimi'nde protokolleri (TLS) ve şifreleri (DES) nasıl yöneteceğimiz öğrenin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 05/29/2019
ms.author: apimpm
ms.openlocfilehash: 7f87389016286c9f1b91abc77155be9e94005371
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335877"
---
# <a name="manage-protocols-and-ciphers-in-azure-api-management"></a>Azure API Management'taki protokolleri ve şifrelemeleri yönetme

Azure API Yönetimi, hem istemci hem de arka uç tarafları için TLS protokolünün birden çok versiyonunu ve 3DES şifresini destekler.

Bu kılavuz, bir Azure API Yönetimi örneği için protokolleri ve şifreleme yapılandırmasını nasıl yöneteceğimiz gösterilmektedir.

![APIM'de protokolleri ve şifreleri yönetme](./media/api-management-howto-manage-protocols-ciphers/api-management-protocols-ciphers.png)

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları izlemek için şunları yapmış olmalısınız:

* API Yönetimi örneği

## <a name="how-to-manage-tls-protocols-and-3des-cipher"></a>TLS protokolleri ve 3DES şifresi nasıl yönetilir?

1. Azure portalındaki **API Yönetimi örneğinize** gidin.
2. Menüden **Protokol ayarlarını** seçin.  
3. İstenilen protokolleri veya şifreleri etkinleştirin veya devre dışı.
4. **Kaydet**'e tıklayın. Değişiklikler bir saat içinde uygulanacaktır.  

## <a name="next-steps"></a>Sonraki adımlar

* [TLS (Transport Layer Security)](https://docs.microsoft.com/dotnet/framework/network-programming/tls)hakkında daha fazla bilgi edinin.
* API Yönetimi hakkında daha fazla [video](https://azure.microsoft.com/documentation/videos/index/?services=api-management) göz atın.