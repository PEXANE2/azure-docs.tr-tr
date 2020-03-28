---
title: Uygulama adımlarını içe aktarma
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 12/03/2019
ms.author: diberry
ms.openlocfilehash: b0d1735df7a3ac36e20af771939aec6b8013f2df
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74840825"
---
1. Önizleme [LUIS portalında,](https://preview.luis.ai) **Uygulamalarım** sayfasında, **Içe Aktar'ı**seçin, ardından **JSON olarak içe aktarın.** Önceki adımdan kaydedilen JSON dosyasını bulun. Uygulamanın adını değiştirmeniz gerekmez. **Bitti'yi** seçin

1. **Sürümler** sekmesinde **Yönet** bölümünden sürümü seçin, ardından sürümü klonlamak için **Klon'u** seçin ve ona yeni bir 10 karakterlik ad verin, ardından klon işlemini tamamlamak için **Bitti'yi** seçin. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.

    > [!TIP]
    > Uygulamanızı değiştirmeden önce yeni bir sürüme klonlamak en iyi yöntemdir. Bir sürümü tamamladığınızda, sürümü (.json veya .lu dosyası olarak) dışa aktarın ve dosyayı kaynak denetim sisteminize denetleyin.

1. Bir LUIS uygulamasının ana yapı taşları olan amaçları görmek için **Build** sonra **Intents'i** seçin.

    ![Sürümler sayfasından Niyetler sayfasına değiştirin.](../media/tutorial-machine-learned-entity/new-version-imported-app.png)