---
title: İş sürekliliği & olağanüstü durum kurtarma-Azure eşleştirilmiş bölgeler
description: Veri merkezi sorunları sırasında uygulamaların dayanıklı olmasını sağlamak için Azure bölgesel eşleştirme hakkında bilgi edinin.
author: rayne-wiselman
manager: carmon
ms.service: multiple
ms.topic: article
ms.date: 07/01/2019
ms.author: raynew
ms.openlocfilehash: c1e14db9dafc8b03acbeb1c6b97e5ac0e27cb0fd
ms.sourcegitcommit: 1f738a94b16f61e5dad0b29c98a6d355f724a2c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "78163057"
---
# <a name="business-continuity-and-disaster-recovery-bcdr-azure-paired-regions"></a>İş sürekliliği ve olağanüstü durum kurtarma (BCDR): Azure eşlenmiş bölgeler

## <a name="what-are-paired-regions"></a>Eşleştirilmiş bölgeler nelerdir?

Azure dünyanın dört bir yanındaki birden fazla coğrafi ortamda çalışır. Azure coğrafya, en az bir Azure bölgesi içeren, dünyanın tanımlı bir alanıdır. Bir Azure bölgesi, bir veya daha fazla veri merkezi içeren coğrafya içindeki bir alandır.

Her Azure bölgesi aynı coğrafya içindeki başka bir bölge ile eşleştirilir ve bir bölgesel çift oluşturur. Özel durum, Coğrafya dışında bir bölgeyle eşleştirilmiş Brezilya Güney. Tek seferde yalnızca bir eşleştirilmiş bölge güncelleştirildiğinden, Azure bölge çiftleri arasında Platform güncelleştirmelerini (planlı bakım) seri hale getirir. Birden çok bölgeyi etkileyen kesinti durumunda, her bir çiftin en az bir bölgesine kurtarma için öncelik alınacaktır.

