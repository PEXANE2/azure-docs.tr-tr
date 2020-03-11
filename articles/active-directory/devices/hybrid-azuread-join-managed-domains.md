---
title: Yönetilen etki alanları için karma Azure Active Directory birleştirmesini yapılandırma | Microsoft Docs
description: Yönetilen etki alanları için karma Azure Active Directory birleştirmesini yapılandırmayı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: tutorial
ms.date: 03/06/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: bcd00972c2da0d3d5dafe76a8619e0f0ccaedc19
ms.sourcegitcommit: 72c2da0def8aa7ebe0691612a89bb70cd0c5a436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79081882"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-managed-domains"></a>Öğretici: Yönetilen etki alanları için hibrit Azure Active Directory'ye katılımı yapılandırma

Bu öğreticide, etki alanına katılmış Active Directory cihazları için karma Azure Active Directory (Azure AD) katılmayı nasıl yapılandıracağınızı öğreneceksiniz. Bu yöntem, hem şirket içi Active Directory hem de Azure AD içeren bir yönetilen ortamı destekler.

Kuruluşunuzdaki bir kullanıcı gibi, bir cihaz da korumak istediğiniz çekirdek kimliktir. Kaynaklarınızı istediğiniz zaman ve herhangi bir konumdan korumak için bir cihazın kimliğini kullanabilirsiniz. Azure AD 'de cihaz kimliklerini yöneterek bu hedefi gerçekleştirebilirsiniz. Aşağıdaki yöntemlerden birini kullanın:

- Azure AD'ye katılım
- Hibrit Azure AD'ye katılım
- Azure AD kaydı

Bu makale, karma Azure AD JOIN 'e odaklanır.

Cihazlarınızı Azure AD 'ye getirmek, bulut ve şirket içi kaynaklarınız genelinde çoklu oturum açma (SSO) ile Kullanıcı üretkenliğini en üst düzeye çıkarır. Bulut ve şirket içi kaynaklarınız için aynı anda [koşullu erişimle](../active-directory-conditional-access-azure-portal.md) erişimi güvenli hale getirebilirsiniz.

Yönetilen bir ortamı, [sorunsuz çoklu oturum açma](../hybrid/how-to-connect-sso.md)ile [Parola karması eşitlemesi (PHS)](../hybrid/whatis-phs.md) veya [geçişli kimlik doğrulaması (PTA)](../hybrid/how-to-connect-pta.md) kullanarak dağıtabilirsiniz. Bu senaryolar, kimlik doğrulaması için bir federasyon sunucusu yapılandırmanızı gerektirmez.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Hibrit Azure AD'ye katılımı yapılandırma
> * Windows alt düzey cihazlarını etkinleştirme
> * Katılmış cihazları doğrulama
> * Sorun giderme

## <a name="prerequisites"></a>Önkoşullar

- [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) (1.1.819.0 veya üzeri)
- Azure AD kiracınız için genel yönetici kimlik bilgileri
- Ormanların her biri için Kuruluş Yöneticisi kimlik bilgileri

Bu makalelerle ilgili bilgi edinin:

- [Cihaz kimliği nedir?](overview.md)
- [Nasıl yapılır: karma Azure Active Directory JOIN Uygulamanızı planlayın](hybrid-azuread-join-plan.md)
- [Karma Azure AD JOIN 'in denetimli doğrulaması](hybrid-azuread-join-control.md)

> [!NOTE]
> Azure AD, yönetilen etki alanlarında akıllı kartlar veya sertifikaları desteklemez.

Azure AD Connect, karma Azure AD 'ye katılmış olmasını istediğiniz cihazların bilgisayar nesnelerini Azure AD 'ye eşitlemediğini doğrulayın. Bilgisayar nesneleri belirli kuruluş birimlerine (OU) aitse, OU 'Ları Azure AD Connect eşitlenecek şekilde yapılandırın. Azure AD Connect kullanarak bilgisayar nesnelerini eşitleme hakkında daha fazla bilgi için bkz. [kuruluş birimi tabanlı filtreleme](../hybrid/how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering).

Sürüm 1.1.819.0 ile başlayarak, Azure AD Connect karma Azure AD JOIN 'i yapılandırmaya yönelik bir sihirbaz içerir. Sihirbaz yapılandırma işlemini önemli ölçüde basitleştirir. Sihirbaz, cihaz kaydı için hizmet bağlantı noktalarını (SCP) yapılandırır.

Bu makaledeki yapılandırma adımları Azure AD Connect ' deki Sihirbazı kullanmayı temel alır.

Karma Azure AD katılımı, cihazların kuruluşunuzun ağının içinden aşağıdaki Microsoft kaynaklarına erişmesini gerektirir:  

- `https://enterpriseregistration.windows.net`
- `https://login.microsoftonline.com`
- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com` (veya sorunsuz SSO kullanmayı planlıyorsanız)

Kuruluşunuzun bir giden proxy aracılığıyla internet 'e erişmesi gerekiyorsa, Windows 10 bilgisayarları Azure AD 'ye cihaz kaydı için etkinleştirmek üzere [Web proxy otomatik bulma (WPAD)](https://docs.microsoft.com/previous-versions/tn-archive/cc995261(v%3dtechnet.10)) uygulamanızı öneririz. WPAD yapılandırma ve yönetme sorunlarını gidermek için bkz. [otomatik algılama sorunlarını giderme](/previous-versions/tn-archive/cc302643(v=technet.10)).

WPAD kullanmıyorsanız, bilgisayarınızda Windows 10 1709 ile başlayarak ara sunucu ayarlarını yapılandırabilirsiniz. Daha fazla bilgi için bkz. [GPO tarafından dağıtılan WinHTTP proxy ayarları](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/).

> [!NOTE]
> Bilgisayarınızda, WinHTTP ayarlarını kullanarak ara sunucu ayarlarını yapılandırırsanız, yapılandırılmış ara sunucuya bağlanamamakta olan tüm bilgisayarlar internet 'e bağlanamaz.

Kuruluşunuzun kimliği doğrulanmış bir giden ara sunucu üzerinden İnternet 'e erişmesi gerekiyorsa, Windows 10 bilgisayarlarınızın giden ara sunucuda başarıyla kimlik doğrulaması yapamadığından emin olun. Windows 10 bilgisayarları makine bağlamını kullanarak cihaz kaydı çalıştırdığından, makine bağlamını kullanarak giden proxy kimlik doğrulamasını yapılandırın. Yapılandırma gereksinimleri için giden bağlantı proxy'si sağlayıcınızı izleyin.

Cihazın, [test cihazı kayıt bağlantısı](https://gallery.technet.microsoft.com/Test-Device-Registration-3dc944c0) betiğini kullanarak sistem hesabı altında yukarıdaki Microsoft kaynaklarına erişebileceğini doğrulayın.

## <a name="configure-hybrid-azure-ad-join"></a>Hibrit Azure AD'ye katılımı yapılandırma

Azure AD Connect kullanarak hibrit bir Azure AD katılımı yapılandırmak için:

1. Azure AD Connect başlatın ve ardından **Yapılandır**' ı seçin.

   ![Hoş Geldiniz](./media/hybrid-azuread-join-managed-domains/welcome-azure-ad-connect.png)

1. **Ek görevler**' de **cihaz seçeneklerini yapılandır**' ı seçin ve ardından **İleri**' yi seçin.

   ![Ek görevler](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-additional-tasks.png)

1. **Genel bakış**' da **İleri**' yi seçin.

   ![Genel Bakış](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-overview.png)

1. **Azure AD 'ye Bağlan**' da Azure AD kiracınız için genel bir yöneticinin kimlik bilgilerini girin.  

   ![Azure AD'ye Bağlanma](./media/hybrid-azuread-join-managed-domains/connect-to-azure-ad-username-password.png)

1. **Cihaz seçenekleri**' nde **karma Azure AD birleştirmesini Yapılandır**' ı seçin ve ardından **İleri**' yi seçin.

   ![Cihaz seçenekleri](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-device-options.png)

1. **SCP yapılandırmasında**Azure AD Connect SCP 'yi yapılandırmak istediğiniz her orman için aşağıdaki adımları tamamlayıp **İleri**' yi seçin.

   1. **Ormanı**seçin.
   1. Bir **kimlik doğrulama hizmeti**seçin.
   1. Kuruluş Yöneticisi kimlik bilgilerini girmek için **Ekle** ' yi seçin.

   ![SCP](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-scp-configuration.png)

1. **Cihaz işletim sistemlerinde**Active Directory ortamınızdaki cihazların kullandığı işletim sistemlerini seçin ve ardından **İleri**' yi seçin.

   ![Cihaz işletim sistemi](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-device-operating-systems.png)

1. **Yapılandırmaya hazırlanma**bölümünde **Yapılandır**' ı seçin.

   ![Yapılandırma için hazır](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-ready-to-configure.png)

1. **Yapılandırma Tamam**bölümünde **Çıkış**' ı seçin.

   ![Yapılandırma tamamlandı](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-configuration-complete.png)

## <a name="enable-windows-down-level-devices"></a>Windows alt düzey cihazlarını etkinleştirme

Etki alanına katılmış cihazlarınızdan bazıları Windows alt düzey aygıtlardır, şunları yapmanız gerekir:

- Cihaz kaydı için yerel intranet ayarlarını yapılandırma
- Sorunsuz SSO yapılandırma
- Windows alt düzey bilgisayarlar için Microsoft Workplace Join 'yi yükler

> [!NOTE]
> Windows 7 desteği 14 Ocak 2020 tarihinde sona erdi. Daha fazla bilgi için bkz. [Windows 7 desteği sona erdi](https://support.microsoft.com/help/4057281/windows-7-support-ended-on-january-14-2020).

### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Cihaz kaydı için yerel intranet ayarlarını yapılandırma

Windows alt düzey cihazlarınızın karma Azure AD katılmasını tamamlayıp cihazların Azure AD 'de kimlik doğrulaması yaparken sertifika istemlerine engel olmak için, Internet Explorer 'daki yerel intranet bölgesine aşağıdaki URL 'Leri eklemek üzere etki alanına katılmış cihazlarınıza bir ilke gönderebilirsiniz:

- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com`

Ayrıca, kullanıcının yerel intranet bölgesindeki **komut dosyası aracılığıyla durum çubuğu güncelleştirmelerine Izin ver** ' i etkinleştirmeniz gerekir.

### <a name="configure-seamless-sso"></a>Sorunsuz SSO yapılandırma

Windows alt düzey cihazlarınızın karma Azure AD birleştirmesini, [Parola karması eşitleme](../hybrid/whatis-phs.md) veya Azure AD bulut kimlik doğrulama yöntemi olarak [geçişli kimlik doğrulaması](../hybrid/how-to-connect-pta.md) kullanan yönetilen bir etkı alanında, [sorunsuz SSO](../hybrid/how-to-connect-sso-quick-start.md#step-2-enable-the-feature)'yu da yapılandırmanız gerekir.

### <a name="install-microsoft-workplace-join-for-windows-down-level-computers"></a>Windows alt düzey bilgisayarlar için Microsoft Workplace Join 'yi yükler

Windows alt düzey cihazlarını kaydettirmek için kuruluşların [Windows 10 olmayan bilgisayarlar Için Microsoft Workplace Join](https://www.microsoft.com/download/details.aspx?id=53554)yüklemesi gerekir. Windows 10 olmayan bilgisayarlar için Microsoft Workplace Join, Microsoft Indirme Merkezi ' nde bulunabilir.

Paketi [Microsoft uç noktası Configuration Manager](/configmgr/)gibi bir yazılım dağıtım sistemi kullanarak dağıtabilirsiniz. Paket, `quiet` parametresiyle standart sessiz yükleme seçeneklerini destekler. Configuration Manager geçerli sürümü, tamamlanan kayıtları izleme özelliği gibi önceki sürümlerden daha fazla avantaj sunar.

Yükleyici, kullanıcı bağlamında çalışan sistemde zamanlanmış bir görev oluşturur. Görev, Kullanıcı Windows 'da oturum açtığında tetiklenir. Görev, Azure AD ile kimlik doğrulamasından geçtikten sonra Kullanıcı kimlik bilgilerini kullanarak cihazı Azure AD ile sessizce birleştirir.

## <a name="verify-the-registration"></a>Kaydı doğrulama

**[Get-MsolDevice](/powershell/msonline/v1/get-msoldevice)** kullanarak Azure kiracınızdaki cihaz kayıt durumunu doğrulayın. Bu cmdlet [Azure Active Directory PowerShell modülüdür](/powershell/azure/install-msonlinev1?view=azureadps-2.0).

Hizmet ayrıntılarını denetlemek için **Get-MSolDevice** cmdlet 'ini kullandığınızda:

- Windows istemcisinde KIMLIĞIYLE eşleşen **CIHAZ kimliğine** sahip bir nesne var olmalıdır.
- **Devicetrusttype** değeri **etki alanına katılmış**. Bu ayar, Azure AD portalındaki **cihazlar** sayfasında **karma Azure AD 'ye katılmış** durumuna eşdeğerdir.
- Koşullu erişimde kullanılan cihazlarda, **Enabled** değeri **true** 'Dur ve **devicetrustlevel** **yönetilir**.

Hizmet ayrıntılarını denetlemek için:

1. Windows PowerShell 'i yönetici olarak açın.
1. Azure kiracınıza bağlanmak için `Connect-MsolService` girin.  
1. `get-msoldevice -deviceId <deviceId>` yazın.
1. **Enabled** değerinin **True** olarak ayarlandığını doğrulayın.

## <a name="troubleshoot-your-implementation"></a>Uygulamanızda sorun giderme

Etki alanına katılmış Windows cihazlarına yönelik karma Azure AD katılımı tamamlanırken sorunlarla karşılaşırsanız, bkz.:

- [Karma Azure Active Directory katılmış cihazlarda sorun giderme](troubleshoot-hybrid-join-windows-current.md)
- [Karma Azure Active Directory katılmış alt düzey cihazlarda sorun giderme](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure portal kullanarak cihaz kimliklerini yönetmeyi öğrenmek için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Cihaz kimliklerini yönetme](device-management-azure-portal.md)
