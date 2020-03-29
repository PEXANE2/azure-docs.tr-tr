---
title: Verileri Azure SQL Veritabanına Taşıma - Takım Veri Bilimi Süreci
description: Verileri düz dosyalardan (CSV veya TSV biçimleri) veya şirket içi BIR SQL Server'da depolanan verilerden Azure SQL Veritabanına taşıyın.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: f9a1424f2afe6c5153e208601b21dff9651880a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76722467"
---
# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>Azure Machine Learning için Azure SQL Veritabanına veri taşıma

Bu makalede, verileri düz dosyalardan (CSV veya TSV biçimleri) veya şirket içi SQL Server'da depolanan verilerden Azure SQL Veritabanına taşıma seçenekleri özetlenmektedir. Verileri buluta taşımak için yapılan bu görevler, Ekip Veri Bilimi Sürecinin bir parçasıdır.

Verileri makine öğrenimi için şirket içi BIR SQL Server'a taşıma seçeneklerini özetleyen bir konu için, [azure sanal makinesinde verileri SQL Server'a taşıyın'](move-sql-server-virtual-machine.md)a bakın.

Aşağıdaki tabloda, verileri Bir Azure SQL Veritabanına taşıma seçenekleri özetlenmiştir.

| <b>Kaynak</b> | <b>HEDEF: Azure SQL Veritabanı</b> |
| --- | --- |
| <b>Düz dosya (CSV veya TSV biçimlendirilmiş)</b> |[Toplu Ekle SQL Sorgusu](#bulk-insert-sql-query) |
| <b>Şirket Içi SQL Server</b> |1.[Düz Dosyaya İhracat](#export-flat-file)<br> 2. [SQL Veritabanı Geçiş Sihirbazı](#insert-tables-bcp)<br> 3. [Veritabanı yedekleme ve geri yükleme](#db-migration)<br> 4. [Azure Veri Fabrikası](#adf) |

## <a name="prerequisites"></a><a name="prereqs"></a>Ön koşullar
Burada özetlenen yordamlar şunları gerektirir:

* **Azure aboneliği.** Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Bir **Azure depolama hesabı.** Verileri bu öğreticide depolamak için bir Azure depolama hesabı kullanırsınız. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md) makalesine bakın. Depolama hesabını oluşturduktan sonra, depolamaya erişmek için kullanılan hesap anahtarını edinmeniz gerekir. Bkz. [Depolama hesabı erişim anahtarlarını yönet.](../../storage/common/storage-account-keys-manage.md)
* **Azure SQL Veritabanına**erişim. Bir Azure SQL Veritabanı ayarlamanız gerekiyorsa, [Microsoft Azure SQL Veritabanı ile Başlarken,](../../sql-database/sql-database-get-started.md) Azure SQL Veritabanı'nın yeni bir örneğini nasıl sağabileceğiniz hakkında bilgi sağlar.
* **Azure PowerShell'i** yerel olarak yükledi ve yapılandırıldı. Talimatlar için Azure [PowerShell'i nasıl yükleyip yapılandıracağınıza](/powershell/azure/overview)bakın.

