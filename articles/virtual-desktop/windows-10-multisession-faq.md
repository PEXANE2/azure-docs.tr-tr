---
title: Windows 10 Enterprise çoklu oturum SSS-Azure
description: Windows sanal masaüstü için Windows 10 Kurumsal Çoklu oturum kullanımı hakkında sık sorulan sorular ve en iyi uygulamalar.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 02/19/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 218ec6a5beab32729e106a7397d58ebaa2a30123
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86086934"
---
# <a name="windows-10-enterprise-multi-session-faq"></a>Windows 10 Enterprise çoklu oturum hakkında SSS

Bu makalede sık sorulan sorular yanıtlanmaktadır ve Windows 10 Kurumsal Çoklu oturum için en iyi yöntemler açıklanmaktadır.
 
## <a name="what-is-windows-10-enterprise-multi-session"></a>Windows 10 Enterprise çoklu oturum nedir?

Daha önce sanal masaüstleri için Windows 10 Enterprise (EVD) olarak bilinen Windows 10 Enterprise çoklu oturum, birden çok eşzamanlı etkileşimli oturuma izin veren yeni bir Uzak Masaüstü Oturumu Ana Bilgisayarı. Daha önce bu, yalnızca Windows Server tarafından gerçekleştirilebilir. Bu özellik, birden çok oturumun maliyet avantajlarından faydalanabileceği ve RDS Istemci erişim lisansları (CAL) yerine mevcut Kullanıcı başına Windows lisanslamayı kullanmanın yanı sıra kullanıcılara tanıdık bir Windows 10 deneyimi sağlar. Lisanslar ve fiyatlandırma hakkında daha fazla bilgi için bkz. [Windows sanal masaüstü fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-desktop/). 
 
## <a name="how-many-users-can-simultaneously-have-an-interactive-session-on-windows-10-enterprise-multi-session"></a>Windows 10 Enterprise çoklu oturumunda kaç kullanıcının aynı anda etkileşimli oturumu olabilir?

Aynı anda etkin olabilecek kaç etkileşimli oturum, sisteminizin donanım kaynaklarına (vCPU, bellek, disk ve vGPU) bağlıdır, kullanıcılarınız bir oturumda oturum açmış durumdayken uygulamalarını nasıl kullanır ve sisteminizin iş yükünün ne kadar ağır olduğunu. Windows 10 Enterprise çoklu oturumunda kaç Kullanıcı kullanabileceğinizi anlamak için sisteminizin performansını doğrulamanızı öneririz. Daha fazla bilgi için bkz. [Windows sanal masaüstü fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-desktop/). 
 
## <a name="why-does-my-application-report-windows-10-enterprise-multi-session-as-a-server-operating-system"></a>Uygulamamın neden Windows 10 Kurumsal Çoklu oturumunu sunucu işletim sistemi olarak raporlıyor?

Windows 10 Enterprise çoklu oturum, Windows 10 Enterprise 'ın sanal bir sürümüdür. Farklardan biri, bu işletim sisteminin (OS) [ProductType](/windows/win32/cimwin32prov/win32-operatingsystem) 'ı, Windows Server ile aynı değeri 3 değerine sahip olarak raporlarıdır. Bu özellik, işletim sisteminin mevcut RDSH yönetimi araçları, RDSH çoklu oturum kullanan uygulamalar ve en düşük düzeyde sistem performansı iyileştirmeleri olan RDSH ortamları ile uyumlu kalmasını önler. Bazı uygulama yükleyicileri, ProductType 'ın Istemci olarak ayarlandığını algılamadığına bağlı olarak Windows 10 çoklu oturumunda yüklemeyi engelleyebilir. Uygulamanız yüklenemezse, güncelleştirilmiş bir sürüm için uygulama satıcınıza başvurun. 
 
## <a name="can-i-run-windows-10-enterprise-multi-session-on-premises"></a>Windows 10 Enterprise çoklu oturum 'nı şirket içi çalıştırabilir miyim?

Windows 10 Enterprise çoklu oturum, Azure için Windows sanal masaüstü hizmeti için iyileştirildiğinden, şirket içi üretim ortamlarında çalıştırılamaz. Üretim amacıyla Azure dışında Windows 10 Kurumsal Çoklu oturum çalıştırmak için lisanslama anlaşmasıyla karşılaştırılır. Windows 10 Enterprise çoklu oturum, şirket içi anahtar yönetim hizmetleri 'nde (KMS) etkinleştirilemez.
 
