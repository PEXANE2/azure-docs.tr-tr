---
title: 'Azure ExpressRoute: yönlendirici yapılandırma örnekleri-NAT'
description: Bu sayfa, Cisco ve Juniper yönlendiricileri için yönlendirici yapılandırma örnekleri sağlar.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 12/06/2018
ms.author: cherylmc
ms.openlocfilehash: 3393c661240ae5619597256a6691ae43608d622b
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85856707"
---
# <a name="router-configuration-samples-to-set-up-and-manage-nat"></a>NAT ayarlamak ve yönetmek için yönlendirici yapılandırma örnekleri

Bu sayfa, ExpressRoute ile çalışırken Cisco ASA ve Juniper SRX serisi yönlendiricileri için NAT yapılandırma örnekleri sağlar. Bunların yalnızca rehberlik için örnek olması amaçlanmıştır ve olduğu gibi kullanılmamalıdır. Ağınız için uygun yapılandırmalara sahip olmak üzere satıcınızla birlikte çalışabilirsiniz.

> [!IMPORTANT]
> Bu sayfadaki örneklerin amacı yalnızca kılavuzluk amaçlıdır. Gereksinimlerinizi karşılayacak uygun yapılandırmalara sahip olmak için satıcının satış/teknik ekibi ve ağ ekibiniz ile çalışmanız gerekir. Microsoft, bu sayfada listelenen yapılandırmalara ilişkin sorunları desteklemez. Destek sorunları için cihaz satıcınıza başvurmanız gerekir.
> 
> 

* Aşağıdaki yönlendirici yapılandırması örnekleri, Azure genel ve Microsoft eşlemeleri için geçerlidir. Azure özel eşlemesi için NAT yapılandırmamalısınız. Daha fazla ayrıntı için [ExpressRoute eşayarlarını](expressroute-circuit-peerings.md) ve [ExpressRoute NAT gereksinimlerini](expressroute-nat.md) gözden geçirin.

* İnternet ve ExpressRoute bağlantısı için ayrı NAT IP havuzları kullanmanız gerekır. İnternet ve ExpressRoute ile aynı NAT IP havuzunun kullanılması, asimetrik yönlendirmeye ve bağlantı kaybına neden olur.


## <a name="cisco-asa-firewalls"></a>Cisco ASA güvenlik duvarları
### <a name="pat-configuration-for-traffic-from-customer-network-to-microsoft"></a>Müşteri ağından Microsoft 'a giden trafik için PAT yapılandırması

```console
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
```

### <a name="pat-configuration-for-traffic-from-microsoft-to-customer-network"></a>Microsoft 'tan müşteri ağına giden trafik için PAT yapılandırması

**Arabirimler ve yön:**

Kaynak arabirim (trafiğin ASA girdiği yer): iç hedef arabirim

**Yapılandırmada**

NAT havuzu:

```console
object network outbound-PAT
    host <NAT-IP>
```

Hedef sunucu:

```console
object network Customer-Network
    network-object <IP> <Subnet-Mask>
```

Müşteri IP adresleri için nesne grubu:

```console
object-group network MSFT-Network-1
    network-object <MSFT-IP> <Subnet-Mask>

object-group network MSFT-PAT-Networks
    network-object object MSFT-Network-1
```

NAT komutları:

```console
nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network
```


## <a name="juniper-srx-series-routers"></a>Juniper SRX serisi yönlendiricileri
### <a name="1-create-redundant-ethernet-interfaces-for-the-cluster"></a>1. küme için yedekli Ethernet arabirimleri oluşturun

```console
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
```

### <a name="2-create-two-security-zones"></a>2. iki güvenlik bölgesi oluşturun
* Dış ağa yönelik uç yönlendiriciler için iç ağ ve güvenilir olmayan bölge için güven bölgesi
* Bölgelere uygun arabirimler atama
* Arabirimlerde hizmetlere izin ver

```console
    security {
        zones {
            security-zone Trust {
                host-inbound-traffic {
                    system-services {
                        ping;
                    }
                    protocols {
                        bgp;
                    }
                }
                interfaces {
                    reth0.100;
                }
            }
            security-zone Untrust {
                host-inbound-traffic {
                    system-services {
                        ping;
                    }
                    protocols {
                        bgp;
                    }
                }
                interfaces {
                    reth1.100;
                }
            }
        }
    }
```


### <a name="3-create-security-policies-between-zones"></a>3. bölgeler arasında güvenlik ilkeleri oluşturma

```console
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
```

### <a name="4-configure-nat-policies"></a>4. NAT ilkelerini yapılandırma
* İki NAT havuzu oluşturun. Bunlardan biri, Microsoft 'a giden ve diğer Microsoft ile müşteri arasındaki trafik için kullanılacaktır.
* İlgili trafiğe NAT için kurallar oluşturma

```console
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
```

### <a name="5-configure-bgp-to-advertise-selective-prefixes-in-each-direction"></a>5. her yönde seçmeli önekleri tanıtmak için BGP 'yi yapılandırın
[Yönlendirme yapılandırma örnekleri](expressroute-config-samples-routing.md) sayfasındaki örneklere bakın.

### <a name="6-create-policies"></a>6. ilke oluşturma

```console
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
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla ayrıntı için bkz. [ExpressRoute SSS](expressroute-faqs.md).

