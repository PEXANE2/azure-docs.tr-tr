---
title: Azure Stream Analytics çıkış hatası ilkeleri
description: Azure Stream Analytics ' de bulunan çıkış hatası işleme ilkeleri hakkında bilgi edinin.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 22112272bb302769e5969cf6995d486438deb41f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75431619"
---
# <a name="azure-stream-analytics-output-error-policy"></a>Çıkış hatası ilkesini Azure Stream Analytics
Bu makalede, Azure Stream Analytics ' de yapılandırılabilen çıkış verileri hata işleme ilkeleri açıklanmaktadır.

Çıkış verileri hata işleme ilkeleri yalnızca bir Stream Analytics işi tarafından üretilen çıkış olayı hedef havuzun şemasına uygun olmadığında oluşan veri dönüştürme hataları için geçerlidir. **Yeniden dene** veya **bırak**seçeneğini belirleyerek bu ilkeyi yapılandırabilirsiniz. Azure portal, bir Stream Analytics işinde, **Yapılandır**altında, seçiminizi yapmak Için **hata ilkesi** ' ni seçin.

![Çıkış hatası ilke konumunu Azure Stream Analytics](./media/stream-analytics-output-error-policy/stream-analytics-error-policy-locate.png)


## <a name="retry"></a>Yeniden Dene
Bir hata oluştuğunda, yazma başarılı olana kadar olayın süresiz olarak yazılmasına yeniden denemeler Azure Stream Analytics. Yeniden denemeler için zaman aşımı yok. Sonuç olarak, sonraki tüm olayların yeniden denenecek olay tarafından işlenmesi engellenir. Bu seçenek, varsayılan çıkış hatası işleme ilkesidir.

## <a name="drop"></a>Açılan
Azure Stream Analytics, veri dönüştürme hatasıyla sonuçlanan tüm çıkış olaylarını bırakır. Bırakılan olaylar daha sonra yeniden işleme için kurtarılamaz.


Tüm geçici hatalar (örneğin, ağ hataları), ilke yapılandırmasından oluşan çıkış hatası işleme ne olursa olsun yeniden denenir.


## <a name="next-steps"></a>Sonraki adımlar
[Azure Stream Analytics için sorun giderme kılavuzu](stream-analytics-troubleshooting-guide.md)
