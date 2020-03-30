---
title: Azure Active Directory B2C nedir?
description: Facebook, Google ve diğer kimlik sağlayıcılarıyla sosyal giriş de dahil olmak üzere uygulamalarınızdaki harici kimlikleri desteklemek için Azure Active Directory B2C'yi nasıl kullanabileceğinizi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 09/19/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 61b6d5ea903d00519c58556bc99da7065741a6e3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78184069"
---
# <a name="what-is-azure-active-directory-b2c"></a>Azure Active Directory B2C nedir?

Azure Active Directory B2C, hizmet olarak işletmeden müşteriye kimlik sağlar. Müşterileriniz, uygulamalarınız ve API'lerinize tek oturum açma erişimi elde etmek için tercih ettikleri sosyal, kurumsal veya yerel hesap kimliklerini kullanır.

![Azure AD B2C kimlik sağlayıcılarının ve akış aşağı uygulamalarının bilgi grafiği](./media/overview/azureadb2c-overview.png)

Azure Active Directory B2C (Azure AD B2C), milyonlarca kullanıcıyı ve günde milyarlarca kimlik doğrulamayı destekleyebilecek bir müşteri kimliği erişim yönetimi (CIAM) çözümüdür. Kimlik doğrulama platformunun ölçeklendirme ve güvenliğiyle ilgilenir, hizmet reddi, parola spreyi veya kaba kuvvet saldırıları gibi tehditleri izler ve otomatik olarak ele alır.

## <a name="custom-branded-identity-solution"></a>Özel markalı kimlik çözümü

Azure AD B2C bir beyaz etiketli kimlik doğrulama çözümüdür. Tüm kullanıcı deneyimini markanızla özelleştirebilir, böylece web ve mobil uygulamalarınızla sorunsuz bir şekilde uyum sağlayabilirsiniz.

Kullanıcılarınız kaydolurken, oturum açtıklarında ve profil bilgilerini değiştirirken Azure AD B2C tarafından görüntülenen her sayfayı özelleştirin. Kullanıcı yolculuklarınızda HTML, CSS ve JavaScript'i Azure AD B2C deneyiminin uygulamanızın yerel bir parçası ymış gibi görünmesi ve hissetmesi için özelleştirin.

![Özelleştirilmiş kayıt ve oturum açma sayfaları ve arka plan görüntüsü](./media/overview/sign-in-small.png)

## <a name="single-sign-on-access-with-a-user-provided-identity"></a>Kullanıcı tarafından sağlanan kimlikle tek oturum açma erişimi

Azure AD B2C, OpenID Connect, OAuth 2.0 ve SAML gibi standartlara dayalı kimlik doğrulama protokollerini kullanır. Bu en modern uygulamalar ve ticari off-the-raf yazılımı ile entegre.

