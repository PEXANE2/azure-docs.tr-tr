---
title: Azure SYNAPSE bağlantı ayarları
description: Azure SYNAPSE Analytics 'te bağlantı ayarlarını yapılandırmanızı öğretir bir makale
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: security
ms.date: 03/15/2021
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: ce1a4808833cbd897da17f9ad75af346538d23d1
ms.sourcegitcommit: 66ce33826d77416dc2e4ba5447eeb387705a6ae5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2021
ms.locfileid: "103473096"
---
# <a name="azure-synapse-analytics-connectivity-settings"></a>Azure SYNAPSE Analytics bağlantı ayarları

Bu makale, Azure SYNAPSE Analytics 'teki bağlantı ayarlarını ve bunların uygun olduğu yerlerde nasıl yapılandırılacağını açıklar.


## <a name="connection-policy"></a>Bağlantı ilkesi
Azure SYNAPSE Analytics 'te SYNAPSE SQL bağlantı ilkesi *varsayılan* olarak ayarlanır. Bunu Azure SYNAPSE Analytics 'te değiştiremezsiniz. Bunun Azure [SYNAPSE Analytics 'Te](https://docs.microsoft.com/azure/azure-sql/database/connectivity-architecture#connection-policy)SYNAPSE SQL bağlantılarını nasıl etkilediği hakkında daha fazla bilgi edinebilirsiniz. 

## <a name="minimal-tls-version"></a>En düşük TLS sürümü
Azure SYNAPSE Analytics 'te SYNAPSE SQL, tüm TLS sürümlerini kullanarak bağlantılara izin verir. Azure SYNAPSE Analytics 'te SYNAPSE SQL için en düşük TLS sürümünü ayarlayamazsınız.

## <a name="next-steps"></a>Sonraki adımlar

[Azure SYNAPSE çalışma alanı](./synapse-workspace-ip-firewall.md) oluşturma
