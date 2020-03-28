---
title: Şablonlarda güvenli yapılar
description: Azure Kaynak Yöneticisi şablonlarınızda kullanılan yapıtların nasıl güvende olduğunu öğrenin.
author: mumian
ms.date: 12/09/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: ad6ea3c68ed6f48ac48bbbdafed7f8660df23937
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80239222"
---
# <a name="tutorial-secure-artifacts-in-arm-template-deployments"></a>Öğretici: ARM şablon dağıtımlarında güvenli yapılar

Paylaşılan erişim imzalarına (SAS) sahip bir Azure Depolama hesabı kullanarak Azure Kaynak Yöneticisi (ARM) şablonlarınızda kullanılan yapıları nasıl güvene getireceğinizi öğrenin. Dağıtım yapıları, dağıtımtamamlamak için gereken ana şablon dosyasına ek olarak tüm dosyalardır. Örneğin, [Tutorial: ARM şablonlu SQL BACPAC dosyalarını içe aktarın,](./template-tutorial-deploy-sql-extensions-bacpac.md)ana şablon bir Azure SQL Veritabanı örneği oluşturur. Ayrıca tablolar oluşturmak ve veri eklemek için bir BACPAC dosyası çağırır. BACPAC dosyası bir yapıdır ve bir Azure Depolama hesabında depolanır. Yapıya erişmek için bir depolama hesabı anahtarı kullanıldı.

Bu eğitimde, kendi Azure Depolama hesabınızdaki BACPAC dosyasına sınırlı erişim sağlamak için SAS'ı kullanırsınız. SAS hakkında daha fazla bilgi için [bkz.](../../storage/common/storage-dotnet-shared-access-signature-part-1.md)

