---
title: Azure Akışı Analizi tanı günlüğü veri hataları
description: Bu makalede, Azure Akış Analizi kullanılırken oluşabilecek farklı giriş ve çıktı veri hataları açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 0546464b4d1bcc9eaa4fbffe265486985d9c58f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75465035"
---
# <a name="azure-stream-analytics-data-errors"></a>Azure Akışı Analizi veri hataları

Veri hataları, verileri işlerken oluşan hatalardır.  Bu hatalar genellikle veri de-serialization, serileştirme ve yazma işlemleri sırasında oluşur.  Veri hataları oluştuğunda, Akış Analizi tanılama günlüklerine ayrıntılı bilgi ve örnek olaylar yazar.  Bazı durumlarda, bu bilgilerin özeti portal bildirimleri aracılığıyla da sağlanır.

Bu makalede, giriş ve çıktı veri hataları için farklı hata türleri, nedenleri ve tanılama günlüğü ayrıntıları özetler.

## <a name="diagnostic-log-schema"></a>Tanılama günlüğü şeması

Tanılama günlükleri için şemayı görmek için [tanılama günlüklerini kullanarak Sorun Giderme Azure Akış Analizi'ne](stream-analytics-job-diagnostic-logs.md#diagnostics-logs-schema) bakın. Aşağıdaki JSON, bir veri hatası için tanılama günlüğünün **Özellikler** alanı için örnek bir değerdir.

```json
{
    "Source": "InputTelemetryData",
    "Type": "DataError",
    "DataErrorType": "InputDeserializerError.InvalidData",
    "BriefMessage": "Json input stream should either be an array of objects or line separated objects. Found token type: Integer",
    "Message": "Input Message Id: https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt Error: Json input stream should either be an array of objects or line separated objects. Found token type: Integer",
    "ExampleEvents": "[\"1,2\\\\u000d\\\\u000a3,4\\\\u000d\\\\u000a5,6\"]",
    "FromTimestamp": "2019-03-22T22:34:18.5664937Z",
    "ToTimestamp": "2019-03-22T22:34:18.5965248Z",
    "EventCount": 1
}
```

## <a name="input-data-errors"></a>Giriş veri hataları

### <a name="inputdeserializererrorinvalidcompressiontype"></a>InputDeserializerError.InvalidCompressionType

* Neden: Seçilen giriş sıkıştırma türü verilerle eşleşmiyor.
* Portal bildirimi sağlandı: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Etki: Geçersiz sıkıştırma türü de dahil olmak üzere herhangi bir deserialization hataları olan iletiler girişten bırakılır.
* Günlük bilgileri
   * Giriş ileti tanımlayıcısı. Olay Hub'ı için tanımlayıcı PartitionId, Ofset ve Sıra Numarası'dır.

**Hata iletisi**

```json
"BriefMessage": "Unable to decompress events from resource 'https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt'. Please ensure compression setting fits the data being processed."
```

### <a name="inputdeserializererrorinvalidheader"></a>InputDeserializerError.InvalidHeader

* Neden: Giriş verilerinin üstbilgisi geçersizdir. Örneğin, bir CSV'de yinelenen adlara sahip sütunlar bulunur.
* Portal bildirimi sağlandı: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Etki: Geçersiz üstbilgi de dahil olmak üzere herhangi bir deserialization hataları olan iletiler girişten bırakılır.
* Günlük bilgileri
   * Giriş ileti tanımlayıcısı. 
   * Birkaç kilobayta kadar gerçek yük.

**Hata iletisi**

```json
"BriefMessage": "Invalid CSV Header for resource 'https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt'. Please make sure there are no duplicate field names."
```

### <a name="inputdeserializererrormissingcolumns"></a>InputDeserializerError.MissingColumns

* Neden: CREATE TABLE veya TIMESTAMP BY ile tanımlanan giriş sütunları yok.
* Portal bildirimi sağlandı: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Etki: Eksik sütunları olan olaylar girişten bırakılır.
* Günlük bilgileri
   * Giriş ileti tanımlayıcısı. 
   * Eksik olan sütunların adları. 
   * Birkaç kilobayta kadar gerçek yük.

**Hata iletileri**

```json
"BriefMessage": "Could not deserialize the input event(s) from resource 'https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt' as Csv. Some possible reasons: 1) Malformed events 2) Input source configured with incorrect serialization format" 
```

```json
"Message": "Missing fields specified in query or in create table. Fields expected:ColumnA Fields found:ColumnB"
```

### <a name="inputdeserializererrortypeconversionerror"></a>InputDeserializerError.TypeConversionError

* Neden: Girişin CREATE TABLE deyiminde belirtilen türe dönüştürülememesi.
* Portal bildirimi sağlandı: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Etki: Tür dönüştürme hatası olan olaylar girişten bırakılır.
* Günlük bilgileri
   * Giriş ileti tanımlayıcısı. 
   * Sütunun adı ve beklenen tür.

**Hata iletileri**

```json
"BriefMessage": "Could not deserialize the input event(s) from resource '''https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt ' as Csv. Some possible reasons: 1) Malformed events 2) Input source configured with incorrect serialization format" 
```

```json
"Message": "Unable to convert column: dateColumn to expected type."
```

### <a name="inputdeserializererrorinvaliddata"></a>InputDeserializerError.InvalidData

* Neden: Giriş verileri doğru biçimde değildir. Örneğin, giriş JSON geçerli değildir.
* Portal bildirimi sağlandı: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Etki: Geçersiz bir veri hatasıyla karşılaştıktan sonra iletideki tüm olaylar girişten bırakılır.
* Günlük bilgileri
   * Giriş ileti tanımlayıcısı. 
   * Birkaç kilobayta kadar gerçek yük.

**Hata iletileri**

```json
"BriefMessage": "Json input stream should either be an array of objects or line separated objects. Found token type: String"
```

```json
"Message": "Json input stream should either be an array of objects or line separated objects. Found token type: String"
```

### <a name="invalidinputtimestamp"></a>InvalidInputTimeStamp

* Neden: TIMESTAMP BY ifadesinin değeri datetime dönüştürülemez.
* Portal bildirimi sağlandı: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Etki: Geçersiz giriş zaman damgası olan olaylar girişten bırakılır.
* Günlük bilgileri
   * Giriş ileti tanımlayıcısı. 
   * Hata iletisi. 
   * Birkaç kilobayta kadar gerçek yük.

**Hata iletisi**

```json
"BriefMessage": "Unable to get timestamp for resource 'https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt ' due to error 'Cannot convert string to datetime'"
```

### <a name="invalidinputtimestampkey"></a>InvalidInputTimeStampKey

* Neden: TIMESTAMP BY OVER timestampColumn değeri NULL'dur.
* Portal bildirimi sağlandı: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Etki: Geçersiz giriş zaman damgası anahtarı olan olaylar girişten bırakılır.
* Günlük bilgileri
   * Gerçek yük birkaç kilobayta kadar.

**Hata iletisi**

```json
"BriefMessage": "Unable to get value of TIMESTAMP BY OVER COLUMN"
```

### <a name="lateinputevent"></a>LateInputOlay

* Neden: Başvuru süresi ile varış saati arasındaki fark, geç varış tolerans penceresinden daha fazladır.
* Portal bildirimi sağlandı: Hayır
* Tanılama günlüğü düzeyi: Bilgi
* Etki: Geç giriş olayları, iş yapılandırmasının Olay Sırası bölümündeki "Diğer olayları ele al" ayarına göre işlenir. Daha fazla bilgi için [Bkz. Zaman İşleme İlkeleri.](https://docs.microsoft.com/stream-analytics-query/time-skew-policies-azure-stream-analytics)
* Günlük bilgileri
   * Başvuru süresi ve varış saati. 
   * Birkaç kilobayta kadar gerçek yük.

**Hata iletisi**

```json
"BriefMessage": "Input event with application timestamp '2019-01-01' and arrival time '2019-01-02' was sent later than configured tolerance."
```

### <a name="earlyinputevent"></a>Early InputOlay

* Neden: Uygulama süresi ile Varış süresi arasındaki fark 5 dakikadan fazladır.
* Portal bildirimi sağlandı: Hayır
* Tanılama günlüğü düzeyi: Bilgi
* Etki: Erken giriş olayları iş yapılandırmasının Olay Sırası bölümündeki "Diğer olayları ele al" ayarına göre işlenir. Daha fazla bilgi için [Bkz. Zaman İşleme İlkeleri.](https://docs.microsoft.com/stream-analytics-query/time-skew-policies-azure-stream-analytics)
* Günlük bilgileri
   * Başvuru süresi ve varış saati. 
   * Birkaç kilobayta kadar gerçek yük.

**Hata iletisi**

```json
"BriefMessage": "Input event arrival time '2019-01-01' is earlier than input event application timestamp '2019-01-02' by more than 5 minutes."
```

### <a name="outoforderevent"></a>OutOfOrderOlay

* Neden: Olay tanımlanan sıra dışı tolerans penceresine göre sıra dışı olarak kabul edilir.
* Portal bildirimi sağlandı: Hayır
* Tanılama günlüğü düzeyi: Bilgi
* Etki: Sipariş dışı olaylar, iş yapılandırmasının Olay Sırası bölümündeki "Diğer olayları ele al" ayarına göre işlenir. Daha fazla bilgi için [Bkz. Zaman İşleme İlkeleri.](https://docs.microsoft.com/stream-analytics-query/time-skew-policies-azure-stream-analytics)
* Günlük bilgileri
   * Birkaç kilobayta kadar gerçek yük.

**Hata iletisi**

```json
"Message": "Out of order event(s) received."
```

## <a name="output-data-errors"></a>Çıktı veri hataları

### <a name="outputdataconversionerrorrequiredcolumnmissing"></a>OutputDataConversionError.RequiredColumnMissing

* Neden: Çıktı için gerekli sütun yok. Örneğin, Azure Tablo BölümüAnahtar olarak tanımlanan bir sütun yok.
* Portal bildirimi sağlandı: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Etki: Eksik gerekli sütun da dahil olmak üzere tüm çıktı veri dönüştürme hataları [Çıktı Veri Politikası](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-output-error-policy) ayarına göre işlenir.
* Günlük bilgileri
   * Sütunun adı ve kayıt tanımlayıcısı veya kaydın bir parçası.

**Hata iletisi**

```json
"Message": "The output record does not contain primary key property: [deviceId] Ensure the query output contains the column [deviceId] with a unique non-empty string less than '255' characters."
```

### <a name="outputdataconversionerrorcolumnnameinvalid"></a>OutputDataConversionError.ColumnNameGeçersiz

* Neden: Sütun değeri çıktıyla uyumlu değildir. Örneğin, sütun adı geçerli bir Azure tablo sütunu değildir.
* Portal bildirimi sağlandı: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Etki: Geçersiz sütun adı da dahil olmak üzere tüm çıktı veri dönüştürme hataları [Çıktı Veri Politikası](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-output-error-policy) ayarına göre işlenir.
* Günlük bilgileri
   * Sütunun adı ve kayıt tanımlayıcısı veya kaydın bir parçası.

**Hata iletisi**

```json
"Message": "Invalid property name #deviceIdValue. Please refer MSDN for Azure table property naming convention."
```

### <a name="outputdataconversionerrortypeconversionerror"></a>OutputDataConversionError.typeConversionError

* Neden: Bir sütun çıktıda geçerli bir türe dönüştürülemez. Örneğin, sütunun değeri SQL tablosunda tanımlanan kısıtlamalar veya türile uyumsuzdur.
* Portal bildirimi sağlandı: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Etki: Tür dönüştürme hatası da dahil olmak üzere tüm çıktı veri dönüştürme hataları [Çıktı Veri Politikası](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-output-error-policy) ayarına göre işlenir.
* Günlük bilgileri
   * Sütunun adı.
   * Kayıt tanımlayıcısı veya kaydın bir parçası.

**Hata iletisi**

```json
"Message": "The column [id] value null or its type is invalid. Ensure to provide a unique non-empty string less than '255' characters."
```

### <a name="outputdataconversionerrorrecordexceededsizelimit"></a>OutputDataConversionError.RecordExceededSizeLimit

* Neden: İletinin değeri desteklenen çıktı boyutundan büyüktür. Örneğin, bir olay hub çıkışı için bir kayıt 1 MB'dan büyüktür.
* Portal bildirimi sağlandı: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Etki: Kayıt aşıldığında boyut sınırı da dahil olmak üzere tüm çıktı veri dönüştürme [hataları, Çıktı Veri Politikası](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-output-error-policy) ayarına göre işlenir.
* Günlük bilgileri
   * Kayıt tanımlayıcısı veya kaydın bir parçası.

**Hata iletisi**

```json
"BriefMessage": "Single output event exceeds the maximum message size limit allowed (262144 bytes) by Event Hub."
```

### <a name="outputdataconversionerrorduplicatekey"></a>OutputDataConversionError.duplicatekey

* Neden: Kayıt zaten sistem sütunuyla aynı ada sahip bir sütun içerir. Örneğin, Kimlik sütunu farklı bir sütuna olduğunda Kimlik adlı bir sütunile CosmosDB çıkışı.
* Portal bildirimi sağlandı: Evet
* Tanılama günlüğü düzeyi: Uyarı
* Etki: Yinelenen anahtar da dahil olmak üzere tüm çıktı veri dönüştürme hataları [Çıktı Veri Politikası](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-output-error-policy) ayarına göre işlenir.
* Günlük bilgileri
   * Sütunun adı.
   * Kayıt tanımlayıcısı veya kaydın bir parçası.

```json
"BriefMessage": "Column 'devicePartitionKey' is being mapped to multiple columns."
```

## <a name="next-steps"></a>Sonraki adımlar

* [Tanılama günlüklerini kullanarak Azure Akış Analizi'ni sorun giderme](stream-analytics-job-diagnostic-logs.md)

* [Akış Analizi iş izlemeyi ve sorguları nasıl izleyebilirsiniz anlayın](stream-analytics-monitoring.md)
