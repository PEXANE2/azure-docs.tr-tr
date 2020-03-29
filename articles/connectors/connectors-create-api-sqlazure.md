---
title: SQL Server veya Azure SQL Veritabanına bağlanın
description: Azure Logic Apps'ı kullanarak sql veritabanları nın görevlerini şirket içinde veya bulutta otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam; logicappspm
ms.topic: conceptual
ms.date: 11/08/2019
tags: connectors
ms.openlocfilehash: 93b63d332f00c31a352c11e483fc3ce5cb45a922
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789213"
---
# <a name="automate-workflows-for-sql-server-or-azure-sql-database-by-using-azure-logic-apps"></a>Azure Logic Apps'ı kullanarak SQL Server veya Azure SQL Veritabanı için iş akışlarını otomatikleştirin

Bu makalede, SQL Server bağlayıcısı ile bir mantık uygulaması içinden SQL veritabanınızdaki verilere nasıl erişebileceğiniz gösterilmektedir. Bu şekilde, mantıksal uygulamalar oluşturarak SQL verilerinizi ve kaynaklarınızı yöneten görevleri, işlemleri veya iş akışlarını otomatikleştirebilirsiniz. SQL Server bağlayıcısı hem [şirket içi SQL Server](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation) hem de bulut tabanlı Azure SQL [Veritabanı](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)için çalışır.

SQL veritabanınızdaki veya Dynamics CRM Online gibi diğer sistemlerdeki olaylar tarafından tetiklendiğinde çalışan mantıksal uygulamalar oluşturabilirsiniz. Mantıksal uygulamalarınız, çalışan SQL sorguları ve depolanan yordamlarla birlikte verileri alabilir, ekleyebilir ve silebilir. Örneğin, Dynamics CRM Online'da yeni kayıtları otomatik olarak denetleyen, yeni kayıtlar için SQL veritabanınıza öğeler ekleyen ve eklenen öğeler le ilgili e-posta uyarıları gönderen bir mantık uygulaması oluşturabilirsiniz.

Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları ve](../logic-apps/logic-apps-overview.md) Quickstart nedir'yi inceleyin: İlk [mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) Bağlayıcıya özgü teknik bilgiler, sınırlamalar ve bilinen sorunlar için [SQL Server bağlayıcı başvuru sayfasına](https://docs.microsoft.com/connectors/sql/)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* [SQL Server veritabanı](https://docs.microsoft.com/sql/relational-databases/databases/create-a-database) veya Azure SQL [veritabanı](../sql-database/sql-database-get-started-portal.md)

  Mantık uygulamanızın işlemleri ararken sonuçları döndürebilmeleri için tablolarınızda veri bulunması gerekir. Bir Azure SQL Veritabanı oluşturursanız, dahil olan örnek veritabanlarını kullanabilirsiniz.

* SQL sunucu adınız, veritabanı adınız, kullanıcı adınız ve parolanız. SQL sunucunuza erişmek için mantığınızı yetkilendirmek için bu kimlik bilgilerine ihtiyacınız var.

  * SQL Server için bu ayrıntıları bağlantı dizesinde bulabilirsiniz:

    `Server={your-server-address};Database={your-database-name};User Id={your-user-name};Password={your-password};`

  * Azure SQL Veritabanı için bu ayrıntıları bağlantı dizesinde veya SQL Veritabanı özellikleri altındaki Azure portalında bulabilirsiniz:

    `Server=tcp:{your-server-name}.database.windows.net,1433;Initial Catalog={your-database-name};Persist Security Info=False;User ID={your-user-name};Password={your-password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;`

* Yerel bir bilgisayara yüklenen [şirket içi veri ağ geçidi](../logic-apps/logic-apps-gateway-install.md) ve bu senaryolar için Azure portalında oluşturulan bir Azure veri ağ geçidi [kaynağı:](../logic-apps/logic-apps-gateway-connection.md)

  * Mantıksal uygulamalarınız entegrasyon hizmeti [ortamında (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)çalışmaz.

  * Mantıksal *uygulamalarınız* bir tümleştirme hizmeti ortamında çalışır, ancak SQL Server bağlantınız için Windows kimlik doğrulamasını kullanmanız gerekir. Bu senaryo için, ISE sürümü Windows kimlik doğrulamasını desteklemediği için SQL Server bağlayıcısının Ise olmayan sürümünü ve veri ağ geçidini kullanın.

* SQL veritabanınıza erişmeniz gereken mantık uygulaması. Mantık uygulamanızı bir SQL tetikleyicisi ile başlatmak için boş bir [mantık uygulamasına](../logic-apps/quickstart-create-first-logic-app-workflow.md)ihtiyacınız var.

<a name="add-sql-trigger"></a>

## <a name="add-a-sql-trigger"></a>SQL tetikleyicisi ekleme

Azure Logic Apps'ta, her mantık uygulaması, belirli bir olay olduğunda veya belirli bir koşul yerine getirildiğinde ateş eden bir [tetikleyiciyle](../logic-apps/logic-apps-overview.md#logic-app-concepts)başlamalıdır. Tetikleyici her ateşleninardından, Logic Apps motoru bir mantık uygulaması örneği oluşturur ve mantık uygulamanızın iş akışını çalıştırmaya başlar.

1. Azure portalında veya Visual Studio'da, Logic Apps Designer'ı açan boş bir mantık uygulaması oluşturun. Bu örnekte Azure portalı kullanır.

1. Tasarımcıda, arama kutusuna filtreolarak "sql server" girin. Tetikleyiciler listesinden istediğiniz SQL tetikleyicisini seçin.

   Bu **örnekte, bir öğe ne zaman tetikleyicisi oluşturulur.**

   !["Bir öğe oluşturulduğunda" tetikleyicisi](./media/connectors-create-api-sqlazure/select-sql-server-trigger.png)

1. Bir bağlantı oluşturmanız istenirse, [SQL bağlantınızı şimdi oluşturun.](#create-connection) Bağlantınız varsa, bir **Tablo adı**seçin.

   ![İstediğiniz tabloyu seçin](./media/connectors-create-api-sqlazure/azure-sql-database-table.png)

1. Mantık uygulamanızın tabloyu ne sıklıkta denetleyişolduğunu belirten **Aralık** ve **Sıklık** özelliklerini ayarlayın.

   Bu tetikleyici, seçili tablodan yalnızca bir satır döndürür, başka bir şey değil. Diğer görevleri gerçekleştirmek için, istediğiniz görevleri gerçekleştiren diğer eylemler ekleyin. Örneğin, bu satırdaki verileri görüntülemek için, döndürülen satırdaki alanları içeren bir dosya oluşturan başka eylemler ekleyebilir ve ardından e-posta uyarıları gönderebilirsiniz. Bu bağlayıcı için diğer kullanılabilir eylemler hakkında bilgi edinmek için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/sql/)bakın.

1. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.

   Bu adım, mantık uygulamanızı Azure'da canlı olarak etkinleştirive yayınlar.

<a name="add-sql-action"></a>

## <a name="add-a-sql-action"></a>SQL eylemi ekleme

Azure Logic Apps'ta [eylem,](../logic-apps/logic-apps-overview.md#logic-app-concepts) iş akışınızda bir tetikleyici veya başka bir eylemi izleyen bir adımdır. Bu örnekte, mantık uygulaması [Yineleme tetikleyicisi](../connectors/connectors-native-recurrence.md)ile başlar ve BIR SQL veritabanından satır alan bir eylemi çağırır.

1. Azure portalında veya Visual Studio'da mantık uygulamanızı Logic Apps Designer'da açın. Bu örnekte Azure portalı kullanır.

1. SQL eylemini eklemek istediğiniz tetikleyici veya eylemin altında **Yeni adım'ı**seçin.

   ![Mantık uygulamanıza yeni bir adım ekleyin](./media/connectors-create-api-sqlazure/select-new-step-logic-app.png)

   Varolan adımlar arasında eylem eklemek için farenizi bağlanan okun üzerine taşıyın. Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Arama kutusuna **bir eylem seçin**altında filtreniz olarak "sql server" girin. Eylemler listesinden, istediğiniz SQL eylemini seçin.

   Bu örnek, tek bir kayıt alan **Satır Al** eylemini kullanır.

   ![SQL "Satır al" eylemini bul ve seç](./media/connectors-create-api-sqlazure/find-select-sql-get-row-action.png)

   Bu eylem, seçili tablodan yalnızca bir satır döndürür, başka bir şey değil. Bu satırdaki verileri görüntülemek için, döndürülen satırdaki alanları içeren bir dosya oluşturan başka eylemler ekleyebilir ve bu dosyayı bir bulut depolama hesabında depolayabilirsiniz. Bu bağlayıcı için diğer kullanılabilir eylemler hakkında bilgi edinmek için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/sql/)bakın.

