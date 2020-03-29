---
title: API başvurusu - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: İçerik Moderatörü için çeşitli içerik moderasyonu hakkında bilgi edinin ve API'leri inceleyin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: reference
ms.date: 05/29/2019
ms.author: pafarley
ms.openlocfilehash: 7fc46d06b68dca074da060b4866186a6242ffad2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72757371"
---
# <a name="content-moderator-api-reference"></a>İçerik Moderatör API başvuru

Azure İçerik Moderatör API'leri ile aşağıdaki yollarla işe alabilirsiniz:

- Azure portalında [İçerik Moderatör api'sine abone olun.](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator)
- [İçerik Moderatör İnceleme aracı](https://contentmoderator.cognitive.microsoft.com/)ile kaydolmak için [web'de İçerik Moderatör deneyin](quick-start.md) bakın.

## <a name="moderation-apis"></a>Yönetim API’leri

Moderasyon sonrası iş akışlarınızı ayarlamak için aşağıdaki İçerik Moderatör API'lerini kullanabilirsiniz.

| Açıklama | Başvuru |
| -------------------- |-------------|
| **Görüntü Moderasyonu API’si**<br /><br />Etiketleri, güven puanlarını ve diğer çıkarılan bilgileri kullanarak görüntüleri tarayın ve olası yetişkinlere uygun ve müstehcen içeriği algılayın. <br /><br />Bu bilgileri, aşırılık sonrası iş akışınızdaki içeriği yayınlamak, reddetmek veya gözden geçirmek için kullanın. <br /><br />| [Görüntü Moderasyon API başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c "Görüntü Moderasyon API başvurusu")   |
| **Metin Moderasyonu API’si**<br /><br />Metin içeriğini tazyikle. Küfür terimleri ve kişisel veriler iade edilir. <br /><br />Bu bilgileri, aşırılık sonrası iş akışınızdaki içeriği yayınlamak, reddetmek veya gözden geçirmek için kullanın.<br /><br /> | [Metin Moderasyonu API başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f "Metin Moderasyonu API başvurusu")   |
| **Video Moderasyon API**<br /><br />Videoları tarayıp olası yetişkinlere uygun ve müstehcen içeriği algıla. <br /><br />Bu bilgileri, aşırılık sonrası iş akışınızdaki içeriği yayınlamak, reddetmek veya gözden geçirmek için kullanın.<br /><br /> | [Video Moderasyon API genel bakış](video-moderation-api.md "Video Moderasyon API genel bakış")   |
| **Liste Yönetimi API'si**<br /><br />Resim ve metnin özel dışlama veya dahil etme listelerini oluşturun ve yönetin. Etkinleştirilirse, **Görüntü - Eşleştirme** ve Metin - **Ekran** işlemleri, gönderilen içeriğin özel listelerinize göre bulanık bir şekilde eşleştirilmesini sağlar. <br /><br />Verimlilik için, makine öğrenimi tabanlı ılımlılık adımını atlayabilirsiniz.<br /><br /> | [Liste Yönetimi API başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f675 "Liste Yönetimi API başvurusu")   |

## <a name="review-apis"></a>API’leri inceleme

İnceleme API'leri aşağıdaki bileşenlere sahiptir:

| Açıklama | Başvuru |
| -------------------- |-------------|
| **İşler**<br /><br /> Hem resim hem de metin içeriği için tetkik ve gözden geçirme ılımlılığı iş akışlarını başlatın. Bir Moderasyon işi, Görüntü Moderasyon API'sini ve Metin Moderasyon API'sini kullanarak içeriğinizi tarar. Denetleme işleri, incelemeler oluşturmak için tanımlı ve varsayılan iş akışlarını kullanır. <br /><br />Bir insan moderatör otomatik olarak atanan etiketleri ve tahmin verilerini inceledikten ve bir içerik denetleme kararı gönderdikten sonra, Gözden Geçirme API tüm bilgileri API bitiş noktanıza gönderir.<br /><br /> | [İş başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5 "İş başvurusu")   |
| **İncelemeler**<br /><br />İnsan moderatörler için doğrudan görüntü veya metin incelemeleri oluşturmak için Gözden Geçirme aracını kullanın.<br /><br /> Bir insan moderatör otomatik olarak atanan etiketleri ve tahmin verilerini inceledikten ve bir içerik denetleme kararı gönderdikten sonra, Gözden Geçirme API tüm bilgileri API bitiş noktanıza gönderir.<br /><br /> | [Başvuruları gözden geçirme](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 "Başvuruları gözden geçirme")   |
| **İş akışları**<br /><br />Ekibinizin oluşturduğu özel iş akışları hakkında oluşturun, güncelleyin ve ayrıntıları alın. Gözden Geçirme aracını kullanarak iş akışlarını tanımlarsınız. <br /> <br />İş akışları genellikle İçerik Moderatörünü kullanır, ancak Gözden Geçirme aracında bağlayıcı olarak kullanılabilen bazı diğer API'leri de kullanabilir.<br /><br /> | [İş akışı başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59 "İş akışı başvurusu")   |