---
title: Azure Marketi 'nde sanal makine teklifi oluşturma
description: Azure Marketi için yeni bir sanal makine (VM) teklifi oluşturmak için gereken adımları listeler.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: dsindona
ms.openlocfilehash: a25f6877f1fb4940fb1de127b81d83975c8e835c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82142718"
---
# <a name="create-virtual-machine-offer"></a>Sanal Makine teklifi oluştur

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure sanal makine tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure sanal makine oluşturma teklifi](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer) ' nde bulunan yönergeleri izleyin.

Bu bölümde, Azure Marketi için yeni bir sanal makine (VM) teklif isteği oluşturmak için gereken adımlar listelenmektedir.  Her teklif Azure Marketi 'nde kendi varlığı olarak görünür ve bir veya daha fazla SKU ile ilişkilendirilir.  Bir VM teklifi, aşağıdaki varlık ve destekleyici hizmetler gruplarından oluşur: 

![Bir VM teklifi için varlıklar](./media/publishvm_002.png)

burada:

|  **Varlık grubu**   |  **Açıklama**  |
|  ---------------   |  ---------------  |
|    SKU'lar            |  Bir teklifin en küçük satın alınabilir alınırken birimi. Tek bir teklif (ürün sınıfı) ile ilişkili birden fazla SKU, desteklenen özellikler, VM görüntü türleri ve faturalandırma modelleri arasında ayrım yapmak için kullanılabilir. |
|  Market       | Pazarlama, yasal ve lider yönetimi varlıkları ve belirtimleri içerir.  <ul><li> Pazarlama varlıkları, teklif adı, açıklama ve logo içerir</li> <li> Yasal varlıklar, bir gizlilik ilkesi, kullanım koşulları ve diğer yasal belgeleri içerir</li>  <li> Lider yönetimi ilkesi, Azure Marketi Son Kullanıcı portalından müşteri adaylarını nasıl işleyeceğinizi belirtmenizi sağlar.</li> </ul> |
| Destek            | Destek iletişim ve ilke bilgilerini içerir |
| Test Sürüşü         | Son kullanıcıların satın almadan önce teklifinizi test edebilmesi için varlık tanımlar |
|  |  |


## <a name="new-offer-form"></a>Yeni teklif formu

[Bulut iş ortağı portalı](https://cloudpartner.azure.com/)oturum açtıktan sonra sol menü çubuğunda **+ yeni teklif** öğesine tıklayın. Elde edilen menüde, **sanal makineler** ' e tıklayarak **yeni teklif** formunu GÖRÜNTÜLEYIN ve yeni bir VM teklifi için varlık tanımlama sürecini başlatın. 
<!-- not all publishers see corevm or azure apps test, you need to be whitelisted to see them. we should hide those in these images. -->

![Yeni sanal makine teklifi Kullanıcı arabirimi seçimi](./media/publishvm_003.png)

> [!WARNING]
> **Sanal makineler** seçeneği gösterilmezse veya etkinleştirilmemişse, hesabınız bu teklif türünü oluşturmak için izne sahip olmaz.  Lütfen bir Geliştirici hesabına kaydolma dahil olmak üzere bu teklif türü için tüm [önkoşulları](./cpp-prerequisites.md) karşıladığınızı kontrol edin.


## <a name="next-steps"></a>Sonraki adımlar

Bu bölümdeki sonraki konular, **yeni teklif** sayfasındaki (bir VM teklif türü için) sekmeleri yansıtır.  Her makalede, yeni VM teklifiniz için varlık gruplarını ve destekleyici hizmetleri tanımlamak üzere ilişkili sekmenin nasıl kullanılacağı açıklanmaktadır.

- [Teklif Ayarları sekmesi](./cpp-offer-settings-tab.md)
- [SKU'lar sekmesi](./cpp-skus-tab.md)
- [Test Sürüşü sekmesi](./cpp-test-drive-tab.md)
- [Market sekmesi](./cpp-marketplace-tab.md)
- [Destek sekmesi](./cpp-support-tab.md)
