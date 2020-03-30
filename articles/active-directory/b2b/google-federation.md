---
title: B2B için kimlik sağlayıcısı olarak Google'ı ekleyin - Azure AD
description: Konuk kullanıcıların Azure REKLAM uygulamalarınızda kendi Gmail hesaplarıyla oturum açmalarını sağlamak için Google ile federate
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 03/05/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 72c18e48c27942c7bea47931ec79a31af941064e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79126669"
---
# <a name="add-google-as-an-identity-provider-for-b2b-guest-users"></a>B2B konuk kullanıcıları için google'ı kimlik sağlayıcısı olarak ekleyin

Google ile federasyon kurarak, davet edilen kullanıcıların Microsoft hesapları (MSAs) oluşturmak zorunda kalmadan paylaşılan uygulamalarınızda ve kaynaklarınızda kendi Gmail hesaplarıyla oturum açmalarına izin verebilirsiniz. 

> [!NOTE]
> Google federasyonu, Gmail kullanıcıları için özel olarak tasarlanmıştır. G Suite etki alanları ile fetorat yapmak için [doğrudan federasyon özelliğini](direct-federation.md)kullanın.

## <a name="what-is-the-experience-for-the-google-user"></a>Google kullanıcısı için deneyim nedir?
Bir Google Gmail kullanıcısına davet gönderdiğinizde, konuk kullanıcının kiracı bağlamını içeren bir bağlantıyı kullanarak paylaşılan uygulamalarınıza veya kaynaklarınıza erişmesi gerekir. Deneyimleri, Google'da oturum açıp etmediklerine bağlı olarak değişir:
  - Konuk kullanıcı Google'da oturum açmadıysa, Google'da oturum açmaları istenir.
  - Konuk kullanıcı Google'da zaten oturum açmışsa, kullanmak istedikleri hesabı seçmeleri istenir. Onları davet etmek için kullandığınız hesabı seçmeleri gerekir.

Konuk kullanıcı "üstbilgi çok uzun" bir hata görürse, tanımlama bilgilerini temizlemeyi deneyebilir veya özel veya gizli bir pencere açıp yeniden oturum açmayı deneyebilir.

![Google oturum açma sayfasını gösteren ekran görüntüsü](media/google-federation/google-sign-in.png)

## <a name="limitations"></a>Sınırlamalar

Ekipler, google konuk kullanıcılarını tüm cihazlarda tam olarak destekler. Google kullanıcıları Takımlar'da oturum açabilir. `https://teams.microsoft.com`

Diğer uygulamaların ortak bitiş noktaları Google kullanıcılarını desteklemeyebilir. Google konuk kullanıcıları, kiracı bilgilerinizi içeren bir bağlantıyı kullanarak oturum açmalıdır. Aşağıda örnekleri verilmektedir:
  * `https://myapps.microsoft.com/?tenantid=<your tenant id>`
  * `https://portal.azure.com/<your tenant id>`
  * `https://myapps.microsoft.com/<your verified domain>.onmicrosoft.com`

   Google konuk kullanıcıları gibi bir bağlantı `https://myapps.microsoft.com` `https://portal.azure.com`kullanmaya çalışırsa, bir hata alırsınız.

Ayrıca, google konuk kullanıcılarına, örneğin `https://myapps.microsoft.com/signin/Twitter/<application ID?tenantId=<your tenant ID>`kiracı bilgilerinizi içerdiği sürece, bir uygulama veya kaynağa doğrudan bağlantı verebilirsiniz. 

## <a name="step-1-configure-a-google-developer-project"></a>Adım 1: Bir Google geliştirici projesini yapılandırma
İlk olarak, bir istemci kimliği ve daha sonra Azure AD'ye ekleyebileceğiniz bir istemci sırrı elde etmek için Google Geliştiriciler Konsolu'nda yeni bir proje oluşturun. 
1. Google API'lerine https://console.developers.google.comgidin ve Google hesabınızla oturum açın. Paylaşılan bir google hesabı kullanmanızı öneririz.
2. Yeni bir proje oluşturun: Panoda **Proje Oluştur'u**seçin ve ardından **Oluştur'u**seçin. Yeni Proje sayfasında, bir **Proje Adı**girin ve sonra **Oluştur'u**seçin.
   
   ![Google için yeni bir proje sayfasını gösteren ekran görüntüsü](media/google-federation/google-new-project.png)

3. Proje menüsünde yeni projenizin seçildiğinden emin olun. Sonra **API'ler & Hizmetleri**altında, **OAuth onay ekranı**seçin.

4. **Dış'ı**seçin ve sonra **Oluştur'u**seçin. 
5. **OAuth onay ekranında,** bir **Uygulama adı**girin. (Diğer ayarları bırakın.)

   ![Google OAuth onay ekranı seçeneğini gösteren ekran görüntüsü](media/google-federation/google-oauth-consent-screen.png)

6. **Yetkili etki alanları** bölümüne gidin ve microsoftonline.com girin.

   ![Yetkili etki alanları bölümünü gösteren ekran görüntüsü](media/google-federation/google-oauth-authorized-domains.png)

7. **Kaydet'i**seçin.

