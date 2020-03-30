---
title: Azure Site Kurtarma'yı kullanarak olağanüstü durum kurtarma için VMware VM'leri etkinleştirme
description: Bu makalede, Azure Site Kurtarma hizmetini kullanarak olağanüstü durum kurtarma için VMware VM çoğaltma etkinleştirin nasıl açıklanmaktadır
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.date: 06/28/2019
ms.topic: conceptual
ms.author: ramamill
ms.openlocfilehash: 10b3e572ec61d1eff342f24a6a5a7bcba6276983
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257322"
---
# <a name="enable-replication-to-azure-for-vmware-vms"></a>VMware VM'ler için Azure'a çoğaltmayı etkinleştirme

Bu makalede, şirket içi VMware VM'lerin Azure'a çoğaltılması nasıl etkinleştirilir.

## <a name="resolve-common-issues"></a>Sık karşılaşılan sorunları çözümle

* Her disk 4 TB'den daha küçük olmalıdır.
* İşletim sistemi diski dinamik bir disk değil, temel bir disk olmalıdır.
* Nesil 2/UEFI özellikli sanal makineler için işletim sistemi ailesi Windows olmalı ve önyükleme diski 300 GB'tan küçük olmalıdır.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, sahip olduğunuzu varsayar:

- [Şirket içi kaynak ortamınızı ayarlayın.](vmware-azure-set-up-source.md)
- [Hedef ortamınızı Azure'da ayarlayın.](vmware-azure-set-up-target.md)
- Başlamadan önce [gereksinimleri ve ön koşulları doğrulayın.](vmware-physical-azure-support-matrix.md) Unutulmaması gereken önemli şeyler şunlardır:
    - Çoğaltılan makineler için [desteklenen işletim sistemleri.](vmware-physical-azure-support-matrix.md#replicated-machines)
    - [Depolama/disk](vmware-physical-azure-support-matrix.md#storage) desteği.
    - Şirket içi makinelerin uyması gereken [Azure gereksinimleri.](vmware-physical-azure-support-matrix.md#azure-vm-requirements)


## <a name="before-you-start"></a>Başlamadan önce
VMware sanal makinelerini çoğaltırken, şu bilgileri aklınızda bulundurun:

* Azure kullanıcı hesabınızın, yeni bir sanal makinenin Azure'da çoğaltılmasını etkinleştirmek için belirli [izinlere](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) sahip olması gerekir.
* VMware VM'ler her 15 dakikada bir keşfedilir. VM'lerin keşiften sonra Azure portalında görünmesi 15 dakika veya daha uzun sürebilir. Aynı şekilde, yeni bir vCenter sunucusu veya vSphere ana bilgisayar eklediğinizde keşif 15 dakika veya daha uzun sürebilir.
* Sanal makinedeki ortam değişikliklerinin (VMware araçları kurulumu gibi) portalda güncelleştirilmesi 15 dakika veya daha uzun sürebilir.
* VMware VM'ler için son keşfedilen zamanı kontrol edebilirsiniz: VCenter sunucusu/vSphere ana bilgisayarı için **Configuration Servers** sayfasındaki **Son Kişi** At alanına bakın.
* Zamanlanan keşfi beklemeden çoğaltma için sanal makineler eklemek için yapılandırma sunucusunu vurgulayın (ancak tıklatmayın) ve **Yenile'yi**seçin.
* Çoğaltmayı etkinleştirdiğinizde, sanal makine hazırlanırsa, işlem sunucusu azure site kurtarma mobilite hizmetini otomatik olarak yükler.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Bu bölümdeki adımları izlemeden önce aşağıdaki bilgileri not edin:
* Azure Site Kurtarma artık tüm yeni çoğaltmalar için yönetilen disklere doğrudan çoğaltır. İşlem sunucusu, çoğaltma günlüklerini hedef bölgedeki bir önbellek depolama hesabına yazar. Bu günlükler, asrseeddisk adlandırma kuralına sahip çoğaltma yönetilen disklerde kurtarma noktaları oluşturmak için kullanılır.
* Yönetilen disklere çoğaltma için Powershell desteği [Az.RecoveryServices modül sürüm 2.0.0'dan itibaren](https://www.powershellgallery.com/packages/Az.RecoveryServices/2.0.0-preview) kullanılabilir 
* Başarısız olduğu sırada, seçtiğiniz kurtarma noktası hedef yönetilen diski oluşturmak için kullanılır.
* Daha önce hedef depolama hesaplarına çoğaltmak üzere yapılandırılan VM'ler etkilenmez.
* Yeni bir sanal makine için depolama hesaplarına çoğaltma yalnızca Temsili Durum Aktarımı (REST) API ve Powershell üzerinden kullanılabilir. Depolama hesaplarına çoğaltmak için Azure REST API sürümünü 2016-08-10 veya 2018-01-10'u kullanın.

Çoğaltmayı etkinleştirmek için lütfen aşağıdaki adımları izleyin:
1. Adım **2'ye git: Uygulama** > **Kaynağını**çoğaltma. Çoğaltmayı ilk kez etkinleştirdikten sonra, ek sanal makineler için çoğaltmayı etkinleştirmek için kasada **+Çoğaltma'yı** seçin.
2. **Kaynak** > **Kaynak**sayfasında yapılandırma sunucusunu seçin.
3. **Makine türü için**Sanal Makineler veya Fiziksel **Makineler'i** seçin. **Physical Machines**
4. **vCenter/vSphere Hypervisor** bölümünde vSphere konağını yöneten vCenter sunucusunu veya konağı seçin. Fiziksel bilgisayarları çoğaltırsanız bu ayar önemli değildir.
5. İşlem sunucusunu seçin. Oluşturulan ek işlem sunucuları yoksa, açılan açılır yolda yapılandırma sunucusunun dahili işlem sunucusu kullanılabilir. Her işlem sunucusunun sistem durumu, önerilen sınırlar ve diğer parametrelere göre belirtilir. Sağlıklı bir işlem sunucusu seçin. [Kritik](vmware-physical-azure-monitor-process-server.md#process-server-alerts) bir işlem sunucusu seçilemez. Hataları [giderebilir ve çözebilir](vmware-physical-azure-troubleshoot-process-server.md) **veya** [bir ölçeklendirme işlem sunucusu ayarlayabilirsiniz.](vmware-azure-set-up-process-server-scale.md)
    ![Çoğaltma kaynak penceresini etkinleştirme](media/vmware-azure-enable-replication/ps-selection.png)

> [!NOTE]
> [9.24 sürümlerinden](service-updates-how-to.md#links-to-currently-supported-update-rollups)itibaren, işlem sunucusunun sistem durumu uyarılarını geliştirmek için ek uyarılar getirilir. Oluşturulacak tüm uyarılar için Site Kurtarma bileşenlerini 9,24 sürümlerine veya daha yukarısına yükseltin.

6. **Hedef**için, başarısız olan sanal makineleri oluşturmak istediğiniz abonelik ve kaynak grubunu seçin. Başarısız olan VM'ler için Azure'da kullanmak istediğiniz dağıtım modelini seçin.
2. Azure VM'lerinin başarısız olduktan sonra bağlanacak azure ağını ve alt ağını seçin. Ağ, Site Kurtarma hizmet kasası ile aynı bölgede olmalıdır.

   Ağ ayarını koruma için seçtiğiniz tüm sanal makinelere uygulamak **için seçili makineler için şimdi Yapılandır'ı** seçin. Sanal makine başına Azure ağını seçmek için **daha sonra Yapılandır'ı** seçin. Ağınız yoksa, bir ağ oluşturmanız gerekir. Azure Kaynak Yöneticisi'ni kullanarak ağ oluşturmak için **yeni oluştur'u**seçin. Varsa bir alt ağ seçin ve ardından **Tamam'ı**seçin.
   
   ![Çoğaltma hedef penceresini etkinleştirme](./media/vmware-azure-enable-replication/enable-rep3.png)

1. **Sanal makineler** > için**sanal makineleri seçin,** çoğaltmak istediğiniz her sanal makineyi seçin. Yalnızca çoğaltmanın etkinleştirilebileceği sanal makineleri seçebilirsiniz. Sonra **Tamam**’ı seçin. Belirli bir sanal makineyi göremiyor veya seçmiyorsanız, sorunu gidermek için [Kaynak makinenin Azure portalında listelenmediğini](https://aka.ms/doc-plugin-VM-not-showing) görün.

    ![Çoğaltmayı etkinleştirsanal makineler penceresini seçin](./media/vmware-azure-enable-replication/enable-replication5.png)

1. **Özellikleri** > **Yapılandırma özellikleri**için, işlem sunucusunun Site Kurtarma Mobilite hizmetini sanal makineye otomatik olarak yüklemek için kullandığı hesabı seçin. Ayrıca, veri karmaşası desenlerinizi temel alan çoğaltmak için hedef yönetilen disk türünü seçin.
10. Varsayılan olarak, bir kaynak sanal makinenin tüm diskleri çoğaltılır. Diskleri çoğaltmadışında tutmak için çoğaltmak istemediğiniz diskler için **Ekle** onay kutusunu temizleyin. Sonra **Tamam**’ı seçin. Daha sonra ek özellikleri ayarlayabilirsiniz. [Diskleri hariç tlandırma](vmware-azure-exclude-disk.md)hakkında daha fazla bilgi edinin.

    ![Çoğaltma yapılandırma özellikleri penceresini etkinleştirme](./media/vmware-azure-enable-replication/enable-replication6.png)

1. **Çoğaltma ayarlarında** > **çoğaltma ayarlarını yapılandırın,** doğru çoğaltma ilkesinin seçildiğini doğrulayın. **Çoğaltma** > ilkesi ayarlarını Ayarlar**Çoğaltma ilkeleri** > ***ilkesi adı*** > **Edit Ayarları'nda**değiştirebilirsiniz. Bir ilke için uyguladığınız değişiklikler çoğaltma ve yeni sanal makineler için de geçerlidir.
1. Sanal makineleri bir çoğaltma grubuna toplamak istiyorsanız **Multi-VM tutarlılığını** etkinleştirin. Grup için bir ad belirtin ve ardından **Tamam'ı**seçin.

    > [!NOTE]
    >    * Çoğaltma grubundaki sanal makineler birlikte çoğalır ve başarısız olduklarında kilitlenme tutarlısı ve uygulama tutarlı kurtarma noktalarını paylaştılar.
    >    * İş yüklerinizi yansıtmaları için VM'leri ve fiziksel sunucuları bir araya getirin. Çoklu VM tutarlılığını etkinleştirmek iş yükü performansını etkileyebilir. Bunu yalnızca sanal makineler aynı iş yükünü çalıştırıyorsa ve tutarlılığa ihtiyacınız varsa yapın.

    ![Çoğaltma penceresini etkinleştirme](./media/vmware-azure-enable-replication/enable-replication7.png)
    
1. **Çoğaltmayı Etkinleştir'i**seçin.  >  **Ayarlar** > **İşleri****Sitesi Kurtarma İşlerinden** **Koruma Yı etkinleştir** işinin ilerlemesini izleyebilirsiniz. **Finalize Protection** işi çalıştırdıktan sonra, sanal makine başarısız olmaya hazırdır.

## <a name="view-and-manage-vm-properties"></a>VM özelliklerini görüntüleme ve yönetme

Ardından, kaynak sanal makinenin özelliklerini doğrulayın. Azure VM adının Azure sanal [makine gereksinimlerine](vmware-physical-azure-support-matrix.md#replicated-machines)uyması gerektiğini unutmayın.

1. Yinelenen **Ayarlar** > **öğelerine**gidin ve ardından sanal makineyi seçin. **Essentials** sayfası VM'nin ayarları ve durumu hakkında bilgi gösterir.
1. **Özellikler'de,** VM için çoğaltma ve başarısız bilgileri görüntüleyebilirsiniz.
1. **İşlem ve Ağ** > **İşlem özelliklerinde,** birden çok VM özelliğini değiştirebilirsiniz. 

    ![İşlem ve ağ özellikleri penceresi](./media/vmware-azure-enable-replication/vmproperties.png)

    * Azure VM adı: Gerekirse Azure gereksinimlerini karşılayacak şekilde adı değiştirin.
    * Hedef VM boyutu veya VM türü: Varsayılan VM boyutu, hedef Azure bölgesinde Disk sayısı, NIC sayısı, CPU çekirdek sayısı, Bellek ve kullanılabilir VM rol boyutlarını içeren birkaç parametreye göre seçilir. Azure Site Kurtarma, tüm ölçütleri karşılayan kullanılabilir ilk VM boyutunu seçer. Başarısız olmadan önce istediğiniz zaman ihtiyaçlarınıza göre farklı bir VM boyutu seçebilirsiniz. VM disk boyutunun da kaynak disk boyutuna dayandığını ve yalnızca başarısız olduktan sonra değiştirilebileni unutmayın. [Windows'daki VM diskler için Ölçeklenebilirlik ve performans hedeflerinde](../virtual-machines/windows/disk-scalability-targets.md)disk boyutları ve IOPS oranları hakkında daha fazla bilgi edinin.

    *  Kaynak grubu: Sanal bir makinenin bir gönderi başarısızının parçası haline geldiği bir [kaynak grubu](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-resource-groups-guidelines)seçebilirsiniz. Bu ayarı, başarısız olmadan önce istediğiniz zaman değiştirebilirsiniz. Başarısız olduktan sonra, sanal makineyi farklı bir kaynak grubuna geçirin, bu sanal makinenin koruma ayarları kırılır.
    * Kullanılabilirlik kümesi: Sanal makinenizin bir gönderi başarısızlığı nın bir parçası olması gerekiyorsa bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) seçebilirsiniz. Bir kullanılabilirlik kümesi seçtiğinizde, aşağıdaki bilgileri aklınızda bulundurun:

        * Yalnızca belirtilen kaynak grubuna ait kullanılabilirlik kümeleri listelenir.  
        * Farklı sanal ağlarda olan VM'ler aynı kullanılabilirlik kümesinin bir parçası olamaz.
        * Yalnızca aynı boyuttaki sanal makineler kullanılabilirlik kümesinin bir parçası olabilir.
1. Ayrıca, Azure VM'ye atanan hedef ağ, alt ağ ve IP adresi hakkında da bilgi ekleyebilirsiniz.
2. **Disklerde,** VM'de çoğaltılacak işletim sistemini ve veri disklerini görebilirsiniz.

### <a name="configure-networks-and-ip-addresses"></a>Ağları ve IP adreslerini yapılandırma

Hedef IP adresini ayarlayabilirsiniz. Bir adres sağlamazsanız, başarısız olan sanal makine DHCP kullanır. Başarısız bir adreste kullanılamayan bir adres ayarlarsanız, başarısız lık işe yaramaz. Adres test başarısız ağında varsa, test başarısız için aynı hedef IP adresini kullanabilirsiniz.

Ağ bağdaştırıcılarının sayısı, hedef sanal makine için belirttiğiniz boyuta göre aşağıdaki gibi belirlenir:

- Kaynak sanal makinedeki ağ bağdaştırıcılarının sayısı hedef VM boyutu için izin verilen bağdaştırıcı sayısından daha az veya eşitse, hedef kaynakla aynı sayıda bağdaştırıcıya sahiptir.
- Kaynak sanal makineiçin bağdaştırıcı sayısı hedef VM boyutu için izin verilen sayıyı aşıyorsa, hedef boyutu maksimum kullanılır. Örneğin, bir kaynak sanal makinenin iki ağ bağdaştırıcısı varsa ve hedef VM boyutu dört ünü destekliyorsa, hedef sanal makinenin iki bağdaştırıcısı vardır. Kaynak VM'de iki bağdaştırıcı varsa ancak hedef boyutu yalnızca birini destekliyorsa, hedef VM'de yalnızca bir bağdaştırıcı vardır.
- Sanal makinede birden çok ağ bağdaştırıcısı varsa, hepsi aynı ağa bağlanır. Ayrıca, listede gösterilen ilk bağdaştırıcı, Azure sanal makinesindeki *varsayılan* ağ bağdaştırıcısı olur. 

### <a name="azure-hybrid-benefit"></a>Azure Hibrit Avantajı

Microsoft Yazılım Güvencesi müşterileri, Azure'a geçirilen Windows Server bilgisayarlarının lisans maliyetlerinden tasarruf etmek için Azure Karma Avantajı'nı kullanabilir. Bu avantaj, Azure olağanüstü durum kurtarma için de geçerlidir. Uygunsanız, bir hata olduğunda Site Kurtarma'nın oluşturduğu sanal makineye avantaj atayabilirsiniz. Bunu yapmak için şu adımları uygulayın:
1. Çoğaltılan sanal makinenin **Bilgisayar ve Ağ özelliklerine** gidin.
2. Azure Karma Avantajı için uygun hale getiren bir Windows Server lisansına sahip olup olmadığınız sorulduğunda yanıt verin.
3. Yazılım Güvencesi ile uygun bir Windows Server lisansına sahip olduğunuzu ve bu avantajı başarısız bir şekilde oluşturulacak VM'ye uygulamak için kullanabileceğinizi doğrulayın.
4. Yinelenen sanal makine için ayarları kaydedin.

[Azure Karma Avantajı](https://aka.ms/azure-hybrid-benefit-pricing)hakkında daha fazla bilgi edinin.



## <a name="next-steps"></a>Sonraki adımlar

Sanal makine korumalı bir duruma ulaştıktan sonra, uygulamanızın Azure'da görünüp görünmediğini kontrol etmek için bir [başarısızlık](site-recovery-failover.md) deneyin.

* Çoğaltmayı devre dışı kıvmak için [kayıt ve koruma ayarlarını](site-recovery-manage-registration-and-protection.md) nasıl temizleyebileceğimi öğrenin.
* [Powershell'i kullanarak sanal makineleriniz için çoğaltmayı nasıl otomatikleştirebilirsiniz](vmware-azure-disaster-recovery-powershell.md)öğrenin.
