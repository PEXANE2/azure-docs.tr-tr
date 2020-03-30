---
title: Bağımsız Azure Hizmet Kumaşında periyodik yedekleme/geri yükleme
description: Uygulama verilerinizin periyodik veri yedeklemesini etkinleştirmek için Service Fabric'in periyodik yedekleme ve geri yükleme özelliğini kullanın.
author: hrushib
ms.topic: conceptual
ms.date: 5/24/2019
ms.author: hrushib
ms.openlocfilehash: 938cbbde9f53c52350ef64715f6c61c4aa961057
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75526252"
---
# <a name="periodic-backup-and-restore-in-a-standalone-service-fabric"></a>Tek başına Hizmet Kumaşı'nda periyodik yedekleme ve geri yükleme
> [!div class="op_single_selector"]
> * [Azure’da kümeler](service-fabric-backuprestoreservice-quickstart-azurecluster.md) 
> * [Bağımsız Kümeler](service-fabric-backuprestoreservice-quickstart-standalonecluster.md)
> 

Service Fabric, güvenilir, dağıtılmış, mikro hizmetlere dayalı bulut uygulamalarının geliştirilmesini ve yönetilmesini kolaylaştıran dağıtılmış bir sistem platformudur. Hem devletsiz hem de devlete özel mikro hizmetlerin yürütülmesini sağlar. Durumlu hizmetler, istek ve yanıt ın veya tam bir işlemin ötesinde mutable, yetkili durumu koruyabilir. Bir Devlet hizmeti uzun bir süre için aşağı gider veya bir felaket nedeniyle bilgi kaybederse, geri geldikten sonra hizmet vermeye devam etmek için devletin bazı yeni yedekleme geri yüklenebilir.

Service Fabric, hizmetin yüksek oranda kullanılabilir olduğundan emin olmak için durumu birden çok düğüm üzerinde çoğaltır. Kümedeki bir düğüm başarısız olsa bile, hizmet kullanılabilir olmaya devam eder. Ancak bazı durumlarda, hizmet verilerinin daha geniş hatalara karşı güvenilir olması yine de arzu edilir.
 
Örneğin, bir hizmet aşağıdaki senaryolardan korumak için verilerini yedeklemek isteyebilir:
- Tüm Service Fabric kümesinin kalıcı kaybı.
- Bir hizmet bölümünün yinelemelerinin çoğunluğunun kalıcı kaybı
- Durum yanlışlıkla silinir veya bozuk alır yönetim hataları. Örneğin, yeterli ayrıcalığa sahip bir yönetici hizmeti hatalı bir şekilde siler.
- Hizmette veri bozulmasına neden olan hatalar. Örneğin, bir hizmet kodu yükseltmesi güvenilir bir koleksiyona hatalı veri yazmaya başladığında bu durum oluşabilir. Böyle bir durumda, hem kod hem de veri önceki bir duruma döndürülebilir.
- Çevrimdışı veri işleme. Verileri oluşturan hizmetten ayrı olarak gerçekleşen iş zekası için verilerin çevrimdışı işlenmesi uygun olabilir.

Service Fabric, zaman [yedeklemeve geri yükleme](service-fabric-reliable-services-backup-restore.md)noktası yapmak için dahili bir API sağlar. Uygulama geliştiricileri bu API'leri hizmetin durumunu düzenli aralıklarla yedeklemek için kullanabilir. Ayrıca, hizmet yöneticileri uygulamayı yükseltmeden önce olduğu gibi belirli bir zamanda hizmet dışından bir yedekleme tetiklemek istiyorlarsa, geliştiricilerin yedeklemeyi (ve geri yüklemeyi) hizmetten API olarak çıkarmaları gerekir. Yedeklemeleri korumak bunun üzerinde ek bir maliyettir. Örneğin, her yarım saatte beş artımlı yedekleme almak isteyebilirsiniz ve ardından tam bir yedekleme. Tam yedeklemeden sonra, önceki artımlı yedeklemeleri silebilirsiniz. Bu yaklaşım, uygulama geliştirme sırasında ek maliyete yol açan ek kod gerektirir.

Uygulama verilerinin periyodik olarak yedeklenmeleri, dağıtılmış bir uygulamayı yönetmek ve veri kaybına veya hizmet kullanılabilirliğinin uzun süreli kaybedilmesine karşı koruma için temel bir ihtiyaçtır. Service Fabric, herhangi bir ek kod yazmak zorunda kalmadan, özel Güvenilir Hizmetlerin (Aktör Hizmetleri dahil) periyodik yedeklemesini yapılandırmanızı sağlayan isteğe bağlı yedekleme ve geri yükleme hizmeti sağlar. Ayrıca, daha önce alınan yedeklemelerin geri alınmasını da kolaylaştırır. 

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
* Kumaş sürüm 6.4 veya üzeri servis kumaş küme. Gerekli paketi indirmek için adımlar için bu [makaleye](service-fabric-cluster-creation-for-windows-server.md) bakın.
* Depolama alanına bağlanmak için gereken sırlarışifrelemek için X.509 Sertifikası yedeklemeleri depolamak için. Kendi imzalı X.509 sertifikasını nasıl edineceklerini veya oluşturacağınıbilmek için [makaleye](service-fabric-windows-cluster-x509-security.md) bakın.

* Service Fabric SDK sürüm 3.0 veya üzeri kullanılarak oluşturulmuş Servis Kumaşı Güvenilir Stateful uygulaması. .Net Core 2.0'ı hedefleyen uygulamalar için, uygulama Service Fabric SDK sürüm 3.1 veya üzeri kullanılarak oluşturulmalıdır.
* Yapılandırma aramaları yapmak için Microsoft.ServiceFabric.Powershell.Http Modülü [Önizlemede] yükleyin.

```powershell
    Install-Module -Name Microsoft.ServiceFabric.Powershell.Http -AllowPrerelease
```

* Microsoft.ServiceFabric.Powershell.Http Module kullanarak herhangi bir yapılandırma isteği yapmadan önce Cluster'ın `Connect-SFCluster` komutu kullanarak bağlı olduğundan emin olun.

```powershell

    Connect-SFCluster -ConnectionEndpoint 'https://mysfcluster.southcentralus.cloudapp.azure.com:19080'   -X509Credential -FindType FindByThumbprint -FindValue '1b7ebe2174649c45474a4819dafae956712c31d3' -StoreLocation 'CurrentUser' -StoreName 'My' -ServerCertThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'  

```

## <a name="enabling-backup-and-restore-service"></a>Yedekleme ve geri yükleme hizmetini etkinleştirme
Öncelikle kümenizdeki _yedekleme ve geri yükleme hizmetini_ etkinleştirmeniz gerekir. Dağıtmak istediğiniz küme için şablonu alın. [Örnek şablonları](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)kullanabilirsiniz. Aşağıdaki adımlarla _yedekleme ve geri yükleme hizmetini_ etkinleştirin:

1. `apiversion` Küme yapılandırma `10-2017` dosyasında ayarlanıp ayarlanmadığını denetleyin ve değilse, aşağıdaki parçacıkta gösterildiği gibi güncelleştirin:

    ```json
    {
        "apiVersion": "10-2017",
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        ...
    }
    ```

