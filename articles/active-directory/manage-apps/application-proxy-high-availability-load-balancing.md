---
title: Yüksek kullanılabilirlik ve yük dengeleme - Azure AD Application Proxy
description: Uygulama Proxy dağıtımınızla trafik dağıtımı nasıl çalışır? Konektör performansını en iyi duruma getirme ve arka uç sunucuları için yük dengelemeyi kullanma ipuçları içerir.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/08/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 992075378737552e890bd2d6fed3c519e6c62aa7
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81312944"
---
# <a name="high-availability-and-load-balancing-of-your-application-proxy-connectors-and-applications"></a>Uygulama Proxy bağlayıcılarınızın ve uygulamalarınızın yüksek kullanılabilirliği ve yük dengelemesi

Bu makalede, Uygulama Proxy dağıtımınızla trafik dağıtımının nasıl çalıştığı açıklanmaktadır. Tartışacağız:

- Trafiğin kullanıcılar ve bağlayıcılar arasında nasıl dağıtıldığı ve konektör performansını optimize etme ipuçları

- Birden çok arka uç sunucusu arasında yük dengeleme önerileri ile bağlayıcılar ve arka uç uygulama sunucuları arasında trafik nasıl akar?

## <a name="traffic-distribution-across-connectors"></a>Konektörler arasında trafik dağılımı

Bağlayıcılar, bağlantılarını yüksek kullanılabilirlik ilkelerine göre kurarlar. Trafiğin her zaman bağlayıcılar arasında eşit olarak dağıtılacağının ve oturum yakınlığı olmadığının garantisi yoktur. Ancak, kullanım değişir ve istekler rasgele Uygulama Proxy hizmet örneklerine gönderilir. Sonuç olarak, trafik genellikle bağlayıcılar arasında neredeyse eşit olarak dağıtılır. Aşağıdaki diyagram ve adımlar, kullanıcılar ve bağlayıcılar arasında bağlantıların nasıl kurulduğunu göstermektedir.

![Kullanıcılar ve bağlayıcılar arasındaki bağlantıları gösteren diyagram](media/application-proxy-high-availability-load-balancing/application-proxy-connections.png)

