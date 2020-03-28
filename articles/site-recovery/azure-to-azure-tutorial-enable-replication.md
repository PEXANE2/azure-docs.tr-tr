---
title: Azure Site Kurtarma ile Azure VM olağanüstü durum kurtarma yı ayarlama
description: Azure Site Recovery (Önizleme) ile Azure VM’leri için ikincil Azure bölgesine olağanüstü durum kurtarmanın nasıl ayarlanacağını öğrenin.
ms.topic: tutorial
ms.date: 1/24/2020
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: c5d2bbe920f87421550fadf30a7e7e9d23931bfd
ms.sourcegitcommit: fab450a18a600d72b583ecfbe6c5e53afd43408c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80292476"
---
# <a name="set-up-disaster-recovery-for-azure-vms"></a>Azure VM'leri için olağanüstü durum kurtarma ayarlama

[Azure Site Kurtarma](site-recovery-overview.md) hizmeti, şirket içi makineleri ve Azure sanal makinelerini (VM) yöneterek ve düzenleyerek olağanüstü durum kurtarma stratejinize katkıda bulunur.

Bu öğretici, Azure VM'leri bir Azure bölgesinden diğerine kopyalayarak olağanüstü durum kurtarmayı nasıl ayarlayabileceğinizi gösterir. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Kurtarma Hizmetleri kasası oluşturma
> * Hedef kaynak ayarlarını doğrulama
> * VM'ler için giden ağ bağlantısı ayarlama
> * Sanal makine için çoğaltmayı etkinleştirme

> [!NOTE]
> Bu makalede, en basit ayarlarla olağanüstü durum kurtarma dağıtma yönergeleri sağlar. Özelleştirilmiş ayarlar hakkında bilgi edinmek istiyorsanız, [Nasıl Yapılır bölümündeki](azure-to-azure-how-to-enable-replication.md)makaleleri inceleyin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- [Senaryo mimarisini ve bileşenlerini](concepts-azure-to-azure-architecture.md) gözden geçirin.
- Başlamadan önce [destek gereksinimlerini](site-recovery-support-matrix-azure-to-azure.md) gözden geçirin.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kaynak bölgesi dışında herhangi bir bölgede kasayı oluşturun.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin. Ardından, **BT & Yönetim Araçları** > **Yedekleme ve Site Kurtarma'yı**seçin.
1. **Ad,** tonoz tanımlamak için bir dostu ad belirtin. Birden fazla aboneliğiniz varsa uygun olanı seçin.
1. Kaynak grubu oluşturun veya var olan bir grubu seçin. Bir Azure bölgesi belirtin. Desteklenen bölgeleri kontrol etmek için [Azure Site Recovery Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/) bölümündeki coğrafi kullanılabilirlik kısmına bakın.
1. Panodan kasaya erişmek **için panoya Sabitle'yi** seçin ve ardından **Oluştur'u**seçin.

   ![Yeni kasa](./media/azure-to-azure-tutorial-enable-replication/new-vault-settings.png)

Yeni kasa, **Pano**’da **Tüm kaynaklar** bölümüne ve ana **Kurtarma Hizmetleri kasaları** sayfasına eklenir.

## <a name="verify-target-resource-settings"></a>Hedef kaynak ayarlarını doğrulama

Hedef bölge için Azure aboneliğinizi kontrol edin.

- Azure aboneliğinizin hedef bölgede VM'ler oluşturmanıza izin verdiğini doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.
- Aboneliğinizin kaynak VM'lerinizle eşleşen VM boyutlarını destekleyecek yeterli kaynağa sahip olduğundan emin olun. Site Kurtarma, hedef VM için aynı boyutu veya mümkün olan en yakın boyutu seçer.

## <a name="set-up-outbound-network-connectivity-for-vms"></a>VM'ler için giden ağ bağlantısı ayarlama

Site Kurtarma'nın beklendiği gibi çalışması için, çoğaltmak istediğiniz VM'lerden giden ağ bağlantısını değiştirmeniz gerekir.

