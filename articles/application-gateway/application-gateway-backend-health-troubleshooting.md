---
title: Azure Uygulama Ağ Geçidi'ndeki arka uç sistem sorunlarını giderme
description: Azure Uygulama Ağ Geçidi için arka uç sistem sorunlarının nasıl giderilenle öğrenilebildiğini açıklar
services: application-gateway
author: surajmb
ms.service: application-gateway
ms.topic: article
ms.date: 08/30/2019
ms.author: surmb
ms.openlocfilehash: 71e1f8be2af5556d86996175e8a1ddbccc9c7de1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72001673"
---
<a name="troubleshoot-backend-health-issues-in-application-gateway"></a>Uygulama Ağ Geçidi'ndeki arka uç sistem sorunlarını giderme
==================================================

<a name="overview"></a>Genel Bakış
--------

Varsayılan olarak, Azure Application Gateway arka uç sunucularını sistem durumu durumlarını denetlemek ve isteklere hizmet vermeye hazır olup olmadıklarını denetlemek için sondalar. Kullanıcılar ayrıca ana bilgisayar adını, incelenecek yolu ve Sağlıklı olarak kabul edilecek durum kodlarından bahsetmek için özel sondalar oluşturabilir. Her durumda, arka uç sunucusu başarılı bir şekilde yanıt vermezse, Uygulama Ağ Geçidi sunucuyu Sağlıksız olarak işaretler ve istekleri sunucuya iletmeyi durdurur. Sunucu başarılı bir şekilde yanıt vermeye başladıktan sonra, Uygulama Ağ Geçidi istekleri iletmeye devam eder.

### <a name="how-to-check-backend-health"></a>Arka uç durumunu niçin kontrol edin

