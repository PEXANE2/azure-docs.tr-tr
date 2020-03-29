---
title: SLES'te Azure VM'lerde SAP HANA'nın yüksek kullanılabilirliği | Microsoft Dokümanlar
description: SUSE Linux Enterprise Server'da Azure VM'lerde SAP HANA'nın yüksek kullanılabilirliği
services: virtual-machines-linux
documentationcenter: ''
author: rdeltcheva
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/06/2020
ms.author: radeltch
ms.openlocfilehash: 69dcf91957263cea36f8ff6db6a7af14588998ee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78927220"
---
# <a name="high-availability-of-sap-hana-on-azure-vms-on-suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server'da Azure VM'lerde SAP HANA'nın yüksek kullanılabilirliği

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
[401162]:https://launchpad.support.sap.com/#/notes/401162

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d
[sles-for-sap-bp]:https://www.suse.com/documentation/sles-for-sap-12/

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json

Şirket içi geliştirme için HANA Sistem Çoğaltma'yı kullanabilir veya SAP HANA için yüksek kullanılabilirlik oluşturmak için paylaşılan depolamayı kullanabilirsiniz.
Azure sanal makinelerde (VM) Azure'daki HANA System Replication şu anda desteklenen tek yüksek kullanılabilirlik işlevidir. SAP HANA Çoğaltma bir birincil düğüm ve en az bir ikincil düğüm oluşur. Birincil düğümdeki verilerdeki değişiklikler, ikincil düğüme eşzamanlı olarak veya eşzamanlı olarak çoğaltılır.

Bu makalede, sanal makinelerin nasıl dağıtılanın ve yapılandırılabilmek, küme çerçevesini yüklemek ve SAP HANA Sistem Çoğaltma'yı yüklemek ve yapılandırmak açıklanmaktadır.
Örnek yapılandırmalarda yükleme komutları, örnek numarası **03**ve HANA System ID **HN1** kullanılır.

Önce aşağıdaki SAP Notlarını ve bildirilerini okuyun:

* SAP Note [1928533], olan:
  * SAP yazılımının dağıtımı için desteklenen Azure VM boyutlarının listesi.
  * Azure VM boyutları için önemli kapasite bilgileri.
  * Desteklenen SAP yazılımı ve işletim sistemi (OS) ve veritabanı kombinasyonları.
  * Microsoft Azure'da Windows ve Linux için gerekli SAP çekirdeği sürümü.
