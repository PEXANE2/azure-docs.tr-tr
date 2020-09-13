---
title: B2B için kimlik sağlayıcısı olarak Google ekleme-Azure AD
description: Konuk kullanıcıların Azure AD uygulamalarınızda kendi Gmail hesabıyla oturum açmasını sağlamak için Google ile Federasyonun
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
ms.openlocfilehash: eef04be1891eac35577a5f4cb18d5b83b8d0f301
ms.sourcegitcommit: 5d7f8c57eaae91f7d9cf1f4da059006521ed4f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89669380"
---
# <a name="add-google-as-an-identity-provider-for-b2b-guest-users"></a>B2B Konuk kullanıcıları için bir kimlik sağlayıcısı olarak Google ekleme

Google ile Federasyon ayarlayarak, davet edilen kullanıcıların Microsoft hesapları (MSAs) oluşturmaya gerek kalmadan kendi Gmail hesaplarıyla paylaşılan uygulamalarınızda ve kaynaklarda oturum açmalarına izin verebilirsiniz. 

> [!NOTE]
> Google Federasyonu, Gmail kullanıcıları için özel olarak tasarlanmıştır. G Suite etki alanlarıyla federasyona eklemek için, [doğrudan Federasyon özelliğini](direct-federation.md)kullanın.

## <a name="what-is-the-experience-for-the-google-user"></a>Google kullanıcısına yönelik deneyim nedir?
Bir Google Gmail kullanıcısına davetiye gönderdiğinizde, Konuk kullanıcının kiracı bağlamını içeren bir bağlantıyı kullanarak paylaşılan uygulamalarınıza veya kaynaklarınıza erişmesi gerekir. Bu deneyimler, Google 'da zaten oturum açmış olmasına bağlı olarak farklılık gösterir:
  - Konuk Kullanıcı Google 'da oturum açmamışsa, Google 'da oturum açması istenir.
  - Konuk Kullanıcı zaten Google 'da oturum açmışsa, kullanmak istedikleri hesabı seçmeleri istenir. Bunları davet etmek için kullandığınız hesabı seçmesi gerekir.

Konuk Kullanıcı bir "üst bilgi çok uzun" hatası görürse, kendi tanımlama bilgilerini temizlemeyi deneyebilir veya özel ya da bilişsiz pencereyi açıp yeniden oturum açmayı deneyebilir.

![Google oturum açma sayfasını gösteren ekran görüntüsü](media/google-federation/google-sign-in.png)

## <a name="limitations"></a>Sınırlamalar

Takımlar tüm cihazlarda Google Konuk kullanıcılarını tamamen destekler. Google kullanıcıları gibi ortak bir uç noktadan ekiplerde oturum açabilirler `https://teams.microsoft.com` .

Diğer uygulamaların ortak uç noktaları Google kullanıcılarını desteklemeyebilir. Google Konuk kullanıcılarının kiracı bilgilerinizi içeren bir bağlantı kullanarak oturum açması gerekir. Aşağıda örnekleri verilmektedir:
  * `https://myapps.microsoft.com/?tenantid=<your tenant id>`
  * `https://portal.azure.com/<your tenant id>`
  * `https://myapps.microsoft.com/<your verified domain>.onmicrosoft.com`

   Google Konuk kullanıcıları veya gibi bir bağlantı kullanmayı denlerse `https://myapps.microsoft.com` `https://portal.azure.com` bir hata alırlar.

Ayrıca, bu bağlantı kiracı bilgilerinizi içerdiği sürece Google Konuk kullanıcılarına bir uygulama veya kaynağa doğrudan bağlantı verebilirsiniz `https://myapps.microsoft.com/signin/Twitter/<application ID?tenantId=<your tenant ID>` . 

## <a name="step-1-configure-a-google-developer-project"></a>1. Adım: Google Geliştirici projesini yapılandırma
İlk olarak, bir istemci KIMLIĞI ve daha sonra Azure AD 'ye ekleyebileceğiniz bir istemci gizli anahtarı almak için Google geliştiricileri konsolunda yeni bir proje oluşturun. 
1. Adresindeki Google API 'Lerine gidin https://console.developers.google.com ve Google hesabınızla oturum açın. Paylaşılan bir takım Google hesabı kullanmanızı öneririz.
2. İstenirse hizmet koşullarını kabul edin
3. Yeni bir proje oluşturun: panoda **proje oluştur**' u seçin, projeye bir ad verin (örneğin, "Azure AD B2B") ve ardından **Oluştur**' u seçin. 
   
   ![Google için yeni bir proje sayfası gösteren ekran görüntüsü](media/google-federation/google-new-project.png)

4. Artık size sunulan **apı & Hizmetleri** sayfasında yeni projenizin altındaki **Görünüm** ' e tıklayın.

5. API 'Ler kartına **API 'Lere git ' e** tıklayın. **OAuth onay ekranı**' nı seçin.

6. **Dış**' i seçin ve ardından **Oluştur**' u seçin. 

7. **OAuth onay ekranında**bir **uygulama adı**girin. 

   ![Google OAuth onay ekranı seçeneğini gösteren ekran görüntüsü](media/google-federation/google-oauth-consent-screen.png)

8. **Yetkili etki alanları** bölümüne gidin ve microsoftonline.com girin.

   ![Yetkili etki alanları bölümünü gösteren ekran görüntüsü](media/google-federation/google-oauth-authorized-domains.PNG)

9. **Kaydet**’i seçin.

