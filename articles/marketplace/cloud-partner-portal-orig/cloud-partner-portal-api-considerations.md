---
title: API konuları | Azure Marketi
description: Market API 'Leri kullanılırken sürüm oluşturma, hata işleme ve yetkilendirme sorunları.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 762c90b62ed2a9347ae88a50a11bfe02f3b23ba4
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73162656"
---
# <a name="api-considerations"></a>API konuları


<a name="api-versioning"></a>API sürümü oluşturma
--------------

API 'nin aynı anda kullanılabilen birden fazla sürümü olabilir. İstemciler, sorgu dizesinin bir parçası olarak `api-version` parametresini sağlayarak hangi sürümü çağırmak istediğinizi belirtmelidir.

   `GET https://cloudpartner.azure.com/api/offerTypes?api-version=2017-10-31`

Bilinmeyen veya geçersiz API sürümüne sahip bir isteğin yanıtı bir HTTP kodudur 400. Bu hata, yanıt gövdesinde bilinen API sürümlerinin koleksiyonunu döndürür.

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

API, karşılık gelen HTTP durum kodlarıyla ve isteğe bağlı olarak JSON olarak serileştirildiği yanıtta ek bilgilerle hatalara yanıt verir.
Bir hata aldığınızda, özellikle 400 sınıfı bir hata, temeldeki nedeni düzeltmeden önce isteği yeniden denemeyin. Örneğin, yukarıdaki örnek yanıtta, isteği yeniden göndermeden önce API sürümü parametresini onarın.

<a name="authorization-header"></a>Yetkilendirme üst bilgisi
--------------------

Bu başvurudaki tüm API 'Ler için, yetkilendirme üst bilgisini Azure Active Directory (Azure AD) tarafından alınan taşıyıcı belirteçle birlikte geçirmeniz gerekir. Bu üst bilgi, geçerli bir yanıt almak için gereklidir; Yoksa `401 Unauthorized` bir hata döndürülür. 

``` HTTP
  GET https://cloudpartner.azure.com/api/offerTypes?api-version=2016-08-01-preview

    Accept: application/json 
    Authorization: Bearer <YOUR_TOKEN> 
```
