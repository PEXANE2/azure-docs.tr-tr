---
title: Azure Active Directory Koşullu erişim dağıtımı planlayın
description: Koşullu erişim ilkelerini tasarlamayı ve kuruluşunuzda etkin bir şekilde dağıtmayı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 09/17/2019
ms.author: baselden
author: BarbaraSelden
manager: daveba
ms.reviewer: joflore
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d4ae1c9926c7ea1d18bf5c87fbed837edc2a5d5
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83641449"
---
# <a name="plan--a-conditional-access-deployment"></a>Koşullu erişim dağıtımı planlayın

Koşullu erişim dağıtımınızı planlamak, kuruluşunuzun uygulamalar ve kaynaklar için erişim stratejisini sağlamak açısından kritik öneme sahiptir.

Mobil ve bulut öncelikli bir dünyada, kullanıcılarınız kuruluşunuzun kaynaklarına çeşitli cihaz ve uygulama kullanarak her yerden erişir. Sonuç olarak, kaynağa erişebilecek olan bir kaynağa odaklanma artık yeterli değildir. Ayrıca, kullanıcının nerede olduğunu, kullanılmakta olan cihazı, erişilmekte olan kaynağı ve daha fazlasını da dikkate almanız gerekir. 

Azure Active Directory (Azure AD) koşullu erişim (CA), kararları otomatik hale getirmek ve kaynak için kuruluş erişim ilkelerini zorlamak için Kullanıcı, cihaz ve konum gibi sinyalleri sağlar. CA ilkelerini, Multi-Factor Authentication (MFA) gibi erişim denetimleri uygulamak için kullanabilirsiniz. CA ilkeleri, kullanıcılardan güvenlik için gerektiğinde MFA için kullanıcılara istem yapmanızı ve gerek duyulmadığında kullanıcıların nasıl bir şekilde haberdar olmanızı sağlar.

![Koşullu Erişime genel bakış](./media/plan-conditional-access/conditional-access-overview-how-it-works.png)

Microsoft, temel düzeyde güvenlik sağlayan [güvenlik Varsayılanları](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) adlı standart koşullu ilkeler sağlar. Bununla birlikte, kuruluşunuzun güvenlik varsayılanlarını önerenden daha fazla esneklik ihtiyacı olabilir. Koşullu erişimi, güvenlik varsayılanlarını daha fazla ayrıntı ile özelleştirmek ve gereksinimlerinizi karşılayan yeni ilkeleri yapılandırmak için kullanabilirsiniz.

## <a name="learn"></a>Learn

Başlamadan önce, [Koşullu erişimin](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) nasıl çalıştığını ve ne zaman kullanılacağını anladığınızdan emin olun.

### <a name="benefits"></a>Avantajlar

Koşullu erişim dağıtmanın avantajları şunlardır:

* Üretkenliği artırın. Yalnızca bir veya daha fazla sinyal bunu yaparken MFA gibi bir oturum açma koşuluna sahip kullanıcıları kesintiye uğratır. CA ilkeleri, kullanıcılardan MFA için ne zaman istendiğini, erişim engellendiğinde ve ne zaman güvenilir bir cihaz kullanması gerektiğini denetlemenize olanak tanır.

