---
title: Azure Stream Analytics kullanarak IoT çözümü oluşturma
description: Bir tollstand senaryosunun IoT çözümü Stream Analytics için Başlarken öğreticisi
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: f506cc526a824d45ae2d6b7a75e1c1a99dae4d64
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75426452"
---
# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Stream Analytics kullanarak IoT çözümü oluşturma

## <a name="introduction"></a>Giriş
Bu çözümde, verilerinizden gerçek zamanlı içgörüler elde etmek için Azure Stream Analytics kullanmayı öğreneceksiniz. Geliştiriciler, iş öngörüleri türetmede geçmiş kayıtları veya başvuru verileriyle birlikte tıklama-akışlar, Günlükler ve cihaz tarafından oluşturulan olaylar gibi veri akışlarını kolayca birleştirebilir. Microsoft Azure içinde barındırılan, tam olarak yönetilen, gerçek zamanlı bir akış hesaplama hizmeti olarak Azure Stream Analytics, dakikalar içinde çalışmaya başlamanızı sağlamak için yerleşik dayanıklılık, düşük gecikme süresi ve ölçeklenebilirlik sağlar.

Bu çözümü tamamladıktan sonra şunları yapabilirsiniz:

* Azure Stream Analytics portalı hakkında bilgi edinin.
* Bir akış işini yapılandırın ve dağıtın.
* Gerçek dünya sorunlarını Stream Analytics ve sorgu dilini kullanarak çözün.
* Stream Analytics güvenle kullanarak müşterileriniz için akış çözümleri geliştirin.
* Sorunları gidermek için izleme ve günlüğe kaydetme deneyimini kullanın.

## <a name="prerequisites"></a>Ön koşullar
Bu çözümü tamamlayabilmeniz için aşağıdaki önkoşulların olması gerekir:
* Bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/)

## <a name="scenario-introduction-hello-toll"></a>Senaryoya giriş: "Merhaba, ücretli!"
Ücretli istasyon ortak bir Phenomenon olur. Dünyanın dört bir yanında birçok ifade, köprü ve tünelle karşılaşırsınız. Her ücretli istasyonda birden çok ücretsiz olarak bulunur. El ile yapılan bir görevlisi ile ücretsiz olarak ödeme yapmayı durdurursunuz. Otomatik olarak, her bir stand üzerinde yapılan bir algılayıcı, stand 'yi geçirdiğiniz sırada, sizin için tek bir bir RFıD kartını tarar. Bu ücretli istasyonlardan, ilginç işlemlerin gerçekleştirilebileceği bir olay akışı olarak taşıtmayı kolayca görselleştirebilirsiniz.

![Ücretsiz otomobillerin resmi](media/stream-analytics-build-an-iot-solution-using-stream-analytics/cars-in-toll-booth.jpg)

## <a name="incoming-data"></a>Gelen veriler
Bu çözüm iki veri akışı ile birlikte kullanılabilir. Ücretli istasyonların giriş ve çıkış bölümünde yüklenen sensörler ilk akışı üretir. İkinci akış, araç kayıt verilerine sahip bir statik arama veri kümesidir.

### <a name="entry-data-stream"></a>Giriş veri akışı
Giriş veri akışı, ücretli istasyonlara girerken otomobiller hakkında bilgiler içerir. Çıkış verileri olayları, örnek uygulamada yer alan bir Web uygulamasından bir olay hub 'ı kuyruğuna canlı olarak kaydedilir.

| Tollıd | EntryTime | Licenselevha | Durum | Marka | Model | VehicleType | VehicleWeight | Numarası | Etiket |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 |2014-09-10 12:01:00.000 |JNB 7001 |NY |Honda |CRV |1 |0 |7 | |
| 1 |2014-09-10 12:02:00.000 |YıLXZ 1001 |NY |Toyota |Camry |1 |0 |4 |123456789 |
| 3 |2014-09-10 12:02:00.000 |ABC 1004 |CT |Ford |Boğa |1 |0 |5 |456789123 |
| 2 |2014-09-10 12:03:00.000 |XYZ 1003 |CT |Toyota |Corolla |1 |0 |4 | |
| 1 |2014-09-10 12:03:00.000 |BNJ 1007 |NY |Honda |CRV |1 |0 |5 |789123456 |
| 2 |2014-09-10 12:05:00.000 |CDE 1007 |NJ |Toyota |4x4 |1 |0 |6 |321987654 |

Aşağıda sütunların kısa bir açıklaması verilmiştir:

