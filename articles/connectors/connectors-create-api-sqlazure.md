---
title: SQL Server, Azure SQL veritabanı veya Azure SQL yönetilen örneği 'ne bağlanma
description: Azure Logic Apps kullanarak şirket içinde veya bulutta SQL veritabanları için görevleri otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, jonfan, logicappspm
ms.topic: conceptual
ms.date: 06/06/2020
tags: connectors
ms.openlocfilehash: ba8a6e5b53634850670a7d6b2fb55ef0e7b18d09
ms.sourcegitcommit: bf99428d2562a70f42b5a04021dde6ef26c3ec3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85255526"
---
# <a name="automate-workflows-for-a-sql-database-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak bir SQL veritabanı için iş akışlarını otomatikleştirme

Bu makalede, SQL veritabanınızdaki verilere SQL Server Bağlayıcısı ile bir mantıksal uygulama içinden nasıl erişebileceğiniz gösterilmektedir. Bu şekilde, mantıksal uygulamalar oluşturarak SQL verilerinizi ve kaynaklarınızı yöneten görevleri, işlemleri veya iş akışlarını otomatikleştirebileceğinizi unutmayın. SQL Server Bağlayıcısı, [Azure SQL veritabanı](../azure-sql/database/sql-database-paas-overview.md) ve [Azure SQL yönetilen örneği](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md)ile [SQL Server](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation) için de kullanılabilir.

SQL veritabanınızda veya Dynamics CRM Online gibi diğer sistemlerde olaylar tarafından tetiklendiğinde çalışan Logic Apps oluşturabilirsiniz. Mantıksal uygulamalarınız, çalışan SQL sorguları ve saklı yordamlar ile birlikte verileri de alabilir, ekleyebilir ve silebilir. Örneğin, Dynamics CRM Online 'da yeni kayıtları otomatik olarak denetleyen bir mantıksal uygulama oluşturabilir, her yeni kayıt için SQL veritabanınıza öğe ekler ve ardından eklenen öğeler hakkında e-posta uyarıları gönderir.

