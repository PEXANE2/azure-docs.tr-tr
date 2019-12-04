---
title: Facebook 'a bağlanma
description: Facebook REST API 'Leri ve Azure Logic Apps zaman çizelgenizi ve sayfanızı yönetin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 11/07/2016
tags: connectors
ms.openlocfilehash: 1aa936f902dc17c9a401959c19824f6c581547b1
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789828"
---
# <a name="manage-your-facebook-timeline-and-page-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak Facebook zaman çizelgenizi ve sayfanızı yönetme

Facebook 'a bağlanın ve bir zaman çizelgesinde gönderi yapın, sayfa akışı alın ve daha fazlasını yapın. Facebook ile şunları yapabilirsiniz:

* Facebook 'tan aldığınız verilere göre iş akışınızı oluşturun. 
* Yeni bir gönderi alındığında bir tetikleyici kullanın.
* Zaman çizelgenize gönderi yapan eylemleri kullanın, sayfa akışı alın ve daha fazlasını yapın. Bu eylemler bir yanıt alır ve daha sonra çıktıyı diğer eylemler için kullanılabilir hale getirir. Örneğin, zaman çizelgenizde yeni bir gönderi olduğunda, bu gönderiyi alıp Twitter akışınıza gönderebilirsiniz. 

Şimdi bir mantıksal uygulama oluşturarak çalışmaya başlamak için bkz. [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-facebook"></a>Facebook bağlantısı oluşturma

Bu bağlayıcıyı Logic Apps 'e eklediğinizde, Facebook 'ınızla bağlantı kurmak için Logic Apps 'i yetkilendirmelisiniz.

1. Facebook hesabınızda oturum açın.

2. **Yetkilendir**' i seçin ve mantıksal uygulamalarınızın Facebook 'a bağlanıp bunları kullanmasına izin verin. 

> [!INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]
> 

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının Openapı (eski adıyla Swagger) dosyasında açıklandığı gibi Tetikleyiciler, Eylemler ve sınırlar gibi teknik ayrıntılar için [bağlayıcının başvuru sayfasına](/connectors/facebook/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin