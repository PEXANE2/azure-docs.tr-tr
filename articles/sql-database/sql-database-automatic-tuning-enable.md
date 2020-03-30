---
title: Otomatik ayarlamayı etkinleştirme
description: Azure SQL Veritabanınızda otomatik ayarı kolayca etkinleştirebilirsiniz.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 12/03/2019
ms.openlocfilehash: eed839c277156046ff9b7d97c6e87636a0822889
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299337"
---
# <a name="enable-automatic-tuning-to-monitor-queries-and-improve-workload-performance"></a>Sorguları izlemek ve iş yükü performansını artırmak için otomatik ayar ı etkinleştirme

Azure SQL Veritabanı, sorgularınızı sürekli olarak izleyen ve iş yükünüzün performansını artırmak için gerçekleştirebileceğiniz eylemi tanımlayan otomatik olarak yönetilen bir veri hizmetidir. Önerileri gözden geçirip el ile uygulayabilir veya Azure SQL Veritabanı'nın otomatik olarak düzeltici eylemler uygulamasına izin verebilirsiniz - bu **otomatik ayar modu**olarak bilinir.

[Azure portalı,](sql-database-automatic-tuning-enable.md#azure-portal) [REST API](sql-database-automatic-tuning-enable.md#rest-api) aramaları ve [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-current) komutları aracılığıyla sunucuda veya veritabanı düzeyinde otomatik ayar etkinleştirilebilir.

> [!NOTE]
> Yönetilen Örnek için desteklenen seçenek FORCE_LAST_GOOD_PLAN yalnızca [T-SQL](https://azure.microsoft.com/blog/automatic-tuning-introduces-automatic-plan-correction-and-t-sql-management) üzerinden yapılandırılabilir. Bu makalede açıklanan portal tabanlı yapılandırma ve otomatik dizin ayar seçenekleri Yönetilen Örnek için geçerli değildir.

> [!NOTE]
> Arm (Azure Kaynak Yöneticisi) şablonu ile Otomatik ayar seçeneklerini yapılandırma kaynıyor, şu anda desteklenmiyor.

## <a name="enable-automatic-tuning-on-server"></a>Sunucuda otomatik ayar yapma olanağı sağlama

Sunucu düzeyinde otomatik ayar yapılandırmasını "Azure Varsayılanları"ndan devralmayı veya yapılandırmayı devralmamayı seçebilirsiniz. Azure varsayılanları etkinleştirildiFORCE_LAST_GOOD_PLAN, CREATE_INDEX etkin ve DROP_INDEX devre dışı bırakılır.

> [!IMPORTANT]
> Mart ayı itibariyle, otomatik ayar için Azure varsayılanlarında yapılan 2020 değişiklikleri aşağıdaki gibi yürürlüğe girecektir:
>
> - Yeni Azure varsayılanları FORCE_LAST_GOOD_PLAN = etkin, CREATE_INDEX = devre dışı bırakılır ve DROP_INDEX = devre dışı bırakılır.
> - Otomatik ayar tercihi yapılamayan mevcut sunucular, yeni Azure varsayılanlarını devralmak üzere otomatik olarak yapılandırılır. Bu, şu anda tanımlanmamış bir durumda otomatik ayar için sunucu ayarlarına sahip tüm müşteriler için geçerlidir.
> - Oluşturulan yeni sunucular otomatik olarak yeni Azure varsayılanlarını devralacak şekilde yapılandırılır (yeni sunucu oluşturma da tanımlanmamış bir durumdayken daha önceki durumlarda nisbeten farklı olarak).

### <a name="azure-portal"></a>Azure portalında

Azure SQL Veritabanı mantıksal **sunucusunda**otomatik ayar sağlamak için Azure portalındaki sunucuya gidin ve ardından menüde **Otomatik ayar'ı** seçin.

![Sunucu](./media/sql-database-automatic-tuning-enable/server.png)

> [!NOTE]
> Şu anda **DROP_INDEX** seçeneğinin bölüm anahtarlama ve dizin ipuçları nı kullanan uygulamalarla uyumlu olmadığını ve bu gibi durumlarda etkinleştirilmemesi gerektiğini lütfen unutmayın. Kullanılmayan dizinlerin düşmesi Premium ve İş Açısından Kritik hizmet katmanları için desteklenmez.
>

Etkinleştirmek istediğiniz otomatik ayar seçeneklerini seçin ve **Uygula'yı**seçin.

Sunucudaki otomatik ayar seçenekleri bu sunucudaki tüm veritabanlarına uygulanır. Varsayılan olarak, tüm veritabanları yapılandırmayı üst sunucularından devralır, ancak bu geçersiz kılınabilir ve her veritabanı için ayrı ayrı belirtilebilir.

### <a name="rest-api"></a>REST API

Sunucuda Otomatik ayar yapmak için REST API'yi kullanma hakkında daha fazla bilgi edinin, [SQL Server Automatic tuning UPDATE ve GET HTTP yöntemlerine](https://docs.microsoft.com/rest/api/sql/serverautomatictuning)bakın.

## <a name="enable-automatic-tuning-on-an-individual-database"></a>Tek bir veritabanında otomatik ayar yapma olanağı sağlama

Azure SQL Veritabanı, her veritabanı için otomatik ayar yapılandırmasını tek tek belirtmenize olanak tanır. Veritabanı düzeyinde, otomatik ayar yapılandırmasını üst sunucudan devralmayı seçebilirsiniz, "Azure Varsayılanları" veya yapılandırmayı devralmamayı. Azure Varsayılanları etkin FORCE_LAST_GOOD_PLAN, CREATE_INDEX etkin ve DROP_INDEX devre dışı bırakılabilmek üzere ayarlanır.

> [!TIP]
> Genel öneri, her veritabanında aynı yapılandırma ayarlarının otomatik olarak uygulanabilmesi için otomatik ayar yapılandırmasını **sunucu düzeyinde** yönetmektir. Yalnızca bu veritabanının aynı sunucudan ayarları devralan diğer lerinden farklı ayarlara sahip olması gerekiyorsa, tek bir veritabanında otomatik ayar yapmayı yapılandırın.
>

### <a name="azure-portal"></a>Azure portalında

Tek bir **veritabanında**otomatik ayar sağlamak için Azure portalındaki veritabanına gidin ve **Otomatik ayar'ı**seçin.

Bireysel otomatik ayar ayarları her veritabanı için ayrı ayrı yapılandırılabilir. Tek bir otomatik ayar seçeneğini el ile yapılandırabilir veya bir seçeneğin ayarlarını sunucudan devraldığını belirtebilirsiniz.

![Database](./media/sql-database-automatic-tuning-enable/database.png)

Şu anda DROP_INDEX seçeneğinin bölüm anahtarlama ve dizin ipuçları nı kullanan uygulamalarla uyumlu olmadığını ve bu gibi durumlarda etkinleştirilmemesi gerektiğini lütfen unutmayın.

İstediğiniz yapılandırmayı seçtikten sonra **Uygula'yı**tıklatın.

### <a name="rest-api"></a>Dinlenme API'si

Tek bir veritabanında Otomatik ayar sağlamak için REST API kullanma hakkında daha fazla bilgi edinin, [SQL Veritabanı Otomatik ayar GÜNCELLEME ve GET HTTP yöntemleri](https://docs.microsoft.com/rest/api/sql/databaseautomatictuning)bakın.

### <a name="t-sql"></a>T-SQL

T-SQL üzerinden tek bir veritabanında otomatik ayar yapabilmek için veritabanına bağlanın ve aşağıdaki sorguyu uygulayın:

```SQL
ALTER DATABASE current SET AUTOMATIC_TUNING = AUTO | INHERIT | CUSTOM
```

Otomatik ayarlamayı AUTO'ya ayarlamak Azure Varsayılanları'nı uygular. INHERIT olarak ayarlanırsa, otomatik ayar yapılandırması üst sunucudan devralınır. CUSTOM'ı seçerken, otomatik tuning'i el ile yapılandırmanız gerekir.

T-SQL üzerinden tek tek otomatik ayar seçeneklerini yapılandırmak için veritabanına bağlanın ve bunun gibi sorguyu çalıştırın:

```SQL
ALTER DATABASE current SET AUTOMATIC_TUNING (FORCE_LAST_GOOD_PLAN = ON, CREATE_INDEX = DEFAULT, DROP_INDEX = OFF)
```

Tek tek ayar seçeneğini AÇIK olarak ayarlamak, veritabanının devraldığı tüm ayarı geçersiz kılar ve ayarlama seçeneğini etkinleştirilir. OFF'a ayarlanması, veritabanının devraldığı tüm ayarı geçersiz kılar ve ayar seçeneğini devre dışı bırakacaktır. Varsayılan olarak belirtilen otomatik ayar seçeneği, sunucu düzeyi ayarlarından otomatik ayar yapılandırmasını devralır.  

> [!IMPORTANT]
> [Etkin coğrafi çoğaltma](sql-database-auto-failover-group.md)durumunda, otomatik ayar yalnızca birincil veritabanında yapılandırılmalıdır. Otomatik olarak uygulanan ayar eylemleri, örneğin dizin oluşturma veya silme otomatik olarak salt okunur ikincil çoğaltılır. Salt okunur ikincil de T-SQL üzerinden Otomatik ayar etkinleştirmeye çalışmak, salt okunur ikincil farklı bir ayar yapılandırması desteklenmez olarak bir başarısızlık neden olur.
>

Otomatik ayarlamayı yapılandırmak için daha fazla abut T-SQL seçeneklerimizi bulun, [SQL Veritabanı sunucusu için ALTER DATABASE SET Seçenekleri 'ne (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-current)bakın.

## <a name="disabled-by-the-system"></a>Sistem tarafından devre dışı

Otomatik ayar veritabanında gerçekleştirdiği tüm eylemleri izliyor ve bazı durumlarda otomatik ayarı veritabanıüzerinde düzgün çalışamaz belirleyebilirsiniz. Bu durumda, atoalma seçeneği sistem tarafından devre dışı bırakılır. Çoğu durumda, Sorgu Deposu etkin olmadığı veya belirli bir veritabanında salt okunur durumda olması nedeniyle bu durum gerçekleşir.

## <a name="permissions"></a>İzinler

Otomatik ayar Azure özelliği olduğundan, bunu kullanmak için Azure'un yerleşik RBAC rollerini kullanmanız gerekir. Yalnızca SQL Kimlik Doğrulaması kullanmak Azure portalındaki özelliği kullanmak için yeterli olmayacaktır.

Otomatik ayar kullanmak için, kullanıcıya vermek için gereken minimum izin Azure'un yerleşik [SQL DB katılımcı](../role-based-access-control/built-in-roles.md#sql-db-contributor) sıfatıdır. AYRıCA, SQL Server Katkıda Bulunan, Katkıda Bulunan ve Sahip olan daha yüksek ayrıcalık rolleri kullanmayı da düşünebilirsiniz.

## <a name="configure-automatic-tuning-e-mail-notifications"></a>Otomatik ayar e-posta bildirimlerini yapılandırma

[Otomatik ayar e-posta bildirimleri kılavuzuna](sql-database-automatic-tuning-email-notifications.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Otomatik ayar ve performansınızı artırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinmek için [Otomatik ayar makalesini](sql-database-automatic-tuning.md) okuyun.
* Azure SQL Veritabanı performans önerilerine genel bakış için [Performans önerilerine](sql-database-advisor.md) bakın.
* En iyi sorgularınızın performans etkisini görüntüleme hakkında bilgi edinmek için [Sorgu Performansı Öngörüleri'ne](sql-database-query-performance.md) bakın.
