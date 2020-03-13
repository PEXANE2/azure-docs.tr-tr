---
title: Otomatik ayarlamaya genel bakış
description: Azure SQL veritabanı, SQL sorgusunu analiz eder ve Kullanıcı iş yüküne otomatik olarak uyum sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 03/10/2020
ms.openlocfilehash: d4762c86268353ff0464ff3919250cd86f0038d4
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79214113"
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Azure SQL veritabanı 'nda otomatik ayarlama

Azure SQL veritabanı otomatik ayarlama, AI ve makine öğrenimine dayalı sürekli performans ayarları aracılığıyla en yüksek performans ve kararlı iş yükleri sağlar.

Otomatik ayarlama, bir veritabanında yürütülen sorguları sürekli olarak izlemek için yerleşik zeka kullanan tam olarak yönetilen bir akıllı performans hizmetidir ve performanslarını otomatik olarak geliştirir. Bu, veritabanı değişen iş yüklerine dinamik olarak uyarlanarak ve ayarlama önerilerini uygulayarak elde edilir. Otomatik ayarlama, Azure 'daki tüm veritabanlarından AI aracılığıyla yatay olarak öğrenir ve ayarlama eylemlerini dinamik olarak geliştirir. Daha uzun bir veritabanı otomatik ayarlama ile birlikte çalıştırıldığında, daha iyidir.

Azure SQL veritabanı otomatik ayarlama, kararlı ve en yüksek performanslı veritabanı iş yükleri sağlamak üzere etkinleştirebileceğiniz en önemli özelliklerden biri olabilir.

## <a name="what-can-automatic-tuning-do-for-you"></a>Otomatik ayarlama sizin için ne yapabilir?

- Azure SQL veritabanlarının otomatik performans ayarlaması
- Performans kazançlarını otomatik doğrulama
- Otomatik geri alma ve kendi kendini düzeltme
- Ayarlama geçmişi
- El ile dağıtımlar için işlem T-SQL betikleri ayarlama
- Proaktif iş yükü performansı izleme
- Yüzlerce binlerce veritabanında ölçeği genişletme özelliği
- DevOps kaynaklarına yönelik olumlu etki ve toplam sahip olma maliyeti

## <a name="safe-reliable-and-proven"></a>Güvenli, güvenilir ve kanıtlanmış

Azure SQL veritabanı 'nda veritabanlarına uygulanan ayarlama işlemleri, en yoğun iş yüklerinizin performansı için tamamen güvenlidir. Sistem, Kullanıcı iş yükleriyle karışmamak üzere tasarlanmıştır. Otomatik ayarlama önerileri yalnızca düşük kullanım saatlerinde uygulanır. Ayrıca sistem, iş yükü performansını korumak için otomatik ayarlama işlemlerini geçici olarak devre dışı bırakabilir. Böyle bir durumda, "sistem tarafından devre dışı bırakıldı" iletisi Azure portal görüntülenecektir. Otomatik ayarlama, en yüksek kaynak önceliğine sahip iş yüklerini sayar.

Otomatik ayarlama mekanizmaları aynı ve Azure üzerinde çalışan birkaç milyon veritabanında ölçünüzü. Otomatik ayarlama işlemleri, iş yükü performansının olumlu bir geliştirmesi olduğundan emin olmak için otomatik olarak doğrulanır. Gerileyen performans önerileri dinamik olarak algılanır ve geri döndürülür. Kaydedilen ayarlama geçmişi ile, her bir Azure SQL veritabanı için yapılan ayarlama geliştirmelerinden oluşan açık bir izleme bulunur.

![Otomatik ayarlama nasıl çalışır?](./media/sql-database-automatic-tuning/how-does-automatic-tuning-work.png)

Azure SQL veritabanı otomatik ayarlama, temel mantığını SQL Server otomatik ayarlama altyapısıyla paylaşıyor. Yerleşik zeka mekanizması hakkında daha fazla teknik bilgi için [SQL Server otomatik ayarlama](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)bölümüne bakın.

Otomatik ayarlamanın nasıl çalıştığına ve tipik kullanım senaryolarıyla ilgili genel bir bakış için bkz. gömülü video:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Improve-Azure-SQL-Database-Performance-with-automatic-tuning/player]

## <a name="enable-automatic-tuning"></a>Otomatik ayarlamayı etkinleştirme

Azure portal veya [alter database](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-current) T-SQL deyimini kullanarak [tek ve havuza alınmış veritabanları için otomatik ayarlamayı etkinleştirebilirsiniz](sql-database-automatic-tuning-enable.md) . [Alter database](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-mi-current) T-SQL deyimini kullanarak yönetilen örnek dağıtımında örnek veritabanları için otomatik ayarlamayı etkinleştirirsiniz.

## <a name="automatic-tuning-options"></a>Otomatik ayarlama seçenekleri

Azure SQL veritabanında bulunan otomatik ayarlama seçenekleri şunlardır:

| Otomatik ayarlama seçeneği | Tek veritabanı ve havuza alınmış veritabanı desteği | Örnek veritabanı desteği |
| :----------------------------- | ----- | ----- |
| **Dizin oluşturma** -iş yükünüzün performansını iyileştirebilecek dizinleri tanımlar, dizinler oluşturur ve sorguların performansının iyileştirildiğini otomatik olarak doğrular. | Yes | Hayır |
| **Drop Index** -benzersiz dizinler ve uzun süredir kullanılmayan dizinler dışında, yedekli ve yinelenen dizinleri her gün tanımlar (> 90 gün). Bu seçeneğin bölüm değiştirme ve Dizin ipuçlarını kullanan uygulamalarla uyumlu olmadığına lütfen unutmayın. Kullanılmayan dizinleri bırakma, Premium ve İş Açısından Kritik hizmet katmanlarında desteklenmez. | Yes | Hayır |
| **Son ıyı planı zorla** (otomatik plan düzeltmesi)-SQL sorgularını, önceki iyi plandan daha yavaş bir yürütme planı kullanarak tanımlar ve gerileme planı yerine bilinen son iyi planı kullanarak sorgular. | Yes | Yes |

