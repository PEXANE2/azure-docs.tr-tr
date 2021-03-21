---
title: Application Gateway yüksek trafik hacmi desteği
description: Bu makalede, Azure Application Gateway 'yi yüksek ağ trafiği hacmi ile desteklemek için yapılandırma kılavuzu sunulmaktadır.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: caya
ms.openlocfilehash: d8940d791920daca6ef0af186a4bb5e17009637b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100586107"
---
# <a name="application-gateway-high-traffic-support"></a>Application Gateway yüksek trafik desteği

>[!NOTE]
> Bu makalede, oluşabilecek tüm yüksek trafikli hacimde çok sayıda trafiği işlemek üzere Application Gateway ayarlamanıza yardımcı olacak birkaç önerilen yönerge açıklanmaktadır. Uyarı eşikleri yalnızca öneriler ve genel olarak geneldir. Kullanıcılar, iş yüklerine ve kullanım beklentilerine göre uyarı eşiklerini tespit edebilir.

Web uygulamalarınızdaki trafiği yönetmenin ölçeklenebilir ve güvenli bir yolu için Web uygulaması güvenlik duvarı (WAF) ile Application Gateway kullanabilirsiniz.

Application Gateway, trafiğiniz veya ani artışlar için hazırlanabilmeniz ve bu durumda, QoS 'de sahip olabileceği etkiyi en aza indirmek için, trafiğinizi ve arabelleğin bir bitini göre ölçeklendirmeniz önemlidir. Aşağıdaki öneriler, ek trafiği işlemek için WAF ile Application Gateway ayarlamanıza yardımcı olur.

