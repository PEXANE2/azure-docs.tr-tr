---
title: dosya dahil etme
description: dosya dahil etme
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 08/24/2020
ms.openlocfilehash: d8ee7b61897a9718d7078fc16eddc177fefbff43
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89147603"
---
 Azure Machine Learning işlem kümeleri, kodunuzun kimlik bilgilerini eklemeden Azure kaynaklarına erişimin kimliğini doğrulamak için de [yönetilen kimlikleri](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) destekler. İki tür yönetilen kimlik vardır:

* **Sistem tarafından atanan yönetilen kimlik** Azure Machine Learning işlem kümesi üzerinde doğrudan etkinleştirilir. Sistem tarafından atanan kimliğin yaşam döngüsü doğrudan işlem kümesine bağlıdır. İşlem kümesi silinirse Azure, kimlik bilgilerini ve kimlik bilgilerini Azure AD 'de otomatik olarak temizler.
* **Kullanıcı tarafından atanan yönetilen kimlik** , Azure yönetilen kimlik hizmeti aracılığıyla sunulan tek başına bir Azure kaynağıdır. Kullanıcı tarafından atanan bir yönetilen kimliği birden fazla kaynağa atayabilir ve istediğiniz sürece devam edebilirsiniz.