---
title: Medya Hizmetleri platformunda Medya Analitiği | Microsoft Dokümanlar
description: Kurumsal ölçekte konuşma ve bilgisayar görme hizmetleri koleksiyonu, uyumluluk, güvenlik ve küresel erişim medya analitiğinin genel önizlemesi
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: c56e3781-8510-4f7f-b5ff-a218c1bb6f4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/13/2019
ms.author: juliako
ms.reviewer: milanga; johndeu
ms.openlocfilehash: ab1eba3de474d9ff985e62f491c24fa63be0fa63
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77069634"
---
# <a name="media-analytics-on-the-media-services-platform"></a>Medya Hizmetleri platformunda Medya Analitiği 

## <a name="retirement-plans"></a>Emeklilik planları

> [!IMPORTANT]
> Bazı medya işlemcileri kullanımdan kaldırılıyor. Emeklilik tarihleri ve daha fazla bilgi için [eski bileşenler](legacy-components.md) konusuna bakın. 

## <a name="overview"></a>Genel Bakış

Daha fazla kuruluş, çalışanlarını eğitmek, müşterilerini meşgul etmek ve iş işlevlerini belgelemek için videoyu tercih edilen ortam olarak kullanıyor. Bulut bilgi işlem, bu büyük medya dosyalarını depolamanın, akışa aktarmanın ve bunlara erişmenin bir yolunu sağlar. Ancak bir şirketin video içeriği kütüphanesi büyüdükçe, içerikten içgörü ler elde etmek için eşit derecede etkili bir araç gerekir. 

Bu artan ihtiyacı gidermek için Azure Media Services, Azure Media Analytics'i sunar. Medya Analizi, kuruluş ve işletmelerin video dosyalarından eyleme dönüştürülebilir öngörüler türetmesini kolaylaştıran bir grup konuşma ve görme bileşenidir. Temel Medya Hizmetleri platformu bileşenlerini kullanarak tasarlanan Media Analytics, ilk gün medya işlemlerini ölçekte işleyebilir.

Media Analytics ile geliştiriciler gelişmiş video işlevselliğini uygulamalara hızla getirebilir. Kurumsal ortamlara büyük kuruluşların gerektirdiği tam ölçek, uyumluluk, güvenlik ve küresel erişimi sağlar.

Aşağıdaki diyagram, Media Analytics ve Medya Hizmetleri platformunun diğer önemli bölümlerini gösterir. 

![VoD iş akışı](./media/media-services-analytics-overview/media-services-analytics-overview01.png)

Medya Analizi medya işlemcileri MP4 veya JSON dosyaları üretir. Bir ortam işlemcisi bir MP4 dosyası üretirse, dosyayı aşamalı olarak karşıdan yükleyebilirsiniz. Bir ortam işlemcisi bir JSON dosyası üretiyorsa, dosyayı Azure Blob depolamasından indirebilirsiniz. 

## <a name="media-analytics-services"></a>Medya Analizi hizmetleri

### <a name="indexer"></a>Dizin Oluşturucu
Azure Media Indexer ile içeriği aranabilir hale getirebilir ve kapalı altyazı parçaları oluşturabilirsiniz. Ayrıntılı bilgi ve örnekler için, [Azure Media Indexer ile Medya Dosyalarını Dizine](media-services-index-content.md)Ekleme'ye bakın.

### <a name="motion-detector"></a>Hareket Algılayıcısı
Sabit arka planlara sahip bir videodaki hareketi algılamak için Hareket Dedektörü'ni kullanabilirsiniz. Bu, güvenlik kameraları tarafından algılanan hareket olaylarında yanlış pozitif olup olmadığını kontrol etmeyi mümkün kılar. Ayrıntılı bilgi ve örnekler [için Azure Media Analytics için Hareket algılama bölümüne](media-services-motion-detection.md)bakın.

### <a name="face-detector"></a>Yüz Algılayıcısı
Yüz Dedektörü kullanarak, mutluluk, üzüntü ve sürpriz de dahil olmak üzere insanların yüzlerini ve duygularını tespit edebilirsiniz. Bu, bir etkinliğe katılan kişilerin tepkilerini toplama ve analiz etme de dahil olmak üzere daha sonra açıklanan birkaç yararlı endüstri uygulamasına sahiptir. Ayrıntılı bilgi ve örnekler için Azure [Media Analytics için Yüz ve duygu algılama bölümüne](media-services-face-and-emotion-detection.md)bakın.

