---
title: Elastik Veritabanı Araçları ile başlayın
description: Kolay çalıştırılanabilecek örnek uygulama da dahil olmak üzere Azure SQL Veritabanı'nın Elastik Veritabanı Araçları özelliğinin temel açıklaması.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anumjs
ms.author: anjangsh
ms.reviewer: sstein
ms.date: 01/25/2019
ms.openlocfilehash: 4577f611287a27366b1168e07565861c16fe0e38
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76987031"
---
# <a name="get-started-with-elastic-database-tools"></a>Elastik Veritabanı Araçları ile başlayın

Bu belge, örnek bir uygulama çalıştırmanıza yardımcı olarak [elastik veritabanı istemci kitaplığı](sql-database-elastic-database-client-library.md) için geliştirici deneyimi tanıtır. Örnek uygulama basit bir parçalı uygulama oluşturur ve Azure SQL Veritabanı'nın Elastik Veritabanı Araçları özelliğinin temel özelliklerini inceler. [Bu shard harita yönetimi,](sql-database-elastic-scale-shard-map-management.md)veri [bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md)ve çok [parça](sql-database-elastic-scale-multishard-querying.md)lı sorgulama için kullanım örnekleri üzerinde duruluyor. İstemci kitaplığı .NET'in yanı sıra Java için de kullanılabilir. 

## <a name="elastic-database-tools-for-java"></a>Java için Elastik Veritabanı Araçları

### <a name="prerequisites"></a>Ön koşullar

