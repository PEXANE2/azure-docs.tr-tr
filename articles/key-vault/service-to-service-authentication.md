---
title: .NET kullanarak Azure Key Vault için hizmetten hizmete kimlik doğrulaması
description: .NET kullanarak Azure Key Vault kimlik doğrulaması yapmak için Microsoft. Azure. Services. AppAuthentication kitaplığını kullanın.
keywords: Azure Anahtar Kasası kimlik doğrulaması yerel kimlik bilgileri
author: msmbaldwin
manager: barbkess
services: key-vault
ms.author: mbaldwin
ms.date: 07/06/2019
ms.topic: conceptual
ms.service: key-vault
ms.openlocfilehash: d34c94ccca47d29afc4f3d83bec58db737be270c
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68840424"
---
# <a name="service-to-service-authentication-to-azure-key-vault-using-net"></a>.NET kullanarak Azure Key Vault için hizmetten hizmete kimlik doğrulaması

Azure Key Vault kimlik doğrulaması yapmak için, paylaşılan bir gizli dizi ya da sertifika olan bir Azure Active Directory (AD) kimlik bilgisi gerekir. 

Bu kimlik bilgilerini yönetmek zor olabilir ve kimlik bilgilerini kaynak veya yapılandırma dosyalarına dahil ederek bir uygulamaya paketlemekte olması mümkündür.  .Net `Microsoft.Azure.Services.AppAuthentication` kitaplığı için bu sorunu basitleştirir. Yerel geliştirme sırasında kimlik doğrulaması yapmak için geliştiricinin kimlik bilgilerini kullanır. Çözüm daha sonra Azure 'a dağıtıldığında, kitaplık otomatik olarak uygulama kimlik bilgilerine geçer.    Geliştirici kimlik bilgilerini yerel geliştirme sırasında kullanmak daha güvenlidir çünkü Azure AD kimlik bilgileri oluşturmanız veya kimlik bilgilerini geliştiriciler arasında paylaşmanız gerekmez.

`Microsoft.Azure.Services.AppAuthentication` Kitaplık kimlik doğrulamasını otomatik olarak yönetir, bu da kimlik bilgileriniz yerine çözümünüze odaklanabilmenizi sağlar.  Microsoft Visual Studio, Azure CLı veya Azure AD Tümleşik kimlik doğrulamasıyla yerel geliştirmeyi destekler. Yönetilen bir kimliği destekleyen bir Azure kaynağına dağıtıldığında, kitaplık otomatik olarak [Azure kaynakları için Yönetilen kimlikler](../active-directory/msi-overview.md)kullanır. Kod veya yapılandırma değişikliği gerekli değildir. Kitaplık Ayrıca, yönetilen bir kimlik olmadığında veya geliştiricinin güvenlik bağlamı yerel geliştirme sırasında belirlenemediğinde Azure AD [istemci kimlik bilgilerinin](../azure-resource-manager/resource-group-authenticate-service-principal.md) doğrudan kullanılmasını da destekler.

## <a name="using-the-library"></a>Kitaplığı kullanma

.NET uygulamaları için, yönetilen kimlik ile çalışmanın en kolay yolu `Microsoft.Azure.Services.AppAuthentication` paket üzerinden yapılır. Kullanmaya başlamak için şu adımları uygulayın:

1. Uygulamanıza [Microsoft. Azure. Services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) ve [Microsoft. Azure. keykasa](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) NuGet paketlerine başvurular ekleyin. 

2. Aşağıdaki kodu ekleyin:

    ``` csharp
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Azure.KeyVault;

    // Instantiate a new KeyVaultClient object, with an access token to Key Vault
    var azureServiceTokenProvider1 = new AzureServiceTokenProvider();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider1.KeyVaultTokenCallback));

    // Optional: Request an access token to other Azure services
    var azureServiceTokenProvider2 = new AzureServiceTokenProvider();
    string accessToken = await azureServiceTokenProvider2.GetAccessTokenAsync("https://management.azure.com/").ConfigureAwait(false);
    ```

