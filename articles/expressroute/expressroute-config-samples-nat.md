---
title: 'Azure ExpressRoute: Yönlendirici yapılandırma örnekleri - NAT'
description: Bu sayfa, Cisco ve Juniper yönlendiricileri için yönlendirici yapılandırma örnekleri sağlar.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 12/06/2018
ms.author: cherylmc
ms.openlocfilehash: ef2fd40db422c459ca966e802344ef45f7ec01de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74072107"
---
# <a name="router-configuration-samples-to-set-up-and-manage-nat"></a>NAT'yi kurmak ve yönetmek için yönlendirici yapılandırma örnekleri

Bu sayfa, ExpressRoute ile çalışırken Cisco ASA ve Juniper SRX serisi yönlendiriciler için NAT yapılandırma örnekleri sağlar. Bunlar yalnızca rehberlik için örnek olarak tasarlanmıştır ve olduğu gibi kullanılmamalıdır. Ağınız için uygun yapılandırmaları bulmak için satıcınızla birlikte çalışabilirsiniz.

> [!IMPORTANT]
> Bu sayfadaki örneklerin tamamen rehberlik amaçlı olması amaçlanmıştır. İhtiyaçlarınızı karşılamak için uygun yapılandırmaları bulmak için satıcınızın satış / teknik ekibi ve ağ ekibi ile çalışmanız gerekir. Microsoft, bu sayfada listelenen yapılandırmalarla ilgili sorunları desteklemez. Destek sorunları için aygıt satıcınıza başvurmanız gerekir.
> 
> 

* Aşağıdaki yönlendirici yapılandırma örnekleri Azure Genel ve Microsoft eşlemeleri için geçerlidir. Azure özel eşleme için NAT'yi yapılandırmamalısınız. Daha fazla ayrıntı için [ExpressRoute eşlemelerini](expressroute-circuit-peerings.md) ve [ExpressRoute NAT gereksinimlerini](expressroute-nat.md) gözden geçirin.

* Internet ve ExpressRoute bağlantısı için ayrı NAT IP havuzları kullanmanız gerekir. Internet ve ExpressRoute üzerinden aynı NAT IP havuzunu kullanmak asimetrik yönlendirmeye ve bağlantı kaybına neden olur.


## <a name="cisco-asa-firewalls"></a>Cisco ASA güvenlik duvarları
### <a name="pat-configuration-for-traffic-from-customer-network-to-microsoft"></a>Müşteri ağından Microsoft'a trafik için PAT yapılandırması
    object network MSFT-PAT
      range <SNAT-START-IP> <SNAT-END-IP>


    object-group network MSFT-Range
      network-object <IP> <Subnet_Mask>

    object-group network on-prem-range-1
      network-object <IP> <Subnet-Mask>

    object-group network on-prem-range-2
      network-object <IP> <Subnet-Mask>

    object-group network on-prem
      network-object object on-prem-range-1
      network-object object on-prem-range-2

    nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range

### <a name="pat-configuration-for-traffic-from-microsoft-to-customer-network"></a>Microsoft'tan müşteri ağına trafik için PAT yapılandırması

**Arayüzler ve Yön:**

    Source Interface (where the traffic enters the ASA): inside
    Destination Interface (where the traffic exits the ASA): outside

**Yapılandırma:**

NAT Havuzu:

    object network outbound-PAT
        host <NAT-IP>

Hedef Sunucu:

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

Müşteri IP Adresleri için Nesne Grubu

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>

    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