> [!NOTE]
> Site Kurtarma, ağ bağlantısını denetlemek için kimlik doğrulama proxy'si kullanmayı desteklemez.

### <a name="outbound-connectivity-for-urls"></a>URL'ler için giden bağlantı

Giden bağlantıyı denetlemek için URL tabanlı bir güvenlik duvarı proxy'si kullanıyorsanız, bu URL'lere erişmeye izin verin:

| **URL** | **Şey** |
| ------- | ----------- |
| `*.blob.core.windows.net` | Verilerin VM’den kaynak bölgedeki önbellek depolama hesabına yazılmasına izin verir. |
| `login.microsoftonline.com` | Site Recovery hizmet URL’leri için yetkilendirme ve kimlik doğrulama özellikleri sağlar. |
| `*.hypervrecoverymanager.windowsazure.com` | VM’nin Site Recovery hizmetiyle iletişim kurmasına izin verir. |
| `*.servicebus.windows.net` | VM’nin Site Recovery izleme ve tanılama verilerini yazmasına izin verir. |

### <a name="outbound-connectivity-for-ip-address-ranges"></a>IP adresi aralıkları için giden bağlantı

Bir ağ güvenlik grubu (NSG) kullanıyorsanız, Azure Depolama, Azure Etkin Dizini, Site Kurtarma hizmeti ve Site Kurtarma izleme hizmetlerine erişmek için hizmet etiketi tabanlı NSG kuralları oluşturun. [Daha fazla bilgi edinin](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags).

## <a name="verify-azure-vm-certificates"></a>Azure VM sertifikalarını doğrulama

Çoğaltmak istediğiniz VM'lerin en son kök sertifikalarına sahip olup olmadığını denetleyin. Bunu yapmazlarsa, Güvenlik kısıtlamaları nedeniyle VM Site Kurtarma'ya kaydedilemez.

- Windows VM’ler için, güvenilir kök sertifikaların tamamı makinede mevcut olacak şekilde sanal makineye en son Windows güncelleştirmelerinin tümünü yükleyin. Bağlantısı kesilmiş bir ortamda, kuruluşunuz için standart Windows Update ve sertifika güncelleştirme işlemlerini uygulayın.
- Linux VM'lerde, VM’deki en son güvenilen kök sertifikalarını ve sertifika iptal listesini almak için Linux dağıtıcınız tarafından sağlanan yönergeleri uygulayın.

## <a name="set-permissions-on-the-account"></a>Hesapta izinleri ayarlama

Azure Site Recovery, Site Recovery yönetim işlemlerini denetlemek için üç yerleşik rol sağlar.

- **Site Recovery Katkıda Bulunanı** - Bu rol, Kurtarma Hizmetleri kasasında Site Recovery işlemlerini yönetmek için gereken tüm izinlere sahiptir. Ancak bu role sahip kullanıcı, Kurtarma Hizmetleri kasasını oluşturamaz veya silemez ya da diğer kullanıcılara erişim hakkı atayamaz. Bu rol, uygulamalar veya kuruluşların tamamı için olağanüstü durum kurtarmayı etkinleştirebilen ve yönetebilen olağanüstü durum kurtarma yöneticileri için idealdir.

- **Site Recovery Operatörü** - Bu rol, Yük Devretme ve Yeniden Çalışma işlemlerini yürütme ve yönetme izinlerine sahiptir. Bu role sahip olan kullanıcı çoğaltmayı etkinleştirip devre dışı bırakabilir, kasaları oluşturup silebilir, yeni altyapılar kaydedebilir veya diğer kullanıcılara erişim hakkı atayabilir. Bu rol, uygulama sahipleri ve BT yöneticileri tarafından istendiğinde sanal makinelerin veya uygulamaların yükünü devredebilen olağanüstü durum kurtarma operatörü için idealdir. Felaketin çözümü nden sonra, olağanüstü durum kurtarma operatörü sanal makineleri yeniden koruyabilir ve geri alabilir.

