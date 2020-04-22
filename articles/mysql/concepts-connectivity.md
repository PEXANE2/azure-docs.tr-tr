---
title: Geçici bağlantı hataları - MySQL için Azure Veritabanı
description: Geçici bağlantı hatalarını nasıl işleyeceğinizi ve MySQL için Azure Veritabanı'na verimli bir şekilde nasıl bağlanılacağınızı öğrenin.
keywords: mysql bağlantısı,bağlantı dizesi,bağlantı sorunları,geçici hata,bağlantı hatası,verimli bağlanma
author: jasonwhowell
ms.author: jasonh
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 4f9101b4108f5512ee9779f4633845b34fdfad5a
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81767867"
---
# <a name="handle-transient-errors-and-connect-efficiently-to-azure-database-for-mysql"></a>Geçici hataları işleme ve MySQL için Azure Veritabanı'na verimli bir şekilde bağlanma

Bu makalede, geçici hataların nasıl işleyeceğiniz ve MySQL için Azure Veritabanı'na verimli bir şekilde nasıl bağlanılacağı açıklanmaktadır.

## <a name="transient-errors"></a>Geçici hatalar

Geçici hata olarak da bilinen geçici hata, kendi kendini çözecek bir hatadır. Genellikle bu hatalar bırakılan veritabanı sunucusuna bir bağlantı olarak tezahür eder. Ayrıca bir sunucuya yeni bağlantılar açılamaz. Geçici hatalar örneğin donanım veya ağ hatası oluştuğunda oluşabilir. Başka bir nedeni dışarı haddelenmiş olan bir PaaS hizmetinin yeni bir sürümü olabilir. Bu olayların çoğu sistem tarafından 60 saniyeden kısa bir sürede otomatik olarak azaltılır. Bulutta uygulama tasarlamak ve geliştirmek için en iyi uygulama geçici hatalar beklemektir. Herhangi bir bileşende herhangi bir zamanda gerçekleşebileceğini ve bu durumları işlemek için uygun mantığa sahip olabileceklerini varsayalım.

## <a name="handling-transient-errors"></a>Geçici hataları işleme

Geçici hatalar yeniden deneme mantığı kullanılarak ele alınmalıdır. Göz önünde bulundurulması gereken durumlar:

* Bağlantıyı açmaya çalıştığınızda bir hata oluşur
* Sunucu tarafına boşta bağlantı bırakılır. Bir komut vermeye çalıştığınızda yürütülemez
* Şu anda bir komutuyguluyor etkin bir bağlantı bırakılır.

Birinci ve ikinci durumda oldukça düz ele almak için vardır. Bağlantıyı yeniden açmaya çalışın. Başarılı olduğunuzda, geçici hata sistem tarafından azaltıldı. MySQL için Azure Veritabanınızı yeniden kullanabilirsiniz. Bağlantıyı yeniden denemeden önce beklemenizi öneririz. İlk yeniden denemeler başarısız olursa geri çek. Bu şekilde sistem hata durumunu aşmak için kullanılabilir tüm kaynakları kullanabilirsiniz. Takip etmek için iyi bir desen:

* İlk denemenizden önce 5 saniye bekleyin.
* Her bir yeniden deneme için, 60 saniyeye kadar bekleme yi katlanarak artırır.
* Uygulamanızın işlemin başarısız olduğunu düşündüğü en yüksek yeniden deneme sayısını ayarlayın.

Etkin bir işlemle bağlantı başarısız olduğunda, kurtarmaişlemini doğru şekilde işlemek daha zordur. İki durum vardır: Hareket yalnızca salt okunursa, bağlantıyı yeniden açmak ve hareketi yeniden denemek güvenlidir. Ancak hareket veritabanına da yazıyorsa, hareketin geri alınıp alınıp alınıp alınıp alınmayacağına veya geçici hata gerçekleşmeden önce başarılı olup olmadığını belirlemeniz gerekir. Bu durumda, veritabanı sunucusundan taahhüt bildirimi ni almamış olabilirsiniz.

