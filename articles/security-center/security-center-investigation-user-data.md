---
title: Azure Güvenlik Merkezi soruşturmasında bulunan kullanıcı verilerini yönetme
description: " Azure Güvenlik Merkezi'nin araştırma özelliğinde bulunan kullanıcı verilerini nasıl yönetebilirsiniz öğrenin. "
services: operations-management-suite
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2018
ms.author: memildin
ms.openlocfilehash: aa262b0be3902f6e143a53f8f1302156fc5aede6
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80582976"
---
# <a name="manage-user-data-found-in-an-azure-security-center-investigation"></a>Azure Güvenlik Merkezi soruşturmasında bulunan kullanıcı verilerini yönetme
Bu makalede, Azure Güvenlik Merkezi'nin araştırma özelliğinde bulunan kullanıcı verilerinin nasıl yönetilenekadar yönetilene ilişkin bilgiler verilmektedir. Araştırma verileri Azure [Monitor günlüklerinde](../log-analytics/log-analytics-overview.md) depolanır ve Güvenlik Merkezi'nde açığa çıkarır. Kullanıcı verilerini yönetmek, verileri silme veya dışa aktarma olanağı nı içerir.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="searching-for-and-identifying-personal-data"></a>Kişisel verileri arama ve tanımlama
Azure portalında, kişisel verileri aramak için Güvenlik Merkezi'nin araştırma özelliğini kullanabilirsiniz. Araştırma özelliği Güvenlik **Uyarıları**altında kullanılabilir.

Araştırma özelliği, **Varlıklar** sekmesi altındaki tüm varlıkları, kullanıcı bilgilerini ve verileri gösterir.

## <a name="securing-and-controlling-access-to-personal-information"></a>Kişisel bilgilere erişimi güvenli hale getirme ve denetleme
Okuyucu, Sahip, Katkıda Bulunan veya Hesap Yöneticisi rolünü atadığı bir Güvenlik Merkezi kullanıcısı, araç içindeki müşteri verilerine erişebilir.

Okuyucu, Sahip ve Katılımcı rolleri hakkında daha fazla bilgi edinmek [için Azure rol tabanlı erişim denetimi için yerleşik rollere](../role-based-access-control/built-in-roles.md) bakın. Hesap Yöneticisi rolü hakkında daha fazla bilgi edinmek için [Azure abonelik yöneticilerine](../cost-management-billing/manage/add-change-subscription-administrator.md) bakın.

## <a name="deleting-personal-data"></a>Kişisel verileri silme
Sahibi, Katılımcısı veya Hesap Yöneticisi rolünü atadığı bir Güvenlik Merkezi kullanıcısı, soruşturma bilgilerini silebilir.

Bir araştırmayı silmek için `DELETE` Azure Kaynak Yöneticisi REST API'sine bir istek gönderebilirsiniz:

```HTTP
DELETE
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/features/security/incidents/{incidentName}
```

Giriş, `incidentName` bir `GET` istek kullanarak tüm olayları listeleyerek bulunabilir:

```HTTP
GET
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/features/security/incidents
```

## <a name="exporting-personal-data"></a>Kişisel verileri dışarı aktarma
Sahibi, Katılımcısı veya Hesap Yöneticisi rolünü atadığı bir Güvenlik Merkezi kullanıcısı, araştırma bilgilerini dışa aktarabilir. Soruşturma bilgilerini dışa aktarmak için, ilgili bilgileri kopyalamak ve yapıştırmak için **Varlıklar** sekmesine gidin.

## <a name="next-steps"></a>Sonraki adımlar
Kullanıcı verilerini yönetme hakkında daha fazla bilgi için azure [güvenlik merkezinde kullanıcı verilerini yönet'](security-center-privacy.md)e bakın.
Azure Monitor günlüklerinde özel verileri silme hakkında daha fazla bilgi edinmek için [özel verileri nasıl dışa aktarıp silebilirsiniz](../azure-monitor/platform/personal-data-mgmt.md#how-to-export-and-delete-private-data)bölümüne bakın.
