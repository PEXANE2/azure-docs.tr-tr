---
title: Azure Akış Analizi JavaScript kullanıcı tanımlı işlevler
description: Bu makale, Stream Analytics'te JavaScript kullanıcı tanımlı işlevlerine giriştir.
author: rodrigoaatmicrosoft
ms.author: rodrigoa
ms.service: stream-analytics
ms.topic: tutorial
ms.reviewer: mamccrea
ms.custom: mvc
ms.date: 03/23/2020
ms.openlocfilehash: 58d750b47f3f6a2bcfbf23399ca249131e7876ae
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80235380"
---
# <a name="javascript-user-defined-functions-in-azure-stream-analytics"></a>Azure Akış Analizi'nde JavaScript kullanıcı tanımlı işlevler
 
Azure Stream Analytics, JavaScript dilinde yazılmış kullanıcı tanımlı işlevleri destekler. JavaScript’in sağladığı **String**, **RegExp**, **Math**, **Array** ve **Date** yöntemlerinden oluşan zengin küme sayesinde Stream Analytics işleriyle karmaşık veri dönüşümlerini oluşturmak daha kolay hale gelir.

## <a name="overview"></a>Genel Bakış

JavaScript kullanıcı tanımlı işlevleri, dış bağlantı gerektirmeyen, yalnızca bilgi işlem den gelen skaler işlevleri destekler. Bir işlevin dönüş değeri yalnızca skaler (tek) değer olabilir. JavaScript kullanıcı tanımlı işlevini bir işe ekledikten sonra işlevi, yerleşik bir skaler değerli işlev gibi sorgunun herhangi bir yerinde kullanabilirsiniz.

JavaScript kullanıcı tanımlı işlevlerini yararlı bulabileceğiniz bazı senaryolar aşağıda verilmiştir:
* Normal ifade işlevlerine sahip dizeleri ayrıştırma ve düzenleme; örneğin, **Regexp_Replace()** and **Regexp_Extract()**
* İkili-onaltılık dönüşüm gibi verilerin kodunu çözme ve kodlama
* JavaScript **Math** işlevleri ile matematik hesaplamaları yapma
* Sıralama, birleştirme, bulma ve doldurma gibi dizi işlemleri yapma

Stream Analytics'te JavaScript kullanıcı tanımlı bir işlevle yapamayacağınız bazı şeyler şunlardır:
* Örneğin, harici REST uç noktalarını çağırın, ters IP araması yapmak veya dış kaynaktan referans verileri çekme
* Girdiler/çıktılar üzerinde özel durum serileştirme veya seri durumdan çıkarma işlemi gerçekleştirme
* Özel toplam değerler oluşturma

**Date.GetDate()** veya **Math.random()** gibi işlevler işlev tanımında engellenmese de, bunları kullanmaktan kaçınmalısınız. Bu işlevler her aradığınızda aynı sonucu **döndürmez** ve Azure Akış Analizi hizmeti işlev çağrıları ve döndürülen sonuçlar dan oluşan bir günlük tutmaz. Bir işlev aynı olaylarda farklı bir sonuç döndürürse, bir iş sizin veya Stream Analytics hizmeti tarafından yeniden başlatıldığında yinelenebilirlik garanti etmez.

## <a name="add-a-javascript-user-defined-function-to-your-job"></a>İşinize JavaScript kullanıcı tanımlı işlev ekleme

