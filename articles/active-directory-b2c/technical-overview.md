---
title: Teknik ve özelliğe genel bakış - Azure Active Directory B2C
description: Azure Active Directory B2C'deki özellikler ve teknolojilere derinlemesine giriş.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 09/19/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: d3d6b33211f6f247d9f30c0f162b388085faabe6
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80332541"
---
# <a name="technical-and-feature-overview-of-azure-active-directory-b2c"></a>Azure Active Directory B2C'ye teknik ve özelliğe genel bakış

[Azure Etkin Dizin i (Hakkında B2C)](overview.md)bir arkadaşı olan bu makale, hizmete daha ayrıntılı bir giriş sağlar. Burada tartışılan birincil kaynaklar, hizmette birlikte çalıştığınız özellikler ve bunların uygulamalarınızda müşterileriniz için tamamen özel bir kimlik deneyimi sağlamanızı nasıl sağladığıdır.

## <a name="azure-ad-b2c-tenant"></a>Azure AD B2C kiracı

Azure Etkin Dizin B2C'de (Azure AD B2C) *kiracı* kuruluşunuzu temsil eder ve kullanıcıların dizinidir. Her Azure AD B2C kiracısı benzersizdir ve diğer Azure AD B2C kiracılarından ayrıdır. Azure AD B2C kiracısı, zaten sahip olabileceğiniz Bir Azure Etkin Dizin kiracıdan farklıdır.

Azure AD B2C kiracısında birlikte çalıştığınız birincil kaynaklar şunlardır:

* **Dizin** - *Dizini,* Azure AD B2C'nin kullanıcılarınızın kimlik bilgilerini ve profil verilerinin yanı sıra uygulama kayıtlarınızı depoladığı yerdir.
* **Uygulama kayıtları** - Kimlik yönetimini etkinleştirmek için web, mobil ve yerel uygulamalarınızı Azure AD B2C ile kaydedebilirsiniz. Ayrıca, Azure AD B2C ile korumak istediğiniz API'ler.
* **Kullanıcı akışları** ve **özel ilkeler** - Uygulamalarınız için yerleşik (kullanıcı akışları) ve tamamen özelleştirilebilir (özel ilkeler) kimlik deneyimleri.
  * Hızlı yapılandırma ve kaydolma, oturum açma ve profil düzenleme gibi yaygın kimlik görevlerini etkinleştirmek için *kullanıcı akışlarını* kullanın.
  * Yalnızca ortak kimlik görevleri için değil, aynı zamanda kuruluşunuz, müşterileriniz, çalışanlarınız, iş ortaklarınız ve vatandaşlarınıza özgü karmaşık kimlik iş akışları için destek oluşturma için de kullanıcı deneyimlerini etkinleştirmek için *özel ilkeler* kullanın.
* **Kimlik sağlayıcıları** - Federasyon ayarları:
  * Uygulamalarınızda desteklemek istediğiniz Facebook, LinkedIn veya Twitter gibi *sosyal* kimlik sağlayıcıları.
  * OAuth 2.0, OpenID Connect ve daha fazlası gibi standart kimlik protokollerini destekleyen *dış* kimlik sağlayıcıları.
  * Kullanıcıların bir kullanıcı adı (veya e-posta adresi veya başka bir kimlik) ve parolayla kaydolmasını ve oturum açmalarını sağlayan *yerel* hesaplar.
* **Anahtarlar** - Belirteçleri imzalamak ve doğrulamak için şifreleme anahtarları ekleyin ve yönetin.

Azure AD B2C kiracı, Azure AD B2C ile başlamak için oluşturmanız gereken ilk kaynaktır. Öğretici'de nasıl olduğunu [öğrenin: Azure Active Directory B2C kiracıoluşturun.](tutorial-create-tenant.md)

## <a name="accounts-in-azure-ad-b2c"></a>Azure AD B2C'deki hesaplar

Azure AD B2C çeşitli kullanıcı hesapları tanımlar. Azure Etkin Dizini, Azure Etkin Dizini B2B ve Azure Etkin Dizin B2C bu hesap türlerini paylaşır.

