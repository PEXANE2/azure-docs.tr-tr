---
title: Denetim düzlemi ve veri düzlemi işlemleri
description: Denetim düzlemi ve veri düzlemi işlemleri arasındaki farkı açıklar. Denetim düzlemi işlemleri Azure Resource Manager tarafından işlenir. Veri düzlemi işlemleri bir hizmet tarafından işlenir.
ms.topic: conceptual
ms.date: 09/10/2020
ms.openlocfilehash: a0575c37c80417d9859ef36366dc9f26cdb4dbe4
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90039107"
---
# <a name="azure-control-plane-and-data-plane"></a>Azure denetim düzlemi ve veri düzlemi

Azure işlemleri, denetim düzlemi ve veri düzlemi olmak üzere iki kategoriye ayrılabilir. Bu makalede, bu iki tür işlem arasındaki farklar açıklanmaktadır.

Aboneliğinizdeki kaynakları yönetmek için denetim düzlemi 'ni kullanırsınız. Kaynak türü örneğiniz tarafından sunulan özellikleri kullanmak için veri düzlemi kullanın.

Örneğin:

* Denetim düzlemi aracılığıyla bir sanal makine oluşturursunuz. Sanal makine oluşturulduktan sonra, Uzak Masaüstü Protokolü (RDP) gibi veri düzlemi işlemleri aracılığıyla etkileşime geçin.

* Denetim düzlemi aracılığıyla bir depolama hesabı oluşturursunuz. Depolama hesabındaki verileri okumak ve yazmak için veri düzlemi 'ni kullanırsınız.

* Denetim düzlemi aracılığıyla Cosmos veritabanı oluşturursunuz. Cosmos veritabanındaki verileri sorgulamak için veri düzlemi 'ni kullanırsınız.

## <a name="control-plane"></a>Kontrol düzlemi

Denetim düzlemi işlemlerine yönelik tüm istekler Azure Resource Manager URL 'sine gönderilir. Bu URL, Azure ortamına göre farklılık gösterir.

* Azure genel için, URL olur `https://management.azure.com` .
* Azure Kamu için, URL olur `https://management.usgovcloudapi.net/` .
* Azure Almanya için, URL olur `https://management.microsoftazure.de/` .
* Microsoft Azure Çin 21Vianet için, URL olur `https://management.chinacloudapi.cn` .

Azure Resource Manager URL 'sini hangi işlemlerin kullanacağınızı saptamak için bkz. [Azure REST API](/rest/api/azure/). Örneğin, MySql için [oluşturma veya güncelleştirme işlemi](/rest/api/mysql/databases/createorupdate) , istek URL 'si şu olduğundan bir denetim düzlemi işlemidir:

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{serverName}/databases/{databaseName}?api-version=2017-12-01
```

Azure Resource Manager tüm denetim düzlemi isteklerini işler. Kaynaklarınızı yönetmek için uyguladığınız Azure özelliklerini otomatik olarak uygular, örneğin:

* [Azure rol tabanlı Access Control (RBAC)](../../role-based-access-control/overview.md)
* [Azure İlkesi](../../governance/policy/overview.md)
* [Yönetim kilitleri](lock-resources.md)
* [Etkinlik Günlükleri](view-activity-logs.md)

İsteğin kimliğini doğruladıktan sonra, Azure Resource Manager işlemi tamamlayan kaynak sağlayıcısına gönderir.

Denetim düzlemi istekleri işlemek için iki senaryo içerir-"yeşil alan" ve "kahverengi alan". Yeşil alan yeni kaynaklara başvurur. Kahverengi alanı mevcut kaynaklara başvurur. Kaynakları dağıtırken, yeni kaynakların ne zaman oluşturulacağını ve mevcut kaynakların ne zaman güncelAzure Resource Manager anladığını. Özdeş kaynakların oluşturulmasını endişelenmeniz gerekmez.

## <a name="data-plane"></a>Veri düzlemi

Veri düzlemi işlemlerine yönelik istekler, örneğinize özgü bir uç noktaya gönderilir. Örneğin, bilişsel hizmetler 'deki [dil Algıla işlemi](/rest/api/cognitiveservices/textanalytics/detect%20language/detect%20language) , istek URL 'si şu olduğundan bir veri düzlemi işlemidir:

```http
POST {Endpoint}/text/analytics/v2.0/languages
```

Veri düzlemi işlemleri REST API sınırlı değildir. Bunlar, bir sanal makinede veya veritabanı sunucusunda oturum açma gibi ek kimlik bilgileri gerektirebilir.

Yönetim ve idare uygulayan özellikler veri düzlemi işlemlerine uygulanmayabilir. Kullanıcılarınızın çözümlerinizle etkileşime girmesine yönelik farklı yolları göz önünde bulundurmanız gerekir. Örneğin, kullanıcıların bir veritabanını silmesini engelleyen bir kilit, kullanıcıların sorgular aracılığıyla veri silmesini engellemez.

Bazı ilkeleri, veri düzlemi işlemlerini yönetmek için kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Ilkesinde kaynak sağlayıcısı modları (Önizleme)](../../governance/policy/concepts/definition-structure.md#resource-provider-modes).

## <a name="next-steps"></a>Sonraki adımlar

* Azure Resource Manager genel bir bakış için bkz. [Azure Resource Manager nedir?](overview.md)

* Yeni kaynaklarda ve mevcut kaynaklarda ilke tanımlarının etkisi hakkında daha fazla bilgi edinmek için bkz. [Yeni bir Azure ilke tanımının etkisini değerlendirme](../../governance/policy/concepts/evaluate-impact.md).
