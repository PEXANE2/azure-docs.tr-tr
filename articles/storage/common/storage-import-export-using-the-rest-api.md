---
title: Azure İçe Alma/Dışa Aktarma hizmetini kullanma REST API | Microsoft Dokümanlar
description: Azure İçe Alma/Dışa Aktarma hizmeti REST API'yi kullanmak için hem nasıl yapılacağını nhem de başvuru malzemesi de dahil olmak üzere kaynakları nerede bulacağınızı öğrenin.
author: twooley
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: twooley
ms.subservice: common
ms.openlocfilehash: 833b8c79fba57b7129092e084381c0671c396496
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74978875"
---
# <a name="using-the-azure-importexport-service-rest-api"></a>Azure İçeri/Dışarı Aktarma hizmeti REST API’sini kullanma

Microsoft Azure İçe Alma/Dışa Aktarma hizmeti, alma/dışa aktarma işlerinin programlı denetimini etkinleştirmek için bir REST API'sini ortaya çıkarır. Azure [portalı](https://portal.azure.com/)ile gerçekleştirebileceğiniz tüm alma/dışa aktarma işlemlerini gerçekleştirmek için REST API'sini kullanabilirsiniz. Ayrıca, Şu anda Azure portalında bulunmayan bir işin tamamlanma yüzdesini sorgulamak gibi belirli parçalı işlemleri gerçekleştirmek için REST API'sini kullanabilirsiniz.

İçe Alma/Dışa Aktarma hizmetine genel bir bakış ve alma ve dışa aktarma işlerini oluşturmak ve yönetmek için portalın nasıl kullanılacağını gösteren bir öğretici için [Verileri Blob Depolama'ya aktarmak için Microsoft Azure İçe Alma/Dışa Aktarma hizmetini](../storage-import-export-service.md) kullanma bölümüne bakın.

## <a name="service-endpoints"></a>Hizmet uç noktaları

Azure İçe Alma/Dışa Aktarma hizmeti Azure Kaynak Yöneticisi için bir kaynak sağlayıcısıdır ve alma/dışa aktarma işlerini yönetmek için aşağıdaki HTTPS bitiş noktasında bir dizi REST API'si sağlar:

```
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ImportExport/jobs/<job-name>
```

## <a name="versioning"></a>Sürüm Oluşturma

İçe/Dışa Aktar hizmetine `api-version` yapılan istekler parametreyi belirtmeli ve değerini ' olarak `2016-11-01`ayarlamalıdır.

## <a name="importexport-service-operations"></a>İthalat/İhracat hizmet işlemleri

[İçeri aktarma işi oluşturma](../storage-import-export-creating-an-import-job.md)

[Dışarı aktarma işi oluşturma](../storage-import-export-creating-an-export-job.md)

[Bir iş için durum bilgilerini alma](storage-import-export-retrieving-state-info-for-a-job.md)

[İşleri numaralandırma](../storage-import-export-enumerating-jobs.md)

[İşleri iptal etme ve silme](storage-import-export-cancelling-and-deleting-jobs.md)

[Sürücü bildirimlerini yedekleme](../storage-import-export-backing-up-drive-manifests.md)

[İçeri/Dışarı Aktarma işleri için tanılama ve hata kurtarma](../storage-import-export-diagnostics-and-error-recovery.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama İthalat/İhracat REST](/rest/api/storageimportexport)
