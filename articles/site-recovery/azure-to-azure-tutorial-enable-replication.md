---
title: Azure Site Recovery ile Azure VM olağanüstü durum kurtarmayı ayarlama
description: Azure Site Recovery (Önizleme) ile Azure VM’leri için ikincil Azure bölgesine olağanüstü durum kurtarmanın nasıl ayarlanacağını öğrenin.
ms.topic: tutorial
ms.date: 1/24/2020
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: e7f7535cf66da721e1738da6d0efbf335d97a6da
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86134503"
---
# <a name="set-up-disaster-recovery-for-azure-vms"></a>Azure VM 'Leri için olağanüstü durum kurtarmayı ayarlama

[Azure Site Recovery](site-recovery-overview.md) hizmeti, şirket içi makinelerin ve Azure sanal makinelerinin (VM) çoğaltma, yük devretme ve yeniden çalışma işlemlerini yönetip düzenleyerek olağanüstü durum kurtarma stratejinize katkıda bulunur.

Bu öğreticide, Azure sanal makineleri için bir Azure bölgesinden diğerine çoğaltılarak olağanüstü durum kurtarmayı ayarlama konusu gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kurtarma Hizmetleri kasası oluşturma
> * Hedef kaynak ayarlarını doğrulama
> * VM 'Ler için giden ağ bağlantısını ayarlama
> * Sanal makine için çoğaltmayı etkinleştirme

> [!NOTE]
> Bu makale, en basit ayarlarla olağanüstü durum kurtarma dağıtımı için yönergeler sağlar. Özelleştirilmiş ayarlar hakkında bilgi edinmek istiyorsanız, [nasıl yapılır bölümündeki](azure-to-azure-how-to-enable-replication.md)makaleleri gözden geçirin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- [Senaryo mimarisini ve bileşenlerini](./azure-to-azure-architecture.md) gözden geçirin.
- Başlamadan önce [destek gereksinimlerini](./azure-to-azure-support-matrix.md) gözden geçirin.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kaynak bölgesi dışında herhangi bir bölgede kasayı oluşturun.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin. Ardından, **Yönetim Araçları**  >  **yedeklemesi ve Site Recovery**& seçin.
1. **Ad**alanında, kasayı tanımlamak için bir kolay ad belirtin. Birden fazla aboneliğiniz varsa uygun olanı seçin.
1. Kaynak grubu oluşturun veya var olan bir grubu seçin. Bir Azure bölgesi belirtin. Desteklenen bölgeleri kontrol etmek için [Azure Site Recovery Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/) bölümündeki coğrafi kullanılabilirlik kısmına bakın.
1. Panodan kasaya erişmek için **panoya sabitle** ' yi seçin ve ardından **Oluştur**' u seçin.

   ![Yeni kasa](./media/azure-to-azure-tutorial-enable-replication/new-vault-settings.png)

Yeni kasa, **Pano**’da **Tüm kaynaklar** bölümüne ve ana **Kurtarma Hizmetleri kasaları** sayfasına eklenir.

## <a name="verify-target-resource-settings"></a>Hedef kaynak ayarlarını doğrulama

Hedef bölge için Azure aboneliğinizi denetleyin.

- Azure aboneliğinizin hedef bölgede VM 'Ler oluşturmanıza izin verdiğini doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.
- Aboneliğinizin, kaynak VM 'larınızla eşleşen VM boyutlarını desteklemek için yeterli kaynağa sahip olduğundan emin olun. Site Recovery, hedef VM için aynı boyutu veya mümkün olan en yakın boyutu seçer.

## <a name="set-up-outbound-network-connectivity-for-vms"></a>VM 'Ler için giden ağ bağlantısını ayarlama

Site Recovery beklendiği gibi çalışması için, çoğaltmak istediğiniz VM 'lerden giden ağ bağlantısını değiştirmeniz gerekir.

> [!NOTE]
> Site Recovery, ağ bağlantısını denetlemek için bir kimlik doğrulama proxy 'si kullanmayı desteklemez.

### <a name="outbound-connectivity-for-urls"></a>URL'ler için giden bağlantı

