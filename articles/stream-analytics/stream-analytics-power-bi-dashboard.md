---
title: Azure Stream Analytics ile pano tümleştirmesi Power BI
description: Bu makalede, bir Azure Stream Analytics işinden verileri görselleştirmek için gerçek zamanlı Power BI panonun nasıl kullanılacağı açıklanır.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 76f5c1f0cd3186244e9a262358c9c9a652a73fdb
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75431642"
---
# <a name="stream-analytics-and-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Stream Analytics ve Power BI: veri akışı verileri için gerçek zamanlı analiz panosu

Azure Stream Analytics, önde gelen iş zekası araçlarından birini [Microsoft Power BI](https://powerbi.com/)avantajlarından yararlanmanıza olanak sağlar. Bu makalede, Azure Stream Analytics işleriniz için çıkış olarak Power BI kullanarak iş zekası araçları oluşturma hakkında bilgi edineceksiniz. Ayrıca, gerçek zamanlı bir pano oluşturmayı ve kullanmayı öğreneceksiniz.

Bu makale Stream Analytics [gerçek zamanlı sahtekarlık algılama](stream-analytics-real-time-fraud-detection.md) öğreticisiyle devam etmektedir. Bu öğreticide oluşturulan iş akışını oluşturur ve bir akış analizi işi tarafından algılanan sahte telefon çağrılarını görselleştirebilmeniz için Power BI bir çıktı ekler. 

Bu senaryoyu gösteren [bir video](https://www.youtube.com/watch?v=SGUpT-a99MA) izleyebilirsiniz.


## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunlara sahip olduğunuzdan emin olun:

* Bir Azure hesabı.
* Power BI için bir hesap. Bir iş hesabı veya okul hesabı kullanabilirsiniz.
* [Gerçek zamanlı sahtekarlık algılama](stream-analytics-real-time-fraud-detection.md) öğreticisinin tamamlanmış bir sürümü. Öğretici, kurgusal telefon araması meta verileri üreten bir uygulama içerir. Öğreticide bir olay hub 'ı oluşturur ve akış telefon araması verilerini Olay Hub 'ına gönderirsiniz. Sahte çağrıları algılayan bir sorgu (farklı konumlarda aynı anda aynı sayıdan gelen çağrılar) yazarsınız. 


## <a name="add-power-bi-output"></a>Power BI çıkışı Ekle
Gerçek zamanlı sahtekarlık algılama öğreticisinde, çıkış Azure Blob depolama alanına gönderilir. Bu bölümde, Power BI bilgi gönderen bir çıktı eklersiniz.

1. Azure portal, daha önce oluşturduğunuz Akış Analizi işini açın. Önerilen adı kullandıysanız, iş `sa_frauddetection_job_demo`olarak adlandırılır.

2. Sol menüde, **iş topolojisi**altında **çıktılar** ' i seçin. Ardından **+ Ekle** ' yi seçin ve açılan menüden **Power BI** seçin.

3. **+ Ekle** > **Power BI**'ı seçin. Ardından formu aşağıdaki bilgilerle doldurun ve **Yetkilendir**'i seçin:

   |**Ayar**  |**Önerilen değer**  |
   |---------|---------|
   |Çıkış diğer adı  |  CallStream-PowerBI  |
   |Veri kümesi adı  |   sa veri kümesi  |
   |Tablo adı |  sahte çağrılar  |

   ![Stream Analytics çıktı yapılandırma](media/stream-analytics-power-bi-dashboard/configure-stream-analytics-output.png)

   > [!WARNING]
   > Power BI, Stream Analytics işinde belirttikleriyle aynı adlara sahip bir veri kümesi ve tablo içeriyorsa, mevcut olanların üzerine yazılır.
   > Bu veri kümesini ve tabloyu Power BI hesabınızda açıkça oluşturmamanız önerilir. Stream Analytics işinizi başlattığınızda otomatik olarak oluşturulur ve iş, Power BI içine çıktı olarak başlatılır. İş sorgunuz herhangi bir sonuç döndürmezse, veri kümesi ve tablo oluşturulmaz.
   >

4. **Yetkilendir**'i seçtiğinizde bir açılır pencere görünür ve Power BI hesabınızda kimlik doğrulaması için sizden kimlik bilgilerini sağlamanız istenir. Yetkilendirme başarılı olduktan sonra **Kaydet** seçeneğine tıklayarak ayarları kaydedin.

8. **Oluştur**'a tıklayın.

Veri kümesi aşağıdaki ayarlarla oluşturulur:

* **Defaultretentionpolicy: Basicfıfo** -Data, en fazla 200.000 satır içeren FIFO.
* **DefaultMode: pushStreaming** -DataSet, hem akış kutucukları hem de geleneksel rapor tabanlı görselleri (gönderim olarak da bilinir) destekler.

Şu anda, diğer bayraklarla veri kümeleri oluşturamazsınız.

Power BI veri kümeleri hakkında daha fazla bilgi için [Power BI REST API](https://msdn.microsoft.com/library/mt203562.aspx) başvurusuna bakın.


## <a name="write-the-query"></a>Sorguyu yazma

1. **Çıktılar** dikey penceresini kapatın ve iş dikey penceresine dönün.

2. **Sorgu** kutusuna tıklayın. 

3. Aşağıdaki sorguyu girin. Bu sorgu, sahtekarlık algılama öğreticisinde oluşturduğunuz kendi kendine JOIN sorgusuna benzer. Fark, bu sorgunun sonuçları oluşturduğunuz yeni çıktıya (`CallStream-PowerBI`) göndermesi gerektiğidir. 

    >[!NOTE]
    >Sahtekarlık algılama öğreticisinde giriş `CallStream` adı belirtmediyseniz, sorgudaki **from** ve **JOIN** yan tümcelerinde `CallStream` adınızı yerine koyun.

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

4. **Save (Kaydet)** düğmesine tıklayın.


## <a name="test-the-query"></a>Sorguyu test etme

Bu bölüm isteğe bağlıdır, ancak önerilir. 

1. TelcoStreaming uygulaması şu anda çalışmıyorsa, şu adımları izleyerek başlatın:

    * Komut Istemi 'ni açın.
    * Telcogenerator. exe ve Modified telcodatagen. exe. config dosyalarının olduğu klasöre gidin.
    * Şu komutu çalıştırın:

       `telcodatagen.exe 1000 .2 2`

2. Stream Analytics işinizin **sorgu** sayfasında, `CallStream` girişinin yanındaki noktalara tıklayın ve sonra **girişten örnek veriler**' i seçin.

3. Üç dakikalık verileri istediğinizi belirtip **Tamam**' a tıklayın. Veri örneğinin alındığını belirten bildirim gelene kadar bekleyin.

4. **Test** ' e tıklayın ve sonuçları gözden geçirin.

## <a name="run-the-job"></a>İşi çalıştırma

1. TelcoStreaming uygulamasının çalıştığından emin olun.

2. Stream Analytics işiniz için **genel bakış** sayfasına gidin ve **Başlat**' ı seçin.

    ![Stream Analytics işini Başlat](./media/stream-analytics-power-bi-dashboard/stream-analytics-sa-job-start-output.png)

Akış Analizi işiniz, gelen akıştaki sahte çağrılar aramaya başlar. İş, Power BI veri kümesini ve tabloyu da oluşturur ve bunlara yönelik sahte çağrılar hakkında veri göndermeye başlar.


## <a name="create-the-dashboard-in-power-bi"></a>Panoyu Power BI oluşturma

1. [Powerbi.com](https://powerbi.com) adresine gidin ve iş veya okul hesabınızla oturum açın. Stream Analytics iş sorgusu sonuçları çıktıdaysa, veri kümenizin zaten oluşturulduğunu görürsünüz:

    ![Power BI 'de akış veri kümesi konumu](./media/stream-analytics-power-bi-dashboard/stream-analytics-streaming-dataset.png)

2. Çalışma alanınızda **+&nbsp;oluştur**' a tıklayın.

    ![Power BI çalışma alanındaki oluştur düğmesi](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard.png)

3. Yeni bir pano oluşturun ve `Fraudulent Calls`adlandırın.

    ![Bir pano oluşturun ve Power BI çalışma alanında bir ad verin](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard-name.png)

4. Pencerenin üst kısmında **kutucuk Ekle**' ye tıklayın, **özel akış verileri**' ni seçin ve ardından **İleri**' ye tıklayın.

    ![Power BI 'de özel akış veri kümesi kutucuğu](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

5. **Datkümeleriniz**altında veri kümenizi seçip **İleri**' ye tıklayın.

    ![Akış veri kümeniz Power BI](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

6. **Görselleştirme türü**altında **kart**' ı seçin ve ardından **alanlar** listesinde **fraudulentcalls**' yi seçin.

    ![Yeni kutucuk için görselleştirme ayrıntıları](./media/stream-analytics-power-bi-dashboard/add-fraudulent-calls-tile.png)

7. **İleri**’ye tıklayın.

8. Başlık ve alt başlık gibi kutucuk ayrıntılarını girin.

    ![Yeni kutucuk için başlık ve alt başlık](./media/stream-analytics-power-bi-dashboard/pbi-new-tile-details.png)

9. **Uygula**'ya tıklayın.

    Artık bir sahtekarlık sayaçından sahipsiniz!

    ![Power BI panosunda sahtekarlık sayacı](./media/stream-analytics-power-bi-dashboard/power-bi-fraud-counter-tile.png)

8. Bir kutucuk eklemek için adımları izleyin (4. adımdan itibaren). Bu kez, şunları yapın:

    * **Görselleştirme türüne**geldiğinizde **çizgi grafik**' i seçin. 
    * Eksen ekleyin ve **windowend** seçeneğini belirleyin. 
    * Değer ekleyip **fraudulentcalls** seçeneğini belirleyin.
    * **Görüntülenecek zaman penceresini** için son 10 dakikayı seçin.

      ![Power BI çizgi grafik için kutucuk oluştur](./media/stream-analytics-power-bi-dashboard/pbi-create-tile-line-chart.png)

9. **İleri**' ye tıklayın, başlık ve alt başlık ekleyin ve **Uygula**' ya tıklayın.

     Power BI panosu artık akış verilerinde algılanan sahte çağrılar hakkında iki veri görünümü sağlar.

     ![Sahte çağrılar için iki kutucuk gösteren Power BI panosu tamamlandı](./media/stream-analytics-power-bi-dashboard/pbi-dashboard-fraudulent-calls-finished.png)


## <a name="learn-more-about-power-bi"></a>Power BI hakkında bilgi alın

Bu öğreticide, bir veri kümesi için yalnızca birkaç görselleştirme türü oluşturma gösterilmektedir. Power BI, kuruluşunuz için diğer müşteri iş zekası araçları oluşturmanıza yardımcı olabilir. Daha fazla fikir için aşağıdaki kaynaklara bakın:

* Power BI panosunun başka bir örneği için, [kullanmaya başlama Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) videosunu izleyin.
* Akış Analizi iş çıkışını Power BI ve Power BI grupları kullanarak yapılandırma hakkında daha fazla bilgi için [Stream Analytics çıkışları](stream-analytics-define-outputs.md) makalesinin [Power BI](stream-analytics-define-outputs.md#power-bi) bölümünü gözden geçirin. 
* Power BI kullanımı hakkında daha fazla bilgi için bkz. [Power BI panolar](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).


## <a name="learn-about-limitations-and-best-practices"></a>Sınırlamalar ve en iyi uygulamalar hakkında bilgi edinin
Şu anda, Power BI saniyede kabaca bir kez çağrılabilir. Akış görselleri 15 KB 'lik paketleri destekler. Bunun ötesinde, akış görselleri başarısız olur (ancak gönderim çalışmaya devam eder). Bu sınırlamalar nedeniyle, Azure Stream Analytics önemli bir veri yükleme azalmasıyla ilgili olarak en doğal olarak Power BI. Veri gönderimi 'nin saniyede en çok bir gönderim olduğundan ve sorgunuzun işleme gereksinimleri içinde yer aldığından emin olmak için, atlayan bir pencere veya Hoppıng penceresi kullanmanızı öneririz.

Aşağıdaki denklemi, pencerenize saniye cinsinden vermek için değeri hesaplamak üzere kullanabilirsiniz:

![Saniye cinsinden pencere sağlamak için değer hesaplama denklemi](./media/stream-analytics-power-bi-dashboard/compute-window-seconds-equation.png)  

Örneğin:

* Tek saniyelik aralıklarla veri gönderen 1.000 cihazlardır.
* Saat başına 1.000.000 satırı destekleyen Power BI Pro SKU 'SU kullanıyorsunuz.
* Power BI cihaz başına ortalama veri miktarını yayınlamak istiyorsunuz.

Sonuç olarak, denklemi şöyle olur:

![Örnek ölçütlere göre denklem](./media/stream-analytics-power-bi-dashboard/power-bi-example-equation.png)  

Bu yapılandırma verildiğinde, özgün sorguyu aşağıdaki şekilde değiştirebilirsiniz:

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

### <a name="renew-authorization"></a>Yetkilendirmeyi Yenile
İşiniz oluşturulduktan veya en son kimlik doğrulamasından sonra parola değişmişse, Power BI hesabınızı yeniden kimlik doğrulaması yapmanız gerekir. Azure Multi-Factor Authentication Azure Active Directory (Azure AD) kiracınızda yapılandırılırsa, her iki haftada bir Power BI yetkilendirmeyi de yenilemeniz gerekir. Yenilemezseniz, iş çıktısının bulunmaması veya işlem günlüklerinde bir `Authenticate user error` gibi belirtilerle karşılaşabilirsiniz.

Benzer şekilde, bir iş, belirtecin süresi dolduktan sonra başlarsa bir hata oluşur ve iş başarısız olur. Bu sorunu çözmek için, çalıştıran işi durdurun ve Power BI çıktısına gidin. Veri kaybını önlemek için, **Yetkilendirmeyi Yenile** bağlantısını seçin ve ardından **son durdurulma zamanından**sonra işinizi yeniden başlatın.

Yetkilendirme Power BI ile yenilendikten sonra, sorunun çözümlendiğini yansıtmak için yetkilendirme alanında yeşil bir uyarı belirir.

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics sorgu dili başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Stream Analytics yönetim REST API başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
