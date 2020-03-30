---
title: Azure Medya Hizmetleri'nde filtreleri tanımlama
description: Bu konu, istemcinizin bir akışın belirli bölümlerini akışı için bunları kullanabilmesi için filtrelerin nasıl oluşturulabileceğini açıklar. Medya Hizmetleri, bu seçici akışı elde etmek için dinamik bildirimler oluşturur.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251472"
---
# <a name="filters"></a>Filtreler

İçeriğinizi müşterilere teslim ederken (Canlı Akış etkinlikleri veya Talep Üzerine Video) müşterinizin varsayılan varlığın bildirim dosyasında açıklanandan daha fazla esnekliğe ihtiyacı olabilir. Azure Medya Hizmetleri, önceden tanımlanmış filtrelere dayalı [Dinamik Bildirimler](filters-dynamic-manifest-overview.md) sunar. 

Filtreler, müşterilerinizin şu gibi şeyler yapmasına olanak tanıyan sunucu tarafı kurallarıdır: 

- Videonun yalnızca bir bölümünü oynatın (videonun tamamını oynatmak yerine). Örnek:
  - Canlı bir olayın bir alt klibini ("alt klip filtresi") göstermek için bildirimi azaltın veya
  - Bir videonun başlangıcını kırpın ("videoyu kırpma").
- Yalnızca içeriği oynatmak için kullanılan aygıt tarafından desteklenen belirtilen yorumlamaları ve/veya belirtilen dil parçalarını ("yorumlama filtresi") sunun. 
- Oyuncudaki DVR penceresinin sınırlı bir uzunluğunu sağlamak için Sunu Penceresini (DVR) ayarlayın ("sunu penceresini ayarlama").

Medya Hizmetleri, içeriğiniz için **Hesap filtreleri** ve **Varlık filtreleri** oluşturmanıza olanak tanır. Buna ek olarak, önceden oluşturulmuş filtrelerinizi bir **Akış Bulucuile**ilişkilendirebilirsiniz.

## <a name="defining-filters"></a>Filtreleri tanımlama

İki tür filtre vardır: 

