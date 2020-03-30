---
title: Azure AD'de Otomatik SaaS uygulaması kullanıcı sağlama | Microsoft Dokümanlar
description: Birden çok üçüncü taraf SaaS uygulamasında kullanıcı hesaplarını otomatik olarak sağlamak, sağlamadan çıkarmak ve sürekli olarak güncelleştirmek için Azure AD'yi nasıl kullanabileceğinize ilişkin bir giriş.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/25/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5e828fd9c2561007c332db67bfd0b20dda9b845f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79454542"
---
# <a name="automate-user-provisioning-and-deprovisioning-to-applications-with-azure-active-directory"></a>Azure Active Directory ile uygulamalara kullanıcı sağlama ve uygulama silme yi otomatikleştirin

Azure Etkin Dizini'nde (Azure AD) **uygulama sağlama** terimi, kullanıcıların erişilmesi gereken bulut[(SaaS)](https://azure.microsoft.com/overview/what-is-saas/)uygulamalarında otomatik olarak kullanıcı kimlikleri ve rolleri oluşturmayı ifade eder. Kullanıcı kimlikleri oluşturmaya ek olarak, otomatik sağlama, durum veya roller değiştikçe kullanıcı kimliklerinin bakımını ve kaldırılmasını içerir. Sık karşılaşılan senaryolar arasında Bir Azure AD kullanıcısına [Dropbox,](../saas-apps/dropboxforbusiness-provisioning-tutorial.md) [Salesforce](../saas-apps/salesforce-provisioning-tutorial.md), [ServiceNow](../saas-apps/servicenow-provisioning-tutorial.md)ve daha fazlası gibi uygulamalarda yardımcı olmak yer almaktadır.

![Genel bakış diyagramı sağlama](./media/user-provisioning/provisioning-overview.png)

Bu özellik şunları yapmanızı sağlar:

- **Sağlamayı otomatikleştirin**: Ekibinize veya kuruluşunuza katıldıklarında yeni kişiler için doğru sistemlerde otomatik olarak yeni hesaplar oluşturun.
- **Deprovisioning'i otomatikleştirin:** İnsanlar takımdan veya kuruluştan ayrıldıklarında doğru sistemlerdeki hesapları otomatik olarak devre dışı bırakın.
- **Sistemleri arasında veri eşitlemek:** Uygulamalarınızdaki ve sistemlerinizdeki kimliklerin dizindeki veya insan kaynakları sisteminizdeki değişikliklere bağlı olarak güncel tutulmasını sağlayın.
- **Tedarik grupları:** Onları destekleyen uygulamalara sağlama grupları.
- **Erişimi yönetin:** Uygulamalarınıza kimlerin sağlandığını izleyin ve denetleyebilirsiniz.
- **Kahverengi alan senaryolarında sorunsuz bir şekilde dağıtın:** Sistemler arasındaki varolan kimlikleri eşleştirin ve kullanıcılar hedef sistemde zaten mevcut olsa bile kolay tümleştirmeye olanak sağlar.
- **Zengin özelleştirme kullanın:** Kaynak sistemden hedef sisteme hangi kullanıcı verilerinin akması gerektiğini tanımlayan özelleştirilebilir öznitelik eşlemelerinden yararlanın.
- **Kritik olaylar için uyarılar alın:** Sağlama hizmeti, kritik olaylar için uyarılar sağlar ve iş ihtiyaçlarınızı karşılamak için özel uyarılar tanımlayabileceğiniz Log Analytics entegrasyonuna olanak tanır.

## <a name="benefits-of-automatic-provisioning"></a>Otomatik sağlamanın faydaları

Modern kuruluşlarda kullanılan uygulama sayısı artmaya devam ettikçe, BT yöneticileri ölçekte erişim yönetimi ile görevlidir. Güvenlik İddiaları Biçimlendirme Dili (SAML) veya Open ID Connect (OIDC) gibi standartlar, yöneticilerin tek oturum açma (SSO) hızlı bir şekilde ayarlamasına olanak sağlar, ancak erişim, kullanıcıların uygulamaya girmesini de gerektirir. Birçok yönetici için, sağlama, her kullanıcı hesabını el ile oluşturma veya her hafta CSV dosyaları yüklemek anlamına gelir, ancak bu işlemler zaman alıcı, pahalı ve hataya açıktır. Sağlamayı otomatikleştirmek için SAML (JIT) gibi çözümler benimsenmiştir, ancak işletmelerin kullanıcılar kuruluştan ayrıldıklarında veya artık rol değişikliğine dayalı belirli uygulamalara erişim gerektirmediğinde de bir çözüme ihtiyaçları vardır.

