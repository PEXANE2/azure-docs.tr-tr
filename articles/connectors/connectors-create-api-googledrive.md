---
title: Google Drive 'a Bağlan
description: Azure Logic Apps kullanarak Google Drive için dosya oluşturup yöneten iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 11/07/2016
tags: connectors
ms.openlocfilehash: 155a579438747ca7de67eba6449b8a0191e86e2c
ms.sourcegitcommit: ff9688050000593146b509a5da18fbf64e24fbeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75666848"
---
# <a name="create-and-manage-files-for-google-drive-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak Google Drive için dosya oluşturma ve yönetme

Dosya oluşturmak, satırları almak ve daha fazlası için Google Drive 'a bağlanın. Google Drive ile şunları yapabilirsiniz: 

* Aramadan aldığınız verilere göre iş akışınızı oluşturun. 
* Görüntüleri aramak, haberleri aramak ve daha fazlasını yapmak için eylemleri kullanın. Bu eylemler bir yanıt alır ve daha sonra çıktıyı diğer eylemler için kullanılabilir hale getirir. Örneğin, bir video arayabilir ve ardından Twitter kullanarak bu videoyu Twitter akışına gönderebilirsiniz.

Şimdi bir mantıksal uygulama oluşturarak çalışmaya başlamak için bkz. [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-the-connection-to-google-drive"></a>Google Drive bağlantısını oluşturun

Bu bağlayıcıyı Logic Apps 'e eklediğinizde, Google Drive 'ınızla bağlantı kurmak için Logic Apps 'i yetkilendirmelisiniz.

> [!INCLUDE [Steps to create a connection to googledrive](../../includes/connectors-create-api-googledrive.md)]

Bağlantıyı oluşturduktan sonra, klasör yolu veya dosya adı gibi Google Drive özelliklerini girersiniz. 

## <a name="connector-specific-details"></a>Bağlayıcıya özgü ayrıntılar

Bağlayıcının Swagger açıklaması tarafından tanımlanan Tetikleyiciler, Eylemler ve limitlerle ilgili teknik ayrıntılar için [bağlayıcının başvuru sayfasını](/connectors/googledrive/)gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](apis-list.md) hakkında bilgi edinin
