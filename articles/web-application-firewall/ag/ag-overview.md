---
title: Azure Web Uygulaması Güvenlik Duvarına Giriş
titleSuffix: Azure Web Application Firewall
description: Bu makale, Uygulama Ağ Geçidi'ndeki web uygulama güvenlik duvarına (WAF) genel bir bakış sağlar
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 11/14/2019
ms.author: victorh
ms.topic: overview
ms.openlocfilehash: 1a210ff7da57147762dcf13e1dda7fee26dbe5f0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74384106"
---
# <a name="azure-web-application-firewall-on-azure-application-gateway"></a>Azure Uygulama Ağ Geçidi'nde Azure Web Uygulaması Güvenlik Duvarı

Azure Uygulama Ağ Geçidi'ndeki Azure Web Uygulama Güvenlik Duvarı (WAF), web uygulamalarınızın yaygın açıklardan ve güvenlik açıklarına karşı merkezi koruma sağlar. Web uygulamaları, yaygın olarak bilinen güvenlik açıklarından yararlanan kötü amaçlı saldırılar tarafından giderek daha fazla hedef alınır. SQL enjeksiyonve çapraz site komut dosyası en yaygın saldırılar arasında yer alıyor.

WaF on Application Gateway, Açık Web Uygulama Güvenliği [Projesi'nden (OWASP) Çekirdek Kural Kümesi (CRS)](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.1, 3.0 veya 2.2.9'u temel almaktadır. WAF, ek yapılandırma gerekmeden yeni güvenlik açıklarına karşı koruma içerecek şekilde otomatik olarak güncellenir. 

Aşağıda listelenen TÜM WAF özellikleri bir WAF Politikası içinde bulunmaktadır. Birden çok ilke oluşturabilirsiniz ve bunlar bir Uygulama Ağ Geçidi, tek tek dinleyiciveya bir Uygulama Ağ Geçidi'ndeki yol tabanlı yönlendirme kurallarıyla ilişkilendirilebilir. Bu şekilde, gerekirse Uygulama Ağ Geçidinizin arkasındaki her site için ayrı ilkeler etütleriniz olabilir. WAF İlkeleri hakkında daha fazla bilgi için [bkz.](create-waf-policy-ag.md)

   > [!NOTE]
   > Site başına ve URI BAŞıNA WAF İlkeleri Genel Önizleme'dedir. Bu, bu özelliğin Microsoft'un Ek Kullanım Koşullarına tabi olduğu anlamına gelir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

![Uygulama Ağ Geçidi WAF diyagramı](../media/ag-overview/waf1.png)

Uygulama Ağ Geçidi bir uygulama teslim denetleyicisi (ADC) olarak çalışır. Güvenli Soket katmanı (SSL) sonlandırma, çerez tabanlı oturum yakınlığı, round-robin yük dağıtımı, içerik tabanlı yönlendirme, birden çok web sitesini barındırma olanağı ve güvenlik geliştirmeleri sunar.

Uygulama Ağ Geçidi güvenlik geliştirmeleri SSL ilke yönetimi ve uçlardan uca SSL desteği içerir. Uygulama güvenliği, WAF'ın Application Gateway'e entegrasyonu yla güçlendirilir. Bu kombinasyon, web uygulamalarınızı sık karşılaşılan güvenlik açıklarına karşı korur. Ayrıca, yönetilmesi kolay bir merkezi konum sağlar.

## <a name="benefits"></a>Avantajlar

Bu bölümde, WAF'ın Application Gateway'de sağladığı temel avantajlar açıklanmaktadır.

### <a name="protection"></a>Koruma

* Web uygulamalarınızı arka uç kodda değişiklik yapmadan web güvenlik açıklarından ve saldırılara karşı koruyun.

* Aynı anda birden çok web uygulamasını koruyun. Uygulama Ağ Geçidi'nin bir örneği, bir web uygulaması güvenlik duvarı tarafından korunan en fazla 40 web sitesini barındırabilir.

* Aynı WAF'ın arkasındaki farklı siteler için özel WAF ilkeleri oluşturma 

* IP Reputation kural kümesi (önizleme) ile web uygulamalarınızı kötü amaçlı botlardan koruyun

### <a name="monitoring"></a>İzleme

* Gerçek zamanlı WAF günlüğü kullanarak web uygulamalarınıza yönelik saldırıları izleyin. Günlük, WAF uyarılarını izlemek ve eğilimleri kolayca izlemek için [Azure Monitor](../../azure-monitor/overview.md) ile entegre edilmiştir.

* Application Gateway WAF, Azure Güvenlik Merkezi ile tümleşiktir. Güvenlik Merkezi, tüm Azure kaynaklarınızın güvenlik durumunun merkezi bir görünümünü sağlar.

### <a name="customization"></a>Özelleştirme

* WAF kurallarını ve kural gruplarını uygulama gereksinimlerinize uyacak şekilde özelleştirin ve yanlış pozitifleri ortadan kaldırın.

* Siteye özgü yapılandırmaya izin vermek için WAF'ınızın arkasındaki her site için bir WAF Politikası ilişkilendirin

* Uygulamanızın gereksinimlerine uygun özel kurallar oluşturun

## <a name="features"></a>Özellikler

- SQL enjeksiyon koruması.
- Site arası komut dosyası koruması.
- Komut enjeksiyonu, HTTP isteği kaçakçılık, HTTP yanıt bölme ve uzaktan dosya ekleme gibi diğer yaygın web saldırılarına karşı koruma.
- HTTP protokol ihlallerine karşı koruma.
- Eksik ana bilgisayar aracısı ve üstbilgi kabul gibi HTTP protokolü anormalliklerine karşı koruma.
- Tarayıcılara ve tarayıcılara karşı koruma.
- Yaygın uygulama yanlış yapılandırmalarının algılanması (örneğin, Apache ve IIS).
- Alt ve üst sınırlar ile yapılandırılabilir istek boyutu sınırları.
- Dışlama listeleri, WAF değerlendirmesinden belirli istek özniteliklerini atlayasınız. Yaygın bir örnek, kimlik doğrulama veya parola alanları için kullanılan Active Directory eklenmiş belirteçleridir.
- Uygulamalarınızın özel gereksinimlerine uyacak özel kurallar oluşturun.
- Belirli ülkelerin uygulamalarınıza erişmesine izin vermek veya engellemek için trafiği coğrafi filtreleyin. (önizleme)
- Bot azaltma kuralı ile uygulamalarınızı botlardan koruyun. (önizleme)

## <a name="waf-policy"></a>WAF Politikası

Bir Uygulama Ağ Geçidi'nde bir Web Uygulaması Güvenlik Duvarı etkinleştirmek için bir WAF İlkesi oluşturmanız gerekir. Bu İlke, tüm yönetilen kuralların, özel kuralların, hariç tutmaların ve dosya yükleme sınırı gibi diğer özelleştirmelerin bulunduğu yerdir. 

### <a name="core-rule-sets"></a>Çekirdek kural kümeleri

Uygulama Ağ Geçidi üç kural kümesini destekler: CRS 3.1, CRS 3.0 ve CRS 2.2.9. Bu kurallar, web uygulamalarınızı kötü amaçlı etkinliklere karşı korur.

Daha fazla bilgi için [Bkz. Web uygulaması güvenlik duvarı CRS kural grupları ve kuralları.](application-gateway-crs-rulegroups-rules.md)

### <a name="custom-rules"></a>Özel kurallar

Uygulama Ağ Geçidi de özel kuralları destekler. Özel kurallarla, WAF'den geçen her istek için değerlendirilen kendi kurallarınızı oluşturabilirsiniz. Bu kurallar, yönetilen kural kümelerinde yer alan diğer kurallardan daha yüksek bir önceliğe sahip. Bir dizi koşul yerine getirilirse, izin vermek veya engellemek için bir eylem yapılır. 

Geomatch işleci artık özel kurallar için genel önizlemede kullanılabilir. Daha fazla bilgi için lütfen [geomatch özel kurallarına](custom-waf-rules-overview.md#geomatch-custom-rules-preview) bakın.

> [!NOTE]
> Özel kurallar için geomatch işleci şu anda genel önizlemede dir ve bir önizleme hizmet düzeyi sözleşmesiyle sağlanır. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Özel kurallar hakkında daha fazla bilgi [için Uygulama Ağ Geçidi için Özel Kurallar'a bakın.](custom-waf-rules-overview.md)

### <a name="bot-mitigation-preview"></a>Bot Azaltma (önizleme)

Yönetilen Bot koruma kuralı kümesi, WAF'Nizin yönetilen kural kümesinin yanı sıra bilinen kötü amaçlı IP adreslerinden gelen istekleri engellemesi veya günlüğe kaydetmesi için etkinleştirilebilir. IP adresleri Microsoft Threat Intelligence akışından kaynaklanmaktadır. Akıllı Güvenlik Grafiği Microsoft tehdit zekasına güç sağlar ve Azure Güvenlik Merkezi de dahil olmak üzere birden çok hizmet tarafından kullanılır.

> [!NOTE]
> Bot koruma kuralı kümesi şu anda genel önizlemededir ve bir önizleme hizmeti düzeyi sözleşmesiyle sağlanır. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bot Koruması etkinse, Kötü Amaçlı Bot'un istemci IP'leri ile eşleşen gelen istekler Güvenlik Duvarı günlüğünde günlüğe kaydedilir ve daha fazla bilgi için aşağıya bakın. WAF günlüklerine depolama hesabından, etkinlik merkezinden veya günlük analitiğinden erişebilirsiniz. 

### <a name="waf-modes"></a>WAF modları

Uygulama Ağ Geçidi WAF aşağıdaki iki modda çalışacak şekilde yapılandırılabilir:

* **Algılama modu**: Tüm tehdit uyarılarını izler ve kaydeder. **Tanılama** bölümünde Uygulama Ağ Geçidi için günlük tanılamayı açarsınız. AYRıCA WAF günlüğünün seçilip açık olduğundan da emin olmalısınız. Web uygulaması güvenlik duvarı Algılama modunda çalışırken gelen istekleri engellemez.
* **Önleme modu**: Kuralların algıladığınız izinsiz girişleri ve saldırıları engeller. Saldırgan bir "403 yetkisiz erişim" özel durumu alır ve bağlantı kapatılır. Önleme modu waf günlüklerinde bu tür saldırıları kaydeder.

> [!NOTE]
> Üretim ortamında kısa bir süre için Algılama modunda yeni dağıtılan bir WAF çalıştırmanız önerilir. Bu, [güvenlik duvarı günlükleri](../../application-gateway/application-gateway-diagnostics.md#firewall-log) elde etme ve Önleme moduna geçmeden önce özel durumları veya [özel kuralları](./custom-waf-rules-overview.md) güncelleştirme fırsatı sağlar. Bu, beklenmeyen engellenen trafiğin oluşumunu azaltmaya yardımcı olabilir.

### <a name="anomaly-scoring-mode"></a>Anomali Puanlama modu

OWASP'nin trafiği engelleyip engellememeye karar vermek için iki modu vardır: Geleneksel mod ve Anomali Puanlama modu.

Geleneksel modda, herhangi bir kuralla eşleşen trafik, diğer kural eşleşmelerinden bağımsız olarak kabul edilir. Bu modu anlamak kolaydır. Ancak, belirli bir istekle eşleşen kaç kural hakkında bilgi eksikliği bir sınırlamadır. Böylece, Anomali Puanlama modu tanıtıldı. OWASP 3 için varsayılan dır. *x*.

Anomali Puanlama modunda, güvenlik duvarı Önleme modundayken herhangi bir kuralla eşleşen trafik hemen engellenmez. Kurallar belirli bir öneme sahiptir: *Kritik*, *Hata*, *Uyarı*, veya *Bildirim*. Bu önem derecesi, Anomali Skoru olarak adlandırılan istek için sayısal bir değeri etkiler. Örneğin, bir *Uyarı* kuralı eşleşmesi skora 3 katkıda bulunur. Bir *Kritik* kural maç 5 katkıda bulunur.

|Severity  |Değer  |
|---------|---------|
|Kritik     |5|
|Hata        |4|
|Uyarı      |3|
|Fark       |2|

Anomali Skoru'nun trafiği engellemesi için 5 barajı var. Bu nedenle, tek bir *Kritik* kural eşleşmesi, Uygulama Ağ Geçidi WAF'ın Önleme modunda bile bir isteği engellemesi için yeterlidir. Ama bir *Uyarı* kuralı eşleşmesi Anomali Skorunu sadece 3 arttırır, bu da trafiği engellemek için tek başına yeterli değildir.

> [!NOTE]
> WAF kuralı trafikle eşleştiğinde günlüğe kaydedilen ileti, "Engellendi" eylem değerini içerir. Ama trafik aslında sadece 5 veya daha yüksek bir Anomali Puanı için bloke edilir.  

### <a name="waf-monitoring"></a>WAF izleme

Uygulama ağ geçidinizin durumunu izlemek önemlidir. WAF'Nizin ve koruduğu uygulamaların sistem durumunuizlemek, Azure Güvenlik Merkezi, Azure Monitor ve Azure Monitor günlükleriyle tümleştirmeyle desteklenir.

![Uygulama Ağ Geçidi WAF tanılama diyagramı](../media/ag-overview/diagnostics.png)

#### <a name="azure-monitor"></a>Azure İzleyici

Uygulama Ağ Geçidi günlükleri [Azure Monitor](../../azure-monitor/overview.md)ile entegre edilmiştir. Bu, WAF uyarıları ve günlükleri de dahil olmak üzere tanılama bilgilerini izlemenize olanak tanır. Bu yeteneğe portaldaki Uygulama Ağ Geçidi kaynağındaki **Tanılama** sekmesinden veya doğrudan Azure Monitor üzerinden erişebilirsiniz. Günlükleri etkinleştirme hakkında daha fazla bilgi edinmek için [Uygulama Ağ Geçidi tanılama'ya](../../application-gateway/application-gateway-diagnostics.md)bakın.

#### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

[Güvenlik Merkezi,](../../security-center/security-center-intro.md) tehditleri önlemenize, algılamanıza ve yanıt vermenize yardımcı olur. Azure kaynaklarınızın güvenliği ne kadar görünürlük ve denetim sağlar. Uygulama Ağ Geçidi [Güvenlik Merkezi ile entegre](../../application-gateway/application-gateway-integration-security-center.md)edilmiştir. Güvenlik Merkezi, korumasız web uygulamalarını algılamak için ortamınızı tarar. Bu güvenlik açığı olan kaynakları korumak için Application Gateway WAF'ı önerebilir. Güvenlik duvarlarını doğrudan Güvenlik Merkezi'nden oluşturursunuz. Bu WAF örnekleri Güvenlik Merkezi ile tümleşiktir. Bildirim için Güvenlik Merkezi'ne uyarı ve sağlık bilgileri gönderirler.

![Güvenlik Merkezi genel bakış penceresi](../media/ag-overview/figure1.png)

#### <a name="azure-sentinel"></a>Azure Sentinel

Microsoft Azure Sentinel ölçeklenebilir, bulut ait, güvenlik bilgileri olay yönetimi (SIEM) ve güvenlik düzenleme otomatik yanıt (SOAR) çözümüdür. Azure Sentinel, kuruluş genelinde akıllı güvenlik analitiği ve tehdit istihbaratı sunarak uyarı algılama, tehdit görünürlüğü, proaktif avlanma ve tehdit yanıtı için tek bir çözüm sunar.

Yerleşik Azure WAF güvenlik duvarı etkinlikleri çalışma kitabıyla, WAF'ınızdaki güvenlik olaylarına genel bir bakış elde edebilirsiniz. Buna olaylar, eşleşen ve engellenen kurallar ve güvenlik duvarı günlüklerinde günlüğe kaydedilen diğer her şey dahildir. Aşağıda günlük hakkında daha fazla bkz. 


![Sentinel](../media/ag-overview/sentinel.png)

#### <a name="logging"></a>Günlüğe Kaydetme

Uygulama Ağ Geçidi WAF, algıladığını her tehdit hakkında ayrıntılı raporlama sağlar. Günlüğe kaydetme, Azure Tanılama günlükleriyle tümleştirilir. Uyarılar .json biçiminde kaydedilir. Bu günlükler Azure [Monitor günlükleriyle](../../azure-monitor/insights/azure-networking-analytics.md)tümleştirilebilir.

![Uygulama Ağ Geçidi tanılama günlükleri pencereler](../media/ag-overview/waf2.png)

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupId}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{appGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    {
      "instanceId": "ApplicationGatewayRole_IN_0",
      "clientIp": "52.161.109.145",
      "clientPort": "0",
      "requestUri": "/",
      "ruleSetType": "OWASP",
      "ruleSetVersion": "3.0",
      "ruleId": "920350",
      "ruleGroup": "920-PROTOCOL-ENFORCEMENT",
      "message": "Host header is a numeric IP address",
      "action": "Matched",
      "site": "Global",
      "details": {
        "message": "Warning. Pattern match \"^[\\\\d.:]+$\" at REQUEST_HEADERS:Host ....",
        "data": "127.0.0.1",
        "file": "rules/REQUEST-920-PROTOCOL-ENFORCEMENT.conf",
        "line": "791"
      },
      "hostname": "127.0.0.1",
      "transactionId": "16861477007022634343"
      "policyId": "/subscriptions/1496a758-b2ff-43ef-b738-8e9eb5161a86/resourceGroups/drewRG/providers/Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/globalWafPolicy",
      "policyScope": "Global",
      "policyScopeName": " Global "
    }
  }
} 

```

## <a name="application-gateway-waf-sku-pricing"></a>Application Gateway WAF SKU fiyatlandırması

Fiyatlandırma modelleri WAF_v1 ve WAF_v2 SNU'lar için farklıdır. Daha fazla bilgi için lütfen [Uygulama Ağ Geçidi fiyatlandırma](https://azure.microsoft.com/pricing/details/application-gateway/) sayfasına bakın. 

## <a name="next-steps"></a>Sonraki adımlar

- [WAF ilkesi oluşturarak](create-waf-policy-ag.md) başlayın
- [WAF yönetilen kurallar](application-gateway-crs-rulegroups-rules.md) hakkında daha fazla bilgi edinin
- [Özel Kurallar](custom-waf-rules-overview.md) hakkında daha fazla bilgi edinin
- Azure [Ön Kapı'da Web Uygulaması Güvenlik Duvarı](../afds/afds-overview.md) hakkında bilgi edinin

