---
title: Yönetilen bir uygulamayı izlemek için Azure portalını kullanma
description: Yönetilen bir uygulamaiçin kullanılabilirliği ve uyarıları izlemek için Azure portalının nasıl kullanılacağını gösterir.
author: tfitzmac
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: afe78dd00ecebdc54b6d73c4c8324729e117d95b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75651754"
---
# <a name="monitor-a-deployed-instance-of-a-managed-application"></a>Yönetilen bir uygulamanın dağıtılmış örneğini izleme

Azure aboneliğinize yönetilen bir uygulama dağıttıktan sonra, uygulamanın durumunu denetlemek isteyebilirsiniz. Bu makalede, durumu denetlemek için Azure portalında seçenekler gösterilmektedir. Yönetilen uygulamanızdaki kaynakların kullanılabilirliğini izleyebilirsiniz. Uyarıları ayarlayabilir ve görüntüleyebilirsiniz.

## <a name="view-resource-health"></a>Kaynak durumunu görüntüleme

1. Yönetilen uygulama örneğini seçin.

   ![Yönetilen uygulamayı seçin](./media/monitor-managed-application-portal/select-managed-application.png)

1. **Kaynak Durumu'nun 'u**seçin.

   ![Kaynak sistem durumunu seçin](./media/monitor-managed-application-portal/select-resource-health.png)

1. Yönetilen uygulamanızdaki kaynakların kullanılabilirliğini görüntüleyin.

   ![Kaynak durumunu görüntüleme](./media/monitor-managed-application-portal/view-health.png)

## <a name="view-alerts"></a>Uyarıları görüntüleme

1. **Uyarıları**seçin.

   ![Uyarıları seçme](./media/monitor-managed-application-portal/select-alerts.png)

1. Uyarı kurallarını yapılandırMışsanız, yükseltilen uyarılarla ilgili bilgileri görürsünüz.

   ![Uyarıları görüntüleme](./media/monitor-managed-application-portal/view-alerts.png)

1. Uyarı kuralları eklemek için **+ Yeni uyarı kuralını**seçin.

   ![Uyarı oluşturma](./media/monitor-managed-application-portal/create-new-alert.png)

Yönetilen uygulama örneğiniz veya yönetilen uygulamadaki kaynaklar için uyarılar oluşturabilirsiniz. Uyarı oluşturma hakkında bilgi için Microsoft [Azure'daki uyarılara Genel Bakış](../../azure-monitor/platform/alerts-overview.md)bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulama örnekleri [için, Azure yönetilen uygulamalar için Örnek projelere](sample-projects.md)bakın.
* Yönetilen bir uygulamayı dağıtmak için Azure [portalı üzerinden hizmet kataloğu uygulamasını dağıt'a](deploy-service-catalog-quickstart.md)bakın.