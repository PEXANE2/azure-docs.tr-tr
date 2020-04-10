---
title: Azure Veri Fabrikası'nda şirket içi SSIS iş yüklerini SSIS'e geçirin
description: Şirket içi SSIS iş yüklerini ADF'de SSIS'e geçirin.
services: data-factory
documentationcenter: ''
author: chugugrace
ms.author: chugu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 9/3/2019
ms.openlocfilehash: 6ad5bb26959916f60973a8c0274e17eee03aa7a1
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80991475"
---
# <a name="migrate-on-premises-ssis-workloads-to-ssis-in-adf"></a>Şirket içi SSIS iş yüklerini ADF'de SSIS'e geçirin

## <a name="overview"></a>Genel Bakış

Veritabanı iş yüklerinizi SQL Server'dan azure veritabanı hizmetlerine (Azure SQL Veritabanı veya Azure SQL Veritabanı yönetilen örnek) geçirdiğinizde, birincil katma değerli hizmetlerden biri olan SQL Server Tümleştirme Hizmetleri'ndeki (SSIS) ETL iş yüklerinizin de geçirilmesi gerekir.

Azure Veri Fabrikası'ndaki (ADF) Azure-SSIS Tümleştirme Çalışma Süresi (IR), SSIS paketlerini çalıştırmayı destekler. Azure-SSIS IR sağlandıktan sonra, paketlerinizi Azure'da dağıtmak ve çalıştırmak için SQL Server Data Tools (SSDT)/SQL Server Management Studio (SSMS) ve dtinstall/dtutil/dtexec gibi komut satırı yardımcı programları gibi tanıdık araçları kullanabilirsiniz. Daha fazla bilgi için Azure [SSIS kaldırma ve kaydırma genel görünümüne](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview)bakın.

Bu makalede, ETL iş yüklerinizin şirket içi SSIS'ten ADF'deki SSIS'e geçiş süreci vurgulanır. Geçiş süreci iki aşamadan oluşur: **Değerlendirme** ve **Geçiş.**

## <a name="assessment"></a>Değerlendirme

Tam bir geçiş planı oluşturmak için, kapsamlı bir değerlendirme, başarılı bir geçişi engelleyecek kaynak SSIS paketleriyle ilgili sorunların belirlenmesine yardımcı olacaktır.

Veri Geçiş Yardımcısı (DMA), yerel olarak yüklenebilir ve yürütülebilir bu amaç için ücretsiz olarak indirilebilir bir araçtır. SSIS paketlerini toplu olarak değerlendirmek ve aşağıdaki kategorilerde sunulan uyumluluk sorunlarını belirlemek için tip **Tümleştirme Hizmetlerinin** DMA değerlendirme projesi oluşturulabilir:

- Geçiş engelleyicileri: Bunlar, Azure-SSIS IR'de çalışacak geçiş kaynağı paketlerini engelleyen uyumluluk sorunlarıdır. DMA, bu sorunları gidermenize yardımcı olacak kılavuzlar sağlar.

- Bilgilendirici sorunlar: Bunlar kaynak paketlerinde kullanılan kısmen desteklenen veya azalan özelliklerdir. DMA kapsamlı bir öneri kümesi, Azure'da kullanılabilen alternatif yaklaşımlar ve çözüme kavuşturulması gereken azaltıcı adımlar sağlar.

### <a name="four-storage-types-for-ssis-packages"></a>SSIS paketleri için dört depolama türü

- SSIS kataloğu (SSISDB). Bu, SQL Server 2012 ile tanıtıldı ve SSIS projeleri/paketleriyle çalışmak için kullanılan bir dizi depolanmış yordam, görünüm ve tablo değerinde işlevler içerir.
- Dosya Sistemi.
- SQL Server sistem veritabanı (MSDB).
- SSIS Paket Mağazası. Bu, iki alt türünün üstündeki bir paket yönetim katmanıdır:
  - SSIS paketlerini depolamak için kullanılan SQL Server'da bir sistem veritabanı olan MSDB.
  - Yönetilen dosya sistemi, SQL Server yükleme yolunda belirli bir klasör olan SSIS paketlerini depolamak için kullanılır.

