---
title: Azure Eşli Bölgeleri kullanarak iş sürekliliğinin & olağanüstü durum kurtarmayı sağlama
description: Azure bölgesel eşleştirmesini kullanarak uygulama esnekliğini sağlayın
author: jpconnock
manager: angrobe
ms.service: multiple
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: jeconnoc
ms.openlocfilehash: 778943dad9a04632797d5d9165b6f1f9a3eb9850
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248261"
---
# <a name="business-continuity-and-disaster-recovery-bcdr-azure-paired-regions"></a>İş sürekliliği ve olağanüstü durum kurtarma (BCDR): Eşleştirilmiş Azure Bölgeleri

## <a name="what-are-paired-regions"></a>Eşleştirilmiş bölgeler nelerdir?

Azure bölgesi, gecikme tanımlı bir çevre içinde dağıtılan ve özel bir düşük gecikme ağı üzerinden bağlanan bir veri merkezi kümesinden oluşur.  Bu, bir Azure bölgesindeki Azure hizmetlerinin mümkün olan en iyi performansı ve güvenliği sunmasını sağlar.  

Azure coğrafyası, dünyanın en az bir Azure bölgesi içeren bir alanını tanımlar. Coğrafyalar, genellikle veri ikametve uyumluluk sınırlarını koruyan iki veya daha fazla bölge içeren ayrı bir pazar tanımlar.  Azure'un küresel altyapısı hakkında daha fazla bilgiyi [burada](https://azure.microsoft.com/global-infrastructure/regions/) bulabilirsiniz

Bölgesel bir çift aynı coğrafyaiçinde iki bölgeden oluşur. Azure, platform güncelleştirmelerini (planlı bakım) bölgesel çiftler arasında seri hale sağlayarak her çiftte aynı anda yalnızca bir bölgenin güncelleştirilmesini sağlar. Bir kesinti birden çok bölgeyi etkiliyorsa, kurtarma için her çiftten en az bir bölgeye öncelik verilecektir.

