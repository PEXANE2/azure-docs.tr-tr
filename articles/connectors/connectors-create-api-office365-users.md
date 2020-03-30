---
title: Office 365 Kullanıcılarına Bağlan
description: Azure Mantık Uygulamaları'nı kullanarak Office 365 Kullanıcıları profillerinde profil alan ve yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.date: 08/18/2016
ms.topic: article
tags: connectors
ms.openlocfilehash: 179c7538d8072f0e1a4f7d049cf521971efd7a21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75666865"
---
# <a name="get-and-manage-profiles-in-office-365-users-by-using-azure-logic-apps"></a>Azure Mantık Uygulamalarını kullanarak Office 365 Kullanıcıları'ndaki profilleri edinin ve yönetin

Profiller, arama kullanıcıları ve daha fazlasını almak için Office 365 Kullanıcılarına bağlanın. Office 365 Kullanıcıları ile şunları yapabilirsiniz:

* İş akışınızı Office 365 Kullanıcılarından aldığınız verilere göre oluşturun. 
* Doğrudan rapor alan, yöneticinin kullanıcı profilini ve daha fazlasını alan eylemleri kullanın. Bu eylemler bir yanıt alır ve çıktıyı diğer eylemler için kullanılabilir hale getirin. Örneğin, bir kişinin doğrudan raporlarını alın ve ardından bu bilgileri alın ve bir SQL Azure veritabanını güncelleştirin. 

Şimdi bir mantık uygulaması oluşturarak başlatabilirsiniz, [bkz.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="create-a-connection-to-office-365-users"></a>Office 365 Kullanıcıları ile bağlantı oluşturma

Bu bağlayıcıyı mantık uygulamalarınıza eklediğinizde, Azure Mantık Uygulamalarının hesabınıza bağlanabilmesi için Office 365 Kullanıcılar hesabınızda oturum açmanız gerekir.

> [!INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]
> 
> 

Bağlantıyı oluşturduktan sonra, kullanıcı kimliği gibi Office 365 Users özelliklerini girersiniz. Bu makaledeki **REST API başvurusu** bu özellikleri açıklar.

## <a name="connector-specific-details"></a>Bağlayıcıya özel ayrıntılar

Bağlayıcının Swagger açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için [bağlayıcının başvuru sayfasını](/connectors/officeusers/)inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](apis-list.md) hakkında bilgi edinin