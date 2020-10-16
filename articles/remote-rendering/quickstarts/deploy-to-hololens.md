---
title: Unity örneğini HoloLens’e dağıtma
description: Unity örneğinin HoloLens'te nasıl alınacağını gösteren hızlı başlangıç
author: jakrams
ms.author: jakras
ms.date: 02/14/2020
ms.topic: quickstart
ms.openlocfilehash: 3eec935d0a25f9510cd9a2f6e00b7ac22756e697
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "88796808"
---
# <a name="quickstart-deploy-unity-sample-to-hololens"></a>Hızlı Başlangıç: Unity örneğini HoloLens’e dağıtma

Bu hızlı başlangıç, Unity için hızlı başlangıç örnek uygulamasını HoloLens 2'de dağıtma ve çalıştırma işlemlerini kapsar.

Bu hızlı başlangıçta şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
>
>* HoloLens için hızlı başlangıç örnek uygulamasını derleme
>* Örneği cihaza dağıtma
>* Örneği cihazda çalıştırma

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıçta [Hızlı Başlangıç: Unity ile model işleme](render-model.md) bölümündeki örnek uygulamayı dağıtacağız.

Kimlik bilgilerinizin sahneyle düzgün kaydedildiğinden ve Unity düzenleyicisinin içinden bir oturuma bağlanabildiğinizden emin olun.

## <a name="build-the-sample-project"></a>Örnek projeyi derleme

1. *Dosya > Derleme Ayarları*'nı açın.
1. *Platform*'u **Evrensel Windows Platformu** olarak değiştirin
1. *Hedef Cihaz*'ı **HoloLens** olarak ayarlayın
1. *Mimari*'yi **ARM64** olarak ayarlayın
1. *Derleme Türü*'nü **D3D Project**\ olarak ayarlayın
    ![Derleme ayarları](./media/unity-build-settings.png)
1. **Platforma Geç**'i seçin
1. **Derle**'ye (veya 'Derle ve Çalıştır') bastığınızda çözümün depolanacağı klasörü seçmeniz istenir
1. Oluşturulan **Quickstart.sln**'yi Visual Studio ile açın
1. Yapılandırmayı **Sürüm** ve **ARM64** olarak değiştirin
1. Hata ayıklayıcı modunu **Uzak Makine**\ olarak değiştirin
    ![Çözüm yapılandırması](media/unity-deploy-config.png)
1. Çözümü derleme
1. 'Quickstart' projesi için *Özellikler > Hata Ayıklama*'ya gidin
    1. Yapılandırma olarak *Sürüm*'ün etkin olduğundan emin olun
    1. *Çalıştırılacak Hata Ayıklayıcı*'yı **Uzak Makine** olarak ayarlayın
    1. *Makine Adı*'nı **HoleLens IP'si** olarak değiştirin

## <a name="launch-the-sample-project"></a>Örnek projeyi başlatma

1. HoloLens'i USB kablosuyla bilgisayarınıza bağlayın.
1. Visual Studio'da Hata Ayıklayıcı'yı başlatın (F5). Bu, uygulamayı otomatik olarak cihaza dağıtır.

Örnek uygulama ve ardından yeni bir oturum başlatılmalıdır. Bir süre sonra oturum hazır olur ve uzaktan işlenen model karşınıza gelir.
Örneği daha sonra ikinci kez başlatmak isterseniz, artık HoloLens başlat menüsünden de bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki hızlı başlangıçta özel modülü dönüştürme konusunu gözden geçireceğiz.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Modeli işlenmek üzere dönüştürme](convert-model.md)
