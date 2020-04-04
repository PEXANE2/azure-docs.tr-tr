---
title: İş yükü yalıtımı
description: Azure Synapse Analytics'te iş yükü gruplarıyla iş yükü yalıtımı ayarlama kılavuzu.
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
ms.openlocfilehash: c3fcbf69e7dae14ccd2114a14c685b0443f70fef
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632431"
---
# <a name="azure-synapse-analytics-workload-group-isolation-preview"></a>Azure Synapse Analytics iş yükü grubu yalıtımı (Önizleme)

Bu makalede, iş yükü gruplarının iş yükü yalıtımını yapılandırmak, kaynakları içeren ve sorgu yürütmeiçin çalışma zamanı kurallarını uygulamak için nasıl kullanılabileceğini açıklamaktadır.

## <a name="workload-groups"></a>İş yükü grupları

İş yükü grupları bir dizi istek için kapsayıcılardır ve iş yükü yalıtımı da dahil olmak üzere iş yükü yönetiminin bir sistemde nasıl yapılandırıldığına temel teşkil eder.  İş YÜKÜ GRUBU [İş](/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest) YÜKÜ Grubu sözdizimi kullanılarak iş yükü grupları oluşturulur.  Basit bir iş yükü yönetimi yapılandırması veri yüklerini ve kullanıcı sorgularını yönetebilir.  Örneğin, adlı `wgDataLoads` bir iş yükü grubu, sisteme yüklenen veriler için iş yükü yönlerini tanımlar. Ayrıca, adlı `wgUserQueries` bir iş yükü grubu, sistemden verileri okumak için sorguları çalıştıran kullanıcılar için iş yükü yönlerini tanımlar.

Aşağıdaki bölümlerde, iş yükü gruplarının yalıtımı tanımlama, kapsama, kaynak tanımı isteme ve yürütme kurallarına uyma olanağı nı nasıl sağladığı vurgulanır.

## <a name="workload-isolation"></a>İş yükü yalıtımı

İş yükü yalıtımı, kaynakların yalnızca bir iş yükü grubu için ayrılmış olduğu anlamına gelir.  İş yükü yalıtımı, MIN_PERCENTAGE_RESOURCE parametresinin [CREATE İş YÜKÜ GRUBU](/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest) sözdiziminde sıfırdan büyük şekilde yapılandırılarak sağlanır.  Sıkı SLA'lara uyması gereken sürekli yürütme iş yükleri için yalıtım, kaynakların iş yükü grubu için her zaman kullanılabilir olmasını sağlar.

İş yükü yalıtımını yapılandırmak, garantili bir eşzamanlılık düzeyini tanımlar. Örneğin, %30'a `MIN_PERCENTAGE_RESOURCE` ve `REQUEST_MIN_RESOURCE_GRANT_PERCENT` %2'ye ayarlanmış bir iş yükü grubu 15 eşzamanlılık garanti edilir.  Eşzamanlılık düzeyi garanti edilir, çünkü %15-2 kaynak yuvası iş yükü grubunda her zaman `REQUEST_*MAX*_RESOURCE_GRANT_PERCENT` ayrılmıştır (nasıl yapılandırıldığına bakılmaksızın).  İstek `REQUEST_MAX_RESOURCE_GRANT_PERCENT` başına `REQUEST_MIN_RESOURCE_GRANT_PERCENT` ek `CAP_PERCENTAGE_RESOURCE` kaynaklar `MIN_PERCENTAGE_RESOURCE` dan büyükse ve daha büyükse.  Eğer `REQUEST_MAX_RESOURCE_GRANT_PERCENT` `REQUEST_MIN_RESOURCE_GRANT_PERCENT` ve eşit `CAP_PERCENTAGE_RESOURCE` ve `MIN_PERCENTAGE_RESOURCE`daha büyükise , ek eşzamanlılık mümkündür.  Garantili eşzamanlılığı belirlemek için aşağıdaki yöntemi göz önünde bulundurun:

[Garantili Eşzamanlılık] =`MIN_PERCENTAGE_RESOURCE`[`REQUEST_MIN_RESOURCE_GRANT_PERCENT`] / [ ]

