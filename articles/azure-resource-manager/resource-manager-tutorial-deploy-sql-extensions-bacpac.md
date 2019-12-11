---
title: SQL BACPAC dosyalarını şablonlarla içeri aktarma
description: SQL veritabanı uzantısı 'nı kullanarak SQL BACPAC dosyalarını Azure Resource Manager şablonlarıyla içeri aktarma hakkında bilgi edinin.
author: mumian
ms.date: 12/09/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 9f5e2e402e13076dc538a9c9d55e5b67e0d86d4f
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74978926"
---
# <a name="tutorial-import-sql-bacpac-files-with-azure-resource-manager-templates"></a>Öğretici: Azure Resource Manager şablonlarıyla SQL BACPAC dosyalarını içeri aktarma

Azure Resource Manager şablonlarıyla BACPAC dosyasını içeri aktarmak için Azure SQL veritabanı uzantıları 'nı nasıl kullanacağınızı öğrenin. Dağıtım yapıtları, bir dağıtımı tamamlaması gereken ana şablon dosyalarına ek olarak herhangi bir dosya. BACPAC dosyasını bir yapıdır. Bu öğreticide, bir Azure SQL Server, bir SQL veritabanını dağıtmak ve BACPAC dosyasını içeri aktarmak için bir şablon oluşturacaksınız. Azure Resource Manager şablonlarını kullanarak Azure sanal makine uzantılarını dağıtma hakkında daha fazla bilgi için bkz. [# Öğretici: Azure Resource Manager şablonlarıyla sanal makine uzantılarını dağıtma](./resource-manager-tutorial-deploy-vm-extensions.md).

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

* Kaynak Yöneticisi Araçları uzantısı ile Visual Studio Code. Bkz. [Azure Resource Manager şablonları oluşturmak için Visual Studio Code kullanma](./resource-manager-tools-vs-code.md).
* Güvenliği artırmak istiyorsanız SQL Server yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```azurecli-interactive
    openssl rand -base64 32
    ```

    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için bkz. [Öğretici: Azure Key Vault'u Resource Manager şablonu dağıtımıyla tümleştirme](./resource-manager-tutorial-use-key-vault.md). Ayrıca parolanızı üç ayda bir güncelleştirmenizi öneririz.

## <a name="prepare-a-bacpac-file"></a>BACPAC dosyası hazırlama

Bir BACPAC dosyası [GitHub](https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-sql-extension/SQLDatabaseExtension.bacpac)'da paylaşılır. Kendiniz bir dosya oluşturmak isterseniz bkz. [Azure SQL Veritabanı’nı bir BACPAC dosyasına dışarı aktarma](../sql-database/sql-database-export.md). Dosyayı kendi belirleyeceğiniz bir konumda yayımlarsanız öğreticinin ilerleyen bölümlerinde şemayı güncelleştirmeniz gerekir.

BACPAC dosyası, Kaynak Yöneticisi şablonu kullanılarak içeri aktarılmadan önce bir Azure depolama hesabında depolanmalıdır. Aşağıdaki PowerShell betiği BACPAC dosyasını şu adımlarla hazırlar:

* BACPAC dosyasını indirin.
* Bir Azure Depolama hesabı oluşturun.
* Bir depolama hesabı Blob kapsayıcı oluşturun.
* BACPAC dosyasını kapsayıcıya yükleyin.
* Depolama hesabı anahtarını ve BLOB URL 'sini görüntüleyin.

1. Cloud Shell 'i açmak için **deneyin** ' i seçin ve ardından aşağıdaki PowerShell betiğini kabuk penceresine yapıştırın.

    ```azurepowershell-interactive
    $projectName = Read-Host -Prompt "Enter a project name that is used to generate Azure resource names"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"

    $resourceGroupName = "${projectName}rg"
    $storageAccountName = "${projectName}store"
    $containerName = "bacpacfiles"
    $bacpacFileName = "SQLDatabaseExtension.bacpac"
    $bacpacUrl = "https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-sql-extension/SQLDatabaseExtension.bacpac"

    # Download the bacpac file
    Invoke-WebRequest -Uri $bacpacUrl -OutFile "$HOME/$bacpacFileName"

    # Create a resource group
    New-AzResourceGroup -Name $resourceGroupName -Location $location

    # Create a storage account
    $storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroupName `
                                           -Name $storageAccountName `
                                           -SkuName Standard_LRS `
                                           -Location $location
    $storageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName `
                                                  -Name $storageAccountName).Value[0]

    # Create a container
    New-AzStorageContainer -Name $containerName -Context $storageAccount.Context

    # Upload the BACPAC file to the container
    Set-AzStorageBlobContent -File $HOME/$bacpacFileName `
                             -Container $containerName `
                             -Blob $bacpacFileName `
                             -Context $storageAccount.Context

    Write-Host "The storage account key is $storageAccountKey"
    Write-Host "The BACPAC file URL is https://$storageAccountName.blob.core.windows.net/$containerName/$bacpacFileName"
    Write-Host "Press [ENTER] to continue ..."
    ```

1. Depolama hesabı anahtarını ve BACPAC dosya URL 'sini yazın. Şablonu dağıtırken bu değerlere ihtiyacınız vardır.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Bu öğreticide kullanılan şablon [GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json)' da depolanır.

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json
    ```

3. Dosyayı açmak için **Aç**’ı seçin.

    Şablonda iki kaynak tanımlanmış:

   * `Microsoft.Sql/servers`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers).
   * `Microsoft.SQL.servers/databases`.  Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).

     Şablonu özelleştirmeden önce temel noktaları kavramak faydalı olacaktır.
