---
title: Outlook.com 'e bağlanma
description: Outlook.com REST API 'Leri ile e-posta, takvim ve kişileri yönetme ve Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/18/2016
tags: connectors
ms.openlocfilehash: 750efc2cb928bf127c4f3c68d5a58c5f52ca7d51
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789383"
---
# <a name="manage-email-calendars-and-contacts-in-outlookcom-with-azure-logic-apps"></a>Azure Logic Apps ile e-posta, takvim ve kişileri Outlook.com içinde yönetme

Bu makalede, Outlook.com hesabınızı Box Bağlayıcısı ile bir mantıksal uygulama içinde nasıl oluşturabileceğiniz ve yönetebileceğiniz gösterilmektedir. Bu şekilde, Outlook.com hesabınız için görevleri ve iş akışlarını otomatikleştiren mantıksal uygulamalar oluşturabilirsiniz, örneğin:

* E-posta gönderin. 
* Toplantıları zamanlayın.
* Kişi ekle. 

Logic Apps 'e yeni başladıysanız [Azure Logic Apps ne olduğunu](../logic-apps/logic-apps-overview.md)gözden geçirin.

## <a name="prerequisites"></a>Önkoşullar

* Bir [Outlook.com hesabı](https://outlook.live.com/owa/)

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Outlook.com hesabınıza erişmek istediğiniz mantıksal uygulama. Mantıksal uygulamanızı bir Outlook tetikleyicisiyle başlatmak için [boş bir mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md)gerekir. 

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)hakkında temel bilgi.

## <a name="connect-to-outlookcom"></a>Outlook.com 'e bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Connect to Outlook.com](../../includes/connectors-create-api-outlook.md)]

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının Swagger dosyasında açıklandığı şekilde Tetikleyiciler, Eylemler ve sınırlar gibi teknik ayrıntılar için [bağlayıcının başvuru sayfasına](/connectors/outlook/)bakın. 

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin