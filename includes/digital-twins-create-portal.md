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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895525"
---
1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Ana sayfa yan çubuğunu seçin, ardından **+ Kaynak oluşturun.** 

   [![Ana sayfa yan çubuğunu genişletin, ardından + Kaynak Oluştur'u seçin](./media/create-digital-twins-portal/azure-portal-create-a-resource.png)](./media/create-digital-twins-portal/azure-portal-create-a-resource.png#lightbox)

1. Dijital **İkizler**için arama ve **Dijital İkizler**seçin. 

   [![Yeni bir Dijital İkizler örneği oluşturmak için seçimler](./media/create-digital-twins-portal/azure-portal-create-digital-twins.png)](./media/create-digital-twins-portal/azure-portal-create-digital-twins.png#lightbox)

   Alternatif olarak, **Nesnelerin İnterneti'ni**seçin ve **Digital Twins'i (önizleme)** seçin.

1. Dağıtım işlemini başlatmak için **Oluştur**’u seçin.

   [![Kaynağın dağıtımını oluşturma ve onaylama](./media/create-digital-twins-portal/azure-create-and-confirm-resource.png)](./media/create-digital-twins-portal/azure-create-and-confirm-resource.png#lightbox)

1. **Digital Twins** bölmesine şu bilgileri girin:
   * **Kaynak Adı**: Digital Twins örneğiniz için benzersiz bir ad oluşturun.
   * **Abonelik**: Bu Digital Twins örneğini oluşturmak için kullanmak istediğiniz aboneliği seçin. 
   * **Kaynak grubu**: Digital Twins örneği için bir [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) seçin veya oluşturun.
   * **Konum**: Cihazlarınıza en yakın konumu seçin.

     [![Girilen bilgilerle Dijital İkizler bölmesi](./media/create-digital-twins-portal/create-digital-twins-param.png)](./media/create-digital-twins-portal/create-digital-twins-param.png#lightbox)

1. Dijital İkizler bilgilerinizi gözden geçirin ve ardından **Oluştur'u**seçin. Dijital İkizler örneğinizin oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden izleyebilirsiniz.

1. Digital Twins örneğinizin **Genel Bakış** bölmesini açın. Yönetim API altında bağlantı not **edin.** **Yönetim API** URL'si aşağıdaki gibi biçimlendirilmiştir: 
   
   ```URL
   https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger
   ```
   
   Bu URL, örneğiniz için geçerli olan Azure Digital Twins REST API belgesini açar. Bu API belgelerini okumayı ve kullanmayı öğrenmek için bkz. [Azure Digital Twins Swagger'ı kullanma](../articles/digital-twins/how-to-use-swagger.md). **Yönetim API** URL'sini bu biçimde kopyalayın ve değiştirin: 
    
   ```URL
   https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/
   ```
    
   Uygulamanız, değiştirilen URL'yi örneğinize erişmek için temel URL olarak kullanır. Değiştirdiğiniz URL'yi geçici bir dosyaya kopyalayın. Bir sonraki bölümde buna ihtiyacın olacak.

   [![Yönetim API'si genel bakış](./media/create-digital-twins-portal/digital-twins-management-api.png)](./media/create-digital-twins-portal/digital-twins-management-api.png#lightbox)