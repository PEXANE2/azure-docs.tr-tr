---
title: Azure Integration Runtime IP adresleri
description: Veri depolarına ağ erişiminin güvenliğini sağlamak üzere güvenlik duvarlarını doğru bir şekilde yapılandırmak için gelen trafiğe izin vermeniz gereken IP adreslerini öğrenin.
services: data-factory
ms.author: abnarain
author: nabhishek
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/06/2020
ms.openlocfilehash: f3eed03c585ba6b48a21b36c21cb77585456fc2c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84660270"
---
# <a name="azure-integration-runtime-ip-addresses"></a>Azure Integration Runtime IP adresleri

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Integration Runtime kullandığı IP adresleri, Azure tümleştirme çalışma zamanının bulunduğu bölgeye bağlıdır. *Tümü* Aynı bölgedeki Azure tümleştirme çalışma zamanları aynı IP adresi aralıklarını kullanır.

> [!IMPORTANT]  
> Veri akışları sabit IP aralıklarının kullanılmasını desteklemez.
>
> Bu IP aralıklarını veri taşıma, işlem hattı ve dış etkinlik yürütmeleri için kullanabilirsiniz. Bu IP aralıkları, Azure tümleştirme çalışma zamanından gelen erişim için veri depoları/ağ güvenlik grubu (NSG)/güvenlik duvarları içindeki beyaz listeye yönelik olarak kullanılabilir. 

## <a name="azure-integration-runtime-ip-addresses-specific-regions"></a>Azure Integration Runtime IP adresleri: belirli bölgeler

Kaynaklarınızın bulunduğu belirli bir Azure bölgesindeki Azure tümleştirme çalışma zamanı için listelenen IP adreslerinden gelen trafiğe izin verin. Hizmet etiketleri [IP aralığı indirme bağlantısı](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#discover-service-tags-by-using-downloadable-json-files)' ndan bir IP aralığı hizmet etiketlerinin listesini alabilirsiniz. Örneğin, Azure bölgesi **AustraliaEast**Ise, **DataFactory. AustraliaEast**' den bir IP aralığı listesi alabilirsiniz.


## <a name="known-issue-with-azure-storage"></a>Azure depolama ile ilgili bilinen sorun

* Azure depolama hesabına bağlanırken, IP ağ kuralları depolama hesabıyla aynı bölgedeki Azure tümleştirme çalışma zamanından kaynaklanan isteklere hiçbir etkiye sahip değildir. Daha ayrıntılı bilgi için lütfen [Bu makaleye bakın](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-an-internet-ip-range). 

  Bunun yerine, [Azure depolama 'ya bağlanırken Güvenilen Hizmetleri](https://techcommunity.microsoft.com/t5/azure-data-factory/data-factory-is-now-a-trusted-service-in-azure-storage-and-azure/ba-p/964993)kullanmanızı öneririz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Factory veri hareketine yönelik güvenlik konuları](data-movement-security-considerations.md)
