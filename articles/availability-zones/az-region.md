---
title: Azure 'da Kullanılabilirlik Alanları destekleyen bölgeler
description: Azure 'da yüksek oranda kullanılabilir ve dayanıklı uygulamalar oluşturmak için, kaynaklarınızı çalıştırmak için kullanabileceğiniz fiziksel olarak ayrı konumlar sağlayın Kullanılabilirlik Alanları.
author: cynthn
ms.service: azure
ms.topic: article
ms.date: 08/05/2020
ms.author: cynthn
ms.custom: fasttrack-edit, mvc, references_regions
ms.openlocfilehash: cf1fc81ea63db21d2e864c00e1987eec3d376b59
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87853169"
---
# <a name="regions-that-support-availability-zones-in-azure"></a>Azure 'da Kullanılabilirlik Alanları destekleyen bölgeler

Kullanılabilirlik Alanları, uygulamalarınızı ve verilerinizi veri merkezi hatalarından koruyan yüksek kullanılabilirliğe sahip bir tekliftir. Kullanılabilirlik Alanları hakkında daha fazla bilgi için bkz. [Azure 'Da bölgeler ve kullanılabilirlik alanları](az-overview.md).

Bu bölümde Kullanılabilirlik Alanları destekleyen Azure hizmetleri ve bölgeleri listelenmektedir.

Her bölgede kullanılabilir olan hizmetler, kullanılabilirlik için yaklaşan yol haritası ile birlikte [bölge tarafından kullanılabilen ürünlerde](https://azure.microsoft.com/global-infrastructure/services/)bulunabilir.

## <a name="americas"></a>Kuzey ve Güney Amerika

| Hizmet | Central US | Doğu ABD | Doğu ABD 2 | Batı ABD 2 |
| --- | :---: | :---: | :---: | :---: |
| **İşlem** |  |  |  |  |
| Linux Sanal Makineleri             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Windows Sanal Makineleri           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Sanal Makine Ölçek Kümeleri         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure App Service ortamları ıLB | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure Kubernetes Service           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Depolama** |  |  |  |  |
| Yönetilen Diskler                      | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Bölgesel olarak yedekli depolama             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Ağ** |  |  |  |  |
| Standart IP adresi                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Standart Load Balancer             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| VPN Gateway                        | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| ExpressRoute Ağ Geçidi               | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Application Gateway (v2)           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure Güvenlik Duvarı                     | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Veritabanları** |  |  |  |  |
| Azure Veri Gezgini                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| SQL Veritabanı                       | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | : heavy_check_mark: (Önizleme) |
| Redis için Azure Önbelleği              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure Cosmos DB                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Analiz** |  |  |  |  |
| Event Hubs                         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Tümleştirme** |  |  |  |  |
| Service Bus (yalnızca Premium katman)    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Event Grid                         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Kimlik** |  |  |  |  |
| Azure AD Domain Services           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

## <a name="europe"></a>Avrupa

| Hizmet | Orta Fransa | Kuzey Avrupa | Güney Birleşik Krallık | West Europe |
| --- | :---: | :---: | :---: | :---: |
| **İşlem** |  |  |  |  |
| Linux Sanal Makineleri             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Windows Sanal Makineleri           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Sanal Makine Ölçek Kümeleri         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure App Service ortamları ıLB | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure Kubernetes Service           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Depolama** |  |  |  |  |
| Yönetilen Diskler                      | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Bölgesel olarak yedekli depolama             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Ağ** |  |  |  |  |
| Standart IP adresi                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Standart Load Balancer             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| VPN Gateway                        | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| ExpressRoute Ağ Geçidi               | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Application Gateway (v2)           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure Güvenlik Duvarı                     | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Veritabanları** |  |  |  |  |
| Azure Veri Gezgini                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| SQL Veritabanı                       | :heavy_check_mark: | : heavy_check_mark: (Önizleme) | :heavy_check_mark: | :heavy_check_mark: |
| Redis için Azure Önbelleği              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure Cosmos DB                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Analiz** |  |  |  |  |
| Event Hubs                         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Tümleştirme**  |  |  |  |  |
| Service Bus (yalnızca Premium katman)    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Event Grid                         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Kimlik** |  |  |  |  |
| Azure AD Domain Services           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

## <a name="asia-pacific"></a>Asya Pasifik

| Hizmet | Doğu Japonya | Güneydoğu Asya | Doğu Avustralya |
| --- | :---: | :---: | :---: |
| **İşlem** |  |  |  |
| Linux Sanal Makineleri             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Windows Sanal Makineleri           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Sanal Makine Ölçek Kümeleri         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure App Service ortamları ıLB | :heavy_check_mark: | :heavy_check_mark: |  |
| Azure Kubernetes Service           | :heavy_check_mark: | :heavy_check_mark: |  |
| **Depolama** |  |  |  |
| Yönetilen Diskler                      | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Bölgesel olarak yedekli depolama             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Ağ** |  |  |  |
| Standart IP adresi                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Standart Load Balancer             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| VPN Gateway                        | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| ExpressRoute Ağ Geçidi               | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Application Gateway (v2)           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure Güvenlik Duvarı                     | :heavy_check_mark: | :heavy_check_mark: |  |
| **Veritabanları** |  |  |  |
| Azure Veri Gezgini                | :heavy_check_mark: | :heavy_check_mark: |  |
| SQL Veritabanı                       | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Redis için Azure Önbelleği              | :heavy_check_mark: | :heavy_check_mark: |  |
| Azure Cosmos DB                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Analiz** |  |  |  |
| Event Hubs                         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Tümleştirme** |  |  |  |
| Service Bus (yalnızca Premium katman)    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Event Grid                         | :heavy_check_mark: | :heavy_check_mark: |  |
| **Kimlik** |  |  |  |
| Azure AD Domain Services           | :heavy_check_mark: | :heavy_check_mark: |  |

## <a name="other"></a>Diğer

Azure ayrıca aşağıdaki bölgelerde Kullanılabilirlik Alanları destek sunar:

- US Gov Virginia
- Güney Afrika Kuzey
- Orta Güney ABD
- Orta Kanada

Bu dört bölgede Kullanılabilirlik Alanları destek hakkında daha fazla bilgi edinmek için Microsoft Sales veya müşteri temsilcinizle iletişim kurun veya bir teknik destek isteği açın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure’daki Bölgeler ve Kullanılabilirlik Alanları](az-overview.md)
