---
title: Unity örneğini HoloLens’e dağıtma
description: HoloLens üzerinde Unity örneğini almayı gösteren hızlı başlangıç
author: jakrams
ms.author: jakras
ms.date: 02/14/2020
ms.topic: quickstart
ms.openlocfilehash: 3eec935d0a25f9510cd9a2f6e00b7ac22756e697
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88796808"
---
# <a name="quickstart-deploy-unity-sample-to-hololens"></a>Hızlı başlangıç: Unity örneğini HoloLens 'e dağıtma

Bu hızlı başlangıçta, Unity için hızlı başlangıç örnek uygulamasının bir HoloLens 2 ' ye nasıl dağıtılacağı ve çalıştırılacağı ele alınmaktadır.

Bu hızlı başlangıçta şunları nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
>
>* HoloLens için hızlı başlangıç örnek uygulaması oluşturma
>* Örneği cihaza dağıtın
>* Örneği cihazda çalıştırın

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta örnek projeyi hızlı başlangıç konumundan dağıtacağız [: Unity ile model işleme](render-model.md).

Kimlik bilgilerinizin sahneye doğru şekilde kaydedildiğinden emin olun ve Unity Düzenleyicisi içinden bir oturuma bağlanabilirsiniz.

## <a name="build-the-sample-project"></a>Örnek projeyi oluşturma

1. *Dosya > derleme ayarları*'nı açın.
1. *Platformu* **Evrensel Windows platformu** olarak değiştir
1. *Hedef cihazı* **HoloLens** olarak ayarla
1. *Mimariyi* **ARM64** olarak ayarla
1. *Yapı türünü* **D3D projesi** olarak ayarla\
    ![Derleme ayarları](./media/unity-build-settings.png)
1. **Platforma geç** ' i seçin
1. **Derleme** (veya ' derleme ve çalıştırma ') tuşlarına basıldığında çözümün depolanacağı bir klasör seçmeniz istenir
1. Visual Studio ile oluşturulan **hızlı başlangıç. sln** 'yi açın
1. Yapılandırmayı **Release** ve **ARM64** olarak değiştirme
1. Hata ayıklayıcı modunu **uzak makineye** değiştirme\
    ![Çözüm yapılandırması](media/unity-deploy-config.png)
1. Çözümü derleme
1. ' Hızlı başlangıç ' projesi için *özellikler > hata ayıklama* sayfasına gidin
    1. Yapılandırma *sürümünün* etkin olduğundan emin olun
    1. Hata ayıklayıcıyı **uzak makineye** *başlatılacak şekilde* ayarla
    1. *Makine adını* **Hoellens sitenizin IP 'si** olarak değiştirin

## <a name="launch-the-sample-project"></a>Örnek projeyi başlatma

1. HoloLens 'i bir USB kablosu ile BILGISAYARıNıZA bağlayın.
1. Visual Studio 'da hata ayıklayıcıyı başlatın (F5). Uygulamayı cihaza otomatik olarak dağıtır.

Örnek uygulama, yeni bir oturum başlatıp başlatmalıdır. Bir süre sonra, oturum hazırlanıyor ve uzaktan işlenen model sizin sizin için görünür olacaktır.
Daha sonra örneği ikinci kez başlatmak isterseniz, şimdi HoloLens Başlat menüsünden de bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki hızlı başlangıçta, özel bir modeli dönüştürmeye göz atacağız.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: bir modeli işleme için dönüştürme](convert-model.md)