Bunu yapmanın bir yolu, tüm yeniden denemeler için kullanılan istemci üzerinde benzersiz bir kimlik oluşturmaktır. Bu benzersiz kimliği işlemin bir parçası olarak sunucuya aktarın ve benzersiz bir kısıtlaması olan bir sütunda saklarsınız. Bu şekilde hareketi güvenli bir şekilde yeniden deneyebilirsiniz. Önceki işlem geri alındıysa ve istemci tarafından oluşturulan benzersiz kimlik sistemde henüz yoksa başarılı olur. Önceki işlem başarıyla tamamlandığı için, benzersiz kimlik daha önce depolanmışsa yinelenen anahtar ihlalini gösteren başarısız olur.

Programınız MySQL için Azure Veritabanı ile üçüncü taraf ara yazılımlar aracılığıyla iletişim kurduğunda, satıcıya ara yazılımın geçici hatalar için yeniden deneme mantığı içerip içermediğini sorun.

Mantığınızı yeniden denediğinizi test ettiğinizden emin olun. Örneğin, MySQL sunucusu için Azure Veritabanınızın bilgi işlem kaynaklarını ölçeklerken veya küçülterek kodunuzu yürütmeyi deneyin. Uygulamanız, bu işlem sırasında karşılaşılan kısa kapalı kalma süresini sorunsuz bir şekilde ele almalıdır.

## <a name="connect-efficiently-to-azure-database-for-mysql"></a>MySQL için Azure Veritabanına verimli bir şekilde bağlanma

Veritabanı bağlantıları sınırlı bir kaynaktır, bu nedenle MySQL için Azure Veritabanı'na erişmek için bağlantı havuzundan etkin bir şekilde yararlanmak performansı en iyi duruma getirir. Aşağıdaki bölümde MySQL için Azure Veritabanı'na daha etkili bir şekilde erişmek için bağlantı havuzu veya kalıcı bağlantıların nasıl kullanılacağı açıklanmaktadır.

## <a name="access-databases-by-using-connection-pooling-recommended"></a>Bağlantı havuzu kullanarak veritabanlarına erişin (önerilir)

Veritabanı bağlantılarını yönetmek, uygulamanın bir bütün olarak performansı üzerinde önemli bir etkiye sahip olabilir. Uygulamanızın performansını optimize etmek için, amaç bağlantıların oluşturulan sayısını ve anahtar kod yollarında bağlantı kurma süresini azaltmak olmalıdır. MySQL için Azure Veritabanı'na bağlanmak için veritabanı bağlantısı havuzu veya kalıcı bağlantıları kullanmanızı şiddetle öneririz. Veritabanı bağlantısı havuzu, veritabanı bağlantılarının oluşturulmasını, yönetimini ve tahsisini işler. Bir program veritabanı bağlantısı istediğinde, yeni bir bağlantı oluşturmak yerine varolan boşta veritabanı bağlantılarının tahsisine öncelik verir. Program veritabanı bağlantısını kullanarak tamamlandıktan sonra, bağlantı yalnızca kapatılmadan daha fazla kullanıma hazırlık olarak kurtarılır.

