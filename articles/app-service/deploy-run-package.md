---
title: Uygulamanızı bir ZIP paketinden çalıştırın
description: Uygulamanızın ZIP paketini atomik olarak dağıtın. ZIP dağıtım işlemi sırasında uygulamanızın davranışının öngörülebilirliğini ve güvenilirliğini artırın.
ms.topic: article
ms.date: 01/14/2020
ms.openlocfilehash: 5cc909d79b3f5ea2b4c6a3da12bc7250addbe00c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77920731"
---
# <a name="run-your-app-in-azure-app-service-directly-from-a-zip-package"></a>Uygulamanızı Azure Uygulama Hizmeti'nde doğrudan bir ZIP paketinden çalıştırın

[Azure Uygulama Hizmeti'nde](overview.md)uygulamalarınızı doğrudan dağıtım ZIP paketi dosyasından çalıştırabilirsiniz. Bu makalede, bu işlevselliği uygulamanızda nasıl etkinleştirilen gösterilmektedir.

Uygulama Hizmeti'ndeki diğer tüm dağıtım yöntemlerinin ortak bir noktası vardır: dosyalarınız uygulamanızdaki *D:\home\site\wwwroot'a* (veya Linux uygulamaları için */home/site/wwwroot)* dağıtılır. Aynı dizin çalışma zamanında uygulamanız tarafından kullanıldığından, dosya kilidi çakışmaları nedeniyle dağıtımın başarısız olması ve bazı dosyalar henüz güncelleştirilmediği için uygulamanın öngörülemeyen şekilde çalışması mümkündür.

Bunun aksine, doğrudan bir paketten çalıştırdığınızda, paketteki dosyalar *wwwroot* dizinine kopyalanmaz. Bunun yerine, ZIP paketinin kendisi doğrudan salt okunur *wwwroot* dizini olarak monte olur. Doğrudan bir paketten çalıştırmanın çeşitli avantajları vardır:

- Dağıtım ve çalışma zamanı arasındaki dosya kilidi çakışmalarını ortadan kaldırır.
- Yalnızca tam olarak dağıtılan uygulamaların her anda çalışmasını sağlar.
- Bir üretim uygulamasına dağıtılabilir (yeniden başlatılır).
- Azure Kaynak Yöneticisi dağıtımlarının performansını artırır.
- Özellikle büyük npm paket ağaçlarıyla JavaScript işlevleri için soğuk başlangıç sürelerini azaltabilir.

> [!NOTE]
> Şu anda yalnızca ZIP paket dosyaları desteklenir.

[!INCLUDE [Create a project ZIP file](../../includes/app-service-web-deploy-zip-prepare.md)]

## <a name="enable-running-from-package"></a>Paketten çalıştırmayı etkinleştirme

Uygulama `WEBSITE_RUN_FROM_PACKAGE` ayarı bir paketten çalıştırmayı sağlar. Ayarlamak için aşağıdaki komutu Azure CLI ile çalıştırın.

```azurecli-interactive
az webapp config appsettings set --resource-group <group-name> --name <app-name> --settings WEBSITE_RUN_FROM_PACKAGE="1"
```

`WEBSITE_RUN_FROM_PACKAGE="1"`uygulamanızı yerel bir paketten uygulamanıza çalıştırmanızı sağlar. [Ayrıca uzak bir paketten çalıştırabilirsiniz.](#run-from-external-url-instead)

## <a name="run-the-package"></a>Paketi çalıştırma

Uygulama Hizmetinizde bir paketi çalıştırmanın en kolay yolu Azure CLI [az webapp dağıtım kaynağı config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-zip) komutudur. Örnek:

```azurecli-interactive
az webapp deployment source config-zip --resource-group <group-name> --name <app-name> --src <filename>.zip
```

`WEBSITE_RUN_FROM_PACKAGE` Uygulama ayarı ayarlandığı için, bu komut paket içeriğini uygulamanızın *D:\home\site\wwwroot* dizinine çıkarmaz. Bunun yerine, ZIP dosyasını olduğu gibi *D:\home\data\SitePackages'e*yükler ve çalışma zamanında yüklenmesi gereken ZIP paketinin adını içeren aynı dizinde bir *packagename.txt* oluşturur. ZIP paketinizi farklı bir şekilde yüklerseniz [(FTP](deploy-ftp.md)gibi), *D:\home\data\SitePaketleri* dizinini ve *packagename.txt* dosyasını el ile oluşturmanız gerekir.

Komut ayrıca uygulamayı yeniden başlatır. `WEBSITE_RUN_FROM_PACKAGE` Ayarlandığı için, App Service yüklenen paketi salt okunur *wwwroot* dizini olarak bağlar ve uygulamayı doğrudan bu monte edilen dizinden çalıştırın.

## <a name="run-from-external-url-instead"></a>Bunun yerine harici URL'den çalıştırın

Paketi Azure Blob Depolama gibi harici bir URL'den de çalıştırabilirsiniz. Paket dosyalarını Blob depolama hesabınıza yüklemek için [Azure Depolama Gezgini'ni](../vs-azure-tools-storage-manage-with-storage-explorer.md) kullanabilirsiniz. Uygulama Hizmeti'nin pakete güvenli bir şekilde erişmesini sağlamak için [Paylaşılan Erişim İmzası (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#generate-a-sas-in-storage-explorer) içeren özel bir depolama kapsayıcısı kullanmanız gerekir. 

Dosyanızı Blob depolama alanına yükledikten ve dosya için bir `WEBSITE_RUN_FROM_PACKAGE` SAS URL'sine sahip olduktan sonra, uygulama ayarını URL'ye ayarlayın. Aşağıdaki örnekte, Azure CLI'yi kullanarak bunu yapar:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_RUN_FROM_PACKAGE="https://myblobstorage.blob.core.windows.net/content/SampleCoreMVCApp.zip?st=2018-02-13T09%3A48%3A00Z&se=2044-06-14T09%3A48%3A00Z&sp=rl&sv=2017-04-17&sr=b&sig=bNrVrEFzRHQB17GFJ7boEanetyJ9DGwBSV8OM3Mdh%2FM%3D"
```

Blob depolama alanına aynı ada sahip güncelleştirilmiş bir paket yayımlarsanız, güncelleştirilmiş paketin App Service'e yüklenmesi için uygulamanızı yeniden başlatmanız gerekir.

## <a name="troubleshooting"></a>Sorun giderme

- Doğrudan bir paketten `wwwroot` çalıştırmak salt okunur yapar. Bu dizine dosya yazmaya çalışırsa uygulamanız bir hata alır.
- TAR ve GZIP biçimleri desteklenmez.
- Bu özellik yerel [önbellekle](overview-local-cache.md)uyumlu değildir.
- Daha iyi soğuk başlangıç performansı için yerel`WEBSITE_RUN_FROM_PACKAGE`Zip seçeneğini (=1) kullanın.

## <a name="more-resources"></a>Diğer kaynaklar

- [Azure Uygulama Hizmeti için sürekli dağıtım](deploy-continuous-deployment.md)
- [ZIP veya WAR dosyasıyla kod dağıtma](deploy-zip.md)
