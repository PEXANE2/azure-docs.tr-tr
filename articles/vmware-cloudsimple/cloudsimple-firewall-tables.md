---
title: Azure VMware çözümleri (AVS)-güvenlik duvarı tabloları
description: AVS özel bulut güvenlik duvarı tabloları ve güvenlik duvarı kuralları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 631ccb33e14586edce71752826f134c1e50819c4
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025053"
---
# <a name="firewall-tables-overview"></a>Güvenlik Duvarı tablolarına genel bakış

Bir güvenlik duvarı tablosu, AVS özel bulut kaynaklarına gelen ve giden ağ trafiğini filtrelemek için kuralları listeler. Bir VLAN/subnet 'e güvenlik duvarı tabloları uygulayabilirsiniz. Kurallar, bir kaynak ağ veya IP adresi ile hedef ağ veya IP adresi arasındaki ağ trafiğini denetler.

## <a name="firewall-rules"></a>Güvenlik duvarı kuralları

Aşağıdaki tabloda bir güvenlik duvarı kuralındaki parametreler açıklanmaktadır.

| Özellik | Ayrıntılar |
| ---------| --------|
| **Ad** | Güvenlik duvarı kuralını ve amacını benzersiz bir şekilde tanımlayan bir ad. |
| **Öncelik** | 100 ile 4096 arasında bir sayı, 100 en yüksek önceliğe sahip. Kurallar öncelik sırasına göre işlenir. Trafik bir kural eşleşmesi ile karşılaştığında, kural işleme durduruluyor. Sonuç olarak, daha yüksek önceliklere sahip kurallarla aynı özniteliklere sahip düşük önceliklere sahip kurallar işlenmez. Çakışan kurallardan kaçınmak için dikkatli olmanız. |
| **Durum Izleme** | İzleme durum bilgisiz (AVS özel bulutu, Internet veya VPN) ya da durum bilgisi olan (genel IP) olabilir.  |
| **Protokol** | Seçenekler arasında, TCP veya UDP bulunur. ICMP gerekliyse, herhangi birini kullanın. |
| **Yön** | Kuralın gelen veya giden trafiğe uygulanma seçeneği. |
| **Eylem** | Kuralda tanımlanan trafik türüne izin verin veya reddedin. |
| **Kaynak** | Bir IP adresi, sınıfsız etki alanları arası yönlendirme (CıDR) bloğu (örneğin, 10.0.0.0/24) veya herhangi bir IP adresi.  Bir Aralık, hizmet etiketi veya uygulama güvenlik grubu belirtme, daha az güvenlik kuralı oluşturmanıza olanak sağlar. |
| **Kaynak bağlantı noktası** | Ağ trafiğinin kaynaklandığı bağlantı noktası.  Tek bir bağlantı noktası veya bağlantı noktası aralığı belirtebilirsiniz (örneğin, 443 veya 8000-8080). Aralık belirterek oluşturmanız gereken güvenlik kuralı sayısını azaltabilirsiniz. |
| **Hedefine** | Bir IP adresi, sınıfsız etki alanları arası yönlendirme (CıDR) bloğu (örneğin, 10.0.0.0/24) veya herhangi bir IP adresi.  Bir Aralık, hizmet etiketi veya uygulama güvenlik grubu belirtme, daha az güvenlik kuralı oluşturmanıza olanak sağlar.  |
| **Hedef bağlantı noktası** | Ağ trafiğinin akacağını belirten bağlantı noktası.  Tek bir bağlantı noktası veya bağlantı noktası aralığı belirtebilirsiniz (örneğin, 443 veya 8000-8080). Aralık belirterek oluşturmanız gereken güvenlik kuralı sayısını azaltabilirsiniz.|

### <a name="stateless"></a>Durum bilgisi olmayan

Durum bilgisi olmayan bir kural yalnızca tek tek paketlere bakar ve kurala göre filtre uygular. Trafik akışı için ters yönde ek kurallar gerekebilir. Aşağıdaki noktaları arasındaki trafik için durum bilgisi olmayan kurallar kullanın:

* AVS özel bulutlarının alt ağları
* Şirket içi alt ağ ve bir AVS özel bulut alt ağı
* AVS özel bulutlarından Internet trafiği

### <a name="stateful"></a>Durum bilgisi olan

 Durum bilgisi olan bir kural, üzerinden geçen bağlantılardan haberdar olur. Var olan bağlantılar için bir akış kaydı oluşturulur. Akış kaydının bağlantı durumuna göre iletişime izin verilir veya iletişim reddedilir. Trafiği Internet 'ten filtrelemek için genel IP adresleri için bu kural türünü kullanın.

### <a name="default-rules"></a>Varsayılan kurallar

Aşağıdaki varsayılan kurallar her güvenlik duvarı tablosunda oluşturulur.

|Öncelik|Ad|Durum Izleme|Yön|Trafik türü|Protokol|Kaynak|Kaynak Bağlantı Noktası|Hedef|Hedef Bağlantı Noktası|Eylem|
|--------|----|--------------|---------|------------|--------|------|-----------|-----------|----------------|------|
|65000|Tüm-Internet 'e izin ver|Durum bilgisi olan|Giden|Genel IP veya internet trafiği|Tümü|Herhangi biri|Herhangi biri|Herhangi biri|Herhangi biri|Allow|
|65001|Reddet-tümü-internet 'ten|Durum bilgisi olan|Gelen|Genel IP veya internet trafiği|Tümü|Herhangi biri|Herhangi biri|Herhangi biri|Herhangi biri|Deny|
|65002|tümünü-intranete izin ver|Durum bilgisi olmayan|Giden|AVS özel bulutu iç veya VPN trafiği|Tümü|Herhangi biri|Herhangi biri|Herhangi biri|Herhangi biri|Allow|
|65003|-All-ıntranetden izin ver|Durum bilgisi olmayan|Gelen|AVS özel bulutu iç veya VPN trafiği|Tümü|Herhangi biri|Herhangi biri|Herhangi biri|Herhangi biri|Allow|

## <a name="next-steps"></a>Sonraki adımlar

* [Güvenlik Duvarı tablolarını ve kurallarını ayarlama](firewall.md)
