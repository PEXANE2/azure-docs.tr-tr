---
title: Depolama ve yedekleme için en iyi uygulamalar
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Hizmetinde (AKS) depolama, veri şifreleme ve yedekleme için küme operatörünün en iyi uygulamaları öğrenin
services: container-service
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 90abf2e36fd46c707904d87f00362091fe931743
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668108"
---
# <a name="best-practices-for-storage-and-backups-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) depolama ve yedekleme ler için en iyi uygulamalar

Azure Kubernetes Hizmeti'nde (AKS) kümeler oluşturur ve yönetirken, uygulamalarınızın genellikle depolama alanına ihtiyacı vardır. Uygulamalara uygun depolama alanını sağlayabilmeniz için bölmelerin performans gereksinimlerini ve erişim yöntemlerini anlamak önemlidir. AKS düğümü boyutu bu depolama seçeneklerini etkileyebilir. Ayrıca, eklenmiş depolama için geri yükleme işlemini yedekleme ve test etmenin yollarını da planlamanız gerekir.

Bu en iyi uygulamalar makalesi küme operatörleri için depolama hususları üzerinde duruluyor. Bu makalede, öğrenmek:

> [!div class="checklist"]
> * Ne tür depolama alanı mevcuttur
> * Depolama performansı için AKS düğümlerini doğru boyutlandırma
> * Birimlerin dinamik ve statik sağlanması arasındaki farklar
> * Veri birimlerinizi yedeklemenin ve korumanın yolları

## <a name="choose-the-appropriate-storage-type"></a>Uygun depolama türünü seçin

**En iyi uygulama kılavuzu** - Doğru depolama alanını seçmek için uygulamanızın gereksinimlerini anlayın. Üretim iş yükleri için yüksek performanslı, SSD destekli depolama yı kullanın. Birden çok eşzamanlı bağlantı ya da ağ tabanlı depolama alanı için planlayın.

Uygulamalar genellikle farklı depolama türleri ve hızları gerektirir. Uygulamalarınızda tek tek bölmelere bağlanan veya birden çok bölmede paylaşılan depolama alanına mı ihtiyacınız var? Depolama, verilere salt okunur erişim veya büyük miktarda yapılandırılmış veri yazmak için mi? Bu depolama gereksinimleri kullanılacak en uygun depolama türünü belirler.

Aşağıdaki tablo, kullanılabilir depolama türlerini ve bunların yeteneklerini özetleyerek:

| Kullanım örneği | Ses eklentisi | Bir kez okuma/yazma | Yalnızca okunan çok | Çok okuma/yazma | Windows Server kapsayıcı desteği |
|----------|---------------|-----------------|----------------|-----------------|--------------------|
| Paylaşılan yapılandırma       | Azure Dosyaları   | Evet | Evet | Evet | Evet |
| Yapılandırılmış uygulama verileri        | Azure Diskleri   | Evet | Hayır  | Hayır  | Evet |
| Yapılandırılmamış veriler, dosya sistemi işlemleri | [BlobFuse][blobfuse] | Evet | Evet | Evet | Hayır |

AKS'deki birimler için sağlanan iki birincil depolama alanı türü Azure Diskleri veya Azure Dosyaları tarafından desteklenir. Güvenliği artırmak için, her iki depolama alanı türü de varsayılan olarak verileri istirahatte şifreleyen Azure Depolama Hizmeti Şifrelemesini (SSE) kullanır. Diskler şu anda AKS düğümü düzeyinde Azure Disk Şifrelemesi kullanılarak şifrelenemez.

Azure Dosyaları şu anda Standart performans katmanında kullanılabilir. Azure Diskleri Standart ve Premium performans katmanlarında kullanılabilir:

- *Premium* diskler yüksek performanslı katı hal diskleri (SSD'ler) tarafından desteklenir. Tüm üretim iş yükleri için premium diskler önerilir.
- *Standart* diskler normal dönen diskler (HDD'ler) tarafından desteklenir ve arşivleme veya seyrek erişilen veriler için iyidir.

Uygun depolama katmanını seçmek için uygulama performans gereksinimlerini ve erişim desenlerini anlayın. Yönetilen Diskler boyutları ve performans katmanları hakkında daha fazla bilgi için [Azure Yönetilen Diskler'e genel bakış][managed-disks]

### <a name="create-and-use-storage-classes-to-define-application-needs"></a>Uygulama gereksinimlerini tanımlamak için depolama sınıfları oluşturma ve kullanma

Kullandığınız depolama türü Kubernetes depolama *sınıfları*kullanılarak tanımlanır. Depolama sınıfı daha sonra bölme veya dağıtım belirtiminde başvurur. Bu tanımlar, uygun depolama alanı oluşturmak ve bölmelere bağlamak için birlikte çalışır. Daha fazla bilgi için [AKS'deki Depolama sınıflarına][aks-concepts-storage-classes]bakın.

## <a name="size-the-nodes-for-storage-needs"></a>Depolama ihtiyaçları için düğümleri boyutlandırma

**En iyi uygulama kılavuzu** - Her düğüm boyutu en fazla sayıda diski destekler. Farklı düğüm boyutları da yerel depolama ve ağ bant genişliği farklı miktarda sağlar. Düğümlerin uygun boyutunu dağıtmak için uygulama taleplerinizi planlayın.

AKS düğümleri Azure VM'ler olarak çalışır. Farklı VM türleri ve boyutları mevcuttur. Her VM boyutu, CPU ve bellek gibi farklı miktarda temel kaynak sağlar. Bu VM boyutları, eklenebilir disklerin maksimum sayıda vardır. Depolama performansı, maksimum yerel ve bağlı disk IOPS (saniyede giriş/çıkış işlemleri) için VM boyutları arasında da değişir.

Uygulamalarınız depolama çözümü olarak Azure Diskleri gerektiriyorsa, uygun bir düğüm VM boyutu planlayın ve seçin. Bir VM boyutu seçtiğinizde CPU ve bellek miktarı tek faktör değildir. Depolama yetenekleri de önemlidir. Örneğin, hem *Standard_B2ms* hem de *Standard_DS2_v2* VM boyutları benzer miktarda CPU ve bellek kaynakları içerir. Aşağıdaki tabloda gösterildiği gibi olası depolama performansları farklıdır:

| Düğüm türü ve boyutu | Sanal işlemci | Bellek (GİB) | Maksimum veri diskleri | Maksimum önsüz disk IOPS | Maksimum uncached iş girişi (MBps) |
|--------------------|------|--------------|----------------|------------------------|--------------------------------|
| Standard_B2ms      | 2    | 8            | 4              | 1,920                  | 22.5                           |
| Standard_DS2_v2    | 2    | 7            | 8              | 6,400                  | 96                             |

Burada, *Standard_DS2_v2* bağlı disk sayısının iki katına izin verir ve IOPS ve disk iştibunu üç ila dört kat daha fazla sağlar. Yalnızca temel işlem kaynaklarına ve karşılaştırıldığında maliyetlere baktıysanız, *Standard_B2ms* VM boyutunu seçebilir ve depolama performansı ve sınırlamaları düşüktür. Depolama kapasitelerini ve performans gereksinimlerini anlamak için uygulama geliştirme ekibinizle birlikte çalışın. AKS düğümlerinin performans gereksinimlerini karşılaması veya aşması için uygun VM boyutunu seçin. VM boyutunu gerektiği gibi ayarlamak için düzenli olarak temel uygulamalar.

Kullanılabilir VM boyutları hakkında daha fazla bilgi için [Azure'daki Linux sanal makineleri için Boyutlar'a][vm-sizes]bakın.

## <a name="dynamically-provision-volumes"></a>Dinamik olarak sağlama hacimleri

**En iyi uygulama kılavuzu** - Yönetim yükü azaltmak ve ölçeklendirmenize izin vermek için, kalıcı birimler oluşturmayın ve atayın. Dinamik sağlama kullanın. Depolama sınıflarınızda, bölmeler silindikten sonra gereksiz depolama maliyetlerini en aza indirmek için uygun geri alma ilkesini tanımlayın.

Bölmelere depolama alanı eklemeniz gerektiğinde kalıcı birimler kullanırsınız. Bu kalıcı birimler el ile veya dinamik olarak oluşturulabilir. Kalıcı birimlerin el ile oluşturulması, yönetim ek yükü ekler ve ölçeklendirme yeteneğinizi sınırlar. Depolama yönetimini basitleştirmek ve uygulamalarınızın gerektiğinde büyümesine ve ölçeklemesine izin vermek için dinamik kalıcı birim sağlama yı kullanın.

![Azure Kubernetes Hizmetleri (AKS) kümesinde kalıcı birim talepleri](media/concepts-storage/persistent-volume-claims.png)

Kalıcı bir hacim talebi (PVC), gerektiğinde dinamik olarak depolama oluşturmanıza olanak tanır. Temel Azure diskleri, bölmelerin isteği üzerine oluşturulur. Pod tanımında, oluşturulacak ve belirlenmiş bir montaj yoluna eklenecek bir birim talep esiniz.

Birim oluşturma ve kullanma ile ilgili kavramlar [için, Kalıcı Birim Talepleri'ne][aks-concepts-storage-pvcs]bakın.

Bu birimleri iş başında görmek için, [Azure Diskleri][dynamic-disks] veya [Azure Dosyaları][dynamic-files]ile kalıcı bir birimin dinamik olarak nasıl oluşturulup kullanılacağını görün.

Depolama sınıfı tanımlarınızın bir parçası olarak, uygun *geri alma Politikası'nı*ayarlayın. Bu geri alma Politikası, bölme silindiğinde ve kalıcı birim artık gerekmediğinde, temel Azure depolama kaynağının davranışını denetler. Temel depolama kaynağı silinebilir veya gelecekteki bir bölmeyle kullanılmak üzere saklanabilir. ReclaimPolicy *korumak* veya *silmek*için ayarlayabilirsiniz. Uygulama gereksinimlerinizi anlayın ve kullanılan ve faturalanan kullanılmayan depolama miktarını en aza indirmek için tutulan depolama için düzenli denetimler uygulayın.

Depolama sınıfı seçenekleri hakkında daha fazla bilgi için [depolama geri alma ilkelerine][reclaim-policy]bakın.

## <a name="secure-and-back-up-your-data"></a>Verilerinizi güvenli hale getirin ve yedekle

**En iyi uygulama kılavuzu** - Velero veya Azure Site Kurtarma gibi depolama türünüz için uygun bir araç kullanarak verilerinizi yedekleyin. Bu yedeklemelerin bütünlüğünü ve güvenliğini doğrulayın.

Uygulamalarınız disklerde veya dosyalarda kalıcı verileri depolayıp tükettiği zaman, bu verilerin düzenli yedeklemelerini veya anlık görüntülerini almanız gerekir. Azure Diskleri yerleşik anlık görüntü teknolojilerini kullanabilir. Anlık görüntü işlemini gerçekleştirmeden önce yazmaları diske yıkamak için uygulamalarınızı aramanız gerekebilir. [Velero,][velero] kalıcı birimleri ek küme kaynakları ve yapılandırmalarla birlikte yedekleyebilir. [Durumları uygulamalarınızdan][remove-state]kaldıramıyorsanız, verileri kalıcı birimlerden yedekleyebilir ve veri bütünlüğünü ve gerekli işlemleri doğrulamak için geri yükleme işlemlerini düzenli olarak sınama.

Veri yedeklemelerine yönelik farklı yaklaşımların sınırlamalarını ve anlık görüntüden önce verilerinizi sorgulamanız gerekiyorsa, sınırlamaları anlayın. Veri yedeklemeleri, uygulama ortamınızı küme dağıtımıyla geri yüklemenize izin vermez. Bu senaryolar hakkında daha fazla bilgi için, [AKS'de iş sürekliliği ve olağanüstü durum kurtarma için en iyi uygulamalara][best-practices-multi-region]bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, AKS depolama en iyi uygulamaları üzerinde duruldu. Kubernetes depolama temelleri hakkında daha fazla bilgi [için, AKS uygulamaları için Depolama kavramları][aks-concepts-storage]bakın.

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
