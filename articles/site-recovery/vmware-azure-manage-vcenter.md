---
title: Azure Site Recovery VMware vCenter sunucularını yönetme
description: Bu makalede, Azure Site Recovery ile Azure 'a VMware VM 'lerinin olağanüstü durum kurtarması için VMware vCenter 'ın nasıl ekleneceği ve yönetileceği açıklanmaktadır.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/24/2019
ms.author: ramamill
ms.openlocfilehash: ba5f31049b599cd55a4a9a4261080c1672d336b1
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/26/2019
ms.locfileid: "75495338"
---
# <a name="manage-vmware-vcenter-server"></a>VMware vCenter Server 'ı yönetme

Bu makale, [Azure Site Recovery](site-recovery-overview.md)VMware vCenter Server yönetim eylemlerini özetler. 

## <a name="verify-prerequisites-for-vcenter-server"></a>vCenter Server için önkoşulları doğrulama

VMware VM 'lerinden Azure 'a olağanüstü durum kurtarma sırasında vCenter sunucuları ve VM 'Ler için Önkoşullar [destek matrisinde](vmware-physical-azure-support-matrix.md#replicated-machines)listelenmiştir.


## <a name="set-up-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap ayarlayın

Şirket içi VMware VM 'Leri için olağanüstü durum kurtarmayı ayarlarken, Site Recovery işlem sunucusunun sanal makineleri otomatik olarak bulabilmesi ve gerektiğinde bunların yük devredebilmesi için vCenter Server/vSphere konağına Site Recovery erişmesi gerekir. İşlem sunucusu varsayılan olarak Site Recovery yapılandırma sunucusunda çalışır. Yapılandırma sunucusu için vCenter Server/vSphere konağına aşağıdaki şekilde bağlanacak bir hesap ekleyin:

1. Yapılandırma sunucusu makinesinde oturum açın.
2. Masaüstü kısayolunu kullanarak yapılandırma sunucusu aracını (Cspsconfigtool. exe) açın.
3. **Hesabı Yönet** sekmesinde **Hesap Ekle**' ye tıklayın. 

   ![Hesap Ekle](./media/vmware-azure-manage-vcenter/addaccount.png)
4. Hesap ayrıntılarını girip **Tamam** ' a tıklayarak ekleyin.  Hesap, aşağıdaki tabloda özetlenen ayrıcalıklara sahip olmalıdır. 

Hesap bilgilerinin Site Recovery ile eşitlenmesi yaklaşık 15 dakika sürer.

### <a name="account-permissions"></a>Hesap izinleri

|**Görev** | **Hesap** | **İzinler** | **Ayrıntılar**|
|--- | --- | --- | ---|
|**VM bulma/geçiş (yeniden çalışma olmadan)** | En azından salt okunurdur bir kullanıcı hesabı. | Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için, **alt nesnelere yay** nesnesine göre **erişim** rolü (vSphere Konakları, veri depoları, sanal makineler ve ağlar) atayın.|
|**Çoğaltma/yük devretme** | En azından salt okunurdur bir kullanıcı hesabı. | Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için, alt nesnelere (vSphere Konakları, datamağazalar, sanal makineler ve ağlar), **alt nesneye yay** ile **hiçbir erişim** rolü atayın.<br/><br/> Geçiş amaçları için kullanışlıdır, ancak tam çoğaltma, yük devretme, yeniden çalışma.|
|**Çoğaltma/yük devretme/yeniden çalışma** | Gerekli izinlerle bir rol (AzureSiteRecoveryRole) oluşturmanızı ve ardından rolü bir VMware kullanıcısına veya grubuna atamayı öneririz. | Veri merkezi nesnesi – > alt nesneye yay, rol = AzureSiteRecoveryRole<br/><br/> Veri deposu -> Alan ayırma, veri deposuna göz atma, düşük düzeyli dosya işlemleri, dosyayı kaldırma, sanal makine dosyalarını güncelleştirme<br/><br/> Ağ -> Ağ ataması<br/><br/> Kaynak -> VM’yi kaynak havuzuna atama, kapalı VM’yi geçirme, açık VM’yi geçirme<br/><br/> Görevler -> Görev oluşturma, görevi güncelleştirme<br/><br/> Sanal makine -> Yapılandırma<br/><br/> Sanal makine -> Etkileşim -> soruyu yanıtlama, cihaz bağlantısı, CD ortamını yapılandırma, disket ortamını yapılandırma, kapatma, açma, VMware araçlarını yükleme<br/><br/> Sanal makine -> Envanter -> Oluşturma, kaydetme, kaydı kaldırma<br/><br/> Sanal makine -> Sağlama -> Sanal makine indirmeye izin verme, Sanal makine dosyalarını karşıya yüklemeye izin verme<br/><br/> Sanal makine -> Anlık görüntüler -> Anlık görüntüleri kaldırma | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için, **alt nesnelere yay** nesnesine göre **erişim** rolü (vSphere Konakları, veri depoları, sanal makineler ve ağlar) atayın.|


