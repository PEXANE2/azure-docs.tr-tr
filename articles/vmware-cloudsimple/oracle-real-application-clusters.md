---
title: CloudSimple tarafından Azure VMware Çözümü - Oracle RAC için CloudSimple Private Cloud'unuzu optimize edin
description: Oracle Real Application Clusters (RAC) yüklemesi ve yapılandırması için yeni bir kümenin nasıl dağıtılabildiğini ve vm'yi nasıl optimize edeceğimi açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/06/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: b945beaa7497e1ad19315bacf1284dd0cbc24d6a
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81868070"
---
# <a name="optimize-your-cloudsimple-private-cloud-for-installing-oracle-rac"></a>Oracle RAC'yi yüklemek için CloudSimple Private Cloud'unuzu optimize edin

Oracle Real Application Clusters'ı (RAC) CloudSimple Private Cloud ortamınızda dağıtabilirsiniz. Bu kılavuzda, yeni bir kümenin nasıl dağıtılanış ve Oracle RAC çözümü için bir VM'yi optimize etmek açıklanmaktadır. Bu konudaki adımları tamamladıktan sonra Oracle RAC'yi yükleyebilir ve yapılandırabilirsiniz.

## <a name="storage-policy"></a>Depolama Politikası

Oracle RAC'nin başarılı bir şekilde uygulanması kümede yeterli sayıda düğüm gerektirir.  vSAN depolama ilkesinde, veritabanı, günlük ve redo disklerini depolamak için kullanılan veri disklerine tolere etme hataları (FTT) uygulanır.  Hataları etkili bir şekilde tolere etmek için gerekli düğüm sayısı 2N+1'dir, N FTT'nin değeridir.

Örnek: İstenilen FTT 2 ise, kümedeki toplam düğüm sayısı 2*2+1 = 5 olmalıdır.

## <a name="overview-of-deployment"></a>Dağıtıma genel bakış

Aşağıdaki bölümlerde Oracle RAC için CloudSimple Private Cloud ortamınızın nasıl ayarlır.

1. Disk yapılandırması için en iyi uygulamalar
2. Cloud'u DağıtBasit Özel Bulut vSphere Cluster
3. Oracle RAC için Ağ kurma
4. VSAN depolama ilkelerini kurulum
5. Oracle VM'ler oluşturun ve paylaşılan VM diskleri oluşturun
6. VM'den barındırılaclığa yakınlık kurallarını ayarlama

## <a name="best-practices-for-disk-configuration"></a>Disk yapılandırması için en iyi uygulamalar

Oracle RAC sanal makineleri belirli bir işlev için kullanılan birden çok diske sahiptir.  Paylaşılan diskler, Oracle RAC Cluster tarafından kullanılan tüm sanal makinelere monte edilir.  İşletim sistemi ve yazılım yükleme diskleri yalnızca tek tek sanal makinelere monte edilir.  

![Oracle RAC sanal makine diskleri genel bakış](media/oracle-vm-disks-overview.png)

Aşağıdaki örnekte aşağıdaki tabloda tanımlanan diskler kullanır.

| Disk                                      | Amaç                                       | Paylaşılan Disk |
|-------------------------------------------|-----------------------------------------------|-------------|
| İşletim Sistemi                                        | İşletim sistemi diski                         | Hayır          |
| Kılavuz                                      | Oracle Grid yazılımı için konum yükleme     | Hayır          |
| Veritabanı                                  | Oracle veritabanı yazılımı için konum yükleme | Hayır          |
| ORAHOME                                   | Oracle veritabanı ikilileri için temel konum    | Hayır          |
| DATA1, DATA2, DATA3, DATA4                | Oracle veritabanı dosyalarının depolandığı disk   | Evet         |
| REDO1, REDO2, REDO3, REDO4, REDO5, REDO6  | Günlük disklerini redo                                | Evet         |
| OCR1, OCR2, OCR3, OCR4, OCR5              | Oylama diskleri                                  | Evet         |
| FRA1, FRA2                                | Hızlı kurtarma alanı diskleri                      | Evet         |

![Oracle sanal makine diski yapılandırması](media/oracle-vmdk.png)

### <a name="virtual-machine-configuration"></a>Sanal makine yapılandırması

* Her sanal makine dört SCSI denetleyicisi ile yapılandırılır.
* SCSI denetleyici türü VMware paravirtual olarak ayarlanır.
* Birden çok sanal disk (.vmdk) oluşturulur.
* Diskler farklı SCSI denetleyicilerine monte edilir.
* Paylaşılan küme diskleri için çok yazarlı paylaşım türü ayarlanır.
* vSAN depolama politikası, disklerin yüksek kullanılabilirliğini sağlamak için tanımlanmıştır.

