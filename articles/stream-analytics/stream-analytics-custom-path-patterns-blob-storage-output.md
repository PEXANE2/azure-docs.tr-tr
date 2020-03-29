---
title: Azure Stream Analytics özel blob çıktı bölümleme
description: Bu makalede, Azure Akış Analizi işlerinden blob depolama çıktısı için özel DateTime yol desenleri ve özel alan veya öznitelikler özellikleri açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/07/2019
ms.custom: seodec18
ms.openlocfilehash: e978771eaafafe4120f9eec802525c293fb9c7c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75426376"
---
# <a name="azure-stream-analytics-custom-blob-output-partitioning"></a>Azure Stream Analytics özel blob çıktı bölümleme

Azure Akış Analizi, özel alanlar veya öznitelikler ve özel DateTime yol desenleri ile özel blob çıktı bölümleme destekler. 

## <a name="custom-field-or-attributes"></a>Özel alan veya öznitelikler

Özel alan veya giriş öznitelikleri, çıktı üzerinde daha fazla denetim sağlayarak akış aşağı veri işleme ve raporlama iş akışlarını geliştirir.

### <a name="partition-key-options"></a>Bölme anahtar seçenekleri

Giriş verilerini bölmek için kullanılan bölüm anahtarı veya sütun adı tire, alt çizgi ve boşluk içeren alfasayısal karakterler içerebilir. Takma adlarla birlikte kullanılmadığı sürece iç içe geçen alanları bölme anahtarı olarak kullanmak mümkün değildir. Bölüm anahtarı NVARCHAR(MAX) olmalıdır.

### <a name="example"></a>Örnek

Bir işin, yutulan verilerin oturumları tanımlamak için bir sütun **client_id** içerdiği harici bir video oyunu hizmetine bağlı canlı kullanıcı oturumlarından giriş verilerini aldığını varsayalım. Verileri **client_id**bölmek için, blob yol deseni alanını, iş oluştururken blob çıktı sıcağa **{client_id}** bölüm belirteci içerecek şekilde ayarlayın. Çeşitli client_id **değerlerine** sahip veriler Akış Analizi işinden aktığından, çıktı verileri klasör başına tek bir **client_id** değerine dayalı olarak ayrı klasörlere kaydedilir.

![İstemci kimliği ile yol deseni](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-path-pattern-client-id.png)

Benzer şekilde, iş girişi her sensörün **sensor_id**sahip olduğu milyonlarca sensörden gelen sensör verileri yse, Her sensör verilerini farklı klasörlere bölmek için Yol Deseni **{sensor_id}** olacaktır.  


REST API'yi kullanarak, bu istek için kullanılan bir JSON dosyasının çıktı bölümü aşağıdaki gibi görünebilir:  

![REST API çıkışı](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-rest-output.png)

İş çalışmaya başladıktan sonra, *istemci* kapsayıcısı aşağıdaki gibi görünebilir:  

![İstemci konteyner](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-clients-container.png)

Her klasör, her blob bir veya daha fazla kayıt içeren birden çok blobiçerebilir. Yukarıdaki örnekte, "0600000" etiketli bir klasörde aşağıdaki içerikleri içeren tek bir leke vardır:

![Blob içeriği](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-blob-contents.png)

Çıktı yolunda çıktıyı bölmek için kullanılan sütun **client_id**olduğundan, blob'daki her kaydın klasör adıyla eşleşen **bir client_id** sütunu olduğuna dikkat edin.

### <a name="limitations"></a>Sınırlamalar

1. Yol Deseni blob çıkış özelliğinde yalnızca bir özel bölüm anahtarına izin verilir. Aşağıdaki Yol Desenleri geçerlidir:

   * cluster1/{date}/{aFieldInMyData}  
   * cluster1/{time}/{aFieldInMyData}  
   * cluster1/{aFieldInMyData}  
   * cluster1/{date}/{time}/{aFieldInMyData} 
   
2. Bölüm tuşları büyük/küçük harf duyarsızdır, bu nedenle "John" ve "john" gibi bölüm tuşları eşdeğerdir. Ayrıca, ifadeler bölüm anahtarları olarak kullanılamaz. Örneğin, **{columnA + columnB}** çalışmıyor.  

3. Bir giriş akışı 8000'in altında bir bölüm anahtarlığına sahip kayıtlardan oluştuğunda, kayıtlar varolan blob'lara eklenir ve yalnızca gerektiğinde yeni lekeler oluşturur. Kardinallik 8000'in üzerindeyse, varolan lekelerin yazılacağına ve aynı bölüm anahtarına sahip rasgele sayıda kayıt için yeni lekeler oluşturulacağının garantisi yoktur.

