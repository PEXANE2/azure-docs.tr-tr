---
title: Azure IoT Edge modül teklifini yayımla | Azure Marketi
description: Nasıl bir IoT Edge modül teklifi yayımlamak için.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: 9d78ff53fb520dcfc80a812d53ae188e340722af
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80983203"
---
# <a name="publish-iot-edge-module-offer"></a>IoT Edge modülü teklifini yayımlama

>[!Important]
>13 Nisan 2020'den itibaren, IoT Edge modül tekliflerinizi İş Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Bir IoT Edge modülü teklifi oluştur'daki](https://aka.ms/AzureCreateIoT) yönergeleri izleyin.

 **Yeni Teklif** sayfasındaki bilgileri sağlayarak yeni bir teklif oluşturduktan sonra teklifi yayımlayabilirsiniz. Yayımlama işlemini başlatmak için **Yayımla'yı** seçin.

Aşağıdaki diyagram, "canlı yayına geçmek" için yayımlama işlemindeki ana adımları gösterir.

![IoT Edge modülü teklifi için yayımlama adımları](./media/iot-edge-module-publishing-steps.png)

## <a name="detailed-description-of-publishing-steps"></a>Yayımlama adımlarının ayrıntılı açıklaması

Aşağıdaki tablo, her adımı tamamlamak için bir zaman tahmini (maksimum) ile, her yayımlama adımı açıklar.
<!-- P2: we need to tell them that if an offer seems stuck in a step, to know that they should file a support ticket (link to support ticket doc) -->


|  **Yayımlama Adımı**           | **Zaman**    | **Açıklama**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Ön koşulları doğrula         | 15 dk   | Teklif bilgileri ve teklif ayarları doğrulanır.                        |
| Sertifika                  | 2 hafta | Teklif, Azure Sertifika Ekibi tarafından analiz edilir. Bu adım, virüsler, kötü amaçlı yazılımlar, güvenlik uyumluluğu ve güvenlik sorunları için taramalar gerçekleştirir. Ayrıca, bu IoT Edge modül teklifinin tüm uygunluk kriterlerini karşıladığını da doğrulayacaktır [(ön koşullara](./cpp-prerequisites.md) bakın ve [teknik varlıklarınızı hazırlayın).](./cpp-create-technical-assets.md) Bir sorun bulunursa geri bildirim sağlanır. |
| Paketleme | 1 saat  | Teklifin teknik varlıkları müşteri kullanımı için paketlenir ve müşteri adayı sistemleri yapılandırılır ve ayarlanır. |
|  Yayıncı oturum kapatma             |  -        | Teklif yayına girmeden önce son yayıncı incelemesi ve onayı. Teklifinizi, tüm gereksinimlerinizi karşıladığını doğrulamak için seçili aboneliklerde (teklif bilgileri adımlarında) dağıtabilirsiniz.  Teklifinizin bir sonraki adıma geçebilmeleri için **Canlı Git'i** seçin. |
| Paketleme                 | 1 saat | Kesinleşen teklif, pazar yeri üretim sistemleri nde ve bölgelerde çoğaltılır. | 
| Canlı                           | 4 gün |Teklif serbest bırakılır, gerekli bölgelere çoğaltılır ve halka açık hale getirilir. |

Yayımlama işleminin tamamlanması için 10 iş gününe kadar izin verin ve teklif serbest bırakılır. Yayımlama işlemini tamamladıktan sonra, IoT Edge modül teklifiniz [Microsoft Azure Marketi'nde](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules)listelenecektir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Marketi'nde varolan bir IoT Edge modül teklifini güncelleştirme](./cpp-update-existing-offer.md)
