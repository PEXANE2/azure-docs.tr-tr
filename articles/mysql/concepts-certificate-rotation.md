---
title: MySQL için Azure veritabanı için sertifika döndürme
description: MySQL için Azure veritabanı 'nı etkileyecek kök sertifika değişikliklerinin yakında değişiklikler hakkında bilgi edinin
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 09/02/2020
ms.openlocfilehash: 7e78c0d2538dbb6326df30d63ec1c38de48b62e6
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90057281"
---
# <a name="understanding-the-changes-in-the-root-ca-change-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'ndaki kök CA 'daki değişiklikleri anlama

MySQL için Azure veritabanı, [veritabanı sunucusuna bağlanmak](concepts-connectivity-architecture.md)için kullanılan, SSL ile etkinleştirilen istemci uygulaması/sürücüsü için kök sertifikayı değiştiriyor. Şu anda kullanılabilir kök sertifika, standart bakım ve güvenlik en iyi uygulamalarının bir parçası olarak 26 Ekim 2020 (10/26/2020) tarihinde dolacak şekilde ayarlanmıştır. Bu makale, yaklaşan değişiklikler hakkında daha fazla ayrıntıyı, etkilenecek kaynakları ve uygulamanızın veritabanı sunucunuza bağlantıyı korumasından emin olmak için gerekli adımları sağlar.

## <a name="what-update-is-going-to-happen"></a>Hangi güncelleştirme gerçekleşecektir?

Bazı durumlarda, uygulamalar güvenli bir şekilde bağlanmak için güvenilir bir sertifika yetkilisi (CA) sertifika dosyasından oluşturulan yerel bir sertifika dosyası kullanır. Şu anda müşteriler, [burada](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem)yer alan bir MySQL Için Azure veritabanı sunucusuna bağlanmak üzere önceden tanımlanmış sertifikayı kullanabilir. Bununla birlikte, [sertifika yetkilisi (CA) tarayıcı Forumu](https://cabforum.org/),   CA satıcıları tarafından verilen birden çok sertifikanın uyumsuz olması için en son yayımlanmış raporlar.

Sektörün uyumluluk gereksinimlerine göre, CA satıcıları uyumlu olmayan CA 'Lar için CA sertifikalarını iptal etmeyi, sunucuların uyumlu CA 'lar tarafından verilen sertifikaları kullanmalarını ve bu uyumlu CA 'lardan CA sertifikaları kullanmasını gerektirir. MySQL için Azure veritabanı şu anda bu uyumlu olmayan sertifikalardan birini kullandığından, istemci uygulamalarının SSL bağlantılarını doğrulamak için kullandığı, MySQL sunucularınızın olası etkisini en aza indirmek için uygun eylemlerin (aşağıda açıklanmıştır) yapıldığından emin olmamız gerekir.

Yeni sertifika 26 Ekim 2020 (10/26/2020) tarihinden itibaren kullanılacaktır. Bir MySQL istemcisinden bağlanırken (sslmode = Verify-CA veya sslmode = Verify-Full), CA doğrulaması ya da sunucu sertifikasının tam doğrulamasını kullanırsanız, uygulama yapılandırmanızı 26 Ekim 2020 (10/26/2020) tarihinden önce güncelleştirmeniz gerekir.

## <a name="how-do-i-know-if-my-database-is-going-to-be-affected"></a>Nasıl yaparım? Veritabanımın etkilenip etkilenmediğini öğrensin mi?

SSL/TLS kullanan tüm uygulamalar ve kök sertifikayı, MySQL için Azure veritabanı 'na bağlanmak üzere kök sertifikayı güncelleştirme ihtiyacı vardır. Şu anda SSL/TLS kullanmıyorsanız, uygulamanızın kullanılabilirliğine hiçbir etkisi yoktur. İstemci uygulamanızın önceden tanımlanmış güvenilir sertifika yetkilisi ( [CA) Ile](concepts-ssl-connection-security.md#ssl-default-settings)SSL modunu kullanmaya çalıştığını doğrulayabilirsiniz.

Sertifikaların beklenmedik şekilde iptal edildiği veya bir sertifikayı güncelleştirme, iptal edilmiş olması nedeniyle uygulamanızın kullanılabilirliğinin kesintiye uğramasını önlemek için, [**"bağlantıyı sürdürmek Için ne yapmam gerekir"**](concepts-certificate-rotation.md#what-do-i-need-to-do-to-maintain-connectivity) bölümüne bakın.

## <a name="what-do-i-need-to-do-to-maintain-connectivity"></a>Bağlantıyı sürdürmek için ne yapmam gerekir?

Sertifikaların beklenmedik şekilde iptal edildiği veya bir sertifikayı güncelleştirme, iptal edilmiş olması nedeniyle uygulamanızın kullanılabilirliğinin kesintiye uğramasını önlemek için aşağıdaki adımları izleyin:

*   Aşağıdaki bağlantılardan BaltimoreCyberTrustRoot & DigiCertGlobalRootG2 kök CA 'sını indirin:
    *   https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem
    *   https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem

*   Hem **Baltimorecyıbertrustroot** hem de **DigiCertGlobalRootG2** sertifikaları dahil olmak üzere bir birleştirilmiş CA sertifika deposu oluşturun.
    *   Java (MySQL Connector/J) kullanıcıları için şunu yürütün:

          ```azurecli-interactive
          keytool -importcert -alias MySQLServerCACert  -file D:\BaltimoreCyberTrustRoot.crt.pem  -keystore truststore -storepass password -noprompt
          ```

          ```azurecli-interactive
          keytool -importcert -alias MySQLServerCACert2  -file D:\DigiCertGlobalRootG2.crt.pem -keystore truststore -storepass password  -noprompt
          ```

          Ardından orijinal anahtar deposu dosyasını yeni oluşturulan yeni bir anahtar ile değiştirin:
        *   System. setProperty ("javax. net. SSL. trustStore", "path_to_truststore_file"); 
        *   System. setProperty ("javax. net. SSL. trustStorePassword", "Password");

    *   .NET (MySQL Connector/NET, MySQLConnector) kullanıcıları için, Windows sertifika deposunda, güvenilen kök sertifika yetkililerinde **Baltimorecyıbertrustroot** ve **DigiCertGlobalRootG2** 'ın aynı olduğundan emin olun. Herhangi bir sertifika yoksa, eksik sertifikayı içeri aktarın.

        ![MySQL için Azure veritabanı .net sertifikası](media/overview/netconnecter-cert.png)

    *   Linux üzerinde SSL_CERT_DIR kullanarak .NET kullanıcıları için, SSL_CERT_DIR tarafından belirtilen dizinde **Baltimorecrivbertrustroot** ve **DigiCertGlobalRootG2** 'ın aynı olduğundan emin olun. Herhangi bir sertifika yoksa, eksik sertifika dosyasını oluşturun.

    *   Diğer (MySQL Client/MySQL çalışma ekranı/C/C++/Go/Python/Ruby/PHP/NodeJS/Perl/Swift) kullanıcıları için, aşağıdaki biçimde iki CA sertifika dosyasını birleştirebilirsiniz</b>

        </br>-----BAŞLANGıÇ SERTIFIKASı-----
 </br>(Root CA1: BaltimoreCyberTrustRoot. CRT. pek)
 </br>-----SON SERTIFIKA-----
 </br>-----BAŞLANGıÇ SERTIFIKASı-----
 </br>(Root CA2: DigiCertGlobalRootG2. CRT. pek)
 </br>-----SON SERTIFIKA-----

*   Özgün kök CA ped dosyasını birleştirilmiş kök CA dosyası ile değiştirin ve uygulamanızı/istemcinizi yeniden başlatın.
*   Daha sonra, yeni sertifika sunucu tarafında dağıtıldıktan sonra, CA PEI dosyanızı DigiCertGlobalRootG2. CRT. ped olarak değiştirebilirsiniz.

## <a name="what-can-be-the-impact"></a>Ne etkisi olabilir?
Burada belgelenen MySQL için Azure veritabanı sertifikası ' nı kullanıyorsanız, veritabanına ulaşılamamasından bu yana uygulamanızın kullanılabilirliği kesintiye uğramış olabilir. Uygulamanıza bağlı olarak, aşağıdakiler dahil ancak bunlarla sınırlı olmamak üzere çeşitli hata iletileri alabilirsiniz:
*   Geçersiz sertifika/iptal edilmiş sertifika
*   Bağlantı zaman aşımına uğradı
*   Geçerliyse hata

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="1-if-i-am-not-using-ssltls-do-i-still-need-to-update-the-root-ca"></a>1. SSL/TLS kullanmıyorum, yine de kök CA 'yı güncelleştirmem gerekir mi?
SSL/TLS kullanmıyorsanız hiçbir eylem gerekmez. 

### <a name="2-if-i-am-using-ssltls-do-i-need-to-restart-my-database-server-to-update-the-root-ca"></a>2. SSL/TLS kullanıyorum, kök CA 'yı güncelleştirmek için veritabanı sunucusunu yeniden başlatmem gerekir mi?
Hayır, yeni sertifikayı kullanmaya başlamak için veritabanı sunucusunu yeniden başlatmanız gerekmez. Bu kök sertifika, istemci tarafı bir değişiklik ve gelen istemci bağlantılarının veritabanı sunucusuna bağlanabildiklerinden emin olmak için yeni sertifikayı kullanması gerekir.

### <a name="3-what-will-happen-if-i-do-not-update-the-root-certificate-before-october-26-2020-10262020"></a>3. kök sertifikayı 26 Ekim 2020 (10/26/2020) tarihinden önce güncelleştirdiğimde ne olur?
Kök sertifikayı 30 Kasım 2020 tarihinden önce güncelleştirmediğinizi, SSL/TLS aracılığıyla bağlanan ve kök sertifika için doğrulama yapan uygulamalarınız MySQL veritabanı sunucusuyla iletişim kuramayacaktır ve uygulama MySQL veritabanı sunucunuza bağlantı sorunlarıyla karşılaşacaktır.

### <a name="4-do-i-need-to-plan-a-maintenance-downtime-for-this-changebr"></a>4. bu değişiklik için bir bakım kapalı kalma süresi planlamanız gerekir mi?<BR>
Hayır. Buradaki değişiklik yalnızca istemci tarafında, veritabanı sunucusuna bağlanmak için olduğundan, bu değişiklik için burada gereken bakım kapalı kalma süresi yoktur.

### <a name="5--what-if-i-cannot-get-a-scheduled-downtime-for-this-change-before-october-26-2020-10262020"></a>5. bu değişiklik için 26 Ekim 10/26/2020 2020 ' den önce zamanlanan kapalı kalma süresi alamazsanız ne olur?
Sunucuya bağlanmak için kullanılan istemcilerin [buradaki](./concepts-certificate-rotation.md#what-do-i-need-to-do-to-maintain-connectivity)çözüm bölümünde açıklandığı gibi sertifika bilgilerini güncelleştirmesi gerektiğinden, bu durumda sunucu için kapalı kalma süresine gerek kalmaz.

###  <a name="6-if-i-create-a-new-server-after-nov-30-2020-will-i-be-impacted"></a>6.30 Kasım 2020 ' den sonra yeni bir sunucu oluştururum, bundan etkilenmem gerekir mi?
26 Ekim 2020 (10/26/2020) sonrasında oluşturulan sunucular için, SSL kullanarak bağlanmak üzere uygulamalarınız için yeni verilen sertifikayı kullanabilirsiniz.

### <a name="7-how-often-does-microsoft-update-their-certificates-or-what-is-the-expiry-policy"></a>7. Microsoft sertifikalarını ne sıklıkla güncelleştiriyor veya süre sonu ilkesi nedir?
MySQL için Azure veritabanı tarafından kullanılan bu sertifikalar, güvenilen sertifika yetkilileri (CA) tarafından sağlanır. Bu nedenle, MySQL için Azure veritabanı 'nda bu sertifikaların desteklenmesi, CA tarafından bu sertifikaların desteğine bağlıdır. Bununla birlikte, bu örnekte olduğu gibi, önceden tanımlanmış bu sertifikalarda, en erken düzeltilmelidir.

### <a name="8-if-i-am-using-read-replicas-do-i-need-to-perform-this-update-only-on-master-server-or-all-the-read-replicas"></a>8. okuma çoğaltmaları kullanıyorum, bu güncelleştirmeyi yalnızca ana sunucuda mi yoksa tüm okuma çoğaltmalarıyla mi gerçekleştirmem gerekir?
Bu güncelleştirme bir istemci tarafı değişikliği olduğundan, istemci Çoğaltma sunucusundan veri okumak için kullanılıyorsa, bu istemciler için değişiklikleri de uygulamamız gerekecektir. 

### <a name="9-do-we-have-server-side-query-to-verify-if-ssl-is-being-used"></a>9. SSL 'nin kullanıldığını doğrulamak için sunucu tarafı sorgumuz var mı?
Sunucuya bağlanmak için SSL bağlantısı kullanıp kullandığınızı doğrulamak için [SSL doğrulaması](howto-configure-ssl.md#step-4-verify-the-ssl-connection)' na başvurun.

### <a name="10-what-if-i-have-further-questions"></a>10. daha fazla sorunuz varsa ne yapmalıyım?
Sorularınız varsa, [Microsoft Q&A](mailto:AzureDatabaseforMySQL@service.microsoft.com)'daki topluluk uzmanlarının yanıtlarını alın. Destek planınız varsa ve teknik yardıma ihtiyacınız varsa  [bizimle iletişime geçin](mailto:AzureDatabaseforMySQL@service.microsoft.com)