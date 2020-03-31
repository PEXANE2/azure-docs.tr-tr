---
title: Dinamik PaketLeyici kullanarak bildirimlerinizi filtreleyin
titleSuffix: Azure Media Services
description: Bildirimlerinizi filtrelemek ve seçarak akışla aktarmak için Dinamik Paketleyici'yi kullanarak filtreleri nasıl oluşturup oluşturup aktaracaklarınızı öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 07/11/2019
ms.author: juliako
ms.openlocfilehash: cd955f97a2f26543f799d95b7dc0b1de235333c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74186209"
---
# <a name="filter-your-manifests-using-dynamic-packager"></a>Dinamik PaketLeyici kullanarak bildirimlerinizi filtreleyin

Aygıtlara uyarlanabilir bit hızı akış içeriği sunarken, bazen belirli aygıt özelliklerini veya kullanılabilir ağ bant genişliğini hedeflemek için bir bildirimin birden çok sürümü yayımlamanız gerekir. [Dinamik PaketLeyici,](dynamic-packaging-overview.md) belirli kodcları, çözünürlükleri, bit hızları ve ses parça kombinasyonlarını anında filtreleyebilen filtreler belirtmenizi sağlar. Bu filtreleme, birden çok kopya oluşturma gereksinimini ortadan kaldırır. Hedef aygıtlarınıza (iOS, Android, SmartTV veya tarayıcılar) ve ağ yeteneklerine (yüksek bant genişliği, mobil veya düşük bant genişliği senaryoları) yapılandırılan belirli bir filtre kümesiiçeren yeni bir URL yayınlamanız yeterlidir. Bu durumda, istemciler sorgu dizesi üzerinden içeriğinizin akışını (kullanılabilir [Varlık filtreleri veya Hesap filtreleri](filters-concept.md)belirterek) işleyebilir ve bir akışın belirli bölümlerini akış için filtreler kullanabilir.

Bazı teslimat senaryoları, müşterinin belirli parçalara erişemediğinden emin olun. Örneğin, belirli bir abone katmanında HD parça içeren bir bildirim yayımlamak istemeyebilirsiniz. Veya, ek parçalardan yararlanamayacak belirli bir cihaza teslim maliyetini azaltmak için belirli uyarlanabilir bitrate (ABR) parçalarını kaldırmak isteyebilirsiniz. Bu durumda, önceden oluşturulmuş filtrelerin listesini oluşturma da [Akış bulucunuzla](streaming-locators-concept.md) ilişkilendirebilirsiniz. İstemciler daha sonra **Akış Lı Yer Bulucu**tarafından tanımlandığı için içeriğin akış şeklini değiştiremez.

Filtreleme, Akış Lı [Keştör 'de filtreleme](filters-concept.md#associating-filters-with-streaming-locator) ( istemcinizin URL'de belirttiği ek cihaza özel filtreler) ile birleştirebilirsiniz. Bu kombinasyon, meta veriler veya olay akışları, ses dilleri veya açıklayıcı ses parçaları gibi ek parçaları kısıtlamak için yararlıdır.

Akışınızda farklı filtreler belirtme yeteneği, hedef aygıtlarınız için birden çok kullanım durumu senaryosuhedeflemek için güçlü bir **Dinamik Bildirim** işleme çözümü sağlar. Bu **konu, Dinamik Bildirimler** ile ilgili kavramları açıklar ve bu özelliği kullanabileceğiniz senaryoörnekleri verir.

> [!NOTE]
> Dinamik Bildirimler, kıymeti ve bu varlığın varsayılan bildirimini değiştirmez.

## <a name="overview-of-manifests"></a>Bildirimlere genel bakış

