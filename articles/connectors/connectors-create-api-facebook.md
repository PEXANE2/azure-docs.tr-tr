---
title: Facebook'a bağlan
description: Azure Mantık Uygulamaları'nı kullanarak Facebook zaman tünelinizi ve sayfanızı yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 11/07/2016
tags: connectors
ms.openlocfilehash: 803453291b5cab2c51fec6641f8b096f62336325
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75665811"
---
# <a name="manage-your-facebook-timeline-and-page-by-using-azure-logic-apps"></a>Azure Mantık Uygulamalarını kullanarak Facebook zaman çizelgenizi ve sayfanızı yönetme

Facebook'a bağlanın ve bir zaman çizelgesine gönderin, bir sayfa akışı ve daha fazlasını alın. Facebook ile şunları yapabilirsiniz:

* İş akışınızı Facebook'tan aldığınız verilere göre oluşturun. 
* Yeni bir gönderi aldığında tetikleyici kullanın.
* Zaman tünelinize gönderen eylemleri kullanın, sayfa akışı alın ve daha fazlasını alın. Bu eylemler bir yanıt alır ve çıktıyı diğer eylemler için kullanılabilir hale getirin. Örneğin, zaman tünelinizde yeni bir gönderi olduğunda, bu gönderiyi alıp Twitter akışınıza itebilirsiniz. 

Şimdi bir mantık uygulaması oluşturarak başlatabilirsiniz, [bkz.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="create-a-connection-to-facebook"></a>Facebook'a bağlantı oluşturma

Bu bağlayıcıyı mantık uygulamalarınıza eklediğinizde, Facebook'unuza bağlanmak için mantık uygulamalarını yetkilendirmeniz gerekir.

1. Facebook hesabınızda oturum açın.

2. **Yetkilendirme'yi**seçin ve mantık uygulamalarınızın Facebook'unuzu bağlamasına ve kullanmasına izin verin. 

> [!INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]
> 

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) dosyasında açıklandığı gibi tetikleyiciler, eylemler ve sınırlar gibi teknik ayrıntılar için [bağlayıcının başvuru sayfasına](/connectors/facebook/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin