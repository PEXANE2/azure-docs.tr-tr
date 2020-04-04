---
title: İş yükü yönetimi portalı izleme
description: Azure Synapse Analytics'te iş yükü yönetimi portalı izleme kılavuzu.
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
ms.openlocfilehash: b1f21a996f7394def4d6b1e8bde9a5ccdf703dbb
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632419"
---
# <a name="azure-synapse-analytics--workload-management-portal-monitoring-preview"></a>Azure Synapse Analytics – İş Yükü Yönetimi Portalı İzleme (Önizleme)

Bu [makalede, iş yükü grubu](sql-data-warehouse-workload-isolation.md#workload-groups) kaynak kullanımı ve sorgu etkinliği nasıl izlenir açıklar.
Azure Ölçümleri Gezgini'ni yapılandırma hakkında ayrıntılı bilgi için [Azure Ölçümleri Gezgini makalesiyle başlarken](../../azure-monitor/platform/metrics-getting-started.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) bkz.  Sistem kaynak tüketimini nasıl izleyeceğine ilişkin ayrıntılar için Azure Synapse Analytics Monitoring belgelerindeki [Kaynak kullanımı](sql-data-warehouse-concept-resource-utilization-query-activity.md#resource-utilization) bölümüne bakın.
İş yükü yönetimini izlemek için sağlanan iki farklı iş yükü grubu ölçütleri vardır: kaynak ayırma ve sorgu etkinliği.  Bu ölçümler iş yükü grubuna göre bölünebilir ve filtrelenebilir.  Ölçümler, sistem tanımlı (kaynak sınıfı iş yükü grupları) veya kullanıcı tanımlı [(CREATE İŞ YÜKÜ GRUBU](https://docs.microsoft.com/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest) sözdizimi ile kullanıcı tarafından oluşturulan) olarak bölünebilir ve filtrelenebilir.

## <a name="workload-management-metric-definitions"></a>İş yükü yönetimi metrik tanımları

|Metrik Adı                    |Açıklama  |Toplama Türü |
|-------------------------------|-------------|-----------------|
|Etkili kap kaynak yüzdesi | *Etkin kap kaynak yüzdesi,* diğer iş yükü grupları için ayrılan *etkili min kaynak yüzdesini* hesaba katarak, iş yükü grubu tarafından erişilebilen kaynakların yüzdesi üzerinde sabit bir sınırdır. *Etkin kap kaynak yüzdesi* [ölçümü, CREATE İş YÜKÜ GRUBU](https://docs.microsoft.com/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest) sözdiziminde `CAP_PERCENTAGE_RESOURCE` parametre kullanılarak yapılandırılır.  Etkin değeri burada açıklanmıştır.<br><br>Örneğin= 100 `DataLoads` ile `CAP_PERCENTAGE_RESOURCE` bir iş yükü grubu oluşturulursa ve etkili bir min kaynak yüzdesi %25 `DataLoads` ile başka bir iş yükü grubu oluşturulursa, iş yükü grubu için *etkin kap kaynağı yüzdesi* %75'tir.<br><br>*Etkin kap kaynağı yüzdesi,* bir iş yükü grubunun elde edebileceği eşzamanlıbirimin (ve dolayısıyla potansiyel iş kaynağının) üst sınırını belirler.  *Etkili kap kaynak yüzdesi* ölçümü tarafından şu anda bildirilenin ötesinde `CAP_PERCENTAGE_RESOURCE`ek iş `MIN_PERCENTAGE_RESOURCE` kaynağı gerekiyorsa, diğer iş yükü gruplarının azaltını veya daha fazla kaynak eklemek için örneği ölçeklendirin.  Azalan `REQUEST_MIN_RESOURCE_GRANT_PERCENT` eşzamanlılık artırabilir, ancak genel iş bilgili artış olmayabilir.| Min, Avg, Max |
|Etkili min kaynak yüzdesi |*Etkin min kaynak yüzdesi,* hizmet düzeyi minimumunu dikkate alarak iş yükü grubu için ayrılmış ve yalıtılmış kaynakların minimum yüzdesidir.  Etkin min kaynak yüzdesi [ölçümü, CREATE İş YÜKÜ GRUBU](https://docs.microsoft.com/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest) sözdiziminde `MIN_PERCENTAGE_RESOURCE` parametre kullanılarak yapılandırılır.  Etkin değeri [burada](https://docs.microsoft.com/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest#effective-values)açıklanmıştır.<br><br>Bu metrik filtreuygulanmadığında ve sistemde yapılandırılan toplam iş yükü yalıtımını izlemek için bölünmeden önce Toplam toplama türünü kullanın.<br><br>*Etkin min kaynak yüzdesi,* bir iş yükü grubunun elde edebileceği garantili eşzamanlılık (ve dolayısıyla garantili iş kaynağı) alt sınırını belirler.  *Etkili min kaynak yüzdesi* ölçümü tarafından bildirilenin ötesinde ek garantili kaynaklar `MIN_PERCENTAGE_RESOURCE` aranıyorsa, iş yükü grubu için yapılandırılan parametreyi artırın.  Azalan `REQUEST_MIN_RESOURCE_GRANT_PERCENT` eşzamanlılık artırabilir, ancak genel iş bilgili artış olmayabilir. |Min, Avg, Max|
|İş yükü grubu etkin sorguları  |Bu metrik, iş yükü grubundaki etkin sorguları raporlar.  Bu metrik filtresiz ve bölünmemiş kullanarak sistemde çalışan tüm etkin sorguları görüntüler.|Toplam         |
|Maksimum kaynak yüzdeye göre iş yükü grubu tahsisi |Bu metrik, iş yükü grubu başına *Etkin kap kaynak yüzdesine* göre kaynakların yüzde dağılımını görüntüler.  Bu metrik, iş yükü grubunun etkin kullanımını sağlar.<br><br>Etkin bir `DataLoads` kap *kaynağı %75* ve `REQUEST_MIN_RESOURCE_GRANT_PERCENT` %25 yapılandırılmış bir iş yükü grubunu düşünün.  Filtre uygulanmış maksimum kaynak yüzdesi değerine `DataLoads` göre İş Yükü grubu *tahsisi* %33 (%25 / %75) olacaktır bu iş yükü grubunda tek bir sorgu çalışıyorsa.<br><br>İş yükü grubunun kullanımını tanımlamak için bu ölçüt'ün kullanın.  %100'e yakın bir değer, iş yükü grubunun kullanabileceği tüm kaynakların kullanıldığını gösterir.  Ayrıca, aynı iş yükü grubu için sıfırdan büyük bir değer gösteren *Iş Yükü grubu sıralanan sorgular ölçümü,* iş yükü grubunun tahsis edilmesi halinde ek kaynaklardan yararlanacağını gösterir.  Tersine, bu metrik tutarlı olarak düşükse ve *İş Yükü grubu etkin sorguları* düşükse, iş yükü grubu kullanılmaz.  Etkin kap kaynak *yüzdesi* sıfırdan büyükse, bu durum özellikle sorunludur, çünkü [bu, az kullanılan iş yükü yalıtımı](#underutilized-workload-isolation)anlamına geliyor.|Min, Avg, Max |
|Sistem yüzdeye göre iş yükü grup tahsisi | Bu metrik, tüm sisteme göre kaynakların yüzde dağılımını görüntüler.<br><br>%25 `DataLoads` `REQUEST_MIN_RESOURCE_GRANT_PERCENT` yapılandırılmış bir iş yükü grubunu düşünün.  Sistem `DataLoads` *değerine göre iş yükü grup tahsisi* filtrelenen %25 (%25 / %100) olacaktır bu iş yükü grubunda tek bir sorgu çalışıyorsa.|Min, Avg, Max |
|İş yükü grubu sorgu zaman ları |Zaman dolan iş yükü grubu için sorgular.  Bu ölçüm tarafından bildirilen sorgu zaman zaman zaman ları yalnızca sorgu yürütmeye başladıktan sonra olur (kilitleme veya kaynak bekleme nedeniyle bekleme süresini içermez).<br><br>Sorgu zaman, `QUERY_EXECUTION_TIMEOUT_SEC` CREATE İş [YÜKÜ GRUBU](https://docs.microsoft.com/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest) sözdiziminde parametre kullanılarak yapılandırılır.  Değeri artırmak, sorgu zaman larını azaltabilir.<br><br>Zaman aşımlarının miktarını azaltmak ve sorgu başına daha fazla kaynak ayırmak için iş yükü grubunun `REQUEST_MIN_RESOURCE_GRANT_PERCENT` parametresini artırmayı düşünün.  Not, `REQUEST_MIN_RESOURCE_GRANT_PERCENT` artan iş yükü grubu için eşzamanlılık miktarını azaltır. |Toplam |
|İş yükü grubu sıralı sorgular | Yürütmeyi başlatmak için şu anda sıraya dizilen iş yükü grubu için sorgular.  Kaynaklar veya kilitler beklediği için sorgular sıraya alınabilir.<br><br>Sorgular çeşitli nedenlerle bekliyor olabilir.  Sistem aşırı yüklenirse ve eşzamanlılık talebi kullanılabilir olandan daha büyükse, sorgular sıraya alınır.<br><br>[CREATE İş YÜKÜ GRUBU](https://docs.microsoft.com/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest) deyimindeki parametreyi `CAP_PERCENTAGE_RESOURCE` artırarak iş yükü grubuna daha fazla kaynak eklemeyi düşünün.  Etkili `CAP_PERCENTAGE_RESOURCE` *üst kaynak yüzdesi* ölçümünden büyükse, diğer iş yükü grubu için yapılandırılan iş yükü yalıtımı bu iş yükü grubuna ayrılan kaynakları etkiler.  Diğer iş `MIN_PERCENTAGE_RESOURCE` yükü gruplarını düşürmeyi düşünün veya daha fazla kaynak eklemek için örneği ölçeklendirin. |Toplam |

## <a name="monitoring-scenarios-and-actions"></a>Senaryoları ve eylemleri izleme

Aşağıda, sorunu gidermek için ilişkili eylemlerle birlikte sorun giderme için iş yükü yönetimi metrik kullanımını vurgulamak için bir dizi grafik yapılandırması yer almaktadır.

### <a name="underutilized-workload-isolation"></a>Az kullanılan iş yükü yalıtımı

Aşağıdaki iş yükü grubunu ve `wgPriority` sınıflandırıcı yapılandırmasını, adı verilen bir iş yükü grubunun oluşturulduğu ve *TheCEO'nun* `membername` iş yükü sınıflandırıcısını kullanarak eşlendiği yapılandırmayı `wcCEOPriority` düşünün.  İş `wgPriority` yükü grubunda bunun için yapılandırılan %25 iş yükü yalıtımı vardır (=`MIN_PERCENTAGE_RESOURCE` 25).  *TheCEO* tarafından gönderilen her sorgu, sistem`REQUEST_MIN_RESOURCE_GRANT_PERCENT` kaynaklarının %5'i (= 5) verilir.

```sql
CREATE WORKLOAD GROUP wgPriority
WITH ( MIN_PERCENTAGE_RESOURCE = 25
      ,CAP_PERCENTAGE_RESOURCE = 50
      ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 5);

CREATE WORKLOAD CLASSIFIER wcCEOPriority
WITH ( WORKLOAD_GROUP = 'wgPriority'
      ,MEMBERNAME = 'TheCEO');
```

Aşağıdaki grafik aşağıdaki gibi yapılandırılmıştır:<br>
Metrik 1: *Etkili min kaynak yüzdesi* (Avg toplama, `blue line`)<br>
Metrik 2: *Sistem yüzdesine göre iş yükü grup tahsisi* (Avg toplama, `purple line`)<br>
Filtre: [İş Yükü Grubu] =`wgPriority`<br>
![underutilized-wg.png](./media/sql-data-warehouse-workload-management-portal-monitor/underutilized-wg.png) Grafik% 25 iş yükü izolasyon ile, sadece% 10 ortalama kullanıldığını göstermektedir.  Bu durumda, `MIN_PERCENTAGE_RESOURCE` parametre değeri 10 veya 15 arasında düşürülebilir ve sistemdeki diğer iş yüklerinin kaynakları tüketmesine izin verebilir.

### <a name="workload-group-bottleneck"></a>İş yükü grubu darboğaz

Adlı `wgDataAnalyst` bir iş yükü grubunun oluşturulduğu ve *DataAnalyst'in* `membername` iş yükü sınıflandırıcısını kullanarak `wcDataAnalyst` eşlendiği aşağıdaki iş yükü grubunu ve sınıflandırıcı yapılandırmasını düşünün.  İş `wgDataAnalyst` yükü grubunda bunun için yapılandırılan`MIN_PERCENTAGE_RESOURCE` %6 iş yükü yalıtımı (= 6) ve %9 kaynak sınırı (=`CAP_PERCENTAGE_RESOURCE` 9) vardır.  *DataAnalyst* tarafından gönderilen her sorgu, sistem kaynaklarının %3'ü (=`REQUEST_MIN_RESOURCE_GRANT_PERCENT` 3) verilir.

```sql
CREATE WORKLOAD GROUP wgDataAnalyst  
WITH ( MIN_PERCENTAGE_RESOURCE = 6
      ,CAP_PERCENTAGE_RESOURCE = 9
      ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 3);

CREATE WORKLOAD CLASSIFIER wcDataAnalyst
WITH ( WORKLOAD_GROUP = 'wgDataAnalyst'
      ,MEMBERNAME = 'DataAnalyst');
```

Aşağıdaki grafik aşağıdaki gibi yapılandırılmıştır:<br>
Metrik 1: *Etkin kap kaynak yüzdesi* (Avg toplama, `blue line`)<br>
Metrik 2: *Maksimum kaynak yüzdesine göre iş yükü grubu ayırma* (Avg toplama, `purple line`)<br>
Metrik 3: *İş yükü grubu sıralı sorgular* (Toplam toplama, `turquoise line`)<br>
Filtre: [İş Yükü Grubu] =`wgDataAnalyst`<br>
![şişe yakalı-wg](./media/sql-data-warehouse-workload-management-portal-monitor/bottle-necked-wg.png) Grafik, kaynaklarda %9'luk bir kap ile iş yükü grubunun %90+ olduğunu gösterir *(maksimum kaynak yüzdesi ölçümüne göre İş Yükü grubu tahsisinden).*  *İş Yükü grubu sıraya giren sorgular ölçümünden*gösterildiği gibi sürekli bir sorgu sırası vardır.  Bu durumda, değeri `CAP_PERCENTAGE_RESOURCE` %9'un üzerinde bir değere çıkarmak, aynı anda daha fazla sorgunun yürütülmesine olanak sağlar.  Diğer `CAP_PERCENTAGE_RESOURCE` iş yükü grupları tarafından yalıtılmış değil ve yeterli kaynak olduğunu varsayar artırma.  *Etkili kap kaynak yüzdesi ölçümünü*kontrol ederek kapağın arttığını doğrulayın.  Daha fazla iş elde etmek isteniyorsa, 3'ten büyük bir `REQUEST_MIN_RESOURCE_GRANT_PERCENT` değere de yükseltin.  Artan `REQUEST_MIN_RESOURCE_GRANT_PERCENT` sorguları daha hızlı çalışmasına izin verebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Quickstart: T-SQL kullanarak iş yükü yalıtımı yapılandırın](quickstart-configure-workload-isolation-tsql.md)<br>
- [İş YÜKÜ GRUBU OLUŞTUR (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest)<br>
- [İş YÜKÜ SINIFI (Transact-SQL) OLUŞTUR](https://docs.microsoft.com/sql/t-sql/statements/create-workload-classifier-transact-sql?view=azure-sqldw-latest)<br>
- [Kaynak kullanımını izleme](sql-data-warehouse-concept-resource-utilization-query-activity.md)