## <a name="add-vmware-server-to-the-vault"></a>VMware sunucusunu kasaya ekleme

Şirket içi VMware VM 'Leri için olağanüstü durum kurtarmayı ayarlarken, VM Site Recovery 'Leri keşfederen vCenter Server/vSphere konağını aşağıdaki gibi ekleyin:

1. Kasa > **Site Recovery altyapı** > **yapılandırma merkezi**' nde, yapılandırma sunucusunu açın.
2. **Ayrıntılar** sayfasında **vCenter**' a tıklayın.
3. **VCenter Ekle**' de, vSphere konağı veya vCenter sunucusu için bir kolay ad belirtin.
4. Sunucunun IP adresini veya FQDN 'sini belirtin.
5. VMware sunucularınız farklı bir bağlantı noktasındaki istekleri dinleyecek şekilde yapılandırılmadıkça bağlantı noktasını 443 olarak bırakın.
6. VMware vCenter veya vSphere ESXi sunucusuna bağlanmak için kullanılan hesabı seçin. Daha sonra, **Tamam**'a tıklayın.



## <a name="modify-credentials"></a>Kimlik bilgilerini değiştir

Gerekirse, vCenter Server/vSphere konağına bağlanmak için kullanılan kimlik bilgilerini aşağıdaki şekilde değiştirebilirsiniz:

1. Yapılandırmada oturum açın.
2. Masaüstü kısayolunu kullanarak yapılandırma sunucusu aracını (Cspsconfigtool. exe) açın.
2. **Hesabı Yönet** sekmesinde **Hesap Ekle** ' ye tıklayın.

   ![Hesap Ekle](./media/vmware-azure-manage-vcenter/addaccount.png)
   
