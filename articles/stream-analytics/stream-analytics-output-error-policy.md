---
title: Azure Stream analytics'te çıkış hatası ilkeleri
description: Azure Stream Analytics'te kullanılabilir ilkeleri işleme çıkış hatası hakkında bilgi edinin.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 22112272bb302769e5969cf6995d486438deb41f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75431619"
---
# <a name="azure-stream-analytics-output-error-policy"></a>Azure Stream Analytics çıkış hata İlkesi
Bu makalede, Azure Stream Analytics'te yapılandırılabilir ilkeleri işleme çıkış veri hatası açıklanır.

Çıkış verilerine ilişkin hata işleme çıkış olayı, bir Stream Analytics işi tarafından üretilen kullanırken oluşan veri dönüştürme hataları ilkeleri uygulamak için hedef havuz şemaya uymuyor. Bu ilke ya da seçerek yapılandırabileceğiniz **yeniden deneyin** veya **bırak**. Azure portalında bir Stream Analytics işinde altında çalışırken **yapılandırma**seçin **hata İlkesi** seçiminizi yapma.

![Azure Stream Analytics çıkış hata ilkesi konumu](./media/stream-analytics-output-error-policy/stream-analytics-error-policy-locate.png)


## <a name="retry"></a>Yeniden Dene
Ne zaman bir hata, yazma süresiz olarak başarılı olana kadar olayı yazmayı Azure Stream Analytics yeniden denemeler gerçekleşir. İçin yeniden deneme zaman aşımı yoktur. Sonunda tüm sonraki olayların işlenmesini deniyor olay tarafından engellenir. İlke işleme varsayılan çıkış hatası seçenektir.

## <a name="drop"></a>Kaldır
Azure Stream Analytics, veri dönüştürme hatasıyla sonuçlanan tüm çıkış olaylarını bırakır. Bırakılan olaylar daha sonra yeniden işleme için kurtarılamaz.


Çıkış hatası işleme ilkesi yapılandırması bağımsız olarak tüm geçici hatalar (örneğin, ağ hataları) denenir.


## <a name="next-steps"></a>Sonraki adımlar
[Azure Stream Analytics için sorun giderme kılavuzu](stream-analytics-troubleshooting-guide.md)
