---
author: billmath
ms.service: active-directory
ms.subservice: cloud-provisioning
ms.topic: include
ms.date: 10/16/2019
ms.author: billmath
ms.openlocfilehash: 3aa1571b46938b03f556fa124d3f0a2a70f2c5c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79504292"
---
## <a name="steps-to-enable-single-sign-on"></a>Tek Oturum Açma'yı etkinleştirme adımları
Bulut sağlama, Tek Oturum Açma ile çalışır.  Şu anda aracı yüklendiğinde SSO'yu etkinleştirme seçeneği yoktur, ancak SSO'yu etkinleştirmek ve kullanmak için aşağıdaki adımları kullanabilirsiniz. 

### <a name="step-1-download-and-extract-azure-ad-connect-files"></a>Adım 1: Azure AD Connect dosyalarını indirin ve ayıklayın
1.  İlk olarak, Azure [AD Connect'in](https://www.microsoft.com/download/details.aspx?id=47594) en son sürümünü indirin
2.  Yönetim ayrıcalıklarını kullanarak bir komut istemi açın ve az önce indirdiğiniz msi'ye gidin.
3.  Aşağıdakileri çalıştırın:`msiexec /a C:\filepath\AzureADConnect.msi /qb TARGETDIR=C:\filepath\extractfolder`
4. Dosya yolunu ve extractfolder'ı dosya yolunuza ve ayıklama klasörünüzü eşleştirecek şekilde değiştirin.  İçeriği şimdi çıkarma klasöründe olmalıdır.

### <a name="step-2-import-the-seamless-sso-powershell-module"></a>Adım 2: Dikişsiz SSO PowerShell modüllerini alma

1. Azure AD [PowerShell'i](https://docs.microsoft.com/powershell/azure/active-directory/overview)indirin ve yükleyin.
2. Klasöre `%programfiles%\Microsoft Azure Active Directory Connect` göz atın.
3. Bu komutu kullanarak Dikişsiz SSO `Import-Module .\AzureADSSO.psd1`PowerShell modüllerini içeri aktarın: .

### <a name="step-3-get-the-list-of-active-directory-forests-on-which-seamless-sso-has-been-enabled"></a>Adım 3: Kesintisiz SSO'nun etkinleştirildiği Active Directory ormanlarının listesini alın

1. PowerShell'i yönetici olarak çalıştırın. PowerShell'de, `New-AzureADSSOAuthenticationContext`arayın. İstendiğinde, kiracınızın genel yönetici kimlik bilgilerini girin.
2. Arayın. `Get-AzureADSSOStatus` Bu komut, bu özelliğin etkinleştirildiği Etkin Dizin ormanları ("Etki Alanları" listesine bakın) listesini sağlar.

### <a name="step-4-enable-seamless-sso-for-each-active-directory-forest"></a>Adım 4: Her Active Directory ormanı için Kesintisiz SSO'ya olanak sağlama

1. Arayın. `Enable-AzureADSSOForest` İstendiğinde, amaçlanan Etkin Dizin ormanı için etki alanı yöneticisi kimlik bilgilerini girin.

   > [!NOTE]
   >Etki alanı yöneticisi kimlik bilgileri kullanıcı adı SAM hesap adı biçiminde girilmelidir (contoso\johndoe veya contoso.com\johndoe). DNS kullanarak Etki Alanı Yöneticisi'nin Etki Alanı Denetleyicisini bulmak için kullanıcı adının etki alanı bölümünü kullanırız.

   >[!NOTE]
   >Kullanılan etki alanı yöneticisi hesabı Korumalı Kullanıcılar grubunun bir üyesi olmamalıdır. Bu nedenle, işlem başarısız olur.

2. Özelliği ayarlamak istediğiniz her Etkin Dizin ormanı için önceki adımı yineleyin.

### <a name="step-5-enable-the-feature-on-your-tenant"></a>5. Adım. Kiracınızdaki özelliği etkinleştirme

Kiracınızdaki özelliği açmak için . `Enable-AzureADSSO -Enable $true`
