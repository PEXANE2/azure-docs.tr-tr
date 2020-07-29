---
title: Şablonlarda dağıtım betikleri için geliştirme ortamını yapılandırma | Microsoft Docs
description: Azure Resource Manager şablonlarındaki dağıtım betikleri için geliştirme ortamını yapılandırın.
services: azure-resource-manager
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/27/2020
ms.author: jgao
ms.openlocfilehash: 232a1ae5d125a2ea1d5723e85073fb3dd02420cc
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87294478"
---
# <a name="configure-development-environment-for-deployment-scripts-in-templates-preview"></a>Şablonlarda dağıtım betikleri için geliştirme ortamını yapılandırma (Önizleme)

Dağıtım komut dosyalarını bir dağıtım betiği görüntüsüyle geliştirmek ve test etmek için bir geliştirme ortamı oluşturmayı öğrenin. [Azure Container Instance](../../container-instances/container-instances-overview.md) oluşturabilir ya da [Docker](https://docs.docker.com/get-docker/)kullanabilirsiniz. Her ikisi de bu makalede ele alınmıştır.

## <a name="prerequisites"></a>Önkoşullar

Dağıtım betiğinizin yoksa, aşağıdaki içeriğe sahip bir **hello.ps1** dosyası oluşturabilirsiniz:

```powershell
param([string] $name)
$output = 'Hello {0}' -f $name
Write-Output $output
$DeploymentScriptOutputs = @{}
$DeploymentScriptOutputs['text'] = $output
```

## <a name="use-azure-container-instance"></a>Azure Container Instance 'ı kullanma

Betiklerinizi bilgisayarınızda yazmak için bir depolama hesabı oluşturmanız ve depolama hesabını kapsayıcı örneğine bağlamanız gerekir. Böylece komut dosyanızı depolama hesabına yükleyebilir ve betiği kapsayıcı örneğinde çalıştırabilirsiniz.

> [!NOTE]
> Komut dosyanızı test etmek için oluşturduğunuz depolama hesabı, dağıtım betiği hizmetinin betiği yürütmek için kullandığı depolama hesabı değildir. Dağıtım betiği hizmeti, her yürütmede dosya paylaşma olarak benzersiz bir ad oluşturur.

### <a name="create-an-azure-container-instance"></a>Azure Container Instance oluşturma

Aşağıdaki ARM şablonu bir kapsayıcı örneği ve bir dosya paylaşma oluşturup dosya paylaşımının kapsayıcı görüntüsüne takar.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "projectName": {
      "type": "string",
      "metadata": {
        "description": "Specify a project name that is used for generating resource names."
      }
    },
    "containerImage": {
      "type": "string",
      "defaultValue": "mcr.microsoft.com/azuredeploymentscripts-powershell:az4.3",
      "metadata": {
        "description": "Specify the container image."
      }
    },
    "mountPath": {
      "type": "string",
      "defaultValue": "deploymentScript",
      "metadata": {
        "description": "Specify the mount path."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(parameters('projectName'), 'store')]",
    "fileShareName": "[concat(parameters('projectName'), 'share')]",
    "containerGroupName": "[concat(parameters('projectName'), 'cg')]",
    "containerName": "[concat(parameters('projectName'), 'container')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-06-01",
      "name": "[variables('storageAccountName')]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "StorageV2",
      "properties": {
        "accessTier": "Hot"
      }
    },
    {
      "type": "Microsoft.Storage/storageAccounts/fileServices/shares",
      "apiVersion": "2019-06-01",
      "name": "[concat(variables('storageAccountName'), '/default/', variables('fileShareName'))]",
      "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
      ]
    },
    {
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2019-12-01",
      "name": "[variables('containerGroupName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
      ],
      "properties": {
        "containers": [
          {
            "name": "[variables('containerName')]",
            "properties": {
              "image": "[parameters('containerImage')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "protocol": "TCP",
                  "port": 80
                }
              ],
              "volumeMounts": [
                {
                  "name": "filesharevolume",
                  "mountPath": "[parameters('mountPath')]"
                }
              ],
              "command": [
                "/bin/sh",
                "-c",
                "pwsh -c 'Start-Sleep -Seconds 1800'"
              ]
            }
          }
        ],
        "osType": "Linux",
        "volumes": [
          {
            "name": "filesharevolume",
            "azureFile": {
              "readOnly": false,
              "shareName": "[variables('fileShareName')]",
              "storageAccountName": "[variables('storageAccountName')]",
              "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')), '2019-06-01').keys[0].value]"
            }
          }
        ]
      }
    }
  ]
}
```
Bağlama yolu için varsayılan değer **Deploymentscript**' dir.  Bu, kapsayıcı örneğindeki yoldur ve dosya paylaşımında bağlanır.

Şablonda belirtilen varsayılan kapsayıcı görüntüsü **MCR.Microsoft.com/azuredeploymentscripts-PowerShell:AZ4.3 "** dir.  Desteklenen Azure PowerShell sürümlerinin ve Azure CLı sürümlerinin listesi için bkz. [Azure PowerShell veya Azure CLI](./deployment-script-template.md#prerequisites).

Şablon, 1800 saniye kapsayıcı örneğini askıya alır. Kapsayıcı örneği Terminal durumuna geçmeden önce 30 dakika, oturum sona erer.

Şablonu dağıtmak için:

```azurepowershell
$projectName = Read-Host -Prompt "Enter a project name that is used to generate resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateFile = Read-Host -Prompt "Enter the template file path and file name"
$resourceGroupName = "${projectName}rg"