### <a name="operating-system-and-software-disk-configuration"></a>İşletim sistemi ve yazılım diski yapılandırması

Her Oracle sanal makine ana bilgisayar işletim sistemi, takas, yazılım yükleme ve diğer işletim sistemi işlevleri için birden çok disk ile yapılandırılmıştır.  Bu diskler sanal makineler arasında paylaşılmaz.  

* Her sanal makine için üç disk sanal diskler olarak yapılandırılır ve Oracle RAC sanal makinelere monte edilir.
    * İşletim Sistemi Diski
    * Oracle Grid depolamak için disk dosyaları yükler
    * Oracle veritabanı yükleme dosyaları depolamak için Disk
* Diskler **İnce Karşılıksız**olarak yapılandırılabilir.
* Her disk ilk SCSI denetleyicisi (SCSI0) üzerine monte edilir.  
* Paylaşım paylaşım **yok**olarak ayarlanır.
* Artıklık depolamada vSAN ilkeleri kullanılarak tanımlanır.  

![Oracle RAC veri disk grubu yapılandırması](media/oracle-vm-os-disks.png)

### <a name="data-disk-configuration"></a>Veri diski yapılandırması

Veri diskleri öncelikle veritabanı dosyalarını depolamak için kullanılır.  

* Dört disk sanal diskler olarak yapılandırılır ve tüm Oracle RAC sanal makinelerine monte edilir.
* Her disk farklı bir SCSI denetleyicisi üzerine monte edilmiştir.
* Her sanal disk **Kalın Provizyon Eager Zeroed**olarak yapılandırılır.  
* Paylaşım Çok **yazar**olarak ayarlanır.  
* Diskler Otomatik Depolama Yönetimi (ASM) disk grubu olarak yapılandırılmalıdır.  
* Artıklık depolamada vSAN ilkeleri kullanılarak tanımlanır.  
* ASM artıklığı **Dış** artıklık olarak ayarlanır.

![Oracle RAC veri disk grubu yapılandırması](media/oracle-vm-data-disks.png)

### <a name="redo-log-disk-configuration"></a>Günlük disk yapılandırması redo

Redo günlük dosyaları veritabanında yapılan değişikliklerin bir kopyasını depolamak için kullanılır.  Günlük dosyaları, verilerin herhangi bir hatadan sonra kurtarılması gerektiğinde kullanılır.

* Redo günlük diskleri birden çok disk grubu olarak yapılandırılmalıdır.  
* Altı diskoluşturulur ve tüm Oracle RAC sanal makineleri monte.
* Diskler farklı SCSI denetleyicilerine monte edilir
* Her sanal disk **Kalın Provizyon Eager Zeroed**olarak yapılandırılır.
* Paylaşım Çok **yazar**olarak ayarlanır.  
* Diskler iki ASM disk grubu olarak yapılandırılmalıdır.
* Her ASM disk grubu, farklı SCSI denetleyicileri üzerinde bulunan üç disk içerir.  
* ASM artıklığı **Normal** artıklık olarak ayarlanır.
* Her iki ASM Redo günlük grubunda beş redo günlük dosyası oluşturulur

```
SQL > alter database add logfile thread 1 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 1 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 1 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 1 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 1 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 2 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 2 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 2 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 2 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 2 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
```

![Oracle RAC redo günlük disk grubu yapılandırması](media/oracle-vm-redo-log-disks.png)

### <a name="oracle-voting-disk-configuration"></a>Oracle oylama disk yapılandırması

Oylama diskleri, herhangi bir bölünmüş beyin durumunu önlemek için ek bir iletişim kanalı olarak çoğunluk disk işlevselliği sağlar.

* Beş disk oluşturulur ve tüm Oracle RAC sanal makineleri monte.
* Diskler tek bir SCSI denetleyicisi üzerine monte edilir
* Her sanal disk **Kalın Provizyon Eager Zeroed**olarak yapılandırılır.
* Paylaşım Çok **yazar**olarak ayarlanır.  
* Diskler ASM disk grubu olarak yapılandırılmalıdır.  
* ASM artıklığı **Yüksek** artıklık olarak ayarlanır.

![Oracle RAC oylama disk grubu yapılandırması](media/oracle-vm-voting-disks.png)

### <a name="oracle-fast-recovery-area-disk-configuration-optional"></a>Oracle hızlı kurtarma alanı disk yapılandırması (isteğe bağlı)

