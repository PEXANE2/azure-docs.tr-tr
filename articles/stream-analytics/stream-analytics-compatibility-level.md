---
title: Azure Akışı Analizi uyumluluk düzeyleri
description: Azure Akış Analizi işi ve en son uyumluluk düzeyindeki önemli değişiklikler için uyumluluk düzeyi ayarlamayı öğrenin
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 8f22b1ff97826dc318794aca58973b1276e74209
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79087864"
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Azure Akış Analizi işleri için uyumluluk düzeyi

Bu makalede, Azure Akış Analizi'nde uyumluluk düzeyi seçeneği açıklanmaktadır. Stream Analytics, düzenli özellik güncelleştirmeleri ve performans iyileştirmeleri ile yönetilen bir hizmettir. Hizmetin çalışma saatleri güncelleştirmelerinin çoğu son kullanıcılar tarafından otomatik olarak kullanıma sunulmuştur. 

Ancak, hizmetteki bazı yeni işlevler, varolan bir işin davranışında değişiklik veya çalışan işlerde verilerin tüketilme şeklindedeğişiklik gibi önemli bir değişikliğe neden olabilir. Uyumluluk düzeyi ayarını düşürerek, mevcut Akış Analizi işlerinizi büyük değişiklikler olmadan çalışır durumda tutabilirsiniz. En son çalışma zamanı davranışlarına hazır olduğunuzda, uyumluluk düzeyini yükselterek kabul edebilirsiniz. 

## <a name="choose-a-compatibility-level"></a>Uyumluluk düzeyi seçin

Uyumluluk düzeyi, akış analizi işinin çalışma zamanı davranışını denetler. 

Azure Akış Analizi şu anda üç uyumluluk düzeyi destekler:

* 1.0 - Azure Akış Analizi'nin birkaç yıl önce genel kullanılabilirliği sırasında tanıtılan orijinal uyumluluk düzeyi.
* 1.1 - Önceki davranış
* 1.2 - En son iyileştirmeler ile yeni davranış

Yeni bir Akış Analizi işi oluşturduğunuzda, en son uyumluluk düzeyini kullanarak bu işi oluşturmak en iyi yöntemdir. Daha sonra ek değişiklik ve karmaşıklığı önlemek için, en son davranışlara güvenerek iş tasarımınızı başlatın.

## <a name="set-the-compatibility-level"></a>Uyumluluk düzeyini ayarlama

Azure portalındaki bir Akış Analizi işinin uyumluluk düzeyini veya [create job REST API çağrısını](/rest/api/streamanalytics/stream-analytics-job)kullanarak ayarlayabilirsiniz.

Azure portalındaki işin uyumluluk düzeyini güncelleştirmek için:

1. Akış Analizi işinizi bulmak için [Azure portalını](https://portal.azure.com) kullanın.
2. Uyumluluk düzeyini güncelleştirmeden önce işi **durdurun.** İşiniz çalışıyorsa uyumluluk düzeyini güncelleştiremezsiniz.
3. **Yapılandırma** başlığı altında **Uyumluluk düzeyini**seçin.
4. İstediğiniz uyumluluk düzeyi değerini seçin.
5. Sayfanın altındaki **Kaydet'i** seçin.

![Azure portalında Akış Analizi uyumluluk düzeyi](media/stream-analytics-compatibility-level/stream-analytics-compatibility.png)

Uyumluluk düzeyini güncelleştirdiğinizde, T-derleyici silecek dizini seçili uyumluluk düzeyine karşılık gelen işi doğrular.

## <a name="compatibility-level-12"></a>Uyumluluk düzeyi 1.2

Aşağıdaki büyük değişiklikler uyumluluk düzeyi 1.2 tanıtıldı:

###  <a name="amqp-messaging-protocol"></a>AMQP mesajlaşma protokolü

**1.2 düzeyi**: Azure Akışı Analytics, Servis Veri Hizmetleri Kuyrukları ve Konuları'na yazmak için [Gelişmiş İleti Kuyruk Protokolü (AMQP)](../service-bus-messaging/service-bus-amqp-overview.md) ileti protokolünü kullanır. AMQP, açık standart bir protokol kullanarak çapraz platform, karma uygulamalar oluşturmanıza olanak tanır.

### <a name="geospatial-functions"></a>Jeo-uzamsal işlevler

**Önceki düzeyler:** Azure Akış Analizi, Coğrafya hesaplamalarını kullandı.

**1.2 seviyesi:** Azure Akış Analizi, Geometrik öngörülen coğrafi koordinatları hesaplamanızı sağlar. Jeouzamsal fonksiyonların imzasında bir değişiklik yok. Ancak, onların semantik biraz farklı, eskisinden daha kesin hesaplama sağlayan.

Azure Akış Analizi, coğrafi başvuru veri dizini oluşturmayı destekler. Daha hızlı bir birleştirme hesaplaması için jeouzamsal öğeleri içeren Başvuru Verileri dizinlenebilir.

Güncellenen jeouzamsal işlevler, Bilinen Metin (WKT) jeouzamsal biçiminin tam ifade sini getirir. GeoJson ile daha önce desteklenmeyen diğer coğrafi bileşenleri belirtebilirsiniz.

Daha fazla bilgi için Azure [Akış Analizi – Bulut ve IoT Edge'deki coğrafi özelliklergüncelleştirmeleri](https://azure.microsoft.com/blog/updates-to-geospatial-functions-in-azure-stream-analytics-cloud-and-iot-edge/)bölümüne bakın.

### <a name="parallel-query-execution-for-input-sources-with-multiple-partitions"></a>Birden çok bölümlü giriş kaynakları için paralel sorgu yürütme

**Önceki düzeyler:** Azure Akış Analizi sorguları, sorgu işlemini giriş kaynağı bölümleri arasında paralelleştirmek için PARTITION BY yan tümcesinin kullanılmasını gerektiriyor.

**1.2 seviyesi:** Sorgu mantığı giriş kaynağı bölümleri arasında paralelleştirilebiliyorsa, Azure Akış Analizi ayrı sorgu örnekleri oluşturur ve hesaplamaları paralel olarak çalıştırAr.

### <a name="native-bulk-api-integration-with-cosmosdb-output"></a>CosmosDB çıkışı ile Yerel Toplu API entegrasyonu

**Önceki düzeyler:** Yükseltme davranışı *ekleme veya birleştirme*oldu.

**1.2 seviyesi:** CosmosDB çıkışıyla yerel Toplu API tümleştirmesi verimi en üst düzeye çıkarır ve azaltma isteklerini verimli bir şekilde işler. Daha fazla bilgi için [Azure Cosmos DB sayfasına Azure Akış Analizi çıktısı bölümüne](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-documentdb-output#improved-throughput-with-compatibility-level-12)bakın.

Yükseltme davranışı *ekle veya değiştir.*

### <a name="datetimeoffset-when-writing-to-sql-output"></a>SQL çıktısına yazarken DateTimeOffset

**Önceki düzeyler:** [DateTimeOffset](https://docs.microsoft.com/sql/t-sql/data-types/datetimeoffset-transact-sql?view=sql-server-2017) türleri UTC'ye ayarlandı.

**1.2 seviyesi:** DateTimeOffset artık ayarlanmaz.

### <a name="long-when-writing-to-sql-output"></a>SQL çıktısına yazarken uzun

**Önceki düzeyler:** Değerler hedef türüne göre kesildi.

**1.2 seviyesi:** Hedef türüne sığmayan değerler çıktı hata ilkesine göre işlenir.

### <a name="record-and-array-serialization-when-writing-to-sql-output"></a>SQL çıktısına yazarken kayıt ve dizi serileştirme

**Önceki düzeyler:** Kayıtlar "Kayıt" olarak, diziler ise "Dizi" olarak yazılmıştır.

**1.2 seviyesi:** Kayıtlar ve diziler JSON biçiminde seri hale getirilir.

### <a name="strict-validation-of-prefix-of-functions"></a>Fonksiyonların önekinin kesin doğrulanması

**Önceki düzeyler:** İşlev öneklerinin kesin bir doğrulaması yoktu.

**1.2 seviyesi:** Azure Akış Analizi, işlev öneklerinin sıkı bir doğrulaması vardır. Yerleşik bir işleve önek eklemek hataya neden olur. Örneğin,`myprefix.ABS(…)` desteklenmez.

Yerleşik toplamlara bir önek eklemek de hatayla sonuçlanır. Örneğin, `myprefix.SUM(…)` desteklenmez.

Kullanıcı tarafından tanımlanan işlevler için "sistem" önekinin kullanılması hatayla sonuçlanır.

### <a name="disallow-array-and-object-as-key-properties-in-cosmos-db-output-adapter"></a>Cosmos DB çıkış bağdaştırıcısında anahtar özellikler olarak Dizi ve Nesneyi İzin Vermeme

**Önceki düzeyler:** Dizi ve Nesne türleri önemli bir özellik olarak desteklendi.

**1.2 seviyesi:** Dizi ve Nesne türleri artık önemli bir özellik olarak desteklenmez.

## <a name="compatibility-level-11"></a>Uyumluluk düzeyi 1.1

Aşağıdaki büyük değişiklikler uyumluluk düzeyi 1.1 tanıtıldı:

### <a name="service-bus-xml-format"></a>Servis Veri Servisi XML formatı

**1.0 seviyesi:** Azure Stream Analytics DataContractSerializer'ı kullandığından, ileti içeriği nde XML etiketleri yer aldı. Örnek:

`@\u0006string\b3http://schemas.microsoft.com/2003/10/Serialization/\u0001{ "SensorId":"1", "Temperature":64\}\u0001`

**1.1 seviye:** İleti içeriği, akışı doğrudan ek etiket içermeyen içerir. Örneğin, `{ "SensorId":"1", "Temperature":64}`

### <a name="persisting-case-sensitivity-for-field-names"></a>Alan adları için kalıcı durum duyarlılığı

**1.0 seviyesi:** Alan adları, Azure Akışı Analytics altyapısı tarafından işlendiğinde küçük harf olarak değiştirildi.

**1.1 düzeyi:** Azure Akışı Analytics motoru tarafından işlendiğinde alan adları için büyük/küçük harf duyarlılığı kalıcıdır.

> [!NOTE]
> Edge ortamı kullanılarak barındırılan Stream Analytic işleri için kalıcı servis talebi duyarlılığı henüz mevcut değildir. Sonuç olarak, işiniz Edge'de barındırılıyorsa, tüm alan adları küçük harfe dönüştürülür.

### <a name="floatnandeserializationdisabled"></a>FloatNaNDeserializationDevre Dışı

**1.0 seviyesi:** CREATE TABLE komutu NaN (Not-a-Number) ile olayları filtrelemedi. Örneğin, Sonsuzluk, -Infinity) float sütun türünde, çünkü bu sayılar için belgelenmiş aralık dışındadır.

**1.1 seviye:** CREATE TABLE güçlü bir şema belirtmenizi sağlar. Akış Analizi motoru, verilerin bu şemaya uygun olduğunu doğrular. Bu modelle, komut NaN değerleri ile olayları filtreleyebilir.

### <a name="disable-automatic-upcast-for-datetime-strings-in-json"></a>JSON'daki datetime dizeleri için otomatik veyazamanı devre dışı devre dışı

**1.0 seviyesi:** JSON parser otomatik olarak DateTime türüne tarih/ saat/ bölge bilgileri ile dize değerleri upcast ve sonra UTC dönüştürecek. Bu davranış, saat dilimi bilgilerinin kaybedilesonuçlandı.

**1.1 seviye:** DateTime türüne tarih/saat/bölge bilgileri yle dize değerlerinin otomatik olarak yer değiştirmesi yoktur. Sonuç olarak, saat dilimi bilgileri tutulur.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Akış Analizi girişlerini giderme](stream-analytics-troubleshoot-input.md)
* [Akış Analizi Kaynak durumu](stream-analytics-resource-health.md)
