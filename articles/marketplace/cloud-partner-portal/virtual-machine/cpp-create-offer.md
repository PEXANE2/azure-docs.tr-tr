---
title: Azure Marketi'nde Sanal Makine teklifi oluşturun
description: Azure Marketi için yeni bir sanal makine (VM) teklifi oluşturmak için gereken adımları listeler.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: dsindona
ms.openlocfilehash: 9d06809df2774224b61fd3fb643ab628dd2890f6
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273996"
---
# <a name="create-virtual-machine-offer"></a>Sanal Makine teklifi oluşturun

> [!IMPORTANT]
> 13 Nisan 2020'den itibaren Azure Sanal Makine tekliflerinizin yönetimini İş Ortağı Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure Sanal Makine Oluştur teklifindeki](https://aka.ms/CreateAzureVMoffer) yönergeleri izleyin.

Bu bölümde, Azure Marketi için yeni bir sanal makine (VM) teklif isteği oluşturmak için gereken adımlar listelanmaktadır.  Her teklif Azure Marketi'nde kendi varlığı olarak görünür ve bir veya daha fazla SUK ile ilişkilidir.  VM teklifi, varlıkların ve destekleyici hizmetlerin aşağıdaki gruplandırmalarından oluşur: 

![VM teklifi için varlıklar](./media/publishvm_002.png)

burada:

|  **Varlık grubu**   |  **Açıklama**  |
|  ---------------   |  ---------------  |
|    SKU'lar            |  Bir teklifin satın alınabilir en küçük birimidir. Desteklenen özellikler, VM görüntü türleri ve faturalandırma modelleri arasında ayrım yapmak için tek bir teklifin (ürün sınıfı) ilişkili birden çok SBI'si olabilir. |
|  Market       | Pazarlama, yasal ve müşteri adayı yönetim varlıkları ve teknik özellikleri içerir.  <ul><li> Pazarlama varlıkları teklif adı, açıklama ve logolar içerir</li> <li> Yasal varlıklar arasında gizlilik politikası, kullanım koşulları ve diğer yasal belgeler yer almaktadır.</li>  <li> Müşteri adayı yönetimi ilkesi, Azure Marketi Son kullanıcı portalından müşteri adaylarını nasıl işleyeceğinibelirtmenize olanak tanır.</li> </ul> |
| Destek            | Destek iletişimi ve ilke bilgilerini içerir |
| Test Sürüşü         | Son kullanıcıların teklifinizi satın almadan önce test etmesini sağlayan varlıkları tanımlar |
|  |  |


## <a name="new-offer-form"></a>Yeni Teklif formu

[Bulut Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açtıktan sonra sol menü çubuğundaki **+ Yeni Teklif** öğesini tıklatın. Ortaya çıkan menüde, **Yeni Teklif** formunu görüntülemek ve yeni bir VM teklifi için varlıkları tanımlama işlemini başlatmak için **Sanal Makineler'e** tıklayın. 
<!-- not all publishers see corevm or azure apps test, you need to be whitelisted to see them. we should hide those in these images. -->

![Yeni sanal makine kullanıcı arayüzü seçimi teklif](./media/publishvm_003.png)

> [!WARNING]
> Sanal **Makineler** seçeneği gösterilmez veya etkinleştirilemiyorsa, hesabınızda bu teklif türünü oluşturma izni yoktur.  Lütfen bu teklif türü için geliştirici hesabına kaydolmak da dahil olmak üzere tüm [ön koşulları](./cpp-prerequisites.md) karşıladığınızdan kontrol edin.


## <a name="next-steps"></a>Sonraki adımlar

Bu bölümdeki sonraki konular **Yeni Teklif** sayfasındaki sekmeleri (VM teklif türü için) yansıtMıştır.  Her makalede, yeni VM teklifiniz için varlık gruplarını ve destekleyici hizmetleri tanımlamak için ilişkili sekmeyi nasıl kullanacağınızı açıklar.

- [Teklif Ayarları sekmesi](./cpp-offer-settings-tab.md)
- [SKU'lar sekmesi](./cpp-skus-tab.md)
- [Test Sürüşü sekmesi](./cpp-test-drive-tab.md)
- [Market sekmesi](./cpp-marketplace-tab.md)
- [Destek sekmesi](./cpp-support-tab.md)
