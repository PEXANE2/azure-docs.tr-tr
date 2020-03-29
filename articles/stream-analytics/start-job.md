---
title: Azure Akış Analizi işine nasıl başlarsın?
description: Bu makalede, Azure portalı, PowerShell ve Visual Studio'dan bir Akış Analizi işinin nasıl başlatılacak olduğu açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/03/2019
ms.openlocfilehash: c393eb782c2ff16eb5b3e5967b39938dfe2f1534
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75426461"
---
# <a name="how-to-start-an-azure-stream-analytics-job"></a>Azure Akış Analizi işine nasıl başlarsın?

Azure Akış Analizi işinize Azure portalı, Visual Studio ve PowerShell'i kullanarak başlayabilirsiniz. Bir işe başladığınızda, işin çıktı oluşturmaya başlaması için bir zaman seçersiniz. Azure portalı, Visual Studio ve PowerShell'in her birinin başlangıç saatini ayarlamak için farklı yöntemleri vardır. Bu yöntemler aşağıda açıklanmıştır.

## <a name="start-options"></a>Başlangıç seçenekleri
Bir işe başlamak için aşağıdaki üç seçenek kullanılabilir. Aşağıda belirtilen tüm zamanların [TIMESTAMP BY'de](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics)belirtilenler olduğunu unutmayın. TIMESTAMP BY belirtilmemişse, varış saati kullanılacaktır.
* **Şimdi**: Çıktı olay akışının başlangıç noktasını, iş başlatıldığındaolduğu gibi yapar. Zamansal bir işleç kullanılırsa (örneğin zaman penceresi, LAG veya JOIN), Azure Akış Analizi giriş kaynağındaki verilere otomatik olarak bakar. Örneğin, bir işe "Şimdi" olarak başlarsanız ve sorgunuz 5 dakikalık Bir Yuvarlanma Penceresi kullanırsa, Azure Akış Analizi girişte 5 dakika öncesine ait verileri arar.
İlk olası çıktı olayı, geçerli saate eşit veya daha büyük bir zaman damgasına sahip olur ve ASA, çıktıya mantıksal olarak katkıda bulunabilecek tüm giriş olaylarının hesaba katıldığını garanti eder. Örneğin, kısmi pencereli agregalar oluşturulur. Her zaman toplam değerin tamamıdır.

* **Özel**: Çıktının başlangıç noktasını seçebilirsiniz. **Şimdi** seçeneğine benzer şekilde, zamansal bir operatör kullanılırsa Azure Akış Analizi bu tarihten önce verileri otomatik olarak okur 

* **En son durduğunda.** Bu seçenek, iş daha önce başlatıldığında kullanılabilir, ancak el ile durduruldu veya başarısız oldu. Bu seçeneği seçerken Azure Stream Analytics, hiçbir veri kaybolmaması için işi yeniden başlatmak için son çıktı süresini kullanır. Önceki seçeneklerde olduğu gibi, geçici bir işleç kullanılırsa Azure Akış Analizi bu tarihten önce verileri otomatik olarak okur. Birkaç giriş bölümü farklı zamana sahip olabileceğinden, tüm bölümlerin en erken durma süresi kullanılır, sonuç olarak çıktıda bazı yinelemeler görülebilir. Tam olarak bir kez işleme hakkında daha fazla bilgi sayfa [Olay Teslim Garantileri](https://docs.microsoft.com/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics)mevcuttur.


## <a name="azure-portal"></a>Azure portalında

Azure portalında işinize gidin ve genel bakış sayfasında **Başlat'ı** seçin. İş **çıktısı başlangıç saatini** seçin ve ardından **Başlat'ı**seçin.

**İş çıktısı başlangıç saati**için seçeneklerden birini seçin. Seçenekler *Şimdi*, *Özel*, ve, iş daha önce çalıştırıldı ise, Ne zaman *son durduruldu*. Bu seçenekler hakkında daha fazla bilgi için yukarıya bakın.

## <a name="visual-studio"></a>Visual Studio

İş görünümünde, işe başlamak için yeşil ok düğmesini seçin. İş **Çıktısı Başlangıç Modunu** ayarlayın ve **Başlat'ı**seçin. İş durumu **Running**olarak değişecektir.

**İş Çıktısı Başlangıç Modu**için üç seçenek vardır: *JobStartTime*, *CustomTime*ve *LastOutputEventTime*. Bu özellik yoksa, varsayılan *İşBaşlangıç*Zamanı'dır. Bu seçenekler hakkında daha fazla bilgi için yukarıya bakın.


## <a name="powershell"></a>PowerShell

PowerShell'i kullanarak işinize başlamak için aşağıdaki cmdlet'i kullanın:

```powershell
Start-AzStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode 'JobStartTime'
```

**OutputStartMode**için üç seçenek vardır: *JobStartTime*, *CustomTime*, ve *LastOutputEventTime*. Bu özellik yoksa, varsayılan *İşBaşlangıç*Zamanı'dır. Bu seçenekler hakkında daha fazla bilgi için yukarıya bakın.

`Start-AzStreamAnalyitcsJob` cmdlet hakkında daha fazla bilgi için [Start-AzStreamAnalyticsJob referansını](/powershell/module/az.streamanalytics/start-azstreamanalyticsjob)görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)
* [Hızlı başlangıç: Azure PowerShell'i kullanarak Bir Akış Analizi işi oluşturun](stream-analytics-quick-create-powershell.md)
* [Hızlı başlangıç: Visual Studio için Azure Stream Analytics araçlarını kullanarak bir Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
