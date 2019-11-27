---
title: Azure AD Uygulama Ara Sunucusu joker karakter uygulamaları
description: Joker karakter içeren uygulamalar Azure Active Directory Uygulama proxy kullanmayı öğrenin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2018
ms.author: mimart
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5a9e7be5f582051e03cba08733fcbfa697cc8f5
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74275040"
---
# <a name="wildcard-applications-in-the-azure-active-directory-application-proxy"></a>Azure Active Directory Uygulama proxy'si joker karakteri uygulamalarında

Azure Active Directory'de çok sayıda şirket içi yapılandırma (Azure AD), uygulamaları hızla yönetilemeyen olabilir ve birçoğu aynı ayarları gerekiyorsa yapılandırma hataları için gereksiz risklerini de beraberinde getirir. [Azure AD uygulama ara sunucusu](application-proxy.md)ile, aynı anda birçok uygulamayı yayımlamak ve yönetmek için joker karakter uygulama yayımlamasını kullanarak bu sorunu ele alabilirsiniz. Bu olanak tanıyan bir çözüm.

- Yönetim yükünüzü basitleştirin
- Olası yapılandırma hatalarını sayısını azaltın
- Daha fazla kaynaklarına güvenle erişmesini sağlayın

Bu makalede, ortamınızda joker uygulama yayımlamayı yapılandırmak gereken bilgileri sağlar.

## <a name="create-a-wildcard-application"></a>Bir joker uygulama oluşturma

Aynı yapılandırmaya sahip uygulamalar grubunuz varsa, bir joker (*) uygulama oluşturabilirsiniz. Bir joker uygulama için potansiyel adaylar, aşağıdaki ayarları paylaşan uygulamalar şunlardır:

- Onlara yönelik erişimi olan kullanıcı grubu
- SSO yöntemi
- Erişim Protokolü (http, https)

Hem, iç ve dış URL'leri şu biçimde olması durumunda joker karakterler içeren uygulamalar yayımlayabilmeniz için:

> http (s)://*.\<etki alanı\>

Örneğin: `http(s)://*.adventure-works.com`.

İç ve dış URL, en iyi uygulama farklı etki alanlarını kullanabilirsiniz, ancak bunlar aynı olmalıdır. URL'lerinden biri joker karakter yoksa, uygulama yayımlama sırasında bir hata görürsünüz.

Farklı yapılandırma ayarları ile ek uygulamalarınız varsa, bu özel durumlar için joker karakter belirlenen Varsayılanların üzerine yazmak için ayrı uygulamalar olarak yayımlamanız gerekir. Joker karakter olmayan uygulamalar joker uygulamalar her zaman önceliklidir. Yapılandırma açısından bakıldığında, bunlar "Yeni" Normal uygulamalardır.

Joker bir uygulama oluşturmak, diğer tüm uygulamalar için kullanılabilir olan aynı [uygulama yayımlama akışını](application-proxy-add-on-premises-application.md) temel alır. Tek fark, URL'leri ve potansiyel olarak SSO yapılandırmasını bir joker karakter dahil edilir.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için, bu gereksinimleri karşıladığınızdan emin olun.

### <a name="custom-domains"></a>Özel etki alanları

[Özel etki alanları](application-proxy-configure-custom-domain.md) diğer tüm uygulamalar için isteğe bağlı olsa da, joker uygulamalar için bir önkoşuldur. Özel etki alanları oluşturmak, gerektirir:

1. Azure 'da doğrulanmış bir etki alanı oluşturun.
1. Bir SSL sertifikası PFX biçimi, uygulama ara sunucusuna yükleyin.

Uygulama oluşturmayı planladığınız eşleştirmek için joker karakter sertifika kullanmayı düşünmeniz gerekir. Alternatif olarak, yalnızca belirli uygulamaların listeleyen bir sertifika kullanabilirsiniz. Bu durumda, yalnızca sertifikasında listelenen uygulamalar bu joker uygulama erişilebilir.

Güvenlik nedeniyle, bu bir gereksinim ve joker karakter özel etki alanı için dış URL'yi kullanamayan uygulamalar için destekleyeceğiz değil.

### <a name="dns-updates"></a>DNS güncelleştirmeleri

Özel etki alanları kullanırken, dış URL için CNAME kaydıyla bir DNS girişi oluşturmanız gerekir (örneğin, `*.adventure-works.com`), uygulama proxy uç noktasının dış URL 'sini işaret eder. Joker uygulamalar için CNAME kaydının ilgili dış URL 'Leri göstermesi gerekir:

> `<yourAADTenantId>.tenant.runtime.msappproxy.net`

CNAME 'nizi doğru şekilde yapılandırdığınızdan emin olmak için, hedef uç noktalardan birinde [nslookup](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup) ' ı kullanabilirsiniz (örneğin, `expenses.adventure-works.com`).  Yanıtınız zaten belirtilen diğer adı (`<yourAADTenantId>.tenant.runtime.msappproxy.net`) içermelidir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Joker uygulamalar için dikkate almanız gereken bazı noktalar aşağıda verilmiştir.

