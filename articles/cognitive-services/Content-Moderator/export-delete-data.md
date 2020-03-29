---
title: Kullanıcı verilerini dışa aktarma veya silme - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: Verileriniz üzerinde tam kontrole sahipsiniz. İçerik Moderatör'ünde verilerinizi nasıl görüntüleyecek, dışa aktarın veya silenleri öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: pafarley
ms.openlocfilehash: 81713bf8d424b9f272f6b1bccf3657810160d4cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73744799"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>İçerik Moderatör'ünde kullanıcı verilerini dışa aktarma veya silme

İçerik Moderatör hizmeti çalıştırmak için kullanıcı verileri toplar, ancak müşteriler [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) ve [Moderasyon ve İnceleme API'leri](https://docs.microsoft.com/azure/cognitive-services/content-moderator/api-reference)kullanarak verilerini görüntülemek, dışa aktarma ve silmek için tam denetime sahiptir.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

İçerik Moderatör'ünde kullanıcı verilerinin nasıl dışa aktarılabildiğini ve silinmeniz hakkında daha fazla bilgi için aşağıdaki tabloya bakın.

| Veri | İhracat Operasyonu | Silme İşlemi |
| ---- | ---------------- | ---------------- |
| Hesap Bilgileri (Abonelik Anahtarları) | Yok | Azure portalını (Azure Abonelikleri) kullanarak silin. Veya [Kullanıcı BirE Ekibi](https://contentmoderator.cognitive.microsoft.com/) Ayarlarını İncele sayfasındaki Ekibi **Sil** düğmesini kullanın. |
| Özel eşleştirme için görüntüler | Görüntü [II'leri Al'ı arayın.](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676) Görüntüler tek yönlü özel karma biçimde depolanır ve gerçek görüntüleri ayıklamanın bir yolu yoktur. | Tüm [Görüntüleri Sil API'yi](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686)arayın. Veya Azure portalını kullanarak İçerik Moderatörü kaynağını silin. |
| Özel eşleştirme terimleri | Cal [tüm terimler API alın](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e) | Tüm [terimleri Sil API'yi](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d)arayın. Veya Azure portalını kullanarak İçerik Moderatörü kaynağını silin. |
| Etiketler | Yok | Kullanıcı Arasını Etiketini İncele sayfasındaki her etiket için kullanılabilir **Sil** simgesini kullanın. Veya [Kullanıcı BirE Ekibi](https://contentmoderator.cognitive.microsoft.com/) Ayarlarını İncele sayfasındaki Ekibi **Sil** düğmesini kullanın. |
| İncelemeler | İnceleme [API'sini Al'ı](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) arayın | [Kullanıcı Bire-ge](https://contentmoderator.cognitive.microsoft.com/) Ekibi ayarlarını gözden geçir sayfasında **Ekibi Sil** düğmesini kullanın.
| Kullanıcılar | Yok | [Kullanıcı Arabirimi](https://contentmoderator.cognitive.microsoft.com/) Ekibi ayarlarını gözden geçir sayfasındaki her kullanıcı için kullanılabilir **Sil** simgesini kullanın. Veya [Kullanıcı BirE Ekibi](https://contentmoderator.cognitive.microsoft.com/) Ayarlarını İncele sayfasındaki Ekibi **Sil** düğmesini kullanın. |

