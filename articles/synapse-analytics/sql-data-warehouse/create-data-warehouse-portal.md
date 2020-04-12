---
title: Synapse SQL havuzu oluşturma ve sorgulama (Azure portalı)
description: Azure portalını kullanarak bir Synapse SQL havuzu oluşturma ve sorgula
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 05/28/2019
ms.author: Kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 9605d20fa6a1480b24d7b64963aa9579ed3b5a11
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115174"
---
# <a name="quickstart-create-and-query-a-synapse-sql-pool-using-the-azure-portal"></a>Hızlı başlatma: Azure portalını kullanarak bir Synapse SQL havuzu oluşturma ve sorgula

Azure portalını kullanarak Azure Synapse Analytics'te (eski adıyla SQL DW) bir Synapse SQL havuzu (veri ambarı) oluşturun ve sorgulayın.

## <a name="prerequisites"></a>Ön koşullar

1. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

   > [!NOTE]
   > Azure Synapse'de SQL havuzu oluşturmak, faturalandırılabilir yeni bir hizmete neden olabilir. Daha fazla bilgi için Azure [Synapse Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/synapse-analytics/)bölümüne bakın.

2. [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)’nun (SSMS) en yeni sürümünü indirin ve yükleyin.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-sql-pool"></a>SQL havuzu oluşturma

Veri ambarları, Azure Synapse Analytics'teki SQL havuzu kullanılarak oluşturulur. Tanımlı bir [işlem kaynakları](memory-concurrency-limits.md)kümesiyle bir SQL havuzu oluşturulur. Veritabanı bir [Azure kaynak grubu](../../azure-resource-manager/management/overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) ve bir [Azure SQL mantıksal sunucusu](../../sql-database/sql-database-servers.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) içinde oluşturulur.

**AdventureWorksDW** örnek verilerini içeren bir SQL havuzu oluşturmak için aşağıdaki adımları izleyin.

1. Azure portalının sol üst köşesinde **kaynak oluştur'u** seçin.

   ![Azure portalında kaynak oluşturma](./media/create-data-warehouse-portal/create-a-resource.png)

2. **Yeni** sayfada Veritabanları'nı seçin ve **Öne Çıkanlar** listesinde **Azure Synapse Analytics'i (eski adıyla SQL DW)** seçin. **Databases**

   ![boş veri ambarı oluşturma](./media/create-data-warehouse-portal/create-a-data-warehouse.png)

3. **Temel Bilgiler'de**aboneliğinizi, kaynak grubunuzu, SQL havuz adınızı ve sunucu adınızı sağlayın:

   | Ayar | Önerilen değer | Açıklama |
   | :------ | :-------------- | :---------- |
   | **Abonelik** | Aboneliğiniz | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
   | **Kaynak grubu** | myResourceGroup | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). |
   | **SQL havuz adı** | Herhangi bir küresel benzersiz adı (Bir örnek *mySampleDataWarehouse*olduğunu) | Geçerli veritabanı adları için bkz. [Veritabanı Tanımlayıcıları](/sql/relational-databases/databases/database-identifiers?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest). Not, SQL havuzu veritabanı bir türüdür. |
   | **Sunucu** | Genel olarak benzersiz bir ad | Varolan sunucuseçin veya yeni bir sunucu adı oluşturun, **yeni oluştur'u**seçin. Geçerli sunucu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). |

   ![veri ambarı temel ayrıntıları oluşturma](./media/create-data-warehouse-portal/create-sql-pool-basics.png)

4. **Performans düzeyi**altında, yapılandırmanızı kaydırıcıyla isteğe bağlı olarak değiştirmek için performans **düzeyini seçin'i** seçin.

   ![veri ambarı performans düzeyini değiştirme](./media/create-data-warehouse-portal/create-sql-pool-performance-level.png)  

   Performans düzeyleri hakkında daha fazla bilgi için Azure [SQL Veri Ambarı'nda bilgi işlem denetle'ye](sql-data-warehouse-manage-compute-overview.md)bakın.

