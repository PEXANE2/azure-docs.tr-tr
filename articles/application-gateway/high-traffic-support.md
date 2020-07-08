---
title: Application Gateway yüksek trafik hacmi desteği
description: Bu makalede, Azure Application Gateway 'yi yüksek ağ trafiği hacmi ile desteklemek için yapılandırma kılavuzu sunulmaktadır.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: caya
ms.openlocfilehash: b96720ead2c7b7bc942efca32a8510f57c2dbcad
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85250257"
---
# <a name="application-gateway-high-traffic-support"></a>Application Gateway yüksek trafik desteği

>[!NOTE]
> Bu makalede, COVıD-19 pase nedeniyle ortaya çıkabilecek yüksek trafikli bir birim nedeniyle ek trafiği işlemek üzere Application Gateway ayarlamanıza yardımcı olacak birkaç önerilen yönerge açıklanmaktadır.

Web uygulamalarınızdaki trafiği yönetmenin ölçeklenebilir ve güvenli bir yolu için Web uygulaması güvenlik duvarı (WAF) ile Application Gateway kullanabilirsiniz.

Aşağıdaki öneriler, ek trafiği işlemek için WAF ile Application Gateway ayarlamanıza yardımcı olur.

## <a name="use-the-v2-sku-over-v1-for-its-autoscaling-capabilities-and-performance-benefits"></a>Otomatik ölçeklendirme özellikleri ve performans avantajları için v1 üzerinde v2 SKU 'SU kullanın
V2 SKU 'SU, Application Gateway trafik artışına göre ölçeklenebilmesini sağlamak için otomatik ölçeklendirme olanağı sunar. Ayrıca,/v1 ile karşılaştırıldığında, 5x daha iyi TLS yük boşaltma performansı, daha hızlı dağıtım ve güncelleştirme süreleri, bölge artıklığı ve daha fazlası gibi önemli performans avantajları da sunar. Daha fazla bilgi için [v2 belgelerimize](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant)bakın. 

## <a name="set-maximum-instance-count-to-the-maximum-possible-125"></a>En fazla örnek sayısını mümkün olan en yüksek sayıya ayarla (125)
 
Application Gateway v2 SKU 'sunun olduğu varsayılırsa, en fazla örnek sayısını en yüksek 125 değerine ayarlamak Application Gateway gerektiği gibi ölçeklenebilmesini sağlar. Bu, uygulamalarınıza giden trafiğin olası artışını işlemesini sağlar. Yalnızca kullandığınız Kapasite birimleri (cu düzeyinde kapsanır) için ücretlendirilirsiniz.  

## <a name="set-your-minimum-instance-count-based-on-your-average-cu-usage"></a>Ortalama CU kullanımınız temelinde en az örnek sayısını ayarlayın

Bir Application Gateway v2 SKU 'SU olduğu varsayıldığında, otomatik ölçeklendirme ölçeği genişletmek için altı ila yedi dakika sürer. Daha yüksek örnek sayısı ile, yük arttığı sırada Application Gateway trafiğinizi daha iyi işleyebilir, çünkü trafikte ani bir otomatik ölçeklendirme işlemi gerektirmez.  

## <a name="alert-if-a-certain-metric-surpasses-75-of-average-cu-utilization"></a>Belirli bir ölçüm, ortalama CU kullanımının %75 ' üne geçerse uyarır 
Ölçümlerimizin ve diğer talimatlarımızın ayrıntılı bir açıklaması için [Application Gateway ölçümleri belgelerine](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics#metrics-visualization) bakın. 

### <a name="example-setting-up-an-alert-on-75-of-average-cu-usage"></a>Örnek: ortalama CU kullanımının %75 ' de uyarı ayarlanıyor

Bu örnek, ortalama CU kullanımının %75 ' e ulaşıldığında bir uyarı ayarlamak için Azure portal nasıl kullanacağınızı gösterir. 
1. **Application Gateway**gidin.
2. Sol bölmede, **izleme** sekmesinin altındaki **ölçümler** ' i seçin. 
3. **Ortalama geçerli Işlem birimleri**için bir ölçüm ekleyin. 
![WAF ölçümünü ayarlama](./media/application-gateway-covid-guidelines/waf-setup-metrics.png)
4. Minimum örnek sayınızı ortalama CU kullanımınız olarak ayarladıysanız, en düşük örneklerinizin %75 ' ünü kullanımda olduğunda bir uyarı ayarlayın. Örneğin, ortalama kullanımınız 10 cu düzeyinde kapsanır ise 7,5 cu düzeyinde kapsanır üzerinde bir uyarı ayarlayın. Bu, kullanım arttırıldığında size uyarı verir ve size zaman yanıt verebilir. Trafiğin artmakta olabileceğini düşünüyorsanız, bu trafiğin sürekli olarak size uyarı vermesi gerektiğini düşünüyorsanız en düşük değeri artırabilirsiniz. 
![WAF uyarısını ayarlama](./media/application-gateway-covid-guidelines/waf-setup-monitoring-alert.png)

> [!NOTE]
> Olası trafik artışlarına ne kadar duyarlı olduğunuza bağlı olarak, uyarının daha düşük veya daha yüksek bir kullanım yüzdesi olarak gerçekleşmesini sağlayabilirsiniz.

## <a name="set-up-waf-with-geofiltering-and-bot-protection-to-stop-attacks"></a>Saldırıları durdurmak için bölge sınırlaması ve bot koruması ile WAF 'yi ayarlama
Uygulamanızın önünde ek bir güvenlik katmanı istiyorsanız WAF özellikleri için Application Gateway WAF_v2 SKU 'SU kullanın. V2 SKU 'sunu yalnızca belirli bir ülke/bölge veya ülkelerden/bölgelerden uygulamalarınıza erişime izin verecek şekilde yapılandırabilirsiniz. Bir WAF özel kuralını, coğrafi konum temelinde trafiği açıkça izin verecek veya engelleyecek şekilde ayarlayabilirsiniz. Daha fazla bilgi için bkz. [bölge için özel kurallar](https://docs.microsoft.com/azure/web-application-firewall/ag/geomatch-custom-rules) ve [PowerShell aracılığıyla Application Gateway WAF_v2 SKU 'sunda özel kurallar yapılandırma](https://docs.microsoft.com/azure/web-application-firewall/ag/configure-waf-custom-rules).

Bilinen kötü botları engellemek için bot korumasını etkinleştirin. Bu, uygulamanıza yönelik trafik miktarını azaltmalıdır. Daha fazla bilgi için bkz. [set up yönergeleri ile bot koruması](https://docs.microsoft.com/azure/web-application-firewall/ag/configure-waf-custom-rules).

## <a name="turn-on-diagnostics-on-application-gateway-and-waf"></a>Application Gateway ve WAF üzerinde tanılamayı aç

Tanılama günlükleri, güvenlik duvarı günlüklerini, performans günlüklerini ve erişim günlüklerini görüntülemenize izin verir. Uygulama ağ geçitlerini yönetmek ve sorunlarını gidermek için bu günlükleri Azure 'da kullanabilirsiniz. Daha fazla bilgi için bkz. [Tanılama Belgelerimiz](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#diagnostic-logging). 

## <a name="set-up-an-tls-policy-for-extra-security"></a>Ek güvenlik için bir TLS ilkesi ayarlama
En son TLS ilkesi sürümünü ([AppGwSslPolicy20170401S](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-policy-overview#appgwsslpolicy20170401s)) kullandığınızdan emin olun. Bu, TLS 1,2 ve daha güçlü şifrelemeleri zorlar. Daha fazla bilgi için bkz. [PowerShell Ile TLS ilke sürümlerini ve şifre paketlerini yapılandırma](https://docs.microsoft.com/azure/application-gateway/application-gateway-configure-ssl-policy-powershell).
