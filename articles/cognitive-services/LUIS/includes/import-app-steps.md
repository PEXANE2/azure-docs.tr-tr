---
title: Uygulama adımlarını içeri aktar
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/22/2020
ms.author: diberry
ms.openlocfilehash: 37f1b85b4ce8510d5e288df985a55dba659f0c9b
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86035473"
---
1. [Luu portalında](https://www.luis.ai), **uygulamalarım** sayfasında, **konuşma için + yeni uygulama**' yı seçin ve ardından **JSON olarak içeri aktarın**. Önceki adımdan kaydedilen JSON dosyasını bulun. Uygulamanın adını değiştirmeniz gerekmez. **Bitti** 'yi seçin

1. **Yönet** bölümünde, **sürümler** sekmesinde `0.1` sürümü seçin, ardından sürümü klonlamak için **Kopyala** ' yı seçin ve ardından yeni bir adı verin ve `ml-entity` ardından Kopyala ' yı seçerek kopyalama işlemini sona **Done** erdirin. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.

    > [!TIP]
    > Uygulamanızı değiştirmeden önce yeni bir sürüme kopyalama en iyi uygulamadır. Bir sürümle ilgili değişiklik ile bitirdiğinizde sürümü (. JSON veya. lu dosyası olarak) dışa aktarın ve dosyayı kaynak denetim sisteminize kontrol edin.

1. Bir LUSıS uygulamasının ana bina blokları olan amaçları görmek için **Oluştur** ve **amaçlar** ' ı seçin.

    ![Sürümler sayfasından amaçlar sayfasına geçin.](../media/tutorial-machine-learned-entity/new-version-imported-app.png)
