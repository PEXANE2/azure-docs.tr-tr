---
title: Video Dizinleyici hakkında sık sorulan sorular - Azure
titleSuffix: Azure Media Services
description: Bu makalede, Azure Medya Hizmetleri Video Dizinleyici hakkında sık sorulan soruların yanıtlarını verir.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 04/20/2020
ms.author: juliako
ms.openlocfilehash: dc57978dd881532cab59150dec921df9ffa958c3
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81767206"
---
# <a name="video-indexer-frequently-asked-questions"></a>Video Indexer sık sorulan sorular

Bu makalede, Video Dizinleyici hakkında sık sorulan soruları yanıtlar.

## <a name="general-questions"></a>Genel sorular

### <a name="what-is-video-indexer"></a>Video Indexer nedir?

Video Indexer, Microsoft Azure Medya Hizmetleri'nin bir parçası olan bir yapay zeka hizmetidir. Video Indexer kolayca bir video derin içgörü ayıklamak sağlayan birden fazla makine öğrenme modelleri bir orkestrasyon sağlar. Gelişmiş ve doğru öngörüler sağlamak için Video Indexer videonun birden çok kanalını kullanır: ses, konuşma ve görsel. Video Indexer'ın öngörüleri, içerik bulunabilirliğini ve erişilebilirliğini geliştirmek, yeni para kazanma fırsatları yaratmak veya öngörüleri kullanan yeni deneyimler oluşturmak gibi birçok şekilde kullanılabilir. Video Indexer, hesabınızdaki modelleri test etmek, yapılandırmak ve özelleştirmek için web tabanlı bir arayüz sağlar. Geliştiriciler, Video Indexer'ı üretim sistemine entegre etmek için REST tabanlı bir API kullanabilir. 

### <a name="what-can-i-do-with-video-indexer"></a>Video Indexer ile ne yapabilirim?

Video Dizinleyici'nin medya dosyalarında gerçekleştirebileceği işlemlerden bazıları şunlardır:

* Konuşmanın tanımlanması ve ayıklanması ve konuşmacıların tanımlanması.
* Videodaki ekran metnini tanımlama ve ayıklama.
* Video dosyasındaki nesneleri algılama.
* Ses parçalarından ve videodaki ekran metninden markaları (örneğin: Microsoft) tanımlayın.
* Ünlülerin bir veritabanından ve kullanıcı tarafından tanımlanan yüzveritabanından yüzleri algılama ve tanıma.
* Tartışılan ancak ses ve video içeriğinde mutlaka belirtilmeyen konuların ayıklanması.
* Ses parçasından kapalı altyazılar veya altyazılar oluşturma.

Daha fazla bilgi ve daha fazla Video Indexer özelliği için [genel bakış](video-indexer-overview.md)alabakın.

### <a name="how-do-i-get-started-with-video-indexer"></a>Video Indexer ile nasıl başlarım?

