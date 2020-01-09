---
title: Özel blob çıkış bölümlendirme Azure Stream Analytics
description: Bu makalede, Azure Stream Analytics işlerden BLOB depolama çıktısı için özel tarih/saat yol desenleri ve özel alan veya öznitelikler özellikleri açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/07/2019
ms.custom: seodec18
ms.openlocfilehash: e978771eaafafe4120f9eec802525c293fb9c7c9
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75426376"
---
# <a name="azure-stream-analytics-custom-blob-output-partitioning"></a>Özel blob çıkış bölümlendirme Azure Stream Analytics

Azure Stream Analytics özel alanlar veya öznitelikler ve özel tarih saat yolu desenleriyle özel blob çıkış bölümlendirme destekler. 

## <a name="custom-field-or-attributes"></a>Özel alan veya öznitelikler

Özel alan veya giriş öznitelikleri, çıkış üzerinde daha fazla denetime izin vererek aşağı akış veri işleme ve raporlama iş akışlarını geliştirir.

### <a name="partition-key-options"></a>Bölüm anahtarı seçenekleri

Giriş verilerini bölümlemek için kullanılan bölüm anahtarı veya sütun adı, kısa çizgi, alt çizgi ve boşluk içeren alfasayısal karakterler içerebilir. Diğer adlarla birlikte kullanılmamışsa, iç içe geçmiş alanları bölüm anahtarı olarak kullanmak mümkün değildir. Bölüm anahtarı NVARCHAR (MAX) olmalıdır.

### <a name="example"></a>Örnek

Bir işin, oturumları belirlemek için bir sütun **client_id** içeren bir dış video oyun hizmetine bağlı canlı kullanıcı oturumlarından giriş verileri aldığını varsayalım. Verileri **client_id**göre bölümlemek Için blob yol deseninin alanını, bir iş oluştururken blob çıkış özelliklerine bir **{client_id}** bölüm belirteci içerecek şekilde ayarlayın. Çeşitli **client_id** değerleri olan veriler Stream Analytics iş aracılığıyla akar, çıkış verileri her klasör için tek bir **client_id** değerine göre ayrı klasörlere kaydedilir.

![İstemci kimlikli yol deseninin](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-path-pattern-client-id.png)

Benzer şekilde, iş girişi her bir sensör **sensor_id**sahip milyonlarca sensörden veri algılayıcısı ise, her algılayıcı verisini farklı klasörlere bölümlemek Için yol deseninin **{sensor_id}** olması gerekir.  


REST API kullanarak, bu istek için kullanılan JSON dosyasının çıkış bölümü aşağıdaki gibi görünebilir:  

![REST API çıkışı](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-rest-output.png)

İş çalışmaya başladıktan sonra, *istemciler* kapsayıcısı aşağıdaki gibi görünebilir:  

![İstemci kapsayıcısı](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-clients-container.png)

Her bir dizin, her Blobun bir veya daha fazla kayıt içerdiği birden çok blob içerebilir. Yukarıdaki örnekte, "06000000" adlı klasörde bulunan ve aşağıdaki içeriklerle tek bir blob vardır:

![Blob içeriği](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-blob-contents.png)

Çıkış yolundaki çıktıyı bölümlemek için kullanılan sütun **client_id**olduğundan, blobdaki her kaydın klasör adıyla eşleşen bir **client_id** sütunu olduğunu unutmayın.

### <a name="limitations"></a>Sınırlamalar

1. Yol deseninin blob output özelliğinde yalnızca bir özel bölüm anahtarına izin verilir. Aşağıdaki yol desenlerinin tümü geçerlidir:

   * Cluster1/{Date}/{Afielınvomydata}  
   * Cluster1/{Time}/{Afielınvomydata}  
   * Cluster1/{Afielınvomydata}  
   * Cluster1/{Date}/{Time}/{Afielınvomydata} 
   
2. Bölüm anahtarları büyük/küçük harfe duyarlıdır, bu nedenle "John" ve "John" gibi bölüm anahtarları eşdeğerdir. Ayrıca, ifadeler bölüm anahtarı olarak kullanılamaz. Örneğin, **{columnA + columnB}** çalışmıyor.  

3. Bir giriş akışı 8000 altında bölüm anahtarı kardinalitesi olan kayıtlardan oluşuyorsa, kayıtlar mevcut bloblara eklenir ve gerektiğinde yalnızca yeni blob 'lar oluşturulur. Kardinalite 8000 üzerinde ise, var olan Blobların yazılacağı garantisi yoktur ve aynı bölüm anahtarına sahip rastgele sayıda kayıt için yeni Bloblar oluşturulmaz.