Otomatik sağlama yı kullanmak için bazı yaygın motivasyonlar şunlardır:

- Tedarik süreçlerinin verimliliğini ve doğruluğunu en üst düzeye çıkarmak.
- Özel olarak geliştirilmiş sağlama çözümleri ve komut dosyaları barındırma ve sürdürme ile ilgili maliyetlerden tasarruf.
- Kullanıcıların kuruluştan ayrıldıklarında önemli SaaS uygulamalarından kimliklerini anında kaldırarak kuruluşunuzun güvenliğini sağlar.
- Çok sayıda kullanıcıyı belirli bir SaaS uygulamasına veya sistemine kolayca aktarın.
- Kimlerin sağlanıp bir uygulamada kimlerin oturum açabileceğini belirlemek için tek bir ilke kümesine sahip olmak.

Azure AD kullanıcı sağlama, bu zorlukların üstesinden gelinmede yardımcı olabilir. Müşterilerin Azure AD kullanıcı sağlamasını nasıl kullandıkları hakkında daha fazla bilgi edinmek için [ASOS örnek olay incelemesini](https://aka.ms/asoscasestudy)okuyabilirsiniz. Aşağıdaki video, Azure AD'deki kullanıcı sağlama genel görünümünü sağlar:

> [!VIDEO https://www.youtube.com/embed/_ZjARPpI6NI]

## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>Azure AD otomatik kullanıcı sağlama ile hangi uygulamaları ve sistemleri kullanabilirim?

Azure AD, birçok popüler SaaS uygulaması ve insan kaynakları sistemi için önceden entegre edilmiş destek ve [SCIM 2.0 standardının](https://techcommunity.microsoft.com/t5/Identity-Standards-Blog/Provisioning-with-SCIM-getting-started/ba-p/880010)belirli bölümlerini uygulayan uygulamalar için genel destek sunar.

* **Önceden entegre uygulamalar (galeri SaaS uygulamaları)**. Azure AD'nin önceden tümleşik bir sağlama bağlayıcısını desteklediği tüm [uygulamaları, kullanıcı sağlama için uygulama öğreticileri listesinde](../saas-apps/tutorial-list.md)bulabilirsiniz. Galeride listelenen önceden tümleşik uygulamalar genellikle sağlama için SCIM 2.0 tabanlı kullanıcı yönetimi API'lerini kullanır. 

   ![Salesforce logosu](./media/user-provisioning/gallery-app-logos.png)

   Tedarik için yeni bir uygulama talep etmek istiyorsanız, [uygulamanızın uygulama galerimizle bütünleştirilmesini isteyebilirsiniz.](../develop/howto-app-gallery-listing.md) Bir kullanıcı sağlama isteği için, uygulamanın SCIM uyumlu bir bitiş noktasına sahip olmasını şart görüyoruz. Uygulama satıcısının SCIM standardına uymasını isteyin, böylece uygulamadan hızlı bir şekilde platformumuza inebilir.

* **SCIM 2.0'ı destekleyen uygulamalar.** SCIM 2.0 tabanlı kullanıcı yönetimi API'lerini uygulayan uygulamaların genel olarak nasıl bağlanacağı hakkında bilgi [için](use-scim-to-provision-users-and-groups.md)bkz.

## <a name="what-is-system-for-cross-domain-identity-management-scim"></a>Etki Alanı Arası Kimlik Yönetimi Sistemi (SCIM) nedir?

Sağlama ve deprovisioning otomatikleştirmek yardımcı olmak için, uygulamalar özel kullanıcı ve grup API'leri ortaya çıkarır. Ancak, birden fazla uygulamada kullanıcıları yönetmeye çalışan herkes, her uygulamanın kullanıcıları oluşturma veya güncelleme, gruplara kullanıcı ekleme veya kullanıcıları sağlama gibi aynı basit eylemleri gerçekleştirmeye çalıştığını söyler. Ancak, tüm bu basit eylemler, farklı uç nokta yolları, kullanıcı bilgilerini belirtmek için farklı yöntemler ve her bilgi öğesini temsil edecek farklı bir şema kullanılarak biraz farklı olarak uygulanır.

Bu zorlukları gidermek için SCIM belirtimi, kullanıcıların uygulamalara girmelerine, uygulamalara girmelerine, uygulamalara girmelerine ve bunların etrafında dolaşmalarına yardımcı olmak için ortak bir kullanıcı şeması sağlar. SCIM, sağlama için fiili standart haline gelmektedir ve SAML veya OpenID Connect gibi federasyon standartlarıyla birlikte kullanıldığında, yöneticilere erişim yönetimi için uçtan uca standartlara dayalı bir çözüm sağlar.

Kullanıcıların ve grupların bir uygulamaya sağlanması nı ve yok etmesini otomatikleştirmek için bir SCIM bitiş noktası geliştirme hakkında ayrıntılı kılavuz için [bkz.](use-scim-to-provision-users-and-groups.md) Galerideki önceden tümleşik uygulamalar için (Slack, Azure Databricks, Snowflake, vb.), geliştirici belgelerini atlayabilir ve [burada](../saas-apps/tutorial-list.md)sağlanan öğreticileri kullanabilirsiniz.

## <a name="manual-vs-automatic-provisioning"></a>El ile - otomatik sağlama karşılaştırması

Azure AD galerisindeki uygulamalar iki sağlama modundan birini destekler:

* **El ile** sağlama, uygulama için henüz otomatik Azure AD sağlama konektörü olmadığı anlamına gelir. Kullanıcı hesapları, örneğin kullanıcıları doğrudan uygulamanın yönetim portalına ekleyerek veya kullanıcı hesabı ayrıntılarıyla bir elektronik tablo yükleyerek el ile oluşturulmalıdır. Uygulama tarafından sağlanan belgelere başvurun veya hangi mekanizmaların kullanılabileni belirlemek için uygulama geliştiricisine başvurun.

* **Otomatik,** bu uygulama için bir Azure AD sağlama bağlayıcısı geliştirildiği anlamına gelir. Uygulama için sağlama ayarlamasına özgü kurulum öğreticisini izlemeniz gerekir. Uygulama [eğitimlerini, SaaS Uygulamalarının Azure Active Directory ile Nasıl Entegre Edilen Öğreticiler Listesi'nde](../saas-apps/tutorial-list.md)bulabilirsiniz.

Azure AD galerisinde, otomatik sağlamayı destekleyen uygulamalar bir **Sağlama** simgesi tarafından belirlenir. Bu simgeleri görmek için yeni galeri önizleme deneyimine geçin **(uygulama sayfası ekle'nin**üst kısmındaki başlıkta, **yeni ve geliştirilmiş uygulama galerisini denemek için buraya tıklayın**yazan bağlantıyı seçin).

![Uygulama galerisinde sağlama simgesi](./media/user-provisioning/browse-gallery.png)

Uygulamayı **Kurumsal uygulamalarınız**için ekledikten sonra, bir uygulama tarafından desteklenen sağlama modu, **Sağlama** sekmesinde de görünür.

## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Bir uygulamaya otomatik sağlamayı nasıl ayarlıyorum?

Galeride listelenen önceden tümleşik uygulamalar için, otomatik sağlama nın ayarlanması için adım adım kılavuzlur. Entegre [galeri uygulamaları için öğreticilerin listesine](../saas-apps/tutorial-list.md)bakın. Aşağıdaki video, SalesForce için otomatik kullanıcı sağlamanın nasıl ayarlanabildiğini göstermektedir.

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

SCIM 2.0'ı destekleyen diğer uygulamalar için, makalede [bir SCIM bitiş noktası oluşturun ve kullanıcı sağlamayı yapılandırın](use-scim-to-provision-users-and-groups.md)adımları izleyin.


## <a name="related-articles"></a>İlgili makaleler:

- [SaaS uygulamalarının nasıl entegre edilebildiğini anlatan öğreticiler listesi](../saas-apps/tutorial-list.md)
- [Kullanıcı sağlama için öznitelik eşlemelerini özelleştirme](customize-application-attributes.md)
- [Öznitelik eşlemeleri için ifadeler yazma](../app-provisioning/functions-for-customizing-application-data.md)
- [Kullanıcı sağlama için kapsam filtreleri](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)
- [SCIM bitiş noktası oluşturma ve kullanıcı sağlamayı yapılandırma](use-scim-to-provision-users-and-groups.md)
- [Azure AD senkronizasyon API'ye genel bakış](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
