---
title: Azure Monitor Log Analytics'te günlük sorgu kapsamı | Microsoft Dokümanlar
description: Azure Monitor Log Analytics'te bir günlük sorgusunun kapsam ve zaman aralığını açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/25/2019
ms.openlocfilehash: 897eff62fcbab5996b6b9493bd825ae412aa4c3e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249600"
---
# <a name="log-query-scope-and-time-range-in-azure-monitor-log-analytics"></a>Azure Monitör Günlük Analizi'nde günlük sorgusu kapsamı ve zaman aralığı
[Azure portalında Log Analytics'te](get-started-portal.md)bir [günlük sorgusu](log-query-overview.md) çalıştırdığınızda, sorgu tarafından değerlendirilen veri kümesi, seçtiğiniz kapsam ve zaman aralığına bağlıdır. Bu makalede, kapsam ve zaman aralığı ve gereksinimlerinize bağlı olarak her nasıl ayarlayabilirsiniz açıklanır. Ayrıca, farklı kapsam türlerinin davranışını da açıklar.


## <a name="query-scope"></a>Sorgu kapsamı
Sorgu kapsamı, sorgu tarafından değerlendirilen kayıtları tanımlar. Bu genellikle tek bir Log Analytics çalışma alanı veya Application Insights uygulamasındaki tüm kayıtları içerir. Log Analytics ayrıca belirli bir izlenen Azure kaynağı için bir kapsam belirlemenize de olanak tanır. Bu, kaynak birden çok çalışma alanı için yazsa bile, kaynak sahibinin yalnızca verilerine odaklanmasına olanak tanır.

Kapsam her zaman Log Analytics penceresinin sol üst kısmında görüntülenir. Simge, kapsamın Log Analytics çalışma alanı mı yoksa Application Insights uygulaması mı olduğunu gösterir. Hiçbir simge başka bir Azure kaynağını gösterir.

![Kapsam](media/scope/scope.png)

Kapsam, Log Analytics'i başlatmak için kullandığınız yöntemle belirlenir ve bazı durumlarda üzerine tıklayarak kapsamı değiştirebilirsiniz. Aşağıdaki tabloda kullanılan farklı kapsam türleri ve her biri için farklı ayrıntılar listelenir.

| Sorgu kapsamı | Kapsamdaki kayıtlar | Nasıl seçilir? | Kapsamı Değiştirme |
|:---|:---|:---|:---|
| Log Analytics çalışma alanı | Log Analytics çalışma alanındaki tüm kayıtlar. | **Azure Monitörü** menüsünden günlükler veya **Günlük Analizi çalışma alanları** menüsünden **Günlükler'i** seçin.  | Kapsamı başka bir kaynak türüyle değiştirebilir. |
| Uygulama Öngörüleri uygulaması | Application Insights uygulamasındaki tüm kayıtlar. | Uygulama **Öngörüleri'nin Genel Bakış** sayfasından **Analitik'i** seçin. | Kapsamı yalnızca başka bir Application Insights uygulamasıyla değiştirebilir. |
| Kaynak grubu | Kaynak grubundaki tüm kaynaklar tarafından oluşturulan kayıtlar. Birden çok Log Analytics çalışma alanlarından gelen verileri içerebilir. | Kaynak grubu menüsünden **Günlükler'i** seçin. | Kapsamı değiştiremezsiniz.|
| Abonelik | Abonelikteki tüm kaynaklar tarafından oluşturulan kayıtlar. Birden çok Log Analytics çalışma alanlarından gelen verileri içerebilir. | Abonelik menüsünden **Günlükler'i** seçin.   | Kapsamı değiştiremezsiniz. |
| Diğer Azure kaynakları | Kaynak tarafından oluşturulan kayıtlar. Birden çok Log Analytics çalışma alanlarından gelen verileri içerebilir.  | Kaynak menüsünden **Günlükler'i** seçin.<br>OR<br>**Azure Monitörü** menüsünden **Günlükler'i** seçin ve ardından yeni bir kapsam seçin. | Kapsamı yalnızca aynı kaynak türüyle değiştirebilir. |

### <a name="limitations-when-scoped-to-a-resource"></a>Bir kaynağa kapsamlandığında sınırlamalar

Sorgu kapsamı Log Analytics çalışma alanı veya Application Insights uygulaması olduğunda, portaldaki tüm seçenekler ve tüm sorgu komutları kullanılabilir. Ancak bir kaynağa kapsamlandığında, tek bir çalışma alanı veya uygulamayla ilişkili oldukları için portaldaki aşağıdaki seçenekler kullanılamıyor:

- Kaydet
- Sorgu gezgini
- Yeni uyarı kuralı

Sorgu kapsamı, söz söz le ilgili kaynak veya kaynak kümesiiçin veri içeren çalışma alanlarını zaten içereceği için, bir kaynağa kapsamlı olduğunda sorguda aşağıdaki komutları kullanamazsınız:

- [App](app-expression.md)
- [Çalışma alanı](workspace-expression.md)
 

## <a name="query-limits"></a>Sorgu sınırları
Bir Azure kaynağının birden çok Log Analytics çalışma alanlarına veri yazması için iş gereksinimleriniz olabilir. Çalışma alanının kaynakla aynı bölgede olması gerekmez ve tek bir çalışma alanı çeşitli bölgelerdeki kaynaklardan veri topleyebilir.  

Kapsamı bir kaynağa veya kaynak kümesine ayarlamak, dağıtılmış verileri tek bir sorguda otomatik olarak birleştirmenize olanak sağladığından Log Analytics'in özellikle güçlü bir özelliğidir. Verilerin birden çok Azure bölgesinde çalışma alanlarından alınması gerekiyorsa, performansı önemli ölçüde etkileyebilir.

Log Analytics, belirli sayıda bölge kullanılırken bir uyarı veya hata yayınlayarak birden çok bölgede çalışma alanlarını kapsayan sorgulara karşı aşırı ek yükün korunmasına yardımcı olur. Kapsam 5 veya daha fazla bölgede çalışma alanlarını içeriyorsa, sorgunuz bir uyarı alır. yine de çalışır, ancak tamamlamak için aşırı zaman alabilir.

![Sorgu uyarısı](media/scope/query-warning.png)

Kapsam 20 veya daha fazla bölgede çalışma alanlarını içeriyorsa, sorgunuzun çalışması engellenir. Bu durumda, çalışma alanı bölgelerinin sayısını azaltmanız ve sorguyu yeniden çalıştırmayı denemeniz istenir. Açılan tüm bölgeler sorgu kapsamındaki tüm görüntüler ve sorguyu yeniden çalıştırmayı denemeden önce bölge sayısını azaltmanız gerekir.

![Sorgu başarısız oldu](media/scope/query-failed.png)


## <a name="time-range"></a>Zaman aralığı
Zaman aralığı, kaydın oluşturulduğu zamana göre sorgu için değerlendirilen kayıt kümesini belirtir. Bu, aşağıdaki tabloda belirtildiği gibi çalışma alanı veya uygulamadaki her kayıtta standart bir özellik tarafından tanımlanır.

| Konum | Özellik |
|:---|:---|
| Log Analytics çalışma alanı          | TimeGenerated |
| Uygulama Öngörüleri uygulaması | timestamp     |

Log Analytics penceresinin üst kısmındaki zaman seçicisinden seçerek zaman aralığını ayarlayın.  Belirli bir zaman aralığını belirtmek için önceden tanımlanmış bir dönem seçebilir veya **Özel'i** seçebilirsiniz.

![Zaman seçici](media/scope/time-picker.png)

Sorguda yukarıdaki tabloda gösterildiği gibi standart saat özelliğini kullanan bir filtre ayarlarsanız, zaman seçici **sorguda**Ayarla'da değişir ve zaman seçici devre dışı bırakılır. Bu durumda, sonraki işlemlerin yalnızca filtrelenmiş kayıtlarla çalışması gerekmesi için filtreyi sorgunun en üstüne koymak en verimli yöntemdir.

![Filtreuygulanmış sorgu](media/scope/query-filtered.png)

Başka bir çalışma [alanından](workspace-expression.md) veya uygulamadan veri almak için çalışma alanını veya [uygulama](app-expression.md) komutunu kullanırsanız, zaman seçici farklı davranabilir. Kapsam bir Log Analytics çalışma alanıysa ve **uygulamayı**kullanıyorsanız veya kapsam bir Application Insights uygulamasıysa ve **çalışma alanı**kullanıyorsanız, Log Analytics filtrede kullanılan özelliğin zaman filtresini belirlemesi gerektiğini anlamayabilir.

Aşağıdaki örnekte, kapsam bir Log Analytics çalışma alanına ayarlanmıştır.  Sorgu, başka bir Log Analytics çalışma alanından veri almak için **çalışma alanını** kullanır. Zaman seçici, beklenen **TimeGenerated** özelliğini kullanan bir filtre gördüğünden **sorguda** Ayarla'ya değişir.

![Çalışma alanıyla sorgula](media/scope/query-workspace.png)

Sorgu, uygulama öngörüleri uygulamasından veri almak için **uygulamayı** kullanıyorsa, Log Analytics filtredeki **zaman damgası** özelliğini tanımaz ve zaman seçici değişmeden kalır. Bu durumda, her iki filtre uygulanır. Örnekte, **where** clause'da 7 gün belirtilmiş olsa bile, yalnızca son 24 saat içinde oluşturulan kayıtlar sorguya dahil edilir.

![Uygulamayla sorgula](media/scope/query-app.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalında Log Analytics'i kullanma yla ilgili bir öğreticiye yürüyün.](get-started-portal.md)
- [Sorgu yazma konusunda](get-started-queries.md)bir öğretici ile yürüyün.
