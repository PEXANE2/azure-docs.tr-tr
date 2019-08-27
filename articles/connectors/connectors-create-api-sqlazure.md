---
title: SQL Server veya Azure SQL veritabanı 'na bağlanma-Azure Logic Apps | Microsoft Docs
description: Azure Logic Apps ile iş akışlarını otomatikleştirerek şirket içinde veya bulutta SQL veritabanlarına erişme ve bunları yönetme
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/15/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 804a913d17c3151d07a1ecf229e2db148dc45558
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70050759"
---
# <a name="connect-to-sql-server-or-azure-sql-database-from-azure-logic-apps"></a>Azure Logic Apps SQL Server veya Azure SQL veritabanı 'na bağlanma

Bu makalede, SQL veritabanınızdaki verilere SQL Server Bağlayıcısı ile bir mantıksal uygulama içinden nasıl erişebileceğiniz gösterilmektedir. Bu şekilde, mantıksal uygulamalar oluşturarak SQL verilerinizi ve kaynaklarınızı yöneten görevleri, işlemleri ve iş akışlarını otomatikleştirebileceğinizi unutmayın. Bağlayıcı hem şirket içinde hem de [Bulutta Azure SQL veritabanı](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)için [SQL Server](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation) . 

SQL veritabanınızda veya Dynamics CRM Online gibi diğer sistemlerde olaylar tarafından tetiklendiğinde çalışan Logic Apps oluşturabilirsiniz. Mantıksal uygulamalarınız SQL sorguları ve saklı yordamlar yürütme ile birlikte verileri de alabilir, ekleyebilir ve silebilir. Örneğin, Dynamics CRM Online 'da yeni kayıtları otomatik olarak denetleyen bir mantıksal uygulama oluşturabilir, her yeni kayıt için SQL veritabanınıza öğe ekler ve ardından e-posta uyarıları gönderir.

Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). Logic Apps 'e yeni başladıysanız [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç ' ı inceleyin: İlk mantıksal uygulamanızı](../logic-apps/quickstart-create-first-logic-app-workflow.md)oluşturun. Bağlayıcıya özgü teknik bilgiler için [SQL Server bağlayıcı başvurusuna](https://docs.microsoft.com/connectors/sql/)bakın.

## <a name="prerequisites"></a>Önkoşullar

* SQL veritabanınıza erişmeniz gereken mantıksal uygulama. Mantıksal uygulamanızı bir SQL tetikleyicisi ile başlatmak için [boş bir mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md)gerekir. 

* Bir [Azure SQL veritabanı](../sql-database/sql-database-get-started-portal.md) veya bir [SQL Server veritabanı](https://docs.microsoft.com/sql/relational-databases/databases/create-a-database) 

  Mantıksal uygulamanızın işlemleri çağırırken sonuçlar döndürebilmesi için tablolarınızın verileri olmalıdır. Azure SQL veritabanı oluşturursanız, dahil edilen örnek veritabanlarını kullanabilirsiniz. 

* SQL sunucunuzun adı, veritabanı adı, Kullanıcı adınız ve parolanız. Mantığınızı SQL Server 'a erişim için yetkilendirebilmeniz için bu kimlik bilgilerine ihtiyacınız vardır. 

  * Azure SQL veritabanı için bu ayrıntıları bağlantı dizesinde veya SQL veritabanı özellikleri altındaki Azure portal bulabilirsiniz:

    "Server = TCP: <*yourservername*>. Database. Windows. net, 1433; Initial Catalog = <*yourdatabasename*>; Kalıcı güvenlik bilgisi = false; Kullanıcı KIMLIĞI = <*Etkiadı*>; Password =*parolanızı*< >; MultipleActiveResultSets = false; Encrypt = true; TrustServerCertificate = false; Bağlantı zaman aşımı = 30; "

  * SQL Server için bu ayrıntıları bağlantı dizesinde bulabilirsiniz: 

    "Server = <*Yourserveraddress*>;D atabase = <*yourdatabasename*>; Kullanıcı kimliği = <*Etkiadı*>; Password =*parolanızı*< >; "

* Mantıksal uygulamaları SQL Server gibi şirket içi sistemlere bağlayabilmeniz için önce şirket [içi veri ağ geçidini ayarlamanız](../logic-apps/logic-apps-gateway-install.md)gerekir. Bu şekilde, mantıksal uygulamanız için SQL bağlantısı oluştururken ağ geçidini seçebilirsiniz.

<a name="add-sql-trigger"></a>

## <a name="add-sql-trigger"></a>SQL tetikleyicisi Ekle

Azure Logic Apps, her mantıksal uygulama, belirli bir olay gerçekleştiğinde [](../logic-apps/logic-apps-overview.md#logic-app-concepts)veya belirli bir koşul karşılandığında tetiklenen bir tetikleyiciyle başlamalıdır. Tetikleyici her tetiklendiğinde Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışını çalıştırmaya başlar.

1. Azure portal veya Visual Studio 'da, Logic Apps tasarımcısını açan boş bir mantıksal uygulama oluşturun. Bu örnek Azure portal kullanır.

2. Arama kutusuna filtreniz olarak "SQL Server" yazın. Tetikleyiciler listesinden istediğiniz SQL tetikleyicisini seçin. 

   Bu örnek için şu tetikleyiciyi seçin: **SQL Server-bir öğe oluşturulduğunda**

   !["SQL Server-bir öğe oluşturulduğunda" tetikleyicisi seçin](./media/connectors-create-api-sqlazure/sql-server-trigger.png)

3. Bağlantı ayrıntıları istenirse, [SQL bağlantınızı şimdi oluşturun](#create-connection). 
   Ya da bağlantınız zaten varsa, listeden istediğiniz **tablo adını** seçin.

   ![Tablo seçin](./media/connectors-create-api-sqlazure/azure-sql-database-table.png)

4. Mantıksal uygulamanızın tabloyu ne sıklıkta denetleyeceğini belirten **Aralık** ve **Sıklık** özelliklerini ayarlayın.

   Bu örnek yalnızca seçili tabloyu denetler, başka hiçbir şey yapmaz. 
   Daha ilgi çekici bir şey yapmak için, istediğiniz görevleri gerçekleştiren eylemler ekleyin. 
   
   Örneğin, tablodaki yeni öğeyi görüntülemek için, tablodaki alanları içeren bir dosya oluştur ve sonra e-posta uyarıları Gönder gibi başka eylemler ekleyebilirsiniz. 
   Bu bağlayıcıya veya diğer bağlayıcılara yönelik diğer eylemler hakkında bilgi edinmek için bkz. [Logic Apps bağlayıcıları](../connectors/apis-list.md).

5. İşiniz bittiğinde, Tasarımcı araç çubuğunda **Kaydet**' i seçin. 

   Bu adım otomatik olarak Azure 'da mantıksal uygulamanızı etkin bir şekilde etkinleştirilir ve yayımlar. 

<a name="add-sql-action"></a>

## <a name="add-sql-action"></a>SQL eylemi Ekle

Azure Logic Apps bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) , iş akışınızda bir tetikleyiciyi veya başka bir eylemi izleyen bir adımdır. Bu örnekte, mantıksal uygulama [yinelenme tetikleyicisiyle](../connectors/connectors-native-recurrence.md)başlar ve bir SQL veritabanından bir satır alan bir eylem çağırır.

1. Azure portal veya Visual Studio 'da mantıksal uygulamanızı Logic Apps tasarımcısında açın. Bu örnek Azure portal kullanır.

2. Mantıksal uygulama Tasarımcısı ' nda, tetikleyici veya eylem altında **yeni adım** > **Eylem Ekle**' yi seçin.

   !["Yeni adım", "Eylem Ekle" yi seçin](./media/connectors-create-api-sqlazure/add-action.png)
   
   Varolan adımlar arasında bir eylem eklemek için farenizi bağlantı oku üzerine taşıyın. 
   Görüntülenen artı işaretini ( **+** ) seçin ve ardından **Eylem Ekle**' yi seçin.

2. Arama kutusuna filtreniz olarak "SQL Server" yazın. Eylemler listesinden istediğiniz herhangi bir SQL eylemini seçin. 

   Bu örnek için, tek bir kayıt alan bu eylemi seçin: **SQL Server-satırı al**

   !["SQL Server" yazın, "SQL Server-satırı al" ı seçin](./media/connectors-create-api-sqlazure/select-sql-get-row.png) 

3. Bağlantı ayrıntıları istenirse, [SQL bağlantınızı şimdi oluşturun](#create-connection). 
   Ya da bağlantınız varsa, bir **tablo adı**seçin ve Istediğiniz KAYDıN **satır kimliğini** girin.

   ![Tablo adını ve satır KIMLIĞINI girin](./media/connectors-create-api-sqlazure/table-row-id.png)
   
   Bu örnek, seçili tablodan yalnızca bir satır döndürür, başka hiçbir şey yapmaz. 
   Bu satırdaki verileri görüntülemek için, daha sonra gözden geçirmek üzere satırdaki alanlarla bir dosya oluşturan başka eylemler ekleyebilir ve bu dosyayı bir bulut depolama hesabında saklayabilirsiniz. Bu bağlayıcıdaki veya diğer bağlayıcılardaki diğer eylemler hakkında bilgi edinmek için bkz. [Logic Apps bağlayıcıları](../connectors/apis-list.md).

4. İşiniz bittiğinde, Tasarımcı araç çubuğunda **Kaydet**' i seçin. 

<a name="create-connection"></a>

## <a name="connect-to-your-database"></a>Veritabanınıza bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to SQL Server or Azure SQL Database](../../includes/connectors-create-api-sqlazure.md)]

## <a name="handle-bulk-data"></a>Toplu verileri işle

Bazen, bağlayıcının sonuçları aynı anda döndürmemesi veya sonuç kümelerinizin boyut ve yapısı üzerinde daha iyi denetim istemeniz için sonuç kümeleriyle çalışmanız gerekebilir. Bu tür büyük sonuç kümelerini işleyebilmeniz için bazı yollar şunlardır:

* Sonuçları daha küçük kümeler olarak yönetmenize yardımcı olmak için *sayfalandırma*'yı açın. Daha fazla bilgi için bkz. [sayfalandırma kullanarak toplu verileri, kayıtları ve öğeleri edinme](../logic-apps/logic-apps-exceed-default-page-size-with-pagination.md).

* Sonuçları istediğiniz şekilde düzenleyen bir saklı yordam oluşturun.

  Birden çok satır alırken veya eklerken, mantıksal uygulamanız bu [sınırlar](../logic-apps/logic-apps-limits-and-config.md)dahilinde bir [*until döngüsü*](../logic-apps/logic-apps-control-flow-loops.md#until-loop) kullanarak bu satırları yineleyebilir. 
  Ancak, mantıksal uygulamanızın kayıt kümeleriyle birlikte çalışması gerektiğinde, büyük bir deyişle, binlerce veya milyonlarca satır olmak üzere veritabanına yapılan çağrılardan elde edilen maliyetleri en aza indirmek istersiniz.

  Sonuçları istediğiniz şekilde düzenlemek için, SQL örneğiniz içinde çalışan bir [*saklı yordam*](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) oluşturabilir ve **Select-order by** ifadesini kullanabilirsiniz. 
  Bu çözüm, sonuçlarınızın boyutu ve yapısı üzerinde daha fazla denetim sağlar. 
  Mantıksal uygulamanız, SQL Server bağlayıcısının **saklı yordamı Çalıştır** eylemini kullanarak saklı yordamı çağırır.

  Çözüm ayrıntıları için şu makalelere bakın:

  * [Logic Apps ile toplu veri aktarımı için SQL sayfalandırma](https://social.technet.microsoft.com/wiki/contents/articles/40060.sql-pagination-for-bulk-data-transfer-with-logic-apps.aspx)

  * [SELECT-ORDER BY yan tümcesi](https://docs.microsoft.com/sql/t-sql/queries/select-order-by-clause-transact-sql)

## <a name="connector-specific-details"></a>Bağlayıcıya özgü ayrıntılar

Bu bağlayıcının Tetikleyicileri, eylemleri ve limitleri hakkında teknik bilgi için [bağlayıcının başvuru ayrıntılarına](/connectors/sql/)bakın. 

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin

