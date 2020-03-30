---
title: Azure AD Uygulama Proxy'sinde Joker Karakter uygulamaları
description: Azure Active Directory uygulama proxy'sinde Joker uygulamaları nasıl kullanacağınızı öğrenin.
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
ms.openlocfilehash: 3ad2032497b4b01476389428f5a2ef4a3961a1c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481254"
---
# <a name="wildcard-applications-in-the-azure-active-directory-application-proxy"></a>Azure Active Directory uygulama proxy'sinde Joker karakter uygulamaları

Azure Etkin Dizini'nde (Azure AD), çok sayıda şirket içi uygulamayı yapılandırmak hızla yönetilemez hale gelebilir ve çoğu aynı ayarları gerektiriyorsa yapılandırma hataları için gereksiz riskler doğurabilir. [Azure AD Application Proxy](application-proxy.md)ile, birçok uygulamayı aynı anda yayımlamak ve yönetmek için joker karakter uygulaması yayımlamasını kullanarak bu sorunu giderebilirsiniz. Bu, şunları yapmanızı sağlayan bir çözümdür:

- İdari yükünüze basitleştirin
- Olası yapılandırma hatalarının sayısını azaltma
- Kullanıcılarınızın daha fazla kaynağa güvenli bir şekilde erişmesini sağlama

Bu makalede, çevrenizde joker karakter uygulaması yayımlama yapılandırmak için gereken bilgileri sağlar.

## <a name="create-a-wildcard-application"></a>Joker karakter uygulaması oluşturma

Aynı yapılandırmaya sahip bir uygulama grubunuz varsa joker karakter (*) uygulaması oluşturabilirsiniz. Joker karakter uygulaması için potansiyel adaylar aşağıdaki ayarları paylaşan uygulamalardır:

- Bunlara erişimi olan kullanıcı grubu
- SSO yöntemi
- Erişim protokolü (http, https)

Her ikisi de dahili ve harici URL'ler aşağıdaki biçimdeyse joker karakterlerle uygulamaları yayımlayabilirsiniz:

> http(s)http://*. \<etki alanı\>

Örneğin: `http(s)://*.adventure-works.com`.

İç ve dış URL'ler en iyi yöntem olarak farklı etki alanlarını kullanabilse de, aynı olmalıdır. Uygulamayı yayımlarken, URL'lerden birinin joker kartı yoksa bir hata görürsünüz.

Farklı yapılandırma ayarlarına sahip ek uygulamalarınız varsa, joker karakter için ayarlanan varsayılanların üzerine yazmak için bu özel durumları ayrı uygulamalar olarak yayımlamanız gerekir. Joker karaktersiz uygulamalar her zaman joker karakter uygulamalarından önceliklidir. Yapılandırma açısından bakıldığında, bunlar "adil" düzenli uygulamalardır.

Joker karakter uygulaması oluşturmak, diğer tüm uygulamalar için kullanılabilen aynı [uygulama yayımlama akışını](application-proxy-add-on-premises-application.md) temel adatır. Tek fark, URL'lere ve potansiyel olarak SSO yapılandırmasında bir joker karakter eklemenizdir.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için bu gereksinimleri karşıladığınızdan emin olun.

### <a name="custom-domains"></a>Özel etki alanları

[Özel etki alanları](application-proxy-configure-custom-domain.md) diğer tüm uygulamalar için isteğe bağlı olsa da, joker karakter uygulamaları için bir ön koşuldur. Özel etki alanları oluşturmanız şunları gerektirir:

1. Azure içinde doğrulanmış bir etki alanı oluşturun.
1. PFX formatında bir TLS/SSL sertifikasını uygulama proxy'nize yükleyin.

Oluşturmayı planladığınız uygulamayla eşleşecek bir joker karakter sertifikası kullanmayı düşünmelisiniz. Alternatif olarak, yalnızca belirli uygulamaları listeleyen bir sertifika da kullanabilirsiniz. Bu durumda, bu joker karakter uygulaması aracılığıyla yalnızca sertifikada listelenen uygulamalara erişilebilir.

Güvenlik nedeniyle, bu zor bir gereksinimdir ve harici URL için özel bir etki alanı kullanamayan uygulamalar için joker karakterleri desteklemeyiz.

### <a name="dns-updates"></a>DNS güncelleştirmeleri

Özel etki alanlarını kullanırken, uygulama proxy bitiş noktasının dış URL'sini gösteren harici `*.adventure-works.com`URL (örneğin,) için CNAME kaydı içeren bir DNS girişi oluşturmanız gerekir. Joker karakter uygulamaları için CNAME kaydının ilgili harici URL'leri işaret etmesi gerekir:

> `<yourAADTenantId>.tenant.runtime.msappproxy.net`

CNAME'nizi doğru yapılandırdığınızı doğrulamak için, örneğin hedef uç noktalardan birinde [nslookup'u](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup) `expenses.adventure-works.com`kullanabilirsiniz.  Yanıtınız daha önce bahsedilen diğer`<yourAADTenantId>.tenant.runtime.msappproxy.net`adı içermelidir ( ).

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Joker karakter uygulamaları için dikkate almanız gereken bazı hususlar şunlardır.

### <a name="accepted-formats"></a>Kabul edilen biçimler

Joker karakter uygulamaları için **Dahili** URL'nin `http(s)://*.<domain>`' olarak biçimlendirilmesi gerekir.

![Dahili URL için http(ler)://*biçimini kullanın. \<etki alanı>](./media/application-proxy-wildcard/22.png)

**Harici URL**yapılandırDığınızda aşağıdaki biçimi kullanmanız gerekir:`https://*.<custom domain>`

![Harici URL için https://* biçimini kullanın. \<özel etki alanı>](./media/application-proxy-wildcard/21.png)

Joker kartın, birden çok joker kartın veya diğer regex dizelerin diğer konumları desteklenmez ve hatalara neden olur.

### <a name="excluding-applications-from-the-wildcard"></a>Uygulamaları joker karakterden hariç

Bir uygulamayı joker karakter uygulamasından

- İstisna uygulamasını düzenli uygulama olarak yayımlama
- DNS ayarlarınız aracılığıyla joker kartı yalnızca belirli uygulamalar için etkinleştirme

Bir uygulamayı normal bir uygulama olarak yayımlamak, bir uygulamayı joker karakterden dışlamak için tercih edilen yöntemdir. Özel durumlarınızın en başından itibaren uygulandığından emin olmak için, dışlanan uygulamaları joker karakter uygulamalarından önce yayımlamalısınız. En özel uygulama her zaman öncelikli olacaktır `budgets.finance.adventure-works.com` – olarak yayınlanan `*.finance.adventure-works.com`bir uygulama uygulamadan önce gelir `*.adventure-works.com`, bu uygulamadan önce gelir.

Joker kartı yalnızca DNS yönetiminiz aracılığıyla belirli uygulamalar için çalışacak şekilde sınırlandırabilirsiniz. En iyi uygulama olarak, joker karakter içeren ve yapılandırdığınız dış URL biçimiyle eşleşen bir CNAME girişi oluşturmanız gerekir. Ancak, bunun yerine joker karakterlere belirli uygulama URL'leri işaret edebilirsiniz. Örneğin, `*.adventure-works.com`, noktası `hr.adventure-works.com` `expenses.adventure-works.com` ve `travel.adventure-works.com individually` . `000aa000-11b1-2ccc-d333-4444eee4444e.tenant.runtime.msappproxy.net`

Bu seçeneği kullanırsanız, aynı konuma işaret eden `AppId.domain`değer için `00000000-1a11-22b2-c333-444d4d4dd444.adventure-works.com`başka bir CNAME girişine de ihtiyacınız olur. Joker karakter uygulamasının uygulama özellikleri sayfasında **AppId'yi** bulabilirsiniz:

![Uygulama kimliğini uygulamanın özellik sayfasında bulma](./media/application-proxy-wildcard/01.png)

### <a name="setting-the-homepage-url-for-the-myapps-panel"></a>MyApps paneli için ana sayfa URL'sini ayarlama

Joker karakter uygulaması [MyApps panelinde](https://myapps.microsoft.com)sadece bir döşeme ile temsil edilir. Varsayılan olarak bu döşeme gizlidir. Döşemeyi göstermek ve kullanıcıların belirli bir sayfaya inmelerini sağlamak için:

1. Ana sayfa [URL'sini ayarlamak](application-proxy-configure-custom-home-page.md)için yönergeleri izleyin.
1. **Uygulamayı Göster'i** uygulama özellikleri sayfasında **doğru** olarak ayarlayın.

### <a name="kerberos-constrained-delegation"></a>Kerberos kısıtlı delegasyon

[SSO yöntemi olarak kerberos kısıtlı delegasyon (KCD)](application-proxy-configure-single-sign-on-with-kcd.md)kullanan uygulamalar için, SSO yöntemi için listelenen SPN de bir joker gerekebilir. Örneğin, SPN olabilir: `HTTP/*.adventure-works.com`. Yine de tek tek SPN'lerin arka uç sunucularınızda `http://expenses.adventure-works.com and HTTP/travel.adventure-works.com`yapılandırılması gerekir (örneğin, ).

## <a name="scenario-1-general-wildcard-application"></a>Senaryo 1: Genel joker karakter uygulaması

Bu senaryoda, yayımlamak istediğiniz üç farklı uygulama vardır:

- `expenses.adventure-works.com`
- `hr.adventure-works.com`
- `travel.adventure-works.com`

Her üç uygulama:

- Tüm kullanıcılarınız tarafından kullanılır
- *Tümleşik Windows Kimlik Doğrulama'yı* kullanma
- Aynı özelliklere sahip

Azure AD Application Proxy'yi kullanarak [Uygulama Yayımla'da](application-proxy-add-on-premises-application.md)özetlenen adımları kullanarak joker karakter uygulamasını yayımlayabilirsiniz. Bu senaryo varsayar:

- Aşağıdaki kimliği olan bir kiracı:`000aa000-11b1-2ccc-d333-4444eee4444e`
- Adı verilen `adventure-works.com` doğrulanmış bir etki alanı yapılandırıldı.
- Noktayı işaret `*.adventure-works.com` `000aa000-11b1-2ccc-d333-4444eee4444e.tenant.runtime.msappproxy.net` eden bir **CNAME** girişi oluşturuldu.

[Belgelenen adımları](application-proxy-add-on-premises-application.md)izleyerek, kiracınızda yeni bir uygulama proxy uygulaması oluşturursunuz. Bu örnekte, joker karakter aşağıdaki alanlardadır:

- Dahili URL:

    ![Örnek: Dahili URL'deki Joker Karakter](./media/application-proxy-wildcard/42.png)

- Dış URL:

    ![Örnek: Harici URL'deki Joker Karakter](./media/application-proxy-wildcard/43.png)

- Dahili Uygulama SPN:

    ![Örnek: SPN yapılandırmasında Joker Karakter](./media/application-proxy-wildcard/44.png)

Joker karakter uygulamasını yayımlayarak, artık alışık olduğunuz URL'lere (örneğin) `travel.adventure-works.com`gezinerek üç uygulamanıza erişebilirsiniz.

Yapılandırma aşağıdaki yapıyı uygular:

![Örnek yapılandırma tarafından uygulanan yapıyı gösterir](./media/application-proxy-wildcard/05.png)

| Renk | Açıklama |
| ---   | ---         |
| Mavi  | Uygulamalar Açıkça yayımlanır ve Azure portalında görünür. |
| Gri  | Uygulamalara ana uygulama üzerinden erişebilirsiniz. |

## <a name="scenario-2-general-wildcard-application-with-exception"></a>Senaryo 2: Genel joker uygulaması istisna

Bu senaryoda, üç genel uygulamalara ek olarak, `finance.adventure-works.com`yalnızca Finans bölümü tarafından erişilebilir olması gereken başka bir uygulama var. Mevcut uygulama yapısı ile, finans uygulamanıza joker karakter uygulaması ve tüm çalışanlar tarafından erişilebilir olacaktır. Bunu değiştirmek için, Finans'ı daha kısıtlayıcı izinlere sahip ayrı bir uygulama olarak yapılandırarak uygulamanızı joker kartınızdan dışlarsınız.

Uygulama için Uygulama Proxy sayfasında belirtilen uygulamaya `finance.adventure-works.com` özgü bitiş noktasını gösteren bir CNAME kaydının bulunduğundan emin olmanız gerekir. Bu senaryo `finance.adventure-works.com` için, `https://finance-awcycles.msappproxy.net/`işaret .

[Belgelenen adımları](application-proxy-add-on-premises-application.md)takiben, bu senaryo aşağıdaki ayarları gerektirir:

- İç **URL'de**joker karakter yerine **finans** ı ayarlarsınız.

    ![Örnek: Dahili URL'de joker karakter yerine finans ı ayarlama](./media/application-proxy-wildcard/52.png)

- Dış **URL'de**joker karakter yerine **finans** ı ayarlarsınız.

    ![Örnek: Harici URL'de joker karakter yerine finans ı ayarlama](./media/application-proxy-wildcard/53.png)

- Dahili Uygulama SPN bir joker yerine **finans** ayarlayın.

    ![Örnek: SPN yapılandırmasında joker karakter yerine finans ı ayarlama](./media/application-proxy-wildcard/54.png)

Bu yapılandırma aşağıdaki senaryoyu uygular:

![Örnek senaryo tarafından uygulanan yapılandırmayı gösterir](./media/application-proxy-wildcard/09.png)

Çünkü `finance.adventure-works.com` daha özel bir `*.adventure-works.com`URL, öncelik alır. Finans Kaynakları `finance.adventure-works.com` uygulamasında belirtilen deneyime sahip olmak için gezinen kullanıcılar. Bu durumda, yalnızca finans çalışanları `finance.adventure-works.com`erişebilir.

Finans için yayınlanmış birden çok uygulamanız `finance.adventure-works.com` varsa ve doğrulanmış bir etki alanı `*.finance.adventure-works.com`nız varsa, başka bir joker karakter uygulaması yayımlayabilirsiniz. Bu genel `*.adventure-works.com`daha spesifik olduğundan, bir kullanıcı finans etki alanında bir uygulamaya erişir önce gelir.

## <a name="next-steps"></a>Sonraki adımlar

- Özel etki **alanları**hakkında daha fazla bilgi edinmek için Azure [AD Application Proxy'de özel etki alanlarıyla çalışma](application-proxy-configure-custom-domain.md)'ya bakın.
- **Uygulama Yayımlama**hakkında daha fazla bilgi edinmek için Azure [AD Application Proxy kullanarak uygulama yayımla'ya](application-proxy-add-on-premises-application.md) bakın
