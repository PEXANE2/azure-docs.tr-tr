---
title: Azure Integration Runtime IP adresleri
description: Veri depolarına ağ erişimini sağlamak için güvenlik duvarlarını düzgün bir şekilde yapılandırmak için gelen trafiğin hangi IP adreslerinden izin vermesi gerektiğini öğrenin.
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
ms.openlocfilehash: 598876e12fe04129692d0c9a842f4edb2ec00768
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79086800"
---
# <a name="azure-integration-runtime-ip-addresses"></a>Azure Integration Runtime IP adresleri

Azure Tümleştirme Çalışma Zamanı'nın kullandığı IP adresleri, Azure tümleştirme çalışma sürenizin bulunduğu bölgeye bağlıdır. *Tümü* Aynı bölgede bulunan Azure tümleştirme çalışma saatleri aynı IP adresi aralıklarını kullanır.

> [!IMPORTANT]  
> Veri akışları şu anda bu IP'leri kullanmaz. 
>
> Bu IP aralıklarını Veri Hareketi, Pipeline ve Dış etkinlik yürütmeleri için kullanabilirsiniz. Bu IP aralıkları, Azure Tümleştirme çalışma zamanından gelen erişim için veri depolarında/ Ağ Güvenlik Grubu 'nda (NSG)/ Güvenlik Duvarları'nda beyaz liste için kullanılabilir. 

## <a name="azure-integration-runtime-ip-addresses-specific-regions"></a>Azure Tümleştirme Çalışma Zamanı IP adresleri: Belirli bölgeler

Kaynaklarınızın bulunduğu belirli Azure bölgesinde Azure Tümleştirme çalışma zamanı için listelenen IP adreslerinden gelen trafiğe izin verin:

|                | Bölge              | IP adresleri                                                 |
| -------------- | ------------------- | ------------------------------------------------------------ |
| Asya           | Doğu Asya           | 20.189.104.128/25, </br>20.189.106.0/26, </br>13.75.39.112/28 |
| &nbsp;         | Güneydoğu Asya      | 20.43.128.128/25, </br>20.43.130.0/26, </br>40.78.236.176/28 |
| Avustralya      | Doğu Avustralya      | 20.37.193.0/25,</br>20.37.193.128/26,</br>13.70.74.144/28    |
| &nbsp;         | Güneydoğu Avustralya | 20.42.225.0/25,</br>20.42.225.128/26,</br>13.77.53.160/28    |
| Brezilya         | Güney Brezilya        | 191.235.224.128/25,</br>191.235.225.0/26,</br>191.233.205.160/28 |
| Kanada         | Orta Kanada      | 52.228.80.128/25,</br>52.228.81.0/26,</br>13.71.175.80/28    |
| Çin          | Çin Doğu 2        | 40.73.172.48/28,</br>52.130.0.128/25,</br>52.130.1.0/26      |
| Avrupa         | Kuzey Avrupa        | 20.38.82.0/23,</br>20.38.80.192/26,</br>13.69.230.96/28      |
| &nbsp;         | Batı Avrupa         | 40.74.26.0/23,</br>40.74.24.192/26,</br>13.69.67.192/28      |
| Fransa         | Orta Fransa      | 20.43.40.128/25,</br>20.43.41.0/26,</br>40.79.132.112/28     |
| Hindistan          | Orta Hindistan       | 52.140.104.128/25,</br>52.140.105.0/26,</br>20.43.121.48/28  |
| Japonya          | Doğu Japonya          | 20.43.64.128/25,</br>20.43.65.0/26,</br>13.78.109.192/28     |
| Güney Kore          | Güney Kore - Orta       | 20.41.64.128/25,</br>20.41.65.0/26,</br>52.231.20.64/28      |
| Güney Afrika   | Güney Afrika Kuzey  | 102.133.124.104/29,</br>102.133.216.128/25,</br>102.133.217.0/26 |
| Birleşik Krallık | Güney Birleşik Krallık            | 51.104.24.128/25,</br>51.104.25.0/26,</br>51.104.9.32/28     |
| Amerika Birleşik Devletleri  | Orta ABD          | 20.37.154.0/23,</br>20.37.156.0/26,</br>20.44.10.64/28       |
|                | Doğu ABD             | 20.42.2.0/23,</br>20.42.4.0/26,</br>40.71.14.32/28           |
|                | Doğu ABD 2            | 20.41.2.0/23,</br>20.41.4.0/26,</br>20.44.17.80/28           |
|                | Doğu ABD 2 EUAP      | 20.39.8.128/26,</br>20.39.8.96/27,</br>40.75.35.144/28       |
|                | Orta Kuzey ABD    | 40.80.185.0/24,</br>40.80.186.0/25,</br>52.162.111.48/28      |
|                | Orta Güney ABD    | 40.119.9.0/25,</br>40.119.9.128/26,</br>13.73.244.32/28      |
|                | Orta Batı ABD     | 52.150.137.128/25,</br>52.150.136.192/26,</br>13.71.199.0/28 |
|                | Batı ABD             | 40.82.250.0/23,</br>40.82.249.64/26,</br>13.86.219.208/28    |
|                | Batı ABD 2            | 20.42.132.0/23,</br>20.42.129.64/26,</br>13.66.143.128/28    |
|                | US Gov Virginia     | 52.127.45.96/28,</br>52.127.48.128/25,</br>52.127.49.0/26    |

## <a name="known-issue-with-azure-storage"></a>Azure Depolama ile bilinen sorun

* Azure Depolama hesabına bağlanırken, IP ağ kurallarının depolama hesabıyla aynı bölgede bulunan Azure tümleştirme çalışma zamanından kaynaklanan istekler üzerinde hiçbir etkisi yoktur. Daha fazla bilgi için lütfen [bu makaleye bakın.](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-an-internet-ip-range) 

  Bunun yerine, [Azure Depolama'ya bağlanırken güvenilir hizmetleri](https://techcommunity.microsoft.com/t5/azure-data-factory/data-factory-is-now-a-trusted-service-in-azure-storage-and-azure/ba-p/964993)kullanmanızı öneririz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Fabrikası'nda veri hareketi için güvenlik konuları](data-movement-security-considerations.md)