**Veri**: Geçiş işlemleri NYC [Taksi veri seti](https://chriswhong.com/open-data/foil_nyc_taxi/)kullanılarak gösterilmiştir. NYC Taksi veri seti seyahat verileri ve fuarlar hakkında bilgi içerir ve Azure blob depolama mevcuttur: [NYC Taksi Verileri](https://www.andresmh.com/nyctaxitrips/). Bir örnek ve bu dosyaların açıklaması [NYC Taksi Gezileri Dataset Açıklama](sql-walkthrough.md#dataset)sağlanmaktadır.

Burada açıklanan yordamları kendi verilerinizin kümesine uyarlayabilir veya NYC Taxi veri kümesini kullanarak açıklanan adımları izleyebilirsiniz. NYC Taxi veri kümesini şirket içi SQL Server veritabanınıza yüklemek için, Toplu Alma Verilerinde özetlenen yordamı [SQL Server Veritabanı'na](sql-walkthrough.md#dbload)uygulayın. Bu yönergeler Azure Sanal Makine'deki bir SQL Server içindir, ancak şirket içi SQL Server'a yükleme yordamı aynıdır.

## <a name="moving-data-from-a-flat-file-source-to-an-azure-sql-database"></a><a name="file-to-azure-sql-database"></a>Verileri düz bir dosya kaynağından Azure SQL Veritabanına taşıma
Düz dosyalardaki veriler (CSV veya TSV biçimlendirilmiş) Toplu Ekle SQL Sorgusu kullanılarak Azure SQL Veritabanına taşınabilir.

### <a name="bulk-insert-sql-query"></a><a name="bulk-insert-sql-query"></a>Toplu Ekle SQL Sorgusu
Toplu Ekle SQL Sorgusu'nu kullanarak yordam için gereken adımlar, verileri düz bir dosya kaynağından Azure VM'de SQL Server'a taşımak için yapılan yönergelere benzer. Ayrıntılar için [bkz: Toplu Ekle SQL Sorgusu](move-sql-server-virtual-machine.md#insert-tables-bulkquery).

## <a name="moving-data-from-on-premises-sql-server-to-an-azure-sql-database"></a><a name="sql-on-prem-to-sazure-sql-database"></a>Verileri şirket içi SQL Server'dan Azure SQL Veritabanına Taşıma
Kaynak veriler şirket içi bir SQL Server'da depolanırsa, verileri Bir Azure SQL Veritabanına taşımak için çeşitli olanaklar vardır:

1. [Düz Dosyaya Dışa Aktarma](#export-flat-file)
2. [SQL Veritabanı Geçiş Sihirbazı](#insert-tables-bcp)
3. [Veritabanı yedekleme ve geri yükleme](#db-migration)
4. [Azure Data Factory](#adf)

İlk üç ünün adımları, aynı yordamları kapsayan [bir Azure sanal makinesindeki verileri SQL Server'a Taşı'daki](move-sql-server-virtual-machine.md) bölümlere benzer. Bu konudaki ilgili bölümlere bağlantılar aşağıdaki talimatlarda verilmiştir.

### <a name="export-to-flat-file"></a><a name="export-flat-file"></a>Düz Dosyaya Dışa Aktarma
Düz bir dosyaya bu dışa aktarma adımları, [Düz Dosyaya Dışa Aktar'da](move-sql-server-virtual-machine.md#export-flat-file)kapsanan bu yönergelere benzer.

### <a name="sql-database-migration-wizard"></a><a name="insert-tables-bcp"></a>SQL Veritabanı Geçiş Sihirbazı
SQL Veritabanı Geçiş Sihirbazı'nı kullanma [adımları, SQL Veritabanı Geçiş Sihirbazı'nda](move-sql-server-virtual-machine.md#sql-migration)kapsanan bu yol tariflerine benzer.

### <a name="database-back-up-and-restore"></a><a name="db-migration"></a>Veritabanı yedekleme ve geri yükleme
Veritabanı yedekleme ve geri yükleme kullanma adımları [Veritabanı yedekleme ve geri yükleme](move-sql-server-virtual-machine.md#sql-backup)listelenen bu yönergeleri benzer.

### <a name="azure-data-factory"></a><a name="adf"></a>Azure Data Factory
Bu konuda Azure Veri Fabrikası (ADF) ile verileri Azure Sql Veritabanına nasıl taşıyarak taşıyın, [verileri şirket içi bir SQL sunucusundan Azure Veri Fabrikası ile SQL Azure'a taşıyın.](move-sql-azure-adf.md) Bu konu, verileri şirket içi BIR SQL Server veritabanından Azure Blob Depolama üzerinden Azure SQL Veritabanına taşımak için ADF'nin nasıl kullanılacağını gösterir.

Verilerin karma şirket içi ve bulut kaynaklarıyla sürekli olarak geçirilmesi gerektiğinde ADF kullanmayı düşünün.  ADF, verilerin dönüşümlere ihtiyacı olduğunda veya geçiş sırasında yeni iş mantığına ihtiyaç duyduğunda da yardımcı olur. ADF, verilerin hareketini periyodik olarak yöneten basit JSON komut dosyalarını kullanarak işlerin zamanlanmasına ve izlenmesine olanak tanır. ADF'nin karmaşık işlemler için destek gibi başka yetenekleri de vardır.