| Sütun | Açıklama |
| --- | --- |
| Tollıd |Bir stand 'yi benzersiz bir şekilde tanımlayan Stand No |
| EntryTime |Araç için stand 'nın giriş tarihi ve saati (UTC) |
| Licenselevha |Araç için lisans levha numarası |
| Durum |Birleşik Devletler bir durum |
| Marka |Otomobil üreticisi |
| Model |Otomobil 'nin model numarası |
| VehicleType |Yolcular için 1 veya ticari araçlar için 2 |
| Ağırlıklı TType |Ton cinsinden ton ağırlığı; yolcular taşıtlar için 0 |
| Numarası |ABD Doları cinsinden ücretli değer |
| Etiket |Otomatik mobil üzerinde ödemeyi otomatikleştiren e-etiketi; ödemenin el ile yapıldığı yerde boş |

### <a name="exit-data-stream"></a>Çıkış veri akışı
Çıkış veri akışı, ücretli istasyondan çıkan otomobiller hakkında bilgiler içerir. Çıkış verileri olayları, örnek uygulamada yer alan bir Web uygulamasından bir olay hub 'ı kuyruğuna canlı olarak kaydedilir.

| **Tollıd** | **ExitTime** | **Licenselevha** |
| --- | --- | --- |
| 1 |2014-09-10T12:03:00.0000000 Z |JNB 7001 |
| 1 |2014-09-10T12:03:00.0000000 Z |YıLXZ 1001 |
| 3 |2014-09-10T12:04:00.0000000 Z |ABC 1004 |
| 2 |2014-09-10T12:07:00.0000000 Z |XYZ 1003 |
| 1 |2014-09-10T12:08:00.0000000 Z |BNJ 1007 |
| 2 |2014-09-10T12:07:00.0000000 Z |CDE 1007 |

Aşağıda sütunların kısa bir açıklaması verilmiştir:

| Sütun | Açıklama |
| --- | --- |
| Tollıd |Bir stand 'yi benzersiz bir şekilde tanımlayan Stand No |
| ExitTime |Bir araç için ücretli stand 'dan çıkış tarihi ve saati UTC |
| Licenselevha |Araç için lisans levha numarası |

### <a name="commercial-vehicle-registration-data"></a>Ticari araç kayıt verileri
Çözüm, ticari bir araç kayıt veritabanının statik bir anlık görüntüsünü kullanır. Bu veriler, Azure Blob depolama alanına bir JSON dosyası olarak kaydedilir ve örneğe dahildir.

| Licenselevha | RegistrationId | Süresi doldu |
| --- | --- | --- |
| SVT 6023 |285429838 |1 |
| XLZ 3463 |362715656 |0 |
| ARKA ARKAYA 1005 |876133137 |1 |
| RIV 8632 |992711956 |0 |
| SNY 7188 |592133890 |0 |
| ELH 9896 |678427724 |1 |

Aşağıda sütunların kısa bir açıklaması verilmiştir:

| Sütun | Açıklama |
| --- | --- |
| Licenselevha |Araç için lisans levha numarası |
| RegistrationId |Araç kayıt KIMLIĞI |
| Süresi doldu |Aracın kayıt durumu: araç kaydı etkinse 0, kaydın geçerliliği dolmuşsa 1 |

