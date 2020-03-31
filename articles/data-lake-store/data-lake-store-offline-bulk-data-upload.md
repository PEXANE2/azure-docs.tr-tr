---
title: Azure Veri Gölü Depolama Gen1'e büyük veri kümesi ni yükleyin - çevrimdışı yöntemler
description: Azure Blob depolamadan Azure Veri Gölü Depolama Gen1'e veri kopyalamak için İçe Alma/Dışa Aktarma hizmetini kullanın
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: aa3eb0bcd9ddd2a094563efe326f7af7e9e8708a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73839302"
---
# <a name="use-the-azure-importexport-service-for-offline-copy-of-data-to-data-lake-storage-gen1"></a>Verilerin çevrimdışı kopyalanması için Azure İçe Alma/Dışa Aktarma hizmetini kullanın Veri Gölü Depolama Gen1

Bu makalede, [Azure İçe Alma/Dışa Aktarma hizmeti](../storage/common/storage-import-export-service.md)gibi çevrimdışı kopyalama yöntemlerini kullanarak büyük veri kümelerini (>200 GB) Veri Gölü Depolama Gen1'e kopyalamayı öğreneceksiniz. Özellikle, bu makalede örnek olarak kullanılan dosya 339.420.860.416 bayt veya diskte yaklaşık 319 GB'dır. Bu dosyaya 319GB.tsv diyelim.

Azure İçe Alma/Dışa Aktarma hizmeti, sabit disk sürücülerini bir Azure veri merkezine göndererek büyük miktarda veriyi Azure Blob depolamaalanına daha güvenli bir şekilde aktarmanıza yardımcı olur.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakileri almalısınız:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure depolama hesabı.**
* **Bir Azure Veri Gölü Depolama Gen1 hesabı.** Nasıl oluşturulacağına ilişkin talimatlar için [bkz.](data-lake-store-get-started-portal.md)

## <a name="prepare-the-data"></a>Verileri hazırlama

