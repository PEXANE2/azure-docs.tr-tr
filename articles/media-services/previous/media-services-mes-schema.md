---
title: Medya Encoder Standart şema | Microsoft Dokümanlar
description: Bu makalede, Media Encoder Standart ön ayarlarının dayandığı XML şemasının bazı öğeleri ve türleri açıklanmaktadır.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 622f14beabb1f2f109dff5d28c1591ffdd5aa000
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74901445"
---
# <a name="media-encoder-standard-schema"></a>Media Encoder Standard şeması
Bu makalede, [Media Encoder Standart ön ayarlarının](media-services-mes-presets-overview.md) dayandığı XML şemasının bazı öğeleri ve türleri açıklanmaktadır. Makalede, öğeler ve bunların geçerli değerleri açıklama verir.  

## <a name="preset-root-element"></a><a name="Preset"></a>Önceden ayarlanmış (kök öğesi)
Kodlama ön kümesini tanımlar.  

### <a name="elements"></a>Öğeler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Encoding** |[Encoding](media-services-mes-schema.md#Encoding) |Kök öğesi, giriş kaynaklarının kodlanacak olduğunu gösterir. |
| **Çıkışlar** |[Çıkışlar](media-services-mes-schema.md#Output) |İstenilen çıktı dosyalarının toplanması. |
| **StretchMode**<br/>minOccurs="0"<br/>default="Otomatik Boyutlandırma|xs:string|Çıkış video kare boyutunu, dolguyu, pikseli veya ekran en boy oranını denetle. **StretchMode** aşağıdaki değerlerden biri olabilir: **Yok**, **Otomatik Boyut** (varsayılan) veya **AutoFit**.<br/><br/>**Yok**: Giriş videosunun piksel en boy oranını veya ekran en boy oranını düşünmeden çıkış çözünürlüğünü (örneğin, ön ayardaki **Genişlik** ve **Yükseklik)** kesinlikle izleyin. Kırpma [gibi](media-services-crop-video.md)senaryolarda önerilen , çıktı video girişine göre farklı bir en boy oranına sahip olduğu. <br/><br/>**AutoSize**: Çıkış çözünürlüğü önceden ayarlanmış olarak belirtilen pencerenin içine (Genişlik * Yükseklik) sığar. Ancak, kodlayıcı kare (1:1) piksel en boy oranına sahip bir çıkış videosu üretir. Bu nedenle, girişin ekran en boy oranıyla eşleşecek şekilde, dolgu olmadan çıkış Genişliği veya çıkış Yüksekliği geçersiz kılınabilir. Örneğin, giriş 1920x1080 ise ve kodlama ön kümesi 1280x1280 isterse, ön ayardaki Yükseklik değeri geçersiz kılınır ve çıktı 16:9 giriş en boy oranını koruyan 1280x720'de olur. <br/><br/>**AutoFit**: Gerekirse, çıkış videosunu (letterbox veya pillarbox ile) istenilen çıkış çözünürlüğünü onurlandırmak için pad leştirin ve çıktıdaki etkin video bölgesinin girişle aynı en boy oranına sahip olmasını sağlar. Örneğin, girişin 1920x1080 olduğunu ve kodlama ön kümesinin 1280x1280'i sorduğunu varsayalım. Sonra çıkış video 1280x1280 olacak, ama 16:9 en boy oranı ile 'aktif video' bir iç 1280x720 dikdörtgen içerecektir ve letterbox bölgeleri 280 piksel üst ve alt yüksek. Başka bir örnek, giriş 1440x1080 ise ve kodlama ön kümesi 1280x720 sorarsa, çıkış 1280x720' de olacaktır, bu da 4:3 en boy oranıyla 960x720' lik bir iç dikdörtgen ve sol ve sağda 160 piksel genişliğinde sütun kutusu bölgeleri içerir. 

### <a name="attributes"></a>Öznitelikler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Sürüm**<br/><br/> Gerekli |**xs: ondalık** |Önceden ayarlanmış sürüm. Aşağıdaki kısıtlamalar geçerlidir: xs:fractionDigits value="1" ve xs:minInclusive value="1" Örneğin, **version="1.0"**. |

## <a name="encoding"></a><a name="Encoding"></a>Kodlama
Aşağıdaki öğelerin bir dizi içerir:  

### <a name="elements"></a>Öğeler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **H264Video** |[H264Video](media-services-mes-schema.md#H264Video) |Videonun H.264 kodlanması için ayarlar. |
| **AACAudio** |[AACAudio](media-services-mes-schema.md#AACAudio) |Sesin AAC kodlaması için ayarlar. |
| **BmpImage** |[BmpImage](media-services-mes-schema.md#BmpImage) |Bmp görüntü ayarları. |
| **PngImage** |[PngImage](media-services-mes-schema.md#PngImage) |Png görüntü için ayarlar. |
| **JpgImage** |[JpgImage](media-services-mes-schema.md#JpgImage) |Jpg görüntü için ayarlar. |

## <a name="h264video"></a><a name="H264Video"></a>H264Video
### <a name="elements"></a>Öğeler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **İki Geçit**<br/><br/> minOccurs="0" |**xs:boolean** |Şu anda yalnızca tek geçişli kodlama desteklenir. |
| **Anahtar Çerçeve Aralığı**<br/><br/> minOccurs="0"<br/><br/> **varsayılan="00:00:02"** |**xs:zaman** |IDR çerçeveler arasındaki sabit aralığı saniye birimleri olarak belirler. Ayrıca GOP süresi olarak anılacaktır. Kodlayıcının bu değerden sapıp sapamayacağını denetlemek için **SceneChangeDetection'a** bakın. |
| **Sahne Değişikliği Algılama**<br/><br/> minOccurs="0"<br/><br/> varsayılan="false" |**xs: boolean** |Doğru olarak ayarlanmışsa, kodlayıcı videodaki sahne değişikliğini algılamaya çalışır ve bir IDR çerçeve ekler. |
| **Karmaşık -lığı**<br/><br/> minOccurs="0"<br/><br/> default="Dengeli" |**xs:string** |Kod hızı ve video kalitesi arasındaki dengeleri kontrol eder. Aşağıdaki değerlerden biri olabilir: **Hız**, **Dengeli**, veya **Kalite**<br/><br/> Varsayılan: **Dengeli** |
| **Eşitleme Modu**<br/><br/> minOccurs="0" | |Özellik gelecekteki bir sürümde ortaya çıkacaktır. |
| **H264Katmanlar**<br/><br/> minOccurs="0" |[H264Katmanlar](media-services-mes-schema.md#H264Layers) |Çıktı video katmanlarının toplanması. |

### <a name="attributes"></a>Öznitelikler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Koşul** |**xs:string** | Girişte video yoksa, kodlayıcıyı tek renkli bir video parçası eklemeye zorlamak isteyebilirsiniz. Bunu yapmak için, Koşul="InsertBlackIfNoVideoBottomLayerOnly" (yalnızca en düşük bit hızında bir video eklemek için) veya Condition="InsertBlackIfNoVideo" (tüm çıktı bitratelerine video eklemek için) kullanın. Daha fazla bilgi için [bu makaleye](media-services-advanced-encoding-with-mes.md#no_video) bakın.|

## <a name="h264layers"></a><a name="H264Layers"></a>H264Katmanlar

Varsayılan olarak, kodlayıcıya yalnızca ses içeren ve video içermeyen bir giriş gönderirseniz, çıkış kıymeti yalnızca ses verilerine sahip dosyalar içerir. Bazı oyuncular bu tür çıkış akışlarını işleyebilir. H264Video'nun **InsertBlackIfNoVideo** öznitelik ayarını kullanarak kodlayıcıyı bu senaryoda çıktıya bir video parçası eklemeye zorlayabilirsiniz. Daha fazla bilgi için [bu makaleye](media-services-advanced-encoding-with-mes.md#no_video) bakın.
              
### <a name="elements"></a>Öğeler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **H264Layer**<br/><br/> minOccurs="0" maxOccurs="sınırsız" |[H264Layer](media-services-mes-schema.md#H264Layer) |H264 katmanları bir koleksiyon. |

## <a name="h264layer"></a><a name="H264Layer"></a>H264Layer
> [!NOTE]
> Video sınırları [H264 Düzeyleri](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC#Levels) tablosunda açıklanan değerleri temel almaktadır.  
> 
> 

### <a name="elements"></a>Öğeler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Profil**<br/><br/> minOccurs="0"<br/><br/> default="Otomatik" |**xs: dize** |Aşağıdaki **x'lerden biri olabilir: dize** değerleri: **Otomatik**, **Ana**, **Ana**, **Yüksek**. |
| **Düzey**<br/><br/> minOccurs="0"<br/><br/> default="Otomatik" |**xs: dize** | |
| **Bitrate**<br/><br/> minOccurs="0" |**xs:int** |KBP'lerde belirtilen bu video katmanı için kullanılan bit hızı. |
| **MaxBitrate**<br/><br/> minOccurs="0" |**xs: int** |KBP'lerde belirtilen bu video katmanı için kullanılan maksimum bit hızı. |
| **ArabellekPenceresi**<br/><br/> minOccurs="0"<br/><br/> varsayılan="00:00:05" |**xs: zaman** |Video arabelleği uzunluğu. |
| **Genişlik**<br/><br/> minOccurs="0" |**xs: int** |Çıkış video çerçevesinin piksel genişliği.<br/><br/> Şu anda, hem Genişlik hem de Yükseklik belirtmeniz gerekir. Genişlik ve Yükseklik çift sayılar olmalıdır. |
| **Height**<br/><br/> minOccurs="0" |**xs:int** |Çıkış video çerçevesinin piksel yüksekliği.<br/><br/> Şu anda, hem Genişlik hem de Yükseklik belirtmeniz gerekir. Genişlik ve Yükseklik çift sayılar olmalıdır.|
| **BFrame'ler**<br/><br/> minOccurs="0" |**xs: int** |Başvuru çerçeveleri arasındaki B kare sayısı. |
| **Referans Çerçeveler**<br/><br/> minOccurs="0"<br/><br/> varsayılan="3" |**xs:int** |GOP'daki başvuru çerçevesi sayısı. |
| **EntropiMode**<br/><br/> minOccurs="0"<br/><br/> default="Cabac" |**xs: dize** |**Cabac** ve **Cavlc:** Aşağıdaki değerlerden biri olabilir. |
| **Framerate**<br/><br/> minOccurs="0" |rasyonel sayı |Çıktı videosunun kare hızını belirler. Kodlayıcının giriş videosuyla aynı kare hızını kullanmasına izin vermek için "0/1" varsayılanını kullanın. İzin verilen değerlerin ortak video kare hızları olması beklenir. Ancak, herhangi bir geçerli rasyonel izin verilir. Örneğin, 1/1 1 fps olacaktır ve geçerlidir.<br/><br/> - 12/1 (12 fps)<br/><br/> - 15/1 (15 fps)<br/><br/> - 24/1 (24 fps)<br/><br/> - 24000/1001 (23.976 fps)<br/><br/> - 25/1 (25 fps)<br/><br/>  - 30/1 (30 fps)<br/><br/> - 30000/1001 (29,97 fps) <br/> <br/>**NOT** Birden çok bit hızında kodlama için özel bir ön ayar oluşturuyorsanız, önceden ayarlanmış tüm katmanları FrameRate'nin aynı değerini **kullanmalıdır.**|
| **AdaptiveBFrame**<br/><br/> minOccurs="0" |**xs: boolean** |Azure ortam kodlayıcısından kopyalama |
| **Dilimler**<br/><br/> minOccurs="0"<br/><br/> varsayılan="0" |**xs:int** |Bir çerçevenin kaç dilime bölündüğü belirlenir. Varsayılan kullanmanızı öneririz. |

## <a name="aacaudio"></a><a name="AACAudio"></a>AACAudio
 Aşağıdaki öğelerin ve grupların bir dizi içerir.  

 AAC hakkında daha fazla bilgi için [AAC'ye](https://en.wikipedia.org/wiki/Advanced_Audio_Coding)bakın.  

### <a name="elements"></a>Öğeler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Profil**<br/><br/> minOccurs="0 "<br/><br/> varsayılan="AACLC" |**xs: dize** |Aşağıdaki değerlerden biri olabilir: **AACLC**, **HEAACV1**, veya **HEAACV2**. |

### <a name="attributes"></a>Öznitelikler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Koşul** |**xs: dize** |Girişte ses olmadığında sessiz ses parçası içeren bir varlık üretmeye kodlayıcıyı zorlamak için "InsertSilenceIfNoAudio" değerini belirtin.<br/><br/> Varsayılan olarak, kodlayıcıya yalnızca video içeren ve ses içermeyen bir giriş gönderirseniz, çıkış kıymeti yalnızca video verileri içeren dosyalar içerir. Bazı oyuncular bu tür çıkış akışlarını işleyebilir. Bu ayarı, kodlayıcıyı bu senaryoda çıktıya sessiz bir ses parçası eklemeye zorlamak için kullanabilirsiniz. |

### <a name="groups"></a>Gruplar

| Başvuru | Açıklama |
| --- | --- |
| [Sesli Grup](media-services-mes-schema.md#AudioGroup)<br/><br/> minOccurs="0" |Her profil için ayarlanabilecek uygun kanal sayısını, örnekleme oranını ve bit hızını bilmek için [AudioGroup'un](media-services-mes-schema.md#AudioGroup) açıklamasına bakın. |

## <a name="audiogroup"></a><a name="AudioGroup"></a>Sesli Grup
Her profil için hangi değerlerin geçerli olduğu yla ilgili ayrıntılar için aşağıdaki "Ses kodayrıntıları" tablosuna bakın.  

### <a name="elements"></a>Öğeler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Kanallar**<br/><br/> minOccurs="0" |**xs: int** |Kodlanmış ses kanallarının sayısı. Geçerli seçenekler şunlardır: 1, 2, 5, 6, 8.<br/><br/> Varsayılan: 2. |
| **Örnekleme Hızı**<br/><br/> minOccurs="0" |**xs: int** |Hz.'de belirtilen ses örnekleme hızı. |
| **Bitrate**<br/><br/> minOccurs="0" |**xs: int** |KBP'lerde belirtilen ses kodlarken kullanılan bithızı. |

### <a name="audio-codec-details"></a>Ses codec ayrıntıları

Ses Codec|Ayrıntılar  
-----------------|---  
**AACLC** |1:<br/><br/> - 11025: &lt;8 &lt; = bithızı 16<br/><br/> - 12000: &lt;8 &lt; = bithızı 16<br/><br/> - 16000: &lt;8 &lt;= bithızı 32<br/><br/>- 22050: &lt;24 &lt; = bithızı 32<br/><br/> - 24000: &lt;24 &lt; = bithızı 32<br/><br/> - 32000: &lt;32 &lt;= bithızı = 192<br/><br/> - 44100: &lt;56 &lt;= bithızı = 288<br/><br/> - 48000: &lt;56 &lt;= bithızı = 288<br/><br/> - 88200 : &lt;128 &lt;= bithızı = 288<br/><br/> - 96000 : &lt;128 &lt;= bithızı = 288<br/><br/> 2:<br/><br/> - 11025: &lt;16 &lt; = bithızı 24<br/><br/> - 12000: &lt;16 &lt; = bithızı 24<br/><br/> - 16000: &lt;16 &lt; = bithızı 40<br/><br/> - 22050: &lt;32 &lt; = bithızı 40<br/><br/> - 24000 : &lt;32 &lt; = bithızı 40<br/><br/> - 32000: &lt;40 &lt;= bithızı = 384<br/><br/> - 44100: &lt;96 &lt;= bithızı = 576<br/><br/> - 48000 : &lt;96 &lt;= bithızı = 576<br/><br/> - 88200: 256 &lt; &lt;= bithızı = 576<br/><br/> - 96000: 256 &lt; &lt;= bithızı = 576<br/><br/> 5/6:<br/><br/> - 32000: 160 &lt; &lt;= bithızı = 896<br/><br/> - 44100: 240 &lt; &lt;= bithızı = 1024<br/><br/> - 48000: 240 &lt; &lt;= bithızı = 1024<br/><br/> - 88200: 640 &lt; &lt;= bithızı = 1024<br/><br/> - 96000: 640 &lt; &lt;= bithızı = 1024<br/><br/> 8:<br/><br/> - 32000 : &lt;224 &lt;= bithızı = 1024<br/><br/> - 44100 : &lt;384 &lt;= bithızı = 1024<br/><br/> - 48000: 384 &lt; &lt;= bithızı = 1024<br/><br/> - 88200: 896 &lt; &lt;= bithızı = 1024<br/><br/> - 96000: 896 &lt; &lt;= bitrate = 1024  
**HEAACV1** |1:<br/><br/> - 22050: bitrate = 8<br/><br/> - 24000: &lt;8 &lt;= bithızı = 10<br/><br/> - 32000: &lt;12 &lt;= bithızı = 64<br/><br/> - 44100: &lt;20 &lt;= bithızı = 64<br/><br/> - 48000: &lt;20 &lt;= bithızı = 64<br/><br/> - 88200: bitrate = 64<br/><br/> 2:<br/><br/> - 32000: &lt;16 &lt;= bithızı = 128<br/><br/> - 44100: &lt;16 &lt;= bithızı = 128<br/><br/> - 48000: &lt;16 &lt;= bithızı = 128<br/><br/> - 88200 : &lt;96 &lt;= bithızı = 128<br/><br/> - 96000: &lt;96 &lt;= bithızı = 128<br/><br/> 5/6:<br/><br/> - 32000 : &lt;64 &lt;= bithızı = 320<br/><br/> - 44100: &lt;64 &lt;= bithızı = 320<br/><br/> - 48000: &lt;64 &lt;= bithızı = 320<br/><br/> - 88200 : &lt;256 &lt;= bithızı = 320<br/><br/> - 96000: 256 &lt; &lt;= bithızı = 320<br/><br/> 8:<br/><br/> - 32000: &lt;96 &lt;= bithızı = 448<br/><br/> - 44100: &lt;96 &lt;= bithızı = 448<br/><br/> - 48000: &lt;96 &lt;= bithızı = 448<br/><br/> - 88200: 384 &lt; &lt;= bithızı = 448<br/><br/> - 96000: 384 &lt; &lt;= bithızı = 448  
**HEAACV2** |2:<br/><br/> - 22050: &lt;8 &lt;= bithızı = 10<br/><br/> - 24000: &lt;8 &lt;= bithızı = 10<br/><br/> - 32000: &lt;12 &lt;= bithızı = 64<br/><br/> - 44100: &lt;20 &lt;= bithızı = 64<br/><br/> - 48000: &lt;20 &lt;= bithızı = 64<br/><br/> - 88200: &lt;64 &lt;= bithızı = 64  
  
## <a name="clip"></a><a name="Clip"></a>Klip
### <a name="attributes"></a>Öznitelikler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Starttime** |**xs:süre** |Sununun başlangıç saatini belirtir. Başlangıç Zamanı'nın değerinin giriş videosunun mutlak zaman damgaları ile eşleşmesi gerekir. Örneğin, giriş videosunun ilk karesinde 12:00:10.000'lik bir zaman damgası varsa, Başlangıç Saati en az 12:00:10.000 veya daha büyük olmalıdır. |
| **Süre** |**xs:süre** |Sununun süresini belirtir (örneğin, videodaki bir bindirmenin görünümü). |

## <a name="output"></a><a name="Output"></a>Çıkış
### <a name="attributes"></a>Öznitelikler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Dosyaadı** |**xs:string** |Çıktı dosyasının adı.<br/><br/> Çıktı dosya adlarını oluşturmak için aşağıdaki tabloda açıklanan makroları kullanabilirsiniz. Örnek:<br/><br/> **"Çıktılar": [ { "FileName": "{Basename} {Resolution} {Bitrate}.mp4", "Format": { "Type": "MP4Format" } } } } } } } } }*} } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } }*** |

### <a name="macros"></a>Makrolar

| Makro | Açıklama |
| --- | --- |
| **{Taban ad}** |VoD kodlaması yapıyorsanız, {Basename} giriş kıymetindeki birincil dosyanın AssetFile.Name özelliğinin ilk 32 karakteridir.<br/><br/> Giriş kıymeti canlı bir arşivse, sunucu bildirimindeki trackName özniteliklerinden {Basename} türetilir. TopBitrate'yi kullanarak bir alt klip işi gönderiliyorsanız: "<VideoStream\>TopBitrate</VideoStream\>", ve çıktı dosyası video içeriyorsa, {Basename} en yüksek bit hızına sahip video katmanının trackName'sinin ilk 32 karakteridir.<br/><br/> Bunun yerine , "<VideoStream\>*</VideoStream\>" gibi tüm giriş bitratesini kullanarak bir alt klip işi gönderiliyorsanız ve çıktı dosyası video içeriyorsa, {Basename} ilgili video katmanının trackName'sinin ilk 32 karakteridir. |
| **{Codec}** |Video için "H264" ve ses için "AAC" haritaları. |
| **{Bitrate}** |Çıkış dosyası video ve ses içeriyorsa hedef video bithızı veya çıkış dosyası yalnızca ses içeriyorsa hedef ses bithızı. Kullanılan değer kbps bitrate olduğunu. |
| **{Kanal}** |Dosya ses içeriyorsa ses kanalı sayısı. |
| **{Genişlik}** |Videonun genişliği, pikselolarak, çıktı dosyasında, dosya video içeriyorsa. |
| **{Yükseklik}** |Videonun yüksekliği, piksel olarak, çıktı dosyasında, dosya video içeriyorsa. |
| **{Uzantı}** |Çıktı dosyası için "Type" özelliğinden devralır. Çıktı dosyası adı bir uzantısı vardır: "mp4", "ts", "jpg", "png", veya "bmp". |
| **{Dizin}** |Küçük resim için zorunludur. Sadece bir kez orada olmalı. |

## <a name="video-complex-type-inherits-from-codec"></a><a name="Video"></a>Video (Codec'ten karmaşık tür devralır)
### <a name="attributes"></a>Öznitelikler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Başlangıç** |**xs:string** | |
| **Adım** |**xs:string** | |
| **Aralığı** |**xs:string** | |
| **Döndürme Sonrası Çözünürlüğü Koruma** |**xs:boolean** |Ayrıntılı açıklama için aşağıdaki bölüme bakın: [PreserveResolutionAfterRotation](media-services-mes-schema.md#PreserveResolutionAfterRotation) |

### <a name="preserveresolutionafterrotation"></a><a name="PreserveResolutionAfterRotation"></a>Döndürme Sonrası Çözünürlüğü Koruma
Yüzde açısından ifade edilen çözünürlük değerleri ile birlikte **PreserveResolutionAfterRotation** bayrağının kullanılması önerilir (Genişlik="%100" , Yükseklik="%100").  

Varsayılan olarak, Media Encoder Standard (MES) hazır ayarlarındaki kod çözme çözünürlüğü ayarları (Genişlik, Yükseklik) 0 derece döndürmeye sahip videolara hedeflenir. Örneğin, giriş videonuz sıfır derece döndürme ile 1280x720 ise, varsayılan hazır ayarlar çıktının aynı çözünürlüğe sahip olmasını sağlar.  

![MESRoation1](./media/media-services-shemas/media-services-mes-roation1.png) 

Giriş videosu sıfır olmayan döndürme (örneğin, dikey olarak tutulan bir akıllı telefon veya tablet) ile yakalanmışsa, MES varsayılan olarak giriş videosuna kod çözme çözünürlüğü ayarlarını (Genişlik, Yükseklik) uygular ve sonra döndürmeyi telafi eder. Örneğin, aşağıdaki resme bakın. Önceden ayarlanmış, Mes'in çıkışın 1280 piksel genişliğinde ve 720 piksel yüksekliğinde olmasını gerektiren olarak yorumladığı Genişlik = "%100", Yükseklik = "%100"' i kullanır. Videoyu döndürdükten sonra, resmi bu pencereye sığacak şekilde küçülterek sol ve sağdaki sütun kutusu alanlarına yönlendirilir.  

![MESRoation2](./media/media-services-shemas/media-services-mes-roation2.png) 

Alternatif olarak, **PreserveResolutionAfterRotation** bayrağını kullanabilir ve "true" (varsayılan "yanlış") olarak ayarlayabilirsiniz. Yani ön ayarınızın Genişliği = "%100", Yükseklik = "%100" ve PreserveResolutionAfterRotation "true" olarak ayarlanmışsa, 1280 piksel genişliğinde ve 90 derece döndürülme ile 720 piksel yüksekliğinde bir giriş videosu sıfır derece döndürme ile bir çıkış üretir, ancak 720 piksel genişliğinde ve 1280 piksel uzun boylu. Aşağıdaki resme bakın:  

![MESRoation3](./media/media-services-shemas/media-services-mes-roation3.png) 

## <a name="formatgroup-group"></a><a name="FormatGroup"></a>FormatGroup (grup)
### <a name="elements"></a>Öğeler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **BmpFormat** |**BmpFormat** | |
| **PngFormat** |**PngFormat** | |
| **JpgFormat** |**JpgFormat** | |

## <a name="bmplayer"></a><a name="BmpLayer"></a>BmpLayer
### <a name="element"></a>Öğe

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Genişlik**<br/><br/> minOccurs="0" |**xs:int** | |
| **Height**<br/><br/> minOccurs="0" |**xs:int** | |

### <a name="attributes"></a>Öznitelikler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Koşul** |**xs:string** | |

## <a name="pnglayer"></a><a name="PngLayer"></a>PngLayer
### <a name="element"></a>Öğe

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Genişlik**<br/><br/> minOccurs="0" |**xs:int** | |
| **Height**<br/><br/> minOccurs="0" |**xs:int** | |

### <a name="attributes"></a>Öznitelikler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Koşul** |**xs:string** | |

## <a name="jpglayer"></a><a name="JpgLayer"></a>JpgLayer
### <a name="element"></a>Öğe

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Genişlik**<br/><br/> minOccurs="0" |**xs:int** | |
| **Height**<br/><br/> minOccurs="0" |**xs:int** | |
| **Kalite**<br/><br/> minOccurs="0" |**xs:int** |Geçerli değerler: 1(en kötü)-100(en iyi) |

### <a name="attributes"></a>Öznitelikler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Koşul** |**xs:string** | |

## <a name="pnglayers"></a><a name="PngLayers"></a>PngKatmanları
### <a name="elements"></a>Öğeler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **PngLayer**<br/><br/> minOccurs="0" maxOccurs="sınırsız" |[PngLayer](media-services-mes-schema.md#PngLayer) | |

## <a name="bmplayers"></a><a name="BmpLayers"></a>BmpLayers
### <a name="elements"></a>Öğeler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **BmpLayer**<br/><br/> minOccurs="0" maxOccurs="sınırsız" |[BmpLayer](media-services-mes-schema.md#BmpLayer) | |

## <a name="jpglayers"></a><a name="JpgLayers"></a>Jpg Katmanları
### <a name="elements"></a>Öğeler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **JpgLayer**<br/><br/> minOccurs="0" maxOccurs="sınırsız" |[JpgLayer](media-services-mes-schema.md#JpgLayer) | |

## <a name="bmpimage-complex-type-inherits-from-video"></a><a name="BmpImage"></a>BmpImage (karmaşık tip Video'dan devralır)
### <a name="elements"></a>Öğeler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **PngKatmanları**<br/><br/> minOccurs="0" |[PngKatmanları](media-services-mes-schema.md#PngLayers) |Png katmanları |

## <a name="jpgimage-complex-type-inherits-from-video"></a><a name="JpgImage"></a>JpgImage (videodan karmaşık tip devralır)
### <a name="elements"></a>Öğeler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **PngKatmanları**<br/><br/> minOccurs="0" |[PngKatmanları](media-services-mes-schema.md#PngLayers) |Png katmanları |

## <a name="pngimage-complex-type-inherits-from-video"></a><a name="PngImage"></a>PngImage (videodan karmaşık tür devralır)
### <a name="elements"></a>Öğeler

| Adı | Tür | Açıklama |
| --- | --- | --- |
| **PngKatmanları**<br/><br/> minOccurs="0" |[PngKatmanları](media-services-mes-schema.md#PngLayers) |Png katmanları |

## <a name="examples"></a>Örnekler
Bu şema temel alınca oluşturulmuş XML hazır ayarlarının örneklerine bakın, [MES (Media Encoder Standard) için Görev Hazır Ayarları'na](media-services-mes-presets-overview.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

