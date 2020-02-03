---
title: BI analizi araçları kullanarak Azure Cosmos DB'ye bağlanma
description: Azure Cosmos DB ODBC sürücüsü, böylece normalleştirilmiş veri BI ve veri analizi yazılımda görüntülenebilir tabloları ve görünümleri oluşturmak için kullanmayı öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: sngun
ms.openlocfilehash: 8be17f0b624c5c34709fb420adb434b77dbc0d91
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721090"
---
# <a name="connect-to-azure-cosmos-db-using-bi-analytics-tools-with-the-odbc-driver"></a>ODBC sürücüsü ile BI analizi araçları kullanarak Azure Cosmos DB'ye bağlanma

Azure Cosmos DB ODBC sürücüsü, çözümleyebilir ve bu çözümleri Azure Cosmos DB verilerinizi görselleştirmeler oluşturmak için SQL Server Integration Services, Power BI Desktop ve Tableau gibi BI analizi araçları kullanarak Azure Cosmos DB'ye bağlanmak sağlar.

Azure Cosmos DB ODBC sürücüsü ODBC 3.8 ile uyumludur ve ANSI SQL-92 söz dizimini destekler. Sürücü, verileri Azure Cosmos DB'de yeniden normalleştirmenize yardımcı olan zengin özellikler sunar. Sürücüyü kullanarak verilerinizi Azure Cosmos DB'de tablo ve görünüm olarak gösterebilirsiniz. Sürücü sorgulara, eklemelere, güncelleştirmelere ve silmelere göre gruplama dahil olmak üzere tablo ve görünümlerde SQL işlemleri gerçekleştirmenizi sağlar.

> [!NOTE]
> ODBC sürücüsü ile Azure Cosmos DB'ye bağlanma, şu anda yalnızca Azure Cosmos DB SQL API hesapları için desteklenir.

## <a name="why-do-i-need-to-normalize-my-data"></a>Verilerim'leri normalleştirmek neden ihtiyacım var?
Azure Cosmos DB hızlı uygulama geliştirme ve veri modelleri için kesin bir şema sınırlı olmadan yineleme özelliğini etkinleştirir şemasız bir veritabanıdır. Tek bir Azure Cosmos veritabanı, çeşitli yapıların JSON belgelerini içerebilir. Bu hızlı uygulama geliştirme için harika bir deneyimdir ancak analiz edin ve veri analizi ve BI araçları kullanarak verilerinizi raporları oluşturmak istediğinizde, verilerin genellikle düzleştirilmiş ve belirli bir şemaya bağlı kalması gerekir.

Bu, ODBC sürücüsü burada devreye girer. ODBC sürücüsünü kullanarak Azure Cosmos DB'de tablolar ve görünümler, verileri analiz ve raporlama ihtiyaçlarını uyan verileri artık normalleştirebilir. Renormalized şemaları, temel alınan veriler üzerinde hiçbir etkisi ve bunları kullanan geliştiricilerin sınırlamak değil. Bunun yerine, verilere erişmek için ODBC uyumlu araçlarından yararlanarak sağlıyor. Bu nedenle, artık Azure Cosmos veritabanınız yalnızca geliştirme ekibiniz için sık kullanılanlara sahip olmayacaktır, ancak veri analistlerinizi de çok seveceksiniz.

ODBC sürücüsü ile başlayalım.

## <a id="install"></a>1. Adım: Azure Cosmos DB ODBC sürücüsünü yükler

