---
title: Azure dosyaları ölçeklenebilirlik ve performans hedefleri
description: Kapasite, istek hızı ve gelen ve giden bant genişliği sınırlarını dahil olmak üzere Azure dosyaları için ölçeklenebilirlik ve performans hedefleri hakkında öğrenin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 1a74ec3610367193b5eee53ea0e0818901433e96
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79255138"
---
# <a name="azure-files-scalability-and-performance-targets"></a>Azure dosyaları ölçeklenebilirlik ve performans hedefleri

[Azure dosyaları](storage-files-introduction.md) , bulutta ENDÜSTRI standardı SMB protokolü aracılığıyla erişilebilen tam olarak yönetilen dosya paylaşımları sunar. Bu makalede, Azure dosyaları ve Azure dosya eşitleme için ölçeklenebilirlik ve performans hedefleri ele alır.

Burada listelenen ölçeklenebilirlik ve performans hedefleri olan çeşitli yüksek teknolojiye sahip hedefler, ancak diğer değişkenleri, dağıtımınızdaki etkilenebilir. Örneğin, bir dosya aktarım hızı da, kullanılabilir ağ bant genişliği ile yalnızca Azure dosyaları hizmeti barındıran sunucular sınırlı olabilir. Azure dosyaları performansını ve ölçeklenebilirliğini gereksinimlerinizi karşılayıp karşılamadığını belirlemek için kullanım şekillerini sınama önerilir. Biz de bu sınırların zaman içinde artmasına uygulanır. Lütfen aşağıdaki açıklamalarda veya [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files)'ta, ne kadar yükseğine bakmak istediğinizi öğrenmek istediğiniz limitleri bize geri bildirimde bulunun.

## <a name="azure-storage-account-scale-targets"></a>Azure depolama hesabı ölçek hedefleri

Azure depolama hesabınız bir Azure dosya paylaşımı için üst kaynaktır. Bir depolama hesabı, Azure dosyaları dahil olmak üzere birden çok depolama hizmeti tarafından verileri depolamak için kullanılan Azure depolama havuzunu temsil eder. Veri depolama hesaplarında depolayabilir diğer hizmetleri Azure Blob Depolama, Azure kuyruk depolama ve Azure tablo depolama tabidir. Tüm depolama hizmetlerine verileri bir depolama hesabında depolama aşağıdaki hedeflere Uygula:

