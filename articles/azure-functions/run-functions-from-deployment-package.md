---
title: Azure Işlevlerinizi bir paketten çalıştırın
description: Azure Işlevleri çalışma zamanının, işlev uygulaması proje dosyalarınızı içeren bir dağıtım paketi dosyası bağlayarak işlevlerinizi çalıştırmasını sağlayabilirsiniz.
ms.topic: conceptual
ms.date: 07/15/2019
ms.openlocfilehash: a3e11a7c4f3fd91df2fd9dd7a44f3922c4922585
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77921122"
---
# <a name="run-your-azure-functions-from-a-package-file"></a>Azure Işlevlerinizi bir paket dosyasından çalıştırın

Azure 'da işlevlerinizi doğrudan işlev uygulamanızdaki bir dağıtım paketi dosyasından çalıştırabilirsiniz. Diğer seçenek, dosyalarınızı işlev uygulamanızın `d:\home\site\wwwroot` dizininde dağıtmaktır.

Bu makalede, işlevlerinizi bir paketten çalıştırmanın avantajları açıklanmaktadır. Ayrıca, işlev uygulamanızda bu işlevselliği nasıl etkinleştireceğinizi gösterir.

> [!IMPORTANT]
> İşlevlerinizi [Premium bir planda](functions-scale.md#premium-plan)bir Linux işlev uygulamasına dağıttığınızda, her zaman paket dosyasından çalıştırmanız ve [Azure Functions Core Tools kullanarak uygulamanızı yayımlamanız](functions-run-local.md#project-file-deployment)gerekir.

## <a name="benefits-of-running-from-a-package-file"></a>Bir paket dosyasından çalıştırmanın avantajları
  
Bir paket dosyasından çalıştırmanın çeşitli avantajları vardır:

+ Dosya kopyalama kilitleme sorunları riskini azaltır.
+ , Bir üretim uygulamasına dağıtılabilir (yeniden başlatma ile).
+ Uygulamanızda çalışmakta olan dosyalardan emin olabilirsiniz.
+ [Azure Resource Manager dağıtımlarının](functions-infrastructure-as-code.md)performansını geliştirir.
+ Özellikle büyük NPM paket ağaçları olan JavaScript işlevleri için soğuk başlangıç zamanlarını azaltabilir.

Daha fazla bilgi için [bu duyuruya](https://github.com/Azure/app-service-announcements/issues/84)bakın.

## <a name="enabling-functions-to-run-from-a-package"></a>İşlevleri bir paketten çalıştırmak için etkinleştirme

İşlev uygulamanızın bir paketten çalıştırılmasını sağlamak için, işlev uygulaması ayarlarınıza yalnızca bir `WEBSITE_RUN_FROM_PACKAGE` ayarı eklersiniz. `WEBSITE_RUN_FROM_PACKAGE` ayarı aşağıdaki değerlerden birine sahip olabilir:

| Değer  | Açıklama  |
|---------|---------|
| **`1`**  | Windows üzerinde çalışan işlev uygulamaları için önerilir. İşlev uygulamanızın `d:\home\data\SitePackages` klasöründeki bir paket dosyasından çalıştırın. [ZIP dağıtımı ile dağıtılmadığından](#integration-with-zip-deployment), bu seçenek klasörün Ayrıca `packagename.txt`adlı bir dosyaya sahip olmasını gerektirir. Bu dosya, bir boşluk olmadan yalnızca klasördeki paket dosyasının adını içerir. |
|**`<URL>`**  | Çalıştırmak istediğiniz belirli bir paket dosyasının konumu. Blob depolamayı kullanırken, Işlevlerin çalışma zamanının pakete erişmesini sağlamak için [paylaşılan erişim imzası (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#generate-a-sas-in-storage-explorer) olan bir özel kapsayıcı kullanmanız gerekir. Paket dosyalarını BLOB depolama hesabınıza yüklemek için [Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md) kullanabilirsiniz. Bir URL belirttiğinizde, güncelleştirilmiş bir paket yayımladıktan sonra [Tetikleyicileri de eşitlemeniz](functions-deployment-technologies.md#trigger-syncing) gerekir. |

> [!CAUTION]
> Windows üzerinde bir işlev uygulaması çalıştırırken, dış URL seçeneği daha kötü soğuk başlangıç performansı verir. İşlev uygulamanızı Windows 'a dağıttığınızda, `1` için `WEBSITE_RUN_FROM_PACKAGE` ayarlamanız ve ZIP dağıtımıyla yayımlamanız gerekir.

Aşağıda, Azure Blob depolamada barındırılan bir. zip dosyasından çalışacak şekilde yapılandırılmış bir işlev uygulaması gösterilmektedir:

![WEBSITE_RUN_FROM_ZIP uygulama ayarı](./media/run-functions-from-deployment-package/run-from-zip-app-setting-portal.png)

> [!NOTE]
> Şu anda yalnızca. zip paketi dosyaları desteklenir.

## <a name="integration-with-zip-deployment"></a>ZIP dağıtımıyla tümleştirme

[ZIP dağıtımı][Zip deployment for Azure Functions] , işlev uygulaması projenizi `wwwroot` dizinine dağıtmanıza olanak sağlayan bir Azure App Service özelliğidir. Proje bir. zip dağıtım dosyası olarak paketlenmiştir. Paketinizin `d:\home\data\SitePackages` klasöre dağıtılması için aynı API 'Ler kullanılabilir. `1``WEBSITE_RUN_FROM_PACKAGE` uygulama ayarı değeri ile, ZIP dağıtım API 'Leri, paketi `d:\home\site\wwwroot`ayıklamak yerine `d:\home\data\SitePackages` klasörüne kopyalar. Ayrıca, `packagename.txt` dosyası da oluşturur. Yeniden başlatmadan sonra, paket salt okunurdur `wwwroot` bir dosya sistemi olarak bağlanır. ZIP dağıtımı hakkında daha fazla bilgi için bkz. [Azure işlevleri Için zip dağıtımı](deployment-zip-push.md).

## <a name="adding-the-website_run_from_package-setting"></a>WEBSITE_RUN_FROM_PACKAGE ayarı ekleniyor

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

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

- [App Service için Key Vault başvurular](../app-service/app-service-key-vault-references.md)
- [Bekleyen veriler için Azure depolama şifrelemesi](../storage/common/storage-service-encryption.md)

## <a name="troubleshooting"></a>Sorun giderme

- Paketten çalıştırma `wwwroot` salt okunurdur, bu nedenle dosyaları bu dizine yazarken bir hata alırsınız.
- Tar ve gzip biçimleri desteklenmez.
- Bu özellik yerel önbellek ile oluşturmaz.
- İyileştirilmiş soğuk başlangıç performansı için yerel posta seçeneğini kullanın (`WEBSITE_RUN_FROM_PACKAGE`= 1).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevleri için sürekli dağıtım](functions-continuous-deployment.md)

[Zip deployment for Azure Functions]: deployment-zip-push.md
