---
title: Cisco ASA cihazlarını Azure VPN ağ geçitlerine bağlamak için örnek yapılandırma
description: Bu makalede, Cisco ASA cihazlarını Azure VPN ağ geçitlerine bağlamak için örnek bir yapılandırma sağlanmaktadır.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 10/19/2018
ms.author: yushwang
ms.openlocfilehash: f693f6a40b87d024430c7626736fab0d0a032238
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87082010"
---
# <a name="sample-configuration-cisco-asa-device-ikev2no-bgp"></a>Örnek yapılandırma: Cisco ASA cihazı (Ikev2/BGP yok)
Bu makalede Cisco Uyarlamalı güvenlik gereci (ASA) cihazlarını Azure VPN ağ geçitlerine bağlamak için örnek yapılandırma sağlanmaktadır. Örnek, Sınır Ağ Geçidi Protokolü (BGP) olmadan Ikev2 çalıştıran Cisco ASA cihazları için geçerlidir. 

## <a name="device-at-a-glance"></a>Tek bakışta cihaz

* Cihaz satıcısı: **Cisco**
* Cihaz modeli: **asa**           
* Hedef sürüm: **8,4 ve üzeri**
* Test edilen model: **ASA 5505**
* Test edilen sürüm: **9,2**             
* IKE sürümü: **Ikev2**                  
* BGP: **No**      
* Azure VPN ağ geçidi türü: **Rota tabanlı VPN Gateway**

> [!NOTE]
> Örnek yapılandırma, bir Cisco ASA cihazını Azure **Rota tabanlı** bir VPN ağ geçidine bağlar. Bağlantı, [Bu makalede](vpn-gateway-connect-multiple-policybased-rm-ps.md)açıklandığı gibi **UsePolicyBasedTrafficSelectors** seçeneğiyle özel bir IPSec/IKE ilkesi kullanır.
>
> Örnek **, asa cihazlarının VNET ilkesini** VTI tabanlı değil erişim-liste tabanlı yapılandırmalara kullanmasını gerektirir. Ikev2 ilkesinin şirket içi VPN cihazlarınızda desteklendiğinden emin olmak için VPN cihaz satıcınızın belirtimlerine başvurun.


## <a name="vpn-device-requirements"></a>VPN cihazı gereksinimleri
Azure VPN ağ geçitleri, siteden siteye (S2S) VPN tünelleri oluşturmak için standart IPSec/ıKE protokol paketlerini kullanır. Ayrıntılı IPSec/ıKE protokol parametreleri ve Azure VPN ağ geçitleri için varsayılan şifreleme algoritmaları için bkz. [VPN cihazları hakkında](vpn-gateway-about-vpn-devices.md).

> [!NOTE]
> İsteğe bağlı olarak, belirli bir bağlantı için şifreleme algoritmaları ve anahtar güçlerinin tam birleşimini, [Şifreleme gereksinimleri hakkında](vpn-gateway-about-compliance-crypto.md)bölümünde açıklandığı gibi belirtebilirsiniz. Algoritmaların ve anahtar güçlerinin tam bir birleşimini belirtirseniz, VPN cihazlarınızda ilgili belirtimleri kullandığınızdan emin olun.

## <a name="single-vpn-tunnel"></a>Tek VPN tüneli
Bu yapılandırma, bir Azure VPN ağ geçidi ve şirket içi VPN cihazı arasındaki tek bir S2S VPN tünelinden oluşur. BGP 'yi VPN tüneli genelinde isteğe bağlı olarak yapılandırabilirsiniz.

![Tek S2S VPN tüneli](./media/vpn-gateway-3rdparty-device-config-cisco-asa/singletunnel.png)

