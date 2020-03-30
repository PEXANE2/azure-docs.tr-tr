---
title: Azure Güvenlik Duvarı kural işleme mantığı
description: Azure Güvenlik Duvarı'nda NAT kuralları, ağ kuralları ve uygulama kuralları vardır. Kurallar kural türüne göre işlenir.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 03/10/2020
ms.author: victorh
ms.openlocfilehash: d3f8e52b4582c9467ae3ec61ee984771b801fe4f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264784"
---
# <a name="azure-firewall-rule-processing-logic"></a>Azure Güvenlik Duvarı kural işleme mantığı
Azure Güvenlik Duvarı'nda NAT kurallarını, ağ kurallarını ve uygulama kurallarını yapılandırabilirsiniz. Kurallar kural türüne göre işlenir. 

> [!NOTE]
> Tehdit istihbaratı tabanlı filtrelemeni etkinleştiriyorsanız, bu kurallar en yüksek önceliğe kadar dır ve her zaman önce işlenir. Tehdit-istihbarat filtreleme, yapılandırılan kurallar işlenmeden önce trafiği reddedebilir. Daha fazla bilgi için Azure [Güvenlik Duvarı tehdit istihbaratı tabanlı filtreleme](threat-intel.md)ye bakın.

## <a name="outbound"></a>Giden

### <a name="network-rules-and-applications-rules"></a>Ağ kuralları ve uygulama kuralları

Ağ kurallarını ve uygulama kurallarını yapılandırırsanız, ağ kuralları uygulama kurallarından önce öncelik sırasına göre uygulanır. Kurallar sona eriyor. Bu nedenle, bir eşleşme bir ağ kuralında bulunursa, başka kural işlenmez.  Ağ kuralı eşleşmesi yoksa ve protokol HTTP, HTTPS veya MSSQL ise, paket uygulama kuralları tarafından öncelik sırasına göre değerlendirilir. Hala eşleşme bulunamazsa, paket altyapı kuralı [koleksiyonuna](infrastructure-fqdns.md)göre değerlendirilir. Ardından hala eşleşme yoksa paket varsayılan olarak reddedilir.

## <a name="inbound"></a>Gelen

### <a name="nat-rules"></a>NAT kuralları

Gelen Internet bağlantısı, Öğretici'de açıklandığı gibi Hedef Ağ Adresi Çevirisi 'ni (DNAT) yapılandırarak [etkinleştirilebilir: Azure portalını kullanarak Azure Güvenlik Duvarı DNAT ile gelen trafiği filtreleyin.](tutorial-firewall-dnat.md) NAT kuralları ağ kurallarından önce öncelik olarak uygulanır. Bir eşleşme bulunursa, çevrilen trafiğe izin vermek için örtük karşılık gelen bir ağ kuralı eklenir. Bu davranışı, çevrilen trafikle eşleşen reddetme kuralları olan bir ağ kural koleksiyonunu açıkça ekleyerek geçersiz kılabilirsiniz.

Gelen bağlantılar için uygulama kuralları uygulanmaz. Bu nedenle, gelen HTTP/S trafiğini filtrelemek istiyorsanız, Web Uygulaması Güvenlik Duvarı 'nı (WAF) kullanmalısınız. Daha fazla bilgi için Azure [Web Uygulaması Güvenlik Duvarı nedir?](../web-application-firewall/overview.md)

## <a name="examples"></a>Örnekler

Aşağıdaki örnekler, bu kural kombinasyonlarından bazılarının sonuçlarını gösterir.

### <a name="example-1"></a>Örnek 1

Eşleşen bir ağ kuralı nedeniyle google.com bağlantısına izin verilir.

**Ağ kuralı**

- Eylem: İzin Ver


|ad  |Protokol  |Kaynak türü  |Kaynak  |Hedef türü  |Hedef adresi  |Hedef bağlantı noktaları|
|---------|---------|---------|---------|----------|----------|--------|
|İzin ver-web     |TCP|IP adresi|*|IP adresi|*|80,443

**Uygulama kuralı**

- Eylem: Reddet

|ad  |Kaynak türü  |Kaynak  |Protokol:Bağlantı Noktası|Hedef FQDN'ler|
|---------|---------|---------|---------|----------|----------|
|Reddet-google     |IP adresi|*|http:80,https:443|google.com

**Sonuç**

Paket, *Web Ağına İzin Ver* kuralıyla eşleştiğinden google.com bağlantıya izin verilir. Kural işleme bu noktada durur.

### <a name="example-2"></a>Örnek 2

Daha yüksek öncelikli Bir Ağ *Kuralı* toplama bloklar çünkü SSH trafiği reddedilir.

**Ağ kuralı toplama 1**

- Adı: İzin-toplama
- Öncelik: 200
- Eylem: İzin Ver

|ad  |Protokol  |Kaynak türü  |Kaynak  |Hedef türü  |Hedef adresi  |Hedef bağlantı noktaları|
|---------|---------|---------|---------|----------|----------|--------|
|İzin Ver-SSH     |TCP|IP adresi|*|IP adresi|*|22

**Ağ kuralı toplama 2**

- Adı: Deny-collection
- Öncelik: 100
- Eylem: Reddet

|ad  |Protokol  |Kaynak türü  |Kaynak  |Hedef türü  |Hedef adresi  |Hedef bağlantı noktaları|
|---------|---------|---------|---------|----------|----------|--------|
|İnkar-SSH     |TCP|IP adresi|*|IP adresi|*|22

**Sonuç**

Daha yüksek öncelikli ağ kuralı toplama onu engellediği için SSH bağlantıları reddedilir. Kural işleme bu noktada durur.

## <a name="rule-changes"></a>Kural değişiklikleri

Daha önce izin verilen trafiği reddetmek için bir kuralı değiştirirseniz, ilgili varolan oturumlar bırakılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Duvarı'nı nasıl dağıtıp yapılandırılamayı](tutorial-firewall-deploy-portal.md)öğrenin.