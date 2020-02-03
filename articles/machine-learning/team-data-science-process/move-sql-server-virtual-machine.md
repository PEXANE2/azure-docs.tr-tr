---
title: Bir SQL Server sanal makinesi - Team Data Science Process için veri taşıma
description: Verileri düz dosyalara veya bir şirket içi SQL Server'dan Azure sanal makinesinde SQL Server'a taşıyın.
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
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721379"
---
# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a>Bir Azure sanal makinesinde SQL Server’a veri taşıma

Bu makalede, verileri düz dosyaları (CSV veya TSV biçimleri) veya bir şirket içi SQL Server'dan SQL Server için Azure sanal makinesinde taşımak için seçenekler özetlenmektedir. Verileri buluta taşımak için bu görevleri Team Data Science Process bir parçasıdır.

Machine Learning için verileri bir Azure SQL veritabanına taşıma seçeneklerini özetleyen bir konu için bkz. [Azure Machine Learning için verileri Azure SQL veritabanına taşıma](move-sql-azure.md).

Aşağıdaki tabloda, verileri bir Azure sanal makinesinde SQL Server'a taşımak için seçenekler özetlenmektedir.

| <b>KAYNAKTAKI</b> | <b>Hedef: Azure VM 'de SQL Server</b> |
| --- | --- |
| <b>Düz dosya</b> |1. <a href="#insert-tables-bcp">komut satırı toplu kopyalama yardımcı programı (bcp)</a><br> 2. <a href="#insert-tables-bulkquery">SQL sorgusunu toplu ekleme</a><br> 3. <a href="#sql-builtin-utilities">SQL Server Içindeki grafik yerleşik yardımcı programlar</a> |
| <b>Şirket Içi SQL Server</b> |1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">bir SQL Server veritabanını MICROSOFT Azure VM sihirbazına dağıtma</a><br> 2. <a href="#export-flat-file">düz bir dosyaya aktarın</a><br> 3. <a href="#sql-migration">SQL veritabanı geçiş Sihirbazı</a> <br> 4. <a href="#sql-backup">veritabanı yedekleme ve geri yükleme</a><br> |

Bu belgede SQL komutlarının SQL Server Management Studio veya Visual Studio Veritabanı Gezgini yürütüldüğü varsayılmaktadır.

> [!TIP]
> Alternatif olarak, verileri Azure 'da bir SQL Server VM taşıyacağınız bir işlem hattı oluşturmak ve zamanlamak için [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Data Factory verileri kopyalama (kopyalama etkinliği)](../../data-factory/copy-activity-overview.md).
>
>

## <a name="prereqs"></a>Önkoşullar
Bu öğreticide, sahip olduğunuz varsayılır:

* Bir **Azure aboneliği**. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Bir **Azure depolama hesabı**. Bu öğreticide verilerin depolanması için bir Azure depolama hesabı kullanır. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md) makalesine bakın. Depolama hesabı oluşturduktan sonra depolamaya erişmek için kullanılan hesap anahtarını edinmeniz gerekir. Bkz. [depolama hesabı erişim anahtarlarını yönetme](../../storage/common/storage-account-keys-manage.md).
* **Azure VM üzerinde SQL Server**sağlandı. Yönergeler için bkz. [Azure SQL Server sanal makinesini gelişmiş analizler için bir IPython Not defteri sunucusu olarak ayarlama](../data-science-virtual-machine/setup-sql-server-virtual-machine.md).
* **Azure PowerShell** yerel olarak yüklendi ve yapılandırıldı. Yönergeler için bkz. [Azure PowerShell nasıl yüklenir ve yapılandırılır](/powershell/azure/overview).

## <a name="filesource_to_sqlonazurevm"></a>Verileri bir Azure VM 'deki SQL Server düz bir dosya kaynağından taşıma
Verilerinizi (bir satır/sütun biçimde düzenlenmesi) düz bir dosya ise, bu SQL Server VM'si için Azure'da aşağıdaki yöntemleri taşınabilir:

