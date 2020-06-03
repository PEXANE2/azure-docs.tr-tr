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
ms.openlocfilehash: 33495dd5b092cb51b3421e7204f3b529077d63b3
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84309042"
---
# <a name="face-service-encryption-of-data-at-rest"></a>Bekleyen veriler için yüz hizmeti şifreleme

Yüz hizmeti, verileri buluta kalıcı hale geldiğinde otomatik olarak şifreler. Yüz hizmeti şifrelemesi, verilerinizi korur ve kurumsal güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur.

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar yalnızca E0 fiyatlandırma katmanında kullanılabilir. Müşteri tarafından yönetilen anahtarları kullanma olanağı istemek için [yüz hizmeti müşteri tarafından yönetilen anahtar Isteği formunu](https://aka.ms/cogsvc-cmk)doldurun ve iletin. İsteğinizin durumunu öğrenmek yaklaşık 3-5 iş günü sürer. Talebe bağlı olarak, bir kuyruğa yerleştirilmiş ve alan kullanılabilir olduğunda onaylanmış olabilir. Yüz hizmeti ile CMK kullanmaya onaylandıktan sonra, yeni bir yüz kaynağı oluşturmanız ve fiyatlandırma katmanı olarak E0 seçmeniz gerekir. E0 fiyatlandırma katmanı ile yüz kaynağınız oluşturulduktan sonra, yönetilen kimliğinizi ayarlamak için Azure Key Vault kullanabilirsiniz.

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Müşteri tarafından yönetilen anahtarlar Şu anda şu bölgelerde kullanılabilir:

* ABD Orta Güney
* Batı ABD 2
* Doğu ABD
* US Gov Virginia

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="next-steps"></a>Sonraki adımlar

* CMK 'yı destekleyen hizmetlerin tam listesi için bkz. bilişsel [Hizmetler Için müşteri tarafından yönetilen anahtarlar](../encryption/cognitive-services-encryption-keys-portal.md)
* [Azure Key Vault nedir](https://docs.microsoft.com/azure/key-vault/key-vault-overview)?
* [Bilişsel hizmetler müşteri tarafından yönetilen anahtar Istek Formu](https://aka.ms/cogsvc-cmk)
