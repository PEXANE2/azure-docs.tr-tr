---
title: Azure CDN kaynaklarının sistem durumunu izleme | Microsoft Docs
description: Azure Kaynak Durumu kullanarak Azure CDN kaynaklarınızın sistem durumunu nasıl izleyeceğinizi öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67593448"
---
# <a name="monitor-the-health-of-azure-cdn-resources"></a>Azure CDN kaynaklarının sistem durumunu izleme
  
Azure CDN kaynak sistem durumu, [Azure Kaynak sistem](../resource-health/resource-health-overview.md)durumunun bir alt kümesidir.  Azure Kaynak durumu ' nu kullanarak CDN kaynaklarının sistem durumunu izleyebilir ve sorunları gidermek için eyleme dönüştürülebilir rehberlik alabilirsiniz.

>[!IMPORTANT] 
>Kaynak durumu Azure CDN yalnızca genel CDN tesliminin ve API yeteneklerinin sistem durumu için olan hesaplardır.  Azure CDN kaynak durumu, bireysel CDN uç noktalarını doğrulamaz.
>
>Akış Azure CDN kaynak sistem durumu, en fazla 15 dakika gecikiyor olabilir.

## <a name="how-to-find-azure-cdn-resource-health"></a>Azure CDN kaynak durumunu bulma

1. [Azure Portal](https://portal.azure.com), CDN profilinize gidin.

2. **Ayarlar** düğmesine tıklayın.

    ![Ayarlar düğmesi](./media/cdn-resource-health/cdn-profile-settings.png)

3. *Destek + sorun giderme*altında **kaynak sistem durumu**' na tıklayın.

    ![CDN kaynak durumu](./media/cdn-resource-health/cdn-resource-health3.png)

>[!TIP] 
>Ayrıca, *Yardım + Destek* dikey penceresinde *kaynak sistem durumu* kutucuğunda listelenen CDN kaynaklarını bulabilirsiniz.  Daire içine tıklayarak yardım ve *destek* 'e hızlıca ulaşabilirsiniz miyim **?** , portalın sağ üst köşesinde.
>
> ![Yardım + destek](./media/cdn-resource-health/cdn-help-support.png)

## <a name="azure-cdn-specific-messages"></a>Azure CDN özel iletiler

Azure CDN kaynak durumu ile ilgili durumlar aşağıda bulunabilir.

|İleti | Önerilen Eylem |
|---|---|
|Bir veya daha çok CDN uç noktanızı durdurmuş, kaldırmış veya yanlış yapılandırmış olabilirsiniz | Bir veya daha çok CDN uç noktanızı durdurmuş, kaldırmış veya yanlış yapılandırmış olabilirsiniz.|
|Üzgünüz, CDN yönetim hizmeti şu anda kullanılamıyor | Durum güncelleştirmeleri için burayı tekrar işaretleyin; Sorununuz beklenen çözüm zamanından sonra devam ederse desteğe başvurun.|
|CDN uç noktalarınız bazı CDN sağlayıcılarımızla ilgili devam eden sorunlardan etkileniyor olabilir | Durum güncelleştirmeleri için burayı tekrar işaretleyin; Kaynak ve CDN uç noktanızı test etme hakkında bilgi edinmek için sorun giderme aracını kullanın; Sorununuz beklenen çözüm zamanından sonra devam ederse desteğe başvurun. |
|Üzgünüz, CDN uç noktası yapılandırma değişikliklerinde yayma gecikmeleri yaşanıyor | Durum güncelleştirmeleri için burayı tekrar işaretleyin; Yapılandırma değişiklikleriniz beklenen sürede tamamen yayıldıysa desteğe başvurun.|
|Üzgünüz, ek portalı yüklerken sorun yaşıyoruz | Durum güncelleştirmeleri için burayı tekrar işaretleyin; Sorununuz beklenen çözüm zamanından sonra devam ederse desteğe başvurun.|
Üzgünüz, bazı CDN sağlayıcılarımızla ilgili sorun yaşıyoruz | Durum güncelleştirmeleri için burayı tekrar işaretleyin; Sorununuz beklenen çözüm zamanından sonra devam ederse desteğe başvurun. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Kaynak durumu 'na genel bakış konusunu okuyun](../resource-health/resource-health-overview.md)
- [CDN sıkıştırması sorunlarını giderme](./cdn-troubleshoot-compression.md)
- [404 hatalarıyla ilgili sorunları giderme](./cdn-troubleshoot-endpoint.md)