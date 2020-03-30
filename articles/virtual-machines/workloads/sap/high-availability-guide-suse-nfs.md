---
title: SLES'te Azure VM'lerde NFS için yüksek kullanılabilirlik | Microsoft Dokümanlar
description: SUSE Linux Enterprise Server'da Azure VM'lerde NFS için yüksek kullanılabilirlik
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/26/2020
ms.author: radeltch
ms.openlocfilehash: 4dce0a675f5841591da00a322b72718964d382ac
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80348866"
---
# <a name="high-availability-for-nfs-on-azure-vms-on-suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server'da Azure VM'lerde NFS için yüksek kullanılabilirlik

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[sles-hae-guides]:https://www.suse.com/documentation/sle-ha-12/
[sles-for-sap-bp]:https://www.suse.com/documentation/sles-for-sap-12/
[suse-ha-12sp3-relnotes]:https://www.suse.com/releasenotes/x86_64/SLE-HA/12-SP3/

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md

Bu makalede, sanal makinelerin nasıl dağıtılanabileceği, sanal makineleri yapılandırma, küme çerçevesini yükleme ve kullanılabilir sap sisteminin paylaşılan verilerini depolamak için kullanılabilecek yüksek kullanılabilir bir NFS sunucusunun nasıl yüklenmesi açıklanmaktadır.
Bu kılavuz, iki SAP sistemleri, NW1 ve NW2 tarafından kullanılan bir yüksek kullanılabilir NFS sunucusu kurmak için nasıl açıklanır. Örnekteki kaynakların adları (örneğin sanal makineler, sanal ağlar) kaynak öneki **prod**ile [SAP dosya sunucusu şablonu][template-file-server] kullandığınızı varsayar.

Önce aşağıdaki SAP Notlarını ve bildirilerini okuyun

* SAP Note [1928533], olan:
  * SAP yazılımının dağıtımı için desteklenen Azure VM boyutları listesi
  * Azure VM boyutları için önemli kapasite bilgileri
  * Desteklenen SAP yazılımı ve işletim sistemi (OS) ve veritabanı kombinasyonları
  * Microsoft Azure'da Windows ve Linux için gerekli SAP çekirdeği sürümü

* SAP Note [2015553,] Azure'da SAP destekli SAP yazılım dağıtımları için ön koşulları listeler.
* SAP Note [2205917] SAP Uygulamaları için SUSE Linux Enterprise Server için işletim sistemi ayarlarını tavsiye etti
* SAP Note [1944799] SAP Uygulamaları için SUSE Linux Enterprise Server için SAP HANA Yönergeleri vardır
* SAP Note [2178632,] Azure'da SAP için bildirilen tüm izleme ölçümleri hakkında ayrıntılı bilgilere sahiptir.
* SAP Note [2191498,] Azure'da Linux için gerekli SAP Host Agent sürümüne sahiptir.
* SAP Note [2243692,] Azure'da Linux'ta SAP lisanslama hakkında bilgi edinmiştir.
* SAP Note [1984787,] SUSE Linux Enterprise Server 12 hakkında genel bilgilere sahiptir.
* SAP Note [1999351,] SAP için Azure Gelişmiş İzleme Uzantısı için ek sorun giderme bilgilerine sahiptir.
* [SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) Linux için gerekli tüm SAP Notları'na sahiptir.
* [Linux'ta SAP için Azure Sanal Makineler planlaması ve uygulaması][planning-guide]
* [Linux'ta SAP için Azure Sanal Makineler dağıtımı (bu makale)][deployment-guide]
* [Linux'ta SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* [SUSE Linux Enterprise Yüksek Kullanılabilirlik Uzantısı 12 SP3 en iyi uygulamalar kılavuzları][sles-hae-guides]
  * DRBD ve Kalp Pili ile Yüksek Kullanılabilir NFS Depolama
* [SAP Applications 12 SP3 en iyi uygulamalar kılavuzları için SUSE Linux Enterprise Server][sles-for-sap-bp]
* [SUSE Yüksek Kullanılabilirlik Uzantısı 12 SP3 Sürüm Notları][suse-ha-12sp3-relnotes]

## <a name="overview"></a>Genel Bakış

Yüksek kullanılabilirlik elde etmek için SAP NetWeaver bir NFS sunucusu gerektirir. NFS sunucusu ayrı bir kümede yapılandırılır ve birden çok SAP sistemi tarafından kullanılabilir.

![SAP NetWeaver Yüksek Kullanılabilirlik genel bakış](./media/high-availability-guide-nfs/ha-suse-nfs.png)

NFS sunucusu, bu NFS sunucusunu kullanan her SAP sistemi için özel bir sanal ana bilgisayar adı ve sanal IP adresleri kullanır. Azure'da, sanal bir IP adresi kullanmak için bir yük dengeleyicisi gereklidir. Aşağıdaki liste yük dengeleyicisinin yapılandırmasını gösterir.        

* Frontend yapılandırması
  * NW1 için IP adresi 10.0.0.4
  * NW2 için IP adresi 10.0.0.5
* Arka uç yapılandırması
  * NFS kümesinin bir parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı
* Sonda Bağlantı Noktası
  * NW1 için Bağlantı Noktası 61000
  * NW2 için Port 61001
* Yük dengeleme kuralları (temel yük dengeleyicisi kullanıyorsanız)
  * NW1 için 2049 TCP
  * NW1 için 2049 UDP
  * NW2 için 2049 TCP
  * NW2 için 2049 UDP

## <a name="set-up-a-highly-available-nfs-server"></a>Yüksek kullanılabilirlikte bir NFS sunucusu ayarlama

Sanal makineler, kullanılabilirlik kümesi ve yük dengeleyicisi de dahil olmak üzere gerekli tüm Azure kaynaklarını dağıtmak için GitHub'dan bir Azure Şablonu kullanabilir veya kaynakları el ile dağıtabilirsiniz.

### <a name="deploy-linux-via-azure-template"></a>Azure Şablonu ile Linux'u dağıtma

Azure Marketi, SAP Applications 12 için SUSE Linux Enterprise Server için yeni sanal makineleri dağıtmak için kullanabileceğiniz bir görüntü içerir.
Gerekli tüm kaynakları dağıtmak için GitHub'daki hızlı başlangıç şablonlarından birini kullanabilirsiniz. Şablon sanal makineleri, yük dengeleyicisini, kullanılabilirlik kümesini vb. dağıtır. Şablonu dağıtmak için aşağıdaki adımları izleyin:

1. Azure portalında [SAP dosya sunucusu şablonu][template-file-server] açma   
1. Aşağıdaki parametreleri girin
   1. Kaynak Öneki  
      Kullanmak istediğiniz önek girin. Değer, dağıtılan kaynaklar için önek olarak kullanılır.
   2. SAP Sistem Sayısı  
      Bu dosya sunucusunu kullanacak SAP sistemlerinin numarasını girin. Bu, gerekli miktarda ön uç yapılandırmaları, yük dengeleme kuralları, prob bağlantı noktaları, diskler vb. dağıtAcaktır.
   3. Os Tipi  
      Linux dağıtımlarından birini seçin. Bu örnekiçin, SLES 12'yi seçin
   4. Admin Kullanıcı Adı ve Yönetici Şifresi  
      Makinede oturum açmak için kullanılabilecek yeni bir kullanıcı oluşturulur.
   5. Alt net kimliği  
      VM'yi, VM'nin atanması gereken tanımlanmış bir alt ağınız olduğu varolan bir VNet'e dağıtmak istiyorsanız, bu alt ağın kimliğini adlandırın. Kimlik genellikle /abonelik/**&lt;abonelik kimliği&gt;**/kaynakGruplar/**&lt;kaynak&gt;grup adı**/providers/Microsoft.Network/virtualNetworks/**&lt;sanal ağ adı&gt;**/subnets/**&lt;subnet adı&gt; ** gibi görünür

### <a name="deploy-linux-manually-via-azure-portal"></a>Azure portalı üzerinden Linux'u el ile dağıtın

Öncelikle bu NFS kümesi için sanal makineler oluşturmanız gerekir. Daha sonra, bir yük dengeleyici oluşturmak ve arka uç havuzlarında sanal makineleri kullanın.

1. Kaynak Grubu oluşturma
1. Sanal Ağ Oluşturma
1. Kullanılabilirlik Kümesi Oluşturma  
   Max update etki alanını ayarlama
1. Sanal Makine Oluştur 1 En az SLES4SAP 12 SP3 kullanın, bu örnekte SAP Uygulamaları için SLES 4SAP 12 SP3 BYOS görüntü SLES 12 SP3 (BYOS) kullanılır  
   Daha önce oluşturulan Kullanılabilirlik Kümesini seçin  
1. Sanal Makine Oluştur 2 En az SLES4SAP 12 SP3 kullanın, bu örnekte SLES4SAP 12 SP3 BYOS görüntüsü  
   SAP Uygulamaları için SLES 12 SP3 (BYOS) kullanılır  
   Daha önce oluşturulan Kullanılabilirlik Kümesini seçin  
1. Her iki SANAL makineye her SAP sistemi için bir veri diski ekleyin.
1. Yük Dengeleyici (dahili) oluşturun. Standart [yük dengeleyicisi](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview)öneririz.  
   1. Standart Yük dengeleyicisi oluşturmak için aşağıdaki yönergeleri izleyin:
      1. Ön uç IP adreslerini oluşturma
         1. NW1 için IP adresi 10.0.0.4
            1. Yük bakiyesini açın, frontend IP havuzunu seçin ve Ekle'yi tıklatın
            1. Yeni ön uç IP havuzunun adını girin (örneğin **nw1-frontend)**
            1. Atamayı Statik olarak ayarlayın ve IP adresini girin (örneğin **10.0.0.4)**
            1. Tamam'ı tıklatın
         1. NW2 için IP adresi 10.0.0.5
            * NW2 için yukarıdaki adımları tekrarlayın
      1. Arka uç havuzları oluşturma
         1. NFS kümesinin bir parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı
            1. Yük bakiyesini açın, arka uç havuzlarını seçin ve Ekle'yi tıklatın
            1. Yeni arka uç havuzunun adını girin (örneğin **nw-backend)**
            1. Sanal Ağı Seçin
            1. Sanal makine ekle'yi tıklatın
            1. NFS kümesinin sanal makinelerini ve IP adreslerini seçin.
            1. Ekle’ye tıklayın.
      1. Sağlık sondalarını oluşturma
         1. NW1 için Bağlantı Noktası 61000
            1. Yük bakiyesini açın, sistem durumu sondalarını seçin ve Ekle'yi tıklatın
            1. Yeni sağlık sondasının adını girin (örneğin **nw1-hp)**
            1. Protokol olarak TCP'yi seçin, bağlantı noktası 610**00,** Aralık 5'i ve Sağlıksız eşiği 2'yi koruyun
            1. Tamam'ı tıklatın
         1. NW2 için Port 61001
            * NW2 için bir sistem durumu sondası oluşturmak için yukarıdaki adımları yineleyin
      1. Yük dengeleme kuralları
         1. Yük bakiyesini açın, yük dengeleme kurallarını seçin ve Ekle'yi tıklatın
         1. Yeni yük dengeleyici kuralının adını girin (örneğin **nw1-lb)**
         1. Daha önce oluşturduğunuz ön uç IP adresini, arka uç havuzunu ve sistem durumu sondasını seçin (örneğin **nw1-frontend.** **nw-backend** ve **nw1-hp**)
         1. **HA Bağlantı Noktalarını**seçin.
         1. Boşta kalma süresini 30 dakikaya çıkarma
         1. **Kayan IP'yi etkinleştirdiğinden emin olun**
         1. Tamam'ı tıklatın
         * NW2 için yük dengeleme kuralı oluşturmak için yukarıdaki adımları yineleme
   1. Alternatif olarak, senaryonuz temel yük dengeleyicisi gerektiriyorsa aşağıdaki yönergeleri izleyin:
      1. Ön uç IP adreslerini oluşturma
         1. NW1 için IP adresi 10.0.0.4
            1. Yük bakiyesini açın, frontend IP havuzunu seçin ve Ekle'yi tıklatın
            1. Yeni ön uç IP havuzunun adını girin (örneğin **nw1-frontend)**
            1. Atamayı Statik olarak ayarlayın ve IP adresini girin (örneğin **10.0.0.4)**
            1. Tamam'ı tıklatın
         1. NW2 için IP adresi 10.0.0.5
            * NW2 için yukarıdaki adımları tekrarlayın
      1. Arka uç havuzları oluşturma
         1. NFS kümesinin bir parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı
            1. Yük bakiyesini açın, arka uç havuzlarını seçin ve Ekle'yi tıklatın
            1. Yeni arka uç havuzunun adını girin (örneğin **nw-backend)**
            1. Sanal makine ekle'yi tıklatın
            1. Daha önce oluşturduğunuz Kullanılabilirlik Kümesini seçin
            1. NFS kümesinin sanal makinelerini seçin
            1. Tamam'ı tıklatın
      1. Sağlık sondalarını oluşturma
         1. NW1 için Bağlantı Noktası 61000
            1. Yük bakiyesini açın, sistem durumu sondalarını seçin ve Ekle'yi tıklatın
            1. Yeni sağlık sondasının adını girin (örneğin **nw1-hp)**
            1. Protokol olarak TCP'yi seçin, bağlantı noktası 610**00,** Aralık 5'i ve Sağlıksız eşiği 2'yi koruyun
            1. Tamam'ı tıklatın
         1. NW2 için Port 61001
            * NW2 için bir sistem durumu sondası oluşturmak için yukarıdaki adımları yineleyin
      1. Yük dengeleme kuralları
         1. NW1 için 2049 TCP
            1. Yük bakiyesini açın, yük dengeleme kurallarını seçin ve Ekle'yi tıklatın
            1. Yeni yük dengeleyici kuralının adını girin (örneğin **nw1-lb-2049)**
            1. Daha önce oluşturduğunuz ön uç IP adresini, arka uç havuzunu ve sistem durumu sondasını seçin (örneğin **nw1-frontend)**
            1. Protokol **TCP**tutun , port **2049** girin
            1. Boşta kalma süresini 30 dakikaya çıkarma
            1. **Kayan IP'yi etkinleştirdiğinden emin olun**
            1. Tamam'ı tıklatın
         1. NW1 için 2049 UDP
            * NW1 için bağlantı noktası 2049 ve UDP için yukarıdaki adımları tekrarlayın
         1. NW2 için 2049 TCP
            * NW2 için bağlantı noktası 2049 ve TCP için yukarıdaki adımları yineleme
         1. NW2 için 2049 UDP
            * NW2 için bağlantı noktası 2049 ve UDP için yukarıdaki adımları tekrarlayın

> [!Note]
> Ortak IP adresi olmayan VM'ler dahili (genel IP adresi yok) Standart Azure yük bakiyesi arka uç havuzuna yerleştirildiğinde, ortak bitiş noktalarına yönlendirmeye izin verecek ek yapılandırma yapılmadığı sürece giden internet bağlantısı olmaz. Giden bağlantının nasıl elde edilene ilişkin ayrıntılar [için, SAP yüksek kullanılabilirlik senaryolarında Azure Standart Yük Dengeleyicisini kullanan Sanal Makineler için Genel uç nokta bağlantısına](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-standard-load-balancer-outbound-connections)bakın.  

> [!IMPORTANT]
> Azure Yük Bakiyesi'nin arkasına yerleştirilen Azure VM'lerinde TCP zaman damgalarını etkinleştirme. TCP zaman damgalarını etkinleştirmek sistem durumu sondalarının başarısız lığa neden olur. Parametre **net.ipv4.tcp_timestamps** **0**'a ayarlayın. Ayrıntılar için [Bkz. Yük Dengeleyici sağlık probları.](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)

### <a name="create-pacemaker-cluster"></a>Kalp Pili kümesi ni oluştur

Bu NFS sunucusu için temel bir Pacemaker kümesi oluşturmak için [Azure'da SUSE Linux Enterprise Server'da Pacemaker'ı ayarlama](high-availability-guide-suse-pacemaker.md) adımlarını izleyin.

### <a name="configure-nfs-server"></a>NFS sunucusuyapılandırma

Aşağıdaki öğeler, tüm düğümler için geçerli olan **[A]** ile önceden belirlenmiştir, **[1] -** yalnızca düğüm 1 veya **[2]** için geçerlidir - yalnızca düğüm 2 için geçerlidir.

1. **[A]** Kurulum ana bilgisayar ad çözümü

   Bir DNS sunucusu kullanabilir veya tüm düğümlerde /etc/hosts'ı değiştirebilirsiniz. Bu örnek, /etc/hosts dosyasının nasıl kullanılacağını gösterir.
   Aşağıdaki komutlarda IP adresini ve ana bilgisayar adını değiştirme

   <pre><code>sudo vi /etc/hosts
   </code></pre>
   
   /etc/hosts'a aşağıdaki satırları ekleyin. IP adresini ve ana bilgisayar adını ortamınıza uyacak şekilde değiştirme
   
   <pre><code># IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nw1-nfs</b>
   <b>10.0.0.5 nw2-nfs</b>
   </code></pre>

1. **[A]** NFS sunucusuetkinleştir

   Kök NFS dışa giriş oluşturma

   <pre><code>sudo sh -c 'echo /srv/nfs/ *\(rw,no_root_squash,fsid=0\)>/etc/exports'
   
   sudo mkdir /srv/nfs/
   </code></pre>

1. **[A]** Drbd bileşenlerini yükleme

   <pre><code>sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. **[A]** Drbd cihazlar için bir bölüm oluşturma

   Kullanılabilir tüm veri disklerini listele

   <pre><code>sudo ls /dev/disk/azure/scsi1/
   </code></pre>

   Örnek çıktı
   
   ```
   lun0  lun1
   ```

   Her veri diski için bölümler oluşturma

   <pre><code>sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun0'
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun1'
   </code></pre>

1. **[A]** LVM yapılandırmaları oluşturma

   Kullanılabilir tüm bölümleri listele

   <pre><code>ls /dev/disk/azure/scsi1/lun*-part*
   </code></pre>

   Örnek çıktı
   
   ```
   /dev/disk/azure/scsi1/lun0-part1  /dev/disk/azure/scsi1/lun1-part1
   ```

   Her bölüm için LVM birimleri oluşturma

   <pre><code>sudo pvcreate /dev/disk/azure/scsi1/lun0-part1  
   sudo vgcreate vg-<b>NW1</b>-NFS /dev/disk/azure/scsi1/lun0-part1
   sudo lvcreate -l 100%FREE -n <b>NW1</b> vg-<b>NW1</b>-NFS

   sudo pvcreate /dev/disk/azure/scsi1/lun1-part1
   sudo vgcreate vg-<b>NW2</b>-NFS /dev/disk/azure/scsi1/lun1-part1
   sudo lvcreate -l 100%FREE -n <b>NW2</b> vg-<b>NW2</b>-NFS
   </code></pre>

1. **[A]** Drbd'yi yapılandır

   <pre><code>sudo vi /etc/drbd.conf
   </code></pre>

   Drbd.conf dosyasının aşağıdaki iki satırı içerdiğinden emin olun

   <pre><code>include "drbd.d/global_common.conf";
   include "drbd.d/*.res";
   </code></pre>

   Küresel drbd yapılandırmasını değiştirme

   <pre><code>sudo vi /etc/drbd.d/global_common.conf
   </code></pre>

   İşleyici ve net bölümüne aşağıdaki girişleri ekleyin.

   <pre><code>global {
        usage-count no;
   }
   common {
        handlers {
             fence-peer "/usr/lib/drbd/crm-fence-peer.sh";
             after-resync-target "/usr/lib/drbd/crm-unfence-peer.sh";
             split-brain "/usr/lib/drbd/notify-split-brain.sh root";
             pri-lost-after-sb "/usr/lib/drbd/notify-pri-lost-after-sb.sh; /usr/lib/drbd/notify-emergency-reboot.sh; echo b > /proc/sysrq-trigger ; reboot -f";
        }
        startup {
             wfc-timeout 0;
        }
        options {
        }
        disk {
             md-flushes yes;
             disk-flushes yes;
             c-plan-ahead 1;
             c-min-rate 100M;
             c-fill-target 20M;
             c-max-rate 4G;
        }
        net {
             after-sb-0pri discard-younger-primary;
             after-sb-1pri discard-secondary;
             after-sb-2pri call-pri-lost-after-sb;
             protocol     C;
             tcp-cork yes;
             max-buffers 20000;
             max-epoch-size 20000;
             sndbuf-size 0;
             rcvbuf-size 0;
        }
   }
   </code></pre>

1. **[A]** NFS drbd aygıtları oluşturma

   <pre><code>sudo vi /etc/drbd.d/<b>NW1</b>-nfs.res
   </code></pre>

   Yeni drbd cihazı için yapılandırma yı ekleyin ve çıkış

   <pre><code>resource <b>NW1</b>-nfs {
        protocol     C;
        disk {
             on-io-error       detach;
        }
        on <b>prod-nfs-0</b> {
             address   <b>10.0.0.6:7790</b>;
             device    /dev/drbd<b>0</b>;
             disk      /dev/<b>vg-NW1-NFS</b>/<b>NW1</b>;
             meta-disk internal;
        }
        on <b>prod-nfs-1</b> {
             address   <b>10.0.0.7:7790</b>;
             device    /dev/drbd<b>0</b>;
             disk      /dev/<b>vg-NW1-NFS</b>/<b>NW1</b>;
             meta-disk internal;
        }
   }
   </code></pre>

   <pre><code>sudo vi /etc/drbd.d/<b>NW2</b>-nfs.res
   </code></pre>

   Yeni drbd cihazı için yapılandırma yı ekleyin ve çıkış

   <pre><code>resource <b>NW2</b>-nfs {
        protocol     C;
        disk {
             on-io-error       detach;
        }
        on <b>prod-nfs-0</b> {
             address   <b>10.0.0.6:7791</b>;
             device    /dev/drbd<b>1</b>;
             disk      /dev/<b>vg-NW2-NFS</b>/<b>NW2</b>;
             meta-disk internal;
        }
        on <b>prod-nfs-1</b> {
             address   <b>10.0.0.7:7791</b>;
             device    /dev/drbd<b>1</b>;
             disk      /dev/<b>vg-NW2-NFS</b>/<b>NW2</b>;
             meta-disk internal;
        }
   }
   </code></pre>

   Drbd cihazı oluşturun ve başlatın

   <pre><code>sudo drbdadm create-md <b>NW1</b>-nfs
   sudo drbdadm create-md <b>NW2</b>-nfs
   sudo drbdadm up <b>NW1</b>-nfs
   sudo drbdadm up <b>NW2</b>-nfs
   </code></pre>

1. **[1]** İlk eşitleme atla

   <pre><code>sudo drbdadm new-current-uuid --clear-bitmap <b>NW1</b>-nfs
   sudo drbdadm new-current-uuid --clear-bitmap <b>NW2</b>-nfs
   </code></pre>

1. **[1]** Birincil düğümü ayarlama

   <pre><code>sudo drbdadm primary --force <b>NW1</b>-nfs
   sudo drbdadm primary --force <b>NW2</b>-nfs
   </code></pre>

1. **[1]** Yeni drbd cihazlar senkronize olana kadar bekleyin

   <pre><code>sudo drbdsetup wait-sync-resource NW1-nfs
   sudo drbdsetup wait-sync-resource NW2-nfs
   </code></pre>

1. **[1]** Drbd cihazlarda dosya sistemleri oluşturma

   <pre><code>sudo mkfs.xfs /dev/drbd0
   sudo mkdir /srv/nfs/NW1
   sudo chattr +i /srv/nfs/NW1
   sudo mount -t xfs /dev/drbd0 /srv/nfs/NW1
   sudo mkdir /srv/nfs/NW1/sidsys
   sudo mkdir /srv/nfs/NW1/sapmntsid
   sudo mkdir /srv/nfs/NW1/trans
   sudo mkdir /srv/nfs/NW1/ASCS
   sudo mkdir /srv/nfs/NW1/ASCSERS
   sudo mkdir /srv/nfs/NW1/SCS
   sudo mkdir /srv/nfs/NW1/SCSERS
   sudo umount /srv/nfs/NW1

   sudo mkfs.xfs /dev/drbd1
   sudo mkdir /srv/nfs/NW2
   sudo chattr +i /srv/nfs/NW2
   sudo mount -t xfs /dev/drbd1 /srv/nfs/NW2
   sudo mkdir /srv/nfs/NW2/sidsys
   sudo mkdir /srv/nfs/NW2/sapmntsid
   sudo mkdir /srv/nfs/NW2/trans
   sudo mkdir /srv/nfs/NW2/ASCS
   sudo mkdir /srv/nfs/NW2/ASCSERS
   sudo mkdir /srv/nfs/NW2/SCS
   sudo mkdir /srv/nfs/NW2/SCSERS
   sudo umount /srv/nfs/NW2
   </code></pre>

1. **[A]** Kurulum drbd bölünmüş beyin algılama

   Bir ana bilgisayardan diğerine verileri senkronize etmek için drbd kullanırken, sözde bölünmüş beyin oluşabilir. Bölünmüş beyin, her iki küme düğümünün de drbd cihazını birincil olarak tanıttığı ve senkronize olmadığı bir senaryodur. Bu nadir bir durum olabilir ama yine de işlemek ve mümkün olduğunca hızlı bir bölünmüş beyin çözmek istiyorum. Bu nedenle bölünmüş bir beyin meydana geldiğinde haberdar olmak önemlidir.

   Bölünmüş bir beyin bildiriminin nasıl ayarlandığına dair [resmi drbd belgelerini](https://www.linbit.com/drbd-user-guide/users-guide-drbd-8-4/#s-split-brain-notification) okuyun.

   Bölünmüş bir beyin senaryosundan otomatik olarak kurtulmak da mümkündür. Daha fazla bilgi için [Otomatik bölünmüş beyin kurtarma politikalarını](https://www.linbit.com/drbd-user-guide/users-guide-drbd-8-4/#s-automatic-split-brain-recovery-configuration) okuyun
   
### <a name="configure-cluster-framework"></a>Küme Çerçeveyi Yapılandırma

1. **[1]** SAP sistemi NW1 için NFS drbd aygıtlarını küme yapılandırmasına ekleme

   > [!IMPORTANT]
   > Son testler, netcat'in biriktirme listesi ve yalnızca bir bağlantı taşıma sınırlaması nedeniyle isteklere yanıt vermeyi bıraktığı durumları ortaya çıkardı. Netcat kaynağı Azure Yük dengeleyici isteklerini dinlemeyi durdurur ve kayan IP kullanılamaz hale gelir.  
   > Mevcut Pacemaker kümeleri için, geçmişte netcat'i socat ile değiştirmemizi tavsiye ettik. Şu anda, paket kaynak aracılarının bir parçası olan azure-lb kaynak aracısını aşağıdaki paket sürüm gereksinimleriyle birlikte kullanmanızı öneririz:
   > - SLES 12 SP4/SP5 için, sürüm en az kaynak-aracılar-4.3.018.a7fb5035-3.30.1 olmalıdır.  
   > - SLES 15/15 SP1 için, sürüm en az kaynak-ajanlar-4.3.0184.6ee15eb2-4.13.1 olmalıdır.  
   >
   > Değişikliğin kısa bir kapalı kalma süresi gerektireceğini unutmayın.  
   > Varolan Pacemaker kümeleri için yapılandırma Azure [Load-Balancer Detection Hardening'de](https://www.suse.com/support/kb/doc/?id=7024128)açıklandığı gibi socat kullanmak üzere zaten değiştirildiyse, hemen azure-lb kaynak aracısına geçmeniz gerek yoktur.

   <pre><code>sudo crm configure rsc_defaults resource-stickiness="200"

   # Enable maintenance mode
   sudo crm configure property maintenance-mode=true
   
   sudo crm configure primitive drbd_<b>NW1</b>_nfs \
     ocf:linbit:drbd \
     params drbd_resource="<b>NW1</b>-nfs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"
   
   sudo crm configure ms ms-drbd_<b>NW1</b>_nfs drbd_<b>NW1</b>_nfs \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true" interleave="true"
   
   sudo crm configure primitive fs_<b>NW1</b>_sapmnt \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd0 \
     directory=/srv/nfs/<b>NW1</b>  \
     fstype=xfs \
     op monitor interval="10s"
   
   sudo crm configure primitive nfsserver systemd:nfs-server \
     op monitor interval="30s"
   sudo crm configure clone cl-nfsserver nfsserver

   sudo crm configure primitive exportfs_<b>NW1</b> \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW1</b>" \
     options="rw,no_root_squash,crossmnt" clientspec="*" fsid=1 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive vip_<b>NW1</b>_nfs \
     IPaddr2 \
     params ip=<b>10.0.0.4</b> cidr_netmask=<b>24</b> op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW1</b>_nfs azure-lb port=<b>61000</b>
   
   sudo crm configure group g-<b>NW1</b>_nfs \
     fs_<b>NW1</b>_sapmnt exportfs_<b>NW1</b> nc_<b>NW1</b>_nfs vip_<b>NW1</b>_nfs
   
   sudo crm configure order o-<b>NW1</b>_drbd_before_nfs inf: \
     ms-drbd_<b>NW1</b>_nfs:promote g-<b>NW1</b>_nfs:start
   
   sudo crm configure colocation col-<b>NW1</b>_nfs_on_drbd inf: \
     g-<b>NW1</b>_nfs ms-drbd_<b>NW1</b>_nfs:Master
   </code></pre>

1. **[1]** SAP sistemi NW2 için NFS drbd aygıtlarını küme yapılandırmasına ekleme

   <pre><code># Enable maintenance mode
   sudo crm configure property maintenance-mode=true
   
   sudo crm configure primitive drbd_<b>NW2</b>_nfs \
     ocf:linbit:drbd \
     params drbd_resource="<b>NW2</b>-nfs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"
   
   sudo crm configure ms ms-drbd_<b>NW2</b>_nfs drbd_<b>NW2</b>_nfs \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true" interleave="true"
   
   sudo crm configure primitive fs_<b>NW2</b>_sapmnt \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd1 \
     directory=/srv/nfs/<b>NW2</b>  \
     fstype=xfs \
     op monitor interval="10s"
   
   sudo crm configure primitive exportfs_<b>NW2</b> \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NW2</b>" \
     options="rw,no_root_squash,crossmnt" clientspec="*" fsid=2 wait_for_leasetime_on_stop=true op monitor interval="30s"
   
   sudo crm configure primitive vip_<b>NW2</b>_nfs \
     IPaddr2 \
     params ip=<b>10.0.0.5</b> cidr_netmask=<b>24</b> op monitor interval=10 timeout=20
   
   sudo crm configure primitive nc_<b>NW2</b>_nfs azure-lb port=<b>61001</b>
   
   sudo crm configure group g-<b>NW2</b>_nfs \
     fs_<b>NW2</b>_sapmnt exportfs_<b>NW2</b> nc_<b>NW2</b>_nfs vip_<b>NW2</b>_nfs
   
   sudo crm configure order o-<b>NW2</b>_drbd_before_nfs inf: \
     ms-drbd_<b>NW2</b>_nfs:promote g-<b>NW2</b>_nfs:start
   
   sudo crm configure colocation col-<b>NW2</b>_nfs_on_drbd inf: \
     g-<b>NW2</b>_nfs ms-drbd_<b>NW2</b>_nfs:Master
   </code></pre>

   `exportfs` Küme `crossmnt` kaynaklarındaki seçenek, eski SLES sürümleriyle geriye dönük uyumluluk için belgelerimizde mevcuttur.  

1. **[1]** Bakım modunu devre dışı
   
   <pre><code>sudo crm configure property maintenance-mode=false
   </code></pre>

## <a name="next-steps"></a>Sonraki adımlar

* [SAP ASCS'yi ve veritabanını yükleme](high-availability-guide-suse.md)
* [AZURE Sanal Makineler SAP için planlama ve uygulama][planning-guide]
* [SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* Azure Sanal M'lerde SAP HANA'nın yüksek kullanılabilirlik oluşturmasını ve olağanüstü kurtarma yı planlamayı öğrenmek için Azure [Sanal Makinelerde (VM) SAP HANA'nın Yüksek Kullanılabilirliği bölümüne][sap-hana-ha] bakın
