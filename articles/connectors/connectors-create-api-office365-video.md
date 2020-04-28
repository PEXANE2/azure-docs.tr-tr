---
title: Office 365 video 'ya bağlanma
description: Azure Logic Apps kullanarak Office 365 videolarında videoları yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 05/18/2016
tags: connectors
ms.openlocfilehash: 8ac6b7b411e7f42dd076c5b16e7b500a819c617f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75665794"
---
# <a name="manage-videos-in-office365-video-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak videoları Office365 videolarında yönetme

Office 365 videosu hakkında bilgi almak, videoların listesini almak ve daha fazlası için Office 365 video 'ya bağlanın. Office 365 videosu ile şunları yapabilirsiniz:

* Office 365 videolarından aldığınız verilere göre iş akışınızı oluşturun. 

* Video portalı durumunu kontrol eden eylemleri kullanın, bir kanaldaki tüm videonun bir listesini alın ve daha fazlasını yapın. Bu eylemler bir yanıt alır ve daha sonra çıktıyı diğer eylemler için kullanılabilir hale getirir. 

Örneğin, Office 365 videolarını aramak için Bing Arama bağlayıcısını kullanabilir ve ardından bu video hakkında bilgi almak için Office 365 video bağlayıcısını kullanabilirsiniz. Video gereksinimlerinizi karşılıyorsa, bu videoyu Facebook 'ta gönderebilirsiniz.

Şimdi bir mantıksal uygulama oluşturarak çalışmaya başlamak için bkz. [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-office365-video"></a>Office365 video 'ya bağlanma

Bu bağlayıcıyı Logic Apps 'e eklediğinizde, Office 365 video hesabınızda oturum açmanız ve mantıksal uygulamaların hesabınıza bağlanmasına izin vermeniz gerekir.

> [!INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]

Bağlantıyı oluşturduktan sonra, kiracı adı veya kanal KIMLIĞI gibi Office 365 video özelliklerini girersiniz. 

## <a name="connector-specific-details"></a>Bağlayıcıya özgü ayrıntılar

Swagger 'da tanımlanan Tetikleyicileri ve eylemleri görüntüleyin ve ayrıca [bağlayıcı ayrıntılarında](/connectors/office365videoconnector/)tüm limitleri inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin