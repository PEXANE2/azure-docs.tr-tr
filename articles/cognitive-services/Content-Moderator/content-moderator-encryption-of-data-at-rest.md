---
title: İçerik Moderatör veri şifreleme stoyon
titleSuffix: Azure Cognitive Services
description: İçerik Moderatör veri şifreleme istirahat.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: b41d822791f61fce274f628b87c478c3a986f4c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79372166"
---
# <a name="content-moderator-encryption-of-data-at-rest"></a>İçerik Moderatör veri şifreleme stoyon

İçerik Moderatörü, verilerinizi bulutta kalıcı olduğunda otomatik olarak şifreler ve kuruluş güvenliği ve uyumluluk hedeflerinize uymanıza yardımcı olur.

[!INCLUDE [cognitive-services-about-encryption](../../../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar yalnızca E0 fiyatlandırma katmanında kullanılabilir. Müşteri tarafından yönetilen anahtarları kullanabilme isteğinde bulunmak [için, İçerik Moderatörmüşteri tarafından yönetilen Anahtar İstek Formunu](https://aka.ms/cogsvc-cmk)doldurun ve gönderin. İsteğinizin durumunu duymak yaklaşık 3-5 iş günü sürer. İsteğe bağlı olarak, bir kuyruğa yerleştirilebilir ve alan kullanılabilir hale geldikçe onaylanabilirsiniz. İçerik Moderatör hizmetiile CMK'yı kullanmak için onaylandıktan sonra, yeni bir İçerik Moderatör kaynağı oluşturmanız ve Fiyatlandırma Katmanı olarak E0'i seçmeniz gerekir. E0 fiyatlandırma katmanına sahip İçerik Moderatör kaynağınız oluşturulduktan sonra, yönetilen kimliğinizi ayarlamak için Azure Key Vault'u kullanabilirsiniz.

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk-regions.md)]

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk.md)]

## <a name="enable-data-encryption-for-your-content-moderator-team"></a>İçerik Moderatör Ekibiniz için veri şifrelemeyi etkinleştirme

İçerik Moderatör İnceleme Ekibiniz için veri şifrelemesini etkinleştirmek için [Quickstart: Web'de İçerik Moderatörü'nu deneyin.](quick-start.md#create-a-review-team)  

> [!NOTE]
> İçerik Moderatöre E0 fiyatlandırma katmanıyla bir _Kaynak Kimliği_ sağlamanız gerekir.


## <a name="next-steps"></a>Sonraki adımlar

* [İçerik Moderatör Müşteri Tarafından Yönetilen Anahtar İstek Formu](https://aka.ms/cogsvc-cmk)
* [Azure Anahtar Kasası hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