Hızlı kurtarma alanı (FRA), Oracle ASM disk grubu tarafından yönetilen dosya sistemidir.  FRA yedekleme ve kurtarma dosyaları için paylaşılan bir depolama konumu sağlar. Oracle, hızlı kurtarma alanında arşivlenmiş günlükler ve flashback günlükleri oluşturur. Oracle Recovery Manager (RMAN), yedekleme kümelerini ve görüntü kopyalarını isteğe bağlı olarak hızlı kurtarma alanında saklayabilir ve ortam kurtarma sırasında dosyaları geri alırken kullanır.

* İki disk oluşturulur ve tüm Oracle RAC sanal makineleri monte.
* Diskler farklı SCSI denetleyicisi üzerine monte edilir
* Her sanal disk **Kalın Provizyon Eager Zeroed**olarak yapılandırılır.
* Paylaşım Çok **yazar**olarak ayarlanır.  
* Diskler ASM disk grubu olarak yapılandırılmalıdır.  
* ASM artıklığı **Dış** artıklık olarak ayarlanır.

![Oracle RAC oylama disk grubu yapılandırması](media/oracle-vm-fra-disks.png)

## <a name="deploy-cloudsimple-private-cloud-vsphere-cluster"></a>CloudSimple Private Cloud vSphere kümesini dağıt

Özel Bulut'unuza bir vSphere kümesi dağıtmak için aşağıdaki işlemi izleyin:

1. CloudSimple portalından [özel bulut oluşturun.](create-private-cloud.md) CloudSimple, yeni oluşturulan Özel Bulut'ta 'cloudowner' adlı varsayılan bir vCenter kullanıcısı oluşturur. Varsayılan Özel Bulut kullanıcısı ve izin modeliyle ilgili ayrıntılar [için](learn-private-cloud-permissions.md)bkz.  Bu adım, Özel Bulut'unuzun birincil yönetim kümesini oluşturur.

