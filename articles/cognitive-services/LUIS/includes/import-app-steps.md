---
title: Uygulama adımlarını içeri aktar
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/30/2020
ms.author: diberry
ms.openlocfilehash: b7b8befa0f5871b65b9b5621bfb99c659bf07235
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80422740"
---
1. [ÖNIZLEME Luu portalında](https://preview.luis.ai), **uygulamalarım** sayfasında, **konuşma için + yeni uygulama**' yı seçin ve ardından **JSON olarak içeri aktarın**. Önceki adımdan kaydedilen JSON dosyasını bulun. Uygulamanın adını değiştirmeniz gerekmez. **Bitti** 'yi seçin

1. **Yönet** bölümünde, **sürümler** sekmesinde `0.1` sürümü seçin, sonra sürümü klonlamak için **Kopyala** ' yı seçin ve ardından yeni 10 karakterlik bir ad verin ve ardından, kopyalama işlemini bitirmeyi `ml-entity`bitirdiğinizde **bitti** ' yi seçin. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.

    > [!TIP]
    > Uygulamanızı değiştirmeden önce yeni bir sürüme kopyalama en iyi uygulamadır. Bir sürümle ilgili değişiklik ile bitirdiğinizde sürümü (. JSON veya. lu dosyası olarak) dışa aktarın ve dosyayı kaynak denetim sisteminize kontrol edin.

1. Bir LUSıS uygulamasının ana bina blokları olan amaçları görmek için **Oluştur** ve **amaçlar** ' ı seçin.

    ![Sürümler sayfasından amaçlar sayfasına geçin.](../media/tutorial-machine-learned-entity/new-version-imported-app.png)