> [!NOTE]
> min_percentage_resource için belirli hizmet düzeyi minimum uygun değerler vardır.  Daha fazla bilgi için etkili [değerler](/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest#effective-values) e bakın.

İş yükü yalıtımı yokluğunda, istekler paylaşılan kaynak [havuzunda](#shared-pool-resources) çalışır.  Paylaşılan havuzdaki kaynaklara erişim garanti edilmez ve [önem](sql-data-warehouse-workload-importance.md) bazında atanır.

İş yükü grubunda etkin istek olmasa bile, kaynaklar iş yükü grubuna ayrıldıkça iş yükü yalıtımı dikkatli bir şekilde yapılmalıdır. Aşırı yapılandırma yalıtımı, genel sistem kullanımının azalmasına neden olabilir.

Kullanıcılar % 100 iş yükü yalıtımı yapılandıran bir iş yükü yönetimi çözümünden kaçınmalıdır: Tüm iş yükü gruplarında yapılandırılan min_percentage_resource toplamı %100'e eşit olduğunda %100 yalıtım sağlanır.  Bu tür bir yapılandırma aşırı kısıtlayıcı ve katıdır ve yanlışlıkla yanlış sınıflandırılan kaynak istekleri ne kadar az yer bırakır. Yalıtım için yapılandırılan iş yükü gruplarından bir isteğin yürütülmesine izin veren bir hüküm vardır. Bu isteğe ayrılan kaynaklar DMV sistemlerinde sıfır olarak gösterilir ve sistem ayrılmış kaynaklardan küçük bir kaynak hibe düzeyi ödünç alınır.

> [!NOTE]
> En iyi kaynak kullanımını sağlamak için, SLA'ların karşılanmasını ve [iş yükü önemine](sql-data-warehouse-workload-importance.md)bağlı olarak erişilen paylaşılan kaynaklarla karıştırılmasını sağlamak için bazı yalıtımlardan yararlanan bir iş yükü yönetimi çözümü düşünün.

## <a name="workload-containment"></a>İş yükü kapsama

İş yükü kapsama, iş yükü grubunun tüketebileceği kaynak miktarını sınırlamak anlamına gelir.  İş yükü oluşturma grubu sözdiziminde CAP_PERCENTAGE_RESOURCE parametresi 100'den az olarak yapılandırılarak [iş](/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest) yükü kapsama sağlanır.  Kullanıcıların geçici sorgular aracılığıyla bir "eğer- çözümleme" çalıştırabilmeleri için sisteme okuma erişimine ihtiyaç duydukları senaryoyu düşünün.  Bu tür istekler, sistemde çalışan diğer iş yüklerini olumsuz etkileyebilir.  Yapılandırma, kaynak miktarının sınırlı olmasını sağlar.

İş yükü içeren yapılandırma örtülü olarak eşzamanlılık maksimum düzeyini tanımlar.  İş yükü grubu için %60'a ayarlanmış bir CAP_PERCENTAGE_RESOURCE ve %1'e ayarlanmış bir REQUEST_MIN_RESOURCE_GRANT_PERCENT ile 60 eşzamanlılık düzeyine kadar izin verilir.  Maksimum eşzamanlılığı belirlemek için aşağıda yer alan yöntemi göz önünde bulundurun:

[Max Eşzamanlılık]`CAP_PERCENTAGE_RESOURCE`= [`REQUEST_MIN_RESOURCE_GRANT_PERCENT`] / [ ]

> [!NOTE]
> İş yükü grubunun etkili CAP_PERCENTAGE_RESOURCE, sıfırdan büyük düzeyde MIN_PERCENTAGE_RESOURCE iş yükü grupları oluşturulduğunda %100'e ulaşamaz.  Etkili çalışma zamanı değerleri için [sys.dm_workload_management_workload_groups_stats'a](/sql/relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql?view=azure-sqldw-latest) bakın.

## <a name="resources-per-request-definition"></a>İstek tanımı başına kaynaklar

İş yükü grupları, [CREATE İş YÜKÜ GRUBU](/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest) sözdiziminde REQUEST_MIN_RESOURCE_GRANT_PERCENT ve REQUEST_MAX_RESOURCE_GRANT_PERCENT parametreleri ile istek başına ayrılan kaynakların min ve maksimum miktarını tanımlamak için bir mekanizma sağlar.  Bu durumda kaynaklar CPU ve bellekvardır.  Bu değerlerin yapılandırılması, sistemde ne kadar kaynak ve eşzamanlılık düzeyine ulaşılabileceğini belirler.

> [!NOTE]
> REQUEST_MAX_RESOURCE_GRANT_PERCENT, REQUEST_MIN_RESOURCE_GRANT_PERCENT için belirtilen değere varsayılan isteğe bağlı bir parametredir.

Kaynak sınıfı seçmek gibi, yapılandırma REQUEST_MIN_RESOURCE_GRANT_PERCENT bir istek tarafından kullanılan kaynakların değerini ayarlar.  Ayarlanan değerle belirtilen kaynak miktarı, yürütmeye başlamadan önce isteğe tahsis için garanti edilir.  Kaynak sınıflarından iş yükü gruplarına geçiş yapılan müşteriler için, başlangıç noktası olarak kaynak sınıflarından iş yükü gruplarına eşlenecek [nasıl](sql-data-warehouse-how-to-convert-resource-classes-workload-groups.md) makaleyi izlemeyi düşünün.

REQUEST_MAX_RESOURCE_GRANT_PERCENT REQUEST_MIN_RESOURCE_GRANT_PERCENT'den büyük bir değere yapılandırmak, sistemin istek başına daha fazla kaynak ayırmasına olanak tanır.  Bir istek zamanlarken, sistem paylaşılan havuzdaki kaynak kullanılabilirliğine ve sistemdeki geçerli yüklemeye bağlı olarak, REQUEST_MIN_RESOURCE_GRANT_PERCENT ile REQUEST_MAX_RESOURCE_GRANT_PERCENT arasında olan isteğe gerçek kaynak tahsisini belirler.  Sorgu zamanlandığında kaynakların paylaşılan kaynak [havuzunda](#shared-pool-resources) bulunması gerekir.  

> [!NOTE]
> REQUEST_MIN_RESOURCE_GRANT_PERCENT ve REQUEST_MAX_RESOURCE_GRANT_PERCENT etkin MIN_PERCENTAGE_RESOURCE ve CAP_PERCENTAGE_RESOURCE değerlerine bağlı etkili değerlere sahiptir.  Etkili çalışma zamanı değerleri için [sys.dm_workload_management_workload_groups_stats'a](/sql/relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql?view=azure-sqldw-latest) bakın.

## <a name="execution-rules"></a>Yürütme Kuralları

Geçici raporlama sistemlerinde, müşteriler yanlışlıkla başkalarının üretkenliğini ciddi şekilde etkileyen kaçak sorguları yürütebilir.  Sistem yöneticileri, sistem kaynaklarını boşaltmak için kaçak sorguları öldürmek için zaman harcamak zorunda kalırlar.  İş yükü grupları, belirtilen değeri aşan sorguları iptal etmek için sorgu yürütme zaman aşımı kuralını yapılandırma olanağı sunar.  Kural, CREATE İş YÜKÜ `QUERY_EXECUTION_TIMEOUT_SEC` [GRUBU](/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest) sözdiziminde parametre ayarlayarak yapılandırılır.

## <a name="shared-pool-resources"></a>Paylaşılan havuz kaynakları

Paylaşılan havuz kaynakları, yalıtım için yapılandırılmayan kaynaklardır.  İstekleri yürütmek için paylaşılan havuzda MIN_PERCENTAGE_RESOURCE sıfır kaldıraç kaynağına ayarlanmış iş yükü grupları.  MIN_PERCENTAGE_RESOURCE'den büyük bir CAP_PERCENTAGE_RESOURCE olan iş yükü grupları paylaşılan kaynakları da kullandı.  Paylaşılan havuzda bulunan kaynak miktarı aşağıdaki gibi hesaplanır.

[Paylaşılan Havuz] = 100 `MIN_PERCENTAGE_RESOURCE` - [tüm iş yükü gruplarında toplamı]

Paylaşılan havuzdaki kaynaklara erişim [önem](sql-data-warehouse-workload-importance.md) esasına göre tahsis edilir.  Aynı önem düzeyine sahip istekler, paylaşılan havuz kaynaklarına ilk giriş/ilk çıkış temelinde erişecektir.

## <a name="next-steps"></a>Sonraki adımlar

- [Quickstart: iş yükü yalıtımı yapılandırma](quickstart-configure-workload-isolation-tsql.md)
- [İş YÜKÜ GRUBU OLUŞTUR](/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest)
- [Kaynak sınıflarını iş yükü gruplarına dönüştürün.](sql-data-warehouse-how-to-convert-resource-classes-workload-groups.md)
- [İş Yükü Yönetimi Portalı İzleme](sql-data-warehouse-workload-management-portal-monitor.md).  