2. CloudSimple [portalından, Özel Bulut'u](expand-private-cloud.md) yeni bir kümeyle genişletin.  Bu küme Oracle RAC dağıtmak için kullanılacaktır.  İstenilen hata toleransına (en az üç düğüm) göre düğüm sayısını seçin.

## <a name="set-up-networking-for-oracle-rac"></a>Oracle RAC için ağ kurma

1. Özel Bulut'unuzda, biri Oracle ortak ağı için, diğeri Oracle özel ağı için olmak üzere [iki VOL oluşturun](create-vlan-subnet.md)ve uygun alt ağ ID'leri atayın.
2. VLAN'lar oluşturulduktan sonra, [Özel Bulut vCenter'da dağıtılmış bağlantı noktası gruplarını](create-vlan-subnet.md#use-vlan-information-to-set-up-a-distributed-port-group-in-vsphere)oluşturun.
3. Oracle ortamı için yönetim kümenizde bir [DHCP ve DNS sunucusu sanal makinesi](dns-dhcp-setup.md) ayarlayın.
4. Özel [Bulut'ta yüklü olan DNS sunucusunda DNS iletmesini yapılandırın.](on-premises-dns-setup.md#create-a-conditional-forwarder)

## <a name="set-up-vsan-storage-policies"></a>vSAN depolama ilkeleri ayarlama

vSAN ilkeleri, VM disklerde depolanan veriler için tolere etme ve disk şeritleme hatalarını tanımlar.  Oluşturulan depolama ilkesi VM oluşturulurken VM diskler üzerinde uygulanmalıdır.

1. Özel Bulut'unuzun [vSphere istemcisinde oturum açın.](https://docs.microsoft.com/azure/vmware-cloudsimple/vcenter-access)
2. Üst menüden **İlkeler ve Profiller'i**seçin.
3. Sol menüden **VM Depolama İlkeleri'ni** seçin ve ardından **VM depolama İlkesi Oluştur'u**seçin.
4. İlke için anlamlı bir ad girin ve **NEXT'i**tıklatın.
5. **İlke yapısı** bölümünde, **vSAN depolama için kuralları etkinleştir'i** seçin ve **NEXT'i**tıklatın.
6. **vSAN** > **Kullanılabilirlik** bölümünde, Site felaket toleransı için **Yok'u** seçin. Tolere Etmede Başarısız olmak için, istenen FTT için **RAID - Yansıtma** seçeneğini seçin.
    ![vSAN](media/oracle-rac-storage-wizard-vsan.png)ayarları .
7. **Gelişmiş** bölümünde, nesne başına disk şeritlerinin sayısını seçin. Nesne alanı rezervasyonu için **Kalın Karşılıklı'yı**seçin. **Nesne denetimini devre dışı nı**seçin. **NEXT'e**tıklayın.
8. Uyumlu vSAN veri depolarının listesini görüntülemek, ayarları gözden geçirmek ve kurulumu bitirmek için ekrandaki yönergeleri izleyin.

## <a name="create-oracle-vms-and-create-shared-vm-disks-for-oracle"></a>Oracle VM'leri oluşturun ve Oracle için paylaşılan VM diskleri oluşturun

Oracle için bir VM oluşturmak için varolan bir VM'yi klonlaveya yeni bir VM oluşturun.  Bu bölümde, temel işletim sistemini yükledikten sonra ikinci bir VM oluşturmak ve sonra klonlamak için nasıl açıklanır.  VM'ler oluşturulduktan sonra, bunlara bir ek disk oluşturabilirsiniz.  Oracle kümesi depolama, veri, günlükleri ve redo günlükleri için paylaşılan diskler kullanır.

### <a name="create-vms"></a>VM oluşturma

1. vCenter'da, **Ana Bilgisayarlar ve Kümeler** simgesini tıklatın. Oracle için oluşturduğunuz kümeyi seçin.
2. Kümeye sağ tıklayın ve **Yeni Sanal Makine'yi**seçin.
3. **Yeni sanal makine oluştur'u** seçin ve **İleri'yi**tıklatın.
4. Makineyi adlandırın, Oracle VM'nin konumunu seçin ve **İleri'yi**tıklatın.
5. Küme kaynağını seçin ve **İleri'yi**tıklatın.
6. Küme için vSAN veri deposunu seçin ve **İleri'yi**tıklatın.
7. Varsayılan ESXi 6.5 uyumluluk seçimini tutun ve **İleri'yi**tıklatın.
8. Oluşturduğunuz VM için ISO'nun konuk işletim sistemi'ni seçin ve **İleri'yi**tıklatın.
9. İşletim sistemi yüklemek için gereken sabit disk boyutunu seçin.
10. Uygulamayı farklı bir cihaza yüklemek için **yeni aygıt ekle'yi**tıklatın.
11. Ağ seçeneklerini seçin ve ortak ağ için oluşturulan dağıtılmış bağlantı noktası grubunu atayın.
12. Ek ağ arabirimleri eklemek için **yeni aygıt ekle'yi** tıklatın ve özel ağ için oluşturulan dağıtılmış bağlantı noktası grubunu seçin.
13. Yeni DC/DVD Drive için, tercih edilen işletim sistemi kurulumu için ISO içeren datastore ISO dosyasını seçin. Daha önce ISO'lar ve Şablonlar klasörüne yüklediğiniz dosyayı seçin ve **Tamam'ı**tıklatın.
14. Ayarları gözden geçirin ve yeni VM'yi oluşturmak için **Tamam'ı** tıklatın.
15. VM'de güç. İşletim sistemini ve gerekli güncellemeleri yükleme

İşletim sistemi yüklendikten sonra ikinci bir VM klonlayabilirsiniz. VM girişine sağ tıklayın ve klon seçeneğini seçin.

### <a name="create-shared-disks-for-vms"></a>VM'ler için paylaşılan diskler oluşturma

Oracle, günlük dosyalarını depolamak, günlüğe kaydetmek ve yeniden yapmak için paylaşılan diski kullanır.  vCenter'da paylaşılan bir disk oluşturabilir ve her iki VM'ye de monte edebilirsiniz.  Daha yüksek performans için, veri disklerini farklı SCSI denetleyicilerine yerleştirin Aşağıdaki adımlarvCenter'da paylaşılan bir diskin nasıl oluşturulup sanal bir makineye nasıl eklenebildiğini gösterir. vCenter Flash istemcisi VM özelliklerini değiştirmek için kullanılır.

#### <a name="create-disks-on-the-first-vm"></a>İlk VM'de disk oluşturma

1. vCenter'da Oracle VM'lerden birini sağ tıklatın ve **ayarları edin'i**seçin.
2. Yeni aygıt bölümünde **SCSI denetleyicisini** seçin ve **Ekle'yi**tıklatın.
3. Yeni aygıt bölümünde Yeni **Sabit disk'i** seçin ve **Ekle'yi**tıklatın.
4. Yeni Sabit diskin özelliklerini genişletin.
5. Sabit diskin boyutunu belirtin.
6. VM depolama ilkesini daha önce tanımladığınız vSAN depolama ilkesi olarak belirtin.
7. vSAN datastore'da konumu klasör olarak seçin. Konum, disklere göz atma ve ikinci bir VM'ye takılmaya yardımcı olur.
8. Disk sağlama için, **Kalın hükmü eager zeroed**seçin.
9. Paylaşım için **Çok yazarlı'yı belirtin.**
10. Sanal aygıt düğümü için, adım 2'de oluşturulan yeni SCSI denetleyicisini seçin.

    ![İlk VM'de disk oluşturma](media/oracle-rac-new-hard-disk.png)

Oracle verileri, günlükler ve redo günlük dosyaları için gereken tüm yeni diskler için 2 - 10 adımlarını yineleyin.

#### <a name="attach-disks-to-second-vm"></a>Diskleri ikinci VM'ye ekleme

1. vCenter'da Oracle VM'lerden birini sağ tıklatın ve **ayarları edin'i**seçin.
2. Yeni aygıt bölümünde **SCSI denetleyicisini** seçin ve **Ekle'yi**tıklatın.
3. Yeni aygıt bölümünde, **Varolan Sabit diski** seçin ve **Ekle'yi**tıklatın.
4. İlk VM için diskin oluşturulduğu konuma göz atın ve VMDK dosyasını seçin.
5. VM depolama ilkesini daha önce tanımladığınız vSAN depolama ilkesi olarak belirtin.
6. Disk sağlama için, **Kalın hükmü eager zeroed**seçin.
7. Paylaşım için **Çok yazarlı'yı belirtin.**
8. Sanal aygıt düğümü için, adım 2'de oluşturulan yeni SCSI denetleyicisini seçin.

    ![İlk VM'de disk oluşturma](media/oracle-rac-existing-hard-disk.png)

Oracle verileri, günlükler ve redo günlük dosyaları için gereken tüm yeni diskler için 2 - 7 adımlarını yineleyin.

## <a name="set-up-vm-host-affinity-rules"></a>VM ana bilgisayar afisite kurallarını ayarlama

VM'den barındırılabilen yakınlık kuralları, VM'nin istenilen ana bilgisayarda çalıştığından emin olun.  Oracle VM'nin yeterli kaynaklarla ana bilgisayarda çalıştığından emin olmak ve belirli lisans gereksinimlerini karşılamak için vCenter'da kurallar tanımlayabilirsiniz.

1. CloudSimple portalında bulut sahibi kullanıcının [ayrıcalıklarını artırın.](escalate-private-cloud-privileges.md)
2. Özel Bulut'unuzun [vSphere istemcisinde oturum açın.](https://docs.azure.cloudsimple.com/vsphere-access)
3. vSphere istemcisinde, Oracle VM'lerin dağıtıldığı kümeyi seçin ve **Yapılandır'ı**tıklatın.
4. Yapılandırma altında **VM/Host Grupları'nı**seçin.
5. 'yi tıklatın. **+**
6. Bir VM grubu ekleyin. Tür olarak **VM grubunu** seçin. Grubun adını girin. VM'leri seçin ve ardından grubu oluşturmak için **Tamam'ı** tıklatın.
6. Ana bilgisayar grubu ekleyin. Tür olarak **Ana Bilgisayar Grubu'nu** seçin. Grubun adını girin. VM'lerin çalışacağı ana bilgisayarları seçin ve grubu oluşturmak için **Tamam'ı** tıklatın.
7. Bir kural oluşturmak için **VM/Host kurallarını**tıklatın.
8. 'yi tıklatın. **+**
9. Kural için bir ad girin ve **Etkinleştir'i**denetleyin.
10. Kural türü için, **Barındırış'a Sanal Makineler'i**seçin.
11. Oracle VM'leri içeren VM grubunu seçin.
12. **Bu gruptaki ana bilgisayarlarda çalıştırıl malı'yı**seçin.
13. Oluşturduğunuz ana bilgisayar grubunu seçin.
14. Kuralı oluşturmak için **Tamam**'a tıklayın.

## <a name="references"></a>Başvurular

* [vSAN Politikaları Hakkında](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.virtualsan.doc/GUID-08911FD3-2462-4C1C-AE81-0D4DBC8F7990.html)
* [Paylaşılan VMDK'ler için VMware Çok Yazaröz Özniteliği](https://docs.vmware.com/en/VMware-Cloud-on-AWS/solutions/VMware-Cloud-on-AWS.df6735f8b729fee463802083d46fdc75/GUID-A7642A82B3D6C5F7806DB40A3F2766D9.html)