### <a name="automatic-tuning-for-single-and-pooled-databases"></a>Tek ve havuza alınmış veritabanları için otomatik ayarlama

Tek ve havuza alınmış veritabanlarının otomatik olarak ayarlanması, veritabanı performansınızı iyileştirmek için **Dizin oluşturma**, **Drop Index**ve **en son iyi plan planı** veritabanı Danışmanı önerilerini kullanır. Daha fazla bilgi için Azure portal, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaserecommendedaction)'de ve [REST API](https://docs.microsoft.com/rest/api/sql/serverautomatictuning) [veritabanı Danışmanı önerilerine](sql-database-advisor-portal.md)bakın.

Azure portal kullanarak ayarlama önerilerini el ile uygulayabilir veya otomatik ayarlama olarak çalışabilen sizin için ayarlama önerilerini uygulamanıza izin verebilirsiniz. System olarak çalışabilen 'ın ayarlama önerilerini uygulamasına izin vermenin avantajları, iş yükü performansına yönelik olumlu bir kazanç olduğunu otomatik olarak doğruladığından, önemli bir performans geliştirmesi algılanmadığında, bu işlem ayarlama önerisini otomatik olarak döndürür. Bu, sık yürütülemeyen bir ayar önerisi tarafından etkilenen sorgular söz konusu olduğunda, doğrulama aşamasının tasarım ile 72 saat arasında sürebileceğini unutmayın.

T-SQL aracılığıyla ayarlama önerileri uyguluyorsanız, otomatik performans doğrulaması ve ters çevirme mekanizmaları kullanılamaz. Bu şekilde uygulanan öneriler etkin kalır ve 24-48 saat için ayarlama önerileri listesinde gösterilir. sistemi otomatik olarak çizmadan önce. Bir öneriyi daha erken kaldırmak istiyorsanız, Azure portal ' den atabilirsiniz.

Otomatik ayarlama seçenekleri veritabanı başına bağımsız olarak etkinleştirilebilir veya devre dışı bırakılabilir ya da SQL veritabanı sunucularında yapılandırılabilirler ve ayarları sunucudan devralan her veritabanına uygulanabilir. SQL veritabanı sunucuları, otomatik ayarlama ayarları için Azure varsayılanlarını devralınabilir. Azure Varsayılanları Şu anda FORCE_LAST_GOOD_PLAN etkinleştirilir, CREATE_INDEX etkinleştirilir ve DROP_INDEX devre dışı bırakılır.

> [!IMPORTANT]
> Mart itibariyle, otomatik ayarlama için Azure varsayılanlarına yapılan 2020 değişiklikler şu şekilde etkili olacaktır:
>
> - Yeni Azure Varsayılanları FORCE_LAST_GOOD_PLAN = etkin, CREATE_INDEX = devre dışı ve DROP_INDEX = devre dışı olacaktır.
> - Yapılandırılmış otomatik ayarlama tercihleri olmayan mevcut sunucular, yeni Azure varsayılanlarını DEVRALACAK şekilde otomatik olarak yapılandırılır. Bu, şu anda tanımsız bir durumda otomatik ayarlama için sunucu ayarlarına sahip olan tüm müşteriler için geçerlidir.
> - Oluşturulan yeni sunucular yeni Azure varsayılanlarını DEVRALACAK şekilde otomatik olarak yapılandırılacak (otomatik ayarlama yapılandırması yeni sunucu oluşturma sırasında tanımsız bir durumda olduğunda, daha önce farklı olur).

Bir sunucuda otomatik ayarlama seçeneklerinin yapılandırılması ve üst sunucuya ait olan veritabanlarının ayarlarını devralma, çok sayıda veritabanı için otomatik ayarlama seçeneklerinin yönetimini basitleştikten sonra otomatik ayarlamayı yapılandırmak için önerilen bir yöntemdir.

Otomatik ayarlama önerileri için e-posta bildirimleri oluşturma hakkında bilgi edinmek için bkz. [otomatik ayarlama Için e-posta bildirimleri](sql-database-automatic-tuning-email-notifications.md)

### <a name="automatic-tuning-for-instance-databases"></a>Örnek veritabanları için otomatik ayarlama

Yönetilen bir örnek dağıtımında örnek veritabanlarının otomatik olarak ayarlanması yalnızca **son Iyi zorla planını**destekler. T-SQL aracılığıyla otomatik ayarlama seçeneklerini yapılandırma hakkında daha fazla bilgi için bkz. otomatik [ayarlama otomatik plan düzeltmesini](https://azure.microsoft.com/blog/automatic-tuning-introduces-automatic-plan-correction-and-t-sql-management/) ve [Otomatik plan düzeltmesini](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning?view=sql-server-ver15#automatic-plan-correction)tanıtır.

## <a name="next-steps"></a>Sonraki adımlar

- Otomatik ayarlama sırasında kullanılan yerleşik zeka hakkında bilgi edinmek için bkz. [yapay zeka uyarlayan Azure SQL veritabanları](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/).
