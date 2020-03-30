---
title: Azure AD Uygulama Proxy için güvenlik hususları | Microsoft Dokümanlar
description: Azure AD Uygulama Proxy'si kullanmak için güvenlik hususlarını kapsar
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
ms.date: 03/13/2020
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6fd6794bafc3c209032f32626e8c46b51769d05e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481237"
---
# <a name="security-considerations-for-accessing-apps-remotely-with-azure-ad-application-proxy"></a>Azure AD Application Proxy ile uygulamalara uzaktan erişmek için güvenlik hususları

Bu makalede, Azure Etkin Dizin Uygulama Proxy'sini kullandığınızda kullanıcılarınızın ve uygulamalarınızın güvende kalması için çalışan bileşenler açıklanmaktadır.

Aşağıdaki diyagram, Azure AD'nin şirket içi uygulamalarınıza nasıl güvenli uzaktan erişim sağladığını gösterir.

 ![Azure AD Application Proxy ile güvenli uzaktan erişim diyagramı](./media/application-proxy-security/secure-remote-access.png)

## <a name="security-benefits"></a>Güvenlik avantajları

Azure AD Application Proxy aşağıdaki güvenlik avantajlarını sunar:

### <a name="authenticated-access"></a>Kimlik doğrulaması erişim 

Azure Active Directory ön kimlik doğrulamasını kullanmayı seçerseniz, ağınızda yalnızca kimlik doğrulaması olan bağlantılar erişebilir.

Azure AD Application Proxy, tüm kimlik doğrulamaiçin Azure AD güvenlik belirteç hizmetine (STS) güvenir.  Yalnızca kimlik doğrulama, arka uç uygulamasına erişebildiği için, kimlik doğrulama, doğası gereği önemli sayıda anonim saldırıyı engeller.

Ön kimlik doğrulama yönteminiz olarak Passthrough'ı seçerseniz, bu avantajdan yararlanamazsınız. 

### <a name="conditional-access"></a>Koşullu Erişim

Ağınıza bağlantılar kurulmadan önce daha zengin ilke denetimleri uygulayın.

[Koşullu Erişim](../conditional-access/overview.md)ile, arka uç uygulamalarınıza hangi trafiğin erişmesine izin verildiğine ilişkin kısıtlamalar tanımlayabilirsiniz. Konuma, kimlik doğrulama nın gücüne ve kullanıcı risk profiline göre oturum açmaları kısıtlayan ilkeler oluşturabilirsiniz.

Çok Faktörlü Kimlik Doğrulama ilkelerini yapılandırmak ve kullanıcı kimlik doğrulamalarınıza başka bir güvenlik katmanı eklemek için Koşullu Erişim'i de kullanabilirsiniz. Ayrıca, uygulamalarınız, erişim ve [oturum](https://docs.microsoft.com/cloud-app-security/session-policy-aad) ilkeleri aracılığıyla gerçek zamanlı izleme ve denetimsağlamak için Azure AD Koşullu Erişim aracılığıyla Microsoft Bulut Uygulama [Güvenliği'ne](https://docs.microsoft.com/cloud-app-security/access-policy-aad) de yönlendirilebilir

### <a name="traffic-termination"></a>Trafik sonlandırma

Buluttaki tüm trafik sonlandırıldı.

Azure AD Application Proxy bir ters proxy olduğundan, hizmette arka uç uygulamaları için tüm trafik sonlandırılır. Oturum yalnızca arka uç sunucusuyla yeniden oluşturulabilir, bu da arka uç sunucularınızın doğrudan HTTP trafiğine maruz kalınmadığı anlamına gelir. Bu yapılandırma, hedeflenen saldırılara karşı daha iyi korunduğunuz anlamına gelir.

### <a name="all-access-is-outbound"></a>Tüm erişim giden 

Şirket ağına gelen bağlantıları açmanız gerekmez.

Uygulama Proxy bağlayıcıları yalnızca Azure AD Application Proxy hizmetine giden bağlantıları kullanır, bu da gelen bağlantılar için güvenlik duvarı bağlantı noktalarını açmaya gerek olmadığı anlamına gelir. Geleneksel yakınlıklar bir çevre ağı *(dmz*olarak da bilinir, *arındırılmış bölge*veya *ekranlı alt ağ)* gerekli ve ağ kenarında kimlik doğrulamamış bağlantılara erişime izin verdi. Bu senaryo, trafiği analiz etmek ve çevreyi korumak için web uygulaması güvenlik duvarı ürünlerine yatırım yapılmasını gerektir. Uygulama Proxy ile, tüm bağlantılar giden ve güvenli bir kanal üzerinden gerçekleşecek bir çevre ağı gerekmez.

Bağlayıcılar hakkında daha fazla bilgi için [bkz.](application-proxy-connectors.md)

### <a name="cloud-scale-analytics-and-machine-learning"></a>Bulut ölçekli analitik ve makine öğrenimi 

En son güvenlik koruması alın.

Azure Etkin Dizin'in bir parçası olduğundan, Uygulama Proxy'si Microsoft Güvenlik Yanıt Merkezi ve Dijital Suçlar Birimi'nden alınan verilerle [Azure AD Kimlik Koruması'ndan](../active-directory-identityprotection.md)yararlanabilir. Birlikte, gizliliği ihlal edilen hesapları proaktif olarak tanımlar ve yüksek riskli oturum açmalara karşı koruma sağlarız. Hangi oturum açma girişimlerinin yüksek riskli olduğunu belirlemek için birçok faktörü dikkate alıyoruz. Bu etkenler arasında virüslü aygıtları işaretleme, anonimleştirme ağları ve atipik veya olası konumlar yer almaktadır.

