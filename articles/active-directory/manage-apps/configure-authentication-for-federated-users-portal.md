---
title: Home Realm Discovery'yi kullanarak oturum açma otomatik ivmeyi yapılandırma
description: Otomatik hızlandırma ve etki alanı ipuçları da dahil olmak üzere federe kullanıcılar için Azure Etkin Dizin kimlik doğrulaması için Home Realm Discovery ilkesini nasıl yapılandıracaklarını öğrenin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mimart
ms.custom: seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 60bfc964ffc394b3f79c9d279158003f383b7331
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78943439"
---
# <a name="configure-azure-active-directory-sign-in-behavior-for-an-application-by-using-a-home-realm-discovery-policy"></a>Bir Uygulama için Bir Giriş Alanı Bulma ilkesini kullanarak Azure Etkin Dizin oturum açma davranışını yapılandırma

Bu makalede, federe kullanıcılar için Azure Active Directory kimlik doğrulama davranışını yapılandırmak için bir giriş sağlar. Federe etki alanlarındaki kullanıcılar için otomatik hızlandırma ve kimlik doğrulama kısıtlamaları yapılandırmayı kapsar.

## <a name="home-realm-discovery"></a>Giriş Bölgesi Bulma
Home Realm Discovery (HRD), Azure Etkin Dizininin (Azure AD) bir kullanıcının oturum açma zamanında nerede kimlik doğrulaması yapması gerektiğini belirlemesine olanak tanıyan işlemdir.  Bir kullanıcı bir kaynağa erişmek için bir Azure AD kiracısına veya Azure AD ortak oturum açma sayfasına oturum açtıklarında bir kullanıcı adı (UPN) yazar. Azure AD, kullanıcının oturum açması gereken yeri bulmak için bunu kullanır. 

Kullanıcının kimlik doğrulaması için aşağıdaki konumlardan birine götürülmek gerekebilir:

- Kullanıcının ev kiracısı (kullanıcının erişmeye çalıştığı kaynakla aynı kiracı olabilir). 

- Microsoft hesabı.  Kullanıcı kaynak kiracı bir konuk.

-  Active Directory Federation Services (AD FS) gibi şirket içi kimlik sağlayıcısı.

- Azure AD kiracısıyla beslenen başka bir kimlik sağlayıcısı.

## <a name="auto-acceleration"></a>Otomatik ivmelenme 
Bazı kuruluşlar, Azure Etkin Dizin kiracılarındaki etki alanlarını, kullanıcı kimlik doğrulaması için AD FS gibi başka bir IdP ile federate olacak şekilde yapılandırır.  

Bir kullanıcı bir uygulamaya oturum ettiğinde, ilk olarak bir Azure AD oturum açma sayfası yla birlikte sunulur. UPN'lerini yazdıktan sonra, federe bir etki alanındaysalar, o etki alanına hizmet veren IdP'nin oturum açma sayfasına alınırlar. Belirli koşullar altında, yöneticiler kullanıcıları belirli uygulamalarda oturum açken oturum açma sayfasına yönlendirmek isteyebilir. 

Sonuç olarak, kullanıcılar ilk Azure Etkin Dizin sayfasını atlayabilir. Bu işleme "oturum açma otomatik ivme" adı verilir.

Kiracının oturum açma için başka bir IdP'ye federe olduğu durumlarda, otomatik hızlanma kullanıcıoturum açmayı daha kolay hale getirir.  Tek tek uygulamalar için otomatik ivmeyi yapılandırabilirsiniz.

>[!NOTE]
>Otomatik hızlandırma için bir uygulama yapılandırırsanız, konuk kullanıcılar oturum açamaz. Bir kullanıcıyı kimlik doğrulaması için doğrudan federe bir IdP'ye götürürseniz, azure Active Directory oturum açma sayfasına geri dönmeleri için hiçbir yol yoktur. Diğer kiracılara veya Microsoft hesabı gibi harici bir IdP'ye yönlendirilmesi gereken konuk kullanıcılar, Home Realm Discovery adımını atladıkları için bu uygulamada oturum açamaz.  

Federe IdP'ye otomatik ivmelenmeyi denetlemenin iki yolu vardır:   

- Bir uygulama için kimlik doğrulama istekleri yle ilgili etki alanı ipucunu kullanın. 
- Otomatik ivmeyi etkinleştirmek için Bir Home Realm Discovery ilkesini yapılandırın.

