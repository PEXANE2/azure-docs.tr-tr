---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: b052772bbfe9d69e430d9f722d8db56b48db7610
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "72933496"
---
**Yapı'yı**seçin. Açılan iletişim kutusunda, Xcode projesini dışa aktarmak için bir klasör seçin.

Dışa aktarma tamamlandığında, dışa aktarılan Xcode projesini içeren bir klasör görüntülenir.

> [!NOTE]
> Değiştirmeveya uygulama isteyip istemediğinisoran bir pencere görünürse, daha hızlı olduğu için **Append'i** seçmenizi öneririz. Yalnızca sahnenizdeki varlıkları değiştiriyorsanız **Değiştir'i** seçmeniz gerekir. (Örneğin, üst/alt ilişkileri ekliyor, kaldırıyor veya değiştiriyorsanız veya özellikler ekliyor, kaldırıyor veya değiştiriyorsanız.) Yalnızca kaynak kodu değişiklikleri yapıyorsanız, **Append** yeterli olmalıdır.

### <a name="convert-the-xcode-project-to-xcworkspace-containing-azure-spatial-anchors-references"></a>Xcode projesini Azure Uzamsal Bağlantı referansları içeren xcworkspace'e dönüştürün

Dışa aktarılan Xcode proje klasöründe, proje için gerekli CocoaPod'ları yüklemek için bu komutu Terminal'de çalıştırın:

```bash
pod install --repo-update
```

Artık projeyi `Unity-iPhone.xcworkspace` Xcode'da açmak için açabilirsiniz:

```bash
open ./Unity-iPhone.xcworkspace
```

> [!NOTE]
> MacOS Catalina'ya (10.15) yükselttikten sonra CocoaPod sorunları yaşıyorsanız sorun giderme adımlarını [buradan](../articles/spatial-anchors/quickstarts/get-started-unity-ios.md#cocoapods-issues-on-macos-catalina-1015) görebilirsiniz.

Proje ayarlarını görüntülemek için kök **Unity-iPhone** düğümünü seçin ve ardından **Genel** sekmesini seçin.

**İmzalama**altında, **otomatik olarak imzalamayı yönetmek** etkin olduğundan emin olun. Değilse, etkinleştirin ve ardından yapı ayarlarını sıfırlamak için görünen iletişim kutusunda **Otomatik'i etkinleştir'i** seçin.

**Dağıtım Bilgileri**altında, **Dağıtım Hedefi'nin** `11.0`' olarak ayarlandıkundan emin olun.

### <a name="deploy-the-app-to-your-ios-device"></a>Uygulamayı iOS aygıtınıza dağıtma

iOS aygıtını Mac'e bağlayın ve **etkin düzeni** iOS aygıtınıza ayarlayın.

![Cihazı seçin](./media/spatial-anchors-unity/select-device.png)

Yapı'yı seçin **ve ardından geçerli düzeni çalıştırın.**

![Dağıt Ve çalıştır](./media/spatial-anchors-unity/deploy-run.png)

> [!NOTE]
> Bir `library not found for -lPods-Unity-iPhone` hata görürseniz, büyük `.xcodeproj` olasılıkla dosya `.xcworkspace` yerine dosyayı açmışsınızdır.
