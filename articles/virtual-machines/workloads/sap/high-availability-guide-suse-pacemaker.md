---
title: Azure'da SLES'te Kalp Pili Ayarlama | Microsoft Dokümanlar
description: Azure'da SUSE Linux Enterprise Server'da Pacemaker'ı ayarlama
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
ms.date: 03/17/2020
ms.author: radeltch
ms.openlocfilehash: 9d3d0ddbd1282827f17cd82228fcf0f3fba3a60f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471991"
---
# <a name="setting-up-pacemaker-on-suse-linux-enterprise-server-in-azure"></a>Azure'da SUSE Linux Enterprise Server'da Pacemaker'ı ayarlama

[planning-guide]:planning-guide.md
[deployment-guide]:deployment-guide.md
[dbms-guide]:dbms-guide.md
[sap-hana-ha]:sap-hana-high-availability.md
[virtual-machines-linux-maintenance]:../../maintenance-and-updates.md#maintenance-that-doesnt-require-a-reboot
[virtual-machines-windows-maintenance]:../../maintenance-and-updates.md#maintenance-that-doesnt-require-a-reboot
[sles-nfs-guide]:high-availability-guide-suse-nfs.md
[sles-guide]:high-availability-guide-suse.md

Azure'da bir Pacemaker kümesi kurmak için iki seçenek vardır. Başarısız bir düğümü Azure API'leri üzerinden yeniden başlatmayı sağlayan bir eskrim aracısı veya bir SBD aygıtı kullanabilirsiniz.

SBD aygıtı, iSCSI hedef sunucusu gibi davranan ve Bir SBD aygıtı sağlayan en az bir ek sanal makine gerektirir. Ancak bu iSCSI hedef sunucuları diğer Pacemaker kümeleriyle paylaşılabilir. SBD aygıtı kullanmanın avantajı, zaman içinde daha hızlı bir arızadır ve şirket içinde SBD aygıtları kullanıyorsanız, kalp pili kümesini nasıl çalıştırdığınızkonusunda herhangi bir değişiklik gerektirmez. Bir Pacemaker kümesi için en fazla üç SBD aygıtı nı kullanarak bir SBD aygıtının kullanılamamasına izin verebilirsiniz( örneğin iSCSI hedef sunucusunun işletim sistemi yaması sırasında). Pacemaker başına birden fazla SBD aygıtı kullanmak istiyorsanız, birden çok iSCSI hedef sunucusu dağıttığınızdan ve her iSCSI hedef sunucusundan bir SBD bağladığınızdan emin olun. Bir Veya üç adet SBD cihazı kullanmanızı öneririz. Yalnızca iki SBD aygıtı yapılandırırsanız ve bunlardan biri kullanılamıyorsa, kalp pili otomatik olarak bir küme düğümü çitleme zedemeyecektir. Bir iSCSI hedef sunucusu çöktüğinde çit leyebilmek istiyorsanız, üç SBD aygıtı ve bu nedenle üç iSCSI hedef sunucusu kullanmanız gerekir.

Bir sanal makineye yatırım yapmak istemiyorsanız, Azure Çit aracısını da kullanabilirsiniz. Dezavantajı bir kaynak durdurma başarısız olursa veya küme düğümleri artık birbirleri iletişim kuramıyorsa bir failover 10 ila 15 dakika sürebilir.

![SLES'te Kalp Pili'ne genel bakış](./media/high-availability-guide-suse-pacemaker/pacemaker.png)

