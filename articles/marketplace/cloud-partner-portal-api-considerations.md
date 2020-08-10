---
title: API konuları-Azure Marketi
description: Market API 'Leri kullanılırken sürüm oluşturma, hata işleme ve yetkilendirme sorunları.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: mingshen-ms
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 0920bec160874f27c8b1b6f2132951b57719b31c
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88035289"
---
# <a name="api-considerations"></a>API'lerde dikkat edilmesi gerekenler

<a name="api-versioning"></a>API sürümü oluşturma
--------------

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri ile tümleşiktir ve Iş Ortağı Merkezi 'nde çalışmaya devam edecektir. Geçiş küçük değişiklikler sunar. Iş Ortağı Merkezi 'ne geçtikten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](./cloud-partner-portal-api-overview.md) listelenen değişiklikleri gözden geçirin. CPP API 'Leri yalnızca Iş Ortağı Merkezi 'ne geçişten önce tümleştirilmiş mevcut ürünler için kullanılmalıdır; Yeni ürünlerin Iş Ortağı Merkezi gönderme API 'Leri kullanması gerekir.

API 'nin aynı anda kullanılabilen birden fazla sürümü olabilir. İstemciler, `api-version` sorgu dizesinin parçası olarak parametre sağlayarak hangi sürümü çağırmak istediğinizi belirtmelidir.

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

Bu başvurudaki tüm API 'Ler için, yetkilendirme üst bilgisini Azure Active Directory (Azure AD) tarafından alınan taşıyıcı belirteçle birlikte geçirmeniz gerekir. Bu üst bilgi, geçerli bir yanıt almak için gereklidir; Yoksa bir `401 Unauthorized` hata döndürülür. 

``` HTTP
  GET https://cloudpartner.azure.com/api/offerTypes?api-version=2016-08-01-preview

    Accept: application/json 
    Authorization: Bearer <YOUR_TOKEN> 
```
