---
author: craigktreasure
ms.service: azure-object-anchors
ms.topic: include
ms.date: 03/02/2021
ms.author: crtreasu
ms.openlocfilehash: 8b7ab183dfb7b6721beae7835acd75bc7a05e72c
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102044821"
---
### <a name="upload-your-model"></a>Modelinizi karşıya yükleyin

Henüz bir nesne bağlantıları modeliniz yoksa, [model](/object-anchors/quickstarts/get-started-model-conversion.md) oluşturma ' daki yönergeleri izleyerek bir tane oluşturun. Ardından buraya geri dönün.

HoloLens 'i Windows cihaz portalına bağladığınıza göre, uygulamanın kullanması için bir model yüklemek üzere aşağıdaki adımları izleyin:

1. Windows cihaz portalında, **System > dosya Gezgini ' ne > LocalAppData**' a gidin. Burada, uygulamanızı uygulamalar listesinde görmeniz gerekir.

    :::image type="content" source="./media/object-anchors-quickstarts-unity/portal-localappdata.png" alt-text="Dosya Gezgini":::

2. Uygulamanızı açın ve `LocalState` klasöre tıklayın.

    :::image type="content" source="./media/object-anchors-quickstarts-unity/portal-localstate.png" alt-text="LocalState klasörünü açın":::

3. Model dosyasını `LocalState` klasöre yükleyin.

    :::image type="content" source="./media/object-anchors-quickstarts/portal-upload-model.png" alt-text="modeli portala yükleme":::

    Uygulamayı HoloLens 'ten yeniden başlatın. Artık modelle eşleşen nesneleri tespit edebilirsiniz.
