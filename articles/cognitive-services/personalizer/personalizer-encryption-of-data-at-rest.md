---
title: Rest 'de verilerin kişiselleştirici hizmeti şifrelemesi
titleSuffix: Azure Cognitive Services
description: Microsoft, Microsoft tarafından yönetilen şifreleme anahtarları sunar ve ayrıca bilişsel hizmetler aboneliklerinizi, müşteri tarafından yönetilen anahtarlar (CMK) olarak adlandırılan kendi Anahtarlarınıza göre yönetmenizi sağlar. Bu makalede, kişiselleştirici için bekleyen veri şifreleme ve CMK 'nin nasıl etkinleştirileceği ve yönetileceği ele alınmaktadır.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: egeaney
ms.openlocfilehash: a19f0a204bec1c0a43a84d93c2dc4b70ef6ecbe6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89069934"
---
# <a name="personalizer-service-encryption-of-data-at-rest"></a>Rest 'de verilerin kişiselleştirici hizmeti şifrelemesi

Kişiselleştirici hizmeti, buluta kalıcı hale geldiğinde verilerinizi otomatik olarak şifreler. Kişiselleştirici hizmeti şifrelemesi, verilerinizi korur ve kurumsal güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur.

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar yalnızca E0 fiyatlandırma katmanında kullanılabilir. Müşteri tarafından yönetilen anahtarları kullanma yeteneği istemek için, [Kişiselleştirme hizmeti Customer-Managed anahtar Isteği formunu](https://aka.ms/cogsvc-cmk)doldurun ve gönderebilirsiniz. İsteğinizin durumunu öğrenmek yaklaşık 3-5 iş günü sürer. Talebe bağlı olarak, bir kuyruğa yerleştirilmiş ve alan kullanılabilir olduğunda onaylanmış olabilir. Kişiselleştirici hizmeti ile CMK kullanmaya onaylandıktan sonra, yeni bir kişiselleştirici kaynak oluşturmanız ve fiyatlandırma katmanı olarak E0 ' ı seçmeniz gerekir. E0 fiyatlandırma katmanı ile kişiselleştirici kaynağınız oluşturulduktan sonra, yönetilen kimliğinizi ayarlamak için Azure Key Vault kullanabilirsiniz.

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Kişiselleştirici hizmeti Customer-Managed anahtar Isteği formu](https://aka.ms/cogsvc-cmk)
* [Azure Key Vault hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
