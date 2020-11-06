---
title: Azure Data Factory veri artıklığı | Microsoft Docs
description: Azure Data Factory meta veri artıklığı mekanizmaları hakkında bilgi edinin
services: data-factory
documentationcenter: ''
author: nabhishek
manager: weehyongtok
ms.reviewer: abnarain
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/05/2020
ms.author: abnarain
ms.openlocfilehash: f71fdf66624d67939f915f91c2cc1dbe7553cad7
ms.sourcegitcommit: 46c5ffd69fa7bc71102737d1fab4338ca782b6f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94332158"
---
# <a name="azure-data-factory-data-redundancy"></a>**Azure Data Factory veri artıklığı**

Azure Data Factory veriler meta veriler (işlem hattı, veri kümeleri, bağlı hizmetler, tümleştirme çalışma zamanı ve Tetikleyiciler) ve izleme verileri (işlem hattı, tetikleyici ve etkinlik çalıştırmaları) içerir. 

Tüm bölgelerde (Brezilya Güney ve Güneydoğu Asya dışında), Azure Data Factory verileri, meta veri kaybına karşı korumak için [eşleştirilmiş bölgede](https://docs.microsoft.com/azure/best-practices-availability-paired-regions#azure-regional-pairs) depolanır ve çoğaltılır. Bölgesel veri merkezi sorunları sırasında, Microsoft Azure Data Factory örneğinizin bölgesel bir yük devretmesini başlatabilir. Çoğu durumda, sizin bölüminizdeki herhangi bir eylem gerekmez. Microsoft tarafından yönetilen yük devretme tamamlandığında, yük devretme bölgesindeki Azure Data Factory erişebileceksiniz. 

Brezilya Güney ve Güneydoğu Asya 'daki veri fazlalığı nedeniyle Azure Data Factory veriler [yalnızca yerel bölgede](https://docs.microsoft.com/azure/storage/common/storage-redundancy#locally-redundant-storage)depolanır. Güneydoğu Asya için tüm veriler Singapur 'da depolanır. Brezilya Güney için tüm veriler Brezilya 'da depolanır. Bölgenin önemli bir olağanüstü durum nedeniyle kaybolması durumunda Microsoft Azure Data Factory verilerinizi kurtaramayacak.  

> [!NOTE]
> Microsoft tarafından yönetilen yük devretme, bu altyapı genellikle müşteri tarafından yönetildiğinden bu yana şirket içinde barındırılan tümleştirme çalışma zamanı (SHıR) için geçerlidir. Azure VM 'de SHıR ayarlandıysa, Azure [VM yük devretmesini](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-architecture) başka bir bölgeye Işlemek için [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) 'den faydalanır.



## <a name="using-source-control-in-azure-data-factory"></a>**Azure Data Factory 'da kaynak denetimini kullanma**

Azure Data Factory meta verileriniz üzerinde yapılan değişiklikleri izleyebilmeniz ve denetleyeceğinizi sağlamak için, Azure Data Factory kaynak denetimi ayarlamayı göz önünde bulundurmanız gerekir. Ayrıca, işlem hatları, veri kümeleri, bağlı hizmetler ve tetikleyici için meta veri JSON dosyalarınıza erişmenizi sağlar. Azure Data Factory, farklı bir git deposu (Azure DevOps ve GitHub) ile çalışmanıza olanak sağlar. 

 [Azure Data Factory ' de kaynak denetimi](https://docs.microsoft.com/azure/data-factory/source-control)ayarlamayı öğrenin. 

> [!NOTE]
> Olağanüstü bir durum oluşması durumunda (bölge kaybı), yeni veri fabrikası el ile veya otomatik bir biçimde sağlanabilir. Yeni Veri Fabrikası oluşturulduktan sonra, işlem hatlarınızı, veri kümelerinizi ve bağlı hizmetleri JSON ' ı mevcut git deposundan geri yükleyebilirsiniz. 



## <a name="data-stores"></a>**Veri depolama alanları**

Azure Data Factory, verileri şirket içinde ve bulutta bulunan veri depoları arasında taşımanızı sağlar. Veri depolarınız ile iş sürekliliği sağlamak için, bu veri depolarının her biri için iş sürekliliği önerilerine başvurmalısınız. 

 

## <a name="see-also"></a>Ayrıca bkz.

- [Azure bölgesel çiftleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)
- [Azure 'da veri yerleşimi](https://azure.microsoft.com/global-infrastructure/data-residency/) 
