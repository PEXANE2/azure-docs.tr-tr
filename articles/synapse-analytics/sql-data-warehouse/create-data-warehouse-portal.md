---
title: SYNAPSE SQL havuzu oluşturma ve sorgulama (Azure portal)
description: Azure portal kullanarak bir Synapse SQL havuzu oluşturma ve sorgulama
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: sql-dw
ms.date: 05/28/2019
ms.author: Kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: c7efccf35ddb47bf349d9a21628e9445e37b7699
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85211315"
---
# <a name="quickstart-create-and-query-a-synapse-sql-pool-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak bir Synapse SQL havuzu oluşturma ve sorgulama

Azure portal kullanarak Azure SYNAPSE Analytics 'te (eski adıyla SQL DW) bir Synapse SQL havuzunu (veri ambarı) hızlıca oluşturun ve sorgulayın.

## <a name="prerequisites"></a>Ön koşullar

1. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

   > [!NOTE]
   > Azure SYNAPSE 'de bir SQL havuzu oluşturmak, yeni bir faturalanabilir hizmetle sonuçlanabilir. Daha fazla bilgi için bkz. [Azure SYNAPSE Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/synapse-analytics/).

2. [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)’nun (SSMS) en yeni sürümünü indirin ve yükleyin.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-sql-pool"></a>SQL havuzu oluşturma

Veri ambarları, Azure SYNAPSE Analytics 'te SQL havuzu kullanılarak oluşturulur. Bir SQL havuzu tanımlanmış bir [işlem kaynakları](memory-concurrency-limits.md)kümesiyle oluşturulur. Veritabanı bir [Azure Kaynak grubu](../../azure-resource-manager/management/overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) içinde ve [mantıksal SQL Server](../../azure-sql/database/logical-servers.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)'da oluşturulur.

**AdventureWorksDW** örnek verilerini IÇEREN bir SQL havuzu oluşturmak için bu adımları izleyin.

1. Azure portal sol üst köşesinde **kaynak oluştur** ' u seçin.

   ![Azure portal bir kaynak oluşturun](./media/create-data-warehouse-portal/create-a-resource.png)

2. **Yeni** sayfada **veritabanları** ' nı seçin ve **öne çıkan** listede **Azure SYNAPSE Analytics (eski adıyla SQL DW)** öğesini seçin.

   ![boş veri ambarı oluşturma](./media/create-data-warehouse-portal/create-a-data-warehouse.png)

3. **Temel bilgiler**için aboneliğinizi, kaynak grubunuzu, SQL havuzu adınızı ve sunucu adını sağlayın:

   | Ayar | Önerilen değer | Description |
   | :------ | :-------------- | :---------- |
   | **Abonelik** | Aboneliğiniz | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
   | **Kaynak grubu** | myResourceGroup | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). |
   | **SQL havuzu adı** | Herhangi bir genel benzersiz ad (örneğin, *Mysampledatawarehouse*) | Geçerli veritabanı adları için bkz. [Veritabanı Tanımlayıcıları](/sql/relational-databases/databases/database-identifiers?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest). SQL havuzu tek bir veritabanı türüdür. |
   | **Sunucu** | Genel olarak benzersiz bir ad | Var olan sunucuyu seçin veya yeni bir sunucu adı oluşturun, **Yeni oluştur**' u seçin. Geçerli sunucu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). |

   ![veri ambarı temel ayrıntıları oluşturma](./media/create-data-warehouse-portal/create-sql-pool-basics.png)

4. **Performans düzeyi**altında, isteğe bağlı olarak yapılandırmanızı bir kaydırıcı ile değiştirmek için **performans düzeyi seç** ' i seçin.

   ![veri ambarı performans düzeyini değiştirme](./media/create-data-warehouse-portal/create-sql-pool-performance-level.png)  

   Performans düzeyleri hakkında daha fazla bilgi için bkz. [Azure SYNAPSE Analytics 'te Işlem yönetme](sql-data-warehouse-manage-compute-overview.md).

5. **Ek ayarlar**' ı seçin, **var olan verileri kullan**bölümünde örnek veritabanı olarak AdventureWorksDW oluşturulacak şekilde **Sample** ' ı seçin.

    ![mevcut verileri kullan seçeneğini belirleyin](./media/create-data-warehouse-portal/create-sql-pool-additional-1.png) 

6. Azure SYNAPSE Analytics formunun temel bilgiler sekmesini tamamladığınıza göre, SQL havuzunu oluşturmak için **gözden geçir + oluştur** ' u ve ardından **Oluştur** ' u seçin. Sağlama birkaç dakika sürer.

   ![Gözden geçir + oluştur ' u seçin](./media/create-data-warehouse-portal/create-sql-pool-review-create.png)

   ![oluştur’u seçin](./media/create-data-warehouse-portal/create-sql-pool-create.png)

7. Araç çubuğunda, dağıtım sürecini izlemek için **Bildirimler** ' i seçin.

   ![bildirim](./media/create-data-warehouse-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

Azure SYNAPSE hizmeti, sunucu düzeyinde bir güvenlik duvarı oluşturur. Bu güvenlik duvarı, dış uygulamaların ve araçların sunucuya veya sunucudaki herhangi bir veritabanına bağlanmasını engeller. Bağlantıyı etkinleştirmek için, belirli IP adresleri için bağlantıyı etkinleştiren güvenlik duvarı kuralları ekleyebilirsiniz. İstemcinizin IP adresine yönelik bir [sunucu düzeyi güvenlik duvarı kuralı](../../azure-sql/database/firewall-configure.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) oluşturmak için bu adımları izleyin.

> [!NOTE]
> Azure SYNAPSE, 1433 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, BT departmanınız 1433 numaralı bağlantı noktasını açmadığı takdirde sunucunuza bağlanamazsınız.

1. Dağıtım tamamlandıktan sonra, sol taraftaki menüden **tüm hizmetler** ' i seçin. **Veritabanları**' nı seçin, Azure **SYNAPSE Analytics** ' in yanındaki yıldızı seçerek sık kullanılanlarınıza Azure SYNAPSE Analytics 'i ekleyin.

2. Sol taraftaki menüden **Azure SYNAPSE Analytics** ' i seçin ve ardından **Azure SYNAPSE Analytics** sayfasında **mysampledatawarehouse** ' ı seçin. Veritabanınızın genel bakış sayfası açılır ve tam sunucu adı (örneğin, **sqlpoolservername.Database.Windows.net**) görüntülenerek daha fazla yapılandırma seçeneği sunulur.

3. Bu tam sunucu adını, bu ve diğer hızlı başlangıçtaki sunucunuza ve veritabanlarına bağlanmak için kullanmak üzere kopyalayın. Sunucu ayarlarını açmak için sunucu adını seçin.

   ![sunucu adını bulma](./media/create-data-warehouse-portal/find-server-name.png)

4. **Güvenlik Duvarı ayarlarını göster**' i seçin.

   ![sunucu ayarları](./media/create-data-warehouse-portal/server-settings.png)

5. Sunucu için **güvenlik duvarı ayarları** sayfası açılır.

   ![sunucu güvenlik duvarı kuralı](./media/create-data-warehouse-portal/server-firewall-rule.png)

6. Geçerli IP adresinizi yeni bir güvenlik duvarı kuralına eklemek için araç çubuğunda **istemci IP 'Si Ekle** ' yi seçin. Güvenlik duvarı kuralı, 1433 numaralı bağlantı noktasını tek bir IP adresi veya bir IP adresi aralığı için açabilir.

7. **Kaydet**' i seçin. Sunucuda 1433 numaralı bağlantı noktasını açan geçerli IP adresiniz için sunucu düzeyinde bir güvenlik duvarı kuralı oluşturulur.

8. **Tamam** ' ı seçin ve ardından **güvenlik duvarı ayarları** sayfasını kapatın.

Artık bu IP adresini kullanarak sunucuya ve SQL havuzlarına bağlanabilirsiniz. Bağlantı SQL Server Management Studio’dan veya seçtiğiniz diğer bir araçtan çalışır. Bağlandığınızda, daha önce oluşturduğunuz ServerAdmin hesabını kullanın.

> [!IMPORTANT]
> Varsayılan olarak, SQL Veritabanı güvenlik duvarı üzerinden erişim tüm Azure hizmetleri için etkindir. Bu sayfada **kapalı** ' yı seçin ve ardından **Kaydet** ' i seçerek tüm Azure hizmetleri için güvenlik duvarını devre dışı bırakın.

## <a name="get-the-fully-qualified-server-name"></a>Tam sunucu adını alma

Azure portal sunucunuzun tam sunucu adını alın. Daha sonra sunucuya bağlanırken tam adı kullanırsınız.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Sol taraftaki menüden **Azure SYNAPSE Analytics** ' i seçin ve **Azure SYNAPSE Analytics** sayfasında hesabınızı seçin.

3. Veritabanınızın Azure portal sayfasındaki **Temel Bilgiler** bölmesinde, **Sunucu adını** bulup kopyalayın. Bu örnekte, tam adı sqlpoolservername.database.windows.net ' dir.

    ![bağlantı bilgileri](./media/create-data-warehouse-portal/find-server-name-copy.png)

## <a name="connect-to-the-server-as-server-admin"></a>Sunucu yöneticisi olarak sunucuya bağlanma

Bu bölüm, sunucunuza bağlantı kurmak için [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) (SSMS) kullanır.

1. SQL Server Management Studio’yu açın.

2. **Sunucuya Bağlan** iletişim kutusuna şu bilgileri girin:

   | Ayar | Önerilen değer | Description |
   | :------ | :-------------- | :---------- |
   | Sunucu türü | Veritabanı altyapısı | Bu değer gereklidir |
   | Sunucu adı | Tam sunucu adı | Örnek: **sqlpoolservername.Database.Windows.net**. |
   | Kimlik doğrulaması | SQL Server Kimlik Doğrulaması | Bu öğreticide yapılandırılan tek kimlik doğrulaması türü SQL Kimlik Doğrulamasıdır. |
   | Oturum aç | Sunucu yöneticisi hesabı | Sunucuyu oluştururken belirttiğiniz hesap. |
   | Parola | Sunucu yöneticisi hesabınızın parolası | Sunucuyu oluştururken belirttiğiniz parola. |
   ||||

   ![sunucuya bağlan](./media/create-data-warehouse-portal/connect-to-server-ssms.png)

3. **Bağlan**' ı seçin. SSMS’te Nesne Gezgini penceresi açılır.

4. Nesne Gezgini’nde, **Veritabanları**’nı genişletin. Daha sonra yeni veritabanınızdaki nesneleri görüntülemek için **mySampleDatabase**’i genişletin.

   ![veritabanı nesneleri](./media/create-data-warehouse-portal/connected-ssms.png)

## <a name="run-some-queries"></a>Sorgular çalıştırma

[Sınırlı bir kaynak sınıfı](resource-classes-for-workload-management.md)kullandığından, sunucu yöneticisi olarak günlüğe yazılırken büyük sorguların çalıştırılması önerilmez. Bunun yerine [, öğreticilerde gösterildiği](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/load-data-wideworldimportersdw#create-a-user-for-loading-data)gibi [iş yükü yalıtımını](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/quickstart-configure-workload-isolation-tsql) yapılandırın.

SQL Veri Ambarı sorgu dili olarak T-SQL kullanır. Bir sorgu penceresi açıp T-SQL sorguları çalıştırmak için, aşağıdaki adımları kullanın:

1. **Mysampledatawarehouse** öğesine sağ tıklayın ve **Yeni sorgu**' yu seçin. Yeni bir sorgu penceresi açılır.

2. Sorgu penceresinde, veritabanlarının listesini görmek için aşağıdaki komutu girin.

    ```sql
    SELECT * FROM sys.databases
    ```

3. **Yürüt**' ü seçin. Sorgu sonuçları iki veritabanı gösterir: **master** ve **mySampleDataWarehouse**.

   ![Sorgu veritabanları](./media/create-data-warehouse-portal/query-databases.png)

4. Verilere bakmak için, Adams soyadına sahip ve üç çocuğu olan müşteri sayısını görmek için aşağıdaki komutu kullanın. Sonuçlarda altı müşteri listelenir.

    ```sql
    SELECT LastName, FirstName FROM dbo.dimCustomer
    WHERE LastName = 'Adams' AND NumberChildrenAtHome = 3;
    ```

   ![dbo.dimCustomer’ı sorgulama](./media/create-data-warehouse-portal/query-customer.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Veri ambarı birimleri ve SQL havuzunuzu depolanan veriler için ücret ödersiniz. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır.

- Verileri depolama alanında tutmak istiyorsanız, SQL havuzunu kullanmadığınız sırada işlem duraklatabilirsiniz. İşlem duraklatıldığında yalnızca veri depolama alanı için ücret ödersiniz. Verilerle çalışmaya her seferinde işlem yapabilirsiniz.

- Gelecekteki ücretleri kaldırmak istiyorsanız, SQL havuzunu silebilirsiniz.

Artık ihtiyacınız olmayan kaynakları temizlemek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com)oturum açın, SQL havuzunuzu seçin.

   ![Kaynakları temizleme](./media/create-data-warehouse-portal/clean-up-resources.png)

2. İşlem duraklatmak için **Duraklat** düğmesini seçin. SQL havuzu duraklatıldığında **, bir geri** düğmesi görürsünüz. İşlemi sürdürmesini sağlamak için, yeniden gerçekleştir ' **i seçin.**

3. İşlem veya depolama için ücretlendirilmemek üzere SQL havuzunu kaldırmak için **Sil**' i seçin.

4. Oluşturduğunuz sunucuyu kaldırmak için önceki görüntüde **sqlpoolservername.Database.Windows.net** ' ı seçin ve **Sil**' i seçin. Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

5. Kaynak grubunu kaldırmak için **Myresourcegroup**' ı seçin ve **kaynak grubunu sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

SQL havuzunuza veri yükleme hakkında daha fazla bilgi edinmek için [VERILERI SQL havuzu 'Na yükleme](load-data-from-azure-blob-storage-using-polybase.md) makalesine ilerleyin.
