---
title: Azure Site Kurtarma'da VMware vCenter sunucularını yönetme
description: Bu makalede, Azure Site Kurtarma ile VMware VM'lerin Azure'a olağanüstü kurtarma için VMware vCenter'ın nasıl ekleyip yönetileni açıklanmaktadır.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/24/2019
ms.author: ramamill
ms.openlocfilehash: 01aef3aca4f6967b1681bff9598c7dd7a24739cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257270"
---
# <a name="manage-vmware-vcenter-server"></a>VMware vCenter Server'ı yönetin

Bu makalede, [Azure Site Kurtarma'daki](site-recovery-overview.md)bir VMware vCenter Server'daki yönetim eylemleri özetlenmiştir.

## <a name="verify-prerequisites-for-vcenter-server"></a>vCenter Server için ön koşulları doğrula

VMware VM'lerin Azure'a olağanüstü kurtarma sırasında vCenter Sunucuları ve VM'leri için ön koşullar [destek matrisinde](vmware-physical-azure-support-matrix.md#replicated-machines)listelenmiştir.

## <a name="set-up-an-account-for-automatic-discovery"></a>Otomatik bulma için hesap ayarlama

Şirket içi VMware VM'ler için olağanüstü durum kurtarma ayarladığınızda, Site Kurtarma'nın vCenter Server/vSphere ana bilgisayarına erişmesi gerekir. Site Kurtarma işlem sunucusu daha sonra otomatik olarak VM'leri keşfedebilir ve gerektiğinde başarısız olabilir. Varsayılan olarak, işlem sunucusu Site Kurtarma yapılandırma sunucusunda çalışır. VCenter Server/vSphere ana lığına bağlanmak için yapılandırma sunucusu için aşağıdaki gibi bir hesap ekleyin:

1. Yapılandırma sunucusunda oturum açın.
1. Masaüstü kısayolu kullanarak configuration server aracını _(cspsconfigtool.exe)_ açın.
1. Hesabı **Yönet** sekmesinde **Hesap Ekle'yi**tıklatın.

   ![ek-hesap](./media/vmware-azure-manage-vcenter/addaccount.png)

1. Hesap ayrıntılarını sağlayın ve eklemek için **Tamam'ı** tıklatın. Hesabın ayrıcalıkları hesap izinleri tablosunda özetlenmelidir.

   > [!NOTE]
   > Hesap bilgilerini Site Kurtarma ile eşitlemek yaklaşık 15 dakika sürer.

### <a name="account-permissions"></a>Hesap izinleri

|**Görev** | **Hesap** | **Izin** | **Şey**|
|--- | --- | --- | ---|
|**VM bulma/geçiş (failback olmadan)** | En azından salt okunur kullanıcı hesabı. | Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak **için, Yayı alt nesneye** (vSphere ana bilgisayarları, veri depoları, sanal makineler ve ağlar) **Access'le erişim yok** rolünü atayın.|
|**Çoğaltma/başarısız** | En azından salt okunur kullanıcı hesabı. | Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak **için, Yayı olan** **Access'i** alt nesnelere (vSphere ana bilgisayarlar, veri depoları, sanal makineler ve ağlar) alt nesneye atayın.<br/><br/> Geçiş amaçları için yararlıdır, ancak tam çoğaltma, başarısız, başarısız.|
|**Çoğaltma/başarısız/failback** | Gerekli izinleri içeren bir rol (AzureSiteRecoveryRole) oluşturmanızı ve ardından rolü bir VMware kullanıcı veya grubuna atamanızı öneririz. | Veri Merkezi nesnesi –> Alt Nesneye Yayılma, role=AzureSiteRecoveryRole<br/><br/> Veri deposu -> Alan ayırma, veri deposuna göz atma, düşük düzeyli dosya işlemleri, dosyayı kaldırma, sanal makine dosyalarını güncelleştirme<br/><br/> Ağ -> Ağ ataması<br/><br/> Kaynak -> VM’yi kaynak havuzuna atama, kapalı VM’yi geçirme, açık VM’yi geçirme<br/><br/> Görevler -> Görev oluşturma, görevi güncelleştirme<br/><br/> Sanal makine -> Yapılandırma<br/><br/> Sanal makine -> Etkileşim -> soruyu yanıtlama, cihaz bağlantısı, CD ortamını yapılandırma, disket ortamını yapılandırma, kapatma, açma, VMware araçlarını yükleme<br/><br/> Sanal makine -> Envanter -> Oluşturma, kaydetme, kaydı kaldırma<br/><br/> Sanal makine -> Sağlama -> Sanal makine indirmeye izin verme, Sanal makine dosyalarını karşıya yüklemeye izin verme<br/><br/> Sanal makine -> Anlık görüntüler -> Anlık görüntüleri kaldırma | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak **için, Yayı alt nesneye** (vSphere ana bilgisayarları, veri depoları, sanal makineler ve ağlar) **Access'le erişim yok** rolünü atayın.|

## <a name="add-vmware-server-to-the-vault"></a>Kasaya VMware sunucusu ekleme

Şirket içi VMware VM'ler için olağanüstü durum kurtarma ayarladığınızda, VM'leri keşfettiğiniz vCenter Server/vSphere ana bilgisayarını Site Kurtarma kasasına aşağıdaki gibi eklersiniz:

1. Vault > **Site Kurtarma Altyapı** > **Yapılandırma Severs**, yapılandırma sunucusu açın.
1. **Ayrıntılar** sayfasında **vCenter'ı**tıklatın.
1. **Ekle vCenter'da,** vSphere ana bilgisayar veya vCenter sunucusu için uygun bir ad belirtin.
1. Sunucunun IP adresini veya FQDN'sini belirtin.
1. VMware sunucularınız farklı bir bağlantı noktasındaki istekleri dinleyecek şekilde yapılandırılmadıkça bağlantı noktasını 443 olarak bırakın.
1. VMware vCenter veya vSphere ESXi sunucusuna bağlanmak için kullanılan hesabı seçin. Ardından **Tamam**'a tıklayın.

## <a name="modify-credentials"></a>Kimlik bilgilerini değiştirme

Gerekirse, vCenter Server/vSphere ana bilgisayara bağlanmak için kullanılan kimlik bilgilerini aşağıdaki gibi değiştirebilirsiniz:

1. Yapılandırma sunucusunda oturum açın.
1. Masaüstü kısayolu kullanarak configuration server aracını _(cspsconfigtool.exe)_ açın.
1. **Hesabı Yönet** sekmesinde **Hesap Ekle'yi** tıklatın.

   ![ek-hesap](./media/vmware-azure-manage-vcenter/addaccount.png)

1. Yeni hesap ayrıntılarını sağlayın ve **Tamam'ı**tıklatın. Hesabın [hesap izinleri](#account-permissions) tablosunda listelenen izinlere ihtiyacı vardır.
1. Site Kurtarma **Altyapı** > **Yapılandırma Severs**> kasasında, yapılandırma sunucusu açın.
1. **Ayrıntılar'da,** **Sunucu'yı Yenile'yi**tıklatın.
1. Sunucuyu Yenile işi bittikten sonra vCenter Server'ı seçin.
1. **Özetle,** **vCenter server/vSphere ana bilgisayar**hesabında yeni eklenen hesabı seçin ve **Kaydet'i**tıklatın.

   ![değişiklik hesabı](./media/vmware-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-server"></a>vCenter Sunucusu'ni silme

1. Site Kurtarma **Altyapı** > **Yapılandırma Severs**> kasasında, yapılandırma sunucusu açın.
1. **Ayrıntılar** sayfasında vCenter sunucusunu seçin.
1. **Sil** düğmesine tıklayın.

   ![silme hesabı](./media/vmware-azure-manage-vcenter/delete-vcenter.png)

## <a name="modify-the-ip-address-and-port"></a>IP adresini ve bağlantı noktasını değiştirme

vCenter Server'ın IP adresini veya sunucu ile Site Kurtarma arasındaki iletişim için kullanılan bağlantı noktalarını değiştirebilirsiniz. Varsayılan olarak, Site Kurtarma bağlantı noktası 443 üzerinden vCenter Server/vSphere ana bilgisayar bilgilerine erişir.

1. Site Kurtarma **Altyapı** > **Yapılandırma Sunucuları**> kasasında, vCenter Server'ın eklendiği yapılandırma sunucusuna tıklayın.
1. **vCenter sunucularında,** değiştirmek istediğiniz vCenter Server'ı tıklatın.
1. **Özetle,** IP adresini ve bağlantı noktasını güncelleştirin ve değişiklikleri kaydedin.

   ![add_ip_new_vcenter](media/vmware-azure-manage-vcenter/add-ip.png)

1. Değişikliklerin etkili olması için 15 dakika bekleyin veya [yapılandırma sunucusunu yenileyin.](vmware-azure-manage-configuration-server.md#refresh-configuration-server)

## <a name="migrate-all-vms-to-a-new-server"></a>Tüm VM'leri yeni bir sunucuya geçirin

Yeni bir vCenter Server kullanmak için tüm VM'leri geçirmek istiyorsanız, vCenter Server'a atanan IP adresini güncelleştirmeniz gerekir. Yinelenen girişlere yol açabileceğinden, başka bir VMware hesabı eklemeyin. Adresi aşağıdaki gibi güncelleyin:

1. Site Kurtarma **Altyapı** > **Yapılandırma Sunucuları**> kasasında, vCenter Server'ın eklendiği yapılandırma sunucusuna tıklayın.
1. **vCenter sunucuları** bölümünde, geçiş yapmak istediğiniz vCenter Server'ı tıklatın.
1. **Özetle,** IP adresini yeni vCenter Server'ınkine güncelleştirin ve değişiklikleri kaydedin.
1. IP adresi güncellenir güncellenmez, Site Kurtarma yeni vCenter Server'dan VM bulma bilgilerini almaya başlar. Bu, devam eden çoğaltma etkinliklerini etkilemez.

## <a name="migrate-a-few-vms-to-a-new-server"></a>Birkaç VM'yi yeni bir sunucuya geçirin

Çoğaltma VM'lerinizden yalnızca birkaçını yeni bir vCenter Server'a geçirmek istiyorsanız, aşağıdakileri yapın:

1. Yapılandırma sunucusuna yeni vCenter Server'ı [ekleyin.](#add-vmware-server-to-the-vault)
1. Yeni sunucuya taşınacak VM'ler için [çoğaltmayı devre dışı kaldırın.](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure)
1. VMware'de, VM'leri yeni vCenter Server'a geçirin.
1. Geçirilen VM'ler için çoğaltmayı yeniden [etkinleştirin](vmware-azure-tutorial.md#enable-replication) ve yeni vCenter Server'ı seçin.

## <a name="migrate-most-vms-to-a-new-server"></a>Çoğu VM'yi yeni bir sunucuya geçirin

Yeni bir vCenter Server'a geçirmek istediğiniz VM sayısı, orijinal vCenter Sunucusunda kalacak VM sayısından daha yüksekse, aşağıdakileri yapın:

1. Yapılandırma sunucusu ayarlarında vCenter Server'a atanan IP adresini yeni vCenter Server'ın adresine [güncelleştirin.](#modify-the-ip-address-and-port)
1. Eski sunucuda kalan birkaç VM için [çoğaltmayı devre dışı bırakın.](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure)
1. [Eski vCenter Server'ı](#add-vmware-server-to-the-vault) ve IP adresini yapılandırma sunucusuna ekleyin.
1. Eski sunucuda kalan VM'ler için [çoğaltmayı yeniden etkinleştirin.](vmware-azure-tutorial.md#enable-replication)

## <a name="next-steps"></a>Sonraki adımlar

Herhangi bir sorununz varsa, [sorun giderme vCenter Server bulma hatalarına](vmware-azure-troubleshoot-vcenter-discovery-failures.md)bakın.