## <a name="custom-datetime-path-patterns"></a>Özel DateTime yol desenleri

Özel DateTime yol desenleri, Azure Akış Analizi'ne akış aşağı işlemleri için Azure HDInsight ve Azure Databricks'e veri gönderme olanağı sağlayarak Hive Streaming kurallarıyla uyumlu bir çıktı biçimi belirtmenize olanak tanır. Özel DateTime yol desenleri, `datetime` blob çıktınızın Yol Öneki alanındaki anahtar sözcüğü ve biçim belirteci kullanılarak kolayca uygulanır. Örneğin, `{datetime:yyyy}`.

### <a name="supported-tokens"></a>Desteklenen belirteçler

Aşağıdaki biçim belirtici belirtici belirteçleri tek başına veya özel DateTime biçimleri elde etmek için birlikte kullanılabilir:

|Biçim belirteci   |Açıklama   |Örnek zaman sonuçları 2018-01-02T10:06:08|
|----------|-----------|------------|
|{datetime:yyyy}|Dört basamaklı sayı olarak yıl|2018|
|{datetime:MM}|01 ile 12 arasında ay|01|
|{datetime:M}|1 ile 12 arasında ay|1|
|{datetime:dd}|01'den 31'e kadar olan gün|02|
|{datetime:d}|1 ile 12 arasında gün|2|
|{datetime:HH}|00'dan 23'e kadar 24 saat biçimini kullanma saati|10|
|{datetime:mm}|00-24 arası dakikalar|06|
|{datetime:m}|0 ile 24 arası dakikalar|6|
|{datetime:ss}|00 ile 60 arasında saniye|08|

Özel DateTime desenleri kullanmak istemiyorsanız, yerleşik DateTime biçimlerine sahip bir açılır bırakma oluşturmak için Yol Öneki'ne {date} ve/veya {time} belirteci ekleyebilirsiniz.

![Akış Analizi eski DateTime biçimleri](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-old-date-time-formats.png)

### <a name="extensibility-and-restrictions"></a>Genişletilebilirlik ve kısıtlamalar

Yol Öneki karakter sınırına `{datetime:<specifier>}`ulaşana kadar yol deseninde istediğiniz kadar belirteç kullanabilirsiniz. Biçim belirteçleri, tarih ve saat açılırlarına göre listelenen kombinasyonların dışında tek bir belirteç içinde birleştirilemez. 

Bir yol bölümü `logs/MM/dd`için:

|Geçerli ifade   |Geçersiz ifade   |
|----------|-----------|
|`logs/{datetime:MM}/{datetime:dd}`|`logs/{datetime:MM/dd}`|

Yol Öneki'nde aynı biçim belirteci birden çok kez kullanabilirsiniz. Belirteç her seferinde tekrarlanmalıdır.

### <a name="hive-streaming-conventions"></a>Kovan Akış kuralları

Blob depolama için özel yol desenleri, klasörlerin klasör adı içinde etiketlenmesini bekleyen Kovan Akış kuralıyla `column=` kullanılabilir.

Örneğin, `year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}/hour={datetime:HH}`.

Özel çıktı, Azure Akış Analizi ve Hive arasındaki bağlantı noktası verilerine tabloları değiştirme ve el ile bölüm ekleme sorununu ortadan kaldırır. Bunun yerine, birçok klasör otomatik olarak kullanılabilir:

```SQL
MSCK REPAIR TABLE while hive.exec.dynamic.partition true
```

### <a name="example"></a>Örnek

[Azure Akış Analytics Azure Portalı](stream-analytics-quick-create-portal.md) hızlı başlangıç kılavuzuna göre bir depolama hesabı, bir kaynak grubu, Bir Akış Analizi işi ve bir giriş kaynağı oluşturun. [GitHub'da](https://raw.githubusercontent.com/Azure/azure-stream-analytics/master/Samples/GettingStarted/HelloWorldASA-InputStream.json)da bulunan hızlı başlangıç kılavuzunda kullanılan aynı örnek verileri kullanın.

Aşağıdaki yapılandırmaile bir blob çıkış lavabosu oluşturun:

![Stream Analytics blob çıkış lavabo oluşturmak](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-create-output-sink.png)

Tam yol deseni aşağıdaki gibidir:


`year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}`


İşi başlattığınızda, blob kapsayıcınızda yol desenini temel alan bir klasör yapısı oluşturulur. Gün seviyesine inebilirsiniz.

![Özel yol deseni ile Stream Analytics blob çıkışı](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-blob-output-folder-structure.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Akış Analitiği'nden çıktıları anlama](stream-analytics-define-outputs.md)
