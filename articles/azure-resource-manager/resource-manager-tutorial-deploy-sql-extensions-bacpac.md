---
title: SQL BACPAC dosyalarını Azure Resource Manager şablonlarıyla içeri aktarma | Microsoft Docs
description: SQL veritabanı uzantısı 'nı kullanarak SQL BACPAC dosyalarını Azure Resource Manager şablonlarıyla içeri aktarma hakkında bilgi edinin.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 04/08/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 239bb77d486e8cb845ec439d84def5e34cf64348
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170224"
---
# <a name="tutorial-import-sql-bacpac-files-with-azure-resource-manager-templates"></a>Öğretici: Azure Resource Manager şablonlarıyla SQL BACPAC dosyalarını Içeri aktarma

Azure Resource Manager şablonlarıyla BACPAC dosyasını içeri aktarmak için Azure SQL veritabanı uzantıları 'nı nasıl kullanacağınızı öğrenin. Dağıtım yapıtları, bir dağıtımı tamamlaması gereken ana şablon dosyasına ek olarak herhangi bir dosyadır. BACPAC dosyası bir yapıdır. Bu öğreticide, bir Azure SQL Server, bir SQL veritabanını dağıtmak ve BACPAC dosyasını içeri aktarmak için bir şablon oluşturacaksınız. Azure Resource Manager şablonları kullanarak Azure sanal makine uzantıları dağıtma hakkında daha fazla bilgi için bkz. [# öğreticisi: Azure Resource Manager şablonlarla sanal makine uzantıları dağıtma](./resource-manager-tutorial-deploy-vm-extensions.md).

Bu öğretici aşağıdaki görevleri ele almaktadır:

> [!div class="checklist"]
> * BACPAC dosyası hazırlama
> * Hızlı başlangıç şablonu açma
> * Şablonu düzenleme
> * Şablonu dağıtma
> * Dağıtımı doğrulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlayabilmeniz için şunlar gerekir:

* Kaynak Yöneticisi Araçları uzantısıyla [Visual Studio Code](https://code.visualstudio.com/) . Bkz. [uzantıyı yüklemeyi ](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Güvenliği artırmak için, SQL Server yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturmak için bir örnek aşağıda verilmiştir:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault, şifreleme anahtarlarını ve diğer gizli dizileri korumak için tasarlanmıştır. Daha fazla bilgi için bkz. [öğretici: Kaynak Yöneticisi Şablon dağıtımı Azure Key Vault tümleştirme](./resource-manager-tutorial-use-key-vault.md). Parolanızı her üç ayda bir de güncelleştirmeniz önerilir.

## <a name="prepare-a-bacpac-file"></a>BACPAC dosyası hazırlama

Bir BACPAC dosyası [GitHub](https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-sql-extension/SQLDatabaseExtension.bacpac)'da paylaşılır. Kendinizinkini oluşturmak için bkz. [Azure SQL veritabanını BACPAC dosyasına aktarma](../sql-database/sql-database-export.md). Dosyayı kendi konumunuzda yayımlamayı seçerseniz, bu şablonu öğreticide daha sonra güncelleştirmeniz gerekir.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonu açma

Bu öğreticide kullanılan şablon [GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json)' da depolanır.

1. Visual Studio Code, **dosya**> dosya**Aç**' ı seçin.
2. **Dosya adı**' nda aşağıdaki URL 'yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç** ' ı seçin.

    Şablonda üç kaynak tanımlı:

   * `Microsoft.Sql/servers`. [Şablon başvurusuna](https://docs.microsoft.com/azure/templates/microsoft.sql/servers)bakın.
   * `Microsoft.SQL/servers/securityAlertPolicies`. [Şablon başvurusuna](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/securityalertpolicies)bakın.
   * `Microsoft.SQL.servers/databases`.  [Şablon başvurusuna](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases)bakın.

     Özelleştirmeden önce şablon hakkında bazı temel bilgileri almak faydalı olur.
4. Dosyanın bir kopyasını yerel bilgisayarınıza **azuredeploy. JSON**adıyla kaydetmek için **Dosya**>**farklı kaydet** ' i seçin.

## <a name="edit-the-template"></a>Şablonu düzenleme

Şablona iki ek kaynak ekleyin.

* SQL veritabanı uzantısının BACPAC dosyalarını içeri aktarmasını sağlamak için Azure hizmetlerine erişime izin vermeniz gerekir. Aşağıdaki JSON 'ı SQL Server tanımına ekleyin:

    ```json
    {
        "type": "firewallrules",
        "name": "AllowAllAzureIps",
        "location": "[parameters('location')]",
        "apiVersion": "2015-05-01-preview",
        "dependsOn": [
            "[variables('databaseServerName')]"
        ],
        "properties": {
            "startIpAddress": "0.0.0.0",
            "endIpAddress": "0.0.0.0"
        }
    }
    ```

    Şablon şöyle görünmelidir:

    ![SQL uzantıları BACPAC 'yi dağıtma Azure Resource Manager](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-firewall.png)

* Aşağıdaki JSON ile veritabanı tanımına bir SQL veritabanı uzantısı kaynağı ekleyin:

    ```json
    "resources": [
        {
            "name": "Import",
            "type": "extensions",
            "apiVersion": "2014-04-01",
            "dependsOn": [
                "[resourceId('Microsoft.Sql/servers/databases', variables('databaseServerName'), variables('databaseName'))]"
            ],
            "properties": {
                "storageKeyType": "SharedAccessKey",
                "storageKey": "?",
                "storageUri": "https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-sql-extension/SQLDatabaseExtension.bacpac",
                "administratorLogin": "[variables('databaseServerAdminLogin')]",
                "administratorLoginPassword": "[variables('databaseServerAdminLoginPassword')]",
                "operationMode": "Import",
            }
        }
    ]
    ```

    Şablon şöyle görünmelidir:

    ![SQL uzantıları BACPAC 'yi dağıtma Azure Resource Manager](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac.png)

    Kaynak tanımını anlamak için bkz. [SQL veritabanı uzantısı başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases/extensions). Aşağıda bazı önemli öğeler verilmiştir:

    * **Bağımlıdson**: SQL veritabanı oluşturulduktan sonra uzantı kaynağı oluşturulmalıdır.
    * **storageKeyType**: kullanılacak depolama anahtarının türü. Değer `StorageAccessKey` ya da `SharedAccessKey` olabilir. Belirtilen BACPAC dosyası ortak erişime sahip bir Azure depolama hesabında paylaşıldığından, burada ' SharedAccessKey ' kullanılır.
    * **StorageKey**: kullanılacak depolama anahtarı. Depolama anahtarı türü SharedAccessKey ise, önüne "?" gelmelidir.
    * **storageuri**: kullanılacak depolama URI 'si. Belirtilen BACPAC dosyasını kullanmayı tercih ederseniz, değerleri güncelleştirmeniz gerekir.
    * **Tınloginpassword**: SQL yöneticisinin parolası. Oluşturulan bir parola kullanın. [Önkoşulları](#prerequisites)inceleyin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Dağıtım yordamının [şablonu dağıtma](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) bölümüne bakın. Bunun yerine aşağıdaki PowerShell dağıtım betiğini kullanın:

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the SQL admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -TemplateFile "$HOME/azuredeploy.json"
```

Oluşturulan bir parola kullanın. [Önkoşulları](#prerequisites)inceleyin.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

Portalda, yeni dağıtılan kaynak grubundan SQL veritabanı ' nı seçin. **Sorgu Düzenleyicisi (Önizleme)** öğesini seçin ve ardından yönetici kimlik bilgilerini girin. Veritabanına aktarılmış iki tablo göreceksiniz:

![SQL uzantıları BACPAC 'yi dağıtma Azure Resource Manager](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-query-editor.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure kaynaklarına artık ihtiyaç duyulmadığında, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portal, sol menüden **kaynak grubu** ' nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubunda toplam altı kaynak görürsünüz.
4. Üstteki menüden **kaynak grubunu sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir SQL Server, SQL veritabanı dağıtmış ve BACPAC dosyasını içeri aktardınız. BACPAC dosyası bir Azure depolama hesabında depolanır. URL ile anybody dosyaya erişebilir. BACPAC dosyasını (yapıt) güvenli hale getirme hakkında bilgi edinmek için bkz.

> [!div class="nextstepaction"]
> [Yapıtları güvenli hale getirme](./resource-manager-tutorial-secure-artifacts.md)
