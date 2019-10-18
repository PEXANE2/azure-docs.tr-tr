---
title: Güvenli yapıtlar-Azure Resource Manager şablonları
description: Azure Resource Manager şablonlarında kullanılan yapıtları güvenli hale getirme hakkında bilgi edinin.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/08/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: d9570c30e2870a136bd97ed5fc2304d6f0d280c9
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72528280"
---
# <a name="tutorial-secure-artifacts-in-azure-resource-manager-template-deployments"></a>Öğretici: Azure Resource Manager şablonu dağıtımlarında güvenli yapıtlar

Paylaşılan erişim imzaları (SAS) ile Azure depolama hesabı 'nı kullanarak Azure Resource Manager şablonlarında kullanılan yapıtların güvenliğini nasıl sağlayacağınızı öğrenin. Dağıtım yapıtları, ana şablon dosyasına ek olarak, bir dağıtımı tamamlaması için gereken tüm dosyalardır. Örneğin, [öğretici: SQL BACPAC dosyalarını Azure Resource Manager şablonlarıyla Içeri aktarma](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md)ana şablon BIR Azure SQL veritabanı oluşturur; Ayrıca tablo oluşturmak ve veri eklemek için BACPAC dosyasını çağırır. BACPAC dosyası bir yapıdır. Yapıt, genel erişime sahip bir Azure depolama hesabında depolanır. Bu öğreticide, kendi Azure Depolama hesabınızdaki BACPAC dosyasına sınırlı erişim sağlamak için SAS kullanacaksınız. SAS hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanma](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

Bağlı şablonu güvenli hale getirme hakkında bilgi edinmek için bkz. [öğretici: bağlı Azure Resource Manager şablonları oluşturma](./resource-manager-tutorial-create-linked-templates.md).

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

