---
title: Azure Uygulama Hizmeti'nde aralıklı giden bağlantı hatalarını giderme
description: Azure Uygulama Hizmeti'nde aralıklı bağlantı hatalarını ve ilgili performans sorunlarını giderme
author: v-miegge
manager: barbkess
ms.topic: troubleshooting
ms.date: 03/24/2020
ms.author: ramakoni
ms.custom: security-recommendations
ms.openlocfilehash: 028ddccdb989d35710e387081b08a3b973d75bdc
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80367557"
---
# <a name="troubleshooting-intermittent-outbound-connection-errors-in-azure-app-service"></a>Azure Uygulama Hizmeti'nde aralıklı giden bağlantı hatalarını giderme

Bu makale, [Azure Uygulama Hizmeti'nde](https://docs.microsoft.com/azure/app-service/overview)aralıklı bağlantı hatalarını ve ilgili performans sorunlarını gidermenize yardımcı olur. Bu konu, kaynak adresi ağ çevirisi (SNAT) bağlantı noktalarının tükenmesi ve sorun giderme metodolojileri hakkında daha fazla bilgi ve sorun giderme metodolojileri sağlayacaktır. Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyaç duyarsanız, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarına başvurun. Alternatif olarak, bir Azure destek olayı dosyala. [Azure Destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.

## <a name="symptoms"></a>Belirtiler

Azure Uygulaması hizmetinde barındırılan uygulamalar ve Işlevler aşağıdaki belirtilerden birini veya birkaçını gösterebilir:

* Hizmet planındaki örneklerin tümlerinde veya bazılarında yanıt süreleri yavaş.
* Aralıklı 5xx veya **Kötü Ağ Geçidi** hataları
* Zaman arası hata iletileri
* Harici uç noktalara bağlanamadı (SQLDB, Service Fabric, diğer Uygulama hizmetleri vb.)

## <a name="cause"></a>Nedeni

Bu belirtilerin önemli bir nedeni, uygulama örneğinin aşağıdaki sınırlardan birine ulaştığı için dış uç noktaya yeni bir bağlantı açamayabilmesidir:

* TCP Bağlantıları: YapIlebilen giden bağlantı sayısında bir sınır vardır. Bu, kullanılan işçinin boyutuyla ilişkilidir.
* SNAT bağlantı noktaları: [Azure'daki Giden bağlantılarda](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections)tartışıldığı gibi Azure, ortak IP adres alanında Azure dışındaki uç noktalarla iletişim kurmak için kaynak ağ adresi çevirisi (SNAT) ve Yük Dengeleyicisi (müşterilere açık değildir) kullanır. Azure App hizmetindeki her örnekte başlangıçta önceden ayrılmış **128** SNAT bağlantı noktası sayısı verilir. Bu sınır, aynı ana bilgisayar ve bağlantı noktası birleşimine açılan bağlantıları etkiler. Uygulamanız adres ve bağlantı noktası birleşimlerinin bir karışımına bağlantı oluşturuyorsa, SNAT bağlantı noktalarınızı kullanmazsınız. Aynı adres ve bağlantı noktası birleşimine tekrarlanan çağrılar yaptığınızda SNAT bağlantı noktaları kullanılır. Bir bağlantı noktası serbest bırakıldıktan sonra, bağlantı noktası gerektiği gibi yeniden kullanılabilir. Azure Ağı yük dengeleyicisi, SNAT bağlantı noktasını yalnızca 4 dakika bekledikten sonra kapalı bağlantılardan geri alır.

Uygulamalar veya işlevler hızla yeni bir bağlantı açtığında, 128 bağlantı noktasının önceden tahsis edilmiş kotasını hızla tüketebilirler. Daha sonra, dinamik olarak ek SNAT bağlantı noktaları tahsis edilmeden veya geri alınan bir SNAT bağlantı noktasının yeniden kullanımı yoluyla yeni bir SNAT bağlantı noktası kullanılabilir hale dönene kadar engellenirler. Yeni bağlantı oluşturamaması nedeniyle engellenen uygulamalar veya işlevler, bu makalenin **Belirtiler** bölümünde açıklanan sorunlardan birini veya birkaçını yaşamaya başlar.

## <a name="avoiding-the-problem"></a>Sorundan kaçınma

SNAT bağlantı noktası sorunundan kaçınmak, aynı ana bilgisayara ve bağlantı noktasına tekrar tekrar yeni bağlantılar oluşturulmasını önlemek anlamına gelir.

Azure belgelerinin **Giden Bağlantıları'nın** [Sorun çözme bölümünde,](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#problemsolving) SNAT bağlantı noktası yorgunluğunu azaltmaya yönelik genel stratejiler tartışılmıştır. Bu stratejilerden aşağıdakileri Azure Uygulaması hizmetinde barındırılan uygulamalar ve işlevler için geçerlidir.

### <a name="modify-the-application-to-use-connection-pooling"></a>Bağlantı havuzunu kullanmak için uygulamayı değiştirme

* HTTP bağlantılarını bir araya sağlamak [için, HttpClientFactory ile Havuz HTTP bağlantılarını gözden geçirin.](https://docs.microsoft.com/aspnet/core/performance/performance-best-practices#pool-http-connections-with-httpclientfactory)
* SQL Server bağlantı havuzu hakkında daha fazla bilgi için [SQL Server Connection Pooling (ADO.NET)](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling)incelemesini gözden geçirin.
* Varlık çerçeve uygulamaları ile havuzlama uygulamak için, [DbContext havuzu gözden geçirin.](https://docs.microsoft.com/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)

Burada farklı çözüm yığını tarafından Bağlantı havuzu uygulamak için bağlantılar topluluğudur.

#### <a name="node"></a>Node

Varsayılan olarak, NodeJS bağlantıları canlı tutulmaz. Aşağıda, bağlantı havuzu için popüler veritabanları ve paketler yer almaktadır ve bunları nasıl uygulayacağınız için örnekler içerir.

* [Mysql](https://github.com/mysqljs/mysql#pooling-connections)
* [MongoDB](https://blog.mlab.com/2017/05/mongodb-connection-pooling-for-express-applications/)
* [PostgreSQL](https://node-postgres.com/features/pooling)
* [SQL Server](https://github.com/tediousjs/node-mssql#connection-pools)

HTTP Canlı tutun

* [ajankeepalive](https://www.npmjs.com/package/agentkeepalive)
* [Düğüm.js v13.9.0 Dokümantasyon](https://nodejs.org/api/http.html)

#### <a name="java"></a>Java

Aşağıda, bunları nasıl uygulayacağınız için örnekler içeren JDBC bağlantı havuzu için kullanılan popüler kitaplıklar verilmiştir: JDBC Bağlantı Havuzu.

* [Tomcat 8](https://tomcat.apache.org/tomcat-8.0-doc/jdbc-pool.html)
* [C3p0](https://github.com/swaldman/c3p0)
* [HikariCP](https://github.com/brettwooldridge/HikariCP)
* [Apache DBCP](https://commons.apache.org/proper/commons-dbcp/)

HTTP Bağlantı Havuzu

* [Apache Bağlantı Yönetimi](https://hc.apache.org/httpcomponents-client-ga/tutorial/html/connmgmt.html)
* [Sınıf HavuzlamaHttpClientConnectionManager](http://hc.apache.org/httpcomponents-client-ga/httpclient/apidocs/org/apache/http/impl/conn/PoolingHttpClientConnectionManager.html)

#### <a name="php"></a>PHP

PHP bağlantı havuzu desteklemese de, arka uç sunucunuza kalıcı veritabanı bağlantılarını kullanmayı deneyebilirsiniz.
 
* MySQL sunucusu

   * Yeni sürümler için [MySQLi bağlantıları](https://www.php.net/manual/mysqli.quickstart.connections.php)
   * [PHP'nin](https://www.php.net/manual/function.mysql-pconnect.php) eski sürümleri için mysql_pconnect

* Diğer veri Kaynakları

   * [PHP Bağlantı Yönetimi](https://www.php.net/manual/en/pdo.connections.php)

#### <a name="python"></a>Python

* [Mysql](https://github.com/mysqljs/mysql#pooling-connections)
* [MongoDB](https://blog.mlab.com/2017/05/mongodb-connection-pooling-for-express-applications/)
* [PostgreSQL](https://node-postgres.com/features/pooling)
* [SQL Server](https://github.com/tediousjs/node-mssql#connection-pools) (NOT: SQLAlchemy MicrosoftSQL Server dışında diğer veritabanları ile kullanılabilir)
* [HTTP Keep-alive](https://requests.readthedocs.io/en/master/user/advanced/#keep-alive)(Keep-Alive oturum [nesneleri](https://requests.readthedocs.io/en/master/user/advanced/#keep-alive)kullanırken otomatiktir).

Diğer ortamlar için, uygulamalarınızda bağlantı havuzu uygulamak için sağlayıcıyı veya sürücüye özel belgeleri gözden geçirin.

### <a name="modify-the-application-to-reuse-connections"></a>Bağlantıları yeniden kullanmak için uygulamayı değiştirme

*  Azure işlevlerinde bağlantıları yönetmeye ilişkin ek işaretçiler ve örnekler için Azure [İşlevlerinde Bağlantıları Yönet'i gözden geçirin.](https://docs.microsoft.com/azure/azure-functions/manage-connections)

### <a name="modify-the-application-to-use-less-aggressive-retry-logic"></a>Daha az agresif yeniden deneme mantığını kullanmak için uygulamayı değiştirin

* Ek kılavuz ve örnekler için Yeniden Deneme deseni gözden [geçirin.](https://docs.microsoft.com/azure/architecture/patterns/retry)

### <a name="use-keepalives-to-reset-the-outbound-idle-timeout"></a>Giden boşta zaman azamanı sıfırlamak için keepalives kullanın

* Node.js uygulamaları için keepalives uygulamak için, benim düğüm uygulaması gözden [aşırı giden aramalar yapıyor](https://docs.microsoft.com/azure/app-service/app-service-web-nodejs-best-practices-and-troubleshoot-guide#my-node-application-is-making-excessive-outbound-calls).

### <a name="additional-guidance-specific-to-app-service"></a>Uygulama Hizmetine özel ek kılavuz:

* Bir [yük testi](https://docs.microsoft.com/azure/devops/test/load-test/app-service-web-app-performance-test) sabit bir besleme hızında gerçek dünya verilerini simüle etmelidir. Gerçek dünya stresi altındaki uygulamaları ve işlevleri test etmek, SNAT bağlantı noktası tükenme sorunlarını önceden belirleyebilir ve çözebilir.
* Arka uç hizmetlerinin yanıtları hızla döndürebilmesini sağlayın. Azure SQL veritabanıyla ilgili performans sorunlarını giderme de, [Intelligent Insights ile Sorun Giderme Azure SQL Veritabanı performans sorunlarını gözden geçirin.](https://docs.microsoft.com/azure/sql-database/sql-database-intelligent-insights-troubleshoot-performance#recommended-troubleshooting-flow)
* Uygulama Hizmeti planını daha fazla örnekle ölçeklendirin. Ölçeklendirme hakkında daha fazla bilgi için Azure [Uygulama Hizmeti'nde bir uygulamayı ölçeklendir'e](https://docs.microsoft.com/azure/app-service/manage-scale-up)bakın. Bir uygulama hizmet planındaki her alt örnek, bir dizi SNAT bağlantı noktası olarak ayrılır. Kullanımınızı daha fazla örne yayıltırsanız, snat bağlantı noktası kullanımını örnek başına önerilen 100 giden bağlantı sınırının altında, benzersiz uzak bitiş noktası başına alabilirsiniz.
* Tek bir giden IP adresi nin tahsis edildiği ve bağlantıların ve SNAT bağlantı noktalarının sınırlarının çok daha yüksek olduğu [App Service Environment'a (ASE)](https://docs.microsoft.com/azure/app-service/environment/using-an-ase)taşınmayı düşünün.

Giden TCP sınırlarından kaçınmak, sınırların işçinizin boyutuna göre ayarlanması nedeniyle çözülmesi daha kolaydır. [Sandbox Cross VM Sayısal Limitler - TCP Bağlantıları'nda](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#cross-vm-numerical-limits) sınırları görebilirsiniz

|Sınır adı|Açıklama|Küçük (A1)|Orta (A2)|Büyük (A3)|Yalıtılmış katman (ASE)|
|---|---|---|---|---|---|
|Bağlantılar|Tüm VM'deki bağlantı sayısı|1920|3968|8064|16.000|

Giden TCP sınırlarından kaçınmak için, çalışanlarınızın boyutunu artırabilir veya yatay olarak ölçeklendirebilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme

İki tür giden bağlantı limitini ve uygulamanızın ne yaptığını bilmek, sorun gidermenizi kolaylaştırır. Uygulamanızın aynı depolama hesabına çok sayıda arama yaptığını biliyorsanız, bir SNAT sınırından şüphelenebilirsiniz. Uygulamanız internetin her yerinde uç noktalara çok sayıda çağrı oluşturuyorsa, VM sınırına ulaştığınızdan şüphelenebilirsiniz.

Uygulama davranışını nedenini hızlı bir şekilde belirlemek için yeterli bilmiyorsanız, bu belirlemede yardımcı olmak için Uygulama Hizmeti'nde bazı araçlar ve teknikler mevcuttur.

### <a name="find-snat-port-allocation-information"></a>SNAT bağlantı noktası ayırma bilgilerini bulma

SNAT bağlantı noktası tahsisi bilgilerini bulmak ve bir App Service sitesinin SNAT bağlantı noktaları ayırma ölçümünün gözlemlemek için [App Service Diagnostics'i](https://docs.microsoft.com/azure/app-service/overview-diagnostics) kullanabilirsiniz. SNAT bağlantı noktası ayırma bilgilerini bulmak için aşağıdaki adımları izleyin:

1. Uygulama Hizmeti tanılamalarına erişmek için [Azure portalındaki](https://portal.azure.com/)Uygulama Hizmeti web uygulamanıza veya Uygulama Hizmeti Ortamınıza gidin. Sol gezintide **Tanıla'yı seçin ve sorunları çözün.**
2. Kullanılabilirlik ve Performans Kategorilerini Seçin
3. Kategori altındaki kullanılabilir kutucuklar listesinde SNAT Bağlantı Noktası Tükenme kutucuğu'nu seçin. Uygulama 128 altında tutmaktır.
İhtiyacınız varsa, yine de bir destek bileti açabilirsiniz ve destek mühendisi sizin için arka uçtan metrik alırsınız.

SNAT bağlantı noktası kullanımı bir metrik olarak kullanılamadığından, SNAT bağlantı noktası kullanımına göre otomatik ölçeklendirme nin veya SNAT bağlantı noktası ayırma ölçümüne göre otomatik ölçeklendirmenin mümkün olmadığını unutmayın.

### <a name="tcp-connections-and-snat-ports"></a>TCP Bağlantıları ve SNAT Bağlantı Noktaları

TCP bağlantıları ve SNAT bağlantı noktaları doğrudan ilişkili değildir. Herhangi bir Uygulama Hizmeti sitesinin Sorunları Tanıla ve Çöze'ye Bir TCP bağlantıları kullanım dedektörü dahildir. Bulmak için "TCP bağlantıları" ifadesini arayın.

* SNAT Bağlantı Noktaları yalnızca dış ağ akışları için kullanılırken, toplam TCP Bağlantısı yerel geri dönüş bağlantılarını içerir.
* Akışlar protokol, IP adresi veya bağlantı noktası farklıysa, SNAT bağlantı noktası farklı akışlarla paylaşılabilir. TCP Bağlantıları ölçümü her TCP bağlantısını sayar.
* TCP bağlantıları sınırı alt örnek düzeyinde gerçekleşir. Azure Ağı giden yük dengelemesi, SNAT bağlantı noktası sınırlaması için TCP Bağlantıları ölçümlerini kullanmaz.
* TCP bağlantı limitleri [Sandbox Cross VM Sayısal Limitler - TCP Bağlantıları'nda](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#cross-vm-numerical-limits) açıklanmıştır

|Sınır adı|Açıklama|Küçük (A1)|Orta (A2)|Büyük (A3)|Yalıtılmış katman (ASE)|
|---|---|---|---|---|---|
|Bağlantılar|Tüm VM'deki bağlantı sayısı|1920|3968|8064|16.000|

### <a name="webjobs-and-database-connections"></a>Webİşler ve Veritabanı bağlantıları
 
WebJobs'Un Azure SQL veritabanına bağlanamadığı SNAT bağlantı noktaları tükenmişse, her bir web başvuru işlemi tarafından kaç bağlantıaçıldığını gösteren bir metrik yoktur. Sorunlu WebJob'u bulmak için, durumun iyileşip iyileşmeip iyileşmedigini veya bir sorunun planlardan birinde kalıp kanını silip edinip taşımadığını görüşmek için üsteşek Sorunlu WebJob'u bulana kadar işlemi yineleyin.

### <a name="using-snat-ports-sooner"></a>SNAT bağlantı noktalarını daha erken kullanma

Kullanılan SNAT bağlantı noktalarını daha erken serbest bırakmak için azure ayarlarını değiştiremezsiniz, çünkü tüm SNAT bağlantı noktaları aşağıdaki koşullara göre serbest bırakılacak ve davranışlar tasarım gereğidir.
 
* Sunucu veya istemci FINACK gönderirse, [SNAT bağlantı noktası](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#tcp-snat-port-release) 240 saniye sonra serbest bırakılır.
* Bir RST görülürse, SNAT bağlantı noktası 15 saniye sonra serbest bırakılır.
* Boşta zaman adedine ulaşıldıysa, bağlantı noktası serbest bırakılır.
 
## <a name="additional-information"></a>Ek bilgiler

* [Uygulama Hizmeti ile SNAT](https://4lowtherabbit.github.io/blogs/2019/10/SNAT/)
* [Azure Uygulama Hizmeti'nde yavaş uygulama performansı sorunlarını giderme](https://docs.microsoft.com/azure/app-service/troubleshoot-performance-degradation)
