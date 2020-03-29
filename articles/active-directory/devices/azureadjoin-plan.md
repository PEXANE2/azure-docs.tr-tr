---
title: Azure Etkin Dizininizi uygulamayla birleştirme planı nasıl
description: Ortamınızda Azure AD birleştirilmiş aygıtları uygulamak için gereken adımları açıklar.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: a6bbecf0e365ba7a8424da775245181fa64c21f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672690"
---
# <a name="how-to-plan-your-azure-ad-join-implementation"></a>Nasıl yapilir: Azure AD'nizi uygulamayla planlayın

Azure AD join, kullanıcılarınızı üretken ve güvenli tutarken şirket içi Active Directory'ye katılmanıza gerek kalmadan aygıtları doğrudan Azure AD'ye katılmanızı sağlar. Azure AD join, hem ölçekhem de kapsamlı dağıtımlar için kurumsal kullanıma hazırdır.   

Bu makale, Azure AD join uygulamanızı planlamak için ihtiyacınız olan bilgileri sağlar.
 
## <a name="prerequisites"></a>Ön koşullar

Bu makalede, [Azure Etkin Dizin'de aygıt yönetimine Giriş'i](../device-management-introduction.md)bildiğiniz varsayar.

## <a name="plan-your-implementation"></a>Uygulamanızı planlayın

Azure AD join uygulamanızı planlamak için şunları aramalısınız:

|   |   |
|---|---|
|![İşaretli][1]|Senaryolarınızı gözden geçirin|
|![İşaretli][1]|Kimlik altyapınızı gözden geçirin|
|![İşaretli][1]|Cihaz yönetiminizi değerlendirin|
|![İşaretli][1]|Uygulamalar ve kaynaklar la ilgili hususları anlama|
|![İşaretli][1]|Sağlama seçeneklerinizi anlama|
|![İşaretli][1]|Kurumsal durum dolaşımını yapılandırma|
|![İşaretli][1]|Koşullu Erişimi Yapılandırma|

## <a name="review-your-scenarios"></a>Senaryolarınızı gözden geçirin 

Karma Azure AD join belirli senaryolar için tercih edilebilirken, Azure AD join, Windows ile bulutilk modeline geçiş yapmanızı sağlar. Aygıt yönetiminizi modernize etmeyi ve aygıtla ilgili BT maliyetlerini azaltmayı planlıyorsanız, Azure AD join bu hedeflere ulaşmak için harika bir temel sağlar.  
 
Hedefleriniz aşağıdaki ölçütlere uygunsa Azure AD join'i göz önünde bulundurmalısınız:

- Microsoft 365'i kullanıcılarınız için üretkenlik paketi olarak benimsiyorsunuz.
- Bir bulut aygıt yönetimi çözümüyle cihazları yönetmek istiyorsunuz.
- Coğrafi olarak dağıtılmış kullanıcılar için aygıt sağlamayı basitleştirmek istiyorsunuz.
- Uygulama altyapınızı modernize etmeyi planlıyorsunuz.

## <a name="review-your-identity-infrastructure"></a>Kimlik altyapınızı gözden geçirin  

Azure AD, hem yönetilen hem de federe ortamlarla çalışır.  

### <a name="managed-environment"></a>Yönetilen ortam