Daha iyi bir illüstrasyon için, bu makalede örnek olarak JAVA kullanan [örnek kod bir parça](./sample-scripts-java-connection-pooling.md) sağlar. Daha fazla bilgi için [Apache ortak DBCP'ye](https://commons.apache.org/proper/commons-dbcp/)bakın.

> [!NOTE]
> Sunucu, kaynakları boşaltmak için bir süredir boşta olan bir bağlantıyı kapatmak için bir zaman aramekanizması yapılandırır. Kalıcı bağlantılarıkullanırken etkinliğini sağlamak için doğrulama sistemini kurduğunuzdan emin olun. Daha fazla bilgi için, [kalıcı bağlantıların etkinliğini sağlamak için istemci tarafındaki doğrulama sistemlerini yapılandırın.](concepts-connectivity.md#configure-verification-mechanisms-in-clients-to-confirm-the-effectiveness-of-persistent-connections)

## <a name="access-databases-by-using-persistent-connections-recommended"></a>Kalıcı bağlantıları kullanarak veritabanlarına erişin (önerilir)

Kalıcı bağlantı kavramı, bağlantı birleştirme kavramına benzer. Kısa bağlantıları kalıcı bağlantılarla değiştirmek için kodda yalnızca küçük değişiklikler yapmak gerektir, ancak birçok tipik uygulama senaryosunda performansı artırma açısından önemli bir etkisi vardır.

## <a name="access-databases-by-using-wait-and-retry-mechanism-with-short-connections"></a>Kısa bağlantılarla bekleme ve yeniden deneme mekanizmasını kullanarak veritabanlarına erişin

Kaynak sınırlamalarınız varsa, veritabanlarına erişmek için veritabanı havuzu veya kalıcı bağlantıları kullanmanızı şiddetle öneririz. Uygulamanız eşzamanlı bağlantı sayısındaki üst sınıra yaklaştığınızda kısa bağlantılar kullanıyorsa ve bağlantı hataları yla karşılaşırsanız, bekleme ve yeniden deneme mekanizmasını deneyebilirsiniz. İlk denemeden sonra daha kısa bir bekleme süresiyle uygun bir bekleme süresi ayarlayabilirsiniz. Bundan sonra, olayları birden çok kez beklemeyi deneyebilirsiniz.

## <a name="configure-verification-mechanisms-in-clients-to-confirm-the-effectiveness-of-persistent-connections"></a>Kalıcı bağlantıların etkinliğini onaylamak için istemcilerde doğrulama mekanizmalarını yapılandırın

Sunucu, kaynakları boşaltmak için bir süredir boşta olan bir bağlantıyı kapatmak için bir zaman aramekanizması yapılandırır. İstemci veritabanına yeniden eriştiğinde, istemci ve sunucu arasında yeni bir bağlantı isteği oluşturmaya eşdeğerdir. Bağlantıların kullanımı sırasında ki etkinliğini sağlamak için istemci üzerinde bir doğrulama mekanizması yapılandırın. Aşağıdaki örnekte gösterildiği gibi, bu doğrulama mekanizmasını yapılandırmak için Tomcat JDBC bağlantı havuzunu kullanabilirsiniz.

TestOnBorrow parametresini ayarlayarak, yeni bir istek olduğunda, bağlantı havuzu kullanılabilir boşta bağlantıların etkinliğini otomatik olarak doğrular. Böyle bir bağlantı etkili olursa, doğrudan döndürülen bağlantı havuzu bağlantıyı geri çeker. Bağlantı havuzu daha sonra yeni bir etkili bağlantı oluşturur ve döndürür. Bu işlem, veritabanına verimli bir şekilde erişilmesini sağlar. 

Belirli ayarlar hakkında bilgi için [JDBC bağlantı havuzu resmi giriş belgesine](https://tomcat.apache.org/tomcat-7.0-doc/jdbc-pool.html#Common_Attributes)bakın. Esas olarak aşağıdaki üç parametreyi ayarlamanız gerekir: TestOnBorrow (doğru olarak ayarlanmış), ValidationQuery (SELECT 1'e ayarlanmıştır) ve ValidationQueryTimeout (1 olarak ayarlanır). Belirli örnek kod aşağıda gösterilmiştir:

```java
public class SimpleTestOnBorrowExample {
      public static void main(String[] args) throws Exception {
          PoolProperties p = new PoolProperties();
          p.setUrl("jdbc:mysql://localhost:3306/mysql");
          p.setDriverClassName("com.mysql.jdbc.Driver");
          p.setUsername("root");
          p.setPassword("password");
            // The indication of whether objects will be validated by the idle object evictor (if any). 
            // If an object fails to validate, it will be dropped from the pool. 
            // NOTE - for a true value to have any effect, the validationQuery or validatorClassName parameter must be set to a non-null string. 
          p.setTestOnBorrow(true); 

            // The SQL query that will be used to validate connections from this pool before returning them to the caller.
            // If specified, this query does not have to return any data, it just can't throw a SQLException.
          p.setValidationQuery("SELECT 1");

            // The timeout in seconds before a connection validation queries fail. 
            // This works by calling java.sql.Statement.setQueryTimeout(seconds) on the statement that executes the validationQuery. 
            // The pool itself doesn't timeout the query, it is still up to the JDBC driver to enforce query timeouts. 
            // A value less than or equal to zero will disable this feature.
          p.setValidationQueryTimeout(1);
            // set other useful pool properties.
          DataSource datasource = new DataSource();
          datasource.setPoolProperties(p);

          Connection con = null;
          try {
            con = datasource.getConnection();
            // execute your query here
          } finally {
            if (con!=null) try {con.close();}catch (Exception ignore) {}
          }
      }
  }
```

## <a name="next-steps"></a>Sonraki adımlar

* [MySQL için Azure Veritabanı bağlantı sorunlarını giderme](howto-troubleshoot-common-connection-issues.md)
