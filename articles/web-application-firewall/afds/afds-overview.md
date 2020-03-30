---
title: Azure Ön Kapı'daki Azure web uygulaması güvenlik duvarı nedir?
description: Azure Ön Kapı hizmetindeki Azure web uygulaması güvenlik duvarının web uygulamalarınızı kötü amaçlı saldırılara karşı nasıl koruduğunun öğrenin.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: overview
ms.date: 02/01/2020
ms.author: victorh
ms.openlocfilehash: c8ff1849668d5effe15b6c25d00f3965a17b8e3e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77915648"
---
# <a name="azure-web-application-firewall-on-azure-front-door"></a>Azure Ön Kapı'da Azure Web Uygulaması Güvenlik Duvarı

Azure Ön Kapı'daki Azure Web Uygulama Güvenlik Duvarı (WAF), web uygulamalarınız için merkezi koruma sağlar. WAF, web hizmetlerinizi sık karşılaşılan açıklara ve güvenlik açıklarına karşı korur. Hizmetinizi kullanıcılarınız için yüksek oranda kullanılabilir tutar ve uyumluluk gereksinimlerini karşılamanıza yardımcı olur.

WAF On Front Door küresel ve merkezi bir çözümdür. Dünya çapındaki Azure ağ kenarı konumlarında dağıtılır. WAF etkin web uygulamaları, Ön Kapı tarafından teslim edilen her gelen isteği ağ kenarında inceler. 

WAF, sanal ağınıza girmeden önce saldırı kaynaklarına yakın kötü amaçlı saldırıları önler. Performanstan ödün vermeden ölçekte küresel koruma elde elabilirsiniz. WAF politikası, aboneliğinizdeki herhangi bir Ön Kapı profiline kolayca bağlanır. Yeni kurallar birkaç dakika içinde dağıtılabilir, böylece değişen tehdit kalıplarına hızlı bir şekilde yanıt verebilirsiniz.

![Azure web uygulaması güvenlik duvarı](../media/overview/wafoverview.png)

## <a name="waf-policy-and-rules"></a>WAF politikası ve kuralları

Bir WAF ilkesini yapılandırabilir ve bu ilkeyi koruma için bir veya daha fazla Ön Kapı ön uçlarıyla ilişkilendirebilirsiniz. WAF ilkesi iki tür güvenlik kuralından oluşur:

- müşteri tarafından yazılmış özel kurallar.

- Azure tarafından yönetilen önceden yapılandırılmış kurallar kümesi koleksiyonu olan yönetilen kural kümeleri.

Her ikisi de mevcut olduğunda, yönetilen bir kural kümesinde kuralları işlemeden önce özel kurallar işlenir. Bir kural bir eşleşme koşulu, bir öncelik ve bir eylem yapılır. Desteklenen eylem türleri şunlardır: ALLOW, BLOCK, LOG ve DIRECT. Yönetilen ve özel kuralları birleştirerek özel uygulama koruma gereksinimlerinizi karşılayan tamamen özelleştirilmiş bir ilke oluşturabilirsiniz.

İlke içindeki kurallar öncelikli sırada işlenir. Öncelik, işleme kurallarının sırasını tanımlayan benzersiz bir tamsayıdır. Daha küçük tamsayı değeri daha yüksek bir önceliği gösterir ve bu kurallar daha yüksek bir tamsayı değerine sahip kurallardan önce değerlendirilir. Bir kural eşleştikten sonra, kuralda tanımlanan ilgili eylem isteğe uygulanır. Böyle bir eşleşme işlendikten sonra, daha düşük önceliklere sahip kurallar daha fazla işlenmez.

Front Door tarafından teslim edilen bir web uygulaması, aynı anda onunla ilişkili yalnızca bir WAF ilkesine sahip olabilir. Ancak, onunla ilişkili herhangi bir WAF ilkeleri olmadan bir Ön Kapı yapılandırması olabilir. Bir WAF ilkesi varsa, dünya çapında tutarlı güvenlik politikaları sağlamak için tüm kenar konumlarımızda çoğaltılır.

## <a name="waf-modes"></a>WAF modları

WAF ilkesi aşağıdaki iki modda çalışacak şekilde yapılandırılabilir:

- **Algılama modu:** Algılama modunda çalıştırıldığında, WAF izlemeler dışında başka bir eylemde bulunmaz ve isteği ve eşleşen WAF kuralını WAF günlüklerine kaydeder. Ön Kapı için günlük tanılama açabilirsiniz. Portalı kullandığınızda **Tanılama** bölümüne gidin.

- **Önleme modu:** Önleme modunda, bir istek bir kuralla eşleşirse WAF belirtilen eylemi alır. Bir eşleşme bulunursa, daha düşük önceliğe sahip başka kural değerlendirilmez. Eşleşen istekler WAF günlüklerinde de günlüğe kaydedilir.

## <a name="waf-actions"></a>WAF eylemleri

WAF müşterileri, bir istek bir kuralın koşullarıyla eşleştiğinde eylemlerden birinden çalıştırmayı seçebilir:

- **İzin ver:**  İstek WAF'tan geçer ve arka uca iletilir. Daha düşük öncelik kuralları bu isteği engelleyebilir.
- **Blok:** İstek engellenir ve WAF isteği arka uca iletmeden istemciye bir yanıt gönderir.
- **Günlük:**  İstek WAF günlüklerinde günlüğe kaydedilir ve WAF daha düşük öncelik kurallarını değerlendirmeye devam eder.
- **Yeniden yönlendirme:** WAF isteği belirtilen URI'ye yönlendirir. Belirtilen URI bir ilke düzeyi ayarıdır. Yapılandırıldıktan sonra, **Yönlendirme** eylemiyle eşleşen tüm istekler o URI'ye gönderilir.

## <a name="waf-rules"></a>WAF kuralları

WAF ilkesi iki tür güvenlik kuralından oluşabilir : müşteri tarafından yazılmış özel kurallar ve yönetilen kural kümeleri, Azure tarafından önceden yapılandırılmış kurallar kümesi.

### <a name="custom-authored-rules"></a>Özel yazarkuralları

WAF özel kurallarını aşağıdaki gibi yapılandırabilirsiniz:

- **IP izin listesi ve blok listesi:** Web uygulamalarınıza erişimi istemci IP adresleri veya IP adresi aralıklarının listesini temel alınabiliyor. Hem IPv4 hem de IPv6 adres türleri desteklenir. Bu liste, kaynak IP'nin listedeki bir IP ile eşleştiği bu istekleri engelleyecek veya izin verecek şekilde yapılandırılabilir.

- **Coğrafi tabanlı erişim kontrolü:** Web uygulamalarınız için erişimi, istemcinin IP adresiyle ilişkili ülke koduna göre denetleyebilirsiniz.

- **HTTP parametreleri tabanlı erişim kontrolü:** Kuralları HTTP/HTTPS istek parametrelerinde dize eşleşmeleri üzerine temel alabilirsiniz.  Örneğin, sorgu dizeleri, POST rgs, İstek URI, İstek Üstbilgi ve İstek Gövdesi.

- **İstek yöntemi tabanlı erişim denetimi:** İsteğin HTTP istek yöntemine göre kuralları temel alabilirsiniz. Örneğin, GET, PUT veya HEAD.

- **Boyut kısıtlaması:** Kuralları sorgu dizesi, Uri veya istek gövdesi gibi bir isteğin belirli bölümlerinin uzunluklarına dayandırabilirsiniz.

- **Oran sınırlayıcı kurallar:** Bir oran denetimi kuralı herhangi bir istemci IP anormal yüksek trafik sınırlamaktır. Bir dakikalık süre boyunca istemci IP'den izin verilen web isteklerinin sayısına bir eşik göre yapılandırabilirsiniz. Bu kural, istemci IP'nin tüm isteğine izin veren veya engelleyen IP listesi tabanlı izin verme/engelleme özel kuralından farklıdır. Oran sınırları, tanecikli hız kontrolü için HTTP(S) parametre eşleşmeleri gibi ek eşleşme koşullarıyla birleştirilebilir.

### <a name="azure-managed-rule-sets"></a>Azure tarafından yönetilen kural kümeleri

Azure tarafından yönetilen kural kümeleri, ortak bir güvenlik tehditleri kümesine karşı koruma dağıtmanın kolay bir yolunu sağlar. Bu tür kural kümeleri Azure tarafından yönetildiği için, kurallar yeni saldırı imzalarına karşı korumak için gerektiği gibi güncelleştirilir. Azure tarafından yönetilen Varsayılan Kural Kümesi, aşağıdaki tehdit kategorilerine karşı kurallar içerir:

- Siteler arası betik
- Java saldırıları
- Yerel dosya ekleme
- PHP enjeksiyon atakları
- Uzaktan komut yürütme
- Uzak dosya ekleme
- Oturum fiksasyonu
- SQL ekleme koruması
- Protokol saldırganları

