---
title: Azure Medya Hizmetlerini Kullanarak Canlı Akışa Genel Bakış | Microsoft Dokümanlar
description: Bu makalede, Microsoft Azure Medya Hizmetleri kullanılarak canlı akışa genel bir bakış sunulur.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: fb63502e-914d-4c1f-853c-4a7831bb08e8
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 99efe375fad142963214b09df24be70bc3bc9d99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77131611"
---
# <a name="overview-of-live-streaming-using-media-services"></a>Medya Hizmetlerini Kullanarak Canlı Akışa Genel Bakış

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

## <a name="overview"></a>Genel Bakış

Azure Medya Hizmetleri ile canlı akış etkinlikleri sunarken aşağıdaki bileşenler sık sık yer almaktadır:

* Etkinliği yayınlamak için kullanılan bir kamera.
* Kameradan gelen sinyalleri bir canlı akış hizmetine gönderilen akışlara dönüştüren gerçek zamanlı bir video kodlayıcısı.

    İsteğe bağlı olarak, birden çok, gerçek zamanlı, zaman eşitlenmiş kodlayıcı. Yüksek kullanılabilirlik ve üstün kaliteli bir deneyim gerektiren bazı önemli etkinliklerde, veri kaybı olmadan sorunsuz yük devretme elde etmek için zaman eşitlemeli aktif-aktif yedekli kodlayıcılar kullanılması önerilir.
* Aşağıdakileri yapmanıza olanak sağlayan bir canlı akış hizmeti:

  * çeşitli canlı akış protokollerini (RTMP veya Kesintisiz Akış gibi) kullanarak canlı içerik alma,
  * (isteğe bağlı olarak) akışınızı, bit hızı uyarlamalı akışa kodlama,
  * canlı akışınızı önizleme,
  * alınan içeriği daha sonra akışla aktarmak üzere kaydedip depolama (İsteğe Bağlı Video),
  * içeriği yaygın akış protokolleri (örneğin MPEG DASH, Kesintisiz, HLS) aracılığıyla doğrudan müşterilerinize veya başkalarına dağıtım için bir Content Delivery Network’e (CDN) teslim etme.

**Microsoft Azure Media Services** (AMS) canlı akış içeriğinizi alma, kodlama, önizleme, depolama ve teslim etme olanağı sağlar.

Medya Hizmetleri ile, canlı akışlarınızı MPEG DASH, HLS ve Sorunsuz Akış biçimlerinde hizmete gönderilen katkı akışından yayınlamanızı sağlayan [dinamik ambalajlardan](media-services-dynamic-packaging-overview.md)yararlanabilirsiniz. Görüntüleyenleriniz canlı yayını herhangi bir HLS, DASH veya Smooth Streaming uyumlu oyuncuyla oynatabilir. Bu protokollerden herhangi birinde akışınızı sağlamak için web veya mobil uygulamalarınızda Azure Media Player'ı kullanabilirsiniz.

> [!NOTE]
> 12 Mayıs 2018'den itibaren canlı kanallar RTP/MPEG-2 aktarım akışı protokolünü desteklemeyedevam edecektir. Lütfen RTP/MPEG-2'den RTMP'ye veya parçalanmış MP4 (Düzgün Akış) yutma protokollerine geçiş yapmayın.

## <a name="streaming-endpoints-channels-programs"></a>Akış Uç Noktaları, Kanallar, Programlar

Azure Media Services’de **Kanallar**, **Programlar** ve **Akış Uç Noktaları**; alma biçimlendirme, DVR, güvenlik, ölçeklenebilirlik ve yedeklilik dahil olmak üzere tüm canlı akış işlevlerini idare eder.

**Kanal**, canlı akış içeriğinin işleneceği bir işlem hattını temsil eder. Kanal aşağıdaki yollarla bir canlı girdi akışı alabilir:

* Şirket içi bir gerçek zamanlı kodlayıcı, çoklu bit hızına sahip **RTMP** veya **Kesintisiz Akışı** (parçalanmış MP4) **doğrudan geçiş** teslimi için yapılandırılmış Kanala gönderir. **Doğrudan geçiş** teslimi, alınan akışların herhangi başka bir işlemeye uğramadan **Kanallardan** geçmesidir. Multi-bitrate Smooth Streaming: MediaExcel, Ateme, Imagine Communications, Envivio, Cisco ve Elemental çıktıaşağıdaki canlı kodlayıcılar kullanabilirsiniz. Telestream Wirecast, Haivision, Teradek ve Tricaster transcoders: Aşağıdaki canlı kodlayıcılar çıkış RTMP.  Gerçek zamanlı bir kodlayıcı, gerçek zamanlı kodlama için etkinleştirilmemiş bir kanala tek bit hızlı bir akış da gönderebilir, ancak bu işlem önerilmez. İstendiğinde, Media Services akışı müşterilere teslim eder.

  > [!NOTE]
  > Uzun bir dönem içerisinde birden çok etkinlik gerçekleştirecekseniz ve zaten şirket içi kodlayıcılara yatırım yaptıysanız, doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) detaylarına bakın.
  > 
  > 
* Şirket içi canlı kodlayıcı, Medya Hizmetleri ile canlı kodlama yı aşağıdaki biçimlerden birinde gerçekleştirebilen tek bit hızında bir akış gönderir: RTMP veya Düzgün Akış (parçalanmış MP4). RTMP çıkışı olan aşağıdaki canlı kodlayıcıların bu tür kanallarla çalıştıkları bilinmektedir: Telestream Wirecast. Ardından Kanal, gelen tek bit hızlı akışın çoklu bit hızlı (uyarlamalı) bir video akışına gerçek zamanlı kodlanmasını gerçekleştirir. İstendiğinde, Media Services akışı müşterilere teslim eder.

Medya Hizmetleri 2.10 sürümünden başlayarak, bir Kanal oluşturduğunuzda, kanalınızın giriş akışını hangi şekilde almasını istediğinizi ve kanalın akışınızın canlı kodlamasını gerçekleştirip gerçekleştirmesini isteyip istemediğiniz belirtebilirsiniz. İki seçeneğiniz vardır:

* **Yok** (geçiş) – Çok bit hızında akış (geçiş akışı) çıkacak şirket içi canlı kodlayıcı kullanmayı planlıyorsanız, bu değeri belirtin. Bu durumda, gelen akış herhangi bir kodlama olmadan çıktıgeçti. Bu, 2.10 sürümünden önce bir Kanalın davranışıdır.  
* **Standart** – Tek bit hızındaki canlı akışınızı çoklu bit hızında akışa kodlamak için Medya Hizmetlerini kullanmayı planlıyorsanız bu değeri seçin. Bu yöntem, seyrek olaylar için hızlı bir şekilde ölçekleme için daha ekonomiktir. Canlı kodlama için bir faturalandırma etkisi olduğunu unutmayın ve "Çalışan" durumunda canlı bir kodlama kanalı bırakarak fatura ücretleri tabi olacağını unutmayın.  Ekstra saatlik ücretödememek için canlı akış etkinliğiniz tamamlandıktan sonra çalışan kanallarınızı derhal durdurmanız önerilir.

## <a name="comparison-of-channel-types"></a>Kanal Türlerinin Karşılaştırılması

Aşağıdaki tablo, Medya Hizmetlerinde desteklenen iki Kanal türünü karşılaştırmak için bir kılavuz sağlar

