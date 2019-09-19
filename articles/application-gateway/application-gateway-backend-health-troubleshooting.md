---
title: Azure Application Gateway arka uç sistem durumu sorunlarını giderme
description: Bu makale, Azure Application Gateway arka uç sistem durumu sorunlarını giderme konusunda size rehberlik eder
services: application-gateway
author: surajmb
ms.service: application-gateway
ms.topic: article
ms.date: 08/30/2019
ms.author: surmb
ms.openlocfilehash: 1fd4e9156e29133b1db4fe9ab9a0825eb1aa3b55
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71097588"
---
<a name="troubleshoot-backend-health-issues-in-application-gateway"></a>Application Gateway arka uç sistem durumu sorunlarını giderme
==================================================

<a name="overview"></a>Genel Bakış
--------

Application Gateway, varsayılan olarak arka uç sunucularını, sistem durumunu denetlemek için yoklamaları ve isteklere hazırlanmaya hazır olup olmadığını denetler. Kullanıcılar ana bilgisayar adının yanı sıra, araştırılan yol ve durum kodlarının sağlıklı olarak kabul edileceği özel yoklamalar oluşturabilir. Her iki durumda da, arka uç sunucusu başarıyla yanıt vermiyorsa, Application Gateway sunucuyu sağlıksız olarak işaretler ve isteği sunucuya iletmeyi durdurur. Sunucu başarıyla yanıt vermeye başladıktan sonra isteklere hizmet vermeye sürdürülecek.

### <a name="how-to-check-backend-health"></a>Arka uç durumunu denetleme