Kural kümesine yeni saldırı imzaları eklendiğinde Varsayılan Kural Kümesi'nin sürüm numarası artışlar.
Varsayılan Kural Kümesi, WAF ilkelerinizde Algılama modunda varsayılan olarak etkinleştirilir. Uygulama gereksinimlerinizi karşılamak için Varsayılan Kural Kümesi'ndeki tek tek kuralları devre dışı bırakabilir veya etkinleştirebilirsiniz. Ayrıca kural başına belirli eylemleri (ALLOW/BLOCK/REDIRECT/LOG) da ayarlayabilirsiniz.

Bazen bir WAF değerlendirmesinden belirli istek özniteliklerini atlamalısınız. Yaygın bir örnek, kimlik doğrulama için kullanılan Active Directory eklenmiş belirteçleridir. Yönetilen bir kural, kural grubu veya tüm kural kümesi için bir dışlama listesi yapılandırabilirsiniz.  

Varsayılan eylem BLOCK'a dır. Ayrıca, Varsayılan Kural Kümesi'nde önceden yapılandırılmış kurallardan herhangi birini atlamak istiyorsanız, özel kurallar aynı WAF ilkesinde yapılandırılabilir.

Varsayılan Kural Kümesi'ndeki kurallar değerlendirilmeden önce özel kurallar her zaman uygulanır. Bir istek özel bir kuralla eşleşirse, ilgili kural eylemi uygulanır. İstek engellenir veya arka uca aktarılır. Varsayılan Kural Kümesi'ndeki başka özel kurallar veya kurallar işlenmez. Varsayılan Kural Kümesini WAF ilkelerinizden de kaldırabilirsiniz.

### <a name="bot-protection-rule-set-preview"></a>Bot koruma kuralı seti (önizleme)

Bilinen bot kategorilerinden gelen istekler üzerinde özel eylemler de almak üzere yönetilen bir bot koruma kuralını etkinleştirebilirsiniz. 

Desteklenen üç bot kategorisi vardır: Kötü, İyi ve Bilinmeyen. Bot imzaları WAF platformu tarafından yönetilir ve dinamik olarak güncellenir.

Kötü botlar kötü niyetli IP adresleri ve kimliklerini tahrif botlar botlar içerir. Kötü amaçlı IP adresleri Microsoft Threat Intelligence akışından elde edilir ve her saat güncellenir. [Akıllı Güvenlik Grafiği](https://www.microsoft.com/security/operations/intelligence) Microsoft Threat Intelligence'a güç sağlar ve Azure Güvenlik Merkezi de dahil olmak üzere birden çok hizmet tarafından kullanılır.

İyi Botlar doğrulanmış arama motorları içerir. Bilinmeyen kategoriler botlar olarak kendilerini tanımlayan ek bot grupları içerir. Örneğin, pazar çözümleyicisi, besleme alıcıları ve veri toplama aracıları. 

Bilinmeyen botlar ek doğrulama olmadan yayınlanan kullanıcı aracıları üzerinden sınıflandırılır. Farklı türde botları engellemek, izin vermek, günlüğe kaydetmek veya yeniden yönlendirmek için özel eylemler ayarlayabilirsiniz.

![Bot Koruma Kural Seti](../media/afds-overview/botprotect2.png)

> [!IMPORTANT]
> Bot koruma kuralı kümesi şu anda genel önizlemededir ve bir önizleme hizmeti düzeyi sözleşmesiyle sağlanır. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.  Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bot koruması etkinse, bot kurallarına uyan gelen istekler FrontdoorWebApplicationFirewallLog günlüğünde günlüğe kaydedilir. WAF günlüklerine bir depolama hesabından, etkinlik merkezinden veya günlük analitiğinden erişebilirsiniz.

## <a name="configuration"></a>Yapılandırma

Azure portalı, REST API'leri, Azure Kaynak Yöneticisi şablonlarını ve Azure PowerShell'i kullanarak tüm WAF kural türlerini yapılandırabilir ve dağıtabilirsiniz.

## <a name="monitoring"></a>İzleme

Front Door'da WAF izleme, uyarıları izlemek ve trafik eğilimlerini kolayca izlemek için Azure Monitor ile entegre edilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Uygulama Ağ Geçidi'nde Web Uygulaması Güvenlik Duvarı](../ag/ag-overview.md) hakkında bilgi edinin