>[!IMPORTANT]
> Linux Pacemaker kümelenmiş düğümleri ve SBD aygıtları planlarken ve dağıtırken, ilgili VM'ler ile SBD aygıtını barındıran VM(ler) arasındaki yönlendirmenin [NVA'lar](https://azure.microsoft.com/solutions/network-appliances/)gibi diğer aygıtlardan geçmediğinin tam küme yapılandırmasının genel güvenilirliği için gereklidir. Aksi takdirde, NVA ile ilgili sorunlar ve bakım olayları, genel küme yapılandırmasının kararlılığı ve güvenilirliği üzerinde olumsuz bir etki yaratabilir. Bu tür engelleri önlemek için, Linux Pacemaker kümelenmiş düğümleri ve SBD aygıtlarını planlarken ve dağıtırken Kümelenmiş düğümler ve SBD aygıtları arasındaki trafiği NVA'lar ve benzeri aygıtlar üzerinden yönlendiren NVA'ların veya [Kullanıcı Tanımlı Yönlendirme kurallarının](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) yönlendirme kurallarını tanımlamayın. 
>

## <a name="sbd-fencing"></a>SBD eskrim

Eskrim için bir SBD aygıtı kullanmak istiyorsanız aşağıdaki adımları izleyin.

### <a name="set-up-iscsi-target-servers"></a>iSCSI hedef sunucularını ayarlama

Öncelikle iSCSI hedef sanal makineleri oluşturmanız gerekir. iSCSI hedef sunucuları birden çok Pacemaker kümesiyle paylaşılabilir.

1. Yeni SLES 12 SP1 veya daha yüksek sanal makineleri dağıtın ve ssh üzerinden bunlara bağlanın. Makinelerin büyük olması gerekmez. Standard_E2s_v3 veya Standard_D2s_v3 gibi sanal bir makine boyutu yeterlidir. Os diskinin Premium depolamasını kullandığınızdan emin olun.

Tüm **iSCSI hedef sanal makinelerde**aşağıdaki komutları çalıştırın.

1. SLES'i Güncelleştir

   <pre><code>sudo zypper update
   </code></pre>

   > [!NOTE]
   > İşletim sistemi'ni yükselttikten veya güncelledikten sonra işletim sistemi yeniden başlatmanız gerekebilir. 

1. Paketleri kaldırma

   Targetcli ve SLES 12 SP3 ile bilinen bir sorunu önlemek için aşağıdaki paketleri kaldırın. Bulunamayan paketlerle ilgili hataları yoksayabilirsiniz

   <pre><code>sudo zypper remove lio-utils python-rtslib python-configshell targetcli
   </code></pre>

1. iSCSI hedef paketlerini yükleyin

   <pre><code>sudo zypper install targetcli-fb dbus-1-python
   </code></pre>

1. iSCSI hedef hizmetini etkinleştirme

   <pre><code>sudo systemctl enable targetcli
   sudo systemctl start targetcli
   </code></pre>

### <a name="create-iscsi-device-on-iscsi-target-server"></a>iSCSI hedef sunucusunda iSCSI aygıtı oluşturma

SAP sistemleriniz tarafından kullanılan kümeler için iSCSI diskleri oluşturmak için tüm **iSCSI hedef sanal makinelerde** aşağıdaki komutları çalıştırın. Aşağıdaki örnekte, birden çok küme için SBD aygıtları oluşturulur. Birden çok küme için bir iSCSI hedef sunucusunasıl kullanacağınızı gösterir. SBD aygıtları işletim sistemi diskine yerleştirilir. Yeterli alana sahip olduğundan emin olun.

**`nfs`** NFS kümesini tanımlamak için kullanılır, **ascsnw1** **NW1**ASCS kümesini tanımlamak için kullanılır, **dbnw1** **NW1**veritabanı kümesini tanımlamak için kullanılır, **nfs-0** ve **nfs-1** NFS küme düğümlerinin ana adlarını, **nw1-xscs-0** ve **nw1-xscs-1** **NW1** ASCS küme düğümlerinin ana adları, **nw1-db-0** ve **nw1-db-1** veritabanı küme düğümlerinin ana adlarıdır. Bunları küme düğümlerinizin ana bilgisayar adları ve SAP sisteminizin SID'si ile değiştirin.

<pre><code># Create the root folder for all SBD devices
sudo mkdir /sbd

# Create the SBD device for the NFS server
sudo targetcli backstores/fileio create sbdnfs /sbd/sbdnfs 50M write_back=false
sudo targetcli iscsi/ create iqn.2006-04.nfs.local:nfs
sudo targetcli iscsi/iqn.2006-04.nfs.local:nfs/tpg1/luns/ create /backstores/fileio/sbdnfs
sudo targetcli iscsi/iqn.2006-04.nfs.local:nfs/tpg1/acls/ create iqn.2006-04.<b>nfs-0.local:nfs-0</b>
sudo targetcli iscsi/iqn.2006-04.nfs.local:nfs/tpg1/acls/ create iqn.2006-04.<b>nfs-1.local:nfs-1</b>

# Create the SBD device for the ASCS server of SAP System NW1
sudo targetcli backstores/fileio create sbdascs<b>nw1</b> /sbd/sbdascs<b>nw1</b> 50M write_back=false
sudo targetcli iscsi/ create iqn.2006-04.ascs<b>nw1</b>.local:ascs<b>nw1</b>
sudo targetcli iscsi/iqn.2006-04.ascs<b>nw1</b>.local:ascs<b>nw1</b>/tpg1/luns/ create /backstores/fileio/sbdascs<b>nw1</b>
sudo targetcli iscsi/iqn.2006-04.ascs<b>nw1</b>.local:ascs<b>nw1</b>/tpg1/acls/ create iqn.2006-04.<b>nw1-xscs-0.local:nw1-xscs-0</b>
sudo targetcli iscsi/iqn.2006-04.ascs<b>nw1</b>.local:ascs<b>nw1</b>/tpg1/acls/ create iqn.2006-04.<b>nw1-xscs-1.local:nw1-xscs-1</b>

# Create the SBD device for the database cluster of SAP System NW1
sudo targetcli backstores/fileio create sbddb<b>nw1</b> /sbd/sbddb<b>nw1</b> 50M write_back=false
sudo targetcli iscsi/ create iqn.2006-04.db<b>nw1</b>.local:db<b>nw1</b>
sudo targetcli iscsi/iqn.2006-04.db<b>nw1</b>.local:db<b>nw1</b>/tpg1/luns/ create /backstores/fileio/sbddb<b>nw1</b>
sudo targetcli iscsi/iqn.2006-04.db<b>nw1</b>.local:db<b>nw1</b>/tpg1/acls/ create iqn.2006-04.<b>nw1-db-0.local:nw1-db-0</b>
sudo targetcli iscsi/iqn.2006-04.db<b>nw1</b>.local:db<b>nw1</b>/tpg1/acls/ create iqn.2006-04.<b>nw1-db-1.local:nw1-db-1</b>

# save the targetcli changes
sudo targetcli saveconfig
</code></pre>

Her şeyin doğru ayarlandığını kontrol edebilirsiniz

<pre><code>sudo targetcli ls

o- / .......................................................................................................... [...]
  o- backstores ............................................................................................... [...]
  | o- block ................................................................................... [Storage Objects: 0]
  | o- fileio .................................................................................. [Storage Objects: 3]
  | | o- <b>sbdascsnw1</b> ................................................ [/sbd/sbdascsnw1 (50.0MiB) write-thru activated]
  | | | o- alua .................................................................................... [ALUA Groups: 1]
  | | |   o- default_tg_pt_gp ........................................................ [ALUA state: Active/optimized]
  | | o- <b>sbddbnw1</b> .................................................... [/sbd/sbddbnw1 (50.0MiB) write-thru activated]
  | | | o- alua .................................................................................... [ALUA Groups: 1]
  | | |   o- default_tg_pt_gp ........................................................ [ALUA state: Active/optimized]
  | | o- <b>sbdnfs</b> ........................................................ [/sbd/sbdnfs (50.0MiB) write-thru activated]
  | |   o- alua .................................................................................... [ALUA Groups: 1]
  | |     o- default_tg_pt_gp ........................................................ [ALUA state: Active/optimized]
  | o- pscsi ................................................................................... [Storage Objects: 0]
  | o- ramdisk ................................................................................. [Storage Objects: 0]
  o- iscsi ............................................................................................. [Targets: 3]
  | o- <b>iqn.2006-04.ascsnw1.local:ascsnw1</b> .................................................................. [TPGs: 1]
  | | o- tpg1 ................................................................................ [no-gen-acls, no-auth]
  | |   o- acls ........................................................................................... [ACLs: 2]
  | |   | o- <b>iqn.2006-04.nw1-xscs-0.local:nw1-xscs-0</b> ............................................... [Mapped LUNs: 1]
  | |   | | o- mapped_lun0 ............................................................ [lun0 fileio/<b>sbdascsnw1</b> (rw)]
  | |   | o- <b>iqn.2006-04.nw1-xscs-1.local:nw1-xscs-1</b> ............................................... [Mapped LUNs: 1]
  | |   |   o- mapped_lun0 ............................................................ [lun0 fileio/<b>sbdascsnw1</b> (rw)]
  | |   o- luns ........................................................................................... [LUNs: 1]
  | |   | o- lun0 .......................................... [fileio/sbdascsnw1 (/sbd/sbdascsnw1) (default_tg_pt_gp)]
  | |   o- portals ..................................................................................... [Portals: 1]
  | |     o- 0.0.0.0:3260 ...................................................................................... [OK]
  | o- <b>iqn.2006-04.dbnw1.local:dbnw1</b> ...................................................................... [TPGs: 1]
  | | o- tpg1 ................................................................................ [no-gen-acls, no-auth]
  | |   o- acls ........................................................................................... [ACLs: 2]
  | |   | o- <b>iqn.2006-04.nw1-db-0.local:nw1-db-0</b> ................................................... [Mapped LUNs: 1]
  | |   | | o- mapped_lun0 .............................................................. [lun0 fileio/<b>sbddbnw1</b> (rw)]
  | |   | o- <b>iqn.2006-04.nw1-db-1.local:nw1-db-1</b> ................................................... [Mapped LUNs: 1]
  | |   |   o- mapped_lun0 .............................................................. [lun0 fileio/<b>sbddbnw1</b> (rw)]
  | |   o- luns ........................................................................................... [LUNs: 1]
  | |   | o- lun0 .............................................. [fileio/sbddbnw1 (/sbd/sbddbnw1) (default_tg_pt_gp)]
  | |   o- portals ..................................................................................... [Portals: 1]
  | |     o- 0.0.0.0:3260 ...................................................................................... [OK]
  | o- <b>iqn.2006-04.nfs.local:nfs</b> .......................................................................... [TPGs: 1]
  |   o- tpg1 ................................................................................ [no-gen-acls, no-auth]
  |     o- acls ........................................................................................... [ACLs: 2]
  |     | o- <b>iqn.2006-04.nfs-0.local:nfs-0</b> ......................................................... [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ................................................................ [lun0 fileio/<b>sbdnfs</b> (rw)]
  |     | o- <b>iqn.2006-04.nfs-1.local:nfs-1</b> ......................................................... [Mapped LUNs: 1]
  |     |   o- mapped_lun0 ................................................................ [lun0 fileio/<b>sbdnfs</b> (rw)]
  |     o- luns ........................................................................................... [LUNs: 1]
  |     | o- lun0 .................................................. [fileio/sbdnfs (/sbd/sbdnfs) (default_tg_pt_gp)]
  |     o- portals ..................................................................................... [Portals: 1]
  |       o- 0.0.0.0:3260 ...................................................................................... [OK]
  o- loopback .......................................................................................... [Targets: 0]
  o- vhost ............................................................................................. [Targets: 0]
  o- xen-pvscsi ........................................................................................ [Targets: 0]
</code></pre>

### <a name="set-up-sbd-device"></a>SBD aygıtını ayarlama

Kümeden son adımda oluşturulan iSCSI aygıtına bağlanın.
Oluşturmak istediğiniz yeni kümenin düğümlerinde aşağıdaki komutları çalıştırın.
Aşağıdaki öğeler, tüm düğümler için geçerli olan **[A]** ile önceden belirlenmiştir, **[1] -** yalnızca düğüm 1 veya **[2]** için geçerlidir - yalnızca düğüm 2 için geçerlidir.

1. **[A]** iSCSI aygıtlarına bağlanma

   İlk olarak, iSCSI ve SBD hizmetlerini etkinleştirin.

   <pre><code>sudo systemctl enable iscsid
   sudo systemctl enable iscsi
   sudo systemctl enable sbd
   </code></pre>

1. **[1]** İlk düğümdeki başlatıcı adını değiştirme

   <pre><code>sudo vi /etc/iscsi/initiatorname.iscsi
   </code></pre>

   Dosyanın içeriğini, örneğin NFS sunucusu için iSCSI hedef sunucusunda iSCSI aygıtını oluştururken kullandığınız ALA'larla eşleşecek şekilde değiştirin.

   <pre><code>InitiatorName=<b>iqn.2006-04.nfs-0.local:nfs-0</b>
   </code></pre>

1. **[2]** İkinci düğümdeki başlatıcı adını değiştirme

   <pre><code>sudo vi /etc/iscsi/initiatorname.iscsi
   </code></pre>

   iSCSI hedef sunucusunda iSCSI aygıtı nı oluştururken kullandığınız ALA'larla eşleşecek şekilde dosyanın içeriğini değiştirme

   <pre><code>InitiatorName=<b>iqn.2006-04.nfs-1.local:nfs-1</b>
   </code></pre>

1. **[A]** iSCSI hizmetini yeniden başlatın

   Şimdi değişikliği uygulamak için iSCSI hizmetini yeniden başlatın

   <pre><code>sudo systemctl restart iscsid
   sudo systemctl restart iscsi
   </code></pre>

   iSCSI aygıtlarını bağlayın. Aşağıdaki örnekte, 10.0.0.17 iSCSI hedef sunucusunun IP adresi ve 3260 varsayılan bağlantı noktasıdır. <b>iqn.2006-04.nfs.local:nfs</b> aşağıdaki ilk komutu çalıştırdığınızda listelenen hedef adlardan biridir (iscsiadm -m keşfi).

   <pre><code>sudo iscsiadm -m discovery --type=st --portal=<b>10.0.0.17:3260</b>   
   sudo iscsiadm -m node -T <b>iqn.2006-04.nfs.local:nfs</b> --login --portal=<b>10.0.0.17:3260</b>
   sudo iscsiadm -m node -p <b>10.0.0.17:3260</b> --op=update --name=node.startup --value=automatic
   
   # If you want to use multiple SBD devices, also connect to the second iSCSI target server
   sudo iscsiadm -m discovery --type=st --portal=<b>10.0.0.18:3260</b>   
   sudo iscsiadm -m node -T <b>iqn.2006-04.nfs.local:nfs</b> --login --portal=<b>10.0.0.18:3260</b>
   sudo iscsiadm -m node -p <b>10.0.0.18:3260</b> --op=update --name=node.startup --value=automatic
   
   # If you want to use multiple SBD devices, also connect to the third iSCSI target server
   sudo iscsiadm -m discovery --type=st --portal=<b>10.0.0.19:3260</b>   
   sudo iscsiadm -m node -T <b>iqn.2006-04.nfs.local:nfs</b> --login --portal=<b>10.0.0.19:3260</b>
   sudo iscsiadm -m node -p <b>10.0.0.19:3260</b> --op=update --name=node.startup --value=automatic
   </code></pre>

   iSCSI aygıtlarının kullanılabilir olduğundan emin olun ve aygıt adını not edin (aşağıdaki örnekte /dev/sde)

   <pre><code>lsscsi
   
   # [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
   # [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
   # [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
   # [5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd
   # <b>[6:0:0:0]    disk    LIO-ORG  sbdnfs           4.0   /dev/sdd</b>
   # <b>[7:0:0:0]    disk    LIO-ORG  sbdnfs           4.0   /dev/sde</b>
   # <b>[8:0:0:0]    disk    LIO-ORG  sbdnfs           4.0   /dev/sdf</b>
   </code></pre>

   Şimdi, iSCSI aygıtlarının dislerini alın.

   <pre><code>ls -l /dev/disk/by-id/scsi-* | grep <b>sdd</b>
   
   # lrwxrwxrwx 1 root root  9 Aug  9 13:20 /dev/disk/by-id/scsi-1LIO-ORG_sbdnfs:afb0ba8d-3a3c-413b-8cc2-cca03e63ef42 -> ../../sdd
   # <b>lrwxrwxrwx 1 root root  9 Aug  9 13:20 /dev/disk/by-id/scsi-36001405afb0ba8d3a3c413b8cc2cca03 -> ../../sdd</b>
   # lrwxrwxrwx 1 root root  9 Aug  9 13:20 /dev/disk/by-id/scsi-SLIO-ORG_sbdnfs_afb0ba8d-3a3c-413b-8cc2-cca03e63ef42 -> ../../sdd
   
   ls -l /dev/disk/by-id/scsi-* | grep <b>sde</b>
   
   # lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-1LIO-ORG_cl1:3fe4da37-1a5a-4bb6-9a41-9a4df57770e4 -> ../../sde
   # <b>lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df -> ../../sde</b>
   # lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-SLIO-ORG_cl1_3fe4da37-1a5a-4bb6-9a41-9a4df57770e4 -> ../../sde
   
   ls -l /dev/disk/by-id/scsi-* | grep <b>sdf</b>
   
   # lrwxrwxrwx 1 root root  9 Aug  9 13:32 /dev/disk/by-id/scsi-1LIO-ORG_sbdnfs:f88f30e7-c968-4678-bc87-fe7bfcbdb625 -> ../../sdf
   # <b>lrwxrwxrwx 1 root root  9 Aug  9 13:32 /dev/disk/by-id/scsi-36001405f88f30e7c9684678bc87fe7bf -> ../../sdf</b>
   # lrwxrwxrwx 1 root root  9 Aug  9 13:32 /dev/disk/by-id/scsi-SLIO-ORG_sbdnfs_f88f30e7-c968-4678-bc87-fe7bfcbdb625 -> ../../sdf
   </code></pre>

   Komut listesi her SBD aygıtı için üç aygıt disi. Yukarıdaki örnekte scsi-3 ile başlayan kimliği kullanmanızı öneririz.

   * **/dev/disk/by-id/scsi-36001405afb0ba8d3a3c413b8cc2cca03**
   * **/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df**
   * **/dev/disk/by-id/scsi-36001405f88f30e7c9684678bc87fe7bf**

1. **[1]** SBD aygıtı nı oluşturma

   İlk küme düğümünde yeni SBD aygıtları oluşturmak için iSCSI aygıtlarının aygıt kimliğini kullanın.

   <pre><code>sudo sbd -d <b>/dev/disk/by-id/scsi-36001405afb0ba8d3a3c413b8cc2cca03</b> -1 60 -4 120 create

   # Also create the second and third SBD devices if you want to use more than one.
   sudo sbd -d <b>/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df</b> -1 60 -4 120 create
   sudo sbd -d <b>/dev/disk/by-id/scsi-36001405f88f30e7c9684678bc87fe7bf</b> -1 60 -4 120 create
   </code></pre>

1. **[A]** SBD config'i uyarla

   SBD config dosyasını açma

   <pre><code>sudo vi /etc/sysconfig/sbd
   </code></pre>

   SBD aygıtının özelliğini değiştirin, kalp pili tümleştirmesini etkinleştirin ve SBD'nin başlangıç modunu değiştirin.

   <pre><code>[...]
   <b>SBD_DEVICE="/dev/disk/by-id/scsi-36001405afb0ba8d3a3c413b8cc2cca03;/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df;/dev/disk/by-id/scsi-36001405f88f30e7c9684678bc87fe7bf"</b>
   [...]
   <b>SBD_PACEMAKER="yes"</b>
   [...]
   <b>SBD_STARTMODE="always"</b>
   [...]
   </code></pre>

   Yapılandırma `softdog` dosyasını oluşturma

   <pre><code>echo softdog | sudo tee /etc/modules-load.d/softdog.conf
   </code></pre>

   Şimdi modülü yükleyin

   <pre><code>sudo modprobe -v softdog
   </code></pre>

## <a name="cluster-installation"></a>Küme yükleme

Aşağıdaki öğeler, tüm düğümler için geçerli olan **[A]** ile önceden belirlenmiştir, **[1] -** yalnızca düğüm 1 veya **[2]** için geçerlidir - yalnızca düğüm 2 için geçerlidir.

1. **[A]** Güncelleme SLES

   <pre><code>sudo zypper update
   </code></pre>

1. **[A]** Küme kaynakları için gerekli bileşeni yükleme

   <pre><code>sudo zypper in socat
   </code></pre>

1. **[A]** Küme kaynakları için gerekli olan azure-lb bileşenini yükleyin

   <pre><code>sudo zypper in resource-agents
   </code></pre>

   > [!NOTE]
   > Paket kaynak aracılarının sürümünü denetleyin ve minimum sürüm gereksinimlerinin karşılandıktan emin olun:  
   > - SLES 12 SP4/SP5 için, sürüm en az kaynak-aracılar-4.3.018.a7fb5035-3.30.1 olmalıdır.  
   > - SLES 15/15 SP1 için, sürüm en az kaynak-ajanlar-4.3.0184.6ee15eb2-4.13.1 olmalıdır.  

1. **[A]** İşletim sistemini yapılandırma

   Bazı durumlarda, Pacemaker birçok işlem oluşturur ve bu nedenle izin verilen işlem sayısını tüketir. Böyle bir durumda, küme düğümleri arasındaki bir sinyal başarısız olabilir ve kaynaklarınızın başarısız olmamasına neden olabilir. Aşağıdaki parametreyi ayarlayarak izin verilen maksimum işlemleri artırmanızı öneririz.

   <pre><code># Edit the configuration file
   sudo vi /etc/systemd/system.conf
   
   # Change the DefaultTasksMax
   #DefaultTasksMax=512
   DefaultTasksMax=4096
   
   #and to activate this setting
   sudo systemctl daemon-reload
   
   # test if the change was successful
   sudo systemctl --no-pager show | grep DefaultTasksMax
   </code></pre>

   Kirli önbelleğin boyutunu küçültün. Daha fazla bilgi için, [büyük RAM'li SLES 11/12 sunucularında düşük yazma performansı](https://www.suse.com/support/kb/doc/?id=7010287)na bakın.

   <pre><code>sudo vi /etc/sysctl.conf

   # Change/set the following settings
   vm.dirty_bytes = 629145600
   vm.dirty_background_bytes = 314572800
   </code></pre>

1. **[A]** HA Cluster için bulut-netconfig-azure yapılandırma

   Bulut ağı eklentisinin sanal IP adresini kaldırmasını önlemek için ağ arabiriminin yapılandırma dosyasını aşağıda gösterildiği gibi değiştirin (Pacemaker VIP atamasını denetlemelidir). Daha fazla bilgi için Bkz. [SUSE KB 7023633.](https://www.suse.com/support/kb/doc/?id=7023633) 

   <pre><code># Edit the configuration file
   sudo vi /etc/sysconfig/network/ifcfg-eth0 
   
   # Change CLOUD_NETCONFIG_MANAGE
   # CLOUD_NETCONFIG_MANAGE="yes"
   CLOUD_NETCONFIG_MANAGE="no"
   </code></pre>

1. **[1]** SSH erişimini etkinleştirme

   <pre><code>sudo ssh-keygen
   
   # Enter file in which to save the key (/root/.ssh/id_rsa): -> Press ENTER
   # Enter passphrase (empty for no passphrase): -> Press ENTER
   # Enter same passphrase again: -> Press ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_rsa.pub
   </code></pre>

1. **[2]** SSH erişimini etkinleştirme

   <pre><code>
   sudo ssh-keygen
   
   # Enter file in which to save the key (/root/.ssh/id_rsa): -> Press ENTER
   # Enter passphrase (empty for no passphrase): -> Press ENTER
   # Enter same passphrase again: -> Press ENTER
   
   # insert the public key you copied in the last step into the authorized keys file on the second server
   sudo vi /root/.ssh/authorized_keys   
   
   # copy the public key
   sudo cat /root/.ssh/id_rsa.pub
   </code></pre>

1. **[1]** SSH erişimini etkinleştirme

   <pre><code># insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. **[A]** Çit aracıları yükle
   
   <pre><code>sudo zypper install fence-agents
   </code></pre>

   >[!IMPORTANT]
   > SAP 15 için Suse Linux Enterprise Server kullanıyorsanız, Azure Çit Aracısı'nı kullanmak için ön koşul olan ek modülü etkinleştirmeniz ve ek bileşen yüklemeniz gerektiğini unutmayın. SUSE modülleri ve uzantıları hakkında daha fazla bilgi edinmek için [açıklanan Modüller ve Uzantılar'a](https://www.suse.com/documentation/sles-15/singlehtml/art_modules/art_modules.html)bakın. Azure Python SDK'yı yüklemek için talimatları izleyin. 

   Azure Python SDK'nın nasıl yüklenire ilişkin aşağıdaki talimatlar yalnızca SAP **15**için Suse Enterprise Server için geçerlidir.  

    - Kendi Aboneliğini Getir'i kullanıyorsanız, aşağıdaki talimatları izleyin  

    <pre><code>
    #Activate module PackageHub/15/x86_64
    sudo SUSEConnect -p PackageHub/15/x86_64
    #Install Azure Python SDK
    sudo zypper in python3-azure-sdk
    </code></pre>

     - Kullandıkça Öde aboneliği kullanıyorsanız, aşağıdaki talimatları izleyin  

    <pre><code>#Activate module PackageHub/15/x86_64
    zypper ar https://download.opensuse.org/repositories/openSUSE:/Backports:/SLE-15/standard/ SLE15-PackageHub
    #Install Azure Python SDK
    sudo zypper in python3-azure-sdk
    </code></pre>

1. **[A]** Kurulum ana bilgisayar ad çözümü

   Bir DNS sunucusu kullanabilir veya tüm düğümlerde /etc/hosts'ı değiştirebilirsiniz. Bu örnek, /etc/hosts dosyasının nasıl kullanılacağını gösterir.
   Aşağıdaki komutlarda IP adresini ve ana bilgisayar adını değiştirin. /etc/hosts kullanmanın yararı, kümenizin DNS'den bağımsız hale gelmesidir, bu da tek bir hata noktası olabilir.

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   /etc/hosts'a aşağıdaki satırları ekleyin. IP adresini ve ana bilgisayar adını ortamınıza uyacak şekilde değiştirme   

   <pre><code># IP address of the first cluster node
   <b>10.0.0.6 prod-cl1-0</b>
   # IP address of the second cluster node
   <b>10.0.0.7 prod-cl1-1</b>
   </code></pre>

1. **[1]** Yükleme Kümesi

   <pre><code>sudo ha-cluster-init -u
   
   # ! NTP is not configured to start at system boot.
   # Do you want to continue anyway (y/n)? <b>y</b>
   # /root/.ssh/id_rsa already exists - overwrite (y/n)? <b>n</b>
   # Address for ring0 [10.0.0.6] <b>Press ENTER</b>
   # Port for ring0 [5405] <b>Press ENTER</b>
   # SBD is already configured to use /dev/disk/by-id/scsi-36001405639245768818458b930abdf69;/dev/disk/by-id/scsi-36001405afb0ba8d3a3c413b8cc2cca03;/dev/disk/by-id/scsi-36001405f88f30e7c9684678bc87fe7bf - overwrite (y/n)? <b>n</b>
   # Do you wish to configure an administration IP (y/n)? <b>n</b>
   </code></pre>

1. **[2]** Kümeye düğüm ekleme

   <pre><code>sudo ha-cluster-join
   
   # ! NTP is not configured to start at system boot.
   # Do you want to continue anyway (y/n)? <b>y</b>
   # IP address or hostname of existing node (e.g.: 192.168.1.1) []<b>10.0.0.6</b>
   # /root/.ssh/id_rsa already exists - overwrite (y/n)? <b>n</b>
   </code></pre>

1. **[A]** Hacluster parolasını aynı parolayla değiştirme

   <pre><code>sudo passwd hacluster
   </code></pre>

1. **[A]** Corosync ayarlarını ayarlayın.  

   <pre><code>sudo vi /etc/corosync/corosync.conf
   </code></pre>

   Değerler yoksa veya farklıdeğilse, aşağıdaki kalın içeriği dosyaya ekleyin. Belleğe koruma bakımına izin vermek için belirteci 30000 olarak değiştirdiğinden emin olun. Daha fazla bilgi için, Linux veya [Windows][virtual-machines-windows-maintenance] [için bu makaleye][virtual-machines-linux-maintenance] bakın.

   <pre><code>[...]
     <b>token:          30000
     token_retransmits_before_loss_const: 10
     join:           60
     consensus:      36000
     max_messages:   20</b>
     
     interface { 
        [...] 
     }
     transport:      udpu
   } 
   nodelist {
     node {
      ring0_addr:10.0.0.6
     }
     node {
      ring0_addr:10.0.0.7
     } 
   }
   logging {
     [...]
   }
   quorum {
        # Enable and configure quorum subsystem (default: off)
        # see also corosync.conf.5 and votequorum.5
        provider: corosync_votequorum
        <b>expected_votes: 2</b>
        <b>two_node: 1</b>
   }
   </code></pre>

   Ardından corosync hizmetini yeniden başlatın

   <pre><code>sudo service corosync restart
   </code></pre>

## <a name="create-azure-fence-agent-stonith-device"></a>Azure Fence aracısı STONITH cihazı oluşturma

STONITH aygıtı, Microsoft Azure'a karşı yetkilendirme yapmak için bir Hizmet Sorumlusu kullanır. Hizmet Sorumlusu oluşturmak için aşağıdaki adımları izleyin.

1. Şuraya gidin: <https://portal.azure.com>
1. Azure Active Directory bıçağını açın  
   Özellikler'e gidin ve Dizin Kimliğini yazın. Bu **kiracı kimliği.**
1. Uygulama kayıtlarını tıklatın
1. Yeni Kayıt'ı tıklatın
1. Bir Ad girin, "Yalnızca bu kuruluş dizinindeki hesaplar" seçeneğini belirleyin 
2. Uygulama Türü "Web"i seçin, oturum açma URL'si girin (örneğin http:\//localhost) ve Ekle'yi tıklatın  
   Oturum açma URL'si kullanılmaz ve geçerli bir URL olabilir
1. Sertifikalar ve Sırlar'ı seçin ve ardından Yeni istemci sırrını tıklatın
1. Yeni bir anahtar için açıklama girin, "Asla süresi dolmaz" seçeneğini belirleyin ve Ekle'yi tıklatın
1. Değeri yazın. Hizmet Sorumlusunun **şifresi** olarak kullanılır
1. Genel Bakış’ı seçin. Başvuru Kimliğini yazın. Hizmet Sorumlusunun kullanıcı adı (aşağıdaki adımlardaki**giriş kimliği)** olarak kullanılır.

### <a name="1-create-a-custom-role-for-the-fence-agent"></a>**[1]** Çit aracısı için özel bir rol oluşturun

Hizmet Sorumlusunun Varsayılan olarak Azure kaynaklarına erişim izni yoktur. Kümedeki tüm sanal makineleri başlatmak ve durdurmak (anlaşma yapmak) için Hizmet Sorumlusuna izin vermeniz gerekir. Özel rolü zaten oluşturmadıysanız, [PowerShell](https://docs.microsoft.com/azure/role-based-access-control/custom-roles-powershell#create-a-custom-role) veya Azure [CLI](https://docs.microsoft.com/azure/role-based-access-control/custom-roles-cli) kullanarak oluşturabilirsiniz

Giriş dosyası için aşağıdaki içeriği kullanın. İçeriği aboneliklerinize uyarlamanız, c276fc76-9cd4-44c9-99a7-4fd71546436e ve e91d47c4-76f3-4271-a796-21b4ecfe3624'ü aboneliğinizin kimlikleriyle değiştirmeniz gerekmektedir. Yalnızca bir aboneliğiniz varsa, AssignableScopes'taki ikinci girişi kaldırın.

```json
{
  "Name": "Linux Fence Agent Role",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows to deallocate and start virtual machines",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/deallocate/action",
    "Microsoft.Compute/virtualMachines/start/action", 
    "Microsoft.Compute/virtualMachines/powerOff/action" 
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```

### <a name="a-assign-the-custom-role-to-the-service-principal"></a>**[A]** Özel rolü Hizmet Sorumlusuna atama

Son bölümde oluşturulan özel rolü "Linux Fence Agent Role"yi Hizmet Sorumlusuna atayın. Artık Sahibi rolünü kullanmayın!

1. Git[https://portal.azure.com](https://portal.azure.com)
1. Tüm kaynaklar bıçağını açın
1. İlk küme düğümünün sanal makinesini seçin
1. Erişim denetimine (IAM) tıklayın
1. Rol Atama ekle'yi tıklatın
1. "Linux Fence Agent Role" rolünü seçin
1. Yukarıda oluşturduğunuz uygulamanın adını girin
1. Kaydet’e tıklayın.

İkinci küme düğümü için yukarıdaki adımları yineleyin.

### <a name="1-create-the-stonith-devices"></a>**[1]** STONITH aygıtlarını oluşturun

Sanal makinelerin izinlerini düzenledikten sonra kümedeki STONITH aygıtlarını yapılandırabilirsiniz.

<pre><code># replace the bold string with your subscription ID, resource group, tenant ID, service principal ID and password
sudo crm configure primitive rsc_st_azure stonith:fence_azure_arm \
   params subscriptionId="<b>subscription ID</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" login="<b>login ID</b>" passwd="<b>password</b>"

sudo crm configure property stonith-timeout=900
sudo crm configure property stonith-enabled=true
</code></pre>

## <a name="default-pacemaker-configuration-for-sbd"></a>SBD için Varsayılan Kalp Pili yapılandırması

1. **[1]** Bir STONITH cihazının kullanımını etkinleştirin ve çit gecikmesini ayarlayın

<pre><code>sudo crm configure property stonith-timeout=144
sudo crm configure property stonith-enabled=true

# List the resources to find the name of the SBD device
sudo crm resource list
sudo crm resource stop stonith-sbd
sudo crm configure delete <b>stonith-sbd</b>
sudo crm configure primitive <b>stonith-sbd</b> stonith:external/sbd \
   params pcmk_delay_max="15" \
   op monitor interval="15" timeout="15"
</code></pre>

## <a name="pacemaker-configuration-for-azure-scheduled-events"></a>Azure zamanlanmış etkinlikler için kalp pili yapılandırması

Azure [zamanlanmış etkinlikler](https://docs.microsoft.com/azure/virtual-machines/linux/scheduled-events)sunar. Zamanlanmış olaylar meta-veri hizmeti aracılığıyla sağlanır ve uygulamanın VM kapatma, VM yeniden dağıtım, vb. gibi olaylara hazırlanmak için zaman sağlar. Zamanlanan Azure olayları için kaynak **[aracısı azure olaylarını](https://github.com/ClusterLabs/resource-agents/pull/1161)** izler. Olaylar algılanırsa, aracı etkilenen VM'deki tüm kaynakları durdurmaya ve kümedeki başka bir düğüme taşımayı dener. Bunu başarmak için ek Pacemaker kaynaklarının yapılandırılması gerekir. 

1. **[A]** **Azure olayları** aracısının paketinin zaten yüklendiğinden ve güncel olduğundan emin olun. 

<pre><code>sudo zypper info resource-agents
</code></pre>

2. **[1]** Pacemaker'daki kaynakları yapılandırın. 

<pre><code>
#Place the cluster in maintenance mode
sudo crm configure property maintenance-mode=true

#Create Pacemaker resources for the Azure agent
sudo crm configure primitive rsc_azure-events ocf:heartbeat:azure-events op monitor interval=10s
sudo crm configure clone cln_azure-events rsc_azure-events

#Take the cluster out of maintenance mode
sudo crm configure property maintenance-mode=false
</code></pre>

   > [!NOTE]
   > Hız Makinesi kaynaklarını azure olayları aracısı için yapılandırdıktan sonra, kümeyi bakım moduna veya dışına yerleştirdiğinizde aşağıdaki gibi uyarı iletileri alabilirsiniz:  
     UYARI: cib-bootstrap-seçenekleri: bilinmeyen öznitelik 'hostName_ <strong>hostname</strong>'  
     UYARI: cib-bootstrap-seçenekleri: bilinmeyen öznitelik 'azure-events_globalPullState'  
     UYARI: cib-bootstrap-seçenekleri: bilinmeyen öznitelik 'hostName_ <strong>hostname</strong>'  
   > Bu uyarı iletileri yoksayılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [AZURE Sanal Makineler SAP için planlama ve uygulama][planning-guide]
* [SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* [SUSE Linux Enterprise Server'da Azure VM'lerde NFS için yüksek kullanılabilirlik][sles-nfs-guide]
* [SAP uygulamaları için SUSE Linux Enterprise Server'da Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik][sles-guide]
* Azure Sanal M'lerde SAP HANA'nın yüksek kullanılabilirlik oluşturmasını ve olağanüstü kurtarma yı planlamayı öğrenmek için Azure [Sanal Makinelerde (VM) SAP HANA'nın Yüksek Kullanılabilirliği bölümüne][sap-hana-ha] bakın