![AzureGeography](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

> [!NOTE]
> Atanan Azure bölgesel çiftleri değiştirilemez.

Şekil 1 – Azure bölgesel çiftleri

| Coğrafya | Eşleştirilmiş bölgeler |  |
|:--- |:--- |:--- |
| Asya |Doğu Asya |Güneydoğu Asya |
| Avustralya |Doğu Avustralya |Güneydoğu Avustralya |
| Avustralya |Orta Avustralya |Orta Avustralya 2 |
| Brezilya |Güney Brezilya |Orta Güney ABD |
| Kanada |Orta Kanada |Doğu Kanada |
| Çin |Çin Kuzey |Çin Doğu|
| Çin |Çin Kuzey 2 |Çin Doğu 2|
| Avrupa |Kuzey Avrupa (Irlanda) |Batı Avrupa (Hollanda) |
| Fransa |Orta Fransa|Güney Fransa|
| Almanya |Orta Almanya |Kuzeydoğu Almanya |
| Hindistan |Orta Hindistan |Güney Hindistan |
| Hindistan |Batı Hindistan |Güney Hindistan |
| Japonya |Doğu Japonya |Batı Japonya |
| Güney Kore |Güney Kore - Orta |Güney Kore - Güney |
| Kuzey Amerika |Doğu ABD |Batı ABD |
| Kuzey Amerika |Doğu ABD 2 |Orta ABD |
| Kuzey Amerika |Orta Kuzey ABD |Orta Güney ABD |
| Kuzey Amerika |Batı ABD 2 |Orta Batı ABD 
| Güney Afrika | Güney Afrika Kuzey | Güney Afrika Batı
| UK |Batı Birleşik Krallık |Güney Birleşik Krallık |
| Birleşik Arap Emirlikleri | BAE Kuzey | BAE Orta
| ABD Savunma Bakanlığı |Doğu US DoD |Orta US DoD |
| ABD devleti |US Gov Arizona |US Gov Texas |
| ABD devleti |US Gov Iowa |US Gov Virginia |
| ABD devleti |US Gov Virginia |US Gov Texas |

Tablo 1-Azure bölgesel çiftlerinin eşleme

- Batı Hindistan yalnızca bir yönde eşleştirilmiş. Batı Hindistan ikincil bölgesi Güney Hindistan, ancak Güney Hindistan ikincil bölgesi Orta Hindistan.
- Brezilya Güney, kendi Coğrafya dışında bir bölgeyle eşleştirildiği için benzersizdir. Brezilya Güney ikincil bölgesi Orta Güney ABD. Orta Güney ABD ikincil bölgesi Brezilya Güney değil.
- US Gov Iowa ikincil bölgesi US Gov Virginia.
- US Gov Virginia ikincil bölgesi US Gov Teksas.
- US Gov Teksas ' ikincil bölgesi US Gov Arizona.


Azure 'un yalıtım ve kullanılabilirlik ilkelerinden faydalanmasını sağlamak için iş sürekliliği olağanüstü durum kurtarma (BCDR) ' i bölgesel çiftler arasında yapılandırmanızı öneririz. Birden çok etkin bölgeyi destekleyen uygulamalar için, mümkün olan yerlerde her iki bölgeyi de bir bölge çiftinde kullanmanızı öneririz. Bu, uygulamalar için en iyi kullanılabilirliği ve olağanüstü durum durumunda kurtarma süresini en aza indirmesini sağlayacaktır. 

## <a name="an-example-of-paired-regions"></a>Eşleştirilmiş bölgelere örnek
Aşağıdaki şekil 2 ' de olağanüstü durum kurtarma için bölgesel çifti kullanan bir kuramsal uygulama gösterilmektedir. Yeşil sayılar, üç Azure hizmetinin (Azure işlem, depolama ve veritabanı) çapraz bölge etkinliklerini ve bölgeler arasında çoğaltma için nasıl yapılandırıldığını vurgular. Eşleştirilmiş bölgeler arasında dağıtmanın benzersiz avantajları turuncu sayılarla vurgulanır.

![Eşleştirilmiş bölge avantajlarına genel bakış](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

Şekil 2 – kuramsal Azure bölgesel çifti

## <a name="cross-region-activities"></a>Çapraz bölge etkinlikleri
Şekil 2 ' de adlandırılmaktadır.

IaaS](./media/best-practices-availability-paired-regions/1Green.png) **Azure işlem (IaaS)** ![: bir olağanüstü durum sırasında kaynakların başka bir bölgede kullanılabilmesini sağlamak için daha önce ek işlem kaynakları sağlamalısınız. Daha fazla bilgi için bkz. [Azure dayanıklılık Teknik Kılavuzu](https://github.com/uglide/azure-content/blob/master/articles/resiliency/resiliency-technical-guidance.md).

![depolama](./media/best-practices-availability-paired-regions/2Green.png) **Azure Storage** -yönetilen diskler kullanıyorsanız, Azure Backup olan [çapraz bölge yedeklemeleri](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region#virtual-machines) hakkında bilgi edinin ve Azure Site Recovery Ile bir bölgeden diğer [VM 'leri çoğaltma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication) . Depolama hesapları kullanıyorsanız, coğrafi olarak yedekli depolama (GRS), bir Azure depolama hesabı oluşturulduğunda varsayılan olarak yapılandırılır. GRS ile, verileriniz birincil bölgede otomatik olarak üç kez ve eşleştirilmiş bölgede üç kez çoğaltılır. Daha fazla bilgi için bkz. [Azure Storage artıklık seçenekleri](storage/common/storage-redundancy.md).

Azure SQL](./media/best-practices-availability-paired-regions/3Green.png) Azure **SQL veritabanı** ![-Azure SQL veritabanı coğrafi çoğaltma ile, işlemlerin zaman uyumsuz çoğaltmasını dünyanın herhangi bir bölgesine yapılandırabilirsiniz; Ancak, bu kaynakları birçok olağanüstü durum kurtarma senaryosunda eşleştirilmiş bir bölgeye dağıtmanızı öneririz. Daha fazla bilgi için bkz. [Azure SQL veritabanı 'Nda coğrafi çoğaltma](sql-database/sql-database-geo-replication-overview.md).

![Kaynak Yöneticisi](./media/best-practices-availability-paired-regions/4Green.png) **Azure Resource Manager** -Kaynak Yöneticisi doğal olarak bölgeler arasında bileşenlerin mantıksal yalıtımı sağlar. Bu, bir bölgedeki mantıksal hataların diğer etkileri daha az olabilir.

## <a name="benefits-of-paired-regions"></a>Eşleştirilmiş bölgelerin avantajları
Şekil 2 ' de adlandırılmaktadır.  

![yalıtım](./media/best-practices-availability-paired-regions/5Orange.png)
**fiziksel yalıtım** – mümkün olduğunda Azure, bölgesel çiftteki veri merkezleri arasında en az 300 mil ayırmayı tercih eder, ancak bu özellik tüm coğrafi bölgelerde pratik veya mümkün değildir. Fiziksel veri merkezi ayrımı, her iki bölgeyi aynı anda etkileyen doğal felaketler, hukuki geri kalan, güç kesintileri veya fiziksel ağ kesintileri olasılığını azaltır. Yalıtım, coğrafya içindeki kısıtlamalara tabidir (Coğrafya boyutu, güç/ağ altyapısı kullanılabilirliği, yönetmelikler vb.).  

![çoğaltma](./media/best-practices-availability-paired-regions/6Orange.png)
**platform tarafından sağlanmış çoğaltma** -coğrafi olarak yedekli depolama gibi bazı hizmetler eşleştirilmiş bölgeye otomatik çoğaltma sağlar.

![kurtarma](./media/best-practices-availability-paired-regions/7Orange.png)
**Bölgesi kurtarma sırası** – geniş bir kesinti durumunda, bir bölgenin kurtarılması her çiftin dışında bir önceliğe sahiptir. Eşleştirilmiş bölgeler arasında dağıtılan uygulamaların, önceliğe sahip bölgelerden birine sahip olduğu garanti edilir. Bir uygulama eşlenmez bölgeler arasında dağıtılırsa kurtarma geciktirilebilir. en kötü durumda, seçilen bölgeler kurtarılacak son iki durumda olabilir.

**sıralı güncelleştirmeler**](./media/best-practices-availability-paired-regions/8Orange.png)
![güncelleştirmeler – planlı Azure sistem güncelleştirmeleri, kapalı kalma süresini en aza indirmek için (aynı anda değil) sıralı olarak, hataların etkisini ve hatalı bir güncelleştirmenin nadir olayında mantıksal hataları kullanıma almıştır.

![veri](./media/best-practices-availability-paired-regions/9Orange.png)
**veri** yerleşimi – bir bölge, çift ile aynı coğrafya dahilinde (Brezilya Güney dışında), vergi ve yasalar zorlama yasaları için veri fazlalığını karşılamak amacıyla yer alır.