`AzureServiceTokenProvider` Sınıfı, belirteci bellekte önbelleğe alır ve süresi dolmadan hemen önce Azure AD 'den alır. Sonuç olarak, `GetAccessTokenAsync` metodu çağırmadan önce son kullanma süresini denetlemeniz gerekmez. Yalnızca belirtecini kullanmak istediğinizde yöntemi çağırın. 

`GetAccessTokenAsync` Yöntemi bir kaynak tanımlayıcısı gerektirir. Daha fazla bilgi edinmek için bkz. Azure [kaynakları için yönetilen kimlikleri destekleyen Azure hizmetleri](../active-directory/msi-overview.md).

## <a name="local-development-authentication"></a>Yerel geliştirme kimlik doğrulaması

Yerel geliştirme için iki birincil kimlik doğrulama senaryosu vardır: [Azure hizmetlerinde kimlik doğrulaması](#authenticating-to-azure-services)ve [özel hizmetlere kimlik doğrulama](#authenticating-to-custom-services).

### <a name="authenticating-to-azure-services"></a>Azure hizmetlerinde kimlik doğrulaması

Yerel makineler, Azure kaynakları için yönetilen kimlikleri desteklemez.  Sonuç olarak, `Microsoft.Azure.Services.AppAuthentication` kitaplık, geliştirici kimlik bilgilerinizi yerel geliştirme ortamınızda çalıştırmak için kullanır. Çözüm Azure 'a dağıtıldığında, kitaplık bir OAuth 2,0 istemci kimlik bilgisi verme akışına geçiş yapmak için yönetilen bir kimlik kullanır.  Bu, endişelenmeden aynı kodu yerel olarak ve uzaktan sınaymanız anlamına gelir.

Yerel `AzureServiceTokenProvider` geliştirme için, **Visual Studio**, **Azure komut satırı arabirimi** (CLI) veya **Azure AD Tümleşik kimlik doğrulaması**kullanarak belirteçleri getirir. Her seçenek sırayla denenir ve kitaplık başarılı olan ilk seçeneği kullanır. Hiçbir seçenek çalışmadıysanız, ayrıntılı `AzureServiceTokenProviderException` bilgilerle bir özel durum oluşturulur.

### <a name="authenticating-with-visual-studio"></a>Visual Studio ile kimlik doğrulama

Visual Studio ile kimlik doğrulaması aşağıdaki önkoşullara sahiptir:

1. [Visual Studio 2017 v 15.5](https://blogs.msdn.microsoft.com/visualstudio/2017/10/11/visual-studio-2017-version-15-5-preview/) veya üzeri.

2. Visual Studio [Için uygulama kimlik doğrulama uzantısı](https://go.microsoft.com/fwlink/?linkid=862354), visual Studio 2017 güncelleştirme 5 için ayrı bir uzantı olarak kullanılabilir ve güncelleştirme 6 ve sonraki sürümlerinde ürünle birlikte paketlenmiştir. Güncelleştirme 6 veya sonraki sürümlerde, Visual Studio yükleyicisi içinden Azure Geliştirme Araçları ' nı seçerek uygulama kimlik doğrulaması uzantısının yüklenmesini doğrulayabilirsiniz.
 
Visual Studio 'da oturum açın ve yerel geliştirme için bir hesap seçmek üzere**Azure hizmeti kimlik doğrulaması** **Araçlar**&nbsp;&nbsp;&nbsp;>seçeneklerini>&nbsp;kullanın. 

Visual Studio 'Yu kullanarak, belirteç sağlayıcı dosyasıyla ilgili hatalar gibi sorunlarla karşılaşırsanız, bu adımları dikkatle gözden geçirin. 

Geliştirici belirtecinizi yeniden kimlik doğrulaması yapmak da gerekebilir. Bunu yapmak için, **Araçlar**&nbsp;>&nbsp;**Seçenekler** **Azure hizmetikimlik&nbsp;doğrulaması 'nagidinveseçili&nbsp;** > hesabı.  Kimlik doğrulamak için seçin. 

### <a name="authenticating-with-azure-cli"></a>Azure CLı ile kimlik doğrulama

Azure CLı 'yı yerel geliştirme için kullanmak üzere:

1. [Azure CLI v 2.0.12](/cli/azure/install-azure-cli) veya üstünü yükler. Önceki sürümleri yükseltin. 

2. Azure 'da oturum açmak için **az Login** kullanın.

Erişimi `az account get-access-token` doğrulamak için kullanın.  Bir hata alırsanız, adım 1 ' in başarıyla tamamlandığını doğrulayın. 

Azure CLI varsayılan dizine yüklenmemişse, Azure CLI yolunu bulamamayan bir hata bildirimi `AzureServiceTokenProvider` alabilirsiniz.  Azure CLı yükleme klasörünü tanımlamak için **Azureclipath** ortam değişkenini kullanın. `AzureServiceTokenProvider`gerekli olduğunda, **Azureclipath** ortam değişkeninde belirtilen dizini **Path** ortam değişkenine ekler.

Azure CLı 'de birden çok hesap kullanarak oturum açtıysanız veya hesabınızın birden çok aboneliğe erişimi varsa, kullanılacak belirli bir abonelik belirtmeniz gerekir.  Bunu yapmak için şunu kullanın:

```
az account set --subscription <subscription-id>
```

Bu komut yalnızca hata durumunda çıktı üretir.  Geçerli hesap ayarlarını doğrulamak için şunu kullanın:

```
az account list
```

### <a name="authenticating-with-azure-ad-authentication"></a>Azure AD kimlik doğrulamasıyla kimlik doğrulama

Azure AD kimlik doğrulamasını kullanmak için şunları doğrulayın:

- Şirket içi Active Directory 'niz [Azure AD 'ye eşitlenir](../active-directory/connect/active-directory-aadconnect.md).

- Kodunuz, etki alanına katılmış bir makinede çalışıyor.


### <a name="authenticating-to-custom-services"></a>Özel hizmetlerde kimlik doğrulaması

Bir hizmet Azure hizmetlerini çağırdığında, Azure hizmetleri hem kullanıcılara hem de uygulamalara erişim izni sağladığından önceki adımlar çalışır.  

Özel bir hizmet çağıran bir hizmet oluştururken yerel geliştirme kimlik doğrulaması için Azure AD istemci kimlik bilgilerini kullanın.  İki seçenek vardır: 

1.  Azure 'da oturum açmak için hizmet sorumlusu kullanın:

    1.  [Hizmet sorumlusu oluşturun](/cli/azure/create-an-azure-service-principal-azure-cli).

    2.  Oturum açmak için Azure CLı 'yi kullanın:

        ```azurecli
        az login --service-principal -u <principal-id> --password <password> --tenant <tenant-id> --allow-no-subscriptions
        ```

        Hizmet sorumlusunun bir aboneliğe erişimi olmayabilir, `--allow-no-subscriptions` bağımsız değişkenini kullanın.

2.  Hizmet sorumlusu ayrıntılarını belirtmek için ortam değişkenlerini kullanın.  Ayrıntılar için bkz. [bir hizmet sorumlusu kullanarak uygulamayı çalıştırma](#running-the-application-using-a-service-principal).

Azure 'da oturum açtıktan sonra yerel geliştirme için `AzureServiceTokenProvider` bir belirteç almak üzere hizmet sorumlusunu kullanır.

Bu yalnızca yerel geliştirme için geçerlidir. Çözümünüz Azure 'a dağıtıldığında, kitaplık kimlik doğrulaması için yönetilen bir kimliğe geçer.

## <a name="running-the-application-using-managed-identity-or-user-assigned-identity"></a>Yönetilen kimlik veya Kullanıcı tarafından atanan kimlik kullanılarak uygulamayı çalıştırma 

Kodunuzu bir Azure App Service veya yönetilen bir kimlik etkin bir Azure VM 'de çalıştırdığınızda, kitaplık yönetilen kimliği otomatik olarak kullanır. 

Alternatif olarak, Kullanıcı tarafından atanan bir kimlikle kimlik doğrulaması yapabilirsiniz. Kullanıcı tarafından atanan kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları Için Yönetilen kimlikler hakkında](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work). Kullanıcı tarafından atanan bir kimlikle kimlik doğrulamak için, bağlantı dizesinde kullanıcı tarafından atanan kimliğin Istemci KIMLIĞINI belirtmeniz gerekir. Bağlantı dizesi aşağıdaki [bağlantı dizesi desteği](#connection-string-support) bölümünde belirtilmiştir.

## <a name="running-the-application-using-a-service-principal"></a>Bir hizmet sorumlusu kullanarak uygulamayı çalıştırma 

Kimlik doğrulaması için bir Azure AD Istemci kimlik bilgisi oluşturulması gerekebilir. Ortak örnekler şunlardır:

- Kodunuz yerel bir geliştirme ortamında çalışıyor, ancak geliştiricinin kimliği altında değil.  Örneğin, Service Fabric, yerel geliştirme için [NetworkService hesabını](../service-fabric/service-fabric-application-secret-management.md) kullanır.
 
- Kodunuz yerel bir geliştirme ortamında çalışır ve özel bir hizmette kimlik doğrulaması yaptığınızda geliştirici kimliğinizi kullanamazsınız. 
 
- Kodunuz, Azure Batch gibi Azure kaynakları için yönetilen kimlikleri henüz desteklemeyen bir Azure işlem kaynağı üzerinde çalışıyor.

Uygulamanızı çalıştırmak için hizmet sorumlusu kullanmanın üç birincil yöntemi vardır. Bunlardan herhangi birini kullanmak için, önce [bir hizmet sorumlusu oluşturmanız](/cli/azure/create-an-azure-service-principal-azure-cli)gerekir.

### <a name="use-a-certificate-in-local-keystore-to-sign-into-azure-ad"></a>Azure AD 'de oturum açmak için yerel anahtar deposunda bir sertifika kullanın

1. Azure CLı [az ad SP Create-for-RBAC](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak bir hizmet sorumlusu sertifikası oluşturun. 

    ```azurecli
    az ad sp create-for-rbac --create-cert
    ```

    Bu işlem, giriş dizininizde depolanacak bir. ped dosyası (özel anahtar) oluşturur. Bu sertifikayı *LocalMachine* ya da *CurrentUser* deposuna dağıtın. 

    > [!Important]
    > CLı komutu bir. ped dosyası oluşturur, ancak Windows yalnızca PFX sertifikaları için yerel destek sağlar. Bunun yerine bir PFX sertifikası oluşturmak için burada gösterilen PowerShell komutlarını kullanın: [Otomatik olarak imzalanan sertifikayla hizmet sorumlusu oluşturun](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate). Bu komutlar sertifikayı da otomatik olarak dağıtır.

1. **AzureServicesAuthConnectionString** adlı bir ortam değişkenini şu şekilde ayarlayın:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};
          CertificateStoreLocation={CertificateStore}
    ```
 
    *{AppID}* , *{tenantıd}* ve *{parmak izi}* değerlerini adım 1 ' de oluşturulan değerlerle değiştirin. Dağıtım planınıza bağlı olarak, *{certificatestore}* öğesini veya `LocalMachine` `CurrentUser`ya da ile değiştirin.

1. Uygulamayı çalıştırın. 

### <a name="use-a-shared-secret-credential-to-sign-into-azure-ad"></a>Azure AD 'de oturum açmak için paylaşılan gizli kimlik bilgileri kullanın

1. [Az ad SP Create-for-RBAC--Password](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac)kullanarak bir parola ile hizmet sorumlusu sertifikası oluşturun. 

2. **AzureServicesAuthConnectionString** adlı bir ortam değişkenini şu şekilde ayarlayın:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret} 
    ```

    _{AppID}_ , _{tenantıd}_ ve _{ClientSecret}_ değerlerini 1. adımda oluşturulan değerlerle değiştirin.

3. Uygulamayı çalıştırın. 

Her şey doğru şekilde ayarlandıktan sonra daha fazla kod değişikliği yapmanız gerekmez.  `AzureServiceTokenProvider`, Azure AD kimlik doğrulaması için ortam değişkenini ve sertifikayı kullanır. 

### <a name="use-a-certificate-in-key-vault-to-sign-into-azure-ad"></a>Azure AD 'de oturum açmak için Key Vault bir sertifika kullanın

Bu seçenek, hizmet sorumlusunun istemci sertifikasını Key Vault depolamanızı ve hizmet sorumlusu kimlik doğrulaması için kullanmanızı sağlar. Bunu aşağıdaki senaryolar için kullanabilirsiniz:

* Açık hizmet sorumlusu kullanarak kimlik doğrulaması yapmak istediğiniz ve hizmet sorumlusu kimlik bilgilerini bir anahtar kasasında güvenli bir şekilde tutmak istediğiniz yerel kimlik doğrulaması. Geliştirici hesabının, anahtar kasasına erişimi olmalıdır. 
* Açık kimlik bilgisini (örneğin, platformlar arası senaryolar için) kullanmak istediğiniz Azure 'dan kimlik doğrulaması yapın ve hizmet sorumlusu kimlik bilgilerini bir anahtar kasasında güvenli bir şekilde tutmak isteyebilirsiniz. Yönetilen kimliğin anahtar kasasına erişimi olmalıdır. 

Yönetilen kimliğin veya geliştirici kimliğinizin Key Vault istemci sertifikasını alma izni olması gerekir. AppAuthentication kitaplığı, hizmet sorumlusunun istemci kimlik bilgileri olarak alınan sertifikayı kullanır.

Hizmet sorumlusu kimlik doğrulaması için bir istemci sertifikası kullanmak için

1. Hizmet sorumlusu sertifikası oluşturun ve Azure CLI [az ad SP Create-for-RBAC-- <keyvaultname> keykasasını kullanarak keykasasında otomatik olarak depolayın.-CERT <certificatename> --Create-CERT--Skip-atama](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutu:

    ```azurecli
    az ad sp create-for-rbac --keyvault <keyvaultname> --cert <certificatename> --create-cert --skip-assignment
    ```
    
    Sertifika tanımlayıcısı, biçimdeki bir URL olacaktır`https://<keyvaultname>.vault.azure.net/secrets/<certificatename>`

1. Bu `{KeyVaultCertificateSecretIdentifier}` bağlantı dizesinde, sertifika tanımlayıcısı ile değiştirin:

    ```
    RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier={KeyVaultCertificateSecretIdentifier}
    ```

    Örneğin, anahtar kasanızın "Mykeykasası" olarak adlandırıldıysa ve ' myCert ' adlı bir sertifika oluşturduysanız, sertifika tanımlayıcısı şöyle olacaktır:

    ```
    RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier=https://myKeyVault.vault.azure.net/secrets/myCert
    ```


## <a name="connection-string-support"></a>Bağlantı dizesi desteği

Varsayılan olarak, `AzureServiceTokenProvider` bir belirteci almak için birden çok yöntem kullanır. 

İşlemi denetlemek için, `AzureServiceTokenProvider` oluşturucuya geçirilmiş veya *AzureServicesAuthConnectionString* ortam değişkeninde belirtilen bir bağlantı dizesi kullanın. 

Aşağıdaki seçenekler desteklenir:

| Bağlantı dizesi seçeneği | Senaryo | Açıklamalar|
|:--------------------------------|:------------------------|:----------------------------|
| `RunAs=Developer; DeveloperTool=AzureCli` | Yerel geliştirme | AzureServiceTokenProvider belirteci almak için AzureCli kullanır. |
| `RunAs=Developer; DeveloperTool=VisualStudio` | Yerel geliştirme | AzureServiceTokenProvider belirteci almak için Visual Studio 'Yu kullanır. |
| `RunAs=CurrentUser` | Yerel geliştirme | AzureServiceTokenProvider belirteci almak için Azure AD Tümleşik kimlik doğrulamasını kullanır. |
| `RunAs=App` | [Azure kaynakları için yönetilen kimlikler](../active-directory/managed-identities-azure-resources/index.yml) | AzureServiceTokenProvider belirteci almak için yönetilen bir kimlik kullanır. |
| `RunAs=App;AppId={ClientId of user-assigned identity}` | [Azure kaynakları için Kullanıcı tarafından atanan kimlik](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work) | AzureServiceTokenProvider belirteci almak için Kullanıcı tarafından atanan bir kimlik kullanır. |
| `RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier={KeyVaultCertificateSecretIdentifier}` | Özel Hizmetler kimlik doğrulaması | KeyVaultCertificateSecretIdentifier = sertifikanın gizli tanımlayıcısı. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};CertificateStoreLocation={LocalMachine or CurrentUser}`| Hizmet sorumlusu | `AzureServiceTokenProvider`Azure AD 'den belirteç almak için sertifikayı kullanır. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateSubjectName={Subject};CertificateStoreLocation={LocalMachine or CurrentUser}` | Hizmet sorumlusu | `AzureServiceTokenProvider`Azure AD 'den belirteç almak için sertifikayı kullanır|
| `RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}` | Hizmet sorumlusu |`AzureServiceTokenProvider`Azure AD 'den belirteç almak için gizli dizi kullanır. |

## <a name="samples"></a>Örnekler

`Microsoft.Azure.Services.AppAuthentication` Kitaplığı eylemde görmek için lütfen aşağıdaki kod örneklerine bakın.

1. [Çalışma zamanında Azure Key Vault bir gizli dizi almak için yönetilen bir kimlik kullanın](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet)

2. [Yönetilen kimliğe sahip bir Azure VM 'Den program aracılığıyla bir Azure Resource Manager şablonu dağıtın](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet).

3. Azure [LINUX VM 'Den Azure hizmetlerini çağırmak için .NET Core örneğini ve yönetilen bir kimliği kullanın](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/).

## <a name="appauthentication-troubleshooting"></a>AppAuthentication sorunlarını giderme

### <a name="common-issues-during-local-development"></a>Yerel geliştirme sırasında sık karşılaşılan sorunlar

#### <a name="azure-cli-is-not-installed-you-are-not-logged-in-or-you-do-not-have-the-latest-version"></a>Azure CLı yüklü değil, oturum açmadınız veya en son sürüme sahip değilsiniz

Azure CLı 'nın sizin için bir belirteç gösterdiğini görmek için **az Account Get-Access-Token** ' i çalıştırın. Böyle bir program bulunmazsa, lütfen [Azure CLI 'nın en son sürümünü](/cli/azure/install-azure-cli?view=azure-cli-latest)yüklemelisiniz. Yüklediyseniz, oturum açmanız istenebilir. 
 
#### <a name="azureservicetokenprovider-cannot-find-the-path-for-azure-cli"></a>AzureServiceTokenProvider Azure CLı için yolu bulamıyor

AzureServiceTokenProvider, Azure CLı 'yi varsayılan yüklemesi konumlarında arar. Azure CLı bulamazsa lütfen **Azureclipath** ortam DEĞIŞKENINI Azure CLI yükleme klasörü olarak ayarlayın. AzureServiceTokenProvider, ortam değişkenini PATH ortam değişkenine ekler.
 
#### <a name="you-are-logged-into-azure-cli-using-multiple-accounts-the-same-account-has-access-to-subscriptions-in-multiple-tenants-or-you-get-an-access-denied-error-when-trying-to-make-calls-during-local-development"></a>Azure CLı 'de birden çok hesap kullanarak oturum açtınız, aynı hesabın birden çok Kiracıdaki aboneliğe erişimi vardır veya yerel geliştirme sırasında çağrı yapmaya çalışırken bir erişim reddedildi hatası alırsınız

Azure CLı kullanarak, varsayılan aboneliği kullanmak istediğiniz hesabı içeren bir tane olarak ayarlayın ve erişmek istediğiniz kaynakla aynı kiracıda yer vardır: **az hesap kümesi--abonelik [abonelik-kimliği]** . Hiçbir çıkış görülemeyeceğini başarılı olur. Doğru hesabın artık **az Account List**kullanılarak varsayılan olduğunu doğrulayın.

### <a name="common-issues-across-environments"></a>Ortamlar arasında yaygın sorunlar

#### <a name="unauthorized-access-access-denied-forbidden-etc-error"></a>Yetkisiz erişim, erişim engellendi, yasak, vb. hata
 
Kullanılan sorumlunun erişmeye çalıştığı kaynağa erişimi yok. Örneği yerel geliştirme makinenizde çalıştırıp çalıştırdığınıza veya Azure 'da App Service Azure 'da dağıtılmış olmasına bağlı olarak, istenen kaynağa kullanıcı hesabınızı veya App Service MSI "katkıda bulunan" erişimini verin. Anahtar kasaları gibi bazı kaynaklarda ayrıca, sorumlulara (kullanıcılar, uygulamalar, gruplar vb.) erişim izni vermek için kullandığınız kendi [erişim ilkeleri](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault#data-plane-and-access-policies) de vardır.

### <a name="common-issues-when-deployed-to-azure-app-service"></a>Azure App Service dağıtılan yaygın sorunlar

#### <a name="managed-identity-is-not-setup-on-the-app-service"></a>Yönetilen kimlik App Service ayarlanmadı
 
MSI_ENDPOINT ve MSI_SECRET ortam değişkenlerini [kudu hata ayıklama konsolunu](https://azure.microsoft.com/en-us/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/)kullanarak denetleyin. Bu ortam değişkenleri yoksa, App Service yönetilen kimlik etkinleştirilmez. 
 
### <a name="common-issues-when-deployed-locally-with-iis"></a>IIS ile yerel olarak dağıtılan yaygın sorunlar

#### <a name="cant-retrieve-tokens-when-debugging-app-in-iis"></a>IIS 'de uygulamada hata ayıklarken belirteçler alınamıyor

Varsayılan olarak, AppAuth, IIS 'de farklı bir kullanıcı bağlamında çalışır ve bu nedenle, erişim belirteçlerini almak için geliştirici kimliğinizi kullanma erişimine sahip değildir. Aşağıdaki iki adımla IIS 'yi Kullanıcı bağlamla çalışacak şekilde yapılandırabilirsiniz:
- Web uygulaması için uygulama havuzunu geçerli kullanıcı hesabınız olarak çalışacak şekilde yapılandırın. Daha fazla bilgi için [buraya](https://docs.microsoft.com/en-us/iis/manage/configuring-security/application-pool-identities#configuring-iis-application-pool-identities) bakın
- "SetProfileEnvironment" öğesini "true" olarak yapılandırın. Daha fazla bilgi için [buraya](https://docs.microsoft.com/en-us/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)bakın. 

    - %Windir%\System32\inetsrv\config\applicationHost,config adresine gidin
    - "SetProfileEnvironment" araması yapın. "False" olarak ayarlanırsa, "true" olarak değiştirin. Mevcut değilse, processModel öğesine (/configuration/system.applicationHost/applicationPools/applicationPoolDefaults/processModel/@setProfileEnvironment) bir özniteliği olarak ekleyin ve bunu "true" olarak ayarlayın.

- [Azure kaynakları için Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/index.yml)hakkında daha fazla bilgi edinin.
- [Azure AD kimlik doğrulama senaryoları](../active-directory/develop/active-directory-authentication-scenarios.md)hakkında daha fazla bilgi edinin.
