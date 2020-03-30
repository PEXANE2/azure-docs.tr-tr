---
title: Azure Veri Gölü Analizi kodunuzu nasıl test emilir?
description: Azure Veri Gölü Analitiği için U-SQL ve genişletilmiş C# kodu için test örnekleri eklemeyi öğrenin.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 08/30/2019
ms.openlocfilehash: d568a267952a22d2e7a6b7acb6d54cf41f803367
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70913955"
---
# <a name="test-your-azure-data-lake-analytics-code"></a>Azure Veri Gölü Analizi kodunuzu test edin

Azure Veri Gölü [U-SQL](data-lake-analytics-u-sql-get-started.md) dilini sağlar. U-SQL, verileri herhangi bir ölçekte işlemek için bildirimsel SQL ile zorunlu C# ile birleştirir. Bu belgede, U-SQL ve genişletilmiş C# kullanıcı tanımlı işleç (UDO) kodu için test örnekleri oluşturmayı öğrenirsiniz.

## <a name="test-u-sql-scripts"></a>U-SQL betiklerini test etme

U-SQL komut dosyası, Azure'da veya yerel bilgisayarınızda çalıştırılabilir kod için derlenir ve optimize edilir. Derleme ve optimizasyon işlemi, tüm U-SQL komut dosyasını bir bütün olarak ele alar. Her ifade için geleneksel bir birim testi yapamazsınız. Ancak, U-SQL testi SDK ve yerel çalıştırış SDK kullanarak komut dosyası düzeyinde testler yapabilirsiniz.

### <a name="create-test-cases-for-u-sql-script"></a>U-SQL komut dosyası için test örnekleri oluşturma

Visual Studio için Azure Veri Gölü Araçları, U-SQL komut dosyası test örnekleri oluşturmanıza olanak tanır.

1. Solution Explorer'da bir U-SQL komut dosyasına sağ tıklayın ve ardından **Birim Testi Oluştur'u**seçin.

1. Yeni bir test projesi oluşturun veya test çalışması varolan bir test projesine ekleyin.

   ![Visual Studio için Veri Gölü Araçları -- Bir U-SQL test projesi yapılandırması oluşturma](./media/data-lake-analytics-cicd-test/data-lake-tools-create-usql-test-project-configure.png)

### <a name="manage-the-test-data-source"></a>Test veri kaynağını yönetme

U-SQL komut dosyalarını sınadiğinizde, test giriş dosyalarına ihtiyacınız vardır. **Çözüm Gezgini'nde**test verilerini yönetmek için U-SQL projesine sağ tıklayın ve **Özellikler'i**seçin. **Test Veri Kaynağı'na**bir kaynak girebilirsiniz.

![Visual Studio için Veri Gölü Araçları -- proje test veri kaynağını yapılandırma](./media/data-lake-analytics-cicd-test/data-lake-tools-configure-project-test-data-source.png)

U-SQL `Initialize()` test SDK'daki arabirimi çağırdığınızda, test projesinin çalışma dizininin altında geçici bir yerel veri kökü klasörü oluşturulur. U-SQL komut dosyası test taleplerini çalıştırmadan önce test veri kaynağı klasöründeki tüm dosya ve klasörler geçici yerel veri kökü klasörüne kopyalanır. Test veri klasörü yolunu bir yarı nokta nokta ile bölerek daha fazla test veri kaynağı klasörü ekleyebilirsiniz.

### <a name="manage-the-database-environment-for-testing"></a>Sınama için veritabanı ortamını yönetme

U-SQL komut dosyalarınız U-SQL veritabanı nesneleriyle kullanıyorsa veya sorgulanıyorsa, U-SQL test denemelerini çalıştırmadan önce veritabanı ortamını başlatmanız gerekir. Bu yaklaşım, depolanan yordamları ararken gerekli olabilir. U-SQL testi SDK'daki arabirim, `Initialize()` U-SQL projesi tarafından başvurulan tüm veritabanlarını test projesinin çalışma dizinindeki geçici yerel veri kökü klasörüne dağıtmanıza yardımcı olur.

