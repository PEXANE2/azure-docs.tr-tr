---
title: Azure CDN ile Çin içerik teslimi | Microsoft Dokümanlar
description: İçeriği Çin kullanıcılarına sunmak için Azure İçerik Dağıtım Ağı'nı (CDN) kullanma hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: fa144c7ebd68e6f5dd192fca83dc6f306d7b8d63
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81254113"
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



