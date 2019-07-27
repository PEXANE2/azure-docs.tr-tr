---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: d8b6b1bfcbceb1168d0f74c73e72bd42b41bb2ec
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562490"
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
> Bir `library not found for -lPods-Unity-iPhone` hata görürseniz dosya yerine `.xcodeproj` `.xcworkspace` dosyayı açmış olabilirsiniz. 

Proje ayarlarını görüntülemek için kök **Unity-iPhone** düğümünü seçin ve ardından **genel** sekmesini seçin.

**İmza**' ın altında, **imzalamayı otomatik olarak Yönet** ' in etkinleştirildiğinden emin olun. Yoksa, etkinleştirin ve sonra yapı ayarlarını sıfırla açılan iletişim kutusunda **Otomatik Etkinleştir** ' i seçin.

**Dağıtım bilgileri**altında **dağıtım hedefinin** olarak `11.0`ayarlandığından emin olun.

### <a name="deploy-the-app-to-your-ios-device"></a>Uygulamayı iOS cihazınıza dağıtma

İOS cihazını Mac 'e bağlayın ve **etkin düzeni** iOS cihazınıza ayarlayın.

![Cihazı seçin](./media/spatial-anchors-unity/select-device.png)

**Oluştur ' u seçin ve ardından geçerli düzeni çalıştırın**.

![Dağıt ve Çalıştır](./media/spatial-anchors-unity/deploy-run.png)