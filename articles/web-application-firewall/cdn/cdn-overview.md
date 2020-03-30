---
title: Azure CDN'deki Azure web uygulaması güvenlik duvarı nedir?
description: Azure CDN hizmetindeki Azure web uygulaması güvenlik duvarının web uygulamalarınızı kötü amaçlı saldırılara karşı nasıl koruduğunun öğren.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: overview
ms.date: 03/18/2020
ms.author: victorh
ms.openlocfilehash: 559ce4d1500dc5f1b67a3b3dbdbeb5d8dda7e487
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79485856"
---
# <a name="azure-web-application-firewall-on-azure-content-delivery-network"></a>Azure İçerik Dağıtım Ağında Azure Web Uygulaması Güvenlik Duvarı

Azure İçerik Dağıtım Ağı'ndaki (CDN) Azure Web Uygulaması Güvenlik Duvarı (WAF), web içeriğiniz için merkezi koruma sağlar. WAF, web hizmetlerinizi sık karşılaşılan açıklara ve güvenlik açıklarına karşı korur. Hizmetinizi kullanıcılarınız için yüksek oranda kullanılabilir tutar ve uyumluluk gereksinimlerini karşılamanıza yardımcı olur.

> [!IMPORTANT]
> Azure CDN'deki WAF şu anda genel önizlemededir ve bir önizleme hizmeti düzeyi sözleşmesiyle sağlanır. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.  Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure CDN'deki WAF, küresel ve merkezi bir çözümdür. Dünya çapındaki Azure ağ kenarı konumlarında dağıtılır. WAF, kötü amaçlı saldırıları kaynağınıza ulaşmadan önce saldırı kaynaklarına yakın durdurur. Performanstan ödün vermeden ölçekte küresel koruma elde elabilirsiniz. 

WAF ilkesi, aboneliğinizdeki herhangi bir CDN bitiş noktasına kolayca bağlanır. Yeni kurallar birkaç dakika içinde dağıtılabilir, böylece değişen tehdit kalıplarına hızlı bir şekilde yanıt verebilirsiniz.

![Azure web uygulaması güvenlik duvarı](../media/cdn-overview/waf-cdn-overview.png)

## <a name="waf-policy-and-rules"></a>WAF politikası ve kuralları

Bir WAF ilkesini yapılandırabilir ve bu ilkeyi koruma için bir veya daha fazla CDN uç noktasıyla ilişkilendirebilirsiniz. WAF ilkesi iki tür güvenlik kuralından oluşur:

- oluşturabileceğiniz özel kurallar.

- Azure yönetilen önceden yapılandırılmış kuralların bir koleksiyonu olan yönetilen kural kümeleri.

Her ikisi de mevcut olduğunda, yönetilen bir kural kümesinde kuralları işlemeden önce özel kurallar işlenir. Bir kural bir eşleşme koşulu, bir öncelik ve bir eylem yapılır. Desteklenen eylem türleri şunlardır: *ALLOW*, *BLOCK*, *LOG*, ve *YÖNLENDIRME*. Yönetilen ve özel kuralları birleştirerek özel uygulama koruma gereksinimlerinizi karşılayan tamamen özelleştirilmiş bir ilke oluşturabilirsiniz.

İlke içindeki kurallar öncelikli sırada işlenir. Öncelik, işlemek için kuralların sırasını tanımlayan benzersiz bir sayıdır. Daha küçük sayılar daha yüksek bir önceliktir ve bu kurallar daha büyük bir değere sahip kurallardan önce değerlendirilir. Bir kural eşleştikten sonra, kuralda tanımlanan ilgili eylem isteğe uygulanır. Böyle bir eşleşme işlendikten sonra, daha düşük önceliklere sahip kurallar daha fazla işlenmez.

Azure CDN'de barındırılan bir web uygulamasının aynı anda yalnızca bir WAF ilkesi olabilir. Ancak, onunla ilişkili herhangi bir WAF ilkeleri olmadan bir CDN bitiş noktası olabilir. Bir WAF ilkesi varsa, dünya çapında tutarlı güvenlik politikaları sağlamak için tüm kenar konumlarımızda çoğaltılır.

## <a name="waf-modes"></a>WAF modları

WAF ilkesi aşağıdaki iki modda çalışacak şekilde yapılandırılabilir:

- *Algılama modu*: Algılama modunda çalıştırıldığında, WAF, isteği ve eşleşen WAF kuralını WAF günlüklerine izlemek ve günlükler dışında başka bir eylemde bulunmaz. Ön Kapı için günlük tanılama açabilirsiniz. Portalı kullandığınızda **Tanılama** bölümüne gidin.

- *Önleme modu*: Önleme modunda, WAF bir istek bir kuralla eşleşirse belirtilen eylemi alır. Bir eşleşme bulunursa, daha düşük önceliğe sahip başka kural değerlendirilmez. Eşleşen istekler WAF günlüklerinde de günlüğe kaydedilir.

## <a name="waf-actions"></a>WAF eylemleri

Bir istek bir kuralın koşullarına uyan aşağıdaki eylemlerden birini seçebilirsiniz:

- *İzin Ver*: İstek WAF'dan geçer ve arka uca iletilir. Daha düşük öncelik kuralları bu isteği engelleyebilir.
- *Blok*: İstek engellenir ve WAF isteği arka uca iletmeden istemciye bir yanıt gönderir.
- *Günlük*: İstek WAF günlüklerinde günlüğe kaydedilir ve WAF daha düşük öncelik kurallarını değerlendirmeye devam eder.
- *Yönlendirme*: WAF isteği belirtilen URI'ye yönlendirir. Belirtilen URI bir ilke düzeyi ayarıdır. Yapılandırıldıktan sonra, *Yönlendirme* eylemiyle eşleşen tüm istekler bu URI'ye gönderilir.

## <a name="waf-rules"></a>WAF kuralları

WAF ilkesi iki tür güvenlik kuralından oluşabilir:

- *özel kurallar*: kendinizi oluşturduğunuz kurallar 
- *yönetilen kural kümeleri*: Azure yönetilen önceden yapılandırılmış kurallar kümesi

### <a name="custom-rules"></a>Özel kurallar

Özel kurallar, eşmaç kuralları ve oran denetim kuralları olabilir.

Aşağıdaki özel eşmatch kurallarını yapılandırabilirsiniz:

- *IP izin listesi ve blok listesi*: İstemci IP adresleri veya IP adresi aralıklarının listesini temel alınabilen web uygulamalarınız için erişimi kontrol edebilirsiniz. Hem IPv4 hem de IPv6 adres türleri desteklenir. Bu liste, kaynak IP'nin listedeki bir IP ile eşleştiği bu istekleri engelleyecek veya izin verecek şekilde yapılandırılabilir.

- *Coğrafi tabanlı erişim denetimi*: Web uygulamalarınız için erişimi, müşterinin IP adresiyle ilişkili ülke koduna göre denetleyebilirsiniz.

- *HTTP parametrelere dayalı erişim denetimi*: HTTP/HTTPS istek parametrelerinde dize eşleşmelerine kurallar dayandırabilirsiniz.  Örneğin, sorgu dizeleri, POST rgs, İstek URI, İstek Üstbilgi ve İstek Gövdesi.

- *İstek yöntemi tabanlı erişim denetimi*: İstek http istek yöntemine kuralları temel edirsiniz. Örneğin, GET, PUT veya HEAD.

- *Boyut kısıtlaması*: Kuralları sorgu dizesi, Uri veya istek gövdesi gibi bir isteğin belirli bölümlerinin uzunluklarına dayandırabilirsiniz.

Bir oran denetimi kuralı, herhangi bir istemci IP adresinden anormal derecede yüksek trafiği sınırlar.

- *Hız sınırlayıcı kurallar*: Bir dakikalık süre boyunca istemci IP adresinden izin verilen web isteklerinin sayısına bir eşik yapılandırabilirsiniz. Bu kural, istemci IP adresinden gelen tüm isteklere izin veren veya engelleyen IP listesi tabanlı izin verme/engelleme özel kuralından farklıdır. Oran sınırları, tanecikli hız kontrolü için HTTP(S) parametre eşleşmeleri gibi ek eşleşme koşullarıyla birleştirilebilir.

### <a name="azure-managed-rule-sets"></a>Azure tarafından yönetilen kural kümeleri

Azure tarafından yönetilen kural kümeleri, ortak bir güvenlik tehditleri kümesine karşı koruma dağıtmanın kolay bir yolunu sağlar. Bu kural kümeleri Azure tarafından yönetildiği için, kurallar yeni saldırı imzalarına karşı korumak için gerektiği gibi güncelleştirilir. Azure yönetilen Varsayılan Kural Kümesi, aşağıdaki tehdit kategorilerine karşı kurallar içerir:

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
Varsayılan Kural Kümesi, WAF ilkelerinizde *Algılama* modunda varsayılan olarak etkinleştirilir. Uygulama gereksinimlerinizi karşılamak için Varsayılan Kural Kümesi'ndeki tek tek kuralları devre dışı bırakabilir veya etkinleştirebilirsiniz. Ayrıca kural başına belirli eylemleri (ALLOW/BLOCK/REDIRECT/LOG) da ayarlayabilirsiniz. Yönetilen Varsayılan Kural Kümesi için varsayılan eylem *Engelle.*

Varsayılan Kural Kümesi'ndeki kurallar değerlendirilmeden önce özel kurallar her zaman uygulanır. Bir istek özel bir kuralla eşleşirse, ilgili kural eylemi uygulanır. İstek engellenir veya arka uca aktarılır. Varsayılan Kural Kümesi'ndeki başka özel kurallar veya kurallar işlenmez. Varsayılan Kural Kümesini WAF ilkelerinizden de kaldırabilirsiniz.

## <a name="configuration"></a>Yapılandırma

Azure portalı, REST API'leri, Azure Kaynak Yöneticisi şablonlarını ve Azure PowerShell'i kullanarak tüm WAF kural türlerini yapılandırabilir ve dağıtabilirsiniz.

## <a name="monitoring"></a>İzleme

CDN ile WAF izleme, uyarıları izlemek ve trafik eğilimlerini kolayca izlemek için Azure Monitor ile entegre edilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure portalını kullanarak Azure CDN ile BIR WAF ilkesi oluşturun](waf-cdn-create-portal.md)