Bu raporların ve olayların çoğu, güvenlik bilgileriniz ve olay yönetimi (SIEM) sistemlerinizle tümleştirme için bir API aracılığıyla zaten kullanılabilir.

### <a name="remote-access-as-a-service"></a>Hizmet olarak uzaktan erişim

Şirket içi sunucuları koruma ve düzeltme konusunda endişelenmenize gerek yoktur.

Yamasız yazılım hala çok sayıda saldırıyı hesaba katsın. Azure AD Application Proxy, Microsoft'un sahip olduğu Internet ölçeğinde bir hizmettir, bu nedenle her zaman en son güvenlik düzeltme emültlerini ve yükseltmelerini alırsınız.

Azure AD Application Proxy tarafından yayınlanan uygulamaların güvenliğini artırmak için, web tarayıcısı robotlarının uygulamalarınızı dizine ekleyip arşivletmelerini engelliyoruz. Her zaman bir web tarayıcısı robot yayınlanan bir uygulama için robotun ayarlarını almak için çalışır, `User-agent: * Disallow: /`Uygulama Proxy içeren bir robots.txt dosyası ile yanıtlar .

#### <a name="azure-ddos-protection-service"></a>Azure DDoS koruma hizmeti

Application Proxy aracılığıyla yayınlanan uygulamalar, Dağıtılmış Hizmet Reddi (DDoS) saldırılarına karşı korunur. **Azure DDoS koruması,** Azure kaynaklarınızı hizmet reddi saldırılarına karşı korumak için Azure platformuyla sunulan bir hizmettir. **Temel** hizmet katmanı, her zaman çevrimiçi trafik izleme ve ortak ağ düzeyinde saldırıların gerçek zamanlı olarak azaltılması nı sağlayarak otomatik olarak etkinleştirilir. Azure Sanal Ağ kaynaklarına özel olarak ayarlanmış ek azaltma özellikleri sunan **standart** bir katman da mevcuttur. Ayrıntılar için Azure [DDoS Koruma Standardı'na genel bakış](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)alabilirsiniz.

## <a name="under-the-hood"></a>Başlık altında

Azure AD Application Proxy iki bölümden oluşur:

* Bulut tabanlı hizmet: Bu hizmet Azure'da çalışır ve harici istemci/kullanıcı bağlantılarının yapıldığı yerdir.
* [Şirket içi bağlayıcı](application-proxy-connectors.md): Şirket içi bir bileşen olan konektör, Azure AD Application Proxy hizmetinden gelen istekleri dinler ve dahili uygulamalarla bağlantıları işler. 

Aşağıdaki anda bağlayıcı ile Application Proxy hizmeti arasında bir akış belirlenir:

* Konektör ilk olarak ayarlanır.
* Konektör, Uygulama Proxy hizmetinden yapılandırma bilgilerini çeker.
* Kullanıcı yayımlanmış bir uygulamaya erişer.

>[!NOTE]
>Tüm iletişim TLS üzerinden gerçekleşir ve her zaman Uygulama Proxy hizmetinin konektöründen kaynaklanır. Hizmet yalnızca giden.

Bağlayıcı, hemen hemen tüm aramalar için Uygulama Proxy hizmetine kimlik doğrulamak için bir istemci sertifikası kullanır. Bu işlemin tek istisnası, istemci sertifikasının oluşturulduğü ilk kurulum adımıdır.

### <a name="installing-the-connector"></a>Konektörü yükleme

Bağlayıcı ilk ayarlandığında, aşağıdaki akış olayları gerçekleşir:

1. Hizmete konektör kaydı, konektörün yüklenmesinin bir parçası olarak gerçekleşir. Kullanıcıların Azure AD yönetici kimlik bilgilerini girmeleri istenir.Bu kimlik doğrulamasından elde edilen belirteç daha sonra Azure AD Application Proxy hizmetine sunulur.
2. Uygulama Proxy hizmeti belirteç değerlendirir. Kullanıcının kiracıda şirket yöneticisi olup olmadığını denetler.Kullanıcı yönetici değilse, işlem sonlandırılır.
3. Bağlayıcı bir istemci sertifikası isteği oluşturur ve belirteçle birlikte Uygulama Proxy hizmetine iletin. Hizmet sırayla belirteci doğrular ve istemci sertifikası isteğini imzalar.
4. Bağlayıcı, Uygulama Proxy hizmetiyle gelecekteki iletişim için istemci sertifikasını kullanır.
5. Bağlayıcı, istemci sertifikasını kullanarak sistem yapılandırma verilerinin hizmetten ilk çekmesini gerçekleştirir ve artık istek almaya hazırdır.

### <a name="updating-the-configuration-settings"></a>Yapılandırma ayarlarını güncelleştirme

Uygulama Proxy hizmeti yapılandırma ayarlarını güncellediğinde aşağıdaki akış olayları gerçekleşir:

1. Bağlayıcı, istemci sertifikasını kullanarak Uygulama Proxy hizmeti içindeki yapılandırma bitiş noktasına bağlanır.
2. İstemci sertifikası doğrulandıktan sonra, Uygulama Proxy hizmeti yapılandırma verilerini bağlayıcıya döndürür (örneğin, bağlayıcının parçası olması gereken bağlayıcı grubu).
3. Geçerli sertifika 180 günden eskiyse, bağlayıcı her 180 günde bir istemci sertifikasını etkili bir şekilde güncelleyen yeni bir sertifika isteği oluşturur.

### <a name="accessing-published-applications"></a>Yayımlanmış uygulamalara erişim

Kullanıcılar yayımlanmış bir uygulamaya eriştiklerinde, Uygulama Proxy hizmeti ile Application Proxy bağlayıcısı arasında aşağıdaki olaylar gerçekleşir:

1. Hizmet, uygulama için kullanıcının kimliğini doğrular
2. Hizmet, bağlayıcı kuyruğuna bir istek yerleştirir
3. Bir bağlayıcı, sıradaki isteği işler
4. Bağlayıcı yanıt bekler
5. Hizmet verileri kullanıcıya aktarıyor

Bu adımların her birinde neler inanacak hakkında daha fazla bilgi edinmek için okumaya devam edin.


#### <a name="1-the-service-authenticates-the-user-for-the-app"></a>1. Hizmet, uygulama için kullanıcının kimliğini doğrular

Uygulamayı Passthrough'ı ön kimlik doğrulama yöntemi olarak kullanacak şekilde yapılandırırsanız, bu bölümdeki adımlar atlanır.

Uygulamayı Azure AD ile ön kimlik doğrulaması yapacak şekilde yapılandırırsanız, kullanıcılar kimlik doğrulaması için Azure AD STS'ye yönlendirilir ve aşağıdaki adımlar gerçekleşir:

1. Uygulama Proxy belirli bir uygulama için herhangi bir Koşullu Erişim ilkesi gereksinimleri için denetler. Bu adım, kullanıcının uygulamaya atanmasını sağlar. İki aşamalı doğrulama gerekiyorsa, kimlik doğrulama sırası kullanıcıdan ikinci bir kimlik doğrulama yöntemi ister.

2. Tüm denetimler geçtikten sonra, Azure AD STS uygulama için imzalı bir belirteç verir ve kullanıcıyı Uygulama Proxy hizmetine geri yönlendirir.

3. Application Proxy belirteci doğru uygulama için verilmiş olduğunu doğrular. BelirteçAzure AD tarafından imzalanmış olduğundan ve hala geçerli pencereiçinde olduğundan emin olmak gibi diğer denetimleri de gerçekleştirir.

4. Uygulama Proxy, uygulamanın kimlik doğrulamasının gerçekleştiğini belirtmek için şifrelenmiş bir kimlik doğrulama tanımlama bilgisi ayarlar. Çerez, Azure AD'den gelen belirteci ve kimlik doğrulamasının dayandığı kullanıcı adı gibi diğer verileri temel alan bir son kullanma zaman damgası içerir. Çerez, yalnızca Uygulama Proxy hizmeti tarafından bilinen özel bir anahtarla şifrelenir.

5. Application Proxy, kullanıcıyı ilk istenen URL'ye yönlendirir.

Ön kimlik doğrulama adımlarının herhangi bir bölümü başarısız olursa, kullanıcının isteği reddedilir ve kullanıcıya sorunun kaynağını gösteren bir ileti gösterilir.


#### <a name="2-the-service-places-a-request-in-the-connector-queue"></a>2. Servis, bağlayıcı kuyruğuna bir istek yerleştirir

Bağlayıcılar, giden bağlantıyı Uygulama Proxy hizmetine açık tutar. Bir istek geldiğinde, servis bağlayıcının alması için açık bağlantılardan birinde isteği sıraya alır.

İstek, istek üstbilgileri, şifrelenmiş çerezden alınan veriler, isteği yapan kullanıcı ve istek kimliği gibi uygulamadan öğeler içerir. Şifreli çerezden gelen veriler istekle birlikte gönderilmiş olsa da, kimlik doğrulama çerezinin kendisi gönderilmez.

#### <a name="3-the-connector-processes-the-request-from-the-queue"></a>3. Konektör, sıradaki isteği işler. 

İsteğe bağlı olarak, Uygulama Proxy aşağıdaki eylemlerden birini gerçekleştirir:

* İstek basit bir işlemse (örneğin, restful *GET* isteğinde olduğu gibi gövde içinde veri yoksa), bağlayıcı hedef iç kaynağa bağlantı yapar ve ardından yanıt bekler.

* İsteğin gövdesinde onunla ilişkili veriler varsa (örneğin, bir RESTful *POST* işlemi), bağlayıcı istemci sertifikasını Uygulama Proxy örneğine kullanarak giden bir bağlantı yapar. Verileri istemek ve iç kaynağa bağlantı açmak için bu bağlantıyı kurar. İstek bağlayıcıdan geldikten sonra, Uygulama Proxy hizmeti kullanıcıdan içerik kabul etmeye başlar ve verileri bağlayıcıya iletir. Konektör, verileri iç kaynağa ileter.

#### <a name="4-the-connector-waits-for-a-response"></a>4. Bağlayıcı yanıt bekler.

Tüm içeriğin istek ve iletimi arka uca tamamlandıktan sonra bağlayıcı yanıt bekler.

Yanıt aldıktan sonra, bağlayıcı üstbilgi ayrıntılarını döndürmek ve iade verilerini akışa başlamak için Uygulama Proxy hizmetine giden bir bağlantı yapar.

#### <a name="5-the-service-streams-data-to-the-user"></a>5. Hizmet verileri kullanıcıya aktarıyor. 

Uygulamanın bazı işleme burada oluşabilir. Uygulama Proxy'sini, uygulamanızdaki üstbilgi veya URL'leri çevirecek şekilde yapılandırırsanız, bu işlem bu adımda gerektiği gibi gerçekleşir.


## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Uygulama Proxy'si kullanırken ağ topolojisi hususları](application-proxy-network-topology.md)

[Azure AD Uygulama Proxy bağlayıcılarını anlama](application-proxy-connectors.md)
