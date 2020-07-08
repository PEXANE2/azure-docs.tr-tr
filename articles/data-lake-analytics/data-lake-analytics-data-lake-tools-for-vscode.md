---
title: Visual Studio Code için Azure Data Lake Araçları’nı kullanma
description: U-SQL betiklerini oluşturmak, test etmek ve çalıştırmak için Visual Studio için Azure Data Lake Araçları kodu kullanmayı öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: Jejiang
ms.author: jejiang
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.topic: conceptual
ms.date: 02/09/2018
ms.openlocfilehash: dd3db27160f8fcc0ad3135b0ab87921a636fd155
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85565123"
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Visual Studio Code için Azure Data Lake Araçları’nı kullanma

Bu makalede, U-SQL betiklerini oluşturmak, test etmek ve çalıştırmak için Visual Studio için Azure Data Lake Araçları kodu (VS Code) nasıl kullanabileceğinizi öğrenin. Bilgiler aşağıdaki videoda da ele alınmıştır:

[![Video oynatıcı: VS Code için Azure Data Lake araçları](media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png)](https://channel9.msdn.com/Series/AzureDataLake/Azure-Data-Lake-Tools-for-VSCode?term=ADL%20Tools%20for%20VSCode")

## <a name="prerequisites"></a>Ön koşullar

VS Code için Azure Data Lake araçları, Windows, Linux ve macOS 'u destekler.U-SQL yerel çalıştırma ve yerel hata ayıklama yalnızca Windows 'da çalışır.

- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx)

MacOS ve Linux için:

