---
title: Çok bit hızında akışlar oluşturan şirket içi kodlayıcılarla canlı akış akışı - Azure | Microsoft Dokümanlar
description: Bu konu, şirket içi bir kodlayıcıdan çok bit hızında canlı akış alan bir kanalın nasıl ayarlanış edilebildiğini açıklar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: d9f0912d-39ec-4c9c-817b-e5d9fcf1f7ea
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: f6366f162cb09898b694b14440718401c57c0adf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251043"
---
# <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders"></a>Şirket içi kodlayıcılardan çok bit hızında canlı akış alan Kanallarla çalışma

> [!NOTE]
> 12 Mayıs 2018'den itibaren canlı kanallar RTP/MPEG-2 aktarım akışı protokolünü desteklemeyedevam edecektir. Lütfen RTP/MPEG-2'den RTMP'ye veya parçalanmış MP4 (Düzgün Akış) yutma protokollerine geçiş yapmayın.

## <a name="overview"></a>Genel Bakış
Azure Medya Hizmetleri'nde bir *kanal,* canlı akış içeriğini işlemek için bir ardışık hattı temsil eder. Bir kanal canlı giriş akışlarını iki şekilde alır:

* Şirket içinde canlı kodlayıcı, Medya Hizmetleri ile canlı kodlama gerçekleştirmesi etkinleştirilmeyen kanala çok bit hızında RTMP veya Düzgün Akış (parçalanmış MP4) akışı gönderir. Yutulan akışlar başka bir işleme gerek kalmadan kanallardan geçer. Bu *yönteme geçiş*denir. Canlı kodlayıcı, canlı kodlama için etkinleştirilmeyen bir kanala tek bit hızında akış da gönderebilir, ancak bunu önermiyoruz. Medya Hizmetleri akışı isteyen müşterilere sunar.

  > [!NOTE]
  > Geçiş yöntemi kullanmak, canlı akış yapmanın en ekonomik yoludur.


* Şirket içi canlı kodlayıcı, Medya Hizmetleri ile canlı kodlama yı aşağıdaki biçimlerden birinde gerçekleştirebilen kanala tek bit hızında bir akış gönderir: RTMP veya Düzgün Akış (parçalanmış MP4). Kanal daha sonra gelen tek bit hızındaki akışın çok bit hızında (uyarlanabilir) bir video akışına canlı kodlamasını gerçekleştirir. Medya Hizmetleri akışı isteyen müşterilere sunar.

Medya Hizmetleri 2.10 sürümünden başlayarak, bir kanal oluşturduğunuzda, kanalınızın giriş akışını nasıl almasını istediğinizi belirtebilirsiniz. Kanalın akışınızın canlı kodlamasını gerçekleştirmesini isteyip istemediğinide de belirtebilirsiniz. İki seçeneğiniz vardır:

* **Geçiş**: Çıktı olarak çok bit hızında bir akışı (geçiş akışı) olan şirket içi canlı kodlayıcı kullanmayı planlıyorsanız bu değeri belirtin. Bu durumda, gelen akış herhangi bir kodlama olmadan çıktıgeçer. Bu, 2.10 sürümünden önce bir kanalın davranışıdır. Bu makalede, bu tür kanallarla çalışma hakkında ayrıntılı bilgi verir.
* **Canlı Kodlama**: Tek bit hızındaki canlı akışınızı çok bit hızında bir akışa kodlamak için Medya Hizmetlerini kullanmayı planlıyorsanız bu değeri seçin. **Çalışan** bir durumda canlı bir kodlama kanalı bırakmak faturalandırma ücretlerine neden olabilir. İlave saatlik ücretlendirmeleri önlemek için canlı yayın etkinliğiniz tamamlandıktan sonra çalışan kanallarınızı derhal durdurmanızı öneririz. Medya Hizmetleri akışı isteyen müşterilere sunar.

