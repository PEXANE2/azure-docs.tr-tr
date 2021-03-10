---
author: craigktreasure
ms.service: azure-object-anchors
ms.topic: include
ms.date: 03/02/2021
ms.author: crtreasu
ms.openlocfilehash: d8dfc3d4b7a8447250481b98c1adadc865a29da1
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102532729"
---
### <a name="upload-your-model"></a>Modelinizi karşıya yükleyin

Henüz bir nesne bağlantıları modeliniz yoksa, [model](/azure/object-anchors/quickstarts/get-started-model-conversion) oluşturma ' daki yönergeleri izleyerek bir tane oluşturun. Ardından buraya geri dönün.

HoloLens 'i Windows cihaz portalına bağladığınıza göre, uygulamanın kullanması için bir model yüklemek üzere aşağıdaki adımları izleyin:

1. Windows cihaz portalında, **System > dosya Gezgini ' ne > LocalAppData**' a gidin. Burada, uygulamanızı uygulamalar listesinde görmeniz gerekir.

    :::image type="content" source="./media/object-anchors-quickstarts-unity/portal-localappdata.png" alt-text="Dosya Gezgini":::

2. Uygulamanızı açın ve `LocalState` klasöre tıklayın.

    :::image type="content" source="./media/object-anchors-quickstarts-unity/portal-localstate.png" alt-text="LocalState klasörünü açın":::

3. Model dosyasını `LocalState` klasöre yükleyin.

    :::image type="content" source="./media/object-anchors-quickstarts/portal-upload-model.png" alt-text="modeli portala yükleme":::

    Uygulamayı HoloLens 'ten yeniden başlatın. Artık modelle eşleşen nesneleri tespit edebilirsiniz.
