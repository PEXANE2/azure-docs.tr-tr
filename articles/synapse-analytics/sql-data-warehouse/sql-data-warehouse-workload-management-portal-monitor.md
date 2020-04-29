---
title: İş yükü yönetimi portalı izleme
description: Azure SYNAPSE Analytics 'te iş yükü yönetimi portalı izleme Kılavuzu.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: a79e6fb2be717b5ecee243b26824039630e1a416
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80744281"
---
# <a name="azure-synapse-analytics--workload-management-portal-monitoring-preview"></a>Azure SYNAPSE Analytics – Iş yükü Yönetim Portalı Izleme (Önizleme)

Bu makalede [iş yükü grubu](sql-data-warehouse-workload-isolation.md#workload-groups) kaynak kullanımı ve sorgu etkinliğinin nasıl izleneceği açıklanır.
Azure Ölçüm Gezgini yapılandırma hakkında ayrıntılı bilgi için bkz. [Azure ile çalışmaya başlama Ölçüm Gezgini](../../azure-monitor/platform/metrics-getting-started.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) makalesi.  Sistem kaynak tüketiminin nasıl izleneceği hakkında ayrıntılı bilgi edinmek için Azure SYNAPSE Analytics Izleme belgelerindeki [kaynak kullanımı](sql-data-warehouse-concept-resource-utilization-query-activity.md#resource-utilization) bölümüne bakın.
İş yükü yönetimini izlemek için belirtilen iki farklı iş yükü grubu ölçümü kategorisi vardır: kaynak ayırma ve sorgu etkinliği.  Bu ölçümler, iş yükü grubuna göre bölünebilir ve filtrelenebilir.  Ölçümler, sistem tanımlı (kaynak sınıfı iş yükü grupları) veya Kullanıcı tanımlı ( [Iş yükü grubu oluşturma](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) sözdizimi olan kullanıcı tarafından oluşturulan) temelinde ayrılabilir ve filtrelenebilir.

## <a name="workload-management-metric-definitions"></a>İş yükü yönetimi Ölçüm tanımları

|Ölçüm adı                    |Açıklama  |Toplama Türü |
|-------------------------------|-------------|-----------------|
|Etkin uç kaynak yüzdesi | *Etkin sınır yüzdesi* , diğer iş yükü grupları için ayrılan *En düşük kaynak yüzdesine* bağlı olarak, iş yükü grubu tarafından erişilebilen kaynakların yüzdesine yönelik sabit bir sınır olur. *Etkin uç kaynak yüzdesi* ölçümü, `CAP_PERCENTAGE_RESOURCE` [iş yükü grubu oluşturma](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) sözdiziminde parametresi kullanılarak yapılandırılır.  Geçerli değer burada açıklanmıştır.<br><br>Örneğin, = `DataLoads` 100 ile `CAP_PERCENTAGE_RESOURCE` bir iş yükü grubu oluşturulduysa ve geçerli bir en düşük kaynak yüzdesi %25 olan başka bir iş yükü grubu oluşturulduysa, `DataLoads` iş yükü grubu için *geçerli sınır kaynak* yüzdesi %75 ' dir.<br><br>*Etkin sınır kaynağı yüzdesi* , iş yükü grubunun elde edilebileceği eşzamanlılık (ve dolayısıyla potansiyel aktarım hızı) üst sınırını belirler.  Geçerli *sınır kaynak yüzdesi* ölçümü tarafından şu anda raporlanan ek aktarım hızı gerekiyorsa, daha fazla kaynak eklemek için, `CAP_PERCENTAGE_RESOURCE`diğer iş yükü gruplarının `MIN_PERCENTAGE_RESOURCE` sayısını azaltın veya örneği ölçeğini artırın.  `REQUEST_MIN_RESOURCE_GRANT_PERCENT` Azaltmak eşzamanlılık artırabilir, ancak genel aktarım hızını artıramayabilir.| Min, AVG, Max |
|Geçerli Min kaynak yüzdesi |*Geçerli Min kaynak yüzdesi* , hizmet düzeyi en düşük hesaba sahip iş yükü grubu için ayrılan ve yalıtılmış kaynakların en düşük yüzdesidir.  Etkin Min kaynak yüzdesi ölçümü, `MIN_PERCENTAGE_RESOURCE` [Iş yükü grubu oluşturma](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) sözdiziminde parametresi kullanılarak yapılandırılır.  Geçerli değer [burada](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest#effective-values)açıklanmıştır.<br><br>Bu ölçüm filtrelenmemiş olduğunda Sum toplama türünü kullanın ve sistemde yapılandırılan toplam iş yükü yalıtımını izlemek için bölmeyi kaldırın.<br><br>*Geçerli Min kaynak yüzdesi* , garantili eşzamanlılık (ve bu nedenle garanti edilen aktarım hızı) için bir iş yükü grubunun alabileceği alt sınırı belirler.  Geçerli *En düşük kaynak yüzdesi* ölçümü tarafından şu anda raporlanan ek garantili kaynaklar gerekliyse, iş yükü grubu için yapılandırılmış `MIN_PERCENTAGE_RESOURCE` parametreyi artırın.  `REQUEST_MIN_RESOURCE_GRANT_PERCENT` Azaltmak eşzamanlılık artırabilir, ancak genel aktarım hızını artıramayabilir. |Min, AVG, Max|
|İş yükü grubu etkin sorguları  |Bu ölçüm, iş yükü grubu içindeki etkin sorguları raporlar.  Bu metriğin filtrelenmemiş olarak kullanılması, sistemde çalışan tüm etkin sorguları görüntüler.|Toplam         |
|Maksimum kaynak yüzdesi bazında iş yükü grubu ayırması |Bu ölçüm, toplam iş yükü grubu başına düşen *etkin sınır kaynağına* göre kaynakların yüzde ayırmasını görüntüler.  Bu ölçüm, iş yükü grubunun etkin kullanımını sağlar.<br><br>%75 ve% `DataLoads` 25 ' lik `REQUEST_MIN_RESOURCE_GRANT_PERCENT` yapılandırılmış *etkin bir sınır kaynağına* sahip bir iş yükü grubu düşünün.  `DataLoads` Filtrelenen *en fazla kaynak yüzdesi değerine göre ayrılan iş yükü grubu ayırması* %33 (%25/75%) olacaktır Bu iş yükü grubunda tek bir sorgu çalışıyorsa.<br><br>İş yükü grubunun kullanımını belirlemek için bu ölçümü kullanın.  %100 ' e yakın bir değer, iş yükü grubu için kullanılabilir tüm kaynakların kullanıldığını gösterir.  Ayrıca, sıfırdan büyük bir değeri gösteren aynı iş yükü grubu için *Iş yükü grubu sıraya alınmış sorgular ölçümü* , iş yükü grubunun ayrılsa ek kaynakları kullanacağındığını gösterir.  Buna karşılık, bu ölçüm sürekli düşüktür ve *Iş yükü grubu etkin sorguları* düşükse iş yükü grubu kullanılmaz.  Bu durum özellikle, etkin bir *uç kaynak yüzdesi* sıfırdan büyükse sorunlu olur ve bu da [aşırı kullanılan iş yükü yalıtımı](#underutilized-workload-isolation)olduğunu gösterir.|Min, AVG, Max |
|Sistem iş yükü grubuna göre ayırma yüzdesi | Bu ölçüm, tüm sisteme göre kaynakların yüzde ayırmasını görüntüler.<br><br>%25 ' te `DataLoads` yapılandırılmış bir `REQUEST_MIN_RESOURCE_GRANT_PERCENT` iş yükü grubunu düşünün.  Filtre uygulanan `DataLoads` *sistem yüzdesi değerine göre iş yükü grubu ayırması* %25 (%25/100%) Bu iş yükü grubunda tek bir sorgu çalışıyorsa.|Min, AVG, Max |
|İş yükü grubu sorgu zaman aşımları |Zaman aşımına uğramış iş yükü grubu için sorgular.  Bu ölçüm tarafından bildirilen sorgu zaman aşımları yalnızca sorgu yürütülmeye başladıktan sonra (kilitleme veya kaynak bekleme nedeniyle bekleme süresi içermez).<br><br>Sorgu zaman aşımı, `QUERY_EXECUTION_TIMEOUT_SEC` [Iş yükü grubu oluşturma](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) sözdiziminde parametresi kullanılarak yapılandırılır.  Değerin artırılması sorgu zaman aşımı sayısını azaltabilir.<br><br>Zaman aşımı miktarını `REQUEST_MIN_RESOURCE_GRANT_PERCENT` azaltmak ve sorgu başına daha fazla kaynak ayırmak için iş yükü grubunun parametresini artırmayı göz önünde bulundurun.  , Artırma `REQUEST_MIN_RESOURCE_GRANT_PERCENT` , iş yükü grubu için eşzamanlılık miktarını azaltır. |Toplam |
|İş yükü grubu sıraya alınmış sorgular | Şu anda yürütmenin yürütülmeye başlamasını bekleyen sıraya alınmış iş yükü grubu için sorgular.  Sorgular, kaynakları veya kilitleri beklediği için kuyruk olabilir.<br><br>Sorgular çok sayıda nedenden dolayı bekleniyor.  Sistem aşırı yüklenmişse ve eşzamanlılık talebi kullanılabilir olandan fazlaysa sorgular sıraya alınır.<br><br>İş yükü grubu `CAP_PERCENTAGE_RESOURCE` [Oluştur](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) deyimindeki parametreyi artırarak iş yükü grubuna daha fazla kaynak eklemeyi düşünün.  `CAP_PERCENTAGE_RESOURCE` , *Etkin sınır kaynak yüzdesi* ölçüsüne fazlaysa, diğer iş yükü grubu için yapılandırılan iş yükü yalıtımı, bu iş yükü grubuna ayrılan kaynakları etkiler.  Diğer iş `MIN_PERCENTAGE_RESOURCE` yükü gruplarını azaltmayı veya daha fazla kaynak eklemek için örneği ölçeklendirmenizi düşünün. |Toplam |

## <a name="monitoring-scenarios-and-actions"></a>İzleme senaryoları ve eylemler

Sorunu gidermek için ilgili eylemlerle birlikte sorun giderme için iş yükü yönetimi ölçüm kullanımını vurgulayabilecek bir dizi grafik yapılandırması aşağıda verilmiştir.

### <a name="underutilized-workload-isolation"></a>Aşırı kullanılan iş yükü yalıtımı

Adlı `wgPriority` bir iş yükü grubunun oluşturulduğu ve *TheCEO* `membername` `wcCEOPriority` iş yükü Sınıflandırıcısı kullanılarak onunla eşlendiği aşağıdaki iş yükü grubunu ve sınıflandırıcı yapılandırmasını göz önünde bulundurun.  `wgPriority` İş yükü grubu için %25 iş yükü yalıtımı yapılandırıldı (`MIN_PERCENTAGE_RESOURCE` = 25).  *TheCEO* tarafından gönderilen her sorguya sistem kaynaklarının %5 ' i (`REQUEST_MIN_RESOURCE_GRANT_PERCENT` = 5) verilir.

```sql
CREATE WORKLOAD GROUP wgPriority
WITH ( MIN_PERCENTAGE_RESOURCE = 25
      ,CAP_PERCENTAGE_RESOURCE = 50
      ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 5);

CREATE WORKLOAD CLASSIFIER wcCEOPriority
WITH ( WORKLOAD_GROUP = 'wgPriority'
      ,MEMBERNAME = 'TheCEO');
```

Aşağıdaki grafik aşağıdaki şekilde yapılandırılır:<br>
Ölçüm 1: *geçerli en düşük kaynak yüzdesi* (ortalama toplama `blue line`,)<br>
Ölçüm 2: *sisteme göre Iş yükü grubu ayırma yüzdesi* (ort toplama `purple line`,)<br>
Filtre: [Iş yükü grubu] =`wgPriority`<br>
![Underutilized-WG. png](./media/sql-data-warehouse-workload-management-portal-monitor/underutilized-wg.png) grafik, %25 iş yükü yalıtımına sahip olduğunu gösteriyorsa, ortalama olarak yalnızca %10 kullanılır.  Bu durumda, `MIN_PERCENTAGE_RESOURCE` parametre değeri 10 veya 15 arasında düşürülemez ve sistemdeki diğer iş yüklerinin kaynakları kullanmasına izin verebilir.

### <a name="workload-group-bottleneck"></a>İş yükü grubu performans sorunu

Adlı `wgDataAnalyst` bir iş yükü grubunun oluşturulduğu ve *veri analistinin* `membername` `wcDataAnalyst` iş yükü Sınıflandırıcısı kullanılarak kendisine eşlendiği, aşağıdaki iş yükü grubunu ve sınıflandırıcı yapılandırmasını göz önünde bulundurun.  `wgDataAnalyst` İş yükü grubu için %6 iş yükü yalıtımı yapılandırıldı (`MIN_PERCENTAGE_RESOURCE` = 6) ve %9 (`CAP_PERCENTAGE_RESOURCE` = 9) kaynak sınırı vardır.  *Veri analisti* tarafından gönderilen her sorguya sistem kaynaklarının %3 ' i (`REQUEST_MIN_RESOURCE_GRANT_PERCENT` = 3) verilir.

```sql
CREATE WORKLOAD GROUP wgDataAnalyst  
WITH ( MIN_PERCENTAGE_RESOURCE = 6
      ,CAP_PERCENTAGE_RESOURCE = 9
      ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 3);

CREATE WORKLOAD CLASSIFIER wcDataAnalyst
WITH ( WORKLOAD_GROUP = 'wgDataAnalyst'
      ,MEMBERNAME = 'DataAnalyst');
```

Aşağıdaki grafik aşağıdaki şekilde yapılandırılır:<br>
Ölçüm 1: *etkin uç kaynak yüzdesi* (ortalama toplama, `blue line`)<br>
Ölçüm 2: *en fazla kaynak yüzdesine göre Iş yükü grubu ayırması* (ortalama `purple line`toplama,)<br>
Ölçüm 3: *Iş yükü grubu sıraya alınmış sorgular* (Toplam `turquoise line`toplama,)<br>
Filtre: [Iş yükü grubu] =`wgDataAnalyst`<br>
![şişe-Nelen-WG](./media/sql-data-warehouse-workload-management-portal-monitor/bottle-necked-wg.png) -grafik, kaynak üzerinde %9 üst sınır olduğunu gösteriyor, iş yükü grubu %90 ' dır ( *iş yükü grubu ayırmayı en fazla kaynak yüzdesi ölçüsüne göre*).  *Sıraya alınmış sorgular ölçümünün Iş yükü grubundan*gösterildiği gibi düzenli bir sorgu sırası vardır.  Bu durumda, ' ı% `CAP_PERCENTAGE_RESOURCE` 9 ' dan büyük bir değere yükseltmek daha fazla sorgunun eşzamanlı olarak yürütülmesine izin verir.  Arttırmak için `CAP_PERCENTAGE_RESOURCE` yeterli kaynak olduğunu ve diğer iş yükü grupları tarafından yalıtılmamış olduğunu varsaymaktadır.  *Etkin uç kaynak yüzdesi ölçümünü*denetleyerek, Cap 'in arttığını doğrulayın.  Daha fazla işleme isteniyorsa, öğesini 3 ' ten büyük `REQUEST_MIN_RESOURCE_GRANT_PERCENT` bir değere artırmayı de göz önünde bulundurun.  ' Nin `REQUEST_MIN_RESOURCE_GRANT_PERCENT` artırılması sorguların daha hızlı çalışmasına izin verebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı başlangıç: T-SQL kullanarak iş yükü yalıtımını yapılandırma](quickstart-configure-workload-isolation-tsql.md)<br>
- [Iş yükü grubu oluşturma (Transact-SQL)](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)<br>
- [Iş yükü SıNıFLANDıRıCıSı oluşturma (Transact-SQL)](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)<br>
- [İzleme kaynak kullanımı](sql-data-warehouse-concept-resource-utilization-query-activity.md)
