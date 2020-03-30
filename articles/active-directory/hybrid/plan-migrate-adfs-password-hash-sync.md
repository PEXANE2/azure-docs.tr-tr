---
title: "Azure AD Bağlantısı: Azure AD için federasyondan PHS'ye geçiş | Microsoft Dokümanlar"
description: Bu makalede, karma kimlik ortamınızı federasyondan parola karma senkronizasyonuna taşıma hakkında bilgiler vardır.
services: active-directory
author: billmath
manager: daveba
ms.reviewer: martincoetzer
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 05/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: b621c9cbc35d0e9956f6648d870102affd84c24f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76028398"
---
# <a name="migrate-from-federation-to-password-hash-synchronization-for-azure-active-directory"></a>Azure Active Directory için federasyondan parola karma eşitlemesine geçiş

Bu makalede, kuruluş etki alanlarınızın Active Directory Federation Services'dan (AD FS) parola karma eşitlemesine nasıl taşınacağı açıklanmaktadır.

> [!NOTE]
> Kimlik doğrulama yönteminizi değiştirmek için planlama, sınama ve olası kapalı kalma süresi gerekir. [Aşamalı kullanıma göre,](how-to-connect-staged-rollout.md) parola karma eşitlemesi kullanarak test etmek ve yavaş yavaş federasyondan bulut kimlik doğrulamasına geçiş yapmak için alternatif bir yol sağlar.

## <a name="prerequisites-for-migrating-to-password-hash-synchronization"></a>Parola karma senkronizasyonuna geçiş için ön koşullar

Aşağıdaki önkoşullar AD FS kullanarak şifre karma senkronizasyon kullanarak geçiş için gereklidir.

### <a name="update-azure-ad-connect"></a>Azure AD Bağlantısını Güncelleştir

