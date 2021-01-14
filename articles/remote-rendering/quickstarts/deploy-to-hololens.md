---
title: Unity örneğini HoloLens’e dağıtma
description: Unity örneğinin HoloLens'te nasıl alınacağını gösteren hızlı başlangıç
author: jakrams
ms.author: jakras
ms.date: 02/14/2020
ms.topic: quickstart
ms.openlocfilehash: a5a44b9d27901fcd00f8813096de6bbae5f2310d
ms.sourcegitcommit: c136985b3733640892fee4d7c557d40665a660af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98179954"
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
    1. *Makine adını* **HoloLens 'nizin IP 'si** olarak değiştirin

## <a name="launch-the-sample-project"></a>Örnek projeyi başlatma

1. HoloLens'i USB kablosuyla bilgisayarınıza bağlayın.
1. Visual Studio'da Hata Ayıklayıcı'yı başlatın (F5). Bu, uygulamayı otomatik olarak cihaza dağıtır.

Örnek uygulama ve ardından yeni bir oturum başlatılmalıdır. Bir süre sonra oturum hazır olur ve uzaktan işlenen model karşınıza gelir.
Örneği daha sonra ikinci kez başlatmak isterseniz, artık HoloLens başlat menüsünden de bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki hızlı başlangıçta özel modülü dönüştürme konusunu gözden geçireceğiz.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Modeli işlenmek üzere dönüştürme](convert-model.md)
