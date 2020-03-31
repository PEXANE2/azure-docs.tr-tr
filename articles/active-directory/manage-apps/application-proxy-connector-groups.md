---
title: Bağlayıcı grupları aracılığıyla uygulamaları ayrı ağlarda yayımlama - Azure AD
description: Azure AD Application Proxy'de bağlayıcı gruplarının nasıl oluşturulup yönetilenleri kapsar.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 22fa1de0a0e3bb91480212381e07b17875bf0bf4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74275574"
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Bağlayıcı gruplarını kullanarak uygulamaları ayrı ağlarda ve konumlarda yayımlama

Müşteriler, giderek daha fazla senaryo ve uygulama için Azure AD'nin Uygulama Proxy'si'ni kullanır. Bu nedenle, daha fazla topoloji sağlayarak App Proxy'yi daha da esnek hale getirdik. Belirli uygulamalara hizmet etmek için belirli bağlayıcılar atayabilmek için Uygulama Proxy bağlayıcı grupları oluşturabilirsiniz. Bu özellik, daha fazla denetim ve Uygulama Proxy dağıtım optimize etmek için yollar sağlar.

Her Uygulama Proxy bağlayıcısı bir bağlayıcı grubuna atanır. Aynı konektör grubuna ait tüm konnektörler, yüksek kullanılabilirlik ve yük dengelemesi için ayrı bir birim olarak hareket eder. Tüm bağlayıcılar bir bağlayıcı grubuna aittir. Gruplar oluşturmazsanız, tüm bağlayıcılarınız varsayılan bir gruptadır. Yöneticiniz Azure portalında yeni gruplar oluşturabilir ve bunlara bağlayıcılar atayabilir.

Tüm uygulamalar bir bağlayıcı grubuna atanır. Gruplar oluşturmazsanız, tüm uygulamalarınız varsayılan bir gruba atanır. Ancak bağlayıcılarınızı gruplar halinde düzenlerseniz, her uygulamayı belirli bir bağlayıcı grubuyla çalışacak şekilde ayarlayabilirsiniz. Bu durumda, yalnızca bu gruptaki bağlayıcılar istek üzerine uygulamaya hizmet eder. Bu özellik, uygulamalarınız farklı konumlarda barındırılıyorsa yararlıdır. Uygulamalarher zaman fiziksel olarak kendilerine yakın bağlayıcılar tarafından sunulan böylece konuma göre bağlayıcı grupları oluşturabilirsiniz.

> [!TIP]
> Büyük bir Uygulama Proxy dağıtımınız varsa, varsayılan bağlayıcı grubuna herhangi bir uygulama atamayın. Bu şekilde, siz onları etkin bir bağlayıcı grubuna atayınakadar yeni bağlayıcılar canlı trafik almaz. Bu yapılandırma ayrıca, kullanıcılarınızı etkilemeden bakım gerçekleştirebilmeniz için bağlayıcıları varsayılan gruba geri taşıyarak boşta bir moda koymanızı da sağlar.

## <a name="prerequisites"></a>Ön koşullar

Konektörlerinizi gruplandırmak için birden [çok konektör yüklediğinizden](application-proxy-add-on-premises-application.md)emin olun. Yeni bir bağlayıcı yüklediğinizde, **varsayılan** bağlayıcı grubuna otomatik olarak katılır.

## <a name="create-connector-groups"></a>Bağlayıcı grupları oluşturma

İstediğinizi kadar bağlayıcı grubu oluşturmak için bu adımları kullanın.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Azure Active Directory**Enterprise uygulamalarını** > seçin**Uygulama proxy'si.** **Azure Active Directory** > 
1. **Yeni bağlayıcı grubunu**seçin. Yeni Bağlayıcı Grubu bıçağı görüntülenir.

   ![Yeni bir bağlayıcı grubu seçmek için ekranı gösterir](./media/application-proxy-connector-groups/new-group.png)

1. Yeni bağlayıcı grubunuzun adını verin ve bu gruba hangi bağlayıcıların ait olduğunu seçmek için açılır menüyü kullanın.
1. **Kaydet'i**seçin.

## <a name="assign-applications-to-your-connector-groups"></a>Konektör gruplarınıza uygulama atama

Uygulama Proxy ile yayımladınız her uygulama için bu adımları kullanın. Bir uygulamayı ilk yayımladığınızda bağlayıcı gruba atayabilirsiniz veya atamayı istediğiniz zaman değiştirmek için bu adımları kullanabilirsiniz.

1. Dizininizin yönetim panosundan Kurumsal **uygulamaları** > seçin**Tüm uygulamalar** > bir bağlayıcı grubuna atamak istediğiniz uygulama > **Application Proxy'yi**seçin.
1. Uygulamanın kullanmasını istediğiniz grubu seçmek için **Bağlayıcı Grubu** açılır menüsünü kullanın.
1. Değişikliği uygulamak için **Kaydet'i** seçin.

## <a name="use-cases-for-connector-groups"></a>Konektör grupları için servis taleplerini kullanma

Bağlayıcı grupları aşağıdakiler dahil olmak üzere çeşitli senaryolar için yararlıdır:

### <a name="sites-with-multiple-interconnected-datacenters"></a>Birden çok birbirine bağlı veri merkezi olan siteler

Birçok kuruluşun birbirine bağlı veri merkezleri var. Bu durumda, veri merkezi arası bağlantılar pahalı ve yavaş olduğundan, veri merkezi içinde mümkün olduğunca çok trafik tutmak istiyorsunuz. Bağlayıcıları yalnızca veri merkezi içinde bulunan uygulamalara hizmet etmek üzere her veri merkezine dağıtabilirsiniz. Bu yaklaşım, veri merkezi arasındaki bağlantıları en aza indirir ve kullanıcılarınız için tamamen şeffaf bir deneyim sağlar.

### <a name="applications-installed-on-isolated-networks"></a>Yalıtılmış ağlara yüklenen uygulamalar

Uygulamalar ana şirket ağının parçası olmayan ağlarda barındırılabilir. Uygulamaları ağa yalıtmak için yalıtılmış ağlara özel bağlayıcılar yüklemek için bağlayıcı gruplarını kullanabilirsiniz. Bu genellikle bir üçüncü taraf satıcı kuruluşunuz için belirli bir uygulama tutar.

Bağlayıcı grupları, yalnızca belirli uygulamaları yayımlayan bu ağlar için özel bağlayıcılar yüklemenize olanak sağlayarak, uygulama yönetimini üçüncü taraf satıcılara dış kaynak sağlamayı daha kolay ve güvenli hale getirir.

### <a name="applications-installed-on-iaas"></a>IaaS'de yüklü uygulamalar

Bulut erişimi için IaaS'ye yüklenen uygulamalarda bağlayıcı gruplar, tüm uygulamalara erişimi güvence altına almak için ortak bir hizmet sağlar. Bağlayıcı grupları şirket ağınızda ek bağımlılık oluşturmaz veya uygulama deneyimini parçalamayın. Bağlayıcılar her bulut veri merkezine yüklenebilir ve yalnızca bu ağda bulunan uygulamalara hizmet verebilir. Yüksek kullanılabilirlik elde etmek için birkaç bağlayıcı yükleyebilirsiniz.

Örnek olarak, kendi IaaS barındırılan sanal ağına bağlı birkaç sanal makinesi olan bir kuruluş alalım. Çalışanların bu uygulamaları kullanmasına izin vermek için, bu özel ağlar siteden siteye VPN kullanarak şirket ağına bağlanır. Bu, şirket içinde bulunan çalışanlar için iyi bir deneyim sağlar. Ancak, aşağıdaki diyagramda görebileceğiniz gibi, erişimi yönlendirmek için ek şirket içi altyapı gerektirdiğinden, uzak çalışanlar için ideal olmayabilir:

![Azure AD IaaS ağını gösteren diyagram](./media/application-proxy-connector-groups/application-proxy-iaas-network.png)
  
Azure AD Application Proxy bağlayıcı gruplarıyla, şirket ağınıza ek bağımlılık yaratmadan tüm uygulamalara erişimi güvence altına almak için ortak bir hizmeti etkinleştirebilirsiniz:

![Azure AD IaaS Çoklu Bulut Satıcıları](./media/application-proxy-connector-groups/application-proxy-multiple-cloud-vendors.png)

### <a name="multi-forest--different-connector-groups-for-each-forest"></a>Çok ormanlı – her orman için farklı konektör grupları

Uygulama Proxy'yi dağıtan çoğu müşteri, Kerberos Kısıtlı Temsilciliği (KCD) gerçekleştirerek tek oturum açma (SSO) yeteneklerini kullanır. Bunu başarmak için bağlayıcının makinelerinin kullanıcıları uygulamaya yönlendirebilecek bir etki alanına katılması gerekir. KCD, ormanlar arası yetenekleri destekler. Ancak aralarında güven olmayan farklı çok ormanlı ortamlara sahip şirketler için, tüm ormanlar için tek bir konektör kullanılamaz. 

