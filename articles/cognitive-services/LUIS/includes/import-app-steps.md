---
title: Uygulama adımlarını içe aktarma
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/30/2020
ms.author: diberry
ms.openlocfilehash: b7b8befa0f5871b65b9b5621bfb99c659bf07235
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422740"
---
1. Önizleme [LUIS portalında](https://preview.luis.ai), **Uygulamalarım** sayfasında , **+ Konuşma için yeni uygulama**seçin , sonra **JSON olarak içe aktarın.** Önceki adımdan kaydedilen JSON dosyasını bulun. Uygulamanın adını değiştirmeniz gerekmez. **Bitti'yi** seçin

1. **Sürümler** sekmesinde `0.1` **Yönet** bölümünden sürümü seçin, ardından sürümü klonlamak için **Klon'u** seçin ve yeni `ml-entity`bir 10 karakterlik ad verin, ardından klon işlemini tamamlamak için **Bitti'yi** seçin. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.

    > [!TIP]
    > Uygulamanızı değiştirmeden önce yeni bir sürüme klonlamak en iyi yöntemdir. Sürüm değişikliğiyle bitirdiğinizde, sürümü (.json veya .lu dosyası olarak) dışa aktarın ve dosyayı kaynak denetim sisteminize denetleyin.

1. Bir LUIS uygulamasının ana yapı taşları olan amaçları görmek için **Build** sonra **Intents'i** seçin.

    ![Sürümler sayfasından Niyetler sayfasına değiştirin.](../media/tutorial-machine-learned-entity/new-version-imported-app.png)