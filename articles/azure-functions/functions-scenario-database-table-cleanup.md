---
title: Veritabanı temizleme görevini gerçekleştirmek için Azure Işlevlerini kullanma
description: Satırları düzenli aralıklarla temizlemek için Azure SQL Veritabanı'na bağlanan bir görev zamanlamak için Azure İşlevlerini kullanın.
ms.assetid: 076f5f95-f8d2-42c7-b7fd-6798856ba0bb
ms.topic: conceptual
ms.date: 10/02/2019
ms.openlocfilehash: 2e3f53943d45e90b8aff8e386ce8d0e28670673f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79366827"
---
# <a name="use-azure-functions-to-connect-to-an-azure-sql-database"></a>Azure SQL Veritabanına bağlanmak için Azure Işlevlerini kullanma

Bu makalede, bir Azure SQL Veritabanına veya Azure SQL Yönetilen Örneği'ne bağlanan zamanlanmış bir iş oluşturmak için Azure İşlevlerini nasıl kullanacağınızı gösterilmektedir. İşlev kodu veritabanındaki bir tablodaki satırları temizler. Yeni C# işlevi Visual Studio 2019'da önceden tanımlanmış zamanlayıcı tetikleyici şablonuna göre oluşturulur. Bu senaryoyu desteklemek için, işlev uygulamasında bir uygulama ayarı olarak bir veritabanı bağlantı dizesi ayarlamanız gerekir. Azure SQL Yönetilen Örnek için Azure İşlerinden bağlanabilmek için [ortak bitiş noktasını etkinleştirmeniz](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure) gerekir. Bu senaryo veritabanına karşı toplu bir işlem kullanır. 

C# İşlevleri ile çalışma deneyiminiz buysa, [Azure İşlevleri C# geliştirici referansını](functions-dotnet-class-library.md)okumalısınız.

## <a name="prerequisites"></a>Ön koşullar

+ Sürüm 2.x'i veya çalışma zamanının daha sonraki bir sürümünü hedefleyen yerel bir işlev uygulaması oluşturmak için [Visual Studio'yu kullanarak](functions-create-your-first-function-visual-studio.md) makaledeki adımları tamamlayın. Projenizi Azure'daki bir işlev uygulamasına da yayınlamış olmalısınız.

+ Bu makalede, AdventureWorksLT örnek veritabanındaki **SalesOrderHeader** tablosunda toplu temizleme işlemi yürüten bir Transact-SQL komutu gösterilmiştir. AdventureWorksLT örnek veritabanını oluşturmak için, Azure [portalında bir Azure SQL veritabanı oluşturun](../sql-database/sql-database-get-started-portal.md)makaledeki adımları tamamlayın.

+ Bu hızlı başlangıç için kullandığınız bilgisayarın genel IP adresi için sunucu düzeyinde bir [güvenlik duvarı kuralı](../sql-database/sql-database-get-started-portal-firewall.md) eklemeniz gerekir. Bu kural, yerel bilgisayarınızdan SQL veritabanı örneğine erişebilmek için gereklidir.  

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

