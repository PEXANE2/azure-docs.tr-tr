---
title: Uygulama adımlarını içeri aktar
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: include
ms.date: 10/16/2020
ms.openlocfilehash: b70a22e502de5441b5c91a8f33357375834f54e1
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93130755"
---
1. [Luu portalında](https://www.luis.ai), **uygulamalarım** sayfasında, **konuşma için + yeni uygulama** ' yı seçin ve ardından **JSON olarak içeri aktarın** . Önceki adımdan kaydedilen JSON dosyasını bulun. Uygulamanın adını değiştirmeniz gerekmez. **Bitti** 'yi seçin

1. **Yönet** bölümünde, **sürümler** sekmesinde `0.1` sürümü seçin, ardından sürümü klonlamak için **Kopyala** ' yı seçin ve ardından yeni bir adı verin ve `ml-entity` ardından Kopyala ' yı seçerek kopyalama işlemini sona **Done** erdirin. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.

    > [!TIP]
    > Uygulamanızı değiştirmeden önce yeni bir sürüme kopyalama en iyi uygulamadır. Bir sürümle ilgili değişiklik ile bitirdiğinizde sürümü (. JSON veya. lu dosyası olarak) dışa aktarın ve dosyayı kaynak denetim sisteminize kontrol edin.

1. Ekranın üst kısmında **Oluştur** ' u seçin ve ardından sol gezinti menüsünde **amaçlar** ' a tıklayın. Bir LUO uygulamasının ana yapı taşları olan uygulamanın amaçlarını görürsünüz.