Video Indexer, web tabanlı arabirimde 600 dakika ve API üzerinden 2.400 dakika sağlayan ücretsiz deneme teklifi içerir. Video [Indexer web tabanlı arabirimine giriş](https://www.videoindexer.ai/) yapabilir ve herhangi bir web kimliğini kullanarak ve bir Azure Aboneliği ayarlamaya gerek kalmadan kendiniz deneyebilirsiniz. Video Indexer'ın nasıl kullanılacağı hakkında daha iyi bir fikir edinmek için [bu kolay giriş laboratuarını](https://github.com/Azure-Samples/media-services-video-indexer/blob/master/IntroToVideoIndexer.md) izleyin.

Videoları ve ses uçlarını ölçekte dizine bağlamak için Video Indexer'ı ücretli bir Microsoft Azure aboneliğine bağlayabilirsiniz. Fiyatlandırma hakkında daha fazla bilgiyi [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/) sayfasında bulabilirsiniz.

Başlangıç hakkında daha fazla bilgiyi [Başlangıç'a başlayın'](video-indexer-get-started.md)da bulabilirsiniz.

### <a name="do-i-need-coding-skills-to-use-video-indexer"></a>Video Dizinleyici'yi kullanmak için kodlama becerilerine ihtiyacım var mı?

Video Indexer web tabanlı arabirimini kullanarak hesabınızı **kodlama gerektirmeden**değerlendirebilir, yapılandırabilir ve yönetebilirsiniz.  Daha karmaşık uygulamalar geliştirmeye hazır olduğunuzda, Azure Mantık Uygulamaları veya Azure İşlevleri [gibi sunucusuz teknolojileri kullanarak](https://azure.microsoft.com/blog/logic-apps-flow-connectors-will-make-automating-video-indexer-simpler-than-ever/) Video Indexer'ı kendi uygulamalarınıza, web sitelerine veya özel iş akışlarına entegre etmek için Video [Dizinleyici API'sini](https://api-portal.videoindexer.ai/) kullanabilirsiniz.

### <a name="do-i-need-machine-learning-skills-to-use-video-indexer"></a>Video Indexer'ı kullanmak için makine öğrenimi becerilerine ihtiyacım var mı?

Hayır, Video Indexer birden çok makine öğrenme modelinin tek bir ardışık boru hattına entegrasyonunu sağlar. Video Indexer aracılığıyla bir video veya ses dosyasıdizin dizilişi, müşteri adına gerekli algoritmalar hakkında herhangi bir makine öğrenme becerisi veya bilgisi olmadan paylaşılan bir zaman çizelgesinde çıkarılan tam bir öngörü kümesini alır.

### <a name="what-media-formats-does-video-indexer-support"></a>Video Dizinleyici hangi medya biçimlerini destekler?

Video Indexer en yaygın medya biçimlerini destekler. Daha fazla ayrıntı için [Azure Media Encoder standart biçimleri](https://docs.microsoft.com/azure/media-services/latest/media-encoder-standard-formats) listesine bakın.

### <a name="how-do-i-upload-a-media-file-into-video-indexer-and-what-are-the-limitations"></a>Video Dizinleyici'ye bir medya dosyasını nasıl yüklerim ve sınırlamalar nelerdir?

Video Indexer web tabanlı portalında, dosya yükleme iletişim kutusunu kullanarak veya kaynak dosyayı doğrudan barındıran bir URL'yi işaret ederek bir medya dosyası yükleyebilirsiniz [(örneğe](https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/Ignite-short.mp4)bakın). iFrame veya gömme kodu kullanarak medya içeriğini barındıran herhangi bir URL çalışmaz [(örneğe](https://www.videoindexer.ai/accounts/7e1282e8-083c-46ab-8c20-84cae3dc289d/videos/5cfa29e152/?t=4.11)bakın). 

Daha fazla bilgi için lütfen bu [nasıl yapılacağınız kılavuzuna](https://docs.microsoft.com/azure/media-services/video-indexer/upload-index-videos)bakın.

#### <a name="limitations"></a>Sınırlamalar

* Videonun adı 80 karakteri geçemez.
* Bayt dizisini kullanarak bir video yüklerseniz, video boyutu 2 GB (ve URL kullanırken 30 GB) ile sınırlıdır. 

Kapsamlı liste için [bkz.](upload-index-videos.md#uploading-considerations-and-limitations)

### <a name="how-long-does-it-take-video-indexer-to-extract-insights-from-media"></a>Video Indexer'ın medyadan bilgi alması ne kadar sürer?

Video Dizinleyici API ve Video Indexer web tabanlı arabirimi kullanarak bir video veya ses dosyasını dizine dizine ayırmasüresi, dosya uzunluğu ve kalitesi, dosyada bulunan öngörü sayısı, kullanılabilir [ayrılmış birim](https://docs.microsoft.com/azure/media-services/previous/media-services-scale-media-processing-overview) sayısı ve akış [bitiş noktasının](https://docs.microsoft.com/azure/media-services/previous/media-services-streaming-endpoints-overview) etkin olup olmadığı gibi birden çok parametreye bağlıdır. Daha iyi bir fikir elde etmek için kendi içeriğinizle birkaç test dosyası çalıştırmanızı ve ortalama almanızı öneririz.

### <a name="can-i-create-customized-workflows-to-automate-processes-with-video-indexer"></a>Video Dizinleyici ile işlemleri otomatikleştirmek için özelleştirilmiş iş akışları oluşturabilir miyim?

Evet, Video Dizinleyici'yi Logic Apps, Flow ve [Azure Fonksiyonları](https://azure.microsoft.com/services/functions/)gibi sunucusuz teknolojilere entegre edebilirsiniz. Burada Video [Indexer](https://azure.microsoft.com/blog/logic-apps-flow-connectors-will-make-automating-video-indexer-simpler-than-ever/)için [Mantık App](https://azure.microsoft.com/services/logic-apps/) ve [Akış](https://flow.microsoft.com/en-us/) konektörleri hakkında daha fazla bilgi bulabilirsiniz. [Video Dizinleyici Örnekleri](https://github.com/Azure-Samples/media-services-video-indexer) repo'sunda iş ortakları tarafından yapılan bazı otomasyon projelerini görebilirsiniz.

### <a name="in-which-azure-regions-is-video-indexer-available"></a>Video dizinleyicihangi Azure bölgelerinde kullanılabilir?

[Bölgeler](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all) sayfasında hangi Azure bölgeleri Video Dizinleyici'nin kullanılabildiği görülür.

### <a name="can-i-customize-video-indexer-models-for-my-specific-use-case"></a>Özel kullanım örneğim için Video Indexer modellerini özelleştirebilir miyim? 

Evet. Video Indexer'da, gereksinimlerinize daha iyi uyacak şekilde kullanılabilir modellerden bazılarını özelleştirebilirsiniz. 

Örneğin, Person modelimiz 1.000.000 ünlü tanıma yüzü out-of-the-box destekler, ama aynı zamanda bu veritabanında olmayan diğer yüzleri tanımak için eğitebilirsiniz. 

Ayrıntılar için [Kişi,](customize-person-model-overview.md) [Markalar](customize-brands-model-overview.md)ve [Dil](customize-language-model-overview.md) modellerini özelleştirme yle ilgili makalelere bakın. 

###  <a name="can-i-edit-the-videos-in-my-library"></a>Kitaplığımdaki videoları abilir miyim?

Evet. **Projeler** sekmesine ulaşmak için kitaplık ekranından **videoyu veya** oyuncu ekranından **düzenleyicidüğmesine** basın. Yeni bir proje oluşturabilir ve kitaplığınızdan bunları birlikte düzenlemesi için daha fazla video ekleyebilirsiniz, işiniz bittikten sonra videonuzu oluşturabilir ve indirebilirsiniz. 

Yeni videonuz hakkında bilgi almak istiyorsanız, Video Indexer ile dizine dizin ve istatistikleriyle kitaplığınızda görünecektir.

### <a name="what-is-the-sla-for-video-indexer"></a>Video Dizinleyici için SLA nedir?

Azure Media Service'in SLA'sı Video Dizinleyici'yi kapsar ve [SLA](https://azure.microsoft.com/support/legal/sla/media-services/v1_2/) sayfasında bulunabilir. SLA yalnızca Video Indexer ücretli hesaplar için geçerlidir ve ücretsiz deneme için geçerli değildir.

## <a name="privacy-questions"></a>Gizlilik Soruları

### <a name="are-video-and-audio-files-indexed-by-video-indexer-stored"></a>Video ve ses dosyaları Video Indexer tarafından dizine eklenmiş mi?

Evet, Video Indexer web sitesini veya API'yi kullanarak dosyayı Video Indexer'dan silmediğiniz sürece, video ve ses dosyalarınız depolanır. Ücretsiz deneme sürümü için, dizine dizine bindirdiğiniz video ve ses dosyaları Doğu ABD'deki Azure bölgesinde depolanır. Aksi takdirde, video ve ses dosyalarınız Azure aboneliğinizin depolama hesabında depolanır.

### <a name="can-i-delete-my-files-that-are-stored-in-video-indexer-portal"></a>Video Indexer Portalı'nda depolanan dosyalarımı silebilir miyim?

Evet, video ve ses dosyalarınızı ve Video Indexer tarafından çıkarılan meta verileri ve öngörüleri her zaman silebilirsiniz. Bir dosyayı Video Indexer'dan sildiğinizde, dosya ve meta verileri ve öngörüleri Video Dizinleyici'den kalıcı olarak kaldırılır. Ancak, Azure depolama alanında kendi yedekleme çözümünüzü uyguladıysanız, dosya Azure depolama alanınızda kalır.

### <a name="can-i-control-user-access-to-my-video-indexer-account"></a>Video Dizinleyici hesabıma kullanıcı erişimini denetleyebilir miyim?

Evet, yalnızca hesap yöneticileri insanları hesaplarına davet edebilir ve davet etmeyi yanı sıra, düzenleme ayrıcalıklarına sahip olan ları ve salt okunur erişimi olan kişileri atayabilir.

### <a name="who-has-access-to-my-video-and-audio-files-that-have-been-indexed-andor-stored-by-video-indexer-and-the-metadata-and-insights-that-were-extracted"></a>Video Indexer tarafından dizine eklenen ve/veya depolanan video ve ses dosyalarıma ve çıkarılan meta verilere ve öngörülere kimler erişebilir?

Gizlilik ayarı olarak herkese açık olan video veya ses içeriğinize, video veya ses içeriğinize ve görüşlerinize bağlantı veren herkes erişebilir. Gizlilik ayarı olarak özel olan video veya ses içeriğinize yalnızca video veya ses içeriğinin hesabına davet edilen kullanıcılar erişebilir. İçeriğinizin gizlilik ayarı, Video Dizinleyici'nin ayıkettiği meta veriler ve öngörüler için de geçerlidir. Video nuzu veya ses dosyanızı yüklerken gizlilik ayarını atarsınız. Dizin oluşturmadan sonra gizlilik ayarını da değiştirebilirsiniz.

### <a name="what-access-does-microsoft-have-to-my-video-or-audio-files-that-have-been-indexed-andor-stored-by-video-indexer-and-the-metadata-and-insights-that-were-extracted"></a>Microsoft'un Video Indexer tarafından dizine eklenen ve/veya depolanan video veya ses dosyalarıma ve çıkarılan meta verilere ve öngörülere ne gibi bir erişimi vardır?

Azure [Çevrimiçi Hizmet Koşulları](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) (OST) uyarınca, içeriğiniz tamamen size aittir ve Microsoft yalnızca İçeriğinize ve Video Dizincisi'nin Içeriğinizden OST ve Microsoft Gizlilik Bildirimi'ne göre çıkardığı meta verilere ve öngörülere erişecektir.

### <a name="are-the-custom-models-that-i-build-in-my-video-indexer-account-available-to-other-accounts"></a>Video Indexer hesabımda oluşturduğum özel modeller diğer hesaplar için kullanılabilir mi?

 Hayır, hesabınızda oluşturduğunuz özel modeller başka hiçbir hesap tarafından kullanılamaz. Video Indexer şu anda hesabınızda özel [markalar,](customize-brands-model-overview.md) [dil](customize-language-model-overview.md)ve [kişi](customize-person-model-overview.md) modelleri oluşturmanıza olanak sağlar. Bu modeller yalnızca modelleri oluşturduğunuz hesapta kullanılabilir.
  
### <a name="is-the-content-indexed-by-video-indexer-kept-within-the-azure-region-where-i-am-using-video-indexer"></a>Video Dizinleyici tarafından dizine eklenmiş içerik, Video Dizinleyici'yi kullandığım Azure bölgesinde mi tutulur?

Evet, Azure aboneliğinizde birden çok Azure bölgesi kullanan bir el ile yapılandırmanız yoksa içerik ve öngörüleri Azure bölgesinde tutulur. 

### <a name="what-is-the-privacy-policy-for-video-indexer"></a>Video Indexer için gizlilik ilkesi nedir?

Video [Indexer, Microsoft Gizlilik Bildirimi](https://privacy.microsoft.com/privacystatement)kapsamındadır. Gizlilik bildirimi, Microsoft'un işlediği kişisel verileri, Microsoft'un bunları nasıl işlediğini ve Microsoft'un bunları hangi amaçlarla işlediğini açıklar. Gizlilik hakkında daha fazla bilgi edinmek için [Microsoft Güven Merkezi'ni](https://www.microsoft.com/trustcenter)ziyaret edin.

### <a name="what-certifications-does-video-indexer-have"></a>Video Indexer'ın sertifikaları nelerdir?

Video Indexer şu anda SOC sertifikasına sahiptir. Video Indexer'ın sertifikasını incelemek için lütfen [Microsoft Güven Merkezi'ne](https://www.microsoft.com/trustcenter/compliance/complianceofferings?product=Azure)bakın.

### <a name="what-is-the-difference-between-private-and-public-videos"></a>Özel ve genel videolar arasındaki fark nedir? 

Videolar Video Indexer'a yüklendiğinde, özel ve herkese açık olmak üzere iki gizlilik ayarı arasından seçim yapabilirsiniz. Herkese açık videolara anonim ve kimliği belirsiz kullanıcılar da dahil olmak üzere herkes erişebilir. Özel olanlar yalnızca hesap üyeleriyle sınırlıdır. 

### <a name="i-tried-to-upload-a-video-as-public-and-it-was-flagged-for-inappropriate-or-offensive-content-what-does-that-mean"></a>Ben halka açık olarak bir video yüklemek için çalıştı ve uygunsuz veya saldırgan içerik için bayraklı, bu ne anlama geliyor? 

Video Indexer'a video yüklerken, uygunsuz içeriğin herkese açık olarak sunulmadığından emin olmak için algoritmalar ve modeller tarafından otomatik içerik çözümlemesi yapılır. Bir videonun müstehcen içerik içerdiği nden şüpheli olduğu tespit edilirse, videoyu herkese açık olarak ayarlamak mümkün olmayacaktır. Ancak, hesap üyeleri hala özel bir video olarak erişebilir (görüntüleyebilir, istatistikleri ve çıkarılan yapıları indirebilir ve hesap üyelerinin kullanabileceği diğer işlemleri gerçekleştirebilir).   

Videoyu genel erişim için ayarlamak için şunları yapabilirsiniz: 

* Kendi arabirim katmanınızı (uygulama veya web sitesi gibi) oluşturun ve Video Dizinleyici hizmetiyle etkileşim kurmak için kullanın. Bu şekilde video portalımızda gizli kalır ve kullanıcılarınız arayüzünüz aracılığıyla bu video ile etkileşimkurabilir. Örneğin, yine de görüşleri alabilir veya videonun kendi arabiriminizde görüntülenmesine izin verebilirsiniz. 
* İçeriğin açık olmadığını varsayarak kısıtlamanın kaldırılmasıyla sonuçlanabilecek bir içeriğin insan tarafından incelenmesini isteyin. 

    Video Indexer web sitesi doğrudan kullanıcılarınız tarafından arabirim katmanı olarak ve genel (kimlik doğrulaması olmayan) görüntüleme için kullanılıyorsa, bu seçenek araştırılabilir. 

## <a name="api-questions"></a>API Soruları

### <a name="what-apis-does-video-indexer-offer"></a>Video Indexer hangi API'leri sunuyor?

Video Indexer'ın API'leri, meta verileri dizine alma, çıkarma, varlık yönetimi, çeviri, katıştırma, modellerin özelleştirilmesi ve daha fazlasını sağlar. Video Indexer API'sini kullanma hakkında daha ayrıntılı bilgi edinmek için [Video Indexer Geliştirici Portalı'na](https://api-portal.videoindexer.ai/)bakın.

### <a name="what-client-sdks-does-video-indexer-offer"></a>Video Indexer hangi istemci SDK'ları sunuyor?

Şu anda teklif edilen istemci SDK'ları yok. Video Indexer ekibi SDK'lar üzerinde çalışıyor ve bunları yakında teslim etmeyi planlıyor.

### <a name="how-do-i-get-started-with-video-indexers-api"></a>Video Indexer'ın API'si ile nasıl başlarım?

[Öğretici izleyin: Video Indexer API ile başlamak](video-indexer-use-apis.md).

### <a name="what-is-the-difference-between-the-video-indexer-api-and-the-azure-media-service-v3-api"></a>Video Dizinleyici API ile Azure Media Service v3 API arasındaki fark nedir?

Şu anda Video Dizinleyici API ve Azure Media Service v3 API tarafından sunulan özelliklerde bazı çakışmalar vardır. [Burada](compare-video-indexer-with-media-services-presets.md)her iki hizmeti karşılaştırmak için nasıl daha fazla bilgi bulabilirsiniz.

### <a name="what-is-an-api-access-token-and-why-do-i-need-it"></a>API erişim jetonu nedir ve neden ihtiyacım var?

Video Dizinleyici API'si bir Yetkilendirme API'si ve Bir İşlem API'si içerir. Yetkilendirme API'si, size giriş izni veren çağrılar içerir. İşlemler API'sine yapılan her çağrı, çağrının yetkilendirme kapsamına uyan bir erişim belirteci ile ilişkilendirilmelidir.

Video Dizinleyici API'lerini güvenlik amacıyla kullanmak için erişim belirteçleri gereklidir. Bu, sizden veya hesabınıza erişim izni olan kişilerden arama ların gelmesini sağlar. 

### <a name="what-is-the-difference-between-account-access-token-user-access-token-and-video-access-token"></a>Hesap erişim belirteci, Kullanıcı erişim jetonu ve Video erişim jetonu arasındaki fark nedir?

* Hesap düzeyi: Hesap düzeyindeki erişim belirteçleri, hesap veya video düzeyinde işlem gerçekleştirmenize olanak sağlar. Örneğin, bir video yükleyin, tüm videoları listeleyebilir, video istatistikleri alın.
* Kullanıcı düzeyi - kullanıcı düzeyi erişim belirteçleri, işlemleri kullanıcı düzeyinde gerçekleştirmenize izin vermez. Örnek: ilişkili hesaplar alma.
* Video düzeyi: Video düzeyindeki erişim belirteçleri, belirli bir video üzerinde işlem gerçekleştirmenize olanak sağlar. Örnek: video içgörüsü alma, açıklamalı alt yazı indirme, pencere öğesi alma vb.

### <a name="how-often-do-i-need-to-get-a-new-access-token-when-do-access-tokens-expire"></a>Yeni bir erişim jetonu almam ne sıklıkta gerekir? Erişim belirteçlerinin süresi ne zaman doluyor?

Erişim belirteçlerinin süresi her saat sona erer, bu nedenle her saat yeni bir erişim belirteci oluşturmanız gerekir. 

### <a name="what-are-the-login-options-to-video-indexer-developer-portal"></a>Video Indexer Developer portalına giriş seçenekleri nelerdir?

Azure AD, Microsoft hesabı, Google hesabı veya Facebook hesabını kullanarak giriş yapabilirsiniz. 

Bir kimlik sağlayıcısı kullanarak e-posta hesabınızı kaydettikten sonra, bu e-posta hesabını başka bir kimlik sağlayıcısında kullanamazsınız.

## <a name="billing-questions"></a>Faturalama soruları

### <a name="how-much-does-video-indexer-cost"></a>Video Dizinleyici'nin maliyeti nedir?

Video Indexer, dizine dizine koyduğunuz içerik girişinin süresine bağlı olarak basit bir gider olarak ödeme fiyatlandırma modeli kullanır. Kodlama, akış, depolama, ağ kullanımı ve ortam ayrılmış birimler için ek ücretler uygulanabilir. Daha fazla bilgi için [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/) sayfasına bakın.

### <a name="when-am-i-billed-for-using-video-indexer"></a>Video Dizinleyici'yi kullanmak için ne zaman faturalandırılırım?

Dizinlenecek bir video gönderirken, kullanıcı dizin oluşturmayı video analizi, ses analizi veya her ikisi olarak tanımlar. Bu, hangi SUS'ların ücretlendirileceğini belirleyecektir. İşlem sırasında kritik bir düzey hatası varsa, yanıt olarak bir hata kodu döndürülür. Böyle bir durumda faturalandırma yapılmaz.  Kritik bir hata, kodumuzdaki bir hatadan veya hizmetin sahip olduğu bir iç bağımlılıktaki kritik bir hatadan kaynaklanabilir. Yanlış tanımlama veya içgörü çıkarma gibi hatalar kritik olarak kabul edilmez ve yanıt döndürülür. Geçerli (hatasız kod) yanıtın döndürüldüğü durumlarda faturalandırma gerçekleşir.
 
### <a name="does-video-indexer-offer-a-free-trial"></a>Video Indexer ücretsiz deneme sunuyor mu?

Evet, Video Indexer tam hizmet ve API işlevselliği veren ücretsiz bir deneme sunuyor. Web tabanlı arayüz kullanıcıları için 600 dakikalık video ve API kullanıcıları için 2.400 dakika lık bir kota vardır. 

## <a name="next-steps"></a>Sonraki adımlar

[Genel bakış](video-indexer-overview.md)
