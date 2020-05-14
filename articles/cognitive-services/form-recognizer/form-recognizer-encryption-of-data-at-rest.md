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
ms.openlocfilehash: cafe170c4f4485791bbd65471a43d1d5f9726775
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83202254"
---
# <a name="form-recognizer-encryption-of-data-at-rest"></a>Bekleyen verilerin form tanıyıcı şifrelemesini şifreleme

Azure form tanıyıcı, verileri buluta kalıcı hale geldiğinde otomatik olarak şifreler. Form tanıyıcı şifreleme, verilerinizi korur ve kuruluşunuzun güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur.

[!INCLUDE [cognitive-services-about-encryption](../../../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar yalnızca 11 Mayıs 2020 ' den sonra oluşturulan kullanılabilir kaynaklardır. CMK 'yı form tanıyıcı ile birlikte kullanmak için yeni bir form tanıyıcı kaynağı oluşturmanız gerekir. Kaynak oluşturulduktan sonra, yönetilen kimliğinizi ayarlamak için Azure Key Vault kullanabilirsiniz.

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk-regions.md)]

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Form tanıyıcı müşteri tarafından yönetilen anahtar Istek Formu](https://aka.ms/cogsvc-cmk)
* [Azure Key Vault hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)


