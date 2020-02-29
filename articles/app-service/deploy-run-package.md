---
title: Uygulamanızı bir ZIP paketinden çalıştırma
description: Uygulamanın ZIP paketini Atomicity ile dağıtın. ZIP dağıtım sürecinde uygulamanızın davranışının öngörülebilirlik ve güvenilirliğini artırabilirsiniz.
ms.topic: article
ms.date: 01/14/2020
ms.openlocfilehash: 316ada7700a5cf45ee90f515336039702bab48c0
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77920731"
---
# <a name="run-your-app-in-azure-app-service-directly-from-a-zip-package"></a>Uygulamanızı doğrudan bir ZIP paketinden Azure App Service çalıştırma

[Azure App Service](overview.md), uygulamalarınızı doğrudan BIR dağıtım ZIP paketi dosyasından çalıştırabilirsiniz. Bu makalede, uygulamanızda bu işlevselliğin nasıl etkinleştirileceği gösterilmektedir.

App Service diğer tüm dağıtım yöntemlerinin ortak bir şeyi vardır: dosyalarınız uygulamanızda *D:\home\site\wwwroot* dosyasına dağıtılır (veya Linux uygulamaları için */Home/site/Wwwroot* ). Aynı dizin, çalışma zamanında uygulamanız tarafından kullanıldığından, dosya kilidi çakışmaları nedeniyle dağıtımın başarısız olması ve dosyaların bazıları henüz güncelleştirilmemiş olduğundan uygulamanın tahmin edilemeyecek şekilde davranması mümkündür.

Buna karşılık, doğrudan bir paketten çalıştırdığınızda Paketteki dosyalar *Wwwroot* dizinine kopyalanmaz. Bunun yerine, ZIP paketinin kendisi doğrudan salt okuma *Wwwroot* dizini olarak bağlanır. Doğrudan bir paketten çalıştırmanın çeşitli avantajları vardır:

- Dağıtım ve çalışma zamanı arasındaki dosya kilidi çakışmalarını ortadan kaldırır.
- Dilediğiniz zaman yalnızca tam olarak dağıtılan uygulamaların çalıştırılmasını sağlar.
- , Bir üretim uygulamasına dağıtılabilir (yeniden başlatma ile).
- Azure Resource Manager dağıtımlarının performansını geliştirir.
- Özellikle büyük NPM paket ağaçları olan JavaScript işlevleri için soğuk başlangıç zamanlarını azaltabilir.

> [!NOTE]
> Şu anda yalnızca ZIP paketi dosyaları desteklenir.

[!INCLUDE [Create a project ZIP file](../../includes/app-service-web-deploy-zip-prepare.md)]

## <a name="enable-running-from-package"></a>Paketten çalışmayı etkinleştir

`WEBSITE_RUN_FROM_PACKAGE` uygulama ayarı bir paketten çalışmaya izin vermez. Bunu ayarlamak için, Azure CLı ile aşağıdaki komutu çalıştırın.

```azurecli-interactive
az webapp config appsettings set --resource-group <group-name> --name <app-name> --settings WEBSITE_RUN_FROM_PACKAGE="1"
```