Arka uç havuzunuzun durumunu denetlemek için, Azure portal "arka uç durumu" sayfasını kullanabilirsiniz. Ya da sistem durumunu almak için [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.network/get-azapplicationgatewaybackendhealth?view=azps-2.6.0), [CLI](https://docs.microsoft.com/cli/azure/network/application-gateway?view=azure-cli-latest#az-network-application-gateway-show-backend-health)veya [REST API](https://docs.microsoft.com/rest/api/application-gateway/applicationgateways/backendhealth) kullanabilirsiniz. Daha fazla bilgi edinmek için her bir yönteme karşı bağlantılı makaleye bakın.

Yukarıda bahsedilen herhangi bir yöntem tarafından alınan durum üç türde olabilir:

1.  Sorunsuz

2.  İyi durumda değil

3.  Bilinmiyor

Bir sunucunun arka uç sistem durumu sağlıklı ise, Application Gateway istekleri bu sunucuya iletecektir. Ancak arka uç havuzundaki tüm sunucuların arka uç durumu sağlıksız veya bilinmiyor ise, uygulama erişimi sırasında bazı sorunlar ortaya çıkabilir. Bu belgede, arka uç sunucuları sağlıksız veya bilinmiyor olduğunda gösterilen hataların her birinin belirtisi, nedeni ve çözümü açıklanmaktadır.

<a name="backend-health-status-unhealthy"></a>Arka uç sistem durumu sağlıksız
-------------------------------

Arka uç sistem durumu sağlıksız olarak gösteriliyorsa, portalda aşağıdaki ekran görüntüsünde olduğu gibi görüntülenir:

![Application Gateway arka uç durumu-sağlıksız](./media/application-gateway-backend-health-troubleshooting/appgwunhealthy.png)

Ya da Azure PowerShell, CLı veya Azure REST API sorgusu kullanıyorsanız, aşağıdakine benzer bir yanıt göreceksiniz:
```azurepowershell
PS C:\Users\testuser\> Get-AzApplicationGatewayBackendHealth -Name "appgw1" -ResourceGroupName "rgOne"
BackendAddressPools :
{Microsoft.Azure.Commands.Network.Models.PSApplicationGatewayBackendHealthPool}
BackendAddressPoolsText : [
{
                              "BackendAddressPool": {
                                "Id": "/subscriptions/536d30b8-665b-40fc-bd7e-68c65f816365/resourceGroups/rgOne/providers/Microsoft.Network/applicationGateways/appgw1/b
                          ackendAddressPools/appGatewayBackendPool"
                              },
                              "BackendHttpSettingsCollection": [
                                {
                                  "BackendHttpSettings": {
                                    "TrustedRootCertificates": [],
                                    "Id": "/subscriptions/536d30b8-665b-40fc-bd7e-68c65f816365/resourceGroups/rgOne/providers/Microsoft.Network/applicationGateways/appg
                          w1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
                                  },
                                  "Servers": [
                                    {
                                      "Address": "10.0.0.5",
                                      "Health": "Healthy"
                                    },
                                    {
                                      "Address": "10.0.0.6",
                                      "Health": "Unhealthy"
                                    }
                                  ]
                                }
                              ]
                            }
                        ]
```
Arka uç sunucu durumunu bir arka uç havuzundaki tüm sunucular için sağlıksız olarak görtikten sonra, istekler bunlara iletilecektir ve Application Gateway istenen istemciye 502 hatalı ağ geçidi hatası döndürür. Sorunu gidermek için, arka uç sistem durumu sekmesinin Ayrıntılar sütununu kontrol edin.

Arka uç sistem durumu sekmesinin Ayrıntılar sütununda görünen ileti, sorun hakkında daha ayrıntılı öngörüler sağlar ve bunlar temelinde sorun gidermeye başlayabiliriz.

> [!NOTE]
> Varsayılan araştırma isteği, bağlantı noktası 80 ' de http \<araştırması\> <http://127.0.0.1/> için://127.0.0.1\<:\>bağlantı noktası/biçiminde gönderilir ve yalnızca şu şekilde 200-399 http durum kodlarının yanıtını kabul eder sağlıklı yanıt. Protokol ve hedef bağlantı noktası HTTP ayarlarından devralınır. Farklı bir protokol, ana bilgisayar adı veya yol üzerinde araştırmasını Application Gateway ve sağlıklı olarak farklı bir durum kodu kabul etmek istiyorsanız, özel bir araştırma yapılandırın ve bunu HTTP ayarları ile ilişkilendirin.

<a name="error-messages"></a>Hata İletileri
------------------------
#### <a name="backend-server-timeout"></a>Arka uç sunucu zaman aşımı

**İleti:** Uygulama ağ geçidi\'s sistem durumu araştırmasına yanıt vermek için arka uçta geçen süre, araştırma ayarında zaman aşımı eşiğinden daha fazla.

**Sağlamak** Application Gateway, arka uç sunucusuna HTTP (S) araştırma isteği gönderdiğinde, belirli bir süre için arka uç sunucusundan gelen yanıtı bekler. Arka uç sunucusu zaman aşımı değeri içinde yanıt vermiyorsa, zaman aşımı süresi içinde yeniden yanıt vermemeye başlanana kadar sağlıksız olarak işaretlenir.

**Çözüm:** Yapılandırılan zaman aşımı süresi içinde yanıt vermediği için arka uç sunucusunu veya uygulamayı denetleyin ve uygulama bağımlılıklarını da kontrol edin. Örneğin, veritabanında herhangi bir sorun varsa, yanıt verme gecikmesine neden olabilir. Uygulamanın davranışının farkındaysanız ve yalnızca zaman aşımı değerinden sonra yanıt vermesi gerekiyorsa, özel araştırma ayarlarından zaman aşımı değerini artırın.
Zaman aşımı değerini değiştirmek için özel bir araştırmanız gerekir. Özel bir araştırmanın nasıl yapılandırılacağını öğrenmek isterseniz [Belgeler sayfasını kontrol](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal)edin.

Zaman aşımını artırmak için aşağıdaki adımları izleyin:

1.  Arka uç sunucusuna doğrudan erişin ve sunucunun bu sayfada yanıt vermesi için geçen süreyi denetleyin. Geliştirici araçlarını kullanarak bir tarayıcı dahil olmak üzere herhangi bir aracı kullanabilirsiniz.

2.  Uygulamanın yanıt vermesi için geçen süreyi iletişime aldıktan sonra, sistem durumu araştırmaları sekmesine tıklayın ve HTTP ayarlarınızla ilişkili araştırmayı seçin.

3.  Uygulama yanıt süresinden daha yüksek bir zaman aşımı değerini saniye cinsinden girin.

4.  Özel araştırma ayarlarını kaydedin ve şimdi sağlıklı durumu gösteriyorsa arka uç durumunu kontrol edin.

#### <a name="dns-resolution-error"></a>DNS çözümleme hatası

**İleti:** Application Gateway bu arka uç için bir araştırma oluşturamadı. Bu genellikle arka ucun FQDN 'SI doğru girildiyse oluşur. 

**Sağlamak** Arka uç havuzu IP adresi/FQDN veya App Service türünde ise, Application Gateway DNS sunucusu (özel veya Azure varsayılan) kullanılarak girilen FQDN 'nin IP adresini çözümler ve HTTP ayarlarında belirtilen TCP bağlantı noktasındaki sunucuya bağlanmaya çalışır. Ancak bu ileti görüntüleniyorsa Application Gateway, girilen FQDN 'nin IP adresini başarıyla çözümleyemediğini önerir.

**Çözüm:**

1.  Arka uç havuzunda girilen FQDN 'nin doğru olup olmadığını ve ortak bir etki alanı olduğunu doğrulayın, yerel makinenizden çözümlemeyi deneyin.

2.  IP adresini çözümleyebiliyorsanız VNet 'teki DNS yapılandırmasında bir sorun olabilir.

3.  VNet 'in özel bir DNS sunucusu ile yapılandırılıp yapılandırılmadığını denetleyin. Bu durumda, DNS sunucusunu, verilen FQDN 'nin IP adresine neden çözemediğini denetleyin.

4.  Azure varsayılan DNS ise, doğru bir kayıt veya CNAME kaydı eşlemesi yapıldıysa etki alanı adı kayıt şirketinize danışın.

5.  Etki alanı özel/iç ise, aynı VNet 'teki bir VM 'den çözmeyi deneyin ve bu sorunu çözebiliyorsanız Application Gateway yeniden başlatın ve yeniden denetleyin. Application Gateway yeniden başlatmak için, ilgili belge bağlantılarında verilen PowerShell komutlarını kullanmaya [durdurup](https://docs.microsoft.com/powershell/module/azurerm.network/stop-azurermapplicationgateway?view=azurermps-6.13.0) [başlamanız](https://docs.microsoft.com/powershell/module/azurerm.network/start-azurermapplicationgateway?view=azurermps-6.13.0) gerekir.

#### <a name="tcp-connect-error"></a>TCP bağlantı hatası

**İleti:** Application Gateway, arka uca bağlanamadı.
Lütfen arka ucun araştırma için kullanılan bağlantı noktasında yanıt verdiğini kontrol edin.
Ayrıca, herhangi bir NSG/UDR/güvenlik duvarının IP 'ye ve bu arka ucun bağlantı noktasına erişimi engelleyip engellemediğini denetleyin

**Sağlamak** DNS çözümleme aşamasından sonra, Application Gateway HTTP ayarlarında yapılandırılan TCP bağlantı noktasındaki arka uç sunucusuna bağlanmaya çalışır. Application Gateway belirtilen bağlantı noktasında bir TCP oturumu kuramazsa, araştırma Bu iletiyle sağlıksız olarak işaretlenir.

**Çözüm:** Bu hatayı görürseniz, aşağıdakileri denetleyin:

1.  Bir tarayıcı kullanarak veya PowerShell kullanarak HTTP ayarlarında belirtilen bağlantı noktasında arka uç sunucusuna bağlanıp bağlanamadıysanız emin olun; Örneğin, komutunu kullanabilirsiniz: Test-NetConnection-ComputerName www.bing.com-Port 443

2.  Belirtilen bağlantı noktası istenen bağlantı noktası değilse, arka uç sunucusuna bağlanmak için Application Gateway doğru bağlantı noktası numarasını girin

3.  Bağlantı noktasında yerel makinenizden de bağlanamadıysanız:

    a.  Arka uç sunucusunun NIC ve alt ağının NSG ayarlarını denetleyin ve yapılandırılmış bağlantı noktasına gelen bağlantılara izin verilip verilmediğini denetleyin. İzin verilmiyorsa, bağlantılara izin vermek için yeni bir kural oluşturun. NSG kuralları oluşturmayı öğrenmek için [Belgeler sayfasını kontrol](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#create-security-rules)edin.

    b.  Giden genel ve özel trafiğe izin veriliyorsa, bağlantının yapılabilmesi için Application Gateway alt ağının NSG ayarlarını denetleyin. NSG kuralları oluşturma hakkında daha fazla bilgi edinmek için (a) ile bağlantılı belgeyi denetleyin.
    ```azurepowershell
            $vnet = Get-AzVirtualNetwork -Name "vnetName" -ResourceGroupName "rgName"
            Get-AzVirtualNetworkSubnetConfig -Name appGwSubnet -VirtualNetwork $vnet
    ```

    c.  Yönlendirme anormalları için Application Gateway ve arka uç sunucusu alt ağının UDR ayarlarını denetleyin. UDR 'nin trafiği arka uç alt ağından uzakta yönlendirmediğinden emin olun. Örneğin, ExpressRoute/VPN aracılığıyla Application Gateway alt ağına tanıtılan ağ sanal gereçlerine veya varsayılan yollara yönelik yolları denetleyin.

    d.  Bir NIC için geçerli yolları ve kuralları denetlemek için aşağıdaki PowerShell komutlarını kullanabilirsiniz.
    ```azurepowershell
            Get-AzEffectiveNetworkSecurityGroup -NetworkInterfaceName "nic1" -ResourceGroupName "testrg"
            Get-AzEffectiveRouteTable -NetworkInterfaceName "nic1" -ResourceGroupName "testrg"
    ```
4.  NSG veya UDR ile ilgili herhangi bir sorun bulamazsanız, hangi istemcilerin yapılandırılmış bağlantı noktaları üzerinde bir TCP oturumu kuramadıkları için arka uç sunucunuza uygulamayla ilgili sorunlar olup olmadığını denetleyin. Denetlenecek birkaç şey:

    e.  Komut istemi 'ni (Win + R-\> cmd) açın ve netstat yazıp ENTER tuşuna basın

    f.  Sunucunun yapılandırılan bağlantı noktasını dinleyip dinlemediğini denetleyin.
        Örneğin:
    ```
            Proto Local Address Foreign Address State PID
            TCP 0.0.0.0:80 0.0.0.0:0 LISTENING 4
    ```
    g.  Değilse, Web sunucusu ayarlarınıza göz atın, örneğin IIS 'deki site bağlamaları, NGıNX içindeki sunucu bloğu ve Apache 'de sanal ana bilgisayar

    h.  Bağlantı noktasına gelen trafiğe izin verildiğinden emin olmak için işletim sistemi güvenlik duvarı ayarlarınızı denetleyin

#### <a name="http-status-code-mismatch"></a>HTTP durum kodu uyumsuzluğu

**İleti:** Arka uç\'s HTTP yanıtının durum kodu araştırma ayarıyla eşleşmedi. Beklenen: {HTTPStatusCode0} alındı: {HTTPStatusCode1}.

**Sağlamak** TCP bağlantısı kurulduktan ve SSL el sıkışması yapıldıktan sonra (SSL etkinse), Application Gateway arka uç sunucusuna bir HTTP GET isteği olarak araştırmaya gönderilir. Yukarıda belirtildiği gibi, varsayılan \<araştırma protokol\>://127.0.0.1:\<Port\>/olur ve Rage 200-399 yanıt durum kodlarını sağlıklı olarak kabul eder. Sunucu başka bir durum kodu döndürürse, Bu iletiyle sağlıksız olarak işaretlenir.

**Çözüm:** Arka uç sunucusunun yanıt koduna bağlı olarak, aşağıdaki adımları gerçekleştirebilirsiniz. Yaygın durum kodlarından bazıları aşağıda listelenmiştir:

| **Hata:** | **Eylemler** |
| --- | --- |
| Araştırma durum kodu uyumsuzluğu: 401 alındı | Arka uç sunucusunun kimlik doğrulaması gerektirip gerektirmediğini denetleyin. Application Gateway araştırmaları kimlik bilgilerini bu noktada geçiremiyor. Araştırma durum \"kodu eşleşmesi\" veya araştırmasına yönelik http 401 ' i sunucunun kimlik doğrulaması gerektirmeyen bir yola izin verin. | |
| Araştırma durum kodu uyumsuzluğu: 403 alındı | Erişim yasak. Arka uç sunucusunda yola erişime izin verilip verilmediğini denetleyin. | |
| Araştırma durum kodu uyumsuzluğu: 404 alındı | Sayfa bulunamadı. Arka uç sunucusunda erişilebilir durumda değilse, ana bilgisayar adı yolunu denetleyin. Ana bilgisayar adı veya yol parametresini erişilebilir bir değer olarak değiştirin. | |
| Araştırma durum kodu uyumsuzluğu: 405 alındı | Application Gateway araştırma istekleri HTTP GET yöntemini kullanır. Sunucunuzun izin verip içermediğini denetleyin. | |
| Araştırma durum kodu uyumsuzluğu: 500 alındı | İç sunucu hatası. Arka uç sunucusunun sistem durumunu ve hizmetler çalışıp çalışmadığını denetleyin. | |
| Araştırma durum kodu uyumsuzluğu: 503 alındı | Hizmet kullanılamıyor. Arka uç sunucusunun sistem durumunu ve hizmetler çalışıp çalışmadığını denetleyin. | |

Ya da yanıtın uygun olduğunu düşünüyorsanız ve Application Gateway diğer durum kodlarını sağlıklı olarak kabul etmek istiyorsanız, özel bir araştırma oluşturabilirsiniz. Bunun değiştirilmesi, arka uç Web sitesinin kimlik doğrulaması gereken durumlarda ve araştırma istekleri herhangi bir kullanıcı kimlik bilgisi taşımadığından yararlı olur ve arka uç sunucusu tarafından bir HTTP 401 durum kodu döndürülür.

Özel bir araştırma oluşturmak için [burada](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal)listelenen adımları izleyin.

#### <a name="http-response-body-mismatch"></a>HTTP yanıt gövdesi uyumsuzluğu

**İleti:** Arka uç\'s HTTP yanıtının gövdesi, araştırma ayarıyla eşleşmedi. Alınan yanıt gövdesi {String} içermiyor.

**Sağlamak** Özel bir araştırma oluşturduğunuzda, yanıt gövdesinden bir dizeyi eşleştirerek arka uç sunucusunu sağlıklı bir şekilde işaretleme seçeneğiniz vardır. Örneğin, Application Gateway eşleşecek bir dize olarak "yetkisiz" kabul edecek şekilde yapılandırabilirsiniz. Araştırma isteği için arka uç sunucu yanıtı "yetkisiz" dizesini içeriyorsa, bu durum sağlıklı olarak işaretlenir.
Aksi takdirde, Bu iletiyle sağlıksız olarak işaretlenir.

**Çözüm:** Aşağıdaki adımları izleyerek bu sorunu çözebilirsiniz:

1.  Arka uç sunucusuna veya araştırma yolundaki bir istemci makineden yerel olarak erişin ve yanıt gövdesini denetleyin.

2.  Yanıt gövdesinin yapılandırılanlarla eşleşip eşleşmediğini doğrulamak için Application Gateway özel araştırma yapılandırmasını denetleyin.

3.  Eşleşiyorlarsa, araştırma yapılandırmasını kabul edilecek doğru dize değeriyle değiştirin.

Application Gateway araştırma eşleştirmesi hakkında daha fazla bilgiyi [burada](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#probe-matching)bulabilirsiniz.

#### <a name="backend-server-certificate-invalid-ca"></a>Arka uç sunucu sertifikası geçersiz CA

**İleti:** Arka uç tarafından kullanılan sunucu sertifikası iyi bilinen bir sertifika yetkilisi (CA) tarafından imzalanmadı. Arka uç tarafından kullanılan sunucu sertifikasının kök sertifikasını karşıya yükleyerek uygulama ağ geçidinde arka ucunu beyaz listeye ekleyin.

**Sağlamak** Application Gateway v2 ile uçtan uca SSL, arka uç sunucusunun sertifikasının Sunucu sağlıklı olduğunu bozmayacak şekilde doğrulanmasını gerektirir.
Bir SSL sertifikasının güvenilir olması için, arka uç sunucusunun bu sertifikasının Application Gateway güvenilen deposunda bulunan bir CA tarafından verilmiş olması gerekir. Sertifika, güvenilir bir CA tarafından verilmamışsa (örneğin, otomatik olarak imzalanan sertifikalar), kullanıcıların sertifikayı verenin sertifikasını Application Gateway yüklemesi gerekir.

**Çözüm:** Güvenilen kök sertifikayı dışarı aktarmak ve Application Gateway yüklemek için aşağıdaki adımları izleyin (aşağıda verilen adımlar Windows istemcileri içindir)

1.  Uygulamanızın barındırıldığı makinede oturum açın

2.  Win + R tuşuna basarak veya Başlat düğmesine sağ tıklayıp Çalıştır ' ı seçerek çalıştırma ' yı açın.

3.  Certmgr. msc yazın ve ENTER tuşuna basın. Ayrıca, başlangıç menüsünde Sertifika Yöneticisi ' ni de arayabilirsiniz.

4.  Sertifikayı, genellikle sertifikalar-geçerli \'Kullanıcı\\kişisel\\sertifikaları\'' nda bulun ve sertifikayı açın

5.  Sertifika özelliklerinde, sertifika yolu sekmesini seçin.

6.  Kök sertifikayı seçin ve "sertifikayı görüntüle" seçeneğini belirleyin

7.  Sertifika özelliklerinde, Ayrıntılar sekmesine geçin

8.  Ayrıntılar sekmesinde "Dosyaya Kopyala" seçeneğini belirleyin ve dosyayı Base-64 ile kodlanmış X. 509.440 (. CER) biçimi

9.  Kaydedildikten sonra, Azure portal Application Gateway HTTP ayarları sayfasını açın

10. HTTP ayarlarını açın ve "sertifika ekle" ye tıklayın ve yakın zamanda kaydettiğiniz sertifika dosyasını bulun

11. HTTP ayarlarını kaydetmek için Kaydet ' e tıklayın

Alternatif olarak, bir tarayıcı kullanarak sunucuya doğrudan erişerek (Application Gateway atlayarak) ve kök sertifikayı tarayıcıdan dışarı aktararak bir istemci makinesinden kök sertifikayı dışarı aktarabilirsiniz.

Güvenilen kök sertifikaları ayıklama ve karşıya yükleme hakkında ayrıntılı adımlar için [Application Gateway bölümüne bakın](https://docs.microsoft.com/azure/application-gateway/certificates-for-backend-authentication#export-trusted-root-certificate-for-v2-sku).

#### <a name="trusted-root-certificate-mismatch"></a>Güvenilen kök sertifika uyumsuzluğu

**İleti:** Arka uç tarafından kullanılan sunucu sertifikasının kök sertifikası, uygulama ağ geçidine eklenmiş olan güvenilen kök sertifika ile eşleşmiyor. Arka ucun beyaz listeye doğru kök sertifikayı eklemediğinizden emin olun

**Sağlamak** Application Gateway v2 ile uçtan uca SSL, arka uç sunucusunun sertifikasının Sunucu sağlıklı olduğunu bozmayacak şekilde doğrulanmasını gerektirir.
Bir SSL sertifikasının güvenilir olması için, arka uç sunucusunun bu sertifikasının Application Gateway güvenilen deposunda bulunan bir CA tarafından verilmiş olması gerekir. Sertifika, güvenilir bir CA tarafından verilmamışsa (örneğin, otomatik olarak imzalanan sertifikalar), kullanıcıların sertifikayı verenin sertifikasını Application Gateway yüklemesi gerekir.

Application Gateway HTTP ayarlarına yüklenmiş olan sertifika, arka uç sunucu sertifikasının kök sertifikasıyla eşleşmelidir.

**Çözüm:** Yukarıda bahsedilen hata iletisini görürseniz, Application Gateway yüklenen sertifika ile arka uç sunucusuna yüklenen sertifika arasında bir uyumsuzluk olduğu anlamına gelir.

Application Gateway doğru güvenilen kök sertifikayı karşıya yüklemek için yukarıda bahsedilen 1-11 adımlarını izleyin.

Güvenilen kök sertifikaları ayıklama ve karşıya yükleme hakkında ayrıntılı adımlar için [Application Gateway bölümüne bakın](https://docs.microsoft.com/azure/application-gateway/certificates-for-backend-authentication#export-trusted-root-certificate-for-v2-sku).
> [!NOTE]
> Ayrıca, arka uç sunucusu, TLS el sıkışması sırasında kök > ara (varsa)-> yaprak dahil olmak üzere sertifikanın tam zincirini değiş tokuş etmez da meydana gelir. Doğrulamak için herhangi bir istemciden OpenSSL komutlarını kullanabilir ve Application Gateway araştırmasının yapılandırılmış ayarlarını kullanarak arka uç sunucusuna bağlanabilirsiniz.

Örneğin,
```
OpenSSL> s_client -connect 10.0.0.4:443 -servername www.example.com -showcerts
```
Çıktı, döndürülmekte olan sertifikanın tüm zincirini göstermezse, kök sertifika dahil olmak üzere tüm zincirle sertifikayı yeniden dışarı aktarın ve bunu arka uç sunucunuzda yapılandırın. 

BAĞLI (00000188) \
Derinlik = 0 OU = etki alanı denetimi doğrulanan, CN \*=. example.com \
doğrulama hatası: NUM = 20: yerel veren sertifikası alınamıyor \
dönüşü doğrula: 1 \
Derinlik = 0 OU = etki alanı denetimi doğrulanan, CN \*=. example.com \
doğrulama hatası: NUM = 21: ilk sertifika doğrulanamıyor \
dönüşü doğrula: 1 \
\-\-\-\
Sertifika zinciri \
 0 s:/OU = etki alanı denetimi doğrulanan/CN = *. example. com \
   ı:/C = US/St = Arizona/L = Scottsdale/O = GoDaddy. com, Inc./ou =http://certs.godaddy.com/repository//CN=Go Daddy Secure sertifika yetkilisi-G2 \
\----------SERTIFIKAYA BAŞLA
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
\-----SON SERTIFIKA-----

#### <a name="backend-certificate-invalid-common-name-cn"></a>Arka uç sertifikası geçersiz ortak adı (CN)

**İleti:** Arka uç sertifikasının ortak adı (CN), araştırmanın ana bilgisayar üstbilgisiyle eşleşmiyor.

**Sağlamak** Application Gateway, arka uç http ayarında belirtilen ana bilgisayar adının, arka uç sunucusunun SSL sertifikası tarafından sunulan ortak ad (CN) ile eşleşip eşleşmediğini doğrular. Bu yalnızca bir Standard_v2 ve WAF_v2 SKU davranışıdır. Standart ve WAF SKU 'sunun SNı, arka uç havuzu adresinde FQDN olarak ayarlanır.

V2 SKU 'sunda, varsayılan araştırma varsa (özel araştırma yapılandırılmadı/ilişkili), SNı HTTP ayarlarında belirtilen ana bilgisayar adından, veya "arka uç adresinden ana bilgisayar adı Seç" değeri, arka uç adres havuzunun bir geçerli FQDN.

HTTP ayarlarıyla ilişkili özel bir araştırma varsa, SNı özel araştırma yapılandırmasında belirtilen ana bilgisayar adından ayarlanır veya "arka uç HTTP ayarlarından ana bilgisayar adı Seç" işlemi özel araştırmayı işaretlenmişse, HTTP 'de belirtilen ana bilgisayar adından ayarlanır. Ayarlar.

Örnek olarak, "arka uç adresinden ana bilgisayar adı Seç" ayarı HTTP ayarlarında ayarlanır, arka uç adres havuzu geçerli bir FQDN içermelidir.

Yukarıda bahsedilen hata iletisini görürseniz, arka uç sertifikasının ortak adı (CN) özel araştırmakta veya HTTP ayarlarında yapılandırılan ana bilgisayar adıyla eşleşmez ("arka uç HTTP ayarlarından ana bilgisayar adı seçin" seçilir). Varsayılan bir araştırma kullanıyorsanız, ana bilgisayar adı "127.0.0.1" olarak ayarlanır. Bu istenen bir değer değilse, özel bir araştırma oluşturmanız ve bunu HTTP ayarlarıyla ilişkilendirmeniz gerekir.

**Çözüm:**

Sorunu çözmek için aşağıdaki adımları izleyin:

Windows için:

1.  Uygulamanızın barındırıldığı makinede oturum açın

2.  Win + R tuşuna basarak veya Başlat düğmesine sağ tıklayıp Çalıştır ' ı seçerek çalıştırma ' yı açın.

3.  Certmgr. msc yazın ve ENTER tuşuna basın. Ayrıca, başlangıç menüsünde Sertifika Yöneticisi ' ni de arayabilirsiniz.

4.  Sertifikayı, genellikle sertifikalar-geçerli \'Kullanıcı\\kişisel\\sertifikaları\'' nda bulun ve sertifikayı açın

5.  Ayrıntılar sekmesinde, sertifikanın konusunu denetleyin

6.  Ayrıntılardan sertifikanın CN 'sini doğrulayın ve özel araştırmanın veya HTTP ayarlarının ana bilgisayar adı alanına aynısını girin ("arka uç HTTP ayarlarından ana bilgisayar adı Seç" seçilirse). Bu, Web siteniz için istenen konak adı değilse, bu etki alanı için bir sertifika almanız veya özel araştırma/http ayarı yapılandırmasında doğru ana bilgisayar adını girmeniz gerekir.

OpenSSL kullanan Linux için

1.  OpenSSL içinde bu komutu çalıştırın 
```
openssl x509 -in certificate.crt -text -noout
```

2.  Görüntülenözellikler ' de, sertifikanın CN 'sini bulun ve http ayarlarının hostname alanına aynısını girin. Bu, Web siteniz için istenen konak adı değilse, bu etki alanı için bir sertifika almanız veya özel araştırma/http ayarı yapılandırmasında doğru ana bilgisayar adını girmeniz gerekir.

#### <a name="backend-certificate-is-invalid"></a>Arka uç sertifikası geçersiz

**İleti:** Arka uç sertifikası geçersiz. \"Geçerli tarih, sertifikadaki\" geçerli ve \"geçerli\" bir tarih aralığı içinde değil.

**Sağlamak** Sunucunun SSL sertifikası geçerli olmadığı takdirde, her sertifika bir geçerlilik süresiyle birlikte HTTPS bağlantısı güvenli olmayacaktır.
Geçerli veriler, öğesinden geçerli ve geçerli bir aralık dahilinde olmalıdır. Aksi takdirde, sertifika geçersiz olarak kabul edilir ve güvenlik konusu olacaktır. Bu noktada, Application Gateway arka uç sunucusunu sağlıksız olarak işaretleyecek.

**Çözüm:** SSL sertifikanızın geçerliliği dolmuşsa sertifikayı satıcınızla yenileyip sunucu ayarlarını yeni sertifikayla güncelleştirin. Otomatik olarak imzalanan bir sertifika ise, geçerli bir sertifika oluşturmanız ve kök sertifikayı Application Gateway HTTP ayarlarına yüklemeniz gerekir. Bunu yapmak için aşağıdaki adımları izleyin:

1.  Portalda Application Gateway HTTP ayarlarınızı açın

2.  Süresi doldu sertifikası olan HTTP ayarlarını seçin, sertifika ekle ' yi seçin ve yeni sertifika dosyasını açın

3.  Sertifikanın yanındaki Sil simgesini kullanarak eski sertifikayı kaldırın ve Kaydet ' e tıklayın.

#### <a name="certificate-verification-failed"></a>Sertifika doğrulaması başarısız oldu

**İleti:** Arka uç sertifikasının geçerliliği doğrulanamadı. Nedenini öğrenmek için, {errorCode} hata koduyla ilişkili ileti için SSL tanılamayı aç ' ı işaretleyin

**Sağlamak** Bu hata, Application Gateway sertifikanın geçerliliğini doğrulayamadığı zaman oluşur.

**Çözüm:** Bu sorunu çözmek için, sunucunuzun düzgün şekilde oluşturulmuş olması durumunda sertifikayı kontrol edin. Örneğin, sertifikayı ve özelliklerini doğrulamak için [OpenSSL](https://www.openssl.org/docs/man1.0.2/man1/verify.html) 'yi kullanabilir ve SERTIFIKAYı Application Gateway http ayarlarına yeniden yüklemeyi deneyebilirsiniz.

<a name="backend-health-status-unknown"></a>Arka uç sistem durumu bilinmiyor
-------------------------------
Arka uç sistem durumu bilinmiyor olarak gösteriliyorsa, portalda aşağıdaki ekran görüntüsü gibi görüntülenir:

![Application Gateway arka uç durumu-bilinmiyor](./media/application-gateway-backend-health-troubleshooting/appgwunknown.png)

Arka uç durumunun bilinmiyor olarak gösterilmesi durumunda aşağıdaki nedenlerden biri veya birkaçı olabilir:

1.  Application Gateway alt ağındaki NSG, "Internet" üzerinden 65503-65534 (v1 SKU) veya 65200-65535 (v2 SKU) bağlantı noktalarına gelen erişimi engelliyor
2.  Varsayılan yol (0.0.0.0/0) olan Application Gateway alt ağında, Internet olarak değil sonraki atlama ile UDR
3.  BGP üzerinden VNet 'e ExpressRoute/VPN bağlantısı tarafından tanıtılan varsayılan yol
4.  Özel DNS sunucusu, ortak etki alanı adlarını çözümleyemeyecek VNet 'te yapılandırıldı
5.  Application Gateway sağlıksız bir durumda

**Çözüm:**

1.  NSG 'nizin 65503-65534 (v1 SKU) veya 65200-65535 (v2 SKU) bağlantı noktalarına "Internet" üzerinden erişimi engelleyip engellemediğini denetleyin

    a.  Application Gateway "genel bakış" sekmesinde, sanal ağ/alt ağ bağlantısını seçin

    b.  Sanal ağınızın alt ağlar sekmesinde Application Gateway dağıtıldığı alt ağı seçin

    c.  Yapılandırılmış bir NSG olup olmadığını denetleyin

    d.  Varsa, arama sekmesinde veya tüm kaynaklar altında bu NSG kaynağını arayın

    e.  Gelen kuralları bölümünde, v1 SKU 'SU veya 65200-65535 v2 SKU 'SU için 65503-65534 hedef bağlantı noktası aralığına, herhangi bir veya Internet olarak kaynak içeren bir gelen kuralı ekleyin

    f.  Kaydet ' e tıklayın ve arka uç durumunun başarıyla görüntüleyip görüntüleyemediğinizi doğrulayın

    g.  Alternatif olarak, bunu [PowerShell/CLI](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group) aracılığıyla yapabilirsiniz

2.  UDR 'nizin bir sonraki atlama ile Internet olmayan bir varsayılan yolu (0.0.0.0/0) olup olmadığını denetleyin
    
    a.  Alt ağlarınızı öğrenmek için 1. a ve 1. b adımlarını izleyin

    b.  Yapılandırılmış herhangi bir UDR olup olmadığını denetleyin. Bir varsa, arama çubuğunda veya tüm kaynaklar altında kaynağı arayın

    c.  Sonraki atlama ile Internet olarak değil varsayılan yolların (0.0.0.0/0) olup olmadığını denetleyin. Sanal Gereç veya sanal ağ geçidi ise, paketi değiştirmeden sanal gerecinizin veya şirket içi cihazın paketi Internet hedefine doğru bir şekilde yönlendirebilmesini sağlayın

    d.  Aksi halde, bir sonraki atlamayı Internet olarak değiştirin, Kaydet ' e tıklayın ve arka uç durumunu doğrulayın

3.  BGP üzerinden VNet 'e ExpressRoute/VPN bağlantısı tarafından tanıtılan varsayılan yol

    a.  BGP üzerinden sanal ağa yönelik bir ExpressRoute/VPN bağlantınız varsa ve varsayılan yolu tanıtıyorsa, paketin değişiklik yapılmadan internet hedefine geri yönlendirildiğinden emin olmanız gerekir.

    b.  Application Gateway portalında "bağlantı sorunlarını giderme" seçeneğini kullanarak doğrulayabilirsiniz

    c.  "1.1.1.1" gibi internet yönlendirilebilir IP adresi ve hedef bağlantı noktası gibi her şeyi el ile seçin ve bağlantıyı denetleyin.

    d.  Sonraki atlama sanal ağ geçidi ise, ExpressRoute veya VPN üzerinden tanıtılan bir varsayılan yol olabilir

4.  VNet 'te yapılandırılmış özel bir DNS sunucusu varsa, sunucu (ler) in ortak etki alanlarını çözümleyebildiğini doğrulayın. Application Gateway, OCSP sunucuları gibi dış etki alanlarına ulaşabilmesi ya da sertifikanın iptal durumunu denetlemesi, vb. senaryolarda genel etki alanı adı çözümlemesi gerekebilir.

5.  Application Gateway sağlıklı ve çalışır durumda olup olmadığını doğrulamak için portalda Kaynak Durumu seçeneğine gidin ve "sağlıklı" olup olmadığını doğrulayın. Sağlıksız veya düşürülmüş durumu görüyorsanız [desteğe başvurun](https://azure.microsoft.com/support/options/).

<a name="next-steps"></a>Sonraki Adımlar
----------

Tanılama ve günlüğe kaydetme Application Gateway hakkında daha fazla bilgi edinmek için [buraya](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)bakın.