1. Ortamınız için sürücüleri yükleyin:

    | Yükleyici | Desteklenen işletim sistemleri| 
    |---|---| 
    |64 bit Windows için [MICROSOFT Azure Cosmos DB ODBC 64-bit. msi](https://aka.ms/cosmos-odbc-64x64)| 64-bit sürüm Windows 8.1 veya üstü, Windows 8, Windows 7, Windows Server 2012 R2, Windows Server 2012 ve Windows Server 2008 R2.| 
    |64-bit Windows üzerinde 32-bit için [ODBC 32x64-bit. msi Microsoft Azure Cosmos DB](https://aka.ms/cosmos-odbc-32x64)| 64-bit sürüm Windows 8.1 veya üstü, Windows 8, Windows 7, Windows XP, Windows Vista, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ve Windows Server 2003.| 
    |32 bit Windows için [MICROSOFT Azure Cosmos DB ODBC 32-bit. msi](https://aka.ms/cosmos-odbc-32x32)|32-bit sürüm Windows 8.1 veya üstü, Windows 8, Windows 7, Windows XP ve Windows Vista.|

    MSI dosyasını yerel olarak çalıştırın, bu, **MICROSOFT Azure Cosmos DB ODBC Sürücü Yükleme Sihirbazı**'nı başlatır. 

1. Giriş ODBC sürücüsünü yüklemek için varsayılan değeri kullanmanın yükleme sihirbazını tamamlayın.

1. Bilgisayarınızda **ODBC veri kaynağı Yöneticisi** uygulamasını açın. Windows Arama kutusuna **ODBC veri kaynakları** yazarak bunu yapabilirsiniz. 
    **Sürücüler** sekmesine TıKLAYıP **Microsoft Azure Cosmos DB ODBC sürücüsünün** listelendiğinden emin olmak için sürücünün yüklendiğini doğrulayabilirsiniz.

    ![Azure Cosmos DB ODBC Veri Kaynağı Yöneticisi](./media/odbc-driver/odbc-driver.png)

## <a id="connect"></a>2. Adım: Azure Cosmos veritabanınıza bağlanma

1. [Azure Cosmos DB ODBC sürücüsünü](#install)yükledikten sonra, **ODBC veri kaynağı Yöneticisi** penceresinde **Ekle**' ye tıklayın. Bir kullanıcı veya sistem DSN'si oluşturabilirsiniz. Bu örnekte, bir kullanıcı DSN'si oluşturuyorsunuz.

1. **Yeni veri kaynağı oluştur** penceresinde, **Microsoft Azure Cosmos DB ODBC sürücüsü**' nü seçin ve ardından **son**' a tıklayın.

1. **Azure Cosmos DB ODBC sürücüsü SDN kurulumu** penceresinde, aşağıdaki bilgileri girin: 

    ![Azure Cosmos DB ODBC sürücüsü DSN Kurulum penceresi](./media/odbc-driver/odbc-driver-dsn-setup.png)
    - **Veri kaynağı adı**: ODBC DSN için kendi kolay adınız. Bu Azure Cosmos DB hesabınız için benzersiz bir addır, birden çok hesabı varsa, bu nedenle uygun şekilde adlandırın.
    - **Açıklama**: veri kaynağının kısa bir açıklaması.
    - **Ana bilgisayar**: Azure Cosmos DB hesabınız için URI. Bu Azure Cosmos DB anahtarlar sayfasından Azure portalında, aşağıdaki ekran görüntüsünde gösterildiği gibi alabilirsiniz. 
    - **Erişim anahtarı**: aşağıdaki ekran görüntüsünde gösterildiği gibi Azure Portal Azure Cosmos DB anahtarları sayfasından birincil veya ikincil, salt okuma veya salt yazılır anahtar. DSN salt okunur veri işleme ve raporlama için kullanılıyorsa, salt okunur anahtarı kullanmanızı öneririz.
    ![Azure Cosmos DB anahtarlar sayfası](./media/odbc-driver/odbc-cosmos-account-keys.png)
    - **Için erişim anahtarını şifreleyin**: Bu makinenin kullanıcılarına göre en iyi seçimi seçin. 
    
1. Azure Cosmos DB hesabınıza bağlanabildiğinizden emin olmak için **Test** düğmesine tıklayın. 

1.  **Gelişmiş Seçenekler** ' e tıklayın ve aşağıdaki değerleri ayarlayın:
    *  **REST API sürüm**: işlemlerinizin [REST API sürümünü](https://docs.microsoft.com/rest/api/cosmos-db/) seçin. Varsayılan 2015-12-16. [Büyük bölüm anahtarları](large-partition-keys.md) olan kapsayıcınız varsa ve REST API sürüm 2018-12-31 ' yı gerekliyse:
        - REST API sürümü için **2018-12-31** yazın
        - **Kayıt defteri Düzenleyicisi** uygulamasını bulmak ve açmak için **Başlat** menüsünde "regedit" yazın.
        - Kayıt Defteri Düzenleyicisi 'nde yola gidin: **bilgisayar \ HKEY_LOCAL_MACHINE \SOFTWARE\ODBC\ODBC. INI**
        - DSN 'niz ile aynı ada sahip yeni bir alt anahtar oluşturun, örn. "contoso hesabı ODBC DSN".
        - "Contoso hesabı ODBC DSN" alt anahtarına gidin.
        - Yeni bir **dize** değeri eklemek için sağ tıklayın:
            - Değer adı: **ıgnoresessiontoken**
            - Değer verisi: **1**
            ![kayıt defteri düzenleyicisi ayarları](./media/odbc-driver/cosmos-odbc-edit-registry.png)
    - **Sorgu tutarlılığı**: işlemlerinizin [tutarlılık düzeyini](consistency-levels.md) seçin. Varsayılan oturumdur.
    - **Yeniden deneme sayısı**: hizmet hızı sınırlaması nedeniyle ilk istek tamamlanmazsa bir işlemin yeniden denenme sayısını girin.
    - **Şema dosyası**: burada birkaç seçeneğiniz vardır.
        - Varsayılan olarak, bu girişi olduğu gibi bırakmak (boş), sürücü her kapsayıcının şemasını belirlemede tüm kapsayıcılar için verilerin ilk sayfasını tarar. Bu, kapsayıcı eşlemesi olarak bilinir. Tanımlı bir şema dosyası olmadan sürücü her bir sürücü oturumu için tarama yapması ve uygulamanın DSN daha yüksek başlangıç saati, neden olabilir. Bir şema dosyası her zaman için DSN ilişkilendirmenizi öneririz.
        - Zaten bir şema dosyanız varsa (Belki de şema düzenleyicisini kullanarak oluşturduğunuz), git ' e tıklayabilir, dosyanıza gidebilir, **Kaydet**' e ve ardından **Tamam**' **a tıklayabilirsiniz.**
        - Yeni bir şema oluşturmak istiyorsanız **Tamam**' a ve ardından Ana penceredeki **şema Düzenleyicisi** ' ne tıklayın. Ardından şema Düzenleyicisi bilgilerine ilerleyin. Yeni şema dosyasını oluşturduktan sonra, yeni oluşturulan şema dosyasını eklemek için **Gelişmiş Seçenekler** penceresine geri dönüp unutmayın.

1. **Azure Cosmos DB ODBC sürücüsü DSN kurulum** penceresini tamamlayıp kapattıktan sonra, yenı Kullanıcı DSN 'SI Kullanıcı DSN sekmesine eklenir.

    ![Yeni Azure Cosmos DB ODBC DSN Kullanıcı DSN sekmesinde](./media/odbc-driver/odbc-driver-user-dsn.png)

## <a id="#container-mapping"></a>3. Adım: kapsayıcı eşleme yöntemini kullanarak bir şema tanımı oluşturma

Kullanabileceğiniz iki tür örnekleme yöntemi vardır: **kapsayıcı eşleme** veya **tablo sınırlayıcıları**. Örnekleme oturumu her iki örnekleme yönteminden yararlanabilir, ancak her kapsayıcı yalnızca belirli bir örnekleme yöntemi kullanabilir. Aşağıdaki adımlar, kapsayıcı eşleme yöntemi kullanılarak bir veya daha fazla kapsayıcıdaki veriler için bir şema oluşturur. Bu örnekleme yöntemi, verilerin yapısını belirleyebilmek için bir kapsayıcının sayfasındaki verileri alır. ODBC tarafındaki bir tabloya kapsayıcı atar. Bu örnekleme yöntemi, bir kapsayıcıdaki veriler hogenou olduğunda etkilidir ve hızlıdır. Bir kapsayıcı farklı türde veriler içeriyorsa, kapsayıcıdaki veri yapılarını belirlemede daha sağlam bir örnekleme yöntemi sağladığından [tablo sınırlandırıcı eşleme yöntemini](#table-mapping) kullanmanızı öneririz. 

1. [Azure Cosmos veritabanınıza bağlanma](#connect)bölümündeki 1-4 adımlarını tamamladıktan sonra, **Azure Cosmos DB ODBC sürücüsü DSN kurulum** penceresinde **şema Düzenleyicisi** ' ne tıklayın.

    ![Azure Cosmos DB ODBC sürücü DSN Kurulumu penceresinde şema Düzenleyici düğmesi](./media/odbc-driver/odbc-driver-schema-editor.png)
1. **Şema Düzenleyicisi** penceresinde **Yeni oluştur**' a tıklayın.
    **Şema oluştur** penceresi Azure Cosmos DB hesabındaki tüm kapsayıcıları görüntüler. 

1. Örnek olarak bir veya daha fazla kapsayıcı seçin ve ardından **örnek**' e tıklayın. 

1. **Tasarım görünümü** sekmesinde veritabanı, şema ve tablo gösterilir. Tablo Görünümü'nde tarama sütun adları (SQL adı, kaynak adı, vb.) ile ilişkili özellikler kümesini görüntüler.
    Her sütun için değiştirebileceğiniz sütun SQL adı, SQL türü SQL uzunluğu (varsa), (varsa) ölçek, duyarlık (varsa) ve boş değer.
    - Sütunu sorgu sonuçlarından dışlamak istiyorsanız, **sütunu** **true** olarak ayarlayabilirsiniz. Sütunları gizleme sütun işaretli = true yine de şemanın bir parçası olsa seçimi ve projeksiyon döndürülmez. Örneğin, "_" ile başlayan tüm Azure Cosmos DB sistem için gerekli özellikler gizleyebilirsiniz.
    - **Kimlik** sütunu, Normalleştirilmemiş şemada birincil anahtar olarak kullanıldığı için gizlenemediği tek alandır. 

1. Şemayı tanımlamayı tamamladıktan sonra **dosya** | **Kaydet**' e tıklayın, şemayı kaydetmek Için dizine gidin ve **Kaydet**' e tıklayın.

1. Bu şemayı bir DSN ile kullanmak için, **Azure Cosmos DB ODBC sürücü DSN kurulum penceresini** açın (ODBC veri kaynağı Yöneticisi aracılığıyla), **Gelişmiş Seçenekler**' e tıklayın ve ardından **şema dosyası** kutusunda, kaydedilen şemaya gidin. Var olan bir DSN bir şema dosyası kaydetme veri ve şema tarafından tanımlanan yapısına kapsamına DSN bağlantı değiştirir.

## <a id="table-mapping"></a>4. Adım: tablo sınırlayıcıları eşleme yöntemini kullanarak bir şema tanımı oluşturma

Kullanabileceğiniz iki tür örnekleme yöntemi vardır: **kapsayıcı eşleme** veya **tablo sınırlayıcıları**. Örnekleme oturumu her iki örnekleme yönteminden yararlanabilir, ancak her kapsayıcı yalnızca belirli bir örnekleme yöntemi kullanabilir. 

Aşağıdaki adımlarda **tablo sınırlayıcıları** eşleme yöntemi kullanılarak bir veya daha fazla kapsayıcıda bulunan veriler için bir şema oluşturulur. Bu örnekleme yöntemini, kapsayıcılarınız heterojen veri türü içerdiğinde kullanmanızı öneririz. Bir dizi öznitelikleri ve karşılık gelen değerleri örnekleme kapsamını belirlemek için bu yöntemi kullanabilirsiniz. Örneğin, bir belge bir "Type" özelliği içeriyorsa, bu özelliğin değerlerine örnekleme kapsamını belirleyebilirsiniz. Örnekleme nihai sonucu tabloların her biri, belirttiğiniz türü değerleri için bir küme olacaktır. Örneğin = araba, bir araba tablosu türü üretir = düz bir düz tablo oluşturmak.

1. [Azure Cosmos veritabanınıza bağlanma](#connect)bölümündeki 1-4 adımlarını tamamladıktan sonra, Azure Cosmos DB ODBC sürücüsü DSN kurulum penceresinde **şema Düzenleyicisi** ' ne tıklayın.

1. **Şema Düzenleyicisi** penceresinde **Yeni oluştur**' a tıklayın.
    **Şema oluştur** penceresi Azure Cosmos DB hesabındaki tüm kapsayıcıları görüntüler. 

1. **Örnek görünüm** sekmesinde bir kapsayıcı seçin, kapsayıcının **eşleme tanımı** sütununda, **Düzenle**' ye tıklayın. Ardından, **eşleme tanımı** penceresinde **tablo sınırlayıcıları** yöntemi ' ni seçin. Ardından şunları yapın:

    a. **Öznitelikler** kutusuna bir sınırlayıcı özelliğinin adını yazın. Bu kapsam örnekleme için örneğin, şehir ve enter tuşuna basın, belgedeki bir özelliğidir. 

    b. Örneklemeyi yukarıda girdiğiniz özniteliğe ilişkin belirli değerlere bağlamak istiyorsanız, seçim kutusunda özniteliği seçin, **değer** kutusuna bir değer girin (örn. Seattle) ve ENTER tuşuna basın. Öznitelikler için birden çok değer eklemeye devam edebilirsiniz. Yalnızca doğru öznitelik değerleri girerken seçili olduğundan emin olun.

    Örneğin, şehir olarak bir **öznitelik** değeri eklerseniz ve tablonuzu yalnızca New York ve Dubai City değeri olan satırları içerecek şekilde sınırlandırmak Istiyorsanız, Attributes kutusuna City ve New York ' a ve ardından **değerler** kutusuna Dubai yazın.

1. **Tamam** düğmesine tıklayın. 

1. Örneklemek istediğiniz kapsayıcıların eşleme tanımlarını tamamladıktan sonra, **şema Düzenleyicisi** penceresinde **örnek**' e tıklayın.
     Her sütun için değiştirebileceğiniz sütun SQL adı, SQL türü SQL uzunluğu (varsa), (varsa) ölçek, duyarlık (varsa) ve boş değer.
    - Sütunu sorgu sonuçlarından dışlamak istiyorsanız, **sütunu** **true** olarak ayarlayabilirsiniz. Sütunları gizleme sütun işaretli = true yine de şemanın bir parçası olsa seçimi ve projeksiyon döndürülmez. Örneğin, `_`başlayan tüm Azure Cosmos DB sistem gereken özelliklerini gizleyebilirsiniz.
    - **Kimlik** sütunu, Normalleştirilmemiş şemada birincil anahtar olarak kullanıldığı için gizlenemediği tek alandır. 

1. Şemayı tanımlamayı tamamladıktan sonra **dosya** | **Kaydet**' e tıklayın, şemayı kaydetmek Için dizine gidin ve **Kaydet**' e tıklayın.

1. **Azure Cosmos DB ODBC sürücüsü DSN kurulum** penceresinde geri dönüp **Gelişmiş Seçenekler**' e tıklayın. Ardından, **şema dosyası** kutusunda, kaydedilmiş şema dosyasına gidin ve **Tamam**' a tıklayın. DSN 'yi kaydetmek için yeniden **Tamam** ' a tıklayın. Bu, oluşturduğunuz şema DSN'ye kaydeder. 

## <a name="optional-set-up-linked-server-connection"></a>(İsteğe bağlı) Bağlı sunucu bağlantısı kurma

Azure Cosmos DB SQL Server Management Studio (SSMS) gelen bir bağlı sunucu bağlantısı kurmak ayarlayarak sorgulayabilirsiniz.

1. Örneğin `SDS Name`adlı [Adım 2](#connect)' de açıklandığı gibi bir sistem veri kaynağı oluşturun.

1. [SQL Server Management Studio yükleyip](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) sunucuya bağlanın. 

1. SSMS sorgu Düzenleyicisi 'nde, veri kaynağı için aşağıdaki komutlarla bağlı bir sunucu nesnesi `DEMOCOSMOS` oluşturun. `DEMOCOSMOS`, bağlı sunucunuzun adı ile ve `SDS Name` sistem veri kaynağınızın adıyla değiştirin.

    ```sql
    USE [master]
    GO
    
    EXEC master.dbo.sp_addlinkedserver @server = N'DEMOCOSMOS', @srvproduct=N'', @provider=N'MSDASQL', @datasrc=N'SDS Name'
    
    EXEC master.dbo.sp_addlinkedsrvlogin @rmtsrvname=N'DEMOCOSMOS', @useself=N'False', @locallogin=NULL, @rmtuser=NULL, @rmtpassword=NULL
    
    GO
    ```
    
Yeni bağlantılı sunucu adıyla görmek için bağlı sunucular listesini yenileyin.

![Ssms'de bağlantılı sunucu](./media/odbc-driver/odbc-driver-linked-server-ssms.png)

### <a name="query-linked-database"></a>Bağlantılı veritabanı sorgulama

Bağlantılı veritabanı sorgulamak için bir SSMS sorgu girin. Bu örnekte sorgu, `customers`adlı kapsayıcıdaki tablodan seçim yapar:

```sql
SELECT * FROM OPENQUERY(DEMOCOSMOS, 'SELECT *  FROM [customers].[customers]')
```

Sorguyu yürütün. Sonuç şuna benzer olmalıdır:

```
attachments/  1507476156    521 Bassett Avenue, Wikieup, Missouri, 5422   "2602bc56-0000-0000-0000-59da42bc0000"   2015-02-06T05:32:32 +05:00 f1ca3044f17149f3bc61f7b9c78a26df
attachments/  1507476156    167 Nassau Street, Tuskahoma, Illinois, 5998   "2602bd56-0000-0000-0000-59da42bc0000"   2015-06-16T08:54:17 +04:00 f75f949ea8de466a9ef2bdb7ce065ac8
attachments/  1507476156    885 Strong Place, Cassel, Montana, 2069       "2602be56-0000-0000-0000-59da42bc0000"   2015-03-20T07:21:47 +04:00 ef0365fb40c04bb6a3ffc4bc77c905fd
attachments/  1507476156    515 Barwell Terrace, Defiance, Tennessee, 6439     "2602c056-0000-0000-0000-59da42bc0000"   2014-10-16T06:49:04 +04:00      e913fe543490432f871bc42019663518
attachments/  1507476156    570 Ruby Street, Spokane, Idaho, 9025       "2602c156-0000-0000-0000-59da42bc0000"   2014-10-30T05:49:33 +04:00 e53072057d314bc9b36c89a8350048f3
```

> [!NOTE]
> Bağlı bir Cosmos DB sunucusunun dört kısımlı adlandırmayı desteklemiyor. Hata aşağıdaki iletiye benzer döndürülür:

```
Msg 7312, Level 16, State 1, Line 44

Invalid use of schema or catalog for OLE DB provider "MSDASQL" for linked server "DEMOCOSMOS". A four-part name was supplied, but the provider does not expose the necessary interfaces to use a catalog or schema.
``` 

## <a name="optional-creating-views"></a>(İsteğe bağlı) Görünümler oluşturma
Tanımlayabilir ve örnekleme işleminin bir parçası görünümler oluşturun. Bu görünümler için SQL görünümleri eşdeğerdir. Bunlar salt okunur ve yansıtmaların tanımlanan Azure Cosmos DB SQL sorgusu ve seçimleri kapsamı içindedir. 

Verilerinize yönelik bir görünüm oluşturmak için **şema Düzenleyicisi** penceresinde, **tanımları görüntüle** sütununda, kapsayıcının örneğine örnek olarak **Ekle** ' ye tıklayın. 
    ![veri görünümü oluşturma](./media/odbc-driver/odbc-driver-create-view.png)


Ardından **tanımları görüntüle** penceresinde şunları yapın:

1. **Yeni**' ye tıklayın, görünüm için bir ad girin, örneğin, EmployeesfromSeattleView ve ardından **Tamam**' a tıklayın.

1. **Görünüm düzenleme** penceresinde bir Azure Cosmos DB sorgusu girin. Bu, örneğin `SELECT c.City, c.EmployeeName, c.Level, c.Age, c.Manager FROM c WHERE c.City = "Seattle"`[Azure Cosmos DB BIR SQL sorgusu](how-to-sql-query.md)olmalıdır ve ardından **Tamam**' a tıklayın.

    ![Bir görünüm oluşturma sırasında Sorgu Ekle](./media/odbc-driver/odbc-driver-create-view-2.png)


İstediğiniz gibi birçok bir görünüm oluşturabilirsiniz. İşiniz bittiğinde görünümleri tanımlama, ardından veri örnekleme yapabilirsiniz. 

## <a name="step-5-view-your-data-in-bi-tools-such-as-power-bi-desktop"></a>5\. adım: Power BI Desktop gibi BI Araçları'ndaki verilerinizi görüntüleyin

ODBC uyumlu herhangi bir aracı ile Azure Cosmos DB'ye bağlanmak için yeni DSN kullanabilirsiniz: Bu adım yalnızca, bağlanmak için Power BI Desktop ve Power BI görselleştirmeleri oluşturma işlemini göstermektedir.

1. Power BI Desktop’ı açın.

1. **Veri al**' a tıklayın.

    ![Power BI Desktop'ta Veri Al](./media/odbc-driver/odbc-driver-power-bi-get-data.png)

1. **Veri al** penceresinde, **diğer** | **ODBC** | **Bağlan**' a tıklayın.

    ![Power BI Veri Al ODBC veri kaynağı seçin](./media/odbc-driver/odbc-driver-power-bi-get-data-2.png)

1. **ODBC** penceresinde, oluşturduğunuz veri kaynağı adını seçin ve ardından **Tamam**' a tıklayın. **Gelişmiş Seçenekler** girdilerini boş bırakabilirsiniz.

    ![Power BI Veri Al veri kaynağı adı (DSN) seçin](./media/odbc-driver/odbc-driver-power-bi-get-data-3.png)

1. **BIR ODBC sürücüsü kullanarak bir veri kaynağına erişin** penceresinde, **varsayılan veya özel** ' i seçin ve ardından **Bağlan**' a tıklayın. **Kimlik bilgisi bağlantı dizesi özelliklerini**eklemeniz gerekmez.

1. **Gezgin** penceresinde, sol bölmede veritabanını, şemayı genişletin ve ardından tabloyu seçin. Sonuçlar bölmesinde, oluşturduğunuz şemayı kullanarak veriler içerir.

    ![Power BI Get verileri tablo seçin](./media/odbc-driver/odbc-driver-power-bi-get-data-4.png)

1. Power BI masaüstündeki verileri görselleştirmek için tablo adının önündeki kutuyu işaretleyin ve ardından **Yükle**' ye tıklayın.

1. En sağda bulunan Power BI Desktop'ta sol, veri sekmesinde seçin ![Power BI Desktop'ta veri sekmesi](./media/odbc-driver/odbc-driver-data-tab.png) Verilerinizi onaylamak için içeri aktarıldı.

1. Artık, Power BI Desktop](./media/odbc-driver/odbc-driver-report-tab.png)rapor sekmesine ![tıklayıp **yeni görsel**' e tıklayıp Kutucuğunuzu özelleştirerek Power BI kullanarak görsel oluşturabilirsiniz. Power BI Desktop görselleştirmeler oluşturma hakkında daha fazla bilgi için bkz. [Power BI Içindeki görselleştirme türleri](https://powerbi.microsoft.com/documentation/powerbi-service-visualization-types-for-reports-and-q-and-a/).

## <a name="troubleshooting"></a>Sorun giderme

Aşağıdaki hatayı alırsanız [Adım 2](#connect) ' de Azure Portal kopyaladığınız **ana bilgisayar** ve **erişim anahtarı** değerlerinin doğru olduğundan emin olun ve yeniden deneyin. Değer hatasını serbest bir şekilde kopyalamak için **konağın** sağ tarafındaki Kopyala düğmelerini ve Azure Portal **erişim anahtarı** değerlerini kullanın.

    [HY000]: [Microsoft][Azure Cosmos DB] (401) HTTP 401 Authentication Error: {"code":"Unauthorized","message":"The input authorization token can't serve the request. Please check that the expected payload is built as per the protocol, and check the key being used. Server used the following payload to sign: 'get\ndbs\n\nfri, 20 jan 2017 03:43:55 gmt\n\n'\r\nActivityId: 9acb3c0d-cb31-4b78-ac0a-413c8d33e373"}`

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB hakkında daha fazla bilgi edinmek için bkz. [Azure Cosmos DB'ye hoş geldiniz](introduction.md).
