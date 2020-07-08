---
title: dosya dahil etme
description: dosya dahil etme
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 05/04/2018
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 51b687dc2a6264eb12362616429746c9b182c3b1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "68323877"
---
> [!NOTE]
> Batch hesabı oluştururken iki *havuz ayırma* modu arasından seçim yapabilirsiniz: **kullanıcı aboneliği** ve **Batch hizmeti**. Çoğu durumda, havuzların Azure tarafından yönetilen aboneliklerde, arka planda ayrıldığı varsayılan Batch hizmeti modunu kullanmanız gerekir. Alternatif Kullanıcı aboneliği modunda, bir havuz oluşturulduğunda Batch VM'leri ve diğer kaynaklar doğrudan aboneliğinizde oluşturulur. [Azure Ayrılmış VM Örnekleri](https://azure.microsoft.com/pricing/reserved-vm-instances/) kullanarak Batch havuzları oluşturmak istiyorsanız Kullanıcı aboneliği modu gerekir. Kullanıcı aboneliği modunda bir Batch hesabı oluşturmak için aboneliğinizi Azure Batch hizmetine kaydetmeniz ve hesabı bir Azure Key Vault ile ilişkilendirmeniz de gerekir.