Azure yapılandırmalarının derlenmesi için adım adım yönergeler için bkz. [tek VPN tüneli kurulumu](vpn-gateway-3rdparty-device-config-overview.md#singletunnel).

### <a name="virtual-network-and-vpn-gateway-information"></a>Sanal ağ ve VPN ağ geçidi bilgileri
Bu bölümde örnek için parametreler listelenir.

| **Parametre**                | **Değer**                    |
| ---                          | ---                          |
| Sanal ağ adresi önekleri        | 10.11.0.0/16<br>10.12.0.0/16 |
| Azure VPN ağ geçidi IP 'si         | Azure_Gateway_Public_IP      |
| Şirket içi adres ön ekleri | 10.51.0.0/16<br>10.52.0.0/16 |
| Şirket içi VPN cihaz IP 'si    | OnPrem_Device_Public_IP     |
| * Sanal ağ BGP ASN                | 65010                        |
| * Azure BGP eş IP 'si           | 10.12.255.30                 |
| * Şirket içi BGP ASN         | 65050                        |
| * Şirket içi BGP eşi IP 'si     | 10.52.255.254                |
|                              |                              |

\*Yalnızca BGP için isteğe bağlı parametre.

### <a name="ipsecike-policy-and-parameters"></a>IPSec/ıKE ilkesi ve parametreleri
Aşağıdaki tabloda, örnekte kullanılan IPSec/ıKE algoritmaları ve parametreleri listelenmektedir. VPN cihaz modelleriniz ve bellenim sürümleriniz için desteklenen algoritmaların doğrulanması için VPN cihazı belirtimlerine başvurun.

| **IPsec/IKEv2**  | **Değer**                            |
| ---              | ---                                  |
| IKEv2 Şifrelemesi | AES256                               |
| IKEv2 Bütünlüğü  | SHA384                               |
| DH Grubu         | DHGroup24                            |
| * IPSec şifrelemesi | AES256                               |
| * IPSec bütünlüğü  | SHA1                                 |
| PFS Grubu        | PFS24                                |
| QM SA Yaşam Süresi   | 7.200 saniye                         |
| Trafik Seçicisi | UsePolicyBasedTrafficSelectors $True |
| Önceden Paylaşılan Anahtar   | PreSharedKey                         |
|                  |                                      |

\*IPSec şifreleme algoritması AES-GCM olduğunda, bazı cihazlarda IPSec bütünlüğü null bir değer olmalıdır.

### <a name="asa-device-support"></a>ASA cihaz desteği

* Ikev2 desteği, ASA sürüm 8,4 ve üstünü gerektirir.

* Grup 5 ' in ötesinde DH grubu ve PFS Grubu desteği, ASA sürüm 9. x gerektirir.

* AES-GCM ve IP bütünlüğü ile SHA-256, SHA-384 veya SHA-512 ile IPSec şifrelemesi için destek, ASA sürüm 9. x gerektirir. Bu destek gereksinimi, daha yeni ASA cihazları için geçerlidir. Yayın sırasında, ASA modelleri 5505, 5510, 5520, 5540, 5550 ve 5580 bu algoritmaları desteklemez. VPN cihaz modelleriniz ve bellenim sürümleriniz için desteklenen algoritmaların doğrulanması için VPN cihazı belirtimlerine başvurun.


### <a name="sample-device-configuration"></a>Örnek cihaz yapılandırması
Betiği, önceki bölümlerde açıklanan yapılandırma ve parametrelere dayalı bir örnek sağlar. S2S VPN tüneli yapılandırması aşağıdaki bölümlerden oluşur:

1. Arabirimler ve rotalar
2. Erişim listeleri
3. IKE ilkesi ve parametreleri (Aşama 1 veya ana mod)
4. IPSec ilkesi ve parametreleri (Aşama 2 veya hızlı mod)
5. TCP gibi diğer parametreler

> [!IMPORTANT]
> Örnek betiği kullanmadan önce aşağıdaki adımları izleyin. Betikteki yer tutucu değerlerini yapılandırmanızın cihaz ayarlarıyla değiştirin.

* Hem iç hem de dış arabirimler için arabirim yapılandırmasını belirtin.
* İç/özel ve dış/ortak ağlarının yollarını belirler.
* Tüm adların ve ilke numaralarının cihazınızda benzersiz olduğundan emin olun.
* Şifreleme algoritmalarının cihazınızda desteklendiğinden emin olun.
* Aşağıdaki **yer tutucu değerlerini** yapılandırmanızın gerçek değerleriyle değiştirin:
  - Dış arabirim adı: **dışında**
  - **Azure_Gateway_Public_IP**
  - **OnPrem_Device_Public_IP**
  - IKE: **Pre_Shared_Key**
  - Sanal ağ ve yerel ağ geçidi adları: **vgateway** ve **lngname**
  - Sanal ağ ve şirket içi ağ adresi **önekleri**
  - Doğru **netmaskeleri**

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

* IPSec veya ıKE güvenlik ilişkilendirmesini (SA) göster:
    ```
    show crypto ipsec sa
    show crypto ikev2 sa
    ```

* Hata ayıklama moduna gir:
    ```
    debug crypto ikev2 platform <level>
    debug crypto ikev2 protocol <level>
    ```
    `debug`Komutlar konsolda önemli bir çıktı oluşturabilir.

* Cihazdaki geçerli konfigürasyonları göster:
    ```
    show run
    ```
    `show`Cihaz yapılandırmasının belirli kısımlarını listelemek için alt komutları kullanın, örneğin:
    ```
    show run crypto
    show run access-list
    show run tunnel-group
    ```

## <a name="next-steps"></a>Sonraki adımlar
Etkin-etkin şirketler arası ve VNet-VNet bağlantılarını yapılandırmak için bkz. [Active-ACTIVE VPN ağ geçitlerini yapılandırma](vpn-gateway-activeactive-rm-powershell.md).
