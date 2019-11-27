---
title: Şablonlarda güvenli yapıtlar
description: Yapıtlar, Azure Resource Manager şablonlarında kullanılan güvenli hale getirmeyi öğrenin.
author: mumian
ms.date: 10/08/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: b37f7e284b655a362c5a4231a7c1da3719762644
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74326434"
---
# <a name="tutorial-secure-artifacts-in-azure-resource-manager-template-deployments"></a>Öğretici: Azure Resource Manager şablon dağıtımları yapıları güvenliğini sağlama

Paylaşılan erişim imzaları (SAS) ile Azure depolama hesabı kullanarak, Azure Resource Manager şablonlarında kullanılan yapıtların güvenli hale getirmeyi öğrenin. Dağıtım yapıtları dağıtımı tamamlamak için gereken tüm ana şablon dosyası yanı sıra dosyalarıdır. Örneğin, [öğretici: SQL BACPAC dosyalarını Azure Resource Manager şablonlarıyla Içeri aktarma](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md)ana şablon BIR Azure SQL veritabanı oluşturur; Ayrıca tablo oluşturmak ve veri eklemek için BACPAC dosyasını çağırır. BACPAC dosyasını bir yapıdır. Yapıt ortak erişim ile bir Azure depolama hesabında depolanır. Bu öğreticide, SAS kendi Azure depolama hesabında BACPAC dosyasına sınırlı erişim vermek için kullanın. SAS hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanma](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

Bağlı şablonu güvenli hale getirme hakkında bilgi edinmek için bkz. [öğretici: bağlı Azure Resource Manager şablonları oluşturma](./resource-manager-tutorial-create-linked-templates.md).

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * BACPAC dosyası hazırlama
> * Mevcut bir şablonu açın
> * Şablonu düzenleme
> * Şablonu dağıtma
> * Dağıtımı doğrulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Kaynak Yöneticisi Araçları uzantısı ile Visual Studio Code. Bkz. [Azure Resource Manager şablonları oluşturmak için Visual Studio Code kullanma](./resource-manager-tools-vs-code.md).
* Gözden geçirme [öğreticisi: SQL BACPAC dosyalarını Azure Resource Manager şablonlarıyla Içeri aktarma](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md). Bu öğreticide kullanılan Bu öğreticide geliştirilen bir şablonudur. Bu makalede bir indirme bağlantısı tamamlanmış şablonu sağlanır.
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

Bir PowerShell betiği kullanarak bu adımları otomatik hale getirmek için, [bağlantılı şablonu karşıya yükleme](./resource-manager-tutorial-create-linked-templates.md#upload-the-linked-template)komut dosyasına bakın.

### <a name="download-the-bacpac-file"></a>BACPAC dosyasını indirin

[Bacpac dosyasını](https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-sql-extension/SQLDatabaseExtension.bacpac)indirin ve dosyayı, **sqldatabaseextension. bacpac**adlı yerel bilgisayarınıza kaydedin.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

1. Aşağıdaki görüntüde Azure portalında Resource Manager şablonu açmak için seçin.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-storage-account-create%2fazuredeploy.json" target="_blank"><img src="./media/resource-manager-tutorial-secure-artifacts/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Aşağıdaki özellikleri girin:

    * **Abonelik**: Azure aboneliğinizi seçin.
    * **Kaynak grubu**: **Yeni oluştur** ' u seçin ve bir ad verin. Bir kaynak grubu bir Azure kaynakları için yönetim amaç için kapsayıcıdır. Bu öğreticide, depolama hesabı ve Azure SQL veritabanı için aynı kaynak grubunu kullanabilirsiniz. Bu kaynak grubu adını not edin, sonraki öğreticilerde Azure SQL veritabanı oluşturduğunuzda gerekir.
    * **Konum**: bölge seçin. Örneğin, **Orta ABD**.
    * **Depolama hesabı türü**: **Standard_LRS**varsayılan değeri kullanın.
    * **Konum**: **[resourceGroup (). Location]** olan varsayılan değeri kullanın. Depolama hesabı için kaynak grubu konumunu kullanmak anlamına gelir.
    * **Yukarıda başlatılan hüküm ve koşulları kabul ediyorum**: (seçili)
3. **Satın al**'ı seçin.
4. Dağıtım durumunu görmek için portalının sağ üst köşesindeki bildirim simgesine (zil simgesi) seçin.

    ![Resource Manager Öğreticisi portal bildirimler bölmesi](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-portal-notifications-pane.png)
5. Depolama hesabı başarıyla dağıtıldıktan sonra, kaynak grubunu açmak için Bildirim bölmesinden **kaynak grubuna git** ' i seçin.

### <a name="create-a-blob-container"></a>Blob kapsayıcısı oluşturma

Dosyaları karşıya yükleyebilmeniz, bir Blob kapsayıcısı gereklidir.

1. Açmak için depolama hesabını seçin. Kaynak grubunda listelenen yalnızca bir depolama hesabı göreceksiniz. Depolama hesabınızın adını, aşağıdaki ekran görüntüsünde gösterildiği bir farklıdır.

    ![Resource Manager Öğreticisi depolama hesabı](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-storage-account.png)

2. **Bloblar** kutucuğunu seçin.

    ![Resource Manager Öğreticisi BLOB'ları](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-blobs.png)
3. Yeni bir kapsayıcı oluşturmak için yukarıdan **kapsayıcı** ' ı seçin.
4. Aşağıdaki değerleri girin:

    * **Ad**: **sqlbacpac**girin.
    * **Genel erişim düzeyi**: **özel (anonim erişim yok)** varsayılan değerini kullanın.
5. **Tamam**’ı seçin.
6. Yeni oluşturulan kapsayıcıyı açmak için **sqlbacpac** öğesini seçin.

### <a name="upload-the-bacpac-file-to-the-container"></a>BACPAC dosyasını kapsayıcıya yüklemek

1. **Karşıya Yükle**’yi seçin.
2. Aşağıdaki değerleri girin:

    * **Dosyalar**: daha önce indirdiğiniz bacpac dosyasını seçmek için yönergeleri takip edin. Varsayılan ad **Sqldatabaseextension. bacpac**' dir.
    * **Kimlik doğrulama türü**: **SAS**seçin.  *SAS* varsayılan değerdir.
3. **Karşıya Yükle**’yi seçin.  Dosya başarıyla karşıya yüklendikten sonra dosya adı kapsayıcıda listelenen.

### <a name="a-namegenerate-a-sas-token-generate-a-sas-token"></a>SAS belirteci oluşturma <a name="generate-a-sas-token" />

1. Kapsayıcıdan **Sqldatabaseextension. bacpac öğesine** sağ tıklayın ve ardından **SAS oluştur**' u seçin.
2. Aşağıdaki değerleri girin:

    * **İzin**: varsayılan, **Oku**kullanın.
    * **Başlangıç ve bitiş tarihi/saati**: varsayılan değer, SAS belirtecini kullanmak için size sekiz saat verir. Bu öğreticiyi tamamlaması için daha fazla zaman gerekiyorsa, **tarihi**Güncelleştir ' i güncelleştirin.
    * **Izin VERILEN IP adresleri**: bu alanı boş bırakın.
    * **Izin verilen protokoller**: varsayılan değeri kullanın: **https**.
    * **İmzalama anahtarı**: varsayılan değeri kullanın: **Key 1**.
3. **BLOB SAS belirteci oluştur ve URL 'yi**seçin.
4. **BLOB SAS URL**'sinin bir kopyasını oluşturun. URL 'nin ortasında, **Sqldatabaseextension. bacpac**adlı dosya adı bulunur.  Dosya adı, URL üç bölüme ayırır:

   - **Yapıt konumu**: https://xxxxxxxxxxxxxx.blob.core.windows.net/sqlbacpac/. Emin olun konumu ile biten bir "/".
   - **Bacpac dosya adı**: sqldatabaseextension. bacpac.
   - **Yapıt konumu SAS belirteci**: belirtecin "?" ile önce geldiğinden emin olun.

     [Şablonu dağıtırken](#deploy-the-template)bu üç değere ihtiyacınız vardır.

## <a name="open-an-existing-template"></a>Mevcut bir şablonu açın

Bu oturumda, öğreticide oluşturduğunuz şablonu değiştirirsiniz: BACPAC dosyasını bir SAS belirteci ile çağırmak için [SQL bacpac dosyalarını Azure Resource Manager şablonlarla Içeri aktarın](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md) .  SQL uzantı öğreticisinde geliştirilen şablon [GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json)'da paylaşılır.

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç**’ı seçin.

    Şablonda tanımlanan beş kaynak vardır:

   * `Microsoft.Sql/servers`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/2015-05-01-preview/servers).
   * `Microsoft.SQL/servers/securityAlertPolicies`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/securityalertpolicies).
   * `Microsoft.SQL/servers/filewallRules`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/2015-05-01-preview/servers/firewallrules).
   * `Microsoft.SQL/servers/databases`.  Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).
   * `Microsoft.SQL/server/databases/extensions`.  Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions).

     Şablonu özelleştirmeden önce temel noktaları kavramak faydalı olacaktır.
