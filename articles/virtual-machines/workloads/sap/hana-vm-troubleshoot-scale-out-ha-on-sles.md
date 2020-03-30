---
title: SAP HANA, Azure VM sorun gidermede SLES ile HSR-Pacemaker ölçeğini ölçeklendirin| Microsoft Dokümanlar
description: Azure sanal makinelerde çalışan SLES 12 SP3'te SAP HANA Sistem Çoğaltma (HSR) ve Kalp Pili'ni temel alan karmaşık BIR SAP HANA ölçeklendirme yüksek kullanılabilirlik yapılandırmasını denetleme ve sorun giderme kılavuzu
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/24/2018
ms.author: hermannd
ms.openlocfilehash: e93b3412785817050ac53030be9ff2172a678c06
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617132"
---
# <a name="verify-and-troubleshoot-sap-hana-scale-out-high-availability-setup-on-sles-12-sp3"></a>SLES 12 SP3'te SAP HANA ölçeğini yüksek kullanılabilirlik kurulumunuzu doğrulayın ve sorun giderin 

[sles-pacemaker-ha-guide]:high-availability-guide-suse-pacemaker.md
[sles-hana-scale-out-ha-paper]:https://www.suse.com/documentation/suse-best-practices/singlehtml/SLES4SAP-hana-scaleOut-PerfOpt-12/SLES4SAP-hana-scaleOut-PerfOpt-12.html
[sap-hana-iaas-list]:https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html
[suse-pacemaker-support-log-files]:https://www.suse.com/support/kb/doc/?id=7022702
[azure-linux-multiple-nics]:https://docs.microsoft.com/azure/virtual-machines/linux/multiple-nics
[suse-cloud-netconfig]:https://www.suse.com/c/multi-nic-cloud-netconfig-ec2-azure/
[sap-list-port-numbers]:https://help.sap.com/viewer/ports
[sles-12-ha-paper]:https://www.suse.com/documentation/sle-ha-12/pdfdoc/book_sleha/book_sleha.pdf
[sles-zero-downtime-paper]:https://www.suse.com/media/presentation/TUT90846_towards_zero_downtime%20_how_to_maintain_sap_hana_system_replication_clusters.pdf
[sap-nw-ha-guide-sles]:high-availability-guide-suse.md
[sles-12-for-sap]:https://www.suse.com/media/white-paper/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf


Bu makale, Azure sanal makinelerde (VM) çalışan SAP HANA ölçeklendirmesi için Pacemaker küme yapılandırmasını denetlemenize yardımcı olur. Küme kurulumu SAP HANA Sistem Çoğaltma (HSR) ve SUSE RPM paketi SAPHanaSR-ScaleOut ile birlikte gerçekleştirildi. Tüm testler sadece SUSE SLES 12 SP3 üzerinde yapılmıştır. Makalenin bölümleri farklı alanları kapsamaktadır ve config dosyalarından örnek komutları ve alıntılar içerir. Bu örnekleri, küme kurulumunu doğrulamak ve denetlemek için bir yöntem olarak öneririz.



## <a name="important-notes"></a>Önemli notlar

SAP HANA sistem çoğaltma ve Pacemaker ile birlikte SAP HANA ölçeklendirme için tüm testler sadece SAP HANA 2.0 ile yapıldı. İşletim sistemi sürümü SAP uygulamaları için SUSE Linux Enterprise Server 12 SP3 idi. En son RPM paketi, SUSE SAPHanaSR-ScaleOut, Pacemaker kümesi kurmak için kullanılmıştır.
SUSE [bu performans için optimize edilmiş kurulumun ayrıntılı][sles-hana-scale-out-ha-paper]bir açıklamasını yayınladı.

SAP HANA scale-out için desteklenen sanal makine türleri için [SAP HANA sertifikalı IaaS dizinine][sap-hana-iaas-list]bakın.

Sap HANA ile ilgili teknik bir sorun vardı- birden çok alt ağ ve vNIC ile birlikte ve HSR kurulumu. Bu sorunun giderildiği en son SAP HANA 2.0 düzeltme ekleri kullanılması zorunludur. Aşağıdaki SAP HANA sürümleri desteklenir: 

* rev2.00.024.04 veya üzeri 
* rev2.00.032 veya üzeri

SUSE'den desteğe ihtiyacınız varsa, bu [kılavuzu][suse-pacemaker-support-log-files]izleyin. Makalede açıklandığı gibi SAP HANA yüksek kullanılabilirlik (HA) kümesi hakkında tüm bilgileri toplayın. SUSE desteğinin daha fazla analiz için bu bilgilere ihtiyacı vardır.

Dahili sınama sırasında, küme kurulumu Azure portalı üzerinden normal zarif VM kapatma ile karıştı. Bu nedenle, küme nin başarısız olduğunu başka yöntemlerle test edersiniz. Çekirdek paniğini zorlamak veya ağları kapatmak veya **msl** kaynağını geçirmek gibi yöntemler kullanın. Aşağıdaki bölümlerdeki ayrıntılara bakın. Varsayım, standart bir kapatma niyeti ile olur olmasıdır. Kasıtlı kapatmanın en iyi örneği bakım içindir. [Planlı bakım](#planned-maintenance)daki ayrıntılara bakın.

Ayrıca, dahili sınama sırasında, küme bakım modundayken el ile SAP HANA devralma işleminden sonra küme kurulumu karıştı. Küme bakım modunu sona erdirmeden önce el ile yeniden değiştirmenizi öneririz. Başka bir seçenek, kümeyi bakım moduna koymadan önce bir başarısızlığı tetiklemektir. Daha fazla bilgi için Bkz. [Planlı bakım.](#planned-maintenance) SUSE'deki belgeler, **crm** komutunu kullanarak kümeyi bu şekilde nasıl sıfırlayabileceğinizi açıklar. Ama daha önce bahsedilen yaklaşım iç test sırasında sağlam ve herhangi bir beklenmedik yan etkileri gösterdi asla.

**CRM geçir** komutunu kullandığınızda, küme yapılandırmasını temizlediğinizden emin olun. Farkında olmayabileceğiniz konum kısıtlamaları ekler. Bu kısıtlamalar küme davranışını etkiler. [Planlı bakım](#planned-maintenance)hakkında daha fazla bilgi edinin.



## <a name="test-system-description"></a>Test sistemi açıklaması

 SAP HANA ölçekli HA doğrulama ve belgelendirmesi için bir kurulum kullanılmıştır. Her biri üç SAP HANA düğümü olan iki sistemden oluşuyordu: bir ana ve iki işçi. Aşağıdaki tabloda VM adları ve dahili IP adresleri listelenebedilir. Takip eden tüm doğrulama örnekleri bu VM'lerde yapıldı. Komut örneklerinde bu VM adlarını ve IP adreslerini kullanarak komutları ve çıktılarını daha iyi anlayabilirsiniz:


| Düğüm türü | VM adı | IP adresi |
| --- | --- | --- |
| Sitede ana düğüm 1 | hso-hana-vm-s1-0 | 10.0.0.30 |
| Şantiye 1'de işçi düğümü 1 | hso-hana-vm-s1-1 | 10.0.0.31 |
| Şantiye 1'de işçi düğümü 2 | hso-hana-vm-s1-2 | 10.0.0.32 |
| | | |
| Sitede ana düğüm 2 | hso-hana-vm-s2-0 | 10.0.0.40 |
| Şantiyede işçi düğümü 1 2 | hso-hana-vm-s2-1 | 10.0.0.41 |
| Şantiye2'de işçi düğümü 2 | hso-hana-vm-s2-2  | 10.0.0.42 |
| | | |
| Çoğunluk yapıcı düğüm | hso-hana-dm | 10.0.0.13 |
| SBD aygıt sunucusu | hso-hana-sbd | 10.0.0.19 |
| | | |
| NFS sunucu 1 | hso-nfs-vm-0 | 10.0.0.15 |
| NFS sunucu 2 | hso-nfs-vm-1 | 10.0.0.14 |



## <a name="multiple-subnets-and-vnics"></a>Birden çok alt ağ ve vNIC

SAP HANA ağ önerilerinin ardından, bir Azure sanal ağında üç alt ağ oluşturuldu. Azure'daki SAP HANA ölçeğipaylaşılmayan modda yüklenmesi gerekir. Yani her düğüm **/hana/data** ve **/hana/log**için yerel disk birimleri kullanır. Düğümler yalnızca yerel disk birimleri kullandığından, depolama için ayrı bir alt ağ tanımlamaya gerek yoktur:

- SAP HANA internode iletişimi için 10.0.2.0/24
- SAP HANA Sistem Çoğaltma (HSR) için 10.0.1.0/24
- Her şey için 10.0.0.0/24

Birden çok ağ kullanma yla ilgili SAP HANA yapılandırması hakkında daha fazla bilgi için [SAP HANA global.ini'ye](#sap-hana-globalini)bakın.

Kümedeki her VM'nin alt ağ sayısına karşılık gelen üç vNI'su vardır. [Birden çok ağ arabirimi kartı yla Azure'da bir Linux sanal makinesinin nasıl oluşturulması,][azure-linux-multiple-nics] Linux VM dağıtılırken Azure'da olası bir yönlendirme sorununu açıklar. Bu özel yönlendirme makalesi yalnızca birden çok vNI'nin kullanımı için geçerlidir. Sorun, SLES 12 SP3'te varsayılan başına SUSE ile çözülür. Daha fazla bilgi için [EC2 ve Azure'da bulut netconfig içeren Multi-NIC'e][suse-cloud-netconfig]bakın.


SAP HANA'nın birden çok ağ kullanacak şekilde doğru şekilde yapılandırıldığından doğrulamak için aşağıdaki komutları çalıştırın. İlk olarak, üç alt ağ için de üç dahili IP adresinin etkin olduğunu işletim sistemi düzeyinde kontrol edin. Alt ağları farklı IP adres aralıklarına sahip olarak tanımladıysanız, komutları uyarlamanız gerekir:

<pre><code>
ifconfig | grep "inet addr:10\."
</code></pre>

Aşağıdaki örnek çıktı, site 2'deki ikinci alt düğümden dir. Eth0, eth1 ve eth2'den üç farklı dahili IP adresi görebilirsiniz:

<pre><code>
inet addr:10.0.0.42  Bcast:10.0.0.255  Mask:255.255.255.0
inet addr:10.0.1.42  Bcast:10.0.1.255  Mask:255.255.255.0
inet addr:10.0.2.42  Bcast:10.0.2.255  Mask:255.255.255.0
</code></pre>


Ardından, ad sunucusu ve HSR için SAP HANA bağlantı noktalarını doğrulayın. SAP HANA ilgili alt ağlarda dinlemelidir. SAP HANA örnek numarasına bağlı olarak, komutları uyarlamanız gerekir. Test sistemi için örnek numarası **00'dır.** Hangi bağlantı noktalarının kullanıldığını bulmanın farklı yolları vardır. 

Aşağıdaki SQL deyimi örnek kimliği, örnek numarası ve diğer bilgileri döndürür:

<pre><code>
select * from "SYS"."M_SYSTEM_OVERVIEW"
</code></pre>

Doğru bağlantı noktası numaralarını bulmak için, örneğin, **Yapılandırma** altındaki HANA Studio'da veya bir SQL deyimi aracılığıyla bakabilirsiniz:

<pre><code>
select * from M_INIFILE_CONTENTS WHERE KEY LIKE 'listen%'
</code></pre>

SAP HANA da dahil olmak üzere SAP yazılım yığınında kullanılan tüm bağlantı noktalarını bulmak [için, tüm SAP ürünlerinin TCP/IP bağlantı noktalarını][sap-list-port-numbers]arayın.

SAP HANA 2.0 test sistemindeki örnek numarası **00** göz önüne alındığında, ad sunucusunun bağlantı noktası numarası **30001'dir.** HSR meta veri iletişimi için bağlantı noktası numarası **40002'dir.** Bir seçenek, bir işçi düğümüoturum ve sonra ana düğüm hizmetlerini denetlemek tir. Bu makale için, site 2'deki ana düğüme bağlanmaya çalışırken 2 numaralı sitede işçi düğümüne işaret ettik.

Ad sunucusu bağlantı noktasını kontrol edin:

<pre><code>
nc -vz 10.0.0.40 30001
nc -vz 10.0.1.40 30001
nc -vz 10.0.2.40 30001
</code></pre>

Internod iletişiminin **10.0.2.0/24**alt netini kullandığını kanıtlamak için, sonuç aşağıdaki örnek çıktıgibi görünmelidir.
Yalnızca **10.0.2.0/24** alt ağı üzerinden bağlantı başarılı olmalıdır:

<pre><code>
nc: connect to 10.0.0.40 port 30001 (tcp) failed: Connection refused
nc: connect to 10.0.1.40 port 30001 (tcp) failed: Connection refused
Connection to 10.0.2.40 30001 port [tcp/pago-services1] succeeded!
</code></pre>

Şimdi HSR bağlantı noktası **40002**için kontrol edin:

<pre><code>
nc -vz 10.0.0.40 40002
nc -vz 10.0.1.40 40002
nc -vz 10.0.2.40 40002
</code></pre>

HSR iletişiminin **10.0.1.0/24**alt ağı kullandığını kanıtlamak için, sonuç aşağıdaki örnek çıktıgibi görünmelidir.
Yalnızca **10.0.1.0/24** alt ağı üzerinden bağlantı başarılı olmalıdır:

<pre><code>
nc: connect to 10.0.0.40 port 40002 (tcp) failed: Connection refused
Connection to 10.0.1.40 40002 port [tcp/*] succeeded!
nc: connect to 10.0.2.40 port 40002 (tcp) failed: Connection refused
</code></pre>



## <a name="corosync"></a>Corosync


**Corosync** config dosyası çoğunluk yapıcı düğüm de dahil olmak üzere kümedeki her düğümde doğru olmalıdır. Bir düğümün küme birlemesi beklendiği gibi çalışmıyorsa, tüm düğümlere el ile **/etc/corosync/corosync.conf** oluşturun veya kopyalayın ve hizmeti yeniden başlatın. 

Test sisteminden **corosync.conf** içeriği bir örnektir.

İlk bölüm **totem**, [Küme yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#cluster-installation)açıklandığı gibi , adım 11. **Mcastaddr**değerini yoksayabilirsiniz. Sadece mevcut girişi tut. **Belirteç** ve **konsensüs** girişleri [Microsoft Azure SAP HANA belgelerine][sles-pacemaker-ha-guide]göre ayarlanmalıdır.

<pre><code>
totem {
    version:    2
    secauth:    on
    crypto_hash:    sha1
    crypto_cipher:  aes256
    cluster_name:   hacluster
    clear_node_high_bit: yes

    token:      30000
    token_retransmits_before_loss_const: 10
    join:       60
    consensus:  36000
    max_messages:   20

    interface {
        ringnumber:     0
        bindnetaddr: 10.0.0.0
        mcastaddr:  239.170.19.232
        mcastport:  5405

        ttl:        1
    }
    transport:      udpu

}
</code></pre>

İkinci bölüm, **günlük**, verilen varsayılanlar değiştirilmemiştir:

<pre><code>
logging {
    fileline:   off
    to_stderr:  no
    to_logfile:     no
    logfile:    /var/log/cluster/corosync.log
    to_syslog:  yes
    debug:      off
    timestamp:  on
    logger_subsys {
        subsys:     QUORUM
        debug:  off
    }
}
</code></pre>

Üçüncü bölümde **düğüm listesini**gösterir. Kümenin tüm düğümleri kendi **düğümleri**ile göstermek zorunda:

<pre><code>
nodelist {
  node {
   ring0_addr:hso-hana-vm-s1-0
   nodeid: 1
   }
  node {
   ring0_addr:hso-hana-vm-s1-1
   nodeid: 2
   }
  node {
   ring0_addr:hso-hana-vm-s1-2
   nodeid: 3
   }
  node {
   ring0_addr:hso-hana-vm-s2-0
   nodeid: 4
   }
  node {
   ring0_addr:hso-hana-vm-s2-1
   nodeid: 5
   }
  node {
   ring0_addr:hso-hana-vm-s2-2
   nodeid: 6
   }
  node {
   ring0_addr:hso-hana-dm
   nodeid: 7
   }
}
</code></pre>

Son bölümde, **çoğunluk**, bu doğru **expected_votes** için değer ayarlamak önemlidir. Çoğunluk yapıcı düğüm de dahil olmak üzere düğüm sayısı olmalıdır. Ve **two_node** değeri **0**olmalıdır. Girişi tamamen kaldırma. Sadece **değeri 0**olarak ayarlayın.

<pre><code>
quorum {
    # Enable and configure quorum subsystem (default: off)
    # see also corosync.conf.5 and votequorum.5
    provider: corosync_votequorum
    expected_votes: 7
    two_node: 0
}
</code></pre>


**Systemctl**ile hizmeti yeniden başlatın:

<pre><code>
systemctl restart corosync
</code></pre>




## <a name="sbd-device"></a>SBD cihazı

Azure VM'de Bir SBD aygıtının nasıl ayarlanış ı [SBD eskrimde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#sbd-fencing)açıklanmıştır.

İlk olarak, kümedeki her düğüm için ACL girişleri varsa SBD sunucusu VM'yi kontrol edin. SBD sunucusu VM'de aşağıdaki komutu çalıştırın:


<pre><code>
targetcli ls
</code></pre>


Test sisteminde, komutun çıktısı aşağıdaki örneğe benzer. **IQN.2006-04.hso-db-0.local:hso-db-0** gibi ACL adları VM'lerde karşılık gelen başlatıcı adlar olarak girilmelidir. Her VM'nin farklı bir taneye ihtiyacı vardır.

<pre><code>
 | | o- sbddbhso ................................................................... [/sbd/sbddbhso (50.0MiB) write-thru activated]
  | |   o- alua ................................................................................................... [ALUA Groups: 1]
  | |     o- default_tg_pt_gp ....................................................................... [ALUA state: Active/optimized]
  | o- pscsi .................................................................................................. [Storage Objects: 0]
  | o- ramdisk ................................................................................................ [Storage Objects: 0]
  o- iscsi ............................................................................................................ [Targets: 1]
  | o- iqn.2006-04.dbhso.local:dbhso ..................................................................................... [TPGs: 1]
  |   o- tpg1 ............................................................................................... [no-gen-acls, no-auth]
  |     o- acls .......................................................................................................... [ACLs: 7]
  |     | o- iqn.2006-04.hso-db-0.local:hso-db-0 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-1.local:hso-db-1 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-2.local:hso-db-2 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-3.local:hso-db-3 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-4.local:hso-db-4 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-5.local:hso-db-5 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-6.local:hso-db-6 .................................................................. [Mapped LUNs: 1]
</code></pre>

Ardından, tüm VM'lerde başlatıcı adlarının farklı olup olmadığını ve daha önce gösterilen girişlere karşılık gelip olmadığını denetleyin. Bu örnek, site 1'deki işçi düğümü 1'den dir:

<pre><code>
cat /etc/iscsi/initiatorname.iscsi
</code></pre>

Çıktı aşağıdaki örneğe benzer:

<pre><code>
##
## /etc/iscsi/iscsi.initiatorname
##
## Default iSCSI Initiatorname.
##
## DO NOT EDIT OR REMOVE THIS FILE!
## If you remove this file, the iSCSI daemon will not start.
## If you change the InitiatorName, existing access control lists
## may reject this initiator.  The InitiatorName must be unique
## for each iSCSI initiator.  Do NOT duplicate iSCSI InitiatorNames.
InitiatorName=iqn.2006-04.hso-db-1.local:hso-db-1
</code></pre>

Ardından, **keşfin** doğru çalıştığını doğrulayın. SBD sunucusu VM IP adresini kullanarak her küme düğümünde aşağıdaki komutu çalıştırın:

<pre><code>
iscsiadm -m discovery --type=st --portal=10.0.0.19:3260
</code></pre>

Çıktı aşağıdaki örnek gibi görünmelidir:

<pre><code>
10.0.0.19:3260,1 iqn.2006-04.dbhso.local:dbhso
</code></pre>

Bir sonraki prova noktası düğümün SDB aygıtını gördüğünü doğrulamaktır. Çoğunluk üreticisi düğüm de dahil olmak üzere her düğüm üzerinde kontrol edin:

<pre><code>
lsscsi | grep dbhso
</code></pre>

Çıktı aşağıdaki örnek gibi görünmelidir. Ancak, adlar farklı olabilir. VM yeniden başlattıktan sonra aygıt adı da değişebilir:

<pre><code>
[6:0:0:0]    disk    LIO-ORG  sbddbhso         4.0   /dev/sdm
</code></pre>

Sistemin durumuna bağlı olarak, bazen sorunları gidermek için iSCSI hizmetlerini yeniden başlatmaya yardımcı olur. Sonra aşağıdaki komutları çalıştırın:

<pre><code>
systemctl restart iscsi
systemctl restart iscsid
</code></pre>


Herhangi bir düğümden, tüm düğümlerin **açık**olup olmadığını kontrol edebilirsiniz. Belirli bir düğümde doğru aygıt adını kullandığınızdan emin olun:

<pre><code>
sbd -d /dev/sdm list
</code></pre>

Çıktı kümedeki her düğüm için **net** gösterilmelidir:

<pre><code>
0       hso-hana-vm-s1-0        clear
1       hso-hana-vm-s2-2        clear
2       hso-hana-vm-s2-1        clear
3       hso-hana-dm     clear
4       hso-hana-vm-s1-1        clear
5       hso-hana-vm-s2-0        clear
6       hso-hana-vm-s1-2        clear
</code></pre>


Başka bir SBD denetimi **sbd** komutunun **döküm** seçeneğidir. Bu örnek komut ve çıkış çoğunluk üreticisi düğüm, cihaz adı **sdd**oldu , **sdm**değil :

<pre><code>
sbd -d /dev/sdd dump
</code></pre>

Çıkış, aygıt adı dışında, tüm düğümlerde aynı görünmelidir:

<pre><code>
==Dumping header on disk /dev/sdd
Header version     : 2.1
UUID               : 9fa6cc49-c294-4f1e-9527-c973f4d5a5b0
Number of slots    : 255
Sector size        : 512
Timeout (watchdog) : 60
Timeout (allocate) : 2
Timeout (loop)     : 1
Timeout (msgwait)  : 120
==Header on disk /dev/sdd is dumped
</code></pre>

SBD için bir denetim daha, başka bir düğüme ileti gönderme olanağıdır. Site 2'deki işçi düğümü 2'ye ileti göndermek için, site 2'deki işçi düğümü 1'de aşağıdaki komutu çalıştırın:

<pre><code>
sbd -d /dev/sdm message hso-hana-vm-s2-2 test
</code></pre>

Hedef VM tarafında, **hso-hana-vm-s2-2** bu örnekte, **/var/log/messages'da**aşağıdaki girişi bulabilirsiniz:

<pre><code>
/dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68:   notice: servant: Received command test from hso-hana-vm-s2-1 on disk /dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68
</code></pre>

**/etc/sysconfig/sbd'deki** girişlerin Azure'da [SUSE Linux Enterprise Server'da Pacemaker'ı Ayarlama'daki](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#sbd-fencing)açıklamaya uygun olup olmadığını kontrol edin. **/etc/iscsi/iscsid.conf'taki** başlangıç ayarının otomatik olarak ayarlı olduğunu doğrulayın.

**/etc/sysconfig/sbd'de**aşağıdaki girişler önemlidir. Gerekirse **kimlik** değerini uyarla:

<pre><code>
SBD_DEVICE="/dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68;"
SBD_PACEMAKER=yes
SBD_STARTMODE=always
SBD_WATCHDOG=yes
</code></pre>


**/etc/iscsi/iscsid.conf'taki**başlangıç ayarını kontrol edin. Gerekli ayar, belgelerde açıklanan aşağıdaki **iscsiadm** komutuyla gerçeklegelmiş olmalıdır. Farklıysa **vi** ile el ile doğrulayın ve uyarlayın.

Bu komut başlangıç davranışını ayarlar:

<pre><code>
iscsiadm -m node --op=update --name=node.startup --value=automatic
</code></pre>

**/etc/iscsi/iscsid.conf'a**bu girişi yapın:

<pre><code>
node.startup = automatic
</code></pre>

Test ve doğrulama sırasında, bir VM'nin yeniden başlatılmasından sonra, SBD aygıtı bazı durumlarda artık görünmüyordu. Başlangıç ayarı ile YaST2'nin gösterdiği arasında bir tutarsızlık vardı. Ayarları denetlemek için aşağıdaki adımları izleyin:

1. YaST2'yi başlatın.
2. Sol **taraftaağ hizmetlerini** seçin.
3. Sağ tarafta **iSCSI Başlatıcısı'na** gidin ve seçin.
4. **Hizmet** sekmesinin altındaki sonraki ekranda düğümün benzersiz başlatıcı adını görürsünüz.
5. Başlatıcı adının üzerinde, Hizmet **Başlangıç** değerinin **Önyükleme yaparken**ayarlandığınızdan emin olun.
6. Değilse, **el ile**değil de **Önyükleme yaparken** ayarlayın.
7. Ardından, üst sekmeyi **Bağlı Hedefler**olarak değiştirin.
8. Bağlı **Hedefler** ekranında, bu örnek gibi SBD cihaz için bir giriş görmelisiniz: **10.0.0.19:3260 iqn.2006-04.dbhso.local:dbhso**.
9. **Başlangıç** değerinin önyüklemede ayarlı olup olmadığını kontrol **edin.**
10. Değilse, **Edit'i** seçin ve değiştirin.
11. Değişiklikleri kaydedin ve YaST2'den çıkın.



## <a name="pacemaker"></a>Kalp pili

Her şey doğru ayarlandıktan sonra, Pacemaker hizmetinin durumunu kontrol etmek için her düğümde aşağıdaki komutu çalıştırabilirsiniz:

<pre><code>
systemctl status pacemaker
</code></pre>

Çıktının üst kısmı aşağıdaki örnek gibi görünmelidir. **Etkin'den** sonraki durumun **yüklü** ve etkin **(çalışan)** olarak gösterilmesi önemlidir. **Yüklendikten** sonraki durum **etkin**olarak gösterilmelidir.

<pre><code>
  pacemaker.service - Pacemaker High Availability Cluster Manager
   Loaded: loaded (/usr/lib/systemd/system/pacemaker.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2018-09-07 05:56:27 UTC; 4 days ago
     Docs: man:pacemakerd
           http://clusterlabs.org/doc/en-US/Pacemaker/1.1-pcs/html/Pacemaker_Explained/index.html
 Main PID: 4496 (pacemakerd)
    Tasks: 7 (limit: 4915)
   CGroup: /system.slice/pacemaker.service
           ├─4496 /usr/sbin/pacemakerd -f
           ├─4499 /usr/lib/pacemaker/cib
           ├─4500 /usr/lib/pacemaker/stonithd
           ├─4501 /usr/lib/pacemaker/lrmd
           ├─4502 /usr/lib/pacemaker/attrd
           ├─4503 /usr/lib/pacemaker/pengine
           └─4504 /usr/lib/pacemaker/crmd
</code></pre>

Ayar hala devre **dışı**bırakılmışsa, aşağıdaki komutu çalıştırın:

<pre><code>
systemctl enable pacemaker
</code></pre>

Pacemaker'da yapılandırılan tüm kaynakları görmek için aşağıdaki komutu çalıştırın:

<pre><code>
crm status
</code></pre>

Çıktı aşağıdaki örnek gibi görünmelidir. Bu **cln** ve **msl** kaynakları çoğunluk üreticisi VM, **hso-hana-dm**durdu olarak gösterilir iyi. Çoğunluk üreticisi düğümüzerinde SAP HANA kurulumu yoktur. Böylece **cln** ve **msl** kaynakları durdurulmuş olarak gösterilir. Bu VMs, **7**doğru toplam sayısını gösterir önemlidir. Kümenin bir parçası olan tüm VM'ler **Çevrimiçi**durumla birlikte listelenmelidir. Geçerli birincil ana düğüm doğru şekilde tanınmalıdır. Bu örnekte, **hso-hana-vm-s1-0:**

<pre><code>
Stack: corosync
Current DC: hso-hana-dm (version 1.1.16-4.8-77ea74d) - partition with quorum
Last updated: Tue Sep 11 15:56:40 2018
Last change: Tue Sep 11 15:56:23 2018 by root via crm_attribute on hso-hana-vm-s1-0

7 nodes configured
17 resources configured

Online: [ hso-hana-dm hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started hso-hana-dm
 Clone Set: cln_SAPHanaTop_HSO_HDB00 [rsc_SAPHanaTop_HSO_HDB00]
     Started: [ hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]
     Stopped: [ hso-hana-dm ]
 Master/Slave Set: msl_SAPHanaCon_HSO_HDB00 [rsc_SAPHanaCon_HSO_HDB00]
     Masters: [ hso-hana-vm-s1-0 ]
     Slaves: [ hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]
     Stopped: [ hso-hana-dm ]
 Resource Group: g_ip_HSO_HDB00
     rsc_ip_HSO_HDB00   (ocf::heartbeat:IPaddr2):       Started hso-hana-vm-s1-0
     rsc_nc_HSO_HDB00   (ocf::heartbeat:anything):      Started hso-hana-vm-s1-0
</code></pre>

Pacemaker önemli bir özelliği bakım modudur. Bu modda, hemen bir küme eylemi kışkırtmadan değişiklikler yapabilirsiniz. Bir örnek vm yeniden başlatma. Tipik bir kullanım örneği, işletim sistemi veya Azure altyapı bakımı planlanır. Bkz. [Planlı bakım.](#planned-maintenance) Pacemaker'ı bakım moduna almak için aşağıdaki komutu kullanın:

<pre><code>
crm configure property maintenance-mode=true
</code></pre>

**CRM durumunu**kontrol ettiğinizde, çıktıda tüm kaynakların **yönetilmeyen**olarak işaretlendiğini fark esiniz. Bu durumda, küme SAP HANA'yı başlatma veya durdurma gibi değişikliklere tepki vermez.
Aşağıdaki örnek, küme bakım modundayken **crm durum** komutunun çıktısını gösterir:

<pre><code>
Stack: corosync
Current DC: hso-hana-dm (version 1.1.16-4.8-77ea74d) - partition with quorum
Last updated: Wed Sep 12 07:48:10 2018
Last change: Wed Sep 12 07:46:54 2018 by root via cibadmin on hso-hana-vm-s2-1

7 nodes configured
17 resources configured

              *** Resource management is DISABLED ***
  The cluster will not attempt to start, stop or recover services

Online: [ hso-hana-dm hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started hso-hana-dm (unmanaged)
 Clone Set: cln_SAPHanaTop_HSO_HDB00 [rsc_SAPHanaTop_HSO_HDB00] (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s1-1 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s1-0 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s1-2 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s2-1 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s2-2 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s2-0 (unmanaged)
     Stopped: [ hso-hana-dm ]
 Master/Slave Set: msl_SAPHanaCon_HSO_HDB00 [rsc_SAPHanaCon_HSO_HDB00] (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Slave hso-hana-vm-s1-1 (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Slave hso-hana-vm-s1-2 (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Slave hso-hana-vm-s2-1 (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Slave hso-hana-vm-s2-2 (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Master hso-hana-vm-s2-0 (unmanaged)
     Stopped: [ hso-hana-dm hso-hana-vm-s1-0 ]
 Resource Group: g_ip_HSO_HDB00
     rsc_ip_HSO_HDB00   (ocf::heartbeat:IPaddr2):       Started hso-hana-vm-s2-0 (unmanaged)
     rsc_nc_HSO_HDB00   (ocf::heartbeat:anything):      Started hso-hana-vm-s2-0 (unmanaged)
</code></pre>


Bu komut örneği küme bakım modunun nasıl sona erdirilir gösterir:

<pre><code>
crm configure property maintenance-mode=false
</code></pre>


Başka bir **crm** komutu, tam küme yapılandırmasını bir düzenleyiciye dönüştürür, böylece onu edebilirsiniz. Değişiklikleri kaydettikten sonra küme uygun eylemleri başlatır:

<pre><code>
crm configure edit
</code></pre>

Tam küme yapılandırmasına bakmak için **crm göster** seçeneğini kullanın:

<pre><code>
crm configure show
</code></pre>



Küme kaynaklarının hatalarından **sonra, crm durum** komutu **Başarısız Eylemlerin**listesini gösterir. Bu çıktının aşağıdaki örneğine bakın:


<pre><code>
Stack: corosync
Current DC: hso-hana-dm (version 1.1.16-4.8-77ea74d) - partition with quorum
Last updated: Thu Sep 13 07:30:44 2018
Last change: Thu Sep 13 07:30:20 2018 by root via crm_attribute on hso-hana-vm-s1-0

7 nodes configured
17 resources configured

Online: [ hso-hana-dm hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started hso-hana-dm
 Clone Set: cln_SAPHanaTop_HSO_HDB00 [rsc_SAPHanaTop_HSO_HDB00]
     Started: [ hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]
     Stopped: [ hso-hana-dm ]
 Master/Slave Set: msl_SAPHanaCon_HSO_HDB00 [rsc_SAPHanaCon_HSO_HDB00]
     Masters: [ hso-hana-vm-s1-0 ]
     Slaves: [ hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]
     Stopped: [ hso-hana-dm hso-hana-vm-s2-0 ]
 Resource Group: g_ip_HSO_HDB00
     rsc_ip_HSO_HDB00   (ocf::heartbeat:IPaddr2):       Started hso-hana-vm-s1-0
     rsc_nc_HSO_HDB00   (ocf::heartbeat:anything):      Started hso-hana-vm-s1-0

Failed Actions:
* rsc_SAPHanaCon_HSO_HDB00_monitor_60000 on hso-hana-vm-s2-0 'unknown error' (1): call=86, status=complete, exitreason='none',
    last-rc-change='Wed Sep 12 17:01:28 2018', queued=0ms, exec=277663ms
</code></pre>

Hatalardan sonra küme temizleme yapmak gerekir. **CRM** komutunu yeniden kullanın ve bu başarısız eylem girişlerinden kurtulmak için komut seçeneğini **kullanın.** Karşılık gelen küme kaynağını aşağıdaki gibi adlandırın:

<pre><code>
crm resource cleanup rsc_SAPHanaCon_HSO_HDB00
</code></pre>

Komut, çıktıyı aşağıdaki örnek gibi döndürmelidir:

<pre><code>
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-dm
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s1-0
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s1-1
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s1-2
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s2-0
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s2-1
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s2-2
Waiting for 7 replies from the CRMd....... OK
</code></pre>



## <a name="failover-or-takeover"></a>Failover veya devralma

[Önemli notlarda](#important-notes)tartışıldığı gibi, küme nin başarısız olmasını veya SAP HANA HSR devralmasını test etmek için standart bir zarif kapatma kullanmamalısınız. Bunun yerine, çekirdek paniğini tetiklemenizi, kaynak geçişini zorlamanızı veya bir VM'nin işletim sistemi düzeyindeki tüm ağları kapatmanızı öneririz. Başka bir yöntem ** \<crm\> düğüm bekleme** komutudur. [SUSE belgesine][sles-12-ha-paper]bakın. 

Aşağıdaki üç örnek komut bir kümenin başarısız olduğunu zorlayabilir:

<pre><code>
echo c &gt /proc/sysrq-trigger

crm resource migrate msl_SAPHanaCon_HSO_HDB00 hso-hana-vm-s2-0 force

wicked ifdown eth0
wicked ifdown eth1
wicked ifdown eth2
......
wicked ifdown eth&ltn&gt
</code></pre>

[Planlı bakım](#planned-maintenance)da açıklandığı gibi, küme faaliyetlerini izlemek için iyi bir yol **saat** komutu ile **SAPHanaSR-showAttr** çalıştırmaktır:

<pre><code>
watch SAPHanaSR-showAttr
</code></pre>

Ayrıca, sap python komut dosyasından gelen SAP HANA yatay duruma bakmaya da yardımcı olur. Küme kurulumu bu durum değerini arıyor. Bir işçi düğümü başarısızlığı düşündüğünüzde belli olur. Bir işçi düğümü aşağı iner, SAP HANA hemen tüm ölçek-out sisteminin sağlık için bir hata döndürmez. 

Gereksiz başarısızlıkları önlemek için bazı yeniden denemeler vardır. Küme yalnızca durum **Ok,** return value **4,hata** **error**, return value **1'den**değişirse tepki verir. **SAPHanaSR-showAttr** çıktı devlet **çevrimdışı**bir VM gösterirse Bu yüzden doğrudur. Ama henüz birincil ve ikincil geçiş için bir faaliyet yok. SAP HANA hata döndürmediği sürece küme etkinliği tetiklenmez.

SAP Python komut dosyasını aşağıdaki gibi arayarak kullanıcı ** \<HANA\>SID adm** olarak SAP HANA yatay sistem durumunu izleyebilirsiniz. Yolu uyarlamanız gerekebilir:

<pre><code>
watch python /hana/shared/HSO/exe/linuxx86_64/HDB_2.00.032.00.1533114046_eeaf4723ec52ed3935ae0dc9769c9411ed73fec5/python_support/landscapeHostConfiguration.py
</code></pre>

Bu komutun çıktısı aşağıdaki örnek gibi görünmelidir. **Ana Bilgisayar Durumu** sütunu ve genel ana bilgisayar **durumu** her ikisi de önemlidir. Gerçek çıktı, ek sütunlarla daha geniştir.
Çıktı tablosunu bu belgede daha okunabilir hale getirmek için, sağ taraftaki çoğu sütun silindi:

<pre><code>
| Host             | Host   | Host   | Failover | Remove | 
|                  | Active | Status | Status   | Status | 
|                  |        |        |          |        | 
| ---------------- | ------ | ------ | -------- | ------ |    .......
| hso-hana-vm-s2-0 | yes    | ok     |          |        |        
| hso-hana-vm-s2-1 | yes    | ok     |          |        |         
| hso-hana-vm-s2-2 | yes    | ok     |          |        |        

overall host status: ok
</code></pre>


Geçerli küme etkinliklerini denetlemek için başka bir komut daha var. Birincil sitenin ana düğümü öldürüldükten sonra aşağıdaki komutu ve çıkış kuyruğuna bakın. Yeni birincil ana sayı olarak eski ikincil ana düğüm, **hso-hana-vm-s2-0** **teşvik** gibi geçiş eylemlerinlistesini görebilirsiniz. Her şey yolundaysa ve tüm etkinlikler tamamlandıysa, bu **Geçiş Özeti** listesinin boş olması gerekir.

<pre><code>
 crm_simulate -Ls

...........

Transition Summary:
 * Fence hso-hana-vm-s1-0
 * Stop    rsc_SAPHanaTop_HSO_HDB00:1   (hso-hana-vm-s1-0)
 * Demote  rsc_SAPHanaCon_HSO_HDB00:1   (Master -> Stopped hso-hana-vm-s1-0)
 * Promote rsc_SAPHanaCon_HSO_HDB00:5   (Slave -> Master hso-hana-vm-s2-0)
 * Move    rsc_ip_HSO_HDB00     (Started hso-hana-vm-s1-0 -> hso-hana-vm-s2-0)
 * Move    rsc_nc_HSO_HDB00     (Started hso-hana-vm-s1-0 -> hso-hana-vm-s2-0)
</code></pre>



## <a name="planned-maintenance"></a>Planlı bakım 

Planlı bakım söz konusu olduğunda farklı kullanım örnekleri vardır. Bir soru işletim sistemi düzeyinde ve disk yapılandırması veya HANA yükseltme değişiklikleri gibi sadece altyapı bakım olup olmadığıdır.
SUSE'den Sıfır Kapalı Kalma [Süresine Doğru][sles-zero-downtime-paper] veya SAP [HANA SR Performans Optimize Senaryo][sles-12-for-sap]gibi belgelerde ek bilgiler bulabilirsiniz. Bu belgeler, birincil bir birincil nasıl el ile geçirin gösteren örnekler içerir.

Altyapı bakım kullanım örneğini doğrulamak için yoğun dahili test yapıldı. Birincil geçişile ilgili sorunları önlemek için, bir kümeyi bakım moduna almadan önce her zaman birincil bir geçiş yapmaya karar verdik. Bu şekilde, kümenin eski durumu unutturması gerekmez: hangi taraf birincil, hangisi ikincil idi.

Bu konuda iki farklı durum vardır:

- **Mevcut ikincil planlı bakım.** Bu durumda, kümeyi bakım moduna sokabilir ve kümeyi etkilemeden ikincil deki işi yapabilirsiniz.

- **Geçerli birincil planlı bakım**. Kullanıcıların bakım sırasında çalışmaya devam edebilmeleri için, bir hata yapmaya zorlamanız gerekir. Bu yaklaşımla, sadece SAP HANA HSR düzeyinde değil, Pacemaker tarafından küme başarısızlığı tetiklemelisiniz. Kalp Pili kurulumu SAP HANA'nın devralınmasını otomatik olarak tetikler. Ayrıca, kümeyi bakım moduna sokmadan önce başarısızlığı gerçekleştirmeniz gerekir.

Mevcut ikincil sitede bakım prosedürü aşağıdaki gibidir:

1. Kümeyi bakım moduna geçirin.
2. İkincil sitede iş gerçekleştirin. 
3. Küme bakım modunu sonla.

Geçerli birincil sitede bakım prosedürü daha karmaşıktır:

1. Bir Pacemaker kaynak geçişi yoluyla bir başarısızlığı veya SAP HANA devralmaişlemini manuel olarak tetikler. Takip eden ayrıntılara bakın.
2. SAP HANA eski birincil sitede küme kurulumu tarafından kapatılır.
3. Kümeyi bakım moduna geçirin.
4. Bakım çalışmaları yapıldıktan sonra, eski birincil yeni ikincil site olarak kaydedin.
5. Küme yapılandırmasını temizleyin. Takip eden ayrıntılara bakın.
6. Küme bakım modunu sonla.


Bir kaynağın geçirilmesi küme yapılandırmasına bir giriş ekler. Bir örnek bir failover zorluyor. Bakım modunu sona erdirmeden önce bu girişleri temizlemeniz gerekir. Aşağıdaki örneğe bakın.

İlk olarak, **msl** kaynağını geçerli ikincil ana düğüme geçirerek kümebaşarısızlığı zorlar. Bu komut, bir **hareket kısıtlaması** oluşturulduğuna dair bir uyarı verir:

<pre><code>
crm resource migrate msl_SAPHanaCon_HSO_HDB00 force

INFO: Move constraint created for msl_SAPHanaCon_HSO_HDB00
</code></pre>


**SAPHanaSR-showAttr**komutu ile arıza işlemini kontrol edin. Küme durumunu izlemek için, özel bir kabuk penceresi açın ve **komutu saatle**başlatın:

<pre><code>
watch SAPHanaSR-showAttr
</code></pre>

Çıktı, el ile başarısız olduğunu göstermelidir. Eski ikincil ana düğüm **terfi**var , Bu örnekte, **hso-hana-vm-s2-0**. Eski birincil site durduruldu, eski birincil ana düğüm **hso-hana-vm-s1-0**için **lss** değeri **1** : 

<pre><code>
Global cib-time                 prim  sec srHook sync_state
------------------------------------------------------------
global Wed Sep 12 07:40:02 2018 HSOS2 -   SFAIL  SFAIL


Sites lpt        lss mns              srr
------------------------------------------
HSOS1 10         1   hso-hana-vm-s1-0 P
HSOS2 1536738002 4   hso-hana-vm-s2-0 P


Hosts            clone_state node_state roles                        score  site
----------------------------------------------------------------------------------
hso-hana-dm                  online
hso-hana-vm-s1-0 UNDEFINED   online     master1::worker:             150    HSOS1
hso-hana-vm-s1-1 DEMOTED     online     slave::worker:               -10000 HSOS1
hso-hana-vm-s1-2 DEMOTED     online     slave::worker:               -10000 HSOS1
hso-hana-vm-s2-0 PROMOTED    online     master1:master:worker:master 150    HSOS2
hso-hana-vm-s2-1 DEMOTED     online     slave:slave:worker:slave     -10000 HSOS2
hso-hana-vm-s2-2 DEMOTED     online     slave:slave:worker:slave     -10000 HSOS2
</code></pre>

Küme failover ve SAP HANA devralma sonra, [Pacemaker](#pacemaker)açıklandığı gibi bakım moduna küme koymak.

**SAPHanaSR-showAttr** ve **crm durumu** komutları kaynak geçişi tarafından oluşturulan kısıtlamalar hakkında hiçbir şey göstermez. Bu kısıtlamaları görünür kılmak için bir seçenek aşağıdaki komutile tam küme kaynak yapılandırmasını göstermektir:

<pre><code>
crm configure show
</code></pre>

Küme yapılandırması içinde, eski el ile kaynak geçişinin neden olduğu yeni bir konum kısıtlaması bulursunuz. Bu örnek giriş **konum cli**ile başlar- :

<pre><code>
location cli-ban-msl_SAPHanaCon_HSO_HDB00-on-hso-hana-vm-s1-0 msl_SAPHanaCon_HSO_HDB00 role=Started -inf: hso-hana-vm-s1-0
</code></pre>

Ne yazık ki, bu tür kısıtlamalar genel küme davranışını etkileyebilir. Bu yüzden tüm sistemi geri getirmeden önce bunları tekrar kaldırmak zorunludur. **Geçişsiz** komutuyla, daha önce oluşturulmuş konum kısıtlamalarını temizlemek mümkündür. Adlandırma biraz kafa karıştırıcı olabilir. Kaynağı, geçirildiği orijinal VM'ye geri getirmeye çalışmaz. Yalnızca konum kısıtlamalarını kaldırır ve komutu çalıştırdığınızda ilgili bilgileri de döndürür:


<pre><code>
crm resource unmigrate msl_SAPHanaCon_HSO_HDB00

INFO: Removed migration constraints for msl_SAPHanaCon_HSO_HDB00
</code></pre>

Bakım çalışmasının sonunda, [Pacemaker'da](#pacemaker)gösterildiği gibi küme bakım modunu durdurursunuz.



## <a name="hb_report-to-collect-log-files"></a>Günlük dosyalarını toplamak için hb_report

Pacemaker küme sorunlarını çözümlemek için, yardımcı olur ve ayrıca **hb_report** yardımcı programı çalıştırmak için SUSE desteği tarafından istenir. Ne olduğunu analiz etmek için gereken tüm önemli günlük dosyalarını toplar. Bu örnek arama, belirli bir olayın meydana geldiği bir başlangıç ve bitiş saatini kullanır. Ayrıca bkz: [Önemli notlar:](#important-notes)

<pre><code>
hb_report -f "2018/09/13 07:36" -t "2018/09/13 08:00" /tmp/hb_report_log
</code></pre>

Komut, sıkıştırılmış günlük dosyalarını nereye koyduğunu söyler:

<pre><code>
The report is saved in /tmp/hb_report_log.tar.bz2
Report timespan: 09/13/18 07:36:00 - 09/13/18 08:00:00
</code></pre>

Daha sonra standart **katran** komutu ile tek tek dosyaları ayıklayabilirsiniz:

<pre><code>
tar -xvf hb_report_log.tar.bz2
</code></pre>

Çıkarılan dosyalara baktığınızda, tüm günlük dosyalarını bulursunuz. Çoğu kümedeki her düğüm için ayrı dizinlere konulmuştur:

<pre><code>
-rw-r--r-- 1 root root  13655 Sep 13 09:01 analysis.txt
-rw-r--r-- 1 root root  14422 Sep 13 09:01 description.txt
-rw-r--r-- 1 root root      0 Sep 13 09:01 events.txt
-rw-r--r-- 1 root root 275560 Sep 13 09:00 ha-log.txt
-rw-r--r-- 1 root root     26 Sep 13 09:00 ha-log.txt.info
drwxr-xr-x 4 root root   4096 Sep 13 09:01 hso-hana-dm
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s1-0
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s1-1
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s1-2
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s2-0
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s2-1
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s2-2
-rw-r--r-- 1 root root 264726 Sep 13 09:00 journal.log
</code></pre>


Belirtilen zaman aralığında, geçerli ana düğüm **hso-hana-vm-s1-0** öldürüldü. Bu olayla ilgili girişleri **journal.log'da**bulabilirsiniz:

<pre><code>
2018-09-13T07:38:01+0000 hso-hana-vm-s2-1 su[93494]: (to hsoadm) root on none
2018-09-13T07:38:01+0000 hso-hana-vm-s2-1 su[93494]: pam_unix(su-l:session): session opened for user hsoadm by (uid=0)
2018-09-13T07:38:01+0000 hso-hana-vm-s2-1 systemd[1]: Started Session c44290 of user hsoadm.
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 corosync[28302]:   [TOTEM ] A new membership (10.0.0.13:120996) was formed. Members left: 1
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 corosync[28302]:   [TOTEM ] Failed to receive the leave message. failed: 1
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 attrd[28313]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 attrd[28313]:   notice: Removing all hso-hana-vm-s1-0 attributes for peer loss
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 attrd[28313]:   notice: Purged 1 peer with id=1 and/or uname=hso-hana-vm-s1-0 from the membership cache
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 stonith-ng[28311]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 stonith-ng[28311]:   notice: Purged 1 peer with id=1 and/or uname=hso-hana-vm-s1-0 from the membership cache
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 cib[28310]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 corosync[28302]:   [QUORUM] Members[6]: 7 2 3 4 5 6
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 corosync[28302]:   [MAIN  ] Completed service synchronization, ready to provide service.
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 crmd[28315]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 pacemakerd[28308]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 cib[28310]:   notice: Purged 1 peer with id=1 and/or uname=hso-hana-vm-s1-0 from the membership cache
2018-09-13T07:38:03+0000 hso-hana-vm-s2-1 su[93494]: pam_unix(su-l:session): session closed for user hsoadm
</code></pre>

Başka bir örnek, yeni birincil ana oldu ikincil ana, Pacemaker günlük dosyasıdır. Bu alıntı, öldürülen birincil ana düğümün durumunun **çevrimdışı**olarak ayarlandığını gösterir:

<pre><code>
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 3 still member of group stonith-ng (peer=hso-hana-vm-s1-2, counter=5.1)
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 4 still member of group stonith-ng (peer=hso-hana-vm-s2-0, counter=5.2)
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 5 still member of group stonith-ng (peer=hso-hana-vm-s2-1, counter=5.3)
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 6 still member of group stonith-ng (peer=hso-hana-vm-s2-2, counter=5.4)
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 7 still member of group stonith-ng (peer=hso-hana-dm, counter=5.5)
Sep 13 07:38:02 [4184] hso-hana-vm-s2-0       crmd:     info: pcmk_cpg_membership:      Node 1 left group crmd (peer=hso-hana-vm-s1-0, counter=5.0)
Sep 13 07:38:02 [4184] hso-hana-vm-s2-0       crmd:     info: crm_update_peer_proc:     pcmk_cpg_membership: Node hso-hana-vm-s1-0[1] - corosync-cpg is now offline
Sep 13 07:38:02 [4184] hso-hana-vm-s2-0       crmd:     info: peer_update_callback:     Client hso-hana-vm-s1-0/peer now has status [offline] (DC=hso-hana-dm, changed=4000000)
Sep 13 07:38:02 [4184] hso-hana-vm-s2-0       crmd:     info: pcmk_cpg_membership:      Node 2 still member of group crmd (peer=hso-hana-vm-s1-1, counter=5.0)
</code></pre>





## <a name="sap-hana-globalini"></a>SAP HANA global.ini


Aşağıdaki alıntılar küme sitesi 2'deki SAP HANA **global.ini** dosyasından alınmıştır. Bu örnek, SAP HANA internode iletişimi ve HSR için farklı ağları kullanmak için ana bilgisayar adı çözümleme girişlerini gösterir:

<pre><code>
[communication]
tcp_keepalive_interval = 20
internal_network = 10.0.2/24
listeninterface = .internal
</code></pre>


<pre><code>
[internal_hostname_resolution]
10.0.2.40 = hso-hana-vm-s2-0
10.0.2.42 = hso-hana-vm-s2-2
10.0.2.41 = hso-hana-vm-s2-1
</code></pre>

<pre><code>
[ha_dr_provider_SAPHanaSR]
provider = SAPHanaSR
path = /hana/shared/myHooks
execution_order = 1
</code></pre>

<pre><code>
[system_replication_communication]
listeninterface = .internal

[system_replication_hostname_resolution]
10.0.1.30 = hso-hana-vm-s1-0
10.0.1.31 = hso-hana-vm-s1-1
10.0.1.32 = hso-hana-vm-s1-2
10.0.1.40 = hso-hana-vm-s2-0
10.0.1.41 = hso-hana-vm-s2-1
10.0.1.42 = hso-hana-vm-s2-2
</code></pre>



## <a name="hawk"></a>Hawk

Küme çözümü, menüleri ve grafikleri kabuk düzeyindeki tüm komutlara sahip olmak için tercih eden kullanıcılar için bir GUI sunan bir tarayıcı arabirimi sağlar.
Tarayıcı arabirimini kullanmak ** \<için, düğümü\> ** aşağıdaki URL'deki gerçek bir SAP HANA düğümüyle değiştirin. Sonra kümenin kimlik bilgilerini girin (kullanıcı **kümesi):**

<pre><code>
https://&ltnode&gt:7630
</code></pre>

Bu ekran görüntüsü küme panosunu gösterir:


![Şahin kümesi panosu](media/hana-vm-scale-out-HA-troubleshooting/hawk-1.png)


Bu örnek, [Planlı bakımda](#planned-maintenance)açıklandığı gibi küme kaynak geçişinin neden olduğu konum kısıtlamalarını gösterir:


![Şahin listesi kısıtlamaları](media/hana-vm-scale-out-HA-troubleshooting/hawk-2.png)


Hawk under **History'deki** **hb_report** çıktısını da aşağıdaki gibi yükleyebilirsiniz. Günlük dosyalarını toplamak için hb_report bakın: 

![Hawk yükleme hb_report çıktı](media/hana-vm-scale-out-HA-troubleshooting/hawk-3.png)

Geçmiş **Gezgini**ile, daha sonra **hb_report** çıktıdahil tüm küme geçişleri üzerinden gidebilirsiniz:

![hb_report çıktıda şahin geçişleri](media/hana-vm-scale-out-HA-troubleshooting/hawk-4.png)

Bu son ekran görüntüsü, tek bir geçişin **Ayrıntılar** bölümünü gösterir. Küme birincil ana düğüm çökmesine tepki, düğüm **hso-hana-vm-s1-0**. Şimdi yeni ana olarak ikincil düğüm teşvik's, **hso-hana-vm-s2-0:**

![Şahin tek geçiş](media/hana-vm-scale-out-HA-troubleshooting/hawk-5.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu sorun giderme kılavuzu, sap HANA için ölçeklendirilen bir yapılandırmada yüksek kullanılabilirliği açıklar. Veritabanına ek olarak, SAP ortamındaki bir diğer önemli bileşen de SAP NetWeaver yığınıdır. [SUSE Enterprise Linux Server kullanan Azure sanal makinelerinde SAP NetWeaver için yüksek kullanılabilirlik][sap-nw-ha-guide-sles]hakkında bilgi edinin.

