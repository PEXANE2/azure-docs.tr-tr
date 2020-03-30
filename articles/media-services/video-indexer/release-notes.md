---
title: Azure Medya Hizmetleri Video Dizinleyici sürüm notları | Microsoft Dokümanlar
description: En son gelişmelerden haberdar olmak için bu makale, Azure Medya Hizmetleri Video Dizinleyicisi ile ilgili en son güncelleştirmeleri sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: na
ms.topic: article
ms.date: 01/07/2020
ms.author: juliako
ms.openlocfilehash: f1387273f9736fea70682177d5d48dc2f141bbad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76933852"
---
# <a name="azure-media-services-video-indexer-release-notes"></a>Azure Medya Hizmetleri Video Dizinleyici sürüm notları

>RsS özet akışı okuyucunuza bu URL'yi `https://docs.microsoft.com/api/search/rss?search=%22Azure+Media+Services+Video+Indexer+release+notes%22&locale=en-us` kopyalayıp yapıştırarak güncellemeler için bu sayfayı ne zaman tekrar ziyaret edeceğiz konusunda bilgilendirilin.

En son gelişmelerden haberdar olmak için bu makalede, aşağıdakiler hakkında bilgi verilmektedir:

* En son sürümler
* Bilinen sorunlar
* Hata düzeltmeleri
* Kullanım dışı işlevsellik

## <a name="january-2020"></a>Ocak 2020
 
### <a name="custom-language-support-for-additional-languages"></a>Ek diller için özel dil desteği

Video Indexer şimdi için `ar-SY` özel `en-UK`dil `en-AU` modellerini destekler , , ve (yalnızca API).
 
### <a name="delete-account-timeframe-action-update"></a>Hesap zaman dilimi eylem güncelleştirmesini silme

Hesap silme eylemi artık hesabı 48 saat yerine 90 gün içinde siler.
 
### <a name="new-video-indexer-github-repository"></a>Yeni Video Indexer GitHub deposu

Farklı projelere sahip yeni bir Video Indexer GitHub, kılavuzları ve kod örnekleri başlarken artık kullanılabilir:https://github.com/Azure-Samples/media-services-video-indexer
 
### <a name="swagger-update"></a>Swagger güncelleştirmesi

