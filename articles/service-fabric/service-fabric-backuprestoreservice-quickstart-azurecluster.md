---
title: Azure Service Fabric’te düzenli yedekleme ve geri yükleme
description: Uygulama verilerinizin periyodik veri yedeklemesini etkinleştirmek için Service Fabric'in periyodik yedekleme ve geri yükleme özelliğini kullanın.
author: hrushib
ms.topic: conceptual
ms.date: 5/24/2019
ms.author: hrushib
ms.openlocfilehash: f56fcb7d1dde700d954c3b55bcf8cd7759893521
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259012"
---
# <a name="periodic-backup-and-restore-in-an-azure-service-fabric-cluster"></a>Azure Hizmet Kumaşı kümesinde periyodik yedekleme ve geri yükleme
> [!div class="op_single_selector"]
> * [Azure’da kümeler](service-fabric-backuprestoreservice-quickstart-azurecluster.md) 
> * [Bağımsız Kümeler](service-fabric-backuprestoreservice-quickstart-standalonecluster.md)
> 

Service Fabric, güvenilir, dağıtılmış, mikro hizmetlere dayalı bulut uygulamalarının geliştirilmesini ve yönetilmesini kolaylaştıran dağıtılmış bir sistem platformudur. Hem devletsiz hem de devlete özel mikro hizmetlerin yürütülmesini sağlar. Durumlu hizmetler, istek ve yanıt ın veya tam bir işlemin ötesinde mutable, yetkili durumu koruyabilir. Bir Devlet hizmeti uzun bir süre için aşağı gider veya bir felaket nedeniyle bilgi kaybederse, geri geldikten sonra hizmet vermeye devam etmek için devletin bazı yeni yedekleme geri yüklenebilir.

Service Fabric, hizmetin yüksek oranda kullanılabilir olduğundan emin olmak için durumu birden çok düğüm üzerinde çoğaltır. Kümedeki bir düğüm başarısız olsa bile, hizmet kullanılabilir olmaya devam eder. Ancak bazı durumlarda, hizmet verilerinin daha geniş hatalara karşı güvenilir olması yine de arzu edilir.
 
Örneğin, hizmet aşağıdaki senaryolardan korumak için verilerini yedeklemek isteyebilir:
- Tüm Service Fabric kümesinin kalıcı kaybı durumunda.
- Bir hizmet bölümünün yinelemelerinin çoğunluğunun kalıcı kaybı
- Durum yanlışlıkla silinir veya bozuk alır yönetim hataları. Örneğin, yeterli ayrıcalığa sahip bir yönetici hizmeti hatalı bir şekilde siler.
- Hizmette veri bozulmasına neden olan hatalar. Örneğin, bir hizmet kodu yükseltmesi güvenilir bir koleksiyona hatalı veri yazmaya başladığında bu durum oluşabilir. Böyle bir durumda, hem kod hem de veri önceki bir duruma döndürülebilir.
- Çevrimdışı veri işleme. Verileri oluşturan hizmetten ayrı olarak gerçekleşen iş zekası için verilerin çevrimdışı işlenmesi uygun olabilir.

Service Fabric, zaman [yedeklemeve geri yükleme](service-fabric-reliable-services-backup-restore.md)noktası yapmak için dahili bir API sağlar. Uygulama geliştiricileri bu API'leri hizmetin durumunu düzenli aralıklarla yedeklemek için kullanabilir. Ayrıca, hizmet yöneticileri uygulamayı yükseltmeden önce olduğu gibi belirli bir zamanda hizmet dışından bir yedekleme tetiklemek istiyorlarsa, geliştiricilerin yedeklemeyi (ve geri yüklemeyi) hizmetten API olarak çıkarmaları gerekir. Yedeklemeleri korumak bunun üzerinde ek bir maliyettir. Örneğin, her yarım saatte beş artımlı yedekleme almak isteyebilirsiniz ve ardından tam bir yedekleme. Tam yedeklemeden sonra, önceki artımlı yedeklemeleri silebilirsiniz. Bu yaklaşım, uygulama geliştirme sırasında ek maliyete yol açan ek kod gerektirir.

Hizmet Kumaşındaki Yedekleme ve Geri Yükleme hizmeti, özel hizmetlerde depolanan bilgilerin kolay ve otomatik olarak yedeklemesini sağlar. Uygulama verilerinin periyodik olarak yedeklenin, veri kaybına ve hizmet inmemelerine karşı koruma için esastır. Service Fabric, herhangi bir ek kod yazmak zorunda kalmadan, özel Güvenilir Hizmetlerin (Aktör Hizmetleri dahil) periyodik yedeklemesini yapılandırmanızı sağlayan isteğe bağlı yedekleme ve geri yükleme hizmeti sağlar. Ayrıca, daha önce alınan yedeklemelerin geri alınmasını da kolaylaştırır. 


