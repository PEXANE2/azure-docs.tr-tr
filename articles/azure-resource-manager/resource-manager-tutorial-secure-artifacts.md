---
title: Şablonlarda güvenli yapıtlar
description: Yapıtlar, Azure Resource Manager şablonlarında kullanılan güvenli hale getirmeyi öğrenin.
author: mumian
ms.date: 12/09/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 1a9d209e843d8e9a1735a3c6907b00d85be6580b
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74971749"
---
# <a name="tutorial-secure-artifacts-in-azure-resource-manager-template-deployments"></a>Öğretici: Azure Resource Manager şablon dağıtımları yapıları güvenliğini sağlama

Paylaşılan erişim imzaları (SAS) ile Azure depolama hesabı kullanarak, Azure Resource Manager şablonlarında kullanılan yapıtların güvenli hale getirmeyi öğrenin. Dağıtım yapıtları dağıtımı tamamlamak için gereken tüm ana şablon dosyası yanı sıra dosyalarıdır. Örneğin, [öğretici: Azure Resource Manager şablonları ile içeri aktarma SQL BACPAC dosyalarını](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md), Azure SQL veritabanı'ana şablon oluşturur; ayrıca tablo oluşturup veri eklemek için bir BACPAC dosyasını çağırır. BACPAC dosyası bir yapıt Azure depolama hesabında depolanır. Yapıya erişmek için depolama hesabı anahtarı kullanıldı. Bu öğreticide, SAS kendi Azure depolama hesabında BACPAC dosyasına sınırlı erişim vermek için kullanın. SAS hakkında daha fazla bilgi için bkz: [paylaşılan erişim imzaları (SAS) kullanma](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

