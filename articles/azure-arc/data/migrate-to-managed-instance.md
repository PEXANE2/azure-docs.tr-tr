---
title: SQL Server bir veritabanını Azure Arc etkin SQL yönetilen örneği 'ne geçirme
description: SQL Server veritabanını Azure Arc etkin SQL yönetilen örneği 'ne geçirme
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: vin-yu
ms.author: vinsonyu
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 86563b0a44bade2cedaf76af3c247821756111fe
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941759"
---
# <a name="migrate-sql-server-to-azure-arc-enabled-sql-managed-instance"></a>Geçirme: Azure Arc etkin SQL yönetilen örneği 'ne SQL Server

Bu senaryo, iki farklı yedekleme ve geri yükleme yöntemi aracılığıyla bir veritabanını SQL Server örneğinden Azure SQL yönetilen örneği 'ne geçirme adımlarında size yol gösterir.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="use-azure-blob-storage"></a>Azure Blob depolamayı kullanma 

Azure Arc etkin SQL yönetilen örneği 'ne geçiş için Azure Blob Storage 'ı kullanın.

Bu yöntem, Azure Blob Storage 'ı yedekleyebilir ve sonra geri yükleme yapmak için geçici bir depolama konumu olarak kullanır.

### <a name="prerequisites"></a>Önkoşullar

