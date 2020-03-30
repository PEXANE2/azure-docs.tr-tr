---
title: Azure portalını kullanarak medyanızı analiz edin | Microsoft Dokümanlar
description: Bu konu, Azure portalını kullanarak Media Analytics medya işlemcileri (milletvekilleri) ile medyanızın nasıl işlenir olduğunu tartışır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 0e49e90209c7337081458b7c214d27b37d3b4da1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74462621"
---
# <a name="analyze-your-media-using-the-azure-portal"></a>Azure portalını kullanarak medyanızı analiz edin 

> [!IMPORTANT]
> Bazı medya işlemcilerinin [emeklilik planlarını](media-services-analytics-overview.md#retirement-plans) gözden geçirin.

## <a name="overview"></a>Genel Bakış
Azure Media Services Analytics, kuruluşların ve işletmelerin video dosyalarından işlem yapılabilir öngörüler elde etmesini kolaylaştıran bir konuşma ve görme bileşenleri koleksiyonudur (kurumsal ölçekte, uyumluluk, güvenlik ve küresel erişim). Azure Medya Hizmetleri Analizi'ne daha ayrıntılı bir bakış için [bu](media-services-analytics-overview.md) konuya bakın. 

Bu konu, Azure portalını kullanarak Media Analytics medya işlemcileri (milletvekilleri) ile medyanızın nasıl işlenir olduğunu tartışır. Media Analytics milletvekilleri MP4 veya JSON dosyaları üretir. Bir ortam işlemcisi bir MP4 dosyası ürettiyse, dosyayı aşamalı olarak karşıdan yükleyin. Bir ortam işlemcisi bir JSON dosyası ürettiyse, dosyayı Azure blob depolamasından karşıdan yüklezsiniz. 

## <a name="choose-an-asset-that-you-want-to-analyze"></a>Çözümlemek istediğiniz bir varlık seçin
1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. **Ayarlar** penceresinde **Varlıklar**’ı seçin.  
   
    ![Videoları analiz etme](./media/media-services-portal-analyze/media-services-portal-analyze001.png)
3. Analiz etmek istediğiniz varlığı seçin ve **Analiz düğmesine** basın.
   
    ![Videoları analiz etme](./media/media-services-portal-analyze/media-services-portal-analyze002.png)
4. Media **Analytics** penceresi ne sahip Process media varlığında işlemciyi seçin. 
   
    Makalenin geri kalanı, her işlemcinin neden ve nasıl kullanılacağını açıklar. 
5. İş icabı **Oluştur'a** basın.

## <a name="azure-media-indexer"></a>Azure Media Indexer
**Azure Media Indexer** medya işlemcisi, medya dosyalarını ve içeriğini aranabilir hale getirmenin yanı sıra kapalı altyazı parçaları oluşturmanıza olanak tanır. Bu bölümde, bu MP için belirttiğiniz seçenekler hakkında bazı ayrıntılar verir.

![Videoları analiz etme](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Dil
Multimedya dosyasında tanınacak doğal dil. Örneğin, İngilizce veya İspanyolca. 

### <a name="captions"></a>Kapalı açıklamalı altyazılar
İçeriğinizden oluşturulacak bir resim yazısı biçimi seçebilirsiniz. Dizin oluşturma işi aşağıdaki biçimlerde kapalı altyazı dosyaları oluşturabilir:  
 
* **TTML**
* **WebVTT**

Bu biçimlerde kapalı altyazı (CC) dosyaları işitme engelli kişiler için ses ve video dosyalarını erişilebilir hale getirmek için kullanılabilir.

### <a name="keywords"></a>Anahtar Sözcükler
Bir anahtar kelime XML dosyası oluşturmak istiyorsanız bu seçeneği belirleyin. Bu dosya, sıklık ve mahsup bilgileriyle birlikte konuşma içeriğinden çıkarılan anahtar kelimeleri içerir.

### <a name="job-name"></a>İş adı
İşi tanımlamanızı sağlayan dostça bir isim. [Bu makalede,](media-services-portal-check-job-progress.md) bir işin ilerlemesini nasıl izleyebilirsiniz açıklanmaktadır. 

### <a name="output-file"></a>Çıktı dosyası
Çıktı içeriğini tanımlamanıza olanak tanıyan samimi bir ad. 

### <a name="speed"></a>Hız
Giriş videosunu hızlandırmak için hızı belirtin. Çıktı, giriş videosunun sabitlenmiş ve zaman aşımına uğramış bir yorumudur.

### <a name="job-name"></a>İş adı
İşi tanımlamanızı sağlayan dostça bir isim. [Bu makalede,](media-services-portal-check-job-progress.md) bir işin ilerlemesini nasıl izleyebilirsiniz açıklanmaktadır. 

### <a name="output-file"></a>Çıktı dosyası
Çıktı içeriğini tanımlamanıza olanak tanıyan samimi bir ad. 

## <a name="azure-media-face-detector"></a>Azure Media Face Detector
**Azure Media Face Detector** ortam işlemcisi (MP), yüz ifadeleri aracılığıyla izleyici katılımını ve tepkisini saymanızı, hareketleri izlemenizi ve hatta ölçmenizi sağlar. Bu hizmet iki özellik içerir: 

* **Yüz algılama**
  
    Yüz algılama, video daki insan yüzlerini bulur ve izler. Birden çok yüz algılanabilir ve daha sonra hareket ederken izlenebilir ve zaman ve konum meta verileri JSON dosyasında döndürülür. İzleme sırasında, kişi ekranda hareket ederken, engellenmiş olsa lar veya kısa bir süre için çerçeveden ayrılsalar bile, aynı yüze tutarlı bir kimlik vermeye çalışır.
  
  > [!NOTE]
  > Bu hizmetler yüz tanıma gerçekleştirmez. Çerçeveyi terk eden veya çok uzun süre engellenen bir kişiye geri döndüklerinde yeni bir kimlik verilir.
  > 
  > 
* **Duygu algılama**
  
    Duygu Algılama, mutluluk, üzüntü, korku, öfke ve daha fazlası dahil olmak üzere tespit edilen yüzlerden alınan birden fazla duygusal özellik üzerinde analiz sağlayan Yüz Algılama Ortam İşlemcisi'nin isteğe bağlı bir bileşenidir. 

![Videoları analiz etme](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Algılama modu
Aşağıdaki modlardan biri işlemci tarafından kullanılabilir:

* yüz algılama
* yüz duygu algılama başına
* toplu duygu algılama

### <a name="job-name"></a>İş adı
İşi tanımlamanızı sağlayan dostça bir isim. [Bu makalede,](media-services-portal-check-job-progress.md) bir işin ilerlemesini nasıl izleyebilirsiniz açıklanmaktadır. 

### <a name="output-file"></a>Çıktı dosyası
Çıktı içeriğini tanımlamanıza olanak tanıyan samimi bir ad. 

## <a name="azure-media-motion-detector"></a>Azure Media Motion Detector
**Azure Medya Hareket Dedektörü** ortam işlemcisi (MP), uzun ve olaysız bir video içinde ilgi çekici bölümleri verimli bir şekilde belirlemenize olanak tanır. Hareket algılama, hareketin gerçekleştiği video bölümlerini belirlemek için statik kamera görüntülerinde kullanılabilir. Zaman damgaları ve olayın oluştuğu sınırlayıcı bölge içeren bir meta veri içeren bir JSON dosyası oluşturur.

Güvenlik video beslemelerine yönelik olan bu teknoloji, hareketi ilgili olaylara ve gölgeler ve Aydınlatma değişiklikleri gibi yanlış pozitif lere göre kategorize edebilir. Bu, son derece uzun güvenlik videolarından ilgi anları ayıklamak mümkün olurken, sonsuz alakasız olaylar ile spamolmadan kamera beslemeleri güvenlik uyarıları oluşturmak için izin verir.

![Videoları analiz etme](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Azure Media Video Thumbnails
Bu işlemci, kaynak videodan ilginç parçacıkları otomatik olarak seçerek uzun video özetleri oluşturmanıza yardımcı olabilir. Bu, uzun bir videoda neler beklendiğine hızlı bir genel bakış sağlamak istediğinizde yararlıdır. Ayrıntılı bilgi ve örnekler için, [Video Özetleme oluşturmak için Azure Medya Video Küçük Resimlerini Kullanma'ya](media-services-video-summarization.md) bakın

![Videoları analiz etme](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>İş adı
İşi tanımlamanızı sağlayan dostça bir isim. [Bu makalede,](media-services-portal-check-job-progress.md) bir işin ilerlemesini nasıl izleyebilirsiniz açıklanmaktadır. 

### <a name="output-file"></a>Çıktı dosyası
Çıktı içeriğini tanımlamanıza olanak tanıyan samimi bir ad. 

## <a name="azure-media-content-moderator"></a>Azure Medya İçerik Moderatör
Bu işlemci, videolardaki olası yetişkinlere uygun ve müstehcen içeriği algılamanıza yardımcı olur. İşlemci, videodaki çekimleri ve anahtar kareleri otomatik olarak algılar. Olası yetişkinlere uygun veya müstehcen içerik için anahtar kareleri puanlar ve varsayılan eşiklere dayalı değerlendirmeler önerir. Ayrıntılı bilgi ve örnekler [için, videoları ortalamak için Azure Medya İçerik Moderatörü'ne bakın](media-services-content-moderation.md)

![Orta videolar](./media/media-services-portal-analyze/media-services-portal-analyze-content-moderator.PNG)

### <a name="version"></a>Sürüm 
"2.0"ı kullanın.

### <a name="mode"></a>Mod
2.0 sürümü `Mode` ayarı yoksay.

## <a name="next-steps"></a>Sonraki adımlar
Medya Hizmetleri öğrenme yollarını görüntüleyin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
