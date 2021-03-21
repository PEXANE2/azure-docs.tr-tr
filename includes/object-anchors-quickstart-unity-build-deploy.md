---
author: craigktreasure
ms.service: azure-object-anchors
ms.topic: include
ms.date: 04/03/2020
ms.author: crtreasu
ms.openlocfilehash: 43c8c578587c65b6e0317caf77ff2d0604cb4fa3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102044675"
---
### <a name="build-and-deploy-the-app"></a>Uygulama derleme ve dağıtma

`.sln`Unity tarafından oluşturulan dosyayı açın. Yapı yapılandırmasını aşağıdaki şekilde değiştirin.

:::image type="content" source="./media/object-anchors-quickstarts-unity/aoa-unity-vs-build-config.png" alt-text="Derleme yapılandırması":::

Sonra, uygulamanın dağıtılması ve hata ayıklaması için **uzak makıne IP adresini** yapılandırmanız gerekir.

Uygulama projesine sağ tıklayın ve **Özellikler**' i seçin. Özellikler sayfasında **yapılandırma özellikleri-> hata ayıklama**' ı seçin. **Makine adı** değerini, HOLOLENS cihazınızın IP adresi olarak değiştirin ve **Uygula**' ya tıklayın.

:::image type="content" source="./media/object-anchors-quickstarts-unity/aoa-unity-vs-remote-debug.png" alt-text="Uzaktan hata ayıklama":::

Özellik sayfasını kapatın. **Uzak makine**' ye tıklayın. Uygulamanın, uzak cihazınıza derleyip dağıtmaya başlaması gerekir. Cihazınızın etkin olduğundan emin olun.