4. **Dosya**>**Farklı Kaydet**'i seçerek dosyanın bir kopyasını yerel bilgisayarınıza **azuredeploy.json** adıyla kaydedin.

## <a name="edit-the-template"></a>Şablonu düzenleme

1. Depolama hesabı anahtarını ve BACPAC URL 'sini ayarlamak için **Parametreler** bölümünün sonuna iki parametre ekleyin:

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

    **AdminPassword**'ten sonra virgül ekleyin. JSON dosyasını VS Code biçimlendirmek için **[SHIFT] + [alt] + F**tuşlarına basın.

    Bkz. bu iki değeri alma hakkında [BACPAC dosyası hazırlama](#prepare-a-bacpac-file) .

1. Şablona iki ek kaynak ekleyin.

    * SQL veritabanı uzantısının BACPAC dosyalarını içeri aktarmasını sağlamak için Azure hizmetlerinden gelen trafiğe izin vermeniz gerekir. Aşağıdaki güvenlik duvarı kuralı tanımını SQL Server tanımına ekleyin:

        ```json
        "resources": [
          {
            "type": "firewallrules",
            "apiVersion": "2015-05-01-preview",
            "name": "AllowAllAzureIps",
            "location": "[parameters('location')]",
            "dependsOn": [
              "[parameters('databaseServerName')]"
            ],
            "properties": {
              "startIpAddress": "0.0.0.0",
              "endIpAddress": "0.0.0.0"
            }
          }
        ]
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
              "[resourceId('Microsoft.Sql/servers/databases', parameters('databaseServerName'), parameters('databaseName'))]"
            ],
            "properties": {
              "storageKeyType": "StorageAccessKey",
              "storageKey": "[parameters('storageAccountKey')]",
              "storageUri": "[parameters('bacpacUrl')]",
              "administratorLogin": "[parameters('adminUser')]",
              "administratorLoginPassword": "[parameters('adminPassword')]",
              "operationMode": "Import"
            }
          }
        ]
        ```

        Şablonun şu şekilde olması gerekir:

        ![Azure Resource Manager SQL uzantılarını dağıtma BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac.png)

        Kaynak tanımını anlamak için bkz. [SQL Veritabanı uzantısı başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases/extensions). Önemli öğeler şunlardır:

        * **dependsOn**: Uzantı kaynağının SQL veritabanı oluşturulduktan sonra oluşturulması gerekir.
        * **storageKeyType**: kullanılacak depolama anahtarının türünü belirtin. Değer `StorageAccessKey` veya `SharedAccessKey` olabilir. Bu öğreticide `StorageAccessKey` kullanın.
        * **StorageKey**: bacpac dosyasının depolandığı depolama hesabı için anahtarı belirtin. Depolama anahtarı türü SharedAccessKey ise, önünde bir "?" olmalıdır
        * **Storageuri**: bir depolama hesabında depolanan bacpac dosyasının URL 'sini belirtin.
        * **administratorLoginPassword**: SQL yönetici parolasıdır. Oluşturulan bir parola kullanın. [Ön koşullara](#prerequisites) bakın.

Tamamlanan şablon aşağıdaki gibi görünür:

[!code-json[](~/resourcemanager-templates/tutorial-sql-extension/azuredeploy2.json?range=1-106&highlight=38-49,62-76,86-103)]

## <a name="deploy-the-template"></a>Şablonu dağıtma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Dağıtım yordamı için [Şablonu dağıtma](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) bölümüne bakın. Bunun yerine aşağıdaki PowerShell dağıtım betiğini kullanın:

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that is used earlier"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the SQL admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
$storageAccountKey = Read-Host -Prompt "Enter the storage account key"
$bacpacUrl = Read-Host -Prompt "Enter the URL of the BACPAC file"
$resourceGroupName = "${projectName}rg"

#New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -TemplateFile "$HOME/azuredeploy.json" `
    -storageAccountKey $storageAccountKey `
    -bacpacUrl $bacpacUrl

Write-Host "Press [ENTER] to continue ..."
```

Tüm kaynakların aynı kaynak grubunda saklanması için bacpac dosyasını hazırladığınız sırada kullandığınız proje adını kullanmayı düşünün.  Kaynakları Temizleme gibi kaynakların yönetiminde daha kolay hale gelir. Aynı proje adını kullanırsanız, **New-AzResourceGroup** komutunu betikten kaldırabilir ya da mevcut kaynak grubunu güncelleştirmek isteyip istemediğiniz sorulduğunda y veya n yanıtını verebilirsiniz.

Oluşturulan bir parola kullanın. [Ön koşullara](#prerequisites) bakın.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

İstemci bilgisayarınızdan SQL Server 'a erişmek için, ek bir güvenlik duvarı kuralı eklemeniz gerekir. Daha fazla bilgi için bkz. [IP güvenlik duvarı kuralları oluşturma ve yönetme](../sql-database/sql-database-firewall-configure.md#create-and-manage-ip-firewall-rules).

Portalda yeni dağıtılan kaynak grubundaki SQL veritabanını seçin. **Sorgu düzenleyicisi (önizleme)** öğesini seçip yönetici kimlik bilgilerini girin. İki tablonun veritabanına aktarılmış olması gerekir:

![Azure Resource Manager SQL uzantılarını dağıtma BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-query-editor.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir SQL Server ve bir SQL Veritabanı oluşturup bir BACPAC dosyasını içeri aktardınız. BACPAC dosyası bir Azure depolama hesabında depolanır. URL ile anybody dosyaya erişebilir. BACPAC dosyasını (yapıt) güvenli hale getirme hakkında bilgi edinmek için bkz.

> [!div class="nextstepaction"]
> [Yapıtları güvenli hale getirme](./resource-manager-tutorial-secure-artifacts.md)
