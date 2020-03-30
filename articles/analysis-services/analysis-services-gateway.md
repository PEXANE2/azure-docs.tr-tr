---
title: Azure Analiz Hizmetleri için şirket içi veri ağ geçidi | Microsoft Dokümanlar
description: Azure'daki Çözümleme Hizmetleri sunucunuz şirket içi veri kaynaklarına bağlanacaksa şirket içi ağ geçidi gereklidir.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 648646b6f973762245c344cd2629a874a219b170
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76310161"
---
# <a name="connecting-to-on-premises-data-sources-with-on-premises-data-gateway"></a>Şirket içi veri kaynaklarına şirket içi veri ağ geçidiyle bağlanma

Şirket içi veri ağ geçidi, şirket içi veri kaynakları ile buluttaki Azure Analiz Hizmetleri sunucularınız arasında güvenli veri aktarımı sağlar. Ağ geçidinin en son sürümü, aynı bölgedeki birden fazla Azure Analiz Hizmeti sunucusuyla çalışmanın yanı sıra Azure Logic Apps, Power BI, Power Apps ve Power Automate ile de çalışır. Yüklediğiniz ağ geçidi tüm bu hizmetlerde aynı olsa da, Azure Çözümleme Hizmetleri ve Mantıksal Uygulamalar'ın bazı ek adımları var.

Burada sağlanan bilgiler, Azure Analiz Hizmetleri'nin şirket içi Veri Ağ Geçidi ile nasıl çalıştığına özeldir. Genel olarak ağ geçidi ve diğer hizmetlerle nasıl çalıştığı hakkında daha fazla bilgi edinmek için şirket [içi veri ağ geçidi nedir?](/data-integration/gateway/service-gateway-onprem)

Azure Çözümleme Hizmetleri için, ağ geçidinde kurulum ilk kez dört bölümlü bir işlemdir:

- **İndirme ve çalıştırma kurulumu** - Bu adım, kuruluşunuzdaki bir bilgisayara bir ağ geçidi hizmeti yükler. [Ayrıca, kiracınızın](/previous-versions/azure/azure-services/jj573650(v=azure.100)#what-is-an-azure-ad-tenant) Azure AD'inde bir hesabı kullanarak Azure'da oturum açabilirsiniz. Azure B2B (konuk) hesapları desteklenmez.

- **Ağ geçidinizi kaydedin** - Bu adımda, ağ geçidiniz için bir ad ve kurtarma anahtarı belirtin ve ağ geçidinizi Ağ Geçidi Bulut Hizmeti'ne kaydederek bir bölge seçin. Ağ geçidi kaynağınız herhangi bir bölgede kaydedilebilir, ancak Çözümhizmetleri sunucularınızla aynı bölgede olması önerilir. 

- **Azure'da bir ağ geçidi kaynağı oluşturun** - Bu adımda Azure'da bir ağ geçidi kaynağı oluşturursunuz.

- **Sunucularınızı ağ geçidi kaynağınıza bağlayın** - Bir ağ geçidi kaynağınız olduğunda, sunucuları ona bağlamaya başlayabilirsiniz. Aynı bölgede olmaları koşuluyla birden çok sunucuyu ve diğer kaynakları bağlayabilirsiniz.



## <a name="how-it-works"></a><a name="how-it-works"> </a>Nasıl çalışır?
Kuruluşunuzdaki bir bilgisayara yüklediğiniz ağ geçidi, Windows hizmeti, **şirket içi veri ağ geçidi**olarak çalışır. Bu yerel hizmet, Azure Service Bus aracılığıyla Ağ Geçidi Bulut Hizmeti’ne kaydedilir. Ardından Azure aboneliğiniz için şirket içi veri ağ geçidi kaynağı oluşturursunuz. Azure Analiz Hizmetleri sunucularınız daha sonra Azure ağ geçidi kaynağınıza bağlanır. Sunucunuzdaki modellerin sorgu veya işleme için şirket içi veri kaynaklarınıza bağlanması gerektiğinde, sorgu ve veri akışı ağ geçidi kaynağından, Azure Hizmet Veri Servisi'nden, yerel şirket içi veri ağ geçidi hizmetinden ve veri kaynaklarınızdan geçer. 

![Nasıl çalışır?](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Sorgular ve veri akışı:

1. Bulut hizmeti, şirket içi veri kaynağı için şifrelenmiş kimlik bilgileri ile bir sorgu oluşturur. Bu sorgu daha sonra işlenmek üzere ağ geçidi sırasına gönderilir.
2. Ağ geçidi bulut hizmeti sorguyu analiz eder ve isteği [Azure Hizmet Veri Servisi'ne](https://azure.microsoft.com/documentation/services/service-bus/)iter.
3. Şirket içi veri ağ geçidi, Azure Service Bus’ta bekleyen istekler olup olmadığını yoklar.
4. Ağ geçidi sorguyu alır, kimlik bilgilerinin şifresini çözer ve bu kimlik bilgileriyle veri kaynaklarına bağlanır.
5. Ağ geçidi, yürütme için sorguyu veri kaynağına gönderir.
6. Sonuçlar, veri kaynağından ağ geçidine ve ardından bulut hizmetine ve sunucunuza geri gönderilir.

## <a name="installing"></a>Yükleme

Azure Çözümleme Hizmetleri ortamı için yükleme yaparken, Azure Analiz Hizmetleri için Yükle'de açıklanan adımları izlemeniz [ve Azure Analiz Hizmetleri için şirket içi veri ağ geçidini yapılandırmanız](analysis-services-gateway-install.md)önemlidir. Bu makale, Azure Çözümleme Hizmetleri'ne özgüdür. Azure'da bir şirket içi veri ağ geçidi kaynağı kurmak ve Azure Analiz Hizmetleri sunucunuzu kaynağa bağlamak için gereken ek adımları içerir.

## <a name="ports-and-communication-settings"></a>Bağlantı noktaları ve iletişim ayarları

Ağ geçidi, Azure Service Bus'a yönelik bir giden bağlantı oluşturur. Şu giden bağlantı noktaları üzerinden iletişim kurar: TCP 443 (varsayılan), 5671, 5672, 9350 ila 9354.  Ağ geçidi için gelen bağlantı noktaları gerekli değildir.

Güvenlik duvarınıza veri bölgenizin IP adreslerini eklemeniz gerekebilir. [Microsoft Azure Veri Merkezi IP listesini](https://www.microsoft.com/download/details.aspx?id=56519) indirebilirsiniz. Bu liste haftalık olarak güncelleştirilir. Azure Veri Merkezi IP listesindeki adresler, CIDR gösteriminde listelenir. Daha fazla bilgi için [Sınıfsız Etki Alanları Yönlendirme'ye](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)bakın.

Aşağıda ağ geçidi tarafından kullanılan tam nitelikli alan adları vettir.

| Etki alanı adları | Giden bağlantı noktaları | Açıklama |
| --- | --- | --- |
| *.powerbi.com |80 |Yükleyiciyi indirmek için kullanılan HTTP. |
| *.powerbi.com |443 |HTTPS |
| *.analysis.windows.net |443 |HTTPS |
| *.login.windows.net, login.live.com, aadcdn.msauth.net |443 |HTTPS |
| *.servicebus.windows.net |5671-5672 |Gelişmiş İleti Sıraya Alma Protokolü (AMQP) |
| *.servicebus.windows.net |443, 9350-9354 |TCP üzerinden Service Bus Geçişi'ndeki dinleyiciler (Erişim Denetimi belirtecinin alınması için 443 gerekir) |
| *.frontend.clouddatahub.net |443 |HTTPS |
| *.core.windows.net |443 |HTTPS |
| login.microsoftonline.com |443 |HTTPS |
| *.msftncsi.com |443 |Power BI hizmeti tarafından ağ geçidine erişilemiyorsa İnternet bağlantısını test etmek için kullanılır. |
| *.microsoftonline-p.com |443 |Yapılandırmaya bağlı olarak kimlik doğrulaması için kullanılır. |
| dc.services.visualstudio.com  |443 |AppInsights tarafından telemetri toplamak için kullanılır. |

### <a name="forcing-https-communication-with-azure-service-bus"></a><a name="force-https"></a>Azure Service Bus ile HTTPS iletişimini zorlama

Doğrudan TCP yerine HTTPS kullanarak ağ geçidini Azure Hizmet Veri Servisi ile iletişim kurmaya zorlayabilirsiniz; ancak, bunu yapmak performansı büyük ölçüde azaltabilir. *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* dosyasını değerini `AutoDetect` değiştirerek . `Https` Bu dosya genellikle *C:\Program Files\In-premises veri ağ geçidi*nde bulunur.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <a name="next-steps"></a>Sonraki adımlar 

Aşağıdaki makaleler, ağ geçidinin desteklediği tüm hizmetler için geçerli olan şirket içi veri ağ geçidi genel içeriğine dahildir:

* [Şirket içi veri ağ geçidi SSS](https://docs.microsoft.com/data-integration/gateway/service-gateway-onprem-faq)   
* [Şirket içi veri ağ geçidi uygulamasını kullanma](https://docs.microsoft.com/data-integration/gateway/service-gateway-app)   
* [Kiracı düzeyinde yönetim](https://docs.microsoft.com/data-integration/gateway/service-gateway-tenant-level-admin)
* [Proxy ayarlarını yapılandırma](https://docs.microsoft.com/data-integration/gateway/service-gateway-proxy)   
* [İletişim ayarlarını değiştirme](https://docs.microsoft.com/data-integration/gateway/service-gateway-communication)   
* [Günlük dosyalarını yapılandırma](https://docs.microsoft.com/data-integration/gateway/service-gateway-log-files)   
* [Sorun giderme](https://docs.microsoft.com/data-integration/gateway/service-gateway-tshoot)
* [Ağ geçidi performansını izleme ve en iyi duruma getirme](https://docs.microsoft.com/data-integration/gateway/service-gateway-performance)
