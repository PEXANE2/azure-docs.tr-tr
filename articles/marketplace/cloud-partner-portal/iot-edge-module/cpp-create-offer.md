---
title: Azure IoT Edge modülü teklifi oluşturma | Azure Marketi
description: Nasıl Pazar için yeni bir IoT Edge Modülü yayınlamak için.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: 112c2ba831ab27faaca755079e9d20e2aebfcf9f
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80744404"
---
# <a name="create-a-new-iot-edge-module-offer-with-the-cloud-partner-portal"></a>Bulut İş Ortağı Portalı ile yeni bir IoT Edge modülü teklifi oluşturun

>[!Important]
>30 Mart 2020'den itibaren, IoT Edge modül tekliflerinizi İş Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Bir IoT Edge modülü teklifi oluştur'daki](https://aka.ms/AzureCreateIoT) yönergeleri izleyin.

Bu makalede, Azure Marketi için bir IoT Edge modülü teklif girişinin nasıl oluşturulup yayımlandırılabildiğini açıklanmaktadır. Her teklif Azure Marketi'nde kendi varlığı olarak görünür ve bir veya daha fazla SUK ile ilişkilidir.  IoT Edge modül teklifi, varlıkların ve destekleyici hizmetlerin aşağıdaki gruplandırmalarından oluşur:

|  **Varlık grubu**   |  **Açıklama**  |
|  ---------------   |  ---------------  |
|    SKU'lar            |  Bir teklifin en küçük dağıtılabilir birimi. Tek bir teklifte (ürün sınıfı) teklifle ilişkili birden çok SNU olabilir. Desteklenen özellikler ve faturalandırma modelleri arasında ayrım yapmak için SUS'ları kullanabilirsiniz. |
|  Market       | Pazarlama, yasal ve müşteri adayı yönetim varlıkları ve teknik özellikleri içerir.  <ul><li> Pazarlama varlıkları teklif adı, açıklama ve logolar içerir</li> <li> Yasal varlıklar arasında gizlilik politikası, kullanım koşulları ve diğer yasal belgeler yer almaktadır.</li>  <li> Müşteri adayı yönetimi ilkesi, Azure Marketi son kullanıcı portalından müşteri adaylarını nasıl işleyeceğinibelirtmenize olanak tanır.</li> </ul> |
| Destek            | Destek iletişimi ve ilke bilgilerini içerir |


## <a name="new-offer-form"></a>Yeni Teklif formu 

Cloud Partner [Portal'da](https://cloudpartner.azure.com/)oturum açın ve ardından sol menü çubuğunda **+ Yeni Teklif'i** seçin. Yeni Teklif menüsünde, **Yeni Teklif** formunu görüntülemek ve yeni bir ioT Edge Modülü teklifi için varlıkları tanımlama işlemini başlatmak için **IoT Edge Modüllerini** seçin. 

![Yeni IoT Edge modülü kullanıcı arabirimi seçimi sunar](./media/new-iot-edge-module-offer.png)

## <a name="next-steps"></a>Sonraki adımlar

IoT Edge modülü teklif türü için **Yeni Teklif** sayfası, yeni bir teklif oluşturmak için kullanacağınız bir dizi sekme ve form alanı sağlar. Aşağıdaki makalelerin her biri, yeni IoT Edge modülü teklifiniz için varlık gruplarını ve destekleyici hizmetleri tanımlamak için sekmeyi nasıl kullanacağınızı açıklar.

- [Teklif Ayarları sekmesi](./cpp-offer-settings-tab.md)
- [SKU'lar sekmesi](./cpp-skus-tab.md)
- [Market sekmesi](./cpp-marketplace-tab.md)
- [Destek sekmesi](./cpp-support-tab.md)
