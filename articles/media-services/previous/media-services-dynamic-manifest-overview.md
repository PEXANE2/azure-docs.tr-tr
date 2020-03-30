---
title: Filtreler ve dinamik manifestolar | Microsoft Dokümanlar
description: Bu konu, istemcinizin bir akışın belirli bölümlerini akışı için bunları kullanabilmesi için filtrelerin nasıl oluşturulabileceğini açıklar. Medya Hizmetleri, bu seçici akışı arşivlemek için dinamik bildirimler oluşturur.
services: media-services
documentationcenter: ''
author: cenkdin
manager: femila
editor: ''
ms.assetid: ff102765-8cee-4c08-a6da-b603db9e2054
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 1234263fa800a17d0a5c235df54ca2751e3094bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69015861"
---
# <a name="filters-and-dynamic-manifests"></a>Filtreler ve dinamik bildirimler

> [!div class="op_single_selector" title1="Kullandığınız Medya Hizmetleri sürümünü seçin:"]
> * [Sürüm 2](media-services-dynamic-manifest-overview.md)
> * [Sürüm 3](../latest/filters-dynamic-manifest-overview.md)

2.17 sürümüyle başlayarak, Medya Hizmetleri varlıklarınız için filtreler tanımlamanıza olanak tanır. Bu filtreler, müşterilerinizin aşağıdaki gibi şeyler yapmayı seçmelerine olanak tanıyan sunucu tarafı kurallarıdır: videonun yalnızca bir bölümünü oynatın (videonun tamamını oynatmak yerine) veya yalnızca müşterinizin cihazının işleyebilir ses ve video yorumlamalarının bir alt kümesini belirtin ( varlıkla ilişkili tüm yorumlamalar yerine). Varlıklarınızın bu filtrelemi, müşterinizin belirtilen filtre(ler'e dayalı bir video akışı için istek üzerine oluşturulan **Dinamik Bildirimler)** aracılığıyla gerçekleştirilir.

Bu konu, filtre kullanmanın müşterileriniz için yararlı olacağı yaygın senaryoları ve filtrelerin programlı olarak nasıl oluşturulacağını gösteren konulara bağlantılar tartışır.

## <a name="overview"></a>Genel Bakış
İçeriğinizi müşterilere sunarken (canlı etkinlikler akışı veya isteğe bağlı video akışı) amacınız, farklı ağ koşulları altında çeşitli cihazlara yüksek kaliteli bir video sunmaktır. Bu amaca ulaşmak için aşağıdakileri yapın:

* akışınızı çok bit hızında[(uyarlanabilir bit hızı)](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)video akışına kodlamak (bu, kalite ve ağ koşullarını dikkate alacaktır) ve 
* Akışınızı dinamik olarak farklı protokollere yeniden paketlemek için Medya Hizmetleri [Dinamik Ambalaj'ı](media-services-dynamic-packaging-overview.md) kullanın (bu, farklı cihazlarda akış yapmayı halleder). Medya Hizmetleri aşağıdaki uyarlanabilir bitrate akış teknolojilerinin teslimini destekler: HTTP Live Streaming (HLS), Smooth Streaming ve MPEG DASH. 

### <a name="manifest-files"></a>Manifest dosyaları
Bir varlığı uyarlanabilir bithızı akışı için kodladiğinizde, bir **bildirim** (çalma listesi) dosyası oluşturulur (dosya metin tabanlı veya XML tabanlıdır). **Bildirim** dosyası gibi meta veri akışı içerir: parça türü (ses, video veya metin), parça adı, başlangıç ve bitiş saati, bitrate (nitelikleri), izleme dilleri, sunu penceresi (sabit süre kayan pencere), video codec (FourCC). Ayrıca, oyuncuya, kullanılabilir sonraki oynatılabilir video parçaları ve konumları hakkında bilgi vererek bir sonraki parçayı alması için talimat verir. Parçalar (veya segmentler), video içeriğinin gerçek "parçaları"dır.

Aşağıda bir bildirim dosyası örneği verilmiştir: 

    <?xml version="1.0" encoding="UTF-8"?>    
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">

    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />

    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>


    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />

    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>


    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />

    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>

    </SmoothStreamingMedia>

### <a name="dynamic-manifests"></a>Dinamik bildirimler
Müşterinizin varsayılan varlığın bildirim dosyasında açıklanandan daha fazla esnekliğe ihtiyaç duyduğu [senaryolar](media-services-dynamic-manifest-overview.md#scenarios) vardır. Örnek:

* Cihaza özgü: yalnızca içeriği oynatmak için kullanılan aygıt tarafından desteklenen belirtilen yorumlamaları ve/veya belirtilen dil parçalarını teslim edin ("yorumlama filtreleme"). 
* Canlı bir olayın alt klibini ("alt klip filtresi") göstermek için bildirimi azaltın.
* Bir videonun başlangıcını kırpın ("videoyu kırpma").
* Oyuncudaki DVR penceresinin sınırlı bir uzunluğunu sağlamak için Sunu Penceresini (DVR) ayarlayın ("sunu penceresini ayarlama").

Bu esnekliği elde etmek için, Medya Hizmetleri önceden tanımlanmış [filtrelere](media-services-dynamic-manifest-overview.md#filters)dayalı **Dinamik Bildirimler** sunar.  Filtreleri tanımladıktan sonra, istemcileriniz bunları videonuzun belirli bir yorumunu veya alt kliplerini aktarmak için kullanabilir. Akış URL'sinde filtre(ler) belirtirler. Filtreler, [Dinamik Ambalaj](media-services-dynamic-packaging-overview.md)tarafından desteklenen uyarlanabilir bitrate akış protokollerine uygulanabilir: HLS, MPEG-DASH ve Düzgün Akış. Örnek:

Filtreli MPEG DASH URL

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Filtreyle Düzgün Akış URL'si

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


İçeriğinizi nasıl sunup akış URL'leri oluşturup oluştursağlayacağınız hakkında daha fazla bilgi için [bkz.](media-services-deliver-content-overview.md)

> [!NOTE]
> Dinamik Bildirimler'in kıymeti ve bu varlığın varsayılan bildirimini değiştirmediğini unutmayın. İstemciniz filtreli veya filtresiz bir akış isteğinde bulunmayı seçebilir. 
> 
> 

### <a name="filters"></a><a id="filters"></a>Filtreler
İki tür varlık filtresi vardır: 

* Genel filtreler (Azure Medya Hizmetleri hesabındaki herhangi bir varlığa uygulanabilir, hesabın ömrü dolabilir) ve 
* Yerel filtreler (yalnızca filtrenin oluşturulduktan sonra ilişkilendirildiği bir varlığa uygulanabilir, varlığın ömrü vardır). 

Genel ve yerel filtre türleri tam olarak aynı özelliklere sahiptir. İkisi arasındaki temel fark, hangi senaryoların hangi filer türünün daha uygun olduğudur. Genel filtreler genellikle, belirli bir varlığı kırpmak için yerel filtrelerin kullanılabildiği aygıt profilleri (yorumlama filtreleme) için uygundur.

## <a name="common-scenarios"></a><a id="scenarios"></a>Genel senaryolar
Daha önce de belirtildiği gibi, içeriğinizi müşterilere sunarken (canlı etkinlik akışı veya isteğe bağlı video) amacınız, farklı ağ koşulları altında çeşitli cihazlara yüksek kaliteli bir video sunmaktır. Buna ek olarak, varlıklarınızı filtreleme ve Dinamik **Bildirim'i**kullanma nızı içeren başka gereksinimleriniz de olabilir. Aşağıdaki bölümler, farklı filtreleme senaryolarına kısa bir genel bakış sağlar.

* Yalnızca belirli aygıtların işleyebilir ses ve video yorumlamaları bir alt kümesi belirtin (varlık ile ilişkili tüm yorumlamalar yerine). 
* Videonun yalnızca bir bölümünü oynatma (videonun tamamını oynatmak yerine).
* DVR sunu pencereyi ayarlayın.

## <a name="rendition-filtering"></a>Yorumlama filtreleme
Varlığınızı birden çok kodlama profiline (H.264 Baseline, H.264 High, AACL, AACH, Dolby Digital Plus) ve birden çok kalite bitoranlarına kodlamayı seçebilirsiniz. Ancak, tüm istemci aygıtları tüm varlıkınızın profillerini ve bithızlarını desteklemez. Örneğin, eski Android cihazlar yalnızca H.264 Baseline+AACL'yi destekler. Avantajları elde edemeyen, bant genişliğini ve cihaz hesaplamasını boşa harcayan bir cihaza daha yüksek bit hızları göndermek. Bu tür bir aygıt, yalnızca görüntülenmek üzere küçültmek için verilen tüm bilgilerin şifresini çözmelidir.

Dynamic Manifest ile mobil, konsol, HD/SD gibi aygıt profilleri oluşturabilir ve her profilin bir parçası olmak istediğiniz parçaları ve nitelikleri ekleyebilirsiniz.

![Yorumlama filtreleme örneği][renditions2]

Aşağıdaki örnekte, bir büyüleyici varlığı yedi ISO MP4s video yorumlamasına (180p'den 1080p'ye) kodlamak için bir kodlayıcı kullanılmıştır. Kodlanmış varlık dinamik olarak aşağıdaki akış protokollerinden herhangi birinde paketlenebilir: HLS, Smooth ve MPEG DASH.  Diyagramın üst kısmında, filtresiz varlığın HLS bildirimi gösterilir (yedi yorumlamanın tümünün yanıiçerir).  Sol altta, "ott" adlı bir filtrenin uygulandığı HLS bildirimi gösterilir. "Ott" filtresi, yanıtta alt iki kalite düzeyinin sökülmesine neden olan 1Mbps'nin altındaki tüm bit hızları kaldırmak için belirtir. Sağ alttabakada, "mobil" adlı bir filtrenin uygulandığı HLS bildirimi gösterilir. "Mobil" filtresi, çözünürlüğün 720p'den büyük olduğu ve iki 1080p yorumlamanın silinmesine neden olan yorumlamaları ortadan kaldırmak için belirtir.

![Yorumlama filtreleme][renditions1]

## <a name="removing-language-tracks"></a>Dil parçalarını kaldırma
Varlıklarınız İngilizce, İspanyolca, Fransızca, vb. gibi birden çok ses dili içerebilir. Genellikle, Player SDK yöneticileri varsayılan ses parça seçimi ve kullanıcı seçimi başına kullanılabilir ses parçaları. Bu tür Oyuncu SDK'ları geliştirmek zordur, cihaza özel oyuncu çerçeveleri arasında farklı uygulamalar gerektirir. Ayrıca, bazı platformlarda Player API'leri sınırlıdır ve kullanıcıların varsayılan ses parçasını seçemediği veya değiştiremediği ses seçimi özelliğini içermez. Varlık filtreleri ile, yalnızca istenen ses dillerini içeren filtreler oluşturarak davranışı denetleyebilirsiniz.

![Dil izleme filtresi][language_filter]

## <a name="trimming-start-of-an-asset"></a>Bir varlığın başlangıcını kırpma
Çoğu canlı akış olayında, işleçler gerçek olaydan önce bazı testler çalıştırın. Örneğin, olay başlamadan önce buna benzer bir sayfa içerebilirler: "Program anlık olarak başlayacaktır". Program arşivleme ise, test ve kayrak verileri de arşivlenir ve sunuya dahil edilir. Ancak, bu bilgiler istemcilere gösterilmemelidir. Dinamik Bildirim ile bir başlangıç zamanı filtresi oluşturabilir ve istenmeyen verileri bildirimden kaldırabilirsiniz.

![Kırpma başlangıcı][trim_filter]

## <a name="creating-subclips-views-from-a-live-archive"></a>Canlı arşivden alt klipler (görünümler) oluşturma
Birçok canlı etkinlik uzun süredir devam ediyor ve canlı arşiv birden çok olay içerebilir. Canlı etkinlik sona erdikten sonra, yayıncılar canlı arşivi mantıksal program başlatma ve durdurma dizilerine ayırmak isteyebilir. Ardından, bu sanal programları canlı arşivi işlemeden ve ayrı varlıklar oluşturmadan ayrı ayrı yayımlayın (CDN'lerde bulunan önbelleğe alınmış parçalardan yararlanamaz). Bu tür sanal programlara örnek olarak bir futbol veya basketbol maçının dörtte biri, beyzbolda atışlar veya herhangi bir spor programının bireysel etkinlikleri verilebilir.

Dinamik Bildirim ile başlangıç/bitiş saatlerini kullanarak filtreler oluşturabilir ve canlı arşivinizin üst kısmında sanal görünümler oluşturabilirsiniz. 

![Alt klip filtresi][subclip_filter]

Filtreuygulanmış Varlık:

![Kayak][skiing]

## <a name="adjusting-presentation-window-dvr"></a>Sunu Pencereyi Ayarlama (DVR)
Azure Medya Hizmetleri şu anda, sürenin 5 dakika - 25 saat arasında yapılandırılabildiği dairesel arşiv sunar. Bildirim filtreleme, ortam silmeden arşivin üst kısmında yuvarlanan bir DVR penceresi oluşturmak için kullanılabilir. Yayıncıların canlı kenarla hareket etmek ve aynı zamanda daha büyük bir arşivleme penceresi tutmak için sınırlı bir DVR penceresi sağlamak istedikleri birçok senaryo vardır. Bir yayıncı klipleri vurgulamak için DVR penceresi dışında olan verileri kullanmak isteyebilir veya farklı aygıtlar için farklı DVR pencereleri sağlamak isteyebilir. Örneğin, mobil cihazların çoğu büyük DVR pencerelerini işlemez (mobil cihazlar için 2 dakikalık bir DVR penceresi ne de masaüstü istemcileri için bir saat olabilir).

![DVR penceresi][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>LiveBackoff'u ayarlama (canlı konum)
Manifest filtreleme, canlı bir programın canlı kenarından birkaç saniye kaldırmak için kullanılabilir. Filtreleme, yayıncıların sunuyu önizleme yayın noktasında izlemelerine ve görüntüleyenler akışı (30 saniye destekli) almadan önce reklam ekleme noktaları oluşturmalarına olanak tanır. Yayıncılar daha sonra bu reklamları, reklam fırsatından önce bilgileri almaları ve işlemeleri için zamanında müşteri çerçevelerine itebilirler.

Reklam desteğine ek olarak, LiveBackoff ayarı, istemciler drift ve canlı kenar vurmak onlar hala bir HTTP 404 veya 412 hata almak yerine sunucudan parçaları alabilirsiniz böylece izleyici konumunu ayarlamak için kullanılabilir.

![livebackoff_filter][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Birden çok kuralı tek bir filtrede birleştirme
Birden çok filtreleme kuralını tek bir filtrede birleştirebilirsiniz. Örnek olarak, canlı arşivdeki arduvazları kaldırmak ve kullanılabilir bit hızlarını filtrelemek için bir "aralık kuralı" tanımlayabilirsiniz. Birden çok filtreleme kuralı uygularken, sonuç tüm kuralların kesişimidir.

![çoklu kurallar][multiple-rules]

## <a name="create-filters-programmatically"></a>Filtreleri programlı oluşturma
Aşağıdaki makalede, filtrelerle ilgili Medya Hizmetleri varlıkları açıklanmaktadır. Makalede ayrıca filtrelerin nasıl programlanabilir bir şekilde oluşturulacak ları da gösterilmektedir.  

[REST API'leri ile filtreler oluşturun.](media-services-rest-dynamic-manifest.md)

## <a name="combining-multiple-filters-filter-composition"></a>Birden çok filtreyi birleştirme (filtre kompozisyonu)
Birden çok filtreyi tek bir URL'de de birleştirebilirsiniz. 

Aşağıdaki senaryo, filtreleri neden birleştirmek isteyebileceğini gösterir:

1. Video kalitelerinizi Android veya iPAD gibi mobil cihazlar için filtrelemeniz gerekir (video özelliklerini sınırlamak için). İstenmeyen nitelikleri kaldırmak için, aygıt profilleri için uygun genel bir filtre oluşturursunuz. Bu makalede daha önce de belirtildiği gibi, genel filtreler başka bir ilişkilendirme olmadan aynı medya hizmetleri hesabı altında tüm varlıklar için kullanılabilir. 
2. Ayrıca, bir varlığın başlangıç ve bitiş saatini de kırpmak istersiniz. Bunu başarmak için yerel bir filtre oluşturur ve başlangıç/bitiş saatini ayarlarsınız. 
3. Bu filtrelerin her ikisini de birleştirmek istiyorsunuz (kombinasyon olmadan, filtre kullanımını zorlaştıran kırpma filtresine kaliteli filtreleme eklemeniz gerekir).

Filtreleri birleştirmek için, filtre adlarını yarı sütunlu olarak sınırlandırılmış olan manifesto/çalma listesi URL'sine ayarlamanız gerekir. Nitelikleri filtreleyen *MyMobileDevice* adında bir filtreniz olduğunu ve belirli bir başlangıç saatini ayarlamak için *MyStartTime* adında bir filtreniz olduğunu varsayalım. Bunları şu şekilde birleştirebilirsiniz:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

En fazla üç filtreyi birleştirebilirsiniz. 

Daha fazla bilgi için [bu](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) bloga bakın.

## <a name="know-issues-and-limitations"></a>Sorunları ve sınırlamaları bilin
* Dinamik manifesto GOP sınırları (Anahtar Çerçeveler) dolayısıyla kırpma GOP doğruluğu vardır çalışır. 
* Yerel ve genel filtreler için aynı filtre adını kullanabilirsiniz. Yerel filtreler daha yüksek önceliğe sahiptir ve genel filtreleri geçersiz kılar.
* Bir filtreyi güncellerseniz, akış bitiş noktasının kuralları yenilemesi 2 dakika kadar sürebilir. İçerik bazı filtreler kullanılarak sunulduysa (ve yakınlıkve CDN önbelleklerinde önbelleğe alınmışsa), bu filtreleri güncelleştirmek oynatıcı hatalarına neden olabilir. Filtreyi güncelledikten sonra önbelleği temizlemen önerilir. Bu seçenek mümkün değilse, farklı bir filtre kullanmayı düşünün.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Müşterilere İçerik Sunma Genel Bakış](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png
