---
title: Azure Medya Hizmetleri kavramları | Microsoft Dokümanlar
description: Bu makalede, Microsoft Azure Medya Hizmetleri kavramları ve ayrıntılar için diğer makalelere bağlantılar hakkında kısa bir genel bakış yer almaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: 260ddccc1a1b0bd4090284025b79e20ff5ce4fdc
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80475252"
---
# <a name="azure-media-services-concepts"></a>Azure Medya Hizmetleri kavramları 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Bu konu, en önemli Medya Hizmetleri kavramlarına genel bir bakış sağlar.

## <a name="assets-and-storage"></a><a id="assets"/>Varlıklar ve Depolama
### <a name="assets"></a>Varlıklar
[Bir Varlık](https://docs.microsoft.com/rest/api/media/operations/asset) dijital dosyalar (video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı altyazı dosyaları dahil) ve bu dosyalarla ilgili meta verileri içerir. Dijital dosyalar bir varlığa yüklendikten sonra, Medya Hizmetleri kodlama ve akış iş akışlarında kullanılabilir.

Bir varlık Azure Depolama hesabındaki bir blob kapsayıcısına eşlenir ve kıymetteki dosyalar bu kapsayıcıda blok blob'ları olarak depolanır. Sayfa lekeleri Azure Medya Hizmetleri tarafından desteklenmez.

Bir varlıkta hangi medya içeriğinin yükleyip depolanmasına karar verirken aşağıdaki hususlar geçerlidir:

* Bir varlık yalnızca tek bir, benzersiz ortam içeriği örneği içermelidir. Örneğin, bir TV bölümünün, filminin veya reklamın tek bir editi.
* Bir varlık, görsel-işitsel bir dosyanın birden çok yorumlama veya düzeltmesini içermemelidir. Bir Varlığın uygunsuz kullanımına örnek olarak, tek bir üretimden birden fazla TV bölümü, reklam veya birden çok kamera açısını bir varlığın içinde depolamaya çalışmak gerekir. Bir kıymette birden çok yorumlama veya görsel-işitsel dosya nın yeniden depolanması, kodlama işlerinin gönderilmesinde, akışta ve varlığın daha sonra iş akışına teslimini güvence altına almakta güçlük lere neden olabilir.  

### <a name="asset-file"></a>Varlık dosyası
[AssetFile,](https://docs.microsoft.com/rest/api/media/operations/assetfile) blob kapsayıcısında depolanan gerçek bir video veya ses dosyalarını temsil eder. Kıymet dosyası her zaman bir varlıkla ilişkilidir ve bir varlık bir veya birden çok dosya içerebilir. Bir varlık dosyası nesnesi bir blob kapsayıcısındaki dijital bir dosyayla ilişkilendirilmezse, Medya Hizmetleri Encoder görevi başarısız olur.

**AssetFile** örneği ve gerçek ortam dosyası iki farklı nesnedir. AssetFile örneği medya dosyası yla ilgili meta verileri içerirken, ortam dosyası gerçek medya içeriğini içerir.

Medya Hizmeti API'lerini kullanmadan Media Services tarafından oluşturulan blob kapsayıcılarının içeriğini değiştirmeye çalışmamalısınız.

### <a name="asset-encryption-options"></a>Varlık şifreleme seçenekleri
Medya Hizmetleri, yüklemek, depolamak ve sunmak istediğiniz içeriğin türüne bağlı olarak aralarından seçim yapabileceğiniz çeşitli şifreleme seçenekleri sunar.

>[!NOTE]
>Şifreleme kullanılmaz. Varsayılan değer budur. Bu seçeneği kullanırken içeriğiniz taşıma sırasında veya depolama alanında korumalı değildir.

Aşamalı indirmeyi kullanarak bir MP4 sunmayı planlıyorsanız, içeriğinizi yüklemek için bu seçeneği kullanın.

**StorageEncrypted** – Temiz içeriğinizi AES 256 bit şifreleme kullanarak yerel olarak şifrelemek ve ardından istirahatte şifrelenmiş olarak depolandığı Azure Depolama'ya yüklemek için bu seçeneği kullanın. Depolama şifrelemesi ile korunan varlıklar otomatik olarak şifrelenmez ve kodlamadan önce şifrelenmiş bir dosya sistemine yerleştirilir ve isteğe bağlı olarak yeni bir çıktı varlığı olarak geri yüklemeden önce yeniden şifrelenir. Depolama şifrelemesi için birincil kullanım örneği, yüksek kaliteli giriş ortam dosyalarınızı diskte güçlü şifreleme yle güvence altına almak istediğinizde olur. 

Depolama şifreli bir varlık sunmak için, Medya Hizmetleri'nin içeriğinizi nasıl teslim etmek istediğinizi bilmesi için varlığın teslim politikasını yapılandırmanız gerekir. Varlığınız akışa geçmeden önce, akış sunucusu depolama şifrelemesini kaldırır ve belirtilen teslim ilkesini kullanarak içeriğinizi akışı sağlar (örneğin, AES, PlayReady veya şifreleme yok). 

**CommonEncryptionProtected** - Ortak Şifreleme veya PlayReady DRM (örneğin, PlayReady DRM ile korunan Sorunsuz Akış) ile içeriği şifrelemek (veya zaten şifrelenmiş olan) içeriği yüklemek istiyorsanız bu seçeneği kullanın.

**EnvelopeEncryptionProtected** – Gelişmiş Şifreleme Standardı (AES) ile şifrelenmiş HTTP Live Streaming (HLS) koruma (veya zaten korumalı yükleme) istiyorsanız bu seçeneği kullanın. HLS'yi Zaten AES ile şifrelenmiş olarak yüklüyorsanız, Transform Manager tarafından şifrelenmiş olmalıdır.

### <a name="access-policy"></a>Erişim ilkesi
[AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy) izinleri (okuma, yazma ve liste gibi) ve bir varlığa erişim süresini tanımlar. AccessPolicy nesnesini genellikle bir varlıkta bulunan dosyalara erişmek için kullanılacak bir bulucuya geçirirsiniz.

>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). Daha fazla bilgi için [bu](media-services-dotnet-manage-entities.md#limit-access-policies) konu başlığına bakın.

### <a name="blob-container"></a>Blob kapsayıcı
Blob kapsayıcısı, bir dizi blob'un gruplandırmasını sağlar. Blob kapsayıcıları Medya Hizmetleri'nde erişim denetimi için sınır noktası olarak ve varlıklardaki Paylaşılan Erişim İmzası (SAS) bulucuları olarak kullanılır. Azure Depolama hesabı sınırsız sayıda blob kapsayıcısı içerebilir. Kapsayıcıda sınırsız sayıda blob depolanabilir.

>[!NOTE]
> Medya Hizmeti API'lerini kullanmadan Media Services tarafından oluşturulan blob kapsayıcılarının içeriğini değiştirmeye çalışmamalısınız.
> 
> 

### <a name="locators"></a><a id="locators"/>Locators
[Konum bulucu,](https://docs.microsoft.com/rest/api/media/operations/locator)bir kıymette bulunan dosyalara erişmek için bir giriş noktası sağlar. Erişim ilkesi, istemcinin belirli bir varlığa erişebilmek için izinleri ve süresini tanımlamak için kullanılır. Yer bulucuların bir erişim ilkesiyle birbiriyle çok sayıda ilişkisi olabilir, farklı yer bulucular farklı istemcilere farklı başlangıç saatleri ve bağlantı türleri sağlarken hepsi aynı izin ve süre ayarlarını kullanabilir; ancak, Azure depolama hizmetleri tarafından belirlenen paylaşılan erişim ilkesi kısıtlaması nedeniyle, aynı anda belirli bir varlıkla ilişkili beşten fazla benzersiz yer bulasanız olamaz. 

Medya Hizmetleri iki tür yer bulucuyu destekler: Medya akışı (örneğin, MPEG DASH, HLS veya Düzgün Akış) veya medya dosyalarını yüklemek veya Azure depolamadan\a indirmek için kullanılan medya ve SAS URL bulucularını aşamalı olarak indirmek için kullanılan OnDemandOrigin bulucuları. 

>[!NOTE]
>Liste izni (AccessPermissions.List) OnDemandOrigin bulucu oluştururken kullanılmamalıdır. 

### <a name="storage-account"></a>Depolama hesabı
Azure Depolama'ya tüm erişim bir depolama hesabı üzerinden yapılır. Medya Hizmeti hesabı, bir veya daha fazla depolama hesabıyla ilişkilendirilebilir. Bir hesap, toplam boyutu depolama hesabı başına 500 TB'nin altında olduğu sürece sınırsız sayıda kapsayıcı içerebilir.  Medya Hizmetleri, birden çok depolama hesabını yönetmenize ve ölçümlere veya rasgele dağıtıma dayalı olarak bu hesaplara yükleme sırasında varlıklarınızın dağıtımını yüklemenize olanak tanıyan SDK düzeyinde araçlandırma sağlar. Daha fazla bilgi için bkz: [Azure Depolama](https://msdn.microsoft.com/library/azure/dn767951.aspx)ile Çalışma . 

## <a name="jobs-and-tasks"></a>İş ve görevler
Bir [iş](https://docs.microsoft.com/rest/api/media/operations/job) genellikle bir ses/video sunusunu işlemek (örneğin, dizin veya kodlama) için kullanılır. Birden çok videoyu işliyorsanız, her videonun kodlanacak bir iş oluşturun.

Bir iş, gerçekleştirilecek işleme ilişkin meta veriler içerir. Her iş, atomik işleme görevi, giriş Varlıkları, çıkış Varlıkları, bir ortam işlemcisi ve ilişkili ayarlarını belirten bir veya daha fazla [görev](https://docs.microsoft.com/rest/api/media/operations/task)içerir. Bir görevin çıktı varlığı bir sonraki göreve giriş varlığı olarak verildiği bir iş içindeki görevler birlikte zincirlenebilir. Bu şekilde bir iş, bir ortam sunumu için gerekli tüm işlemleri içerebilir.

## <a name="encoding"></a><a id="encoding"></a>Encoding
Azure Medya Hizmetleri, bulutta ortam kodlaması için birden çok seçenek sunar.

Medya Hizmetleri ile başlarken, codec ve dosya biçimleri arasındaki farkı anlamak önemlidir.
Codec'ler sıkıştırma/dekompresyon algoritmalarını uygulayan yazılımdır, oysa dosya biçimleri sıkıştırılmış videoyu tutan kapsayıcılardır.

Medya Hizmetleri, uyarlanabilir bitrate MP4 veya Smooth Streaming kodlanmış içeriğinizi, bu akış biçimlerine yeniden paketlemek zorunda kalmadan Medya Hizmetleri (MPEG DASH, HLS, Smooth Streaming) tarafından desteklenen akış biçimlerinde sunmanızı sağlayan dinamik ambalaj sağlar.

[Dinamik ambalajdan](media-services-dynamic-packaging-overview.md)yararlanmak için, asma (kaynak) dosyanızı uyarlanabilir bitrate MP4 dosyaları veya uyarlanabilir bitrate Smooth Streaming dosyaları kümesine kodlamanız ve başlangıç durumunda en az bir standart veya premium akış bitiş noktasına sahip olmanız gerekir.

Medya Hizmetleri, bu makalede açıklanan aşağıdaki isteğe bağlı kodlayıcıları destekler:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Media Encoder Premium İş Akışı](media-services-encode-asset.md#media-encoder-premium-workflow)

Desteklenen kodlayıcılar hakkında bilgi için [Bkz. Kodlayıcılar.](media-services-encode-asset.md)

## <a name="live-streaming"></a>Canlı Akış
Azure Medya Hizmetleri'nde Kanal, canlı akış içeriğini işlemek için bir ardışık hattı temsil eder. Kanal canlı giriş akışlarını iki şekilde alır:

* Şirket içinde canlı kodlayıcı, Kanala çok bit hızında RTMP veya Düzgün Akış (Parçalanmış MP4) gönderir. Multi-bitrate Smooth Streaming: MediaExcel, Ateme, Imagine Communications, Envivio, Cisco ve Elemental çıktıaşağıdaki canlı kodlayıcılar kullanabilirsiniz. Aşağıdaki canlı kodlayıcılar çıkış RTMP: Adobe Flash Live Encoder, [Telestream Wirecast,](media-services-configure-wirecast-live-encoder.md)Teradek, Haivision ve Tricaster kodlayıcılar. Yutulan akışlar daha fazla kodlama ve kodlama olmadan Kanallar'dan geçer. İstendiğinde, Media Services akışı müşterilere teslim eder.
* Tek bir bit hızı akışı (aşağıdaki biçimlerden birinde: RTMP veya Düzgün Akış (Parçalanmış MP4)) Medya Hizmetleri ile canlı kodlama gerçekleştirmek için etkinleştirilen Kanala gönderilir. Ardından Kanal, gelen tek bit hızlı akışın çoklu bit hızlı (uyarlamalı) bir video akışına gerçek zamanlı kodlanmasını gerçekleştirir. İstendiğinde, Media Services akışı müşterilere teslim eder.

### <a name="channel"></a>Kanal
Medya Hizmetlerinde, [Kanal](https://docs.microsoft.com/rest/api/media/operations/channel)s canlı akış içeriğinin işlenmesiyle sorumludur. Kanal, daha sonra canlı bir transcoder'a sağladığınız bir giriş bitiş noktası (url yutma) sağlar. Kanal, canlı kodlayıcıdan canlı giriş akışları alır ve bir veya daha fazla StreamingEndpoints üzerinden akış için kullanılabilir hale getirir. Kanallar ayrıca, daha fazla işleme ve teslim edilmeden önce akışınızı önizlemek ve doğrulamak için kullandığınız bir önizleme bitiş noktası (önizleme URL'si) de sağlar.

Kanalı oluştururken en yüksek URL'yi ve önizleme URL'sini alabilirsiniz. Bu URL'leri almak için kanalın başlangıç durumunda olması gerekmez. Canlı bir kodlayıcıdan kanala veri itmeye başlamaya hazır olduğunuzda, kanal başlatılmalıdır. Canlı kodlayıcı veri sindirmeye başladıktan sonra akışınızı önizleyebilirsiniz.

Her Medya Hizmetleri hesabı birden çok Kanal, birden çok Program ve birden çok Akış Son Noktası içerebilir. Bant genişliği ve güvenlik gereksinimlerine bağlı olarak, StreamingEndpoint hizmetleri bir veya daha fazla kanala tahsis edilebilir. Herhangi bir StreamingEndpoint herhangi bir Kanal dan çekebilirsiniz.

### <a name="program-event"></a>Program (etkinlik)
[Program (olay),](https://docs.microsoft.com/rest/api/media/operations/program) segmentlerin canlı akışta yayımlanmasını ve depolanmasını denetlemenizi sağlar. Kanallar Programları (olayları) yönetir. Kanal ve Program ilişkisi, bir kanalın sürekli bir içerik akışına sahip olduğu ve bir programın o kanaldaki zamanlanmış bir olaya kapsamının bulunduğu geleneksel ortama benzer.
**ArchiveWindowLength** özelliğini ayarlayarak program için kaydedilen içeriği saklamak istediğiniz saat sayısını belirtebilirsiniz. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir.

ArchiveWindowLength, istemcilerin geçerli canlı konumdan zamanında isteyebileceği maksimum süreyi de belirler. Olaylar belirtilen süre miktarından uzun sürebilir, ancak pencere uzunluğunun gerisine düşen içerik sürekli olarak atılır. Bu özelliğin bu değeri, istemci bildiriminin ne kadar uzayabileceğini de belirler.

Her program (olay) bir Varlık ile ilişkilidir. Programı yayımlamak için ilişkili varlık için bir bulucu oluşturmanız gerekir. Bu bulucuya sahip olmak, istemcilerinize sağlayabileceğiniz bir akış URL’si oluşturmanıza olanak tanır.

Bir kanal eşzamanlı çalışan üçe kadar olayı destekler, böylece aynı gelen akışın birden fazla arşivini oluşturabilirsiniz. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Örneğin, iş gereksiniminiz bir programın 6 saatini arşivlemek ancak son 10 dakikasını yayınlamak olabilir. Bunu yapmak için, eşzamanlı olarak çalışan iki program oluşturmanız gerekir. Bir program olayı 6 saat arşivlemek için ayarlanır ancak program yayımlanmaz. Diğer program 10 dakika arşivlenecek şekilde ve bu program yayımlanır.

Daha fazla bilgi için bkz.

* [Azure Medya Hizmetleriyle Canlı Kodlama Gerçekleştirme özelliğine sahip Kanallarla Çalışma](media-services-manage-live-encoder-enabled-channels.md)
* [Şirket İçi Kodlayıcılardan Çoklu Bit Hızlı Canlı Akış Alan Kanallar ile Çalışma](media-services-live-streaming-with-onprem-encoders.md)
* [Kotalar ve sınırlamalar](media-services-quotas-and-limitations.md).

## <a name="protecting-content"></a>İçerik koruma
### <a name="dynamic-encryption"></a>Dinamik şifreleme
Azure Medya Hizmetleri, depolama, işleme ve teslim yoluyla bilgisayarınızı terk eden andan itibaren medyanızı güvenli hale almanızı sağlar. Medya Hizmetleri, PlayReady ve/veya Widevine DRM kullanarak Gelişmiş Şifreleme Standardı (AES) (128 bit şifreleme tuşları kullanarak) ve ortak şifreleme (CENC) ile dinamik olarak şifrelenmiş içeriğinizi sunmanızı sağlar. Medya Hizmetleri ayrıca yetkili müşterilere AES anahtarları ve PlayReady lisansları sunmak için bir hizmet de sağlar.

Şu anda aşağıdaki akış biçimlerini şifreleyebilirsiniz: HLS, MPEG DASH ve Düzgün Akış. Aşamalı indirmeleri şifreleyemezsiniz.

Medya Hizmetleri'nin bir varlığı şifrelemesini istiyorsanız, bir şifreleme anahtarını (CommonEncryption veya EnvelopeEncryption) kıymetinizle ilişkilendirmeniz ve ayrıca anahtar için yetkilendirme ilkelerini yapılandırmanız gerekir.

Depolama şifreli bir varlığı akışla aktarmak istiyorsanız, varlığınızı nasıl teslim etmek istediğinizi belirtmek için varlığın teslim politikasını yapılandırmanız gerekir.

Bir akış bir oynatıcı tarafından istendiğinde, Medya Hizmetleri belirtilen anahtarı kullanarak içeriğinizi zarf şifreleme (AES ile) veya ortak şifreleme (PlayReady veya Widevine ile) kullanarak dinamik olarak şifreler. Akışın şifresini çözmek için, oynatıcı anahtarı anahtar teslim hizmetinden talep edecektir. Kullanıcının anahtarı almaya yetkili olup olmadığına karar vermek için, hizmet anahtar için belirlediğiniz yetkilendirme ilkelerini değerlendirir.

### <a name="token-restriction"></a>Belirteç kısıtlaması
İçerik anahtarı yetkilendirme ilkesinde bir veya daha fazla yetkilendirme kısıtlaması olabilir: açık, belirteç kısıtlaması veya IP kısıtlaması. Belirteç kısıtlamalı ilkenin beraberinde bir Güvenli Belirteç Hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services, Basit Web Belirteçleri (SWT) ve JSON Web Token (JWT) biçimlerindeki belirteçleri destekler. Ortam Hizmetleri Güvenli Belirteç Hizmetleri sağlamaz. Özel bir STS oluşturabilirsiniz. STS belirtilmiş anahtar ve belirteç kısıtlama yapılandırmasında belirttiğiniz sorun talepleri ile imzalanmış bir belirteç oluşturmak için yapılandırılmalıdır. Ortam Hizmetleri anahtar teslim hizmeti, belirteç geçerliyse ve belirteçteki talepler anahtar (veya lisans) için yapılandırılanlarla eşleşirse, istenen anahtarı (veya lisansı) istemciye iade eder.

Belirteç kısıtlı ilkesini yapılandırırken birincil doğrulama anahtarını, vereni ve hedef kitle parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı belirteç ile imzalanmış anahtarı içerir, veren belirteç sorunları güvenli belirteç hizmetidir. Hedef kitle (bazen kapsam olarak adlandırılır), belirteci veya belirteç erişim eizin kaynağının amacını açıklar. Medya Hizmetleri anahtar teslim hizmeti, belirteçteki bu değerlerin şablondaki değerlerle eşleştirdiğini doğrular.

Daha fazla bilgi için aşağıdaki makalelere bakın:
- [İçerime genel bakışı koruma](media-services-content-protection-overview.md)
- [AES-128 ile koruyun](media-services-protect-with-aes128.md)
- [PlayReady/Widevine ile koruyun](media-services-protect-with-playready-widevine.md)

## <a name="delivering"></a>Teslim
### <a name="dynamic-packaging"></a><a id="dynamic_packaging"/>Dinamik paketleme
Medya Hizmetleri ile çalışırken, asma dosyalarınızı uyarlanabilir bir bitrate MP4 setine kodlamanız ve ardından [Dinamik Ambalaj'ı](media-services-dynamic-packaging-overview.md)kullanarak seti istenilen biçime dönüştürmeniz önerilir.

### <a name="streaming-endpoint"></a>Akış uç noktası
StreamingEndpoint, içeriği doğrudan istemci oynatıcı uygulamasına veya daha fazla dağıtım için Bir İçerik Dağıtım Ağı'na (CDN) sunabilen bir akış hizmetini temsil eder (Azure Media Services artık Azure CDN tümleştirmesini sağlar.) Akış uç noktası hizmetinden giden akış, Medya Hizmetleri hesabınızda canlı akış veya isteğe bağlı bir Varlık olabilir. Media Services müşterileri ihtiyaçlarına göre **Standart** bir akış uç noktası veya bir veya daha fazla **Premium** akış uç noktası seçer. Standart akış uç noktası, çoğu akış iş yükü için uygundur. 

Standart Akış Uç Noktası çoğu akış iş yükü için uygundur. Standart Akış Uç Noktaları, IÇERIĞInizi HLS, MPEG-DASH ve Sorunsuz Akış'a dinamik paketleme yoluyla hemen hemen her cihaza sunma esnekliğinin yanı sıra Microsoft PlayReady, Google Widevine, Apple Fairplay ve AES128 için dinamik şifreleme sunar.  Ayrıca Azure CDN tümleştirmesi aracılığıyla binlerce eşzamanlı görüntüleyenle çok küçükten çok büyük kitlelere ölçeklenirler. Gelişmiş bir iş yükünüz varsa veya akış kapasitesi gereksinimleriniz standart akış uç noktası üretim hedeflerine uymuyorsa veya büyüyen bant genişliği gereksinimlerini karşılamak için StreamingEndpoint hizmetinin kapasitesini denetlemek istiyorsanız, ölçek birimleri (premium akış birimleri olarak da bilinir) ayırmanız önerilir.

Dinamik ambalaj ve/veya dinamik şifreleme kullanılması önerilir.

>[!NOTE]
>AMS hesabınız **oluşturulduğunda,** **Durduruldu** durumunda hesabınıza varsayılan akış bitiş noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 

Daha fazla bilgi için [bu](media-services-portal-manage-streaming-endpoints.md) konu başlığına bakın.

Varsayılan olarak, Medya Hizmetleri hesabınızda en fazla 2 akış uç noktası olabilir. Daha yüksek bir sınır istemek için [Kotalar ve sınırlamalar](media-services-quotas-and-limitations.md)bölümüne bakın.

Yalnızca AkışSon Noktanız çalışırken faturalandırılırsınız.

### <a name="asset-delivery-policy"></a>Varlık teslim ilkesi
Medya Hizmetleri içerik teslim iş akışındaki adımlardan biri, akıştan çıkarmak istediğiniz [varlıklar için teslim ilkelerini](https://docs.microsoft.com/rest/api/media/operations/assetdeliverypolicy)yapılandırmaktır. Varlık teslim ihdası, Medya Hizmetleri'ne varlığınızın nasıl teslim edilmesini istediğinizi söyler: varlığınızın dinamik olarak paketlenmesi gereken akış protokolü (örneğin, MPEG DASH, HLS, Düzgün Akış veya tümü), varlığınızı dinamik olarak şifrelemek isteyip istemediğiniz ve nasıl (zarf veya ortak şifreleme) istediğinizi.

Depolama şifreli bir varlığınız varsa, kıymetiniz akışa geçmeden önce, akış sunucusu depolama şifrelemesini kaldırır ve belirtilen teslim ilkesini kullanarak içeriğinizi akışı sağlar. Örneğin, varlıkdurumunuzu Gelişmiş Şifreleme Standardı (AES) şifreleme anahtarıyla şifrelenmiş olarak teslim etmek için ilke türünü DynamicEnvelopeEncryption olarak ayarlayın. Depolama şifrelemesini kaldırmak ve varlığı açık olarak aktarmak için ilke türünü NoDinamik Şifreleme olarak ayarlayın.

### <a name="progressive-download"></a>Aşamalı indirme
Aşamalı indirme, tüm dosya indirilmeden önce medya oynatmaya başlamanızı sağlar. Yalnızca aşamalı olarak bir MP4 dosyasını indirebilirsiniz.

>[!NOTE]
>Aşamalı olarak indirilebilmeleri için şifrelenmiş varlıkların şifresini çözmeniz gerekir.

Kullanıcılara aşamalı indirme URL'leri sağlamak için öncelikle bir OnDemandOrigin bulucu oluşturmanız gerekir. Yer bulucuyu oluşturmak, size varlığa temel Yolu verir. Daha sonra MP4 dosyasının adını ekleniz gerekir. Örnek:

http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

### <a name="streaming-urls"></a>Akış URL'leri
İçeriğinizi istemcilere aktarın. Kullanıcılara akış URL'leri sağlamak için öncelikle bir OnDemandOrigin bulucu oluşturmanız gerekir. Bulucuoluşturma, akışı istediğiniz içeriği içeren varlığa temel Yol verir. Ancak, bu içeriği akışa göre aktarabilmek için bu yolu daha fazla değiştirmeniz gerekir. Akış bildirimi dosyasına tam bir URL oluşturmak için, bulucunun Yol değerini ve manifestoyu (filename.ism) dosya adını oluşturmanız gerekir. Ardından, (Bildirim) ve yer bulucu yoluna uygun bir biçim (gerekirse) ekle.

İçeriğinizi TLS bağlantısı üzerinden de aktarabilirsiniz. Bunu yapmak için, akış URL'lerinizin HTTPS ile başladığından emin olun. Şu anda, AMS özel etki alanları ile TLS desteklemiyor.  

>[!NOTE]
>TLS üzerinden yalnızca içeriğinizi teslim ettiğiniz akış bitiş noktası 10 Eylül 2014'ten sonra oluşturulduysa akış yapabilirsiniz. Akış URL'leriniz 10 Eylül'den sonra oluşturulan akış uç noktalarını temel alıyorsa, URL "streaming.mediaservices.windows.net" (yeni biçim) içerir. "origin.mediaservices.windows.net" (eski biçim) içeren akış URL'leri TLS'yi desteklemez. URL'niz eski biçimdeyse ve TLS üzerinden akış yapabilmek istiyorsanız, yeni bir akış bitiş noktası oluşturun. İçeriğinizi TLS üzerinden aktarmak için yeni akış bitiş noktasına dayalı olarak oluşturulan URL'leri kullanın.

Aşağıdaki liste farklı akış biçimlerini açıklar ve örnekler verir:

* Kesintisiz Akış

{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest

* MPEG DASH

{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest(format=mpd-time-csf)

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf)

* Apple HTTP Canlı Akış (HLS) V4

{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest(format=m3u8-aapl)

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl)

* Apple HTTP Canlı Akış (HLS) V3

{streaming endpoint name-media services hesap adı}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3)

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

