---
title: Dapper ile esnek veritabanı istemci kitaplığını kullanma
description: Dapper ile esnek veritabanı istemci kitaplığı kullanma.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/04/2018
ms.openlocfilehash: 83d24d45d7628a2e02068c8757fa6568d6d3fc37
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73823467"
---
# <a name="using-elastic-database-client-library-with-dapper"></a>Dapper ile esnek veritabanı istemci kitaplığını kullanma
Bu belge, Dapper'ın uygulama oluşturmasına güvenen, ancak aynı zamanda veri katmanlarını ölçeklendirmek için parçalama uygulayan uygulamalar oluşturmak için [esnek veritabanı aracısını](sql-database-elastic-scale-introduction.md) benimsemek isteyen geliştiriciler içindir.  Bu belge, elastik veritabanı araçları ile tümleştirmek için gerekli olan Dapper tabanlı uygulamalarda değişiklikleri göstermektedir. Odak noktamız, Dapper ile elastik veritabanı parça yönetimi ve veriye bağlı yönlendirme oluşturmaktır. 

**Örnek Kod**: [Azure SQL Veritabanı için elastik veritabanı araçları - Dapper tümleştirmesi](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).

**Dapper** ve **DapperExtensions'ı** Azure SQL Veritabanı için esnek veritabanı istemci kitaplığıyla tümleştirmek kolaydır. [Uygulamalarınız, istemci kitaplığından](https://msdn.microsoft.com/library/azure/dn765902.aspx) [OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) çağrısını kullanmak için yeni [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) nesnelerinin oluşturulmasını ve açılmasını değiştirerek veriye bağımlı yönlendirmeyi kullanabilir. Bu, uygulamanızdaki değişiklikleri yalnızca yeni bağlantıların oluşturulduğu ve açıldığı yerlerle sınırlar. 

## <a name="dapper-overview"></a>Dapper'a genel bakış
**Dapper** bir nesne-ilişkisel mapper olduğunu. .NET nesnelerini uygulamanızdan ilişkisel bir veritabanına (veya tam tersi) eşler. Örnek kodun ilk bölümü, elastik veritabanı istemci kitaplığını Dapper tabanlı uygulamalarla nasıl tümleştirebileceğinizi gösterir. Örnek kodun ikinci bölümü, dapper ve DapperExtensions'i kullanırken nasıl tümleştirileni gösterir.  

Dapper'daki mapper işlevi, veritabanı nın yürütülmesi veya sorgulanması için T-SQL deyimlerinin gönderilmesini kolaylaştıran veritabanı bağlantılarında uzantı yöntemleri sağlar. Örneğin, Dapper, .NET nesneleriniz ile **Execute** çağrıları için SQL deyimlerinin parametreleri arasında eş belirlemeyi veya Dapper'dan gelen **Sorgu** çağrılarını kullanarak SQL sorgularınızın sonuçlarını .NET nesnelerinde tüketmeyi kolaylaştırır. 

DapperExtensions kullanırken, artık SQL deyimleri sağlamanız gerekmez. Veritabanı bağlantısı üzerinden **GetList** veya **Ekle** gibi uzantılar yöntemleri arka planda SQL deyimleri oluşturur.

Dapper ve aynı zamanda DapperExtensions bir diğer yararı uygulama veritabanı bağlantısının oluşturulmasını denetler olmasıdır. Bu, parçaların veritabanlarına eşlemesine dayalı olarak veritabanı bağlantılarını aracılık eden esnek veritabanı istemci kitaplığıyla etkileşime yardımcı olur.

