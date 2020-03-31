---
title: Gelişmiş Media Encoder Premium Workflow öğreticileri
description: Bu belge, Media Encoder Premium İş Akışı ile gelişmiş görevlerin nasıl gerçekleştirildiğini ve İş Akışı Tasarımcısı ile karmaşık iş akışlarının nasıl oluşturulacaklarını gösteren gözden geçirme ler içerir.
services: media-services
documentationcenter: ''
author: xstof
manager: femila
editor: ''
ms.assetid: 1ba52865-b4a8-4ca0-ac96-920d55b9d15b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: christoc
ms.reviewer: xpouyat; juliako
ms.openlocfilehash: 1ab70d56bd3def58d0e814035070cf027a88cd3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251017"
---
# <a name="advanced-media-encoder-premium-workflow-tutorials"></a>Gelişmiş Media Encoder Premium Workflow öğreticileri
## <a name="overview"></a>Genel Bakış
Bu belge, **İş Akışı Tasarımcısı**ile iş akışlarının nasıl özelleştirilebildiğini gösteren iş yol larını içerir. Gerçek iş akışı dosyalarını [burada](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples)bulabilirsiniz.  

## <a name="toc"></a>Toc
Aşağıdaki konular ele alınmıştır:

* [MXF'yi tek bir bit hızına dönüştürme MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
  * [Yeni bir iş akışı başlatma](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new)
  * [Ortam Dosya Girişi'ni kullanma](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
  * [Ortam akışlarını denetleme](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
  * [Için video kodlayıcısı ekleme. MP4 dosya oluşturma](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
  * [Ses akışını kodlama](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
  * [Ses ve Video akışlarını MP4 kapsayıcısına çok katmanlama](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
  * [MP4 dosyasını yazma](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
  * [Çıktı dosyasından Bir Medya Hizmetleri Varlığı Oluşturma](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
  * [Tamamlanan iş akışını yerel olarak test edin](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
* [MXF'yi çoklu bit hızında MP4'lere kodlama - dinamik ambalaj etkin](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
  * [Bir veya daha fazla ek MP4 çıkışı ekleme](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
  * [Dosya çıktı adlarını yapılandırma](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
  * [Ayrı bir Ses Parçası Ekleme](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
  * ["ISM" SMIL Dosyasını Ekleme](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
* [MXF'yi çoklu bit hızında MP4'e kodlama - geliştirilmiş plan](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
  * Geliştirmek için iş akışına genel bakış
  * [Dosya Adlandırma Kuralları](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
  * [Bileşen özelliklerini iş akışı köküne yayımlama](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
  * [Çıktı dosya adları yayımlanmış özellik değerlerine güvenerek oluşturdu](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
* [Çoklu bithızında MP4 çıkışına küçük resim ekleme](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
  * Küçük resimler eklemek için iş akışına genel bakış
  * [JPG Kodlama ekleme](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
  * [Renk Alanı dönüştürme ile başa çıkma](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
  * [Küçük resimleri yazma](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
  * [İş akışındaki hataları algılama](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
  * [Bitmiş İş Akışı](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
* [Çoklu bithızında MP4 çıkışının zamana dayalı kırpma](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
  * [Kırpma eklemeye başlamak için iş akışına genel bakış](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
  * [Akış Düzelticisini Kullanma](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
  * [Bitmiş İş Akışı](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
* [Komut Dosyası Bileşeni yle TanıştırMa](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
  * [İş akışı içinde komut dosyası: merhaba dünya](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
* [Multibitrate MP4 çıkışının çerçeve tabanlı kırpma](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
  * [Kırpma eklemeye başlamak için plana genel bakış](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
  * [Klips Listesini Kullanma XML](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
  * [Komut Dosyası Bileşeninden klip listesini değiştirme](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
  * [Kırpma EklemeEtkin kolaylık özelliği](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

## <a name="encoding-mxf-into-a-single-bitrate-mp4"></a><a id="MXF_to_MP4"></a>MXF'yi tek bir bit hızına dönüştürme MP4
Bu bölümde, tek bir bithızı nasıl oluşturulacak gösteriş. AAC-HE kodlu MP4 dosyası bir . MXF giriş dosyası.

### <a name="starting-a-new-workflow"></a><a id="MXF_to_MP4_start_new"></a>Yeni bir iş akışı başlatma
İş Akışı Tasarımcısını açın ve Dosya > Yeni İş Alanı > Transcode Blueprint'i seçin

Yeni iş akışı üç öğeyi gösterir:

* Birincil Kaynak Dosya
* Klip Listesi XML
* Çıktı Dosyası/Varlık  

![Yeni Kodlama İş Akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Yeni Kodlama İş Akışı*

### <a name="using-the-media-file-input"></a><a id="MXF_to_MP4_with_file_input"></a>Ortam Dosya Girişi'ni kullanma
Giriş ortamı dosyasını kabul etmek için, bir Ortam Dosyası Giriş bileşeni ekleyerek başlar. İş akışına bir bileşen eklemek için, Depo arama kutusunda onu arayın ve istenen girişi tasarımcı bölmesine sürükleyin. Media File Input için eylemi yineleyin ve Birincil Kaynak Dosya bileşenini Media Dosya Girişi'nden Dosya Adı giriş pinine bağlayın.

![Bağlı Ortam Dosya Girişi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Bağlı Ortam Dosya Girişi*

Başlangıçta, özel bir iş akışı tasarlarken kullanılacak uygun bir örnek dosya tanımlayın. Bunu yapmak için, tasarımcı bölmesi arka planı tıklatın ve sağ daki özellik bölmesindeki Birincil Kaynak Dosya özelliğini arayın. Klasör simgesini tıklatın ve iş akışını test etmek için istenen dosyayı seçin. Ortam Dosyası Girişi bileşeni dosyayı inceler ve denetlediği örnek dosyanın ayrıntılarını yansıtacak şekilde çıktı pinlerini doldurur.

![Doldurulan Ortam Dosya Girişi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Doldurulan Ortam Dosya Girişi*

Giriş doldurulur artık, bir sonraki adım çıktı kodlama ayarlarını ayarlamaktır. Birincil Kaynak Dosyanın nasıl yapılandırıldığına benzer şekilde, şimdi çıktı klasörü değişken özelliğini hemen altında yapılandırın.

![Yapılandırılmış Giriş ve Çıktı özellikleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Yapılandırılmış Giriş ve Çıktı özellikleri*

### <a name="inspecting-media-streams"></a><a id="MXF_to_MP4_streams"></a>Ortam akışlarını denetleme
Genellikle akış iş akışı boyunca akar gibi nasıl göründüğünü bilmek istenir. İş akışındaki herhangi bir noktada bir akışı incelemek için bileşenlerden herhangi birinde bir çıktı veya giriş pinini tıklatmanız yeterlidir. Bu durumda, Medya Dosya Girişi'nden Sıkıştırılmamış Video çıkış pinine tıklamayı deneyin. Giden videoyu incelemenize olanak tanıyan bir iletişim açılır.

![Sıkıştırılmamış Video çıkış pininin incelenmesi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*Sıkıştırılmamış Video çıkış pininin incelenmesi*

Bu durumda, videonun yaklaşık 2 dakikalık bir video için 4:2:2 örneklemesinde saniyede 24 karelik 1920x1080 girişi içerdiğini gösterir.

### <a name="adding-a-video-encoder-for-mp4-file-generation"></a><a id="MXF_to_MP4_file_generation"></a>Için video kodlayıcısı ekleme. MP4 dosya oluşturma
Artık, Medya Dosyası Girişi'nde kullanılmak üzere sıkıştırılmamış bir Video ve birden çok Sıkıştırılmamış Ses çıkış pimi kullanılabilir. Gelen videoyu kodlamak için, bir kodlama bileşeninin iş akışına eklenmesi gerekir - bu durumda . MP4 dosyaları.

Video akışını H.264'e kodlamak için AVC Video Encoder bileşenini tasarımcı yüzeyine ekleyin. Bu bileşen giriş olarak sıkıştırılmamış bir video akışı alır ve çıkış piminde Bir AVC sıkıştırılmış video akışı sağlar.

![Bağlantısız AVC Kodlayıcı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Bağlantısız AVC Kodlayıcı*

Özellikleri, kodlamanın tam olarak nasıl gerçekleşacağını belirler. Daha önemli ayarlardan bazılarına bir göz atalım:

* Çıkış genişliği ve Çıkış yüksekliği: kodlanmış videonun çözünürlüğünü belirler. Bu durumda, 640x360 iyi bir ayardır.
* Kare Hızı: sadece kaynak kare hızını benimseyecek geçmek için ayarlandığında, bu olsa geçersiz kılmak mümkündür. Bu tür karehızı dönüştürme hareket telafi değildir.
* Profil ve Seviye: AVC profilini ve düzeyini belirler. Farklı düzeyler ve profiller hakkında daha fazla bilgi almak için, AVC Video Encoder bileşenindeki soru işareti simgesini tıklatın ve yardım sayfası düzeylerin her biri hakkında daha fazla ayrıntı gösterir. Bu örnekte, 3.2 düzeyinde (varsayılan) Ana Profil'i kullanın.
* Hız Kontrol Modu ve Bitrate (kbps): Bu senaryoda, 1200 kbps sabit bitrate (CBR) çıkış ını tercih
* Video Formatı: H.264 akışına yazılan VUI (Video Kullanılabilirlik Bilgileri) hakkında bilgi sağlar (ekranı geliştirmek için bir kod çözücü tarafından kullanılabilecek ancak doğru şekilde çözmek için gerekli olmayan yan bilgiler):
* NTSC (ABD veya Japonya için tipik, 30 fps kullanarak)
* PAL (Avrupa için tipik, 25 fps kullanarak)
* GOP Boyut Modu: Kapalı GOPs ile 2 saniyelik Bir Anahtar Aralığı ile bizim amaçlar için Sabit GOP Boyutu ayarlayın. 2 saniye ayar, Azure Medya Hizmetleri'nin sağladığı dinamik ambalajla uyumluluk sağlar.

AVC kodlayıcısını beslemek için, Media File Input bileşeninden Sıkıştırılmamış Video çıkış pinini AVC kodlayıcısından sıkıştırılmamış Video giriş pinine bağlayın.

![Bağlı AVC Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Bağlı AVC Ana kodlayıcı*

### <a name="encoding-the-audio-stream"></a><a id="MXF_to_MP4_audio"></a>Ses akışını kodlama
Bu noktada, orijinal sıkıştırılmamış ses akışının hala sıkıştırılması gerekir. Ses akışının sıkıştırılaması için iş akışına bir AAC Encoder (Dolby) bileşeni ekleyin.

![Bağlantısız AVC Kodlayıcı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Bağlantısız AAC kodlayıcısı*

Şimdi bir uyumsuzluk var: AAC Encoder'dan yalnızca tek bir sıkıştırılmamış ses giriş pini var, medya dosyası girişi ise büyük olasılıkla iki farklı sıkıştırılmamış ses akışına sahip olacak: biri sol ses kanalı için, diğeri sağ için. (Surround ses ile ilgileniyorsanız, bu altı kanal's.) Bu nedenle, Media File Input kaynağındaki sesi Doğrudan AAC ses kodlayıcısına bağlamak mümkün değildir. AAC bileşeni sözde "interleaved" ses akışı bekler: hem sol hem de sağ kanallar birbirleri ile interleaved olan tek bir akış. Kaynak medya dosyamızdan ses parçalarının kaynaktaki konumda olduğunu bildiğimizde, sol ve sağ için doğru atanmış hoparlör konumlarıyla bu tür aralar arası ses akışı oluşturabiliriz.

İlk olarak, gerekli kaynak ses kanallarından bir interleaved akışı oluşturmak istiyor. Audio Stream Interleaver bileşeni bizim için bu işler. İş akışına ekleyin ve Medya Dosyası Girişi'ndeki ses çıktılarını ona bağlayın.

![Bağlı Ses Akışı Interleaver](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Bağlı Ses Akışı Interleaver*

Artık bir ara ses akışımız olduğuna göre, hala sol veya sağ hoparlör pozisyonlarını nereye atayabileceğimizi belirtmedik. Bunu belirtmek için, Hoparlör Pozisyon Atleyicisi'nden yararlanabiliriz.

![Hoparlör Pozisyon Atleyicisi Ekleme](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*Hoparlör Pozisyon Atleyicisi Ekleme*

"Özel" encoder Ön Ayar Filtresi ve "2.0 (L,R)" adı verilen Kanal Ön Kümesi aracılığıyla stereo giriş akışıyla kullanılmak üzere Hoparlör Pozisyon Atsigner'ını yapılandırın. (Bu kanal 1 için sol hoparlör konumunu ve kanal 2 sağ hoparlör konumunu atar.)

Hoparlör Pozisyon Atleyicisi çıktısını AAC Encoder'ın girişine bağlayın. Daha sonra, AAC Encoder bir "2.0 (L,R)" Kanal Preset ile çalışmak için söyle, bu yüzden giriş olarak stereo ses ile başa çıkmak için bilir.

### <a name="multiplexing-audio-and-video-streams-into-an-mp4-container"></a><a id="MXF_to_MP4_audio_and_fideo"></a>Ses ve Video akışlarını MP4 kapsayıcısına çok katmanlama
Bizim AVC kodlanmış video akışı ve AAC kodlanmış ses akışı göz önüne alındığında, biz bir içine hem yakalayabilir . MP4 konteyner. Farklı akışları tek bir akışa karıştırma işlemine "çoklama" (veya "muxing") denir. Bu durumda, ses ve video akışlarını tek bir tutarlı olarak iç içe bırakıyoruz. MP4 paketi. Bunu bir . MP4 konteyner ISO MPEG-4 Multiplexer denir. Tasarımcı yüzeyine bir tane ekleyin ve hem AVC Video Encoder'ı hem de AAC Encoder'ı girişlerine bağlayın.

![Bağlı MPEG4 Multiplexer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Bağlı MPEG4 Multiplexer*

### <a name="writing-the-mp4-file"></a><a id="MXF_to_MP4_writing_mp4"></a>MP4 dosyasını yazma
Çıktı dosyası yazarken, Dosya Çıktısı bileşeni kullanılır. Bunu ISO MPEG-4 Multiplexer'ın çıkışına bağlayabiliriz, böylece çıktısı diske yazılır. Bunu yapmak için, Kapsayıcı (MPEG-4) çıkış pinini Dosya Çıktısı'nın Giriş Yaz pinine bağlayın.

![Bağlı Dosya Çıktısı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Bağlı Dosya Çıktısı*

Kullanılan dosya adı Dosya özelliği tarafından belirlenir. Bu özellik belirli bir değere sabitlenebilir olsa da, büyük olasılıkla bir ifade yerine ayarlamak istiyor.

İş akışının bir ifadeden çıktı Dosya adı özelliğini otomatik olarak belirlemesi için, Dosya adının yanındaki (klasör simgesinin yanındaki) düğmesini tıklatın. Açılan menüden "İfade" seçeneğini belirleyin. Bu ifade editörü getiriyor. Önce editörün içeriğini temizleyin.

![Boş İfade Düzenleyicisi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Boş İfade Düzenleyicisi*

İfade düzenleyicisi, bir veya daha fazla değişkenle karışık herhangi bir gerçek değeri girmenizi sağlar. Değişkenler bir dolar işareti ile başlar. $ tuşuna bastığınızda, düzenleyici kullanılabilir değişken seçenekleriiçeren bir açılır kutu gösterir. Bizim durumumuzda çıktı dizini değişkeni ve temel giriş dosya adı değişkeninin bir birleşimini kullanacağız:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![İfade Düzenleyicisi Doldurulur](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*İfade Düzenleyicisi Doldurulur*

> [!NOTE]
> Kodlama işinizin çıktı dosyasını Azure'da görmek için ifade düzenleyicisinde bir değer sağlamanız gerekir.
>
>

Ok tuşuna basarak ifadeyi onayladığınızda, özellik penceresi Dosya özelliğinin şu anda hangi değeri çözdüğünü öngörünür.

![Dosya İfadesi çıktı dir giderir](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Dosya İfadesi çıktı dir giderir*

### <a name="creating-a-media-services-asset-from-the-output-file"></a><a id="MXF_to_MP4_asset_from_output"></a>Çıktı dosyasından Bir Medya Hizmetleri Varlığı Oluşturma
Bir MP4 çıktı dosyası yazmış olsak da, yine de bu dosyanın, medya hizmetlerinin bu iş akışının yürütülmesi sonucunda oluşturduğu çıktı varlığına ait olduğunu belirtmemiz gerekir. Bu amaçla, iş akışı tuvalindeki Çıktı Dosyası/Varlık düğümü kullanılır. Bu düğüme gelen tüm dosyalar, ortaya çıkan Azure Medya Hizmetleri varlığının bir parçasını haline getirin.

İş akışını tamamlamak için Dosya Çıktısı bileşenini Çıktı Dosyası/Varlık bileşenine bağlayın.

![Bitmiş İş Akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Bitmiş İş Akışı*

### <a name="test-the-finished-workflow-locally"></a><a id="MXF_to_MP4_test"></a>Tamamlanan iş akışını yerel olarak test edin
İş akışını yerel olarak test etmek için üstteki araç çubuğundaki oynat düğmesine basın. İş akışı yürütme tamamlandığında, yapılandırılan çıktı klasöründe oluşturulan çıktıyı inceleyin. MXF giriş kaynak dosyasından kodlanmış bitmiş MP4 çıkış dosyasını görürsünüz.

## <a name="encoding-mxf-into-mp4---multibitrate-dynamic-packaging-enabled"></a><a id="MXF_to_MP4_with_dyn_packaging"></a>MXF'nin MP4'e kodlanması - çoklu bit hızında dinamik ambalaj etkin
Bu walkthrough tek bir AAC kodlanmış ses ile birden çok bitrate MP4 dosyaları kümesi oluşturur. MXF giriş dosyası.

Azure Media Services tarafından sunulan Dinamik Ambalaj özellikleriyle birlikte kullanılmak üzere çok bit hızında bir varlık çıkışı istendiğinde, her biri farklı bir bit hızı ve çözünürlüğün birden fazla GOP uyumlu MP4 dosyasının oluşturulması gerekir. Bunu yapmak için, [tek bir bitrate MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) walkthrough içine Kodlama MXF iyi bir başlangıç noktası ile bize sağlar.

![İş Akışını Başlatma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*İş Akışını Başlatma*

### <a name="adding-one-or-more-additional-mp4-outputs"></a><a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Bir veya daha fazla ek MP4 çıkışı ekleme
Ortaya çıkan Azure Medya Hizmetleri varlığımızdaki her MP4 dosyası farklı bir bitrate ve çözünürlüğü destekler. İş akışına bir veya daha fazla MP4 çıktı dosyası ekleyelim.

Tüm video kodlayıcılarımızın aynı ayarlarla oluşturulduğundan emin olmak için, mevcut AVC Video Kodlayıcıyı çoğaltmak ve başka bir çözünürlük ve bit hızı kombinasyonunu yapılandırmak en uygun uyruk (2,5 Mbps saniyede 25 karede 960 x 540'dan birini ekleyelim ). Varolan kodlayıcıyı çoğaltmak için, tasarımcı yüzeyine yapıştırın kopyalayın.

Media File Girişi'nin Sıkıştırılmamış Video çıkış pinini yeni AVC bileşenimize bağlayın.

![İkinci AVC kodlayıcısı bağlı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*İkinci AVC kodlayıcısı bağlı*

Şimdi yeni AVC kodlayıcımızın yapılandırmasını 2,5 Mbps'de 960x540 çıkışına uyarlayın. (Bunun için "Çıkış genişliği", "Çıkış yüksekliği" ve "Bitrate (kbps)" özelliklerini kullanın.)

Ortaya çıkan varlığı Azure Media Services'In dinamik ambalajı ile birlikte kullanmak istediğimizgöz önüne alındığında, akış bitiş noktasının, farklı bithızları arasında geçiş yapan istemcilerin tek bir sorunsuz sürekli video ve ses deneyimi elde ettiği şekilde birbirine tam olarak hizalanmış olan bu MP4 dosyaları HLS/Parçalanmış MP4/DASH parçalarından üretebilmesi gerekir. Bunu gerçekleştirmek için, her iki MP4 dosyası için GOP ("resim grubu") boyutunun her iki AVC kodlayıcının özelliklerinde 2 saniyeye ayarlandığından emin olmalıyız:

* GOP Boyut Modunu SABIT GOP boyutuna ayarlama ve
* iki saniyeye anahtar kare aralığı.
* ayrıca gop IDR Denetimini kapalı GOP'a ayarla, tüm GOP'ların bağımlılıkları olmadan kendi başlarına ayakta durmalarını sağlamak için

Bu iş akışının anlaşılmasını kolaylaştırmak için, ilk AVC kodlayıcısını "AVC Video Encoder 640x360 1200 kbps" ve ikinci AVC kodlayıcısı "AVC Video Encoder 960x540 2500 kbps" olarak yeniden adlandırın.

Şimdi ikinci bir ISO MPEG-4 Multiplexer ve ikinci bir Dosya Çıktısı ekleyin. Çoklayıcıyı yeni AVC kodlayıcısına bağlayın ve çıktısının Dosya Çıktısı'na yönlendirildiklerinden emin olun. Ardından AAC ses kodlayıcı çıkışını yeni çoklayıcının girişine bağlayın. Sırayla Dosya Çıktısı, oluşturulacak Medya Hizmetleri Varlığına eklemek için Çıktı Dosyası/Varlık düğümüne bağlanabilir.

![İkinci Muxer ve Dosya Çıktısı bağlı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*İkinci Muxer ve Dosya Çıktısı bağlı*

Azure Media Services dinamik ambalajı ile uyumluluk için, çoklayıcının Yığın Modunu GOP sayısı veya süresine göre yapılandırın ve öbek başına GOP'ları 1 olarak ayarlayın. (Bu varsayılan olmalıdır.)

![Muxer Öbek Modları](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Muxer Öbek Modları*

Not: Varlık çıktısına eklemesini istediğiniz diğer bitrate ve çözünürlük kombinasyonları için bu işlemi yinelemek isteyebilirsiniz.

### <a name="configuring-the-file-output-names"></a><a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Dosya çıktı adlarını yapılandırma
Çıktı varlığına birden fazla dosya eklenmiştir. Bu, çıktı dosyalarının her biri için dosya adlarının birbirinden farklı olduğundan emin olmak ve hatta neyle uğraştığınızı dosya adından netbir şekilde açık hale getirmek için bir dosya adlandırma kuralı uygulama gereksinimi sağlar.

Dosya çıktısı adlandırma tasarımcıdaki ifadeler aracılığıyla denetlenebilir. Dosya Çıktısı bileşenlerinden biri için özellik bölmesini açın ve Dosya özelliğiiçin ifade düzenleyicisini açın. İlk çıktı dosyamız aşağıdaki ifade ile yapılandırıldı [(MXF'den tek bitrate MP4 çıkışına](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)geçmek için verilen öğreticiye bakın):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

Bu, dosya adımızın iki değişkentarafından belirlendiği anlamına gelir: yazış için çıkış dizini ve kaynak dosya temel adı. Eski iş akışı köküüzerinde bir özellik olarak maruz kalır ve ikinci gelen dosya tarafından belirlenir. Çıktı dizini, yerel sınama için kullandığınız dır; iş akışı Azure Medya Hizmetleri'ndeki bulut tabanlı medya işlemcisi tarafından yürütüldüğünde, bu özellik iş akışı altyapısı tarafından geçersiz kılınacaktır.
Her iki çıktı dosyamıza da tutarlı bir çıktı adlandırması sağlamak için, ilk dosya adlandırma ifadesini şu şekilde değiştirin:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

ve ikinci:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Her iki MP4 çıktı dosyasının düzgün oluşturulduğundan emin olmak için bir ara test çalışması yürütün.

### <a name="adding-a-separate-audio-track"></a><a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Ayrı bir Ses Parçası Ekleme
Daha sonra mp4 çıkış dosyalarımızla birlikte bir .ism dosyası oluşturduğumuzda göreceğimiz gibi, uyarlanabilir akışımız için ses parçası olarak yalnızca sese özel bir MP4 dosyasına da ihtiyacımız olacak. Bu dosyayı oluşturmak için iş akışına ek bir muxer ekleyin (ISO-MPEG-4 Multiplexer) ve AAC kodlayıcının çıkış pinini Track 1 giriş pimi ile bağlayın.

![Ses Muxer Eklendi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Ses Muxer Eklendi*

Muxer giden akışı çıktı ve dosya adlandırma ifadesini olarak yapılandırmak için üçüncü bir Dosya Çıktısı bileşeni oluşturun:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Ses Muxer Dosya Çıktıoluşturma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Ses Muxer Dosya Çıktıoluşturma*

### <a name="adding-the-ism-smil-file"></a><a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>'yi ekleme. İsM SMIL Dosyası
Dinamik ambalajın Medya Hizmetleri varlığımızdaki hem MP4 dosyalarıyla (hem de yalnızca sesmp4'üyle) birlikte çalışabilmek için bir manifesto dosyasına ("SMIL" dosyası: Senkronize Multimedya Tümleştirme Dili) de ihtiyacımız vardır. Bu dosya, dinamik paketleme için hangi MP4 dosyalarının kullanılabildiği ve ses akışı için hangilerinin göz önünde bulundurulması gerektiğini Azure Medya Hizmetleri'ne gösterir. Tek bir ses akışına sahip bir MP4 kümesi için tipik bir manifesto dosyası şuna benzer:

```xml
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="https://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>
```

.ism dosyası, bir anahtar deyimi içinde, her bir MP4 video dosyasıiçin bir başvuru ve bu da bir (veya daha fazla) ses dosyası başvuruları sadece ses içeren bir MP4 içerir.

MP4'ler setimiz için manifesto dosyası oluşturma "AMS Manifest Writer" adlı bir bileşen aracılığıyla yapılabilir. Kullanmak için yüzeye sürükleyin ve üç Dosya Çıktısı bileşeninden "Tam Yaz" çıkış pinlerini AMS Manifest Writer girişine bağlayın. Ardından AMS Manifest Writer çıktısını Çıktı Dosyası/Kıymetine bağladıklarından emin olun.

Diğer dosya çıktı bileşenlerimizde olduğu gibi,.ism dosya çıktısı adını bir ifadeyle yapılandırın:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

Bitmiş iş akışımız aşağıdaki gibi görünür:

![Mp4 iş akışını çoklu bithızına çıkarmak için MXF'yi tamamladı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Mp4 iş akışını çoklu bithızına çıkarmak için MXF'yi tamamladı*

## <a name="encoding-mxf-into-multibitrate-mp4---enhanced-blueprint"></a><a id="MXF_to__multibitrate_MP4"></a>MXF'yi çoklu bit hızında MP4'e kodlama - geliştirilmiş plan
Önceki [iş akışı gözden](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) geçiriminde, tek bir MXF giriş varlığının çok bit hızında MP4 dosyaları, yalnızca sese sahip MP4 dosyası ve Azure Medya Hizmetleri dinamik ambalajı ile birlikte kullanılmak üzere bir manifesto dosyasına nasıl dönüştürülebileceğini gördük.

Bu izlenecek yol, bazı yönlerin nasıl geliştirilebildiğini ve daha kullanışlı hale getirilebildiğini gösterir.

### <a name="workflow-overview-to-enhance"></a><a id="MXF_to_multibitrate_MP4_overview"></a>Geliştirmek için iş akışına genel bakış
![Geliştirmek için multibitrate MP4 iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*Geliştirmek için multibitrate MP4 iş akışı*

### <a name="file-naming-conventions"></a><a id="MXF_to__multibitrate_MP4_file_naming"></a>Dosya Adlandırma Kuralları
Önceki iş akışında, çıktı dosya adları oluşturmak için temel olarak basit bir ifade belirtmiştik. Biz olsa bazı yineleme var: tüm tek tek çıktı dosyası bileşenleri bu ifade yi belirtti.

Örneğin, ilk video dosyası için dosya çıktı bileşenimiz şu ifadeyle yapılandırılır:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

İkinci çıkış videosu için şu gibi bir ifademiz var:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

Bu yinelemenin bir kısmını kaldırıp işleri daha yapılandırılabilir hale getirsek daha temiz, daha az hataya yatkın ve daha uygun olmaz mıydı? Neyse ki biz: bizim iş akışı kökü üzerinde özel özellikler oluşturmak için yeteneği ile birlikte tasarımcının ifade yetenekleri kolaylık ek bir katman sağlayacaktır.

Tek tek MP4 dosyalarının bitrates dosya adı yapılandırma sürücü varsayalım. Bu bithızları, dosya adı oluşturma ve yapılandırmak için erişilecekleri yerden tek bir merkezi yerde (grafiğimizin kökünde) yapılandırmayı hedefliyoruz. Bunu yapmak için, bitrate özelliğini hem AVC kodlayıcılarından iş akışımızın köküne kadar yayımlayarak başlarız, böylece hem kökten hem de AVC kodlayıcılarından erişilebilir hale gelir. (İki farklı noktada görüntülense bile, yalnızca bir temel değer vardır.)

### <a name="publishing-component-properties-onto-the-workflow-root"></a><a id="MXF_to__multibitrate_MP4_publishing"></a>Bileşen özelliklerini iş akışı köküne yayımlama
İlk AVC kodlayıcısını açın, Bitrate (kbps) özelliğine gidin ve açılır açılır kaynaktan Yayımla'yı seçin.

![Bitrate özelliğini yayımlama](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*Bitrate özelliğini yayımlama*

Yayımlama iletişim kutusunu iş akışı grafiğimizin kökünde yayımlayacak şekilde,yayınadınız, "video1bitrate" adı ve "Video 1 Bitrate" okunabilir ekran adı ile yapılandırın. "Akış Bitrates" adlı özel bir grup adını yapılandırın ve Yayımla'yı hit leştirin.

![Bitrate özelliğini yayımlama](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Bitrate özelliği için yayımlama iletişim kutusu*

Aynı özel grup "Streaming Bitrates" içinde ikinci AVC kodlayıcısının bithızı özelliği için aynı işlemi tekrarlayın ve "Video 2 Bitrate" ekran adı ile "video2bitrate" olarak adlandırın.

Şimdi iş akışı kök özelliklerini incelersek, yayımlanmış iki özelliği olan özel grubumuzun ortaya çıkışını görürsünüz. Her ikisi de kendi AVC kodlayıcı bitrate değerini yansıtır.

![İş akışı kökü nde yayınlanan bitrate destek](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

Bu özelliklere koddan veya bir ifadeden erişmek istediğimizde, bunu şu şekilde yapabiliriz:

* kökün hemen altındaki bir bileşenden satır kodundan: düğüm.getPropertyAsString('... /video1bitrate', null)
* bir ifade içinde: ${ROOT_video1bitrate}

"Streaming Bitrates" grubunu, üzerinde de ses parça bitrate yayınlayarak tamamlayalım. AAC Encoder özellikleri içinde, Bitrate ayarını arayın ve yanındaki açılır yerden Yayımla'yı seçin. Grafiğin köküne "audio1bitrate" adı ile yayımlayın ve özel grubumuz "Akış Bitrates" içinde "Audio 1 Bitrate" adını görüntüleyin.

![Ses bithızı için yayımlama iletişim kutusu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*Ses bithızı için yayımlama iletişim kutusu*

![Kökte ortaya çıkan video ve ses destek](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*Kökte ortaya çıkan video ve ses destek*

Bu üç değerden herhangi birini değiştirmek, bağlı oldukları (ve yayımlandıkları) ilgili bileşenlerdeki değerleri yeniden yapılandırır ve değiştirir.

### <a name="have-generated-output-file-names-rely-on-published-property-values"></a><a id="MXF_to__multibitrate_MP4_output_files"></a>Çıktı dosya adları yayımlanmış özellik değerlerine güvenerek oluşturdu
Oluşturulan dosya adlarımızı kodlamak yerine, artık grafik kökünde yayınladığımız bitrate özelliklerine güvenmek için Dosya Çıktısı bileşenlerinin her birinde dosya adı ifademizi değiştirebiliriz. İlk dosya çıktımızdan başlayarak, Dosya özelliğini bulun ve ifadeyi şu şekilde de

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

Bu ifadedeki farklı parametrelere, ifade penceresinde klavyedeki dolar işaretine basılarak erişilebilir ve girilebilir. Mevcut parametrelerden biri daha önce yayınladığımız video1bitrate özelliğimizdir.

![Bir ifade içindeki parametrelere erişim](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Bir ifade içindeki parametrelere erişim*

İkinci videomuz için dosya çıktısı için de aynısını yapın:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

ve yalnızca ses dosyası çıktısı için:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Şimdi herhangi bir video veya ses dosyası için bitrate değiştirirsek, ilgili kodlayıcı yeniden yapılandırılır ve bitrate tabanlı dosya adı kuralı tüm otomatik onurlandırılır.

## <a name="adding-thumbnails-to-multibitrate-mp4-output"></a><a id="thumbnails_to__multibitrate_MP4"></a>Çoklu bithızında MP4 çıkışına küçük resim ekleme
[Bir MXF girişinden çok bit hızında MP4 çıkışı](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)üreten bir iş akışından başlayarak, şimdi çıktıya küçük resimler eklemeye bakacağız.

### <a name="workflow-overview-to-add-thumbnails-to"></a><a id="thumbnails_to__multibitrate_MP4_overview"></a>Küçük resimler eklemek için iş akışına genel bakış
![Başlamak için multibitrate MP4 iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*Başlamak için multibitrate MP4 iş akışı*

### <a name="adding-jpg-encoding"></a><a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>JPG Kodlama ekleme
Bizim küçük resim nesil kalp JPG Encoder bileşeni olacak, JPG dosyaları çıktı mümkün.

![JPG Kodlayıcı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*JPG Kodlayıcı*

Ancak, Medya Dosya Girişi'nden sıkıştırılmamış Video akışımızı JPG kodlayıcısına doğrudan bağlayamayız. Bunun yerine, tek tek çerçeveler teslim edilmesi bekliyor. Bunu, Video Frame Gate bileşeni ile yapabilirsiniz.

![JPG kodlayıcısına bir çerçeve kapısı bağlama](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*JPG kodlayıcısına bir çerçeve kapısı bağlama*

Kare kapısı her bu kadar saniye veya kare bir video çerçevesi geçmesine izin verir. Bunun gerçekleştiği aralık ve zaman mahsup ları özelliklerde yapılandırılabilir.

![Video Frame Gate özellikleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Video Frame Gate özellikleri*

Modu Time (saniye) ve Interval'i 60'a ayarlayarak her dakika bir küçük resim oluşturalım.

### <a name="dealing-with-color-space-conversion"></a><a id="thumbnails_to__multibitrate_MP4_color_space"></a>Renk Alanı dönüştürme ile başa çıkma
Çerçeve kapısının sıkıştırılmamış Video pinleri ve Medya Dosyası Girişi artık bağlanabilir mantıklı görünse de, bunu yaparsak bir uyarı alabiliriz.

![Giriş renk alanı hatası](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Giriş renk alanı hatası*

Bunun nedeni, renk bilgilerinin MXF'mizden gelen orijinal ham sıkıştırılmamış video akışımızda temsil edilmesinin JPG Encoder'ın beklediğinden farklı olmasıdır. Daha spesifik olarak, "RGB" veya "Grayscale" bir sözde "renk alanı" akması bekleniyor. Bu, Video Frame Gate'in gelen video akışının önce renk alanıyla ilgili bir dönüşüm uygulaması gerektiği anlamına gelir.

Color Space Converter - Intel iş akışına sürükleyin ve çerçeve kapımıza bağlayın.

![Renk Alanı Dönüştürücü bağlama](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Renk Alanı Dönüştürücü bağlama*

Özellikler penceresinde, Önceden Ayarlanmış listeden BGR 24 girişini seçin.

### <a name="writing-the-thumbnails"></a><a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Küçük resimleri yazma
MP4 videomuzdan farklı olarak, JPG Encoder bileşeni birden fazla dosya çıkar. Bu başa çıkmak için, bir Sahne Arama JPG Dosya Yazar bileşeni kullanılabilir: gelen JPG küçük resimleri alır ve bunları yazar, her dosya adı farklı bir sayı ile suffixed olmak. (Genellikle küçük resmin çekildiği akıştaki saniye/birim sayısını gösteren sayı.)

![Sahne Arama JPG Dosya Yazar Tanıtımı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*Sahne Arama JPG Dosya Yazar Tanıtımı*

Çıktı Klasör Üle'si özelliğini ifadeyle yapılandır: ${ROOT_outputWriteDirectory}

ve Filename Önek özelliği ile:

    ${ROOT_sourceFileBaseName}_thumb_

Önek, küçük resim dosyalarının nasıl adlandırıldığını belirler. Bunlar, başparmağın akıştaki konumunu gösteren bir sayıyla sabitlenirler.

![Sahne Arama JPG Dosya Yazar özellikleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Sahne Arama JPG Dosya Yazar özellikleri*

Sahne Arama JPG Dosya Yazarı'nı Çıkış Dosyası/Varlık düğümüne bağlayın.

### <a name="detecting-errors-in-a-workflow"></a><a id="thumbnails_to__multibitrate_MP4_errors"></a>İş akışındaki hataları algılama
Renk alanı dönüştürücüsünün girişini ham sıkıştırılmamış video çıkışına bağlayın. Şimdi iş akışı için yerel bir test çalışması gerçekleştirin. İş akışının aniden yürütmeyi durdurma ve bir hatayla karşılaşan bileşende kırmızı bir anahatla işaret etme olasılığı yüksektir:

![Renk Alanı Dönüştürücü hatası](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Renk Alanı Dönüştürücü hatası*

Kodlama denemesinin başarısız olmasının nedenini görmek için Color Space Converter bileşeninin sağ üst köşesindeki küçük kırmızı "E" simgesini tıklatın.

![Renk Alanı Dönüştürücü hata iletişim kutusu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Renk Alanı Dönüştürücü hata iletişim kutusu*

Gördüğünüz gibi, renk alanı dönüştürücü için gelen renk uzayı standardı RGB yuv bizim istenen dönüşüm için rec601 olması gerekir çıkıyor. Görünüşe göre akışımız rec601'ini göstermiyor. (Rec 601, interlaced analog video sinyallerini dijital video formunda kodlamak için bir standarttır. Hat başına 720 parlaklık örneği ve 360 chrominance örneğini kapsayan aktif bir bölge belirtir. Renk kodlama sistemi YCbCr 4:2:2 olarak bilinir.)

Bunu düzeltmek için, akışımızın meta verilerinde rec601 içeriğiyle uğraştığımızı belirtiriz. Bunu yapmak için, ham kaynağımız ile renk alanı dönüştürme bileşeni arasına koyacağımız bir Video Veri Türü Güncelleyici bileşeni kullanacağız. Bu veri türü güncelleyici, belirli video veri türü özelliklerinin el ile güncelleştirmesine olanak tanır. "Rec 601" renk alanı standardını gösterecek şekilde yapılandırın. Bu, henüz tanımlanmamış renk alanı yoksa, Video Veri Türü Güncelleyici'nin akışı "Rec 601" renk alanıyla etiketlemesine neden olur. (Geçersiz Kılma onay kutusu işaretlenmedikçe varolan meta verileri geçersiz kılmaz.)

![Veri Türü Güncelleyici'de Renk Alanı Standardını Güncelleme](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Veri Türü Güncelleyici'de Renk Alanı Standardını Güncelleme*

### <a name="finished-workflow"></a><a id="thumbnails_to__multibitrate_MP4_finish"></a>Bitmiş İş Akışı
İş akışımız bittiğine göre, geçtiğini görmek için başka bir test çalışması yapın.

![Küçük resimlerle çoklu mp4 çıktısı için bitmiş iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*Küçük resimlerle çoklu mp4 çıktısı için bitmiş iş akışı*

## <a name="time-based-trimming-of-multibitrate-mp4-output"></a><a id="time_based_trim"></a>Çoklu bithızında MP4 çıkışının zamana dayalı kırpma
[Bir MXF girişinden çoklu bit hızında MP4 çıkışı](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)üreten bir iş akışından başlayarak, şimdi zaman damgalarına dayalı kaynak videoyu kırpmayı araştıracağız.

### <a name="workflow-overview-to-start-adding-trimming-to"></a><a id="time_based_trim_start"></a>Kırpma eklemeye başlamak için iş akışına genel bakış
![Kırpma eklemek için iş akışını başlatma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*Kırpma eklemek için iş akışını başlatma*

### <a name="using-the-stream-trimmer"></a><a id="time_based_trim_use_stream_trimmer"></a>Akış Düzelticisini Kullanma
Akış Düzeltici bileşeni, zamanlama bilgileri (saniye, dakika, ...) üzerinde bir giriş akışı tabanının başlangıcını ve sonunu kırpmanızı sağlar. Düzeltici çerçeve tabanlı kırpma desteklemez.

![Akış Düzeltici](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Akış Düzeltici*

AVC kodlayıcılarını ve hoparlör pozisyon atayan aracını doğrudan Medya Dosya Girişi'ne bağlamak yerine, akış düzelticinin arasına koyacağız. (Bir video sinyali için ve bir interleaved ses sinyali için.)

![Stream Trimmer'ı arada koyun](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*Stream Trimmer'ı arada koyun*

Düzelticiyi, videoda yalnızca 15 saniye ile 60 saniye arasında görüntüleyebilmemiz için yapılandıralım.

Video Akışı Düzeltici'nin özelliklerine gidin ve hem Başlangıç Zamanı (15 s) hem de Bitiş Saati (60 s) özelliklerini yapılandırın. Hem ses hem de video düzelticimizin her zaman aynı başlangıç ve bitiş değerlerine göre yapılandırıldığından emin olmak için, bunları iş akışının kökünde yayınlarız.

![Stream Düzeltici'den başlangıç saati özelliğini yayımlama](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*Stream Düzeltici'den başlangıç saati özelliğini yayımlama*

![Başlangıç saati için özellik iletişim kutusunu yayımlama](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*Başlangıç saati için özellik iletişim kutusunu yayımlama*

![Bitiş saati için özellik iletişim kutusunu yayımlama](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*Bitiş saati için özellik iletişim kutusunu yayımlama*

Şimdi iş akışımızın kökünü incelersek, her iki özellik de düzgün bir şekilde görüntülenir ve buradan yapılandırılabilir.

![Kök te sazda kinleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*Kök te sazda kinleri*

Şimdi ses düzelticiden kırpma özelliklerini açın ve iş akışımızın kökünde yayınlanan özellikleri ifade eden bir ifadeyle hem başlangıç hem de bitiş saatlerini yapılandırın.

Ses kırpma başlangıç saati için:

    ${ROOT_TrimmingStartTime}

ve bitiş saati için:

    ${ROOT_TrimmingEndTime}

### <a name="finished-workflow"></a><a id="time_based_trim_finish"></a>Bitmiş İş Akışı
![Bitmiş İş Akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Bitmiş İş Akışı*

## <a name="introducing-the-scripted-component"></a><a id="scripting"></a>Komut Dosyası Bileşeni yle TanıştırMa
Komut Dosyası Bileşenleri, iş akışımızın yürütme aşamalarında rasgele komut dosyaları çalıştırabilir. Her biri belirli özelliklere sahip ve iş akışı yaşam döngüsündeki kendi yeri olan, yürütülebilecek dört farklı komut dosyası vardır:

* **commandScript**
* **realizeScript**
* **processInputScript**
* **ömür DöngüsüScript**

Komut Dosyası Bileşeni'nin belgeleri yukarıdakilerin her biri için daha ayrıntılı olarak gider. [Aşağıdaki bölümde,](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)iş akışı başladığında anında bir klip listesi xml oluşturmak için **realizeScript** komut dosyası bileşeni kullanılır. Bu komut dosyası, yaşam döngüsünde yalnızca bir kez gerçekleşen bileşen kurulumu sırasında çağrılır.

### <a name="scripting-within-a-workflow-hello-world"></a><a id="scripting_hello_world"></a>İş akışı içinde komut dosyası: merhaba dünya
Komut Dosyası Bileşenini tasarımcı yüzeyine sürükleyin ve yeniden adlandırın (örneğin, "SetClipListXML").

![Komut Dosyası Bileşeni Ekleme](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Komut Dosyası Bileşeni Ekleme*

Komut Dosyası Bileşeni'nin özelliklerini incelediğinizde, her biri farklı bir komut dosyasına yapılandırılabilen dört farklı komut dosyası türü gösterilir.

![Komut Dosyası Bileşen özellikleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Komut Dosyası Bileşen özellikleri*

ProcessInputScript'i temizleyin ve realizeScript için düzenleyiciyi açın. Şimdi hazır ve komut dosyası başlatmaya hazırız.

Komut dosyaları, Java platformu için Java ile uyumluluğu koruyan dinamik olarak derlenmiş bir komut dosyası dili olan Groovy dilinde yazılmıştır. Aslında, çoğu Java kodu geçerli Groovy kodudur.

Bizim realizeScript bağlamında basit bir merhaba dünya groovy script yazalım. Editöre aşağıdakileri girin:

    node.log("hello world");

Şimdi yerel bir test çalışması yürütmek. Bu çalıştırmadan sonra, Günlükler özelliğini (Komut Dosyası Bileşenindeki Sistem sekmesi nden) inceleyin.

![Merhaba dünya günlük çıktı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*Merhaba dünya günlük çıktı*

Günlük yöntemi dediğimiz düğüm nesnesi, geçerli "düğüm" veya içinde komut dosyası yaptığımız bileşeni ifade eder. Bu şekilde her bileşen, sistem sekmesi nden kullanılabilen günlük verisini çıktılama yeteneğine sahiptir. Bu durumda, string literal "merhaba dünya çıktı." Burada anlaşılması gereken önemli olan, bunun senaryonun gerçekte ne yaptığı hakkında size fikir veren paha biçilmez bir hata ayıklama aracı olduğukanıtlanabildiğidir.

Komut dosyası ortamımızın içinden, diğer bileşenlerdeki özelliklere de erişebiliriz. Şunu deneyin:

```java
    //inspect current node:
    def nodepath = node.getNodePath();
    node.log("this node path: " + nodepath);

    //walking up to other nodes:
    def parentnode = node.getParentNode();
    def parentnodepath = parentnode.getNodePath();
    node.log("parent node path: " + parentnodepath);

    //read properties from a node:
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null );
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null);
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);
```

Günlük penceremiz bize şunları gösterir:

![Düğüm yollarına erişmek için günlük çıktısı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*Düğüm yollarına erişmek için günlük çıktısı*

## <a name="frame-based-trimming-of-multibitrate-mp4-output"></a><a id="frame_based_trim"></a>Multibitrate MP4 çıkışının çerçeve tabanlı kırpma
[Bir MXF girişinden çoklu bit hızında MP4 çıkışı](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)üreten bir iş akışından başlayarak, şimdi çerçeve sayımlarına dayalı olarak kaynak videoyu kırpmayı araştıracağız.

### <a name="blueprint-overview-to-start-adding-trimming-to"></a><a id="frame_based_trim_start"></a>Kırpma eklemeye başlamak için plana genel bakış
![Kırpma eklemeye başlamak için iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Kırpma eklemeye başlamak için iş akışı*

### <a name="using-the-clip-list-xml"></a><a id="frame_based_trim_clip_list"></a>Klips Listesini Kullanma XML
Önceki tüm iş akışı öğreticilerinde, video giriş kaynağımız olarak Medya Dosya Girişi bileşenini kullandık. Ancak bu özel senaryo için, bunun yerine Klip Listesi Kaynağı bileşenini kullanıyor olacağız. Bu çalışma tercih edilen yolu olmamalıdır; yalnızca bunu yapmak için gerçek bir neden olduğunda (aşağıdaki durumda olduğu gibi, klip listesi kırpma özelliklerinden yararlandığımız) Clip List Kaynağını kullanın.

Medya Dosya Girişi'nden Klip Listesi Kaynağı'na geçmek için, Klip Listesi Kaynak bileşenini tasarım yüzeyine sürükleyin ve Klip Listesi XML pinini iş akışı tasarımcısının Klip Listesi XML düğümüne bağlayın. Bu, giriş videomuza göre Klip Listesi Kaynağını çıktı pimleriyle doldurur. Şimdi Klip Listesi Kaynağından Sıkıştırılmamış Video ve Sıkıştırılmamış Ses pinlerini ilgili AVC Kodlayıcılara ve Audio Stream Interleaver'a bağlayın. Şimdi Medya Dosya Girişi'ni kaldırın.

![Ortam Dosya Girişi'ni Klip Listesi Kaynağıile Değiştirdi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*Ortam Dosya Girişi'ni Klip Listesi Kaynağıile Değiştirdi*

Klip Listesi Kaynak bileşeni girişi olarak bir "Klip Listesi XML alır." Yerel olarak test etmek için kaynak dosyaseçerken, bu klip listesi xml sizin için otomatik doldurulur.

![Otomatik doldurulan Klip Listesi XML özelliği](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Otomatik doldurulan Klip Listesi XML özelliği*

xml biraz daha yakın arıyorsunuz, bu nasıl görünüyor:

![Klips listesini edin iletişim kutusunu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Klips listesini edin iletişim kutusunu*

Ancak bu, klip listesi xml yeteneklerini yansıtmaz. Sahip olduğumuz seçeneklerden biri, hem video hem de ses kaynağının altına şu şekilde bir "Kırpma" öğesi eklemektir:

![Klip listesine kırpma öğesi ekleme](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*Klip listesine kırpma öğesi ekleme*

Yukarıdaki gibi klip listesi xml değiştirir ve yerel bir test çalışması gerçekleştirirseniz, videonun videoda 10 ila 20 saniye arasında doğru şekilde kırpıldığını görürsünüz.

Yine de yerel bir çalıştırma yaptığınızda olanların aksine, bu aynı klip listesi xml, Azure Medya Hizmetleri'nde çalışan bir iş akışında uygulandığında aynı etkiye sahip olmaz. Azure Premium Encoder başlatıldığında, kodlama işinin verildiği giriş dosyasına göre, cliplist xml her seferinde yeniden oluşturulur. Bu xml üzerinde yaptığımız herhangi bir değişiklik ne yazık ki geçersiz olacağını anlamına gelir.

Bir kodlama işi başlatıldığında cliplist xml'in silinmesini karşıkoymak için, iş akışımız başladıktan hemen sonra anında yeniden oluşturabiliriz. Bu tür özel eylemler "Komut Dosyası Bileşeni" adı verilen bir eylem üzerinden alınabilir. Daha fazla bilgi için [bkz.](media-services-media-encoder-premium-workflow-tutorials.md#scripting)

Komut Dosyası Bileşenini tasarımcı yüzeyine sürükleyin ve "SetClipListXML" olarak yeniden adlandırın.

![Komut Dosyası Bileşeni Ekleme](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Komut Dosyası Bileşeni Ekleme*

Komut Dosyası Bileşeni'nin özelliklerini incelediğinizde, her biri farklı bir komut dosyasına yapılandırılabilen dört farklı komut dosyası türü gösterilir.

![Komut Dosyası Bileşen özellikleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Komut Dosyası Bileşen özellikleri*

### <a name="modifying-the-clip-list-from-a-scripted-component"></a><a id="frame_based_trim_modify_clip_list"></a>Komut Dosyası Bileşeninden klip listesini değiştirme
İş akışı başlatma sırasında oluşturulan cliplist xml'i yeniden yazabilmek için, kliplistesi xml özelliğine ve içeriğine erişmemiz gerekir. Bunu şöyle yapabiliriz:

```java
    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);
```

![Gelen klip listesi günlüğe kaydediliyor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*Gelen klip listesi günlüğe kaydediliyor*

Öncelikle videoyu hangi noktadan hangi noktadan hangi noktaya kadar kesmek istediğimizi belirlemenin bir yolunu bulmalıyız. İş akışının daha az teknik kullanıcı için uygun hale getirmek için, grafiğin kökünde iki özellik yayımlayın. Bunu yapmak için, tasarımcı yüzeyine sağ tıklayın ve "Özellik Ekle"yi seçin:

* İlk özellik: "ClippingTimeStart" türü: "TIMECODE"
* İkinci özellik: "ClippingTimeEnd" türü: "TIMECODE"

![Başlangıç saatini kırpmak için Özellik iletişim kutusu ekle](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Başlangıç saatini kırpmak için Özellik iletişim kutusu ekle*

![İş akışı kökünde yayınlanmış kırpma zamanı destekleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*İş akışı kökünde yayınlanmış kırpma zamanı destekleri*

Her iki özelliği de uygun bir değere göre yapılandırın:

![Kırpma başlangıç ve bitiş özelliklerini yapılandırma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*Kırpma başlangıç ve bitiş özelliklerini yapılandırma*

Şimdi, senaryomuzun içinden, her iki özele de erişebiliriz, şöyle:

```java
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);
```

![Kırpma başlangıç ve bitişini gösteren günlük penceresi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Kırpma başlangıç ve bitişini gösteren günlük penceresi*

Basit bir normal ifade kullanarak zaman kodu dizelerini daha kullanışlı bir biçime ayrıştıralım:

```java
    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse the end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);
    node.log("framerate end is: " + endframerate);
```

![Ayrıştırılmış zaman kodu çıktısı içeren günlük penceresi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Ayrıştırılmış zaman kodu çıktısı içeren günlük penceresi*

Elimizdeki bu bilgilerle, filmin istenilen kare doğru kırpma için başlangıç ve bitiş saatlerini yansıtacak şekilde cliplist xml'i değiştirebiliriz.

![Kırpma öğeleri eklemek için komut dosyası kodu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*Kırpma öğeleri eklemek için komut dosyası kodu*

Bu normal dize işleme işlemleri ile yapıldı. Elde edilen değiştirilmiş klip listesi xml "setProperty" yöntemi ile iş akışı kökünde clipListXML özelliğine geri yazılır. Başka bir test çalışmasından sonraki günlük penceresi bize aşağıdakileri gösterir:

![Ortaya çıkan klip listesini günlüğe kaydetme](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*Ortaya çıkan klip listesini günlüğe kaydetme*

Video ve ses akışlarının nasıl kırpıldığını görmek için bir test çalışması yapın. Kırpma noktaları için farklı değerlerle birden fazla test çalışması yapacağınız için, bunların dikkate alınmadığını fark edeceksiniz! Bunun nedeni, tasarımcının Azure çalışma süresinin aksine, her çalıştırmada klip listesi xml'i geçersiz kılmamasıdır. Bu, yalnızca ilk giriş ve çıkış noktalarını belirlediğiniz de xml'in dönüşmesine neden olacağı anlamına`clipListXML.indexOf("<trim>") == -1`gelir, diğer tüm zamanlarda, koruma maddemiz (if ( )) zaten bir tane mevcut olduğunda iş akışının başka bir kırpma öğesi eklemesini engelleyecektir.

İş akışımızı yerel olarak test etmek için kullanışlı hale getirmek için, bir kırpma öğesinin zaten mevcut olup olmadığını denetleyen bazı ev tutma kodu eklemeyi en iyi şekilde ekleriz. Eğer öyleyse, biz yeni değerler ile xml değiştirerek devam etmeden önce kaldırabilirsiniz. Düz dize manipülasyonları kullanmak yerine, bunu gerçek xml nesne modeli ayrıştma yoluyla yapmak muhtemelen daha güvenlidir.

Ancak bu tür bir kod eklemeden önce, önce komut dosyamızın başında bir dizi alma deyimi eklememiz gerekir:

```java
    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;
```

Bundan sonra, gerekli temizlik kodunu ekleyebilirsiniz:

```java
    //for local testing: delete any pre-existing trim elements from the clip list xml by parsing the xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about to delete any existing trim nodes");
     //delete the trim nodes:
    elementsToDelete.each{
        e -> e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();
```

Bu kod, kırpma öğelerini kliplistesi xml'e eklediğimiz noktanın hemen üzerinde dir.

Bu noktada, değişiklikleri her zaman uygularken iş akışımızı istediğimiz kadar çalıştırıp değiştirebiliriz.    

### <a name="adding-a-clippingenabled-convenience-property"></a><a id="frame_based_trim_clippingenabled_prop"></a>Kırpma EklemeEtkin kolaylık özelliği
Her zaman kırpma olmasını istemeyebilirsiniz, kırpma / kırpma sağlamak isteyip istemediğimizi gösteren uygun bir boolean bayrak ekleyerek iş akışımızı bitirelim.

Daha önce olduğu gibi, "BOOLEAN" türünden "Kırpma Etkin" adı verilen iş akışımızın kökünde yeni bir özellik yayımlayın.

![Kırpma özelliğini etkinleştirmek için yayımlanmış bir özellik](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*Kırpma özelliğini etkinleştirmek için yayımlanmış bir özellik*

Aşağıdaki basit koruma maddesi ile, kırpma gerekip gerekmediğini kontrol edebilir ve bu tür olarak klip listemizin değiştirilmesi gerekip gerekmediğini belirleyebiliriz.

```java
    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }
```

### <a name="complete-code"></a><a id="code"></a>Kodu tamamla

```java
    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse the end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);

    //for local testing: delete any pre-existing trim elements
    //from the clip list xml by parsing the xml into a DOM:

    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about to delete any existing trim nodes");
    //delete the trim nodes:
    elementsToDelete.each{ e ->
        e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }

    //add trim elements to cliplist xml
    if ( clipListXML.indexOf("<trim>") == -1 )
    {
        //trim video
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode +
            " </outPoint>\n </trim> \n");
        //trim audio
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" +
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML );
        node.setProperty("../clipListXml",clipListXML);
    }
```

## <a name="also-see"></a>Ayrıca bkz.
[Azure Medya Hizmetlerinde Premium Kodlama Ile Tanış](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Azure Medya Hizmetlerinde Premium Kodlama Nasıl Kullanılır?](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[Azure Medya Hizmeti ile İsteğe Bağlı İçeriği Kodlama](media-services-encode-asset.md#media-encoder-premium-workflow)

[Media Encoder Premium İş Akışı Biçimleri ve Kod Çözücüleri](media-services-premium-workflow-encoder-formats.md)

[Örnek iş akışı dosyaları](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Azure Media Services Gezgini aracı](https://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
