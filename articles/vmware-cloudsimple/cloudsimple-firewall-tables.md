---
title: CloudSimple tarafından Azure VMware Çözümü - Güvenlik Duvarı tabloları
description: CloudSimple özel bulut güvenlik duvarı tabloları ve güvenlik duvarı kuralları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 89bef6cef48f2b972aa3f931008b0db84431b832
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025053"
---
# <a name="firewall-tables-overview"></a>Güvenlik duvarı tablolarına genel bakış

Güvenlik duvarı tablosu, Ağ trafiğini Özel Bulut kaynaklarına ve özel bulut kaynaklarına filtrelemek için kuralları listeler. Güvenlik duvarı tablolarını VLAN/alt ağına uygulayabilirsiniz. Kurallar, bir kaynak ağı veya IP adresi ile hedef ağ veya IP adresi arasındaki ağ trafiğini denetler.

## <a name="firewall-rules"></a>Güvenlik duvarı kuralları

Aşağıdaki tabloda bir güvenlik duvarı kuralı parametreleri açıklanır.

| Özellik | Ayrıntılar |
| ---------| --------|
| **Adı** | Güvenlik duvarı kuralını ve amacını benzersiz olarak tanımlayan bir ad. |
| **Öncelik** | 100 ile 4096 arasında bir sayı, 100 en yüksek öncelik olmak. Kurallar öncelik sırasına göre işlenir. Trafik bir kural eşleşmesi karşılaştığında, kural işleme durur. Sonuç olarak, daha yüksek önceliklere sahip kurallarla aynı özniteliklere sahip daha düşük önceliklere sahip kurallar işlenmez.  Çakışan kurallardan kaçınmaya özen. |
| **Durum Takibi** | İzleme durum ifadesüz (Özel Bulut, Internet veya VPN) veya stateful (Public IP) olabilir.  |
| **Protokolü** | Seçenekler arasında Any, TCP veya UDP yer alıyor. ICMP'ye ihtiyacınız varsa Any'yi kullanın. |
| **Yön** | Kuralın gelen veya giden trafiğe uygulanma seçeneği. |
| **Eylem** | Kuralda tanımlanan trafik türüne izin verin veya reddedin. |
| **Kaynak** | Bir IP adresi, sınıfsız etki alanları arası yönlendirme (CIDR) bloğu (örneğin, 10.0.0.0/24) veya Herhangi biri.  Bir aralık, hizmet etiketi veya uygulama güvenlik grubu belirtme, daha az güvenlik kuralı oluşturmanıza olanak tanır. |
| **Kaynak Bağlantı Noktası** | Ağ trafiğinin kaynaklandığı bağlantı noktası.  443 veya 8000-8080 gibi tek bir bağlantı noktası veya bağlantı noktası aralığı belirtebilirsiniz. Aralık belirterek oluşturmanız gereken güvenlik kuralı sayısını azaltabilirsiniz. |
| **Hedef** | Bir IP adresi, sınıfsız etki alanları arası yönlendirme (CIDR) bloğu (örneğin, 10.0.0.0/24) veya Herhangi biri.  Bir aralık, hizmet etiketi veya uygulama güvenlik grubu belirtme, daha az güvenlik kuralı oluşturmanıza olanak tanır.  |
| **Hedef Bağlantı Noktası** | Ağ trafiğinin aktığı bağlantı noktası.  443 veya 8000-8080 gibi tek bir bağlantı noktası veya bağlantı noktası aralığı belirtebilirsiniz. Aralık belirterek oluşturmanız gereken güvenlik kuralı sayısını azaltabilirsiniz.|

### <a name="stateless"></a>Durum bilgisi olmayan

Devletsiz bir kural yalnızca tek tek paketlere bakar ve bunları kurala göre filtreler.  
Ters yöndeki trafik akışı için ek kurallar gerekebilir.  Aşağıdaki noktalar arasındaki trafik için durumsuz kuralları kullanın:

* Özel Bulutların Alt Ağları
* Şirket içi alt ağ ve Özel Bulut alt ağı
* Özel Bulutlardan Internet trafiği

### <a name="stateful"></a>Durum bilgisi olan

 Devlet kuralı, içinden geçen bağlantıların farkındadır. Var olan bağlantılar için bir akış kaydı oluşturulur. Akış kaydının bağlantı durumuna göre iletişime izin verilir veya iletişim reddedilir.  Internet trafiğini filtrelemek için ortak IP adresleri için bu kural türünü kullanın.

### <a name="default-rules"></a>Varsayılan kurallar

Her güvenlik duvarı tablosunda aşağıdaki varsayılan kurallar oluşturulur.

|Öncelik|Adı|Durum Takibi|Yön|Trafik Türü|Protokol|Kaynak|Kaynak Bağlantı Noktası|Hedef|Hedef Bağlantı Noktası|Eylem|
|--------|----|--------------|---------|------------|--------|------|-----------|-----------|----------------|------|
|65000|izin-all-to-internet|Durum bilgisi olan|Giden|Genel IP veya internet trafiği|Tümü|Herhangi biri|Herhangi biri|Herhangi biri|Herhangi biri|İzin Ver|
|65001|deny-all-from-internet|Durum bilgisi olan|Gelen|Genel IP veya internet trafiği|Tümü|Herhangi biri|Herhangi biri|Herhangi biri|Herhangi biri|Reddet|
|65002|izin-all-to-intranet|Durum bilgisi olmayan|Giden|Özel Bulut dahili veya VPN trafiği|Tümü|Herhangi biri|Herhangi biri|Herhangi biri|Herhangi biri|İzin Ver|
|65003|intranet'ten izin|Durum bilgisi olmayan|Gelen|Özel Bulut dahili veya VPN trafiği|Tümü|Herhangi biri|Herhangi biri|Herhangi biri|Herhangi biri|İzin Ver|

## <a name="next-steps"></a>Sonraki adımlar

* [Güvenlik duvarı tablolarını ve kurallarını ayarlama](firewall.md)
