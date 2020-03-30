---
title: Azure otomatik ölçekle sorun giderme
description: Service Fabric, Virtual Machines, Web Apps ve bulut hizmetlerinde kullanılan Azure otomatik ölçekleme yle ilgili sorunları izleme.
ms.topic: conceptual
ms.date: 11/4/2019
ms.subservice: autoscale
ms.openlocfilehash: 9780cf88070110c4efc13c477d65307aa3985fe5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75751342"
---
# <a name="troubleshooting-azure-autoscale"></a>Azure otomatik ölçekle sorun giderme
 
Azure Monitor otomatik ölçeklendirme, uygulamanızdaki yükü işlemek için doğru miktarda kaynağın çalışmanıza yardımcı olur. Yük artışlarını işlemek için kaynak eklemenize ve boşta oturan kaynakları kaldırarak paradan tasarruf etmenizi sağlar. Seçtiğiniz bir zamanlamaya, sabit tarih saatine veya kaynak ölçümüne göre ölçeklendirebilirsiniz. Daha fazla bilgi için Bkz. [Otomatik Ölçekle reisi genel bakış.](autoscale-overview.md)

Otomatik ölçeklendirme hizmeti, ölçek eylemlerinin neler oluştuğunu ve bu eylemlere yol açan koşulların değerlendirilmesi için ölçümler ve günlükler sağlar. Gibi soruların yanıtlarını bulabilirsiniz:

- Hizmet imneden ölçeklendirildi veya içeri mi?
- Hizmetim neden ölçeklendirilemedi?
- Otomatik ölçeklendirme eylemi neden başarısız oldu?
- Otomatik ölçeklendirme eylemi neden ölçeklendirmek için zaman alıyor?
  
## <a name="autoscale-metrics"></a>Otomatik ölçeklendirme ölçümleri

