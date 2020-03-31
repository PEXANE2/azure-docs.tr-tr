---
title: Hizmet Kumaşı için Azure Dosyaları toplu sürücü
description: Service Fabric, kapsayıcınızdaki birimleri yedeklemek için Azure Dosyalarını kullanmayı destekler.
ms.topic: conceptual
ms.date: 6/10/2018
ms.openlocfilehash: 514a0cb12359d58e38ebc30ae12cdb277757f2b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75750048"
---
# <a name="azure-files-volume-driver-for-service-fabric"></a>Hizmet Kumaşı için Azure Dosyaları toplu sürücü

Azure Dosyaları birim sürücüsü, Docker kapsayıcıları için [Azure Dosyaları](/azure/storage/files/storage-files-introduction) tabanlı birimler sağlayan bir Docker [birim eklentisidir.](https://docs.docker.com/engine/extend/plugins_volume/) Küme deki diğer Servis Kumaşı konteyner uygulamaları için hacim sağlamak için Service Fabric kümesine dağıtılabilen servis kumaşı uygulaması olarak paketlenir.

> [!NOTE]
> Azure Dosyaları ses eklentisinin 6.5.661.9590 sürümü genel kullanılabilirlik için yayımlanmıştır.
>

## <a name="prerequisites"></a>Ön koşullar
* Azure Dosyaları ses eklentisinin Windows sürümü [Yalnızca Windows Server sürüm 1709,](/windows-server/get-started/whats-new-in-windows-server-1709)Windows [10 sürüm 1709](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1709) veya daha sonraki işletim sistemlerinde çalışır.

* Azure Dosyaları ses eklentisinin Linux sürümü Service Fabric tarafından desteklenen tüm işletim sistemi sürümlerinde çalışır.

* Azure Dosyaları ses eklentisi yalnızca Service Fabric sürüm 6.2 ve daha yeni sürümde çalışır.

* Hizmet Kumaşı kapsayıcı uygulaması için birim olarak kullanmak üzere bir dosya paylaşımı oluşturmak için [Azure Dosyaları belgelerindeki](/azure/storage/files/storage-how-to-create-file-share) yönergeleri izleyin.

* Service Fabric modülü veya [SFCTL](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) yüklü [Powershell'e](/azure/service-fabric/service-fabric-get-started) ihtiyacınız olacaktır.

* Hyper-V kapsayıcıları kullanıyorsanız, Azure Kaynak Yöneticisi şablonunuzda (Azure kümesi) veya ClusterConfig.json (bağımsız küme) kümesindeki ClusterManifest (yerel küme) veya fabricSettings bölümüne aşağıdaki parçacıkların eklenmesi gerekir.

ClusterManifest'te Barındırma bölümüne aşağıdakilerin eklenmesi gerekir. Bu örnekte, birim adı **sfazurefile** ve küme üzerinde dinlediği bağlantı noktası **19100'dür.** Bunları kümeniz için doğru değerlerle değiştirin.

``` xml 
<Section Name="Hosting">
  <Parameter Name="VolumePluginPorts" Value="sfazurefile:19100" />
</Section>
```

Azure Kaynak Yöneticisi şablonunuzdaki (Azure dağıtımları için) veya ClusterConfig.json (bağımsız dağıtımlar için) kumaş Ayarları bölümünde aşağıdaki snippet'in eklenmesi gerekir. Yine, birim adı ve bağlantı noktası değerlerini kendi

```json
"fabricSettings": [
  {
    "name": "Hosting",
    "parameters": [
      {
          "name": "VolumePluginPorts",
          "value": "sfazurefile:19100"
      }
    ]
  }
]
```

## <a name="deploy-a-sample-application-using-service-fabric-azure-files-volume-driver"></a>Service Fabric Azure Files toplu sürücü kullanarak örnek bir uygulama dağıtma

### <a name="using-azure-resource-manager-via-the-provided-powershell-script-recommended"></a>Sağlanan Powershell komut dosyası üzerinden Azure Kaynak Yöneticisi'ni kullanma (önerilir)

Kümeniz Azure'da dayanıyorsa, kullanım kolaylığı ve altyapıyı kod olarak koruma modeline doğru ilerlemeye yardımcı olmak için Azure Kaynak Yöneticisi uygulama kaynak modelini kullanarak uygulamaları ona dağıtmanızı öneririz. Bu yaklaşım, Azure Dosyaları ses sürücüsü için uygulama sürümünü izleme gereksinimini ortadan kaldırır. Ayrıca, desteklenen her işletim sistemi için ayrı Azure Kaynak Yöneticisi şablonlarını korumanızı da sağlar. Komut dosyası, Azure Dosyaları uygulamasının en son sürümünü dağıtdığınızı varsayar ve işletim sistemi türü, küme abonelik kimliği ve kaynak grubu için parametreler alır. Script'i [Service Fabric indirme sitesinden](https://sfazfilevd.blob.core.windows.net/sfazfilevd/DeployAzureFilesVolumeDriver.zip)indirebilirsiniz. Bunun, Azure Dosyaları ses eklentisinin Docker daemon'dan gelen istekler için dinlediği bağlantı noktası olan ListenPort'u otomatik olarak 19100 olarak ayarladığını unutmayın. "listenPort" adlı parametre ekleyerek değiştirebilirsiniz. Bağlantı noktasının kümenin veya uygulamalarınızın kullandığı başka bir bağlantı noktasıyla çakışmadığını sağlayın.
 

Windows için Azure Kaynak Yöneticisi dağıtım komutu:
```powershell
.\DeployAzureFilesVolumeDriver.ps1 -subscriptionId [subscriptionId] -resourceGroupName [resourceGroupName] -clusterName [clusterName] -windows
```

Linux için Azure Kaynak Yöneticisi dağıtım komutu:
```powershell
.\DeployAzureFilesVolumeDriver.ps1 -subscriptionId [subscriptionId] -resourceGroupName [resourceGroupName] -clusterName [clusterName] -linux
```

Komut dosyasını başarıyla çalıştırdıktan sonra, [uygulama bölümünüzü yapılandırmaya geçebilirsiniz.](/azure/service-fabric/service-fabric-containers-volume-logging-drivers#configure-your-applications-to-use-the-volume)


### <a name="manual-deployment-for-standalone-clusters"></a>Bağımsız kümeler için el ile dağıtım

Konteynerleriniz için hacimsağlayan Servis Kumaşı uygulamasını Service [Fabric indirme sitesinden](https://sfazfilevd.blob.core.windows.net/sfazfilevd/AzureFilesVolumePlugin.6.5.661.9590.zip)indirebilirsiniz. Uygulama [PowerShell,](./service-fabric-deploy-remove-applications.md) [CLI](./service-fabric-application-lifecycle-sfctl.md) veya [FabricClient API'leri](./service-fabric-deploy-remove-applications-fabricclient.md)aracılığıyla kümeye dağıtılabilir.

1. Komut satırını kullanarak, dizini indirilen uygulama paketinin kök dizini olarak değiştirin.

    ```powershell
    cd .\AzureFilesVolume\
    ```

    ```bash
    cd ~/AzureFilesVolume
    ```

2. Ardından, uygulama paketini [ApplicationPackagePath] ve [ImageStoreConnectionString] için uygun değerlerle görüntü deposuna kopyalayın:

    ```powershell
    Copy-ServiceFabricApplicationPackage -ApplicationPackagePath [ApplicationPackagePath] -ImageStoreConnectionString [ImageStoreConnectionString] -ApplicationPackagePathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl cluster select --endpoint https://testcluster.westus.cloudapp.azure.com:19080 --pem test.pem --no-verify
    sfctl application upload --path [ApplicationPackagePath] --show-progress
    ```

3. Başvuru türünü kaydedin

    ```powershell
    Register-ServiceFabricApplicationType -ApplicationPathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl application provision --application-type-build-path [ApplicationPackagePath]
    ```

4. **ListenPort** uygulama parametre değerine dikkat edin. Bu değer, Azure Dosyaları ses eklentisinin Docker daemon'dan gelen istekler için dinlediği bağlantı noktasıdır. Uygulamaya sağlanan bağlantı noktasının ClusterManifest'teki VolumePluginPorts ile eşleştiğinden ve kümenin veya uygulamalarınızın kullandığı başka bir bağlantı noktasıyla çakışmadığını sağlayın.

    ```powershell
    New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.5.661.9590   -ApplicationParameter @{ListenPort='19100'}
    ```

    ```bash
    sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.5.661.9590  --parameter '{"ListenPort":"19100"}'
    ```

> [!NOTE]
> 
> Windows Server 2016 Datacenter, BDS'nin kapsayıcılara eşlemesi takmayı desteklemez ([Bu yalnızca Windows Server sürüm 1709'da desteklenir).](/virtualization/windowscontainers/manage-containers/container-storage) Bu kısıtlama, ağ toplu eşlemesini ve 1709'dan eski sürümlerde Azure Dosyaları toplu sürücüleri engeller.

#### <a name="deploy-the-application-on-a-local-development-cluster"></a>Uygulamayı yerel bir geliştirme kümesine dağıtma
Yukarıdaki adımları 1-3 [izleyin.](/azure/service-fabric/service-fabric-containers-volume-logging-drivers#manual-deployment-for-standalone-clusters)

 Azure Dosyaları birim eklentisi uygulaması için varsayılan hizmet örneği sayısı -1'dir, bu da kümedeki her düğüme dağıtılan hizmetin bir örneği olduğu anlamına gelir. Ancak, Azure Dosyaları birim eklentisi uygulamasını yerel bir geliştirme kümesine dağıtırken, hizmet örneği sayısı 1 olarak belirtilmelidir. Bu **instancecount** uygulama parametresi üzerinden yapılabilir. Bu nedenle, yerel bir geliştirme kümesinde Azure Dosyaları ses eklentisi uygulamasını oluşturma komutu:

```powershell
New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.5.661.9590  -ApplicationParameter @{ListenPort='19100';InstanceCount='1'}
```

```bash
sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.5.661.9590  --parameter '{"ListenPort": "19100","InstanceCount": "1"}'
```

## <a name="configure-your-applications-to-use-the-volume"></a>Ses düzeyini kullanacak şekilde uygulamalarınızı yapılandırın
Aşağıdaki snippet, uygulamanızın uygulama bildirim dosyasında Azure Dosyaları tabanlı birimin nasıl belirtilebileceğini gösterir. Belirli ilgi öğesi **Birim** etiketidir:

```xml
?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WinNodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
    <Description>Calculator Application</Description>
    <Parameters>
      <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
      <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      <Parameter Name="MyStorageVar" DefaultValue="c:\tmp"></Parameter>
    </Parameters>
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
     <Policies>
       <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
            <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
            <Volume Source="azfiles" Destination="c:\VolumeTest\Data" Driver="sfazurefile">
                <DriverOption Name="shareName" Value="" />
                <DriverOption Name="storageAccountName" Value="" />
                <DriverOption Name="storageAccountKey" Value="" />
                <DriverOption Name="storageAccountFQDN" Value="" />
            </Volume>
       </ContainerHostPolicies>
   </Policies>
    </ServiceManifestImport>
    <ServiceTemplates>
        <StatelessService ServiceTypeName="StatelessNodeService" InstanceCount="5">
            <SingletonPartition></SingletonPartition>
        </StatelessService>
    </ServiceTemplates>
</ApplicationManifest>
```

Azure Dosyaları ses eklentisinin sürücü adı **sfazurefile'dır.** Bu değer, uygulama bildirimindeki **Birim** etiket öğesinin **Sürücü** özniteliği için ayarlanır.

Yukarıdaki parçacıktaki **Ses düzeyi** etiketinde, Azure Dosyaları ses eklentisi aşağıdaki öznitelikleri gerektirir:
- **Kaynak** - Bu birimin adıdır. Kullanıcı, hacmi için herhangi bir ad seçebilir.
- **Hedef** - Bu öznitelik, birimin çalışan kapsayıcı içinde eşlenen konumudur. Bu nedenle, hedefiniz kapsayıcınızın içinde zaten var olan bir konum olamaz

Yukarıdaki parçacıktaki **DriverOption** öğelerinde gösterildiği gibi, Azure Dosyaları ses eklentisi aşağıdaki sürücü seçeneklerini destekler:
- **shareName** - Kapsayıcının hacmini sağlayan Azure Dosyaları dosya paylaşımının adı.
- **storageAccountName** - Azure Dosyaları dosya paylaşımını içeren Azure depolama hesabının adı.
- **storageAccountKey** - Azure Dosyaları dosya paylaşımını içeren Azure depolama hesabıiçin erişim anahtarı.
- **storageAccountFQDN** - Depolama hesabıyla ilişkili alan adı. storageAccountFQDN belirtilmemişse, alan adı varsayılan sonek (.file.core.windows.net) ile storageAccountName kullanılarak oluşturulur.  

    ```xml
    - Example1: 
        <DriverOption Name="shareName" Value="myshare1" />
        <DriverOption Name="storageAccountName" Value="myaccount1" />
        <DriverOption Name="storageAccountKey" Value="mykey1" />
        <!-- storageAccountFQDN will be "myaccount1.file.core.windows.net" -->
    - Example2: 
        <DriverOption Name="shareName" Value="myshare2" />
        <DriverOption Name="storageAccountName" Value="myaccount2" />
        <DriverOption Name="storageAccountKey" Value="mykey2" />
        <DriverOption Name="storageAccountFQDN" Value="myaccount2.file.core.chinacloudapi.cn" />
    ```

## <a name="using-your-own-volume-or-logging-driver"></a>Kendi ses veya günlük sürücüsünü kullanma
Service Fabric ayrıca kendi özel [hacminizin](https://docs.docker.com/engine/extend/plugins_volume/) veya [tomruk sürücülerinizin](https://docs.docker.com/engine/admin/logging/overview/) kullanımına da olanak tanır. Docker birim/günlük sürücüsü kümeye yüklenmezse, RDP/SSH protokollerini kullanarak el ile yükleyebilirsiniz. Bu protokollerle yüklemeyi [sanal makine ölçeği nde başlangıç komut dosyası](https://azure.microsoft.com/resources/templates/201-vmss-custom-script-windows/) veya [SetupEntryPoint komut dosyası](/azure/service-fabric/service-fabric-application-model)aracılığıyla gerçekleştirebilirsiniz.

Azure için Docker birim [sürücüsünü](https://docs.docker.com/docker-for-azure/persistent-data-volumes/) yüklemek için komut dosyasıörneği aşağıdaki gibidir:

```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

Uygulamalarınızda, yüklediğiniz ses düzeyini veya günlük sürücüsünü kullanmak için, uygulama bildiriminizde **ContainerHostPolicies** altında **Ses** ve **LogConfig** öğelerinde uygun değerleri belirtmeniz gerekir.

```xml
<ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
    <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
    <LogConfig Driver="[YOUR_LOG_DRIVER]" >
        <DriverOption Name="test" Value="vale"/>
    </LogConfig>
    <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
    <Volume Source="[MyStorageVar]" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
    <Volume Source="myvolume1" Destination="c:\testmountlocation2" Driver="[YOUR_VOLUME_DRIVER]" IsReadOnly="true">
        <DriverOption Name="[name]" Value="[value]"/>
    </Volume>
</ContainerHostPolicies>
```

Servis Kumaşı, birim eklentisi belirtirken, belirtilen parametreleri kullanarak otomatik olarak birimi oluşturur. **Birim** öğesinin **Kaynak** etiketi birimin adıdır ve **Sürücü** etiketi birim sürücü eklentisini belirtir. **Hedef** etiketi, **Kaynağın** çalışan kapsayıcı içinde eşlenen konumudur. Bu nedenle, hedefiniz kapsayıcınızın içinde zaten var olan bir konum olamaz. Seçenekler, **DriverOption** etiketi kullanılarak aşağıdaki gibi belirtilebilir:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```

Uygulama parametreleri, önceki bildirim snippet'inde gösterildiği gibi `MyStorageVar` birimler için desteklenir (örnek kullanım için arayın).

Docker günlük sürücüsü belirtilirse, kümedeki günlükleri işlemek için aracıları (veya kapsayıcıları) dağıtmanız gerekir. **Sürücü Seçeneği** etiketi, günlük sürücüsü için seçenekleri belirtmek için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
* Birim sürücüsü de dahil olmak üzere konteyner örneklerini görmek için lütfen [Service Fabric konteyner örneklerini](https://github.com/Azure-Samples/service-fabric-containers) ziyaret edin
* Kapsayıcıları Hizmet Kumaşı kümesine dağıtmak için, [hizmet kumaşında bir kapsayıcı yıdağıt'a](service-fabric-deploy-container.md) bakın