Giden bağlantıyı denetlemek için URL tabanlı bir güvenlik duvarı proxy 'si kullanıyorsanız, bu URL 'Lere erişim izni verin:

| **URL** | **Ayrıntılar** |
| ------- | ----------- |
| `*.blob.core.windows.net` | Verilerin VM’den kaynak bölgedeki önbellek depolama hesabına yazılmasına izin verir. |
| `login.microsoftonline.com` | Site Recovery hizmet URL’leri için yetkilendirme ve kimlik doğrulama özellikleri sağlar. |
| `*.hypervrecoverymanager.windowsazure.com` | VM’nin Site Recovery hizmetiyle iletişim kurmasına izin verir. |
| `*.servicebus.windows.net` | VM’nin Site Recovery izleme ve tanılama verilerini yazmasına izin verir. |

### <a name="outbound-connectivity-for-ip-address-ranges"></a>IP adresi aralıkları için giden bağlantı

Bir ağ güvenlik grubu (NSG) kullanıyorsanız, Azure depolama, Azure Active Directory, Site Recovery hizmeti ve Site Recovery izlemeye erişim için hizmet etiketi tabanlı NSG kuralları oluşturun. [Daha fazla bilgi edinin](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags).

## <a name="verify-azure-vm-certificates"></a>Azure VM sertifikalarını doğrulama

Çoğaltmak istediğiniz VM 'Lerin en son kök sertifikalara sahip olup olmadığını denetleyin. Aksi takdirde, güvenlik kısıtlamaları nedeniyle sanal makine Site Recovery kaydedilemez.

- Windows VM’ler için, güvenilir kök sertifikaların tamamı makinede mevcut olacak şekilde sanal makineye en son Windows güncelleştirmelerinin tümünü yükleyin. Bağlantısı kesilmiş bir ortamda, kuruluşunuz için standart Windows Update ve sertifika güncelleştirme işlemlerini uygulayın.
- Linux VM'lerde, VM’deki en son güvenilen kök sertifikalarını ve sertifika iptal listesini almak için Linux dağıtıcınız tarafından sağlanan yönergeleri uygulayın.

## <a name="set-permissions-on-the-account"></a>Hesapta izinleri ayarlama

Azure Site Recovery, Site Recovery yönetim işlemlerini denetlemek için üç yerleşik rol sağlar.

- **Site Recovery Katkıda Bulunanı** - Bu rol, Kurtarma Hizmetleri kasasında Site Recovery işlemlerini yönetmek için gereken tüm izinlere sahiptir. Ancak bu role sahip kullanıcı, Kurtarma Hizmetleri kasasını oluşturamaz veya silemez ya da diğer kullanıcılara erişim hakkı atayamaz. Bu rol, uygulamalar veya kuruluşların tamamı için olağanüstü durum kurtarmayı etkinleştirebilen ve yönetebilen olağanüstü durum kurtarma yöneticileri için idealdir.

- **Site Recovery Operatörü** - Bu rol, Yük Devretme ve Yeniden Çalışma işlemlerini yürütme ve yönetme izinlerine sahiptir. Bu role sahip olan kullanıcı çoğaltmayı etkinleştirip devre dışı bırakabilir, kasaları oluşturup silebilir, yeni altyapılar kaydedebilir veya diğer kullanıcılara erişim hakkı atayabilir. Bu rol, uygulama sahipleri ve BT yöneticileri tarafından istendiğinde sanal makinelerin veya uygulamaların yükünü devredebilen olağanüstü durum kurtarma operatörü için idealdir. Olağanüstü durum kurtarma operatörü, olağanüstü durum çözümlemesi sonrasında sanal makineleri yeniden koruyabilir ve yeniden gerçekleştirebilir.

- **Site Recovery Okuyucusu** - Bu rol tüm Site Recovery yönetim işlemlerini görüntüleme iznine sahiptir. Bu rol, mevcut koruma durumunu izleyebilen ve destek biletleri oluşturabilen BT izleme yöneticisi için idealdir.

[Azure RBAC yerleşik rolleri](../role-based-access-control/built-in-roles.md)hakkında daha fazla bilgi edinin.

