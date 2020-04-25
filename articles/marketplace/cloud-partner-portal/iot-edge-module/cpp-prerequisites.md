---
title: Azure IoT Edge modül önkoşulları | Azure Marketi
description: IoT Edge modülünü yayımlama önkoşulları.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: d1bfc29a0cb031421b4017454bc8a2cdd8be09f9
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82142379"
---
# <a name="iot-edge-module-publishing-prerequisites"></a>IoT Edge modülü yayımlama önkoşulları

>[!Important]
>13 Nisan 2020 ' den itibaren, IoT Edge modülü tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilmiş tekliflerinizi yönetmek için [IoT Edge modülü oluşturma teklifi](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-iot-edge-module-creation) bölümündeki yönergeleri izleyin.

Bu makalede bir IoT Edge modülü teklifini yayımlama önkoşulları açıklanmaktadır.  Daha önce yapmadıysanız, [IoT Edge modülleri Yayımlama Kılavuzu](../..//iot-edge-module.md)' nu gözden geçirin.


## <a name="publishing-prerequisites"></a>Ön koşullar yayımlanıyor

Azure Marketi 'Nde bir IoT Edge modülünü yayımlamak için aşağıdaki önkoşulları karşılamanız gerekir:

<!-- P2: It would be great to point to the terms of use of CPP here. This can often be a blocker for big companies and these terms of use are not anonymously visible yet.-->
- [Bulut iş ortağı portalı](https://cloudpartner.azure.com/)erişim. Daha fazla bilgi için bkz. [Azure Marketi ve AppSource Yayımlama Kılavuzu](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- [Azure Marketi koşullarına](https://azure.microsoft.com/support/legal/marketplace-terms/) yönelik anlaşma
- IoT Edge modülü teknik varlığınızı bir Azure Container Registry barındırın.  Daha fazla bilgi için bkz. [IoT Edge modülü teknik varlığınızı hazırlama](./cpp-create-technical-assets.md)
- IoT Edge modülünüzün meta verilerini kullanıma hazırlayın. Örneğin, aşağıdaki varlıkları hazırlayın:
    - Bir başlık
    - Bir açıklama (HTML biçiminde)
    - Logo resmi (PNG biçimi ve 40x40px, 90x90px, 115x115px, 255x115px dahil)
    - Kullanım Koşulları ve Gizlilik ilkesi
    - Şunu içeren bir varsayılan modül yapılandırması: rotalar, ikizi istenen özellikler, createOptions ve ortam değişkenleri.
    - Modül belgeleri
    - Destek kişileri


## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge modülü teknik varlığınızı hazırladıktan](./cpp-create-technical-assets.md)sonra, [IoT Edge modül teklifinizi oluşturmaya](./cpp-create-offer.md)hazır olursunuz. 
