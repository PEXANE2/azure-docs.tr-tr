---
title: Azure Site Kurtarma ile fiziksel şirket içi sunucuların olağanüstü durum kurtarmasını ayarlama
description: Azure Site Kurtarma hizmeti yle şirket içi Windows ve Linux sunucuları için Azure'da olağanüstü durum kurtarma yı nasıl ayarlayacağınızı öğrenin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 11/12/2019
ms.author: raynew
ms.openlocfilehash: 2f92c2b800c6d30cc5f365e6d24925a70d3db55a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257933"
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-physical-servers"></a>Şirket içi fiziksel sunucular için Azure'da olağanüstü durum kurtarma ayarlama

[Azure Site Recovery](site-recovery-overview.md) hizmeti, şirket içi makinelerin ve Azure sanal makinelerinin (VM) çoğaltma, yük devretme ve yeniden çalışma işlemlerini yöneterek ve düzenleyerek olağanüstü durum kurtarma stratejinize katkı sağlar.

Bu öğretici, şirket içi fiziksel Windows ve Linux sunucularının Azure'da olağanüstü durum kurtarmasını nasıl ayarlayabileceğinizi gösterir. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure ve şirket içi ön koşulları ayarlama
> * Site Recovery için Kurtarma Hizmetleri kasası oluşturma 
> * Kaynak ve hedef çoğaltma ortamlarını ayarlama
> * Çoğaltma ilkesi oluşturma
> * Sunucu için çoğaltmayı etkinleştirme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- Bu senaryonun [mimarisini ve bileşenlerini](physical-azure-architecture.md) anladığınızdan emin olun.
- Tüm bileşenler için [destek gereksinimlerini](vmware-physical-secondary-support-matrix.md) gözden geçirin.
- Çoğaltmak istediğiniz sunucuların [Azure VM gereksinimlerine](vmware-physical-secondary-support-matrix.md#replicated-vm-support)uygun olduğundan emin olun.
- Azure'u hazırlayın. Bir Azure aboneliğine, Azure sanal ağına ve bir depolama hesabına ihtiyacınız var.
- Çoğaltmak istediğiniz her sunucuda Mobilite hizmetinin otomatik olarak yüklenmesi için bir hesap hazırlayın.

Başlamadan önce şunları unutmayın:

- Azure'da başarısız olduktan sonra, fiziksel sunucular şirket içi fiziksel makinelere geri başarısız olamaz. Yalnızca VMware VM'lere geri dönebilirsiniz. 
- Bu öğretici, en basit ayarlarla Azure'a fiziksel sunucu olağanüstü durum kurtarma yı ayarlar. Diğer seçenekler hakkında bilgi edinmek istiyorsanız, Nasıl Olur?
    - Site Kurtarma yapılandırma sunucusu da dahil olmak üzere [çoğaltma kaynağını](physical-azure-set-up-source.md)ayarlayın.
    - [Çoğaltma hedefini](physical-azure-set-up-target.md) ayarlayın.
    - Bir [çoğaltma ilkesi](vmware-azure-set-up-replication.md) yapılandırın ve [çoğaltmayı etkinleştirin](vmware-azure-enable-replication.md).


### <a name="set-up-an-azure-account"></a>Azure hesabı ayarlama

Bir Microsoft [Azure hesabı](https://azure.microsoft.com/)alın.

- [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz.
- Site [Kurtarma fiyatlandırması](site-recovery-faq.md#pricing)hakkında bilgi edinin ve [fiyatlandırma ayrıntılarını](https://azure.microsoft.com/pricing/details/site-recovery/)öğrenin.
- Site Kurtarma için hangi [bölgelerin desteklendiğine](https://azure.microsoft.com/pricing/details/site-recovery/) öğrenin.

### <a name="verify-azure-account-permissions"></a>Azure hesap izinlerini doğrulama

Azure hesabınızın VM'lerin Azure'a çoğaltılması için izinleri olduğundan emin olun.

- Makineleri Azure'da çoğaltmak için gereken [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) gözden geçirin.
- [Rol tabanlı erişim](../role-based-access-control/role-assignments-portal.md) izinlerini doğrulayın ve değiştirin. 



### <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Azure [ağı](../virtual-network/quick-create-portal.md)ayarlayın.

- Azure VM'ler, başarısız olduktan sonra oluşturulduklarında bu ağa yerleştirilir.
- Ağ, Kurtarma Hizmetleri kasası ile aynı bölgede olmalıdır


## <a name="set-up-an-azure-storage-account"></a>Azure depolama hesabı ayarlama

Bir [Azure depolama hesabı](../storage/common/storage-account-create.md)ayarlayın.

- Site Kurtarma, şirket içi makineleri Azure depolamasına kopyalar. Azure VM'leri, başarısız olduktan sonra depolama dan oluşturulur.
- Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.


### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Mobilite hizmeti çoğaltmak istediğiniz her sunucuya yüklenmiş olmalıdır. Site Kurtarma, sunucu için çoğaltmayı etkinleştirdiğinizde bu hizmeti otomatik olarak yükler. Otomatik olarak yüklemek için, Site Kurtarma'nın sunucuya erişmek için kullanacağı bir hesap hazırlamanız gerekir.

- Bir etki alanı veya yerel hesap kullanabilirsiniz
- Windows VM'leri için, bir etki alanı hesabı kullanmıyorsanız, yerel makinede Uzaktan Kullanıcı Erişimi denetimini devre dışı bırakın. Bunu yapmak için, **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**altında kayıt, 1 değeri ile DWORD girişi **LocalAccountTokenFilterPolicy**ekleyin.
- CLI'den ayarı devre dışı atmak için kayıt defteri girişini eklemek için şunları yazın:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Linux için, hesap kaynak Linux sunucusunda kök olmalıdır.


## <a name="create-a-vault"></a>Kasa oluşturma

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Koruma hedefi seçme

Neyi kopyalayabilmek ve çoğaltmak için seçin.

1. **Kurtarma Hizmetleri kasaları** > kasa öğesine tıklayın.
2. Kaynak Menüsünde, **Site Kurtarma** > Altyapı > **Koruma yı****hazırlayın**hedefini tıklatın.
3. **Koruma hedefinde,****Sanallaştırılan** **Değil/Diğer'e Azure'u** > seçin.

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Yapılandırma sunucusunu ayarlayın, kasaya kaydedin ve VM'leri keşfedin.

1. **Site Kurtarma** > **Hazırlık Altyapı** > **Kaynağı'nı**tıklatın.
2. Yapılandırma sunucunuz yoksa **+Configuration server'ı**tıklatın.
3. **Add Server'da**Configuration **Server'ın** **Sunucu türünde**görünip görünmediğini denetleyin.
4. Site Kurtarma Birleşik Kurulum yükleme dosyasını indirin.
5. Kasa kayıt anahtarını indirin. Birleşik Kurulum'u çalıştırdığınızda buna ihtiyacınız vardır. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

   ![Kaynağı ayarlama](./media/physical-azure-disaster-recovery/source-environment.png)


### <a name="register-the-configuration-server-in-the-vault"></a>Yapılandırma sunucusunu kasaya kaydedin

Başlamadan önce aşağıdakileri yapın: 

#### <a name="verify-time-accuracy"></a>Zaman doğruluğunu doğrula
Yapılandırma sunucusu makinesinde, sistem saatinin Bir Time [Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service)ile senkronize olduğundan emin olun. Eşleşmeli. Önde veya arkada 15 dakika varsa, kurulum başarısız olabilir.

#### <a name="verify-connectivity"></a>Bağlantıyı doğrulama
Makinenin ortamınıza bağlı olarak bu URL'lere erişebileceğinden emin olun: 

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

IP adresi tabanlı güvenlik duvarı kuralları, https (443) bağlantı noktası üzerinde yukarıda listelenen tüm Azure URL'leri ile iletişime izin vermelidir. IP Aralıklarını basitleştirmek ve sınırlamak için URL filtrelemenin yapılması önerilir.

- **Ticari IP'ler** - [Azure Veri Merkezi IP Aralıklarına](https://www.microsoft.com/download/confirmation.aspx?id=41653)ve HTTPS (443) bağlantı noktasına izin verin. AAD, Yedekleme, Çoğaltma ve Depolama URL'lerini desteklemek için aboneliğinizin Azure bölgesi için IP adres aralıklarına izin verin.  
- **Devlet IP'leri** - AAD, Yedekleme, Çoğaltma ve Depolama URL'lerini desteklemek için Tüm USGov Bölgeleri (Virginia, Teksas, Arizona ve Iowa) için [Azure Devlet Veri Merkezi IP Aralıkları](https://www.microsoft.com/en-us/download/details.aspx?id=57063)ve HTTPS (443) bağlantı noktasına izin verin.  

#### <a name="run-setup"></a>Kurulumu çalıştırma
Yapılandırma sunucusunu yüklemek için Birleşik Kurulumu Yerel Yönetici olarak çalıştırın. İşlem sunucusu ve ana hedef sunucu da varsayılan olarak yapılandırma sunucusuna yüklenir.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

Kayıt bittikten sonra yapılandırma sunucusu kasadaki **Ayarlar** > **Sunucuları** sayfasında görüntülenir.

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın.

1. **Altyapı Hedef'i Hazırla'yı** > **Target**tıklatın ve kullanmak istediğiniz Azure aboneliğini seçin.
2. Hedef dağıtım modelini belirtin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

   ![Hedef](./media/physical-azure-disaster-recovery/network-storage.png)


## <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. Yeni bir çoğaltma ilkesi oluşturmak için **Site Kurtarma altyapısı** > **Çoğaltma İlkeleri** > **+Çoğaltma İlkesi'ni**tıklatın.
2. **Çoğaltma ilkesi oluştur** bölümünde bir ilke adı belirtin.
3. **RPO eşiği** bölümünde kurtarma noktası hedefi (RPO) sınırını belirtin. Bu değer, veri kurtarma noktalarının ne sıklıkta oluşturulduğunu belirtir. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
4. **Kurtarma noktası bekletme** bölümünde, her kurtarma noktası için bekletme süresinin ne kadar olacağını (saat) belirtin. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir. Premium depolama alanına çoğaltılan makineler için 24 saate, standart depolama için de 72 saate kadar bekletme desteklenir.
5. **Uygulama tutarlı anlık görüntü sıklığında,** uygulama tutarlı anlık görüntüler içeren kurtarma noktalarının ne sıklıkta (dakika dakika içinde) oluşturulacağını belirtin. İlkeyi oluşturmak için **Tamam**’a tıklayın.

    ![Çoğaltma ilkesi](./media/physical-azure-disaster-recovery/replication-policy.png)


İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. Eşleşen bir ilke, varsayılan olarak yeniden çalışma için otomatik oluşturulur. Örneğin, çoğaltma **ilkesi rep-ilke** ise, başarısız ilke **temsilcisi-failback** oluşturulur. Bu ilke Azure’dan bir yeniden çalışma başlatılana kadar kullanılmaz.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Her sunucu için çoğaltmayı etkinleştirin.

- Site Kurtarma çoğaltma etkinleştirildiğinde Mobilite hizmetini yükler.
- Bir sunucu için çoğaltmayı etkinleştirdiğinizde, değişikliklerin etkili olması 15 dakika veya daha uzun sürebilir ve portalda görünebilir.

1. Uygulama**Kaynağını** **Çoğaltma'yı** > tıklatın.
2. **Kaynak** bölümünde yapılandırma sunucusunu seçin.
3. **Makine türünde,** **Fiziksel makineleri**seçin.
4. İşlem sunucusunu (yapılandırma sunucusu) seçin. Ardından **Tamam**'a tıklayın.
5. **Hedef'te,** başarısız olduktan sonra Azure VM'lerini oluşturmak istediğiniz abonelik ve kaynak grubunu seçin. Azure'da kullanmak istediğiniz dağıtım modelini (klasik veya kaynak yönetimi) seçin.
6. Verileri çoğaltmak için kullanmak istediğiniz Azure depolama hesabını seçin. 
7. Yük devretme sonrasında oluşturulan Azure VM'lerinin bağlanacağı Azure ağını ve alt ağını seçin.
8. Ağ ayarını koruma için seçtiğiniz tüm makinelere uygulamak **için seçili makineler için şimdi Yapılandır'ı**seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin. 
9. **Fiziksel Makineler'de**ve **+Fiziksel makine'ye**tıklayın. Adı ve IP adresini belirtin. Çoğaltmak istediğiniz makinenin işletim sistemini seçin. Sunucuların bulunması ve listelenilmesi birkaç dakika sürer. 
10. **Özellikleri** > **Yapılandırma özelliklerinde,** mobilite hizmetini makineye otomatik olarak yüklemek için işlem sunucusu tarafından kullanılacak hesabı seçin.
11. **Çoğaltma ayarlarında** > **çoğaltma ayarlarını yapılandırın,** doğru çoğaltma ilkesinin seçildiğini doğrulayın. 
12. **Çoğaltmayı Etkinleştir**’e tıklayın. **Ayarlar** > **İşler** > **Site Recovery İşleri** bölümünden **Korumayı Etkinleştir** işinin ilerleme durumunu izleyebilirsiniz. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.


Eklediğiniz sunucuları izlemek için, **yapılandırma sunucularında** > bunlar için son keşfedilen zamanı son**kişi adresinden**kontrol edebilirsiniz. Zamanlanan bir bulma süresini beklemeden makine eklemek için yapılandırma sunucusunu vurgulayın (tıklatmayın) ve **Yenile'yi**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

[Bir felaket kurtarma tatbikatı çalıştırın.](tutorial-dr-drill-azure.md)