New-azResourceGroup -Location $location -name $resourceGroupName
New-AzResourceGroupDeployment -resourceGroupName $resourceGroupName -TemplateFile $templatefile -projectName $projectName
```

### <a name="upload-deployment-script"></a>Dağıtım betiğini karşıya yükle

Dağıtım betiğinizi depolama hesabına yükleyin. Aşağıda bir PowerShell örneği verilmiştir:

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name that you used earlier"
$fileName = Read-Host -Prompt "Enter the deployment script file name with the path"

$resourceGroupName = "${projectName}rg"
$storageAccountName = "${projectName}store"
$fileShareName = "${projectName}share"

$context = (Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName).Context
Set-AzStorageFileContent -Context $context -ShareName $fileShareName -Source $fileName -Force
```

Ayrıca, Azure portal ve Azure CLı kullanarak dosyayı karşıya yükleyebilirsiniz.

### <a name="test-the-deployment-script"></a>Dağıtım betiğini test etme

1. Azure portal, kapsayıcı örneğini ve depolama hesabını dağıttığınız kaynak grubunu açın.
1. Kapsayıcı grubunu açın. Varsayılan kapsayıcı grubu adı **CG** 'ye eklenen proje adıdır. Kapsayıcı örneğinin **çalışır** durumda olduğunu görürsünüz.
1. Sol menüden **kapsayıcılar** ' ı seçin. Bir kapsayıcı örneği görürsünüz.  Kapsayıcı örneği adı **kapsayıcının** eklendiği proje adıdır.

    ![Dağıtım betiği bağlama kapsayıcı örneği](./media/deployment-script-template-configure-dev/deployment-script-container-instance-connect.png)

1. **Bağlan**' ı seçin ve ardından **Bağlan**' ı seçin. Kapsayıcı örneğine bağlanamıyorsanız, kapsayıcı grubunu yeniden başlatın ve yeniden deneyin.
1. Konsol bölmesinde aşağıdaki komutları çalıştırın:

    ```
    cd deploymentScript
    ls
    pwsh ./hello.ps1 "John Dole"
    ```

    Çıkış **Merhaba John Dole**.

    ![Dağıtım betiği kapsayıcısı örnek testi](./media/deployment-script-template-configure-dev/deployment-script-container-instance-test.png)

## <a name="use-docker"></a>Docker kullanma

Dağıtım komut dosyası geliştirme ortamınız olarak önceden yapılandırılmış bir Docker kapsayıcı görüntüsü kullanabilirsiniz. Docker 'ı yüklemek için bkz. [Docker 'ı edinme](https://docs.docker.com/get-docker/).
Ayrıca, Dağıtım betiklerini Docker kapsayıcısına içeren dizini bağlamak için dosya paylaşımı ' nı yapılandırmanız gerekir.

1. Dağıtım betiği kapsayıcı görüntüsünü yerel bilgisayara çekin:

    ```command
    docker pull mcr.microsoft.com/azuredeploymentscripts-powershell:az4.3
    ```

    Örnek PowerShell 4.3.0 sürümünü kullanır.

    Bir Microsoft Container Registry (MCR) CLı görüntüsünü çekmek için:

    ```command
    docker pull mcr.microsoft.com/azure-cli:2.0.80
    ```

    Bu örnekte, CLı 2.0.80 sürümü kullanılmıştır. Dağıtım betiği [burada](https://hub.docker.com/_/microsoft-azure-cli)bulunan varsayılan CLI kapsayıcıları görüntülerini kullanır.

1. Docker görüntüsünü yerel olarak çalıştırın.

    ```command
    docker run -v <host drive letter>:/<host directory name>:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az4.3
    ```

    ** &lt; Konak sürücü harfi>** ve ** &lt; konak dizin adı>** paylaşılan sürücüdeki mevcut bir klasörle değiştirin.  Klasörü, kapsayıcıdaki **/Data** klasörüne eşler. Örneğin, D:\docker 'ı eşlemek için:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az4.3
    ```

    **-** kapsayıcı görüntüsünün canlı tutulması anlamına gelir.

    CLı örneği:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azure-cli:2.0.80
    ```

1. Aşağıdaki ekran görüntüsünde, paylaşılan sürücüde bir helloworld.ps1 dosyanız olduğunda bir PowerShell betiğinin nasıl çalıştırılacağı gösterilmektedir.

    ![Kaynak Yöneticisi şablonu dağıtım betiği Docker cmd](./media/deployment-script-template/resource-manager-deployment-script-docker-cmd.png)

Betiği başarıyla test edildikten sonra, şablonlarınızı şablonlarda bir dağıtım betiği olarak kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede dağıtım betiklerini nasıl kullanacağınızı öğrendiniz. Bir dağıtım betiği öğreticisini gözden aktarmak için:

> [!div class="nextstepaction"]
> [Öğretici: Azure Resource Manager şablonlarda dağıtım betikleri kullanma](./template-tutorial-deployment-script.md)
