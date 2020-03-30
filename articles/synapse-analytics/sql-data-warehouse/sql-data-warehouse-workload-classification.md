---
title: İş yükü sınıflandırma
description: Azure Synapse Analytics'teki sorgular için eşzamanlılık, önem ve bilgi işlem kaynaklarını yönetmek için sınıflandırmayı kullanma kılavuzu.
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
ms.openlocfilehash: 7661981f07799592f9fdfcab3fb402336d48b4d4
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80349970"
---
# <a name="azure-synapse-analytics-workload-classification"></a>Azure Synapse Analytics iş yükü sınıflandırması

Bu makalede, bir iş yükü grubu atama iş yükü sınıflandırma işlemi ve Azure Synapse SQL Analytics ile gelen isteklerin önemi açıklanmaktadır.

## <a name="classification"></a>Sınıflandırma

> [!Video https://www.youtube.com/embed/QcCRBAhoXpM]

İş yükü yönetimi sınıflandırması, [kaynak sınıfları](resource-classes-for-workload-management.md#what-are-resource-classes) ve [önemi](sql-data-warehouse-workload-importance.md)atayarak isteklere iş yükü ilkelerinin uygulanmasına olanak tanır.

Veri depolama iş yüklerini sınıflandırmak için birçok yol olsa da, en basit ve en yaygın sınıflandırma yük ve sorgudur. Ekleme, güncelleştirme ve silme ifadeleri ile veri yüklersiniz.  Selects'i kullanarak verileri sorgularsınız. Veri depolama çözümü genellikle daha fazla kaynakla daha yüksek bir kaynak sınıfı atamak gibi yük etkinliği için bir iş yükü ilkesine sahip olur. Yük etkinliklerine kıyasla daha düşük önem emayeti gibi sorgular için farklı bir iş yükü ilkesi uygulanabilir.

Ayrıca yük ve sorgu iş yüklerinizi alt sınıflayabilirsiniz. Alt sınıflandırma, iş yükleri üzerinde daha fazla denetim sağlar. Örneğin, sorgu iş yükleri küp yenilemeleri, pano sorguları veya geçici sorgulardan oluşabilir. Bu sorgu iş yüklerinin her birini farklı kaynak sınıfları veya önem ayarlarıyla sınıflandırabilirsiniz. Yük alt sınıflandırmadan da yararlanabilir. Büyük dönüşümler daha büyük kaynak sınıflarına atanabilir. Hava durumu verilerinden veya sosyal veri akışından önce önemli satış verilerinin yükleyici olmasını sağlamak için daha yüksek önem kullanılabilir.

Tüm ifadeler kaynak gerektirmedikleri veya yürütmeyi etkilemek için önem gerektirdikleri için sınıflandırılmamışdeğildir.  DBCC komutları, BEGIN, COMMIT ve ROLLBACK Işlem deyimleri sınıflandırılmamıştır.

## <a name="classification-process"></a>Sınıflandırma süreci

Azure Sinaps'ta SQL Analytics için sınıflandırma bugün, kullanıcıları [sp_addrolemember](/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql)kullanarak kendisine atanan ilgili kaynak sınıfına atanan bir role atayarak elde edilir. Kaynak sınıfına giriş in ötesinde istekleri karakterize etme yeteneği bu yetenekle sınırlıdır. [Create WORKLOAD CLASSIFIER](/sql/t-sql/statements/create-workload-classifier-transact-sql) sözdizimi ile sınıflandırma için daha zengin bir yöntem artık kullanılabilir.  Bu sözdizimi ile SQL Analytics kullanıcıları önem atayabilir ve `workload_group` parametre aracılığıyla bir isteğe ne kadar sistem kaynağı atanabilir. 

> [!NOTE]
> Sınıflandırma istek bazında değerlendirilir. Tek bir oturumdaki birden çok istek farklı olarak sınıflandırılabilir.

## <a name="classification-weighting"></a>Sınıflandırma ağırlıklandırma

Sınıflandırma işleminin bir parçası olarak, hangi iş yükü grubunun atandığını belirlemek için ağırlıklandırma yapılır.  Ağırlık aşağıdaki gibidir:

|Sınıflandırıcı Parametresi |Ağırlık   |
|---------------------|---------|
|ÜYE ADI:KULLANICI      |64       |
|ÜYE ADI:ROL      |32       |
|WLM_LABEL            |16       |
|WLM_CONTEXT          |8        |
|START_TIME/END_TIME  |4        |

Parametre `membername` zorunludur.  Ancak, belirtilen üye adı veritabanı rolü yerine bir veritabanı kullanıcısıysa, kullanıcı için ağırlıklandırma daha yüksektir ve böylece sınıflandırıcı seçilir.

Bir kullanıcı, birden çok sınıflayıcıda atanmış veya eşleşen farklı kaynak sınıfları olan birden çok rolün üyesiyse, kullanıcıya en yüksek kaynak sınıfı ataması verilir.  Bu davranış, varolan kaynak sınıf atama davranışı ile tutarlıdır.

## <a name="system-classifiers"></a>Sistem sınıflandırıcıları

İş yükü sınıflandırması sistem iş yükü sınıflayıcıları vardır. Sistem sınıflandırıcılar, varolan kaynak sınıfı rol üyeliklerini normal öneme sahip kaynak sınıf kaynak ayırmalarına eşler. Sistem sınıflandırıcıları bırakılamaz. Sistem sınıflandırıcılarını görüntülemek için aşağıdaki sorguyu çalıştırabilirsiniz:

```sql
SELECT * FROM sys.workload_management_workload_classifiers where classifier_id <= 12
```

## <a name="mixing-resource-class-assignments-with-classifiers"></a>Kaynak sınıfı ödevlerini sınıflayıcılarla karıştırma

Sizin adınıza oluşturulan sistem sınıflandırıcıları, iş yükü sınıflandırmasına geçiş için kolay bir yol sağlar. Sınıflandırma önceliği ile kaynak sınıfı rol eşlemeleri kullanarak, önemli yeni sınıflayıcılar oluşturmaya başladığınızda yanlış sınıflandırmaya yol açabilir.

Şu senaryoyu göz önünde bulundurun:

- Varolan bir veri ambarı, daha büyük kaynak sınıfı rolüne atanmış bir veritabanı kullanıcısı DBAUser'e sahiptir. Kaynak sınıfı ataması sp_addrolemember ile yapıldı.
- Veri ambarı artık iş yükü yönetimi yle güncelleştirildi.
- Yeni sınıflandırma sözdizimini test etmek için, DBAUser'in üyesi olduğu veritabanı rolü DBARole'de, onları mediumrc ve yüksek öneme göre eşlemeleri için oluşturulmuş bir sınıflandırıcı vardır.
- DBAUser oturum açıp bir sorgu çalıştırdığında, sorgu daha büyük türe atanır. Çünkü bir kullanıcı rol üyeliğinden önce gelir.

Sorun giderme yanlış sınıflandırmasını basitleştirmek için, iş yükü sınıflayıcıları oluştururken kaynak sınıfı rol eşlemelerini kaldırmanızı tavsiye ettik.  Aşağıdaki kod varolan kaynak sınıfı rol üyeliklerini döndürür.  İlgili kaynak sınıfından döndürülen her üye adı için [sp_droprolemember](/sql/relational-databases/system-stored-procedures/sp-droprolemember-transact-sql) çalıştırın.

```sql
SELECT  r.name AS [Resource Class]
,       m.name AS membername
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r ON rm.role_principal_id = r.principal_id
JOIN    sys.database_principals AS m ON rm.member_principal_id = m.principal_id
WHERE   r.name IN ('mediumrc','largerc','xlargerc','staticrc10','staticrc20','staticrc30','staticrc40','staticrc50','staticrc60','staticrc70','staticrc80');

--for each row returned run
sp_droprolemember ‘[Resource Class]’, membername
```

## <a name="next-steps"></a>Sonraki adımlar

- Sınıflandırıcı oluşturma hakkında daha fazla bilgi için [CREATE WORKLOAD CLASSIFIER (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-workload-classifier-transact-sql)'ye bakın.  
- İş yükü sınıflandırıcısı oluşturma konusunda Hızlı Başlangıç'a bakın [İş yükü sınıflandırıcısı oluşturun.](quickstart-create-a-workload-classifier-tsql.md)
- [İş Yükü Önemini Yapılandırmak](sql-data-warehouse-how-to-configure-workload-importance.md) ve İş Yükü Yönetimini nasıl [yönetip izleyeceğini](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md)görmek için nasıl yapılan makalelere bakın.
- Sorguları ve atanan önemi görüntülemek için [sys.dm_pdw_exec_requests'a](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) bakın.