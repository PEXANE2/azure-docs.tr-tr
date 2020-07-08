---
title: Azure Data Lake Storage 1. için olağanüstü durum kurtarma Kılavuzu | Microsoft Docs
description: Azure Data Lake Storage 1. için olağanüstü durum kurtarma Kılavuzu
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: twooley
ms.openlocfilehash: b33977ca5184ea07b5651be18e3a132d30ce4b39
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75966066"
---
# <a name="disaster-recovery-guidance-for-data-in-azure-data-lake-storage-gen1"></a>Azure Data Lake Storage 1. veri için olağanüstü durum kurtarma Kılavuzu

Azure Data Lake Storage 1. yerel olarak yedekli depolama (LRS) sağlar. Bu nedenle, Data Lake Storage 1. hesabınızdaki veriler otomatik çoğaltmalar aracılığıyla bir veri merkezindeki geçici donanım arızalarına dayanıklı olur. Bu, dayanıklılık ve yüksek kullanılabilirlik sağlar ve Data Lake Storage 1. SLA 'yı ister. Bu makalede, verilerinizi nadir bölge genelinde kesintiler veya yanlışlıkla silinmelerden daha fazla korumaya yönelik yönergeler sunulmaktadır.

## <a name="disaster-recovery-guidance"></a>Olağanüstü durum kurtarma kılavuzu
Her müşterinin kendi olağanüstü durum kurtarma planını hazırlaması kritik öneme sahiptir. Olağanüstü durum kurtarma planınızı oluşturmak için bu makaledeki bilgileri okuyun. Kendi planınızı oluşturmanıza yardımcı olabilecek bazı kaynaklar aşağıda verilmiştir.

* [Azure uygulamaları için olağanüstü durum kurtarma ve yüksek kullanılabilirlik](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Azure dayanıklılık teknik kılavuzu](../resiliency/resiliency-technical-guidance.md)

### <a name="best-practices"></a>En iyi uygulamalar
Kritik verilerinizi başka bir bölgedeki başka bir Data Lake Storage 1. hesabına, olağanüstü durum kurtarma planınızın ihtiyaçlarına göre hizalı bir sıklık ile kopyalamanızı öneririz. Verileri kopyalamak için [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) veya [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) dahil olmak üzere çeşitli yöntemler mevcuttur. Azure Data Factory, yinelenen bir düzende veri taşıma işlem hatları oluşturmak ve dağıtmak için kullanışlı bir hizmettir.

Bölgesel bir kesinti oluşursa, verilerin kopyalandığı bölgedeki verilerinize erişebilirsiniz. Azure hizmet durumu [panosunu](https://azure.microsoft.com/status/) , dünya genelinde Azure hizmet durumunu tespit etmek için izleyebilirsiniz.

## <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Verilerin bozulması veya yanlışlıkla silinmesi durumunda kurtarma kılavuzu
Data Lake Storage 1. otomatikleştirilmiş çoğaltmalar aracılığıyla veri dayanıklılığı sağlarken, bu, uygulamanızın (veya geliştiricilerin/kullanıcıların) verileri bozmasını veya yanlışlıkla silmesini engellemez.

### <a name="best-practices"></a>En iyi uygulamalar
Yanlışlıkla silinmeye engel olmak için, Data Lake Storage 1. hesabınız için ilk olarak doğru erişim ilkelerini ayarlamanızı öneririz.  Bu, önemli kaynakları kilitlemek ve kullanılabilir [Data Lake Storage 1. güvenlik özelliklerini](data-lake-store-security-overview.md)kullanarak hesap ve dosya düzeyi erişim denetimi uygulamak için [Azure kaynak kilitlerini](../azure-resource-manager/management/lock-resources.md) uygulamayı içerir. Ayrıca, başka bir Data Lake Storage 1. hesabı, klasörü veya Azure aboneliğine [AdlCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) veya [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) kullanarak kritik verilerinizin kopyalarını düzenli olarak oluşturmanızı öneririz.  Bu yöntem, verilerin bozulması veya silinmesi durumunda kurtarılması için de kullanılabilir. Azure Data Factory, yinelenen bir düzende veri taşıma işlem hatları oluşturmak ve dağıtmak için kullanışlı bir hizmettir.

Kuruluşlar, bir dosyayı kimin sildiği veya güncelleştirmiş olabileceği hakkında bilgi sağlayan veri erişim denetimi izlerini toplamak üzere Data Lake Storage 1. hesabının [Tanılama günlüğünü](data-lake-store-diagnostic-logs.md) de etkinleştirebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Data Lake Storage 1. kullanmaya başlayın](data-lake-store-get-started-portal.md)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)