Application Gateway tarafından sunulan ölçümlerin tüm listesi için lütfen [ölçüm belgelerini](./application-gateway-metrics.md) denetleyin. Ölçüm uyarılarını ayarlama hakkında bilgi için Azure portal ve [Azure izleyici belgelerindeki](../azure-monitor/alerts/alerts-metric.md) [ölçümleri görselleştirin](./application-gateway-metrics.md#metrics-visualization) .

## <a name="scaling-for-application-gateway-v1-sku-standardwaf-sku"></a>Application Gateway v1 SKU 'SU için ölçeklendirme (Standart/WAF SKU 'SU)

### <a name="set-your-instance-count-based-on-your-peak-cpu-usage"></a>En yoğun CPU kullanımınıza göre örnek sayınızın sayısını ayarlayın
Bir v1 SKU ağ geçidi kullanıyorsanız, ölçeklendirmeyi ölçeklendirmek için en fazla 32 örneğine Application Gateway ayarlayabilirsiniz. Son bir 80 ayda Application Gateway CPU kullanımını kontrol edin, bu, izlemeniz için bir ölçüm olarak sunulmaktadır. Tüm trafik artışlarını hesaba eklemek için örnek sayınızın en yüksek kullanımınıza ve %10 ila %20 ek arabelleğe göre ayarlamanız önerilir.

:::image type="content" source="./media/application-gateway-covid-guidelines/v1-cpu-utilization-inline.png" alt-text="V1 CPU kullanım ölçümleri" lightbox="./media/application-gateway-covid-guidelines/v1-cpu-utilization-exp.png":::

### <a name="use-the-v2-sku-over-v1-for-its-autoscaling-capabilities-and-performance-benefits"></a>Otomatik ölçeklendirme özellikleri ve performans avantajları için v1 üzerinde v2 SKU 'SU kullanın
V2 SKU 'SU, Application Gateway trafik artışına göre ölçeklenebilmesini sağlamak için otomatik ölçeklendirme olanağı sunar. Ayrıca,/v1 ile karşılaştırıldığında, 5x daha iyi TLS yük boşaltma performansı, daha hızlı dağıtım ve güncelleştirme süreleri, bölge artıklığı ve daha fazlası gibi önemli performans avantajları da sunar. Daha fazla bilgi için, [v2 belgelerimize](./application-gateway-autoscaling-zone-redundant.md) bakın ve mevcut v1 SKU ağ geçitlerini v2 SKU 'suna nasıl geçirebileceğinizi öğrenmek için v1 sürüm [geçiş belgelerini](./migrate-v1-v2.md) inceleyin. 

## <a name="autoscaling-for-application-gateway-v2-sku-standard_v2waf_v2-sku"></a>Application Gateway v2 SKU'su (Standard_v2/WAF_v2 SKU) için otomatik ölçeklendirme

### <a name="set-maximum-instance-count-to-the-maximum-possible-125"></a>En fazla örnek sayısını mümkün olan en yüksek sayıya ayarla (125)
 
Application Gateway v2 SKU 'SU için en fazla örnek sayısını en yüksek 125 değerine ayarlamak Application Gateway gerektiği gibi ölçeklenebilmesini sağlar. Bu, uygulamalarınıza giden trafiğin olası artışını işlemesini sağlar. Yalnızca kullandığınız Kapasite birimleri (cu düzeyinde kapsanır) için ücretlendirilirsiniz. 

Alt ağınızın boyutunu ve kullanılabilir IP adresi sayısını, alt ağınızda kontrol ettiğinizden ve en büyük örnek sayısını bu şekilde ayarladığınızdan emin olun. Alt ağınız için yeterli alan yoksa, ağ geçidinizi, yeterli kapasiteye sahip aynı veya farklı bir alt ağda yeniden oluşturmanız gerekecektir. 

:::image type="content" source="./media/application-gateway-covid-guidelines/v2-autoscaling-max-instances-inline.png" alt-text="V2 otomatik ölçeklendirme yapılandırması" lightbox="./media/application-gateway-covid-guidelines/v2-autoscaling-max-instances-exp.png":::

### <a name="set-your-minimum-instance-count-based-on-your-average-compute-unit-usage"></a>Minimum örnek sayınız ortalama Işlem birimi kullanımınıza göre ayarlayın

Application Gateway v2 SKU 'SU için otomatik ölçeklendirme, ölçeği genişletmek ve trafik almaya hazır ek örnek kümesi sağlamak için altı ila yedi dakika sürer. Bu durumda, trafikte kısa ani artışlar varsa, var olan ağ geçidi örneklerinizde stres altında olabilir ve bu durum beklenmedik gecikme süresine veya trafik kaybına neden olabilir. 

Minimum örnek sayınız en uygun düzeye ayarlamanız önerilir. Örneğin, yoğun yük sırasında trafiği işlemek için 50 örneklerine ihtiyacınız varsa, en az 25 ila 30 olarak ayarlanması, <10 ' u yerine en düşük bir fikrtir. böylece, trafiğin kısa olması durumunda bile Application Gateway, otomatik ölçeklendirme yanıt vermek için yeterli zaman alabilir.

Son bir ay için Işlem birimi ölçümünüzün olup olmadığını denetleyin. İşlem birimi ölçümü, ağ geçidinizin CPU kullanımının bir gösterimidir ve en yüksek kullanımınız 10 ' a bölünmesiyle, gereken minimum örnek sayısını ayarlayabilirsiniz. 1 uygulama ağ geçidi örneğinin en az 10 işlem birimini işleyebileceğini unutmayın

:::image type="content" source="./media/application-gateway-covid-guidelines/compute-unit-metrics-inline.png" alt-text="V2 işlem birimi ölçümleri" lightbox="./media/application-gateway-covid-guidelines/compute-unit-metrics-exp.png":::

## <a name="manual-scaling-for-application-gateway-v2-sku-standard_v2waf_v2"></a>Application Gateway v2 SKU 'SU için el ile ölçeklendirme (Standard_v2/WAF_v2)

### <a name="set-your-instance-count-based-on-your-peak-compute-unit-usage"></a>En yüksek Işlem birimi kullanımınıza göre örnek sayınız ayarlayın 

Otomatik ölçeklendirmenin aksine, el ile ölçeklendirilirken, trafik gereksinimlerine bağlı olarak uygulama ağ geçidinizin örneklerinin sayısını el ile ayarlamanız gerekir. Tüm trafik artışlarını hesaba eklemek için örnek sayınızın en yüksek kullanımınıza ve %10 ila %20 ek arabelleğe göre ayarlamanız önerilir. Örneğin, trafiğiniz en yoğun 50 örnek gerektiriyorsa, oluşabilecek beklenmeyen trafik artışlarını işlemek için 55 ' e, 60 örnek sağlayın.

Son bir ay için Işlem birimi ölçümünüzün olup olmadığını denetleyin. İşlem birimi ölçümü, ağ geçidinizin CPU kullanımının bir gösterimidir ve 10 ' a bölünmüş en yüksek kullanımlarınız temelinde, 1 uygulama ağ geçidi örneği en az 10 işlem birimini işleyebileceğinizden, gerekli örnek sayısını ayarlayabilirsiniz.

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı

Herhangi bir trafik veya kullanım anormalileri hakkında bildirim almak için, belirli ölçümler üzerinde uyarılar ayarlayabilirsiniz. Application Gateway tarafından sunulan ölçümlerin tüm listesi için bkz. [ölçüm belgeleri](./application-gateway-metrics.md) . Ölçüm uyarılarını ayarlama hakkında bilgi için Azure portal ve [Azure izleyici belgelerindeki](../azure-monitor/alerts/alerts-metric.md) [ölçümleri görselleştirin](./application-gateway-metrics.md#metrics-visualization) .

## <a name="alerts-for-application-gateway-v1-sku-standardwaf"></a>Application Gateway v1 SKU 'SU (Standart/WAF) için uyarılar

### <a name="alert-if-average-cpu-utilization-crosses-80"></a>Ortalama CPU kullanımı %80 kesişmesi durumunda uyarır

Normal koşullar altında CPU kullanımı, Application Gateway’in arkasında barındırılan web sitelerinde gecikmeye neden olabileceğinden ve istemci deneyimini kesintiye uğratabileceğinden düzenli olarak %90’ı aşmamalıdır. Örnek sayısını artırarak veya daha büyük bir SKU boyutuna geçerek ya da her ikisini de yaparak Application Gateway yapılandırmasını değiştirerek CPU kullanımını dolaylı olarak kontrol edebilir veya artırabilirsiniz. CPU kullanım ölçümü %80 ortalamanın üzerine gittiğinde bir uyarı ayarlayın.

### <a name="alert-if-unhealthy-host-count-crosses-threshold"></a>Sağlıksız ana bilgisayar sayısı eşiğin kesiştiği takdirde uyarır

Bu ölçüm, Application Gateway 'in başarıyla araştırılmadığını arka uç sunucusu sayısını belirtir. Bu, uygulama ağ geçidi örneklerinin arka uca bağlanamadığınız sorunları yakalar. Bu sayı, arka uç kapasitesinin %20 ' si üzerinde olursa uyarır. Örneğin Şu anda arka uç havuzunda 30 arka uç sunucunuz varsa, sağlıksız ana bilgisayar sayısı 6 ' nın üzerine gittiğinde bir uyarı ayarlayın.

### <a name="alert-if-response-status-4xx-5xx-crosses-threshold"></a>Yanıt durumu (4xx, 5xx) eşiği kesiştiği takdirde uyar 

Application Gateway yanıt durumu 4xx veya 5xx olduğunda uyarı oluştur. Geçici sorunlar nedeniyle zaman zaman 4xx veya 5xx yanıtı görüldü. Statik eşiği belirleyebilmek veya uyarı için dinamik eşik kullanmak üzere üretim ortamında ağ geçidini gözlemleyebilirsiniz.

### <a name="alert-if-failed-requests-crosses-threshold"></a>Başarısız istekler eşikle Kesişmeleri durumunda uyar 

Başarısız istek ölçümü eşiği kesiştiği zaman uyarı oluştur. Statik eşiği belirleyebilmek veya uyarı için dinamik eşik kullanmak üzere üretim ortamında ağ geçidini gözlemleyebilirsiniz.

### <a name="example-setting-up-an-alert-for-more-than-100-failed-requests-in-the-last-5-minutes"></a>Örnek: son 5 dakika içinde 100 ' den fazla başarısız istek için uyarı ayarlama

Bu örnek, son 5 dakika içinde başarısız istek sayısı 100 ' den fazla olduğunda bir uyarı ayarlamak için Azure portal nasıl kullanacağınızı gösterir.
1. **Application Gateway** gidin.
2. Sol bölmede, **izleme** sekmesinin altındaki **ölçümler** ' i seçin. 
3. **Başarısız istekler** için bir ölçüm ekleyin.
4. **Yeni uyarı kuralına** tıklayın ve koşullarınızı ve eylemlerinizi tanımlayın
5. Uyarı oluşturmak ve etkinleştirmek için **Uyarı kuralı oluştur** ' a tıklayın

:::image type="content" source="./media/application-gateway-covid-guidelines/create-alerts-inline.png" alt-text="V2 uyarı oluşturma" lightbox="./media/application-gateway-covid-guidelines/create-alerts-exp.png":::

## <a name="alerts-for-application-gateway-v2-sku-standard_v2waf_v2"></a>Application Gateway v2 SKU 'SU (Standard_v2/WAF_v2) için uyarılar

### <a name="alert-if-compute-unit-utilization-crosses-75-of-average-usage"></a>Işlem birimi kullanımı ortalama kullanımının %75 ' ü kesişmesi durumunda uyarır 

İşlem birimi, Application Gateway işlem kullanımının ölçüdür. Son bir ayda ortalama işlem birimi kullanımınızı denetleyin ve bunun %75 ' ü kesiştiği takdirde uyarı ayarlayın. Örneğin, ortalama kullanımınız 10 işlem birimlik ise, 7,5 cu düzeyinde kapsanır üzerinde bir uyarı ayarlayın. Bu, kullanım arttırıldığında size uyarı verir ve size zaman yanıt verebilir. Trafiğin artmakta olabileceğini düşünüyorsanız, bu trafiğin sürekli olarak size uyarı vermesi gerektiğini düşünüyorsanız en düşük değeri artırabilirsiniz. Ölçeği gerektiği kadar genişletmek için yukarıdaki ölçeklendirme önerilerini izleyin.

### <a name="example-setting-up-an-alert-on-75-of-average-cu-usage"></a>Örnek: ortalama CU kullanımının %75 ' de uyarı ayarlanıyor

Bu örnek, ortalama CU kullanımının %75 ' e ulaşıldığında bir uyarı ayarlamak için Azure portal nasıl kullanacağınızı gösterir. 
1. **Application Gateway** gidin.
2. Sol bölmede, **izleme** sekmesinin altındaki **ölçümler** ' i seçin. 
3. **Ortalama geçerli Işlem birimleri** için bir ölçüm ekleyin. 
4. Minimum örnek sayınızı ortalama CU kullanımınız olarak ayarladıysanız, en düşük örneklerinizin %75 ' ünü kullanımda olduğunda bir uyarı ayarlayın. Örneğin, ortalama kullanımınız 10 cu düzeyinde kapsanır ise 7,5 cu düzeyinde kapsanır üzerinde bir uyarı ayarlayın. Bu, kullanım arttırıldığında size uyarı verir ve size zaman yanıt verebilir. Trafiğin artmakta olabileceğini düşünüyorsanız, bu trafiğin sürekli olarak size uyarı vermesi gerektiğini düşünüyorsanız en düşük değeri artırabilirsiniz. 

:::image type="content" source="./media/application-gateway-covid-guidelines/compute-unit-alert-inline.png" alt-text="V2 işlem birimi uyarıları" lightbox="./media/application-gateway-covid-guidelines/compute-unit-alert-exp.png":::

> [!NOTE]
> Olası trafik artışlarına ne kadar duyarlı olduğunuza bağlı olarak, uyarının daha düşük veya daha yüksek bir kullanım yüzdesi olarak gerçekleşmesini sağlayabilirsiniz.

### <a name="alert-if-capacity-unit-utilization-crosses-75-of-peak-usage"></a>Kapasite birimi kullanımı en yüksek kullanımlarda %75 kesişmesi durumunda uyarır 

Kapasite birimleri aktarım hızı, işlem ve bağlantı sayısı açısından genel ağ geçidi kullanımını temsil eder. Son bir ayda maksimum kapasite birimi kullanımınızı denetleyin ve bunun %75 ' ü kesiştiği takdirde uyarı ayarlayın. Örneğin, en büyük kullanımınız 100 kapasite birimleridir, 75 cu düzeyinde kapsanır üzerinde bir uyarı ayarlayın. Ölçeği genişletmek için yukarıdaki iki önerisi izleyin.

### <a name="alert-if-unhealthy-host-count-crosses-threshold"></a>Sağlıksız ana bilgisayar sayısı eşiğin kesiştiği takdirde uyarır 

Bu ölçüm, Application Gateway 'in başarıyla araştırılmadığını arka uç sunucusu sayısını belirtir. Bu, uygulama ağ geçidi örneklerinin arka uca bağlanamadığınız sorunları yakalar. Bu sayı, arka uç kapasitesinin %20 ' si üzerinde olursa uyarır. Örneğin Şu anda arka uç havuzunda 30 arka uç sunucunuz varsa, sağlıksız ana bilgisayar sayısı 6 ' nın üzerine gittiğinde bir uyarı ayarlayın.

### <a name="alert-if-response-status-4xx-5xx-crosses-threshold"></a>Yanıt durumu (4xx, 5xx) eşiği kesiştiği takdirde uyar 

Application Gateway yanıt durumu 4xx veya 5xx olduğunda uyarı oluştur. Geçici sorunlar nedeniyle zaman zaman 4xx veya 5xx yanıtı görüldü. Statik eşiği belirleyebilmek veya uyarı için dinamik eşik kullanmak üzere üretim ortamında ağ geçidini gözlemleyebilirsiniz.

### <a name="alert-if-failed-requests-crosses-threshold"></a>Başarısız istekler eşikle Kesişmeleri durumunda uyar 

Başarısız istek ölçümü eşiği kesiştiği zaman uyarı oluştur. Statik eşiği belirleyebilmek veya uyarı için dinamik eşik kullanmak üzere üretim ortamında ağ geçidini gözlemleyebilirsiniz.

### <a name="alert-if-backend-last-byte-response-time-crosses-threshold"></a>Arka uç son bayt yanıt süresi eşiğin kesiştiği takdirde uyar 

Bu ölçüm, arka uç sunucusuna bağlantı kurma ve yanıt gövdesinin son baytını alma arasındaki zaman aralığını gösterir. Arka uç yanıt gecikmesi belirli eşikten daha fazla olursa uyarı oluştur. Örneğin, arka uç yanıt gecikmesi her zamanki değerden %30 ' dan daha fazla arttığında bunu uyarı olarak ayarlayın.

### <a name="alert-if-application-gateway-total-time-crosses-threshold"></a>Toplam Application Gateway süre eşiğe kesiştiği zaman uyar

Bu, Application Gateway HTTP isteğinin ilk baytını istemciye Son Yanıt baytı gönderilirken geçen zaman olan aralıktır. Arka uç yanıt gecikmesi belirli eşikten daha fazla olursa bir uyarı oluşturulması gerekir. Örneğin, olağan değerden toplam süre gecikme süresi %30 ' dan fazla olduğunda bunu uyarı olarak ayarlayabilirler.

## <a name="set-up-waf-with-geo-filtering-and-bot-protection-to-stop-attacks"></a>Saldırıları durdurmak için coğrafi filtreleme ve bot koruması ile WAF 'yi ayarlama
Uygulamanızın önünde ek bir güvenlik katmanı istiyorsanız WAF özellikleri için Application Gateway WAF_v2 SKU 'SU kullanın. V2 SKU 'sunu yalnızca belirli bir ülke/bölge veya ülkelerden/bölgelerden uygulamalarınıza erişime izin verecek şekilde yapılandırabilirsiniz. Coğrafi konuma göre trafiği açıkça izin vermek veya engellemek için bir WAF özel kuralı ayarlarsınız. Daha fazla bilgi için bkz. [coğrafi filtreleme özel kuralları](../web-application-firewall/ag/geomatch-custom-rules.md) ve [PowerShell aracılığıyla Application Gateway WAF_v2 SKU 'sunda özel kuralları yapılandırma](../web-application-firewall/ag/configure-waf-custom-rules.md).

Bilinen kötü botları engellemek için bot korumasını etkinleştirin. Bu, uygulamanıza yönelik trafik miktarını azaltmalıdır. Daha fazla bilgi için bkz. [set up yönergeleri ile bot koruması](../web-application-firewall/ag/configure-waf-custom-rules.md).

## <a name="turn-on-diagnostics-on-application-gateway-and-waf"></a>Application Gateway ve WAF üzerinde tanılamayı aç

Tanılama günlükleri, güvenlik duvarı günlüklerini, performans günlüklerini ve erişim günlüklerini görüntülemenize izin verir. Uygulama ağ geçitlerini yönetmek ve sorunlarını gidermek için bu günlükleri Azure 'da kullanabilirsiniz. Daha fazla bilgi için bkz. [Tanılama Belgelerimiz](./application-gateway-diagnostics.md#diagnostic-logging). 

## <a name="set-up-an-tls-policy-for-extra-security"></a>Ek güvenlik için bir TLS ilkesi ayarlama
En son TLS ilkesi sürümünü ([AppGwSslPolicy20170401S](./application-gateway-ssl-policy-overview.md#appgwsslpolicy20170401s)) kullandığınızdan emin olun. Bu, TLS 1,2 ve daha güçlü şifrelemeleri zorlar. Daha fazla bilgi için bkz. [PowerShell Ile TLS ilke sürümlerini ve şifre paketlerini yapılandırma](./application-gateway-configure-ssl-policy-powershell.md).