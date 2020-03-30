---
title: Yayınlanan uygulamalar için özel giriş sayfası - Azure AD Application Proxy
description: Azure AD Uygulama Proxy bağlayıcıları hakkındaki temel bilgileri kapsar
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: mimart
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1621b273f617955a374ed46d9c215ba99e5b2913
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74275609"
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a>Azure AD Uygulama Proxy'yi kullanarak yayımlanmış uygulamalar için özel bir giriş sayfası ayarlama

Bu makalede, bir uygulamayı özel bir giriş sayfasına yönlendirecek şekilde nasıl yapılandırılabilmek açıklanmıştır. Uygulama Proxy'li bir uygulama yayımladığınızda, dahili bir URL ayarlarsınız, ancak bazen bu, kullanıcının ilk olarak görmesi gereken sayfa değildir. Kullanıcının uygulamaya erişirken doğru sayfayı kapabilmesi için özel bir giriş sayfası ayarlayın. Kullanıcı, uygulamaya Azure Active Directory Access Paneli'nden veya Office 365 uygulama başlatıcısından erişip erişmediklerine bakılmaksızın ayarladığınız özel giriş sayfasını görür.

Bir kullanıcı uygulamayı başlattığında, varsayılan olarak yayınlanan uygulamanın kök etki alanı URL'sine yönlendirilir. Açılış sayfası genellikle ana sayfa URL'si olarak ayarlanır. Bir uygulama kullanıcısının uygulama içindeki belirli bir sayfaya inmesini istediğinizde özel bir ana sayfa URL'si tanımlamak için Azure AD PowerShell modüllerini kullanın.

Şirketinizin neden özel bir giriş sayfası ayarladığını açıklayan bir senaryo aşağıda verilmiştir:

- Şirket ağınızda, bir kullanıcı `https://ExpenseApp/login/login.aspx` oturum açmave uygulamanıza erişmeye gider.
- Uygulama Proxy klasör yapısının en üst düzeyinde erişmek için gereken diğer varlıklar (resimler gibi) `https://ExpenseApp` olduğundan, dahili URL olarak uygulama yayımlamak.
- Varsayılan dış `https://ExpenseApp-contoso.msappproxy.net`URL, oturum açma sayfasına harici bir kullanıcı götürmez.
- Bunun yerine `https://ExpenseApp-contoso.msappproxy.net/login/login.aspx` ana sayfa URL'si olarak ayarlamak istiyorsunuz, böylece harici bir kullanıcı önce oturum açma sayfasını görür.

