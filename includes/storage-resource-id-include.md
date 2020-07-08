---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/15/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 39b61c78cbd0333183c80234256d6041a109d846
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "68249936"
---
Bir Azure AD kaynak KIMLIĞI, bir Azure kaynağına erişim sağlamak için verilen belirtecin kullanılabileceği hedef kitlelerinin olduğunu gösterir. Azure depolama söz konusu olduğunda, kaynak KIMLIĞI tek bir depolama hesabına özgü olabilir veya herhangi bir depolama hesabı için de uygulanabilir. Aşağıdaki tabloda kaynak KIMLIĞI için sağlayabileceğinizi belirten değerler açıklanmaktadır:

|Kaynak kimliği  |Açıklama  |
|---------|---------|
|`https://<account>.blob.core.windows.net` <br /><br /> `https://<account>.queue.core.windows.net`    | Belirli bir depolama hesabı için hizmet uç noktası. İstekleri yalnızca ilgili Azure depolama hesabına ve hizmete yetkilendirmek için bir belirteç almak üzere bu değeri kullanın. Köşeli ayraçlar içindeki değeri depolama hesabınızın adıyla değiştirin.      |
|`https://storage.azure.com/`     | İstekleri herhangi bir Azure depolama hesabına yetkilendirmek için bir belirteç almak üzere kullanın.        |
