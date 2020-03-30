---
title: Cisco ASA aygıtlarını Azure VPN ağ geçitlerine bağlamak için örnek yapılandırma
description: Bu makalede, Cisco ASA aygıtlarını Azure VPN ağ geçitlerine bağlamak için örnek bir yapılandırma sağlanmaktadır.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 10/19/2018
ms.author: yushwang
ms.openlocfilehash: 96e5c26ea7b5f1baa33fd8830491ee3aa1e60221
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75778091"
---
# <a name="sample-configuration-cisco-asa-device-ikev2no-bgp"></a>Örnek yapılandırma: Cisco ASA cihazı (IKEv2/no BGP)
Bu makalede, Cisco Adaptive Security Appliance (ASA) aygıtlarını Azure VPN ağ geçitlerine bağlamak için örnek yapılandırmalar sağlanmaktadır. Bu örnek, Border Gateway Protocol (BGP) olmadan IKEv2 çalıştıran Cisco ASA aygıtları için geçerlidir. 

## <a name="device-at-a-glance"></a>Bir bakışta cihaz

|                        |                                   |
| ---                    | ---                               |
| Cihaz satıcısı          | Cisco                             |
| Cihaz modeli           | ASA                               |
| Hedef sürüm         | 8.4 ve sonrası                     |
| Test edilmiş model           | ASA 5505                          |
| Test edilmiş sürüm         | 9.2                               |
| IKE sürümü            | IKEv2                             |
| BGP                    | Hayır                                |
| Azure VPN ağ geçidi türü | Rota tabanlı VPN ağ geçidi           |
|                        |                                   |

> [!NOTE]
> Örnek yapılandırma, Cisco ASA aygıtını Azure **rota tabanlı** bir VPN ağ geçidine bağlar. Bağlantı, [bu makalede](vpn-gateway-connect-multiple-policybased-rm-ps.md)açıklandığı gibi **UsePolicyBasedTrafficSelectors** seçeneği ile özel bir IPsec/IKE ilkesi kullanır.
>
> Örnek, ASA aygıtlarının **IKEv2** ilkesini VTI tabanlı değil, erişim listesi tabanlı yapılandırmalarla kullanmasını gerektirir. IKEv2 ilkesinin şirket içi VPN aygıtlarınızda desteklendiğini doğrulamak için VPN cihaz satıcınızın belirtimlerine başvurun.


## <a name="vpn-device-requirements"></a>VPN cihaz gereksinimleri
Azure VPN ağ geçitleri, Siteden Siteye (S2S) VPN tünelleri oluşturmak için standart IPsec/IKE protokol paketlerini kullanır. Azure VPN ağ geçitleri için ayrıntılı IPsec/IKE protokol parametreleri ve varsayılan şifreleme algoritmaları için [VPN aygıtları hakkında](vpn-gateway-about-vpn-devices.md)bilgi edinin.

