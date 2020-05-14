---
title: Bekleyen veriler için yüz hizmeti şifreleme
titleSuffix: Azure Cognitive Services
description: Bekleyen veri hizmeti şifrelemesi.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: egeaney
ms.openlocfilehash: 1e0275c91b2243132650be7af256071589091c4b
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83201940"
---
# <a name="face-service-encryption-of-data-at-rest"></a>Bekleyen veriler için yüz hizmeti şifreleme

Yüz hizmeti, verileri buluta kalıcı hale geldiğinde otomatik olarak şifreler. Yüz hizmeti şifrelemesi, verilerinizi korur ve kurumsal güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur.

[!INCLUDE [cognitive-services-about-encryption](../../../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar yalnızca E0 fiyatlandırma katmanında kullanılabilir. Müşteri tarafından yönetilen anahtarları kullanma olanağı istemek için [yüz hizmeti müşteri tarafından yönetilen anahtar Isteği formunu](https://aka.ms/cogsvc-cmk)doldurun ve iletin. İsteğinizin durumunu öğrenmek yaklaşık 3-5 iş günü sürer. Talebe bağlı olarak, bir kuyruğa yerleştirilmiş ve alan kullanılabilir olduğunda onaylanmış olabilir. Yüz hizmeti ile CMK kullanmaya onaylandıktan sonra, yeni bir yüz kaynağı oluşturmanız ve fiyatlandırma katmanı olarak E0 seçmeniz gerekir. E0 fiyatlandırma katmanı ile yüz kaynağınız oluşturulduktan sonra, yönetilen kimliğinizi ayarlamak için Azure Key Vault kullanabilirsiniz.

### <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Müşteri tarafından yönetilen anahtarlar Şu anda şu bölgelerde kullanılabilir:

* ABD Orta Güney
* Batı ABD 2
* Doğu ABD
* US Gov Virginia

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Yüz hizmeti müşteri tarafından yönetilen anahtar Istek Formu](https://aka.ms/cogsvc-cmk)
* [Azure Key Vault hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)


