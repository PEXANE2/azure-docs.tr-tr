---
title: Bir Azure Linux VM'de DPDK | Microsoft Dokümanlar
description: DPDK'yı bir Linux sanal makinesinde nasıl kuracağız öğrenin.
services: virtual-network
documentationcenter: na
author: laxmanrb
manager: gedegrac
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2018
ms.author: labattul
ms.openlocfilehash: c79c1fd687e329b97a854a3ff66a3cf95076b5d6
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80384237"
---
# <a name="set-up-dpdk-in-a-linux-virtual-machine"></a>Bir Linux sanal makinedpDK kurmak

Azure'daki Veri Düzlemi Geliştirme Kiti (DPDK), performans yoğun uygulamalar için daha hızlı bir kullanıcı alanı paketi işleme çerçevesi sunar. Bu çerçeve, sanal makinenin çekirdek ağ yığınını atlar.

Çekirdek ağ yığınını kullanan tipik paket işlemede, işlem kesme odaklıdır. Ağ arabirimi gelen paketleri aldığında, paketi işlemek için bir çekirdek kesintisi ve çekirdek alanından kullanıcı alanına bir bağlam geçişi vardır. DPDK, hızlı paket işleme için yoklama modu sürücüleri kullanan bir kullanıcı alanı uygulaması lehine bağlam anahtarlama ve kesme odaklı yöntemi ortadan kaldırır.

DPDK, alt düzey kaynaklara erişim sağlayan kullanıcı alanı kitaplıklarından oluşur. Bu kaynaklar, ağ arabirimi kartları için donanım, mantıksal çekirdekler, bellek yönetimi ve yoklama modu sürücüleri içerebilir.

DPDK, birden çok işletim sistemi dağıtımLarını destekleyen Azure sanal makinelerinde çalıştırılabilir. DPDK, sürüş ağı işlevi sanallaştırma uygulamalarında önemli performans farklılaşması sağlar. Bu uygulamalar, sanal yönlendiriciler, güvenlik duvarları, VPN'ler, yük dengeleyicileri, gelişmiş paket çekirdekleri ve hizmet reddi (DDoS) uygulamaları gibi ağ sanal cihazları (NV'ler) biçiminde olabilir.

## <a name="benefit"></a>Avantaj

**Saniyede daha yüksek paketler (PPS)**: Çekirdek atlayarak ve kullanıcı alanında paketlerin denetimini ele geçirmek, bağlam anahtarlarını ortadan kaldırarak çevrim sayısını azaltır. Ayrıca, Azure Linux sanal makinelerinde saniyede işlenen paketlerin oranını da artırır.


## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Azure Marketi'nden aşağıdaki dağıtımlar desteklenir:

| Linux İşletim Sistemi     | Çekirdek sürümü               | 
|--------------|---------------------------   |
| Ubuntu 16.04 | 4.15.0-1014-azure+           | 
| Ubuntu 18.04 | 4.15.0-1014-azure+           |
| SLES 15 SP1  | 4.12.14-8.27-azure+          | 
| RHEL 7.5     | 3.10.0-862.11.6.el7.x86_64+  | 
| CentOS 7.5   | 3.10.0-862.11.6.el7.x86_64+  | 

**Özel çekirdek desteği**

Listede yer almayan herhangi bir Linux çekirdeği sürümü [için, Azure ayarlı bir Linux çekirdeği oluşturmak için Yamalar'a](https://github.com/microsoft/azure-linux-kernel)bakın. Daha fazla bilgi için, [azuredpdk@microsoft.com](mailto:azuredpdk@microsoft.com)ayrıca irtibata geçebilirsiniz. 

## <a name="region-support"></a>Bölge desteği

Tüm Azure bölgeleri DPDK'yı destekler.

## <a name="prerequisites"></a>Ön koşullar

Hızlandırılmış ağ bir Linux sanal makinede etkinleştirilmelidir. Sanal makine, yönetim için bir arayüze sahip en az iki ağ arabirimine sahip olmalıdır. [Hızlandırılmış ağ etkin leştirilmiş bir Linux sanal makinesini](create-vm-accelerated-networking-cli.md)nasıl oluşturabilirsiniz öğrenin.

## <a name="install-dpdk-dependencies"></a>DPDK bağımlılıklarını yükleme

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

### <a name="rhel75centos-75"></a>RHEL7.5/CentOS 7.5

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

**Varsayılan çekirdek**

```bash
zypper \
  --no-gpg-checks \
  --non-interactive \
  --gpg-auto-import-keys install kernel-default-devel gcc make libnuma-devel numactl librdmacm1 rdma-core-devel
```

## <a name="set-up-the-virtual-machine-environment-once"></a>Sanal makine ortamını ayarlama (bir kez)

1. [En son DPDK'yı indirin.](https://core.dpdk.org/download) Azure için sürüm 18.11 LTS veya 19.11 LTS gereklidir.
2. Varsayılan config'i `make config T=x86_64-native-linuxapp-gcc`' ile oluşturun.
3. Mellanox PMD'leri oluşturulan config `sed -ri 's,(MLX._PMD=)n,\1y,' build/.config`ile etkinleştirin.
4. Ile `make`derle.
5. 'ile `make install DESTDIR=<output folder>`yükleyin.

## <a name="configure-the-runtime-environment"></a>Çalışma zamanı ortamını yapılandırma

Yeniden başladıktan sonra aşağıdaki komutları bir kez çalıştırın:

1. Büyük sayfalar

   * Her numa düğümü için bir kez aşağıdaki komutu çalıştırarak büyük sayfayı yapılandırın:

     ```bash
     echo 1024 | sudo tee /sys/devices/system/node/node*/hugepages/hugepages-2048kB/nr_hugepages
     ```

   * 'ile `mkdir /mnt/huge`montaj için bir dizin oluşturun.
   * Mount hugepages `mount -t hugetlbfs nodev /mnt/huge`ile .
   * Büyük sayfaların `grep Huge /proc/meminfo`.

     > [NOT] DpDK [yönergelerini](https://dpdk.org/doc/guides/linux_gsg/sys_reqs.html#use-of-hugepages-in-the-linux-environment) izleyerek büyük sayfaların önyüklemede rezerve edilebilmeleri için grub dosyasını değiştirmenin bir yolu vardır. Talimatlar sayfanın alt kısmındadır. Bir Azure Linux sanal makinesi kullanırken, yeniden başlatmalar arasında büyük sayfaları rezerve etmek için **/etc/config/grub.d** altındaki dosyaları değiştirin.

2. MAC & IP adresleri: Ağ arabirimlerinin MAC ve IP adresini görüntülemek için kullanın. `ifconfig –a` *VF* ağ arabirimi ve *NETVSC* ağ arabirimi aynı MAC adresine sahiptir, ancak yalnızca *NETVSC* ağ arabiriminin IP adresi vardır. *VF* *arabirimleri, NETVSC* arabirimlerinin alt arabirimleri olarak çalışıyor.

3. PCI adresleri

   * `ethtool -i <vf interface name>` *VF*için hangi PCI adresini kullanacağımı öğrenmek için kullanın.
   * *eth0* ağ etkin hızlandırılmış varsa, testpmd yanlışlıkla *eth0*için *VF* pci cihazı devralmak olmadığından emin olun. DPDK uygulaması yanlışlıkla yönetim ağı arabirimini ele geçirirse ve SSH bağlantınızı kaybetmenize neden olursa, DPDK uygulamasını durdurmak için seri konsolu kullanın. Sanal makineyi durdurmak veya başlatmak için seri konsolu da kullanabilirsiniz.

4. Her yeniden başlatmada `modprobe -a ib_uverbs` *ibuverbs'i* . Yalnızca SLES 15 için *mlx4_ib* mlx4_ib `modprobe -a mlx4_ib`yükleyin.

## <a name="failsafe-pmd"></a>Failsafe PMD

DPDK uygulamaları, Azure'da açığa çıkan failsafe PMD üzerinden geçmelidir. Uygulama doğrudan *VF* PMD üzerinden çalışırsa, bazı paketler sentetik arabirim üzerinde görününcünüz, VM'ye gidecek **tüm** paketleri almaz. 

Bir DPDK uygulamasını failsafe PMD üzerinden çalıştırıyorsanız, uygulamanın ona yönelik tüm paketleri almasını garanti eder. Ayrıca, ana bilgisayar adede hizmet verirken VF iptal edilebilse bile uygulamanın DPDK modunda çalışmaya devam etmesini sağlar. Failsafe PMD hakkında daha fazla bilgi için, [fail-safe anket modu sürücü kitaplığı](https://doc.dpdk.org/guides/nics/fail_safe.html)bakın.

## <a name="run-testpmd"></a>Testpmd'i çalıştır

Kök modunda testpmd çalıştırmak `sudo` *için, testpmd* komutundan önce kullanın.

### <a name="basic-sanity-check-failsafe-adapter-initialization"></a>Temel: Akıl sağlığı kontrolü, failsafe adaptör başlatma

1. Tek bir bağlantı noktası testpmd uygulamasını başlatmak için aşağıdaki komutları çalıştırın:

   ```bash
   testpmd -w <pci address from previous step> \
     --vdev="net_vdev_netvsc0,iface=eth1" \
     -- -i \
     --port-topology=chained
    ```

2. Çift bağlantı noktası testpmd uygulamasını başlatmak için aşağıdaki komutları çalıştırın:

   ```bash
   testpmd -w <pci address nic1> \
   -w <pci address nic2> \
   --vdev="net_vdev_netvsc0,iface=eth1" \
   --vdev="net_vdev_netvsc1,iface=eth2" \
   -- -i
   ```

   İkiden fazla NIC ile testpmd çalıştırıyorsanız, `--vdev` bağımsız değişken `net_vdev_netvsc<id>,iface=<vf’s pairing eth>`aşağıdaki şekilde çalışır: .

3.  Başladıktan sonra bağlantı `show port info all` noktası bilgilerini denetlemek için çalıştırın. net_failsafe *(net_mlx4*değil) bir veya iki DPDK bağlantı noktası görmelisiniz.
4.  Trafiği `start <port> /stop <port>` başlatmak için kullanın.

Önceki komutlar *testpmd* komutları denemek için önerilen etkileşimli modda testpmd başlar.

### <a name="basic-single-sendersingle-receiver"></a>Temel: Tek gönderen/tek alıcı

Aşağıdaki komutlar paketleri saniye istatistikleri başına düzenli olarak yazdırır:

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

Önceki komutları sanal bir makinede çalıştırırken, derlemeden `app/test-pmd/txonly.c` önce sanal makinelerin gerçek IP adresiyle eşleşecek şekilde *IP_SRC_ADDR* ve *IP_DST_ADDR* değiştirin. Aksi takdirde, paketler alıcıya ulaşmadan önce bırakılır.

### <a name="advanced-single-sendersingle-forwarder"></a>Gelişmiş: Tek gönderen/tek iletici
Aşağıdaki komutlar paketleri saniye istatistikleri başına düzenli olarak yazdırır:

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

2. FWD tarafında, aşağıdaki komutu çalıştırın:

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

Önceki komutları sanal bir makinede çalıştırırken, derlemeden `app/test-pmd/txonly.c` önce sanal makinelerin gerçek IP adresiyle eşleşecek şekilde *IP_SRC_ADDR* ve *IP_DST_ADDR* değiştirin. Aksi takdirde, paketler iletme ye ulaşmadan önce bırakılır. *Testpmd* iletmecisi, bazı kod değişiklikleri yapmadığısürece katman-3 adreslerini değiştirmediği için üçüncü bir makinenin iletilen trafiği almasını sağlayamazsınız.

## <a name="references"></a>Başvurular

* [EAL seçenekleri](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#eal-command-line-options)
* [Testpmd komutları](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#testpmd-command-line-options)
