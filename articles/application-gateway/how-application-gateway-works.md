---
title: Uygulama ağ geçidi nasıl çalışır?
description: Bu makalede, bir uygulama ağ geçidinin gelen istekleri nasıl kabul edip arka uca yönlendiriri hakkında bilgi verilmektedir.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 11/16/2019
ms.author: absha
ms.openlocfilehash: 84a7bdfb9f8f7c741140cbe2086149dff90db211
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132979"
---
# <a name="how-an-application-gateway-works"></a>Uygulama ağ geçidi nasıl çalışır?

Bu makalede, bir uygulama ağ geçidinin gelen istekleri nasıl kabul edip arka uca yönlendirir.

![Bir uygulama ağ geçidi isteği nasıl kabul eder?](./media/how-application-gateway-works/how-application-gateway-works.png)

## <a name="how-an-application-gateway-accepts-a-request"></a>Bir uygulama ağ geçidi isteği nasıl kabul eder?

1. İstemci bir uygulama ağ geçidine istek göndermeden önce, bir Etki Alanı Adı Sistemi (DNS) sunucusu kullanarak uygulama ağ geçidinin etki alanı adını çözer. Azure, tüm uygulama ağ geçitleri azure.com etki alanında olduğundan DNS girişini denetler.

2. Azure DNS, IP adresini uygulama ağ geçidinin ön uç IP adresi olan istemciye döndürür.

3. Uygulama ağ geçidi, bir veya daha fazla dinleyiciye gelen trafiği kabul eder. Dinleyici, bağlantı isteklerini denetleyen mantıksal bir varlıktır. İstemlerden uygulama ağ geçidine bağlantılar için bir ön uç IP adresi, protokol ve bağlantı noktası numarasıyla yapılandırılır.

4. Bir web uygulaması güvenlik duvarı (WAF) kullanılıyorsa, uygulama ağ geçidi istek üstbilgisini ve varsa gövdeyi WAF kurallarına göre denetler. Bu eylem, isteğin geçerli istek mi yoksa bir güvenlik tehdidi mi olduğunu belirler. İstek geçerliyse, arka uca yönlendirilir. İstek geçerli değilse ve WAF Önleme modundaysa, güvenlik tehdidi olarak engellenir. Algılama modundaysa, istek değerlendirilir ve günlüğe kaydedilir, ancak yine de arka uç sunucusuna iletilir.

Azure Uygulama Ağ Geçidi dahili uygulama yük dengeleyicisi veya internete bakan uygulama yük dengeleyicisi olarak kullanılabilir. Internet'e bakan bir uygulama ağ geçidi ortak IP adreslerini kullanır. Internet'e bakan bir uygulama ağ geçidinin DNS adı genel IP adresine genel olarak çözülebilir. Sonuç olarak, internete bakan uygulama ağ geçitleri istemci isteklerini internete yönlendirebilir.

