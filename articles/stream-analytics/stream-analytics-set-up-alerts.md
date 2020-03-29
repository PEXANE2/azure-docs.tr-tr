---
title: Azure Akış Analizi işleri için izleme uyarıları ayarlama
description: Bu makalede, Azure Akış Analizi işleri için izleme ve uyarılar ayarlamak için Azure portalının nasıl kullanılacağı açıklanmaktadır.
author: jseb225
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 836b7a489e3c73d745b128cbbc0c3566220ac409
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75458726"
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Azure Akış Analizi işleri için uyarılar ayarlama

İşin sorunsuz bir şekilde sürekli çalıştığından emin olmak için Azure Akış Analizi işinizi izlemeniz önemlidir. Bu makalede, izlenmesi gereken yaygın senaryolar için uyarıların nasıl ayarlanış edildiği açıklanmaktadır. 

Portal aracılığıyla İşlem Günlükleri verilerinden ölçümlerle ilgili kuralları ve [programlı olarak](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a)tanımlayabilirsiniz.

## <a name="set-up-alerts-in-the-azure-portal"></a>Azure portalında uyarılar ayarlama
### <a name="get-alerted-when-a-job-stops-unexpectedly"></a>Bir iş beklenmedik bir şekilde durduğunda uyarı alın

Aşağıdaki örnek, işiniz başarısız bir duruma girdiğinde nasıl uyarılar ayarlayınızı gösterir. Bu uyarı tüm işler için önerilir.

1. Azure portalında, uyarı oluşturmak istediğiniz Akış Analizi işini açın.

2. **İş** sayfasında, **İzleme** bölümüne gidin.  

3. **Ölçümler'i**ve ardından **Yeni uyarı kuralını**seçin.

   ![Azure portalı Stream Analytics kurulumu uyarıları](./media/stream-analytics-set-up-alerts/stream-analytics-set-up-alerts.png)  

4. Stream Analytics iş adınız kaynak **altında**otomatik olarak görünmelidir. **Koşul Ekle'yi**tıklatın ve **Sinyal mantığını Yapılandır'ın**altındaki **Tüm Yönetim işlemlerini** seçin.

   ![Stream Analytics uyarısı için sinyal adını seçin](./media/stream-analytics-set-up-alerts/stream-analytics-condition-signal.png)  

5. **Yapılandırılan sinyal mantığı**altında, **Olay Düzeyini** **Tümü** olarak değiştirin ve **Durum'u** Başarısız olarak **değiştirin.** **Boş tarafından başlatılan Olay** bırak ve **Bitti'yi**seçin.

   ![Stream Analytics uyarısı için sinyal mantığını yapılandırma](./media/stream-analytics-set-up-alerts/stream-analytics-configure-signal-logic.png) 

6. Varolan bir eylem grubu seçin veya yeni bir grup oluşturun. Bu örnekte, **Sahibi** Azure Kaynak Yöneticisi Rolü'ne sahip kullanıcılara e-posta gönderen bir **E-posta** eylemiyle **TIDashboardGroupActions** adlı yeni bir eylem grubu oluşturuldu.

   ![Azure Akış Analizi işi için uyarı ayarlama](./media/stream-analytics-set-up-alerts/stream-analytics-add-group-email-action.png)

7. **KAYNAK**, **KOŞUL**ve **EYLEM GRUPLARI** her bir girişi olmalıdır. Uyarıların ateş edilebilmesi için tanımlanan koşulların karşılanması gerektiğini unutmayın. Örneğin, son 15 dakika için ölçümün ortalama değerini her 5 dakikada bir ölçebilirsiniz.

   ![Akış Analizi uyarı kuralı oluşturun](./media/stream-analytics-set-up-alerts/stream-analytics-create-alert-rule-2.png)

   **ALERT DETAILS'na** bir **Uyarı kuralı adı,** **Açıklama**ve **Kaynak Grubu'nu** ekleyin ve Akış Analizi işinizin kuralını oluşturmak için **uyarı kuralını oluştur'u** tıklatın.

   ![Akış Analizi uyarı kuralı oluşturun](./media/stream-analytics-set-up-alerts/stream-analytics-create-alert-rule.png)
   
## <a name="scenarios-to-monitor"></a>İzlenecek senaryolar

Stream Analytics işinizin performansını izlemek için aşağıdaki uyarılar önerilir. Bu ölçümler son 5 dakikalık periyotta her dakika değerlendirilmelidir.

|Ölçüm|Koşul|Zaman Toplama|Eşik|Düzeltici Eylemler|
|-|-|-|-|-|
|SU% Kullanımı|Büyüktür|Maksimum|80|SU% Kullanımını artıran birden fazla faktör vardır. Sorgu paralellemesi ile ölçeklendirebilir veya akış birimi sayısını artırabilirsiniz. Daha fazla bilgi için bkz. [Azure Stream Analytics'te sorgu paralelleştirmesinden yararlanma](stream-analytics-parallelization.md).|
|Çalışma zamanı hataları|Büyüktür|Toplam|0|Etkinlik veya tanı günlüklerini inceleyin ve giriş, sorgu veya çıktılarda uygun değişiklikleri yapın.|
|Filigran gecikmesi|Büyüktür|Maksimum|Bu ölçümün son 15 dakikadaki ortalama değeri geç varış toleransından (saniye cinsinden) büyük olduğunda. Geç varış toleransını modifiye etmediyseniz, varsayılan değer 5 saniye olarak ayarlanır.|SUs sayısını artırmayı veya sorgunuzu paralelleştirmeyi deneyin. SUs hakkında daha fazla bilgi için Akış [Birimlerini Anlayın ve Ayarlayın'](stream-analytics-streaming-unit-consumption.md#how-many-sus-are-required-for-a-job)a bakın. Sorgunuzu paralelleştirme hakkında daha fazla bilgi için [Azure Akış Analizi'nde Kaldıraç sorgusu paralellemesi](stream-analytics-parallelization.md)bölümüne bakın.|
|Giriş deserialization hataları|Büyüktür|Toplam|0|Etkinliği veya tanı lama günlüklerini inceleyin ve girişte uygun değişiklikleri yapın. Tanılama günlükleri hakkında daha fazla bilgi için [tanılama günlüklerini kullanarak Azure Akış Analizi Sorun](stream-analytics-job-diagnostic-logs.md) Giderme bölümüne bakın|

## <a name="get-help"></a>Yardım alın

Azure portalındaki uyarıları yapılandırma hakkında daha fazla ayrıntı için uyarı [bildirimleri alın'](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)a bakın.  

Daha fazla yardım için [Azure Akışı Analizi forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-get-started.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