![AzureCoğrafya](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

Bazı Azure hizmetleri, iş sürekliliğini sağlamak ve veri kaybına karşı korunmak için eşleştirilmiş bölgelerden daha fazla yararlanır.  Azure, veri kullanılabilirliğini sağlamak için eşleştirilmiş bölgelerden yararlanan çeşitli [depolama çözümleri](./storage/common/storage-redundancy.md#redundancy-in-a-secondary-region) sağlar. Örneğin, [Azure Coğrafi Yedekli Depolama](./storage/common/storage-redundancy.md#geo-redundant-storage) (GRS), birincil bölgenin kurtarılamamış olması durumunda bile verilerin dayanıklı olmasını sağlayarak verileri ikincil bir bölgeye otomatik olarak çoğaltır. 

Tüm Azure hizmetlerinin verileri otomatik olarak çoğaltmadığını veya tüm Azure hizmetlerinin başarısız bir bölgeden çiftine otomatik olarak geri düşmediğini unutmayın.  Bu gibi durumlarda, kurtarma ve çoğaltma müşteri tarafından yapılandırılmalıdır.

## <a name="can-i-select-my-regional-pairs"></a>Bölgesel çiftlerimi seçebilir miyim?

Hayır. Bazı Azure hizmetleri, Azure'un [gereksiz depolama alanı](./storage/common/storage-redundancy.md)gibi bölgesel çiftlere dayanır. Bu hizmetler, yeni bölgesel eşleştirmeler oluşturmanıza izin vermez.  Benzer şekilde, Azure bölgesel çiftler için planlı bakım ve kurtarma önceliklendirmesini denetlediğinden, bu hizmetlerden yararlanmak için kendi bölgesel çiftlerinizi tanımlayamazsınız. Ancak, herhangi bir sayıda bölgede hizmetler oluşturarak ve bunları eşleştirmek için Azure hizmetlerinden yararlanarak kendi olağanüstü durum kurtarma çözümünüzü oluşturabilirsiniz. 

Örneğin, veri yedeklemelerini farklı bir bölgedeki bir Depolama hesabına zamanlamak için [AzCopy](./storage/common/storage-use-azcopy-v10.md) gibi Azure hizmetlerini kullanabilirsiniz.  [Azure DNS ve Azure Trafik Yöneticisi'ni](./networking/disaster-recovery-dns-traffic-manager.md)kullanan müşteriler, uygulamaları için birincil bölgenin kaybından kurtulacak esnek bir mimari tasarlayabilir.

## <a name="am-i-limited-to-using-services-within-my-regional-pairs"></a>Bölgesel çiftlerimdeki hizmetleri kullanmakla sınırlı mıyım?

Hayır. Belirli bir Azure hizmeti bölgesel bir çifte güvenebilir, ancak diğer hizmetlerinizi işletme gereksinimlerinizi karşılayan herhangi bir bölgede barındırabilirsiniz.  Azure GRS depolama çözümü, Doğu ABD'de bulunan Bilgi İşlem kaynaklarını kullanırken Kanada Merkezi'ndeki verileri Kanada Doğu'daki bir eşle eşleyebilir.  

## <a name="must-i-use-azure-regional-pairs"></a>Azure bölgesel çiftleri kullanmalı mıyım?

Hayır. Müşteriler, Azure'un bölgesel çiftlerine güvenmeden esnek bir hizmet oluşturmak için Azure hizmetlerinden yararlanabilir.  Ancak, [izolasyondan](./security/fundamentals/isolation-choices.md) yararlanmak ve [kullanılabilirliği](./availability-zones/az-overview.md)artırmak için iş sürekliliği olağanüstü durum kurtarmayı (BCDR) bölgesel çiftler arasında yapılandırmanızı öneririz. Birden çok etkin bölgeyi destekleyen uygulamalarda mümkün olduğunca bölge çiftindeki her iki bölgenin de kullanılmasını öneririz. Bu, uygulamalar için en iyi kullanılabilirliği ve bir felaket durumunda kurtarma süresini en aza indirir. Mümkün olduğunda, [maksimum esneklik](https://docs.microsoft.com/azure/architecture/framework/resiliency/overview) ve olağanüstü [durum kurtarma](https://docs.microsoft.com/azure/architecture/framework/resiliency/backup-and-recovery)kolaylığı için uygulamanızı tasarlayın.

## <a name="azure-regional-pairs"></a>Azure Bölgesel Çiftler

| Coğrafya | Bölgesel A Çifti | Bölgesel B Çifti  |
|:--- |:--- |:--- |
| Asya Pasifik |Doğu Asya (Hong Kong) | Güneydoğu Asya (Singapur) |
| Avustralya |Doğu Avustralya |Güneydoğu Avustralya |
| Avustralya |Orta Avustralya |Orta Avustralya 2 |
| Brezilya |Güney Brezilya |Orta Güney ABD |
| Kanada |Orta Kanada |Doğu Kanada |
| Çin |Kuzey Çin |Doğu Çin|
| Çin |Çin Kuzey 2 |Çin Doğu 2|
| Avrupa |Kuzey Avrupa (İrlanda) |Batı Avrupa (Hollanda) |
| Fransa |Orta Fransa|Güney Fransa|
| Almanya |Orta Almanya |Kuzeydoğu Almanya |
| Hindistan |Orta Hindistan |Güney Hindistan |
| Hindistan |Batı Hindistan |Güney Hindistan |
| Japonya |Doğu Japonya |Batı Japonya |
| Güney Kore |Güney Kore - Orta |Güney Kore - Güney |
| Kuzey Amerika |Doğu ABD |Batı ABD |
| Kuzey Amerika |Doğu ABD 2 |Orta ABD |
| Kuzey Amerika |Orta Kuzey ABD |Orta Güney ABD |
| Kuzey Amerika |Batı ABD 2 |Orta Batı ABD |
| Norveç | Norveç Doğu | Norveç Batı |
| Güney Afrika | Güney Afrika Kuzey |Güney Afrika Batı |
| İsviçre | İsviçre Kuzey |İsviçre Batı |
| Birleşik Krallık |Batı Birleşik Krallık |Güney Birleşik Krallık |
| Birleşik Arap Emirlikleri | BAE Kuzey | BAE Merkez
| ABD Savunma Bakanlığı |Doğu US DoD |Orta US DoD |
| ABD Kamu |US Gov Arizona |US Gov Texas |
| ABD Kamu |US Gov Iowa |US Gov Virginia |
| ABD Kamu |US Gov Virginia |US Gov Texas |

> [!Important]
> - Batı Hindistan sadece tek yönde eşlenmiştir. Batı Hindistan'ın ikincil bölgesi Güney Hindistan, güney Hindistan'ın ikincil bölgesi orta Hindistan'dır.
> - Brezilya Güney kendi coğrafyası dışında bir bölge ile eşleştirilmiş olduğu için benzersizdir. Brezilya Güney ikincil bölge Güney Orta ABD olduğunu. Güney Orta ABD'nin ikincil bölge Brezilya Güney değildir.


## <a name="an-example-of-paired-regions"></a>Eşleştirilmiş bölge örneği
Aşağıdaki resimde, olağanüstü durum kurtarma için bölgesel çifti kullanan varsayımsal bir uygulama gösteriş. Yeşil sayılar, üç Azure hizmetinin (Azure bilgi işlem, depolama ve veritabanı) bölgeler arası etkinliklerini ve bölgeler arasında çoğaltAcak şekilde nasıl yapılandırıldıklarını vurgular. Eşleştirilmiş bölgeler arasında dağıtımın benzersiz yararları turuncu sayılarla vurgulanır.

![Eşleştirilmiş Bölge Faydalarına Genel Bakış](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

Şekil 2 – Varsayımsal Azure bölgesel çifti

## <a name="cross-region-activities"></a>Bölgeler arası etkinlikler
Şekil 2'de belirtildiği gibi.

1. **Azure İşlem (IaaS)** – Bir felaket sırasında kaynakların başka bir bölgede kullanılabilir olduğundan emin olmak için ek bilgi işlem kaynaklarını önceden sağlamanız gerekir. Daha fazla bilgi için [Azure esnekliği teknik kılavuzuna](https://github.com/uglide/azure-content/blob/master/articles/resiliency/resiliency-technical-guidance.md)bakın. 

2. **Azure Depolama** - Yönetilen diskler kullanıyorsanız, Azure Yedekleme ile [bölgeler arası yedeklemeler](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region#virtual-machines) hakkında bilgi edinin ve Azure Site Kurtarma ile [VM'leri](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication) bir bölgeden diğerine çoğaltın. Depolama hesapları kullanıyorsanız, bir Azure Depolama hesabı oluşturulduğunda coğrafi yedekli depolama (GRS) varsayılan olarak yapılandırılır. GRS ile verileriniz birincil bölge içinde üç kez, eşlenen bölgede üç kez otomatik olarak çoğaltılır. Daha fazla bilgi için [Azure Depolama Artıkları Seçenekleri'ne](storage/common/storage-redundancy.md)bakın.

3. **Azure SQL Veritabanı** – Azure SQL Veritabanı Coğrafi Çoğaltma ile, işlemlerin eşzamanlı çoğaltmasını dünyanın herhangi bir bölgesine yapılandırabilirsiniz; ancak, bu kaynakları çoğu olağanüstü durum kurtarma senaryosu için eşleştirilmiş bir bölgede dağıtmanızı öneririz. Daha fazla bilgi için Azure [SQL Veritabanı'nda Coğrafi Çoğaltma'ya](sql-database/sql-database-geo-replication-overview.md)bakın.

4. **Azure Resource Manager**: Resource Manager doğal olarak bölgeler arasında bileşenlerin mantıksal yalıtımını sağlar. Bu, bir bölgedeki mantıksal hataların diğerini etkileme olasılığının daha düşük olduğu anlamına gelir.

## <a name="benefits-of-paired-regions"></a>Eşleştirilmiş bölgelerin yararları

5. **Fiziksel yalıtım** – Azure, tüm coğrafyalarda pratik veya mümkün olmasa da, mümkün olduğunda bölgesel bir çiftteki veri merkezleri arasında en az 300 mil lik ayrımı tercih eder. Fiziksel veri merkezi ayrımı, doğal afetler, sivil huzursuzluk, elektrik kesintileri veya fiziksel ağ kesintilerinin her iki bölgeyi aynı anda etkileme olasılığını azaltır. İzolasyon, coğrafyadaki kısıtlamalara (coğrafya boyutu, güç/ağ altyapısı kullanılabilirliği, düzenlemeler, vb.) tabidir.  

6. **Platform tarafından sağlanan çoğaltma** - Coğrafi Yedekli Depolama gibi bazı hizmetler eşleştirilmiş bölgeye otomatik çoğaltma sağlar.

7. **Bölge kurtarma düzeni** – Geniş bir kesinti durumunda, her çiftten bir bölgenin kurtarılmasına öncelik verilir. Eşleştirilmiş bölgelere dağıtılan uygulamalarda bölgelerden birinin öncelikli olarak kurtarılacağı garanti edilir. Bir uygulama eşleştirilmiş olmayan bölgeler arasında dağıtılırsa, kurtarma gecikebilir – en kötü durumda seçilen bölgeler kurtarılacak son iki bölge olabilir.

8. **Ardışık güncelleştirmeler** – Planlı Azure sistem güncelleştirmeleri, kapalı kalma süresini, hataların etkisini ve kötü bir güncelleştirme durumundaki mantıksal hataları en aza indirmek için sırayla eşleştirilmiş bölgelere (aynı anda değil) dağıtılır.

9. **Veri ikamet** - Bir bölge vergi ve kolluk yargı amaçları için veri ikamet gereksinimlerini karşılamak için çifti (Brezilya Güney hariç) aynı coğrafya içinde bulunur.
