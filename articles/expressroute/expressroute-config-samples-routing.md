---
title: 'Azure ExpressRoute: yönlendirici yapılandırma örnekleri'
description: Azure ExpressRoute ile çalışmak için örnek olarak Cisco IOS-XE ve Juniper MX Series yönlendiricilerine yönelik bu arabirimi ve yönlendirme yapılandırma örneklerini kullanın.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: article
ms.date: 03/26/2020
ms.author: duau
ms.openlocfilehash: 3bc850f02884ae0547c2ecf56a46a57a4e66a752
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89397415"
---
# <a name="router-configuration-samples-to-set-up-and-manage-routing"></a>Yönlendirmeyi ayarlamak ve yönetmek için yönlendirici yapılandırma örnekleri
Bu sayfa, Azure ExpressRoute ile çalışırken Cisco IOS-XE ve Juniper MX Series yönlendiricileri için arabirim ve yönlendirme yapılandırma örnekleri sağlar.

> [!IMPORTANT]
> Bu sayfadaki örnekler yalnızca kılavuzluk içindir. Gereksinimlerinizi karşılayacak uygun konfigürasyonları bulmak için satıcının satış/teknik ekibi ve ağ ekibiniz ile çalışmanız gerekir. Microsoft bu sayfada listelenen yapılandırmalara ilişkin sorunları desteklememektedir. Destek sorunları için cihaz satıcınızla iletişim kurun.
> 
> 

## <a name="mtu-and-tcp-mss-settings-on-router-interfaces"></a>Yönlendirici arabirimlerinde MTU ve TCP ve TCP ayarları
ExpressRoute arabirimi için en yüksek iletim birimi (MTU), bir yönlendiricide Ethernet arabirimi için tipik varsayılan MTU olan 1500 ' dir. Yönlendiricinizin varsayılan olarak farklı bir MTU değeri yoksa, yönlendirici arabiriminde bir değer belirtmeniz gerekmez.

Azure VPN ağ geçidinden farklı olarak, bir ExpressRoute bağlantı hattı için TCP en büyük kesim boyutu (en fazla) belirtilmelidir.

Bu makaledeki yönlendirici yapılandırma örnekleri tüm eşlemeler için geçerlidir. Yönlendirme hakkında daha fazla ayrıntı için [ExpressRoute eşayarlarını](expressroute-circuit-peerings.md) ve [ExpressRoute yönlendirme gereksinimlerini](expressroute-routing.md) gözden geçirin.


## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS-XE tabanlı yönlendiriciler
Bu bölümdeki örnekler, IOS-XE işletim sistemi ailesini çalıştıran tüm yönlendiriciler için geçerlidir.

### <a name="configure-interfaces-and-subinterfaces"></a>Arabirimleri ve alt arabirimleri yapılandırma
Microsoft 'a bağlandığınız her yönlendiricide eşleme başına bir arabirim olmalıdır. Bir alt arabirim, VLAN KIMLIĞI veya yığılmış bir VLAN kimlikleri ve bir IP adresi ile tanımlanabilir.

**Dot1Q arabirimi tanımı**

Bu örnek, tek bir VLAN KIMLIĞI olan bir alt arabirim için alt arabirim tanımını sağlar. VLAN KIMLIĞI, eşleme başına benzersizdir. IPv4 adresinizin son sekizlisinin her zaman tek bir sayı olması gerekir.

```console
interface GigabitEthernet<Interface_Number>.<Number>
 encapsulation dot1Q <VLAN_ID>
 ip address <IPv4_Address><Subnet_Mask>
```

**Qınq arabirimi tanımı**

Bu örnek, iki VLAN kimliği olan bir alt arabirim için alt arabirim tanımını sağlar. Dış VLAN KIMLIĞI (s-Tag) kullanılıyorsa, tüm eşlemeler genelinde aynı kalır. İç VLAN KIMLIĞI (c-Tag) eşleme başına benzersizdir. IPv4 adresinizin son sekizlisinin her zaman tek bir sayı olması gerekir.

```console
interface GigabitEthernet<Interface_Number>.<Number>
 encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
 ip address <IPv4_Address><Subnet_Mask>
```

### <a name="set-up-ebgp-sessions"></a>EBGP oturumlarını ayarlama
Her eşleme için Microsoft ile bir BGP oturumu ayarlamanız gerekir. Aşağıdaki örneği kullanarak bir BGP oturumu ayarlayın. Alt ağınız için kullandığınız IPv4 adresi a. b. c. d ise, BGP komşusunun (Microsoft) IP adresi a. b. c. d + 1 olur. BGP komşusunun IPv4 adresinin son sekizlisinin her zaman çift sayı olması gerekir.

```console
router bgp <Customer_ASN>
 bgp log-neighbor-changes
 neighbor <IP#2_used_by_Azure> remote-as 12076
 !
 address-family ipv4
 neighbor <IP#2_used_by_Azure> activate
 exit-address-family
!
```

### <a name="set-up-prefixes-to-be-advertised-over-the-bgp-session"></a>BGP oturumu üzerinden tanıtımak üzere ön ekleri ayarlama
Aşağıdaki örneği kullanarak, Microsoft 'a Select öneklerini tanıtmak için yönlendiricinizi yapılandırın.

