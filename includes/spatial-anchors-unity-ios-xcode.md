---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: b052772bbfe9d69e430d9f722d8db56b48db7610
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72933496"
---
**Oluştur**' u seçin. Açılan iletişim kutusunda, Xcode projesinin dışarı aktarılacağı klasörü seçin.

Dışarı aktarma işlemi tamamlandığında dışarı aktarılmış Xcode projesini içeren bir klasör görüntülenir.

> [!NOTE]
> Değiştirmek veya eklemek isteyip istemediğinizi soran bir pencere görünürse, daha hızlı olduğundan **ekleme** ' yi seçmenizi öneririz. Yalnızca sahneinizdeki varlıkları değiştiriyorsanız **Değiştir** ' i seçmeniz gerekir. (Örneğin, üst/alt ilişkileri ekliyor, kaldırdıysanız veya değiştiriyorsanız ya da özellikleri ekliyorsanız, kaldırıyorsanız veya değiştiriyorsanız.) Yalnızca kaynak kodu değişikliği yapıyorsanız, **ekleme** yeterli olmalıdır.

### <a name="convert-the-xcode-project-to-xcworkspace-containing-azure-spatial-anchors-references"></a>Xcode projesini Azure uzamsal bağlayıcı başvurularını içeren xcworkspace 'e Dönüştür

Proje için gerekli CocoaPods yüklemek üzere, dışarıya aktarılmış Xcode proje klasöründe bu komutu terminalde çalıştırın:

```bash
pod install --repo-update
```

Artık projeyi Xcode `Unity-iPhone.xcworkspace` 'da açmak için açabilirsiniz:

```bash
open ./Unity-iPhone.xcworkspace
```

> [!NOTE]
> MacOS Catalina 'e yükselttikten sonra CocoaPod sorunları [yaşıyorsanız, sorun giderme adımlarına bakın](../articles/spatial-anchors/quickstarts/get-started-unity-ios.md#cocoapods-issues-on-macos-catalina-1015) (10,15).

Proje ayarlarını görüntülemek için kök **Unity-iPhone** düğümünü seçin ve ardından **genel** sekmesini seçin.

**İmza**' ın altında, **imzalamayı otomatik olarak Yönet** ' in etkinleştirildiğinden emin olun. Yoksa, etkinleştirin ve sonra yapı ayarlarını sıfırla açılan iletişim kutusunda **Otomatik Etkinleştir** ' i seçin.

**Dağıtım bilgileri**altında **dağıtım hedefinin** olarak `11.0`ayarlandığından emin olun.

### <a name="deploy-the-app-to-your-ios-device"></a>Uygulamayı iOS cihazınıza dağıtma

İOS cihazını Mac 'e bağlayın ve **etkin düzeni** iOS cihazınıza ayarlayın.

![Cihazı seçin](./media/spatial-anchors-unity/select-device.png)

**Oluştur ' u seçin ve ardından geçerli düzeni çalıştırın**.

![Dağıt ve Çalıştır](./media/spatial-anchors-unity/deploy-run.png)

> [!NOTE]
> Bir `library not found for -lPods-Unity-iPhone` hata görürseniz dosya yerine `.xcodeproj` `.xcworkspace` dosyayı açmış olabilirsiniz.
