---
title: Azure Active Directory kullanıcı sağlama için bulut İk uygulamasını planlayın
description: Bu makalede, İş Günü ve Başarı Faktörleri gibi bulut İk sistemlerini Azure Etkin Dizini ile tümleştirme dağıtım işlemi açıklanmaktadır. Azure AD'yi bulut İk sisteminizle tümleştirmek, tam bir kimlik yaşam döngüsü yönetim sistemi sağlar.
services: active-directory
author: martincoetzer
manager: CelesteDG
tags: azuread
ms.service: active-directory
ms.subservice: app-provisioning
ms.topic: conceptual
ms.workload: identity
ms.date: 11/22/2019
ms.author: martinco
ms.reviewer: arvindha
ms.openlocfilehash: 28abe2dfa5a1a13ba09e20202180cb5e47d94072
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77522441"
---
# <a name="plan-cloud-hr-application-to-azure-active-directory-user-provisioning"></a>Azure Active Directory kullanıcı sağlama için bulut İk uygulamasını planlayın

Tarihsel olarak, BT personeli çalışanları oluşturmak, güncelleştirmek ve silmek için el ile yöntemlere güvendi. Çalışan verilerini eşitlemek için CSV dosyaları veya özel komut dosyaları yükleme gibi yöntemler kullandılar. Bu sağlama işlemleri hataya yatkın, güvensiz ve yönetilmesi zor.

Çalışanların, satıcıların veya şartlı çalışanların kimlik yaşam döngülerini yönetmek için [Azure Active Directory (Azure AD) kullanıcı sağlama hizmeti](../app-provisioning/user-provisioning.md) bulut tabanlı insan kaynakları (İk) uygulamalarıyla entegrasyon sunar. Uygulamalara örnek olarak İş Günü veya SuccessFactors verilebilir.

Azure AD, aşağıdaki bulut İk uygulaması (uygulama) iş akışlarını etkinleştirmek için bu tümleştirmeyi kullanır:

- **Kullanıcıları Active Directory'ye sağlama:** Bir bulut İk uygulamasından seçilen kullanıcı kümelerini bir veya daha fazla Active Directory etki alanına sağlama.
- **Azure AD'ye yalnızca bulut kullanıcıları sağlama:** Active Directory'nin kullanılmadığı senaryolarda, kullanıcıları doğrudan bulut İk uygulamasından Azure AD'ye kaydedin.
- **Bulut İk uygulamasına geri yazın:** Azure AD'deki e-posta adreslerini ve kullanıcı adı özniteliklerini bulut İk uygulamasına geri yazın.

