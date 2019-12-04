---
title: GitHub 'a Bağlan
description: GitHub REST API 'Leri ve Azure Logic Apps GitHub olaylarını izleme
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/02/2018
tags: connectors
ms.openlocfilehash: 7dc865f520b6f4667ace720e656a210e0252d1a1
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789765"
---
# <a name="connect-to-github-from-azure-logic-apps"></a>Azure Logic Apps GitHub 'a bağlanma

GitHub, git ve diğer özelliklerde tüm dağıtılmış düzeltme denetimi ve kaynak kodu yönetimi (SCM) işlevlerini sunan Web tabanlı bir git deposu barındırma hizmetidir.

GitHub bağlayıcısını kullanmaya başlamak için [önce bir mantıksal uygulama oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>GitHub bağlantısı oluşturma

Bir mantıksal uygulamada GitHub bağlayıcısını kullanmak için, önce bir *bağlantı* oluşturmanız ve ardından bu özelliklerin ayrıntılarını sağlamanız gerekir: 

| Özellik | Gereklidir | Açıklama | 
| -------- | -------- | ----------- | 
| Belirteç | Yes | GitHub kimlik bilgilerinizi sağlayın. |

Bağlantıyı oluşturduktan sonra, eylemleri yürütebilir ve bu makalede açıklanan Tetikleyicileri dinleyebilirsiniz.

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının Openapı (eski adıyla Swagger) açıklaması tarafından tanımlanan Tetikleyiciler, Eylemler ve limitlerle ilgili teknik ayrıntılar için [bağlayıcının başvuru sayfasını](/connectors/github/)gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin