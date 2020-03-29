---
title: Azure Akış Analizi ile Power BI pano entegrasyonu
description: Bu makalede, azure akışı analizi işinin verilerini görselleştirmek için gerçek zamanlı Power BI panosunun nasıl kullanılacağı açıklanmaktadır.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/05/2019
ms.openlocfilehash: 8466fbcb4325dc244551a3b84fc20581366b7071
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78851158"
---
# <a name="stream-analytics-and-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Akış Analizi ve Power BI: Veri akışı için gerçek zamanlı analiz panosu

Azure Akış Analizi, önde gelen iş zekası araçlarından biri olan [Microsoft Power BI'den](https://powerbi.com/)yararlanmanızı sağlar. Bu makalede, Azure Akış Analizi işleriniz için bir çıktı olarak Power BI'yi kullanarak iş zekası araçlarının nasıl oluşturulabildiğini öğrenirsiniz. Ayrıca, gerçek zamanlı bir pano oluşturmayı ve kullanmayı da öğrenirsiniz.

Bu makale, Stream Analytics [gerçek zamanlı dolandırıcılık algılama](stream-analytics-real-time-fraud-detection.md) öğretici devam eder. Bu öğreticide oluşturulan iş akışını oluşturur ve Bir Akış Analizi işi tarafından algılanan sahte telefon aramalarını görselleştirebilmeniz için bir Power BI çıktısı ekler. 

Bu senaryoyu gösteren [bir video](https://www.youtube.com/watch?v=SGUpT-a99MA) izleyebilirsiniz.


## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunlara sahip olduğunuzdan emin olun:

* Bir Azure hesabı.
* Power BI Pro için bir hesap. Bir iş hesabı veya okul hesabı kullanabilirsiniz.
* [Gerçek zamanlı dolandırıcılık algılama](stream-analytics-real-time-fraud-detection.md) öğretici tamamlanmış bir sürümü. Öğretici, hayali telefon görüşmesi meta verileri üreten bir uygulama içerir. Öğreticide, bir olay hub'ı oluşturur ve akışlı telefon arama verilerini olay merkezine gönderirsiniz. Sahte aramaları algılayan bir sorgu yazarsınız (farklı konumlarda aynı anda aynı numaradan yapılan aramalar). 


## <a name="add-power-bi-output"></a>Güç BI çıkışı ekle
Gerçek zamanlı dolandırıcılık algılama öğreticisinde, çıktı Azure Blob depolama alanına gönderilir. Bu bölümde, Power BI'ye bilgi gönderen bir çıktı eklersiniz.

1. Azure portalında, daha önce oluşturduğunuz Akış Analizi işini açın. Önerilen adı kullandıysanız, iş adı `sa_frauddetection_job_demo`.

2. Sol **menüde, İş topolojisi**altında **Çıktılar'ı** seçin. Ardından, açılan menüden **+ Ekle'yi** seçin ve **Power BI'yi** seçin.

3. Seçin +**Güç BI** **ekle** > . Ardından formu aşağıdaki bilgilerle doldurun ve **Yetkilendir**'i seçin:

   |**Ayar**  |**Önerilen değer**  |
   |---------|---------|
   |Çıktı diğer adı  |  CallStream-PowerBI  |
   |Veri kümesi adı  |   sa-veri seti  |
   |Tablo adı |  sahte aramalar  |

   ![Akış Analizi çıktısını yapılandır](media/stream-analytics-power-bi-dashboard/configure-stream-analytics-output.png)

   > [!WARNING]
   > Power BI'de, Akış Analizi işinde belirttiğiniz adlarla aynı adlara sahip bir veri kümesi ve tablo varsa, varolanların üzerine yazılır.
   > Bu veri kümesini ve tabloyu Power BI hesabınızda açıkça oluşturmamanızı öneririz. Stream Analytics işinize başladığınızda ve iş Power BI'ye çıktı pompalamaya başladığınızda otomatik olarak oluşturulur. İş sorgunuz herhangi bir sonuç döndürmüyorsa, veri kümesi ve tablo oluşturulmaz.
   >

4. **Yetkilendir**'i seçtiğinizde bir açılır pencere görünür ve Power BI hesabınızda kimlik doğrulaması için sizden kimlik bilgilerini sağlamanız istenir. Yetkilendirme başarılı olduktan sonra **Kaydet** seçeneğine tıklayarak ayarları kaydedin.

8. **Oluştur'u**tıklatın.

Veri kümesi aşağıdaki ayarlarla oluşturulur:

* **defaultRetentionPolicy: BasicFIFO** - Veri FIFO, en fazla 200.000 satır ile.
* **defaultMode: pushStreaming** - Veri kümesi hem akış kutucuklarını hem de geleneksel rapor tabanlı görselleri (itme olarak da bilinir) destekler.

Şu anda, diğer bayraklarla veri kümeleri oluşturamazsınız.

Power BI veri kümeleri hakkında daha fazla bilgi için [Power BI REST API](https://msdn.microsoft.com/library/mt203562.aspx) başvurusuna bakın.


## <a name="write-the-query"></a>Sorguyu yazma

1. **Outputs** bıçağını kapatın ve iş bıçağına geri dönün.

2. **Sorgu** kutusunu tıklatın. 

3. Aşağıdaki sorguyu girin. Bu sorgu, sahtekarlık algılama öğreticisinde oluşturduğunuz kendi kendine birleştirme sorgusuna benzer. Aradaki fark, bu sorgunun oluşturduğunuz yeni`CallStream-PowerBI`çıktıya sonuç göndermesidir ( ). 

    >[!NOTE]
    >Sahtekarlık algılama `CallStream` öğreticisinde girdiyi adlandırmadıysanız, `CallStream` sorgudaki **FROM** ve **JOIN** yan tümcelerinde adınızı değiştirin.

   ```SQL
   /* Our criteria for fraud:
   Calls made from the same caller to two phone switches in different locations (for example, Australia and Europe) within five seconds */

   SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
   INTO "CallStream-PowerBI"
   FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
   JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime

   /* Where the caller is the same, as indicated by IMSI (International Mobile Subscriber Identity) */
   ON CS1.CallingIMSI = CS2.CallingIMSI

   /* ...and date between CS1 and CS2 is between one and five seconds */
   AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5

   /* Where the switch location is different */
   WHERE CS1.SwitchNum != CS2.SwitchNum
   GROUP BY TumblingWindow(Duration(second, 1))
   ```

4. **Kaydet**'e tıklayın.


## <a name="test-the-query"></a>Sorguyu test etme

Bu bölüm isteğe bağlıdır, ancak önerilir. 

1. TelcoStreaming uygulaması şu anda çalışmıyorsa, aşağıdaki adımları izleyerek başlatın:

    * Komut İstem'ini aç.
    * telcogenerator.exe ve modifiye telcodatagen.exe.config dosyaları nın bulunduğu klasöre gidin.
    * Şu komutu çalıştırın:

       `telcodatagen.exe 1000 .2 2`

2. Akış Analizi işinizin **Sorgu** sayfasında, girişin yanındaki `CallStream` noktalarını tıklatın ve ardından **girişten Örnek verileri**seçin.

3. Üç dakikalık veri istediğinizi belirtin ve **Tamam'ı**tıklatın. Veri örneğinin alındığını belirten bildirim gelene kadar bekleyin.

4. **Test'i** tıklatın ve sonuçları gözden geçirin.

## <a name="run-the-job"></a>İşi çalıştırma

1. TelcoStreaming uygulamasının çalışır durumda olduğundan emin olun.

2. Akış Analizi işin için **Genel Bakış** sayfasına gidin ve **Başlat'ı**seçin.

    ![Akış Analizi işini başlatın](./media/stream-analytics-power-bi-dashboard/stream-analytics-sa-job-start-output.png)

Streaming Analytics işin, gelen akışta sahte aramalar aramaya başlar. İş ayrıca Power BI'de veri kümesi ni ve tabloyu oluşturur ve onlara sahte aramalar hakkında veri göndermeye başlar.


## <a name="create-the-dashboard-in-power-bi"></a>Power BI'de pano oluşturma

1. [Powerbi.com](https://powerbi.com) gidin ve iş veya okul hesabınızla oturum açın. Akış Analizi iş sorgusu sonuçları ortaya çıkarsa, veri kümenizin zaten oluşturulduğunu görürsünüz:

    ![Power BI'de veri kümesi konumunu akışla](./media/stream-analytics-power-bi-dashboard/stream-analytics-streaming-dataset.png)

2. Çalışma alanınızda ** + &nbsp;Oluştur'u**tıklatın.

    ![Power BI çalışma alanında Oluştur düğmesi](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard.png)

3. Yeni bir pano oluşturun `Fraudulent Calls`ve adlandırın.

    ![Bir pano oluşturun ve Power BI çalışma alanında bir ad verin](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard-name.png)

4. Pencerenin üst kısmında, **döşeme ekle'yi**tıklatın, **ÖZEL AKTARıM VERİlerİ'ni**tıklatın ve sonra **İleri'yi**tıklatın.

    ![Power BI'de özel akış veri seti döşemesi](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

5. **DATSETS'inaltında,** veri kümenizi seçin ve **sonra İleri'yi**tıklatın.

    ![Power BI'de akış veri setiniz](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

6. **Visualization Type**altında, **Kart'ı**seçin ve **ardından Alanlar** listesinde sahte **aramaları**seçin.

    ![Yeni döşeme için görselleştirme ayrıntıları](./media/stream-analytics-power-bi-dashboard/add-fraudulent-calls-tile.png)

7. **İleri**'ye tıklayın.

8. Başlık ve altyazı gibi döşeme ayrıntılarını doldurun.

    ![Yeni döşeme için başlık ve altyazı](./media/stream-analytics-power-bi-dashboard/pbi-new-tile-details.png)

9. **Uygula**’ya tıklayın.

    Şimdi bir dolandırıcılık sayacı var!

    ![Power BI panosunda dolandırıcılık sayacı](./media/stream-analytics-power-bi-dashboard/power-bi-fraud-counter-tile.png)

8. Döşeme eklemek için adımları yeniden izleyin (adım 4'ten başlayarak). Bu kez, aşağıdakileri yapın:

    * **Visualization Type'a**ulaştığınızda, **Satır grafiğini**seçin. 
    * Eksen ekleyin ve **windowend** seçeneğini belirleyin. 
    * Değer ekleyip **fraudulentcalls** seçeneğini belirleyin.
    * **Görüntülenecek zaman penceresini** için son 10 dakikayı seçin.

      ![Power BI'de çizgi grafiği için döşeme oluşturma](./media/stream-analytics-power-bi-dashboard/pbi-create-tile-line-chart.png)

9. **İleri'yi**tıklatın, başlık ve altyazı ekleyin ve **Uygula'yı**tıklatın.

     Power BI panosu artık akış verilerinde algılanan sahte aramalarla ilgili iki veri görünümü sağlar.

     ![Hileli aramalar için iki kutucuk gösteren Bitmiş Güç BI panosu](./media/stream-analytics-power-bi-dashboard/pbi-dashboard-fraudulent-calls-finished.png)


## <a name="learn-more-about-power-bi"></a>Power BI hakkında daha fazla bilgi edinin

Bu öğretici, bir veri kümesi için yalnızca birkaç tür görselleştirmenin nasıl oluşturulabildiğini gösterir. Power BI, kuruluşunuz için başka müşteri iş zekası araçları oluşturmanıza yardımcı olabilir. Daha fazla fikir için aşağıdaki kaynaklara bakın:

* Power BI panosunun başka bir örneği için, Power BI video [ile başlarken](https://youtu.be/L-Z_6P56aas?t=1m58s) izleyin.
* Streaming Analytics iş çıktısını Power BI'ye yapılandırma kalım ve Power BI gruplarını kullanma hakkında daha fazla bilgi için [Stream Analytics çıktıları](stream-analytics-define-outputs.md) makalesinin [Power BI](stream-analytics-define-outputs.md#power-bi) bölümünü inceleyin. 
* Power BI'yi genel olarak kullanma hakkında bilgi için Power [BI'deki Panolar'a](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/)bakın.


## <a name="learn-about-limitations-and-best-practices"></a>Sınırlamalar ve en iyi uygulamalar hakkında bilgi edinin
Şu anda, Güç BI kabaca saniyede bir kez çağrılabilir. Akış görselleri 15 KB'lik paketleri destekler. Bunun ötesinde, akış görselleri başarısız (ancak itme çalışmaya devam eder). Bu sınırlamalar nedeniyle Power BI, Azure Akış Analizi'nin önemli bir veri yükü azaltma yaptığı durumlara en doğal şekilde kendini ödünç verir. Veri itme nin saniyede en fazla bir itme olduğundan ve sorgunuzun iş verme gereksinimlerine katkıda olduğundan emin olmak için Yuvarlanma penceresi veya Atlamalı pencere kullanmanızı öneririz.

Pencerenize saniyeler içinde vermek için değeri hesaplamak için aşağıdaki denklemi kullanabilirsiniz:

![Saniye cinsinden pencere vermek için hesaplama değeri denklemi](./media/stream-analytics-power-bi-dashboard/compute-window-seconds-equation.png)  

Örnek:

* Bir saniyelik aralıklarla veri gönderen 1.000 aygıtınız var.
* Saatte 1.000.000 satırı destekleyen Power BI Pro SKU'yu kullanıyorsunuz.
* Aygıt başına ortalama veri miktarını Power BI'ye yayımlamak istiyorsunuz.

Sonuç olarak, denklem olur:

![Örnek ölçütlere dayalı denklem](./media/stream-analytics-power-bi-dashboard/power-bi-example-equation.png)  

Bu yapılandırma göz önüne alındığında, özgün sorguyu aşağıdakiyle değiştirebilirsiniz:

```SQL
    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO "CallStream-PowerBI"
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl
```

### <a name="renew-authorization"></a>Yetkilendirmeyi yenileme
İşinizin oluşturulmasından veya en son kimlik doğrulanmasından sonra parola değiştiyse, Power BI hesabınızı yeniden doğrulamanız gerekir. Azure Çok Faktörlü Kimlik Doğrulaması Azure Etkin Dizin (Azure AD) kiracınızda yapılandırıldıysa, Power BI yetkilendirmesini her iki haftada bir yenilemeniz gerekir. Yenilemezseniz, iş çıktısı eksikliği veya işlem günlüklerinde bir `Authenticate user error` eksiklik gibi belirtiler görebilirsiniz.

Benzer şekilde, bir iş belirteç süresi dolduktan sonra başlarsa, bir hata oluşur ve iş başarısız olur. Bu sorunu gidermek için çalışan işi durdurun ve Power BI çıkışınıza gidin. Veri kaybını önlemek için **yetkilendirme yi yenile** bağlantısını seçin ve ardından son **durdurulan süreden**işinizi yeniden başlatın.

Yetkilendirme Power BI ile yenilendikten sonra, sorunun çözüldüğünü yansıtmak için yetkilendirme alanında yeşil bir uyarı görüntülenir.

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Akışı Analizi forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akışı Analytics sorgu dili başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akışı Analytics Yönetimi REST API başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
