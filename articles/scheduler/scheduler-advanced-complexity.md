---
title: Gelişmiş iş zamanlamaları ve yinelemeler oluşturun
description: Azure Zamanlayıcısı'ndaki işler için gelişmiş zamanlamalar ve yinelemeler oluşturmayı öğrenin
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.suite: infrastructure-services
ms.topic: article
ms.date: 11/14/2018
ms.openlocfilehash: b85932bf0d4fd080afadef2bc28d6a218b2d627a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898595"
---
# <a name="build-advanced-schedules-and-recurrences-for-jobs-in-azure-scheduler"></a>Azure Zamanlayıcısı'ndaki işler için gelişmiş zamanlamalar ve yinelemeler oluşturun

> [!IMPORTANT]
> [Azure Logic Apps,](../logic-apps/logic-apps-overview.md) [kullanımdan kaldırılan](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date)Azure Zamanlayıcısı'nın yerini alıyor. Zamanlayıcı'da ayarladığınız işlerle çalışmaya devam etmek için lütfen mümkün olan en kısa sürede [Azure Logic Apps'a geçirin.](../scheduler/migrate-from-scheduler-to-logic-apps.md) 
>
> Zamanlayıcı artık Azure portalında kullanılamıyor, ancak işlerinizi ve iş koleksiyonlarınızı yönetebilmeniz için [ŞU anda REST API](/rest/api/scheduler) ve [Azure Scheduler PowerShell cmdlets'i](scheduler-powershell-reference.md) kullanılabilir durumda kalır.

Azure [Zamanlayıcısı](../scheduler/scheduler-intro.md) işi içinde zamanlama, Zamanlayıcı hizmetinin işi ne zaman ve nasıl çalıştırdığını belirleyen temeldir. Zamanlayıcı ile bir iş için birden çok tek seferlik ve yinelenen zamanlamalar ayarlayabilirsiniz. Tek seferlik zamanlamalar belirli bir zamanda yalnızca bir kez çalışır ve temelde yalnızca bir kez çalışan yinelenen zamanlamalardır. Yinelenen zamanlamalar belirli bir sıklıkta çalışır. Bu esneklikle, örneğin çeşitli iş senaryoları için Zamanlayıcı'yı kullanabilirsiniz:

* **Verileri düzenli olarak temizleyin**: Üç aydan eski tüm tweetleri silen günlük bir iş oluşturun.

* **Arşiv verileri**: Fatura geçmişini yedekleme hizmetine iten aylık bir iş oluşturun.

* **Dış veri isteği**: Her 15 dakikada bir çalışan ve NOAA'dan yeni bir hava durumu raporu çeken bir iş oluşturun.

* **İşlem görüntüleri**: Yoğun olmayan saatlerde çalışan ve gün içinde yüklenen görüntüleri sıkıştırmak için bulut bilgi işlem kullanan bir hafta içi iş oluşturun.

Bu makalede, Scheduler ve [Azure Scheduler REST API](/rest/api/scheduler)kullanarak oluşturabileceğiniz örnek işler açıklanır ve her zamanlama için JavaScript Nesne Gösterimi (JSON) tanımını içerir. 

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Bu örnekler, Azure Zamanlayıcısı'nın desteklediği senaryo aralığını ve örneğin çeşitli davranış desenleri için zamanlamaların nasıl oluşturulacak olduğunu gösterir:

* Belirli bir tarih ve saatte bir kez çalıştırın.
* Belirli sayıda çalıştırın ve yinele.
* Hemen çalıştırın ve yinele.
* Belirli bir andan başlayarak her *dakika,* saat, gün, hafta veya ay çalıştırın ve yineleyin.
* Haftalık veya aylık olarak çalıştırın ve yinelenin, ancak yalnızca haftanın belirli günlerinde veya ayın belirli günlerinde.
* Belirli bir süre için birden fazla kez çalıştırın ve yinele. Örneğin, her ay son Cuma ve Pazartesi günü veya her gün 05:15'te ve 17:15'te.

Bu makalede daha sonra bu senaryoları daha ayrıntılı olarak açıklanır.

<a name="create-scedule"></a>

## <a name="create-schedule-with-rest-api"></a>REST API ile zamanlama oluşturma

[Azure Zamanlayıcısı REST API](/rest/api/scheduler)ile temel bir zamanlama oluşturmak için aşağıdaki adımları izleyin:

1. Kayıt işlemini - Kaynak Yöneticisi [REST API'yi](https://docs.microsoft.com/rest/api/resources/providers)kullanarak Azure aboneliğinizi bir kaynak sağlayıcısına kaydedin. Azure Zamanlayıcısı hizmetinin sağlayıcı adı **Microsoft.Scheduler'dır.** 

1. Zamanlayıcı REST API'ndeki [iş koleksiyonları için Oluştur veya Güncelleştir işlemini](https://docs.microsoft.com/rest/api/scheduler/jobcollections) kullanarak bir iş koleksiyonu oluşturun. 

1. İşler için Oluştur [veya Güncelleştir işlemini](https://docs.microsoft.com/rest/api/scheduler/jobs/createorupdate)kullanarak bir iş oluşturun. 

## <a name="job-schema-elements"></a>İş şeması elemanları

Bu tablo, yinelemeler ve işler için zamanlamalar ayarlarken kullanabileceğiniz başlıca JSON öğeleri için üst düzey bir genel bakış sağlar. 

| Öğe | Gerekli | Açıklama | 
|---------|----------|-------------|
| **startTime** | Hayır | [ISO 8601 biçiminde,](https://en.wikipedia.org/wiki/ISO_8601) işin temel bir zamanlamada ilk ne zaman başlayacağını belirten datetime dize değeri. <p>Karmaşık zamanlamalar için, iş **başlangıç Saatinden**daha erken başlar. | 
| **yineleme** | Hayır | İş ne zaman çalışır için yineleme kuralları. **Yineleme nesnesi** bu öğeleri destekler: **sıklık,** **aralık,** **zamanlama,** **sayı,** ve **bitiş Zamanı**. <p>**Yineleme** öğesini kullanıyorsanız, diğer **yineleme** öğeleri isteğe bağlı **iken, sıklık** öğesini de kullanmanız gerekir. |
| **Frekans** | Evet, **yineleme** kullandığınızda | Oluşumlar arasındaki zaman birimi ve bu değerleri destekler: "Dakika", "Saat", "Gün", "Hafta", "Ay" ve "Yıl" | 
| **Aralığı** | Hayır | **Frekansa**göre olaylar arasındaki zaman birimlerinin sayısını belirleyen pozitif bir tamsayı. <p>Örneğin, **aralık** 10 ve **sıklık** "Hafta" ise, iş her 10 haftada bir yinelenir. <p>Burada her frekans için aralıkları en çok sayıda: <p>- 18 ay <br>- 78 hafta <br>- 548 gün <br>- Saat ve dakika için aralık 1 <= <*aralığı*> <= 1000'dir. | 
| **Zamanlama** | Hayır | Belirtilen dakika işaretlerine, saat işaretlerine, haftanın günlerine ve ayın günlerine göre yinelemedeki değişiklikleri tanımlar | 
| **count** | Hayır | Bitirmeden önce işin kaç kez çalıştığını belirten pozitif bir tamsayı. <p>Örneğin, günlük bir iş **sayısı** 7 olarak ayarlanmışsa ve başlangıç tarihi Pazartesi ise, iş Pazar günü çalışır. Başlangıç tarihi zaten geçtiyse, ilk çalıştırma oluşturma zamanından hesaplanır. <p>**Bitiş Saati** veya **sayı**olmadan, iş sonsuz çalışır. Aynı işte hem **sayıyı** hem de **bitiş saatini** kullanamazsınız, ancak ilk bitiren kural yerine getirilmiştir. | 
| **endTime** | Hayır | [ISO 8601 biçiminde,](https://en.wikipedia.org/wiki/ISO_8601) işin ne zaman çalışmayı durdurduğunu belirten bir Tarih veya DateTime dize değeri. Geçmişte olan son **Zaman** için bir değer ayarlayabilirsiniz. <p>**Bitiş Saati** veya **sayı**olmadan, iş sonsuz çalışır. Aynı işte hem **sayıyı** hem de **bitiş saatini** kullanamazsınız, ancak ilk bitiren kural yerine getirilmiştir. |
|||| 

Örneğin, bu JSON şeması bir iş için temel bir zamanlama ve yineleme açıklar: 

```json
"properties": {
   "startTime": "2012-08-04T00:00Z", 
   "recurrence": {
      "frequency": "Week",
      "interval": 1,
      "schedule": {
         "weekDays": ["Monday", "Wednesday", "Friday"],
         "hours": [10, 22]                      
      },
      "count": 10,       
      "endTime": "2012-11-04"
   },
},
``` 

*Tarihler ve DateTime değerleri*

* Zamanlayıcı işlerinde tarihler yalnızca tarihi içerir ve [ISO 8601 belirtimini](https://en.wikipedia.org/wiki/ISO_8601)izler.

* Zamanlayıcı işlerinde tarih-saatler hem tarih hem de saat içerir, [ISO 8601 belirtimini](https://en.wikipedia.org/wiki/ISO_8601)izler ve UTC ofset belirtilmediğinde UTC olarak kabul edilir. 

Daha fazla bilgi için [Kavramlar, terminoloji ve varlıklara](../scheduler/scheduler-concepts-terms.md)bakın.

<a name="start-time"></a>

## <a name="details-starttime"></a>Ayrıntılar: startTime

Bu **tabloda, başlangıç Zamanı'nın** bir işin çalışma biçimini nasıl denetlenen açıklanır:

| startTime | Yineleme yok | Yineleme, zamanlama yok | Zamanlama ile yinelenme |
|-----------|---------------|-------------------------|--------------------------|
| **Başlangıç saati yok** | Hemen bir kez koş. | Hemen bir kez koş. Son yürütme saatinden hesaplanan daha sonra yürütmeleri çalıştırın. | Hemen bir kez koş. Yineleme zamanlamasına göre daha sonraki yürütmeleri çalıştırın. | 
| **Geçmişte başlangıç zamanı** | Hemen bir kez koş. | Başlangıç saatinden sonra gelecekteki ilk çalışma süresini hesaplayın ve o anda çalıştırın. <p>Son yürütme saatinden hesaplanan daha sonra yürütmeleri çalıştırın. <p>Bu tablodan sonraki örneğe bakın. | Belirtilen başlangıç saatinden *daha erken* işe başlayın. İlk yinelenme, başlangıç zamanından hesaplanan zamanlamaya göre gerçekleştirilir. <p>Yineleme zamanlamasına göre daha sonraki yürütmeleri çalıştırın. | 
| **Gelecekte başlangıç saati veya geçerli saat** | Belirtilen başlangıç saatinde bir kez çalıştırın. | Belirtilen başlangıç saatinde bir kez çalıştırın. <p>Son yürütme saatinden hesaplanan daha sonra yürütmeleri çalıştırın. | Belirtilen başlangıç saatinden *daha erken* işe başlayın. İlk yinelenme, başlangıç zamanından hesaplanan zamanlamaya göre gerçekleştirilir. <p>Yineleme zamanlamasına göre daha sonraki yürütmeleri çalıştırın. |
||||| 

Bu koşullarla bu örneği varsayalım: geçmişte yinelenen bir başlangıç zamanı, ancak zamanlama yok.

```json
"properties": {
   "startTime": "2015-04-07T14:00Z", 
   "recurrence": {
      "frequency": "Day",
      "interval": 2
   }
}
```

* Şu anki tarih ve saat 08 Nisan 2015 saat 13:00'tir.

* Başlangıç tarihi ve saati 07 Nisan 2015 saat 14:00'tir ve bu tarih ve saatten öncedir.

* Nüks her iki günde bir.

1. Bu şartlar altında ilk infaz 09 Nisan 2015 tarihinde saat 14:00'te gerçekleştirilecektir. 

   Zamanlayıcı yürütme oluşumlarını başlangıç saatini temel alınatır, geçmişteki tüm örnekleri atar ve gelecekte bir sonraki örneği kullanır. 
   Bu durumda, **başlangıç Saati** 07 Nisan 2015 tarihinde saat 14:00'te, bu nedenle bir sonraki örnek o tarihten iki gün sonra, 09 Nisan 2015 saat 14:00'te.

   İlk yürütme **startTime** 2015-04-05 14:00 veya 2015-04-01 14:00 olup olmadığını aynıdır. İlk yürütmeden sonra, daha sonraki yürütmeler zamanlamaya göre hesaplanır. 
   
1. Yürütmeler daha sonra bu sırada izleyin: 
   
   1. 2015-04-11 saat 14:00
   1. 2015-04-13 saat 14:00 
   1. 2015-04-15, 14:00
   1. Ve böylece ...

1. Son olarak, bir işin bir zamanlaması olduğunda ancak belirli bir saat ve dakika yoksa, bu değerler sırasıyla ilk yürütmedeki saat ve dakikaiçin varsayılan dır.

<a name="schedule"></a>

## <a name="details-schedule"></a>Ayrıntılar: zamanlama

İş yürütme sayısını *sınırlamak* için **zamanlamayı** kullanabilirsiniz. Örneğin, "ay" **sıklığı** olan bir iş yalnızca 31 gün çalışan bir zamanlama varsa, iş yalnızca 31 gün olan aylarda çalışır.

**Zamanlamayı,** iş yürütme sayısını *artırmak* için de kullanabilirsiniz. Örneğin, "ay" **sıklığı** olan bir iş, ay gün 1 ve 2'de çalışan bir zamanlamaya sahipse, iş ayda yalnızca bir kez değil, ayın ilk ve ikinci günlerinde çalışır.

Birden fazla zamanlama öğesi belirtirseniz, değerlendirme sırası en büyükten en küçme doğru: hafta sayısı, ay günü, hafta içi, saat ve dakika.

Aşağıdaki tabloda schedule öğeleri ayrıntılı bir şekilde açıklanmıştır:

| JSON adı | Açıklama | Geçerli değerler |
|:--- |:--- |:--- |
| **minutes** |İşin çalıştığı saatin dakikaları. |Bir dizi sonal. |
| **Saat** |İşin çalıştığı günün saatleri. |Bir dizi sonal. |
| **weekDays** |Haftanın günleri iş çalışır. Yalnızca haftalık frekansla belirtilebilir. |Aşağıdaki değerlerden herhangi biri (maksimum dizi boyutu 7'dir):<br />- "Pazartesi"<br />- "Salı"<br />- "Çarşamba"<br />- "Perşembe"<br />- "Cuma"<br />- "Cumartesi"<br />- "Pazar"<br /><br />Davaya duyarlı değil. |
| **monthlyOccurrences** |Ayın hangi günlerini çalışır belirler. Yalnızca aylık bir sıklıkta belirtilebilir. |**Aylık Oluşumlar** nesneleri dizisi:<br /> `{ "day": day, "occurrence": occurrence}`<br /><br /> **gün,** işin çalıştığı haftanın günüdür. Örneğin, *{Pazar}* ayın her Pazar günüdür. Gereklidir.<br /><br />**meydana** gelmesi ay boyunca günün meydana gelmesidir. Örneğin, *{Pazar, -1}* ayın son Pazar günüdür. İsteğe bağlı. |
| **monthDays** |Ayın günü iş çalışır. Yalnızca aylık bir sıklıkta belirtilebilir. |Aşağıdaki değerlerden oluşan bir dizi:<br />- <= -1 ve >= -31 koşullarına uyan herhangi bir değer<br />- >= 1 ve <= 31 koşullarına uyan herhangi bir değer|

## <a name="examples-recurrence-schedules"></a>Örnekler: Yineleme çizelgeleri

Aşağıdaki örneklerde çeşitli yineleme zamanlamaları gösterilmektedir. Örnekler zamanlama nesnesi ve alt öğeleri üzerinde duruluyor.

Bu **zamanlamalar, aralığın** 1 olarak ayarlanır\. Örnekler de **zamanlamadaki**değerler için doğru **frekans** değerlerini varsayar. Örneğin, "gün" **frekansını** kullanamazsınız ve **zamanlamada** **ay günleri** değişikliği niz vardır. Bu kısıtlamaları makalede daha önce açıklıyoruz.

| Örnek | Açıklama |
|:--- |:--- |
| `{"hours":[5]}` |Her gün sabah 5'te koş.<br /><br />Zamanlayıcı, işin çalıştığı tüm zamanların bir listesini oluşturmak için "dakika"daki her değerle "saat" deki her değeri tek tek eşleşir. |
| `{"minutes":[15], "hours":[5]}` |Her gün 05.15'te çalıştır. |
| `{"minutes":[15], "hours":[5,17]}` |Her gün 05.15 ve 17.15’te çalıştır. |
| `{"minutes":[15,45], "hours":[5,17]}` |Her gün 05.15, 05.45, 17.15 ve 17.45’te çalıştır. |
| `{"minutes":[0,15,30,45]}` |15 dakikada bir çalıştır. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` |Saatte bir çalıştır.<br /><br />Bu iş her saat başı çalışıyor. Dakika, belirtilirse **startTime**değeri tarafından denetlenir. Başlangıç **Zamanı** değeri belirtilmemişse, dakika oluşturma süresi tarafından denetlenir. Örneğin, başlangıç saati veya oluşturma zamanı (hangisi geçerliyse) 12:25 ise, iş 00:25, 01:25, 02:25, ..., 23:25'te çalışır.<br /><br />Zamanlama, "saat" **sıklığı,** 1 **aralığı** ve **zamanlama** değeri olmayan bir iş ile aynıdır. Fark, diğer işleri oluşturmak için farklı **sıklık** ve **aralık** değerleri ile bu zamanlama kullanabilirsiniz. Örneğin, **sıklık** "ay" ise, zamanlama her gün yerine ayda yalnızca bir kez çalışır **(sıklık** "gün" ise). |
| `{minutes:[0]}` |Her saat başı çalıştır.<br /><br />Bu iş de her saat çalışır, ama saat (12, 01,02, 02:00 ve benzeri). Bu zamanlama, "saat" **sıklığı,** sıfır dakika **başlangıç Zamanı** değeri ve zamanlama **yok**, frekans "gün" ise bir iş ile aynıdır. Ancak, **sıklık** "hafta" veya "ay" ise, zamanlama sırasıyla haftada yalnızca bir gün veya ayda bir gün yürütür. |
| `{"minutes":[15]}` |Saat başı 15 dakika koş.<br /><br />00:15, 01:15, 02:15, 02:15 ve benzeri her saat çalışır. 23:15'te bitiyor. |
| `{"hours":[17], "weekDays":["saturday"]}` |Her hafta Cumartesi günü saat 17:00'de koşun. |
| `{hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Her hafta Pazartesi, Çarşamba ve Cuma günleri saat 17:00'de çalıştırın. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Her hafta Pazartesi, Çarşamba ve Cuma günleri saat 17.15 ve 17.45'te çalıştır. |
| `{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |Her hafta Pazartesi, Çarşamba ve Cuma günleri 05:00 ve 17:00'de çalıştırın. |
| `{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |Her hafta Pazartesi, Çarşamba ve Cuma günleri 05:15, 05:45, 17:15 ve Pazartesi, Çarşamba ve Cuma günleri çalıştırın. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Haftanın her günü 15 dakikada bir çalıştır. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Hafta içi her 15 dakikada bir, 09:00-16:45 saatleri arasında çalıştırın. |
| `{"weekDays":["sunday"]}` |Pazar günleri başlangıç saatinde koş. |
| `{"weekDays":["tuesday", "thursday"]}` |Başlangıç saatinde salı ve perşembe günleri koşun. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` |Her ayın 28'inde sabah 6'da **çalıştırın** ("ay" sıklığı varsayarak). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` |Ayın son günü sabah 6'da koş.<br /><br />Bir işi ayın son gününde çalıştırmak istiyorsanız, 28, 29, 30 veya 31 gün yerine -1 kullanın. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` |Her ayın ilk ve son gününde sabah 6'da çalıştırın. |
| `{monthDays":[1,-1]}` |Her ayın ilk ve son gününde başlangıç saatinde çalıştırın. |
| `{monthDays":[1,14]}` |Her ayın ilk ve 14. |
| `{monthDays":[2]}` |Ayın ikinci gününde başlangıç saatinde çalıştırın. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |Her ayın ilk Cuma günü sabah 5'te çalıştırın. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |Her ayın ilk Cuma günü başlangıç saatinde çalıştırın. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` |Ay sonundan itibaren üçüncü Cuma günü, her ay, başlangıç saatinde çalıştırın. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |Her ayın ilk ve son Cuma günü saat 05.15'te çalıştır. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |Her ayın ilk ve son Cuma günü başlangıç saatinde çalıştırın. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` |Her ayın beşinci Cuma günü başlangıç saatinde çalıştırın.<br /><br />Bir ay içinde beşinci Cuma olmazsa, iş çalışmaz. İşi ayın son Cuma günü çalıştırmak istiyorsanız, olay için 5 yerine -1 kullanmayı düşünebilirsiniz. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` |Ayın son Cuma günü 15 dakikada bir çalıştır. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` |Her ayın üçüncü Çarşamba günü 05.15, 05.45, 17.15 ve 17.45’te çalıştır. |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)
* [Azure Scheduler REST API başvurusu](/rest/api/scheduler)
* [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)
* [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)