10. **Kimlik bilgilerini**seçin. **Kimlik bilgileri oluştur** menüsünde **OAuth istemci kimliği**' ni seçin.

    ![Google API 'Leri kimlik bilgileri oluşturma seçeneğini gösteren ekran görüntüsü](media/google-federation/google-api-credentials.png)

11. **Uygulama türü**' nün altında **Web uygulaması** ' nı seçin ve uygulamaya uygun bir ad verin (ÖRNEĞIN, "Azure AD B2B") ve sonra **yetkilendirilmiş yeniden yönlendirme URI 'leri**altında aşağıdaki URI 'leri girin:
    - `https://login.microsoftonline.com` 
    - `https://login.microsoftonline.com/te/<directory id>/oauth2/authresp` <br>( `<directory id>` DIZIN kimliğiniz nerede)
   
    > [!NOTE]
    > Dizin KIMLIĞINIZI bulmak için adresine gidin https://portal.azure.com ve **Azure Active Directory**altında **Özellikler** ' i seçin ve **dizin kimliğini**kopyalayın.

    ![Yetkili yeniden yönlendirme URI 'Leri bölümünü gösteren ekran görüntüsü](media/google-federation/google-create-oauth-client-id.png)

12. **Oluştur**’u seçin. Kimlik sağlayıcısını Azure AD portalına eklerken kullanacağınız istemci KIMLIĞINI ve istemci parolasını kopyalayın.

    ![OAuth İstemci KIMLIĞINI ve istemci gizli anahtarını gösteren ekran görüntüsü](media/google-federation/google-auth-client-id-secret.png)

## <a name="step-2-configure-google-federation-in-azure-ad"></a>2. Adım: Google Federation 'ı Azure AD 'de yapılandırma 
Artık Google istemci KIMLIĞINI ve istemci gizli anahtarını Azure AD portalına girerek ya da PowerShell kullanarak ayarlayacaksınız. Kendinize bir Gmail adresi kullanarak davet ederek Google Federation yapılandırmanızı test ettiğinizden emin olun ve daveti davet edilen Google hesabınızla kullanmaya çalışıyorsunuz. 

#### <a name="to-configure-google-federation-in-the-azure-ad-portal"></a>Google Federasyonu 'ni Azure AD portalında yapılandırmak için 
1. [Azure portalına](https://portal.azure.com) gidin. Sol bölmede **Azure Active Directory**’yi seçin. 
2. **Dış kimlikler**' i seçin.
3. **Tüm kimlik sağlayıcıları**' nı seçin ve ardından **Google** düğmesine tıklayın.
4. Daha önce edindiğiniz istemci KIMLIĞINI ve istemci gizli anahtarını girin. **Kaydet**’i seçin. 

   ![Google Identity Provider Ekle sayfasını gösteren ekran görüntüsü](media/google-federation/google-identity-provider.png)

#### <a name="to-configure-google-federation-by-using-powershell"></a>PowerShell kullanarak Google Federasyonu yapılandırmak için
1. Graph modülü için Azure AD PowerShell 'in en son sürümünü ([Azureadpreview](https://www.powershellgallery.com/packages/AzureADPreview)) yükler.
2. Şu komutu çalıştırın: `Connect-AzureAD` .
3. Oturum açma isteminde, yönetilen genel yönetici hesabıyla oturum açın.  
4. Şu komutu çalıştırın: 
   
   `New-AzureADMSIdentityProvider -Type Google -Name Google -ClientId [Client ID] -ClientSecret [Client secret]`
 
   > [!NOTE]
   > "Adım 1: Google Geliştirici projesi yapılandırma" bölümünde oluşturduğunuz uygulamadan istemci kimliğini ve istemci gizli anahtarını kullanın. Daha fazla bilgi için bkz. [New-AzureADMSIdentityProvider](https://docs.microsoft.com/powershell/module/azuread/new-azureadmsidentityprovider?view=azureadps-2.0-preview) makalesi. 
 
## <a name="how-do-i-remove-google-federation"></a>Google Federasyonu Nasıl yaparım? kaldırılsın mı?
Google Federation kurulumunuzu silebilirsiniz. Bunu yaparsanız, davetini zaten kullanan Google Konuk kullanıcılar oturum açamaz, ancak bunları dizinden silerek ve yeniden davet ederek kaynaklarınıza yeniden erişim izni verebilirsiniz. 
 
### <a name="to-delete-google-federation-in-the-azure-ad-portal"></a>Azure AD portalında Google Federasyonu 'ni silmek için: 
1. [Azure portalına](https://portal.azure.com) gidin. Sol bölmede **Azure Active Directory**’yi seçin. 
2. **Dış kimlikler**' i seçin.
3. **Tüm kimlik sağlayıcılarını**seçin.
4. **Google** satırında bağlam menüsünü (**...**) seçin ve **Sil**' i seçin. 
   
   ![Sosyal kimlik sağlayıcısı için silme seçeneğini gösteren ekran görüntüsü](media/google-federation/google-social-identity-providers.png)

1. Silmeyi onaylamak için **Evet** ' i seçin. 

### <a name="to-delete-google-federation-by-using-powershell"></a>PowerShell kullanarak Google Federasyonu silmek için: 
1. Graph modülü için Azure AD PowerShell 'in en son sürümünü ([Azureadpreview](https://www.powershellgallery.com/packages/AzureADPreview)) yükler.
2. `Connect-AzureAD` öğesini çalıştırın.  
4. Oturum açma isteminde, yönetilen genel yönetici hesabıyla oturum açın.  
5. Aşağıdaki komutu girin:

    `Remove-AzureADMSIdentityProvider -Id Google-OAUTH`

   > [!NOTE]
   > Daha fazla bilgi için bkz. [Remove-AzureADMSIdentityProvider](https://docs.microsoft.com/powershell/module/azuread/Remove-AzureADMSIdentityProvider?view=azureadps-2.0-preview). 
