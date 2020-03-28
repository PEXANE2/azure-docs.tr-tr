---
title: Yönetilen etki alanları için karma Azure Etkin Dizin birleştirme yapılandırma | Microsoft Dokümanlar
description: Yönetilen etki alanları için karma Azure Etkin Dizin birleştirmesini nasıl yapılandırılabildiğini öğrenin.
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
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239115"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-managed-domains"></a>Öğretici: Yönetilen etki alanları için hibrit Azure Active Directory'ye katılımı yapılandırma

Bu eğitimde, Etkin Dizin etki alanına katılan aygıtlar için karma Azure Etkin Dizin (Azure AD) join'i nasıl yapılandırabileceğinizi öğrenirsiniz. Bu yöntem, hem şirket içi Etkin Dizin hem de Azure AD'yi içeren yönetilen bir ortamı destekler.

Kuruluşunuzdaki bir kullanıcı gibi, aygıt da korumak istediğiniz temel bir kimliktir. Kaynaklarınızı istediğiniz zaman ve herhangi bir konumdan korumak için aygıtın kimliğini kullanabilirsiniz. Bu hedefe, Azure AD'de aygıt kimliklerini yöneterek ulaşabilirsiniz. Aşağıdaki yöntemlerden birini kullanın:

- Azure AD'ye katılım
- Hibrit Azure AD'ye katılım
- Azure AD kaydı

Bu makalede, karma Azure AD birleştirme üzerinde duruluyor.

Cihazlarınızı Azure AD'ye getirmek, bulutunuzda ve şirket içi kaynaklarınızda tek oturum açma (SSO) aracılığıyla kullanıcı üretkenliğini en üst düzeye çıkarır. [Koşullu Erişim](../active-directory-conditional-access-azure-portal.md) ile bulutlarınıza ve şirket içi kaynaklarınıza erişimi aynı anda güvenli hale getirebilirsiniz.

Parola [karma eşitleme (PHS)](../hybrid/whatis-phs.md) veya [geçiş kimlik doğrulaması (PTA)](../hybrid/how-to-connect-pta.md) kullanarak yönetilen bir ortamı [sorunsuz tek oturum açma](../hybrid/how-to-connect-sso.md)ile dağıtabilirsiniz. Bu senaryolar, kimlik doğrulaması için bir federasyon sunucusu yapılandırmanızı gerektirmez.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Hibrit Azure AD'ye katılımı yapılandırma
> * Windows alt düzey cihazlarını etkinleştirme
> * Katılmış cihazları doğrulama
> * Sorun giderme

## <a name="prerequisites"></a>Ön koşullar