### <a name="accepted-formats"></a>Kabul edilen biçimler

Joker uygulamalar için **Iç URL** 'nin `http(s)://*.<domain>`olarak biçimlendirilmesi gerekir.

![İç URL için http (s)://* biçimini kullanın.\<etki alanı >](./media/application-proxy-wildcard/22.png)

Bir **dış URL**yapılandırdığınızda, aşağıdaki biçimi kullanmanız gerekir: `https://*.<custom domain>`

![Dış URL için https://* biçimini kullanın. Özel etki alanı\<>](./media/application-proxy-wildcard/21.png)

Joker karakter, birden fazla joker karakterler veya diğer normal ifade dizeleri diğer konumlarını desteklenmez ve hatalara neden oluyor.

### <a name="excluding-applications-from-the-wildcard"></a>Joker karakter ' uygulamaları hariç

Bir uygulama tarafından joker uygulamadan hariç tutabilirsiniz

- Normal bir uygulama olarak özel durum uygulama yayımlama
- Joker karakter yalnızca DNS ayarlarınızı aracılığıyla belirli uygulamalar için etkinleştirme

Normal bir uygulama olarak bir uygulama yayımlama, joker karakter bir uygulama hariç tutmak için tercih edilen yöntemdir. Joker karakter içeren uygulamalar, özel durumlar en baştan uygulanmasını sağlamak için önce hariç tutulan uygulamalar yayımlamanız gerekir. En belirli uygulama her zaman öncelikli olur. `budgets.finance.adventure-works.com` yayımlanan bir uygulama, uygulama `*.finance.adventure-works.com`üzerinde önceliğe sahiptir ve bu da uygulama `*.adventure-works.com`göre önceliklidir.

Ayrıca, DNS Yönetimi aracılığıyla belirli uygulamalar için yalnızca iş için joker karakter sınırlayabilirsiniz. En iyi uygulama, bir joker karakter içeren ve biçimi, yapılandırdığınız dış URL ile eşleşen bir CNAME girişi oluşturmanız gerekir. Ancak, joker karakterler için bunun yerine belirli bir uygulama URL'lerini işaret edebilir. Örneğin, `*.adventure-works.com`yerine `hr.adventure-works.com`, `expenses.adventure-works.com` ve `travel.adventure-works.com individually` `000aa000-11b1-2ccc-d333-4444eee4444e.tenant.runtime.msappproxy.net`.

Bu seçeneği kullanırsanız, `AppId.domain`değer için başka bir CNAME girişine de ihtiyacınız vardır, örneğin `00000000-1a11-22b2-c333-444d4d4dd444.adventure-works.com`, ayrıca aynı konuma işaret eder. Uygulama **kimliği** , joker karakter uygulamasının uygulama özellikleri sayfasında bulunabilir:

![Uygulamanın özellik sayfasında uygulama KIMLIĞINI bulun](./media/application-proxy-wildcard/01.png)

### <a name="setting-the-homepage-url-for-the-myapps-panel"></a>Giriş sayfası URL'si MyApps paneli için ayarlama

Joker uygulama, [Uygps panelinde](https://myapps.microsoft.com)yalnızca bir kutucuk ile temsil edilir. Bu kutucuk, varsayılan olarak gizlidir. Kutucuğu Göster ve belirli bir sayfada kullanıcı land olması için:

1. [Giriş sayfası URL 'sini ayarlamaya](application-proxy-configure-custom-home-page.md)yönelik yönergeleri izleyin.
1. Uygulama özellikleri sayfasında uygulamayı **true** olarak **göster** ' i ayarlayın.

### <a name="kerberos-constrained-delegation"></a>Kısıtlı Kerberos temsilcisi seçme

[SSO yöntemi olarak Kerberos kısıtlanmış temsili (KCD)](application-proxy-configure-single-sign-on-with-kcd.md)kullanan uygulamalar IÇIN, SSO yöntemi IÇIN listelenen SPN 'nin de bir joker karakter olması gerekebilir. Örneğin, SPN: `HTTP/*.adventure-works.com`olabilir. Yine de arka uç sunucularınızda (örneğin, `http://expenses.adventure-works.com and HTTP/travel.adventure-works.com`) tek tek SPN 'Leri yapılandırmış olmanız gerekir.

## <a name="scenario-1-general-wildcard-application"></a>Senaryo 1: Genel joker uygulama

Bu senaryoda, yayımlamak istediğiniz üç farklı uygulamaları vardır:

- `expenses.adventure-works.com`
- `hr.adventure-works.com`
- `travel.adventure-works.com`

Üç uygulama:

- Tüm kullanıcılar tarafından kullanılır
- *Tümleşik Windows kimlik doğrulaması* kullan
- Aynı özelliklere sahiptir

Joker uygulamayı [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](application-proxy-add-on-premises-application.md)bölümünde özetlenen adımları kullanarak yayımlayabilirsiniz. Bu senaryo varsayar:

- Şu KIMLIĞE sahip bir kiracı: `000aa000-11b1-2ccc-d333-4444eee4444e`
- `adventure-works.com` adlı doğrulanmış bir etki alanı yapılandırıldı.
- `000aa000-11b1-2ccc-d333-4444eee4444e.tenant.runtime.msappproxy.net` `*.adventure-works.com` işaret eden bir **CNAME** girişi oluşturuldu.

[Belgelenen adımları](application-proxy-add-on-premises-application.md)izleyerek kiracınızda yeni bir uygulama proxy 'si uygulaması oluşturursunuz. Bu örnekte, aşağıdaki alanlarda joker karakterdir:

- İç URL:

    ![Örnek: İç URL 'de joker karakter](./media/application-proxy-wildcard/42.png)

- Dış URL:

    ![Örnek: dış URL 'de joker karakter](./media/application-proxy-wildcard/43.png)

- İç uygulama SPN'si:

    ![Örnek: SPN yapılandırmasında joker karakter](./media/application-proxy-wildcard/44.png)

Joker uygulamayı yayımlayarak, artık kullandığınız URL 'lere giderek üç uygulamanıza erişebilirsiniz (örneğin, `travel.adventure-works.com`).

Aşağıdaki yapı yapılandırmasını uygular:

![Örnek yapılandırma tarafından uygulanan yapıyı gösterir](./media/application-proxy-wildcard/05.png)

| Renk | Açıklama |
| ---   | ---         |
| Mavi  | Azure portal açıkça yayımlanmış ve görünür uygulamalar. |
| Gri  | Uygulamaları üst uygulama üzerinden erişilebilir olabilir. |

## <a name="scenario-2-general-wildcard-application-with-exception"></a>Senaryo 2: Genel joker uygulama özel durum ile

Bu senaryoda, yalnızca finans bölümünün erişimine açık olması gereken `finance.adventure-works.com`başka bir uygulama olan üç genel uygulamaya ek olarak sahip olursunuz. Geçerli uygulama yapısıyla Finans uygulamanız aracılığıyla joker uygulama ve tüm çalışanlar tarafından erişilebilir olacaktır. Bunu değiştirmek için uygulamanızın, joker karakter daha kısıtlayıcı izinlerle ayrı bir uygulama olarak, Finans yapılandırarak hariç tutun.

Uygulamanın uygulama proxy sayfasında belirtilen uygulamaya özgü uç noktaya `finance.adventure-works.com` işaret eden bir CNAME kayıtları bulunduğundan emin olmanız gerekir. Bu senaryo için `https://finance-awcycles.msappproxy.net/`işaret `finance.adventure-works.com`.

[Belgelenen adımları](application-proxy-add-on-premises-application.md)izleyerek, bu senaryo aşağıdaki ayarları gerektirir:

- **Iç URL**'de, bir joker karakter yerine **finans** ayarlarsınız.

    ![Örnek: İç URL 'de joker karakter yerine finans ayarla](./media/application-proxy-wildcard/52.png)

- **Dış URL**'de, bir joker karakter yerine **finans** ayarlarsınız.

    ![Örnek: dış URL 'de joker karakter yerine finans ayarla](./media/application-proxy-wildcard/53.png)

- İç uygulama SPN 'si, bir joker karakter yerine **finans** 'yi ayarlar.

    ![Örnek: SPN yapılandırmasında joker karakter yerine finans ayarlama](./media/application-proxy-wildcard/54.png)

Bu yapılandırma aşağıdaki senaryoyu uygular:

![Örnek senaryo tarafından uygulanan yapılandırmayı gösterir](./media/application-proxy-wildcard/09.png)

`finance.adventure-works.com`, `*.adventure-works.com`daha özel bir URL olduğundan öncelik kazanır. `finance.adventure-works.com` gezinmekte olan kullanıcılar, finans kaynakları uygulamasında belirtilen deneyimdir. Bu durumda, yalnızca finans çalışanları `finance.adventure-works.com`erişebiliyor.

Finans için yayımlanmış birden fazla uygulamanız varsa ve doğrulanmış etki alanı olarak `finance.adventure-works.com`, `*.finance.adventure-works.com`başka bir joker uygulama yayımlayabilirsiniz. Bu, genel `*.adventure-works.com`göre daha belirgin olduğundan, bir Kullanıcı finans etki alanındaki bir uygulamaya eriştiğinde öncelik kazanır.

## <a name="next-steps"></a>Sonraki adımlar

- **Özel etki alanları**hakkında daha fazla bilgi için bkz. [Azure AD uygulama ara sunucusu özel etki alanları ile çalışma](application-proxy-configure-custom-domain.md).
- **Uygulama yayımlama**hakkında daha fazla bilgi edinmek için bkz. [Azure kullanarak uygulama yayımlama ad uygulama ara sunucusu](application-proxy-add-on-premises-application.md)
