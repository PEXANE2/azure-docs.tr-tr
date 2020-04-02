---
title: RHEL'de Azure VM'lerde SAP HANA'nın yüksek kullanılabilirliği | Microsoft Dokümanlar
description: Azure sanal makinelerinde (VM) SAP HANA'nın yüksek kullanılabilirliğini belirleyin.
services: virtual-machines-linux
documentationcenter: ''
author: rdeltcheva
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/31/2020
ms.author: radeltch
ms.openlocfilehash: f1ae2c3c949e8bdbf30c8bef496177d56cd2dcbd
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521402"
---
# <a name="high-availability-of-sap-hana-on-azure-vms-on-red-hat-enterprise-linux"></a>Red Hat Enterprise Linux'ta Azure VM'lerde SAP HANA'nın yüksek kullanılabilirliği

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
[2388694]:https://launchpad.support.sap.com/#/notes/2388694
[2292690]:https://launchpad.support.sap.com/#/notes/2292690
[2455582]:https://launchpad.support.sap.com/#/notes/2455582
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2009879]:https://launchpad.support.sap.com/#/notes/2009879

[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json

Şirket içi geliştirme için HANA Sistem Çoğaltma'yı kullanabilir veya SAP HANA için yüksek kullanılabilirlik oluşturmak için paylaşılan depolamayı kullanabilirsiniz.
Azure sanal makinelerde (VM) Azure'daki HANA System Replication şu anda desteklenen tek yüksek kullanılabilirlik işlevidir.
SAP HANA Çoğaltma bir birincil düğüm ve en az bir ikincil düğüm oluşur. Birincil düğümdeki verilerdeki değişiklikler, ikincil düğüme eşzamanlı olarak veya eşzamanlı olarak çoğaltılır.

Bu makalede, sanal makinelerin nasıl dağıtılanın ve yapılandırılabilmek, küme çerçevesini yüklemek ve SAP HANA Sistem Çoğaltma'yı yüklemek ve yapılandırmak açıklanmaktadır.
Örnek yapılandırmalarda yükleme komutları, örnek numarası **03**ve HANA System ID **HN1** kullanılır.

Önce aşağıdaki SAP Notlarını ve bildirilerini okuyun:

* SAP Note [1928533], olan:
  * SAP yazılımının dağıtımı için desteklenen Azure VM boyutlarının listesi.
  * Azure VM boyutları için önemli kapasite bilgileri.
  * Desteklenen SAP yazılımı ve işletim sistemi (OS) ve veritabanı kombinasyonları.
  * Microsoft Azure'da Windows ve Linux için gerekli SAP çekirdeği sürümü.
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
* [Kalp pili kümesinde SAP HANA sistem replikasyonu](https://access.redhat.com/articles/3004101)
* Genel RHEL belgeleri
  * [Yüksek Kullanılabilirlik Eklentisi Genel Bakış](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index)
  * [Yüksek Kullanılabilirlik Eklenti Yönetimi](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index)
  * [Yüksek Kullanılabilirlik Eklenti Başvurusu](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index)
* Azure'a özgü RHEL belgeleri:
  * [RHEL Yüksek Kullanılabilirlik Kümeleri için Destek İlkeleri - Küme Üyesi Olarak Microsoft Azure Sanal Makineleri](https://access.redhat.com/articles/3131341)
  * [Microsoft Azure'da Red Hat Enterprise Linux 7.4 (ve sonrası) Yüksek Kullanılabilirlik Kümesi'ni yükleme ve yapılandırma](https://access.redhat.com/articles/3252491)
  * [SAP HANA'yı Microsoft Azure'da Kullanılmak Üzere Red Hat Enterprise Linux'a yükleyin](https://access.redhat.com/solutions/3193782)

## <a name="overview"></a>Genel Bakış

Yüksek kullanılabilirlik elde etmek için, SAP HANA iki sanal makineye yüklenir. Veriler HANA Sistem Çoğaltma kullanılarak çoğaltılır.

![SAP HANA yüksek kullanılabilirlik genel bakış](./media/sap-hana-high-availability-rhel/ha-hana.png)

SAP HANA Sistem Çoğaltma kurulumu, özel bir sanal ana bilgisayar adı ve sanal IP adresleri kullanır. Azure'da, sanal bir IP adresi kullanmak için bir yük dengeleyicisi gereklidir. Aşağıdaki liste yük dengeleyicisinin yapılandırmasını gösterir:

* Ön uç yapılandırması: HN1-db için IP adresi 10.0.0.13
* Arka uç yapılandırması: HANA Sistem Çoğaltma'nın bir parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı
* Sonda Bağlantı Noktası: Bağlantı Noktası 62503
* Yük dengeleme kuralları: 30313 TCP, 30315 TCP, 30317 TCP, 30340 TCP, 30341 TCP, 30342 TCP

## <a name="deploy-for-linux"></a>Linux için dağıt

Azure Marketi, SAP HANA için Red Hat Enterprise Linux 7.4 için yeni sanal makineleri dağıtmak için kullanabileceğiniz bir resim içerir.

### <a name="deploy-with-a-template"></a>Şablonla dağıtma

Gerekli tüm kaynakları dağıtmak için GitHub'da olan hızlı başlangıç şablonlarından birini kullanabilirsiniz. Şablon sanal makineleri, yük dengeleyicisini, kullanılabilirlik kümesini ve benzeri uygulamaları dağır.
Şablonu dağıtmak için aşağıdaki adımları izleyin:

1. Azure portalında [veritabanı şablonu][template-multisid-db] açın.
1. Aşağıdaki parametreleri girin:
    * **Sap Sistem Kimliği**: Yüklemek istediğiniz SAP sisteminin SAP sistem kimliğini girin. Kimlik, dağıtılan kaynaklar için önek olarak kullanılır.
    * **Os Türü**: Linux dağıtımlarından birini seçin. Bu örnek için **RHEL 7'yi**seçin.
    * **Db Türü**: **HANA'yı**seçin.
    * **Sap Sistem Boyutu**: Yeni sistemin sağlayacağı SAPS sayısını girin. Sistemin kaç SAPS gerektirdiğinden emin değilseniz, SAP Teknoloji Ortağınıza veya Sistem Entegratörünüze danışın.
    * **Sistem Kullanılabilirliği**: **SELECT HA**.
    * **Admin Kullanıcı Adı, Yönetici Şifresi veya SSH tuşu**: Makinede oturum açabilen yeni bir kullanıcı oluşturulur.
    * **Subnet ID**: VM'yi, vm'nin atanması gereken tanımlanmış bir alt ağına yerleştirmek istiyorsanız, o alt ağın kimliğini adlandırın. Kimlik genellikle **/abonelik/\<abonelik kimliği>/kaynakGruplar/\<kaynak grup adı>/providers/Microsoft.Network/virtualNetworks/\<sanal\<ağ adı>/subnet/subnet adı>** gibi görünür. Yeni bir sanal ağ oluşturmak istiyorsanız boş bırakın

### <a name="manual-deployment"></a>El ile dağıtım

1. Bir kaynak grubu oluşturun.
1. Sanal ağ oluşturun.
1. Kullanılabilirlik kümesi oluşturun.  
   Maksimum güncelleştirme etki alanını ayarlayın.
1. Yük dengeleyici (dahili) oluşturun. Standart [yük dengeleyicisi](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview)öneririz.
   * Adım 2'de oluşturulan sanal ağı seçin.
1. Sanal makine 1 oluşturun.  
   SAP HANA için en az Red Hat Enterprise Linux 7.4 kullanın. Bu örnek, SAP HANA görüntüsü <https://portal.azure.com/#create/RedHat.RedHatEnterpriseLinux75forSAP-ARM> için Red Hat Enterprise Linux 7.4'ü kullanır Adım 3'te oluşturulan kullanılabilirlik kümesini seçin.
1. Sanal makine 2 oluşturun.  
   SAP HANA için en az Red Hat Enterprise Linux 7.4 kullanın. Bu örnek, SAP HANA görüntüsü <https://portal.azure.com/#create/RedHat.RedHatEnterpriseLinux75forSAP-ARM> için Red Hat Enterprise Linux 7.4'ü kullanır Adım 3'te oluşturulan kullanılabilirlik kümesini seçin.
1. Veri diskleri ekleyin.
1. Standart yük dengeleyicisi kullanıyorsanız, aşağıdaki yapılandırma adımlarını izleyin:
   1. İlk olarak, bir ön uç IP havuzu oluşturun:

      1. Yük bakiyesini açın, **frontend IP havuzunu**seçin ve **Ekle'yi**seçin.
      1. Yeni ön uç IP havuzunun adını girin (örneğin, **hana-frontend).**
      1. **Atamayı** **Statik** olarak ayarlayın ve IP adresini girin (örneğin, **10.0.0.13).**
      1. **Tamam'ı**seçin.
      1. Yeni ön uç IP havuzu oluşturulduktan sonra, havuz IP adresine dikkat edin.

   1. Ardından, bir arka uç havuzu oluşturun:

      1. Yük bakiyesini açın, **arka uç havuzlarını**seçin ve **Ekle'yi**seçin.
      1. Yeni arka uç havuzunun adını girin (örneğin, **hana-backend).**
      1. **Sanal makine ekle'yi**seçin.
      1. ** Sanal makine**'yi seçin.
      1. SAP HANA kümesinin sanal makinelerini ve IP adreslerini seçin.
      1. **Ekle'yi**seçin.

   1. Ardından, bir sistem durumu sondası oluşturun:

      1. Yük bakiyesini açın, **sistem durumu sondalarını**seçin ve **Ekle'yi**seçin.
      1. Yeni sağlık sondasının adını girin (örneğin, **hana-hp).**
      1. Protokol olarak **TCP'yi** ve 625**03**bağlantı noktasını seçin. **Aralık** değerini 5'e, **Sağlıksız eşik** değerini 2 olarak ayarlayın.
      1. **Tamam'ı**seçin.

   1. Ardından, yük dengeleme kurallarını oluşturun:
   
      1. Yük dengeleyicisini açın, **yük dengeleme kurallarını**seçin ve **Ekle'yi**seçin.
      1. Yeni yük dengeleyici kuralının adını girin (örneğin, **hana-lb).**
      1. Ön uç IP adresini, arka uç havuzunu ve daha önce oluşturduğunuz sağlık sondasını seçin (örneğin, **hana-frontend,** **hana-backend** ve **hana-hp).**
      1. **HA Bağlantı Noktalarını**seçin.
      1. **Boşta kalma süresini** 30 dakikaya çıkarın.
      1. **Kayan IP'yi etkinleştirdiğinden**emin olun.
      1. **Tamam'ı**seçin.

   > [!Note]
   > Ortak IP adresi olmayan VM'ler dahili (genel IP adresi yok) Standart Azure yük bakiyesi arka uç havuzuna yerleştirildiğinde, ortak bitiş noktalarına yönlendirmeye izin verecek ek yapılandırma yapılmadığı sürece giden internet bağlantısı olmaz. Giden bağlantının nasıl elde edilene ilişkin ayrıntılar [için, SAP yüksek kullanılabilirlik senaryolarında Azure Standart Yük Dengeleyicisini kullanan Sanal Makineler için Genel uç nokta bağlantısına](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-standard-load-balancer-outbound-connections)bakın.  

1. Alternatif olarak, senaryonuz temel yük dengeleyicisi kullanmayı gerektiriyorsa, aşağıdaki yapılandırma adımlarını izleyin:
   1. Yük dengeleyicisini yapılandırın. İlk olarak, bir ön uç IP havuzu oluşturun:

      1. Yük bakiyesini açın, **frontend IP havuzunu**seçin ve **Ekle'yi**seçin.
      1. Yeni ön uç IP havuzunun adını girin (örneğin, **hana-frontend).**
      1. **Atamayı** **Statik** olarak ayarlayın ve IP adresini girin (örneğin, **10.0.0.13).**
      1. **Tamam'ı**seçin.
      1. Yeni ön uç IP havuzu oluşturulduktan sonra, havuz IP adresine dikkat edin.

   1. Ardından, bir arka uç havuzu oluşturun:

      1. Yük bakiyesini açın, **arka uç havuzlarını**seçin ve **Ekle'yi**seçin.
      1. Yeni arka uç havuzunun adını girin (örneğin, **hana-backend).**
      1. **Sanal makine ekle'yi**seçin.
      1. Adım 3'te oluşturulan kullanılabilirlik kümesini seçin.
      1. SAP HANA kümesinin sanal makinelerini seçin.
      1. **Tamam'ı**seçin.

   1. Ardından, bir sistem durumu sondası oluşturun:

      1. Yük bakiyesini açın, **sistem durumu sondalarını**seçin ve **Ekle'yi**seçin.
      1. Yeni sağlık sondasının adını girin (örneğin, **hana-hp).**
      1. Protokol olarak **TCP'yi** ve 625**03**bağlantı noktasını seçin. **Aralık** değerini 5'e, **Sağlıksız eşik** değerini 2 olarak ayarlayın.
      1. **Tamam'ı**seçin.

   1. SAP HANA 1.0 için yük dengeleme kurallarını oluşturun:

      1. Yük dengeleyicisini açın, **yük dengeleme kurallarını**seçin ve **Ekle'yi**seçin.
      1. Yeni yük dengeleyici kuralının adını girin (örneğin, hana-lb-3**03**15).
      1. Ön uç IP adresini, arka uç havuzunu ve daha önce oluşturduğunuz sistem durumu sondasını (örneğin, **hana-frontend)** seçin.
      1. **Protokol'ün** **TCP**olarak ayarlı olmasını ve 3**03**15 bağlantı noktasını girin.
      1. **Boşta kalma süresini** 30 dakikaya çıkarın.
      1. **Kayan IP'yi etkinleştirdiğinden**emin olun.
      1. **Tamam'ı**seçin.
      1. Bağlantı noktası 3**03**17 için bu adımları tekrarlayın.

   1. SAP HANA 2.0 için sistem veritabanı için yük dengeleme kurallarını oluşturun:

      1. Yük dengeleyicisini açın, **yük dengeleme kurallarını**seçin ve **Ekle'yi**seçin.
      1. Yeni yük dengeleyici kuralının adını girin (örneğin, hana-lb-3**03**13).
      1. Ön uç IP adresini, arka uç havuzunu ve daha önce oluşturduğunuz sistem durumu sondasını (örneğin, **hana-frontend)** seçin.
      1. **Protokol'ün** **TCP**olarak ayarlı olmasını ve 3**03**13 bağlantı noktasını girin.
      1. **Boşta kalma süresini** 30 dakikaya çıkarın.
      1. **Kayan IP'yi etkinleştirdiğinden**emin olun.
      1. **Tamam'ı**seçin.
      1. Bağlantı noktası 3**03**14 için bu adımları tekrarlayın.

   1. SAP HANA 2.0 için öncelikle kiracı veritabanı için yük dengeleme kurallarını oluşturun:

      1. Yük dengeleyicisini açın, **yük dengeleme kurallarını**seçin ve **Ekle'yi**seçin.
      1. Yeni yük dengeleyici kuralının adını girin (örneğin, hana-lb-3**03**40).
      1. Daha önce oluşturduğunuz ön uç IP adresini, arka uç havuzunu ve sistem durumu sondasını seçin (örneğin, **hana-frontend).**
      1. **Protokol'ün** **TCP**olarak ayarlı olmasını ve 3**03**40 bağlantı noktasını girin.
      1. **Boşta kalma süresini** 30 dakikaya çıkarın.
      1. **Kayan IP'yi etkinleştirdiğinden**emin olun.
      1. **Tamam'ı**seçin.
      1. 3**03**41 ve 3**03**42 bağlantı noktaları için bu adımları tekrarlayın.

SAP HANA için gerekli bağlantı noktaları hakkında daha fazla bilgi için SAP [HANA Kiracı Veritabanları](https://help.sap.com/viewer/78209c1d3a9b41cd8624338e42a12bf6) kılavuzunda veya [SAP Note 2388694'teki][2388694] [Kiracı Veritabanlarına Bağlantılar](https://help.sap.com/viewer/78209c1d3a9b41cd8624338e42a12bf6/latest/en-US/7a9343c9f2a2436faa3cfdb5ca00c052.html) bölümünü okuyun.

> [!IMPORTANT]
> Azure Yük Bakiyesi'nin arkasına yerleştirilen Azure VM'lerinde TCP zaman damgalarını etkinleştirme. TCP zaman damgalarını etkinleştirmek sistem durumu sondalarının başarısız lığa neden olur. Parametre **net.ipv4.tcp_timestamps** **0**'a ayarlayın. Ayrıntılar için [Bkz. Yük Dengeleyici sağlık probları.](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)
> Ayrıca bakınız SAP notu [2382421](https://launchpad.support.sap.com/#/notes/2382421). 

## <a name="install-sap-hana"></a>SAP HANA yükleme

Bu bölümdeki adımlar aşağıdaki önekleri kullanır:

* **[A]**: Adım tüm düğümler için geçerlidir.
* **[1]**: Adım yalnızca düğüm 1 için geçerlidir.
* **[2]**: Adım yalnızca Pacemaker kümesinin 2 numaralı düğümü için geçerlidir.

1. **[A]** Disk düzenini ayarlama: **Mantıksal Birim Yöneticisi (LVM)**.

   Verileri depolayan ve dosyaları kaydeden birimler için LVM kullanmanızı öneririz. Aşağıdaki örnek, sanal makinelerin iki birim oluşturmak için kullanılan dört veri diski eklenmiş olduğunu varsayar.

   Kullanılabilir tüm diskleri listele:

   <pre><code>ls /dev/disk/azure/scsi1/lun*
   </code></pre>

   Örnek çıktı:

   <pre><code>
   /dev/disk/azure/scsi1/lun0  /dev/disk/azure/scsi1/lun1  /dev/disk/azure/scsi1/lun2  /dev/disk/azure/scsi1/lun3
   </code></pre>

   Kullanmak istediğiniz tüm diskler için fiziksel birimler oluşturun:

   <pre><code>sudo pvcreate /dev/disk/azure/scsi1/lun0
   sudo pvcreate /dev/disk/azure/scsi1/lun1
   sudo pvcreate /dev/disk/azure/scsi1/lun2
   sudo pvcreate /dev/disk/azure/scsi1/lun3
   </code></pre>

   Veri dosyaları için bir birim grubu oluşturun. Günlük dosyaları için bir birim grubu ve SAP HANA'nın paylaşılan dizini için bir birim grubu kullanın:

   <pre><code>sudo vgcreate vg_hana_data_<b>HN1</b> /dev/disk/azure/scsi1/lun0 /dev/disk/azure/scsi1/lun1
   sudo vgcreate vg_hana_log_<b>HN1</b> /dev/disk/azure/scsi1/lun2
   sudo vgcreate vg_hana_shared_<b>HN1</b> /dev/disk/azure/scsi1/lun3
   </code></pre>

   Mantıksal birimleri oluşturun. `-i` Anahtar olmadan kullandığınızda `lvcreate` doğrusal bir birim oluşturulur. Daha iyi G/Ç performansı için çizgili bir hacim oluşturmanızı ve şerit boyutlarını [SAP HANA VM depolama yapılandırmalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)belgelenen değerlerle hizalamanızı öneririz. `-i` Bağımsız değişken, temel fiziksel birimlerin sayısı `-I` olmalıdır ve bağımsız değişken şerit boyutudur. Bu belgede, veri hacmi için iki fiziksel birim `-i` kullanılır, bu nedenle anahtar bağımsız değişkeni **2**olarak ayarlanır. Veri hacmi için şerit boyutu **256KiB'dir.** Günlük birimi için bir fiziksel birim `-i` kullanılır, bu nedenle günlük hacmi komutları için açık bir şekilde hiçbir anahtar veya `-I` anahtar kullanılır.  

   > [!IMPORTANT]
   > `-i` Her veri, günlük veya paylaşılan birimler için birden fazla fiziksel birim kullandığınızda anahtarı kullanın ve temel fiziksel birim sayısına ayarlayın. Çizgili `-I` bir birim oluştururken şerit boyutunu belirtmek için anahtarı kullanın.  
   > Şerit boyutları ve disk sayısı da dahil olmak üzere önerilen depolama yapılandırmaları için [SAP HANA VM depolama yapılandırmalarına](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) bakın.  

   <pre><code>sudo lvcreate <b>-i 2</b> <b>-I 256</b> -l 100%FREE -n hana_data vg_hana_data_<b>HN1</b>
   sudo lvcreate -l 100%FREE -n hana_log vg_hana_log_<b>HN1</b>
   sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared_<b>HN1</b>
   sudo mkfs.xfs /dev/vg_hana_data_<b>HN1</b>/hana_data
   sudo mkfs.xfs /dev/vg_hana_log_<b>HN1</b>/hana_log
   sudo mkfs.xfs /dev/vg_hana_shared_<b>HN1</b>/hana_shared
   </code></pre>

   Montaj dizinlerini oluşturun ve tüm mantıksal birimlerin UUID'sini kopyalayın:

   <pre><code>sudo mkdir -p /hana/data/<b>HN1</b>
   sudo mkdir -p /hana/log/<b>HN1</b>
   sudo mkdir -p /hana/shared/<b>HN1</b>
   # Write down the ID of /dev/vg_hana_data_<b>HN1</b>/hana_data, /dev/vg_hana_log_<b>HN1</b>/hana_log, and /dev/vg_hana_shared_<b>HN1</b>/hana_shared
   sudo blkid
   </code></pre>

   Üç `fstab` mantıksal birim için girişler oluşturun:

   <pre><code>sudo vi /etc/fstab
   </code></pre>

   `/etc/fstab` Dosyaya aşağıdaki satırı ekleyin:

   <pre><code>/dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_data_<b>HN1</b>-hana_data&gt;</b> /hana/data/<b>HN1</b> xfs  defaults,nofail  0  2
   /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_log_<b>HN1</b>-hana_log&gt;</b> /hana/log/<b>HN1</b> xfs  defaults,nofail  0  2
   /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_shared_<b>HN1</b>-hana_shared&gt;</b> /hana/shared/<b>HN1</b> xfs  defaults,nofail  0  2
   </code></pre>

   Yeni ciltleri monte edin:

   <pre><code>sudo mount -a
   </code></pre>

1. **[A]** Disk düzenini ayarlama: **Düz Diskler**.

   Demo sistemleri için HANA verilerinizi yerleyebilir ve dosyalarınızı tek bir diske kaydedebilirsiniz. /dev/disk/azure/scsi1/lun0 üzerinde bir bölüm oluşturun ve xfs ile biçimlendirin:

   <pre><code>sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun0'
   sudo mkfs.xfs /dev/disk/azure/scsi1/lun0-part1
   
   # Write down the ID of /dev/disk/azure/scsi1/lun0-part1
   sudo /sbin/blkid
   sudo vi /etc/fstab
   </code></pre>

   /etc/fstab dosyasına bu satırı ekleyin:

   <pre><code>/dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
   </code></pre>

   Hedef dizini oluşturun ve diski monte edin:

   <pre><code>sudo mkdir /hana
   sudo mount -a
   </code></pre>

1. **[A]** Tüm ana bilgisayarlar için ana bilgisayar ad çözümlemesi ayarlayın.

   Bir DNS sunucusu kullanabilir veya tüm düğümlerde /etc/hosts dosyasını değiştirebilirsiniz. Bu örnekte /etc/hosts dosyasının nasıl kullanılacağı gösterilmektedir.
   Aşağıdaki komutlarda IP adresini ve ana bilgisayar adını değiştirin:

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   /etc/hosts dosyasına aşağıdaki satırları ekleyin. IP adresini ve ana bilgisayar adını ortamınıza uyacak şekilde değiştirin:

   <pre><code><b>10.0.0.5 hn1-db-0</b>
   <b>10.0.0.6 hn1-db-1</b>
   </code></pre>

1. HANA yapılandırması için **[A]** RHEL

   SAP Note [2292690] ve [2455582 ve 2455582] ve 'de <https://access.redhat.com/solutions/2447641>açıklandığı şekilde RHEL'i yapılandırın.

1. **[A]** SAP HANA'yı yükleyin

   SAP HANA Sistem Çoğaltma yüklemek <https://access.redhat.com/articles/3004101>için, izleyin.

   * HANA DVD **hdblcm** programı çalıştırın. İstem de aşağıdaki değerleri girin:
   * Yüklemeyi seçin: **1**girin.
   * Kurulum için ek bileşenler seçin: **1**girin.
   * Yükleme Yolunu girin [/hana/shared]: Enter'u seçin.
   * Yerel Ana Bilgisayar Adı girin [..]: Enter'u seçin.
   * Sisteme ek ana bilgisayarlar eklemek istiyor musunuz? (y/n) [n]: Enter'u seçin.
   * SAP HANA Sistem Kimliği girin: ÖRNEĞIN HANA'nın SID'ini girin: **HN1**.
   * Örnek Numarası [00]: HANA Örnek numarasını girin. Azure şablonunu kullandıysanız veya bu makalenin el ile dağıtım bölümünü izlediyseniz **03** girin.
   * Veritabanı Modu ' nu seçin / Dizini girin [1]: Enter'u seçin.
   * Sistem Kullanımı / Enter Index 'i seçin [4]: Sistem kullanım değerini seçin.
   * Veri Hacimlerinin Konumunu Girin [/hana/data/HN1]: Enter'u seçin.
   * Günlük Hacimlerinin Konumunu Girin [/hana/log/HN1]: Enter'u seçin.
   * Maksimum bellek ayırmayı kısıtlamak mı? [n]: Enter'u seçin.
   * Ana Bilgisayar Için Sertifika Ana Bilgisayar Adı Girin '...' [...]: Enter'u seçin.
   * SAP Host Agent User (sapadm) Şifresini girin: Ana bilgisayar temsilcisi kullanıcı parolasını girin.
   * SAP Host Agent User (sapadm) Parolasını onaylayın: Onaylamak için ana bilgisayar kullanıcı parolasını yeniden girin.
   * Sistem Yöneticisi (hdbadm) Şifresini girin: Sistem yöneticisi parolasını girin.
   * Sistem Yöneticisi (hdbadm) Şifresini Onaylayın: Onaylamak için sistem yöneticisi parolasını tekrar girin.
   * Sistem Yöneticisi Giriş Dizini [/usr/sap/HN1/home]: Enter'u seçin.
   * Sistem Yöneticisi Giriş Kabuğu [/bin/sh]: Enter'u seçin.
   * Sistem Yöneticisi Kullanıcı Kimliğini Girin [1001]: Enter'u seçin.
   * Kullanıcı Grubunun Kimliğini girin (sapsys) [79]: Enter'u seçin.
   * Veritabanı Kullanıcı (SYSTEM) Şifresini girin: Veritabanı kullanıcı parolasını girin.
   * Veritabanı Kullanıcı (SYSTEM) Şifresini Onaylayın: Onaylamak için veritabanı kullanıcı parolasını yeniden girin.
   * Makine yeniden başlatıldıktan sonra sistemi yeniden başlatın? [n]: Enter'u seçin.
   * Devam etmek istiyor musunuz? (y/n): Özeti doğrulayın. Devam etmek için **y** girin.

1. **SAP** Ana Bilgisayar Aracısını yükseltin.

   [SAP Yazılım Merkezi'nden][sap-swcenter] en son SAP Ana Bilgisayar Aracısı arşivini indirin ve aracıyı yükseltmek için aşağıdaki komutu çalıştırın. İndirdiğiniz dosyayı işaret etmek için arşivyolunu değiştirin:

   <pre><code>sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive &lt;path to SAP Host Agent SAR&gt;
   </code></pre>

1. **[A]** Güvenlik duvarLarını yapılandırma

   Azure yük dengeleyici sondası bağlantı noktası için güvenlik duvarı kuralını oluşturun.

   <pre><code>sudo firewall-cmd --zone=public --add-port=625<b>03</b>/tcp
   sudo firewall-cmd --zone=public --add-port=625<b>03</b>/tcp --permanent
   </code></pre>

## <a name="configure-sap-hana-20-system-replication"></a>SAP HANA 2.0 Sistem Çoğaltma yapılandırma

Bu bölümdeki adımlar aşağıdaki önekleri kullanır:

* **[A]**: Adım tüm düğümler için geçerlidir.
* **[1]**: Adım yalnızca düğüm 1 için geçerlidir.
* **[2]**: Adım yalnızca Pacemaker kümesinin 2 numaralı düğümü için geçerlidir.

1. **[A]** Güvenlik duvarLarını yapılandırma

   HANA Sistem Çoğaltma ve istemci trafiğine izin vermek için güvenlik duvarı kuralları oluşturun. Gerekli bağlantı noktaları [Tüm SAP Ürünlerinin TCP/IP bağlantı noktalarında](https://help.sap.com/viewer/ports)listelenir. Aşağıdaki komutlar HANA 2.0 Sistem Çoğaltma ve veritabanı SYSTEMDB, HN1 ve NW1 istemci trafiğine izin vermek için sadece bir örnektir.

   <pre><code>sudo firewall-cmd --zone=public --add-port=40302/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=40302/tcp
   sudo firewall-cmd --zone=public --add-port=40301/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=40301/tcp
   sudo firewall-cmd --zone=public --add-port=40307/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=40307/tcp
   sudo firewall-cmd --zone=public --add-port=40303/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=40303/tcp
   sudo firewall-cmd --zone=public --add-port=40340/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=40340/tcp
   sudo firewall-cmd --zone=public --add-port=30340/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=30340/tcp
   sudo firewall-cmd --zone=public --add-port=30341/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=30341/tcp
   sudo firewall-cmd --zone=public --add-port=30342/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=30342/tcp
   </code></pre>

1. **[1]** Kiracı veritabanını oluşturun.

   SAP HANA 2.0 veya MDC kullanıyorsanız, SAP NetWeaver sisteminiz için bir kiracı veritabanı oluşturun. **NW1'i** SAP sisteminizin SID'si ile değiştirin.

   Hanasid\>adm <aşağıdaki komutu çalıştırın:

   <pre><code>hdbsql -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> -d SYSTEMDB 'CREATE DATABASE <b>NW1</b> SYSTEM USER PASSWORD "<b>passwd</b>"'
   </code></pre>

1. **[1]** Sistem Çoğaltma'yı ilk düğümde yapılandır:

   <hanasid\>adm olarak veritabanları yedekleme:

   <pre><code>hdbsql -d SYSTEMDB -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupSYS</b>')"
   hdbsql -d <b>HN1</b> -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupHN1</b>')"
   hdbsql -d <b>NW1</b> -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupNW1</b>')"
   </code></pre>

   Sistem PKI dosyalarını ikincil siteye kopyalayın:

   <pre><code>scp /usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/data/SSFS_<b>HN1</b>.DAT   <b>hn1-db-1</b>:/usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/data/
   scp /usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/key/SSFS_<b>HN1</b>.KEY  <b>hn1-db-1</b>:/usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/key/
   </code></pre>

   Birincil siteyi oluşturun:

   <pre><code>hdbnsutil -sr_enable --name=<b>SITE1</b>
   </code></pre>

1. **[2]** İkinci düğümde Sistem Çoğaltma'yı yapılandırın:
    
   Sistem çoğaltmabaşlatmak için ikinci düğüm kaydedin. Hanasid\>adm <aşağıdaki komutu çalıştırın:

   <pre><code>sapcontrol -nr <b>03</b> -function StopWait 600 10
   hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b>
   </code></pre>

1. **[1]** Çoğaltma durumunu denetleyin

   Çoğaltma durumunu denetleyin ve tüm veritabanları eşitlenene kadar bekleyin. Durum bilinmiyorsa, güvenlik duvarı ayarlarınızı kontrol edin.

   <pre><code>sudo su - <b>hn1</b>adm -c "python /usr/sap/<b>HN1</b>/HDB<b>03</b>/exe/python_support/systemReplicationStatus.py"
   # | Database | Host     | Port  | Service Name | Volume ID | Site ID | Site Name | Secondary | Secondary | Secondary | Secondary | Secondary     | Replication | Replication | Replication    |
   # |          |          |       |              |           |         |           | Host      | Port      | Site ID   | Site Name | Active Status | Mode        | Status      | Status Details |
   # | -------- | -------- | ----- | ------------ | --------- | ------- | --------- | --------- | --------- | --------- | --------- | ------------- | ----------- | ----------- | -------------- |
   # | SYSTEMDB | <b>hn1-db-0</b> | 30301 | nameserver   |         1 |       1 | <b>SITE1</b>     | <b>hn1-db-1</b>  |     30301 |         2 | <b>SITE2</b>     | YES           | SYNC        | ACTIVE      |                |
   # | <b>HN1</b>      | <b>hn1-db-0</b> | 30307 | xsengine     |         2 |       1 | <b>SITE1</b>     | <b>hn1-db-1</b>  |     30307 |         2 | <b>SITE2</b>     | YES           | SYNC        | ACTIVE      |                |
   # | <b>NW1</b>      | <b>hn1-db-0</b> | 30340 | indexserver  |         2 |       1 | <b>SITE1</b>     | <b>hn1-db-1</b>  |     30340 |         2 | <b>SITE2</b>     | YES           | SYNC        | ACTIVE      |                |
   # | <b>HN1</b>      | <b>hn1-db-0</b> | 30303 | indexserver  |         3 |       1 | <b>SITE1</b>     | <b>hn1-db-1</b>  |     30303 |         2 | <b>SITE2</b>     | YES           | SYNC        | ACTIVE      |                |
   #
   # status system replication site "2": ACTIVE
   # overall system replication status: ACTIVE
   #
   # Local System Replication State
   # ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
   #
   # mode: PRIMARY
   # site id: 1
   # site name: <b>SITE1</b>
   </code></pre>

## <a name="configure-sap-hana-10-system-replication"></a>SAP HANA 1.0 Sistem Çoğaltma yapılandırma

Bu bölümdeki adımlar aşağıdaki önekleri kullanır:

* **[A]**: Adım tüm düğümler için geçerlidir.
* **[1]**: Adım yalnızca düğüm 1 için geçerlidir.
* **[2]**: Adım yalnızca Pacemaker kümesinin 2 numaralı düğümü için geçerlidir.

1. **[A]** Güvenlik duvarLarını yapılandırma

   HANA Sistem Çoğaltma ve istemci trafiğine izin vermek için güvenlik duvarı kuralları oluşturun. Gerekli bağlantı noktaları [Tüm SAP Ürünlerinin TCP/IP bağlantı noktalarında](https://help.sap.com/viewer/ports)listelenir. Aşağıdaki komutlar HANA 2.0 Sistem Çoğaltma izin vermek için sadece bir örnektir. SAP HANA 1.0 kurulumunuza uyarla.

   <pre><code>sudo firewall-cmd --zone=public --add-port=40302/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=40302/tcp
   </code></pre>

1. **[1]** Gerekli kullanıcıları oluşturun.

   Aşağıdaki komutu kök olarak çalıştırın. Sap HANA kurulumdeğerlerinin değerleriile kalın dizeleri (HANA System ID **HN1** ve örnek numarası **03)** değiştirmeyi unutmayın:

   <pre><code>PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"'
   hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync'
   hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME'
   </code></pre>

1. **[A]** Keystore girişini oluşturun.

   Yeni bir keystore girişi oluşturmak için aşağıdaki komutu kök olarak çalıştırın:

   <pre><code>PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
   hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
   </code></pre>

1. **[1]** Veritabanını yedekleyin.

   Veritabanlarını kök olarak yedekleyin:

   <pre><code>PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
   hdbsql -d SYSTEMDB -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')"
   </code></pre>

   Çok kiracılı yükleme kullanıyorsanız, kiracı veritabanını da yedekleyin:

   <pre><code>hdbsql -d <b>HN1</b> -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')"
   </code></pre>

1. **[1]** Sistem Çoğaltma'yı ilk düğümde yapılandırın.

   <hanasid\>adm olarak birincil site oluşturun:

   <pre><code>su - <b>hdb</b>adm
   hdbnsutil -sr_enable –-name=<b>SITE1</b>
   </code></pre>

1. **[2]** İkincil düğümüzerinde Sistem Çoğaltma'yı yapılandırın.

   <hanasid\>adm olarak ikincil site kaydedin:

   <pre><code>HDB stop
   hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b>
   HDB start
   </code></pre>

## <a name="create-a-pacemaker-cluster"></a>Kalp Pili kümesi oluşturma

Bu HANA sunucusu için temel bir Pacemaker kümesi oluşturmak için [Azure'da Red Hat Enterprise Linux'ta Pacemaker'ı ayarlama](high-availability-guide-rhel-pacemaker.md) adımlarını izleyin.

## <a name="create-sap-hana-cluster-resources"></a>SAP HANA küme kaynakları oluşturma

**Tüm düğümlere**SAP HANA kaynak aracılarını yükleyin. Paketi içeren bir depoyu etkinleştirdiğinden emin olun.

<pre><code># Enable repository that contains SAP HANA resource agents
sudo subscription-manager repos --enable="rhel-sap-hana-for-rhel-7-server-rpms"
   
sudo yum install -y resource-agents-sap-hana
</code></pre>

Sonra, HANA topolojisi oluşturun. Pacemaker küme düğümlerinden birinde aşağıdaki komutları çalıştırın:

<pre><code>sudo pcs property set maintenance-mode=true

# Replace the bold string with your instance number and HANA system ID
sudo pcs resource create SAPHanaTopology_<b>HN1</b>_<b>03</b> SAPHanaTopology SID=<b>HN1</b> InstanceNumber=<b>03</b> \
op start timeout=600 op stop timeout=300 op monitor interval=10 timeout=600 \
--clone clone-max=2 clone-node-max=1 interleave=true
</code></pre>

Ardından, HANA kaynaklarını oluşturun:

<pre><code># Replace the bold string with your instance number, HANA system ID, and the front-end IP address of the Azure load balancer.

sudo pcs resource create SAPHana_<b>HN1</b>_<b>03</b> SAPHana SID=<b>HN1</b> InstanceNumber=<b>03</b> PREFER_SITE_TAKEOVER=true DUPLICATE_PRIMARY_TIMEOUT=7200 AUTOMATED_REGISTER=false \
op start timeout=3600 op stop timeout=3600 \
op monitor interval=61 role="Slave" timeout=700 \
op monitor interval=59 role="Master" timeout=700 \
op promote timeout=3600 op demote timeout=3600 \
master notify=true clone-max=2 clone-node-max=1 interleave=true

sudo pcs resource create vip_<b>HN1</b>_<b>03</b> IPaddr2 ip="<b>10.0.0.13</b>"

sudo pcs resource create nc_<b>HN1</b>_<b>03</b> azure-lb port=625<b>03</b>

sudo pcs resource group add g_ip_<b>HN1</b>_<b>03</b> nc_<b>HN1</b>_<b>03</b> vip_<b>HN1</b>_<b>03</b>

sudo pcs constraint order SAPHanaTopology_<b>HN1</b>_<b>03</b>-clone then SAPHana_<b>HN1</b>_<b>03</b>-master symmetrical=false

sudo pcs constraint colocation add g_ip_<b>HN1</b>_<b>03</b> with master SAPHana_<b>HN1</b>_<b>03</b>-master 4000

sudo pcs property set maintenance-mode=false
</code></pre>

Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıklarından emin olun. Kaynakların hangi düğümün çalıştırıldığı önemli değildir.

> [!NOTE]
> Yukarıdaki yapılandırmadaki zaman ekmeleri yalnızca örneklerdir ve belirli HANA kurulumuna uyarlanmış olması gerekebilir. Örneğin, SAP HANA veritabanını başlatmak daha uzun sürüyorsa, başlangıç zaman aşımını artırmanız gerekebilir.  

<pre><code>sudo pcs status

# Online: [ hn1-db-0 hn1-db-1 ]
#
# Full list of resources:
#
# azure_fence     (stonith:fence_azure_arm):      Started hn1-db-0
#  Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
#      Started: [ hn1-db-0 hn1-db-1 ]
#  Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
#      Masters: [ hn1-db-0 ]
#      Slaves: [ hn1-db-1 ]
#  Resource Group: g_ip_HN1_03
#      nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-0
#      vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-0
</code></pre>

## <a name="test-the-cluster-setup"></a>Küme kurulumını test edin

Bu bölümde kurulumunuzu nasıl sınayabileceğiniz açıklanmaktadır. Bir teste başlamadan önce, Pacemaker'ın herhangi bir başarısız eylemi olmadığından (pc'ler durumu yla), beklenmeyen konum kısıtlamaları olmadığından (örneğin geçiş testi artıkları) ve HANA'nın eşitleme durumu olduğundan emin olun, örneğin systemReplicationStatus ile:

<pre><code>[root@hn1-db-0 ~]# sudo su - hn1adm -c "python /usr/sap/HN1/HDB03/exe/python_support/systemReplicationStatus.py"
</code></pre>

### <a name="test-the-migration"></a>Geçişi test edin

Teste başlamadan önce kaynak durumu:

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-0 ]
    Slaves: [ hn1-db-1 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-0
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-0
</code></pre>

Aşağıdaki komutu çalıştırarak SAP HANA ana düğümü geçirebilirsiniz:

<pre><code>[root@hn1-db-0 ~]# pcs resource move SAPHana_HN1_03-master
</code></pre>

Ayarlarsanız, `AUTOMATED_REGISTER="false"`bu komut SAP HANA ana düğümü ve hn1-db-1 için sanal IP adresi içeren grup geçirmelisiniz.

Geçiş yapıldıktan sonra, 'sudo pcs durumu' çıkışı şu na benzer

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-1 ]
    Stopped: [ hn1-db-0 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-1
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-1
</code></pre>

HN1-db-0'daki SAP HANA kaynağı durduruldu. Bu durumda, hana örneğini bu komutu çalıştırarak ikincil olarak yapılandırın:

<pre><code>[root@hn1-db-0 ~]# su - hn1adm

# Stop the HANA instance just in case it is running
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> sapcontrol -nr 03 -function StopWait 600 10
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-1 --remoteInstance=03 --replicationMod
e=sync --name=SITE1
</code></pre>

Geçiş, yeniden silinmesi gereken konum kısıtlamaları oluşturur:

<pre><code># Switch back to root
exit
[root@hn1-db-0 ~]# pcs resource clear SAPHana_HN1_03-master
</code></pre>

'Pcs durumunu' kullanarak HANA kaynağının durumunu izleyin. HANA hn1-db-0'da başlatıldıktan sonra, çıktı şu şekilde görünmelidir

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-1 ]
    Slaves: [ hn1-db-0 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-1
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-1
</code></pre>

### <a name="test-the-azure-fencing-agent"></a>Azure eskrim aracısını test edin

Teste başlamadan önce kaynak durumu:

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-1 ]
    Slaves: [ hn1-db-0 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-1
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-1
</code></pre>

SAP HANA'nın Master olarak çalıştığı düğümdeki ağ arabirimini devre dışı bırakarak Azure eskrim aracısının kurulumlarını sınayabilirsiniz.
Bir ağ hatasının nasıl simüle edilebildiğini anlatan bir açıklama için [Red Hat Knowledgebase makalesi 79523'e](https://access.redhat.com/solutions/79523) bakın. Bu örnekte, ağa tüm erişimi engellemek için net_breaker komut dosyasını kullanırız.

<pre><code>[root@hn1-db-1 ~]# sh ./net_breaker.sh BreakCommCmd 10.0.0.6
</code></pre>

Sanal makine artık küme yapılandırmanıza bağlı olarak yeniden başlatmalı veya durmalıdır.
Ayarı `stonith-action` kapalı ayarlarsanız, sanal makine durdurulur ve kaynaklar çalışan sanal makineye geçirilir.

> [!NOTE]
> Sanal makinelerin yeniden çevrimiçi olması 15 dakika kadar sürebilir.

Sanal makineyi yeniden başlattıktan sonra, SAP HANA kaynağını ayarlarsanız `AUTOMATED_REGISTER="false"`ikincil olarak başlatılmaz. Bu durumda, hana örneğini bu komutu çalıştırarak ikincil olarak yapılandırın:

<pre><code>su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> sapcontrol -nr <b>03</b> -function StopWait 600 10
hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b>

# Switch back to root and clean up the failed state
exit
[root@hn1-db-1 ~]# pcs resource cleanup SAPHana_HN1_03-master
</code></pre>

Testten sonra kaynak durumu:

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-0 ]
    Slaves: [ hn1-db-1 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-0
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-0
</code></pre>

### <a name="test-a-manual-failover"></a>Manuel başarısızlamayı test edin

Teste başlamadan önce kaynak durumu:

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-0 ]
    Slaves: [ hn1-db-1 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-0
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-0
</code></pre>

Hn1-db-0 düğümünde kümeyi durdurarak manuel bir başarısızlığı test edebilirsiniz:

<pre><code>[root@hn1-db-0 ~]# pcs cluster stop
</code></pre>

Başarısız olduktan sonra kümeyi yeniden başlatabilirsiniz. Ayarlarsanız, `AUTOMATED_REGISTER="false"`hn1-db-0 düğümündeki SAP HANA kaynağı ikincil olarak başlayamaz. Bu durumda, hana örneğini bu komutu çalıştırarak ikincil olarak yapılandırın:

<pre><code>[root@hn1-db-0 ~]# pcs cluster start
[root@hn1-db-0 ~]# su - hn1adm

# Stop the HANA instance just in case it is running
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> sapcontrol -nr 03 -function StopWait 600 10
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# Switch back to root and clean up the failed state
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> exit
[root@hn1-db-1 ~]# pcs resource cleanup SAPHana_HN1_03-master
</code></pre>

Testten sonra kaynak durumu:

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-1 ]
     Slaves: [ hn1-db-0 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-1
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-1
</code></pre>

## <a name="next-steps"></a>Sonraki adımlar

* [AZURE Sanal Makineler SAP için planlama ve uygulama][planning-guide]
* [SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* Yüksek kullanılabilirlik oluşturmayı ve AZURE'da SAP HANA'nın olağanüstü durum kurtarmasını nasıl planlayabilirsiniz (büyük [örnekler), Azure'da SAP HANA (büyük örnekler) yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md) bölümüne bakın