1. [Komut satırı toplu kopyalama yardımcı programı (BCP)](#insert-tables-bcp)
2. [SQL sorgusunu toplu ekleme](#insert-tables-bulkquery)
3. [SQL Server 'de grafik yerleşik yardımcı programları (Içeri/dışarı aktarma, SSIS)](#sql-builtin-utilities)

### <a name="insert-tables-bcp"></a>Komut satırı toplu kopyalama yardımcı programı (BCP)
BCP ile SQL Server yüklü bir komut satırı yardımcı programıdır ve verilerini taşımak için hızlı yollarından biridir. Bu, üç SQL Server varyantları (Şirket içi SQL Server, SQL Azure ve SQL Server VM Azure 'da) üzerinde çalışmaktadır.

> [!NOTE]
> **Veri BCP için nerede olacak?**  
> Gerekli olmamasına karşın, hedef SQL Server ile aynı makinede bulunan kaynak verileri içeren dosyalarını daha hızlı aktarımları için (ağ hızı vs yerel disk g/ç hızı) sağlar. Veri içeren düz dosyaları, [AzCopy](../../storage/common/storage-use-azcopy.md), [Azure Depolama Gezgini](https://storageexplorer.com/) veya Windows kopyalama/yapıştırma gibi çeşitli dosya kopyalama araçlarını Uzak Masaüstü Protokolü (RDP) kullanarak taşıyabilirsiniz SQL Server.
>
>

1. Hedef SQL Server veritabanında veritabanı ve tabloları oluşturduğunuzdan emin olun. `Create Database` ve `Create Table` komutlarını kullanarak nasıl yapılacağını gösteren bir örnek aşağıda verilmiştir:

    ```sql
    CREATE DATABASE <database_name>
    
    CREATE TABLE <tablename>
    (
        <columnname1> <datatype> <constraint>,
        <columnname2> <datatype> <constraint>,
        <columnname3> <datatype> <constraint>
    )
    ```

1. BCP 'nin yüklü olduğu makinenin komut satırından aşağıdaki komutu vererek tablo şemasını açıklayan biçim dosyasını oluşturun.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n`
1. Aynı makineye SQL Server yüklendiğinde komut satırından çalışması gereken bcp komutunu kullanarak verileri veritabanına ekleyin:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attempt -t \t -r \n`

> **Bcp eklemeleri Iyileştiriliyor** Bu tür eklemeleri iyileştirmek için lütfen aşağıdaki makaleye [toplu Içeri aktarmayı iyileştirmek Için yönergelere](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) bakın.
>
>

### <a name="insert-tables-bulkquery-parallel"></a>Daha hızlı veri hareketi için paralelleştirme eklemeleri
Taşıdığınız veriler büyükse, bir PowerShell betiğinden paralel olarak birden çok BCP komutunu aynı anda yürüten şeyleri hızlandırabilirsiniz.

> [!NOTE]
> **Büyük veri** alımı Büyük ve çok büyük veri kümelerinde veri yüklemeyi iyileştirmek için, mantıksal ve fiziksel veritabanı tablolarınızı birden çok dosya grubu ve bölüm tablosu kullanarak bölümleyin. Bölüm tablolarına veri oluşturma ve yükleme hakkında daha fazla bilgi için bkz. [paralel yük SQL Bölüm tabloları](parallel-load-sql-partitioned-tables.md).
>
>

Aşağıdaki örnek PowerShell betiği bcp kullanarak paralel eklemeleri gösterir:

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

### <a name="insert-tables-bulkquery"></a>SQL sorgusunu toplu ekleme
[Toplu ekleme SQL sorgusu](https://msdn.microsoft.com/library/ms188365) , satır/sütun tabanlı dosyalardan veritabanına veri aktarmak için kullanılabilir (desteklenen türler,[Toplu dışa aktarma veya içeri aktarma (SQL Server) için verileri hazırlama](https://msdn.microsoft.com/library/ms188609)bölümünde ele alınmıştır.

Toplu ekleme için bazı örnek komutlar şunlardır aşağıda gösterildiği gibi şunlardır:  

1. Verilerinizi analiz edin ve SQL Server veritabanı herhangi bir özel alanın tarihler gibi aynı biçimde varsayar emin olmak için içeri aktarmadan önce tüm özel seçenekleri belirleyin. (Verilerinizi yıl ay gün biçimine tarihi içeriyorsa) tarih biçimini yıl ay-günlük ayarlamak nasıl bir örnek aşağıda verilmiştir:

    ```sql
    SET DATEFORMAT ymd;
    ```
2. Toplu içeri aktarma deyimlerini kullanarak verileri içeri aktarın:

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

### <a name="sql-builtin-utilities"></a>SQL Server yerleşik yardımcı programlar
SQL Server Integration Services (SSIS) kullanarak verileri Azure üzerinde SQL Server VM bir düz dosyadan içeri aktarabilirsiniz.
SSIS iki studio ortamlarında kullanılabilir. Ayrıntılar için bkz. [Integration Services (SSIS) ve Studio ortamları](https://technet.microsoft.com/library/ms140028.aspx):

* SQL Server Veri Araçları Ayrıntılar için bkz. [Microsoft SQL Server veri araçları](https://msdn.microsoft.com/data/tools.aspx)  
* Içeri/dışarı aktarma Sihirbazı hakkında daha fazla bilgi için bkz. [SQL Server içeri ve dışarı aktarma Sihirbazı](https://msdn.microsoft.com/library/ms141209.aspx)

## <a name="sqlonprem_to_sqlonazurevm"></a>Verileri şirket içi SQL Server Azure sanal makinesine SQL Server taşıma
Aşağıdaki geçiş stratejileri de kullanabilirsiniz:

1. [Microsoft Azure VM sihirbazına SQL Server veritabanı dağıtma](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Düz dosyaya aktar](#export-flat-file)
3. [SQL veritabanı geçiş Sihirbazı](#sql-migration)
4. [Veritabanı yedekleme ve geri yükleme](#sql-backup)

Aşağıdaki seçeneklerin her birini aşağıda anladık:

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Bir Microsoft Azure VM Sihirbazı için bir SQL Server veritabanı dağıtma
**SQL Server veritabanını MICROSOFT Azure VM 'ye Dağıtma Sihirbazı** , verileri şirket içi SQL Server örneğinden BIR Azure sanal makinesinde SQL Server taşımak için basit ve önerilen bir yoldur. Ayrıntılı adımlar ve diğer alternatiflere ilişkin bir açıklama için bkz. bir [veritabanını Azure VM 'de SQL Server geçirme](../../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).

### <a name="export-flat-file"></a>Düz dosyaya aktar
[SQL Server verileri toplu Içe aktarma ve dışarı aktarma](https://msdn.microsoft.com/library/ms175937.aspx) konusunun belgelendiği gibi şirket içi SQL Server verileri toplu olarak dışarı aktarmak için çeşitli yöntemler kullanılabilir. Bu belge, örnek olarak toplu kopyalama programı (BCP) kapsar. Veri düz bir dosyaya dışarı aktarıldıktan sonra başka bir SQL server kullanarak toplu içeri aktarılabilir.

1. Verileri şirket içi SQL Server'dan bcp yardımcı programını kullanarak aşağıdaki gibi bir dosyaya aktarın.

    `bcp dbname..tablename out datafile.tsv -S    servername\sqlinstancename -T -t \t -t \n -c`
2. Adım 1 ' de bulunan tablo şeması için `create database` ve `create table` kullanarak Azure 'da veritabanı ve tablo oluşturun SQL Server VM.
3. Dışarı/içeri aktarılan verilerin tablo şemasını tanımlamak için bir biçim dosyası oluşturun. Biçim dosyasının ayrıntıları, [Biçim dosyası oluşturma (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx)bölümünde açıklanmaktadır.

    Biçim dosyası oluşturma SQL Server makine BCP çalışırken

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n

    Biçim dosyası oluşturma çalışırken BCP uzaktan karşı bir SQL Server

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
4. Verileri düz dosyalardaki bir SQL Server taşımak için [dosya kaynağından verileri taşıma](#filesource_to_sqlonazurevm) bölümünde açıklanan yöntemlerden herhangi birini kullanın.

### <a name="sql-migration"></a>SQL veritabanı geçiş Sihirbazı
[SQL Server veritabanı geçiş Sihirbazı](https://sqlazuremw.codeplex.com/) , VERILERI iki SQL Server örneği arasında taşımak için kolay bir yol sağlar. Bu sütun türlerini ve diğer çeşitli İşlevler'i seçin, kaynakları ve hedef tablolar arasında veri şemasını harita izin verir. Bu işlem arka planda altında toplu kopyalama (BCP) kullanır. SQL veritabanı Geçiş Sihirbazı Hoş Geldiniz ekranının ekran görüntüsü aşağıda gösterilmiştir.  

![SQL Server Yükseltme Sihirbazı][2]

### <a name="sql-backup"></a>Veritabanı yedekleme ve geri yükleme
SQL Server'ı destekler:

1. [Veritabanı yedekleme ve geri yükleme işlevselliği](https://msdn.microsoft.com/library/ms187048.aspx) (hem yerel bir dosya ya da bacpac blob 'a dışarı aktarma) ve [veri katmanı uygulamaları](https://msdn.microsoft.com/library/ee210546.aspx) (bacpac kullanılarak).
2. Doğrudan kopyalanmış veritabanı veya varolan bir SQL Azure veritabanına kopyalama Azure'da SQL Server Vm'leri oluşturma olanağı. Daha fazla bilgi için bkz. [veritabanını kopyalama Sihirbazı 'Nı kullanma](https://msdn.microsoft.com/library/ms188664.aspx).

Veritabanı geri görüntüsü yukarı/geri yükleme SQL Server Management Studio seçenekleri aşağıda gösterilmiştir.

![SQL Server içeri aktarma aracı][1]

## <a name="resources"></a>Kaynaklar
[Azure VM 'de bir veritabanını SQL Server geçirme](../../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md)

[Azure Sanal Makineler’de SQL Server’a genel bakış](../../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/move-sql-server-virtual-machine/database_migration_wizard.png
