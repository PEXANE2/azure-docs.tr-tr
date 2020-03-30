---
title: Excel'i tek bir veritabanına bağlama
description: Microsoft Excel'i Azure SQL veritabanında tek bir veritabanına nasıl bağlayabilirsiniz öğrenin. Raporlama ve veri araştırması için Excel'e veri aktarın.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: joseidz
ms.author: craigg
ms.reviewer: ''
ms.date: 02/12/2019
ms.openlocfilehash: de5f23bf1e8acd8a5fcd0cf8e1526f88667800c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73827125"
---
# <a name="connect-excel-to-a-single-database-in-azure-sql-database-and-create-a-report"></a>Excel'i Azure SQL veritabanında tek bir veritabanına bağlayın ve bir rapor oluşturun

Excel'i Azure SQL Veritabanı'nda tek bir veritabanına bağlayın ve verileri aktarın ve veritabanındaki değerlere dayalı tablolar ve grafikler oluşturun. Bu öğreticide, Excel ile bir veritabanı tablosu arasındaki bağlantıyı ayarlayacak, Excel'e ilişkin verilerin ve bağlantı bilgilerinin depolandığı dosyayı kaydedecek ve ardından veritabanı değerlerini kullanarak bir özet grafik oluşturacaksınız.

Başlamadan önce tek bir veritabanına ihtiyacınız olacak. Yoksa, örnek verileri birkaç dakika içinde çalışır duruma getirmek için [tek bir veritabanı oluştur](sql-database-single-database-get-started.md) ve sunucu düzeyinde IP güvenlik duvarı [oluşturun'a](sql-database-server-level-firewall-rule.md) bakın.

Bu makalede, bu makaleden Excel'e örnek verileri aktaracaksınız, ancak kendi verilerinizle benzer adımları izleyebilirsiniz.

Ayrıca, bir Excel kopyanızın olması gerekir. Bu makalede [Microsoft Excel 2016](https://products.office.com/) kullanılmıştır.

## <a name="connect-excel-to-a-sql-database-and-load-data"></a>Excel'i SQL veritabanına bağlayın ve verileri yükleyin

1. Excel'i SQL veritabanına bağlamak için Excel'i açarak yeni bir çalışma kitabı oluşturun veya var olan bir Excel çalışma kitabını açın.
2. Sayfanın üst kısmındaki menü çubuğunda **Veri** sekmesini seçin, **Verileri Al'ı**, Azure'dan seçin ve ardından **Azure SQL Veritabanı'ndan'ı**seçin. 

   ![Veri kaynağı seçin: Excel'i SQL veritabanına bağlayın.](./media/sql-database-connect-excel/excel_data_source.png)

   Veri Bağlantı Sihirbazı açılır.
3. **Veritabanı Sunucusuna Bağlan** iletişim kutusunda, bağlanmak istediğiniz SQL Database **Sunucu adını** şu biçimde girin: <*sunucuadı*>**.database.windows.net**. Örneğin, **msftestserver.database.windows.net.** İsteğe bağlı olarak, veritabanınızın adına girin. Kimlik bilgileri penceresini açmak için **Tamam'ı** seçin. 

   ![Veritabanı Sunucusu İletişim kutusuna bağlanın](media/sql-database-connect-excel/server-name.png)

4. SQL **Server Database** iletişim kutusunda, sol tarafta **veritabanını** seçin ve ardından bağlanmak istediğiniz SQL Veritabanı sunucusu için **Kullanıcı Adınızı** ve **Parolanızı** girin. **Navigator'ı**açmak için **Bağlan'ı** seçin. 

   ![Sunucu adını ve oturum açma kimlik bilgilerini girme](./media/sql-database-connect-excel/connect-to-server.png)

   > [!TIP]
   > Ağ ortamınıza bağlı olarak, SQL Veritabanı sunucusunun istemci IP adresinizden gelen trafiğe izin vermemesi halinde bağlanamayabilirsiniz veya mevcut bağlantınız kesilebilir. [Azure portalına](https://portal.azure.com/) gidip SQL sunucuları seçeneğine tıklayın, sunucunuza tıklayın ve ardından ayarlar altında bulunan güvenlik duvarı seçeneğine tıklayıp istemci IP adresinizi ekleyin. Ayrıntılı bilgi için bkz. [Güvenlik duvarı ayarlarını yapılandırma](sql-database-configure-firewall-settings.md).

5. **Navigator'da,** listeden çalışmak istediğiniz veritabanını seçin, çalışmak istediğiniz tabloları veya görünümleri seçin **(vGetAllCategories'i**seçtik) ve ardından verileri veritabanınızdan Excel elektronik tablonuza taşımak için **Yükle'yi** seçin.

    ![Bir veritabanı ve tablo seçin.](./media/sql-database-connect-excel/select-database-and-table.png)

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Excel'e veri aktarma ve özet grafik oluşturma

Artık bağlantıyı kurduğunuza göre, verileri nasıl yükleyinizle ilgili birkaç farklı seçeneğiniz var. Örneğin, aşağıdaki adımlar, SQL Veritabanınızda bulunan verileri temel alan bir özet grafiği oluşturur. 

1. Önceki bölümdeki adımları izleyin, ancak bu sefer **Yük'ü**seçmek yerine **Yük** açılır kısmından **Yükle'yi** seçin.
2. Ardından, bu verileri çalışma kitabınızda nasıl görüntülemek istediğinizi seçin. Biz **PivotChart** seçeneğini belirledik. Ayrıca, **Yeni çalışma sayfası** oluşturmayı veya **Bu verileri Veri Modeline ekle** seçeneğini belirlemeyi de tercih edebilirsiniz. Veri Modelleri hakkında daha fazla bilgi için bkz. [Excel'de veri modeli oluşturma](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). 

    ![Excel'de veri biçimini belirleme](./media/sql-database-connect-excel/import-data.png)

    Artık çalışma sayfasında boş bir özet grafiği ve grafik var.
3. **PivotTable Alanları** altında, görüntülemek istediğiniz alanların onay kutularını işaretleyin.

    ![Veritabanı raporunu yapılandırın.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> Diğer Excel çalışma kitaplarını ve çalışma sayfalarını veritabanına bağlamak istiyorsanız, **Veri** sekmesini seçin ve **Son Kaynaklar** iletişim kutusunu başlatmak için **Son Kaynaklar'ı** seçin. Buradan, listeden oluşturduğunuz bağlantıyı seçin ve sonra **Aç'ı**tıklatın.
> ![Son Kaynaklar iletişim kutusu](media/sql-database-connect-excel/recent-connections.png)

## <a name="create-a-permanent-connection-using-odc-file"></a>.odc dosyalarını kullanarak kalıcı bağlantı oluşturma

Bağlantı ayrıntılarını kalıcı olarak kaydetmek için bir .odc dosyası oluşturabilir ve bu bağlantıyı **Varolan Bağlantılar** iletişim kutusunda seçilebilir bir seçenek haline getirebilirsiniz. 

1. Sayfanın üst kısmındaki menü çubuğunda **Veri** sekmesini seçin ve **varolan bağlantılar** iletişim kutusunu başlatmak için **Varolan Bağlantılar'ı** seçin. 
   1. **Veri Kaynağı Seç** iletişim kutusunu açmak için daha fazlası için **Gözat'ı** seçin.   
   2. **+NewSqlServerConnection.odc** dosyasını seçin ve ardından **Veri Bağlantısı Sihirbazı'nı**açmak için **Aç'ı** seçin.

      ![Yeni Bağlantı iletişim kutusu](media/sql-database-connect-excel/new-connection.png)

2. Veri **Bağlantısı Sihirbazı'nda**sunucu adınızı ve SQL Veritabanı kimlik bilgilerinizi yazın. **Sonraki'ni**seçin. 
   1. Açılan açılır yerden verilerinizi içeren veritabanını seçin. 
   2. İlgilendiğiniz tabloyu veya görünümü seçin. vGetAllCategories'i seçtik.
   3. **Sonraki'ni**seçin. 

      ![Veri Bağlantısı Sihirbazı](media/sql-database-connect-excel/data-connection-wizard.png) 

3. Veri Bağlantısı Sihirbazı'nın bir sonraki ekranında dosyanızın konumunu, **Dosya Adı'nı**ve **Arkadaş Çası Adını** seçin. Parolayı dosyaya kaydetmeyi de seçebilirsiniz, ancak bu durum verilerinizi istenmeyen erişime maruz bırakabilir. Hazır olduğunda **Bitir'i** seçin. 

    ![Veri Bağlantısını Kaydet](media/sql-database-connect-excel/save-data-connection.png)

4. Verilerinizi nasıl almak istediğinizi seçin. PivotTable yapmayı seçtik. Ayrıca, bağlantının özelliklerini belirli **Özellikler'e**göre de değiştirebilirsiniz. Hazır olduğunda **Tamam'ı** seçin. Parolayı dosyayla birlikte kaydetmeyi seçmediyseniz, kimlik bilgilerinizi girmeniz istenir. 

    ![Verileri İçeri Aktarma](media/sql-database-connect-excel/import-data2.png)

5. **Veri** sekmesini genişleterek ve **Varolan Bağlantıları**seçerek yeni bağlantınızın kaydedildiğini doğrulayın. 

    ![Varolan Bağlantı](media/sql-database-connect-excel/existing-connection.png)

## <a name="next-steps"></a>Sonraki adımlar

* Gelişmiş sorgulama ve analiz için [SQL Server Management Studio ile SQL Database'e bağlanma](sql-database-connect-query-ssms.md) işlemini nasıl gerçekleştireceğinizi öğrenin.
* [Esnek havuzların](sql-database-elastic-pool.md) avantajları hakkında bilgi edinin.
* [Arka uçta SQL Database'e bağlanan bir web uygulaması oluşturma](../app-service/app-service-web-tutorial-dotnet-sqldatabase.md) hakkında bilgi edinin.