DMA şu anda **Dosya Sistemi,** **Paket Deposu**ve **SSIS kataloğunda** depolanan paketlerin toplu değerlendirmesini destekler. **DMA version v5.0**

[DMA](https://docs.microsoft.com/sql/dma/dma-overview)alın ve [onunla paket değerlendirme gerçekleştirmek.](https://docs.microsoft.com/sql/dma/dma-assess-ssis)

## <a name="migration"></a>Geçiş

Kaynak SSIS paketlerinin [depolama türlerine](#four-storage-types-for-ssis-packages) ve veritabanı iş yüklerinin geçiş hedefine bağlı olarak, **SSIS paketlerini ve SSIS** paket yürütmelerini zamanlayan **SQL Server Agent işlerini** geçirme adımları değişebilir. İki senaryo vardır:

- [**Azure SQL Veritabanı veritabanı** iş yükü hedefi olarak yönetilen örnek](#azure-sql-database-managed-instance-as-database-workload-destination)
- [Veritabanı iş yükü hedefi olarak **Azure SQL Veritabanı**](#azure-sql-database-as-database-workload-destination)

### <a name="azure-sql-database-managed-instance-as-database-workload-destination"></a>**Azure SQL Veritabanı veritabanı** iş yükü hedefi olarak yönetilen örnek

| **Paket depolama türü** |SSIS paketleri toplu geçiş nasıl yapılır?|SSIS işleri toplu geçiş nasıl yapılır?|
|-|-|-|
|Ssısdb|[**SSISDB'yi** geçirin](scenario-ssis-migration-ssisdb-mi.md)|[SSIS işlerini Azure SQL Veritabanı yönetilen örnek aracısına geçirin](scenario-ssis-migration-ssisdb-mi.md#ssis-jobs-to-azure-sql-database-managed-instance-agent)|
|Dosya Sistemi|Bunları dtinstall/dtutil/manuel kopya yoluyla paylaşımları/Azure Dosyalarını dosyalamak veya VNet/Self-Hosted IR üzerinden erişmek için dosya sistemlerinde tutmak için yeniden dağıtın. Daha fazla bilgi için, [dtutil yardımcı programı](https://docs.microsoft.com/sql/integration-services/dtutil-utility)bakın.|<li> [SSMS'te SSIS İş Geçişi Sihirbazı] ile geçirin. (how-to-migrate-ssis-job-ssms.md) <li>Komut dosyaları/SSMS/ADF portalı aracılığıyla bunları ADF ardışık larına/etkinliklerine/tetikleyicilerine dönüştürün. Daha fazla bilgi için [SSMS zamanlama özelliğine](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms)bakın.|
|SQL Server (MSDB)|Bunları SSMS/dtutil üzerinden dosya sistemlerine/dosya paylaşımlarına/Azure Dosyalarına aktarın. Daha fazla bilgi için [SSIS paketlerini dışa](https://docs.microsoft.com/sql/integration-services/import-and-export-packages-ssis-service)aktarma ya da dışa bakın.|Komut dosyaları/SSMS/ADF portalı aracılığıyla bunları ADF ardışık larına/etkinliklerine/tetikleyicilerine dönüştürün. Daha fazla bilgi için [SSMS zamanlama özelliğine](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms)bakın.|
|Paket Mağaza|Bunları SSMS/dtutil üzerinden dosya sistemlerine/dosya paylaşımlarına/Azure Dosyalarına aktarın veya dtinstall/dtutil/manuel kopya ile paylaşımları/Azure Dosyalarını dosyaya yeniden dağıtın veya VNet/Self-Hosted IR üzerinden erişmek için dosya sistemlerinde tutun. Daha fazla bilgi için, dtutil yardımcı programı bakın. Daha fazla bilgi için, [dtutil yardımcı programı](https://docs.microsoft.com/sql/integration-services/dtutil-utility)bakın.|Komut dosyaları/SSMS/ADF portalı aracılığıyla bunları ADF ardışık larına/etkinliklerine/tetikleyicilerine dönüştürün. Daha fazla bilgi için [SSMS zamanlama özelliğine](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms)bakın.|

### <a name="azure-sql-database-as-database-workload-destination"></a>Veritabanı iş yükü hedefi olarak **Azure SQL Veritabanı**

| **Paket depolama türü** |SSIS paketleri toplu geçiş nasıl yapılır?|Toplu iş geçiş i|
|-|-|-|
|Ssısdb|SSDT/SSMS ile Azure-SSISDB'ye yeniden dağıtın. Daha fazla bilgi için bkz: [Azure'da SSIS paketlerini dağıtma.](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)|Komut dosyaları/SSMS/ADF portalı aracılığıyla bunları ADF ardışık larına/etkinliklerine/tetikleyicilerine dönüştürün. Daha fazla bilgi için [SSMS zamanlama özelliğine](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms)bakın.|
|Dosya Sistemi|Bunları dtinstall/dtutil/manuel kopya yoluyla paylaşımları/Azure Dosyalarını dosyalamak veya VNet/Self-Hosted IR üzerinden erişmek için dosya sistemlerinde tutmak için yeniden dağıtın. Daha fazla bilgi için, [dtutil yardımcı programı](https://docs.microsoft.com/sql/integration-services/dtutil-utility)bakın.|<li> [SSMS'te SSIS İş Geçişi Sihirbazı] ile geçirin. (how-to-migrate-ssis-job-ssms.md) <li> Komut dosyaları/SSMS/ADF portalı aracılığıyla bunları ADF ardışık larına/etkinliklerine/tetikleyicilerine dönüştürün. Daha fazla bilgi için [SSMS zamanlama özelliğine](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms)bakın.|
|SQL Server (MSDB)|Bunları SSMS/dtutil üzerinden dosya sistemlerine/dosya paylaşımlarına/Azure Dosyalarına aktarın. Daha fazla bilgi için [SSIS paketlerini dışa](https://docs.microsoft.com/sql/integration-services/import-and-export-packages-ssis-service)aktarma ya da dışa bakın.|Komut dosyaları/SSMS/ADF portalı aracılığıyla bunları ADF ardışık larına/etkinliklerine/tetikleyicilerine dönüştürün. Daha fazla bilgi için [SSMS zamanlama özelliğine](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms)bakın.|
|Paket Mağaza|Bunları SSMS/dtutil üzerinden dosya sistemlerine/dosya paylaşımlarına/Azure Dosyalarına aktarın veya dtinstall/dtutil/manuel kopya ile paylaşımları/Azure Dosyalarını dosyaya yeniden dağıtın veya VNet/Self-Hosted IR üzerinden erişmek için dosya sistemlerinde tutun. Daha fazla bilgi için, dtutil yardımcı programı bakın. Daha fazla bilgi için, [dtutil yardımcı programı](https://docs.microsoft.com/sql/integration-services/dtutil-utility)bakın.|Komut dosyaları/SSMS/ADF portalı aracılığıyla bunları ADF ardışık larına/etkinliklerine/tetikleyicilerine dönüştürün. Daha fazla bilgi için [SSMS zamanlama özelliğine](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms)bakın.|

## <a name="additional-resources"></a>Ek kaynaklar

- [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction)
- [Veritabanı Geçiş Yardımcısı](https://docs.microsoft.com/sql/dma/dma-overview)
- [SSIS iş yüklerini buluta kaldırın ve kaydırın](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview?view=sql-server-2017)
- [SSIS paketlerini Azure SQL Veritabanı yönetilen örneğine geçirme](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages-managed-instance)
- [Paketleri Azure SQL Veritabanına yeniden dağıtma](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'a dağıtılan SSIS paketlerini doğrulama](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-validate-packages)
- [Azure'da dağıtılan SSIS paketlerini çalıştırma](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-run-packages)
- [Azure-SSIS Integration Runtime'ı izleme](https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime)
- [Azure'da SSIS paket yürütmelerini zamanlama](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
