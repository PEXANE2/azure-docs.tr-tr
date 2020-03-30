---
title: İnternet ekibe bakma - SSS
titleSuffix: Azure
description: İnternet ekibe bakma - SSS
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: reference
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 9b0b2b08e01c99fc918c4bc5649197c9caa4978a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75775504"
---
# <a name="internet-peering---faqs"></a>İnternet ekibe bakma - SSS

Genel sorular için aşağıdaki bilgileri inceleyebilirsiniz.

**Internet peering ve Peering Service arasındaki fark nedir?**

Peering Service, kurumsal müşterileri için Microsoft'a kurumsal sınıf genel IP bağlantısı sağlamayı amaçlayan bir hizmettir. Kurumsal sınıf Internet, Microsoft'a yüksek iş verimi bağlantısı na sahip ISS'ler aracılığıyla bağlantı ve HA bağlantısı için artıklık içerir. Ayrıca, kullanıcı trafiği en yakın Microsoft Edge'in gecikmesi için en iyi duruma getirilmiştir. Peering Service, iş ortağı taşıyıcısıyla karşılabağlantı yada bir bağlantı oluşturur. Ortakla karşıt bağlantı Exchange'in aksine Doğrudan bakan olmalıdır. Doğrudan bakma yerel ve coğrafi artıklık olmalıdır.

**Miras alabakan nedir?**

Azure PowerShell kullanılarak kurulan peering bağlantısı, Azure kaynağı olarak yönetilir. Geçmişte ayarlanan bakan bağlantıları sistemimizde, Azure kaynağı olarak yönetmek için dönüştürmeyi seçebileceğiniz eski bir bakış olarak depolanır.

**New-AzPeeringDirectConnectionObject çağrıldığında, Microsoft ve Peer aygıtlarına hangi IP adresleri verilir?**

New-AzPeeringDirectConnectionObject cmdlet'i ararken bir /31 adresi (a.b.c.d/31) veya /30 adresi (a.b.c.d/30) girilir. İlk IP adresi (a.b.c.d+0) Peer'in cihazına, ikinci IP adresi (a.b.c.d.d+1) Microsoft cihazına verilir.

**Yeni-AzPeeringDirectConnectionObject cmdlet MaxPrefixesAdvertisedIPv4 ve MaxPrefixesAdvertisedIPv6 parametreleri nedir?**

MaxPrefixesAdvertisedIPv4 ve MaxPrefixesAdvertisedIPv6 parametreleri, bir Eşin Microsoft'un kabul etmesini istediği maksimum IPv4 ve IPv6 önekleri sayısını temsil eder. Bu parametreler her zaman değiştirilebilir.