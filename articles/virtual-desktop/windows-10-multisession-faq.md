---
title: Windows 10 Kurumsal çok oturumlu SSS - Azure
description: Windows Sanal Masaüstü için Windows 10 Enterprise çoklu oturumunu kullanmak için sık sorulan sorular ve en iyi uygulamalar.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 02/19/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 4301aaed0152e07eb2a941b56637239b66c33878
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127364"
---
# <a name="windows-10-enterprise-multi-session-faq"></a>Windows 10 Enterprise çoklu oturum hakkında SSS

Bu makalede, sık sorulan soruları yanıtlar ve Windows 10 Enterprise çoklu oturumu için en iyi uygulamaları açıklar.
 
## <a name="what-is-windows-10-enterprise-multi-session"></a>Windows 10 Enterprise çoklu oturumu nedir?

Windows 10 Enterprise çok oturumlu, eski adıyla Windows 10 Enterprise for Virtual Desktops (EVD), birden çok eşzamanlı etkileşimli oturuma izin veren yeni bir Uzak Masaüstü Oturum Ana Bilgisayarıdır. Daha önce, bunu yalnızca Windows Server yapabilirdi. BU özellik kullanıcılara tanıdık bir Windows 10 deneyimi sunarken, BT çok oturumlu maliyet avantajlarından yararlanabilir ve RDS İstemci Erişim Lisansları (CALs) yerine mevcut kullanıcı başına Windows lisansını kullanabilir. Lisanslar ve fiyatlandırma hakkında daha fazla bilgi için [Windows Sanal Masaüstü fiyatlandırması'na](https://azure.microsoft.com/pricing/details/virtual-desktop/)bakın. 
 
## <a name="how-many-users-can-simultaneously-have-an-interactive-session-on-windows-10-enterprise-multi-session"></a>Windows 10 Enterprise çoklu oturumunda aynı anda kaç kullanıcı etkileşimli oturum yapabilir?

Aynı anda etkin olabilecek kaç etkileşimli oturum, sisteminizin donanım kaynaklarına (vCPU, bellek, disk ve vGPU), kullanıcılarınızın oturumda oturum alırken uygulamalarını nasıl kullandığına ve sisteminizin iş yükünün ne kadar ağır olduğuna bağlıdır. Windows 10 Enterprise çoklu oturumunda kaç kullanıcıya sahip olabileceğinizi anlamak için sisteminizin performansını doğrulamanızı öneririz. Daha fazla bilgi için [Windows Sanal Masaüstü fiyatlandırmasına](https://azure.microsoft.com/pricing/details/virtual-desktop/)bakın. 
 
## <a name="why-does-my-application-report-windows-10-enterprise-multi-session-as-a-server-operating-system"></a>Uygulamam neden Windows 10 Enterprise çoklu oturumunu sunucu işletim sistemi olarak bildiriyor?

Windows 10 Enterprise çoklu oturumu, Windows 10 Enterprise'ın sanal bir sürümüdür. Farklardan biri, bu işletim sisteminin (OS) [ProductType'ı](/windows/win32/cimwin32prov/win32-operatingsystem) Windows Server ile aynı değere sahip 3 olarak bildirmesidir. Bu özellik, işletim sistemini mevcut RDSH yönetim takımlarıyla, RDSH çok oturumlu uygulamalarla ve rdsh ortamları için çoğunlukla düşük düzeyli sistem performansı optimizasyonlarıyla uyumlu tutar. Bazı uygulama yükleyiciler, ProductType'ın Istemci olarak ayarlanıp ayarlıolduğunu algılayıp algılamadıklarına bağlı olarak Windows 10 çoklu oturumda yüklemeyi engelleyebilir. Uygulamanız yüklenmiyorsa, güncelleştirilmiş bir sürüm için uygulama satıcınıza başvurun. 
 
## <a name="can-i-run-windows-10-enterprise-multi-session-on-premises"></a>Windows 10 Enterprise çoklu oturumlarını şirket içinde çalıştırabilir miyim?

Windows 10 Enterprise çoklu oturumu, Azure için Windows Sanal Masaüstü hizmeti için optimize edilemediği için şirket içi üretim ortamlarında çalıştırılamıyor. Windows 10 Enterprise çoklu oturumunu üretim amacıyla Azure dışında çalıştırmak lisans anlaşmasına aykırıdır. Windows 10 Enterprise çoklu oturumu şirket içi Anahtar Yönetim Hizmetleri 'ne (KMS) karşı etkinleştirilmez.
 
## <a name="how-do-i-customize-the-windows-10-enterprise-multi-session-image-for-my-organization"></a>Kuruluşum için Windows 10 Enterprise çok oturumlu görüntüyü nasıl özelleştirebilirim?

Windows 10 Windows 10 Enterprise çoklu oturumlu Azure'da sanal bir makine (VM) başlatabilir ve LOB uygulamalarını yükleyerek özelleştirebilir, sysprep/genelleme yapabilir ve Azure portalını kullanarak bir resim oluşturabilirsiniz.  
 
Başlamak için Windows 10 Windows 10 Enterprise çoklu oturumu yla Azure'da bir VM oluşturun. VM'yi Azure'da başlatmak yerine, VHD'yi doğrudan indirebilirsiniz. Bundan sonra, Hyper-V etkin bir Windows 10 pc'de yeni bir Nesil 1 VM oluşturmak için indirdiğiniz VHD'yi kullanabilirsiniz.

LOB uygulamalarını yükleyerek görüntüyü ihtiyaçlarınıza göre özelleştirin ve görüntüyü sysprep. Özelleştirmeyi bitirdiğinizde, içindeki VHD ile görüntüyü Azure'a yükleyin. Bundan sonra, Azure Marketi'nden Windows Sanal Masaüstü'nü alın ve özelleştirilmiş görüntüyle yeni bir ana bilgisayar havuzu dağıtmak için kullanın.
 
## <a name="how-do-i-manage-windows-10-enterprise-multi-session-after-deployment"></a>Dağıtımdan sonra Windows 10 Enterprise çoklu oturumunu nasıl yönetirim?

Desteklenen herhangi bir yapılandırma aracını kullanabilirsiniz, ancak Windows 10 Enterprise çoklu oturumunu desteklediği için Configuration Manager sürüm 1906'yı öneririz. Şu anda Microsoft Intune desteği üzerinde çalışıyoruz.
 
## <a name="can-windows-10-enterprise-multi-session-be-azure-active-directory-ad-joined"></a>Windows 10 Enterprise çok oturumlu Azure Etkin Dizin (AD) birleştirilmiş olabilir mi?

Windows 10 Enterprise çoklu oturumu şu anda karma Azure AD'ye katılmak üzere desteklenir. Windows 10 Enterprise çok oturumlu etki alanına katıldıktan sonra, Azure REKLAM kaydını etkinleştirmek için varolan Grup İlkesi Nesnesi'ni kullanın. Daha fazla bilgi için bkz: [Karma Azure Etkin Dizininizi planlayın.](../active-directory/devices/hybrid-azuread-join-plan.md)
 
## <a name="where-can-i-find-the-windows-10-enterprise-multi-session-image"></a>Windows 10 Enterprise çok oturumlu görüntüyü nerede bulabilirim?

Windows 10 Enterprise çoklu oturumu Azure galerisindedir. Bulmak için Azure portalına gidin ve Sanal Masaüstü için Windows 10 Enterprise sürümünde arama yapın. Office Pro Plus ile entegre edilmiş bir görüntü için Azure portalına gidin ve Microsoft Windows 10 + Office 365 ProPlus'ı arayın.

## <a name="which-windows-10-enterprise-multi-session-image-should-i-use"></a>Hangi Windows 10 Enterprise çok oturumlu görüntüyü kullanmalıyım?

Azure galerisinde Windows 10 Enterprise çok oturumlu, sürüm 1809 ve Windows 10 Enterprise çok oturumlu sürüm 1903 dahil olmak üzere çeşitli sürümler bulunmaktadır. Gelişmiş performans ve güvenilirlik için en son sürümü kullanmanızı öneririz.
 
## <a name="which-windows-10-enterprise-multi-session-versions-are-supported"></a>Hangi Windows 10 Enterprise çok oturumlu sürümleri desteklenir?

Windows 10 Enterprise çok oturumlu, sürüm 1809 ve sonrası desteklenir ve Azure galerisinde kullanılabilir. Bu sürümler, Windows 10 Enterprise ile aynı destek yaşam döngüsü ilkesini izler, bu da bahar sürümü18 ay ve sonbahar sürümü 30 ay boyunca desteklendiği anlamına gelir.
 
## <a name="which-profile-management-solution-should-i-use-for-windows-10-enterprise-multi-session"></a>Windows 10 Enterprise çoklu oturumu için hangi profil yönetimi çözümlerini kullanmalıyım?

Windows 10 Enterprise'ı kalıcı olmayan ortamlarda veya merkezi olarak depolanmış bir profil gerektiren diğer senaryolarda yapılandırırken FSLogix profil kapsayıcılarını kullanmanızı öneririz. FSLogix, kullanıcı profilinin her kullanıcı oturumu için kullanılabilir ve güncel olmasını sağlar. Ayrıca, bir kullanıcı profilini uygun izinlerle herhangi bir Kobİ paylaşımında depolamak için FSLogix profil kapsayıcınızı kullanmanızı öneririz, ancak gerekirse kullanıcı profillerini Azure sayfası blob depolama alanında depolayabilirsiniz. Windows Sanal Masaüstü kullanıcıları FSLogix'i ek ücret ödemeden kullanabilir.
 
Bir FSLogix profil kapsayıcısının nasıl yapılandırılabildiğini hakkında daha fazla bilgi [için](create-host-pools-user-profile.md#configure-the-fslogix-profile-container)bkz.  

## <a name="which-license-do-i-need-to-access-windows-10-enterprise-multi-session"></a>Windows 10 Enterprise çoklu oturumuna erişmek için hangi lisansa ihtiyacım var?

Geçerli lisansların tam listesi için [Windows Sanal Masaüstü fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-desktop/)bölümüne bakın.

## <a name="why-do-my-apps-disappear-after-i-sign-out"></a>Oturum dışı çıktıktan sonra uygulamalarım neden kayboluyor?

Bunun nedeni, Windows 10 Enterprise çoklu oturumunu FSLogix gibi bir profil yönetimi çözümüyle kullandığınız için gerçekleşir. Yöneticiniz veya profil çözümünüz, kullanıcılar oturumunuzu tamamladığında kullanıcı profillerini silmek için sisteminizi yapılandırmış. Bu yapılandırma, oturumu nuzdan sonra sisteminiz kullanıcı profilinizi sildiğinde, oturumunuz sırasında yüklediğiniz uygulamaları da kaldırdığı anlamına gelir. Yüklediğiniz uygulamaları tutmak istiyorsanız, yöneticinizden bu uygulamaları Windows Sanal Masaüstü ortamınızdaki tüm kullanıcılar için sağlamasını istemeniz gerekir.

## <a name="how-do-i-make-sure-apps-dont-disappear-when-users-sign-out"></a>Kullanıcılar oturum dışı kaldığında uygulamaların kaybolmadığından nasıl emin olabilirim?

Sanallaştırılmış ortamların çoğu, kullanıcıların profillerine ek uygulamalar yüklemesini önlemek için varsayılan olarak yapılandırılır. Kullanıcınız Windows Sanal Masaüstü'nden çıkış yaptığında bir uygulamanın kaybolmadığından emin olmak istiyorsanız, bu uygulamayı ortamınızdaki tüm kullanıcı profilleri için sağlamanız gerekir. Uygulama sağlama hakkında daha fazla bilgi için şu kaynaklara göz atın:

- [Windows Sanal Masaüstü'nde yerleşik uygulamalar yayınlama](publish-apps.md)
- [DISM uygulama paketi servis komut satırı seçenekleri](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-app-package--appx-or-appxbundle--servicing-command-line-options)
- [Add-AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/add-appxprovisionedpackage?view=win10-ps)

## <a name="how-do-i-make-sure-users-dont-download-and-install-apps-from-the-microsoft-store"></a>Kullanıcıların Microsoft Mağazası'ndan uygulama indirip yüklemediğinden nasıl emin olabilirim?

Kullanıcıların kendileri için zaten sağlamış olduğunuz uygulamaların dışında fazladan uygulama indirmediğinden emin olmak için Microsoft Mağazası uygulamasını devre dışı bırakabilirsiniz.

Mağaza uygulamasını devre dışı kalmak için:

1. Yeni bir Grup İlkesi oluşturun.
2. **Bilgisayar Yapılandırma** > **Yönetim Şablonları** > **Windows Bileşenleri'ni**seçin.
3. **Mağaza'yı**seçin.
4. **Mağaza Uygulamasını**seçin.
5. **Devre Dışı'yı**seçin, ardından **Tamam'ı**seçin.
6. **Uygula**’yı seçin.
 
## <a name="next-steps"></a>Sonraki adımlar

Windows Sanal Masaüstü ve Windows 10 Enterprise çoklu oturumu hakkında daha fazla bilgi edinmek için:

- Windows [Sanal Masaüstü Önizleme belgelerimizi](overview.md) okuyun
- Windows [Virtual Desktop TechCommunity'imizi](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) ziyaret edin
- Windows Sanal Masaüstü [eğitimleriyle](tenant-setup-azure-active-directory.md) Windows Sanal Masaüstü dağıtımınızı ayarlama