* SAP Note [2015553,] Azure'da SAP destekli SAP yazılım dağıtımları için ön koşulları listeler.
* SAP Note [2205917] SAP Uygulamaları için SUSE Linux Enterprise Server için işletim sistemi ayarlarını önermiştir.
* SAP Note [1944799] SAP Uygulamaları için SUSE Linux Enterprise Server için SAP HANA Yönergeleri vardır.
* SAP Note [2178632,] Azure'da SAP için bildirilen tüm izleme ölçümleri hakkında ayrıntılı bilgilere sahiptir.
* SAP Note [2191498,] Azure'da Linux için gerekli SAP Host Agent sürümüne sahiptir.
* SAP Note [2243692,] Azure'da Linux'ta SAP lisanslama hakkında bilgi edinmiştir.
* SAP Note [1984787,] SUSE Linux Enterprise Server 12 hakkında genel bilgilere sahiptir.
* SAP Note [1999351,] SAP için Azure Gelişmiş İzleme Uzantısı için ek sorun giderme bilgilerine sahiptir.
* SAP Note [401162,] HANA Sistem Çoğaltma'yı kurarken "zaten kullanılmakta olan adresten" nasıl kaçınılane ilişkin bilgilere sahiptir.
* [SAP Community WIKI,](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) Linux için gerekli tüm SAP Notlarına sahiptir.
* [SAP HANA Sertifikalı IaaS Platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)
* [Azure Sanal Makineler, SAP için Linux kılavuzunda planlama ve uygulama.][planning-guide]
* [Linux'ta SAP için Azure Sanal Makineler dağıtımı][deployment-guide] (bu makale).
* [Linux kılavuzunda SAP için Azure Sanal Makineler DBMS dağıtımı.][dbms-guide]
* [SAP Applications 12 SP3 en iyi uygulamalar kılavuzları için SUSE Linux Enterprise Server][sles-for-sap-bp]
  * SAP HANA SR Performans Optimize Edilmiş Altyapı (SAP Uygulamaları için SLES 12 SP1) kurulumu. Kılavuz, şirket içi geliştirme için SAP HANA Sistem Çoğaltma'yı kurmak için gerekli tüm bilgileri içerir. Bu kılavuzu temel olarak kullanın.
  * SAP HANA SR Maliyet Optimize Edilmiş Altyapı Kurulumu (SAP Uygulamaları için SLES 12 SP1) Kurulumu

## <a name="overview"></a>Genel Bakış

Yüksek kullanılabilirlik elde etmek için, SAP HANA iki sanal makineye yüklenir. Veriler HANA Sistem Çoğaltma kullanılarak çoğaltılır.

![SAP HANA yüksek kullanılabilirlik genel bakış](./media/sap-hana-high-availability/ha-suse-hana.png)

SAP HANA Sistem Çoğaltma kurulumu, özel bir sanal ana bilgisayar adı ve sanal IP adresleri kullanır. Azure'da, sanal bir IP adresi kullanmak için bir yük dengeleyicisi gereklidir. Aşağıdaki liste yük dengeleyicisinin yapılandırmasını gösterir:

* Ön uç yapılandırması: HN1-db için IP adresi 10.0.0.13
* Arka uç yapılandırması: HANA Sistem Çoğaltma'nın bir parçası olması gereken tüm sanal makinelerin birincil ağ arabirimlerine bağlı
* Sonda Bağlantı Noktası: Bağlantı Noktası 62503
* Yük dengeleme kuralları: 30313 TCP, 30315 TCP, 30317 TCP

## <a name="deploy-for-linux"></a>Linux için dağıt

SAP HANA'nın kaynak aracısı SAP Uygulamaları için SUSE Linux Enterprise Server'a dahildir.
Azure Marketi, SAP Applications 12 için SUSE Linux Enterprise Server için yeni sanal makineleri dağıtmak için kullanabileceğiniz bir görüntü içerir.

### <a name="deploy-with-a-template"></a>Şablonla dağıtma

Gerekli tüm kaynakları dağıtmak için GitHub'da olan hızlı başlangıç şablonlarından birini kullanabilirsiniz. Şablon sanal makineleri, yük dengeleyicisini, kullanılabilirlik kümesini ve benzeri uygulamaları dağır.
Şablonu dağıtmak için aşağıdaki adımları izleyin:

1. Azure portalında [veritabanı şablonu][template-multisid-db] veya [yakınsama şablonu][template-converged] açın. 
    Veritabanı şablonu yalnızca bir veritabanı için yük dengeleme kuralları oluşturur. Yakınsanan şablon, ascs/SCS ve ERS (yalnızca Linux) örneği için yük dengeleme kurallarını da oluşturur. SAP NetWeaver tabanlı bir sistem yüklemeyi planlıyorsanız ve ASCS/SCS örneğini aynı makinelere yüklemek istiyorsanız, [yakınsanan şablonu][template-converged]kullanın.

1. Aşağıdaki parametreleri girin:
    - **Sap Sistem Kimliği**: Yüklemek istediğiniz SAP sisteminin SAP sistem kimliğini girin. Kimlik, dağıtılan kaynaklar için önek olarak kullanılır.
    - **Yığın Türü**: (Bu parametre yalnızca yakınsanmış şablonu kullanıyorsanız uygulanabilir.) SAP NetWeaver yığın türünü seçin.
    - **Os Türü**: Linux dağıtımlarından birini seçin. Bu örnekiçin, **SLES 12'yi**seçin.
    - **Db Türü**: **HANA'yı**seçin.
    - **Sap Sistem Boyutu**: Yeni sistemin sağlayacağı SAPS sayısını girin. Sistemin kaç SAPS gerektirdiğinden emin değilseniz, SAP Teknoloji Ortağınıza veya Sistem Entegratörünüze danışın.
    - **Sistem Kullanılabilirliği**: **SELECT HA**.
    - **Yönetici Kullanıcı Adı ve Yönetici Şifresi**: Makinede oturum açabilen yeni bir kullanıcı oluşturulur.
    - **Yeni veya Varolan Alt Ağ**: Yeni bir sanal ağ ve alt ağ oluşturulup oluşturulmaması gerektiğini veya varolan bir alt ağ kullanılıp kullanılmayacağını belirler. Şirket içi ağınıza bağlı bir sanal ağınız **varsa, Varolan'ı**seçin.
    - **Subnet ID**: VM'yi, vm'nin atanması gereken tanımlanmış bir alt ağına yerleştirmek istiyorsanız, o alt ağın kimliğini adlandırın. Kimlik genellikle **/abonelik/\<abonelik kimliği>/kaynakGruplar/\<kaynak grup adı>/providers/Microsoft.Network/virtualNetworks/\<sanal\<ağ adı>/subnet/subnet adı>** gibi görünür.

### <a name="manual-deployment"></a>El ile dağıtım

> [!IMPORTANT]
> Seçtiğiniz işletim sistemi kullandığınız belirli VM türleri SAP HANA için SAP sertifikalı olduğundan emin olun. SAP HANA sertifikalı VM türleri ve işletim sistemi sürümleri listesi [SAP HANA Sertifikalı IaaS Platformlarında](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)aranabilir. Belirli VM türü için SAP HANA destekli işletim sistemi sürümlerinin tam listesini almak için listelenen VM türünün ayrıntılarına tıklamayı unutmayın
>  

1. Bir kaynak grubu oluşturun.
1. Sanal ağ oluşturun.
1. Kullanılabilirlik kümesi oluşturun.
   - Maksimum güncelleştirme etki alanını ayarlayın.
1. Yük dengeleyici (dahili) oluşturun. Standart [yük dengeleyicisi](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview)öneririz.
   - Adım 2'de oluşturulan sanal ağı seçin.
1. Sanal makine 1 oluşturun.
   - Seçtiğiniz VM türünde SAP HANA için desteklenen Azure galerisinde bir SLES4SAP resmi kullanın.
   - Adım 3'te oluşturulan kullanılabilirlik kümesini seçin.
1. Sanal makine 2 oluşturun.
   - Seçtiğiniz VM türünde SAP HANA için desteklenen Azure galerisinde bir SLES4SAP resmi kullanın.
   - Adım 3'te oluşturulan kullanılabilirlik kümesini seçin. 
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
      1. **Sanal Ağ'ı**seçin.
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

## <a name="create-a-pacemaker-cluster"></a>Kalp Pili kümesi oluşturma

Bu HANA sunucusu için temel bir Pacemaker kümesi oluşturmak için [Azure'da SUSE Linux Enterprise Server'da Pacemaker'ı ayarlama](high-availability-guide-suse-pacemaker.md) adımlarını izleyin. SAP HANA ve SAP NetWeaver (A)SCS için aynı Kalp Pili kümesini kullanabilirsiniz.

## <a name="install-sap-hana"></a>SAP HANA yükleme

Bu bölümdeki adımlar aşağıdaki önekleri kullanır:
- **[A]**: Adım tüm düğümler için geçerlidir.
- **[1]**: Adım yalnızca düğüm 1 için geçerlidir.
- **[2]**: Adım yalnızca Pacemaker kümesinin 2 numaralı düğümü için geçerlidir.

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

   Mantıksal birimleri oluşturun. `-i` Anahtar olmadan kullandığınızda `lvcreate` doğrusal bir birim oluşturulur. Daha iyi G/Ç performansı için, bağımsız değişkenin `-i` temel fiziksel birim sayısı olması gereken çizgili bir hacim oluşturmanızı öneririz. Bu belgede, veri hacmi için iki fiziksel birim `-i` kullanılır, bu nedenle anahtar bağımsız değişkeni **2**olarak ayarlanır. Günlük hacmi için bir fiziksel birim `-i` kullanılır, bu nedenle açık bir şekilde anahtar kullanılmaz. `-i` Her veri, günlük veya paylaşılan birimler için birden fazla fiziksel birim kullandığınızda anahtarı kullanın ve temel fiziksel birim sayısına ayarlayın.

   <pre><code>sudo lvcreate <b>-i 2</b> -l 100%FREE -n hana_data vg_hana_data_<b>HN1</b>
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

1. **[A]** SAP HANA yüksek kullanılabilirlik paketlerini yükleyin:

   <pre><code>sudo zypper install SAPHanaSR
   </code></pre>

SAP HANA Sistem Çoğaltma yüklemek için, [SAP HANA SR Performans Optimize Senaryo kılavuzunun](https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/)bölüm 4 izleyin.

1. **[A]** HANA DVD'sinden **hdblcm** programını çalıştırın. İstem de aşağıdaki değerleri girin:
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

## <a name="configure-sap-hana-20-system-replication"></a>SAP HANA 2.0 Sistem Çoğaltma yapılandırma

Bu bölümdeki adımlar aşağıdaki önekleri kullanır:

* **[A]**: Adım tüm düğümler için geçerlidir.
* **[1]**: Adım yalnızca düğüm 1 için geçerlidir.
* **[2]**: Adım yalnızca Pacemaker kümesinin 2 numaralı düğümü için geçerlidir.

1. **[1]** Kiracı veritabanını oluşturun.

   SAP HANA 2.0 veya MDC kullanıyorsanız, SAP NetWeaver sisteminiz için bir kiracı veritabanı oluşturun. **NW1'i** SAP sisteminizin SID'si ile değiştirin.

   Hanasid\>adm <aşağıdaki komutu yürütmek:

   <pre><code>hdbsql -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> -d SYSTEMDB 'CREATE DATABASE <b>NW1</b> SYSTEM USER PASSWORD "<b>passwd</b>"'
   </code></pre>

1. **[1]** Sistem Çoğaltma'yı ilk düğümde yapılandır:

   <hanasid\>adm olarak veritabanları yedeklemek:

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
    
   Sistem çoğaltmabaşlatmak için ikinci düğüm kaydedin. hanasid\>adm <aşağıdaki komutu çalıştırın:

   <pre><code>sapcontrol -nr <b>03</b> -function StopWait 600 10
   hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
   </code></pre>

## <a name="configure-sap-hana-10-system-replication"></a>SAP HANA 1.0 Sistem Çoğaltma yapılandırma

Bu bölümdeki adımlar aşağıdaki önekleri kullanır:

* **[A]**: Adım tüm düğümler için geçerlidir.
* **[1]**: Adım yalnızca düğüm 1 için geçerlidir.
* **[2]**: Adım yalnızca Pacemaker kümesinin 2 numaralı düğümü için geçerlidir.

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

   <pre><code>sapcontrol -nr <b>03</b> -function StopWait 600 10
   hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
   </code></pre>

## <a name="create-sap-hana-cluster-resources"></a>SAP HANA küme kaynakları oluşturma

İlk olarak, HANA topolojioluşturun. Pacemaker küme düğümlerinden birinde aşağıdaki komutları çalıştırın:

<pre><code>sudo crm configure property maintenance-mode=true

# Replace the bold string with your instance number and HANA system ID

sudo crm configure primitive rsc_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> ocf:suse:SAPHanaTopology \
  operations \$id="rsc_sap2_<b>HN1</b>_HDB<b>03</b>-operations" \
  op monitor interval="10" timeout="600" \
  op start interval="0" timeout="600" \
  op stop interval="0" timeout="300" \
  params SID="<b>HN1</b>" InstanceNumber="<b>03</b>"

sudo crm configure clone cln_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> \
  meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"
</code></pre>

Ardından, HANA kaynaklarını oluşturun:

> [!IMPORTANT]
> Son testler, netcat'in biriktirme listesi ve yalnızca bir bağlantı taşıma sınırlaması nedeniyle isteklere yanıt vermeyi bıraktığı durumları ortaya çıkardı. Netcat kaynağı Azure Yük dengeleyici isteklerini dinlemeyi durdurur ve kayan IP kullanılamaz hale gelir.  
> Mevcut Pacemaker kümeleri için, geçmişte netcat'i socat ile değiştirmemizi tavsiye ettik. Şu anda, paket kaynak aracılarının bir parçası olan azure-lb kaynak aracısını aşağıdaki paket sürüm gereksinimleriyle birlikte kullanmanızı öneririz:
> - SLES 12 SP4/SP5 için, sürüm en az kaynak-aracılar-4.3.018.a7fb5035-3.30.1 olmalıdır.  
> - SLES 15/15 SP1 için, sürüm en az kaynak-ajanlar-4.3.0184.6ee15eb2-4.13.1 olmalıdır.  
>
> Değişikliğin kısa bir kapalı kalma süresi gerektireceğini unutmayın.  
> Varolan Pacemaker kümeleri için yapılandırma Azure [Load-Balancer Detection Hardening'de](https://www.suse.com/support/kb/doc/?id=7024128)açıklandığı gibi socat kullanmak üzere zaten değiştirildiyse, hemen azure-lb kaynak aracısına geçmeniz gerek yoktur.

<pre><code># Replace the bold string with your instance number, HANA system ID, and the front-end IP address of the Azure load balancer. 

sudo crm configure primitive rsc_SAPHana_<b>HN1</b>_HDB<b>03</b> ocf:suse:SAPHana \
  operations \$id="rsc_sap_<b>HN1</b>_HDB<b>03</b>-operations" \
  op start interval="0" timeout="3600" \
  op stop interval="0" timeout="3600" \
  op promote interval="0" timeout="3600" \
  op monitor interval="60" role="Master" timeout="700" \
  op monitor interval="61" role="Slave" timeout="700" \
  params SID="<b>HN1</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
  DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"

sudo crm configure ms msl_SAPHana_<b>HN1</b>_HDB<b>03</b> rsc_SAPHana_<b>HN1</b>_HDB<b>03</b> \
  meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
  target-role="Started" interleave="true"

sudo crm configure primitive rsc_ip_<b>HN1</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \
  meta target-role="Started" is-managed="true" \
  operations \$id="rsc_ip_<b>HN1</b>_HDB<b>03</b>-operations" \
  op monitor interval="10s" timeout="20s" \
  params ip="<b>10.0.0.13</b>"

sudo crm configure primitive rsc_nc_<b>HN1</b>_HDB<b>03</b> azure-lb port=625<b>03</b>

sudo crm configure group g_ip_<b>HN1</b>_HDB<b>03</b> rsc_ip_<b>HN1</b>_HDB<b>03</b> rsc_nc_<b>HN1</b>_HDB<b>03</b>

sudo crm configure colocation col_saphana_ip_<b>HN1</b>_HDB<b>03</b> 4000: g_ip_<b>HN1</b>_HDB<b>03</b>:Started \
  msl_SAPHana_<b>HN1</b>_HDB<b>03</b>:Master  

sudo crm configure order ord_SAPHana_<b>HN1</b>_HDB<b>03</b> Optional: cln_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> \
  msl_SAPHana_<b>HN1</b>_HDB<b>03</b>

# Clean up the HANA resources. The HANA resources might have failed because of a known issue.
sudo crm resource cleanup rsc_SAPHana_<b>HN1</b>_HDB<b>03</b>

sudo crm configure property maintenance-mode=false
sudo crm configure rsc_defaults resource-stickiness=1000
sudo crm configure rsc_defaults migration-threshold=5000
</code></pre>

Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıklarından emin olun. Kaynakların hangi düğümün çalıştırıldığı önemli değildir.

<pre><code>sudo crm_mon -r

# Online: [ hn1-db-0 hn1-db-1 ]
#
# Full list of resources:
#
# stonith-sbd     (stonith:external/sbd): Started hn1-db-0
# rsc_st_azure    (stonith:fence_azure_arm):      Started hn1-db-1
# Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
#     Started: [ hn1-db-0 hn1-db-1 ]
# Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
#     Masters: [ hn1-db-0 ]
#     Slaves: [ hn1-db-1 ]
# Resource Group: g_ip_HN1_HDB03
#     rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
#     rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-0
</code></pre>

## <a name="test-the-cluster-setup"></a>Küme kurulumını test edin

Bu bölümde kurulumunuzu nasıl sınayabileceğiniz açıklanmaktadır. Her test kök olduğunuzu varsayar ve SAP HANA yöneticisi **hn1-db-0** sanal makinede çalışır.

### <a name="test-the-migration"></a>Geçişi test edin

Teste başlamadan önce, Pacemaker'ın herhangi bir başarısız eylemi olmadığından (crm_mon -r üzerinden), beklenmeyen konum kısıtlamaları olmadığından (örneğin geçiş testiartıkları) ve HANA'nın örneğin SAPHanaSR-showAttr ile eşitleme durumu olduğundan emin olun:

<pre><code>hn1-db-0:~ # SAPHanaSR-showAttr

Global cib-time
--------------------------------
global Mon Aug 13 11:26:04 2018

Hosts    clone_state lpa_hn1_lpt node_state op_mode   remoteHost    roles                            score site  srmode sync_state version                vhost
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
hn1-db-0 PROMOTED    1534159564  online     logreplay nws-hana-vm-1 4:P:master1:master:worker:master 150   SITE1 sync   PRIM       2.00.030.00.1522209842 nws-hana-vm-0
hn1-db-1 DEMOTED     30          online     logreplay nws-hana-vm-0 4:S:master1:master:worker:master 100   SITE2 sync   SOK        2.00.030.00.1522209842 nws-hana-vm-1
</code></pre>

Aşağıdaki komutu çalıştırarak SAP HANA ana düğümü geçirebilirsiniz:

<pre><code>crm resource migrate msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-1</b>
</code></pre>

Ayarlarsanız, `AUTOMATED_REGISTER="false"`bu komut dizisi SAP HANA ana düğümü ve sanal IP adresini içeren grubu hn1-db-1'e geçirmelidir.

Geçiş yapıldıktan sonra crm_mon -r çıktısı şu na benzer

<pre><code>Online: [ hn1-db-0 hn1-db-1 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started hn1-db-1
 Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
     Started: [ hn1-db-0 hn1-db-1 ]
 Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
     Masters: [ hn1-db-1 ]
     Stopped: [ hn1-db-0 ]
 Resource Group: g_ip_HN1_HDB03
     rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
     rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-1

Failed Actions:
* rsc_SAPHana_HN1_HDB03_start_0 on hn1-db-0 'not running' (7): call=84, status=complete, exitreason='none',
    last-rc-change='Mon Aug 13 11:31:37 2018', queued=0ms, exec=2095ms
</code></pre>

HN1-db-0'daki SAP HANA kaynağı ikincil olarak başlamazsa. Bu durumda, hana örneğini bu komutu çalıştırarak ikincil olarak yapılandırın:

<pre><code>su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> sapcontrol -nr <b>03</b> -function StopWait 600 10
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>
</code></pre>

Geçiş, yeniden silinmesi gereken konum kısıtlamaları oluşturur:

<pre><code># Switch back to root and clean up the failed state
exit
hn1-db-0:~ # crm resource unmigrate msl_SAPHana_<b>HN1</b>_HDB<b>03</b>
</code></pre>

Ayrıca ikincil düğüm kaynağının durumunu temizlemeniz gerekir:

<pre><code>hn1-db-0:~ # crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

crm_mon -r kullanarak HANA kaynağının durumunu izleyin. HANA hn1-db-0'da başlatıldıktan sonra, çıktı şu şekilde görünmelidir

<pre><code>Online: [ hn1-db-0 hn1-db-1 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started hn1-db-1
 Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
     Started: [ hn1-db-0 hn1-db-1 ]
 Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
     Masters: [ hn1-db-1 ]
     Slaves: [ hn1-db-0 ]
 Resource Group: g_ip_HN1_HDB03
     rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
     rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-1
</code></pre>

### <a name="test-the-azure-fencing-agent-not-sbd"></a>Azure eskrim aracısını test edin (SBD değil)

Hn1-db-0 düğümündeki ağ arabirimini devre dışı bırakarak Azure eskrim aracısının kurulumını test edebilirsiniz:

<pre><code>sudo ifdown eth0
</code></pre>

Sanal makine artık küme yapılandırmanıza bağlı olarak yeniden başlatmalı veya durmalıdır.
Ayarı `stonith-action` kapalı ayarlarsanız, sanal makine durdurulur ve kaynaklar çalışan sanal makineye geçirilir.

Sanal makineyi yeniden başlattıktan sonra, SAP HANA kaynağını ayarlarsanız `AUTOMATED_REGISTER="false"`ikincil olarak başlatılmaz. Bu durumda, hana örneğini bu komutu çalıştırarak ikincil olarak yapılandırın:

<pre><code>su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# Switch back to root and clean up the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

### <a name="test-sbd-fencing"></a>Test SBD eskrim

Engizitör işlemini öldürerek SBD'nin kurulumlarını test edebilirsiniz.

<pre><code>hn1-db-0:~ # ps aux | grep sbd
root       1912  0.0  0.0  85420 11740 ?        SL   12:25   0:00 sbd: inquisitor
root       1929  0.0  0.0  85456 11776 ?        SL   12:25   0:00 sbd: watcher: /dev/disk/by-id/scsi-360014056f268462316e4681b704a9f73 - slot: 0 - uuid: 7b862dba-e7f7-4800-92ed-f76a4e3978c8
root       1930  0.0  0.0  85456 11776 ?        SL   12:25   0:00 sbd: watcher: /dev/disk/by-id/scsi-360014059bc9ea4e4bac4b18808299aaf - slot: 0 - uuid: 5813ee04-b75c-482e-805e-3b1e22ba16cd
root       1931  0.0  0.0  85456 11776 ?        SL   12:25   0:00 sbd: watcher: /dev/disk/by-id/scsi-36001405b8dddd44eb3647908def6621c - slot: 0 - uuid: 986ed8f8-947d-4396-8aec-b933b75e904c
root       1932  0.0  0.0  90524 16656 ?        SL   12:25   0:00 sbd: watcher: Pacemaker
root       1933  0.0  0.0 102708 28260 ?        SL   12:25   0:00 sbd: watcher: Cluster
root      13877  0.0  0.0   9292  1572 pts/0    S+   12:27   0:00 grep sbd

hn1-db-0:~ # kill -9 1912
</code></pre>

Küme düğümü hn1-db-0 yeniden başlatılmalıdır. Pacemaker hizmeti daha sonra başlamayabilir. Tekrar başlattığından emin ol.

### <a name="test-a-manual-failover"></a>Manuel başarısızlamayı test edin

Hn1-db-0 düğümünde `pacemaker` hizmeti durdurarak manuel bir hata üzerinde test edebilirsiniz:

<pre><code>service pacemaker stop
</code></pre>

Başarısız olduktan sonra, hizmeti yeniden başlatabilirsiniz. Ayarlarsanız, `AUTOMATED_REGISTER="false"`hn1-db-0 düğümündeki SAP HANA kaynağı ikincil olarak başlayamaz. Bu durumda, hana örneğini bu komutu çalıştırarak ikincil olarak yapılandırın:

<pre><code>service pacemaker start
su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 

# Switch back to root and clean up the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

### <a name="suse-tests"></a>SUSE testleri

> [!IMPORTANT]
> Seçtiğiniz işletim sistemi kullandığınız belirli VM türleri SAP HANA için SAP sertifikalı olduğundan emin olun. SAP HANA sertifikalı VM türleri ve işletim sistemi sürümleri listesi [SAP HANA Sertifikalı IaaS Platformlarında](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)aranabilir. Belirli VM türü için SAP HANA destekli işletim sistemi sürümlerinin tam listesini almak için listelenen VM türünün ayrıntılarına tıklamayı unutmayın

Kullanım durumunuza bağlı olarak SAP HANA SR Performance Optimize Edilmiş Senaryo veya SAP HANA SR Maliyet Optimize Senaryo kılavuzunda listelenen tüm test örneklerini çalıştırın. SAP en iyi uygulamalar [için SLES][sles-for-sap-bp]kılavuzları bulabilirsiniz.

Aşağıdaki testler SAP Uygulamaları 12 SP1 kılavuzu için SAP HANA SR Performans Optimize Senaryo SUSE Linux Enterprise Server test açıklamaları bir kopyasıdır. Güncel bir sürüm için, her zaman kılavuzun kendisini de okuyun. Teste başlamadan önce HANA'nın senkronize olduğundan ve Kalp Pili yapılandırmasının doğru olduğundan her zaman emin olun.

Aşağıdaki test açıklamalarında PREFER_SITE_TAKEOVER="true" ve AUTOMATED_REGISTER="false" olarak varsayıyoruz.
NOT: Aşağıdaki testler sırayla çalıştırılacak şekilde tasarlanmıştır ve önceki testlerin çıkış durumuna bağlıdır.

1. TEST 1: DÜĞÜM 1'DE BIRINCIL VERITABANıNı DURDUR

   Teste başlamadan önce kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-0
   </code></pre>

   Aşağıdaki komutları hn1-db-0 nod1'de hanasid\>adm <olarak çalıştırın:

   <pre><code>hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> HDB stop
   </code></pre>

   Kalp pili durdurulan HANA örneğini algılamalı ve diğer düğüme geçmelidir. Arıza yapıldıktan sonra hn1-db-0 düğümündeki HANA örneği durdurulur, çünkü Pacemaker düğümü otomatik olarak HANA ikincil olarak kaydetmez.

   Düğüm hn1-db-0'ı ikincil olarak kaydetmek ve başarısız kaynağı temizlemek için aşağıdaki komutları çalıştırın.

   <pre><code>hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-1 --remoteInstance=03 --replicationMode=sync --name=SITE1
   
   # run as root
   hn1-db-0:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-0
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-1
   </code></pre>

1. TEST 2: DÜĞÜM 2'DE BIRINCIL VERITABANıNı DURDUR

   Teste başlamadan önce kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-1
   </code></pre>

   Hn1-db-1 düğümünde hanasid\>adm <aşağıdaki komutları çalıştırın:

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> HDB stop
   </code></pre>

   Kalp pili durdurulan HANA örneğini algılamalı ve diğer düğüme geçmelidir. Arıza yapıldıktan sonra hn1-db-1 düğümündeki HANA örneği durdurulur, çünkü Pacemaker düğümü otomatik olarak HANA ikincil olarak kaydetmez.

   Düğüm hn1-db-1'i ikincil olarak kaydetmek ve başarısız kaynağı temizlemek için aşağıdaki komutları çalıştırın.

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-0 --remoteInstance=03 --replicationMode=sync --name=SITE2
   
   # run as root
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-0
   </code></pre>

1. TEST 3: DÜĞÜM ÜZERINDE BIRINCIL VERITABANıNı KILITLE

   Teste başlamadan önce kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-0
   </code></pre>

   Aşağıdaki komutları hn1-db-0 nod1'de hanasid\>adm <olarak çalıştırın:

   <pre><code>hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> HDB kill-9
   </code></pre>
   
   Kalp pili öldürülen HANA örneğini tespit etmeli ve diğer düğüme geçmelidir. Arıza yapıldıktan sonra hn1-db-0 düğümündeki HANA örneği durdurulur, çünkü Pacemaker düğümü otomatik olarak HANA ikincil olarak kaydetmez.

   Düğüm hn1-db-0'ı ikincil olarak kaydetmek ve başarısız kaynağı temizlemek için aşağıdaki komutları çalıştırın.

   <pre><code>hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-1 --remoteInstance=03 --replicationMode=sync --name=SITE1
   
   # run as root
   hn1-db-0:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-0
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-1
   </code></pre>

1. TEST 4: DÜĞÜM 2'DE BIRINCIL VERITABANıNı KILITLE

   Teste başlamadan önce kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-1
   </code></pre>

   Hn1-db-1 düğümünde hanasid\>adm <aşağıdaki komutları çalıştırın:

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> HDB kill-9
   </code></pre>

   Kalp pili öldürülen HANA örneğini tespit etmeli ve diğer düğüme geçmelidir. Arıza yapıldıktan sonra hn1-db-1 düğümündeki HANA örneği durdurulur, çünkü Pacemaker düğümü otomatik olarak HANA ikincil olarak kaydetmez.

   Düğüm hn1-db-1'i ikincil olarak kaydetmek ve başarısız kaynağı temizlemek için aşağıdaki komutları çalıştırın.

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-0 --remoteInstance=03 --replicationMode=sync --name=SITE2
   
   # run as root
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-0
   </code></pre>

1. TEST 5: KILITLENME BIRINCIL SITE DÜĞÜMÜ (DÜĞÜM 1)

   Teste başlamadan önce kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-0
   </code></pre>

   Aşağıdaki komutları hn1-db-0 düğümünde kök olarak çalıştırın:

   <pre><code>hn1-db-0:~ #  echo 'b' > /proc/sysrq-trigger
   </code></pre>

   Kalp pili öldürülen küme düğümlerini tespit etmeli ve düğümü çitlemelidir. Düğüm çitle çevrildikten sonra, Pacemaker HANA örneğini ele geçirmeyi tetikleyecek. Çitle çevrilmiş düğüm yeniden başlatıldığında, Kalp Pili otomatik olarak başlatılacaktır.

   Pacemaker'ı başlatmak için aşağıdaki komutları çalıştırın, hn1-db-0 düğümü için SBD iletilerini temizleyin, hn1-db-0 düğümlerini ikincil olarak kaydedin ve başarısız kaynağı temizleyin.

   <pre><code># run as root
   # list the SBD device(s)
   hn1-db-0:~ # cat /etc/sysconfig/sbd | grep SBD_DEVICE=
   # SBD_DEVICE="/dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116;/dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1;/dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3"
   
   hn1-db-0:~ # sbd -d /dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116 -d /dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1 -d /dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3 message hn1-db-0 clear
   
   hn1-db-0:~ # systemctl start pacemaker
   
   # run as &lt;hanasid&gt;adm
   hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-1 --remoteInstance=03 --replicationMode=sync --name=SITE1
   
   # run as root
   hn1-db-0:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-0
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-1
   </code></pre>

1. TEST 6: CRASH IKINCIL SITE DÜĞÜMÜ (DÜĞÜM 2)

   Teste başlamadan önce kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-1
   </code></pre>

   Aşağıdaki komutları hn1-db-1 düğümünde kök olarak çalıştırın:

   <pre><code>hn1-db-1:~ #  echo 'b' > /proc/sysrq-trigger
   </code></pre>

   Kalp pili öldürülen küme düğümlerini tespit etmeli ve düğümü çitlemelidir. Düğüm çitle çevrildikten sonra, Pacemaker HANA örneğini ele geçirmeyi tetikleyecek. Çitle çevrilmiş düğüm yeniden başlatıldığında, Kalp Pili otomatik olarak başlatılacaktır.

   Pacemaker'ı başlatmak için aşağıdaki komutları çalıştırın, hn1-db-1 düğümü için SBD iletilerini temizleyin, hn1-db-1 düğümlerini ikincil olarak kaydedin ve başarısız kaynağı temizleyin.

   <pre><code># run as root
   # list the SBD device(s)
   hn1-db-1:~ # cat /etc/sysconfig/sbd | grep SBD_DEVICE=
   # SBD_DEVICE="/dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116;/dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1;/dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3"
   
   hn1-db-1:~ # sbd -d /dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116 -d /dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1 -d /dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3 message hn1-db-1 clear
   
   hn1-db-1:~ # systemctl start pacemaker
   
   # run as &lt;hanasid&gt;adm
   hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-0 --remoteInstance=03 --replicationMode=sync --name=SITE2
   
   # run as root
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-0
   </code></pre>

1. TEST 7: DÜĞÜM 2'DEKI IKINCIL VERITABANıNı DURDURUN

   Teste başlamadan önce kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-0
   </code></pre>

   Hn1-db-1 düğümünde hanasid\>adm <aşağıdaki komutları çalıştırın:

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> HDB stop
   </code></pre>

   Kalp pili durdurulan HANA örneğini algılar ve kaynağı hn1-db-1 düğümünde başarısız olarak işaretler. Kalp pili HANA örneğini otomatik olarak yeniden başlatmalıdır. Başarısız durumu temizlemek için aşağıdaki komutu çalıştırın.

   <pre><code># run as root
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-0
   </code></pre>

1. TEST 8: DÜĞÜM 2'DEKI IKINCIL VERITABANıNı KILITLEYIN

   Teste başlamadan önce kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-0
   </code></pre>

   Hn1-db-1 düğümünde hanasid\>adm <aşağıdaki komutları çalıştırın:

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> HDB kill-9
   </code></pre>

   Kalp pili öldürülen HANA örneğini algılar ve kaynağı hn1-db-1 düğümünde başarısız olarak işaretler. Başarısız durumu temizlemek için aşağıdaki komutu çalıştırın. Kalp pili daha sonra HANA örneğini otomatik olarak yeniden başlatmalıdır.

   <pre><code># run as root
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-0
   </code></pre>

1. TEST 9: CRASH SECONDARY SITE DÜĞÜMÜ (NODE 2) ÇALIŞAN SEKONDER HANA VERİTABANI

   Teste başlamadan önce kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-0
   </code></pre>

   Aşağıdaki komutları hn1-db-1 düğümünde kök olarak çalıştırın:

   <pre><code>hn1-db-1:~ # echo b > /proc/sysrq-trigger
   </code></pre>

   Kalp pili öldürülen küme düğümlerini tespit etmeli ve düğümü çitlemelidir. Çitle çevrilmiş düğüm yeniden başlatıldığında, Kalp Pili otomatik olarak başlatılacaktır.

   Pacemaker'ı başlatmak için aşağıdaki komutları çalıştırın, hn1-db-1 düğümü için SBD iletilerini temizleyin ve başarısız kaynağı temizleyin.

   <pre><code># run as root
   # list the SBD device(s)
   hn1-db-1:~ # cat /etc/sysconfig/sbd | grep SBD_DEVICE=
   # SBD_DEVICE="/dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116;/dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1;/dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3"
   
   hn1-db-1:~ # sbd -d /dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116 -d /dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1 -d /dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3 message hn1-db-1 clear
   
   hn1-db-1:~ # systemctl start pacemaker  
   
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   Testten sonra kaynak durumu:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:azure-lb):      Started hn1-db-0
   </code></pre>

## <a name="next-steps"></a>Sonraki adımlar

* [AZURE Sanal Makineler SAP için planlama ve uygulama][planning-guide]
* [SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]

