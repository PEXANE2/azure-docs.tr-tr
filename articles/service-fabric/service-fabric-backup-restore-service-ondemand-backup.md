---
title: Azure Hizmet Kumaşı'nda isteğe bağlı yedekleme
description: Uygulama verilerinizi ihtiyac temelinde yedeklemek için Hizmet Dokusu'ndaki yedekleme ve geri yükleme özelliğini kullanın.
author: aagup
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: d5eada62bec49fe771373671e9438d2786d6b165
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75458427"
---
# <a name="on-demand-backup-in-azure-service-fabric"></a>Azure Hizmet Kumaşı'nda isteğe bağlı yedekleme

Olağanüstü durum veya veri kaybı senaryolarını gidermek için Güvenilir Durum hizmetleri ve Güvenilir Aktörler verilerini yedekleyebilirsiniz.

Azure Hizmet Kumaşı, [verilerin periyodik olarak yedeklemi](service-fabric-backuprestoreservice-quickstart-azurecluster.md) ve ihtiyaç bazında verilerin yedeklemi için özelliklere sahiptir. İsteğe bağlı yedekleme, temel hizmet veya ortamında planlanan değişiklikler_nedeniyle_ _veri kaybına_/karşı koruma yaptığı için yararlıdır.

İsteğe bağlı yedekleme özellikleri, bir hizmet veya hizmet ortamı işlemini el ile tetiklemeden önce hizmetlerin durumunu yakalamak için yararlıdır. Örneğin, hizmeti yükseltirken veya küçülterken hizmet ikililerinde bir değişiklik yaparsanız. Böyle bir durumda, isteğe bağlı yedekleme, uygulama kodu hataları tarafından bozulmaya karşı verileri korumaya yardımcı olabilir.
## <a name="prerequisites"></a>Ön koşullar

- Yapılandırma aramaları yapmak için Microsoft.ServiceFabric.Powershell.Http Modülü [Önizlemede] yükleyin.

```powershell
    Install-Module -Name Microsoft.ServiceFabric.Powershell.Http -AllowPrerelease
```

- Microsoft.ServiceFabric.Powershell.Http Module kullanarak herhangi bir yapılandırma isteği yapmadan önce Cluster'ın `Connect-SFCluster` komutu kullanarak bağlı olduğundan emin olun.

```powershell

    Connect-SFCluster -ConnectionEndpoint 'https://mysfcluster.southcentralus.cloudapp.azure.com:19080'   -X509Credential -FindType FindByThumbprint -FindValue '1b7ebe2174649c45474a4819dafae956712c31d3' -StoreLocation 'CurrentUser' -StoreName 'My' -ServerCertThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'  

```


## <a name="triggering-on-demand-backup"></a>İsteğe bağlı yedeklemeyi tetikleme

İsteğe bağlı yedekleme, yedekleme dosyalarını yüklemek için depolama ayrıntıları gerektirir. İsteğe bağlı yedekleme konumunu, periyodik yedekleme ilkesinde veya isteğe bağlı yedekleme isteğinde belirtirsiniz.

### <a name="on-demand-backup-to-storage-specified-by-a-periodic-backup-policy"></a>Periyodik yedekleme ilkesi tarafından belirtilen depolamaya isteğe bağlı yedekleme

Depolamaya ekstra isteğe bağlı yedekleme için Güvenilir Durum hizmetinin veya Güvenilir Aktörün bir bölümü kullanmak üzere periyodik yedekleme ilkesini yapılandırabilirsiniz.

Aşağıdaki durumda [Güvenilir Stateful hizmet ve Güvenilir Aktörler için periyodik yedekleme etkinleştirme](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors)senaryonun devamıdır. Bu durumda, bir yedekleme ilkesinin bir bölümü kullanmasını etkinleştirin ve yedekleme Azure Depolama'da belirli bir frekansta gerçekleşir.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell Microsoft.ServiceFabric.Powershell.Http Modülü kullanarak

```powershell

Backup-SFPartition -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22' 

```

#### <a name="rest-call-using-powershell"></a>Powershell kullanarak Dinlenme Çağrısı

Bölüm kimliği için isteğe bağlı yedekleme için tetikleyici yi ayarlamak `974bd92a-b395-4631-8a7f-53bd4ae9cf22`için [BackupPartition](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition) API'sını kullanın.

```powershell
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

[İsteğe bağlı yedekleme ilerlemesi](service-fabric-backup-restore-service-ondemand-backup.md#tracking-on-demand-backup-progress)için izlemeyi etkinleştirmek için [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress) API'sını kullanın.

### <a name="on-demand-backup-to-specified-storage"></a>Belirtilen depolama alanına isteğe bağlı yedekleme

Güvenilir Durum hizmetinin veya Güvenilir Aktörün bir bölümü için isteğe bağlı yedekleme isteyebilirsiniz. Depolama bilgilerini isteğe bağlı yedekleme isteğinin bir parçası olarak sağlayın.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell Microsoft.ServiceFabric.Powershell.Http Modülü kullanarak

```powershell

Backup-SFPartition -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22' -AzureBlobStore -ConnectionString  'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' -ContainerName 'backup-container'

