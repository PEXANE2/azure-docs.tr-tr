---
title: Azure CDN kaynaklarının durumunu izleyin| Microsoft Dokümanlar
description: Azure Kaynak Durumu'nu kullanarak Azure CDN kaynaklarınızın durumunu nasıl izleyeceğinizi öğrenin.
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: ''
ms.assetid: bf23bd89-35b2-4aca-ac7f-68ee02953f31
ms.service: azure-cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 6710f5e5b873f751ad21068acdc15d38574f8378
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67593448"
---
# <a name="monitor-the-health-of-azure-cdn-resources"></a>Azure CDN kaynaklarının durumunu izleme
  
Azure CDN Kaynak durumu, [Azure kaynak durumunun](../resource-health/resource-health-overview.md)bir alt kümesidir.  CDN kaynaklarının sistem durumunu izlemek ve sorunları gidermek için işlem yapılabilir kılavuzlar almak için Azure kaynak durumunu kullanabilirsiniz.

>[!IMPORTANT] 
>Azure CDN kaynak durumu yalnızca şu anda genel CDN tesliminin ve API yeteneklerinin durumunu açıklar.  Azure CDN kaynak durumu tek tek CDN uç noktalarını doğrulamaz.
>
>Azure CDN kaynak durumunu besleyen sinyaller 15 dakikaya kadar gecikebilir.

## <a name="how-to-find-azure-cdn-resource-health"></a>Azure CDN kaynak durumunu bulma

1. Azure [portalında](https://portal.azure.com)CDN profilinize göz atın.

2. **Ayarlar** düğmesini tıklatın.

    ![Ayarlar düğmesi](./media/cdn-resource-health/cdn-profile-settings.png)

3. *Destek + sorun giderme*altında, Kaynak **durumu'na**tıklayın.

    ![CDN kaynak durumu](./media/cdn-resource-health/cdn-resource-health3.png)

>[!TIP] 
>*Kaynak sistem durumu* döşemesinde listelenen CDN kaynaklarını *Yardım + destek* bıçağında da bulabilirsiniz.  Daire içine tıklayarak Hızlı bir şekilde *Yardım + destek* alabilirsiniz? **?** portalın sağ üst köşesinde.
>
> ![Yardım + destek](./media/cdn-resource-health/cdn-help-support.png)

## <a name="azure-cdn-specific-messages"></a>Azure CDN'ye özgü iletiler

Azure CDN kaynak durumuyla ilgili durumları aşağıda bulabilirsiniz.

|İleti | Önerilen Eylem |
|---|---|
|Bir veya daha çok CDN uç noktanızı durdurmuş, kaldırmış veya yanlış yapılandırmış olabilirsiniz | Bir veya daha çok CDN uç noktanızı durdurmuş, kaldırmış veya yanlış yapılandırmış olabilirsiniz.|
|Üzgünüz, CDN yönetim hizmeti şu anda kullanılamıyor | Durum güncellemeleri için buraya bakın; Sorunuzun beklenen çözüm süresinden sonra devam ederse, desteğe başvurun.|
|CDN uç noktalarınız bazı CDN sağlayıcılarımızla ilgili devam eden sorunlardan etkileniyor olabilir | Durum güncellemeleri için buraya bakın; Başlangıç ve CDN bitiş noktanızı nasıl test edebilirsiniz öğrenmek için Sorun Giderme aracını kullanın; Sorunuzun beklenen çözüm süresinden sonra devam ederse, desteğe başvurun. |
|Üzgünüz, CDN uç noktası yapılandırma değişikliklerinde yayma gecikmeleri yaşanıyor | Durum güncellemeleri için buraya bakın; Yapılandırma değişiklikleriniz beklenen süre içinde tam olarak yayılılılmıyorsa, desteğe iletin.|
|Üzgünüz, ek portalı yüklerken sorun yaşıyoruz | Durum güncellemeleri için buraya bakın; Sorunuzun beklenen çözüm süresinden sonra devam ederse, desteğe başvurun.|
Üzgünüz, bazı CDN sağlayıcılarımızla ilgili sorun yaşıyoruz | Durum güncellemeleri için buraya bakın; Sorunuzun beklenen çözüm süresinden sonra devam ederse, desteğe başvurun. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynak durumu yla ilgili genel bir bakış okuma](../resource-health/resource-health-overview.md)
- [CDN sıkıştırma ile sorun giderme sorunları](./cdn-troubleshoot-compression.md)
- [404 hatayla ilgili sorun giderme sorunları](./cdn-troubleshoot-endpoint.md)