- **Site Recovery Okuyucusu** - Bu rol tüm Site Recovery yönetim işlemlerini görüntüleme iznine sahiptir. Bu rol, mevcut koruma durumunu izleyebilen ve destek biletleri oluşturabilen BT izleme yöneticisi için idealdir.

[Azure RBAC yerleşik rolleri](../role-based-access-control/built-in-roles.md)hakkında daha fazla bilgi edinin.

## <a name="enable-replication-for-a-vm"></a>Sanal makine için çoğaltmayı etkinleştirme

Aşağıdaki bölümlerde çoğaltma nasıl etkinleştirilir açıklanmıştır.

### <a name="select-the-source"></a>Kaynağı seçme

Çoğaltma kurulumuna başlamak için Azure VM'lerinizin çalıştığı kaynağı seçin.

1. Kurtarma **Hizmetleri kasalarına**gidin, kasa adını seçin ve **ardından +Çoğaltma'yı**seçin.
1. **Kaynak**için **Azure'u**seçin.
1. **Kaynak konumu**’nda, VM’lerinizin çalışmakta olduğu kaynak Azure bölgesini seçin.
1. Sanal makinelerin çalıştığı **Kaynak aboneliği** seçin. Bu abonelik, kurtarma hizmetleri kasanızın bulunduğu Azure Active Directory kiracısında bulunan aboneliklerden biri olabilir.
1. Kaynak **kaynak grubunu**seçin ve ayarları kaydetmek için **Tamam'ı** seçin.

   ![Kaynağı ayarlama](./media/azure-to-azure-tutorial-enable-replication/source.png)

### <a name="select-the-vms"></a>VM’leri seçme

Site Recovery, abonelik ve kaynak grup/bulut hizmeti ile ilişkili VM’lerin listesini alır.

1. **Sanal Makineler** bölümünde çoğaltmak istediğiniz VM’leri seçin.
1. **Tamam'ı**seçin.

### <a name="configure-replication-settings"></a>Çoğaltma ayarlarını yapılandırma

Site Recovery, hedef bölge için varsayılan ayarları ve çoğaltma ilkesini oluşturur. Bu ayarları gerektiği gibi değiştirebilirsiniz.

1. Hedef ve çoğaltma ayarlarını görüntülemek için **Ayarlar'ı** seçin.

1. Varsayılan hedef ayarlarını geçersiz kılmak için **Kaynak grubu, Ağ, Depolama ve Kullanılabilirlik'in**yanında **Özelleştir'i** seçin.

   ![Ayarları yapılandırma](./media/azure-to-azure-tutorial-enable-replication/settings.png)

1. Tabloda özetlendiği gibi hedef ayarlarını özelleştirin.

   | **Ayar** | **Şey** |
   | --- | --- |
   | **Hedef abonelik** | Varsayılan olarak, hedef abonelik kaynak abonelikle aynıdır. Aynı **Customize** Azure Etkin Dizin kiracısı içinde farklı bir hedef abonelik seçmek için Özelleştir'i'ni seçin. |
   | **Hedef konum** | Felaket kurtarma için kullanılan hedef bölge.<br/><br/> Hedef konumun Site Recovery kasasının konumuyla eşleşmesini öneririz. |
   | **Hedef kaynak grubu** | Başarısız olduktan sonra Azure VM'lerini tutan hedef bölgedeki kaynak grubu.<br/><br/> Varsayılan olarak, Site Kurtarma hedef bölgede sonek ile `asr` yeni bir kaynak grubu oluşturur. Hedef kaynak grubunun konumu, kaynak sanal makinelerinizin barındırıldığı bölge dışında herhangi bir bölge olabilir. |
   | **Hedef sanal ağ** | VM'lerin başarısız olduktan sonra bulunduğu hedef bölgedeki ağ.<br/><br/> Varsayılan olarak, Site Kurtarma hedef bölgede bir sonek ile yeni `asr` bir sanal ağ (ve alt ağlar) oluşturur. |
   | **Önbellek depolama hesapları** | Site Kurtarma kaynak bölgede bir depolama hesabı kullanır. Kaynak VM’lere yönelik değişiklikler, hedef konuma çoğaltılmadan önce bu hesaba gönderilir.<br/><br/> Güvenlik duvarı etkin leştirilmiş bir önbellek depolama hesabı kullanıyorsanız, **güvenilen Microsoft hizmetlerine İzin Ver'i etkinleştirdiğinizden**emin olun. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions). Ayrıca, kaynak Vnet'in en az bir alt ağına erişime izin verdiğinden emin olun. |
   | **Hedef depolama hesapları (kaynak VM yönetilmeyen diskleri kullanır)** | Varsayılan olarak, Site Kurtarma kaynak VM depolama hesabını yansıtmak için hedef bölgede yeni bir depolama hesabı oluşturur.<br/><br/> Güvenlik duvarı etkin bir önbellek depolama hesabı kullanıyorsanız **güvenilen Microsoft hizmetlerine İzin Ver'i** etkinleştirin. |
   | **Çoğaltma yönetilen diskler (Kaynak VM yönetilen diskleri kullanıyorsa)** | Varsayılan olarak, Site Kurtarma, kaynak VM'nin yönetilen diskiyle aynı depolama türüne (standart veya premium) sahip kaynak VM yönetilen diskleri yansıtmak için hedef bölgede yineleme yönetilen diskler oluşturur. Yalnızca Disk türünü özelleştirebilirsiniz. |
   | **Hedef kullanılabilirlik kümeleri** | Varsayılan olarak, Azure Site Kurtarma, kaynak bölgede ayarlanan `asr` kullanılabilirlik bölümünün VM'ler bölümü için ad soneki içeren hedef bölgede yeni bir kullanılabilirlik kümesi oluşturur. Azure Site Kurtarma tarafından oluşturulan kullanılabilirlik kümesi zaten varsa, yeniden kullanılır. |
   | **Hedef kullanılabilirlik bölgeleri** | Varsayılan olarak, Hedef Bölge kullanılabilirlik bölgelerini destekliyorsa, Site Kurtarma hedef bölgedeki kaynak bölgeyle aynı bölge numarasını atar.<br/><br/> Hedef bölge kullanılabilirlik bölgelerini desteklemiyorsa, hedef VM'ler varsayılan olarak tek örnek olarak yapılandırılır.<br/><br/> Hedef **Customize** bölgede ayarlanan kullanılabilirlik kümesinin bir parçası olarak VM'leri yapılandırmak için Özelleştir'i'ni seçin.<br/><br/> Çoğaltmayı etkinleştirdikten sonra kullanılabilirlik türünü (tek örnek, kullanılabilirlik kümesi veya kullanılabilirlik bölgesi) değiştiremezsiniz. Kullanılabilirlik türünü değiştirmek için, devre dışı kalın ve çoğaltmayı etkinleştirin. |

1. Çoğaltma ilkesi ayarlarını özelleştirmek için **Çoğaltma ilkesinin**yanında **Özelleştir'i** seçin ve ayarları gerektiği gibi değiştirin.

   | **Ayar** | **Şey** |
   | --- | --- |
   | **Çoğaltma ilkesi adı** | İlke adı. |
   | **Kurtarma noktası tutma** | Varsayılan olarak, Site Kurtarma kurtarma noktalarını 24 saat boyunca tutar. 1 ile 72 saat arasında bir değer yapılandırabilirsiniz. |
   | **Uygulama tutarlı anlık görüntü frekansı** | Varsayılan olarak, Site Kurtarma her 4 saatte bir uygulama tutarlı bir anlık görüntü alır. 1 ile 12 saat arasında bir değer yapılandırabilirsiniz.<br/><br/> Uygulama tutarlı anlık görüntüsü, VM içindeki uygulama verilerinin anlık görüntüsüdür. Birim Gölge Kopyası Hizmeti (VSS), anlık görüntü alınırken VM’deki uygulamanın tutarlı bir durumda olmasını sağlar. |
   | **Çoğaltma grubu** | Uygulamanızın VM'ler arasında çoklu VM tutarlılığına ihtiyacı varsa, bu VM'ler için bir çoğaltma grubu oluşturabilirsiniz. Seçilen VM’ler varsayılan olarak hiçbir çoğaltma grubunun parçası değildir. |