![Azure AD B2C'ye aktarılanan üçüncü taraf kimliklerin diyagramı](./media/overview/scenario-singlesignon.png)

Azure AD B2C, web uygulamalarınız, mobil uygulamalarınız ve API'leriniz için merkezi kimlik doğrulama yetkilisi olarak hizmet vererek, hepsi için tek bir oturum açma (SSO) çözümü oluşturmanıza olanak tanır. Kullanıcı profili ve tercih bilgilerinin toplanmasını merkezileştirin ve oturum açma davranışı ve kaydolma dönüşümü hakkında ayrıntılı analizler yakalayın.

## <a name="integrate-with-external-user-stores"></a>Harici kullanıcı depolarıyla tümleştirme

Azure AD B2C, kullanıcı başına 100 özel öznitelik tutabilen bir dizin sağlar. Ancak, dış sistemlerle de entegre olabilirsiniz. Örneğin, kimlik doğrulaması için Azure AD B2C'yi kullanın, ancak müşteri verilerinin doğruluk kaynağı olarak harici bir müşteri ilişkileri yönetimi (CRM) veya müşteri sadakatveritabanına temsilci olarak temsilci olarak verin.

Başka bir harici kullanıcı deposu senaryosu, Azure AD B2C'nin uygulamanızın kimlik doğrulamasını işlemesini sağlamak, ancak kullanıcı profilini veya kişisel verileri depolayan harici bir sistemle bütünleştirmektir. Örneğin, bölgesel veya şirket içi veri depolama ilkeleri gibi veri ikamet gereksinimlerini karşılamak için.

![Azure AD B2C'nin harici bir kullanıcı mağazasıyla iletişim kurmasının mantıksal diyagramı](./media/overview/scenario-remoteprofile.png)

Azure AD B2C, kayıt veya profil düzenleme sırasında kullanıcıdan bilgi toplamayı kolaylaştırabilir ve ardından bu verileri dış sisteme aktarabilir. Daha sonra, gelecekteki kimlik doğrulamaları sırasında Azure AD B2C verileri dış sistemden alabilir ve gerekirse uygulamanıza gönderdiği kimlik doğrulama belirteci yanıtının bir parçası olarak içerebilir.

## <a name="progressive-profiling"></a>Aşamalı profil oluşturma

Başka bir kullanıcı yolculuğu seçeneği aşamalı profil oluşturma içerir. Aşamalı profil oluşturma, müşterilerinizin en az miktarda bilgi toplayarak ilk işlemlerini hızla tamamlamalarını sağlar. Ardından, gelecekteki oturum açma işlemlerinde müşteriden kademeli olarak daha fazla profil verisi toplayın.

![Aşamalı profillemenin görsel bir tasviri](./media/overview/scenario-progressive.png)

## <a name="third-party-identity-verification-and-proofing"></a>Üçüncü taraf kimlik doğrulama ve prova

Kullanıcı verilerini toplayarak kimlik doğrulamayı ve provayı kolaylaştırmak ve ardından kullanıcı hesabı oluşturma için doğrulama, güven puanlama ve onay gerçekleştirmek için üçüncü taraf bir sisteme aktarmak için Azure AD B2C'yi kullanın.

![Üçüncü taraf kimlik provası için kullanıcı akışını gösteren bir diyagram](./media/overview/scenario-idproofing.png)

Bunlar, azure AD B2C ile işletmeden müşteriye kimlik platformuolarak yapabileceğiniz şeylerden sadece bazılarıdır. Bu genel bakışın aşağıdaki bölümleri, Azure AD B2C kullanan bir demo uygulamasında size yol sunar. Ayrıca doğrudan [Azure AD B2C'nin](technical-overview.md)daha ayrıntılı teknik genel görünümüne geçebilirsiniz.

## <a name="example-woodgrove-groceries"></a>Örnek: WoodGrove Bakkaliye

[WoodGrove Groceries,][woodgrove] Microsoft tarafından çeşitli Azure AD B2C özelliklerini göstermek için oluşturulan canlı bir web uygulamasıdır. Sonraki birkaç bölüm, Azure AD B2C tarafından WoodGrove web sitesine sağlanan kimlik doğrulama seçeneklerinden bazılarını gözden geçirebilirsiniz.

### <a name="business-overview"></a>İş genel bakış

WoodGrove bireysel tüketiciler ve iş müşterilerine hem de bakkal satan bir online bakkal olduğunu. İş müşterileri, şirketleri veya yönettikleri işletmeler adına alışveriş satın alırlar.

### <a name="sign-in-options"></a>Oturum açma seçenekleri

WoodGrove Bakkaliye, müşterilerinin mağazayla olan ilişkisine dayalı çeşitli oturum açma seçenekleri sunar:

* **Bireysel** müşteriler, sosyal kimlik sağlayıcısı veya e-posta adresi ve parola gibi bireysel hesaplarla kaydolabilir veya oturum açabilir.
* **Kurumsal** müşteriler kurumsal kimlik bilgileriyle kaydolabilir veya oturum açabilir.
* **Ortaklar** ve tedarikçiler, markete satacak ürünler sağlayan kişilerdir. İş ortağı kimliği [Azure Active Directory B2B](../active-directory/b2b/what-is-b2b.md)tarafından sağlanır.

![Bireysel (B2C), iş (B2C) ve ortak (B2B) oturum açma sayfaları](./media/overview/woodgrove-overview.png)

### <a name="authenticate-individual-customers"></a>Bireysel müşterileri doğrula

Bir müşteri **kişisel hesabınızla Oturum Aç'ı**seçtiğinde, Azure AD B2C tarafından barındırılan özelleştirilmiş bir oturum açma sayfasına yönlendirilir. Aşağıdaki resimde, woodgrove market web sitesi gibi görünmek ve hissetmek için kullanıcı arabirimini (UI) özelleştirdiğimizi görebilirsiniz. WoodGrove müşterileri, kimlik doğrulama deneyiminin Azure AD B2C tarafından barındırılan ve güvenli hale geldiğinin farkında olmalıdır.

![Azure AD B2C tarafından barındırılan Özel WoodGrove oturum açma sayfası](./media/overview/sign-in.png)

WoodGrove, müşterilerinin Google, Facebook veya Microsoft hesaplarını kimlik sağlayıcısı olarak kullanarak kaydolmalarına ve oturum açmalarına olanak tanır. Veya, *yerel hesap*olarak adlandırılan bir hesap oluşturmak için e-posta adreslerini ve parolalarını kullanarak kaydolabilirler.

Bir müşteri **kişisel hesabınızla kaydolun** ve **şimdi kaydolun**seçtiğinde, özel bir kayıt sayfası yla sunulur.

![Azure AD B2C tarafından barındırılan Özel WoodGrove kayıt sayfası](./media/overview/sign-up.png)

Bir e-posta adresi girdikten ve **doğrulama kodu gönder'i**seçtikten sonra Azure AD B2C onlara kodu gönderir. Kodlarını girdikten sonra, **Kodu Doğrula'yı**seçin ve ardından formdaki diğer bilgileri girin, hizmet koşullarını da kabul etmeleri gerekir.

**Oluştur** düğmesini tıklatmak, Azure AD B2C'nin kullanıcıyı WoodGrove Marketler web sitesine yönlendirmesine neden olur. Yeniden yönlendirildiğinde, Azure AD B2C Bir OpenID Connect kimlik doğrulama belirtecinden WoodGrove web uygulamasına geçer. Kullanıcı artık oturum açmış ve kullanıma hazırdır, oturum açmış olduklarını belirtmek için sağ üst köşede görüntü adı gösterilir.

![WoodGrove Bakkaliye web sitesi başlığı kullanıcı gösteren oturum](./media/overview/signed-in-individual.png)

### <a name="authenticate-business-customers"></a>İş müşterilerini doğrulama

Bir müşteri **Business müşterilerine**bağlı seçeneklerden birini seçtiğinde, WoodGrove Marketler web sitesi bireysel müşterilere göre farklı bir Azure AD B2C ilkesi çağırır.

Bu ilke, kullanıcıya kaydolma ve kaydolma için şirket kimlik bilgilerini kullanma seçeneği sunar. WoodGrove örneğinde, kullanıcılardan herhangi bir Office 365 veya Azure AD hesabıyla oturum açmaları istenir. Bu ilke, dünyanın herhangi bir `/common` Office 365 müşterisi ile Azure AD B2C'yi fecofe etmek için [çok kiracılı](../active-directory/develop/howto-convert-app-to-be-multi-tenant.md) bir Azure AD uygulaması ve Azure AD bitiş noktası kullanır.

### <a name="authenticate-partners"></a>İş ortaklarını doğrula

**Tedarikçi hesap bağlantınızla oturum açma,** Azure Active Directory B2B'nin işbirliği işlevini kullanır. Azure AD B2B, azure Active Directory'de iş ortağı kimliklerini yönetmek için bir özellik ailesidir. Bu kimlikler, Azure AD B2C korumalı uygulamalara erişmek için Azure Active Directory'den yararlanılabilir.

[Azure Active Directory B2B'de konuk kullanıcı erişimi nedir?](../active-directory/b2b/what-is-b2b.md)

<!-- UNCOMMENT WHEN REPO IS UPDATED WITH LATEST DEMO CODE
### Sample code

If you'd like to jump right into the code to see how the WoodGrove Groceries application is built, you can find the repository on GitHub:

[Azure-Samples/active-directory-external-identities-woodgrove-demo][woodgrove-repo] (GitHub)
-->

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure AD B2C'nin ne olduğu ve yardımcı olabileceği bazı senaryolar hakkında bir fikriniz olduğuna göre, özellikleri ve teknik yönlerini biraz daha derinlemesine inceleyebilirsiniz.

> [!div class="nextstepaction"]
> [Azure AD B2C teknik genel bakış >](technical-overview.md)

<!-- LINKS - External -->
[woodgrove]: https://aka.ms/ciamdemo
[woodgrove-repo]: https://github.com/Azure-Samples/active-directory-external-identities-woodgrove-demo
