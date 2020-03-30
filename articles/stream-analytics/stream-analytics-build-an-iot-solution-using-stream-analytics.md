---
title: Azure Akış Analizi'ni kullanarak bir IoT çözümü oluşturun
description: Gişe senaryosunun Stream Analytics IoT çözümü için eğitime başlama
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: f506cc526a824d45ae2d6b7a75e1c1a99dae4d64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75426452"
---
# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Stream Analytics'i kullanarak bir IoT çözümü oluşturun

## <a name="introduction"></a>Giriş
Bu çözümde, verilerinizden gerçek zamanlı öngörüler elde etmek için Azure Akış Analizi'ni nasıl kullanacağınızı öğrenirsiniz. Geliştiriciler, iş öngörüleri elde etmek için geçmiş kayıtları veya başvuru verileriyle tıklama akışları, günlükler ve aygıt tarafından oluşturulan olaylar gibi veri akışlarını kolayca birleştirebilir. Azure Akış Analizi, Microsoft Azure'da barındırılan tam olarak yönetilen, gerçek zamanlı akış hesaplama hizmeti olarak, dakikalar içinde çalışmaya devam edebilmeniz için yerleşik esneklik, düşük gecikme süresi ve ölçeklenebilirlik sağlar.

Bu çözümü tamamladıktan sonra şunları yapabileceksiniz:

* Azure Akış Analizi portalı hakkında bilgi edinin.
* Akış işini yapılandırın ve dağıtın.
* Gerçek dünyadaki sorunları ifade edin ve Stream Analytics sorgu dilini kullanarak bunları çözün.
* Stream Analytics'i güvenle kullanarak müşterileriniz için akış çözümleri geliştirin.
* Sorunları gidermek için izleme ve günlüğe kaydetme deneyimini kullanın.

## <a name="prerequisites"></a>Ön koşullar
Bu çözümü tamamlamak için aşağıdaki ön koşullara ihtiyacınız vardır:
* [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/)

## <a name="scenario-introduction-hello-toll"></a>Senaryo giriş: "Merhaba, Toll!"
Bir gişe yaygın bir fenomendir. Onlarla dünya çapında birçok otoyolda, köprüde ve tünelde karşılaşırsınız. Her gişede birden fazla gişe vardır. Manuel kabinlerde, bir görevliye geçiş ücretini ödemek için durursunuz. Otomatik kabinlerde, her kabinin üstündeki sensör, gişeden geçerken aracınızın ön camına yapıştırılmış bir RFID kartı tarar. Araçların bu gişelerden geçişini, ilginç operasyonların yapıldığı bir olay akışı olarak görselleştirmek kolaydır.

![Gişelerde araba resmi](media/stream-analytics-build-an-iot-solution-using-stream-analytics/cars-in-toll-booth.jpg)

## <a name="incoming-data"></a>Gelen veriler
Bu çözüm, iki veri akışıyla çalışır. Gişelerin giriş ve çıkışına yerleştirilen sensörler ilk akışı üretiyor. İkinci akış, araç kayıt verilerine sahip statik bir arama veri kümesidir.

### <a name="entry-data-stream"></a>Giriş veri akışı
Giriş veri akışı, otoyol istasyonlarına girerken otomobiller hakkında bilgi içerir. Çıkış veri solayları, örnek uygulamaya dahil olan bir Web Uygulamasından Olay Hub kuyruğuna canlı olarak aktarılır.

| TollID | Giriş Zamanı | Ruhsat Plakası | Durum | Marka | Model | Araç Tipi | Araç Ağırlığı | Ücretli | Etiket |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 |2014-09-10 12:01:00.000 |JNB 7001 |NY |Honda |Crv |1 |0 |7 | |
| 1 |2014-09-10 12:02:00.000 |YXZ 1001 |NY |Toyota |Camry |1 |0 |4 |123456789 |
| 3 |2014-09-10 12:02:00.000 |ABC 1004 |CT |Ford |Toros |1 |0 |5 |456789123 |
| 2 |2014-09-10 12:03:00.000 |XYZ 1003 |CT |Toyota |Corolla |1 |0 |4 | |
| 1 |2014-09-10 12:03:00.000 |BNJ 1007 |NY |Honda |Crv |1 |0 |5 |789123456 |
| 2 |2014-09-10 12:05:00.000 |CDE 1007 |NJ |Toyota |4x4 |1 |0 |6 |321987654 |