- [Azure AD Bağlantısı](https://www.microsoft.com/download/details.aspx?id=47594) (1.1.819.0 veya sonrası)
- Azure AD kiracınız için genel bir yöneticinin kimlik bilgileri
- Ormanların her biri için kuruluş yöneticisi kimlik bilgileri

Bu makaleleri kendinizi tanıtın:

- [Cihaz kimliği nedir?](overview.md)
- [Nasıl Olunması: Karma Azure Etkin Dizininizi uygulamayla birleştirin](hybrid-azuread-join-plan.md)
- [Hibrit Azure AD’ye katılıma yönelik denetimli doğrulama](hybrid-azuread-join-control.md)

> [!NOTE]
> Azure AD, yönetilen etki alanlarındaki akıllı kartları veya sertifikaları desteklemez.

Azure AD Connect'in karma Azure AD olmak istediğiniz aygıtların bilgisayar nesnelerini Azure AD ile eşitlediğini doğrulayın. Bilgisayar nesneleri belirli kuruluş birimlerine (OS) aitse, OSB'leri Azure AD Connect'te eşitlemek üzere yapılandırın. Azure AD Connect'i kullanarak bilgisayar nesnelerini nasıl eşitleyecekleri hakkında daha fazla bilgi edinmek için [Kuruluş birimi tabanlı filtreleme](../hybrid/how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering)bölümüne bakın.

Sürüm 1.1.819.0 ile başlayan Azure AD Connect, karma Azure AD birleştirmeyapılandırmak için bir sihirbaz içerir. Sihirbaz yapılandırma işlemini önemli ölçüde basitleştirir. Sihirbaz, aygıt kaydı için servis bağlantı noktalarını (SCP' ler) yapılandırır.

Bu makaledeki yapılandırma adımları, Azure AD Connect'teki sihirbazı kullanmaya dayanır.

Karma Azure AD join, aygıtların kuruluşunuzun ağından aşağıdaki Microsoft kaynaklarına erişebulaşmasını gerektirir:  

- `https://enterpriseregistration.windows.net`
- `https://login.microsoftonline.com`
- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com`(Sorunsuz SSO kullanıyorsanız veya kullanmayı planlıyorsanız)

Kuruluşunuz giden bir proxy üzerinden Internet erişimine ihtiyaç varsa, Windows 10 bilgisayarlarını Azure AD ile aygıt kaydı için etkinleştirmek için [Web Proxy Otomatik Bulma (WPAD) uygulamanızı](https://docs.microsoft.com/previous-versions/tn-archive/cc995261(v%3dtechnet.10)) öneririz. WPAD'i yapılandırma ve yönetme sorunlarını gidermek için [Bkz. Sorun Giderme Otomatik Algılama.](/previous-versions/tn-archive/cc302643(v=technet.10))

WPAD kullanmıyorsanız, Windows 10 1709'dan başlayarak bilgisayarınızda proxy ayarlarını yapılandırabilirsiniz. Daha fazla bilgi için [GPO tarafından dağıtılan WinHTTP Proxy Ayarları'na](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/)bakın.

> [!NOTE]
> WinHTTP ayarlarını kullanarak bilgisayarınızdaki proxy ayarlarını yapılandırırsanız, yapılandırılan proxy'ye bağlanamayan bilgisayarlar internete bağlanmaz.

Kuruluşunuz kimlik doğrulaması giden bir proxy aracılığıyla Internet erişimine ihtiyaç varsa, Windows 10 bilgisayarlarınızın giden proxy'ye başarılı bir şekilde kimlik doğrulayabilmesini sağlayın. Windows 10 bilgisayarları makine bağlamı kullanarak aygıt kaydı çalıştırdığından, giden proxy kimlik doğrulamasını makine bağlamı kullanarak yapılandırın. Yapılandırma gereksinimleri için giden bağlantı proxy'si sağlayıcınızı izleyin.

Test [Cihazı Kaydı Bağlantı](https://gallery.technet.microsoft.com/Test-Device-Registration-3dc944c0) komut dosyasını kullanarak aygıtın sistem hesabı altındaki yukarıdaki Microsoft kaynaklarına erişebileceğini doğrulayın.

## <a name="configure-hybrid-azure-ad-join"></a>Hibrit Azure AD'ye katılımı yapılandırma

Azure AD Connect'i kullanarak karma Bir Azure REKLAM birleştirmesini yapılandırmak için:

1. Azure AD Connect'i başlatın ve sonra **Yapıla'yı**seçin.

   ![Hoş Geldiniz](./media/hybrid-azuread-join-managed-domains/welcome-azure-ad-connect.png)

1. **Ek görevlerde,** Aygıt **seçeneklerini yapılandır'ı**seçin ve sonra **İleri'yi**seçin.

   ![Ek görevler](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-additional-tasks.png)

1. **Genel**Bakış'ta, **İleri'yi**seçin.

   ![Genel Bakış](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-overview.png)

1. **Azure AD'ye**Bağlan'da, Azure AD kiracınız için genel bir yöneticinin kimlik bilgilerini girin.  

   ![Azure AD'ye Bağlanma](./media/hybrid-azuread-join-managed-domains/connect-to-azure-ad-username-password.png)

1. **Aygıt seçeneklerinde,** Karma Azure **AD birleştirme'yi yapılandır'ı**ve **sonra İleri'yi**seçin.

   ![Cihaz seçenekleri](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-device-options.png)

1. **SCP yapılandırmasında,** Azure AD Connect'in SCP'yi yapılandırmasını istediğiniz her orman için aşağıdaki adımları tamamlayın ve **sonra İleri'yi**seçin.

   1. **Ormanı**seçin.
   1. Kimlik **Doğrulama Hizmeti**seçin.
   1. Kurumsal yönetici kimlik bilgilerini girmek için **Ekle'yi** seçin.

   ![SCP](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-scp-configuration.png)

1. **Aygıt işletim sistemlerinde,** Active Directory ortamınızdaki aygıtların kullandığı işletim sistemlerini seçin ve ardından **İleri'yi**seçin.

   ![Cihaz işletim sistemi](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-device-operating-systems.png)

1. **Yapılandırmaya Hazır**olarak, **Yapılandırma'yı**seçin.

   ![Yapılandırma için hazır](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-ready-to-configure.png)

1. **Configuration complete'de** **Exit'i**seçin.

   ![Yapılandırma tamamlandı](./media/hybrid-azuread-join-managed-domains/azure-ad-connect-configuration-complete.png)

## <a name="enable-windows-down-level-devices"></a>Windows alt düzey cihazlarını etkinleştirme

Etki alanına katılan aygıtlarınızdan bazıları Windows alt düzey aygıtlarsa, şunları yapmalısınız:

- Cihaz kaydı için yerel intranet ayarlarını yapılandırma
- Sorunsuz SSO'ya yapı
- Windows alt düzey bilgisayarlar için Microsoft İşyeri Birleştirme'yi yükleme

> [!NOTE]
> Windows 7 desteği 14 Ocak 2020 tarihinde sona erdi. Daha fazla bilgi için Windows [7 desteğinin sona erdirine](https://support.microsoft.com/help/4057281/windows-7-support-ended-on-january-14-2020)bakın.

### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Cihaz kaydı için yerel intranet ayarlarını yapılandırma

Windows alt düzey aygıtlarınızın karma Azure REKLAM birleştirmesini tamamlamak ve aygıtlar Azure AD'ye kimlik doğruladığında sertifika istemlerinden kaçınmak için, Internet Explorer'daki yerel intranet bölgesine aşağıdaki URL'leri eklemek için etki alanı yla birleştirilmiş aygıtlarınıza bir ilke taşıyabilirsiniz:

- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com`

Ayrıca, kullanıcının yerel intranet bölgesinde **komut dosyası aracılığıyla durum çubuğuna güncelleştirmelere izin ver'i** etkinleştirmeniz gerekir.

### <a name="configure-seamless-sso"></a>Sorunsuz SSO'ya yapı

Azure AD bulut kimlik doğrulama yönteminiz olarak parola karma [eşitleme](../hybrid/whatis-phs.md) veya [geçiş kimlik doğrulaması](../hybrid/how-to-connect-pta.md) kullanan yönetilen bir etki alanında Windows alt düzey aygıtlarınızın karma Azure AD birleştirmesini tamamlamak [için, kesintisiz SSO'nuzu da yapılandırmanız](../hybrid/how-to-connect-sso-quick-start.md#step-2-enable-the-feature)gerekir.

### <a name="install-microsoft-workplace-join-for-windows-down-level-computers"></a>Windows alt düzey bilgisayarlar için Microsoft İşyeri Birleştirme'yi yükleme

Windows alt düzey aygıtları kaydetmek için kuruluşların [Windows 10 olmayan bilgisayarlar için Microsoft İşyeri Birleştirme'yi](https://www.microsoft.com/download/details.aspx?id=53554)yüklemesi gerekir. Windows olmayan bilgisayarlar için Microsoft İşyeri Birleştirme, Microsoft İndirme Merkezi'nde kullanılabilir.

 [Paketi Microsoft Endpoint Configuration Manager](/configmgr/)gibi bir yazılım dağıtım sistemi kullanarak dağıtabilirsiniz. Paket `quiet` parametre ile standart sessiz kurulum seçeneklerini destekler. Configuration Manager'ın geçerli sürümü, tamamlanan kayıtları izleme olanağı gibi önceki sürümlere göre avantajlar sunar.

Yükleyici, kullanıcı bağlamında çalışan sistemde zamanlanmış bir görev oluşturur. Kullanıcı Windows'da kaydolduğunda görev tetiklenir. Görev, Azure AD ile kimlik doğrulaması yaptıktan sonra kullanıcı kimlik bilgilerini kullanarak aygıtı Azure AD ile sessizce birleştirir.

## <a name="verify-the-registration"></a>Kaydı doğrulama

Azure kiracınızdaki aygıt kayıt durumunu **[Get-MsolDevice'u](/powershell/msonline/v1/get-msoldevice)** kullanarak doğrulayın. Bu cmdlet [Azure Active Directory PowerShell](/powershell/azure/install-msonlinev1?view=azureadps-2.0)modülündedir.

Servis ayrıntılarını kontrol etmek için **Get-MSolDevice** cmdlet'i kullandığınızda:

- Windows istemcisindeki kimlikle eşleşen **aygıt kimliğine** sahip bir nesne nin bulunması gerekir.
- **DeviceTrustType'ın** değeri **Etki Alanı Birleştirilmiştir.** Bu ayar, Azure AD portalındaki **Aygıtlar** sayfasında **ki Karma Azure AD'nin birleştirilmiş** durumuna eşdeğerdir.
- Koşullu Erişim'de kullanılan aygıtlar için **Etkin'in** değeri **True** ve **DeviceTrustLevel** **yönetilir.**

Hizmet ayrıntılarını kontrol etmek için:

1. Windows PowerShell'i yönetici olarak açın.
1. Azure `Connect-MsolService` kiracınıza bağlanmak için girin.  
1. `get-msoldevice -deviceId <deviceId>` yazın.
1. **Enabled** değerinin **True** olarak ayarlandığını doğrulayın.

## <a name="troubleshoot-your-implementation"></a>Uygulamanızda sorun giderme

Etki alanı yla birleştirilmiş Windows aygıtları için karma Azure AD birleştirmeyi tamamlama sorunlarıyla karşılaşırsanız, bkz:

- [Karma Azure Etkin Dizin'ine katılan aygıtları sorun giderme](troubleshoot-hybrid-join-windows-current.md)
- [Karma Azure Etkin Dizini'nde sorun giderme, alt düzey aygıtları birleştirdi](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure portalını kullanarak aygıt kimliklerini nasıl yöneteceklerini öğrenmek için bir sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Cihaz kimliklerini yönetme](device-management-azure-portal.md)
