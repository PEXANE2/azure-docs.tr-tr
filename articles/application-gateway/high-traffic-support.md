---
title: Uygulama Ağ Geçidi yüksek trafik hacmi desteği
description: Bu makalede, yüksek ağ trafiği hacmi senaryoları desteklemek için Azure Uygulama Ağ Geçidi yapılandırmak için kılavuz sağlar.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 03/24/2020
ms.author: caya
ms.openlocfilehash: 65f404b52a5fc06d8fa5bb5aad291e57fde8caba
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80617194"
---
# <a name="application-gateway-high-traffic-support"></a>Application Gateway yüksek trafik desteği

>[!NOTE]
> Bu makalede, COVID-19 krizi nedeniyle oluşabilecek yüksek trafik hacmi nedeniyle ek trafiği işlemek için Uygulama Ağ Geçidinizi ayarlamanıza yardımcı olacak önerilen birkaç yönerge açıklanmaktadır.

Web uygulamalarınızdaki trafiği yönetmek için ölçeklenebilir ve güvenli bir yol için Web Uygulama Güvenlik Duvarı (WAF) ile Uygulama Ağ Geçidi'ni kullanabilirsiniz.

Aşağıdaki öneriler, ekstra trafiği işlemek için WAF ile Uygulama Ağ Geçidi'ni kurmanıza yardımcı olur.

## <a name="use-the-v2-sku-over-v1-for-its-autoscaling-capabilities-and-performance-benefits"></a>Otomatik ölçekleme özellikleri ve performans avantajları için v1 üzerinden v2 SKU'yu kullanın
v2 SKU, Trafik arttıkça Uygulama Ağ Geçidinizin ölçeklendirilebilmesini sağlamak için otomatik ölçeklendirme sunar. Ayrıca, v1 ile karşılaştırıldığında 5 kat daha iyi TLS boşaltma performansı, daha hızlı dağıtım ve güncelleştirme süreleri, bölge artıklığı ve daha fazlası gibi diğer önemli performans avantajları da sunar. Daha fazla bilgi için [v2 belgelerimize](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant)bakın. 

## <a name="set-maximum-instance-count-to-the-maximum-possible-125"></a>Maksimum örnek sayısını mümkün olan en yüksek sayıya ayarlama (125)
 
Bir Uygulama Ağ Geçidi v2 SKU'nuz olduğunu varsayarsak, en yüksek örnek sayısını 125'in olası değerine ayarlamak, Uygulama Ağ Geçidi'nin gerektiği gibi ölçeklendirmesine olanak tanır. Bu, uygulamalarınıza trafik olası artış işlemek için izin verir. Yalnızca kullandığınız Kapasite Birimleri (CUs) için ücretlendirilirsiniz.  

## <a name="set-your-minimum-instance-count-based-on-your-average-cu-usage"></a>Ortalama CU kullanımınıza göre minimum örnek sayınızı ayarlama

Bir Uygulama Ağ Geçidi v2 SKU'nuz olduğunu varsayarsak, otomatik ölçeklendirmenin ölçeklendirmesi altı ila yedi dakika sürer. Daha yüksek bir minimum örnek sayısıyla, yük arttığında Uygulama Ağ Geçidi trafiğinizi daha iyi işleyebilir, çünkü trafikteki ani artış otomatik ölçekleme işlemi gerektirmez.  