## <a name="enable-replication-for-a-vm"></a>Sanal makine için çoğaltmayı etkinleştirme

Aşağıdaki bölümlerde, çoğaltmanın nasıl etkinleştirileceği açıklanır.

### <a name="select-the-source"></a>Kaynağı seçme

Çoğaltma Kurulumu 'nu başlatmak için Azure sanal makinelerinizin çalıştığı kaynağı seçin.

1. **Kurtarma Hizmetleri kasaları**' ne gidin, kasa adını seçin ve **+ Çoğalt**' ı seçin.
1. **Kaynak**için **Azure**' u seçin.
1. **Kaynak konumu**’nda, VM’lerinizin çalışmakta olduğu kaynak Azure bölgesini seçin.
1. Sanal makinelerin çalıştığı **Kaynak aboneliği** seçin. Bu abonelik, kurtarma hizmetleri kasanızın bulunduğu Azure Active Directory kiracısında bulunan aboneliklerden biri olabilir.
1. **Kaynak kaynak grubunu**seçin ve ayarları kaydetmek için **Tamam** ' ı seçin.

   ![Kaynağı ayarlama](./media/azure-to-azure-tutorial-enable-replication/source.png)

### <a name="select-the-vms"></a>VM’leri seçme

Site Recovery, abonelik ve kaynak grup/bulut hizmeti ile ilişkili VM’lerin listesini alır.

1. **Sanal Makineler** bölümünde çoğaltmak istediğiniz VM’leri seçin.
1. **Tamam**’ı seçin.

### <a name="configure-replication-settings"></a>Çoğaltma ayarlarını yapılandırma

Site Recovery, hedef bölge için varsayılan ayarları ve çoğaltma ilkesini oluşturur. Bu ayarları gerektiği gibi değiştirebilirsiniz.

1. Hedef ve çoğaltma ayarlarını görüntülemek için **Ayarlar** ' ı seçin.

1. Varsayılan hedef ayarlarını geçersiz kılmak için **kaynak grubu, ağ, depolama ve kullanılabilirlik**' ın yanındaki **Özelleştir** ' i seçin.

   ![Ayarları yapılandırma](./media/azure-to-azure-tutorial-enable-replication/settings.png)

