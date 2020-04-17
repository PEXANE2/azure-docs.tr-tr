---
title: Güvenlik
description: Uygulama Hizmeti'nin uygulamanızın güvenliğini sağlamaya nasıl yardımcı olduğu ve uygulamanızı tehditlere karşı nasıl daha fazla kilitlenebildiğiniz hakkında bilgi edinin.
keywords: azure app hizmeti, web uygulaması, mobil uygulama, api uygulaması, fonksiyon uygulaması, güvenlik, güvenli, güvenli, uyumluluk, uyumlu, sertifika, sertifika, https, ftps, tls, güven, şifreleme, şifreleme, şifreli, ip kısıtlaması, kimlik doğrulama, yetkilendirme, authn, autho, msi, yönetilen hizmet kimliği, yönetilen kimlik, sırlar, gizli, yama, yama, yama, yamalar, sürüm, izolasyon, ağ yalıtımı, ddos, mitm
ms.topic: article
ms.date: 08/24/2018
ms.custom: seodec18
ms.openlocfilehash: 8a098b1924bf7c2866f6afd7452b8dd3b93f3109
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535664"
---
# <a name="security-in-azure-app-service"></a>Azure Uygulama Hizmetinde Güvenlik

Bu makalede, [Azure Uygulama Hizmeti'nin](overview.md) web uygulamanızı, mobil uygulama arka uçunuzu, API uygulamanızı ve [işlev uygulamanızı](/azure/azure-functions/)nasıl güvence altına aldığınıza yardımcı olduğu gösterilmektedir. Ayrıca, yerleşik Uygulama Hizmeti özellikleriyle uygulamanızı nasıl daha da güvenebileceğinizi de gösterir.

Azure VM'leri, depolama, ağ bağlantıları, web çerçeveleri, yönetim ve tümleştirme özellikleri dahil olmak üzere Uygulama Hizmeti'nin platform bileşenleri etkin bir şekilde güvenli ve sertleştirilmiştir. Uygulama Hizmeti, aşağıdakileri sağlamak için sürekli olarak güçlü uyumluluk denetimlerinden geçer:

- Uygulama kaynaklarınız diğer müşterilerin Azure kaynaklarından [güvence altına alınır.](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
- [VM örnekleri ve çalışma zamanı yazılımları,](overview-patch-os-runtime.md) yeni keşfedilen güvenlik açıklarını gidermek için düzenli olarak güncelleştirilir. 
- Uygulamanızla diğer Azure kaynakları [(SQL Veritabanı](https://azure.microsoft.com/services/sql-database/)gibi) arasındaki sırların (bağlantı dizeleri gibi) iletişimi Azure içinde kalır ve ağ sınırlarını aşmaz. Sırlar depolandığında her zaman şifrelenir.
- [Hibrit bağlantı](app-service-hybrid-connections.md)gibi App Service bağlantı özellikleri üzerinden tüm iletişim şifrelenir. 
- Azure PowerShell, Azure CLI, Azure SDK'lar, REST API'leri gibi uzaktan yönetim araçlarına sahip bağlantıların tümü şifrelenir.
- 24 saat tehdit yönetimi, altyapıyı ve platformu kötü amaçlı yazılımlara, dağıtılmış hizmet reddi (DDoS), ortadaki adam (MITM) ve diğer tehditlere karşı korur.

Azure'da altyapı ve platform güvenliği hakkında daha fazla bilgi için [Azure Güven Merkezi'ne](https://azure.microsoft.com/overview/trusted-cloud/)bakın.

Aşağıdaki bölümler, Uygulama Hizmeti uygulamanızı tehditlere karşı nasıl daha fazla koruyacağınızı gösterir.

## <a name="https-and-certificates"></a>HTTPS ve Sertifikalar

Uygulama [Hizmeti,](https://wikipedia.org/wiki/HTTPS)https ile uygulamalarınızı güvenli hale almanızı sağlar. Uygulamanız oluşturulduğunda, varsayılan etki\<alanı adı (app_name>.azurewebsites.net) https kullanılarak zaten erişilebilir. [Uygulamanız için özel bir etki alanı yapılandırırsanız,](app-service-web-tutorial-custom-domain.md)istemci tarayıcıların özel etki alanınıza güvenli HTTPS bağlantıları yapabilmesi için [uygulamayı bir TLS/SSL sertifikasıyla da güvenli hale](configure-ssl-bindings.md) getirmeniz gerekir. App Service tarafından desteklenen çeşitli sertifika türleri vardır:

- Ücretsiz Uygulama Hizmeti Yönetilen Sertifika
- Uygulama Hizmeti sertifikası
- Üçüncü taraf sertifikası
- Azure Anahtar Kasasından alınan sertifika

Daha fazla bilgi için bkz: [Azure Uygulama Hizmeti'nde TLS/SSL sertifikası ekleyin.](configure-ssl-certificate.md)

## <a name="insecure-protocols-http-tls-10-ftp"></a>Güvensiz protokoller (HTTP, TLS 1.0, FTP)

Uygulama Hizmeti, uygulamanızı tüm şifrelenmemiş (HTTP) bağlantılara karşı korumak için HTTPS'yi uygulamak için tek tıklamayla yapılandırma sağlar. Güvenli olmayan istekler, uygulama kodunuza ulaşmadan önce geri çevrilitir. Daha fazla bilgi için [bkz.](configure-ssl-bindings.md#enforce-https)

[TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1.0 artık [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard)gibi endüstri standartlarına göre güvenli olarak kabul edilmiştir. Uygulama [Hizmeti, TLS 1.1/1.2'yi uygulayarak](configure-ssl-bindings.md#enforce-tls-versions)eski geçmiş protokolleri devre dışı etmenizi sağlar.

Uygulama Hizmeti, dosyalarınızı dağıtmak için hem FTP'yi hem de FTPS'yi destekler. Ancak, mümkünse FTP yerine FTPS kullanılmalıdır. Bu protokollerden biri veya her ikisi kullanılmadığında, [bunları devre dışı bırakmalısınız.](deploy-ftp.md#enforce-ftps)

## <a name="static-ip-restrictions"></a>Statik IP kısıtlamaları

Varsayılan olarak, Uygulama Hizmeti uygulamanız internetten gelen tüm IP adreslerinden gelen istekleri kabul eder, ancak bu erişimi küçük bir IP adresi alt kümesiyle sınırlandırabilirsiniz. Windows'daki Uygulama Hizmeti, uygulamanıza erişmesine izin verilen IP adreslerinin bir listesini tanımlamanıza olanak tanır. İzin verilen liste tek tek IP adreslerini veya bir alt ağ maskesi tarafından tanımlanan bir dizi IP adresini içerebilir. Daha fazla bilgi için Azure [Uygulama Hizmeti Statik IP Kısıtlamaları'na](app-service-ip-restrictions.md)bakın.

Windows'daki Uygulama Hizmeti _için, web.config'i_yapılandırarak IP adreslerini dinamik olarak kısıtlayabilirsiniz. Daha fazla bilgi için [Dinamik IP Security \<dynamicIpSecurity>. ](https://docs.microsoft.com/iis/configuration/system.webServer/security/dynamicIpSecurity/)

## <a name="client-authentication-and-authorization"></a>İstemci kimlik doğrulaması ve yetkilendirme

Azure Uygulama Hizmeti, kullanıcıların veya istemci uygulamalarının anahtar teslimi kimlik doğrulamasını ve yetkilendirmesini sağlar. Etkinleştirildiğinde, çok az veya hiç uygulama kodu olmayan kullanıcıları ve istemci uygulamalarını oturum açabilir. Kendi kimlik doğrulama ve yetkilendirme çözümünüzü uygulayabilir veya Uygulama Hizmeti'nin bunu sizin için işlemesine izin verebilirsiniz. Kimlik doğrulama ve yetkilendirme modülü, web isteklerini uygulama kodunuza teslim etmeden önce işler ve kodunuza ulaşmadan önce yetkisiz istekleri reddeder.

Uygulama Hizmeti kimlik doğrulaması ve yetkilendirmesi, Azure Active Directory, Microsoft hesapları, Facebook, Google ve Twitter dahil olmak üzere birden çok kimlik doğrulama sağlayıcısını destekler. Daha fazla bilgi için bkz. [Azure App Service’de kimlik doğrulama ve yetkilendirme](overview-authentication-authorization.md).

## <a name="service-to-service-authentication"></a>Hizmetten hizmete kimlik doğrulaması

Bir arka uç hizmetine karşı kimlik doğrulaması yaparken, App Service ihtiyacınıza bağlı olarak iki farklı mekanizma sağlar:

- **Servis kimliği** - Uygulamanın kimliğini kullanarak uzak kaynakta oturum açın. Uygulama Hizmeti, [Azure SQL Veritabanı](/azure/sql-database/) veya [Azure Key Vault](/azure/key-vault/)gibi diğer hizmetlerle kimlik doğrulamayapmak için kullanabileceğiniz yönetilen bir [kimliği](overview-managed-identity.md)kolayca oluşturmanıza olanak tanır. Bu yaklaşımın uçtan uca bir öğreticisi için, [yönetilen bir kimlik kullanarak Uygulama Hizmetinden Güvenli Azure SQL Veritabanı bağlantısına](app-service-web-tutorial-connect-msi.md)bakın.
- **Adına(OBO)** - Kullanıcı adına uzak kaynaklara temsilci erişimi yapın. Azure Active Directory kimlik doğrulama sağlayıcısı olarak uygulama nız, [Microsoft Graph API](../active-directory/develop/microsoft-graph-intro.md) veya Uygulama Hizmeti'ndeki uzak bir API uygulaması gibi uzak bir hizmete devredilen oturum açma gerçekleştirebilir. Bu yaklaşımın uçtan uca bir öğreticisi için Azure [Uygulama Hizmeti'nde kimlik doğrulaması ve kullanıcıları uçtan uca yetkilendirme bölümüne](app-service-web-tutorial-auth-aad.md)bakın.

## <a name="connectivity-to-remote-resources"></a>Uzak kaynaklara bağlantı

Uygulamanızın erişmek için ihtiyaç duyabileceği üç tür uzak kaynak vardır: 

- [Azure kaynakları](#azure-resources)
- [Azure Sanal Ağı'nın içindeki kaynaklar](#resources-inside-an-azure-virtual-network)
- [Şirket içi kaynaklar](#on-premises-resources)

Bu durumların her birinde, App Service güvenli bağlantılar kurmanız için bir yol sağlar, ancak yine de güvenlik en iyi uygulamalarını gözlemlemeniz gerekir. Örneğin, arka uç kaynağı şifrelenmemiş bağlantılara izin verse bile her zaman şifreli bağlantıları kullanın. Ayrıca, arka uç Azure hizmetinizin minimum IP adresi kümesine izin verdiğinden emin olun. Uygulamanızın giden IP adreslerini [Azure Uygulama Hizmeti'nde Gelen ve giden IP adreslerinde](overview-inbound-outbound-ips.md)bulabilirsiniz.

### <a name="azure-resources"></a>Azure kaynakları

Uygulamanız [SQL Veritabanı](https://azure.microsoft.com/services/sql-database/) ve [Azure Depolama](/azure/storage/)gibi Azure kaynaklarına bağlandığında, bağlantı Azure içinde kalır ve ağ sınırlarını aşmaz. Ancak, bağlantı Azure'daki paylaşılan ağ üzerinden geçer, bu nedenle bağlantınızın her zaman şifrelenmiş olduğundan emin olun. 

Uygulamanız bir Uygulama [Hizmeti ortamında](environment/intro.md)barındırılıyorsa, [Sanal Ağ hizmeti bitiş noktalarını kullanarak desteklenen Azure hizmetlerine bağlanmalısınız.](../virtual-network/virtual-network-service-endpoints-overview.md)

### <a name="resources-inside-an-azure-virtual-network"></a>Azure Sanal Ağı'nın içindeki kaynaklar

[Uygulamanız, Sanal Ağ tümleştirmesi](web-sites-integrate-with-vnet.md)aracılığıyla [Bir Azure Sanal Ağı'ndaki](/azure/virtual-network/) kaynaklara erişebilir. Tümleştirme, bir sanal ağ ile bir noktadan siteye VPN kullanılarak kurulur. Uygulama daha sonra sanal ağdaki kaynaklara kendi özel IP adreslerini kullanarak erişebilir. Ancak, noktaya bağlantı, Azure'daki paylaşılan ağlarda hala geçiş yapmaya devam eder. 

Kaynak bağlantınızı Azure'daki paylaşılan ağlardan tamamen yalıtmak için [uygulamanızı](environment/intro.md)bir Uygulama Hizmeti ortamında oluşturun. Bir Uygulama Hizmeti ortamı her zaman özel bir Sanal Ağa dağıtıldığından, uygulamanız ve Sanal Ağ içindeki kaynaklar arasındaki bağlantı tamamen izole edilmiştir. Bir Uygulama Hizmeti ortamındaağ güvenliğinin diğer yönleri için [Bkz. Ağ yalıtımı.](#network-isolation)

### <a name="on-premises-resources"></a>Şirket içi kaynaklar

Veritabanları gibi şirket içi kaynaklara üç şekilde güvenle erişebilirsiniz: 

- [Karma bağlantılar](app-service-hybrid-connections.md) - TCP tüneli aracılığıyla uzak kaynağınıza noktadan noktaya bağlantı kurar. TCP tüneli, paylaşılan erişim imzası (SAS) anahtarları ile TLS 1.2 kullanılarak kurulmuştur.
- Siteden siteye VPN ile [Sanal Ağ entegrasyonu](web-sites-integrate-with-vnet.md) - Azure Sanal Ağ içindeki [Kaynaklar'da](#resources-inside-an-azure-virtual-network)açıklandığı gibi, ancak Sanal Ağ [siteden siteye VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)aracılığıyla şirket içi ağınıza bağlanabilir. Bu ağ topolojisinde, uygulamanız Sanal Ağdaki diğer kaynaklar gibi şirket içi kaynaklara bağlanabilir.
- Siteden siteye VPN ile [Uygulama Hizmeti ortamı](environment/intro.md) - Azure Sanal Ağ içindeki [Kaynaklar'da](#resources-inside-an-azure-virtual-network)açıklandığı gibi, ancak Sanal Ağ [siteden siteye VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)aracılığıyla şirket içi ağınıza bağlanabilir. Bu ağ topolojisinde, uygulamanız Sanal Ağdaki diğer kaynaklar gibi şirket içi kaynaklara bağlanabilir.

## <a name="application-secrets"></a>Uygulama sırları

Veritabanı kimlik bilgileri, API belirteçleri ve özel anahtarlar gibi uygulama sırlarını kod veya yapılandırma dosyalarınızda depolamayın. Yaygın olarak kabul edilen yaklaşım, seçtiğiniz dilde standart deseni kullanarak [ortam değişkenleri](https://wikipedia.org/wiki/Environment_variable) olarak bunlara erişmektir. Uygulama Hizmeti'nde, ortam değişkenlerini tanımlamanın yolu [uygulama ayarlarından](configure-common.md#configure-app-settings) (ve özellikle .NET uygulamaları için [bağlantı dizeleri)](configure-common.md#configure-connection-strings)geçer. Uygulama ayarları ve bağlantı dizeleri Azure'da şifrelenir ve uygulama başladığında uygulamanızın işlem bellene enjekte edilmeden önce yalnızca şifresi çözülür. Şifreleme anahtarları düzenli olarak döndürülür.

Alternatif olarak, gelişmiş sırlar yönetimi için Uygulama Hizmeti uygulamanızı [Azure Key Vault](/azure/key-vault/) ile entegre edebilirsiniz. [Anahtar Kasası'na yönetilen bir kimlikle erişerek,](../key-vault/tutorial-web-application-keyvault.md)Uygulama Hizmeti uygulamanız ihtiyacınız olan sırlara güvenli bir şekilde erişebilir.

## <a name="network-isolation"></a>Ağ yalıtımı

**Yalıtılmış** fiyatlandırma katmanı dışında, tüm katmanlar uygulamalarınızı App Service'deki paylaşılan ağ altyapısında çalıştırın. Örneğin, genel IP adresleri ve ön uç yük dengeleyicileri diğer kiracılarla paylaşılır. **İzole** katman, uygulamalarınızı özel bir Uygulama [Hizmeti ortamında](environment/intro.md)çalıştırarak tam ağ yalıtımı sağlar. Bir Uygulama Hizmeti ortamı Azure [Sanal Ağı'nın](/azure/virtual-network/)kendi örneğinde çalışır. Bu size sağlar: 

- Uygulamalarınızı özel bir genel uç noktası üzerinden, özel ön uçlarıyla servis edin.
- Yalnızca Azure Sanal Ağınızın içinden erişime olanak tanıyan dahili yük dengeleyicisini (ILB) kullanarak dahili uygulamalara hizmet verin. ILB'nin özel alt netinizden gelen ve uygulamalarınızın internetten tamamen izole edilmesine izin veren bir IP adresi vardır.
- [Bir web uygulaması güvenlik duvarı (WAF) arkasında bir ILB kullanın.](environment/integrate-with-application-gateway.md) WAF, DDoS koruması, URI filtreleme ve SQL enjeksiyon önleme gibi kamuya açık uygulamalarınız için kurumsal düzeyde koruma sağlar.

Daha fazla bilgi için azure [uygulama hizmeti ortamlarına giriş](environment/intro.md)için bkz. 
