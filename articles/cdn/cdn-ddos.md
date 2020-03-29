---
title: Azure CDN DDoS Koruma özellikleri | Microsoft Dokümanlar
description: Microsoft'un Azure CDN'si ek ücret ödemeden DDoS Protection temeli tarafından korunmaktadır
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
ms.topic: article
ms.date: 03/22/2019
ms.author: magattus
ms.openlocfilehash: 9cd688de861015cc12d1f98ed71e5376e5f574db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67593880"
---
# <a name="azure-cdn-ddos-protection"></a>Azure CDN DDoS Koruması

İçerik dağıtım ağı, tasarım gereği DDoS koruması sağlar. Azure CDN, hacimsel saldırıları absorbe etme kapasitesine ek olarak, ek ücret ödemeden aşağıda belirtildiği gibi ek DDoS korumasına sahiptir.

## <a name="azure-cdn-from-microsoft"></a>Microsoft'tan Azure CDN

Microsoft'un Azure [CDN'si Azure Basic DDoS](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)tarafından korunmaktadır. Varsayılan olarak microsoft platformundan Azure CDN'ye entegre edilmiştir ve ek ücret ödemeden. Microsoft'un genel olarak dağıtılan ağındaki Azure CDN'nin tam ölçeği ve kapasitesi, her zaman çevrimiçi trafik izleme ve gerçek zamanlı azaltma yoluyla yaygın ağ katmanı saldırılarına karşı savunma sağlar. Temel DDoS koruması, CDN uç noktalarını hedefleyen en yaygın, sık rastlanan Katman 7 DNS Sorgu Sellerine ve Katman 3 ve 4 hacimsel saldırılara karşı da savunma yapar. Bu hizmet, Microsoft'un kurumsal ve tüketici hizmetlerini büyük ölçekli saldırılara karşı koruma da kanıtlanmış bir geçmişe sahiptir.

## <a name="azure-cdn-from-verizon"></a>Verizon'dan Azure CDN

Verizon'daki Azure CDN, Verzion'un tescilli DDoS azaltma platformu tarafından korunmaktadır. Varsayılan olarak Verizon'dan Azure CDN'ye entegre edilmiştir ve ek ücret ödemeden. CDN uç noktalarını hedefleyen en yaygın, sık rastlanan Katman 7 DNS Sorgu Sellerine ve Katman 3 ve 4 hacimsel saldırılara karşı temel koruma sağlar.

## <a name="azure-cdn-from-akamai"></a>Akamai'den Azure CDN

Akamai'nin Azure CDN'si Akamai'nin tescilli DDoS azaltma platformu tarafından korunmaktadır. Varsayılan olarak Akamai'den Azure CDN'ye entegre edilmiştir ve ek ücret ödemeden. CDN uç noktalarını hedefleyen en yaygın, sık rastlanan Katman 7 DNS Sorgu Sellerine ve Katman 3 ve 4 hacimsel saldırılara karşı temel koruma sağlar.

## <a name="next-steps"></a>Sonraki adımlar

[Azure DDoS](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)hakkında daha fazla bilgi edinin. 
