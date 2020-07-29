---
title: Excel ile bağlanma
description: Microsoft Excel 'i Azure SQL veritabanı veya Azure SQL yönetilen örneği 'nde bir veritabanına bağlamayı öğrenin. Raporlama ve veri araştırması için Excel'e veri aktarın.
titleSuffix: Azure SQL Database & SQL Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 05/29/2020
ms.openlocfilehash: e0a413847142ee516a06f924dd058578047fb3a8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84267520"
---
# <a name="connect-excel-to-a-database-in-azure-sql-database-or-azure-sql-managed-instance-and-create-a-report"></a>Excel 'i Azure SQL veritabanı veya Azure SQL yönetilen örneği 'nde bir veritabanına bağlama ve rapor oluşturma
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Excel 'i bir veritabanına bağlayıp sonra verileri içeri aktarabilir ve veritabanındaki değerlere göre tablolar ve grafikler oluşturabilirsiniz. Bu öğreticide, Excel ile bir veritabanı tablosu arasındaki bağlantıyı ayarlayacak, Excel'e ilişkin verilerin ve bağlantı bilgilerinin depolandığı dosyayı kaydedecek ve ardından veritabanı değerlerini kullanarak bir özet grafik oluşturacaksınız.

Başlamadan önce bir veritabanı oluşturmanız gerekir. Bir tane yoksa, örnek veri içeren bir veritabanını birkaç dakikada çalışır duruma getirmek için bkz. [Azure SQL veritabanı 'nda veritabanı oluşturma](single-database-create-quickstart.md) ve [sunucu düzeyinde IP güvenlik duvarı oluşturma](firewall-create-server-level-portal-quickstart.md) .

Bu makalede, bu makaleden Excel 'e örnek veri aktarırsınız, ancak kendi verilerinize benzer adımları izleyebilirsiniz.

Ayrıca, bir Excel kopyanızın olması gerekir. Bu makalede [Microsoft Excel 2016](https://products.office.com/) kullanılmıştır.

## <a name="connect-excel-and-load-data"></a>Excel 'i bağlama ve verileri yükleme

1. Excel 'i SQL veritabanı 'ndaki bir veritabanına bağlamak için Excel 'i açın ve yeni bir çalışma kitabı oluşturun veya var olan bir Excel çalışma kitabını açın.
2. Sayfanın üstündeki menü çubuğunda, **veri** sekmesini seçin, **veri al**' ı seçin, Azure 'dan seçim YAPıN ve ardından **Azure SQL veritabanı**' nı seçin.

   ![Veri kaynağı seçin: Excel 'i SQL veritabanı 'na bağlama.](./media/connect-excel/excel_data_source.png)

3. **SQL Server veritabanı** iletişim kutusunda, <*ServerName*. Database.Windows.net biçiminde bağlanmak istediğiniz **sunucu adını** yazın > **.database.windows.net**. Örneğin, **msftestserver.Database.Windows.net**. İsteğe bağlı olarak, veritabanınızın adını girin. **Tamam ' ı** seçerek kimlik bilgileri penceresini açın.

   ![Veritabanı sunucusuna bağlan Iletişim kutusu](./media/connect-excel/server-name.png)

4. **SQL Server veritabanı** iletişim kutusunda, sol taraftaki **veritabanı** ' nı seçin ve ardından bağlanmak Istediğiniz sunucu Için **Kullanıcı adınızı** ve **parolanızı** girin. **Gezginini**açmak için **Bağlan** ' ı seçin.

   ![Sunucu adını ve oturum açma kimlik bilgilerini girme](./media/connect-excel/connect-to-server.png)

   > [!TIP]
   > Ağ ortamınıza bağlı olarak, sunucu istemci IP adresinizden gelen trafiğe izin vermezse bağlantıyı kaybedebilirsiniz. [Azure portalına](https://portal.azure.com/) gidip SQL sunucuları seçeneğine tıklayın, sunucunuza tıklayın ve ardından ayarlar altında bulunan güvenlik duvarı seçeneğine tıklayıp istemci IP adresinizi ekleyin. Ayrıntılı bilgi için bkz. [Güvenlik duvarı ayarlarını yapılandırma](firewall-configure.md).

5. **Gezgin**'de, çalışmak istediğiniz veritabanını listeden seçin, birlikte çalışmak istediğiniz tabloları veya görünümleri seçin ( **Vgetallcategories**' i seçtik) ve sonra verileri veritabanından Excel elektronik Tablonuzya taşımak için **Yükle** ' yi seçin.

    ![Bir veritabanı ve tablo seçin.](./media/connect-excel/select-database-and-table.png)

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Excel'e veri aktarma ve özet grafik oluşturma

Artık bağlantıyı oluşturduğunuza göre, verilerin nasıl yükleneceği ile ilgili birkaç farklı seçeneğiniz vardır. Örneğin, aşağıdaki adımlar SQL veritabanı 'nda veritabanınızda bulunan verileri temel alan bir Özet Grafik oluşturur.

1. Önceki bölümdeki adımları izleyin, ancak bu kez Yükle ' **yi seçmek yerine** **yük açılır listesinden** **Yükle ' yi**seçin.
2. Sonra, bu verileri çalışma kitabınızda nasıl görüntülemek istediğinizi seçin. Biz **PivotChart** seçeneğini belirledik. Ayrıca, **Yeni çalışma sayfası** oluşturmayı veya **Bu verileri Veri Modeline ekle** seçeneğini belirlemeyi de tercih edebilirsiniz. Veri Modelleri hakkında daha fazla bilgi için bkz. [Excel'de veri modeli oluşturma](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B).

    ![Excel'de veri biçimini belirleme](./media/connect-excel/import-data.png)

    Artık çalışma sayfasında boş bir özet grafiği ve grafik var.
3. **PivotTable Alanları** altında, görüntülemek istediğiniz alanların onay kutularını işaretleyin.

    ![Veritabanı raporunu yapılandırın.](./media/connect-excel/power-pivot-results.png)

> [!TIP]
> Diğer Excel çalışma kitaplarını ve çalışma sayfalarını veritabanına bağlamak istiyorsanız, **veri** sekmesini seçin **ve son kaynaklar iletişim kutusunu** başlatmak için **son kaynaklar** ' ı seçin. Buradan, listeden oluşturduğunuz bağlantıyı seçin ve ardından **Aç**' a tıklayın.
> ![Son kaynaklar iletişim kutusu](./media/connect-excel/recent-connections.png)

## <a name="create-a-permanent-connection-using-odc-file"></a>. Odc dosyası kullanarak kalıcı bağlantı oluşturma

Bağlantı ayrıntılarını kalıcı olarak kaydetmek için bir. odc dosyası oluşturabilir ve **var olan bağlantılar** iletişim kutusunda bu bağlantıyı seçilebilir bir seçenek haline getirebilirsiniz.

1. Sayfanın üst kısmındaki menü çubuğunda, **veri** sekmesini seçin ve **var olan bağlantılar iletişim kutusunu** başlatmak için **mevcut bağlantılar** ' ı seçin.
   1. **Daha fazla araştır** ' ı seçerek **veri kaynağı seç** iletişim kutusunu açın.
   2. **+ Newsqlserverconnection. odc** dosyasını seçin ve ardından **Aç** ' ı seçerek **veri bağlantısı sihirbazını**açın.

      ![Yeni bağlantı iletişim kutusu](./media/connect-excel/new-connection.png)

2. **Veri bağlantısı sihirbazında**sunucu ADıNıZı ve SQL veritabanınızın kimlik bilgilerini yazın. **İleri**’yi seçin.
   1. Açılan listeden verilerinizi içeren veritabanını seçin.
   2. İlgilendiğiniz tabloyu veya görünümü seçin. VGetAllCategories seçtik.
   3. **İleri**’yi seçin.

      ![Veri Bağlantısı Sihirbazı](./media/connect-excel/data-connection-wizard.png)

3. Veri bağlantısı sihirbazının sonraki ekranında dosyanızın konumunu, **dosya adını**ve **kolay adını** seçin. Ayrıca, verilerinizi dosyaya kaydetmeyi de tercih edebilirsiniz; ancak bu, verilerinizi istenmeyen erişime açabilir. Hazırsanız **son** ' u seçin.

    ![Veri bağlantısını Kaydet](./media/connect-excel/save-data-connection.png)

4. Verilerinizi nasıl içeri aktarmak istediğinizi seçin. Bir PivotTable yapmayı seçtik. Ayrıca, **Özellikler**' i seçerek bağlantının özelliklerini de değiştirebilirsiniz. Hazırlanıyor **Tamam ' ı** seçin. Parolayı dosya ile kaydetmeyi seçmediyseniz, kimlik bilgilerinizi girmeniz istenir.

    ![Verileri İçeri Aktarma](./media/connect-excel/import-data2.png)

5. **Veri** sekmesini genişleterek ve **var olan bağlantılar**' ı seçerek yeni bağlantınızın kaydedildiğini doğrulayın.

    ![Mevcut bağlantı](./media/connect-excel/existing-connection.png)

## <a name="next-steps"></a>Sonraki adımlar

* Gelişmiş sorgulama ve analizler için [SQL Server Management Studio bağlama ve sorgulama](connect-query-ssms.md) hakkında bilgi edinin.
* [Esnek havuzların](elastic-pool-overview.md) avantajları hakkında bilgi edinin.
* [Arka uçta Azure SQL veritabanı 'na bağlanan bir Web uygulaması oluşturmayı](../../app-service/app-service-web-tutorial-dotnet-sqldatabase.md)öğrenin.
