---
title: Koşullu Erişim Ne Olursa aracı - Azure Active Directory
description: Koşullu Erişim ilkelerinizin ortamınız üzerindeki etkisini nasıl anlayabileceğinizi öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 02/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: dc87b434664ba12cefeb233972e749f631d8a2b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77620677"
---
# <a name="troubleshoot-using-the-what-if-tool-in-conditional-access"></a>Koşullu Erişimde "Eğer" aracını kullanarak sorun giderme

[Koşullu Erişim,](../active-directory-conditional-access-azure-portal.md) Azure Etkin Dizin (Azure AD) özelliği, yetkili kullanıcıların bulut uygulamalarınıza nasıl erişeceklerini denetlemenize olanak tanıyan bir özelliktir. Ortamınızdaki Koşullu Erişim ilkelerinden ne bekleyeceğinizi nasıl biliyorsunuz? Bu soruyu yanıtlamak için **Koşullu Erişim Ne Varsa aracını**kullanabilirsiniz.

Bu makalede, Koşullu Erişim ilkelerinizi sınamak için bu aracı nasıl kullanabileceğiniz açıklanmaktadır.

## <a name="what-it-is"></a>Nedir?

**Koşullu Erişim Ne Olursa, ilke aracı** Koşullu Erişim ilkelerinizin ortamınız üzerindeki etkisini anlamanızı sağlar. Bu araç, birden çok oturum açma işlemi gerçekleştirerek ilkelerinizi test etmek yerine, bir kullanıcının simüle edilmiş oturum açma oturumunu değerlendirmenize olanak tanır. Benzetim, bu oturum açma işleminin ilkeleriniz üzerindeki etkisini tahmin eder ve bir benzetim raporu oluşturur. Rapor yalnızca uygulanan Koşullu Erişim ilkelerini değil, varsa [klasik ilkeleri](policy-migration.md#classic-policies) de listelemektedir.    

**Ne Varsa** aracı, belirli bir kullanıcı için geçerli olan ilkeleri hızla belirlemek için bir yol sağlar. Örneğin, bir sorunu gidermeniz gerekiyorsa, bilgileri kullanabilirsiniz.    

## <a name="how-it-works"></a>Nasıl çalışır?

**Koşullu Erişim Ne Varsa aracında,** öncelikle simüle etmek istediğiniz oturum açma senaryosunun ayarlarını yapılandırmanız gerekir. Bu ayarlar şunlardır:

- Test etmek istediğiniz kullanıcı 
- Kullanıcının erişmeye çalışacağı bulut uygulamaları
- Yapılandırılan bulut uygulamalarına erişimin hangi koşullar altında gerçekleştirildiği
     
Bir sonraki adım olarak, ayarlarınızı değerlendiren bir simülasyon çalışması başlatabilirsiniz. Yalnızca etkinleştirilen ilkeler bir değerlendirme çalışmasının bir parçasıdır.

Değerlendirme tamamlandığında, araç etkilenen ilkeler hakkında bir rapor oluşturur.

## <a name="running-the-tool"></a>Aracı çalıştırma

**[Koşullu Erişim - İlkeler](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies)** sayfasında **"Eğer"** aracını Azure portalında bulabilirsiniz.

Aracı başlatmak için, ilkeler listesinin üst kısmındaki araç çubuğunda **Ne Olur'u**tıklatın.

![What If](./media/what-if-tool/01.png)

Bir değerlendirme yi çalıştıramadan önce ayarları yapılandırmanız gerekir.

## <a name="settings"></a>Ayarlar

Bu bölümde simülasyon çalıştırma ayarları hakkında bilgi verilmektedir.

![What If](./media/what-if-tool/02.png)

### <a name="user"></a>Kullanıcı

Yalnızca bir kullanıcı seçebilirsiniz. Bu gerekli tek alandır.

### <a name="cloud-apps"></a>Bulut uygulamaları

Bu ayar için varsayılan **tüm bulut uygulamaları.** Varsayılan ayar, ortamınızdaki kullanılabilir tüm ilkelerin değerlendirmesini gerçekleştirir. Kapsamı belirli bulut uygulamalarını etkileyen ilkelere daraltabilirsiniz.

### <a name="ip-address"></a>IP adresi

IP [adresi, konum koşulunu](location-condition.md)taklit etmek için tek bir IPv4 adresidir. Adres, kullanıcınız tarafından oturum açmada kullanılan aygıtın Internet'e bakan adresini temsil eder. Bir aygıtın IP adresini, [örneğin, IP adresim in](https://whatismyipaddress.com)ilerleyerek doğrulayabilirsiniz.    

### <a name="device-platforms"></a>Cihaz platformları

Bu ayar [aygıt platformları koşulunu](concept-conditional-access-conditions.md#device-platforms) taklit eder ve **tüm platformların eşdeğerini (desteklenmeyen**ler dahil) temsil eder. 

### <a name="client-apps"></a>İstemci uygulamaları

Bu ayar [istemci uygulamaları koşulunu](concept-conditional-access-conditions.md#client-apps-preview)taklit eder.
Varsayılan olarak, bu ayar **Tarayıcı** veya Mobil **uygulamaları ve masaüstü istemcileri** tek tek veya her ikisi de seçili olan tüm ilkelerin değerlendirilmesi neden olur. Exchange **ActiveSync (EAS)** uygulayan ilkeleri de algılar. Bu ayarı şu seçerek daraltabilirsiniz:

- **Tarayıcı** en az **Tarayıcı** seçili olan tüm ilkeleri değerlendirmek için. 
- **Mobil uygulamalar ve masaüstü istemcileri** en az **Mobil uygulamalar ve masaüstü istemcileri** seçilen olan tüm ilkeleri değerlendirmek için. 

### <a name="sign-in-risk"></a>Oturum açma riski

Bu ayar [oturum açma risk durumunu](concept-conditional-access-conditions.md#sign-in-risk)taklit eder.   

## <a name="evaluation"></a>Değerlendirme 

**What If'i**tıklatarak bir değerlendirmeye başlarsınız. Değerlendirme sonucu size aşağıdakilerden oluşan bir rapor sağlar: 

![What If](./media/what-if-tool/03.png)

- Ortamınızda klasik ilkelerin var olup olmadığının bir göstergesi
- Kullanıcınız için geçerli olan ilkeler
- Kullanıcınız için geçerli olmayan ilkeler

Seçili bulut uygulamaları için [klasik ilkeler](policy-migration.md#classic-policies) varsa, size bir gösterge sunulur. Göstergeyi tıklatarak, klasik ilkeler sayfasına yönlendirilirsiniz. Klasik ilkeler sayfasında, klasik bir ilkeyi geçirebilirsiniz veya devre dışı bırakabilirsiniz. Bu sayfayı kapatarak değerlendirme sonucunuza dönebilirsiniz.

Seçili kullanıcınız için geçerli olan ilkeler listesinde, kullanıcınızın karşılaması gereken [hibe denetimleri](concept-conditional-access-grant.md) ve [oturum denetimlerinin](concept-conditional-access-session.md) bir listesini de bulabilirsiniz.

Kullanıcınız için geçerli olmayan ilkeler listesinde, bu ilkelerin neden geçerli olmadığının nedenlerini de bulabilirsiniz. Listelenen her ilke için, neden memnun olmayan ilk koşulu temsil eder. Uygulanmayan bir ilke için olası bir nedeni, daha fazla değerlendirilmedikleri için devre dışı bırakılmış bir ilkedir.   

## <a name="next-steps"></a>Sonraki adımlar

- Koşullu Erişim ilkesini nasıl yapılandırdığınızı bilmek istiyorsanız, [Azure Active Directory Koşullu Erişim'e sahip belirli uygulamalar için MFA'yı zorunlu kılmasını gerektir](app-based-mfa.md)ini.
- Ortamınız için Koşullu Erişim ilkelerini yapılandırmaya hazırsanız, [Azure Etkin Dizini'nde Koşullu Erişim için en iyi uygulamalara](best-practices.md)bakın. 
- Klasik ilkeleri geçirmek istiyorsanız, Azure [portalında klasik ilkeleri geçir'e](policy-migration.md) bakın  
