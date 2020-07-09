---
title: Azure Site Recovery kullanarak olağanüstü durum kurtarma için VMware VM 'lerini etkinleştirin
description: Bu makalede, Azure Site Recovery hizmeti kullanılarak olağanüstü durum kurtarma için VMware VM çoğaltmasının nasıl etkinleştirileceği açıklanır.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.date: 04/01/2020
ms.topic: conceptual
ms.author: ramamill
ms.openlocfilehash: 74870d10348421bf726b9bdc58504a74cf4105a9
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86129925"
---
# <a name="enable-replication-to-azure-for-vmware-vms"></a>VMware VM 'Leri için Azure 'a çoğaltmayı etkinleştirme

Bu makalede, şirket içi VMware sanal makinelerinin (VM) Azure 'a nasıl çoğaltılmasının nasıl etkinleştirileceği açıklanır.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, sisteminizin aşağıdaki ölçütleri karşıladığı varsayılmaktadır:

- Şirket [içi kaynak ortamınızı ayarlayın](vmware-azure-set-up-source.md).
- [Azure 'da hedef ortamınızı ayarlayın](vmware-azure-set-up-target.md).
- Başlamadan önce [gereksinimleri ve önkoşulları doğrulayın](vmware-physical-azure-support-matrix.md) . Dikkat etmeniz gereken önemli noktalar:
  - Çoğaltılan makineler için [desteklenen işletim sistemleri](vmware-physical-azure-support-matrix.md#replicated-machines) .
  - [Depolama/disk](vmware-physical-azure-support-matrix.md#storage) desteği.
  - Şirket içi makinelerin uyması gereken [Azure gereksinimleri](vmware-physical-azure-support-matrix.md#azure-vm-requirements) .

### <a name="resolve-common-issues"></a>Yaygın sorunları çözme

- Her disk 4 TB 'tan küçük olmalıdır.
- İşletim sistemi diski, dinamik disk değil, temel bir disk olmalıdır.
- 2. nesil UEFı özellikli sanal makinelerde, işletim sistemi ailesi Windows olmalıdır ve önyükleme diski 300 GB 'tan küçük olmalıdır.

## <a name="before-you-start"></a>Başlamadan önce

VMware sanal makinelerini çoğalttığınızda, bu bilgileri göz önünde bulundurun:

- Azure Kullanıcı hesabınızın yeni bir sanal makinenin Azure 'a çoğaltılmasını sağlamak için belirli [izinlere](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) sahip olması gerekir.
- VMware VM 'Leri 15 dakikada bir bulunur. VM 'Lerin bulmadan sonra Azure portal görünmesi 15 dakika veya daha fazla sürebilir. Yeni bir vCenter sunucusu veya vSphere Konağı eklediğinizde, bulma işlemi 15 dakika veya daha fazla sürebilir.
- Sanal makinede bulunan ortam değişikliklerinin portalda güncelleştirilmesini 15 dakika veya daha fazla sürebilir. Örneğin, VMware araçları yüklemesi.
- VMware VM 'Leri için en son bulunan saati denetleyebilirsiniz: vCenter Server/vSphere konağının **yapılandırma sunucuları** sayfasında **yer alan son iletişim** bölümüne bakın.
- Zamanlanan bulmayı beklemeden sanal makineleri çoğaltmaya eklemek için yapılandırma sunucusunu vurgulayın (ancak tıklamayın) ve **Yenile**' yi seçin.
- Çoğaltmayı etkinleştirdiğinizde, sanal makine hazırlanmışsa, işlem sunucusu Azure Site Recovery Mobility hizmetini otomatik olarak VM 'ye yüklenir.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Bu bölümdeki adımları uygulamadan önce, aşağıdaki bilgileri gözden geçirin:

- Azure Site Recovery artık tüm yeni çoğaltmalar için yönetilen disklere doğrudan çoğaltılır. İşlem sunucusu, çoğaltma günlüklerini hedef bölgedeki bir önbellek depolama hesabına yazar. Bu Günlükler, adlandırma kuralına sahip olan çoğaltma tarafından yönetilen disklerde kurtarma noktaları oluşturmak için kullanılır `asrseeddisk` .
- Yönetilen disklere çoğaltma için PowerShell desteği [az. RecoveryServices Modül sürümü 2.0.0](https://www.powershellgallery.com/packages/Az.RecoveryServices/2.0.0-preview) ile başlayarak kullanılabilir
- Yük devretme sırasında, seçtiğiniz kurtarma noktası, hedef tarafından yönetilen diski oluşturmak için kullanılır.
- Daha önce hedef depolama hesaplarına çoğaltılmak üzere yapılandırılmış VM 'Ler etkilenmez.
- Yeni bir sanal makine için depolama hesaplarına çoğaltma yalnızca bir temsili durum aktarımı (REST) API 'SI ve PowerShell ile kullanılabilir. Depolama hesaplarına çoğaltma için Azure REST API 2016-08-10 veya 2018-01-10 sürümünü kullanın.

Çoğaltmayı etkinleştirmek için şu adımları izleyin:

1. 2. **Adım: uygulama kaynağını Çoğalt**' a gidin  >  **Source**. Çoğaltmayı ilk kez etkinleştirdikten sonra ek sanal makineler için çoğaltmayı etkinleştirmek üzere kasada **+ Çoğalt** ' ı seçin.
1. **Kaynak** sayfada > **kaynak**bölümünde yapılandırma sunucusunu seçin.
1. **Makine türü**Için **sanal makineler** veya **fiziksel makineler**' i seçin.
1. **vCenter/vSphere Hypervisor** bölümünde vSphere konağını yöneten vCenter sunucusunu veya konağı seçin. Fiziksel bilgisayarları çoğaltdıysanız Bu ayar ilgili değildir.
1. İşlem sunucusunu seçin. Başka bir işlem sunucusu oluşturulmadıysa, yapılandırma sunucusunun yerleşik işlem sunucusu açılan menüde kullanılabilir olacaktır. Her işlem sunucusunun sistem durumu, önerilen sınırlara ve diğer parametrelere göre belirtilir. Sağlıklı bir işlem sunucusu seçin. [Kritik](vmware-physical-azure-monitor-process-server.md#process-server-alerts) bir işlem sunucusu seçilemez. Hataları [giderebilir ve çözümleyebilir](vmware-physical-azure-troubleshoot-process-server.md) **ya** da bir [genişleme işlem sunucusu](vmware-azure-set-up-process-server-scale.md)ayarlayabilirsiniz.

   :::image type="content" source="./media/vmware-azure-enable-replication/ps-selection.png" alt-text="Çoğaltma kaynağı penceresini etkinleştir":::

   > [!NOTE]
   > [Sürüm 9,24](site-recovery-whats-new.md)' den başlayarak, işlem sunucusunun sistem durumu uyarılarını iyileştirmek için ek uyarılar sunulmuştur. Tüm uyarıların üretilmesi için Site Recovery bileşenlerini 9,24 veya üzeri sürüme yükseltin.

1. **Hedef**için, yük devredilen sanal makineleri oluşturmak istediğiniz aboneliği ve kaynak grubunu seçin. Yük devredilen VM 'Ler için Azure 'da kullanmak istediğiniz dağıtım modelini seçin.
1. Yük devretmeden sonra Azure VM 'lerinin bağlanacağı Azure ağını ve alt ağını seçin. Ağın Site Recovery hizmet kasası ile aynı bölgede olması gerekir.

   Koruma için seçtiğiniz tüm sanal makinelere ağ ayarını uygulamak için **Seçili makineler için Şimdi Yapılandır** ' ı seçin. Sanal makine başına Azure ağı seçmek için **daha sonra Yapılandır** ' ı seçin. Ağınız yoksa bir tane oluşturmanız gerekir. Azure Resource Manager kullanarak bir ağ oluşturmak için **Yeni oluştur**' u seçin. Uygulanabiliyorsa bir alt ağ seçin ve ardından **Tamam**' ı seçin.

   :::image type="content" source="./media/vmware-azure-enable-replication/enable-rep3.png" alt-text="Çoğaltma hedefi penceresini etkinleştir":::

1. Sanal **makineler için**  >  **sanal makineler**' i seçin, çoğaltmak istediğiniz her bir sanal makineyi seçin. Yalnızca çoğaltmanın etkinleştiribileceği sanal makineleri seçebilirsiniz. Sonra **Tamam**’ı seçin. Belirli bir sanal makineyi göremiyorsanız veya seçmezseniz, bu sorunu çözmek için [Azure Portal kaynak makine listede listelenmiyor](vmware-azure-troubleshoot-replication.md#step-3-troubleshoot-source-machines-that-arent-available-for-replication) bölümüne bakın.

   :::image type="content" source="./media/vmware-azure-enable-replication/enable-replication5.png" alt-text="Çoğaltmayı etkinleştir sanal makineler penceresi seçin":::

1. Özellikler **Properties**  >  **yapılandırma özellikleri**için, işlem sunucusunun VM 'ye Site Recovery Mobility hizmetini otomatik olarak yüklemek için kullandığı hesabı seçin. Ayrıca, veri dalgalanma desenlerinize göre çoğaltma için kullanılacak hedef yönetilen disk türünü seçin.
1. Varsayılan olarak, bir kaynak VM 'nin tüm diskleri çoğaltılır. Diskleri çoğaltmanın dışında tutmak için, çoğaltmak istemediğiniz disklerin **dahil** etme onay kutusunu temizleyin. Sonra **Tamam**’ı seçin. Daha sonra ek özellikleri ayarlayabilirsiniz. Diskleri dışarıda bırakma hakkında [daha fazla bilgi edinin](vmware-azure-exclude-disk.md) .

   :::image type="content" source="./media/vmware-azure-enable-replication/enable-replication6.png" alt-text="Çoğaltmayı etkinleştir özellikleri yapılandırma penceresi":::

1. Çoğaltma **ayarları**  >  **çoğaltma ayarlarını yapılandır**' dan doğru çoğaltma ilkesinin seçildiğini doğrulayın. **Ayarlar**  >  **çoğaltma ilkeleri**  >  _ilke adı_  >  **düzenleme ayarları**' nın çoğaltma ilkesi ayarlarını değiştirebilirsiniz. Bir ilkeye uygulanan değişiklikler, çoğaltma ve yeni sanal makineler için de geçerlidir.
1. Sanal makineleri bir çoğaltma grubuna toplamak istiyorsanız **Çoklu VM tutarlılığını**etkinleştirin. Grup için bir ad belirtin ve ardından **Tamam**' ı seçin.

   > [!NOTE]
   > - Bir çoğaltma grubundaki sanal makineler birlikte çoğaltılır ve yük devreder, kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarına sahip olur.
   > - VM 'Leri ve fiziksel sunucuları birlikte iş yüklerinizi yansıtacak şekilde toplayın. Çoklu VM tutarlılığını etkinleştirmek, iş yükü performansını etkileyebilir. Bunu yalnızca sanal makineler aynı iş yükünü çalıştırıyorsa ve tutarlılığa ihtiyaç duyuyorsanız yapın.

   :::image type="content" source="./media/vmware-azure-enable-replication/enable-replication7.png" alt-text="Çoğaltma penceresini etkinleştir":::

1. **Çoğaltmayı etkinleştir**' i seçin. **Koruma** işinin ilerleme durumunu **Ayarlar**  >  **işleri**  >  **Site Recovery işler**' de izleyebilirsiniz. **Korumayı Sonlandır** işi çalıştıktan sonra, sanal makine yük devretmeye hazırsa.

## <a name="view-and-manage-vm-properties"></a>VM özelliklerini görüntüleme ve yönetme

Ardından, kaynak sanal makinenin özelliklerini doğrulayın. Azure VM adının [Azure sanal makine gereksinimleriyle](vmware-physical-azure-support-matrix.md#replicated-machines)uyumlu olması gerektiğini unutmayın.

1. **Ayarlar**  >  **çoğaltılan öğeler**' e gidin ve ardından sanal makineyi seçin. **Essentials** SAYFASıNDA, sanal makinenin ayarları ve durumuyla ilgili bilgiler gösterilir.
1. **Özellikler**' de VM için çoğaltma ve yük devretme bilgilerini görüntüleyebilirsiniz.
1. **İşlem ve ağ**  >  **işlem özellikleri**' nde, birden çok VM özelliğini değiştirebilirsiniz.

   :::image type="content" source="./media/vmware-azure-enable-replication/vmproperties.png" alt-text="İşlem ve ağ özellikleri penceresi":::

   - **Azure VM adı**: gerekirse, Azure gereksinimlerine uyacak şekilde adı değiştirin.
   - **Hedef VM boyutu veya VM türü**: varsayılan VM boyutu, hedef Azure bölgesindeki disk sayısı, NIC sayısı, CPU çekirdek sayısı, bellek ve kullanılabilir VM rolü boyutlarını içeren parametrelere göre seçilir. Azure Site Recovery tüm ölçütleri karşılayan kullanılabilir ilk VM boyutunu seçer. Yük devretmeden önce istediğiniz zaman, gereksinimlerinize göre farklı bir VM boyutu seçebilirsiniz. VM disk boyutu da kaynak disk boyutuna dayalıdır ve yalnızca yük devretme işleminden sonra değiştirilebilir. [Windows ÜZERINDE VM diskleri Için ölçeklenebilirlik ve performans hedeflerinde](../virtual-machines/windows/disk-scalability-targets.md)disk BOYUTLARı ve IOPS ücretleri hakkında daha fazla bilgi edinin.
   - **Kaynak grubu**: bir sanal makinenin yük devretmenin bir parçası haline geldiği bir [kaynak grubu](../azure-resource-manager/management/overview.md#resource-groups)seçebilirsiniz. Bu ayarı, yük devretmeden önce istediğiniz zaman değiştirebilirsiniz. Yük devretmeden sonra, sanal makineyi farklı bir kaynak grubuna geçirirseniz bu sanal makine için koruma ayarları kesilir.
   - **Kullanılabilirlik kümesi**: sanal makinenizin yük devretmenin bir parçası olması gerekiyorsa bir [kullanılabilirlik kümesi](../virtual-machines/windows/tutorial-availability-sets.md) seçebilirsiniz. Bir kullanılabilirlik kümesi seçtiğinizde, aşağıdaki bilgileri aklınızda bulundurun:
     - Yalnızca belirtilen kaynak grubuna ait kullanılabilirlik kümeleri listelenir.
     - Farklı sanal ağlardaki VM 'Ler aynı Kullanılabilirlik kümesinin parçası olamaz.
     - Yalnızca aynı boyuttaki sanal makineler bir kullanılabilirlik kümesinin parçası olabilir.

1. Hedef ağ, alt ağ ve Azure VM 'sine atanan IP adresi hakkında bilgi de ekleyebilirsiniz.
1. **Diskler**' de, çoğaltılacak sanal makinede işletim sistemi ve veri disklerini görebilirsiniz.

### <a name="configure-networks-and-ip-addresses"></a>Ağları ve IP adreslerini yapılandırma

Hedef IP adresini ayarlayabilirsiniz:

- Bir adres sağlamazsanız, yük devredilen VM DHCP kullanır.
- Yük devretmede kullanılamayan bir adres ayarlarsanız, yük devretme çalışmaz.
- Adres, yük devretme testi ağında kullanılabiliyorsa, yük devretme testi için aynı hedef IP adresini kullanabilirsiniz.

Ağ bağdaştırıcılarının sayısı, hedef sanal makine için belirttiğiniz boyuta göre dikte edilir ve aşağıdaki gibi:

- Kaynak sanal makinedeki ağ bağdaştırıcılarının sayısı, hedef VM 'nin boyutu için izin verilen bağdaştırıcıların sayısından küçük veya bu sayıya eşitse, hedef, kaynak ile aynı sayıda bağdaştırıcıya sahiptir.
- Kaynak sanal makinenin bağdaştırıcı sayısı hedef VM 'nin boyutu için izin verilen sayıyı aşarsa, en fazla hedef boyutu kullanılır. Örneğin, bir kaynak sanal makinede iki ağ bağdaştırıcısı varsa ve hedef VM boyutu dördü destekliyorsa, hedef sanal makinenin iki bağdaştırıcısı vardır. Kaynak VM 'nin iki bağdaştırıcısı varsa ancak hedef boyutu yalnızca birini destekliyorsa, hedef VM 'nin yalnızca bir bağdaştırıcısı vardır.
- Sanal makinenin birden çok ağ bağdaştırıcısı varsa, hepsi aynı ağa bağlanır. Ayrıca, listede gösterilen ilk bağdaştırıcı, Azure sanal makinesinde varsayılan ağ bağdaştırıcısı olur.

### <a name="azure-hybrid-benefit"></a>Azure Hibrit Avantajı

Microsoft yazılım güvencesi müşterileri, Azure 'a geçirilen Windows Server bilgisayarları için lisanslama maliyetlerinden tasarruf etmek üzere Azure Hibrit Avantajı kullanabilir. Bu avantaj Ayrıca Azure olağanüstü durum kurtarma için de geçerlidir. Uygunsanız, bir yük devretme işlemi varsa Site Recovery oluşturduğu sanal makineye avantajınızı atayabilirsiniz.

1. Çoğaltılan sanal makinenin **bilgisayar ve ağ özelliklerine** gidin.
1. Azure Hibrit Avantajı uygun hale getirmenizi sağlayan bir Windows Server lisansınız olup olmadığı sorulduğunda yanıtlayın.
1. Yük devretmede oluşturulacak VM 'nin avantajını uygulamak için kullanabileceğiniz, Yazılım Güvencesi kapsamındaki uygun bir Windows Server lisansınızın olduğunu doğrulayın.
1. Çoğaltılan sanal makinenin ayarlarını kaydedin.

Azure Hibrit Avantajı hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/hybrid-benefit/) .

## <a name="next-steps"></a>Sonraki adımlar

Sanal makine korumalı duruma ulaştıktan sonra uygulamanızın Azure 'da görünüp görüntülenmediğini denetlemek için bir [Yük devretme](site-recovery-failover.md) işlemi deneyin.

- Çoğaltmayı devre dışı bırakmak için kayıt ve koruma ayarlarını temizleme hakkında [daha fazla bilgi edinin](site-recovery-manage-registration-and-protection.md) .
- PowerShell kullanarak sanal makineleriniz için çoğaltmayı otomatikleştirme hakkında [daha fazla bilgi edinin](vmware-azure-disaster-recovery-powershell.md) .