1. **Özelleştir'de,** yeni veya varolan çoğaltma grubuna VM eklemek istiyorsanız çoklu VM tutarlılığı için **Evet'i** seçin. Sonra **Tamam**’ı seçin.

   > [!NOTE]
   > - Çoğaltma grubundaki tüm makineler, başarısız olduğunda kilitlenme tutarlı ve uygulama tutarlı kurtarma noktalarını paylaştı.
   > - Çoklu VM tutarlılığını etkinleştirmek iş yükü performansını etkileyebilir (CPU yoğundur). Yalnızca makineler aynı iş yükünü çalıştırıyorsa ve birden çok makinede tutarlılık gerekiyorsa kullanılmalıdır.
   > - Bir çoğaltma grubunda en fazla 16 VM'ye sahip olabilirsiniz.
   > - Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 numaralı bağlantı noktası üzerinden iletişim kurar. Bu bağlantı noktası üzerindeki VM'ler arasındaki iç iletişimi engelleyen güvenlik duvarı olmadığından emin olun.
   > - Bir çoğaltma grubundaki Linux VM'leri için, 20004 bağlantı noktasındaki giden trafiğin Linux sürümü kılavuzuna uygun olarak el ile açıldığından emin olun.

### <a name="configure-encryption-settings"></a>Şifreleme ayarlarını yapılandırma

Kaynak VM'de Azure disk şifrelemesi (ADE) etkinleştirilmişse, ayarları gözden geçirin.

1. Ayarları doğrulayın:
   1. **Disk şifreleme anahtar kasaları**: Varsayılan olarak, Site Kurtarma, kaynak VM disk `asr` şifreleme anahtarları üzerinde sonek li yeni bir anahtar kasası oluşturur. Anahtar kasası zaten varsa, yeniden kullanılır.
   1. **Anahtar şifreleme anahtar kasaları**: Varsayılan olarak, Site Kurtarma hedef bölgede yeni bir anahtar kasası oluşturur. Adın bir `asr` sonek vardır ve kaynak VM anahtar şifreleme anahtarlarını temel alınarak yapılır. Site Kurtarma tarafından oluşturulan anahtar kasası zaten varsa, yeniden kullanılır.
1. Özel anahtar tonozlarını seçmek için **Özelleştir'i** seçin.

> [!NOTE]
> Şu anda Yalnızca Windows işletim sistemleri çalıştıran ve [Azure AD uygulamasıyla şifreleme için etkinleştirilen](https://aka.ms/ade-aad-app) Azure VM'leri Azure Site Kurtarma tarafından desteklenir.

### <a name="track-replication-status"></a>Çoğaltma durumunu izleme

Çoğaltma etkinleştirildikten sonra, işin durumunu izleyebilirsiniz.

1. **Ayarlar'da,** en son durumu almak için **Yenile'yi** seçin.
1. İlerlemeyi ve durumu aşağıdaki gibi izleyin:
   1.  >  **Ayarlar** > **İşleri****Sitesi Kurtarma İşlerinde**Koruma Işini **Etkinleştir'in** ilerlemesini izleyin.
   1. Yinelenen**Ayarlar'da,** **Settings** > VM'lerin durumunu ve ilk çoğaltma ilerlemesini görüntüleyebilirsiniz. Ayarlarına sondaj yapmak için VM'yi seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure VM’si için olağanüstü durum kurtarmayı yapılandırdınız. Şimdi bu failover beklendiği gibi çalıştığını kontrol etmek için bir felaket kurtarma matkap çalıştırabilirsiniz.

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](azure-to-azure-tutorial-dr-drill.md)
