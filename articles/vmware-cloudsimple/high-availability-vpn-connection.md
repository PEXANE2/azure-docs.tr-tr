---
title: CloudSimple tarafından Azure VMware Çözümü - Şirket içinde CloudSimple VPN ağ geçidine yüksek kullanılabilirlik yapılandırma
description: Şirket içi ortamınızdan yüksek kullanılabilirlik için yüksek kullanılabilirlik bağlantısını nasıl yapılandırıştırılabildiğini açıklar, yüksek kullanılabilirlik için etkinleştirilen CloudSimple VPN ağ geçidine
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6e3118814eacc6cc63b5db59bd7f1877c1d347dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025274"
---
# <a name="configure-a-high-availability-connection-from-on-premises-to-cloudsimple-vpn-gateway"></a>Şirket içi cloudsimple VPN ağ geçidine yüksek kullanılabilirlik bağlantısını yapılandırma

Ağ yöneticileri, şirket içi ortamlarından CloudSimple VPN ağ geçidine kadar yüksek kullanılabilirlikte bir IPsec Site-to-Site VPN bağlantısını yapılandırabilir.

Bu kılavuz, bir IPsec Site-to-Site VPN yüksek kullanılabilirlik bağlantısı için şirket içi bir güvenlik duvarı yapılandırmak için adımlar sunar. Ayrıntılı adımlar şirket içi güvenlik duvarı türüne özgüdir. Örnek olarak, bu kılavuz iki tür güvenlik duvarı için adımlar sunar: Cisco ASA ve Palo Alto Networks.

## <a name="before-you-begin"></a>Başlamadan önce

Şirket içi güvenlik duvarını yapılandırmadan önce aşağıdaki görevleri tamamlayın.

1. Kuruluşunuzun gerekli düğümleri [sağladığını](create-nodes.md) ve en az bir CloudSimple Private Cloud oluşturduğunu doğrulayın.
2. Şirket içi ağınızla CloudSimple Private Cloud'unuz arasında [Siteden Siteye VPN ağ geçidini yapılandırın.](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway)

Desteklenen faz 1 ve aşama 2 teklifleri için [VPN ağ geçitlerine genel bakışa](cloudsimple-vpn-gateways.md) bakın.

## <a name="configure-on-premises-cisco-asa-firewall"></a>Şirket içi Cisco ASA güvenlik duvarLarını yapılandırma

Bu bölümdeki talimatlar Cisco ASA sürüm 8.4 ve sonrası için geçerlidir. Yapılandırma örneğinde, Cisco Adaptive Security Appliance Software Version 9.10 IKEv1 modunda dağıtılır ve yapılandırılır.

Siteden Siteye VPN'in çalışması için, şirket içi Cisco ASA VPN ağ geçidinin dış arabiriminde CloudSimple birincil ve ikincil genel IP'den (peer IP) UDP 500/4500 ve ESP 'ye (IP protokolü 50) izin vermelisiniz.

### <a name="1-configure-phase-1-ikev1"></a>1. Faz 1 'i yapılandırın (IKEv1)

Dış arabirimde faz 1'i (IKEv1) etkinleştirmek için Cisco ASA güvenlik duvarına aşağıdaki CLI komutunu girin.

```crypto ikev1 enable outside```

### <a name="2-create-an-ikev1-policy"></a>2. IkEv1 politikası oluşturma

Karma, kimlik doğrulama, Diffie-Hellman grubu, ömür boyu ve şifreleme için kullanılacak algoritmaları ve yöntemleri tanımlayan bir IKEv1 ilkesi oluşturun.

```
crypto ikev1 policy 1
authentication pre-share
encryption aes-256
hash sha
group 2
lifetime 28800
```

### <a name="3-create-a-tunnel-group"></a>3. Tünel grubu oluşturma

IPsec öznitelikleri altında bir tünel grubu oluşturun. [Siteden Siteye VPN ağ geçidinizi yapılandırırken](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway)ayarladığınız eş IP adresini ve önceden paylaşılan tüneli yapılandırın.

```
tunnel-group <primary peer ip> type ipsec-l2l
tunnel-group <primary peer ip> ipsec-attributes
ikev1 pre-shared-key *****

tunnel-group <secondary peer ip> type ipsec-l2l
tunnel-group <secondary peer ip> ipsec-attributes
ikev1 pre-shared-key *****
```

### <a name="4-configure-phase-2-ipsec"></a>4. Faz 2 'yi (IPsec) yapılandırın