### <a name="domain-hints"></a>Etki alanı ipuçları    
Etki alanı ipuçları, bir uygulamadan gelen kimlik doğrulama isteğine dahil edilen yönergelerdir. Kullanıcıyı federe IdP oturum açma sayfasına hızlandırmak için kullanılabilirler. Veya çok kiracılı bir uygulama tarafından, kullanıcıyı doğrudan kiracıları için markalı Azure AD oturum açma sayfasına hızlandırmak için kullanılabilirler.  

Örneğin, "largeapp.com" uygulaması, müşterilerinin uygulamaya özel bir URL olan "contoso.largeapp.com" olarak erişmesini sağlayabilir. Uygulama, kimlik doğrulama isteğine contoso.com için bir etki alanı ipucu da içerebilir. 

Etki alanı ipucu sözdizimi, kullanılan protokole bağlı olarak değişir ve genellikle uygulamada yapılandırılır.

**WS-Federation**: whr=contoso.com sorgu dizesi içinde.

**SAML**: Etki alanı ipucu veya sorgu dizesi whr=contoso.com içeren bir SAML kimlik doğrulama isteği.

**Open ID Connect**: sorgu dizesi domain_hint=contoso.com. 

Uygulamadan gelen kimlik doğrulama isteğine bir etki alanı ipucu dahil edilirse ve kiracı bu etki alanıyla beslenirse, Azure AD oturum açma yı bu etki alanı için yapılandırılan IdP'ye yönlendirmeye çalışır. 

Etki alanı ipucu doğrulanmış federe etki alanına atıfta bulunmuyorsa, bu yoksayılır ve normal Home Realm Discovery çağrılır.

Azure Active Directory tarafından desteklenen etki alanı ipuçlarını kullanarak otomatik hızlandırma hakkında daha fazla bilgi için [Kurumsal Mobilite + Güvenlik bloguna](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/11/using-azure-ad-to-land-users-on-their-custom-login-page-from-within-your-app/)bakın.

>[!NOTE]
>Bir etki alanı ipucu kimlik doğrulama isteğine dahil edilirse, varlığı HRD ilkesinde uygulama için ayarlanan otomatik ivmeyi geçersiz kılar.

### <a name="home-realm-discovery-policy-for-auto-acceleration"></a>Otomatik hızlandırma için Home Realm Discovery ilkesi
Bazı uygulamalar, yadıkları kimlik doğrulama isteğini yapılandırmak için bir yol sağlamaz. Bu gibi durumlarda, otomatik ivmeyi denetlemek için etki alanı ipuçlarını kullanmak mümkün değildir. Otomatik ivme, aynı davranışı elde etmek için ilke yoluyla yapılandırılabilir.  

## <a name="enable-direct-authentication-for-legacy-applications"></a>Eski uygulamalar için doğrudan kimlik doğrulamayı etkinleştirme
En iyi yöntem, uygulamaların kullanıcıları doğrulamak için AAD kitaplıklarını ve etkileşimli oturum açmayı kullanmasıdır. Kütüphaneler federe kullanıcı akışları ilgilenir.  Bazen eski uygulamalar federasyonu anlamak için yazılmaz. Onlar ev bölge bulma gerçekleştirmek ve bir kullanıcı doğrulamak için doğru federe bitiş noktası ile etkileşim yok. İsterseniz, doğrudan Azure Active Directory ile kimlik doğrulaması yapmak için kullanıcı adı/parola kimlik bilgilerini gönderen belirli eski uygulamaları etkinleştirmek için HRD İlkesi'ni kullanabilirsiniz. Parola Karma Eşitleme etkin olmalıdır. 

> [!IMPORTANT]
> Yalnızca Parola Karma Eşitlemesini açtıysanız ve şirket içi IdP'niz tarafından uygulanan herhangi bir ilke olmadan bu uygulamanın kimliğini doğrulamanın sorun olmadığını biliyorsanız doğrudan kimlik doğrulamasını etkinleştirin. Parola Karma Eşitlemesini kapatırsanız veya herhangi bir nedenle AD Connect ile Dizin Eşitlemesini kapatırsanız, eski bir parola karma kullanarak doğrudan kimlik doğrulama olasılığını önlemek için bu ilkeyi kaldırmanız gerekir.

