---
title: API Hususlar | Azure Marketi
description: Pazar apilerini kullanırken sürüm, hata işleme ve yetkilendirme sorunları.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: e4d4d5cb16e1037458d09f8c7681ab2d2ecf8676
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81256340"
---
# <a name="api-considerations"></a>API Hususlar

<a name="api-versioning"></a>API sürüm
--------------

> [!NOTE]
> Bulut İş Ortağı Portalı API'leri İş Ortağı Merkezi ile entegre edilmiştir ve teklifleriniz İş Ortağı Merkezi'ne geçtikten sonra çalışmaya devam edecektir. Entegrasyon küçük değişiklikler sunar. Kodunuzu İş Ortağı Merkezi'ne geçişten sonra çalışmaya devam etmesini sağlamak için [Bulut İş Ortağı Portalı API Başvurusu'nda](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) listelenen değişiklikleri gözden geçirin.

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