- [.NET Core SDK 2,0](https://www.microsoft.com/net/download/core)
- [Mono 5.2. x](https://www.mono-project.com/download/)

## <a name="install-azure-data-lake-tools"></a>Azure Data Lake araçları 'nı yükler

Önkoşulları yükledikten sonra, VS Code için Azure Data Lake Araçları yükleyebilirsiniz.

### <a name="to-install-azure-data-lake-tools"></a>Azure Data Lake araçları 'nı yüklemek için

1. Visual Studio Code'u açın.
2. Sol bölmedeki **Uzantılar** ' ı seçin. Arama kutusuna **Azure Data Lake araçları** girin.
3. **Azure Data Lake araçları**' nın yanındaki **yüklemeyi** seçin.

   ![Data Lake araçlarını yüklemeye yönelik seçimler](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

   Birkaç saniye sonra Yükle düğmesi yeniden **yüklenmek** üzere değişir. **Reload**
4. **Azure Data Lake araçları** uzantısını etkinleştirmek Için **yeniden yükle** ' yi seçin.
5. Onaylamak için **pencereyi yeniden yükle** ' yi seçin. **Uzantılar** bölmesinde **Azure Data Lake araçları** ' na bakabilirsiniz.

## <a name="activate-azure-data-lake-tools"></a>Azure Data Lake araçlarını etkinleştir

Uzantıyı etkinleştirmek için. usql dosyası oluşturun veya var olan bir. usql dosyasını açın.

## <a name="work-with-u-sql"></a>U-SQL ile çalışma

U-SQL ile çalışmak için, bir U-SQL dosyası ya da bir klasör açmanız gerekir.

### <a name="to-open-the-sample-script"></a>Örnek betiği açmak için

Komut paletini açın (Ctrl + Shift + P) ve **adl: açık örnek betiği**girin. Bu örnek, bu örneğin başka bir örneğini açar. Ayrıca, bu örnekte bir betiği düzenleyebilir, yapılandırabilir ve gönderebilirsiniz.

### <a name="to-open-a-folder-for-your-u-sql-project"></a>U-SQL projenizin klasörünü açmak için

1. Visual Studio Code, **Dosya** menüsünü seçin ve sonra **klasörü aç**' ı seçin.
2. Bir klasör belirtin ve ardından **Klasör Seç**' i seçin.
3. **Dosya** menüsünü seçin ve ardından **Yeni**' yi seçin. Bir adsız-1 dosyası projeye eklenir.
4. Aşağıdaki kodu başlıksız-1 dosyasına girin:

   ```usql
   @departments  =
       SELECT * FROM
           (VALUES
               (31,    "Sales"),
               (33,    "Engineering"),
               (34,    "Clerical"),
               (35,    "Marketing")
           ) AS
                 D( DepID, DepName );
   ```

   @departments      Outputters.Csv () kullanarak "/output/departments.csv" öğesine çıkış yapın

    Betik,/Output klasörüne eklenen bazı verilerle bir departments.csv dosyası oluşturur.

5. Dosyayı açılan klasöre **Myusql. usql** olarak kaydedin.

### <a name="to-compile-a-u-sql-script"></a>U-SQL betiğini derlemek için

1. Komut paletini açmak için CTRL + SHIFT + P ' yi seçin.
2. **Adl: derleme betiği**girin. Derleme sonuçları **Çıkış** penceresinde görüntülenir. Ayrıca bir betik dosyasına sağ tıklayıp **adl: betiği derle** ' yi seçerek U-SQL işini derleyebilirsiniz. Derleme sonucu **çıktı** bölmesinde görünür.

### <a name="to-submit-a-u-sql-script"></a>U-SQL betiği göndermek için

1. Komut paletini açmak için CTRL + SHIFT + P ' yi seçin.
2. **Adl: gönderme işini**girin. Ayrıca bir betik dosyasına sağ tıklayıp **adl: Işi gönder**' i seçebilirsiniz.

Bir U-SQL işi gönderdikten sonra, gönderim günlükleri VS Code **Çıkış** penceresinde görüntülenir. İş görünümü sağ bölmede görüntülenir. Gönderim başarılı olursa, iş URL 'SI de görüntülenir. Gerçek zamanlı iş durumunu izlemek için iş URL 'sini bir Web tarayıcısında açabilirsiniz.

İş görünümünün **Özet** sekmesinde iş ayrıntılarını görebilirsiniz. Ana işlevler, bir betiği yeniden gönder, bir betiği Yinele ve portalda aç ' ı içerir. İş görünümünün **veri** sekmesinde giriş dosyalarına, çıkış dosyalarına ve kaynak dosyalarına başvurabilirsiniz. Dosyalar yerel bilgisayara indirilebilir.

![İş görünümündeki Özet sekmesi](./media/data-lake-analytics-data-lake-tools-for-vscode/job-view-summary.png)

![İş görünümündeki veri sekmesi](./media/data-lake-analytics-data-lake-tools-for-vscode/job-view-data.png)

### <a name="to-set-the-default-context"></a>Varsayılan bağlamı ayarlamak için

Dosya parametrelerini ayrı ayrı ayarlamadıysanız, bu ayarı tüm betik dosyalarına uygulamak için varsayılan bağlamı ayarlayabilirsiniz.

1. Komut paletini açmak için CTRL + SHIFT + P ' yi seçin.
2. **Adl: varsayılan bağlamı ayarla**' yı girin. Veya betik düzenleyicisine sağ tıklayıp **adl: varsayılan bağlamı ayarla**' yı seçin.
3. İstediğiniz hesabı, veritabanını ve şemayı seçin. Ayar, yapılandırma dosyasında xxx_settings.jskaydedilir.

   ![Hesap, veritabanı ve şema varsayılan bağlam olarak ayarlandı](./media/data-lake-analytics-data-lake-tools-for-vscode/default-context-sequence.png)

### <a name="to-set-script-parameters"></a>Betik parametrelerini ayarlamak için

1. Komut paletini açmak için CTRL + SHIFT + P ' yi seçin.
2. **Adl: betik parametrelerini ayarla**' yı girin.
3. Dosyadaki xxx_settings.jsaşağıdaki özelliklerle açılır:

   - **Hesap**: U-SQL işlerini derlemek ve çalıştırmak Için gereken Azure aboneliğiniz kapsamındaki bir Azure Data Lake Analytics hesabı. U-SQL işlerini derleyip çalıştırmadan önce bilgisayar hesabını yapılandırmanız gerekir.
   - **veritabanı**: hesabınızdaki bir veritabanı. Varsayılan değer **yöneticisidir**.
   - **şema**: veritabanınız altındaki bir şema. Varsayılan değer **dbo**' dır.
   - **Optionalsettings**:
        - **Öncelik**: öncelik aralığı 1 ile 1000 arasındadır ve en yüksek öncelik 1 ' dir. Varsayılan değer **1000**'dir.
        - **Degreeofparalellik**: paralellik aralığı 1 ile 150 arasındadır. Varsayılan değer Azure Data Lake Analytics hesabınızda izin verilen en büyük paralellik değeridir.

   ![JSON dosyasının içeriği](./media/data-lake-analytics-data-lake-tools-for-vscode/default-context-setting.png)

> [!NOTE]
> Yapılandırmayı kaydettikten sonra, varsayılan bir bağlam ayarlanmamışsa, ilgili. usql dosyasının sol alt köşesindeki hesap, veritabanı ve şema bilgileri görüntülenir.

### <a name="to-set-git-ignore"></a>Git Ignore 'yi ayarlamak için

1. Komut paletini açmak için CTRL + SHIFT + P ' yi seçin.
2. **Adl: git Ignore ayarla**' yı girin.

   - VS Code çalışma klasörünüzde bir **. gitIgnore** dosyanız yoksa, klasörünüzde **. gitIgnore** adlı bir dosya oluşturulur. Dört öğe (**usqlCodeBehindReference**, **usqlCodeBehindGenerated**, **. Cache**, **obj**) varsayılan olarak dosyaya eklenir. Gerekirse daha fazla güncelleştirme yapabilirsiniz.
   - VS Code çalışma klasörünüzde zaten bir **. gitIgnore** dosyanız varsa, bu araç, dosyada dört öğe dahil edilmediğinden **. gitIgnore** dosyanıza dört öğe (**usqlCodeBehindReference**, **usqlCodeBehindGenerated**, **. Cache**, **obj**) ekler.

   ![. GitIgnore dosyasındaki öğeler](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-gitignore.png)

## <a name="work-with-code-behind-files-c-sharp-python-and-r"></a>Arka plan kod dosyalarıyla çalışma: C diyez, Python ve R

Azure Data Lake araçları birden çok özel kodu destekler. Yönergeler için, bkz. [VS Code Azure Data Lake Analytics Için Python, R ve C Sharp Ile U-SQL geliştirme](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md).

## <a name="work-with-assemblies"></a>Derlemelerle çalışma

Derleme geliştirme hakkında bilgi için bkz. [Azure Data Lake Analytics işleri Için U-SQL derlemelerini geliştirme](data-lake-analytics-u-sql-develop-assemblies.md).

Data Lake Analytics kataloğuna özel kod derlemelerini kaydetmek için Data Lake araçlarını kullanabilirsiniz.

### <a name="to-register-an-assembly"></a>Bir derlemeyi kaydetmek için

Derlemeyi **adl: Register Assembly** veya **adl: Register Assembly (Gelişmiş)** komutu aracılığıyla kaydedebilirsiniz.

### <a name="to-register-through-the-adl-register-assembly-command"></a>ADL: Register Assembly komutunu kullanarak kaydolmak için

1. Komut paletini açmak için CTRL + SHIFT + P ' yi seçin.
2. **Adl: Register derlemesini**girin.
3. Yerel derleme yolunu belirtin.
4. Bir Data Lake Analytics hesabı seçin.
5. Bir veritabanı seçin.

Portal bir tarayıcıda açılır ve derleme kayıt işlemini görüntüler.  

**Adl: Register Assembly** komutunu tetiklemenin daha kolay bir yolu, dosya Gezgini 'nde. dll dosyasına sağ tıklamanız.

### <a name="to-register-through-the-adl-register-assembly-advanced-command"></a>ADL: yazmaç derlemesi (Gelişmiş) komutu üzerinden kaydolmak için

1. Komut paletini açmak için CTRL + SHIFT + P ' yi seçin.
2. **Adl: yazmaç derlemesini (Gelişmiş)** girin.
3. Yerel derleme yolunu belirtin.
4. JSON dosyası görüntülenir. Gerekirse, derleme bağımlılıklarını ve kaynak parametrelerini gözden geçirin ve düzenleyin. Yönergeler **Çıkış** penceresinde görüntülenir. Derleme kaydına devam etmek için JSON dosyasını kaydedin (CTRL + S).

   ![Bütünleştirilmiş kod bağımlılıkları ve kaynak parametreleri içeren JSON dosyası](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)

>[!NOTE]
>
>- Azure Data Lake araçları, DLL 'nin herhangi bir derleme bağımlılığı olup olmadığını algılar. Bağımlılıklar, algılandıktan sonra JSON dosyasında görüntülenir.
>- DLL kaynaklarınızı (örneğin,. txt,. png ve. csv) derleme kaydının bir parçası olarak yükleyebilirsiniz.

**Adl: yazmaç derlemesi (Gelişmiş)** komutunu tetiklemenin bir diğer yolu, dosya Gezgini 'nde. dll dosyasına sağ tıklamanız.

Aşağıdaki U-SQL kodu, bir derlemenin nasıl çağrılacağını gösterir. Örnekte, derleme adı *Test*olur.

```usql
REFERENCE ASSEMBLY [test];
@a =
    EXTRACT
        Iid int,
    Starts DateTime,
    Region string,
    Query string,
    DwellTime int,
    Results string,
    ClickedUrls string
    FROM @"Sample/SearchLog.txt"
    USING Extractors.Tsv();
@d =
    SELECT DISTINCT Region
    FROM @a;
@d1 =
    PROCESS @d
    PRODUCE
        Region string,
    Mkt string
    USING new USQLApplication_codebehind.MyProcessor();
OUTPUT @d1
    TO @"Sample/SearchLogtest.txt"
    USING Outputters.Tsv();
```

## <a name="use-u-sql-local-run-and-local-debug-for-windows-users"></a>Windows kullanıcıları için U-SQL yerel çalıştırma ve yerel hata ayıklama kullanın

U-SQL yerel çalıştırma, kodunuzun Data Lake Analytics yayınlanmasından önce yerel verilerinizi sınar ve betiğinizi yerel olarak doğrular. Kodunuzun Data Lake Analytics gönderilmeden önce aşağıdaki görevleri gerçekleştirmek için yerel hata ayıklama özelliğini kullanabilirsiniz:

- C# arka plan kodunuzda hata ayıklayın.
- Kodda adım adım ilerleyin.
- Betiğinizi yerel olarak doğrulayın.

Yerel çalıştırma ve yerel hata ayıklama özelliği yalnızca Windows ortamlarında çalışır ve macOS ve Linux tabanlı işletim sistemlerinde desteklenmez.

Yerel çalıştırma ve yerel hata ayıklama yönergeleri için, bkz. [U-SQL yerel çalıştırma ve Visual Studio Code ile yerel hata ayıklama](data-lake-tools-for-vscode-local-run-and-debug.md).

## <a name="connect-to-azure"></a>Azure'a Bağlanma

Data Lake Analytics 'de U-SQL betiklerini derlemek ve çalıştırmak için önce Azure hesabınıza bağlanmanız gerekir.

<a id="sign-in-by-command"></a>

### <a name="to-connect-to-azure-by-using-a-command"></a>Bir komut kullanarak Azure 'a bağlanmak için

1. Komut paletini açmak için CTRL + SHIFT + P ' yi seçin.

2. **Adl: Login**girin. Oturum açma bilgileri sağ alt tarafta görüntülenir.

   ![Login komutunu girme](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)

   ![Oturum açma ve kimlik doğrulama hakkında bildirim](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)

3. [Oturum açma Web sayfasını](https://aka.ms/devicelogin)açmak için **Kopyala & aç** ' ı seçin. Kodu kutuya yapıştırın ve sonra **devam**' ı seçin.

    ![Oturum açma Web sayfası](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png)  

4. Web sayfasından oturum açmak için yönergeleri izleyin. Bağlandığınızda, Azure hesabınızın adı VS Code penceresinin sol alt köşesindeki durum çubuğunda görüntülenir.

> [!NOTE]
>
> - Data Lake araçlar, oturumunuzu otomatik olarak bir sonraki sefer kapatır.
> - Hesabınızda iki faktör etkinse, PIN kullanmak yerine telefon kimlik doğrulaması kullanmanızı öneririz.

Oturumu kapatmak için **adl: logout**komutunu girin.

### <a name="to-connect-to-azure-from-the-explorer"></a>Azure 'a gezgin 'e bağlanmak için

Azure **Data Lake**' i genişletin, **Azure 'da oturum aç**' ı seçin ve ardından [bir komut kullanarak Azure 'a bağlanmak için](#sign-in-by-command)adım 3 ve adım 4 ' ü izleyin.

![Gezgin 'de "Azure 'da oturum aç" seçimi](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-sign-in-from-explorer.png )  

Gezgin oturumunuzu açamazsınız. Oturumu kapatmak için, bkz. [bir komut kullanarak Azure 'a bağlanmak için](#sign-in-by-command).

## <a name="create-an-extraction-script"></a>Ayıklama betiği oluşturma

**Adl: Create Ayıkla betiğini** veya Azure Data Lake Gezgini ' ni kullanarak. csv,. tsv ve. txt dosyaları için bir ayıklama betiği oluşturabilirsiniz.

### <a name="to-create-an-extraction-script-by-using-a-command"></a>Bir komut kullanarak ayıklama betiği oluşturmak için

1. Komut paletini açmak için CTRL + SHIFT + P ' yi seçin ve **adl: ayıklama betiği oluştur**yazın.
2. Bir Azure depolama dosyası için tam yolu belirtin ve Enter tuşunu seçin.
3. Bir hesap seçin.
4. Bir. txt dosyası için, dosyayı ayıklamak üzere bir sınırlayıcı seçin.

![Ayıklama betiği oluşturma işlemi](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-process.png)

Ayıklama betiği, girdlarınız temel alınarak oluşturulur. Sütunları algılayamadığı bir betik için, iki seçenekten birini seçin. Aksi takdirde, yalnızca bir betik oluşturulur.

![Ayıklama betiği oluşturma sonucu](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-result.png)

### <a name="to-create-an-extraction-script-from-the-explorer"></a>Gezgin 'den bir ayıklama betiği oluşturmak için

Ayıklama betiğini oluşturmanın bir başka yolu da Azure Data Lake Store veya Azure Blob depolama alanındaki. csv,. tsv veya. txt dosyasındaki sağ tıklama (kısayol) menüsünü kullanmaktır.

![Kısayol menüsünden "betiği AYıKLA" komutu](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-from-context-menu.png)

## <a name="integrate-with-azure-data-lake-analytics-through-a-command"></a>Komut ile Azure Data Lake Analytics tümleştirme

Hesapları listelemek, meta verilere erişmek ve analiz işlerini görüntülemek için Azure Data Lake Analytics kaynaklarına erişebilirsiniz.

### <a name="to-list-the-azure-data-lake-analytics-accounts-under-your-azure-subscription"></a>Azure aboneliğinizin altındaki Azure Data Lake Analytics hesaplarını listelemek için

1. Komut paletini açmak için CTRL + SHIFT + P ' yi seçin.
2. **Adl: List hesaplarını**girin. Hesaplar **Çıkış** bölmesinde görünür.

### <a name="to-access-azure-data-lake-analytics-metadata"></a>Azure Data Lake Analytics meta verilerine erişmek için

1. CTRL + SHIFT + P ' yi seçin ve ardından **adl: List tablolarını**girin.
2. Data Lake Analytics hesaplarından birini seçin.
3. Data Lake Analytics veritabanlarından birini seçin.
4. Şemalardan birini seçin. Tablo listesini görebilirsiniz.

### <a name="to-view-azure-data-lake-analytics-jobs"></a>Azure Data Lake Analytics işleri görüntülemek için

1. Komut paletini açın (Ctrl + Shift + P) ve **adl: Işleri göster**' i seçin.
2. Data Lake Analytics veya yerel hesap seçin.
3. Hesap için iş listesinin görünmesini bekleyin.
4. İş listesinden bir iş seçin. Data Lake araçlar, sağ bölmedeki iş görünümünü açar ve VS Code çıkışında bazı bilgileri görüntüler.

    ![İş listesi](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="integrate-with-azure-data-lake-store-through-a-command"></a>Komut ile Azure Data Lake Store tümleştirme

Azure Data Lake Store ilgili komutları kullanarak şunları yapabilirsiniz:

- [Azure Data Lake Store kaynaklarına göz at](#list-the-storage-path)
- [Azure Data Lake Store dosyasını önizleyin](#preview-the-storage-file)
- Dosyayı doğrudan VS Code Azure Data Lake Store yükleme
- Dosyayı doğrudan VS Code Azure Data Lake Store 'tan indirin

### <a name="list-the-storage-path"></a>Depolama yolunu listeleme

### <a name="to-list-the-storage-path-through-the-command-palette"></a>Depolama yolunu komut paleti aracılığıyla listelemek için

1. Betik düzenleyicisine sağ tıklayın ve **adl: liste yolu**' nu seçin.
2. Listeden klasörü seçin veya **bir yol girin** ' i veya **kök yolundan gözatıp**' yi seçin. (Örnek olarak **bir yol girin** .)
3. Data Lake Analytics hesabınızı seçin.
4. Depolama klasörü yoluna gidin veya (örneğin,/output/) girin.  

Komut paleti, girdinize bağlı olarak yol bilgilerini listeler.

![Depolama yolu sonuçları](./media/data-lake-analytics-data-lake-tools-for-vscode/list-storage-path.png)

Göreli yolu listelemek için daha uygun bir yol, kısayol menüsü kullanmaktır.

### <a name="to-list-the-storage-path-through-the-shortcut-menu"></a>Kısayol menüsü aracılığıyla depolama yolunu listelemek için

Yol dizesine sağ tıklayın ve **liste yolu**' nu seçin.

![Kısayol menüsünde "liste yolu"](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)

### <a name="preview-the-storage-file"></a>Depolama dosyasının önizlemesini görüntüleyin

1. Betik düzenleyicisine sağ tıklayın ve **adl: Preview dosyası**' nı seçin.
2. Data Lake Analytics hesabınızı seçin.
3. Bir Azure depolama dosya yolu girin (örneğin,/Output/SearchLog.txt).

Dosya VS Code açılır.

![Adımlar ve depolama dosyasının önizlenmesi sonucu](./media/data-lake-analytics-data-lake-tools-for-vscode/preview-storage-file.png)

Dosyayı önizlemenin bir başka yolu da dosyanın tam yolundaki kısayol menüsü veya dosya Düzenleyicisi 'ndeki dosyanın göreli yolu ile yapılır.

### <a name="upload-a-file-or-folder"></a>Dosya veya klasörü karşıya yükleme

1. Betik düzenleyicisine sağ tıklayıp **dosyayı karşıya yükle** veya **klasörü karşıya yükle**' yi seçin.
2. **Karşıya dosya yükle**' yi seçtiyseniz bir dosya veya birden çok dosya seçin veya **klasörü karşıya yükle**' yi seçtiyseniz tüm klasörü seçin. Ardından **Karşıya Yükle**’yi seçin.
3. Listeden depolama klasörünü seçin veya **bir yol girin** veya **kök yolundan gözatıp**' yi seçin. (Örnek olarak **bir yol girin** .)
4. Data Lake Analytics hesabınızı seçin.
5. Depolama klasörü yoluna gidin veya (örneğin,/output/) girin.
6. Karşıya yükleme hedefini belirtmek için **geçerli klasörü seç** ' i seçin.

![Dosya veya klasör yükleme adımları ve sonuçları](./media/data-lake-analytics-data-lake-tools-for-vscode/upload-file.png)

Dosyaları depolama alanına yüklemenin başka bir yolu da dosyanın tam yolundaki kısayol menüsünün veya dosya Düzenleyicisi 'ndeki dosyanın göreli yolunda yer alır.

[Karşıya yükleme durumunu izleyebilirsiniz](#check-storage-tasks-status).

### <a name="download-a-file"></a>Dosya indirme

**Adl: Download File** veya **adl: Download FILE (Gelişmiş)** komutunu kullanarak bir dosyayı indirebilirsiniz.

### <a name="to-download-a-file-through-the-adl-download-file-advanced-command"></a>ADL: Download File (Gelişmiş) komutuyla bir dosyayı indirmek için

1. Betik düzenleyicisine sağ tıklayıp **dosya indir (Gelişmiş)** öğesini seçin.
2. VS Code bir JSON dosyası görüntüler. Dosya yollarını girebilir ve aynı anda birden çok dosyayı indirebilirsiniz. Yönergeler **Çıkış** penceresinde görüntülenir. Dosya veya dosyaları karşıdan yüklemeye devam etmek için JSON dosyasını kaydedin (CTRL + S).

    ![Dosya indirme yollarındaki JSON dosyası](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-files.png)

**Çıkış** penceresinde, dosya indirme durumu görüntülenir.

![İndirme durumundaki çıkış penceresi](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-file-result.png)

[İndirme durumunu izleyebilirsiniz](#check-storage-tasks-status).

### <a name="to-download-a-file-through-the-adl-download-file-command"></a>ADL: Download File komutuyla bir dosyayı indirmek için

1. Betik Düzenleyicisi ' ne sağ tıklayın, **dosya indir**' i seçin ve ardından **Klasör Seç** iletişim kutusunda hedef klasörü seçin.

1. Listeden klasörü seçin veya **bir yol girin** ' i veya **kök yolundan gözatıp**' yi seçin. (Örnek olarak **bir yol girin** .)

1. Data Lake Analytics hesabınızı seçin.

1. Depolama klasörü yoluna gidin veya girin (örneğin,/output/) ve ardından İndirilecek dosyayı seçin.

![Adımlar ve dosya indirme sonucu](./media/data-lake-analytics-data-lake-tools-for-vscode/download-file.png)

Depolama dosyalarını karşıdan yüklemenin başka bir yolu da dosyanın tam yolundaki kısayol menüsünün veya dosya Düzenleyicisi 'ndeki dosyanın göreli yolunda yer alır.

[İndirme durumunu izleyebilirsiniz](#check-storage-tasks-status).

### <a name="check-storage-tasks-status"></a>Depolama görevlerinin durumunu denetle

Karşıya yükleme ve indirme durumu durum çubuğunda görüntülenir. Durum çubuğunu seçin ve ardından **Çıkış** sekmesinde durum görüntülenir.

![Durum çubuğu ve çıkış ayrıntıları](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-status.png)

## <a name="integrate-with-azure-data-lake-analytics-from-the-explorer"></a>Gezgin Azure Data Lake Analytics ile tümleştirme

Oturum açtıktan sonra Azure hesabınız için tüm abonelikler, **Azure Data Lake**altında sol bölmede listelenir.

![Data Lake Gezgini](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer.png)

### <a name="data-lake-analytics-metadata-navigation"></a>Data Lake Analytics meta veri gezintisi

Azure aboneliğinizi genişletin. **U-SQL veritabanları** düğümü altında, u-SQL veritabanınıza göz atabilir ve **şemalar**, **kimlik bilgileri**, **derlemeler**, **Tablolar**ve **Dizin**gibi klasörleri görüntüleyebilirsiniz.

### <a name="data-lake-analytics-metadata-entity-management"></a>Data Lake Analytics meta veri varlık yönetimi

**U-SQL veritabanlarını**genişletin. Karşılık gelen düğüme sağ tıklayıp ardından kısayol menüsünde **oluşturulacak betiği** seçerek bir veritabanı, şema, tablo, tablo türü, dizin veya istatistik oluşturabilirsiniz. Açılan komut dosyası sayfasında, komut dosyasını gereksinimlerinize göre düzenleyin. Ardından, sağ tıklayıp **adl: gönder işini**seçerek işi gönderebilirsiniz.

Öğeyi oluşturmayı tamamladıktan sonra, düğümüne sağ tıklayın ve ardından öğeyi göstermek için **Yenile** ' yi seçin. Öğeyi sağ tıklayıp **Sil**' i seçerek de silebilirsiniz.

![Data Lake Gezgini 'ndeki kısayol menüsünde "oluşturulacak komut dosyası" komutu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create.png)

![Yeni öğe için komut dosyası sayfası](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create-snippet.png)

### <a name="data-lake-analytics-assembly-registration"></a>Data Lake Analytics bütünleştirilmiş kod kaydı

**Derlemeler** düğümüne sağ tıklayıp **derlemeyi kaydet**' i seçerek ilgili veritabanına bir derlemeyi kaydedebilirsiniz.

![Derlemeler düğümünün kısayol menüsünde "derlemeyi Kaydet" komutu](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer-register-assembly.png)

## <a name="integrate-with-azure-data-lake-store-from-the-explorer"></a>Gezgin Azure Data Lake Store ile tümleştirme

**Data Lake Store**gidin:

- Klasör düğümüne sağ tıklayıp **Yenile**, **Sil**, **karşıya yükle**, **klasörü karşıya yükle**, **göreli yolu Kopyala**ve kısayol menüsünde **tam yolu Kopyala** komutlarını kullanabilirsiniz.

   ![Data Lake Gezgininde klasör düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-folder-menu.png)

- Dosya düğümüne sağ tıklayıp **Önizleme**, **indirme**, **silme**, **ayıklama betiği oluşturma** (yalnızca CSV, TSV ve txt dosyaları için kullanılabilir), **göreli yolu Kopyala**ve kısayol menüsünde **tam yolu Kopyala** komutlarını kullanabilirsiniz.

   ![Data Lake Gezgininde bir dosya düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-extract.png)

## <a name="integrate-with-azure-blob-storage-from-the-explorer"></a>Gezgin 'den Azure Blob depolama ile tümleştirme

Blob depolamaya gidin:

- Blob kapsayıcı düğümüne sağ tıklayıp ardından **Yenile**, **BLOB kapsayıcısını Sil**ve BLOB komutlarını kısayol menüsünde **Yükle** komutlarını kullanabilirsiniz.

   ![BLOB depolama alanındaki bir blob kapsayıcı düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/blob-storage-blob-container-node.png)

- Klasör düğümüne sağ tıklayıp ardından kısayol menüsünde blobu **Yenile** ve **karşıya yükle** komutlarını kullanabilirsiniz.

   ![BLOB depolama altındaki klasör düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/blob-storage-folder-node.png)

- Dosya düğümüne sağ tıklayıp **Önizleme/düzenleme**, **indirme**, **silme**, **ayıklama betiği oluşturma** (yalnızca CSV, TSV ve txt dosyaları için kullanılabilir), **göreli yolu Kopyala**ve kısayol menüsünde **tam yolu Kopyala** komutlarını kullanabilirsiniz.

    ![BLOB depolama alanındaki bir dosya düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-from-context-menu-2.png)

## <a name="open-the-data-lake-explorer-in-the-portal"></a>Portalda Data Lake Gezginini açın

1. Komut paletini açmak için CTRL + SHIFT + P ' yi seçin.
2. **Açık web Azure Depolama Gezgini** girin veya bir göreli yolu ya da bir tam yolu sağ tıklatın ve ardından **Web Azure Depolama Gezgini aç**' ı seçin.
3. Bir Data Lake Analytics hesabı seçin.

Data Lake araçlar Azure portal Azure depolama yolunu açar. Yolu bulabilir ve dosyanın önizlemesini web 'den görüntüleyebilirsiniz.

## <a name="additional-features"></a>Ek özellikler

VS Code için Data Lake araçları aşağıdaki özellikleri destekler:

- **IntelliSense otomatik tamamlama**: öneriler, Yöntemler ve değişkenler gibi öğelerin etrafında açılır pencereler içinde görünür. Farklı simgeler farklı nesne türlerini temsil eder:

  - Scala veri türü
  - Karmaşık veri türü
  - Yerleşik Udtalar
  - .NET koleksiyonu ve sınıfları
  - C# ifadeleri
  - Yerleşik C# UDF 'Leri, UDOs ve Udadags
  - U-SQL işlevleri
  - U-SQL Pencereleme işlevleri

    ![IntelliSense nesne türleri](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)

- **Data Lake Analytics meta verilerde IntelliSense otomatik tamamlama**: Data Lake araçları Data Lake Analytics meta veri bilgilerini yerel olarak indirir. IntelliSense özelliği, nesneleri Data Lake Analytics meta verilerden otomatik olarak doldurur. Bu nesneler veritabanı, şema, tablo, görünüm, tablo değerli işlev, yordamlar ve C# derlemelerini içerir.

  ![IntelliSense meta verileri](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

- **IntelliSense hata işaretleyicisi**: Data Lake araçları, U-SQL ve C# için hataları düzenlemenin alt çizgileri.
- **Sözdizimi vurguları**: Data Lake araçları, değişkenleri, anahtar sözcükleri, veri türleri ve işlevleri gibi öğeleri ayırt etmek için renkler kullanır.

    ![Çeşitli renklerle sözdizimi](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

> [!NOTE]
> Visual Studio için Azure Data Lake Araçları sürüm 2.3.3000.4 veya sonraki bir sürüme yükseltmenizi öneririz. Önceki sürümler artık indirilemiyor ve kullanım dışı.  

## <a name="next-steps"></a>Sonraki adımlar

- [VS Code Azure Data Lake Analytics için Python, R ve C Sharp ile U-SQL geliştirme](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
- [U-SQL yerel çalıştırma ve Visual Studio Code ile yerel hata ayıklama](data-lake-tools-for-vscode-local-run-and-debug.md)
- [Öğretici: Azure Data Lake Analytics kullanmaya başlayın](data-lake-analytics-get-started-portal.md)
- [Öğretici: Visual Studio için Data Lake araçları 'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
