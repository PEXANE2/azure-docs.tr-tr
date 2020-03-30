---
title: CloudSimple'a göre Azure VMware Çözümü - Layer 2 ağını şirket içinde Özel Bulut'a genişletin
description: CloudSimple Private Cloud'da NSX-T ile şirket içi bağımsız NSX Edge istemcisi arasında Katman 2 VPN'inin nasıl ayarlanır?
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2ddfa9611143d5c3f823539e018c8afc885c6a46
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77083216"
---
# <a name="migrate-workloads-using-layer-2-stretched-networks"></a>Katman 2 esnetilmiş ağlarını kullanarak iş yüklerini geçirme

Bu kılavuzda, Katman 2 VPN'i (L2VPN) kullanarak katman 2 ağını şirket içi ortamınızdan CloudSimple Private Cloud'unuza kadar nasıl genişleteceğinizi öğreneceksiniz. Bu çözüm, şirket içi VMware ortamınızda çalışan iş yüklerinin, iş yüklerinizi yeniden IP'ye yeniden iplemek zorunda kalmadan aynı alt ağ adres alanı içinde Azure'daki Özel Bulut'a geçişini sağlar.

Katman 2 ağlarının L2VPN tabanlı esneme leri, şirket içi VMware ortamınızda NSX tabanlı ağlarla veya şirket içi VMware tabanlı ağlarla çalışabilir. Şirket içi iş yükleri için NSX tabanlı ağlarınız yoksa, bağımsız bir NSX Edge Services Gateway kullanabilirsiniz.

> [!NOTE]
> Bu kılavuz, şirket içi ve Özel Bulut veri merkezlerinin Siteden Siteye VPN üzerinden bağlı olduğu senaryoyu kapsar.

## <a name="deployment-scenario"></a>Dağıtım senaryosu

Şirket içi ağınızı L2VPN kullanarak genişletmek için bir L2VPN sunucusu (hedef NSX-T Tier0 yönlendiricisi) ve bir L2VPN istemcisi (kaynak bağımsız istemcisi) yapılandırmanız gerekir.  

Bu dağıtım senaryosunda, Özel Bulut'unuzun şirket içi ortamınıza, şirket içi yönetimin ve vMotion alt ağlarının Özel Bulut yönetimi ve vMotion alt ağlarıyla iletişim kurmasına olanak tanıyan siteden siteye VPN tüneli aracılığıyla bağlanır. Bu düzenleme Cross vCenter vMotion (xVC-vMotion) için gereklidir. Özel Bulut'ta L2VPN sunucusu olarak bir NSX-T Tier0 yönlendiricisi dağıtılır.

Bağımsız NSX Edge, şirket içi ortamınızda L2VPN istemcisi olarak dağıtılır ve daha sonra L2VPN sunucusuyla eşleştirilir. Her iki tarafta da bir GRE tünel bitiş noktası oluşturulur ve şirket içi Katman 2 ağını Özel Bulut'unuza 'esnetmek' için yapılandırılır. Bu yapılandırma aşağıdaki şekilde gösterilmiştir.

![Dağıtım senaryosu](media/l2vpn-deployment-scenario.png)