```

#### <a name="rest-call-using-powershell"></a>Powershell kullanarak Dinlenme Çağrısı

Bölüm kimliği için isteğe bağlı yedekleme için tetikleyici yi ayarlamak `974bd92a-b395-4631-8a7f-53bd4ae9cf22`için [BackupPartition](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition) API'sını kullanın. Aşağıdaki Azure Depolama bilgilerini ekleyin:

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$OnDemandBackupRequest = @{
    BackupStorage = $StorageInfo
}

$body = (ConvertTo-Json $OnDemandBackupRequest)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

İsteğe [bağlı yedekleme ilerlemesi](service-fabric-backup-restore-service-ondemand-backup.md#tracking-on-demand-backup-progress)için izleme ayarlamak için [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress) API'sını kullanabilirsiniz.

### <a name="using-service-fabric-explorer"></a>Servis Kumaş Explorer kullanma
Gelişmiş Modu'nun Service Fabric Explorer ayarlarında etkinleştirildiğinden emin olun.
1. İstenilen bölümleri seçin ve Eylemler'e tıklayın. 
2. Tetikleyici Bölüm Yedekleme'yi seçin ve Azure için bilgileri doldurun:

    ![Tetikbölümü Yedekleme][0]

    veya FileShare:

    ![Tetik Bölümü Yedekleme FileShare][1]

## <a name="tracking-on-demand-backup-progress"></a>İsteğe bağlı yedekleme ilerlemesini izleme

Güvenilir Durum lu hizmetin veya Güvenilir Aktör'ün bölümü, aynı anda yalnızca bir isteğe bağlı yedekleme isteği kabul eder. Başka bir istek, yalnızca geçerli isteğe bağlı yedekleme isteği tamamlandıktan sonra kabul edilebilir.

Farklı bölümler aynı anda isteğe bağlı yedekleme isteklerini tetikleyebilir.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell Microsoft.ServiceFabric.Powershell.Http Modülü kullanarak

```powershell

Get-SFPartitionBackupProgress -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22'

```
#### <a name="rest-call-using-powershell"></a>Powershell kullanarak Dinlenme Çağrısı

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetBackupProgress?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3' 
$backupResponse = (ConvertFrom-Json $response.Content) 
$backupResponse
```

İsteğe bağlı yedekleme istekleri aşağıdaki durumlarda olabilir:

- **Kabul :** Yedekleme bölüm üzerinde başladı ve devam ediyor.
  ```
  BackupState             : Accepted
  TimeStampUtc            : 0001-01-01T00:00:00Z
  BackupId                : 00000000-0000-0000-0000-000000000000
  BackupLocation          :
  EpochOfLastBackupRecord :
  LsnOfLastBackupRecord   : 0
  FailureError            :
  ```
- **Başarı**, **Hata**, veya **Zaman Ayarı**: İstenen isteğe bağlı yedekleme aşağıdaki durumlardan herhangi birinde tamamlanabilir:
  - **Başarı**: _Başarı_ yedekleme durumu, bölüm durumunun başarıyla yedeklediğini gösterir. Yanıt UTC zaman ile birlikte bölüm için _BackupEpoch_ ve _BackupLSN_ sağlar.
    ```
    BackupState             : Success
    TimeStampUtc            : 2018-11-21T20:00:01Z
    BackupId                : 5d64b697-6acd-45a4-adbd-3d75e0078081
    BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-11-21 20.00.01.zip
    EpochOfLastBackupRecord : @{DataLossNumber=131873018908156893; ConfigurationNumber=8589934592}
    LsnOfLastBackupRecord   : 36
    FailureError            :
    ```
  - **Hata**: _Hata_ yedekleme durumu, bölümün durumunun yedeklemesi sırasında bir hata oluştuğunu gösterir. Başarısızlığın nedeni yanıt olarak belirtilir.
    ```
    BackupState             : Failure
    TimeStampUtc            : 0001-01-01T00:00:00Z
    BackupId                : 00000000-0000-0000-0000-000000000000
    BackupLocation          :
    EpochOfLastBackupRecord :
    LsnOfLastBackupRecord   : 0
    FailureError            : @{Code=FABRIC_E_BACKUPCOPIER_UNEXPECTED_ERROR; Message=An error occurred during this operation.  Please check the trace logs for more details.}
    ```
  - **Zaman Ara :** _Zaman Arası_ yedekleme durumu, bölüm durumu yedeklemesinin belirli bir süre içinde oluşturulamayacağını gösterir. Varsayılan zaman kaybı değeri 10 dakikadır. Bu senaryoda daha fazla [BackupTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout) ile yeni bir isteğe bağlı yedekleme isteği başlatın.
    ```
    BackupState             : Timeout
    TimeStampUtc            : 0001-01-01T00:00:00Z
    BackupId                : 00000000-0000-0000-0000-000000000000
    BackupLocation          :
    EpochOfLastBackupRecord :
    LsnOfLastBackupRecord   : 0
    FailureError            : @{Code=FABRIC_E_TIMEOUT; Message=The request of backup has timed out.}
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Periyodik yedekleme yapılandırması anlama](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [BackupGeri REST API başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/trigger-partition-backup.png
[1]: ./media/service-fabric-backuprestoreservice/trigger-backup-fileshare.png