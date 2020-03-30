---
title: Uygulama kaynağınızı istirahatte şifreleme
description: Uygulama verilerinizi Azure Depolama'da şifreleyin ve paket dosyası olarak dağıtın.
ms.topic: article
ms.date: 03/06/2020
ms.openlocfilehash: 62179e900ace0d6d7b8b1f07e8f0ab685508f991
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79408733"
---
# <a name="encryption-at-rest-using-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları kullanarak istirahatte şifreleme

İşlev uygulamanızın uygulama verilerini başka bir yerde şifrelemek için bir Azure Depolama Hesabı ve Azure Anahtar Kasası gerekir. Bu hizmetler, uygulamanızı bir dağıtım paketinden çalıştırdığınızda kullanılır.

  - [Azure Depolama, şifrelemeyi istirahatte sağlar.](../storage/common/storage-service-encryption.md) Sistem tarafından sağlanan anahtarları veya müşteri tarafından yönetilen kendi anahtarlarınızı kullanabilirsiniz. Azure'daki bir işlev uygulamasında çalışmadığında uygulama verileriniz burada depolanır.
  - [Dağıtım paketinden çalıştırma] ((run-functions-from-deployment-package.md) Uygulama Hizmetinin bir dağıtım özelliğidir. Paylaşılan Erişim İmzası (SAS) URL'sini kullanarak sitenizin içeriğini Azure Depolama Hesabından dağıtmanızı sağlar.
  - [Anahtar Vault başvuruları,](../app-service/app-service-key-vault-references.md) App Service'in bir güvenlik özelliğidir. Bu uygulama ayarları olarak çalışma zamanında sırları almanızı sağlar. Azure Depolama Hesabınızın SAS URL'sini şifrelemek için bunu kullanın.

## <a name="set-up-encryption-at-rest"></a>Şifrelemeyi istirahatte ayarlama

### <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma

İlk olarak, [bir Azure Depolama hesabı oluşturun](../storage/common/storage-account-create.md) ve müşteri yönetilen [anahtarlarla şifreleyin.](../storage/common/encryption-customer-managed-keys.md) Depolama hesabı oluşturulduktan sonra paket dosyalarını yüklemek için [Azure Depolama Gezgini'ni](../vs-azure-tools-storage-manage-with-storage-explorer.md) kullanın.

Ardından, [Bir SAS oluşturmak](../vs-azure-tools-storage-manage-with-storage-explorer.md?tabs=windows#generate-a-sas-in-storage-explorer)için Depolama Gezgini'ni kullanın. 

> [!NOTE]
> Bu SAS URL'yi kaydedin, bu daha sonra dağıtım paketinin çalışma zamanında güvenli erişimini sağlamak için kullanılır.

### <a name="configure-running-from-a-package-from-your-storage-account"></a>Depolama hesabınızdan bir paketten çalıştırmayı yapılandırma
  
Dosyanızı Blob depolama alanına yükledikten ve dosya için bir `WEBSITE_RUN_FROM_PACKAGE` SAS URL'sine sahip olduktan sonra, uygulama ayarını SAS URL'sine ayarlayın. Aşağıdaki örnekte, Azure CLI'yi kullanarak bunu yapar:

```
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_RUN_FROM_PACKAGE="<your-SAS-URL>"
```

Bu uygulama ayarını eklemek işlev uygulamanızın yeniden başlatılmasına neden olur. Uygulama yeniden başlatıldıktan sonra, uygulamanın dağıtım paketini doğru şekilde kullanmaya başladığından emin olun. Uygulama doğru başlamadıysa, paket [sorun giderme kılavuzundan Çalıştır'a](run-functions-from-deployment-package.md#troubleshooting)bakın.

### <a name="encrypt-the-application-setting-using-key-vault-references"></a>Anahtar Kasa başvurularını kullanarak uygulama ayarını şifreleme

Artık `WEBSITE_RUN_FROM_PACKAGE` uygulama ayarının değerini SAS kodlanmış URL'ye bir Key Vault başvurusuyla değiştirebilirsiniz. Bu, SAS URL'sini ekstra bir güvenlik katmanı sağlayan Key Vault'ta şifrelenmiş olarak tutar.

1. Anahtar Kasaörneği oluşturmak için aşağıdaki [`az keyvault create`](/cli/azure/keyvault#az-keyvault-create) komutu kullanın.       

    ```azurecli    
    az keyvault create --name "Contoso-Vault" --resource-group <group-name> --location eastus    
    ```    

1. Uygulamanızın anahtar [kasanıza erişmesini sağlamak için aşağıdaki talimatları uygulayın:](../app-service/app-service-key-vault-references.md#granting-your-app-access-to-key-vault)

1. Anahtar kasanızda harici URL'nizi gizli olarak eklemek için aşağıdaki [`az keyvault secret set`](/cli/azure/keyvault/secret#az-keyvault-secret-set) komutu kullanın:   

    ```azurecli    
    az keyvault secret set --vault-name "Contoso-Vault" --name "external-url" --value "<SAS-URL>"    
    ```    

1.  Dış URL'ye Key `WEBSITE_RUN_FROM_PACKAGE` Vault başvurusu olarak değeri olan uygulama ayarını oluşturmak için aşağıdaki [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) komutu kullanın:

    ```azurecli    
    az webapp config appsettings set --settings WEBSITE_RUN_FROM_PACKAGE="@Microsoft.KeyVault(SecretUri=https://Contoso-Vault.vault.azure.net/secrets/external-url/<secret-version>"    
    ```

    Önceki `<secret-version>` `az keyvault secret set` komutun çıktısında olacaktır.

Bu uygulama ayarını güncelleştirmek, işlev uygulamanızın yeniden başlatılmasına neden olur. Uygulama yeniden başlatıldıktan sonra, Anahtar Kasası başvurularını doğru kullanmaya başladığından emin olun.

## <a name="how-to-rotate-the-access-token"></a>Erişim belirteci döndürme

Depolama hesabınızın SAS anahtarını periyodik olarak döndürmek en iyi yöntemdir. İşlev uygulamasının yanlışlıkla erişimi kaybetmediğinden emin olmak için Key Vault'taki SAS URL'sini de güncellemeniz gerekir.

1. Azure portalındaki depolama hesabınıza giderek SAS tuşunu döndürün. **Ayarlar** > **Erişim tuşları**altında, SAS tuşunu döndürmek için simgeyi tıklatın.

1. Yeni SAS URL'sini kopyalayın ve anahtar kasanızda güncelleştirilmiş SAS URL'sini ayarlamak için aşağıdaki komutu kullanın:

    ```azurecli    
    az keyvault secret set --vault-name "Contoso-Vault" --name "external-url" --value "<SAS-URL>"    
    ``` 

1. Uygulama ayarınızdaki anahtar kasa başvurusunu yeni gizli sürüme güncelleştirin:

    ```azurecli    
    az webapp config appsettings set --settings WEBSITE_RUN_FROM_PACKAGE="@Microsoft.KeyVault(SecretUri=https://Contoso-Vault.vault.azure.net/secrets/external-url/<secret-version>"    
    ```

    Önceki `<secret-version>` `az keyvault secret set` komutun çıktısında olacaktır.

## <a name="how-to-revoke-the-function-apps-data-access"></a>İşlev uygulamasının veri erişimi nasıl iptal edile

İşlev uygulamasının depolama hesabına erişimini iptal etmek için iki yöntem vardır. 

### <a name="rotate-the-sas-key-for-the-azure-storage-account"></a>Azure Depolama hesabı için SAS tuşunu döndürme

Depolama hesabının SAS anahtarı döndürülürse, işlev uygulaması artık depolama hesabına erişmez, ancak paket dosyasının en son indirilen sürümüyle çalışmaya devam eder. Son indirilen sürümü temizlemek için işlev uygulamasını yeniden başlatın.

### <a name="remove-the-function-apps-access-to-key-vault"></a>İşlev uygulamasının Key Vault'a erişimini kaldırma

İşlev uygulamasının Key Vault'a erişimini devre dışı bırakarak işlev uygulamasının site verilerine erişimini iptal edebilirsiniz. Bunu yapmak için, işlev uygulamasının kimliğine yönelik erişim ilkesini kaldırın. Bu, anahtar kasa başvurularını yapılandırırken daha önce oluşturduğunuz kimliğin aynısır.

## <a name="summary"></a>Özet

Uygulama dosyalarınız artık depolama hesabınızda dinleniyerde şifrelenir. İşlev uygulamanız başlatıldığında, anahtar kasanızdan SAS URL'sini alır. Son olarak, işlev uygulaması uygulama dosyalarını depolama hesabından yükler. 

İşlev uygulamasının depolama hesabınıza erişimini iptal etmeniz gerekiyorsa, anahtar kasasına erişimi iptal edebilir veya Depolama Hesabı anahtarlarını döndürebilirsiniz, bu da SAS URL'sini geçersiz kılabilir.

## <a name="frequently-asked-questions"></a>Sık Sorulan Sorular

### <a name="is-there-any-additional-charge-for-running-my-function-app-from-the-deployment-package"></a>Dağıtım paketinden işlev uygulamamı çalıştırmak için ek bir ücret var mı?

Yalnızca Azure Depolama Hesabı ile ilişkili maliyet ve geçerli çıkış ücretleri.

### <a name="how-does-running-from-the-deployment-package-affect-my-function-app"></a>Dağıtım paketinden kaçmak işlev uygulamamı nasıl etkiler?

- Uygulamanızı dağıtım paketinden `wwwroot/` çalıştırmak salt okunur hale getirir. Uygulamanız bu dizine yazmaya çalıştığında bir hata alır.
- TAR ve GZIP biçimleri desteklenmez.
- Bu özellik yerel önbellekle uyumlu değildir.

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama Hizmeti için Anahtar Vault referansları](../app-service/app-service-key-vault-references.md)
- [Veriler için Azure Depolama şifrelemesi](../storage/common/storage-service-encryption.md)
