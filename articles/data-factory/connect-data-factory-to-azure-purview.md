---
title: Azure purview 'e bir Data Factory bağlama
description: Azure purview 'a bir Data Factory bağlama hakkında bilgi edinin
services: data-factory
ms.author: lle
author: lrtoyou1223
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom:
- seo-lt-2019
- references_regions
ms.date: 12/3/2020
ms.openlocfilehash: 94b2ed8a25ca5cc837f6677dea1c0bbb54225fcd
ms.sourcegitcommit: c4246c2b986c6f53b20b94d4e75ccc49ec768a9a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2020
ms.locfileid: "96603353"
---
# <a name="connect-data-factory-to-azure-purview-preview"></a>Data Factory Azure purview 'a bağlanma (Önizleme)
[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu makale, Data Factory 'yi Azure purview 'e bağlamayı ve ADF etkinliklerinin veri kökenini, veri akışını ve SSIS paketini yürütmeyi nasıl bildirebileceğini açıklar.

## <a name="connect-data-factory-to-azure-purview"></a>Data Factory 'yi Azure purview 'a bağlama
Azure purview, veri kullanıcılarının bulut ve şirket içi ortamları kapsayan verileri genelinde veri yönetimini merkezi olarak yönetmesine yönelik yeni bir bulut hizmetidir. Veri fabrikanızı Azure Mağazası 'na bağlayabilirsiniz ve bağlantı, kopyalama, veri akışı ve SSIS paketi yürütme kökenini verilerini yakalamak için Azure takip görünümü 'nden yararlanmanızı sağlar. Azure purview 'da Data Factory 'yi kaydetme hakkında bilgi için bkz. [bağlanma Azure Data Factory ve Azure purview](https://docs.microsoft.com/azure/purview/how-to-link-azure-data-factory). 

## <a name="report-lineage-data-to-azure-purview"></a>Kökenini verilerini Azure purview 'a raporla
Müşteriler, Azure Data Factory 'de kopyalama, veri akışı veya SSIS paketi yürütme etkinliğini çalıştırdığınızda, müşteriler bağımlılık ilişkisini alabilir ve veri kaynakları ve hedef arasındaki tüm iş akışı işlemleri için üst düzey bir genel bakışa sahip olabilir.
Azure Data Factory 'den kökenini nasıl toplayacağınız hakkında bilgi için bkz. [Data Factory kökenini](https://docs.microsoft.com/azure/purview/how-to-link-azure-data-factory#supported-azure-data-factory-activities).

## <a name="next-steps"></a>Sonraki adımlar
[Catalog kökenini Kullanıcı Kılavuzu](https://docs.microsoft.com/azure/purview/catalog-lineage-user-guide)

[Öğretici: Azure purview 'a Data Factory kökenini verileri gönderme](turorial-push-lineage-to-purview.md)