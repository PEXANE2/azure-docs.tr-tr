---
title: Dynamics 365 'e bağlanma
description: Azure Logic Apps kullanarak Dynamics 365 kayıtları oluşturma ve yönetme
services: logic-apps
ms.suite: integration
ms.reviewer: jdaly, logicappspm
ms.topic: conceptual
ms.date: 05/09/2020
tags: connectors
ms.openlocfilehash: fe1e31ce0bc842e1da18327d12e7a52562d84b53
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87284039"
---
# <a name="create-and-manage-records-in-dynamics-365-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak Dynamics 365 ' de kayıt oluşturma ve yönetme

Dynamics 365 [Common Data Service](/powerapps/maker/common-data-service/data-platform-intro)kullanır. Dynamics 365 bağlantılarında [Common Data Service bağlayıcısını](/connectors/commondataservice/)kullanın.

> [!IMPORTANT]
> [Dynamics 365 Bağlayıcısı](/connectors/dynamicscrmonline/) kullanım dışıdır, ancak kaldırılana kadar çalışmaya devam eder. Yeni Logic Apps için Dynamics 365 bağlayıcısını kullanmayın. Dynamics 365 bağlayıcısının kaldırılması için zaman çizelgesi henüz duyurulmadı. Var olan mantıksal uygulamaları Common Data Service bağlayıcısına veya planlanan diğer yeni bağlayıcılarına dönüştürmenize gerek yoktur ancak bağlayıcı kaldırıldığında mantıksal uygulamalarınızı dönüştürmeniz gerekecektir. Daha fazla bilgi için bkz. [Dynamics 365 Bağlayıcısı kullanım dışıdır](/power-platform/important-changes-coming).
>
> [Common Data Service bağlayıcısı](/connectors/commondataservice/), kullanım dışı bırakılan Dynamics 365 bağlayıcısıyla aynı özelliklere sahiptir ancak güvenilirliği artıran iyileştirmeler yapılmıştır. Common Data Service bağlayıcısını mantıksal uygulamalarda kullanma hakkında daha fazla bilgi için bkz. [Azure Logic Apps'i kullanarak Common Data Service kaydı oluşturma ve yönetme](../connectors/connect-common-data-service.md).

Common Data Service hakkında daha fazla bilgi için şu konulara bakın:

* [Öğrenin: Common Data Service kullanmaya başlayın](/learn/modules/get-started-with-powerapps-common-data-service/)
* [Öğrenin: Power platform ve Common Data Service kullanarak Dynamics 365 verilerinizi bağlama ve çözümleme](/learn/wwl/connect-analyze-dynamics-365-data/)
