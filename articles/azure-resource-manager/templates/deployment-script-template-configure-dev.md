---
title: Şablonlarda dağıtım betikleri için geliştirme ortamını yapılandırma | Microsoft Docs
description: Azure Resource Manager şablonlarındaki (ARM şablonları) dağıtım betikleri için geliştirme ortamını yapılandırın.
services: azure-resource-manager
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 12/14/2020
ms.author: jgao
ms.openlocfilehash: b2e1ffb3cbd513766945864e33589c46284bf942
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102200946"
---
# <a name="configure-development-environment-for-deployment-scripts-in-arm-templates"></a>ARM şablonlarındaki dağıtım betikleri için geliştirme ortamını yapılandırma

Bir dağıtım betiği görüntüsüyle ARM şablonu dağıtım betikleri geliştirmek ve test etmek için bir geliştirme ortamı oluşturmayı öğrenin. Bir [Azure Container Instance](../../container-instances/container-instances-overview.md) oluşturabilir ya da [Docker](https://docs.docker.com/get-docker/)kullanabilirsiniz. Her iki seçenek de bu makalede ele alınmıştır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="azure-powershell-container"></a>Azure PowerShell kapsayıcı

Azure PowerShell dağıtım betiğinizin yoksa, aşağıdaki içeriği kullanarak bir *hello.ps1* dosyası oluşturabilirsiniz:

```powershell
param([string] $name)
$output = 'Hello {0}' -f $name
Write-Output $output
$DeploymentScriptOutputs = @{}
$DeploymentScriptOutputs['text'] = $output
```

### <a name="azure-cli-container"></a>Azure CLı kapsayıcısı

Bir Azure CLı kapsayıcı görüntüsü için aşağıdaki içeriği kullanarak bir *Hello.sh* dosyası oluşturabilirsiniz:

```bash
firstname=$1
lastname=$2
output="{\"name\":{\"displayName\":\"$firstname $lastname\",\"firstName\":\"$firstname\",\"lastName\":\"$lastname\"}}"
echo -n "Hello "
echo $output | jq -r '.name.displayName'
```

> [!NOTE]
> Bir Azure CLı dağıtım betiği çalıştırdığınızda adlı bir ortam değişkeni, `AZ_SCRIPTS_OUTPUT_PATH` komut dosyası çıkış dosyasının konumunu depolar. Ortam değişkeni, geliştirme ortamı kapsayıcısında kullanılamıyor. Azure CLı çıkışları ile çalışma hakkında daha fazla bilgi için bkz. [CLI betiğinin çıktılarla çalışma](deployment-script-template.md#work-with-outputs-from-cli-script).

## <a name="use-azure-powershell-container-instance"></a>Azure PowerShell Container Instance kullanın

Betiklerinizi bilgisayarınızda yazmak için bir depolama hesabı oluşturmanız ve depolama hesabını kapsayıcı örneğine bağlamanız gerekir. Böylece komut dosyanızı depolama hesabına yükleyebilir ve betiği kapsayıcı örneğinde çalıştırabilirsiniz.

> [!NOTE]
> Komut dosyanızı test etmek için oluşturduğunuz depolama hesabı, dağıtım betiği hizmetinin betiği yürütmek için kullandığı depolama hesabı değildir. Dağıtım betiği hizmeti, her yürütmede dosya paylaşma olarak benzersiz bir ad oluşturur.

### <a name="create-an-azure-powershell-container-instance"></a>Azure PowerShell kapsayıcı örneği oluşturma

Aşağıdaki Azure Resource Manager şablonu (ARM şablonu) bir kapsayıcı örneği ve bir dosya paylaşma oluşturup dosya paylaşımının kapsayıcı görüntüsüne takar.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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
      "defaultValue": "mcr.microsoft.com/azuredeploymentscripts-powershell:az5.2",
      "metadata": {
        "description": "Specify the container image."
      }
    },
    "mountPath": {
      "type": "string",
      "defaultValue": "/mnt/azscripts/azscriptinput",
      "metadata": {
        "description": "Specify the mount path."
      }
    }
  },
  "variables": {
    "storageAccountName": "[tolower(concat(parameters('projectName'), 'store'))]",
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

Bağlama yolu için varsayılan değer `/mnt/azscripts/azscriptinput` . Bu, dosya paylaşımının bağlı olduğu kapsayıcı örneğindeki yoldur.

Şablonda belirtilen varsayılan kapsayıcı görüntüsü **MCR.Microsoft.com/azuredeploymentscripts-PowerShell:az5.2**. [Desteklenen tüm Azure PowerShell sürümlerinin](https://mcr.microsoft.com/v2/azuredeploymentscripts-powershell/tags/list)listesini görüntüleyin.

Şablon, 1.800 saniye sonra kapsayıcı örneğini askıya alır. Kapsayıcı örneği sonlandırılmış duruma geçmeden önce 30 dakika, oturum sona erer.

Şablonu dağıtmak için:

```azurepowershell
$projectName = Read-Host -Prompt "Enter a project name that is used to generate resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateFile = Read-Host -Prompt "Enter the template file path and file name"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Location $location -name $resourceGroupName
New-AzResourceGroupDeployment -resourceGroupName $resourceGroupName -TemplateFile $templatefile -projectName $projectName
```

### <a name="upload-the-deployment-script"></a>Dağıtım betiğini karşıya yükle

Dağıtım betiğinizi depolama hesabına yükleyin. PowerShell betiğine bir örnek aşağıda verilmiştir:

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name that you used earlier"
$fileName = Read-Host -Prompt "Enter the deployment script file name with the path"

$resourceGroupName = "${projectName}rg"
$storageAccountName = "${projectName}store"
$fileShareName = "${projectName}share"

$context = (Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName).Context
Set-AzStorageFileContent -Context $context -ShareName $fileShareName -Source $fileName -Force
```

Ayrıca, Azure portal veya Azure CLı kullanarak dosyayı karşıya yükleyebilirsiniz.

### <a name="test-the-deployment-script"></a>Dağıtım betiğini test etme

1. Azure portal, kapsayıcı örneğini ve depolama hesabını dağıttığınız kaynak grubunu açın.
2. Kapsayıcı grubunu açın. Varsayılan kapsayıcı grubu adı, *CG* ile eklenen proje adıdır. Kapsayıcı örneği **çalışır** durumda.
3. Kaynak menüsünde **kapsayıcılar**' ı seçin. Kapsayıcı örneği adı, *kapsayıcının* eklendiği proje adıdır.

    ![Azure portal dağıtım betiği bağlama kapsayıcısı örneğinin ekran görüntüsü.](./media/deployment-script-template-configure-dev/deployment-script-container-instance-connect.png)

4. **Bağlan**' ı seçin ve ardından **Bağlan**' ı seçin. Kapsayıcı örneğine bağlanamıyorsanız, kapsayıcı grubunu yeniden başlatın ve yeniden deneyin.
5. Konsol bölmesinde aşağıdaki komutları çalıştırın:

    ```console
    cd /mnt/azscripts/azscriptinput
    ls
    pwsh ./hello.ps1 "John Dole"
    ```

    Çıkış **Merhaba John Dole**.

    ![Konsolundaki dağıtım betiği bağlama kapsayıcı örneği test çıktısının ekran görüntüsü.](./media/deployment-script-template-configure-dev/deployment-script-container-instance-test.png)

## <a name="use-an-azure-cli-container-instance"></a>Azure CLı kapsayıcı örneği kullanma

Betiklerinizi bilgisayarınızda yazmak için bir depolama hesabı oluşturun ve depolama hesabını kapsayıcı örneğine bağlayın. Ardından, komut dosyanızı depolama hesabına yükleyebilir ve betiği kapsayıcı örneğinde çalıştırabilirsiniz.

> [!NOTE]
> Komut dosyanızı test etmek için oluşturduğunuz depolama hesabı, dağıtım betiği hizmetinin betiği yürütmek için kullandığı depolama hesabı değildir. Dağıtım betiği hizmeti, her yürütmede dosya paylaşma olarak benzersiz bir ad oluşturur.

### <a name="create-an-azure-cli-container-instance"></a>Azure CLı kapsayıcı örneği oluşturma

Aşağıdaki ARM şablonu bir kapsayıcı örneği ve bir dosya paylaşma oluşturup dosya paylaşımından kapsayıcı görüntüsüne bağlar:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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
      "defaultValue": "mcr.microsoft.com/azure-cli:2.9.1",
      "metadata": {
        "description": "Specify the container image."
      }
    },
    "mountPath": {
      "type": "string",
      "defaultValue": "/mnt/azscripts/azscriptinput",
      "metadata": {
        "description": "Specify the mount path."
      }
    }
  },
  "variables": {
    "storageAccountName": "[tolower(concat(parameters('projectName'), 'store'))]",
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
                "/bin/bash",
                "-c",
                "echo hello; sleep 1800"
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

Bağlama yolu için varsayılan değer `/mnt/azscripts/azscriptinput` . Bu, dosya paylaşımının bağlı olduğu kapsayıcı örneğindeki yoldur.

Şablonda belirtilen varsayılan kapsayıcı görüntüsü **MCR.Microsoft.com/Azure-cli:2.9.1**. [Desteklenen Azure CLI sürümlerinin](https://mcr.microsoft.com/v2/azure-cli/tags/list)listesini görüntüleyin.

> [!IMPORTANT]
> Dağıtım betiği, Microsoft Container Registry (MCR) ' deki kullanılabilir CLı görüntülerini kullanır. Bir bir ayda, bir dağıtım betiği için CLı görüntüsünü onaylamak için bir ay sürer. 30 gün içinde Yayınlanan CLı sürümlerini kullanmayın. Görüntülerin yayın tarihlerini bulmak için bkz. [Azure CLI sürüm notları](/cli/azure/release-notes-azure-cli). Desteklenmeyen bir sürüm kullanıyorsanız, hata iletisinde desteklenen sürümler listelenir.

Şablon, 1.800 saniye sonra kapsayıcı örneğini askıya alır. Kapsayıcı örneği bir Terminal durumuna geçmeden önce 30 dakika, oturum sona erer.

Şablonu dağıtmak için:

```azurepowershell
$projectName = Read-Host -Prompt "Enter a project name that is used to generate resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateFile = Read-Host -Prompt "Enter the template file path and file name"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Location $location -name $resourceGroupName
New-AzResourceGroupDeployment -resourceGroupName $resourceGroupName -TemplateFile $templatefile -projectName $projectName
```

### <a name="upload-the-deployment-script"></a>Dağıtım betiğini karşıya yükle

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

Ayrıca, Azure portal veya Azure CLı kullanarak dosyayı karşıya yükleyebilirsiniz.

### <a name="test-the-deployment-script"></a>Dağıtım betiğini test etme

1. Azure portal, kapsayıcı örneğini ve depolama hesabını dağıttığınız kaynak grubunu açın.
1. Kapsayıcı grubunu açın. Varsayılan kapsayıcı grubu adı, *CG* ile eklenen proje adıdır. Kapsayıcı örneği **çalışır** durumda gösterilir.
1. Kaynak menüsünde **kapsayıcılar**' ı seçin. Kapsayıcı örneği adı, *kapsayıcının* eklendiği proje adıdır.

    ![Dağıtım betiği bağlama kapsayıcı örneği](./media/deployment-script-template-configure-dev/deployment-script-container-instance-connect.png)

1. **Bağlan**' ı seçin ve ardından **Bağlan**' ı seçin. Kapsayıcı örneğine bağlanamıyorsanız, kapsayıcı grubunu yeniden başlatın ve yeniden deneyin.
1. Konsol bölmesinde aşağıdaki komutları çalıştırın:

    ```console
    cd /mnt/azscripts/azscriptinput
    ls
    ./hello.sh John Dole
    ```

    Çıkış **Merhaba John Dole**.

    ![Dağıtım betiği kapsayıcısı örnek testi](./media/deployment-script-template-configure-dev/deployment-script-container-instance-test-cli.png)

## <a name="use-docker"></a>Docker kullanma

Dağıtım komut dosyası geliştirme ortamınız olarak önceden yapılandırılmış bir Docker kapsayıcı görüntüsü kullanabilirsiniz. Docker 'ı yüklemek için bkz. [Docker 'ı edinme](https://docs.docker.com/get-docker/).
Ayrıca, Dağıtım betiklerini Docker kapsayıcısına içeren dizini bağlamak için dosya paylaşımı ' nı yapılandırmanız gerekir.

1. Dağıtım betiği kapsayıcı görüntüsünü yerel bilgisayara çekin:

    ```command
    docker pull mcr.microsoft.com/azuredeploymentscripts-powershell:az4.3
    ```

    Örnek PowerShell 4.3.0 sürümünü kullanır.

    MCR 'den CLı görüntüsü çekmek için:

    ```command
    docker pull mcr.microsoft.com/azure-cli:2.0.80
    ```

    Bu örnekte, CLı 2.0.80 sürümü kullanılmıştır. Dağıtım betiği [burada](https://hub.docker.com/_/microsoft-azure-cli)bulunan varsayılan CLI kapsayıcıları görüntülerini kullanır.

1. Docker görüntüsünü yerel olarak çalıştırın.

    ```command
    docker run -v <host drive letter>:/<host directory name>:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az4.3
    ```

    **&lt; Konak sürücü harfi>** ve **&lt; konak dizin adı>** paylaşılan sürücüdeki mevcut bir klasörle değiştirin. Klasörü, kapsayıcıdaki _/Data_ klasörüne eşler. Örneğin, _D:\docker_'ı eşlemek için:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az4.3
    ```

    **-** kapsayıcı görüntüsünün canlı tutulması anlamına gelir.

    CLı örneği:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azure-cli:2.0.80
    ```

1. Aşağıdaki ekran görüntüsünde, paylaşılan sürücüde bir *helloworld.ps1* dosyanız olduğunda bir PowerShell betiğinin nasıl çalıştırılacağı gösterilmektedir.

    ![Kaynak Yöneticisi şablonu dağıtım betiği Docker cmd](./media/deployment-script-template/resource-manager-deployment-script-docker-cmd.png)

Betiği başarıyla test edildikten sonra, şablonlarınızı şablonlarda bir dağıtım betiği olarak kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede dağıtım betiklerini nasıl kullanacağınızı öğrendiniz. Bir dağıtım betiği öğreticisini gözden aktarmak için:

> [!div class="nextstepaction"]
> [Öğretici: ARM şablonlarında dağıtım betikleri kullanma](./template-tutorial-deployment-script.md)