1. Hedef ayarları tabloda özetlenen şekilde özelleştirin.

   | **Ayar** | **Ayrıntılar** |
   | --- | --- |
   | **Hedef abonelik** | Varsayılan olarak, hedef abonelik, kaynak abonelikle aynı olur. Aynı Azure Active Directory kiracısında farklı bir hedef abonelik seçmek için **Özelleştir** ' i seçin. |
   | **Hedef konum** | Olağanüstü durum kurtarma için kullanılan hedef bölge.<br/><br/> Hedef konumun Site Recovery kasasının konumuyla eşleşmesini öneririz. |
   | **Hedef kaynak grubu** | Yük devretmeden sonra Azure VM 'Leri tutan hedef bölgedeki kaynak grubu.<br/><br/> Varsayılan olarak, Site Recovery hedef bölgede bir sonek ile yeni bir kaynak grubu oluşturur `asr` . Hedef kaynak grubunun konumu, kaynak sanal makinelerinizin barındırıldığı bölge dışında herhangi bir bölge olabilir. |
   | **Hedef sanal ağ** | Hedef bölgedeki sanal makineler, yük devretmeden sonra bulunur.<br/><br/> Site Recovery, varsayılan olarak hedef bölgede bir sonek ile yeni bir sanal ağ (ve alt ağlar) oluşturur `asr` . |
   | **Önbellek depolama hesapları** | Site Recovery, kaynak bölgedeki bir depolama hesabını kullanır. Kaynak VM’lere yönelik değişiklikler, hedef konuma çoğaltılmadan önce bu hesaba gönderilir.<br/><br/> Güvenlik Duvarı etkinleştirilmiş önbellek depolama hesabı kullanıyorsanız, **Güvenilen Microsoft hizmetlerine Izin ver**' i etkinleştirdiğinizden emin olun. [Daha fazla bilgi edinin](../storage/common/storage-network-security.md#exceptions). Ayrıca, kaynak VNET 'in en az bir alt ağına erişime izin verdiğinizden emin olun. |
   | **Hedef depolama hesapları (kaynak VM, yönetilmeyen diskler kullanır)** | Varsayılan olarak Site Recovery, kaynak VM depolama hesabını yansıtmak için hedef bölgede yeni bir depolama hesabı oluşturur.<br/><br/> Güvenlik Duvarı etkinleştirilmiş önbellek depolama hesabı kullanıyorsanız **Güvenilen Microsoft hizmetlerine Izin ver** ' i etkinleştirin. |
   | **Yönetilen çoğaltma diskleri (kaynak VM yönetilen diskleri kullanıyorsa)** | Varsayılan olarak Site Recovery, kaynak VM 'nin yönetilen diskiyle aynı depolama türüyle (Standart veya Premium) kaynak sanal makinenin yönetilen disklerini yansıtmak için hedef bölgede çoğaltma tarafından yönetilen diskler oluşturur. Yalnızca disk türünü özelleştirebilirsiniz. |
   | **Hedef kullanılabilirlik kümeleri** | Varsayılan olarak, Azure Site Recovery hedef bölgede, `asr` kaynak bölgedeki bir kullanılabilirlik kümesinin VM 'leri bölümü için sonek içeren adı olan yeni bir kullanılabilirlik kümesi oluşturur. Azure Site Recovery tarafından oluşturulan kullanılabilirlik kümesi zaten mevcut olduğunda, yeniden kullanılır. |
   | **Hedef kullanılabilirlik alanları** | Varsayılan olarak, hedef bölge kullanılabilirlik bölgelerini destekliyorsa, Site Recovery hedef bölgedeki kaynak bölgeyle aynı bölge numarasını atar.<br/><br/> Hedef bölge kullanılabilirlik bölgelerini desteklemiyorsa, hedef VM 'Ler varsayılan olarak tek örnek olarak yapılandırılır.<br/><br/> VM 'Leri hedef bölgedeki bir kullanılabilirlik kümesinin parçası olarak yapılandırmak için **Özelleştir** ' i seçin.<br/><br/> Çoğaltmayı etkinleştirdikten sonra kullanılabilirlik türünü (tek örnek, kullanılabilirlik kümesi veya kullanılabilirlik bölgesi) değiştiremezsiniz. Kullanılabilirlik türünü değiştirmek için çoğaltmayı devre dışı bırakıp etkinleştirin. |

1. Çoğaltma İlkesi ayarlarını özelleştirmek için, **Çoğaltma İlkesi**' nin yanındaki **Özelleştir** ' i seçin ve ayarları gerektiği gibi değiştirin.

   | **Ayar** | **Ayrıntılar** |
   | --- | --- |
   | **Çoğaltma ilkesi adı** | İlke adı. |
   | **Kurtarma noktası bekletme** | Varsayılan olarak, Site Recovery kurtarma noktalarını 24 saat korur. 1 ile 72 saat arasında bir değer yapılandırabilirsiniz. |
   | **Uygulamayla tutarlı anlık görüntü sıklığı** | Varsayılan olarak, Site Recovery her 4 saatte bir uygulamayla tutarlı bir anlık görüntü alır. 1 ile 12 saat arasında bir değer yapılandırabilirsiniz.<br/><br/> Uygulamayla tutarlı bir anlık görüntü, VM içindeki uygulama verilerinin zaman içinde bir noktadaki anlık görüntüsüdür. Birim Gölge Kopyası Hizmeti (VSS), anlık görüntü alınırken VM’deki uygulamanın tutarlı bir durumda olmasını sağlar. |
   | **Çoğaltma grubu** | Uygulamanızın VM 'lerde çoklu VM tutarlılığı gerekiyorsa, bu VM 'Ler için bir çoğaltma grubu oluşturabilirsiniz. Seçilen VM’ler varsayılan olarak hiçbir çoğaltma grubunun parçası değildir. |