- [Azure Data Studio yüklensin](install-client-tools.md)
- [Azure Depolama Gezgini yüklensin](https://azure.microsoft.com/features/storage-explorer/)
- Azure aboneliği

### <a name="step-1-provision-azure-blob-storage"></a>1. Adım: Azure Blob depolamayı sağlama

1. [Azure Blob depolama hesabı oluşturma](../../storage/blobs/storage-blob-create-account-block-blob.md?tabs=azure-portal) bölümünde açıklanan adımları izleyin
1. Azure Depolama Gezgini Başlat
1. Önceki adımda oluşturulan BLOB depolama alanına erişmek için [Azure 'Da oturum açın](../../vs-azure-tools-storage-manage-with-storage-explorer.md?tabs=windows#sign-in-to-azure)
1. BLOB depolama hesabına sağ tıklayın ve yedekleme dosyasının depolanacağı yeni bir kapsayıcı oluşturmak için **BLOB kapsayıcısı oluştur** ' u seçin.

### <a name="step-2-get-storage-blob-credentials"></a>2. Adım: Depolama Blobu kimlik bilgilerini alın

1. Azure Depolama Gezgini, yeni oluşturulan blob kapsayıcısına sağ tıklayın ve **paylaşılan erişim Imzasını al** ' ı seçin.

1. **Okuma**, **yazma** ve **Listeyi** seçin

1. **Oluştur**’u seçin

   Bu ekrandan URI ve sorgu dizesini de göz önünde yararlanın. Bunlar sonraki adımlarda gerekecektir. Not Defteri/OneNote vb. kaydetmek için **Kopyala** düğmesine tıklayın.

1. **Paylaşılan erişim imzası** penceresini kapatın.

### <a name="step-3-backup-database-file-to-azure-blob-storage"></a>3. Adım: veritabanı dosyasını Azure Blob depolama alanına yedekleme

Bu adımda, kaynak SQL Server bağlanacağız ve SQL yönetilen örneği-Azure yaya geçirmek istediğimiz veritabanının yedek dosyasını oluşturacağız.

1. Azure Data Studio Başlat
1. SQL yönetilen örneği-Azure yaya geçirmek istediğiniz veritabanına sahip SQL Server örneğine bağlanın
1. Veritabanına sağ tıklayın ve **Yeni sorgu** ' yı seçin.
1. Aşağıdaki biçimde, `<...>` daha önceki adımlarda paylaşılan erişim imzasında bulunan bilgileri kullanarak belirtilen yer tutucuları değiştirerek sorgunuzu hazırlayın.  Değerleri yerine girdikten sonra sorguyu çalıştırın.

   ```sql
   IF NOT EXISTS  
   (SELECT * FROM sys.credentials
   WHERE name = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>')  
   CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
     WITH IDENTITY = 'SHARED ACCESS SIGNATURE',  
      SECRET = '<SAS_TOKEN>';  
   ```

1. Benzer şekilde, blob kapsayıcısına bir yedekleme dosyası oluşturmak için **Backup Database** komutunu aşağıdaki şekilde hazırlayın.  Değerleri yerine girdikten sonra sorguyu çalıştırın.

   ```sql
   BACKUP DATABASE <database name> TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>'
   ```

1. Azure Depolama Gezgini açın ve önceki adımda oluşturulan yedekleme dosyasının blob kapsayıcısında görünür olduğunu doğrulayın

### <a name="step-4-restore-the-database-from-azure-blob-storage-to-sql-managed-instance---azure-arc"></a>4. Adım: veritabanını Azure Blob depolama 'dan SQL yönetilen örneği-Azure yaya geri yükleme

1. Azure Data Studio, oturum açın ve SQL yönetilen örneği-Azure yaya bağlanın.
1. **Sistem veritabanlarını**genişletin, **ana** veritabanına sağ tıklayın ve **Yeni sorgu**' yı seçin.
1. Sorgu Düzenleyicisi penceresinde, kimlik bilgilerini oluşturmak için önceki adımdan aynı sorguyu hazırlayın ve çalıştırın.

   ```sql
   IF NOT EXISTS  
   (SELECT * FROM sys.credentials
   WHERE name = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>')  
   CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
     WITH IDENTITY = 'SHARED ACCESS SIGNATURE',  
     SECRET = '<SAS_TOKEN>';  
   ```

1. Yedekleme dosyasının okunabilir olduğunu ve bozulmadan emin olmak için aşağıdaki komutu hazırlayın ve çalıştırın.

   ```console
   RESTORE FILELISTONLY FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<file name>.bak'
   ```

1. Yedekleme dosyasını SQL yönetilen örneğindeki bir veritabanına geri yüklemek için **veritabanını geri yükle** komutunu aşağıdaki şekilde hazırlayın ve çalıştırın-Azure Arc

   ```sql
   RESTORE DATABASE <database name> FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<file name>'
   WITH MOVE 'Test' to '/var/opt/mssql/data/<file name>.mdf'
   ,MOVE 'Test_log' to '/var/opt/mssql/data/<file name>.ldf'
   ,RECOVERY  
   ,REPLACE  
   ,STATS = 5;  
   GO
   ```

URL 'ye yedekleme hakkında daha fazla bilgi edinin:

[URL belgelerine yedekleme](/sql/relational-databases/backup-restore/sql-server-backup-to-url)

[SQL Server Management Studio (SSMS) kullanarak URL 'ye yedekleme](/sql/relational-databases/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service)

-------

## <a name="method-2-copy-the-backup-file-into-an-azure-sql-managed-instance---azure-arc-pod-using-kubectl"></a>Yöntem 2: yedekleme dosyasını Azure SQL yönetilen örneği 'ne kopyalama-kubectl kullanarak Azure Arc Pod

Bu yöntem, herhangi bir yöntem aracılığıyla oluşturduğunuz bir yedekleme dosyasını nasıl kullanacağınızı ve sonra Windows veya Linux 'ta tipik bir dosya sisteminde yaptığınız gibi daha fazla geri yükleme yapabilmeniz için Azure SQL yönetilen örneği Pod içindeki yerel depolama alanına kopyalamanız gerektiğini gösterir. Bu senaryoda, `kubectl cp` dosyayı Pod 'un dosya sistemine bir yerden kopyalamak için komutunu kullanacaksınız.

### <a name="prerequisites"></a>Önkoşullar

- Kubectl 'yi, Azure Arc veri Hizmetleri 'nin dağıtıldığı Kubernetes kümenizi işaret etmek üzere yükleyip yapılandırın
- Azure Data Studio veya SQL Server Management Server 'ın yüklü olduğu ve yedek dosyayı oluşturmak istediğiniz SQL Server bağlı olduğundan ya da yerel dosya sisteminizde zaten oluşturulmuş bir. bak dosyasının olması gibi bir araç vardır.

### <a name="step-1-backup-the-database-if-you-havent-already"></a>1. Adım: henüz yapmadıysanız veritabanını yedekleyin

SQL Server veritabanını, tüm normal SQL Server yedeklemesi gibi yerel dosya yolunuza yedekleyin:

 ```sql
BACKUP DATABASE Test
TO DISK = 'c:\tmp\test.bak'
WITH FORMAT, MEDIANAME = 'Test’ ;
GO
```

### <a name="step-2-copy-the-backup-file-into-the-pods-file-system"></a>2. Adım: yedekleme dosyasını Pod 'un dosya sistemine kopyalama

SQL örneğinin dağıtıldığı Pod 'ın adını bulun. Genellikle şöyle görünmelidir `pod/<sqlinstancename>-0`

Şunu çalıştırarak tüm yığınların listesini alın:

 ```console
kubectl get pods -n <namespace of data controller>
```

Örnek:

Yedek dosyayı yerel depolamadan kümedeki SQL Pod 'e kopyalayın.

 ```console
kubectl cp <source file location> <pod name>:var/opt/mssql/data/<file name> -n <namespace name>

#Example:
kubectl cp C:\Backupfiles\test.bak sqlinstance1-0:var/opt/mssql/data/test.bak -n arc
```

### <a name="step-3-restore-the-database"></a>3. Adım: veritabanını geri yükleme

Yedekleme dosyasını Azure SQL yönetilen örneği-Azure yaya geri yüklemek için RESTORE komutunu hazırlayın ve çalıştırın

```sql
RESTORE DATABASE test FROM DISK = '/var/opt/mssql/data/<file name>.bak'
WITH MOVE '<database name>' to '/var/opt/mssql/data/<file name>.mdf'  
,MOVE '<database name>' to '/var/opt/mssql/data/<file name>_log.ldf'  
,RECOVERY  
,REPLACE  
,STATS = 5;  
GO
```

Örnek:

```sql
RESTORE DATABASE test FROM DISK = '/var/opt/mssql/data/test.bak'
WITH MOVE 'test' to '/var/opt/mssql/data/test.mdf'  
,MOVE 'test' to '/var/opt/mssql/data/test_log.ldf'  
,RECOVERY  
,REPLACE  
,STATS = 5;  
GO
```


## <a name="next-steps"></a>Sonraki adımlar

[Azure Arc etkin SQL yönetilen örneği özellikleri ve özellikleri hakkında daha fazla bilgi edinin](managed-instance-features.md)

[Veri denetleyicisi oluşturarak başlayın](create-data-controller.md)

[Zaten bir veri denetleyicisi oluşturulmuş mı? Azure Arc etkin SQL yönetilen örneği oluşturma](create-sql-managed-instance.md)