Bağlantılı bir şablonu nasıl güvenli hale getireceğinizi öğrenmek için [Bkz. Öğretici: Bağlantılı ARM şablonları oluşturun.](./template-tutorial-create-linked-templates.md)

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Bir BACPAC dosyası hazırlayın.
> * Varolan bir şablonu açın.
> * Şablonu edin.
> * Şablonu dağıtın.
> * Dağıtımı doğrulayın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Kaynak Yöneticisi Araçları uzantılı Visual Studio Kodu. Bkz. [ARM şablonları oluşturmak için Görsel Stüdyo Kodunu Kullan.](./use-vs-code-to-create-template.md)
* [İnceleme Öğretici: ARM şablonları ile SQL BACPAC dosyaları içe aktarın.](./template-tutorial-deploy-sql-extensions-bacpac.md) Bu öğreticide kullanılan şablon, bu öğreticide geliştirilmiş olandır. Tamamlanan şablonun indirme bağlantısı bu makalede sağlanır.
* Güvenliği artırmak istiyorsanız SQL Server yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturmak için kullanabileceğiniz bir örnek aşağıda verebilirsiniz:

    ```console
    openssl rand -base64 32
    ```

    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için [Bkz. Öğretici: Azure Anahtar Kasası'nı ARM şablon dağıtımına entegre edin.](./template-tutorial-use-key-vault.md) Ayrıca parolanızı üç ayda bir güncelleştirmenizi öneririz.

## <a name="prepare-a-bacpac-file"></a>BACPAC dosyası hazırlama

Bu bölümde, ARM şablonu dağıtırken dosyaya güvenli bir şekilde erişilebilmek için BACPAC dosyasını hazırlarsınız. Bu bölümde beş yordam vardır:

* BACPAC dosyasını indirin.
* Bir Azure Depolama hesabı oluşturun.
* Bir depolama hesabı blob kapsayıcı oluşturun.
* BACPAC dosyasını kapsayıcıya yükleyin.
* BACPAC dosyasının SAS belirteci alın.

1. Bulut kabuğunu açmak için **deneyin'i** seçin. Ardından aşağıdaki PowerShell komut dosyasını kabuk penceresine yapıştırın.

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

1. BACPAC dosya URL'sini ve SAS belirteci'ni yazın. Şablonu dağıtırken bu değerlere ihtiyacınız var.

## <a name="open-an-existing-template"></a>Varolan bir şablonu açma

Bu oturumda, [Tutorial: SQL BACPAC dosyalarını ARM şablonlu içe aktararak](./template-tutorial-deploy-sql-extensions-bacpac.md) BACPAC dosyasını SAS belirteciyle aramak için değiştirin. SQL uzantılı öğreticide geliştirilen şablon [GitHub'da](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json)paylaşılır.

1. Visual Studio Code'dan **Dosya** > **Aç Dosya'yı**seçin.
1. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy2.json
    ```

1. Dosyayı açmak için **Aç**’ı seçin.

    Şablonda tanımlanan dört kaynak vardır:

   * `Microsoft.Sql/servers`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/2015-05-01-preview/servers).
   * `Microsoft.SQL/servers/firewallRules`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/2015-05-01-preview/servers/firewallrules).
   * `Microsoft.SQL/servers/databases`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).
   * `Microsoft.SQL/server/databases/extensions`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions).

        Şablonu özelleştirmeden önce şablonhakkında temel bir anlayış elde etmek yararlı dır.
1. Dosyanın bir kopyasını *azuredeploy.json*adıyla yerel bilgisayarınıza kaydetmek için **Dosya** > **Yı Kaydet'i** seçin.

## <a name="edit-the-template"></a>Şablonu düzenleme

1. storageAccountKey parametre tanımını aşağıdaki parametre tanımıyla değiştirin:

    ```json
        "_artifactsLocationSasToken": {
          "type": "securestring",
          "metadata": {
            "description": "Specifies the SAS token required to access the artifact location."
          }
        },
    ```

    ![Kaynak Yöneticisi öğretici güvenli yapılar parametreleri](./media/template-tutorial-secure-artifacts/resource-manager-tutorial-secure-artifacts-parameters.png)

1. SQL uzantı kaynağının aşağıdaki üç öğesinin değerini güncelleştirin:

    ```json
    "storageKeyType": "SharedAccessKey",
    "storageKey": "[parameters('_artifactsLocationSasToken')]",
    "storageUri": "[parameters('bacpacUrl')]",
    ```

Tamamlanan şablon aşağıdaki gibi görünür:

[!code-json[](~/resourcemanager-templates/tutorial-sql-extension/azuredeploy3.json?range=1-106&highlight=38-43,95-97)]

## <a name="deploy-the-template"></a>Şablonu dağıtma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Dağıtım [yordamı](./template-tutorial-create-multiple-instances.md#deploy-the-template) için şablonu dağıt bölümüne bakın. Bunun yerine aşağıdaki PowerShell dağıtım komut dosyasını kullanın.

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

Oluşturulan parolakullanın. [Ön koşullara](#prerequisites) bakın.
_artifactsLocation, _artifactsLocationSasToken ve bacpacFileName değerleri için bakınız [BIR BACPAC dosyası hazırla.](#prepare-a-bacpac-file)

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

Portalda yeni dağıtılan kaynak grubundaki SQL veritabanını seçin. **Sorgu düzenleyicisi (önizleme)** öğesini seçip yönetici kimlik bilgilerini girin. Veritabanına içe aktarılan iki tablo görürsünüz.

![Sorgu düzenleyicisi (önizleme)](./media/template-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-query-editor.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalında, sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin. Kaynak grubunda toplam altı kaynak görürsünüz.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir SQL sunucusu ve bir SQL veritabanı dağıttınız ve bir SAS belirteci kullanarak bir BACPAC dosyası aldınız. Azure kaynaklarını birden fazla bölgede dağıtma ve güvenli dağıtım uygulamalarını kullanma hakkında bilgi edinmek için bkz.

> [!div class="nextstepaction"]
> [Güvenli dağıtım uygulamaları kullanma](./deployment-manager-tutorial.md)
