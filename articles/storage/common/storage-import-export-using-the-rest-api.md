---
title: Azure Içeri/dışarı aktarma hizmeti 'ni kullanma REST API | Microsoft Docs
description: Azure Içeri/dışarı aktarma hizmeti REST API, hem nasıl yapılır hem de başvuru malzemeleri dahil olmak üzere kaynakları nerede bulacağınızı öğrenin.
author: twooley
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: twooley
ms.subservice: common
ms.openlocfilehash: 833b8c79fba57b7129092e084381c0671c396496
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74978875"
---
# <a name="using-the-azure-importexport-service-rest-api"></a>Azure İçeri/Dışarı Aktarma hizmeti REST API’sini kullanma

Microsoft Azure İçeri/Dışarı Aktarma Hizmeti içeri/dışarı aktarma işlerinin programlı denetimini etkinleştirmek için bir REST API sunar. [Azure Portal](https://portal.azure.com/)ile gerçekleştirebileceğiniz tüm içeri/dışarı aktarma işlemlerini gerçekleştirmek için REST API kullanabilirsiniz. Ayrıca, şu anda Azure portal kullanılamayan bir işin yüzde tamamlanmasını sorgulama gibi belirli ayrıntılı işlemleri gerçekleştirmek için REST API kullanabilirsiniz.

Içeri/dışarı aktarma hizmetine genel bir bakış ve içeri aktarma ve dışarı aktarma işlerini oluşturmak ve yönetmek için portalın nasıl kullanılacağını gösteren bir öğreticiye yönelik [Microsoft Azure içeri/dışarı aktarma hizmetini kullanarak](../storage-import-export-service.md) Içeri/dışarı aktarma hizmeti 'Ne veri aktarma

## <a name="service-endpoints"></a>Hizmet uç noktaları

Azure Içeri/dışarı aktarma hizmeti Azure Resource Manager için bir kaynak sağlayıcıdır ve içeri/dışarı aktarma işlerini yönetmek için aşağıdaki HTTPS uç noktasındaki bir REST API kümesi sağlar:

```
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ImportExport/jobs/<job-name>
```

## <a name="versioning"></a>Sürüm oluşturma

Içeri/dışarı aktarma hizmetine yapılan isteklerin `api-version` parametresini belirtmesi ve değerini `2016-11-01`olarak ayarlaması gerekir.

## <a name="importexport-service-operations"></a>İçeri/dışarı aktarma hizmeti işlemleri

[İçeri aktarma işi oluşturma](../storage-import-export-creating-an-import-job.md)

[Dışarı aktarma işi oluşturma](../storage-import-export-creating-an-export-job.md)

[Bir iş için durum bilgilerini alma](storage-import-export-retrieving-state-info-for-a-job.md)

[İşleri numaralandırma](../storage-import-export-enumerating-jobs.md)

[İşleri iptal etme ve silme](storage-import-export-cancelling-and-deleting-jobs.md)

[Sürücü bildirimlerini yedekleme](../storage-import-export-backing-up-drive-manifests.md)

[İçeri/Dışarı Aktarma işleri için tanılama ve hata kurtarma](../storage-import-export-diagnostics-and-error-recovery.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama Içeri/dışarı aktarma REST](/rest/api/storageimportexport)