## <a name="set-up-the-environment-for-azure-stream-analytics"></a>Azure Stream Analytics için ortamı ayarlama
Bu çözümü tamamlayabilmeniz için bir Microsoft Azure aboneliğine sahip olmanız gerekir. Azure hesabınız yoksa, [ücretsiz bir deneme sürümü](https://azure.microsoft.com/pricing/free-trial/)isteyebilirsiniz.

Azure kredinizi en iyi şekilde kullanabilmeniz için bu makalenin sonundaki "Azure hesabınızı Temizleme" bölümündeki adımları izlediğinizden emin olun.

## <a name="deploy-the-sample"></a>Örneği dağıtma
Bir kaynak grubunda birkaç tıklamayla kolayca dağıtılabilecek birkaç kaynak vardır. Çözüm tanımı, konumundaki [https://github.com/Azure/azure-stream-analytics/tree/master/Samples/TollApp](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/TollApp)GitHub deposunda barındırılır.

### <a name="deploy-the-tollapp-template-in-the-azure-portal"></a>Azure portal için TollApp şablonunu dağıtın
1. TollApp ortamını Azure 'a dağıtmak için bu bağlantıyı kullanarak [Tollapp Azure şablonunu dağıtın](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-stream-analytics%2Fmaster%2FSamples%2FTollApp%2FVSProjects%2FTollAppDeployment%2Fazuredeploy.json).

2. İstenirse Azure portal oturum açın.

3. Çeşitli kaynakların faturalandırılabileceği aboneliği seçin.

4. Örneğin `MyTollBooth`, benzersiz bir ada sahip yeni bir kaynak grubu belirtin.

5. Bir Azure konumu seçin.

6. Saniye sayısı olarak bir **Aralık** belirtin. Bu değer, örnek Web uygulamasında, Olay Hub 'ına veri gönderme sıklığı için kullanılır.

7. Hüküm ve koşulları kabul etmek için **işaretleyin** .

8. Kaynakları daha sonra kolayca bulabilmeniz **için panoya sabitle ' yi** seçin.

9. Örnek şablonu dağıtmak için **satın al** ' ı seçin.

10. Birkaç dakika sonra, **dağıtımın başarılı**olduğunu onaylamak için bir bildirim belirir.

### <a name="review-the-azure-stream-analytics-tollapp-resources"></a>Azure Stream Analytics TollApp kaynaklarını gözden geçirin

1. Azure portalında oturum açın

2. Önceki bölümde adlandırdığınız kaynak grubunu bulun.

3. Aşağıdaki kaynakların kaynak grubunda listelendiğinden emin olun:
   - Bir Cosmos DB hesabı
   - Bir Azure Stream Analytics Işi
   - Bir Azure depolama hesabı
   - Bir Azure Olay Hub 'ı
   - İki Web Apps

## <a name="examine-the-sample-tollapp-job"></a>Örnek TollApp işini inceleyin
1. Önceki bölümde kaynak grubundan başlayarak, **tollapp** adı ile başlayan Stream Analytics akış işini seçin (ad, benzersizlik için rastgele karakterler içerir).

2. İşin **genel bakış** sayfasında sorgu söz dizimini görüntülemek için **sorgu** kutusuna dikkat edin.

   ```sql
   SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
   INTO CosmosDB
   FROM EntryStream TIMESTAMP BY EntryTime
   GROUP BY TUMBLINGWINDOW(minute, 3), TollId
   ```

   Sorgunun amacını ifade etmek için, bir stand girerek telefon numarasını saymanız gerektiğini varsayalım. Bir otoyol 'e girerken sürekli bir telefon akışı olduğundan, giriş olayları, hiç durdurulmamış bir akışa benzerdir. Akışı ölçmek için, ölçmek üzere bir "zaman dilimi" tanımlamanız gerekir. Bu soruyu daha da belirginleştirem, "kaç kişinin her üç dakikada bir ücretli bir stand mi girsin?" Bu, yaygın olarak belirtilen sayı olarak adlandırılır.

   Gördüğünüz gibi Azure Stream Analytics, SQL gibi bir sorgu dili kullanır ve sorgunun zaman ilişkili yönlerini belirtmek için birkaç uzantı ekliyor.  Daha fazla ayrıntı için sorguda kullanılan [zaman yönetimi](https://docs.microsoft.com/stream-analytics-query/time-management-azure-stream-analytics) ve [Pencereleme](https://docs.microsoft.com/stream-analytics-query/windowing-azure-stream-analytics) yapıları hakkında bilgi edinin.

3. TollApp örnek işinin girişlerini inceleyin. Geçerli sorguda yalnızca EntryStream girişi kullanılır.
   - **Entrystream** girişi, bir araba otoyol üzerine bir tollstand girdiğinde verileri sıraya alan bir olay hub bağlantısıdır. Örneğin bir parçası olan bir Web uygulaması olayları oluşturuyor ve bu veri bu olay hub 'ında sıraya alındı. Bu girişin, akış sorgusunun FROM yan tümcesinde sorgulandığını unutmayın.
   - **Exitstream** girişi, her bir otomobil otovaya bir tollstand 'tan çıktığında verileri sıraya alan bir olay hub bağlantısıdır. Bu akış girişi, sorgu sözdiziminin sonraki varyasyonları içinde kullanılır.
   - **Kayıt** girişi, gerektiğinde aramalar için kullanılan bir statik kayıt. json dosyasını işaret eden bir Azure Blob depolama bağlantısıdır. Bu başvuru veri girişi, sorgu sözdiziminin sonraki varyasyonları içinde kullanılır.

4. TollApp örnek işinin çıkışlarını inceleyin.
   - **Cosmos DB** çıktısı, çıkış havuzu olaylarını alan bir Cosmos veritabanı kapsayıcısıdır. Bu çıkışın, akış sorgusunun INTO yan tümcesinde kullanıldığını unutmayın.

## <a name="start-the-tollapp-streaming-job"></a>TollApp akış işini başlatma
Akış işini başlatmak için aşağıdaki adımları izleyin:

1. İşin **genel bakış** sayfasında **Başlat**' ı seçin.

2. **Işi Başlat** bölmesinde **Şimdi**' yi seçin.

3. Birkaç dakika sonra, iş çalışmaya başladıktan sonra, akış işinin **genel bakış** sayfasında **izleme** grafiğini görüntüleyin. Grafik birkaç bin giriş olayı ve onlarca çıkış olayı göstermelidir.

## <a name="review-the-cosmosdb-output-data"></a>CosmosDB çıkış verilerini gözden geçirme
1. TollApp kaynaklarını içeren kaynak grubunu bulun.

2. **Tollapp\<Random\>-cosmos**ad düzenine sahip Azure Cosmos DB hesabını seçin.

3. Veri Gezgini sayfasını açmak için **Veri Gezgini** başlığını seçin.

4. **Tollappdatabase** > **tollappcollection** > **belgelerini**genişletin.

5. Kimlik listesinde, çıktı kullanılabilir olduğunda birkaç belge gösterilir.

6. JSON belgesini gözden geçirmek için her kimliği seçin. Her bir tollıd, windowend saati ve bu penceredeki otomobillerin sayısını fark edin.

7. Ek üç dakika sonra, bir dizi dört belge, her tollıd için bir belge vardır.


## <a name="report-total-time-for-each-car"></a>Her araba için toplam süre raporu
Bir otomobil tarafından ücretsiz olarak geçmesi gereken ortalama süre, işlemin verimliliğini ve müşteri deneyimini değerlendirmenize yardımcı olur.

Toplam süreyi bulmak için, ExitTime akışıyla birlikte EntryTime akışına katın. Eşit eşleşen Tollıd ve Licencelevha sütunlarında iki giriş akışını birleştirin. **JOIN** işleci, birleştirilmiş olaylar arasında kabul edilebilir zaman farkını açıklayan, zamana bağlı Leeway belirtmenizi gerektirir. Olayların birbirinden 15 dakikadan fazla olmaması gerektiğini belirtmek için **DATEDIFF** işlevini kullanın. Ayrıca, bir otomobil 'nin ücretli istasyonda harcadığı gerçek süreyi hesaplamak için, çıkış ve giriş zamanlarında **DATEDIFF** işlevini uygulayın. Bir **JOIN** koşulu yerine **Select** ifadesinde kullanıldığında, **DATEDIFF** 'in kullanımını fark edin.

```sql
SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute, EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
INTO CosmosDB
FROM EntryStream TIMESTAMP BY EntryTime
JOIN ExitStream TIMESTAMP BY ExitTime
ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15
```

### <a name="to-update-the-tollapp-streaming-job-query-syntax"></a>TollApp akış işi sorgu söz dizimini güncelleştirmek için:

1. İşin **genel bakış** sayfasında **Durdur**' u seçin.

2. İşin durdurulduğu bildirim için birkaç dakika bekleyin.

3. Iş TOPOLOJISI başlığı altında **< > sorgula** ' yı seçin.

4. Ayarlanmış akış SQL sorgusunu yapıştırın.

5. Sorguyu kaydetmek için **Kaydet** ' i seçin. Değişiklikleri kaydetmek için **Evet** ' i onaylayın.

6. İşin **genel bakış** sayfasında **Başlat**' ı seçin.

7. **Işi Başlat** bölmesinde **Şimdi**' yi seçin.

### <a name="review-the-total-time-in-the-output"></a>Çıkışdaki toplam süreyi gözden geçirin
Akış işinden CosmosDB çıkış verilerini gözden geçirmek için yukarıdaki bölümdeki adımları yineleyin. En son JSON belgelerini gözden geçirin.

Örneğin, bu belgede, belirli bir lisans kalıbına, entrytime ve çıkış zamanına ve DATESTANDDURATIONINMINUTES alanını iki dakika olarak gösteren bir örnek otomobil gösterilmektedir:
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

## <a name="report-vehicles-with-expired-registration"></a>Son kaydedilen kayıt ile rapor Araçlar
Azure Stream Analytics, zamana bağlı veri akışları ile birleştirmek için başvuru verilerinin statik anlık görüntülerini kullanabilir. Bu özelliği göstermek için aşağıdaki örnek soruyu kullanın. Kayıt girişi, lisans etiketlerinin süre sonlarını listeleyen bir statik blob JSON dosyasıdır. Lisans kalıbına katılarak, başvuru verileri her bir araç ile her iki yönlü olarak geçen her bir araç ile karşılaştırılır.

Ticari bir araç ücretli şirkete kayıtlı ise, denetim için durdurulmaksızın ücretli stand aracılığıyla geçiş yapabilir. Son kullanma kayıtları olan tüm ticari araçlar 'ı belirlemek için kayıt arama tablosunu kullanın.

```sql
SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
INTO CosmosDB
FROM EntryStream TIMESTAMP BY EntryTime
JOIN Registration
ON EntryStream.LicensePlate = Registration.LicensePlate
WHERE Registration.Expired = '1'
```

1. TollApp akış işi sorgu söz dizimini güncelleştirmek için yukarıdaki bölümdeki adımları yineleyin.

2. Akış işinden CosmosDB çıkış verilerini gözden geçirmek için yukarıdaki bölümdeki adımları yineleyin.

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

## <a name="scale-out-the-job"></a>İşi ölçeklendirme
Azure Stream Analytics, büyük hacimlerinizi işleyebilecek şekilde esnek ölçeklendirmek üzere tasarlanmıştır. Azure Stream Analytics sorgusu, sisteme bu adımın ölçeklendirmiş olduğunu bildirmek için bir **PARTITION by** yan tümcesini kullanabilir. **PartitionID** , sistemin, GIRIŞIN bölüm kimliğini (Event hub) eşleştirmek için eklediği özel bir sütundur.

Sorguyu bölümlere ölçeklendirmek için sorgu söz dizimini aşağıdaki kodla düzenleyin:
```sql
SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
INTO CosmosDB
FROM EntryStream
TIMESTAMP BY EntryTime
PARTITION BY PartitionId
GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId
```

Akış işini daha fazla akış birimine ölçeklendirmek için:

1. Geçerli işi **durdurun** .

2. Sorgu söz dizimini **< > sorgu** sayfasında güncelleştirin ve değişiklikleri kaydedin.

3. Akış işindeki yapılandırma başlığı altında **Ölçek**' i seçin.

4. **Akış birimleri** kaydırıcısını 1 ' den 6 ' a kaydırın. Akış birimleri, işin alabileceği işlem gücü miktarını tanımlar. **Kaydet**’i seçin.

5. Ek ölçeği göstermek için akış işini **başlatın** . Azure Stream Analytics, işleri daha fazla işlem kaynağına dağıtır ve bölüm BY yan tümcesinde belirtilen sütunu kullanarak işleri kaynaklar genelinde bölümleyerek daha iyi işleme elde edin.

## <a name="monitor-the-job"></a>İş izleme
**İzleme** alanı çalışan iş hakkında istatistikler içerir. Depolama hesabını aynı bölgede (Bu belgenin geri kalanı gibi) kullanmak için ilk kez yapılandırma gerekir.

![Azure Stream Analytics iş izleme](media/stream-analytics-build-an-iot-solution-using-stream-analytics/stream-analytics-job-monitoring.png)

**Etkinlik günlüklerine** Iş Pano **ayarları** alanından da erişebilirsiniz.

## <a name="clean-up-the-tollapp-resources"></a>TollApp kaynaklarını Temizleme
1. Azure portal Stream Analytics işini durdurun.

2. TollApp şablonuyla ilgili sekiz kaynak içeren kaynak grubunu bulun.

3. **Kaynak grubunu sil**'i seçin. Silmeyi onaylamak için kaynak grubunun adını yazın.

## <a name="conclusion"></a>Sonuç
Bu çözüm sizi Azure Stream Analytics hizmetine tanıtmaktadır. Stream Analytics işi için girişlerin ve çıktıların nasıl yapılandırılacağı gösterilmiştir. Ücretsiz veri senaryosunu kullanarak çözüm, hareket halindeki verilerin alanında ortaya çıkan ve Azure Stream Analytics içindeki basit SQL benzeri sorgularla nasıl çözülebilecekleri hakkında yaygın olarak karşılaşılan sorun türlerini açıklanmıştı. Geçici verilerle çalışmak için SQL uzantı yapılarını tarif eden çözüm. Veri akışlarına nasıl katılabileceğiniz, veri akışının statik başvuru verileriyle nasıl zenginleştirileceği ve daha yüksek aktarım hızı elde etmek için bir sorgunun nasıl ölçekleneceği gösterildi.

Bu çözüm iyi bir giriş sağlasa da, herhangi bir nedenle tamamlanmaz. [Ortak Stream Analytics kullanım desenleri Için sorgu örneklerinde](stream-analytics-stream-analytics-query-patterns.md)saql dilini kullanarak daha fazla sorgu deseni bulabilirsiniz.
