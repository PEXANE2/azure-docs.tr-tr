---
title: Azure Akış Analizi ile gerçek zamanlı IoT veri akışlarını işleme
description: Akış analizi ve gerçek zamanlı veri işleme ile birlikte IoT algılayıcı etiketleri ve veri akışları
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 11/26/2019
ms.openlocfilehash: 0755131f7d8071e37eadc1339ebc5e122725fa71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75426239"
---
# <a name="process-real-time-iot-data-streams-with-azure-stream-analytics"></a>Azure Akış Analizi ile gerçek zamanlı IoT veri akışlarını işleme

Bu makalede, Nesnelerin İnterneti (IoT) aygıtlarından veri toplamak için akış işleme mantığının nasıl oluşturulabileceğinizi öğrenirsiniz. Çözümünüzü hızlı ve ekonomik bir şekilde nasıl oluşturabileceğinizi göstermek için gerçek bir Nesnelerin İnterneti (IoT) kullanım örneğini kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

* Ücretsiz bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/)oluşturun.
* Örnek sorguyu ve veri dosyalarını [GitHub'dan indirin.](https://aka.ms/azure-stream-analytics-get-started-iot)

## <a name="scenario"></a>Senaryo

Endüstriyel otomasyon alanında faaliyet gösteren Contoso şirketi, üretim süreçlerini tamamen otomatik hale getirmiştir. Bu fabrikada bulunan makineler, gerçek zamanlı olarak veri akışları yayabilen algılayıcılara sahiptir. Bu senaryoda bir üretim katı yöneticisi, algılayıcı verilerinden gerçek zamanlı bilgiler alarak belirli kalıpları aramak ve bunlara yönelik işlemler yapmak istiyor. Gelen veri akışından ilginç desenler bulmak için sensör verileri üzerinden Akış Analizi Sorgu Dili'ni (SAQL) kullanabilirsiniz.

Bu örnekte, veriler Texas Instruments sensör etiketi aygıtından oluşturulur. Verilerin yükü JSON biçimindedir ve aşağıdaki gibi görünür:

```json
{
    "time": "2016-01-26T20:47:53.0000000",  
    "dspl": "sensorE",  
    "temp": 123,  
    "hmdt": 34  
}  
```

Gerçek hayattaki bir senaryoda, olayları bir akış şeklinde oluşturan bunun gibi yüzlerce algılayıcınız olabilir. Bir ağ geçidi cihazının bu olayları [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)'a veya [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)'larına göndermek için kod çalıştırması idealdir. Akış Analizi işiniz, bu olayları Event Hubs'dan alır ve akışlara yönelik gerçek zamanlı analiz sorguları çalıştırır. Ardından, sonuçları [desteklenen çıktılardan](stream-analytics-define-outputs.md)birine gönderebilirsiniz.

Kullanım kolaylığı için, bu başlangıç kılavuzunda gerçek algılayıcı etiket cihazlarından yakalanan bir örnek veri dosyası sunulmaktadır. Örnek veriler üzerinde sorgu çalıştırabilir ve sonuçları görebilirsiniz. Daha sonraki öğreticilerde, işinizi girişlere ve çıkışlara nasıl bağlayacağınızı ve bunları Azure hizmetine nasıl dağıtacağınızı öğreneceksiniz.

## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

1. Azure [portalında](https://portal.azure.com), + Soldaki gezinme menüsünden **kaynak oluşturun'** seçeneğini belirleyin. Ardından, **Analytics'ten** **Stream Analytics işini** seçin.
   
    ![Yeni bir Akış Analizi işi oluşturma](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

1. Benzersiz bir iş adı girin ve aboneliğin işiniz için doğru olduğundan emin olun. Yeni bir kaynak grubu oluşturun veya aboneliğinizden varolan bir tane seçin.

1. İşiniz için bir konum seçin. Daha fazla işlem hızı ve maliyetleri azaltmak için kaynak grubunuz ve tüm kaynaklarınız için aynı konumu kullanın. Yapılandırmaları yaptıktan sonra **Oluştur'u**seçin.
   
    ![Yeni bir Akış Analizi işi oluşturma ayrıntıları](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)

## <a name="create-an-azure-stream-analytics-query"></a>Azure Akış Analizi sorgusu oluşturma
İşiniz oluşturulduktan sonra bir sonraki adım bir sorgu yazmaktır. Bir giriş veya çıktıyı işinize bağlamadan sorguları örnek verilere karşı sınayabilirsiniz.

GitHub'dan [HelloWorldASA-InputStream.json'u](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/GettingStarted/HelloWorldASA-InputStream.json
) indirin. Ardından, Azure portalındaki Azure Akış Analizi işinize gidin.

Sol menüden **İş topolojisi** altında **Sorgula'yı** seçin. Ardından **Yükle örnek girişi**seçin. Dosyayı `HelloWorldASA-InputStream.json` yükleyin ve **Tamam'ı**seçin.

![Akış Analizi pano sorgu döşemesi](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

**Giriş önizleme** tablosunda verilerin önizlemesinin otomatik olarak doldurulanana dikkat edin.

![Örnek giriş verilerinin önizlemesi](./media/stream-analytics-get-started-with-iot-devices/input-preview.png)

### <a name="query-archive-your-raw-data"></a>Sorgu: Ham verilerinizi arşivleme

En basit sorgu biçimi, tüm giriş verilerini belirlenmiş çıkışlarına arşivleyen bir doğrudan sorgudur. Bu sorgu, yeni bir Azure Akışı Analizi işinde doldurulan varsayılan sorgudur.

```sql
SELECT 
    *
INTO
    Output
FROM
    InputStream
```

**Test sorgusunu** seçin ve **sonuçları Test sonuçları** tablosunda görüntüleyin.

![Stream Analytics sorgusu için test sonuçları](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-the-data-based-on-a-condition"></a>Sorgu: Verileri bir koşula göre filtreleme

Bir koşula göre sonuçları filtrelemeye çalışalım. Sadece "sensorA"dan gelen olayların sonuçlarını göstermek istiyoruz.

```sql
SELECT 
    time,
    dspl AS SensorName,
    temp AS Temperature,
    hmdt AS Humidity
INTO
   Output
FROM
    InputStream
WHERE dspl='sensorA'
```

Sorguyu düzenleyiciye yapıştırın ve sonuçları gözden geçirmek için **Test sorgusunu** seçin.

![Bir veri akışını filtreleme](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

### <a name="query-alert-to-trigger-a-business-workflow"></a>Sorgu: İş akışı tetiklemesi için uyarı

Şimdi sorgumuzu daha ayrıntılı hale getirelim. Her algılayıcı türü için ortalama sıcaklığı 30 saniyelik aralıklarda izlemek ve yalnızca ortalama sıcaklık 100 derecenin üzerinde olduğu zaman sonuçları görüntülemek istiyoruz.

```sql
SELECT 
    System.Timestamp AS OutputTime,
    dspl AS SensorName,
    Avg(temp) AS AvgTemperature
INTO
   Output
FROM
    InputStream TIMESTAMP BY time
GROUP BY TumblingWindow(second,30),dspl
HAVING Avg(temp)>100
```

![30 saniyelik filtre sorgusu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Ortalama ılıman 100'den büyük olan sensörlerin yalnızca 245 satır ve adlarını içeren sonuçlar görmeniz gerekir. Bu sorguda olay akışı, 30 saniyelik bir **Atlayan Pencere** üzerinden algılayıcı adı olan **dspl**'ye göre gruplanır. Zamansal sorgular, zamanın nasıl ilerlemesini istediğinizi belirtmelidir. **TIMESTAMP BY** yan tümcesini kullanarak, zamanları tüm zamansal hesaplamalarla ilişkilendirmek için **OUTPUTTIME** sütununa belirttiniz. Ayrıntılı bilgi için [Zaman Yönetimi](https://docs.microsoft.com/stream-analytics-query/time-management-azure-stream-analytics) ve [Pencereleme işlevleri](https://docs.microsoft.com/stream-analytics-query/windowing-azure-stream-analytics)hakkında bilgi edinin.

### <a name="query-detect-absence-of-events"></a>Sorgu: Var olmayan olayları algılama

Eksik giriş olaylarını bulmak için nasıl sorgu yazabiliriz? Bir sensör veri gönderdi ve daha sonra sonraki 5 saniye için olayları göndermedi son kez bulalım.

```sql
SELECT 
    t1.time,
    t1.dspl AS SensorName
INTO
   Output
FROM
    InputStream t1 TIMESTAMP BY time
LEFT OUTER JOIN InputStream t2 TIMESTAMP BY time
ON
    t1.dspl=t2.dspl AND
    DATEDIFF(second,t1,t2) BETWEEN 1 and 5
WHERE t2.dspl IS NULL
```

![Var olmayan olayları algılama](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

Burada, **LEFT OUTER** deyimini aynı veri akışı üzerinde kullanırız (kendi kendine birleşme). Bir **INNER** birleşimde, yalnızca bir eşleşme bulunduğu zaman bir sonuç döndürülür.  Bir **LEFT OUTER** birleşimde, birleştirmenin sol tarafındaki bir olay eşleşmemişse sağ taraftaki tüm sütunlar için NULL değerine sahip bir satır döndürülür. Bu teknik, var olmayan olayların bulunması için oldukça kullanışlıdır. Daha fazla bilgi için [JOIN'e](https://docs.microsoft.com/stream-analytics-query/join-azure-stream-analytics)bakın.

## <a name="conclusion"></a>Sonuç

Bu makalenin amacı, farklı Akış Analizi Sorgu Dili sorgularının nasıl yazılabildiğini ve sonuçları tarayıcıda nasıl göreceğini göstermektir. Ancak, bu sadece başlamak için. Akış Analizi birçok giriş ve çıkışı desteklemenin yanı sıra Azure Machine Learning'deki işlevlerden de faydalanır. Bu da onu veri akışlarının analizi için sağlam bir araç haline getirir. Sorgu yazma hakkında daha fazla bilgi için [ortak sorgu desenleri](stream-analytics-stream-analytics-query-patterns.md) hakkındaki makaleyi okuyun.

