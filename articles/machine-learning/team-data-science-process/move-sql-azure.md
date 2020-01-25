---
title: Azure SQL veritabanı - Team Data Science Process veri taşıma
description: Verileri düz dosyalardan (CSV veya TSV biçimlerinden) veya şirket içi SQL Server depolanan verilerden Azure SQL veritabanı 'na taşıyın.
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
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76722467"
---
# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>Azure Machine Learning için Azure SQL Veritabanına veri taşıma

Bu makalede, verileri düz dosyalardan (CSV veya TSV biçimlerinden) ya da şirket içi SQL Server depolanan verilerden Azure SQL veritabanına taşıma seçenekleri özetlenmektedir. Verileri buluta taşımak için bu görevleri Team Data Science Process bir parçasıdır.

Machine Learning için bir şirket içi SQL Server'a veri taşıma için seçenekler özetlenmektedir bir konuya bakın [veri taşıma için SQL Server Azure sanal makinesinde](move-sql-server-virtual-machine.md).

Aşağıdaki tabloda, bir Azure SQL veritabanı'na veri taşımak için seçenekler özetlenmektedir.

| <b>KAYNAK</b> | <b>Hedef: Azure SQL veritabanı</b> |
| --- | --- |
| <b>Düz dosya (CSV veya biçimlendirilmiş TSV)</b> |[Toplu ekleme SQL sorgusu](#bulk-insert-sql-query) |
| <b>Şirket içi SQL Server</b> |1.[dışarı aktarmak için düz dosya](#export-flat-file)<br> 2. [SQL veritabanı geçiş Sihirbazı](#insert-tables-bcp)<br> 3. [veritabanı yedekleme ve geri yükleme](#db-migration)<br> 4. [Azure Data Factory](#adf) |

## <a name="prereqs"></a>Önkoşullar
Burada özetlenen yordamlar sahip olmanızı gerektirir:

* Bir **Azure aboneliği**. Bir aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Bir **Azure depolama hesabı**. Bu öğreticide verilerin depolanması için bir Azure depolama hesabını kullanırsınız. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md) makalesine bakın. Depolama hesabını oluşturduktan sonra, depolamaya erişmek için kullanılan hesap anahtarını edinmeniz gerekir. Bkz. [depolama hesabı erişim anahtarlarını yönetme](../../storage/common/storage-account-keys-manage.md).
* Erişim bir **Azure SQL veritabanı**. Bir Azure SQL veritabanı, ayarlamanız gerekirse [Microsoft Azure SQL veritabanı ile çalışmaya başlama](../../sql-database/sql-database-get-started.md) Azure SQL veritabanı yeni bir örneğini sağlama hakkında bilgi sağlar.
* Yüklenmiş ve yapılandırılmış **Azure PowerShell** yerel olarak. Yönergeler için [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).

**Veri**: kullanarak geçiş işlemleri gösterilmiştir [NYC taksi dataset](https://chriswhong.com/open-data/foil_nyc_taxi/). NYC taksi dataset seyahat verilerini ve fairs ilgili bilgiler içerir ve Azure blob depolama alanında kullanılabilir: [NYC taksi verileri](https://www.andresmh.com/nyctaxitrips/). Bir örnek ve açıklama bu dosyaların sağlanan [NYC taksi Gelişlerin veri kümesi tanımı](sql-walkthrough.md#dataset).

Kendi veri kümesine burada açıklanan yordamlar uyarlayabilir veya NYC taksi veri kümesini kullanarak açıklanan adımları izleyin. Şirket içi SQL Server veritabanınıza NYC taksi veri kümesini yüklemek için bölümünde açıklanan yordamı izleyin [toplu içeri aktarma verileri SQL Server veritabanına](sql-walkthrough.md#dbload). SQL Server üzerinde bir Azure sanal makine için bu yönergeleri yöneliktir, ancak şirket içi SQL Server'a yükleme yordamı aynıdır.

## <a name="file-to-azure-sql-database"></a>Verileri düz bir dosya kaynağından Azure SQL veritabanına taşıma
Düz dosyalardaki (CSV veya TSV biçimli) veriler, toplu ekleme SQL sorgusu kullanarak bir Azure SQL veritabanına taşınabilir.

### <a name="bulk-insert-sql-query"></a> Toplu ekleme SQL sorgusu
Toplu ekleme SQL sorgusunu kullanan yordamın adımları, verileri bir Azure VM üzerindeki SQL Server bir düz dosya kaynağından taşıma yönlerine benzer. Ayrıntılar için bkz [toplu ekleme SQL sorgusu](move-sql-server-virtual-machine.md#insert-tables-bulkquery).

## <a name="sql-on-prem-to-sazure-sql-database"></a>Verileri şirket içi SQL Server Azure SQL veritabanına taşıma
Kaynak veriler şirket içi SQL Server depolanıyorsa, verileri bir Azure SQL veritabanına taşımaya yönelik çeşitli olanaklar vardır:

1. [Düz dosyaya](#export-flat-file)
2. [SQL veritabanı Geçiş Sihirbazı](#insert-tables-bcp)
3. [Yedekleme geri veritabanı ve geri yükleme](#db-migration)
4. [Azure Data Factory](#adf)

İlk üç adım, verileri aynı yordamları kapsayan [bir Azure sanal makinesinde SQL Server taşıma](move-sql-server-virtual-machine.md) bölümündeki bölümlerle benzerdir. Bu konudaki uygun bölümleri bağlantılar aşağıdaki yönergelerde yer verilmiştir.

### <a name="export-flat-file"></a>Düz dosyaya
Düz bir dosyaya dışa aktarma adımları, [düz dosyaya dışa aktarma](move-sql-server-virtual-machine.md#export-flat-file)içinde kapsanan yönlere benzerdir.

### <a name="insert-tables-bcp"></a>SQL veritabanı Geçiş Sihirbazı
SQL veritabanı geçiş Sihirbazı 'Nı kullanma adımları [SQL veritabanı geçiş Sihirbazı](move-sql-server-virtual-machine.md#sql-migration)'nda ele alınan yönlere benzerdir.

### <a name="db-migration"></a>Yedekleme geri veritabanı ve geri yükleme
Veritabanı yedeklemesini ve geri yüklemeyi kullanma adımları, [veritabanı yedekleme ve geri yükleme](move-sql-server-virtual-machine.md#sql-backup)bölümünde listelenen yönlere benzerdir.

### <a name="adf"></a>Azure veri fabrikası
Bu konudaki verileri bir Azure SQL veritabanına taşıma hakkında bilgi edinin Azure Data Factory (ADF), Şirket [ıçı SQL Server 'dan verileri Azure Data Factory SQL Azure taşıyın](move-sql-azure-adf.md). Bu konu başlığı altında, Azure Blob depolama aracılığıyla şirket içi SQL Server veritabanından Azure SQL veritabanına veri taşımak için ADF 'nin nasıl kullanılacağı gösterilmektedir.

Verilerin karma şirket içi ve bulut kaynaklarıyla sürekli geçirilmesi gerektiğinde ADF kullanmayı göz önünde bulundurun.  ADF Ayrıca verilerin Dönüştürmelere ihtiyacı olduğunda veya geçiş sırasında yeni iş mantığına ihtiyacı olduğunda de yardımcı olur. Zamanlama ve düzenli aralıklarla veri taşıma işlemlerini yönetmek basit JSON betiklerini kullanarak işleri izlemek için ADF sağlar. ADF karmaşık işlemleri desteği gibi diğer özellikleri de vardır.
