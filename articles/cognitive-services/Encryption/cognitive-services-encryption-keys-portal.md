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
ms.openlocfilehash: fca3fc80411432f2e82a68faa3b6f297b8ca5f9c
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2021
ms.locfileid: "100522011"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-for-cognitive-services"></a>Bilişsel hizmetler için Azure Key Vault müşteri tarafından yönetilen anahtarları yapılandırın

Bilişsel hizmetler için Azure Key Vault Customer-Managed anahtarları etkinleştirme işlemi ürüne göre farklılık gösterir. Hizmete özgü yönergeler için bu bağlantıları kullanın:

## <a name="vision"></a>Görsel

* [Bekleyen verilerin şifrelenmesi Özel Görüntü İşleme](../Custom-Vision-Service/encrypt-data-at-rest.md)
* [Yüz Hizmetleri, bekleyen verilerin şifrelenmesi](../Face/encrypt-data-at-rest.md)
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