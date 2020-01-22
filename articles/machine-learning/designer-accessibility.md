---
title: Tasarımcıda erişilebilirlik özelliklerini kullanma
titleSuffix: Azure Machine Learning
description: Tasarımcıda bulunan klavye kısayolları ve ekran okuyucusu erişilebilirlik özellikleri hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: peterlu
author: peterclu
ms.date: 01/09/2020
ms.openlocfilehash: 7d5c8047fea8e9081de08b582c537711714ea463
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76314241"
---
# <a name="use-a-keyboard-to-use-azure-machine-learning-designer"></a>Azure Machine Learning tasarımcısını kullanmak için klavye kullanma

Azure Machine Learning tasarımcısını kullanmak için klavye ve ekran okuyucu kullanmayı öğrenin. Azure portal her yerde çalışan klavye kısayollarının bir listesi için, bkz [. Azure Portal klavye kısayolları](../azure-portal/azure-portal-keyboard-shortcuts.md)

Bu iş akışı, [ekran okuyucusu](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator) ve [Jaw](https://www.freedomscientific.com/products/software/jaws/)ile test edilmiştir, ancak diğer standart ekran okuyucularıyla birlikte çalışmalıdır.

## <a name="navigate-the-pipeline-graph"></a>İşlem hattı grafiğinde gezinme

Ardışık düzen grafiği liste listesi olarak düzenlenir. Üst düzey modül listesi, işlem hattının tüm modülünü içerir. Modül listesindeki her öğe, tüm bağlantılarını açıklayan bir bağlantı listesi içerir. 

1. Modül listesinde, modülleri değiştirmek için ok tuşunu kullanın.
1. Hedef modülün bağlantı listesini açmak için Tab kullanın.
1. Modülün bağlantı bağlantı noktaları arasında geçiş yapmak için ok tuşunu kullanın.
1. Hedef modüle gitmek için "G" kullanın.

## <a name="edit-the-pipeline-graph"></a>Ardışık düzen grafiğini düzenleme

### <a name="add-a-module-to-the-graph"></a>Grafiğe modül ekleme

1. Odağı tuvalden modül ağacına geçirmek için CTRL + F6 tuşlarını kullanın.
1. Standart TreeView denetimini kullanarak modül ağacında istenen modülü bulun.

### <a name="edit-a-module"></a>Modül düzenleme

Bir modülü başka bir modüle bağlamak için:

1. Modül listesinde bir modülü hedeflerken CTRL + SHIFT + H tuşlarını kullanarak bağlantı yardımcısını açın.
1. Modülün bağlantı bağlantı noktalarını düzenleyin.

Modül özelliklerini ayarlamak için:

1. Modül özelliklerini açmak için bir modülü hedeflerken CTRL + SHIFT + E tuşlarını kullanın.
1. Modül özelliklerini düzenleyin.

## <a name="navigation-shortcuts"></a>Gezinti kısayolları

| Tuş vuruşu | Açıklama |
|-|-|
| Ctrl + F6 | Tuval ve modül ağacı arasında odağı değiştirme |
| Ctrl + F1   | Modül ağacındaki bir düğüme odaklanan bilgi kartını açın |
| Ctrl + Shift + H | Odak bir düğüm üzerinde olduğunda bağlantı yardımcısını açın |
| Ctrl + Shift + E | Odak bir düğüm üzerinde olduğunda modül özelliklerini aç |
| Ctrl + G | İşlem hattı çalıştırması başarısız olursa odağı ilk başarısız düğüme taşı |

## <a name="action-shortcuts"></a>Eylem kısayolları

Erişim anahtarıyla aşağıdaki kısayolları kullanın. Erişim anahtarları hakkında daha fazla bilgi için bkz. https://en.wikipedia.org/wiki/Access_key.

| Tuş vuruşu | Eylem |
|-|-|
| Erişim tuşu + R | Çalıştırın |
| Erişim tuşu + P | Yayımlama |
| Erişim tuşu + C | Kopyala |
| Erişim tuşu + D | Kurulum |
| Erişim tuşu + ı | Çıkarım ardışık düzeni oluştur/güncelleştir |
| Erişim tuşu + B | Batch çıkarım ardışık düzeni oluştur/güncelleştir |
| Erişim tuşu + K | "Çıkarım işlem hattı oluştur" açılan menüsünü aç |
| Erişim tuşu + U | "Güncelleştirme çıkarımı ardışık düzeni" açılan menüsünü açın |
| Erişim tuşu + a | Daha fazla (...) açılan listesini aç |

## <a name="next-steps"></a>Sonraki adımlar

- [Yüksek karşıtlığı açma veya tema değiştirme](../azure-portal/azure-portal-change-theme-high-contrast.md)
- [Microsoft 'ta erişilebilirlik ile ilgili araçlar](https://www.microsoft.com/accessibility)
