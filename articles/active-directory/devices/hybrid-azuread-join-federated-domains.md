---
title: Federe etki alanları için karma Azure Active Directory join'i yapılandırma | Microsoft Dokümanlar
description: Federal etki alanları için karma Azure Active Directory join'i nasıl yapılandıracaklarını öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: tutorial
ms.date: 05/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a61c89199c89f09b5cc0e553dbbf48655ad1b6a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239101"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-federated-domains"></a>Öğretici: Federasyon etki alanları için hibrit Azure Active Directory'ye katılımı yapılandırma

Kuruluşunuzdaki bir kullanıcı gibi, aygıt da korumak istediğiniz temel bir kimliktir. Kaynaklarınızı istediğiniz zaman ve herhangi bir konumdan korumak için aygıtın kimliğini kullanabilirsiniz. Aşağıdaki yöntemlerden birini kullanarak aygıt kimliklerini getirerek ve Azure Etkin Dizini'nde (Azure AD) yöneterek bu hedefe ulaşabilirsiniz:

- Azure AD'ye katılım
- Hibrit Azure AD'ye katılım
- Azure AD kaydı

Cihazlarınızı Azure AD'ye getirmek, bulutunuzda ve şirket içi kaynaklarınızda tek oturum açma (SSO) aracılığıyla kullanıcı üretkenliğini en üst düzeye çıkarır. [Koşullu Erişim](../active-directory-conditional-access-azure-portal.md) ile bulutlarınıza ve şirket içi kaynaklarınıza erişimi aynı anda güvenli hale getirebilirsiniz.

Federe bir ortamda, aşağıdaki gereksinimleri destekleyen bir kimlik sağlayıcısı olmalıdır. Active Directory Federation Services (AD FS) kullanarak federe bir ortamınız varsa, aşağıdaki gereksinimler zaten desteklenir.

- **WIAORMULTIAUTHN iddiası:** Bu talep, Windows alt düzey aygıtlar için karma Azure AD join yapmak için gereklidir.
- **WS-Trust protokolü:** Bu protokol, Azure AD ile windows geçerli karma Azure AD birleştirilmiş aygıtları doğrulamak için gereklidir.
  AD FS kullanırken, aşağıdaki WS-Trust uç noktalarını etkinleştirmeniz gerekir:`/adfs/services/trust/2005/windowstransport`
   `/adfs/services/trust/13/windowstransport`
   `/adfs/services/trust/2005/usernamemixed`
   `/adfs/services/trust/13/usernamemixed`
   `/adfs/services/trust/2005/certificatemixed`
   `/adfs/services/trust/13/certificatemixed` 