Bu durumda, orman başına belirli bağlayıcılar dağıtılabilir ve yalnızca belirli ormanın kullanıcılarına hizmet vermek üzere yayımlanmış uygulamalara hizmet etmek üzere ayarlanabilir. Her bağlayıcı grubu farklı bir ormanı temsil eder. Kiracı ve deneyimin çoğu tüm ormanlar için birleştirilmiş olsa da, kullanıcılar Azure REKLAM gruplarını kullanarak orman uygulamalarına atanabilir.

### <a name="disaster-recovery-sites"></a>Olağanüstü Durum Kurtarma siteleri

Sitelerinizin nasıl uygulandığına bağlı olarak, bir olağanüstü durum kurtarma (DR) sitesiyle uygulayabileceğiniz iki farklı yaklaşım vardır:

* DR siteniz tam olarak ana site gibi aktif-aktif modda yerleşikse ve aynı ağ ve AD ayarlarına sahipse, DR sitesindeki bağlayıcıları ana siteyle aynı bağlayıcı grupta oluşturabilirsiniz. Bu, Azure AD'nin sizin için arızaları algılamasını sağlar.
* DR siteniz ana siteden ayrıysa, DR sitesinde farklı bir bağlayıcı grubu oluşturabilir ve 1) yedek uygulamalara sahip veya 2) mevcut uygulamayı gerektiğinde DR bağlayıcı grubuna el ile yönlendirebilirsiniz.

### <a name="serve-multiple-companies-from-a-single-tenant"></a>Tek bir kiracıdan birden fazla şirkete hizmet

Tek bir hizmet sağlayıcısının birden çok şirket için Azure AD ile ilgili hizmetleri dağıtıp koruduğu bir modeli uygulamanın birçok farklı yolu vardır. Bağlayıcı grupları, yöneticinin bağlayıcıları ve uygulamaları farklı gruplara ayırmasına yardımcı olur. Küçük şirketler için uygun olan bir yol, farklı şirketlerin kendi etki alanı adı ve ağları varken tek bir Azure AD kiracısına sahip olmaktır. Bu, M&Tek bir BT bölümünün düzenleyici veya iş nedenleriyle birden fazla şirkete hizmet ettiği senaryolar ve durumlar için de geçerlidir.

## <a name="sample-configurations"></a>Örnek yapılandırmalar

Uygulayabileceğiniz bazı örnekler, aşağıdaki bağlayıcı grupları içerir.

### <a name="default-configuration--no-use-for-connector-groups"></a>Varsayılan yapılandırma – bağlayıcı grupları için kullanılmaz

Konektör gruplarını kullanmıyorsanız, yapılandırmanız şu şekilde görünür:

![Örnek Azure AD Hayır Bağlayıcı Grupları](./media/application-proxy-connector-groups/application-proxy-sample-config-1.png)

Bu yapılandırma küçük dağıtımlar ve testler için yeterlidir. Kuruluşunuzun düz bir ağ topolojisi varsa da iyi çalışacaktır.

### <a name="default-configuration-and-an-isolated-network"></a>Varsayılan yapılandırma ve yalıtılmış ağ

Bu yapılandırma, IaaS sanal ağı gibi yalıtılmış bir ağda çalışan belirli bir uygulamanın bulunduğu varsayılan uygulamanın evrimidir:

![Örnek Azure AD Bağlantı Yok Grupları ve yalıtılmış ağ](./media/application-proxy-connector-groups/application-proxy-sample-config-2.png)

### <a name="recommended-configuration--several-specific-groups-and-a-default-group-for-idle"></a>Önerilen yapılandırma – birkaç özel grup ve boşta kalan varsayılan grup

Büyük ve karmaşık kuruluşlar için önerilen yapılandırma, varsayılan bağlayıcı grubuna herhangi bir uygulamaya hizmet vermeyen ve boşta veya yeni yüklenen bağlayıcılar için kullanılan bir grup olarak sahip olmaktır. Tüm uygulamalar özelleştirilmiş bağlayıcı grupları kullanılarak sunulur. Bu, yukarıda açıklanan senaryoların tüm karmaşıklığını sağlar.

Aşağıdaki örnekte, şirketin her siteye hizmet veren iki bağlayıcısı olan A ve B olmak üzere iki veri merkezi vardır. Her siteüzerinde çalışan farklı uygulamalar vardır.

![2 veri merkezi ve 2 konektöre sahip şirket örneği](./media/application-proxy-connector-groups/application-proxy-sample-config-3.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD Uygulama Proxy bağlayıcılarını anlama](application-proxy-connectors.md)
* [Çoklu oturum açmayı etkinleştirme](what-is-single-sign-on.md)