Service Fabric, periyodik yedekleme ve geri yükleme özelliğiyle ilgili aşağıdaki işlevselliği elde etmek için bir dizi API sağlar:

- (Harici) depolama konumlarına yedekleme yüklemek için destek ile Güvenilir Stateful hizmetleri ve Güvenilir Aktörler periyodik yedekleme zamanlayın. Desteklenen depolama konumları
    - Azure Storage
    - Dosya Paylaşımı (şirket içi)
- Yedeklemeleri sayısallandırma
- Bir bölümün geçici yedeklemesini tetikleme
- Önceki yedeklemeyi kullanarak bir bölümü geri yükleme
- Yedeklemeleri geçici olarak askıya alma
- Yedeklemelerin bekletme yönetimi (gelecek)

## <a name="prerequisites"></a>Ön koşullar
* Kumaş sürüm 6.4 veya üzeri servis kumaş küme. Azure kaynak şablonu kullanarak Hizmet Dokusu kümesi oluşturma adımları için bu [makaleye](service-fabric-cluster-creation-via-arm.md) bakın.
* Depolama alanına bağlanmak için gereken sırlarışifrelemek için X.509 Sertifikası yedeklemeleri depolamak için. X.509 sertifikasıalmak veya oluşturmak için [makaleye](service-fabric-cluster-creation-via-arm.md) bakın.
* Service Fabric SDK sürüm 3.0 veya üzeri kullanılarak oluşturulmuş Servis Kumaşı Güvenilir Stateful uygulaması. .NET Core 2.0'ı hedefleyen uygulamalariçin, uygulama Service Fabric SDK sürüm 3.1 veya üzeri kullanılarak oluşturulmalıdır.
* Uygulama yedeklemelerini depolamak için Azure Depolama hesabı oluşturun.
* Yapılandırma aramaları yapmak için Microsoft.ServiceFabric.Powershell.Http Modülü [Önizlemede] yükleyin.

```powershell
    Install-Module -Name Microsoft.ServiceFabric.Powershell.Http -AllowPrerelease
```

* Microsoft.ServiceFabric.Powershell.Http Module kullanarak herhangi bir yapılandırma isteği yapmadan önce Cluster'ın `Connect-SFCluster` komutu kullanarak bağlı olduğundan emin olun.

```powershell

    Connect-SFCluster -ConnectionEndpoint 'https://mysfcluster.southcentralus.cloudapp.azure.com:19080'   -X509Credential -FindType FindByThumbprint -FindValue '1b7ebe2174649c45474a4819dafae956712c31d3' -StoreLocation 'CurrentUser' -StoreName 'My' -ServerCertThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'  

```

## <a name="enabling-backup-and-restore-service"></a>Yedekleme ve geri yükleme hizmetini etkinleştirme

### <a name="using-azure-portal"></a>Azure portalını kullanma

`+ Show optional settings` Sekmede `Cluster Configuration` onay kutusunu etkinleştirin. `Include backup restore service`

![Portal ile Yedekleme Geri Yükleme Hizmetini Etkinleştir][1]


### <a name="using-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi Şablonu'nun Kullanımı
Öncelikle kümenizdeki _yedekleme ve geri yükleme hizmetini_ etkinleştirmeniz gerekir. Dağıtmak istediğiniz küme için şablonu alın. [Örnek şablonları](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) kullanabilir veya Bir Kaynak Yöneticisi şablonu oluşturabilirsiniz. Aşağıdaki adımlarla _yedekleme ve geri yükleme hizmetini_ etkinleştirin:

