---
title: Azure'da ters DNS'ye genel bakış - Azure DNS
description: Bu öğrenme yolunda, Ters DNS'nin nasıl çalıştığını ve Azure'da nasıl kullanılabileceğini öğrenmeye başlayın
services: dns
documentationcenter: na
author: rohinkoul
manager: KumuD
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: rohink
ms.openlocfilehash: bf3da62e989f0e029efdc8e9c70f5f45e0ddd765
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76932308"
---
# <a name="overview-of-reverse-dns-and-support-in-azure"></a>Azure'da ters DNS ve desteğe genel bakış

Bu makalede, ters DNS'nin nasıl çalıştığına ve Azure'da desteklenen ters DNS senaryolarına genel bir bakış sunulur.

## <a name="what-is-reverse-dns"></a>Ters DNS nedir?

Geleneksel DNS kayıtları, Bir DNS adından ('www.contoso.com' gibi) bir IP adresine (64.4.6.100 gibi) eşleme sağlar.  Ters DNS, bir IP adresinin (64.4.6.100) bir ada ('www.contoso.com') geri çevrilmesini sağlar.

Ters DNS kayıtları çeşitli durumlarda kullanılır. Örneğin, ters DNS kayıtları, bir e-posta iletisinin gönderenini doğrulayarak e-posta spam'ıyla mücadelede yaygın olarak kullanılır.  Alıcı posta sunucusu gönderen sunucunun IP adresinin ters DNS kaydını alır ve bu ana bilgisayara kaynak etki alanından e-posta göndermeye yetkili olup olmadığını doğrular. 

## <a name="how-reverse-dns-works"></a>Ters DNS nasıl çalışır?

Ters DNS kayıtları , 'ARPA' bölgeleri olarak bilinen özel DNS bölgelerinde barındırılır.  Bu bölgeler, 'contoso.com' gibi etki alanlarını barındıran normal hiyerarşiye paralel olarak ayrı bir DNS hiyerarşisi oluşturur.

Örneğin, 'www.contoso.com' dns kaydı , 'contoso.com' bölgesinde 'www' adı olan bir DNS 'A' kaydı kullanılarak uygulanır.  Bu A kaydı, bu durumda 64.4.6.100 ilgili IP adresine işaret edilir.  Ters arama, '6.4.64.in-addr.arpa' bölgesinde '100' adlı bir 'PTR' kaydı kullanılarak ayrı ayrı uygulanır (IP adreslerinin ARPA bölgelerinde tersine çevrildiğini unutmayın.)  Bu PTR kaydı, doğru yapılandırılmışsa, 'www.contoso.com' adına işaret edilir.

Bir kuruluşa bir IP adresi bloğu atandığında, ilgili ARPA bölgesini yönetme hakkını da kazanır. Azure tarafından kullanılan IP adres bloklarıyla karşılık gelen ARPA bölgeleri Microsoft tarafından barındırılır ve yönetilir. ISS'niz ARPA bölgesini sizin için kendi IP adresleriniz için barındırabilir veya ARPA bölgesini Azure DNS gibi seçtiğiniz bir DNS hizmetinde barındırmanıza izin verebilir.

> [!NOTE]
> İleri DNS aramaları ve ters DNS aramaları ayrı, paralel DNS hiyerarşilerinde uygulanır. 'www.contoso.com' için ters arama bölge 'contoso.com' **barındırılan değil,** daha ziyade ilgili IP adresi bloğu için ARPA bölgesinde barındırılan. IPv4 ve IPv6 adres blokları için ayrı bölgeler kullanılır.

### <a name="ipv4"></a>IPv4

IPv4 ters arama bölgesinin adı aşağıdaki biçimde `<IPv4 network prefix in reverse order>.in-addr.arpa`olmalıdır: .

Örneğin, 192.0.2.0/24 önekinde bulunan IP'leri barındıracak kayıtları barındıracak bir ters bölge oluştururken, adresin ağ öneki (192.0.2) yalıtılarak ve sırayı (2,0.192) tersine çevirerek ve sonek `.in-addr.arpa`eklenerek bölge adı oluşturulur.

|Subnet sınıfı|Ağ öneki  |Ters ağ öneki  |Standart sonek  |Ters bölge adı |
|-------|----------------|------------|-----------------|---------------------------|
|A sınıfı|203.0.0.0/8     | 203        | .in-addr.arpa   | `203.in-addr.arpa`        |
|B sınıfı|198.51.0.0/16   | 51.198     | .in-addr.arpa   | `51.198.in-addr.arpa`     |
|C sınıfı|192.0.2.0/24    | 2.0.192    | .in-addr.arpa   | `2.0.192.in-addr.arpa`    |

### <a name="classless-ipv4-delegation"></a>Sınıfsız IPv4 delegasyonu

Bazı durumlarda, bir kuruluşa ayrılan IP aralığı C (/24) sınıfı aralığından daha küçüktür. Bu durumda, IP aralığı `.in-addr.arpa` bölge hiyerarşisi içinde bir bölge sınırına düşmez ve bu nedenle alt bölge olarak devredilemez.

Bunun yerine, tek tek ters arama (PTR) kayıtlarının denetimini özel bir DNS bölgesine aktarmak için farklı bir mekanizma kullanılır. Bu mekanizma, her IP aralığı için bir alt bölge delegesi, ardından aralıktaki her IP adresini CNAME kayıtlarını kullanarak bu alt bölgeye ayrı ayrı eşler.

