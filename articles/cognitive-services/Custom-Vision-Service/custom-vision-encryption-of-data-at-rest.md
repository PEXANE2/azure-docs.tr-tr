---
title: Bekleyen verilerin şifrelenmesi Özel Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Microsoft, Microsoft tarafından yönetilen şifreleme anahtarları sunar ve ayrıca bilişsel hizmetler aboneliklerinizi, müşteri tarafından yönetilen anahtarlar (CMK) olarak adlandırılan kendi Anahtarlarınıza göre yönetmenizi sağlar. Bu makalede, Özel Görüntü İşleme için bekleyen veri şifrelemesi ve CMK 'nin nasıl etkinleştirileceği ve yönetileceği ele alınmaktadır.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: egeaney
ms.openlocfilehash: 39257419f179bdce8c94f2ddb3a7cd8f5ac2d34f
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89077764"
---
# <a name="custom-vision-encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi Özel Görüntü İşleme

Azure Özel Görüntü İşleme, verileri buluta kalıcı hale geldiğinde otomatik olarak şifreler. Özel Görüntü İşleme şifreleme, verilerinizi korur ve kurumsal güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur.

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar yalnızca 11 Mayıs 2020 ' den sonra oluşturulan kullanılabilir kaynaklardır. CMK 'yi Özel Görüntü İşleme kullanmak için yeni bir Özel Görüntü İşleme kaynağı oluşturmanız gerekir. Kaynak oluşturulduktan sonra, yönetilen kimliğinizi ayarlamak için Azure Key Vault kullanabilirsiniz.

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
