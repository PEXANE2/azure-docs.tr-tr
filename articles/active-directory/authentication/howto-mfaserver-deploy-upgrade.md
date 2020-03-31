---
title: Azure MFA Server'ı Yükseltme - Azure Etkin Dizini
description: Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'nu daha yeni bir sürüme yükseltme adımları ve kılavuzu.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 756c45541907c52448805376e1b054180c31fdf5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74848111"
---
# <a name="upgrade-to-the-latest-azure-multi-factor-authentication-server"></a>En son Azure Multi-Factor Authentication Sunucusu’na yükseltme

Bu makale, Azure Çok Faktörlü Kimlik Doğrulama (MFA) Server v6.0 veya daha yüksek yükseltme sürecinde size yol kat eder. PhoneFactor Aracısı'nın eski bir sürümünü yükseltmeniz gerekiyorsa, [PhoneFactor Aracısını Azure Çok Faktörlü Kimlik Doğrulama Sunucusuna Yükselt'e](howto-mfaserver-deploy-upgrade-pf.md)bakın.

V6.x'ten veya daha eskiye veya daha yeniye yükseltiyorsanız, tüm bileşenler .NET 2.0'dan .NET 4.5'e değişir. Tüm bileşenler ayrıca Microsoft Visual C++ 2015 Yeniden Dağıtılabilir Güncelleştirme 1 veya üzeri gerektirir. MFA Server yükleyici, bu bileşenlerin hem x86 hem de x64 sürümlerini yüklenmemişse yükler. Kullanıcı Portalı ve Mobil Uygulama Web Hizmeti ayrı sunucularda çalıştırılırsa, bu bileşenleri yükseltmeden önce bu paketleri yüklemeniz gerekir. [Microsoft Download Center'daki](https://www.microsoft.com/download/)en son Microsoft Visual C++ 2015 Yeniden Dağıtılabilir güncelleştirmesini arayabilirsiniz. 

> [!IMPORTANT]
> 1 Temmuz 2019 itibariyle Microsoft, yeni dağıtımlar için Artık MFA Server sunmayacak. Kullanıcılarından çok faktörlü kimlik doğrulaması isteyen yeni müşteriler bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmalıdır. 1 Temmuz'dan önce MFA Server'ı etkinleştirmiş olan mevcut müşteriler en son sürümü, gelecekteki güncelleştirmeleri karşıdan yükleyebilecek ve her zamanki gibi etkinleştirme kimlik bilgilerini oluşturabilecek.

Yükseltme adımlarını bir bakışta yükseltin:

* Azure MFA Sunucularını Yükseltme (Astlar sonra Ana)
* Kullanıcı Portalı örneklerini yükseltme
* AD FS Bağdaştırıcı örneklerini yükseltin

## <a name="upgrade-azure-mfa-server"></a>Azure MFA Server'ı Yükselt

1. Azure MFA Server yükleyicisinin en son sürümünü almak için [Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'nu İndir'deki](howto-mfaserver-deploy.md#download-the-mfa-server) yönergeleri kullanın.
2. C:\Program Files\Multi-Factor Authentication Server\Data\PhoneFactor.pfdata adresinde bulunan MFA Server veri dosyasının yedekini (varsayılan yükleme konumunu varsayarak) ana MFA Server'ınızda yapın.
3. Yüksek kullanılabilirlik için birden çok sunucu çalıştırıyorsanız, yükseltme yapan sunuculara trafik göndermeyi durduracak şekilde kimlik doğrulaması yapan istemci sistemlerini MFA Sunucusu'na değiştirin. Yük dengeleyicisi kullanıyorsanız, alt takim mfa sunucusunu yük bakiyesinden kaldırın, yükseltmeyi yapın ve sunucuyu çiftliğe geri ekleyin.
4. Her MFA Sunucusundaki yeni yükleyiciyi çalıştırın. Ana bilgisayar tarafından çoğaltılan eski veri dosyasını okuyabildikleri için önce alt sunucuları yükseltin.

   > [!NOTE]
   > Bir sunucuyu yükseltirken, diğer MFA Sunucularıyla herhangi bir yük dengeleme veya trafik paylaşımından kaldırılmalıdır.
   >
   > Yükleyiciyi çalıştırmadan önce geçerli MFA Sunucunuzu kaldırmanız gerekmez. Yükleyici yerinde yükseltme yapar. Yükleme yolu önceki yüklemeden kayıt defterinden alınır, böylece aynı konuma yüklenir (örneğin, C:\Program Files\Multi-Factor Authentication Server).
  
5. Microsoft Visual C++ 2015 Yeniden Dağıtılabilir güncelleştirme paketini yüklemeniz istenirse, istemi kabul edin. Paketin hem x86 hem de x64 versiyonları yüklenir.
6. Web Hizmeti SDK'yı kullanıyorsanız, yeni Web Hizmeti SDK'sını yüklemeniz istenir. Yeni Web Hizmeti SDK'sını yüklediğinizde, sanal dizin adının önceden yüklenmiş sanal dizine (örneğin, MultiFactorAuthWebServiceSdk) eşleştiğinden emin olun.
7. Tüm alt sunucularda adımları yineleyin. Astlardan birini yeni ana usta olarak tanıtın ve eski ana sunucuyu yükseltin.

## <a name="upgrade-the-user-portal"></a>Kullanıcı Portalını Yükselt

Bu bölüme geçmeden önce MFA Sunucularınızın yükseltmesini tamamlayın.

1. Kullanıcı Portalı yükleme konumunun sanal dizininde bulunan web.config dosyasının yedeğini yapın (örneğin, C:\inetpub\wwwroot\MultiFactorAuth). Varsayılan temada herhangi bir değişiklik yapıldıysa, App_Themes\Varsayılan klasörünün de yedekte olmasını sağlayabilir. Varsayılan klasörün bir kopyasını oluşturmak ve Varsayılan temayı değiştirmekyerine yeni bir tema oluşturmak daha iyidir.
2. Kullanıcı Portalı diğer MFA Server bileşenleriyle aynı sunucuda çalışıyorsa, MFA Server yüklemesi Kullanıcı Portalını güncelleştirmenizi ister. İstemi kabul edin ve Kullanıcı Portalı güncelleştirmesini yükleyin. Sanal dizin adının daha önce yüklenmiş sanal dizine (örneğin, MultiFactorAuth) eşleşip eşleşmediğini denetleyin.
3. Kullanıcı Portalı kendi sunucusundaysa, MultiFactorAuthenticationUserPortalSetup64.msi dosyasını MFA Sunucularından birinin yükleme konumundan kopyalayın ve Kullanıcı Portalı web sunucusuna koyun. Yükleyiciyi çalıştırın.

   "Microsoft Visual C++ 2015 Yeniden Dağıtılabilir Güncelleştirme 1 veya üzeri gereklidir" yazan bir hata oluşursa, [Microsoft Download Center'dan](https://www.microsoft.com/download/)en son güncelleştirme paketini indirin ve yükleyin. Hem x86 hem de x64 sürümlerini yükleyin.

4. Güncelleştirilmiş Kullanıcı Portalı yazılımı yüklendikten sonra, adım 1'de yaptığınız web.config yedeklemesini yeni web.config dosyasıyla karşılaştırın. Yeni web.config'de yeni öznitelikler yoksa, yeni web.config'ini yenisinin üzerine yazmak için sanal dizine kopyalayın. Başka bir seçenek, appSettings değerlerini ve Web Hizmeti SDK URL'sini yedekleme dosyasından yeni web.config'e kopyalamak/yapıştırmaktır.

Birden çok sunucuda Kullanıcı Portalı varsa, yüklemeyi hepsinde yineleyin.

## <a name="upgrade-the-mobile-app-web-service"></a>Mobil Uygulama Web Hizmetini Yükseltin

> [!NOTE]
> Azure MFA Server'ın 8,0'dan 8,0+ daha eski bir sürümünden yükseltme yaparken, yükseltmeden sonra mobil uygulama web hizmeti kaldırılabilir

## <a name="upgrade-the-ad-fs-adapters"></a>AD FS Adaptörlerini Yükseltin

Bu bölüme geçmeden önce MFA Sunucularınızın ve Kullanıcı Portalınızın yükseltmesini tamamlayın.

### <a name="if-mfa-runs-on-different-servers-than-ad-fs"></a>MFA, AD FS'den farklı sunucularda çalışıyorsa

Bu yönergeler yalnızca Çok Faktörlü Kimlik Doğrulama Sunucusu'nu AD FS sunucularınızdan ayrı olarak çalıştırıyorsanız geçerlidir. Her iki hizmet de aynı sunucularda çalışıyorsa, bu bölümü atlayın ve yükleme adımlarına gidin. 

1. AD FS'de kayıtlı olan MultiFactorAuthenticationAdfsAdapter.config dosyasının bir kopyasını kaydedin veya `Export-AdfsAuthenticationProviderConfigurationData -Name [adapter name] -FilePath [path to config file]`aşağıdaki PowerShell komutunu kullanarak yapılandırmayı dışa aktarın: . Bağdaştırıcı adı, daha önce yüklenen sürüme bağlı olarak "WindowsAzureMultiFactorAuthentication" veya "AzureMfaServerAuthentication" dır.
2. Aşağıdaki dosyaları MFA Server yükleme konumundan AD FS sunucularına kopyalayın:

   * MultiFactorAuthenticationAdfsAdapterSetup64.msi
   * Register-MultiFactorAuthenticationAdfsAdapter.ps1
   * Unregister-MultiFactorAuthenticationAdfsAdapter.ps1
   * MultiFactorAuthenticationAdfsAdapter.config

3. Register-MultiFactorAuthenticationAdfsAdapter.ps1 komutunu komutun `-ConfigurationFilePath [path]` sonuna ekleyerek düzenleme. `Register-AdfsAuthenticationProvider` *[yol]* multifactorAuthenticationAdfsAdapter.config dosyası veya yapılandırma dosyası önceki adımda dışa aktarılan tam yol ile değiştirin.

   Eski config dosyasıyla eşleşip eşleşmediklerini görmek için yeni MultiFactorAuthenticationAdfsAdapter.config'deki öznitelikleri kontrol edin. Yeni sürümde öznitelik eklendiyse veya kaldırıldıysa, öznitelik değerlerini eski yapılandırma dosyasından yenisine kopyalayın veya eski yapılandırma dosyasını eşleşecek şekilde değiştirin.

### <a name="install-new-ad-fs-adapters"></a>Yeni AD FS bağdaştırıcılarını yükleyin

> [!IMPORTANT]
> Kullanıcılarınızın bu bölümün 3-8 adımları sırasında iki aşamalı doğrulama gerçekleştirmeleri gerekmez. Ad FS'niz birden çok kümede yapılandırıldıysa, kapalı kalma süresini önlemek için çiftlikteki her kümeyi diğer kümelerden bağımsız olarak kaldırabilir, yükseltebilir ve geri yükleyebilirsiniz.

1. Bazı AD FS sunucularını çiftlikten kaldırın. Diğerleri çalışırken bu sunucuları güncelleştirin.
2. AD FS çiftliğinden kaldırılan her sunucuya yeni AD FS bağdaştırıcısını yükleyin. MFA Server her AD FS sunucusuna yüklenmişse, MFA Server yöneticisi UX aracılığıyla güncelleyebilirsiniz. Aksi takdirde, MultiFactorAuthenticationAdfsAdapterSetup64.msi çalıştırarak güncelleştirin.

   "Microsoft Visual C++ 2015 Yeniden Dağıtılabilir Güncelleştirme 1 veya üzeri gereklidir" yazan bir hata oluşursa, [Microsoft Download Center'dan](https://www.microsoft.com/download/)en son güncelleştirme paketini indirin ve yükleyin. Hem x86 hem de x64 sürümlerini yükleyin.

3. AD **FS** > **Kimlik Doğrulama İlkelerini** > Küresel**MultiFactor Kimlik Doğrulama İlkesini Düzenleme'ye**gidin. **WindowsAzureMultiFactorAuthentication** veya **AzureMFAServerAuthentication** (yüklü geçerli sürüme bağlı olarak) uncheck.

   Bu adım tamamlandıktan sonra, MFA Server üzerinden iki aşamalı doğrulama, adım 8'i tamamlayana kadar bu AD FS kümesinde kullanılamaz.

4. Unregister-MultiFactorAuthenticationAdfsAdapter.ps1 PowerShell komut dosyası çalıştırarak AD FS bağdaştırıcısının eski sürümünü niçin silin. *-Ad* parametresi ("WindowsAzureMultiFactorAuthenticAuthentication" veya "AzureMFAServerAuthentication") adım 3'te görüntülenen adla eşleştiğinden emin olun. Bu, merkezi bir yapılandırma olduğundan, aynı AD FS kümesindeki tüm sunucular için geçerlidir.
5. Register-MultiFactorAuthenticationAdfsAdapter.ps1 PowerShell komut dosyasını çalıştırarak yeni AD FS adaptörkaydedin. Bu, merkezi bir yapılandırma olduğundan, aynı AD FS kümesindeki tüm sunucular için geçerlidir.
6. AD FS çiftliğinden kaldırılan her sunucudaki AD FS hizmetini yeniden başlatın.
7. Güncelleştirilmiş sunucuları AD FS ekine geri ekleyin ve diğer sunucuları çiftlikten kaldırın.
8. AD **FS** > **Kimlik Doğrulama İlkelerini** > Küresel**MultiFactor Kimlik Doğrulama İlkesini Düzenleme'ye**gidin. **AzureMfaServerAuthentication'ı kontrol edin.**
9. Şimdi AD FS çiftliğinden kaldırılan sunucuları güncelleştirmek ve bu sunucularda AD FS hizmetini yeniden başlatmak için adım 2'yi yineleyin.
10. Bu sunucuları AD FS çiftliğine geri ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Çok Faktörlü Kimlik Doğrulama ve üçüncü taraf VPN'leri ile Gelişmiş senaryoörnekleri](howto-mfaserver-nps-vpn.md) alın

* [MFA Server'ı Windows Server Active Directory ile senkronize edin](howto-mfaserver-dir-ad.md)

* Uygulamalarınız için [Windows Kimlik Doğrulaması'nı yapılandırma](howto-mfaserver-windows.md)
