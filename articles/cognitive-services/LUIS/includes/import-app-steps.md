---
title: Uygulama adımlarını içeri aktar
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: include
ms.date: 06/22/2020
ms.author: diberry
ms.openlocfilehash: d1eafde1cfb02234b2a52ccf4909ab47930177b9
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91334743"
---
1. [Luu portalında](https://www.luis.ai), **uygulamalarım** sayfasında, **konuşma için + yeni uygulama**' yı seçin ve ardından **JSON olarak içeri aktarın**. Önceki adımdan kaydedilen JSON dosyasını bulun. Uygulamanın adını değiştirmeniz gerekmez. **Bitti** 'yi seçin

1. **Yönet** bölümünde, **sürümler** sekmesinde `0.1` sürümü seçin, ardından sürümü klonlamak için **Kopyala** ' yı seçin ve ardından yeni bir adı verin ve `ml-entity` ardından Kopyala ' yı seçerek kopyalama işlemini sona **Done** erdirin. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.

    > [!TIP]
    > Uygulamanızı değiştirmeden önce yeni bir sürüme kopyalama en iyi uygulamadır. Bir sürümle ilgili değişiklik ile bitirdiğinizde sürümü (. JSON veya. lu dosyası olarak) dışa aktarın ve dosyayı kaynak denetim sisteminize kontrol edin.

1. Bir LUSıS uygulamasının ana bina blokları olan amaçları görmek için **Oluştur** ve **amaçlar** ' ı seçin.

    ![Sürümler sayfasından amaçlar sayfasına geçin.](../media/tutorial-machine-learned-entity/new-version-imported-app.png)