Logic Apps 'e yeni başladıysanız, [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: Ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)' yı gözden geçirin. Bağlayıcıya özgü teknik bilgiler, sınırlamalar ve bilinen sorunlar için, [SQL Server Bağlayıcısı başvuru sayfasına](https://docs.microsoft.com/connectors/sql/)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* [SQL Server veritabanı](https://docs.microsoft.com/sql/relational-databases/databases/create-a-database), [Azure SQL VERITABANı](../azure-sql/database/single-database-create-quickstart.md)veya [Azure SQL yönetilen örneği](../azure-sql/managed-instance/instance-create-quickstart.md).

  Mantıksal uygulamanızın işlemleri çağırırken sonuçlar döndürebilmesi için tablolarınızın verileri olmalıdır. Azure SQL veritabanı kullanıyorsanız, dahil edilen örnek veritabanlarını kullanabilirsiniz.

* SQL sunucunuzun adı, veritabanı adı, Kullanıcı adınız ve parolanız. Mantığınızı SQL Server 'a erişim için yetkilendirebilmeniz için bu kimlik bilgilerine ihtiyacınız vardır.

  * Şirket içi SQL Server, bu ayrıntıları bağlantı dizesinde bulabilirsiniz:

    `Server={your-server-address};Database={your-database-name};User Id={your-user-name};Password={your-password};`

  * Azure SQL veritabanı için bu ayrıntıları bağlantı dizesinde bulabilirsiniz.
  
    Örneğin, bu dizeyi Azure portal bulmak için veritabanınızı açın. Veritabanı menüsünde, **bağlantı dizeleri** veya **Özellikler**' i seçin:

    `Server=tcp:{your-server-name}.database.windows.net,1433;Initial Catalog={your-database-name};Persist Security Info=False;User ID={your-user-name};Password={your-password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;`

<a name="multi-tenant-or-ise"></a>

* Mantıksal uygulamalarınızın küresel, çok kiracılı Azure veya bir [tümleştirme hizmeti ortamında (ıSE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)çalışmasına bağlı olarak, şirket içi SQL Server bağlanmak için diğer gereksinimler şunlardır:

  * Şirket içi SQL Server bağlanan küresel, çok kiracılı Azure 'da mantıksal uygulamalar için, Şirket [içi veri ağ geçidinin](../logic-apps/logic-apps-gateway-install.md) yerel bir bilgisayarda yüklü olması ve [Azure 'da zaten oluşturulmuş bir veri ağ geçidi kaynağı](../logic-apps/logic-apps-gateway-connection.md)olması gerekir.

  * Şirket içi SQL Server bağlanan ve Windows kimlik doğrulaması kullanan bir ıSE içindeki Logic Apps için, ıSE sürümlü SQL Server Bağlayıcısı Windows kimlik doğrulamasını desteklemez. Bu nedenle, hala Data Gateway ve ıSE SQL Server bağlayıcısını kullanmanız gerekir. Diğer kimlik doğrulama türleri için, veri ağ geçidini kullanmanız gerekmez ve ıSE sürümlü bağlayıcıyı kullanabilirsiniz.

* SQL veritabanınıza erişmeniz gereken mantıksal uygulama. Mantıksal uygulamanızı bir SQL tetikleyicisi ile başlatmak için [boş bir mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md)gerekir.

<a name="create-connection"></a>

## <a name="connect-to-your-database"></a>Veritabanınıza bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

Şimdi şu adımlarla devam edin:

* [Bulut tabanlı Azure SQL veritabanı veya yönetilen örneğe bağlanma](#connect-azure-sql-db)
* [Şirket içi SQL Server bağlanma](#connect-sql-server)

<a name="connect-azure-sql-db"></a>

### <a name="connect-to-azure-sql-database-or-managed-instance"></a>Azure SQL veritabanı veya yönetilen örnek 'e bağlanma

İlk kez bir [SQL tetikleyicisi](#add-sql-trigger) ya da [SQL eylemi](#add-sql-action)eklediğinizde ve daha önce veritabanınıza bir bağlantı oluşturmadıysanız, şu adımları gerçekleştirmeniz istenir:

1. **Kimlik doğrulama türü**Için, Azure SQL veritabanı veya Azure SQL yönetilen örneği 'nde gerekli ve etkinleştirilmiş olan kimlik doğrulamasını seçin:

   | Kimlik doğrulaması | Description |
   |----------------|-------------|
   | [**Azure AD Tümleşik**](../azure-sql/database/authentication-aad-overview.md) | -Hem ıSE hem de ıSE SQL Server bağlayıcısını destekler. <p><p>-Azure Active Directory (Azure AD) içinde veritabanınıza erişimi olan geçerli bir kimlik gerektirir. <p>Daha fazla bilgi için şu konulara bakın: <p>- [Azure SQL güvenliğine genel bakış-kimlik doğrulama](../azure-sql/database/security-overview.md#authentication) <br>- [Azure SQL kimlik doğrulaması ve yetkilendirmesi için veritabanı erişimini yetkilendir](../azure-sql/database/logins-create-manage.md#authentication-and-authorization) <br>- [Azure SQL-Azure AD Tümleşik kimlik doğrulaması](../azure-sql/database/authentication-aad-overview.md) |
   | [**SQL Server Kimlik Doğrulaması**](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode#connecting-through-sql-server-authentication) | -Hem ıSE hem de ıSE SQL Server bağlayıcısını destekler. <p><p>-Veritabanınıza oluşturulup depolanan geçerli bir Kullanıcı adı ve güçlü bir parola gerektirir. <p>Daha fazla bilgi için şu konulara bakın: <p>- [Azure SQL güvenliğine genel bakış-kimlik doğrulama](../azure-sql/database/security-overview.md#authentication) <br>- [Azure SQL kimlik doğrulaması ve yetkilendirmesi için veritabanı erişimini yetkilendir](../azure-sql/database/logins-create-manage.md#authentication-and-authorization) |
   |||

   Bu örnek **Azure AD Tümleşik**ile devam eder:

   ![Kullanılacak kimlik doğrulaması türünü seçin](./media/connectors-create-api-sqlazure/select-azure-ad-authentication.png)

1. **Azure AD Tümleşik**seçeneğini belirledikten sonra **oturum aç**' ı seçin. Azure SQL veritabanı veya Azure SQL yönetilen örneği kullanmanıza bağlı olarak, kimlik doğrulaması için Kullanıcı kimlik bilgilerinizi seçin.

1. Veritabanınız için şu değerleri seçin:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Sunucu adı** | Yes | SQL sunucunuzun adresi, örneğin,`Fabrikam-Azure-SQL.database.windows.net` |
   | **Veritabanı adı** | Yes | SQL veritabanınızın adı, örneğin`Fabrikam-Azure-SQL-DB` |
   | **Tablo adı** | Yes | Kullanmak istediğiniz tablo, örneğin,`SalesLT.Customer` |
   ||||

   > [!TIP]
   > Bu bilgileri veritabanınızın bağlantı dizesinde bulabilirsiniz. Örneğin, Azure portal, veritabanınızı bulun ve açın. Veritabanı menüsünde, **bağlantı dizeleri** veya bu dizeyi bulabileceğiniz **Özellikler** ' i seçin:
   >
   > `Server=tcp:{your-server-address}.database.windows.net,1433;Initial Catalog={your-database-name};Persist Security Info=False;User ID={your-user-name};Password={your-password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;`

   Bu örnekte bu değerlerin nasıl görünebileceğini gösterilmektedir:

   ![SQL veritabanı bağlantısı oluşturma](./media/connectors-create-api-sqlazure/azure-sql-database-create-connection.png)

1. Şimdi, [BIR SQL tetikleyicisi ekleyip](#add-sql-trigger) veya [bir SQL eylemi ekleyerek](#add-sql-action)henüz tamamlamadığınızı adımlara devam edin.

<a name="connect-sql-server"></a>

### <a name="connect-to-on-premises-sql-server"></a>Şirket içi SQL Server bağlanma

İlk kez bir [SQL tetikleyicisi](#add-sql-trigger) ya da [SQL eylemi](#add-sql-action)eklediğinizde ve daha önce veritabanınıza bir bağlantı oluşturmadıysanız, şu adımları gerçekleştirmeniz istenir:

1. Şirket içi veri ağ geçidini gerektiren şirket içi SQL Server 'a bağlantılar için, [bu önkoşulları tamamladığınızdan](#multi-tenant-or-ise)emin olun.

   Aksi halde, bağlantınızı oluştururken veri ağ geçidi kaynağınız **bağlantı ağ geçidi** listesinde görünmez.

1. **Kimlik doğrulama türü**için, gerekli olan ve SQL Server etkin olan kimlik doğrulamasını seçin:

   | Kimlik doğrulaması | Description |
   |----------------|-------------|
   | [**Windows Kimlik Doğrulaması**](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode#connecting-through-windows-authentication) | -Çok kiracılı Azure veya bir ıSE kullanıp kullanmadığına bakılmaksızın, bağlantınız için daha önce Azure 'da oluşturulmuş bir veri ağ geçidi kaynağı gerektiren yalnızca ıSE SQL Server bağlayıcısını destekler. <p><p>-Windows hesabınız aracılığıyla kimliğinizi doğrulamak için geçerli bir Windows Kullanıcı adı ve parola gerektirir. <p>Daha fazla bilgi için bkz. [Windows kimlik doğrulaması](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode#connecting-through-windows-authentication) |
   | [**SQL Server Kimlik Doğrulaması**](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode#connecting-through-sql-server-authentication) | -Hem ıSE hem de ıSE SQL Server bağlayıcısını destekler. <p><p>-SQL Server oluşturulup depolanan geçerli bir Kullanıcı adı ve güçlü bir parola gerektirir. <p>Daha fazla bilgi için [SQL Server kimlik doğrulaması](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode#connecting-through-sql-server-authentication)konusuna bakın. |
   |||

   Bu örnek, **Windows kimlik doğrulamasıyla**devam eder:

   ![Kullanılacak kimlik doğrulaması türünü seçin](./media/connectors-create-api-sqlazure/select-windows-authentication.png)

1. SQL veritabanınız için aşağıdaki değerleri seçin veya sağlayın:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **SQL Server adı** | Yes | SQL sunucunuzun adresi, örneğin,`Fabrikam-Azure-SQL.database.windows.net` |
   | **SQL veritabanı adı** | Yes | SQL Server veritabanınızın adı, örneğin`Fabrikam-Azure-SQL-DB` |
   | **Nitelen** | Yes | SQL Server ve veritabanı için Kullanıcı adınız |
   | **Parola** | Yes | SQL Server ve veritabanı için parolanız |
   | **Abonelik** |  Evet, Windows kimlik doğrulaması için | Daha önce Azure 'da oluşturduğunuz veri ağ geçidi kaynağı için Azure aboneliği |
   | **Bağlantı ağ geçidi** | Evet, Windows kimlik doğrulaması için | Daha önce Azure 'da oluşturduğunuz veri ağ geçidi kaynağının adı <p><p>**İpucu**: ağ geçidiniz listede görünmüyorsa, [ağ geçidinizin](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection)doğru şekilde ayarlandığından emin olun. |
   |||

   > [!TIP]
   > Bu bilgileri veritabanınızın bağlantı dizesinde bulabilirsiniz:
   > 
   > * `Server={your-server-address}`
   > * `Database={your-database-name}`
   > * `User ID={your-user-name}`
   > * `Password={your-password}`

   Bu örnekte bu değerlerin nasıl görünebileceğini gösterilmektedir:

   ![SQL Server bağlantı oluşturma tamamlandı](./media/connectors-create-api-sqlazure/sql-server-create-connection-complete.png)

1. Hazırsanız **Oluştur**' u seçin.

1. Şimdi, [BIR SQL tetikleyicisi ekleyip](#add-sql-trigger) veya [bir SQL eylemi ekleyerek](#add-sql-action)henüz tamamlamadığınızı adımlara devam edin.

<a name="add-sql-trigger"></a>

## <a name="add-a-sql-trigger"></a>SQL tetikleyicisi ekleme

1. [Azure Portal](https://portal.azure.com) veya Visual Studio 'Da mantıksal uygulama Tasarımcısı ' nı açan boş bir mantıksal uygulama oluşturun. Bu örnek Azure portal devam eder.

1. Tasarımcıda arama kutusuna girin `sql server` . Tetikleyiciler listesinden istediğiniz SQL tetikleyicisini seçin. Bu örnek, **bir öğe oluşturulduğunda** tetikleyicisi kullanır.

   !["Bir öğe oluşturulduğunda" tetikleyicisi seçin](./media/connectors-create-api-sqlazure/select-sql-server-trigger.png)

1. SQL veritabanınıza ilk kez bağlanıyorsanız, [ŞIMDI SQL Veritabanı bağlantınızı oluşturmanız](#create-connection)istenir. Bu bağlantıyı oluşturduktan sonra, bir sonraki adımla devam edebilirsiniz.

1. Tetikleyicide, tetikleyicinin tabloyu ne sıklıkta denetleyeceğini belirten aralığı ve sıklığı belirtin.

1. Bu tetikleyicinin kullanılabilir diğer özelliklerini eklemek için **yeni parametre Ekle** listesini açın.

   Bu tetikleyici seçili tablodan yalnızca bir satır döndürür ve başka hiçbir şey yapmaz. Diğer görevleri gerçekleştirmek için, mantıksal uygulama iş akışınızda istediğiniz sonraki görevi gerçekleştiren bir [SQL Bağlayıcısı eylemi](#add-sql-action) ya da [başka bir eylem](../connectors/apis-list.md) ekleyerek devam edin.
   
   Örneğin, bu satırdaki verileri görüntülemek için döndürülen satırdaki alanları içeren bir dosya oluşturan başka eylemler ekleyebilir ve ardından e-posta uyarıları gönderebilirsiniz. Bu bağlayıcıya yönelik diğer mevcut eylemler hakkında bilgi edinmek için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/sql/)bakın.

1. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

   Bu adım, mantıksal uygulamanızı Azure 'da otomatik olarak etkinleştirse ve yayımladığında, mantıksal uygulamanızın Şu anda aldığı tek eylem, veritabanınızı belirtilen aralığa ve sıklığa göre denetlemenize bağlıdır.

<a name="add-sql-action"></a>

## <a name="add-a-sql-action"></a>SQL eylemi ekleme

Bu örnekte, mantıksal uygulama [yinelenme tetikleyicisiyle](../connectors/connectors-native-recurrence.md)başlar ve bir SQL veritabanından bir satır alan bir eylem çağırır.

1. [Azure Portal](https://portal.azure.com) veya Visual Studio 'Da mantıksal uygulama Tasarımcısı 'nda mantıksal uygulamanızı açın. Bu örnek Azure portal devam eder.

1. SQL eylemini eklemek istediğiniz tetikleyici veya eylem altında **yeni adım**' ı seçin.

   ![Mantıksal uygulamanıza eylem ekleme](./media/connectors-create-api-sqlazure/select-new-step-logic-app.png)

   Ya da varolan adımlar arasında bir eylem eklemek için farenizi bağlantı oku üzerine taşıyın. Görüntülenen artı işaretini ( **+** ) seçin ve ardından **Eylem Ekle**' yi seçin.

1. **Eylem seçin**altında, arama kutusuna girin `sql server` . Eylemler listesinden istediğiniz SQL eylemini seçin. Bu örnek, tek bir kayıt alan **satırı al** eylemini kullanır.

   ![SQL "satırı al" eylemini seçin](./media/connectors-create-api-sqlazure/select-sql-get-row-action.png)

1. SQL veritabanınıza ilk kez bağlanıyorsanız, [ŞIMDI SQL Veritabanı bağlantınızı oluşturmanız](#create-connection)istenir. Bu bağlantıyı oluşturduktan sonra, bir sonraki adımla devam edebilirsiniz.

1. Bu örnekteki **tablo adını**seçin `SalesLT.Customer` . İstediğiniz kayıt için **satır kimliğini** girin.

   ![Tablo adı seçin ve satır KIMLIĞINI belirtin](./media/connectors-create-api-sqlazure/specify-table-row-id.png)

   Bu eylem, seçili tablodan yalnızca bir satır döndürür, başka hiçbir şey yapmaz. Bu nedenle, bu satırdaki verileri görüntülemek için döndürülen satırdaki alanları içeren bir dosya oluşturan ve bu dosyayı bir bulut depolama hesabında depolayabilen başka eylemler ekleyebilirsiniz. Bu bağlayıcıya yönelik diğer mevcut eylemler hakkında bilgi edinmek için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/sql/)bakın.

1. İşiniz bittiğinde, Tasarımcı araç çubuğunda **Kaydet**' i seçin.

   Bu adım otomatik olarak Azure 'da mantıksal uygulamanızı etkin bir şekilde etkinleştirilir ve yayımlar.

## <a name="handle-bulk-data"></a>Toplu verileri işle

Bazen, bağlayıcının tüm sonuçları aynı anda döndürmemesi veya sonuç kümelerinizin boyutu ve yapısı üzerinde daha iyi denetim sağlamak istiyorsanız sonuç kümeleriyle çalışmanız gerekir. Bu tür büyük sonuç kümelerini işleyebilmeniz için bazı yollar şunlardır:

* Sonuçları daha küçük kümeler olarak yönetmenize yardımcı olmak için *sayfalandırma*'yı açın. Daha fazla bilgi için bkz. [sayfalandırma kullanarak toplu verileri, kayıtları ve öğeleri edinme](../logic-apps/logic-apps-exceed-default-page-size-with-pagination.md).

* Sonuçları istediğiniz şekilde düzenleyen bir saklı yordam oluşturun.

  Birden çok satır alırken veya eklerken, mantıksal uygulamanız bu [sınırlar](../logic-apps/logic-apps-limits-and-config.md)dahilinde bir [*until döngüsü*](../logic-apps/logic-apps-control-flow-loops.md#until-loop) kullanarak bu satırları yineleyebilir. Ancak, mantıksal uygulamanızın kayıt kümeleriyle birlikte çalışması gerektiğinde, büyük bir deyişle, binlerce veya milyonlarca satır olmak üzere veritabanına yapılan çağrılardan elde edilen maliyetleri en aza indirmek istersiniz.

  Sonuçları istediğiniz şekilde düzenlemek için, SQL örneğiniz içinde çalışan bir [*saklı yordam*](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) oluşturabilir ve **Select-order by** ifadesini kullanabilirsiniz. Bu çözüm, sonuçlarınızın boyutu ve yapısı üzerinde daha fazla denetim sağlar. Mantıksal uygulamanız, SQL Server bağlayıcısının **saklı yordamı Çalıştır** eylemini kullanarak saklı yordamı çağırır.

  Daha fazla çözüm ayrıntısı için şu makalelere bakın:

  * [Logic Apps ile toplu veri aktarımı için SQL sayfalandırma](https://social.technet.microsoft.com/wiki/contents/articles/40060.sql-pagination-for-bulk-data-transfer-with-logic-apps.aspx)

  * [SELECT-ORDER BY yan tümcesi](https://docs.microsoft.com/sql/t-sql/queries/select-order-by-clause-transact-sql)

### <a name="handle-dynamic-bulk-data"></a>Dinamik toplu verileri işle

SQL Server bağlayıcısını kullanarak bir saklı yordamı çağırdığınızda döndürülen çıkış bazen dinamik olur. Bu senaryoda, aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. Bir test çalıştırması gerçekleştirerek çıkış biçimini görüntüleyin. Örnek çıktılarınızı kopyalayın ve kaydedin.

1. Tasarımcıda, saklı yordamı çağırdığınız eylemin altında **yeni adım**' ı seçin.

1. **Eylem seçin**altında [**JSON 'u Ayrıştır**](../logic-apps/logic-apps-perform-data-operations.md#parse-json-action) eylemini bulun ve seçin.

1. JSON 'u **Ayrıştır** eyleminde **şema oluşturmak Için örnek yük kullan**' ı seçin.

1. **Örnek JSON yükü girin veya yapıştırın** kutusunda, örnek çıktlarınızı yapıştırın ve **bitti**' yi seçin.

   > [!NOTE]
   > Logic Apps şema oluşturmadığının bir hata alırsanız, örnek çıktısının sözdiziminin doğru biçimlendirildiğinden emin olun. Şemayı yine de üretemiyor, **şema** kutusunda şemayı el ile girin.

1. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

1. JSON içerik özelliklerine başvurmak için, dinamik içerik listesinin görünmesi için bu özelliklere başvurmak istediğiniz düzenleme kutularının içine tıklayın. Listede, [**JSON başlığını Ayrıştır**](../logic-apps/logic-apps-perform-data-operations.md#parse-json-action) başlığı altında istediğiniz JSON içerik özellikleri için veri belirteçlerini seçin.

## <a name="connector-specific-details"></a>Bağlayıcıya özgü ayrıntılar

Bu bağlayıcının Tetikleyicileri, eylemleri ve limitleri hakkında teknik bilgi için, Swagger açıklasından oluşturulan [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/sql/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps için diğer bağlayıcılar](../connectors/apis-list.md) hakkında bilgi edinin
