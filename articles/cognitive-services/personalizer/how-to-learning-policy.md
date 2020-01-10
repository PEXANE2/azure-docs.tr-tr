---
title: Öğrenme ilkesini yönetme-kişiselleştirici
description: Bu makale, kişiselleştirici hakkında sık sorulan sorun giderme sorularına yanıtlar içerir.
ms.topic: conceptual
ms.date: 01/08/2019
ms.openlocfilehash: 707ba9fe7b73ef74e21321533ba02b11f2bad850
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75843487"
---
# <a name="manage-learning-policy"></a>Öğrenme ilkesini Yönet

Öğrenme ilkesi ayarları, model eğitiminin _hiper parametrelerini_ belirlenir. Öğrenme ilkesi bir `.json` dosyasında tanımlanmıştır.

## <a name="import-a-new-learning-policy"></a>Yeni bir öğrenme ilkesini içeri aktar

1. [Azure Portal](https://portal.azure.com)açın ve kişiselleştirici kaynağı ' nı seçin.
1. **Kaynak yönetimi** bölümünde **model ve öğrenme ayarları** ' nı seçin.
1. **Içeri aktarma öğrenimi ayarları** için yukarıda belirtilen JSON biçimiyle oluşturduğunuz dosyayı seçin, ardından **karşıya yükle** düğmesini seçin.

    Öğrenme ilkesinin başarıyla karşıya yüklendiğini bildirimin bekleyin.

## <a name="export-a-learning-policy"></a>Bir öğrenme ilkesini dışarı aktarma

1. [Azure Portal](https://portal.azure.com)açın ve kişiselleştirici kaynağı ' nı seçin.
1. **Kaynak yönetimi** bölümünde **model ve öğrenme ayarları** ' nı seçin.
1. **Içeri aktarma öğrenimi ayarları** için **öğrenme ayarlarını dışarı aktar** düğmesini seçin. Bu, `json` dosyasını yerel bilgisayarınıza kaydeder.

## <a name="next-steps"></a>Sonraki adımlar

Öğrenme ilkesi [kavramları](concept-active-learning.md#learning-settings) hakkında bilgi edinin

[Bölge kullanılabilirliği hakkında bilgi edinin](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)