---
title: Azure Güvenlik Duvarı kural işleme mantığı
description: Azure Güvenlik duvarında NAT kuralları, ağ kuralları ve uygulamalar kuralları vardır. Kurallar kural türüne göre işlenir.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 04/10/2020
ms.author: victorh
ms.openlocfilehash: 93677b3e473ab825665fed5590ac345a8cfcc300
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81113443"
---
# <a name="azure-firewall-rule-processing-logic"></a>Azure Güvenlik Duvarı kural işleme mantığı
Azure Güvenlik duvarında NAT kurallarını, ağ kurallarını ve uygulama kurallarını yapılandırabilirsiniz. Kural koleksiyonları, kural türüne göre öncelik sırasına göre işlenir ve 100 ' den 65.000 ' ye kadar daha yüksek sayılara daha düşük sayılar. Bir kural koleksiyonu adı yalnızca harf, sayı, alt çizgi, nokta veya kısa çizgi içerebilir. Bir harf veya sayı ile başlamalı ve bir harf, sayı veya alt çizgi ile bitmelidir. En büyük ad uzunluğu 80 karakterdir.

İlk olarak kural koleksiyonu öncelik numaralarınızı 100 artışlara (100, 200, 300, vb.) göre boşluk, gerekirse daha fazla kural koleksiyonu eklemek için yeriniz olması gerekir.

> [!NOTE]
> Tehdit zekası tabanlı filtreleme 'yi etkinleştirirseniz, bu kurallar en yüksek önceliktir ve ilk olarak her zaman işlenir. Tehdit bilgileri filtreleme, yapılandırılmış kuralların işlenmeden önce trafiği reddedebilir. Daha fazla bilgi için bkz. [Azure Güvenlik Duvarı tehdit zekası tabanlı filtreleme](threat-intel.md).

## <a name="outbound-connectivity"></a>Giden bağlantı

### <a name="network-rules-and-applications-rules"></a>Ağ kuralları ve uygulama kuralları

Ağ kurallarını ve uygulama kurallarını yapılandırırsanız, ağ kuralları uygulama kurallarından önce öncelik sırasına göre uygulanır. Kurallar sonlandırılıyor. Bu nedenle bir ağ kuralında eşleşme bulunursa, başka hiçbir kural işlenmez.  Ağ kuralı eşleşmesi yoksa ve protokol HTTP, HTTPS veya MSSQL ise, paket daha sonra öncelik sırasıyla uygulama kuralları tarafından değerlendirilir. Hala eşleşme bulunamazsa paket, [altyapı kuralı koleksiyonuna](infrastructure-fqdns.md)göre değerlendirilir. Ardından hala eşleşme yoksa paket varsayılan olarak reddedilir.

## <a name="inbound-connectivity"></a>Gelen bağlantı

### <a name="nat-rules"></a>NAT kuralları

Gelen Internet bağlantısı [, öğretici: Azure Güvenlik Duvarı ile gelen trafiği Azure Portal kullanarak](tutorial-firewall-dnat.md)hedef ağ adresi çevirisi (DNAT) yapılandırılarak etkinleştirilebilir. NAT kuralları ağ kurallarından önce öncelik olarak uygulanır. Bir eşleşme bulunursa, çevrilmiş trafiğe izin veren örtülü olarak karşılık gelen bir ağ kuralı eklenir. Bu davranışı, çevrilen trafikle eşleşen reddetme kuralları olan bir ağ kural koleksiyonunu açıkça ekleyerek geçersiz kılabilirsiniz.

Uygulama kuralları gelen bağlantılar için uygulanmıyor. Bu nedenle, gelen HTTP/S trafiğini filtrelemek istiyorsanız, Web uygulaması güvenlik duvarını (WAF) kullanmanız gerekir. Daha fazla bilgi için bkz. [Azure Web uygulaması güvenlik duvarı nedir?](../web-application-firewall/overview.md)

## <a name="examples"></a>Örnekler

Aşağıdaki örneklerde, bu kural birleşimlerinin bazılarının sonuçları gösterilmektedir.

### <a name="example-1"></a>Örnek 1

Eşleşen bir ağ kuralı nedeniyle google.com bağlantısına izin verilir.

**Ağ kuralı**

- Eylem: İzin Ver


|name  |Protokol  |Kaynak türü  |Kaynak  |Hedef türü  |Hedef adres  |Hedef bağlantı noktaları|
|---------|---------|---------|---------|----------|----------|--------|
|Web 'e izin ver     |TCP|IP adresi|*|IP adresi|*|80,443

**Uygulama kuralı**

- Eylem: reddetme

|name  |Kaynak türü  |Kaynak  |Protokol: bağlantı noktası|Hedef FQDN 'Ler|
|---------|---------|---------|---------|----------|----------|
|Reddet-Google     |IP adresi|*|http: 80, https: 443|google.com

**Sonuç**

Paket *Allow-Web* Network kuralıyla eşleştiğinden, Google.com bağlantısına izin verilir. Kural işleme bu noktada durdu.

### <a name="example-2"></a>Örnek 2

Daha *yüksek öncelikli bir* ağ kuralı koleksiyonu ENGELLEDIĞI için SSH trafiği reddedildi.

**Ağ kuralı koleksiyonu 1**

- Ad: Izin ver-koleksiyon
- Öncelik: 200
- Eylem: İzin Ver

|name  |Protokol  |Kaynak türü  |Kaynak  |Hedef türü  |Hedef adres  |Hedef bağlantı noktaları|
|---------|---------|---------|---------|----------|----------|--------|
|SSH 'ye izin ver     |TCP|IP adresi|*|IP adresi|*|22

**Ağ kuralı koleksiyonu 2**

- Ad: reddetme-koleksiyon
- Öncelik: 100
- Eylem: reddetme

|name  |Protokol  |Kaynak türü  |Kaynak  |Hedef türü  |Hedef adres  |Hedef bağlantı noktaları|
|---------|---------|---------|---------|----------|----------|--------|
|Deny-SSH     |TCP|IP adresi|*|IP adresi|*|22

**Sonuç**

Daha yüksek öncelikli bir ağ kuralı koleksiyonu tarafından engellediği için SSH bağlantıları reddedilir. Kural işleme bu noktada durdu.

## <a name="rule-changes"></a>Kural değişiklikleri

Önceden izin verilen trafiğe izin vermek için bir kural değiştirirseniz, varsa ilgili mevcut oturumlar bırakılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik duvarını dağıtmayı ve yapılandırmayı](tutorial-firewall-deploy-portal.md)öğrenin.