Faz 2'yi (IPsec) yapılandırmak için, şifrelenecek ve tünel oluşturulacak trafiği tanımlayan bir erişim denetim listesi (ACL) oluşturun. Aşağıdaki örnekte, ilgi trafiği şirket içi yerel alt ağdan (10.16.1.0/24) Özel Bulut uzak alt ağına (192.168.0.0/24) gelen tünelden dir. Siteler arasında birden çok alt ağ varsa, ACL birden çok giriş içerebilir.

Cisco ASA sürümleri 8.4 ve sonraki sürümlerde, ağlar, alt ağlar, ana bilgisayar IP adresleri veya birden çok nesne için kapsayıcı görevi veren nesneler veya nesne grupları oluşturulabilir. Uzak alt ağlar için yerel ve bir nesne için bir nesne oluşturun ve bunları kripto ACL ve NAT deyimleri için kullanın.

#### <a name="define-an-on-premises-local-subnet-as-an-object"></a>Şirket içi yerel alt ağı nesne olarak tanımlama

```
object network AZ_inside
subnet 10.16.1.0 255.255.255.0
```

#### <a name="define-the-cloudsimple-remote-subnet-as-an-object"></a>CloudSimple uzak alt netini nesne olarak tanımlama

```
object network CS_inside
subnet 192.168.0.0 255.255.255.0
```

#### <a name="configure-an-access-list-for-the-traffic-of-interest"></a>İlgi çekici trafik için bir erişim listesini yapılandırma

```
access-list ipsec-acl extended permit ip object AZ_inside object CS_inside
```

### <a name="5-configure-the-transform-set"></a>5. Dönüştürme kümesini yapılandırın

Anahtar sözcüğü ```ikev1```içermesi gereken dönüştürme kümesini (TS) yapılandırın. TS'de belirtilen şifreleme ve karma [öznitelikler, CloudSimple VPN ağ geçitleri için Varsayılan yapılandırmada](cloudsimple-vpn-gateways.md)listelenen parametrelerle eşleşmelidir.

```
crypto ipsec ikev1 transform-set devtest39 esp-aes-256 esp-sha-hmac 
```

### <a name="6-configure-the-crypto-map"></a>6. Kripto haritasını yapılandırın

Bu bileşenleri içeren kripto haritasını yapılandırın:

* Eş IP adresi
* İlgi trafiğini içeren tanımlı ACL
* Dönüştürme Seti

```
crypto map mymap 1 set peer <primary peer ip> <secondary peer ip>
crypto map mymap 1 match address ipsec-acl
crypto map mymap 1 set ikev1 transform-set devtest39
```

### <a name="7-apply-the-crypto-map"></a>7. Kripto haritasını uygulayın

Kripto haritayı dış arabirimde uygulayın:

```crypto map mymap interface outside```

### <a name="8-confirm-applicable-nat-rules"></a>8. Geçerli NAT kurallarını onaylayın

Aşağıda kullanılan NAT kuralı dır. VPN trafiğinin başka bir NAT kuralına tabi olmadığından emin olun.

```nat (inside,outside) source static AZ_inside AZ_inside destination static CS_inside CS_inside```

### <a name="sample-ipsec-site-to-site-vpn-established-output-from-cisco-asa"></a>Örnek IPsec Site-to-Site VPN Cisco ASA çıktı kurdu

Faz 1 çıktısı:

![Cisco ASA güvenlik duvarı için Faz 1 çıkışı](media/ha-vpn-connection-cisco-phase1.png)

Faz 2 çıktı:

![Cisco ASA güvenlik duvarı için Aşama 2 çıkışı](media/ha-vpn-connection-cisco-phase2.png)

## <a name="configure-on-premises-palo-alto-networks-firewall"></a>Şirket içi Palo Alto Networks güvenlik duvarLarını yapılandırma

Bu bölümdeki talimatlar Palo Alto Networks sürüm 7.1 ve sonrası için geçerlidir. Bu yapılandırma örneğinde, Palo Alto Networks VM-Serisi Yazılım Sürüm 8.1.0 ikev1 modunda dağıtılır ve yapılandırılır.

Siteden Siteye VPN'in çalışması için, şirket içi Palo Alto Networks ağ geçidinin dış arabiriminde CloudSimple birincil ve ikincil genel IP'den (eş IP) UDP 500/4500 ve ESP'ye (IP protokolü 50) izin vermelisiniz.

### <a name="1-create-primary-and-secondary-tunnel-interfaces"></a>1. Birincil ve ikincil tünel arabirimleri oluşturma

