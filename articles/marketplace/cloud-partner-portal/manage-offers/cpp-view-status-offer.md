---
title: Pazar tekliflerinin durumunu görüntüleyin | Azure Marketi
description: Bulut İş Ortağı Portalını kullanarak Azure ve AppSource Pazar Yerlerinde tekliflerin durumunu görüntüleyin
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: dsindona
ms.openlocfilehash: 0cbe6a45ba205f32a764bdadb021dc0dcf5bf0cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275977"
---
# <a name="view-the-publishing-status-of-azure-marketplace-and-appsource-offers"></a>Azure Marketi ve AppSource tekliflerinin yayın durumunu görüntüleyin

Bir teklif oluşturduktan sonra ve özellikle yayımlama işlemi sırasında, teklifinizin durumunu Bulut İş Ortağı Portalı'nda görüntüleyebilirsiniz.  Genel yayımlama durumu, portalın [**Tüm Teklifler**](../portal-tour/cpp-all-offers-page.md) ve [**Onaylar**](../portal-tour/cpp-approvals-page.md) sayfalarında mevcuttur.  Her teklif için aşağıdaki durum göstergelerinden biri görüntülenmelidir.  

|            Durum              |   Açıklama                                                           |
|            ------              |   -----------                                                           |
| **-**                          | Teklif oluşturuldu, ancak yayımlama süreci başlamadı.            |
| **Devam eden yayımlama**        | Teklif, yayımlama sürecinin adımlarını gözden geçiriyor.   |
| **Yayımlama başarısız oldu**             | Microsoft tarafından doğrulama veya gözden geçirme sırasında kritik bir sorun keşfedildi. |
| **Yayımlama iptal edildi**           | Yayımcı teklif yayımlama işlemini iptal etti.  Bu durum, pazardaki mevcut bir teklifi delist değildir. | 
| **Yayımcının oturumu imzalamayı bekliyor** | Teklif Microsoft tarafından incelendi ve şimdi yayıncı tarafından son bir doğrulama bekliyor. |
| **Listeden çıkarıldı**                   | Pazarda daha önce yayınlanmış bir teklif kaldırıldı.      | 
|  |  |


## <a name="publishing-status-details"></a>Yayımlama durumu ayrıntıları 

Yayımlama sürecinden geçerken bir teklifteki durum hakkında daha fazla ayrıntı **Yeni Teklif** sayfasının **Durum** sekmesinde bulunur.  Bu sayfa, bu teklif türü için tüm yayımlama adımlarını listeler.  *Sayı ve belirli adımların genellikle teklif türleri arasında farklılık olduğunu unutmayın.*  Bu sayfa ayrıca, yayımlama işlemi devam etmeden önce yayımcıtarafından işlem yapılmasını gerektiren Microsoft doğrulama ve gözden geçirme adımları tarafından ortaya atılan bekleyen sorunları da gösterir.  Örneğin, aşağıdaki resim, yeni bir sanal makine teklifi için **Durum** sekmesini gösterir. 

![VM teklifi için durum sekmesi](./media/vm-offer-pub-steps1.png)

Bir danışmanlık hizmeti için sonraki örnek **Durum** sekmesi, müşteri adayı yönetimi ayarlarında bildirilen bir hatayı gösterir.  Danışmanlık hizmetleri için müşteri adayı yönetimi gerektiğinden, yayımlamanın devam edilemeden önce bu hatanın düzeltilmesi gerekir.

![Hata gösteren danışmanlık hizmeti için durum sekmesi](./media/consulting-service-error.png)

Azure uygulamasının son örnek durumu, kritik bir Microsoft gözden geçirme sorununu gösterir.  Azure DevOps öğesine bu inceleme sorunu hakkında ayrıntılı bilgi içeren sıcak bir bağlantı içerir.  Daha fazla bilgi için [bkz.](cpp-publish-offer.md)

![Azure uygulaması için inceleme sorununu gösteren durum sekmesi](../azure-applications/media/status-tab-ms-review.png)


## <a name="next-steps"></a>Sonraki adımlar

Bekleyen sorunları düzeltmek veya teklif ayarlarını güncelleştirmek için [bir teklifi güncelleştirmeniz](./cpp-update-offer.md)gerekir. 
