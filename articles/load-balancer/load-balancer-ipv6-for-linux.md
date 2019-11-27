---
title: Linux Vm'leri için DHCPv6'ı yapılandırma
titleSuffix: Azure Load Balancer
description: Bu makalede, Linux VM 'Leri için DHCPv6 yapılandırma hakkında bilgi edinin.
services: load-balancer
documentationcenter: na
author: asudbring
keywords: IPv6, azure yük dengeleyici, ikili yığın, genel IP, yerel IPv6, mobil veya IOT
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2019
ms.author: allensu
ms.openlocfilehash: 6ea215b6aa826231e940f88c3687bb65591303f2
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74225314"
---
# <a name="configure-dhcpv6-for-linux-vms"></a>Linux Vm'leri için DHCPv6'ı yapılandırma


Linux sanal makine görüntüleri Azure Market'te bazıları, dinamik konak Yapılandırma Protokolü sürüm 6 (DHCPv6) varsayılan olarak yapılandırılmış yoktur. IPv6 desteği için kullandığınız Linux işletim sistemi dağıtımlarında DHCPv6 yapılandırılması gerekir. Farklı paketler kullandıkları için çeşitli Linux dağıtımlarını çeşitli şekillerde DHCPv6 yapılandırın.

> [!NOTE]
> Son SUSE Linux ve CoreOS görüntüleri Azure Market'te DHCPv6 ile önceden yapılandırılmış. Bu görüntüleri kullandığınızda, ek değişiklik gerekmez.

Bu belge, Linux sanal makinenizi bir IPv6 adresi alır, böylece DHCPv6 etkinleştirmek açıklar.

> [!WARNING]
> Hatalı ağ yapılandırma dosyalarını düzenleyerek, sanal makinenizde ağ erişimi kaybedebilir. Üretim dışı sistemlere yapılandırma değişiklikleri test etmenizi öneririz. Bu makaledeki yönergeleri Azure Market'teki Linux görüntüleri en son sürümlerinde test edilmiştir. Daha ayrıntılı yönergeler için kendi Linux sürümü için belgelerine bakın.

## <a name="ubuntu"></a>Ubuntu

1. */Etc/DHCP/dhclient6.conf* dosyasını düzenleyin ve aşağıdaki satırı ekleyin:

        timeout 10;

2. Aşağıdaki yapılandırma ile eth0 arabirimi için ağ yapılandırmasını düzenleyin:

   * **Ubuntu 12,04 ve 14,04**' de, */etc/Network/Interfaces.exe d/ET0.cfg* dosyasını düzenleyin. 
   * **Ubuntu 16,04**' de */etc/Network/Interfaces.exe d/50-Cloud-init.exe* dosyasını düzenleyin.

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. IPv6 adresini Yenile:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```
Ubuntu 17,10 ile başlayarak, varsayılan ağ yapılandırma mekanizması [Netplan]( https://netplan.io)' dır.  Install/örneklemesi oluşturma sırasında NETPLAN şu konumdaki YAML yapılandırma dosyalarından ağ yapılandırmasını okur:/{lib, vs, Run}/Netplan/*. YAML.

Lütfen yapılandırmanızda her Ethernet arabirimi için bir *dhcp6: true* ifadesini ekleyin.  Örneğin:
  
        network:
          version: 2
          ethernets:
            eno1:
              dhcp6: true

Erken önyükleme sırasında, Netplan "ağ Oluşturucu", bir yandan, NETPLAN ile ilgili başvuru bilgileri Için belirtilen ağ Daemon 'a cihaz denetimini el ile almak için yapılandırmayı bir şekilde yazar. https://netplan.io/reference.
 
## <a name="debian"></a>Debian

1. */Etc/DHCP/dhclient6.conf* dosyasını düzenleyin ve aşağıdaki satırı ekleyin:

        timeout 10;

2. */Etc/Network/Interfaces* dosyasını düzenleyin ve aşağıdaki yapılandırmayı ekleyin:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. IPv6 adresini Yenile:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel-centos-and-oracle-linux"></a>RHEL, CentOS ve Oracle Linux

1. */Etc/sysconfig/Network* dosyasını düzenleyin ve aşağıdaki parametreyi ekleyin:

        NETWORKING_IPV6=yes

2. */Etc/sysconfig/Network-Scripts/ifcfg-ET0* dosyasını düzenleyin ve aşağıdaki iki parametreyi ekleyin:

        IPV6INIT=yes
        DHCPV6C=yes

3. IPv6 adresini Yenile:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11-and-opensuse-13"></a>SLES 11 ve openSUSE 13

Azure'da openSUSE görüntülerine ve son SUSE Linux Enterprise Server (SLES) DHCPv6 ile önceden yapılandırılmış olmuştur. Bu görüntüleri kullandığınızda, ek değişiklik gerekmez. Bir daha eski veya özel bir SUSE görüntüyü temel alarak bir VM varsa, aşağıdakileri yapın:

1. Gerekirse `dhcp-client` paketini yükler:

    ```bash
    sudo zypper install dhcp-client
    ```

2. */Etc/sysconfig/Network/ifcfg-ET0* dosyasını düzenleyin ve aşağıdaki parametreyi ekleyin:

        DHCLIENT6_MODE='managed'

3. IPv6 adresini Yenile:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 ve openSUSE artık

Azure'nün son SLES ve openSUSE görüntülerde DHCPv6 ile önceden yapılandırılmış. Bu görüntüleri kullandığınızda, ek değişiklik gerekmez. Bir daha eski veya özel bir SUSE görüntüyü temel alarak bir VM varsa, aşağıdakileri yapın:

1. */Etc/sysconfig/Network/ifcfg-ET0* dosyasını düzenleyin ve `#BOOTPROTO='dhcp4'` parametresini aşağıdaki değerle değiştirin:

        BOOTPROTO='dhcp'

2. */Etc/sysconfig/Network/ifcfg-ET0* dosyasına şu parametreyi ekleyin:

        DHCLIENT6_MODE='managed'

3. IPv6 adresini Yenile:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Azure'da yeni bir CoreOS görüntüleri DHCPv6 ile önceden yapılandırılmış. Bu görüntüleri kullandığınızda, ek değişiklik gerekmez. Bir daha eski veya özel bir CoreOS görüntüyü temel alarak bir VM varsa, aşağıdakileri yapın:

1. */Etc/systemd/Network/10_dhcp. Network* dosyasını düzenleyin:

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. IPv6 adresini Yenile:

    ```bash
    sudo systemctl restart systemd-networkd
    ```