Azure Medya Hizmetleri HLS, MPEG DASH ve Sorunsuz Akış protokollerini destekler. [Dinamik Paketleme'nin](dynamic-packaging-overview.md)bir parçası olarak, akışlı istemci bildirimleri (HLS Master Playlist, DASH Media Presentation Description [MPD] ve Düzgün Akış) URL'deki biçim seçiciye bağlı olarak dinamik olarak oluşturulur. Daha fazla bilgi için, [Ortak isteğe bağlı iş akışındaki](dynamic-packaging-overview.md#delivery-protocols)teslim protokollerine bakın.

### <a name="get-and-examine-manifest-files"></a>Bildirim dosyalarını alın ve inceleyin

Akışınızın (canlı veya isteğe bağlı video [VOD]) parçalarının dinamik olarak oluşturulmuş bir bildirime eklenmesi gerektiğine bağlı olarak filtre izleme özelliği koşullarının bir listesini belirtirsiniz. Parçaların özelliklerini almak ve incelemek için önce Düzgün Akış bildirimini yüklemeniz gerekir.

[.NET öğreticisine sahip Yükleme, kodlama ve akış dosyaları,](stream-files-tutorial-with-api.md#get-streaming-urls) .NET ile akış URL'lerini nasıl oluşturabileceğinizi gösterir. Uygulamayı çalıştırıyorsanız, URL'lerden biri Düzgün Akış bildirimine `https://amsaccount-usw22.streaming.media.azure.net/00000000-0000-0000-0000-0000000000000/ignite.ism/manifest`işaret eder: .<br/> URL'yi kopyalayıp tarayıcının adres çubuğuna yapıştırın. Dosya indirilecektir. Herhangi bir metin düzenleyicisinde açabilirsiniz.

REST örneği için, [REST ile dosya yükle, kodlama ve akış dosyalarına](stream-files-tutorial-with-rest.md#list-paths-and-build-streaming-urls)bakın.

### <a name="monitor-the-bitrate-of-a-video-stream"></a>Video akışının bit hızını izleme

Bir video akışının bitrate'sini izlemek için [Azure Media Player demo sayfasını](https://aka.ms/azuremediaplayer) kullanabilirsiniz. Demo sayfası **Tanılama** sekmesinde tanılama bilgilerini görüntüler:

![Azure Media Player tanılama][amp_diagnostics]

### <a name="examples-urls-with-filters-in-query-string"></a>Örnekler: Sorgu dizesi filtreli URL'ler

Filtreleri ABR akış protokollerine uygulayabilirsiniz: HLS, MPEG-DASH ve Düzgün Akış. Aşağıdaki tablo, filtreli URL'lerin bazı örneklerini gösterir:

|Protokol|Örnek|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Kesintisiz Akış|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="rendition-filtering"></a>Yorumlama filtreleme

Varlığınızı birden çok kodlama profiline (H.264 Baseline, H.264 High, AACL, AACH, Dolby Digital Plus) ve birden çok kalite bit oranlarına kodlamayı seçebilirsiniz. Ancak, tüm istemci aygıtları tüm varlıkınızın profillerini ve bithızlarını desteklemez. Örneğin, eski Android cihazlar yalnızca H.264 Taban Çizgisi+AACL'yi destekler. Avantajlardan yararlanabilen bir cihaza daha yüksek bit hızları göndermek bant genişliğini ve aygıt hesaplamasını boşa harcar. Böyle bir aygıt, yalnızca görüntülenmek üzere küçültmek için verilen tüm bilgilerin şifresini çözmelidir.

Dinamik Bildirim ile cihaz profilleri (mobil, konsol veya HD/SD gibi) oluşturabilir ve her profilin bir parçası olmak istediğiniz parçaları ve nitelikleri ekleyebilirsiniz. Buna yorumlama filtreleme denir. Aşağıdaki diyagram bunun bir örneğini gösterir.

![Dinamik Bildirim ile yorumlama filtreleme örneği][renditions2]

Aşağıdaki örnekte, bir büyüleyici varlığı yedi ISO MP4s video yorumlamasına (180p'den 1080p'ye) kodlamak için bir kodlayıcı kullanılmıştır. Kodlanmış varlık dinamik [olarak](dynamic-packaging-overview.md) aşağıdaki akış protokollerinden herhangi birinde paketlenebilir: HLS, MPEG DASH ve Smooth.

Aşağıdaki diyagramın üst kısmında, filtresiz varlık için HLS bildirimi gösterilmektedir. (Yedi yorumlamanın hepsini içerir.)  Sol alttabakada, diyagramda "ott" adlı bir filtrenin uygulandığı bir HLS bildirimi gösterilmektedir. "Ott" filtresi 1 Mbps'nin altındaki tüm bit hızların kaldırılmasını belirtir, böylece yanıtta alttaki iki kalite düzeyi silinmiştir. Sağ alttabakada, diyagram "mobil" adlı bir filtrenin uygulandığı HLS bildirimini gösterir. "Mobil" filtresi, çözünürlüğün 720p'den büyük olduğu yorumlamaların kaldırılmasını belirtir, bu nedenle iki 1080p yorumlama sıyrılır.

![Dinamik Bildirim ile yorumlama filtreleme][renditions1]

## <a name="removing-language-tracks"></a>Dil parçalarını kaldırma
Varlıklarınız İngilizce, İspanyolca, Fransızca ve benzeri birçok ses dili içerebilir. Genellikle, Player SDK varsayılan ses parça seçimi ve kullanıcı seçimi başına kullanılabilir ses parçaları yönetir.

Bu tür Player SDK'ları geliştirmek zordur, çünkü cihaza özel oyuncu çerçeveleri arasında farklı uygulamalar gerektirir. Ayrıca, bazı platformlarda Player API'leri sınırlıdır ve kullanıcıların varsayılan ses parçasını seçemediği veya değiştirebildiği ses seçimi özelliğini içermez. Varlık filtreleri ile, yalnızca istenen ses dillerini içeren filtreler oluşturarak davranışı denetleyebilirsiniz.

![Dinamik Bildirim ile dil parçalarının filtrelemi][language_filter]

## <a name="trimming-the-start-of-an-asset"></a>Bir varlığın başlangıcını kırpma

Çoğu canlı akış olayında, işleçler gerçek olaydan önce bazı testler çalıştırın. Örneğin, olay başlamadan önce buna benzer bir sayfa içerebilirler: "Program anlık olarak başlayacaktır."

Program arşivleme ise, test ve kayrak verileri de arşivlenir ve sunuya dahil edilir. Ancak, bu bilgiler istemcilere gösterilmemelidir. Dinamik Bildirim ile bir başlangıç zamanı filtresi oluşturabilir ve istenmeyen verileri bildirimden kaldırabilirsiniz.

![Dinamik Bildirim ile bir varlığın başlangıç kırpma][trim_filter]

## <a name="creating-subclips-views-from-a-live-archive"></a>Canlı arşivden alt klipler (görünümler) oluşturma

Birçok canlı etkinlik uzun süredir devam ediyor ve canlı arşiv birden çok olay içerebilir. Canlı etkinlik sona erdikten sonra, yayıncılar canlı arşivi mantıksal program başlatma ve durdurma dizilerine ayırmak isteyebilir.

Bu sanal programları canlı arşivi işlemeden ve ayrı varlıklar oluşturmadan ayrı ayrı yayımlayabilirsiniz (CDN'lerde bulunan önbelleğe alınmış parçalardan yararlanamaz). Bu tür sanal programlara örnek olarak bir futbol veya basketbol maçının dörtte biri, beyzbolda atışlar veya herhangi bir spor programının bireysel etkinlikleri verilebilir.

Dinamik Bildirim ile başlangıç/bitiş saatlerini kullanarak filtreler oluşturabilir ve canlı arşivinizin üst kısmında sanal görünümler oluşturabilirsiniz.

![Dinamik Bildirimli Alt klip filtresi][subclip_filter]

İşte filtreuygulanmış varlık:

![Dinamik Bildirim ile filtrelenmiş varlık][skiing]

## <a name="adjusting-the-presentation-window-dvr"></a>Sunu penceresini ayarlama (DVR)

Azure Medya Hizmetleri şu anda, sürenin 1 dakika - 25 saat arasında yapılandırılabildiği dairesel arşiv sunar. Bildirim filtreleme, ortam silmeden arşivin üst kısmında yuvarlanan bir DVR penceresi oluşturmak için kullanılabilir. Yayıncıların canlı kenarla hareket etmek ve aynı zamanda daha büyük bir arşivleme penceresi tutmak için sınırlı bir DVR penceresi sağlamak istedikleri birçok senaryo vardır. Bir yayıncı klipleri vurgulamak için DVR penceresi dışında olan verileri kullanmak isteyebilir veya farklı aygıtlar için farklı DVR pencereleri sağlamak isteyebilir. Örneğin, mobil cihazların çoğu büyük DVR pencerelerini işlemez (mobil cihazlar için 2 dakikalık bir DVR penceresi ne de masaüstü istemcileri için bir saat olabilir).

![Dinamik Manifesto ile DVR penceresi][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>LiveBackoff'u ayarlama (canlı konum)

Manifest filtreleme, canlı bir programın canlı kenarından birkaç saniye kaldırmak için kullanılabilir. Filtreleme, yayıncıların sunuyu önizleme yayın noktasında izlemelerine ve görüntüleyenler akışı almadan önce reklam ekleme noktaları oluşturmalarına (30 saniye geri tepme) olanak tanır. Yayıncılar daha sonra bu reklamları, reklam fırsatından önce bilgileri almaları ve işlemeleri için zamanında müşteri çerçevelerine itebilirler.

Reklam desteğine ek olarak, canlı geri dönüş ayarı, istemciler sürüklenip canlı kenarına geldiklerinde sunucudan parçalar alabilmeleri için görüntüleyenlerin konumunu ayarlamak için kullanılabilir. Bu şekilde, istemciler bir HTTP 404 veya 412 hatası almaz.

![Dynamic Manifest ile canlı geri tepme için filtre][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Birden çok kuralı tek bir filtrede birleştirme

Birden çok filtreleme kuralını tek bir filtrede birleştirebilirsiniz. Örneğin, canlı arşivdeki arduvazları kaldırmak ve kullanılabilir bit hızlarını filtrelemek için bir "aralık kuralı" tanımlayabilirsiniz. Birden çok filtreleme kuralı uygularken, sonuç tüm kuralların kesişimidir.

![Dinamik Bildirim ile birden çok filtreleme kuralı][multiple-rules]

## <a name="combining-multiple-filters-filter-composition"></a>Birden çok filtreyi birleştirme (filtre kompozisyonu)

Birden çok filtreyi tek bir URL'de de birleştirebilirsiniz. Aşağıdaki senaryo, filtreleri neden birleştirmek isteyebileceğini gösterir:

1. Video kalitelerinizi Android veya iPad gibi mobil cihazlar için filtrelemeniz gerekir (video özelliklerini sınırlamak için). İstenmeyen nitelikleri kaldırmak için aygıt profillerine uygun bir hesap filtresi oluşturursunuz. Aynı Medya Hizmetleri hesabı altındaki tüm varlıklarınız için hesap filtrelerini başka bir ilişkilendirme olmadan kullanabilirsiniz.
1. Ayrıca, bir varlığın başlangıç ve bitiş saatini de kırpmak istersiniz. Kırpma yapmak için bir varlık filtresi oluşturur ve başlangıç/bitiş saatini ayarlarsınız.
1. Bu filtrelerin her ikisini de birleştirmek istiyorsunuz. Kombinasyon olmadan, filtre kullanımını zorlaştıracak olan kesme filtresine kaliteli filtreleme eklemeniz gerekir.

Filtreleri birleştirmek için filtre adlarını yarı sütunlu biçimde manifesto/çalma listesi URL'sine ayarlayın. Nitelikleri filtreleyen *MyMobileDevice* adında bir filtreniz olduğunu ve belirli bir başlangıç saatini ayarlamak için *MyStartTime* adında bir filtreniz olduğunu varsayalım. En fazla üç filtreyi birleştirebilirsiniz.

Daha fazla bilgi için [bu blog gönderisi](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/)bakın.

## <a name="considerations-and-limitations"></a>Önemli noktalar ve sınırlamalar

- **ForceEndTimestamp,** **presentationWindowDuration**ve **liveBackoffDuration** değerleri VOD filtresi için ayarlanamamalıdır. Bunlar yalnızca canlı filtre senaryoları için kullanılır.
- Dinamik bir manifesto GOP sınırları (anahtar kareler) çalışır, bu nedenle kırpma GOP doğruluğu vardır.
- Hesap ve varlık filtreleri için aynı filtre adını kullanabilirsiniz. Varlık filtreleri daha yüksek önceliğe sahiptir ve hesap filtrelerini geçersiz kılar.
- Bir filtreyi güncellerseniz, akış bitiş noktasının kuralları yenilemesi 2 dakika kadar sürebilir. İçeriğe hizmet etmek için filtreler kullandıysanız (ve içeriği eksek ve CDN önbelleklerinde önbelleğe aldıysanız), bu filtreleri güncelleştirmek oynatıcı hatalarına neden olabilir. Filtreyi güncelledikten sonra önbelleği temizlemenizi öneririz. Bu seçenek mümkün değilse, farklı bir filtre kullanmayı düşünün.
- Müşterilerin manifestoyu el ile indirmesi ve tam başlangıç saati damgasını ve zaman ölçeğini ayrıştması gerekir.

    - Bir varlıktaki parçaların özelliklerini belirlemek [için, bildirim dosyasını alın ve inceleyin.](#get-and-examine-manifest-files)
    - Varlık filtresi zaman damgası özelliklerini ayarlamak için formül: <br/>startTimestamp &lt;= manifestoda&gt; +  &lt;başlama saati beklenen&gt; filtre nin başlama saati saniye * zaman ölçeği

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler, filtrelerin programlı olarak nasıl oluşturulacak larını gösterir:  

- [REST API'leri ile filtreler oluşturma](filters-dynamic-manifest-rest-howto.md)
- [.NET ile filtre oluşturma](filters-dynamic-manifest-dotnet-howto.md)
- [CLI ile filtre oluşturma](filters-dynamic-manifest-cli-howto.md)

[renditions1]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/filters-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/filters-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/filters-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/filters-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/filters-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/filters-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/filters-dynamic-manifest-overview/media-services-skiing.png
[amp_diagnostics]: ./media/filters-dynamic-manifest-overview/amp_diagnostics.png