> [!NOTE]
> Bu makalede, canlı kodlama gerçekleştirmek için etkin olmayan kanalların öznitelikleri anlatılmaktadır. Canlı kodlama gerçekleştirmesi etkin olan kanallarla çalışma hakkında bilgi [için, çok bit hızında akışlar oluşturmak için Azure Medya Hizmetleri'ni kullanarak Canlı akış](media-services-manage-live-encoder-enabled-channels.md)bölümüne bakın.
>
>Tesislerinde kodlayıcılar hakkında daha fazla bilgi [için,](media-services-recommended-encoders.md)bkz.

Aşağıdaki diyagram, çıktı olarak çok bit hızında RTMP veya parçalanmış MP4 (Düzgün Akış) akışlarına sahip olmak için şirket içi canlı kodlayıcı kullanan canlı akış lı iş akışını temsil eder.

![Canlı iş akışı][live-overview]

## <a name="common-live-streaming-scenario"></a><a id="scenario"></a>Yaygın canlı akış senaryosu
Aşağıdaki adımlar, ortak canlı akış uygulamaları oluşturmayla ilgili görevleri açıklar.

1. Bilgisayara bir video kamera bağlayın. Çıktı olarak çok bit hızında RTMP veya parçalanmış MP4 (Düzgün Akış) akışına sahip şirket içi canlı kodlayıcıyı başlatın ve yapılandırın. Daha fazla bilgi için bkz. [Azure Media Services RTMP Desteği ve Gerçek Zamanlı Kodlayıcılar](https://go.microsoft.com/fwlink/?LinkId=532824).

    Kanalınızı oluşturduktan sonra bu adımı da gerçekleştirebilirsiniz.
2. Bir kanal oluşturup başlatın.

3. Kanal URL'sini alın.

    Canlı kodlayıcı, akışı kanala göndermek için en çok yutulan URL'yi kullanır.
4. Kanal önizleme URL'sini alın.

    Kanalınızın canlı akışı düzgün şekilde aldığını doğrulamak için bu URL’yi kullanın.
5. Bir program oluşturun.

    Azure portalını kullandığınızda, program oluşturmak da bir varlık oluşturur.

    .NET SDK veya REST'i kullandığınızda, bir varlık oluşturmanız ve program oluştururken bu varlığı kullanacağınızı belirtmeniz gerekir.
6. Programla ilişkili varlığı yayımlayın.   

    >[!NOTE]
    >Azure Medya Hizmetleri hesabınız oluşturulduğunda, **Durduruldu** durumunda hesabınıza **varsayılan** bir akış bitiş noktası eklenir. İçerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumunda olması gerekir.

7. Akış ve arşivlemeye başlamaya hazır olduğunuzda programı başlatın.

8. İsteğe bağlı olarak, gerçek zamanlı kodlayıcıya bir reklam başlatması bildirilebilir. Reklam, çıktı akışına eklenir.

9. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun.

10. Programı silin (ve isteğe bağlı olarak varlığı silin).     

## <a name="description-of-a-channel-and-its-related-components"></a><a id="channel"></a>Bir kanalın ve ilgili bileşenlerinin açıklaması
### <a name="channel-input-ingest-configurations"></a><a id="channel_input"></a>Kanal girişi (yutma) yapılandırmaları
#### <a name="ingest-streaming-protocol"></a><a id="ingest_protocols"></a>Akış protokolünü yutma
Medya Hizmetleri, akış protokolleri olarak çok bit hızında parçalanmış MP4 ve çok bit hızında RTMP kullanarak canlı akışları sindirenleri destekler. RTMP ingest akış protokolü seçildiğinde, kanal için iki yutma (giriş) uç noktası oluşturulur:

* **Birincil URL**: Kanalın birincil RTMP yutma bitiş noktasının tam nitelikli URL'sini belirtir.
* **İkincil URL** (isteğe bağlı): Kanalın ikincil RTMP yutma bitiş noktasının tam nitelikli URL'sini belirtir.

Özellikle aşağıdaki senaryolar için, yutma akışınızın dayanıklılığını ve hata toleransını (yanı sıra kodlayıcı hatası ve hata toleransı) iyileştirmek istiyorsanız ikincil URL'yi kullanın:

- Birincil ve ikincil URL'lere tek kodlayıcı çift itme:

    Bu senaryonun temel amacı, ağ dalgalanmalarına ve gerginliklere karşı daha fazla esneklik sağlamaktır. Bazı RTMP kodlayıcıları ağ bağlantılarını iyi işlemez. Ağ bağlantısı kesildiğinde, bir kodlayıcı kodlamayı durdurabilir ve yeniden bağlanma olduğunda arabelleğe alınan verileri göndermeyebilir. Bu, süreksizliklere ve veri kaybına neden olur. Ağ bağlantıları, bozuk bir ağ veya Azure tarafında bakım nedeniyle gerçekleşebilir. Birincil/ikincil URL'ler ağ sorunlarını azaltır ve kontrollü bir yükseltme işlemi sağlar. Zamanlanan bir ağ bağlantısı nın kesilmesi her gerçekleştiğinde, Medya Hizmetleri birincil ve ikincil bağlantı bağlantılarını yönetir ve ikisi arasında gecikmeli bir bağlantı keser. Kodlayıcılar daha sonra veri göndermeye devam etmek ve yeniden bağlanmak için zamana sahip olur. Bağlantı kesilmelerinin sırası rasgele olabilir, ancak birincil/ikincil veya ikincil/birincil URL'ler arasında her zaman bir gecikme olacaktır. Bu senaryoda, kodlayıcı hala tek hata noktasıdır.

- Birden fazla kodlayıcı, her kodlayıcı özel bir noktaya iterek:

    Bu senaryo hem kodlayıcı sağlar hem de artıklık yutuculuk. Bu senaryoda, encoder1 birincil URL'ye, encoder2 ise ikincil URL'ye iter. Bir kodlayıcı başarısız olduğunda, diğer kodlayıcı veri göndermeye devam edebilir. Medya Hizmetleri birincil ve ikincil URL'lerin bağlantısını aynı anda kesmediği için veri artıklığı korunabilir. Bu senaryo, kodlayıcıların zaman eşitlenmiş olduğunu varsayar ve tam olarak aynı verileri sağlar.  

- Birden fazla kodlayıcı hem birincil hem de ikincil URL'lere çift itme:

    Bu senaryoda, her iki kodlayıcı da verileri hem birincil hem de ikincil URL'lere iter. Bu, en iyi güvenilirlik ve hata toleransı ve veri artıklığı sağlar. Bu senaryo, bir kodlayıcı çalışmayı durdursa bile hem kodlayıcı hatalarını hem de bağlantı bağlantılarını tolere edebilir. Kodlayıcıların zaman senkronize edildiğini ve tam olarak aynı verileri sağladığını varsayar.  

RTMP canlı kodlayıcıları hakkında daha fazla bilgi için [Azure Medya Hizmetleri RTMP Desteği ve Live Encoders'a](https://go.microsoft.com/fwlink/?LinkId=532824)bakın.

#### <a name="ingest-urls-endpoints"></a>URL'leri yutma (uç noktalar)
Kanal, canlı kodlayıcıda belirttiğiniz bir giriş bitiş noktası (URL yutma) sağlar, böylece kodlayıcı akışları kanallarınıza itebilir.   

Kanalı oluştururken en yüksek URL'leri alabilirsiniz. Bu URL'leri alabilmek için kanalın **Çalışan** durumunda olması gerekmez. Verileri kanala itmeye başlamaya hazır olduğunuzda, kanalın **Çalışan** durumunda olması gerekir. Kanal veri sindirilmeye başladıktan sonra, akışınızı önizleme URL'si üzerinden önizleyebilirsiniz.

Parçalanmış bir MP4 (Smooth Streaming) canlı akışını Bir SSL bağlantısı üzerinden sindirebilirsiniz. SSL üzerinden yutmak için, https için yutulan URL'yi güncelleştirdiğinden emin olun. Şu anda, SSL üzerinden RTMP yutamazsınız.

#### <a name="keyframe-interval"></a><a id="keyframe_interval"></a>Anahtar kare aralığı
Çok bit hızında akış oluşturmak için şirket içi canlı kodlayıcı kullandığınızda, anahtar kare aralığı bu harici kodlayıcı tarafından kullanılan resim grubunun (GOP) süresini belirtir. Kanal bu gelen akışı aldıktan sonra, canlı akışınızı aşağıdaki biçimlerden herhangi birinde istemci oynatma uygulamalarına sunabilirsiniz: Http üzerinden Düzgün Akış, Dinamik Uyarlanabilir Akış (DASH) ve HTTP Canlı Akış (HLS). Canlı akış yaparken HLS her zaman dinamik olarak paketlenir. Varsayılan olarak, Medya Hizmetleri canlı kodlayıcıdan alınan anahtar kare aralığını temel alarak HLS segment ambalaj oranını (segment başına parçalar) otomatik olarak hesaplar.

Aşağıdaki tablo, segment süresinin nasıl hesaplandığı gösterilmektedir:

| Anahtar kare aralığı | HLS segment ambalaj oranı (FragmentsPerSegment) | Örnek |
| --- | --- | --- |
| 3 saniyeden az veya eşit |3:1 |KeyFrameInterval (veya GOP) 2 saniye ise, varsayılan HLS segment iması paketleme oranı 3 ile 1 arasındadır. Bu 6 saniyelik HLS segmenti oluşturur. |
| 3 ila 5 saniye |2:1 |KeyFrameInterval (veya GOP) 4 saniye ise, varsayılan HLS segment iması paketleme oranı 2 ile 1 arasındadır. Bu 8 saniyelik HLS segmenti oluşturur. |
| 5 saniyeden büyük |1:1 |KeyFrameInterval (veya GOP) 6 saniye ise, varsayılan HLS segment i Bu 6 saniyelik HLS segmenti oluşturur. |

Kanalın çıktısını yapılandırarak ve KanalOutputHls'te FragmentsPerSegment ayarlayarak parça başına parça oranını değiştirebilirsiniz.

Ayrıca, ChannelInput'ta KeyFrameInterval özelliğini ayarlayarak anahtar kare aralığı değerini de değiştirebilirsiniz. KeyFrameInterval'ı açıkça ayarlarsanız, HLS segment ambalaj oranı FragmentsPerSegment daha önce açıklanan kurallar la hesaplanır.  

Hem KeyFrameInterval hem de FragmentsPerSegment'i açıkça ayarlarsanız, Medya Hizmetleri ayarladığınız değerleri kullanır.

#### <a name="allowed-ip-addresses"></a>İzin verilen IP adresleri
Bu kanalda video yayımlamalarına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen bir IP adresi aşağıdakilerden biri olarak belirtilebilir:

* Tek bir IP adresi (örneğin, 10.0.0.1)
* IP adresi ve CIDR alt ağ maskesi kullanan bir IP aralığı (örneğin, 10.0.0.1/22)
* IP adresi ve noktalı ondalık alt ağ maskesi kullanan bir IP aralığı (örneğin, 10.0.0.1(255.255.252.0))

IP adresi belirtilmemişse ve kural tanımı yoksa, IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.

### <a name="channel-preview"></a>Kanal önizleme
#### <a name="preview-urls"></a>Önizleme URL'leri
Kanallar, daha fazla işleme ve teslim edilmeden önce akışınızı önizlemek ve doğrulamak için kullandığınız bir önizleme bitiş noktası (önizleme URL'si) sağlar.

Kanalı oluştururken önizleme URL'sini alabilirsiniz. URL'yi alabilmek için kanalın **Çalışan** durumunda olması gerekmez. Kanal veri sindirmeye başladıktan sonra akışınızı önizleyebilirsiniz.

Şu anda, önizleme akışı yalnızca parçalanmış MP4 (Düzgün Akış) biçiminde, belirtilen giriş türüne bakılmaksızın teslim edilebilir. Düzgün akışı test etmek için [Düzgün Akış Durumu Monitörü](https://playready.directtaps.net/smoothstreaming/) oynatıcısını kullanabilirsiniz. Akışınızı görüntülemek için Azure portalında barındırılan bir oynatıcıyı da kullanabilirsiniz.

#### <a name="allowed-ip-addresses"></a>İzin verilen IP adresleri
Önizleme bitiş noktasına bağlanmasına izin verilen IP adreslerini tanımlayabilirsiniz. IP adresi belirtilmemişse, herhangi bir IP adresine izin verilir. İzin verilen bir IP adresi aşağıdakilerden biri olarak belirtilebilir:

* Tek bir IP adresi (örneğin, 10.0.0.1)
* IP adresi ve CIDR alt ağ maskesi kullanan bir IP aralığı (örneğin, 10.0.0.1/22)
* IP adresi ve noktalı ondalık alt ağ maskesi kullanan bir IP aralığı (örneğin, 10.0.0.1(255.255.252.0))

### <a name="channel-output"></a>Kanal çıkışı
Kanal çıkışı hakkında bilgi için [Anahtar Kare aralığı](#keyframe_interval) bölümüne bakın.

### <a name="channel-managed-programs"></a>Kanal tarafından yönetilen programlar
Kanal, canlı akıştaki segmentlerin yayımlanmasını ve depolanmasını denetlemek için kullanabileceğiniz programlarla ilişkilidir. Kanallar programları yönetir. Kanal ve program ilişkisi, bir kanalın sürekli bir içerik akışına sahip olduğu ve bir programın o kanaldaki zamanlanmış bir olaya kapsamının bulunduğu geleneksel ortama benzer.

Program için kaydedilen içeriği kaç saat tutmak istediğinizi **Arşiv Penceresi** uzunluğunu ayarlayarak belirleyebilirsiniz. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir. Arşiv pencere uzunluğu, istemcilerin geçerli canlı konumdan zamanında isteyebileceği maksimum süre sayısını da belirler. Olaylar belirtilen süre miktarından uzun sürebilir, ancak pencere uzunluğunun gerisine düşen içerik sürekli olarak atılır. Bu özelliğin bu değeri, istemci bildiriminin ne kadar uzayabileceğini de belirler.

Her program, akışlı içeriği depolayan bir varlıkla ilişkilidir. Bir varlık Azure depolama hesabındaki bir blok blob kapsayıcısına eşlenir ve kıymetteki dosyalar bu kapsayıcıda blob olarak depolanır. Müşterilerinizin akışı görüntüleyebilmeleri için programı yayımlamak için ilişkili varlık için bir OnDemand bulucu oluşturmanız gerekir. Bu bulucu, müşterilerinize sağlayabileceğiniz bir akış URL'si oluşturmak için kullanabilirsiniz.

Bir kanal aynı anda çalışan en fazla üç programı destekler, böylece aynı gelen akışın birden çok arşivini oluşturabilirsiniz. Gerektiğinde bir olayın farklı bölümlerini yayımlayabilir ve arşivleyebilirsiniz. Örneğin, işletmenizin gereksiniminin bir programın 6 saatini arşivlemek, ancak yalnızca son 10 dakikayı yayınlamak olduğunu düşünün. Bunu yapmak için, eşzamanlı olarak çalışan iki program oluşturmanız gerekir. Bir program olayın altı saatini arşivlemek üzere ayarlanır, ancak program yayımlanmaz. Diğer program 10 dakika arşivlemek üzere ayarlanır ve bu program yayımlanır.

Yeni olaylar için mevcut programları yeniden kullanmamalısınız. Bunun yerine, her olay için yeni bir program oluşturun. Akış ve arşivlemeye başlamaya hazır olduğunuzda programı başlatın. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun.

Arşivlenmiş içeriği silmek için programı durdurun ve silin ve ardından ilişkili varlığı silin. Bir program kullanıyorsa bir varlık silinemez. Önce program silinmelidir.

Programı durdurup sildikten sonra bile, siz varlığı silene kadar kullanıcılar arşivlenmiş içeriğinizi isteğe bağlı olarak video olarak aktarabilir. Arşivlenmiş içeriği korumak, ancak akış için kullanılabilir değilseniz, akış bulucusunu silin.

## <a name="channel-states-and-billing"></a><a id="states"></a>Kanal durumları ve faturalandırma
Bir kanalın geçerli durumu için olası değerler şunlardır:

* **Durduruldu**: Bu, oluşturulduktan sonra kanalın ilk durumudur. Bu durumda, kanal özellikleri güncelleştirilebilir, ancak akışa izin verilmez.
* **Başlangıç**: Kanal başlatılıyor. Bu durumda güncelleştirmeveya akışa izin verilmez. Bir hata oluşursa, kanal **Durdurulan** duruma geri döner.
* **Çalışan**: Kanal canlı akışları işleyebilir.
* **Durdurma**: Kanal durduruluyor. Bu durumda güncelleştirmeveya akışa izin verilmez.
* **Silme**: Kanal siliniyor. Bu durumda güncelleştirmeveya akışa izin verilmez.

Aşağıdaki tablo, kanal durumlarını faturalandırma moduyla nasıl eşleşdireceklerini gösterir.

| Kanal durumu | Portal UI göstergeleri | Fatura? |
| --- | --- | --- |
| **Başlatılıyor** |**Başlatılıyor** |Hayır (geçici durum) |
| **Çalışıyor** |**Hazır** (çalışan programlar yok)<p><p>or<p>**Akış** (en az bir çalışan program) |Evet |
| **Durduruluyor** |**Durduruluyor** |Hayır (geçici durum) |
| **Durduruldu** |**Durduruldu** |Hayır |

## <a name="closed-captioning-and-ad-insertion"></a><a id="cc_and_ads"></a>Kapalı altyazı ve reklam ekleme
Aşağıdaki tablo, kapalı altyazı ve reklam ekleme için desteklenen standartları göstermektedir.

| Standart | Notlar |
| --- | --- |
| CEA-708 ve ÇED-608 (708/608) |CEA-708 ve EIA-608, AMERIKA Birleşik Devletleri ve Kanada için kapalı altyazı standartlarıdır.<p><p>Şu anda, altyazı yalnızca kodlanmış giriş akışında taşınırsa desteklenir. Medya Hizmetleri'ne gönderilen kodlanmış akışa 608 veya 708 altyazı ekleyen canlı bir ortam kodlayıcısı kullanmanız gerekir. Medya Hizmetleri, içeriği görüntüleyenlerinize eklenen altyazılarla sunar. |
| TTML içinde .ismt (Düzgün Akış metin parçaları) |Medya Hizmetleri dinamik paketleme, müşterilerinizin aşağıdaki biçimlerden herhangi birinde içerik akışı sağlamasını sağlar: DASH, HLS veya Düzgün Akış. Ancak, .ismt (Düzgün Akış metin parçaları) içinde altyazılar içeren parçalanmış MP4 (Düzgün Akış) yutabilirseniz, akışı yalnızca Düzgün Akış istemcilerine teslim edebilirsiniz. |
| SCTE-35 |SCTE-35, reklam ekleme için kullanılan dijital bir sinyal sistemidir. Downstream alıcıları, ayrılan süre boyunca reklamı akışa aktarmak için sinyali kullanır. SCTE-35 giriş akışında seyrek bir parça olarak gönderilmelidir.<p><p>Şu anda, reklam sinyalleri taşıyan desteklenen tek giriş akışı biçimi parçalanmış MP4 (Düzgün Akış). Desteklenen tek çıkış biçimi de Smooth Streaming'dir. |

## <a name="considerations"></a><a id="considerations"></a>Husus -lar
Bir kanala çok bit hızında akış göndermek için şirket içi canlı kodlayıcı kullanıyorsanız, aşağıdaki kısıtlamalar uygulanır:

* Veri yutma noktalarına göndermek için yeterli ücretsiz Internet bağlantısına sahip olduğundan emin olun.
* İkincil bir yutma URL'si kullanmak için ek bant genişliği gerekir.
* Gelen çok bithızında akış en fazla 10 video kalitesi düzeyine (katman) ve en fazla 5 ses parçasına sahip olabilir.
* Video kalitesi düzeylerinin herhangi biri için en yüksek ortalama bit hızı 10 Mbps'nin altında olmalıdır.
* Tüm video ve ses akışları için ortalama bit hızlarının toplamı 25 Mbps'nin altında olmalıdır.
* Kanal veya ilişkili programları çalışırken giriş iletişim kuralını değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa her bir giriş protokolü için farklı bir kanal oluşturmalısınız.
* Kanalınızda tek bir bit hızıyla yutabilirsiniz. Ancak kanal akışı işlemediği için istemci uygulamaları da tek bir bithızı akışı alır. (Bu seçeneği önermiyoruz.)

Kanallar ve ilgili bileşenlerle çalışmakla ilgili diğer hususlar şunlardır:

* Canlı kodlayıcıyı her yeniden yapılandırdığınızda, kanaldaki **Sıfırlama** yöntemini arayın. Kanalı sıfırlamadan önce programı durdurmanız gerekir. Kanalı sırdamadıktan sonra programı yeniden başlatın.

  > [!NOTE]
  > Programı yeniden başlattığınızda, programı yeni bir varlıkla ilişkilendirmeniz ve yeni bir yer bulucu oluşturmanız gerekir. 
  
* Bir kanal yalnızca **Çalışan** durumundayken ve kanaldaki tüm programlar durdurulduğunda durdurulabilir.
* Varsayılan olarak, Medya Hizmetleri hesabınıza yalnızca beş kanal ekleyebilirsiniz. Daha fazla bilgi için [Kotalar ve sınırlamalar](media-services-quotas-and-limitations.md)bölümüne bakın.
* Yalnızca kanalınız **Çalışan** durumundayken faturalandırılırsınız. Daha fazla bilgi için [Kanal durumları ve faturalandırma bölümüne](media-services-live-streaming-with-onprem-encoders.md#states) bakın.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="feedback"></a>Geri Bildirim
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>İlgili konular
[Şirket kodlayıcılarında önerilir](media-services-recommended-encoders.md)

[Azure Medya Hizmetleri parçalanmış MP4 yaşamları özellikleri yutmak](../media-services-fmp4-live-ingest-overview.md)

[Azure Media Services’e genel bakış ve sıklıkla karşılaşılan senaryolar](media-services-overview.md)

[Medya Hizmetleri kavramları](media-services-concepts.md)

[live-overview]: ./media/media-services-manage-channels-overview/media-services-live-streaming-current.png
