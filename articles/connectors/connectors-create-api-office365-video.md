---
title: Office 365 video 'ya bağlanma
description: Videoları Office 365 video REST API 'Leri ve Azure Logic Apps yönetme
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 05/18/2016
tags: connectors
ms.openlocfilehash: bf3c7c4faac3fcd4dd0c3f368fbe18a224ed591b
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789502"
---
# <a name="get-started-with-the-office365-video-connector"></a>Office365 video bağlayıcısını kullanmaya başlama

Office 365 videosu hakkında bilgi almak, videoların listesini almak ve daha fazlası için Office 365 video 'ya bağlanın. Office 365 videosu ile şunları yapabilirsiniz:

* Office 365 videolarından aldığınız verilere göre iş akışınızı oluşturun. 

* Video portalı durumunu kontrol eden eylemleri kullanın, bir kanaldaki tüm videonun bir listesini alın ve daha fazlasını yapın. Bu eylemler bir yanıt alır ve daha sonra çıktıyı diğer eylemler için kullanılabilir hale getirir. 

Örneğin, Office 365 videolarını aramak için Bing Arama bağlayıcısını kullanabilir ve ardından bu video hakkında bilgi almak için Office 365 video bağlayıcısını kullanabilirsiniz. Video gereksinimlerinizi karşılıyorsa, bu videoyu Facebook 'ta gönderebilirsiniz.

Şimdi bir mantıksal uygulama oluşturarak çalışmaya başlamak için bkz. [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-office365-video-connector"></a>Office365 video Bağlayıcısı bağlantısı oluşturma

Bu bağlayıcıyı Logic Apps 'e eklediğinizde, Office 365 video hesabınızda oturum açmanız ve mantıksal uygulamaların hesabınıza bağlanmasına izin vermeniz gerekir.

> [!INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]

Bağlantıyı oluşturduktan sonra, kiracı adı veya kanal KIMLIĞI gibi Office 365 video özelliklerini girersiniz. 

## <a name="connector-specific-details"></a>Bağlayıcıya özgü ayrıntılar

Swagger 'da tanımlanan Tetikleyicileri ve eylemleri görüntüleyin ve ayrıca [bağlayıcı ayrıntılarında](/connectors/office365videoconnector/)tüm limitleri inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin