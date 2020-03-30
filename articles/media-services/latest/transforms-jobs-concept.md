---
title: Medya Hizmetlerinde Dönüşümler ve İşler
titleSuffix: Azure Media Services
description: Videolarınızı Azure Medya Hizmetleri'nde işleme kurallarını açıklamak için dönüşümleri nasıl oluşturabileceğinizi öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 08/19/2019
ms.author: juliako
ms.openlocfilehash: ab99b974aed6f8cd5e1da2ee9b427f593b405889
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73571230"
---
# <a name="transforms-and-jobs-in-media-services"></a>Medya Hizmetlerinde Dönüşümler ve İşler

Bu konu [Dönüşümler](https://docs.microsoft.com/rest/api/media/transforms) ve [İşler](https://docs.microsoft.com/rest/api/media/jobs) hakkında ayrıntılı bilgi verir ve bu varlıklar arasındaki ilişkiyi açıklar.

## <a name="overview"></a>Genel Bakış

### <a name="transformsjobs-workflow"></a>Dönüşümler/İşler iş akışı

Aşağıdaki diyagram dönüşümleri/iş iş akışını gösterir:

![Azure Medya Hizmetleri'nde iş akışını dönüştürür ve işler](./media/encoding/transforms-jobs.png)

#### <a name="typical-workflow"></a>Tipik iş akışı

1. Dönüşüm oluşturun.
2. Bu Dönüşüm altında İş Gönder.
3. Dönüşümleri Listele.
4. Gelecekte kullanmayı düşünmüyorsanız, bir Dönüşümü silin.

#### <a name="example"></a>Örnek

Tüm videolarınızın ilk karesini küçük resim görüntüsü olarak ayıklamak istediğinizi varsayalım- atacağınız adımlar şunlardır:

1. Tarifi veya videolarınızı işleme kuralını tanımlayın: "videonun ilk karesini küçük resim olarak kullanın".
2. Her video için, hizmete şunu söylerdiniz:
    1. Videoyu nerede bulabilirim?
    2. Çıktı küçük resim resminin yazılması gereken yer.

**Dönüşüm,** tarifi bir kez oluşturmanıza (Adım 1) ve bu tarifi kullanarak İşler göndermenize yardımcı olur (Adım 2).

> [!NOTE]
> **Datetime** türünün Dönüşüm ve **İş** özellikleri her zaman UTC biçimindedir.

## <a name="transforms"></a>Dönüştürmeler

Videoları kodlamak veya çözümlemek için ortak görevleri yapılandırmak için **Dönüşümler'i** kullanın. Her **Dönüşüm,** video veya ses dosyalarınızı işlemek için bir reçete yi veya iş akışını açıklar. Tek bir Dönüşüm birden fazla kural uygulayabilir. Örneğin, Dönüşüm, her videonun belirli bir bit hızında bir MP4 dosyasına kodlanmış olmasını ve videonun ilk çerçevesinden küçük resim görüntüsünün oluşturulabileceğini belirtebilir. Dönüşüm'ünüzüze eklemek istediğiniz her kural için bir TransformOutput girişi eklersiniz. Giriş ortamı dosyalarının nasıl işlenmesi gerektiğini Dönüştür'e anlatmak için hazır ayarları kullanırsınız.

### <a name="viewing-schema"></a>Şema görüntüleme

Media Services v3'te, ön ayarlar API'nin kendisinde güçlü bir şekilde yazılan varlıklardır. Bu nesneler için "şema" tanımını [Açık API Belirtiminde (veya Swagger)'da](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01)bulabilirsiniz. [Ayrıca REST API,](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.standardencoderpreset?view=azure-dotnet)veya diğer Medya Hizmetleri v3 SDK başvuru belgelerinde önceden ayarlanmış tanımları **(StandardEncoderPreset**gibi) görüntüleyebilirsiniz.

### <a name="creating-transforms"></a>Dönüşümler Oluşturma

REST, CLI veya yayınlanan SDK'lardan herhangi birini kullanarak Dönüşümler oluşturabilirsiniz. Medya Hizmetleri v3 API'si Azure Kaynak Yöneticisi tarafından yürütüldedilir, bu nedenle Medya Hizmetleri hesabınızda Dönüşümler oluşturmak ve dağıtmak için Kaynak Yöneticisi şablonlarını da kullanabilirsiniz. Dönüşümlere erişimi kilitlemek için rol tabanlı erişim denetimi kullanılabilir.

### <a name="updating-transforms"></a>Dönüşümleri Güncelleme

[Dönüşüm'ünüzü](https://docs.microsoft.com/rest/api/media/transforms)güncelleştirmeniz gerekiyorsa, **Güncelleştirme** işlemini kullanın. Açıklamaveya temel TransformOutputs önceliklerinde değişiklik yapmak için tasarlanmıştır. Devam eden tüm işler tamamlandığında bu tür güncelleştirmelerin yapılması önerilir. Tarifi yeniden yazmak istiyorsanız, yeni bir Dönüşüm oluşturmanız gerekir.

### <a name="transform-object-diagram"></a>Nesne diyagramına dönüştürme

Aşağıdaki diyagram, **türetme** ilişkileri de dahil olmak üzere Dönüştür nesnesini ve başvurur olduğu nesneleri gösterir. Gri oklar, İş başvurularının ve yeşil okların sınıf türetme ilişkilerini gösterdiği bir türü gösterir.

Tam boyutu görüntülemek için resmi seçin.  

<a href="./media/api-diagrams/transform-large.png" target="_blank"><img src="./media/api-diagrams/transform-small.png"></a>

## <a name="jobs"></a>İşler

**İş,** Medya Hizmetleri'nin **Transform'i** belirli bir giriş videosuna veya ses içeriğine uygulamak için gerçek isteğidir. Dönüşüm oluşturulduktan sonra, Medya Hizmetleri API'lerini veya yayımlanmış SDK'lardan herhangi birini kullanarak iş gönderebilirsiniz. **İş,** giriş videosunun konumu ve çıktının konumu gibi bilgileri belirtir. Giriş videonuzun konumunu https URL'leri, SAS URL'leri veya [Varlıklar'ı](https://docs.microsoft.com/rest/api/media/assets)kullanarak belirtebilirsiniz.  

### <a name="job-input-from-https"></a>HTTPS'den iş girişi

İçeriğinize bir URL üzerinden zaten erişilebiliyorsa ve kaynak dosyayı Azure'da depolamanız gerekmiyorsa (örneğin, S3'ten alma) [HTTPS'den iş girişi](job-input-from-http-how-to.md) kullanın. Bu yöntem, Azure Blob depolama alanında içeriğiniz varsa ancak dosyanın bir Varlık'ta olmasına gerek yoksa da uygundur. Şu anda, bu yöntem yalnızca giriş için tek bir dosyayı destekler.

### <a name="asset-as-job-input"></a>İş girişi olarak varlık

Giriş içeriği zaten bir Varlık'taysa veya içerik yerel dosyada depolanmışsa, [Varlık'ı iş girişi olarak](job-input-from-local-file-how-to.md) kullanın. Akış veya indirme için giriş varlığını yayımlamayı planlıyorsanız (indirmek için mp4 yayınlamak istediğinizi varsan, ancak aynı zamanda metin veya yüz algılama konuşma yapmak istediğinizi varsaysanız) aynı zamanda iyi bir seçenektir. Bu yöntem çok dosyalı varlıkları destekler (örneğin, yerel olarak kodlanmış MBR akış kümeleri).

### <a name="checking-job-progress"></a>İş ilerlemesini denetleme

İşlerin ilerlemesi ve durumu Olay Izgarası ile olayları izleyerek elde edilebilir. Daha fazla bilgi için [EventGrid'i kullanarak olayları izleyin.](job-state-events-cli-how-to.md)

### <a name="updating-jobs"></a>İşleri Güncelleme

[İş](https://docs.microsoft.com/rest/api/media/jobs) varlığındaki Güncelleştirme işlemi, iş gönderildikten sonra *açıklamayı* ve *öncelik* özelliklerini değiştirmek için kullanılabilir. *Öncelik* özelliğinde yapılan bir değişiklik, yalnızca iş hala sıraya ait durumdaysa etkili olur. İş işlemeye başlamışsa veya tamamlanmışsa, önceliği değiştirmenin bir etkisi yoktur.

### <a name="job-object-diagram"></a>İş nesnesi diyagramı

Aşağıdaki diyagram, **İş** nesnesini ve türetme ilişkilerini de içeren başvurur nesneleri gösterir.

Resmi tam boyutlu görüntülemek için tıklayın.  

<a href="./media/api-diagrams/job-large.png" target="_blank"><img src="./media/api-diagrams/job-small.png"></a>

## <a name="configure-media-reserved-units"></a>Ortam Ayrılmış Birimleri Yapılandırma

Medya Hizmetleri v3 veya Video Indexer tarafından tetiklenen Ses Analizi ve Video Analizi İşleri için, hesabınıza 10 S3 Medya Ayrılmış Birim (MRUs) sağlamanız önerilir. 10'dan fazla S3 MRUs'a ihtiyacınız varsa, [Azure portalını](https://portal.azure.com/)kullanarak bir destek bileti açın.

Ayrıntılar için [CLI ile ölçeklendirme ortamı işleme](media-reserved-units-cli-how-to.md)ye bakın.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="see-also"></a>Ayrıca bkz.

* [Hata kodları](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode)
* [Medya Hizmetleri kuruluşlarının filtreleme, sipariş, sayfalama](entities-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

- Geliştirmeye başlamadan önce, [Media Services v3 API'leri ile Geliştirme'yi](media-services-apis-overview.md) gözden geçirin (API'lere erişim, adlandırma kuralları vb. hakkında bilgi içerir)
- Şu öğreticilere göz atın:

    - [Öğretici: URL'ye dayalı uzak bir dosyayı kodlayın ve videoyu akış](stream-files-tutorial-with-rest.md)
    - [Öğretici: Video yükleme, kodlama ve akış](stream-files-tutorial-with-api.md)
    - [Öğretici: Medya Hizmetleri v3 ile videoları analiz edin](analyze-videos-tutorial-with-api.md)