## <a name="how-do-i-customize-the-windows-10-enterprise-multi-session-image-for-my-organization"></a>Windows 10 Enterprise çoklu oturum görüntüsünü Kuruluşum için Nasıl yaparım? özelleştirmek ister misiniz?

Azure 'da bir sanal makineyi (VM) Windows 10 Windows 10 Enterprise çoklu oturum ile başlatabilir ve LOB uygulamalarını yükleyerek, Sysprep/genelleştirin ve ardından Azure portal kullanarak bir görüntü oluşturabilirsiniz.  
 
Başlamak için, Windows 10 Enterprise çoklu oturum ile Azure 'da bir VM oluşturun. VM 'yi Azure 'da başlatmak yerine doğrudan VHD 'yi indirebilirsiniz. Bundan sonra, Hyper-V ' y i destekleyen bir Windows 10 bilgisayarında yeni nesil 1 VM oluşturmak için indirdiğiniz VHD 'yi kullanabileceksiniz.

LOB uygulamalarını ve Sysprep 'i yükleyerek görüntüyü gereksinimlerinize göre özelleştirin. Özelleştirmeyi bitirdiğinizde, içindeki VHD ile görüntüyü Azure 'a yükleyin. Bundan sonra, Azure Marketi 'nden Windows sanal masaüstü 'Nü alın ve özelleştirilmiş görüntüyle yeni bir konak havuzu dağıtmak için kullanın.
 
## <a name="how-do-i-manage-windows-10-enterprise-multi-session-after-deployment"></a>Dağıtımdan sonra Windows 10 Enterprise çoklu oturum 'nı yönetmek Nasıl yaparım? mı?

Desteklenen herhangi bir yapılandırma aracını kullanabilirsiniz, ancak Windows 10 Enterprise çoklu oturum 'nı desteklediğinden Configuration Manager sürüm 1906 önerilir. Şu anda Microsoft Intune desteği üzerinde çalışıyoruz.
 
## <a name="can-windows-10-enterprise-multi-session-be-azure-active-directory-ad-joined"></a>Windows 10 Enterprise çoklu oturum Azure Active Directory (AD) ile katılmış olabilir mi?

Windows 10 Enterprise çok oturumu şu anda karma Azure AD 'ye katılmış olarak desteklenmektedir. Windows 10 Enterprise çoklu oturum, etki alanına katılmış olduktan sonra Azure AD kaydı 'nı etkinleştirmek için mevcut grup ilkesi nesnesini kullanın. Daha fazla bilgi için bkz. [karma Azure Active Directory JOIN Uygulamanızı planlayın](../active-directory/devices/hybrid-azuread-join-plan.md).
 
## <a name="where-can-i-find-the-windows-10-enterprise-multi-session-image"></a>Windows 10 Enterprise çoklu oturum görüntüsünü nerede bulabilirim?

Windows 10 Enterprise çoklu oturum, Azure galerisidir. Bunu bulmak için Azure portal gidin ve sanal masaüstleri için Windows 10 Enterprise sürümünü arayın. Kurumsal Microsoft 365 uygulamalarla tümleşik bir görüntü için Azure portal gidin ve **Enterprise Için Microsoft Windows 10 + Microsoft 365 uygulamalarını**arayın.

## <a name="which-windows-10-enterprise-multi-session-image-should-i-use"></a>Hangi Windows 10 Kurumsal Çoklu oturum görüntüsünü kullanmalıyım?

Azure Galerisi 'nde Windows 10 Enterprise multi-session, sürüm 1809 ve Windows 10 Enterprise multi-session, sürüm 1903 gibi çeşitli yayınlar vardır. Daha iyi performans ve güvenilirlik için en son sürümü kullanmanızı öneririz.
 
## <a name="which-windows-10-enterprise-multi-session-versions-are-supported"></a>Hangi Windows 10 Kurumsal Çoklu oturum sürümleri desteklenir?

Windows 10 Enterprise multi-session, 1809 ve üzeri sürümler desteklenmektedir ve Azure galerisinde kullanılabilir. Bu sürümler, Windows 10 Enterprise ile aynı destek yaşam döngüsü ilkesini izler, bu da Mart sürümünün 18 ay ve Eylül yayını için 30 ay içinde desteklendiği anlamına gelir.
 
## <a name="which-profile-management-solution-should-i-use-for-windows-10-enterprise-multi-session"></a>Windows 10 Enterprise çoklu oturum için hangi profil yönetimi çözümünü kullanmalıyım?

