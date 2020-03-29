---
title: Azure Hizmet Kumaşında yedeklemeyi geri alma
description: Uygulama verilerinizin yedeklerinden gelen verileri geri yüklemek için Hizmet Kumaşı'ndaki periyodik yedekleme ve geri yükleme özelliğini kullanın.
author: aagup
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: 1737102ee652cc2263bd0a908c1336bc93a6757b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75377914"
---
# <a name="restoring-backup-in-azure-service-fabric"></a>Azure Hizmet Kumaşında yedeklemeyi geri alma

Azure Hizmet Kumaşı'nda, Güvenilir Durum hizmetleri ve Güvenilir Aktörler, istek ve yanıt hareketi tamamlandıktan sonra mutable, yetkili bir durumu koruyabilir. Devlet hizmeti uzun bir süre için aşağı gidebilir veya bir felaket nedeniyle bilgi kaybedebilir. Bu durumda, hizmetin çalışmaya devam edebilmesi için en son kabul edilebilir yedeklemeden geri yüklenmeleri gerekir.

Örneğin, bir hizmeti, aşağıdaki senaryolara karşı korumak için verilerini yedeklemek üzere yapılandırabilirsiniz:

- **Olağanüstü durum kurtarma durumu**: Tüm Service Fabric kümesinin kalıcı kaybı.
- **Veri kaybı örneği**: Bir hizmet bölümünün yinelemelerinin çoğunluğunun kalıcı kaybı.
- **Veri kaybı durumu**: Hizmetin yanlışlıkla silinmesi veya bozulması. Örneğin, bir yönetici hizmeti yanlışlıkla siler.
- **Veri bozulması örneği**: Hizmetteki hatalar veri bozulmasına neden olur. Örneğin, bir hizmet kodu yükseltmesi hatalı verileri Güvenilir Bir Koleksiyona yazdığında veri bozulması oluşabilir. Böyle bir durumda, hem kodu hem de verileri önceki bir duruma geri yüklemeniz gerekebilir.

## <a name="prerequisites"></a>Ön koşullar

- Geri yüklemeyi tetiklemek için küme için _Hata Çözümleme Hizmeti 'nin (FAS)_ etkinleştirilmesi gerekir.
- _Yedekleme Geri Yükleme Hizmeti (BRS)_ yedeklemeyi oluşturdu.
- Geri yükleme yalnızca bir bölümde tetiklenebilir.
- Yapılandırma aramaları yapmak için Microsoft.ServiceFabric.Powershell.Http Modülü [Önizlemede] yükleyin.

```powershell
    Install-Module -Name Microsoft.ServiceFabric.Powershell.Http -AllowPrerelease
```

- Microsoft.ServiceFabric.Powershell.Http Module kullanarak herhangi bir yapılandırma isteği yapmadan önce Cluster'ın `Connect-SFCluster` komutu kullanarak bağlı olduğundan emin olun.

```powershell

    Connect-SFCluster -ConnectionEndpoint 'https://mysfcluster.southcentralus.cloudapp.azure.com:19080'   -X509Credential -FindType FindByThumbprint -FindValue '1b7ebe2174649c45474a4819dafae956712c31d3' -StoreLocation 'CurrentUser' -StoreName 'My' -ServerCertThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'  

```


## <a name="triggered-restore"></a>Tetiklenen geri yükleme

Aşağıdaki senaryolardan herhangi biri için geri yükleme tetiklenebilir:

- Olağanüstü durum kurtarma için veri geri _yükleme._
- _Veri bozulması/veri kaybı_için veri geri yükleme.

### <a name="data-restore-in-the-case-of-disaster-recovery"></a>Olağanüstü durum kurtarma durumunda veri geri yükleme

