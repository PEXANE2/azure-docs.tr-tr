---
title: Kullanıcı sağlamayı Azure Active Directory için bulut HR uygulaması planlayın
description: Bu makalede, Azure Active Directory ile Workday ve Sucessfaktörleri gibi bulut HR sistemlerini tümleştirme işleminin dağıtım süreci açıklanmaktadır. Azure AD 'yi bulut HR sistemiyle tümleştirmek, bir kimlik yaşam döngüsü yönetim sistemiyle sonuçlanır.
services: active-directory
author: martincoetzer
manager: CelesteDG
tags: azuread
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 11/22/2019
ms.author: martinco
ms.reviewer: arvindha
ms.openlocfilehash: 5d55aafc29b3b022d1023077d2d8f459b0608ae7
ms.sourcegitcommit: 428fded8754fa58f20908487a81e2f278f75b5d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2019
ms.locfileid: "74555658"
---
# <a name="plan-cloud-hr-application-to-azure-active-directory-user-provisioning"></a>Kullanıcı sağlamayı Azure Active Directory için bulut HR uygulaması planlayın

Tarihsel BT personeli, CSV dosyalarını karşıya yükleme, güncelleştirme ve silme gibi çalışanları veya çalışan verilerinin eşitlenmesi için özel betikleri el ile oluşturma, güncelleştirme ve silme yöntemlerine güvendi. Bu sağlama işlemlerinde hata-açık, güvensiz ve yönetimi zor.

Çalışanların, satıcıların veya contentik çalışanların uçtan uca kimlik yaşam döngülerini sorunsuzca yönetmek için [Azure Active Directory (Azure AD) Kullanıcı sağlama hizmeti](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) , bulut tabanlı insan kaynakları (hr) uygulamalarıyla, Workday veya başarılı faktörlerle tümleştirme sağlar.

Azure AD, aşağıdaki bulut HR uygulaması (uygulama) iş akışlarını etkinleştirmek için bu tümleştirmeyi kullanır:

- **Kullanıcıları** , BIR bulut HR uygulamasından bir veya daha fazla ACTIVE DIRECTORY (ad) etki alanına bir veya daha fazla kullanıcı kümesine ad sağlama
- AD 'nin kullanılmayan senaryolarda **yalnızca bulutta bulunan kullanıcıların Azure AD 'de sağlanması** , kullanıcıların doğrudan bulut HR UYGULAMASıNDAN Azure AD 'ye sağlanması.
- **Bulut HR uygulamasına geri yazın.** -e-posta adreslerini ve Kullanıcı adı özniteliklerini Azure AD 'den Cloud HR uygulamasına geri yazın.