Yönetilen bir ortam, Parola [Karma Eşitle'si](/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization) veya Sorunsuz Tek İşaret Açma ile [Kimlik Doğrulamadan Geç](/azure/active-directory/hybrid/how-to-connect-pta-quick-start) üzerinden dağıtılabilir.

Bu senaryolar, kimlik doğrulaması için bir federasyon sunucusu yapılandırmanızı gerektirmez.

### <a name="federated-environment"></a>Federe ortam

Federe bir ortamda hem WS-Trust hem de WS-Fed protokollerini destekleyen bir kimlik sağlayıcısı olmalıdır:

- **WS-Fed:** Bu protokolün bir aygıtı Azure AD'ye katması gerekir.
- **WS-Güven:** Bu protokolün Azure AD'ye katılan bir aygıtta oturum açması gerekir.

AD FS kullanırken, aşağıdaki WS-Trust uç noktalarını etkinleştirmeniz gerekir:`/adfs/services/trust/2005/usernamemixed`
 `/adfs/services/trust/13/usernamemixed`
 `/adfs/services/trust/2005/certificatemixed`
 `/adfs/services/trust/13/certificatemixed`

Kimlik sağlayıcınız bu protokolleri desteklemiyorsa, Azure AD join yerel olarak çalışmaz. 

>[!NOTE]
> Şu anda Azure AD join, [birincil kimlik doğrulama yöntemi olarak harici kimlik doğrulama sağlayıcılarıyla yapılandırılan AD FS 2019](/windows-server/identity/ad-fs/operations/additional-authentication-methods-ad-fs#enable-external-authentication-methods-as-primary)ile çalışmıyor. Azure AD, bu senaryoda kimlik doğrulama hatalarıyla sonuçlanan birincil yöntem olarak parola kimlik doğrulamasına varsayılan katılma


### <a name="smartcards-and-certificate-based-authentication"></a>Akıllı kartlar ve sertifika tabanlı kimlik doğrulama

Aygıtları Azure AD'ye katılmak için akıllı kartları veya sertifika tabanlı kimlik doğrulamayı kullanamazsınız. Ancak, AD FS yapılandırılmış varsa, akıllı kartlar Azure AD'ye katılan aygıtlarda oturum açabilmek için kullanılabilir.

**Tavsiye:** Windows 10 aygıtlarına güçlü, parolasız kimlik doğrulama için Windows Hello for Business'ı uygulayın.

### <a name="user-configuration"></a>Kullanıcı yapılandırması

Şunları içererek:

- **Şirket içi Active Directory**, [Azure AD Connect'i](/azure/active-directory/hybrid/how-to-connect-sync-whatis)kullanarak bunları Azure AD ile senkronize etmeniz gerekir. 
- **Azure AD,** ek kurulum gerekmez.

Azure AD UPN'lerinden farklı şirket içi UPN'ler Azure AD'ye katılan cihazlarda desteklenmez. Kullanıcılarınız şirket içi bir UPN kullanıyorsa, Azure AD'de birincil UPN'lerini kullanmaya geçmeyi planlamanız gerekir.

## <a name="assess-your-device-management"></a>Cihaz yönetiminizi değerlendirin

### <a name="supported-devices"></a>Desteklenen cihazlar

Azure AD join:

- Yalnızca Windows 10 aygıtları için geçerlidir. 
- Windows'un veya diğer işletim sistemlerinin önceki sürümleri için geçerli değildir. Windows 7/8.1 aygıtlarınız varsa, Azure AD birleştirme'yi dağıtmak için Windows 10'a yükseltmeniz gerekir.
- FIPS modunda TPM'li cihazlarda desteklenmez.
 
**Tavsiye:** Güncelleştirilmiş özelliklerden yararlanmak için her zaman en son Windows 10 sürümükullanın.

### <a name="management-platform"></a>Yönetim platformu

Azure AD'ye katılan aygıtlar için aygıt yönetimi, Intune ve MDM CSP'ler gibi bir MDM platformuna dayanır. Windows 10, tüm uyumlu MDM çözümleriyle çalışan yerleşik bir MDM aracısı vardır.

> [!NOTE]
> Grup ilkeleri, şirket içi Active Directory'ye bağlı olmadıklarından Azure AD'ye katılan aygıtlarda desteklenmez. Azure AD'ye katılan cihazların yönetimi yalnızca MDM ile mümkündür

Azure AD birleştirilmiş aygıtları yönetmek için iki yaklaşım vardır:

- **Yalnızca MDM** - Bir cihaz yalnızca Intune gibi bir MDM sağlayıcısı tarafından yönetilir. Tüm ilkeler MDM kayıt sürecinin bir parçası olarak teslim edilir. Azure AD Premium veya EMS müşterileri için MDM kaydı, Azure AD join'in bir parçası olan otomatik bir adımdır.
- **Ortak yönetim** - Bir cihaz bir MDM sağlayıcısı ve SCCM tarafından yönetilir. Bu yaklaşımda, SCCM aracısı belirli yönleri yönetmek için MDM yönetilen bir aygıta yüklenir.

Grup ilkeleri kullanıyorsanız, [MDM geçiş çözümleme aracını (MMAT)](https://github.com/WindowsDeviceManagement/MMAT)kullanarak MDM ilke paritenizi değerlendirin. 

Grup ilkeleri yerine bir MDM çözümünün kullanılıp kullanamayacağını belirlemek için desteklenen ve desteklenmeyen ilkeleri gözden geçirin. Desteklenmeyen ilkeler için aşağıdakileri göz önünde bulundurun:

- Azure AD'ye katılan aygıtlar veya kullanıcılar için desteklenmeyen ilkeler gerekli mi?
- Bulut odaklı bir dağıtımda desteklenmeyen ilkeler uygulanabilir mi?

MDM çözümünüz Azure AD uygulama galerisinde kullanılamıyorsa, [MDM ile Azure Active Directory entegrasyonunda](/windows/client-management/mdm/azure-active-directory-integration-with-mdm)özetlenen işlemi izleyerek ekleyebilirsiniz. 

Eş-yönetim aracılığıyla, ilkeler MDM platformunuz aracılığıyla teslim edilirken cihazlarınızın belirli yönlerini yönetmek için SCCM'yi kullanabilirsiniz. Microsoft Intune, SCCM ile ortak yönetim sağlar. Windows 10 aygıtları için ortak yönetim hakkında daha fazla bilgi için [bkz.](/configmgr/core/clients/manage/co-management-overview) Intune dışında bir MDM ürünü kullanıyorsanız, lütfen ilgili ortak yönetim senaryoları için MDM sağlayıcınıza danışın.

**Tavsiye:** Azure AD'ye katılan aygıtlar için yalnızca MDM yönetimini göz önünde bulundurun.

## <a name="understand-considerations-for-applications-and-resources"></a>Uygulamalar ve kaynaklar la ilgili hususları anlama

Daha iyi bir kullanıcı deneyimi ve erişim denetimi için uygulamaları şirket içinden buluta geçirmenizi öneririz. Ancak Azure AD'ye katılan aygıtlar hem şirket içi hem de bulut uygulamalarına sorunsuz bir şekilde erişim sağlayabilir. Daha fazla bilgi için, [Azure AD'ye katılan aygıtlarda SSO'nun şirket içi kaynaklarının nasıl çalıştığını](azuread-join-sso.md)görün.

Aşağıdaki bölümlerde, farklı uygulama türleri ve kaynaklar için dikkate alınması gerekenler listelenebönemlidir.

### <a name="cloud-based-applications"></a>Bulut tabanlı uygulamalar

Bir uygulama Azure AD uygulama galerisine eklenirse, kullanıcılar Azure AD'ye katılan aygıtlar aracılığıyla SSO'yu alır. Ek yapılandırma gerekmez. Kullanıcılar Hem Microsoft Edge hem de Chrome tarayıcılarında SSO alır. Chrome için [Windows 10 Hesapları uzantısını](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji)dağıtmanız gerekir. 

Tüm Win32 uygulamaları:

- Belirteç istekleri için Web Hesap Yöneticisi'ne (WAM) güvenin, Azure AD'ye katılan aygıtlarda SSO'ya da bindirin. 
- WAM'a güvenmeyin, kullanıcıları kimlik doğrulamasına teşvik edebilir. 

### <a name="on-premises-web-applications"></a>Şirket içi web uygulamaları

Uygulamalarınız şirket içinde özel olarak oluşturulmuş ve/veya barındırılıyorsa, bunları tarayıcınızın güvenilen sitelerine şunları eklemeniz gerekir:

- Windows tümleşik kimlik doğrulamasını çalışır şekilde etkinleştirme 
- Kullanıcılara hızlı olmayan bir SSO deneyimi sağlayın. 

AD FS kullanıyorsanız, [bkz.](/previous-versions/azure/azure-services/jj151809(v%3dazure.100)) 

**Tavsiye:** Bulutta barındırmayı (örneğin Azure) ve daha iyi bir deneyim için Azure AD ile tümleştirmeyi düşünün.

### <a name="on-premises-applications-relying-on-legacy-protocols"></a>Eski protokollere dayanan şirket içi uygulamalar

Kullanıcılar, aygıtın bir etki alanı denetleyicisine erişimi varsa, Azure AD'ye katılan aygıtlardan SSO alır. 

**Tavsiye:** Bu uygulamalar için güvenli erişim sağlamak için [Azure AD App proxy'sini](/azure/active-directory/manage-apps/application-proxy) dağıtın.

### <a name="on-premises-network-shares"></a>Şirket içi ağ paylaşımları

Bir aygıt şirket içi etki alanı denetleyicisine erişediğinde, kullanıcılarınız Azure AD'ye katılan aygıtlardan SSO'ya sahiptir.

### <a name="printers"></a>Yazıcı

Yazıcılar için, Azure AD'ye katılan aygıtlarda yazıcıları keşfetmek için [karma bulut baskısını](/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy) dağıtmanız gerekir. 

Yazıcılar yalnızca bulut ortamında otomatik olarak bulunamazken, kullanıcılarınız doğrudan eklemek için yazıcıların UNC yolunu da kullanabilir. 

### <a name="on-premises-applications-relying-on-machine-authentication"></a>Makine kimlik doğrulaması güvenerek şirket içi uygulamalar

Azure AD'ye katılan aygıtlar, makine kimlik doğrulamaya dayanan şirket içi uygulamaları desteklemez. 

**Tavsiye:** Bu uygulamaları emekli ve modern alternatifleri hareket düşünün.

### <a name="remote-desktop-services"></a>Uzak Masaüstü Hizmetleri

Azure AD'ye katılan aygıtlara uzak masaüstü bağlantısı, ana bilgisayarın Azure AD'ye katılmasını veya Karma Azure AD'sinin katılmasını gerektirir. Katılmamış veya Windows'a ait olmayan bir aygıttan uzak masaüstü desteklenmez. Daha fazla bilgi için bkz: [Uzak Azure AD'ye bağlan bilgisayara bağlan](/windows/client-management/connect-to-remote-aadj-pc)

## <a name="understand-your-provisioning-options"></a>Sağlama seçeneklerinizi anlama

Aşağıdaki yaklaşımları kullanarak Azure AD join'i sağlayabilirsiniz:

- **OOBE/Ayarlar'da self servis** - Self servis modunda, kullanıcılar Windows Out of Box Experience (OOBE) sırasında veya Windows Ayarları'ndan Azure AD join işleminden geçer. Daha fazla bilgi için [bkz.](/azure/active-directory/user-help/user-help-join-device-on-network) 
- **Windows Autopilot** - Windows Autopilot, OOBE'de daha sorunsuz bir deneyim için cihazların önceden yapılandırmasını sağlayarak Azure AD birleştirmesini gerçekleştirir. Daha fazla bilgi için [Windows Otomatik Pilot'a Genel Bakış bölümüne](/windows/deployment/windows-autopilot/windows-10-autopilot)bakın. 
- Toplu kayıt - Toplu **kayıt,** aygıtları yapılandırmak için toplu sağlama aracı kullanarak yönetici tarafından yönlendirilen Azure AD join'e olanak tanır. Daha fazla bilgi için [Windows aygıtları için Toplu kayıt bölümüne](/intune/windows-bulk-enroll)bakın.
 
İşte bu üç yaklaşımın bir karşılaştırması 
 
|   | Self servis kurulumu | Windows Autopilot | Toplu kayıt |
| --- | --- | --- | --- |
| Kullanıcı etkileşiminin ayarlatılmasını zorunlu kılmasını | Evet | Evet | Hayır |
| BT çabası gerektirir | Hayır | Evet | Evet |
| Uygulanabilir akışlar | OOBE & Ayarları | Yalnızca OOBE | Yalnızca OOBE |
| Birincil kullanıcının yerel yönetici hakları | Evet, varsayılan olarak | Yapılandırılabilir | Hayır |
| Cihaz OEM desteği gerektirir | Hayır | Evet | Hayır |
| Desteklenen sürümler | 1511+ | 1709+ | 1703+ |
 
Yukarıdaki tabloyu gözden geçirerek ve her iki yaklaşımı benimsemek için aşağıdaki hususları gözden geçirerek dağıtım yaklaşımınızı veya yaklaşımlarınızı seçin:  

- Kullanıcılarınız kurulum kendilerini gitmek için teknoloji anlayışlı mı? 
   - Self servis bu kullanıcılar için en iyi şekilde çalışabilir. Kullanıcı deneyimini geliştirmek için Windows Otomatik Pilot'u göz önünde bulundurun.  
- Kullanıcılarınız uzak mı yoksa şirket binaları içinde mi? 
   - Self servis veya Otomatik Pilot sorunsuz bir kurulum için uzak kullanıcılar için en iyi şekilde çalışır. 
- Kullanıcı odaklı mı yoksa yönetici tarafından yönetilen bir yapılandırmayı mı tercih edersiniz? 
   - Toplu kayıt, kullanıcılara teslim edilmeden önce aygıtları ayarlamak için yönetici odaklı dağıtım için daha iyi çalışır.     
- 1-2 OEMS'den cihaz satın aldınız mı, yoksa geniş bir OEM aygıtı dağıtımınız mı var?  
   - Otomatik Pilot'u da destekleyen sınırlı OEM'lerden satın alma durumunda, Otomatik Pilot ile daha sıkı entegrasyondan yararlanabilirsiniz. 

## <a name="configure-your-device-settings"></a>Cihaz ayarlarınızı yapılandırın

Azure portalı, kuruluşunuzdaki Azure AD birleştirilmiş aygıtların dağıtımını denetlemenize olanak tanır. Azure **Etkin Dizin sayfasında**ilgili ayarları yapılandırmak `Devices > Device settings`için .

### <a name="users-may-join-devices-to-azure-ad"></a>Kullanıcılar Azure AD'ye aygıtlara katılabilir

Bu seçeneği, dağıtımınızın kapsamına ve Azure AD'ye katılan bir aygıtı ayarlamasına izin vermek istediğiniz kişi olarak **Tümü** veya **Seçili** olarak ayarlayın. 

![Kullanıcılar Azure AD'ye aygıtlara katılabilir](./media/azureadjoin-plan/01.png)

### <a name="additional-local-administrators-on-azure-ad-joined-devices"></a>Azure AD'deki ek yerel yöneticiler birleştirilmiş aygıtlar

**Seçili'yi** seçin ve tüm Azure AD birleştirilmiş cihazlarda yerel yöneticilerin grubuna eklemek istediğiniz kullanıcıları seçin. 

![Azure AD'deki ek yerel yöneticiler birleştirilmiş aygıtlar](./media/azureadjoin-plan/02.png)

### <a name="require-multi-factor-auth-to-join-devices"></a>Aygıtlara katılmak için çok faktörlü Auth'a ihtiyaç duyma

"Kullanıcıların, aygıtları Azure AD'ye katılırken MFA gerçekleştirmelerini istiyorsanız **evet'** seçeneğini belirleyin. MFA kullanarak aygıtları Azure AD'ye birleştiren kullanıcılar için aygıtın kendisi ikinci bir faktör haline gelir.

![Aygıtlara katılmak için çok faktörlü Auth'a ihtiyaç duyma](./media/azureadjoin-plan/03.png)

## <a name="configure-your-mobility-settings"></a>Mobilite ayarlarınızı yapılandırın

Mobilite ayarlarınızı yapılandırmadan önce bir MDM sağlayıcısı eklemeniz gerekebilir.

**MDM sağlayıcısı eklemek için:**

1. Azure **Etkin Dizin sayfasında**, **Yönet** `Mobility (MDM and MAM)`bölümünde. 
1. **Uygulama Ekle'yi**tıklatın.
1. Listeden MDM sağlayıcınızı seçin.

   ![Uygulama ekleme](./media/azureadjoin-plan/04.png)

İlgili ayarları yapılandırmak için MDM sağlayıcınızı seçin. 

### <a name="mdm-user-scope"></a>MDM kullanıcı kapsamı

Dağıtımınızın kapsamına bağlı olarak **Bazılarını** veya **Tümünü** seçin. 

![MDM kullanıcı kapsamı](./media/azureadjoin-plan/05.png)

Kapsamınıza bağlı olarak aşağıdakilerden biri gerçekleşir: 

- **Kullanıcı MDM kapsamındadır**: Azure AD Premium aboneliğiniz varsa, MDM kaydı Azure AD join ile birlikte otomatiktir. Kapsamdaki tüm kullanıcıların MDM'niz için uygun bir lisansa sahip olması gerekir. MDM kaydı bu senaryoda başarısız olursa, Azure AD join da geri alınır.
- **Kullanıcı MDM kapsamında değildir**: Kullanıcılar MDM kapsamında değilse, Azure AD join herhangi bir MDM kaydı olmadan tamamlanır. Bu, yönetilmeyen bir aygıtla sonuçlanır.

### <a name="mdm-urls"></a>MDM URL'leri

MDM yapılandırmanızla ilgili üç URL vardır:

- MDM kullanım koşulları URL'si
- MDM bulma URL'si 
- MAM uyumluluk URL’si

![Uygulama ekleme](./media/azureadjoin-plan/06.png)

Her URL'nin önceden tanımlanmış bir varsayılan değeri vardır. Bu alanlar boşsa, daha fazla bilgi için lütfen MDM sağlayıcınıza başvurun.

### <a name="mam-settings"></a>MAM ayarları

MAM, Azure AD join için geçerli değildir. 

## <a name="configure-enterprise-state-roaming"></a>Kurumsal durum dolaşımını yapılandırma

Kullanıcıların ayarlarını cihazlar arasında eşitleyebilmeleri için Azure AD'de günlük dolaşımı etkinleştirmek istiyorsanız, Bkz. [Azure Etkin Dizini'nde Kurumsal Durum Dolaşımını Etkinleştir](enterprise-state-roaming-enable.md). 

**Öneri**: Karma Azure AD birleştirilmiş aygıtlar için bile bu ayarı etkinleştirin.

## <a name="configure-conditional-access"></a>Koşullu Erişimi Yapılandırma

Azure AD'ye katılan aygıtlarınız için yapılandırılan bir MDM sağlayıcınız varsa, sağlayıcı aygıt yönetim altında olduğu anda aygıtı uyumlu olarak işaretler. 

![Uyumlu cihaz](./media/azureadjoin-plan/46.png)

Bu [uygulamayı, Koşullu Erişim ile bulut uygulamasıerişimi için yönetilen aygıtlar gerektirmek için](../conditional-access/require-managed-devices.md)kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [İlk çalıştırma](azuread-joined-devices-frx.md)
> sırasında Azure AD ile yeni bir Windows 10 cihazına katılın[İş cihazınızı kuruluşunuzun ağına taşıyın](/azure/active-directory/user-help/user-help-join-device-on-network)

<!--Image references-->
[1]: ./media/azureadjoin-plan/12.png
