---
title: API Hususlar | Azure Marketi
description: Pazar apilerini kullanırken sürüm, hata işleme ve yetkilendirme sorunları.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 4e04f521ed2023dfb9cd562549cb2e1bcd319b8c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288640"
---
# <a name="api-considerations"></a>API Hususlar


<a name="api-versioning"></a>API sürüm
--------------

API'nin aynı anda kullanılabilen birden çok sürümü olabilir. İstemciler, sorgu dizesinin `api-version` bir parçası olarak parametreyi sağlayarak hangi sürümü kullanmak istediklerini belirtmelidir.

   `GET https://cloudpartner.azure.com/api/offerTypes?api-version=2017-10-31`

Bilinmeyen veya geçersiz API sürümü olan bir isteğe yanıt bir HTTP kodu 400'dür. Bu hata, yanıt gövdesinde bilinen API sürümlerinin koleksiyonunu döndürür.

``` json
    {
        "error": { 
            "code":"InvalidAPIVersion",
            "message":"Invalid api version. Allowed values are [2016-08-01-preview]"
        }
    }
```            

<a name="errors"></a>Hatalar
------

API, ilgili HTTP durum kodlarındaki hatalara yanıt verir ve isteğe bağlı olarak, yanıttaki ek bilgiler JSON olarak serihale getirirken.
Bir hata aldığınızda, özellikle 400 sınıflık bir hata, temel nedeni düzeltmeden önce isteği yeniden denemeyin. Örneğin, yukarıdaki örnek yanıtta, isteği yeniden göndermeden önce API sürüm parametresini düzeltin.

<a name="authorization-header"></a>Yetkilendirme üstbilgi
--------------------

Bu başvurudaki tüm API'ler için, Yetkilendirme üstbilgisini Azure Active Directory'den (Azure AD) alınan taşıyıcı belirteciyle birlikte geçirmeniz gerekir. Bu üstbilginin geçerli bir yanıt alması gerekir; yoksa, bir `401 Unauthorized` hata döndürülür. 

``` HTTP
  GET https://cloudpartner.azure.com/api/offerTypes?api-version=2016-08-01-preview

    Accept: application/json 
    Authorization: Bearer <YOUR_TOKEN> 
```
