---
title: Öğretici - özel eylemler & kaynakları
description: Bu öğretici, Azure Özel Sağlayıcısı ile Azure Yönetilen Uygulama'nın nasıl oluşturulacağÝný açıklar.
ms.topic: tutorial
ms.author: lazinnat
author: lazinnat
ms.date: 06/20/2019
ms.openlocfilehash: c3750da6bd76c8cb3908fbdc71ba676f09d77def
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75650082"
---
# <a name="tutorial-create-managed-application-with-custom-actions-and-resources"></a>Öğretici: Özel eylemler ve kaynaklarla yönetilen uygulama oluşturma

Bu öğreticide, özel eylemler ve kaynaklarla kendi yönetilen uygulamanızı oluşturursunuz. Yönetilen `Overview` uygulama, sayfada özel bir eylem, ayrı bir menü öğesi olarak `Table of Content` görüntülenen özel bir kaynak türü ve özel kaynak sayfasında özel bir bağlam eylemi içerir.

Bu öğretici aşağıdaki adımları içerir:

> [!div class="checklist"]
> * Yönetilen bir uygulama örneği oluşturmak için yazar kullanıcı arabirimi tanım dosyası
> * Azure Özel [Sağlayıcısı, Azure](../custom-providers/overview.md)Depolama Hesabı ve Azure İşlevi ile yazar dağıtım şablonu
> * Özel eylemler ve kaynaklarla yazar görünümü tanımı artifakı
> * Yönetilen bir uygulama tanımı dağıtma
> * Yönetilen uygulamanın bir örneğini dağıtma
> * Özel eylemler gerçekleştirin ve özel kaynaklar oluşturun

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için şunları bilmeniz gerekir:

* Yönetilen [bir uygulama tanımı oluşturma ve yayımlama.](publish-service-catalog-app.md)
* Azure [portalı üzerinden Hizmet Kataloğu uygulaması](deploy-service-catalog-quickstart.md)nasıl dağıtılır?
* Yönetilen [uygulamanız için Azure portalı kullanıcı arabirimi](create-uidefinition-overview.md)nasıl oluşturulur?
* [Tanım yapı özellikleri](concepts-view-definition.md) özelliklerini görüntüleyin.
* [Azure Özel Sağlayıcı](../custom-providers/overview.md) özellikleri.

## <a name="user-interface-definition"></a>Kullanıcı arabirimi tanımı

Bu öğreticide, yönetilen bir uygulama oluşturursunuz ve yönetilen kaynak grubu özel sağlayıcı örneği, depolama hesabı ve işlev içerir. Bu örnekte kullanılan Azure İşlevi, eylemler ve kaynaklar için özel sağlayıcı işlemlerini işleyen bir API uygular. Azure Depolama Hesabı, özel sağlayıcı kaynaklarınız için temel depolama alanı olarak kullanılır.