Otomatik ölçek, işlemini anlamak için [dört ölçüm](metrics-supported.md#microsoftinsightsautoscalesettings) sağlar. 

- **Gözlenen Metrik Değer** - Otomatik ölçek motoru tarafından görüldüğü veya hesaplanan ölçek eylemini yapmak için seçtiğiniz ölçümün değeri. Tek bir otomatik ölçek ayarı birden çok kurala ve dolayısıyla birden çok metrik kaynağa sahip olabileceğinden, boyut olarak "metrik kaynak" kullanarak filtre uygulayabilirsiniz.
- **Metrik Eşik** - Ölçek eylemini yapmak için ayarladığınız eşik. Tek bir otomatik ölçek ayarı birden çok kurala ve dolayısıyla birden çok metrik kaynağa sahip olabileceğinden, boyut olarak "metrik kural" kullanarak filtre uygulayabilirsiniz.
- **Gözlenen Kapasite** - Otomatik Ölçek motorutarafından görülen hedef kaynağın etkin sayısı.
- **Başlatılan Ölçeklendirme Eylemleri**: Otomatik ölçeklendirme altyapısı tarafından başlatılan ölçeği genişletme ve ölçeği daraltma eylemlerinin sayısıdır. Eylemlerde ölçeklendirme ve ölçek lere göre filtre uygulayabilirsiniz.

Yukarıdaki ölçümleri tek bir yerde grafiklemek için [Metrics Explorer'ı](metrics-getting-started.md) kullanabilirsiniz. Grafik göstermelidir:

  - gerçek metrik
  - otomatik ölçek motoru tarafından görülen/hesaplanan metrik
  - bir ölçek eylemi için eşik
  - kapasite değişikliği 

## <a name="example-1---analyzing-a-simple-autoscale-rule"></a>Örnek 1 - Basit bir otomatik ölçek kuralını çözümleme 

Sanal makine ölçeği seti için basit bir otomatik ölçek ayarımız var:

- bir kümenin ortalama CPU yüzdesi 10 dakika boyunca %70'ten büyük olduğunda ölçeklenir 
- kümesinin CPU yüzdesinin 10 dakikadan fazla bir süre için %5'ten az olduğu ölçeklenir. 

Otomatik ölçeklendirme hizmetinden ölçümleri gözden geçirelim.
 
![Sanal makine ölçeği set yüzdesi CPU örneği](media/autoscale-troubleshoot/autoscale-vmss-CPU-ex-full-1.png)

![Sanal makine ölçeği set yüzdesi CPU örneği](media/autoscale-troubleshoot/autoscale-vmss-CPU-ex-full-2.png)

***Şekil 1a - Sanal makine ölçek kümesi için yüzde CPU ölçümü ve otomatik ölçek ayarı için Gözlenen Metrik Değer ölçümü***

![Metrik Eşik ve Gözlemlenen Kapasite](media/autoscale-troubleshoot/autoscale-metric-threshold-capacity-ex-full.png)

***Şekil 1b - Metrik Eşik ve Gözlemlenen Kapasite***

Şekil 1b'de, ölçeklendirme kuralı için **Metrik Eşik** (açık mavi çizgi) 70'tir.  **Gözlenen Kapasite** (koyu mavi çizgi), şu anda 3 olan etkin örneklerin sayısını gösterir. 

> [!NOTE]
> Ölçeklendirme eşiğini ve kural (azalış) ölçeğini görmek için metrik tetik kuralı boyutu ölçeği (artır) kuralına göre **Metrik Eşiği** filtrelemeniz gerekir. 

## <a name="example-2---advanced-autoscaling-for-a-virtual-machine-scale-set"></a>Örnek 2 - Sanal makine ölçek kümesi için gelişmiş otomatik ölçeklendirme

Sanal makine ölçeği ayarlı kaynağın kendi metrik **Giden Akışları'na**göre ölçeklemesine olanak tanıyan otomatik ölçeklendirme ayarımız var. Metrik eşik için **örnek sayısı seçeneğine bölme metriğinin** denetlenmiş olduğuna dikkat edin. 

Ölçek eylem kuralı: 

**Çıkış Akışı'nın örnek başına** değeri 10'dan büyükse, otomatik ölçeklendirme hizmeti 1 örneğine göre ölçeklendirilmelidir. 

Bu durumda, otomatik ölçeklendirme motorunun gözlenen metrik değeri, örnek sayısına bölünen gerçek metrik değer olarak hesaplanır. Gözlenen metrik değer eşikten küçükse, ölçeklendirme eylemi başlatılmadı. 
 
![Sanal makine ölçeği kümesi otomatik ölçek ölçümleri grafikleri örnek](media/autoscale-troubleshoot/autoscale-vmss-metric-chart-ex-1.png)

![Sanal makine ölçeği kümesi otomatik ölçek ölçümleri grafikleri örnek](media/autoscale-troubleshoot/autoscale-vmss-metric-chart-ex-2.png)

***Şekil 2 - Sanal makine ölçeği otomatik ölçek ölçüm grafikleri örnek ayarlamak***

Şekil 2'de iki metrik grafik görebilirsiniz. 

Üstteki **grafik, Giden Akışlar** ölçümünün gerçek değerini gösterir. Gerçek değer 6'dır. 

Alttaki grafik birkaç değer gösterir. 
 - **Gözlenen Metrik değeri** (açık mavi) 3'tür, çünkü 2 etkin örnek vardır ve 6 2'ye bölünür 3'tür. 
 - **Gözlenen Kapasite** (mor), otomatik ölçeklendirme motoru tarafından görülen örnek sayısını gösterir. 
 - **Metrik Eşik** (açık yeşil) 10 olarak ayarlanır. 

Birden çok ölçek eylem kuralı varsa, ölçümlere belirli bir kaynak veya kurala göre bakmak için Ölçümler gezgini grafiğinde bölme veya **filtre ekleme** seçeneğini kullanabilirsiniz. Metrik grafiği bölme hakkında daha fazla bilgi için [metrik grafiklerin gelişmiş özelliklerine](metrics-charts.md#apply-splitting-to-a-chart) bakın - bölme

## <a name="example-3---understanding-autoscale-events"></a>Örnek 3 - Otomatik ölçek olaylarını anlama

Otomatik ölçek ayar ekranında, en son ölçek eylemlerini görmek için **Geçmiş Çalıştır** sekmesine gidin. Sekme, zaman içinde **Gözlenen Kapasitedeki** değişikliği de gösterir. Otomatik ölçek ayarlarını güncelleştirme/silme gibi işlemler de dahil olmak üzere tüm otomatik ölçeklendirme eylemleri hakkında daha fazla bilgi edinmek için, otomatik ölçeklendirme işlemlerine göre etkinlik günlüğünü ve filtreyi görüntüleyin.

![Otomatik ölçeklendirme ayarları geçmişi çalıştırın](media/autoscale-troubleshoot/autoscale-setting-run-history-smaller.png)

## <a name="autoscale-resource-logs"></a>Otomatik Ölçekkaynak Günlükleri

Diğer Azure kaynakları yla aynı olan otomatik ölçeklendirme hizmeti [de kaynak günlükleri](platform-logs-overview.md)sağlar. Günlüklerin iki kategorisi vardır.

- **Otomatik Ölçeklendirme Değerlendirmeleri** - Otomatik ölçeklendirme motoru, her denetim yaptığında her koşul değerlendirmesi için günlük girişlerini kaydeder.  Giriş, ölçümlerin gözlenen değerleri, değerlendirilen kurallar ve değerlendirmenin bir ölçek eylemiyle sonuçlanıp sonuçlanmadığı ile ilgili ayrıntıları içerir.

- **Otomatik Ölçeklendirme Eylemleri** - Motor, otomatik ölçeklendirme hizmeti tarafından başlatılan eylem olaylarını ve bu ölçeklendirme eylemlerinin sonuçlarını (başarı, başarısızlık ve otomatik ölçeklendirme hizmetinde görüldüğü gibi ne kadar ölçeklendirme oluştuğunu) kaydeder.

Azure Monitor desteklenen herhangi bir hizmette olduğu gibi, bu günlükleri yönlendirmek için [Tanılama Ayarlarını](diagnostic-settings.md) kullanabilirsiniz:

- ayrıntılı analiz için Log Analytics çalışma alanınıza
- Olay Hub'larına ve ardından Azure olmayan araçlara
- arşivleme için Azure depolama hesabınıza  

![Otomatik Ölçeklendirme Tanı ayarları](media/autoscale-troubleshoot/diagnostic-settings.png)

Önceki resimde Azure portalı otomatik ölçeklendirme tanılama ayarları gösterilmektedir. Burada Tanılama/Kaynak Günlükleri sekmesini seçebilir ve günlük toplama ve yönlendirmeyi etkinleştirebilirsiniz. Ayrıca, *microsoft.Insights/AutoscaleSettings*olarak kaynak türünü seçerek, Tanılama Ayarları için REST API, CLI, PowerShell, Resource Manager şablonlarını kullanarak aynı eylemi gerçekleştirebilirsiniz. 

## <a name="troubleshooting-using-autoscale-logs"></a>Otomatik ölçekgünlüklerini kullanarak sorun giderme 

En iyi sorun giderme deneyimi için, otomatik ölçek ayarını oluştururken günlüklerinizi bir çalışma alanı üzerinden Azure Monitör Günlükleri'ne (Log Analytics) yönlendirmenizi öneririz. Bu işlem önceki bölümde resimde gösterilir. Log Analytics'i kullanarak değerlendirmeleri ve ölçeklendirme eylemlerini daha iyi doğrulayabilirsiniz.

Oto ölçekli günlüklerinizi Log Analytics çalışma alanına gönderilecek şekilde yapılandırdıktan sonra, günlükleri denetlemek için aşağıdaki sorguları gerçekleştirebilirsiniz. 

Başlamak için, en son otomatik ölçek değerlendirme günlüklerini görüntülemek için bu sorguyu deneyin:

```Kusto
AutoscaleEvaluationsLog
| limit 50
```

Veya en son ölçek eylem günlüklerini görüntülemek için aşağıdaki sorguyu deneyin:

```Kusto
AutoscaleScaleActionsLog
| limit 50
```

Bu sorular için aşağıdaki bölümleri kullanın. 

## <a name="a-scale-action-occurred-that-i-didnt-expect"></a>Beklemediğim bir ölçek eylemi oluştu.

Önce ilgilendiğiniz ölçek eylemini bulmak için ölçek eylemi sorgusunu yürütün. En son ölçek eylemiyse, aşağıdaki sorguyu kullanın:

```Kusto
AutoscaleScaleActionsLog
| take 1
```

Ölçek eylemleri günlüğünden Korelasyon Alanı'nı seçin. Doğru Değerlendirme günlüğünü bulmak için KorelasyonId'i kullanın. Aşağıdaki sorguyu yürütmek, bu ölçek eylemine yol açan değerlendirilen tüm kural ve koşulları görüntüler.

```Kusto
AutoscaleEvaluationsLog
| where CorrelationId = "<correliationId>"
```

## <a name="what-profile-caused-a-scale-action"></a>Hangi profil ölçek eylemine neden oldu?

Ölçeklenmiş bir eylem oluştu, ancak çakışan kurallarınız ve profilleriniz var ve eyleme neden olan bu eylemi izlemeniz gerekiyor. 

Ölçek eyleminin korelasyonid'ini bulun (örnek 1'de açıklandığı gibi) ve ardından profil hakkında daha fazla bilgi edinmek için değerlendirme günlüklerinde sorguyu yürütün.

```Kusto
AutoscaleEvaluationsLog
| where CorrelationId = "<correliationId_Guid>"
| where ProfileSelected == true
| project ProfileEvaluationTime, Profile, ProfileSelected, EvaluationResult
```

Profil değerlendirmesinin tamamı aşağıdaki sorgu kullanılarak daha iyi anlaşılabilir

```Kusto
AutoscaleEvaluationsLog
| where TimeGenerated > ago(2h)
| where OperationName contains == "profileEvaluation"
| project OperationName, Profile, ProfileEvaluationTime, ProfileSelected, EvaluationResult
```

## <a name="a-scale-action-did-not-occur"></a>Bir ölçek eylemi gerçekleşmedi

Ben bir ölçek eylem bekleniyor ve olmadı. Ölçekeylem olayları veya günlükleri olmayabilir.

Metrik tabanlı ölçek kuralı kullanıyorsanız otomatik ölçek ölçümlerini gözden geçirin. **Gözlenen metrik değer** in veya **Gözlenen Kapasite'nin** beklediğiniz gibi olmaması ve bu nedenle ölçek kuralının ateşlenmemesi mümkündür. Yine de değerlendirmeler görürdün, ama ölçeklendirme kuralı değil. Ayrıca, soğuma süresinin bir ölçek eyleminin oluşmasını engellenebilmektedir. 

 Ölçek eyleminin gerçekleşmesini beklediğiniz süre boyunca otomatik ölçek değerlendirme günlüklerini gözden geçirin. Yaptığı tüm değerlendirmeleri ve neden bir ölçek eylemini tetiklememeye karar verdi.


```Kusto
AutoscaleEvaluationsLog
| where TimeGenerated > ago(2h)
| where OperationName == "MetricEvaluation" or OperationName == "ScaleRuleEvaluation"
| project OperationName, MetricData, ObservedValue, Threshold, EstimateScaleResult
```

## <a name="scale-action-failed"></a>Ölçek eylemi başarısız oldu

Otomatik ölçeklendirme hizmetinin ölçeklendirme eylemini üstlendiği bir durum olabilir, ancak sistem ölçeklendirmemeye karar verdi veya ölçek eylemini tamamlamadı. Başarısız ölçek eylemlerini bulmak için bu sorguyu kullanın.

```Kusto
AutoscaleScaleActionsLog
| where ResultType == "Failed"
| project ResultDescription
```

Otomatik ölçeklendirme eylemleri veya hataları hakkında bilgi almak için uyarı kuralları oluşturun. Otomatik ölçek olayları hakkında bilgilendirilmek için uyarı kuralları da oluşturabilirsiniz.

## <a name="schema-of-autoscale-resource-logs"></a>Otomatik ölçek kaynak günlüklerinin şeması

Daha fazla bilgi için [otomatik ölçeklendirme kaynak günlüklerine](autoscale-resource-log-schema.md) bakın

## <a name="next-steps"></a>Sonraki adımlar
[Otomatik ölçekle en iyi uygulamalar](autoscale-best-practices.md)hakkında bilgi okuyun. 