### <a name="video-summarization"></a>Video özetleme
Video özetleme, kaynak videodan ilginç parçacıkları otomatik olarak seçerek uzun videoözetleri oluşturmanıza yardımcı olabilir. Bu yetenek, uzun bir videoda neler ini beklemeniz gerektiğinin hızlı bir özetini sağlamak istediğinizde yararlıdır. Ayrıntılı bilgi ve örnekler için, [video özetleme oluşturmak için Azure Medya Video Küçük Resimlerini Kullanın'a](media-services-video-summarization.md)bakın.
### <a name="optical-character-recognition"></a>Optik karakter tanıma
Azure Media OCR (optik karakter tanıma) ile video dosyalarındaki metin içeriğini düzenlenebilir, aranabilir dijital metne dönüştürebilirsiniz. Daha sonra medyanızın video sinyalinden anlamlı meta verilerin çıkarılmasını otomatikleştirebilirsiniz.
### <a name="scalable-face-redaction"></a>Ölçeklenebilir yüz redaksiyon
Azure Media Redactor, bulutta ölçeklenebilir yüz redaksiyonu sunan bir Media Analytics medya işlemcisidir. Yüz redaksiyonunu kullanarak, videonuzu seçili kişilerin yüzlerini bulanıklaştıracak şekilde değiştirebilirsiniz. Yüz redaksiyon hizmetini haber medyasında veya kamu güvenliği söz konusu olduğunda kullanmak isteyebilirsiniz. Birden çok yüz içeren görüntülerin birkaç dakika el ile redact saat sürebilir, ancak bu hizmet ile, yüz redaksiyon sadece birkaç basit adım alır. Daha fazla bilgi için [Azure Media Analytics makalesinde Redact yüzlerini](media-services-face-redaction.md) görün.

### <a name="content-moderation"></a>İçerik Moderasyonu
Azure İçerik Moderatörü, videolarınız için makine destekli Moderasyon kullanmanızı sağlar. Örneğin videolardaki yetişkinlere yönelik veya müstehcen içerikleri tespit edip belirlenen içeriklerin moderasyon ekibiniz tarafından gözden geçirilmesini isteyebilirsiniz. İstenmeyen içerik için videoları el ile denetleme, zaman alıcı ve pahalı bir görevdir. Bu hizmet ve ilişkili inceleme araçlarıyla, en iyi sonuçları verimli ve uygun maliyetli bir şekilde elde etmek için makine destekli ılımlılığı döngü içinde insan özellikleriyle birleştirirsiniz. Daha fazla bilgi edinmek için [Azure İçerik Moderatörü makalesiyle videolarınızı](media-services-content-moderation.md) İşleme'ye bakın.

## <a name="common-scenarios"></a>Genel senaryolar
Media Analytics, kuruluşların ve işletmelerin videodan yeni bilgiler elde edinmelerine ve büyük hacimli video içeriğini daha etkili bir şekilde yönetmelerine yardımcı olabilir. Aşağıda birkaç senaryo verilmiştir:

* **Çağrı merkezleri.** Sosyal medyanın gelişiyle bile, müşteri çağrı merkezleri hala müşteri hizmetleri işlemlerinin büyük bir yüzdesini kolaylaştırmak. Bu ses verilerinde kodlanmış daha yüksek müşteri memnuniyeti elde etmek için analiz edilebilir müşteri bilgileri büyük miktarda. Kuruluşlar, Media Indexer'ı kullanarak metin ayıklayabilir ve arama dizinleri ve panolar oluşturabilir. Daha sonra ortak şikayetler, şikayet kaynakları ve diğer ilgili veriler etrafında istihbarat ayıklayabilirsiniz.
* **Kullanıcı tarafından oluşturulan içerik moderasyonu.** Haber medyalarından polis departmanlarına kadar birçok kuruluşun, video ve görüntü gibi kullanıcı tarafından oluşturulan medyayı kabul eden kamuya açık portalları vardır. Beklenmeyen olaylar nedeniyle içerik hacmi artabilir. Bu senaryolarda, uygunluk için içeriğin etkili manuel incelemelerini yapmak zordur. Müşteriler, uygun içeriğe odaklanmak için içerik denetleme hizmetine güvenebilir.

## <a name="media-analytics-media-processors"></a>Media Analytics medya işlemcileri
Bu bölümde Media Analytics medya işlemcileri listelenir ve bir medya işlemcisi (MP) nesnesi almak için .NET veya REST'in nasıl kullanılacağını gösterir.

### <a name="mp-names"></a>MP adları

* Azure Media Indexer
* Azure Media Face Detector
* Azure Media Motion Detector
* Azure Media Video Thumbnails
* Azure Media OCR
* Azure Medya İçerik Moderatör

### <a name="net"></a>.NET
Aşağıdaki işlev belirtilen MP adlarından birini alır ve bir MP nesnesini döndürür.

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }


### <a name="rest"></a>REST
İstek:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.19
    Host: media.windows.net

Yanıt:

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

## <a name="demos"></a>Demolar
Bkz. [Azure Medya Analizi demoları.](https://azuremedialabs.azurewebsites.net/demos/Analytics.html)

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>İlgili makaleler:
[Bkz. Medya Hizmetleri Analizi duyurusu.](https://azure.microsoft.com/blog/introducing-azure-media-analytics/)

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]