> [!NOTE]
> Kullanıcılara yayınlanmış uygulamalara erişim verdiğinizde, uygulamalar [Azure AD Erişim Paneli'nde](../user-help/my-apps-portal-end-user-access.md) ve Office [365 uygulama başlatıcısında](https://www.microsoft.com/microsoft-365/blog/2016/09/27/introducing-the-new-office-365-app-launcher/)görüntülenir.

## <a name="before-you-start"></a>Başlamadan önce

Ana sayfa URL'sini ayarlamadan önce aşağıdaki gereksinimleri göz önünde bulundurun:

- Belirttiğiniz yol, kök etki alanı URL'sinin bir alt etki alanı yolu olmalıdır.

  Örneğin, kök etki alanı URL'si `https://apps.contoso.com/app1/`ise, yapılandırdığınız ana `https://apps.contoso.com/app1/`sayfa URL'si ile başlamalıdır.

- Yayınlanan uygulamada bir değişiklik yaparsanız, değişiklik ana sayfa URL'sinin değerini sıfırlanabilir. Uygulamayı gelecekte güncellediğinizde, ana sayfa nın URL'sini yeniden denetlemeli ve gerekirse güncellemelisiniz.

Ana sayfa URL'sini Azure portalı üzerinden veya PowerShell'i kullanarak ayarlayabilirsiniz.

## <a name="change-the-home-page-in-the-azure-portal"></a>Azure portalındaki ana sayfayı değiştirme

Azure AD portalı aracılığıyla uygulamanızın ana sayfa URL'sini değiştirmek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com/)’da yönetici olarak oturum açın.
1. **Azure Etkin Dizini'ni**ve ardından **Uygulama kayıtlarını**seçin. Kayıtlı uygulamaların listesi görüntülenir.
1. Uygulamanızı listeden seçin. Kayıtlı uygulamanın ayrıntılarını gösteren bir sayfa görüntülenir.
1. **Yönet**altında, **Markalama'yı**seçin.
1. Giriş **sayfası nın URL'sini** yeni yolunuzla güncelleştirin.

   ![Giriş Sayfası URL alanını gösteren kayıtlı bir uygulama için marka sayfası](media/application-proxy-configure-custom-home-page/app-proxy-app-branding.png)

1. **Kaydet'i**seçin.

## <a name="change-the-home-page-with-powershell"></a>PowerShell ile ana sayfayı değiştirme

PowerShell'i kullanarak bir uygulamanın ana sayfasını yapılandırmak için şunları yapmanız gerekir:

1. Azure AD PowerShell modüllerini yükleyin.
1. Uygulamanın ObjectId değerini bulun.
1. PowerShell komutlarını kullanarak uygulamanın ana sayfa URL'sini güncelleyin.

### <a name="install-the-azure-ad-powershell-module"></a>Azure AD PowerShell modüllerini yükleme

PowerShell'i kullanarak özel bir ana sayfa URL'si tanımlamadan önce Azure AD PowerShell modüllerini yükleyin.Paketi, Grafik API bitiş noktasını kullanan [PowerShell Galerisi'nden](https://www.powershellgallery.com/packages/AzureAD/2.0.2.16)indirebilirsiniz.

Paketi yüklemek için aşağıdaki adımları izleyin:

1. Standart bir PowerShell penceresini açın ve ardından aşağıdaki komutu çalıştırın:

   ```powershell
   Install-Module -Name AzureAD
   ```

    Komutu yönetici olmayan olarak çalıştırıyorsanız, `-scope currentuser` seçeneği kullanın.

1. Yükleme sırasında, Nuget.org iki paket yüklemek için **Y'yi** seçin. Her iki paket gereklidir.

### <a name="find-the-objectid-of-the-app"></a>Uygulamanın ObjectId'ini bulun

Uygulamanın ObjectId'ini görüntü adıyla veya ana sayfasına bakarak elde elabilirsiniz.

1. Aynı PowerShell penceresinde Azure AD modüllerini içe aktarın.

   ```powershell
   Import-Module AzureAD
   ```

1. Azure AD modülünde kiracı yönetici olarak oturum açın.

   ```powershell
   Connect-AzureAD
   ```

1. Uygulamayı bulun. Bu örnekte, display adı olan uygulamayı arayarak ObjectId'i bulmak için PowerShell `SharePoint`kullanır.

   ```powershell
   Get-AzureADApplication | Where-Object { $_.DisplayName -eq "SharePoint" } | Format-List DisplayName, Homepage, ObjectId
   ```

   Burada gösterilene benzer bir sonuç almalısınız. Sonraki bölümde kullanmak üzere ObjectId GUID'i kopyalayın.

   ```console
   DisplayName : SharePoint
   Homepage    : https://sharepoint-iddemo.msappproxy.net/
   ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
   ```

   Alternatif olarak, tüm uygulamaların listesini çekebilir, belirli bir ekran adı veya ana sayfayla uygulama nın listesini arayabilir ve uygulama bulunduğunda uygulamanın ObjectId'ini kopyalayabilirsiniz.

   ```powershell
   Get-AzureADApplication | Format-List DisplayName, Homepage, ObjectId
   ```

### <a name="update-the-home-page-url"></a>Ana sayfa URL'sini güncelleştirme

Ana sayfa URL'sini oluşturun ve uygulamanızı bu değerle güncelleyin. Aynı PowerShell penceresini kullanmaya devam edin veya yeni bir PowerShell penceresi kullanıyorsanız, `Connect-AzureAD`azure AD modülünde yeniden oturum açın. Ardından aşağıdaki adımları izleyin:

1. Önceki bölümde kopyaladığınız ObjectId değerini tutmak için bir değişken oluşturun. (Bu SharePoint örneğinde kullanılan ObjectId değerini uygulamanızın ObjectId değeriyle değiştirin.)

   ```powershell
   $objguid = "8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4"
   ```

1. Aşağıdaki komutu çalıştırarak doğru uygulamaya sahip olduğunuzu doğrulayın. Çıktı, önceki bölümde gördüğünüz çıktıyla aynı olmalıdır[(Uygulamanın ObjectId'ini bulun).](#find-the-objectid-of-the-app)

   ```powershell
   Get-AzureADApplication -ObjectId $objguid | Format-List DisplayName, Homepage, ObjectId
   ```

1. Yapmak istediğiniz değişiklikleri tutmak için boş bir uygulama nesnesi oluşturun.

   ```powershell
   $appnew = New-Object "Microsoft.Open.AzureAD.Model.Application"
   ```

1. Ana sayfa URL'sini istediğiniz değere ayarlayın. Değer, yayınlanan uygulamanın bir alt etki alanı yolu olmalıdır. Örneğin, ana sayfa URL'sini `https://sharepoint-iddemo.msappproxy.net/` `https://sharepoint-iddemo.msappproxy.net/hybrid/`, uygulama kullanıcıları doğrudan özel giriş sayfasına gider olarak değiştirirseniz.

   ```powershell
   $homepage = "https://sharepoint-iddemo.msappproxy.net/hybrid/"
   ```

1. Ana sayfanın güncelliğini yapın.

   ```powershell
   Set-AzureADApplication -ObjectId $objguid -Homepage $homepage
   ```

1. Değişikliğin başarılı olduğunu doğrulamak için aşağıdaki komutu 2.

   ```powershell
   Get-AzureADApplication -ObjectId $objguid | Format-List DisplayName, Homepage, ObjectId
   ```

   Örneğin, çıktı şimdi aşağıdaki gibi görünmelidir:

   ```console
   DisplayName : SharePoint
   Homepage    : https://sharepoint-iddemo.msappproxy.net/hybrid/
   ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
   ```

1. Ana sayfanın beklendiği gibi ilk ekran olarak göründüğünü doğrulamak için uygulamayı yeniden başlatın.

> [!NOTE]
> Uygulamada yaptığınız değişiklikler ana sayfa URL'sini sıfırlayabilir. Ana sayfaURL'niz sıfırlanırsa, geri ayarlamak için bu bölümdeki adımları yineleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD Uygulama Ara Sunucusu ile SharePoint’e uzaktan erişimi etkinleştirme](application-proxy-integrate-with-sharepoint-server.md)
- [Öğretici: Azure Active Directory'de Application Proxy aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](application-proxy-add-on-premises-application.md)