## <a name="alert-if-a-certain-metric-surpasses-75-of-average-cu-utilization"></a>Belirli bir metrik ortalama CU kullanımının %75'ini aşması durumunda uyarı 
Ölçümlerimiz ve diğer izbimizlerimiz hakkında ayrıntılı bir açıklama için [Uygulama Ağ Geçidi Ölçümleri belgelerine](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics#metrics-visualization) bakın. 

### <a name="example-setting-up-an-alert-on-75-of-average-cu-usage"></a>Örnek: Ortalama CU kullanımının %75'inde uyarı ayarlama

Bu örnek, ortalama CU kullanımının %75'ine ulaşıldığında bir uyarı oluşturmak için Azure portalını nasıl kullanacağınızı gösterir. 
1. **Uygulama Ağ Geçidinize**gidin.
2. Sol panelde, **İzleme** sekmesinin altındaki **Ölçümler'i** seçin. 
3. Ortalama Geçerli **İşlem Birimleri**için metrik ekleyin. 
![WAF ölçümü ayarlama](./media/application-gateway-covid-guidelines/waf-setup-metrics.png)
4. Minimum örnek sayınızı ortalama CU kullanımınız olarak ayarladıysanız, minimum örneklerinizin %75'i kullanımdayken bir uyarı belirleyin. Örneğin, ortalama kullanımınız 10 CUs ise, 7,5 CUs'ta bir uyarı ayarlayın. Bu, kullanımın artması durumunda sizi uyarır ve yanıt vermek için size zaman verir. Bu trafiğin, trafiğin artmakta olabileceği konusunda sizi uyarmak için sürdürüleceğini düşünüyorsanız, minimum yükseltme yapabilirsiniz. 
![WAF uyarısı ayarlama](./media/application-gateway-covid-guidelines/waf-setup-monitoring-alert.png)

> [!NOTE]
> Olası trafik artışlarına karşı ne kadar hassas olmak istediğinize bağlı olarak uyarıyı daha düşük veya daha yüksek cu kullanım yüzdesi olarak ayarlayabilirsiniz.

## <a name="set-up-waf-with-geofiltering-and-bot-protection-to-stop-attacks"></a>Saldırıları durdurmak için WAF'ı geofiltering ve bot koruması yla ayarlama
Uygulamanızın önünde fazladan bir güvenlik katmanı istiyorsanız, WAF yetenekleri için SKU WAF_v2 Uygulama Ağ Geçidi'ni kullanın. V2 SKU'yu yalnızca belirli bir ülkeden veya ülkelerden uygulamalarınıza erişime izin verecek şekilde yapılandırabilirsiniz. Coğrafi konuma bağlı olarak trafiğe açıkça izin vermek veya engellemek için bir WAF özel kuralı ayarlarsınız. Daha fazla bilgi için, [geofiltering özel kurallar](https://docs.microsoft.com/azure/web-application-firewall/ag/geomatch-custom-rules) ve [PowerShell üzerinden Uygulama Ağ Geçidi WAF_v2 SKU özel kuralları yapılandırmak için nasıl](https://docs.microsoft.com/azure/web-application-firewall/ag/configure-waf-custom-rules)bakın.

Bilinen kötü botları engellemek için bot korumasını etkinleştirin. Bu, uygulamanıza gelen trafik miktarını azaltacak. Daha fazla bilgi için, [kurulum yönergeleri ile bot koruması](https://docs.microsoft.com/azure/web-application-firewall/ag/configure-waf-custom-rules)na bakın.

## <a name="turn-on-diagnostics-on-application-gateway-and-waf"></a>Uygulama Ağ Geçidi ve WAF'da tanılamayı açın

Tanılama günlükleri, güvenlik duvarı günlüklerini, performans günlüklerini ve erişim günlüklerini görüntülemenize olanak sağlar. Uygulama Ağ Geçitlerini yönetmek ve sorun gidermek için bu günlükleri Azure'da kullanabilirsiniz. Daha fazla bilgi için [tanılama belgelerimize](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#diagnostic-logging)bakın. 

## <a name="set-up-an-tls-policy-for-extra-security"></a>Ekstra güvenlik için bir TLS ilkesi ayarlama
En son TLS politika sürümünü[(AppGwSslPolicy20170401S)](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-policy-overview#appgwsslpolicy20170401s)kullandığınızdan emin olun. Bu tls 1.2 ve daha güçlü şifreleri zorlar. Daha fazla bilgi için [PowerShell üzerinden TLS ilke sürümlerini ve şifre paketlerini yapılandırmaya](https://docs.microsoft.com/azure/application-gateway/application-gateway-configure-ssl-policy-powershell)bakın.
