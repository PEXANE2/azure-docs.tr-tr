---
title: Unity örneğini HoloLens'e dağıtın
description: Unity örneğini HoloLens'e nasıl aktarılacağınızda gösteren hızlı başlangıç
author: jakrams
ms.author: jakras
ms.date: 02/14/2020
ms.topic: quickstart
ms.openlocfilehash: 75b6629ea924ec17888ec2f981ff85ed917434c9
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679740"
---
# <a name="quickstart-deploy-unity-sample-to-hololens"></a>Quickstart: HoloLens'e Birlik örneğini dağıtın

Bu hızlı başlangıç, Unity için hızlı başlatma örnek uygulamasının HoloLens 2'ye nasıl dağıtılanup çalıştırılabildiğini kapsar.

Bu hızlı başlangıçta nasıl öğreneceksiniz:

> [!div class="checklist"]
>
>* HoloLens için hızlı başlangıç örneği uygulaması oluşturun
>* Örneği aygıta dağıtma
>* Örneği aygıtta çalıştırma

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıçta örnek projeyi [Quickstart:Unity ile bir model oluşturma'dan](render-model.md)dağıtacağız.

Kimlik bilgilerinizin sahneyle birlikte doğru şekilde kaydolduğundan emin olun ve Unity düzenleyicisinin içinden bir oturuma bağlanabilirsiniz.

## <a name="build-the-sample-project"></a>Örnek projeyi oluşturma

1. *Dosya > Yapı Ayarları'nı*açın.
1. *Platformu* **Evrensel Windows Platformuna** Değiştir
1. *Hedef Cihazı* **HoloLens'e** Ayarla
1. *Mimariyi* **ARM64** olarak ayarlayın
1. *Yapı Türünü* **D3D Proje** ![Oluşturma ayarlarına ayarlama](./media/unity-build-settings.png)
1. **Platforma Geçiş'i** seçin
1. **Oluştur** 'veya 'Çalıştır'a bastığınızda, çözümün depolanması gereken bazı klasörü seçmeniz istenir
1. Visual Studio ile oluşturulan **Quickstart.sln'yi** açın
1. Yapılandırmayı **Release** ve **ARM64** olarak değiştirin
1. Hata ayıklama modunu **Uzak Makine** ![Çözümü yapılandırmasına değiştirme](media/unity-deploy-config.png)
1. Çözümü oluşturun (F7)
1. 'Quickstart' projesi için, Hata *Ayıklama özellikleri >* gidin
    1. Yapılandırma *Sürümü'nün* etkin olduğundan emin olun
    1. *Hata Ayıklama'yı* **Uzak Makineye** Fırlatacak Şekilde Ayarlayın
    1. *Makine Adını* **HoleLens'inizin IP'si** ile değiştirin

## <a name="launch-the-sample-project"></a>Örnek projeyi başlatın

1. HoloLens'i bir USB kablosuyla bilgisayarınıza bağlayın.
1. Debugger'ı Visual Studio(F5) ile başlatın. Uygulamayı otomatik olarak cihaza dağıtır.

Örnek uygulama başlatılmalı ve ardından yeni bir oturum başlatılmalıdır. Bir süre sonra oturum hazır olur ve uzaktan işlenen model önünüzde görünür.
Örneği ikinci kez başlatmak isterseniz, şimdi HoloLens başlangıç menüsünden de bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki hızlı başlangıçta, özel bir modeli dönüştürmeye bir göz atacağız.

> [!div class="nextstepaction"]
> [Quickstart: Oluşturma için bir modeli dönüştürme](convert-model.md)
