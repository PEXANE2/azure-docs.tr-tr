---
title: Karma kimlik tasarımı - yaşam döngüsü benimseme stratejisi Azure | Microsoft Dokümanlar
description: Her yaşam döngüsü aşaması için kullanılabilir seçeneklere göre karma kimlik yönetimi görevlerinin tanımlanmasına yardımcı olur.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 420b6046-bd9b-4fce-83b0-72625878ae71
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 85f600c8bd46e699e80bf7b596574dc01467ef79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67109309"
---
# <a name="determine-hybrid-identity-lifecycle-adoption-strategy"></a>Karma kimlik yaşam döngüsü benimseme stratejisini belirleme
Bu görevde, [karma kimlik yönetimi görevlerini](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)belirleyin'de tanımladığınız iş gereksinimlerini karşılamak için karma kimlik çözümünüz için kimlik yönetimi stratejisini tanımlarsınız.

Bu adımda daha önce sunulan uçtan uca kimlik yaşam döngüsüne göre karma kimlik yönetimi görevlerini tanımlamak için, her yaşam döngüsü aşaması için kullanılabilir seçenekleri göz önünde bulundurmanız gerekir.

## <a name="access-management-and-provisioning"></a>Erişim yönetimi ve sağlama
İyi bir hesap erişim yönetimi çözümüyle, kuruluşunuz kuruluş genelinde hangi bilgilere erişebileceğini tam olarak izleyebilir.

Erişim denetimi, merkezi, tek noktalı bir sağlama sisteminin kritik bir işlevidir. Erişim denetimleri, hassas bilgileri korumanın yanı sıra onaylanmamış yetkilendirmelere sahip veya artık gerekli olmayan varolan hesapları da ortaya çıkarır. Eski hesapları denetlemek için, sağlama sistemi hesap bilgilerini, hesapların sahibi olan kullanıcılar hakkında yetkili bilgilerle bir araya getirir. Yetkili kullanıcı kimliği bilgileri genellikle insan kaynakları veritabanlarında ve dizinlerinde tutulur.

Gelişmiş BT işletmelerindeki hesaplar, yetkilileri tanımlayan yüzlerce parametre içerir ve bu ayrıntılar sağlama sisteminiz tarafından denetlenebilir. Yeni kullanıcılar yetkili kaynaktan sağladığınız verilerle tanımlanabilir. Erişim isteği onay özelliği, onlar için kaynak sağlanmasını onaylayan (veya reddeden) işlemleri başlatır.