> [!WARNING] 
> Hem **adfs/services/trust/2005/windowstransport** hem de **adfs/services/trust/13/windowstransport,** intranet uç noktalarına bakan intranet olarak etkinleştirilmeli ve Web Application Proxy aracılığıyla uç noktalara bakan extranet olarak açıklanmamalıdır. WS-Trust Windows uç noktalarını nasıl devre dışı kalacaksınız hakkında daha fazla bilgi edinmek için [proxy'deki WS-Trust Windows uç noktalarını devre dışı etme bilgisini](/windows-server/identity/ad-fs/deployment/best-practices-securing-ad-fs#disable-ws-trust-windows-endpoints-on-the-proxy-ie-from-extranet)edinin. **Service** > **Endpoints**altında AD FS yönetim konsolu aracılığıyla hangi uç noktaların etkinleştirildigini görebilirsiniz.

Bu eğitimde, AD FS kullanarak federe bir ortamda Active Directory etki alanı birleştirilmiş bilgisayar aygıtları için karma Azure AD join yapılandırmayı öğrenirsiniz.

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Hibrit Azure AD'ye katılımı yapılandırma
> * Windows alt düzey aygıtları etkinleştirme
> * Kaydı doğrulama
> * Sorun giderme

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, bu makalelere aşina olduğunuzu varsayar:

- [Cihaz kimliği nedir?](overview.md)
- [Karma Azure AD'nizin uygulamayla nasıl planlanması?](hybrid-azuread-join-plan.md)
- [Karma Azure AD join'in kontrollü doğrulaması nasıl yapılacak?](hybrid-azuread-join-control.md)

Bu öğreticide senaryoyu yapılandırmak için şunlar gereklidir:

- AD FS içeren Windows Server 2012 R2
- [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) sürümü 1.1.819.0 veya sonrası

Azure AD Connect, sürüm 1.1.819.0 ile başlayarak, karma Azure AD birleştirmesini yapılandırmak için kullanabileceğiniz bir sihirbaz içerir. Sihirbaz yapılandırma işlemini önemli ölçüde basitleştirir. İlgili sihirbaz:

- Cihaz kaydı için servis bağlantı noktalarını (SCP' ler) yapılandırır
- Mevcut Azure AD bağlı olan tarafınıza yedekleme yapar
- Azure AD Trust'ınızdaki talep kurallarını güncelleştirir

Bu makaledeki yapılandırma adımları Azure AD Connect sihirbazını kullanmaya dayanır. Azure AD Connect'in daha önceki bir sürümü yüklüyse, sihirbazı kullanmak için 1.1.819 veya daha sonra yükseltmeniz gerekir. Azure AD Connect'in en son sürümünü yüklemek sizin için bir seçenek değilse, [karma Azure AD birleştirmeyi el ile nasıl yapılandırabileceğinize](hybrid-azuread-join-manual.md)bakın.

Karma Azure AD join, aygıtların kuruluşunuzun ağından aşağıdaki Microsoft kaynaklarına erişebulaşmasını gerektirir:  

- `https://enterpriseregistration.windows.net`
- `https://login.microsoftonline.com`
- `https://device.login.microsoftonline.com`
- Kuruluşunuzun Güvenlik Belirteç Hizmeti (STS) (Federe etki alanları için)
- `https://autologon.microsoftazuread-sso.com`(Sorunsuz SSO kullanıyorsanız veya kullanmayı planlıyorsanız)

Windows 10 1803'ten başlayarak, anlık karma Azure AD'sı AD FS kullanarak federe bir ortama katılırsa, azure AD'deki bilgisayar nesnesini daha sonra karma Azure için aygıt kaydını tamamlamak için kullanılan bilgisayar nesnesini eşitlemek için Azure AD Connect'e güveniriz AD katılmak. Azure AD Connect'in karma Azure AD olmak istediğiniz aygıtların bilgisayar nesnelerini Azure AD ile eşitlediğini doğrulayın. Bilgisayar nesneleri belirli kuruluş birimlerine (OS) aitse, OS B'leri Azure AD Connect'te eşitlemek üzere yapılandırmanız gerekir. Azure AD Connect'i kullanarak bilgisayar nesnelerini nasıl eşitleyeceklerini öğrenmek için Azure [AD Connect'i kullanarak filtreyi yapılandır'a](../hybrid/how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering)bakın.

Kuruluşunuz giden bir proxy üzerinden Internet erişimine ihtiyaç varsa, Microsoft, Windows 10 bilgisayarlarını Azure AD ile aygıt kaydı için etkinleştirmek için [Web Proxy Otomatik Bulma (WPAD) uygulamasını](/previous-versions/tn-archive/cc995261(v%3dtechnet.10)) önerir. WPAD'i yapılandıran ve yöneten sorunlarla karşılaşırsanız, [Sorun Giderme otomatik algılama](/previous-versions/tn-archive/cc302643(v=technet.10))konusuna bakın. 

WPAD kullanmıyorsanız ve bilgisayarınızda proxy ayarlarını yapılandırmak istiyorsanız, bunu Windows 10 1709 ile başlayarak yapabilirsiniz. Daha fazla bilgi için, [bir grup ilkesi nesnesi (GPO) kullanarak WinHTTP ayarlarını yapılandırın.](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/)

> [!NOTE]
> WinHTTP ayarlarını kullanarak bilgisayarınızdaki proxy ayarlarını yapılandırırsanız, yapılandırılan proxy'ye bağlanamayan bilgisayarlar internete bağlanmaz.

Kuruluşunuz kimlik doğrulaması giden bir proxy aracılığıyla Internet erişimi gerektiriyorsa, Windows 10 bilgisayarlarınızın giden proxy'ye başarılı bir şekilde kimlik doğrulayabilmesini sağlamalısınız. Windows 10 bilgisayarları makine bağlamı kullanarak aygıt kaydı çalıştırdığından, makine bağlamı kullanarak giden proxy kimlik doğrulamasını yapılandırmanız gerekir. Yapılandırma gereksinimleri için giden bağlantı proxy'si sağlayıcınızı izleyin.

Aygıtın sistem hesabı altında yukarıdaki Microsoft kaynaklarına erişip erişemeyebileceğini doğrulamak için [Test Cihazı Kaydı Bağlantı](https://gallery.technet.microsoft.com/Test-Device-Registration-3dc944c0) komut dosyasını kullanabilirsiniz.

## <a name="configure-hybrid-azure-ad-join"></a>Hibrit Azure AD'ye katılımı yapılandırma

Azure AD Connect'i kullanarak karma bir Azure REKLAM birleştirme yapılandırmak için şunları yapmanız gerekir:

- Azure AD kiracınız için genel bir yöneticinin kimlik bilgileri  
- Ormanların her biri için kuruluş yöneticisi kimlik bilgileri
- AD FS yöneticinizin kimlik bilgileri

**Azure AD Connect'i kullanarak karma Bir Azure REKLAM birleştirmesini yapılandırmak için:**

1. Azure AD Connect'i başlatın ve sonra **Yapıla'yı**seçin.

   ![Hoş Geldiniz](./media/hybrid-azuread-join-federated-domains/11.png)

1. Ek **görevler** sayfasında, **aygıtı yapılandır'ı**seçin ve **sonra İleri'yi**seçin.

   ![Ek görevler](./media/hybrid-azuread-join-federated-domains/12.png)

1. Genel **Bakış** sayfasında **İleri'yi**seçin.

   ![Genel Bakış](./media/hybrid-azuread-join-federated-domains/13.png)

1. Azure **AD'ye Bağlan** sayfasında, Azure AD kiracınız için genel bir yöneticinin kimlik bilgilerini girin ve ardından **İleri'yi**seçin.

   ![Azure AD'ye Bağlanma](./media/hybrid-azuread-join-federated-domains/14.png)

1. Aygıt **seçenekleri** sayfasında, **Karma Azure AD birleştirme'yi yapılandır'ı**ve **ardından İleri'yi**seçin.

   ![Cihaz seçenekleri](./media/hybrid-azuread-join-federated-domains/15.png)

1. **SCP** sayfasında, aşağıdaki adımları tamamlayın ve sonra **Sonraki'ni**seçin:

   ![SCP](./media/hybrid-azuread-join-federated-domains/16.png)

   1. Ormanı seçin.
   1. Kimlik doğrulama hizmetini seçin. Kuruluşunuz yalnızca Windows 10 istemcileri yoksa ve bilgisayar/aygıt eşitlemesi yapılandırmışsa veya kuruluşunuz sorunsuz SSO kullanmıyorsa **AD FS sunucususeçmeniz** gerekir.
   1. Kurumsal yönetici kimlik bilgilerini girmek için **Ekle'yi** seçin.

1. Aygıt **işletim sistemleri** sayfasında, Active Directory ortamınızdaki aygıtların kullandığı işletim sistemlerini seçin ve ardından **İleri'yi**seçin.

   ![Cihaz işletim sistemi](./media/hybrid-azuread-join-federated-domains/17.png)

1. Federasyon **yapılandırma** sayfasında, AD FS yöneticinizin kimlik bilgilerini girin ve sonra **İleri'yi**seçin.

   ![Federasyon yapılandırması](./media/hybrid-azuread-join-federated-domains/18.png)

1. **Yapılandırmaya Hazır** **sayfasında, Yapılandırma'yı**seçin.

   ![Yapılandırma için hazır](./media/hybrid-azuread-join-federated-domains/19.png)

1. Yapılandırma **tam** **sayfasında, Exit'i**seçin.

   ![Yapılandırma tamamlandı](./media/hybrid-azuread-join-federated-domains/20.png)

## <a name="enable-windows-downlevel-devices"></a>Windows alt düzey aygıtları etkinleştirme

Etki alanına katılan aygıtlarınızdan bazıları Windows'un alt düzey aygıtlarıysa, şunları yapmalısınız:

- Cihaz kaydı için yerel intranet ayarlarını yapılandırma
- Windows alt düzey bilgisayarlar için Microsoft İşyeri Birleştirme'yi yükleme

> [!NOTE]
> Windows 7 desteği 14 Ocak 2020 tarihinde sona erdi. Daha fazla bilgi [için Windows 7 desteği sona erdi.](https://support.microsoft.com/en-us/help/4057281/windows-7-support-ended-on-january-14-2020)

### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Cihaz kaydı için yerel intranet ayarlarını yapılandırma

Windows alt düzey aygıtlarınızın karma Azure REKLAM'ını başarıyla tamamlamak ve aygıtlar Azure AD'ye kimlik doğruladığında sertifika istemlerinden kaçınmak için, etki alanı yla birleştirilmiş aygıtlarınıza bir ilke ekleyerek Internet'teki yerel intranet bölgesine aşağıdaki URL'leri ekleyebilirsiniz Explorer:

- `https://device.login.microsoftonline.com`
- Kuruluşunuzun STS'si (Federe etki alanları için)
- `https://autologon.microsoftazuread-sso.com`(Sorunsuz SSO için)

Ayrıca, kullanıcının yerel intranet bölgesinde **komut dosyası aracılığıyla durum çubuğuna güncelleştirmelere izin ver'i** etkinleştirmeniz gerekir.

### <a name="install-microsoft-workplace-join-for-windows-downlevel-computers"></a>Windows alt düzey bilgisayarlar için Microsoft İşyeri Birleştirme'yi yükleme

Windows alt düzey aygıtları kaydetmek için kuruluşların [Windows 10 olmayan bilgisayarlar için Microsoft İşyeri Birleştirme'yi](https://www.microsoft.com/download/details.aspx?id=53554)yüklemesi gerekir. Windows olmayan bilgisayarlar için Microsoft İşyeri Birleştirme, Microsoft İndirme Merkezi'nde kullanılabilir.

 [Paketi Microsoft Endpoint Configuration Manager](/configmgr/)gibi bir yazılım dağıtım sistemi kullanarak dağıtabilirsiniz. Paket `quiet` parametre ile standart sessiz kurulum seçeneklerini destekler. Configuration Manager'ın geçerli şubesi, tamamlanan kayıtları izleme olanağı gibi önceki sürümlere göre avantajlar sunar.

Yükleyici, kullanıcı bağlamında çalışan sistemde zamanlanmış bir görev oluşturur. Kullanıcı Windows'da kaydolduğunda görev tetiklenir. Görev, Azure AD ile kimlik doğrulaması yaptıktan sonra kullanıcı kimlik bilgilerini kullanarak aygıtı Azure AD ile sessizce birleştirir.

## <a name="verify-the-registration"></a>Kaydı doğrulama

Azure kiracınızda cihaz kaydı durumunu doğrulamak için, [Azure Active Directory PowerShell modülünde](/powershell/azure/install-msonlinev1?view=azureadps-2.0)**[Get-MsolDevice](/powershell/msonline/v1/get-msoldevice)** cmdlet öğesini kullanabilirsiniz.

Servis ayrıntılarını kontrol etmek için **Get-MSolDevice** cmdlet'i kullandığınızda:

- Windows istemcisindeki kimlikle eşleşen **aygıt kimliğine** sahip bir nesne nin bulunması gerekir.
- **DeviceTrustType** değerinin **Etki Alanına Katılmış** olması gerekir. Bu ayar, Azure AD portalındaki **Aygıtlar** altında **birleştirilmiş Karma Azure AD** durumuna eşdeğerdir.
- Koşullu Erişim'de kullanılan aygıtlar **için, Etkin'in** değeri **Doğru** olmalı ve **DeviceTrustLevel** **yönetilmelidir.**

**Servis ayrıntılarını kontrol etmek için:**

1. Windows PowerShell'i yönetici olarak açın.
1. Azure `Connect-MsolService` kiracınıza bağlanmak için girin.  
1. `get-msoldevice -deviceId <deviceId>` yazın.
1. **Enabled** değerinin **True** olarak ayarlandığını doğrulayın.

## <a name="troubleshoot-your-implementation"></a>Uygulamanızda sorun giderme

Etki alanı yla birleştirilmiş Windows aygıtları için karma Azure AD birleştirmeyi tamamlamayla ilgili sorunlarla karşılaşırsanız, bkz:

- [Windows geçerli aygıtlar için karma Azure AD join sorun giderme](troubleshoot-hybrid-join-windows-current.md)
- [Windows alt düzey aygıtları için karma Azure AD birleştirme sorunu](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Sonraki adımlar

[Azure portalını kullanarak aygıt kimliklerini](device-management-azure-portal.md)nasıl yöneteceklerini öğrenin.

<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