U-SQL projesi için U-SQL veritabanı proje başvurularını nasıl yönetilir hakkında daha fazla bilgi için [bkz.](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project)

### <a name="verify-test-results"></a>Test sonuçlarını doğrulama

`Run()` Arabirim bir iş yürütme sonucu döndürür. *0* başarı, *1* başarısızlık demektir. Çıktıları doğrulamak için C# assert işlevlerini de kullanabilirsiniz.

### <a name="run-test-cases-in-visual-studio"></a>Visual Studio'da test test lerini çalıştırma

Bir U-SQL komut dosyası test projesi, C# birimi test çerçevesinin üzerine inşa edilmiştir. Projeyi yaptıktan**sonra, Windows** > **Test Gezgini'ni** **test** > et'i seçin. **Test Gezgini'nden**test çalışmaları çalıştırabilirsiniz. Alternatif olarak, birim testinizdeki .cs dosyasına sağ tıklayın ve **Testleri Çalıştır'ı**seçin.

## <a name="test-c-udos"></a>Test C# UDOs

### <a name="create-test-cases-for-c-udos"></a>C# UdO'lar için test çalışmaları oluşturma

C# kullanıcı tanımlı operatörlerinizi (UdOs) test etmek için C# birimi test çerçevesi kullanabilirsiniz. UdOs test ederken, giriş olarak ilgili **IRowset** nesneleri hazırlamak gerekir.

**Bir IRowset** nesnesi oluşturmanın iki yolu vardır:

- **IRowset**oluşturmak için bir dosyadan veri yükleme:

    ```csharp
    //Schema: "a:int, b:int"
    USqlColumn<int> col1 = new USqlColumn<int>("a");
    USqlColumn<int> col2 = new USqlColumn<int>("b");
    List<IColumn> columns = new List<IColumn> { col1, col2 };
    USqlSchema schema = new USqlSchema(columns);

    //Generate one row with default values
    IUpdatableRow output = new USqlRow(schema, null).AsUpdatable();

    //Get data from file
    IRowset rowset = UnitTestHelper.GetRowsetFromFile(@"processor.txt", schema, output.AsReadOnly(), discardAdditionalColumns: true, rowDelimiter: null, columnSeparator: '\t');
    ```

- **IRowset**oluşturmak için bir veri koleksiyonundan verileri kullanın:

    ```csharp
    //Schema: "a:int, b:int"
    USqlSchema schema = new USqlSchema(
        new USqlColumn<int>("a"),
        new USqlColumn<int>("b")
    );

    IUpdatableRow output = new USqlRow(schema, null).AsUpdatable();

    //Generate Rowset with specified values
    List<object[]> values = new List<object[]>{
        new object[2] { 2, 3 },
        new object[2] { 10, 20 }
    };

    IEnumerable<IRow> rows = UnitTestHelper.CreateRowsFromValues(schema, values);
    IRowset rowset = UnitTestHelper.GetRowsetFromCollection(rows, output.AsReadOnly());
    ```

### <a name="verify-test-results"></a>Test sonuçlarını doğrulama

UDO işlevlerini aradıktan sonra, C# assert işlevlerini kullanarak sonuçları şema ve Rowset değer doğrulama yoluyla doğrulayabilirsiniz. Çözümünüze **bir U-SQL C# UDO Birim Test Projesi** ekleyebilirsiniz. Bunu yapmak için Visual **Studio'da Dosya > Yeni > Projesi'ni** seçin.

### <a name="run-test-cases-in-visual-studio"></a>Visual Studio'da test test lerini çalıştırma

