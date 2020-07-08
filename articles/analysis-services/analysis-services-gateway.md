---
title: Azure Analysis Services için şirket içi veri ağ geçidi | Microsoft Docs
description: Azure 'daki Analysis Services sunucunuz şirket içi veri kaynaklarına bağlanabiliyor ise şirket içi ağ geçidi gereklidir.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 648646b6f973762245c344cd2629a874a219b170
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76310161"
---
# <a name="connecting-to-on-premises-data-sources-with-on-premises-data-gateway"></a>Şirket içi veri ağ geçidi ile şirket içi veri kaynaklarına bağlanma

Şirket içi veri ağ geçidi, bulutta şirket içi veri kaynakları ve Azure Analysis Services sunucularınız arasında güvenli veri aktarımı sağlar. Aynı bölgedeki birden çok Azure Analysis Services sunucusu ile çalışmanın yanı sıra, ağ geçidinin en son sürümü de Azure Logic Apps, Power BI, güç uygulamaları ve güç otomatikleştir ile de çalışır. Yüklediğiniz ağ geçidi bu hizmetlerin tümünde aynı olduğundan, Azure Analysis Services ve Logic Apps bazı ek adımlara sahiptir.

Burada sunulan bilgiler, Azure Analysis Services şirket içi veri ağ geçidiyle nasıl çalıştığına özgüdür. Genel olarak ağ geçidi ve diğer hizmetlerle nasıl çalıştığı hakkında daha fazla bilgi edinmek için bkz. [Şirket içi veri ağ geçidi nedir?](/data-integration/gateway/service-gateway-onprem).

Azure Analysis Services için, ilk kez ağ geçidi ile kurulum almak dört bölümden oluşan bir işlemdir:

