---
title: Medya Hizmetleri ile video ve ses kodlama
titleSuffix: Azure Media Services
description: Bu makalede, Azure Medya Hizmetleri ile video ve ses kodlama hakkında açıklanmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 09/10/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 6e1c9aa5c2e049d5fc1ebd8bf745417f56d232ec
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366569"
---
# <a name="encoding-video-and-audio-with-media-services"></a>Medya Hizmetleri ile video ve ses kodlama

Medya Hizmetleri'ndeki kodlama terimi, (a) dosyaların boyutunu küçültmek ve/veya (b) çok çeşitli aygıt lar ve uygulamalarla uyumlu bir biçim üretmek amacıyla, dijital video ve/veya ses içeren dosyaları bir standart biçimden diğerine dönüştürme işlemi için geçerlidir. Bu işlem, video sıkıştırma veya kodlama olarak da adlandırılır. Kavramların daha fazla tartışılması için [Veri sıkıştırma](https://en.wikipedia.org/wiki/Data_compression) ve Kodlama ve [Kodlama Nedir?](https://www.streamingmedia.com/Articles/Editorial/What-Is-/What-Is-Encoding-and-Transcoding-75025.aspx)

Videolar genellikle cihazlara ve uygulamalara [aşamalı olarak indirilerek](https://en.wikipedia.org/wiki/Progressive_download) veya [uyarlanabilir bit hızı akışı](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)yoluyla teslim edilir.

> [!IMPORTANT]
> Medya Hizmetleri iptal edilen veya hatalı olan işler için faturalandırmaz. Örneğin, %50 ilerleme kaydetmiş ve iptal edilmiş bir iş, iş dakikalarının %50'si olarak faturalandırılmez. Sadece bitmiş işler için ücretlendirilirsiniz.

* Aşamalı olarak karşıdan yükleme yapmak için, dijital medya dosyasını (asma kat) [H.264](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC) codec ile kodlanmış video ve [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) codec ile kodlanmış ses içeren bir [MP4](https://en.wikipedia.org/wiki/MPEG-4_Part_14) dosyasına dönüştürmek için Azure Medya Hizmetleri'ni kullanabilirsiniz. Bu MP4 dosyası, depolama hesabınızdaki bir Kıymete yazılır. Dosyayı doğrudan indirmek için Azure Depolama API'lerini veya SDK'larını (örneğin, [Storage REST API](../../storage/common/storage-rest-api-auth.md) veya [.NET SDK)](../../storage/blobs/storage-quickstart-blobs-dotnet.md)kullanabilirsiniz. Çıktı Varlık'ı depolamaalanında belirli bir kapsayıcı adı ile oluşturduysanız, bu konumu kullanın. Aksi takdirde, [varlık kapsayıcı URL'lerini listelemek](https://docs.microsoft.com/rest/api/media/assets/listcontainersas)için Medya Hizmetleri'ni kullanabilirsiniz. 
* Adaptif bitrate akışı ile teslimat için içerik hazırlamak için, asma dosya birden fazla bitrate (yüksek ten düşük) kodlanmış olması gerekir. Zarif bir kalite geçişi sağlamak için, bit hızı düşürüldükçe videonun çözünürlüğü düşürülür. Bu, kodçözme merdiveni olarak adlandırılan çözünürlükler ve bithızlar tablosuyla sonuçlanır (bkz. [otomatik olarak oluşturulmuş uyarlanabilir bitrate merdiven).](autogen-bitrate-ladder.md) Asma dosyalarınızı birden çok bitrates'de kodlamak için Medya Hizmetleri'ni kullanabilirsiniz. Bunu yaparken, depolama hesabınızdaki bir Varlık'a yazılmış bir MP4 dosyaları ve ilişkili akış yapılandırma dosyaları kümesi alırsınız. Daha sonra Medya Hizmetleri [Dinamik Ambalaj](dynamic-packaging-overview.md) yeteneğini [MPEG-DASH](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP) ve [HLS](https://en.wikipedia.org/wiki/HTTP_Live_Streaming)gibi akış protokolleri aracılığıyla video sunmak için kullanabilirsiniz. Bu, desteklenen protokollere karşılık gelen bir [Akış Bulucu](streaming-locators-concept.md) oluşturmanızı ve daha sonra yeteneklerine göre aygıtlara/uygulamalara dağıtılabilen akış URL'leri oluşturmanızı gerektirir.

Aşağıdaki diyagram, dinamik ambalajla isteğe bağlı kodlama için iş akışını gösterir.

![Dinamik ambalajile isteğe bağlı kodlama için iş akışı](./media/dynamic-packaging-overview/media-services-dynamic-packaging.svg)

Bu konu, Medya Hizmetleri v3 ile içeriğinizi kodlamak için nasıl rehberlik sağlar.

## <a name="transforms-and-jobs"></a>Dönüşümler ve işler

Media Services v3 ile kodlamak için bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) ve [İş](https://docs.microsoft.com/rest/api/media/jobs)oluşturmanız gerekir. Dönüştürme, kodlama ayarlarınız ve çıktılarınız için bir reçete tanımlar; iş tarifi bir örneğidir. Daha fazla bilgi için [Dönüşümler ve İşler'e](transforms-jobs-concept.md)bakın.

Medya Hizmetleri ile kodlama yaparken, giriş ortamı dosyalarının nasıl işlenmesi gerektiğini kodlayıcıya anlatmak için hazır ayarları kullanırsınız. Media Services v3'te dosyalarınızı kodlamak için Standart Encoder'ı kullanırsınız. Örneğin, kodlanmış içerikte video çözünürlüğünü ve/veya istediğiniz ses kanalı sayısını belirtebilirsiniz.

Endüstrinin en iyi uygulamalarını temel alan önerilen yerleşik hazır ayarlardan biriyle hızlı bir şekilde başlanabilir veya belirli senaryo veya aygıt gereksinimlerinizi hedeflemek için özel bir ön ayar oluşturmayı seçebilirsiniz. Daha fazla bilgi için özel [bir Dönüştür ile Encode'a](customize-encoder-presets-how-to.md)bakın.

Ocak 2019'dan itibaren MP4 dosyası(lar) oluşturmak için Standart Kodlayıcı ile kodlama yaparken, yeni bir .mpi dosyası oluşturulur ve çıktı Varlığına eklenir. Bu MPI dosyası, dinamik [paketleme](dynamic-packaging-overview.md) ve akış senaryoları için performansı artırmak için tasarlanmıştır.

> [!NOTE]
> MPI dosyasını değiştirmemeli veya kaldırmamalı veya hizmetinizde böyle bir dosyanın varlığına (veya olmadığına) herhangi bir bağımlılık yapmamanız gerekir.

### <a name="creating-job-input-from-an-https-url"></a>HTTPS URL'sinden iş girişi oluşturma

Videolarınızı işlemek için İşler'i gönderdiğinizde, Medya Hizmetleri'ne giriş videosunu nerede bulacağını söylemeniz gerekir. Seçeneklerden biri, iş girişi olarak bir HTTPS URL'si belirtmektir. Şu anda, Media Services v3 HTTPS URL'leri üzerinden ödenebilir aktarım kodlamayı desteklemez.

#### <a name="examples"></a>Örnekler

* [.NET ile https URL'sinden kodlama](stream-files-dotnet-quickstart.md)
* [REST ile HTTPS URL'sinden kodlama](stream-files-tutorial-with-rest.md)
* [CLI ile https URL'sinden kodlama](stream-files-cli-quickstart.md)
* [Node.js ile https URL'sinden kodlama](stream-files-nodejs-quickstart.md)

### <a name="creating-job-input-from-a-local-file"></a>Yerel bir dosyadan iş girişi oluşturma

Giriş videosu Bir Medya Hizmet Varlığı olarak depolanabilir ve bu durumda bir dosyaya dayalı bir giriş varlığı oluşturursunuz (yerel olarak veya Azure Blob depolama alanında depolanır).

#### <a name="examples"></a>Örnekler

[Yerleşik ön ayarları kullanarak yerel bir dosyayı kodlama](job-input-from-local-file-how-to.md)

### <a name="creating-job-input-with-subclipping"></a>Subclipping ile iş girişi oluşturma

Bir videoyu kodlarken, kaynak dosyayı kırpmak veya klibini de belirtebilir ve giriş videosunun yalnızca istenilen bölümüne sahip bir çıktı üretebilirsiniz. Bu işlevsellik, [BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) hazır ayarları veya [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) hazır ayarları kullanılarak oluşturulan herhangi bir [Transform](https://docs.microsoft.com/rest/api/media/transforms) ile çalışır.

İsteğe bağlı bir videonun veya canlı arşivin (kaydedilen bir olay) tek bir klibiyle [Bir İş](https://docs.microsoft.com/rest/api/media/jobs/create) oluşturmak için belirtebilirsiniz. İş girişi bir Varlık veya HTTPS URL'si olabilir.

> [!TIP]
> Videoyu yeniden kodlamadan videonuzun bir alt lip'ini aktarmak istiyorsanız, [Dinamik Paketleyici ile Ön filtreleme bildirimlerini](filters-dynamic-manifest-overview.md)kullanmayı düşünün.

#### <a name="examples"></a>Örnekler

Örneklere bakın:

* [.NET ile bir video alt klip](subclip-video-dotnet-howto.md)
* [REST ile bir video nun alt klibi](subclip-video-rest-howto.md)

## <a name="built-in-presets"></a>Dahili hazır ayarlar

Medya Hizmetleri, aşağıdaki yerleşik kodlama ön ayarlarını destekler:  

### <a name="builtinstandardencoderpreset"></a>BuiltInStandardEncoderPreset

[BuiltInStandardEncoderPreset,](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) giriş videosunu Standart Kodlayıcı ile kodlamak için yerleşik bir ön ayar ayarlamak için kullanılır.

Aşağıdaki ön ayarlar şu anda desteklenir:

- **EncoderNamedPreset.AACGoodQualityAudio**: 192 kbps kodlanmış sadece stereo ses içeren tek bir MP4 dosyası üretir.
- **EncoderNamedPreset.AdaptiveStreaming** (önerilir): Daha fazla bilgi için, [otomatik bir bitrate merdiven imal](autogen-bitrate-ladder.md)bakın.
- **EncoderNamedPreset.ContentAwareEncodingExperimental**: içerik farkında kodlama için deneysel bir ön ayar ortaya çıkarır. Herhangi bir giriş içeriği göz önüne alındığında, hizmet otomatik olarak en uygun katman sayısını ve uygun bithızı ve çözüm ayarlarını uyarlamalı akış tarafından teslim için belirlemeye çalışır. Altta yatan algoritmalar zaman içinde gelişmeye devam edecektir. Çıkış video ve ses interleaved mp4 dosyaları içerecektir. Daha fazla bilgi [için, içerik farkında kodlama için Deneysel ön ayar'a](content-aware-encoding.md)bakın.
- **EncoderNamedPreset.H264MultipleBitrate1080p**: 6000 kbps ile 400 kbps arasında değişen sekiz GOP uyumlu MP4 dosyası ve stereo AAC ses kümesi üretir. Çözünürlük 1080p başlar ve 360p aşağı gider.
- **EncoderNamedPreset.H264MultipleBitrate720p**: 3400 kbps ile 400 kbps arasında değişen altı GOP uyumlu MP4 dosyası ve stereo AAC ses kümesi üretir. Çözünürlük 720p başlar ve 360p aşağı gider.
- **EncoderNamedPreset.H264MultipleBitrateSD**: 1600 kbps ile 400 kbps arasında değişen beş GOP uyumlu MP4 dosyası ve stereo AAC ses kümesi üretir. Çözünürlük 480p başlar ve 360p aşağı gider.
- **EncoderNamedPreset.H264SingleBitrate1080p**: video 6750 kbps h.264 codec ve 1080 piksel bir resim yüksekliği ile kodlanır bir MP4 dosyası üretir ve stereo ses 64 kbps AAC-LC codec ile kodlanır.
- **EncoderNamedPreset.H264SingleBitrate720p**: video 4500 kbps h.264 codec ve 720 piksel bir resim yüksekliği ile kodlanır bir MP4 dosyası üretir ve stereo ses 64 kbps AAC-LC codec ile kodlanır.
- **EncoderNamedPreset.H264SingleBitrateSD**: video 2200 kbps h.264 codec ve 480 piksel bir resim yüksekliği ile kodlanır bir MP4 dosyası üretir ve stereo ses 64 kbps AAC-LC codec ile kodlanır.

En güncel hazır ayarlar listesini görmek [için, videoları kodlamak için kullanılacak yerleşik hazır ayarlara](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#encodernamedpreset)bakın.

Hazır ayarların nasıl kullanıldığını görmek için [dosya yükleme, kodlama ve akış](stream-files-tutorial-with-api.md)adabına bakın.

### <a name="standardencoderpreset"></a>StandardEncoderPreset

[StandardEncoderPreset,](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) giriş videosunu Standart Kodlayıcı ile kodlarken kullanılacak ayarları açıklar. Dönüşüm hazır ayarlarını özelleştirirken bu önceden ayaryı kullanın.

#### <a name="considerations"></a>Dikkat edilmesi gerekenler

Özel ön ayarlar oluşturulurken aşağıdaki hususlar geçerlidir:

- AVC içeriğindeki yükseklik ve genişlik için tüm değerler dört değerin bir katı olmalıdır.
- Azure Media Services v3'te, tüm kodlama bit hızları saniyede bit şeklindedir. Bu birim olarak kilobit /saniye kullanılan bizim v2 API'lar ile ön ayarlar farklıdır. Örneğin, v2'deki bit hızı 128 (kilobit/saniye) olarak belirtilmişse, v3'te 128000 (bit/saniye) olarak ayarlanır.

### <a name="customizing-presets"></a>Hazır ayarları özelleştirme

Medya Hizmetleri, özel kodlama gereksinimlerinizi ve gereksinimlerinizi karşılamak için tüm değerleri ön ayarlarda özelleştirmeyi tam olarak destekler. Kodlayıcı hazır ayarlarını nasıl özelleştireceğimi gösteren örnekler için aşağıdaki listeye bakın:

#### <a name="examples"></a>Örnekler

- [.NET ile ön ayarları özelleştirin](customize-encoder-presets-how-to.md)
- [CLI ile hazır ayarları özelleştirin](custom-preset-cli-howto.md)
- [REST ile hazır ayarları özelleştirin](custom-preset-rest-howto.md)

## <a name="preset-schema"></a>Önceden ayarlanmış şema

Media Services v3'te, ön ayarlar API'nin kendisinde güçlü bir şekilde yazılan varlıklardır. Bu nesneler için "şema" tanımını [Açık API Belirtiminde (veya Swagger)'da](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01)bulabilirsiniz. [Ayrıca REST API,](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.standardencoderpreset?view=azure-dotnet) (veya diğer Medya Hizmetleri v3 SDK başvuru belgelerinde **StandardEncoderPreset**gibi) önceden ayarlanmış tanımları görüntüleyebilirsiniz.

## <a name="scaling-encoding-in-v3"></a>v3'te kodlama ölçekleme

Ortam işlemeyi ölçeklendirmek için [CLI ile Ölçek'e](media-reserved-units-cli-how-to.md)bakın.

## <a name="billing"></a>Faturalandırma

Medya Hizmetleri iptal edilen veya hatalı olan işler için faturalandırmaz. Örneğin, %50 ilerleme kaydetmiş ve iptal edilmiş bir iş, iş dakikalarının %50'si olarak faturalandırılmez. Sadece bitmiş işler için ücretlendirilirsiniz.

Daha fazla bilgi için bkz. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/).

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

* [Medya Hizmetlerini kullanarak yükleme, kodlama ve akış.](stream-files-tutorial-with-api.md)
* [Yerleşik hazır ayarları kullanarak bir HTTPS URL'sinden kodlayın.](job-input-from-http-how-to.md)
* [Yerleşik hazır ayarları kullanarak yerel bir dosyayı kodlayın.](job-input-from-local-file-how-to.md)
* [Belirli senaryonuzu veya aygıt gereksinimlerinizi hedeflemek için özel bir ön ayar oluşturun.](customize-encoder-presets-how-to.md)