8. **Kimlik Bilgilerini**seçin. Kimlik **bilgileri oluştur** menüsünde, **OAuth istemci kimliğini**seçin.

   ![Google API'leri gösteren ekran görüntüsü kimlik bilgileri oluşturma seçeneği](media/google-federation/google-api-credentials.png)

9. **Uygulama türü altında,** **Web uygulamasını**seçin ve ardından Yetkili yeniden **yönlendirme URI'leri**altında aşağıdaki URL'leri girin:
   - `https://login.microsoftonline.com` 
   - `https://login.microsoftonline.com/te/<directory id>/oauth2/authresp` <br>(dizin kimliğiniz nerede) `<directory id>`
   
     > [!NOTE]
     > Dizin kimliğinizi bulmak için https://portal.azure.comAzure **Active Directory'ye**gidin ve Azure Active Directory'nin altında **Özellikler'i** seçin ve **Dizin Kimliğini**kopyalayın.

   ![Yetkili yeniden yönlendirme URI'leri bölümünü gösteren ekran görüntüsü](media/google-federation/google-create-oauth-client-id.png)

10. **Oluştur'u**seçin. Azure AD portalına kimlik sağlayıcı eklediğinizde kullanacağınız istemci kimliğini ve istemci sırrını kopyalayın.

   ![OAuth istemci kimliğini ve istemci sırrını gösteren ekran görüntüsü](media/google-federation/google-auth-client-id-secret.png)

## <a name="step-2-configure-google-federation-in-azure-ad"></a>Adım 2: Azure AD'de Google federasyonuyapılandır 
Artık Azure AD portalına girerek veya PowerShell'i kullanarak Google istemci kimliğini ve istemci sırrını ayarlarsınız. Bir Gmail adresi kullanarak kendinizi davet ederek ve daveti davet edilen Google hesabınızla kullanmaya çalışarak Google federasyonu yapılandırmanızı test etmeyi unutmayın. 

#### <a name="to-configure-google-federation-in-the-azure-ad-portal"></a>Azure AD portalında Google federasyonuyapılandırmak için 
1. [Azure portalına](https://portal.azure.com)gidin. Sol bölmede **Azure Active Directory**’yi seçin. 
2. **Kuruluş İlişkileri'ni**seçin.
3. **Kimlik sağlayıcılarını**seçin ve ardından **Google** düğmesini tıklatın.
4. Bir ad girin. Daha sonra daha önce elde ettiğiniz istemci kimliğini ve istemci sırrını girin. **Kaydet'i**seçin. 

   ![Google kimlik sağlayıcısı ekle sayfasını gösteren ekran görüntüsü](media/google-federation/google-identity-provider.png)

#### <a name="to-configure-google-federation-by-using-powershell"></a>PowerShell'i kullanarak Google federasyonunu yapılandırmak için
1. Azure AD PowerShell'in en son sürümünü Grafik modülü için yükleyin[(AzureADPreview).](https://www.powershellgallery.com/packages/AzureADPreview)
2. Aşağıdaki komutu `Connect-AzureAD`çalıştırın: .
3. Oturum açma isteminde, yönetilen Global Administrator hesabıyla oturum açın.  
4. Şu komutu çalıştırın: 
   
   `New-AzureADMSIdentityProvider -Type Google -Name Google -ClientId [Client ID] -ClientSecret [Client secret]`
 
   > [!NOTE]
   > "Adım 1: Bir Google geliştirici projesini yapılandırın" adlı uygulamadan müşteri kimliği ve istemci sırrını kullanın. Daha fazla bilgi için [Yeni AzureADMSIdentityProvider](https://docs.microsoft.com/powershell/module/azuread/new-azureadmsidentityprovider?view=azureadps-2.0-preview) makalesine bakın. 
 
## <a name="how-do-i-remove-google-federation"></a>Google federasyonu nasıl kaldıracağım?
Google federasyon kurulumunuzu silebilirsiniz. Bunu yaparsanız, davetlerini zaten kullanan Google konuk kullanıcıları oturum açamaz, ancak bunları dizinden silerek ve yeniden davet ederek kaynaklarınıza yeniden erişim hakkı verebilirsiniz. 
 
### <a name="to-delete-google-federation-in-the-azure-ad-portal"></a>Azure AD portalındaki Google federasyonunu silmek için: 
1. [Azure portalına](https://portal.azure.com)gidin. Sol bölmede **Azure Active Directory**’yi seçin. 
2. **Kuruluş İlişkileri'ni**seçin.
3. **Kimlik sağlayıcılarını**seçin.
4. **Google** satırında bağlam menüsünü (**...**) seçin ve sonra **Sil'i**seçin. 
   
   ![Sosyal kimlik sağlayıcısı için Sil seçeneğini gösteren ekran görüntüsü](media/google-federation/google-social-identity-providers.png)

1. Silme işlemini onaylamak için **Evet'i** seçin. 

### <a name="to-delete-google-federation-by-using-powershell"></a>PowerShell'i kullanarak Google federasyonunu silmek için: 
1. Azure AD PowerShell'in en son sürümünü Grafik modülü için yükleyin[(AzureADPreview).](https://www.powershellgallery.com/packages/AzureADPreview)
2. `Connect-AzureAD` öğesini çalıştırın.  
4. Giriş isteminde, yönetilen Global Administrator hesabıyla oturum açın.  
5. Aşağıdaki komutu girin:

    `Remove-AzureADMSIdentityProvider -Id Google-OAUTH`

   > [!NOTE]
   > Daha fazla bilgi için bkz: [Remove-AzureADMSIdentityProvider.](https://docs.microsoft.com/powershell/module/azuread/Remove-AzureADMSIdentityProvider?view=azureadps-2.0-preview) 
