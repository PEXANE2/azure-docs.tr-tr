---
title: BLEU puanı nedir? -Özel çevirici
titleSuffix: Azure Cognitive Services
description: BLEU, makine çevirisi ile aynı kaynak cümlenin insan tarafından oluşturulan başvuru çevirileri arasındaki farkların ölçümüdür.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 08/17/2020
ms.author: swmachan
ms.openlocfilehash: 61c1efb7337bee5fe329c9d548e23f3931e6ce3c
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88510785"
---
# <a name="what-is-a-bleu-score"></a>BLEU puanı nedir?

[Bleu (iki noktalı değerlendirme çalışması)](https://en.wikipedia.org/wiki/BLEU) , bir otomatik çeviri ve aynı kaynak cümlenin bir veya daha fazla insan tarafından oluşturulan başvuru çevirisi arasındaki farkların ölçümüdür.

## <a name="scoring-process"></a>Puanlama işlemi

BLEU algoritması, otomatik çevirinin ardışık tümceciklerini başvuru çevirisi içinde bulduğu ardışık tümceciklerle karşılaştırır ve eşleşmelerin sayısını ağırlıklı bir biçimde sayar. Bu eşleşmeler konumdan bağımsızdır. Daha yüksek bir eşleşme derecesi, başvuru çevirisi ve daha yüksek puan ile daha yüksek bir benzerlik derecesini gösterir. Intelligibility ve dilbilgisi doğruluğu dikkate alınmaz.

## <a name="how-bleu-works"></a>BLEU nasıl çalışıyor?

BLEU 'nın kuvveti, her tümce için uygun insan kararından bağımsız olarak her tümce için bir test etcesini değerlendirerek bir test Corp, her tümce için bir değerlendirme hatası duymuştur.

BLEU puanlarını daha kapsamlı bir tartışmada [bulabilirsiniz](https://youtu.be/-UqDljMymMg).

BLEU sonuçları, etki alanınızda yer alan, eğitim ve ayarlama verileriyle test verilerinin tutarlılığı ve eğitene kadar veri harcandığına bağlıdır. Modelleriniz dar bir etki alanı üzerinde eğitildi ve eğitim verileriniz test verileriyle tutarlıdır, yüksek bir BLEU puanı bekleyebilir.

>[!NOTE]
>BLEU skorları arasında bir karşılaştırma yalnızca BLEU sonuçları aynı test kümesiyle, aynı dil çiftinden ve aynı MT altyapısından karşılaştırıldığı zaman yeniden oluşturulabilir. Farklı bir test kümesinden bir BLEU puanı farklı olacak şekilde bağlanır.
