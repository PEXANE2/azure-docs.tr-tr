---
title: BI analiz araçlarını kullanarak Azure Cosmos DB'ye bağlanın
description: Normalleştirilmiş verilerin BI ve veri analizi yazılımında görüntülenebileceği tablolar ve görünümler oluşturmak için Azure Cosmos DB ODBC sürücüsünü nasıl kullanacağınızı öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: sngun
ms.openlocfilehash: 8be17f0b624c5c34709fb420adb434b77dbc0d91
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721090"
---
# <a name="connect-to-azure-cosmos-db-using-bi-analytics-tools-with-the-odbc-driver"></a>ODBC sürücüsüyle BI analiz araçlarını kullanarak Azure Cosmos DB'ye bağlanın

Azure Cosmos DB ODBC sürücüsü, bu çözümlerdeki Azure Cosmos DB verilerinizin görselleştirmelerini analiz edebilmeniz ve oluşturabilmeniz için SQL Server Tümleştirme Hizmetleri, Power BI Desktop ve Tableau gibi BI analiz araçlarını kullanarak Azure Cosmos DB'ye bağlanmanızı sağlar.

Azure Cosmos DB ODBC sürücüsü ODBC 3.8 ile uyumludur ve ANSI SQL-92 söz dizimini destekler. Sürücü, verileri Azure Cosmos DB'de yeniden normalleştirmenize yardımcı olan zengin özellikler sunar. Sürücüyü kullanarak verilerinizi Azure Cosmos DB'de tablo ve görünüm olarak gösterebilirsiniz. Sürücü sorgulara, eklemelere, güncelleştirmelere ve silmelere göre gruplama dahil olmak üzere tablo ve görünümlerde SQL işlemleri gerçekleştirmenizi sağlar.

> [!NOTE]
> Azure Cosmos DB'ye ODBC sürücüsüyle bağlanmak şu anda yalnızca Azure Cosmos DB SQL API hesapları için desteklenmektedir.

## <a name="why-do-i-need-to-normalize-my-data"></a>Verilerimi neden normalleştirmem gerekiyor?
Azure Cosmos DB, hızlı uygulama geliştirme ve sıkı bir şema ile sınırlandırılmadan veri modellerinde yeniden hesaplaşabilme olanağı sağlayan şemasız bir veritabanıdır. Tek bir Azure Cosmos veritabanı çeşitli yapılara ait JSON belgelerini içerebilir. Bu hızlı uygulama geliştirme için harikadır, ancak veri analitiği ve BI araçlarını kullanarak verilerinizin raporlarını analiz etmek ve oluşturmak istediğinizde, verilerin genellikle düzleştirilmiş olması ve belirli bir şemaya uyması gerekir.

Burada ODBC sürücüsü devreye giriyor. ODBC sürücüsünü kullanarak, azure cosmos DB'deki verileri veri analitiğinize ve raporlama gereksinimlerinize uygun tablolara ve görünümlere dönüştürebilirsiniz. Yeniden normalleştirilmiş şemaların temel veriler üzerinde hiçbir etkisi yoktur ve geliştiricileri bunlara uymak la sınırlandırmaz. Bunun yerine, verilere erişmek için ODBC uyumlu araçlardan yararlanmanızı sağlarlar. Bu nedenle, Azure Cosmos veritabanınız yalnızca geliştirme ekibiniz için favori olmakla kalmıyor, aynı zamanda veri analistleriniz de bundan bayılacak.

ODBC sürücüsüyle başlayalım.

## <a name="step-1-install-the-azure-cosmos-db-odbc-driver"></a><a id="install"></a>Adım 1: Azure Cosmos DB ODBC sürücüsünü yükleme

