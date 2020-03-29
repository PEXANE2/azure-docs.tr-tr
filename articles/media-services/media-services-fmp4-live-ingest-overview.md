---
title: Azure Medya Hizmetleri parçalanmış MP4 canlı yutma belirtimi | Microsoft Dokümanlar
description: Bu belirtim, Azure Medya Hizmetleri için parçalanmış MP4 tabanlı canlı akış için protokol ve biçimi açıklar. Bu belge aynı zamanda son derece gereksiz ve sağlam canlı yutma mekanizmaları oluşturmak için en iyi uygulamaları tartışır.
services: media-services
documentationcenter: ''
author: cenkdin
manager: femila
editor: ''
ms.assetid: 43fac263-a5ea-44af-8dd5-cc88e423b4de
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 507afad294e8233ea4de4130795f29925870fcdf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74888062"
---
# <a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Azure Medya Hizmetleri parçalanmış MP4 canlı yutma belirtimi 

Bu belirtim, Azure Medya Hizmetleri için parçalanmış MP4 tabanlı canlı akış için protokol ve biçimi açıklar. Medya Hizmetleri, müşterilerin bulut platformu olarak Azure'u kullanarak canlı etkinlikleri ve içerikleri gerçek zamanlı olarak yayınlamak için kullanabilecekleri canlı bir akış hizmeti sağlar. Bu belge aynı zamanda son derece gereksiz ve sağlam canlı yutma mekanizmaları oluşturmak için en iyi uygulamaları tartışır.

## <a name="1-conformance-notation"></a>1. Uygunluk gösterimi
Bu belgede yer alan "MUST", "MUST NOT", "MUST", "SHOULD", "SHOULD NOT", "SHOULD NOT", "RECOMMENDED", "MAY" ve "Optional" anahtar kelimeleri RFC 2119'da açıklandığı şekilde yorumlanmalıdır.

## <a name="2-service-diagram"></a>2. Hizmet diyagramı
Aşağıdaki diyagram, Medya Hizmetleri'ndeki canlı akış hizmetinin üst düzey mimarisini gösterir:

1. Canlı kodlayıcı, canlı akışları Azure Medya Hizmetleri SDK aracılığıyla oluşturulan ve sağlanan kanallara iter.
1. Medya Hizmetleri'ndeki kanallar, programlar ve akış uç noktaları, yutma, biçimlendirme, bulut DVR, güvenlik, ölçeklenebilirlik ve artıklık dahil olmak üzere tüm canlı akış işlevlerini işler.
1. İsteğe bağlı olarak, müşteriler akış bitiş noktası ile istemci bitiş noktaları arasında bir Azure İçerik Teslim Ağı katmanı dağıtmayı seçebilir.
1. İstemci uç noktaları, HTTP Uyarlanabilir Akış protokollerini kullanarak akış bitiş noktasından akışı sağlar. Örnek olarak Microsoft Smooth Streaming, HTTP üzerinden Dinamik Uyarlamalı Akış (DASH veya MPEG-DASH) ve Apple HTTP Canlı Akış (HLS) verilebilir.

![akış yutmak][image1]

## <a name="3-bitstream-format--iso-14496-12-fragmented-mp4"></a>3. Bitstream formatı – ISO 14496-12 parçalanmış MP4
Bu belgede tartışılan canlı akış için tel formatı [ISO-14496-12] dayanmaktadır. Parçalı MP4 biçimi nin ve uzantıların hem video-on-demand dosyaları hem de canlı akış alımı için ayrıntılı bir açıklama için [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx)bölümüne bakın.

### <a name="live-ingest-format-definitions"></a>Biçim tanımlarını canlı olarak yutma
Aşağıdaki liste, Azure Medya Hizmetleri'nde yaşamak için geçerli olan özel biçim tanımlarını açıklamaktadır:

1. **Ftyp**, **Live Server Manifest Box**, ve **moov** kutuları her istek (HTTP POST) ile gönderilmelidir. Bu kutular akışın başında gönderilmeli ve her zaman kodlayıcı akışı almak devam etmek için yeniden bağlanmak gerekir. Daha fazla bilgi için bkz.
1. Bölüm 3.3.2 [1] canlı yutmak için **StreamManifestBox** adlı isteğe bağlı bir kutu tanımlar. Azure yük bakiyesinin yönlendirme mantığı nedeniyle, bu kutuyu kullanmak amortismana alınır. Kutu Medya Hizmetleri içine yutma mevcut olmamalıdır. Bu kutu varsa, Medya Hizmetleri sessizce yok sayar.
1. **TrackFragmentExtendedHeaderBox** kutusu 3.2.3.2 içinde tanımlanan [1] her parça için mevcut olmalıdır.
1. **TrackFragmentExtendedHeaderBox** kutusunun Sürüm 2 birden çok veri merkezinde aynı URL'lere sahip medya segmentleri oluşturmak için kullanılmalıdır. Parça dizin alanı, Apple HLS ve dizin tabanlı MPEG-DASH gibi dizin tabanlı akış biçimlerinin çapraz veri merkezi başarısızlığı için GEREKLIDIR. Çapraz veri merkezi hatasını etkinleştirmek için, parça dizini birden çok kodlayıcı arasında eşitlenmeli ve encoder yeniden başlatmaları veya hataları arasında bile birbirini izleyen her ortam parçası için 1 artırılmalıdır.
1. Bölüm 3.3.6 [1] **moviefragmentRandomAccessBox** **(mfra)** adlı bir kutu tanımlar bu KANALa akış sonu (EOS) göstermek için canlı yutma sonunda gönderilebilir. Medya Hizmetlerinin en yüksek mantığı nedeniyle EOS kullanımı amortismana sokulmaz ve canlı yutma için **mfra** kutusu gönderilmemelidir. Eğer gönderilirse, Medya Hizmetleri sessizce yok sayar. Yutma noktasının durumunu sıfırlamak için [Kanal Sıfırlama'yı](https://docs.microsoft.com/rest/api/media/operations/channel#reset_channels)kullanmanızı öneririz. Ayrıca, sunuyu ve akışı sona erdirmek için [Program Durdur'u](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) kullanmanızı öneririz.
1. MP4 parça süresi sabit olmalıdır, istemci manifestoları boyutunu azaltmak için. Sabit bir MP4 parçası süresi, yinelenen etiketleri kullanarak istemci indirme buluşlarını da geliştirir. May süresi, integer olmayan çerçeve hızlarını dengelemek için dalgalanır.
1. MP4 parça süresi yaklaşık 2 ile 6 saniye arasında olmalıdır.
1. MP4 parça zaman damgaları ve dizinleri **(TrackFragmentExtendedHeaderBox** `fragment_ absolute_ time` ve `fragment_index`) artan sırada gelmelidir. Medya Hizmetleri yinelenen parçalara karşı dirençli olsa da, parçaları ortam zaman çizelgesine göre yeniden sıralama yeteneği sınırlıdır.

## <a name="4-protocol-format--http"></a>4. Protokol formatı – HTTP
Media Services için ISO parçalanmış MP4 tabanlı canlı yutma, parçalanmış MP4 formatında paketlenmiş kodlanmış medya verilerini hizmete iletmek için uzun süredir çalışan standart bir HTTP POST isteği kullanır. Her HTTP POST tam bir parçalanmış MP4 bitstream gönderir ("akış"), başlık kutuları ile başından itibaren **(ftyp**, **Live Server Manifest Box**, ve **moov** kutuları), ve parçaları **(moof** ve **mdat** kutuları) bir dizi ile devam. HTTP POST isteği için URL sözdizimi için bkz. POST URL'ye bir örnek: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

### <a name="requirements"></a>Gereksinimler
İşte ayrıntılı gereksinimler şunlardır:

1. Kodlayıcı, aynı yutma URL'sini kullanarak boş bir "gövde" (sıfır içerik uzunluğu) içeren bir HTTP POST isteği göndererek yayını başlamalıdır. Bu, kodlayıcının canlı yutma bitiş noktasının geçerli olup olmadığını ve kimlik doğrulaması veya gerekli başka koşullar olup olmadığını hızla algılamaya yardımcı olabilir. HTTP protokolü ne göre, sunucu POST gövdesi de dahil olmak üzere tüm istek alınana kadar bir HTTP yanıtını geri gönderemez. Canlı bir olayın uzun süren doğası göz önüne alındığında, bu adım olmadan, kodlayıcı tüm verileri göndermeyi bitirene kadar herhangi bir hata algılayamayabilir.
1. Kodlayıcı MUST nedeniyle (1) herhangi bir hata veya kimlik doğrulama zorlukları ele. (1) 200 yanıtla başarılı olursa, devam edin.
1. Kodlayıcı parçalanmış MP4 akışı ile yeni bir HTTP POST isteği başlatmak GEREKIR. Taşıma kapasitesi üstbilgi kutularıyla başlar ve ardından parçalar dan başlar. **Ftyp**, **Live Server Manifest Box**, ve **moov** kutuları (bu sırada) her istek ile gönderilmesi gerektiğini unutmayın, kodlayıcı önceki istek akışı sona ermeden önce sonlandırıldı çünkü yeniden bağlanmak gerekir olsa bile. 
1. Kodlayıcı yükleme için yüklü aktarım kodlaması kullanmalıdır, çünkü canlı etkinliğin tüm içerik uzunluğunu tahmin etmek imkansızdır.
1. Olay sona erdiğinde, son parçayı gönderdikten sonra, kodlayıcı, parçalanmış aktarım kodlama ileti sibunu düzgün bir şekilde sona erdirmeli (çoğu HTTP istemci yığınları otomatik olarak işler). Kodlayıcı, hizmetin son yanıt kodunu döndürmesini beklemeli ve sonra bağlantıyı sonlandırmalıdır. 
1. Kodlayıcı, [1] `Events()` 9.2'de açıklandığı gibi medya hizmetlerine canlı olarak girmek için nonu kullanmamalıdır.
1. HTTP POST isteği akışı sona ermeden önce bir TCP hatası ile sona erer veya saatler dolursa, kodlayıcı yeni bir bağlantı kullanarak yeni bir POST isteği yayımlamak ve önceki gereksinimleri izleyin GEREKIR. Ayrıca, kodlayıcı, akıştaki her parça için önceki iki MP4 parçasını yeniden göndermeli ve ortam zaman çizelgesinde bir süreksizlik sunmadan devam etmelidir. Her parça için son iki MP4 parçasının yeniden gönderilmesi veri kaybı olmamasını sağlar. Başka bir deyişle, bir akış hem ses hem de video parçası içeriyorsa ve geçerli POST isteği başarısız olursa, kodlayıcı daha önce başarıyla gönderilen ses parçası ve video için son iki parçayı yeniden bağlamalı ve yeniden göndermelidir veri kaybı olmadığından emin olmak için daha önce başarıyla gönderilen parça. Kodlayıcı, yeniden bağlandığında yeniden gönderdiği ortam parçalarının "ileri" arabelleği olmalıdır.

## <a name="5-timescale"></a>5. Zaman ölçeği
[[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx) **SmoothStreamingMedia** (Bölüm 2.2.2.1), **StreamElement** (Bölüm 2.2.2.3), **StreamFragmentElement** (Bölüm 2.2.2.6) ve **LiveSMIL** (Bölüm 2.2.7.3.1) için zaman ölçeği kullanımını açıklar. Zaman ölçeği değeri yoksa, kullanılan varsayılan değer 10.000.000 (10 MHz) olur. Düzgün Akış biçimi belirtimi diğer zaman ölçeği değerlerinin kullanımını engellemese de, kodlayıcı uygulamalarının çoğu Bu varsayılan değeri (10 MHz) düzgün akış veri oluşturmak için kullanır. [Azure Media Dinamik Ambalaj](media-services-dynamic-packaging-overview.md) özelliği nedeniyle, video akışları için 90 KHz zaman ölçeği ve ses akışları için 44,1 KHz veya 48,1 KHz kullanmanızı öneririz. Farklı akışlar için farklı zaman ölçeği değerleri kullanılıyorsa, akış düzeyindeki zaman ölçeği gönderilmeli. Daha fazla bilgi için [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx)bakın.     

## <a name="6-definition-of-stream"></a>6. "Akarsu" tanımı
Akış, canlı sunular oluşturmak, akış başarısızlığını işlemek ve artıklık senaryolarını işlemek için canlı yutma da temel çalışma birimidir. Akış, tek bir parça veya birden çok parça içerebilecek benzersiz, parçalanmış bir MP4 bit akışı olarak tanımlanır. Tam canlı sunu, canlı kodlayıcıların yapılandırmasına bağlı olarak bir veya daha fazla akış içerebilir. Aşağıdaki örnekler, tam bir canlı sunu oluşturmak için akışları kullanmanın çeşitli seçeneklerini göstermektedir.

**Örnek:** 

Bir müşteri aşağıdaki ses/video bithızlarını içeren bir canlı akış sunusu oluşturmak ister:

Video – 3000 kbps, 1500 kbps, 750 kbps

Ses – 128 kbps

### <a name="option-1-all-tracks-in-one-stream"></a>Seçenek 1: Tek bir akıştaki tüm parçalar
Bu seçenekte, tek bir kodlayıcı tüm ses/video parçalarını oluşturur ve bunları parçalanmış bir MP4 bitstream'de paketler. Parçalanmış MP4 bitstream sonra tek bir HTTP POST bağlantısı üzerinden gönderilir. Bu örnekte, bu canlı sunu için yalnızca bir akış vardır.

![Akışlar-bir parça][image2]

### <a name="option-2-each-track-in-a-separate-stream"></a>Seçenek 2: Ayrı bir akıştaki her parça
Bu seçenekte, kodlayıcı her parçamp4 bitstream içine bir parça koyar ve sonra ayrı HTTP bağlantıları üzerinden tüm akışları gönderir. Bu, bir kodlayıcı veya birden fazla kodlayıcı ile yapılabilir. Canlı yutma bu canlı sunuyu dört akıştan oluşan olarak görür.

![Akışlar ayrı parçalar][image3]

### <a name="option-3-bundle-audio-track-with-the-lowest-bitrate-video-track-into-one-stream"></a>Seçenek 3: En düşük bit hızına sahip ses parçasını tek bir akışta paketleyin
Bu seçenekte, müşteri ses parçasını bir parça MP4 bitstream'de en düşük bit hızındaki video parçasıyla bir araya getirmeyi ve diğer iki video parçasını ayrı akışlar olarak bırakmayı seçer. 

![Akışlar-ses ve video parçaları][image4]

### <a name="summary"></a>Özet
Bu, bu örnek için tüm olası yutma seçeneklerinin ayrıntılı bir listesi değildir. Nitekim, akışları içine parça herhangi bir gruplama canlı yutma tarafından desteklenir. Müşteriler ve kodlayıcı satıcıları mühendislik karmaşıklığı, kodlayıcı kapasitesi ve fazlalık ve başarısızlık konularına göre kendi uygulamalarını seçebilirler. Ancak, çoğu durumda, tüm canlı sunu için yalnızca bir ses parçası vardır. Bu nedenle, ses parçasını içeren yutma akışının sağlıklı olmasını sağlamak önemlidir. Bu husus genellikle ses izini kendi akışına yerleştirmesiyle (Seçenek 2'de olduğu gibi) veya en düşük bit hızındaki video parçasıyla (Seçenek 3'te olduğu gibi) biraraya getirmekle sonuçlanır. Ayrıca, daha iyi artıklık ve hata toleransı için, aynı ses parçasını iki farklı akışta (Yedekses parçalarıyla opsiyon 2) göndermek veya ses parçasını en az iki adet en düşük bit hızındaki video parçasıyla donatır (En az iki adet ses içeren seçenek 3 video akışları) medya hizmetlerine canlı olarak girmeleri için şiddetle tavsiye edilir.

## <a name="7-service-failover"></a>7. Hizmet inanın
Canlı yayının doğası göz önüne alındığında, iyi bir başarısız destek, hizmetin kullanılabilirliğini sağlamak için çok önemlidir. Ortam Hizmetleri, ağ hataları, sunucu hataları ve depolama sorunları da dahil olmak üzere çeşitli hata türlerini işlemek üzere tasarlanmıştır. Canlı kodlayıcı tarafından uygun arıza mantığı ile birlikte kullanıldığında, müşteriler buluttan son derece güvenilir bir canlı akış hizmeti elde edebilirsiniz.

Bu bölümde, hizmet başarısız senaryoları tartışmak. Bu durumda, hata hizmet içinde bir yerde olur ve bir ağ hatası olarak kendini gösterir. Aşağıda, hizmet ini işleme için kodlayıcı uygulaması için bazı öneriler vereme aşağıdakiler ve

1. TCP bağlantısını kurmak için 10 saniyelik bir zaman ayarı kullanın. Bağlantıyı kurma girişimi 10 saniyeden uzun sürerse, işlemi iptal edin ve yeniden deneyin. 
1. HTTP istek iletisi öbeklerini göndermek için kısa bir zaman ayırın. Hedef MP4 parça süresi N saniyeise, N ile 2 N saniye arasında bir gönderme zaman dışarısı kullanın; örneğin, MP4 parça süresi 6 saniyeise, 6 ila 12 saniyelik bir zaman dilimi kullanın. Bir zaman ası oluşursa, bağlantıyı sıfırla, yeni bir bağlantı açın ve yeni bağlantıda akış akışın devamına devam edin. 
1. Başarıyla ve tamamen hizmete gönderilen her parça için son iki parçaya sahip bir yuvarlanma arabelleği koruyun.  Bir akış için HTTP POST isteği sonlandırılırsa veya akış sona ermeden önce zaman ları sona erdirilirse, yeni bir bağlantı açın ve başka bir HTTP POST isteği başlatın, akış üstbilgilerini yeniden gönderin, her parça için son iki parçayı yeniden gönderin ve akışa giriş yapmadan devam edin medya zaman çizelgesinde süreksizlik. Bu, veri kaybı olasılığını azaltır.
1. Kodlayıcının, bir TCP hatası oluştuktan sonra bağlantı kurmak veya akışı devam ettirmek için yeniden deneme sayısını sınırlamamasını öneririz.
1. Bir TCP hatasından sonra:
  
    a. Geçerli bağlantı KAPATILMASI GEREKEN VE yeni bir HTTP POST isteği için yeni bir bağlantı oluşturulmalıdır.

    b. Yeni HTTP POST URL'si ilk POSTA URL'si ile aynı olmalıdır.
  
    c. Yeni HTTP POST ilk POST akış üstbilgilerini aynı akış üstbilgilerini **(ftyp**, **Live Server Manifest Box**, ve **moov** kutuları) içermelidir.
  
    d. Her parça için gönderilen son iki parça dargın olmalı ve akış ortam zaman çizelgesinde bir süreksizlik sunulmadan devam etmelidir. MP4 parçalı zaman damgaları, HTTP POST istekleri arasında bile sürekli olarak artmalıdır.
1. Veri MP4 parça süresi ile orantılı bir oranda gönderiliyor değilse kodlayıcı HTTP POST isteğini sonlandırmalıdır.  Veri göndermeyen bir HTTP POST isteği, bir hizmet güncelleştirmesi durumunda Medya Hizmetlerinin kodlayıcıyla hızla bağlantısını kesmesini engelleyebilir. Bu nedenle, seyrek (reklam sinyali) parçaları için HTTP POST seyrek (reklam sinyali) parçaları kısa ömürlü olmalı, seyrek parça gönderilir gönderilmez sonlandırıcı.

## <a name="8-encoder-failover"></a>8. Encoder failover
Encoder failover uçtan uca canlı akış teslimiçin ele alınması gereken başarısız senaryo ikinci türüdür. Bu senaryoda, hata koşulu kodlayıcı tarafında oluşur. 

![encoder failover][image5]

Encoder failover gerçekleştiğinde, canlı yutma bitiş noktasından aşağıdaki beklentiler geçerlidir:

1. Diyagramda gösterildiği gibi (Kesik çizgili 3000k video için akış) olarak akışı sürdürmek için yeni bir kodlayıcı örneği oluşturulmalıdır.
1. Yeni kodlayıcı, http post istekleri için başarısız örnekle aynı URL'yi kullanmalıdır.
1. Yeni kodlayıcının POST isteği, başarısız örnekle aynı parçalanmış MP4 üstbilgi kutularını içermelidir.
1. Yeni kodlayıcı, hizalanmış parça sınırlarıyla senkronize ses/video örnekleri oluşturmak için aynı canlı sunu için çalışan diğer tüm kodlayıcılarla düzgün bir şekilde senkronize edilmelidir.
1. Yeni akış, önceki akışla anlamsal olarak eşdeğer olmalı ve üstbilgi ve parça düzeylerinde değiştirilebilir.
1. Yeni kodlayıcı veri kaybını en aza indirmek için denemelisiniz. Ortam `fragment_absolute_time` `fragment_index` parçalarının ve kodlayıcının en son durduğu noktadan artması gerekir. Ve `fragment_absolute_time` `fragment_index` sürekli bir şekilde artırmak GEREKIR, ancak gerekirse bir süreksizlik tanıtmak için izin verilir. Medya Hizmetleri, zaten aldığı ve işlediği parçaları yok sayar, bu nedenle medya zaman çizelgesinde süreksizlikler getirmektense parçaları yeniden gönderme tarafında hatalı olmak daha iyidir. 

## <a name="9-encoder-redundancy"></a>9. Kodlayıcı artıklığı
Daha yüksek kullanılabilirlik ve deneyim kalitesi gerektiren bazı kritik canlı etkinlikler için, veri kaybı olmadan kesintisiz bir şekilde başarısız olmak için etkin-etkin yedekkodlayıcılar kullanmanızı tavsiye ettik.

![kodlayıcı artıklığı][image6]

Bu diyagramda gösterildiği gibi, iki kodlayıcı grubu her akışın iki kopyasını aynı anda canlı hizmete iter. Medya Hizmetleri, akış kimliği ve parça zaman damgasına göre yinelenen parçaları filtreleyebildiği için bu kurulum desteklenir. Elde edilen canlı akış ve arşiv, iki kaynaktan mümkün olan en iyi toplama olan tüm akışların tek bir kopyasıdır. Örneğin, varsayımsal bir ekstrem durumda, her akış için belirli bir zamanda çalışan bir kodlayıcı (aynı olmak zorunda değildir) olduğu sürece, hizmetten elde edilen canlı akış veri kaybı olmadan süreklidir. 

Bu senaryonun gereksinimleri, ikinci kodlayıcı kümesinin birincil kodlayıcılarla aynı anda çalıştırdığı durumlar dışında, "Kodlayıcı başarısızlığı" durumundaki gereksinimlerle hemen hemen aynıdır.

## <a name="10-service-redundancy"></a>10. Hizmet artıklığı
Son derece gereksiz küresel dağıtım için, bazen bölgesel felaketleri işlemek için bölgeler arası yedeklemeye sahip olmalısınız. "Kodlayıcı artıklığı" topolojisini genişleten müşteriler, ikinci kodlayıcı kümesiyle bağlantılı farklı bir bölgede gereksiz bir hizmet dağıtımı yapmayı seçebilir. Müşteriler, istemci trafiğini sorunsuz bir şekilde yönlendirmek için iki hizmet dağıtımının önünde bir Global Traffic Manager dağıtmak için bir İçerik Dağıtım Ağı sağlayıcısıyla da çalışabilir. Kodlayıcılar için gereksinimler "Kodlayıcı artıklığı" durumuyla aynıdır. Tek istisna, ikinci kodlayıcı kümesinin farklı bir canlı yutma bitiş noktasına işaret edilmesi gerektiğidir. Aşağıdaki diyagram bu kurulumu gösterir:

![hizmet artıklığı][image7]

## <a name="11-special-types-of-ingestion-formats"></a>11. Yutma biçimlerinin özel türleri
Bu bölümde, belirli senaryoları işlemek üzere tasarlanmış özel canlı yutma biçimleri türleri açıklanmaktadır.

### <a name="sparse-track"></a>Seyrek parça
Zengin bir istemci deneyimine sahip canlı akış sunusu sunarken, genellikle zaman senkronize edilmiş olayları veya sinyalleri ana ortam verileriyle bant içinde iletmek gerekir. Bunun bir örneği dinamik canlı reklam eklemedir. Bu tür olay sinyalleri, seyrek yapısı nedeniyle normal ses/video akışından farklıdır. Başka bir deyişle, sinyal verileri genellikle sürekli olarak gerçekleşmez ve aralığı tahmin etmek zor olabilir. Seyrek parça kavramı, bant içi sinyal verilerini sindirecek ve yayınlayabilmek için tasarlanmıştır.

Seyrek parça yutmak için önerilen bir uygulama aşağıdaki adımlardır:

1. Ses/video parçaları olmadan yalnızca seyrek parçalar içeren ayrı bir parçalanmış MP4 bitstream oluşturun.
1. [1]'deki Bölüm 6'da tanımlandığı şekilde **Live Server Manifest Box'ta,** üst parçanın adını belirtmek için üst *TrackName* parametresini kullanın. Daha fazla bilgi için bkz: [1].
1. Live **Server Manifest Box,** **manifestOutput** **gerçek**olarak ayarlanmalıdır .
1. Sinyal leme olayının seyrek doğası göz önüne alındığında, aşağıdakileri tavsiye ettik:
   
    a. Canlı etkinliğin başlangıcında, kodlayıcı ilk üstbilgi kutularını hizmete gönderir ve bu da hizmetin seyrek parçayı istemci bildirimine kaydetmesini sağlar.
   
    b. Kodlayıcı, veri gönderilmediğinde HTTP POST isteğini sonlandırmalıdır. Veri göndermeyen uzun süreli bir HTTP POST, bir hizmet güncelleştirmesi veya sunucu yeniden başlatması durumunda Medya Hizmetlerinin kodlayıcıyla hızla bağlantısını kesmesini engelleyebilir. Bu gibi durumlarda, ortam sunucusu soketüzerindeki bir alma işleminde geçici olarak engellenir.
   
    c. Sinyal verisinin kullanılamadığı süre boyunca, kodlayıcı HTTP POST isteğini kapatmalı. POST isteği etkin ken, kodlayıcı veri göndermeli.

    d. Seyrek parçalar gönderirken, kodlayıcı varsa açık bir içerik uzunluğu üstbilgi ayarlayabilir.

    e. Yeni bir bağlantı ile seyrek parçaları gönderirken, kodlayıcı üstbilgi kutularından göndermeye başlamalı ve ardından yeni parçalar. Bu, failover'ın arada gerçekleştiği ve seyrek bağlantının daha önce seyrek parçayı görmemiş yeni bir sunucuya kurulduğu durumlar içindir.

    f. Eşit veya daha büyük bir zaman damgası değerine sahip ilgili ana parça parçası istemci tarafından kullanılabilir hale geldiğinde seyrek parça parçası istemci tarafından kullanılabilir hale gelir. Örneğin, seyrek parçanın t=1000 zaman damgası varsa, istemci "video" (üst parça adının "video" olduğunu varsayarak) parçasını zaman damgası 1000 veya ötesini gördükten sonra, seyrek parça yı t=1000'i indirebilir. Gerçek sinyalin, sunu zaman çizelgesinde belirlenen amaç için farklı bir konum için kullanılabileceğini unutmayın. Bu örnekte, t=1000'in seyrek parçasının xml yükü vardır ve bu da bir reklamı birkaç saniye sonra bir konuma eklemek içindir.

    g. Seyrek parça parçalarının yükü, senaryoya bağlı olarak farklı biçimlerde (XML, metin veya ikili gibi) olabilir.

### <a name="redundant-audio-track"></a>Gereksiz ses parçası
Tipik bir HTTP uyarlanabilir akış senaryosunda (örneğin, Düzgün Akış veya DASH), genellikle tüm sunuda yalnızca bir ses parçası vardır. İstemci için hata koşullarında seçebileceği birden çok kalite düzeyine sahip olan video parçalarının aksine, ses parçasını içeren akış bozuksa, ses parçası tek bir hata noktası olabilir. 

Bu sorunu çözmek için, Medya Hizmetleri gereksiz ses parçalarının canlı olarak yutulmasını destekler. Fikir, aynı ses parçası farklı akışlarda birden çok kez gönderilebilir olmasıdır. Hizmet ses parçasını istemci bildirimine yalnızca bir kez kaydetse de, birincil ses parçasında sorun varsa, ses parçalarını almak için yedek olarak gereksiz ses parçalarını kullanabilir. Gereksiz ses parçalarını yutmak için kodlayıcının şunları yapması gerekir:

1. Birden çok parça MP4 bitstreams aynı ses parçasını oluşturun. Yedek ses parçaları, aynı parça zaman damgaları ile anlamsal olarak eşdeğer olmalıdır ve üstbilgi ve parça düzeylerinde değiştirilebilir olmalıdır.
1. Live Server Manifest'teki (Bölüm 6 in [1]) "ses" girişinin tüm gereksiz ses parçaları için aynı olduğundan emin olun.

Gereksiz ses parçaları için aşağıdaki uygulama önerilir:

1. Her benzersiz ses parçasını tek başına bir akışta gönderin. Ayrıca, ikinci akış ilkinden yalnızca HTTP POST URL'sindeki tanımlayıcıya göre farklılık gösterir: {protocol}://{server address}/{publishing point path}/Streams({identifier})
1. En düşük iki video bitisini göndermek için ayrı akışlar kullanın. Bu akışların her biri, her benzersiz ses parçasının bir kopyasını da içermelidir. Örneğin, birden çok dil desteklendiğinde, bu akışlar HER dil için ses parçaları içermelidir.
1. (1) ve (2)'de belirtilen gereksiz akışları kodlamak ve göndermek için ayrı sunucu (kodlayıcı) örneklerini kullanın. 

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png
