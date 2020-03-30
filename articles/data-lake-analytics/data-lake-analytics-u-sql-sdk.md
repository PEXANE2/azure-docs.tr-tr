---
title: U-SQL işlerini yerel olarak çalıştırın - Azure Data Lake U-SQL SDK
description: Yerel iş istasyonunuzdaki komut satırı ve programlama arabirimlerini kullanarak U-SQL işlerini yerel olarak çalıştırmayı ve test etmeyi öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: yanacai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.topic: conceptual
ms.date: 03/01/2017
ms.openlocfilehash: 51d9060eaf4b30c696ef2a3b5f798a31e2f2a98a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71309690"
---
# <a name="run-and-test-u-sql-with-azure-data-lake-u-sql-sdk"></a>Azure Veri Gölü U-SQL SDK ile U-SQL'i çalıştırın ve test edin

U-SQL komut dosyası geliştirirken, buluta göndermeden önce U-SQL komut dosyasını yerel olarak çalıştırmak ve sınamak yaygındır. Azure Veri Gölü, bu senaryo için U-SQL çalıştır'ı kolayca ölçeklendirebileceğiniz ve test edilebilen Azure Data Lake U-SQL SDK adlı bir Nuget paketi sağlar. Derlemeyi ve testi otomatikleştirmek için bu U-SQL testini CI (Sürekli Tümleştirme) sistemiyle entegre etmek de mümkündür.

GUI aracıyla U-SQL komut dosyasını el ile yerel olarak çalıştırma ve hata ayıklama konusunda ilgileniyorsanız, bunun için Visual Studio için Azure Veri Göl Araçları'nı kullanabilirsiniz. [Buradan](data-lake-analytics-data-lake-tools-local-run.md)daha fazla bilgi edinebilirsiniz.

## <a name="install-azure-data-lake-u-sql-sdk"></a>Azure Veri Gölü U-SQL SDK'yı yükleyin

Azure Veri Gölü U-SQL SDK'yı [Nuget.org'da buradan](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) alabilirsiniz. Ve kullanmadan önce, aşağıdaki gibi bağımlılıkları olduğundan emin olmanız gerekir.

### <a name="dependencies"></a>Bağımlılıklar

Veri Gölü U-SQL SDK aşağıdaki bağımlılıkları gerektirir:

- [Microsoft .NET Framework 4.6 veya daha yeni.](https://www.microsoft.com/download/details.aspx?id=17851)
- Microsoft Visual C++ 14 ve Windows SDK 10.0.10240.0 veya daha yeni (bu makalede CppSDK olarak adlandırılır). CppSDK almanın iki yolu vardır:

  - [Visual Studio Community Edition'ı](https://developer.microsoft.com/downloads/vs-thankyou)yükleyin. Program Dosyaları klasörü altında bir \Windows Kitleri\10 klasörü olacak-örneğin, C:\Program Files (x86)\Windows Kitleri\10\. Windows 10 SDK sürümünü \Windows Kitleri\10\Lib altında da bulabilirsiniz. Bu klasörleri görmüyorsanız Visual Studio'yu yeniden yükleyin ve yükleme sırasında Windows 10 SDK'yı seçtiğinizden emin olun. Bunu Visual Studio yüklüyorsanız, U-SQL yerel derleyicisi bunu otomatik olarak bulur.

    ![Visual Studio yerel çalışan Windows 10 SDK için Veri Gölü Araçları](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)

  - [Visual Studio için Veri Gölü Araçları](https://aka.ms/adltoolsvs)yükleyin. Önceden paketlenmiş Visual C++ ve Windows SDK dosyalarını C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK adresinde bulabilirsiniz. Bu durumda, U-SQL yerel derleyicisi bağımlılıkları otomatik olarak bulamıyor. Bunun için CppSDK yolunu belirtmeniz gerekir. Dosyaları başka bir konuma kopyalayabilir veya olduğu gibi kullanabilirsiniz.

## <a name="understand-basic-concepts"></a>Temel kavramları anlama

### <a name="data-root"></a>Veri kökü

Veri kökü klasörü, yerel bilgi işlem hesabı için bir "yerel depo"dur. Bir Veri Gölü Analizi hesabının Azure Veri Gölü Deposu hesabına eşdeğerdir. Farklı bir veri kökü klasörüne geçmek, farklı bir mağaza hesabına geçmek gibidir. Sık paylaşılan verilere farklı veri kökü klasörleriyle erişmek istiyorsanız, komut dosyalarınızda mutlak yollar kullanmanız gerekir. Veya, paylaşılan verilere işaret etmek için veri kökü klasörü altında dosya sistemi sembolik bağlantılar (örneğin, NTFS **mklink)** oluşturun.

Veri kökü klasörü aşağıdakiler için kullanılır:

- Veritabanları, tablolar, tablo değerli işlevler (TVF'ler) ve derlemeler dahil olmak üzere yerel meta verileri depolayın.
- U-SQL'de göreli yollar olarak tanımlanan giriş ve çıkış yollarına bakın. Göreli yollar kullanmak, U-SQL projelerinizi Azure'a dağıtmanızı kolaylaştırır.

### <a name="file-path-in-u-sql"></a>U-SQL'de dosya yolu

U-SQL komut dosyalarında hem göreli bir yol hem de yerel mutlak yol kullanabilirsiniz. Göreli yol, belirtilen veri kökü klasör üyle göreli. Komut dosyalarınızı sunucu tarafıyla uyumlu hale getirmek için yol ayırıcısı olarak "/" kullanmanızı öneririz. Burada göreli yolların bazı örnekleri ve eşdeğer mutlak yolları verilmiştir. Bu örneklerde, C:\LocalRunDataRoot veri kökü klasörüdür.

|Göreli yol|Mutlak yol|
|-------------|-------------|
|/abc/def/input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|abc/def/input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/abc/def/input.csv |D:\abc\def\input.csv|

### <a name="working-directory"></a>Çalışma dizini

U-SQL komut dosyasını yerel olarak çalıştırırken, derleme sırasında geçerli çalışan dizini altında bir çalışma dizini oluşturulur. Derleme çıktılarına ek olarak, yerel yürütme için gereken çalışma zamanı dosyaları bu çalışma dizinine gölge kopyalanır. Çalışma dizini kök klasörü "ScopeWorkDir" olarak adlandırılır ve çalışma dizininin altındaki dosyalar aşağıdaki gibidir:

|Dizin/dosya|Dizin/dosya|Dizin/dosya|Tanım|Açıklama|
|--------------|--------------|--------------|----------|-----------|
|C6A101DDCB470506| | |Çalışma zamanı sürümünün karma dizesi|Yerel yürütme için gereken çalışma zamanı dosyalarının gölge kopyası|
| |Script_66AE4909AA0ED06C| |Komut dosyası adı + komut dosyası yolunun karma dizesi|Derleme çıktıları ve yürütme adımı günlüğe kaydetme|
| | |\_komut\_dosyası .abr|Derleyici çıktısı|Cebir dosyası|
| | |\_ScopeCodeGen\_.*|Derleyici çıktısı|Oluşturulan yönetilen kod|
| | |\_ScopeCodeGenEngine\_.*|Derleyici çıktısı|Oluşturulan yerel kod|
| | |başvurulan derlemeler|Montaj başvurusu|Başvurulan derleme dosyaları|
| | |deployed_resources|Kaynak dağıtımı|Kaynak dağıtım dosyaları|
| | |xxxxxxxx.xxx[1..n]\_\*.*|Yürütme günlüğü|Yürütme adımları günlüğü|


## <a name="use-the-sdk-from-the-command-line"></a>Komut satırından SDK'yı kullanma

### <a name="command-line-interface-of-the-helper-application"></a>Yardımcı uygulamanın komut satırı arabirimi

SDK dizininin altında\build\runtime, LocalRunHelper.exe yaygın olarak kullanılan yerel çalıştırış işlevlerinin çoğuna arabirim sağlayan komut satırı yardımcı uygulamasıdır. Hem komutun hem de bağımsız değişken anahtarlarının büyük/küçük harf duyarlı olduğunu unutmayın. Çağırmak için:

    LocalRunHelper.exe <command> <Required-Command-Arguments> [Optional-Command-Arguments]

LocalRunHelper.exe'yi bağımsız olarak veya yardım bilgilerini göstermek için **yardım** anahtarıyla çalıştırın:

    > LocalRunHelper.exe help

        Command 'help' :  Show usage information
        Command 'compile' :  Compile the script
        Required Arguments :
            -Script param
                    Script File Path
        Optional Arguments :
            -Shallow [default value 'False']
                    Shallow compile

Yardım bilgilerinde:

-  **Komut,** komutun adını verir.  
-  **Gerekli Bağımsız Değişken,** sağlanmalıdır bağımsız değişkenleri listeler.  
-  **İsteğe bağlı Bağımsız Değişken,** varsayılan değerlerle isteğe bağlı bağımsız değişkenleri listeler.  İsteğe bağlı Boolean bağımsız değişkenlerinin parametreleri yoktur ve görünümleri varsayılan değerleri için negatif anlamına gelir.

### <a name="return-value-and-logging"></a>İade değeri ve günlüğe kaydetme

Yardımcı uygulama başarı için **0** ve başarısızlık için **-1** döndürür. Varsayılan olarak, yardımcı tüm iletileri geçerli konsola gönderir. Ancak, komutların çoğu **-MessageOut path_to_log_file** çıktıları bir günlük dosyasına yönlendiren isteğe bağlı bağımsız değişkeni destekler.

### <a name="environment-variable-configuring"></a>Ortam değişkeni yapılandırma

U-SQL yerel çalıştırma, yerel depolama hesabı olarak belirli bir veri kökü ve bağımlılıklar için belirli bir CppSDK yolu gerekir. Bağımsız değişkeni komut satırında ayarlayabilir veya bunlar için ortam değişkenini ayarlayabilirsiniz.

- **SCOPE_CPP_SDK** ortamı değişkenini ayarlayın.

    Visual Studio için Veri Gölü Araçları'nı yükleyerek Microsoft Visual C++ ve Windows SDK'yı alırsanız, aşağıdaki klasöre sahip olduğunuzu doğrulayın:

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    Bu dizini işaret etmek için **SCOPE_CPP_SDK** adlı yeni bir ortam değişkeni tanımlayın. Veya klasörü diğer konuma kopyalayın ve **SCOPE_CPP_SDK** olarak belirtin.

    Ortam değişkenini ayarlamaya ek olarak, komut satırını kullanırken **-CppSDK** bağımsız değişkenini belirtebilirsiniz. Bu bağımsız değişken, varsayılan CppSDK ortam değişkeninizin üzerine yazar.

- **LOCALRUN_DATAROOT** ortamı değişkenini ayarlayın.

    Veri kökünü işaret eden **LOCALRUN_DATAROOT** adlı yeni bir ortam değişkeni tanımlayın.

    Ortam değişkenini ayarlamaya ek olarak, komut satırı kullanırken **-DataRoot** bağımsız değişkenini veri kökü yolu ile belirtebilirsiniz. Bu bağımsız değişken, varsayılan veri kökü ortamı değişkeninizin üzerine yazar. Tüm işlemler için varsayılan veri kökü ortamı değişkeninin üzerine yazabilmeniz için çalıştırdığınız her komut satırına bu bağımsız değişkeni eklemeniz gerekir.

### <a name="sdk-command-line-usage-samples"></a>SDK komut satırı kullanım örnekleri

#### <a name="compile-and-run"></a>Derle ve çalıştır

**Run** komutu komut dosyası derlemek ve sonra derlenmiş sonuçları yürütmek için kullanılır. Komut satırı bağımsız değişkenleri **derleme** ve **yürütmeden**gelenlerin bir birleşimidir.

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

Aşağıdaki **çalıştırmak**için isteğe bağlı bağımsız değişkenler şunlardır:


|Bağımsız Değişken|Varsayılan değer|Açıklama|
|--------|-------------|-----------|
|-CodeBehind|False|Komut dosyasının arkasında .cs kodu var|
|-CppSDK| |CppSDK Rehberi|
|-DataRoot| DataRoot çevre değişkeni|Yerel çalıştırma için DataRoot, varsayılan olarak 'LOCALRUN_DATAROOT' ortam değişkeni|
|-MessageOut| |İletileri konsola dosyaya atma|
|-Paralel|1|Planı belirtilen paralellikle çalıştırma|
|-Referanslar| |';' ile ayrılmış, arkasında ekstra başvuru derlemeleri veya kod veri dosyaları için yollar listesi|
|-UdoRedirect|False|Udo derlemesi yeniden yönlendirme config oluşturma|
|-Kullanım Veritabanı|ana|Geçici montaj kaydının arkasındaki kod için kullanılacak veritabanı|
|-Verbose|False|Çalışma zamanından ayrıntılı çıktıları göster|
|-WorkDir|Geçerli Dizin|Derleyici kullanımı ve çıktıları için dizin|
|-RunScopeCEP|0|Kullanım için ScopeCEP modu|
|-ScopeCEPTempPath|Temp|Veri akışı için kullanılacak geçici yol|
|-OptFlags| |En iyi duruma getirici bayraklarının virgülle ayrılmış listesi|


Bir örneği aşağıda verilmiştir:

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

**Derleme** ve **yürütmeyi**birleştirmenin yanı sıra, derlenen yürütücüleri ayrı ayrı derleyebilir ve yürütebilirsiniz.

#### <a name="compile-a-u-sql-script"></a>U-SQL komut dosyası derleme

**Derleme** komutu, bir U-SQL komut dosyasını yürütülebilir lere derlemek için kullanılır.

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

Derlemek **compile**için isteğe bağlı bağımsız değişkenler şunlardır:


|Bağımsız Değişken|Açıklama|
|--------|-----------|
| -CodeBehind [varsayılan değer 'False']|Komut dosyasının arkasında .cs kodu var|
| -CppSDK [varsayılan değer '']|CppSDK Rehberi|
| -DataRoot [varsayılan değer 'DataRoot çevre değişkeni']|Yerel çalıştırma için DataRoot, varsayılan olarak 'LOCALRUN_DATAROOT' ortam değişkeni|
| -MessageOut [varsayılan değer '']|İletileri konsola dosyaya atma|
| -Başvurular [varsayılan değer '']|';' ile ayrılmış, arkasında ekstra başvuru derlemeleri veya kod veri dosyaları için yollar listesi|
| -Sığ [varsayılan değer 'False']|Sığ derleme|
| -UdoRedirect [varsayılan değer 'False']|Udo derlemesi yeniden yönlendirme config oluşturma|
| -UseDatabase [varsayılan değer 'ana']|Geçici montaj kaydının arkasındaki kod için kullanılacak veritabanı|
| -WorkDir [varsayılan değer 'Geçerli Dizin']|Derleyici kullanımı ve çıktıları için dizin|
| -RunScopeCEP [varsayılan değer '0']|Kullanım için ScopeCEP modu|
| -ScopeCEPTempPath [varsayılan değer 'temp']|Veri akışı için kullanılacak geçici yol|
| -OptFlags [varsayılan değer '']|En iyi duruma getirici bayraklarının virgülle ayrılmış listesi|


Aşağıda bazı kullanım örnekleri verilmiştir.

U-SQL komut dosyası derleme:

    LocalRunHelper compile -Script d:\test\test1.usql

U-SQL komut dosyası derleyin ve veri kökü klasörünü ayarlayın. Bunun ayarlanan ortam değişkeninin üzerine yazacağını unutmayın.

    LocalRunHelper compile -Script d:\test\test1.usql –DataRoot c:\DataRoot

Bir U-SQL komut dosyası derleyin ve çalışan bir dizini, başvuru derlemesi ve veritabanı ayarlayın:

    LocalRunHelper compile -Script d:\test\test1.usql -WorkDir d:\test\bin -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB

#### <a name="execute-compiled-results"></a>Derlenmiş sonuçları yürütme

**Yürütme** komutu derlenmiş sonuçları yürütmek için kullanılır.   

    LocalRunHelper execute -Algebra path_to_compiled_algebra_file [optional_arguments]

Yürütmek **için**isteğe bağlı bağımsız değişkenler şunlardır:

|Bağımsız Değişken|Varsayılan değer|Açıklama|
|--------|-------------|-----------|
|-DataRoot | '' |Meta veri yürütme için veri kökü. **LOCALRUN_DATAROOT** ortamı değişkenine varsayılan olarak.|
|-MessageOut | '' |İletileri konsola bir dosyaya döküm.|
|-Paralel | '1' |Oluşturulan yerel çalıştırma adımlarını belirtilen paralellik düzeyiyle çalıştırmak için gösterge.|
|-Verbose | 'Yanlış' |Çalışma zamanından ayrıntılı çıktıları göstermek için gösterge.|

Aşağıda bir kullanım örneği verilmiştir:

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5


## <a name="use-the-sdk-with-programming-interfaces"></a>Programlama arayüzleri ile SDK'yı kullanma

Programlama arabirimleri tüm LocalRunHelper.exe bulunmaktadır. Bunları U-SQL SDK ve C# test çerçevesinin işlevselliğini tümleştirmek için kullanabilirsiniz. Bu makalede, U-SQL komut dosyanızı test etmek için bu arabirimleri nasıl kullanacağımı göstermek için standart C# birim test projesini kullanacağım.

### <a name="step-1-create-c-unit-test-project-and-configuration"></a>Adım 1: C# birim test projesi ve yapılandırmaoluşturma

- Dosya > Yeni > Projesi > Visual C# > Test > Ünite Test Projesi ile C# birim test projesi oluşturun.
- Proje için bir referans olarak LocalRunHelper.exe ekleyin. LocalRunHelper.exe Nuget paketinde \build\runtime\LocalRunHelper.exe bulunur.

    ![Azure Veri Gölü U-SQL SDK Referans Ekle](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-add-reference.png)

- U-SQL SDK **sadece** x64 ortamını destekler, yapı platformu hedefini x64 olarak belirlediğinizden emin olun. Bunu Project Property > Build > Platform hedefi üzerinden ayarlayabilirsiniz.

    ![Azure Veri Gölü U-SQL SDK Yapılandırma x64 Projesi](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-x64.png)

- Test ortamınızı x64 olarak ayarladıknızdan emin olun. Visual Studio'da, test > Test Ayarları > Varsayılan İşlemci Mimarisi > x64 üzerinden ayarlayabilirsiniz.

    ![Azure Veri Gölü U-SQL SDK Yapılandırma x64 Test Ortamı](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-test-x64.png)

- NugetPackage\build\runtime\ altındaki tüm bağımlılık dosyalarını, genellikle ProjectFolder\bin\x64\Debug altında olan çalışma dizinini yansıtmak için kopyaladığından emin olun.

### <a name="step-2-create-u-sql-script-test-case"></a>Adım 2: U-SQL komut dosyası test çalışması oluşturma

Aşağıda U-SQL komut dosyası testi için örnek kodu verilmiştir. Sınama için komut dosyaları, giriş dosyaları ve beklenen çıktı dosyaları hazırlamanız gerekir.

    using System;
    using Microsoft.VisualStudio.TestTools.UnitTesting;
    using System.IO;
    using System.Text;
    using System.Security.Cryptography;
    using Microsoft.Analytics.LocalRun;

    namespace UnitTestProject1
    {
        [TestClass]
        public class USQLUnitTest
        {
            [TestMethod]
            public void TestUSQLScript()
            {
                //Specify the local run message output path
                StreamWriter MessageOutput = new StreamWriter("../../../log.txt");

                LocalRunHelper localrun = new LocalRunHelper(MessageOutput);

                //Configure the DateRoot path, Script Path and CPPSDK path
                localrun.DataRoot = "../../../";
                localrun.ScriptPath = "../../../Script/Script.usql";
                localrun.CppSdkDir = "../../../CppSDK";

                //Run U-SQL script
                localrun.DoRun();

                //Script output 
                string Result = Path.Combine(localrun.DataRoot, "Output/result.csv");

                //Expected script output
                string ExpectedResult = "../../../ExpectedOutput/result.csv";

                Test.Helpers.FileAssert.AreEqual(Result, ExpectedResult);

                //Don't forget to close MessageOutput to get logs into file
                MessageOutput.Close();
            }
        }
    }

    namespace Test.Helpers
    {
        public static class FileAssert
        {
            static string GetFileHash(string filename)
            {
                Assert.IsTrue(File.Exists(filename));

                using (var hash = new SHA1Managed())
                {
                    var clearBytes = File.ReadAllBytes(filename);
                    var hashedBytes = hash.ComputeHash(clearBytes);
                    return ConvertBytesToHex(hashedBytes);
                }
            }

            static string ConvertBytesToHex(byte[] bytes)
            {
                var sb = new StringBuilder();

                for (var i = 0; i < bytes.Length; i++)
                {
                    sb.Append(bytes[i].ToString("x"));
                }
                return sb.ToString();
            }

            public static void AreEqual(string filename1, string filename2)
            {
                string hash1 = GetFileHash(filename1);
                string hash2 = GetFileHash(filename2);

                Assert.AreEqual(hash1, hash2);
            }
        }
    }


### <a name="programming-interfaces-in-localrunhelperexe"></a>LocalRunHelper.exe'de programlama arayüzleri

LocalRunHelper.exe U-SQL yerel derlemek, çalıştırmak, vb programlama arabirimleri sağlar. Arabirimler aşağıdaki gibi listelenir.

**Oluşturucu**

public LocalRunHelper([System.IO.TextWriter messageOutput = null])

|Parametre|Tür|Açıklama|
|---------|----|-----------|
|mesajÇıktı|System.IO.TextWriter|çıkış iletileri için, Console kullanmak için null olarak ayarlanmış|

**Özellikler**

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|Cebir Yolu|string|Cebir dosyasına giden yol (cebir dosyası derleme sonuçlarından biridir)|
|CodeBehindReferences|string|Komut dosyasında başvuruların arkasında ek kod varsa, ';' ile ayrılmış yolları belirtin|
|CppSdkDir|string|CppSDK dizini|
|AkımDir|string|Geçerli dizin|
|DataRoot|string|Veri kök yolu|
|DebuggerMailPath|string|Hata ayıklama posta yuvasına giden yol|
|GenerateUdoRedirect|bool|Derleme yükleme yeniden yönlendirme oluşturmak istiyorsanız config geçersiz|
|HasCodeBehind|bool|Komut dosyasının arkasında kod varsa|
|GirişDir|string|Giriş verileri için dizin|
|MessagePath|string|İleti dökümü dosya yolu|
|ÇıktılarDir|string|Çıktı verileri için dizin|
|Parallelism|int|Paralellik cebir çalıştırmak için|
|ParentPid|int|Hizmetin çıkmak için izlediği üst öğenin PID'si, yoksaymak için 0 olarak ayarlanmış veya negatif|
|Resultpath|string|Sonuç dökümü dosya yolu|
|Çalışma ZamanıDir|string|Çalışma zamanı dizini|
|Komut Dosyası Yolu|string|Komut dosyası nerede bulabilirim|
|Sığ|bool|Sığ derleme veya değil|
|Rıdda|string|Geçici dizin|
|DataBase'i Kullanma|string|Geçici montaj kaydının arkasındaki kod için kullanılacak veritabanını belirtin, varsayılan olarak ana|
|WorkDir|string|Tercih edilen çalışma dizini|


**Yöntem**

|Yöntem|Açıklama|Dönüş|Parametre|
|------|-----------|------|---------|
|kamu bool DoCompile()|U-SQL komut dosyasını derleme|Başarı konusunda doğru| |
|kamu bool DoExec()|Derlenen sonucu çalıştırma|Başarı konusunda doğru| |
|kamu bool DoRun()|U-SQL komut dosyasını çalıştırın (Derleme + Yürütme)|Başarı konusunda doğru| |
|kamu bool IsValidRuntimeDir(dize yolu)|Verilen yolun geçerli çalışma zamanı yolu olup olmadığını denetleme|Geçerli için doğru|Çalışma zamanı dizininin yolu|


## <a name="faq-about-common-issue"></a>Ortak sorun hakkında SSS

### <a name="error-1"></a>Hata 1:
E_CSC_SYSTEM_INTERNAL: İç hata! 'ScopeEngineManaged.dll' veya bağımlılıklarından biri dosya veya montaj yükleyemedi. Belirtilen modül bulunamadı.

Lütfen aşağıdakileri kontrol edin:

- X64 ortamına sahip olduğundan emin olun. Yapı hedef platformu ve test ortamı x64 olmalıdır, **Adım 1'e bakın: C# birimi test projesi oluşturma ve** yukarıdaki yapılandırma.
- NugetPackage\build\runtime\ altındaki tüm bağımlılık dosyalarını proje çalışma dizinine kopyaladığınızdan emin olun.


## <a name="next-steps"></a>Sonraki adımlar

* U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).
* Tanılama bilgilerini günlüğe kaydetmek için Bkz. [Azure Veri Gölü Analizi için tanılama günlüklerine erişme.](data-lake-analytics-diagnostic-logs.md)
* Daha karmaşık bir sorgu görmek için Azure [Veri Gölü Analizi'ni kullanarak web sitesi günlüklerini analiz etme](data-lake-analytics-analyze-weblogs.md)bölümüne bakın.
* İş ayrıntılarını görüntülemek [için Azure Veri Gölü Analizi işleri için İş Tarayıcısı ve İş Görünümü'nü kullanın'a](data-lake-analytics-data-lake-tools-view-jobs.md)bakın.
* Vertex yürütme görünümünü kullanmak [için](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)bkz.
