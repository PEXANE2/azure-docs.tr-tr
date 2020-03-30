---
title: Karma Azure Active Directory join planı - Azure Active Directory
description: Hibrit Azure Active Directory'ye katılmış cihazları elle nasıl yapılandıracağınızı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 152ff52ce52b573d7f24cbb2fafc944b1794f6d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80129252"
---
# <a name="how-to-plan-your-hybrid-azure-active-directory-join-implementation"></a>Nasıl Olunması: Karma Azure Etkin Dizininizi uygulamayla birleştirin

Kullanıcıya benzer bir şekilde, aygıt, kaynaklarınızı istediğiniz zaman ve herhangi bir konumdan korumak ve kullanmak istediğiniz başka bir temel kimliktir. Aşağıdaki yöntemlerden birini kullanarak Azure AD'ye aygıt kimliklerini getirerek ve yöneterek bu hedefe ulaşabilirsiniz:

- Azure AD'ye katılım
- Hibrit Azure AD'ye katılım
- Azure AD kaydı

Cihazlarınızı Azure AD'ye taşıyarak, çoklu oturum açma (SSO) özelliği sayesinde bulut ve şirket içi kaynaklarınız genelinde kullanıcılarınızın üretkenliğini en üst düzeye çıkarırsınız. Aynı zamanda, [Koşullu Erişim](../active-directory-conditional-access-azure-portal.md)ile bulutve şirket içi kaynaklarınıza erişimi güvence altına alabilirsiniz.

Şirket içinde Bir Active Directory (AD) ortamınız varsa ve AD etki alanı yla birleştirilmiş bilgisayarlarınıza Azure AD'ye katılmak istiyorsanız, karma Azure AD join yaparak bunu başarabilirsiniz. Bu makalede, ortamınızda karma bir Azure REKLAM join uygulamak için ilgili adımları sağlar. 

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, [Azure Etkin Dizin'de aygıt kimlik yönetimine Giriş'e](../device-management-introduction.md)aşina olduğunuzu varsayar.

> [!NOTE]
> Windows 10 karma Azure AD join için gereken minimum etki alanı denetleyici sürümü Windows Server 2008 R2'dir.

## <a name="plan-your-implementation"></a>Uygulamanızı planlayın

Karma Azure AD uygulamanızı planlamak için şunları aramalısınız:

|   |   |
| --- | --- |
| ![İşaretli][1] | Desteklenen cihazları gözden geçirme |
| ![İşaretli][1] | Bilmeniz gereken leri gözden geçirin |
| ![İşaretli][1] | Karma Azure AD join'in kontrollü geçerliliğini gözden geçirme |
| ![İşaretli][1] | Kimlik altyapınıza göre senaryonuzu seçin |
| ![İşaretli][1] | Karma Azure REKLAM join için şirket içi AD UPN desteğini gözden geçirin |

## <a name="review-supported-devices"></a>Desteklenen cihazları gözden geçirme

Karma Azure AD join, çok çeşitli Windows aygıtlarını destekler. Windows'un eski sürümlerini çalıştıran aygıtlar için yapılandırma ek veya farklı adımlar gerektirdiğinden, desteklenen aygıtlar iki kategoriye ayrılır:

### <a name="windows-current-devices"></a>Windows geçerli aygıtlar

- Windows 10
- Windows Server 2016
  - **Not**: Azure Ulusal bulut müşterileri sürüm 1809 gerektirir
- Windows Server 2019

Windows masaüstü işletim sistemini çalıştıran aygıtlar için desteklenen sürüm bu makalede [Windows 10 sürüm bilgileri](/windows/release-information/)listelenir. En iyi yöntem olarak Microsoft, Windows 10'un en son sürümüne yükseltmenizi önerir.

### <a name="windows-down-level-devices"></a>Windows alt düzey aygıtlar

