---
title: C# ve Kaynak Yöneticisi şablonu kullanarak VM dağıtma
description: Azure VM dağıtmak için C# ve Kaynak Yöneticisi şablonunun nasıl kullanılacağını öğrenin.
author: cynthn
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 07/14/2017
ms.author: cynthn
ms.openlocfilehash: dfcc0c550af9df6c884c8cd864ed90daf5f78e2f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82082926"
---
# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>C# ve Kaynak Yöneticisi şablonu kullanarak bir Azure sanal makinesini dağıtma

Bu makalede, C# kullanarak Azure Resource Manager şablonunun nasıl dağıtılacağı gösterilir. Oluşturduğunuz şablon, tek bir alt ağa sahip yeni bir sanal ağda Windows Server çalıştıran tek bir sanal makine dağıtır.

Sanal makine kaynağının ayrıntılı açıklaması için, bkz. [Azure Resource Manager şablonundaki sanal makineler](template-description.md). Şablondaki tüm kaynaklar hakkında daha fazla bilgi için bkz. [Azure Resource Manager Template izlenecek yol](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Bu adımların uygulanması yaklaşık 10 dakika sürer.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

Bu adımda, Visual Studio 'nun yüklü olduğundan ve şablonu dağıtmak için kullanılan bir konsol uygulaması oluşturduğunuzdan emin olursunuz.

1. Henüz yapmadıysanız, [Visual Studio 'yu](https://docs.microsoft.com/visualstudio/install/install-visual-studio)yükleyemezsiniz. Iş yükleri sayfasında **.net masaüstü geliştirme** ' yi seçin ve ardından **yükler**' i tıklatın. Özet içinde, **.NET Framework 4-4,6 geliştirme araçlarının** sizin için otomatik olarak seçili olduğunu görebilirsiniz. Visual Studio 'Yu önceden yüklediyseniz, Visual Studio başlatıcısı 'nı kullanarak .NET iş yükünü ekleyebilirsiniz.
2. Visual Studio 'da **Dosya** > **Yeni** > **Proje**' ye tıklayın.
3. **Şablonlar** > **Visual C#**' de **konsol uygulaması (.NET Framework)** seçeneğini belirleyin, projenin adı için *myDotnetProject* girin, projenin konumunu seçin ve ardından **Tamam**' a tıklayın.

## <a name="install-the-packages"></a>Paketleri yükler

NuGet paketleri, bu adımları tamamlaması için gereken kitaplıkları yüklemenin en kolay yoludur. Visual Studio 'da ihtiyacınız olan kitaplıkları almak için şu adımları uygulayın:

1. **Araçlar** > **NuGet Paket Yöneticisi**' ne ve ardından **Paket Yöneticisi konsolu**' na tıklayın.
2. Konsola şu komutları yazın:

    ```powershell
    Install-Package Microsoft.Azure.Management.Fluent
    Install-Package WindowsAzure.Storage
    ```

## <a name="create-the-files"></a>Dosyaları oluşturma

Bu adımda, şablona parametre değerleri sağlayan bir şablon dosyası ve bir parametre dosyası oluşturacaksınız. Ayrıca, Azure Resource Manager işlemleri gerçekleştirmek için kullanılan bir yetkilendirme dosyası da oluşturursunuz.

### <a name="create-the-template-file"></a>Şablon dosyası oluşturma

1. Çözüm Gezgini ' de, *myDotnetProject* > **Add** > **Yeni öğe**Ekle ' ye sağ tıklayın ve ardından *Visual C# öğelerinde* **metin dosyası** ' nı seçin. *Createvmtemplate. JSON*dosyasını adlandırın ve ardından **Ekle**' ye tıklayın.
2. Bu JSON kodunu oluşturduğunuz dosyaya ekleyin:

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

3. CreateVMTemplate. json dosyasını kaydedin.

### <a name="create-the-parameters-file"></a>Parametreler dosyası oluşturma

Şablondaki kaynak parametrelerinin değerlerini belirtmek için değerleri içeren bir parametre dosyası oluşturursunuz.

1. Çözüm Gezgini ' de, *myDotnetProject* > **Add** > **Yeni öğe**Ekle ' ye sağ tıklayın ve ardından *Visual C# öğelerinde* **metin dosyası** ' nı seçin. *Parameters. JSON*dosyasını adlandırın ve ardından **Ekle**' ye tıklayın.
2. Bu JSON kodunu oluşturduğunuz dosyaya ekleyin:

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

4. Parameters. json dosyasını kaydedin.

### <a name="create-the-authorization-file"></a>Yetkilendirme dosyasını oluşturma

Bir şablonu dağıtabilmeniz için önce bir [Active Directory Hizmet sorumlusuna](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)erişiminizin olduğundan emin olun. Hizmet sorumlusu ' ndan Azure Resource Manager kimlik doğrulaması için bir belirteç elde edersiniz. Ayrıca, yetkilendirme dosyasında ihtiyaç duyduğunuz uygulama KIMLIĞI, kimlik doğrulama anahtarı ve kiracı KIMLIĞINI de kaydetmeniz gerekir.

1. Çözüm Gezgini ' de, *myDotnetProject* > **Add** > **Yeni öğe**Ekle ' ye sağ tıklayın ve ardından *Visual C# öğelerinde* **metin dosyası** ' nı seçin. Dosyayı *azureauth. Properties*olarak adlandırın ve **Ekle**' ye tıklayın.
2. Şu yetkilendirme özelliklerini ekleyin:

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

    Abonelik ** &lt;kimliği&gt; ** , abonelik tanımlayıcıınız, ** &lt;uygulama kimliği&gt; ** Active Directory uygulama tanımlayıcısı, ** &lt;kimlik doğrulama anahtarı&gt; ** ve ** &lt;kiracı tanımlayıcısı ile Kiracı kimliği&gt; ** ile değiştirin.

3. Azureauth. Properties dosyasını kaydedin.
4. Windows adlandırılmış AZURE_AUTH_LOCATION içinde, oluşturduğunuz yetkilendirme dosyasının tam yolu ile bir ortam değişkeni ayarlayın, örneğin, aşağıdaki PowerShell komutunu kullanabilirsiniz:

    ```powershell
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2019\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```

    

## <a name="create-the-management-client"></a>Yönetim istemcisi oluşturma

1. Oluşturduğunuz proje için Program.cs dosyasını açın. Ardından, bu using deyimlerini dosyanın en üstündeki mevcut deyimlere ekleyin:

    ```csharp
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

2. Yönetim istemcisini oluşturmak için bu kodu Main yöntemine ekleyin:

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

Uygulamanın değerlerini belirtmek için, ana yönteme kod ekleyin:

```csharp
var groupName = "myResourceGroup";
var location = Region.USWest;

var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Şablon ve parametreler, Azure 'daki bir depolama hesabından dağıtılır. Bu adımda, hesabı oluşturur ve dosyaları karşıya yüklersiniz. 

Hesabı oluşturmak için bu kodu Main yöntemine ekleyin:

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

Şablon ve parametreleri oluşturulan depolama hesabından dağıtın. 

Şablonu dağıtmak için bu kodu Main yöntemine ekleyin:

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

Azure 'da kullanılan kaynaklar için ücretlendirildiğiniz için, artık gerekli olmayan kaynakları silmek her zaman iyi bir uygulamadır. Her kaynağı bir kaynak grubundan ayrı olarak silmeniz gerekmez. Kaynak grubunu silin ve tüm kaynakları otomatik olarak silinir. 

Kaynak grubunu silmek için, bu kodu Main yöntemine ekleyin:

```csharp
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Bu konsol uygulamasının başlangıçtan sonuna kadar tam olarak çalıştırılması yaklaşık beş dakika sürer. 

1. Konsol uygulamasını çalıştırmak için **Başlat**' a tıklayın.

2. Kaynakları silmeye başlamak üzere **ENTER** tuşuna basmadan önce Azure Portal kaynakların oluşturulmasını doğrulamak birkaç dakika sürebilir. Dağıtım hakkındaki bilgileri görmek için dağıtım durumuna tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

* Dağıtım ile ilgili sorunlar varsa, bir sonraki adım [Azure Resource Manager Genel Azure dağıtım hatalarıyla Ilgili sorunları giderme](../../resource-manager-common-deployment-errors.md)bölümüne bakmak olacaktır.
* [C# kullanarak bir Azure sanal makinesini dağıtmayı](csharp.md)inceleyerek bir sanal makineyi ve destekleyici kaynaklarını dağıtmayı öğrenin.