> [!NOTE]
> İsteğe bağlı olarak, [şifreleme gereksinimleri hakkında'da](vpn-gateway-about-compliance-crypto.md)açıklandığı gibi, belirli bir bağlantı için şifreleme algoritmaları ve anahtar güçlü yönlerinin tam bir birleşimini belirtebilirsiniz. Algoritmalar ve anahtar güçlü yangüçlerinin tam bir birleşimini belirtirseniz, VPN aygıtlarınızda ilgili özellikleri kullandığınızdan emin olun.

## <a name="single-vpn-tunnel"></a>Tek VPN tüneli
Bu yapılandırma, Azure VPN ağ geçidi ile şirket içi VPN aygıtı arasında tek bir S2S VPN tünelinden oluşur. BGP'yi isteğe bağlı olarak VPN tüneli üzerinden yapılandırabilirsiniz.

![Tek S2S VPN tüneli](./media/vpn-gateway-3rdparty-device-config-cisco-asa/singletunnel.png)

Azure yapılandırmalarını oluşturmak için adım adım talimatlar için [Tek VPN tünel kurulumuna](vpn-gateway-3rdparty-device-config-overview.md#singletunnel)bakın.

### <a name="virtual-network-and-vpn-gateway-information"></a>Sanal ağ ve VPN ağ geçidi bilgileri
Bu bölümde örnek parametreleri listelenir.

| **Parametre**                | **Değer**                    |
| ---                          | ---                          |
| Sanal ağ adresi önekleri        | 10.11.0.0/16<br>10.12.0.0/16 |
| Azure VPN ağ geçidi IP         | Azure_Gateway_Public_IP      |
| Şirket içi adres önekleri | 10.51.0.0/16<br>10.52.0.0/16 |
| Şirket içi VPN cihazı IP    | OnPrem_Device_Public_IP     |
| * Sanal ağ BGP ASN                | 65010                        |
| * Azure BGP eş IP           | 10.12.255.30                 |
| * Şirket içi BGP ASN         | 65050                        |
| * Şirket içi BGP eş IP     | 10.52.255.254                |
|                              |                              |

\*Yalnızca BGP için isteğe bağlı parametre.

### <a name="ipsecike-policy-and-parameters"></a>IPsec/IKE politikası ve parametreleri
Aşağıdaki tabloda iPsec/IKE algoritmaları ve örnekte kullanılan parametreler listelenir. VPN cihaz modelleri ve firmware sürümleri için desteklenen algoritmaları doğrulamak için VPN cihaz özelliklerine başvurun.

| **IPsec/IKEv2**  | **Değer**                            |
| ---              | ---                                  |
| IKEv2 Şifrelemesi | AES256                               |
| IKEv2 Bütünlüğü  | SHA384                               |
| DH Grubu         | DHGroup24                            |
| * IPsec Şifreleme | AES256                               |
| * IPsec Bütünlüğü  | SHA1                                 |
| PFS Grubu        | PFS24                                |
| QM SA Yaşam Süresi   | 7.200 saniye                         |
| Trafik Seçicisi | UsePolicyBasedTrafficSelectors $True |
| Önceden Paylaşılan Anahtar   | Önceden Paylaşılan Anahtar                         |
|                  |                                      |

\*Bazı cihazlarda, IPsec Şifreleme algoritması AES-GCM olduğunda IPsec Bütünlüğü boş bir değer olmalıdır.

### <a name="asa-device-support"></a>ASA cihaz desteği

* IKEv2 desteği ASA sürüm 8.4 ve sonrası gerektirir.

* GRUP 5'in ötesindeKI DH Grubu ve PFS Grubu desteği ASA 9.x gerektirir.

* SHA-256, SHA-384 veya SHA-512 ile AES-GCM ve IPsec Bütünlüğü ile IPsec Şifreleme desteği, ASA sürüm 9.x gerektirir. Bu destek gereksinimi yeni ASA aygıtları için geçerlidir. Yayın sırasında, ASA modelleri 5505, 5510, 5520, 5540, 5550 ve 5580 bu algoritmaları desteklemez. VPN cihaz modelleri ve firmware sürümleri için desteklenen algoritmaları doğrulamak için VPN cihaz özelliklerine başvurun.


### <a name="sample-device-configuration"></a>Örnek aygıt yapılandırması
Komut dosyası, önceki bölümlerde açıklanan yapılandırma ve parametreleri temel alan bir örnek sağlar. S2S VPN tünel yapılandırması aşağıdaki parçalardan oluşur:

1. Arayüzler ve rotalar
2. Erişim listeleri
3. IKE ilkesi ve parametreleri (faz 1 veya ana mod)
4. IPsec politikası ve parametreleri (faz 2 veya hızlı mod)
5. TCP MSS bağlama gibi diğer parametreler

> [!IMPORTANT]
> Örnek komut dosyasını kullanmadan önce aşağıdaki adımları tamamlayın. Komut dosyasındaki yer tutucu değerlerini yapılandırmanız için aygıt ayarlarıyla değiştirin.

* Hem iç hem de dış arabirimler için arabirim yapılandırmasını belirtin.
* İç/özel ve dış/genel ağlarınızın güzergahlarını belirleyin.
* Tüm adların ve ilke numaralarının cihazınızda benzersiz olduğundan emin olun.
* Şifreleme algoritmalarının cihazınızda desteklendirilmesini sağlayın.
* Yapılandırmanız için aşağıdaki **yer tutucu değerlerini** gerçek değerlerle değiştirin:
  - Dış arayüz adı: **dış**
  - **Azure_Gateway_Public_IP**
  - **OnPrem_Device_Public_IP**
  - IKE: **Pre_Shared_Key**
  - Sanal ağ ve yerel ağ ağ geçidi adları: **VNetName** ve **LNGName**
  - Sanal ağ ve şirket içi ağ adresi **önekleri**
  - Uygun **ağ maskeleri**

#### <a name="sample-script"></a>Örnek betik

```
! Sample ASA configuration for connecting to Azure VPN gateway
!
! Tested hardware: ASA 5505
! Tested version:  ASA version 9.2(4)
!
! Replace the following place holders with your actual values:
!   - Interface names - default are "outside" and "inside"
!   - <Azure_Gateway_Public_IP>
!   - <OnPrem_Device_Public_IP>
!   - <Pre_Shared_Key>
!   - <VNetName>*
!   - <LNGName>* ==> LocalNetworkGateway - the Azure resource that represents the
!     on-premises network, specifies network prefixes, device public IP, BGP info, etc.
!   - <PrivateIPAddress> ==> Replace it with a private IP address if applicable
!   - <Netmask> ==> Replace it with appropriate netmasks
!   - <Nexthop> ==> Replace it with the actual nexthop IP address
!
! (*) Must be unique names in the device configuration
!
! ==> Interface & route configurations
!
!     > <OnPrem_Device_Public_IP> address on the outside interface or vlan
!     > <PrivateIPAddress> on the inside interface or vlan; e.g., 10.51.0.1/24
!     > Route to connect to <Azure_Gateway_Public_IP> address
!
!     > Example:
!
!       interface Ethernet0/0
!        switchport access vlan 2
!       exit
!
!       interface vlan 1
!        nameif inside
!        security-level 100
!        ip address <PrivateIPAddress> <Netmask>
!       exit
!
!       interface vlan 2
!        nameif outside
!        security-level 0
!        ip address <OnPrem_Device_Public_IP> <Netmask>
!       exit
!
!       route outside 0.0.0.0 0.0.0.0 <NextHop IP> 1
!
! ==> Access lists
!
!     > Most firewall devices deny all traffic by default. Create access lists to
!       (1) Allow S2S VPN tunnels between the ASA and the Azure gateway public IP address
!       (2) Construct traffic selectors as part of IPsec policy or proposal
!
access-list outside_access_in extended permit ip host <Azure_Gateway_Public_IP> host <OnPrem_Device_Public_IP>
!
!     > Object group that consists of all VNet prefixes (e.g., 10.11.0.0/16 &
!       10.12.0.0/16)
!
object-group network Azure-<VNetName>
 description Azure virtual network <VNetName> prefixes
 network-object 10.11.0.0 255.255.0.0
 network-object 10.12.0.0 255.255.0.0
exit
!
!     > Object group that corresponding to the <LNGName> prefixes.
!       E.g., 10.51.0.0/16 and 10.52.0.0/16. Note that LNG = "local network gateway".
!       In Azure network resource, a local network gateway defines the on-premises
!       network properties (address prefixes, VPN device IP, BGP ASN, etc.)
!
object-group network <LNGName>
 description On-Premises network <LNGName> prefixes
 network-object 10.51.0.0 255.255.0.0
 network-object 10.52.0.0 255.255.0.0
exit
!
!     > Specify the access-list between the Azure VNet and your on-premises network.
!       This access list defines the IPsec SA traffic selectors.
!
access-list Azure-<VNetName>-acl extended permit ip object-group <LNGName> object-group Azure-<VNetName>
!
!     > No NAT required between the on-premises network and Azure VNet
!
nat (inside,outside) source static <LNGName> <LNGName> destination static Azure-<VNetName> Azure-<VNetName>
!
! ==> IKEv2 configuration
!
!     > General IKEv2 configuration - enable IKEv2 for VPN
!
group-policy DfltGrpPolicy attributes
 vpn-tunnel-protocol ikev1 ikev2
exit
!
crypto isakmp identity address
crypto ikev2 enable outside
!
!     > Define IKEv2 Phase 1/Main Mode policy
!       - Make sure the policy number is not used
!       - integrity and prf must be the same
!       - DH group 14 and above require ASA version 9.x.
!
crypto ikev2 policy 1
 encryption       aes-256
 integrity        sha384
 prf              sha384
 group            24
 lifetime seconds 86400
exit
!
!     > Set connection type and pre-shared key
!
tunnel-group <Azure_Gateway_Public_IP> type ipsec-l2l
tunnel-group <Azure_Gateway_Public_IP> ipsec-attributes
 ikev2 remote-authentication pre-shared-key <Pre_Shared_Key> 
 ikev2 local-authentication  pre-shared-key <Pre_Shared_Key> 
exit
!
! ==> IPsec configuration
!
!     > IKEv2 Phase 2/Quick Mode proposal
!       - AES-GCM and SHA-2 requires ASA version 9.x on newer ASA models. ASA
!         5505, 5510, 5520, 5540, 5550, 5580 are not supported.
!       - ESP integrity must be null if AES-GCM is configured as ESP encryption
!
crypto ipsec ikev2 ipsec-proposal AES-256
 protocol esp encryption aes-256
 protocol esp integrity  sha-1
exit
!
!     > Set access list & traffic selectors, PFS, IPsec proposal, SA lifetime
!       - This sample uses "Azure-<VNetName>-map" as the crypto map name
!       - ASA supports only one crypto map per interface, if you already have
!         an existing crypto map assigned to your outside interface, you must use
!         the same crypto map name, but with a different sequence number for
!         this policy
!       - "match address" policy uses the access-list "Azure-<VNetName>-acl" defined 
!         previously
!       - "ipsec-proposal" uses the proposal "AES-256" defined previously 
!       - PFS groups 14 and beyond requires ASA version 9.x.
!
crypto map Azure-<VNetName>-map 1 match address Azure-<VNetName>-acl
crypto map Azure-<VNetName>-map 1 set pfs group24
crypto map Azure-<VNetName>-map 1 set peer <Azure_Gateway_Public_IP>
crypto map Azure-<VNetName>-map 1 set ikev2 ipsec-proposal AES-256
crypto map Azure-<VNetName>-map 1 set security-association lifetime seconds 7200
crypto map Azure-<VNetName>-map interface outside
!
! ==> Set TCP MSS to 1350
!
sysopt connection tcpmss 1350
!
```

## <a name="simple-debugging-commands"></a>Basit hata ayıklama komutları

Hata ayıklama amacıyla aşağıdaki ASA komutlarını kullanın:

* IPsec veya IKE güvenlik ilişkisini (SA) göster:
    ```
    show crypto ipsec sa
    show crypto ikev2 sa
    ```

* Hata ayıklama modunu girin:
    ```
    debug crypto ikev2 platform <level>
    debug crypto ikev2 protocol <level>
    ```
    Komutlar `debug` konsolda önemli çıktı oluşturabilir.

* Aygıttaki geçerli yapılandırmaları göster:
    ```
    show run
    ```
    Aygıt `show` yapılandırmasının belirli bölümlerini listelemek için alt komutları kullanın( örneğin:
    ```
    show run crypto
    show run access-list
    show run tunnel-group
    ```

## <a name="next-steps"></a>Sonraki adımlar
Etkin-etkin binalar arası ve VNet'e vnet bağlantılarını yapılandırmak için [bkz.](vpn-gateway-activeactive-rm-powershell.md)