* Java Geliştirici Kiti (JDK), sürüm 1.8 veya sonrası
* [Maven](https://maven.apache.org/download.cgi)
* Azure'da bir SQL Veritabanı sunucusu veya yerel bir SQL Server örneği

### <a name="download-and-run-the-sample-app"></a>Örnek uygulamayı indirin ve çalıştırın

JAR dosyalarını oluşturmak ve örnek projeye başlamak için aşağıdakileri yapın: 
1. Örnek uygulamayla birlikte istemci kitaplığını içeren [GitHub deposunu](https://github.com/Microsoft/elastic-db-tools-for-java) klonla. 

2. _./sample/src/main/resources/resource.properties_ dosyasını aşağıdakileri ayarlamak için edin:
    * TEST_CONN_USER
    * TEST_CONN_PASSWORD
    * TEST_CONN_SERVER_NAME

3. Örnek projeyi oluşturmak için _./örnek_ dizininde aşağıdaki komutu çalıştırın:

    ```
    mvn install
    ```
    
4. Örnek projeyi başlatmak için _./örnek_ dizininde aşağıdaki komutu çalıştırın: 
    
    ```
    mvn -q exec:java "-Dexec.mainClass=com.microsoft.azure.elasticdb.samples.elasticscalestarterkit.Program"
    ```
    
5. İstemci kitaplığı yetenekleri hakkında daha fazla bilgi edinmek için çeşitli seçenekleri deneyin. Örnek uygulama uygulaması hakkında bilgi edinmek için kodu keşfetmekten çekinmeyin.

    ![İlerleme-java][5]
    
Tebrikler! Azure SQL Veritabanı'nda Elastik Veritabanı Araçları'nı kullanarak ilk kırık uygulamanızı başarıyla oluşturmuş ve çalıştırmış sınız. SQL veritabanınıza bağlanmak ve numunenin oluşturduğu kırıklara hızlıca bakmak için Visual Studio veya SQL Server Management Studio'yu kullanın. Yeni örnek parça veritabanları ve örnek oluşturduğu bir parça harita yöneticisi veritabanı fark edeceksiniz. 

İstemci kitaplığını kendi Maven projenize eklemek için POM dosyanıza aşağıdaki bağımlılıkları ekleyin:

```xml
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>elastic-db-tools</artifactId> 
    <version>1.0.0</version> 
</dependency> 
```

## <a name="elastic-database-tools-for-net"></a>.NET için Elastik Veritabanı Araçları

### <a name="prerequisites"></a>Ön koşullar

* Visual Studio 2012 veya daha sonra C# ile. Visual Studio Downloads ücretsiz bir sürümünü [indirin.](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* NuGet 2.7 veya sonrası. En son sürümü almak için [NuGet'i Yükleme'ye](https://docs.nuget.org/docs/start-here/installing-nuget)bakın.

### <a name="download-and-run-the-sample-app"></a>Örnek uygulamayı indirin ve çalıştırın

Kitaplığı yüklemek için [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)'a gidin. Kitaplık, aşağıdaki bölümde açıklanan örnek uygulamayla birlikte yüklenir.

Örneği indirmek ve çalıştırmak için aşağıdaki adımları izleyin: 

1. Azure [SQL için Elastik DB Araçlarını İndirin - Başlangıç örneğini başlatın.](https://github.com/Azure/elastic-db-tools) Örneği seçtiğiniz bir konuma aç.

2. Bir proje oluşturmak için *C#* dizininden *ElasticScaleStarterKit.sln* çözümlerini açın.

3. Örnek proje için çözümde *app.config* dosyasını açın. Ardından Azure SQL Veritabanı sunucu adınızı ve oturum açma bilgilerinizi (kullanıcı adı ve parola) eklemek için dosyadaki yönergeleri izleyin.

4. Uygulamayı derleyin ve çalıştırın. Sizden istendiğinde, Visual Studio'nun çözümün NuGet paketlerini geri yüklemesini etkinleştirin. Bu eylem, NuGet'den elastik veritabanı istemci kitaplığı en son sürümünü indirir.

5. İstemci kitaplığı yetenekleri hakkında daha fazla bilgi edinmek için çeşitli seçenekleri deneyin. Uygulamanın konsol çıkışında attığı adımlara dikkat edin ve arka daki kodu keşfetmekten çekinmeyin.
   
    ![İlerleme durumu][4]

Tebrikler! SQL Veritabanı'nda Elastik Veritabanı Araçları'nı kullanarak ilk kırık uygulamanızı başarıyla oluşturmuş ve çalıştırmışsınız. SQL veritabanınıza bağlanmak ve numunenin oluşturduğu kırıklara hızlıca bakmak için Visual Studio veya SQL Server Management Studio'yu kullanın. Yeni örnek parça veritabanları ve örnek oluşturduğu bir parça harita yöneticisi veritabanı fark edeceksiniz.

> [!IMPORTANT]
> Azure ve SQL Veritabanı güncellemeleriyle senkronize kalmanız için Management Studio'nun en son sürümünü her zaman kullanmanızı öneririz. [SQL Server Management Studio’yu güncelleyin](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="key-pieces-of-the-code-sample"></a>Kod örneğinin anahtar parçaları

* **Parça ve parça haritalarının yönetilmesi**: Kod, *ShardManagementUtils.cs* dosyasındaki kırıklar, aralıklar ve eşlemelerle nasıl çalışılabildiğini gösterir. Daha fazla bilgi için [bkz.](https://go.microsoft.com/?linkid=9862595)  

* **Veriye bağlı yönlendirme**: Hareketlerin sağ parçaya yönlendirmesi *DataDependentRoutingSample.cs* dosyasında gösterilir. Daha fazla bilgi için [veriye bağımlı yönlendirme](https://go.microsoft.com/?linkid=9862596)ye bakın. 

* **Birden çok parça üzerinde sorgulama**: MultiShardQuerySample.cs dosyasında kırıklar arasında sorgulama yapılır. *MultiShardQuerySample.cs* Daha fazla bilgi için, [Çoklu Parça sorgulama'ya](https://go.microsoft.com/?linkid=9862597)bakın.

* **Boş kırıkekleme**: Yeni boş parçaların yinelemeli *eklenmesi, CreateShardSample.cs* dosyasındaki kod tarafından gerçekleştirilir. Daha fazla bilgi için [bkz.](https://go.microsoft.com/?linkid=9862595)

## <a name="other-elastic-scale-operations"></a>Diğer elastik ölçekli işlemler

* **Varolan bir parçanın bölünmesi**: Parçaları bölme yeteneği bölme birleştirme aracı tarafından sağlanır. Daha fazla bilgi için [bkz.](sql-database-elastic-scale-overview-split-and-merge.md)

* **Varolan parçaların birleştirilmesi**: Parça birleştirmeler de bölme birleştirme aracı kullanılarak gerçekleştirilir. Daha fazla bilgi için [bkz.](sql-database-elastic-scale-overview-split-and-merge.md)   

## <a name="cost"></a>Maliyet

Elastik Veritabanı Araçları kitaplığı ücretsizdir. Elastik Veritabanı Araçları'nı kullandığınızda, Azure kullanımınızın maliyetinin ötesinde ek ücret ödemezsiniz. 

Örneğin, örnek uygulama yeni veritabanları oluşturur. Bu özelliğin maliyeti, seçtiğiniz SQL Veritabanı sürümüne ve uygulamanızın Azure kullanımına bağlıdır.

Fiyatlandırma bilgileri için [SQL Veritabanı fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/sql-database/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Elastik Veritabanı Araçları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* Kod örnekleri: 
  * Elastik Veritabanı Araçları ([.NET](https://github.com/Azure/elastic-db-tools), [Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-elasticdb-tools%22))
  * [Azure SQL için Elastik Veritabanı Araçları - Entity Framework Integration](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
  * [Script Merkezi'nde Shard Elasticity](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
* Blog: [Elastik Ölçek duyurusu](https://azure.microsoft.com/blog/20../../introducing-elastic-scale-preview-for-azure-sql-database/)
* Kanal 9: [Elastik Ölçek genel bakış videosu](https://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
* Tartışma forumu: [Azure SQL Veritabanı forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
* Performansı ölçmek [için: Sabit harita yöneticisi için performans sayaçları](sql-database-elastic-database-client-library.md)

<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
[5]: ./media/sql-database-elastic-scale-get-started/java-client-library.PNG

