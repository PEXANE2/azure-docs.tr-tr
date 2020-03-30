---
title: U-SQL veritabanı projesi geliştirme - Azure Veri Gölü
description: Visual Studio için Azure Veri Gölü Araçlarını kullanarak bir U-SQL veritabanını nasıl geliştireceklerini öğrenin.
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 07/03/2018
ms.openlocfilehash: a9b271b5f7d4e53dbf871d03dd43b62b9299aa53
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71309919"
---
# <a name="use-a-u-sql-database-project-to-develop-a-u-sql-database-for-azure-data-lake"></a>Azure Veri Gölü için bir U-SQL veritabanı geliştirmek için bir U-SQL veritabanı projesi kullanma

U-SQL veritabanı, tablolarda yapılandırılmamış veriler ve yönetilen yapılandırılmış veriler üzerinde yapılandırılmış görünümler sağlar. Ayrıca, yapılandırılmış verilerinizi ve özel kodunuzu düzenlemek için genel bir meta veri kataloğu sistemi de sağlar. Veritabanı, bu ilgili nesneleri bir araya getiren kavramdır.

[U-SQL veritabanı ve Veri Tanım Dili (DDL)](/u-sql/data-definition-language-ddl-statements)hakkında daha fazla bilgi edinin. 

U-SQL veritabanı projesi Visual Studio'da geliştiricilerin U-SQL veritabanlarını hızlı ve kolay bir şekilde geliştirmelerine, yönetmelerine ve dağıtmalarına yardımcı olan bir proje türüdür.

## <a name="create-a-u-sql-database-project"></a>U-SQL veritabanı projesi oluşturma

Visual Studio için Azure Veri Gölü Araçları, sürüm 2.3.3000.0'dan sonra U-SQL veritabanı projesi adı verilen yeni bir proje şablonu ekledi. Bir U-SQL projesi oluşturmak için **Dosya > Yeni > Projesi'ni**seçin. U-SQL Veritabanı **Projesi, Azure Veri Gölü > U-SQL düğümü**altında bulunabilir.

![Visual Studio için Veri Gölü Araçları-U-SQL veritabanı projesi oluşturma](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-create-usql-database-project-creation.png) 

## <a name="develop-u-sql-database-objects-by-using-a-database-project"></a>Veritabanı projesi kullanarak U-SQL veritabanı nesneleri geliştirme

U-SQL veritabanı projesini sağ tıklatın. Yeni **öğeyi >** seçin. **Yeni Öğe Ekle** Sihirbazı'nda desteklenen tüm yeni nesne türlerini bulabilirsiniz. 

Derleme olmayan bir nesne (örneğin, tablo değerindebir işlev) için, yeni bir öğe ekledikten sonra yeni bir U-SQL komut dosyası oluşturulur. Düzenleyicide bu nesne için DDL deyimini geliştirmeye başlayabilirsiniz.

Bir derleme nesnesi için araç, derlemeyi kaydetmenize ve DLL dosyalarını ve diğer ek dosyaları dağıtmanıza yardımcı olan kullanıcı dostu bir kullanıcı arabirimi düzenleyicisi sağlar. Aşağıdaki adımlar, U-SQL veritabanı projesine montaj nesnesi tanımının nasıl ekleyeceğinigösterir:

1.  U-SQL veritabanı projesi için UDO/UDAG/UDF içeren C# projesine başvurular ekleyin.

    ![Visual Studio için Veri Gölü Araçları-- U-SQL veritabanı proje başvurusu ekle](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-project-reference.png) 

    ![Visual Studio için Veri Gölü Araçları-- U-SQL veritabanı proje başvurusu ekle](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-project-reference-wizard.png)

2.  Derleme tasarım görünümünde, başvuru yuvarlanması menüsünden derleme **oluştur'dan** başvurulan derlemeyi seçin.

    ![Visual Studio için Veri Gölü Araçları- başvurudan montaj oluşturma](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-create-assembly-from-reference.png)

3.  **Varsa Yönetilen Bağımlılıklar** ve **Ek Dosyalar** ekleyin. Ek dosyalar eklediğinizde, araç montajları hem yerel makinenizde hem de daha sonra yapı makinesinde bulabileceğinden emin olmak için göreli yolu kullanır. 

@_DeployTempDirectoryaracı yapı çıktıklasörüne işaret eden önceden tanımlanmış bir değişkendir. Yapı çıktısı klasörü altında, her derlemenin derleme adı ile adlandırılan bir alt klasörü vardır. Tüm DL'ler ve ek dosyalar bu alt klasördedir. 
 
## <a name="build-a-u-sql-database-project"></a>U-SQL veritabanı projesi oluşturma

U-SQL veritabanı projesinin yapı çıktısı, sonek `.usqldbpack`ile birlikte bir U-SQL veritabanı dağıtım paketidir. Paket, `.usqldbpack` **DDL** klasöründeki tek bir U-SQL komut dosyasındaki tüm DDL ifadelerini ve **Temp** klasöründeki derlemeler için tüm DL'leri ve ek dosyaları içeren bir .zip dosyasıdır.

[MSBuild komut satırı ve Azure DevOps Hizmetleri oluşturma görevi yle bir U-SQL veritabanı projesi oluşturma](data-lake-analytics-cicd-overview.md)hakkında daha fazla bilgi edinin.

## <a name="deploy-a-u-sql-database"></a>U-SQL veritabanıdağıtma

.usqldbpack paketi Visual Studio veya dağıtım SDK kullanılarak yerel bir hesaba veya Azure Veri Gölü Analytics hesabına dağıtılabilir. 