Sütunların kısa bir açıklaması aşağıda veda edilmiştir:

| Sütun | Açıklama |
| --- | --- |
| TollID |Bir gişeyi benzersiz olarak tanımlayan gişe kimliği |
| Giriş Zamanı |Aracın UTC'deki gişeye giriş tarihi ve saati |
| Ruhsat Plakası |Aracın plaka numarası |
| Durum |Amerika Birleşik Devletleri'nde bir devlet |
| Marka |Otomobil üreticisi |
| Model |Otomobilin model numarası |
| Araç Tipi |Binek araçlar için 1, ticari araçlar için 2 |
| Ağırlık Türü |Ton araç ağırlığı; Binek araçlar için 0 |
| Ücretli |USD'deki geçiş ücreti değeri |
| Etiket |Ödemeyi otomatikleştiren otomobildeki e-Tag; ödemenin el ile yapıldığı boş |

### <a name="exit-data-stream"></a>Veri akışından çıkış
Çıkış veri akışı, gişeden ayrılan araçlar la ilgili bilgiler içerir. Çıkış veri solayları, örnek uygulamaya dahil olan bir Web Uygulamasından Olay Hub kuyruğuna canlı olarak aktarılır.

| **Tollid** | **Çıkış Zamanı** | **Ruhsat Plakası** |
| --- | --- | --- |
| 1 |2014-09-10T12:03:00.000000Z |JNB 7001 |
| 1 |2014-09-10T12:03:00.000000Z |YXZ 1001 |
| 3 |2014-09-10T12:04:00.000000Z |ABC 1004 |
| 2 |2014-09-10T12:07:00.000000Z |XYZ 1003 |
| 1 |2014-09-10T12:08:00.000000Z |BNJ 1007 |
| 2 |2014-09-10T12:07:00.000000Z |CDE 1007 |

Sütunların kısa bir açıklaması aşağıda veda edilmiştir:

| Sütun | Açıklama |
| --- | --- |
| TollID |Bir gişeyi benzersiz olarak tanımlayan gişe kimliği |
| Çıkış Zamanı |UTC'deki gişeden aracın çıkış tarihi ve saati |
| Ruhsat Plakası |Aracın plaka numarası |

### <a name="commercial-vehicle-registration-data"></a>Ticari araç tescil verileri
Çözüm, ticari araç tescil veritabanının statik anlık görüntüsünü kullanır. Bu veriler, örnekte yer alan Azure blob depolama alanına JSON dosyası olarak kaydedilir.

| Ruhsat Plakası | RegistrationId | Süresi doldu |
| --- | --- | --- |
| SVT 6023 |285429838 |1 |
| XLZ 3463 |362715656 |0 |
| BAC 1005 |876133137 |1 |
| RIV 8632 |992711956 |0 |
| SNY 7188 |592133890 |0 |
| ELH 9896 |678427724 |1 |

Sütunların kısa bir açıklaması aşağıda veda edilmiştir:

| Sütun | Açıklama |
| --- | --- |
| Ruhsat Plakası |Aracın plaka numarası |
| RegistrationId |Aracın plaka numarası |
| Süresi doldu |Aracın tescil durumu: 0 araç tescili aktifse, 1 kayıt süresi dolmuşsa |

