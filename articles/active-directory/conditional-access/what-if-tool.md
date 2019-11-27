---
title: Koşullu erişim What If aracı-Azure Active Directory
description: Ortamınızdaki koşullu erişim ilkelerinizin etkisini nasıl anlayabileceğinizi öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: a5846934a8ad8455ca375b4bc54fc46d45aba1cd
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74379987"
---
# <a name="troubleshoot-using-the-what-if-tool-in-conditional-access"></a>Koşullu erişimde What If aracını kullanarak sorun giderme

[Koşullu erişim](../active-directory-conditional-access-azure-portal.md) , yetkili kullanıcıların bulut uygulamalarınıza nasıl erişebileceğini denetlemenize olanak tanıyan Azure Active Directory (Azure AD) özelliğidir. Ortamınızdaki koşullu erişim ilkelerinden ne bekleneceğinizi nasıl anlarsınız? Bu soruyu yanıtlamak için **koşullu erişim What If aracı**' nı kullanabilirsiniz.

Bu makalede, koşullu erişim ilkelerinizi test etmek için bu aracı nasıl kullanabileceğiniz açıklanır.

## <a name="what-it-is"></a>Nedir?

**Koşullu erişim What If ilkesi aracı** , ortamınızda koşullu erişim ilkelerinizin etkisini anlamanıza olanak tanır. İlkelerinizi test etmek için elle birden fazla oturum açma işlemi gerçekleştirmek yerine, bu aracı kullanarak bir kullanıcının oturum açmasının simülasyonunu yapabilirsiniz. Simülasyon, bu oturum açma işleminin ilkeleriniz üzerindeki etkisini tahmin eder ve bir simülasyon raporu oluşturur. Rapor yalnızca uygulanan koşullu erişim ilkelerini listelemez, ancak varsa [Klasik ilkeleri](policy-migration.md#classic-policies) de listeler.    

**What If** Aracı, belirli bir kullanıcı için uygulanan ilkeleri hızlıca belirleyebilmek için bir yol sağlar. Bu bilgileri, örneğin bir sorunu gidermeniz gerekiyorsa kullanabilirsiniz.    

## <a name="how-it-works"></a>Nasıl çalışır?

**Koşullu erişim What If aracında**, önce benzetimini yapmak istediğiniz oturum açma senaryosunun ayarlarını yapılandırmanız gerekir. Bu ayarlar şunlardır:

- Test etmek istediğiniz Kullanıcı 
- Kullanıcının erişmeyi deneyeceği bulut uygulamaları
- Bulut uygulamalarını yapılandıran erişimi olan koşullar gerçekleştiriliyor
     
Bir sonraki adım olarak, ayarlarınızı değerlendiren bir simülasyon çalıştırması başlatabilirsiniz. Yalnızca etkinleştirilen ilkeler bir değerlendirme çalıştırmasının parçasıdır.

Değerlendirme tamamlandığında, araç etkilenen ilkelerin bir raporunu oluşturur.

## <a name="running-the-tool"></a>Aracı çalıştırma

**What If** aracını, Azure Portal **[koşullu erişim ilkeleri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies)** sayfasında bulabilirsiniz.

Aracı başlatmak için, ilke listesinin üstündeki araç çubuğunda **What If**' ye tıklayın.

![What If](./media/what-if-tool/01.png)

Bir değerlendirmeyi çalıştırmadan önce, ayarları yapılandırmanız gerekir.

## <a name="settings"></a>Ayarlar

Bu bölüm, benzetim çalıştırmasının ayarları hakkında bilgi sağlar.

![What If](./media/what-if-tool/02.png)

### <a name="user"></a>Kullanıcı

Yalnızca bir kullanıcı seçebilirsiniz. Bu, tek gerekli alandır.

### <a name="cloud-apps"></a>Bulut uygulamaları

Bu ayar için varsayılan değer **tüm bulut**uygulamalardır. Varsayılan ayar, ortamınızdaki kullanılabilir tüm ilkelerin değerlendirmesini gerçekleştirir. Belirli bulut uygulamalarını etkileyen ilkeler için kapsamı daraltabilirsiniz.

### <a name="ip-address"></a>IP adresi

IP adresi, [konum koşulunu](location-condition.md)taklit eden tek bir IPv4 adresidir. Adres, Kullanıcı tarafından oturum açmak için kullanılan cihazın Internet 'e yönelik adresini temsil eder. Bir cihazın IP adresini, örneğin, [IP adresim](https://whatismyipaddress.com)'e giderek doğrulayabilirsiniz.    

### <a name="device-platforms"></a>Cihaz platformları

Bu ayar [cihaz platformları koşulunu](conditions.md#device-platforms) taklit eder ve **Tüm platformların (desteklenmeyen dahil)** eşdeğerini temsil eder. 

### <a name="client-apps"></a>İstemci uygulamaları

Bu ayar, [istemci uygulamaları koşulunu](conditions.md#client-apps)taklit eder.
Varsayılan olarak, bu ayar **tarayıcı** veya **mobil uygulamalar ve Masaüstü istemcileri** tek tek veya her ikisi de seçili olan tüm ilkelerin değerlendirilmesine neden olur. Ayrıca, **Exchange ActiveSync (EAS)** uygulayan ilkeleri de algılar. Şunları seçerek bu ayarı daraltabilirsiniz:

- En az **tarayıcı** seçili olan tüm ilkeleri değerlendirmek için **tarayıcı** . 
- **Mobil uygulamalar ve Masaüstü istemcileri** , en az **mobil uygulamalara ve Masaüstü istemcilerine** sahip tüm ilkeleri değerlendirmelidir. 

### <a name="sign-in-risk"></a>Oturum açma riski

Bu ayar, [oturum açma risk koşulunu](conditions.md#sign-in-risk)taklit eder.   

## <a name="evaluation"></a>Değerlendirmesinin 

**What If**' ye tıklayarak bir değerlendirme başlatabilirsiniz. Değerlendirme sonucu size şunları içeren bir rapor sağlar: 

![What If](./media/what-if-tool/03.png)

- Ortamınızda klasik ilkelerin var olup olmadığı göstergesi
- Kullanıcılarınız için uygulanan ilkeler
- Kullanıcılarınız için uygulanan ilkeler

Seçili bulut uygulamaları için [Klasik ilkeler](policy-migration.md#classic-policies) varsa, size bir gösterge sunulur. Göstergeye tıklayarak klasik ilkeler sayfasına yönlendirilirsiniz. Klasik ilkeler sayfasında, klasik bir ilkeyi geçirebilir veya devre dışı bırakabilirsiniz. Bu sayfayı kapatarak değerlendirme sonucuna dönebilirsiniz.

Seçtiğiniz kullanıcı için uygulanan ilke listesinde, kullanıcılarınızın karşılaması gereken [Denetim](controls.md#grant-controls) ve [oturum](controls.md#session-controls) denetimleri listesini de bulabilirsiniz.

Kullanıcılarınız için uygulanamadıkları ilkeler listesinde, bu ilkelerin neden uygulanamadığına ilişkin nedenleri de bulabilirsiniz. Listelenen her ilke için, nedeni karşılanmadı ilk koşulu temsil eder. Uygulanamayan bir ilke için olası bir neden, daha fazla değerlendirilmediğinden devre dışı bırakılmış bir ilkedir.   

## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesini nasıl yapılandıracağınızı öğrenmek isterseniz bkz. [koşullu erişim Azure Active Directory belirli uygulamalar IÇIN MFA gerektirme](app-based-mfa.md).
- Ortamınız için koşullu erişim ilkelerini yapılandırmaya hazırsanız, [Azure Active Directory Koşullu erişim için en iyi yöntemlere](best-practices.md)bakın. 
- Klasik ilkeleri geçirmek istiyorsanız, bkz [. Azure Portal klasik Ilkeleri geçirme](policy-migration.md)  
