---
title: Sayfalandırmaya sahip daha fazla öğe veya kayıt alın
description: Azure Logic Apps içindeki bağlayıcı eylemleri için varsayılan sayfa boyutu sınırını aşacak şekilde sayfalandırma ayarla
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 04/11/2019
ms.openlocfilehash: d46bf711a46e27b81a1284b5fc55cf403b7da048
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87090272"
---
# <a name="get-more-data-items-or-records-by-using-pagination-in-azure-logic-apps"></a>Azure Logic Apps ' de sayfalandırma kullanarak daha fazla veri, öğe veya kayıt alın

[Azure Logic Apps](../logic-apps/logic-apps-overview.md)bir bağlayıcı eylemini kullanarak veri, öğe veya kayıt aldığınızda, sonuç kümelerini, eylemin aynı anda tüm sonuçları döndürmediğini büyük bir şekilde elde edebilirsiniz. Bazı eylemlerle, sonuç sayısı bağlayıcının varsayılan sayfa boyutunu aşabilir. Bu durumda, eylem yalnızca ilk sonuç sayfasını döndürür. Örneğin, SQL Server bağlayıcının **satırları al** eyleminin varsayılan sayfa boyutu 2048 ' dir, ancak diğer ayarlara göre farklılık gösterebilir.

Bazı eylemler, mantıksal uygulamanızın sayfalandırma sınırına kadar daha fazla sonuç alabilmesi için bir *sayfalandırma* ayarını açmanızı sağlar, ancak eylem tamamlandığında bu sonuçları tek bir ileti olarak döndürebilir. Sayfalandırma kullandığınızda, eylemin döndürmesini istediğiniz sonuç sayısı olan bir *eşik* değeri belirtmeniz gerekir. Eylem, belirtilen eşiğe ulaşıncaya kadar sonuçları alır. Toplam öğe sayısı belirtilen eşikten az olduğunda, eylem tüm sonuçları alır.

Sayfalandırma ayarını açmak, bir bağlayıcının sayfa boyutuna bağlı olarak sonuçların sayfalarını alır. Bu davranış bazen, belirtilen eşikten daha fazla sonuç elde edemeyeceğiniz anlamına gelir. Örneğin, sayfalandırma ayarını destekleyen SQL Server **satırları al** eylemini kullanırken:

* Eylemin varsayılan sayfa boyutu sayfa başına 2048 kayıtdır.
* 10.000 kayıt olduğunu ve en az 5000 kayıt olduğunu varsayalım.
* Sayfalandırma, kayıt sayfalarını alır, bu nedenle en az belirtilen minimum değeri almak için, işlem 5000 kayıt değil 6144 kayıt (3 sayfa x 2048 kayıt) döndürür.

Aşağıda, belirli eylemler için varsayılan sayfa boyutunu aştığınız bazı bağlayıcılardan yalnızca bazılarına sahip bir liste verilmiştir:

* [Azure Blob Depolama](/connectors/azureblob/)
* [Dynamics 365](/connectors/dynamicscrmonline/)
* [Excel](/connectors/excel/)
* [HTTP](../connectors/connectors-native-http.md)
* [IBM DB2](/connectors/db2/)
* [Microsoft Teams](/connectors/teams/)
* [Oracle Veritabanı](/connectors/oracle/)
* [Salesforce](/connectors/salesforce/)
* [SharePoint](/connectors/sharepointonline/)
* [SQL Server](/connectors/sql/)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Henüz bir Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Mantıksal uygulama ve sayfalandırmayı açmak istediğiniz eylem. Mantıksal uygulamanız yoksa, bkz. [hızlı başlangıç: ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="turn-on-pagination"></a>Sayfalandırmayı aç

Mantıksal uygulama tasarımcısında bir eylemin sayfalandırmayı destekleyip desteklemediğini anlamak için, **sayfalandırma** ayarı için eylemin ayarlarını denetleyin. Bu örnekte, SQL Server **satırları al** eyleminde sayfalandırmayı açma işlemi gösterilmektedir.

1. Eylemin sağ üst köşesinde üç nokta (**...**) düğmesini ve ardından **Ayarlar**' ı seçin.

   ![Eylemin ayarlarını açın](./media/logic-apps-exceed-default-page-size-with-pagination/sql-action-settings.png)

   Eylem sayfalandırmayı destekliyorsa, eylem **sayfalandırma** ayarını gösterir.

1. **Sayfalama** ayarını **Açık** **olarak değiştirin** . **Threshold** özelliğinde, eylemin döndürmesini istediğiniz sonuçların hedef sayısı için bir tamsayı değeri belirtin.

   ![Döndürülecek en az sonuç sayısını belirtin](./media/logic-apps-exceed-default-page-size-with-pagination/sql-action-settings-pagination.png)

1. Hazırsanız, **bitti**' yi seçin.

## <a name="workflow-definition---pagination"></a>İş akışı tanımı-sayfalandırma

Bu özelliği destekleyen bir eylem için sayfalandırmayı açtığınızda, mantıksal uygulamanızın iş akışı tanımı, `"paginationPolicy"` Bu eylemin özelliğindeki özelliği ile birlikte özelliği içerir `"minimumItemCount"` `"runtimeConfiguration"` , örneğin:

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

## <a name="get-support"></a>Destek alma

Sorular için, [Azure Logic Apps Için Microsoft Q&soru sayfasını](/answers/topics/azure-logic-apps.html)ziyaret edin.
