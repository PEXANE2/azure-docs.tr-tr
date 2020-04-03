---
title: Azure Bastion tanı günlüklerini etkinleştirme ve bu tür uygulamalarla çalışma
description: Bu makalede, Azure Bastion tanı günlüklerini etkinleştirmeyi ve bunlarla nasıl çalışacağınızı öğrenin.
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: charwen
ms.openlocfilehash: 97f0cdb1e93ef2ad06d2daa04b2f4893fd5dfac2
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619272"
---
# <a name="enable-and-work-with-bastion-diagnostic-logs"></a>Bastion tanı günlüklerini etkinleştirme ve bu günlüklerle çalışma

Kullanıcılar Azure Bastion'u kullanarak iş yüklerine bağlandıkça, Bastion uzak oturumların tanılamalarını kaydedebilir. Daha sonra tanılamayı hangi kullanıcıların hangi iş yüklerine, hangi zamanda, nerede ve diğer ilgili günlük bilgileriyle bağdaştıracağını görüntülemek için kullanabilirsiniz. Tanılamayı kullanabilmek için Azure Bastion'daki tanılama günlüklerini etkinleştirmeniz gerekir. Bu makale, tanılama günlüklerini etkinleştirmenize ve ardından günlükleri görüntülemenize yardımcı olur.

## <a name="enable-the-diagnostics-log"></a><a name="enable"></a>Tanılama günlüğünü etkinleştirme

1. Azure [portalında](https://portal.azure.com)Azure Bastion kaynağınıza gidin ve Azure Bastion sayfasından **Tanılama ayarlarını** seçin.

   ![tanılama ayarları](./media/diagnostic-logs/1diagnostics-settings.png)
2. **Tanılama ayarlarını**seçin, ardından günlükler için hedef eklemek için **+Tanılama ayarı ekle'yi** seçin.

   ![tanılama ayarı ekleme](./media/diagnostic-logs/2add-diagnostic-setting.png)
3. **Tanılama ayarları** sayfasında, tanılama günlüklerini depolamak için kullanılacak depolama hesabı türünü seçin.

   ![depolama konumunu seçin](./media/diagnostic-logs/3add-storage-account.png)
4. Ayarları tamamladığınızda, bu örneğe benzer:

   ![örnek ayarlar](./media/diagnostic-logs/4example-settings.png)

## <a name="view-diagnostics-log"></a><a name="view"></a>Tanılama günlüğünü görüntüle

Tanılama günlüklerinize erişmek için, tanılama ayarlarını etkinleştirirken belirttiğiniz depolama hesabını doğrudan kullanabilirsiniz.

1. Depolama hesabı kaynağınıza, ardından **Kapsayıcılar'a**gidin. Depolama hesabı blob konteyner oluşturulan **insights-logs-bastionauditlogs** blob bakın.

   ![tanılama ayarları](./media/diagnostic-logs/1-navigate-to-logs.png)
2. Kapsayıcının içine girerken, blogunuzda çeşitli klasörler görürsünüz. Bu klasörler, Azure Bastion kaynağınızın kaynak hiyerarşisini gösterir.

   ![tanılama ayarı ekleme](./media/diagnostic-logs/2-resource-h.png)
3. Erişim/görünüm istediğiniz tanılama günlükleri olan Azure Bastion kaynağınızın tam hiyerarşisine gidin. 'y=', 'm=', 'd=', 'h=' ve 'm=' tanı günlükleri için sırasıyla yılı, ayı, günü, saati ve dakikayı gösterir.

   ![depolama konumunu seçin](./media/diagnostic-logs/3-resource-location.png)
4. Azure Bastion tarafından oluşturulan ve gezindiği döneme ait tanılama günlüğü verilerini içeren json dosyasını bulun.

5. Depolama blob konteyner json dosyasını indirin. Json dosyasından örnek bir giriş başvuru için aşağıda gösterilmiştir:

   ```json
   { 
   "time":"2019-10-03T16:03:34.776Z",
   "resourceId":"/SUBSCRIPTIONS/<subscripionID>/RESOURCEGROUPS/MYBASTION/PROVIDERS/MICROSOFT.NETWORK/BASTIONHOSTS/MYBASTION-BASTION",
   "operationName":"Microsoft.Network/BastionHost/connect",
   "category":"BastionAuditLogs",
   "level":"Informational",
   "location":"eastus",
   "properties":{ 
      "userName":"<username>",
      "userAgent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36",
      "clientIpAddress":"131.107.159.86",
      "clientPort":24039,
      "protocol":"ssh",
      "targetResourceId":"/SUBSCRIPTIONS/<subscripionID>/RESOURCEGROUPS/MYBASTION/PROVIDERS/MICROSOFT.COMPUTE/VIRTUALMACHINES/LINUX-KEY",
      "subscriptionId":"<subscripionID>",
      "message":"Successfully Connected.",
      "resourceType":"VM",
      "targetVMIPAddress":"172.16.1.5",
      "tunnelId":"<tunnelID>"
   },
   "FluentdIngestTimestamp":"2019-10-03T16:03:34.0000000Z",
   "Region":"eastus",
   "CustomerSubscriptionId":"<subscripionID>"
   }
   ```

## <a name="next-steps"></a>Sonraki adımlar

[Bastion SSS'yi](bastion-faq.md)okuyun.