1. Bir bağlantı oluşturmanız istenirse, [SQL bağlantınızı şimdi oluşturun.](#create-connection) Bağlantınız varsa, bir **Tablo adı**seçin ve istediğiniz kayıt için Satır **Kimliği'ni** girin.

   ![Tablo adı ve satır kimliği girin](./media/connectors-create-api-sqlazure/specify-table-row-id-property-value.png)

1. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.

   Bu adım, mantık uygulamanızı Azure'da canlı olarak etkinleştirive yayınlar.

<a name="create-connection"></a>

## <a name="connect-to-your-database"></a>Veritabanınıza bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to SQL Server or Azure SQL Database](../../includes/connectors-create-api-sqlazure.md)]

## <a name="handle-bulk-data"></a>Toplu verileri işleme

Bazen, bağlayıcının tüm sonuçları aynı anda döndürmemesi için çok büyük sonuç kümeleriyle çalışmanız gerekir veya sonuç kümelerinizin boyutu ve yapısı üzerinde daha iyi denetim istersiniz. Bu tür büyük sonuç kümelerini işleyebilirsiniz bazı yolları şunlardır:

* Sonuçları daha küçük kümeler olarak yönetmenize yardımcı olmak *için, pagnation'ı*açın. Daha fazla bilgi için [bkz.](../logic-apps/logic-apps-exceed-default-page-size-with-pagination.md)

* Sonuçları istediğiniz şekilde düzenleyen depolanmış bir yordam oluşturun.

  Birden çok satır alırken veya eklerken, mantık uygulamanız bu [sınırlar](../logic-apps/logic-apps-limits-and-config.md)içinde bir [*ada döngüsü*](../logic-apps/logic-apps-control-flow-loops.md#until-loop) kullanarak bu satırları yineleyebilir. Ancak, mantık uygulamanız, örneğin binlerce veya milyonlarca satır gibi çok büyük kayıt kümeleriyle çalışması gerektiğinde, veritabanına yapılan aramalardan kaynaklanan maliyetleri en aza indirmek istersiniz.

  Sonuçları istediğiniz şekilde düzenlemek için, SQL örneğinde çalışan ve **SELECT - ORDER BY** deyimini kullanan bir [*depolanmış yordam*](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) oluşturabilirsiniz. Bu çözüm, sonuçlarınızın boyutu ve yapısı üzerinde daha fazla kontrol sağlar. Mantık uygulamanız, SQL Server bağlayıcısının Saklı Yordam eylemini kullanarak depolanan **yordamı** çağırır.

  Daha fazla çözüm ayrıntıları için şu makalelere bakın:

  * [Logic Apps ile toplu veri aktarımı için SQL Pagination](https://social.technet.microsoft.com/wiki/contents/articles/40060.sql-pagination-for-bulk-data-transfer-with-logic-apps.aspx)

  * [SELECT - Yan Tümceye GÖRE SİPARİş](https://docs.microsoft.com/sql/t-sql/queries/select-order-by-clause-transact-sql)

## <a name="connector-specific-details"></a>Bağlayıcıya özel ayrıntılar

Bu bağlayıcının tetikleyicileri, eylemleri ve sınırları hakkında teknik bilgi için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/sql/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Mantık Uygulamaları için diğer [bağlayıcılar](../connectors/apis-list.md) hakkında bilgi edinin
