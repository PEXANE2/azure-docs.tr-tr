---
title: Azure CDN ile Çin içerik teslimi | Microsoft Dokümanlar
description: İçeriği Çin kullanıcılarına sunmak için Azure İçerik Dağıtım Ağı'nı (CDN) kullanma hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: a63914116f5ef9922ac05745764e0ad505dc3c91
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67593960"
---
# <a name="china-content-delivery-with-azure-cdn"></a>Azure CDN ile Çin içerik teslimi

Azure İçerik Teslim Ağı (CDN) global, Çin'e yakın bir yerde bulunma noktası (POP) konumları veya Çin'den gelen isteklere en iyi performansı sağlayan herhangi bir POP ile Çin kullanıcılarına içerik sunabilir. Ancak, Çin müşterileriniz için önemli bir pazarsa ve hızlı performansa ihtiyaçları varsa, bunun yerine Azure CDN China'yı kullanmayı düşünün.

Azure CDN China, bir dizi yerel sağlayıcıyla ortaklık yaparak Çin'deki AP'lerden içerik sunduğu için Azure CDN global'den farklıdır. Çin uyumluluğu ve düzenlemesi nedeniyle, Azure CDN China'yı kullanmak için ayrı bir abonelik kaydetmeniz gerekir ve web sitelerinizin ICP lisansına sahip olması gerekir. İçerik teslimini etkinleştirmek ve yönetmek için portal ve API deneyimi, Azure CDN global ve Azure CDN China arasında aynıdır.

## <a name="comparison-of-azure-cdn-global-and-azure-cdn-china"></a>Azure CDN global ve Azure CDN China karşılaştırması

Azure CDN global ve Azure CDN China aşağıdaki özelliklere sahiptir:

- Azure CDN genel:

     - Portal:https://portal.azure.com  

     - Çin dışında içerik dağıtımı gerçekleştirir

     - Dört fiyatlandırma katmanı: Microsoft standardı, Verizon standardı, Verizon premium ve Akamai standardı

     - [Belgeler](https://docs.microsoft.com/azure/cdn/)

- Azure CDN Çin:

     - Portal:https://portal.azure.cn

     - Çin içinde içerik dağıtımı gerçekleştirir

     - İki fiyatlandırma katmanı: Standart ve premium

     - [Belgeler](https://docs.azure.cn/en-us/cdn/)
 

## <a name="next-steps"></a>Sonraki adımlar

Azure CDN China hakkında daha fazla bilgi edinmek için bkz:

- [İçerik Dağıtım Ağı özellikleri](https://www.azure.cn/en-us/home/features/cdn/)

- [Azure İçerik Dağıtım Ağına Genel Bakış](https://docs.azure.cn/en-us/cdn/cdn-overview)

- [Azure İçerik Dağıtım Ağını Kullanma](https://docs.azure.cn/en-us/cdn/cdn-how-to-use)

- [Çin'de Azure hizmet kullanılabilirliği](https://docs.microsoft.com/azure/china/concepts-service-availability)



