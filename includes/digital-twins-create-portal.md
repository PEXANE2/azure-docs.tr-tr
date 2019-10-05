---
title: içerme dosyası
description: içerme dosyası
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 09/24/2019
ms.custom: include file
ms.openlocfilehash: 7b2df437833f270a6e102257693426f4cc65b9d2
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949774"
---
1. [Azure Portal](https://portal.azure.com)oturum açın.

1. Sol bölmeden **kaynak oluştur**' u seçin. **Dijital TWINS**araması yapın ve **dijital TWINS**' i seçin. Dağıtım işlemini başlatmak için **Oluştur** ' u seçin.

   [![ yeni bir dijital TWINS örneği oluşturmaya yönelik seçimler](./media/create-digital-twins-portal/create-digital-twins.png)](./media/create-digital-twins-portal/create-digital-twins.png#lightbox)

1. **Dijital TWINS** bölmesinde aşağıdaki bilgileri girin:
   * **Kaynak adı**: dijital TWINS örneğiniz için benzersiz bir ad oluşturun.
   * **Abonelik**: Bu dijital TWINS örneğini oluşturmak için kullanmak istediğiniz aboneliği seçin. 
   * **Kaynak grubu**: dijital TWINS örneği için bir [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) seçin veya oluşturun.
   * **Konum**: cihazlarınızın en yakın konumunu seçin.

     [![Dijital TWINS bölmesi, girilen bilgilerle birlikte](./media/create-digital-twins-portal/create-digital-twins-param.png)](./media/create-digital-twins-portal/create-digital-twins-param.png#lightbox)

1. Dijital TWINS bilgilerinizi gözden geçirin ve ardından **Oluştur**' u seçin. Dijital TWINS örneğinizin oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinde izleyebilirsiniz.

1. Dijital TWINS örneğinizin **genel bakış** bölmesini açın. **Yönetim API 'si**altındaki bağlantıyı aklınızda edin.

   **YÖNETIM API** URL 'si `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger` olarak biçimlendirilir. Bu URL sizi, örneğiniz için geçerli olan Azure dijital TWINS REST API belgelerine götürür. Bu API belgelerini okumayı ve kullanmayı öğrenmek için [Azure Digital TWINS Swagger 'Yi nasıl kullanacağınızı](../articles/digital-twins/how-to-use-swagger.md) okuyun.

    **YÖNETIM API** 'si URL 'sini bu biçimle değiştirin `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. Uygulamanız, örneğinize erişmek için temel URL olarak değiştirilen URL 'yi kullanacaktır. Bu değiştirilmiş URL 'YI geçici bir dosyaya kopyalayın. Bu, sonraki bölümde gerekecektir.

    [![Yönetim API 'SI](./media/create-digital-twins-portal/digital-twins-management-api.png)](./media/create-digital-twins-portal/digital-twins-management-api.png#lightbox)