Palo Alto güvenlik duvarında oturum açın, **Ağ** > **Arabirimleri** > **Tünel** > **Ekle'yi**seçin, aşağıdaki alanları yapılandırın ve **Tamam'ı**tıklatın.

* Arayüz Adı. İlk alan 'tünel' anahtar kelimesi ile otomatik olarak doldurulur. Bitişik alana, 1 ile 9999 arasında herhangi bir sayı girin. Bu arabirim, şirket içi veri merkezi ile Özel Bulut arasında Siteden Siteye trafiği taşımak için birincil tünel arabirimi olarak kullanılacaktır.
* Yorum. Tünelin amacının kolay tanımlanması için yorum girin
* Netflow Profili. Varsayılan olarak bırakın.
* Config. Arayüz Atama: Sanal Yönlendirici: **Varsayılan ı**seçin. 
        Güvenlik Bölgesi: Güvenilen LAN trafiği için bölgeyi seçin. Bu örnekte, LAN trafiği için bölgenin adı 'Güven'dir.
* IPv4' den. Birincil tünel arabirimine atanacak ve tünelleri izlemek için kullanılacak olan (daha sonra açıklanacak) ortamınızda çakışmayan kullanılmamış /32 ip adresini **ekle'yi** tıklatın.

Bu yapılandırma yüksek kullanılabilirlik VPN için olduğundan, iki tünel arabirimi gereklidir: bir birincil ve bir ikincil. İkincil tünel arabirimini oluşturmak için önceki adımları yineleyin. Farklı bir tünel kimliği ve kullanılmayan farklı bir /32 ip adresi seçin.

### <a name="2-set-up-static-routes-for-private-cloud-subnets-to-be-reached-over-the-site-to-site-vpn"></a>2. Siteden Siteye VPN üzerinden ulaşılacak Özel Bulut alt ağları için statik rotalar ayarlama

Şirket içi alt ağların CloudSimple özel bulut alt ağlarına ulaşması için rotalar gereklidir.

**Ağ** > **Sanal Yönlendiriciler** > *varsayılan* > **Statik Yolları** > **Ekle'yi**seçin, aşağıdaki alanları yapılandırın ve **Tamam'ı**tıklatın.

* Adı. Rotanın amacının kolayca tanımlanması için herhangi bir ad girin.
* Hedef. Şirket içinde S2S tünel arabirimleri üzerinden ulaşılacak CloudSimple özel bulut alt ağlarını belirtin
* Arabirim. Açılan alt kattan adım-1'de (Bölüm-2) oluşturulan birincil tünel arabirimini seçin. Bu örnekte, tünel.20'dir.
* Sonraki Hop. **Hiçbiri** seçeneğini belirtin.
* Yönetici Mesafesi. Varsayılan olarak bırakın.
* Metrik. 1 ile 65535 arasında herhangi bir değer girin. Anahtar, birincil tünel arabirimine karşılık gelen rota için, eski rotayı tercih eden ikincil tünel arabirimine karşılık gelen rota için daha düşük metrik girmektir. Tünel.20'nin metrik değeri 20 ise, tünel için 30 metrik değeri ise, tünel.20 tercih edilir.
* Rota Tablosu. Varsayılan olarak bırakın.
* BFD Profili. Varsayılan olarak bırakın.
* Yol izleme. İşaretlemeden bırakın.

Özel Bulut alt ağlarının ikincil tünel arabirimi üzerinden ikincil/yedekleme rotası olarak kullanması için başka bir rota oluşturmak için önceki adımları yineleyin. Bu kez, birincil rotaiçin farklı tünel kimliği ve daha yüksek bir metrik seçin.

### <a name="3-define-the-cryptographic-profile"></a>3. Şifreleme profilini tanımlayın

IKEv1 Faz 1'de VPN tünellerinin kurulumu için kullanılacak tanımlama, kimlik doğrulama ve şifreleme protokollerini ve algoritmalarını belirten bir şifreleme profili tanımlayın.

**Ağ** > **Genişletme Ağ Profillerini** > Seçin**IKE Crypto** > **Add,** aşağıdaki alanları yapılandırın ve **Tamam'ı**tıklatın.

* Adı. IKE kripto profilinin herhangi bir adını girin.
* DH Grubu. **Ekle'yi** tıklatın ve uygun DH grubunu seçin.
* Şifreleme. **Ekle'yi** tıklatın ve uygun şifreleme yöntemini seçin.
* Kimlik doğrulaması. **Ekle'yi** tıklatın ve uygun kimlik doğrulama yöntemini seçin.
* Anahtar ömrü. Varsayılan olarak bırakın.
* IKEv2 Kimlik Doğrulama Çoklu. Varsayılan olarak bırakın.