- **Kurulumu indir ve Çalıştır** -Bu adım, kuruluşunuzdaki bir bilgisayara ağ geçidi hizmeti yükler. Ayrıca [kiracınızın](/previous-versions/azure/azure-services/jj573650(v=azure.100)#what-is-an-azure-ad-tenant) Azure AD 'deki bir hesabı kullanarak Azure 'da oturum açın. Azure B2B (konuk) hesapları desteklenmez.

- **Ağ geçidinizin kaydetme** -Bu adımda, ağ geçidiniz için bir ad ve kurtarma anahtarı belirtip ağ geçidinizi ağ geçidi bulut hizmetine kaydederek bir bölge seçin. Ağ Geçidi kaynağınız herhangi bir bölgeye kaydedilebilir, ancak Analysis Services sunucularınız ile aynı bölgede olması önerilir. 

- **Azure 'da ağ geçidi kaynağı oluşturma** -Bu adımda, Azure 'da bir ağ geçidi kaynağı oluşturursunuz.

- **Sunucularınızı ağ geçidinize bağlama** -bir ağ geçidi kaynağına sahip olduktan sonra sunucuları buna bağlamaya başlayabilirsiniz. Aynı bölgede olduklarından, birden çok sunucuyu ve diğer kaynakları bağlayabilirsiniz.



## <a name="how-it-works"></a><a name="how-it-works"> </a>Nasıl çalışır?
Kuruluşunuzdaki bir bilgisayara yüklediğiniz ağ geçidi, bir Windows hizmeti, Şirket **içi veri ağ geçidi**olarak çalışır. Bu yerel hizmet, Azure Service Bus aracılığıyla Ağ Geçidi Bulut Hizmeti’ne kaydedilir. Daha sonra Azure aboneliğiniz için bir şirket içi veri ağ geçidi kaynağı oluşturursunuz. Azure Analysis Services sunucularınız Azure Gateway kaynağına bağlanır. Sunucunuzdaki modellerin sorgular veya işleme için şirket içi veri kaynaklarınıza bağlanması gerektiğinde, bir sorgu ve veri akışı, ağ geçidi kaynağı, Azure Service Bus, yerel şirket içi veri ağ geçidi hizmeti ve veri kaynaklarınızı ele alır. 

![Nasıl çalışır?](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Sorgular ve veri akışı:

1. Bulut hizmeti, şirket içi veri kaynağı için şifrelenmiş kimlik bilgileri ile bir sorgu oluşturur. Bu sorgu daha sonra işlenmek üzere ağ geçidi sırasına gönderilir.
2. Ağ Geçidi Bulut hizmeti sorguyu analiz eder ve isteği [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/)gönderir.
3. Şirket içi veri ağ geçidi, Azure Service Bus’ta bekleyen istekler olup olmadığını yoklar.
4. Ağ geçidi sorguyu alır, kimlik bilgilerinin şifresini çözer ve bu kimlik bilgileriyle veri kaynaklarına bağlanır.
5. Ağ geçidi, yürütme için sorguyu veri kaynağına gönderir.
6. Sonuçlar, veri kaynağından ağ geçidine ve ardından bulut hizmetine ve sunucunuza geri gönderilir.

## <a name="installing"></a>Yükleme

Azure Analysis Services ortamı için yükleme yaparken, [Azure Analysis Services için şirket içi veri ağ geçidini yükleme ve yapılandırma](analysis-services-gateway-install.md)bölümünde açıklanan adımları izlemeniz önemlidir. Bu makale Azure Analysis Services özeldir. Azure 'da şirket içi veri ağ geçidi kaynağı kurmak ve Azure Analysis Services sunucunuzu kaynağa bağlamak için gereken ek adımları içerir.

## <a name="ports-and-communication-settings"></a>Bağlantı noktaları ve iletişim ayarları

Ağ geçidi, Azure Service Bus'a yönelik bir giden bağlantı oluşturur. Şu giden bağlantı noktaları üzerinden iletişim kurar: TCP 443 (varsayılan), 5671, 5672, 9350 ila 9354.  Ağ geçidi için gelen bağlantı noktaları gerekli değildir.

Güvenlik duvarınızdaki veri bölgenizin IP adreslerini eklemeniz gerekebilir. [Microsoft Azure Veri Merkezi IP listesini](https://www.microsoft.com/download/details.aspx?id=56519) indirebilirsiniz. Bu liste haftalık olarak güncelleştirilir. Azure Veri Merkezi IP listesindeki adresler, CIDR gösteriminde listelenir. Daha fazla bilgi için bkz. [sınıfsız etki alanları arası yönlendirme](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).

Ağ Geçidi tarafından kullanılan tam etki alanı adları aşağıda verilmiştir.

| Etki alanı adları | Giden bağlantı noktaları | Açıklama |
| --- | --- | --- |
| *.powerbi.com |80 |Yükleyiciyi indirmek için kullanılan HTTP. |
| *.powerbi.com |443 |HTTPS |
| *.analysis.windows.net |443 |HTTPS |
| *. login.windows.net, login.live.com, aadcdn.msauth.net |443 |HTTPS |
| *.servicebus.windows.net |5671-5672 |Gelişmiş İleti Sıraya Alma Protokolü (AMQP) |
| *.servicebus.windows.net |443, 9350-9354 |TCP üzerinden Service Bus Geçişi'ndeki dinleyiciler (Erişim Denetimi belirtecinin alınması için 443 gerekir) |
| *.frontend.clouddatahub.net |443 |HTTPS |
| *.core.windows.net |443 |HTTPS |
| login.microsoftonline.com |443 |HTTPS |
| *.msftncsi.com |443 |Power BI hizmeti tarafından ağ geçidine erişilemiyorsa İnternet bağlantısını test etmek için kullanılır. |
| *.microsoftonline-p.com |443 |Yapılandırmaya bağlı olarak kimlik doğrulaması için kullanılır. |
| dc.services.visualstudio.com  |443 |Appınsights tarafından telemetri toplamak için kullanılır. |

### <a name="forcing-https-communication-with-azure-service-bus"></a><a name="force-https"></a>Azure Service Bus ile HTTPS iletişimini zorlama

Ağ geçidini, doğrudan TCP yerine HTTPS kullanarak Azure Service Bus ile iletişim kurmaya zorlayabilirsiniz; Ancak bunu yapmak performansı önemli ölçüde azaltabilir. Değerini olarak değiştirerek *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* dosyasını değiştirebilirsiniz `AutoDetect` `Https` . Bu dosya genellikle *C:\Program Files\On-premises Data Gateway*konumunda bulunur.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <a name="next-steps"></a>Sonraki adımlar 

Aşağıdaki makaleler, ağ geçidinin desteklediği tüm hizmetler için geçerli olan şirket içi veri ağ geçidi genel içeriğine eklenmiştir:

* [Şirket içi veri ağ geçidi hakkında SSS](https://docs.microsoft.com/data-integration/gateway/service-gateway-onprem-faq)   
* [Şirket içi veri ağ geçidi uygulamasını kullanma](https://docs.microsoft.com/data-integration/gateway/service-gateway-app)   
* [Kiracı düzeyinde yönetim](https://docs.microsoft.com/data-integration/gateway/service-gateway-tenant-level-admin)
* [Ara sunucu ayarlarını yapılandırma](https://docs.microsoft.com/data-integration/gateway/service-gateway-proxy)   
* [İletişim ayarlarını değiştirme](https://docs.microsoft.com/data-integration/gateway/service-gateway-communication)   
* [Günlük dosyalarını yapılandırma](https://docs.microsoft.com/data-integration/gateway/service-gateway-log-files)   
* [Sorun giderme](https://docs.microsoft.com/data-integration/gateway/service-gateway-tshoot)
* [Ağ geçidi performansını izleme ve en iyi duruma getirme](https://docs.microsoft.com/data-integration/gateway/service-gateway-performance)