## <a name="set-hrd-policy"></a>HRD ilkesini ayarlama
Federe oturum açma otomatik hızlandırma veya doğrudan bulut tabanlı uygulamalar için bir uygulamada HRD ilkesini ayarlamak için üç adım vardır:

1. Bir HRD ilkesi oluşturun.

2. İlke eklemek için hizmet ilkesini bulun.

3. İlkeyi hizmet ilkesine iliştirin. 

İlkeler yalnızca bir hizmet ilkesine iliştirildiğinde belirli bir uygulama için etkili olur. 

Yalnızca bir HRD ilkesi herhangi bir anda bir hizmet sorumlusunda etkin olabilir.  

HRD ilkesini oluşturmak ve yönetmek için Azure Active Directory PowerShell cmdlets'i kullanabilirsiniz.

Aşağıda örnek bir HRD ilke tanımı verilmiştir:
    
 ```
   {  
    "HomeRealmDiscoveryPolicy":
    {  
    "AccelerateToFederatedDomain":true,
    "PreferredDomain":"federated.example.edu",
    "AllowCloudPasswordValidation":true
    }
   }
```

İlke türü "HomeRealmDiscoveryPolicy."

**AccelerateToFederatedDomain** isteğe bağlıdır. **AccelerateToFederatedDomain** yanlışsa, politikanın otomatik ivmelenme üzerinde hiçbir etkisi yoktur. **AccelerateToFederatedDomain** doğruysa ve kiracıda yalnızca bir doğrulanmış ve federe etki alanı varsa, kullanıcılar oturum açmaları için doğrudan federe IdP'ye götürülür. Doğruysa ve kiracıda birden fazla doğrulanmış etki alanı varsa, **PreferredDomain** belirtilmelidir.

**PreferredDomain** isteğe bağlıdır. **PreferredDomain** hızlandırmak için bir etki alanı belirtmelidir. Kiracının yalnızca bir federe etki alanı varsa atlanabilir.  Atlanırsa ve birden fazla doğrulanmış federe etki alanı varsa, ilkenin hiçbir etkisi yoktur.

 **PreferredDomain** belirtilirse, kiracı için doğrulanmış, federe etki alanı yla eşleşmelidir. Uygulamanın tüm kullanıcıları bu etki alanında oturum açabilmeli.

**AllowCloudPasswordValidation** isteğe bağlıdır. **İzin VermeCloudPasswordValidation** doğruysa, uygulama nın kullanıcı adı/parola kimlik bilgilerini doğrudan Azure Active Directory belirteci bitiş noktasına sunarak federe bir kullanıcının kimliğini doğrulamasına izin verilir. Bu yalnızca Parola Hash Eşitlemi etkinse çalışır.

### <a name="priority-and-evaluation-of-hrd-policies"></a>HRD politikalarının önceliği ve değerlendirilmesi
HRD ilkeleri oluşturulabilir ve daha sonra belirli kuruluşlara ve hizmet ilkelerine atanabilir. Bu, birden çok ilkenin belirli bir uygulamaya uygulanmasının mümkün olduğu anlamına gelir. Geçerli olan HRD ilkesi aşağıdaki kuralları izler:


- Kimlik doğrulama isteğinde bir etki alanı ipucu varsa, otomatik hızlandırma için herhangi bir HRD ilkesi yoksayılır. Etki alanı ipucu tarafından belirtilen davranış kullanılır.

- Aksi takdirde, bir ilke açıkça hizmet ilkesine atanmışsa, bu uygulanır. 

- Etki alanı ipucu yoksa ve hizmet ilkesine açıkça atanmamışsa, hizmet sorumlusunun üst kuruluşuna açıkça atanan bir ilke zorlanır. 

- Etki alanı ipucu yoksa ve hizmet sorumlusuna veya kuruluşa ilke atanmamışsa, varsayılan HRD davranışı kullanılır.

## <a name="tutorial-for-setting-hrd-policy-on-an-application"></a>Bir uygulamada HRD ilkesini ayarlamak için öğretici 
Azure AD PowerShell cmdlets'i kullanarak şunları dahil olmak üzere birkaç senaryoyu gözden geçiririz:


- Tek bir federe etki alanına sahip bir kiracıda bir uygulama için otomatik hızlandırma yapmak için HRD ilkesi ayarlama.

