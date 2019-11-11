---
title: include dosyası
description: include dosyası
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 09/24/2019
ms.custom: include file
ms.openlocfilehash: 92b9a4754769566feb3658e07081e9fdae78fcfc
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2019
ms.locfileid: "73903968"
---
1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Giriş sayfasından **+ kaynak oluştur**' u seçin. **Dijital TWINS**araması yapın ve **dijital TWINS**' i seçin. Dağıtım işlemini başlatmak için **Oluştur**’u seçin.

   [Yeni bir dijital TWINS örneği oluşturmak için ![seçimleri](./media/create-digital-twins-portal/create-digital-twins.png)](./media/create-digital-twins-portal/create-digital-twins.png#lightbox)

1. **Digital Twins** bölmesine şu bilgileri girin:
   * **Kaynak Adı**: Digital Twins örneğiniz için benzersiz bir ad oluşturun.
   * **Abonelik**: Bu Digital Twins örneğini oluşturmak için kullanmak istediğiniz aboneliği seçin. 
   * **Kaynak grubu**: Digital Twins örneği için bir [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) seçin veya oluşturun.
   * **Konum**: Cihazlarınıza en yakın konumu seçin.

     [girilen bilgilerle dijital TWINS bölmesi ![](./media/create-digital-twins-portal/create-digital-twins-param.png)](./media/create-digital-twins-portal/create-digital-twins-param.png#lightbox)

1. Dijital TWINS bilgilerinizi gözden geçirin ve ardından **Oluştur**' u seçin. Dijital TWINS örneğinizin oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden izleyebilirsiniz.

1. Digital Twins örneğinizin **Genel Bakış** bölmesini açın. **Yönetim API 'si**altındaki bağlantıyı aklınızda edin.

   **YÖNETIM API** URL 'si `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger`olarak biçimlendirilir. Bu URL, örneğiniz için geçerli olan Azure Digital Twins REST API belgesini açar. Bu API belgelerini okumayı ve kullanmayı öğrenmek için bkz. [Azure Digital Twins Swagger'ı kullanma](../articles/digital-twins/how-to-use-swagger.md).

    **YÖNETIM API** 'si URL 'sini şu biçimde kopyalayın ve değiştirin: `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. Uygulamanız, değiştirilen URL'yi örneğinize erişmek için temel URL olarak kullanır. Değiştirdiğiniz URL'yi geçici bir dosyaya kopyalayın. Bu, sonraki bölümde gerekecektir.

    [![yönetim API 'SI](./media/create-digital-twins-portal/digital-twins-management-api.png)](./media/create-digital-twins-portal/digital-twins-management-api.png#lightbox)