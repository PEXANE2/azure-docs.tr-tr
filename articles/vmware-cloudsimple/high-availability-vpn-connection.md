---
title: CloudSimple tarafından Azure VMware çözümü-Şirket içinden CloudSimple VPN Gateway 'e yüksek kullanılabilirlik yapılandırın
description: Şirket içi ortamınızdaki yüksek kullanılabilirliğe sahip bir bağlantının yüksek kullanılabilirlik için etkin bir CloudSimple VPN Gateway 'e nasıl yapılandırılacağını açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d08dce95836328f6a0991601951057944fa5f61e
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69536449"
---
# <a name="configure-a-high-availability-connection-from-on-premises-to-cloudsimple-vpn-gateway"></a>Şirket içinden CloudSimple VPN Gateway 'e yüksek kullanılabilirliğe sahip bir bağlantı yapılandırın

Ağ yöneticileri, şirket içi ortamlarından bir CloudSimple VPN Gateway 'e yüksek kullanılabilirliğe sahip bir IPSec siteden siteye VPN bağlantısı yapılandırabilir.

Bu kılavuzda, bir IPSec siteden siteye VPN yüksek kullanılabilirlik bağlantısı için bir şirket içi güvenlik duvarı yapılandırma adımları sunulmaktadır. Ayrıntılı adımlar, şirket içi güvenlik duvarının türüne özeldir. Örnek olarak, bu kılavuzda iki tür güvenlik duvarının adımları sunulmaktadır: Cisco ASA ve Palo Alto ağları.

## <a name="default-configuration-for-cloudsimple-vpn-gateways"></a>CloudSimple VPN ağ geçitleri için varsayılan yapılandırma

Varsayılan olarak, CloudSimple VPN Gateway 'ler, aşağıdaki Aşama 1 ve Aşama 2 öznitelikleriyle birlikte IKEv1 modunda yapılandırılır. Farklı VPN özniteliklerini kullanmak veya ıKEV1 yerine Ikev2 kullanmak istiyorsanız, <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">bir destek isteği açın</a>.

### <a name="phase-1"></a>1\. aşama

| Parametre | Value |
|-----------|-------|
| IKE Sürümü | IKEv1 |
| Şifreleme | AES 256 |
| Karma algoritması| SHA 256 |
| Diffie Hellman grubu (DH grubu) | 1\. |
| Yaşam süresi | 86.400 saniye |
| Veri Boyutu | 4 GB |

### <a name="phase-2"></a>2\. aşama

| Parametre | Value |
|-----------|-------|
| Şifreleme | AES 256 |
| Karma algoritması| SHA 256 |
| Kusursuz Iletme gizliliği grubu (PFS Grubu) | Yok. |
| Yaşam süresi | 28.800 saniye |
| Veri Boyutu | 4 GB |

## <a name="before-you-begin"></a>Başlamadan önce

Şirket içi güvenlik duvarını yapılandırmadan önce aşağıdaki görevleri doldurun.

1. Kuruluşunuzun gerekli düğümleri [satın](create-nodes.md) aldığını ve en az bir Cloudsimple özel bulutu oluşturduğunu doğrulayın.
2. Şirket içi ağınız ve CloudSimple özel bulutunuz arasında [siteden sıteye VPN ağ geçidi yapılandırın](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway) .

## <a name="configure-on-premises-cisco-asa-firewall"></a>Şirket içi Cisco ASA güvenlik duvarını yapılandırma

Bu bölümdeki yönergeler Cisco ASA sürüm 8,4 ve üzeri için geçerlidir. Yapılandırma örneğinde, Cisco Uyarlamalı güvenlik gereci yazılım sürüm 9,10 dağıtılır ve IKEv1 modunda yapılandırılır.

Siteden siteye VPN 'nin çalışması için, şirket içi Cisco ASA VPN Gateway 'in dış arabirimindeki CloudSimple birincil ve ikincil genel IP 'den (eş IP) UDP 500/4500 ve ESP 'ye (IP protokolü 50) izin vermeniz gerekir.