> [!NOTE]
> Bu adımlar, bulutta çalışacak şekilde yapılandırılan Akış Analizi işlerinde çalışır. Akış Analizi işiniz Azure IoT Edge'de çalışacak şekilde yapılandırıldıysa, bunun yerine Visual Studio'yu kullanın ve [C# kullanarak kullanıcı tanımlı işlevi yazın.](stream-analytics-edge-csharp-udf.md)

Akış Analizi işinizde JavaScript kullanıcı tanımlı bir işlev oluşturmak için **İş Topolojisi**altında **İş Fonksiyonları'nı** seçin. Ardından, **+Ekle** açılır menüsünden **JavaScript UDF'yi** seçin. 

![JavaScript UDF ekle](./media/javascript/stream-analytics-jsudf-add.png)

Daha sonra aşağıdaki özellikleri sağlamanız ve **Kaydet'i**seçmeniz gerekir.

|Özellik|Açıklama|
|--------|-----------|
|İşlev takma adı|Sorgunuzdaki işlevi çağırmak için bir ad girin.|
|Çıkış türü|JavaScript kullanıcı tanımlı işleviniz tarafından Stream Analytics sorgunuza döndürülecek yazın.|
|Fonksiyon tanımı|UdF'niz sorgunuzdan her çağrıldığında yürütülecek JavaScript işlevinizin uygulanması.|

## <a name="test-and-troubleshoot-javascript-udfs"></a>JavaScript UDF'leri test edin ve sorun giderme 

JavaScript UDF mantığınızı herhangi bir tarayıcıda test edebilir ve hata ayıklayabilirsiniz. Bu kullanıcı tanımlı işlevlerin mantığını hata ayıklama ve sınama şu anda Stream Analytics portalında desteklenmemektedir. İşlev beklendiği gibi çalıştığında, yukarıda belirtildiği gibi Akış Analizi işine ekleyebilir ve doğrudan sorgunuzdan çağırabilirsiniz. [Visual Studio için Stream Analytics araçlarını](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-install)kullanarak javascript UDF ile sorgu mantığınızı test edebilirsiniz.

JavaScript çalışma zamanı hataları önemli kabul edilir ve Etkinlik günlüğünde öne çıkarılır. Günlüğü almak için Azure portalında işinize gidin ve **Etkinlik günlüğü**’nü seçin.

## <a name="call-a-javascript-user-defined-function-in-a-query"></a>Bir sorguda JavaScript kullanıcı tanımlı işlevi çağırma

**Udf**ile önceden belirlenmiş işlev diğer adını kullanarak sorgunuzdaki JavaScript işlevini kolayca çağırabilirsiniz. Aşağıda, bir Akış Analizi sorgusunda çağrılan tamsayıya hexadecimal değerleri dönüştüren bir JavaScript UDF örneği verilmiştir.

```SQL
    SELECT
        time,
        UDF.hex2Int(offset) AS IntOffset
    INTO
        output
    FROM
        InputStream
```

## <a name="supported-javascript-objects"></a>Desteklenen JavaScript nesneleri

Azure Stream Analytics JavaScript kullanıcı tanımlı işlevleri standart, yerleşik JavaScript nesnelerini destekler. Bu nesnelerin bir listesi için bkz. [Genel Nesneler](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

### <a name="stream-analytics-and-javascript-type-conversion"></a>Stream Analytics ve JavaScript tür dönüşümü

Stream Analytics sorgu dili ile JavaScript’in desteklediği türler arasında farklılıklar vardır. Bu tabloda ikisi arasındaki dönüştürme eşlemeleri listelenmektedir:

Stream Analytics | JavaScript
--- | ---
bigint | Sayı (JavaScript yalnızca tam olarak 2^53’e kadar tamsayıları temsil edebilir)
DateTime | Tarih (JavaScript yalnızca milisaniye birimini destekler)
double | Sayı
nvarchar(MAX) | Dize
Kayıt | Nesne
Dizi | Dizi
NULL | Null

JavaScript’ten Stream Analytics’e dönüşümler aşağıda verilmiştir:

JavaScript | Stream Analytics
--- | ---
Sayı | Bigint (sayı yuvarlak ve long.MinValue ile long.MaxValue arasındaysa; aksi takdirde iki katıdır)
Tarih | DateTime
Dize | nvarchar(MAX)
Nesne | Kayıt
Dizi | Dizi
Null, Tanımsız | NULL
Başka bir tür (örneğin, bir işlev veya hata) | Desteklenmiyor (çalışma zamanı hatası ile sonuçlanır)

JavaScript dili büyük/küçük harf duyarlıdır ve JavaScript kodundaki nesne alanlarının gövdesi, gelen verilerdeki alanların kasası ile eşleşmelidir. Uyumluluk düzeyi 1.0 olan işler, alanları SQL SELECT deyiminden küçük harfe dönüştürür. Uyumluluk düzeyi 1.1 ve üzeri altında, SELECT deyimindeki alanlar SQL sorgusunda belirtilen kasayla aynı kasaya sahip olacaktır.

## <a name="other-javascript-user-defined-function-patterns"></a>Diğer JavaScript kullanıcı tanımlı işlev desenleri

### <a name="write-nested-json-to-output"></a>Çıktıya iç içe geçmiş JSON yazma

Girdi olarak bir Stream Analytics iş çıktısını kullanan bir takip işleme adımınız varsa ve bir JSON biçimi gerektiriyorsa, çıktıya bir JSON dizesi yazabilirsiniz. Aşağıdaki örnekte girdinin tüm ad/değer çiftlerini paketlemek ve sonra bunları çıktıda tek bir dize olarak yazmak için **JSON.stringify()** işlevi çağrılmaktadır.

**JavaScript kullanıcı tanımlı işlev tanımı:**

```javascript
function main(x) {
return JSON.stringify(x);
}
```

**Örnek sorgu:**
```SQL
SELECT
    DataString,
    DataValue,
    HexValue,
    UDF.jsonstringify(input) As InputEvent
INTO
    output
FROM
    input PARTITION BY PARTITIONID
```

## <a name="next-steps"></a>Sonraki adımlar

* [Makine Öğrenimi UDF](https://docs.microsoft.com/azure/stream-analytics/machine-learning-udf)
* [C# UDF](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-edge-csharp-udf-methods)