5. Azure Synapse Analytics formunun Temeller sekmesini tamamladığınızda, SQL havuzunu oluşturmak için **Gözden Geçir + Oluştur'u** seçin ve ardından **Oluştur'u** seçin. Sağlama birkaç dakika sürer.

   ![Gözden Geçir + Oluştur'u seçin](./media/create-data-warehouse-portal/create-sql-pool-review-create.png)

   ![oluştur’u seçin](./media/create-data-warehouse-portal/create-sql-pool-create.png)

6. Araç çubuğunda, dağıtım işlemini izlemek için **Bildirimler'i** seçin.

   ![bildirim](./media/create-data-warehouse-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

Azure Synapse hizmeti sunucu düzeyinde bir güvenlik duvarı oluşturur. Bu güvenlik duvarı, harici uygulamaların ve araçların sunucuya veya sunucudaki veritabanlarına bağlanmasını engeller. Bağlantıyı etkinleştirmek için, belirli IP adresleri için bağlantıyı etkinleştiren güvenlik duvarı kuralları ekleyebilirsiniz. İstemcinizin IP adresine yönelik bir [sunucu düzeyi güvenlik duvarı kuralı](../../sql-database/sql-database-firewall-configure.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) oluşturmak için bu adımları izleyin.

> [!NOTE]
> Azure Synapse bağlantı noktası 1433 üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda BT departmanınız 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL Veritabanı sunucunuza bağlanamazsınız.

1. Dağıtım tamamlandıktan sonra, sol menüden **Tüm hizmetleri** seçin. **Veritabanları'nı**seçin, sık kullanılanlarınıza **Azure Synapse Analytics eklemek için Azure Synapse Analytics'in** yanındaki yıldızı seçin.

2. Sol menüden **Azure Synapse Analytics'i** seçin ve ardından **Azure Synapse Analytics** sayfasında **mySampleDataWarehouse'u** seçin. Veritabanınız için genel bakış sayfası açılır ve size tam nitelikli sunucu adını **(sqlpoolservername.database.windows.net**gibi) gösterir ve daha fazla yapılandırma için seçenekler sunar.

3. Bu ve diğer hızlı başlar sunucu ve veritabanlarına bağlanmak için kullanmak için bu tam nitelikli sunucu adını kopyalayın. Sunucu ayarlarını açmak için sunucu adını seçin.

   ![sunucu adını bulma](./media/create-data-warehouse-portal/find-server-name.png)

4. **Güvenlik duvarı ayarlarını göster'i**seçin.

   ![sunucu ayarları](./media/create-data-warehouse-portal/server-settings.png)

5. SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır.

   ![sunucu güvenlik duvarı kuralı](./media/create-data-warehouse-portal/server-firewall-rule.png)

6. Geçerli IP adresinizi yeni bir güvenlik duvarı kuralına eklemek için araç çubuğuna **istemci IP ekle'yi** seçin. Güvenlik duvarı kuralı, 1433 numaralı bağlantı noktasını tek bir IP adresi veya bir IP adresi aralığı için açabilir.

7. **Kaydet'i**seçin. Geçerli IP adresiniz için mantıksal sunucuda 1433 numaralı bağlantı noktası açılarak sunucu düzeyinde güvenlik duvarı kuralı oluşturulur.

8. **Tamam'ı** seçin ve ardından **Güvenlik Duvarı ayarları** sayfasını kapatın.

Artık bu IP adresini kullanarak SQL sunucusuna ve SQL havuzlarına bağlanabilirsiniz. Bağlantı SQL Server Management Studio’dan veya seçtiğiniz diğer bir araçtan çalışır. Bağlandığınızda, daha önce oluşturduğunuz ServerAdmin hesabını kullanın.

> [!IMPORTANT]
> Varsayılan olarak, SQL Veritabanı güvenlik duvarı üzerinden erişim tüm Azure hizmetleri için etkindir. bu sayfada **KAPALI'yi** seçin ve ardından tüm Azure hizmetleri için güvenlik duvarını devre dışı bırakabilmek için **Kaydet'i** seçin.

## <a name="get-the-fully-qualified-server-name"></a>Tam sunucu adını alma

SQL sunucunuzun tam sunucu adını Azure portalından alabilirsiniz. Daha sonra sunucuya bağlanırken tam adı kullanırsınız.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Sol menüden **Azure Synapse Analytics'i** seçin ve **Azure Synapse Analytics** sayfasında niçin istediğinizi seçin.

3. Veritabanınızın Azure portal sayfasındaki **Temel Bilgiler** bölmesinde, **Sunucu adını** bulup kopyalayın. Bu örnekte, tam nitelikli ad sqlpoolservername.database.windows.net.

    ![bağlantı bilgileri](./media/create-data-warehouse-portal/find-server-name-copy.png)

## <a name="connect-to-the-server-as-server-admin"></a>Sunucu yöneticisi olarak sunucuya bağlanma

Bu bölümde Azure SQL sunucunuzla bağlantı kurmak için [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) (SSMS) kullanılmaktadır.

1. SQL Server Management Studio’yu açın.

2. **Sunucuya Bağlan** iletişim kutusuna şu bilgileri girin:

   | Ayar | Önerilen değer | Açıklama |
   | :------ | :-------------- | :---------- |
   | Sunucu türü | Veritabanı altyapısı | Bu değer gereklidir |
   | Sunucu adı | Tam sunucu adı | İşte bir örnek: **sqlpoolservername.database.windows.net**. |
   | Kimlik Doğrulaması | SQL Server Kimlik Doğrulaması | Bu öğreticide yapılandırılan tek kimlik doğrulaması türü SQL Kimlik Doğrulamasıdır. |
   | Oturum Aç | Sunucu yöneticisi hesabı | Sunucuyu oluşturduğunuzda belirttiğiniz hesap. |
   | Parola | Sunucu yöneticisi hesabınızın parolası | Sunucuyu oluşturduğunuzda belirttiğiniz parola. |
   ||||

   ![sunucuya bağlan](./media/create-data-warehouse-portal/connect-to-server-ssms.png)

3. **Connect'i**seçin. SSMS’te Nesne Gezgini penceresi açılır.

4. Nesne Gezgini’nde, **Veritabanları**’nı genişletin. Daha sonra yeni veritabanınızdaki nesneleri görüntülemek için **mySampleDatabase**’i genişletin.

   ![veritabanı nesneleri](./media/create-data-warehouse-portal/connected-ssms.png)

## <a name="run-some-queries"></a>Sorgular çalıştırma

SQL Veri Ambarı sorgu dili olarak T-SQL kullanır. Bir sorgu penceresi açıp T-SQL sorguları çalıştırmak için, aşağıdaki adımları kullanın:

1. **mySampleDataWarehouse'u** sağ seçin ve **Yeni Sorgu'yu**seçin. Yeni bir sorgu penceresi açılır.

2. Sorgu penceresinde, veritabanlarının listesini görmek için aşağıdaki komutu girin.

    ```sql
    SELECT * FROM sys.databases
    ```

3. Yürüt'ün'u seçin. **Execute** Sorgu sonuçları iki veritabanı gösterir: **master** ve **mySampleDataWarehouse**.

   ![Sorgu veritabanları](./media/create-data-warehouse-portal/query-databases.png)

4. Verilere bakmak için, Adams soyadına sahip ve üç çocuğu olan müşteri sayısını görmek için aşağıdaki komutu kullanın. Sonuçlarda altı müşteri listelenir.

    ```sql
    SELECT LastName, FirstName FROM dbo.dimCustomer
    WHERE LastName = 'Adams' AND NumberChildrenAtHome = 3;
    ```

   ![dbo.dimCustomer’ı sorgulama](./media/create-data-warehouse-portal/query-customer.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Veri ambarı birimleri için ücretlendirilirsiniz ve SQL havuzunuzda depolanan veriler. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır.

- Verileri depolamada tutmak istiyorsanız, SQL havuzunu kullanmadığınızda bilgi işlem'i duraklatabilirsiniz. İşlemi duraklatarak, yalnızca veri depolama için ücretlendirilirsiniz. Verilerle çalışmaya hazır olduğunuzda bilgi işlem yapmaya devam edebilirsiniz.

- Gelecekteki ücretleri kaldırmak istiyorsanız, SQL havuzunu silebilirsiniz.

Artık ihtiyacınız olmayan kaynakları temizlemek için aşağıdaki adımları izleyin.

1. [Azure portalında](https://portal.azure.com)oturum açın, SQL havuzunuzu seçin.

   ![Kaynakları temizleme](./media/create-data-warehouse-portal/clean-up-resources.png)

2. İşlemi duraklatmak için **Duraklat** düğmesini seçin. SQL havuzu duraklatıldığunda, **Bir Özgeçmiş** düğmesi görürsünüz. İşleme devam etmek için **Devam edin'i**seçin.

3. İşlem veya depolama için ücretlendirilmemek için SQL havuzunu kaldırmak için **Sil'i**seçin.

4. Oluşturduğunuz SQL sunucusunu kaldırmak için önceki resimdeki **sqlpoolservername.database.windows.net'yi** seçin ve sonra **Sil'i**seçin. Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

5. Kaynak grubunu kaldırmak için **myResourceGroup'u**seçin ve ardından **kaynak grubunu sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

SQL havuzunuza veri yükleme hakkında daha fazla bilgi edinmek [için, verileri SQL havuzu makalesine yüklemeye](load-data-from-azure-blob-storage-using-polybase.md) devam edin.
