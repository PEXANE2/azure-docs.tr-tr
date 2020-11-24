---
title: include dosyası
description: include dosyası
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 08/24/2020
ms.openlocfilehash: 70b636b7bb508b71475a7464983b091d5d10e0e1
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95555935"
---
 Azure Machine Learning işlem kümeleri, kodunuzun kimlik bilgilerini eklemeden Azure kaynaklarına erişimin kimliğini doğrulamak için de [yönetilen kimlikleri](../articles/active-directory/managed-identities-azure-resources/overview.md) destekler. İki tür yönetilen kimlik vardır:

* **Sistem tarafından atanan yönetilen kimlik** Azure Machine Learning işlem kümesi üzerinde doğrudan etkinleştirilir. Sistem tarafından atanan kimliğin yaşam döngüsü doğrudan işlem kümesine bağlıdır. İşlem kümesi silinirse Azure, kimlik bilgilerini ve kimlik bilgilerini Azure AD 'de otomatik olarak temizler.
* **Kullanıcı tarafından atanan yönetilen kimlik** , Azure yönetilen kimlik hizmeti aracılığıyla sunulan tek başına bir Azure kaynağıdır. Kullanıcı tarafından atanan bir yönetilen kimliği birden fazla kaynağa atayabilir ve istediğiniz sürece devam edebilirsiniz.