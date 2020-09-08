---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 08/14/2020
ms.author: rgarcia
ms.openlocfilehash: b93243a537fafce6d865ec207b12dc2654cafd20
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89536286"
---
**Oluştur**' u seçin. Açılan iletişim kutusunda, Xcode projesinin dışarı aktarılacağı klasörü seçin.

Dışarı aktarma işlemi tamamlandığında dışarı aktarılmış Xcode projesini içeren bir klasör görüntülenir.

> [!NOTE]
> Değiştirmek veya eklemek isteyip istemediğinizi soran bir pencere görünürse, daha hızlı olduğundan **ekleme** ' yi seçmenizi öneririz. Yalnızca sahneinizdeki varlıkları değiştiriyorsanız **Değiştir** ' i seçmeniz gerekir. (Örneğin, üst/alt ilişkileri ekliyor, kaldırdıysanız veya değiştiriyorsanız ya da özellikleri ekliyorsanız, kaldırıyorsanız veya değiştiriyorsanız.) Yalnızca kaynak kodu değişikliği yapıyorsanız, **ekleme** yeterli olmalıdır.

## <a name="open-the-xcode-project"></a>Xcode projesini açın

Artık `Unity-iPhone.xcodeproj` Xcode 'da açabilirsiniz. `Unity-iPhone.xcodeproj`Projeyi verdiğiniz konumdan aşağıdaki komutu çalıştırarak Xcode 'u başlatabilir ve içe aktarılmış projeyi açabilir ya da projeyi Xcode 'da başlatabilirsiniz:

```bash
open ./Unity-iPhone.xcodeproj
```

Proje ayarlarını görüntülemek için kök **Unity-iPhone** düğümünü seçin ve ardından **genel** sekmesini seçin.

**İmza**' ın altında, **imzalamayı otomatik olarak Yönet** ' in etkinleştirildiğinden emin olun. Yoksa, etkinleştirin ve sonra yapı ayarlarını sıfırla açılan iletişim kutusunda **Otomatik Etkinleştir** ' i seçin.

**Dağıtım bilgileri**altında **dağıtım hedefinin** olarak ayarlandığından emin olun `11.0` .

## <a name="deploy-the-app-to-your-ios-device"></a>Uygulamayı iOS cihazınıza dağıtma

İOS cihazını Mac 'e bağlayın ve **etkin düzeni** iOS cihazınıza ayarlayın.

![Cihazı seçin](./media/spatial-anchors-unity/select-device.png)

**Oluştur ' u seçin ve ardından geçerli düzeni çalıştırın**.

![Dağıt ve Çalıştır](./media/spatial-anchors-unity/deploy-run.png)
