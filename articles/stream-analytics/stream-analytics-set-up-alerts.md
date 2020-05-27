---
title: Azure Stream Analytics işleri için izleme uyarılarını ayarlama
description: Bu makalede, Azure Stream Analytics işleri için izleme ve uyarıları ayarlamak üzere Azure portal nasıl kullanılacağı açıklanır.
author: jseb225
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 8219fba44be608a9fd31139a89c8dac2cc3e3082
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835419"
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Azure Stream Analytics işleri için uyarıları ayarlama

İş sorunsuz çalıştığından emin olmak için Azure Stream Analytics işinizi izlemeniz önemlidir. Bu makalede izlenmesi gereken yaygın senaryolar için uyarıların nasıl ayarlanacağı açıklanır. 

Işlem günlüğü verilerinden, Portal üzerinden ve [programlama](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a)yoluyla ölçümler hakkında kurallar tanımlayabilirsiniz.

## <a name="set-up-alerts-in-the-azure-portal"></a>Azure portal uyarıları ayarlama
### <a name="get-alerted-when-a-job-stops-unexpectedly"></a>Bir iş beklenmedik bir şekilde durdurulduğunda uyarı alın

Aşağıdaki örnekte, işiniz başarısız durumuna girdiğinde için uyarıların nasıl ayarlanacağı gösterilmektedir. Bu uyarı tüm işler için önerilir.

1. Azure portal, bir uyarı oluşturmak istediğiniz Stream Analytics işi açın.

2. **İş** sayfasında **izleme** bölümüne gidin.  

3. **Ölçümler**' i ve sonra **Yeni uyarı kuralı**' nı seçin.

   ![Azure portal Stream Analytics Uyarıları kurulumu](./media/stream-analytics-set-up-alerts/stream-analytics-set-up-alerts.png)  

4. Stream Analytics iş adınız **kaynak**altında otomatik olarak görünmelidir. **Koşul Ekle**' ye tıklayın ve **sinyal mantığını Yapılandır**altında **tüm yönetim işlemlerini** seçin.

   ![Stream Analytics uyarının sinyal adını seçin](./media/stream-analytics-set-up-alerts/stream-analytics-condition-signal.png)  

5. **Sinyal mantığını Yapılandır**altında **olay düzeyini** **Tümü** olarak değiştirin ve **durumu** **başarısız**olarak değiştirin. **Olayı boş olarak başlattığını** bırakın ve **bitti**' yi seçin.

   ![Stream Analytics uyarısı için sinyal mantığını yapılandırma](./media/stream-analytics-set-up-alerts/stream-analytics-configure-signal-logic.png) 

6. Mevcut bir eylem grubu seçin veya yeni bir grup oluşturun. Bu örnekte, **sahip** Azure Resource Manager rolüne sahip kullanıcılara bir e-posta gönderen bir **e** -posta eylemiyle **TIDashboardGroupActions** adlı yeni bir eylem grubu oluşturulmuştur.

   ![Azure Akış Analizi işi için uyarı ayarlama](./media/stream-analytics-set-up-alerts/stream-analytics-add-group-email-action.png)

7. **Kaynak**, **koşul**ve **eylem gruplarının** her birinin bir girişi olmalıdır. Uyarıların tetiklenmesi için, tanımlanan koşulların karşılanması gerektiğini unutmayın. Örneğin, son 15 dakika için ölçümün ortalama değerini her 5 dakikada bir ölçebilirsiniz.

   ![Stream Analytics uyarı kuralı oluştur](./media/stream-analytics-set-up-alerts/stream-analytics-create-alert-rule-2.png)

   **Uyarı ayrıntılarına** bir **Uyarı kuralı adı**, **açıklaması**ve **kaynak grubunuz** ekleyin ve Stream Analytics işiniz için kural oluşturmak üzere **Uyarı kuralı oluştur** ' a tıklayın.

   ![Stream Analytics uyarı kuralı oluştur](./media/stream-analytics-set-up-alerts/stream-analytics-create-alert-rule.png)
   
## <a name="scenarios-to-monitor"></a>İzlenecek senaryolar

Stream Analytics işinizin performansını izlemek için aşağıdaki uyarılar önerilir. Bu ölçümler, son 5 dakikalık dönemde dakikada bir değerlendirilmelidir.

|Ölçüm|Koşul|Zaman toplama|Eşik|Düzeltici eylemler|
|-|-|-|-|-|
|SU kullanımı yüzdesi|Büyüktür|Maksimum|80|% SU kullanımını artıran birden fazla faktör vardır. Sorgu paralelleştirme ile ölçeklendirebilir veya akış birimi sayısını artırabilirsiniz. Daha fazla bilgi için bkz. [Azure Stream Analytics'te sorgu paralelleştirmesinden yararlanma](stream-analytics-parallelization.md).|
|Çalışma zamanı hataları|Büyüktür|Toplam|0|Etkinlik veya kaynak günlüklerini inceleyin ve giriş, sorgu veya çıkışlara uygun değişiklikler yapın.|
|Filigran gecikmesi|Büyüktür|Maksimum|Bu ölçümün son 15 dakika boyunca ortalama değeri, geç varış toleransına (saniye cinsinden) fazla olduğunda. Geç varış toleransını değiştirmediyse, varsayılan değer 5 saniyeye ayarlanır.|SUs sayısını artırmayı deneyin veya sorgunuzu paralelleştirme. SUs hakkında daha fazla bilgi için bkz. [akış birimlerini anlama ve ayarlama](stream-analytics-streaming-unit-consumption.md#how-many-sus-are-required-for-a-job). Sorgunuzu paralel hale getirme hakkında daha fazla bilgi için bkz. [Azure Stream Analytics sorgu paralelleştirme özelliğinden yararlanma](stream-analytics-parallelization.md).|
|Giriş seri kaldırma hataları|Büyüktür|Toplam|0|Etkinlik veya kaynak günlüklerini inceleyin ve girişte uygun değişiklikleri yapın. Kaynak günlükleri hakkında daha fazla bilgi için bkz. [kaynak günlüklerini kullanarak Azure Stream Analytics sorunlarını giderme](stream-analytics-job-diagnostic-logs.md)|

## <a name="get-help"></a>Yardım alın

Azure portal uyarılarını yapılandırma hakkında daha fazla ayrıntı için bkz. [uyarı bildirimleri alma](../azure-monitor/platform/alerts-overview.md).  

Daha fazla yardım için, [Azure Stream Analytics Için Microsoft Q&soru sayfasını](https://docs.microsoft.com/answers/topics/azure-stream-analytics.html)deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-get-started.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