> [!NOTE]
> Bu dağıtım planı, Azure AD kullanıcı sağlama ile bulut İk uygulama iş akışlarınızı nasıl dağıtabileceğinizi gösterir. Otomatik kullanıcı sağlamanın yazılıma hizmet (SaaS) uygulamaları olarak nasıl dağıtılacak hakkında bilgi [için](https://aka.ms/deploymentplans/provisioning)bkz.

## <a name="enabled-hr-scenarios"></a>Etkin İk senaryoları

Azure AD kullanıcı sağlama hizmeti, aşağıdaki İk tabanlı kimlik yaşam döngüsü yönetimi senaryolarının otomasyonuna olanak tanır:

- **Yeni çalışan işe alım:** Bulut İk uygulamasına yeni bir çalışan eklendiğinde, bulut İk uygulamasına e-posta adresini ve kullanıcı adı özniteliklerini geri yazma seçeneğiyle Active Directory ve Azure AD'de otomatik olarak bir kullanıcı hesabı oluşturulur.
- **Çalışan özniteliği ve profil güncelleştirmeleri:** Bulut İk uygulamasında ad, unvan veya yönetici gibi bir çalışan kaydı güncelleştirildiğinde, kullanıcı hesabı Active Directory ve Azure AD'de otomatik olarak güncelleştirilir.
- **Çalışan sonlandırmaları:** Bulut İk uygulamasında bir çalışanın işine son verildiğinde, kullanıcı hesabı Active Directory ve Azure AD'de otomatik olarak devre dışı bırakılır.
- **Çalışan yeniden işe:** Bir çalışan bulut İk uygulamasında yeniden işe alındığında, eski hesabı otomatik olarak yeniden etkinleştirilebilir veya Active Directory ve Azure AD'ye yeniden kullanılabilir.

## <a name="who-is-this-integration-best-suited-for"></a>Bu entegrasyon kimin için en uygun?

Azure AD kullanıcı sağlama ile bulut İk uygulaması entegrasyonu, aşağıdaki leri yapan kuruluşlar için idealdir:

- Bulut İk kullanıcı sağlama için önceden oluşturulmuş, bulut tabanlı bir çözüm istiyorum.
- Bulut İk uygulamasından Active Directory veya Azure AD'ye doğrudan kullanıcı sağlama gereksinimi.
- Bulut İk uygulamasından elde edilen verileri kullanarak kullanıcıların sağlanmasını zorunlu kılmasını zorunlu kınla.
- Yalnızca bulut İk uygulamasında algılanan değişiklik bilgilerine dayalı olarak, kullanıcıların bir veya daha fazla Active Directory ormanları, etki alanları ve OS'lerle eşitlenmelerini, taşımayı ve bırakmayı zorunlu kılmasını gerektirir.
- E-posta için Office 365'i kullanın.

## <a name="learn"></a>Öğrenin

Kullanıcı sağlama, sürekli kimlik yönetimi için bir temel oluşturur. Yetkili kimlik verilerine dayanan iş süreçlerinin kalitesini artırır.

### <a name="terms"></a>Koşullar

Bu makalede aşağıdaki terimler kullan›r:

- **Kaynak sistem**: Azure AD'nin karşıladığı kullanıcı deposu. İş Günü veya SuccessFactors gibi bir bulut İk uygulaması örneğidir.
- **Hedef sistem**: Azure REKLAM'ın karşılık verdiği kullanıcı deposu. Etkin Dizin, Azure AD, Office 365 veya diğer SaaS uygulamaları bunlara örnek olarak verilebilir.
- **Joiners-Movers-Leavers süreci**: Bir bulut İk uygulamasını kayıt sistemi olarak kullanarak yeni işe alımlar, transferler ve sonlandırma için kullanılan bir terimdir. Hizmet, hedef sisteme gerekli öznitelikleri başarıyla karşıladığında işlem tamamlar.

### <a name="key-benefits"></a>Önemli avantajlar

İk odaklı BT sağlamanın bu özelliği aşağıdaki önemli iş avantajlarını sunar:

- **Üretkenliği artırın:** Artık kullanıcı hesaplarının ve Office 365 lisanslarının atamasını otomatikleştirebilir ve anahtar gruplara erişim sağlayabilirsiniz. Atamaları otomatikleştirmek, yeni işe alımlara iş araçlarına anında erişim sağlar ve üretkenliği artırır.
- **Riski yönetin:** Bulut İk uygulamasından gelen verilerle çalışan durumuna veya grup üyeliklerine göre değişiklikleri otomatikleştirerek güvenliği artırabilirsiniz. Değişiklikleri otomatikleştirmek, kullanıcılar kuruluşa geçiş yaptığında veya kuruluştan ayrıldıklarında kullanıcı kimliklerinin ve önemli uygulamalara erişimin otomatik olarak güncelolmasını sağlar.
- **Adres uygunluğu ve yönetişim:** Azure AD, hem kaynak hem de hedef sistemlerin uygulamaları tarafından gerçekleştirilen kullanıcı sağlama istekleri için yerel denetim günlüklerini destekler. Denetim de, uygulamalara kimlerin erişebildiği tek bir ekrandan izleyebilirsiniz.
- **Maliyeti yönetin:** Otomatik sağlama, manuel sağlamayla ilişkili verimsizlikleri ve insan hatalarını önleyerek maliyetleri azaltır. Eski ve eski platformlar kullanarak zaman içinde oluşturulmuş özel olarak geliştirilmiş kullanıcı sağlama çözümlerine olan gereksinimi azaltır.

### <a name="licensing"></a>Lisanslama

Bulut İk uygulamasını Azure AD kullanıcı sağlama tümleştirmesine yapılandırmak için geçerli bir [Azure AD Premium lisansına](https://azure.microsoft.com/pricing/details/active-directory/) ve İş Günü veya Başarı Faktörleri gibi bulut İk uygulaması için bir lisansa gereksinim duyarsınız.

Ayrıca, bulut İk uygulamasından temin edilecek ve Active Directory veya Azure AD'ye sağlanan her kullanıcı için geçerli bir Azure AD Premium P1 veya daha yüksek abonelik lisansına ihtiyacınız var. Bulut İk uygulamasında sahip olunan yanlış sayıda lisans, kullanıcı sağlama sırasında hatalara neden olabilir.

### <a name="prerequisites"></a>Ön koşullar

- Azure AD Connect sağlama aracısını yapılandırmak için Azure AD yöneticisi erişimi.
- Bulut İk uygulamasının test ve üretim örneği.
- Bir sistem tümleştirme kullanıcısı oluşturmak ve test amacıyla çalışan verilerini sınamak için değişiklikler yapmak için bulut İk uygulamasındayönetici izinleri.
- Active Directory'ye kullanıcı sağlama için, .NET 4.7.1+ çalışma süresiyle Windows Server 2012 veya daha büyük çalışan bir sunucunun [Azure AD Connect sağlama aracısını](https://go.microsoft.com/fwlink/?linkid=847801)barındırması gerekir.
- Kullanıcıları Active Directory ve Azure AD arasında eşitlemek için [Azure AD Connect.](../hybrid/whatis-azure-ad-connect.md)

### <a name="training-resources"></a>Eğitim kaynakları

| **Kaynaklar** | **Bağlantı ve açıklama** |
|:-|:-|
| Videolar | [Active Azure Directory'de kullanıcı sağlama nedir?](https://youtu.be/_ZjARPpI6NI) |
| | [Etkin Azure Dizini'nde kullanıcı sağlama dağıtımı nasıl](https://youtu.be/pKzyts6kfrw) |
| Öğreticiler | [SaaS uygulamalarının Azure AD ile nasıl tümleştirileneilişkin öğreticiler listesi](../saas-apps/tutorial-list.md) |
| | [Öğretici: Otomatik kullanıcı sağlama için İş Gününü Yapılandır](../saas-apps/workday-inbound-tutorial.md#frequently-asked-questions-faq) |
| SSS | [Otomatik kullanıcı sağlama](../app-provisioning/user-provisioning.md#what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning) |
| | [İş Günü'nden Azure AD'ye sağlama](../saas-apps/workday-inbound-tutorial.md#frequently-asked-questions-faq) |

### <a name="solution-architecture"></a>Çözüm mimarisi

Aşağıdaki örnekte, ortak karma ortamlar için uçtan uca kullanıcı sağlama çözüm mimarisi açıklanır ve şunları içerir:

- **Bulut İk uygulamasından Active Directory'ye yetkili İk veri akışı.** Bu akışta, Bulut İk uygulaması kiracısında İk etkinliği (Joiners-Movers-Leavers işlemi) başlatılır. Azure AD sağlama hizmeti ve Azure AD Connect sağlama aracısı, bulut İk uygulaması kiracısından Etkin Dizin'e kullanıcı verilerini sağlar. Olaya bağlı olarak, Etkin Dizini'nde işlemleri oluşturmaya, güncelleştirmeye, etkinleştirmeye ve devre dışı etmeye neden olabilir.
- **Azure AD ile senkronize edin ve şirket içi Active Directory'den bulut İk uygulamasına e-posta ve kullanıcı adı yazın.** Hesaplar Active Directory'de güncelleştirildikten sonra Azure AD Connect aracılığıyla Azure AD ile senkronize edilir. E-posta adresleri ve kullanıcı adı öznitelikleri bulut İk uygulaması kiracısına geri yazılabilir.

![İş akışı diyagramı](media/plan-cloud-hr-provision/plan-cloudhr-provisioning-img1.png)

#### <a name="description-of-workflow"></a>İş akışının açıklaması

Aşağıdaki önemli adımlar diyagramda gösterilir:  

1. **İk ekibi** bulut İk uygulaması kiracı işlemleri gerçekleştirir.
2. **Azure AD sağlama hizmeti,** bulut İk uygulaması kiracısından zamanlanan döngüleri çalıştırır ve Active Directory ile eşitlenmesi için işlenmesi gereken değişiklikleri tanımlar.
3. **Azure AD sağlama hizmeti,** Etkin Dizin hesabı oluşturma, güncelleştirme, etkinleştirme ve devre dışı etme işlemlerini içeren bir istek yüküyle Azure AD Connect sağlama aracısını çağırır.
4. **Azure AD Connect sağlama aracısı,** Active Directory hesap verilerini yönetmek için bir hizmet hesabı kullanır.
5. **Azure AD Connect,** Etkin Dizini'nde güncelleştirmeleri çekmek için delta [eşitleme](../hybrid/how-to-connect-sync-whatis.md) çalıştırın.
6. **Etkin Dizin** güncelleştirmeleri Azure AD ile senkronize edilir.
7. **Azure AD sağlama hizmeti,** Azure AD'den bulut İk uygulaması kiracısına e-posta özniteliğini ve kullanıcı adını geri verir.

## <a name="plan-the-deployment-project"></a>Dağıtım projesini planlama

Ortamınızda bu dağıtım için strateji belirlerken kuruluş ihtiyaçlarınızı göz önünde bulundurun.

### <a name="engage-the-right-stakeholders"></a>Doğru paydaşlarla etkileşimde bulunun

Teknoloji projeleri başarısız olduğunda, genellikle etki, sonuçlar ve sorumluluklar hakkındaki uyumsuz beklentiler nedeniyle bunu yaparlar. Bu tuzaklardan kaçınmak için, [doğru paydaşlarla etkileşimde bulunduğunuzdan emin olun.](https://aka.ms/deploymentplans) Ayrıca, projedeki paydaş rollerinin iyi anlaşıldığından emin olun. Paydaşları ve proje girişlerini ve hesap yeteneklerini belgele.

İk kuruluşundan, mevcut İk iş süreçleri ve işçi kimliği artı iş veri işleme gereksinimleri ne kadar girdi sağlayabilir bir temsilci ekleyin.

### <a name="plan-communications"></a>İletişimi planlama

İletişim, herhangi bir yeni hizmetin başarısı için çok önemlidir. Deneyimlerinin ne zaman ve nasıl değişeceği hakkında kullanıcılarınızla proaktif olarak iletişim kurun. Sorunlarla karşılaşırsanız nasıl destek kazanacaklarını bildirin.

### <a name="plan-a-pilot"></a>Bir pilot planlayın

Bulut İk uygulamasından İk iş süreçlerini ve kimlik iş akışlarını hedef sistemlere entegre etmek, çözümü üretime dağıtmadan önce önemli miktarda veri doğrulama, veri dönüşümü, veri temizleme ve uçtan uca test gerektirir.

Üretimdeki tüm kullanıcılara ölçeklendirmeden önce ilk yapılandırmayı [pilot ortamda](../fundamentals/active-directory-deployment-plans.md#best-practices-for-a-pilot) çalıştırın.

## <a name="select-cloud-hr-provisioning-connector-apps"></a>Bulut İk sağlama bağlayıcısı uygulamalarını seçin

Azure AD sağlama iş akışlarını bulut İk uygulaması ve Active Directory arasında kolaylaştırmak için, Azure AD uygulama galerisinden birden çok sağlama bağlayıcısı uygulaması ekleyebilirsiniz:

- **Active Directory kullanıcı sağlama bulut İk uygulaması**: Bu sağlama bağlayıcı uygulaması, bulut İk uygulamasından tek bir Active Directory etki alanına kullanıcı hesabı sağlamasını kolaylaştırır. Birden çok etki alanınız varsa, sağlamanız gereken her Active Directory etki alanı için Azure AD uygulama galerisinden bu uygulamanın bir örneğini ekleyebilirsiniz.
- **Azure AD kullanıcı sağlama sına Bulut İk uygulaması**: Azure AD Connect, Active Directory kullanıcılarını Azure AD ile senkronize etmek için kullanılması gereken bir araç olsa da, bu sağlama bağlayıcısı uygulaması bulut İk uygulamasından yalnızca bulut KULLANıCıLARının tek bir Azure AD kiracısına sağlanmasını kolaylaştırmak için kullanılabilir.
- **Cloud HR uygulaması nın geri yazılması**: Bu sağlama bağlayıcısı uygulaması, kullanıcının e-posta adreslerinin Azure AD'den bulut İk uygulamasına geri yazılmasını kolaylaştırır.

Örneğin, aşağıdaki resim, Azure AD uygulama galerisinde bulunan İş Günü bağlayıcı uygulamalarını listeler.

![Azure Active Directory portal uygulaması galerisi](media/plan-cloud-hr-provision/plan-cloudhr-provisioning-img2.png)

### <a name="decision-flow-chart"></a>Karar akış şeması

Hangi bulut İk sağlama uygulamalarının senaryonuzla alakalı olduğunu belirlemek için aşağıdaki karar akış şemasını kullanın.

![Karar akış şeması](media/plan-cloud-hr-provision/plan-cloudhr-provisioning-img3.png)

## <a name="design-the-azure-ad-connect-provisioning-agent-deployment-topology"></a>Azure AD Connect sağlama aracıdağıtım topolojisini tasarlayın

Bulut İk uygulaması ile Active Directory arasındaki sağlama tümleştirmesi dört bileşen gerektirir:

- Bulut İk uygulaması kiracı
- Konektör uygulaması sağlama
- Azure AD Connect sağlama aracısı
- Active Directory etki alanı

Azure AD Connect sağlama aracısı dağıtım topolojisi, entegre etmeyi planladığınız bulut İk uygulaması kiracılarının ve Active Directory alt etki alanlarının sayısına bağlıdır. Birden çok Active Directory etki alanınız varsa, bu Etkin Dizin etki alanlarının bitişik veya [ayrı kıvranan](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/disjoint-namespace)olmasına bağlıdır.

Kararınızı temel alan, dağıtım senaryolarından birini seçin:

- Tek bulut İk uygulaması kiracı -> güvenilir bir ormanda tek veya birden çok Active Directory alt etki alanı hedef
- Tek bulut İk uygulaması kiracı -> ayrı bir Active Directory ormanda birden fazla alt etki hedefliyoruz

### <a name="single-cloud-hr-app-tenant---target-single-or-multiple-active-directory-child-domains-in-a-trusted-forest"></a>Tek bulut İk uygulaması kiracı -> güvenilir bir ormanda tek veya birden çok Active Directory alt etki alanı hedef

Aşağıdaki üretim yapılandırmasını öneririz:

|Gereksinim|Öneri|
|:-|:-|
|Dağıtılanacak Azure AD Connect sağlama aracılarının sayısı|İki (yüksek kullanılabilirlik ve başarısızlık için)
|Yapılandırmak için sağlayan bağlayıcı uygulama sayısı|Alt etki alanı başına bir uygulama|
|Azure AD Connect sağlama aracısı için sunucu ana bilgisayar|Windows 2012 R2+ coğrafi konumlu Active Directory etki alanı denetleyicilerine görüş hattı ile</br>Azure AD Connect hizmetiyle birlikte olabilir|

![Şirket içi acentelere akış](media/plan-cloud-hr-provision/plan-cloudhr-provisioning-img4.png)

### <a name="single-cloud-hr-app-tenant---target-multiple-child-domains-in-a-disjoint-active-directory-forest"></a>Tek bulut İk uygulaması kiracı -> ayrı bir Active Directory ormanda birden fazla alt etki hedefliyoruz

Bu senaryo, kullanıcıları bulut İk uygulamasından ayrı aktif dizin ormanlarında etki alanına sağlamayı içerir.

Aşağıdaki üretim yapılandırmasını öneririz:

|Gereksinim|Öneri|
|:-|:-|
|Şirket içinde dağıtılabilmek için Azure AD Connect sağlama aracılarının sayısı|Ayrık Aktif Dizin ormanı başına iki|
|Yapılandırmak için sağlayan bağlayıcı uygulama sayısı|Alt etki alanı başına bir uygulama|
|Azure AD Connect sağlama aracısı için sunucu ana bilgisayar|Windows 2012 R2+ coğrafi konumlu Active Directory etki alanı denetleyicilerine görüş hattı ile</br>Azure AD Connect hizmetiyle birlikte olabilir|

![Tek bulut İk uygulaması kiracı ayrık Aktif Dizin orman](media/plan-cloud-hr-provision/plan-cloudhr-provisioning-img5.png)

### <a name="azure-ad-connect-provisioning-agent-requirements"></a>Azure AD Connect sağlama aracıgereksinimleri

Active Directory kullanıcı sağlama çözümüne bulut İk uygulaması, Windows 2012 R2 veya daha büyük çalıştıran sunucularda bir veya daha fazla Azure AD Connect sağlama aracısı dağıtmanızı gerektirir. Sunucularda en az 4 GB RAM ve .NET 4.7.1+ çalışma süresi olmalıdır. Ana bilgisayar sunucusunun hedef Active Directory etki alanına ağ erişimi olduğundan emin olun.

Şirket içi ortamı hazırlamak için Azure AD Connect sağlama aracısı yapılandırma sihirbazı aracıyı Azure AD kiracınıza kaydeder, [bağlantı noktalarını açar,](../manage-apps/application-proxy-add-on-premises-application.md#open-ports) [URL'lere erişimsağlar](../manage-apps/application-proxy-add-on-premises-application.md#allow-access-to-urls)ve [giden HTTPS proxy yapılandırmasını](../saas-apps/workday-inbound-tutorial.md#how-do-i-configure-the-provisioning-agent-to-use-a-proxy-server-for-outbound-http-communication)destekler.

Sağlama aracısı, Etkin Dizin etki alanlarıyla iletişim kurmak için bir hizmet hesabı kullanır. Aracıyı yüklemeden önce, Etkin Dizin Kullanıcıları ve Bilgisayarlarında aşağıdaki gereksinimleri karşılayan bir hizmet hesabı oluşturun:

- Süresi dolmayan bir parola
- Kullanıcı hesaplarını okumak, oluşturmak, silmek ve yönetmek için yetkin denetim izinleri

Sağlama isteklerini işlemesi gereken etki alanı denetleyicilerini seçebilirsiniz. Coğrafi olarak dağıtılmış birkaç etki alanı denetleyiciniz varsa, sağlama aracısını tercih ettiğiniz etki alanı denetleyicileri ile aynı siteye yükleyin. Bu konumlandırma, uçtan uca çözümün güvenilirliğini ve performansını artırır.

Yüksek kullanılabilirlik için birden fazla Azure AD Bağlantısı sağlama aracısı dağıtabilirsiniz. Aynı şirket içi Active Directory etki alanlarını işlemek için aracıyı kaydedin.

## <a name="plan-scoping-filters-and-attribute-mapping"></a>Kapsam filtreleri ve öznitelik eşleme planı

Bulut İk uygulamasından Active Directory veya Azure AD'ye sağlama olanağı sağladığınızı, Azure portalı öznitelik eşleme yoluyla öznitelik değerlerini denetler.

### <a name="define-scoping-filters"></a>Kapsam filtrelerini tanımlama

Bulut İk uygulamasından Active Directory veya Azure AD'ye hangi kullanıcıların sağlanması gerektiğini belirleyen öznitelik tabanlı kuralları tanımlamak için [kapsam filtreleri](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) kullanın.

Birleştiriciler işlemini başlattığınızda, aşağıdaki gereksinimleri toplayın:

- Bulut İk uygulaması hem çalışanları hem de şartlı çalışanları gemiye getirmek için kullanılıyor mu?
- Bulut İk uygulamasını hem çalışanları hem de şartlı çalışanları yönetmek için Azure AD kullanıcı sağlama için kullanmayı planlıyor musunuz?
- Bulut İk uygulamasını Azure AD kullanıcı sağlama uygulamasına yalnızca bulut İk uygulaması kullanıcılarının bir alt kümesi için sunmayı planlıyor musunuz? Bir örnek yalnızca çalışanlar olabilir.

Gereksinimlerinize bağlı olarak, öznitelik eşlemelerini yapılandırdığınızda, **Kaynak Nesne Kapsamı** alanını, Bulut İk uygulamasındaki hangi kullanıcı kümelerinin Active Directory'ye sağlama kapsamında olması gerektiğini seçecek şekilde ayarlayabilirsiniz. Daha fazla bilgi için, yaygın olarak kullanılan kapsam kapsam filtreleri için bulut İk uygulaması öğreticisine bakın.

### <a name="determine-matching-attributes"></a>Eşleşen öznitelikleri belirleme

Sağlama ile, kaynak ve hedef sistem arasındaki varolan hesapları eşleştirme olanağı elde elabilirsiniz. Bulut İk uygulamasını Azure AD sağlama hizmetiyle tümleştirdiğinizde, bulut İk uygulamasından Active Directory veya Azure AD'ye hangi kullanıcı verilerinin akması gerektiğini belirlemek için [öznitelik eşlemesi yapılandırabilirsiniz.](../app-provisioning/configure-automatic-user-provisioning-portal.md#mappings)

Birleştiriciler işlemini başlattığınızda, aşağıdaki gereksinimleri toplayın:

- Bu bulut İk uygulamasındaki benzersiz kimlik nedir?
- Kimlik yaşam döngüsü açısından, yeniden işe alımlarla nasıl başa çıkarsınız? Yeniden işe alınanlar eski çalışan kimliklerini saklar mı?
- Gelecekteki işe alma işlemlerini işleyip onlar için Active Directory hesaplarını önceden mi oluşturuyorsunuz?
- Kimlik yaşam döngüsü açısından bakıldığında, çalışandan şartlı işçi dönüşümüne veya başka bir şekilde nasıl başa çıkarabilirsiniz?
- Dönüştürülmüş kullanıcılar eski Active Directory hesaplarını tutuyor mu yoksa yenilerini mi alacaklar?

Gereksinimlerinize bağlı olarak, Azure AD sabit değerler sağlayarak veya [öznitelik eşlemeleri için ifadeler yazarak](../app-provisioning/functions-for-customizing-application-data.md)doğrudan öznitelik-öznitelik eşleme destekler. Bu esneklik, hedeflenen uygulama özniteliğinde nelerin doldurulan ne olduğunu en iyi şekilde kontrol eşimdi. Microsoft Graph [API](../app-provisioning/export-import-provisioning-configuration.md) ve Graph Explorer'ı kullanarak kullanıcı sağlama öznitelik eşlemelerinizi ve şemasını bir JSON dosyasına dışa aktarabilir ve Azure AD'ye geri aktarabilirsiniz.

Varsayılan olarak, benzersiz çalışan kimliğini temsil eden bulut İk uygulamasındaki öznitelik, *Active Directory'deki benzersiz özniteliğe eşlenen* eşleşen öznitelik olarak kullanılır. Örneğin, İş Günü uygulaması senaryosunda, **İş Günü** **İşçi Kimliği** özniteliği Active Directory **employeeID** özniteliğine eşlenir.

Birden çok eşleşen öznitelik ayarlayabilir ve eşleşen öncelik atayabilirsiniz. Eşleşen öncelikte değerlendirilirler. Eşleşme bulunur bulunmaz, başka eşleşen öznitelikler değerlendirilmez.

Varolan [öznitelik eşlemelerini](../app-provisioning/customize-application-attributes.md#understanding-attribute-mapping-types)değiştirme veya silme gibi varsayılan öznitelik eşlemelerini de özelleştirebilirsiniz. Ayrıca, işletme gereksinimlerinize göre yeni öznitelik eşlemeleri de oluşturabilirsiniz. Daha fazla bilgi için, haritaya özel özniteliklerin listesi için bulut İk uygulaması öğreticisine [(İş Günü](../saas-apps/workday-inbound-tutorial.md#planning-workday-to-active-directory-user-attribute-mapping-and-transformations)gibi) bakın.

### <a name="determine-user-account-status"></a>Kullanıcı hesabı durumunu belirleme

Varsayılan olarak, sağlama bağlayıcısı uygulaması, kullanıcı hesabını etkinleştirip etkinleştirmeyeceğini veya devre dışı edip etmeyeceğini belirlemek için İk kullanıcı profili durumunu Active Directory veya Azure AD'deki kullanıcı hesabı durumuna eşler.

Joiners-Leavers işlemini başlattığınızda, aşağıdaki gereksinimleri toplayın.

| İşleme | Gereksinimler |
| - | - |
| **Doğrama** | Kimlik yaşam döngüsü açısından, yeniden işe alımlarla nasıl başa çıkarsınız? Yeniden işe alınanlar eski çalışan kimliklerini saklar mı? |
| | Gelecekteki işe alma işlemlerini işleyip onlar için Active Directory hesaplarını önceden mi oluşturuyorsunuz? Bu hesaplar etkin veya devre dışı bırakılmış durumda mı oluşturuldu? |
| | Kimlik yaşam döngüsü açısından bakıldığında, çalışandan şartlı işçi dönüşümüne veya başka bir şekilde nasıl başa çıkarabilirsiniz? |
| | Dönüştürülmüş kullanıcılar eski Active Directory hesaplarını mı tutuyor, yoksa yenilerini mi alacaklar? |
| **Ayrılanlar** | Fesihler, Active Directory'deki çalışanlar ve şartlı çalışanlar için farklı mı ele alınır? |
| | Kullanıcı sonlandırma işleminin işlenmesi için hangi etkili tarihler dikkate alınır? |
| | Çalışan ve bağlı çalışan dönüşümleri varolan Active Directory hesaplarını nasıl etkiler? |
| | Active Directory'deki Rescind işlemini nasıl işliyorsunuz? Joiner işleminin bir parçası olarak Active Directory'de gelecekteki tarihli işe alımlar oluşturulacaksa, yeniden yapılan işlemlerin ele alınması gerekir. |

Gereksinimlerinize bağlı olarak, Etkin Dizin hesabının veri noktalarının birleşimine bağlı olarak etkin olması veya devre dışı bırakılmış olması için [Azure AD ifadelerini](../app-provisioning/functions-for-customizing-application-data.md) kullanarak eşleme mantığını özelleştirebilirsiniz.

### <a name="map-cloud-hr-app-to-active-directory-user-attributes"></a>Active Directory kullanıcı özniteliklerine bulut İk uygulaması

Her bulut İk uygulaması, varsayılan bulut İk uygulamasıyla Active Directory haritalamalarına iletir.

Joiners-Movers-Leavers işlemini başlattığınızda, aşağıdaki gereksinimleri toplayın.

| İşleme | Gereksinimler |
| - | - |
| **Doğrama** | Active Directory hesap oluşturma işlemi kılavuzu otomatik mi yoksa kısmen otomatik mi? |
| | Bulut İk uygulamasından Active Directory'ye özel öznitelikleri yaymayı planlıyor musunuz? |
| **Movers** | Bulut İk uygulamasında bir Movers işlemi gerçekleştiğinde hangi özellikleri işlemek istersiniz? |
| | Kullanıcı güncelleştirmeleri sırasında belirli bir öznitelik doğrulaması gerçeklebiliyor musunuz? Evet ise, ayrıntıları sağlayın. |
| **Ayrılanlar** | Fesihler, Active Directory'deki çalışanlar ve şartlı çalışanlar için farklı mı ele alınır? |
| | Kullanıcı sonlandırma işleminin işlenmesi için hangi etkili tarihler dikkate alınır? |
| | Çalışan ve bağlı çalışan dönüşümleri varolan Active Directory hesaplarını nasıl etkiler? |

Gereksinimlerinize bağlı olarak, eşlemeleri tümleştirme hedeflerinize ulaşmak için değiştirebilirsiniz. Daha fazla bilgi için, haritaya özel özniteliklerin listesi için belirli bulut İk uygulaması öğreticisine [(İş Günü](../saas-apps/workday-inbound-tutorial.md#planning-workday-to-active-directory-user-attribute-mapping-and-transformations)gibi) bakın.

### <a name="generate-a-unique-attribute-value"></a>Benzersiz bir öznitelik değeri oluşturma

Birleştiriciler işlemini başlattığınızda, CN, samAccountName ve benzersiz kısıtlamaları olan UPN gibi öznitelikleri ayarladığınızda benzersiz öznitelik değerleri oluşturmanız gerekebilir.

Azure AD işlevi [SelectUniqueValues](../app-provisioning/functions-for-customizing-application-data.md#selectuniquevalue) her kuralı değerlendirir ve ardından hedef sistemde benzersizlik için oluşturulan değeri denetler. Örneğin, bkz. [userPrincipalName (UPN) özniteliği için benzersiz değer oluştur.](../app-provisioning/functions-for-customizing-application-data.md#generate-unique-value-for-userprincipalname-upn-attribute)

> [!NOTE]
> Bu işlev şu anda yalnızca Active Directory kullanıcı sağlama Için İş Günü için desteklenir. Diğer sağlama uygulamalarıyla kullanılamaz.

### <a name="configure-active-directory-ou-container-assignment"></a>Etkin Dizin OU konteyner ataması yapılandır

Active Directory kullanıcı hesaplarını iş birimlerine, konumlara ve departmanlara göre kapsayıcılara yerleştirmek yaygın bir gereksinimdir. Bir Movers işlemini başlattığınızda ve denetleyici bir kuruluş değişikliği varsa, kullanıcıyı Active Directory'de bir OU'dan diğerine taşımanız gerekebilir.

OU ataması için iş mantığını yapılandırmak için [Switch()](../app-provisioning/functions-for-customizing-application-data.md#switch) işlevini kullanın ve active Directory özniteliği **üst öğesiDistinguishedName**ile eşlenin.

Örneğin, İk özniteliği **Belediyesi'ne**göre OU'da kullanıcı oluşturmak istiyorsanız, aşağıdaki ifadeyi kullanabilirsiniz:

`
Switch([Municipality], "OU=Default,OU=Users,DC=contoso,DC=com", "Dallas", "OU=Dallas,OU=Users,DC=contoso,DC=com", "Austin", "OU=Austin,OU=Users,DC=contoso,DC=com", "Seattle", "OU=Seattle,OU=Users,DC=contoso,DC=com", "London", "OU=London,OU=Users,DC=contoso,DC=com")
`

Bu ifadeyle, Belediye değeri Dallas, Austin, Seattle veya Londra ise, kullanıcı hesabı ilgili OU'da oluşturulur. Eşleşme yoksa, hesap varsayılan OU'da oluşturulur.

## <a name="plan-for-password-delivery-of-new-user-accounts"></a>Yeni kullanıcı hesaplarının parola teslimi için plan

Birebir işlemi başlattığınızda, yeni kullanıcı hesaplarının geçici bir parolasını ayarlamanız ve teslim etmeniz gerekir. Bulut İk'dan Azure AD kullanıcı sağlamasına kadar, kullanıcı için azure AD [self servis parola sıfırlama](../authentication/quickstart-sspr.md) (SSPR) özelliğini ilk gün kullanıma sunabilirsiniz.

SSPR, BT yöneticilerinin kullanıcıların parolalarını sıfırlamalarını veya hesaplarının kilidini açmalarını sağlamak için basit bir araçtır. **Mobil Numara** özniteliğini bulut İk uygulamasından Active Directory'ye sağlayabilir ve Azure AD ile senkronize edebilirsiniz. Mobil **Numara** özniteliği Azure AD'de bulunduktan sonra, kullanıcının hesabı için SSPR'yi etkinleştirebilirsiniz. Ardından ilk gün, yeni kullanıcı kimlik doğrulaması için kayıtlı ve doğrulanmış cep telefonu numarasını kullanabilir.

## <a name="plan-for-initial-cycle"></a>İlk döngü için plan

Azure AD sağlama hizmeti ilk kez çalıştığında, bulut İk uygulamasındaki tüm kullanıcı nesnelerinin anlık görüntüsünü oluşturmak için bulut İk uygulamasına karşı bir [başlangıç döngüsü](../app-provisioning/how-provisioning-works.md#initial-cycle) gerçekleştirir. İlk döngüler için alınan süre doğrudan kaynak sistemde kaç kullanıcının bulunduğuna bağlıdır. 100.000'den fazla kullanıcısı olan bazı bulut İk uygulaması kiracılarının ilk döngüsü uzun sürebilir.

**Büyük bulut İk uygulaması kiracıları (30.000 kullanıcı >) için,** ilk döngüyü aşamalı aşamalarda çalıştırın. Artımlı güncelleştirmeleri yalnızca doğru özniteliklerin farklı kullanıcı sağlama senaryoları için Etkin Dizin'de ayarlı olduğunu doğruladıktan sonra başlatın. Siparişi buradan takip edin.

1. İlk döngüyü yalnızca sınırlı sayıda kullanıcı için [kapsam filtresini](#plan-scoping-filters-and-attribute-mapping)ayarlayarak çalıştırın.
2. Etkin Dizin hesabı sağlama ve ilk çalıştırma için seçilen kullanıcılar için ayarlanan öznitelik değerlerini doğrulayın. Sonuç beklentilerinizi karşılıyorsa, kapsama filtresini giderek daha fazla kullanıcı içerecek şekilde genişletin ve ikinci çalıştırmanın sonuçlarını doğrulayın.

Test kullanıcıları için ilk döngünün sonuçlarından memnun olduktan sonra, [artımlı güncelleştirmeleri](../app-provisioning/how-provisioning-works.md#incremental-cycles)başlatın.

## <a name="plan-testing-and-security"></a>Test ve güvenlik planı

Kullanıcı sağlamayı etkinleştirerek ilk pilottan dağıtımınızın her aşamasında, sonuçların beklendiği gibi test ettiğinizden ve sağlama döngülerini denetlediğinizden emin olun.

### <a name="plan-testing"></a>Planı testi

Bulut İk uygulamasını Azure AD kullanıcı sağlama ile yapılandırdıktan sonra, bu çözümün kuruluşunuzun gereksinimlerini karşılayıp karşılamadığını doğrulamak için test servis taleplerini çalıştırın.

|Senaryolar|Beklenen sonuçlar|
|:-|:-|
|Bulut İk uygulamasında yeni çalışan işe alınır.| - Kullanıcı hesabı Active Directory'de hazırdır.</br>- Kullanıcı Active Directory-etki alanı uygulamalarına giriş yapabilir ve istenen eylemleri gerçekleştirebilir.</br>- Azure AD Connect eşitlemi yapılandırılırsa, kullanıcı hesabı Azure AD'de de oluşturulur.
|Bulut İk uygulamasında kullanıcı sonlandırılır.|- Kullanıcı hesabı Active Directory'de devre dışı bırakılır.</br>- Kullanıcı Active Directory tarafından korunan hiçbir kurumsal uygulamaya giriş yapamaz.
|Kullanıcı denetim kuruluşu bulut İk uygulamasında güncellenir.|Öznitelik eşleme özelliğine bağlı olarak, kullanıcı hesabı Active Directory'de bir OU'dan diğerine geçer.|
|İk, bulut İk uygulamasında kullanıcının yöneticisini güncelleştirir.|Active Directory'deki yönetici alanı yeni yöneticinin adını yansıtacak şekilde güncelleştirilir.|
|İk yeni bir rol içine bir çalışanı yeniden işe alır.|Davranış, bulut İk uygulamasının çalışan t.c. oluşturmak üzere nasıl yapılandırıldığına bağlıdır:</br>- Eski çalışan kimliği yeniden işe almak için yeniden kullanılırsa, bağlayıcı kullanıcı için varolan Active Directory hesabını etkinleştirir.</br>- Yeniden işe alma yeni bir çalışan kimliği alırsa, bağlayıcı kullanıcı için yeni bir Active Directory hesabı oluşturur.|
|İk, işçiyi sözleşmeli bir işçiye dönüştürür veya tam tersi.|Yeni persona için yeni bir Active Directory hesabı oluşturulur ve eski hesap dönüşüm geçerlilik tarihinde devre dışı bırakılır.|

Otomatik kullanıcı sağlama uygulamanızı, belirlediğiniz zaman çizelgelerine göre üretime nasıl dönüştürerinizi belirlemek için önceki sonuçları kullanın.

> [!TIP]
> Gizlilik ve güvenlik standartlarına uymak için hassas kişisel verileri kaldırmak veya maskelemek için test ortamını üretim verileriyle yenilerken veri azaltma ve veri temizleme gibi teknikler kullanın. 

### <a name="plan-security"></a>Güvenlik planı

Yeni bir hizmetin dağıtımının bir parçası olarak bir güvenlik incelemesinin gerekli olması yaygındır. Bir güvenlik incelemesi gerekiyorsa veya yapılmadıysa, hizmet olarak kimliğin genel görünümünü sağlayan birçok Azure AD [teknik incelemesine](https://www.microsoft.com/download/details.aspx?id=36391) bakın.

### <a name="plan-rollback"></a>Geri alma planı

Bulut İk kullanıcı sağlama uygulaması, üretim ortamında istenildiği gibi çalışmayabilir. Bu durumda, aşağıdaki geri alma adımları bilinen iyi duruma geri dönmenize yardımcı olabilir.

1. Etkilenen kullanıcılar veya gruplar üzerinde hangi yanlış işlemlerin gerçekleştirildiğini belirlemek için [sağlama özet raporunu](../app-provisioning/check-status-user-account-provisioning.md#getting-provisioning-reports-from-the-azure-portal) ve sağlama [günlüklerini](../app-provisioning/check-status-user-account-provisioning.md#provisioning-logs-preview) gözden geçirin. Özet raporu ve günlükleri hakkında daha fazla bilgi için bulut [İk uygulamasını yönet'e](#manage-your-configuration)bakın.
2. Etkilenen kullanıcıların veya grupların bilinen son iyi durumu, denetim günlükleri sağlama yoluyla veya hedef sistemleri (Azure AD veya Active Directory) gözden geçirerek belirlenebilir.
3. Bilinen son iyi durum değerlerini kullanarak doğrudan uygulamada etkilenen kullanıcıları veya grupları güncellemek için uygulama sahibiyle birlikte çalışın.

## <a name="deploy-the-cloud-hr-app"></a>Bulut İk uygulamasını dağıtma

Çözüm gereksinimlerinize uygun bulut İk uygulamasını seçin.

**İş Günü**: İş Günü'nden Etkin Dizin ve Azure AD'ye işçi profillerini almak [için](../saas-apps/workday-inbound-tutorial.md#planning-your-deployment)bkz. İsteğe bağlı olarak, E-posta adresini ve kullanıcı adını İş Günü'ne geri yazabilirsiniz.

## <a name="manage-your-configuration"></a>Yapılandırmanızı yönetme

Azure AD, denetim günlükleri ve raporlar aracılığıyla kuruluşunuzun kullanıcı sağlama kullanımı ve operasyonel durumu hakkında ek bilgiler sağlayabilir.

### <a name="gain-insights-from-reports-and-logs"></a>Raporlardan ve günlüklerden öngörüler elde edin

Başarılı bir [ilk döngüden](../app-provisioning/how-provisioning-works.md#initial-cycle)sonra, Azure AD sağlama hizmeti, aşağıdaki olaylardan biri meydana gelene kadar, her uygulamaya özgü öğreticilerde tanımlanan aralıklarla, arka arkaya artımlı güncelleştirmeleri süresiz olarak çalıştırmaya devam eder:

- Hizmet el ile durdurulur. [Azure portalı](https://portal.azure.com/) veya uygun Microsoft [Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview) komutu kullanılarak yeni bir başlangıç döngüsü tetiklenir.
- Öznitelik eşlemeleri veya kapsam filtreleri bir değişiklik nedeniyle yeni bir başlangıç döngüsü tetiklenir.
- Sağlama işlemi, yüksek hata oranı nedeniyle karantinaya alınır. Dört haftadan fazla karantinada kalır ve bu süre otomatik olarak devre dışı bırakılır.

Bu olayları ve sağlama hizmeti tarafından gerçekleştirilen diğer tüm etkinlikleri gözden geçirmek için, [günlükleri nasıl inceleyip etkinlik sağlama yla ilgili raporlar almayı öğrenin.](../app-provisioning/check-status-user-account-provisioning.md)

#### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

Sağlama hizmeti tarafından gerçekleştirilen tüm etkinlikler Azure AD denetim günlüklerine kaydedilir. Daha fazla analiz için Azure AD denetim günlüklerini Azure Monitor günlüklerine yönlendirebilirsiniz. Azure Monitor günlükleri (Günlük Analizi çalışma alanı olarak da bilinir) ile, olayları bulmak, eğilimleri analiz etmek ve çeşitli veri kaynakları arasında korelasyon gerçekleştirmek için verileri sorgulayabilirsiniz. Pratik kullanıcı senaryolarında Azure AD günlükleri için Azure Monitor günlüklerini kullanmanın avantajlarını öğrenmek için bu [videoyu](https://youtu.be/MP5IaCTwkQg) izleyin.

Ortamınızdaki olayları sağlama yla ilgili [önceden oluşturulmuş raporlara](https://github.com/AzureAD/Deployment-Plans/tree/master/Log%20Analytics%20Views) erişmek [için Azure AD etkinlik günlüklerinin günlük analizi görünümlerini](../reports-monitoring/howto-install-use-log-analytics-views.md) yükleyin.

Daha fazla bilgi için Azure [Monitor günlüklerinizle Azure REKLAM etkinlik günlüklerini](../reports-monitoring/howto-analyze-activity-logs-log-analytics.md)nasıl analiz edin.

### <a name="manage-personal-data"></a>Kişisel verileri yönetme

Windows sunucusuna yüklenen Azure AD Connect sağlama aracısı, Windows etkinlik günlüğünde, bulut İk uygulamanıza bağlı olarak Active Directory öznitelik eşlemelerine bağlı olarak kişisel veriler içerebilecek günlükler oluşturur. Kullanıcı gizliliği yükümlülüklerine uymak için, olay günlüğünü temizlemek ve hiçbir verinin 48 saatten fazla tutulmamasını sağlamak için windows zamanlanmış bir görev ayarlayın.

Azure AD sağlama hizmeti, herhangi bir veriyi 30 günden fazla depolamadığı, işlemediği veya tutmadığı için raporlar oluşturmaz, analiz yapmaz veya 30 günden fazla öngörü sağlamaz.

### <a name="troubleshoot"></a>Sorun giderme

Sağlama sırasında ortaya çıkan sorunları gidermek için aşağıdaki makalelere bakın:

- [Kullanıcı sağlamayı azure AD Galerisi uygulamasına yapılandırma sorunu](application-provisioning-config-problem.md)
- [Bir uygulamayı sağlamak için şirket içi Active Directory'nizden Azure AD'ye bir özniteliği senkronize etme](user-provisioning-sync-attributes-for-mapping.md)
- [Azure AD Galerisi uygulamasına kullanıcı sağlama süresi birkaç saat veya daha uzun sürüyor](application-provisioning-when-will-provisioning-finish.md)
- [Kullanıcı sağlamayı Azure Etkin Dizin Galerisi uygulamasına yapılandırırken yönetici kimlik bilgilerini kaydetme sorunu](application-provisioning-config-problem-storage-limit.md)
- [Azure AD Galerisi uygulamasında hiçbir kullanıcı sağlanmadı](application-provisioning-config-problem-no-users-provisioned.md)
- [Azure AD Galerisi uygulamasına yanlış kullanıcı kümesi sağlanıyor](application-provisioning-config-problem-wrong-users-provisioned.md)
- [Aracı sorun giderme için Windows Event Viewer ayarlama](../saas-apps/workday-inbound-tutorial.md#setting-up-windows-event-viewer-for-agent-troubleshooting)
- [Hizmet sorun giderme için Azure portalı Denetim Günlükleri ayarlama](../saas-apps/workday-inbound-tutorial.md#setting-up-azure-portal-audit-logs-for-service-troubleshooting)
- [AD Kullanıcı Hesabı için günlükleri anlama işlemleri oluşturmak](../saas-apps/workday-inbound-tutorial.md#understanding-logs-for-ad-user-account-create-operations)
- [Yönetici güncelleştirme işlemleri için günlükleri anlama](../saas-apps/workday-inbound-tutorial.md#understanding-logs-for-manager-update-operations)
- [Sık karşılaşılan hataları çözme](../saas-apps/workday-inbound-tutorial.md#resolving-commonly-encountered-errors)

### <a name="next-steps"></a>Sonraki adımlar

- [Öznitelik eşlemeleri için ifadeler yazma](functions-for-customizing-application-data.md)
- [Azure AD senkronizasyon API'ye genel bakış](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
- [Kapsam dışına çıkan kullanıcı hesaplarının silinmesini atlama](skip-out-of-scope-deletions.md)
- [Azure AD Connect Provisioning Aracısı: Sürüm sürüm geçmişi](provisioning-agent-release-version-history.md)
