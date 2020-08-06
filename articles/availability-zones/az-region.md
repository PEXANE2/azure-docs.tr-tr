---
title: Azure 'da Kullanılabilirlik Alanları destekleyen bölgeler
description: Azure 'da yüksek oranda kullanılabilir ve dayanıklı uygulamalar oluşturmak için, kaynaklarınızı çalıştırmak için kullanabileceğiniz fiziksel olarak ayrı konumlar sağlayın Kullanılabilirlik Alanları.
author: cynthn
ms.service: azure
ms.topic: article
ms.date: 08/04/2020
ms.author: cynthn
ms.custom: fasttrack-edit, mvc, references_regions
ms.openlocfilehash: e13d7a1cb35d2b0c9544a8be36f834efc6970501
ms.sourcegitcommit: 5a37753456bc2e152c3cb765b90dc7815c27a0a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87759657"
---
# <a name="regions-that-support-availability-zones-in-azure"></a>Azure 'da Kullanılabilirlik Alanları destekleyen bölgeler

Kullanılabilirlik Alanları, uygulamalarınızı ve verilerinizi veri merkezi hatalarından koruyan yüksek kullanılabilirliğe sahip bir tekliftir. Kullanılabilirlik Alanları hakkında daha fazla bilgi için bkz. [Azure 'Da bölgeler ve kullanılabilirlik alanları](az-overview.md).

## <a name="services-support-by-region"></a>Bölgeye göre hizmet desteği

Bu bölümde Kullanılabilirlik Alanları destekleyen Azure hizmetleri ve bölgeleri listelenmektedir.

Her bölgede kullanılabilir olan hizmetler, kullanılabilirlik için yaklaşan yol haritası ile birlikte [bölge tarafından kullanılabilen ürünlerde](https://azure.microsoft.com/global-infrastructure/services/)bulunabilir.

| Hizmet |Kuzey: Orta ABD|Kuzey: Doğu ABD|Kuzey: Doğu ABD 2|Kuzey: Batı ABD 2|Avrupa: Fransa Orta|Avrupa: Kuzey Avrupa|Avrupa: UK Güney|Avrupa: Batı Avrupa|Asya Pasifik: Japonya Doğu|Asya Pasifik: Güneydoğu Asya|Asya Pasifik: Avustralya Doğu|
|----------------------------|----------|----------|---------|---------|--------------|------------|--------|----------|----------|-------------|-------------|
| **İşlem**                         |            |              |           |           |                |              |          |             |            |                |                |
| Linux Sanal Makineleri          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       | &#10003;               |
| Windows Sanal Makineleri        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       | &#10003;               |
| Sanal Makine Ölçek Kümeleri      | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       | &#10003;               |
| Azure App Service ortamları ıLB | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Azure Kubernetes Service        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| **Depolama**   |            |              |           |           |                |              |          |             |            |                |                |
| Yönetilen Diskler                   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       | &#10003;               |
| Bölgesel olarak yedekli depolama          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       | &#10003;               |
| **Ağ**                     |            |              |           |           |                |              |          |             |            |                |                |
| Standart IP adresi        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       | &#10003;               |
| Standart Load Balancer     | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       | &#10003;               |
| VPN Gateway            | &#10003;   |  &#10003;    | &#10003;  | &#10003;  | &#10003;       | &#10003;     |  &#10003;  | &#10003;    |  &#10003;   | &#10003;       | &#10003;               |
| ExpressRoute Ağ Geçidi   | &#10003;   |  &#10003;    | &#10003;  | &#10003;  | &#10003;       | &#10003;     |  &#10003;  | &#10003;    |  &#10003;   | &#10003;       | &#10003;               |
| Application Gateway (v2)    | &#10003;   |  &#10003;    | &#10003;  | &#10003;  | &#10003;       | &#10003;     |  &#10003;  | &#10003;    |  &#10003;   | &#10003;       | &#10003;               |
| Azure Güvenlik Duvarı           | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    |  &#10003;       | &#10003;       |                |
| **Veritabanları**                     |            |              |           |           |                |              |          |             |            |                |                |
| Azure Veri Gezgini                   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;        | &#10003;       |                |
| SQL Veritabanı                    | &#10003;   | &#10003;     | &#10003;  | &#10003; (Önizleme) | &#10003;       | &#10003; (Önizleme)     | &#10003; | &#10003;    | &#10003;       | &#10003;       |&#10003;        |
| Redis için Azure Önbelleği           | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    |  &#10003;       | &#10003;       |                |
| Azure Cosmos DB      | &#10003;   |  &#10003;  |  &#10003; | &#10003; |  &#10003;  | &#10003;    | &#10003; |  &#10003;   |     &#10003;       | &#10003;    | &#10003;  |
| **Analiz**                       |            |              |           |           |                |              |          |             |            |                |                |
| Event Hubs                      | &#10003;   |   &#10003; | &#10003;  | &#10003;  | &#10003; | &#10003; | &#10003; | &#10003; | &#10003; | &#10003;       |  &#10003;              |
| **Tümleştirme**                     |            |              |           |           |                |              |          |             |            |                |                |
| Service Bus (yalnızca Premium katman) | &#10003;   |  &#10003;  | &#10003;  | &#10003;  | &#10003;  | &#10003;     |&#10003;   | &#10003;    |&#10003;      | &#10003;       |  &#10003;              |
| Event Grid | &#10003;   |  &#10003;  | &#10003;  | &#10003;  | &#10003;  | &#10003;     |&#10003;   | &#10003;    |&#10003;      | &#10003;       |                |
| **Kimlik**                     |            |              |           |           |                |              |          |             |            |                |                |
| Azure AD Domain Services | &#10003;   |  &#10003;  | &#10003;  | &#10003;  | &#10003;  | &#10003;     |&#10003;   | &#10003;    |&#10003;      | &#10003;       |                |

Azure ayrıca aşağıdaki bölgelerde Kullanılabilirlik Alanları destek sunar:

- US Gov Virginia
- Güney Afrika Kuzey
- Orta Güney ABD
- Orta Kanada

Bu dört bölgede Kullanılabilirlik Alanları destek hakkında daha fazla bilgi edinmek için Microsoft Sales veya müşteri temsilcinizle iletişim kurun veya bir teknik destek isteği açın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure’daki Bölgeler ve Kullanılabilirlik Alanları](az-overview.md)
