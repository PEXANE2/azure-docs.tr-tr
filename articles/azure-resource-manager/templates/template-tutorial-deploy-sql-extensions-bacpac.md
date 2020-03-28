---
title: Şablonlu SQL BACPAC dosyalarını alma
description: Azure Kaynak Yöneticisi şablonlarıyla SQL BACPAC dosyalarını almak için Azure SQL Veritabanı uzantılarını nasıl kullanacağınızı öğrenin.
author: mumian
ms.date: 12/09/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 8e65ebbfa0971bf2156165b55ca18eee3cc74bc9
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80239271"
---
# <a name="tutorial-import-sql-bacpac-files-with-arm-templates"></a>Öğretici: ARM şablonları ile SQL BACPAC dosyalarını alma

Azure Kaynak Yöneticisi (ARM) şablonlarıyla bir BACPAC dosyasını almak için Azure SQL Veritabanı uzantılarını nasıl kullanacağınızı öğrenin. Dağıtım yapıları, dağıtımtamamlamak için gereken ana şablon dosyalarına ek olarak tüm dosyalardır. BACPAC dosyası bir artefakttır. 

Bu öğreticide, bir Azure SQL sunucusu ve SQL veritabanı dağıtmak ve bir BACPAC dosyası almak için bir şablon oluşturursunuz. ARM şablonlarını kullanarak Azure sanal makine uzantılarının nasıl dağıtılanacınız hakkında bilgi için [Bkz.](./template-tutorial-deploy-vm-extensions.md)

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Bir BACPAC dosyası hazırlayın.
> * Hızlı başlangıç şablonu açın.
> * Şablonu edin.
> * Şablonu dağıtın.
> * Dağıtımı doğrulayın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Visual Studio Code ve Resource Manager Araçları uzantısı. Bkz. [ARM şablonları oluşturmak için Görsel Stüdyo Kodunu Kullan.](./use-vs-code-to-create-template.md)
* Güvenliği artırmak için Azure SQL Server yönetici hesabı için oluşturulan parolayı kullanın. Parola oluşturmak için kullanabileceğiniz bir örnek aşağıda verebilirsiniz:

    ```console
    openssl rand -base64 32
    ```

    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için [Bkz. Öğretici: Azure Anahtar Kasası'nı ARM şablon dağıtımına entegre edin.](./template-tutorial-use-key-vault.md) Ayrıca parolanızı üç ayda bir güncelleştirmenizi öneririz.

## <a name="prepare-a-bacpac-file"></a>BACPAC dosyası hazırlama

Bir BACPAC dosyası [GitHub'da](https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-sql-extension/SQLDatabaseExtension.bacpac)paylaşılır. Kendiniz bir dosya oluşturmak isterseniz bkz. [Azure SQL Veritabanı’nı bir BACPAC dosyasına dışarı aktarma](../../sql-database/sql-database-export.md). Dosyayı kendi belirleyeceğiniz bir konumda yayımlarsanız öğreticinin ilerleyen bölümlerinde şemayı güncelleştirmeniz gerekir.

BACPAC dosyasının ARM şablonu kullanılarak içe aktarılaabilmesi için azure depolama hesabında depolanması gerekir. Aşağıdaki PowerShell komut dosyası BACPAC dosyasını şu adımlarla hazırlar:

* BACPAC dosyasını indirin.
* Bir Azure Depolama hesabı oluşturun.
* Bir depolama hesabı blob kapsayıcı oluşturun.
* BACPAC dosyasını kapsayıcıya yükleyin.
* Depolama hesabı anahtarını ve blob URL'sini görüntüleyin.

1. Bulut kabuğunu açmak için **deneyin'i** seçin. Ardından aşağıdaki PowerShell komut dosyasını kabuk penceresine yapıştırın.

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

1. Depolama hesabı anahtarını ve BACPAC dosya URL'sini yazın. Şablonu dağıtırken bu değerlere ihtiyacınız var.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Bu öğreticide kullanılan şablon [GitHub'da](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json)depolanır.

1. Visual Studio Code'dan **Dosya** > **Aç Dosya'yı**seçin.
1. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json
    ```

1. Dosyayı açmak için **Aç**’ı seçin.

    Şablonda tanımlanan iki kaynak vardır:

   * `Microsoft.Sql/servers`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers).
   * `Microsoft.SQL.servers/databases`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).

        Şablonu özelleştirmeden önce şablonhakkında temel bir anlayış elde etmek yararlı dır.
1. Dosyanın bir kopyasını *azuredeploy.json*adıyla yerel bilgisayarınıza kaydetmek için **Dosya** > **Yı Kaydet'i** seçin.

## <a name="edit-the-template"></a>Şablonu düzenleme

1. Depolama hesabı anahtarını ve BACPAC URL'sini ayarlamak için **parametreler** bölümünün sonuna iki parametre daha ekleyin.

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

    **adminPassword**sonra virgül ekleyin. Visual Studio Code'tan JSON dosyasını biçimlendirmek için Shift+Alt+F'yi seçin.

    Bu iki değeri almak için [bacpac dosyası hazırlama'ya](#prepare-a-bacpac-file)bakın.

1. Şablona iki ek kaynak ekleyin.

    * SQL Veritabanı uzantısının BACPAC dosyalarını içe aktarabilmesi için Azure hizmetlerinden gelen trafiğe izin vermeniz gerekir. SQL sunucu tanımına aşağıdaki güvenlik duvarı kuralı tanımını ekleyin:

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

        Şablon aşağıdaki gibi görünür:

        ![Güvenlik duvarı kuralı tanımına sahip şablon](./media/template-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-firewall.png)

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

        Şablon aşağıdaki gibi görünür:

        ![SQL Veritabanı uzantılı şablon](./media/template-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac.png)

        Kaynak tanımını anlamak için bkz. [SQL Veritabanı uzantısı başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases/extensions). Önemli öğeler şunlardır:

        * **dependsOn**: Uzantı kaynağının SQL veritabanı oluşturulduktan sonra oluşturulması gerekir.
        * **storageKeyType**: Kullanılacak depolama anahtarının türünü belirtin. Değer `StorageAccessKey` veya `SharedAccessKey` olabilir. Bu `StorageAccessKey` öğreticide kullanın.
        * **storageKey**: BACPAC dosyasının depolandığı depolama hesabının anahtarını belirtin. Depolama anahtarı türü `SharedAccessKey`ise, önce "?".
        * **storageUri**: Bir depolama hesabında depolanan BACPAC dosyasının URL'sini belirtin.
        * **administratorLoginPassword**: SQL yönetici parolasıdır. Oluşturulan parolakullanın. [Ön koşullara](#prerequisites) bakın.

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

BACPAC dosyasını hazırlarken kullandığınız proje adını, tüm kaynakların aynı kaynak grubunda depolanabilmesi için kullanmayı düşünün. Bu şekilde, kaynakları temizleme gibi kaynak görevlerini yönetmek daha kolaydır. Aynı proje adını kullanıyorsanız, varolan `New-AzResourceGroup` kaynak grubunu güncelleştirmek isteyip istemediğiniz sorulduğunda komutu komutdosyasından kaldırabilir veya evet (y) veya hayır (n) yanıtını verebilirsiniz.

Oluşturulan parolakullanın. [Ön koşullara](#prerequisites) bakın.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

Sql sunucusuna istemci bilgisayarınızdan erişmek için ek bir güvenlik duvarı kuralı eklemeniz gerekir. Daha fazla bilgi için IP [güvenlik duvarı kuralları oluştur ve yönet'](../../sql-database/sql-database-firewall-configure.md#create-and-manage-ip-firewall-rules)e bakın.

Azure portalında, yeni dağıtılan kaynak grubundan SQL veritabanını seçin. **Sorgu düzenleyicisi (önizleme)** öğesini seçip yönetici kimlik bilgilerini girin. Veritabanına içe aktarılan iki tablo görürsünüz.

![Sorgu düzenleyicisi (önizleme)](./media/template-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-query-editor.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalında, sol menüden **Kaynak grubunu** seçin.
1. **Ada göre filtrele** alanına kaynak grubu adını girin.
1. Kaynak grubu adını seçin. Kaynak grubunda toplam altı kaynak görürsünüz.
1. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir SQL sunucusu ve BIR SQL veritabanı dağıttınız ve bir BACPAC dosyası aldınız. BACPAC dosyası bir Azure Depolama hesabında depolanır. URL'si olan herkes dosyaya erişebilir. BACPAC dosyasını (artefakt) nasıl güvenli hale erdireceklerini öğrenmek için bkz:

> [!div class="nextstepaction"]
> [Yapıları güvenli hale](./template-tutorial-secure-artifacts.md)
