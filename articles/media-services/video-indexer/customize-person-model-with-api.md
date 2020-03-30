---
title: Video Dizinleyici API ile Kişi modelini özelleştirme
titleSuffix: Azure Media Services
description: Video Dizinleyici API ile Bir Kişi modelini nasıl özelleştireceğimiz öğrenin.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 01/14/2020
ms.author: anzaman
ms.openlocfilehash: fa41fca7f8ad96cf507aa6f04059b1254c8c3961
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80127900"
---
# <a name="customize-a-person-model-with-the-video-indexer-api"></a>Video Dizinleyici API ile Kişi modelini özelleştirme

Video Indexer, video içeriği için yüz algılamayı ve ünlülerin tanınmasını destekler. Ünlü tanıma özelliği, IMDB, Wikipedia ve en çok LinkedIn etkileyenler gibi yaygın olarak istenen veri kaynağına dayalı yaklaşık bir milyon yüzü kapsar. Ünlü tanıma özelliği tarafından tanınmayan yüzler algılanır, ancak adsız bırakılır. Videonuzu Video Indexer'a yükledikten ve sonuçları geri aldıktan sonra, geri dönüp tanınmayan yüzleri adlandırabilirsiniz. Bir yüzü bir adla etiketledikten sonra, yüz ve ad hesabınızın Kişi modeline eklenir. Video Indexer daha sonra gelecekteki videolarınızda ve geçmiş videolarınızda bu yüzü tanıyacaktır.

Video Dizinleyici API'sini, bu konuda açıklandığı gibi, bir videoda algılanan yüzleri yeniden yapmak için kullanabilirsiniz. Video Indexer web sitesini kullanarak Kişiyi [Özelleştir'de](customize-person-model-with-api.md)açıklandığı gibi Video Indexer web sitesini de kullanabilirsiniz.

## <a name="managing-multiple-person-models"></a>Birden çok Kişi modelini yönetme

Video Indexer hesap başına birden çok Kişi modelini destekler. Bu özellik şu anda yalnızca Video Dizinleyici API'leri aracılığıyla kullanılabilir.

Hesabınız farklı kullanım durumu senaryolarına hitap ediyorsa, hesap başına birden çok Kişi modeli oluşturmak isteyebilirsiniz. Örneğin, içeriğiniz sporla ilgiliyse, her spor için ayrı bir Kişi modeli (futbol, basketbol, futbol vb.) oluşturabilirsiniz.

Bir model oluşturulduktan sonra, bir videoyu yüklerken/dizine eklerken veya yeniden dizine eklerken belirli bir Kişi modelinin model kimliğini sağlayarak onu kullanabilirsiniz. Video için yeni bir yüz eğitimi, videonun ilişkili olduğu özel modeli güncelleştirir.

Her hesabın 50 Kişi modeli sınırı vardır. Birden çok Kişi modeli desteğine ihtiyacınız yoksa, yükleme/dizin oluşturma veya yeniden dizine ekin oluşturma yaparken videonuza Bir Kişi model kimliği atamayın. Bu durumda, Video Dizinleyici hesabınızda varsayılan özel Kişi modelini kullanır.

## <a name="create-a-new-person-model"></a>Yeni bir Kişi modeli oluşturma

Belirtilen hesapta yeni bir Kişi modeli oluşturmak [için, bir kişi modeli](https://api-portal.videoindexer.ai/docs/services/operations/operations/Create-Person-Model?) API oluşturun kullanın.

Yanıt, aşağıdaki örnekbiçimiiz aşağıdaki şekilde n için oluşturduğunuz Kişi modelinin adını ve oluşturulan model kimliğini sağlar.

```json
{
    "id": "227654b4-912c-4b92-ba4f-641d488e3720",
    "name": "Example Person Model"
}
```

Daha sonra bir [videoyu dizin yüklemek](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) veya bir videoyu yeniden [dizine eklerken](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-index-video?) **personModelId** parametresinin **kimlik** değerini kullanırsınız.

## <a name="delete-a-person-model"></a>Kişi modelini silme

Belirtilen hesaptan özel bir Kişi modelini silmek [için, bir kişi modeli API'sini silme](https://api-portal.videoindexer.ai/docs/services/operations/operations/Delete-Person-Model?) yi kullanın.

Kişi modeli başarıyla silindikten sonra, silinen modeli kullanan geçerli videolarınızın dizini siz bunları yeniden dizine ekleyene kadar değişmeden kalır. Yeniden dizine alındıktan sonra, silinen modelde adı geçen yüzler, bu model kullanılarak dizine eklenmiş olan geçerli videolarınızda Video Indexer tarafından tanınmaz, ancak yüzler yine de algılanır. Silinen modeli kullanarak dizine eklenmiş geçerli videolarınız artık hesabınızın varsayılan Kişi modelini kullanır. Silinen modelden yüzler de hesabınızın varsayılan modelinde adlandırılmışsa, bu yüzler videolarda tanınmaya devam eder.

Kişi modeli başarıyla silindiğinde iade edilen içerik yoktur.

## <a name="get-all-person-models"></a>Tüm Kişi modellerini alın

Belirtilen hesaptaki tüm Kişi modellerini almak [için, bir kişi modeli API'yi alın'ı](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Person-Models?) kullanın.

Yanıt, hesabınızdaki tüm Kişi modellerinin (belirtilen hesaptaki varsayılan Kişi modeli dahil) ve aşağıdaki örneğin biçimini izleyen ad ve kimliklerinin her birinin bir listesini sağlar.

```json
[
    {
        "id": "59f9c326-b141-4515-abe7-7d822518571f",
        "name": "Default"
    }, 
    {
        "id": "9ef2632d-310a-4510-92e1-cc70ae0230d4",
        "name": "Test"
    }
]
```

Bir [videoyu dizin](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) yüklemek veya yeniden [dizine](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-index-video?)eklerken `id` `personModelId` parametre için Kişi modelinin değerini kullanarak bir video için hangi modeli kullanmak istediğinizi seçebilirsiniz.

## <a name="update-a-face"></a>Bir yüzü güncelleştirin

Bu komut, videonun kimliğini ve yüz kimliğini kullanarak videonuzdaki bir yüzü bir adla güncellemenize olanak tanır. Bu eylem daha sonra videonun yüklendikten/dizinlendikten veya yeniden dizine yükledikten sonra ilişkili olduğu Kişi modelini güncelleştirir. Hiçbir Kişi modeli atanmamışsa, hesabın varsayılan Kişi modelini güncelleştirir.

Sistem daha sonra aynı Kişi modelini paylaşan diğer geçerli videolarınızda aynı yüzün oluşumlarını tanır. Bu bir toplu işlem olduğundan, diğer geçerli videolarınızdaki yüzün tanınması nın etkili olması biraz zaman alabilir.

Video Dizinleyici'nin yeni bir adla ünlü olarak tanıdığı bir yüzü güncelleştirebilirsiniz. Verdiğiniz yeni ad, yerleşik ünlü lerin tanınmasından önce gelecektir.

Yüzü güncellemek [için, güncelleştirmeyi bir video yüz](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-Video-Face?) API'si kullanın.

Adlar Kişi modelleri için benzersizdir, bu nedenle aynı Kişi `name` modelinde iki farklı yüze aynı parametre değeri verirseniz, Video Dizinleyici yüzleri aynı kişi olarak görüntüler ve videonuzu yeniden dizine diğinizde biraraya gelir.

## <a name="next-steps"></a>Sonraki adımlar

[Video Indexer web sitesini kullanarak Kişi modelini özelleştirin](customize-person-model-with-website.md)
