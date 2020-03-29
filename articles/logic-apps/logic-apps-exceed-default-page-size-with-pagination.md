---
title: Pagination ile daha fazla öğe veya kayıt alın
description: Azure Mantık Uygulamaları'ndaki bağlayıcı eylemleri için varsayılan sayfa boyutu sınırını aşacak şekilde pagination ayarlama
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 04/11/2019
ms.openlocfilehash: 75d9660eb35b5d7ddc644d177c11ae489e2853dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74792115"
---
# <a name="get-more-data-items-or-records-by-using-pagination-in-azure-logic-apps"></a>Azure Logic Apps'ta pagination kullanarak daha fazla veri, öğe veya kayıt edinin

[Azure Logic Apps'ta](../logic-apps/logic-apps-overview.md)bir bağlayıcı eylemi kullanarak veri, öğe veya kayıt aldığınızda, eylemin tüm sonuçları aynı anda döndürmeyebileceği kadar büyük sonuç kümeleri alabilirsiniz. Bazı eylemlerde, sonuç sayısı bağlayıcının varsayılan sayfa boyutunu aşabilir. Bu durumda, eylem sonuçların yalnızca ilk sayfasını döndürür. Örneğin, SQL Server bağlayıcısının **Satır Al** eylemi için varsayılan sayfa boyutu 2048'dir, ancak diğer ayarlara bağlı olarak değişebilir.

Bazı eylemler, mantık uygulamanızın pagination *sınırına* kadar daha fazla sonuç alabilmeniz için bir duraklama ayarını açmanızı sağlar, ancak eylem bittiğinde bu sonuçları tek bir ileti olarak döndürebilir. Pagination kullandığınızda, eylemin döndürülmesi için istediğiniz sonuç hedef sayısı olan bir *eşik* değeri belirtmeniz gerekir. Eylem, belirtilen eşiğinize ulaşana kadar sonuçları alır. Toplam öğe sayınız belirtilen eşikten küçükse, eylem tüm sonuçları alır.

Pagination ayarını açmak, bağlayıcının sayfa boyutuna bağlı olarak sonuçların sayfalarını alır. Bu davranış, bazen belirttiğiniz eşiğe göre daha fazla sonuç alabileceğiniz anlamına gelir. Örneğin, sql server kullanarak pagination ayarı destekleyen **satırlar** eylem alın:

* Eylemin varsayılan sayfa boyutu, sayfa başına 2048 kayıttır.
* 10.000 kaydınız olduğunu ve en az 5000 kaydı belirtdiğinizi varsayalım.
* Pagination kayıtların sayfaları alır, bu yüzden en az belirtilen minimum almak için, eylem 6144 kayıtları (3 sayfa x 2048 kayıtları değil, 5000 kayıtları döndürür.

Aşağıda, belirli eylemler için varsayılan sayfa boyutunu aşabileceğiniz bağlayıcılardan bazılarının yer aldığı bir liste verilmiştir:

* [Azure Blob Depolama](https://docs.microsoft.com/connectors/azureblob/)
* [Dynamics 365](https://docs.microsoft.com/connectors/dynamicscrmonline/)
* [Excel](https://docs.microsoft.com/connectors/excel/)
* [HTTP Kı.,](https://docs.microsoft.com/azure/connectors/connectors-native-http)
* [IBM DB2](https://docs.microsoft.com/connectors/db2/)
* [Microsoft Teams](https://docs.microsoft.com/connectors/teams/)
* [Oracle Veritabanı](https://docs.microsoft.com/connectors/oracle/)
* [Salesforce](https://docs.microsoft.com/connectors/salesforce/)
* [SharePoint](https://docs.microsoft.com/connectors/sharepointonline/)
* [SQL Server](https://docs.microsoft.com/connectors/sql/)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir Azure aboneliğiniz yoksa, [ücretsiz bir Azure hesabına kaydolun.](https://azure.microsoft.com/free/)

* Mantık uygulaması ve pagination açmak istediğiniz eylem. Bir mantık uygulamanız yoksa, [Bkz. Quickstart: İlk mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="turn-on-pagination"></a>Pagination açın

Bir eylemin Logic App Designer'da pagination'ı destekleyip desteklemediğini belirlemek için, **Pagination** ayarı için eylemin ayarlarını kontrol edin. Bu örnek, SQL Server'ın **Satırları Al** eyleminde pagination'ı nasıl açacağını gösterir.

1. Eylemin sağ üst köşesinde, elipsleri (**...**) düğmesini seçin ve **Ayarlar'ı**seçin.

   ![Eylemin ayarlarını açma](./media/logic-apps-exceed-default-page-size-with-pagination/sql-action-settings.png)

   Eylem pagination destekliyorsa, eylem **Pagination** ayarı gösterir.

1. **Pagination** ayarını **Off'tan** **On'a**değiştirin. **Eşik** özelliğinde, eylemin dönmesini istediğiniz hedef sonuç sayısı için bir tamsayı değeri belirtin.

   ![İade etmek için en az sonuç sayısını belirtin](./media/logic-apps-exceed-default-page-size-with-pagination/sql-action-settings-pagination.png)

1. Hazır **olduğunuzda, Bitti'yi**seçin.

## <a name="workflow-definition---pagination"></a>İş akışı tanımı - pagination

Bu özelliği destekleyen bir eylem için pagination'ı açtığınızda, mantık uygulamanızın iş `"minimumItemCount"` akışı tanımı, örneğin, o eylemin `"runtimeConfiguration"` özelliğindeki `"paginationPolicy"` özelliğin yanı sıra özelliği de içerir:

```json
"actions": {
   "HTTP": {
      "inputs": {
         "method": "GET",
         "uri": "https://www.testuri.com"
      },
      "runAfter": {},
      "runtimeConfiguration": {
         "paginationPolicy": {
            "minimumItemCount": 1000
         }
      },
      "type": "Http"
   }
},
```

## <a name="get-support"></a>Destek alın

Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
