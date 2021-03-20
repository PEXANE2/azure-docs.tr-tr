---
title: Azure Site Recovery için Azure VMware çözümünü olağanüstü durum kurtarma için hazırlama
description: Azure VMware Çözüm sunucularını Azure Site Recovery hizmetini kullanarak Azure 'a olağanüstü durum kurtarma için nasıl hazırlayacağınızı öğrenin.
author: Harsha-CS
manager: rochakm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/29/2020
ms.author: harshacs
ms.custom: MVC
ms.openlocfilehash: 8e77ede7b04c95bfd6b6b8f660c8d811e7434c0f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93395453"
---
# <a name="prepare-azure-vmware-solution-for-disaster-recovery-to-azure-site-recovery"></a>Azure Site Recovery için Azure VMware çözümünü olağanüstü durum kurtarma için hazırlama

Bu makalede, Azure VMware Çözüm sunucularının [Azure Site Recovery](site-recovery-overview.md) Hizmetleri kullanılarak Azure 'a olağanüstü durum kurtarma için nasıl hazırlanacağı açıklanmaktadır. 

Bu, Azure VMware Çözüm VM 'Leri için Azure 'da olağanüstü durum kurtarmanın nasıl ayarlanacağını gösteren bir serideki ikinci öğreticidir. İlk öğreticide, Azure VMware çözümü olağanüstü durum kurtarma için gereken [Azure bileşenlerini ayarladık](avs-tutorial-prepare-azure.md) .


Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * VM bulmayı otomatikleştirmek için vCenter sunucusunda veya vSphere ESXi konağında bir hesap hazırlayın.
> * VMware VM 'lerinde Mobility hizmetinin otomatik olarak yüklenmesine yönelik bir hesap hazırlayın.
> * VMware sunucusu ve VM gereksinimlerini ve desteğini gözden geçirin.
> * Yük devretmeden sonra Azure VM 'lerine bağlanmayı hazırlayın.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için Site Recovery Içindekiler tablosunun nasıl yapılır bölümündeki makaleyi gözden geçirin.

## <a name="before-you-start"></a>Başlamadan önce

[Bu serinin ilk öğreticisinde](avs-tutorial-prepare-azure.md)açıklandığı gibi Azure 'ı hazırladığınızdan emin olun.

## <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site Recovery için Azure VMware Çözüm sunucularına erişmesi gerekir:

- VM'leri otomatik olarak bulma. En az bir salt okunur hesap gereklidir.
- Çoğaltma, yük devretme ve yeniden çalıştırmayı yönetme. Diskleri oluşturma ve kaldırma ve VM’leri çalıştırma gibi işlemleri gerçekleştirebilen bir hesabınızın olması gerekir.

Hesabı aşağıdaki gibi oluşturun:

1. Ayrılmış bir hesap kullanmak için, rolü vCenter düzeyinde oluşturun. Role **Azure_Site_Recovery** gibi bir ad verin.
2. Role aşağıdaki tabloda özetlenen izinleri atayın.
3. vCenter sunucusu veya vSphere ana bilgisayarında bir kullanıcı oluşturun. Rolü kullanıcıya atayın.

### <a name="vmware-account-permissions"></a>VMware hesap izinleri

**Görev** | **Rol/İzinler** | **Ayrıntılar**
--- | --- | ---
**VM bulma** | En az bir salt okunur kullanıcı<br/><br/> Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için, **alt nesnelere yay** nesnesine göre **erişim** rolü (vSphere Konakları, veri depoları, VM 'ler ve ağlar) atayın.
**Tam çoğaltma, yük devretme, yeniden çalışma** |  Gerekli izinlere sahip bir rol (Azure_Site_Recovery) oluşturup rolü VMware kullanıcısı veya grubuna atayın<br/><br/> Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Azure_Site_Recovery<br/><br/> Veri deposu -> Alan ayırma, veri deposuna göz atma, düşük düzeyli dosya işlemleri, dosyayı kaldırma, sanal makine dosyalarını güncelleştirme<br/><br/> Ağ -> Ağ ataması<br/><br/> Kaynak -> VM’yi kaynak havuzuna atama, kapalı VM’yi geçirme, açık VM’yi geçirme<br/><br/> Görevler -> Görev oluşturma, görevi güncelleştirme<br/><br/> Sanal makine -> Yapılandırma<br/><br/> Sanal makine -> Etkileşim -> soruyu yanıtlama, cihaz bağlantısı, CD ortamını yapılandırma, disket ortamını yapılandırma, kapatma, açma, VMware araçlarını yükleme<br/><br/> Sanal makine -> Envanter -> Oluşturma, kaydetme, kaydı kaldırma<br/><br/> Sanal makine -> Sağlama -> Sanal makine indirmeye izin verme, Sanal makine dosyalarını karşıya yüklemeye izin verme<br/><br/> Sanal makine -> Anlık görüntüler -> Anlık görüntüleri kaldırma | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için, **alt nesnelere yay** nesnesine göre **erişim** rolü (vSphere Konakları, veri depoları, VM 'ler ve ağlar) atayın.

## <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Çoğaltmak istediğiniz makinelerde Mobility hizmeti yüklü olmalıdır. Makine için çoğaltmayı etkinleştirdiğinizde Site Recovery bu hizmeti göndererek yükleme işlemi yapabileceği gibi, bunu el ile yükleyebilir veya yükleme araçlarını kullanabilirsiniz.

- Bu öğreticide, Mobility hizmetini göndererek yükleme yoluyla yükleyeceğiz.
- Bu göndererek yükleme için, Site Recovery’nin sanal makineye erişmek için kullanabileceği bir hesap hazırlamanız gerekir. Azure konsolunda olağanüstü durum kurtarmayı ayarlarken bu hesabı belirtirsiniz.

Hesabı aşağıdaki gibi hazırlayın:

VM üzerinde yükleme izinleri ile bir etki alanı veya yerel hesap hazırlayın.

- **Windows VM'leri**: Windows VM'lerine yüklemek için, bir etki alanı hesabı kullanmıyorsanız yerel makinede Uzak Kullanıcı Erişim denetimini devre dışı bırakın. Bunu yapmak için kayıt defterinde > **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** altında **LocalAccountTokenFilterPolicy** adlı DWORD girişini 1 değeriyle ekleyin.
- **Linux VM'leri**: Linux VM'lerine yüklemek için, kaynak Linux sunucusunda bir kök hesabı hazırlayın.


## <a name="check-vmware-requirements"></a>VMware gereksinimlerini denetleme

VMware sunucularının ve sanal makinelerin gereksinimlerle uyumlu olduğundan emin olun.

1. Azure VMware Çözüm [yazılımı sürümlerini](../azure-vmware/concepts-private-clouds-clusters.md#vmware-software-versions)doğrulayın.
2. [VMware sunucusu gereksinimlerini](vmware-physical-azure-support-matrix.md#on-premises-virtualization-servers)doğrulayın.
3. Linux VM'leri için dosya sistemini ve depolama gereksinimlerini [denetleyin](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage). 
4. [Ağ](vmware-physical-azure-support-matrix.md#network) ve [depolama](vmware-physical-azure-support-matrix.md#storage) desteğini denetleyin. 
5. Yük devretmenin ardından [Azure ağ](vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [depolama](vmware-physical-azure-support-matrix.md#azure-storage) ve [işlem](vmware-physical-azure-support-matrix.md#azure-compute) için nelerin desteklendiğini denetleyin.
6. Azure 'a çoğaltılan Azure VMware çözümünüz sanal makinelerinizin [Azure VM gereksinimleriyle](vmware-physical-azure-support-matrix.md#azure-vm-requirements)uyumlu olması gerekir.
7. Linux sanal makinelerinde, cihaz adı veya bağlama noktası adı benzersiz olmalıdır. İki cihaz/bağlama noktasının aynı ada sahip olmadığından emin olun. Adın büyük/küçük harfe duyarlı olmadığına unutmayın. Örneğin, _Device1_ ve _DEVICE1_ ile aynı VM için iki cihazın adlandırılmasına izin verilmez.


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Yük devretmeden sonra Azure VMware Çözüm ağından Azure VM 'lerine bağlanmak isteyebilirsiniz.

Yük devretmeden sonra RDP kullanarak Windows VM’lerine bağlanmak için aşağıdakileri yapın:

- **Internet erişimi**. Yük devretmeden önce, yük devretmeden önce Azure VMware Çözüm VM 'sinde RDP 'yi etkinleştirin. TCP ve UDP kurallarının **Ortak** profil için eklendiğinden ve tüm profillerde **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalar** içinde RDP’ye izin verildiğinden emin olun.
- **Konumdan konuma VPN erişimi**:
    - Yük devretmeden önce Azure VMware Çözüm VM 'sinde RDP 'yi etkinleştirin.
    -   ->  **Etki alanı ve özel** ağlar için Windows Güvenlik Duvarı **izin verilen uygulamalar ve özelliklerde** RDP 'ye izin verilmelidir.
    - İşletim sisteminin SAN ilkesinin **OnlineAll** olarak ayarlandığından emin olun. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135).
- Bir yük devretme tetiklediğinizde VM’de bekleyen Windows güncelleştirmelerinin olmaması gerekir. Varsa, güncelleştirme tamamlanana kadar sanal makinede oturum açamazsınız.
- Yük devretmeden sonra Windows Azure VM’sinde, VM’nin bir ekran görüntüsünü görmek için **Önyükleme tanılaması**’nı kontrol edin. Bağlanamıyorsanız, VM’nin çalıştığından emin olun ve şu [sorun giderme ipuçlarını](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) gözden geçirin.

Yük devretmeden sonra SSH kullanarak Linux VM’lerine bağlanmak için aşağıdakileri yapın:

- Yük devretmeden önce Azure VMware Çözüm VM 'sinde, Secure Shell hizmetinin sistem önyüklemesi sırasında otomatik olarak başlayacak şekilde ayarlandığından emin olun.
- Güvenlik duvarı kurallarının SSH bağlantısına izin verdiğinden emin olun.
- Yük devretmeden sonra Azure VM’sinde, yük devredilen VM üzrindeki ağ güvenlik grubu kuralları ve VM’nin bağlı olduğu Azure alt ağı için SSH bağlantı noktasına gelen bağlantılara izin verin.
- VM için bir [ortak IP adresi ekleyin](./site-recovery-monitor-and-troubleshoot.md).
- VM’nin bir ekran görüntüsünü görmek için **Önyükleme tanılaması**’nı kontrol edebilirsiniz.


## <a name="failback-requirements"></a>Yeniden çalışma gereksinimleri
Azure VMware Çözüm bulutuna yeniden yük devredecaksanız, yeniden [çalışma için](avs-tutorial-reprotect.md#before-you-begin)bir dizi önkoşul vardır. Bunları şimdi hazırlayabilirsiniz, ancak bunu yapmanız gerekmez. Azure 'a yük devreden sonra hazırlanabilirsiniz.




## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma kurulumu](avs-tutorial-replication.md)
- Birden çok VM 'yi çoğaltırken [Kapasite planlaması gerçekleştirin](site-recovery-deployment-planner.md).
