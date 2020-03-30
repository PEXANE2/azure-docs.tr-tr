---
title: Linux VM'leri için DHCPv6'yı yapılandırın
titleSuffix: Azure Load Balancer
description: Bu makalede, Linux VM'leri için DHCPv6'yı nasıl yapılandırıştırılamayı öğrenin.
services: load-balancer
documentationcenter: na
author: asudbring
keywords: ipv6, azure yük dengeleyici, çift yığın, genel ip, yerli ipv6, mobil, iot
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2019
ms.author: allensu
ms.openlocfilehash: 6ea215b6aa826231e940f88c3687bb65591303f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74225314"
---
# <a name="configure-dhcpv6-for-linux-vms"></a>Linux VM'leri için DHCPv6'yı yapılandırın


Azure Marketi'ndeki Linux sanal makine görüntülerinden bazılarının varsayılan olarak yapılandırılan Dinamik Ana Bilgisayar Yapılandırma Protokolü sürüm 6 (DHCPv6) yoktur. IPv6'yı desteklemek için DHCPv6'nın kullandığınız Linux işletim sistemi dağıtımında yapılandırılması gerekir. Çeşitli Linux dağıtımları DHCPv6'yı çeşitli şekillerde yapılandırır, çünkü farklı paketler kullanırlar.

> [!NOTE]
> Azure Marketi'ndeki son SUSE Linux ve CoreOS görüntüleri DHCPv6 ile önceden yapılandırılmıştır. Bu görüntüleri kullandığınızda ek değişiklik gerekmez.

Bu belge, Linux sanal makinenizin bir IPv6 adresi edinebilmesi için DHCPv6'nın nasıl etkinleştirilen olduğunu açıklar.

> [!WARNING]
> Ağ yapılandırma dosyalarını yanlış düzenleyerek VM'nize ağ erişimini kaybedebilirsiniz. Yapılandırma değişikliklerinizi üretim dışı sistemlerde test edeyim önerilir. Bu makaledeki yönergeler Azure Marketi'ndeki Linux görüntülerinin en son sürümlerinde test edilmiştir. Daha ayrıntılı talimatlar için, Linux'un kendi sürümünüz için belgelere başvurun.

## <a name="ubuntu"></a>Ubuntu

1. */etc/dhcp/dhclient6.conf* dosyasını düzenle ve aşağıdaki satırı ekleyin:

        timeout 10;

2. Eth0 arabiriminin ağ yapılandırmasını aşağıdaki yapılandırmayla düzenleme:

   * **Ubuntu 12.04 ve 14.04'te** */etc/network/interfaces.d/eth0.cfg* dosyasını değiştirin. 
   * **Ubuntu 16.04'te** */etc/network/interfaces.d/50-cloud-init.cfg* dosyasını değiştirin.

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. IPv6 adresini yenileyin:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```
Ubuntu 17.10 ile başlayarak varsayılan ağ yapılandırma mekanizması [NETPLAN'dır.]( https://netplan.io)  Yükleme/anlık işlem saatinde NETPLAN, bu konumdaki YAML yapılandırma dosyalarından ağ yapılandırması okur: /{lib,etc,run}/netplan/*.yaml.

Lütfen yapılandırmanızdaki her ethernet arabirimi için *bir dhcp6:true* deyimi ekleyin.  Örnek:
  
        network:
          version: 2
          ethernets:
            eno1:
              dhcp6: true

Erken önyükleme sırasında, netplan "ağ işleyicisi" netplan hakkında referans bilgileri için belirtilen ağ daemon https://netplan.io/referencecihazların kontrolünü elden /çalıştırmak için yapılandırma yazar, bkz.
 
## <a name="debian"></a>Debian

1. */etc/dhcp/dhclient6.conf* dosyasını düzenle ve aşağıdaki satırı ekleyin:

        timeout 10;

2. */etc/network/interfaces* dosyasını düzenleme ve aşağıdaki yapılandırmayı ekleyin:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. IPv6 adresini yenileyin:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel-centos-and-oracle-linux"></a>RHEL, CentOS ve Oracle Linux

1. */etc/sysconfig/network* dosyasını edin ve aşağıdaki parametreyi ekleyin:

        NETWORKING_IPV6=yes

2. */etc/sysconfig/network-scripts/ifcfg-eth0* dosyasını edin ve aşağıdaki iki parametreyi ekleyin:

        IPV6INIT=yes
        DHCPV6C=yes

3. IPv6 adresini yenileyin:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11-and-opensuse-13"></a>SLES 11 ve açıkSUSE 13

Son SUSE Linux Enterprise Server (SLES) ve Azure'daki openSUSE görüntüleri DHCPv6 ile önceden yapılandırılmıştır. Bu görüntüleri kullandığınızda ek değişiklik gerekmez. Eski veya özel bir SUSE resmine dayanan bir VM'iniz varsa, aşağıdakileri yapın:

1. Gerekirse `dhcp-client` paketi yükleyin:

    ```bash
    sudo zypper install dhcp-client
    ```

2. */etc/sysconfig/network/ifcfg-eth0* dosyasını edin ve aşağıdaki parametreyi ekleyin:

        DHCLIENT6_MODE='managed'

3. IPv6 adresini yenileyin:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 ve openSUSE Leap

Azure'daki son SLES ve openSUSE görüntüleri DHCPv6 ile önceden yapılandırılmıştır. Bu görüntüleri kullandığınızda ek değişiklik gerekmez. Eski veya özel bir SUSE resmine dayanan bir VM'iniz varsa, aşağıdakileri yapın:

1. */etc/sysconfig/network/ifcfg-eth0* dosyasını ve parametreyi `#BOOTPROTO='dhcp4'` aşağıdaki değerle değiştirin:

        BOOTPROTO='dhcp'

2. */etc/sysconfig/network/ifcfg-eth0* dosyasına aşağıdaki parametreyi ekleyin:

        DHCLIENT6_MODE='managed'

3. IPv6 adresini yenileyin:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Azure'daki son CoreOS görüntüleri DHCPv6 ile önceden yapılandırılmıştır. Bu görüntüleri kullandığınızda ek değişiklik gerekmez. Eski veya özel bir CoreOS resmine dayalı bir VM'iniz varsa, aşağıdakileri yapın:

1. */etc/systemd/network/10_dhcp.network* dosyasını düzenle:

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. IPv6 adresini yenileyin:

    ```bash
    sudo systemctl restart systemd-networkd
    ```
