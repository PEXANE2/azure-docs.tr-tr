---
title: Office 365 Videosuna Bağlan
description: Azure Mantık Uygulamalarını kullanarak Office 365 Video'da videoları yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 05/18/2016
tags: connectors
ms.openlocfilehash: 8ac6b7b411e7f42dd076c5b16e7b500a819c617f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75665794"
---
# <a name="manage-videos-in-office365-video-by-using-azure-logic-apps"></a>Azure Mantık Uygulamalarını kullanarak Office365 Video'da videoları yönetme

Office 365 videosu hakkında bilgi almak, video ların listesini almak ve daha fazlası için Office 365 Video'ya bağlanın. Office 365 Video ile şunları yapabilirsiniz:

* İş akışınızı Office 365 Video'dan aldığınız verilere göre oluşturun. 

* Video portalı durumunu kontrol eden, bir kanaldaki tüm videoların listesini ve daha fazlasını alan eylemleri kullanın. Bu eylemler bir yanıt alır ve çıktıyı diğer eylemler için kullanılabilir hale getirin. 

Örneğin, Office 365 videolarını aramak için Bing Arama bağlayıcısını kullanabilir ve ardından bu video hakkında bilgi almak için Office 365 video konektörünü kullanabilirsiniz. Video gereksinimlerinizi karşılıyorsa, bu videoyu Facebook'ta yayınlayabilirsiniz.

Şimdi bir mantık uygulaması oluşturarak başlatabilirsiniz, [bkz.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="connect-to-office365-video"></a>Office365 Videosuna Bağlan

Bu bağlayıcıyı mantık uygulamalarınıza eklediğinizde, Office 365 Video hesabınızda oturum açmanız ve mantık uygulamalarının hesabınıza bağlanmasına izin vermeniz gerekir.

> [!INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]

Bağlantıyı oluşturduktan sonra, kiracı adı veya kanal kimliği gibi Office 365 video özelliklerini girersiniz. 

## <a name="connector-specific-details"></a>Bağlayıcıya özel ayrıntılar

Swagger'da tanımlanan tetikleyicileri ve eylemleri görüntüleyin ve [bağlayıcı ayrıntılarında](/connectors/office365videoconnector/)da herhangi bir sınır görün.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin