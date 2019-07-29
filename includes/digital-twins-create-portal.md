---
title: include dosyası
description: include dosyası
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: include
ms.date: 07/26/2019
ms.author: dkshir
ms.custom: include file
ms.openlocfilehash: de81700496089ffc6cfdba8e02eaa4e14200d16d
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68601393"
---
1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol bölmeden **kaynak oluştur**' u seçin. **Dijital TWINS**araması yapın ve **dijital TWINS**' i seçin. Dağıtım işlemini başlatmak için **Oluştur** ' u seçin.

   ![Yeni dijital TWINS örneği oluşturma seçimleri](./media/create-digital-twins-portal/create-digital-twins.png)

1. **Digital Twins** bölmesine şu bilgileri girin:
   * **Kaynak adı**: Dijital TWINS örneğiniz için benzersiz bir ad oluşturun.
   * **Abonelik**: Bu dijital TWINS örneğini oluşturmak için kullanmak istediğiniz aboneliği seçin. 
   * **Kaynak grubu**: Dijital TWINS örneği için bir [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) seçin veya oluşturun.
   * **Konum**: Cihazlarınız için en yakın konumu seçin.

     ![Girilen bilgileri içeren dijital TWINS bölmesi](./media/create-digital-twins-portal/create-digital-twins-param.png)

1. Dijital TWINS bilgilerinizi gözden geçirin ve ardından **Oluştur**' u seçin. Dijital TWINS örneğinizin oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden izleyebilirsiniz.

1. Digital Twins örneğinizin **Genel Bakış** bölmesini açın. **Yönetim API 'si**altındaki bağlantıyı aklınızda edin.

   **YÖNETIM API** URL 'si olarak `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger`biçimlendirilir. Bu URL, örneğiniz için geçerli olan Azure Digital Twins REST API belgesini açar. Bu API belgelerini okumayı ve kullanmayı öğrenmek için bkz. [Azure Digital Twins Swagger'ı kullanma](../articles/digital-twins/how-to-use-swagger.md).

    **YÖNETIM API** 'sini bu biçimde `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`değiştirin. Uygulamanız, değiştirilen URL'yi örneğinize erişmek için temel URL olarak kullanır. Değiştirdiğiniz URL'yi geçici bir dosyaya kopyalayın. Bu, sonraki bölümde gerekecektir.

    ![Yönetim API'si](./media/create-digital-twins-portal/digital-twins-management-api.png)