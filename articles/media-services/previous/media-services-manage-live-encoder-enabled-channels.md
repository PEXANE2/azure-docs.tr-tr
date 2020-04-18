---
title: Çok bit hızında akışlar oluşturmak için Azure Medya Hizmetleri'ni kullanarak canlı akış | Microsoft Dokümanlar
description: Bu konu, şirket içi bir kodlayıcıdan tek bit hızıyla canlı akış alan ve ardından Medya Hizmetleri ile uyarlanabilir bitrate akışına canlı kodlama gerçekleştiren bir Kanal'ın nasıl ayarlanacağını açıklar.
services: media-services
documentationcenter: ''
author: anilmur
manager: femila
editor: ''
ms.assetid: 30ce6556-b0ff-46d8-a15d-5f10e4c360e2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: anilmur
ms.reviewer: juliako
ms.openlocfilehash: 6210d6ee4877c6ba84178340cf0a6610e402da31
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641114"
---
# <a name="live-streaming-using-azure-media-services-to-create-multi-bitrate-streams"></a>Azure Media Services aracılığıyla canlı akış gerçekleştirerek çoklu bit hızına sahip akışlar oluşturma

> [!NOTE]
> 12 Mayıs 2018'den itibaren canlı kanallar RTP/MPEG-2 aktarım akışı protokolünü desteklemeyedevam edecektir. Lütfen RTP/MPEG-2'den RTMP'ye veya parçalanmış MP4 (Düzgün Akış) yutma protokollerine geçiş yapmayın.

## <a name="overview"></a>Genel Bakış
Azure Medya Hizmetleri'nde (AMS) bir **Kanal,** canlı akış içeriğini işlemek için bir ardışık hattı temsil eder. **Kanal** canlı giriş akışlarını iki şekilde alır:

* Şirket içi canlı kodlayıcı, Medya Hizmetleri ile canlı kodlama yı aşağıdaki biçimlerden birinde gerçekleştirebilen tek bit hızında bir akış gönderir: RTMP veya Düzgün Akış (Parçalanmış MP4). Ardından Kanal, gelen tek bit hızlı akışın çoklu bit hızlı (uyarlamalı) bir video akışına gerçek zamanlı kodlanmasını gerçekleştirir. İstendiğinde, Media Services akışı müşterilere teslim eder.
* Şirket içinde canlı kodlayıcı, KANALA AMS ile canlı kodlama yapmak için etkinleştirilmeyen çok bit hızında **RTMP** veya **Düzgün Akış** (Parçalanmış MP4) gönderir. Yutulan akışlar başka bir işleme gerek kalmadan **Kanal**s'den geçer. Bu **yönteme geçiş**denir. Multi-bitrate Smooth Streaming: MediaExcel, Ateme, Imagine Communications, Envivio, Cisco ve Elemental çıktıaşağıdaki canlı kodlayıcılar kullanabilirsiniz. Aşağıdaki canlı kodlayıcılar çıkış RTMP: [Telestream Wirecast](media-services-configure-wirecast-live-encoder.md), Haivision, Teradek kodlayıcılar.  Gerçek zamanlı bir kodlayıcı, gerçek zamanlı kodlama için etkinleştirilmemiş bir kanala tek bit hızlı bir akış da gönderebilir, ancak bu işlem önerilmez. İstendiğinde, Media Services akışı müşterilere teslim eder.

  > [!NOTE]
  > Geçiş yöntemi kullanmak, canlı akış yapmanın en ekonomik yoludur.
  > 
  > 

Medya Hizmetleri 2.10 sürümünden başlayarak, bir Kanal oluşturduğunuzda, kanalınızın giriş akışını hangi şekilde almasını istediğinizi ve kanalın akışınızın canlı kodlamasını gerçekleştirip gerçekleştirmesini isteyip istemediğiniz belirtebilirsiniz. İki seçeneğiniz vardır:

* **Yok** – Çok bit hızında akış (geçiş akışı) çıkacak şirket içi canlı kodlayıcı kullanmayı planlıyorsanız, bu değeri belirtin. Bu durumda, gelen akış herhangi bir kodlama olmadan çıktıgeçti. Bu, 2.10 sürümünden önce bir Kanalın davranışıdır.  Bu tür kanallarla çalışma hakkında daha ayrıntılı bilgi için, [çok bit hızında akışlar oluşturan şirket içi kodlayıcılarla Canlı akış](media-services-live-streaming-with-onprem-encoders.md)bölümüne bakın.
* **Standart** – Tek bit hızındaki canlı akışınızı çoklu bit hızında akışa kodlamak için Medya Hizmetlerini kullanmayı planlıyorsanız bu değeri seçin. Canlı kodlama için bir faturalandırma etkisi olduğunu unutmayın ve "Çalışan" durumunda canlı bir kodlama kanalı bırakarak fatura ücretleri tabi olacağını unutmayın.  Ekstra saatlik ücretödememek için canlı akış etkinliğiniz tamamlandıktan sonra çalışan kanallarınızı derhal durdurmanız önerilir.

> [!NOTE]
> Bu konu, canlı kodlama **(Standart** kodlama türü) gerçekleştirmek için etkinleştirilen kanalların özniteliklerini tartışır. Canlı kodlama gerçekleştirmesi etkinleştirilen olmayan kanallarla çalışma hakkında bilgi için, [çok bit hızında akışlar oluşturan şirket içi kodlayıcılarla Canlı akış](media-services-live-streaming-with-onprem-encoders.md)bölümüne bakın.
> 
> [Dikkat Edilmesi Gerekenler](media-services-manage-live-encoder-enabled-channels.md#Considerations) bölümünü gözden geçirin.
> 
> 

## <a name="billing-implications"></a>Faturalama Nın Etkileri
Canlı kodlama kanalı, API üzerinden "Running"e devlet geçişleri olur olmaz faturalandırmaya başlar.   Durumu Azure portalında veya Azure Medya Hizmetleri Gezgini aracındahttps://aka.ms/amse)da görüntüleyebilirsiniz.

Aşağıdaki tablo, Kanal devletlerinin API ve Azure portalındaki faturalandırma durumlarını nasıl eşleşdireceklerini gösterir. Devletler API ve Portal UX arasında biraz farklıdır. Bir kanal API üzerinden "Çalışma" durumuna veya Azure portalındaki "Hazır" veya "Akış" durumuna girer bağlanmaz, faturalandırma etkin olur.
Kanal'ın size daha fazla fatura landırmasını engellemek için, API üzerinden veya Azure portalında Kanalı Durdurmanız gerekir.
Canlı kodlama kanalıyla işiniz bittiğinde kanallarınızı durdurmaksizin sorumlusunuz.  Bir kodlama kanalının durdurulamaması faturalandırmanın devam ına neden olur.

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

### <a name="automatic-shut-off-for-unused-channels"></a>Kullanılmayan Kanallar için otomatik kapatma
Media Services, 25 Ocak 2016'dan itibaren, uzun bir süre kullanılmayan bir durumda çalıştırıldıktan sonra bir Kanalı (canlı kodlama etkinken) otomatik olarak durduran bir güncelleştirme yayınlamıştır. Bu, etkin Programı olmayan ve uzun bir süre için giriş katkısı akışı almamış kanallar için geçerlidir.

Kullanılmayan bir dönemin eşiği nominal olarak 12 saattir, ancak değiştirilebilir.

## <a name="live-encoding-workflow"></a>Canlı Kodlama İş Akışı
Aşağıdaki diyagram, bir kanalın aşağıdaki protokollerden birinde tek bir bit hızı akışı aldığı canlı akış iş akışını temsil eder: RTMP veya Düzgün Akış; daha sonra akışı çok bit hızında bir akışa kodlar. 

![Canlı iş akışı][live-overview]

## <a name="common-live-streaming-scenario"></a><a id="scenario"></a>Ortak Canlı Akış Senaryosu
Yaygın canlı akış uygulamaları oluşturmak için gerekli olan genel adımlar aşağıdadır.

> [!NOTE]
> Canlı bir etkinlik için önerilen en uzun süre şu anda 8 saattir.
>
> Canlı kodlama için bir faturalandırma etkisi vardır ve "Çalışan" durumunda canlı bir kodlama kanalı bırakarak saatlik fatura ücretleri tabi olacağını unutmamalısınız. Ekstra saatlik ücretödememek için canlı akış etkinliğiniz tamamlandıktan sonra çalışan kanallarınızı derhal durdurmanız önerilir. 

1. Bilgisayara bir video kamera bağlayın. RtMP veya Düzgün Akış: Başlat ın ve aşağıdaki protokollerden birinde **tek** bir bithızı akışı çıktı olabilir şirket içi canlı kodlayıcı yapılandırır. 

    Bu adım, Kanalınızı oluşturduktan sonra da gerçekleştirilebilir.
2. Bir Kanal oluşturup başlatın. 
3. Kanal alma URL’sini alın. 

    Alma URL’si gerçek zamanlı kodlayıcı tarafından akışı Kanala göndermek için kullanılır.
4. Kanal önizleme URL’sini alın. 

    Kanalınızın canlı akışı düzgün şekilde aldığını doğrulamak için bu URL’yi kullanın.
5. Bir program oluşturun. 

    Azure portalını kullanırken, program oluşturmak da bir varlık oluşturur. 

    .NET SDK veya REST kullanırken bir varlık oluşturmanız ve program oluştururken bu varlığı kullanacağınızı belirtmeniz gerekir. 
6. Programla ilişkili varlığı yayımlayın.   

    >[!NOTE]
    >AMS hesabınız **oluşturulduğunda,** **Durduruldu** durumunda hesabınıza varsayılan akış bitiş noktası eklenir. İçerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumunda olması gerekir. 

7. Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda programı başlatın.
8. İsteğe bağlı olarak, gerçek zamanlı kodlayıcıya bir reklam başlatması bildirilebilir. Reklam, çıktı akışına eklenir.
9. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun.
10. Programı silin (ve isteğe bağlı olarak varlığı da silin).   

> [!NOTE]
> Canlı Kodlama KanalLarını Durdurmayı unutmamak çok önemlidir. Canlı kodlama için saatlik faturalandırma etkisi olduğunu unutmayın ve "Çalışan" durumunda canlı bir kodlama kanalı bırakmanın faturalandırma ücretlerine tabi olacağını unutmayın.  Ekstra saatlik ücretödememek için canlı akış etkinliğiniz tamamlandıktan sonra çalışan kanallarınızı derhal durdurmanız önerilir. 
> 
> 

## <a name="channels-input-ingest-configurations"></a><a id="channel"></a>Kanal giriş (yutma) yapılandırmaları
### <a name="ingest-streaming-protocol"></a><a id="Ingest_Protocols"></a>Akış protokolünü yutma
**Encoder Türü** **Standart**olarak ayarlanmışsa, geçerli seçenekler şunlardır:

* Tek bit hızı **RTMP**
* Tek bit hızı **Parçalanmış MP4** (Düzgün Akış)

#### <a name="single-bitrate-rtmp"></a><a id="single_bitrate_RTMP"></a>Tek bit hızlı RTMP
Dikkat edilmesi gerekenler:

* Gelen akış çok bit hızında video içeremez
* Video akışı 15 Mbps'nin altında ortalama bir bit hızına sahip olmalıdır
* Ses akışı 1 Mbps'nin altında ortalama bir bit hızına sahip olmalıdır
* Desteklenen codec'ler şunlardır:
* MPEG-4 AVC / H.264 Video
* Taban çizgisi, Ana, Yüksek Profil (8-bit 4:2:0)
* Yüksek 10 Profil (10-bit 4:2:0)
* Yüksek 422 Profil (10-bit 4:2:2)
* MPEG-2 AAC-LC Ses
* Mono, Stereo, Surround (5.1, 7.1)
* 44,1 kHz örnekleme oranı
* MPEG-2 tarzı ADTS ambalaj
* Önerilen kodlayıcılar şunlardır:
* [Telestream Wirecast](media-services-configure-wirecast-live-encoder.md)
* Flash Medya Canlı Encoder

#### <a name="single-bitrate-fragmented-mp4-smooth-streaming"></a>Tek bit hızlı Parçalanmış MP4 (Kesintisiz Akış)
Tipik kullanım örneği:

Elemental Technologies, Ericsson, Ateme, Envivio gibi satıcıların yerleşik canlı kodlayıcılarını kullanarak giriş akışını açık internet üzerinden yakındaki bir Azure veri merkezine gönderin.

Dikkat edilmesi gerekenler:

[Tek bitrate RTMP](media-services-manage-live-encoder-enabled-channels.md#single_bitrate_RTMP)için aynı .

#### <a name="other-considerations"></a>Diğer konular
* Kanal veya ilişkili programları çalışıyorken giriş protokolünü değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa her bir giriş protokolü için farklı bir kanal oluşturmalısınız.
* Gelen video akışı için maksimum çözünürlük 1920x1080 ve en fazla 60 alan/saniye eğer iç içe yse, veya 30 kare/saniye aşamalıdır.

### <a name="ingest-urls-endpoints"></a>URL'leri yutma (uç noktalar)
Kanal, canlı kodlayıcıda belirttiğiniz bir giriş bitiş noktası (URL yutma) sağlar, böylece kodlayıcı akışları Kanallarınıza itebilir.

Bir Kanal oluşturduktan sonra en yüksek URL'leri alabilirsiniz. Bu URL'leri almak için, **Kanal'ın Çalışan** durumunda olması gerekmez. Verileri Kanal'a itmeye başlamaya hazır olduğunuzda, bu veri **Çalışan** durumunda olmalıdır. Kanal veri sindirilmeye başladıktan sonra, akışınızı önizleme URL'si üzerinden önizleyebilirsiniz.

Bir TLS bağlantısı üzerinden Parçalanmış MP4 (Smooth Streaming) canlı akışını sindireseçeneğiniz vardır. TLS üzerinden yutmak için, https için yutulan URL'yi güncelleştirdiğinden emin olun. Şu anda, AMS özel etki alanları ile TLS desteklemiyor.  

### <a name="allowed-ip-addresses"></a>İzin verilen IP adresleri
Bu kanalda video yayımlamalarına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresleri tek bir IP adresi (örneğin, '10.0.0.1'), IP adresi ve CIDR alt ağ maskesi kullanan bir IP aralığı (örneğin, '10.0.0.1/22') veya IP adresi ve noktalı ondalık alt ağ maskesi (örneğin, '10.0.0.1(255.25.25.05.02)) kullanan bir IP aralığı olarak belirtilebilir.

Herhangi bir IP adresi belirtilmezse ve bir kural tanımı yoksa hiçbir IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.

## <a name="channel-preview"></a>Kanal önizleme
### <a name="preview-urls"></a>Önizleme URL'leri
Kanallar, daha fazla işleme ve teslim edilmeden önce akışınızı önizlemek ve doğrulamak için kullandığınız bir önizleme bitiş noktası (önizleme URL'si) sağlar.

Kanalı oluştururken önizleme URL'sini alabilirsiniz. URL'yi almak için kanalın **Çalışan** durumunda olması gerekmez.

Kanal veri sindirmeye başladıktan sonra akışınızı önizleyebilirsiniz.

> [!NOTE]
> Şu anda önizleme akışı yalnızca belirtilen giriş türüne bakılmaksızın Parçalanmış MP4 (Düzgün Akış) biçiminde teslim edilebilir.  Akışınızı görüntülemek için Azure portalında barındırılan bir oynatıcıyı kullanabilirsiniz.
> 
> 

### <a name="allowed-ip-addresses"></a>İzin verilen IP Adresleri
Önizleme bitiş noktasına bağlanmasına izin verilen IP adreslerini tanımlayabilirsiniz. IP adresi belirtilmemişse herhangi bir IP adresine izin verilir. İzin verilen IP adresleri tek bir IP adresi (örneğin, '10.0.0.1'), IP adresi ve CIDR alt ağ maskesi kullanan bir IP aralığı (örneğin, '10.0.0.1/22') veya IP adresi ve noktalı ondalık alt ağ maskesi (örneğin, '10.0.0.1(255.25.25.05.02)) kullanan bir IP aralığı olarak belirtilebilir.

## <a name="live-encoding-settings"></a>Canlı kodlama ayarları
Bu bölümde, Kanal'ın **Kodlama Türü** **Standart**olarak ayarlandığında, Kanal içindeki canlı kodlayıcı ayarlarının nasıl ayarlanabileceği açıklanmaktadır.

> [!NOTE]
> Katkı akışınız yalnızca tek bir ses parçası içerebilir – birden çok ses parçasının sindirilmesi şu anda desteklenmez. Şirket içi canlı [kodlarla](media-services-live-streaming-with-onprem-encoders.md)canlı kodlama yaparken, birden çok ses parçası içeren Düzgün Akış protokolüne bir katkı akışı gönderebilirsiniz.
> 
> 

### <a name="ad-marker-source"></a>Reklam işaretleyici kaynağı
Reklam işaretçileri sinyallerinin kaynağını belirtebilirsiniz. Varsayılan değer **Api'** dir, bu da Kanal içindeki canlı kodlayıcının eşzamanlı Bir **Reklam İşaretleyici API'sini dinlemesi**gerektiğini gösterir.

### <a name="cea-708-closed-captions"></a>CEA 708 Kapalı Başlıklar
Canlı kodlayıcıya gelen videoya katıştak CEA 708 altyazı verilerini yoksaymasını söyleyen isteğe bağlı bir bayrak. Bayrak false (varsayılan) olarak ayarlandığında, kodlayıcı CEA 708 verilerini algılar ve çıkış video akışlarına yeniden ekler.

#### <a name="index"></a>Dizin oluşturma
Tek bir program aktarım akışı (SPTS) göndermek önerilir. Giriş akışı birden fazla program içeriyorsa, Kanal içindeki canlı kodlayıcı girişte Program Harita Tablosunu (PMT) ayrıştırır, MPEG-2 AAC ADTS veya AC-3 System-A veya AC-3 System-B veya MPEG-2 Private PES veya MPEG-1 Audio veya MPEG-2 Audio akış türü ne olursa oylayır ve bunları PMT'de belirtilen sırada düzenler. Sıfır tabanlı dizin daha sonra bu düzenlemedeki n-th girişini almak için kullanılır.

#### <a name="language"></a>Dil
SES akışının eng gibi ISO 639-2'ye uygun dil tanımlayıcısı. Yoksa, varsayılan UND (tanımsız).

### <a name="system-preset"></a><a id="preset"></a>Sistem Ön Kurulumu
Bu Kanal'daki canlı kodlayıcı tarafından kullanılacak önceden ayarlanmışı belirtir. Şu anda, izin verilen tek değer **Varsayılan 720p** (varsayılan).

**Default720p** aşağıdaki 6 katmana video kodlamak olacaktır.

#### <a name="output-video-stream"></a>Çıktı Video Akışı

| Bitrate | Genişlik | Height | MaxFPS | Profil | Çıkış Akışı Adı |
| --- | --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Yüksek |Video_1280x720_3500kbps |
| 2200 |960 |540 |30 |Yüksek |Video_960x540_2200kbps |
| 1350 |704 |396 |30 |Yüksek |Video_704x396_1350kbps |
| 850 |512 |288 |30 |Yüksek |Video_512x288_850kbps |
| 550 |384 |216 |30 |Yüksek |Video_384x216_550kbps |
| 200 |340 |192 |30 |Yüksek |Video_340x192_200kbps |

#### <a name="output-audio-stream"></a>Çıkış Ses Akışı

Ses stereo AAC-LC için 128 kbps, örnekleme hızı 48 kHz kodlanır.

## <a name="signaling-advertisements"></a>Sinyal Reklamlar
Kanalınızda Canlı Kodlama etkinleştirildiğinde, ardışık bilgisayarınızda videoyu işleyen ve onu işleyen bir bileşen vardır. Kanal'ın giden uyarlanabilir bitrate akışına kayrak ve/veya reklam eklemesi için sinyal alabilirsiniz. Arduvazlar, belirli durumlarda (örneğin ticari bir mola sırasında) giriş canlı yayınını örtbas etmek için kullanabileceğiniz görüntülerdir. Reklam sinyalleri, video oynatıcıya uygun zamanda bir reklama geçmek gibi özel bir eylemde bulunmasını söylemek için giden akışa yerleştirdiğiniz zaman senkronize sinyallerdir. Bu amaçla kullanılan SCTE-35 sinyal mekanizmasına genel bir bakış için bu [bloga](https://codesequoia.wordpress.com/2014/02/24/understanding-scte-35/) bakın. Aşağıda, canlı etkinliğinizde uygulayabileceğiniz tipik bir senaryo yer almaktadır.

1. Görüntüleyenlerinize etkinlik başlamadan önce bir ETKINLIK ÖNCESI görüntü alın.
2. Görüntüleyenlerinizin etkinlik sona erdikten sonra bir ETKINLIK sonrası görüntü almalarını sağlar.
3. Etkinlik sırasında bir sorun varsa (örneğin, stadyumda elektrik kesintisi) görüntüleyenlerinizin bir HATA-OLAY görüntüsü almalarını sağlar.
4. Reklam arası sırasında canlı etkinlik akışını gizlemek için bir AD-BREAK görüntüsü gönderin.

Aşağıda, reklam sinyali verirken ayarlayabileceğiniz özellikler verilmiştir. 

### <a name="duration"></a>Süre
Saniyeler içinde, ticari mola süresi. Bu ticari mola başlatmak için sıfır olmayan pozitif değer olmalıdır. Ticari bir mola devam ediyorsa ve cueid devam eden ticari molayla eşleşen süre sıfıra ayarlanmışsa, bu mola iptal edilir.

### <a name="cueid"></a>CueId
Ticari mola için benzersiz bir kimlik, uygun eylemi (ler) almak için downstream uygulaması tarafından kullanılacak. Pozitif bir tamsayı olmalı. Bu değeri herhangi bir rasgele pozitif tamsayıya ayarlayabilir veya Cue Id'leri izlemek için bir upstream sistemi kullanabilirsiniz. API üzerinden göndermeden önce herhangi bir iT'yi pozitif tamsayılara normalleştirdiğinden emin olun.

### <a name="show-slate"></a>Kayrak göster
İsteğe bağlı. Canlı kodlayıcının ticari bir mola sırasında [varsayılan kayrak](media-services-manage-live-encoder-enabled-channels.md#default_slate) görüntüsüne geçmesini ve gelen video akışını gizlediğini bildirir. Ses de kayrak sırasında kapatılır. Varsayılan **yanlıştır.** 

Kullanılan görüntü, kanal oluşturulducu anda varsayılan kayrak varlık Kimliği özelliği üzerinden belirtilen görüntü olacaktır. Kayrak, görüntü boyutuna uyacak şekilde uzatılır. 

## <a name="insert-slate--images"></a>Kayrak görüntüleri ekleme
Kanal içindeki canlı kodlayıcı, bir kayrak görüntüsüne geçmek için sinyal verebilir. Ayrıca devam eden bir kayrak sona erdirmek için sinyal olabilir. 

Canlı kodlayıcı, bir kayrak görüntüsüne geçmek ve gelen video sinyalini belirli durumlarda gizlemek için yapılandırılabilir – örneğin, bir reklam molası sırasında. Böyle bir kayrak yapılandırılmamışsa, giriş videosu bu reklam arası sırasında maskelenmez.

### <a name="duration"></a>Süre
Saniye cinsinden kayrak süresi. Bu, kayrak başlatmak için sıfır olmayan pozitif bir değer olmalıdır. Devam eden bir kayrak varsa ve sıfır süresi belirtilirse, devam eden kayrak sonlandırılır.

### <a name="insert-slate-on-ad-marker"></a>Reklam işaretçisi üzerine kayrak ekleme
Doğru ayarlandığında, bu ayar, reklam molası sırasında bir kayrak görüntüsü eklemek için canlı kodlayıcıyı yapılandırır. Varsayılan değer true şeklindedir. 

### <a name="default-slate-asset-id"></a><a id="default_slate"></a>Varsayılan kayrak Varlık Kimliği

İsteğe bağlı. Arduvaz görüntüsünü içeren Medya Hizmetleri Varlığının Varlık Kimliğini belirtir. Varsayılan değer geçersizdir. 


> [!NOTE] 
> Kanalı oluşturmadan önce, aşağıdaki kısıtlamalara sahip kayrak görüntüsü özel bir varlık olarak yüklenmelidir (bu kıymette başka dosya olmamalıdır). Bu resim yalnızca canlı kodlayıcı bir reklam sonu nedeniyle bir kayrak eklerken veya bir kayrak eklemek için açıkça işaret lendiğinde kullanılır. Şu anda canlı kodlayıcı böyle bir 'giriş sinyali kayıp' durumuna girdiğinde özel bir görüntü kullanmak için bir seçenek yoktur. [Burada](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/10190457-define-custom-slate-image-on-a-live-encoder-channel)bu özellik için oy verebilirsiniz.

* En fazla 1920x1080 çözünürlükte.
* Boyutu en fazla 3 Mbytes.
* Dosya adı *.jpg uzantısı olmalıdır.
* Görüntü, o Varlıktaki tek Varlık Dosyası olarak bir Varlığa yüklenmelidir ve bu Varlık Dosyası birincil dosya olarak işaretlenmelidir. Varlık depolanamaz.

Varsayılan **kayrak Varlık Kimliği** belirtilmemişse ve **reklam işaretçisine kayrak eklemek** **doğru**olarak ayarlanmışsa, giriş video akışını gizlemek için varsayılan bir Azure Media Services görüntüsü kullanılır. Ses de kayrak sırasında kapatılır. 

## <a name="channels-programs"></a>Kanal programları
Kanal, bir canlı akıştaki segmentlerin yayımlanması ve depolanmasını denetlemenizi sağlayan programlarla ilişkilidir. Kanallar, Programları yönetir. Kanal ve Program ilişkisi, bir Kanalın sürekli bir içerik akışına sahip olduğu ve bir programın o Kanal'daki zamanlanmış bir etkinliğe kapsamının bulunduğu geleneksel ortama çok benzer.

Program için kaydedilen içeriği kaç saat tutmak istediğinizi **Arşiv Penceresi** uzunluğunu ayarlayarak belirleyebilirsiniz. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir. Arşiv pencere uzunluğu, istemcilerin geçerli canlı konumdan zamanında isteyebileceği maksimum süre sayısını da belirler. Olaylar belirtilen süre miktarından uzun sürebilir, ancak pencere uzunluğunun gerisine düşen içerik sürekli olarak atılır. Bu özelliğin bu değeri, istemci bildiriminin ne kadar uzayabileceğini de belirler.

Her program, akışlı içeriği depolayan bir Varlıkla ilişkilidir. Bir varlık Azure Depolama hesabındaki bir blok blob kapsayıcısına eşlenir ve kıymetteki dosyalar bu kapsayıcıda blob olarak depolanır. Programı yayımladırmak için müşterilerinizin akışı görüntüleyebilmeleri için ilişkili varlık için bir OnDemand bulucu oluşturmanız gerekir. Bu bulucuya sahip olmak, istemcilerinize sağlayabileceğiniz bir akış URL’si oluşturmanıza olanak tanır.

Kanal, aynı anda çalışan en fazla üç programı destekler, böylece aynı gelen akışın birden çok arşivini oluşturabilirsiniz. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Örneğin, iş gereksiniminiz bir programın 6 saatini arşivlemek ancak son 10 dakikasını yayınlamak olabilir. Bunu yapmak için, eşzamanlı olarak çalışan iki program oluşturmanız gerekir. Bir program olayı 6 saat arşivlemek için ayarlanır ancak program yayımlanmaz. Diğer program 10 dakika arşivlenecek şekilde ve bu program yayımlanır.

Yeni olaylar için mevcut programları yeniden kullanmamalısınız. Bunun yerine, Programlama Canlı Akış Uygulamaları bölümünde açıklandığı gibi her olay için yeni bir program oluşturun ve başlatın.

Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda programı başlatın. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun. 

Arşivlenen içeriği silmek için, programı durdurup silin ve ardından ilişkili varlığı silin. Bir program tarafından kullanılıyorsa varlık silinemez; önce programın silinmesi gerekir. 

Programı durdurup sildikten sonra bile, varlığı silmediğiniz sürece kullanıcılar, arşivlenen içeriğinizin isteğe bağlı video olarak akışını gerçekleştirebilir.

Arşivlenen içeriği tutmak istiyor ancak bu içeriğin akış için kullanılmasını istemiyorsanız, akış bulucuyu silin.

## <a name="getting-a-thumbnail-preview-of-a-live-feed"></a>Canlı yayının küçük resim önizlemesini alma
Canlı Kodlama etkinleştirildiğinde, artık Kanal'a ulaştığında canlı yayının önizlemesini alabilirsiniz. Bu, canlı yayınınızın gerçekten Kanal'a ulaşıp ulaşmadığını kontrol etmek için değerli bir araç olabilir. 

## <a name="channel-states-and-how-states-map-to-the-billing-mode"></a><a id="states"></a>Kanal durumları ve devletlerin faturalandırma moduna nasıl eşlenebleri
Bir Kanalın geçerli durumu. Olası değerler şunlardır:

* **Durduruldu.** Bu, Kanal'ın oluşturulduktan sonraki ilk durumudur. Bu durumda, Kanal özellikleri güncelleştirilebilir, ancak akışa izin verilmez.
* **Başlangıç**. Kanal başlatılıyor. Bu durumda güncelleştirmeveya akışa izin verilmez. Bir hata oluşursa, Kanal Durdurulan duruma geri döner.
* **Koşmak**. Kanal canlı akışları işleyebilecek kapasitededir.
* **Durmak**. Kanal durduruluyor. Bu durumda güncelleştirmeveya akışa izin verilmez.
* **Silme**. Kanal silinmiş. Bu durumda güncelleştirmeveya akışa izin verilmez.

Aşağıdaki tablo, Kanal durumlarını faturalandırma moduyla nasıl eşleşdireceklerini gösterir. 

| Kanal durumu | Portal UI Göstergeleri | Fatura? |
| --- | --- | --- |
| Başlatılıyor |Başlatılıyor |Hayır (geçici durum) |
| Çalışıyor |Hazır (çalışan programlar yok)<br/>or<br/>Akış (en az bir çalışan program) |Evet |
| Durduruluyor |Durduruluyor |Hayır (geçici durum) |
| Durduruldu |Durduruldu |Hayır |

> [!NOTE]
> Şu anda, Kanal başlangıç ortalaması yaklaşık 2 dakikadır, ancak zaman zaman 20+ dakika kadar sürebilir. Kanal sıfırlamaları 5 dakika kadar sürebilir.
> 
> 

## <a name="considerations"></a><a id="Considerations"></a>Dikkat edilmesi gerekenler
* **Standart** kodlama türünden bir Kanal giriş kaynağı/katkı akışı kaybı yaşadığında, kaynak video/ses hata ve sessizlik ile değiştirerek bunu telafi eder. Kanal, giriş/katkı akışı devam edene kadar bir kayrak yamaya devam edecektir. Canlı bir kanalın 2 saatten uzun süre böyle bir durumda bırakılmaması tavsiye ediyoruz. Bu noktanın ötesinde, Kanal'ın giriş yeniden bağlantı davranışı garanti edilmez, sıfırlama komutuna yanıt olarak davranışı da garanti edilmez. Kanalı durdurmanız, silmeniz ve yeni bir kanal oluşturmanız gerekir.
* Kanal veya ilişkili programları çalışıyorken giriş protokolünü değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa her bir giriş protokolü için farklı bir kanal oluşturmalısınız.
* Canlı kodlayıcıyı her yeniden yapılandırdığınızda, kanaldaki **Sıfırlama** yöntemini arayın. Kanalı sıfırlamadan önce programı durdurmanız gerekir. Kanalı sırdamadıktan sonra programı yeniden başlatın.
* Bir kanal yalnızca Çalışan durumundayken durdurulabilir ve kanaldaki tüm programlar durdurulabilir.
* Varsayılan olarak Medya Hizmetleri hesabınıza yalnızca 5 kanal ekleyebilirsiniz. Bu tüm yeni hesaplarda yumuşak bir kotadır. Daha fazla bilgi için [Kotalar ve Sınırlamalar](media-services-quotas-and-limitations.md)bölümüne bakın.
* Kanal veya ilişkili programları çalışıyorken giriş protokolünü değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa her bir giriş protokolü için farklı bir kanal oluşturmalısınız.
* Yalnızca Kanalınız **Çalışan** durumundayken faturalandırılırsınız. Daha fazla bilgi için [bu](media-services-manage-live-encoder-enabled-channels.md#states) bölüme bakın.
* Canlı bir etkinlik için önerilen en uzun süre şu anda 8 saattir. 
* **İçeriği Çalışan** durumunda aktarmak istediğiniz akış bitiş noktasına sahip olduğundan emin olun.
* Kodlama ön ayar 30 fps "max kare hızı" kavramını kullanır. Yani giriş 60fps/59.94i ise, giriş çerçeveleri 30/29.97 fps düştü/ de-interlaced. Giriş 50fps/50i ise, giriş çerçeveleri 25 fps'ye bırakılır/interlaced. Giriş 25 fps ise, çıkış 25 fps kalır.
* Bittiğinde KANALLARINIZI DURDURMAYI UNUTMAYıN. Bunu yapmazsanız, faturalandırma devam edecektir.

## <a name="known-issues"></a>Bilinen Sorunlar
* Kanal başlatma süresi ortalama 2 dakikaya yükseltildi, ancak artan talep zamanlarında yine de 20+ dakika ya da 2dakika kadar sürebilir.
* Kayrak görüntüleri [burada](media-services-manage-live-encoder-enabled-channels.md#default_slate)açıklanan kısıtlamalara uygun olmalıdır. Varsayılan kayrak 1920x1080'den büyük olan bir Kanal oluşturmaya çalışırsanız, istek sonunda hata çıkarır.
* Bir daha.... Akışı bitirdiğinizde KANALLARINIZI DURDURMAYI unutmayın. Bunu yapmazsanız, faturalandırma devam edecektir.

## <a name="need-help"></a>Yardıma mı ihtiyacınız var?

[Yeni destek isteğine](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) yönlendirerek destek bileti açabilirsiniz

## <a name="next-step"></a>Sonraki adım

Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>İlgili konular
[Azure Medya Hizmetleri ile Canlı Akış Etkinlikleri Sunma](media-services-overview.md)

[Portal ile uyarlanabilir bitrate akışına bir singe bitrate canlı kodlama gerçekleştiren kanallar oluşturun](media-services-portal-creating-live-encoder-enabled-channel.md)

[.NET SDK ile uyarlanabilir bitrate akışına bir singe bitrate canlı kodlama gerçekleştiren kanallar oluşturun](media-services-dotnet-creating-live-encoder-enabled-channel.md)

[REST API ile kanalları yönetme](https://docs.microsoft.com/rest/api/media/operations/channel)

[Medya Hizmetleri Kavramları](media-services-concepts.md)

[Azure Medya Hizmetleri Parçalı MP4 Canlı Yutma Belirtimi](../media-services-fmp4-live-ingest-overview.md)

[live-overview]: ./media/media-services-manage-live-encoder-enabled-channels/media-services-live-streaming-new.png