- Kiracınız için doğrulanan birkaç etki alanından birine bir uygulama için otomatik hızlandırma yapmak üzere HRD ilkesiayarlama.

- Eski bir uygulamanın federe bir kullanıcı için Azure Active Directory'ye doğrudan kullanıcı adı/parola kimlik doğrulaması yapmasını sağlamak için HRD ilkesini ayarlama.

- İlkenin yapılandırıldığı uygulamaları listeleme.


### <a name="prerequisites"></a>Ön koşullar
Aşağıdaki örneklerde, Azure AD'deki uygulama hizmeti ilkelerine ilişkin ilkeleri oluşturur, günceller, bağlantı verir ve silebilirsiniz.

1.  Başlamak için en son Azure AD PowerShell cmdlet önizlemesini indirin. 

2.  Azure AD PowerShell cmdlets'i indirdikten sonra, yönetici hesabınızla Azure AD'de oturum açabilmek için Bağlan komutunu çalıştırın:

    ``` powershell
    Connect-AzureAD -Confirm
    ```
3.  Kuruluşunuzdaki tüm ilkeleri görmek için aşağıdaki komutu çalıştırın:

    ``` powershell
    Get-AzureADPolicy
    ```

Hiçbir şey döndürülmezse, kiracınızda oluşturulan ilkeler yok demektir.

### <a name="example-set-hrd-policy-for-an-application"></a>Örnek: Bir uygulama için HRD ilkesini ayarlama 

Bu örnekte, bir uygulamaya atandığında aşağıdakileri içeren bir ilke oluşturursunuz: 
- Kiracınızda tek bir etki alanı olduğunda, kullanıcıları bir uygulamada oturum açken AD FS oturum açma ekranına otomatik olarak hızlandırır. 
- Kullanıcıları AD FS oturum açma ekranına otomatik olarak hızlandırır ve kiracınızda birden fazla federe etki alanı vardır.
- Etkileşimli olmayan kullanıcı adı/parola oturum açmanızı, ilkenin atandığı uygulamalar için federe kullanıcılar için doğrudan Azure Active Directory'de oturum açmanızı sağlar.

#### <a name="step-1-create-an-hrd-policy"></a>Adım 1: HRD ilkesi oluşturma

Aşağıdaki ilke, kullanıcıları kiracınızda tek bir etki alanı olduğunda bir uygulamada oturum açken AD FS oturum açma ekranına otomatik olarak hızlandırır.

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true}}") -DisplayName BasicAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```
Aşağıdaki ilke, kullanıcıları bir AD FS oturum açma ekranına otomatik olarak hızlandırır ve kiracınızda birden fazla federe etki alanı vardır. Kullanıcılara uygulamalar için kimlik doğrulayan birden fazla federe etki alanınız varsa, otomatik hızlandırmak için etki alanını belirtmeniz gerekir.

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true, `"PreferredDomain`":`"federated.example.edu`"}}") -DisplayName MultiDomainAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```

Belirli uygulamalar için doğrudan Azure Active Directory ile federe kullanıcılar için kullanıcı adı/parola kimlik doğrulaması etkinleştirmek için bir ilke oluşturmak için aşağıdaki komutu çalıştırın:

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AllowCloudPasswordValidation`":true}}") -DisplayName EnableDirectAuthPolicy -Type HomeRealmDiscoveryPolicy
```


Yeni ilkenizi görmek ve **ObjectID'sini**almak için aşağıdaki komutu çalıştırın:

``` powershell
Get-AzureADPolicy
```


OLUŞTURDUKTAN sonra HRD ilkesini uygulamak için, birden çok uygulama hizmeti ilkesine atayabilirsiniz.

#### <a name="step-2-locate-the-service-principal-to-which-to-assign-the-policy"></a>Adım 2: İlke atamak için hizmet sorumlusu bulun  
İlke atamak istediğiniz hizmet ilkelerinin **ObjectID'sine** ihtiyacınız vardır. Hizmet ilkelerinin **ObjectID'sini** bulmanın birkaç yolu vardır.    