Dapper meclisleri almak için, [Dapper nokta net](https://www.nuget.org/packages/Dapper/)bakın. Dapper uzantıları için [DapperExtensions'a](https://www.nuget.org/packages/DapperExtensions)bakın.

## <a name="a-quick-look-at-the-elastic-database-client-library"></a>Elastik veritabanı istemcikitaplığı hızlı bir bakış
Elastik veritabanı istemci kitaplığı ile, *shardlets*denilen uygulama verilerinizin bölümlerini tanımlarsınız, veritabanlarıyla eşlersiniz ve *anahtarları parçalayarak*tanımlarsınız. İhtiyacınız olan kadar veritabanına sahip olabilir ve parçalarınızı bu veritabanlarına dağıtabilirsiniz. Veritabanlarıiçin parçalama anahtar değerlerineşleme kitaplık API'ler tarafından sağlanan bir parça harita tarafından saklanır. Bu özellik **shard harita yönetimi**olarak adlandırılır. Parçalı harita aynı zamanda bir parçalama anahtarı taşıyan istekler için veritabanı bağlantılarının aracısı olarak da hizmet vermektedir. Bu özellik, **veriye bağımlı yönlendirme**olarak adlandırılır.

![Shard haritalar ve veri bağımlı yönlendirme][1]

Shard map yöneticisi, kullanıcıları veritabanlarında eşzamanlı shardlet yönetimi işlemleri gerçekleştiğinde oluşabilecek shardlet verilerine tutarsız görünümlerden korur. Bunu yapmak için, parça haritalar kitaplıkla oluşturulmuş bir uygulama için veritabanı bağlantılarını aracılık eder. Parça yönetimi işlemleri parçayı etkileyebileceğinde, bu parça lı harita işlevinin veritabanı bağlantısını otomatik olarak öldürmesini sağlar. 

Dapper için bağlantı oluşturmak için geleneksel yolu kullanmak yerine [OpenConnectionForKey yöntemini](https://msdn.microsoft.com/library/azure/dn824099.aspx)kullanmanız gerekir. Bu, tüm doğrulamanın gerçekleşmesini ve herhangi bir veri parçaları arasında hareket ettiğinde bağlantıların düzgün yönetilmesini sağlar.

### <a name="requirements-for-dapper-integration"></a>Dapper entegrasyonu için gereksinimler
Hem elastik veritabanı istemci kitaplığı yla hem de Dapper API'larıyla çalışırken, aşağıdaki özellikleri korumak istersiniz:

* **Ölçeklendirme**: Uygulamanın kapasite talepleri için gerektiğinde, parçalanan uygulamanın veri katmanından veritabanları eklemek veya kaldırmak istiyoruz. 
* **Tutarlılık**: Uygulama parçalama kullanılarak ölçeklendirildiklerinden, veriye bağlı yönlendirme gerçekleştirmeniz gerekir. Bunu yapmak için kitaplığın veriye bağlı yönlendirme özelliklerini kullanmak istiyoruz. Özellikle, bozukluğu veya yanlış sorgu sonuçlarını önlemek için parça harita yöneticisi aracılığıyla aracılık edilen bağlantılar tarafından sağlanan doğrulama ve tutarlılık garantilerini korumak istersiniz. Bu, (örneğin) parça parça şu anda Split/Merge API'leri kullanılarak farklı bir parçaya taşınırsa, belirli bir parçaya olan bağlantıların reddedilmesini veya durdurulmasını sağlar.
* **Nesne Eşleme**: Dapper tarafından sağlanan eşlemelerin, uygulamadaki sınıflar ve temel veritabanı yapıları arasında çeviri yapmak için sağladığı kolaylığı korumak istiyoruz. 

Aşağıdaki bölümde Dapper ve **DapperExtensions**dayalı uygulamalar için bu gereksinimleri için rehberlik sağlar. **Dapper**

## <a name="technical-guidance"></a>Teknik Rehberlik
### <a name="data-dependent-routing-with-dapper"></a>Dapper ile veriye bağımlı yönlendirme
Dapper ile uygulama genellikle temel veritabanına bağlantıları oluşturmak ve açmaktan sorumludur. Uygulama tarafından T türü göz önüne alındığında, Dapper sorgu sonuçlarını T. Dapper tipinin .NET koleksiyonları olarak döndürür, T-SQL sonuç satırlarından T tipi nesnelere eşleme gerçekleştirir. Benzer şekilde, Dapper .NET nesnelerini SQL değerlerine veya veri işleme dili (DML) deyimleri için parametrelere eşler. Dapper bu işlevselliği, ADO .NET SQL Client kitaplıklarından normal [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) nesnesi üzerindeki uzantı yöntemleri aracılığıyla sunar. DDR için Elastik Ölçek API'leri tarafından döndürülen SQL bağlantısı da normal [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) nesneleridir. Bu, aynı zamanda basit bir SQL İstemci bağlantısı olduğu için, istemci kitaplığın DDR API tarafından döndürülen tür üzerinde Dapper uzantılarını doğrudan kullanmamıza olanak tanır.

Bu gözlemler, Dapper için elastik veritabanı istemci kitaplığı tarafından aracılık edilen bağlantıların kullanılmasını kolaylaştırıyor.

Bu kod örneği (eşlik eden örnekten) sağ parçaya bağlantı aracılık etmek için kitaplık için uygulama tarafından sağ parça anahtarı sağlanan yaklaşımı göstermektedir.   

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1,
                     connectionString: connStrBldr.ConnectionString,
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

[OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) API'ye yapılan çağrı, varsayılan oluşturma ve SQL Client bağlantısının açılmasının yerini alır. [OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) çağrısı, veriye bağımlı yönlendirme için gereken bağımsız değişkenleri alır: 

* Veribağımlı yönlendirme arabirimlerine erişmek için parça harita
* Parçayı tanımlamak için parçalama tuşu
* Parçaya bağlanmak için kimlik bilgileri (kullanıcı adı ve parola)

Parça eşleme nesnesi, verilen parçalama anahtarının parçasını tutan parçaya bir bağlantı oluşturur. Esnek veritabanı istemcisi API'leri de tutarlılık garantilerini uygulamak için bağlantıyı etiketler. [OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) çağrısı normal bir SQL Client bağlantı nesnesi döndürür, Dapper'dan **Yürüt uzantısı** yöntemine sonraki çağrı standart Dapper uygulaması izler.

Sorgular aynı şekilde çalışır - önce istemci API'den [OpenConnectionForKey'i](https://msdn.microsoft.com/library/azure/dn807226.aspx) kullanarak bağlantıyı açarsınız. Ardından, SQL sorgunuzun sonuçlarını .NET nesnelerine eşlemek için normal Dapper uzantı yöntemlerini kullanırsınız:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1,
                    connectionString: connStrBldr.ConnectionString,
                    options: ConnectionOptions.Validate ))
    {
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT *
                                FROM Blog
                                ORDER BY Name");

           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

DDR bağlantısına sahip **kullanım** bloğunun, blok içindeki tüm veritabanı işlemlerini tenantId1'in tutulduğu parçaya kadar kapsamadığını unutmayın. Sorgu yalnızca geçerli parçada depolanan blogları döndürür, ancak diğer kırıklarda depolananları döndürmez. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Dapper ve DapperExtensions ile veriye bağlı yönlendirme
Dapper, veritabanı uygulamaları geliştirirken veritabanından daha fazla kolaylık ve soyutlama sağlayabilen ek uzantılardan oluşan bir ekosistemle birlikte gelir. DapperExtensions bir örnektir. 

Uygulamanızda DapperExtensions'in kullanılması veritabanı bağlantılarının nasıl oluşturulduğunu ve yönetildiğini değiştirmez. Bağlantıları açmak yine de uygulamanın sorumluluğundadır ve uzantı yöntemleri yle normal SQL İstemci bağlantı nesneleri beklenmektedir. Yukarıda belirtildiği gibi [OpenConnectionForKey'e](https://msdn.microsoft.com/library/azure/dn807226.aspx) güvenebiliriz. Aşağıdaki kod örneklerinin gösterdiği gibi, tek değişiklik artık T-SQL deyimleri yazmak zorunda olmasıdır:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2,
                    connectionString: connStrBldr.ConnectionString,
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

Ve burada sorgu için kod örneği: 

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2,
                    connectionString: connStrBldr.ConnectionString,
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### <a name="handling-transient-faults"></a>Geçici hataların işlenmesi
Microsoft Patterns & Uygulamaları ekibi, uygulama geliştiricilerin bulutta çalışırken karşılaşılan yaygın geçici hata koşullarını azaltmalarına yardımcı olmak için [Geçici Hata İşleme Uygulama Bloğu'nu](https://msdn.microsoft.com/library/hh680934.aspx) yayımladı. Daha fazla bilgi için bkz: [Azim, Tüm Triumphs Secret: Geçici Hata İşleme Uygulama Bloğu kullanarak.](https://msdn.microsoft.com/library/dn440719.aspx)

Kod örneği, geçici hatalara karşı korumak için geçici hata kitaplığına dayanır. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn =
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

Yukarıdaki kodda **SqlDatabaseUtils.SqlRetryPolicy** 10 yeniden deneme sayısı ile bir **SqlDatabaseTransientErrorDetectionStrategy** olarak tanımlanır ve yeniden denemeler arasında 5 saniye bekleme süresi. Hareketleri kullanıyorsanız, geçici bir hata durumunda yeniden deneme kapsamınızın işlemin başına geri gittiğinden emin olun.

## <a name="limitations"></a>Sınırlamalar
Bu belgede özetlenen yaklaşımlar birkaç sınırlama gerektirir:

* Bu belgenin örnek kodu, şema parçaları arasında nasıl yönetilmeyeceğini göstermez.
* Bir istek göz önüne alındığında, tüm veritabanı işleme isteği tarafından sağlanan parçalama anahtarı tarafından tanımlanan tek bir parça içinde içerdiğini varsayalım. Ancak, bu varsayım her zaman tutmaz, örneğin, bir parçalama anahtarı kullanılabilir yapmak mümkün olmadığında. Bu adresi, elastik veritabanı istemcikitap [MultiShardQuery sınıf](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx)içerir. Sınıf, birkaç parça üzerinde sorgulama için bir bağlantı soyutlama uygular. Dapper ile birlikte MultiShardQuery kullanarak bu belgenin kapsamı dışındadır.

## <a name="conclusion"></a>Sonuç
Dapper ve DapperExtensions kullanan uygulamalar, Azure SQL Veritabanı için esnek veritabanı araçlarından kolayca yararlanabilir. Bu belgede özetlenen adımlar sayesinde, bu uygulamalar, elastik veritabanı istemci kitaplığı [OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) çağrısını kullanmak için yeni [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) nesnelerinin oluşturulmasını ve açılmasını değiştirerek aracın veriye bağımlı yönlendirme yeteneğini kullanabilir. Bu, yeni bağlantıların oluşturulduğu ve açıldığı yerler için gerekli uygulama değişikliklerini sınırlar. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
