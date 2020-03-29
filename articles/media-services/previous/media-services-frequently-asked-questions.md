---
title: Azure Medya Hizmetleri sık sorulan sorular
description: Bu makalede, Azure Medya Hizmetleri hakkında sık sorulan soruların yanıtları verebisin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: bdb5a43ad6669bfcd6e93ef4e3bf1646314e4606
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76705879"
---
# <a name="media-services-v2-frequently-asked-questions"></a>Medya Hizmetleri v2 sık sorulan sorular

Bu makalede, Azure Medya Hizmetleri (AMS) kullanıcı topluluğu tarafından sık sorulan sorular ele al›nmaktadır.

## <a name="general-ams-faqs"></a>Genel AMS Sıkça Sorulan Soru

S: Apple iOS aygıtlarına nasıl akış yaparsınız?

C: akışlı kaynak sunucusuna Apple iOS yerel cihazlarında tüketim için HLS içeriğini geri döndüreceğini söylemek için URL'nin "/Manifest" bölümüne "(biçim=m3u8-aapl)" yolunu ekleyin (ayrıntılar için [bkz. içerik teslimi),](media-services-deliver-content-overview.md)

S: Dizin oluşturmayı nasıl ölçeklendirin?

C: Ayrılmış birimler Kodlama ve Dizin oluşturma görevleri için aynıdır. [Ayrılmış Birimleri Kodlama Ölçeği ile](media-services-scale-media-processing-overview.md)ilgili yönergeleri izleyin. Dizinleyici performansının Ayrılmış Birim Türü'den etkilenmediğini **unutmayın.**

S: Bir video yükledim, kodladım ve yayınladım. Akışı sağlamaya çalıştığımda videonun oynamamasının nedeni ne olur?

C: En yaygın nedenlerden biri, **Çalışan** durumunda oynatmaya çalıştığınız akış bitiş noktasına sahip olmamasıdır.  

S: Canlı yayında derleme yapabilir miyim?

C: Azure Medya Hizmetleri'nde şu anda canlı akışlarda bir araya oluşturma sunulmadığı için bilgisayarınızda önceden beste yapmanız gerekir.

S: Azure CDN'yi Canlı Akış'ta kullanabilir miyim?

C: Medya Hizmetleri Azure CDN ile tümleştirmeyi destekler (daha fazla bilgi için [bkz.](media-services-portal-manage-streaming-endpoints.md)  CDN ile Canlı akışı kullanabilirsiniz. Azure Media Services, Düzgün Akış, HLS ve MPEG-DASH çıkışları sağlar. Tüm bu biçimler veri aktarmak ve HTTP önbelleğe alma avantajlarından yararlanmak için HTTP'yi kullanır. Canlı akışta gerçek video/ses verileri parçalara ayrılır ve bu ayrı parçalar CDN'de önbelleğe alınr. Yalnızca verilerin yenilenmesi gerekir, bu veriler apaçık verilerdir. CDN, bildirim verilerini düzenli aralıklarla yeniler.

S: Azure Media hizmetleri görüntülerin depolanmasını destekliyor mu?

C: Yalnızca JPEG veya PNG görüntülerini depolamak istiyorsanız, bunları Azure Blob Depolama'da saklamanız gerekir. Video veya Ses Varlıklarınız ile ilişkili tutmak istemediğiniz sürece bunları Medya Hizmetleri hesabınıza koymanın hiçbir yararı yoktur. Veya görüntüleri video kodlayıcısında yer döşemesi olarak kullanmanız gerekebilirse. Media Encoder Standard, videoların üst üste bindiren görüntüleri destekler ve desteklenen giriş biçimleri olarak JPEG ve PNG'yi listeler. Daha fazla bilgi için [bindirme oluşturma](media-services-advanced-encoding-with-mes.md#overlay)bilgisine bakın.

S: Bir Medya Hizmetleri hesabındaki varlıkları diğerine nasıl kopyalayabilirim?

C: Bir Medya Hizmetleri hesabındaki kıymetleri .NET'i kullanarak diğerine kopyalamak için [Azure Media Services .NET SDK Uzantıları](https://github.com/Azure/azure-sdk-for-media-services-extensions/) deposunda bulunan [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) uzantı yöntemini kullanın. Daha fazla bilgi için [bu](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) forum konusuna bakın.

S: AMS ile çalışırken dosyaları adlandırmak için desteklenen karakterler nelerdir?

C: Medya Hizmetleri, akış içeriği için URL'ler inşa ederken IAssetFile.Name özelliğinin değerini kullanır (örneğin, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Bu nedenle, yüzde kodlama izin verilmez. **Ad** özelliğinin değeri aşağıdaki [yüzde kodlama-ayrılmış karakterlerden](https://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters)hiçbirine sahip olamaz : !*'();:@&=+$,/?%#[]". Ayrıca, sadece bir 'olabilir.' dosya adı uzantısı için.

S: REST kullanarak nasıl bağlanır?

C: AMS API'sine nasıl bağlanabileceğiniz hakkında bilgi için Azure [AD kimlik doğrulaması yla Azure Medya Hizmetleri API'sine eriş'e](media-services-use-aad-auth-to-access-ams-api.md)bakın. 

S: Kodlama işlemi sırasında bir videoyu nasıl döndürebilirim?

C: [Media Encoder Standardı](media-services-dotnet-encode-with-media-encoder-standard.md) 90/180/270 açıları ile döndürme yi destekler. Varsayılan davranış, gelen MP4/MOV dosyasındaki döndürme meta verilerini algılamaya ve bunu telafi etmeye çalıştığı "Otomatik"tir. [Burada](media-services-mes-presets-overview.md)tanımlanan json hazır ayarlarından birine aşağıdaki **Kaynaklar** öğesini ekleyin:

    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [

    ...


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