- Windows 8.1
- Windows 7 desteği 14 Ocak 2020 tarihinde sona erdi. Daha fazla bilgi için bkz. [Windows 7 desteği sona erdi.](https://support.microsoft.com/en-us/help/4057281/windows-7-support-ended-on-january-14-2020)
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2. Windows Server 2008 ve 2008 R2 hakkında destek bilgileri için windows [server 2008 desteği nin sonuna hazırla'ya](https://www.microsoft.com/cloud-platform/windows-server-2008)bakın.

İlk planlama adımı olarak, ortamınızı gözden geçirmeli ve Windows alt düzey aygıtları desteklemeniz gerekip gerekmediğini belirlemelisiniz.

## <a name="review-things-you-should-know"></a>Bilmeniz gereken leri gözden geçirin

### <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar
- Karma Azure AD join, ortamınız birden fazla Azure AD kiracısına kimlik verilerini senkronize eden tek bir AD ormanından oluşuyorsa şu anda desteklenmez.

- Etki Alanı Denetleyicisi (DC) rolünü çalıştıran Windows Server için karma Azure AD join desteklenmez.

- Kimlik bilgisi dolaşımı veya kullanıcı profili dolaşımı veya zorunlu profil kullanılırken, Karma Azure AD join, Windows'un alt düzey aygıtlarında desteklenmez.

- Server Core OS herhangi bir aygıt kaydı nı desteklemez.

### <a name="os-imaging-considerations"></a>İşletim sistemi görüntüleme hususları
- Sistem Hazırlama Aracı'na (Sysprep) güveniyorsanız ve yükleme için **Windows 10 1809 öncesi** bir görüntü kullanıyorsanız, bu görüntünün Azure AD'ye Karma Azure AD join olarak zaten kaydedilmiş bir aygıttan görüntü olmadığından emin olun.

- Ek VM'ler oluşturmak için Sanal Makine (VM) anlık görüntüsüne güveniyorsanız, anlık görüntün Azure AD'ye Karma Azure AD join olarak zaten kaydedilmiş bir VM'den olmadığından emin olun.

- Yeniden başlatılırken diskteki değişiklikleri temizleyen [Birleşik Yazma Filtresi](/windows-hardware/customize/enterprise/unified-write-filter) ve benzeri teknolojiler kullanıyorsanız, bunlar aygıt Karma Azure AD'ye katıldıktan sonra uygulanmalıdır. Hibrit Azure AD birleştirme'yi tamamlamadan önce bu tür teknolojileri etkinleştirmek, aygıtın her yeniden başlatmada katılmadan katılmasına neden olur

### <a name="handling-devices-with-azure-ad-registered-state"></a>Azure AD kayıtlı durumuna sahip aygıtları işleme
Windows 10 etki alanı birleştirilmiş aygıtlarınız kiracınıza [kayıtlı Azure AD](overview.md#getting-devices-in-azure-ad) ise, bu durum çift karma Azure AD'ye ve Azure AD kayıtlı aygıtına neden olabilir. Bu senaryoyu otomatik olarak ele almak için Windows 10 1803'e (KB4489894 uygulanmış) veya daha yukarısına yükseltmenizi öneririz. 1803 öncesi sürümlerde, Karma Azure AD birleştirme'yi etkinleştirmeden önce Azure AD kayıtlı durumunu el ile kaldırmanız gerekir. 1803 ve üzeri sürümlerde, bu ikili durumu önlemek için aşağıdaki değişiklikler yapılmıştır:

- Aygıt <i>Karma Azure AD katıldıktan ve aynı kullanıcı oturum açtıktan sonra</i>bir kullanıcı için varolan Azure AD kayıtlı durum otomatik olarak kaldırılır. Örneğin, Kullanıcı A'nın aygıtta Azure AD kayıtlı durumu varsa, Kullanıcı A'nın çift durumu yalnızca Kullanıcı A aygıtta oturum açtıklarında temizlenir. aynı aygıtta birden çok kullanıcı varsa, bu kullanıcılar oturum açtıklarında çift durum ayrı ayrı temizlenir.
- Bu kayıt defteri anahtarı - HKLM\SOFTWARE\Policies\Microsoft\Windows\WorkplaceJoin, "BlockAADWorkplaceJoin"=dword:00000001 ekleyerek etki alanı birleştirilmiş aygıtınızın Azure AD olarak kaydolmasını engelleyebilirsiniz.
- Windows 10 1803'te, Windows Hello for Business yapılandırıldıysa, kullanıcının çift durum temizlendikten sonra Windows Hello for Business'ı yeniden kurması gerekir. Bu sorun KB4512509 ile ele alınmıştır

> [!NOTE]
> Azure AD kayıtlı aygıt, Intune tarafından yönetiliyorsa otomatik olarak kaldırılmaz.

### <a name="additional-considerations"></a>Diğer konular
- Ortamınız sanal masaüstü altyapısı (VDI) kullanıyorsa, [Bkz. Aygıt kimliği ve masaüstü sanallaştırması.](/azure/active-directory/devices/howto-device-identity-virtual-desktop-infrastructure)

- Karma Azure REKLAM birleştirme FIPS uyumlu TPM 2.0 için desteklenir ve TPM 1.2 için desteklenmez. Aygıtlarınızda FIPS uyumlu TPM 1.2 varsa, Karma Azure AD join ile devam etmeden önce aygıtları devre dışı bırakmalısınız. Microsoft, TPM üreticisine bağlı olduğu için TPM'ler için FIPS modunu devre dışı bırakmak için herhangi bir araç sağlamaz. Destek için lütfen donanımınız OEM ile iletişime geçin. 

- Windows 10 1903 sürümünden itibaren, TPM 1.2 karma Azure AD birleştirme ile kullanılmaz ve bu TPM'lere sahip aygıtlar TPM'leri yokmuş gibi kabul edilir.

## <a name="review-controlled-validation-of-hybrid-azure-ad-join"></a>Karma Azure AD join'in kontrollü geçerliliğini gözden geçirme

Ön koşulların tümü yerinde olduğunda, Windows aygıtları otomatik olarak Azure AD kiracınızda aygıt olarak kaydedilir. Azure AD'deki bu aygıt kimliklerinin durumu karma Azure AD join olarak adlandırılır. Bu makalede kapsanan kavramlar hakkında daha fazla bilgiyi [Azure Active Directory'de aygıt kimlik yönetimine giriş makalesinde](overview.md)bulabilirsiniz.

Kuruluşlar, tüm kuruluşlarında tek seferde etkinleştirmeden önce karma Azure AD birleştirmesinin kontrollü bir doğrulamasını yapmak isteyebilir. Nasıl başarlayacağımı anlamak için [karma Azure AD join'in](hybrid-azuread-join-control.md) makale kontrollü doğrulamasını gözden geçirin.

## <a name="select-your-scenario-based-on-your-identity-infrastructure"></a>Kimlik altyapınıza göre senaryonuzu seçin

Karma Azure AD join, UPN'nin routable veya non-routable olup olmadığına bağlı olarak hem yönetilen hem de federe ortamlarla çalışır. Desteklenen senaryolarda tablo için sayfanın altına bakın.  

### <a name="managed-environment"></a>Yönetilen ortam

Yönetilen bir ortam, Parola [Karma Eşitleme (PHS) veya](/azure/active-directory/hybrid/whatis-phs) Sorunsuz Tek İşaret [Açma](/azure/active-directory/hybrid/how-to-connect-sso)ile Kimlik [Doğrulamadan Geçiş (PTA)](/azure/active-directory/hybrid/how-to-connect-pta) aracılığıyla dağıtılabilir.

Bu senaryolar, kimlik doğrulaması için bir federasyon sunucusu yapılandırmanızı gerektirmez.

### <a name="federated-environment"></a>Federe ortam

Federe bir ortamda, aşağıdaki gereksinimleri destekleyen bir kimlik sağlayıcısı olmalıdır. Active Directory Federation Services (AD FS) kullanarak federe bir ortamınız varsa, aşağıdaki gereksinimler zaten desteklenir.

- **WIAORMULTIAUTHN iddiası:** Bu talep, Windows alt düzey aygıtlar için karma Azure AD join yapmak için gereklidir.
- **WS-Trust protokolü:** Bu protokol, Azure AD ile windows geçerli karma Azure AD birleştirilmiş aygıtları doğrulamak için gereklidir. AD FS kullanırken, aşağıdaki WS-Trust uç noktalarını etkinleştirmeniz gerekir:`/adfs/services/trust/2005/windowstransport`  
`/adfs/services/trust/13/windowstransport`  
  `/adfs/services/trust/2005/usernamemixed` 
  `/adfs/services/trust/13/usernamemixed`
  `/adfs/services/trust/2005/certificatemixed` 
  `/adfs/services/trust/13/certificatemixed` 

> [!WARNING] 
> Hem **adfs/services/trust/2005/windowstransport** veya **adfs/services/trust/13/windowstransport,** intranet uç noktalarına bakan intranet olarak etkinleştirilmeli ve Web Application Proxy aracılığıyla uç noktalara bakan extranet olarak açıklanmamalıdır. WS-Trust Windows uç noktalarını nasıl devre dışı kalacaksınız hakkında daha fazla bilgi edinmek için [proxy'deki WS-Trust Windows uç noktalarını devre dışı etme bilgisini](/windows-server/identity/ad-fs/deployment/best-practices-securing-ad-fs#disable-ws-trust-windows-endpoints-on-the-proxy-ie-from-extranet)edinin. **Service** > **Endpoints**altında AD FS yönetim konsolu aracılığıyla hangi uç noktaların etkinleştirildigini görebilirsiniz.

> [!NOTE]
> Azure AD, yönetilen etki alanlarındaki akıllı kartları veya sertifikaları desteklemez.

1.1.819.0 sürümünden itibaren Azure AD Connect hibrit Azure AD'ye katılımı yapılandırmak için bir sihirbaz sağlar. Sihirbaz, yapılandırma işlemini önemli ölçüde basitleştirebilmenizi sağlar. Azure AD Connect'in gerekli sürümünü yüklemek sizin için bir seçenek değilse, [aygıt kaydının el ile nasıl yapılandırılabildiğini](hybrid-azuread-join-manual.md)öğrenin. 

Kimlik altyapınızla eşleşen senaryoya göre bkz:

- [Federe ortam için karma Azure Active Directory birleştirme yapılandırma](hybrid-azuread-join-federated-domains.md)
- [Yönetilen ortam için karma Azure Etkin Dizin birleştirme yapılandırma](hybrid-azuread-join-managed-domains.md)

## <a name="review-on-premises-ad-users-upn-support-for-hybrid-azure-ad-join"></a>Karma Azure AD join için şirket içi AD kullanıcıları UPN desteğini gözden geçirin

Bazen şirket içi AD kullanıcılarınız UPN'leriniz Azure REKLAM YÜKSELTMElerinizden farklı olabilir. Bu gibi durumlarda, Windows 10 Karma Azure AD join [kimlik doğrulama yöntemi,](/azure/security/fundamentals/choose-ad-authn)etki alanı türü ve Windows 10 sürümüne dayalı şirket içi AD UPN'ler için sınırlı destek sağlar. Ortamınızda bulunabilecek iki tür şirket içi AD UPN'si vardır:

- Routable kullanıcıları UPN: Routable UPN geçerli bir doğrulanmış etki alanı vardır, bir etki alanı kayıt şirketi ile kayıtlı. Örneğin, contoso.com Azure AD'deki birincil etki alanıysa, contoso.org Contoso'ya ait olan ve [Azure AD'de doğrulanan](/azure/active-directory/fundamentals/add-custom-domain) şirket içi AD'deki birincil etki alanıdır
- Routable olmayan kullanıcılar UPN: Routable olmayan bir UPN doğrulanmış bir etki alanı yok. Yalnızca kuruluşunuzun özel ağında geçerlidir. Örneğin, contoso.com Azure AD'deki birincil etki alanıysa, contoso.local şirket içi AD'deki birincil etki alanıdır, ancak internette doğrulanabilir bir etki alanı değildir ve yalnızca Contoso'nun ağında kullanılır.

> [!NOTE]
> Bu bölümdeki bilgiler yalnızca şirket içi kullanıcılar UPN için geçerlidir. Şirket içi bilgisayar etki alanı soneki için geçerli değildir (örnek: computer1.contoso.local).

Aşağıdaki tablo, Windows 10 Karma Azure AD join'teki bu şirket içi AD UPN'leri için destek hakkında ayrıntılı bilgi sağlar

| Şirket içi AD UPN türü | Etki alanı türü | Windows 10 sürümü | Açıklama |
| ----- | ----- | ----- | ----- |
| Yönlendirilebilir | Federe | 1703 sürümünden itibaren | Genel kullanıma sunuldu |
| Routable olmayan | Federe | 1803 sürümünden itibaren | Genel kullanıma sunuldu |
| Yönlendirilebilir | Yönetilen | 1803 sürümünden itibaren | Genel olarak kullanılabilir, Windows kilit ekranındaki Azure AD SSPR desteklenmez |
| Routable olmayan | Yönetilen | Desteklenmiyor | |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Federe ortam](hybrid-azuread-join-federated-domains.md)
> için karma Azure Active Directory join'i yapılandırın[Yönetilen ortam için karma Azure Active Directory join'i yapılandırın](hybrid-azuread-join-managed-domains.md)

<!--Image references-->
[1]: ./media/hybrid-azuread-join-plan/12.png
