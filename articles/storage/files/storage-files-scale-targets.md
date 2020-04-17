---
title: Azure Dosyaları ölçeklenebilirlik ve performans hedefleri
description: Azure Dosyaları'nın kapasite, istek oranı ve gelen ve giden bant genişliği sınırları da dahil olmak üzere ölçeklenebilirlik ve performans hedefleri hakkında bilgi edinin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 46c46faf8f7ee52978ae5542ab7ebd72a41b8357
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536463"
---
# <a name="azure-files-scalability-and-performance-targets"></a>Azure Dosyaları ölçeklenebilirlik ve performans hedefleri

[Azure Files,](storage-files-introduction.md) endüstri standardı SMB protokolü aracılığıyla erişilebilen bulutta tam olarak yönetilen dosya paylaşımları sunar. Bu makalede, Azure Dosyaları ve Azure Dosya Eşitlemi için ölçeklenebilirlik ve performans hedefleri açıklanmaktadır.

Burada listelenen ölçeklenebilirlik ve performans hedefleri üst düzey hedeflerdir, ancak dağıtımınızdaki diğer değişkenlerden etkilenebilir. Örneğin, bir dosyanın iş bilgililiği yalnızca Azure Dosyaları hizmetini barındıran sunucularla değil, kullanılabilir ağ bant genişliğinizle de sınırlı olabilir. Azure Dosyalarının ölçeklenebilirliğinin ve performansının gereksinimlerinizi karşılayıp karşılamadığını belirlemek için kullanım deseninizi test etmenizi şiddetle öneririz. Biz de zaman içinde bu sınırları artırmak için kararlıyız. Lütfen aşağıdaki yorumlarda veya [Azure Files UserVoice'da](https://feedback.azure.com/forums/217298-storage/category/180670-files)hangi sınırların arttığına dair geri bildirimde bulunan bize geri bildirimde bulunan lar için çekinmeyin.

## <a name="azure-storage-account-scale-targets"></a>Azure depolama hesabı ölçeği hedefleri

Azure dosya paylaşımıiçin ana kaynak bir Azure depolama hesabıdır. Depolama hesabı, Azure'da veri depolamak için Azure Dosyaları da dahil olmak üzere birden çok depolama hizmeti tarafından kullanılabilecek bir depolama havuzutemsil eder. Depolama hesaplarında veri depolayan diğer hizmetler Azure Blob depolama, Azure Sıra depolama ve Azure Tablo depolamadır. Aşağıdaki hedefler, verileri bir depolama hesabında depolayan tüm depolama hizmetlerini uygular:

[!INCLUDE [azure-storage-account-limits-standard](../../../includes/azure-storage-account-limits-standard.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Diğer depolama hizmetlerinden alınan genel amaçlı depolama hesabı kullanımı, depolama hesabınızdaki Azure dosya paylaşımlarınızı etkiler. Örneğin, Azure Blob depolama alanıyla maksimum depolama hesabı kapasitesine ulaşırsanız, Azure dosya paylaşımınız maksimum paylaşım boyutunun altında olsa bile Azure dosya paylaşımınızda yeni dosyalar oluşturamazsınız.

## <a name="azure-files-scale-targets"></a>Azure Dosyaları ölçek hedefleri

Azure Dosyaları için göz önünde bulundurulması gereken üç sınırlama kategorisi vardır: depolama hesapları, paylaşımlar ve dosyalar.

Örneğin: Premium dosya paylaşımları ile tek bir hisse 100.000 IOPS'ye ulaşabilir ve tek bir dosya 5.000 IOPS'ye kadar ölçeklenebilir. Yani, bir hissede üç dosya varsa, bu paylaşımdan alabileceğiniz maksimum IOPS 15.000'dir.

### <a name="standard-storage-account-limits"></a>Standart depolama hesabı limitleri

Bu sınırlar için [Azure depolama hesabı ölçek hedefleri](#azure-storage-account-scale-targets) bölümüne bakın.

### <a name="premium-filestorage-account-limits"></a>Premium FileStorage hesap limitleri

[!INCLUDE [azure-storage-limits-filestorage](../../../includes/azure-storage-limits-filestorage.md)]

> [!IMPORTANT]
> Depolama hesabı sınırları tüm paylaşımlar için geçerlidir. FileStorage hesapları için maksimum ölçekleme yalnızca FileStorage hesabı başına yalnızca bir paylaşım varsa elde edilebilir.

### <a name="file-share-and-file-scale-targets"></a>Dosya paylaşımı ve dosya ölçeği hedefleri

> [!NOTE]
> Standart dosya paylaşımları 5 TiB'den büyük belirli sınırlamalar vardır. Daha büyük dosya paylaşımı boyutlarını etkinleştirmek için sınırlamalar ve yönergeler listesi için, planlama kılavuzunun standart dosya paylaşımları bölümünde [daha büyük dosya paylaşımlarını etkinleştirme](storage-files-planning.md#enable-standard-file-shares-to-span-up-to-100-tib) bölümüne bakın.

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

[!INCLUDE [storage-files-premium-scale-targets](../../../includes/storage-files-premium-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Azure Dosya Eşitleme ölçek hedefleri

Azure Dosya Eşitlemesi sınırsız kullanım amacıyla tasarlanmıştır, ancak sınırsız kullanım her zaman mümkün değildir. Aşağıdaki tablo Microsoft'un test sınırlarını gösterir ve aynı zamanda hangi hedeflerin sabit sınırlar olduğunu gösterir:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

### <a name="azure-file-sync-performance-metrics"></a>Azure Dosya Eşitleme performans ölçümleri

Azure Dosya Eşitleme aracısı Azure dosya paylaşımlarına bağlanan bir Windows Server makinesinde çalıştığından, etkili eşitleme performansı altyapınızdaki bir dizi etkene bağlıdır: Windows Server ve temel disk yapılandırması, sunucu ile Azure depolama arasındaki ağ bant genişliği, dosya boyutu, toplam veri kümesi boyutu ve veri kümesindeki etkinlik. Azure Dosya Eşitlemi dosya düzeyinde çalıştığından, Azure Dosya Eşitleme tabanlı bir çözümün performans özellikleri saniyede işlenen nesne (dosya ve dizinler) sayısıyla daha iyi ölçülür.

Azure Dosya Eşitleme için performans iki aşamada önemlidir:

1. **İlk tek seferlik sağlama**: İlk sağlamada performansı optimize etmek için, en iyi dağıtım ayrıntıları için [Azure Dosya Eşitlemi ile Onboarding'e](storage-sync-files-deployment-guide.md#onboarding-with-azure-file-sync) bakın.
2. **Devam eden eşitleme**: Veriler başlangıçta Azure dosya paylaşımlarında seriye alındıktan sonra, Azure Dosya Eşitlemi birden çok uç noktayı eşit tutar.

Her aşama için dağıtımınızı planlamanıza yardımcı olmak için, bir config ile bir sistem üzerinde dahili sınama sırasında gözlenen sonuçlar aşağıda verilmiştir

| Sistem yapılandırması |  |
|-|-|
| CPU | 64 MiB L3 önbelleği ile 64 Sanal Çekirdek |
| Bellek | 128 GiB |
| Disk | Raid 10 pil destekli önbelleğe sahip SAS diskleri |
| Ağ | 1 Gbps Ağ |
| İş yükü | Genel Amaçlı Dosya Sunucusu|

| İlk tek seferlik sağlama  |  |
|-|-|
| Nesne sayısı | 25 milyon nesne |
| Dataset Boyutu| ~4.7 TiB |
| Ortalama Dosya Boyutu | ~200 KiB (En Büyük Dosya: 100 GiB) |
| Yükleme Throughput | Eşitleme grubu başına saniyede 20 nesne |
| Namespace İndirme Throughput* | Saniyede 400 nesne |

*Yeni bir sunucu bitiş noktası oluşturulduğunda, Azure Dosya Eşitleme aracısı dosya içeriğinin hiçbirini karşıdan yüklemez. Önce tam ad alanını eşitler ve ardından dosyaları tam olarak veya bulut katmanlama etkinse sunucu bitiş noktasında ayarlanan bulut katmanlama ilkesine indirmek için arka plan hatırlamayı tetikler.

| Devam eden eşitleme  |   |
|-|--|
| Eşitlenen nesne sayısı| 125.000 nesne (~%1 çalkalama) |
| Dataset Boyutu| 50 GİB |
| Ortalama Dosya Boyutu | ~500 KiB |
| Yükleme Throughput | Eşitleme grubu başına saniyede 20 nesne |
| Tam İndir Throughput* | Saniyede 60 nesne |

*Bulut katmanlama etkinse, dosya verilerinin yalnızca bir kısmı karşıdan yüklenirken daha iyi performans gözlemleme olasılığınız yüksektir. Azure Dosya Eşitleme, önbelleğe alınan dosyaların verilerini yalnızca uç noktalardan herhangi birinde değiştirildiğinde indirir. Katmanlı veya yeni oluşturulan dosyalar için aracı dosya verilerini karşıdan yüklemez ve bunun yerine ad alanını yalnızca tüm sunucu bitiş noktalarıyla eşitler. Aracı, kullanıcı tarafından erişilen katmanlı dosyaların kısmi indirilmelerini de destekler. 

> [!Note]  
> Yukarıdaki sayılar, karşılaşacağınız performansın bir göstergesi değildir. Gerçek performans, bu bölümün başında belirtildiği gibi birden çok etkene bağlıdır.

Dağıtımınız için genel bir kılavuz olarak, birkaç şeyi aklınızda bulundurmalısınız:

- Nesne iş çıktısı, sunucudaki eşitleme gruplarının sayısıyla orantılı olarak yaklaşık olarak ölçeklendirilir. Verileri bir sunucuda birden çok eşitleme grubuna bölmek, sunucu ve ağ la da sınırlı olan daha iyi iş verimi sağlar.
- Nesne iş kısmı, saniye başına MiB ile ters orantılıdır. Daha küçük dosyalar için, saniyede işlenen nesne sayısı açısından daha yüksek iş elde işlemi, ancak saniye başına daha düşük MiB deneyimi yaşarsınız. Tersine, daha büyük dosyalar için saniyede daha az nesne işlenir, ancak saniyede daha yüksek MiB elde erecektir. Saniyebaşına MIB iş ortası, Azure Dosyaları ölçeği hedefleri ile sınırlıdır.

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Dosyaları dağıtımı planlama](storage-files-planning.md)
- [Azure Dosya Eşitleme dağıtımı planlama](storage-sync-files-planning.md)