Parola karma eşitlemesine geçiş adımlarını başarıyla gerçekleştirmek için en az olarak, Azure AD'nin 1.1.819.0'a [bağlanmasını](https://www.microsoft.com/download/details.aspx?id=47594) gerekir. Bu sürüm, oturum açma dönüşüm işleminin gerçekleştirilme şeklinde önemli değişiklikler içerir ve Federasyon'dan Bulut Kimlik Doğrulamasına geçiş için toplam süreyi olası saatlerden dakikaya azaltır.


> [!IMPORTANT]
> Etki alanlarını federe kimlikten yönetilen kimliğe dönüştürdüğünüzde kullanıcı dönüştürmenin gerekli olduğu eski belgelerde, araçlarda ve bloglarda okuyabilirsiniz. *Kullanıcıları dönüştürmeye* artık gerek yoktur. Microsoft, bu değişikliği yansıtacak belgeleri ve araçları güncelleştirmek için çalışmaktadır.

Azure AD Connect'i güncelleştirmek için Azure AD Connect' teki adımları [tamamlayın: En son sürüme yükseltin.](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version)

### <a name="password-hash-synchronization-required-permissions"></a>Parola karma eşitleme gerekli izinler

Azure AD Connect'i ekspres ayarları veya özel yükleme yi kullanarak yapılandırabilirsiniz. Özel yükleme seçeneğini kullandıysanız, parola karma eşitlemesi için [gerekli izinler](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-accounts-permissions) yerinde olmayabilir.

Azure AD Connect Active Directory Domain Services (AD DS) hizmet hesabı, parola açıklarını eşitlemek için aşağıdaki izinleri gerektirir:

* Dizin Değişikliklerini Çoğaltma
* Dizin Değişikliklerini Tümünü Çoğaltma

Şimdi, bu izinlerin ormandaki tüm etki alanları için yerinde olduğunu doğrulamak için iyi bir zaman.

### <a name="plan-the-migration-method"></a>Geçiş yöntemini planlama

Federe kimlik yönetiminden parola karma senkronizasyonuna ve sorunsuz tek oturum açma (SSO) için geçiş yapmak için iki yöntem arasından seçim yapabilirsiniz. Kullandığınız yöntem, AD FS örneğinizin başlangıçta nasıl yapılandırıldığına bağlıdır.

* **Azure AD Connect**. AD FS'yi ilk olarak Azure AD Connect'i kullanarak yapılandırıyorsanız, Azure AD Connect sihirbazını kullanarak parola karma senkronizasyonuna geçiş *yapmak zorundasınız.*

   Azure AD Connect, kullanıcı oturum açma yöntemini değiştirdiğinizde **Set-MsolDomainAuthentication** cmdlet'i otomatik olarak çalıştırAr. Azure AD Connect, Azure AD kiracınızdaki doğrulanmış tüm federe etki alanlarını otomatik olarak kullanılamaz.

   > [!NOTE]
   > Şu anda, AD FS yapılandırmak için azure AD Connect'i ilk kez kullandıysanız, kullanıcı oturum açma'yı parola karma senkronizasyonuolarak değiştirdiğinizde kiracınızdaki tüm etki alanlarını federating'den kaçınamazsınız. ‎
* **Azure AD PowerShell ile bağlanın.** Bu yöntemi yalnızca Azure AD Connect'i kullanarak AD FS'yi ilk olarak yapılandırmadıysanız kullanabilirsiniz. Bu seçenek için, Azure AD Connect sihirbazı aracılığıyla kullanıcı oturum açma yöntemini değiştirmeniz gerekir. Bu seçenekteki temel fark, sihirbazın **Set-MsolDomainAuthentication** cmdlet'i otomatik olarak çalıştırmamasıdır. Bu seçenekle, hangi etki alanlarının ve hangi sırayla dönüştürüldüğü üzerinde tam denetime sahip siniz.

Hangi yöntemi kullanmanız gerektiğini anlamak için aşağıdaki bölümlerdeki adımları tamamlayın.

#### <a name="verify-current-user-sign-in-settings"></a>Geçerli kullanıcı oturum açma ayarlarını doğrulama

Geçerli kullanıcı oturum açma ayarlarınızı doğrulamak için:

1. Bir Global Administrator hesabı kullanarak [Azure AD portalında](https://aad.portal.azure.com/) oturum açın.
2. Kullanıcı **oturum açma** bölümünde aşağıdaki ayarları doğrulayın:
   * **Federasyon** **Etkin**olarak ayarlanır.
   * **Sorunsuz tek oturum açma** **Devre Dışı olarak**ayarlanır.
   * **Geçiş kimlik doğrulaması** **Devre Dışı bırakılmış**olarak ayarlanır.

   ![Azure AD Connect Kullanıcı oturum açma bölümündeki ayarların ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image1.png)

#### <a name="verify-the-azure-ad-connect-configuration"></a>Azure AD Connect yapılandırmasını doğrulama

1. Azure AD Connect sunucunuzda Azure AD Connect'i açın. **Yapılandır**'ı seçin.
2. Ek **görevler** **sayfasında, geçerli yapılandırmayı görüntüle'yi**seçin ve ardından **İleri'yi**seçin.<br />

   ![Ek görevler sayfasında seçilen geçerli yapılandırma seçeneğini görüntüle ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image2.png)<br />
3. **Çözümünüzü Gözden Geçir** sayfasında Parola **karma eşitleme** durumuna dikkat edin.<br /> 

   * **Parola karma eşitlemesi** Devre **Dışı olarak**ayarlanmışsa, etkinleştirmek için bu makaledeki adımları tamamlayın.
   * **Parola karma eşitleme** **etkin**olarak ayarlanmışsa, Bu makalede adım 1: Bu makalede parola **karma eşitlemesini etkinleştir'** bölümünü atlayabilirsiniz.
4. Çözüm **sayfanızı gözden geçirin,** **Active Directory Federation Services (AD FS)** sayfasına gidin.<br />

   * AD FS yapılandırması bu bölümde görünüyorsa, AD FS'nin ilk olarak Azure AD Connect kullanılarak yapılandırıldığını güvenle varsayabilirsiniz. Azure AD Connect **Change kullanıcı oturum açma** seçeneğini kullanarak etki alanlarınızı federe kimlikten yönetilen kimliğe dönüştürebilirsiniz. İşlem, **A Seçeneği: Azure AD Connect'i kullanarak federasyondan parola karma senkronizasyonuna geçiş**bölümünde ayrıntılı olarak açıklanmıştır.
   * AD FS geçerli ayarlarda listelenmiyorsa, PowerShell'i kullanarak etki alanlarınızı el ile federe kimlikten yönetilen kimliğe dönüştürmeniz gerekir. Bu işlem hakkında daha fazla bilgi için, **Azure AD Connect ve PowerShell'i kullanarak Seçenek B: Federasyondan parola karma senkronizasyonuna geçiş**bölümüne bakın.

### <a name="document-current-federation-settings"></a>Geçerli federasyon ayarlarını belgeleme

Geçerli federasyon ayarlarınızı bulmak için **Get-MsolDomainFederationSettings** cmdlet'i çalıştırın:

``` PowerShell
Get-MsolDomainFederationSettings -DomainName YourDomain.extention | fl *
```

Örnek:

``` PowerShell
Get-MsolDomainFederationSettings -DomainName Contoso.com | fl *
```

Federasyon tasarım ve dağıtım belgeleriniz için özelleştirilmiş olabilecek ayarları doğrulayın. Özellikle, **PreferredAuthenticationProtocol**özelleştirmeler için bakmak , **SupportsMfa**, ve **PromptLoginBehavior**.

Daha fazla bilgi için şu makalelere bakın:

* [AD FS istemi=giriş parametresi desteği](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-prompt-login)
* [Set-MsolDomainAuthentication](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainauthentication?view=azureadps-1.0)

> [!NOTE]
> **SupportsMfa** **True**olarak ayarlanmışsa, kullanıcı kimlik doğrulama akışına ikinci faktörlü bir sorun enjekte etmek için şirket içi çok faktörlü bir kimlik doğrulama çözümü kullanıyorsunuz. Bu kurulum, bu etki alanını federeden yönetilen kimlik doğrulamasına dönüştürdükten sonra Azure AD kimlik doğrulama senaryolarında artık çalışmaz. Federasyonu devre dışı attıktan sonra, şirket içi federasyonunuzla olan ilişkiyi kesersiniz ve bu şirket içi MFA bağdaştırıcılarını içerir. 
>
> Bunun yerine, aynı işlevi gerçekleştirmek için Azure Çok Faktörlü Kimlik Doğrulama bulut tabanlı hizmetini kullanın. Devam etmeden önce çok faktörlü kimlik doğrulama gereksinimlerinizi dikkatle değerlendirin. Etki alanlarınızı dönüştürmeden önce, Azure Çok Faktörlü Kimlik Doğrulaması'nı, lisanslama sonuçlarını ve kullanıcı kayıt işlemini nasıl kullanacağınızı anladığınızdan emin olun.

#### <a name="back-up-federation-settings"></a>Federasyon ayarlarını yedekleme

Bu makalede açıklanan işlemler sırasında AD FS çiftliğinizdeki diğer güvenen taraflarda herhangi bir değişiklik yapılmasa da, ad FS çiftliğinizin güncel geçerli bir yedeklemesine sahip olduğunuzu ve geri yükleyebileceğinizi öneririz. Ücretsiz Microsoft [AD FS Hızlı Geri Yükleme Aracı'nı](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-rapid-restore-tool)kullanarak geçerli bir yedekleme oluşturabilirsiniz. Aracı, AD FS'yi yedeklemek ve varolan bir çiftliği geri yüklemek veya yeni bir çiftlik oluşturmak için kullanabilirsiniz.

AD FS Hızlı Geri Yükleme Aracı'nı en azından kullanmamayı seçerseniz, parti güvenine ve eklediğiniz ilişkili özel talep kurallarına güvenerek Microsoft Office 365 Kimlik Platformu'nu dışa aktarmanız gerekir. Aşağıdaki PowerShell örneğini kullanarak, güvenen taraf güvenini ve ilişkili talep kurallarını dışa aktarabilirsiniz:

``` PowerShell
(Get-AdfsRelyingPartyTrust -Name "Microsoft Office 365 Identity Platform") | Export-CliXML "C:\temp\O365-RelyingPartyTrust.xml"
```

## <a name="deployment-considerations-and-using-ad-fs"></a>Dağıtım hususları ve AD FS kullanma

Bu bölümde dağıtım konuları ve AD FS kullanma yla ilgili ayrıntılar açıklanmaktadır.

### <a name="current-ad-fs-use"></a>Geçerli AD FS kullanımı

Federe kimlikten yönetilen kimliğe dönüştürmeden önce, şu anda Azure AD, Office 365 ve diğer uygulamalar (parti güvenlerine güvenerek) için AD FS'yi şu anda nasıl kullandığınızı yakından inceleyin. Özellikle, aşağıdaki tabloda açıklanan senaryoları göz önünde bulundurun:

| Eğer | Ardından |
|-|-|
| AD FS'yi diğer uygulamalarla (Azure AD ve Office 365 dışında) kullanmaya devam etmeyi planlıyorsunuz. | Etki alanlarınızı dönüştürdükten sonra hem AD FS hem de Azure AD kullanırsınız. Kullanıcı deneyimini göz önünde bulundurun. Bazı senaryolarda, kullanıcıların iki kez kimlik doğrulaması gerekebilir: bir kez Azure AD'ye (bir kullanıcının Office 365 gibi diğer uygulamalara SSO erişimi aldığı yerde) ve yine güvenen taraf güveni olarak AD FS'ye bağlı olan uygulamalar için. |
| AD FS örneğiniz ağır bir şekilde özelleştirilmiştir ve onload.js dosyasındaki belirli özelleştirme ayarlarına dayanır (örneğin, oturum açma deneyimini, kullanıcıların Kullanıcı Adı (UPN) yerine kullanıcı adı için yalnızca **SamAccountName** biçimini kullanmaları için değiştirdiyseniz veya kuruluşunuz oturum açma deneyimini ağır şekilde damgaladı). onload.js dosyası Azure AD'de çoğaltılamaz. | Devam etmeden önce, Azure AD'nin geçerli özelleştirme gereksinimlerinizi karşıladığını doğrulamanız gerekir. Daha fazla bilgi ve rehberlik için, AD FS markalaşma ve AD FS özelleştirme bölümlerine bakın.|
| Kimlik doğrulama istemcilerinin önceki sürümlerini engellemek için AD FS'yi kullanırsınız.| [Koşullu Erişim denetimleri](https://docs.microsoft.com/azure/active-directory/conditional-access/conditions) ve [Exchange Online İstemci Erişim Kuralları'nın](https://aka.ms/EXOCAR)bir birleşimini kullanarak kimlik doğrulama istemcilerinin önceki sürümlerini engelleyen AD FS denetimlerini değiştirmeyi düşünün. |
| Kullanıcıların AD FS'ye kimlik doğrulaması yaptığında, kullanıcıların şirket içi çok faktörlü kimlik doğrulama sunucusu çözümüne karşı çok faktörlü kimlik doğrulama gerçekleştirmelerini gerekir.| Yönetilen bir kimlik etki alanında, şirket içi çok faktörlü kimlik doğrulama çözümü aracılığıyla kimlik doğrulama akışına çok faktörlü kimlik doğrulama sorunu enjekte edemezsiniz. Ancak, etki alanı dönüştürüldükten sonra çok faktörlü kimlik doğrulama için Azure Çok Faktörlü Kimlik Doğrulama hizmetini kullanabilirsiniz.<br /><br /> Kullanıcılarınız şu anda Azure Çok Faktörlü Kimlik Doğrulaması kullanmıyorsa, tek seferlik bir kullanıcı kaydı adımı gereklidir. Planlanan kaydı hazırlayıp kullanıcılarınıza iletmelidir. |
| Şu anda AD FS'de Office 365'e erişimi denetlemek için erişim denetimi ilkelerini (AuthZ kuralları) kullanıyorsunuz.| İlkeleri eşdeğer Azure AD [Koşullu Erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) ve [Exchange Online İstemci Erişim Kuralları](https://aka.ms/EXOCAR)ile değiştirmeyi düşünün.|

### <a name="common-ad-fs-customizations"></a>Ortak AD FS özelleştirmeleri

Bu bölümde, yaygın AD FS özelleştirmeleri açıklanmaktadır.

#### <a name="insidecorporatenetwork-claim"></a>InsideCorporateNetwork iddiası

AD FS, kimlik doğrulaması yapan kullanıcı şirket ağının içindeyse **InsideCorporateNetwork** iddiasını yayınlar. Bu talep daha sonra Azure AD'ye aktarılabilir. Talep, kullanıcının ağ konumuna göre çok faktörlü kimlik doğrulamasını atlamak için kullanılır. Bu işlevselliğin şu anda AD FS'de etkin olup olmadığını belirlemek [için federe kullanıcılar için Güvenilir IP'lere](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud)bakın.

Alan adlarınız parola karma senkronizasyonuna dönüştürüldükten sonra **InsideCorporateNetwork** talebi kullanılamaz. Bu işlevselliği değiştirmek için [Azure AD'de adlandırılmış konumları](https://docs.microsoft.com/azure/active-directory/active-directory-named-locations) kullanabilirsiniz.

Adlandırılmış konumları yapılandırdıktan sonra, ağ **Tüm güvenilen konumlar** veya **MFA Trusted IP** değerlerini yeni adlandırılmış konumları yansıtacak şekilde içerecek veya hariç tutmak üzere yapılandırılan tüm Koşullu Erişim ilkelerini güncelleştirmeniz gerekir.

Koşullu Erişim'deki **Konum** durumu hakkında daha fazla bilgi için [Active Directory Koşullu Erişim konumlarını](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations)görün.

#### <a name="hybrid-azure-ad-joined-devices"></a>Karma Azure AD'ye bağlı aygıtlar

Bir aygıta Azure AD'ye katıldığınızda, aygıtların güvenlik ve uyumluluk için erişim standartlarınızı karşılamasını zorunlu kınlayan Koşullu Erişim kuralları oluşturabilirsiniz. Ayrıca, kullanıcılar kişisel bir hesap yerine kuruluş çalışması veya okul hesabı kullanarak bir aygıtta oturum açabilir. Karma Azure AD'ye bağlı aygıtları kullandığınızda, Etkin Dizin etki alanı birleştirilmiş aygıtlarınızı Azure AD'ye katılabilirsiniz. Federe ortamınız bu özelliği kullanmak üzere ayarlanmış olabilir.

Karma birleştirmenin etki alanlarınız parola karma senkronizasyonuna dönüştürüldükten sonra etki alanına katılan tüm aygıtlar için çalışmaya devam etmesini sağlamak için, Windows 10 istemcileri için Active Directory bilgisayarını eşitlemek için Azure AD Connect aygıt seçeneklerini kullanmanız gerekir hesapları Azure AD'ye sunar. 

Windows 8 ve Windows 7 bilgisayar hesapları için karma birleştirme, bilgisayarı Azure AD'ye kaydetmek için sorunsuz SSO kullanır. Windows 10 aygıtları için yaptığınız gibi Windows 8 ve Windows 7 bilgisayar hesaplarını eşitlemeniz gerekmemektedir. Ancak, sorunsuz SSO kullanarak kendilerini kaydedebilmeleri için güncelleştirilmiş bir workplacejoin.exe dosyasını (.msi dosyası üzerinden) Windows 8 ve Windows 7 istemcilerine dağıtmanız gerekir. [.msi dosyasını indirin.](https://www.microsoft.com/download/details.aspx?id=53554)

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup)

#### <a name="branding"></a>Markalama

Kuruluşunuz [AD FS oturum açma sayfalarınızı](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-user-sign-in-customization) kuruluşla daha ilgili bilgileri görüntülemek için özelleştirmişse, [Azure AD oturum açma sayfasına](https://docs.microsoft.com/azure/active-directory/customize-branding)benzer özelleştirmeler yapmayı düşünün.

Benzer özelleştirmeler mevcut olsa da, dönüşümden sonra oturum açma sayfalarında bazı görsel değişiklikler beklenmelidir. Kullanıcılara iletilerinizde beklenen değişiklikler hakkında bilgi vermek isteyebilirsiniz.

> [!NOTE]
> Kuruluş markası yalnızca Azure Active Directory için Premium veya Temel lisans satın aldığınızda veya Office 365 lisansınvarsa sahipseniz kullanılabilir.

## <a name="plan-deployment-and-support"></a>Dağıtım ve desteği planlayın

Dağıtım ve destek için plan yaptığınızda yardımcı olmak için bu bölümde açıklanan görevleri tamamlayın.

### <a name="plan-the-maintenance-window"></a>Bakım penceresini planlayın

Etki alanı dönüştürme işlemi nispeten hızlı olsa da, Azure AD etki alanı dönüştürme işlemi tamamlandıktan sonra dört saate kadar AD FS sunucularınıza bazı kimlik doğrulama istekleri göndermeye devam edebilir. Bu dört saatlik süre boyunca ve çeşitli hizmet tarafı önbelleklerine bağlı olarak Azure AD bu kimlik doğrulamalarını kabul etmeyebilir. Kullanıcılar bir hata alabilir. Kullanıcı AD FS'ye karşı hala başarılı bir şekilde kimlik doğrulaması yapabilir, ancak Azure AD artık kullanıcının verilen belirteci kabul etmez, çünkü bu federasyon güveni artık kaldırılmıştır.

Yalnızca bu dönüşüm sonrası pencerede hizmetlere bir web tarayıcısı üzerinden erişen kullanıcılar, hizmet yan önbelleği temizlenmeden önce etkilenir. Exchange Online belirli bir süre için kimlik bilgilerinin önbelleğini tuttuğundan, eski istemcilerin (Exchange ActiveSync, Outlook 2010/2013) etkilenmesi beklenmiyor. Önbellek, kullanıcıyı sessizce yeniden doğrulamak için kullanılır. Kullanıcının AD FS'ye dönmesi gerekmez. Bu istemciler için aygıtta depolanan kimlik bilgileri, önbelleğe alındıktan sonra sessizce kendilerini yeniden doğrulamak için kullanılır. Kullanıcıların etki alanı dönüştürme işlemi nin bir sonucu olarak herhangi bir parola istemleri almaları beklenmiyor. 

Modern kimlik doğrulama istemcileri (Office 2016 ve Office 2013, iOS ve Android uygulamaları), AD FS'ye dönmek yerine kaynaklara sürekli erişim için yeni erişim belirteçleri elde etmek için geçerli bir yenileme belirteci kullanır. Bu istemciler etki alanı dönüştürme işleminden kaynaklanan herhangi bir parola istemlerine karşı bağışıktır. İstemciler ek yapılandırma olmadan çalışmaya devam edecektir.

> [!IMPORTANT]
> Tüm kullanıcıların bulut kimlik doğrulaması kullanarak başarılı bir şekilde kimlik doğrulaması yapabileceklerini doğrulayana kadar AD FS ortamınızı kapatmayın veya Office 365 güvendiğiniz parti güvenini kaldırmayın.

### <a name="plan-for-rollback"></a>Geri alma planı

Hızlı bir şekilde çözemediğiniz önemli bir sorunla karşılaşırsanız, çözümü federasyona geri döndürmeye karar verebilirsiniz. Dağıtımınız beklendiği gibi kullanıma sunmazsa ne yapacağınızı planlamak önemlidir. Etki alanının veya kullanıcıların dönüşümü dağıtım sırasında başarısız olursa veya federasyona geri dönmeniz gerekiyorsa, herhangi bir kesintinin nasıl azaltılabildiğini ve kullanıcılarınız üzerindeki etkisini nasıl azaltacağınızı anlamanız gerekir.

#### <a name="to-roll-back"></a>Geri dönmek için

Geri alma planı yapmak için, belirli dağıtım ayrıntılarınız için federasyon tasarım ve dağıtım belgelerini denetleyin. İşlem şu görevleri içermelidir:

* **Convert-MSOLDomainToFederated** cmdlet kullanarak yönetilen etki alanlarını federe etki alanına dönüştürme.
* Gerekirse, ek talep kuralları yapılandırma.

### <a name="plan-communications"></a>İletişimi planlama

Dağıtım ve desteğin planlanmasının önemli bir bölümü, kullanıcılarınızın yaklaşan değişiklikler hakkında proaktif olarak bilgilendirilmesini sağlamaktır. Kullanıcılar neler yaşayabileceklerini ve neleri deneyimlediklerini önceden bilmelidir. 

Hem parola karma eşitleme ve sorunsuz SSO dağıtıldıktan sonra, Office 365'e ve Azure AD değişiklikleri yle kimlik doğrulaması yapılan diğer kaynaklara erişmek için kullanıcı oturum açma deneyimi. Ağın dışında kalan kullanıcılar yalnızca Azure AD oturum açma sayfasını görür. Bu kullanıcılar, dışa bakan web uygulaması proxy sunucuları tarafından sunulan form tabanlı sayfaya yönlendirilir.

İletişim stratejinize aşağıdaki öğeleri ekleyin:

* Kullanıcıları yaklaşan ve yayımlanan işlevsellik hakkında şu ları kullanarak bilgilendirin:
   * E-posta ve diğer iç iletişim kanalları.
   * Posterler gibi görseller.
   * Yönetici, canlı veya diğer iletişim.
* İletişimi kimin özelleştireceğini ve iletişimi kimin ve ne zaman göndereceğini belirleyin.

## <a name="implement-your-solution"></a>Çözümünüzü uygulayın

Çözümünüzü planladınız. Şimdi, artık bunu uygulayabilirsiniz. Uygulama aşağıdaki bileşenleri içerir:

* Parola karma eşitlemesini etkinleştirme.
* Sorunsuz SSO için hazırlanıyor.
* Oturum açma yöntemini parola karma senkronizasyonuna değiştirme ve sorunsuz SSO'yu etkinleştirme.

### <a name="step-1-enable-password-hash-synchronization"></a>Adım 1: Parola karma eşitlemesini etkinleştirme

Bu çözümü uygulamanın ilk adımı, Azure AD Connect sihirbazını kullanarak parola karma senkronizasyonunu etkinleştirmektir. Parola karma eşitleme, federasyon kullanan ortamlarda etkinleştirebileceğiniz isteğe bağlı bir özelliktir. Kimlik doğrulama akışının hiçbir etkisi yoktur. Bu durumda, Azure AD Connect, federasyon kullanarak oturum açan kullanıcıları etkilemeden parola gruplarını eşitlemeye başlar.

Bu nedenle, etki alanınızın oturum açma yöntemini değiştirmeden önce bu adımı bir hazırlık görevi olarak tamamlamanızı öneririz. Ardından, parola karma eşitlemenin doğru çalıştığını doğrulamak için yeterli zamana sahip olursunuz.

Parola karma senkronizasyonunu etkinleştirmek için:

1. Azure AD Connect sunucusunda Azure AD Connect sihirbazını açın ve sonra **Yapıla'yı**seçin.
2. **Eşitleme seçeneklerini özelleştir'i**seçin ve sonra **İleri'yi**seçin.
3. **Azure'a Bağlan AD** sayfasında, Global Administrator hesabının kullanıcı adını ve parolasını girin.
4. **Dizinlerinizi Bağla** sayfasında **İleri'yi**seçin.
5. Etki **Alanı ve OU filtreleme** sayfasında **İleri'yi**seçin.
6. **İsteğe Bağlı özellikler** sayfasında **Parola eşitlemesi'ni**seçin ve ardından **İleri'yi**seçin.
 
   ![İsteğe Bağlı özellikler sayfasında seçilen Parola senkronizasyon seçeneğinin ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image6.png)<br />
7. Kalan sayfalarda **İleri'yi** seçin. Son sayfada, **Yapıla'yı**seçin.
8. Azure AD Connect, bir sonraki eşitlemede parola gruplarını eşitlemeye başlar.

Parola karma eşitleme etkinleştirildikten sonra, Azure AD Connect eşitleme kapsamındaki tüm kullanıcıların parola karmaları yeniden etkinleştirilir ve Azure AD'ye yazılır. Kullanıcı sayısına bağlı olarak, bu işlem birkaç dakika veya birkaç saat sürebilir.

Planlama amacıyla, yaklaşık 20.000 kullanıcının 1 saat içinde işlenir tahmin etmelidir.

Parola karma eşitlemenin doğru çalıştığını doğrulamak için Azure AD Connect sihirbazındaki **Sorun Giderme** görevini tamamlayın:

1. Yönetici olarak Çalıştır seçeneğini kullanarak Azure AD Connect sunucunuzda yeni bir Windows PowerShell oturumu açın.
2. Çalıştır `Set-ExecutionPolicy RemoteSigned` `Set-ExecutionPolicy Unrestricted`ya da .
3. Azure AD Bağlantısı sihirbazını başlatın.
4. **Ek görevler** sayfasına gidin, **Sorun Gider'i**seçin ve sonra **İleri'yi**seçin.
5. Sorun **Giderme** sayfasında, PowerShell'deki sorun giderme menüsünü başlatmak için **Başlat'ı** seçin.
6. Ana menüde **Sorun Giderme parola karma senkronizasyonu'nu**seçin.
7. Alt menüde **Parola karma senkronizasyonu'nu**seçin.

Sorun giderme sorunları için, [Azure AD Connect eşitleme ile Sorun Giderme parola karma senkronizasyonu'na](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-hash-synchronization)bakın.

### <a name="step-2-prepare-for-seamless-sso"></a>Adım 2: Sorunsuz SSO'ya hazırlanın

Aygıtlarınızın sorunsuz SSO kullanabilmesi için, Etkin Dizini'nde bir grup ilkesi kullanarak kullanıcıların intranet bölge ayarlarına bir Azure REKLAM URL'si eklemeniz gerekir.

Varsayılan olarak, web tarayıcıları bir URL'den internet veya intranet gibi doğru bölgeyi otomatik olarak hesaplar. Örneğin, **http:\/\/** intranet bölgesine kontoso/ haritalar ve **\/\/http: internet** bölgesine intranet.contoso.com haritalar (URL bir dönem içerdiğinden). Tarayıcılar, Kerberos biletlerini Azure REKLAM URL'si gibi bir bulut bitiş noktasına gönderir, ancak URL'yi tarayıcının intranet bölgesine açıkça eklerseniz.

Aygıtlarınızda gerekli değişiklikleri [tamamlamak](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start) için adımları tamamlayın.

> [!IMPORTANT]
> Bu değişikliği yapmak, kullanıcılarınızın Azure AD'de oturum açma şeklini değiştirmez. Ancak, devam etmeden önce bu yapılandırmayı tüm aygıtlarınıza uygulamanız önemlidir. Bu yapılandırmayı almamış cihazlarda oturum açan kullanıcıların Azure AD'de oturum açabilmek için bir kullanıcı adı ve parola girmeleri yeterlidir.

### <a name="step-3-change-the-sign-in-method-to-password-hash-synchronization-and-enable-seamless-sso"></a>Adım 3: Oturum açma yöntemini parola karma senkronizasyonuyla değiştirin ve sorunsuz SSO'yu etkinleştirin

Oturum açma yöntemini parola karma senkronizasyonuna değiştirmek ve sorunsuz SSO'yu etkinleştirmek için iki seçeneğiniz vardır.

#### <a name="option-a-switch-from-federation-to-password-hash-synchronization-by-using-azure-ad-connect"></a>Seçenek A: Azure AD Connect'i kullanarak federasyondan parola karma senkronizasyonuna geçiş

Ad FS ortamınızı azure AD Connect kullanarak yapılandırıyorsanız bu yöntemi kullanın. Azure AD Connect'i kullanarak AD FS ortamınızı *yapılandırmadıysanız* bu yöntemi kullanamazsınız.

İlk olarak, oturum açma yöntemini değiştirin:

1. Azure AD Connect sunucusunda Azure AD Connect sihirbazını açın.
2. **Kullanıcı oturum aç'ı değiştir'i**seçin ve sonra **İleri'yi**seçin. 

   ![Ek görevler sayfasında kullanıcı oturum açma seçeneğini değiştir'in ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image7.png)<br />
3. **Azure'a Bağlan AD** sayfasında, Global Administrator hesabının kullanıcı adını ve parolasını girin.
4. Kullanıcı **oturum açma** sayfasında Parola **karma senkronizasyon düğmesini**seçin. **Kullanıcı hesaplarını dönüştürme onay** kutusunu seçtiğinizden emin olun. Seçenek küçümsüyor. **Tek oturum aç'ı etkinleştir'i**ve ardından **İleri'yi**seçin.

   ![Tek oturum açma sayfası etkinleştir'in ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image8.png)<br />

   > [!NOTE]
   > Azure AD Connect sürüm 1.1.880.0 ile başlayarak, Varsayılan olarak **Sorunsuz tek oturum açma** onay kutusu seçilir.

   > [!IMPORTANT]
   > Kullanıcı dönüştürme ve tam parola karma eşitlemenin federasyondan bulut kimlik doğrulamasına dönüştürmek için gerekli adımlar olduğunu belirten uyarıları güvenle yok sayabilirsiniz. Bu adımların artık gerekli olmadığını unutmayın. Bu uyarıları hala görüyorsanız, Azure AD Connect'in en son sürümünü çalıştırdığınızdan ve bu kılavuzun en son sürümünü kullandığınızdan emin olun. Daha fazla bilgi için Azure [AD Bağlantısını Güncelleştir](#update-azure-ad-connect)bölümüne bakın.

5. Tek **oturum açma** yı etkinleştir sayfasında, Etki Alanı Yöneticisi hesabının kimlik bilgilerini girin ve sonra **İleri'yi**seçin.

   ![Tek oturum açma sayfası etkinleştir'in ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image9.png)<br />

   > [!NOTE]
   > Bağımsız SSO sağlamak için Etki Alanı Yöneticisi hesap kimlik bilgileri gereklidir. İşlem, bu yüksek izinleri gerektiren aşağıdaki eylemleri tamamlar. Etki Alanı Yöneticisi hesap kimlik bilgileri Azure AD Connect'te veya Azure AD'de depolanmıyor. Etki Alanı Yöneticisi hesap kimlik bilgileri yalnızca özelliği açmak için kullanılır. İşlem başarıyla tamamlandığında kimlik bilgileri atılır.
   >
   > 1. Şirket içi Active Directory örneğinde AZUREADSSOACC (Azure AD'yi temsil eden) adlı bir bilgisayar hesabı oluşturulur.
   > 2. Bilgisayar hesabının Kerberos şifre çözme anahtarı Azure AD ile güvenli bir şekilde paylaşılır.
   > 3. Azure AD oturum açma sırasında kullanılan iki URL'yi temsil etmek üzere iki Kerberos hizmet temel adı (SPN) oluşturulur.

6. **Yapılandırmaya Hazır** sayfasında, yapılandırma onay kutusunu **tamamladığında eşitleme işlemini başlat'ın** seçildiğinden emin olun. Ardından, **Yapıl'ı**seçin.

      ![Yapılandırmaya Hazır sayfanın ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image10.png)<br />

   > [!IMPORTANT]
   > Bu noktada, tüm federe etki alanlarınız yönetilen kimlik doğrulamasına göre değişir. Parola karma eşitleme kimlik doğrulama yeni yöntemdir.

7. Azure AD portalında **Azure Active Directory** > **Azure AD Connect'i**seçin.
8. Bu ayarları doğrulayın:
   * **Federasyon** Devre **Dışı bırakılaayarlanır.**
   * **Sorunsuz tek oturum açma** **Etkin**olarak ayarlanır.
   * **Parola Eşitleme** **Etkin**olarak ayarlanır.<br /> 

   ![Kullanıcı oturum açma bölümündeki ayarları gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image11.png)<br />

[Sınama ve sonraki adımlara](#testing-and-next-steps)atlayın.

   > [!IMPORTANT]
   > **Seçenek B: Azure AD Connect ve PowerShell'i kullanarak federasyondan parola karma senkronizasyonuna geçiş**seçeneğini atlayın. Oturum açma yöntemini parola karma senkronizasyonuyla değiştirmek ve sorunsuz SSO'yu etkinleştirmek için Seçenek A'yı seçtiyseniz, bu bölümdeki adımlar geçerli değildir.

#### <a name="option-b-switch-from-federation-to-password-hash-synchronization-using-azure-ad-connect-and-powershell"></a>Seçenek B: Azure AD Connect ve PowerShell'i kullanarak federasyondan parola karma senkronizasyonuna geçiş

Başlangıçta federal etki alanlarınızı Azure AD Connect'i kullanarak yapılandırmadıysanız bu seçeneği kullanın. Bu işlem sırasında, sorunsuz SSO'ya olanak sağlar ve etki alanlarınızı federeden yönetilene değiştirirsiniz.

1. Azure AD Connect sunucusunda Azure AD Connect sihirbazını açın.
2. **Kullanıcı oturum aç'ı değiştir'i**seçin ve sonra **İleri'yi**seçin.
3. **Azure'a Bağlan AD** sayfasında, Global Administrator hesabının kullanıcı adını ve parolasını girin.
4. Kullanıcı **oturum açma** sayfasında Parola **karma senkronizasyon** düğmesini seçin. **Tek oturum aç'ı etkinleştir'i**ve ardından **İleri'yi**seçin.

   Parola karma senkronizasyonunu etkinleştirmeden önce: ![Kullanıcı oturum açma sayfasında yapılandırma seçeneğini gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image12.png)<br />

   Parola karma senkronizasyonunu etkinleştirdikten ![sonra: Kullanıcı oturum açma sayfasında yeni seçenekler gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image13.png)<br />
   
   > [!NOTE]
   > Azure AD Connect sürüm 1.1.880.0 ile başlayarak, Varsayılan olarak **Sorunsuz tek oturum açma** onay kutusu seçilir.

5. Tek **oturum açma** yı etkinleştir sayfasında, Etki Alanı Yöneticisi hesabının kimlik bilgilerini girin ve sonra **İleri'yi**seçin.

   > [!NOTE]
   > Bağımsız SSO sağlamak için Etki Alanı Yöneticisi hesap kimlik bilgileri gereklidir. İşlem, bu yüksek izinleri gerektiren aşağıdaki eylemleri tamamlar. Etki Alanı Yöneticisi hesap kimlik bilgileri Azure AD Connect'te veya Azure AD'de depolanmıyor. Etki Alanı Yöneticisi hesap kimlik bilgileri yalnızca özelliği açmak için kullanılır. İşlem başarıyla tamamlandığında kimlik bilgileri atılır.
   >
   > 1. Şirket içi Active Directory örneğinde AZUREADSSOACC (Azure AD'yi temsil eden) adlı bir bilgisayar hesabı oluşturulur.
   > 2. Bilgisayar hesabının Kerberos şifre çözme anahtarı Azure AD ile güvenli bir şekilde paylaşılır.
   > 3. Azure AD oturum açma sırasında kullanılan iki URL'yi temsil etmek üzere iki Kerberos hizmet temel adı (SPN) oluşturulur.

6. **Yapılandırmaya Hazır** sayfasında, yapılandırma onay kutusunu **tamamladığında eşitleme işlemini başlat'ın** seçildiğinden emin olun. Ardından, **Yapıl'ı**seçin.

   ![Yapılandırmaya Hazır sayfasında ki Yapılandırma düğmesini gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image15.png)<br />
   **Yapılandırma** düğmesini seçtiğinizde, sorunsuz SSO önceki adımda belirtildiği gibi yapılandırılır. Parola karma eşitleme yapılandırması daha önce etkinleştirildiğinden değiştirilmez.

   > [!IMPORTANT]
   > Şu anda kullanıcıların oturum açma şeklinde değişiklik yapılmaz.

7. Azure AD portalında aşağıdaki ayarları doğrulayın:
   * **Federasyon** **Etkin**olarak ayarlanır.
   * **Sorunsuz tek oturum açma** **Etkin**olarak ayarlanır.
   * **Parola Eşitleme** **Etkin**olarak ayarlanır.

   ![Kullanıcı oturum açma bölümündeki ayarları gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image16.png)

#### <a name="convert-domains-from-federated-to-managed"></a>Etki alanlarını federeden yönetilene dönüştürme

Bu noktada, federasyon hala etkin ve etki alanlarınız için çalışır durumda. Dağıtıma devam etmek için her etki alanının federeden parola karma eşitleme yoluyla kullanıcı kimlik doğrulamasını zorlamaya dönüştürülmesi gerekir.

> [!IMPORTANT]
> Tüm etki alanlarını aynı anda dönüştürmeniz gerekmez. Üretim kiracınızda bir test etki alanıyla başlamayı veya en düşük kullanıcı sayısına sahip etki alanınızla başlamayı seçebilirsiniz.

Azure AD PowerShell modüllerini kullanarak dönüşümü tamamlayın:

1. PowerShell'de, Bir Global Administrator hesabı kullanarak Azure AD'de oturum açın.
2. İlk etki alanını dönüştürmek için aşağıdaki komutu çalıştırın:

   ``` PowerShell
   Set-MsolDomainAuthentication -Authentication Managed -DomainName <domain name>
   ```

3. Azure AD portalında **Azure Active Directory** > **Azure AD Connect'i**seçin.
4. Etki alanının aşağıdaki komutu çalıştırarak yönetilene dönüştürüldüğünü doğrulayın:

   ``` PowerShell
   Get-MsolDomain -DomainName <domain name>
   ```

## <a name="testing-and-next-steps"></a>Test ve sonraki adımlar

Parola karma eşitlemasını doğrulamak ve dönüştürme işlemini tamamlamak için aşağıdaki görevleri tamamlayın.

### <a name="test-authentication-by-using-password-hash-synchronization"></a>Parola karma eşitleme kullanarak kimlik doğrulamayı test edin 

Kiracınız federe kimlik kullandığında, kullanıcılar Azure AD oturum açma sayfasından AD FS ortamınıza yönlendirildi. Kiracı, federal kimlik doğrulaması yerine parola karma senkronizasyonu kullanacak şekilde yapılandırıldığından, kullanıcılar AD FS'ye yönlendirilmemiştir. Bunun yerine, kullanıcılar doğrudan Azure AD oturum açma sayfasında oturum açabilirsiniz.

Parola karma senkronizasyonunu test etmek için:

1. Internet Explorer'ı InPrivate modunda açın, böylece sorunsuz SSO sizi otomatik olarak oturum açmaz.
2. Office 365 oturum açma sayfasına[https://portal.office.com](https://portal.office.com/)gidin ( ).
3. Bir kullanıcı UPN girin ve sonra **İleri'yi**seçin. Şirket içi Active Directory örneğiniz tarafından senkronize edilen ve daha önce federe kimlik doğrulaması kullanan karma bir kullanıcının UPN'ini girdiğinizden emin olun. Kullanıcı adını ve parolayı girdiğiniz sayfa görüntülenir:

   ![Kullanıcı adını girdiğiniz oturum açma sayfasını gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image18.png)

   ![Parola girdiğiniz oturum açma sayfasını gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image19.png)

4. Parolayı girdikten ve **Oturum Aç'ı**seçtikten sonra Office 365 portalına yönlendirilirsiniz.

   ![Office 365 portalını gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image20.png)


### <a name="test-seamless-sso"></a>Sorunsuz SSO'u test edin

1. Şirket ağına bağlı etki alanı birleştirilmiş makinede oturum açın.
2. Internet Explorer veya Chrome'da aşağıdaki URL'lerden birine gidin (etki alanınızla "contoso"yu değiştirin):

   * https:\/\/myapps.microsoft.com/contoso.com
   * https:\/\/myapps.microsoft.com/contoso.onmicrosoft.com

   Kullanıcı kısa bir süre için Azure AD oturum açma sayfasına yönlendirilir ve bu da "Oturum açmaya çalışmak" iletisini gösterir. Kullanıcıdan kullanıcı adı veya parola istenmez.<br />

   ![Azure AD oturum açma sayfasını ve iletisini gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image21.png)<br />
3. Kullanıcı yeniden yönlendirilir ve erişim panelinde başarıyla oturum adatır:

   > [!NOTE]
   > Sorunsuz SSO, etki alanı ipucunu destekleyen Office 365 hizmetlerinde çalışır (örneğin, myapps.microsoft.com/contoso.com). Şu anda, Office 365 portalı (portal.office.com) etki alanı ipuçlarını desteklemiyor. Kullanıcıların bir UPN girmeleri gerekmektedir. Bir UPN girildikten sonra, sorunsuz SSO kullanıcı adına Kerberos biletini alır. Kullanıcı bir şifre girmeden oturum açmış.

   > [!TIP]
   > Gelişmiş bir SSO deneyimi için [Windows 10'da Azure AD karma birleştirme](https://docs.microsoft.com/azure/active-directory/device-management-introduction) dağıtmayı düşünün.

### <a name="remove-the-relying-party-trust"></a>Güvenilen parti güvenini kaldırma

Tüm kullanıcıların ve istemcilerin Azure AD aracılığıyla başarılı bir şekilde kimlik doğrulaması yaptığınızı doğruladıktan sonra, Office 365 güvenilen parti güvenini kaldırmak güvenlidir.

AD FS'yi başka amaçlarla (diğer güvenen taraf güvenleri için) kullanmıyorsanız, bu noktada AD FS'nin devre dışı bırakılması güvenlidir.

### <a name="rollback"></a>Geri alma

Önemli bir sorun keşfederseniz ve bunu hızlı bir şekilde çözemezseniz, çözümü federasyona geri almayı seçebilirsiniz.

Belirli dağıtım ayrıntılarınız için federasyon tasarım ve dağıtım belgelerine başvurun. İşlem şu görevleri içermelidir:

* Yönetilen etki alanlarını **Convert-MSOLDomainToFederated** cmdlet kullanarak federe kimlik doğrulamasına dönüştürün.
* Gerekirse, ek talep kurallarını yapılandırın.

### <a name="sync-userprincipalname-updates"></a>Kullanıcıyı senkronize edinPrincipalName güncelleştirmeleri

Tarihsel olarak, şirket içi ortamdan eşitleme hizmetini kullanan **UserPrincipalName** özniteliğindeki güncelleştirmeler, bu koşulların her ikisi de doğru olmadığı sürece engellenir:

* Kullanıcı yönetilen (federe olmayan) bir kimlik etki alanındadır.
* Kullanıcıya lisans atanmadı.

Bu özelliği nasıl doğrulayamayınız veya nasıl açabilirsiniz öğrenmek için, [bkz.](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsyncservice-features)

### <a name="troubleshooting"></a>Sorun giderme

Destek ekibiniz, federasyondan yönetilene geçiş sırasında veya sonrasında ortaya çıkan kimlik doğrulama sorunlarını nasıl gidereceğini anlamalıdır. Destek ekibinizin sık karşılaşılan sorun giderme adımlarını ve sorunu yalıtmaya ve çözmeye yardımcı olabilecek uygun eylemleri tanımasına yardımcı olmak için aşağıdaki sorun giderme belgelerini kullanın.

[Sorun giderme Azure Active Directory parola karma senkronizasyon](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-hash-synchronization)

[Sorun Giderme Azure Active Directory Sorunsuz Tek Oturum Açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-sso)

## <a name="roll-over-the-seamless-sso-kerberos-decryption-key"></a>Sorunsuz SSO Kerberos şifre çözme anahtarı üzerinde rulo

AZUREADSSOACC bilgisayar hesabının (Azure AD'yi temsil eden) Kerberos şifre çözme anahtarını sık sık üzerinde yuvarlamak önemlidir. AZUREADSSOACC bilgisayar hesabı şirket içi Active Directory ormanınızda oluşturulur. Aktif Dizin etki alanı üyelerinin parola değişiklikleri gönderme şekliyle aynı hizaya gelmek için En az 30 günde bir Kerberos şifre çözme anahtarını yuvarlamanı öneririz. AZUREADSSOACC bilgisayar hesabı nesnesine bağlı ilişkili bir aygıt bulunmadığından, rollover'ı el ile gerçekleştirmeniz gerekir.

Azure AD Connect'i çalıştıran şirket içi sunucuda sorunsuz SSO Kerberos şifre çözme anahtarının rollover'ını başlatın.

Daha fazla bilgi için [azureadssoacc bilgisayar hesabının Kerberos şifre çözme anahtarını nasıl yuvarlayacağım bölümüne bakın?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-faq)

## <a name="next-steps"></a>Sonraki adımlar

* Azure [AD Connect tasarım kavramları](plan-connect-design-concepts.md)hakkında bilgi edinin.
* Doğru [kimlik doğrulaması](https://docs.microsoft.com/azure/security/fundamentals/choose-ad-authn)seçin.
* Desteklenen [topolojiler](plan-connect-design-concepts.md)hakkında bilgi edinin.