1. Kaynağın `apiversion` **`2018-02-01`** `Microsoft.ServiceFabric/clusters` kaynak için ayarlanıp ayarlanmadığını denetleyin ve değilse, aşağıdaki parçacıkta gösterildiği gibi güncelleştirin:

    ```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Şimdi aşağıdaki parçacık gösterildiği gibi bölüm `addonFeatures` altında `properties` aşağıdaki bölümü ekleyerek _yedekleme ve geri yükleme hizmeti_ etkinleştirin: 

    ```json
        "properties": {
            ...
            "addonFeatures":  ["BackupRestoreService"],
            "fabricSettings": [ ... ]
            ...
        }

    ```
3. Kimlik bilgilerinin şifrelemesi için X.509 sertifikasını yapılandırın. Bu, depolamaya bağlanmak için sağlanan kimlik bilgilerinin kalıcı olmadan önce şifrelenmiş olduğundan emin olmak için önemlidir. Aşağıdaki parçacıkta gösterildiği gibi `BackupRestoreService` aşağıdaki `fabricSettings` bölümü ekleyerek şifreleme sertifikasını yapılandırın: 

    ```json
    "properties": {
        ...
        "addonFeatures": ["BackupRestoreService"],
        "fabricSettings": [{
            "name": "BackupRestoreService",
            "parameters":  [{
                "name": "SecretEncryptionCertThumbprint",
                "value": "[Thumbprint]"
            }]
        }
        ...
    }
    ```

4. Küme şablonunuzu önceki değişikliklerle güncelledikten sonra bunları uygulayın ve dağıtım/yükseltmenin tamamlanmasına izin verin. Tamamlandıktan sonra, _yedekleme ve geri yükleme hizmeti_ kümenizde çalışmaya başlar. Bu hizmetin Uri `fabric:/System/BackupRestoreService` ve hizmet Hizmet Kumaş explorer sistem servis bölümü altında bulunabilir. 

## <a name="enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors"></a>Güvenilir Stateful hizmeti ve Güvenilir Aktörler için periyodik yedeklemeyi etkinleştirme
Güvenilir Stateful hizmeti ve Güvenilir Aktörler için periyodik yedeklemeyi etkinleştirmek için adımları inceleyelim. Bu adımlar,
- Küme yedekleme _ve geri yükleme hizmeti_ile X.509 güvenlik kullanarak kurulum olduğunu.
- Kümede Güvenilir Durumlu hizmet dağıtılır. Bu hızlı başlangıç kılavuzu nun amacı `fabric:/SampleApp` için, uygulama Uri ve Uri Güvenilir `fabric:/SampleApp/MyStatefulService`Stateful hizmet için bu uygulamaya ait . Bu hizmet tek bir bölümle dağıtılır `974bd92a-b395-4631-8a7f-53bd4ae9cf22`ve bölüm kimliği.
- Yönetici rolü olan istemci sertifikası, aşağıdaki komut dosyalarının çağrılacağı makinedeki _CurrentUser_ sertifikası mağaza konumunun _(Kişisel)_ _mağazam_ ın adına yüklenir. Bu örnek, bu sertifikanın parmak izi olarak kullanır. `1b7ebe2174649c45474a4819dafae956712c31d3` İstemci sertifikaları hakkında daha fazla bilgi için [Service Fabric istemcileri için Rol tabanlı erişim denetimine](service-fabric-cluster-security-roles.md)bakın.

### <a name="create-backup-policy"></a>Yedekleme ilkesi oluşturma

İlk adım, yedekleme depolama için tam yedekleme ve bekletme ilkesini tetiklemeden önce izin verilecek yedekleme çizelgesi, yedekleme verileri için hedef depolama, ilke adı, maksimum artımlı yedeklemeler açıklayan yedekleme ilkesi oluşturmaktır. 

Yedekleme depolama için yukarıda oluşturulan Azure Depolama hesabını kullanın. Kapsayıcı `backup-container` yedekleme depolamak için yapılandırılır. Yedekleme yükleme sırasında bu ada sahip bir kapsayıcı zaten yoksa oluşturulur. Azure `ConnectionString` Depolama hesabı için geçerli bir bağlantı dizesi ile, depolama `account-key` hesabı adınız ve depolama hesabı anahtarınızla değiştirin. `account-name`

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>PowerShell Microsoft.ServiceFabric.Powershell.Http Modülü kullanarak

Yeni yedekleme ilkesi oluşturmak için PowerShell cmdlets'i uygulayın. Depolama `account-name` hesabı adınız ve `account-key` depolama hesabı anahtarınızla değiştirin.

```powershell

New-SFBackupPolicy -Name 'BackupPolicy1' -AutoRestoreOnDataLoss $true -MaxIncrementalBackups 20 -FrequencyBased -Interval 00:15:00 -AzureBlobStore -ConnectionString 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' -ContainerName 'backup-container' -Basic -RetentionDuration '10.00:00:00'

```

#### <a name="rest-call-using-powershell"></a>PowerShell kullanarak Dinlenme Çağrısı

Yeni ilke oluşturmak için gerekli REST API çağırmak için PowerShell komut dosyası aşağıdaki çalıştırın. Depolama `account-name` hesabı adınız ve `account-key` depolama hesabı anahtarınızla değiştirin.

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$ScheduleInfo = @{
    Interval = 'PT15M'
    ScheduleKind = 'FrequencyBased'
}

$RetentionPolicy = @{ 
    RetentionPolicyType = 'Basic'
    RetentionDuration =  'P10D'
}

$BackupPolicy = @{
    Name = 'BackupPolicy1'
    MaxIncrementalBackups = 20
    Schedule = $ScheduleInfo
    Storage = $StorageInfo
    RetentionPolicy = $RetentionPolicy
}

$body = (ConvertTo-Json $BackupPolicy)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/BackupRestore/BackupPolicies/$/Create?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'

```

#### <a name="using-service-fabric-explorer"></a>Servis Kumaş Explorer kullanma

1. Hizmet Kumaşı Gezgini'nde Yedekler sekmesine gidin ve Yedekleme İlkesi Oluştur'> Eylemleri seçin.

    ![Yedekleme İlkesi Oluştur][6]

2. Bilgileri doldurun. Azure kümeleri için AzureBlobStore seçilmelidir.

    ![Yedekleme İlkesi Azure Blob Depolama Oluşturma][7]

### <a name="enable-periodic-backup"></a>Periyodik yedeklemeyi etkinleştirme
Uygulamanın veri koruma gereksinimlerini karşılamak için yedekleme ilkesini tanımladıktan sonra, yedekleme ilkesi uygulamayla ilişkilendirilmelidir. Gereksinime bağlı olarak, yedekleme ilkesi bir uygulama, hizmet veya bir bölümle ilişkilendirilebilir.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>PowerShell Microsoft.ServiceFabric.Powershell.Http Modülü kullanarak

```powershell

Enable-SFApplicationBackup -ApplicationId 'SampleApp' -BackupPolicyName 'BackupPolicy1'

```
#### <a name="rest-call-using-powershell"></a>PowerShell kullanarak Dinlenme Çağrısı

Yukarıdaki adımda oluşturulan adla `BackupPolicy1` yedekleme ilkesini uygulamayla `SampleApp`ilişkilendirmek için gerekli REST API'yi çağırmak için PowerShell komut dosyasını aşağıdaki yürütme.

```powershell
$BackupPolicyReference = @{
    BackupPolicyName = 'BackupPolicy1'
}

$body = (ConvertTo-Json $BackupPolicyReference)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Applications/SampleApp/$/EnableBackup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
``` 

#### <a name="using-service-fabric-explorer"></a>Servis Kumaş Explorer kullanma

1. Bir uygulama seçin ve eyleme geçin. Uygulama Yedeklemesini Etkinleştir/Güncelle'yi tıklatın.

    ![Uygulama Yedeklemesini Etkinleştir][3]

2. Son olarak, istenen ilkeyi seçin ve Yedeklemeyi Etkinleştir'i tıklatın.

    ![İlke Seçin][4]


### <a name="verify-that-periodic-backups-are-working"></a>Periyodik yedeklemelerin çalıştığını doğrulama

Uygulama düzeyinde yedeklemeyi etkinleştirdikten sonra, uygulama altındaki Güvenilir Durum hizmetleri ve Güvenilir Aktörler'e ait tüm bölümler, ilişkili yedekleme ilkesiuyarınca düzenli olarak yedeklenmeye başlar. 

![Bölüm BackedUp Sağlık Etkinliği][0]

### <a name="list-backups"></a>Yedeklemeleri Listele

Güvenilir Durum hizmetleri ve uygulamanın Güvenilir Aktörleri'ne ait tüm bölümlerle ilişkili yedeklemeler _GetBackups_ API kullanılarak numaralandırılabilir. Yedeklemeler bir uygulama, hizmet veya bölüm için numaralandırılabilir.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>PowerShell Microsoft.ServiceFabric.Powershell.Http Modülü kullanarak

```powershell
    
Get-SFApplicationBackupList -ApplicationId WordCount
```

#### <a name="rest-call-using-powershell"></a>PowerShell kullanarak Dinlenme Çağrısı

Uygulama içindeki tüm bölümler için oluşturulan yedeklemeleri doğrulamak için HTTP API'yi `SampleApp` çağırmak için PowerShell komut dosyasını aşağıdaki şekilde uygulayın.

```powershell
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Applications/SampleApp/$/GetBackups?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'

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

#### <a name="using-service-fabric-explorer"></a>Servis Kumaş Explorer kullanma

Hizmet Kumaş ı Explorer'daki yedeklemeleri görüntülemek için bir bölüme gidin ve Yedekler sekmesini seçin.

![Yedeklemeleri Sayısalat][5]

## <a name="limitation-caveats"></a>Sınırlama / uyarılar
- Servis Kumaş PowerShell cmdlets önizleme modunda.
- Linux'ta Service Fabric kümeleri için destek yok.

## <a name="next-steps"></a>Sonraki adımlar
- [Düzenli aralıklarla yedekleme yapılandırmasını anlama](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Yedekleme geri yükleme REST API başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/partition-backedup-health-event-azure.png
[1]: ./media/service-fabric-backuprestoreservice/enable-backup-restore-service-with-portal.png
[3]: ./media/service-fabric-backuprestoreservice/enable-app-backup.png
[4]: ./media/service-fabric-backuprestoreservice/enable-application-backup.png
[5]: ./media/service-fabric-backuprestoreservice/backup-enumeration.png
[6]: ./media/service-fabric-backuprestoreservice/create-bp.png
[7]: ./media/service-fabric-backuprestoreservice/creation-bp.png