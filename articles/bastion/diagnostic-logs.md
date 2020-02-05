---
title: Azure savunma tanılama günlüklerini etkinleştirme ve bunlarla çalışma
description: Bu makalede, Azure savunma tanılama günlüklerini etkinleştirme ve bunlarla çalışmayı öğrenin.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: cherylmc
ms.openlocfilehash: 2167a17d5d388c97ad357398c4ac2676e43be5a4
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76989459"
---
# <a name="enable-and-work-with-bastion-diagnostic-logs"></a>Etkinleştirme tanılama günlüklerini etkinleştirme ve bunlarla çalışma

Kullanıcılar Azure savunma kullanarak iş yüklerine bağlandıklarında, uzak oturumların tanılamayı günlüğe kaydedebilir. Daha sonra tanılamayı, hangi zaman, nerede ve diğer ilgili günlüğe kaydetme bilgilerini hangi iş yüklerine bağladığınızı görüntülemek için kullanabilirsiniz. Tanılamayı kullanabilmeniz için Azure 'da tanılama günlüklerini etkinleştirmeniz gerekir. Bu makale, tanılama günlüklerini etkinleştirmenize ve sonra günlükleri görüntülemenize yardımcı olur.

## <a name="enable"></a>Tanılama günlüğünü etkinleştirme

1. [Azure Portal](https://portal.azure.com)Azure savunma kaynağınız ' ne gidin ve Azure savunma sayfasından **Tanılama ayarları** ' nı seçin.

   ![Tanılama ayarları](./media/diagnostic-logs/1diagnostics-settings.png)
2. **Tanılama ayarları**' nı seçin, sonra da Günlükler için bir hedef eklemek için **+ Tanılama ayarı Ekle** ' yi seçin.

   ![Tanılama ayarı Ekle](./media/diagnostic-logs/2add-diagnostic-setting.png)
3. **Tanılama ayarları** sayfasında, tanılama günlüklerini depolamak için kullanılacak depolama hesabı türünü seçin.

   ![depolama konumunu seçin](./media/diagnostic-logs/3add-storage-account.png)
4. Ayarları tamamladığınızda, bu örneğe benzer şekilde görünür:

   ![örnek ayarlar](./media/diagnostic-logs/4example-settings.png)

## <a name="view"></a>Tanılama günlüğünü görüntüle

Tanılama günlüklerine erişmek için, tanılama ayarlarını etkinleştirirken belirttiğiniz depolama hesabını doğrudan kullanabilirsiniz.

1. Depolama hesabınızın kaynağına ve ardından **kapsayıcılara**gidin. Depolama hesabı blobu kapsayıcıda oluşturulan **Öngörüler-logs-bastionauditlogs** blob 'unu görürsünüz.

   ![Tanılama ayarları](./media/diagnostic-logs/1-navigate-to-logs.png)
2. Kapsayıcının içinde gezinirken, Blogunuzdaki çeşitli klasörleri görürsünüz. Bu klasörler, Azure savunma kaynağınız için kaynak hiyerarşisini gösterir.

   ![Tanılama ayarı Ekle](./media/diagnostic-logs/2-resource-h.png)
3. Tanılama günlüklerine erişmek/görüntülemek istediğiniz Azure savunma kaynağınızın tam hiyerarşisine gidin. ' Y = ', 'm = ', ' = ', ' h = ' ve 'm = ', tanılama günlükleri için sırasıyla yılı, ayı, günü, saati ve dakikayı gösterir.

   ![depolama konumunu seçin](./media/diagnostic-logs/3-resource-location.png)
4. Azure savunma tarafından oluşturulan ve zaman dönemi için tanılama günlük verilerini içeren JSON dosyasını bulun.

5. JSON dosyasını depolama blob kapsayıcıınızdan indirin. Aşağıdaki başvuru için JSON dosyasından örnek bir giriş gösterilmektedir:

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

Savunma [hakkında SSS](bastion-faq.md)makalesini okuyun.
