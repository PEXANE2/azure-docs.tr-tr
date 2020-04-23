---
title: 'Azure ExpressRoute: Yönlendirici yapılandırma örnekleri'
description: Bu sayfa, Cisco ve Ardıç yönlendiricileri için yönlendirici config örnekleri sağlar.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 03/26/2020
ms.author: osamaz
ms.openlocfilehash: 3603bc45b920dc62eb8bf6f2eb8557f98e21638e
ms.sourcegitcommit: 75089113827229663afed75b8364ab5212d67323
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82024821"
---
# <a name="router-configuration-samples-to-set-up-and-manage-routing"></a>Yönlendirmeyi ayarlamak ve yönetmek için yönlendirici yapılandırma örnekleri
Bu sayfa, Azure ExpressRoute ile çalışırken Cisco IOS-XE ve Juniper MX serisi yönlendiricileri için arabirim ve yönlendirme yapılandırma örnekleri sağlar.

> [!IMPORTANT]
> Bu sayfadaki örnekler tamamen rehberlik içindir. İhtiyaçlarınızı karşılayacak uygun yapılandırmaları bulmak için satıcınızın satış/teknik ekibi ve ağ ekibinizle birlikte çalışmanız gerekir. Microsoft, bu sayfada listelenen yapılandırmalarla ilgili sorunları desteklemez. Destek sorunları için aygıt satıcınıza başvurun.
> 
> 

## <a name="mtu-and-tcp-mss-settings-on-router-interfaces"></a>Yönlendirici arabirimlerinde MTU ve TCP MSS ayarları
ExpressRoute arabirimi için maksimum iletim birimi (MTU), yönlendiricideki bir Ethernet arabirimi için tipik varsayılan MTU olan 1500'dür. Yönlendiriciniz varsayılan olarak farklı bir MTU'ya sahip değilse, yönlendirici arabiriminde bir değer belirtmenize gerek yoktur.

Azure VPN ağ geçidinin aksine, ExpressRoute devresi için TCP maksimum segment boyutunun (MSS) belirtilmesi gerekmez.

Bu makaledeki yönlendirici yapılandırma örnekleri tüm eşlemeler için geçerlidir. Yönlendirme hakkında daha fazla bilgi için [ExpressRoute eşlemelerini](expressroute-circuit-peerings.md) ve [ExpressRoute yönlendirme gereksinimlerini](expressroute-routing.md) gözden geçirin.


## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS-XE tabanlı yönlendiriciler
Bu bölümdeki örnekler, IOS-XE OS ailesini çalıştıran tüm yönlendiriciler için geçerlidir.

### <a name="configure-interfaces-and-subinterfaces"></a>Arabirimleri ve alt arabirimleri yapılandırma
Microsoft'a bağlandığınız her yönlendiricide her eşleme başına bir alt arabirimeye ihtiyacınız vardır. Bir alt arabirim, bir VLAN kimliği veya yığılmış bir VLAN kimliği çifti ve ip adresi ile tanımlanabilir.

**Dot1Q arayüz tanımı**

Bu örnek, tek bir VLAN kimliğine sahip bir alt arabirim için alt arabirim tanımısağlar. VLAN Kimliği, her biri eşleme başına benzersizdir. IPv4 adresinizin son sekizlisi her zaman tek bir sayı olacaktır.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

**QinQ arayüz tanımı**

Bu örnek, iki VLAN iD'si olan bir alt arabirim için alt arabirim tanımını sağlar. Dış VLAN Kimliği (s-etiketi), kullanılırsa, tüm eşlemeler arasında aynı kalır. İç VLAN Kimliği (c-tag) her biri eşleme başına benzersizdir. IPv4 adresinizin son sekizlisi her zaman tek bir sayı olacaktır.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>

### <a name="set-up-ebgp-sessions"></a>eBGP oturumlarını ayarlama
Her bir bakış için Microsoft ile bir BGP oturumu ayarlamanız gerekir. Aşağıdaki örneği kullanarak bir BGP oturumu ayarlayın. Alt arabiriminiz için kullandığınız IPv4 adresi a.b.c.d ise, BGP komşusunun (Microsoft) IP adresi a.b.c.d+1 olacaktır. BGP komşunun IPv4 adresinin son sekizlisi her zaman çift sayı olacaktır.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="set-up-prefixes-to-be-advertised-over-the-bgp-session"></a>BGP oturumunda duyurulacak önekleri ayarlama
Aşağıdaki örneği kullanarak belirli öneklerin Microsoft'a duyurulması için yönlendiricinizi yapılandırın.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="route-maps"></a>Rota haritaları
Ağınıza yayılan önekleri filtrelemek için rota eşlerini ve önek listelerini kullanın. Aşağıdaki örneğe bakın ve uygun öneek listelerinin ayarlandığından emin olun.

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

### <a name="configure-bfd"></a>BFD'yi yapılandır

BFD'yi iki yerde yapılandıracaksınız: biri arabirim düzeyinde, diğeri BGP düzeyinde. Buradaki örnek QinQ arabirimi içindir. 

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


## <a name="juniper-mx-series-routers"></a>Ardıç MX serisi yönlendiriciler
Bu bölümdeki örnekler herhangi bir Ardıç MX serisi yönlendirici için geçerlidir.

### <a name="configure-interfaces-and-subinterfaces"></a>Arabirimleri ve alt arabirimleri yapılandırma

**Dot1Q arayüz tanımı**

Bu örnek, tek bir VLAN kimliğine sahip bir alt arabirim için alt arabirim tanımısağlar. VLAN Kimliği, her biri eşleme başına benzersizdir. IPv4 adresinizin son sekizlisi her zaman tek bir sayı olacaktır.

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


**QinQ arayüz tanımı**

Bu örnek, iki VLAN iD'si olan bir alt arabirim için alt arabirim tanımını sağlar. Dış VLAN Kimliği (s-etiketi), kullanılırsa, tüm eşlemeler arasında aynı kalır. İç VLAN Kimliği (c-tag) her biri eşleme başına benzersizdir. IPv4 adresinizin son sekizlisi her zaman tek bir sayı olacaktır.

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

### <a name="set-up-ebgp-sessions"></a>eBGP oturumlarını ayarlama
Her bir bakış için Microsoft ile bir BGP oturumu ayarlamanız gerekir. Aşağıdaki örneği kullanarak bir BGP oturumu ayarlayın. Alt arabiriminiz için kullandığınız IPv4 adresi a.b.c.d ise, BGP komşusunun (Microsoft) IP adresi a.b.c.d+1 olacaktır. BGP komşunun IPv4 adresinin son sekizlisi her zaman çift sayı olacaktır.

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

### <a name="set-up-prefixes-to-be-advertised-over-the-bgp-session"></a>BGP oturumunda duyurulacak önekleri ayarlama
Aşağıdaki örneği kullanarak belirli öneklerin Microsoft'a duyurulması için yönlendiricinizi yapılandırın.

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter 
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
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="route-policies"></a>Rota ilkeleri
Ağınızda yayılan önekleri filtrelemek için rota eşlemlerini ve önek listelerini kullanabilirsiniz. Aşağıdaki örneğe bakın ve uygun öneek listelerinin ayarlandığından emin olun.

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
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="configure-bfd"></a>BFD'yi yapılandır
BFD'yi yalnızca protokol BGP bölümü altında yapılandırın.

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


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla ayrıntı için bkz. [ExpressRoute SSS](expressroute-faqs.md).