### <a name="4-define-ike-gateways"></a>4. IKE ağ geçitlerini tanımlama

VPN tünelinin her iki ucundaki eşler arasında iletişim kurmak için IKE ağ geçitlerini tanımlayın.

**Ağ** > **Genişletme Ağ Profillerini** > Seçin**IKE Ağ Geçitleri** > **Ekle,** aşağıdaki alanları yapılandırın ve **Tamam'ı**tıklatın.

Genel sekme:

* Adı. Birincil CloudSimple VPN eşile bakılması için IKE ağ geçidinin adını girin.
* Sürüm. **Yalnızca IKEv1 modunu**seçin.
* Adres Türü. **IPv4** seçin.
* Arabirim. Genel karşı karşıya veya dış arabirimi seçin.
* Yerel IP Adresi. Varsayılan olarak bırakın.
* Eş IP Adresi Türü. **IP'yi**seçin.
* Akran Adresi. Birincil CloudSimple VPN eş IP adresini girin.
* Kimlik doğrulaması. **Önceden Paylaşılan Anahtarı**seçin.
* Önceden paylaşılan Anahtar / Önceden Paylaşılan Key'i onaylayın. CloudSimple VPN ağ geçidi anahtarıyla eşleşecek önceden paylaşılan anahtarı girin.
* Yerel kimlik. Şirket içi Palo Alto güvenlik duvarının genel IP adresini girin.
* Akran Tanımlama. Birincil CloudSimple VPN eş IP adresini girin.

Gelişmiş Seçenekler sekmesi:

* Pasif Modu etkinleştirin. İşaretlemeden bırakın.
* NAT Geçişi'ni etkinleştirin. Şirket içi Palo Alto güvenlik duvarı herhangi bir NAT aygıtının arkasında değilse, kontrolsüz bırakın. Aksi takdirde, onay kutusunu seçin.

IKEv1:

* Değişim modu. **Ana**seçin.
* IKE Kripto Profili. Daha önce oluşturduğunuz IKE Crypto profilini seçin. Parçalamayı Etkinleştir kutusunu işaretlenmemiş bırakın.
* Ölü Akran Algılama. Kutuyu işaretsiz bırakın.

İkincil IKE ağ geçidini oluşturmak için önceki adımları yineleyin.

### <a name="5-define-ipsec-crypto-profiles"></a>5. IPSEC Kripto profillerini tanımlayın

**Ağ** > **Genişlet Ağ Profillerini** > **IPSEC Crypto** > **Ekle'yi**seçin, aşağıdaki alanları yapılandırın ve **Tamam'ı**tıklatın.

* Adı. IPsec kripto profili için bir ad girin.
* IPsec Protokolü. **ESP'yi**seçin.
* Şifreleme. **Ekle'yi** tıklatın ve uygun şifreleme yöntemini seçin.
* Kimlik doğrulaması. **Ekle'yi** tıklatın ve uygun kimlik doğrulama yöntemini seçin.
* DH Grubu. **No-pfs**seçin.
* Ömür. 30 dakika olarak ayarlayın.
* Etkinleştirmek. Kutuyu işaretsiz bırakın.

İkincil CloudSimple VPN eş olarak kullanılacak başka bir IPsec kripto profili oluşturmak için önceki adımları yineleyin. Aynı IPSEC Crypto profili hem birincil hem de ikincil IPsec tünelleri için de kullanılabilir (aşağıdaki yordamı görün)

### <a name="6-define-monitor-profiles-for-tunnel-monitoring"></a>6. Tünel izleme için monitör profillerini tanımlayın

**Ağ** > **Genişletme Ağ Profillerini** > Genişlet**İzle'****Monitor** > yi seçin, aşağıdaki alanları yapılandırın ve **Tamam'ı**tıklatın.

* Adı. Hataya proaktif tepki vermek için tünel izleme için kullanılacak Monitör profilinin herhangi bir adını girin.
* Eylem. **Üzerinden Başarısız'ı**seçin.
* Aralığı. **3**değerini girin.
* Eşik öğesini seçin. **Değeri girin 7**.

### <a name="7-set-up-primary-and-secondary-ipsec-tunnels"></a>7. Birincil ve ikincil IPsec tünelleri kurmak.

**Ağ** > **IPsec Tünelleri** > **Ekle'yi**seçin, aşağıdaki alanları yapılandırın ve **Tamam'ı**tıklatın.

Genel sekme:

* Adı. Birincil CloudSimple VPN eşile eşlenecek birincil IPSEC tüneli için herhangi bir ad girin.
* Tünel Arayüzü. Birincil tünel arabirimini seçin.
* Türü. Varsayılan olarak bırakın.
* Adres Türü. **IPv4** seçin.
* IKE Geçidi. Birincil IKE ağ geçidini seçin.
* IPsec Kripto Profili. Birincil IPsec profilini seçin. **Gelişmiş seçenekleri göster'i**seçin.
* Yeniden Oynatma Koruması'nı etkinleştirin. Varsayılan olarak bırakın.
* TOS Üstbilgisini kopyalayın. Kutuyu işaretsiz bırakın.
* Tünel Monitörü. Kutuyu kontrol et.
* Hedef IP. Siteden Siteye bağlantısı üzerinden izin verilen CloudSimple Private Cloud alt ağına ait herhangi bir IP adresi girin. Palo Alto'daki tünel arabirimlerinin (tünel.20 - 10.64.5.2/32 ve tünel.30 - 10.64.6.2/32 gibi) Siteden Siteye VPN üzerinden CloudSimple Private Cloud IP adresine ulaşmasına izin verildiğinden emin olun. Proxy tbm'leri için aşağıdaki yapılandırmaya bakın.
* Profil. Monitör profilini seçin.

Proxy IDs sekmesi: **IPv4** > **Ekle'yi** tıklatın ve aşağıdakileri yapılandırın:

* Proxy kimliği. İlginç trafik için herhangi bir ad girin. Bir IPsec tünelinde taşınan birden fazla Proxy bilgisayarı olabilir.
* Yerel. Siteden Siteye VPN üzerinden Özel Bulut alt ağlarıyla iletişim kurmasına izin verilen şirket içi yerel alt ağları belirtin.
* Uzaktan. Yerel alt ağlarla iletişim kurmasına izin verilen Özel Bulut uzak alt ağlarını belirtin.
* Protokolü. **Herhangi bir**.

İkincil CloudSimple VPN eş için kullanmak üzere başka bir IPsec tüneli oluşturmak için önceki adımları yineleyin.

## <a name="references"></a>Başvurular

Cisco ASA'da NAT'nin yapılandırılması:

<a href="https://www.cisco.com/c/en/us/td/docs/security/asa/asa84/configuration/guide/asa_84_cli_config/nat_objects.html" target="_blank">Cisco ASA 5500 Serisi Yapılandırma Kılavuzu</a>

Cisco ASA'da IKEv1 ve IKEv2 özellikleri desteklenmiştir:

<a href="https://www.cisco.com/c/en/us/td/docs/security/asa/asa90/configuration/guide/asa_90_cli_config/vpn_ike.html#21661" target="_blank">Cisco ASA Serisi CLI Yapılandırma Kılavuzu</a>

Cisco ASA'da IPsec Site-To-Site VPN'ini sürüm 8.4 ve sonrası ile yapılandırma:

<a href="https://www.cisco.com/c/en/us/support/docs/security/asa-5500-x-series-next-generation-firewalls/119141-configure-asa-00.html#anc8" target="_blank">IKEv1 IPsec Site-To-Site Tünellerini ASA'da ASDM veya CLI ile yapılandırın</a>

Cisco Adaptive Security Appliance sanal (ASAv) Azure'da yapılandırma:

<a href="https://www.cisco.com/c/en/us/td/docs/security/asa/asa96/asav/quick-start-book/asav-96-qsg/asav-azure.html" target="_blank">Cisco Adaptive Security Virtual Appliance (ASAv) quickstart Kılavuzu</a>

Palo Alto'da Proxy T'lerle Siteden Siteye VPN'i yapılandırma:

[Siteden Siteye VPN'i Ayarlama](https://docs.paloaltonetworks.com/pan-os/9-0/pan-os-admin/vpns/set-up-site-to-site-vpn#)

Tünel monitörü kurulumu:

[Tünel İzlemeyi Ayarlama](https://docs.paloaltonetworks.com/pan-os/7-1/pan-os-admin/vpns/set-up-tunnel-monitoring.html)

IKE ağ geçidi veya IPsec tünel işlemleri:

<a href="https://docs.paloaltonetworks.com/pan-os/9-0/pan-os-admin/vpns/set-up-site-to-site-vpn/enabledisable-refresh-or-restart-an-ike-gateway-or-ipsec-tunnel#" target="_blank">Bir IKE Ağ Geçidi veya IPsec Tünelini Etkinleştirme/Devre Dışı Kaldırma, Yenileme veya Yeniden Başlatma</a>
