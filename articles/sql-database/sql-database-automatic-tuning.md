---
title: Otomatik ayarlamaya genel bakış
description: Azure SQL Veritabanı, SQL sorgusunu analiz eder ve kullanıcı iş yüküne otomatik olarak uyarlar.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 03/30/2020
ms.openlocfilehash: 7488fd5f8a572788933856f03bb0ad4351885704
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80518219"
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Azure SQL Veritabanında otomatik ayar

Azure SQL Veritabanı otomatik ayar, Yapay AI ve makine öğrenimine dayalı sürekli performans ayarı yoluyla en yüksek performans ve kararlı iş yüklerini sağlar.

Otomatik ayar, veritabanında yürütülen sorguları sürekli olarak izlemek için yerleşik zekakullanan ve performanslarını otomatik olarak artıran tam olarak yönetilen akıllı bir performans hizmetidir. Bu, veritabanını değişen iş yüklerine dinamik olarak uyarlayarak ve atokslama önerileri uygulayarak elde edilir. Otomatik ayar, AI aracılığıyla Azure'daki tüm veritabanlarından yatay olarak öğrenir ve ayar eylemlerini dinamik olarak geliştirir. Bir veritabanı otomatik ayarla ne kadar uzun süre çalışırsa, o kadar iyi performans gösterir.

Azure SQL Veritabanı otomatik ayar, kararlı ve en yüksek performanslı veritabanı iş yüklerini sağlamak için etkinleştirebileceğiniz en önemli özelliklerden biri olabilir.

## <a name="what-can-automatic-tuning-do-for-you"></a>Otomatik ayar sizin için ne yapabilir

- Azure SQL veritabanlarının otomatik performans alamı
- Performans kazançlarının otomatik olarak doğrulanması
- Otomatik geri alma ve kendi kendini düzeltme
- Tuning geçmişi
- Manuel dağıtımlar için tuning eylem T-SQL komut dosyaları
- Proaktif iş yükü performans izleme
- Yüz binlerce veritabanındaki yeteneği ölçeklendirin
- DevOps kaynaklarına olumlu etki ve toplam sahip olma maliyeti

## <a name="safe-reliable-and-proven"></a>Güvenli, Güvenilir ve Kanıtlanmış

Azure SQL Veritabanı'ndaki veritabanlarına uygulanan tuning işlemleri, en yoğun iş yüklerinizin performansı için tamamen güvenlidir. Sistem, kullanıcı iş yüklerine müdahale etmemek için özenle tasarlanmıştır. Otomatik tuning önerileri yalnızca düşük kullanım zamanlarında uygulanır. Sistem ayrıca iş yükü performansını korumak için otomatik ayar işlemlerini geçici olarak devre dışı kılabilir. Bu durumda Azure portalında "Sistem tarafından devre dışı" iletisi gösterilir. Otomatik ayar, en yüksek kaynak önceliğine sahip iş yükleriyle ilgili.

Otomatik ayar mekanizmaları olgunve Azure'da çalışan milyonlarca veritabanlarında mükemmelleştirilmiştir. Uygulanan otomatik ayar işlemleri, iş yükü performansında olumlu bir iyileşme olduğundan emin olmak için otomatik olarak doğrulanır. Gerilenen performans önerileri dinamik olarak algılanır ve hemen geri döndürülmez. Kaydedilen tuning geçmişi sayesinde, her Azure SQL Veritabanı'nda yapılan aparat geliştirmelerinin net bir izi vardır.

![Otomatik ayar nasıl çalışır?](./media/sql-database-automatic-tuning/how-does-automatic-tuning-work.png)

Azure SQL Veritabanı otomatik ayar, temel mantığını SQL Server otomatik ayar motoruyla paylaşıyor. Yerleşik zeka mekanizması hakkında ek teknik bilgi için [SQL Server otomatik ayar'a](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)bakın.

Otomatik ayarlamanın nasıl çalıştığına ve tipik kullanım senaryolarına genel bir bakış için katıştırılmış videoya bakın:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Improve-Azure-SQL-Database-Performance-with-automatic-tuning/player]

## <a name="enable-automatic-tuning"></a>Otomatik ayarlamayı etkinleştirme

Azure [portalında tek ve havuza toplanan veritabanları için otomatik ayar'ı](sql-database-automatic-tuning-enable.md) veya [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-current) T-SQL deyimini kullanarak etkinleştirebilirsiniz. [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-mi-current) T-SQL deyimini kullanarak yönetilen bir örnek dağıtımında örnek veritabanları için otomatik ayar ı etkinleştirin.

## <a name="automatic-tuning-options"></a>Otomatik ayar seçenekleri

Azure SQL Veritabanı'nda kullanılabilen otomatik ayar seçenekleri şunlardır:

| Otomatik ayar seçeneği | Tek veritabanı ve havuzlu veritabanı desteği | Örnek veritabanı desteği |
| :----------------------------- | ----- | ----- |
| **CREATE INDEX** - İş yükünüzün performansını artırabilecek dizinleri tanımlar, dizinler oluşturur ve sorguların performansının iyileştiğini otomatik olarak doğrular. | Evet | Hayır |
| **DROP INDEX** - Benzersiz dizinler ve uzun süre kullanılmayan dizinler (>90 gün) dışında, günlük gereksiz ve yinelenen dizinleri tanımlar. Bu seçeneğin bölüm değiştirme ve dizin ipuçlarını kullanan uygulamalarla uyumlu olmadığını lütfen unutmayın. Kullanılmayan dizinlerin düşmesi Premium ve İş Açısından Kritik hizmet katmanları için desteklenmez. | Evet | Hayır |
| **FORCE LAST GOOD PLAN** (otomatik plan düzeltme) - Önceki iyi plandan daha yavaş olan yürütme planını kullanarak SQL sorgularını ve gerileyen plan yerine bilinen son iyi planı kullanarak sorguları tanımlar. | Evet | Evet |

### <a name="automatic-tuning-for-single-and-pooled-databases"></a>Tek ve havuzlu veritabanları için otomatik ayar

Tek ve havuzlu veritabanları için otomatik **ayar,** veritabanı performansınızı optimize etmek için CREATE INDEX , **DROP INDEX**ve FORCE LAST **GOOD PLAN** veritabanı danışmanı önerilerini kullanır. Daha fazla bilgi için, Azure portalında , [PowerShell'de](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaserecommendedaction)ve [REST API'deki](https://docs.microsoft.com/rest/api/sql/serverautomatictuning) [Veritabanı danışmanı önerilerine](sql-database-advisor-portal.md)bakın.

Azure portalını kullanarak ayar önerilerini el ile uygulayabilir veya otomatik ayarın sizin için otomatik ayar önerilerini bağımsız olarak uygulamasına izin verebilirsiniz. Sistemin sizin için otomatik olarak ayar önerileri uygulamasına izin vermenin yararları, iş yükü performansında olumlu bir kazanç olduğunu otomatik olarak doğrular ve önemli bir performans iyileştirmesi algılanmazsa, ayar önerisini otomatik olarak geri alır. Sık sık yürütülmeyen ayar önerilerinden etkilenen sorgularda doğrulama aşamasının tasarım gereği 72 saate kadar sürebileceğini lütfen unutmayın.

T-SQL üzerinden ayar önerileri uyguluyorsanız, otomatik performans doğrulama ve ters mekanizmalar kullanılamaz. Bu şekilde uygulanan öneriler etkin kalır ve 24-48 saat için ayarı önerileri listesinde gösterilir. sistem otomatik olarak geri çekmeden önce. Bir öneriyi daha erken kaldırmak isterseniz, öneriyi Azure portalından atabilirsiniz.

Otomatik ayar seçenekleri bağımsız olarak etkinleştirilebilir veya veritabanı başına devre dışı bırakılabilir veya SQL Veritabanı sunucularında yapılandırılabilir ve ayarları sunucudan devralan her veritabanına uygulanabilir. SQL Veritabanı sunucuları, otomatik ayar ayarları için Azure varsayılanlarını devralabilir. Şu anda Azure varsayılanları FORCE_LAST_GOOD_PLAN etkin, CREATE_INDEX etkin ve DROP_INDEX devre dışı bırakılmış olarak ayarlanır.

> [!IMPORTANT]
> Mart ayı itibariyle, otomatik ayar için Azure varsayılanlarında yapılan 2020 değişiklikleri aşağıdaki gibi yürürlüğe girecektir:
>
> - Yeni Azure varsayılanları FORCE_LAST_GOOD_PLAN = etkin, CREATE_INDEX = devre dışı bırakılır ve DROP_INDEX = devre dışı bırakılır.
> - Otomatik ayar tercihi yapılamayan mevcut sunucular, yeni Azure varsayılanlarını devralmak üzere otomatik olarak yapılandırılır. Bu, şu anda tanımlanmamış bir durumda otomatik ayar için sunucu ayarlarına sahip tüm müşteriler için geçerlidir.
> - Oluşturulan yeni sunucular otomatik olarak yeni Azure varsayılanlarını devralacak şekilde yapılandırılır (yeni sunucu oluşturma da tanımlanmamış bir durumdayken daha önceki durumlarda nisbeten farklı olarak).

Sunucuda otomatik ayar seçeneklerini yapılandırmak ve ana sunucuya ait veritabanları için ayarları devralmak, çok sayıda veritabanları için otomatik ayar seçeneklerinin yönetimini basitleştirerek otomatik ayar yapmayı yapılandırmak için önerilen bir yöntemdir.

Otomatik ayar önerileri için e-posta bildirimleri oluşturma hakkında bilgi edinmek için otomatik [ayar için E-posta bildirimlerine](sql-database-automatic-tuning-email-notifications.md)bakın.

### <a name="automatic-tuning-for-instance-databases"></a>Örneğin veritabanları için otomatik ayar

Yönetilen bir örnek dağıtım örneğin veritabanları için otomatik ayar sadece **FORCE LAST GOOD PLAN**destekler. T-SQL üzerinden otomatik ayar seçeneklerini yapılandırma hakkında daha fazla bilgi için [bkz.](https://azure.microsoft.com/blog/automatic-tuning-introduces-automatic-plan-correction-and-t-sql-management/) [Automatic plan correction](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning?view=sql-server-ver15#automatic-plan-correction)

## <a name="next-steps"></a>Sonraki adımlar

- Otomatik ayarda kullanılan yerleşik zeka hakkında bilgi edinmek için, [Yapay Zeka Azure SQL veritabanlarını ayarlıyor.](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/)
- Otomatik ayar ın kaputun altında nasıl çalıştığını öğrenmek için [bkz.](https://www.microsoft.com/en-us/research/uploads/prod/2019/02/autoindexing_azuredb.pdf)
