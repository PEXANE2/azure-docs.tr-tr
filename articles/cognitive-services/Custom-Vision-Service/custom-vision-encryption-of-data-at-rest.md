---
title: Bekleyen verilerin şifrelenmesi Özel Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Bekleyen verilerin şifrelenmesi Özel Görüntü İşleme.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: egeaney
ms.openlocfilehash: 287344b325237843a549973ae61f569eae6dac93
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83202276"
---
# <a name="custom-vision-encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi Özel Görüntü İşleme

Azure Özel Görüntü İşleme, verileri buluta kalıcı hale geldiğinde otomatik olarak şifreler. Özel Görüntü İşleme şifreleme, verilerinizi korur ve kurumsal güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur.

[!INCLUDE [cognitive-services-about-encryption](../../../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar yalnızca 11 Mayıs 2020 ' den sonra oluşturulan kullanılabilir kaynaklardır. CMK 'yi Özel Görüntü İşleme kullanmak için yeni bir Özel Görüntü İşleme kaynağı oluşturmanız gerekir. Kaynak oluşturulduktan sonra, yönetilen kimliğinizi ayarlamak için Azure Key Vault kullanabilirsiniz.

### <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Müşteri tarafından yönetilen anahtarlar Şu anda şu bölgelerde kullanılabilir:

* ABD Orta Güney
* Batı ABD 2
* Doğu ABD
* US Gov Virginia

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Özel Görüntü İşleme müşteri tarafından yönetilen anahtar Istek Formu](https://aka.ms/cogsvc-cmk)
* [Azure Key Vault hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)