Dahili uygulama ağ geçitleri yalnızca özel IP adreslerini kullanır. Özel veya [Özel DNS bölgesi](https://docs.microsoft.com/azure/dns/private-dns-overview)kullanıyorsanız, alan adı Uygulama Ağ Geçidi'nin özel IP adresine dahili olarak çözülebilir olmalıdır. Bu nedenle, dahili yük dengeleyiciler yalnızca uygulama ağ geçidi için sanal ağa erişimi olan istemcilerden gelen istekleri yönlendirebilir.

## <a name="how-an-application-gateway-routes-a-request"></a>Bir uygulama ağ geçidi isteği nasıl yönlendirir?

Bir istek geçerliyse ve WAF tarafından engellenmiyorsa, uygulama ağ geçidi dinleyiciyle ilişkili istek yönlendirme kuralını değerlendirir. Bu eylem, isteği yönlendirmek için hangi arka uç havuzunu belirler.

İstek yönlendirme kuralına bağlı olarak, uygulama ağ geçidi dinleyicideki tüm istekleri belirli bir arka uç havuzuna yönlendirecek, istekleri URL yolunu temel alan farklı arka uç havuzlarına yönlendirecek veya istekleri başka bir bağlantı noktasına veya harici siteye yönlendirecek şekilde belirler.
>[!NOTE]
>Kurallar v1 SKU için portalda listelenen sırayla işlenir. 

Uygulama ağ geçidi arka uç havuzunu seçtiğinde, isteği havuzdaki sağlıklı arka uç sunucularından birine (y.y.y.y) gönderir. Sunucunun durumu bir sistem durumu sondası tarafından belirlenir. Arka uç havuzu birden çok sunucu içeriyorsa, uygulama ağ geçidi istekleri sağlıklı sunucular arasında yönlendirmek için bir round-robin algoritması kullanır. Bu yük, sunucudaki istekleri dengeler.

Uygulama ağ geçidi arka uç sunucusunu belirledikten sonra, HTTP ayarlarını temel alan arka uç sunucusuyla yeni bir TCP oturumu açar. HTTP ayarları, arka uç sunucusuyla yeni bir oturum oluşturmak için gereken protokolü, bağlantı noktasını ve yönlendirmeyle ilgili diğer ayarları belirtir.

HTTP ayarlarında kullanılan bağlantı noktası ve protokolü, uygulama ağ geçidi ve arka uç sunucuları arasındaki trafiğin şifrelenip şifrelenmediğini (böylece uçlardan uca TLS'yi mi gerçekleştirdiğini) veya şifrelenmemiş olup olmadığını belirler.

Bir uygulama ağ geçidi özgün isteği arka uç sunucusuna gönderdiğinde, ana bilgisayar adı, yol ve protokolü geçersiz kılmayla ilgili HTTP ayarlarında yapılan özel yapılandırmayı onurlandırar. Bu eylem, çerez tabanlı oturum afinitesini, bağlantı drenajını, arka uçtan ana bilgisayar adı seçimini ve benzeri nidaları korur.

 >[!NOTE]
>Eğer arka uç havuzu:
> - **Genel bir bitiş noktasıdır,** uygulama ağ geçidi sunucuya ulaşmak için ön uç genel IP kullanır. Ön uç genel IP adresi yoksa, giden dış bağlantı için bir atanır.
> - **Dahili olarak çözülebilen bir FQDN veya özel bir IP adresi içerir,** uygulama ağ geçidi isteği örnek özel IP adreslerini kullanarak arka uç sunucusuna yönlendirir.
> - **Harici bir uç nokta veya harici olarak çözülebilen bir FQDN içerir**, uygulama ağ geçidi ön uç ortak IP adresini kullanarak isteği arka uç sunucuya yönlendirir. DNS çözünürlüğü, yapılandırılırsa özel bir DNS bölgesine veya özel DNS sunucusuna dayanır veya varsayılan Azure sağlanan DNS'yi kullanır. Ön uç genel IP adresi yoksa, giden dış bağlantı için bir atanır.

### <a name="modifications-to-the-request"></a>İstekte yapılan değişiklikler

Bir uygulama ağ geçidi, istekleri arka uca iletmeden önce tüm isteklere dört ek üstbilgi ekler. Bu başlıklar x-forwarded-for, x-forwarded-proto, x-forwarded-portve ve x-original-host vardır. X-forwarded-for üstbilginin biçimi, VIRgülle ayrılmış IP:port listesidir.

X-forwarde-proto için geçerli değerler HTTP veya HTTPS'dir. X-forwarded-port, isteğin uygulama ağ geçidine ulaştığı bağlantı noktasını belirtir. X-original-host üstbilgi, isteğin geldiği özgün ana bilgisayar üstbilgisini içerir. Bu üstbilgi, gelen ana bilgisayar üstbilginin trafik arka uca yönlendirilmeden önce değiştirildiği Azure web sitesi tümleştirmesinde yararlıdır. Oturum afinitebir seçenek olarak etkinleştirilmişse, ağ geçidi tarafından yönetilen bir yakınlık çerezi ekler.

UYGULAMA ağ [geçidini, HTTP üstbilgileri Yeniden Yaz'ı](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) kullanarak üstbilgileri değiştirmek veya bir yol geçersiz kılma ayarı kullanarak URI yolunu değiştirmek için yapılandırabilirsiniz. Ancak, bunu yapmak için yapılandırılmadığı sürece, tüm gelen istekler arka uca yakın.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama ağ geçidi bileşenleri hakkında bilgi edinin](application-gateway-components.md)
