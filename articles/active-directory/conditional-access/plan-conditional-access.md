---
title: Azure Etkin Dizini'nde Koşullu Erişim ilkelerini planlayın | Microsoft Dokümanlar
description: Bu makalede, Azure Etkin Dizini için Koşullu Erişim ilkelerini nasıl planladığınızı öğreneceksiniz.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: martincoetzer
ms.collection: M365-identity-device-management
ms.openlocfilehash: e1c75d5022432a9a57b30aabec4dd2c4f76f2f29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671824"
---
# <a name="how-to-plan-your-conditional-access-deployment-in-azure-active-directory"></a>Nasıl Olunması: Koşullu Erişim dağıtımınızı Azure Etkin Dizini'nde planlama

Koşullu Erişim dağıtımınızı planlamak, kuruluşunuzdaki uygulamalar ve kaynaklar için gerekli erişim stratejisini elde ettiğinizden emin olmak için çok önemlidir. Kullanıcılarınızın erişimini seçtiğiniz koşullar altında vermek veya engellemek için gereken çeşitli ilkeleri tasarlamak için dağıtımınızın planlama aşamasında zamanınızın çoğunu harcayın. Bu belge, güvenli ve etkili Koşullu Erişim ilkeleri uygulamak için atmanız gereken adımları açıklar. Başlamadan önce, [Koşullu Erişim'in](overview.md) nasıl çalıştığını ve ne zaman kullanmanız gerektiğini anladığınızdan emin olun.

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

Koşullu Erişim'i, tek başına bir özellik yerine kuruluşunuzun uygulamalarına ve kaynaklarına erişimi denetlemenize olanak tanıyan bir çerçeve olarak düşünün. Sonuç olarak, bazı Koşullu Erişim ayarları nın yapılandırılması için ek özellikler gerekir. Örneğin, belirli bir [oturum açma risk düzeyine](../identity-protection/howto-identity-protection-configure-risk-policies.md)yanıt veren bir ilke yapılandırabilirsiniz. Ancak, oturum açma risk düzeyini temel alan bir ilke, [Azure Etkin Dizin kimlik korumasının](../identity-protection/overview-identity-protection.md) etkinleştirilmesini gerektirir.

Ek özellikler gerekiyorsa, ilgili lisansları da almanız gerekebilir. Örneğin, Koşullu Erişim Azure AD Premium P1 özelliği yken, kimlik koruması için Azure AD Premium P2 lisansı gerekir.

Koşullu Erişim ilkelerinin iki türü vardır: taban çizgisi ve standart. [Temel ilke,](baseline-protection.md) önceden tanımlanmış bir Koşullu Erişim ilkesidir. Bu ilkelerin amacı, en azından temel güvenlik düzeyietkin olduğundan emin olmaktır. Temel ilkeler. Temel ilkeler Azure AD'nin tüm sürümlerinde kullanılabilir ve yalnızca sınırlı özelleştirme seçenekleri sağlar. Bir senaryo daha fazla esneklik gerektiriyorsa, temel ilkesini devre dışı edin ve gereksinimlerinizi özel bir standart ilkeyle uygulayın.

Standart bir Koşullu Erişim ilkesinde, ilkeyi iş gereksinimlerinize göre ayarlamak için tüm ayarları özelleştirebilirsiniz. Standart ilkeler için Azure AD Premium P1 lisansı gerekir.

>[!NOTE]
> İlk aygıt kimlik doğrulaması sonrasında en iyi zorlamayı elde etmek için Azure AD aygıt tabanlı Koşullu Erişim ilkesini kullanmanızı öneririz. Bu, aygıt uyumluluğu ve aygıt kodu akışının dışına düşerse oturumları kapatmayı içerir.

## <a name="draft-policies"></a>Taslak ilkeler

Azure Active Directory Koşullu Erişim, bulut uygulamalarınızın korumasını yeni bir düzeye getirmenizi sağlar. Bu yeni düzeyde, bir bulut uygulamasına nasıl erişebileceğiniz statik erişim yapılandırması yerine dinamik bir ilke değerlendirmesine dayanır. Koşullu Erişim ilkesiyle, bir erişim durumuna **(bu durumda)** bir yanıt**tanımlarsınız.**

![Sebep ve yanıt](./media/plan-conditional-access/10.png)

Bu planlama modelini kullanarak uygulamak istediğiniz her Koşullu Erişim ilkesini tanımlayın. Planlama egzersizi:

- Her ilke için yanıtları ve koşulları anahatlaşamanıza yardımcı olur.
- Kuruluşunuz için iyi belgelenmiş bir Koşullu Erişim ilkesi kataloğu yla sonuçlanır. 

İlke uygulamanızın kuruluşunuzun iş gereksinimlerini yansıtıp yansıtmadığını değerlendirmek için kataloğunuzu kullanabilirsiniz. 

Kuruluşunuz için Koşullu Erişim ilkeleri oluşturmak için aşağıdaki örnek şablonunu kullanın:

|*Bu* durumda:|O zaman *şunu*yap:|
|-|-|
|Bir erişim denemesi yapılır:<br>- Bir bulut uygulamasına*<br>- Kullanıcılara ve gruplara göre*<br>Kullan -arak:<br>- Koşul 1 (örneğin, Corp ağı dışında)<br>- Koşul 2 (örneğin, cihaz platformları)|Uygulamaya erişimi engelleme|
|Bir erişim denemesi yapılır:<br>- Bir bulut uygulamasına*<br>- Kullanıcılara ve gruplara göre*<br>Kullan -arak:<br>- Koşul 1 (örneğin, Corp ağı dışında)<br>- Koşul 2 (örneğin, cihaz platformları)|(AND) ile erişim izni verilmiştir:<br>- Gereksinim 1 (örneğin, MFA)<br>- Gereksinim 2 (örneğin, Cihaz uyumluluğu)|
|Bir erişim denemesi yapılır:<br>- Bir bulut uygulamasına*<br>- Kullanıcılara ve gruplara göre*<br>Kullan -arak:<br>- Koşul 1 (örneğin, Corp ağı dışında)<br>- Koşul 2 (örneğin, cihaz platformları)|(OR) ile erişim izni:<br>- Gereksinim 1 (örneğin, MFA)<br>- Gereksinim 2 (örneğin, Cihaz uyumluluğu)|

En azından, **bu durumda** bir bulut uygulamasına erişmeye çalışan asıl **(who)** tanımlanır (**ne**). Gerekirse, bir erişim denemesinin **nasıl** gerçekleştirilebileceğini de ekleyebilirsiniz. Koşullu Erişim'de, kim, ne ve nasıl koşulları tanımlayan öğeler kullanılır. Daha fazla bilgi için Azure [Etkin Dizin Koşullu Erişim'deki koşullar nelerdir?](concept-conditional-access-conditions.md) 