Windows 10 Enterprise 'ı kalıcı olmayan ortamlarda veya merkezi olarak depolanmış bir profile ihtiyaç duyulan diğer senaryolarda yapılandırırken FSLogix profil kapsayıcılarını kullanmanızı öneririz. FSLogix, Kullanıcı profilinin her Kullanıcı oturumu için kullanılabilir ve güncel olmasını sağlar. Ayrıca, uygun izinlere sahip herhangi bir SMB paylaşımında bir kullanıcı profilini depolamak için FSLogix profil kapsayıcınızı kullanmanızı öneririz, ancak gerekirse Azure sayfa BLOB depolama alanında Kullanıcı profillerini saklayabilirsiniz. Windows sanal masaüstü kullanıcıları FSLogix 'i ek bir ücret olmadan kullanabilir.
 
FSLogix profil kapsayıcısını yapılandırma hakkında daha fazla bilgi için bkz. [FSLogix profil kapsayıcısını yapılandırma](create-host-pools-user-profile.md#configure-the-fslogix-profile-container).  

## <a name="which-license-do-i-need-to-access-windows-10-enterprise-multi-session"></a>Windows 10 Enterprise çoklu oturumuna erişmek için hangi lisansa ihtiyacım var?

Geçerli lisansların tam listesi için bkz. [Windows sanal masaüstü fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-desktop/).

## <a name="why-do-my-apps-disappear-after-i-sign-out"></a>Oturumu kapattıktan sonra Uygulamalarım neden kayboluyor?

Bu durum, FSLogix gibi bir profil yönetimi çözümüyle Windows 10 Kurumsal Çoklu oturum kullanıyor olmanız nedeniyle oluşur. Yönetici veya profil çözümünüz, kullanıcılar oturumu kapattığınızda kullanıcı profillerini silmek için sisteminizi yapılandırdı. Bu yapılandırma, oturumunuzu kapattıktan sonra sistem Kullanıcı profilinizi sildiğinde, oturumunuz sırasında yüklediğiniz tüm uygulamaları da kaldıran anlamına gelir. Yüklediğiniz uygulamaları korumak istiyorsanız, yöneticinizden Windows sanal masaüstü ortamınızdaki tüm kullanıcılar için bu uygulamaları sağlamasını istemeniz gerekir.

## <a name="how-do-i-make-sure-apps-dont-disappear-when-users-sign-out"></a>Nasıl yaparım? kullanıcılar oturumunu kapattığınızda uygulamaların kaybolmadığından emin olun.

En çok sanallaştırılmış ortamlar, kullanıcıların profillerine ek uygulamalar yüklemesini engellemek için varsayılan olarak yapılandırılır. Kullanıcı Windows Sanal Masaüstü oturumunu kapattığında uygulamanın kaybolmadığından emin olmak istiyorsanız, ortamınızdaki tüm Kullanıcı profilleri için bu uygulamayı sağlamanız gerekir. Uygulama sağlama hakkında daha fazla bilgi için şu kaynaklara göz atın:

- [Windows sanal masaüstünde yerleşik uygulamaları yayımlama](publish-apps.md)
- [DıSM uygulama paketi hizmet komut satırı seçenekleri](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-app-package--appx-or-appxbundle--servicing-command-line-options)
- [Add-AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/add-appxprovisionedpackage?view=win10-ps)

## <a name="how-do-i-make-sure-users-dont-download-and-install-apps-from-the-microsoft-store"></a>Nasıl yaparım? kullanıcıların Microsoft Store uygulamaları indirmeyin ve yüklememelerini sağlayın mi?

Kullanıcıların daha önce sağladığınız uygulamaların ötesinde ek uygulamalar indirmelerini sağlamak için Microsoft Store uygulamasını devre dışı bırakabilirsiniz.

Mağaza uygulamasını devre dışı bırakmak için:

1. Yeni bir grup ilkesi oluşturun.
2. **Computer Configuration**  >  **Administrative Templates**  >  **Windows bileşenleri**Yönetim Şablonları bilgisayar yapılandırması ' nı seçin.
3. **Mağaza**' ı seçin.
4. **Mağaza uygulamasını**seçin.
5. **Devre dışı**seçeneğini belirleyip **Tamam**' ı seçin.
6. **Uygula**’yı seçin.
 
## <a name="next-steps"></a>Sonraki adımlar

Windows sanal masaüstü ve Windows 10 Enterprise çoklu oturum hakkında daha fazla bilgi edinmek için:

- [Windows sanal masaüstü önizleme belgelerimizi](overview.md) okuyun
- [Windows sanal masaüstü teknik topluluğumuzu](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) ziyaret edin
- Windows sanal masaüstü [öğreticileri](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md) ile Windows sanal masaüstü dağıtımınızı ayarlama
