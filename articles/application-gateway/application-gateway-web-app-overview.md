---
title: Çok kiracılı arka uçlar
titleSuffix: Azure Application Gateway
description: Bu sayfada, Application Gateway’in çok kiracılı arka uç desteği için genel bir bakış sunulmuştur.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: victorh
ms.openlocfilehash: efa2885ce0534c5d78bb08bbf24da59850f6ea22
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74075186"
---
# <a name="application-gateway-support-for-multi-tenant-back-ends-such-as-app-service"></a>Uygulama hizmeti gibi çok kiracılı arka uçlar için Uygulama Ağ Geçidi desteği

Web sunucularında çok kiracılı mimari tasarımlarda, aynı web sunucusu örneğinde birden çok web sitesi çalışır. Ana bilgisayar adları, barındırılan farklı uygulamaları ayırt etmek için kullanılır. Varsayılan olarak, application gateway istemciden gelen HTTP ana bilgisayar üst bilgisini değiştirmez ve üst bilgiyi değiştirilmemiş halde arka uca gönderir. Bu, NIC'ler, sanal makine ölçek kümeleri, genel IP adresleri, dahili IP adresleri ve FQDN gibi arka uç havuz üyeleri için iyi çalışır, çünkü bunlar doğru bitiş noktasına kadar çözmek için belirli bir ana bilgisayar üstbilgisine veya SNI uzantısına güvenmez. Ancak, Azure App hizmeti web uygulamaları ve Azure API yönetimi gibi, doğada çok kiracılı olan ve doğru bitiş noktasına kadar çözüme kavuşturulması için belirli bir ana bilgisayar üstbilgisine veya SNI uzantısına güvenen birçok hizmet vardır. Genellikle, uygulamanın DNS adı, sırayla uygulama ağ geçidi ile ilişkili DNS adı, arka uç hizmetinin etki alanı adından farklıdır. Bu nedenle, uygulama ağ geçidi tarafından alınan özgün istekteki ana bilgisayar üstbilgi, arka uç hizmetinin ana bilgisayarının adı ile aynı değildir. Bu nedenle, uygulama ağ geçidinden arka uça gelen istekteki ana bilgisayar üstbilgi arka uç hizmetinin ana bilgisayarı olarak değiştirilmediği sürece, çok kiracılı arka uçlar isteği doğru bitiş noktasına çözümlemez. 

Application Gateway kullanıcılara, arka ucun ana bilgisayar adına göre istekteki HTTP barındırma üst bilgisini geçersiz kılma olanağı tanıyan bir özellik sunar. Bu özellik Azure App Service web uygulamaları ve API Management gibi çok kiracılı arka uçlar için destek sağlar. Bu özellik hem v1 ve v2 standardı hem de WAF SKU’larında kullanılabilir. 

![ana bilgisayar geçersiz kılma](./media/application-gateway-web-app-overview/host-override.png)

> [!NOTE]
> ASE, çok kiracılı bir kaynak olan Azure App hizmetinin aksine özel bir kaynak olduğundan, bu durum Azure Uygulaması hizmet ortamı (ASE) için geçerli değildir.

## <a name="override-host-header-in-the-request"></a>İstekteki ana bilgisayar üstbilgisini geçersiz kılma

Ana bilgisayar geçersiz kılma özelliği HTTP [ayarlarında](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http-settings) tanımlanır ve kural oluşturma sırasında herhangi bir arka uç havuzuna uygulanabilir. Çok kiracılı arka uçlar için ana bilgisayar üstbilgisini ve SNI uzantısını geçersiz kılmanın aşağıdaki iki yolu desteklenir:

- Ana bilgisayar adını, HTTP ayarlarına açıkça girilen sabit bir değere ayarlama yeteneği. Bu özellik, ana bilgisayar üstbilgisinin, belirli HTTP ayarlarının uygulandığı arka uç havuzuna yapılan tüm trafik için bu değere geçersiz kılınmasını sağlar. Uçtan uca SSL kullanılırken, geçersiz kılınan bu ana bilgisayar adı SNI uzantısında kullanılır. Bu özellik, bir arka uç havuz eksede gelen müşteri ana bilgisayar üstbilgisinden farklı bir ana bilgisayar üstbilgi beklediği senaryolara olanak tanır.

