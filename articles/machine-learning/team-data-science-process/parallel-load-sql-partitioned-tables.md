---
title: SQL bölme tablolarında paralel toplu veri alma - Takım Veri Bilimi Süreci
description: Sql Server veritabanına hızlı paralel toplu veri aktarım için bölümlenmiş tablolar oluşturun.
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
ms.openlocfilehash: 673a801e218d055bf482dc97972e36584cddd402
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721345"
---
# <a name="build-and-optimize-tables-for-fast-parallel-import-of-data-into-a-sql-server-on-an-azure-vm"></a>Azure VM'de sql sunucusuna hızlı paralel veri aktarımı için tablolar oluşturma ve optimize edin

Bu makalede, bir SQL Server veritabanına veri hızlı paralel toplu alma için bölümlenmiş tablolar oluşturmak için nasıl açıklanır. Büyük veri yükleme/sql veritabanına aktarımı için, SQL DB'ye veri alma ve sonraki sorgular *Bölümlenmiş Tablolar ve Görünümler*kullanılarak geliştirilebilir. 

## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Yeni bir veritabanı ve dosya grupları kümesi oluşturma
* Zaten yoksa, [yeni bir veritabanı oluşturun.](https://technet.microsoft.com/library/ms176061.aspx)
* Bölümlenmiş fiziksel dosyaları tutan veritabanı dosya gruplarını veritabanına ekleyin. 
* Veritabanı zaten varsa, bu create [DATABASE](https://technet.microsoft.com/library/ms176061.aspx) ile [yapılabilir.](https://msdn.microsoft.com/library/bb522682.aspx)
* Her veritabanı dosya grubuna bir veya daha fazla dosya (gerektiği gibi) ekleyin.
  
  > [!NOTE]
  > Bu bölüm için veri tutan hedef dosya grubunu ve dosya grubu verilerinin depolandığı fiziksel veritabanı dosya adını(lar) belirtin.
  > 
  > 

Aşağıdaki örnek, birincil ve günlük grupları dışında üç dosya grubundan içeren ve her birinde bir fiziksel dosya içeren yeni bir veritabanı oluşturur. Veritabanı dosyaları, SQL Server örneğinde yapılandırıldığı gibi varsayılan SQL Server Data klasöründe oluşturulur. Varsayılan dosya konumları hakkında daha fazla bilgi için Varsayılan [ve SQL Server'ın Adlandırılmış Örnekleri için Dosya Konumları'na](https://msdn.microsoft.com/library/ms143547.aspx)bakın.

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);

    EXECUTE ('
        CREATE DATABASE <database_name>
         ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup2'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup3'', FILENAME = ''' + @data_path + '<file_name_3>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ) 
         LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')

## <a name="create-a-partitioned-table"></a>Bölümlenmiş tablo oluşturma
Önceki adımda oluşturulan veritabanı dosya gruplarına eşlenen veri şemasına göre bölümlenmiş tablo(lar) oluşturmak için önce bir bölüm işlevi ve düzeni oluşturmanız gerekir. Veriler bölümlenmiş tablo(lar)a toplu olarak alındığında, kayıtlar aşağıda açıklandığı gibi, bir bölüm düzenine göre dosya grupları arasında dağıtılır.

### <a name="1-create-a-partition-function"></a>1. Bölüm işlevi oluşturma
[Bölüm işlevi oluşturma](https://msdn.microsoft.com/library/ms187802.aspx) Bu işlev, 2013 yılında bölümleri aya göre sınırlamak (bazı\_tarih alanı)\_gibi, her bir bölüm tablosuna eklenecek değerler/sınırlar aralığını tanımlar:
  
        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )

### <a name="2-create-a-partition-scheme"></a>2. Bir bölüm düzeni oluşturma
[Bir bölüm düzeni oluşturun.](https://msdn.microsoft.com/library/ms179854.aspx) Bu şema, bölüm işlevindeki her bölüm aralığını fiziksel bir dosya grubuyla eşler:
  
        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )
  
  Her bölümde işleve/düzene göre geçerli olan aralıkları doğrulamak için aşağıdaki sorguyu çalıştırın:
  
        SELECT psch.name as PartitionScheme,
            prng.value AS PartitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>

### <a name="3-create-a-partition-table"></a>3. Bölüm tablosu oluşturma
Veri şemanıza göre [bölümlenmiş tablo](https://msdn.microsoft.com/library/ms174979.aspx)(lar) oluşturun ve tabloyu bölmek için kullanılan bölümşemasını ve kısıtlama alanını belirtin, örneğin:
  
        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

Daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/ms188730.aspx)

## <a name="bulk-import-the-data-for-each-individual-partition-table"></a>Her bir bölüm tablosu için verileri toplu alma

* BCP, BULK INSERT veya SQL Server [Geçiş Sihirbazı](https://sqlazuremw.codeplex.com/)gibi diğer yöntemleri kullanabilirsiniz. Sağlanan örnek BCP yöntemini kullanır.
* [Örneğin,](https://msdn.microsoft.com/library/bb522682.aspx) günlüğe kaydetme nin ek yükü en aza indirmek için işlem günlüğe kaydetme düzenini BULK_LOGGED değiştirmek için veritabanını değiştirin:
  
        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED
* Veri yüklemesini hızlandırmak için toplu alma işlemlerini paralel olarak başlatın. Büyük verilerin SQL Server veritabanlarına toplu olarak aktarılmasını hızlandırmayla ilgili ipuçları için, [1 saatten kısa bir sürede Load 1 TB'ye](https://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx)bakın.

Aşağıdaki PowerShell komut dosyası, BCP kullanılarak paralel veri yüklemesi örneğidir.

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # The example assumes the partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes the input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0

    # For SQL authentication, set the server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match the number of input data files per table
    $numofparts = <number_of_partitions>

    # Set table name to be loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"

    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir

    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }

    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }

    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }

    Get-Job

    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a>Birleştirmeleri en iyi duruma getirmek ve performansı sorgulamak için dizinler oluşturma
* Birden çok tablodan modelleme için veri ayıklarsanız, birleştirme performansını artırmak için birleştirme anahtarlarında dizinler oluşturun.
* Her bölüm için aynı dosya grubunu hedefleyen [dizinler](https://technet.microsoft.com/library/ms188783.aspx) (kümelenmiş veya kümelenmemiş) oluşturun, örneğin:
  
        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  Veya
  
        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  
  > [!NOTE]
  > Verileri toplu olarak aktarmadan önce dizinleri oluşturmayı seçebilirsiniz. Toplu içe aktarmadan önce dizin oluşturma veri yüklemesini yavaşlatıyor.
  > 
  > 

## <a name="advanced-analytics-process-and-technology-in-action-example"></a>İş Başında Gelişmiş Analitik Süreci ve Teknolojisi Örneği
Ortak bir veri kümesiyle Takım Veri Bilimi İşlemi'ni kullanan uçtan uca bir gözden geçirme örneği için, [Team Data Science Process in Action: SQL Server'ı kullanma](sql-walkthrough.md)' ya bakın.

