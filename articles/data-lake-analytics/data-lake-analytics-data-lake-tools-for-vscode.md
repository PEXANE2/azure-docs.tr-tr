---
title: Visual Studio Code için Azure Data Lake Araçları’nı kullanma
description: U-SQL komut dosyaları oluşturmak, test etmek ve çalıştırmak için Visual Studio Code için Azure Veri Göl Araçlarını nasıl kullanacağınızı öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: Jejiang
ms.author: jejiang
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.topic: conceptual
ms.date: 02/09/2018
ms.openlocfilehash: 5042d89f1cb5e928444e4b3c9a23db7bb1d66585
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60509349"
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Visual Studio Code için Azure Data Lake Araçları’nı kullanma

Bu makalede, U-SQL komut dosyaları oluşturmak, test etmek ve çalıştırmak için Visual Studio Code (VS Kodu) için Azure Veri Göl Araçlarını nasıl kullanabileceğinizi öğrenin. Bilgiler ayrıca aşağıdaki videoda yer almaktadır:

<a href="https://channel9.msdn.com/Series/AzureDataLake/Azure-Data-Lake-Tools-for-VSCode?term=ADL%20Tools%20for%20VSCode"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>Ön koşullar

VS Kodu için Azure Veri Gölü Araçları Windows, Linux ve macOS'u destekler.U-SQL yerel çalıştırma ve yerel hata ayıklama yalnızca Windows'da çalışır.

- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx)