Arka uç havuzunuzun durumunu kontrol etmek için Azure portalındaki **Arka Uç Durumu** sayfasını kullanabilirsiniz. Veya [Azure PowerShell,](https://docs.microsoft.com/powershell/module/az.network/get-azapplicationgatewaybackendhealth?view=azps-2.6.0) [CLI](https://docs.microsoft.com/cli/azure/network/application-gateway?view=azure-cli-latest#az-network-application-gateway-show-backend-health)veya [REST API'yi](https://docs.microsoft.com/rest/api/application-gateway/applicationgateways/backendhealth)kullanabilirsiniz.

Bu yöntemlerden herhangi biri tarafından alınan durum aşağıdakilerden biri olabilir:

- Sağlam

- Uygun Değil

- Bilinmiyor

Bir sunucunun arka uç durumu Sağlıklı ise, Bu, Uygulama Ağ Geçidi'nin istekleri bu sunucuya ileteceği anlamına gelir. Ancak arka uç havuzundaki tüm sunucuların arka uç durumu sağlıksız veya bilinmiyorsa, uygulamalara erişmeye çalıştığınızda sorunlarla karşılaşabilirsiniz. Bu makalede, gösterilen hataların her biri için belirtiler, neden ve çözüm açıklanır.

<a name="backend-health-status-unhealthy"></a>Arka uç sağlık durumu: Sağlıksız
-------------------------------

Arka uç durumu sağlıksızsa, portal görünümü aşağıdaki ekran görüntüsüne benzer:

![Uygulama Ağ Geçidi arka uç sağlık - Sağlıksız](./media/application-gateway-backend-health-troubleshooting/appgwunhealthy.png)

Veya Bir Azure PowerShell, CLI veya Azure REST API sorgusu kullanıyorsanız, aşağıdakilere benzeyen bir yanıt alırsınız:
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
Arka uç havuzundaki tüm sunucular için sağlıksız bir arka uç sunucu durumu aldıktan sonra, istekler sunuculara iletilir ve Application Gateway isteyen istemciye "502 Kötü Ağ Geçidi" hatası döndürür. Bu sorunu gidermek için, Arka Uç Durumu sekmesindeki **Ayrıntılar** **sütununa** bakın.

**Ayrıntılar** sütununda görüntülenen ileti, sorun hakkında daha ayrıntılı bilgiler sağlar ve bunlara dayanarak sorunu gidermeye başlayabilirsiniz.

> [!NOTE]
> Varsayılan sonda isteği \<protokol\>biçiminde gönderilir://127.0.0.1:\>\<bağlantı noktası /. Örneğin, http://127.0.0.1:80 bağlantı noktası 80'deki bir http sondası için. Sadece 200 ile 399 arasında olan HTTP durum kodları sağlıklı kabul edilir. Protokol ve hedef bağlantı noktası HTTP ayarlarından devralınır. Uygulama Ağ Geçidi'nin farklı bir protokol, ana bilgisayar adı veya yolda araştırma sını ve farklı bir durum kodunu Sağlıklı olarak tanımasını istiyorsanız, özel bir sonda yıkın ve HTTP ayarlarıyla ilişkilendirin.

<a name="error-messages"></a>Hata iletileri
------------------------
#### <a name="backend-server-timeout"></a>Arka uç sunucu zaman sonu

**İleti:** Uygulama ağ geçidinin\'sistem durumu sondasına yanıt vermek için arka uç tarafından alınan süre, sonda ayarındaki zaman ayarındaki zaman ayarından daha fazladır.

**Sebep:** Application Gateway arka uç sunucusuna bir HTTP(S) sonda isteği gönderdikten sonra, yapılandırılmış bir süre için arka uç sunucusundan yanıt bekler. Arka uç sunucusu yapılandırılan süre içinde yanıt vermiyorsa (zaman sonu değeri), yapılandırılan zaman sonu süresi içinde yeniden yanıt vermeye başlayana kadar Sağlıksız olarak işaretlenir.

**Çözünürlük:** Arka uç sunucusunun veya uygulamanın yapılandırılan zaman sonu süresi içinde neden yanıt vermediğini denetleyin ve uygulama bağımlılıklarını da denetleyin. Örneğin, veritabanının yanıt gecikmesini tetikleyebilecek sorunları olup olmadığını denetleyin. Uygulamanın davranışının farkındaysanız ve yalnızca zaman sonu değerinden sonra yanıt vermesi gerekiyorsa, özel sonda ayarlarından zaman sonu değerini artırın. Zaman açıklama değerini değiştirmek için özgür bir sondanıniz olmalı. Özel bir sondanın nasıl yapılandırılabildiğini öğrenmek için [belgeler sayfasına bakın.](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal)

Zaman aradeğerini artırmak için aşağıdaki adımları izleyin:

1.  Arka uç sunucusuna doğrudan erişin ve sunucunun bu sayfada yanıt vermesi için gereken süreyi kontrol edin. Geliştirici araçlarını kullanan bir tarayıcı da dahil olmak üzere arka uç sunucusuna erişmek için herhangi bir aracı kullanabilirsiniz.

1.  Uygulamanın yanıt vermesi için gereken süreyi bulduktan **sonra, Sistem Durumu Sondaları** sekmesini seçin ve ardından HTTP ayarlarınızla ilişkili sondayı seçin.

1.  Uygulama yanıt süresinden daha yüksek herhangi bir zaman dışı değeri saniye cinsinden girin.

1.  Özel sonda ayarlarını kaydedin ve arka uç durumunun şimdi Sağlıklı olarak gösterip göstermediğini kontrol edin.

#### <a name="dns-resolution-error"></a>DNS çözümhatası

**İleti:** Uygulama Ağ Geçidi bu arka uç için bir sonda oluşturamadı. Bu durum genellikle arka ucun FQDN değerinin doğru girilmemesinden kaynaklanır. 

**Sebep:** Arka uç havuzu IP Adresi/FQDN veya Uygulama Hizmeti türündeyse, Uygulama Ağ Geçidi Alan Adı Sistemi (DNS) (özel veya Azure varsayılanı) üzerinden girilen FQDN'nin IP adresine gider ve HTTP Ayarları'nda belirtilen TCP bağlantı noktasındasunucuya bağlanmaya çalışır. Ancak bu ileti görüntülenirse, Uygulama Ağ Geçidi'nin girilen FQDN IP adresini başarıyla çözemeyeceğini gösterir.

**Çözünürlük:**

1.  Arka uç havuzuna girilen FQDN'nin doğru olduğunu ve bunun bir kamu malı olduğunu doğrulayın ve ardından yerel makinenizden çözmeye çalışın.

1.  IP adresini çözebilirseniz, sanal ağdaki DNS yapılandırmasında bir sorun olabilir.

1.  Sanal ağın özel bir DNS sunucusuyla yapılandırılıp yapılandırılmadığını denetleyin. Eğer varsa, belirtilen FQDN IP adresine neden çözülemez hakkında DNS sunucusunu kontrol edin.

1.  Azure varsayılan DNS kullanıyorsanız, uygun A kaydının veya CNAME kaydı eşlemenin tamamlanıp tamamlanmadığını alan adı kayıt şirketinize danışın.

1.  Etki alanı özel veya dahiliyse, etki alanı aynı sanal ağdaki bir VM'den çözmeye çalışın. Bunu çözebilirseniz, Uygulama Ağ Geçidi'ni yeniden başlatın ve yeniden denetleyin. Uygulama Ağ Geçidi'ni yeniden başlatmak için, bu bağlantılı kaynaklarda açıklanan PowerShell komutlarını kullanarak [durdurmanız](https://docs.microsoft.com/powershell/module/azurerm.network/stop-azurermapplicationgateway?view=azurermps-6.13.0) ve [başlatmanız](https://docs.microsoft.com/powershell/module/azurerm.network/start-azurermapplicationgateway?view=azurermps-6.13.0) gerekir.

#### <a name="tcp-connect-error"></a>TCP bağlantı hatası

**İleti:** Uygulama Ağ Geçidi arka uca bağlanamadı.
Lütfen sonda için kullanılan bağlantı noktasında arka uç yanıt olup olmadığını kontrol edin.
Ayrıca herhangi bir NSG/UDR/Firewall ip ve bu arka uç bağlantı noktasına erişimi engelleyen olup olmadığını kontrol edin

**Sebep:** DNS çözümleme aşamasından sonra, Uygulama Ağ Geçidi HTTP ayarlarında yapılandırılan TCP bağlantı noktasındaki arka uç sunucusuna bağlanmaya çalışır. Application Gateway belirtilen bağlantı noktasında bir TCP oturumu kuramazsa, sonda bu iletiyle sağlıksız olarak işaretlenir.

**Çözüm:** Bu hatayı alırsanız, aşağıdaki adımları izleyin:

1.  Bir tarayıcı veya PowerShell kullanarak HTTP ayarlarında belirtilen bağlantı noktasındaki arka uç sunucusuna bağlanıp bağlanamayacağınızı kontrol edin. Örneğin, aşağıdaki komutu çalıştırın:`Test-NetConnection -ComputerName
    www.bing.com -Port 443`

1.  Belirtilen bağlantı noktası istenilen bağlantı noktası değilse, arka uç sunucusuna bağlanmak için Uygulama Ağ Geçidi için doğru bağlantı noktası numarasını girin

1.  Bağlantı noktasına yerel makinenizden de bağlanamıyorsanız, aşağıdakileri yapabilirsiniz:

    a.  Arka uç sunucusunun ağ bağdaştırıcısının ve alt ağının ağ güvenlik grubu (NSG) ayarlarını ve yapılandırılan bağlantı noktasına gelen bağlantılara izin verilip verilmediğini denetleyin. Değilse, bağlantılara izin vermek için yeni bir kural oluşturun. NSG kurallarının nasıl oluşturulacak larını öğrenmek için [belgeler sayfasına bakın.](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#create-security-rules)

    b.  Uygulama Ağ Geçidi alt netinin NSG ayarlarının giden genel ve özel trafiğe izin verip verilip vermediğini kontrol edin, böylece bağlantı yapılabilir. NSG kurallarının nasıl oluşturulması hakkında daha fazla bilgi edinmek için adım 3a'da sağlanan belge sayfasını kontrol edin.
    ```azurepowershell
            $vnet = Get-AzVirtualNetwork -Name "vnetName" -ResourceGroupName "rgName"
            Get-AzVirtualNetworkSubnetConfig -Name appGwSubnet -VirtualNetwork $vnet
    ```

    c.  Yönlendirme anormallikleri için Uygulama Ağ Geçidi'nin ve arka uç sunucusunun alt netinin kullanıcı tanımlı rota (UDR) ayarlarını denetleyin. UDR'nin trafiği arka uç alt ağdan uzağa yönlendirmediğinden emin olun. Örneğin, Azure ExpressRoute ve/veya VPN üzerinden Uygulama Ağ Geçidi alt ağına duyurulan sanal cihazlara giden yolları veya varsayılan yolları denetleyin.

    d.  Bir ağ bağdaştırıcısının etkili rotalarını ve kurallarını denetlemek için aşağıdaki PowerShell komutlarını kullanabilirsiniz:
    ```azurepowershell
            Get-AzEffectiveNetworkSecurityGroup -NetworkInterfaceName "nic1" -ResourceGroupName "testrg"
            Get-AzEffectiveRouteTable -NetworkInterfaceName "nic1" -ResourceGroupName "testrg"
    ```
1.  NSG veya UDR ile ilgili herhangi bir sorun bulamazsanız, yapılandırılan bağlantı noktalarında istemcilerin TCP oturumu oluşturmasını engelleyen uygulamayla ilgili sorunlar için arka uç sunucunuzu denetleyin. Kontrol etmek için birkaç şey:

    a.  Komut istemini açın (Win+R\> - `netstat`cmd), enter ve Enter'u seçin.

    b.  Sunucunun yapılandırılan bağlantı noktasında dinleyip dinlemediğini denetleyin. Örnek:
    ```
            Proto Local Address Foreign Address State PID
            TCP 0.0.0.0:80 0.0.0.0:0 LISTENING 4
    ```
    c.  Yapılandırılan bağlantı noktasında niçin dinlemiyorsa, web sunucusu ayarlarınızı kontrol edin. Örneğin: IIS'deki site ciltlemeleri, NGINX'teki sunucu bloğu ve Apache'deki sanal ana bilgisayar.

    d.  Bağlantı noktasına gelen trafiğe izin verilebilmek için işletim sistemi güvenlik duvarı ayarlarınızı kontrol edin.

#### <a name="http-status-code-mismatch"></a>HTTP durum kodu uyuşmazlığı

**İleti:** Arka uç\'s HTTP yanıtının durum kodu sonda ayarı yla eşleşmedi. Beklenen:{HTTPStatusCode0} Alındı:{HTTPStatusCode1}.

**Sebep:** TCP bağlantısı kurulduktan ve Bir SSL el sıkışması yapıldıktan sonra (SSL etkinse), Application Gateway sondayı arka uç sunucusuna HTTP GET isteği olarak gönderir. Daha önce açıklandığı gibi, varsayılan \<\>sonda protokole http17.0.0.1:\<bağlantı noktası\>/, olacak ve öfke 200 ile 399 arasında yanıt durum kodlarını Sağlıklı olarak dikkate alır. Sunucu başka bir durum kodu döndürürse, bu iletiyle sağlıksız olarak işaretlenir.

**Çözüm:** Arka uç sunucusunun yanıt koduna bağlı olarak aşağıdaki adımları atabilirsiniz. Ortak durum kodlarından bazıları burada listelenmiştir:

| **Hata** | **Eylemler** |
| --- | --- |
| Sonda durum kodu uyuşmazlığı: Alınan 401 | Arka uç sunucusunun kimlik doğrulaması gerektirenden denetlemeyin. Uygulama Ağ Geçidi sondaları bu noktada kimlik doğrulama için kimlik bilgilerini geçemez. Http \"401'e\" sonda durum kodu eşleşmesinde izin verin veya sunucunun kimlik doğrulaması gerektirmediği bir yola sonda verin. | |
| Sonda durum kodu uyuşmazlığı: Alınan 403 | Erişim yasak. Arka uç sunucusunda yola erişime izin verilip verilmediğini denetleyin. | |
| Sonda durum kodu uyuşmazlığı: Alınan 404 | Sayfa bulunamadı. Ana bilgisayar ad yolunun arka uç sunucusunda erişilebilir olup olmadığını denetleyin. Ana bilgisayar adı veya yol parametresini erişilebilir bir değerle değiştirin. | |
| Sonda durum kodu uyuşmazlığı: Alınan 405 | Uygulama Ağ Geçidi için sonda istekleri HTTP GET yöntemini kullanır. Sunucunuzun bu yönteme izin verip vermediğini kontrol edin. | |
| Sonda durum kodu uyuşmazlığı: Alınan 500 | İç sunucu hatası. Arka uç sunucusunun sistem durumunu ve hizmetlerin çalışıp çalışmadığını denetleyin. | |
| Sonda durum kodu uyuşmazlığı: Alınan 503 | Hizmet kullanılamıyor. Arka uç sunucusunun sistem durumunu ve hizmetlerin çalışıp çalışmadığını denetleyin. | |

Veya yanıtın yasal olduğunu düşünüyorsanız ve Application Gateway'in diğer durum kodlarını Sağlıklı olarak kabul etmesini istiyorsanız, özel bir sonda oluşturabilirsiniz. Bu yaklaşım, arka uç web sitesinin kimlik doğrulaması gerektiren durumlarda yararlıdır. Sonda istekleri herhangi bir kullanıcı kimlik bilgisi taşımadığından, başarısız olurlar ve bir HTTP 401 durum kodu arka uç sunucusu tarafından döndürülür.

Özel bir sonda oluşturmak için [aşağıdaki adımları](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal)izleyin.

#### <a name="http-response-body-mismatch"></a>HTTP yanıt vücut uyuşmazlığı

**İleti:** Arka uç\'s HTTP yanıt Gövdesi sonda ayarı eşleşmedi. Alınan yanıt gövdesi {string} içermez.

**Sebep:** Özel bir sonda oluşturduğunuzda, yanıt gövdesinden bir dize eşleştirerek bir arka uç sunucusunu Sağlıklı olarak işaretleme seçeneğiniz vardır. Örneğin, Uygulama Ağ Geçidi'ni "yetkisiz" bir dize olarak eşleşecek şekilde kabul etmek üzere yapılandırabilirsiniz. Sonda isteği için arka uç sunucu yanıtı **yetkisiz**dize içeriyorsa, Sağlıklı olarak işaretlenir. Aksi takdirde, bu ileti ile Sağlıksız olarak işaretlenir.

**Çözüm:** Bu sorunu gidermek için aşağıdaki adımları izleyin:

1.  Arka uç sunucusuna yerel olarak veya sonda yolundaki bir istemci makinesinden erişin ve yanıt gövdesini denetleyin.

1.  Uygulama Ağ Geçidi özel sonda yapılandırmasındaki yanıt gövdesinin yapılandırılanlarla eşleştiğini doğrulayın.

1.  Eşleşmezlerse, sonda yapılandırmasını değiştirin, böylece kabul etmek için doğru dize değeri vardır.

[Uygulama Ağ Geçidi sondası eşleştirme](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#probe-matching)hakkında daha fazla bilgi edinin.

#### <a name="backend-server-certificate-invalid-ca"></a>Arka uç sunucu sertifikası geçersiz CA

**İleti:** Arka uç tarafından kullanılan sunucu sertifikası tanınmış bir Sertifika Yetkilisi (CA) tarafından imzalanmaz. Arka uç tarafından kullanılan sunucu sertifikasının kök sertifikasını yükleyerek Uygulama Ağ Geçidi'ndeki arka ucu beyaz listeye ait.

**Sebep:** Application Gateway v2 ile uçlardan uca SSL, sunucuyu Sağlıklı görebilmek için arka uç sunucusunun sertifikasının doğrulanmasını gerektirir.
Bir SSL sertifikasının güvenilir olması için, arka uç sunucusunun sertifikasının, Uygulama Ağ Geçidi'nin güvenilir deposuna dahil edilmiş bir CA tarafından verilmesi gerekir. Sertifika güvenilir bir CA tarafından verilmediyse (örneğin, kendi imzasını taşıyan bir sertifika kullanıldıysa), kullanıcılar verenin sertifikasını Uygulama Ağ Geçidi'ne yüklemelidir.

**Çözüm:** Güvenilen kök sertifikayı dışa aktarmak ve Uygulama Ağ Geçidi'ne yüklemek için aşağıdaki adımları izleyin. (Bu adımlar Windows istemcileri içindir.)

1.  Uygulamanızın barındırıldığı makinede oturum açın.

1.  Kazan+R'yi seçin veya **Başlat** düğmesine sağ tıklayın ve ardından **Çalıştır'ı**seçin.

1.  Girin `certmgr.msc` ve Enter'u seçin. **Başlat** menüsünde Sertifika Yöneticisi'ni de arayabilirsiniz.

1.  Sertifikayı genellikle `\Certificates - Current User\\Personal\\Certificates\`içinde bulun ve açın.

1.  Kök sertifikayı seçin ve ardından **Sertifikayı Görüntüle'yi**seçin.

1.  Sertifika özelliklerinde **Ayrıntılar** sekmesini seçin.

1.  **Ayrıntılar** sekmesinde, **Dosyaya Kopyala** seçeneğini seçin ve dosyayı Base-64 kodlu X.509'a kaydedin (. CER) biçimi.

1.  Azure portalında Uygulama Ağ Geçidi HTTP **Ayarları** sayfasını açın.

1. HTTP ayarlarını açın, **Sertifika Ekle'yi**seçin ve kaydettiğiniz sertifika dosyasını bulun.

1. HTTP ayarlarını kaydetmek için **Kaydet'i** seçin.

Alternatif olarak, sunucuya doğrudan erişerek (Uygulama Ağ Geçidi'ni atlayarak) istemci makinesinden kök sertifikasını tarayıcıdan dışa aktarabilirsiniz.

Uygulama Ağ Geçidi'nde Güvenilen Kök Sertifikalarının nasıl ayıklanıp yükleyilebileceği hakkında daha fazla bilgi için, [güvenilen kök sertifikasını dışa aktar (v2 SKU için)](https://docs.microsoft.com/azure/application-gateway/certificates-for-backend-authentication#export-trusted-root-certificate-for-v2-sku)bakın.

#### <a name="trusted-root-certificate-mismatch"></a>Güvenilen kök sertifikası uyuşmazlığı

**İleti:** Arka uç tarafından kullanılan sunucu sertifikasının kök sertifikası, uygulama ağ geçidine eklenen güvenilen kök sertifikasıyla eşleşmiyor. Arka uçbeyaz listeye doğru kök sertifikası eklediğinizden emin olun

**Sebep:** Application Gateway v2 ile uçlardan uca SSL, sunucuyu Sağlıklı görebilmek için arka uç sunucusunun sertifikasının doğrulanmasını gerektirir.
Bir SSL sertifikasının güvenilir olması için arka uç sunucu sertifikasının, Uygulama Ağ Geçidi'nin güvenilir deposuna dahil edilmiş bir CA tarafından verilmesi gerekir. Sertifika güvenilir bir CA tarafından verilmeyense (örneğin, kendi imzasını taşıyan bir sertifika kullanıldı), kullanıcılar verenin sertifikasını Uygulama Ağ Geçidi'ne yüklemelidir.

Uygulama Ağ Geçidi HTTP ayarlarına yüklenen sertifika, arka uç sunucu sertifikasının kök sertifikasıyla eşleşmelidir.

**Çözüm:** Bu hata iletisini alırsanız, Uygulama Ağ Geçidi'ne yüklenen sertifika ile arka uç sunucusuna yüklenen sertifika arasında bir uyumsuzluk vardır.

Doğru güvenilir kök sertifikayı Uygulama Ağ Geçidi'ne yüklemek için önceki yöntemdeki 1-11 adımlarını izleyin.

Uygulama Ağ Geçidi'nde Güvenilen Kök Sertifikalarının nasıl ayıklanıp yükleyilebileceği hakkında daha fazla bilgi için, [güvenilen kök sertifikasını dışa aktar (v2 SKU için)](https://docs.microsoft.com/azure/application-gateway/certificates-for-backend-authentication#export-trusted-root-certificate-for-v2-sku)bakın.
> [!NOTE]
> Bu hata, TLS el sıkışması sırasında Root > Intermediate (varsa) > Leaf de dahil olmak üzere arka uç sunucusu nun certzincirinin tam zincirini değiştirmemesi durumunda da oluşabilir. Doğrulamak için, herhangi bir istemciden OpenSSL komutlarını kullanabilir ve Uygulama Ağ Geçidi sondasında yapılandırılan ayarları kullanarak arka uç sunucusuna bağlanabilirsiniz.

Örnek:
```
OpenSSL> s_client -connect 10.0.0.4:443 -servername www.example.com -showcerts
```
Çıktı, döndürülen sertifikanın tam zincirini göstermiyorsa, sertifikayı kök sertifika da dahil olmak üzere tam zincirle yeniden dışa aktarın. Bu sertifikayı arka uç sunucunuzda yapılandırın. 

```
  CONNECTED(00000188)\
  depth=0 OU = Domain Control Validated, CN = \*.example.com\
  verify error:num=20:unable to get local issuer certificate\
  verify return:1\
  depth=0 OU = Domain Control Validated, CN = \*.example.com\
  verify error:num=21:unable to verify the first certificate\
  verify return:1\
  \-\-\-\
  Certificate chain\
   0 s:/OU=Domain Control Validated/CN=*.example.com\
     i:/C=US/ST=Arizona/L=Scottsdale/O=GoDaddy.com, Inc./OU=http://certs.godaddy.com/repository//CN=Go Daddy Secure Certificate Authority - G2\
  \-----BEGIN CERTIFICATE-----\
  xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx\
  \-----END CERTIFICATE-----
```

#### <a name="backend-certificate-invalid-common-name-cn"></a>Arka uç sertifikası geçersiz ortak ad (CN)

**İleti:** Arka uç sertifikasının Ortak Adı (CN), sondanın ana bilgisayarıüster ile eşleşmez.

**Sebep:** Uygulama Ağ Geçidi, arka uç HTTP ayarlarında belirtilen ana bilgisayar adının arka uç sunucusunun SSL sertifikası tarafından sunulan CN ile eşleşip eşleşmediğini denetler. Bu Standard_v2 ve WAF_v2 SKU davranışıdır. Standart ve WAF SKU'nun Sunucu Adı Göstergesi (SNI), arka uç havuzu adresindeki FQDN olarak ayarlanır.

v2 SKU'da, varsayılan bir sonda varsa (özel bir sonda yapılandırılmamış ve ilişkilendirilmemişse), SNI HTTP ayarlarında belirtilen ana bilgisayar adından ayarlanır. Veya, arka uç adresi havuzunun geçerli bir FQDN içerdiği HTTP ayarlarında "Arka uç adresinden ana bilgisayar adı seç" belirtilmişse, bu ayar uygulanır.

HTTP ayarlarıyla ilişkili özel bir sonda varsa, SNI özel sonda yapılandırmasında belirtilen ana bilgisayar adından ayarlanır. Veya, özel sondada **arka uç HTTP ayarlarından hostname seç** seçilirse, SNI HTTP ayarlarında belirtilen ana bilgisayar adından ayarlanır.

**Arka uç adresinden ana bilgisayar adını seç** HTTP ayarlarında ayarlanmışsa, arka uç adresi havuzunda geçerli bir FQDN olmalıdır.

Bu hata iletisini alırsanız, arka uç sertifikasının CN'si özel sondadaveya HTTP ayarlarında yapılandırılan ana bilgisayar adıyla eşleşmez **(arka uç HTTP ayarlarından ana bilgisayar adını** seçiliyse). Varsayılan bir sonda kullanıyorsanız, ana bilgisayar adı **127.0.0.1**olarak ayarlanır. Bu istenilen bir değer değilse, özel bir sonda oluşturmalı ve HTTP ayarları ile ilişkilendirmelisiniz.

**Çözüm:**

Sorunu çözmek için aşağıdaki adımları izleyin.

Windows için:

1.  Uygulamanızın barındırıldığı makinede oturum açın.

1.  Kazan+R'yi seçin veya **Başlat** düğmesine sağ tıklayın ve **Çalıştır'ı**seçin.

1.  **certmgr.msc** girin ve Enter'u seçin. **Başlat** menüsünde Sertifika Yöneticisi'ni de arayabilirsiniz.

1.  Sertifikayı bulun (genellikle `\Certificates - Current User\\Personal\\Certificates`içinde), ve sertifikayı açın.

1.  **Ayrıntılar** sekmesinde, sertifika **Öznesi'ni**işaretleyin.

1.  Ayrıntılardan sertifikanın CN'sini doğrulayın ve özel sondanın ana bilgisayar adı alanına veya HTTP ayarlarına aynı girin **(arka uç HTTP ayarlarından ana bilgisayar adını** seçiliyse). Web siteniz için istenen ana bilgisayar adı bu değilse, bu etki alanı için bir sertifika almanız veya özel sonda veya HTTP ayar yapılandırmasında doğru ana bilgisayar adını girmeniz gerekir.

OpenSSL kullanan Linux için:

1.  OpenSSL bu komutu çalıştırın:
    ```
    openssl x509 -in certificate.crt -text -noout
    ```

2.  Görüntülenen özelliklerden, sertifikanın CN'sini bulun ve http ayarlarının ana bilgisayar adı alanına aynı girin. Web siteniz için istenen ana bilgisayar adı bu değilse, bu etki alanı için bir sertifika almanız veya özel sonda veya HTTP ayar yapılandırmasında doğru ana bilgisayar adını girmeniz gerekir.

#### <a name="backend-certificate-is-invalid"></a>Arka uç sertifikası geçersiz

**İleti:** Arka uç sertifikası geçersizdir. \"Geçerli tarih, sertifikadaki\" Geçerli \"ve\" Geçerli lik aralığında değildir.

**Sebep:** Her sertifikanın geçerlilik aralığı vardır ve sunucunun SSL sertifikası geçerli olmadığı sürece HTTPS bağlantısı güvenli olmayacaktır. Geçerli veriler **geçerli** ve aralık için **geçerli** olmalıdır. Değilse, sertifika geçersiz kabul edilir ve bu, Uygulama Ağ Geçidi'nin arka uç sunucusunu Sağlıksız olarak işaretlediği bir güvenlik sorunu oluşturur.

**Çözüm:** SSL sertifikanızın süresi dolduysa, sertifikayı satıcınızla yenileyin ve sunucu ayarlarını yeni sertifikayla güncelleştirin. Kendi imzalı bir sertifikaysa, geçerli bir sertifika oluşturmanız ve kök sertifikayı Uygulama Ağ Geçidi HTTP ayarlarına yüklemeniz gerekir. Bunu yapmak için şu adımları uygulayın:

1.  Portalda Uygulama Ağ Geçidi HTTP ayarlarınızı açın.

1.  Süresi dolmuş sertifikaya sahip ayarı seçin, **Sertifika Ekle'yi**seçin ve yeni sertifika dosyasını açın.

1.  Sertifikanın yanındaki **Sil** simgesini kullanarak eski sertifikayı kaldırın ve sonra **Kaydet'i**seçin.

#### <a name="certificate-verification-failed"></a>Sertifika doğrulama başarısız oldu

**İleti:** Arka uç sertifikasının geçerliliği doğrulanamadı. Nedenini öğrenmek için hata kodu {errorCode} ile ilişkili ileti için SSL tanısını aç'ı işaretleyin

**Sebep:** Bu hata, Uygulama Ağ Geçidi sertifikanın geçerliliğini doğrulayamadığında oluşur.

**Çözüm:** Bu sorunu gidermek için, sunucunuzdaki sertifikanın düzgün oluşturulduğunu doğrulayın. Örneğin, sertifikayı ve özelliklerini doğrulamak için [OpenSSL'yi](https://www.openssl.org/docs/man1.0.2/man1/verify.html) kullanabilir ve ardından sertifikayı Uygulama Ağ Geçidi HTTP ayarlarına yeniden yüklemeyi deneyebilirsiniz.

<a name="backend-health-status-unknown"></a>Arka uç sağlık durumu: bilinmiyor
-------------------------------
Arka uç durumu Bilinmiyor olarak gösterilirse, portal görünümü aşağıdaki ekran görüntüsüne benzer:

![Uygulama Ağ Geçidi arka uç sağlık - Bilinmiyor](./media/application-gateway-backend-health-troubleshooting/appgwunknown.png)

Bu davranış, aşağıdaki nedenlerden biri veya birkaçı için oluşabilir:

1.  Application Gateway alt netindeki NSG, "Internet"ten 65503-65534 (v1 SKU) veya 65200-65535 (v2 SKU) bağlantı noktalarına gelen erişimi engelliyor.
1.  Uygulama Ağ Geçidi alt netindeki UDR varsayılan rotaya (0.0.0.0/0) ayarlanır ve bir sonraki atlama "Internet" olarak belirtilmez.
1.  Varsayılan rota, BGP üzerinden sanal ağa giden Bir Ekspres Rota/VPN bağlantısı yla duyurur.
1.  Özel DNS sunucusu, ortak alan adlarını çözemeyen bir sanal ağda yapılandırılır.
1.  Uygulama Ağ Geçidi sağlıksız bir durumda.

**Çözüm:**

1.  NSG'nizin **Internet'ten**65503-65534 (v1 SKU) veya 65200-65535 (v2 SKU) bağlantı noktalarına erişimi engelleyip engellemediğini kontrol edin:

    a.  Uygulama Ağ **Geçidine Genel Bakış** sekmesinde **Sanal Ağ/Altnet** bağlantısını seçin.

    b.  Sanal ağınızın **Alt Ağlar** sekmesinde, Uygulama Ağ Geçidi'nin dağıtıldığı alt ağı seçin.

    c.  Herhangi bir NSG yapılandırılıp yapılandırılmadığını kontrol edin.

    d.  Bir NSG yapılandırılırsa, **Arama** sekmesinde veya **Tüm kaynakların**altında bu NSG kaynağını arayın.

    e.  Gelen **Kurallar** bölümünde, **kaynak** **herhangi** bir veya **internet**olarak ayarlanmış v1 SKU veya 65200-65535 v2 SKU için hedef bağlantı noktası aralığı 65503-65534 izin vermek için gelen kural ekleyin.

    f.  **Kaydet'i** seçin ve arka uçunuzu Sağlıklı olarak görüntüleyebileceğinizi doğrulayın. Alternatif olarak, [PowerShell / CLI](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)ile yapabilirsiniz.

1.  UDR'nizin varsayılan rotası olup olmadığını (0.0.0.0/0) kontrol edin ve bir sonraki atlama **Internet**olarak ayarlanmaz:
    
    a.  Alt ağınızı belirlemek için 1a ve 1b adımlarını izleyin.

    b.  Yapılandırılmış udr olup olmadığını kontrol edin. Varsa, arama çubuğunda veya **Tüm kaynakların**altında kaynağı arayın.

    c.  Bir sonraki atlama **Internet**olarak ayarlanmazken varsayılan rotalar (0.0.0.0/0) olup olmadığını denetleyin. Ayar **Sanal Aygıt** veya **Sanal Ağ Ağ Ağ Geçidi**ise, sanal cihazınızın veya şirket içi aygıtın paketi değiştirmeden paketi internet hedefine doğru şekilde yönlendirebileceğinden emin olmalısınız.

    d.  Aksi takdirde, bir sonraki atlamayı **Internet**olarak değiştirin, **Kaydet'i**seçin ve arka uç durumunu doğrulayın.

1.  ExpressRoute/VPN bağlantısı tarafından BGP üzerinden sanal ağa reklamı yapılan varsayılan rota:

    a.  BGP üzerinden sanal ağa Ekspres Rota/VPN bağlantınız varsa ve varsayılan bir rotanın reklamını yapıyorsanız, paketin değiştirmeden internet hedefine yönlendirildiğından emin olmalısınız. Uygulama Ağ Geçidi portalındaki **Bağlantı Sorun Giderme** seçeneğini kullanarak doğrulayabilirsiniz.

    b.  1.1.1.1 gibi herhangi bir internet routable IP adresi olarak hedef el ile seçin. Hedef bağlantı noktasını herhangi bir şey olarak ayarlayın ve bağlantıyı doğrulayın.

    c.  Bir sonraki atlama sanal ağ ağ ağ geçidi ise, ExpressRoute veya VPN üzerinden reklamı yapılan varsayılan bir rota olabilir.

1.  Sanal ağda yapılandırılan özel bir DNS sunucusu varsa, sunucunun (veya sunucuların) ortak etki alanlarını çözebileceğini doğrulayın. Uygulama Ağ Geçidi'nin OCSP sunucuları gibi dış etki adlarına ulaşması veya sertifikanın iptal durumunu denetlemesi gereken senaryolarda ortak alan adı çözümlemesi gerekebilir.

1.  Uygulama Ağ Geçidi'nin sağlıklı ve çalışır durumda olduğunu doğrulamak için portaldaki **Kaynak Durumu** seçeneğine gidin ve durumun **Sağlıklı**olduğunu doğrulayın. **Sağlıksız** veya **Bozulmuş** bir durum görürseniz, [desteğe başvurun.](https://azure.microsoft.com/support/options/)

<a name="next-steps"></a>Sonraki adımlar
----------

[Uygulama Ağ Geçidi tanılama ve günlüğe kaydetme](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)hakkında daha fazla bilgi edinin.
