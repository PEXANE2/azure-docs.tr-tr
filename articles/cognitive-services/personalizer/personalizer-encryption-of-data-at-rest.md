---
title: Rest 'de verilerin kişiselleştirici hizmeti şifrelemesi
titleSuffix: Azure Cognitive Services
description: Rest 'de verilerin kişiselleştirici hizmeti şifrelemesi.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: egeaney
ms.openlocfilehash: 63526454bb366b214c27bddce24ed9ebc09556b3
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80071094"
---
# <a name="personalizer-service-encryption-of-data-at-rest"></a>Rest 'de verilerin kişiselleştirici hizmeti şifrelemesi

Kişiselleştirici hizmeti, buluta kalıcı hale geldiğinde verilerinizi otomatik olarak şifreler. Kişiselleştirici hizmeti şifrelemesi, verilerinizi korur ve kurumsal güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur.

[!INCLUDE [cognitive-services-about-encryption](../../../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar yalnızca E0 fiyatlandırma katmanında kullanılabilir. Müşteri tarafından yönetilen anahtarları kullanma olanağı istemek için, [Kişiselleştirme hizmeti müşteri tarafından yönetilen anahtar Isteği formunu](https://aka.ms/cogsvc-cmk)doldurun ve gönderebilirsiniz. İsteğinizin durumunu öğrenmek yaklaşık 3-5 iş günü sürer. Talebe bağlı olarak, bir kuyruğa yerleştirilmiş ve alan kullanılabilir olduğunda onaylanmış olabilir. Kişiselleştirici hizmeti ile CMK kullanmaya onaylandıktan sonra, yeni bir kişiselleştirici kaynak oluşturmanız ve fiyatlandırma katmanı olarak E0 ' ı seçmeniz gerekir. E0 fiyatlandırma katmanı ile kişiselleştirici kaynağınız oluşturulduktan sonra, yönetilen kimliğinizi ayarlamak için Azure Key Vault kullanabilirsiniz.

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk-regions.md)]

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Kişiselleştirici hizmeti müşteri tarafından yönetilen anahtar Isteği formu](https://aka.ms/cogsvc-cmk)
* [Azure Key Vault hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