| Özellik | Geçiş Kanalı | Standart Kanal |
| --- | --- | --- |
| Tek bit hızı girişi bulutta birden çok bit hızına kodlanır |Hayır |Evet |
| Maksimum çözünürlük, katman sayısı |1080p, 8 katman, 60+fps |720p, 6 katman, 30 fps |
| Giriş protokolleri |RTMP, Düzgün Akış |RTMP, Düzgün Akış |
| Fiyat |Fiyatlandırma [sayfasına](https://azure.microsoft.com/pricing/details/media-services/) bakın ve "Canlı Video" sekmesine tıklayın |Fiyatlandırma [sayfasına](https://azure.microsoft.com/pricing/details/media-services/) bakın |
| Maksimum çalışma süresi |24x7 |8 saat |
| Kayrak ekleme desteği |Hayır |Evet |
| Reklam sinyali desteği |Hayır |Evet |
| Pass-through CEA 608/708 altyazılar |Evet |Evet |
| Tek tip olmayan giriş GOP'ları için destek |Evet |Hayır – giriş 2sn GOS sabit olmalıdır |
| Değişken kare hızı girişi desteği |Evet |Hayır – giriş sabit kare hızı olmalıdır.<br/>Küçük varyasyonlar, örneğin, yüksek hareket sahneleri sırasında tolere edilir. Ancak kodlayıcı 10 kare/sn'ye düşemez. |
| Giriş akışı kaybolduğunda Kanalların otomatik olarak kapatılması |Hayır |12 saat sonra, çalışan bir Program yoksa |

## <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Şirket içi kodlayıcılardan çoklu bit hızlı canlı akış alan Kanallar ile çalışma (doğrudan geçiş)

Aşağıdaki diyagramda, AMS platformunun **doğrudan geçiş** iş akışında rol oynayan başlıca parçaları gösterilmektedir.

![Canlı iş akışı](./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png)

Daha fazla bilgi için bkz. [Şirket İçi Kodlayıcılardan Çoklu Bit Hızlı Canlı Akış Alan Kanallar ile Çalışma](media-services-live-streaming-with-onprem-encoders.md).

## <a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Azure Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş Kanallar ile çalışma

Aşağıdaki diyagramda, AMS platformunun bir Kanalın, Media Services ile kodlama gerçekleştirmek için etkinleştirildiği Canlı Akış iş akışında rol oynayan başlıca parçaları gösterilmektedir.

![Canlı iş akışı](./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png)

Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

## <a name="description-of-a-channel-and-its-related-components"></a>Bir Kanalın ve ilgili bileşenlerinin açıklaması

### <a name="channel"></a>Kanal

Medya Hizmetlerinde, [Kanal](https://docs.microsoft.com/rest/api/media/operations/channel)s canlı akış içeriğinin işlenmesiyle sorumludur. Kanal, daha sonra canlı bir transcoder'a sağladığınız bir giriş bitiş noktası (url yutma) sağlar. Kanal, canlı kodlayıcıdan canlı giriş akışları alır ve bir veya daha fazla StreamingEndpoints üzerinden akış için kullanılabilir hale getirir. Kanallar ayrıca, daha fazla işleme ve teslim edilmeden önce akışınızı önizlemek ve doğrulamak için kullandığınız bir önizleme bitiş noktası (önizleme URL'si) de sağlar.

Kanalı oluştururken en yüksek URL'yi ve önizleme URL'sini alabilirsiniz. Bu URL'leri almak için kanalın başlangıç durumunda olması gerekmez. Canlı bir kodlayıcıdan kanala veri itmeye başlamaya hazır olduğunuzda, kanal başlatılmalıdır. Canlı kodlayıcı veri sindirmeye başladıktan sonra akışınızı önizleyebilirsiniz.

Her Medya Hizmetleri hesabı birden çok Kanal, birden çok Program ve birden çok Akış Son Noktası içerebilir. Bant genişliği ve güvenlik gereksinimlerine bağlı olarak, StreamingEndpoint hizmetleri bir veya daha fazla kanala tahsis edilebilir. Herhangi bir StreamingEndpoint herhangi bir Kanal dan çekebilirsiniz.

Kanal oluştururken, izin verilen IP adreslerini aşağıdaki biçimlerden birinde belirtebilirsiniz: 4 numaralı IpV4 adresi, CIDR adres aralığı.

### <a name="program"></a>Program
[Program,](https://docs.microsoft.com/rest/api/media/operations/program) canlı akışta segmentlerin yayımlanmasını ve depolanmasını denetlemenizi sağlar. Kanallar, Programları yönetir. Kanal ve Program arasındaki ilişki, kanalın sürekli bir içerik akışının bulunduğu ve programın bu kanalda zamanlanmış bir olayı kapsadığı geleneksel medyadaki ilişkiye benzer.
**ArchiveWindowLength** özelliğini ayarlayarak program için kaydedilen içeriği saklamak istediğiniz saat sayısını belirtebilirsiniz. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir.

ArchiveWindowLength, istemcilerin geçerli canlı konumdan zamanında isteyebileceği maksimum süreyi de belirler. Olaylar belirtilen süre miktarından uzun sürebilir, ancak pencere uzunluğunun gerisine düşen içerik sürekli olarak atılır. Bu özelliğin değeri, istemci bildirimlerinin ne kadar büyüyebileceğini de belirler.

Her program bir Varlık ile ilişkilidir. Programı yayımlamak için ilişkili varlık için bir bulucu oluşturmanız gerekir. Bu bulucuya sahip olmak, istemcilerinize sağlayabileceğiniz bir akış URL’si oluşturmanıza olanak tanır.

Bir kanal eşzamanlı çalışan üçe kadar olayı destekler, böylece aynı gelen akışın birden fazla arşivini oluşturabilirsiniz. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Örneğin, iş gereksiniminiz bir programın 6 saatini arşivlemek ancak son 10 dakikasını yayınlamak olabilir. Bunu yapmak için, eşzamanlı olarak çalışan iki program oluşturmanız gerekir. Bir program olayı 6 saat arşivlemek için ayarlanır ancak program yayımlanmaz. Diğer program 10 dakika arşivlenecek şekilde ve bu program yayımlanır.

## <a name="billing-implications"></a>Faturalama Nın Etkileri
Bir kanal, API üzerinden "Running"e geçiş ler olur olmaz faturalandırmaya başlar.  

Aşağıdaki tablo, Kanal devletlerinin API ve Azure portalındaki faturalandırma durumlarını nasıl eşleşdireceklerini gösterir. Durumların API ve Portal UX arasında biraz farklı olduğunu unutmayın. Bir kanal API üzerinden "Çalışma" durumuna veya Azure portalındaki "Hazır" veya "Akış" durumuna girer bağlanmaz, faturalandırma etkin olur.

Kanal'ın size daha fazla fatura landırmasını engellemek için, API üzerinden veya Azure portalında Kanalı Durdurmanız gerekir.
Kanalla işiniz bittiğinde kanallarınızı durdurmaksizin sorumlusunuz. Kanalın durdurulamaması faturalandırmanın devam ına neden olur.

> [!NOTE]
> Standart kanallarla çalışırken, AMS giriş akışı kaybolduktan 12 saat sonra hala "Çalışıyor" durumunda olan tüm Kanalları otomatik olarak kapatır ve çalışan Program yoktur. Ancak, Kanal'ın "Çalışan" durumunda olduğu süre için faturalandırılırsınız.
>
>

### <a name="channel-states-and-how-they-map-to-the-billing-mode"></a><a id="states"></a>Kanal durumları ve faturalandırma moduna nasıl eşlediklerini
Bir Kanalın geçerli durumu. Olası değerler şunlardır:

* **Durduruldu.** Bu, Oluşturulduktan sonra Kanalın ilk durumudur (portalda otomatik başlatma seçilmediği sürece.) Bu durumda faturalandırma oluşmaz. Bu durumda, Kanal özellikleri güncelleştirilebilir, ancak akışa izin verilmez.
* **Başlangıç**. Kanal başlatılıyor. Bu durumda faturalandırma oluşmaz. Bu durumda güncelleştirmeveya akışa izin verilmez. Bir hata oluşursa, Kanal Durdurulan duruma geri döner.
* **Koşmak**. Kanal canlı akışları işleyebilecek kapasitededir. Şimdi fatura kullanımı. Daha fazla faturalandırmayı önlemek için kanalı durdurmanız gerekir.
* **Durmak**. Kanal durduruluyor. Bu geçici durumda faturalandırma oluşmaz. Bu durumda güncelleştirmeveya akışa izin verilmez.
* **Silme**. Kanal silinmiş. Bu geçici durumda faturalandırma oluşmaz. Bu durumda güncelleştirmeveya akışa izin verilmez.

Aşağıdaki tablo, Kanal durumlarını faturalandırma moduyla nasıl eşleşdireceklerini gösterir.

| Kanal durumu | Portal UI Göstergeleri | Billing mi? |
| --- | --- | --- |
| Başlatılıyor |Başlatılıyor |Hayır (geçici durum) |
| Çalışıyor |Hazır (çalışan programlar yok)<br/>or<br/>Akış (en az bir çalışan program) |EVET |
| Durduruluyor |Durduruluyor |Hayır (geçici durum) |
| Durduruldu |Durduruldu |Hayır |

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>İlgili konular
[Azure Medya Hizmetleri Parçalı MP4 Canlı Yutma Belirtimi](../media-services-fmp4-live-ingest-overview.md)

[Azure Medya Hizmetleriyle Canlı Kodlama Gerçekleştirme özelliğine sahip Kanallarla Çalışma](media-services-manage-live-encoder-enabled-channels.md)

[Şirket İçi Kodlayıcılardan Çoklu Bit Hızlı Canlı Akış Alan Kanallar ile Çalışma](media-services-live-streaming-with-onprem-encoders.md)

[Kotalar ve sınırlamalar](media-services-quotas-and-limitations.md).  

[Medya Hizmetleri Kavramları](media-services-concepts.md)