2. Şimdi aşağıdaki parçacık gösterildiği gibi bölüm `addonFeatures` altında `properties` aşağıdaki bölümü ekleyerek _yedekleme ve geri yükleme hizmeti_ etkinleştirin: 

    ```json
        "properties": {
            ...
            "addonFeatures": ["BackupRestoreService"],
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

4. Küme yapılandırma dosyanızı önceki değişikliklerle güncelleştirdikten sonra bunları uygulayın ve dağıtım/yükseltmenin tamamlanmasına izin verin. Tamamlandıktan sonra, _yedekleme ve geri yükleme hizmeti_ kümenizde çalışmaya başlar. Bu hizmetin Uri `fabric:/System/BackupRestoreService` ve hizmet Hizmet Kumaş explorer sistem servis bölümü altında bulunabilir. 



## <a name="enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors"></a>Güvenilir Stateful hizmeti ve Güvenilir Aktörler için periyodik yedeklemeyi etkinleştirme
Güvenilir Stateful hizmeti ve Güvenilir Aktörler için periyodik yedeklemeyi etkinleştirmek için adımları inceleyelim. Bu adımlar,
- Kümenin yedekleme ve _geri yükleme hizmetiyle_ayarlanır.
- Kümede Güvenilir Durumlu hizmet dağıtılır. Bu hızlı başlangıç kılavuzu nun amacı `fabric:/SampleApp` için, uygulama Uri ve Uri Güvenilir `fabric:/SampleApp/MyStatefulService`Stateful hizmet için bu uygulamaya ait . Bu hizmet tek bir bölümle dağıtılır `23aebc1e-e9ea-4e16-9d5c-e91a614fefa7`ve bölüm kimliği.  

### <a name="create-backup-policy"></a>Yedekleme ilkesi oluşturma

İlk adım, yedekleme depolama için tam yedekleme ve bekletme ilkesini tetiklemeden önce izin verilecek yedekleme çizelgesi, yedekleme verileri için hedef depolama, ilke adı, maksimum artımlı yedeklemeler açıklayan yedekleme ilkesi oluşturmaktır. 

Yedekleme depolama için dosya paylaşımı oluşturun ve ReadWrite'ın tüm Service Fabric Node makineleri için bu dosya paylaşımına erişmesini sağlar. Bu örnek, adı `BackupStore` olan paylaşımın `StorageServer`üzerinde mevcut olduğunu varsayar.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell Microsoft.ServiceFabric.Powershell.Http Modülü kullanarak

```powershell

New-SFBackupPolicy -Name 'BackupPolicy1' -AutoRestoreOnDataLoss $true -MaxIncrementalBackups 20 -FrequencyBased -Interval 00:15:00 -FileShare -Path '\\StorageServer\BackupStore' -Basic -RetentionDuration '10.00:00:00'

```
#### <a name="rest-call-using-powershell"></a>Powershell kullanarak Dinlenme Çağrısı

Yeni ilke oluşturmak için gerekli REST API çağırmak için PowerShell komut dosyası aşağıdaki çalıştırın.

```powershell
$ScheduleInfo = @{
    Interval = 'PT15M'
    ScheduleKind = 'FrequencyBased'
}   