1. İstemci aygıtındaki bir kullanıcı, Application Proxy aracılığıyla yayınlanan şirket içi bir uygulamaya erişmeye çalışır.
2. İstek, hangi Uygulama Proxy hizmeti örneğinin isteği alması gerektiğini belirlemek için bir Azure Yük Dengeleyicisi'nden geçer. Bölge başına, isteği kabul etmek için kullanılabilir onlarca örnek vardır. Bu yöntem, hizmet örnekleri arasında trafiği eşit dağıtmaya yardımcı olur.
3. İstek [Servis Otobüsü'ne](https://docs.microsoft.com/azure/service-bus-messaging/)gönderilir.
4. Servis Veri Servisi sinyalleri kullanılabilir bir konektöre bağlanır. Konektör daha sonra Servis Veri Servisi'nden isteği alır.
   - Adım 2'de, istekler farklı Uygulama Proxy hizmet örneklerine gider, bu nedenle bağlantıların farklı bağlayıcılarla yapılma olasılığı daha yüksektir. Sonuç olarak, konektörler grup içinde neredeyse eşit olarak kullanılır.
5. Bağlayıcı isteği uygulamanın arka uç sunucusuna geçirir. Daha sonra uygulama yanıtı bağlayıcıya geri gönderir.
6. Bağlayıcı, isteğin geldiği hizmet örneğine giden bir bağlantı açarak yanıtı tamamlar. Sonra bu bağlantı hemen kapatılır. Varsayılan olarak, her bağlayıcı 200 eşzamanlı giden bağlantılarla sınırlıdır.
7. Yanıt daha sonra hizmet örneğinden istemciye geri aktarılır.
8. Aynı bağlantıdan sonraki istekler yukarıdaki adımları yineler.

Bir uygulama genellikle birçok kaynağa sahiptir ve yüklendiğinde birden çok bağlantı açar. Her bağlantı, bir hizmet örneğine tahsis olmak için yukarıdaki adımlardan geçer, bağlantı daha önce bir bağlayıcıyla eşleşmiyorsa yeni bir kullanılabilir bağlayıcı seçin.


## <a name="best-practices-for-high-availability-of-connectors"></a>Konektörlerin yüksek kullanılabilirliği için en iyi uygulamalar

- Trafiğin yüksek kullanılabilirlik için bağlayıcılar arasında dağıtılme biçimi nedeniyle, bir bağlayıcı grubunda her zaman en az iki bağlayıcının olması önemlidir. Bağlayıcılar arasında ek arabellek sağlamak için üç konektör tercih edilir. İhtiyacınız olan doğru bağlayıcı sayısını belirlemek için kapasite planlama belgelerini izleyin.

- Tek bir hata noktasını önlemek için bağlayıcıları farklı giden bağlantılara yerleştirin. Bağlayıcılar aynı giden bağlantıyı kullanıyorsa, bağlantıyla ilgili bir ağ sorunu onu kullanan tüm bağlayıcıları etkileyebilir.

- Üretim uygulamalarına bağlandığında bağlayıcıları yeniden başlatmaya zorlamaktan kaçının. Bunu yapmak, trafiğin bağlayıcılar arasında dağılımını olumsuz etkileyebilir. Konektörlerin yeniden başlatılması, daha fazla bağlayıcının kullanılamamasına ve kalan kullanılabilir bağlayıcıya bağlanmaya zorlar. Sonuç, başlangıçta bağlayıcıların düzensiz kullanımıdır.

- Bağlayıcılar ve Azure arasındaki giden TLS iletişimlerinde her türlü satır ara denetiminden kaçının. Bu tür satır ortası denetimleri iletişim akışında bozulmaya neden olur.

- Konektörleriniz için otomatik güncelleştirmeleri çalışır durumda tuttuğunızdan emin olun. Uygulama Proxy Bağlayıcı Sıtkı Güncelleme hizmeti çalışıyorsa, bağlayıcılarınız otomatik olarak güncellenir ve en son yükseltilmiş olanları alır. Sunucunuzda Bağlayıcı Güncelleyici hizmetini görmüyorsanız, herhangi bir güncelleştirme almak için bağlayıcınızı yeniden yüklemeniz gerekir.

## <a name="traffic-flow-between-connectors-and-back-end-application-servers"></a>Bağlayıcılar ve arka uç uygulama sunucuları arasındaki trafik akışı

Yüksek kullanılabilirlik bir faktör olduğu bir diğer önemli alan bağlayıcılar ve arka uç sunucuları arasındaki bağlantıdır. Bir uygulama Azure AD Application Proxy aracılığıyla yayımlandığında, kullanıcılardan uygulamalara giden trafik üç atlamadan akar:

1. Kullanıcı, Azure'daki Azure AD Application Proxy hizmetinin genel bitiş noktasına bağlanır. Bağlantı, istemcinin kaynak olan istemci IP adresi (ortak) ile Uygulama Proxy bitiş noktasının IP adresi arasında kurulur.
2. Application Proxy bağlayıcısı, istemcinin HTTP isteğini Uygulama Proxy Hizmeti'nden çeker.
3. Uygulama Proxy bağlayıcısı hedef uygulamaya bağlanır. Bağlayıcı, bağlantıyı kurmak için kendi IP adresini kullanır.

![Uygulama Proxy ile bir uygulamaya bağlanan kullanıcı diyagramı](media/application-proxy-high-availability-load-balancing/application-proxy-three-hops.png)

### <a name="x-forwarded-for-header-field-considerations"></a>X-Forwarded-Üstbilgi alanı hususları için
Bazı durumlarda (denetim, yük dengeleme vb.) dış istemcinin kaynaklanan IP adresini şirket içi ortamla paylaşmak bir gerekliliktir. Gereksinimi gidermek için Azure AD Application Proxy bağlayıcısı, http isteğine kaynak istemci IP adresine (ortak) sahip X-Forwarded-For üstbilgi alanını ekler. Uygun ağ aygıtı (yük dengeleyici, güvenlik duvarı) veya web sunucusu veya arka uç uygulaması daha sonra bilgileri okuyabilir ve kullanabilir.

## <a name="best-practices-for-load-balancing-among-multiple-app-servers"></a>Birden çok uygulama sunucusu arasında yük dengeleme için en iyi uygulamalar
Uygulama Proxy uygulamasına atanan bağlayıcı grubunun iki veya daha fazla bağlayıcısı varsa ve arka uç web uygulamasını birden çok sunucuda (sunucu çiftliğinde) çalıştırıyorsanız, iyi bir yük dengeleme stratejisi gereklidir. İyi bir strateji, sunucuların istemci isteklerini eşit olarak almasını sağlar ve sunucu çiftliğindeki sunucuların aşırı veya az kullanılmasını engeller.
### <a name="scenario-1-back-end-application-does-not-require-session-persistence"></a>Senaryo 1: Arka uç uygulaması oturum kalıcılığı gerektirmez
En basit senaryo, arka uç web uygulamasının oturum yapışkanlığı (oturum kalıcılığı) gerektirmediği yerdir. Kullanıcıdan gelen herhangi bir istek, sunucu çiftliğindeki herhangi bir arka uç uygulama örneği tarafından işlenebilir. Bir katman 4 yük dengeleyicik kullanabilirsiniz ve hiçbir yakınlık ile yapılandırAbilirsiniz. Bazı seçenekler arasında Microsoft Ağ Yük Dengeleme ve Azure Yük Dengeleyicisi veya başka bir satıcıdan gelen yük dengeleyicisi yer almaktadır. Alternatif olarak, round-robin DNS yapılandırılabilir.
### <a name="scenario-2-back-end-application-requires-session-persistence"></a>Senaryo 2: Arka uç uygulaması oturum kalıcılığı gerektirir
Bu senaryoda, arka uç web uygulaması, kimlik doğrulaması yapılan oturum sırasında oturum yapışkanlığı (oturum kalıcılığı) gerektirir. Kullanıcıdan gelen tüm istekler, sunucu çiftliğinde aynı sunucuda çalışan arka uç uygulama örneği tarafından işlenmelidir.
İstemci genellikle Uygulama Proxy hizmetine birden çok bağlantı oluşturduğundan, bu senaryo daha karmaşık olabilir. Farklı bağlantılar üzerindeki istekler çiftlikteki farklı bağlayıcılara ve sunuculara gelebilir. Her bağlayıcı bu iletişim için kendi IP adresini kullandığından, yük dengeleyici bağlayıcıların IP adresine bağlı olarak oturum yapışkanlığı sağlayamaz. Kaynak IP Affinity de kullanılamaz.
Senaryo 2 için bazı seçenekler şunlardır:

- Seçenek 1: Oturum kalıcılığını yük dengeleyicisi tarafından ayarlanan bir oturum çerezine dayandırın. Bu seçenek, yükün arka uç sunucular arasında daha eşit olarak yayılmasını sağladığı için önerilir. Bu yeteneğe sahip bir katman 7 yük dengeleyici gerektirir ve BU http trafik işleyebilir ve TLS bağlantısını sona erdirebilir. Azure Uygulama Ağ Geçidi 'ni (Oturum Affinity) veya başka bir satıcıdan yük dengeleyicisi kullanabilirsiniz.

- Seçenek 2: Oturum kalıcılığını X-Forwarded-For üstbilgi alanına dayandırın. Bu seçenek, bu yeteneğe sahip bir katman 7 yük dengeleyicisi gerektirir ve BU özellik HTTP trafiğini işleyebilir ve TLS bağlantısını sonlandırabilir.  

- Seçenek 3: Arka uç uygulamasını oturum kalıcılığı gerektirmeyecek şekilde yapılandırın.

Arka uç uygulamasının yük dengeleme gereksinimlerini anlamak için yazılım satıcınızın belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama Ara Sunucusunu etkinleştirme](application-proxy-add-on-premises-application.md)
- [Çoklu oturum açmayı etkinleştirme](application-proxy-configure-single-sign-on-with-kcd.md)
- [Koşullu Erişimi Etkinleştir](application-proxy-integrate-with-sharepoint-server.md)
- [Uygulama Ara Sunucusu ile ilgili sorunları giderme](application-proxy-troubleshoot.md)
- [Azure AD mimarisinin yüksek kullanılabilirliği nasıl desteklediğini öğrenin](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-architecture)