[Azure portalında Bir Azure SQL veritabanı oluşturma'yı](../sql-database/sql-database-get-started-portal.md)tamamladığınızda oluşturduğunuz veritabanının bağlantı dizesini almanız gerekir.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Sol menüden **SQL Veritabanları'nı** seçin ve SQL **veritabanları** sayfasında veritabanınızı seçin.

1. **Ayarlar** altında **Bağlantı dizelerini** seçin ve **ADO.NET** bağlantı dizesinin tamamını kopyalayın. Azure SQL Yönetilen Örnek için ortak bitiş noktası için bağlantı dizesi.

    ![ADO.NET bağlantı dizesini kopyalayın.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-the-connection-string"></a>Bağlantı dizesini ayarlama

Azure'da işlevlerinizin yürütülmesini bir işlev uygulaması barındırır. En iyi güvenlik uygulaması olarak, bağlantı dizelerini ve diğer sırları işlev uygulaması ayarlarınızda saklayın. Uygulama ayarlarını kullanmak, bağlantı dizesinin kodunla yanlışlıkla açıklanmasını engeller. İşlev uygulamanızın uygulama ayarlarına hemen Visual Studio'dan erişebilirsiniz.

Uygulamanızı daha önce Azure'da yayımlamış olmalısınız. Henüz yapmadıysanız, [işlev uygulamanızı Azure'da yayımlayın.](functions-develop-vs.md#publish-to-azure)

1. Solution Explorer'da, işlev uygulaması projesine sağ tıklayın ve**Azure Uygulama Hizmetini Edit'i** **Yayımla'yı** > seçin. **Yeni uygulama ayarı adı**, `sqldb_connection`yazın ve **Tamam'ı**seçin **ayar ekle'yi**seçin.

    ![Fonksiyon uygulaması için uygulama ayarları.](./media/functions-scenario-database-table-cleanup/functions-app-service-add-setting.png)

1. Yeni **sqldb_connection** ayarında, önceki bölümde kopyaladığınız bağlantı dizesini **Yerel** alana yapıştırın ve gerçek değerlerle değiştirin ve `{your_username}` `{your_password}` yer tutucuları değiştirin. Güncelleştirilmiş değeri **Uzak** alana kopyalamak için **yerelden Ekle değerini** seçin ve ardından **Tamam'ı**seçin.

    ![SQL bağlantı dizesi ayarı ekleyin.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-string.png)

    Bağlantı dizeleri Azure 'da **(Remote)** şifreli olarak depolanır. Sırların sızdırılmasını önlemek için local.settings.json project file (**Local**) bir .gitignore dosyası kullanmak gibi kaynak denetiminin dışında tutulmalıdır.

## <a name="add-the-sqlclient-package-to-the-project"></a>Projeye SqlClient paketini ekleme

SqlClient kitaplığını içeren NuGet paketini eklemeniz gerekir. Bu veri erişim kitaplığı bir SQL veritabanına bağlanmak için gereklidir.

1. Visual Studio 2019'da yerel fonksiyon uygulama projenizi açın.

1. Solution Explorer'da, işlev uygulaması projesine sağ tıklayın ve **NuGet Paketlerini Yönet'i**seçin.

1. **Gözat** sekmesinde ```System.Data.SqlClient``` öğesini arayın ve bulduğunuzda seçin.

1. **System.Data.SqlClient** sayfasında sürümü `4.5.1` seçin ve sonra **Yükle'yi**tıklatın.

1. Yükleme tamamlandığında değişiklikleri gözden geçirin ve **Önizleme** penceresini kapamak için **Tamam**’a tıklayın.

1. **Lisans Kabulü** penceresi gösterilirse **Kabul Ediyorum**’a tıklayın.

Şimdi, SQL Veritabanınıza bağlanan C# işlev kodunu ekleyebilirsiniz.

## <a name="add-a-timer-triggered-function"></a>Zamanlayıcı ile tetiklenen işlev ekleme

1. Solution Explorer'da, işlev uygulaması projesine sağ tıklayın ve**Yeni Azure işlevi ekle'yi** **Add** > seçin.

1. Azure **İşlevler** şablonu seçili seve, yeni öğeye benzer `DatabaseCleanup.cs` bir şey adlandırın ve **Ekle'yi**seçin.

1. Yeni **Azure işlev** iletişim kutusunda **Timer tetikleyicisini** ve ardından **Tamam'ı**seçin. Bu iletişim kutusu, zamanlayıcı tetiklenen işlev için bir kod dosyası oluşturur.

1. Yeni kod dosyasını açın ve dosyanın üst kısmında aşağıdaki ifadeleri kullanarak ekleyin:

    ```cs
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

1. Varolan `Run` işlevi aşağıdaki kodla değiştirin:

    ```cs
    [FunctionName("DatabaseCleanup")]
    public static async Task Run([TimerTrigger("*/15 * * * * *")]TimerInfo myTimer, ILogger log)
    {
        // Get the connection string from app settings and use it to create a connection.
        var str = Environment.GetEnvironmentVariable("sqldb_connection");
        using (SqlConnection conn = new SqlConnection(str))
        {
            conn.Open();
            var text = "UPDATE SalesLT.SalesOrderHeader " +
                    "SET [Status] = 5  WHERE ShipDate < GetDate();";

            using (SqlCommand cmd = new SqlCommand(text, conn))
            {
                // Execute the command and log the # rows affected.
                var rows = await cmd.ExecuteNonQueryAsync();
                log.LogInformation($"{rows} rows were updated");
            }
        }
    }
    ```

    Bu işlev, sütunu `Status` sevk tarihine göre güncelleştirmek için her 15 saniyede bir çalışır. Zamanlayıcı tetikleyicisi hakkında daha fazla bilgi edinmek [için Azure İşlevler için Zamanlayıcı tetikleyicisine](functions-bindings-timer.md)bakın.

1. Fonksiyon uygulamasını başlatmak için **F5** tuşuna basın. Visual Studio'nun arkasındaki [Azure İşleçleri Temel Araçları](functions-develop-local.md) yürütme penceresi açılır.

1. Başlatmadan 15 saniye sonra işlev çalışır. Çıktıyı izleyin ve **SalesOrderHeader** tablosunda güncelleştirilen satır sayısını not edin.

    ![İşlev günlüklerini görüntüleyin.](./media/functions-scenario-database-table-cleanup/function-execution-results-log.png)

    İlk yürütmede, 32 veri satırını güncelleştirmeniz gerekir. SalesOrderHeader tablo verilerinde değişiklik yapmazsanız, `UPDATE` ekstre tarafından daha fazla satır seçilsin.

Bu işlevi [yayımlamayı](functions-develop-vs.md#publish-to-azure)planlıyorsanız, `TimerTrigger` özniteliği her 15 saniyede daha makul bir [cron zamanlamasına](functions-bindings-timer.md#ncrontab-expressions) değiştirmeyi unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

Sonra, nasıl kullanılacağını öğrenin. Diğer hizmetlerle tümleştirmek için Logic Apps ile işlev ler.

> [!div class="nextstepaction"]
> [Logic Apps ile tümleşen bir işlev oluşturma](functions-twitter-email.md)

İşlevler hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

+ [Azure İşlevleri geliştirici başvurusu](functions-reference.md)  
  İşlevleri kodlamak ve tetikleyicileri ve bağlamaları tanımlamak için programcı başvurusu
+ [Azure İşlevlerini test etme](functions-test-a-function.md)  
  İşlevlerinizi test etmek için çeşitli araçları ve teknikleri açıklar.  
