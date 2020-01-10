---
title: Office 365 kullanıcılarına bağlanma
description: Azure Logic Apps kullanarak Office 365 kullanıcı profillerinde profilleri alan ve yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.date: 08/18/2016
ms.topic: article
tags: connectors
ms.openlocfilehash: 179c7538d8072f0e1a4f7d049cf521971efd7a21
ms.sourcegitcommit: ff9688050000593146b509a5da18fbf64e24fbeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75666865"
---
# <a name="get-and-manage-profiles-in-office-365-users-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak Office 365 kullanıcıları 'nda profilleri edinme ve yönetme

Profilleri almak, kullanıcıları aramak ve daha fazlasını yapmak için Office 365 kullanıcılarına bağlanın. Office 365 kullanıcıları ile şunları yapabilirsiniz:

* İş akışınızı Office 365 kullanıcılarından aldığınız verilere göre oluşturun. 
* Doğrudan rapor alan eylemleri, yöneticinin kullanıcı profilini al ve daha fazlasını kullanın. Bu eylemler bir yanıt alır ve daha sonra çıktıyı diğer eylemler için kullanılabilir hale getirir. Örneğin, bir kişinin doğrudan raporlarını alın ve bu bilgileri alıp SQL Azure bir veritabanını güncelleştirin. 

Şimdi bir mantıksal uygulama oluşturarak çalışmaya başlamak için bkz. [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-office-365-users"></a>Office 365 kullanıcılarına bağlantı oluşturma

Bu bağlayıcıyı Logic Apps 'e eklediğinizde, Azure Logic Apps hesabınıza bağlanabilmesi için Office 365 kullanıcıları hesabınızda oturum açmanız gerekir.

> [!INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]
> 
> 

Bağlantıyı oluşturduktan sonra, Kullanıcı KIMLIĞI gibi Office 365 kullanıcıları özelliklerini girersiniz. Bu makaledeki **REST API başvurusu** bu özellikleri açıklamaktadır.

## <a name="connector-specific-details"></a>Bağlayıcıya özgü ayrıntılar

Bağlayıcının Swagger açıklaması tarafından tanımlanan Tetikleyiciler, Eylemler ve limitlerle ilgili teknik ayrıntılar için [bağlayıcının başvuru sayfasını](/connectors/officeusers/)gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](apis-list.md) hakkında bilgi edinin