3. Yeni hesap ayrıntılarını girip **Tamam**' a tıklayın. Hesap, [yukarıda](#account-permissions)listelenen izinlere ihtiyaç duyuyor.
4. Kasa > **Site Recovery altyapı** > **yapılandırma merkezi**' nde, yapılandırma sunucusunu açın.
5. **Ayrıntılar**' da **sunucuyu Yenile**' ye tıklayın.
6. Sunucuyu Yenile işi tamamlandıktan sonra vCenter Server seçin.
7. **Özet**' te, **orta sunucu/vSphere ana bilgisayar hesabındaki**yeni eklenen hesabı seçin ve **Kaydet**' e tıklayın.

   ![değiştirme hesabı](./media/vmware-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-server"></a>vCenter Server silme 

1. Kasa > **Site Recovery altyapı** > **yapılandırma merkezi**' nde, yapılandırma sunucusunu açın.
2. **Ayrıntılar** sayfasında vCenter Server ' ı seçin.
3. **Sil** düğmesine tıklayın.

   ![Hesabı Sil](./media/vmware-azure-manage-vcenter/delete-vcenter.png)

## <a name="modify-the-ip-address-and-port"></a>IP adresini ve bağlantı noktasını değiştirme

VCenter Server IP adresini veya sunucu ile Site Recovery arasındaki iletişim için kullanılan bağlantı noktalarını değiştirebilirsiniz. Varsayılan olarak, Site Recovery bağlantı noktası 443 üzerinden vCenter Server/vSphere ana bilgisayar bilgilerine erişir.

1. Kasa > **Site Recovery altyapı** > **yapılandırma sunucularında**, vCenter Server eklendiği yapılandırmalar sunucusuna tıklayın.
2. **VCenter sunucularında**, değiştirmek istediğiniz vCenter Server tıklatın.
5. **Özet**' te IP adresini ve bağlantı noktasını güncelleştirip değişiklikleri kaydedin.

   ![add_ip_new_vcenter](media/vmware-azure-manage-vcenter/add-ip.png)

6. Değişikliklerin etkili olması için 15 dakika bekleyin veya [yapılandırma sunucusunu yenileyin](vmware-azure-manage-configuration-server.md#refresh-configuration-server).


## <a name="migrate-all-vms-to-a-new-server"></a>Tüm VM 'Leri yeni bir sunucuya geçir

Tüm VM 'Leri yeni bir vCenter Server kullanmak üzere geçirmek istiyorsanız, yalnızca vCenter Server atanan IP adresini güncelleştirmeniz gerekir. Başka bir VMware hesabı eklemeyin, çünkü bu yinelenen girişlere yol açabilir. Adresi aşağıdaki gibi güncelleştirin:

1. Kasa > **Site Recovery altyapı** > **yapılandırma sunucuları**, vCenter Server eklendiği yapılandırmalar sunucusunda tat.
2. **VCenter Servers** bölümünde, geçiş yapmak istediğiniz vCenter Server tıklatın.
5. **Özet**' te, IP adresini yeni vCenter Server güncelleştirin ve değişiklikleri kaydedin.
6. IP adresi güncelleştirildikten hemen sonra, Site Recovery yeni vCenter Server VM bulma bilgilerini almaya başlar. Bu, devam eden çoğaltma etkinliklerini etkilemez.

## <a name="migrate-a-few-vms-to-a-new-server"></a>Birkaç VM 'yi yeni bir sunucuya geçirme

Çoğaltma sanal makinelerinizin yalnızca birkaçını yeni bir vCenter sunucusuna geçirmek istiyorsanız, aşağıdakileri yapın:

1. Yeni vCenter Server yapılandırma sunucusuna [ekleyin](#add-vmware-server-to-the-vault) .
2. Yeni sunucuya taşınacak VM 'Ler için [çoğaltmayı devre dışı bırakın](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) .
3. VMware 'de, VM 'Leri yeni vCenter Server geçirin. 
4. Geçirilen VM 'Ler için yeni vCenter Server seçerek [çoğaltmayı yeniden etkinleştirin](vmware-azure-tutorial.md#enable-replication) .


## <a name="migrate-most-vms-to-a-new-server"></a>VM 'Lerin çoğunu yeni bir sunucuya geçirme
 Yeni bir vCenter Server geçirmek istediğiniz sanal makinelerin sayısı, özgün vCenter Server kalacak VM sayısından yüksekse, aşağıdakileri yapın

1. Yapılandırma sunucusu ayarlarındaki vCenter Server atanan [IP adresini](#modify-the-ip-address-and-port) yeni vCenter Server adresine güncelleştirin.
2. Eski sunucuda kalan birkaç VM için [çoğaltmayı devre dışı bırakın](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) .
3. [Eski vCenter Server](#add-vmware-server-to-the-vault) ve IP adresini yapılandırma sunucusuna ekleyin.
4. Eski sunucuda kalan VM 'Ler için [çoğaltmayı yeniden etkinleştirin](vmware-azure-tutorial.md#enable-replication) .
 
 ## <a name="next-steps"></a>Sonraki adımlar
Herhangi bir sorunla karşılaşırsanız vCenter Server hatalarıyla [ilgili sorunları giderin](vmware-azure-troubleshoot-vcenter-discovery-failures.md) .