- Arka uç havuz üyelerinin IP veya FQDN'sinden ana bilgisayar adını elde etme yeteneği. HTTP ayarları, tek bir arka uç havuzu üyesinden ana bilgisayar adı türetme seçeneğiyle yapılandırılırsa, arka uç havuzu üyesinin FQDN'sinden ana bilgisayar adını dinamik olarak seçme seçeneği de sağlar. Uçtan uca SSL kullanılırken, bu ana bilgisayar adı FQDN’den türetilir ve SNI uzantısında kullanılır. Bu özellik, bir arka uç havuzunun Azure web uygulamaları gibi iki veya daha fazla çok kiracılı PaaS hizmetine sahip olabileceği ve her üyeye istek ana bilgisayar üstbilgisinin FQDN'sinden türetilen ana bilgisayar adını içebileceği senaryolar sağlar. Bu senaryoyu uygulamak için, http ayarlarında arka [uç adresinden ana bilgisayar adını seç](https://docs.microsoft.com/azure/application-gateway/configuration-overview#pick-host-name-from-back-end-address) adlı bir anahtar kullanırız ve bu anahtar, orijinal istekteki ana bilgisayar üstbilgisini arka uç havuzunda belirtilene dinamik olarak geçersiz kılar.  Örneğin, arka uç havuzunuz FQDN "contoso11.azurewebsites.net" ve "contoso22.azurewebsites.net" içeriyorsa, özgün isteğin ana bilgisayar üstbilgisi contoso.com veya contoso11.azurewebsites.net veya contoso22.azurewebsites.net istek uygun arka uç sunucusuna gönderildiğinde. 

  ![web uygulaması senaryosu](./media/application-gateway-web-app-overview/scenario.png)

Bu özellik sayesinde müşteriler, HTTP ayarları ve özel araştırmalardaki seçenekleri uygun yapılandırmaya göre belirtebilir. Bu ayar daha sonra bir kural kullanılarak bir dinleyici ve bir arka uç havuzuna bağlanır.

## <a name="special-considerations"></a>Özel hususlar

### <a name="ssl-termination-and-end-to-end-ssl-with-multi-tenant-services"></a>SSL sonlandırma ve çok kiracıhizmetleri ile Sona SSL sonuna kadar

Hem SSL sonlandırma hem de uçuçsA SSL şifrelemesi çok kiracılı hizmetlerle desteklenir. Uygulama ağ geçidindeki SSL sonlandırma için, uygulama ağ geçidi dinleyicisine eklenmesi için SSL sertifikası nın eklenmesi gerekir. Ancak, SSL'nin sona ermesi durumunda, Azure Uygulaması hizmeti web uygulamaları gibi güvenilir Azure hizmetleri, uygulama ağ geçidinde arka uçların beyaz listeye alınmasını gerektirmez. Bu nedenle, kimlik doğrulama sertifikaları eklemenize gerek yoktur. 

![sonuna kadar SSL](./media/application-gateway-web-app-overview/end-to-end-ssl.png)

Yukarıdaki resimde, Uygulama hizmeti arka uç olarak seçildiğinde kimlik doğrulama sertifikaları eklemezorunluluğu olmadığına dikkat edin.

### <a name="health-probe"></a>Durum yoklaması

**HTTP ayarlarında** ana bilgisayar üstbilgisini geçersiz kılmak yalnızca isteği ve yönlendirmeyi etkiler. sağlık sondası davranışını etkilemez. Uçtan uca işlevselliğin çalışması için hem araştırma hem de HTTP ayarları doğru yapılandırmayı yansıtacak şekilde değiştirilmelidir. Özel sondalar, sonda yapılandırmasında ana bilgisayar üstbilgisini belirtme olanağı sağlamanın yanı sıra, ana bilgisayar üstbilgisini şu anda yapılandırılmış HTTP ayarlarından türetme yeteneğini de destekler. Bu yapılandırma, araştırma yapılandırmasındaki `PickHostNameFromBackendHttpSettings` parametresi kullanılarak belirtilebilir.

### <a name="redirection-to-app-services-url-scenario"></a>Uygulama Hizmeti'nin URL senaryosuna yeniden yönlendirme

Uygulama hizmetinden gelen yanıttaki ana bilgisayar adının, Uygulama Ağ Geçidi ile ilişkili etki alanı yerine son kullanıcı tarayıcısını *.azurewebsites.net ana bilgisayar adına yönlendirebileceği senaryolar olabilir. Bu sorun şu anda oluşabilir:

- Uygulama Hizmetinizde yeniden yönlendirme yapılandırıldı. Yeniden yönlendirme, isteğe bir çizgi eklemek kadar basit olabilir.
- Yeniden yönlendirmeye neden olan Azure AD kimlik doğrulaması vardır.

Bu gibi durumları çözmek [için, Uygulama hizmetinin URL sorununa yeniden yönlendirme sorununu](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url)bkz.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama Ağ Geçidi ile [Yapılandır Uygulama Hizmeti web uygulamalarını](https://docs.microsoft.com/azure/application-gateway/configure-web-app-portal) ziyaret ederek, arka uç havuzu üyesi olarak Azure App hizmeti web uygulaması gibi çok kiracılı bir uygulamayla uygulama ağ geçidi nasıl ayarlayabilirsiniz öğrenin
