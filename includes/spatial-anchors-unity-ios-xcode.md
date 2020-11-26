---
author: msftradford
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 11/20/2020
ms.author: parkerra
ms.openlocfilehash: d969b1639dbb39b920c985964abe1239f068d740
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96185442"
---
**Oluştur**' u seçin. Açılan bölmede, Xcode projesinin dışarı aktarılacağı klasörü seçin.

   Dışarı aktarma işlemi tamamlandığında dışarı aktarılmış Xcode projesini içeren bir klasör görüntülenir.

   > [!NOTE]
   > Değiştirmek veya eklemek isteyip istemediğinizi soran bir ileti içeren bir pencere görünürse, daha hızlı olduğundan **Ekle**' yi seçmenizi öneririz. Yalnızca sahneinizdeki varlıkları değiştiriyorsanız **Değiştir** ' i seçmeniz gerekir. Örneğin, üst/alt ilişkileri ekliyor, kaldırıyor veya değiştiriyor olabilirsiniz veya özellikleri ekleme, kaldırma ya da değiştirme olabilirsiniz. Yalnızca kaynak kodu değişikliği yapıyorsanız, **ekleme** yeterli olmalıdır.

## <a name="open-the-xcode-project"></a>Xcode projesini açın

Artık `Unity-iPhone.xcodeproj` projenizi Xcode 'da açabilirsiniz. 

`Unity-iPhone.xcodeproj`Projeyi verdiğiniz konumdan aşağıdaki komutu çalıştırarak Xcode 'u başlatabilir ve içe aktarılmış projeyi açabilir ya da projeyi Xcode 'da başlatabilirsiniz:

 ```bash
open ./Unity-iPhone.xcodeproj
```

Proje ayarlarını görüntülemek için kök **Unity-iPhone** düğümünü seçin ve ardından **genel** sekmesini seçin.

**İmza**' ın altında, **imzalamayı otomatik olarak Yönet** ' in etkinleştirildiğinden emin olun. Aksi takdirde, etkinleştirin ve ardından açılan bölmedeki **Otomatik Etkinleştir** ' i seçerek yapı ayarlarını sıfırlayın.

**Dağıtım bilgileri** altında **dağıtım hedefinin** **11,0** olarak ayarlandığından emin olun.

## <a name="deploy-the-app-to-your-ios-device"></a>Uygulamayı iOS cihazınıza dağıtma

İOS cihazını Mac 'e bağlayın ve **etkin düzeni** iOS cihazınıza ayarlayın.

   ![Cihazı seçmek için iPhone 'Um düğmesinin ekran görüntüsü.](./media/spatial-anchors-unity/select-device.png)

**Oluştur ' u seçin ve ardından geçerli düzeni çalıştırın**.

   !["Dağıt ve Çalıştır" ok düğmesinin ekran görüntüsü.](./media/spatial-anchors-unity/deploy-run.png)
