---
title: Azure statik Web Apps önizlemede kotalar
description: Azure statik Web Apps önizlemesi ile ilişkili kotalar hakkında bilgi edinin
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: overview
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: 915f8675ffda0d70347905d11c7d93975b9d4526
ms.sourcegitcommit: 33368ca1684106cb0e215e3280b828b54f7e73e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92132746"
---
# <a name="quotas-in-azure-static-web-apps-preview"></a>Azure statik Web Apps önizlemede kotalar

Azure statik Web Apps önizlemesi için aşağıdaki kotalar mevcuttur.

> [!IMPORTANT]
> Azure statik Web Apps genel önizlemede ve üretim kullanımı için tasarlanmamıştır.

| Özellik                     | Ücretsiz plan        |
|-----------------------------|------------------|
| Dahil edilen bant genişliği          | ayda 100 GB |
| Fazla kullanım bant genişliği           | Kullanılamaz      |
| Azure aboneliği başına uygulamalar | 10               |
| Uygulama boyutu                    | 250 MB           |
| Üretim öncesi ortamları | 1                |
| Özel etki alanları              | 1                |
| Yetkilendirme<br><br>Özel roller ve yönlendirme kuralları ile | En fazla 25 Son Kullanıcı davet edilen ve atanan roller |
| Azure İşlevleri             | Kullanılabilir        |
| SLA                         | Hiçbiri             |

## <a name="github-storage"></a>GitHub depolama alanı

GitHub eylemleri ve paketleri, kendi kotalar kümesine sahip GitHub Storage kullanır. Azure statik Web Apps sitesi oluşturduğunuzda GitHub, dağıtımdan sonra bile sitenizin yapıtları depolar.

Daha ayrıntılı bilgi için aşağıdaki kaynaklara bakın:

- [Eylemler depolama alanını yönetme](https://github.community/t5/GitHub-Actions/Managing-Actions-storage-space/td-p/38944)
- [GitHub eylemleri için faturalandırma hakkında](https://help.github.com/github/setting-up-and-managing-billing-and-payments-on-github/about-billing-for-github-actions#about-billing-for-github-actions)
- [GitHub eylemleri için harcama limitinizi yönetme](https://help.github.com/github/setting-up-and-managing-billing-and-payments-on-github/managing-your-spending-limit-for-github-actions)

## <a name="next-steps"></a>Sonraki adımlar

- [Genel Bakış](overview.md)
