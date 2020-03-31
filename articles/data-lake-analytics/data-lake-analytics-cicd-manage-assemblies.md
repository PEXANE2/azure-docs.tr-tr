---
title: Bir CI/CD ardışık alanda U-SQL derlemelerini yönetme - Azure Veri Gölü
description: Azure DevOps ile BIR CI/CD ardışık alanda U-SQL C# derlemelerini yönetmek için en iyi uygulamaları öğrenin.
author: yanancai
ms.author: yanacai
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: e6de10ed712688e4ee9dccc22176e81ad5e574ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71315842"
---
# <a name="best-practices-for-managing-u-sql-assemblies-in-a-cicd-pipeline"></a>Bir CI/CD ardışık alanda U-SQL derlemelerini yönetmek için en iyi uygulamalar

Bu makalede, yeni tanıtılan U-SQL veritabanı projesiyle U-SQL derleme kaynak kodunu nasıl yönetisiniz öğreneceksiniz. Ayrıca, Azure DevOps'leri kullanarak montaj kaydı için sürekli bir tümleştirme ve dağıtım (CI/CD) ardışık nasıl ayarlayabileceğinizi de öğrenirsiniz.

## <a name="use-the-u-sql-database-project-to-manage-assembly-source-code"></a>Derleme kaynak kodunu yönetmek için U-SQL veritabanı projesini kullanma

[U-SQL veritabanı projesi](data-lake-analytics-data-lake-tools-develop-usql-database.md) Visual Studio'da geliştiricilerin U-SQL veritabanlarını hızlı ve kolay bir şekilde geliştirmelerine, yönetmelerine ve dağıtmalarına yardımcı olan bir proje türüdür. U-SQL veritabanı projesi ile tüm U-SQL veritabanı nesnelerini (kimlik bilgileri hariç) yönetebilirsiniz. 

C# derleme kaynak kodunu ve montaj kaydı DDL U-SQL komut dosyalarını yönetmek için aşağıdakileri kullanın:

* U-SQL veritabanı projesi montaj kaydı U-SQL komut yönetmek için.
* Sınıf Kitaplığı (U-SQL Uygulaması için) C# kaynak kodunu ve kullanıcı tanımlı işleçler, işlevler ve toplayıcılar (UdOs, UDF'ler ve UDAG'lar) için bağımlılıkları yönetmek için.
* Sınıf Kitaplığı projesine başvurmak için U-SQL veritabanı projesi. 

Bir U-SQL veritabanı projesi, Sınıf Kitaplığı (U-SQL Uygulaması İçin) projesine başvurulabilir. Bu Sınıf Kitaplığı (U-SQL Application için) projesinden başvurulan C# kaynak kodunu kullanarak U-SQL veritabanına kayıtlı derlemeler oluşturabilirsiniz.