### <a name="1-configure-phase-1-ikev1"></a>1. 1. aşama (IKEv1) yapılandırma

Dış arabirimde Aşama 1 ' i (IKEv1) etkinleştirmek için Cisco ASA güvenlik duvarında aşağıdaki CLı komutunu girin.

```crypto ikev1 enable outside```

### <a name="2-create-an-ikev1-policy"></a>2. IKEv1 ilkesi oluşturma

Karma, kimlik doğrulama, Diffie-Hellman grubu, ömür ve şifreleme için kullanılacak algoritmaları ve yöntemleri tanımlayan bir IKEv1 ilkesi oluşturun.

```
crypto ikev1 policy 1
authentication pre-share
encryption aes-256
hash sha
group 2
lifetime 28800
```

### <a name="3-create-a-tunnel-group"></a>3. Tünel grubu oluşturma

IPSec öznitelikleri altında bir tünel grubu oluşturun. [Siteden sıteye VPN ağ geçidinizi yapılandırırken](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway)AYARLADıĞıNıZ eş IP adresini ve tünel önceden paylaşılan anahtarını yapılandırın.

```
tunnel-group <primary peer ip> type ipsec-l2l
tunnel-group <primary peer ip> ipsec-attributes
ikev1 pre-shared-key *****

tunnel-group <secondary peer ip> type ipsec-l2l
tunnel-group <secondary peer ip> ipsec-attributes
ikev1 pre-shared-key *****
```

### <a name="4-configure-phase-2-ipsec"></a>4. 2. aşama (IPSec) yapılandırma

Aşama 2 ' yi (IPSec) yapılandırmak için, şifrelenecek ve Tünellenen trafiği tanımlayan bir erişim denetim listesi (ACL) oluşturun. Aşağıdaki örnekte, ilgilendiğiniz trafik şirket içi yerel alt ağdan (10.16.1.0/24) özel bulut uzak alt ağına (192.168.0.0/24) kaynaklıdır. Siteler arasında birden çok alt ağ varsa ACL birden çok giriş içerebilir.

Cisco ASA 8,4 ve üzeri sürümlerde, ağlar, alt ağlar, ana bilgisayar IP adresleri veya birden çok nesne için kapsayıcı olarak görev yapan nesneler veya nesne grupları oluşturulabilir. Uzak alt ağların yerel ve bir nesnesi için bir nesne oluşturun ve bunları şifre ACL 'SI ve NAT deyimleri için kullanın.

#### <a name="define-an-on-premises-local-subnet-as-an-object"></a>Şirket içi yerel alt ağı bir nesne olarak tanımlama

```
object network AZ_inside
subnet 10.16.1.0 255.255.255.0
```

#### <a name="define-the-cloudsimple-remote-subnet-as-an-object"></a>CloudSimple uzak alt ağını bir nesne olarak tanımlama

```
object network CS_inside
subnet 192.168.0.0 255.255.255.0
```

#### <a name="configure-an-access-list-for-the-traffic-of-interest"></a>İlgilendiğiniz trafik için bir erişim listesi yapılandırın

```
access-list ipsec-acl extended permit ip object AZ_inside object CS_inside
```

### <a name="5-configure-the-transform-set"></a>5. Dönüştürme kümesini yapılandırma

Anahtar sözcüğünü ```ikev1```içeren dönüştürme kümesini (TS) yapılandırın. TS 'lerde belirtilen şifreleme ve karma öznitelikleri, [CloudSimple VPN Gateway 'ler Için varsayılan yapılandırmada](#default-configuration-for-cloudsimple-vpn-gateways)listelenen parametrelerle eşleşmelidir.

```
crypto ipsec ikev1 transform-set devtest39 esp-aes-256 esp-sha-hmac 
```

### <a name="6-configure-the-crypto-map"></a>6. Şifre eşlemesini yapılandırma

Şu bileşenleri içeren şifre haritasını yapılandırın:

* Eş IP adresi
* İlgilendiğiniz trafiği içeren tanımlı ACL
* Dönüşüm kümesi

```
crypto map mymap 1 set peer <primary peer ip> <secondary peer ip>
crypto map mymap 1 match address ipsec-acl
crypto map mymap 1 set ikev1 transform-set devtest39
```

### <a name="7-apply-the-crypto-map"></a>7. Şifre haritasını uygulama

Dış arabirimde şifre eşlemesini uygulama:

```crypto map mymap interface outside```

### <a name="8-confirm-applicable-nat-rules"></a>8. Geçerli NAT kurallarını Onayla

Aşağıda, kullanılan NAT kuralı verilmiştir. VPN trafiğinin başka bir NAT kuralına tabi olmadığından emin olun.

```nat (inside,outside) source static AZ_inside AZ_inside destination static CS_inside CS_inside```

### <a name="sample-ipsec-site-to-site-vpn-established-output-from-cisco-asa"></a>Cisco ASA 'den konuma, IPSec siteden siteye VPN tarafından belirlenen çıkış

Aşama 1 çıkışı:

![Cisco ASA güvenlik duvarı için Aşama 1 çıkışı](media/ha-vpn-connection-cisco-phase1.png)

2\. aşama çıkışı:

![Cisco ASA güvenlik duvarı için aşama 2 çıkışı](media/ha-vpn-connection-cisco-phase2.png)

## <a name="configure-on-premises-palo-alto-networks-firewall"></a>Şirket içi Palo Alto Networks güvenlik duvarını yapılandırma

Bu bölümdeki yönergeler, Palo Alto Networks sürüm 7,1 ve üzeri için geçerlidir. Bu yapılandırma örneğinde, Palo Alto Networks VM Serisi yazılım sürümü 8.1.0 dağıtılır ve IKEv1 modunda yapılandırılır.

Siteden siteye VPN 'nin çalışması için, şirket içi Palo Alto Networks ağ geçidinin dış arabirimindeki CloudSimple birincil ve ikincil genel IP 'den (eş IP) UDP 500/4500 ve ESP 'ye (IP protokolü 50) izin vermelisiniz.

### <a name="1-create-primary-and-secondary-tunnel-interfaces"></a>1. Birincil ve ikincil tünel arabirimleri oluşturma

Palo Alto güvenlik duvarında oturum açın, **ağ** > **arabirimleri** > **tünel** > **Ekle**' yi seçin, aşağıdaki alanları yapılandırın ve **Tamam**' ı tıklatın.

* Arabirim adı. İlk alan ' Tunnel ' anahtar sözcüğüyle tekrar doldurulur. Bitişik alana, 1 ile 9999 arasında bir sayı girin. Bu arabirim, şirket içi veri merkezi ve özel bulut arasında siteden siteye trafiği taşımak için birincil bir tünel arabirimi olarak kullanılacaktır.
* Açıklamanın. Tünelin amacının kolay tanımlanması için açıklama girin
* Netflow profili. Varsayılan olarak bırakın.
* Kurulumunun. Arabirimi ata: Sanal yönlendirici: **Varsayılan**' ı seçin. 
        Güvenlik bölgesi: Güvenilen LAN trafiği için bölgeyi seçin. Bu örnekte, LAN trafiği için bölgenin adı ' Trust ' olur.
* IPv4. **Ekle** ' ye tıklayın ve ortamınızda, birincil tünel arabirimine atanacak ve tünelleri izlemek için kullanılacak olan, çakışmayan kullanılmamış/32 IP adresini ekleyin.

Bu yapılandırma yüksek kullanılabilirliğe sahip bir VPN için olduğundan, iki tünel arabirimi gereklidir: Bir birincil ve bir ikincil. İkinci tünel arabirimini oluşturmak için önceki adımları tekrarlayın. Farklı bir tünel KIMLIĞI ve kullanılmayan farklı/32 IP adresini seçin.

### <a name="2-set-up-static-routes-for-private-cloud-subnets-to-be-reached-over-the-site-to-site-vpn"></a>2. Siteden siteye VPN üzerinden erişilmesi için özel bulut alt ağlarına yönelik statik yollar ayarlayın

Yönlendiriciler, şirket içi alt ağların CloudSimple özel bulut alt ağlarına ulaşması için gereklidir.

**Ağ**sanalyönlendiricileri > varsayılan statik yollarEkle'yiseçin,aşağıdakialanlarıyapılandırınveTamam'ıtıklatın > . >  > 

* Ada. Yolun amacının kolay tanımlanması için herhangi bir ad girin.
* Hedefine. Şirket içinden S2S tünel arabirimleri üzerinden ulaşılmış olan CloudSimple özel bulut alt ağlarını belirtin
* Arayüz. Açılan menüden 1. adım (Bölüm-2) içinde oluşturulan birincil tünel arabirimini seçin. Bu örnekte, Tunnel. 20 ' dir.
* Sonraki atlama. **Hiçbiri**' ni seçin.
* Yönetici uzaklığı. Varsayılan olarak bırakın.
* Ölçüt. 1 ile 65535 arasında bir değer girin. Bu anahtar, önceki yolu tercih eden yol ile ilgili ikincil tünel arabirimine kıyasla, birincil tünel arabirimine karşılık gelen yolun daha düşük ölçüsünü girmelidir. Tunnel. 20 ' nin, Tunnel için 30 ölçüm değerinin aksine 20 ölçüm değeri vardır. 30, Tunnel. 20 tercih edilir.
* Yol tablosu. Varsayılan olarak bırakın.
* BFD profili. Varsayılan olarak bırakın.
* Yol izleme. İşaretlenmemiş olarak bırakın.

İkincil tünel arabirimi aracılığıyla ikincil/yedekleme yolu olarak kullanılacak özel bulut alt ağlarının başka bir yolunu oluşturmak için önceki adımları tekrarlayın. Bu kez, birincil yol için farklı bir tünel KIMLIĞI ve daha yüksek bir ölçüm seçin.

### <a name="3-define-the-cryptographic-profile"></a>3. Şifreleme profilini tanımlama

IKEv1 Phase 1 ' deki VPN tünellerini ayarlamak için kullanılacak tanımlama, kimlik doğrulama ve şifreleme için protokolleri ve algoritmaları belirten bir şifreleme profili tanımlayın.

**Ağ**Genişletağ > profilleri**Ike şifre**Ekle ' yi seçin, aşağıdaki alanları yapılandırın ve Tamam ' ı tıklatın. >  > 

* Ada. IKE şifreleme profilinin adını girin.
* DH grubu. **Ekle** ' ye tıklayın ve uygun DH grubunu seçin.
* Şifreleme. **Ekle** ' ye tıklayın ve uygun şifreleme yöntemini seçin.
* Yetkilendirmesi. **Ekle** ' ye tıklayın ve uygun kimlik doğrulama yöntemini seçin.
* Anahtar yaşam süresi. Varsayılan olarak bırakın.
* Ikev2 kimlik doğrulaması birden çok. Varsayılan olarak bırakın.

### <a name="4-define-ike-gateways"></a>4. IKE ağ geçitlerini tanımlama

VPN tünelinin her bir ucunda bulunan eşler arasında iletişim kurmak için ıKE ağ geçitlerini tanımlayın.

**Ağ**Genişlet ağprofilleri > Ike ağgeçitleri > Ekle ' yi seçin, aşağıdaki alanları yapılandırın ve Tamam ' ı tıklatın. > 

Genel sekmesi:

* Ada. Birincil CloudSimple VPN eşi ile birlikte kullanılacak olan ıKE ağ geçidinin adını girin.
* Sürüm. **Yalnızca IKEv1 modunu**seçin.
* Adres türü. **IPv4**' ü seçin.
* Arayüz. Herkese açık veya dış arabirimi seçin.
* Yerel IP adresi. Varsayılan olarak bırakın.
* Eş IP adresi türü. **IP**'yi seçin.
* Eş adresi. Birincil CloudSimple VPN eşi IP adresini girin.
* Yetkilendirmesi. **Önceden paylaşılan anahtar**' ı seçin.
* Önceden paylaşılan anahtar/önceden paylaşılan anahtarı onayla. CloudSimple VPN Gateway anahtarıyla eşleşmesi için önceden paylaşılan anahtarı girin.
* Yerel kimlik. Şirket içi Palo Alto güvenlik duvarının genel IP adresini girin.
* Eş kimliği. Birincil CloudSimple VPN eşi IP adresini girin.

Gelişmiş Seçenekler sekmesi:

* Pasif modu etkinleştirin. İşaretlenmemiş olarak bırakın.
* NAT geçişini etkinleştirin. Şirket içi Palo Alto güvenlik duvarı herhangi bir NAT cihazının arkasında değilse, işaretini kaldırın. Aksi takdirde, onay kutusunu seçin.

IKEv1

* Exchange modu. **Ana**öğesini seçin.
* IKE şifre profili. Daha önce oluşturduğunuz ıKE şifre profilini seçin. Parçalanmayı etkinleştir kutusunu işaretlenmemiş olarak bırakın.
* Kullanılmayan eş algılama. Kutuyu işaretlenmemiş olarak bırakın.

İkincil ıKE ağ geçidini oluşturmak için önceki adımları tekrarlayın.

### <a name="5-define-ipsec-crypto-profiles"></a>5. IPSEC şifre profillerini tanımlama

**Ağ**Genişletağ > profilleri**IPSec şifre**Ekle ' yi seçin, aşağıdaki alanları yapılandırın ve Tamam ' ı tıklatın. >  > 

* Ada. IPSec şifre profili için bir ad girin.
* IPsec protokolü. **ESP**'yi seçin.
* Şifreleme. **Ekle** ' ye tıklayın ve uygun şifreleme yöntemini seçin.
* Yetkilendirmesi. **Ekle** ' ye tıklayın ve uygun kimlik doğrulama yöntemini seçin.
* DH grubu. **PFS yok**' u seçin.
* Süre. 30 dakika olarak ayarlayın.
* Etkinleştirebilir. Kutuyu işaretlenmemiş olarak bırakın.

İkinci CloudSimple VPN eşi olarak kullanılacak başka bir IPSec şifre profili oluşturmak için önceki adımları tekrarlayın. Aynı ıPSEC şifre profili, hem birincil hem de ikincil IPSec tünelleri (aşağıdaki yordama bakın) için de kullanılabilir.

### <a name="6-define-monitor-profiles-for-tunnel-monitoring"></a>6. Tünel izleme için izleyici profillerini tanımlama

**Ağ**Genişletağ > profilleri**izleyici**Ekle ' yi seçin, aşağıdaki alanları yapılandırın ve Tamam ' ı tıklatın. >  > 

* Ada. Hata için öngörülü yeniden işlem için tünel izlemesi için kullanılacak Izleyici profilinin adını girin.
* Ön. **Yük devret**' i seçin.
* Aralığında. **3**değerini girin.
* Eşiği. **7**değerini girin.

### <a name="7-set-up-primary-and-secondary-ipsec-tunnels"></a>7. Birincil ve ikincil IPSec tünellerini ayarlayın.

**Ağ**IPSec tünelleriEkle > ' yi seçin, aşağıdaki alanları yapılandırın ve Tamam > ' ı tıklatın.

Genel sekmesi:

* Ada. Birincil CloudSimple VPN eşi ile ilişkilendirilen birincil ıPSEC tüneli için herhangi bir ad girin.
* Tünel arabirimi. Birincil tünel arabirimini seçin.
* Türüyle. Varsayılan olarak bırakın.
* Adres türü. **IPv4**' ü seçin.
* IKE ağ geçidi. Birincil ıKE ağ geçidini seçin.
* IPSec şifre profili. Birincil IPSec profilini seçin. **Gelişmiş seçenekleri göster**' i seçin.
* Yeniden yürütmeyi korumayı etkinleştirin. Varsayılan olarak bırakın.
* TOS üst bilgisini kopyala. Kutuyu işaretlenmemiş olarak bırakın.
* Tünel Izleyici. Kutuyu işaretleyin.
* Hedef IP. Siteden siteye bağlantı üzerinden izin verilen CloudSimple özel bulut alt ağına ait herhangi bir IP adresi girin. Palo Alto 'daki tünel arabirimlerinin (Tunnel. 20-10.64.5.2/32 ve Tunnel. 30-10.64.6.2/32 gibi), siteden siteye VPN üzerinden CloudSimple özel bulut IP adresine erişmesine izin verildiğinden emin olun. Proxy kimlikleri için aşağıdaki yapılandırmaya bakın.
* Profilinizi. İzleyici profilini seçin.

Proxy kimlikleri sekmesi: **IPv4** > **Ekle** ' ye tıklayın ve aşağıdakileri yapılandırın:

* Proxy KIMLIĞI. İlginç trafik için herhangi bir ad girin. Tek bir IPSec tünelinde yürütülen birden çok proxy kimliği olabilir.
* Yerel. Siteden siteye VPN üzerinden özel bulut alt ağları ile iletişim kurmasına izin verilen şirket içi yerel alt ağları belirtin.
* Uzak. Yerel alt ağlarla iletişim kurmasına izin verilen özel bulut uzak alt ağlarını belirtin.
* Protocol. **Herhangi birini**seçin.

İkincil CloudSimple VPN eşi için kullanılmak üzere başka bir IPSec tüneli oluşturmak için önceki adımları tekrarlayın.

## <a name="references"></a>Referanslar

Cisco ASA üzerinde NAT yapılandırma:

<a href="https://www.cisco.com/c/en/us/td/docs/security/asa/asa84/configuration/guide/asa_84_cli_config/nat_objects.html" target="_blank">Cisco ASA 5500 serisi yapılandırma kılavuzu</a>

Cisco ASA üzerinde desteklenen IKEv1 ve IKEv2 öznitelikleri:

<a href="https://www.cisco.com/c/en/us/td/docs/security/asa/asa90/configuration/guide/asa_90_cli_config/vpn_ike.html#21661" target="_blank">Cisco ASA serisi CLı yapılandırma kılavuzu</a>

8,4 ve üzeri sürümleriyle Cisco ASA üzerinde IPSec siteden siteye VPN 'yi yapılandırma:

<a href="https://www.cisco.com/c/en/us/support/docs/security/asa-5500-x-series-next-generation-firewalls/119141-configure-asa-00.html#anc8" target="_blank">ASA üzerinde ASDM veya CLı ile IKEv1 IPSec siteden siteye tünellerini yapılandırma</a>

Azure 'da Cisco Uyarlamalı güvenlik gereç sanal (ASAv) yapılandırma:

<a href="https://www.cisco.com/c/en/us/td/docs/security/asa/asa96/asav/quick-start-book/asav-96-qsg/asav-azure.html" target="_blank">Cisco Uyarlamalı güvenlik sanal gereci (ASAv) Hızlı Başlangıç Kılavuzu</a>

Palo Alto 'da proxy kimlikleri ile siteden siteye VPN 'i yapılandırma:

<a href="https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/vpns/set-up-Site-to-Site-vpn.html#" target="_blank">Siteden siteye VPN ayarlama</a>

Tünel izleyicisini ayarlama:

<a href="https://docs.paloaltonetworks.com/pan-os/7-1/pan-os-admin/vpns/set-up-tunnel-monitoring.html" target="_blank">Tünel Izlemeyi ayarlama</a>

IKE Gateway veya IPSec tünel işlemleri:

<a href="https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/vpns/set-up-Site-to-Site-vpn/enabledisable-refresh-or-restart-an-ike-gateway-or-ipsec-tunnel.html#" target="_blank">Bir ıKE ağ geçidini veya IPSec tüneli etkinleştirme/devre dışı bırakma, yenileme veya yeniden başlatma</a>