Yönetilen bir uygulama örneği oluşturmak için `funcname` `storagename` kullanıcı arabirimi tanımı içerir ve giriş öğeleri. Depolama hesabı adı ve işlev adı genel olarak benzersiz olmalıdır. Varsayılan fonksiyon dosyaları [örnek işlev paketinden](https://github.com/Azure/azure-quickstart-templates/tree/master/101-custom-rp-with-function/artifacts/functionzip)dağıtılır, ancak *createUIDefinition.json*bir paket bağlantısı için bir giriş öğesi ekleyerek değiştirebilirsiniz:

```json
{
  "name": "funcname",
  "type": "Microsoft.Common.TextBox",
  "label": "Name of the function to be created",
  "toolTip": "Name of the function to be created",
  "visible": true,
  "constraints": {
    "required": true
  }
},
{
  "name": "storagename",
  "type": "Microsoft.Common.TextBox",
  "label": "Name of the storage to be created",
  "toolTip": "Name of the storage to be created",
  "visible": true,
  "constraints": {
    "required": true
  }
},
{
  "name": "zipFileBlobUri",
  "type": "Microsoft.Common.TextBox",
  "defaultValue": "https://github.com/Azure/azure-quickstart-templates/tree/master/101-custom-rp-with-function/artifacts/functionzip/functionpackage.zip",
  "label": "The Uri to the uploaded function zip file",
  "toolTip": "The Uri to the uploaded function zip file",
  "visible": true
}
```

ve *createUIDefinition.json*çıktı :

```json
  "funcname": "[steps('applicationSettings').funcname]",
  "storageName": "[steps('applicationSettings').storagename]",
  "zipFileBlobUri": "[steps('applicationSettings').zipFileBlobUri]"
```

Tam *createUIDefinition.json* örnek Referans [bulunabilir: Kullanıcı arabirimi elemanları eserler](reference-createuidefinition-artifact.md).

## <a name="template-with-custom-provider"></a>Özel sağlayıcıile şablon

Özel sağlayıcıyla yönetilen bir uygulama örneği oluşturmak için, ad **genelolan** özel sağlayıcı kaynağını tanımlamanız ve **mainTemplate.json'unuzda** **Microsoft.CustomProviders/resourceProviders** yazın. Bu kaynakta, hizmetinizin kaynak türlerini ve eylemlerini tanımlarsınız. Azure İşlevi ve Azure Depolama Hesabı örneklerini `Microsoft.Storage/storageAccounts` dağıtmak için, sırasıyla tür `Microsoft.Web/sites` kaynaklarını tanımlar.

`users` Bu öğreticide, `ping` `users/contextAction` `users` özel bir kaynak bağlamında gerçekleştirilecek bir kaynak türü, özel eylem ve özel eylem oluşturursunuz. Her kaynak türü ve eylem için [createUIDefinition.json](#user-interface-definition)sağlanan adı ile işlevi gösteren bir bitiş noktası sağlar. Kaynak türleri ve `Proxy` eylemler `Proxy,Cache` için **yönlendirme Türünü** belirtin:

```json
{
  "apiVersion": "[variables('customrpApiversion')]",
  "type": "Microsoft.CustomProviders/resourceProviders",
  "name": "[variables('customProviderName')]",
  "location": "[parameters('location')]",
  "properties": {
    "actions": [
      {
        "name": "ping",
        "routingType": "Proxy",
        "endpoint": "[listSecrets(resourceId('Microsoft.Web/sites/functions', parameters('funcname'), 'HttpTrigger1'), '2018-02-01').trigger_url]"
      },
      {
        "name": "users/contextAction",
        "routingType": "Proxy",
        "endpoint": "[listSecrets(resourceId('Microsoft.Web/sites/functions', parameters('funcname'), 'HttpTrigger1'), '2018-02-01').trigger_url]"
      }
    ],
    "resourceTypes": [
      {
        "name": "users",
        "routingType": "Proxy,Cache",
        "endpoint": "[listSecrets(resourceId('Microsoft.Web/sites/functions', parameters('funcname'), 'HttpTrigger1'), '2018-02-01').trigger_url]"
      }
    ]
  },
  "dependsOn": [
    "[concat('Microsoft.Web/sites/',parameters('funcname'))]"
  ]
}
```

*MainTemplate.json* örneğinin tamamı [Başvuru: Dağıtım şablonu artifakı'nda](reference-main-template-artifact.md)bulunabilir.

## <a name="view-definition-artifact"></a>Tanım yapıtını görüntüleme

Yönetilen uygulamanızda özel eylemler ve özel kaynaklar içeren kullanıcı arabirimi tanımlamak için **viewDefinition.json** artifakı yazmanız gerekir. Görünüm tanımı yapı hakkında daha fazla bilgi için Azure [Yönetilen Uygulamalar'da tanım artifakı](concepts-view-definition.md)görüntüle'ye bakın.

Bu öğreticide şunları tanımlarsınız:
* Temel *Overview* metin girişiyle özel bir eylemi `TestAction` temsil eden araç çubuğu düğmesi içeren genel bakış sayfası.
* Özel *Users* bir kaynak türünü `users`temsil eden kullanıcılar sayfası.
* Kullanıcılar sayfasında, `users/contextAction` *Users* özel bir tür `users`kaynak bağlamında gerçekleştirilecek özel bir kaynak eylemi.

Aşağıdaki örnekte, bir "Genel Bakış" sayfası için görünüm yapılandırması gösterilmektedir:

```json
{
    "kind": "Overview",
    "properties": {
      "header": "Welcome to your Demo Azure Managed Application",
      "description": "This Managed application with Custom Provider is for demo purposes only.",
      "commands": [{
          "displayName": "Ping Action",
          "path": "/customping",
          "icon": "LaunchCurrent"
      }]
    }
  }
```

Aşağıdaki örnek, özel kaynak eylemi ile "Kullanıcılar" kaynakları sayfa yapılandırmasını içerir:

```json
{
    "kind": "CustomResources",
    "properties": {
      "displayName": "Users",
      "version": "1.0.0.0",
      "resourceType": "users",
      "createUIDefinition": {
      },
      "commands": [{
        "displayName": "Custom Context Action",
        "path": "users/contextAction",
        "icon": "Start"
      }],
      "columns": [
        { "key": "properties.FullName", "displayName": "Full Name" },
        { "key": "properties.Location", "displayName": "Location", "optional": true }
      ]
    }
  }
```

Tam *viewDefinition.json* örneği [Reference: View definition artifakı](reference-view-definition-artifact.md)bulunabilir.

## <a name="managed-application-definition"></a>Yönetilen uygulama tanımı

Aşağıdaki yönetilen uygulama yapılarını zip arşivine paketleyin ve depolamaalanına yükleyin:

* oluşturmaUiDefinition.json
* mainTemplate.json
* viewDefinition.json

Tüm dosyalar kök düzeyinde olmalıdır. Yapıların bulunduğu paket, GitHub blob veya Azure Depolama Hesabı blob'u gibi herhangi bir depolama alanında depolanabilir. Uygulama paketini depolama hesabına yüklemek için bir komut dosyası aşağıda veda edebilirsiniz: 

```powershell
$resourceGroup="appResourcesGroup"
$storageName="mystorageaccount$RANDOM"

# Sign in to your Azure subscription
Connect-AzAccount
# Create resource group for managed application definition and application package
New-AzResourceGroup -Name $resourceGroup -Location eastus

# Create storage account for a package with application artifacts
$storageAccount=New-AzStorageAccount `
  -ResourceGroupName $resourceGroup `
  -Name $storageName `
  -SkuName Standard_LRS `
  -Location eastus `
$ctx=$storageAccount.Context

# Create storage container and upload zip to blob
New-AzStorageContainer -Name appcontainer -Context $ctx -Permission blob
Set-AzStorageBlobContent `
  -File "path_to_your_zip_package" `
  -Container appcontainer `
  -Blob app.zip `
  -Context $ctx 

# Get blob absolute uri
$blobUri=(Get-AzureStorageBlob -Container appcontainer -Blob app.zip -Context $ctx).ICloudBlob.uri.AbsoluteUri
```

Aşağıda Azure CLI komut dosyasını çalıştırın veya Hizmet Kataloğu yönetilen uygulama tanımını dağıtmak için Azure portalındaki adımları izleyin:

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

# <a name="azure-cli"></a>[Azure CLI](#tab/azurecli-interactive)

```azurecli-interactive
resourceGroup="appResourcesGroup"
# Select subscription and create resource group (if you have not created yet)
az account set --subscription <subscriptionID>
az group create --name $resourceGroup --location eastus

# Get object ID of your identity
userid=$(az ad user show --upn-or-object-id example@contoso.org --query objectId --output tsv)
# Get role definition ID for the Owner role
roleid=$(az role definition list --name Owner --query [].name --output tsv)

# Create managed application definition resource
az managedapp definition create \
  --name "ManagedUsersAppDefinition" \
  --location "eastus" \
  --resource-group $resourceGroup \
  --lock-level ReadOnly \
  --display-name "Managed users app definition" \
  --description "Managed application with Azure Custom Provider" \
  --authorizations "$userid:$roleid" \
  --package-file-uri "path to your app.zip package"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Azure portalında **Tüm hizmetler'i**seçin. Kaynaklar listesinde Yönetilen **Uygulamalar Merkezi**yazın ve seçin.
2. Yönetilen **Uygulamalar**Merkezi'nde, **Hizmet Kataloğu uygulama tanımını** seçin ve **Ekle'yi**tıklatın. 
    
    ![Hizmet Kataloğu Ekle](./media/tutorial-create-managed-app-with-custom-provider/service-catalog-managed-application.png)

3. Hizmet Kataloğu tanımı oluşturmak için değerler sağlayın:

    * Hizmet Kataloğu tanımı, **Görüntü Adı** ve *Açıklaması*(isteğe bağlı) için benzersiz bir **Ad** sağlayın.
    * **Abonelik,** **Kaynak grubu**ve uygulama tanımının oluşturulacağı **konum'u** seçin. Zip paketi için kullanılan aynı kaynak grubunu kullanabilir veya yeni bir kaynak grubu oluşturabilirsiniz.
    * Paket **Dosya Uri**için, önceki adımda oluşturduğunuz zip dosyasına giden yolu sağlayın.

    ![Değerler sağlama](./media/tutorial-create-managed-app-with-custom-provider/add-service-catalog-managed-application.png)

4. Kimlik Doğrulama ve Kilit Düzeyi bölümüne ulaştığınızda, **Yetki Ekle'yi**seçin.

    ![Yetkilendirme ekleme](./media/tutorial-create-managed-app-with-custom-provider/add-authorization.png)

5. Kaynakları yönetmek için bir Azure Etkin Dizin grubu seçin ve **Tamam'ı**seçin.

   ![Yetkilendirme grubu ekleme](./media/tutorial-create-managed-app-with-custom-provider/add-auth-group.png)

6. Tüm değerleri sağladığınızda **Oluştur'u**seçin.

   ![Yönetilen uygulama tanımı oluşturma](./media/tutorial-create-managed-app-with-custom-provider/create-service-catalog-definition.png)

---

## <a name="managed-application-instance"></a>Yönetilen uygulama örneği

Yönetilen uygulama tanımı dağıtıldığında, yönetilen uygulama örneğini özel sağlayıcıyla dağıtmak için aşağıdaki komut dosyasını çalıştırın veya Azure portalındaki adımları izleyin:

# <a name="azure-cli"></a>[Azure CLI](#tab/azurecli-interactive)

```azurecli-interactive
appResourcesGroup="appResourcesGroup"
applicationGroup="usersApplicationGroup"

# Create resource group for managed application instance
az group create --name $applicationGroup --location eastus

# Get ID of managed application definition
appid=$(az managedapp definition show --name ManagedUsersAppDefinition --resource-group $appResourcesGroup --query id --output tsv)

# Create the managed application
az managedapp create \
  --name ManagedUsersApp \
  --location "eastus" \
  --kind "Servicecatalog" \
  --resource-group $applicationGroup \
  --managedapp-definition-id $appid \
  --managed-rg-id "managedResourcesGroup" \
  --parameters "{\"funcname\": {\"value\": \"managedusersappfunction\"}, \"storageName\": {\"value\": \"managedusersappstorage\"}}"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Azure portalında **Tüm hizmetler'i**seçin. Kaynaklar listesinde Yönetilen **Uygulamalar Merkezi**yazın ve seçin.
2. Yönetilen **Uygulamalar**Merkezi'nde, **Hizmet Kataloğu uygulamalarını** seçin ve **Ekle'yi**tıklatın. 

    ![Yönetilen uygulama ekleme](./media/tutorial-create-managed-app-with-custom-provider/add-managed-application.png)

3. Hizmet **Kataloğu uygulamalarında** sayfa türü Hizmet Kataloğu tanım arama kutusunda görüntü adı. Önceki adımda oluşturulan tanımı seçin ve **Oluştur'u**tıklatın.

    ![Hizmet kataloğu seçme](./media/tutorial-create-managed-app-with-custom-provider/select-service-catalog-definition.png)

4. Hizmet Kataloğu tanımından yönetilen bir uygulama örneği oluşturmak için değerler sağlayın:

    * **Abonelik,** **Kaynak grubu**ve uygulama örneğinin oluşturulacağı **konum'u** seçin.
    * Benzersiz bir Azure İşlevi adı ve Azure Depolama Hesabı adı sağlayın.

    ![Uygulama ayarları](./media/tutorial-create-managed-app-with-custom-provider/application-settings.png)

5. Doğrulama geçtiğinde, yönetilen bir uygulamaörneğini dağıtmak için **Tamam'ı** tıklatın. 
    
    ![Yönetilen uygulamayı dağıtma](./media/tutorial-create-managed-app-with-custom-provider/deploy-managed-application.png)

---

## <a name="custom-actions-and-resources"></a>Özel eylemler ve kaynaklar

Hizmet kataloğu uygulama örneği dağıtıldıktan sonra iki yeni kaynak grubunuz vardır. İlk kaynak `applicationGroup` grubu yönetilen uygulamanın bir örneğini içerir, ikinci kaynak grubu `managedResourceGroup` **özel sağlayıcı**da dahil olmak üzere yönetilen uygulama için kaynakları tutar.

![Uygulama kaynak grupları](./media/tutorial-create-managed-app-with-custom-provider/application-resource-groups.png)

Yönetilen uygulama örneğine gidebilir ve "Genel Bakış" sayfasında **özel eylem** gerçekleştirebilir, "Kullanıcılar" sayfasında **kullanıcılara** özel kaynak oluşturabilir ve özel kaynak üzerinde **özel bağlam eylemi** çalıştırabilirsiniz.

* "Genel Bakış" sayfasına gidin ve "Ping Eylemi" düğmesine tıklayın:

![Özel eylem gerçekleştirin](./media/tutorial-create-managed-app-with-custom-provider/perform-custom-action.png)

* "Kullanıcılar" sayfasına gidin ve "Ekle" düğmesine tıklayın. Kaynak oluşturmak için girişler sağlayın ve formu gönderin:

![Özel kaynak oluşturma](./media/tutorial-create-managed-app-with-custom-provider/create-custom-resource.png)

* "Kullanıcılar" sayfasına gidin, bir "kullanıcılar" kaynağı seçin ve "Özel Bağlam Eylemi"ni tıklayın:

![Özel kaynak oluşturma](./media/tutorial-create-managed-app-with-custom-provider/perform-custom-resource-action.png)

[!INCLUDE [clean-up-section-portal](../../../includes/clean-up-section-portal.md)]

## <a name="looking-for-help"></a>Yardım arıyorum

Azure Yönetilen Uygulamalar hakkında sorularınız varsa, [Yığın Taşma'da](https://stackoverflow.com/questions/tagged/azure-managedapps)soru sormayı deneyin. Benzer bir soru zaten sorulmuş ve yanıtlanmış olabilir, bu nedenle göndermeden önce ilk kontrol edin. Hızlı yanıt `azure-managedapps` almak için etiketi ekleyin!

## <a name="next-steps"></a>Sonraki adımlar

Yönetilen uygulamanızı Azure Market'te yayımlamak için bkz. [Market'teki Azure tarafından yönetilen uygulamalar](publish-marketplace-app.md).

[Azure Özel Sağlayıcıları](../custom-providers/overview.md)hakkında daha fazla bilgi edinin.
