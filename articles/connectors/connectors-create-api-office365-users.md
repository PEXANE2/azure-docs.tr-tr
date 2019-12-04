---
title: Office 365 kullanıcılarına bağlanma
description: Office 365 kullanıcıları REST API 'Leri ve Azure Logic Apps kullanıcı profillerini yönetme
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.date: 08/18/2016
ms.topic: article
tags: connectors
ms.openlocfilehash: addb64a9b43c51af8363caa6f0fb3261a618e893
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789537"
---
# <a name="get-started-with-the-office-365-users-connector"></a>Office 365 kullanıcıları bağlayıcısını kullanmaya başlama
Profilleri almak, kullanıcıları aramak ve daha fazlasını yapmak için Office 365 kullanıcılarına bağlanın. Office 365 kullanıcıları ile şunları yapabilirsiniz:

* İş akışınızı Office 365 kullanıcılarından aldığınız verilere göre oluşturun. 
* Doğrudan rapor alan eylemleri, yöneticinin kullanıcı profilini al ve daha fazlasını kullanın. Bu eylemler bir yanıt alır ve daha sonra çıktıyı diğer eylemler için kullanılabilir hale getirir. Örneğin, bir kişinin doğrudan raporlarını alın ve bu bilgileri alıp SQL Azure bir veritabanını güncelleştirin. 

Şimdi bir mantıksal uygulama oluşturarak çalışmaya başlamak için bkz. [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-office-365-users"></a>Office 365 kullanıcılarına bağlantı oluşturma
Bu bağlayıcıyı Logic Apps 'e eklediğinizde, Office 365 kullanıcıları hesabınızda oturum açmanız ve mantıksal uygulamaların hesabınıza bağlanmasına izin vermeniz gerekir.

> [!INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]
> 
> 

Bağlantıyı oluşturduktan sonra, Kullanıcı KIMLIĞI gibi Office 365 kullanıcıları özelliklerini girersiniz. Bu makaledeki **REST API başvurusu** bu özellikleri açıklamaktadır.

## <a name="connector-specific-details"></a>Bağlayıcıya özgü ayrıntılar

Swagger 'da tanımlanan Tetikleyicileri ve eylemleri görüntüleyin ve ayrıca [bağlayıcı ayrıntılarında](/connectors/officeusers/)tüm limitleri inceleyin.

## <a name="more-connectors"></a>Daha fazla bağlayıcı
[API listesine](apis-list.md)geri dönün.