* [Hesap Filtreleri](https://docs.microsoft.com/rest/api/media/accountfilters) (genel) - Azure Medya Hizmetleri hesabındaki herhangi bir varlığa uygulanabilir, hesabın ömrü dolabilir.
* [Varlık Filtreleri](https://docs.microsoft.com/rest/api/media/assetfilters) (yerel) - yalnızca filtrenin oluşturulduktan sonra ilişkili olduğu, varlığın ömrü ne kadar olduğu bir varlığa uygulanabilir. 

**Hesap Filtreleri** ve **Varlık Filtreleri** türleri, filtreyi tanımlamak/açıklamak için tam olarak aynı özelliklere sahiptir. **Varlık Filtresi**oluştururken dışında, filtreyi ilişkilendirmek istediğiniz varlık adını belirtmeniz gerekir.

Senaryonuza bağlı olarak, hangi filtre türünün daha uygun olduğuna (Varlık Filtresi veya Hesap Filtresi) karar verirsiniz. Hesap Filtreleri, Varlık Filtreleri'nin belirli bir varlığı kırpmak için kullanılabildiği aygıt profilleri (yorumlama filtreleme) için uygundur.

Filtreleri açıklamak için aşağıdaki özellikleri kullanırsınız. 

|Adı|Açıklama|
|---|---|
|firstQuality|Filtrenin ilk kalite bithızı.|
|sunumTimeRange|Sunum süresi aralığı. Bu özellik, bildirim başlangıç/bitiş noktalarını, sunu penceresi uzunluğunu ve canlı başlangıç konumunu filtreleme için kullanılır. <br/>Daha fazla bilgi için [PresentationTimeRange'e](#presentationtimerange)bakın.|
|Parça|Parça seçim koşulları. Daha fazla bilgi için [parçalara](#tracks) bakın|

### <a name="presentationtimerange"></a>sunumTimeRange

**Varlık Filtreleri**ile bu özelliği kullanın. **Özelliği Hesap Filtreleri**ile ayarlamak önerilmez.

|Adı|Açıklama|
|---|---|
|**endTimestamp**|İsteğe Bağlı Video (VoD) için geçerlidir.<br/>Canlı Akış sunusu için, sunu sona erdiğinde ve akış VoD olduğunda sessizce yoksayılır ve uygulanır.<br/>Bu, sununun mutlak bitiş noktasını temsil eden ve en yakın sonraki GOP başlangıcına yuvarlanan uzun bir değerdir. Birim zaman ölçeği, bu nedenle 1800000000 bir endTimestamp 3 dakika olacaktır.<br/>Çalma listesinde (bildirim) yer alacak parçaları kırpmak için startTimestamp ve endTimestamp'ı kullanın.<br/>Örneğin, varsayılan zaman ölçeğini kullanarak startTimestamp=40000000 ve endTimestamp=10000000, VoD sununun 4 saniye ile 10 saniye arasında parça içeren bir çalma listesi oluşturur. Bir parça sınıra kendize olursa, tüm parça bildirime dahil edilir.|
|**forceEndTimestamp**|Yalnızca Canlı Akış için geçerlidir.<br/>EndTimestamp özelliğinin bulunması gerekip gerekmediğini gösterir. Doğruysa, endTimestamp belirtilmelidir veya kötü bir istek kodu döndürülür.<br/>İzin verilen değerler: yanlış, doğru.|
|**liveBackoffSüre**|Yalnızca Canlı Akış için geçerlidir.<br/> Bu değer, istemcinin isteyebileceği en son canlı konumu tanımlar.<br/>Bu özelliği kullanarak, canlı oynatma konumunu geciktirebilir ve oyuncular için sunucu tarafı arabelleği oluşturabilirsiniz.<br/>Bu özelliğin birimi zaman ölçeğidir (aşağıya bakın).<br/>Maksimum canlı geri dönüş süresi 300 saniyedir (3000000000).<br/>Örneğin, 2000000000 değeri, kullanılabilir en son içeriğin gerçek canlı kenardan 20 saniye geciktiği anlamına gelir.|
|**sunumPencereSüresi**|Yalnızca Canlı Akış için geçerlidir.<br/>Çalma listesine eklemek üzere kayan bir parça penceresini uygulamak için sunuyuKullanınWindow süresini kullanın.<br/>Bu özelliğin birimi zaman ölçeğidir (aşağıya bakın).<br/>Örneğin, iki dakikalık bir sürgülü pencere uygulamak için sunuyu ayarlayınWindowDuration=1200000000. Canlı kenardan 2 dakika içinde medya çalma listesine dahil edilecektir. Bir parça sınıra kendize olursa, tüm parça çalma listesine eklenir. Minimum sunu penceresi süresi 60 saniyedir.|
|**başlangıçTimestamp**|İsteğe Bağlı Video (VoD) veya Canlı Akış için geçerlidir.<br/>Bu, akışın mutlak başlangıç noktasını temsil eden uzun bir değerdir. Değer, en yakın sonraki GOP başlangıcına yuvarlanır. Birim zaman ölçeğidir, bu nedenle 150000000'lik bir startTimestamp 15 saniye olacaktır.<br/>Çalma listesinde (bildirim) yer alacak parçaları kırpmak için startTimestamp ve endTimestampp'ı kullanın.<br/>Örneğin, varsayılan zaman ölçeğini kullanarak startTimestamp=40000000 ve endTimestamp=10000000, VoD sununun 4 saniye ile 10 saniye arasında parça içeren bir çalma listesi oluşturur. Bir parça sınıra kendize olursa, tüm parça bildirime dahil edilir.|
|**Zaman ölçeği**|Sunu Zaman Aralığındaki tüm zaman damgaları ve süreler için geçerlidir, bir saniyedeki artış sayısı olarak belirtilir.<br/>Varsayılan değer 10000000 - her artış 100 nanosaniye uzunluğunda olacağını bir saniyede on milyon artışvardır.<br/>Örneğin, bir başlangıç Zaman Damgası'nı 30 saniye olarak ayarlamak istiyorsanız, varsayılan zaman ölçeğini kullanırken 300000000 değerini kullanırsınız.|

### <a name="tracks"></a>Parça

Akışınızın (Canlı Akış veya İsteğe Bağlı Video) parçalarının dinamik olarak oluşturulan bildirime dahil edilmesi gereken filtre izleme özellik koşullarının (FilterTrackPropertyConditions) listesini belirtirsiniz. Filtreler mantıksal **VE** ve **OR** işlemi kullanılarak birleştirilir.

Filtre izleme özelliği koşulları parça türlerini, değerleri (aşağıdaki tabloda açıklanan) ve işlemleri (Eşit, Eşit olmayan) açıklar. 

|Adı|Açıklama|
|---|---|
|**Bitrate**|Filtreleme için parçanın bithızını kullanın.<br/><br/>Önerilen değer, saniyede bit olarak bir bit aralığıdır. Örneğin, "0-2427000".<br/><br/>Not: 250000 (saniyede bit) gibi belirli bir bit hızı değeri kullanabiliyor olsanız da, tam bitoranları bir Varlık'tan diğerine dalgalandırabildiği için bu yaklaşım önerilmez.|
|**Fourcc**|Filtreleme için parçanın FourCC değerini kullanın.<br/><br/>Değer, [RFC 6381'de](https://tools.ietf.org/html/rfc6381)belirtildiği gibi codec biçiminin ilk öğesidir. Şu anda, aşağıdaki codec'ler desteklenir: <br/>Video için: "avc1", "hev1", "hvc1"<br/>Ses için: "mp4a", "ec-3"<br/><br/>Bir Varlıktaki parçalar için FourCC değerlerini belirlemek için, bildirim dosyasını alın ve inceleyin.|
|**Dil**|Filtreleme için parçanın dilini kullanın.<br/><br/>Değer, RFC 5646'da belirtildiği gibi eklemek istediğiniz dilin etiketidir. Örneğin, "en".|
|**Adı**|Filtreleme için parçanın adını kullanın.|
|**Tür**|Filtreleme için parçanın türünü kullanın.<br/><br/>Aşağıdaki değerlere izin verilir: "video", "ses" veya "metin".|

### <a name="example"></a>Örnek

Aşağıdaki örnekte Canlı Akış filtresi tanımlanır: 

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

## <a name="associating-filters-with-streaming-locator"></a>Akış Lı Bulucu ile filtreleri ilişkilendirme

[Akış Lı Bulucunuzda](https://docs.microsoft.com/rest/api/media/streaminglocators/create#request-body) [varlık veya hesap filtrelerinin](filters-concept.md) listesini belirtebilirsiniz. [Dinamik Paketleyici,](dynamic-packaging-overview.md) bu filtre listesini istemcinizin URL'de belirttiği filtrelerle birlikte uygular. Bu kombinasyon, Akış Bulucu'da belirttiğiniz URL + filtrelerdeki filtrelere dayanan [dinamik bir bildirim](filters-dynamic-manifest-overview.md)oluşturur. 

Aşağıdaki örneklere bakın:

* [Filtreleri Akış Lı Bulucu ile ilişkilendir - .NET](filters-dynamic-manifest-dotnet-howto.md#associate-filters-with-streaming-locator)
* [Filtreleri Akış Lı Bulucu ile ilişkilendirin - CLI](filters-dynamic-manifest-cli-howto.md#associate-filters-with-streaming-locator)

## <a name="updating-filters"></a>Filtreleri güncelleme
 
**Akış Konum belirleyicileri** güncelleştirilmese de güncellenebilir değildir. 

Etkin olarak yayınlanan **Bir Akış Bulucu**ile ilişkili filtrelerin tanımını güncelleştirmek için tavsiye edilmez , özellikle CDN etkin olduğunda. Akış sunucuları ve CDN'leri, eski önbelleğe alınmış verilerin döndürülmesine neden olabilecek dahili önbelleklere sahip olabilir. 

Filtre tanımının değiştirilmesi gerekiyorsa, yeni bir filtre oluşturmayı ve **Akışlı Konum belirleyici** URL'ye eklemeyi veya filtreye doğrudan başvuran yeni bir **Akışlı Bulucu** yayımlamayı düşünün.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler, filtrelerin programlı olarak nasıl oluşturulacak larını gösterir.  

- [REST API'leri ile filtreler oluşturma](filters-dynamic-manifest-rest-howto.md)
- [.NET ile filtre oluşturma](filters-dynamic-manifest-dotnet-howto.md)
- [CLI ile filtre oluşturma](filters-dynamic-manifest-cli-howto.md)