`WEBSITE_RUN_FROM_PACKAGE="1"`, uygulamanızı yerel bir paketten uygulamanıza çalıştırmanızı sağlar. Ayrıca, [uzak bir paketten da çalıştırabilirsiniz](#run-from-external-url-instead).

## <a name="run-the-package"></a>Paketi çalıştırın

App Service bir paketi çalıştırmanın en kolay yolu Azure CLı [az WebApp Deployment Source config-ZIP](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-zip) komutıdır. Örnek:

```azurecli-interactive
az webapp deployment source config-zip --resource-group <group-name> --name <app-name> --src <filename>.zip
```

`WEBSITE_RUN_FROM_PACKAGE` uygulama ayarı ayarlandığından, bu komut paket içeriğini uygulamanızın *D:\home\site\wwwroot* dizinine ayıklamaz. Bunun yerine, ZIP dosyasını *D:\home\data\sitepackages*dizinine yükler ve aynı dizinde, çalışma ZAMANıNDA yüklenecek ZIP paketinin adını içeren bir *PackageName. txt* dosyası oluşturur. ZIP paketinizi farklı bir şekilde (örneğin, [FTP](deploy-ftp.md)) yüklerseniz, *D:\home\data\sitepackages* dizinini ve *PackageName. txt* dosyasını el ile oluşturmanız gerekir.

Komut ayrıca uygulamayı yeniden başlatır. `WEBSITE_RUN_FROM_PACKAGE` ayarlandığı için, App Service karşıya yüklenen *paketi salt okunurdur* ve uygulamayı doğrudan bu bağlı dizinden çalıştırır.

## <a name="run-from-external-url-instead"></a>Bunun yerine dış URL 'den Çalıştır

Ayrıca, Azure Blob depolama gibi bir dış URL 'den paket de çalıştırabilirsiniz. Paket dosyalarını BLOB depolama hesabınıza yüklemek için [Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md) kullanabilirsiniz. App Service çalışma zamanının pakete güvenli bir şekilde erişmesini sağlamak için, [paylaşılan erişim imzası (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#generate-a-sas-in-storage-explorer) ile özel bir depolama kapsayıcısı kullanmanız gerekir. 

Dosyanızı blob depolamaya yükledikten ve dosya için bir SAS URL 'sine sahip olduktan sonra `WEBSITE_RUN_FROM_PACKAGE` uygulama ayarını URL olarak ayarlayın. Aşağıdaki örnek, Azure CLı kullanarak bunu yapar:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_RUN_FROM_PACKAGE="https://myblobstorage.blob.core.windows.net/content/SampleCoreMVCApp.zip?st=2018-02-13T09%3A48%3A00Z&se=2044-06-14T09%3A48%3A00Z&sp=rl&sv=2017-04-17&sr=b&sig=bNrVrEFzRHQB17GFJ7boEanetyJ9DGwBSV8OM3Mdh%2FM%3D"
```

Aynı ada sahip güncelleştirilmiş bir paketi BLOB depolama alanına yayımlarsanız, güncelleştirilmiş paketin App Service ' ye yüklenmesi için uygulamanızı yeniden başlatmanız gerekir.

### <a name="use-key-vault-references"></a>Key Vault başvurularını kullan

Ek güvenlik için, dış URL 'niz ile birlikte Key Vault başvurularını kullanabilirsiniz. Bu, URL 'YI Rest 'te şifreli tutar ve gizli yönetim ve döndürme için Key Vault faydalanmasına olanak tanır. İlişkili SAS anahtarını kolayca döndürebilmeniz için Azure Blob Storage kullanılması önerilir. Azure Blob depolama, bekleyen bir şekilde şifrelenir ve bu, App Service üzerinde dağıtıldığında Uygulama verilerinizi güvende tutar.

1. Azure Key Vault oluşturun.

    ```azurecli
    az keyvault create --name "Contoso-Vault" --resource-group <group-name> --location eastus
    ```

1. Dış URL 'nizi Key Vault bir gizli dizi olarak ekleyin.

    ```azurecli
    az keyvault secret set --vault-name "Contoso-Vault" --name "external-url" --value "<insert-your-URL>"
    ```

1. `WEBSITE_RUN_FROM_PACKAGE` uygulama ayarı oluşturun ve değeri dış URL 'ye Key Vault başvuru olarak ayarlayın.

    ```azurecli
    az webapp config appsettings set --settings WEBSITE_RUN_FROM_PACKAGE="@Microsoft.KeyVault(SecretUri=https://Contoso-Vault.vault.azure.net/secrets/external-url/<secret-version>"
    ```

Daha fazla bilgi için aşağıdaki makalelere bakın.

- [App Service için Key Vault başvurular](app-service-key-vault-references.md)
- [Bekleyen veriler için Azure depolama şifrelemesi](../storage/common/storage-service-encryption.md)

## <a name="troubleshooting"></a>Sorun giderme

- Doğrudan bir paketten çalıştırmak `wwwroot` salt okunurdur. Uygulamanız, bu dizine dosya yazmayı denediğinde bir hata alır.
- TAR ve GZIP biçimleri desteklenmez.
- Bu özellik [Yerel önbellek](overview-local-cache.md)ile uyumlu değil.
- İyileştirilmiş soğuk başlangıç performansı için yerel posta seçeneğini kullanın (`WEBSITE_RUN_FROM_PACKAGE`= 1).

## <a name="more-resources"></a>Diğer kaynaklar

- [Azure App Service için sürekli dağıtım](deploy-continuous-deployment.md)
- [Kodu ZIP veya WAR dosyası ile dağıtma](deploy-zip.md)