* [Visual Studio Code](https://code.visualstudio.com/) ve Resource Manager Araçları uzantısı. Bkz. [Uzantıyı yükleme](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Gözden geçirme [öğreticisi: SQL BACPAC dosyalarını Azure Resource Manager şablonlarıyla Içeri aktarma](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md). Bu öğreticide kullanılan şablon, bu öğreticide geliştirilen bir şablondur. Tamamlanmış şablonun karşıdan yükleme bağlantısı bu makalede verilmiştir.
* Güvenliği artırmak istiyorsanız SQL Server yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için bkz. [Öğretici: Azure Key Vault'u Resource Manager şablonu dağıtımıyla tümleştirme](./resource-manager-tutorial-use-key-vault.md). Ayrıca parolanızı üç ayda bir güncelleştirmenizi öneririz.

## <a name="prepare-a-bacpac-file"></a>BACPAC dosyası hazırlama

Bu bölümde, Kaynak Yöneticisi şablonunu dağıtırken dosyanın güvenli bir şekilde erişilebilir olması için BACPAC dosyasını hazırlarsınız. Bu bölümde beş yordam vardır:

* BACPAC dosyasını indirin.
* Bir Azure Depolama hesabı oluşturun.
* Depolama hesabı blobu kapsayıcısı oluşturun.
* BACPAC dosyasını kapsayıcıya yükleyin.
* BACPAC dosyasının SAS belirtecini alın.

Bir PowerShell betiği kullanarak bu adımları otomatik hale getirmek için, [bağlantılı şablonu karşıya yükleme](./resource-manager-tutorial-create-linked-templates.md#upload-the-linked-template)komut dosyasına bakın.

### <a name="download-the-bacpac-file"></a>BACPAC dosyasını indirin

[Bacpac dosyasını](https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-sql-extension/SQLDatabaseExtension.bacpac)indirin ve dosyayı, **sqldatabaseextension. bacpac**adlı yerel bilgisayarınıza kaydedin.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

1. Azure portal bir Kaynak Yöneticisi şablonu açmak için aşağıdaki görüntüyü seçin.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-storage-account-create%2fazuredeploy.json" target="_blank"><img src="./media/resource-manager-tutorial-secure-artifacts/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Aşağıdaki özellikleri girin:

    * **Abonelik**: Azure aboneliğinizi seçin.
    * **Kaynak grubu**: **Yeni oluştur** ' u seçin ve bir ad verin. Kaynak grubu, yönetim amacına yönelik olarak Azure kaynakları için bir kapsayıcıdır. Bu öğreticide, depolama hesabı ve Azure SQL veritabanı için aynı kaynak grubunu kullanabilirsiniz. Bu kaynak grubu adını, öğreticilerde daha sonra Azure SQL veritabanı oluşturduğunuzda ihtiyacınız olduğunu unutmayın.
    * **Konum**: bölge seçin. Örneğin, **Orta ABD**.
    * **Depolama hesabı türü**: **Standard_LRS**olan varsayılan değeri kullanın.
    * **Konum**: **[resourceGroup (). Location]** olan varsayılan değeri kullanın. Bu, depolama hesabı için kaynak grubu konumunu kullandığınız anlamına gelir.
    * **Yukarıda başlatılan hüküm ve koşulları kabul ediyorum**: (seçili)
3. **Satın al**'ı seçin.
4. Dağıtım durumunu görmek için portalın sağ üst köşesindeki bildirim simgesini (zil simgesi) seçin.

    ![Kaynak Yöneticisi öğreticisi Portal bildirimleri bölmesi](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-portal-notifications-pane.png)
5. Depolama hesabı başarıyla dağıtıldıktan sonra, kaynak grubunu açmak için Bildirim bölmesinden **kaynak grubuna git** ' i seçin.

### <a name="create-a-blob-container"></a>Blob kapsayıcısı oluşturma

Herhangi bir dosyayı karşıya yükleyebilmeniz için önce bir blob kapsayıcısı gerekir.

1. Açmak için depolama hesabını seçin. Kaynak grubunda listelenen yalnızca bir depolama hesabı görürsünüz. Depolama hesabınızın adı, aşağıdaki ekran görüntüsünde gösterilenden farklı.

    ![Kaynak Yöneticisi öğreticisi depolama hesabı](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-storage-account.png)

2. **Bloblar** kutucuğunu seçin.

    ![Kaynak Yöneticisi öğretici blob 'ları](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-blobs.png)
3. Yeni bir kapsayıcı oluşturmak için yukarıdan **kapsayıcı** ' ı seçin.
4. Aşağıdaki değerleri girin:

    * **Ad**: **sqlbacpac**girin.
    * **Genel erişim düzeyi**: **özel (anonim erişim yok)** varsayılan değerini kullanın.
5. **Tamam**’ı seçin.
6. Yeni oluşturulan kapsayıcıyı açmak için **sqlbacpac** öğesini seçin.

### <a name="upload-the-bacpac-file-to-the-container"></a>BACPAC dosyasını kapsayıcıya yükleyin

1. **Karşıya Yükle**’yi seçin.
2. Aşağıdaki değerleri girin:

    * **Dosyalar**: daha önce indirdiğiniz bacpac dosyasını seçmek için yönergeleri takip edin. Varsayılan ad **Sqldatabaseextension. bacpac**' dir.
    * **Kimlik doğrulama türü**: **SAS**seçin.  *SAS* varsayılan değerdir.
3. **Karşıya Yükle**’yi seçin.  Dosya başarıyla karşıya yüklendikten sonra, dosya adı kapsayıcıda listelenecektir.

### <a name="a-namegenerate-a-sas-token-generate-a-sas-token"></a><a name="generate-a-sas-token" />Bir SAS belirteci oluştur

1. Kapsayıcıdan **Sqldatabaseextension. bacpac öğesine** sağ tıklayın ve ardından **SAS oluştur**' u seçin.
2. Aşağıdaki değerleri girin:

    * **İzin**: varsayılan, **Oku**kullanın.
    * **Başlangıç ve bitiş tarihi/saati**: varsayılan değer, SAS belirtecini kullanmak için size sekiz saat verir. Bu öğreticiyi tamamlaması için daha fazla zaman gerekiyorsa, **tarihi**Güncelleştir ' i güncelleştirin.
    * **Izin VERILEN IP adresleri**: bu alanı boş bırakın.
    * **Izin verilen protokoller**: varsayılan değeri kullanın: **https**.
    * **İmzalama anahtarı**: varsayılan değeri kullanın: **Key 1**.
3. **BLOB SAS belirteci oluştur ve URL 'yi**seçin.
4. **BLOB SAS URL**'sinin bir kopyasını oluşturun. URL 'nin ortasında, **Sqldatabaseextension. bacpac**adlı dosya adı bulunur.  Dosya adı, URL 'YI üç parçaya böler:

   - **Yapıt konumu**: https://xxxxxxxxxxxxxx.blob.core.windows.net/sqlbacpac/. Konumun bir "/" ile bitdiğinizden emin olun.
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

    Şablonda tanımlanmış beş kaynak vardır:

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

![Kaynak Yöneticisi öğreticisi güvenli yapıt parametreleri](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-secure-artifacts-parameters.png)

Aşağıdaki iki öğenin değerini güncelleştirin:

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

Bu öğreticide, bir SQL Server, SQL veritabanı dağıtmış ve SAS belirtecini kullanarak BACPAC dosyasını içeri aktardınız. Kaynak Yöneticisi şablonlarını sürekli olarak geliştirip dağıtmak için Azure işlem hattı oluşturmayı öğrenmek için bkz.

> [!div class="nextstepaction"]
> [Azure işlem hattı ile sürekli tümleştirme](./resource-manager-tutorial-use-azure-pipelines.md)
