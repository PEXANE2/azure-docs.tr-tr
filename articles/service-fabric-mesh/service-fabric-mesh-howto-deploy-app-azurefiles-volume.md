---
title: Service Fabric Mesh uygulamasında Azure Dosyaları tabanlı birim kullanma
description: Azure CLI'yi kullanarak bir hizmetin içine Azure Dosyaları tabanlı bir birim monte ederek bir Azure Hizmet Kumaş Kafesi uygulamasında durumu nasıl depolayabilirsiniz öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 11/21/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: 5bb7ab6c861d958f6811ca852363c59cfced3940
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76718829"
---
# <a name="mount-an-azure-files-based-volume-in-a-service-fabric-mesh-application"></a>Service Fabric Mesh uygulamasında Azure Dosyaları tabanlı bir hacim ekleme 

Bu makalede, Bir Hizmet Kumaş Kafesi uygulamasının bir hizmetine Azure Dosyaları tabanlı bir birimin nasıl monte edilebildiğini açıklanmaktadır.  Azure Dosyaları toplu sürücü, hizmet durumunu sürdürmek için kullandığınız bir kapsayıcıya Azure Dosyaları paylaşımını monte etmek için kullanılan bir Docker birim sürücüsüdür. Birimler size genel amaçlı dosya depolama sağlar ve normal disk G/Ç dosya API'lerini kullanarak dosyaları okuma/yazmanızı sağlar.  Birim ve uygulama verilerini depolama seçenekleri hakkında daha fazla bilgi edinmek için, [depolama durumunu](service-fabric-mesh-storing-state.md)okuyun.

Bir hizmete birim monte etmek için, Service Fabric Mesh uygulamanızda bir birim kaynağı oluşturun ve ardından hizmetinizdeki bu topluya başvurun.  Birim kaynağının beyan edilmesi ve hizmet kaynağına başvurması [YAML tabanlı kaynak dosyalarında](#declare-a-volume-resource-and-update-the-service-resource-yaml) veya [JSON tabanlı dağıtım şablonunda](#declare-a-volume-resource-and-update-the-service-resource-json)yapılabilir. Birimi artırmadan önce, önce bir Azure depolama hesabı ve [Azure Dosyaları'nda](/azure/storage/files/storage-how-to-create-file-share)bir dosya paylaşımı oluşturun.

## <a name="prerequisites"></a>Ön koşullar
> [!NOTE]
> **Windows RS5 geliştirme makinesinde dağıtım ile bilinen Sorun:** Azuredosya Birimlerinin montajını engelleyen RS5 Windows makinelerinde Powershell cmdlet New-SmbGlobalMapping ile açık hata vardır. Aşağıda, AzureFile tabanlı birim yerel geliştirme makinesine monte edilirken karşılaşılan örnek hatadır.
```
Error event: SourceId='System.Hosting', Property='CodePackageActivation:counterService:EntryPoint:131884291000691067'.
There was an error during CodePackage activation.System.Fabric.FabricException (-2147017731)
Failed to start Container. ContainerName=sf-2-63fc668f-362d-4220-873d-85abaaacc83e_6d6879cf-dd43-4092-887d-17d23ed9cc78, ApplicationId=SingleInstance_0_App2, ApplicationName=fabric:/counterApp. DockerRequest returned StatusCode=InternalServerError with ResponseBody={"message":"error while mounting volume '': mount failed"}
```
Sorun için geçici çözüm 1) Powershell yöneticisi olarak komutun altında çalıştırın ve 2) Makineyi yeniden başlatın.
```powershell
PS C:\WINDOWS\system32> Mofcomp c:\windows\system32\wbem\smbwmiv2.mof
```

Bu makaleyi tamamlamak için Azure Bulut Kabuğu'nu veya Azure CLI'nin yerel yüklemesini kullanabilirsiniz. 

Azure CLI'yi bu makalede yerel olarak `az --version` kullanmak `azure-cli (2.0.43)`için en az .  Azure Hizmet Kumaş ıstıraki CLI uzantı [modüllerini](service-fabric-mesh-howto-setup-cli.md)bu yönergeleri izleyerek yükleyin (veya güncelleyin).

Azure'da oturum açmak ve aboneliğinizi ayarlamak için:

```azurecli
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-a-storage-account-and-file-share-optional"></a>Depolama hesabı ve dosya paylaşımı oluşturma (isteğe bağlı)
Azure Dosyaları birimini oluşturmak için bir depolama hesabı ve dosya paylaşımı gerekir.  Varolan bir Azure depolama hesabı ve dosya paylaşımı kullanabilir veya kaynak oluşturabilirsiniz:

```azurecli-interactive
az group create --name myResourceGroup --location eastus

az storage account create --name myStorageAccount --resource-group myResourceGroup --location eastus --sku Standard_LRS --kind StorageV2

$current_env_conn_string=$(az storage account show-connection-string -n myStorageAccount -g myResourceGroup --query 'connectionString' -o tsv)

az storage share create --name myshare --quota 2048 --connection-string $current_env_conn_string
```

## <a name="get-the-storage-account-name-and-key-and-the-file-share-name"></a>Depolama hesabı adını, anahtarı nı ve dosya paylaşım adını alma
Depolama hesabı adı, depolama hesabı anahtarı ve dosya paylaşım `<storageAccountName>` `<storageAccountKey>`adı `<fileShareName>` , , olarak başvurulan ve aşağıdaki bölümlerde. 

Depolama hesaplarınızı listele ve kullanmak istediğiniz dosya paylaşımıyla depolama hesabının adını alın:
```azurecli-interactive
az storage account list
```

Dosya paylaşımının adını alın:
```azurecli-interactive
az storage share list --account-name <storageAccountName>
```

Depolama hesabı anahtarını ("key1") alın:
```azurecli-interactive
az storage account keys list --account-name <storageAccountName> --query "[?keyName=='key1'].value"
```

Bu değerleri [Azure portalında](https://portal.azure.com)da bulabilirsiniz:
* `<storageAccountName>`- **Depolama Hesapları**altında, dosya paylaşımı oluşturmak için kullanılan depolama hesabının adı.
* `<storageAccountKey>`- **Depolama Hesapları** altında depolama hesabınızı seçin ve ardından **Access tuşlarını** seçin ve **değeri tuş1**altında kullanın.
* `<fileShareName>`- **Depolama Hesapları** altında depolama hesabınızı seçin ve ardından **Dosyalar'ı**seçin. Kullanılacak ad, oluşturduğunuz dosya paylaşımının adıdır.

## <a name="declare-a-volume-resource-and-update-the-service-resource-json"></a>Birim kaynağı bildirin ve hizmet kaynağını güncelleştirin (JSON)

Bir önceki `<fileShareName>`adımda `<storageAccountKey>` bulduğunuz , ve `<storageAccountName>`değerler için parametreler ekleyin. 

Uygulama kaynağının eş olarak Birim kaynak oluşturun. Azure Dosyaları tabanlı birimi kullanmak için bir ad ve sağlayıcı belirtin ("SFAzureFile"). Içinde `azureFileParameters`, `<fileShareName>`bir önceki `<storageAccountName>`adımda `<storageAccountKey>` bulduğunuz , ve değerleri parametrelerini belirtin.

Hizmetinizde ses düzeyini monte etmek `volumeRefs` için, hizmet öğesine `codePackages` bir öğe ekleyin.  `name`birim (veya birim kaynak için dağıtım şablonu parametresi) için kaynak kimliği ve birim.yaml kaynak dosyasında bildirilen birimin adıdır.  `destinationPath`hacmin monte edileceği yerel dizinidir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "defaultValue": "EastUS",
      "type": "String",
      "metadata": {
        "description": "Location of the resources."
      }
    },
    "fileShareName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Azure Files file share that provides the volume for the container."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Azure storage account that contains the file share."
      }
    },
    "storageAccountKey": {
      "type": "securestring",
      "metadata": {
        "description": "Access key for the Azure storage account that contains the file share."
      }
    },
    "stateFolderName": {
      "type": "string",
      "defaultValue": "TestVolumeData",
      "metadata": {
        "description": "Folder in which to store the state. Provide an empty value to create a unique folder for each container to store the state. A non-empty value will retain the state across deployments, however if more than one applications are using the same folder, the counter may update more frequently."
      }
    }
  },
  "resources": [
    {
      "apiVersion": "2018-09-01-preview",
      "name": "VolumeTest",
      "type": "Microsoft.ServiceFabricMesh/applications",
      "location": "[parameters('location')]",
      "dependsOn": [
        "Microsoft.ServiceFabricMesh/networks/VolumeTestNetwork",
        "Microsoft.ServiceFabricMesh/volumes/testVolume"
      ],
      "properties": {
        "services": [
          {
            "name": "VolumeTestService",
            "properties": {
              "description": "VolumeTestService description.",
              "osType": "Windows",
              "codePackages": [
                {
                  "name": "VolumeTestService",
                  "image": "volumetestservice:dev",
                  "volumeRefs": [
                    {
                      "name": "[resourceId('Microsoft.ServiceFabricMesh/volumes', 'testVolume')]",
                      "destinationPath": "C:\\app\\data"
                    }
                  ],
                  "environmentVariables": [
                    {
                      "name": "ASPNETCORE_URLS",
                      "value": "http://+:20003"
                    },
                    {
                      "name": "STATE_FOLDER_NAME",
                      "value": "[parameters('stateFolderName')]"
                    }
                  ],
                  ...
                }
              ],
              ...
            }
          }
        ],
        "description": "VolumeTest description."
      }
    },
    {
      "apiVersion": "2018-09-01-preview",
      "name": "testVolume",
      "type": "Microsoft.ServiceFabricMesh/volumes",
      "location": "[parameters('location')]",
      "dependsOn": [],
      "properties": {
        "description": "Azure Files storage volume for the test application.",
        "provider": "SFAzureFile",
        "azureFileParameters": {
          "shareName": "[parameters('fileShareName')]",
          "accountName": "[parameters('storageAccountName')]",
          "accountKey": "[parameters('storageAccountKey')]"
        }
      }
    }
    ...
  ]
}
```

