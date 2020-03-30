---
title: Azure Veri Gölü Depolama Gen1 için olağanüstü durum kurtarma kılavuzu | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen1 için olağanüstü durum kurtarma kılavuzu
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75966066"
---
# <a name="disaster-recovery-guidance-for-data-in-azure-data-lake-storage-gen1"></a>Azure Veri Gölü Depolama Gen1'deki veriler için olağanüstü durum kurtarma kılavuzu

Azure Veri Gölü Depolama Gen1, yerel olarak yedekli depolama (LRS) sağlar. Bu nedenle, Veri Gölü Depolama Gen1 hesabınızdaki veriler, otomatik yinelemeler aracılığıyla bir veri merkezi içindeki geçici donanım hatalarına karşı dayanıklıdır. Bu, Veri Gölü Depolama Gen1 SLA'yı karşılarak dayanıklılık ve yüksek kullanılabilirlik sağlar. Bu makalede, verilerinizi bölge genelinde ki nadir kesintilere veya yanlışlıkla silmelere karşı daha fazla nasıl koruyacağınız konusunda kılavuz luklar sağlanmaktadır.

## <a name="disaster-recovery-guidance"></a>Olağanüstü durum kurtarma kılavuzu
Her müşterinin kendi olağanüstü durum kurtarma planını hazırlaması kritik öneme sahiptir. Olağanüstü durum kurtarma planınızı oluşturmak için bu makaledeki bilgileri okuyun. Kendi planınızı oluşturmanıza yardımcı olabilecek bazı kaynaklar aşağıda verilmiştir.

* [Azure uygulamaları için olağanüstü durum kurtarma ve yüksek kullanılabilirlik](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Azure dayanıklılık teknik kılavuzu](../resiliency/resiliency-technical-guidance.md)

### <a name="best-practices"></a>En iyi uygulamalar
Kritik verilerinizi, olağanüstü durum kurtarma planınızın gereksinimlerine uygun bir sıklıkta başka bir bölgedeki başka bir Veri Gölü Depolama Gen1 hesabına kopyalamanızı öneririz. Verileri kopyalamak için [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) veya [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) dahil olmak üzere çeşitli yöntemler mevcuttur. Azure Data Factory, yinelenen bir düzende veri taşıma işlem hatları oluşturmak ve dağıtmak için kullanışlı bir hizmettir.

Bölgesel bir kesinti oluşursa, verilerinizin kopyalandığı bölgede verilerinize erişebilirsiniz. Tüm dünyada Azure hizmet durumunu belirlemek için [Azure Hizmet Durumu Panosu'nu](https://azure.microsoft.com/status/) izleyebilirsiniz.

## <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Verilerin bozulması veya yanlışlıkla silinmesi durumunda kurtarma kılavuzu
Veri Gölü Depolama Gen1 otomatik yinelemeler aracılığıyla veri esnekliği sağlarken, bu uygulamanızın (veya geliştiricilerin/kullanıcıların) verileri bozmasını veya yanlışlıkla silmesine engel olmaz.

### <a name="best-practices"></a>En iyi uygulamalar
Yanlışlıkla silinmesini önlemek için, önce Veri Gölü Depolama Gen1 hesabınız için doğru erişim ilkelerini ayarlamanızı öneririz.  Buna önemli kaynakları kilitlemek için [Azure kaynak kilitleri](../azure-resource-manager/management/lock-resources.md) uygulamanın yanı sıra kullanılabilir [Veri Gölü Depolama Gen1 güvenlik özelliklerini](data-lake-store-security-overview.md)kullanarak hesap ve dosya düzeyi erişim denetimi uygulama dahildir. Ayrıca, [adlcopy,](data-lake-store-copy-data-azure-storage-blob.md) [Azure PowerShell](data-lake-store-get-started-powershell.md) veya [Azure Veri Fabrikası'nı](../data-factory/connector-azure-data-lake-store.md) kullanarak kritik verilerinizin kopyalarını başka bir Veri Gölü Depolama Gen1 hesabında, klasöründe veya Azure aboneliğinde düzenli olarak oluşturmanızı öneririz.  Bu yöntem, verilerin bozulması veya silinmesi durumunda kurtarılması için de kullanılabilir. Azure Data Factory, yinelenen bir düzende veri taşıma işlem hatları oluşturmak ve dağıtmak için kullanışlı bir hizmettir.

Kuruluşlar ayrıca, bir dosyayı kimin silmiş veya güncelleştirmiş olabileceği hakkında bilgi sağlayan veri erişim denetim izlerini toplamak için Veri Gölü Depolama Gen1 hesapları için [tanısal günlüğe kaydetmeyi](data-lake-store-diagnostic-logs.md) de etkinleştirebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Veri Gölü Depolama Gen1 ile Başlayın](data-lake-store-get-started-portal.md)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)