L2 VPN kullanarak geçiş hakkında daha fazla bilgi edinmek için VMware belgelerinde [Sanal Özel Ağlar'a](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/2.3/com.vmware.nsxt.admin.doc/GUID-A8B113EC-3D53-41A5-919E-78F1A3705F58.html#GUID-A8B113EC-3D53-41A5-919E-78F1A3705F58__section_44B4972B5F12453B90625D98F86D5704) bakın.

## <a name="prerequisites-for-deploying-the-solution"></a>Çözümü dağıtmak için ön koşullar

Çözümü dağıtmadan ve yapılandırmadan önce aşağıdakilerin yerinde olduğunu doğrulayın:

* Şirket içi vSphere versiyonu 6.7U1+ veya 6.5P03+ 'dır.
* Şirket içi vSphere lisansı Enterprise Plus düzeyindedir (vSphere Distributed Switch için).
* Özel Bulut'unuza genişletilecek iş yükü Layer 2 ağını tanımlayın.
* L2VPN istemci cihazınızı dağıtmak için şirket içi ortamınızda bir Katman 2 ağı belirleyin.
* [Özel Bulut zaten oluşturuldu.](create-private-cloud.md)
* Bağımsız NSX-T Edge cihazının sürümü, Özel Bulut ortamınızda kullanılan NSX-T Manager sürümüyle (NSX-T 2.3.0) uyumludur.
* Şirket içi vCenter'da sahte iletim etkin leştirilmiş bir gövde bağlantı noktası grubu oluşturuldu.
* NSX-T bağımsız istemci uplink IP adresi için kullanmak üzere genel bir IP adresi ayrılmıştır ve 1:1 NAT iki adres arasında çeviri için yerindedir.
* DNS iletme, özel bulut DNS sunucularını işaret etmek için az.cloudsimple.io etki alanı için şirket içi DNS sunucularında ayarlanır.
* RTT gecikmesi, vMotion'ın iki sitede çalışması için gerekli olduğu gibi 150 ms'den daha az veya eşittir.

## <a name="limitations-and-considerations"></a>Sınırlamalar ve önemli noktalar

Aşağıdaki tabloda desteklenen vSphere sürümleri ve ağ bağdaştırıcısı türleri listelenmiştir.  

| vSphere sürümü | Kaynak vSwitch türü | Sanal NIC sürücüsü | Hedef vSwitch Türü | Destekleniyor mu? |
------------ | ------------- | ------------ | ------------- | ------------- 
| Tümü | Dvs | Tümü | Dvs | Evet |
| vSphere 6.7UI veya üstü, 6.5P03 veya üstü | Dvs | VMXNET3 | N-VDS | Evet |
| vSphere 6.7UI veya üstü, 6.5P03 veya üstü | Dvs | E1000 | N-VDS | [VWware başına desteklenmiyor](https://kb.vmware.com/s/article/56991) |
| vSphere 6.7UI veya 6.5P03, NSX-V veya NSX-T2.2, 6.5P03 veya üstü sürümler | Tümü | Tümü | N-VDS | [VWware başına desteklenmiyor](https://kb.vmware.com/s/article/56991) |

VMware NSX-T 2.3 sürümü itibariyle:

* L2VPN üzerinden şirket içi uzatılmış Özel Bulut tarafındaki mantıksal anahtar aynı anda yönlendirilemiyor. Uzatılmış mantıksal anahtar mantıksal bir yönlendiriciye bağlanamıyor.
* L2VPN ve rota tabanlı IPSEC VPN'ler yalnızca API çağrıları kullanılarak yapılandırılabilir.

Daha fazla bilgi için VMware belgelerinde [Sanal Özel Ağlar'a](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/2.3/com.vmware.nsxt.admin.doc/GUID-A8B113EC-3D53-41A5-919E-78F1A3705F58.html#GUID-A8B113EC-3D53-41A5-919E-78F1A3705F58__section_44B4972B5F12453B90625D98F86D5704) bakın.

### <a name="sample-l2-vpn-deployment-addressing"></a>Örnek L2 VPN dağıtım adresleme

### <a name="on-premises-network-where-the-standalone-esg-l2-vpn-client-is-deployed"></a>Bağımsız ESG'nin (L2 VPN istemcisi) dağıtıldığı şirket içi ağ

| **Öğe** | **Değer** |
|------------|-----------------|
| Ağ adı | MGMT_NET_VLAN469 |
| VLAN | 469 |
| Cıdr| 10.250.0.0/24 |
| Bağımsız Edge cihazı IP adresi | 10.250.0.111 |
| Bağımsız Kenar cihazı NAT IP adresi | 192.227.85.167 |

### <a name="on-premises-network-to-be-stretched"></a>Şirket içi ağ uzatılır

| **Öğe** | **Değer** |
|------------|-----------------|
| VLAN | 472 |
| Cıdr| 10.250.3.0/24 |

### <a name="private-cloud-ip-schema-for-nsx-t-tier0-router-l2-vpn-serve"></a>NSX-T Tier0 Router için Özel Bulut IP şeması (L2 VPN hizmeti)

| **Öğe** | **Değer** |
|------------|-----------------|
| Loopback arabirimi | 192.168.254.254/32 |
| Tünel arabirimi | 5.5.5.1/29 |
| Mantıksal anahtar (gerilmiş) | Stretch_LS |
| Loopback arabirimi (NAT IP adresi) | 104.40.21.81 |

### <a name="private-cloud-network-to-be-mapped-to-the-stretched-network"></a>Özel Bulut ağı gerilmiş ağa eşlenecek

| **Öğe** | **Değer** |
|------------|-----------------|
| VLAN | 712 |
| Cıdr| 10.200.15.0/24 |

## <a name="fetch-the-logical-router-id-needed-for-l2vpn"></a>L2VPN için gereken mantıksal yönlendirici kimliğini getirin

Aşağıdaki adımlar, IPsec ve L2VPN hizmetleri için Tier0 DR mantıksal yönlendirici örneğinin mantıksal yönlendirici kimliğinin nasıl getirilip getirilip getirilip getirilip getirilip getirilip getirilip getirilip getirili gösterileceğidir. L2VPN'i uygularken mantıksal yönlendirici kimliği daha sonra gereklidir.

1. NSX-T Manager https://*nsx-t-manager-ip-adresinde* oturum açın ve **Ağ** > **Yönlendiricileri** > **Sağlayıcısı-LR** > **Genel Bakış'ı**seçin. **Yüksek Kullanılabilirlik Modu**için **Active-Standby'yi**seçin. Bu eylem, Tier0 yönlendiricisinin şu anda etkin olduğu Edge VM'yi gösteren bir açılır pencere açar.

    ![Etkin bekleme yi seçin](media/l2vpn-fetch01.png)

2. **Kumaş** > **Düğümleri** > **Kenarları**seçin. Önceki adımda tanımlanan etkin Edge VM'nin (Edge VM1) yönetim IP adresini not alın.

    ![Not yönetimi IP](media/l2vpn-fetch02.png)

3. Edge VM'nin yönetim IP adresine bir SSH oturumu açın. Kullanıcı ```get logical-router``` adı **admin** ve şifre **CloudSimple 123**ile komutu çalıştırın! .

    ![mantıksal yönlendirici çıktısı alma](media/l2vpn-fetch03.png)

4. 'DR-Provider-LR' girdisi görmüyorsanız, aşağıdaki adımları tamamlayın.

5. İki bindirme destekli mantıksal anahtar oluşturun. Mantıksal bir anahtar, geçirilen iş yüklerinin bulunduğu şirket içi ne kadar uzatılır. Başka bir mantıksal anahtar bir kukla anahtarıdır. Talimatlar için vmware belgelerinde [Mantıksal Anahtar Oluştur'a](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/2.3/com.vmware.nsxt.admin.doc/GUID-23194F9A-416A-40EA-B9F7-346B391C3EF8.html) bakın.

    ![Mantıksal anahtar oluşturma](media/l2vpn-fetch04.png)

6. Sahte anahtarı, şirket içi veya Özel Bulut'unuzdan bir bağlantı yerel IP adresi veya çakışmayan herhangi bir alt ağla birlikte Tier1 yönlendiricisine takın. Bkz. VMware belgelerinde [Tier-1 Mantıksal Yönlendirici'de Downlink Bağlantı Noktası Ekle.](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/2.3/com.vmware.nsxt.admin.doc/GUID-E7EA867C-604C-4224-B61D-2A8EF41CB7A6.html)

    ![Kukla anahtarını tak](media/l2vpn-fetch05.png)

7. Edge `get logical-router` VM'nin SSH oturumunda komutu yeniden çalıştırın. 'DR-Provider-LR' mantıksal yönlendiricinin UUID'si görüntülenir. L2VPN'i yapılandırırken gerekli olan UUID'ye not edin.

    ![mantıksal yönlendirici çıktısı alma](media/l2vpn-fetch06.png)

## <a name="fetch-the-logical-switch-id-needed-for-l2vpn"></a>L2VPN için gereken mantıksal anahtarlı kimliği getirin

1. [NSX-T Yöneticisi'nde](https://nsx-t-manager-ip-address)oturum açın.
2. **Ağ** > **Anahtarlama** > **Anahtarları**  >  **<\Mantıksal\>anahtar** > **Genel Bakış'ı**seçin.
3. L2VPN'i yapılandırırken gerekli olan streç mantıksal anahtarın UUID'sini not edin.

    ![mantıksal yönlendirici çıktısı alma](media/l2vpn-fetch-switch01.png)

## <a name="routing-and-security-considerations-for-l2vpn"></a>L2VPN için yönlendirme ve güvenlik hususları

NSX-T Tier0 yönlendiricisi ile bağımsız NSX Edge istemcisi arasında IPsec rota tabanlı bir VPN oluşturmak için, NSX-T Tier0 yönlendiricisinin geri dönüş arabiriminin UDP 500/4500 üzerinden nsx bağımsız istemcisinin ortak IP adresiyle iletişim kurababilmesi gerekir.

### <a name="allow-udp-5004500-for-ipsec"></a>IPsec için UDP 500/4500'e izin ver

1. CloudSimple portalındaki NSX-T Tier0 geri dönüş arabirimi için [genel bir IP adresi oluşturun.](public-ips.md)

2. UDP 500/ 4500 gelen trafiğe izin veren ve güvenlik duvarı tablosunu NSX-T HostTransport alt ağına takmaya izin veren durum sallayan [bir güvenlik duvarı tablosu oluşturun.](firewall.md)

### <a name="advertise-the-loopback-interface-ip-to-the-underlay-network"></a>Loopback arabirimi IP'sinin alttaki ağına tanıtın

1. Geri dönüş arabirimi ağı için null bir rota oluşturun. NSX-T Manager'da oturum açın ve Ağ **Yönlendirme** > **Routing** > **Yönlendiricileri** > **Sağlayıcı-LR** > **Yönlendirme** > Statik**Rotalarını**seçin. **Ekle**’ye tıklayın. **Network**için loopback arabirimi IP adresini girin. **Sonraki Atlamalar**için **Ekle'yi**tıklatın, bir sonraki atlama için 'Null' belirtin ve Yönetici Mesafesi için 1 varsayılanını tutun.

    ![Statik rota ekleme](media/l2vpn-routing-security01.png)

2. IP öneki listesi oluşturun. NSX-T Manager'da oturum açın ve Ağ **Yönlendirme** > **Routing** > **Yönlendiricileri** > **Sağlayıcı-LR** > **Yönlendirme** > IP**Önek Listeleri'ni**seçin. **Ekle**’ye tıklayın. Listeyi tanımlamak için bir ad girin. **Öneekler**için iki kez **Ekle'yi** tıklatın. İlk satırda, **Ağ** için '0.0.0.0/0' ve **Eylem**için 'Reddet' girin. İkinci satırda, **Ağ** için **Any'yi** ve **Eylem** **İzni'ni** seçin.
3. IP önek listesini her iki BGP komşusuna (TOR) ekleyin. IP önek listesini BGP komşuya eklemek, varsayılan rotanın BGP'de TOR anahtarlarına duyurulmasını engeller. Ancak, null rota içeren başka bir rota TOR anahtarları için loopback arabirim IP adresi reklamını yapacaktır.

    ![IP önek listesi oluşturma](media/l2vpn-routing-security02.png)

4. NSX-T Manager'da oturum açın ve Ağ **Yönlendirme** > **Routing** > **Yönlendiricileri** > **Sağlayıcı-LR** > **Yönlendirme** > **BGP** > **Komşuları'nı**seçin. İlk komşuyu seçin. **Adres Ailelerini** **Edit'i** > tıklatın. IPv4 ailesi **için, Çıkış Filtresi** sütununa edin ve oluşturduğunuz IP önek listesini seçin. **Kaydet**'e tıklayın. İkinci komşu için bu adımı tekrarlayın.

    ![IP önek listesini](media/l2vpn-routing-security03.png) ![ekle 1 IP önek listesini ekle 2](media/l2vpn-routing-security04.png)

5. Null statik rotayı BGP'ye yeniden dağıtın. Geri dönüş arabirimi yolunun alt kainatanıtımı için null statik rotayı BGP'ye yeniden dağıtmanız gerekir. NSX-T Manager'da oturum açın ve Ağ **Yönlendirme** > **Routing** > **Yönlendiricileri** > **Sağlayıcı-LR** > **Yönlendirme** > Rotası Yeniden**Dağıtım** > **Komşuları'nı**seçin. **Sağlayıcı-LR-Route_Redistribution'yi** seçin ve **Edit'i**tıklatın. **Statik** onay kutusunu seçin ve **Kaydet'i**tıklatın.

    ![Null statik rotayı BGP'ye yeniden dağıtma](media/l2vpn-routing-security05.png)

## <a name="configure-a-route-based-vpn-on-the-nsx-t-tier0-router"></a>NSX-T Tier0 yönlendiriciüzerinde rota tabanlı VPN'i yapılandırma

NSX-T Tier0 yönlendiricisinde rota tabanlı bir VPN yapılandırmak için tüm ayrıntıları doldurmak için aşağıdaki şablonu kullanın. Her POST aramasında ki UUID'ler sonraki POST aramalarında gereklidir. L2VPN için geri dönüş ve tünel arabirimlerinin IP adresleri benzersiz olmalı ve şirket içi veya Özel Bulut ağlarıyla örtüşmemelidir.

L2VPN için kullanılan geri dönüş ve tünel arabirimi için seçilen IP adresleri benzersiz olmalı ve şirket içi veya Özel Bulut ağlarıyla örtüşmemelidir. Loopback arabirim ağı her zaman /32 olmalıdır.

```
Loopback interface ip : 192.168.254.254/32
Tunnel interface subnet : 5.5.5.0/29
Logical-router ID : UUID of Tier0 DR logical router obtained in section “Steps to fetch Logical-Router ID needed for L2VPN”
Logical-switch ID(Stretch) : UUID of Stretch Logical Switch obtained earlier
IPSec Service ID :
IKE profile ID :
DPD profile ID :
Tunnel Profile ID :
Local-endpoint ID :
Peer end-point ID :
IPSec VPN session ID (route-based) :
L2VPN service ID :
L2VPN session ID :
Logical-Port ID :
Peer Code :
```

Aşağıdaki API çağrılarının tümü için IP adresini NSX-T Manager IP adresinizle değiştirin. Tüm bu API çağrılarını POSTMAN istemcisinden `curl` veya komutları kullanarak çalıştırabilirsiniz.

### <a name="enable-the-ipsec-vpn-service-on-the-logical-router"></a>Mantıksal yönlendiricide IPSec VPN hizmetini etkinleştirme

```
POST   https://192.168.110.201/api/v1/vpn/ipsec/services/
{
"resource_type": "IPSecVPNService",
"description": "Manage VPN service",
"display_name": "IPSec VPN service",
"logical_router_id": "Logical-router ID",
"ike_log_level": "INFO",
"enabled": true
}
```

### <a name="create-profiles-ike"></a>Profil oluşturma: IKE

```
POST https://192.168.110.201/api/v1/vpn/ipsec/ike-profiles
 
{
"resource_type": "IPSecVPNIKEProfile",
"description": "IKEProfile for siteA",
"display_name": "IKEProfile siteA",
"encryption_algorithms": ["AES_128"],
"ike_version": "IKE_V2",
"digest_algorithms": ["SHA2_256"],
"sa_life_time":21600,
"dh_groups": ["GROUP14"]
}
```

### <a name="create-profiles-dpd"></a>Profil oluşturma: DPD

```
POST  https://192.168.110.201/api/v1/vpn/ipsec/dpd-profiles  

{
"resource_type": "IPSecVPNDPDProfile",
"display_name": "nsx-default-dpd-profile",
"enabled": true
}
```

### <a name="create-profiles-tunnel"></a>Profil oluşturma: Tünel

```
POST  https://192.168.110.201/api/v1/vpn/ipsec/tunnel-profiles
 
{
"resource_type": "IPSecVPNTunnelProfile",
"display_name": "nsx-default-tunnel-profile",
"enable_perfect_forward_secrecy": true,
"encryption_algorithms": ["AES_GCM_128"],
"digest_algorithms": [],
"sa_life_time":3600,
"dh_groups": ["GROUP14"],
"encapsulation_mode": "TUNNEL_MODE",
"transform_protocol": "ESP",
"df_policy": "COPY"
}
```

### <a name="create-a-local-endpoint"></a>Yerel bir bitiş noktası oluşturma

``` 
POST https://192.168.110.201/api/v1/vpn/ipsec/local-endpoints
 
{
"resource_type": "IPSecVPNLocalEndpoint",
"description": "Local endpoint",
"display_name": "Local endpoint",
"local_id": "<Public IP of Loopback interface>",
"ipsec_vpn_service_id": {
"target_id": "IPSec VPN service ID"},
"local_address": "<IP of Loopback interface>",
"trust_ca_ids": [],
"trust_crl_ids": []
}
```

### <a name="create-a-peer-endpoint"></a>Eş bitiş noktası oluşturma

```
POST https://192.168.110.201/api/v1/vpn/ipsec/peer-endpoints

{
"resource_type": "IPSecVPNPeerEndpoint",
"description": "Peer endpoint for site B",
"display_name": "Peer endpoint for site B",
"connection_initiation_mode": "INITIATOR",
"authentication_mode": "PSK",
"ipsec_tunnel_profile_id": "IPSec Tunnel profile ID",
"dpd_profile_id": "DPD profile ID",
"psk":"nsx",
"ike_profile_id": "IKE profile ID",
"peer_address": "<Public IP of Standalone client",
"peer_id": "<Public IP of Standalone client>"
}
```

### <a name="create-a-route-based-vpn-session"></a>Rota tabanlı VPN oturumu oluşturma

```
POST :  https://192.168.110.201/api/v1/vpn/ipsec/sessions
 
{
"resource_type": "RouteBasedIPSecVPNSession",
"peer_endpoint_id": "Peer Endpoint ID",
"ipsec_vpn_service_id": "IPSec VPN service ID",
"local_endpoint_id": "Local Endpoint ID",
"enabled": true,
"tunnel_ports": [
{
"ip_subnets": [
{
"ip_addresses": [
 "5.5.5.1"
],
"prefix_length": 24
}
  ]
}
]
}
```

## <a name="configure-l2vpn-on-nsx-t-tier0-router"></a>NSX-T Tier0 yönlendiriciüzerinde L2VPN'i yapılandırın

Her POST çağrısından sonra aşağıdaki bilgileri doldurun. Sonraki POST aramalarında DOĞRULAR gereklidir.

```
L2VPN Service ID:
L2VPN Session ID:
Logical Port ID:
```

### <a name="create-the-l2vpn-service"></a>L2VPN hizmetini oluşturun

Yapılandırma henüz tamamlanmadığından, aşağıdaki GET komutunun çıktısı boş olacaktır.

```
GET : https://192.168.110.201/api/v1/vpn/l2vpn/services
```

Aşağıdaki POST komutu için mantıksal yönlendirici kimliği, daha önce elde edilen Tier0 DR mantıksal yönlendiricinin UUID'sidir.

```
POST : https://192.168.110.201/api/v1/vpn/l2vpn/services

{
"logical_router_id": "Logical Router ID",
"enable_full_mesh" : true
}
```

### <a name="create-the-l2vpn-session"></a>L2VPN oturumunu oluşturma

Aşağıdaki POST komutu için L2VPN hizmet kimliği yeni aldığınız kimliktir ve IPsec VPN oturum kimliği önceki bölümde elde edilen kimliktir.

``` 
POST: https://192.168.110.201/api/v1/vpn/l2vpn/sessions

{
"l2vpn_service_id" : "L2VPN service ID",
"transport_tunnels" : [
    {
    "target_id" : "IPSec VPN session ID"
    }]
}
```

Bu çağrılar gre tüneli bitiş noktası oluşturur. Durumu denetlemek için aşağıdaki komutu çalıştırın.

```
edge-2> get tunnel-port
Tunnel      : 44648dae-8566-5bc9-a065-b1c4e5c3e03f
IFUID       : 328
LOCAL       : 169.254.64.1
REMOTE      : 169.254.64.2
ENCAP       : GRE

Tunnel      : cf950ca1-5cf8-5438-9b1a-d2c8c8e7229b
IFUID       : 318
LOCAL       : 192.168.140.155
REMOTE      : 192.168.140.152
ENCAP       : GENEVE

Tunnel      : 63639321-87c5-529e-8a61-92c1939799b2
IFUID       : 304
LOCAL       : 192.168.140.155
REMOTE      : 192.168.140.156
ENCAP       : GENEVE
```

### <a name="create-logical-port-with-the-tunnel-id-specified"></a>Tünel kimliği belirtilen mantıksal bağlantı noktası oluşturma

```
    POST https://192.168.110.201/api/v1/logical-ports/

{
"resource_type": "LogicalPort",
"display_name": "Extend logicalSwitch, port for service",
"logical_switch_id": "Logical switch ID",
"admin_state" : "UP",
"attachment": {
"attachment_type":"L2VPN_SESSION",
"id":"L2VPN session ID",
"context" : {
"resource_type" : "L2VpnAttachmentContext",
    "tunnel_id" : 10
}
    }
        }
```

## <a name="obtain-the-peer-code-for-l2vpn-on-the-nsx-t-side"></a>NSX-T tarafında L2VPN için eş kodunu edinin

NSX-T bitiş noktasının eş kodunu edinin. Uzak bitiş noktasını yapılandırırken eş kodu gereklidir. L2VPN <oturum kimliği> bir önceki bölümden elde edilebilir. Daha fazla bilgi için [NSX-T 2.3 API Kılavuzu'na](https://www.vmware.com/support/nsxt/doc/nsxt_23_api.html)bakın.

```
GET https://192.168.110.201/api/v1/vpn/l2vpn/sessions/<session-id>/peer-codes
```

## <a name="deploy-the-nsx-t-standalone-client-on-premises"></a>NSX-T bağımsız istemcisini dağıtma (şirket içinde)

Dağıtmadan önce, şirket içi güvenlik duvarı kurallarınızın NSX-T T0 yönlendirici geri dönüş arabirimi için daha önce ayrılmış olan CloudSimple genel IP adresine gelen ve giden UDP 500/4500 trafiğine izin verdiğini doğrulayın. 

1. [Bağımsız NSX Edge İstemciyi İndirin](https://my.vmware.com/group/vmware/details?productId=673&rPId=33945&downloadGroup=NSX-T-230) OVF ve İndirilen paketten dosyaları bir klasöre ayıklayın.

    ![Bağımsız NSX Edge istemcisi indirin](media/l2vpn-deploy-client01.png)

2. Tüm çıkarılan dosyaların olduğu klasöre gidin. Büyük cihaz boyutu veya NSX-l2t-client-Xlarge.mf ve ekstra büyük boyutlu cihaz boyutu için NSX-l2t-client-Xlarge.ovf için tüm vmdk'ları (NSX-l2t-client-large.mf ve NSX-l2t-client-Xlarge.ovf) seçin. **İleri**'ye tıklayın.

    ![Şablonu](media/l2vpn-deploy-client02.png) ![seçin Şablonu seçin](media/l2vpn-deploy-client03.png)

3. NSX-T bağımsız istemcisi için bir ad girin ve **İleri'yi**tıklatın.

    ![Şablon adını girin](media/l2vpn-deploy-client04.png)

4. Veri deposu ayarlarına ulaşmak için gerektiği gibi **İleri'yi** tıklatın. NSX-T bağımsız istemcisi için uygun veri deposunu seçin ve **İleri'yi**tıklatın.

    ![Veri deposu seçin](media/l2vpn-deploy-client06.png)

5. NSX-T bağımsız istemcisi için Trunk (Trunk PG), Public (Uplink PG) ve HA arabirimi (Uplink PG) için doğru bağlantı noktası gruplarını seçin. **İleri**'ye tıklayın.

    ![Bağlantı noktası gruplarını seçin](media/l2vpn-deploy-client07.png)

6. **Şablonu Özelleştir** ekranında aşağıdaki ayrıntıları doldurun ve **İleri'yi**tıklatın:

    L2T'yi genişletin:

    * **Eş Adresi**. NSX-T Tier0 Loopback arabirimi için Azure CloudSimple portalında rezerve edilen IP adresini girin.
    * **Eş Kodu**. L2VPN Server dağıtımının son adımından elde edilen eş kodunu yapıştırın.
    * **Alt Arabirimler VLAN (Tünel Kimliği)**. Gerilecek VLAN Kimliğini girin. Parantez içinde (), daha önce yapılandırılan tünel kimliğini girin.

    Uplink Arabirimini Genişlet:

    * **DNS IP Adresi**. Şirket içi DNS IP adresini girin.
    * **Varsayılan Ağ Geçidi**.  Bu istemci için varsayılan ağ geçidi olarak hareket edecek VLAN varsayılan ağ geçidi girin.
    * **IP Adresi**. Bağımsız istemcinin uplink IP adresini girin.
    * **Önek Uzunluğu**. Uplink VLAN/subnet'in önek uzunluğunu girin.
    * **CLI admin/enable/root Kullanıcı Şifresi**. Admin /enable /root hesabının parolasını ayarlayın.

      ![Şablonu](media/l2vpn-deploy-client08.png)
      ![özelleştir şablonu özelleştir - daha fazla](media/l2vpn-deploy-client09.png)

7. Ayarları gözden geçirin ve **Bitir'e**tıklayın.

    ![Tam yapılandırma](media/l2vpn-deploy-client10.png)

## <a name="configure-an-on-premises-sink-port"></a>Şirket içi lavabo bağlantı noktasını yapılandırma

VPN sitelerinden birinde NSX dağıtılmamışsa, o siteye bağımsız bir NSX Edge dağıtarak bir L2 VPN yapılandırabilirsiniz. Bağımsız bir NSX Edge, NSX tarafından yönetilmeyen bir ana bilgisayarda OVF dosyası kullanılarak dağıtılır. Bu, L2 VPN istemcisi olarak çalışması için bir NSX Edge Services Gateway cihazı dağıtır.

Tek başına kenar gövde vNIC vSphere Distributed Switch'e bağlıysa, L2 VPN işlevi için promiscuous modu veya lavabo bağlantı noktası gereklidir. Promiscuous modunu kullanmak yinelenen ping'lere ve yinelenen yanıtlara neden olabilir. Bu nedenle, L2 VPN bağımsız NSX Edge yapılandırmasında lavabo bağlantı noktası modunu kullanın. VMware belgelerinde [bir lavabo bağlantı noktasını yapılandır'](https://docs.vmware.com/en/VMware-NSX-Data-Center-for-vSphere/6.4/com.vmware.nsx.admin.doc/GUID-3CDA4346-E692-4592-8796-ACBEEC87C161.html) a bakın.

## <a name="ipsec-vpn-and-l2vpn-verification"></a>IPsec VPN ve L2VPN doğrulaması

Bağımsız NSX-T Edge'den IPsec ve L2VPN oturumlarını doğrulamak için aşağıdaki komutları kullanın.

```
nsx-l2t-edge> show service ipsec
-----------------------------------------------------------------------
vShield Edge IPSec Service Status:
IPSec Server is running.
AESNI is enabled.
Total Sites: 1, 1 UP, 0 Down
Total Tunnels: 1, 1 UP, 0 Down
----------------------------------
Site:  10.250.0.111_0.0.0.0/0-104.40.21.81_0.0.0.0/0
Channel: PeerIp: 104.40.21.81    LocalIP: 10.250.0.111  Version: IKEv2  Status: UP
Tunnel: PeerSubnet: 0.0.0.0/0    LocalSubnet: 0.0.0.0/0   Status: UP
----------------------------------
```

```
nsx-l2t-edge> show service l2vpn
L2 VPN is running
----------------------------------------
L2 VPN type: Client/Spoke

SITENAME                       IPSECSTATUS          VTI                  GRE
1ecb00fb-a538-4740-b788-c9049e8cb6c6 UP                   vti-100              l2t-1
```

NSX-T Tier0 yönlendiricisinden IPsec ve L2VPN oturumlarını doğrulamak için aşağıdaki komutları kullanın.

```
edge-2> get ipsecvpn session
Total Number of Sessions: 1

IKE Session ID : 3
UUID           : 1ecb00fb-a538-4740-b788-c9049e8cb6c6
Type           : Route

Local IP       : 192.168.254.254      Peer IP        : 192.227.85.167
Local ID       : 104.40.21.81         Peer ID        : 192.227.85.167
Session Status : Up

Policy Rules
    VTI UUID       : 4bf96e3b-e50b-49cc-a16e-43a6390e3d53
    ToRule ID      : 560874406            FromRule ID    : 2708358054
    Local Subnet   : 0.0.0.0/0            Peer Subnet    : 0.0.0.0/0
    Tunnel Status  : Up
```

```
edge-2> get l2vpn session
Session       : f7147137-5dd0-47fe-9e53-fdc2b687b160
Tunnel        : b026095b-98c8-5932-96ff-dda922ffe316
IPSEC Session : 1ecb00fb-a538-4740-b788-c9049e8cb6c6
Status        : UP
```

NSX-T bağımsız istemci VM'nin şirket içi ortamda bulunduğu ESXi ana bilgisayardaki lavabo bağlantı noktasını doğrulamak için aşağıdaki komutları kullanın.

```
 [root@esxi02:~] esxcfg-vswitch -l |grep NSX
  53                  1           NSXT-client-large.eth2
  225                1           NSXT-client-large.eth1
  52                  1           NSXT-client-large.eth0
```

```
[root@esxi02:~] net-dvs -l | grep "port\ [0-9]\|SINK\|com.vmware.common.alias"
                com.vmware.common.alias = csmlab2DS ,   propType = CONFIG
        port 24:
        port 25:
        port 26:
        port 27:
        port 13:
        port 19:
        port 169:
        port 54:
        port 110:
        port 6:
        port 107:
        port 4:
        port 199:
        port 168:
        port 201:
        port 0:
        port 49:
        port 53:
        port 225:
                com.vmware.etherswitch.port.extraEthFRP =   SINK
        port 52:
```