## <a name="declare-a-volume-resource-and-update-the-service-resource-yaml"></a>Birim kaynağı bildirin ve hizmet kaynağını (YAML) güncelleştirin

Uygulamanız için *Uygulama kaynakları* dizinine yeni bir *volume.yaml* dosyası ekleyin.  Azure Dosyaları tabanlı birimi kullanmak için bir ad ve sağlayıcı belirtin ("SFAzureFile"). `<fileShareName>`, `<storageAccountName>`ve `<storageAccountKey>` önceki adımda bulduğunuz değerlerdir.

```yaml
volume:
  schemaVersion: 1.0.0-preview2
  name: testVolume
  properties:
    description: Azure Files storage volume for counter App.
    provider: SFAzureFile
    azureFileParameters: 
        shareName: <fileShareName>
        accountName: <storageAccountName>
        accountKey: <storageAccountKey>
```

*Servis Kaynakları* dizinindeki *service.yaml* dosyasını güncelleştirin ve ses düzeyini hizmetinize bağla.  Öğeye `volumeRefs` öğeyi `codePackages` ekleyin.  `name`birim (veya birim kaynak için dağıtım şablonu parametresi) için kaynak kimliği ve birim.yaml kaynak dosyasında bildirilen birimin adıdır.  `destinationPath`hacmin monte edileceği yerel dizinidir.

```yaml
## Service definition ##
application:
  schemaVersion: 1.0.0-preview2
  name: VolumeTest
  properties:
    services:
      - name: VolumeTestService
        properties:
          description: VolumeTestService description.
          osType: Windows
          codePackages:
            - name: VolumeTestService
              image: volumetestservice:dev
              volumeRefs:
                - name: "[resourceId('Microsoft.ServiceFabricMesh/volumes', 'testVolume')]"
                  destinationPath: C:\app\data
              endpoints:
                - name: VolumeTestServiceListener
                  port: 20003
              environmentVariables:
                - name: ASPNETCORE_URLS
                  value: http://+:20003
                - name: STATE_FOLDER_NAME
                  value: TestVolumeData
              resources:
                requests:
                  cpu: 0.5
                  memoryInGB: 1
          replicaCount: 1
          networkRefs:
            - name: VolumeTestNetwork
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure Dosyaları toplu örnek uygulamasını [GitHub'da](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter)görüntüleyin.
- Service Fabric Kaynak Modeli hakkında daha fazla bilgi için bkz. [Service Fabric Mesh Kaynak Modeli](service-fabric-mesh-service-fabric-resources.md).
- Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh’e genel bakış](service-fabric-mesh-overview.md) makalesini okuyun.
