---
title: C ve C++ kullanarak SQL Veritabanına bağlanın
description: C++ ile modern bir uygulama oluşturmak için bu hızlı başlangıçta örnek kodu kullanın ve Azure SQL Veritabanı ile bulutta güçlü bir ilişkisel veritabanı tarafından desteklenen.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: cpp
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/12/2018
ms.openlocfilehash: fb6094ec418d2b212759bddd2c4d49c7e6193849
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73690709"
---
# <a name="connect-to-sql-database-using-c-and-c"></a>C ve C++ kullanarak SQL Veritabanına bağlanın

Bu yazı, Azure SQL DB'ye bağlanmaya çalışan C ve C++ geliştiricileri hedefleniyor. Bu bölümlere ayrılmıştır böylece en iyi ilgi yakalar bölümüne atlayabilirsiniz.

## <a name="prerequisites-for-the-cc-tutorial"></a>C/C++ eğitimi için ön koşullar

Aşağıdaki öğelere sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [Ücretsiz Azure Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* [Görsel Stüdyo](https://www.visualstudio.com/downloads/). Bu örneği oluşturmak ve çalıştırmak için C++ dil bileşenlerini yüklemeniz gerekir.
* [Visual Studio Linux Geliştirme](https://visualstudiogallery.msdn.microsoft.com/725025cf-7067-45c2-8d01-1e0fd359ae6e). Linux üzerinde geliştiriyorsanız, Visual Studio Linux uzantısını da yüklemeniz gerekir.

## <a name="azure-sql-database-and-sql-server-on-virtual-machines"></a><a id="AzureSQL"></a>Sanal makinelerde Azure SQL Veritabanı ve SQL Server
Azure SQL, Microsoft SQL Server'da yerleşiktir ve yüksek kullanılabilirlik, performans ve ölçeklenebilir bir hizmet sağlamak üzere tasarlanmıştır. SQL Azure'u, şirket içinde çalışan özel veritabanınız üzerinden kullanmanın birçok avantajı vardır. SQL Azure ile veritabanınızı yüklemek, kurmak, bakımını yapmak veya yönetmek zorunda değilsiniz, yalnızca veritabanınızın içeriğini ve yapısını yüklemeniz gerekir. Hata toleransı ve artıklık gibi veritabanları ile ilgili endişe tipik şeyler tüm yerleşiktir.

Azure'un şu anda SQL sunucu iş yüklerini barındırmak için iki seçeneği vardır: Azure SQL veritabanı, hizmet olarak veritabanı ve Sanal Makinelerde SQL sunucusu (VM). Azure SQL veritabanının, bulut hizmetlerinin sağladığı maliyet tasarrufu ve performans optimizasyonundan yararlanmak için yeni bulut tabanlı uygulamalar için en iyi bahis olması dışında, bu ikisi arasındaki farklar hakkında ayrıntılı bilgi almayacağız. Şirket içi uygulamalarınızı buluta geçirmeyi veya genişletmeyi düşünüyorsanız, Azure sanal makinesindeki SQL sunucusu sizin için daha iyi çalışabilir. Bu makale için basit şeyler tutmak için bir Azure SQL veritabanı oluşturalım.

## <a name="data-access-technologies-odbc-and-ole-db"></a><a id="ODBC"></a>Veri erişim teknolojileri: ODBC ve OLE DB
Azure SQL DB'ye bağlanmak da farklı değildir ve şu anda veritabanlarına bağlanmanın iki yolu vardır: ODBC (Açık Veritabanı bağlantısı) ve OLE DB (Nesne Bağlama ve Katıştırma veritabanı). Son yıllarda, Microsoft yerel [ilişkisel veri erişimi için ODBC](https://blogs.msdn.microsoft.com/sqlnativeclient/20../../microsoft-is-aligning-with-odbc-for-native-relational-data-access/)ile hizalamıştır. ODBC nispeten basit ve aynı zamanda Çok Daha hızlı OLE DB daha. Buradaki tek uyarı, ODBC'nin eski bir C tarzı API kullanmasıdır.

## <a name="step-1--creating-your-azure-sql-database"></a><a id="Create"></a>Adım 1: Azure SQL Veritabanınızı Oluşturma
Örnek veritabanı oluşturmayı öğrenmek için [başlangıç sayfasına](sql-database-single-database-get-started.md) bakın.  Alternatif olarak, Azure portalını kullanarak bir Azure SQL veritabanı oluşturmak için bu [kısa iki dakikalık videoyu](https://azure.microsoft.com/documentation/videos/azure-sql-database-create-dbs-in-seconds/) takip edebilirsiniz.

## <a name="step-2--get-connection-string"></a><a id="ConnectionString"></a>Adım 2: Bağlantı dizesi alın
Azure SQL veritabanınız sağlandıktan sonra, bağlantı bilgilerini belirlemek ve güvenlik duvarı erişimi için istemci IP'nizi eklemek için aşağıdaki adımları gerçekleştirmeniz gerekir.

[Azure portalında,](https://portal.azure.com/)veritabanınızın genel bakış bölümünün bir parçası olarak listelenen veritabanı **bağlantı dizelerini** göster'i kullanarak Azure SQL veritabanı odbc bağlantı dizenize gidin:

![ODBCConnectionString](./media/sql-database-develop-cplusplus-simple/azureportal.png)

![ODBCConnectionStringProps](./media/sql-database-develop-cplusplus-simple/dbconnection.png)

**ODBC (Node.js içerir) [SQL kimlik doğrulama]** dizesinin içeriğini kopyalayın. Bu dizeyi daha sonra C++ ODBC komut satırı yorumlayıcısından bağlanmak için kullanırız. Bu dize, sürücü, sunucu ve diğer veritabanı bağlantı parametreleri gibi ayrıntıları sağlar.

## <a name="step-3--add-your-ip-to-the-firewall"></a><a id="Firewall"></a>Adım 3: IP'nizi güvenlik duvarına ekleyin
Veritabanı sunucunuzun güvenlik duvarı bölümüne gidin ve başarılı bir bağlantı kurabileceğimizden emin olmak için [aşağıdaki adımları kullanarak istemci IP'nizi güvenlik duvarına](sql-database-configure-firewall-settings.md) ekleyin:

![AddyourIPWindow](./media/sql-database-develop-cplusplus-simple/ip.png)

Bu noktada, Azure SQL DB'nizi yapılandırmış ve C++ kodunuzdan bağlanmaya hazırsınız.

## <a name="step-4-connecting-from-a-windows-cc-application"></a><a id="Windows"></a>Adım 4: Windows C/C++ uygulamasından bağlanma
Visual Studio ile yapılan [bu örneği kullanarak Windows'daki ODBC'yi kullanarak Azure SQL DB'nize](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28windows%29) kolayca bağlanabilirsiniz. Örnek, Azure SQL DB'mize bağlanmak için kullanılabilecek bir ODBC komut satırı yorumlayıcısı uygular. Bu örnek, komut satırı bağımsız değişkeni olarak bir Veritabanı kaynak adı dosyası (DSN) dosyasını veya Azure portalından daha önce kopyaladığımız ayrıntılı bağlantı dizesini alır. Bu proje için özellik sayfasını getirin ve bağlantı dizesini burada gösterildiği gibi bir komut bağımsız değişkeni olarak yapıştırın:

![DSN Propsfile](./media/sql-database-develop-cplusplus-simple/props.png)

Veritabanı bağlantı dizesinin bir parçası olarak veritabanınız için doğru kimlik doğrulama ayrıntılarını sağladığınızdan emin olun.

Oluşturmak için uygulamayı başlatın. Başarılı bir bağlantıyı doğrulayan aşağıdaki pencereyi görmeniz gerekir. Veritabanı bağlantınızı doğrulamak için **tablo oluşturma** gibi bazı temel SQL komutlarını da çalıştırabilirsiniz:

![SQL Komutları](./media/sql-database-develop-cplusplus-simple/sqlcommands.png)

Alternatif olarak, komut bağımsız değişkenleri sağlanmadığında başlatılan sihirbazı kullanarak bir DSN dosyası oluşturabilirsiniz. Bu seçeneği de denemenizi öneririz. Bu DSN dosyanızı otomasyon ve kimlik doğrulama ayarlarınızı korumak için kullanabilirsiniz:

![DSN dosyası oluşturma](./media/sql-database-develop-cplusplus-simple/datasource.png)

Tebrikler! Artık Windows'ta C++ ve ODBC'yi kullanarak Azure SQL'e başarıyla bağlandınız. Linux platformu için de aynı şeyi yapmak için okumaya devam edebilirsiniz.

## <a name="step-5-connecting-from-a-linux-cc-application"></a><a id="Linux"></a>Adım 5: Linux C/C++ uygulamasından bağlanma
Henüz haberleri duymadıysanız, Visual Studio artık C++ Linux uygulamasını da geliştirmenize olanak sağlıyor. Linux Geliştirme için [Visual C++](https://blogs.msdn.microsoft.com/vcblog/20../../visual-c-for-linux-development/) blogunda bu yeni senaryo yu okuyabilirsiniz. Linux için inşa etmek için, Linux dağıtımınızın çalıştığı uzak bir makineye ihtiyacınız var. Kullanılabilir bir tane niz yoksa, Linux Azure Sanal [makineleri](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)kullanarak hızlı bir şekilde bir tane ayarlayabilirsiniz.

Bu öğretici için, bize bir Ubuntu 16.04 Linux dağıtım kurmak olduğunu varsayalım. Buradaki adımlar Ubuntu 15.10, Red Hat 6 ve Red Hat 7 için de geçerli olmalıdır.

Dağıtımınız için SQL ve ODBC için gereken kitaplıkları aşağıdaki adımlar yükleyin:

    sudo su
    sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/mssql-ubuntu-test/ xenial main" > /etc/apt/sources.list.d/mssqlpreview.list'
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    apt-get update
    apt-get install msodbcsql
    apt-get install unixodbc-dev-utf16 #this step is optional but recommended*

Visual Studio’yu başlatın. Araçlar -> Seçenekleri -> Çapraz Platform -> Bağlantı Yöneticisi altında, Linux kutunuza bir bağlantı ekleyin:

![Araç Seçenekleri](./media/sql-database-develop-cplusplus-simple/tools.png)

SSH üzerinden bağlantı kurulduktan sonra Boş proje (Linux) şablonu oluşturun:

![Yeni proje şablonu](./media/sql-database-develop-cplusplus-simple/template.png)

Daha sonra yeni bir C kaynak dosyası ekleyebilir [ve bu içerikle değiştirebilirsiniz.](https://github.com/Microsoft/VCSamples/blob/master/VC2015Samples/ODBC%20database%20sample%20%28linux%29/odbcconnector/odbcconnector.c) ODBC API'leri SQLAllocHandle, SQLSetConnectAttr ve SQLDriverConnect'i kullanarak, veritabanınıza bir bağlantı kurabilirsiniz.
Windows ODBC örneğinde olduğu gibi, SQLDriverConnect çağrısını daha önce Azure portalından kopyalanan veritabanı bağlantı dize parametrelerinizin ayrıntılarıyla değiştirmeniz gerekir.

     retcode = SQLDriverConnect(
        hdbc, NULL, "Driver=ODBC Driver 13 for SQL"
                    "Server;Server=<yourserver>;Uid=<yourusername>;Pwd=<"
                    "yourpassword>;database=<yourdatabase>",
        SQL_NTS, outstr, sizeof(outstr), &outstrlen, SQL_DRIVER_NOPROMPT);

Derlemeden önce yapılacak son şey, **odbc'yi** kitaplık bağımlılığı olarak eklemektir:

![Giriş kitaplığı olarak ODBC ekleme](./media/sql-database-develop-cplusplus-simple/lib.png)

Uygulamanızı başlatmak için **Hata Ayıklama** menüsünden Linux Konsolu'nu getirin:

![Linux Konsolu](./media/sql-database-develop-cplusplus-simple/linuxconsole.png)

Bağlantınız başarılı olduysa, artık Linux Konsolu'nda yazdırılan geçerli veritabanı adını görmeniz gerekir:

![Linux Konsol Pencere Çıkışı](./media/sql-database-develop-cplusplus-simple/linuxconsolewindow.png)

Tebrikler! Öğreticiyi başarıyla tamamladınız ve artık Windows ve Linux platformlarında C++'dan Azure SQL DB'nize bağlanabilirsiniz.

## <a name="get-the-complete-cc-tutorial-solution"></a><a id="GetSolution"></a>Tam C/C++ öğretici çözüme ulaşın
Bu makalede tüm örnekleri içeren GetStarted çözümlerini GitHub'da bulabilirsiniz:

* [ODBC C++ Windows örneği](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28windows%29), Azure SQL'e bağlanmak için Windows C++ ODBC Örneğini İndirin
* [ODBC C++ Linux örneği](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28linux%29), Azure SQL'e bağlanmak için Linux C++ ODBC Örneğini İndirin

## <a name="next-steps"></a>Sonraki adımlar
* SQL [Veritabanı Geliştirme Genel Bakışını](sql-database-develop-overview.md) Gözden Geçirin
* [ODBC API Başvurusu](https://docs.microsoft.com/sql/odbc/reference/syntax/odbc-api-reference/) hakkında daha fazla bilgi

## <a name="additional-resources"></a>Ek kaynaklar
* [Azure SQL Veritabanı ile Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [SQL Veritabanı'nın](https://azure.microsoft.com/services/sql-database/) tüm özelliklerini keşfedin