Video Indexer tek bir Video [Indexer OpenAPI Belirtimi (swagger)](https://api-portal.videoindexer.ai/docs/services/Operations/export?DocumentFormat=OpenApiJson)içine **kimlik doğrulamaları** ve **işlemleri** birleştirdi. Develpers [Video Indexer Geliştirici Portalı'nda](https://api-portal.videoindexer.ai/)API'ler bulabilirsiniz.

## <a name="december-2019"></a>Aralık 2019

### <a name="update-transcript-with-the-new-api"></a>Transkripti yeni API ile güncelleştirin

[Güncelleştirme-Video Dizini](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Index?&pattern=update) API'sini kullanarak transkriptteki belirli bir bölümü güncelleştirin.

### <a name="fix-account-configuration-from-the-video-indexer-portal"></a>Video Indexer portalından hesap yapılandırmasını düzeltme

Şimdi, şu gibi sorunlarla ilgili kendi kendine yardım etmek için Medya Hizmetleri bağlantı yapılandırmasını güncelleştirebilirsiniz: 

* yanlış Azure Medya Hizmetleri kaynağı
* parola değişiklikleri
* Medya Hizmetleri kaynakları abonelikler arasında taşındı  

Hesap yapılandırmasını düzeltmek için, Video Indexer portalında Ayarlar > Hesap sekmesine (sahip olarak) gidin.

### <a name="configure-the-custom-vision-account"></a>Özel görme hesabını yapılandırma

Video Indexer portalını kullanarak ücretli hesaplardaki özel vizyon hesabını yapılandırın (daha önce, bu yalnızca API tarafından desteklenmiştir). Bunu yapmak için Video Indexer portalında oturum açın, Yapılandırılan karakterleri > Animasyon karakterleri > Model Özelleştirme'> yi seçin. 

### <a name="scenes-shots-and-keyframes--now-in-one-insight-pane"></a>Sahneler, çekimler ve anahtar kareler - şimdi tek bir içgörü bölmesinde

Sahneler, çekimler ve anahtar kareler artık daha kolay tüketim ve gezinme için tek bir içgörüyle birleştirildi. İstediğiniz sahneyi seçtiğinizde hangi çekim ve anahtar karelerden oluştuğunu görebilirsiniz. 

### <a name="notification-about-a-long-video-name"></a>Uzun bir video adı hakkında bildirim

Bir video adı 80 karakterden uzunsa, Video Dizinleyici yüklemede açıklayıcı bir hata gösterir.

### <a name="streaming-endpoint-is-disabled-notification"></a>Akış bitiş noktası bildirimi devre dışı bırakılır

Akış bitiş noktası devre dışı bırakıldığında, Video Dizinleyici oynatıcı sayfasında açıklayıcı bir hata gösterir.

### <a name="error-handling-improvement"></a>Hata işleme geliştirme

Durum kodu 409, bir videonun etkin olarak dizine eklenmiş olması durumunda, geçerli yeniden dizin değişikliklerinin kazara geçersiz kılınmasını önlemek için [Artık Yeniden Dizin Video](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Re-Index-Video?https://api-portal.videoindexer.ai/docs/services/Operations/operations/Re-Index-Video?) ve Güncelleştirme Video [Dizini](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Index?) API'lerinden döndürülecektir.

## <a name="november-2019"></a>Kasım 2019
 
* Korece özel dil modelleri desteği

    Video dizinleyici şimdi Hem API ve portal Korece özel dil modellerini destekler .`ko-KR` 
* Konuşma-metin için desteklenen yeni diller (STT)

    Video Indexer API'leri artık Arapça Levanten (ar-SY), İngiliz İngiltere lehçesi (tr-GB) ve Avustralya Lehçesi (tr-AU) stt'yi desteklemede bulunmaktadır.
    
    Video yükleme için, zh-HANS zh-CN yerine, her ikisi de desteklenir ama zh-CN tavsiye edilir ve daha doğru.
    
## <a name="october-2019"></a>Ekim 2019
 
* Galeride animasyonkarakterleri arama

    Animasyonlu karakterleri dizine ekrirken, artık bunları hesabın video kadırgasında arayabilirsiniz. Daha fazla bilgi için [Animasyon karakterleri tanıma'ya](animated-characters-recognition.md)bakın.

## <a name="september-2019"></a>Eylül 2019
 
IBC 2019'da açıklanan birden fazla gelişme:
 
* Animasyonlu karakter tanıma (genel önizleme)

    Grup reklamını algılama yeteneği, özel görme yle tümleştirme yoluyla animasyonlu içerikteki karakterleri tanır. Daha fazla bilgi için [Animasyonkarakter algılama'ya](animated-characters-recognition.md)bakın.
* Çok dilli tanımlama (genel önizleme)

    Ses parçasındaki birden çok dilde segmentleri algılayın ve bunlara dayalı çok dilli bir transkript oluşturun. İlk destek: İngilizce, İspanyolca, Almanca ve Fransızca. Daha fazla bilgi için, [çoklu dil içeriğini otomatik olarak tanımlayın ve yazıya döktü.](multi-language-identification-transcription.md)
* Kişiler ve Konum için adlandırılmış varlık çıkarma

    Doğal dil işleme (NLP) aracılığıyla markaları, konumları ve kişileri konuşma ve görsel metinden ayıklar.
* Editoryal atış türü sınıflandırması

    Yakın çekim, orta çekim, iki atış, iç mekan, açık alan vb. gibi editoryal tiplerle çekim lerin etiketleme Daha fazla bilgi [için, Editoryal çekim türü algılama](scenes-shots-keyframes.md#editorial-shot-type-detection)bakın.
* Konu çıkartma geliştirme - şimdi seviye 2 kapsayan
    
    Konu çıkarım modeli şimdi IPTC taksonomi derin granüler destekler. Azure Media Services'te tüm ayrıntıları okuyun [Yeni AI destekli yenilik.](https://azure.microsoft.com/blog/azure-media-services-new-ai-powered-innovation/)

## <a name="august-2019"></a>Ağustos 2019
 
### <a name="video-indexer-deployed-in-uk-south"></a>Video Indexer İngiltere Güney dağıtılan

Artık İngiltere'nin güney bölgesinde bir Video Indexer ücretli hesap oluşturabilirsiniz.

### <a name="new-editorial-shot-type-insights-available"></a>Yeni Editoryal Shot Type öngörüleri mevcuttur

Video çekimlerine eklenen yeni etiketler, içerik oluşturma iş akışında kullanılan yaygın editoryal ifadeler ile tanımlamak için editoryal "çekim türleri" sağlar: aşırı yakın çekim, closeup, geniş, orta, iki çekim, açık, kapalı, sol yüz ve sağ yüz (Mevcut JSON).

### <a name="new-people-and-locations-entities-extraction-available"></a>Yeni Kişiler ve Konumlar varlıklar çıkarma kullanılabilir

Video Indexer, adı geçen konumları ve kişileri, videonun OCR'si ve transkripsiyonundan doğal dil işleme (NLP) yoluyla tanımlar. Video Indexer, belirli konumların (örneğin, Eyfel Kulesi) veya kişilerin (örneğin, John Doe) bir videoda çağrıldığını anlamak için makine öğrenimi algoritmasını kullanır.

### <a name="keyframes-extraction-in-native-resolution"></a>Yerel çözünürlükte anahtar kare çıkarma

Video Indexer tarafından çıkarılan anahtar kareler videonun orijinal çözünürlükte mevcuttur.
 
### <a name="ga-for-training-custom-face-models-from-images"></a>Resimlerden özel yüz modelleri eğitimi için GA

Önizleme modundan GA'ya taşınan resimlerden alınan eğitim yüzleri (API üzerinden ve portalda kullanılabilir).

> [!NOTE]
> "Önizleme-GA" geçişiyle ilgili fiyatlandırma etkisi yoktur.

### <a name="hide-gallery-toggle-option"></a>Galeri geçiş seçeneğini gizle

Kullanıcı galeri sekmesini portaldan gizlemeyi seçebilir (örnekler sekmesini saklamaya benzer).
 
### <a name="maximum-url-size-increased"></a>Maksimum URL boyutu artırıldı

Bir videoyu dizine eksinmede 4096 (2048 yerine) URL sorgu dizesi desteği.
 
### <a name="support-for-multi-lingual-projects"></a>Çok dilli projelere destek

Projeler artık farklı dillerde dizine eklenmiş videolara göre oluşturulabilir (yalnızca API).

## <a name="july-2019"></a>Temmuz 2019

### <a name="editor-as-a-widget"></a>Bir widget olarak Editör

Video Indexer AI-editor artık müşteri uygulamalarına katıştırılmış bir widget olarak kullanılabilir.

### <a name="update-custom-language-model-from-closed-caption-file-from-the-portal"></a>Portaldan kapalı resim yazısı dosyasından özel dil modelini güncelleştirme

Müşteriler portalın özelleştirme sayfasında dil modelleri için giriş olarak VTT, SRT ve TTML dosya biçimleri sağlayabilir.

## <a name="june-2019"></a>Haziran 2019

### <a name="video-indexer-deployed-to-japan-east"></a>Video Indexer Japonya Doğu konuşlandırıldı

Artık Japonya Doğu bölgesinde bir Video Indexer ücretli hesap oluşturabilirsiniz.

### <a name="create-and-repair-account-api-preview"></a>Hesap API'si oluşturma ve onarma (Önizleme)

Azure Medya Hizmeti bağlantı bitiş [noktasını veya anahtarı güncelleştirmenizi](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Paid-Account-Azure-Media-Services?&groupBy=tag)sağlayan yeni bir API eklendi.

### <a name="improve-error-handling-on-upload"></a>Yüklemede hata işlemeyi geliştirme 

Temel Azure Medya Hizmetleri hesabının yanlış yapılandırılması durumunda açıklayıcı bir ileti döndürülür.

### <a name="player-timeline-keyframes-preview"></a>Oyuncu zaman çizelgesi Keyframes önizleme 

Artık oyuncunun zaman çizelgesinde her defası için bir görüntü önizlemesi görebilirsiniz.

### <a name="editor-semi-select"></a>Editör yarı-select

Artık editörde belirli bir içgörü zaman dilimi seçmenin bir sonucu olarak seçilen tüm öngörülerin önizlemesini görebilirsiniz.

## <a name="may-2019"></a>Mayıs 2019

### <a name="update-custom-language-model-from-closed-caption-file"></a>Kapalı resim yazısı dosyasından özel dil modelini güncelleştirme

[Özel dil modeli oluşturun](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Language-Model?&groupBy=tag) ve [özel dil modellerini güncelleştir](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Language-Model?&groupBy=tag) API'ler artık dil modelleri için giriş olarak VTT, SRT ve TTML dosya biçimlerini destekler.

[Videoyu Güncelleştir transkript API'sini](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Transcript?&pattern=transcript)ararken, transkript otomatik olarak eklenir. Videoyla ilişkili eğitim modeli de otomatik olarak güncelleştirilir. Dil modellerinizi nasıl özelleştirip eğitin, [video dizinleyici ile Dil Modelini Özelleştir'e](customize-language-model-overview.md)bakın.

### <a name="new-download-transcript-formats--txt-and-csv"></a>Yeni indirme transkript biçimleri – TXT ve CSV

Video Indexer, zaten desteklenen kapalı altyazı biçimine (SRT, VTT ve TTML) ek olarak, transkriptin TXT ve CSV biçimlerinde indirilmesine de destek veriyor.

## <a name="next-steps"></a>Sonraki adımlar

[Genel bakış](video-indexer-overview.md)
