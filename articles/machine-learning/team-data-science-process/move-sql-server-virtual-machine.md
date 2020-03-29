---
title: Verileri SQL Server sanal makinesine taşıma - Takım Veri Bilimi Süreci
description: Verileri düz dosyalardan veya şirket içi bir SQL Server'dan Azure VM'de SQL Server'a taşıyın.
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
ms.openlocfilehash: b8a01b5f2f5ec64fea014468356408220f9c4f1a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721379"
---
# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a>Bir Azure sanal makinesinde SQL Server’a veri taşıma

Bu makalede, verileri düz dosyalardan (CSV veya TSV biçimleri) veya bir şirket içi SQL Server'dan Azure sanal makinesindeki SQL Server'a taşıma seçenekleri özetlenmektedir. Verileri buluta taşımak için yapılan bu görevler, Ekip Veri Bilimi Sürecinin bir parçasıdır.

Verileri Machine Learning için Azure SQL Veritabanına taşıma seçeneklerini özetleyen bir konu [için](move-sql-azure.md)bkz.

Aşağıdaki tablo, bir Azure sanal makinesinde verileri SQL Server'a taşıma seçeneklerini özetley.

| <b>Kaynak</b> | <b>HEDEF: Azure VM'de SQL Server</b> |
| --- | --- |
| <b>Düz Dosya</b> |1. <a href="#insert-tables-bcp">Komut satırı toplu kopya yardımcı programı (BCP)</a><br> 2. <a href="#insert-tables-bulkquery">Toplu Ekleme SQL Sorgusu</a><br> 3. <a href="#sql-builtin-utilities">SQL Server'da Grafiksel Yerleşik Yardımcı Programlar</a> |
| <b>Şirket Içi SQL Server</b> |1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Microsoft Azure VM sihirbazına SQL Server Veritabanı dağıtma</a><br> 2. <a href="#export-flat-file">Düz Bir Dosyaya Dışa Aktarma</a><br> 3. <a href="#sql-migration">SQL Veritabanı Geçiş Sihirbazı</a> <br> 4. <a href="#sql-backup">Veritabanı yedekleme ve geri yükleme</a><br> |

Bu belge, SQL komutlarının SQL Server Management Studio veya Visual Studio Database Explorer'dan yürütüldedildiğini varsayar.

> [!TIP]
> Alternatif olarak, verileri Azure'daki bir SQL Server VM'ye taşıyacak bir ardışık hatlar oluşturmak ve zamanlamak için [Azure Veri Fabrikası'nı](https://azure.microsoft.com/services/data-factory/) kullanabilirsiniz. Daha fazla bilgi için [bkz.](../../data-factory/copy-activity-overview.md)
>
>

## <a name="prerequisites"></a><a name="prereqs"></a>Ön koşullar
Bu öğretici var sayıyor:

* **Azure aboneliği.** Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Bir **Azure depolama hesabı.** Verileri bu öğreticide depolamak için bir Azure depolama hesabı kullanırsınız. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md) makalesine bakın. Depolama hesabını oluşturduktan sonra, depolama alanına erişmek için kullanılan hesap anahtarını edinmeniz gerekir. Bkz. [Depolama hesabı erişim anahtarlarını yönet.](../../storage/common/storage-account-keys-manage.md)
* Azure **VM'de**sağlanan SQL Server . Talimatlar için bkz. [Gelişmiş analitik için bir Azure SQL Server sanal makinesini IPython Notebook sunucusu olarak ayarlayın.](../data-science-virtual-machine/setup-sql-server-virtual-machine.md)
* **Azure PowerShell'i** yerel olarak yükledi ve yapılandırıldı. Talimatlar için Azure [PowerShell'i nasıl yükleyip yapılandıracağınıza](/powershell/azure/overview)bakın.

