---
title: 'Azure ExpressRoute: Yönlendirici yapılandırma örnekleri'
description: Bu sayfa, Cisco ve Ardıç yönlendiricileri için yönlendirici config örnekleri sağlar.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 03/26/2020
ms.author: osamaz
ms.openlocfilehash: 5304aefaf3ad70bb552b4b0d1b26fcce9867c9c0
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80397741"
---
# <a name="router-configuration-samples-to-set-up-and-manage-routing"></a>Yönlendirmeyi ayarlamak ve yönetmek için yönlendirici yapılandırma örnekleri
Bu sayfa, ExpressRoute ile çalışırken Cisco IOS-XE ve Juniper MX serisi yönlendiriciler için arayüz ve yönlendirme yapılandırma örnekleri sağlar. Bunlar yalnızca rehberlik için örnek olarak tasarlanmıştır ve olduğu gibi kullanılmamalıdır. Ağınız için uygun yapılandırmaları bulmak için satıcınızla birlikte çalışabilirsiniz. 

> [!IMPORTANT]
> Bu sayfadaki örneklerin tamamen rehberlik amaçlı olması amaçlanmıştır. İhtiyaçlarınızı karşılamak için uygun yapılandırmaları bulmak için satıcınızın satış / teknik ekibi ve ağ ekibi ile çalışmanız gerekir. Microsoft, bu sayfada listelenen yapılandırmalarla ilgili sorunları desteklemez. Destek sorunları için aygıt satıcınıza başvurmanız gerekir.
> 
> 

## <a name="mtu-and-tcp-mss-settings-on-router-interfaces"></a>Yönlendirici arabirimlerinde MTU ve TCP MSS ayarları
* ExpressRoute arabirimi için MTU 1500'dür, bu da yönlendiricideki ethernet arabirimi için tipik varsayılan MTU'dur. Yönlendiriciniz varsayılan olarak farklı bir MTU'ya sahip değilse, yönlendirici arabiriminde bir değer belirtmenize gerek yoktur.
* Azure VPN Ağ Geçidi'nin aksine, ExpressRoute devresi için TCP MSS'nin belirtilmesi gerekmez.

Aşağıdaki yönlendirici yapılandırma örnekleri tüm eşlemeler için geçerlidir. Yönlendirme hakkında daha fazla bilgi için [ExpressRoute eşlemelerini](expressroute-circuit-peerings.md) ve [ExpressRoute yönlendirme gereksinimlerini](expressroute-routing.md) gözden geçirin.


## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS-XE tabanlı yönlendiriciler
Bu bölümdeki örnekler, IOS-XE OS ailesini çalıştıran herhangi bir yönlendirici için geçerlidir.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. Arayüzleri ve alt arabirimleri yapılandırma
Microsoft'a bağlandığınız her yönlendiricide her eşleme başına bir alt arabirim gerekir. Bir alt arabirim, vlan kimliği veya yığılmış bir vlan kimliği çifti ve ip adresi ile tanımlanabilir.

**Dot1Q arayüz tanımı**

Bu örnek, tek bir VLAN kimliğine sahip bir alt arabirim için alt arabirim tanımını sağlar. VLAN Kimliği, her biri eşleme başına benzersizdir. IPv4 adresinizin son sekizlisi her zaman tek bir sayı olacaktır.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

**QinQ arayüz tanımı**

Bu örnek, iki VLAN iD'si olan bir alt arabirim için alt arabirim tanımını sağlar. Dış VLAN Kimliği (s-tag), kullanılırsa tüm eşlemeler arasında aynı kalır. İç VLAN Kimliği (c-tag) her biri eşleme başına benzersizdir. IPv4 adresinizin son sekizlisi her zaman tek bir sayı olacaktır.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>

### <a name="2-setting-up-ebgp-sessions"></a>2. eBGP oturumlarının ayarlanması
Her bir eşleme için Microsoft ile bir BGP oturumu kurmanız gerekir. Aşağıdaki örnek, Microsoft ile bir BGP oturumu kurmanızı sağlar. Alt arabiriminiz için kullandığınız IPv4 adresi a.b.c.d ise, BGP komşusunun (Microsoft) IP adresi a.b.c.d+1 olacaktır. BGP komşunun IPv4 adresinin son sekizlisi her zaman çift sayı olacaktır.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. BGP oturumunda duyurulacak önekleri ayarlama
Yönlendiricinizi, belirli öneklerin Reklamını Microsoft'a yapacak şekilde yapılandırabilirsiniz. Bunu aşağıdaki örneği kullanarak yapabilirsiniz.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4. Rota haritaları
Ağınızda yayılan önekleri filtrelemek için rota eşlemlerini ve önek listelerini kullanabilirsiniz. Görevi yerine getirmek için aşağıdaki örneği kullanabilirsiniz. Uygun öneek listeleri kurulumuna sahip olduğundan emin olun.

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

### <a name="5-configuring-bfd"></a>5. BFD Yapılandırma

BFD'yi iki yerde yapılandıracaksınız. Biri arayüz düzeyinde, diğeri bgp düzeyinde. Aşağıdaki örnek QinQ arabirimi içindir. 

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
Bu bölümdeki örnekler herhangi bir Ardıç MX serisi yönlendiriciler için geçerlidir.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. Arayüzleri ve alt arabirimleri yapılandırma

**Dot1Q arayüz tanımı**

Bu örnek, tek bir VLAN kimliğine sahip bir alt arabirim için alt arabirim tanımını sağlar. VLAN Kimliği, her biri eşleme başına benzersizdir. IPv4 adresinizin son sekizlisi her zaman tek bir sayı olacaktır.

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

Bu örnek, iki VLAN iD'si olan bir alt arabirim için alt arabirim tanımını sağlar. Dış VLAN Kimliği (s-tag), kullanılırsa tüm eşlemeler arasında aynı kalır. İç VLAN Kimliği (c-tag) her biri eşleme başına benzersizdir. IPv4 adresinizin son sekizlisi her zaman tek bir sayı olacaktır.

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

### <a name="2-setting-up-ebgp-sessions"></a>2. eBGP oturumlarının ayarlanması
Her bir eşleme için Microsoft ile bir BGP oturumu kurmanız gerekir. Aşağıdaki örnek, Microsoft ile bir BGP oturumu kurmanızı sağlar. Alt arabiriminiz için kullandığınız IPv4 adresi a.b.c.d ise, BGP komşusunun (Microsoft) IP adresi a.b.c.d+1 olacaktır. BGP komşunun IPv4 adresinin son sekizlisi her zaman çift sayı olacaktır.

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

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. BGP oturumunda duyurulacak önekleri ayarlama
Yönlendiricinizi, belirli öneklerin Reklamını Microsoft'a yapacak şekilde yapılandırabilirsiniz. Bunu aşağıdaki örneği kullanarak yapabilirsiniz.

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
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


### <a name="4-route-policies"></a>4. Rota Politikaları
Ağınızda yayılan önekleri filtrelemek için rota eşlemlerini ve önek listelerini kullanabilirsiniz. Görevi yerine getirmek için aşağıdaki örneği kullanabilirsiniz. Uygun öneek listeleri kurulumuna sahip olduğundan emin olun.

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

### <a name="4-configuring-bfd"></a>4. BFD'nin Yapılandırılması
BFD'yi yalnızca protokol BGP bölümü altında yapılandırırsınız.

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

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla ayrıntı için bkz. [ExpressRoute SSS](expressroute-faqs.md).



