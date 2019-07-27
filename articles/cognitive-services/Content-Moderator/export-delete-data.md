---
title: Kullanıcı verilerini dışarı aktarma veya silme-Content Moderator
titleSuffix: Azure Cognitive Services
description: Content Moderator verilerinizi dışarı veya silme hakkında bilgi edinin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: pafarley
ms.openlocfilehash: b18bc2907c00ed1424db440f09c914faff6a9863
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565562"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>Content Moderator Kullanıcı verilerini dışarı aktarma veya silme

Content Moderator, hizmeti çalıştırmak için Kullanıcı verilerini toplar, ancak müşteriler [İnceleme aracını](https://contentmoderator.cognitive.microsoft.com/) ve [denetleme ve gözden geçirme API 'lerini](https://docs.microsoft.com/azure/cognitive-services/content-moderator/api-reference)kullanarak verilerini görüntülemek, dışarı aktarmak ve silmek için tam denetime sahiptir.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Content Moderator Kullanıcı verilerini dışarı aktarma ve silme hakkında daha fazla bilgi için aşağıdaki tabloya bakın.

| Data | Dışarı aktarma Işlemi | İşlemi Siler |
| ---- | ---------------- | ---------------- |
| Hesap bilgileri (abonelik anahtarları) | Yok | Azure portal (Azure abonelikleri) kullanarak silin. Ya da UI ekip ayarlarını [gözden geçir](https://contentmoderator.cognitive.microsoft.com/) sayfasında **takımı Sil** düğmesini kullanın. |
| Özel eşleştirme için görüntüler | [Görüntü kimliklerini al API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676)'sini çağırın. Görüntüler tek yönlü bir özel karma biçiminde depolanır ve gerçek görüntüleri ayıklamanın bir yolu yoktur. | [Tüm görüntüleri SILME API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686)'sini çağırın. Veya Azure portal kullanarak Content Moderator kaynağını silebilirsiniz. |
| Özel eşleştirme koşulları | Cal [tüm terimleri al API 'si](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e) | [Tüm terimleri SILME API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d)'sini çağırın. Veya Azure portal kullanarak Content Moderator kaynağını silebilirsiniz. |
| Tags | Yok | UI etiketi ayarlarını gözden geçir sayfasında her etiket için kullanılabilir **Sil** simgesini kullanın. Ya da UI ekip ayarlarını [gözden geçir](https://contentmoderator.cognitive.microsoft.com/) sayfasında **takımı Sil** düğmesini kullanın. |
| İncelemeler | [Inceleme al API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) 'sini çağırma | UI ekip ayarlarını [gözden geçir](https://contentmoderator.cognitive.microsoft.com/) sayfasındaki **takımı Sil** düğmesini kullanın.
| Kullanıcılar | Yok | UI ekip ayarlarını [gözden geçir](https://contentmoderator.cognitive.microsoft.com/) sayfasında her bir Kullanıcı Için kullanılabilen **Sil** simgesini kullanın. Ya da UI ekip ayarlarını [gözden geçir](https://contentmoderator.cognitive.microsoft.com/) sayfasında **takımı Sil** düğmesini kullanın. |