* **İş hesabı** - Çalışma hesabı olan kullanıcılar kiracıdaki kaynakları yönetebilir ve yönetici rolüyle kiracıları da yönetebilir. Çalışma hesabı olan kullanıcılar yeni tüketici hesapları oluşturabilir, parolaları sıfırlayabilir, hesapları engelleyebilir/kaldırabilir ve izinler ayarlayabilir veya bir güvenlik grubuna hesap atayabilir.
* **Misafir hesabı** - Kiracınıza misafir olarak davet ettiğiniz harici kullanıcılar. Konuk kullanıcıyı Azure AD B2C kiracınıza davet etmek için tipik bir senaryo, yönetim sorumluluklarını paylaşmaktır.
* Tüketici hesabı - Tüketici **hesapları,** kullanıcılar kiracınıza kaydettiğiniz bir uygulamada kaydolma kullanıcı yolculuğunu tamamladıklarında Azure AD B2C dizininizde oluşturulan hesaplardır.

![Azure portalında Azure AD B2C kullanıcı yönetimi sayfası](media/technical-overview/portal-01-users.png)<br/>*Şekil: Azure portalındaki bir Azure AD B2C kiracısındaki kullanıcı dizini*

### <a name="consumer-accounts"></a>Tüketici hesapları

Bir *tüketici* hesabıyla, kullanıcılar Azure AD B2C ile güvence altına aldığınız uygulamalarda oturum açabilir. Ancak, tüketici hesabı olan kullanıcılar Azure kaynaklarına (örneğin Azure portalı) erişemez.

Bir tüketici hesabı bu kimlik türleri ile ilişkilendirilebilir:

* Azure AD B2C dizininde yerel olarak depolanan kullanıcı adı ve parolayla **yerel** kimlik. Bu kimliklere genellikle "yerel hesaplar" diyoruz.
* Kullanıcının kimliğinin Facebook, Microsoft, ADFS veya Salesforce gibi federe bir kimlik sağlayıcısı tarafından yönetildiği **sosyal** veya **kurumsal** kimlikler.

Tüketici hesabı olan bir kullanıcı, kullanıcı adı, e-posta, çalışan kimliği, devlet kimliği ve diğerleri gibi birden çok kimlikle oturum açabilir. Tek bir hesabın hem yerel hem de sosyal olarak birden çok kimliği olabilir.

![Tüketici hesap kimlikleri](media/technical-overview/identities.png)<br/>*Şekil: Azure AD B2C'de birden fazla kimliği olan tek bir tüketici hesabı*

Azure AD B2C, görüntü adı, soyadı, verilen ad, şehir ve diğerleri gibi tüketici hesabı profillerinin ortak özniteliklerini yönetmenize olanak tanır. Ayrıca, kullanıcılarınız hakkında ek bilgiler depolamak için Azure AD şemasını da genişletebilirsiniz. Örneğin, ülkelerini veya ikametgahları, tercih ettikleri dil ve bir bültene abone olmak veya çok faktörlü kimlik doğrulamayı etkinleştirmek isteyip istemedikleri gibi tercihler.

[Azure Etkin Dizin Ii B2C'deki kullanıcı hesaplarına Genel Bakış'ta](user-overview.md)Azure AD B2C'deki kullanıcı hesabı türleri hakkında daha fazla bilgi edinin.

## <a name="external-identity-providers"></a>Harici kimlik sağlayıcıları

Azure AD B2C'yi, kullanıcıların harici sosyal veya kurumsal kimlik sağlayıcılarından (IdP) kimlik bilgileriyle uygulamanızda oturum açmalarına izin verecek şekilde yapılandırabilirsiniz. Azure AD B2C, Facebook, Microsoft hesabı, Google, Twitter ve OAuth 1.0, OAuth 2.0, OpenID Connect, SAML veya WS-Federation protokollerini destekleyen tüm kimlik sağlayıcılarını destekler.

![Harici kimlik sağlayıcıları](media/technical-overview/external-idps.png)

Dış kimlik sağlayıcı federasyonu ile, tüketicilerinize sadece uygulamanız için yeni bir hesap oluşturmak zorunda kalmadan mevcut sosyal veya kurumsal hesaplarıyla oturum açma olanağı sunabilirsiniz.

Azure AD B2C, kaydolma veya imzalama sayfasında, kullanıcının oturum açmak için seçebileceği harici kimlik sağlayıcılarının bir listesini sunar. Dış kimlik sağlayıcılardan birini seçtikten sonra, oturum açma işlemini tamamlamak üzere seçilen sağlayıcının web sitesine alınır (yönlendirilir). Kullanıcı başarılı bir şekilde kaydolduktan sonra, uygulamanızdaki hesabın kimlik doğrulaması için Azure AD B2C'ye geri döndürülür.

![Sosyal hesaplı mobil oturum açma örneği (Facebook)](media/technical-overview/external-idp.png)

Azure AD B2C'de kimlik sağlayıcılarını nasıl ekleyeceğiniz hakkında [bkz.](tutorial-add-identity-providers.md)

## <a name="identity-experiences-user-flows-or-custom-policies"></a>Kimlik deneyimleri: kullanıcı akışları veya özel ilkeler

Azure AD B2C'nin genişletilebilir ilke çerçevesi temel gücüdür. İlkeler, kullanıcılarınızın kaydolma, oturum açma ve profil düzenleme gibi kimlik deneyimlerini açıklar.

Azure AD B2C'de, bu kimlik deneyimlerini sağlamak için kullanabileceğiniz iki birincil yol vardır: kullanıcı akışları ve özel ilkeler.

* **Kullanıcı akışları,** dakikalar içinde kaydolma, kaydolma ve ilke düzenleme deneyimleri oluşturabilmeniz için sağladığımız önceden tanımlanmış, yerleşik, yapılandırılabilir ilkelerdir.

* **Özel ilkeler,** karmaşık kimlik deneyimi senaryoları için kendi kullanıcı yolculuklarınızı oluşturmanıza olanak tanır.

Hem kullanıcı akışları hem de özel *ilkeler, Azure*AD B2C'nin ilke düzenleme altyapısı olan Identity Experience Framework tarafından desteklenmektedir.

### <a name="user-flow"></a>Kullanıcı akışı

Azure portalı, en yaygın kimlik görevlerini hızlı bir şekilde ayarlamanıza yardımcı olmak için *kullanıcı akışları*adı verilen önceden tanımlanmış ve yapılandırılabilir birkaç ilke içerir.

Uygulamalarınızdaki kimlik deneyimi davranışlarını denetlemek için bu gibi kullanıcı akışı ayarlarını yapılandırabilirsiniz:

* Facebook gibi sosyal hesaplar veya oturum açma için e-posta adresi ve parola kullanan yerel hesaplar gibi oturum açma için kullanılan hesap türleri
* Tüketiciden toplanacak ad, posta kodu veya ikamet eden ülke gibi öznitelikler
* Azure Multi-Factor Authentication (MFA)
* Kullanıcı arabiriminin özelleştirmesi
* Kullanıcı kullanıcı akışını tamamladıktan sonra uygulamanızın aldığı bir belirteçteki talepler kümesi
* Oturum yönetimi
* ... ve daha fazlası.

Mobil, web ve tek sayfalı uygulamaların çoğu için en yaygın kimlik senaryoları kullanıcı akışları ile tanımlanabilir ve etkili bir şekilde uygulanabilir. Özel ilkelerin tam esnekliğini gerektiren karmaşık kullanıcı yolculuğu senaryolarınız yoksa yerleşik kullanıcı akışlarını kullanmanızı öneririz.

[Azure Active Directory B2C'deki Kullanıcı akışlarındaki](user-flow-overview.md)kullanıcı akışları hakkında daha fazla bilgi edinin.

### <a name="custom-policy"></a>Özel ilke

Özel ilkeler, Kimlik Deneyimi Çerçevesi (IEF) orkestrasyon motorunun tüm gücüne erişimin kilidini açar. Özel ilkelerle, hayal edebileceğiniz hemen hemen her kimlik doğrulama, kullanıcı kaydı veya profil düzenleme deneyimi oluşturmak için IEF'den yararlanabilirsiniz.

Kimlik Deneyimi Çerçevesi, herhangi bir adım kombinasyonuyla kullanıcı yolculukları oluşturma olanağı sağlar. Örnek:

* Diğer kimlik sağlayıcılarla feorate
* Birinci ve üçüncü taraf çok faktörlü kimlik doğrulama (MFA) zorlukları
* Herhangi bir kullanıcı girişi toplama
* REST API iletişimini kullanarak dış sistemlerle tümleştirme

Bu tür her kullanıcı yolculuğu bir ilke tarafından tanımlanır ve kuruluşunuz için en iyi kullanıcı deneyimini etkinleştirmek için istediğiniz kadar veya az sayıda ilke oluşturabilirsiniz.

![IEF tarafından etkinleştirilen karmaşık bir kullanıcı yolculuğunun örneğini gösteren diyagram](media/technical-overview/custom-policy.png)

Özel bir ilke, hiyerarşik bir zincirde birbirine başvuran birkaç XML dosyası tarafından tanımlanır. XML öğeleri, talep şemasını, talep dönüşümlerini, içerik tanımlarını, talep sağlayıcılarını, teknik profilleri, kullanıcı yolculuğu düzenleme adımlarını ve kimlik deneyiminin diğer yönlerini tanımlar.

Özel ilkelerin güçlü esnekliği, karmaşık kimlik senaryoları oluşturmanız gerektiğinde en uygun uyrabilir. Özel ilkeleri yapılandıran geliştiriciler, güvenilir ilişkileri meta veri uç noktalarını, tam talep değişimi tanımlarını içerecek şekilde dikkatli bir şekilde tanımlamalı ve her kimlik sağlayıcısının gerektirdiği sırları, anahtarları ve sertifikaları yapılandırmalıdır.

[Azure Active Directory B2C'de Özel ilkeler hakkında](custom-policy-overview.md)daha fazla bilgi edinin.

## <a name="protocols-and-tokens"></a>Protokoller ve belirteçler

Azure AD B2C, kullanıcı yolculukları için [OpenID Connect ve OAuth 2.0 protokollerini](protocols-overview.md) destekler. OpenID Connect'in Azure AD B2C gerçekleştirmesinde uygulamanız kullanıcı yolculuğunu Azure AD B2C'ye kimlik doğrulama istekleri göndererek başlatır.

Azure AD B2C'ye yapılan isteğin sonucu, kimlik [belirteci veya erişim belirteci](tokens-overview.md)gibi bir güvenlik belirtecidir. Bu güvenlik belirteci kullanıcının kimliğini tanımlar. Belirteçler, Azure AD B2C uç `/token` `/authorize` noktalarından veya bitiş noktası gibi alınır. Bu belirteçlerle, bir kimliği doğrulamak ve güvenli kaynaklara erişime izin vermek için kullanılabilecek taleplere erişebilirsiniz.

Azure AD B2C, dış kimlikler için herhangi bir OAuth 1.0, OAuth 2.0, OpenID Connect, SAML ve WS-Fed kimlik sağlayıcısıyla federasyonu destekler.

![SAML tabanlı IdP ile federating OIDC tabanlı istemci uygulaması diyagramı](media/technical-overview/protocols.png)

Önceki diyagram, Azure AD B2C'nin aynı kimlik doğrulama akışı içinde çeşitli protokoller kullanarak nasıl iletişim kurabileceğini gösterir:

1. Güvenilen taraf uygulaması, OpenID Connect'i kullanarak Azure AD B2C'ye bir yetkilendirme isteği başlatır.
1. Uygulamanın bir kullanıcısı SAML protokolünü kullanan harici bir kimlik sağlayıcısı kullanarak oturum açmayı seçtiğinde, Azure AD B2C bu kimlik sağlayıcısıyla iletişim kurmak için SAML protokolünü çağırır.
1. Kullanıcı oturum açma işlemini harici kimlik sağlayıcısıyla tamamladıktan sonra, Azure AD B2C belirteci OpenID Connect'i kullanarak bağlı taraf uygulamasına döndürür.

## <a name="application-integration"></a>Uygulama tümleştirme

Bir kullanıcı, ister web, mobil, masaüstü veya ister tek sayfalık bir uygulama (SPA) olsun, uygulamanızda oturum imzalamak istediğinde, uygulama kullanıcı akışına veya özel ilke tarafından sağlanan bitiş noktasına yetkilendirme isteği başlatır. Kullanıcı akışı veya özel ilke, kullanıcının deneyimini tanımlar ve denetler. Bir kullanıcı akışını (örneğin *kaydolma veya oturum açma* akışı) tamamladıklarında, Azure AD B2C bir belirteç oluşturur ve kullanıcıyı uygulamanıza geri yönlendirir.

![Azure AD B2C oturum açma sayfası arasındaki akışı gösteren oklara sahip mobil uygulama](media/technical-overview/app-integration.png)

Birden çok uygulama aynı kullanıcı akışını veya özel ilkesini kullanabilir. Tek bir uygulama birden çok kullanıcı akışı veya özel ilkeler kullanabilir.

Örneğin, bir uygulamada oturum açmak için uygulama *kaydolma veya kullanıcı* akışını oturum açma kullanır. Kullanıcı oturum açtıktan sonra, profillerini yeniden deletmek isteyebilir, bu nedenle uygulama bu kez *profil edit* kullanıcı akışını kullanarak başka bir yetkilendirme isteği başlatır.

## <a name="seamless-user-experiences"></a>Sorunsuz kullanıcı deneyimleri

Azure AD B2C'de, kullanıcılarınızın kimlik deneyimlerini, gösterildiği sayfaların markanızın görünümü ve hissi ile sorunsuz bir şekilde harmanlanması için işleyebilirsiniz. Uygulamanızın kimlik yolculuklarında kullanıcılarınıza sunulan HTML ve CSS içeriğinin neredeyse tam kontrolünü elde edersiniz. Bu esneklikle, uygulamanızla Azure AD B2C arasında marka ve görsel tutarlılığı koruyabilirsiniz.

![Markaya özel kayıt sayfasının ekran görüntüleri](media/technical-overview/seamless-ux.png)

Kullanıcı arabirimi özelleştirmesi hakkında bilgi için Azure [Active Directory B2C'deki kullanıcı arabirimi özelleştirmesi hakkında](customize-ui-overview.md)bilgi .

## <a name="localization"></a>Yerelleştirme

Azure AD B2C'deki dil özelleştirmesi, müşteri gereksinimlerinize uygun farklı dilleri barındırmanıza olanak tanır. Microsoft 36 dil için çeviri sağlar, ancak herhangi bir dil için kendi çevirilerinizi de sağlayabilirsiniz. Deneyiminiz yalnızca tek bir dil için sağlanmış olsa bile, sayfalardaki herhangi bir metni özelleştirebilirsiniz.

![Farklı dillerde Kullanıcı Arası Özel Kullanıcı Birleşik metnini güsteren üç oturum kaydolma sayfası](media/technical-overview/localization.png)

[Azure Etkin Dizin B2C'de Dil özelleştirmede yerelleştirmenin](user-flow-language-customization.md)nasıl çalıştığını görün.

## <a name="add-your-own-business-logic"></a>Kendi iş mantığınızı ekleme

Özel ilkeler kullanmayı seçerseniz, yolculukiçin kendi iş mantığınızı eklemek için bir kullanıcı yolculuğunda restful API ile tümleştirebilirsiniz. Örneğin, Azure AD B2C, bir RESTful hizmetiyle veri alışverişinde bulunabilir:

* Özel kullanıcı dostu hata iletileri görüntüleyin.
* Hatalı biçimlendirilmiş verilerin kullanıcı dizininizde kalıcı olmasını önlemek için kullanıcı girişini doğrulayın. Örneğin, kullanıcı tarafından girilen verileri değiştirebilirsiniz( örneğin, tüm küçük harfe girdiklerinde ilk adlarını büyük harfe küçültmek gibi).
* Kurumsal iş hattı uygulamanızla daha fazla entegre olarak kullanıcı verilerini zenginleştirin.
* YENIDEN çağrı larını kullanarak anında iletme bildirimleri gönderebilir, şirket veritabanlarını güncelleştirebilir, kullanıcı geçiş işlemini çalıştırabilir, izinleri, denetim veritabanlarını ve daha fazlasını yönetebilirsiniz.

Sadakat programları, Azure AD B2C'nin REST API'lerini arama desteği tarafından etkinleştirilen başka bir senaryodur. Örneğin, RESTful hizmetiniz bir kullanıcının e-posta adresini alabilir, müşteri veritabanınızı sorgulayabilir ve ardından kullanıcının sadakat numarasını Azure AD B2C'ye döndürebilir. İade verileri Azure AD B2C'deki kullanıcının dizin hesabında depolanabilir, daha sonra ilkedeki sonraki adımlarda daha fazla değerlendirilebilir veya erişim belirtecine dahil edilebilir.

![Mobil uygulamada iş yeri entegrasyonu](media/technical-overview/lob-integration.png)

Özel bir ilke ile tanımlanan kullanıcı yolculuğundaki herhangi bir adımda bir REST API çağrısı ekleyebilirsiniz. Örneğin, REST API'yi arayabilirsiniz:

* Oturum açma sırasında, Azure AD B2C kimlik bilgilerini doğrulamadan hemen önce
* Oturum açmadan hemen sonra
* Azure AD B2C dizinde yeni bir hesap oluşturmadan önce
* Azure AD B2C dizinde yeni bir hesap oluşturduktan sonra
* Azure AD B2C erişim jetonu vermeden önce

Azure AD B2C'de YENIDEN ödeneğe API tümleştirmesi için özel ilkeler nasıl kullanılacağını görmek için [bkz.](custom-policy-rest-api-intro.md)

## <a name="protect-customer-identities"></a>Müşteri kimliklerini koruma

Azure AD B2C, [Microsoft Azure Güven Merkezi'nde](https://www.microsoft.com/trustcenter/cloudservices/azure)açıklanan güvenlik, gizlilik ve diğer taahhütlere uygundur.

Oturumlar, şifre çözme anahtarı yalnızca Azure AD B2C Güvenlik Belirteç Hizmeti tarafından bilinen şifreli veriler olarak modellenir. Güçlü bir şifreleme algoritması, AES-192, kullanılır. Tüm iletişim yolları gizlilik ve bütünlük için TLS ile korunmaktadır. Güvenlik Belirteci Hizmetimiz TLS için Genişletilmiş Doğrulama (EV) sertifikası kullanır. Genel olarak, Güvenlik Belirteci Hizmeti, güvenilmeyen giriş oluşturmayarak siteler arası komut dosyası oluşturma (XSS) saldırılarını azaltır.

![Aktarım ve istirahatte güvenli veri diyagramı](media/technical-overview/user-data.png)

### <a name="access-to-user-data"></a>Kullanıcı verilerine erişim

Azure AD B2C kiracıları, çalışanlar ve iş ortakları için kullanılan kurumsal Azure Etkin Dizin kiracılarıyla birçok özelliği paylaşır. Paylaşılan hususlar arasında yönetim rollerini görüntüleme, roller atama ve denetim etkinlikleri yer alır.

Şunları da dahil olmak üzere Azure AD B2C'de belirli yönetim eylemlerini kimlerin gerçekleştirebileceğini denetlemek için roller atayabilirsiniz:

* Kullanıcı akışlarının tüm yönlerini oluşturma ve yönetme
* Tüm kullanıcı akışlarıiçin kullanılabilir öznitelik şemasını oluşturma ve yönetme
* Kimlik sağlayıcılarını doğrudan federasyonda kullanılmak üzere yapılandırma
* Kimlik Deneyimi Çerçevesi'nde güven çerçevesi ilkeleri oluşturma ve yönetme (özel ilkeler)
* Kimlik Deneyimi Çerçevesi'nde (özel ilkeler) federasyon ve şifreleme yle ilgili sırları yönetme

Azure AD B2C yönetim rol desteği de dahil olmak üzere Azure AD rolleri hakkında daha fazla bilgi için [Azure Etkin Dizin'de Yönetici rol izinlerine](../active-directory/users-groups-roles/directory-assign-admin-roles.md)bakın.

### <a name="multi-factor-authentication-mfa"></a>Çok faktörlü kimlik doğrulaması (MFA)

Azure AD B2C çok faktörlü kimlik doğrulama (MFA), kullanıcılarınız için basitliği korurken verilere ve uygulamalara erişimi korumaya yardımcı olur. İkinci bir kimlik doğrulama biçimi gerektirerek ek güvenlik sağlar ve bir dizi kullanımı kolay kimlik doğrulama yöntemi sunarak güçlü kimlik doğrulaması sağlar. Bir yönetici olarak yapabileceğiniz yapılandırma kararlarına dayanarak kullanıcılarınız MFA için meydan olabilir veya olmayabilir.

[Azure Active Directory B2C'de çok faktörlü kimlik doğrulamayı etkinleştir'de](custom-policy-multi-factor-authentication.md)kullanıcı akışlarında MFA'yı nasıl etkinleştireceğimizin ne olduğunu görün.

### <a name="smart-account-lockout"></a>Akıllı hesap kilitleme

Kaba kuvvet li parola tahmin girişimlerini önlemek için Azure AD B2C, isteklerin IP'sine, girilen parolalara ve diğer çeşitli etkenlere bağlı olarak hesapları kilitlemek için gelişmiş bir strateji kullanır. Kilitleme süresi, riske ve deneme sayısına bağlı olarak otomatik olarak artırılır.

![Hesap akıllı kilitleme](media/technical-overview/smart-lockout1.png)

Parola koruma ayarlarını yönetme hakkında daha fazla bilgi için azure [Active Directory B2C'deki kaynaklara ve verilere yönelik tehditleri yönet'e](threat-management.md)bakın.

### <a name="password-complexity"></a>Parola karmaşıklığı

Kaydolma veya parola sıfırlama sırasında, kullanıcılarınızın karmaşıklık kurallarına uygun bir parola sağlaması gerekir. Varsayılan olarak, Azure AD B2C güçlü bir parola ilkesi uygular. Azure AD B2C, müşterilerinizin kullandığı parolaların karmaşıklık gereksinimlerini belirtmek için yapılandırma seçenekleri de sağlar.

Parola karmaşıklığı gereksinimlerini hem kullanıcı [akışlarında](user-flow-password-complexity.md) hem de [özel ilkelerde](custom-policy-password-complexity.md)yapılandırabilirsiniz.

## <a name="auditing-and-logs"></a>Denetim ve günlükler

Azure AD B2C, kaynakları, verilen belirteçler ve yönetici erişimi hakkında etkinlik bilgileri içeren denetim günlükleri yayar. Platform etkinliğini anlamak ve sorunları tanılamak için bu denetim günlüklerini kullanabilirsiniz. Denetim günlüğü girişleri, olayı oluşturan etkinlik gerçekleştikten kısa bir süre sonra kullanılabilir.

Azure AD B2C kiracınız veya belirli bir kullanıcı için kullanılabilen bir denetim günlüğünde aşağıdakiler de dahil olmak üzere bilgiler bulabilirsiniz:

* Bir kullanıcının B2C kaynaklarına erişim yetkisiyle ilgili etkinlikler (örneğin, B2C ilkelerinin listesine erişen bir yönetici)
* Bir yönetici Azure portalını kullanarak imzaattığında alınan dizin öznitelikleriyle ilgili etkinlikler
* B2C uygulamalarında (CRUD) işlemler oluşturma, okuma, güncelleme ve silme (CRUD) işlemleri
* B2C anahtar kabında depolanan tuşlarda CRUD işlemleri
* B2C kaynaklarında CRUD işlemleri (örneğin, ilkeler ve kimlik sağlayıcıları)
* Kullanıcı kimlik bilgilerinin doğrulanması ve belirteç verme

![Azure portalında gösterilen bireysel kullanıcı denetim günlüğü](media/technical-overview/audit-log.png)

Denetim günlükleri hakkında ek ayrıntılar için [bkz.](view-audit-logs.md)

### <a name="usage-insights"></a>Kullanım bilgileri

Azure AD B2C, kullanıcıların web uygulamanıza ne zaman kaydolduğunu veya oturum açtıklarını, kullanıcılarınızın nerede bulunduğunu ve hangi tarayıcıları ve işletim sistemlerini kullandıklarını keşfetmenize olanak tanır. Azure Uygulama Öngörülerini özel ilkeler kullanarak Azure AD B2C'ye entegre ederek, insanların nasıl kaydolduğu, oturum açtığı, parolalarını sıfırlama veya profillerini nasıl düzenlediği hakkında bilgi edinebilirsiniz. Bu tür bilgilerle, yaklaşan geliştirme döngüleriniz için veri tabanlı kararlar alabilirsiniz.

[Uygulama Öngörülerini kullanarak Azure Active Directory B2C'de Kullanıcı davranışını takip edin'de](analytics-with-application-insights.md)kullanım analitiği hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Active Directory B2C'nin özelliklerini ve teknik yönlerini daha iyi gördüğünüze göre, bir B2C kiracısı oluşturarak hizmete başlayın:

> [!div class="nextstepaction"]
> [Öğretici: Azure Etkin Dizin B2C kiracı >oluşturma](tutorial-create-tenant.md)