[!INCLUDE [azure-storage-account-limits-standard](../../../includes/azure-storage-account-limits-standard.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Diğer depolama hizmetlerinden genel amaçlı depolama hesabı kullanımı, Depolama hesabınızdaki Azure dosya paylaşımlarınızı etkiler. Azure Blob Depolama ile en fazla depolama hesabı kapasitesi ulaşırsa, en fazla paylaşım boyutunun altında Azure dosya paylaşımınızı olsa bile, örneğin, yeni dosyaları Azure dosya paylaşımınızı oluşturmak mümkün olmayacaktır.

## <a name="azure-files-scale-targets"></a>Azure dosyaları ölçek hedefleri

Azure dosyaları için göz önünde bulundurmanız gereken üç sınırlama kategorisi vardır: depolama hesapları, paylaşımlar ve dosyalar.

Örneğin: Premium dosya paylaşımlarında, tek bir paylaşım 100.000 ıOPS elde edebilir ve tek bir dosya 5.000 ıOPS 'ye kadar ölçeklendirebilir. Bu nedenle, bir paylaşımda üç dosya varsa, bu paylaşımdan alabileceğiniz maksimum ıOPS 15.000 ' dir.

### <a name="standard-storage-account-limits"></a>Standart depolama hesabı limitleri

Bu sınırlar için [Azure depolama hesabı ölçek hedefleri](#azure-storage-account-scale-targets) bölümüne bakın.

### <a name="premium-filestorage-account-limits"></a>Premium FileStorage hesap limitleri

[!INCLUDE [azure-storage-limits-filestorage](../../../includes/azure-storage-limits-filestorage.md)]

> [!IMPORTANT]
> Depolama hesabı sınırları tüm paylaşımlar için geçerlidir. FileStorage hesaplarının en büyük değerine kadar ölçeklendirilmesi yalnızca dosya depolama hesabı başına yalnızca bir paylaşımın olması durumunda ulaşılabilir.

### <a name="file-share-and-file-scale-targets"></a>Dosya paylaşma ve dosya ölçeği hedefleri

> [!NOTE]
> 5 TiB 'den büyük standart dosya paylaşımlarında bazı sınırlamalar ve bölgesel kısıtlamalar vardır.
> Sınırlamalar, bölgesel bilgiler ve bu büyük dosya paylaşımı boyutlarını etkinleştirmeye yönelik yönergelerin listesi için, planlama kılavuzunun [daha büyük dosya paylaşımlarına](storage-files-planning.md#enable-standard-file-shares-to-span-up-to-100-tib) ekleme bölümüne bakın.

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

[!INCLUDE [storage-files-premium-scale-targets](../../../includes/storage-files-premium-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Azure dosya eşitleme ölçek hedefleri

Azure Dosya Eşitleme sınırsız kullanım hedefi ile tasarlanmıştır, ancak sınırsız kullanım her zaman mümkün değildir. Aşağıdaki tabloda Microsoft 'un test olan sınırları ve hangi hedeflerin sabit sınırları olduğunu gösterilmektedir:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

### <a name="azure-file-sync-performance-metrics"></a>Azure dosya eşitleme performans ölçümleri

Azure dosya eşitleme aracısının Azure dosya paylaşımları için bağlanan bir Windows Server makinesi üzerinde çalıştığından, etkili eşitleme performansı altyapınızdaki bir dizi etkene bağlıdır: Windows Server ve temel alınan disk yapılandırması, ağ bant genişliği Sunucu ve Azure depolama, dosya boyutu, toplam veri kümesi boyutu ve bir veri kümesini etkinlik arasında. Azure dosya eşitleme dosya düzeyinde çalışır olduğundan, Azure dosya eşitleme tabanlı bir çözüm performans özelliklerini daha iyi ölçülür saniyede işlenen nesne (dosyalar ve dizinler) sayısı.

Azure dosya eşitleme için performans iki aşamada önemlidir:

1. **İlk bir kerelik sağlama**: ilk sağlama performansını iyileştirmek için en iyi dağıtım ayrıntıları için [Azure dosya eşitleme ile ekleme](storage-sync-files-deployment-guide.md#onboarding-with-azure-file-sync) bölümüne bakın.
2. **Devam eden eşitleme**: veriler başlangıçta Azure dosya paylaşımlarında oluşturulduktan sonra Azure dosya eşitleme birden çok uç noktayı eşitlenmiş halde tutar.

Her aşamalar için dağıtım planlamanıza yardımcı olması için aşağıdaki sonuçları iç bir sistemde bir yapılandırma ile test sırasında gözetilir

| Sistem yapılandırması |  |
|-|-|
| CPU | 64 MiB L3 önbellek ile 64 sanal çekirdekler |
| Bellek | 128 GiB |
| Disk | Önbellek pil RAID 10 ile SAS diskleri desteklenir |
| Ağ | 1 GB/sn ağ |
| İş yükü | Genel amaçlı dosya sunucusu|

| İlk tek seferlik sağlama  |  |
|-|-|
| Nesne sayısı | 25.000.000 nesneleri |
| Veri kümesi boyutu| ~ 4,7 TiB |
| Ortalama dosya boyutu | ~ 200 KiB (en büyük dosya: 100 GiB) |
| Aktarım hızı karşıya yükleme | Her eşitleme grubu için saniyede 20 nesne |
| Namespace indirme aktarım hızı * | saniyede 400 nesne |

\* Yeni bir sunucu uç noktası oluşturulduğunda, Azure dosya eşitleme aracısının dosya içeriği indirmez. İlk tam ad alanı eşitler ve ardından Tetikleyiciler ya da tamamen dosyaları indirmek için geri çağırma arka plan veya Bulut katmanlaması sunucu uç noktasında ayarlayın bulut katmanlama ilkesi etkinleştirilir.

| Devam eden eşitleme  |   |
|-|--|
| Eşitlenen nesne sayısı| 125,000 nesneleri (yaklaşık %1 değişim sıklığı) |
| Veri kümesi boyutu| 50 giB |
| Ortalama dosya boyutu | Yaklaşık 500 KiB |
| Aktarım hızı karşıya yükleme | Her eşitleme grubu için saniyede 20 nesne |
| Tam yükleme verimi * | saniyede 60 nesne |

\* İse bulut katmanlama etkin olduğunda, muhtemelen yalnızca bazı veri karşıdan dosya olarak daha iyi performans gözlemleyin. Bunlar herhangi bir uç nokta değiştirildiğinde azure dosya eşitleme yalnızca verilerin önbelleğe alınmış dosyaları indirir. Aracı, tüm katmanlı veya yeni oluşturulan dosyalar için dosya verilerini indirmez ve bunun yerine, tüm sunucu uç noktaları için ad alanı yalnızca eşitleyebilir. Kullanıcı tarafından erişilen gibi aracı katmanlı dosyaların kısmi yüklemeleri de destekler. 

> [!Note]  
> Yukarıdaki sayılar, yaşar performansı değildir. Gerçek performans, bu bölümün başında belirtildiği gibi pek çok unsura bağlıdır.

Dağıtımınız için genel bir kılavuz olarak, bazı noktalar göz önünde tutmalısınız:

- Nesne aktarım hızı derlemekten sunucuda eşitleme grubu sayısı yaklaşık olarak ölçeklendirir. Bir sunucuda birden çok eşitleme gruplarına veri bölme, ağ ve sunucu tarafından da sınırlı daha iyi verim verir.
- Nesne işleme başına ikinci aktarım hızını MIB inversely orantılıdır. Daha küçük dosyalar için ikinci, ancak daha düşük MiB ikinci işleme başına başına işlenen nesnelerin sayısı bakımından daha yüksek iş hacmi karşılaşırsınız. Buna karşılık, daha büyük dosyalar için daha az nesne ikinci, ancak daha yüksek MiB ikinci işleme başına başına işlenen alırsınız. Azure dosyaları ölçeklendirme hedeflerini ikinci işleme başına MiB sınırlıdır.

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Dosyaları dağıtımını planlama](storage-files-planning.md)
- [Azure Dosya Eşitleme dağıtımı planlama](storage-sync-files-planning.md)
