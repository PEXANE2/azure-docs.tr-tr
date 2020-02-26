---
title: İşleç en iyi uygulamalar - Azure Kubernetes Hizmetleri (AKS) depolama
description: Depolama, veri şifreleme ve yedekleri Azure Kubernetes Service (AKS) için küme işleci en iyi uygulamaları öğrenin
services: container-service
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 6521655ded45f0a1d15c3ec40a44993d757b8854
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77594677"
---
# <a name="best-practices-for-storage-and-backups-in-azure-kubernetes-service-aks"></a>Depolama ve yedekleme Azure Kubernetes Service (AKS) için en iyi uygulamalar

Oluşturma ve Azure Kubernetes Service (AKS) kümelerini yönetme gibi uygulamalarınızı genellikle depolama gerekir. Performans gereksinimlerini anlamak ve uygulamalara uygun depolama sağlayabilmesi için pod'ların yöntemlere erişmek önemlidir. AKS düğüm boyutunu bu depolama seçenekleri etkileyebilir. Yedekleme ve geri yükleme işlemi için bağlı depolama test yollarını planlamanız gerekir.

Bu en iyi yöntemler makalesi küme operatörleri için hazırlanan depolama konuları ele alınmaktadır. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Hangi tür depolama vardır
> * AKS düğümleri depolama performansı için doğru boyutu nasıl
> * Dinamik ve statik hacimlerdeki sağlama arasındaki farklar
> * Yedekleme ve veri birimlerinizi güvenli şekilde

## <a name="choose-the-appropriate-storage-type"></a>Uygun depolama türü seçin

**En iyi Yöntem Kılavuzu** -doğru depolamayı seçmek için uygulamanızın ihtiyaçlarını anlayın. Yüksek performanslı, SSD destekli depolamanın üretim iş yükleri için kullanın. Plan için birden çok eş zamanlı bağlantı ihtiyacı olduğunda ağ tabanlı depolama.

Uygulamalar genellikle farklı türler ve depolama saniyeye kadar düşürmeyi başardık gerektirir. Uygulamalarınızı tek pod'ların bağlayan veya birden çok podunuz arasında paylaşılan depolama gerekir mi? Depolama için yalnızca okuma erişimi veri veya büyük miktarlarda yapısal veriyi yazma için mi? Bu depolama, depolama kullanmak için en uygun türünü belirleme.

Aşağıdaki tabloda kullanılabilir depolama alanı türleri ve yeteneklerini özetler:

| Kullanım durumu | Birim eklentisi | Okuma/yazma kez | Salt okunur çok | Okuma/yazma birçok | Windows Server kapsayıcısı desteği |
|----------|---------------|-----------------|----------------|-----------------|--------------------|
| Paylaşılan yapılandırma       | Azure Dosyaları   | Yes | Yes | Yes | Yes |
| Yapılandırılmış uygulama verileri        | Azure Diskleri   | Yes | Hayır  | Hayır  | Yes |
| Yapılandırılmamış veriler, dosya sistemi işlemleri | [Blobsigortası (Önizleme)][blobfuse] | Yes | Yes | Yes | Hayır |

İki birincil tür aks'deki birimler için sağlanan depolama, Azure diskleri veya Azure dosyaları tarafından desteklenir. Her iki tür depolama güvenliğini artırmak için bekleyen verileri şifreler. varsayılan olarak Azure depolama hizmeti şifrelemesi (SSE) kullanın. Diskler, şu anda Azure Disk şifrelemesi kullanılarak AKS düğümü düzeyinde şifrelenemez.

Azure dosyaları şu anda performans standart katmanda kullanılabilir. Azure diskleri, standart ve Premium performans katmanı mevcuttur:

- *Premium* diskler, yüksek performanslı katı hal diskleri (SSD 'ler) tarafından desteklenir. Premium diskler, tüm üretim iş yükleri için önerilir.
- *Standart* diskler normal dönen diskler (HDD 'ler) tarafından desteklenir ve arşiv veya seyrek erişilen veriler için uygundur.

Uygulama Performans gereksinimlerini anlamak ve erişim desenlerini uygun depolama katmanını seçin. Yönetilen disk boyutları ve performans katmanları hakkında daha fazla bilgi için bkz. [Azure yönetilen disklere genel bakış][managed-disks]

### <a name="create-and-use-storage-classes-to-define-application-needs"></a>Oluşturma ve uygulama gereksinimlerini tanımlamak için depolama sınıfları kullanma

Kullandığınız depolamanın türü, Kubernetes *Depolama sınıfları*kullanılarak tanımlanır. Depolama sınıfı ardından pod veya dağıtım belirtiminde başvuruluyor. Bu tanımları uygun depolama oluşturma ve pod'ların bağlamak için birlikte çalışır. Daha fazla bilgi için bkz. [AKS 'de Depolama sınıfları][aks-concepts-storage-classes].

## <a name="size-the-nodes-for-storage-needs"></a>Depolama gereksinimlerini düğümlerin boyutu

**En iyi Yöntem Kılavuzu** -her düğüm boyutu en fazla disk sayısını destekler. Farklı bir düğüme boyutları, yerel depolama ve ağ bant genişliği miktarı da sağlar. Uygulama ihtiyaçlarınıza uygun boyutta düğümlerinin dağıtmayı planlayın.

AKS düğümlerini Azure Vm'leri olarak çalıştırın. Farklı türleri ve VM boyutları kullanılabilir. Her VM boyutunun CPU ve bellek gibi çekirdek kaynakları farklı bir miktarda sağlar. Bu VM boyutları en fazla eklenebilecek diskler var. Depolama performansı, ayrıca VM boyutları için en fazla yerel ve ekli disk IOPS (saniyede giriş/çıkış işlemi) arasında değişir.

Uygulamalarınızı Azure disk depolama çözümü olarak gerektiriyorsa, planlama ve uygun bir düğüm VM boyutu seçin. VM boyutu seçme, CPU ve bellek miktarı yalnızca Faktör değildir. Depolama kapasitesini de önemlidir. Örneğin, hem *Standard_B2ms* hem de *Standard_DS2_v2* VM BOYUTLARı, benzer bir CPU ve bellek kaynakları içerir. Aşağıdaki tabloda gösterildiği gibi olası depolama performanslarını farklıdır:

| Düğüm türü ve boyutu | Sanal işlemci | Bellek (GiB) | Maksimum veri diskleri | Maksimum önbelleğe alınmamış disk IOPS | Maksimum önbelleğe alınmamış aktarım hızı (MB/sn) |
|--------------------|------|--------------|----------------|------------------------|--------------------------------|
| Standard_B2ms      | 2    | 8            | 4              | 1,920                  | 22.5                           |
| Standard_DS2_v2    | 2    | 7            | 8              | 6,400                  | 96                             |

*Standard_DS2_v2* , eklenen disklerin sayısının iki katına izin verır ve IOPS ve disk aktarım hızı için üç-dört kat sağlar. Yalnızca temel işlem kaynaklarına bakıyordu ve maliyetleri karşılaştırdıysanız, *Standard_B2ms* VM boyutunu seçebilir ve yetersiz depolama performansına ve sınırlamalara sahip olabilirsiniz. Depolama kapasite ve performans gereksinimlerini anlamak için uygulama geliştirme ekibinizle birlikte çalışın. AKS düğümlerinin karşıladığında veya performans gereksinimlerine uygun VM boyutunu seçin. VM boyutu gerektiği şekilde ayarlamak için düzenli olarak temel uygulamaları.

Kullanılabilir VM boyutları hakkında daha fazla bilgi için bkz. [Azure 'Da Linux sanal makineleri Için boyutlar][vm-sizes].

## <a name="dynamically-provision-volumes"></a>Dinamik olarak sağlama birimleri

**En iyi Yöntem Kılavuzu** -yönetim yükünü azaltmak ve ölçeklemenize olanak sağlamak için kalıcı birimleri statik olarak oluşturma ve atama. Dinamik sağlama kullanın. Depolama sınıflarınızdaki pod'ların silindikten sonra gereksiz depolama maliyetlerini en aza indirmek için geri kazanmaya uygun ilkeyi tanımlayın.

Pod'ları için depolama ekleme gerektiğinde, kalıcı birimler kullanın. Bu kalıcı birimler, el ile veya dinamik olarak oluşturulabilir. Kalıcı birimlerin el ile oluşturulması, yönetim yükünü ekler ve ölçeklendirme yeteneğinizi kısıtlar. Depolama yönetimini basitleştirmelerine ve uygulamalarınızı büyütün ve gerektiğinde ölçeği izin vermek için sağlama dinamik kalıcı hacim kullanın.

![Azure Kubernetes Hizmetleri (AKS) kümesini Taleplerde kalıcı hacim](media/concepts-storage/persistent-volume-claims.png)

Kalıcı hacim talep (PVC), dinamik olarak gerektiğinde depolama oluşturmanızı sağlar. Pod'ların istekleri gibi temel Azure diskleri oluşturulur. Pod tanımında oluşturulması ve tasarlanmış bağlama yoluna bağlı bir ses isteği

Birimlerin dinamik olarak oluşturulması ve kullanılması hakkındaki kavramlar için bkz. [kalıcı birimler talepleri][aks-concepts-storage-pvcs].

Bu birimleri eylemde görmek için bkz. [Azure diskleri][dynamic-disks] veya [Azure dosyaları][dynamic-files]ile kalıcı bir birimi dinamik olarak oluşturma ve kullanma.

Depolama sınıfı tanımlarınızın bir parçası olarak, uygun *reclaimPolicy*ayarlayın. Bu reclaimPolicy pod silinir ve kalıcı hacim artık gerekli olmayabilir, temel alınan Azure depolama kaynağı davranışını denetler. Temel alınan depolama kaynağı silinmiş veya gelecekteki bir pod ile kullanılmak üzere saklanır. ReclaimPolicy, *sakla* veya *Sil*olarak ayarlanabilir. Uygulama ihtiyaçlarınızı anlayabilmemiz ve kullanılan faturalandırılır ve beklemediğiniz kullanılan depolama miktarını en aza indirmek için korunan depolama için normal denetimlerini uygular.

Depolama sınıfı seçenekleri hakkında daha fazla bilgi için bkz. [depolama geri kazanma ilkeleri][reclaim-policy].

## <a name="secure-and-back-up-your-data"></a>Güvenli ve verilerinizi yedekleme

**En iyi Yöntem Kılavuzu** -depolama türü için Velero veya Azure Site Recovery gibi uygun bir aracı kullanarak verilerinizi yedekleyin. Bütünlük ve güvenliğini, bu yedekleri doğrulayın.

Uygulamalarınızı depolamak ve kullanmak veri diskleri veya dosyaları kalıcı, düzenli yedeklemeler veya verilerin anlık görüntüsünü almak gerekir. Azure diskleri yerleşik anlık görüntü teknolojileri kullanabilirsiniz. Anlık görüntü işlemini gerçekleştirmeden önce, uygulamanızın diske yazma işlemlerini boşaltmaya yönelik arama yapmanız gerekebilir. [Velero][velero] , ek küme kaynakları ve yapılandırmalarının yanı sıra kalıcı birimleri yedekleyebilir. [Uygulamalarınızdan durum kaldıramazsa][remove-state], verileri kalıcı birimlerden yedekleyin ve veri bütünlüğünü ve gerekli işlemleri doğrulamak için geri yükleme işlemlerini düzenli olarak test edin.

Veri yedekleri ve anlık görüntü önce verilerinizi Sessiz mod için gerekiyorsa farklı yaklaşımların kısıtlamaları anlayın. Veri yedekleri, uygulama ortamınız Küme dağıtımı geri mutlaka izin vermeyin. Bu senaryolar hakkında daha fazla bilgi için bkz. [AKS 'de iş sürekliliği ve olağanüstü durum kurtarma Için en iyi uygulamalar][best-practices-multi-region].

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede aks'deki en iyi depolamaya odaklı. Kubernetes 'te depolama temelleri hakkında daha fazla bilgi için bkz. [AKS 'teki uygulamalar Için depolama kavramları][aks-concepts-storage].

<!-- LINKS - External -->
[velero]: https://github.com/heptio/velero
[blobfuse]: https://github.com/Azure/azure-storage-fuse

<!-- LINKS - Internal -->
[aks-concepts-storage]: concepts-storage.md
[vm-sizes]: ../virtual-machines/linux/sizes.md
[dynamic-disks]: azure-disks-dynamic-pv.md
[dynamic-files]: azure-files-dynamic-pv.md
[reclaim-policy]: concepts-storage.md#storage-classes
[aks-concepts-storage-pvcs]: concepts-storage.md#persistent-volume-claims
[aks-concepts-storage-classes]: concepts-storage.md#storage-classes
[managed-disks]: ../virtual-machines/linux/managed-disks-overview.md
[best-practices-multi-region]: operator-best-practices-multi-region.md
[remove-state]: operator-best-practices-multi-region.md#remove-service-state-from-inside-containers
