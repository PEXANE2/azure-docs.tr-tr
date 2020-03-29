---
title: C# ve Kaynak Yöneticisi şablonu kullanarak VM dağıtma
description: Azure VM dağıtmak için C# ve Kaynak Yöneticisi şablonu nasıl kullanılacağını öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: bfba66e8-c923-4df2-900a-0c2643b81240
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 07/14/2017
ms.author: cynthn
ms.openlocfilehash: 6d99c5ae91b80b9b6b9af08001b3a7c57bc7ca8f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78944522"
---
# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>C# ve Kaynak Yöneticisi şablonu kullanarak Azure Sanal Makine dağıtma

Bu makalede, C# kullanarak bir Azure Kaynak Yöneticisi şablonu nasıl dağıtılacağınızda size gösterilmektedir. Oluşturduğunuz şablon, Windows Server'ı tek bir alt ağla çalıştıran tek bir sanal makineyi yeni bir sanal ağda dağıtır.

Sanal makine kaynağının ayrıntılı açıklaması için [Azure Kaynak Yöneticisi şablonundaki Sanal makinelere](template-description.md)bakın. Şablondaki tüm kaynaklar hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi şablonu gözden geçirin.](../../azure-resource-manager/resource-manager-template-walkthrough.md)

Bu adımları yapmak yaklaşık 10 dakika sürer.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

Bu adımda, Visual Studio'nun yüklü olduğundan emin olun ve şablonu dağıtmak için kullanılan bir konsol uygulaması oluşturursunuz.

1. Henüz yapmadıysanız Visual [Studio'yı](https://docs.microsoft.com/visualstudio/install/install-visual-studio)yükleyin. İş Yükleri sayfasında **.NET masaüstü geliştirme** sini seçin ve sonra **Yükle'yi**tıklatın. Özetle, **.NET Framework 4 - 4.6 geliştirme araçlarının** sizin için otomatik olarak seçildiğini görebilirsiniz. Visual Studio'yu zaten yüklediyseniz, Visual Studio Başlatıcısı'nı kullanarak .NET iş yükünü ekleyebilirsiniz.
2. Visual Studio'da **Dosya** > **Yeni** > **Projesi'ni**tıklatın.
3. **Şablonlar** > **Görsel C#** olarak, **Konsol Uygulaması (.NET Framework)** seçin, projenin adı için *myDotnetProject* girin, projenin konumunu seçin ve sonra **Tamam'ı**tıklatın.

## <a name="install-the-packages"></a>Paketleri yükleyin

NuGet paketleri, bu adımları tamamlamak için gereken kitaplıkları yüklemenin en kolay yoludur. Visual Studio'da ihtiyacınız olan kitaplıkları almak için aşağıdaki adımları yapın:

1. **Araçlar** > **Nuget Paket Yöneticisi'ni**tıklatın ve ardından Paket Yöneticisi **Konsolu'nu**tıklatın.
2. Konsola şu komutları yazın:

    ```powershell
    Install-Package Microsoft.Azure.Management.Fluent
    Install-Package WindowsAzure.Storage
    ```

## <a name="create-the-files"></a>Dosyaları oluşturma

Bu adımda, kaynakları dağıtan bir şablon dosyası ve şablona parametre değerleri sağlayan bir parametre dosyası oluşturursunuz. Ayrıca, Azure Kaynak Yöneticisi işlemlerini gerçekleştirmek için kullanılan bir yetkilendirme dosyası da oluşturursunuz.

### <a name="create-the-template-file"></a>Şablon dosyasını oluşturma

1. Çözüm Gezgini'nde *myDotnetProject* > **Yeni Öğe****Ekle'ye** > sağ tıklayın ve ardından Visual *C# Öğeleri'nde* **Metin Dosyası'nı** seçin. *CreateVMTemplate.json*dosyasını adlandırın ve sonra **Ekle'yi**tıklatın.
2. Oluşturduğunuz dosyaya bu JSON kodunu ekleyin:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" }
      },
      "variables": {
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks','myVNet')]", 
        "subnetRef": "[concat(variables('vnetID'),'/subnets/mySubnet')]", 
      },
      "resources": [
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "myPublicIPAddress",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "myresourcegroupdns1"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "myVNet",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "mySubnet",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "myNic",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/publicIPAddresses/', 'myPublicIPAddress')]",
            "[resourceId('Microsoft.Network/virtualNetworks/', 'myVNet')]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": { "id": "[resourceId('Microsoft.Network/publicIPAddresses','myPublicIPAddress')]" },
                  "subnet": { "id": "[variables('subnetRef')]" }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-04-30-preview",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "myVM",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkInterfaces/', 'myNic')]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_DS1" },
            "osProfile": {
              "computerName": "myVM",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "myManagedOSDisk",
                "caching": "ReadWrite",
                "createOption": "FromImage"
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces','myNic')]"
                }
              ]
            }
          }
        }
      ]
    }
    ```

3. CreateVMTemplate.json dosyasını kaydedin.

### <a name="create-the-parameters-file"></a>Parametreler dosyasını oluşturma

Şablondaki kaynak parametreleri için değerleri belirtmek için, değerleri içeren bir parametre dosyası oluşturursunuz.

1. Çözüm Gezgini'nde *myDotnetProject* > **Yeni Öğe****Ekle'ye** > sağ tıklayın ve ardından Visual *C# Öğeleri'nde* **Metin Dosyası'nı** seçin. *Dosyaparametreleri.json'u*adlandırın ve sonra **Ekle'yi**tıklatın.
2. Oluşturduğunuz dosyaya bu JSON kodunu ekleyin:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUserName": { "value": "azureuser" },
        "adminPassword": { "value": "Azure12345678" }
      }
    }
    ```

