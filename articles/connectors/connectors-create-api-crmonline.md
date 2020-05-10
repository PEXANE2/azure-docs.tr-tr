---
title: Dynamics 365 'e bağlanma
description: Azure Logic Apps kullanarak Dynamics 365 kayıtları oluşturma ve yönetme
services: logic-apps
ms.suite: integration
ms.reviewer: jdaly, logicappspm
ms.topic: conceptual
ms.date: 05/09/2020
tags: connectors
ms.openlocfilehash: 00bf8ea2b783e09711a95f203bdfcce0e6b90b2c
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82994323"
---
# <a name="create-and-manage-records-in-dynamics-365-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak Dynamics 365 ' de kayıt oluşturma ve yönetme

Dynamics 365 [Common Data Service](https://docs.microsoft.com/powerapps/maker/common-data-service/data-platform-intro)kullanır. Dynamics 365 bağlantılarında [Common Data Service bağlayıcısını](https://docs.microsoft.com/connectors/commondataservice/)kullanın.

> [!IMPORTANT]
> [Dynamics 365 Bağlayıcısı](https://docs.microsoft.com/connectors/dynamicscrmonline/) kullanım dışıdır, ancak kaldırılana kadar çalışmaya devam eder. Yeni Logic Apps için Dynamics 365 bağlayıcısını kullanmayın. Dynamics 365 bağlayıcısının kaldırılması için zaman çizelgesi henüz duyurulmadı. Mevcut mantıksal uygulamaları Common Data Service bağlayıcıya veya diğer planlı yeni bağlayıcıya dönüştürmeniz gerekmez, ancak bağlayıcı kaldırıldığında mantıksal uygulamalarınızı dönüştürmeniz gerekir. Daha fazla bilgi için bkz. [Dynamics 365 Bağlayıcısı kullanım dışıdır](https://docs.microsoft.com/power-platform/important-changes-coming).
>
> [Common Data Service Bağlayıcısı](https://docs.microsoft.com/connectors/commondataservice/) kullanım dışı Dynamics 365 Bağlayıcısı ile aynı özellikleri sağlar, ancak güvenilirliği artıran iyileştirmeler içerir. Logic Apps 'te Common Data Service bağlayıcısını kullanma hakkında daha fazla bilgi için, bkz. [Azure Logic Apps kullanarak Common Data Service kayıtları oluşturma ve yönetme](../connectors/connect-common-data-service.md).

Common Data Service hakkında daha fazla bilgi için şu konulara bakın:

* [Öğrenin: Common Data Service kullanmaya başlayın](https://docs.microsoft.com/learn/modules/get-started-with-powerapps-common-data-service/)
* [Öğrenin: Power platform ve Common Data Service kullanarak Dynamics 365 verilerinizi bağlama ve çözümleme](https://docs.microsoft.com/learn/wwl/connect-analyze-dynamics-365-data/)