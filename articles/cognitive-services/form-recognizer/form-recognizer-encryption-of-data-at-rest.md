---
title: Form tanıyıcı hizmeti, bekleyen verilerin şifrelenmesi
titleSuffix: Azure Cognitive Services
description: Bekleyen veriler için form tanıyıcı şifrelemesi.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: egeaney
ms.openlocfilehash: b897de94ad8cdb628520f9386b076f762a5cc230
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86537961"
---
# <a name="form-recognizer-encryption-of-data-at-rest"></a>Bekleyen verilerin form tanıyıcı şifrelemesini şifreleme

Azure form tanıyıcı, verileri buluta kalıcı hale geldiğinde otomatik olarak şifreler. Form tanıyıcı şifreleme, verilerinizi korur ve kuruluşunuzun güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur.

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar yalnızca 11 Mayıs 2020 ' den sonra oluşturulan kullanılabilir kaynaklardır. CMK 'yı form tanıyıcı ile birlikte kullanmak için yeni bir form tanıyıcı kaynağı oluşturmanız gerekir. Kaynak oluşturulduktan sonra, yönetilen kimliğinizi ayarlamak için Azure Key Vault kullanabilirsiniz.

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Form tanıyıcı müşteri tarafından yönetilen anahtar Istek Formu](https://aka.ms/cogsvc-cmk)
* [Azure Key Vault hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)


