---
title: Linux VM 'Ler için DHCPv6 'yi yapılandırma
titleSuffix: Azure Load Balancer
description: Bu makalede, Linux VM 'Leri için DHCPv6 yapılandırma hakkında bilgi edinin.
services: load-balancer
documentationcenter: na
author: asudbring
keywords: IPv6, Azure yük dengeleyici, çift yığın, genel IP, yerel IPv6, mobil, IoT
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2019
ms.author: allensu
ms.openlocfilehash: 6ea215b6aa826231e940f88c3687bb65591303f2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74225314"
---
# <a name="configure-dhcpv6-for-linux-vms"></a>Linux VM 'Ler için DHCPv6 'yi yapılandırma


Azure Marketi 'ndeki Linux sanal makine görüntülerinin bazıları varsayılan olarak yapılandırılmış dinamik ana bilgisayar Yapılandırma Protokolü sürüm 6 ' ya (DHCPv6) sahip değildir. IPv6 'yı desteklemek için, kullanmakta olduğunuz Linux işletim sistemi dağıtımında DHCPv6 yapılandırılmalıdır. Çeşitli Linux dağıtımları, farklı paketler kullandıkları için DHCPv6 'yi çeşitli şekillerde yapılandırır.

> [!NOTE]
> Azure Marketi 'ndeki son SUSE Linux ve CoreOS görüntüleri, DHCPv6 ile önceden yapılandırılmıştır. Bu görüntüleri kullandığınızda ek değişiklik yapmanız gerekmez.

Bu belgede, Linux sanal makineniz bir IPv6 adresi edinmesi için DHCPv6 'nin nasıl etkinleştirileceği açıklanır.

> [!WARNING]
> Ağ yapılandırma dosyalarını uygunsuz olarak düzenleyerek sanal makinenize ağ erişimini kaybedebilirsiniz. Üretim dışı sistemlerdeki yapılandırma değişikliklerinizi test etmeniz önerilir. Bu makaledeki yönergeler, Azure Marketi 'nde Linux görüntülerinin en son sürümlerinde test edilmiştir. Daha ayrıntılı yönergeler için kendi Linux sürümünüze yönelik belgelere başvurun.

## <a name="ubuntu"></a>Ubuntu

1. */Etc/DHCP/dhclient6.conf* dosyasını düzenleyin ve aşağıdaki satırı ekleyin:

        timeout 10;

2. Eth0 arabirimi için ağ yapılandırmasını aşağıdaki yapılandırmayla düzenleyin:

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

Erken önyükleme sırasında, Netplan "ağ Oluşturucusu", bir yandan, NETPLAN ile ilgili başvuru bilgileri Için belirtilen ağ Daemon 'e cihazların denetimini bırakma https://netplan.io/reference
 
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

Azure 'daki son SUSE Linux Enterprise Server (SLES) ve openSUSE görüntüleri, DHCPv6 ile önceden yapılandırılmıştır. Bu görüntüleri kullandığınızda ek değişiklik yapmanız gerekmez. Eski veya özel bir SUSE görüntüsünü temel alan bir VM varsa şunları yapın:

1. `dhcp-client`Gerekirse paketi yüklemelisiniz:

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

Azure 'daki son SLES ve openSUSE görüntüleri, DHCPv6 ile önceden yapılandırılmıştır. Bu görüntüleri kullandığınızda ek değişiklik yapmanız gerekmez. Eski veya özel bir SUSE görüntüsünü temel alan bir VM varsa şunları yapın:

1. */Etc/sysconfig/Network/ifcfg-ET0* dosyasını düzenleyin ve `#BOOTPROTO='dhcp4'` parametresini aşağıdaki değerle değiştirin:

        BOOTPROTO='dhcp'

2. */Etc/sysconfig/Network/ifcfg-ET0* dosyasına şu parametreyi ekleyin:

        DHCLIENT6_MODE='managed'

3. IPv6 adresini Yenile:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Azure 'daki son CoreOS görüntüleri, DHCPv6 ile önceden yapılandırılmıştır. Bu görüntüleri kullandığınızda ek değişiklik yapmanız gerekmez. Daha eski veya özel bir CoreOS görüntüsüne dayalı bir VM varsa şunları yapın:

1. */Etc/systemd/Network/10_dhcp. Network* dosyasını düzenleyin:

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. IPv6 adresini Yenile:

    ```bash
    sudo systemctl restart systemd-networkd
    ```