```console
router bgp <Customer_ASN>
 bgp log-neighbor-changes
 neighbor <IP#2_used_by_Azure> remote-as 12076
 !
 address-family ipv4
  network <Prefix_to_be_advertised> mask <Subnet_mask>
  neighbor <IP#2_used_by_Azure> activate
 exit-address-family
!
```

### <a name="route-maps"></a>Rota haritaları
Ağınıza yayılan önekleri filtrelemek için yol haritaları ve ön ek listelerini kullanın. Aşağıdaki örneğe bakın ve uygun ön ek listelerinin ayarlanmış olduğundan emin olun.

```console
router bgp <Customer_ASN>
 bgp log-neighbor-changes
 neighbor <IP#2_used_by_Azure> remote-as 12076
 !
 address-family ipv4
  network <Prefix_to_be_advertised> mask <Subnet_mask>
  neighbor <IP#2_used_by_Azure> activate
  neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
 exit-address-family
!
route-map <MS_Prefixes_Inbound> permit 10
 match ip address prefix-list <MS_Prefixes>
!
```

### <a name="configure-bfd"></a>BFD 'yi yapılandırma

BFD 'yi iki yerde yapılandıracaksınız: biri arabirim düzeyinde diğeri BGP düzeyinde diğeri. Buradaki örnek Qınq arabirimine yöneliktir. 

```console
interface GigabitEthernet<Interface_Number>.<Number>
 bfd interval 300 min_rx 300 multiplier 3
 encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
 ip address <IPv4_Address><Subnet_Mask>

router bgp <Customer_ASN>
 bgp log-neighbor-changes
 neighbor <IP#2_used_by_Azure> remote-as 12076
 !
 address-family ipv4
  neighbor <IP#2_used_by_Azure> activate
  neighbor <IP#2_used_by_Azure> fall-over bfd
 exit-address-family
!
```


## <a name="juniper-mx-series-routers"></a>Juniper MX serisi yönlendiricileri
Bu bölümdeki örnekler, herhangi bir Juniper MX Series yönlendiricisi için geçerlidir.

### <a name="configure-interfaces-and-subinterfaces"></a>Arabirimleri ve alt arabirimleri yapılandırma

**Dot1Q arabirimi tanımı**

Bu örnek, tek bir VLAN KIMLIĞI olan bir alt arabirim için alt arabirim tanımını sağlar. VLAN KIMLIĞI, eşleme başına benzersizdir. IPv4 adresinizin son sekizlisinin her zaman tek bir sayı olması gerekir.

```console
    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }
```


**Qınq arabirimi tanımı**

Bu örnek, iki VLAN kimliği olan bir alt arabirim için alt arabirim tanımını sağlar. Dış VLAN KIMLIĞI (s-Tag) kullanılıyorsa, tüm eşlemeler genelinde aynı kalır. İç VLAN KIMLIĞI (c-Tag) eşleme başına benzersizdir. IPv4 adresinizin son sekizlisinin her zaman tek bir sayı olması gerekir.

```console
    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           
```

### <a name="set-up-ebgp-sessions"></a>EBGP oturumlarını ayarlama
Her eşleme için Microsoft ile bir BGP oturumu ayarlamanız gerekir. Aşağıdaki örneği kullanarak bir BGP oturumu ayarlayın. Alt ağınız için kullandığınız IPv4 adresi a. b. c. d ise, BGP komşusunun (Microsoft) IP adresi a. b. c. d + 1 olur. BGP komşusunun IPv4 adresinin son sekizlisinin her zaman çift sayı olması gerekir.

```console
    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }
```

### <a name="set-up-prefixes-to-be-advertised-over-the-bgp-session"></a>BGP oturumu üzerinden tanıtımak üzere ön ekleri ayarlama
Aşağıdaki örneği kullanarak, Microsoft 'a Select öneklerini tanıtmak için yönlendiricinizi yapılandırın.

```console
    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
                route-filter; 
                <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>;
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }
```

### <a name="route-policies"></a>Yol ilkeleri
Ağınıza yayılan önekleri filtrelemek için yol haritaları ve ön ek listelerini kullanabilirsiniz. Aşağıdaki örneğe bakın ve uygun ön ek listelerinin ayarlanmış olduğundan emin olun.

```console
    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
                    prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>;
                import <MS_Prefixes_Inbound>;
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }
```

### <a name="configure-bfd"></a>BFD 'yi yapılandırma
BFD 'yi yalnızca protokol BGP bölümü altında yapılandırın.

```console
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
                bfd-liveness-detection {
                       minimum-interval 3000;
                       multiplier 3;
                }
            }                               
        }                                   
    }
```

### <a name="configure-macsec"></a>MACSec yapılandırma
MACSec yapılandırması için, bağlantı Ilişkilendirme anahtarı (CAK) ve bağlantı Ilişkisi anahtar adı (CKN), PowerShell komutları aracılığıyla yapılandırılmış değerlerle eşleşmelidir.

```console
    security {
        macsec {
            connectivity-association <Connectivity_Association_Name> {
                cipher-suite gcm-aes-xpn-128;
                security-mode static-cak;
                pre-shared-key {
                    ckn <Connectivity_Association_Key_Name>;
                    cak <Connectivity_Association_Key>; ## SECRET-DATA
                }
            }
            interfaces {
                <Interface_Number> {
                    connectivity-association <Connectivity_Association_Name>;
                }
            }
        }
    }
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla ayrıntı için bkz. [ExpressRoute SSS](expressroute-faqs.md).



