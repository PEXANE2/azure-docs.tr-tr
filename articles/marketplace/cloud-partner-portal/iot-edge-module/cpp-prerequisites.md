---
title: Azure IoT Edge modülü ön koşulları | Azure Marketi
description: IoT Edge modülü yayınlamak için ön koşullar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: 93e0418004ea65f93ad436eec1ce523424a194ba
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80744973"
---
# <a name="iot-edge-module-publishing-prerequisites"></a>IoT Edge modülü yayımlama ön koşulları

>[!Important]
>30 Mart 2020'den itibaren, IoT Edge modül tekliflerinizi İş Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Bir IoT Edge modülü teklifi oluştur'daki](https://aka.ms/AzureCreateIoT) yönergeleri izleyin.

Bu makalede, bir IoT Edge modülü teklifi yayımlama nın ön koşulları açıklanmaktadır.  Bunu zaten yapmadıysanız, [IoT Edge modülleri yayımlama kılavuzunu gözden geçirin.](../..//iot-edge-module.md)


## <a name="publishing-prerequisites"></a>Ön koşullarıyayınlama

Azure Marketi'nde bir IoT Edge modülü yayınlamak için aşağıdaki ön koşulları karşılamanız gerekir:

<!-- P2: It would be great to point to the terms of use of CPP here. This can often be a blocker for big companies and these terms of use are not anonymously visible yet.-->
- [Bulut İş Ortağı Portalına](https://cloudpartner.azure.com/)Erişim . Daha fazla bilgi için [Azure Marketi ve AppSource yayımlama kılavuzuna](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide)bakın.
- [Azure MarketI Koşullarına](https://azure.microsoft.com/support/legal/marketplace-terms/) Uyum
- IoT Edge modülü teknik varlığınızı bir Azure Konteyner Kayıt Defteri'nde barındırın.  Daha fazla bilgi için [IoT Edge modülü teknik varlığınızı nasıl hazırlayacağınızı](./cpp-create-technical-assets.md) öğrenin
- IoT Edge modülü meta verilerinizi kullanıma hazır layın. Örneğin, aşağıdaki varlıkları hazırlayın:
    - Bir başlık
    - Açıklama (HTML biçiminde)
    - Logo görüntüsü (PNG formatı ve 40x40px, 90x90px, 115x115px, 255x115px dahil sabit görüntü boyutları)
    - Kullanım süresi ve gizlilik politikası
    - Rotalar, ikiz istenen özellikler, createOptions ve çevre değişkenleri içeren varsayılan modül yapılandırması.
    - Modül dokümantasyonu
    - Destek kişileri


## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge modülü teknik varlığınızı hazırladıktan](./cpp-create-technical-assets.md) [sonra, IoT Edge modül teklifinizi oluşturmaya](./cpp-create-offer.md)hazır olacaksınız. 