## <a name="custom-datetime-path-patterns"></a>Özel DateTime yol desenleri

Özel DateTime yol desenleri, Azure Stream Analytics, Azure HDInsight ve Azure Databricks için aşağı yönde işleme e veri göndermek üzere olanağı sağlayabilir, Hive akışı kuralları ile eşleşen bir çıktı biçimi belirtmenizi sağlar. Özel DateTime yol desenleri, kullanarak kolayca uygulanır `datetime` , biçim belirteci ile birlikte çıkış BLOB yolu ön eki alanındaki anahtar sözcük. Örneğin, `{datetime:yyyy}`.

### <a name="supported-tokens"></a>Desteklenen belirteçler

Aşağıdaki biçim belirticisi belirteçler, tek başına veya birlikte özel DateTime biçim elde etmek için kullanılabilir:

|Biçim belirleyicisi   |Açıklama   |Sonuçları örnek zaman 2018-01-02T10:06:08|
|----------|-----------|------------|
|{datetime:yyyy}|Dört basamaklı bir sayı olarak yıl|2018|
|{datetime:MM}|01 ile 12 ay|01|
|{datetime:M}|1 ile 12 ay|1|
|{datetime:dd}|01 ile 31 gün|02|
|{datetime:d}|1 ile 12 gün|2|
|{datetime:HH}|Saat 00 ile 23 arasında 24 saatlik biçim kullanılarak|10|
|{datetime:mm}|Dakika 00 ile 24 arasındaki sürümleri|06|
|{datetime:m}|0 dakika ile 24 arasındaki sürümleri|6|
|{datetime:ss}|00 60 saniye|08|

Özel DateTime desenleri kullanmak istemiyorsanız, {date} ekleyin ve/veya {yolu yerleşik DateTime biçimlerinden bir açılan listesi oluşturmak için ön eki için belirteci zaman}.

![Stream Analytics eski tarih/saat biçimleri](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-old-date-time-formats.png)

### <a name="extensibility-and-restrictions"></a>Genişletilebilirlik ve kısıtlamaları

Çok belirteçleri kullanabilirsiniz `{datetime:<specifier>}`yolu ön eki karakter sınırı ulaşana kadar yol deseni istediğiniz gibi. Tarih ve saat Açılır kutudan tarafından zaten listelenmiş birleşimleri ötesinde tek bir belirteç içindeki Biçim belirticileri birleştirilemez. 

Bir yol bölümü için `logs/MM/dd`:

|Geçerli deyim   |Geçersiz ifade   |
|----------|-----------|
|`logs/{datetime:MM}/{datetime:dd}`|`logs/{datetime:MM/dd}`|

Yol ön eki aynı biçim tanımlayıcısı birden çok kez kullanabilirsiniz. Belirteci her yinelenmesi gerekir.

### <a name="hive-streaming-conventions"></a>Hive akış kuralları

Blob depolama için özel yol desenleri ile Etiketlenecek klasörleri bekliyor Hive akış kuralı ile kullanılabilir `column=` klasörü adı.

Örneğin, `year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}/hour={datetime:HH}`.

Özel çıkış tabloları değiştirme ve bölümler Azure Stream Analytics Hive arasında bağlantı noktası verileri el ile ekleyerek ortadan ortadan kaldırır. Bunun yerine, birçok klasörleri kullanılarak otomatik olarak eklenebilir:

```SQL
MSCK REPAIR TABLE while hive.exec.dynamic.partition true
```

### <a name="example"></a>Örnek

Bir depolama hesabı, bir kaynak grubu, bir Stream Analytics işi ve bir giriş kaynağı göre oluşturma [Azure Stream Analytics, Azure portalında](stream-analytics-quick-create-portal.md) Hızlı Başlangıç Kılavuzu. Ayrıca kullanılabilir Hızlı Başlangıç Kılavuzu'nda kullanılan aynı örnek verileri kullanarak [GitHub](https://raw.githubusercontent.com/Azure/azure-stream-analytics/master/Samples/GettingStarted/HelloWorldASA-InputStream.json).

Aşağıdaki yapılandırmaya sahip bir blob çıkış havuzu oluşturun:

![Stream Analytics blob çıkış havuzu oluşturma](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-create-output-sink.png)

Tam yol deseni aşağıdaki gibidir:


`year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}`


Yol deseni temel alınarak bir klasör yapısı, işi başlattığınızda, blob kapsayıcısında oluşturulur. Günlük düzeyi kadar detaya gidebilirsiniz.

![Stream Analytics blob çıktı özel yol deseni](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-blob-output-folder-structure.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics çıkışları anlama](stream-analytics-define-outputs.md)
