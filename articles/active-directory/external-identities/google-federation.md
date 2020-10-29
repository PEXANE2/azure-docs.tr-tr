---
title: B2B için kimlik sağlayıcısı olarak Google ekleme-Azure AD
description: Konuk kullanıcıların Azure AD uygulamalarınızda kendi Gmail hesaplarıyla oturum açmasını sağlamak için Google ile Federasyonun.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 05/11/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: ff8912794169cf61f394a097248a8476b2e0c0f3
ms.sourcegitcommit: dd45ae4fc54f8267cda2ddf4a92ccd123464d411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "92926244"
---
# <a name="add-google-as-an-identity-provider-for-b2b-guest-users"></a>B2B Konuk kullanıcıları için bir kimlik sağlayıcısı olarak Google ekleme

Google ile Federasyonu ayarlayarak, davet edilen kullanıcıların Microsoft hesapları oluşturmaya gerek kalmadan kendi Gmail hesaplarıyla paylaşılan uygulamalarınızda ve kaynaklarda oturum açmalarına izin verebilirsiniz. 

> [!NOTE]
> Google Federasyonu, Gmail kullanıcıları için özel olarak tasarlanmıştır. G Suite etki alanlarıyla federasyona eklemek için [doğrudan Federasyon](direct-federation.md)' ı kullanın.

## <a name="what-is-the-experience-for-the-google-user"></a>Google kullanıcısına yönelik deneyim nedir?
Google Gmail kullanıcılarına davetiye gönderdiğinizde, Konuk kullanıcılar, kiracı bağlamını içeren bir bağlantıyı kullanarak paylaşılan uygulamalarınıza veya kaynaklarınıza erişmelidir. Bu deneyimler, Google 'da zaten oturum açmış olmasına bağlı olarak farklılık gösterir:
  - Google 'da oturum açmamış Konuk kullanıcılardan bunu yapması istenir.
  - Google 'da zaten oturum açmış Konuk kullanıcılardan kullanmak istedikleri hesabı seçmeleri istenir. Bunları davet etmek için kullandığınız hesabı seçmesi gerekir.

"Üst bilgi çok uzun" hatası görebilen Konuk kullanıcılar kendi tanımlama bilgilerini temizleyebilir veya özel veya bilito penceresini açabilir ve yeniden oturum açmayı deneyebilir.

![Google oturum açma sayfasını gösteren ekran görüntüsü.](media/google-federation/google-sign-in.png)

## <a name="limitations"></a>Sınırlamalar

Takımlar tüm cihazlarda Google Konuk kullanıcılarını tamamen destekler. Google kullanıcıları gibi ortak bir uç noktadan ekiplerde oturum açabilirler `https://teams.microsoft.com` .

Diğer uygulamaların ortak uç noktaları Google kullanıcılarını desteklemeyebilir. Google Konuk kullanıcıları kiracı bilgilerinizi içeren bir bağlantıyı kullanarak oturum açmalısınız. Örnekler aşağıda verilmiştir:
  * `https://myapps.microsoft.com/?tenantid=<your tenant ID>`
  * `https://portal.azure.com/<your tenant ID>`
  * `https://myapps.microsoft.com/<your verified domain>.onmicrosoft.com`

   Google Guest kullanıcıları veya gibi bir bağlantı kullanmayı denlerse `https://myapps.microsoft.com` `https://portal.azure.com` bir hata alırlar.

Ayrıca, bağlantı kiracı bilgilerinizi içerdiği sürece, Google Konuk kullanıcılarına bir uygulama veya kaynağa doğrudan bağlantı verebilirsiniz. Örneğin, `https://myapps.microsoft.com/signin/Twitter/<application ID?tenantId=<your tenant ID>`. 

## <a name="step-1-configure-a-google-developer-project"></a>1. Adım: Google Geliştirici projesini yapılandırma
İlk olarak, daha sonra Azure Active Directory (Azure AD) ekleyebileceğiniz istemci KIMLIĞINI ve bir istemci gizli anahtarını almak için Google geliştiricileri konsolunda yeni bir proje oluşturun. 
1. Adresindeki Google API 'Lerine gidin https://console.developers.google.com ve Google hesabınızla oturum açın. Paylaşılan bir takım Google hesabı kullanmanızı öneririz.
2. İstenirse hizmet koşullarını kabul edin.
3. Yeni bir proje oluşturun: panoda **proje oluştur** ' u seçin, projeye bir ad verin (örneğin, **Azure AD B2B** ) ve ardından **Oluştur** ' u seçin: 
   
   ![Yeni bir proje sayfasını gösteren ekran görüntüsü.](media/google-federation/google-new-project.png)

4. **Apı & Hizmetleri** sayfasında yeni projenizin altında **Görünüm** ' ü seçin.

5. API 'Ler kartında **API 'lere genel bakış ' ı** seçin. **OAuth onay ekranı** ' nı seçin.

6. **Dış** ' i seçin ve ardından **Oluştur** ' u seçin. 

7. **OAuth onay ekranında** , bir **uygulama adı** girin:

   ![Google OAuth onay ekranını gösteren ekran görüntüsü.](media/google-federation/google-oauth-consent-screen.png)

8. **Yetkili etki alanları** bölümüne gidin ve **microsoftonline.com** girin:

   ![Yetkili etki alanları bölümünü gösteren ekran görüntüsü.](media/google-federation/google-oauth-authorized-domains.PNG)

9. **Kaydet** ’i seçin.

