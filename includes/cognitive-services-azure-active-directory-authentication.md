---
author: erhopf
ms.author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 05/11/2020
ms.openlocfilehash: 8948850734b310d70e7e457f075d13ade0c55619
ms.sourcegitcommit: 32592ba24c93aa9249f9bd1193ff157235f66d7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85805427"
---
## <a name="authenticate-with-azure-active-directory"></a>Azure Active Directory ile kimlik doğrulama

> [!IMPORTANT]
> 1. Şu anda **yalnızca** Görüntü İşleme API'si, Yüz Tanıma API'si, metin Analizi API'si, modern okuyucu, form tanıyıcı, anomali algılayıcısı ve Bing Özel Arama, Azure ACTIVE DIRECTORY (AAD) kullanarak kimlik doğrulaması desteği hariç tüm Bing hizmetleri.
> 2. AAD kimlik doğrulamasının her zaman Azure kaynağınızın özel alt etki alanı adıyla birlikte kullanılması gerekir. [Bölgesel uç noktalar](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains#is-there-a-list-of-regional-endpoints) AAD kimlik doğrulamasını desteklemez.

Önceki bölümlerde, tek hizmet veya çok sunuculu bir abonelik anahtarı kullanarak Azure bilişsel hizmetler 'de nasıl kimlik doğrulaması yapılacağını gösterdik. Bu anahtarlar geliştirme başlatmaya başlamak için hızlı ve kolay bir yol sağlarken, rol tabanlı erişim denetimleri gerektiren daha karmaşık senaryolara kısa süreli olarak düşmektedir. Azure Active Directory (AAD) kullanarak kimlik doğrulaması yapmak için gereklere göz atalım.

Aşağıdaki bölümlerde, bir alt etki alanı oluşturmak, roller atamak ve Azure bilişsel hizmetler 'i çağırmak için bir taşıyıcı belirteci almak için Azure Cloud Shell ortamını veya Azure CLı 'yi kullanacaksınız. Takıldıysanız, her bölümde Azure Cloud Shell/Azure CLı 'daki her komut için kullanılabilen tüm seçeneklerle bağlantılar sağlanır.

### <a name="create-a-resource-with-a-custom-subdomain"></a>Özel alt etki alanı ile kaynak oluşturma

İlk adım özel bir alt etki alanı oluşturmaktır. Özel alt etki alanı adı olmayan mevcut bir bilişsel hizmetler kaynağını kullanmak istiyorsanız, kaynağınız için özel alt etki alanını etkinleştirmek üzere bilişsel [Hizmetler özel alt etki alanları](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains#how-does-this-impact-existing-resources) ' nda yer alan yönergeleri izleyin.

1. Azure Cloud Shell açarak başlayın. Ardından [bir abonelik seçin](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext?view=azps-3.3.0):

   ```powershell-interactive
   Set-AzContext -SubscriptionName <SubscriptionName>
   ```

2. Ardından, özel bir alt etki alanı ile bilişsel [Hizmetler kaynağı oluşturun](https://docs.microsoft.com/powershell/module/az.cognitiveservices/new-azcognitiveservicesaccount?view=azps-1.8.0) . Alt etki alanı adının genel olarak benzersiz olması gerekir ve örneğin: ".", "!", "," gibi özel karakterler içermemelidir.

   ```powershell-interactive
   $account = New-AzCognitiveServicesAccount -ResourceGroupName <RESOURCE_GROUP_NAME> -name <ACCOUNT_NAME> -Type <ACCOUNT_TYPE> -SkuName <SUBSCRIPTION_TYPE> -Location <REGION> -CustomSubdomainName <UNIQUE_SUBDOMAIN>
   ```

3. Başarılı olursa, **uç nokta** , kaynağınız için benzersiz olan alt etki alanı adını göstermelidir.


### <a name="assign-a-role-to-a-service-principal"></a>Hizmet sorumlusuna rol atama

Artık kaynakla ilişkili özel bir alt etki alanı olduğuna göre, bir hizmet sorumlusu için bir rol atamanız gerekir.

> [!NOTE]
> AAD rol atamalarının yaymanın beş dakika sürebileceğini aklınızda bulundurun.

1. İlk olarak, bir [AAD uygulaması](https://docs.microsoft.com/powershell/module/Az.Resources/New-AzADApplication?view=azps-1.8.0)kaydedelim.

   ```powershell-interactive
   $SecureStringPassword = ConvertTo-SecureString -String <YOUR_PASSWORD> -AsPlainText -Force

   $app = New-AzADApplication -DisplayName <APP_DISPLAY_NAME> -IdentifierUris <APP_URIS> -Password $SecureStringPassword
   ```

   Bir sonraki adımda **ApplicationId** 'ye ihtiyacınız olacak.

2. Ardından, AAD uygulaması için [bir hizmet sorumlusu oluşturmanız](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal?view=azps-1.8.0) gerekir.

   ```powershell-interactive
   New-AzADServicePrincipal -ApplicationId <APPLICATION_ID>
   ```

   >[!NOTE]
   > Azure portal bir uygulamayı kaydettiğinizde, bu adım sizin için tamamlanır.

3. Son adım, "bilişsel [Hizmetler kullanıcısı" rolünün](https://docs.microsoft.com/powershell/module/az.Resources/New-azRoleAssignment?view=azps-1.8.0) hizmet sorumlusuna atanması (kaynağa kapsamlı olarak). Rol atayarak, bu kaynağa hizmet sorumlusu erişimi vermiş olursunuz. Aboneliğinizdeki birden fazla kaynağa aynı hizmet sorumlusu erişimi verebilirsiniz.
   >[!NOTE]
   > Hizmet sorumlusu objectID, uygulamanın ObjectID değil kullanılır.
   > ACCOUNT_ID, oluşturduğunuz bilişsel hizmetler hesabının Azure Kaynak Kimliği olacaktır. Azure Kaynak Kimliği ' ni kaynağın "Özellikler" bölümünde bulabilirsiniz Azure portal.

   ```azurecli-interactive
   New-AzRoleAssignment -ObjectId <SERVICE_PRINCIPAL_OBJECTID> -Scope <ACCOUNT_ID> -RoleDefinitionName "Cognitive Services User"
   ```

### <a name="sample-request"></a>Örnek istek

Bu örnekte, hizmet sorumlusunun kimliğini doğrulamak için bir parola kullanılır. Belirtilen belirteç daha sonra Görüntü İşleme API'si çağırmak için kullanılır.

1. **Tenantıd**'nizi alın:
   ```powershell-interactive
   $context=Get-AzContext
   $context.Tenant.Id
   ```

2. Belirteç al:
   > [!NOTE]
   > Azure Cloud Shell kullanıyorsanız, `SecureClientSecret` sınıfı kullanılabilir değildir. 

   #### <a name="poweershell"></a>[PoweerShell](#tab/powershell)
   ```powershell-interactive
   $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList "https://login.windows.net/<TENANT_ID>"
   $secureSecretObject = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.SecureClientSecret" -ArgumentList $SecureStringPassword   
   $clientCredential = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential" -ArgumentList $app.ApplicationId, $secureSecretObject
   $token=$authContext.AcquireTokenAsync("https://cognitiveservices.azure.com/", $clientCredential).Result
   $token
   ```
   
   #### <a name="azure-cloud-shell"></a>[Azure Cloud Shell](#tab/azure-cloud-shell)
   ```Azure Cloud Shell
   $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList "https://login.windows.net/<TENANT_ID>"
   $clientCredential = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential" -ArgumentList $app.ApplicationId, <YOUR_PASSWORD>
   $token=$authContext.AcquireTokenAsync("https://cognitiveservices.azure.com/", $clientCredential).Result
   $token
   ``` 

   ---

3. Görüntü İşleme API'si çağırın:
   ```powershell-interactive
   $url = $account.Endpoint+"vision/v1.0/models"
   $result = Invoke-RestMethod -Uri $url  -Method Get -Headers @{"Authorization"=$token.CreateAuthorizationHeader()} -Verbose
   $result | ConvertTo-Json
   ```

Alternatif olarak, hizmet sorumlusu bir sertifikayla kimlik doğrulaması yapabilir. Hizmet sorumlusunun yanı sıra, Kullanıcı sorumlusu da başka bir AAD uygulaması aracılığıyla temsilci atanmış izinler sunarak desteklenir. Bu durumda, parola veya sertifika yerine, belirteç alırken kullanıcılardan iki öğeli kimlik doğrulaması istenir.

## <a name="authorize-access-to-managed-identities"></a>Yönetilen kimliklere erişim yetkisi verme
 
Bilişsel hizmetler, [Azure kaynakları için yönetilen kimliklerle](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)Azure Active Directory (Azure AD) kimlik doğrulamasını destekler. Azure kaynakları için Yönetilen kimlikler, Azure sanal makinelerinde (VM), işlev uygulamalarında, sanal makine ölçek kümelerinde ve diğer hizmetlerde çalışan uygulamalardan Azure AD kimlik bilgilerini kullanarak bilişsel hizmetler kaynaklarına erişim yetkisi verebilir. Azure AD kimlik doğrulamasıyla birlikte Azure kaynakları için Yönetilen kimlikler kullanarak, kimlik bilgilerini bulutta çalışan uygulamalarınızla depolamaktan kaçınabilirsiniz.  

### <a name="enable-managed-identities-on-a-vm"></a>VM 'de yönetilen kimlikleri etkinleştirme

VM 'nizden bilişsel hizmetler kaynaklarına erişim yetkisi vermek üzere Azure kaynakları için Yönetilen kimlikler kullanabilmeniz için, VM 'de Azure kaynakları için yönetilen kimlikleri etkinleştirmeniz gerekir. Azure kaynakları için yönetilen kimliklerin nasıl etkinleştirileceği hakkında bilgi edinmek için bkz.:

- [Azure portalındaki](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm)
- [Azure CLI](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm)
- [Azure Resource Manager şablonu](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm)
- [Azure Resource Manager istemci kitaplıkları](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm)

Yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları Için Yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).