$StorageInfo = @{
    Path = '\\StorageServer\BackupStore'
    StorageKind = 'FileShare'
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
$url = "http://localhost:19080/BackupRestore/BackupPolicies/$/Create?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json'
```

#### <a name="using-service-fabric-explorer"></a>Servis Kumaş Explorer kullanma

1. Hizmet Kumaşı Gezgini'nde Yedekler sekmesine gidin ve Yedekleme İlkesi Oluştur'> Eylemleri seçin.

    ![Yedekleme İlkesi Oluştur][6]

2. Bilgileri doldurun. Bağımsız kümeler için FileShare seçilmelidir.

    ![Yedekleme İlkesi Dosya Paylaşımı Oluşturma][7]

### <a name="enable-periodic-backup"></a>Periyodik yedeklemeyi etkinleştirme
Uygulamanın veri koruma gereksinimlerini karşılamak için ilke tanımladıktan sonra, yedekleme ilkesi uygulama ile ilişkili olmalıdır. Gereksinime bağlı olarak, yedekleme ilkesi bir uygulama, hizmet veya bir bölümle ilişkilendirilebilir.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell Microsoft.ServiceFabric.Powershell.Http Modülü kullanarak

```powershell
Enable-SFApplicationBackup -ApplicationId 'SampleApp' -BackupPolicyName 'BackupPolicy1'
```

#### <a name="rest-call-using-powershell"></a>Powershell kullanarak Dinlenme Çağrısı
Yukarıdaki adımda oluşturulan adla `BackupPolicy1` yedekleme ilkesini uygulamayla `SampleApp`ilişkilendirmek için gerekli REST API'yi çağırmak için PowerShell komut dosyasını aşağıdaki yürütme.

```powershell
$BackupPolicyReference = @{
    BackupPolicyName = 'BackupPolicy1'
}

$body = (ConvertTo-Json $BackupPolicyReference)
$url = "http://localhost:19080/Applications/SampleApp/$/EnableBackup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json'
``` 

#### <a name="using-service-fabric-explorer"></a>Servis Kumaş Explorer kullanma

1. Bir uygulama seçin ve eyleme geçin. Uygulama Yedeklemesini Etkinleştir/Güncelle'yi tıklatın.

    ![Uygulama Yedeklemesini Etkinleştir][3] 

2. Son olarak, istenen ilkeyi seçin ve Yedeklemeyi Etkinleştir'i tıklatın.

    ![İlke Seçin][4]

### <a name="verify-that-periodic-backups-are-working"></a>Periyodik yedeklemelerin çalıştığını doğrulama

Uygulama için yedekleme etkinleştirdikten sonra, uygulama altındaki Güvenilir Durum lu hizmetlere ve Güvenilir Aktörlere ait tüm bölümler, ilişkili yedekleme ilkesiuyarınca düzenli olarak yedeklenmeye başlar.

![Bölüm BackedUp Sağlık Etkinliği][0]

### <a name="list-backups"></a>Yedeklemeleri Listele

Güvenilir Durum hizmetleri ve uygulamanın Güvenilir Aktörleri'ne ait tüm bölümlerle ilişkili yedeklemeler _GetBackups_ API kullanılarak numaralandırılabilir. Gereksinime bağlı olarak, yedeklemeler uygulama, hizmet veya bölüm için numaralandırılabilir.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell Microsoft.ServiceFabric.Powershell.Http Modülü kullanarak

```powershell
    Get-SFApplicationBackupList -ApplicationId WordCount     
```

#### <a name="rest-call-using-powershell"></a>Powershell kullanarak Dinlenme Çağrısı

Uygulama içindeki tüm bölümler için oluşturulan yedeklemeleri doğrulamak için HTTP API'yi `SampleApp` çağırmak için PowerShell komut dosyasını aşağıdaki şekilde uygulayın.

```powershell
$url = "http://localhost:19080/Applications/SampleApp/$/GetBackups?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Get

$BackupPoints = (ConvertFrom-Json $response.Content)
$BackupPoints.Items
```

Yukarıdaki çalışma için örnek çıktı:

```
BackupId                : d7e4038e-2c46-47c6-9549-10698766e714
BackupChainId           : d7e4038e-2c46-47c6-9549-10698766e714
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=23aebc1e-e9ea-4e16-9d5c-e91a614fefa7}
BackupLocation          : SampleApp\MyStatefulService\23aebc1e-e9ea-4e16-9d5c-e91a614fefa7\2018-04-01 19.39.40.zip
BackupType              : Full
EpochOfLastBackupRecord : @{DataLossNumber=131670844862460432; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 2058
CreationTimeUtc         : 2018-04-01T19:39:40Z
FailureError            : 

BackupId                : 8c21398a-2141-4133-b4d7-e1a35f0d7aac
BackupChainId           : d7e4038e-2c46-47c6-9549-10698766e714
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=23aebc1e-e9ea-4e16-9d5c-e91a614fefa7}
BackupLocation          : SampleApp\MyStatefulService\23aebc1e-e9ea-4e16-9d5c-e91a614fefa7\2018-04-01 19.54.38.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131670844862460432; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 2237
CreationTimeUtc         : 2018-04-01T19:54:38Z
FailureError            : 

BackupId                : fc75bd4c-798c-4c9a-beee-e725321f73b2
BackupChainId           : d7e4038e-2c46-47c6-9549-10698766e714
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=23aebc1e-e9ea-4e16-9d5c-e91a614fefa7}
BackupLocation          : SampleApp\MyStatefulService\23aebc1e-e9ea-4e16-9d5c-e91a614fefa7\2018-04-01 20.09.44.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131670844862460432; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 2437
CreationTimeUtc         : 2018-04-01T20:09:44Z
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

[0]: ./media/service-fabric-backuprestoreservice/partition-backedup-health-event.png
[3]: ./media/service-fabric-backuprestoreservice/enable-app-backup.png
[4]: ./media/service-fabric-backuprestoreservice/enable-application-backup.png
[5]: ./media/service-fabric-backuprestoreservice/backup-enumeration.png
[6]: ./media/service-fabric-backuprestoreservice/create-bp.png
[7]: ./media/service-fabric-backuprestoreservice/create-bp-fileshare.png