Projeyi yaptıktan**sonra, Windows** > **Test Gezgini'ni** **test** > et'i seçin. **Test Gezgini'nden**test çalışmaları çalıştırabilirsiniz. Alternatif olarak, birim testinizdeki .cs dosyasına sağ tıklayın ve **Testleri Çalıştır'ı**seçin.

## <a name="run-test-cases-in-azure-pipelines"></a>Azure Ardışık Hatlar'da test servis taleplerini çalıştırma<a name="run-test-cases-in-azure-devops"></a>

Hem **U-SQL komut dosyası test projeleri** hem de **C# UDO test projeleri** C# birim test projelerini devralır. Azure Ardışık Hatlar'daki [Visual Studio test görevi](https://docs.microsoft.com/azure/devops/pipelines/test/getting-started-with-continuous-testing?view=vsts) bu test servis taleplerini çalıştırabilir.

### <a name="run-u-sql-test-cases-in-azure-pipelines"></a>Azure Ardışık Hatları'nda U-SQL test servis taleplerini çalıştırma

U-SQL testi için, yapı `CPPSDK` bilgisayarınıza yüklediğinizden emin `CPPSDK` olun `USqlScriptTestRunner(cppSdkFolderFullPath: @"")`ve ardından yolu 'ye' ye geçirin.

#### <a name="what-is-cppsdk"></a>CPPSDK nedir?

CPPSDK, Microsoft Visual C++ 14 ve Windows SDK 10.0.10240.0 içeren bir pakettir. Bu paket, U-SQL çalışma zamanının gerektirdiği ortamı içerir. Bu paketi Visual Studio yükleme klasörü için Azure Veri Gölü Araçları altında niçin alabilirsiniz:

- Visual Studio 2015 için`C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK`
- Visual Studio 2017 için`C:\Program Files (x86)\Microsoft Visual Studio\2017\<Visual Studio Edition>\SDK\ScopeCppSDK`
- Visual Studio 2019 için`C:\Program Files (x86)\Microsoft Visual Studio\2019\<Visual Studio Edition>\SDK\ScopeCppSDK`

#### <a name="prepare-cppsdk-in-the-azure-pipelines-build-agent"></a>Azure Ardışık Hatları yapı aracısında CPPSDK'yi hazırlayın

Azure Ardışık Hatları'nda CPPSDK bağımlılığını hazırlamanın en yaygın yolu aşağıdaki gibidir:

1. CPPSDK kitaplıklarını içeren klasörü zipleyin.

1. .zip dosyasını kaynak kontrol sisteminize iade edin. .zip dosyası, dosyaların bir `.gitignore` dosya nedeniyle göz ardı edilmemesi için CPPSDK klasörü altındaki tüm kitaplıkları iade etmenizi sağlar.

1. Yapı ardışık hattındaki .zip dosyasının zip'ini açın.

1. Yapı `USqlScriptTestRunner` bilgisayarındaki bu fermuarsız klasörü işaret edin.

### <a name="run-c-udo-test-cases-in-azure-pipelines"></a>Azure Ardışık Hatları'nda C# UDO test servis taleplerini çalıştırma

C# UDO testi için, UdO'lar için gerekli olan aşağıdaki derlemelere başvuruyaptığınızdan emin olun.

- Microsoft.Analytics.Interfaces
- Microsoft.Analytics.Types
- Microsoft.Analytics.UnitTest

[Bunları Nuget paketi Microsoft.Azure.DataLake.USQL.Interfaces](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.Interfaces/)aracılığıyla başvurursanız, yapı ardınıza bir NuGet Geri Yükleme görevi eklediğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Gölü Analitiği için CI/CD ardışık kurulumu nasıl ayarlanır?](data-lake-analytics-cicd-overview.md)
- [Yerel makinenizde U-SQL komut dosyası çalıştırma](data-lake-analytics-data-lake-tools-local-run.md)
- [U-SQL veritabanını geliştirmek için U-SQL veritabanı projesini kullanma](data-lake-analytics-data-lake-tools-develop-usql-database.md)
