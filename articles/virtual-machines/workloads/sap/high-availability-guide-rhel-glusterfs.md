---
title: GLUSTERFS ÜZERINDE Azure VMs ÜZERINDE RHEL SAP NetWeaver için | Microsoft Dokümanlar
description: SAP NetWeaver için Red Hat Enterprise Linux üzerinde Azure Sanal Makineler'de GlusterFS
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
ms.date: 08/16/2018
ms.author: radeltch
ms.openlocfilehash: 388a2db2c888be541d89c5f4274bd38b37e4ca28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77591923"
---
# <a name="glusterfs-on-azure-vms-on-red-hat-enterprise-linux-for-sap-netweaver"></a>SAP NetWeaver için Red Hat Enterprise Linux üzerinde Azure Sanal Makineler'de GlusterFS

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2009879]:https://launchpad.support.sap.com/#/notes/2009879
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1999351]:https://launchpad.support.sap.com/#/notes/1999351

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability-rhel.md

Bu makalede, sanal makinelerin nasıl dağıtılanabileceği, sanal makineleri yapılandırma ve kullanılabilir sap sisteminin paylaşılan verilerini depolamak için kullanılabilecek bir GlusterFS kümesinin nasıl yüklenir.
Bu kılavuz, iki SAP sistemleri, NW1 ve NW2 tarafından kullanılan GlusterFS kurmak için nasıl açıklanır. Örnekteki kaynakların adları (örneğin sanal makineler, sanal ağlar) kaynak öneki **glust**ile [SAP dosya sunucusu şablonu][template-file-server] kullandığınızı varsayar.

Önce aşağıdaki SAP Notlarını ve bildirilerini okuyun

* SAP Note [1928533], olan:
  * SAP yazılımının dağıtımı için desteklenen Azure VM boyutları listesi
  * Azure VM boyutları için önemli kapasite bilgileri
  * Desteklenen SAP yazılımı ve işletim sistemi (OS) ve veritabanı kombinasyonları
  * Microsoft Azure'da Windows ve Linux için gerekli SAP çekirdeği sürümü

* SAP Note [2015553,] Azure'da SAP destekli SAP yazılım dağıtımları için ön koşulları listeler.
* SAP Note [2002167] Red Hat Enterprise Linux için işletim sistemi ayarlarını tavsiye etti
* SAP Not [2009879] Red Hat Enterprise Linux için SAP HANA Yönergeleri vardır
* SAP Note [2178632,] Azure'da SAP için bildirilen tüm izleme ölçümleri hakkında ayrıntılı bilgilere sahiptir.
* SAP Note [2191498,] Azure'da Linux için gerekli SAP Host Agent sürümüne sahiptir.
* SAP Note [2243692,] Azure'da Linux'ta SAP lisanslama hakkında bilgi edinmiştir.
* SAP Note [1999351,] SAP için Azure Gelişmiş İzleme Uzantısı için ek sorun giderme bilgilerine sahiptir.
* [SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) Linux için gerekli tüm SAP Notları'na sahiptir.
* [Linux'ta SAP için Azure Sanal Makineler planlaması ve uygulaması][planning-guide]
* [Linux'ta SAP için Azure Sanal Makineler dağıtımı (bu makale)][deployment-guide]
* [Linux'ta SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* [Red Hat Gluster Depolama için Ürün Dokümantasyonu](https://access.redhat.com/documentation/red_hat_gluster_storage/)
* Genel RHEL belgeleri
  * [Yüksek Kullanılabilirlik Eklentisi Genel Bakış](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index)
  * [Yüksek Kullanılabilirlik Eklenti Yönetimi](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index)
  * [Yüksek Kullanılabilirlik Eklenti Başvurusu](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index)
* Azure'a özgü RHEL belgeleri:
  * [RHEL Yüksek Kullanılabilirlik Kümeleri için Destek İlkeleri - Küme Üyesi Olarak Microsoft Azure Sanal Makineleri](https://access.redhat.com/articles/3131341)
  * [Microsoft Azure'da Red Hat Enterprise Linux 7.4 (ve sonrası) Yüksek Kullanılabilirlik Kümesi'ni yükleme ve yapılandırma](https://access.redhat.com/articles/3252491)

## <a name="overview"></a>Genel Bakış

Sap NetWeaver, yüksek kullanılabilirlik elde etmek için paylaşılan depolama yı gerektirir. GlusterFS ayrı bir kümede yapılandırılmıştır ve birden çok SAP sistemi tarafından kullanılabilir.

![SAP NetWeaver Yüksek Kullanılabilirlik genel bakış](./media/high-availability-guide-rhel-glusterfs/rhel-glusterfs.png)

## <a name="set-up-glusterfs"></a>GlusterFS'ı ayarlama

Sanal makineler, kullanılabilirlik kümesi ve ağ arabirimleri de dahil olmak üzere gerekli tüm Azure kaynaklarını dağıtmak için github'dan bir Azure Şablonu kullanabilir veya kaynakları el ile dağıtabilirsiniz.

### <a name="deploy-linux-via-azure-template"></a>Azure Şablonu ile Linux'u dağıtma

Azure Marketi, Red Hat Enterprise Linux için yeni sanal makineleri dağıtmak için kullanabileceğiniz bir resim içerir.
Gerekli tüm kaynakları dağıtmak için github'daki hızlı başlangıç şablonlarından birini kullanabilirsiniz. Şablon sanal makineleri, kullanılabilirlik kümesini vb. dağıtır. Şablonu dağıtmak için aşağıdaki adımları izleyin:

1. Azure portalında [SAP dosya sunucusu şablonu][template-file-server] açma
1. Aşağıdaki parametreleri girin
   1. Kaynak Öneki  
      Kullanmak istediğiniz önek girin. Değer, dağıtılan kaynaklar için önek olarak kullanılır.
   2. SAP Sistem Sayısı Bu dosya sunucusunu kullanacak SAP sistemlerinin sayısını girin. Bu, gerekli sayıda disk vb. dağıtılır.
   3. Os Tipi  
      Linux dağıtımlarından birini seçin. Bu örnek için RHEL 7'yi seçin
   4. Admin Kullanıcı Adı, Yönetici Şifresi veya SSH anahtarı  
      Makinede oturum açmak için kullanılabilecek yeni bir kullanıcı oluşturulur.
   5. Alt net kimliği  
      VM'yi, VM'nin atanması gereken tanımlanmış bir alt ağınız olduğu varolan bir VNet'e dağıtmak istiyorsanız, bu alt ağın kimliğini adlandırın. Kimlik genellikle /abonelik/**&lt;abonelik kimliği&gt;**/kaynakGruplar/**&lt;kaynak&gt;grup adı**/providers/Microsoft.Network/virtualNetworks/**&lt;sanal ağ adı&gt;**/subnets/**&lt;subnet adı&gt; ** gibi görünür

### <a name="deploy-linux-manually-via-azure-portal"></a>Azure portalı üzerinden Linux'u el ile dağıtın

Öncelikle bu küme için sanal makineler oluşturmanız gerekir. Daha sonra, bir yük dengeleyici oluşturmak ve arka uç havuzlarında sanal makineleri kullanın. Standart [yük dengeleyicisi](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview)öneririz.  

1. Kaynak Grubu oluşturma
1. Sanal Ağ Oluşturma
1. Kullanılabilirlik Kümesi Oluşturma  
   Max update etki alanını ayarlama
1. Sanal Makine Oluşturma 1  
   Bu örnekte red hat Enterprise Linux 7.4 görüntüsüen az RHEL 7'yi kullanın<https://portal.azure.com/#create/RedHat.RedHatEnterpriseLinux74-ARM>  
   Daha önce oluşturulan Kullanılabilirlik Kümesini seçin  
1. Sanal Makine Oluşturma 2  
   Bu örnekte red hat Enterprise Linux 7.4 görüntüsüen az RHEL 7'yi kullanın<https://portal.azure.com/#create/RedHat.RedHatEnterpriseLinux74-ARM>  
   Daha önce oluşturulan Kullanılabilirlik Kümesini seçin  
1. Her iki SANAL makineye her SAP sistemi için bir veri diski ekleyin.

### <a name="configure-glusterfs"></a>GlusterFS'ı yapılandır

Aşağıdaki öğeler, tüm düğümler için geçerli olan **[A]** ile önceden belirlenmiştir, **[1] -** yalnızca düğüm 1 için geçerlidir, **[2]** - yalnızca düğüm 2 için geçerlidir, **[3]** - yalnızca düğüm 3 için geçerlidir.

1. **[A]** Kurulum ana bilgisayar ad çözümü

   Bir DNS sunucusu kullanabilir veya tüm düğümlerde /etc/hosts'ı değiştirebilirsiniz. Bu örnek, /etc/hosts dosyasının nasıl kullanılacağını gösterir.
   Aşağıdaki komutlarda IP adresini ve ana bilgisayar adını değiştirme

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   /etc/hosts'a aşağıdaki satırları ekleyin. IP adresini ve ana bilgisayar adını ortamınıza uyacak şekilde değiştirme

   <pre><code># IP addresses of the Gluster nodes
   <b>10.0.0.40 glust-0</b>
   <b>10.0.0.41 glust-1</b>
   <b>10.0.0.42 glust-2</b>
   </code></pre>

1. **[A]** Kayıt

   Sanal makinelerinizi kaydedin ve RHEL 7 ve GlusterFS için depolar içeren bir havuza takın

   <pre><code>sudo subscription-manager register
   sudo subscription-manager attach --pool=&lt;pool id&gt;
   </code></pre>

1. **[A]** GlusterFS depolarını etkinleştir

   Gerekli paketleri yüklemek için aşağıdaki depoları etkinleştirin.

   <pre><code>sudo subscription-manager repos --disable "*"
   sudo subscription-manager repos --enable=rhel-7-server-rpms
   sudo subscription-manager repos --enable=rh-gluster-3-for-rhel-7-server-rpms
   </code></pre>
  
1. **[A]** GlusterFS paketleri yükleyin

   Bu paketleri tüm GlusterFS düğümlerine taşırım

   <pre><code>sudo yum -y install redhat-storage-server
   </code></pre>

   Yüklemeden sonra düğümleri yeniden başlatın.

1. **[A]** Güvenlik DuvarIni Değiştir

   İstemci trafiğine Izin vermek için güvenlik duvarı kuralları ekleyin.

   <pre><code># list the available zones
   firewall-cmd --get-active-zones
   
   sudo firewall-cmd --zone=public --add-service=glusterfs --permanent
   sudo firewall-cmd --zone=public --add-service=glusterfs
   </code></pre>

1. **[A]** GlusterFS hizmetini etkinleştirin ve başlatın

   GlusterFS hizmetini tüm düğümlerde başlatın.

   <pre><code>sudo systemctl start glusterd
   sudo systemctl enable glusterd
   </code></pre>

1. **[1]** GluserFS oluşturma

   GlusterFS kümesini oluşturmak için aşağıdaki komutları çalıştırın

   <pre><code>sudo gluster peer probe glust-1
   sudo gluster peer probe glust-2
   
   # Check gluster peer status
   sudo gluster peer status
   
   # Number of Peers: 2
   # 
   # Hostname: glust-1
   # Uuid: 10d43840-fee4-4120-bf5a-de9c393964cd
   # State: Accepted peer request (Connected)
   # 
   # Hostname: glust-2
   # Uuid: 9e340385-12fe-495e-ab0f-4f851b588cba
   # State: Accepted peer request (Connected)
   </code></pre>

1. **[2]** Test eş durumu

   İkinci düğümdeki eş durumunu test edin

   <pre><code>sudo gluster peer status
   # Number of Peers: 2
   #
   # Hostname: glust-0
   # Uuid: 6bc6927b-7ee2-461b-ad04-da123124d6bd
   # State: Peer in Cluster (Connected)
   #
   # Hostname: glust-2
   # Uuid: 9e340385-12fe-495e-ab0f-4f851b588cba
   # State: Peer in Cluster (Connected)
   </code></pre>

1. **[3]** Test eş durumu

   Üçüncü düğümdeki eş durumunu test edin

   <pre><code>sudo gluster peer status
   # Number of Peers: 2
   #
   # Hostname: glust-0
   # Uuid: 6bc6927b-7ee2-461b-ad04-da123124d6bd
   # State: Peer in Cluster (Connected)
   #
   # Hostname: glust-1
   # Uuid: 10d43840-fee4-4120-bf5a-de9c393964cd
   # State: Peer in Cluster (Connected)
   </code></pre>

1. **[A]** LVM oluştur

   Bu örnekte, GlusterFS iki SAP sistemleri, NW1 ve NW2 için kullanılır. Bu SAP sistemleri için LVM yapılandırmaları oluşturmak için aşağıdaki komutları kullanın.

   NW1 için bu komutları kullanın

   <pre><code>sudo pvcreate --dataalignment 1024K /dev/disk/azure/scsi1/lun0
   sudo pvscan
   sudo vgcreate --physicalextentsize 256K rhgs-<b>NW1</b> /dev/disk/azure/scsi1/lun0
   sudo vgscan
   sudo lvcreate -l 50%FREE -n rhgs-<b>NW1</b>/sapmnt
   sudo lvcreate -l 20%FREE -n rhgs-<b>NW1</b>/trans
   sudo lvcreate -l 10%FREE -n rhgs-<b>NW1</b>/sys
   sudo lvcreate -l 50%FREE -n rhgs-<b>NW1</b>/ascs
   sudo lvcreate -l 100%FREE -n rhgs-<b>NW1</b>/aers
   sudo lvscan
   
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW1</b>/sapmnt
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW1</b>/trans
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW1</b>/sys
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW1</b>/ascs
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW1</b>/aers
   
   sudo mkdir -p /rhs/<b>NW1</b>/sapmnt
   sudo mkdir -p /rhs/<b>NW1</b>/trans
   sudo mkdir -p /rhs/<b>NW1</b>/sys
   sudo mkdir -p /rhs/<b>NW1</b>/ascs
   sudo mkdir -p /rhs/<b>NW1</b>/aers
   
   sudo chattr +i /rhs/<b>NW1</b>/sapmnt
   sudo chattr +i /rhs/<b>NW1</b>/trans
   sudo chattr +i /rhs/<b>NW1</b>/sys
   sudo chattr +i /rhs/<b>NW1</b>/ascs
   sudo chattr +i /rhs/<b>NW1</b>/aers

   echo -e "/dev/rhgs-<b>NW1</b>/sapmnt\t/rhs/<b>NW1</b>/sapmnt\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   echo -e "/dev/rhgs-<b>NW1</b>/trans\t/rhs/<b>NW1</b>/trans\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   echo -e "/dev/rhgs-<b>NW1</b>/sys\t/rhs/<b>NW1</b>/sys\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   echo -e "/dev/rhgs-<b>NW1</b>/ascs\t/rhs/<b>NW1</b>/ascs\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   echo -e "/dev/rhgs-<b>NW1</b>/aers\t/rhs/<b>NW1</b>/aers\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   sudo mount -a
   </code></pre>

   NW2 için bu komutları kullanın

   <pre><code>sudo pvcreate --dataalignment 1024K /dev/disk/azure/scsi1/lun1
   sudo pvscan
   sudo vgcreate --physicalextentsize 256K rhgs-<b>NW2</b> /dev/disk/azure/scsi1/lun1
   sudo vgscan
   sudo lvcreate -l 50%FREE -n rhgs-<b>NW2</b>/sapmnt
   sudo lvcreate -l 20%FREE -n rhgs-<b>NW2</b>/trans
   sudo lvcreate -l 10%FREE -n rhgs-<b>NW2</b>/sys
   sudo lvcreate -l 50%FREE -n rhgs-<b>NW2</b>/ascs
   sudo lvcreate -l 100%FREE -n rhgs-<b>NW2</b>/aers
   
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW2</b>/sapmnt
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW2</b>/trans
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW2</b>/sys
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW2</b>/ascs
   sudo mkfs.xfs -f -K -i size=512 -n size=8192 /dev/rhgs-<b>NW2</b>/aers
   
   sudo mkdir -p /rhs/<b>NW2</b>/sapmnt
   sudo mkdir -p /rhs/<b>NW2</b>/trans
   sudo mkdir -p /rhs/<b>NW2</b>/sys
   sudo mkdir -p /rhs/<b>NW2</b>/ascs
   sudo mkdir -p /rhs/<b>NW2</b>/aers
   
   sudo chattr +i /rhs/<b>NW2</b>/sapmnt
   sudo chattr +i /rhs/<b>NW2</b>/trans
   sudo chattr +i /rhs/<b>NW2</b>/sys
   sudo chattr +i /rhs/<b>NW2</b>/ascs
   sudo chattr +i /rhs/<b>NW2</b>/aers
   sudo lvscan
   
   echo -e "/dev/rhgs-<b>NW2</b>/sapmnt\t/rhs/<b>NW2</b>/sapmnt\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   echo -e "/dev/rhgs-<b>NW2</b>/trans\t/rhs/<b>NW2</b>/trans\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   echo -e "/dev/rhgs-<b>NW2</b>/sys\t/rhs/<b>NW2</b>/sys\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   echo -e "/dev/rhgs-<b>NW2</b>/ascs\t/rhs/<b>NW2</b>/ascs\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   echo -e "/dev/rhgs-<b>NW2</b>/aers\t/rhs/<b>NW2</b>/aers\txfs\tdefaults,inode64,nobarrier,noatime,nouuid 0 2" | sudo tee -a /etc/fstab
   sudo mount -a
   </code></pre>

1. **[1]** Dağıtılmış hacmi oluşturma

   NW1 için GlusterFS hacmini oluşturmak ve başlatmak için aşağıdaki komutları kullanın.

   <pre><code>sudo gluster vol create <b>NW1</b>-sapmnt replica 3 glust-0:/rhs/<b>NW1</b>/sapmnt glust-1:/rhs/<b>NW1</b>/sapmnt glust-2:/rhs/<b>NW1</b>/sapmnt force
   sudo gluster vol create <b>NW1</b>-trans replica 3 glust-0:/rhs/<b>NW1</b>/trans glust-1:/rhs/<b>NW1</b>/trans glust-2:/rhs/<b>NW1</b>/trans force
   sudo gluster vol create <b>NW1</b>-sys replica 3 glust-0:/rhs/<b>NW1</b>/sys glust-1:/rhs/<b>NW1</b>/sys glust-2:/rhs/<b>NW1</b>/sys force
   sudo gluster vol create <b>NW1</b>-ascs replica 3 glust-0:/rhs/<b>NW1</b>/ascs glust-1:/rhs/<b>NW1</b>/ascs glust-2:/rhs/<b>NW1</b>/ascs force
   sudo gluster vol create <b>NW1</b>-aers replica 3 glust-0:/rhs/<b>NW1</b>/aers glust-1:/rhs/<b>NW1</b>/aers glust-2:/rhs/<b>NW1</b>/aers force
   
   sudo gluster volume start <b>NW1</b>-sapmnt
   sudo gluster volume start <b>NW1</b>-trans
   sudo gluster volume start <b>NW1</b>-sys
   sudo gluster volume start <b>NW1</b>-ascs
   sudo gluster volume start <b>NW1</b>-aers
   </code></pre>

   NW2 için GlusterFS hacmini oluşturmak ve başlatmak için aşağıdaki komutları kullanın.

   <pre><code>sudo gluster vol create <b>NW2</b>-sapmnt replica 3 glust-0:/rhs/<b>NW2</b>/sapmnt glust-1:/rhs/<b>NW2</b>/sapmnt glust-2:/rhs/<b>NW2</b>/sapmnt force
   sudo gluster vol create <b>NW2</b>-trans replica 3 glust-0:/rhs/<b>NW2</b>/trans glust-1:/rhs/<b>NW2</b>/trans glust-2:/rhs/<b>NW2</b>/trans force
   sudo gluster vol create <b>NW2</b>-sys replica 3 glust-0:/rhs/<b>NW2</b>/sys glust-1:/rhs/<b>NW2</b>/sys glust-2:/rhs/<b>NW2</b>/sys force
   sudo gluster vol create <b>NW2</b>-ascs replica 3 glust-0:/rhs/<b>NW2</b>/ascs glust-1:/rhs/<b>NW2</b>/ascs glust-2:/rhs/<b>NW2</b>/ascs force
   sudo gluster vol create <b>NW2</b>-aers replica 3 glust-0:/rhs/<b>NW2</b>/aers glust-1:/rhs/<b>NW2</b>/aers glust-2:/rhs/<b>NW2</b>/aers force
   
   sudo gluster volume start <b>NW2</b>-sapmnt
   sudo gluster volume start <b>NW2</b>-trans
   sudo gluster volume start <b>NW2</b>-sys
   sudo gluster volume start <b>NW2</b>-ascs
   sudo gluster volume start <b>NW2</b>-aers
   </code></pre>

## <a name="next-steps"></a>Sonraki adımlar

* [SAP ASCS'yi ve veritabanını yükleme](high-availability-guide-rhel.md)
* [AZURE Sanal Makineler SAP için planlama ve uygulama][planning-guide]
* [SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* Yüksek kullanılabilirlik oluşturmayı ve AZURE'da SAP HANA'nın olağanüstü durum kurtarmasını nasıl planlayabilirsiniz (büyük örnekler), Bkz. [SAP HANA (büyük örnekler) yüksek kullanılabilirlik ve Azure'da olağanüstü durum kurtarma.](hana-overview-high-availability-disaster-recovery.md)
* Azure Sanal M'lerde SAP HANA'nın yüksek kullanılabilirlik oluşturmasını ve olağanüstü kurtarma yı planlamayı öğrenmek için Azure [Sanal Makinelerde (VM) SAP HANA'nın Yüksek Kullanılabilirliği bölümüne][sap-hana-ha] bakın
