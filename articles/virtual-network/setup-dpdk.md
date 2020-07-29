---
title: Azure Linux VM 'de DPDK | Microsoft Docs
description: Veri düzlemi geliştirme seti 'nin (DPDK) avantajlarını ve bir Linux sanal makinesinde DPDK 'yi ayarlamayı öğrenin.
services: virtual-network
documentationcenter: na
author: laxmanrb
manager: gedegrac
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2020
ms.author: labattul
ms.openlocfilehash: 20f22825e6954f32d7f020dd160f36c3c1859c86
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87287727"
---
# <a name="set-up-dpdk-in-a-linux-virtual-machine"></a>Bir Linux sanal makinesinde DPDK 'yi ayarlama

Azure 'da veri düzlemi geliştirme seti (DPDK), performansı yoğun uygulamalar için daha hızlı bir kullanıcı alanı paket işleme çerçevesi sunmaktadır. Bu çerçeve, sanal makinenin çekirdek ağ yığınını atlar.

Çekirdek ağ yığınını kullanan tipik paket işlemede, işlem kesme temelli olur. Ağ arabirimi gelen paketleri aldığında, paketi işlemek için bir çekirdek kesmesi ve çekirdek alanından Kullanıcı alanına bir bağlam anahtarı vardır. DPDK, hızlı paket işleme için yoklama modu sürücüleri kullanan bir kullanıcı alanı uygulamasının tercih edilen bağlam geçişini ve kesme temelli yöntemi ortadan kaldırır.

DPDK, alt düzey kaynaklara erişim sağlayan Kullanıcı-alan kitaplıklarının kümelerinden oluşur. Bu kaynaklar, ağ arabirimi kartları için donanım, mantıksal çekirdek, bellek yönetimi ve yoklama modu sürücüleri içerebilir.

DPDK, birden çok işletim sistemi dağıtımlarını destekleyen Azure sanal makinelerinde çalıştırılabilir. DPDK, ağ işlevi sanallaştırma uygulamalarını yönlendiren önemli performans farklılaşmasını sağlar. Bu uygulamalar, sanal yönlendiriciler, güvenlik duvarları, VPN 'Ler, yük dengeleyiciler, paket çekirdekleri ve hizmet reddi (DDoS) uygulamaları gibi ağ sanal gereçlerinin (NVA 'lar) biçimini alabilir.

## <a name="benefit"></a>Avantaj

**Saniye başına daha yüksek paket (PPS)**: çekirdek atlama ve Kullanıcı alanındaki paketlerin denetimini ele alma, bağlam anahtarlarını ortadan kaldırarak geçiş sayısını azaltır. Ayrıca, Azure Linux sanal makinelerinde saniye başına işlenen paketlerin hızını artırır.


## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Azure Marketi 'nden aşağıdaki dağıtımlar desteklenir:

| Linux işletim sistemi     | Çekirdek sürümü               | 
|--------------|---------------------------   |
| Ubuntu 16.04 | 4.15.0-1014-Azure +           | 
| Ubuntu 18.04 | 4.15.0-1014-Azure +           |
| SLES 15 SP1  | 4.12.14-8,27-Azure +          | 
| RHEL 7.5     | 3.10.0-862.11.6. EL7. x86_64 +  | 
| CentOS 7.5   | 3.10.0-862.11.6. EL7. x86_64 +  | 

**Özel çekirdek desteği**

Listelenmeyen tüm Linux çekirdek sürümleri için bkz. [Azure 'da ayarlanmış bir Linux çekirdeği oluşturmaya yönelik düzeltme ekleri](https://github.com/microsoft/azure-linux-kernel). Daha fazla bilgi için, ile de iletişim sağlayabilirsiniz [azuredpdk@microsoft.com](mailto:azuredpdk@microsoft.com) . 

## <a name="region-support"></a>Bölge desteği

Tüm Azure bölgeleri DPDK 'yi destekler.

## <a name="prerequisites"></a>Önkoşullar

Bir Linux sanal makinesinde hızlandırılmış ağ etkin olmalıdır. Sanal makine, yönetim için bir arabirim ile en az iki ağ arabirimine sahip olmalıdır. [Hızlandırılmış ağ etkin bir Linux sanal makinesi oluşturmayı](create-vm-accelerated-networking-cli.md)öğrenin.

## <a name="install-dpdk-dependencies"></a>DPDK bağımlılıklarını yükler

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo add-apt-repository ppa:canonical-server/dpdk-azure -y
sudo apt-get update
sudo apt-get install -y librdmacm-dev librdmacm1 build-essential libnuma-dev libmnl-dev
```

### <a name="ubuntu-1804"></a>Ubuntu 18.04

```bash
sudo add-apt-repository ppa:canonical-server/dpdk-azure -y
sudo apt-get update
sudo apt-get install -y librdmacm-dev librdmacm1 build-essential libnuma-dev libmnl-dev
```

### <a name="rhel75centos-75"></a>RHEL 7.5/CentOS 7,5

```bash
yum -y groupinstall "Infiniband Support"
sudo dracut --add-drivers "mlx4_en mlx4_ib mlx5_ib" -f
yum install -y gcc kernel-devel-`uname -r` numactl-devel.x86_64 librdmacm-devel libmnl-devel
```

### <a name="sles-15-sp1"></a>SLES 15 SP1

**Azure çekirdeği**

```bash
zypper  \
  --no-gpg-checks \
  --non-interactive \
  --gpg-auto-import-keys install kernel-azure kernel-devel-azure gcc make libnuma-devel numactl librdmacm1 rdma-core-devel
```

**Varsayılan Çekirdek**

```bash
zypper \
  --no-gpg-checks \
  --non-interactive \
  --gpg-auto-import-keys install kernel-default-devel gcc make libnuma-devel numactl librdmacm1 rdma-core-devel
```

## <a name="set-up-the-virtual-machine-environment-once"></a>Sanal makine ortamını ayarlama (bir kez)

1. [En son DPDK 'Yi indirin](https://core.dpdk.org/download). Azure için sürüm 18,11 LTS veya 19,11 LTS gerekir.
2. İle varsayılan yapılandırmayı oluşturun `make config T=x86_64-native-linuxapp-gcc` .
3. İle oluşturulan yapılandırmada Mellanox PMDs 'yi etkinleştirin `sed -ri 's,(MLX._PMD=)n,\1y,' build/.config` .
4. İle derleyin `make` .
5. İle uygulamasını `make install DESTDIR=<output folder>` .

## <a name="configure-the-runtime-environment"></a>Çalışma zamanı ortamını yapılandırma

Yeniden başlattıktan sonra, aşağıdaki komutları bir kez çalıştırın:

1. Kugepages

   * Her NUMA düğümü için bir kez aşağıdaki komutu çalıştırarak kugepage 'i yapılandırın:

     ```bash
     echo 1024 | sudo tee /sys/devices/system/node/node*/hugepages/hugepages-2048kB/nr_hugepages
     ```

   * İle bağlama için bir dizin oluşturun `mkdir /mnt/huge` .
   * İle kugesayfalarý bağlayın `mount -t hugetlbfs nodev /mnt/huge` .
   * Kugepages 'in ile ayrılmadığından emin olun `grep Huge /proc/meminfo` .

     > NOTUN Grub dosyasını değiştirmek için bir yol vardır. bu sayede, DPDK [yönergelerini](https://dpdk.org/doc/guides/linux_gsg/sys_reqs.html#use-of-hugepages-in-the-linux-environment) izleyerek kugepages 'in önyükleme üzerinde ayrılması sağlanır. Yönergeler sayfanın en altında bulunur. Bir Azure Linux sanal makinesi kullanırken, yeniden başlatmalar genelinde kugepages ayırmak için bunun yerine **/etc/config/grub.d** altındaki dosyaları değiştirin.

2. MAC & IP adresleri: `ifconfig –a` ağ ARABIRIMLERININ Mac ve IP adresini görüntülemek Için kullanın. *VF* ağ arabirimi ve *netvsc* ağ arabirimi aynı MAC adresine sahip, ancak yalnızca *netvsc* ağ arabiriminin bir IP adresi vardır. *VF* arabirimleri, *netvsc* arabirimlerinin alt arabirimleri olarak çalışır.

3. PCI adresleri

   * `ethtool -i <vf interface name>` *VF*için hangi PCI adresinin kullanılacağını bulmak için kullanın.
   * *Eth0* hızlandırılmış ağ etkinse, testpmd 'nin *eth0*için *VF* PCI cihazını yanlışlıkla devralmadığınızdan emin olun. DPDK uygulaması yanlışlıkla yönetim ağ arabirimini ele alırsa ve SSH bağlantınızı kaybetmenize neden oluyorsa, DPDK uygulamasını durdurmak için seri konsolu 'nu kullanın. Ayrıca, sanal makineyi durdurmak veya başlatmak için seri konsolunu da kullanabilirsiniz.

4. Her yeniden başlatmada *ıbuverbs* 'ı yükleyin `modprobe -a ib_uverbs` . Yalnızca SLES 15 için ile *mlx4_ib* de yükleyin `modprobe -a mlx4_ib` .

## <a name="failsafe-pmd"></a>Failsafe PMD

DPDK uygulamalarının Azure 'da kullanıma sunulan Failsafe PMD üzerinde çalışması gerekir. Uygulama doğrudan *VF* PMD üzerinde çalışıyorsa, bazı paketler yapay arabirim üzerinde GÖSTERILDIĞINDEN VM 'ye giden **Tüm** paketleri almaz. 

Failsafe PMD üzerinde bir DPDK uygulaması çalıştırırsanız, uygulamanın kendisine gidecek tüm paketleri almasını güvence altına alır. Ayrıca, ana bilgisayara bakım yapıldığında VF iptal edilse bile uygulamanın DPDK modunda çalışmaya devam ettiği de emin olur. Failsafe PMD hakkında daha fazla bilgi için bkz. [başarısız-güvenli yoklama modu sürücü kitaplığı](https://doc.dpdk.org/guides/nics/fail_safe.html).

## <a name="run-testpmd"></a>Testpmd çalıştırma

Testpmd 'yi kök modda çalıştırmak için `sudo` *testpmd* komutundan önce kullanın.

### <a name="basic-sanity-check-failsafe-adapter-initialization"></a>Temel: Sanity denetimi, Failsafe bağdaştırıcısı başlatma

1. Tek bir bağlantı noktası testpmd uygulamasını başlatmak için aşağıdaki komutları çalıştırın:

   ```bash
   testpmd -w <pci address from previous step> \
     --vdev="net_vdev_netvsc0,iface=eth1" \
     -- -i \
     --port-topology=chained
    ```

2. Bir çift bağlantı noktası testpmd uygulamasını başlatmak için aşağıdaki komutları çalıştırın:

   ```bash
   testpmd -w <pci address nic1> \
   -w <pci address nic2> \
   --vdev="net_vdev_netvsc0,iface=eth1" \
   --vdev="net_vdev_netvsc1,iface=eth2" \
   -- -i
   ```

   Testpmd 'yi ikiden fazla NIC ile çalıştırıyorsanız, `--vdev` bağımsız değişken şu düzene uyar: `net_vdev_netvsc<id>,iface=<vf’s pairing eth>` .

3.  Başlatıldıktan sonra `show port info all` bağlantı noktası bilgilerini denetlemek için öğesini çalıştırın. Net_failsafe ( *net_mlx4*değil) bir veya iki DPDK bağlantı noktası görmeniz gerekir.
4.  `start <port> /stop <port>`Trafiği başlatmak için kullanın.

Önceki komutlar *testpmd* ' yi etkileşimli modda başlatır, bu da testpmd komutlarının denemesi için önerilir.

### <a name="basic-single-sendersingle-receiver"></a>Temel: tek gönderici/tek alıcı

Aşağıdaki komutlar paketleri saniyede düzenli olarak yazdırır:

1. TX tarafında, aşağıdaki komutu çalıştırın:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device you plan to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=txonly \
     --eth-peer=<port id>,<receiver peer MAC address> \
     --stats-period <display interval in seconds>
   ```

2. RX tarafında, aşağıdaki komutu çalıştırın:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device you plan to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=rxonly \
     --eth-peer=<port id>,<sender peer MAC address> \
     --stats-period <display interval in seconds>
   ```

Bir sanal makinede önceki komutları çalıştırırken *IP_SRC_ADDR* değiştirin ve ' de *IP_DST_ADDR* , `app/test-pmd/txonly.c` DERLEMEDEN önce sanal makinelerin gerçek IP adresiyle eşleşecek şekilde değiştirin. Aksi takdirde, paketler alıcıya ulaşmadan önce bırakılır.

### <a name="advanced-single-sendersingle-forwarder"></a>Gelişmiş: tekli gönderici/tek iletici
Aşağıdaki komutlar paketleri saniyede düzenli olarak yazdırır:

1. TX tarafında, aşağıdaki komutu çalıştırın:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device you plan to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=txonly \
     --eth-peer=<port id>,<receiver peer MAC address> \
     --stats-period <display interval in seconds>
    ```

2. FWD tarafında aşağıdaki komutu çalıştırın:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address NIC1> \
     -w <pci address NIC2> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     --vdev="net_vdev_netvsc<2nd id>,iface=<2nd iface to attach to>" (you need as many --vdev arguments as the number of devices used by testpmd, in this case) \
     -- --nb-cores <number of cores to use for test pmd> \
     --forward-mode=io \
     --eth-peer=<recv port id>,<sender peer MAC address> \
     --stats-period <display interval in seconds>
    ```

Bir sanal makinede önceki komutları çalıştırırken *IP_SRC_ADDR* değiştirin ve ' de *IP_DST_ADDR* , `app/test-pmd/txonly.c` DERLEMEDEN önce sanal makinelerin gerçek IP adresiyle eşleşecek şekilde değiştirin. Aksi takdirde, paketler ileticiye ulaşmadan önce bırakılır. *Test PMD* ileticisi, bazı kod değişiklikleri yapmadığınız takdirde katman 3 adreslerini değiştirmediğinden, üçüncü bir makineye iletilen trafik alamazsınız.

## <a name="references"></a>Başvurular

* [EAL seçenekleri](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#eal-command-line-options)
* [Testpmd komutları](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#testpmd-command-line-options)