1. Yeni veya mevcut bir çoğaltma grubuna VM 'Ler eklemek istiyorsanız, **Özelleştir**' de, çoklu VM tutarlılığı için **Evet** ' i seçin. Ardından **Tamam**'ı seçin.

   > [!NOTE]
   > - Bir çoğaltma grubundaki tüm makineler, yük devredildiği zaman, paylaşılan kilitlenme ile tutarlı ve uygulamayla tutarlı kurtarma noktalarına sahiptir.
   > - Çoklu VM tutarlılığını etkinleştirmek, iş yükü performansını etkileyebilir (CPU kullanımı yoğun). Yalnızca makineler aynı iş yükünü çalıştırıyorsa ve birden çok makine arasında tutarlılık olması durumunda kullanılmalıdır.
   > - Bir çoğaltma grubunda en fazla 16 VM olabilir.
   > - Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 numaralı bağlantı noktası üzerinden iletişim kurar. Bu bağlantı noktası üzerinden VM 'Ler arasındaki iç iletişimi engelleyen bir güvenlik duvarı olmadığından emin olun.
   > - Bir çoğaltma grubundaki Linux sanal makineleri için, 20004 numaralı bağlantı noktasındaki giden trafiğin Linux sürümüne yönelik kılavuza göre el ile açık olduğundan emin olun.

### <a name="configure-encryption-settings"></a>Şifreleme ayarlarını yapılandırma

Kaynak VM 'de Azure disk şifrelemesi (ADE) etkinse ayarları gözden geçirin.

1. Ayarları doğrulayın:
   1. **Disk şifreleme anahtarı kasaları**: varsayılan olarak Site Recovery, kaynak VM disk şifrelemesi anahtarlarında bir sonek ile yeni bir Anahtar Kasası oluşturur `asr` . Anahtar Kasası zaten varsa, yeniden kullanılır.
   1. **Anahtar şifreleme anahtarı kasaları**: varsayılan olarak Site Recovery, hedef bölgede yeni bir Anahtar Kasası oluşturur. Ad bir sonekine sahiptir `asr` ve kaynak VM anahtar şifreleme anahtarlarını temel alır. Site Recovery tarafından oluşturulan Anahtar Kasası zaten varsa, yeniden kullanılır.
1. **Özelleştir** ' i seçerek özel anahtar kasaları seçin.

>[!NOTE]
> Site Recovery Şu anda Windows işletim sistemlerini çalıştıran VM 'Ler için Azure Active Directory (AAD) ile ve olmadan ADE 'yi desteklemektedir. Linux işletim sistemleri için yalnızca AAD olmadan ADE 'yi destekliyoruz. Ayrıca, ADE 1,1 çalıştıran makineler (AAD olmadan) için VM 'Lerin yönetilen diskleri kullanıyor olması gerekir. Yönetilmeyen disklere sahip VM 'Ler desteklenmez. ADE 0,1 ' den (AAD ile) 1,1 ' e geçiş yaparsanız, bir sanal makine için çoğaltmayı devre dışı bırakmanız ve ' i etkinleştirdikten 1,1 sonra çoğaltmayı etkinleştirmeniz gerekir.

### <a name="track-replication-status"></a>Çoğaltma durumunu izleme

Çoğaltma etkinleştirildikten sonra işin durumunu izleyebilirsiniz.

1. **Ayarlar**' da en son durumu almak için **Yenile** ' yi seçin.
1. İlerleme durumunu ve durumu aşağıdaki gibi izleyin:
   1. **Enable protection** **Ayarlar**  >  **işleri**  >  **Site Recovery işler**' de korumayı etkinleştirme işinin ilerlemesini izleyin.
   1. **Ayarlar**  >  **çoğaltılan öğeler**' de, VM 'lerin durumunu ve ilk çoğaltma ilerlemesini görebilirsiniz. Ayarlarının detayına gitmek için VM 'yi seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure VM’si için olağanüstü durum kurtarmayı yapılandırdınız. Artık yük devretmenin beklendiği gibi çalışıp çalışmadığını denetlemek için bir olağanüstü durum kurtarma detayına sahip olabilirsiniz.

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](azure-to-azure-tutorial-dr-drill.md)
