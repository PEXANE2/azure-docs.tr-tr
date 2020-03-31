---
title: Tasarımcıda erişilebilirlik özelliklerini kullanma (önizleme)
titleSuffix: Azure Machine Learning
description: Tasarımcıda bulunan klavye kısayolları ve ekran okuyucu erişilebilirlik özellikleri hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: peterlu
author: peterclu
ms.date: 01/09/2020
ms.openlocfilehash: 59199291589a81d0a0d96b7867078b8196be086f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77366201"
---
# <a name="use-a-keyboard-to-use-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcısını kullanmak için klavye kullanın (önizleme)

Azure Machine Learning tasarımcısını kullanmak için klavye ve ekran okuyucukullanmayı öğrenin. Azure portalında her yerde çalışan klavye kısayollarının listesi için Azure [portalındaki Klavye kısayolları'na](../azure-portal/azure-portal-keyboard-shortcuts.md) bakın

Bu iş akışı [Ekran Okuyucusu](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator) ve [JAWS](https://www.freedomscientific.com/products/software/jaws/)ile test edilmiştir, ancak diğer standart ekran okuyucuları ile çalışması gerekir.

## <a name="navigate-the-pipeline-graph"></a>Ardışık hatlar grafiğinde gezinme

Boru hattı grafiği iç içe bir liste olarak düzenlenir. Dış liste, boru hattı grafiğindeki tüm modülleri açıklayan bir modül listesidir. İç liste, belirli bir modülün tüm bağlantılarını açıklayan bir bağlantı listesidir.  

1. Modül listesinde, modülleri değiştirmek için ok tuşunu kullanın.
1. Hedef modülün bağlantı listesini açmak için sekmeyi kullanın.
1. Modülün bağlantı noktaları arasında geçiş yapmak için ok tuşunu kullanın.
1. Hedef modüle gitmek için "G"yi kullanın.

## <a name="edit-the-pipeline-graph"></a>Boru hattı grafiğini edin

### <a name="add-a-module-to-the-graph"></a>Grafiğe modül ekleme

1. Odak noktasını tuvalden modül ağacına geçmek için Ctrl+F6'yı kullanın.
1. Standart ağaç görünümü denetimini kullanarak modül ağacında istenen modülü bulun.

### <a name="edit-a-module"></a>Modülü edin

Bir modülü başka bir modüle bağlamak için:

1. Bağlantı yardımcısını açmak için modül listesindeki bir modülü hedefalırken Ctrl + Shift + H kullanın.
1. Modülün bağlantı bağlantı noktalarını değiştirin.

Modül özelliklerini ayarlamak için:

1. Modül özelliklerini açmak için bir modülü hedefalırken Ctrl + Shift + E kullanın.
1. Modül özelliklerini değiştirin.

## <a name="navigation-shortcuts"></a>Gezinme kısayolları

| Tuş | Açıklama |
|-|-|
| Ctrl + F6 | Tuval ve modül ağacı arasında odak geçiş |
| Ctrl + F1   | Modül ağacındaki bir düğüme odaklanırken bilgi kartını açma |
| Ctrl + Shift + H | Odak düğümüzerindeyken bağlantı yardımcısını açma |
| Ctrl + Shift + E | Odak düğüm üzerindeyken modül özelliklerini açma |
| Ctrl + G | Ardışık hat lar çalışır çalıştırılamazsa odağı ilk başarısız düğüme taşıyın |

## <a name="action-shortcuts"></a>Eylem kısayolları

Erişim anahtarıyla aşağıdaki kısayolları kullanın. Erişim tuşları hakkında daha https://en.wikipedia.org/wiki/Access_keyfazla bilgi için bkz.

| Tuş | Eylem |
|-|-|
| Erişim tuşu + R | Çalıştırın |
| Erişim tuşu + P | Yayımlama |
| Erişim tuşu + C | Kopyalama |
| Erişim tuşu + D | Dağıtma |
| Erişim tuşu + I | Çıkarım ardışık lığı oluşturma/güncelleştirme |
| Erişim tuşu + B | Toplu çıkarma ardışık işletisi oluşturma/güncelleştirme |
| Erişim tuşu + K | "Çıkarım ardışık ardışık ardışık ardışık" açılır |
| Erişim tuşu + U | "Çıkarım ardışık hattını güncelleştirme" açılır |
| Erişim tuşu + M | Daha fazla aç(...) açılır |

## <a name="next-steps"></a>Sonraki adımlar

- [Yüksek karşıtlığı açma veya tema değiştirme](../azure-portal/azure-portal-change-theme-high-contrast.md)
- [Microsoft'ta erişilebilirlik ile ilgili araçlar](https://www.microsoft.com/accessibility)
