---
author: erhopf
ms.author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 07/23/2019
ms.openlocfilehash: 8754504655cdd08c9bf9f89311cb6c5d1057f0e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78262301"
---
## <a name="authenticate-with-azure-active-directory"></a>Azure Active Directory ile kimlik doğrulama

> [!IMPORTANT]
> 1. Şu anda **yalnızca** Computer Vision API, Face API, Text Analytics API, Immersive Reader, Form Recognizer, Anomaly Detector ve Azure Active Directory (AAD) kullanarak Bing Özel Arama desteği dışındaki tüm Bing hizmetleri kimlik doğrulaması.
> 2. AAD kimlik doğrulaması, Azure kaynağınızın özel alt etki alanı adı ile her zaman birlikte kullanılmalıdır. [Bölgesel uç noktalar](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains#is-there-a-list-of-regional-endpoints) AAD kimlik doğrulamasını desteklemez.

Önceki bölümlerde, tek hizmetli veya çok hizmetli abonelik anahtarı kullanarak Azure Bilişsel Hizmetleri'ne karşı nasıl kimlik doğrulaması yapacağınızı gösterdik. Bu anahtarlar geliştirmeye başlamak için hızlı ve kolay bir yol sağlarken, rol tabanlı erişim denetimleri gerektiren daha karmaşık senaryolarda yetersiz kalır. Azure Active Directory (AAD) kullanarak kimlik doğrulaması için gerekenlere bir göz atalım.

Aşağıdaki bölümlerde, bir alt etki alanı oluşturmak, roller atamak ve Azure Bilişsel Hizmetleri'ni aramak için bir taşıyıcı belirteci elde etmek için Azure Bulut Bulut Su alanı ortamını veya Azure CLI'yi kullanırsınız. Sıkışıp kalırsanız, her bölümde, Azure Bulut Uyp/Azure CLI'deki her komut için kullanılabilir tüm seçeneklerin bulunduğu bağlantılar sağlanır.

### <a name="create-a-resource-with-a-custom-subdomain"></a>Özel bir alt etki alanı olan bir kaynak oluşturma

İlk adım, özel bir alt etki alanı oluşturmaktır. Özel alt alan adı olmayan varolan bir Bilişsel Hizmetler kaynağını kullanmak istiyorsanız, kaynağınız için özel alt etki alanını etkinleştirmek için [Bilişsel Hizmetler Özel Alt Etki Alanları'ndaki](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains#how-does-this-impact-existing-resources) yönergeleri izleyin.

1. Azure Bulut Kabuğu'nu açarak başlayın. Sonra [bir abonelik seçin:](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext?view=azps-3.3.0)

   ```powershell-interactive
   Set-AzContext -SubscriptionName <SubscriptionName>
   ```

2. Ardından, özel bir alt etki alanına sahip [bir Bilişsel Hizmetler kaynağı oluşturun.](https://docs.microsoft.com/powershell/module/az.cognitiveservices/new-azcognitiveservicesaccount?view=azps-1.8.0) Alt alan adının genel olarak benzersiz olması gerekir ve ".",,!, ","".

   ```powershell-interactive
   New-AzCognitiveServicesAccount -ResourceGroupName <RESOURCE_GROUP_NAME> -name <ACCOUNT_NAME> -Type <ACCOUNT_TYPE> -SkuName <SUBSCRIPTION_TYPE> -Location <REGION> -CustomSubdomainName <UNIQUE_SUBDOMAIN>
   ```

3. Başarılı olursa, **Endpoint** kaynağınıza özgü alt etki alanı adını göstermelidir.


### <a name="assign-a-role-to-a-service-principal"></a>Bir hizmet müdürüne rol atama

Kaynağınızla ilişkili özel bir alt etki alanınız olduğuna göre, bir hizmet ilkesine bir rol atamanız gerekir.

> [!NOTE]
> AAD rol atamalarının yayılması beş dakika kadar sürebilir.

1. İlk olarak, bir [AAD uygulaması](https://docs.microsoft.com/powershell/module/Az.Resources/New-AzADApplication?view=azps-1.8.0)kaydedelim.

   ```powershell-interactive
   $SecureStringPassword = ConvertTo-SecureString -String <YOUR_PASSWORD> -AsPlainText -Force

   New-AzADApplication -DisplayName <APP_DISPLAY_NAME> -IdentifierUris <APP_URIS> -Password $SecureStringPassword
   ```

   Bir sonraki adımda **ApplicationId'e** ihtiyacınız olacak.

2. Ardından, AAD uygulaması için [bir hizmet ilkesi oluşturmanız](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal?view=azps-1.8.0) gerekir.

   ```powershell-interactive
   New-AzADServicePrincipal -ApplicationId <APPLICATION_ID>
   ```

   >[!NOTE]
   > Azure portalına bir uygulama kaydettirseniz, bu adım sizin için tamamlanır.

3. Son adım, ["Bilişsel Hizmetler Kullanıcısı" rolünü](https://docs.microsoft.com/powershell/module/az.Resources/New-azRoleAssignment?view=azps-1.8.0) hizmet ilkesine (kaynağa kapsamlı) atamaktır. Bir rol atayarak, bu kaynağa hizmet temel erişimi vermiş olursunuz. Aboneliğinizdeki birden çok kaynağa aynı hizmet ana erişimini verebilirsiniz.
   >[!NOTE]
   > Hizmet sorumlusunun ObjectId'si kullanılır, uygulama için ObjectId değil.
   > ACCOUNT_ID, oluşturduğunuz Bilişsel Hizmetler hesabının Azure kaynak Kimliği olacaktır. Azure portalında kaynağın "özellikleri"nden Azure kaynak Kimliği'ni bulabilirsiniz.

   ```azurecli-interactive
   New-AzRoleAssignment -ObjectId <SERVICE_PRINCIPAL_OBJECTID> -Scope <ACCOUNT_ID> -RoleDefinitionName "Cognitive Services User"
   ```

### <a name="sample-request"></a>Örnek istek

Bu örnekte, hizmet sorumlusunun kimliğini doğrulamak için bir parola kullanılır. Sağlanan belirteç daha sonra Bilgisayarlı Görme API'sını aramak için kullanılır.

1. **KiracıKimliğinizi**Alın:
   ```powershell-interactive
   $context=Get-AzContext
   $context.Tenant.Id
   ```

2. Bir belirteç alın:
   ```powershell-interactive
   $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList "https://login.windows.net/<TENANT_ID>"
   $secureSecretObject = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.SecureClientSecret" -ArgumentList $SecureStringPassword   
   $clientCredential = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential" -ArgumentList $app.ApplicationId, $secureSecretObject
   $token=$authContext.AcquireTokenAsync("https://cognitiveservices.azure.com/", $clientCredential).Result
   $token
   ```
3. Bilgisayarlı Vizyon API'sini arayın:
   ```powershell-interactive
   $url = $account.Endpoint+"vision/v1.0/models"
   $result = Invoke-RestMethod -Uri $url  -Method Get -Headers @{"Authorization"=$token.CreateAuthorizationHeader()} -Verbose
   $result | ConvertTo-Json
   ```

Alternatif olarak, hizmet sorumlusu nun bir sertifikayla kimlik doğrulaması yapılabilir. Hizmet sorumlusunun yanı sıra, kullanıcı ilkesi de izinlerin başka bir AAD uygulaması aracılığıyla devredilmesiyle desteklenir. Bu durumda, parolalar veya sertifikalar yerine, kullanıcılardan belirteç alırken iki faktörlü kimlik doğrulaması istenir.