> [!NOTE]
> Bu dağıtım planı, Azure AD Kullanıcı sağlama ile bulut HR uygulaması iş akışlarınızı nasıl dağıtacağınızı rehberlik eder. SaaS uygulamalarına otomatik Kullanıcı sağlamayı dağıtma hakkında daha fazla bilgi için, [Otomatik Kullanıcı sağlama dağıtımı planı](https://aka.ms/deploymentplans/provisioning)' na bakın.

## <a name="enabled-hr-scenarios"></a>Etkin HR senaryoları

Azure AD Kullanıcı sağlama hizmeti, aşağıdaki HR tabanlı kimlik yaşam döngüsü yönetimi senaryolarına yönelik Otomasyonu sunar:

- **Yeni çalışanların işe** Alım-bulut HR uygulamasına yeni bir çalışan EKLENDIĞINDE, ad ve Azure AD 'de otomatik olarak bir kullanıcı hesabı oluşturulur ve e-posta adresi ve Kullanıcı adı ÖZNITELIKLERINI bulut HR uygulamasına geri yazma seçeneği sağlanır.
- **Çalışan özniteliği ve profil güncelleştirmeleri** -bir çalışan kaydı, bulut HR uygulamasında (ad, başlık veya yönetici gibi) güncelleştirildiğinde, Kullanıcı hesabı ad ve Azure AD 'de otomatik olarak güncelleştirilir.
- **Çalışan sonlandırmaları** -bulut HR uygulamasında bir çalışan SONLANDıRıLDıĞıNDA, ad ve Azure AD 'de Kullanıcı hesapları otomatik olarak devre dışı bırakılır.
- **Çalışan rehires** -bulut HR uygulamasında bir çalışan yeniden işe alındığında, eski hesapları otomatik olarak yeniden ETKINLEŞTIRILEBILIR veya ad ve Azure AD 'ye yeniden sağlanabilir.

## <a name="who-is-this-integration-best-suited-for"></a>Bu tümleştirme kim için en uygun?

Azure AD Kullanıcı sağlama ile bulut HR uygulaması tümleştirmesi ideal olan kuruluşlar için idealdir:

- bulut HR Kullanıcı sağlama için önceden oluşturulmuş, bulut tabanlı bir çözüm yapmanız gerekir
- bulut HR uygulamasından AD 'ye veya Azure AD 'ye doğrudan kullanıcı sağlamayı gerektir
- Kullanıcıların bulut HR uygulamasından elde edilen veriler kullanılarak sağlanması gerekir
- Kullanıcıların yalnızca bulut HR uygulamasında algılanan değişiklik bilgilerini temel alan bir veya daha fazla AD Ormanı, etki alanı ve kuruluş birimleri ile eşitlenmesi, taşınması ve bırakılması gerekir
- e-posta için Office 365 kullanın

## <a name="learn"></a>Öğrenme

Kullanıcı hazırlama, sürekli kimlik yönetimi için bir temel oluşturur ve yetkili kimlik verilerine dayanan iş işlemlerinin kalitesini geliştirir.

### <a name="terms"></a>Koşullar

Bu makale aşağıdaki terimleri kullanır:

- **Kaynak sistem** -Azure AD 'nin sunduğu kullanıcıların deposu (örneğin, Workday ve başarılı etmenler gibi BIR bulut İK uygulaması).
- **Hedef sistem** -Azure AD tarafından sağlanan (ÖRNEĞIN, ad, Azure AD, Office365 veya diğer SaaS uygulamaları) kullanıcıların deposu.
- **Birleştiriciler-Taşımacılar** -yük devretme süreci-BIR bulut HR uygulamasını bir kayıt sistemi olarak kullanarak yeni Hires, aktarımlar ve sonlandırma için kullanılan bir terimdir. Hizmet, hedef sisteme gerekli öznitelikleri başarıyla sağlarken işlem tamamlanır.

### <a name="key-benefits"></a>Önemli avantajlar

Bu HR-odaklı BT sağlama özelliği aşağıda listelenen önemli iş avantajları sağlar:

- **Üretkenliği artırın** -artık Kullanıcı hesapları, Office365 lisansları atamasını otomatikleştirebilir ve anahtar gruplarına erişim sağlayabilirsiniz. Atamaları otomatik hale getirmek, iş araçlarına yeni bir anında erişim sağlar ve üretkenliği artırır.
- **Risk yönetme** -bulut HR uygulamasından gelen verilerle akan çalışanların durum veya grup üyeliklerine göre değişiklikleri otomatikleştirerek güvenliği artırabilirsiniz. Değişikliklerin otomatikleştirilmesi, kullanıcılar kuruluştan geçiş yaparken veya kuruluştan ayrıldığınızda, anahtar uygulamalarına yönelik kullanıcı kimliklerinin ve erişimin otomatik olarak güncelleştirilmesini sağlar.
- **Adres uyumluluğu ve idare** -Azure AD, hem kaynak hem de hedef sistemlerin uygulamaları tarafından gerçekleştirilen Kullanıcı hazırlama istekleri için yerel denetim günlüklerini destekler. Denetim, uygulamalara tek bir ekrandan kimlerin erişebileceğini izlemenize olanak sağlar.
- **Maliyet** otomatik sağlamayı yönetme, el ile sağlama ile ilişkili verimsizlikleri ve insan hatasından kaçınarak maliyetleri azaltır. Eski ve süresi geçmiş platformlar kullanılarak zaman içinde oluşturulmuş özel geliştirmiş Kullanıcı sağlama çözümleri gereksinimini azaltır.

### <a name="licensing"></a>Lisanslama

Bulut HR uygulamasını Azure AD Kullanıcı sağlama tümleştirmesinde yapılandırmak için, geçerli bir [Azure AD Premium Lisansı](https://azure.microsoft.com/pricing/details/active-directory/) ve Workday veya başarılı FAKTÖRGIBI bulut HR uygulaması için bir lisans gerekir.

Ayrıca, bulut HR uygulamasından kaynaklayacağı ve AD ya da Azure AD 'ye sağlanan her kullanıcı için geçerli Azure AD Premium P1 veya daha yüksek bir abonelik lisansı gerekir. Bulut HR uygulamasına ait hatalı sayıda lisans, Kullanıcı sağlama sırasında hatalara neden olabilir.

### <a name="prerequisites"></a>Önkoşullar

- Azure AD Connect sağlama aracısını yapılandırmak için Azure AD Genel Yöneticisi erişimi.
- Bulut HR uygulamasının test ve üretim örneği.
- Bulut HR uygulamasındaki Yönetici izinleri bir sistem tümleştirme kullanıcısı oluşturur ve test amacıyla çalışan verilerini test etmek için değişiklikler yapar.
- AD 'ye Kullanıcı hazırlama için, [Azure AD Connect sağlama aracısını](https://go.microsoft.com/fwlink/?linkid=847801)barındırmak üzere .NET 4.7.1 + Runtime Ile Windows Server 2012 veya üstünü çalıştıran bir sunucu gerekir.
- AD ve Azure AD arasında kullanıcıları eşitlemek için [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-azure-ad-connect) .

### <a name="training-resources"></a>Eğitim kaynakları

| **Kaynaklar** | **Bağlantı ve açıklama** |
|:-|:-|
| Videolar | [Etkin Azure dizininde kullanıcı hazırlama nedir?](https://youtu.be/_ZjARPpI6NI) |
| | [Active Directory 'de Kullanıcı hazırlama nasıl dağıtılır?](https://youtu.be/pKzyts6kfrw) |
| Öğreticiler | [SaaS uygulamalarını Azure AD ile tümleştirme hakkında öğreticiler listesine](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) bakın |
| | [Öğretici: otomatik Kullanıcı sağlama için Workday yapılandırma](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#frequently-asked-questions-faq) |
| SSS | [Otomatik Kullanıcı sağlama](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning#frequently-asked-questions) |
| | [Workday 'den Azure AD 'ye sağlama](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#frequently-asked-questions-faq) |

### <a name="solution-architecture"></a>Çözüm mimarisi

Aşağıdaki örnek, genel karma ortamlar için uçtan uca Kullanıcı sağlama çözüm mimarisini açıklar ve şunları içerir:

- **Yetkılı HR veri akışı – Bu akışta, Cloud HR UYGULAMASıNDAN ad 'ye** -Bu akışta HR olayı (Joiners-broşür-Leavers işlemi), bulut HR uygulama kiracısında başlatılır. Azure AD sağlama hizmeti ve Azure AD Connect sağlama Aracısı, bulut HR uygulama kiracısından AD 'ye Kullanıcı verilerini sağlar. Olaya bağlı olarak, AD 'de oluşturma/güncelleştirme/etkinleştirme/devre dışı bırakma işlemlerine yol açabilir.
- **Şirket ıçı ad 'den bulut HR uygulamasına Azure AD ve geri yazma e-postası ve Kullanıcı adı Ile eşitleme** -hesaplar ad 'de güncelleştirildikten sonra, Azure AD Connect aracılığıyla Azure AD ile eşitlenir ve e-posta adresleri ve Kullanıcı adı öznitelikleri, bulut HR App kiracısına geri yazılabilir.

![İş akışı diyagramı](./media/plan-cloudhr-provisioning/plan-cloudhr-provisioning-img1.png)

#### <a name="description-of-workflow"></a>İş akışının açıklaması

Diyagramda gösterilen anahtar adımları şunlardır:  

1. **HR ekibi** , bulut HR uygulama kiracısında işlemleri gerçekleştirir.
2. **Azure AD sağlama hizmeti** , bulut HR App kiracısından zamanlanan döngüleri ÇALıŞTıRıR ve ad ile eşitleme için işlenmesi gereken değişiklikleri belirler.
3. **Azure AD sağlama hizmeti** , ad hesabı oluşturma/güncelleştirme/etkinleştirme/devre dışı bırakma işlemlerini içeren bir istek yüküne sahip Azure AD Connect sağlama aracısını çağırır.
4. **Azure AD Connect sağlama Aracısı** , ad hesabı verilerini yönetmek için bir hizmet hesabı kullanır.
5. **Azure AD Connect** , ad 'de güncelleştirmeleri çekmek için Delta [eşitlemesi](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-whatis) çalıştırır.
6. **Ad** GÜNCELLEŞTIRMELERI Azure AD ile eşitlenir.
7. **Azure AD sağlama hizmeti** , Azure AD 'den e-posta özniteliği ve Kullanıcı adı ' nı bulut HR App kiracısına geri yedekler

## <a name="plan-the-deployment-project"></a>Dağıtım projesini planlayın

Ortamınızda bu dağıtımın stratejisini belirlerken kurumsal ihtiyaçlarınızı göz önünde bulundurun.

### <a name="engage-the-right-stakeholders"></a>Doğru paydaşlara katılın

Teknoloji projeleri başarısız olduğunda, genellikle etki, sonuç ve sorumlulukların eşleşmeyen beklentileri nedeniyle bu, genellikle bu şekilde yapılır. Bu girişlerin oluşmasını önlemek için, [doğru paydaşları](https://aka.ms/deploymentplans) ve proje giriş ve accountatçlarını belgeleyerek projedeki paydaş rollerinin iyi anlaşıldığından emin olun.

IK kuruluştan, mevcut ık iş süreçlerine ve çalışan kimliği + iş verileri işleme gereksinimlerine giriş sağlayabilen bir temsilci dahil etmeniz gerekir.

### <a name="plan-communications"></a>İletişimleri planlayın

İletişim, her yeni hizmetin başarısı için önemlidir. Deneyimlerinizin nasıl değiştirileceği, ne zaman değiştirileceği ve sorunlarla karşılaştıklarında nasıl destek kazanabilecekleri konusunda, kullanıcılarınız ile önceden iletişim kurması gerekir.

### <a name="plan-a-pilot"></a>Pilot planı planlayın

Bulut HR uygulamasından hedef sistemlere ık iş süreçlerini ve kimlik iş akışlarını tümleştirmek, çözümü üretime dağıtmaya başlamadan önce önemli miktarda veri doğrulaması, veri dönüştürme, veri temizleme ve uçtan uca test gerektirir.

İlk yapılandırmanın, üretim ortamındaki tüm kullanıcılara ölçeklendirmeden önce bir [pilot ortamda](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-plans#best-practices-for-a-pilot) çalıştırılması önerilir.

## <a name="select-cloud-hr-provisioning-connector-apps"></a>Cloud HR sağlama bağlayıcı uygulamalarını seçin

Bulut HR uygulaması ve AD arasında Azure AD sağlama iş akışlarını kolaylaştırmak için, Azure AD uygulama galerisinden ekleyebileceğiniz çoklu sağlama bağlayıcı uygulamaları vardır:

- **Ad Kullanıcı sağlaması Için bulut HR uygulaması** – bu sağlama bağlayıcı uygulaması, bulut HR uygulamasından tek bir ad etki alanına kullanıcı hesabı sağlamayı kolaylaştırır. Birden çok etki alanınız varsa, sağlamanız gereken her AD etki alanı için Azure AD uygulama galerisinden bu uygulamanın bir örneğini ekleyebilirsiniz.
- **Bulut HR uygulaması Azure AD Kullanıcı sağlama** -Azure AD Connect ad KULLANıCıLARıNı Azure AD 'ye eşitlemede kullanılması gereken araç, bu sağlama bağlayıcı uygulaması, yalnızca bulutta bulunan KULLANıCıLARıN bulut HR uygulamasından tek BIR Azure AD kiracısına sağlanması için kullanılabilir.
- **Cloud İK uygulama geri yazma** -bu sağlama bağlayıcı uygulaması, kullanıcının Azure AD 'den bulut HR uygulamasına ait e-posta adreslerini geri yazmasını kolaylaştırır.

Örneğin, aşağıdaki görüntüde Azure AD uygulama galerisinde bulunan Workday Bağlayıcısı uygulamaları listelenmiştir.

![Azure Active Directory Portal Uygulama Galerisi](./media/plan-cloudhr-provisioning/plan-cloudhr-provisioning-img2.png)

### <a name="decision-flowchart"></a>Karar akış çizelgesi

Senaryolarınızla ilgili hangi bulut HR sağlama uygulamalarının olduğunu belirlemek için aşağıdaki karar akışı grafiğini kullanın.

![Karar akış çizelgesi](./media/plan-cloudhr-provisioning/plan-cloudhr-provisioning-img3.png)

## <a name="design-azure-ad-connect-provisioning-agent-deployment-topology"></a>Azure AD Connect sağlama Aracısı dağıtım topolojisi tasarlama

Cloud HR uygulaması ve AD arasındaki sağlama tümleştirmesi için bu dört bileşen gerekir:

- Cloud ık uygulama kiracısı
- Bağlayıcı uygulaması sağlanıyor
- Azure AD Connect sağlama Aracısı
- AD etki alanı

Azure AD Connect sağlama Aracısı dağıtım topolojisi, tümleştirmeyi planladığınız bulut HR uygulama kiracılarının ve AD alt etki alanlarının sayısına bağlıdır. Birden çok AD etki alanı varsa, AD etki alanlarının bitişik veya [ayrık](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/disjoint-namespace)olmasına bağlıdır.

Kararınız temelinde dağıtım senaryolarından birini seçin:

- Tek bulut HR App Tenant-> Güvenilen bir ormanda tek veya birden çok AD alt etki alanını hedefleyin
- Tek bulut HR App Tenant-> ayrık bir AD ormanında birden çok alt etki alanını hedefleme

### <a name="single-cloud-hr-app-tenant---single-or-multiple-target-ad-child-domains-in-a-trusted-forest"></a>Tek bulut ık uygulama kiracısı-güvenilen bir ormanda tek veya birden çok hedef AD alt etki alanı >

Aşağıdaki üretim yapılandırması önerilir:

|Gereksinim|Öneri|
|:-|:-|
|Dağıtılacak Azure AD Connect sağlama aracılarının sayısı|2 (yüksek kullanılabilirlik ve yük devretme için)
|Yapılandırılacak sağlama bağlayıcı uygulamalarının sayısı|alt etki alanı başına bir uygulama|
|Azure AD Connect sağlama Aracısı için sunucu Konağı|Windows 2012 R2 + coğrafi olarak bulunan AD etki alanı denetleyicileri hakkında görüş satırı</br>Azure AD Connect hizmetiyle birlikte çalışabilir|

![Şirket içi aracılara akış](./media/plan-cloudhr-provisioning/plan-cloudhr-provisioning-img4.png)

### <a name="single-cloud-hr-app-tenant---target-multiple-child-domains-in-a-disjoint-ad-forest"></a>Tek bulut HR App Tenant-> ayrık bir AD ormanında birden çok alt etki alanını hedefleme

Bu senaryo, kullanıcıların bulut HR uygulamasından ayrık AD ormanlarındaki etki alanlarına sağlamasını içerir.

Aşağıdaki üretim yapılandırması önerilir:

|Gereksinim|Öneri|
|:-|:-|
|Şirket içinde dağıtılacak Azure AD Connect sağlama aracılarının sayısı|2 ayrık AD Ormanı başına|
|Yapılandırılacak sağlama bağlayıcı uygulamalarının sayısı|alt etki alanı başına bir uygulama|
|Azure AD Connect sağlama Aracısı için sunucu Konağı|Windows 2012 R2 + coğrafi olarak bulunan AD etki alanı denetleyicileri hakkında görüş satırı</br>Azure AD Connect hizmetiyle birlikte çalışabilir|

![Tek bulut ık uygulama kiracının kopuk AD Ormanı](./media/plan-cloudhr-provisioning/plan-cloudhr-provisioning-img5.png)

### <a name="azure-ad-connect-provisioning-agent-requirements"></a>Azure AD Connect sağlama Aracısı gereksinimleri

Bulut HR uygulamasının AD Kullanıcı sağlama çözümüne, en az 4 GB RAM ve .NET 4.7.1 + çalışma zamanı ile Windows 2012 R2 veya üzeri çalıştıran sunucularda bir veya daha fazla Azure AD Connect sağlama aracısının dağıtılmasını gerektirir. Konak sunucusunun hedef AD etki alanına ağ erişimi olduğundan emin olun.

Şirket içi ortamı hazırlamak için Azure AD Connect sağlama Aracısı Yapılandırma Sihirbazı aracıyı Azure AD kiracınızla kaydeder, [bağlantı noktaları açar](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#open-ports), [URL 'lere erişim sağlar](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#allow-access-to-urls)ve [giden HTTPS proxy yapılandırmasını](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#how-do-i-configure-the-provisioning-agent-to-use-a-proxy-server-for-outbound-http-communication)destekler.

Sağlama Aracısı, AD etki alanı (ler) ile iletişim kurmak için bir hizmet hesabı kullanır. Aracının yüklenmesinden önce, AD kullanıcıları ve bilgisayarlarında aşağıdaki gereksinimleri karşılayan bir hizmet hesabı oluşturmanız önerilir:

- Süre sonu olmayan bir parola
- Kullanıcı hesaplarını okumak, oluşturmak, silmek ve yönetmek için atanan denetim izinleri

Sağlama isteklerini işlemesi gereken etki alanı denetleyicileri seçebilirsiniz. Coğrafi olarak dağıtılmış birkaç etki alanı denetleyiciniz varsa, uçtan uca çözümün güvenilirliğini ve performansını geliştirmek için, tercih edilen etki alanı denetleyicileriniz ile aynı siteye sağlama aracısını yükler.

Yüksek kullanılabilirlik için birden fazla Azure AD Connect sağlama Aracısı dağıtabilir ve aynı şirket içi AD etki alanı kümesini işleyecek şekilde kaydedebilirsiniz.

## <a name="plan-scoping-filters-and-attribute-mapping"></a>Kapsam filtrelerini ve öznitelik eşlemesini planlayın

Bulut HR uygulamasından AD veya Azure AD 'ye sağlamayı etkinleştirdiğinizde Azure portal öznitelik değerlerini öznitelik eşlemesi aracılığıyla denetler.

### <a name="define-scoping-filters"></a>Kapsam filtrelerini tanımlama

Bulut HR uygulamasından AD veya Azure AD 'ye hangi kullanıcıların sağlanması gerektiğini belirleyen öznitelik tabanlı kuralları tanımlamak için [kapsam filtrelerini](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters) kullanın.

Birleştiriciler sürecini başlattığınızda, aşağıdaki gereksinimleri toplayın:

- Bulut ık uygulaması, şirket içi çalışanları ve çalışanların çalışanları için kullanılabilir mi?
- Hem çalışanları hem de çalışanların çalışanlarını yönetmek için bulut HR uygulamasını Azure AD Kullanıcı sağlaması için kullanmayı planlıyor musunuz?
- Bulut HR uygulamasını Azure AD Kullanıcı sağlaması için yalnızca bulut ık uygulaması kullanıcılarının bir alt kümesi (örneğin, yalnızca çalışanlar) için kullanıma almayı planlıyor musunuz?

Gereksinimlerinize bağlı olarak, öznitelik eşlemelerini yapılandırırken, bulut HR uygulamasındaki Kullanıcı kümelerinin AD için sağlama kapsamında olması gerektiğini seçmek için **kaynak nesne kapsamı** alanını ayarlayabilirsiniz. Yaygın olarak kullanılan kapsam filtreleri için Cloud HR uygulaması öğreticisine bakın.

### <a name="determine-matching-attributes"></a>Eşleşen öznitelikleri belirle

Sağlama sayesinde, kaynak ve hedef sistem arasındaki mevcut hesapları eşleştirme imkanını alırsınız. Bulut HR uygulamasını Azure AD sağlama hizmeti ile tümleştirdiğinizde, bulut HR uygulamasından AD 'ye veya Azure AD 'ye hangi kullanıcı verilerinin akaceğini belirlemek için [öznitelik eşlemesini yapılandırabilirsiniz](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-automatic-user-provisioning-portal#mappings) .

Birleştiriciler sürecini başlattığınızda, aşağıdaki gereksinimleri toplayın:

- Bu bulut HR uygulamasındaki her kullanıcıyı tanımlamak için kullanılan benzersiz KIMLIK nedir?
- Kimlik yaşam döngüsü perspektifinden, yeniden oluşturma işlemlerinin nasıl yapılacağını nasıl işleyeceğinizi? Eski çalışan KIMLIKLERINI yeniden bekletme mi?
- Gelecekte bu işlemleri yapmak için gelecekteki ve oluşturulmuş AD hesaplarını işleyebilir mi?
- Bir kimlik yaşam döngüsü perspektifinden, çalışanı iş dönüşümünü veya başka bir şekilde nasıl işleyeceğinizi?
- Dönüştürülmüş kullanıcılar eski AD hesaplarını korur veya yenilerini alır mi?

Gereksiniminize bağlı olarak, Azure AD doğrudan özniteliğin öznitelik eşlemesi, sabit değerler sağlama veya [öznitelik eşlemeleri için ifadeler yazma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-writing-expressions-for-attribute-mappings)ile desteklenir. Bu esneklik, hedeflenen uygulama özniteliğinde doldurulacak öğeleri en son denetim altına almanızı sağlar. Kullanıcı hazırlama öznitelik eşlemelerinizi ve şemanızı bir JSON dosyasına aktarmak ve yeniden Azure AD 'ye aktarmak için [MICROSOFT Graph API](https://docs.microsoft.com/azure/active-directory/manage-apps/export-import-provisioning-configuration) ve Graf Gezginini kullanabilirsiniz.

**Varsayılan**olarak, bulut HR uygulamasındaki BENZERSIZ çalışan kimliğini temsil eden ÖZNITELIK, *ad 'deki UNIQUE özniteliğiyle eşlenen* eşleşen öznitelik olarak kullanılır. Örneğin, Workday App senaryosunda, *Workday* *WORKERıD* özniteliği ad *ÇalışanNo* özniteliğiyle eşleştirilir.

Birden çok eşleşen öznitelik ayarlayabilir ve eşleşen önceliği atayabilirsiniz. Bunlar, eşleşen önceliğe göre değerlendirilir. Bir eşleşme bulunur başlamaz, başka eşleşen öznitelikler değerlendirilir.

Ayrıca, var olan öznitelik eşlemelerini değiştirme veya silme gibi [varsayılan öznitelik eşlemelerini özelleştirebilir](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes#understanding-attribute-mapping-types) veya iş gereksinimlerinize göre yeni öznitelik eşlemeleri oluşturabilirsiniz. Eşlenecek özel özniteliklerin bir listesi için Cloud HR App öğreticisi ( [Workday](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#planning-workday-to-active-directory-user-attribute-mapping-and-transformations)gibi) bölümüne bakın.

### <a name="determine-user-account-status"></a>Kullanıcı hesabı durumunu belirleme

Varsayılan olarak, sağlama bağlayıcı uygulaması, Kullanıcı hesabının etkinleştirilip etkinleştirilmeyeceğini veya devre dışı bırakılacağını öğrenmek için **HR Kullanıcı profili durumunu** AD/Azure AD 'deki **Kullanıcı hesabı durumuyla** eşler.

Joiners/sızıntı sürecini başlattığınızda, aşağıdaki gereksinimleri toplayın:

| Süreç | Gereksinimler |
| - | - |
| **Birleştiriciler** | Kimlik yaşam döngüsü perspektifinden, yeniden oluşturma işlemlerinin nasıl yapılacağını nasıl işleyeceğinizi? Eski çalışan KIMLIKLERINI yeniden bekletme mi? |
| | Gelecekteki ilerletlerinizi mi işyorsunuz, yoksa AD hesapları mı var? Bu hesaplar etkin/devre dışı durumunda oluşturulmuştur mi? |
| | Bir kimlik yaşam döngüsü perspektifinden, çalışanı iş dönüşümünü veya başka bir şekilde nasıl işleyeceğinizi? |
| | Dönüştürülmüş kullanıcılar eski AD hesaplarını korur veya yenilerini alır mi? |
| **Yük devretme** | Sonlandırmalar, çalışanlar ve reklam çalışanları için farklı şekilde işleniyorlar mı? |
| | Kullanıcı sonlandırmasını işlemek için hangi etkin tarihler dikkate alınır? |
| | Çalışan ve çalışanların çalışan dönüştürmeleri mevcut AD hesaplarını nasıl etkiler? |
| | AD 'de "rescind" işlemini nasıl işleirsiniz? Daha sonra AD içinde, birleştirici Işleminin bir parçası olarak bir sonraki kullanımlar oluşturulduysa, rescind işlemlerinin işlenmesi gerekir. |

Gereksinimlerinize bağlı olarak, [Azure AD deyimlerini](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data) kullanarak eşleme mantığını özelleştirebilir, böylece ad hesabı bir veri noktalarının birleşimine göre etkinleştirilebilir veya devre dışı bırakılır.

### <a name="map-cloud-hr-app-to-ad-user-attributes"></a>Bulut HR uygulamasını AD Kullanıcı özniteliklerine eşleyin

Her bir bulut HR uygulaması, AD eşlemelerine varsayılan bulut ık uygulamaları ile birlikte gönderilir.

Birleştiriciler/broşür/yük devretme sürecini başlattığınızda, aşağıdaki gereksinimleri toplayın:

| Süreç | Gereksinimler |
| - | - |
| **Birleştiriciler** | AD hesabı oluşturma işlemi el ile, otomatik veya kısmen otomatikleştirildi mi? |
| | Bulut HR uygulamasından özel öznitelikleri AD 'ye yaymaya planlansın mı? |
| **Taşımacılar** | Bulut HR uygulamasında bir "Taşımacılar" işlemi gerçekleştiğinde hangi öznitelikleri işlemek istiyorsunuz? |
| | Kullanıcı güncelleştirmeleri sırasında belirli öznitelik doğrulamaları gerçekleştirsin mi? Yanıt Evet ise lütfen ayrıntıları belirtin. |
| **Yük devretme** | Sonlandırmalar, çalışanlar ve reklam çalışanları için farklı şekilde işleniyorlar mı? |
| | Kullanıcı sonlandırmasını işlemek için hangi etkin tarihler dikkate alınır? |
| | Çalışan ve çalışanların çalışan dönüştürmeleri mevcut AD hesaplarını nasıl etkiler? |

Gereksinimlerinize bağlı olarak, eşlemeleri tümleştirme hedeflerinize uyacak şekilde değiştirebilirsiniz. Eşlenecek özel özniteliklerin bir listesi için belirli bulut HR uygulama öğreticisine ( [Workday](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#planning-workday-to-active-directory-user-attribute-mapping-and-transformations)gibi) bakın.

### <a name="generate-unique-attribute-value"></a>Benzersiz öznitelik değeri oluştur

Birleştiriciler sürecini başlattığınızda, benzersiz kısıtlamalara sahip CN, samAccountName ve UPN gibi öznitelikleri ayarlarken benzersiz öznitelik değerleri oluşturmanız gerekebilir.

Azure AD işlevi [Selectuniquevalues](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data#selectuniquevalue) her kuralı değerlendirir ve hedef sistemde benzersizlik için oluşturulan değeri denetler. Örnek [, userPrincipalName (UPN) özniteliği için benzersiz değer oluşturma](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data#generate-unique-value-for-userprincipalname-upn-attribute)bölümüne bakın.

> [!NOTE]
> Bu işlev şu anda yalnızca "Workday for User sağlamasını Active Directory için desteklenmektedir." Diğer sağlama uygulamalarıyla birlikte kullanılamaz.

### <a name="configure-ad-ou-container-assignment"></a>AD OU kapsayıcı atamasını yapılandırma

AD Kullanıcı hesaplarını iş birimlerine, konumlarına ve departmanlara göre kapsayıcılara yerleştirmek yaygın bir gereksinimdir. Bir geçiş işlemi başlattığınızda ve bir gözehirde kuruluş değişikliği varsa, kullanıcıyı AD 'de bir OU 'dan diğerine taşımanız gerekebilir.

OU atamasının iş mantığını yapılandırmak ve bunu *PARENTDISTINGUISHEDNAME*ad özniteliğiyle eşlemek için [Switch ()](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data#switch) işlevini kullanın.

Örneğin, "Municipitesi" HR özniteliğine bağlı olarak OU 'da Kullanıcı oluşturmak istiyorsanız aşağıdaki ifadeyi kullanabilirsiniz.

`
Switch([Municipality], "OU=Default,OU=Users,DC=contoso,DC=com", "Dallas", "OU=Dallas,OU=Users,DC=contoso,DC=com", "Austin", "OU=Austin,OU=Users,DC=contoso,DC=com", "Seattle", "OU=Seattle,OU=Users,DC=contoso,DC=com", "London", "OU=London,OU=Users,DC=contoso,DC=com")
`

Bu ifadeyle, Municipitesi değeri Dallas, Austin, Seattle veya Londra ise, Kullanıcı hesabı ilgili OU 'da oluşturulur. Eşleşme yoksa, hesap varsayılan OU 'da oluşturulur.

## <a name="plan-for-password-delivery-of-new-user-accounts"></a>Yeni Kullanıcı hesaplarının parola teslimini planlayın

Birleştiriciler sürecini başlattığınızda, Yeni Kullanıcı hesaplarının geçici bir parolasını ayarlayıp teslim etmeniz gerekir. Azure AD Kullanıcı sağlama bulutu sayesinde, 1. günde Kullanıcı için Azure AD [self servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr) (SSPR) özelliğini kullanıma alabilirsiniz.

SSPR, BT yöneticilerinin kullanıcıların parolalarını sıfırlamalarını veya hesaplarının kilidini açmalarına olanak tanımak için basit bir araçtır. Bulut HR uygulamasından AD 'ye **Mobil Numara** özniteliği sağlayabilir ve Azure AD ile eşitleyebilirsiniz. **Cep telefonu numarası** ÖZNITELIĞI Azure AD 'de olduğunda, Kullanıcı hesabı için SSPR 'yi etkinleştirerek 1. gün, yeni kullanıcıların kimlik doğrulaması için kayıtlı ve onaylanmış mobil numarayı kullanabilmesi gerekir.

## <a name="plan-for-initial-cycle"></a>İlk döngüyü planlayın

Azure AD sağlama hizmeti ilk kez çalıştırıldığında, bulut HR uygulamasındaki tüm Kullanıcı nesnelerinin anlık görüntüsünü oluşturmak için bulut HR uygulamasına karşı bir [Başlangıç döngüsünü](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning#what-happens-during-provisioning) gerçekleştirir. İlk Döngülerde geçen süre, kaynak sistemde kaç kullanıcının var olduğunu doğrudan bağlıdır. 100.000 ' den fazla kullanıcısı olan bazı bulut ık uygulama kiracılarının ilk döngüsünün süresi uzun sürebilir.

**Büyük bulut İK uygulama kiracılar (> 30.000 kullanıcıları) için,** başlangıç döngüsünü aşamalı aşamalarda çalıştırmanızı ve yalnızca farklı Kullanıcı HAZıRLAMA senaryolarında ad içinde doğru özniteliklerin ayarlandığını doğruladıktan sonra Artımlı güncelleştirmeleri etkinleştirmenizi öneririz. Aşağıdaki sırayı izleyin:

1. [Kapsam filtresini](#plan-scoping-filters-and-attribute-mapping)ayarlayarak yalnızca sınırlı bir kullanıcı kümesi için başlangıç döngüsünü çalıştırın.
2. İlk çalıştırma için seçilen kullanıcılar için ayarlanan AD hesabı sağlamayı ve öznitelik değerlerini doğrulayın. Sonuç beklentilerinizi karşılıyorsa, daha fazla kullanıcı eklemek ve ikinci çalıştırmanın sonuçlarını doğrulamak için kapsam filtresini genişletin.

Test kullanıcıları için başlangıç döngüsünün sonuçlarıyla memnun olduktan sonra, [Artımlı güncelleştirmeleri](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning#incremental-cycles)başlatabilirsiniz.

## <a name="plan-testing-and-security"></a>Test ve güvenlik planlaması

İlk pilot bilgisayardan dağıtımınızın her aşamasında, Kullanıcı sağlamayı etkinleştirerek, sonuçların beklenildiği şekilde test olduğunuzdan ve sağlama döngülerini denetleyerek emin olun.

### <a name="plan-testing"></a>Test planı

Bulut HR uygulamasını Azure AD Kullanıcı sağlama olarak yapılandırdıktan sonra, bu çözümün kuruluşunuzun gereksinimlerini karşılayıp karşılamadığını doğrulamak için test çalışmalarını çalıştırmanız gerekir.

|Senaryolar|Beklenen sonuçlar|
|:-|:-|
|Cloud HR uygulamasında yeni çalışan işe alma| -Kullanıcı hesabı AD 'de sağlanır.</br>-Kullanıcılar AD etki alanı uygulamalarında oturum açabilir ve istenen eylemleri gerçekleştirebilir.</br>-AAD Connect eşitlemesi yapılandırılmışsa, Kullanıcı hesabı da Azure AD 'de oluşturulur.
|Kullanıcı, bulut HR uygulamasında sonlandırıldı|-Kullanıcı hesabı AD 'de devre dışı bırakıldı.</br>-Kullanıcı AD tarafından korunan tüm kurumsal uygulamalarda oturum açılamıyor.
|Bulut HR uygulamasında Kullanıcı gözetim kuruluşu güncelleştirildi|Öznitelik eşlemesine göre, Kullanıcı hesabı bir OU 'dan AD 'de diğerine gider.|
|HR, kullanıcının yöneticisini Cloud HR uygulamasında güncelleştirir|AD 'deki yönetici alanı, yeni yöneticinin adını yansıtacak şekilde güncelleştirilir.|
|HR bir çalışanı yeni bir rol olarak yeniden oluşturur.|Davranış, bulut HR uygulamasının çalışan kimliklerini oluşturmak için nasıl yapılandırıldığına bağlıdır:</br>-Eski çalışan KIMLIĞI yeniden kullanılıyorsa, bağlayıcı kullanıcı için mevcut AD hesabını etkinleştirir.</br>-Rehires yeni bir çalışan KIMLIĞI alıyorsa, bağlayıcı kullanıcı için yeni bir AD hesabı oluşturur.|
|HR, çalışanı sözleşmeye göre veya tam tersi yönde dönüştürür|Yeni kişi için yeni bir AD hesabı oluşturulur ve eski hesap dönüştürme geçerlilik tarihinde devre dışı bırakılır.|

Otomatik Kullanıcı sağlama uygulamanızın, belirlenen zaman çizelgelerinizi temel alarak üretime nasıl geçeceğinin belirlenmesi için yukarıdaki sonuçları kullanın.

> [!TIP]
> Gizli PII (kişisel olarak tanımlanabilir bilgiler) verilerini kaldırmak/maskelemek ve gizlilik ve güvenlik standartlarına uymak üzere üretim verileriyle test ortamı yenilenirken veri azaltma ve veri temizleme gibi tekniklerin kullanılmasını öneririz.

### <a name="plan-security"></a>Plan güvenliği

Yeni bir hizmet dağıtımının bir parçası olarak bir güvenlik incelemesinin kullanılması yaygındır. Bir güvenlik incelemesi gerekliyse veya henüz yürütülmediyse, lütfen hizmet olarak kimliğe bir genel bakış sağlayan Azure AD [teknik incelemeler](https://www.microsoft.com/download/details.aspx?id=36391) ' ı inceleyin.

### <a name="plan-rollback"></a>Planı geri alma

Bulut HR Kullanıcı sağlama uygulamasının üretim ortamında istendiği gibi çalışması başarısız olursa, aşağıdaki geri alma adımları önceki bilinen iyi duruma geri dönmede size yardımcı olabilir:

1. Etkilenen kullanıcılar ve/veya gruplar üzerinde hatalı işlemlerin ne olduğunu belirlemek için [sağlama Özeti raporunu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting#getting-provisioning-reports-from-the-azure-management-portal) ve [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting#provisioning-audit-logs) gözden geçirin (bkz. [Cloud İK uygulama Kullanıcı sağlamasını yönetme](#manage-your-configuration)).
2. Etkilenen Kullanıcı ve/veya grupların bilinen son iyi durumu, sağlama denetim günlükleri aracılığıyla veya hedef sistemleri (Azure AD veya AD) inceleyerek belirlenebilir.
3. En son bilinen iyi durum değerlerini kullanarak uygulamada doğrudan etkilenen kullanıcıları ve/veya grupları güncelleştirmek için uygulama sahibiyle birlikte çalışın.

## <a name="deploy-the-cloud-hr-app"></a>Cloud HR uygulamasını dağıtma

Lütfen çözüm gereksinimlerinize göre hizalanan bulut HR uygulamasını seçin.

**Workday** -e-posta adresi ve Kullanıcı adı için isteğe bağlı geri yazma ile Workday 'den çalışan PROFILLERINI hem ad hem de Azure AD 'ye aktarmak için iş parçacığını [Otomatik Kullanıcı sağlaması Için Iş günü yapılandırma öğreticisini](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#planning-your-deployment) inceleyin.

## <a name="manage-your-configuration"></a>Yapılandırmanızı yönetin

Azure AD, kuruluşunuzun Kullanıcı sağlama kullanımı ve denetim günlükleri ve raporları aracılığıyla işletimsel sistem durumu hakkında ek öngörüler sağlayabilir.

### <a name="gain-insights-from-reports-and-logs"></a>Raporlardan ve günlüklerden öngörü elde edin

Başarılı bir [Başlangıç döngüsünden](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning#what-happens-during-provisioning)sonra, Azure AD sağlama hizmeti, aşağıdaki olaylardan biri gerçekleşene kadar her bir uygulamaya özgü öğreticilerde tanımlanan aralıklarda sonsuza kadar arka arkaya Artımlı güncelleştirmeleri çalıştırmaya devam edecektir:

- Hizmet el ile durdurulur ve [Azure Portal](https://portal.azure.com/) veya uygun [Microsoft Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview) komutu kullanılarak tetiklenen yeni bir başlangıç döngüsüdür.
- Öznitelik eşlemelerinde veya kapsam filtrelerinde değişiklik nedeniyle yeni bir başlangıç çevrimi tetiklenir.
- Sağlama işlemi, yüksek bir hata oranı nedeniyle karantinaya alınır ve dört haftadan daha uzun bir süre boyunca otomatik olarak devre dışı bırakılacak şekilde kalır.

Bu olayları ve sağlama hizmeti tarafından gerçekleştirilen diğer tüm etkinlikleri gözden geçirmek için, [günlükleri incelemeyi ve sağlama etkinliğine ilişkin raporları almayı öğrenin](https://docs.microsoft.com/azure/active-directory/manage-apps/check-status-user-account-provisioning).

#### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

Sağlama hizmeti tarafından gerçekleştirilen tüm etkinlikler **Azure AD denetim günlüklerine**kaydedilir. Azure AD denetim günlüklerini daha fazla analiz için Azure Izleyici günlüklerine yönlendirebilirsiniz. **Azure izleyici günlükleri (Log Analytics çalışma alanı olarak da bilinir)** olayları bulmak, eğilimleri analiz etmek ve çeşitli veri kaynakları arasında bağıntı gerçekleştirmek için verileri sorgulamanızı sağlar. Pratik Kullanıcı senaryolarında Azure AD günlükleri için Azure Izleyici günlüklerini kullanmanın avantajlarını öğrenmek için bu [videoyu](https://youtu.be/MP5IaCTwkQg) izleyin.

Ortamınızdaki sağlama olayları etrafında [önceden oluşturulmuş raporlara](https://github.com/AzureAD/Deployment-Plans/tree/master/Log%20Analytics%20Views) erişim sağlamak IÇIN [Azure ad etkinlik günlükleri için Log Analytics görünümlerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-install-use-log-analytics-views) yükleyebilirsiniz.

Daha fazla bilgi için bkz [. Azure izleyici günlüklerinizi Azure ad etkinlik günlüklerini çözümleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics)

### <a name="manage-personal-data"></a>Kişisel verileri yönetme

Windows Server 'da yüklü olan Azure AD Connect sağlama Aracısı, Windows olay günlüğünde, bulut HR uygulamanıza AD öznitelik eşlemelerine bağlı olarak kişisel veriler içerebilen Günlükler oluşturur. Kullanıcı gizliliği yükümlülükleriyle uyum sağlamak için, olay günlüğünü temizlemek üzere bir Windows zamanlanmış görevi ayarlayabilir ve hiçbir verinin 48 saatten fazla tutulmayacağından emin olabilirsiniz.

Azure AD sağlama hizmeti, raporlar oluşturmaz, analiz gerçekleştirmez veya 30 günden fazla öngörü sunmaz. Bu nedenle, Azure AD sağlama hizmeti 30 günden daha fazla veri depolamaz, işlemez veya korumaz. 

### <a name="troubleshoot"></a>Sorun giderme

Sağlama sırasında oluşabilecek sorunları gidermek için aşağıdaki bağlantılara bakın:

- [Azure AD Galeri uygulamasına kullanıcı sağlamayı yapılandırma sorunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem)
- [Bir uygulamaya sağlamak için şirket içi Active Directory bir özniteliği Azure AD 'ye eşitleyin](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning-sync-attributes-for-mapping)
- [Azure AD Galeri uygulamasına Kullanıcı hazırlama saati veya daha fazlasını sürüyor](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish)
- [Azure Active Directory Galeri uygulamasına kullanıcı sağlamayı yapılandırırken yönetici kimlik bilgileri kaydedilirken sorun oluştu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem-storage-limit)
- [Azure AD Galeri uygulamasına hiçbir Kullanıcı sağlanmıyor](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem-no-users-provisioned)
- [Azure AD Galeri uygulamasına yanlış kullanıcı kümesi sağlanıyor](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem-wrong-users-provisioned)
- [Windows Olay Görüntüleyicisi aracı sorunlarını giderme için ayarlama](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#setting-up-windows-event-viewer-for-agent-troubleshooting)
- [Hizmet sorunlarını giderme için Azure portal denetim günlüklerini ayarlama](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#setting-up-azure-portal-audit-logs-for-service-troubleshooting)
- [AD Kullanıcı hesabı oluşturma işlemleri için günlükleri anlama](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#understanding-logs-for-ad-user-account-create-operations)
- [Yönetici güncelleştirme işlemlerine yönelik günlükleri anlama](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#understanding-logs-for-manager-update-operations)
- [Yaygın olarak karşılaşılan hatalar çözümleniyor](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#resolving-commonly-encountered-errors)

### <a name="next-steps"></a>Sonraki adımlar

- [Öznitelik eşlemeleri için ifadeler yazma](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data)
- [Azure AD eşitleme API 'sine genel bakış](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
- [Kapsam dışına çıkan Kullanıcı hesaplarını silmeyi atlayın](https://docs.microsoft.com/azure/active-directory/manage-apps/skip-out-of-scope-deletions)
- [Azure AD Connect sağlama Aracısı: sürüm yayınlama geçmişi](https://docs.microsoft.com/azure/active-directory/manage-apps/provisioning-agent-release-version-history)
