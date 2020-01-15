---
title: İş yükü yönetimi portalı izleme
description: Azure SYNAPSE Analytics 'te iş yükü yönetimi portalı izleme Kılavuzu.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload-management
ms.date: 01/13/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: seo-lt-2019
ms.openlocfilehash: f3baaab59031c4cfad036a7181318502d1969715
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75942431"
---
# <a name="azure-synapse-analytics--workload-management-portal-monitoring-preview"></a>Azure SYNAPSE Analytics – Iş yükü Yönetim Portalı Izleme (Önizleme)
Bu makalede [iş yükü grubu](sql-data-warehouse-workload-isolation.md#workload-groups) kaynak kullanımı ve sorgu etkinliğinin nasıl izleneceği açıklanır. Azure Ölçüm Gezgini yapılandırma hakkında ayrıntılı bilgi için bkz. [Azure ile çalışmaya başlama Ölçüm Gezgini](../azure-monitor/platform/metrics-getting-started.md) makalesi.  Sistem kaynak tüketiminin nasıl izleneceği hakkında ayrıntılı bilgi edinmek için Azure SQL veri ambarı Izleme belgelerindeki [kaynak kullanımı](sql-data-warehouse-concept-resource-utilization-query-activity.md#resource-utilization) bölümüne bakın.
İş yükü yönetimini izlemek için belirtilen iki farklı iş yükü grubu ölçümü kategorisi vardır: kaynak ayırma ve sorgu etkinliği.  Bu ölçümler, iş yükü grubuna göre bölünebilir ve filtrelenebilir.  Ölçümler, sistem tanımlı (kaynak sınıfı iş yükü grupları) veya Kullanıcı tanımlı ( [Iş yükü grubu oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest) sözdizimi olan kullanıcı tarafından oluşturulan) temelinde ayrılabilir ve filtrelenebilir.

## <a name="workload-management-metric-definitions"></a>İş yükü yönetimi Ölçüm tanımları

|Ölçüm Adı                    |Açıklama  |Toplama Türü |
|-------------------------------|-------------|-----------------|
|Etkin uç kaynak yüzdesi | *Etkin sınır yüzdesi* , diğer iş yükü grupları için ayrılan *En düşük kaynak yüzdesine* bağlı olarak, iş yükü grubu tarafından erişilebilen kaynakların yüzdesine yönelik sabit bir sınır olur. *Etkin sınır kaynak yüzdesi* ölçümü, [Iş yükü grubu oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest) sözdiziminde `CAP_PERCENTAGE_RESOURCE` parametresi kullanılarak yapılandırılır.  Geçerli değer burada açıklanmıştır.<br><br>Örneğin, bir iş yükü grubu `DataLoads` `CAP_PERCENTAGE_RESOURCE` = 100 ile oluşturulduysa ve geçerli bir en düşük kaynak yüzdesi %25 ' lik bir iş yükü grubu oluşturulduysa, `DataLoads` iş yükü grubu için *geçerli sınır kaynağı* %75 ' dir.<br><br>*Etkin sınır kaynağı yüzdesi* , iş yükü grubunun elde edilebileceği eşzamanlılık (ve dolayısıyla potansiyel aktarım hızı) üst sınırını belirler.  Geçerli *sınır kaynak yüzdesi* ölçümü tarafından şu anda raporlanan ek aktarım hızı gerekliyse, `CAP_PERCENTAGE_RESOURCE`artırın, diğer iş yükü gruplarının `MIN_PERCENTAGE_RESOURCE` azaltın veya daha fazla kaynak eklemek için örneği ölçeklendirin.  `REQUEST_MIN_RESOURCE_GRANT_PERCENT` düşürmek eşzamanlılık artırabilir, ancak genel performansı artıramayabilir.| Min, AVG, Max |
|Geçerli Min kaynak yüzdesi |*Geçerli Min kaynak yüzdesi* , hizmet düzeyi en düşük hesaba sahip iş yükü grubu için ayrılan ve yalıtılmış kaynakların en düşük yüzdesidir.  Etkin Min kaynak yüzdesi ölçümü, [Iş yükü grubu oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest) sözdiziminde `MIN_PERCENTAGE_RESOURCE` parametresi kullanılarak yapılandırılır.  Geçerli değer [burada](https://docs.microsoft.com/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest#effective-values)açıklanmıştır.<br><br>Bu ölçüm filtrelenmemiş olduğunda Sum toplama türünü kullanın ve sistemde yapılandırılan toplam iş yükü yalıtımını izlemek için bölmeyi kaldırın.<br><br>*Geçerli Min kaynak yüzdesi* , garantili eşzamanlılık (ve bu nedenle garanti edilen aktarım hızı) için bir iş yükü grubunun alabileceği alt sınırı belirler.  Geçerli *En düşük kaynak yüzdesi* ölçümü tarafından şu anda raporlanan ek garantili kaynaklar gerekliyse, iş yükü grubu için yapılandırılan `MIN_PERCENTAGE_RESOURCE` parametresini arttırın.  `REQUEST_MIN_RESOURCE_GRANT_PERCENT` düşürmek eşzamanlılık artırabilir, ancak genel performansı artıramayabilir. |Min, AVG, Max|
|İş yükü grubu etkin sorguları  |Bu ölçüm, iş yükü grubu içindeki etkin sorguları raporlar.  Bu metriğin filtrelenmemiş olarak kullanılması, sistemde çalışan tüm etkin sorguları görüntüler.|Toplam         |
|Maksimum kaynak yüzdesi bazında iş yükü grubu ayırması |Bu ölçüm, toplam iş yükü grubu başına düşen *etkin sınır kaynağına* göre kaynakların yüzde ayırmasını görüntüler.  Bu ölçüm, iş yükü grubunun etkin kullanımını sağlar.<br><br>%75 ve %25 oranında yapılandırılan bir `REQUEST_MIN_RESOURCE_GRANT_PERCENT` *etkin bir sınır kaynağıyla* `DataLoads` bir iş yükü grubu düşünün.  `DataLoads` filtrelenmiş *Maksimum kaynak yüzdesi değerine göre Iş yükü grubu ayırması* %33 (%25/75%) olacaktır Bu iş yükü grubunda tek bir sorgu çalışıyorsa.<br><br>İş yükü grubunun kullanımını belirlemek için bu ölçümü kullanın.  %100 ' e yakın bir değer, iş yükü grubu için kullanılabilir tüm kaynakların kullanıldığını gösterir.  Ayrıca, sıfırdan büyük bir değeri gösteren aynı iş yükü grubu için *Iş yükü grubu sıraya alınmış sorgular ölçümü* , iş yükü grubunun ayrılsa ek kaynakları kullanacağındığını gösterir.  Buna karşılık, bu ölçüm sürekli düşüktür ve *Iş yükü grubu etkin sorguları* düşükse iş yükü grubu kullanılmaz.  Bu durum özellikle, etkin bir *uç kaynak yüzdesi* sıfırdan büyükse sorunlu olur ve bu da [aşırı kullanılan iş yükü yalıtımı](#underutilized-workload-isolation)olduğunu gösterir.|Min, AVG, Max |
|Sistem iş yükü grubuna göre ayırma yüzdesi | Bu ölçüm, tüm sisteme göre kaynakların yüzde ayırmasını görüntüler.<br><br>%25 ' te yapılandırılmış bir `REQUEST_MIN_RESOURCE_GRANT_PERCENT` `DataLoads` bir iş yükü grubu düşünün.  `DataLoads` filtrelenmiş *sistem yüzdesi değerine göre Iş yükü grubu ayırması* %25 (%25/100%) olacaktır Bu iş yükü grubunda tek bir sorgu çalışıyorsa.|Min, AVG, Max |
|İş yükü grubu sorgu zaman aşımları |Zaman aşımına uğramış iş yükü grubu için sorgular.  Bu ölçüm tarafından bildirilen sorgu zaman aşımları yalnızca sorgu yürütülmeye başladıktan sonra (kilitleme veya kaynak bekleme nedeniyle bekleme süresi içermez).<br><br>Sorgu zaman aşımı, [Iş yükü grubu oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest) sözdiziminde `QUERY_EXECUTION_TIMEOUT_SEC` parametresi kullanılarak yapılandırılır.  Değerin artırılması sorgu zaman aşımı sayısını azaltabilir.<br><br>Zaman aşımı miktarını azaltmak ve sorgu başına daha fazla kaynak ayırmak için iş yükü grubunun `REQUEST_MIN_RESOURCE_GRANT_PERCENT` parametresini artırmayı göz önünde bulundurun.  Bu, artırma `REQUEST_MIN_RESOURCE_GRANT_PERCENT` iş yükü grubu için eşzamanlılık miktarını azaltır. |Toplam |
|İş yükü grubu sıraya alınmış sorgular | Şu anda yürütmenin yürütülmeye başlamasını bekleyen sıraya alınmış iş yükü grubu için sorgular.  Sorgular, kaynakları veya kilitleri beklediği için kuyruk olabilir.<br><br>Sorgular çok sayıda nedenden dolayı bekleniyor.  Sistem aşırı yüklenmişse ve eşzamanlılık talebi kullanılabilir olandan fazlaysa sorgular sıraya alınır.<br><br>İş yükü grubu [Oluştur](https://docs.microsoft.com/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest) deyimindeki `CAP_PERCENTAGE_RESOURCE` parametresini artırarak iş yükü grubuna daha fazla kaynak eklemeyi düşünün.  `CAP_PERCENTAGE_RESOURCE`, *etkin sınır kaynak yüzdesi* ölçüsüne fazlaysa, diğer iş yükü grubu için yapılandırılan iş yükü yalıtımı, bu iş yükü grubuna ayrılan kaynakları etkiler.  Diğer iş yükü gruplarının `MIN_PERCENTAGE_RESOURCE` kısmayı veya daha fazla kaynak eklemek için örneği ölçeğini azaltmayı deneyin. |Toplam |

## <a name="monitoring-scenarios-and-actions"></a>İzleme senaryoları ve eylemler
Sorunu gidermek için ilgili eylemlerle birlikte sorun giderme için iş yükü yönetimi ölçüm kullanımını vurgulayabilecek bir dizi grafik yapılandırması aşağıda verilmiştir.

### <a name="underutilized-workload-isolation"></a>Aşırı kullanılan iş yükü yalıtımı
`wgPriority` adlı bir iş yükü grubunun oluşturulduğu ve *TheCEO* `membername` `wcCEOPriority` iş yükü Sınıflandırıcısı kullanılarak onunla eşlendiği aşağıdaki iş yükü grubunu ve sınıflandırıcı yapılandırmasını göz önünde bulundurun.  `wgPriority` iş yükü grubu için yapılandırılmış %25 iş yükü yalıtımı vardır (`MIN_PERCENTAGE_RESOURCE` = 25).  *TheCEO* tarafından gönderilen her sorguya sistem kaynaklarının %5 ' i verilir (`REQUEST_MIN_RESOURCE_GRANT_PERCENT` = 5).
```sql
CREATE WORKLOAD GROUP wgPriority 
WITH ( MIN_PERCENTAGE_RESOURCE = 25   
      ,CAP_PERCENTAGE_RESOURCE = 50 
      ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 5); 

CREATE WORKLOAD CLASSIFIER wcCEOPriority 
WITH ( WORKLOAD_GROUP = 'wgPriority'
      ,MEMBERNAME = 'TheCEO');
```
Aşağıdaki grafik şu şekilde yapılandırılmıştır: ölçüm 1: *geçerli en düşük kaynak yüzdesi* (ortalama toplama, `blue line`) ölçüm 2: *sistem iş yükü grubu ayırması yüzdesi* (ortalama toplama, `purple line`) filtresi: [iş yükü grubu] = `wgPriority`
![Underutilized-WG. png](media/sql-data-warehouse-workload-management-portal-monitor/underutilized-wg.png) grafik, yalnızca %10 ' un ortalama üzerinde kullanıldığını gösterir.  Bu durumda, `MIN_PERCENTAGE_RESOURCE` parametresi değeri 10 veya 15 arasında düşürülemez ve sistemdeki diğer iş yüklerinin kaynakları kullanmasına izin verebilir.

### <a name="workload-group-bottleneck"></a>İş yükü grubu performans sorunu
`wgDataAnalyst` adlı bir iş yükü grubunun oluşturulduğu ve *veri analistinin* `wcDataAnalyst` iş yükü Sınıflandırıcısı kullanılarak `membername` eşlendiği, aşağıdaki iş yükü grubunu ve sınıflandırıcı yapılandırmasını göz önünde bulundurun.  `wgDataAnalyst` iş yükü grubu için %6 iş yükü yalıtımı yapılandırıldı (`MIN_PERCENTAGE_RESOURCE` = 6) ve %9 (`CAP_PERCENTAGE_RESOURCE` = 9) kaynak sınırı vardır.  *Veri analisti* tarafından gönderilen her sorguya sistem kaynaklarının %3 ' i (`REQUEST_MIN_RESOURCE_GRANT_PERCENT` = 3) verilir.

```sql
CREATE WORKLOAD GROUP wgDataAnalyst  
WITH ( MIN_PERCENTAGE_RESOURCE = 6   
      ,CAP_PERCENTAGE_RESOURCE = 9 
      ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 3); 

CREATE WORKLOAD CLASSIFIER wcDataAnalyst 
WITH ( WORKLOAD_GROUP = 'wgDataAnalyst'
      ,MEMBERNAME = 'DataAnalyst');
```
Aşağıdaki grafik şu şekilde yapılandırılır: ölçüm 1: *etkin uç kaynak yüzdesi* (ort toplama, `blue line`) ölçüm 2: *en fazla kaynak yüzdesi tarafından iş yükü grubu ayırması* (ortalama toplama, `purple line`) ölçüm 3: *iş yükü grubu sıraya alınmış sorgular* (Sum toplama, `turquoise line`) filtresi: [iş yükü grubu] = `wgDataAnalyst`
![şişe-nectes-WG](media/sql-data-warehouse-workload-management-portal-monitor/bottle-necked-wg.png) grafik, iş yükü grubunun %9 ' luk bir *üst sınırı Kaynak yüzdesi ölçümü*).  *Sıraya alınmış sorgular ölçümünün Iş yükü grubundan*gösterildiği gibi düzenli bir sorgu sırası vardır.  Bu durumda, `CAP_PERCENTAGE_RESOURCE` %9 ' dan büyük bir değere yükseltmek daha fazla sorgunun eşzamanlı olarak yürütülmesine izin verir.  `CAP_PERCENTAGE_RESOURCE` artırılması, kullanılabilir kaynakların ve diğer iş yükü gruplarının yalıtılmasının yeterli olduğunu varsayar.  *Etkin uç kaynak yüzdesi ölçümünü*denetleyerek, Cap 'in arttığını doğrulayın.  Daha fazla işleme isteniyorsa, `REQUEST_MIN_RESOURCE_GRANT_PERCENT` 3 ' ten büyük bir değere artırmayı de göz önünde bulundurun.  `REQUEST_MIN_RESOURCE_GRANT_PERCENT` artırılması sorguların daha hızlı çalışmasına izin verebilir.

## <a name="next-steps"></a>Sonraki adımlar
[Hızlı başlangıç: T-SQL kullanarak iş yükü yalıtımını yapılandırma](quickstart-configure-workload-isolation-tsql.md)<br>
[Iş yükü grubu oluşturma (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest)<br>
[Iş yükü SıNıFLANDıRıCıSı oluşturma (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-workload-classifier-transact-sql?view=azure-sqldw-latest)<br>
[İzleme kaynak kullanımı](sql-data-warehouse-concept-resource-utilization-query-activity.md)

