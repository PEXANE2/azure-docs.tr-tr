---
title: .NET kullanarak Azure Anahtar Kasası'na servis-servis kimlik doğrulaması
description: .NET kullanarak Azure Key Vault'a kimlik doğrulaması yapmak için Microsoft.Azure.Services.AppAuthentication kitaplığını kullanın.
keywords: azure anahtar kasakimlik doğrulama yerel kimlik bilgileri
author: msmbaldwin
manager: rkarlin
services: key-vault
ms.author: mbaldwin
ms.date: 08/28/2019
ms.topic: conceptual
ms.service: key-vault
ms.subservice: general
ms.openlocfilehash: cd630acfd65f0a79c186ba35bc15627bf7ccfdbe
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686187"
---
# <a name="service-to-service-authentication-to-azure-key-vault-using-net"></a>.NET kullanarak Azure Anahtar Kasası'na servis-servis kimlik doğrulaması

Azure Anahtar Kasası'na kimlik doğrulamak için paylaşılan bir sır veya sertifika olmak üzere bir Azure Etkin Dizin (Azure AD) kimlik bilgisine ihtiyacınız vardır.

Bu tür kimlik bilgilerini yönetmek zor olabilir. Kimlik bilgilerini kaynak veya yapılandırma dosyalarına ekleyerek bir uygulamaya eklemeyapmak caziptir. .NET `Microsoft.Azure.Services.AppAuthentication` kitaplığı bu sorunu basitleştirir. Yerel geliştirme sırasında kimlik doğrulaması yapmak için geliştiricinin kimlik bilgilerini kullanır. Çözüm daha sonra Azure'a dağıtıldığında, kitaplık otomatik olarak uygulama kimlik bilgilerine geçer. Azure AD kimlik bilgileri oluşturmanız veya geliştiriciler arasında kimlik bilgileri paylaşmanız gerekmedığından, yerel geliştirme sırasında geliştirici kimlik bilgilerini kullanmak daha güvenlidir.

`Microsoft.Azure.Services.AppAuthentication` Kitaplık kimlik doğrulamayı otomatik olarak yönetir ve bu da kimlik bilgileriniz yerine çözümünüze odaklanmanızı sağlar. Microsoft Visual Studio, Azure CLI veya Azure AD Tümleşik Kimlik Doğrulaması ile yerel geliştirmeyi destekler. Kitaplık, yönetilen bir kimliği destekleyen bir Azure kaynağına dağıtıldığında, [Azure kaynakları için yönetilen kimlikleri](../../active-directory/msi-overview.md)otomatik olarak kullanır. Kod veya yapılandırma değişikliği gerekmez. Kitaplık, yönetilen bir kimlik kullanılamadığında veya geliştiricinin güvenlik bağlamı yerel geliştirme sırasında belirlenemediğinde Azure AD [istemci kimlik bilgilerinin](../../azure-resource-manager/resource-group-authenticate-service-principal.md) doğrudan kullanımını da destekler.

## <a name="prerequisites"></a>Ön koşullar

