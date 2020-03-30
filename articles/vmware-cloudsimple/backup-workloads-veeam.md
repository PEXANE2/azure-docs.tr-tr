---
title: CloudSimple tarafından Azure VMware Çözümü - Veeam kullanarak Özel Bulut'ta iş yükü sanal makineleri yedekleme
description: Veeam B&R 9.5 kullanarak Azure tabanlı CloudSimple Private Cloud'da çalışan sanal makinelerinizi nasıl yedeklediğinizi açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 3262841efb9109b1de24fe501ea0a7bea0dd612d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025138"
---
# <a name="back-up-workload-vms-on-cloudsimple-private-cloud-using-veeam-br"></a>Veeam B&R kullanarak CloudSimple Private Cloud'da iş yükü VM'lerini yedekleme

Bu kılavuzda, Veeam B&R 9.5 kullanarak Azure tabanlı CloudSimple Private Cloud'da çalışan sanal makinelerinizi nasıl yedekleyebilirsiniz.

## <a name="about-the-veeam-back-up-and-recovery-solution"></a>Veeam yedekleme ve kurtarma çözümü hakkında

Veeam çözümü aşağıdaki bileşenleri içerir.

**Yedek Sunucu**

Yedekleme sunucusu, Veeam için denetim merkezi olarak hizmet veren ve bu işlevleri gerçekleştiren bir Windows sunucusudur (VM) 

* Yedekleme, çoğaltma, kurtarma doğrulama ve görevleri geri yükleme yi koordine eder
* İş zamanlamasını ve kaynak ayırmayı denetler
* Yedekleme altyapısı bileşenlerini ayarlamanızı ve yönetmeniz ve yedekleme altyapısı için genel ayarları belirtmenizi sağlar

**Proxy Sunucuları**

Proxy sunucuları yedek sunucu ve yedekleme altyapısının diğer bileşenleri arasında yüklenir. Aşağıdaki işlevleri yönetirler:

* VM verilerinin üretim depolamasından alınması
* Sıkıştırma
* Yinelenenleri kaldırma
* Şifreleme
* Verilerin yedekleme deposuna aktarılması

**Yedek depo**

Yedekleme deposu, Veeam'in çoğaltılan VM'ler için yedekleme dosyalarını, VM kopyalarını ve meta verileri sakladığı depolama konumudur.  Depo, yerel disklere (veya nfs/SMB'ye monte edilmiş) bir Windows veya Linux sunucusu veya donanım depolama çoğaltma cihazı olabilir.

### <a name="veeam-deployment-scenarios"></a>Veeam dağıtım senaryoları
Uzun süreli yedekleme ve arşivleme için bir yedekleme deposu ve depolama hedefi sağlamak için Azure'dan yararlanabilirsiniz. Özel Bulut'taki VM'ler ile Azure'daki yedekleme deposu arasındaki tüm yedekleme ağı trafiği, yüksek bant genişliği, düşük gecikme sonu bağlantısı üzerinden hareket eder. Bölgeler arasındaki çoğaltma trafiği, kullanıcılar için bant genişliği maliyetlerini düşüren dahili Azure arka düzlem ağı üzerinden gider.

**Temel dağıtım**

Yedeklemek için 30 TB'den az olan ortamlar için CloudSimple aşağıdaki yapılandırmayı önerir:

* Veeam yedekleme sunucusu ve proxy sunucusu Özel Bulut'ta aynı VM'de yüklü.
* Azure'da linux tabanlı birincil yedekleme deposu yedekleme işleri için hedef olarak yapılandırıldı.
* `azcopy`birincil yedekleme deposundaki verileri başka bir bölgeye çoğaltılan bir Azure blob kapsayıcısına kopyalamak için kullanılır.

![Temel dağıtım senaryoları](media/veeam-basicdeployment.png)

**Gelişmiş dağıtım**

Yedeklemek için 30'dan fazla TB'si olan ortamlar için CloudSimple aşağıdaki yapılandırmayı önerir:

* Veeam tarafından önerilen vSAN kümesinde düğüm başına bir proxy sunucusu.
* Hızlı geri yüklemeler için beş günlük verileri önbelleğe almak için Özel Bulut'ta Windows tabanlı birincil yedekleme deposu.
* Daha uzun süre bekletme için yedekleme kopyalama işleri için hedef olarak Azure'daki Linux yedekleme deposu. Bu depo, ölçeklendirme yedek deposu olarak yapılandırılmalıdır.
* `azcopy`birincil yedekleme deposundaki verileri başka bir bölgeye çoğaltılan bir Azure blob kapsayıcısına kopyalamak için kullanılır.

![Temel dağıtım senaryoları](media/veeam-advanceddeployment.png)

Önceki şekilde, yedekleme proxy vSAN veri deposunda iş yükü VM diskleri Hot Add erişimi ile bir VM olduğunu unutmayın. Veeam vSAN için Sanal Cihaz yedek proxy taşıma modunu kullanır.

## <a name="requirements-for-veeam-solution-on-cloudsimple"></a>CloudSimple'da Veeam çözümü için gereksinimler

Veeam çözümü aşağıdakileri yapmanızgerektiğini gerektirir:

* Kendi Veeam lisanslarınızı sağlayın.
* CloudSimple Private Cloud'da çalışan iş yüklerini yedeklemek için Veeam'ı dağıtın ve yönetin.

Bu çözüm, Veeam yedekleme aracı üzerinde tam kontrol sağlar ve VM yedekleme işlerini yönetmek için yerel Veeam arabirimini veya Veeam vCenter eklentisini kullanma seçeneği sunar.

Varolan bir Veeam kullanıcısıysanız, Veeam Çözüm Bileşenleri'ndeki bölümü atlayabilir ve doğrudan [Veeam Dağıtım Senaryolarına](#veeam-deployment-scenarios)geçebilirsiniz.

## <a name="install-and-configure-veeam-backups-in-your-cloudsimple-private-cloud"></a>CloudSimple Private Cloud'unuzda Veeam yedeklemelerini yükleyin ve yapılandırır

Aşağıdaki bölümler, CloudSimple Private Cloud'unuz için bir Veeam yedekleme çözümünüzü nasıl yükleyip yapılandırılabildiğini açıklar.

Dağıtım işlemi aşağıdaki adımlardan oluşur:

1. [vCenter UI: Özel Bulut'unuzda altyapı hizmetleri ayarlama](#vcenter-ui-set-up-infrastructure-services-in-your-private-cloud)
2. [CloudSimple portalı: Veeam için Özel Bulut ağı kurma](#cloudsimple-private-cloud-set-up-private-cloud-networking-for-veeam)
3. [CloudSimple portalı: Ayrıcalıkları Artırın](#cloudsimple-private-cloud-escalate-privileges-for-cloudowner)
4. [Azure portalı: Sanal ağınızı Özel Bulut'a bağlayın](#azure-portal-connect-your-virtual-network-to-the-private-cloud)
5. [Azure portalı: Azure'da yedekleme deposu oluşturma](#azure-portal-connect-your-virtual-network-to-the-private-cloud)
6. [Azure portalı: Uzun vadeli veri saklama için Azure blob depolamayı yapılandırın](#configure-azure-blob-storage-for-long-term-data-retention)
7. [özel bulut vCenter UI: Veeam B&R yükleyin](#vcenter-console-of-private-cloud-install-veeam-br)
8. [Veeam Console: Veeam Yedekleme & Kurtarma yazılımı yapılandırın](#veeam-console-install-veeam-backup-and-recovery-software)
9. [CloudSimple portalı: Veeam erişimi ayarlayın ve ayrıcalıkları artırın](#cloudsimple-portal-set-up-veeam-access-and-de-escalate-privileges)

### <a name="before-you-begin"></a>Başlamadan önce

Veeam dağıtımına başlamadan önce aşağıdakiler gereklidir:

* Size ait bir Azure aboneliği
* Önceden oluşturulmuş bir Azure kaynak grubu
* Aboneliğinizde bir Azure sanal ağı
* Azure depolama hesabı
* CloudSimple portalı kullanılarak oluşturulan [Özel Bulut.](create-private-cloud.md)  

Uygulama aşamasında aşağıdaki öğeler gereklidir:

* Veeam'i yüklemek için Windows için VMware şablonları (Windows Server 2012 R2 - 64 bit görüntü gibi)
* Yedekleme ağı için tanımlanmış kullanılabilir bir VLAN
* Yedekleme ağına atanacak alt ağın CIDR'si
* Veeam 9.5 u3 yüklenebilir medya (ISO) Özel Bulut vSAN veri deposuna yüklendi

### <a name="vcenter-ui-set-up-infrastructure-services-in-your-private-cloud"></a>vCenter UI: Özel Bulut'unuzda altyapı hizmetleri ayarlama

İş yüklerinizi ve araçlarınızı yönetmeyi kolaylaştırmak için Özel Bulut'taki altyapı hizmetlerini yapılandırın.

* Aşağıdakilerden herhangi biri geçerliyse [Active Directory'yi kullanmak için vCenter kimlik kaynaklarını](set-vcenter-identity.md) ayarla'da açıklandığı gibi harici bir kimlik sağlayıcısı ekleyebilirsiniz:

  * Özel Bulut'unuzda şirket içi Active Directory (AD) kullanıcıları tanımlamak istiyorsunuz.
  * Tüm kullanıcılar için Özel Bulut'unuzda bir REKLAM kurmak istiyorsunuz.
  * Azure AD'yi kullanmak istiyorsunuz.
* Özel Bulut'taki iş yüklerinizle IP adresi araması, IP adresi yönetimi ve ad çözümleme hizmetleri sağlamak için, [CloudSimple Private Cloud'unuzda DNS ve DHCP uygulamalarını ve iş yüklerini](dns-dhcp-setup.md)ayarla'da açıklandığı şekilde bir DHCP ve DNS sunucusu ayarlayın.

### <a name="cloudsimple-private-cloud-set-up-private-cloud-networking-for-veeam"></a>CloudSimple Private Cloud: Veeam için Özel Bulut ağı kurma

Veeam çözümü için Özel Bulut ağı kurmak için CloudSimple portalına erişin.

Yedekleme ağı için bir VLAN oluşturun ve bir alt ağ CIDR atayın. Talimatlar için [VLANs/Subnets oluştur ve yönetme'ye](create-vlan-subnet.md)bakın.

Veeam tarafından kullanılan bağlantı noktalarında ağ trafiğine izin vermek için yönetim alt ağı ile yedekleme ağı arasında güvenlik duvarı kuralları oluşturun. [Veeam konusuna](https://helpcenter.veeam.com/docs/backup/vsphere/used_ports.html?ver=95)bakın Kullanılan Bağlantı Noktaları . Güvenlik duvarı kuralı oluşturma yla ilgili yönergeler [için](firewall.md)bkz.

Aşağıdaki tablo bir bağlantı noktası listesi sağlar.

| Simge | Açıklama | Simge | Açıklama |
| ------------ | ------------- | ------------ | ------------- |
| Yedek Sunucu  | vCenter  | HTTPS / TCP  | 443 |
| Yedek Sunucu <br> *Veeam Yedekleme & Çoğaltma bileşenlerini dağıtmak için gereklidir* | Yedek Proxy  | TCP/UDP  | 135, 137 ila 139 ve 445 |
    | Yedek Sunucu   | DNS  | UDP  | 53  | 
    | Yedek Sunucu   | Veeam Güncelleme Bildirim Sunucusu  | TCP  | 80  | 
    | Yedek Sunucu   | Veeam Lisans Güncelleme Server  | TCP  | 443  | 
    | Yedek Proxy   | vCenter |   |   | 
    | Yedek Proxy  | Linux Yedekleme Deposu   | TCP  | 22  | 
    | Yedek Proxy  | Windows Yedekleme Deposu  | TCP  | 49152 - 65535   | 
    | Yedek Depo  | Yedek Proxy  | TCP  | 2500 -5000  | 
    | Kaynak Yedekleme Deposu<br> *Yedekleme kopyalama işleri için kullanılır*  | Hedef Yedekleme Deposu  | TCP  | 2500 - 5000  | 

[Güvenlik duvarı tablolarını ve kurallarını ayarla'da](firewall.md)açıklandığı gibi iş yükü alt ağı ile yedekleme ağı arasında güvenlik duvarı kuralları oluşturun.  Uygulama farkında yedekleme ve geri yükleme için, belirli uygulamaları barındıran iş yükü VM'lerinde [ek bağlantı noktaları](https://helpcenter.veeam.com/docs/backup/vsphere/used_ports.html?ver=95) nın açılması gerekir.

Varsayılan olarak, CloudSimple 1Gbps ExpressRoute bağlantısı sağlar. Daha büyük ortam boyutları için daha yüksek bir bant genişliği bağlantısı gerekebilir. Daha yüksek bant genişliği bağlantıları hakkında daha fazla bilgi için Azure desteğine başvurun.

Kuruluma devam etmek için yetkilendirme anahtarı ve eş devresi URI'ye ve Azure Aboneliğinize erişmeniz gerekir.  Bu bilgiler CloudSimple portalındaki Sanal Ağ Bağlantısı sayfasında mevcuttur. Talimatlar için bkz. Azure [sanal ağı için CloudSimple bağlantısıiçin eşleme bilgileri edinin.](virtual-network-connection.md) Bilgileri almakta sorun yaşıyorsanız, [desteğe başvurun.](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

### <a name="cloudsimple-private-cloud-escalate-privileges-for-cloudowner"></a>CloudSimple Private Cloud: Bulut sahibi için ayrıcalıkları artırın

Varsayılan 'bulut sahibi' kullanıcısı, VEEAM'ı yüklemek için Özel Bulut vCenter'da yeterli ayrıcalıka sahip değildir, bu nedenle kullanıcının vCenter ayrıcalıklarının yükseltilmesi gerekir. Daha fazla bilgi için [bkz.](escalate-private-cloud-privileges.md)

### <a name="azure-portal-connect-your-virtual-network-to-the-private-cloud"></a>Azure portalı: Sanal ağınızı Özel Bulut'a bağlayın

[ExpressRoute'u kullanarak Azure Sanal Ağ Bağlantısı'ndaki](azure-expressroute-connection.md)yönergeleri izleyerek sanal ağınızı Özel Bulut'a bağlayın.

### <a name="azure-portal-create-a-backup-repository-vm"></a>Azure portalı: Yedek depo VM oluşturma

1. (2 vCPUs ve 8 GB bellek) ile standart bir D2 v3 VM oluşturun.
2. CentOS 7.4 tabanlı görüntüyü seçin.
3. VM için bir ağ güvenlik grubu (NSG) yapılandırın. VM'nin genel bir IP adresi olmadığını ve herkese açık internetten erişilemediğini doğrulayın.
4. Yeni VM için kullanıcı adı ve parola tabanlı kullanıcı hesabı oluşturun. Talimatlar için bkz. [Azure portalında bir Linux sanal makinesi oluşturun.](../virtual-machines/linux/quick-create-portal.md)
5. 1x512 GiB standart HDD oluşturun ve depo VM'ye takın.  Yönergeler için, [Azure portalındaki bir Windows VM'ye yönetilen bir veri diskinin nasıl ekildiğini](../virtual-machines/windows/attach-managed-disk-portal.md)görün.
6. [Yönetilen diskte bir XFS birimi oluşturun.](https://www.digitalocean.com/docs/volumes/how-to/) Daha önce belirtilen kimlik bilgilerini kullanarak VM'de oturum açın. Mantıksal bir birim oluşturmak, diski ona eklemek, Bir XFS dosya sistemi [bölümü](https://www.digitalocean.com/docs/volumes/how-to/partition/) oluşturmak ve /backup1 yolunun altındaki bölümü [monte](https://www.digitalocean.com/docs/volumes/how-to/mount/) etmek için aşağıdaki komut dosyasını çalıştırın.

    Örnek betik:

    ```
    sudo pvcreate /dev/sdc
    sudo vgcreate backup1 /dev/sdc
    sudo lvcreate -n backup1 -l 100%FREE backup1
    sudo mkdir -p /backup1
    sudo chown veeamadmin /backup1
    sudo chmod 775 /backup1
    sudo mkfs.xfs -d su=64k -d sw=1 -f /dev/mapper/backup1-backup1
    sudo mount -t xfs /dev/mapper/backup1-backup1 /backup1
    ```

7. /backup1'i Özel Bulut'ta çalışan Veeam yedekleme sunucusuna NFS montaj noktası olarak gösterin. Talimatlar için, Digital Ocean makalesine bakın [CentOS 6'da NFS Mount Nasıl Kurululur?](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-centos-6) Veeam yedekleme sunucusundaki yedekleme deposunu yapılandırırken bu NFS paylaşım adını kullanın.

8. Yedekleme deposu VM için NSG'deki filtreleme kurallarını, VM'ye gelen ve VM'den gelen tüm ağ trafiğine açıkça izin verecek şekilde yapılandırın.

> [!NOTE]
> Veeam Backup & Replication, Linux yedekleme depolarıyla iletişim kurmak için SSH protokolünü kullanır ve Linux depolarında SCP yardımcı programını gerektirir. SSH daemon'un düzgün şekilde yapılandırıldığından ve SCP'nin Linux ana bilgisayarda kullanılabildiğini doğrulayın.

### <a name="configure-azure-blob-storage-for-long-term-data-retention"></a>Uzun vadeli veri saklama için Azure blob depolamayı yapılandırma

1. Microsoft video [Azure Depolama ile Başlarken](https://azure.microsoft.com/resources/videos/get-started-with-azure-storage)açıklandığı gibi standart türünde genel amaçlı depolama hesabı (GPv2) ve bir blob kapsayıcı oluşturun.
2. Kapsayıcı Oluştur başvurusunda açıklandığı gibi bir Azure depolama kapsayıcısı [oluşturun.](https://docs.microsoft.com/rest/api/storageservices/create-container)
2. Microsoft'tan `azcopy` Linux için komut satırı yardımcı programını indirin. CentOS 7.5'teki bash kabuğunda aşağıdaki komutları kullanabilirsiniz.

    ```
    wget -O azcopy.tar.gz https://aka.ms/downloadazcopylinux64
    tar -xf azcopy.tar.gz
    sudo ./install.sh
    sudo yum -y install libunwind.x86_64
    sudo yum -y install icu
    ```

3. Yedek `azcopy` dosyaları blob kapsayıcısına kopyalamak için komutu kullanın.  Ayrıntılı komutlar için [Linux'ta AzCopy ile veri](../storage/common/storage-use-azcopy-linux.md) aktarımı'na bakın.

### <a name="vcenter-console-of-private-cloud-install-veeam-br"></a>özel bulut vCenter konsolu: Veeam B&R yükleyin

Bir Veeam hizmet hesabı oluşturmak, Veeam B&R 9.5'i yüklemek ve hizmet hesabını kullanarak Veeam'i yapılandırmak için Özel Bulut'unuzdan vCenter'a erişin.

1. 'Veeam Yedekleme Rolü' adlı yeni bir rol oluşturun ve Veeam tarafından önerilen gerekli izinleri atayın. Ayrıntılar için Veeam konusu [Gerekli İzinler](https://helpcenter.veeam.com/docs/backup/vsphere/required_permissions.html?ver=95)bakın.
2. vCenter'da yeni bir 'Veeam Kullanıcı Grubu' grubu oluşturun ve ona 'Veeam Yedekleme Rolü' atayın.
3. Yeni bir 'Veeam Hizmet Hesabı' kullanıcısı oluşturun ve 'Veeam Kullanıcı Grubu'na ekleyin.

    ![Veeam hizmet hesabı oluşturma](media/veeam-vcenter01.png)

4. Yedekleme ağı VLAN'ı kullanarak vCenter'da dağıtılmış bir bağlantı noktası grubu oluşturun. Ayrıntılar için [vSphere Web İstemci'sinde Dağıtılmış Bağlantı Noktası Grubu Oluşturma](https://www.youtube.com/watch?v=wpCd5ZbPOpA)VMware videosunu görüntüleyin.
5. [Veeam sistem gereksinimlerine](https://helpcenter.veeam.com/docs/backup/vsphere/system_requirements.html?ver=95)göre vCenter'da Veeam yedekleme ve proxy sunucuları için VM'ler oluşturun. Windows 2012 R2 veya Linux kullanabilirsiniz. Daha fazla bilgi [için Linux yedekleme depolarını kullanma gereksinimlerine](https://www.veeam.com/kb2216)bakın.
6. Yüklenebilir Veeam ISO'yu Veeam yedekleme sunucusu VM'de CDROM aygıtı olarak monte edin.
7. Windows 2012 R2 makinesine (Veeam yüklemesinin hedefi) bir RDP oturumu kullanarak, Windows 2012 R2 VM'de [Veeam B&R 9.5u3'ü yükleyin.](https://helpcenter.veeam.com/docs/backup/vsphere/install_vbr.html?ver=95)
8. Veeam yedekleme sunucusu VM'nin dahili IP adresini bulun ve IP adresini DHCP sunucusunda statik olacak şekilde yapılandırın. Bunu yapmak için gereken tam adımlar DHCP sunucusuna bağlıdır. Örnek olarak, Netgate makalesi <a href="https://www.netgate.com/docs/pfsense/dhcp/dhcp-server.html" target="_blank">statik DHCP eşlemeleri,</a> bir DHCP sunucusunun bir pfSense yönlendiricisi kullanarak nasıl yapılandırılabildiğini açıklar.

### <a name="veeam-console-install-veeam-backup-and-recovery-software"></a>Veeam konsolu: Veeam yedekleme ve kurtarma yazılımı yükleyin

Veeam konsolu kullanarak Veeam yedekleme ve kurtarma yazılımını yapılandırın. Ayrıntılar için, [Veeam Yedekleme & Çoğaltma v9 - Yükleme ve Dağıtım'a](https://www.youtube.com/watch?v=b4BqC_WXARk)bakın.

1. VMware vSphere'i yönetilen bir sunucu ortamı olarak ekleyin. İstendiğinde, özel bulutvCenter Console başında oluşturduğunuz Veeam Hizmet Hesabı'nın kimlik bilgilerini [sağlayın: Veeam B&R'yi yükleyin.](#vcenter-console-of-private-cloud-install-veeam-br)

    * Yük denetimi ve varsayılan gelişmiş ayarlar için varsayılan ayarları kullanın.
    * Montaj sunucusu konumunu yedek sunucu olarak ayarlayın.
    * Veeam sunucusunun yapılandırma yedekleme konumunu uzak depoyla değiştirin.

2. Yedekleme deposu olarak Azure'daki Linux sunucusunu ekleyin.

    * Yük denetimi ve gelişmiş ayarlar için varsayılan ayarları kullanın. 
    * Montaj sunucusu konumunu yedek sunucu olarak ayarlayın.
    * Veeam sunucusunun yapılandırma yedekleme konumunu uzak depoyla değiştirin.

3. Ana Sayfa> **Yapılandırma Yedekleme Ayarlarını**kullanarak yapılandırma yedeklemesinin şifrelemesini etkinleştirin.

4. VMware ortamı için proxy sunucusu olarak bir Windows sunucusu VM ekleyin. Proxy için 'Trafik Kuralları'nı kullanarak, yedek verileri kablo üzerinden şifreleyin.

5. Yedekleme işlerini yapılandırın.
    * Yedekleme işlerini yapılandırmak için [Yedek İş Oluşturma](https://www.youtube.com/watch?v=YHxcUFEss4M)yönergelerini izleyin.
    * Gelişmiş Ayarlar > **Depolama**altında yedekleme dosyalarının şifrelemesini etkinleştirin.

6. Yedekleme kopyalama işlerini yapılandırın.

    * Yedekleme kopyalama işlerini yapılandırmak için, [Yedek Kopya İşoluşturma](https://www.youtube.com/watch?v=LvEHV0_WDWI&t=2s)videosundaki yönergeleri izleyin.
    * Gelişmiş Ayarlar > **Depolama**altında yedekleme dosyalarının şifrelemesini etkinleştirin.

### <a name="cloudsimple-portal-set-up-veeam-access-and-de-escalate-privileges"></a>CloudSimple portalı: Veeam erişimi ayarlayın ve ayrıcalıkları artırın
Veeam yedekleme ve kurtarma sunucusu için genel bir IP adresi oluşturun. Yönergeler için [bkz.](public-ips.md)

Veeam yedekleme sunucusunun TCP bağlantı noktası 80'de güncelleştirmeleri/düzeltme ekileri indirmek için Veeam web sitesine giden bir bağlantı oluşturmasına izin vermek için bir güvenlik duvarı kuralı oluşturun. Yönergeler için [bkz.](firewall.md)

Ayrıcalıkları daha da arttırmak için, [ayrıcalıkları yükseltmebölümüne](escalate-private-cloud-privileges.md#de-escalate-privileges)bakın.

## <a name="references"></a>Başvurular

### <a name="cloudsimple-references"></a>CloudBasit referanslar

* [Özel Bulut Oluşturma](create-private-cloud.md)
* [V'ler/Alt Ağlar oluşturma ve yönetme](create-vlan-subnet.md)
* [vCenter Kimlik Kaynakları](set-vcenter-identity.md)
* [İş Yükü DNS ve DHCP Kurulumu](dns-dhcp-setup.md)
* [Ayrıcalıkları yükseltme](escalate-privileges.md)
* [Güvenlik duvarı tablolarını ve kurallarını ayarlama](firewall.md)
* [Özel Bulut izinleri](learn-private-cloud-permissions.md)
* [Ortak IP Adreslerini Ayırma](public-ips.md)

### <a name="veeam-references"></a>Veeam Referanslar

* [İkinci El Bağlantı Noktaları](https://helpcenter.veeam.com/docs/backup/vsphere/used_ports.html?ver=95)
* [Gerekli İzinler](https://helpcenter.veeam.com/docs/backup/vsphere/required_permissions.html?ver=95)
* [Sistem Gereksinimleri](https://helpcenter.veeam.com/docs/backup/vsphere/system_requirements.html?ver=95)
* [Veeam Yedekleme & Çoğaltma Yükleme](https://helpcenter.veeam.com/docs/backup/vsphere/install_vbr.html?ver=95)
* [Linux için Multi-OS FLR ve Depo desteği için gerekli modüller ve izinler](https://www.veeam.com/kb2216)
* [Veeam Yedekleme & Çoğaltma v9 - Kurulum ve Dağıtım - Video](https://www.youtube.com/watch?v=b4BqC_WXARk)
* [Veeam v9 Yedek İş Oluşturma - Video](https://www.youtube.com/watch?v=YHxcUFEss4M)
* [Veeam v9 Yedekleme Kopyalama İş oluşturma - Video](https://www.youtube.com/watch?v=LvEHV0_WDWI&t=2s)

### <a name="azure-references"></a>Azure başvuruları

* [Azure portalını kullanarak ExpressRoute için sanal ağ ağ geçidini yapılandırma](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md)
* [Bir Devre için bir VNet bağlayın - farklı abonelik](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md#connect-a-vnet-to-a-circuit---different-subscription)
* [Azure portalında bir Linux sanal makinesi oluşturma](../virtual-machines/linux/quick-create-portal.md)
* [Azure portalındaki bir Windows VM'ye yönetilen bir veri diski ekleme](../virtual-machines/windows/attach-managed-disk-portal.md)
* [Azure Depolama ile Başlarken - Video](https://azure.microsoft.com/resources/videos/get-started-with-azure-storage)
* [Kapsayıcı Oluşturma](https://docs.microsoft.com/rest/api/storageservices/create-container)
* [Linux üzerinde AzCopy ile veri aktarma](../storage/common/storage-use-azcopy-linux.md)

### <a name="vmware-references"></a>VMware referansları

* [vSphere Web İstemcisinde Dağıtılmış Bağlantı Noktası Grubu Oluşturma - Video](https://www.youtube.com/watch?v=wpCd5ZbPOpA)

### <a name="other-references"></a>Diğer referanslar

* [Yönetilen diskte Bir XFS hacmi oluşturma - RedHat](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/storage_administration_guide/ch-xfs)
* [CentOS 7'de NFS Dağı Nasıl Kurululur - HowToForge](https://www.howtoforge.com/nfs-server-and-client-on-centos-7)
* [DHCP Sunucusunun Yapılandırılması - Netgate](https://www.netgate.com/docs/pfsense/dhcp/dhcp-server.html)
