---
title: Unity örneğini Masaüstüne dağıtma
description: Unity örneğini masaüstü bilgisayara nasıl alacağımı gösteren hızlı başlatma
author: christophermanthei
ms.author: chmant
ms.date: 03/20/2020
ms.topic: quickstart
ms.openlocfilehash: 95993a54d321c9581d35f12b56d60e4ea052e505
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415715"
---
# <a name="quickstart-deploy-unity-sample-to-desktop"></a>Quickstart: Unity örneğini Masaüstüne dağıtın

Bu hızlı başlangıç, Unity için hızlı başlatma örnek uygulamasını bir masaüstü bilgisayara nasıl dağıtılanın ve çalıştırılabilmek için olduğunu kapsar.

Bu hızlı başlangıçta nasıl öğreneceksiniz:

> [!div class="checklist"]
>
>* Masaüstü için hızlı başlatma örnek uygulamasını oluşturun
>* Örneği bilgisayara dağıtma
>* Örneği bilgisayarda çalıştırma

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıçta örnek projeyi [Quickstart:Unity ile bir model oluşturma'dan](render-model.md)dağıtacağız.

Kimlik bilgilerinizin sahneyle birlikte doğru şekilde kaydolduğundan emin olun ve Unity düzenleyicisinin içinden bir oturuma bağlanabilirsiniz.

## <a name="disable-virtual-reality-support"></a>Sanal gerçeklik desteğini devre dışı

Vr desteğinin devre dışı bırakılması için şu anda masaüstünde yalnızca düz masaüstü uygulamaları desteklenir.

1. *Proje Ayarlarını > Edit'i Aç...*
1. Soldaki **Player'ı** seçin.
1. Evrensel **Windows Platformu ayarları** sekmesini seçin.
1. **XR Ayarlarını**Genişletin.
1. **Sanal Gerçeklik Destekli**devre dışı.
    ![oyuncu ayarları](./media/unity-disable-xr.png)
1. *XR Ayarları'nın*üzerinde, **Yayımlama Ayarlarını genişletin.**
1. **Desteklenen Aygıt Ailelerinde**Masaüstü'nin **Desktop** kontrol edildiğinden emin olun.

## <a name="build-the-sample-project"></a>Örnek projeyi oluşturma

1. *Dosya > Yapı Ayarları'nı*açın.
1. *Platformu* **Evrensel Windows Platformu'na**değiştirin.
1. *Hedef Aygıtı* **PC'ye**ayarlayın.
1. *Mimari'yi* **x86**olarak ayarlayın.
1. *Yapı Türünü* **D3D Projesi'ne**ayarlayın.
  ![Yapı ayarları](./media/unity-build-settings-pc.png)
1. **Platforma Geçiş'i**seçin.
1. **Yap** (veya 'Yap ve Çalıştır' tuşuna bastığınızda), çözümün depolanması gereken bazı klasörü seçmeniz istenir.
1. Visual Studio ile oluşturulan **Quickstart.sln'yi** açın.
1. Yapılandırmayı **Release** ve **x86**olarak değiştirin.
1. Hata ayıklama modunu **Yerel Makine'ye**geçirin.
  ![Çözüm yapılandırması](./media/unity-deploy-config-pc.png)
1. Çözümü oluşturun (F7).

> [!WARNING]
> **x86 seçtiğinizden**emin olun. **x64** şu anda desteklenmiyor, [platform sınırlamaları](../reference/limits.md#platform-limitations)bakın.

## <a name="launch-the-sample-project"></a>Örnek projeyi başlatın

Debugger'ı Visual Studio(F5) ile başlatın. Uygulamayı otomatik olarak BILGISAYARa dağıtır.

Örnek uygulama başlatılmalı ve ardından yeni bir oturum başlatılmalıdır. Bir süre sonra oturum hazır olur ve uzaktan işlenen model önünüzde görünür.
Örneği ikinci kez başlatmak isterseniz, şimdi Başlat menüsünden de bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki hızlı başlangıçta, özel bir modeli dönüştürmeye bir göz atacağız.

> [!div class="nextstepaction"]
> [Quickstart: Oluşturma için bir modeli dönüştürme](convert-model.md)