1. Ortamınız için sürücüleri indirin:

    | Installer | Desteklenen işletim sistemleri| 
    |---|---| 
    |64 bit Windows için [Microsoft Azure Cosmos DB ODBC 64-bit.msi](https://aka.ms/cosmos-odbc-64x64)| Windows 8.1 veya sonraki 64 bit sürümleri, Windows 8, Windows 7, Windows Server 2012 R2, Windows Server 2012 ve Windows Server 2008 R2.| 
    |[Microsoft Azure Cosmos DB ODBC 32x64-bit.msi](https://aka.ms/cosmos-odbc-32x64) için 32-bit 64 bit Windows| Windows 8.1 veya sonraki 64 bit sürümleri, Windows 8, Windows 7, Windows XP, Windows Vista, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ve Windows Server 2003.| 
    |32 bit Windows için [Microsoft Azure Cosmos DB ODBC 32-bit.msi](https://aka.ms/cosmos-odbc-32x32)|Windows 8.1 veya sonraki sürümler, Windows 8, Windows 7, Windows XP ve Windows Vista'nın 32 bit sürümleri.|

    **Microsoft Azure Cosmos DB ODBC Sürücü Yükleme Sihirbazı'nı**başlatan msi dosyasını yerel olarak çalıştırın. 

1. ODBC sürücüsünü yüklemek için varsayılan girişi kullanarak yükleme sihirbazını tamamlayın.

1. Bilgisayarınızdaki **ODBC Veri kaynağı Yöneticisi** uygulamasını açın. Bunu, Windows arama kutusuna **ODBC Veri kaynaklarını** yazarak yapabilirsiniz. 
    **Sürücüler** sekmesine tıklayarak ve **Microsoft Azure Cosmos DB ODBC Driver'ın** listelenmesini sağlayarak sürücünün yüklü olduğunu doğrulayabilirsiniz.

    ![Azure Cosmos DB ODBC Veri Kaynak Yöneticisi](./media/odbc-driver/odbc-driver.png)

## <a name="step-2-connect-to-your-azure-cosmos-database"></a><a id="connect"></a>Adım 2: Azure Cosmos veritabanınıza bağlanın

1. [Azure Cosmos DB ODBC sürücüsünü](#install) **ODBC Veri Kaynağı Yöneticisi** penceresine yükledikten sonra **Ekle'yi**tıklatın. Bir Kullanıcı veya Sistem DSN oluşturabilirsiniz. Bu örnekte, bir Kullanıcı DSN oluşturuyorsunuz.

1. Yeni **Veri Kaynağı Oluştur** penceresinde, **Microsoft Azure Cosmos DB ODBC Sürücüsü'nü**seçin ve ardından **Bitir'i**tıklatın.

1. Azure **Cosmos DB ODBC Driver SDN Kurulum** penceresinde aşağıdaki bilgileri doldurun: 

    ![Azure Cosmos DB ODBC Sürücüsü DSN Kurulum penceresi](./media/odbc-driver/odbc-driver-dsn-setup.png)
    - **Veri Kaynak Adı**: ODBC DSN için kendi dostu adınız. Bu ad Azure Cosmos DB hesabınıza özgüdür, bu nedenle birden çok hesabınız varsa uygun şekilde adlandırın.
    - **Açıklaması**: Veri kaynağının kısa bir açıklaması.
    - **Ana bilgisayar**: Azure Cosmos DB hesabınız için URI. Bunu, aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portalındaki Azure Cosmos DB Tuşları sayfasından alabilirsiniz. 
    - **Erişim Anahtarı**: Azure portalındaki Azure Cosmos DB Keys sayfasındaki birincil veya ikincil, okuma-yazma veya salt okunur anahtarı aşağıdaki ekran görüntüsünde gösterildiği gibi. DSN salt okunur veri işleme ve raporlama için kullanılıyorsa salt okunur anahtarını kullanmanızı öneririz.
    ![Azure Cosmos DB Tuşları sayfası](./media/odbc-driver/odbc-cosmos-account-keys.png)
    - **Encrypt Access Key for:** Bu makinenin kullanıcılarına göre en iyi seçimi seçin. 
    
1. Azure Cosmos DB hesabınıza bağlanabildiğinizden emin olmak için **Test** düğmesini tıklatın. 

1.  **Gelişmiş Seçenekler'i** tıklatın ve aşağıdaki değerleri ayarlayın:
    *  **REST API Sürümü**: Operasyonlarınız için [REST API sürümünü](https://docs.microsoft.com/rest/api/cosmos-db/) seçin. Varsayılan 2015-12-16. [Büyük bölme anahtarlarına](large-partition-keys.md) sahip kapsayıcılarınız varsa ve REST API sürümü 2018-12-31 gerektiriyorsa:
        - REST API sürümü için **2018-12-31** yazın
        - **Başlat** menüsünde, **Kayıt Defteri Düzenleyicisi** uygulamasını bulmak ve açmak için "regedit" yazın.
        - Kayıt Defteri Düzenleyicisi'nde, yola gidin: **Bilgisayar\HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBC. INI**
        - "Contoso Account ODBC DSN" gibi DSN'nizle aynı ada sahip yeni bir alt anahtar oluşturun.
        - "Contoso Hesabı ODBC DSN" alt anahtarına gidin.
        - Yeni bir **String** değeri eklemek için sağ tıklatın:
            - Değer Adı: **IgnoreSessionToken**
            - Değer verileri: **1**
            ![Kayıt Defteri Düzenleyicisi ayarları](./media/odbc-driver/cosmos-odbc-edit-registry.png)
    - **Sorgu Tutarlılığı**: İşlemleriniz için [tutarlılık düzeyini](consistency-levels.md) seçin. Varsayılan, Oturum'dur.
    - **Yeniden Deneme Sayısı**: Hizmet hızı sınırlandırılması nedeniyle ilk istek tamamlanmazsa, bir işlemi yeniden denemek için kaç kez girin.
    - **Şema Dosyası**: Burada bir dizi seçeneğiniz vardır.
        - Varsayılan olarak, bu girişi olduğu gibi (boş) bırakarak, sürücü her kapsayıcının şemasını belirlemek için tüm kapsayıcılar için ilk veri sayfasını tarar. Bu, Konteyner Eşleme olarak bilinir. Tanımlanmış bir şema dosyası olmadan, sürücü her sürücü oturumu için taramaya sahip ve DSN kullanarak bir uygulamanın daha yüksek bir başlangıç süresi neden olabilir. Bir DSN için her zaman bir şema dosyanızı ilişkilendirmenizi öneririz.
        - Zaten bir şema dosyanız varsa (muhtemelen Şema Düzenleyicisi'ni kullanarak oluşturduğunuz bir dosya), **Gözat'ı,** dosyanıza göz at, **Kaydet'i**tıklatabilir ve **ardından Tamam'ı**tıklatabilirsiniz.
        - Yeni bir şema oluşturmak istiyorsanız, **Tamam'ı**tıklatın ve ardından ana pencerede **Şema** Düzenleyicisi'ni tıklatın. Daha sonra Şema Editörü bilgilerine devam edin. Yeni şema dosyasını oluşturduktan sonra, yeni oluşturulan şema dosyasını eklemek için **Gelişmiş Seçenekler** penceresine geri dönmeyi unutmayın.

1. **Azure Cosmos DB ODBC Driver DSN Kurulum** penceresini tamamlayıp kapattıktan sonra, yeni Kullanıcı DSN Kullanıcı DSN sekmesine eklenir.

    ![Kullanıcı DSN sekmesinde Yeni Azure Cosmos DB ODBC DSN](./media/odbc-driver/odbc-driver-user-dsn.png)

## <a name="step-3-create-a-schema-definition-using-the-container-mapping-method"></a><a id="#container-mapping"></a>Adım 3: Kapsayıcı eşleme yöntemini kullanarak şema tanımı oluşturma

Kullanabileceğiniz iki tür örnekleme yöntemi vardır: **konteyner eşleme** veya **tablo delimiters.** Örnekleme oturumu her iki örnekleme yöntemini de kullanabilir, ancak her kapsayıcı yalnızca belirli bir örnekleme yöntemini kullanabilir. Aşağıdaki adımlar, kapsayıcı eşleme yöntemini kullanarak bir veya daha fazla kapsayıcıdaki veriler için bir şema oluşturur. Bu örnekleme yöntemi, verilerin yapısını belirlemek için bir kapsayıcının sayfasındaki verileri alır. Bir kapsayıcıyı ODBC tarafındaki bir masaya aktarır. Bu örnekleme yöntemi, bir kaptaki veriler homojen olduğunda verimli ve hızlıdır. Bir kapsayıcı heterojen veri türü içeriyorsa, kapsayıcıdaki veri yapılarını belirlemek için daha sağlam bir örnekleme yöntemi sağladığından [tablo delimiters eşleme yöntemini](#table-mapping) kullanmanızı öneririz. 

1. [Azure Cosmos veritabanınıza Bağlan'da](#connect)1-4 adımlarını tamamladıktan sonra Azure **Cosmos DB ODBC Driver DSN Kurulum** penceresinde **Schema Düzenleyici'yi** tıklatın.

    ![Azure Cosmos DB ODBC Driver DSN Kurulum penceresinde Şema düzenleyici düğmesi](./media/odbc-driver/odbc-driver-schema-editor.png)
1. **Şema Düzenleyicisi** penceresinde **Yeni Oluştur'u**tıklatın.
    **Şema Oluştur** penceresi, Azure Cosmos DB hesabındaki tüm kapsayıcıları görüntüler. 

1. Örneklemek için bir veya daha fazla kapsayıcı seçin ve ardından **Örnek'i**tıklatın. 

1. Tasarım **Görünümü** sekmesinde veritabanı, şema ve tablo temsil edilir. Tablo görünümünde, tarayın sütun adlarıyla ilişkili özellikler kümesini (SQL Adı, Kaynak Adı, vb.) görüntüler.
    Her sütun için, SQL sütunu adını, SQL türünü, SQL uzunluğunu (varsa), Ölçek (varsa), Hassas (varsa) ve Nullable'ı değiştirebilirsiniz.
    - Bu sütunu sorgu sonuçlarından hariç tutmak istiyorsanız **Sütunu** **True** olarak ayarlayabilirsiniz. Sütunu Gizle = true olarak işaretlenmiş sütunlar, şemanın bir parçası olmalarına rağmen seçim ve projeksiyon için döndürülmez. Örneğin, "_" ile başlayan azure cosmos DB sisteminin gerekli tüm özelliklerini gizleyebilirsiniz.
    - **Kimlik** sütunu, normalleştirilmiş şemada birincil anahtar olarak kullanıldığından gizlenemeyen tek alandır. 

1. Şemayı tanımlamayı bitirdikten sonra **Dosya** | **Yı kaydet'i**tıklatın, şemayı kaydetmek için dizine gidin ve sonra **Kaydet'i**tıklatın.

1. Bu şemayı DSN ile kullanmak için **Azure Cosmos DB ODBC Driver DSN Kurulum penceresini** açın (ODBC Veri Kaynağı Yöneticisi aracılığıyla), **Gelişmiş Seçenekler'i**tıklatın ve ardından **Şema Dosyası** kutusunda kaydedilen şemaya gidin. Şema dosyasını varolan bir DSN'ye kaydetmek, DSN bağlantısını şema tarafından tanımlanan veri ve yapının kapsamına göre değiştirir.

## <a name="step-4-create-a-schema-definition-using-the-table-delimiters-mapping-method"></a><a id="table-mapping"></a>Adım 4: Tablo sınır dışılayıcılar eşleme yöntemini kullanarak şema tanımı oluşturma

Kullanabileceğiniz iki tür örnekleme yöntemi vardır: **konteyner eşleme** veya **tablo delimiters.** Örnekleme oturumu her iki örnekleme yöntemini de kullanabilir, ancak her kapsayıcı yalnızca belirli bir örnekleme yöntemini kullanabilir. 

Aşağıdaki adımlar, **tablo sınır dışı layıcılar** eşleme yöntemini kullanarak bir veya daha fazla kapsayıcıdaki veriler için bir şema oluşturur. Kapsayıcılarınız heterojen veri türü içerdiğinde bu örnekleme yöntemini kullanmanızı öneririz. Örneklemeyi bir öznitelik kümesine ve karşılık gelen değerlere genişletmek için bu yöntemi kullanabilirsiniz. Örneğin, bir belge bir "Tür" özelliği içeriyorsa, örneklemeyi bu özelliğin değerlerine göre kapsamda niçin kapsamınızdır. Örneklemenin sonucu, belirttiğiniz Tür için her değer için bir tablo kümesi olacaktır. Örneğin, Type = Car bir Car tablosu üretirken, Type = Plane bir Düzlem tablosu üretecektir.

1. [Azure Cosmos veritabanınıza Bağlan'da](#connect)1-4 adımlarını tamamladıktan sonra Azure Cosmos DB ODBC Driver DSN Kurulum penceresinde **Schema Düzenleyici'yi** tıklatın.

1. **Şema Düzenleyicisi** penceresinde **Yeni Oluştur'u**tıklatın.
    **Şema Oluştur** penceresi, Azure Cosmos DB hesabındaki tüm kapsayıcıları görüntüler. 

1. **Örnek Görünüm** sekmesinde bir kapsayıcı seçin, kapsayıcının **Eşleme Tanımı** sütununda, **Edit'i**tıklatın. Ardından **Eşleme Tanımı** penceresinde **Tablo Delimiters** yöntemini seçin. Ardından şunları yapın:

    a. **Öznitelikler** kutusunda, sınır layıcı özelliğin adını yazın. Bu, belgenizde örneklemeyi kapsamak istediğiniz bir özelliktir, örneğin, Şehir ve enter tuşuna basın. 

    b. Örneklemeyi yalnızca yukarıda girdiğiniz öznitelik için belirli değerlere genişletmek istiyorsanız, seçim kutusundaki özniteliği seçin, **Değer** kutusuna bir değer girin (örneğin Seattle) ve enter tuşuna basın. Öznitelikler için birden çok değer eklemeye devam edebilirsiniz. Değerleri girerken doğru özniteliğin seçildiğinden emin olun.

    Örneğin, Şehrin **Öznitelikleri** değerini eklerseniz ve tablonuzu yalnızca New York ve Dubai şehir değerine sahip satırları içerecek şekilde sınırlamak istiyorsanız, Özellikler kutusuna City' yi, New York' u ve ardından **Değerler** kutusuna Dubai'yi girersiniz.

1. **Tamam**'a tıklayın. 

1. Örneklemek istediğiniz kapsayıcıların eşleme tanımlarını tamamladıktan sonra, **Şema Düzenleyicisi** penceresinde **Örnek'i**tıklatın.
     Her sütun için, SQL sütunu adını, SQL türünü, SQL uzunluğunu (varsa), Ölçek (varsa), Hassas (varsa) ve Nullable'ı değiştirebilirsiniz.
    - Bu sütunu sorgu sonuçlarından hariç tutmak istiyorsanız **Sütunu** **True** olarak ayarlayabilirsiniz. Sütunu Gizle = true olarak işaretlenmiş sütunlar, şemanın bir parçası olmalarına rağmen seçim ve projeksiyon için döndürülmez. Örneğin, `_`'den başlayarak gerekli tüm Azure Cosmos DB sistemini gizleyebilirsiniz.
    - **Kimlik** sütunu, normalleştirilmiş şemada birincil anahtar olarak kullanıldığından gizlenemeyen tek alandır. 

1. Şemayı tanımlamayı bitirdikten sonra **Dosya** | **Yı kaydet'i**tıklatın, şemayı kaydetmek için dizine gidin ve sonra **Kaydet'i**tıklatın.

1. **Azure Cosmos DB ODBC Sürücüsü DSN Kurulum** penceresinde **Gelişmiş Seçenekler'i**tıklatın. Ardından, **Şema Dosyası** kutusunda, kaydedilen şema dosyasına gidin ve **Tamam'ı**tıklatın. DSN'yi kaydetmek için **Tekrar Tamam'ı** tıklatın. Bu, oluşturduğunuz şemayı DSN'ye kaydeder. 

## <a name="optional-set-up-linked-server-connection"></a>(İsteğe bağlı) Bağlantılı sunucu bağlantısını ayarlama

Sql Server Management Studio'dan (SSMS) Azure Cosmos DB'yi bağlantılı bir sunucu bağlantısı kurarak sorgulayabilirsiniz.

1. [Örneğin,](#connect) `SDS Name`Adım 2'de açıklandığı gibi bir sistem veri kaynağı oluşturun.

1. [SQL Server Management Studio'yu yükleyin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) ve sunucuya bağlanın. 

1. SSMS sorgu düzenleyicisinde, aşağıdaki komutları içeren veri kaynağı için bağlantılı bir sunucu nesnesi `DEMOCOSMOS` oluşturun. Bağlı `DEMOCOSMOS` sunucunuzun adı ve `SDS Name` sistem veri kaynağınızın adı ile değiştirin.

    ```sql
    USE [master]
    GO
    
    EXEC master.dbo.sp_addlinkedserver @server = N'DEMOCOSMOS', @srvproduct=N'', @provider=N'MSDASQL', @datasrc=N'SDS Name'
    
    EXEC master.dbo.sp_addlinkedsrvlogin @rmtsrvname=N'DEMOCOSMOS', @useself=N'False', @locallogin=NULL, @rmtuser=NULL, @rmtpassword=NULL
    
    GO
    ```
    
Yeni bağlantılı sunucu adını görmek için Bağlantılı Sunucular listesini yenileyin.

![SSMS'te Bağlantılı Sunucu](./media/odbc-driver/odbc-driver-linked-server-ssms.png)

### <a name="query-linked-database"></a>Sorgu bağlantılı veritabanı

Bağlı veritabanını sorgulamak için bir SSMS sorgusu girin. Bu örnekte, sorgu adlı `customers`kapsayıcıdaki tablodan seçer:

```sql
SELECT * FROM OPENQUERY(DEMOCOSMOS, 'SELECT *  FROM [customers].[customers]')
```

Sorguyu çalıştırın. Sonuç buna benzer olmalıdır:

```
attachments/  1507476156    521 Bassett Avenue, Wikieup, Missouri, 5422   "2602bc56-0000-0000-0000-59da42bc0000"   2015-02-06T05:32:32 +05:00 f1ca3044f17149f3bc61f7b9c78a26df
attachments/  1507476156    167 Nassau Street, Tuskahoma, Illinois, 5998   "2602bd56-0000-0000-0000-59da42bc0000"   2015-06-16T08:54:17 +04:00 f75f949ea8de466a9ef2bdb7ce065ac8
attachments/  1507476156    885 Strong Place, Cassel, Montana, 2069       "2602be56-0000-0000-0000-59da42bc0000"   2015-03-20T07:21:47 +04:00 ef0365fb40c04bb6a3ffc4bc77c905fd
attachments/  1507476156    515 Barwell Terrace, Defiance, Tennessee, 6439     "2602c056-0000-0000-0000-59da42bc0000"   2014-10-16T06:49:04 +04:00      e913fe543490432f871bc42019663518
attachments/  1507476156    570 Ruby Street, Spokane, Idaho, 9025       "2602c156-0000-0000-0000-59da42bc0000"   2014-10-30T05:49:33 +04:00 e53072057d314bc9b36c89a8350048f3
```

> [!NOTE]
> Bağlantılı Cosmos DB sunucusu dört bölümlü adlandırmayı desteklemez. Bir hata aşağıdaki iletiye benzer döndürülür:

```
Msg 7312, Level 16, State 1, Line 44

Invalid use of schema or catalog for OLE DB provider "MSDASQL" for linked server "DEMOCOSMOS". A four-part name was supplied, but the provider does not expose the necessary interfaces to use a catalog or schema.
``` 

## <a name="optional-creating-views"></a>(İsteğe bağlı) Görünüm oluşturma
Örnekleme işleminin bir parçası olarak görünümler tanımlayabilir ve oluşturabilirsiniz. Bu görünümler SQL görünümlerine eşdeğerdir. Bunlar salt okunur ve Azure Cosmos DB SQL sorgusunun tanımlı seçimleri ve projeksiyonlarıdır. 

Verileriniz için bir görünüm oluşturmak için, **Şema Düzenleyicisi** penceresinde, **Görünüm Tanımları** sütununda, örnek almak için kapsayıcının satırına **Ekle'yi** tıklatın. 
    ![Verilerin görünümünü oluşturma](./media/odbc-driver/odbc-driver-create-view.png)


Ardından **Görünüm Tanımları** penceresinde aşağıdakileri yapın:

1. **Yeni'yi**tıklatın, görünüm için bir ad girin, örneğin, Çalışanlar SeattleView'dan ve ardından **Tamam'ı**tıklatın.

1. Görünüm **edin** penceresinde, Azure Cosmos DB sorgusunu girin. Bu bir [Azure Cosmos DB SQL](how-to-sql-query.md) `SELECT c.City, c.EmployeeName, c.Level, c.Age, c.Manager FROM c WHERE c.City = "Seattle"`sorgusu olmalıdır , örneğin , ve sonra **Tamam'ı**tıklatın.

    ![Görünüm oluştururken sorgu ekleme](./media/odbc-driver/odbc-driver-create-view-2.png)


İstediğiniz gibi birçok görünüm oluşturabilirsiniz. Görünümleri tanımlamayı bitirdikten sonra, verileri örnekleyebilirsiniz. 

## <a name="step-5-view-your-data-in-bi-tools-such-as-power-bi-desktop"></a>Adım 5: Verilerinizi Power BI Desktop gibi BI araçlarda görüntüleyin

Yeni DSN'nizi, ODBC uyumlu araçlarla Azure Cosmos DB'ye bağlanmak için kullanabilirsiniz - bu adım yalnızca Power BI Desktop'a nasıl bağlanabileceğinizi ve power bi görselleştirmenasıl oluşturabileceğinizi gösterir.

1. Power BI Desktop’ı açın.

1. **Veri Al'ı**tıklatın.

    ![Power BI Desktop'ta Veri Al](./media/odbc-driver/odbc-driver-power-bi-get-data.png)

1. Veri **Al** penceresinde, **Diğer** | **ODBC** | **Connect'i**tıklatın.

    ![Power BI Veri Al'da ODBC Veri kaynağını seçin](./media/odbc-driver/odbc-driver-power-bi-get-data-2.png)

1. **ODBC** penceresinde, oluşturduğunuz veri kaynağı adını seçin ve sonra **Tamam'ı**tıklatın. **Gelişmiş Seçenekler** girişlerini boş bırakabilirsiniz.

    ![Power BI Get Data'da Veri kaynak adını (DSN) seçin](./media/odbc-driver/odbc-driver-power-bi-get-data-3.png)

1. ODBC sürücü penceresini **kullanan bir veri kaynağına eriş'te** Varsayılan veya **Özel'i** seçin ve ardından **Bağlan'ı**tıklatın. **Kimlik bilgisi bağlantı dize özelliklerini**eklemeniz gerekmez.

1. **Gezgin** penceresinde, sol bölmede veritabanını, şemı genişletin ve ardından tabloyu seçin. Sonuç bölmesi, oluşturduğunuz şema'yı kullanarak verileri içerir.

    ![Power BI Veri Al'da Tablo'yu Seçin](./media/odbc-driver/odbc-driver-power-bi-get-data-4.png)

1. Power BI masaüstündeki verileri görselleştirmek için, tablo adının önündeki kutuyu işaretleyin ve ardından **Yükle'yi**tıklatın.

1. Power BI Desktop'da, en solda Veri sekmesini seçin ![Power BI Masaüstünde Veri sekmesi](./media/odbc-driver/odbc-driver-data-tab.png) verilerinizin alındığını doğrulamak için.

1. Artık Power BI Masaüstünde ![](./media/odbc-driver/odbc-driver-report-tab.png)Rapor sekmesine tıklayarak, Yeni **Görsel'i**tıklatarak ve döşemenizi özelleştirerek Power BI'yi kullanarak görseller oluşturabilirsiniz. Power BI Desktop'da görselleştirme oluşturma hakkında daha fazla bilgi için [Power BI'deki Görselleştirme türlerine](https://powerbi.microsoft.com/documentation/powerbi-service-visualization-types-for-reports-and-q-and-a/)bakın.

## <a name="troubleshooting"></a>Sorun giderme

Aşağıdaki hatayı alırsanız, [Adım 2'de](#connect) Azure portalını kopyaladığınız **Ana Bilgisayar** ve Erişim **Anahtarı** değerlerinin doğru olduğundan emin olun ve ardından yeniden deneyin. Değerleri hatasız kopyalamak için Azure portalındaki **Ana Bilgisayar** ve **Erişim Anahtarı** değerlerinin sağındaki kopyalama düğmelerini kullanın.

    [HY000]: [Microsoft][Azure Cosmos DB] (401) HTTP 401 Authentication Error: {"code":"Unauthorized","message":"The input authorization token can't serve the request. Please check that the expected payload is built as per the protocol, and check the key being used. Server used the following payload to sign: 'get\ndbs\n\nfri, 20 jan 2017 03:43:55 gmt\n\n'\r\nActivityId: 9acb3c0d-cb31-4b78-ac0a-413c8d33e373"}`

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB hakkında daha fazla bilgi edinmek için bkz. [Azure Cosmos DB'ye hoş geldiniz](introduction.md).
