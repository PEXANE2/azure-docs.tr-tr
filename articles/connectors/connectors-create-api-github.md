---
title: GitHub repo'nuza erişin, izleyin ve yönetin
description: Azure Logic Apps ile otomatik iş akışları oluşturarak GitHub etkinliklerini izleyin ve GitHub reponuzu yönetin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/02/2018
tags: connectors
ms.openlocfilehash: b67d2e860e8c69f6f2f43441144ef2f60ed3a104
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75378458"
---
# <a name="monitor-and-manage-your-github-repo-by-using-azure-logic-apps"></a>Azure Logic Apps'ı kullanarak GitHub reponuzu izleyin ve yönetin

GitHub, Git'deki tüm dağıtılmış revizyon denetimi ve kaynak kodu yönetimi (SCM) işlevselliğini ve diğer özellikleri sunan web tabanlı bir Git deposu barındırma hizmetidir.

GitHub konektörünü almaya başlamak için [önce bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="create-a-connection-to-github"></a>GitHub bağlantısı oluşturma

Bir mantık uygulamasında GitHub bağlayıcısını kullanmak için önce bir *bağlantı* oluşturmanız ve ardından bu özellikler için ayrıntılar sağlamanız gerekir: 

| Özellik | Gerekli | Açıklama | 
| -------- | -------- | ----------- | 
| Belirteç | Evet | GitHub kimlik bilgilerinizi sağlayın. |

Bağlantıyı oluşturduktan sonra, eylemleri yürütebilir ve bu makalede açıklanan tetikleyicileri dinleyebilirsiniz.

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için [bağlayıcının başvuru sayfasını](/connectors/github/)inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin