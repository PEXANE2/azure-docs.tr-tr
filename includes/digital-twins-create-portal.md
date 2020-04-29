---
title: include dosyası
description: include dosyası
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 01/07/2020
ms.custom: include file
ms.openlocfilehash: 9ccdc7a438d1dade534d39dc97a39a3bdae37dc4
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75895525"
---
1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Giriş yan çubuğunu seçin ve ardından **kaynak oluştur ' a**tıklayın. 

   [![Giriş yan çubuğunu genişletin ve + kaynak oluştur ' u seçin.](./media/create-digital-twins-portal/azure-portal-create-a-resource.png)](./media/create-digital-twins-portal/azure-portal-create-a-resource.png#lightbox)

1. **Dijital TWINS**araması yapın ve **dijital TWINS**' i seçin. 

   [![Yeni dijital TWINS örneği oluşturma seçimleri](./media/create-digital-twins-portal/azure-portal-create-digital-twins.png)](./media/create-digital-twins-portal/azure-portal-create-digital-twins.png#lightbox)

   Alternatif olarak, **nesnelerin interneti**öğesini seçin ve **dijital TWINS (Önizleme)** öğesini seçin.

1. Dağıtım işlemini başlatmak için **Oluştur**’u seçin.

   [![Kaynağın dağıtımını oluşturma ve onaylama](./media/create-digital-twins-portal/azure-create-and-confirm-resource.png)](./media/create-digital-twins-portal/azure-create-and-confirm-resource.png#lightbox)

1. **Digital Twins** bölmesine şu bilgileri girin:
   * **Kaynak Adı**: Digital Twins örneğiniz için benzersiz bir ad oluşturun.
   * **Abonelik**: Bu Digital Twins örneğini oluşturmak için kullanmak istediğiniz aboneliği seçin. 
   * **Kaynak grubu**: Digital Twins örneği için bir [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) seçin veya oluşturun.
   * **Konum**: Cihazlarınıza en yakın konumu seçin.

     [![Girilen bilgileri içeren dijital TWINS bölmesi](./media/create-digital-twins-portal/create-digital-twins-param.png)](./media/create-digital-twins-portal/create-digital-twins-param.png#lightbox)

1. Dijital TWINS bilgilerinizi gözden geçirin ve ardından **Oluştur**' u seçin. Dijital TWINS örneğinizin oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden izleyebilirsiniz.

1. Digital Twins örneğinizin **Genel Bakış** bölmesini açın. **Yönetim API 'si**altındaki bağlantıyı aklınızda edin. **YÖNETIM API** URL 'si şöyle biçimlendirilir: 
   
   ```URL
   https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger
   ```
   
   Bu URL, örneğiniz için geçerli olan Azure Digital Twins REST API belgesini açar. Bu API belgelerini okumayı ve kullanmayı öğrenmek için bkz. [Azure Digital Twins Swagger'ı kullanma](../articles/digital-twins/how-to-use-swagger.md). **YÖNETIM API** 'si URL 'sini şu biçimde kopyalayın ve değiştirin: 
    
   ```URL
   https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/
   ```
    
   Uygulamanız, değiştirilen URL'yi örneğinize erişmek için temel URL olarak kullanır. Değiştirdiğiniz URL'yi geçici bir dosyaya kopyalayın. Bu, sonraki bölümde gerekecektir.

   [![Yönetim API 'sine genel bakış](./media/create-digital-twins-portal/digital-twins-management-api.png)](./media/create-digital-twins-portal/digital-twins-management-api.png#lightbox)