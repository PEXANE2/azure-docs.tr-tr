---
title: Bilişsel hizmetler için Customer-Managed anahtarları
titleSuffix: Cognitive Services
description: Azure portal kullanarak müşteri tarafından yönetilen anahtarları Azure Key Vault ile yapılandırma hakkında bilgi edinin. Müşteri tarafından yönetilen anahtarlar, erişim denetimleri oluşturmanıza, döndürmenize, devre dışı bırakmanızı ve iptal edebilmesini sağlar.
services: cognitive-services
author: erindormier
ms.service: cognitive-services
ms.topic: include
ms.date: 05/28/2020
ms.author: egeaney
ms.openlocfilehash: 88a723abc606a527232b7c1949f35c1fedfdba50
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101706852"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-for-cognitive-services"></a>Bilişsel hizmetler için Azure Key Vault müşteri tarafından yönetilen anahtarları yapılandırın

Bilişsel hizmetler için Azure Key Vault Customer-Managed anahtarları etkinleştirme işlemi ürüne göre farklılık gösterir. Hizmete özgü yönergeler için bu bağlantıları kullanın:

## <a name="vision"></a>Görsel

* [Bekleyen verilerin şifrelenmesi Özel Görüntü İşleme](../custom-vision-service/encrypt-data-at-rest.md)
* [Yüz Hizmetleri, bekleyen verilerin şifrelenmesi](../face/encrypt-data-at-rest.md)
* [Bekleyen verilerin form tanıyıcı şifrelemesini şifreleme](../form-recognizer/encrypt-data-at-rest.md)

## <a name="language"></a>Dil

* [Bekleyen verilerin hizmet şifrelemesini Language Understanding](../LUIS/encrypt-data-at-rest.md)
* [Bekleyen verilerin şifrelenmesi Soru-Cevap Oluşturma](../QnAMaker/encrypt-data-at-rest.md)
* [Bekleyen verilerin Translator şifrelemesi](../translator/encrypt-data-at-rest.md)

## <a name="decision"></a>Karar

* [Bekleyen verilerin şifrelenmesi Content Moderator](../Content-Moderator/encrypt-data-at-rest.md)
* [Bekleyen verilerin kişiselleştirici şifrelemesi](../personalizer/encrypt-data-at-rest.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Key Vault nedir](../../key-vault/general/overview.md)?
* [Bilişsel hizmetler Customer-Managed anahtar Isteği formu](https://aka.ms/cogsvc-cmk)