NAT Komutları:

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a>Ardıç SRX serisi yönlendiriciler
### <a name="1-create-redundant-ethernet-interfaces-for-the-cluster"></a>1. Küme için gereksiz Ethernet arabirimleri oluşturma
    interfaces {
        reth0 {
            description "To Internal Network";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 1;
            }
            unit 100 {
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
        reth1 {
            description "To Microsoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "To Microsoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }


### <a name="2-create-two-security-zones"></a>2. İki güvenlik bölgesi oluşturma
* Kenar Yönlendiriciler'e bakan harici ağ için dahili ağ için Güven Bölgesi ve Güvensizlik Bölgesi
* Bölgelere uygun arabirimler atama
* Arabirimlerdeki hizmetlere izin verme

    security { zones { security-zone Trust { ana bilgisayar-gelen trafik { sistem hizmetleri { ping;                   } protokolleri { bgp;                   } } arayüzleri { reth0.100;               } } güvenlik bölgesi Güvensizlik { ana bilgisayar adak-gelen trafik { sistem hizmetleri { ping;                   } protokolleri { bgp;                   } } arayüzleri { reth1.100;               }           }       }   }


### <a name="3-create-security-policies-between-zones"></a>3. Bölgeler arasında güvenlik ilkeleri oluşturma
    security {
        policies {
            from-zone Trust to-zone Untrust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
            from-zone Untrust to-zone Trust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
        }
    }


### <a name="4-configure-nat-policies"></a>4. NAT ilkelerini yapılandırma
* İki NAT havuzu oluşturun. Bunlardan biri Microsoft'a giden NAT trafiği, Microsoft'tan müşteriye giden diğer trafik için kullanılır.
* NAT için ilgili trafik için kurallar oluşturma
  
       security {
           nat {
               source {
                   pool SNAT-To-ExpressRoute {
                       routing-instance {
                           External-ExpressRoute;
                       }
                       address {
                           <NAT-IP-address/Subnet-mask>;
                       }
                   }
                   pool SNAT-From-ExpressRoute {
                       routing-instance {
                           Internal;
                       }
                       address {
                           <NAT-IP-address/Subnet-mask>;
                       }
                   }
                   rule-set Outbound_NAT {
                       from routing-instance Internal;
                       to routing-instance External-ExpressRoute;
                       rule SNAT-Out {
                           match {
                               source-address 0.0.0.0/0;
                           }
                           then {
                               source-nat {
                                   pool {
                                       SNAT-To-ExpressRoute;
                                   }
                               }
                           }
                       }
                   }
                   rule-set Inbound-NAT {
                       from routing-instance External-ExpressRoute;
                       to routing-instance Internal;
                       rule SNAT-In {
                           match {
                               source-address 0.0.0.0/0;
                           }
                           then {
                               source-nat {
                                   pool {
                                       SNAT-From-ExpressRoute;
                                   }
                               }
                           }
                       }
                   }
               }
           }
       }

### <a name="5-configure-bgp-to-advertise-selective-prefixes-in-each-direction"></a>5. Her yönde seçici öneklerin reklamını yapmak için BGP'yi yapılandırın
Yönlendirme yapılandırma [örnekleri](expressroute-config-samples-routing.md) sayfasındaki örneklere bakın.

### <a name="6-create-policies"></a>6. İlkeler oluşturma
    routing-options {
                  autonomous-system <Customer-ASN>;
    }
    policy-options {
        prefix-list Microsoft-Prefixes {
            <IP-Address/Subnet-Mask;
            <IP-Address/Subnet-Mask;
        }
        prefix-list private-ranges {
            10.0.0.0/8;
            172.16.0.0/12;
            192.168.0.0/16;
            100.64.0.0/10;
        }
        policy-statement Advertise-NAT-Pools {
            from {
                protocol static;
                route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
            }
            then accept;
        }
        policy-statement Accept-from-Microsoft {
            term 1 {
                from {
                    instance External-ExpressRoute;
                    prefix-list-filter Microsoft-Prefixes orlonger;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
        policy-statement Accept-from-Internal {
            term no-private {
                from {
                    instance Internal;
                    prefix-list-filter private-ranges orlonger;
                }
                then reject;
            }
            term bgp {
                from {
                    instance Internal;
                    protocol bgp;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
    }
    routing-instances {
        Internal {
            instance-type virtual-router;
            interface reth0.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Microsoft;
            }
            protocols {
                bgp {
                    group customer {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-ASN-1>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
        External-ExpressRoute {
            instance-type virtual-router;
            interface reth1.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Internal;
            }
            protocols {
                bgp {
                    group edge-router {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-Public-ASN>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
    }

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla ayrıntı için bkz. [ExpressRoute SSS](expressroute-faqs.md).

