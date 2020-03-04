---
title: SQL BACPAC dosyalarını şablonlarla içeri aktarma
description: Azure Resource Manager şablonlarıyla SQL BACPAC dosyalarını içeri aktarmak için Azure SQL veritabanı uzantıları 'nı nasıl kullanacağınızı öğrenin.
author: mumian
ms.date: 12/09/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 27ac4b67aa19aa59abe80ccf9409acf7b587a22b
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78250106"
---
# <a name="tutorial-import-sql-bacpac-files-with-azure-resource-manager-templates"></a>Öğretici: Azure Resource Manager şablonlarıyla SQL BACPAC dosyalarını içeri aktarma

Azure Resource Manager şablonlarıyla BACPAC dosyasını içeri aktarmak için Azure SQL veritabanı uzantıları 'nı nasıl kullanacağınızı öğrenin. Dağıtım yapıtları, bir dağıtımı tamamlaması gereken ana şablon dosyalarına ek olarak herhangi bir dosya. BACPAC dosyasını bir yapıdır. 

Bu öğreticide, bir Azure SQL Server 'ı ve bir SQL veritabanını dağıtmak ve BACPAC dosyasını içeri aktarmak için bir şablon oluşturacaksınız. Azure Resource Manager şablonları kullanarak Azure sanal makine uzantıları dağıtma hakkında daha fazla bilgi için bkz. [öğretici: Azure Resource Manager şablonlarla sanal makine uzantıları dağıtma](./template-tutorial-deploy-vm-extensions.md).

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * BACPAC dosyası hazırlayın.
> * Hızlı başlangıç şablonu açın.
> * Şablonu düzenleyin.
> * Şablonu dağıtın.
> * Dağıtım doğrulama.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* Kaynak Yöneticisi Araçları uzantısıyla Visual Studio Code. Bkz. [Azure Resource Manager şablonları oluşturmak için Visual Studio Code kullanma](./use-vs-code-to-create-template.md).
* Güvenliği artırmak için, Azure SQL Server yönetici hesabı için oluşturulan bir parola kullanın. Aşağıda, bir parola oluşturmak için kullanabileceğiniz bir örnek verilmiştir:

    ```console
    openssl rand -base64 32
    ```

    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için bkz. [Öğretici: Azure Key Vault'u Resource Manager şablonu dağıtımıyla tümleştirme](./template-tutorial-use-key-vault.md). Ayrıca parolanızı üç ayda bir güncelleştirmenizi öneririz.

## <a name="prepare-a-bacpac-file"></a>BACPAC dosyası hazırlama

Bir BACPAC dosyası [GitHub](https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-sql-extension/SQLDatabaseExtension.bacpac)'da paylaşılır. Kendiniz bir dosya oluşturmak isterseniz bkz. [Azure SQL Veritabanı’nı bir BACPAC dosyasına dışarı aktarma](../../sql-database/sql-database-export.md). Dosyayı kendi belirleyeceğiniz bir konumda yayımlarsanız öğreticinin ilerleyen bölümlerinde şemayı güncelleştirmeniz gerekir.

BACPAC dosyası bir Kaynak Yöneticisi şablonu kullanılarak içeri aktarılabilmesi için önce bir Azure depolama hesabında depolanmalıdır. Aşağıdaki PowerShell betiği BACPAC dosyasını şu adımlarla hazırlar:

* BACPAC dosyasını indirin.
* Bir Azure Depolama hesabı oluşturun.
* Depolama hesabı blobu kapsayıcısı oluşturun.
* BACPAC dosyasını kapsayıcıya yükleyin.
* Depolama hesabı anahtarını ve BLOB URL 'sini görüntüleyin.

1. Cloud Shell 'i açmak için **deneyin** ' i seçin. Ardından, aşağıdaki PowerShell betiğini kabuk penceresine yapıştırın.

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

1. Visual Studio Code’dan **Dosya** > **Dosya Aç**’ı seçin.
1. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json
    ```

1. Dosyayı açmak için **Aç**’ı seçin.

    Şablonda iki kaynak tanımlanmış:

   * `Microsoft.Sql/servers`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers).
   * `Microsoft.SQL.servers/databases`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).

        Özelleştirebilmeniz için önce şablon hakkında bazı temel bilgileri almanız yararlı olur.
1. **Dosya** > **Farklı Kaydet**'i seçerek dosyanın bir kopyasını yerel bilgisayarınıza *azuredeploy.json* adıyla kaydedin.

## <a name="edit-the-template"></a>Şablonu düzenleme

1. Depolama hesabı anahtarını ve BACPAC URL 'sini ayarlamak için **Parametreler** bölümünün sonuna iki parametre ekleyin.

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

    **AdminPassword**'ten sonra virgül ekleyin. JSON dosyasını Visual Studio Code biçimlendirmek için SHIFT + alt + F ' i seçin.

    Bu iki değeri almak için bkz. [BACPAC dosyası hazırlama](#prepare-a-bacpac-file).

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

        Şablon şöyle görünür:

        ![Güvenlik duvarı kuralı tanımıyla şablon](./media/template-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-firewall.png)

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

        Şablon şöyle görünür:

        ![SQL veritabanı uzantısı olan şablon](./media/template-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac.png)

        Kaynak tanımını anlamak için bkz. [SQL Veritabanı uzantısı başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases/extensions). Önemli öğeler şunlardır:

        * **dependsOn**: Uzantı kaynağının SQL veritabanı oluşturulduktan sonra oluşturulması gerekir.
        * **storageKeyType**: kullanılacak depolama anahtarının türünü belirtin. Değer `StorageAccessKey` veya `SharedAccessKey` olabilir. Bu öğreticide `StorageAccessKey` kullanın.
        * **StorageKey**: bacpac dosyasının depolandığı depolama hesabı için anahtarı belirtin. Depolama anahtarı türü `SharedAccessKey`ise, önünde bir "?" olması gerekir.
        * **Storageuri**: bir depolama hesabında depolanan bacpac dosyasının URL 'sini belirtin.
        * **administratorLoginPassword**: SQL yönetici parolasıdır. Oluşturulan bir parola kullanın. [Ön koşullara](#prerequisites) bakın.

Tamamlanan şablon aşağıdaki gibi görünür:

[!code-json[](~/resourcemanager-templates/tutorial-sql-extension/azuredeploy2.json?range=1-106&highlight=38-49,62-76,86-103)]

## <a name="deploy-the-template"></a>Şablonu dağıtma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Dağıtım yordamı için [Şablonu dağıtma](./template-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) bölümüne bakın. Bunun yerine aşağıdaki PowerShell dağıtım betiğini kullanın:

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

Tüm kaynakların aynı kaynak grubunda depolanması için BACPAC dosyasını hazırladığınız sırada kullandığınız proje adını kullanmayı düşünün. Bu şekilde, kaynakları temizleme gibi kaynak görevlerinin yönetilmesi daha kolay olur. Aynı proje adını kullanırsanız, komut dosyasından `New-AzResourceGroup` komutunu kaldırabilir veya mevcut kaynak grubunu güncelleştirmek isteyip istemediğiniz sorulduğunda Evet (y) veya Hayır (n) yanıtını yazabilirsiniz.

Oluşturulan bir parola kullanın. [Ön koşullara](#prerequisites) bakın.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

İstemci bilgisayarınızdan SQL Server 'a erişmek için, ek bir güvenlik duvarı kuralı eklemeniz gerekir. Daha fazla bilgi için bkz. [IP güvenlik duvarı kuralları oluşturma ve yönetme](../../sql-database/sql-database-firewall-configure.md#create-and-manage-ip-firewall-rules).

Azure portal, yeni dağıtılan kaynak grubundan SQL veritabanı ' nı seçin. **Sorgu düzenleyicisi (önizleme)** öğesini seçip yönetici kimlik bilgilerini girin. Veritabanına aktarılmış iki tablo görürsünüz.

![Sorgu Düzenleyicisi (Önizleme)](./media/template-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-query-editor.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portal, sol menüden **kaynak grubu** ' nu seçin.
1. **Ada göre filtrele** alanına kaynak grubu adını girin.
1. Kaynak grubu adını seçin. Kaynak grubunda toplam altı kaynak görürsünüz.
1. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir SQL Server ve SQL veritabanı dağıttı ve BACPAC dosyasını içeri aktardınız. BACPAC dosyası bir Azure depolama hesabında depolanır. URL ile anybody dosyaya erişebilir. BACPAC dosyasını (yapıt) güvenli hale getirme hakkında bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Yapıtları güvenli hale getirme](./template-tutorial-secure-artifacts.md)