Bağlı şablon güvenliğini sağlamayı öğrenmek için bkz: [Öğreticisi: bağlı bir Azure Resource Manager şablonları oluşturma](./resource-manager-tutorial-create-linked-templates.md).

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * BACPAC dosyası hazırlama
> * Mevcut bir şablonu açın
> * Şablonu düzenleme
> * Şablonu dağıtma
> * Dağıtımı doğrulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* Kaynak Yöneticisi Araçları uzantısı ile Visual Studio Code. Bkz. [Azure Resource Manager şablonları oluşturmak için Visual Studio Code kullanma](./resource-manager-tools-vs-code.md).
* Gözden geçirme [öğretici: Azure Resource Manager şablonları ile içeri aktarma SQL BACPAC dosyalarını](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md). Bu öğreticide kullanılan Bu öğreticide geliştirilen bir şablonudur. Bu makalede bir indirme bağlantısı tamamlanmış şablonu sağlanır.
* Güvenliği artırmak istiyorsanız SQL Server yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```azurecli-interactive
    openssl rand -base64 32
    ```

    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için bkz. [Öğretici: Azure Key Vault'u Resource Manager şablonu dağıtımıyla tümleştirme](./resource-manager-tutorial-use-key-vault.md). Ayrıca parolanızı üç ayda bir güncelleştirmenizi öneririz.

## <a name="prepare-a-bacpac-file"></a>BACPAC dosyası hazırlama

Bu bölümde, Resource Manager şablonu dağıtırken dosyayı güvenli bir şekilde erişilebilir olacak şekilde BACPAC dosyasını hazırlayın. Bu bölümde beş yordamı vardır:

* BACPAC dosyasını indirin.
* Bir Azure Depolama hesabı oluşturun.
* Bir depolama hesabı Blob kapsayıcı oluşturun.
* BACPAC dosyasını kapsayıcıya yükleyin.
* BACPAC dosyası SAS belirtecini alır.

1. Cloud Shell 'i açmak için **deneyin** ' i seçin ve ardından aşağıdaki PowerShell betiğini kabuk penceresine yapıştırın.

    ```azurepowershell-interactive
    $projectName = Read-Host -Prompt "Enter a project name"   # This name is used to generate names for Azure resources, such as storage account name.
    $location = Read-Host -Prompt "Enter a location (i.e. centralus)"

    $resourceGroupName = $projectName + "rg"
    $storageAccountName = $projectName + "store"
    $containerName = "bacpacfile" # The name of the Blob container to be created.

    $bacpacURL = "https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-sql-extension/SQLDatabaseExtension.bacpac"
    $bacpacFileName = "SQLDatabaseExtension.bacpac" # A file name used for downloading and uploading the BACPAC file.

    # Download the bacpac file
    Invoke-WebRequest -Uri $bacpacURL -OutFile "$home/$bacpacFileName"

    # Create a resource group
    New-AzResourceGroup -Name $resourceGroupName -Location $location

    # Create a storage account
    $storageAccount = New-AzStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $storageAccountName `
        -Location $location `
        -SkuName "Standard_LRS"

    $context = $storageAccount.Context

    # Create a container
    New-AzStorageContainer -Name $containerName -Context $context

    # Upload the bacpac file
    Set-AzStorageBlobContent `
        -Container $containerName `
        -File "$home/$bacpacFileName" `
        -Blob $bacpacFileName `
        -Context $context

    # Generate a SAS token
    $bacpacURI = New-AzStorageBlobSASToken `
        -Context $context `
        -Container $containerName `
        -Blob $bacpacFileName `
        -Permission r `
        -ExpiryTime (Get-Date).AddHours(8.0) `
        -FullUri

    $str = $bacpacURI.split("?")

    Write-Host "You need the blob url and the SAS token later in the tutorial:"
    Write-Host $str[0]
    Write-Host (-join ("?", $str[1]))

    Write-Host "Press [ENTER] to continue ..."
    ```

1. BACPAC dosya URL 'sini ve SAS belirtecini yazın. Şablonu dağıtırken bu değerlere ihtiyacınız vardır.

## <a name="open-an-existing-template"></a>Mevcut bir şablonu açın

Bu oturumda, oluşturduğunuz şablonu değiştirmek [öğretici: Azure Resource Manager şablonları ile içeri aktarma SQL BACPAC dosyalarını](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md) BACPAC dosyasını bir SAS belirteci ile çağırmak için.  SQL uzantı öğreticisinde geliştirilen şablon [GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json)'da paylaşılır.

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy2.json
    ```

3. Dosyayı açmak için **Aç**’ı seçin.

    Şablonda tanımlanmış dört kaynak vardır:

   * `Microsoft.Sql/servers`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/2015-05-01-preview/servers).
   * `Microsoft.SQL/servers/firewallRules`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/2015-05-01-preview/servers/firewallrules).
   * `Microsoft.SQL/servers/databases`.  Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).
   * `Microsoft.SQL/server/databases/extensions`.  Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions).

     Şablonu özelleştirmeden önce temel noktaları kavramak faydalı olacaktır.
4. **Dosya**>**Farklı Kaydet**'i seçerek dosyanın bir kopyasını yerel bilgisayarınıza **azuredeploy.json** adıyla kaydedin.

## <a name="edit-the-template"></a>Şablonu düzenleme

1. StorageAccountKey parametre tanımını şu parametre tanımıyla değiştirin:

    ```json
    "_artifactsLocationSasToken": {
      "type": "securestring",
      "metadata": {
        "description": "Specifies the SAS token required to access the artifact location."
      }
    },
    ```

    ![Resource Manager Öğreticisi güvenli yapıtları parametreleri](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-secure-artifacts-parameters.png)

2. SQL uzantı kaynağının aşağıdaki üç öğesinin değerini güncelleştirin:

    ```json
    "storageKeyType": "SharedAccessKey",
    "storageKey": "[parameters('_artifactsLocationSasToken')]",
    "storageUri": "[parameters('bacpacUrl')]",
    ```

Tamamlanan şablon aşağıdaki gibi görünür:

[!code-json[](~/resourcemanager-templates/tutorial-sql-extension/azuredeploy3.json?range=1-106&highlight=38-43,95-97)]

## <a name="deploy-the-template"></a>Şablonu dağıtma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Dağıtım yordamı için [Şablonu dağıtma](./resource-manager-tutorial-create-multiple-instances.md#deploy-the-template) bölümüne bakın. Bunun yerine aşağıdaki PowerShell dağıtım betiğini kullanın:

```azurepowershell
$projectName = Read-Host -Prompt "Enter the project name that is used earlier"   # This name is used to generate names for Azure resources, such as storage account name.
$location = Read-Host -Prompt "Enter a location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the sql database admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
$bacpacUrl = Read-Host -Prompt "Enter the BACPAC url"
$artifactsLocationSasToken = Read-Host -Prompt "Enter the artifacts location SAS token" -AsSecureString

$resourceGroupName = $projectName + "rg"

#New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -_artifactsLocationSasToken $artifactsLocationSasToken `
    -bacpacUrl $bacpacUrl `
    -TemplateFile "$HOME/azuredeploy.json"

Write-Host "Press [ENTER] to continue ..."
```

Oluşturulan bir parola kullanın. [Ön koşullara](#prerequisites) bakın.
_ArtifactsLocation, _artifactsLocationSasToken ve bacpacFileName değerleri için bkz. [BACPAC dosyası hazırlama](#prepare-a-bacpac-file).

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

Portalda yeni dağıtılan kaynak grubundaki SQL veritabanını seçin. **Sorgu düzenleyicisi (önizleme)** öğesini seçip yönetici kimlik bilgilerini girin. İki tablonun veritabanına aktarılmış olması gerekir:

![Azure Resource Manager SQL uzantılarını dağıtma BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-query-editor.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir SQL veritabanı bir SQL sunucusu dağıttınız ve SAS belirteci kullanarak BACPAC dosyasını içeri aktarıldı. Kaynak Yöneticisi şablonlarını sürekli olarak geliştirip dağıtmak için Azure işlem hattı oluşturmayı öğrenmek için bkz.

> [!div class="nextstepaction"]
> [Azure işlem hattı ile sürekli tümleştirme](./resource-manager-tutorial-use-azure-pipelines.md)
