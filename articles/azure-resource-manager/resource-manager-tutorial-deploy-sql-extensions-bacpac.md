---
title: Import SQL BACPAC files with templates
description: Learn how to use SQL Database extension to import SQL BACPAC files with Azure Resource Manager templates.
author: mumian
ms.date: 11/21/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 741521551335712400e5f61822d7dda31199d3df
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74422189"
---
# <a name="tutorial-import-sql-bacpac-files-with-azure-resource-manager-templates"></a>Öğretici: Azure Resource Manager şablonlarıyla SQL BACPAC dosyalarını içeri aktarma

Learn how to use Azure SQL Database extensions to import a BACPAC file with Azure Resource Manager templates. Deployment artifacts are any files, in addition to the main template files that are needed to complete a deployment. The BACPAC file is an artifact. In this tutorial, you create a template to deploy an Azure SQL Server, a SQL Database, and import a BACPAC file. Azure Resource Manager şablonlarını kullanarak Azure sanal makine uzantılarını dağıtma hakkında daha fazla bilgi için bkz. [# Öğretici: Azure Resource Manager şablonlarıyla sanal makine uzantılarını dağıtma](./resource-manager-tutorial-deploy-vm-extensions.md).

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * BACPAC dosyası hazırlama
> * Hızlı başlangıç şablonunu açma
> * Şablonu düzenleme
> * Şablonu dağıtma
> * Dağıtımı doğrulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* Visual Studio Code with Resource Manager Tools extension. See [Use Visual Studio Code to create Azure Resource Manager templates](./resource-manager-tools-vs-code.md).
* Güvenliği artırmak istiyorsanız SQL Server yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```azurecli-interactive
    openssl rand -base64 32
    ```

    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için bkz. [Öğretici: Azure Key Vault'u Resource Manager şablonu dağıtımıyla tümleştirme](./resource-manager-tutorial-use-key-vault.md). Ayrıca parolanızı üç ayda bir güncelleştirmenizi öneririz.

## <a name="prepare-a-bacpac-file"></a>BACPAC dosyası hazırlama

A BACPAC file is shared in [GitHub](https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-sql-extension/SQLDatabaseExtension.bacpac). Kendiniz bir dosya oluşturmak isterseniz bkz. [Azure SQL Veritabanı’nı bir BACPAC dosyasına dışarı aktarma](../sql-database/sql-database-export.md). Dosyayı kendi belirleyeceğiniz bir konumda yayımlarsanız öğreticinin ilerleyen bölümlerinde şemayı güncelleştirmeniz gerekir.

The BACPAC file must be stored in an Azure Storage account before it can be imported using Resource Manager template.

1. Open the [Cloud shell](https://shell.azure.com).
1. Select **Upload/Download files**, and then select **Upload**.
1. Specify the following URL and then select **Open**.

    ```url
    https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-sql-extension/SQLDatabaseExtension.bacpac
    ```

1. Copy and paste the following PowerShell script into the shell window.

    ```azurepowershell-interactive
    $projectName = Read-Host -Prompt "Enter a project name that is used to generate Azure resource names"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"

    $resourceGroupName = "${projectName}rg"
    $storageAccountName = "${projectName}store"
    $containerName = "bacpacfiles"
    $bacpacFile = "$HOME/SQLDatabaseExtension.bacpac"
    $blobName = "SQLDatabaseExtension.bacpac"

    New-AzResourceGroup -Name $resourceGroupName -Location $location
    $storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroupName `
                                           -Name $storageAccountName `
                                           -SkuName Standard_LRS `
                                           -Location $location
    $storageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName `
                                                  -Name $storageAccountName).Value[0]

    New-AzStorageContainer -Name $containerName -Context $storageAccount.Context

    Set-AzStorageBlobContent -File $bacpacFile `
                             -Container $containerName `
                             -Blob $blobName `
                             -Context $storageAccount.Context

    Write-Host "The storage account key is $storageAccountKey"
    Write-Host "The BACPAC file URL is https://$storageAccountName.blob.core.windows.net/$containerName/$blobName"
    Write-Host "Press [ENTER] to continue ..."
    ```

1. Write down storage account key and the BACPAC file URL. You need these values when you deploy the template.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

The template used in this tutorial is stored in [GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json).

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json
    ```

3. Dosyayı açmak için **Aç**’ı seçin.

    Şablonda tanımlanan üç kaynak vardır:

   * `Microsoft.Sql/servers`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers).
   * `Microsoft.SQL/servers/securityAlertPolicies`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/securityalertpolicies).
   * `Microsoft.SQL.servers/databases`.  Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).

     Şablonu özelleştirmeden önce temel noktaları kavramak faydalı olacaktır.
4. **Dosya**>**Farklı Kaydet**'i seçerek dosyanın bir kopyasını yerel bilgisayarınıza **azuredeploy.json** adıyla kaydedin.

## <a name="edit-the-template"></a>Şablonu düzenleme