Proje oluşturmak ve referanseklemek için aşağıdaki adımları izleyin.
1. **Dosya** > **Yeni** > **Projesi'ni**seçerek Sınıf Kitaplığı (U-SQL Uygulaması İçin) projesi oluşturun. Proje, Azure **Veri Gölü > U-SQL** düğümü altındadır.

   ![Visual Studio için Veri Gölü Araçları--C# sınıfı kitaplık projesi oluşturma](./media/data-lake-analytics-cicd-manage-assemblies/create-c-sharp-class-library-project.png)
1. Sınıf Kitaplığı (U-SQL Application için) projesine kullanıcı tanımlı C# kodunuzu ekleyin.

1. **Dosya** > **Yeni** > **Projesi'ni**seçerek bir U-SQL projesi oluşturun. Proje, Azure **Veri Gölü** > **U-SQL** düğümü altındadır.

   ![Visual Studio için Veri Gölü Araçları-- U-SQL veritabanı projesi oluşturma](media/data-lake-analytics-cicd-manage-assemblies/create-u-sql-database-project.png)
1. U-SQL veritabanı projesi için C# sınıfı kitaplık projesine bir başvuru ekleyin.

    ![Visual Studio için Veri Gölü Araçları-- U-SQL veritabanı proje başvurusu ekle](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference.png) 

    ![Visual Studio için Veri Gölü Araçları-- U-SQL veritabanı proje başvurusu ekle](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference-wizard.png)

5. Projeyi sağ tıklatarak ve **Yeni Öğe Ekle'yi**seçerek U-SQL veritabanı projesinde bir derleme komut dosyası oluşturun.

   ![Visual Studio için Veri Gölü Araçları-- Montaj komut dosyası ekleyin](media/data-lake-analytics-cicd-manage-assemblies/add-assembly-script.png)

1. Derleme tasarım görünümünde montaj komut dosyasını açın. Başvuru açılır **menüsünden Create derlemesinden** başvurulan derlemeyi seçin.

    ![Visual Studio için Veri Gölü Araçları- başvurudan montaj oluşturma](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-create-assembly-from-reference.png)

7. **Varsa Yönetilen Bağımlılıklar** ve **Ek Dosyalar**ekleyin. Ek dosyalar eklediğinizde, araç yerel makinenizde ve daha sonra yapı makinesinde derlemeleri bulabileceğinden emin olmak için göreli yolu kullanır.

alttaki düzenleyici penceresinde_DeployTempDirectory aracı yapı çıktıklasörüne işaret eden önceden tanımlanmış bir değişkendir. ** \@** Yapı çıktısı klasörü altında, her derlemenin derleme adı ile adlandırılan bir alt klasörü vardır. Tüm DL'ler ve ek dosyalar bu alt klasördedir.

## <a name="build-a-u-sql-database-project"></a>U-SQL veritabanı projesi oluşturma

U-SQL veritabanı projesinin yapı çıktısı bir U-SQL veritabanı dağıtım paketidir. Soneki `.usqldbpack`yle isimlendirilir. Paket, `.usqldbpack` DDL klasöründeki tek bir U-SQL komut dosyasındaki tüm DDL ifadelerini içeren bir .zip dosyasıdır. Tüm yapılı .dll dosyaları ve derlemeler için ek dosyalar Temp klasöründedir.

## <a name="deploy-a-u-sql-database"></a>U-SQL veritabanıdağıtma

Paket, `.usqldbpack` yerel bir hesaba veya Azure Veri Gölü Analizi hesabına dağıtılabilir. Visual Studio'yu veya dağıtım SDK'sını kullanın. 

### <a name="deploy-a-u-sql-database-in-visual-studio"></a>Visual Studio'da U-SQL veritabanı dağıtma

Visual Studio'da bir U-SQL veritabanı projesi veya `.usqldbpack` paket kullanarak bir U-SQL veritabanı dağıtabilirsiniz.

#### <a name="deploy-by-using-a-u-sql-database-project"></a>U-SQL veritabanı projesi kullanarak dağıtma

1.  U-SQL veritabanı projesini sağ tıklatın ve sonra **Dağıt'ı**seçin.
2.  **U-SQL Veritabanını Dağıt** sihirbazında, veritabanını dağıtmak istediğiniz **ADLA Hesabını** seçin. Hem yerel hesaplar hem de ADLA hesapları desteklenir.
3.  **Veritabanı Kaynağı** otomatik olarak doldurulur. Projenin yapı çıktı klasöründeki .usqldbpack paketini işaret eder.
4.  Veritabanı oluşturmak için **Veritabanı Adı'na** bir ad girin. Hedef Azure Veri Gölü Analizi hesabında aynı ada sahip bir veritabanı zaten varsa, veritabanı projesinde tanımlanan tüm nesneler veritabanını yeniden oluşturmadan oluşturulur.
5.  U-SQL veritabanını dağıtmak için **Gönder'i**seçin. Derlemeler ve ek dosyalar gibi tüm kaynaklar yüklenir. Tüm DDL ekstrelerini içeren bir U-SQL işi gönderilir.

    ![Visual Studio için Veri Gölü Araçları-- U-SQL veritabanı projesini dağıtın](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project.png)

    ![Visual Studio için Veri Gölü Araçları--U-SQL veritabanı proje sihirbazı dağıt](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project-wizard.png)

### <a name="deploy-a-u-sql-database-in-azure-devops"></a>Azure DevOps'lerde Bir U-SQL veritabanı dağıtma

`PackageDeploymentTool.exe`U-SQL veritabanlarını dağıtmaya yardımcı olan programlama ve komut satırı arabirimlerini sağlar. SDK, [U-SQL SDK Nuget paketinde](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) `build/runtime/PackageDeploymentTool.exe`yer almaktadır.

Azure DevOps'lerde, U-SQL veritabanı yenilemesi için bir otomasyon ardışık birimi kurmak için bir komut satırı görevi ve bu SDK kullanabilirsiniz. [SDK ve U-SQL veritabanı dağıtımı için bir CI/CD ardışık kurulumu hakkında daha fazla bilgi edinin.](data-lake-analytics-cicd-overview.md#deploy-u-sql-database-through-azure-pipelines)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gölü Analitiği için BIR CI/CD ardışık nokta ayarlama](data-lake-analytics-cicd-overview.md)
* [Azure Veri Gölü Analizi kodunuzu test edin](data-lake-analytics-cicd-test.md)
* [Yerel makinenizde U-SQL komut dosyası çalıştırma](data-lake-analytics-data-lake-tools-local-run.md)