**Bunu sonra,** bir erişim durumuna ilkenizin yanıtını tanımlarsınız. Yanıtta, çok faktörlü kimlik doğrulama (MFA) gibi ek gereksinimlerle erişimi engeller veya verirsiniz. Tam bir genel bakış için, [Azure Etkin Dizin Koşullu Erişimi'nde erişim denetimleri nelerdir?](controls.md)  

Erişim denetimlerinizle koşulların birleşimi Koşullu Erişim ilkesini temsil eder.

![Sebep ve yanıt](./media/plan-conditional-access/51.png)

Daha fazla bilgi için, [bir ilke çalışması için gerekenlere](best-practices.md#whats-required-to-make-a-policy-work)bakın.

Bu noktada, ilkeleriniz için bir adlandırma standardına karar vermek için iyi bir zaman. Adlandırma standardı, ilkeleri Azure yönetici portalında açmadan bulmanıza ve amaçlarını anlamanıza yardımcı olur. Göstermek için ilkenizi adlandırın:

- Bir sıra numarası
- Uygulandığı bulut uygulaması
- Yanıt
- Kime uygulanır?
- Uygulandığı zaman (varsa)
 
![Adlandırma standardı](./media/plan-conditional-access/11.png)

Açıklayıcı bir ad, Koşullu Erişim uygulamanızın genel görünümünü tutmanıza yardımcı olsa da, bir konuşmada bir ilkeye başvurmanız gerekiyorsa sıra numarası yararlıdır. Örneğin, bir yönetici arkadaşıyla telefonda konuşursanız, bir sorunu çözmek için em063 ilkesini açmalarını isteyebilirsiniz.

Örneğin, aşağıdaki ad, Dynamics CRP uygulamasını kullanarak dış ağlardaki pazarlama kullanıcıları için ilkenin MFA gerektirdiğini belirtir:

`CA01 - Dynamics CRP: Require MFA For marketing When on external networks`

Etkin ilkelerinize ek olarak, [kesinti/acil durum senaryolarında](../authentication/concept-resilient-controls.md)ikincil esnek erişim denetimleri olarak hareket eden devre dışı bırakılmış ilkeler de uygulanması tavsiye edilir. Acil durum ilkeleri için adlandırma standardınız birkaç öğe daha içermelidir: 

- `ENABLE IN EMERGENCY`başında adı diğer politikalar arasında öne yapmak.
- Bozulmaadı için geçerli olmalıdır.
- Yöneticinin hangi sıra ilkelerinin etkinleştirilmesi gerektiğini bilmesine yardımcı olacak bir sıralama sırası numarası. 

Örneğin, aşağıdaki ad, bu ilkenin, MFA kesintisi varsa etkinleştirmeniz gereken dört ilkeden ilki olduğunu gösterir:

`EM01 - ENABLE IN EMERGENCY, MFA Disruption[1/4] - Exchange SharePoint: Require hybrid Azure AD join For VIP users`

## <a name="plan-policies"></a>İlkeleri planla

Koşullu Erişim ilkesi çözümünüzü planlarken, aşağıdaki sonuçlara ulaşmak için ilkeler oluşturmanız gerekip gerekmediğini değerlendirin. 

### <a name="block-access"></a>Hizmete erişimi

Erişimi engelleme seçeneği güçlüdür, çünkü aşağıdakiler:

- Bir kullanıcı için diğer tüm atamaları koz
- Tüm kuruluşunuzun kiracınıza oturum açmasını engelleme gücüne sahiptir
 
Tüm kullanıcılar için erişimi engellemek istiyorsanız, en azından bir kullanıcıyı (genellikle acil durum erişim hesapları) ilkeden hariç tutmalısınız. Daha fazla bilgi için [belirli kullanıcılara ve gruplara](block-legacy-authentication.md#select-users-and-cloud-apps)bakın.  

### <a name="require-mfa"></a>MFA gerektirme

Kullanıcılarınızın oturum açma deneyimini basitleştirmek için, kullanıcı adı ve parola kullanarak bulut uygulamalarınızda oturum açmalarına izin vermek isteyebilirsiniz. Ancak, genellikle, en azından daha güçlü bir hesap doğrulama biçimi gerektirmesi tavsiye edilen bazı senaryolar vardır. Koşullu Erişim ilkesiyle, MFA gereksinimini belirli senaryolarla sınırlandırabilirsiniz. 

MFA gerektiren yaygın kullanım örnekleri erişim şunlardır:

- [Yazar: yöneticiler](howto-baseline-protect-administrators.md)
- [Belirli uygulamalara](app-based-mfa.md) 
- [Ağ konumlarından, güvenmiyorsunuz.](untrusted-networks.md)

### <a name="respond-to-potentially-compromised-accounts"></a>Olası tehlikeye atılmış hesaplara yanıt verme

Koşullu Erişim ilkeleriyle, olası tehlikeye giren kimliklerden oturum açmalara otomatik yanıtlar uygulayabilirsiniz. Bir hesabın tehlikeye girme olasılığı risk düzeyleri şeklinde ifade edilir. Kimlik koruması tarafından hesaplanan iki risk düzeyi vardır: oturum açma riski ve kullanıcı riski. Oturum açma riskine yanıt uygulamak için iki seçeneğiniz var:

- Koşullu Erişim ilkesinde [oturum açma riski durumu](concept-conditional-access-conditions.md#sign-in-risk)
- [Kimlik](../identity-protection/howto-sign-in-risk-policy.md) korumada oturum açma risk ilkesi 

Oturum açma riskini koşul olarak ele almak, size daha fazla özelleştirme seçeneği sağladığından tercih edilen yöntemdir.

Kullanıcı risk düzeyi yalnızca kimlik korumada [kullanıcı risk ilkesi](../identity-protection/howto-user-risk-policy.md) olarak kullanılabilir. 

Daha fazla bilgi için Azure [Active Directory Identity Protection nedir?](../identity-protection/overview.md) 

### <a name="require-managed-devices"></a>Yönetilen cihazları gerektirme

Bulut kaynaklarınıza erişmek için desteklenen cihazların yaygınlaşması, kullanıcılarınızın üretkenliğini artırmaya yardımcı olur. Diğer taraftan, büyük olasılıkla ortamınızdaki belirli kaynaklara bilinmeyen koruma düzeyine sahip aygıtlar tarafından erişilmesini istemezsiniz. Etkilenen kaynaklar için, kullanıcıların bunlara yalnızca yönetilen bir aygıt kullanarak erişebilmeleri gerekir. Daha fazla bilgi için, [Koşullu Erişim ile bulut uygulaması erişimi için yönetilen aygıtları nasıl gerekli göreceğiz.](require-managed-devices.md) 

### <a name="require-approved-client-apps"></a>Onaylı istemci uygulamaları gerektirme

Kendi cihazlarınızı getirmek için yapmanız gereken ilk kararlardan biri (BYOD) senaryolar, tüm aygıtı veya yalnızca üzerindeki verileri yönetmeniz gerekip gerekmediğidir. Çalışanlarınız hem kişisel hem de iş amaçlı görevler için mobil cihazlar kullanır. Çalışanlarınızın üretken olmasını sağlamakla birlikte, veri kaybını da önlemek istersiniz. Azure Active Directory (Azure AD) Koşullu Erişim ile bulut uygulamalarınız için erişimi kurumsal verilerinizi koruyabilecek onaylı istemci uygulamalarıyla sınırlandırabilirsiniz. Daha fazla bilgi için, [Koşullu Erişim ile bulut uygulamasıerişimi için onaylı istemci uygulamalarını nasıl gerekli göreceğiz.](app-based-conditional-access.md)

### <a name="block-legacy-authentication"></a>Eski kimlik doğrulamasını engelleme

Azure AD, eski kimlik doğrulama dahil olmak üzere en yaygın olarak kullanılan kimlik doğrulama ve yetkilendirme protokollerinden birkaçını destekler. Eski kimlik doğrulamasını kullanan uygulamaların kiracınızın kaynaklarına erişmesini nasıl önleyebilirsiniz? Öneri, koşullu erişim ilkesiyle bunları engellemektir. Gerekirse, yalnızca belirli kullanıcıların ve belirli ağ konumlarının eski kimlik doğrulamasına dayalı uygulamaları kullanmasına izin verirsiniz. Daha fazla bilgi için, [Koşullu Erişim ile Azure AD'ye eski kimlik doğrulamasını nasıl engelleyeceksiniz'](block-legacy-authentication.md)a bakın.

## <a name="test-your-policy"></a>İlkinizi test edin

Bir ilkeyi üretime dönüştürmeden önce, beklendiği gibi iyi bir işlem olduğunu doğrulamak için test etmelisiniz.

1. Test kullanıcıları oluşturma
1. Test planı oluşturma
1. İlkeyi yapılandırma
1. Benzetimli oturum açma yı değerlendirme
1. İlkinizi test edin
1. Temizleme

### <a name="create-test-users"></a>Test kullanıcıları oluşturma

Bir ilkeyi sınamak için, ortamınızdaki kullanıcılara benzer bir kullanıcı kümesi oluşturun. Test kullanıcıları oluşturmak, gerçek kullanıcıları etkilemeden önce ilkelerinizin beklendiği gibi çalıştığını doğrulamanıza ve uygulamalara ve kaynaklara erişimlerini kesintiye uğratmanıza olanak tanır. 

Bazı kuruluşların bu amaçla test kiracıları vardır. Ancak, bir ilkeğin sonucunu tam olarak sınamak için test kiracısındaki tüm koşulları ve uygulamaları yeniden oluşturmak zor olabilir. 

### <a name="create-a-test-plan"></a>Test planı oluşturma

Test planı beklenen sonuçlar ve gerçek sonuçlar arasında bir karşılaştırma olması önemlidir. Her zaman bir şey test etmeden önce bir beklenti olmalıdır. Aşağıdaki tabloörnek test örneklerini özetleyin. CA ilkelerinizin nasıl yapılandırıldığına göre senaryoları ve beklenen sonuçları ayarlayın.

|İlke |Senaryo |Beklenen Sonuç | Sonuç |
|---|---|---|---|
|[İşte değilken MFA gerektirir](/azure/active-directory/conditional-access/untrusted-networks)|Güvenilir bir konumda / işteyken *Uygulama'ya* yetkili kullanıcı işaretleri|Kullanıcı MFA'ya istenmez| |
|[İşte değilken MFA gerektirir](/azure/active-directory/conditional-access/untrusted-networks)|Güvenilir bir konumda değilken / işte yken Yetkili kullanıcı *Uygulamasına* giriş|Kullanıcı MFA istenir ve başarıyla oturum açabilir| |
|[MFA gerektirir (yönetici için)](/azure/active-directory/conditional-access/howto-baseline-protect-administrators)|Global Admin *App'e* giriş imzaladı|Admin MFA istenir| |
|[Riskli oturum açma işlemleri](/azure/active-directory/identity-protection/howto-sign-in-risk-policy)|[Kullanıcı, Tor tarayıcısını](/azure/active-directory/active-directory-identityprotection-playbook) kullanarak *Uygulamaya* giriş yaptı|Admin MFA istenir| |
|[Cihaz yönetimi](/azure/active-directory/conditional-access/require-managed-devices)|Yetkili kullanıcı yetkili bir cihazdan oturum açmaya çalışır|Erişim Verildi| |
|[Cihaz yönetimi](/azure/active-directory/conditional-access/require-managed-devices)|Yetkili kullanıcı yetkisiz bir cihazdan oturum açmaya çalışır|Erişim engellendi| |
|[Riskli kullanıcılar için parola değişikliği](/azure/active-directory/identity-protection/howto-user-risk-policy)|Yetkili kullanıcı, gizliliği ihlal edilen kimlik bilgileriyle oturum açmaya çalışır (yüksek risk oturum açma)|Kullanıcıdan parolayı değiştirmesi istenir veya erişim politikanıza göre engellenir| |

### <a name="configure-the-policy"></a>İlkeyi yapılandırma

Koşullu Erişim ilkelerini yönetmek el ile bir görevdir. Azure portalında, Koşullu Erişim ilkelerinizi tek bir merkezi konumda yönetebilirsiniz - Koşullu Erişim sayfası. Koşullu Erişim sayfasına giriş noktası, Etkin **Dizin** gezinti bölmesindeki **Güvenlik** bölümüdür. 

![Koşullu Erişim](media/plan-conditional-access/03.png)

Koşullu Erişim ilkeleri nin nasıl oluşturulması hakkında daha fazla bilgi edinmek istiyorsanız, [Azure Active Directory Koşullu Erişimi olan belirli uygulamalar için MFA'yı zorunlu kılmasını gerektir](app-based-mfa.md)mesuliyetine bakın. Bu hızlı başlatma şunları yardımcı olur:

- Kullanıcı arabirimine aşina olun.
- Koşullu Erişim'in nasıl çalıştığına dair ilk izlenimi edin. 

### <a name="evaluate-a-simulated-sign-in"></a>Benzetimli oturum açma yı değerlendirme

Koşullu Erişim ilkenizi yapılandırdığınıza göre, büyük olasılıkla beklendiği gibi çalışıp çalışmadığını bilmek istersiniz. İlk adım olarak, test kullanıcınızın oturum açma simüle etmek için [ilke aracı ne olur](what-if-tool.md) Koşullu Erişim kullanın. Benzetim, bu oturum açma işleminin ilkeleriniz üzerindeki etkisini tahmin eder ve bir benzetim raporu oluşturur.

>[!NOTE]
> Benzetimli bir çalışma, Koşullu Erişim ilkesinin sahip olduğu etki izlenimini verse de, gerçek bir test çalışmasının yerini almaz.

### <a name="test-your-policy"></a>İlkinizi test edin

Test test lerini test planınıza göre çalıştırın. Bu adımda, her iiemin doğru şekilde diğinden emin olmak için test kullanıcılarınız için her ilkenin uçtan uca sınanmasını sağlarsınız. Her testi yürütmek için yukarıda oluşturulan senaryoları kullanın.

Bir ilkenin dışlama ölçütlerini sınadığınızdan emin olmak önemlidir. Örneğin, bir kullanıcıyı veya grubu MFA gerektiren bir ilkeden hariç tutabilirsiniz. Diğer ilkelerin birleşimi bu kullanıcılar için MFA gerektirebileceğinden, dışlanan kullanıcılardan MFA için istenip istenmediği test edin.

### <a name="cleanup"></a>Temizleme

Temizleme yordamı aşağıdaki adımlardan oluşur:

1. İlkeyi devre dışı.
1. Atanan kullanıcıları ve grupları kaldırın.
1. Test kullanıcılarını silin.  

## <a name="move-to-production"></a>Üretime taşıma

Ortamınız için yeni ilkeler hazır olduğunda, bunları aşamalar halinde dağıtın:

- Son kullanıcılara iç değişim iletişimi sağlayın.
- Küçük bir kullanıcı kümesiyle başlayın ve ilkenin beklendiği gibi şekilde şekilde şekilde iyi olduğunu doğrulayın.
- Bir ilkeyi daha fazla kullanıcı içerecek şekilde genişletdiğinizde, tüm yöneticileri hariç tutmamaya devam edin. Yöneticiler hariç, bir değişiklik gerektiğinde birinin bir ilkeye hala erişmesini sağlar.
- Bir ilkeyi yalnızca gerekli olduğunda tüm kullanıcılara uygulayın.

En iyi uygulama olarak, en az bir kullanıcı hesabı oluşturun:

- Politika yönetimine adanmış
- Tüm ilkelerinizden hariç tutuldu

## <a name="rollback-steps"></a>Geri alma adımları

Yeni uygulanan ilkelerinizi geri almanız gerektiğinde, geri almak için aşağıdaki seçeneklerden birini veya birkaçını kullanın:

1. **İlkeyi devre dışı bırakma** - Bir ilkeyi devre dışı bırakmak, kullanıcı oturum açmaya çalıştığında uygulamadığından emin olur. Kullanmak istediğinizde her zaman geri gelebilir ve ilkeyi etkinleştirebilirsiniz.

   ![İlkeyi devre dışı bırakma](media/plan-conditional-access/07.png)

1. **Bir kullanıcıyı / grubu bir ilkeden hariç tut** - Bir kullanıcı uygulamaya erişemiyorsa, kullanıcıyı poliçeden hariç tutabilirsiniz

   ![Exluce kullanıcıları](media/plan-conditional-access/08.png)

   > [!NOTE]
   > Bu seçenek, yalnızca kullanıcının güvenildiği durumlarda dikkatli kullanılmalıdır. Kullanıcı en kısa sürede ilke veya gruba eklenmelidir.

1. **İlkeyi silin** - İlke artık gerekli değilse, silin.

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilir bilgilere genel bir bakış elde etmek için [Azure AD Koşullu Erişim Belgelerine](index.yml) göz atın.
