---
title: Azure Marketi'nde sanal makine teklifi yayınlama
description: Varolan bir sanal makine teklifini Azure Marketi'nde yayımlamak için gereken adımları listeler.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: dsindona
ms.openlocfilehash: c35f721131b997dcfdb0f23a91a39329168b757c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80277353"
---
# <a name="publish-a-virtual-machine-offer"></a>Sanal makine teklifi yayınlama

 Teklifi portalda tanımladıktan ve ilişkili teknik varlıkları oluşturduktan sonra son adım, teklifi yayımlamak için göndermektir. Aşağıdaki diyagram, yayımlama işleminde "canlı yayına geçmek" için gereken ana adımları görüntüler:

![Sanal makine teklifi için yayımlama adımları](./media/publishvm_013.png)

Aşağıdaki tabloda bu adımlar açıklanır ve tamamlanması için maksimum zaman tahmini sağlar:
<!-- we need to tell them that if an offer seems stuck in a step, to know that they should file a support ticket (link to support ticket doc) -->


|  **Yayımlama Adımı**           | **Zaman**    | **Açıklama**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Ön koşulları doğrula         | 15 dk   | Teklif bilgileri ve teklif ayarları doğrulanır.                        |
| Test Sürüşü Doğrulama (isteğe bağlı) | 2 saat | Test Sürüşünü etkinleştirmek için seçtiyseniz, Microsoft Test Sürücüsü yapılandırmasını, dağıtımını ve çoğaltmaişlemini seçili bölgeler arasında doğrular. |
| Sertifika                  | 3 gün | Teklif, Azure Sertifika Ekibi tarafından analiz edilir. Bu adım, virüsler, kötü amaçlı yazılımlar, güvenlik uyumluluğu ve güvenlik sorunları için taramalar gerçekleştirir. Bir sorun bulunursa geri bildirim sağlanır. |
| Sağlama                   | 4 gün   | VM teklifi pazar üretim sistemlerinde çoğaltılır.               |
| Ambalaj ve kurşun üretimi kaydı | \<1 saat  | Teklifin teknik varlıkları müşteri kullanımı için paketlenir ve müşteri adayı sistemleri yapılandırılır ve ayarlanır. |
|  Yayıncı oturum kapatma             |  -        | Teklif yayına girmeden önce son yayıncı incelemesi ve onayı. Teklifinizi, tüm gereksinimlerinizi karşıladığını doğrulamak için seçili aboneliklerde (teklif bilgileri adımlarında) dağıtabilirsiniz.  |
| Sağlama                   | 4 gün | Kesinleşmiş VM teklifi pazar üretim sistemlerinde ve bölgelerde çoğaltılır. | 
| Canlı                           | 4 gün | VM teklifi serbest bırakılır, gerekli bölgelere çoğaltılır ve halka açık hale getirilir. |
|  |  |

Bu işlemin tamamlanması için 16 güne kadar bekleyin.  Bu yayımlama adımlarını inceledikten sonra, VM teklifiniz [Microsoft Azure Marketi'nde](https://azuremarketplace.microsoft.com/marketplace/)listelenir. 