Örneğin, bir kuruluşa ISS tarafından 192.0.2.128/26 IP aralığının verildiğini varsayalım. Bu, 192.0.2.128 ile 192.0.2.191 arasında 64 IP adresini temsil eder. Bu aralık için ters DNS aşağıdaki gibi uygulanır:
- Kuruluş, 128-26.2.0.192.in-addr.arpa adı verilen ters arama bölgesi oluşturur. '128-26' öneki, C (/24) sınıfı aralığında kuruluşa atanan ağ kesimini temsil eder.
- ISS, C sınıfı üst bölgeden yukarıdaki bölge için DNS delegasyonuna ayariçin NS kayıtları oluşturur. Ayrıca, IP aralığındaki her IP adresini kuruluş tarafından oluşturulan yeni bölgeyle eşleyerek üst (C sınıfı) ters arama bölgesinde CNAME kayıtları oluşturur:

```
$ORIGIN 2.0.192.in-addr.arpa
; Delegate child zone
128-26    NS       <name server 1 for 128-26.2.0.192.in-addr.arpa>
128-26    NS       <name server 2 for 128-26.2.0.192.in-addr.arpa>
; CNAME records for each IP address
129       CNAME    129.128-26.2.0.192.in-addr.arpa
130       CNAME    130.128-26.2.0.192.in-addr.arpa
131       CNAME    131.128-26.2.0.192.in-addr.arpa
; etc
```
- Kuruluş daha sonra alt bölge içinde tek tek PTR kayıtlarını yönetir.

```
$ORIGIN 128-26.2.0.192.in-addr.arpa
; PTR records for each UIP address. Names match CNAME targets in parent zone
129      PTR    www.contoso.com
130      PTR    mail.contoso.com
131      PTR    partners.contoso.com
; etc
```
'129.2.0.192.in-addr.arpa' adlı bir PTR kaydı için IP adresi '192.0.2.129' sorguları için ters arama. Bu sorgu, alt bölgesindeki PTR kaydına üst bölgesindeki CNAME üzerinden çözülür.

### <a name="ipv6"></a>IPv6

IPv6 ters arama bölgesinin adı aşağıdaki formda olmalıdır:`<IPv6 network prefix in reverse order>.ip6.arpa`

Örneğin, . 2001:db8:1000:abdc::/64 önekindeki IP'li ana bilgisayarlar için kayıtları barındıracak bir ters bölge oluştururken, adresin ağ öneki (2001:db8:abdc::)) yalıtılarak bölge adı oluşturulur. Daha sonra iPv6 adresi öneki (2001:0db8:abdc:0000::) kısaltmak için kullanılan [sıfır sıkıştırma](https://technet.microsoft.com/library/cc781672(v=ws.10).aspx)kaldırmak için IPv6 ağ öneki genişletin. Ters ağ öneki (`0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2`) oluşturmak ve soneki `.ip6.arpa`eklemek için önekiher hexadecimal sayı arasında delimiter olarak bir dönem kullanarak, sırayı tersine çevirin.


|Ağ öneki  |Genişletilmiş ve ters çevrilmiş ağ öneki |Standart sonek |Ters bölge adı  |
|---------|---------|---------|---------|
|2001:db8:abdc::/64    | 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2        | .ip6.arpa        | `0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa`       |
|2001:db8:1000:9102::/64    | 2.0.1.9.0.0.0.0.1.8.b.d.0.1.0.0.2        | .ip6.arpa        | `2.0.1.9.0.0.0.1.8.b.d.0.1.0.0.2.ip6.arpa`        |


## <a name="azure-support-for-reverse-dns"></a>Ters DNS için Azure desteği

Azure, ters DNS ile ilgili iki ayrı senaryoyu destekler:

**IP adresi bloğuna karşılık gelen ters arama bölgesini barındırma.**
Azure DNS, hem IPv4 hem de IPv6 [için ters arama bölgelerinizi barındırmak ve her ters DNS araması için PTR kayıtlarını yönetmek](dns-reverse-dns-hosting.md)için kullanılabilir.  Ters arama (ARPA) bölgesi oluşturma, delegasyonu ayarlama ve PTR kayıtlarını yapılandırma işlemi normal DNS bölgeleri için aynıdır.  Tek fark, delegasyonun DNS kayıt şirketiniz yerine ISS'niz aracılığıyla yapılandırılması ve yalnızca PTR kayıt türünün kullanılmasıdır.

**Azure hizmetinize atanan IP adresi için ters DNS kaydını yapılandırın.** Azure, Azure [hizmetinize tahsis edilen IP adreslerinin ters aramasını yapılandırmanızı](dns-reverse-dns-for-azure-services.md)sağlar.  Bu ters arama, Azure tarafından ilgili ARPA bölgesinde PTR kaydı olarak yapılandırılır.  Azure tarafından kullanılan tüm IP aralıklarına karşılık gelen bu ARPA bölgeleri Microsoft tarafından barındırılır

## <a name="next-steps"></a>Sonraki adımlar

Ters DNS hakkında daha fazla bilgi [için, Vikipedi'de ters DNS arama](https://en.wikipedia.org/wiki/Reverse_DNS_lookup)bakın.
<br>
[Azure DNS'de ISS'ye atanmış IP aralığınızın ters arama bölgesini nasıl barındırışla düzenleyeceklerinizi](dns-reverse-dns-for-azure-services.md)öğrenin.
<br>
[Azure hizmetleriniz için ters DNS kayıtlarını nasıl yöneteceğinizi](dns-reverse-dns-for-azure-services.md)öğrenin.

