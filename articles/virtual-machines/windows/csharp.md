---
title: 'C kullanarak Bir Azure Sanal Makinesi Oluşturma ve Yönetme #'
description: Sanal bir makineyi ve tüm destekleyici kaynaklarını dağıtmak için C# ve Azure Kaynak Yöneticisi'ni kullanın.
author: cynthn
ms.service: virtual-machines-windows
ms.workload: infrastructure
ms.topic: article
ms.date: 07/17/2017
ms.author: cynthn
ms.openlocfilehash: 07c66b2955f3df1ffae1a0cb0c2b0888bdc790e9
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82082892"
---
# <a name="create-and-manage-windows-vms-in-azure-using-c"></a>C kullanarak Azure'da Windows VM'leri oluşturma ve yönetme # #

[Azure Sanal Makine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) birkaç destekleyici Azure kaynağına ihtiyaç duyar. Bu makalede, C# kullanarak VM kaynakları oluşturma, yönetme ve silme yer alıyor. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Visual Studio projesi oluşturma
> * Paketi yükleyin
> * Kimlik bilgileri oluşturma
> * Kaynak oluşturma
> * Yönetim görevlerini gerçekleştirme
> * Kaynakları silme
> * Uygulamayı çalıştırma

Bu adımları yapmak yaklaşık 20 dakika sürer.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Henüz yapmadıysanız Visual [Studio'yı](https://docs.microsoft.com/visualstudio/install/install-visual-studio)yükleyin. İş Yükleri sayfasında **.NET masaüstü geliştirme** sini seçin ve sonra **Yükle'yi**tıklatın. Özetle, **.NET Framework 4 - 4.6 geliştirme araçlarının** sizin için otomatik olarak seçildiğini görebilirsiniz. Visual Studio'yu zaten yüklediyseniz, Visual Studio Başlatıcısı'nı kullanarak .NET iş yükünü ekleyebilirsiniz.
2. Visual Studio'da **Dosya** > **Yeni** > **Projesi'ni**tıklatın.
3. **Şablonlar** > **Görsel C#** olarak, **Konsol Uygulaması (.NET Framework)** seçin, projenin adı için *myDotnetProject* girin, projenin konumunu seçin ve sonra **Tamam'ı**tıklatın.

## <a name="install-the-package"></a>Paketi yükleyin

NuGet paketleri, bu adımları tamamlamak için gereken kitaplıkları yüklemenin en kolay yoludur. Visual Studio'da ihtiyacınız olan kitaplıkları almak için aşağıdaki adımları yapın:

1. **Araçlar** > **Nuget Paket Yöneticisi'ni**tıklatın ve ardından Paket Yöneticisi **Konsolu'nu**tıklatın.
2. Konsola bu komutu yazın:

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    ```

## <a name="create-credentials"></a>Kimlik bilgileri oluşturma

Bu adımı başlatmadan önce, [Etkin Dizin hizmet ilkesine](../../active-directory/develop/howto-create-service-principal-portal.md)erişebildiğinizden emin olun. Ayrıca uygulama kimliğini, kimlik doğrulama anahtarını ve daha sonraki bir adımda gereksinim duyduğunuz kiracı kimliğini de kaydetmeniz gerekir.

### <a name="create-the-authorization-file"></a>Yetkilendirme dosyasını oluşturma

1. Çözüm Gezgini'nde *myDotnetProject* > **Yeni Öğe****Ekle'ye** > sağ tıklayın ve ardından Visual *C# Öğeleri'nde* **Metin Dosyası'nı** seçin. *Azureauth.properties*dosyasını adlandırın ve sonra **Ekle'yi**tıklatın.
2. Bu yetkilendirme özelliklerini ekleyin:

    ```
    subscription=<subscription-id>
    client=<application-id>
    key=<authentication-key>
    tenant=<tenant-id>
    managementURI=https://management.core.windows.net/
    baseURL=https://management.azure.com/
    authURL=https://login.windows.net/
    graphURL=https://graph.microsoft.com/
    ```

    ** &lt;Abonelik kimliğiniz&gt; abonelik** tanımlayıcınızla, ** &lt;uygulama kimliğini&gt; ** Active Directory uygulama tanımlayıcısıyla, ** &lt;kimlik doğrulama anahtarını&gt; ** uygulama anahtarıyla ve ** &lt;kiracı&gt; ** kimliğiyle kiracı kimliğiyle değiştirin.

3. azureauth.properties dosyasını kaydedin. 
4. Windows'da AZURE_AUTH_LOCATION adlı bir ortam değişkenini, oluşturduğunuz yetkilendirme dosyasına tam yol ile ayarlayın. Örneğin, aşağıdaki PowerShell komutu kullanılabilir:

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2019\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```

### <a name="create-the-management-client"></a>Yönetim istemcisini oluşturma

1. Oluşturduğunuz proje için Program.cs dosyasını açın. Ardından, dosyanın üst kısmındaki varolan ifadelere bunları kullanarak eklemeekleyin:

    ```csharp
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    ```

2. Yönetim istemcisini oluşturmak için bu kodu Ana yönteme ekleyin:

    ```csharp
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-resources"></a>Kaynak oluşturma

### <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Tüm kaynaklar bir [Kaynak grubunda](../../azure-resource-manager/management/overview.md)yer almalıdır.

Uygulama için değerleri belirtmek ve kaynak grubu oluşturmak için bu kodu Ana yönteme ekleyin:

```csharp
var groupName = "myResourceGroup";
var vmName = "myVM";
var location = Region.USWest;
    
Console.WriteLine("Creating resource group...");
var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

### <a name="create-the-availability-set"></a>Kullanılabilirlik kümesini oluşturma

[Kullanılabilirlik kümeleri,](tutorial-availability-sets.md) uygulamanız tarafından kullanılan sanal makineleri korumanızı kolaylaştırır.

Kullanılabilirlik kümesini oluşturmak için bu kodu Ana yönteme ekleyin:

```csharp
Console.WriteLine("Creating availability set...");
var availabilitySet = azure.AvailabilitySets.Define("myAVSet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithSku(AvailabilitySetSkuTypes.Managed)
    .Create();
```

### <a name="create-the-public-ip-address"></a>Genel IP adresini oluşturma

Sanal makineyle iletişim kurmak için [genel bir IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) gereklidir.

Sanal makinenin genel IP adresini oluşturmak için bu kodu Ana yönteme ekleyin:
   
```csharp
Console.WriteLine("Creating public IP address...");
var publicIPAddress = azure.PublicIPAddresses.Define("myPublicIP")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithDynamicIP()
    .Create();
```

### <a name="create-the-virtual-network"></a>Sanal ağ oluşturma

Sanal bir makine, [Sanal ağın](../../virtual-network/virtual-networks-overview.md)alt ağında olmalıdır.

Bir alt ağ ve sanal ağ oluşturmak için bu kodu Ana yönteme ekleyin:

```csharp
Console.WriteLine("Creating virtual network...");
var network = azure.Networks.Define("myVNet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithAddressSpace("10.0.0.0/16")
    .WithSubnet("mySubnet", "10.0.0.0/24")
    .Create();
```

### <a name="create-the-network-interface"></a>Ağ arabirimini oluşturma

Sanal makinenin sanal ağda iletişim kurabilmesi için ağ arabirimine ihtiyacı vardır.

Ağ arabirimi oluşturmak için bu kodu Ana yönteme ekleyin:

```csharp
Console.WriteLine("Creating network interface...");
var networkInterface = azure.NetworkInterfaces.Define("myNIC")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetwork(network)
    .WithSubnet("mySubnet")
    .WithPrimaryPrivateIPAddressDynamic()
    .WithExistingPrimaryPublicIPAddress(publicIPAddress)
    .Create();
 ```

### <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

Artık tüm destekleyici kaynakları oluşturduğunuziçin, sanal bir makine oluşturabilirsiniz.

Sanal makineyi oluşturmak için bu kodu Ana yönteme ekleyin:

```csharp
Console.WriteLine("Creating virtual machine...");
azure.VirtualMachines.Define(vmName)
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .WithAdminUsername("azureuser")
    .WithAdminPassword("Azure12345678")
    .WithComputerName(vmName)
    .WithExistingAvailabilitySet(availabilitySet)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

> [!NOTE]
> Bu öğretici, Windows Server işletim sisteminin bir sürümünü çalıştıran sanal bir makine oluşturur. Diğer resimleri seçme hakkında daha fazla bilgi edinmek için [Windows PowerShell ve Azure CLI ile Gezin ve Azure sanal makine resimlerini seçin.](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
>

Pazar yeri görüntüsü yerine varolan bir disk kullanmak istiyorsanız, şu kodu kullanın:

```csharp
var managedDisk = azure.Disks.Define("myosdisk")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithWindowsFromVhd("https://mystorage.blob.core.windows.net/vhds/myosdisk.vhd")
    .WithSizeInGB(128)
    .WithSku(DiskSkuTypes.PremiumLRS)
    .Create();

azure.VirtualMachines.Define("myVM")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithSpecializedOSDisk(managedDisk, OperatingSystemTypes.Windows)
    .WithExistingAvailabilitySet(availabilitySet)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

## <a name="perform-management-tasks"></a>Yönetim görevlerini gerçekleştirme

Bir sanal makinenin yaşam döngüsü boyunca, sanal makineyi başlatmak, durdurmak veya silmek gibi yönetim görevleri gerçekleştirmek isteyebilirsiniz. Ayrıca, yinelenen veya karmaşık görevleri otomatikleştirmek için kod oluşturmak isteyebilirsiniz.

VM ile bir şey yapmanız gerektiğinde, bunun bir örneğini almanız gerekir:

```csharp
var vm = azure.VirtualMachines.GetByResourceGroup(groupName, vmName);
```

### <a name="get-information-about-the-vm"></a>VM hakkında bilgi alın

Sanal makine hakkında bilgi almak için bu kodu Ana yönteme ekleyin:

```csharp
Console.WriteLine("Getting information about the virtual machine...");
Console.WriteLine("hardwareProfile");
Console.WriteLine("   vmSize: " + vm.Size);
Console.WriteLine("storageProfile");
Console.WriteLine("  imageReference");
Console.WriteLine("    publisher: " + vm.StorageProfile.ImageReference.Publisher);
Console.WriteLine("    offer: " + vm.StorageProfile.ImageReference.Offer);
Console.WriteLine("    sku: " + vm.StorageProfile.ImageReference.Sku);
Console.WriteLine("    version: " + vm.StorageProfile.ImageReference.Version);
Console.WriteLine("  osDisk");
Console.WriteLine("    osType: " + vm.StorageProfile.OsDisk.OsType);
Console.WriteLine("    name: " + vm.StorageProfile.OsDisk.Name);
Console.WriteLine("    createOption: " + vm.StorageProfile.OsDisk.CreateOption);
Console.WriteLine("    caching: " + vm.StorageProfile.OsDisk.Caching);
Console.WriteLine("osProfile");
Console.WriteLine("  computerName: " + vm.OSProfile.ComputerName);
Console.WriteLine("  adminUsername: " + vm.OSProfile.AdminUsername);
Console.WriteLine("  provisionVMAgent: " + vm.OSProfile.WindowsConfiguration.ProvisionVMAgent.Value);
Console.WriteLine("  enableAutomaticUpdates: " + vm.OSProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);
Console.WriteLine("networkProfile");
foreach (string nicId in vm.NetworkInterfaceIds)
{
    Console.WriteLine("  networkInterface id: " + nicId);
}
Console.WriteLine("vmAgent");
Console.WriteLine("  vmAgentVersion" + vm.InstanceView.VmAgent.VmAgentVersion);
Console.WriteLine("    statuses");
foreach (InstanceViewStatus stat in vm.InstanceView.VmAgent.Statuses)
{
    Console.WriteLine("    code: " + stat.Code);
    Console.WriteLine("    level: " + stat.Level);
    Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
    Console.WriteLine("    message: " + stat.Message);
    Console.WriteLine("    time: " + stat.Time);
}
Console.WriteLine("disks");
foreach (DiskInstanceView disk in vm.InstanceView.Disks)
{
    Console.WriteLine("  name: " + disk.Name);
    Console.WriteLine("  statuses");
    foreach (InstanceViewStatus stat in disk.Statuses)
    {
        Console.WriteLine("    code: " + stat.Code);
        Console.WriteLine("    level: " + stat.Level);
        Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
        Console.WriteLine("    time: " + stat.Time);
    }
}
Console.WriteLine("VM general status");
Console.WriteLine("  provisioningStatus: " + vm.ProvisioningState);
Console.WriteLine("  id: " + vm.Id);
Console.WriteLine("  name: " + vm.Name);
Console.WriteLine("  type: " + vm.Type);
Console.WriteLine("  location: " + vm.Region);
Console.WriteLine("VM instance status");
foreach (InstanceViewStatus stat in vm.InstanceView.Statuses)
{
    Console.WriteLine("  code: " + stat.Code);
    Console.WriteLine("  level: " + stat.Level);
    Console.WriteLine("  displayStatus: " + stat.DisplayStatus);
}
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="stop-the-vm"></a>VM’yi durdurma

Sanal bir makineyi durdurabilir ve tüm ayarlarını saklayabilirsiniz, ancak bunun için ücretlendirilmeye devam edebilir veya sanal bir makineyi durdurup yerini saptayabilirsiniz. Sanal bir makine devre dışı edildiğinde, onunla ilişkili tüm kaynaklar da ayrılır ve faturalandırma sona erer.

Sanal makineyi ayırmadan durdurmak için bu kodu Ana yönteme ekleyin:

```csharp
Console.WriteLine("Stopping vm...");
vm.PowerOff();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

Sanal makineyi bulmak istiyorsanız, PowerOff çağrısını bu kodla değiştirin:

```csharp
vm.Deallocate();
```

### <a name="start-the-vm"></a>VM’yi başlatma

Sanal makineyi başlatmak için bu kodu Ana yönteme ekleyin:

```csharp
Console.WriteLine("Starting vm...");
vm.Start();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="resize-the-vm"></a>VM'yi yeniden boyutlandırın

Sanal makineniz için bir boyut alırken dağıtımın birçok yönü göz önünde bulundurulmalıdır. Daha fazla bilgi için [VM boyutlarına](sizes.md)bakın.  

Sanal makinenin boyutunu değiştirmek için bu kodu Ana yönteme ekleyin:

```csharp
Console.WriteLine("Resizing vm...");
vm.Update()
    .WithSize(VirtualMachineSizeTypes.StandardDS2) 
    .Apply();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="add-a-data-disk-to-the-vm"></a>VM’ye veri diski ekleme

Sanal makineye bir veri diski eklemek için bu kodu Ana yönteme ekleyin. Bu örnek, 2 GB boyutunda bir veri diski, 0 LUN han ve ReadWrite önbelleğe alma türü ekler:

```csharp
Console.WriteLine("Adding data disk to vm...");
vm.Update()
    .WithNewDataDisk(2, 0, CachingTypes.ReadWrite) 
    .Apply();
Console.WriteLine("Press enter to delete resources...");
Console.ReadLine();
```

## <a name="delete-resources"></a>Kaynakları silme

Azure'da kullanılan kaynaklar için ücretlendirildiğiniz için, artık gerekmeyen kaynakları silmek her zaman iyi bir uygulamadır. Sanal makineleri ve tüm destekleyici kaynakları silmek istiyorsanız, tek yapmanız gereken kaynak grubunu silmektir.

Kaynak grubunu silmek için bu kodu Ana yönteme ekleyin:

```csharp
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Bu konsol uygulamasının baştan sona tamamen çalışması yaklaşık beş dakika sürer. 

1. Konsol uygulamasını çalıştırmak için **Başlat'ı**tıklatın.

2. Kaynakları silmeye başlamak için **Enter** tuşuna basmadan önce, Azure portalındaki kaynakların oluşturulmasını doğrulamak birkaç dakikanızı alabilir. Dağıtım la ilgili bilgileri görmek için dağıtım durumunu tıklatın.

## <a name="next-steps"></a>Sonraki adımlar
* [C# ve Kaynak Yöneticisi şablonu kullanarak Azure Sanal Makine](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)dağıt'taki bilgileri kullanarak sanal bir makine oluşturmak için şablon kullanmanın avantajını kullan.
* [.NET için Azure kitaplıklarını](https://docs.microsoft.com/dotnet/azure/?view=azure-dotnet)kullanma hakkında daha fazla bilgi edinin.