* Riski yönetin. Risk değerlendirmesi ilke koşulları ile otomatikleştirilmesi, riskli oturum açma işlemlerinin bir kez tanımlanmış ve düzeltilmiş veya engellenmiş olduğu anlamına gelir. [Kimlik koruması](https://docs.microsoft.com/azure/active-directory/identity-protection/overview)Ile koşullu erişimi, anormallikleri ve şüpheli olayları algılayan şekilde, kaynaklara erişim engellendiğinde veya kapıya erişimi önleyecek şekilde hedefetmenize olanak tanır. 

* Adres uyumluluğu ve idare. Koşullu erişim, uygulamalara erişimi sağlamanıza, izin için kullanım koşulları sunmanızı ve uyumluluk ilkelerine göre erişimi kısıtlamanıza olanak sağlar.

* Maliyeti yönetin. Erişim ilkelerini Azure AD 'ye taşımak, koşullu erişim ve altyapı maliyetlerine yönelik özel veya şirket içi çözümlere karşı rahatlımayı azaltır.

### <a name="license-requirements"></a>Lisans gereksinimleri

Bkz. [koşullu erişim lisans gereksinimleri](https://docs.microsoft.com/azure/active-directory/conditional-access/overview).

Ek özellikler gerekliyse, ilgili lisanslara de ihtiyacınız olabilir. Daha fazla bilgi için bkz. [Azure Active Directory fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="prerequisites"></a>Önkoşullar

* Azure AD Premium veya deneme lisansı etkin çalışan bir Azure AD kiracısı. Gerekirse, [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Koşullu Erişim Yöneticisi ayrıcalıklarına sahip bir hesap.

* Yönetici olmayan ve Kullanıcı tarafından bildiğiniz, testuser gibi bir parola. Bir kullanıcı oluşturmanız gerekiyorsa bkz. [hızlı başlangıç: Azure Active Directory yeni kullanıcı ekleme](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

* Yönetici olmayan kullanıcının üyesi olduğu bir grup. Bir grup oluşturmanız gerekiyorsa, bkz. [Grup oluşturma ve Azure Active Directory üye ekleme](https://docs.microsoft.com/azure/active-directory/active-directory-groups-create-azure-portal).

### <a name="training-resources"></a>Eğitim kaynakları

Koşullu erişim hakkında bilgi edinmek için aşağıdaki kaynaklar yararlı olabilir:


**Videolar**
* [Koşullu Erişim nedir?](https://youtu.be/ffMAw2IVO7A)
* [Koşullu erişim nasıl dağıtılır?](https://youtu.be/c_izIRNJNuk)
* [CA ilkelerinin son kullanıcılara nasıl alınacağı?](https://youtu.be/0_Fze7Zpyvc)
* [Cihaz denetimleriyle Koşullu Erişim](https://youtu.be/NcONUf-jeS4)
* [Azure MFA ile Koşullu Erişim](https://youtu.be/Tbc-SU97G-w)
* [Enterprise Mobility + Security koşullu erişim](https://youtu.be/A7IrxAH87wc)
* [Cihaz tabanlı koşullu erişim](https://in.video.search.yahoo.com/search/video;_ylt=AwrPiBX0yHRcZiMAhFa7HAx.;_ylu=X3oDMTB0N2poMXRwBGNvbG8Dc2czBHBvcwMxBHZ0aWQDBHNlYwNwaXZz?p=conditional+access+videos+microsoft&fr2=piv-web&fr=mcafee)

**Pluralali üzerinde çevrimiçi kurslar**
* [Microsoft Azure 'da kimlik yönetimini tasarlama](https://www.pluralsight.com/courses/microsoft-azure-identity-management-design)
* [Microsoft Azure için tasarım kimlik doğrulaması](https://www.pluralsight.com/courses/microsoft-azure-authentication-design)
* [Microsoft Azure için tasarım yetkilendirmesi](https://www.pluralsight.com/courses/microsoft-azure-authorization-design)

**SSS**

[Azure AD koşullu erişim SSS](https://docs.microsoft.com/azure/active-directory/conditional-access/faqs)
## <a name="plan-the-deployment-project"></a>Dağıtım projesini planlayın

Ortamınızda bu dağıtımın stratejisini belirlerken kurumsal ihtiyaçlarınızı göz önünde bulundurun.
### <a name="engage-the-right-stakeholders"></a>Doğru paydaşlara katılın
Teknoloji projeleri başarısız olduğunda, genellikle etki, sonuç ve sorumlulukların eşleşmeyen beklentileri nedeniyle bu, genellikle bu şekilde yapılır. Bu sınırları önlemek için, doğru paydaşları ve proje rollerinin açık [olduğundan emin olun](https://aka.ms/deploymentplans) .

### <a name="plan-communications"></a>İletişimi planlama
İletişim, her yeni hizmetin başarısı için önemlidir. Deneyimlerinizin nasıl değiştirileceği, ne zaman değiştirileceği ve sorunlarla karşılaştıkları durumlarda nasıl destek kazanabilecekleri hakkında daha etkin bir şekilde iletişim kurun.

### <a name="plan-a-pilot"></a>Pilot planı planlayın
Ortamınız için yeni ilkeler hazırlanıyor, bunları üretim ortamında aşamalar halinde dağıtın. Önce bir ilkeyi bir test ortamında küçük bir kullanıcı kümesine uygulayın ve ilkenin beklendiği gibi davrandığını doğrulayın. [Pilot Için en iyi uygulamalar](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-plans)bölümüne bakın.

> [!NOTE]
> Yöneticilere özgü yeni ilkeler çıkarmak için tüm yöneticileri hariç tutun. Bu, yöneticilerin ilkeye erişmeye devam edip değişiklik yapabilmesini veya önemli bir etki olması durumunda değişikliği iptal edebilmesini sağlar. Tüm kullanıcılara uygulamadan önce ilkeyi daha küçük kullanıcı gruplarıyla her zaman doğrulayın.

## <a name="understand-ca-policy-components"></a>CA ilkesi bileşenlerini anlama

CA ilkeleri-then ifadeleridir: atanan bir koşul karşılanırsa, bu erişim denetimlerini uygulayın. 

![Koşullu Erişime genel bakış](media/plan-conditional-access/10.png)

CA ilkelerini yapılandırırken, koşullar *atama*olarak adlandırılır. CA ilkeleri, belirli atamalara göre Kuruluşunuzun uygulamalarına erişim denetimlerini zorlamanıza olanak sağlar.

![Atamalar ve erişim denetimleri ](media/plan-conditional-access/ca-policy-access.png)


Daha fazla bilgi için bkz. [CA Ilkesi oluşturma](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policies).

[Atamalar](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policies) şunları tanımlar

* ilkeden etkilenecek [Kullanıcılar ve gruplar](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-users-groups)

* ilkenin uygulanacağı [bulut uygulamaları veya eylemler](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-cloud-apps) 

* ilkenin geçerli olacağı [koşullar](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions) . 
<p>

![ilke ekranı oluştur](media/plan-conditional-access/create-policy.png)

[Erişim denetimleri](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policies) ayarları bir ilkenin nasıl zorlanmasını belirleme:

* Bulut uygulamalarına erişim [Izni verin veya erişimi engelleyin](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-grant) .

* [Oturum denetimleri](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-session) , belirli bulut uygulamaları içinde sınırlı deneyimleri etkinleştirir.

### <a name="ask-the-right-questions-to-build-your-policies"></a>İlkelerinizi derlemek için doğru soruları sorun

İlkeler, kaynaklarınıza erişmesi gerekenler, hangi kaynakları erişmesi gerektiğini ve hangi koşullarda ilgili soruları yanıtlar. İlkeler, erişim izni vermek veya erişimi engellemek için tasarlanabilir. İlkenizin elde edilmeye çalıştığı hakkında doğru soruları sorduğunuzdan emin olun. 

Derlemeden önce her ilke için soruların yanıtlarını belgeleyin. 

#### <a name="common-questions-about-assignments"></a>Atamalar hakkında sık sorulan sorular

[Kullanıcılar ve gruplar](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-users-groups)

* İlkeye hangi kullanıcılar ve gruplar dahil edilecek veya ilkeden hariç tutulacak?

* Bu ilke tüm kullanıcıları, belirli kullanıcı grubunu, Dizin rollerini veya dış kullanıcıları içerir mi?

[Bulut uygulamaları veya eylemleri](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-cloud-apps)

* İlke hangi uygulamaları için geçerlidir?

* Bu ilkeye hangi kullanıcı eylemleri tabi olacaktır?

[Koşullar](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions)

* İlkeye hangi cihaz platformları dahil edilecek veya ilkeden hariç tutulacak?

* Kuruluşun Güvenilen konumları nelerdir?

* İlkeye hangi konumlar dahil edilecek veya ilkeden hariç tutulacak?

* Hangi istemci uygulama türleri (tarayıcı, mobil, masaüstü istemcileri, eski kimlik doğrulama yöntemlerine sahip uygulamalar), ilkeye dahil edilecek veya ilkeye dışlanacaktır?

* Azure AD 'ye katılmış cihazları veya karma Azure AD 'ye katılmış cihazları ilkelerden hariç bırakabilecek ilkelermisiniz? 

* [Kimlik koruması](https://docs.microsoft.com/azure/active-directory/identity-protection/overview)kullanılıyorsa, oturum açma risk korumasını dahil etmek istiyor musunuz?

#### <a name="common-questions-about-access-controls"></a>Erişim denetimleri hakkında sık sorulan sorular

[Verme veya engelleme](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-grant) 

Aşağıdakilerden birini veya birkaçını isteyerek kaynaklara erişim izni vermek istiyor musunuz?

* MFA gerektirme

* Cihazın uyumlu olarak işaretlenmesini gerektir

* Karma Azure AD 'ye katılmış cihaz gerektir

* Onaylanan istemci uygulaması gerektir

* Uygulama koruma ilkesi gerektir

[Oturum denetimi](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-session)

Bulut uygulamalarında şu erişim denetimlerinden herhangi birini zorlamak istiyor musunuz?

* Uygulama tarafından zorlanan izinleri kullan

* Koşullu Erişim Uygulama Denetimi Kullan

* Oturum açma sıklığını zorla

* Kalıcı tarayıcı oturumlarını kullanma

### <a name="access-token-issuance"></a>Erişim belirteci verme

Erişim belirteçlerinin nasıl verildiğini anlamak önemlidir. 

![Erişim belirteci verme diyagramı](media/plan-conditional-access/CA-policy-token-issuance.png)

**Özellikle herhangi bir atama gerekmiyorsa ve etkin olmayan BIR CA ilkesi yoksa, varsayılan davranışın bir erişim belirteci verildiğine unutmayın**. 

Örneğin, şu durumlarda bir ilke göz önünde bulundurun:

Kullanıcı Grup 1 ' de ise MFA 'yı uygulama 1 ' e zorla.

Grup 1 ' de olmayan bir Kullanıcı uygulamaya erişmeyi denerse "If" koşulu karşılanırsa ve bir belirteç verildiğinde. Grup 1 dışındaki kullanıcıları dışlamak için, diğer tüm kullanıcıları engellemek üzere ayrı bir ilke gerekir.

## <a name="follow-best-practices"></a>En iyi uygulamaları izleyin

Koşullu erişim çerçevesi size harika bir yapılandırma esnekliği sağlar. Ancak harika esneklik, istenmeyen sonuçlara engel olmak için, her yapılandırma ilkesini serbest bırakmadan önce dikkatli bir şekilde incelemeniz gerektiğini gösterir.

### <a name="apply-ca-policies-to-every-app"></a>Her uygulamaya CA ilkeleri uygulama

Bir CA Ilkesi koşulu bir erişim denetimi tetikleyemezse, erişim belirteçleri varsayılan olarak verilir. Her uygulamanın en az bir koşullu erişim ilkesinin uygulanmış olduğundan emin olun

> [!IMPORTANT]
> Tek bir ilkedeki blok ve tüm uygulamaları kullanmaya çok dikkat edin. Bu, yöneticileri Azure Yönetim portalından kilitleyip kilit, Microsoft Graph gibi önemli uç noktaları için yapılandırılamaz.

### <a name="minimize-the-number-of-ca-policies"></a>CA ilkelerinin sayısını en aza indirme

Her uygulama için bir ilke oluşturulması verimli değildir ve zor yönetime yol açar. Koşullu erişim, Kullanıcı başına yalnızca ilk 195 ilkeyi uygular. Uygulamalarınızı analiz etmenizi ve aynı kullanıcılar için aynı kaynak gereksinimlerine sahip uygulamalarda gruplanmasını öneririz. Örneğin, tüm Office 365 uygulamaları veya tüm IK uygulamaları aynı kullanıcılar için aynı gereksinimlere sahip ise, tek bir ilke oluşturun ve geçerli olduğu tüm uygulamaları dahil edin. 

### <a name="set-up-emergency-access-accounts"></a>Acil durum erişim hesapları ayarlama

Bir ilkeyi yanlış yapılandırırsanız, kuruluşları Azure portal dışına kilitleyebilir. Kuruluşunuzda iki veya daha fazla [acil durum erişim hesabı](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access) oluşturarak yanlışlıkla yöneticinin kilitlenmesinden kaynaklanan etkileri azaltabilirsiniz.

* İlke yönetimine adanmış ve tüm ilkelerinizden çıkarılan bir kullanıcı hesabı oluşturun.

* Karma ortamlar için kesme camı senaryosu:

  * Şirket içi bir güvenlik grubu oluşturun ve Azure AD ile eşitleyin. Güvenlik grubu, adanmış ilke yönetimi hesabınızı içermelidir. 

   * Bu güvenlik grubunu tüm CA ilkelerini DıŞARıDA tut.

   * Bir hizmet kesintisi oluştuğunda, diğer yöneticilerinizi şirket içi gruba uygun şekilde ekleyin ve eşitlemeye zorlayın. Bu, CA ilkelerine ait muafiyetini hareketlendirir.

### <a name="set-up-report-only-mode"></a>Yalnızca rapor modunu ayarlama

Şu gibi yaygın dağıtım girişimlerinin etkilediği kullanıcıların sayısını ve adlarını tahmin etmek zor olabilir:

* Eski kimlik doğrulaması engelleniyor
* MFA gerektirme
* oturum açma risk ilkelerini uygulama

[Yalnızca rapor modu](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-report-only) , yöneticilerin, kendi ortamlarında ETKINLEŞTIRILMEDEN önce CA ilkelerinin etkilerini değerlendirmelerini sağlar.

[CA ilkesinde yalnızca rapor modunu yapılandırmayı](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-report-only)öğrenin.

### <a name="plan-for-disruption"></a>Kesintiyi planlayın

BT sistemlerinizi güvenli hale getirmek için MFA veya ağ konumu gibi tek bir erişim denetimine sahipseniz, tek erişim denetimi kullanılamaz hale gelirse veya yanlış yapılandırıldıkça hatalara erişebilirsiniz. Öngörülemeyen kesintiler sırasında kilitleme riskini azaltmak için, kuruluşunuz için benimseme [stratejileri planlayın](https://docs.microsoft.com/azure/active-directory/authentication/concept-resilient-controls) .

### <a name="set-naming-standards-for-your-policies"></a>İlkeleriniz için adlandırma standartları ayarlama

Adlandırma standardı, ilkeleri bulmanıza ve Azure yönetim portalında açmadan amaçlarını anlamanıza yardımcı olur. İlkenize şunları gösterecek şekilde ad etmenizi öneririz:

* Sıra numarası

* İçin geçerli olduğu bulut uygulamaları

* Yanıt

* Kim için geçerli

* Ne zaman geçerliyse (varsa)

![Adlandırma standardı](media/plan-conditional-access/11.png)

**Örnek**; Dış ağlardan Dynamics CRP uygulamasına erişen pazarlama kullanıcıları için MFA gerektiren bir ilke şu olabilir:

![Adlandırma standardı](media/plan-conditional-access/naming-example.png)

Açıklayıcı bir ad, koşullu erişim uygulamanıza genel bakış sağlamanıza yardımcı olur. Bir konuşmada ilkeye başvurmanız gerekirse sıra numarası yararlı olur. Örneğin, telefonda bir yöneticiye konuştuğunuzdaki bir sorunu gidermek için bu kullanıcılardan ilke CA01 açmasını isteyebilirsiniz.

#### <a name="naming-standards-for-emergency-access-controls"></a>Acil durum erişim denetimleri için adlandırma standartları

Etkin ilkelerinize ek olarak, [kesinti veya acil durum senaryolarında ikincil dayanıklı erişim denetimleri](https://docs.microsoft.com/azure/active-directory/authentication/concept-resilient-controls)olarak davranan devre dışı ilkeler uygulayın. Acil durum ilkelerine yönelik adlandırma standartlarınız şunları içermelidir:
* Adın diğer ilkeler arasında öne çıkmasını sağlamak için başlangıçta acıl durum ' da ETKINLEŞTIRIN.

* İçin uygulanması gereken kesilme adı.

* Yöneticinin hangi sıra ilkelerinin etkin olması gerektiğini bilmesini sağlamak için bir sıralama sıra numarası.

**Örnek**

Aşağıdaki ad, bu ilkenin bir MFA kesintisi olduğunda etkinleştirilecek dört ilkeden ilki olduğunu gösterir:

EM01-acıl durumda ETKINLEŞTIR: MFA kesintisi [1/4]-Exchange SharePoint: VIP kullanıcıları Için karma Azure AD katılımı gerektir.

### <a name="exclude-countries-from-which-you-never-expect-a-sign-in"></a>Hiç oturum açma işleminin beklemediği ülkeleri hariç tutun.

Azure Active Directory, [adlandırılmış konumlar](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition)oluşturmanıza olanak sağlar. Bir oturum açma işleminin gerçekleşmesini beklemediği tüm ülkeleri içeren adlandırılmış bir konum oluşturun. Ardından, bu adlandırılmış konumdan oturum açmayı engelleyen tüm uygulamalar için bir ilke oluşturun. **Yöneticilerinizi bu ilkeden muaf tutamadığınızdan emin**olun.

### <a name="plan-your-policy-deployment"></a>İlke dağıtımınızı planlayın

Ortamınız için yeni ilkeler hazırlanıyor, istenmeyen sonuçlara engel olmak için her bir ilkeyi serbest bırakmadan önce gözden geçirdiğinizden emin olun. İlkelerin uygulanma ve sorunlardan kaçının hakkında önemli bilgileri anlamak için aşağıdaki belgelere bakın

* [Bilmeniz gerekenler](https://docs.microsoft.com/azure/active-directory/conditional-access/best-practices)

* [Yapmanız gerekenler](https://docs.microsoft.com/azure/active-directory/conditional-access/best-practices)

## <a name="common-policies"></a>Ortak ilkeler

CA ilkesi çözümünüzü planlarken, aşağıdaki sonuçları elde etmek için ilke oluşturmanız gerekip gerekmediğini değerlendirin.

### <a name="require-mfa"></a>MFA gerektirme

MFA erişimi gerektiren yaygın kullanım örnekleri:

* [Yöneticiler tarafından](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-admin-mfa)

* [Belirli uygulamalara](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-mfa)

* [Tüm kullanıcılar için](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-all-users-mfa)

* [Ağ konumları ' ndan güvenmediğiniz](https://docs.microsoft.com/azure/active-directory/conditional-access/untrusted-networks)

* [Azure yönetimi için](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-azure-management)

### <a name="respond-to-potentially-compromised-accounts"></a>Tehlikede riskli hesaplara yanıt verme

CA ilkeleriyle, riskli olabilecek kimlikler tarafından gerçekleştirilen oturum açma işlemleri için otomatik yanıtlar uygulayabilirsiniz. Bir hesabın tehlikeye düşmesi olasılığı, risk düzeyleri biçiminde ifade edilir. Kimlik koruması ile hesaplanan iki risk düzeyi vardır: oturum açma riski ve Kullanıcı riski. Aşağıdaki üç varsayılan ilke etkinleştirilebilir.

* [Tüm kullanıcıların MFA için kaydolmesini gerektir](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-risk)

* [Yüksek riskli kullanıcılar için parola değişikliği gerektir](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-risk)

* [Orta veya yüksek oturum açma riski olan kullanıcılar için MFA gerektir](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-risk)

### <a name="require-managed-devices"></a>Yönetilen cihazları gerektirme

Bulut kaynaklarınıza erişmek için desteklenen cihazların uzalama, kullanıcılarınızın verimliliğini artırmaya yardımcı olur. Büyük olasılıkla ortamınızdaki belirli kaynaklara bilinmeyen koruma düzeyi olan cihazların erişmesini istemezsiniz. Bu kaynaklar için [kullanıcıların yalnızca yönetilen bir cihaz kullanarak erişebilmesi gerekir](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices).

### <a name="require-approved-client-apps"></a>Onaylı istemci uygulamaları gerektirme

Çalışanlar hem kişisel hem de iş görevleri için mobil cihazlarını kullanır. KCG senaryolarında tüm cihazı mı yoksa yalnızca üzerindeki verileri mi yönetmeniz gerektiğine karar vermelisiniz. yalnızca verileri ve erişimi yönetiyorsanız, Şirket verilerinizi koruyabilen [onaylanan bulut uygulamaları isteyebilirsiniz](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) . Örneğin, e-postaya yalnızca Outlook Mobile aracılığıyla erişilmek üzere, genel bir posta programı aracılığıyla erişebilirsiniz.

### <a name="block-access"></a>Erişimi engelleme

[Tüm erişimi engelleme](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-block-access) seçeneği güçlü. Örneğin, bir uygulamayı Azure AD 'ye geçirdiğinizde, ancak henüz hiç oturum açmasını sağlamak için de kullanılabilir. Erişimi engelle: 

* Bir kullanıcı için diğer tüm atamaları geçersiz kılar

* , Tüm kuruluşunuzun kiracınızda oturum açmasını engelleme gücüne sahiptir

> [!IMPORTANT]
> Tüm kullanıcılar için erişimi engellemek üzere bir ilke oluşturursanız, acil durum erişim hesaplarını dışlamadığınızdan emin olun ve tüm yöneticileri ilkeden hariç tutun.

Kullanıcılarınıza erişimi engelleyebileceğiniz diğer yaygın senaryolar şunlardır:

* [Belirli ağ konumlarının](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-location) bulut uygulamalarınıza erişmesini engelleyin. Bu ilkeyi, trafiğin gelmemesi gerektiğini bildiğiniz bazı ülkeleri engellemek için kullanabilirsiniz.

* Azure AD eski kimlik doğrulamasını destekler. Ancak, eski kimlik doğrulaması MFA 'yı desteklemez ve birçok ortam, kimlik güvenliğini sağlamak için gereklidir. Bu durumda, [eski kimlik doğrulama kullanan uygulamaları](https://docs.microsoft.com/azure/active-directory/conditional-access/block-legacy-authentication) kiracı kaynaklarınıza erişmesini engelleyebilirsiniz.

## <a name="build-and-test-policies"></a>Derleme ve test ilkeleri

Dağıtımınızın her aşamasında sonuçların beklendiği gibi olduğunu değerlendirdiğinizden emin olun. 

Yeni ilkeler hazırlandığınızda, bunları üretim ortamında aşamalar halinde dağıtın:

* Son kullanıcılara iç değişiklik iletişimi sağlayın.

* Küçük bir Kullanıcı kümesiyle başlayın ve ilkenin beklendiği gibi davrandığını doğrulayın.

* Bir ilkeyi daha fazla Kullanıcı içerecek şekilde genişlettiğinizde, tüm yöneticileri dışlayacak şekilde devam edin. Yönetici dışlama, bir değişiklik gerekliyse birisinin bir ilkeye hala erişmesini sağlar.

* Yalnızca tüm kullanıcılara, tamamen test edildikten sonra bir ilke uygulayın. Bir ilkenin uygulanacağı en az bir yönetici hesabınız olduğundan emin olun.

### <a name="create-test-users"></a>Test kullanıcıları oluşturma

Üretim ortamınızdaki kullanıcıları yansıtan bir test kullanıcıları kümesi oluşturun. Test kullanıcıları oluşturmak, gerçek kullanıcıları etkilemeden ve uygulama ve kaynaklara erişimleri büyük olasılıkla kesintiye uğramadan önce ilkeleri beklendiği gibi çalışır şekilde doğrulamanızı sağlar.

Bazı kuruluşların bu amaçla test kiracılar vardır. Ancak, bir ilkenin sonucunu tam olarak test etmek için bir test kiracısındaki tüm koşulların ve uygulamaların yeniden oluşturulması zor olabilir.

### <a name="create-a-test-plan"></a>Test planı oluşturma

Test planı, beklenen sonuçlarla gerçek sonuçlar arasında bir karşılaştırma yapmak için önemlidir. Bir şeyi test etmeden önce her zaman bir beklenmelidir. Aşağıdaki tabloda örnek test çalışmaları özetlenmektedir. CA ilkelerinizin nasıl yapılandırıldığına bağlı olarak senaryoları ve beklenen sonuçları ayarlayın.

| İlke| Senaryo| Beklenen Sonuç |
| - | - | - |
| [İş olmadığında MFA gerektir](https://docs.microsoft.com/azure/active-directory/conditional-access/untrusted-networks)| Güvenilen bir konum/iş sırasında yetkili kullanıcı uygulama oturumunu kapatır| Kullanıcıdan MFA istenmez |
| [İş olmadığında MFA gerektir](https://docs.microsoft.com/azure/active-directory/conditional-access/untrusted-networks)| Yetkili Kullanıcı, güvenilir bir konum/iş üzerinde olmadığı sürece uygulamada oturum açar| Kullanıcıdan MFA yapması istenir ve başarıyla oturum açabilir |
| [MFA gerektir (yönetici için)](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-baseline-protect-administrators)| Genel yönetici uygulamada oturum açar| Yöneticiye MFA sorulur |
| [Riskli oturum açma işlemleri](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-sign-in-risk-policy)| Kullanıcı bir [Tor tarayıcısı](https://microsoft.sharepoint.com/azure/active-directory/active-directory-identityprotection-playbook) kullanarak uygulamada oturum açar| Yöneticiye MFA sorulur |
| [Cihaz yönetimi](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)| Yetkili Kullanıcı yetkili bir cihazdan oturum açmaya çalışır| Erişim Izni verildi |
| [Cihaz yönetimi](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)| Yetkili kullanıcı yetkisiz bir cihazdan oturum açmaya çalışır| Erişim engellendi |
| [Riskli kullanıcılar için parola değiştirme](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-user-risk-policy)| Yetkili Kullanıcı güvenliği tehlikeye giren kimlik bilgileriyle oturum açmaya çalışır (yüksek riskli oturum açma)| İlkenize göre kullanıcıdan parolayı değiştirmesi veya erişimin engellenmesi istenir |


 

### <a name="configure-the-test-policy"></a>Test ilkesini yapılandırma

[Azure Portal](https://portal.azure.com/), Azure Active Directory > güvenlik > koşullu ERIŞIM altında CA ilkelerini yapılandırırsınız.

CA ilkeleri oluşturma hakkında daha fazla bilgi edinmek istiyorsanız, bkz. Bu örnek: [kullanıcı Azure Portal oturum AÇTıĞıNDA MFA istemek Için CA ilkesi](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-enable-azure-mfa?toc=/azure/active-directory/conditional-access/toc.json&bc=/azure/active-directory/conditional-access/breadcrumb/toc.json). Bu hızlı başlangıç şunları yapmanıza yardımcı olur:

* Kullanıcı arabirimi hakkında bilgi sahibi olun

* Koşullu erişimin nasıl çalıştığına ilişkin bir ilk izlenim alın

### <a name="enable-the-policy-in-report-only-mode"></a>İlkeyi yalnızca rapor modunda etkinleştir

İlkenizin etkisini değerlendirmek için ilkeyi [yalnızca rapor modunda](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-report-only)etkinleştirerek başlayın. Yalnızca rapor ilkeleri oturum açma sırasında değerlendirilir, ancak atama denetimleri ve oturum denetimleri zorlanmaz. İlkeyi yalnızca rapor modunda kaydettiğinizde, oturum açma günlüklerinde gerçek zamanlı oturum açma işlemlerinin etkisini görebilirsiniz. Oturum açma günlüklerinden bir olay seçin ve yalnızca rapor olan her ilkenin sonucunu görmek için yalnızca rapor sekmesine gidin.


![yalnızca rapor modu ](media/plan-conditional-access/report-only-mode.png)

İlkeyi seçtiğinizde, ilke ayrıntıları ekranı kullanılarak ilkenin atamalarının ve erişim denetimlerinin nasıl değerlendirildiğini de görebilirsiniz. Bir ilkenin bir oturum açma için uygulanabilmesi için, yapılandırılmış atamaların her birinin karşılanması gerekir. 

### <a name="understand-the-impact-of-your-policies-using-the-insights-and-reporting-workbook"></a>Öngörüler ve raporlama çalışma kitabını kullanarak ilkelerinizin etkisini anlayın

Koşullu erişim ilkelerinizin Birleşik etkisini Öngörüler ve raporlama çalışma kitabında görüntüleyebilirsiniz. Çalışma kitabına erişmek için bir Azure Izleyici aboneliğine ihtiyacınız vardır ve [oturum açma günlüklerinizi bir Log Analytics çalışma alanına akışı](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)gerekir. 

### <a name="simulate-sign-ins-using-the-what-if-tool"></a>Durum aracını kullanarak oturum açma işlemlerinin benzetimini yapın

Koşullu erişim ilkenizi doğrulamak için başka bir yöntem de, kuramsal şartlar altında oturum açan bir kullanıcı için hangi ilkelerin uygulanacağını taklit eden, [ne tür aracı](https://docs.microsoft.com/azure/active-directory/conditional-access/troubleshoot-conditional-access-what-if)kullanmaktır. Sınamak istediğiniz oturum açma özniteliklerini (Kullanıcı, uygulama, cihaz platformu ve konum gibi) seçin ve hangi ilkelerin uygulanacağını görüntüleyin.

> [!NOTE] 
> Sanal bir çalıştırma, CA ilkesinin sahip olduğu etkiyi size iyi bir fikir sunarken, gerçek bir test çalıştırmasının yerini almaz.

### <a name="test-your-policy"></a>İlkenizi test etme

Test planınızdan test kullanıcıları ile her bir testi gerçekleştirin.

**Bir ilkenin dışlama ölçütünü test edin**. Örneğin, MFA gerektiren bir ilkeden Kullanıcı veya grup dışlayabilirsiniz. Diğer ilkelerin birleşimi bu kullanıcılar için MFA gerektirebileceğinden, dışlanan kullanıcılardan MFA için istenirse test edin.

### <a name="roll-back-policies"></a>Geri alma ilkeleri

Yeni uygulanan ilkelerinizi geri almanız gerekiyorsa, aşağıdaki seçeneklerden birini veya daha fazlasını kullanın:

* **İlkeyi devre dışı bırakın.** Bir ilkeyi devre dışı bırakmak, bir Kullanıcı oturum açmaya çalıştığında uygulanmadığından emin olur. Her zaman geri dönüp ilkeyi kullanmak istediğinizde etkinleştirebilirsiniz.

![ilke görüntüsünü etkinleştir](media/plan-conditional-access/enable-policy.png)

* **Bir kullanıcıyı veya grubu ilkeden hariç tutun.** Bir Kullanıcı uygulamaya erişemedik, kullanıcıyı ilkeden dışlamayı seçebilirsiniz.

![kullanıcıları ve grupları Dışla](media/plan-conditional-access/exclude-users-groups.png)

> [!NOTE]
>  Bu seçenek, yalnızca kullanıcının güvendiği durumlarda gelişigüzel bir şekilde kullanılmalıdır. Kullanıcı, ilke veya gruba mümkün olan en kısa sürede yeniden eklenmelidir.

* **İlkeyi silin.** İlke artık gerekmiyorsa, [silin](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-enable-azure-mfa?toc=/azure/active-directory/conditional-access/toc.json&bc=/azure/active-directory/conditional-access/breadcrumb/toc.json) .

## <a name="manage-access-to-cloud-apps"></a>Bulut uygulamalarına erişimi yönetme

CA ilkelerinizi denetlemek ve yönetmek için aşağıdaki yönetme seçeneklerini kullanın:

![Manage-Access](media/plan-conditional-access/manage-access.png)


### <a name="named-locations"></a>Adlandırılmış konumlar

CA ilkesinin konum koşulu, erişim denetimleri ayarlarını kullanıcılarınızın ağ konumlarına bağlamamanızı sağlar. [Adlandırılmış konumlarla](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition), IP adresi aralıkları veya ülkeler ve bölgeler için mantıksal gruplandırmaları oluşturabilirsiniz.

### <a name="custom-controls"></a>Özel denetimler

[Özel denetimler](https://docs.microsoft.com/azure/active-directory/conditional-access/controls) , Azure AD dışında kimlik doğrulama gereksinimlerini karşılamak üzere kullanıcılarınızı uyumlu bir hizmete yönlendirir. Bu denetimi karşılamak için, bir kullanıcının tarayıcısı dış hizmete yönlendirilir, gerekli kimlik doğrulamasını gerçekleştirir ve ardından Azure AD 'ye yeniden yönlendirilir. Azure AD yanıtı doğrular ve kullanıcının kimliği başarıyla doğrulandıktan veya doğrulandıysa, Kullanıcı koşullu erişim akışında devam eder.

### <a name="terms-of-use"></a>Kullanım koşulları

Ortamınızdaki belirli bulut uygulamalarına erişmeden önce, Kullanım koşulları (ToU) kabul ederek kullanıcılardan izin alabilirsiniz. [Kullanım koşulları oluşturmak için bu hızlı](https://docs.microsoft.com/azure/active-directory/conditional-access/require-tou)başlangıcı izleyin.

### <a name="classic-policies"></a>Klasik ilkeler

[Azure Portal](https://portal.azure.com/)CA Ilkelerinizi, koşullu erişim > güvenlik > Azure Active Directory altında bulabilirsiniz. Kuruluşunuz Ayrıca bu sayfa kullanılarak oluşturulmuş eski CA ilkelerine sahip olabilir. Bu ilkeler klasik ilkeler olarak bilinir. [Bu klasik ilkeleri Azure Portal geçirmeyi düşünmeniz](https://docs.microsoft.com/azure/active-directory/conditional-access/best-practices)önerilir.

## <a name="troubleshoot-conditional-access"></a>Koşullu erişim sorunlarını giderme

Bir Kullanıcı CA ilkesiyle ilgili bir sorun yaşadığınızda, sorun gidermeyi kolaylaştırmak için aşağıdaki bilgileri toplayın.

* Kullanıcı asıl adı

* Kullanıcı görünen adı

* İşletim sistemi adı

* Zaman damgası (yaklaşık Tamam)

* Hedef uygulama

* İstemci uygulama türü (tarayıcı vs istemcisi)

* Bağıntı KIMLIĞI (Bu oturum açma işlemi için benzersizdir)

Kullanıcı daha fazla ayrıntı bağlantısına sahip bir ileti aldıysa, bu bilgilerin çoğunu sizin için toplayabilirler.

![Uygulama hata iletisine alınamıyor](media/plan-conditional-access/cant-get-to-app.png)

Bilgileri topladıktan sonra, aşağıdaki kaynaklara bakın:

* [Koşullu erişimle oturum açma sorunları](https://docs.microsoft.com/azure/active-directory/conditional-access/troubleshoot-conditional-access) – hata Iletilerini ve Azure AD oturum açma günlüğünü kullanarak koşullu erişimle ilgili beklenmedik oturum açma sonuçlarını anlayın.

* [Ne tür aracının kullanılması](https://docs.microsoft.com/azure/active-directory/conditional-access/troubleshoot-conditional-access-what-if) -bir ilkenin belirli bir durumdaki bir kullanıcıya veya bir ilkenin bilinen bir durumda nasıl uygulanacağını anlayın.

## <a name="next-steps"></a>Sonraki Adımlar

[Multi-Factor Authentication hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks)

[Kimlik koruması hakkında daha fazla bilgi](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection)

[CA ilkelerini Microsoft Graph API ile yönetme](https://docs.microsoft.com/graph/api/resources/conditionalaccesspolicy?view=graph-rest-beta)