4. **Dosya**>**Farklı Kaydet**'i seçerek dosyanın bir kopyasını yerel bilgisayarınıza **azuredeploy.json** adıyla kaydedin.

## <a name="edit-the-template"></a>Şablonu düzenleme

Aşağıdaki ek parametreleri ekleyin:

```json
"_artifactsLocation": {
    "type": "string",
    "metadata": {
        "description": "The base URI where artifacts required by this template are located."
    }
},
"_artifactsLocationSasToken": {
    "type": "securestring",
    "metadata": {
        "description": "The sasToken required to access _artifactsLocation."
    },
    "defaultValue": ""
},
"bacpacFileName": {
    "type": "string",
    "defaultValue": "SQLDatabaseExtension.bacpac",
    "metadata": {
        "description": "The bacpac for configure the database and tables."
    }
}
```

![Resource Manager Öğreticisi güvenli yapıtları parametreleri](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-secure-artifacts-parameters.png)

Değeri şu iki öğeyi güncelleştirin:

```json
"storageKey": "[parameters('_artifactsLocationSasToken')]",
"storageUri": "[uri(parameters('_artifactsLocation'), parameters('bacpacFileName'))]",
```

## <a name="deploy-the-template"></a>Şablonu dağıtma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Dağıtım yordamı için [Şablonu dağıtma](./resource-manager-tutorial-create-multiple-instances.md#deploy-the-template) bölümüne bakın. Bunun yerine aşağıdaki PowerShell dağıtım betiğini kullanın:

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the virtual machine admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
$artifactsLocation = Read-Host -Prompt "Enter the artifacts location"
$artifactsLocationSasToken = Read-Host -Prompt "Enter the artifacts location SAS token" -AsSecureString
$bacpacFileName = Read-Host -Prompt "Enter the BACPAC file name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -_artifactsLocation $artifactsLocation `
    -_artifactsLocationSasToken $artifactsLocationSasToken `
    -bacpacFileName $bacpacFileName `
    -TemplateFile "$HOME/azuredeploy.json"
```

Oluşturulan bir parola kullanın. [Ön koşullara](#prerequisites) bakın.
_ArtifactsLocation, _artifactsLocationSasToken ve bacpacFileName değerleri için bkz. [SAS belirteci oluşturma](#generate-a-sas-token).

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