1. Add two more parameters at the end of the **parameters** section to set the storage account key and the BACPAC URL:

    ```json
    "storageAccountKey": {
      "type":"string",
      "metadata":{
        "description": "Specifies the key of the storage account where the BACPAC file is stored."
      }
    },
    "bacpacUrl": {
      "type":"string",
      "metadata":{
        "description": "Specifies the URL of the BACPAC file."
      }
    }
    ```

    Add a comma after **adminPassword**. To format the JSON file from VS Code, press **[SHIFT]+[ALT]+F**.

    See [Prepare a BACPAC file](#prepare-a-bacpac-file) about getting these two values.

1. Add two additional resources to the template.

    * To allow the SQL database extension to import BACPAC files, you need to allow traffic from Azure services. Add the following firewall rule definition under the SQL server definition:

        ```json
        {
          "type": "firewallrules",
          "apiVersion": "2015-05-01-preview",
          "name": "AllowAllAzureIps",
          "location": "[parameters('location')]",
          "dependsOn": [
            "[variables('databaseServerName')]"
          ],
          "properties": {
            "startIpAddress": "0.0.0.0",
            "endIpAddress": "0.0.0.0"
          }
        }
        ```

        Şablonun şu şekilde olması gerekir:

        ![Azure Resource Manager SQL uzantılarını dağıtma BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-firewall.png)

    * Aşağıdaki JSON kodunu kullanarak veritabanı tanımına bir SQL Veritabanı uzantısı kaynağı ekleyin:

        ```json
        "resources": [
            {
              "type": "extensions",
              "apiVersion": "2014-04-01",
              "name": "Import",
              "dependsOn": [
                "[resourceId('Microsoft.Sql/servers/databases', variables('databaseServerName'), variables('databaseName'))]"
              ],
              "properties": {
                "storageKeyType": "StorageAccessKey",
                "storageKey": "[parameters('storageAccountKey')]",
                "storageUri": "[parameters('bacpacUrl')]",
                "administratorLogin": "[variables('databaseServerAdminLogin')]",
                "administratorLoginPassword": "[variables('databaseServerAdminLoginPassword')]",
                "operationMode": "Import"
              }
            }
        ]
        ```

        Şablonun şu şekilde olması gerekir:

        ![Azure Resource Manager SQL uzantılarını dağıtma BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac.png)

        Kaynak tanımını anlamak için bkz. [SQL Veritabanı uzantısı başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases/extensions). Önemli öğeler şunlardır:

        * **dependsOn**: Uzantı kaynağının SQL veritabanı oluşturulduktan sonra oluşturulması gerekir.
        * **storageKeyType**: Specify the type of the storage key to use. Değer `StorageAccessKey` veya `SharedAccessKey` olabilir. Use `StorageAccessKey` in this tutorial.
        * **storageKey**: Specify the key for the storage account where the BACPAC file is stored. If storage key type is SharedAccessKey, it must be preceded with a "?"
        * **storageUri**: Specify the URL of the BACPAC file stored in a storage account.
        * **administratorLoginPassword**: SQL yönetici parolasıdır. Use a generated password. [Ön koşullara](#prerequisites) bakın.

## <a name="deploy-the-template"></a>Şablonu dağıtma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Dağıtım yordamı için [Şablonu dağıtma](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) bölümüne bakın. Bunun yerine aşağıdaki PowerShell dağıtım betiğini kullanın:

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used to generate Azure resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the SQL admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
$storageAccountKey = Read-Host -Prompt "Enter the storage account key"
$bacpacUrl = Read-Host -Prompt "Enter the URL of the BACPAC file"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -TemplateFile "$HOME/azuredeploy.json" `
    -storageAccountKey $storageAccountKey `
    -bacpacUrl $bacpacUrl

Write-Host "Press [ENTER] to continue ..."
```

Consider using the same project name as you used when you prepared the bacpac file, so that all the resources are stored within the same resource group.  It is easier for managing resource, such as cleaning up the resources. If you use the same project name, you can either remove the **New-AzResourceGroup** command from the script, or answer y or n when you are asked whether you want to update the existing resource group.

Use a generated password. [Ön koşullara](#prerequisites) bakın.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

To access the SQL server from your client computer, you need to add an additional firewall rule. For more information, see [Create and manage IP firewall rules](../sql-database/sql-database-firewall-configure.md#create-and-manage-ip-firewall-rules).

Portalda yeni dağıtılan kaynak grubundaki SQL veritabanını seçin. **Sorgu düzenleyicisi (önizleme)** öğesini seçip yönetici kimlik bilgilerini girin. İki tablonun veritabanına aktarılmış olması gerekir:

![Azure Resource Manager SQL uzantılarını dağıtma BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-query-editor.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir SQL Server ve bir SQL Veritabanı oluşturup bir BACPAC dosyasını içeri aktardınız. The BACPAC file is stored in an Azure storage account. Anybody with the URL can access the file. To learn how to secure the BACPAC file (artifact), see

> [!div class="nextstepaction"]
> [Secure the artifacts](./resource-manager-tutorial-secure-artifacts.md)