## <a name="set-up-the-environment-for-azure-stream-analytics"></a>Azure Akış Analitiği için ortamı ayarlama
Bu çözümü tamamlamak için bir Microsoft Azure aboneliğine ihtiyacınız var. Azure hesabınız yoksa, ücretsiz deneme [sürümü isteyebilirsiniz.](https://azure.microsoft.com/pricing/free-trial/)

Azure kredinizi en iyi şekilde kullanabilmek için bu makalenin sonundaki "Azure hesabınızı temizle" bölümündeki adımları izlediğinizden emin olun.

## <a name="deploy-the-sample"></a>Örneği dağıtma
Birkaç tıklamayla birlikte bir kaynak grubunda kolayca dağıtılabilen birkaç kaynak vardır. Çözüm tanımı GitHub deposunda [https://github.com/Azure/azure-stream-analytics/tree/master/Samples/TollApp](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/TollApp)barındırılır.

### <a name="deploy-the-tollapp-template-in-the-azure-portal"></a>Azure portalında TollApp şablonu dağıtma
1. TollApp ortamını Azure'a dağıtmak için bu bağlantıyı kullanarak [TollApp Azure Şablonu'nu dağıtın.](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-stream-analytics%2Fmaster%2FSamples%2FTollApp%2FVSProjects%2FTollAppDeployment%2Fazuredeploy.json)

2. İstenirse Azure portalında oturum açın.

3. Çeşitli kaynakların faturalandırıldıği aboneliği seçin.

4. Örneğin, `MyTollBooth`benzersiz bir ada sahip yeni bir kaynak grubu belirtin.

5. Bir Azure konumu seçin.

6. Bir **Aralığı** saniye sayısı olarak belirtin. Bu değer, olay hub'ına ne sıklıkta veri gönderilebilmek için örnek web uygulamasında kullanılır.

7. Hüküm ve koşulları kabul etmek için **kontrol edin.**

8. Kaynakları daha sonra kolayca bulabilmeniz için **panoya** Sabitle'yi seçin.

9. Örnek şablonu dağıtmak için **Satın Alma'yı** seçin.

10. Birkaç dakika sonra, **Dağıtım'ın başarılı olduğunu**onaylayan bir bildirim görüntülenir.

### <a name="review-the-azure-stream-analytics-tollapp-resources"></a>Azure Akışı Analytics TollApp kaynaklarını inceleyin

1. Azure portalında oturum açın

2. Önceki bölümde adını vermiş olduğunuz Kaynak Grubunu bulun.

3. Aşağıdaki kaynakların kaynak grubunda listelenmiş olduğunu doğrulayın:
   - Bir Cosmos DB Hesabı
   - Bir Azure Akışı Analizi İşi
   - Bir Azure Depolama Hesabı
   - Bir Azure Etkinlik Merkezi
   - İki Web Uygulaması

## <a name="examine-the-sample-tollapp-job"></a>Örnek TollApp işini inceleyin
1. Önceki bölümdeki kaynak grubundan **başlayarak, tollapp** adından başlayarak Akış Analizi akış işini seçin (ad benzersizlik için rasgele karakterler içerir).

2. İşin **Genel Bakış** sayfasında, sorgu sözdizimini görüntülemek için **Sorgu** kutusuna dikkat edin.

   ```sql
   SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
   INTO CosmosDB
   FROM EntryStream TIMESTAMP BY EntryTime
   GROUP BY TUMBLINGWINDOW(minute, 3), TollId
   ```

   Sorgunun amacını ifade etmek için, gişeye giren araç sayısını saymanız gerektiğini varsayalım. Bir otoyol gişesine sürekli giren araç akışı olduğundan, bunlar giriş olayları hiç durmayan bir dereye benzer. Akışı ölçmek için, üzerinde ölçmek için bir "zaman dilimi" tanımlamanız gerekir. "Her üç dakikada bir gişeye kaç araç girer?" sorusunu daha da inceleyelim. Bu genellikle yuvarlanan sayısı olarak adlandırılır.

   Gördüğünüz gibi, Azure Akış Analizi SQL gibi bir sorgu dili kullanır ve sorgunun zamana ilişkin yönlerini belirtmek için birkaç uzantı ekler.  Daha fazla ayrıntı için, sorguda kullanılan [Zaman Yönetimi](https://docs.microsoft.com/stream-analytics-query/time-management-azure-stream-analytics) ve [Pencereoluşturma](https://docs.microsoft.com/stream-analytics-query/windowing-azure-stream-analytics) yapıları hakkında bilgi edinin.

3. TollApp örnek işinin girişlerini inceleyin. Geçerli sorguda yalnızca EntryStream girişi kullanılır.
   - **EntryStream** girişi, otoyolda bir araç gişeye her girdiğinde verileri gösteren verileri sıraya alan bir Olay Hub bağlantısıdır. Örneklerin bir parçası olan bir web uygulaması olayları oluşturuyor ve bu olay hub'ında bu veriler sıraya dizilir. Bu girişin akış sorgusunun FROM yan tümcesinde sorgulanır.
   - **ExitStream** girişi, otoyolda bir araç gişeden her çıkışında verileri gösteren verileri sıraya alan bir Olay Hub bağlantısıdır. Bu akış girişi sorgu sözdiziminin sonraki varyasyonlarında kullanılır.
   - **Kayıt** girişi, gerektiğinde aramalar için kullanılan statik bir kayıt.json dosyasına işaret eden bir Azure Blob depolama bağlantısıdır. Bu başvuru veri girişi sorgu sözdiziminin sonraki varyasyonlarında kullanılır.

4. TollApp örnek işin çıktılarını inceleyin.
   - **Cosmos DB** çıkışı, çıkış lavabo olaylarını alan bir Cosmos veritabanı kapsayıcısidir. Bu çıktının akış sorgusunun INTO yan tümcesinde kullanıldığını unutmayın.

## <a name="start-the-tollapp-streaming-job"></a>TollApp akış işini başlatın
Akış işini başlatmak için aşağıdaki adımları izleyin:

1. İşin **Genel Bakış** sayfasında **Başlat'ı**seçin.

2. İş **bölmesine Başlat'ta** **Şimdi'yi**seçin.

3. Birkaç dakika sonra, iş çalışmaya başladıktan sonra, akış işinin **Genel Bakış** sayfasında, **İzleme** grafiğini görüntüleyin. Grafik birkaç bin giriş olayları ve çıktı olaylar onlarca göstermelidir.

## <a name="review-the-cosmosdb-output-data"></a>CosmosDB çıktı verilerini gözden geçirin
1. TollApp kaynaklarını içeren kaynak grubunu bulun.

2. Ad deseni **tollapp\<rastgele\>-cosmos**ile Azure Cosmos DB Hesabı seçin.

3. Veri **Gezgini** sayfasını açmak için Veri Gezgini başlığını seçin.

4. **TollAppDatabase** > **tollAppCollection** > **Belgelerini**genişletin.

5. Kimlikler listesinde, çıktı kullanılabilir olduğunda birkaç doküman gösterilir.

6. JSON belgesini incelemek için her kimliği seçin. Her tollid, windowend zaman ve bu pencereden araba sayısı dikkat edin.

7. Ek bir üç dakika sonra, dört belge başka bir dizi, tollid başına bir belge kullanılabilir.


## <a name="report-total-time-for-each-car"></a>Her araç için toplam süreyi bildirme
Bir arabanın geçiş ücretinden geçmesi için gereken ortalama süre, sürecin verimliliğini ve müşteri deneyimini değerlendirmeye yardımcı olur.

Toplam zamanı bulmak için, ExitTime akışıyla Giriş Zamanı akışına katılın. Eşit eşleşen TollId ve LicensePlate sütunlarında iki giriş akışına katılın. **JOIN** işleci, birleştirilmiş olaylar arasındaki kabul edilebilir zaman farkını açıklayan zamansal serbestlik belirtmenizi gerektirir. Olayların birbirinden en fazla 15 dakika olması gerektiğini belirtmek için **DATEDIFF** işlevini kullanın. Ayrıca, bir aracın gişede geçirdiği fiili zamanı hesaplamak için çıkış ve giriş sürelerine **DATEDIFF** işlevini uygulayın. **JOIN** koşulu yerine **SELECT** deyiminde kullanıldığında **DATEDIFF'in** kullanımının farkı dikkate ait.

```sql
SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute, EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
INTO CosmosDB
FROM EntryStream TIMESTAMP BY EntryTime
JOIN ExitStream TIMESTAMP BY ExitTime
ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15
```

### <a name="to-update-the-tollapp-streaming-job-query-syntax"></a>TollApp akış iş sorgusu sözdizimini güncellemek için:

1. İşin **Genel Bakış** sayfasında **Durdur'u**seçin.

2. İşin durdurulduğuna dair bildirim için birkaç dakika bekleyin.

3. İş TOPOLOJIsi başlığı altında **sorgu< > **

4. Ayarlanan akışlı SQL sorgusunu yapıştırın.

5. Sorguyu kaydetmek için **Kaydet'i** seçin. Değişiklikleri kaydetmek için **Evet'i** onaylayın.

6. İşin **Genel Bakış** sayfasında **Başlat'ı**seçin.

7. İş **bölmesine Başlat'ta** **Şimdi'yi**seçin.

### <a name="review-the-total-time-in-the-output"></a>Çıktıdaki toplam süreyi gözden geçirme
Akış işinden CosmosDB çıktı verilerini gözden geçirmek için önceki bölümdeki adımları yineleyin. En son JSON belgelerini gözden geçirin.

Örneğin, bu belge, belirli bir plakaya sahip örnek bir aracı, giriş saatini ve çıkış saatini ve gişe süresini iki dakika olarak gösteren DATEDIFF hesaplanan süreleri gösterir:
```JSON
{
    "tollid": 4,
    "entrytime": "2018-04-05T06:51:39.0491173Z",
    "exittime": "2018-04-05T06:53:09.0491173Z",
    "licenseplate": "JVR 9425",
    "durationinminutes": 2,
    "id": "ff52eb25-d580-7566-2879-1f52bba6601e",
    "_rid": "+8E4AI1DZgBjAAAAAAAAAA==",
    "_self": "dbs/+8E4AA==/colls/+8E4AI1DZgA=/docs/+8E4AI1DZgBjAAAAAAAAAA==/",
    "_etag": "\"ad02f6b8-0000-0000-0000-5ac5c8330000\"",
    "_attachments": "attachments/",
    "_ts": 1522911283
}
```

## <a name="report-vehicles-with-expired-registration"></a>Süresi dolmuş kaydı olan araçları bildirme
Azure Akış Analizi, zamansal veri akışlarıyla birleştirmek için başvuru verilerinin statik anlık görüntülerini kullanabilir. Bu yeteneği göstermek için aşağıdaki örnek soruyu kullanın. Kayıt girişi, lisans etiketlerinin son kullanma gün geçmişlerini listeleyen statik bir blob json dosyasıdır. Referans verileri, plakaya katılarak, geçiş ücretinden geçen her bir araçla karşılaştırılır.

Ticari bir araç ücretli şirkete kayıtlı ise, muayene için durdurulmadan gişeden geçebilir. Süresi dolmuş tüm ticari araçları tanımlamak için kayıt arama tablosunu kullanın.

```sql
SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
INTO CosmosDB
FROM EntryStream TIMESTAMP BY EntryTime
JOIN Registration
ON EntryStream.LicensePlate = Registration.LicensePlate
WHERE Registration.Expired = '1'
```

1. TollApp akış iş sorgusu sözdizimini güncellemek için önceki bölümdeki adımları yineleyin.

2. Akış işinden CosmosDB çıktı verilerini gözden geçirmek için önceki bölümdeki adımları yineleyin.

Örnek çıktı:
```json
    {
        "entrytime": "2018-04-05T08:01:28.0252168Z",
        "licenseplate": "GMT 3221",
        "tollid": 1,
        "registrationid": "763220582",
        "id": "47db0535-9716-4eb2-db58-de7886966cbf",
        "_rid": "y+F8AJ9QWACSAQAAAAAAAA==",
        "_self": "dbs/y+F8AA==/colls/y+F8AJ9QWAA=/docs/y+F8AJ9QWACSAQAAAAAAAA==/",
        "_etag": "\"88007d8d-0000-0000-0000-5ac5d7e20000\"",
        "_attachments": "attachments/",
        "_ts": 1522915298
    }
```

## <a name="scale-out-the-job"></a>İşi ölçeklendirin
Azure Akış Analizi, büyük hacimli verileri işleyebilir şekilde elastik ölçeklendirme yapmak üzere tasarlanmıştır. Azure Akışı Analizi sorgusu, sisteme bu adımın ölçeklendirildigini söylemek için **PARTITION BY** yan tümcesini kullanabilir. **PartitionId,** sistemin girişin (olay hub'ı) bölüm kimliğiyle eşleşmek için eklediği özel bir sütundur.

Sorguyu bölümlere ölçeklendirmek için sorgu sözdizimini aşağıdaki koda göre edin:
```sql
SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
INTO CosmosDB
FROM EntryStream
TIMESTAMP BY EntryTime
PARTITION BY PartitionId
GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId
```

Akış işini daha fazla akış birimine ölçeklendirmek için:

1. Geçerli işi **durdurun.**

2. sorgu sorgusu **sayfasındasorgu** sözdizimini< > ve değişiklikleri kaydedin.

3. Akış işinde CONFIGURE başlığı altında **Ölçek'i**seçin.

4. Akış **birimleri** kaydırıcısını 1'den 6'ya kaydırın. Akış birimleri, işin alabileceği işlem gücü miktarını tanımlar. **Kaydet'i**seçin.

5. Ek ölçeği göstermek için akış işini **başlatın.** Azure Akış Analizi, işi daha fazla bilgi işlem kaynağına dağıtır ve daha iyi iş elde ederek, BÖLÜMÜ YAN TÜMcesinde belirlenen sütunu kullanarak işi kaynaklar arasında bölümlere ayırır.

## <a name="monitor-the-job"></a>İş izleme
**MONITOR** alanı çalışan iş hakkında istatistikler içerir. Depolama hesabını aynı bölgede kullanmak için ilk kez yapılandırma gerekir (bu belgenin geri kalanı gibi ad ücreti).

![Azure Akış Analizi iş izleme](media/stream-analytics-build-an-iot-solution-using-stream-analytics/stream-analytics-job-monitoring.png)

İş panosu **Ayarları** alanından **Da Aktivite Günlükleri** erişebilirsiniz.

## <a name="clean-up-the-tollapp-resources"></a>TollApp kaynaklarını temizleme
1. Azure portalındaki Akış Analizi işini durdurun.

2. TollApp şablonuyla ilgili sekiz kaynak içeren kaynak grubunu bulun.

3. **Kaynak grubunu sil**'i seçin. Silme işlemini onaylamak için kaynak grubunun adını yazın.

## <a name="conclusion"></a>Sonuç
Bu çözüm sizi Azure Akış Analizi hizmetiyle tanıştırdı. Akış Analizi işi için girdi ve çıktıların nasıl yapılandırılabildiğini gösterdi. Çözüm, Ücretli Veri senaryosunu kullanarak, hareket halindeki veri alanında ortaya çıkan yaygın sorun türlerini ve Bunların Azure Akış Analizi'ndeki basit SQL benzeri sorgularla nasıl çözülebileceğini açıkladı. Zamansal verilerle çalışmak için SQL uzantısı yapılarını açıklanan çözüm. Veri akışlarına nasıl katılacağını, statik başvuru verileriyle veri akışını nasıl zenginleştireceklerini ve daha yüksek iş elde etmek için sorguyu nasıl ölçeklendireceklerini gösterdi.

Bu çözüm iyi bir giriş sağlasa da, hiçbir şekilde tamamlanmaz. [Ortak Akış Analizi kullanım desenleri için Sorgu örneklerinde](stream-analytics-stream-analytics-query-patterns.md)SAQL dilini kullanarak daha fazla sorgu deseni bulabilirsiniz.