| Yaşam döngüsü yönetimi aşaması | Şirket içinde | Bulut | Karma |
| --- | --- | --- | --- |
| Hesap Yönetimi ve Tedarik |Active Directory® Etki Alanı Hizmetleri (AD DS) sunucu rolünü kullanarak, kullanıcı ve kaynak yönetimi için ölçeklenebilir, güvenli ve yönetilebilir bir altyapı oluşturabilir ve Microsoft® Exchange Server gibi dizin özellikli uygulamalar için destek sağlayabilirsiniz. <br><br> [AD DS'deki grupları bir Kimlik yöneticisi aracılığıyla sağlayabilirsiniz](https://technet.microsoft.com/library/ff686261.aspx) <br>[Kullanıcıları AD DS'de sağlayabilirsiniz](https://technet.microsoft.com/library/ff686263.aspx) <br><br> Yöneticiler, güvenlik amacıyla paylaşılan kaynaklara kullanıcı erişimini yönetmek için erişim denetimini kullanabilir. Etkin Dizin'de, erişim denetimi, Tam Denetim, Yazma, Okuma veya Erişim Yok gibi nesnelere farklı erişim düzeyleri veya izinler ayarlayarak nesne düzeyinde yönetilir. Active Directory'deki erişim denetimi, farklı kullanıcıların Active Directory nesnelerini nasıl kullanabileceğini tanımlar. Varsayılan olarak, Etkin Dizin'deki nesnelerüzerindeki izinler en güvenli ayarda ayarlanır. |Bir Microsoft bulut hizmetine erişecek her kullanıcı için bir hesap oluşturmanız gerekir. Ayrıca, artık ihtiyaç duyulmadığında kullanıcı hesaplarını değiştirebilir veya silebilirsiniz. Varsayılan olarak, kullanıcıların yönetici izinleri yoktur, ancak isteğe bağlı olarak atayabilirsiniz. <br><br> Azure Etkin Dizini'nde en önemli özelliklerden biri kaynaklara erişimi yönetebilme yeteneğidir. Bu kaynaklar dizindeki roller aracılığıyla nesneleri yönetme izinlerinde olduğu gibi bir dizine veya SaaS uygulamaları, Azure hizmetleri ve SharePoint siteleri ya da şirket içi kaynaklar gibi dizin dışındaki kaynaklara ait olabilir. <br><br> Azure Active Directory'nin erişim yönetimi solution'ının merkezinde güvenlik grubu yer alır. Kaynak sahibi (veya dizinin yöneticisi), bir gruba sahip oldukları kaynaklara belirli bir erişim hakkı atayabilir. Grup üyelerine erişim sağlanacaktır ve kaynak sahibi bir grubun üye listesini yönetme hakkını bir bölüm yöneticisi veya yardım masası yöneticisi gibi başka birine devredebilir<br> <br> Azure AD bölümündeki Yönetim grupları, gruplar aracılığıyla erişimi yönetme hakkında daha fazla bilgi sağlar. |Eşitleme ve federasyon aracılığıyla Etkin Dizin kimliklerini buluta genişletme |

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi
Rol tabanlı erişim denetimi (RBAC), kullanıcılara erişim sağlamak için iş süreçlerinizi ve kurallarınızı değerlendirmek, sınamak ve uygulamak için rolleri ve sağlama ilkelerini kullanır. Anahtar yöneticiler sağlama ilkeleri oluşturur ve kullanıcıları rollere atar ve bu roller için kaynaklara yetki kümelerini tanımlar. RBAC, yazılım tabanlı süreçleri kullanmak ve sağlama sürecinde kullanım kılavuzu etkileşimini azaltmak için kimlik yönetimi çözümlerini genişletir.
Azure AD RBAC, şirketin Azure portalına erişiğinde bir kişinin yapabileceği işlem sayısını kısıtlamasına olanak tanır. Portala erişimi denetlemek için RBAC'ı kullanarak BT Yöneticileri aşağıdaki erişim yönetimi yaklaşımlarını kullanarak erişimi devreder:

* **Grup tabanlı rol ataması**: Yerel Active Dizininiz'den senkronize edilebilen Azure REKLAM gruplarına erişim atayabilirsiniz. Bu, kuruluşunuzun grupları yönetmek için takım lama ve işlemlerde yaptığı mevcut yatırımlardan yararlanmanızı sağlar. Azure AD Premium'un temsilci grup yönetimi özelliğini de kullanabilirsiniz.
* **Azure'da yerleşik rollerden yararlanma**: Kullanıcıların ve grupların yalnızca işlerini yapmak için gereken görevleri yapma iznine sahip olmalarını sağlamak için üç rol (Sahip, Katılımcı ve Okuyucu) kullanabilirsiniz.
* **Kaynaklara parçalı erişim**: Belirli bir abonelik, kaynak grubu veya web sitesi veya veritabanı gibi tek bir Azure kaynağı için kullanıcılara ve gruplara roller atayabilirsiniz. Bu şekilde, kullanıcıların gereksinim duydukları tüm kaynaklara erişebilmesini ve yönetmeleri gerekmeyen kaynaklara erişmemelerini sağlayabilirsiniz.

## <a name="provisioning-and-other-customization-options"></a>Sağlama ve diğer özelleştirme seçenekleri
Ekibiniz, kimlik çözümünüzü ne kadar özelleştireceğine karar vermek için iş planlarını ve gereksinimlerini kullanabilir. Örneğin, büyük bir kuruluş, coğrafyalar arasında yaygın olarak kullanılan uygulamaları artımlı olarak sağlama için bir zaman çizgisini temel alan iş akışları ve özel bağdaştırıcılar için aşamalı bir roll-out planı gerektirebilir. Başka bir özelleştirme planı, başarılı sınama dan sonra, tüm kuruluş genelinde iki veya daha fazla uygulamanın sağlanmasını sağlayabilir. Kullanıcı-uygulama etkileşimi özelleştirilebilir ve kaynakları sağlama yordamları otomatik sağlama yı karşılayacak şekilde değiştirilebilir.

Bir hizmeti veya bileşeni kaldırmak için hükmünü kaldırabilirsiniz. Örneğin, bir hesabın deprovisioning hesabın bir kaynaktan silinir anlamına gelir.

Kaynakları sağlamanın karma modeli, her ikisi de Azure AD tarafından desteklenen istek ve rol tabanlı yaklaşımları birleştirir. Çalışanların veya yönetilen sistemlerin bir alt kümesi için, bir işletme rol tabanlı atamayla erişimi otomatikleştirmek isteyebilir. Bir işletme, istek tabanlı bir model aracılığıyla diğer tüm erişim isteklerini veya özel durumlarını da işleyebilir. Bazı işletmeler el ile atamayla başlayabilir ve gelecekteki bir zamanda tam rol tabanlı bir dağıtım niyetiyle karma bir modele doğru evrilir.

Diğer şirketler, tam rol tabanlı sağlama elde etmek için iş nedenleriyle pratik bulabilirsiniz ve istenen bir hedef olarak melez bir yaklaşım hedef. Yine de diğer şirketler yalnızca istek tabanlı sağlamadan memnun olabilir ve rol tabanlı, otomatik sağlama ilkelerini tanımlamak ve yönetmek için ek çaba sarf etmek istemeyebilir.

## <a name="license-management"></a>Lisans yönetimi
Azure AD'deki grup tabanlı lisans yönetimi, yöneticilerin kullanıcıları bir güvenlik grubuna atamasını sağlar ve Azure AD grubun tüm üyelerine otomatik olarak lisans atar. Bir kullanıcı daha sonra gruba eklenir veya gruptan çıkarılırsa, bir lisans uygun şekilde otomatik olarak atanır veya kaldırılır.

Şirket içi AD'den senkronize ettiğiniz veya Azure AD'de yönettiğiniz grupları kullanabilirsiniz. Bunu Azure AD premium Self Servis Grup Yönetimi ile eşleştirmenizde, lisans atamasını uygun karar vericilere kolayca devredebilirsiniz. Lisans çakışmaları ve eksik konum verileri gibi sorunların otomatik olarak çözülmeden emin olabilirsiniz.

## <a name="self-regulating-user-administration"></a>Kendi kendini düzenleyen kullanıcı yönetimi
Kuruluşunuz tüm iç kuruluşlarda kaynak sağlamaya başladığında, kendi kendini düzenleyen kullanıcı yönetimi özelliğini uygularsınız. Kullanıcıları kuruluş sınırları nın ötesine sağlamanın avantaj ve avantajlarını fark edebilirsiniz. Bu ortamda, kullanıcının durumundaki bir değişiklik, kuruluş sınırları ve coğrafyalar arasında erişim haklarına otomatik olarak yansıtılır. Sağlama maliyetlerini azaltabilir ve erişim ve onay işlemlerini kolaylaştırabilirsiniz. Uygulama, kuruluşunuzdaki uçtan uca erişim yönetimi için rol tabanlı erişim denetimi uygulama nın tüm potansiyelini fark eder. Kullanıcı sağlamayı yönetmek için otomatik yordamlar aracılığıyla yönetim maliyetlerini azaltabilirsiniz. Güvenlik ilkesi zorlamayı otomatikleştirerek güvenliği artırabilir ve büyük kullanıcı popülasyonları için kullanıcı yaşam döngüsü yönetimini ve kaynak sağlamayı kolaylaştırabilir ve merkezileştirebilirsiniz.

> [!NOTE]
> Daha fazla bilgi için bkz: Self servis uygulama erişim yönetimi için Azure AD'yi ayarlama
> 
> 

Lisans tabanlı (Yetkilendirme tabanlı) Azure AD hizmetleri, Azure AD dizininizde/hizmet kiracınızda bir aboneliği etkinleştirerek çalışır. Abonelik etkin olduğunda, hizmet yetenekleri dizin/hizmet yöneticileri tarafından yönetilebilir ve lisanslı kullanıcılar tarafından kullanılabilir. 

## <a name="integration-with-other-3rd-party-providers"></a>Diğer üçüncü taraf sağlayıcılarla entegrasyon

Azure Active Directory, binlerce SaaS uygulamasına ve şirket içi web uygulamasına tek oturum açma ve gelişmiş uygulama erişim güvenliği sağlar. Daha fazla bilgi için bkz: [Azure Etkin Dizini ile uygulamaları tümleştirme](../develop/quickstart-v1-integrate-apps-with-azure-ad.md)

## <a name="define-synchronization-management"></a>Eşitleme yönetimini tanımlama
Şirket içi dizinlerinizin Azure AD ile tümleştirilmesi, kullanıcılarınızın hem bulut kaynaklarına hem de şirket içi kaynaklara erişmesi için ortak bir kimlik oluşturarak daha üretken olmalarını sağlar. Bu tümleştirme ile kullanıcılar ve kuruluşlar aşağıdakilerden yararlanabilir:

* Kuruluşlar, Windows Server Active Directory'den yararlanarak ve ardından Azure Active Directory'ye bağlanarak kullanıcılara şirket içi veya bulut tabanlı hizmetler de ortak bir karma kimlik sağlayabilir.
* Yöneticiler, uygulama kaynağına, aygıta ve kullanıcı kimliğine, ağ konumuna ve çok faktörlü kimlik doğrulamaya dayalı Koşullu Erişim sağlayabilir.
* Kullanıcılar, Azure AD'den Office 365, Intune, SaaS uygulamaları ve üçüncü taraf uygulamalarına kadar olan hesaplar aracılığıyla ortak kimliklerini kullanabilirler.
* Geliştiriciler, bulut tabanlı uygulamalar için uygulamaları şirket içinde Active Directory'ye veya Azure'a entegre ederek ortak kimlik modelinden yararlanan uygulamalar oluşturabilir

Aşağıdaki şekilde kimlik eşitleme işleminin üst düzey görünümüne bir örnek verilmiştir.

![Sync](./media/plan-hybrid-identity-design-considerations/identitysync.png)

Kimlik senkronizasyon süreci

Eşitleme seçeneklerini karşılaştırmak için aşağıdaki tabloyu gözden geçirin:

| Senkronizasyon Yönetimi Seçeneği | Yararları | Dezavantajlar |
| --- | --- | --- |
| Eşitleme tabanlı (DirSync veya AADConnect üzerinden) |Kullanıcılar ve şirket içi ve buluttan senkronize edilen gruplar <br>  **İlke denetimi**: Hesap ilkeleri, yöneticiye bulutta ek görevler gerçekleştirmek zorunda kalmadan parola ilkelerini, iş istasyonunu, kısıtlamaları, kilitleme denetimlerini ve daha fazlasını yönetme olanağı sağlayan Active Directory aracılığıyla ayarlanabilir.  <br>  **Erişim denetimi**: Hizmetlere kurumsal ortam, çevrimiçi sunucular veya her ikisi üzerinden erişilebilmek için bulut hizmetine erişimi kısıtlayabilir. <br>  Azaltılmış destek çağrıları: Kullanıcıların hatırlayacak daha az parolası varsa, bunları unutma olasılıkları daha düşüktür. <br>  Güvenlik: Tek oturum açmada kullanılan tüm sunucular ve hizmetler şirket içinde denetlendive denetlendirilebildiği için kullanıcı kimlikleri ve bilgileri korunur. <br>  Güçlü kimlik doğrulama desteği: Bulut hizmetiyle güçlü kimlik doğrulamayı (iki faktörlü kimlik doğrulama olarak da adlandırılır) kullanabilirsiniz. Ancak, güçlü kimlik doğrulaması kullanıyorsanız, tek oturum açma kullanmanız gerekir. | |
| Federasyon tabanlı (AD FS aracılığıyla) |Güvenlik Belirteç Hizmeti (STS) tarafından etkinleştirildi. Bir STS'yi bir Microsoft bulut hizmetiyle tek oturum açma erişimi sağlayacak şekilde yapılandırdığınızda, şirket içi STS'niz ile Azure AD kiracınızda belirttiğiniz federal etki alanı arasında federal bir güven oluşturursunuz. <br> Son kullanıcıların birden çok kaynağa erişmek için aynı kimlik bilgileri kümesini kullanmasına izin verir <br>son kullanıcıların birden çok kimlik bilgileri kümesini korumak zorunda değildir. Ancak, kullanıcıların iştirak eden kaynakların her birine kimlik bilgilerini sağlamaları gerekir.,B2B ve B2C senaryoları desteklenir. |Özel olarak şirket içi AD FS sunucularının dağıtımı ve bakımı için özel personel gerektirir. STS'niz için AD FS kullanmayı planlıyorsanız, güçlü kimlik doğrulamasının kullanımına ilişkin kısıtlamalar vardır. Daha fazla bilgi için, [AD FS 2.0 için Gelişmiş Seçenekleri Yapılandırma'ya](https://go.microsoft.com/fwlink/?linkid=235649)bakın. |

> [!NOTE]
> Daha fazla bilgi için, [şirket içi kimliklerinizi Azure Etkin Dizini ile tümleştirin.](whatis-hybrid-identity.md)
> 
> 

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