Portalı kullanabilir veya [Microsoft Graph'ı](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta)sorgulayabilirsiniz. Ayrıca, kuruluşunuzun tüm hizmet ilkelerini görmek için [Grafik Gezgini Aracı'na](https://developer.microsoft.com/graph/graph-explorer) gidebilir ve Azure REKLAM hesabınızda oturum açabilirsiniz. 

PowerShell kullandığınızdan, servis ilkelerini ve kimliklerini listelemek için aşağıdaki cmdlet'i kullanabilirsiniz.

``` powershell
Get-AzureADServicePrincipal
```

#### <a name="step-3-assign-the-policy-to-your-service-principal"></a>Adım 3: İlkeyi hizmet müdürünüze atama  
Otomatik ivmeyi yapılandırmak istediğiniz uygulamanın hizmet ilkesinin **ObjectID'sini** aldıktan sonra aşağıdaki komutu çalıştırın. Bu komut, adım 1'de oluşturduğunuz HRD ilkesini adım 2'de bulunan hizmet ilkesiyle ilişkilendirer.

``` powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
```

Bu komutu, ilke eklemek istediğiniz her hizmet sorumlusu için yineleyebilirsiniz.

Bir uygulamanın zaten atanmış bir HomeRealmDiscovery ilkesi varsa, ikinci bir tane ekemezsin.  Bu durumda, ek parametreler eklemek için uygulamaya atanan Home Realm Discovery ilkesinin tanımını değiştirin.

#### <a name="step-4-check-which-application-service-principals-your-hrd-policy-is-assigned-to"></a>Adım 4: HRD politikanızın hangi uygulama hizmeti ilkelerine atandığını denetleyin
Hangi uygulamaların HRD ilkesini yapılandırdığından yararlanmak için **Get-AzureADPolicyAppliedObject** cmdlet'i kullanın. Denetlemek istediğiniz ilkenin **ObjectID'sini** geçirin.

``` powershell
Get-AzureADPolicyAppliedObject -id <ObjectId of the Policy>
```
#### <a name="step-5-youre-done"></a>Adım 5: Bitti!
Yeni ilkenin çalışıp çalışmadığını denetlemek için uygulamayı deneyin.

### <a name="example-list-the-applications-for-which-hrd-policy-is-configured"></a>Örnek: HRD ilkesinin yapılandırıldığı uygulamaları listele

#### <a name="step-1-list-all-policies-that-were-created-in-your-organization"></a>Adım 1: Kuruluşunuzda oluşturulan tüm ilkeleri listele 

``` powershell
Get-AzureADPolicy
```

Atamaları listelemek istediğiniz ilkenin **ObjectID'ine** dikkat edin.

#### <a name="step-2-list-the-service-principals-to-which-the-policy-is-assigned"></a>Adım 2: İlkenin atandığı hizmet ilkelerini listele  

``` powershell
Get-AzureADPolicyAppliedObject -id <ObjectId of the Policy>
```

### <a name="example-remove-an-hrd-policy-for-an-application"></a>Örnek: Uygulama için HRD ilkesini kaldırma
#### <a name="step-1-get-the-objectid"></a>Adım 1: ObjectID'yi alın
İlkenin **ObjectID'sini** ve kaldırmak istediğiniz uygulama hizmeti sorumlusunun kini almak için önceki örneği kullanın. 

#### <a name="step-2-remove-the-policy-assignment-from-the-application-service-principal"></a>Adım 2: İlke atamasını uygulama hizmeti ilkesinden kaldırma  

``` powershell
Remove-AzureADServicePrincipalPolicy -id <ObjectId of the Service Principal>  -PolicyId <ObjectId of the policy>
```

#### <a name="step-3-check-removal-by-listing-the-service-principals-to-which-the-policy-is-assigned"></a>Adım 3: İlkenin atandığı hizmet ilkelerini listeleyerek kaldırmayı denetle 

``` powershell
Get-AzureADPolicyAppliedObject -id <ObjectId of the Policy>
```
## <a name="next-steps"></a>Sonraki adımlar
- Azure AD'de kimlik doğrulamanın nasıl çalıştığı hakkında daha fazla bilgi [için Azure AD için Kimlik Doğrulama senaryolarına](../develop/authentication-scenarios.md)bakın.
- Kullanıcı tek oturum açma hakkında daha fazla bilgi için [Azure Etkin Dizin'deki uygulamalarda tek oturum açma'ya](what-is-single-sign-on.md)bakın.
- Geliştiriciyle ilgili tüm içeriğe genel bakış için [Microsoft kimlik platformuna](../develop/v2-overview.md) göz atın.
