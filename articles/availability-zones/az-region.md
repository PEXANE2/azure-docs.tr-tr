---
title: Azure 'da Kullanılabilirlik Alanları destekleyen bölgeler
description: Azure 'da yüksek oranda kullanılabilir ve dayanıklı uygulamalar oluşturmak için, kaynaklarınızı çalıştırmak için kullanabileceğiniz fiziksel olarak ayrı konumlar sağlayın Kullanılabilirlik Alanları.
author: cynthn
ms.service: azure
ms.topic: article
ms.date: 04/27/2020
ms.author: cynthn
ms.custom: fasttrack-edit, mvc, references_regions
ms.openlocfilehash: 903bfd0be6c82cb5836248a784805b45edcabf36
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84196385"
---
# <a name="regions-that-support-availability-zones-in-azure"></a>Azure 'da Kullanılabilirlik Alanları destekleyen bölgeler

Kullanılabilirlik Alanları, uygulamalarınızı ve verilerinizi veri merkezi hatalarından koruyan yüksek kullanılabilirliğe sahip bir tekliftir. Kullanılabilirlik Alanları hakkında daha fazla bilgi için bkz. [Azure 'Da bölgeler ve kullanılabilirlik alanları](az-overview.md).

## <a name="services-support-by-region"></a>Bölgeye göre hizmet desteği

Bu bölümde Kullanılabilirlik Alanları destekleyen Azure hizmetleri ve bölgeleri listelenmektedir.

Her bölgede kullanılabilir olan hizmetler, kullanılabilirlik için yaklaşan yol haritası ile birlikte [bölge tarafından kullanılabilen ürünlerde](https://azure.microsoft.com/global-infrastructure/services/)bulunabilir.

|                                 |Kuzey ve Güney Amerika |              |           |           | Avrupa |              |          |              | Asya Pasifik |                 |                |
|----------------------------|----------|----------|---------|---------|--------------|------------|--------|----------|----------|-------------|-------------|
|          |Orta ABD|Doğu ABD|Doğu ABD 2|Batı ABD 2|Orta Fransa|Kuzey Avrupa|Güney Birleşik Krallık|Batı Avrupa|Doğu Japonya|Güneydoğu Asya|Doğu Avustralya|
| **İşlem**                         |            |              |           |           |                |              |          |             |            |                |                |
| Linux Sanal Makineleri          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Windows Sanal Makineleri        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Sanal Makine Ölçek Kümeleri      | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Azure App Service ortamları ıLB | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Azure Kubernetes Service        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| **Depolama**   |            |              |           |           |                |              |          |             |            |                |                |
| Yönetilen Diskler                   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Bölgesel olarak yedekli depolama          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| **Ağ**                     |            |              |           |           |                |              |          |             |            |                |                |
| Standart IP adresi        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Standart Load Balancer     | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| VPN Gateway            | &#10003;   |  &#10003;    | &#10003;  | &#10003;  | &#10003;       | &#10003;     |  &#10003;  | &#10003;    |  &#10003;   | &#10003;       |                |
| ExpressRoute Ağ Geçidi   | &#10003;   |  &#10003;    | &#10003;  | &#10003;  | &#10003;       | &#10003;     |  &#10003;  | &#10003;    |  &#10003;   | &#10003;       |                |
| Application Gateway (v2)    | &#10003;   |  &#10003;    | &#10003;  | &#10003;  | &#10003;       | &#10003;     |  &#10003;  | &#10003;    |  &#10003;   | &#10003;       |                |
| Azure Güvenlik Duvarı           | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    |  &#10003;       | &#10003;       |                |
| **Veritabanları**                     |            |              |           |           |                |              |          |             |            |                |                |
| Azure Veri Gezgini                   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;        | &#10003;       |                |
| SQL Veritabanı                    | &#10003;   | &#10003;     | &#10003;  | &#10003; (Önizleme) | &#10003;       | &#10003; (Önizleme)     | &#10003; | &#10003;    | &#10003;       | &#10003;       |&#10003;        |
| Redis için Azure Önbelleği           | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    |  &#10003;       | &#10003;       |                |
| Azure Cosmos DB      | &#10003;   |  &#10003;  |  &#10003; | &#10003; |  &#10003;  | &#10003;    | &#10003; |  &#10003;   |     &#10003;       | &#10003;    | &#10003;    |
| **Analiz**                       |            |              |           |           |                |              |          |             |            |                |                |
| Event Hubs                      | &#10003;   |   &#10003; | &#10003;  | &#10003;  | &#10003; | &#10003; | &#10003; | &#10003; | &#10003; | &#10003;       |                |
| **Tümleştirme**                     |            |              |           |           |                |              |          |             |            |                |                |
| Service Bus (yalnızca Premium katman) | &#10003;   |  &#10003;  | &#10003;  | &#10003;  | &#10003;  | &#10003;     |&#10003;   | &#10003;    |&#10003;      | &#10003;       |                |
| Event Grid | &#10003;   |  &#10003;  | &#10003;  | &#10003;  | &#10003;  | &#10003;     |&#10003;   | &#10003;    |&#10003;      | &#10003;       |                |
| **Kimlik**                     |            |              |           |           |                |              |          |             |            |                |                |
| Azure AD Domain Services | &#10003;   |  &#10003;  | &#10003;  | &#10003;  | &#10003;  | &#10003;     |&#10003;   | &#10003;    |&#10003;      | &#10003;       |                |


## <a name="next-steps"></a>Sonraki adımlar

- [Azure 'da bölgeler ve Kullanılabilirlik Alanları](az-overview.md)