### <a name="deploy-a-u-sql-database-in-visual-studio"></a>Visual Studio'da U-SQL veritabanı dağıtma

Visual Studio'da bir U-SQL veritabanı projesi veya .usqldbpack paketi aracılığıyla bir U-SQL veritabanı dağıtabilirsiniz.

#### <a name="deploy-through-a-u-sql-database-project"></a>U-SQL veritabanı projesi aracılığıyla dağıtma

1.  U-SQL veritabanı projesini sağ tıklatın ve sonra **Dağıt'ı**seçin.
2.  **U-SQL Veritabanı**Sihirbazı'nda, veritabanını dağıtmak istediğiniz **ADLA hesabını** seçin. Hem yerel hesaplar hem de ADLA hesapları desteklenir.
3.  **Veritabanı Kaynağı** otomatik olarak doldurulur ve projenin yapı çıktı klasöründeki .usqldbpack paketine işaret eder.
4.  Veritabanı oluşturmak için **Veritabanı Adı'na** bir ad girin. Hedef Azure Veri Gölü Analizi hesabında aynı ada sahip bir veritabanı zaten varsa, veritabanı projesinde tanımlanan tüm nesneler veritabanını yeniden oluşturmadan oluşturulur.
5.  U-SQL veritabanını dağıtmak için **Gönder'i**seçin. Tüm kaynaklar (derlemeler ve ek dosyalar) yüklenir ve tüm DDL ekstrelerini içeren bir U-SQL işi gönderilir.

    ![Visual Studio için Veri Gölü Araçları-- U-SQL veritabanı projesini dağıtın](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-project.png)

    ![Visual Studio için Veri Gölü Araçları--U-SQL veritabanı proje sihirbazı dağıt](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-project-wizard.png)

#### <a name="deploy-through-a-u-sql-database-deployment-package"></a>U-SQL veritabanı dağıtım paketi aracılığıyla dağıtma

1.  **Sunucu Gezgini'ni**açın. Ardından, veritabanını dağıtmak istediğiniz **Azure Veri Gölü Analizi hesabını** genişletin.
2.  Sağ tıklayın **U-SQL Veritabanları**ve sonra **Veritabanı dağıt'ı**seçin.
3.  **Veritabanı Kaynağını** U-SQL veritabanı dağıtım paketi (.usqldbpack dosyası) yoluna ayarlayın.
4.  Veritabanı oluşturmak için **Veritabanı Adını** girin. Hedef Azure Veri Gölü Analizi hesabında zaten var olan aynı ada sahip bir veritabanı varsa, veritabanı projesinde tanımlanan tüm nesneler veritabanını yeniden oluşturmadan oluşturulur.

    ![Visual Studio için Veri Gölü Araçları-- U-SQL veritabanı paketini dağıtın](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-package.png)

    ![Visual Studio için Veri Gölü Araçları-- U-SQL veritabanı paket sihirbazı dağıt](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-deploy-usql-database-package-wizard.png)
  
### <a name="deploy-u-sql-database-by-using-the-sdk"></a>SDK'yı kullanarak U-SQL veritabanını dağıtma

`PackageDeploymentTool.exe`U-SQL veritabanlarını dağıtmaya yardımcı olan programlama ve komut satırı arabirimlerini sağlar. SDK, [U-SQL SDK Nuget paketinde](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) `build/runtime/PackageDeploymentTool.exe`yer almaktadır.

[SDK ve U-SQL veritabanı dağıtımı için CI/CD ardışık kurulumu hakkında daha fazla bilgi edinin.](data-lake-analytics-cicd-overview.md)

## <a name="reference-a-u-sql-database-project"></a>Bir U-SQL veritabanı projesine başvuru

Bir U-SQL projesi bir U-SQL veritabanı projesine başvuruyapabilir. Başvuru iki iş yükünü etkiler:

- *Project build*: U-SQL komut dosyalarını oluşturmadan önce başvurulan veritabanı ortamlarını ayarlayın. 
- *(Yerel proje) hesabına karşı yerel çalıştırma*: Başvurulan veritabanı ortamları U-SQL komut dosyası yürütmesinden önce (yerel proje) hesaba dağıtılır. [Yerel çalıştırmalar ve (yerel makine) ve (yerel proje) hesabı arasındaki fark hakkında daha fazla bilgi edinin.](data-lake-analytics-data-lake-tools-local-run.md)

### <a name="how-to-add-a-u-sql-database-reference"></a>U-SQL veritabanı başvurusu nasıl eklenir?

1. **Solution Explorer'daki**U-SQL projesine sağ tıklayın ve ardından **Ekle U-SQL Veritabanı Başvurusu'nu seçin...** seçeneğini belirleyin.

    ![Visual Studio için Veri Gölü Araçları -- veritabanı proje başvurusu ekleyin](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-database-project-reference.png)

2. Geçerli çözümde veya U-SQL veritabanı paket dosyasında bir U-SQL veritabanı projesinden bir veritabanı başvurusu yapılandırın.
3. Veritabanının adını sağlayın.

    ![Visual Studio için Veri Gölü Araçları veritabanı proje başvuru sihirbazı ekleyin](./media/data-lake-analytics-data-lake-tools-develop-usql-database/data-lake-tools-add-database-project-reference-wizard.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Gölü Analitiği için BIR CI/CD ardışık nokta nasıl ayarlanır?](data-lake-analytics-cicd-overview.md)
- [Azure Veri Gölü Analizi kodunuzu nasıl test emilir?](data-lake-analytics-cicd-test.md)
- [Yerel makinenizde U-SQL komut dosyası çalıştırma](data-lake-analytics-data-lake-tools-local-run.md)
