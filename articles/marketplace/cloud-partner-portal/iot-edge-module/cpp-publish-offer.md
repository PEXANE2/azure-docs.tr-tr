---
title: Azure IoT Edge modül teklifini Yayımla | Azure Marketi
description: IoT Edge modül teklifini yayımlama.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: ef73956b1b0d3e7bed6e91cde0b92bcc3e432795
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82141838"
---
# <a name="publish-iot-edge-module-offer"></a>IoT Edge modülü teklifini yayımlama

>[!Important]
>13 Nisan 2020 ' den itibaren, IoT Edge modülü tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilmiş tekliflerinizi yönetmek için [IoT Edge modülü oluşturma teklifi](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-iot-edge-module-creation) bölümündeki yönergeleri izleyin.

 **Yeni teklif** sayfasında bilgileri sağlayarak yeni bir teklif oluşturduktan sonra teklifi yayımlayabilirsiniz. Yayımlama işlemini başlatmak için **Yayımla** ' yı seçin.

Aşağıdaki diyagramda "canlı çalış" teklifinin bir teklifiyle ilgili yayımlama işlemindeki ana adımlar gösterilmektedir.

![IoT Edge modülü teklifi için yayımlama adımları](./media/iot-edge-module-publishing-steps.png)

## <a name="detailed-description-of-publishing-steps"></a>Yayımlama adımlarının ayrıntılı açıklaması

Aşağıdaki tabloda her bir adımı tamamlaması için bir süre Tahmini (maksimum) ile birlikte her bir yayımlama adımı açıklanmaktadır.
<!-- P2: we need to tell them that if an offer seems stuck in a step, to know that they should file a support ticket (link to support ticket doc) -->


|  **Yayımlama adımı**           | **Işınızda**    | **Açıklama**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Önkoşulları doğrulama         | 15 dk   | Teklif bilgileri ve teklif ayarları onaylanır.                        |
| Sertifika                  | 2 hafta | Teklif, Azure Sertifika ekibi tarafından çözümlenir. Bu adım, virüsler, kötü amaçlı yazılım, güvenlik uyumluluğu ve güvenlik sorunları için taramalar gerçekleştirir. Ayrıca, bu IoT Edge modülü teklifinin tüm uygunluk ölçütlerini karşıladığını doğrular (bkz. [Önkoşullar](./cpp-prerequisites.md) ve [Teknik varlıklarınızı hazırlama](./cpp-create-technical-assets.md)). Sorun bulunursa geri bildirim sağlanır. |
| Paketleme | 1 saat  | Teklifin teknik varlıkları müşteri kullanımı için paketlenmiştir ve müşteri adayı sistemleri yapılandırılır ve ayarlanır. |
|  Yayımcının oturumu Kapat             |  -        | Teklifin etkin olmadan önce son yayımcı incelemesi ve onayı. Teklifinizi, tüm gereksinimlerinizi karşıladığından emin olmak için seçili aboneliklerde (teklif bilgileri adımlarında) dağıtabilirsiniz.  Teklifinizin bir sonraki adıma gidebilmesi için **canlı git** ' i seçin. |
| Paketleme                 | 1 saat | Son teklif, market üretim sistemleri ve bölgelerinde çoğaltılır. | 
| Canlı                           | 4 gün |Teklif serbest bırakılır, gerekli bölgelere çoğaltılır ve herkese açık hale getirilir. |

Yayımlama işleminin tamamlanabilmesi için 10 adede kadar iş gününe izin verin ve teklif serbest bırakılır. Yayımlama işlemini tamamladıktan sonra, IoT Edge modülü teklifiniz [Microsoft Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules)listelenecektir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Marketi 'nde mevcut bir IoT Edge modülü teklifini güncelleştirme](./cpp-update-existing-offer.md)