## <a name="moving-data-from-a-flat-file-source-to-sql-server-on-an-azure-vm"></a><a name="filesource_to_sqlonazurevm"></a>Azure VM'de verileri düz bir dosya kaynağından SQL Server'a taşıma
Verileriniz düz bir dosyadaysa (satır/sütun biçiminde düzenlenmişse), aşağıdaki yöntemlerle Azure'daki SQL Server VM'ye taşınabilir:

1. [Komut satırı toplu kopya yardımcı programı (BCP)](#insert-tables-bcp)
2. [Toplu Ekle SQL Sorgusu](#insert-tables-bulkquery)
3. [SQL Server'da Grafiksel Yerleşik Yardımcı Programlar (Alma/Dışa Aktarma, SSIS)](#sql-builtin-utilities)

### <a name="command-line-bulk-copy-utility-bcp"></a><a name="insert-tables-bcp"></a>Komut satırı toplu kopya yardımcı programı (BCP)
BCP, SQL Server ile yüklü bir komut satırı yardımcı programıdır ve verileri taşımanın en hızlı yollarından biridir. Üç SQL Server varyantında da (Azure'da şirket içi SQL Server, SQL Azure ve SQL Server VM) çalışır.

> [!NOTE]
> **BCP için verilerim nerede olmalıdır?**  
> Gerekli olmasa da, hedef SQL Server ile aynı makinede bulunan kaynak verileri içeren dosyalar daha hızlı aktarımlara (ağ hızı vs yerel disk IO hızı) izin verir. AzCopy , [Azure Depolama Gezgini](https://storageexplorer.com/) veya Uzak Masaüstü Protokolü (RDP) [AZCopy](../../storage/common/storage-use-azcopy.md)aracılığıyla windows kopyalama/yapıştır gibi çeşitli dosya kopyalama araçlarını kullanarak, veri içeren düz dosyaları SQL Server'ın yüklü olduğu makineye taşıyabilirsiniz.
>
>

1. Veritabanının ve tabloların hedef SQL Server veritabanında oluşturulduğundan emin olun. Aşağıda, `Create Database` aşağıdaki `Create Table` komutları kullanarak bunu nasıl yapacağınız hakkında bir örnek verilmiştir:

    ```sql
    CREATE DATABASE <database_name>
    
    CREATE TABLE <tablename>
    (
        <columnname1> <datatype> <constraint>,
        <columnname2> <datatype> <constraint>,
        <columnname3> <datatype> <constraint>
    )
    ```

1. BCP'nin yüklü olduğu makinenin komut satırından aşağıdaki komutu vererek tablonun şemasını açıklayan biçim dosyasını oluşturun.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n`
1. SQL Server aynı makineye yüklendiğinde komut satırından çalışması gereken bcp komutunu kullanarak verileri veritabanına ekleyin:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attempt -t \t -r \n`

> **BCP Uçlarını Optimize Etme** Bu tür ekler optimize etmek için lütfen aşağıdaki makaleye bakın ['Toplu Alma Optimize Rehberi'.](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx)
>
>

### <a name="parallelizing-inserts-for-faster-data-movement"></a><a name="insert-tables-bulkquery-parallel"></a>Daha Hızlı Veri Hareketi Için Eklerin Paralelleştirilmesi
Hareket ettirdiğiniz veriler büyükse, PowerShell Script'te aynı anda birden çok BCP komutunu paralel olarak çalıştırarak işleri hızlandırabilirsiniz.

> [!NOTE]
> **Büyük veri Alımı** Büyük ve çok büyük veri kümeleri için veri yüklemeyi en iyi duruma getirmek için, birden çok dosya grubu ve bölüm tabloları kullanarak mantıksal ve fiziksel veritabanı tablolarınızı bölümleyin. Bölüm tablolarına veri oluşturma ve yükleme hakkında daha fazla bilgi için [Paralel Yük SQL Partition Tabloları'na](parallel-load-sql-partitioned-tables.md)bakın.
>
>

Aşağıdaki örnek PowerShell komut dosyası bcp kullanarak paralel ekler gösterir:

```powershell
$NO_OF_PARALLEL_JOBS=2

Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
# Define what each job does
$ScriptBlock = {
    param($partitionnumber)

    #Explicitly using SQL username password
    bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

    #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
    #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n
}


# Background processing of all partitions
for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
{
    Write-Debug "Submit loading partition # $i"
    Start-Job $ScriptBlock -Arg $i      
}


# Wait for it all to complete
While (Get-Job -State "Running")
{
    Start-Sleep 10
    Get-Job
}

# Getting the information back from the jobs
Get-Job | Receive-Job
Set-ExecutionPolicy Restricted #reset the execution policy
```

### <a name="bulk-insert-sql-query"></a><a name="insert-tables-bulkquery"></a>Toplu Ekle SQL Sorgusu
[Toplu Ekleme SQL Sorgusu](https://msdn.microsoft.com/library/ms188365) satır/sütun tabanlı dosyalardan veritabanına veri almak için kullanılabilir (desteklenen türler[Toplu Dışa Aktarma veya Alma (SQL Server) için Veri Hazırlama (SQL Server)](https://msdn.microsoft.com/library/ms188609)başlığında ele alınmıştır.

Toplu Ekleme için bazı örnek komutlar aşağıda verilmiştir:  

1. SQL Server veritabanının tarihler gibi özel alanlar için aynı biçimi üstlendiğinden emin olmak için verilerinizi analiz edin ve içe aktarmadan önce özel seçenekler ayarlayın. Tarih biçimini yıl-ay günü olarak nasıl ayarladığınıza ilgili bir örnek aşağıda verilmiştir (verileriniz yıl-ay-gün biçimindeki tarihi içeriyorsa):

    ```sql
    SET DATEFORMAT ymd;
    ```
2. Toplu alma ekstrelerini kullanarak veri alma:

    ```sql
    BULK INSERT <tablename>
    FROM
    '<datafilename>'
    WITH
    (
        FirstRow = 2,
        FIELDTERMINATOR = ',', --this should be column separator in your data
        ROWTERMINATOR = '\n'   --this should be the row separator in your data
    )
    ```

### <a name="built-in-utilities-in-sql-server"></a><a name="sql-builtin-utilities"></a>SQL Server'da Yerleşik Yardımcı Programlar
Sql Server Integration Services (SSIS) kullanarak Azure'daki SQL Server VM'ye düz bir dosyadan veri aktarabilirsiniz.
SSIS iki stüdyo ortamda mevcuttur. Ayrıntılar için [Entegrasyon Hizmetleri (SSIS) ve Stüdyo Ortamları'na](https://technet.microsoft.com/library/ms140028.aspx)bakın:

* SQL Server Veri Araçları hakkında ayrıntılı bilgi için [Microsoft SQL Server Veri Araçları](https://msdn.microsoft.com/data/tools.aspx)  
* İçe/Aktar Sihirbazı hakkında ayrıntılı bilgi için [SQL Server İçe Aktarma ve Verme Sihirbazı'na](https://msdn.microsoft.com/library/ms141209.aspx) bakın

## <a name="moving-data-from-on-premises-sql-server-to-sql-server-on-an-azure-vm"></a><a name="sqlonprem_to_sqlonazurevm"></a>Azure VM'de Verileri şirket içi SQL Server'dan SQL Server'a taşıma
Aşağıdaki geçiş stratejilerini de kullanabilirsiniz:

1. [Microsoft Azure VM sihirbazına SQL Server Veritabanı dağıtma](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Düz Dosyaya Dışa Aktarma](#export-flat-file)
3. [SQL Veritabanı Geçiş Sihirbazı](#sql-migration)
4. [Veritabanı yedekleme ve geri yükleme](#sql-backup)

Aşağıdaki seçeneklerin her birini açıklıyoruz:

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Microsoft Azure VM sihirbazına SQL Server Veritabanı dağıtma
**SQL Server Veritabanını Microsoft Azure VM sihirbazına dağıtma,** verileri şirket içi BIR SQL Server örneğinden Azure VM'deki SQL Server'a taşımanın basit ve önerilir yoludur. Ayrıntılı adımların yanı sıra diğer alternatiflerin tartışılması [için](../../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md)bkz.

### <a name="export-to-flat-file"></a><a name="export-flat-file"></a>Düz Dosyaya Dışa Aktarma
[Toplu Veri Alma ve Veri Dışa Aktar(SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) konusunda belgelenen Şirket İçi SQL Server'dan gelen dışa aktarma verilerini toplu olarak yapmak için çeşitli yöntemler kullanılabilir. Bu belge örnek olarak Toplu Kopyalama Programı'nı (BCP) kapsayacaktır. Veriler düz bir dosyaya dışa aktarılındıktan sonra, toplu alma kullanarak başka bir SQL sunucusuna aktarılabilir.

1. BCP yardımcı programını kullanarak şirket içi SQL Server'daki verileri dosyaya şu şekilde aktarın

    `bcp dbname..tablename out datafile.tsv -S    servername\sqlinstancename -T -t \t -t \n -c`
2. Veritabanını ve tabloyu Azure'daki SQL Server `create database` `create table` VM'de, adım 1'de dışa aktarılan şema ve tablo şemasını kullanarak oluşturun.
3. Dışa aktarılan/alınan verilerin tablo şemasını açıklamak için bir biçim dosyası oluşturun. Biçim dosyasının ayrıntıları [Biçim Dosyası Oluştur 'da (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx)açıklanmıştır.

    SQL Server makinesinden BCP çalıştırırken dosya oluşturma biçimini biçimlendirme

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n

    BCP'yi bir SQL Server'a uzaktan çalıştırırken dosya oluşturma biçimini biçimlendirme

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
4. Verileri düz dosyalardaki bir SQL Server'a taşımak için [Dosya Kaynağı'ndan Veri Taşıma](#filesource_to_sqlonazurevm) bölümünde açıklanan yöntemlerden herhangi birini kullanın.

### <a name="sql-database-migration-wizard"></a><a name="sql-migration"></a>SQL Veritabanı Geçiş Sihirbazı
[SQL Server Database Geçiş Sihirbazı,](https://sqlazuremw.codeplex.com/) verileri iki SQL sunucu örneği arasında taşımak için kullanıcı dostu bir yol sağlar. Kullanıcının veri şemasını kaynaklar ve hedef tablolar arasında eşlemesine, sütun türlerini ve diğer çeşitli işlevleri seçmesine olanak tanır. Kapakların altında toplu kopya (BCP) kullanır. SQL Veritabanı Geçiş sihirbazı için karşılama ekranının ekran görüntüsü aşağıda gösterilmiştir.  

![SQL Server Geçiş Sihirbazı][2]

### <a name="database-back-up-and-restore"></a><a name="sql-backup"></a>Veritabanı yedekleme ve geri yükleme
SQL Server destekler:

1. [Veritabanı yedekleme ve işlevselliği geri yükleme](https://msdn.microsoft.com/library/ms187048.aspx) (hem yerel bir dosyaya hem de blob'a bacpac dışa aktarma) ve [Veri Katmanı Uygulamaları](https://msdn.microsoft.com/library/ee210546.aspx) (bacpac kullanarak).
2. Kopyalanmış bir veritabanıyla Azure'da doğrudan SQL Server VM'leri oluşturabilme veya varolan bir SQL Azure veritabanına kopyalayabilme. Daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/ms188664.aspx)

SQL Server Management Studio'dan Veritabanı yedekleme/geri yükleme seçeneklerinin ekran görüntüsü aşağıda gösterilmiştir.

![SQL Server Alma Aracı][1]

## <a name="resources"></a>Kaynaklar
[Azure VM'de Veritabanını SQL Server'a geçirme](../../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md)

[Azure Sanal Makineler’de SQL Server’a genel bakış](../../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/move-sql-server-virtual-machine/database_migration_wizard.png