- [Visual Studio 2019](https://www.visualstudio.com/downloads/) veya [Visual Studio 2017 v15.5](https://blogs.msdn.microsoft.com/visualstudio/2017/10/11/visual-studio-2017-version-15-5-preview/).

- Visual Studio için App Authentication uzantısı, Visual Studio için ayrı bir uzantısı olarak kullanılabilir 2017 Güncelleme 5 ve Güncelleme 6 ve daha sonra ürün ile birlikte. Güncelleştirme 6 veya daha sonra, Visual Studio yükleyicisinden Azure Geliştirme araçlarını seçerek Uygulama Kimlik Doğrulama uzantısının yüklenmesini doğrulayabilirsiniz.

## <a name="using-the-library"></a>Kitaplığı kullanma

.NET uygulamaları için yönetilen kimlikle çalışmanın en basit yolu `Microsoft.Azure.Services.AppAuthentication` paketten geçer. Şu şekilde başlayabilirsiniz:

1.  >  [Projenize Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) ve [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) NuGet paketlerine referans eklemek için Sürü Paketi**YöneticisiNi** > **Yönet'in Çözüm** için Paketleri Yönet'ini seçin. **Tools**

1. Aşağıdaki kodu ekleyin:

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

Sınıf, `AzureServiceTokenProvider` bellekteki belirteci önbelleğe alır ve son kullanma tarihinden hemen önce Azure AD'den alır. Bu nedenle, `GetAccessTokenAsync` yöntemi aramadan önce artık son kullanma tarihini kontrol etmek zorunda kalmamalısınız. Belirteci kullanmak istediğinizde yöntemi aramanız gereken bir yöntem.

Yöntem `GetAccessTokenAsync` bir kaynak tanımlayıcısı gerektirir. Microsoft Azure hizmetleri hakkında daha fazla bilgi edinmek [için Azure kaynakları için yönetilen kimliklerin ne olduğunu](../../active-directory/msi-overview.md)öğrenin.

## <a name="local-development-authentication"></a>Yerel geliştirme kimlik doğrulaması

Yerel geliştirme için iki temel kimlik doğrulama senaryosu vardır: [Azure hizmetlerine kimlik doğrulama](#authenticating-to-azure-services)ve özel [hizmetlere kimlik doğrulama.](#authenticating-to-custom-services)

### <a name="authenticating-to-azure-services"></a>Azure Hizmetlerine Kimlik Doğrulama

Yerel makineler Azure kaynakları için yönetilen kimlikleri desteklemez. Sonuç olarak, `Microsoft.Azure.Services.AppAuthentication` kitaplık yerel geliştirme ortamınızda çalıştırmak için geliştirici kimlik bilgilerinizi kullanır. Çözüm Azure'a dağıtıldığında, kitaplık OAuth 2.0 istemci kimlik bilgisi hibe akışına geçmek için yönetilen bir kimlik kullanır. Bu yaklaşım, aynı kodu endişelenmeden yerel ve uzaktan test edebilirsiniz anlamına gelir.

Yerel geliştirme `AzureServiceTokenProvider` için, **Visual Studio,** **Azure komut satırı arabirimi** (CLI) veya **Azure AD Tümleşik Kimlik Doğrulaması**kullanarak belirteçleri getirir. Her seçenek sırayla denenir ve kitaplık başarılı olan ilk seçeneği kullanır. Hiçbir seçenek işe `AzureServiceTokenProviderException` yaramazsa, ayrıntılı bilgilerle birlikte bir özel durum atılır.

#### <a name="authenticating-with-visual-studio"></a>Visual Studio ile kimlik doğrulama

Visual Studio kullanarak kimlik doğrulaması yapmak için:

1. Visual Studio'da oturum açın ve **Seçenekler'i**açmak için **Araçlar**&nbsp;>&nbsp;**Seçenekleri'ni** kullanın.

1. **Azure Hizmet Kimlik Doğrulaması'nı**seçin, yerel geliştirme için bir hesap seçin ve **Tamam'ı**seçin.

Belirteç sağlayıcısı dosyasını içeren hatalar gibi Visual Studio'yu kullanarak sorunlarla karşılaştıysanız, önceki adımları dikkatle inceleyin.

Geliştirici belirtecinizi yeniden doğrulamanız gerekebilir. Bunu yapmak için **Araçlar**&nbsp;>&nbsp;**Seçenekleri'ni**seçin ve ardından **Azure&nbsp;Hizmet&nbsp;Kimlik Doğrulaması'nı**seçin. Seçili hesabın altında **yeniden kimlik doğrulaması** bağlantısı arayın. Kimlik doğrulaması için seçin.

#### <a name="authenticating-with-azure-cli"></a>Azure CLI ile kimlik doğrulaması

Azure CLI'yi yerel geliştirme için kullanmak için [Azure CLI v2.0.12](/cli/azure/install-azure-cli) veya sonraki sürümüne sahip olduğundan emin olun.

Azure CLI'yi kullanmak için:

1. **Microsoft Azure Komut Komut Istemi'ni**açmak için Windows Görev Çubuğu'nda Azure CLI'yi arayın.

1. Azure portalında oturum açın: Azure'da oturum açmak için *az oturum* açın.

1. *Az hesap get-access-token -kaynak https:\//vault.azure.net*girerek erişimi doğrulayın . Bir hata alırsanız, Azure CLI'nin doğru sürümünün doğru yüklendiğinden denetleyin.

   Azure CLI varsayılan dizine yüklenmezse, Azure CLI'nin `AzureServiceTokenProvider` yolunu bulamayan bir hata bildirimi alabilirsiniz. Azure CLI yükleme klasörünü tanımlamak için **AzureCLIPath** ortamı değişkenini kullanın. `AzureServiceTokenProvider`Gerektiğinde **Yol** ortamı değişkenine **AzureCLIPath** ortamı değişkeninde belirtilen dizini ekler.

1. Birden fazla hesap kullanarak Azure CLI'de oturum açtıysanız veya hesabınızda birden çok aboneliğe erişebiliyorsanız, kullanılacak aboneliği belirtmeniz gerekir. komut az hesap kümesini girin *--abonelik <abonelik-id>. *

Bu komut yalnızca hata yla çıktı üretir. Cari hesap ayarlarını doğrulamak için `az account list`komutu girin.

#### <a name="authenticating-with-azure-ad-authentication"></a>Azure AD kimlik doğrulaması ile kimlik doğrulaması

Azure AD kimlik doğrulamasını kullanmak için şu doğruları doğrulayın:

- Şirket içi Active Directory'niz Azure AD ile senkronize olur. Daha fazla bilgi için Azure [Etkin Dizini ile karma kimlik nedir?](../../active-directory/connect/active-directory-aadconnect.md)

- Kodunuz etki alanı birleştirilmiş bir bilgisayarda çalışıyor.

### <a name="authenticating-to-custom-services"></a>Özel hizmetlere kimlik doğrulama

Bir hizmet Azure hizmetlerini aradığında, Azure hizmetleri hem kullanıcılara hem de uygulamalara erişime izin verdiği için önceki adımlar çalışır.

Özel hizmet çağıran bir hizmet oluştururken, yerel geliştirme kimlik doğrulaması için Azure AD istemci kimlik bilgilerini kullanın. İki seçenek vardır:

- Azure'da oturum imzalamak için bir hizmet sorumlusu kullanın:

    1. Bir hizmet ilkesi oluşturun. Daha fazla bilgi için bkz. Azure [CLI ile bir Azure hizmet ilkesi oluştur.](/cli/azure/create-an-azure-service-principal-azure-cli)

    1. Oturum açabilmek için Azure CLI'yi kullanın:

        ```azurecli
        az login --service-principal -u <principal-id> --password <password> --tenant <tenant-id> --allow-no-subscriptions
        ```

        Hizmet sorumlusunun bir aboneye erişimi olmayabilir, `--allow-no-subscriptions` bağımsız değişkeni kullanın.

- Hizmet temel ayrıntılarını belirtmek için ortam değişkenlerini kullanın. Daha fazla bilgi için [bkz.](#running-the-application-using-a-service-principal)

Azure'da oturum imzaladıktan `AzureServiceTokenProvider` sonra, yerel geliştirme için bir belirteç almak için hizmet ilkesini kullanır.

Bu yaklaşım yalnızca yerel kalkınma için geçerlidir. Çözümünüz Azure'a dağıtıldığında, kitaplık kimlik doğrulaması için yönetilen bir kimliğe geçer.

## <a name="running-the-application-using-managed-identity-or-user-assigned-identity"></a>Uygulamayı yönetilen kimlik veya kullanıcı tarafından atanan kimliği kullanarak çalıştırma

Kodunuzu bir Azure Uygulama Hizmeti'nde veya yönetilen bir kimliğe sahip bir Azure VM'de çalıştırdığınızda, kitaplık yönetilen kimliği otomatik olarak kullanır. Kod değişikliği gerekmez, ancak yönetilen kimlik anahtar kasası için *izin* almış olmalıdır. Yönetilen kimlik anahtar kasanın *Erişim İlkeleri*aracılığıyla izin *alma* verebilirsiniz.

Alternatif olarak, kullanıcı tarafından atanan bir kimlikle kimlik doğrulaması olabilirsiniz. Kullanıcı tarafından atanan kimlikler hakkında daha fazla bilgi için Azure [kaynakları için Yönetilen Kimlikler Hakkında'ya](../../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work)bakın. Kullanıcı tarafından atanan bir kimlikle kimlik doğrulaması yapmak için bağlantı dizesinde kullanıcı tarafından atanan kimliğin İstemci Kimliğini belirtmeniz gerekir. Bağlantı dizesi [Bağlantı Dizesi Desteği'nde](#connection-string-support)belirtilir.

## <a name="running-the-application-using-a-service-principal"></a>Bir Hizmet Sorumlusu kullanarak uygulamayı çalıştırma

Kimlik doğrulaması için bir Azure AD İstemci kimlik bilgisi oluşturmak gerekebilir. Bu durum aşağıdaki örneklerde meydana gelebilir:

- Kodunuz yerel bir geliştirme ortamında çalışır, ancak geliştiricinin kimliği altında değil. Örneğin Service Fabric, yerel geliştirme için [NetworkService hesabını](../../service-fabric/service-fabric-application-secret-management.md) kullanır.

- Kodunuz yerel bir geliştirme ortamında çalışır ve geliştirici kimliğinizi kullanamazsınız, böylece özel bir hizmete kimlik doğrularsınız.

- Kodunuz, Azure Toplu İşi gibi Azure kaynaklarıiçin yönetilen kimlikleri henüz desteklemeyen bir Azure bilgi işlem kaynağında çalışıyor.

Başvurunuzu çalıştırmak için Bir Hizmet Sorumlusu kullanmanın üç birincil yöntemi vardır. Bunlardan herhangi birini kullanmak için öncelikle bir hizmet ilkesi oluşturmanız gerekir. Daha fazla bilgi için bkz. Azure [CLI ile bir Azure hizmet ilkesi oluştur.](/cli/azure/create-an-azure-service-principal-azure-cli)

### <a name="use-a-certificate-in-local-keystore-to-sign-into-azure-ad"></a>Azure AD'de oturum açabilmek için yerel anahtar mağazasında bir sertifika kullanma

1. Azure CLI [az reklam sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak bir hizmet temel sertifikası oluşturun.

    ```azurecli
    az ad sp create-for-rbac --create-cert
    ```

    Bu komut, ev dizininizde depolanan bir .pem dosyası (özel anahtar) oluşturur. Bu sertifikayı *LocalMachine* veya *CurrentUser* deposuna dağıtın.

    > [!Important]
    > CLI komutu bir .pem dosyası oluşturur, ancak Windows yalnızca PFX sertifikaları için yerel destek sağlar. Bunun yerine bir PFX sertifikası oluşturmak için, burada gösterilen PowerShell komutlarını kullanın: [Kendi imzalı sertifikaile hizmet sorumlusu oluşturun.](../../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate) Bu komutlar sertifikayı da otomatik olarak dağır.

1. **AzureServicesAuthConnectionString** adlı bir ortam değişkenini aşağıdaki değere ayarlayın:

    ```azurecli
    RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};
          CertificateStoreLocation={CertificateStore}
    ```

    *{AppId}*, *{TenantId}* ve *{Thumbprint}* adım 1'de oluşturulan değerlerle değiştirin. Dağıtım planınıza göre *{CertificateStore}* ile *LocalMachine*' veya *CurrentUser'ı*değiştirin.

1. Uygulamayı çalıştırın.

### <a name="use-a-shared-secret-credential-to-sign-into-azure-ad"></a>Azure AD'de oturum açabilmek için paylaşılan gizli kimlik bilgileri kullanma

1. --sdk-auth parametresi ile Azure CLI [az reklam sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak parola içeren bir hizmet temel sertifikası oluşturun.

    ```azurecli
    az ad sp create-for-rbac --sdk-auth
    ```

1. **AzureServicesAuthConnectionString** adlı bir ortam değişkenini aşağıdaki değere ayarlayın:

    ```azurecli
    RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}
    ```

    _{AppId}_, _{TenantId}_ ve _{ClientSecret}_ adım 1'de oluşturulan değerlerle değiştirin.

1. Uygulamayı çalıştırın.

Her şey doğru ayarlandıktan sonra, başka kod değişikliği gerekmez. `AzureServiceTokenProvider`Azure AD'ye kimlik doğrulamak için ortam değişkenini ve sertifikayı kullanır.

### <a name="use-a-certificate-in-key-vault-to-sign-into-azure-ad"></a>Azure AD'de oturum açabilmek için Key Vault'ta sertifika kullanma

Bu seçenek, bir servis müdürünün istemci sertifikasını Anahtar Kasa'da saklamanızı ve hizmet asıl kimlik doğrulaması için kullanmanıza olanak tanır. Aşağıdaki senaryolar için bu seçeneği kullanabilirsiniz:

- Açık bir hizmet ilkesi kullanarak kimlik doğrulaması yapmak istediğiniz ve hizmet ana bilim ini anahtar kasasında güvenli bir şekilde tutmak istediğiniz yerel kimlik doğrulaması. Geliştirici hesabının anahtar kasasına erişimi olmalıdır.

- Açık kimlik bilgisi kullanmak istediğiniz ve hizmet temel kimlik belgesini anahtar kasasında güvenli bir şekilde tutmak istediğiniz Azure'dan kimlik doğrulaması. Bu seçeneği, kiracılar arası bir senaryo için kullanabilirsiniz. Yönetilen kimlik anahtar kasasına erişebilmeli.

Yönetilen kimliğin veya geliştirici kimliğinizin anahtar kasadan istemci sertifikasını almak için izni olmalıdır. AppAuthentication kitaplığı, alınan sertifikayı servis sorumlusunun istemci kimlik bilgisi olarak kullanır.

Hizmet asıl kimlik doğrulaması için istemci sertifikası kullanmak için:

1. Bir hizmet temel sertifikası oluşturun ve otomatik olarak Key Vault'unuzda saklayın. Azure CLI [az reklam sp create-for-rbac \<--keyvault keyvaultname> --cert \<sertifika adı> --create-cert -skip-assignment](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanın:

    ```azurecli
    az ad sp create-for-rbac --keyvault <keyvaultname> --cert <certificatename> --create-cert --skip-assignment
    ```

    Sertifika tanımlayıcısı biçimde bir URL olacak`https://<keyvaultname>.vault.azure.net/secrets/<certificatename>`

1. Bu `{KeyVaultCertificateSecretIdentifier}` bağlantı dizesini sertifika tanımlayıcısıyla değiştirin:

    ```azurecli
    RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier={KeyVaultCertificateSecretIdentifier}
    ```

    Örneğin, anahtar kasanız *myKeyVault* olarak adlandırıldıysa ve *myCert*adında bir sertifika oluşturduysanız, sertifika tanımlayıcısı:

    ```azurecli
    RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier=https://myKeyVault.vault.azure.net/secrets/myCert
    ```

## <a name="connection-string-support"></a>Bağlantı Dize Desteği

Varsayılan olarak, `AzureServiceTokenProvider` bir belirteç almak için birden çok yöntem kullanır.

İşlemi denetlemek `AzureServiceTokenProvider` için, oluşturucuya geçirilen veya *AzureServicesAuthConnectionString* ortam değişkeninde belirtilen bir bağlantı dizesini kullanın.

Aşağıdaki seçenekler desteklenir:

| Bağlantı dizesi seçeneği | Senaryo | Yorumlar|
|:--------------------------------|:------------------------|:----------------------------|
| `RunAs=Developer; DeveloperTool=AzureCli` | Yerel geliştirme | `AzureServiceTokenProvider`belirteci almak için AzureCli'yi kullanır. |
| `RunAs=Developer; DeveloperTool=VisualStudio` | Yerel geliştirme | `AzureServiceTokenProvider`belirteci almak için Visual Studio kullanır. |
| `RunAs=CurrentUser` | Yerel geliştirme | `AzureServiceTokenProvider`belirteci almak için Azure AD Tümleşik Kimlik Doğrulaması'nı kullanır. |
| `RunAs=App` | [Azure kaynakları için yönetilen kimlikler](../../active-directory/managed-identities-azure-resources/index.yml) | `AzureServiceTokenProvider`belirteci almak için yönetilen bir kimlik kullanır. |
| `RunAs=App;AppId={ClientId of user-assigned identity}` | [Azure kaynakları için kullanıcı tarafından atanan kimlik](../../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work) | `AzureServiceTokenProvider`belirteci almak için kullanıcı tarafından atanmış bir kimlik kullanır. |
| `RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier={KeyVaultCertificateSecretIdentifier}` | Özel hizmetler kimlik doğrulaması | `KeyVaultCertificateSecretIdentifier`sertifikanın gizli tanımlayıcısIdır. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};CertificateStoreLocation={LocalMachine or CurrentUser}`| Hizmet sorumlusu | `AzureServiceTokenProvider`Azure AD'den belirteç almak için sertifika kullanır. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateSubjectName={Subject};CertificateStoreLocation={LocalMachine or CurrentUser}` | Hizmet sorumlusu | `AzureServiceTokenProvider`Azure AD'den belirteç almak için sertifika kullanır|
| `RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}` | Hizmet sorumlusu |`AzureServiceTokenProvider`Azure AD'den belirteç almak için gizli bir belirteç kullanır. |

## <a name="samples"></a>Örnekler

`Microsoft.Azure.Services.AppAuthentication` Kitaplığı iş başında görmek için aşağıdaki kod örneklerine bakın.

- [Çalışma zamanında Azure Key Vault'tan bir sır almak için yönetilen bir kimlik kullanma](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet)

- [Yönetilen bir kimliğe sahip bir Azure VM'den bir Azure Kaynak Yöneticisi şablonu programlanabilir şekilde dağıtın.](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet)

- [Azure hizmetlerini Azure Linux VM'den aramak için .NET Core örneğini ve yönetilen bir kimliği kullanın.](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/)

## <a name="appauthentication-troubleshooting"></a>AppAuthentication Sorun Giderme

### <a name="common-issues-during-local-development"></a>Yerel kalkınma sırasında sık karşılaşılan sorunlar

#### <a name="azure-cli-is-not-installed-youre-not-logged-in-or-you-dont-have-the-latest-version"></a>Azure CLI yüklü değil, oturum açmadın veya en son sürümün yok

Azure CLI'nin sizin için bir belirteç gösterip gösterip gösterolmadığını görmek için *az hesap erişim* jetonunu çalıştırın. **Böyle bir program bulunamadı**diyorsa, Azure [CLI'nin en son sürümünü yükleyin.](/cli/azure/install-azure-cli?view=azure-cli-latest) Oturum açmanız istenebilir.

#### <a name="azureservicetokenprovider-cant-find-the-path-for-azure-cli"></a>AzureServiceTokenProvider Azure CLI için yol bulamıyor

AzureServiceTokenProvider varsayılan yükleme konumlarında Azure CLI'yi arar. Azure CLI'yi bulamazsa, ortam değişkeni **AzureCLIPath'i** Azure CLI yükleme klasörüne ayarlayın. AzureServiceTokenProvider, Yol ortamı değişkenine ortam değişkenini ekler.

#### <a name="youre-logged-into-azure-cli-using-multiple-accounts-the-same-account-has-access-to-subscriptions-in-multiple-tenants-or-you-get-an-access-denied-error-when-trying-to-make-calls-during-local-development"></a>Birden çok hesap kullanarak Azure CLI'ye giriş yaptınız, aynı hesabın birden çok kiracıdaki aboneliklere erişimi var veya yerel geliştirme sırasında arama yapmaya çalışırken Access Reddedildi hatası alırsınız

Azure CLI'yi kullanarak, varsayılan aboneliği kullanmak istediğiniz hesaba sahip birine ayarlayın. Abonelik, erişmek istediğiniz kaynakla aynı kiracıda olmalıdır: **az hesap kümesi --abonelik [abonelik-id]**. Çıktı görülmezse, başarılı oldu. Doğru hesabı doğrulayın şimdi **az hesap listesini**kullanarak varsayılan .

### <a name="common-issues-across-environments"></a>Ortamlar arasında sık karşılaşılan sorunlar

#### <a name="unauthorized-access-access-denied-forbidden-or-similar-error"></a>Yetkisiz erişim, erişim engellendi, yasaklandı veya benzer bir hata

Kullanılan sorumlunun erişmeye çalıştığı kaynağa erişimi yok. Kullanıcı hesabınızı veya App Service'in MSI "Katılımcısı"nın kaynağa erişimini sağlar. Hangisi, örneği yerel bilgisayarınızda çalıştırıp çalıştırmadığınıza veya Azure'da Uygulama Hizmetinize dağıtılıp dağıtılmadığına bağlıdır. Anahtar kasalar gibi bazı kaynakların, kullanıcılar, uygulamalar ve gruplar gibi ilkelere erişim izni vermek için kullandığınız kendi [erişim ilkeleri](https://docs.microsoft.com/azure/key-vault/secure-your-key-vault#data-plane-and-access-policies) de vardır.

### <a name="common-issues-when-deployed-to-azure-app-service"></a>Azure Uygulama Hizmetine dağıtıldığında sık karşılaşılan sorunlar

#### <a name="managed-identity-isnt-set-up-on-the-app-service"></a>Yönetilen kimlik, Uygulama Hizmeti'nde ayarlı değil

MSI_ENDPOINT ortam değişkenlerini kontrol edin ve [MSI_SECRET Kudu hata ayıklama konsolu](https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/)kullanarak var. Bu ortam değişkenleri yoksa, Yönetilen Kimlik Uygulama Hizmeti'nde etkinleştirilmez.

### <a name="common-issues-when-deployed-locally-with-iis"></a>IIS ile yerel olarak dağıtıldığında sık karşılaşılan sorunlar

#### <a name="cant-retrieve-tokens-when-debugging-app-in-iis"></a>IIS'de hata ayıklama uygulaması nda belirteçleri alamıyorum

Varsayılan olarak, AppAuth IIS'de farklı bir kullanıcı bağlamında çalışır. Bu nedenle, erişim belirteçlerini almak için geliştirici kimliğinizi kullanma erişimi yoktur. IIS'yi kullanıcı bağlamınızla çalışacak şekilde aşağıdaki iki adımda yapılandırabilirsiniz:
- Geçerli kullanıcı hesabınız olarak çalışacak web uygulaması için Uygulama Havuzunu yapılandırın. Daha fazla bilgiyi [burada](https://docs.microsoft.com/iis/manage/configuring-security/application-pool-identities#configuring-iis-application-pool-identities) bulabilirsiniz
- "setProfileEnvironment"ı "True" olarak yapılandırın. [Burada](https://docs.microsoft.com/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)daha fazla bilgi bakın. 

    - %windir%\System32\inetsrv\config\applicationHost.config'e git
    - "setProfileEnvironment" araması yapın. "False" olarak ayarlanmışsa, "True" olarak değiştirin. Yoksa, processModel öğesine bir öznitelik olarak ekleyin (/configuration/system.applicationHost/applicationPools/applicationPoolDefaults/processModel/@setProfileEnvironment), ve "True" olarak ayarlayın.

- [Azure kaynakları için yönetilen kimlikler](../../active-directory/managed-identities-azure-resources/index.yml)hakkında daha fazla bilgi edinin.
- [Azure AD kimlik doğrulama senaryoları](../../active-directory/develop/active-directory-authentication-scenarios.md)hakkında daha fazla bilgi edinin.
