---
title: Azure Media Services filtreleri tanımlama
description: Bu konuda, istemcinizin bir akışın belirli bölümlerini akışa almak için bunları kullanabilmesi için filtrelerin nasıl oluşturulacağı açıklanmaktadır. Media Services, bu seçmeli akışa ulaşmak için dinamik bildirimler oluşturuyor.
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
ms.date: 05/23/2019
ms.author: juliako
ms.openlocfilehash: fdf29924da31db0347938df89e698cb258c2336b
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84708306"
---
# <a name="filters"></a>Filtreler

İçeriğinizi müşterilere sunarken (canlı akış olayları veya Isteğe bağlı video), istemciniz varsayılan varlığın bildirim dosyasında açıklananlardan daha fazla esneklik gerektirebilir. Azure Media Services, önceden tanımlanmış filtreleri temel alarak [dinamik bildirimler](filters-dynamic-manifest-overview.md) sunmaktadır. 

Filtreler, müşterilerinizin şu gibi işlemler yapmasına izin veren sunucu tarafı kurallarıdır: 

- Videonun yalnızca bir bölümünü kayıttan yürütün (videonun tamamını çalmak yerine). Örneğin:
  - Canlı bir etkinliğin alt klibini ("alt klip filtrelemesi") göstermek için bildirimi azaltın veya
  - Videonun başlangıcını kırpın ("video kırpma").
- İçeriği kayıttan yürütmek için kullanılan cihaz tarafından desteklenen yalnızca belirtilen çevirileri ve/veya belirtilen dil izlerini sunun ("yorumlama filtrelemesi"). 
- Player 'da ("sunum penceresini ayarlama"), DVR penceresinde sınırlı bir uzunluk sağlamak için sunum penceresini (DVR) ayarlayın.

Media Services, içeriğiniz için **hesap filtreleri** ve **varlık filtreleri** oluşturmanıza olanak sağlar. Ayrıca, önceden oluşturulmuş filtrelerinizi bir **akış Bulucu**ile ilişkilendirebilirsiniz.

## <a name="defining-filters"></a>Filtreleri tanımlama

İki tür filtre vardır: 

* [Hesap filtreleri](https://docs.microsoft.com/rest/api/media/accountfilters) (Global)-Azure Media Services hesabındaki herhangi bir varlığa uygulanabilir, hesabın kullanım ömrü vardır.
* [Varlık filtreleri](https://docs.microsoft.com/rest/api/media/assetfilters) (yerel)-yalnızca, oluşturulduktan sonra filtrenin ilişkilendirildiği bir varlığa uygulanabilir, varlığın kullanım ömrü vardır. 

**Hesap filtreleri** ve **varlık filtreleri** türleri, filtre tanımlamak/açıklamak için tam olarak aynı özelliklere sahiptir. **Varlık filtresi**oluşturma dışında, filtre ilişkilendirmek istediğiniz varlık adını belirtmeniz gerekir.

Senaryonuza bağlı olarak, hangi tür bir filtrenin daha uygun olduğuna karar verirsiniz (varlık filtresi veya hesap filtresi). Hesap filtreleri, belirli bir varlığı kırpmak için varlık filtrelerinin kullanılabileceği cihaz profilleri (işleme filtrelemesi) için uygundur.

Filtreleri anlatmak için aşağıdaki özellikleri kullanabilirsiniz. 

|Name|Description|
|---|---|
|firstQuality|Filtrenin ilk kalite bit hızı.|
|presentationTimeRange|Sunum zaman aralığı. Bu özellik bildirim başlangıç/bitiş noktaları, sunum penceresi uzunluğu ve canlı başlangıç konumunu filtrelemek için kullanılır. <br/>Daha fazla bilgi için bkz. [Presentationtimerange](#presentationtimerange).|
|izler|Seçim koşullarını izler. Daha fazla bilgi için bkz. [parçalar](#tracks)|

### <a name="presentationtimerange"></a>presentationTimeRange

Bu özelliği **varlık filtreleriyle**kullanın. Özelliği **Hesap filtreleriyle**ayarlamanız önerilmez.

|Name|Description|
|---|---|
|**endTimestamp**|Isteğe bağlı video (VoD) için geçerlidir.<br/>Canlı akış sunusu için sessizce yok sayılır ve sunu sona erdiğinde ve akış VoD olduğunda uygulanır.<br/>Bu, sununun mutlak bitiş noktasını temsil eden, en yakın bir sonraki GOP başlangıcına yuvarlanan uzun bir değerdir. Birim zaman ölçeğdir, bu nedenle bir endTimestamp 1800000000 3 dakika olur.<br/>Şarkı listesinde olacak parçaları kırpmak için startTimestamp ve endTimestamp kullanın (manifest).<br/>Örneğin, varsayılan zaman ölçeğini kullanan startTimestamp = 40000000 ve endTimestamp = 100000000, VoD sunusunun 4 saniye ve 10 saniyelik parçalarını içeren bir çalma listesi oluşturur. Bir parça sınır ayırır, tüm parçalar bildirime dahil edilir.|
|**forceEndTimestamp**|Yalnızca canlı akış için geçerlidir.<br/>EndTimestamp özelliğinin mevcut olup olmadığını gösterir. True ise endTimestamp belirtilmelidir veya hatalı bir istek kodu döndürülür.<br/>İzin verilen değerler: false, true.|
|**Livebackoff süresi**|Yalnızca canlı akış için geçerlidir.<br/> Bu değer, bir istemcinin arayabileceğini en son canlı konumu tanımlar.<br/>Bu özelliği kullanarak, canlı kayıttan yürütme konumunu erteleyebilir ve oyuncular için sunucu tarafı arabelleği oluşturabilirsiniz.<br/>Bu özelliğin birimi zaman ölçeğinde (aşağıya bakın).<br/>Maksimum etkin geri kullanım süresi 300 saniyedir (3000000000).<br/>Örneğin, 2000000000 değeri, en son kullanılabilir içeriğin gerçek canlı kenarından 20 saniye gecikildiği anlamına gelir.|
|**presentationWindowDuration**|Yalnızca canlı akış için geçerlidir.<br/>Bir çalma listesine dahil edilecek parçaların kayan bir penceresini uygulamak için presentationWindowDuration ' i kullanın.<br/>Bu özelliğin birimi zaman ölçeğinde (aşağıya bakın).<br/>Örneğin, presentationWindowDuration = 1200000000 öğesini iki dakikalık bir kayan pencere uygulamak için ayarlayın. Canlı kenardan 2 dakikalık medya, çalma listesine dahil edilir. Bir parça sınır ayırır, tüm parça çalma listesine dahil edilir. En düşük sunu penceresi süresi 60 saniyedir.|
|**startTimestamp**|Isteğe bağlı video (VoD) veya canlı akış için geçerlidir.<br/>Bu, akışın mutlak başlangıç noktasını temsil eden uzun bir değerdir. Değer, en yakın sonraki GOP başlangıcına yuvarlanır. Birim zaman ölçeğinde bir Start150000000 timestamp 15 saniye olur.<br/>Şarkı listesinde olacak parçaları kırpmak için startTimestamp ve endTimestampp kullanın (manifest).<br/>Örneğin, varsayılan zaman ölçeğini kullanan startTimestamp = 40000000 ve endTimestamp = 100000000, VoD sunusunun 4 saniye ve 10 saniyelik parçalarını içeren bir çalma listesi oluşturur. Bir parça sınır ayırır, tüm parçalar bildirime dahil edilir.|
|**zaman**|Bir saniye içindeki Artımlar sayısı olarak belirtilen bir sunum zaman aralığındaki tüm zaman damgaları ve süreler için geçerlidir.<br/>Varsayılan değer 10000000-10.000.000 ' dir; burada her bir artış 100.<br/>Örneğin, 30 saniye içinde bir startTimestamp ayarlamak istiyorsanız varsayılan zaman ölçeğini kullanırken 300000000 değerini kullanırsınız.|

### <a name="tracks"></a>İzler

Stream 'in (canlı akış veya video Isteğe bağlı) dinamik olarak oluşturulmuş bildirime dahil olması gereken bir filtre izleme özelliği koşulları (FilterTrackPropertyConditions) listesini belirtirsiniz. Filtreler, mantıksal bir **and** ve **or** işlemi kullanılarak birleştirilir.

Filtre izleme özellik koşulları, izleme türlerini, değerleri (aşağıdaki tabloda açıklanan) ve işlemleri (eşittir, NotEqual) anlatmaktadır. 

|Name|Description|
|---|---|
|**Bit hızı**|Filtreleme için izlemenin bit hızını kullanın.<br/><br/>Önerilen değer bit/saniye cinsinden bir bit hızı aralığıdır. Örneğin, "0-2427000".<br/><br/>Not: 250000 (bit/saniye) gibi belirli bir bit hızı değeri kullanabileceğiniz sürece, tam bit hızları bir varlıktan diğerine doğru dalgalanmasına rağmen bu yaklaşım önerilmez.|
|**FourCC**|Filtreleme için parçanın FourCC değerini kullanın.<br/><br/>Değer, [RFC 6381](https://tools.ietf.org/html/rfc6381)' de belirtildiği gibi codec biçiminin ilk öğesidir. Şu anda aşağıdaki codec bileşenleri desteklenir: <br/>Video için: "avc1", "hev1", "hvc1"<br/>Ses için: "MP4A", "EC-3"<br/><br/>Bir varlık içindeki izlemelere ait FourCC değerlerini öğrenmek için bildirim dosyasını alın ve inceleyin.|
|**Dil**|Filtreleme için izleme dilini kullanın.<br/><br/>Değer, RFC 5646 ' de belirtildiği gibi, dahil etmek istediğiniz dilin etikettir. Örneğin, "en".|
|**Adı**|Filtreleme için izleme adını kullanın.|
|**Tür**|Filtreleme için izleme türünü kullanın.<br/><br/>Şu değerlere izin verilir: "video", "ses" veya "metin".|

### <a name="example"></a>Örnek

Aşağıdaki örnek bir canlı akış filtresi tanımlar: 

```json
{
  "properties": {
    "presentationTimeRange": {
      "startTimestamp": 0,
      "endTimestamp": 170000000,
      "presentationWindowDuration": 9223372036854776000,
      "liveBackoffDuration": 0,
      "timescale": 10000000,
      "forceEndTimestamp": false
    },
    "firstQuality": {
      "bitrate": 128000
    },
    "tracks": [
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Audio"
          },
          {
            "property": "Language",
            "operation": "NotEqual",
            "value": "en"
          },
          {
            "property": "FourCC",
            "operation": "NotEqual",
            "value": "EC-3"
          }
        ]
      },
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Video"
          },
          {
            "property": "Bitrate",
            "operation": "Equal",
            "value": "3000000-5000000"
          }
        ]
      }
    ]
  }
}
```

## <a name="associating-filters-with-streaming-locator"></a>Akış bulucu ile filtreleri ilişkilendirme

[Akış bulucusinizdeki](https://docs.microsoft.com/rest/api/media/streaminglocators/create#request-body) [varlık veya hesap filtrelerinin](filters-concept.md) listesini belirtebilirsiniz. [Dinamik Paketleyici](dynamic-packaging-overview.md) , bu filtre listesini ISTEMCINIZDEKI URL 'de belirttiği değişikliklerle birlikte uygular. Bu bileşim, akış bulucunun üzerinde belirlediğiniz URL + filtrelerdeki filtreleri temel alan [dinamik bir bildirim](filters-dynamic-manifest-overview.md)oluşturur. 

Aşağıdaki örneklere bakın:

* [Filtreleri akış bulucu ile ilişkilendir-.NET](filters-dynamic-manifest-dotnet-howto.md#associate-filters-with-streaming-locator)
* [Filtreleri akış bulucu ile ilişkilendir-CLı](filters-dynamic-manifest-cli-howto.md#associate-filters-with-streaming-locator)

## <a name="updating-filters"></a>Filtreler güncelleştiriliyor
 
Filtreler güncelleştiriyorsa, **akış Konumlandırıcı** güncelleştirilemez. 

Özellikle CDN etkinleştirildiğinde, etkin olarak yayımlanmış bir **akış bulucula**ilişkili filtrelerin tanımını güncelleştirmeniz önerilmez. Akış sunucuları ve CDNs, eski önbelleğe alınmış verilerin döndürülmesine neden olabilecek iç önbelleklere sahip olabilir. 

Filtre tanımının değiştirilmesi gerekiyorsa, yeni bir filtre oluşturmayı ve **akış Bulucu** URL 'sine eklemeyi veya doğrudan filtreye başvuran yeni bir **akış Bulucu** yayımlamayı düşünün.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde, programlı olarak filtrelerin nasıl oluşturulacağı gösterilmektedir.  

- [REST API 'Leri ile filtre oluşturma](filters-dynamic-manifest-rest-howto.md)
- [.NET ile filtre oluşturma](filters-dynamic-manifest-dotnet-howto.md)
- [CLı ile filtre oluşturma](filters-dynamic-manifest-cli-howto.md)