4. Parameters.json dosyasını kaydedin.

### <a name="create-the-authorization-file"></a>Yetkilendirme dosyasını oluşturma

Bir şablon dağıtmadan önce, [Etkin Dizin hizmet ilkesine](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)erişebildiğinizden emin olun. Hizmet sorumlusundan, taleplerin kimlik doğrulaması için Azure Kaynak Yöneticisi'ne bir belirteci elde elabilirsiniz. Ayrıca uygulama kimliğini, kimlik doğrulama anahtarını ve yetkilendirme dosyasına gereksinim duyduğunuz kiracı kimliğini de kaydetmeniz gerekir.

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
4. Windows'da AZURE_AUTH_LOCATION adlı bir ortam değişkenini oluşturduğunuz yetkilendirme dosyasına tam yol ile ayarlayın, örneğin aşağıdaki PowerShell komutunu kullanabilirsiniz:

    ```powershell
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2019\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```

    

## <a name="create-the-management-client"></a>Yönetim istemcisini oluşturma

1. Oluşturduğunuz proje için Program.cs dosyasını açın. Ardından, dosyanın üst kısmındaki varolan ifadelere bunları kullanarak eklemeekleyin:

    ```csharp
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
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

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Uygulama için değerleri belirtmek için Ana yönteme kod ekleyin:

```csharp
var groupName = "myResourceGroup";
var location = Region.USWest;

var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Şablon ve parametreler Azure'daki bir depolama hesabından dağıtılır. Bu adımda, hesabı oluşturmak ve dosyaları yükleyin. 

Hesabı oluşturmak için bu kodu Ana yönteme ekleyin:

```csharp
string storageAccountName = SdkContext.RandomResourceName("st", 10);

Console.WriteLine("Creating storage account...");
var storage = azure.StorageAccounts.Define(storageAccountName)
    .WithRegion(Region.USWest)
    .WithExistingResourceGroup(resourceGroup)
    .Create();

var storageKeys = storage.GetKeys();
string storageConnectionString = "DefaultEndpointsProtocol=https;"
    + "AccountName=" + storage.Name
    + ";AccountKey=" + storageKeys[0].Value
    + ";EndpointSuffix=core.windows.net";

var account = CloudStorageAccount.Parse(storageConnectionString);
var serviceClient = account.CreateCloudBlobClient();

Console.WriteLine("Creating container...");
var container = serviceClient.GetContainerReference("templates");
container.CreateIfNotExistsAsync().Wait();
var containerPermissions = new BlobContainerPermissions()
    { PublicAccess = BlobContainerPublicAccessType.Container };
container.SetPermissionsAsync(containerPermissions).Wait();

Console.WriteLine("Uploading template file...");
var templateblob = container.GetBlockBlobReference("CreateVMTemplate.json");
templateblob.UploadFromFileAsync("..\\..\\CreateVMTemplate.json").Result();

Console.WriteLine("Uploading parameters file...");
var paramblob = container.GetBlockBlobReference("Parameters.json");
paramblob.UploadFromFileAsync("..\\..\\Parameters.json").Result();
```

## <a name="deploy-the-template"></a>Şablonu dağıtma

Oluşturulan depolama hesabından şablonu ve parametreleri dağıtın. 

Şablonu dağıtmak için bu kodu Ana yönteme ekleyin:

```csharp
var templatePath = "https://" + storageAccountName + ".blob.core.windows.net/templates/CreateVMTemplate.json";
var paramPath = "https://" + storageAccountName + ".blob.core.windows.net/templates/Parameters.json";
var deployment = azure.Deployments.Define("myDeployment")
    .WithExistingResourceGroup(groupName)
    .WithTemplateLink(templatePath, "1.0.0.0")
    .WithParametersLink(paramPath, "1.0.0.0")
    .WithMode(Microsoft.Azure.Management.ResourceManager.Fluent.Models.DeploymentMode.Incremental)
    .Create();
Console.WriteLine("Press enter to delete the resource group...");
Console.ReadLine();
```

## <a name="delete-the-resources"></a>Kaynakları silme

Azure'da kullanılan kaynaklar için ücretlendirildiğiniz için, artık gerekmeyen kaynakları silmek her zaman iyi bir uygulamadır. Her kaynağı bir kaynak grubundan ayrı olarak silmeniz gerekmez. Kaynak grubunu silin ve tüm kaynakları otomatik olarak silinir. 

Kaynak grubunu silmek için bu kodu Ana yönteme ekleyin:

```csharp
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Bu konsol uygulamasının baştan sona tamamen çalışması yaklaşık beş dakika sürer. 

1. Konsol uygulamasını çalıştırmak için **Başlat'ı**tıklatın.

2. Kaynakları silmeye başlamak için **Enter** tuşuna basmadan önce, Azure portalındaki kaynakların oluşturulmasını doğrulamak birkaç dakikanızı alabilir. Dağıtım la ilgili bilgileri görmek için dağıtım durumunu tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

* Dağıtımla ilgili sorunlar varsa, bir sonraki [adım, Azure Kaynak Yöneticisi ile sık karşılaşılan Azure dağıtım hatalarını gidermek](../../resource-manager-common-deployment-errors.md)olacaktır.
* [C# kullanarak bir Azure Sanal Makine dağıt'ı](csharp.md)gözden geçirerek sanal makineyi ve destekleyici kaynaklarını nasıl dağıtılacayacağını öğrenin.