Tüm Service Fabric kümesi kaybolursa, Güvenilir Durum hizmeti ve Güvenilir Aktörler bölümleri için verileri kurtarabilirsiniz. Yedek [depolama ayrıntılarıyla GetBackupAPI'yi](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getbackupsfrombackuplocation)kullandığınızda istenen yedekleme listeden seçilebilir. Yedekleme numaralandırması bir uygulama, hizmet veya bölüm için olabilir.

Aşağıdaki örnekte, kayıp kümenin [Güvenilir Durum hizmeti ve Güvenilir Aktörler için periyodik yedeklemeyi etkinleştirmede](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors)atıfta bulunulan küme olduğunu varsayalım. Bu durumda, `SampleApp` yedekleme ilkesi etkinleştirilmiş olarak dağıtılır ve yedeklemeler Azure Depolama'ya yapılandırılır.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell Microsoft.ServiceFabric.Powershell.Http Modülü kullanarak

```powershell
Get-SFBackupsFromBackupLocation -Application -ApplicationName 'fabric:/SampleApp' -AzureBlobStore -ConnectionString 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' -ContainerName 'backup-container'

```

#### <a name="rest-call-using-powershell"></a>Powershell kullanarak Dinlenme Çağrısı

Uygulama içindeki tüm bölümler için oluşturulan yedeklerin listesini döndürmek için REST API'sını kullanmak için bir PowerShell komut dosyası çalıştırın. `SampleApp` API, kullanılabilir yedeklemeleri listelemek için yedek depolama bilgilerini gerektirir.

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$BackupEntity = @{
    EntityKind = 'Application'
    ApplicationName='fabric:/SampleApp'
}

$BackupLocationAndEntityInfo = @{
    Storage = $StorageInfo
    BackupEntity = $BackupEntity
}

$body = (ConvertTo-Json $BackupLocationAndEntityInfo)
$url = "https://myalternatesfcluster.southcentralus.cloudapp.azure.com:19080/BackupRestore/$/GetBackups?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
$BackupPoints = (ConvertFrom-Json $response.Content)
$BackupPoints.Items
```

Yukarıdaki çalışma için örnek çıktı:

```
BackupId                : b9577400-1131-4f88-b309-2bb1e943322c
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 20.55.16.zip
BackupType              : Full
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3334
CreationTimeUtc         : 2018-04-06T20:55:16Z
FailureError            : 
*
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            :
*
BackupId                : 69436834-c810-4163-9386-a7a800f78359
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.25.36.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3764
CreationTimeUtc         : 2018-04-06T21:25:36Z
FailureError            :
```

Geri yüklemeyi tetiklemek için yedeklerden birini seçin. Örneğin, olağanüstü durum kurtarma için geçerli yedekleme aşağıdaki yedekleme olabilir:

```
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            :
```

Geri yükleme API'si için _BackupId_ ve _BackupLocation_ ayrıntılarını sağlamanız gerekir.

Ayrıca, [bölüm düzeninde](service-fabric-concepts-partitioning.md#get-started-with-partitioning)ayrıntılı olarak alternatif kümede bir hedef bölüm seçmeniz gerekir. Alternatif küme yedeklemesi, özgün kayıp kümeden bölüm düzeninde belirtilen bölüme geri yüklenir.

Alternatif kümedeki bölümleme `1c42c47f-439e-4e09-98b9-88b8f60800c6`kimliği ise, `974bd92a-b395-4631-8a7f-53bd4ae9cf22` _Aralıklı Bölümleme (UniformInt64Partition)_ için yüksek anahtar ve düşük tuşu karşılaştırarak orijinal küme bölümleme kimliğiyle eşleyebilirsiniz.

_Adlandırılmış Bölümleme_için ad değeri, alternatif kümedeki hedef bölümü tanımlamak için karşılaştırılır.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell Microsoft.ServiceFabric.Powershell.Http Modülü kullanarak

```powershell

Restore-SFPartition  -PartitionId '1c42c47f-439e-4e09-98b9-88b8f60800c6' -BackupId 'b0035075-b327-41a5-a58f-3ea94b68faa4' -BackupLocation 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip' -AzureBlobStore -ConnectionString 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' -ContainerName 'backup-container'

```

#### <a name="rest-call-using-powershell"></a>Powershell kullanarak Dinlenme Çağrısı

Aşağıdaki [Geri Yükleme API'sini](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-restorepartition)kullanarak yedekleme küme bölümüne karşı geri yükleme isteğinde bulunun:

```powershell

$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$RestorePartitionReference = @{
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4'
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip'
    BackupStorage  = $StorageInfo
}

$body = (ConvertTo-Json $RestorePartitionReference) 
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/1c42c47f-439e-4e09-98b9-88b8f60800c6/$/Restore?api-version=6.4" 

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

TrackRestoreProgress ile geri yüklemenin ilerlemesini izleyebilirsiniz.

### <a name="using-service-fabric-explorer"></a>Servis Kumaş Explorer kullanma
Service Fabric Explorer'dan geri yüklemeyi tetikleyebilirsiniz. Gelişmiş Modu'nun Service Fabric Explorer ayarlarında etkinleştirildiğinden emin olun.
1. İstenilen bölümleri seçin ve Eylemler'e tıklayın. 
2. Tetikleyici Bölüm Geri Yükleme'yi seçin ve Azure için bilgileri doldurun:

    ![Tetik Bölümü Geri Yükleme][2]

    veya FileShare:

    ![Tetikleyici Bölüm Geri Yükleme Fileshare][3]

### <a name="data-restore-for-_data-corruption__data-loss_"></a>_Veri bozulması_/_veri kaybı_ için veri geri yükleme

_Veri kaybı_ veya _veri bozulması_için, Güvenilir Stateful hizmeti ve Güvenilir Aktörler bölümleri için yedeklenmiş bölümler seçilen yedeklerden herhangi biri geri yüklenebilir.

Aşağıdaki örnek, Güvenilir [Stateful hizmeti ve Güvenilir Aktörler için periyodik yedeklemeyi etkinleştirmenin](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors)bir devamıdır. Bu örnekte, bölüm için bir yedekleme ilkesi etkinleştirilir ve hizmet Azure Depolama'da istenilen sıklıkta yedekleme yapıyor.

[GetBackupAPI](service-fabric-backuprestoreservice-quickstart-azurecluster.md#list-backups)çıktısından bir yedekleme seçin. Bu senaryoda, yedekleme öncekiyle aynı kümeden oluşturulur.

Geri yüklemeyi tetiklemek için listeden bir yedek seçin. Geçerli _veri kaybı_/_veri bozulması_için aşağıdaki yedeklemeyi seçin:

```
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            :
```

Geri yükleme API'si için _BackupId_ ve _BackupLocation_ ayrıntılarını sağlayın. Hizmet Dokusu _Yedekleme Geri Yükleme Hizmeti (BRS)_ ilişkili yedekleme ilkesinden doğru depolama konumunu tanımladığı için küme yedekleme özelliğine sahiptir.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell Microsoft.ServiceFabric.Powershell.Http Modülü kullanarak

```powershell
Restore-SFPartition  -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22' -BackupId 'b0035075-b327-41a5-a58f-3ea94b68faa4' -BackupLocation 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip'

```

#### <a name="rest-call-using-powershell"></a>Powershell kullanarak Dinlenme Çağrısı

```powershell
$RestorePartitionReference = @{
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4',
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip'
}

$body = (ConvertTo-Json $RestorePartitionReference)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Restore?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

TrackRestoreProgress'i kullanarak geri yükleme ilerlemesini izleyebilirsiniz.

## <a name="track-restore-progress"></a>Geri yükleme ilerlemesini izleme

Güvenilir Durum lu hizmetin veya Güvenilir Aktör'ün bir bölümü, aynı anda yalnızca bir geri yükleme isteğini kabul eder. Bir bölüm yalnızca geçerli geri yükleme isteği tamamlandıktan sonra başka bir isteği kabul eder. Aynı anda farklı bölümlerde birden çok geri yükleme isteği tetiklenebilir.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell Microsoft.ServiceFabric.Powershell.Http Modülü kullanarak

```powershell
    Get-SFPartitionRestoreProgress -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22'
```

#### <a name="rest-call-using-powershell"></a>Powershell kullanarak Dinlenme Çağrısı

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetRestoreProgress?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'

$restoreResponse = (ConvertFrom-Json $response.Content)
$restoreResponse | Format-List
```

Geri yükleme isteği aşağıdaki sırada ilerler:

1. **Kabul :** _Kabul Edilen_ geri yükleme durumu, istenen bölümün doğru istek parametreleri ile tetiklendiğini gösterir.
    ```
    RestoreState  : Accepted
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
2. **Devam Etme**: _Devam Etme_ durumu, istekte belirtilen yedeklemeyle birlikte bölmede bir geri yükleme oluştuğunu gösterir. Bölüm, _veri kaybı_ durumunu bildirir.
    ```
    RestoreState  : RestoreInProgress
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
    
3. **Başarı**, **Hata**, veya **Zaman Adabı**: İstenen geri yükleme aşağıdaki durumlardan herhangi birinde tamamlanabilir. Her devlet aşağıdaki önem ve yanıt ayrıntıları vardır:
    - **Başarı**: _Başarı_ geri yükleme durumu yeniden bir bölüm durumunu gösterir. Bölüm utc zaman ile birlikte _Geri Yükleme Epoch_ ve _RestoredLSN_ durumları raporlar.

        ```
        RestoreState  : Success
        TimeStampUtc  : 2018-11-22T11:22:33Z
        RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
        RestoredLsn   : 3552
        ```        
    - **Hata**: _Hata_ geri yükleme durumu geri yükleme isteğinin başarısızlığını gösterir. Hatanın nedeni bildirilir.

        ```
        RestoreState  : Failure
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```
    - **Zaman Ara :** _Zaman A'lık_ geri yükleme durumu, isteğin zaman anına sahip olduğunu gösterir. Daha fazla [RestoreTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout)ile yeni bir geri yükleme isteği oluşturun. Varsayılan zaman dilimi 10 dakikadır. Yeniden geri yükleme isteğinde bulunmadan önce bölümün veri kaybı durumunda olmadığından emin olun.
     
        ```
        RestoreState  : Timeout
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```

## <a name="automatic-restore"></a>Otomatik geri yükleme

Otomatik _geri yükleme_için Servis Kumaşı kümesinde Güvenilir Durum lu hizmeti ve Güvenilir Aktörler bölümlerini yapılandırabilirsiniz. _Yedekleme ilkesinde_ `AutoRestore` doğru olarak ayarlanır. Otomatik _geri yüklemeyi_ etkinleştirmek, veri kaybı raporlandığında en son bölüm yedeklemesinden verileri otomatik olarak geri yükler. Daha fazla bilgi için bkz.

- [Yedekleme İlkesinde Otomatik Geri Yükleme Etkinleştirme](service-fabric-backuprestoreservice-configure-periodic-backup.md#auto-restore-on-data-loss)
- [Geri Yükleme Bölümü API başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-restorepartition)
- [GetPartitionRestoreProgress API başvuru](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionrestoreprogress)

## <a name="next-steps"></a>Sonraki adımlar
- [Düzenli aralıklarla yedekleme yapılandırmasını anlama](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Yedekleme geri yükleme REST API başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[2]: ./media/service-fabric-backuprestoreservice/restore-partition-backup.png
[3]: ./media/service-fabric-backuprestoreservice/restore-partition-fileshare.png