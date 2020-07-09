---
title: API konuları-Azure Marketi
description: Market API 'Leri kullanılırken sürüm oluşturma, hata işleme ve yetkilendirme sorunları.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/08/2020
ms.openlocfilehash: bde55c48e8a3730727af7f3930b2a507c03e3ff3
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86102727"
---
# <a name="api-considerations"></a>API konuları

<a name="api-versioning"></a>API sürümü oluşturma
--------------

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri iş ortağı merkezi ile tümleşiktir ve teklifleriniz iş ortağı merkezi 'ne geçirildikten sonra çalışmaya devam edecektir. Tümleştirme küçük değişiklikler sunar. İş Ortağı Merkezi 'ne geçişten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](./cloud-partner-portal-api-overview.md) listelenen değişiklikleri gözden geçirin.

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