İçe/Dışa Aktar hizmetini kullanmadan önce, **200 GB'dan küçük kopyalara** aktarılacak veri dosyasını kırın. Alma aracı 200 GB'dan büyük dosyalarla çalışmaz. Bu makalede, dosyayı her biri 100 GB'lık parçalara böleriz. Bunu [Cygwin'i](https://cygwin.com/install.html)kullanarak yapabilirsiniz. Cygwin Linux komutlarını destekler. Bu durumda, aşağıdaki komutu kullanın:

    split -b 100m 319GB.tsv

Bölme işlemi aşağıdaki adlara sahip dosyalar oluşturur.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Verilerle diskleri hazırlayın

Sabit disklerinizi hazırlamak için [Azure İçe Alma/Dışa Aktarma hizmetini](../storage/common/storage-import-export-service.md) kullanma **(sürücülerinizi hazırla** bölümünde) yönergeleri izleyin. İşte genel sırası:

1. Azure İçe Alma/Dışa Aktarma hizmeti için kullanılacak gereksinimi karşılayan bir sabit disk satın alın.
2. Verilerin Azure veri merkezine gönderildikten sonra kopyalanacağı bir Azure depolama hesabı tanımlayın.
3. Komut satırı yardımcı programı olan [Azure İçe Aktarma/Dışa Aktarma Aracı'nı](https://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409)kullanın. Burada, aracın nasıl kullanılacağını gösteren bir örnek parçacık vereyim.

    ```
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ```
    Bkz. Daha fazla örnek parçacık için [Azure İçe Aktarma/Verme hizmetini kullanma.](../storage/common/storage-import-export-service.md)
4. Önceki komut, belirtilen konumda bir günlük dosyası oluşturur. [Azure portalından](https://portal.azure.com)bir alma işi oluşturmak için bu günlük dosyasını kullanın.

## <a name="create-an-import-job"></a>Alma işi oluşturma

Artık [Azure İçe Aktarma/Dışa Aktarma hizmetini kullanma](../storage/common/storage-import-export-service.md) yönergelerini kullanarak **(İçe Alma iş** bölümünü oluştur altında) bir alma işi oluşturabilirsiniz. Bu alma işi için, diğer ayrıntılarla birlikte, disk sürücülerini hazırlarken oluşturulan günlük dosyasını da sağlar.

## <a name="physically-ship-the-disks"></a>Diskleri fiziksel olarak gönderme

Artık diskleri fiziksel olarak bir Azure veri merkezine taşıyabilirsiniz. Burada, veriler alma işini oluştururken sağladığınız Azure Depolama bloblarına kopyalanır. Ayrıca, işi oluştururken, izleme bilgilerini daha sonra sağlamayı seçtiyseniz, artık alma işinize geri dönebilir ve izleme numarasını güncelleştirebilirsiniz.

## <a name="copy-data-from-blobs-to-data-lake-storage-gen1"></a>Verileri lekelerden Veri Gölü Depolama Gen1'e kopyalama

Alma işinin durumu tamamlandığını gösterdiğinde, verilerin belirttiğiniz Azure Depolama bloblarında kullanılabilir olup olmadığını doğrulayabilirsiniz. Daha sonra, bu verileri lekelerden Azure Veri Gölü Depolama Gen1'e taşımak için çeşitli yöntemler kullanabilirsiniz. Veri yüklemek için mevcut tüm seçenekler için, [Veri Gölü Depolama Gen1'e veri alma](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-storage-gen1)bilgisine bakın.

Bu bölümde, verileri kopyalamak için bir Azure Veri Fabrikası ardışık hattı oluşturmak için kullanabileceğiniz JSON tanımlarını size salıyoruz. Bu JSON tanımlarını Azure [portalından](../data-factory/tutorial-copy-data-portal.md) veya [Visual Studio'dan](../data-factory/tutorial-copy-data-dot-net.md)kullanabilirsiniz.

### <a name="source-linked-service-azure-storage-blob"></a>Kaynak bağlantılı hizmet (Azure Depolama blob)

```JSON
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="target-linked-service-data-lake-storage-gen1"></a>Hedef bağlantılı hizmet (Veri Gölü Depolama Gen1)

```JSON
{
    "name": "AzureDataLakeStorageGen1LinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Storage Gen1 account with your access rights>",
            "dataLakeStoreUri": "https://<adlsg1_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
```

### <a name="input-data-set"></a>Giriş veri seti

```JSON
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

### <a name="output-data-set"></a>Çıktı veri seti

```JSON
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStorageGen1LinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
```

### <a name="pipeline-copy-activity"></a>Boru hattı (kopyalama etkinliği)

```JSON
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
```

Daha fazla bilgi için bkz. Azure [Veri Fabrikası'nı kullanarak Azure Depolama buzundaki verileri Azure Veri Gölü Depolama Gen1'e taşı.](../data-factory/connector-azure-data-lake-store.md)

## <a name="reconstruct-the-data-files-in-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1'deki veri dosyalarını yeniden oluşturma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

319 GB'lık bir dosyayla başladık ve Azure İçe Alma/Dışa Aktarma hizmetini kullanarak aktarılabilmek için daha küçük boyutlu dosyalara ayırdık. Veriler Azure Veri Gölü Depolama Gen1'de olduğuna göre, dosyayı özgün boyutuna göre yeniden oluşturabiliriz. Bunu yapmak için aşağıdaki Azure PowerShell cmdlets'i kullanabilirsiniz.

```PowerShell
# Login to our account
Connect-AzAccount

# List your subscriptions
Get-AzSubscription

# Switch to the subscription you want to work with
Set-AzContext -SubscriptionId
Register-AzResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzDataLakeStoreItem -AccountName "<adlsg1_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv"
```

## <a name="next-steps"></a>Sonraki adımlar

* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Veri Gölü Depolama Gen1 ile Azure Veri Gölü Analizini Kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Veri Gölü Depolama Gen1 ile Azure HDInsight'ı kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)