MacOS ve Linux için:
- [.NET Çekirdek SDK 2.0](https://www.microsoft.com/net/download/core)
- [Mono 5.2.x](https://www.mono-project.com/download/)

## <a name="install-azure-data-lake-tools"></a>Azure Veri Gölü Araçlarını Yükleme

Ön koşulları yükledikten sonra VS Kodu için Azure Veri Gölü Araçları yükleyebilirsiniz.

**Azure Veri Gölü Araçlarını yüklemek için**

1. Visual Studio Code'u açın.
2. Sol bölmedeki **Uzantılar'ı** seçin. Arama kutusuna **Azure Veri Gölü Araçları** girin.
3. Azure Veri **Gölü Araçlarının**yanında **Yükle'yi** seçin. 

   ![Veri Gölü Araçlarını yüklemek için seçimler](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

   Birkaç saniye sonra **Yükle** düğmesi **Yeniden Yükle'ye**dönüşür.
4. Azure Veri Gölü **Araçları** uzantısını etkinleştirmek için **Yeniden Yükle'yi** seçin.
5. Onaylamak için **Yeniden Yükle Penceresini** seçin. **Uzantılar** bölmesinde **Azure Veri Gölü Araçlarını** görebilirsiniz.

 
## <a name="activate-azure-data-lake-tools"></a>Azure Veri Gölü Araçlarını Etkinleştirme
Uzantıyı etkinleştirmek için bir .usql dosyası oluşturun veya varolan bir .usql dosyasını açın. 


## <a name="work-with-u-sql"></a>U-SQL ile çalışma

U-SQL ile çalışmak için bir U-SQL dosyasını veya klasörü açmanız gerekir.

**Örnek komut dosyasını açmak için**

Komut paletini açın (Ctrl+Shift+P) ve **ADL girin: Örnek Komut Dosyasını Aç.** Bu örneğin başka bir örneğini açar. Ayrıca bu örnekte bir komut dosyası da ayarlayabilir, yapılandırabilir ve gönderebilirsiniz.

**U-SQL projeniz için bir klasör açmak için**

1. Visual Studio Code'dan **Dosya** menüsünü seçin ve ardından **Klasörü Aç'ı**seçin.
2. Bir klasör belirtin ve ardından **Klasör'ü Seçin'i**seçin.
3. **Dosya** menüsünü seçin ve ardından **Yeni'yi**seçin. Projeye Adsız-1 dosyası eklenir.
4. Adsız-1 dosyasına aşağıdaki kodu girin:

        @departments  = 
            SELECT * FROM 
                (VALUES
                    (31,    "Sales"),
                    (33,    "Engineering"), 
                    (34,    "Clerical"),
                    (35,    "Marketing")
                ) AS 
                      D( DepID, DepName );
         
        OUTPUT @departments
            TO "/Output/departments.csv"
        USING Outputters.Csv();

    Komut dosyası , /output klasöründe yer alan bazı verileri içeren bir departments.csv dosyası oluşturur.

5. Dosyayı açılan klasöre **myUSQL.usql** olarak kaydedin.

**U-SQL komut dosyası derlemek için**

1. Komut paletini açmak için Ctrl+Shift+P'yi seçin. 
2. **ADL girin: Script derlemek**. Derleme sonuçları **Çıktı** penceresinde görünür. Ayrıca bir komut dosyası dosyası dosyasına sağ tıklayabilir ve ardından U-SQL işini derlemek için **KOMUT Dosyasını Derle'yi** seçebilirsiniz. Derleme sonucu **Çıktı** bölmesinde görünür.
 
**U-SQL komut dosyası göndermek için**

1. Komut paletini açmak için Ctrl+Shift+P'yi seçin. 
2. **ADL girin: İş gönder.** Ayrıca bir komut dosyası dosyası dosyayı sağ tıklayabilir ve ardından **ADL: İş Gönder'i**seçebilirsiniz. 

Bir U-SQL işi gönderdikten sonra, gönderi günlükleri VS Kodu'ndaki **Çıkış** penceresinde görünür. İş görünümü sağ bölmede görünür. Gönderim başarılı olursa, iş URL'si de görüntülenir. Gerçek zamanlı iş durumunu izlemek için iş URL'sini bir web tarayıcısında açabilirsiniz. 

İş görünümünün **ÖZET** sekmesinde iş ayrıntılarını görebilirsiniz. Ana işlevler, komut dosyasını yeniden göndermeyi, komut dosyasını çoğaltmayı ve portalda açık olmak gibi temel işlevlerdir. İş görünümü **veri** sekmesinde, giriş dosyalarına, çıktı dosyalarına ve kaynak dosyalarına başvurabilirsiniz. Dosyalar yerel bilgisayara indirilebilir.

![İş görünümünde özet sekmesi](./media/data-lake-analytics-data-lake-tools-for-vscode/job-view-summary.png)

![İş görünümünde veri sekmesi](./media/data-lake-analytics-data-lake-tools-for-vscode/job-view-data.png)

**Varsayılan bağlamı ayarlamak için**

Dosyalar için parametreleri tek tek ayarlamadıysanız, bu ayarı tüm komut dosyası dosyalarına uygulamak için varsayılan bağlamı ayarlayabilirsiniz.

1. Komut paletini açmak için Ctrl+Shift+P'yi seçin. 
2. **ADL girin: Varsayılan Bağlamı Ayarlayın.** Veya komut dosyası düzenleyicisini sağ tıklatın ve **ADL'yi seçin: Varsayılan Bağlamı Ayarlayın.**
3. İstediğiniz hesabı, veritabanını ve şemayı seçin. Ayar xxx_settings.json yapılandırma dosyasına kaydedilir.

   ![Varsayılan bağlam olarak hesap, veritabanı ve şema kümesi](./media/data-lake-analytics-data-lake-tools-for-vscode/default-context-sequence.png)

**Komut dosyası parametrelerini ayarlamak için**

1. Komut paletini açmak için Ctrl+Shift+P'yi seçin. 
2. **ADL girin: Komut Dosyası Parametrelerini ayarlayın.**
3. xxx_settings.json dosyası aşağıdaki özelliklerle açılır:

   - **hesap**: Azure aboneliğiniz altında U-SQL işlerini derlemek ve çalıştırmak için gereken bir Azure Veri Gölü Analizi hesabı. U-SQL işlerini derlemeden ve çalıştırmadan önce bilgisayar hesabını yapılandırmanız gerekir.
   - **veritabanı**: Hesabınız altında bir veritabanı. Varsayılan **ana**.
   - şema : Veritabanınızın altında bir **şema.** Varsayılan **dbo'dur.**
   - **isteğe bağlıAyarlar**:
        - **öncelik**: Öncelik aralığı 1 ile 1000 arasındadır ve en yüksek öncelik olarak 1'dir. Varsayılan değer **1000**'dir.
        - **dereceParalellik**: Paralellik aralığı 1 ile 150 arasındadır. Varsayılan değer, Azure Veri Gölü Analizi hesabınızda izin verilen maksimum paralelliktir. 

   ![JSON dosyasının içeriği](./media/data-lake-analytics-data-lake-tools-for-vscode/default-context-setting.png)
      
> [!NOTE] 
> Yapılandırmayı kaydettikten sonra, varsayılan bir bağlam ayarlı yoksa ilgili .usql dosyasının sol alt köşesindeki durum çubuğunda hesap, veritabanı ve şema bilgileri görünür.

**Git'i yok saymayı ayarlamak için**

1. Komut paletini açmak için Ctrl+Shift+P'yi seçin. 
2. **ADL girin: Git Yoksay'ı ayarla.**

   - VS Code çalışma klasörünüzde **bir .gitIgnore** dosyanız yoksa, klasörünüzde **.gitIgnore** adlı bir dosya oluşturulur. Dört öğe **(usqlCodeBehindReference**, **usqlCodeBehindGenerated**, **.cache**, **obj**) varsayılan olarak dosyaya eklenir. Gerekirse daha fazla güncelleştirme yapabilirsiniz.
   - VS Code çalışma klasörünüzde zaten bir **.gitIgnore** dosyası varsa, araç dosyaya dahil edilmediyse **.gitIgnore** dosyanıza dört öğe **(usqlCodeBehindReference**, **usqlCodeBehindGenerated**, **.cache**, **obj**) ekler.

   ![.gitIgnore dosyasındaki öğeler](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-gitignore.png)


## <a name="work-with-code-behind-files-c-sharp-python-and-r"></a>Kod arkası dosyalarla çalışma: C Sharp, Python ve R

Azure Veri Gölü Araçları, birden çok özel kodu destekler. Talimatlar için, [VS Kodu'nda Azure Veri Gölü Analitiği için Python, R ve C Sharp ile U-SQL Geliştir'e](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)bakın.

## <a name="work-with-assemblies"></a>Montajlarla çalışma

Derlemegeliştirme hakkında daha fazla bilgi için [bkz.](data-lake-analytics-u-sql-develop-assemblies.md)

Data Lake Analytics kataloğunda özel kod derlemelerini kaydetmek için Veri Gölü Araçlarını kullanabilirsiniz.

**Bir derlemeyi kaydetmek için**

Derlemeyi **ADL: Register Assembly** veya **ADL: Register Assembly (Advanced)** komutu ile kaydedebilirsiniz.

**ADL üzerinden kayıt yaptırmak için: Register Assembly komutu**
1.  Komut paletini açmak için Ctrl+Shift+P'yi seçin.
2.  **ADL girin: Kayıt Montaj**. 
3.  Yerel montaj yolunu belirtin. 
4.  Bir Data Lake Analytics hesabı seçin.
5.  Bir veritabanı seçin.

Portal bir tarayıcıda açılır ve montaj kayıt işlemini görüntüler.  

**ADL'yi** tetiklemenin daha kullanışlı bir yolu: Register Assembly komutu, Dosya Gezgini'ndeki .dll dosyasına sağ tıklamaktır. 

**ADL üzerinden kayıt olmak için: Register Assembly (Advanced) komutu**
1.  Komut paletini açmak için Ctrl+Shift+P'yi seçin.
2.  **ADL girin: Kayıt Montaj (Gelişmiş)**. 
3.  Yerel montaj yolunu belirtin. 
4.  JSON dosyası görüntülenir. Gerekirse derleme bağımlılıklarını ve kaynak parametrelerini gözden geçirin ve edin. Yönergeler **Çıktı** penceresinde görüntülenir. Montaj kaydına devam etmek için JSON dosyasını kaydedin (Ctrl+S).

    ![Montaj bağımlılıkları ve kaynak parametreleri ile JSON dosyası](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
    
>[!NOTE]
>- Azure Veri Gölü Araçları, DLL'nin montaj bağımlılığı olup olmadığını otomatik olarak algılar. Bağımlılıklar algılandıktan sonra JSON dosyasında görüntülenir. 
>- DLL kaynaklarınızı (örneğin, .txt, .png ve .csv) montaj kaydının bir parçası olarak yükleyebilirsiniz. 

ADL'yi tetiklemenin başka bir **yolu: Register Assembly (Advanced)** komutu, Dosya Gezgini'ndeki .dll dosyasına sağ tıklamaktır. 

Aşağıdaki U-SQL kodu, derlemenin nasıl çağrıldığını gösterir. Örnekte, derleme adı *test*edilir.


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


## <a name="use-u-sql-local-run-and-local-debug-for-windows-users"></a>Windows kullanıcıları için U-SQL yerel çalıştırma ve yerel hata ayıklama kullanma
U-SQL yerel çalıştırma, yerel verilerinizi sınar ve kodunuz Data Lake Analytics'te yayınlanmadan önce komut dosyanızı yerel olarak doğrular. Kodunuz Data Lake Analytics'e gönderilmeden önce aşağıdaki görevleri tamamlamak için yerel hata ayıklama özelliğini kullanabilirsiniz: 
- C# kod unuzu hata ayıklayın. 
- Kodu geç. 
- Komut dosyanızı yerel olarak doğrulayın.

Yerel çalıştırma ve yerel hata ayıklama özelliği yalnızca Windows ortamlarında çalışır ve macOS ve Linux tabanlı işletim sistemlerinde desteklenmez.

Yerel çalıştırma ve yerel hata ayıklama yla ilgili talimatlar için Visual [Studio Code ile U-SQL yerel çalıştırma ve yerel hata ayıklama](data-lake-tools-for-vscode-local-run-and-debug.md)bölümüne bakın.


## <a name="connect-to-azure"></a>Azure'a Bağlanma

Data Lake Analytics'te U-SQL komut dosyalarını derleyip çalıştırabilmek için önce Azure hesabınıza bağlanmanız gerekir.

<b id="sign-in-by-command">Komut kullanarak Azure'a bağlanmak için</b>

1.  Komut paletini açmak için Ctrl+Shift+P'yi seçin. 
2.  **ADL girin: Giriş**. Giriş bilgileri sağ altta görünür.

    ![Giriş komutunu girme](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)

    ![Oturum açma ve kimlik doğrulama hakkında bildirim](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)

3.  [Giriş web sayfasını](https://aka.ms/devicelogin)açmak için & **Aç'ı** seçin. Kodu kutuya yapıştırın ve sonra **Devam et'i**seçin.

    ![Giriş web sayfası](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png)  
     
4.  Web sayfasından oturum açma yönergelerini izleyin. Bağlandığınızda, Azure hesap adınız VS Kodu penceresinin sol alt köşesindeki durum çubuğunda görünür. 

> [!NOTE] 
>- Veri Gölü Araçları, oturum açmazsanız bir sonraki seferde sizi otomatik olarak imzalar.
>- Hesabınızın iki etken etkinleştirilmişolması durumunda, PIN kullanmak yerine telefon kimlik doğrulamasını kullanmanızı öneririz.


Oturumu imzalamak için ADL komutunu **girin: Oturum aç.**

**Gezginden Azure'a bağlanmak için**

**AZURE DATALAKE'i**genişletin, **Azure'da Oturum Aç'ı**seçin ve ardından bir komut kullanarak [Azure'a bağlanmak için](#sign-in-by-command)3 ve adım 4'ü izleyin.

![Explorer'da "Azure'da oturum aç" seçimi](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-sign-in-from-explorer.png )  

Kaşiften çıkış yapamazsınız. Oturum dışı kalmak için [bkz.](#sign-in-by-command)


## <a name="create-an-extraction-script"></a>Çıkarma komut dosyası oluşturma 
**ADL: EXTRACT Script oluşturma** veya Azure Veri Gölü gezgininden .csv, .tsv ve .txt dosyaları için bir çıkarma komut dosyası oluşturabilirsiniz.

**Komut kullanarak bir çıkarma komutu oluşturmak için**

1. Komut paletini açmak için Ctrl+Shift+P'yi seçin ve **ADL girin: EXTRACT Script'i oluşturun.**
2. Azure Depolama dosyasının tam yolunu belirtin ve Enter tuşunu seçin.
3. Bir hesap seçin.
4. .txt dosyası için, dosyayı ayıklamak için bir sınır layıcı seçin. 

![Çıkarma komut dosyası oluşturma işlemi](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-process.png)

Çıkarma komut dosyası, girişlerinize göre oluşturulur. Sütunları algılayabilen bir komut dosyası için iki seçenekarasından birini seçin. Değilse, yalnızca bir komut dosyası oluşturulur.

![Çıkarma komut dosyası oluşturmanın sonucu](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-result.png)

**Gezginden bir çıkarma komut dosyası oluşturmak için**

Çıkarma komut dosyasını oluşturmanın başka bir yolu da,.csv,.tsv veya Azure Veri Gölü Deposu'ndaki .txt dosyasındaki sağ tıklama (kısayol) menüsünden geçer.

![Kısayol menüsünden "EXTRACT Script Oluşturma" komutu](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-from-context-menu.png)

## <a name="integrate-with-azure-data-lake-analytics-through-a-command"></a>Komut aracılığıyla Azure Veri Gölü Analizi ile tümleştirme

Hesapları listelemek, meta verilere erişmek ve analitik işlerini görüntülemek için Azure Veri Gölü Analizi kaynaklarına erişebilirsiniz. 

**Azure aboneliğiniz altında Azure Veri Gölü Analizi hesaplarını listelemek için**

1. Komut paletini açmak için Ctrl+Shift+P'yi seçin.
2. **ADL girin: Liste Hesapları**. Hesaplar **Çıktı** bölmesinde görünür.

**Azure Veri Gölü Analizi meta verilerine erişmek için**

1.  Ctrl+Shift+P'yi seçin ve ardından **ADL girin: Liste Tabloları.**
2.  Veri Gölü Analizi hesaplarından birini seçin.
3.  Data Lake Analytics veritabanlarından birini seçin.
4.  Şemalardan birini seçin. Tabloların listesini görebilirsiniz.

**Azure Veri Gölü Analizi işlerini görüntülemek için**
1.  Komut paletini açın (Ctrl+Shift+P) ve **ADL'yi seçin: İş İlanları Göster.** 
2.  Bir Veri Gölü Analizi veya yerel hesap seçin. 
3.  Hesap için iş listesinin görünmesini bekleyin.
4.  İş listesinden bir iş seçin. Veri Gölü Araçları iş görünümünü sağ bölmede açar ve VS Kodu çıktısında bazı bilgileri görüntüler.

    ![İş listesi](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="integrate-with-azure-data-lake-store-through-a-command"></a>Komut aracılığıyla Azure Veri Gölü Deposu ile tümleştirme

Azure Veri Gölü Deposu ile ilgili komutları şu şekilde kullanabilirsiniz:
 - [Azure Veri Gölü Deposu kaynaklarına göz atın](#list-the-storage-path) 
 - [Azure Veri Gölü Deposu dosyasını önizleme](#preview-the-storage-file) 
 - VS Kodu'nda dosyayı doğrudan Azure Veri Gölü Deposu'na yükleyin
 - VS Code'da dosyayı doğrudan Azure Veri Gölü Deposu'ndan indirin

### <a name="list-the-storage-path"></a>Depolama yolunu listele 

**Depolama yolunu komut paletinden listelemek için**

1. Komut dosyası düzenleyicisini sağ tıklatın ve **ADL: List Path'i**seçin.
2. Listedeki klasörü seçin veya **bir yol girin** veya kök **yolundan Gözat'ı**seçin. (Örnek olarak **bir yol girin'i** kullanıyoruz.) 
3. Data Lake Analytics hesabınızı seçin.
4. Depolama klasörü yoluna göz atın veya girin (örneğin, /çıktı/).  

Komut paleti, girişlerinizi temel alan yol bilgilerini listeler.

![Depolama yolu sonuçları](./media/data-lake-analytics-data-lake-tools-for-vscode/list-storage-path.png)

Göreli yolu listeletmenin daha kullanışlı bir yolu kısayol menüsünden geçer.

**Kısayol menüsünden depolama yolunu listelemek için**

Yol dizesini sağ tıklatın ve **Liste Yolu'nu**seçin.

![Kısayol menüsünde "Liste Yolu"](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)


### <a name="preview-the-storage-file"></a>Depolama dosyasını önizleme

1. Komut dosyası düzenleyicisini sağ tıklatın ve **ADL: Preview File'yi**seçin.
2. Data Lake Analytics hesabınızı seçin. 
3. Azure Depolama dosya yolunu girin (örneğin, /output/SearchLog.txt). 

Dosya VS Kodu'nda açılır.

![Depolama dosyasını önizleme adımları ve sonucu](./media/data-lake-analytics-data-lake-tools-for-vscode/preview-storage-file.png)

Dosyayı önizlemenin başka bir yolu, dosyanın tam yolundaki kısayol menüsünden veya komut dosyası düzenleyicisindeki dosyanın göreli yolundan geçer. 

### <a name="upload-a-file-or-folder"></a>Dosya veya klasör yükleme

1. Komut dosyası düzenleyicisini sağ tıklatın ve Dosya yükle veya **Klasörü** **Yükle'yi** seçin.
2. **Dosya yükle'yi**seçtiyseniz bir dosya veya birden çok dosya seçin veya **Yükle Klasörünü**seçtiyseniz klasörün tamamını seçin. Ardından **Karşıya Yükle**’yi seçin. 
3. Listedeki depolama klasörünü seçin veya **bir yol girin** veya kök **yolundan Gözat'ı**seçin. (Örnek olarak **bir yol girin'i** kullanıyoruz.) 
4. Data Lake Analytics hesabınızı seçin. 
5. Depolama klasörü yoluna göz atın veya girin (örneğin, /çıktı/). 
6. Yükleme hedefinizi belirtmek için **Geçerli Klasörü Seçin'i** seçin.

![Dosya veya klasör yükleme adımları ve sonucu](./media/data-lake-analytics-data-lake-tools-for-vscode/upload-file.png)    

Dosyaları depolamaalanına yüklemenin başka bir yolu, dosyanın tam yolundaki kısayol menüsünden veya komut dosyası düzenleyicisindeki dosyanın göreli yoludur.

Yükleme [durumunu izleyebilirsiniz.](#check-storage-tasks-status)


### <a name="download-a-file"></a>Dosya indirme 
ADL komutunu kullanarak dosya **indirebilirsiniz: Dosyayı İndir** veya **ADL İndir: Dosyayı İndir (Gelişmiş)**.

**ADL üzerinden dosya indirmek için: Dosyayı İndir (Gelişmiş) komutu**
1. Komut dosyası düzenleyicisini sağ tıklatın ve ardından **Dosyayı İndir 'i (Advanced)** seçin.
2. VS Code bir JSON dosyası görüntüler. Dosya yollarını girebilir ve aynı anda birden çok dosya indirebilirsiniz. Yönergeler **Çıktı** penceresinde görüntülenir. Dosyayı veya dosyaları indirmeye devam etmek için JSON dosyasını (Ctrl+S) kaydedin.

    ![Dosya indirme yolları ile JSON dosyası](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-files.png)

**Çıktı** penceresi dosya indirme durumunu görüntüler.

![İndirme durumu olan çıkış penceresi](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-file-result.png)     

[İndirme durumunu izleyebilirsiniz.](#check-storage-tasks-status)

**ADL üzerinden dosya indirmek için: Dosyayı İndir komutu**

1. Komut dosyası düzenleyicisini sağ tıklatın, **Dosyayı İndir'i**seçin ve ardından **Klasörseç** iletişim kutusundan hedef klasörünü seçin.
2. Listedeki klasörü seçin veya **bir yol girin** veya kök **yolundan Gözat'ı**seçin. (Örnek olarak **bir yol girin'i** kullanıyoruz.) 
3. Data Lake Analytics hesabınızı seçin. 
4. Depolama klasörü yoluna göz atın veya girin (örneğin, /çıktı/) ve ardından karşıdan yüklenebilmek için bir dosya seçin.

![Dosya indirme adımları ve sonucu](./media/data-lake-analytics-data-lake-tools-for-vscode/download-file.png) 

Depolama dosyalarını karşıdan yüklemenin başka bir yolu, dosyanın tam yolundaki kısayol menüsünden veya komut dosyası düzenleyicisindeki dosyanın göreli yoludur.

[İndirme durumunu izleyebilirsiniz.](#check-storage-tasks-status)

### <a name="check-storage-tasks-status"></a>Depolama görevlerinin durumunu denetleme
Yükleme ve indirme durumu durum çubuğunda görünür. Durum çubuğunu seçin ve sonra durum **OUTPUT** sekmesinde görüntülenir.

![Durum çubuğu ve çıktı ayrıntıları](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-status.png)


## <a name="integrate-with-azure-data-lake-analytics-from-the-explorer"></a>Explorer'dan Azure Veri Gölü Analizi ile tümleştirme

Oturum açtıktan sonra, Azure hesabınızın tüm abonelikleri **Azure DATALAKE**altında sol bölmede listelenir. 

![Veri Gölü gezgini](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer.png)

### <a name="data-lake-analytics-metadata-navigation"></a>Data Lake Analytics meta veri gezintisi

Azure aboneliğinizi genişletin. **U-SQL Veritabanları** düğümü altında, U-SQL veritabanınıza göz atabilir ve **Şems,** **Kimlik Bilgileri,** **Derlemeler,** **Tablolar**ve **Dizin**gibi klasörleri görüntüleyebilirsiniz.

### <a name="data-lake-analytics-metadata-entity-management"></a>Data Lake Analytics meta veri varlık yönetimi

**U-SQL Veritabanlarını**genişletin. İlgili düğüme sağ tıklayarak ve kısayol menüsünde **Oluşturulacak Komut Dosyası'nı** seçerek veritabanı, şema, tablo, tablo türü, dizin veya istatistik oluşturabilirsiniz. Açılan komut dosyası sayfasında, komut dosyasını gereksinimlerinize göre edin. Daha sonra işi sağ tıklayarak ve ADL'yi seçerek **gönderin: İş Gönder.** 

Öğeyi oluşturmayı bitirdikten sonra düğüme sağ tıklayın ve sonra öğeyi göstermek için **Yenile'yi** seçin. Ayrıca öğeyi sağ tıklayıp **Sil'i**seçerek de silebilirsiniz.

![Veri Gölü gezginindeki kısayol menüsünde "Oluşturulacak Komut" komutu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create.png)

![Yeni öğe için komut dosyası sayfası](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create-snippet.png)

### <a name="data-lake-analytics-assembly-registration"></a>Data Lake Analytics montaj kaydı

**Derlemeler** düğümüne sağ tıklayıp **kayıt montajını**seçerek ilgili veritabanına bir derleme kaydedebilirsiniz.

![Derlemeler düğümü için kısayol menüsünde "Register assembly" komutu](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer-register-assembly.png)

## <a name="integrate-with-azure-data-lake-store-from-the-explorer"></a>Explorer'dan Azure Veri Gölü Deposu ile tümleştirme

**Veri Gölü Deposu'na**göz atın:

- Klasör düğümünü sağ tıklayıp kısayol menüsünde **Delete** **Klasörü Sil,** **Yükle, Yükle,** **Klasörü Yükle,** **Bağıl Yolu Kopyala**ve Tam Yol Komutlarını **Kopyala** komutlarını kullanabilirsiniz.

   ![Veri Gölü gezginindeki bir klasör düğümü için kısayol menüsü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-folder-menu.png)

- Dosya düğümüne sağ tıklayıp **önizleme,** **İndir,** **Sil,** **EXTRACT Script Oluştur** (yalnızca CSV, TSV ve TXT dosyaları için kullanılabilir), **Göreli Yolu Kopyala**ve Kısayol menüsündeki Tam Yol Komutlarını **Kopyala** komutlarını sağ tıklayabilirsiniz.

   ![Veri Gölü gezgininde bir dosya düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-extract.png)

## <a name="integrate-with-azure-blob-storage-from-the-explorer"></a>Explorer'dan Azure Blob depolama ile tümleştirme

Blob depolama alanına göz atın:

- Blob container düğümünü sağ tıklayıp, kısayol menüsünde **Blob Kapsayıcısını Sil**ve Blob komutlarını **yükleyin** komutlarını **yenileyebilirsiniz.**

   ![Blob depolama altında bir blob konteyner düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/blob-storage-blob-container-node.png)

- Klasör düğümüne sağ tıklayıp kısayol menüsündeki **Blob'u Yenile** ve **Yükle** komutlarını kullanabilirsiniz.

   ![Blob depolama alanının altındaki klasör düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/blob-storage-folder-node.png)

- Dosya düğümüne sağ tıklayıp **önizleme/düzenle,** **indir,** **sil,** **EXTRACT Script** (yalnızca CSV, TSV ve TXT dosyaları için kullanılabilir), **Göreli Yolu Kopyala**ve Kısayol menüsündeki **Tam Yolu Kopyala** komutlarını kullanabilirsiniz.

    ![Blob depolama alanı altında bir dosya düğümü için kısayol menü komutları](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-from-context-menu-2.png)

## <a name="open-the-data-lake-explorer-in-the-portal"></a>Portaldaki Veri Gölü gezginini açma
1. Komut paletini açmak için Ctrl+Shift+P'yi seçin.
2. **Açık Web Azure Depolama Gezgini** girin veya komut dosyası düzenleyicisinde göreli bir yolu veya tam yolu sağ tıklatın ve ardından Web Azure Depolama **Gezgini'ni aç'ı**seçin.
3. Bir Data Lake Analytics hesabı seçin.

Veri Gölü Araçları, Azure portalında Azure Depolama yolunu açar. Yolu bulabilir ve web'den dosyayı önizleyebilirsiniz.

## <a name="additional-features"></a>Ek özellikler

VS Kodu için Veri Gölü Araçları aşağıdaki özellikleri destekler:

-   **IntelliSense otomatik tamamlama**: Öneriler anahtar kelimeler, yöntemler ve değişkenler gibi öğelerin çevresindeki açılır pencerelerde görünür. Farklı simgeler farklı nesne türlerini temsil eder:

    - Scala veri türü
    - Karmaşık veri türü
    - Dahili UDT'ler
    - .NET toplama ve sınıflar
    - C# ifadeleri
    - Dahili C# UDF'ler, UD'ler ve UDAG'lar 
    - U-SQL fonksiyonları
    - U-SQL pencereleme fonksiyonları
 
    ![IntelliSense nesne türleri](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   **Data Lake Analytics meta verileri üzerine IntelliSense otomatik tamamlama**: Data Lake Tools, Data Lake Analytics meta veri bilgilerini yerel olarak indirir. IntelliSense özelliği, Data Lake Analytics meta verilerindeki nesneleri otomatik olarak doldurur. Bu nesneler veritabanı, şema, tablo, görünüm, tablo değerli işlev, yordamlar ve C# derlemeleri içerir.
 
    ![IntelliSense meta verileri](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   **IntelliSense hata işaretçisi**: Data Lake Tools, U-SQL ve C# için düzenleme hatalarının altını çizer. 
-   **Sözdizimi vurguları**: Veri Gölü Araçları değişkenler, anahtar kelimeler, veri türleri ve işlevler gibi öğeleri ayırt etmek için renkleri kullanır. 

    ![Çeşitli renklerde sözdizimi](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

> [!NOTE]
> Visual Studio sürümü 2.3.3000.4 veya sonraki sürüm için Azure Veri Göl Araçları'na yükseltmenizi öneririz. Önceki sürümler artık indirilemiyor ve kullanım dışı.  
   
## <a name="next-steps"></a>Sonraki adımlar
- [VS Code'da Azure Veri Gölü Analitiği için Python, R ve C Sharp ile U-SQL geliştirme](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
- [Visual Studio Code ile U-SQL yerel çalıştırma ve yerel hata ayıklama](data-lake-tools-for-vscode-local-run-and-debug.md)
- [Öğretici: Azure Veri Gölü Analitiği ile başlayın](data-lake-analytics-get-started-portal.md)
- [Öğretici: Visual Studio için Veri Gölü Araçlarını kullanarak U-SQL komut dosyaları geliştirin](data-lake-analytics-data-lake-tools-get-started.md)