10. **Kimlik Bilgileri** ’ni seçin. **Kimlik bilgileri oluştur** menüsünde, **OAuth istemci kimliği** ' ni seçin.

    ![Google API 'Leri oluşturma kimlik bilgileri menüsünü gösteren ekran görüntüsü.](media/google-federation/google-api-credentials.png)

11. **Uygulama türü** altında **Web uygulaması** ' nı seçin. Uygulamaya **Azure AD B2B** gibi uygun bir ad verin. **Yetkili yeniden yönlendirme URI 'leri** altında aşağıdaki URI 'leri girin:
    - `https://login.microsoftonline.com`
    - `https://login.microsoftonline.com/te/<tenant ID>/oauth2/authresp` <br>( `<tenant ID>` KIRACı kimliğiniz nerede)
   
    > [!NOTE]
    > Kiracı KIMLIĞINIZI bulmak için [Azure Portal](https://portal.azure.com)gidin. **Azure Active Directory** altında **Özellikler** ' i seçin ve **Kiracı kimliğini** kopyalayın.

    ![Yetkili yeniden yönlendirme URI 'Leri bölümünü gösteren ekran görüntüsü.](media/google-federation/google-create-oauth-client-id.png)

12. **Oluştur** ’u seçin. İstemci KIMLIĞINI ve istemci parolasını kopyalayın. Azure portal kimlik sağlayıcısını eklediğinizde kullanırsınız.

    ![OAuth İstemci KIMLIĞINI ve istemci gizli anahtarını gösteren ekran görüntüsü.](media/google-federation/google-auth-client-id-secret.png)

## <a name="step-2-configure-google-federation-in-azure-ad"></a>2. Adım: Google Federation 'ı Azure AD 'de yapılandırma 
Şimdi Google istemci KIMLIĞINI ve istemci gizli anahtarını ayarlayacaksınız. Azure portal veya PowerShell 'i kullanarak bunu yapabilirsiniz. Google Federation yapılandırmanızı kendinize davet ederek test ettiğinizden emin olun. Bir Gmail adresi kullanın ve daveti davet edilen Google hesabınızla kullanmaya çalışın. 

**Azure portal Google Federation 'ı yapılandırmak için** 
1. [Azure portalına](https://portal.azure.com) gidin. Sol bölmede **Azure Active Directory** ' yi seçin. 
2. **Dış kimlikler** ' i seçin.
3. **Tüm kimlik sağlayıcıları** ' nı seçin ve ardından **Google** düğmesini seçin.
4. Daha önce edindiğiniz istemci KIMLIĞINI ve istemci gizli anahtarını girin. **Kaydet** ' i seçin: 

   ![Google Identity Provider Ekle sayfasını gösteren ekran görüntüsü.](media/google-federation/google-identity-provider.png)

**PowerShell kullanarak Google Federasyonu yapılandırmak için**
1. Graph modülü için Azure AD PowerShell 'in en son sürümünü ([Azureadpreview](https://www.powershellgallery.com/packages/AzureADPreview)) yükler.
2. Şu komutu çalıştırın: `Connect-AzureAD`
3. Oturum açma isteminde, yönetilen genel yönetici hesabıyla oturum açın.  
4. Şu komutu çalıştırın: 
   
   `New-AzureADMSIdentityProvider -Type Google -Name Google -ClientId <client ID> -ClientSecret <client secret>`
 
   > [!NOTE]
   > "Adım 1: Google Geliştirici projesi yapılandırma" bölümünde oluşturduğunuz uygulamadan istemci KIMLIĞINI ve istemci gizli anahtarını kullanın. Daha fazla bilgi için, bkz. [New-AzureADMSIdentityProvider](/powershell/module/azuread/new-azureadmsidentityprovider?view=azureadps-2.0-preview). 
 
## <a name="how-do-i-remove-google-federation"></a>Google Federasyonu Nasıl yaparım? kaldırılsın mı?
Google Federation kurulumunuzu silebilirsiniz. Bunu yaparsanız, davetini zaten kullanan Google Konuk kullanıcılar oturum açamaz. Ancak onları dizinden silerek ve yeniden davet ederek kaynaklarınıza erişim izni verebilirsiniz. 
 
**Azure AD portalında Google Federasyonu 'ni silmek için**
1. [Azure portalına](https://portal.azure.com) gidin. Sol bölmede **Azure Active Directory** ' yi seçin. 
2. **Dış kimlikler** ' i seçin.
3. **Tüm kimlik sağlayıcılarını** seçin.
4. **Google** satırında üç nokta düğmesini ( **...** ) seçin ve **Sil** ' i seçin. 
   
   ![Sosyal kimlik sağlayıcısı için Sil düğmesini gösteren ekran görüntüsü.](media/google-federation/google-social-identity-providers.png)

1. Silmeyi onaylamak için **Evet** ' i seçin. 

**PowerShell kullanarak Google Federasyonu silmek için** 
1. Graph modülü için Azure AD PowerShell 'in en son sürümünü ([Azureadpreview](https://www.powershellgallery.com/packages/AzureADPreview)) yükler.
2. Şu komutu çalıştırın: `Connect-AzureAD`.  
4. Oturum açma isteminde, yönetilen genel yönetici hesabıyla oturum açın.  
5. Aşağıdaki komutu girin:

    `Remove-AzureADMSIdentityProvider -Id Google-OAUTH`

   > [!NOTE]
   > Daha fazla bilgi için bkz. [Remove-AzureADMSIdentityProvider](/powershell/module/azuread/Remove-AzureADMSIdentityProvider?view=azureadps-2.0-preview).
