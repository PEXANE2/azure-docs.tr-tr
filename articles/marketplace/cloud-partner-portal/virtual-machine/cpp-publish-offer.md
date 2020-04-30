---
title: Azure Marketi 'nde bir sanal makine teklifi yayımlama
description: Mevcut bir sanal makine teklifini Azure Marketi üzerine yayımlamak için gereken adımları listeler.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: dsindona
ms.openlocfilehash: f66ce0c15e976898d5022bf5705a82fe0969ec07
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82147653"
---
# <a name="publish-a-virtual-machine-offer"></a>Bir sanal makine teklifi yayımlama

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure sanal makine tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure sanal makine oluşturma teklifi](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer) ' nde bulunan yönergeleri izleyin.

 Son adım, portalda teklif tanımladıktan ve ilişkili teknik varlıkları oluşturduktan sonra, teklifi yayımlama için gönderirsiniz. Aşağıdaki diyagramda yayımlama işlemindeki "canlı çalış" ile ana adımlar gösterilmektedir:

![Sanal Makine teklifi için yayımlama adımları](./media/publishvm_013.png)

Aşağıdaki tabloda bu adımlar açıklanmakta ve tamamlanması için en fazla bir süre tahmini sunulmaktadır:
<!-- we need to tell them that if an offer seems stuck in a step, to know that they should file a support ticket (link to support ticket doc) -->


|  **Yayımlama adımı**           | **Işınızda**    | **Açıklama**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Önkoşulları doğrulama         | 15 dk   | Teklif bilgileri ve teklif ayarları onaylanır.                        |
| Sınama sürücüsü doğrulaması (isteğe bağlı) | 2 saat | Test sürücüsünü etkinleştirmeyi seçtiyseniz, Microsoft test sürücüsü yapılandırmasını, dağıtımını ve çoğaltmasını seçili bölgeler aracılığıyla doğrular. |
| Sertifika                  | 3 gün | Teklif, Azure Sertifika ekibi tarafından çözümlenir. Bu adım, virüsler, kötü amaçlı yazılım, güvenlik uyumluluğu ve güvenlik sorunları için taramalar gerçekleştirir. Sorun bulunursa geri bildirim sağlanır. |
| Sağlama                   | 4 gün   | VM teklifi market üretim sistemlerinde çoğaltılır.               |
| Paketleme ve lider oluşturma kaydı | \<1 saat  | Teklifin teknik varlıkları müşteri kullanımı için paketlenmiştir ve müşteri adayı sistemleri yapılandırılır ve ayarlanır. |
|  Yayımcı oturumu kapatma             |  -        | Teklifin etkin olmadan önce son yayımcı incelemesi ve onayı. Teklifinizi, tüm gereksinimlerinizi karşıladığından emin olmak için seçili aboneliklerde (teklif bilgileri adımlarında) dağıtabilirsiniz.  |
| Sağlama                   | 4 gün | Son VM teklifi market üretim sistemleri ve bölgelerinde çoğaltılır. | 
| Canlı                           | 4 gün | VM teklifi serbest bırakılır, gerekli bölgelere çoğaltılır ve herkese açık hale getirilir. |
|  |  |

Bu işlemin tamamlanabilmesi için 16 güne kadar izin verin.  Bu yayımlama adımlarını tamamladıktan sonra, sanal makine teklifiniz [Microsoft Azure Market](https://azuremarketplace.microsoft.com/marketplace/)listelenecektir. 

