---
title: Azure Akış Analizi'nde çıktı hata ilkeleri
description: Azure Akış Analizi'nde kullanılabilen çıktı hatası işleme ilkeleri hakkında bilgi edinin.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 22112272bb302769e5969cf6995d486438deb41f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75431619"
---
# <a name="azure-stream-analytics-output-error-policy"></a>Azure Akış Analizi çıktı hata ilkesi
Bu makalede, Azure Akış Analizi'nde yapılandırılabilen çıktı veri hatası işleme ilkeleri açıklanmaktadır.

Çıktı veri hatası işleme ilkeleri yalnızca bir Akış Analizi işi tarafından üretilen çıkış olayı hedef lavabonun şemasına uymadığında oluşan veri dönüştürme hataları için geçerlidir. Bu ilkeyi **Yeniden Deneyin** veya **Bırak'ı**seçerek yapılandırabilirsiniz. Azure portalında, Akış Analizi işindeyken, **Yapılandırma**altında, seçiminizi yapmak için **Hata İlkesi'ni** seçin.

![Azure Akış Analizi çıktı hata ilkesi konumu](./media/stream-analytics-output-error-policy/stream-analytics-error-policy-locate.png)


## <a name="retry"></a>Yeniden Dene
Bir hata oluştuğunda, Azure Akış Analizi, yazma başarılı olana kadar olayı süresiz olarak yeniden çalışır. Yeniden denemeler için zaman arası yoktur. Sonunda, sonraki tüm olayların yeniden denemeye gelen olay tarafından işlenmesi engellenir. Bu seçenek varsayılan çıktı hata işleme ilkesidir.

## <a name="drop"></a>Bırak
Azure Stream Analytics, veri dönüştürme hatasıyla sonuçlanan tüm çıkış olaylarını bırakır. Bırakılan olaylar daha sonra yeniden işleme için kurtarılamaz.


Tüm geçici hatalar (örneğin, ağ hataları) çıktı hata işleme ilkesi yapılandırması ne olursa olsun yeniden denener.


## <a name="next-steps"></a>Sonraki adımlar
[Azure Akış Analizi için sorun giderme kılavuzu](stream-analytics-troubleshooting-guide.md)
