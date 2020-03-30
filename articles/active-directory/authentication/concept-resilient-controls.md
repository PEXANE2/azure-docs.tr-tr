---
title: Esnek bir erişim denetimi yönetimi stratejisi oluşturun - Azure AD
description: Bu belge, bir kuruluşun öngörülemeyen aksaklıklar sırasında lokavt riskini azaltmak için esneklik sağlamak için benimsemesi gereken stratejiler hakkında rehberlik sağlar
services: active-directory
author: martincoetzer
manager: daveba
tags: azuread
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.workload: identity
ms.date: 01/29/2020
ms.author: martinco
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0ca5817e744ff81efcd549bc328d7ce5eeedb2d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76908743"
---
# <a name="create-a-resilient-access-control-management-strategy-with-azure-active-directory"></a>Azure Active Directory ile esnek bir erişim denetimi yönetimi stratejisi oluşturun

>[!NOTE]
> The information contained in this document represents the current view of Microsoft Corporation on the issues discussed as of the date of publication. Microsoft değişen pazar koşullarına yanıt vermesi gerektiğinden, bu durum Microsoft adına bir taahhüt olarak yorumlanmamalıdır ve Microsoft yayımtarihinden sonra sunulan bilgilerin doğruluğunu garanti edemez.

BT sistemlerini güvenli hale getirmek için çok faktörlü kimlik doğrulama (MFA) veya tek bir ağ konumu gibi tek bir erişim denetimine güvenen kuruluşlar, tek bir erişim denetimi kullanılamadığında uygulamalarında ve kaynaklarındaki hatalara erişebilir veya yanlış yapılandırılmıştır. Örneğin, doğal bir felaket telekomünikasyon altyapısının veya şirket ağlarının büyük segmentlerinin kullanılamamasına neden olabilir. Böyle bir aksaklık son kullanıcıların ve yöneticilerin oturum açabilmelerini engelleyebilir.

Bu belge, bir kuruluşun aşağıdaki senaryolarda öngörülemeyen aksaklıklar sırasında lokavt riskini azaltmak için esneklik sağlamak için benimsemesi gereken stratejiler hakkında rehberlik sağlar:

 1. Kuruluşlar, azaltma stratejileri veya acil durum planları uygulayarak, **bir aksaklık önce** lokavt riskini azaltmak için esnekliklerini artırabilir.
 2. Kuruluşlar, azaltma stratejileri ve acil durum planları yaparak **bir aksaklık sırasında** seçtikleri uygulamalara ve kaynaklara erişmeye devam edebilirler.
 3. Kuruluşlar, **bir aksaklık** tan sonra ve uyguladıkları olası olasılıkları geri almadan önce günlükler gibi bilgileri koruduklarından emin olmalıdır.
 4. Önleme stratejileri veya alternatif planlar uygulamamış kuruluşlar, aksaklıklarla başa çıkmak için **acil durum seçeneklerini** uygulayabilir.

## <a name="key-guidance"></a>Anahtar kılavuzu

Bu belgede dört önemli paket vardır:

* Acil erişim hesaplarını kullanarak yönetici kilitlemekaçının.
* MFA'yı kullanıcı başına MFA yerine Koşullu Erişim (CA) kullanarak uygulayın.
* Birden çok Koşullu Erişim (CA) denetimi kullanarak kullanıcı kilitlemeyi azaltı.
* Her kullanıcı için birden çok kimlik doğrulama yöntemi veya eşdeğeri sağlayarak kullanıcı kilitlemeyi azaltı.

## <a name="before-a-disruption"></a>Bir bozulmadan önce

Gerçek bir kesintiyi hafifletmek, bir kuruluşun ortaya çıkabilecek erişim denetimi sorunlarıyla başa çıkmadaki birincil odak noktası olmalıdır. Azaltma, gerçek bir olay için planlama nın yanı sıra erişim denetimlerinin ve işlemlerinin kesintiler sırasında etkilenmediğinden emin olmak için stratejiler uygulamayı içerir.

### <a name="why-do-you-need-resilient-access-control"></a>Neden esnek erişim kontrolüne ihtiyacınız var?

 Kimlik, uygulamalara ve kaynaklara erişen kullanıcıların kontrol düzlemidir. Kimlik sisteminiz, erişim denetimleri veya kimlik doğrulama gereksinimleri gibi hangi kullanıcıların hangi koşullarda ve hangi koşullar altında uygulamalara erişebildiğinizi denetler. Öngörülemeyen koşullar nedeniyle kullanıcıların kimlik doğrulaması için bir veya daha fazla kimlik doğrulama veya erişim denetimi gereksinimi kullanılamıyorsa, kuruluşlar aşağıdaki sorunlardan birini veya her ikisini de yaşayabilir:

* **Yönetici kilitleme:** Yöneticiler kiracıyı veya hizmetleri yönetemez.
* **Kullanıcı kilitleme:** Kullanıcılar uygulamalara veya kaynaklara erişemez.

### <a name="administrator-lockout-contingency"></a>Yönetici kilitleme acil durum

Yöneticinin kiracınıza erişiminin kilidini açmak için acil erişim hesapları oluşturmanız gerekir. *Kesme camı* hesapları olarak da bilinen bu acil erişim hesapları, normal ayrıcalıklı hesap erişim yordamları kullanılamıyorsa Azure AD yapılandırmasını yönetmesine olanak tanır. Acil erişim [hesabı önerilerinden]( https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access)sonra en az iki acil erişim hesabı oluşturulmalıdır.

### <a name="mitigating-user-lockout"></a>Kullanıcı kilitlemeyi azaltma

 Kullanıcı kilitleme riskini azaltmak için, kullanıcılara uygulamalara ve kaynaklara nasıl erişeceklerine bir seçenek sunmak için birden çok denetime sahip Koşullu Erişim ilkelerini kullanın. Bir kullanıcıya, örneğin, MFA ile oturum açma **veya** yönetilen bir aygıttan oturum açma **veya** şirket ağında oturum açma arasında seçim yaparak, erişim denetimlerinden biri kullanılamıyorsa, kullanıcının çalışmaya devam etmesi için başka seçenekleri vardır.

#### <a name="microsoft-recommendations"></a>Microsoft önerileri

Kuruluş için varolan Koşullu Erişim ilkelerinizde aşağıdaki erişim denetimlerini birleştirin:

1. Microsoft Authenticator uygulaması (internet tabanlı), OATH jetonu (cihazda oluşturulan) ve SMS (telefon) gibi farklı iletişim kanallarına dayanan her kullanıcı için birden fazla kimlik doğrulama yöntemi sağlama.
2. MFA gereksinimlerini doğrudan aygıt oturum açma dan karşılamak için Windows Hello for Business'ı Windows 10 aygıtlarına dağıtın.
3. Azure [AD Karma Birleştirme](https://docs.microsoft.com/azure/active-directory/devices/overview) veya [Microsoft Intune Yönetilen aygıtlar](https://docs.microsoft.com/intune/planning-guide)aracılığıyla güvenilir aygıtları kullanın. Güvenilen aygıtlar kullanıcı deneyimini geliştirir, çünkü güvenilen aygıtın kendisi kullanıcıya mfa meydan okuması olmadan ilkenin güçlü kimlik doğrulama gereksinimlerini karşılayabilir. Daha sonra yeni bir cihaz kaydederken ve uygulamalara veya kaynaklara güvenilmeyen aygıtlardan erişirken MFA gerekir.
4. Kullanıcı veya oturum açma, sabit MFA ilkeleri yerine risk altında olduğunda erişimi engelleyen Azure AD kimlik koruma risk tabanlı ilkeleri kullanın.

>[!NOTE]
> Risk tabanlı ilkeler, [Azure AD Premium P2](https://azure.microsoft.com/pricing/details/active-directory/) lisansları gerektirir.

Aşağıdaki örnekte, kullanıcının uygulamalarına ve kaynaklarına erişmesi için esnek bir erişim denetimi sağlamak için oluşturmanız gereken ilkeler açıklanmaktadır. Bu örnekte, erişmek istediğiniz hedef kullanıcılara sahip bir güvenlik grubu **AppUsers,** çekirdek yöneticilere sahip **CoreAdmins** adlı bir grup ve acil erişim hesapları olan **EmergencyAccess** adlı bir grup gerekir.
Bu örnek ilke seti, **AppUsers'teki**seçili kullanıcılara, güvenilir bir aygıttan bağlanan veya mfa gibi güçlü bir kimlik doğrulaması sağlayan belirli uygulamalara erişim hakkı verir. Acil durum hesaplarını ve çekirdek yöneticileri hariç tutar.

**CA azaltma ilkeleri kümesi:**

* İlke 1: Hedef grupların dışındaki kişilere erişimi engelleme
  * Kullanıcılar ve Gruplar: Tüm kullanıcıları dahil edin. AppUsers, CoreAdmins ve EmergencyAccess hariç
  * Bulut Uygulamaları: Tüm uygulamaları dahil edin
  * Koşullar: (Yok)
  * Hibe Kontrolü: Blok
* İlke 2: MFA VEYA güvenilir cihaz gerektiren AppUsers'e erişim izni tanıyın.
  * Kullanıcılar ve Gruplar: AppUsers dahil. CoreAdmins ve EmergencyAccess hariç
  * Bulut Uygulamaları: Tüm uygulamaları dahil edin
  * Koşullar: (Yok)
  * Hibe Kontrolü: Erişim izni, çok faktörlü kimlik doğrulaması gerektirir, aygıtın uyumlu olmasını gerektirir. Birden çok denetim için: Seçili denetimlerden birini gerektirir.

### <a name="contingencies-for-user-lockout"></a>Kullanıcı kilitleme için olası durumlar

Alternatif olarak, kuruluşunuz acil durum ilkeleri de oluşturabilir. Acil durum ilkeleri oluşturmak için, iş sürekliliği, operasyonel maliyet, finansal maliyet ve güvenlik riskleri arasındaki denge ölçütlerini tanımlamanız gerekir. Örneğin, bir acil durum ilkesini yalnızca bir kullanıcı alt kümesine, bir uygulama alt kümesiiçin, istemci alt kümesi için veya konumların bir alt kümesinden etkinleştirebilirsiniz. Acil durum ilkeleri, herhangi bir azaltma yöntemi uygulanmadığında kesintiye uğrama sırasında yöneticilere ve son kullanıcılara uygulamalara ve kaynaklara erişim sağlar.
Bir aksaklık sırasında maruz kalmanızı anlamak riskinizi azaltmaya yardımcı olur ve planlama sürecinizin önemli bir parçasıdır. Acil durum planınızı oluşturmak için öncelikle kuruluşunuzun aşağıdaki iş gereksinimlerini belirleyin:

1. Görev açısından kritik uygulamalarınızı önceden belirleyin: Daha düşük bir risk/güvenlik duruşuyla bile erişim vermeniz gereken uygulamalar nelerdir? Bu uygulamaların bir listesini oluşturun ve diğer hissedarlarınızın (iş, güvenlik, yasal, liderlik) tüm erişim denetimi nin ortadan kalkarsa, bu uygulamaların çalışmaya devam etmesi gerektiği konusunda hemfikir olmasını sağla. Büyük olasılıkla kategorileri ile sona erecek:
   * **Kategori 1,** örneğin kuruluşun gelirini doğrudan etkileyen uygulamalar gibi birkaç dakikadan fazla kullanılamayan kritik görev uygulamalarıdır.
   * **Kategori 2,** işletmenin birkaç saat içinde erişilebilir olması gereken önemli uygulamalardır.
   * Birkaç günlük bir kesintiye dayanabilecek **kategori 3 düşük öncelikli uygulamalar.**
2. Microsoft, kategori 1 ve 2'deki uygulamalar için, hangi erişim düzeyine izin vermek istediğinizi önceden planlamanızı önerir:
   * İndirmeleri sınırlama gibi tam erişime veya oturum kısıtlamasına izin vermek istiyor musunuz?
   * Uygulamanın bir kısmına erişmesine izin vermek istiyor ancak uygulamanın tamamına erişmemek istemiyor musunuz?
   * Erişim denetimi geri yüklenene kadar bilgi çalışanı erişimine izin vermek ve yönetici erişimini engellemek istiyor musunuz?
3. Bu uygulamalar için Microsoft, hangi erişim yollarını kasıtlı olarak açacağınızı ve hangilerini kapatacağınızı planlamanızı önerir:
   * Tarayıcıya yalnızca erişim eve izin vermek ve çevrimdışı verileri kaydedebilen zengin istemcileri engellemek istiyor musunuz?
   * Yalnızca şirket ağındaki kullanıcılar için erişime izin vermek ve dışarıdaki kullanıcıların engellenmesini sağlamak mı istiyorsunuz?
   * Yalnızca kesinti sırasında belirli ülkelerden veya bölgelerden erişime izin vermek istiyor musunuz?
   * Alternatif bir erişim denetimi yoksa, özellikle kritik görev uygulamaları için acil durum politikalarına yönelik politikaların başarısız olmasını veya başarılı olmasını istiyor musunuz?

#### <a name="microsoft-recommendations"></a>Microsoft önerileri

Acil Durum Koşullu Erişim ilkesi, Azure MFA, üçüncü taraf MFA, risk tabanlı veya aygıt tabanlı denetimleri atlayan devre dışı bırakılmış bir **ilkedir.** Ardından, kuruluşunuz acil durum planınızı etkinleştirmeye karar verdiğinde, yöneticiler ilkeyi etkinleştirebilir ve düzenli denetim tabanlı ilkeleri devre dışı bırakabilir.

>[!IMPORTANT]
> Kullanıcılarınızın güvenliğini geçici de olsa zorlayan ilkeleri devre dışı bırakmak, acil durum planı yerindeyken güvenlik duruşunuzu azaltır.

* Bir kimlik bilgisi türündeki veya tek bir erişim denetim mekanizmasındaki bir aksaklık uygulamalarınıza erişimi etkiliyorsa, bir dizi geri dönüş ilkesini yapılandırın. Bir üçüncü taraf MFA sağlayıcısı gerektiren etkin bir ilke için yedek olarak, etki alanı denetimi olarak Etki Alanı Join gerektiren devre dışı durum bir ilke yapılandırma.
* MFA gerekli olmadığında, [parola kılavuzu](https://aka.ms/passwordguidance) teknik incelemesindeki uygulamaları izleyerek kötü aktörlerin parolaları tahmin etme riskini azaltın.
* Kullanıcıların yasaklamayı seçtiğiniz ortak parola ve terimleri kullanmadığından emin olmak için [Azure AD Self Servis Parola Sıfırlama (SSPR)](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr) ve Azure AD Parola [Koruması'nı](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises-deploy) dağıtın.
* Yalnızca tam erişime geri dönmek yerine belirli bir kimlik doğrulama düzeyine ulaşılamazsa, uygulamalar içindeki erişimi kısıtlayan ilkeleri kullanın. Örnek:
  * Kısıtlı oturum talebini Exchange ve SharePoint'e gönderen bir yedekleme ilkesini yapılandırın.
  * Kuruluşunuz Microsoft Bulut Uygulaması Güvenliği'ni kullanıyorsa, MCAS'ı devreye saçan bir ilkeyi kullanmayı düşünün ve ardından MCAS yalnızca okunur erişime izin verir, ancak yüklemeleri yapmayın.
* Bir aksaklık sırasında bunları kolayca bulmanın kolay olduğundan emin olmak için ilkelerinizi adlandırın. İlke adına aşağıdaki öğeleri ekleyin:
  * İlke için bir *etiket numarası.*
  * Metin göstermek için, bu ilke sadece acil durumlar içindir. Örneğin: **ACIL DURUM İÇİNDE ETKINLEŞTIR**
  * Bozulma *disruption* için geçerlidir. Örneğin: **MFA Kesintisi sırasında**
  * İlkeleri etkinleştirmeniz gereken sırayı göstermek için bir *sıra numarası.*
  * Uygulandığı *uygulamalar.*
  * Uygulanacak *kontroller.*
  * Gerektirdiği *koşullar.*
  
Acil durum ilkeleri için bu adlandırma standardı aşağıdaki gibi olacaktır: 

```
EMnnn - ENABLE IN EMERGENCY: [Disruption][i/n] - [Apps] - [Controls] [Conditions]
```

Aşağıdaki örnek: **Örnek A - Görev açısından kritik İşbirliği Uygulamalarına Erişimi geri yüklemek için**acil durum CA politikası, tipik bir kurumsal durumdur. Bu senaryoda, kuruluş genellikle tüm Exchange Online ve SharePoint Online erişimi için MFA gerektirir ve bu durumda kesinti müşteri için MFA sağlayıcısı bir kesintisi vardır (Azure MFA, şirket içi MFA sağlayıcısı veya üçüncü taraf MFA olsun). Bu ilke, belirli hedeflenmiş kullanıcıların bu uygulamalara güvenilen Windows aygıtlarından erişmesine izin vererek bu kesintiyi azaltır. Ayrıca, acil durum hesaplarını ve temel yöneticileri bu kısıtlamalardan hariç tutar. Hedeflenen kullanıcılar daha sonra Exchange Online ve SharePoint Online'a erişirken, diğer kullanıcılar kesinti nedeniyle uygulamalara erişemeyecek. Bu örnek, hedef kullanıcılarla birlikte adlandırılmış bir ağ konumu **CorpNetwork** ve bir güvenlik grubu **ContingencyAccess,** çekirdek yöneticilere sahip **CoreAdmins** adlı bir grup ve acil erişim hesapları olan **EmergencyAccess** adlı bir grup gerektirir. Acil durum, istenen erişimi sağlamak için dört ilke gerektirir. 

**Örnek A - Görev açısından kritik İşbirliği Uygulamalarına Erişimi geri yüklemek için acil durum CA ilkeleri:**

* İlke 1: Exchange ve SharePoint için Etki Alanı Birleştirilmiş aygıtları gerektirir
  * Adı: EM001 - ENABLE IN EMERGENCY: MFA Disruption[1/4] - Exchange SharePoint - Hybrid Azure AD Join Gerektirir
  * Kullanıcılar ve Gruplar: ContingencyAccess'i ekleyin. CoreAdmins ve EmergencyAccess hariç
  * Bulut Uygulamaları: Exchange Online ve SharePoint Online
  * Koşullar: Herhangi bir
  * Hibe Kontrolü: Etki Alanının Katılmasını İste
  * Durum: Devre Dışı
* İlke 2: Windows dışındaki platformları engelleme
  * Adı: EM002 - ACIL DURUM İçİnDE ETKINLEŞTIR: MFA Disruption[2/4] - Exchange SharePoint - Windows hariç erişimi engelleyin
  * Kullanıcılar ve Gruplar: Tüm kullanıcıları dahil edin. CoreAdmins ve EmergencyAccess hariç
  * Bulut Uygulamaları: Exchange Online ve SharePoint Online
  * Koşullar: Aygıt Platformu Tüm Platformları Dahil Et, Windows'u hariç tut
  * Hibe Kontrolü: Blok
  * Durum: Devre Dışı
* İlke 3: CorpNetwork dışındaki ağları engelleme
  * Adı: EM003 - ACIL DURUM İçİnDE ETKINLEŞTIR: MFA Disruption[3/4] - Exchange SharePoint - Kurumsal Ağ hariç erişimi engelleyin
  * Kullanıcılar ve Gruplar: Tüm kullanıcıları dahil edin. CoreAdmins ve EmergencyAccess hariç
  * Bulut Uygulamaları: Exchange Online ve SharePoint Online
  * Koşullar: Konumlar Herhangi bir konum içerir, CorpNetwork hariç
  * Hibe Kontrolü: Blok
  * Durum: Devre Dışı
* İlke 4: EAS'yi Açıkça Engelleyin
  * Adı: EM004 - ACIL DURUM İçİnDE ETKINLEŞTIR: MFA Disruption[4/4] - Exchange - Tüm kullanıcılar için EAS blok
  * Kullanıcılar ve Gruplar: Tüm kullanıcıları dahil edin
  * Bulut Uygulamaları: Exchange Online'ı dahil et
  * Koşullar: İstemci uygulamaları: Exchange Active Sync
  * Hibe Kontrolü: Blok
  * Durum: Devre Dışı

Etkinleştirme sırası:

1. Mevcut MFA ilkesinden ContingencyAccess, CoreAdmins ve EmergencyAccess'i hariç tinleyin. ContingencyAccess'teki bir kullanıcının SharePoint Online ve Exchange Online'a erişebileceğini doğrulayın.
2. Etkinleştir İlke 1: Dışgruplarda olmayan Etki Alanı Birleştirilmiş cihazlardaki kullanıcıları Exchange Online ve SharePoint Online'a erişebilmelerini doğrulayın. Dışlama grubundaki kullanıcıların Herhangi bir cihazdan SharePoint Online ve Exchange'e erişebileceğini doğrulayın.
3. Politika yı etkinleştirin 2: Dışlama grubunda olmayan kullanıcıların mobil cihazlarından SharePoint Online ve Exchange Online'a ulaşamamasını doğrulayın. Dışlama grubundaki kullanıcıların herhangi bir cihazdan (Windows/iOS/Android) SharePoint ve Exchange'e erişebileceğini doğrulayın.
4. İlke 3'e EtkinLiği Etkinleştir: Dışlama gruplarında olmayan kullanıcıların, bir etki alanı birleştirilmiş makinede bile SharePoint ve Exchange'e şirket ağından erişemeyeceğini doğrulayın. Dışlama grubundaki kullanıcıların herhangi bir ağdan SharePoint ve Exchange'e erişebileceğini doğrulayın.
5. İlke 4'i Etkinleştir: Tüm kullanıcıların mobil cihazlardaki yerel posta uygulamalarından Exchange Online alamayacağını doğrulayın.
6. SharePoint Online ve Exchange Online için mevcut MFA ilkesini devre dışı kınleyin.

Bu sonraki örnekte, **Örnek B - Salesforce mobil erişim eve izin**acil DURUM CA ilkeleri, bir iş uygulamasının erişimi geri yüklenir. Bu senaryoda, müşteri genellikle satış çalışanlarının yalnızca uyumlu aygıtlardan izin verilabilmesi için satış çalışanlarının mobil cihazlardan Salesforce'a (Azure AD ile tek oturum açma için yapılandırılan) erişimini gerektirir. Bu durumda kesinti, cihaz uyumluluğunun değerlendirilmesiyle ilgili bir sorun olması ve satış ekibinin anlaşmaları kapatmak için Salesforce'a erişmesi gereken hassas bir zamanda kesintinin meydana geldiğidir. Bu acil durum ilkeleri, kritik kullanıcılara satışları kapatmaya devam edebilmeleri ve işi aksatmayacakları bir mobil cihazdan Salesforce'a erişim hakkı verecektir. Bu örnekte, **SalesforceContingency** erişimi korumak için gereken tüm Satış çalışanlarını içerir ve **SalesAdmins** Salesforce gerekli yöneticileri içerir.

**Örnek B - Acil DURUM CA ilkeleri:**

* İlke 1: SalesContingency ekibinde olmayan herkesi engelleme
  * Adı: EM001 - ACIL DURUM İÇİNDE ETKINLEŞTIR: Cihaz Uyumluluğu Bozulması[1/2] - Salesforce - SalesforceContingency dışındaki tüm kullanıcıları engelleyin
  * Kullanıcılar ve Gruplar: Tüm kullanıcıları dahil edin. SalesAdmins ve SalesforceContingency hariç
  * Bulut Uygulamaları: Salesforce.
  * Koşullar: Yok
  * Hibe Kontrolü: Blok
  * Durum: Devre Dışı
* İlke 2: Satış ekibini mobil dışında herhangi bir platformdan engelleyin (saldırı yüzey alanını azaltmak için)
  * Adı: EM002 - ACIL DURUM İçİnDE ETKINLEŞTIR: Cihaz Uyumluluğu Kesintisi[2/2] - Salesforce - iOS ve Android dışındaki tüm platformları engelleyin
  * Kullanıcılar ve Gruplar: SalesforceContingency'yi dahil edin. SalesAdmins hariç
  * Bulut Uygulamaları: Salesforce
  * Koşullar: Cihaz Platformu Tüm Platformları Dahil Edin, iOS ve Android hariç
  * Hibe Kontrolü: Blok
  * Durum: Devre Dışı

Etkinleştirme sırası:

1. SalesAdmins ve SalesforceContingency'yi Salesforce için varolan aygıt uyumluluk ilkesinden hariç tinleyin. SalesforceContingency grubundaki bir kullanıcının Salesforce'a erişebileceğini doğrulayın.
2. Etkinleştir İlke 1: SalesContingency dışındaki kullanıcıların Salesforce'a erişemeyeceğini doğrulayın. SalesAdmins ve SalesforceContingency'deki kullanıcıları Salesforce'a erişebilen doğrulayın.
3. Etkinleştir İlke 2: SalesContingency grubundaki kullanıcıların Windows/Mac dizüstü bilgisayarlarından Salesforce'a erişemediğini ancak mobil cihazlarından erişebilmelerini doğrulayın. SalesAdmin'in Salesforce'a herhangi bir cihazdan erişebileceğini doğrulayın.
4. Salesforce için varolan aygıt uyumluluk ilkesini devre dışı bırakın.

### <a name="deploy-password-hash-sync-even-if-you-are-federated-or-use-pass-through-authentication"></a>Federe olsanız veya geçiş kimlik doğrulaması kullansanız bile parola karma eşitleme dağıtma

Aşağıdaki koşullar doğruysa kullanıcı kilitleme si de oluşabilir:

- Kuruluşunuz geçiş kimlik doğrulaması veya federasyon içeren karma kimlik çözümlü bir çözüm kullanır.
- Şirket içi kimlik sistemleriniz (Active Directory, AD FS veya bağımlı bileşen gibi) kullanılamaz. 
 
Daha esnek olması için, kuruluşunuzun [parola karma eşitlemesini etkinleştirmesi](https://docs.microsoft.com/azure/security/fundamentals/choose-ad-authn)gerekir, çünkü şirket içi kimlik sistemleriniz kapalıysa [parola karma eşitlemesini kullanmaya geçmenizi](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-user-signin) sağlar.

#### <a name="microsoft-recommendations"></a>Microsoft önerileri
 Kuruluşunuzun federasyon veya geçiş kimlik doğrulaması kullanıp kullanmadığına bakılmaksızın Azure AD Connect sihirbazını kullanarak parola karma eşitlemesini etkinleştirin.

>[!IMPORTANT]
> Parola karma eşitleme sini kullanmak için kullanıcıları federeden yönetilen kimlik doğrulamasına dönüştürmek gerekmez.

## <a name="during-a-disruption"></a>Bir aksaklık sırasında

Bir azaltma planı uygulamayı seçtiyseniz, tek bir erişim denetimi kesintisidurumundan otomatik olarak kurtulabilirsiniz. Ancak, bir acil durum planı oluşturmayı seçtiyseniz, erişim denetimi kesintisi sırasında acil durum ilkelerinizi etkinleştirebilirsiniz:

1. Hedeflenen kullanıcılara belirli ağlardan belirli uygulamalara erişim sağlayan acil durum ilkelerinizi etkinleştirin.
2. Düzenli kontrol tabanlı ilkelerinizi devre dışı kalın.

### <a name="microsoft-recommendations"></a>Microsoft önerileri

Bir aksaklık sırasında hangi azaltıcı etkenlerin veya olası durumlar kullanıldığına bağlı olarak, kuruluşunuz yalnızca parolalarla erişim izni verebilir. Hiçbir güvenlik dikkatle tartılması gereken önemli bir güvenlik riski değildir. Kuruluşlar şunları yapmalı:

1. Değişiklik denetimi stratejinizin bir parçası olarak, erişim denetimleri tam olarak çalışır duruma gelmez uyguladığınız olası durumları geri alabilmek için her değişikliği ve önceki durumu belgele.
2. Kötü niyetli aktörlerin, siz MFA'yı devre dışı bıraktığınızda parola spreyi veya kimlik avı saldırıları yoluyla parola toplamaya çalışacağını varsayalım. Ayrıca, kötü aktörler zaten daha önce bu pencere sırasında denenebilir herhangi bir kaynağa erişim izni vermedi parolaları olabilir. Yöneticiler gibi kritik kullanıcılar için, mfa'yı devre dışı bırakmadan önce parolalarını sıfırlayarak bu riski kısmen azaltabilirsiniz.
3. MFA devre dışı bırakıldığı süre içinde kimlere erişenleri belirlemek için tüm oturum açma etkinliklerini arşivleyin.
4. Bu pencere sırasında [bildirilen tüm risk tespitlerini triyaj.](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins)

## <a name="after-a-disruption"></a>Bir aksaklık tan sonra

Etkinleştirilmiş acil durum planının bir parçası olarak yaptığınız değişiklikleri, kesintiye neden olan hizmet geri yüklendikten sonra geri al. 

1. Düzenli ilkeleri etkinleştirme
2. Acil durum ilkelerinizi devre dışı bırakın. 
3. Kesinti sırasında yaptığınız ve belgelediğiniz diğer değişiklikleri geri alabilirsiniz.
4. Acil erişim hesabı kullandıysanız, kimlik bilgilerini yeniden oluşturmayı ve acil erişim hesabı yordamlarınızın bir parçası olarak yeni kimlik bilgilerini fiziksel olarak güvenli hale almayı unutmayın.
5. Şüpheli etkinlik için bozulmadan sonra [bildirilen tüm risk tespitlerini üçe kalım](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins) edin.
6. Bir dizi kullanıcıyı hedeflemek için [PowerShell kullanılarak](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0) verilen tüm yenileme belirteçlerini iptal edin. Tüm yenileme belirteçlerinin iptali, kesinti sırasında kullanılan ayrıcalıklı hesaplar için önemlidir ve bunu yapmak onları yeniden kimlik doğrulamaya ve geri yüklenen politikaların denetimini karşılamaya zorlar.

## <a name="emergency-options"></a>Acil durum seçenekleri

 Acil bir durum olması ve kuruluşunuz daha önce bir azaltma veya acil durum planı uygulamamışsa, MFA'yı uygulamak için Koşullu Erişim ilkelerini zaten kullanıyorlarsa, kullanıcı kilitleme bölümü [için Acil Durumlar'daki](#contingencies-for-user-lockout) önerileri izleyin.
Kuruluşunuz kullanıcı başına MFA eski ilkeleri kullanıyorsa, aşağıdaki alternatifi göz önünde bulundurabilirsiniz:

1. Şirket ağı giden IP adresiniz varsa, kimlik doğrulamayı yalnızca şirket ağına etkinleştirmek için bunları güvenilir IP'ler olarak ekleyebilirsiniz.
   1. Giden IP adreslerinin envanterine sahip değilseniz veya şirket ağının içinde ve dışında erişimi etkinleştirmeniz gerekiyorsa, 0.0.0.0/1 ve 128.0.0.0/1'i belirterek IPv4 adres alanının tamamını güvenilir IP'ler olarak ekleyebilirsiniz.

>[!IMPORTANT]
 > Güvenilen IP adreslerini erişim engelini kaldırmak için genişletirseniz, IP adresleriyle ilişkili risk algılamaları (örneğin, imkansız seyahat veya yabancı konumlar) oluşturulmaz.

>[!NOTE]
 > Azure MFA için [güvenilir IP'leri](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-mfasettings) yapılandırmak yalnızca [Azure AD Premium lisanslarıyla](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-licensing)kullanılabilir.

## <a name="learn-more"></a>Daha fazla bilgi edinin

* [Azure AD Kimlik Doğrulaması Belgeleri](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-iis)
* [Azure AD'de acil erişim yönetim hesaplarını yönetme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access)
* [Azure Etkin Dizini'nde adlandırılmış konumları yapılandırma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)
  * [Set-MsolDomainFederationSettings](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0)
* [Karma Azure Active Directory birleştirilmiş aygıtları yapılandırma](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan)
* [İş İçin Windows Hello Dağıtım Kılavuzu](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-guide)
  * [Parola Kılavuzu - Microsoft Research](https://research.microsoft.com/pubs/265143/microsoft_password_guidance.pdf)
* [Azure Active Directory Koşullu Erişim'deki koşullar nelerdir?](https://docs.microsoft.com/azure/active-directory/conditional-access/conditions)
* [Azure Active Directory Koşullu Erişim'de erişim denetimleri nelerdir?](https://docs.microsoft.com/azure/active-directory/conditional-access/controls)
