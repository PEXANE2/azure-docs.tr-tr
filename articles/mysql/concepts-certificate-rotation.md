---
title: MySQL için Azure veritabanı için sertifika döndürme
description: MySQL için Azure veritabanı 'nı etkileyecek kök sertifika değişikliklerinin yakında değişiklikler hakkında bilgi edinin
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: conceptual
ms.date: 01/18/2021
ms.openlocfilehash: a80910340333c2e6c52bb32644c48fa3a28ab437
ms.sourcegitcommit: c3739cb161a6f39a9c3d1666ba5ee946e62a7ac3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2021
ms.locfileid: "107210763"
---
# <a name="understanding-the-changes-in-the-root-ca-change-for-azure-database-for-mysql-single-server"></a>MySQL için Azure veritabanı tek sunucu için kök CA değişiklik değişikliklerini anlama

MySQL için Azure veritabanı tek sunucu, standart bakım ve güvenlik en iyi uygulamalarının bir parçası olarak **15 şubat 2021 (02/15/2021)** tarihinde kök sertifika değişikliğini başarıyla tamamladı. Bu makalede, değişiklikler hakkında daha fazla ayrıntı, etkilenen kaynaklar ve uygulamanızın veritabanı sunucunuza bağlantıyı korumasından emin olmak için gereken adımlar verilmektedir.

> [!NOTE]
> Bu makale yalnızca [MySQL Için Azure veritabanı-tek sunucu](single-server-overview.md) için geçerlidir. [MySQL Için Azure veritabanı-esnek sunucu](flexible-server/overview.md), SSL üzerinden iletişim kurmak için gereken sertifika, [DigiCert genel kök CA](https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem) 'dır
> 
> Bu makale, Microsoft 'un artık kullandığı bir terim olan _bağımlı_ dönem başvuruları içerir. Terim yazılımlardan kaldırıldığında, bu makaleden kaldıracağız.
>

## <a name="why-root-certificate-update-is-required"></a>Kök sertifika güncelleştirmesi neden gereklidir?

MySQL için Azure veritabanı kullanıcıları, [burada](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem)bulunan MySQL sunucusuna bağlanmak için yalnızca önceden tanımlanmış sertifikayı kullanabilir. Bununla birlikte, [sertifika yetkilisi (CA) tarayıcı Forumu](https://cabforum.org/),   CA satıcıları tarafından verilen birden çok sertifikanın uyumsuz olması için en son yayımlanmış raporlar.

Sektörün uyumluluk gereksinimlerine göre, CA satıcıları uyumlu olmayan CA 'Lar için CA sertifikalarını iptal etmeyi, sunucuların uyumlu CA 'lar tarafından verilen sertifikaları kullanmalarını ve bu uyumlu CA 'lardan CA sertifikaları kullanmasını gerektirir. MySQL için Azure veritabanı, bu uyumlu olmayan sertifikalardan birini kullandığından, MySQL sunucularınızda olası tehdidi en aza indirmek için sertifikayı uyumlu sürüme döndürmemiz gerekiyordu.

Yeni sertifika alınır ve 15 Şubat 2021 (02/15/2021) tarihinden itibaren geçerli olur. 

## <a name="what-change-was-performed-on-february-15-2021-02152021"></a>15 Şubat 2021 ' de hangi değişiklik yapıldı (02/15/2021)?

15 Şubat 2021 ' de, [baltimorecerbertrustroot kök](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) sertifikası aynı [baltimorecerbertrustroot kök sertifikası](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) ile değiştirilmiştir ve bu da mevcut müşterilerin sunucu bağlantılarının hiçbir etkisi olmadığından emin olun.  Bu değişiklik sırasında, [Baltimorecyıbertrustroot kök sertifikası](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) [DigiCertGlobalRootG2](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) ile **değiştirilmez** ve bu değişiklik, müşterilerin değişikliği yapmasına daha fazla zaman tanımak için ertelenir.

## <a name="do-i-need-to-make-any-changes-on-my-client-to-maintain-connectivity"></a>Bağlantıyı sürdürmek için istemcimde herhangi bir değişiklik yapmam gerekiyor mu?

İstemci tarafında bir değişiklik yapılması gerekmez. Aşağıdaki önceki önerimizi izlediyseniz, **Baltimorecyıbertrustroot sertifikası** birleştirilmiş CA sertifikasından kaldırılmadığı sürece yine de bağlanmaya devam edersiniz. **Bağlantıyı sürdürmeye yönelik daha fazla bildirimde bulununcaya kadar, birleştirilmiş CA sertifikanızdan Baltimorecyıbertrustroot 'yi kaldırmadık.**

### <a name="previous-recommendation"></a>Önceki öneri

Sertifikaların beklenmedik şekilde iptal edildiği veya iptal edilen bir sertifikayı güncelleştirme nedeniyle kesintiye uğramasını önlemek için aşağıdaki adımları kullanın. Bu düşünce, geçerli sertifikayı ve yeni birini birleştiren yeni bir *. pem* dosyası OLUŞTURMAKTıR ve SSL sertifika doğrulaması sırasında izin verilen değerlerden biri kullanılacaktır. Aşağıdaki adımlara bakın:

* Aşağıdaki bağlantılardan BaltimoreCyberTrustRoot & DigiCertGlobalRootG2 kök CA 'sını indirin:

  * [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem)
  * [https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem)

* Hem **Baltimorecyıbertrustroot** hem de **DigiCertGlobalRootG2** sertifikaları dahil olmak üzere bir birleştirilmiş CA sertifika deposu oluşturun.

  * Java (MySQL Connector/J) kullanıcıları için şunu yürütün:

    ```console
    keytool -importcert -alias MySQLServerCACert -file D:\BaltimoreCyberTrustRoot.crt.pem -keystore truststore -storepass password -noprompt
    ```

    ```console
    keytool -importcert -alias MySQLServerCACert2 -file D:\DigiCertGlobalRootG2.crt.pem -keystore truststore -storepass password -noprompt
    ```

    Ardından orijinal anahtar deposu dosyasını yeni oluşturulan yeni bir anahtar ile değiştirin:

    * System. setProperty ("javax. net. SSL. trustStore", "path_to_truststore_file");
    * System. setProperty ("javax. net. SSL. trustStorePassword", "Password");

  * .NET (MySQL Connector/NET, MySQLConnector) kullanıcıları için, Windows sertifika deposunda, güvenilen kök sertifika yetkililerinde **Baltimorecyıbertrustroot** ve **DigiCertGlobalRootG2** 'ın aynı olduğundan emin olun. Herhangi bir sertifika yoksa, eksik sertifikayı içeri aktarın.

    :::image type="content" source="media/overview/netconnecter-cert.png" alt-text="MySQL için Azure veritabanı .net sertifika diyagramı":::

  * Linux üzerinde SSL_CERT_DIR kullanarak .NET kullanıcıları için, SSL_CERT_DIR tarafından belirtilen dizinde **Baltimorecrivbertrustroot** ve **DigiCertGlobalRootG2** 'ın aynı olduğundan emin olun. Herhangi bir sertifika yoksa, eksik sertifika dosyasını oluşturun.

  * Diğer (MySQL Client/MySQL çalışma ekranı/C/C++/Go/Python/Ruby/PHP/NodeJS/Perl/Swift) kullanıcıları için iki CA sertifika dosyasını aşağıdaki biçimde birleştirebilirsiniz:

      ```
      -----BEGIN CERTIFICATE-----
      (Root CA1: BaltimoreCyberTrustRoot.crt.pem)
      -----END CERTIFICATE-----
      -----BEGIN CERTIFICATE-----
      (Root CA2: DigiCertGlobalRootG2.crt.pem)
      -----END CERTIFICATE-----
      ```

* Özgün kök CA ped dosyasını birleştirilmiş kök CA dosyası ile değiştirin ve uygulamanızı/istemcinizi yeniden başlatın.
* Daha sonra, yeni sertifika sunucu tarafında dağıtıldıktan sonra, CA PEI dosyanızı DigiCertGlobalRootG2. CRT. ped olarak değiştirebilirsiniz.

> [!NOTE]
> Lütfen sertifika değişikliği yapılıncaya kadar **Baltidaha fazla sertifikayı** kaldırmayın veya değiştirmeyin. Değişiklik yapıldıktan sonra, Baltidaha fazla sertifikayı bırakması için güvenli olacak şekilde bir iletişim göndereceğiz. 

## <a name="why-was-baltimorecybertrustroot-certificate-not-replaced-to-digicertglobalrootg2-during-this-change-on-february-15-2021"></a>15 Şubat 2021 ' de bu değişiklik sırasında Baltimorecyıbertrustroot sertifikası neden DigiCertGlobalRootG2 ile değiştirilmedi?

Bu değişiklik için müşteri hazırlığını değerlendirdik ve çok sayıda müşteri bu değişikliği yönetmek için ek sağlama süresi arıyor. Müşterilere hazırlık sağlamak için daha fazla müşteri adayı süresi sunmanın yanı sıra, müşterilere ve son kullanıcılara yeterli sağlama süresi sağlayan en az bir yılda sertifika değişikliğini DigiCertGlobalRootG2 olarak ertelemenizi karardık. 

Kullanıcılara yönelik önerilerimiz, birleştirilmiş bir sertifika oluşturmak ve sunucunuza bağlanmak için bir iletişim gönderene kadar Baltimoreconbertrustroot sertifikasını kaldırmayın. 

## <a name="what-if-we-removed-the-baltimorecybertrustroot-certificate"></a>Baltimorecyıbertrustroot sertifikasını kaldırdık ne olursa?

MySQL için Azure veritabanı sunucusuna bağlanırken hatalarla bağlantı kurma başına başlayacaksınız. Bağlantıyı sürdürmek için [Baltimorecyıbertrustroot](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) SERTIFIKASı ile [SSL 'yi yapılandırmanız](howto-configure-ssl.md) gerekir.


## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="1-if-im-not-using-ssltls-do-i-still-need-to-update-the-root-ca"></a>1. SSL/TLS kullanmıyorsanız yine de kök CA 'yı güncelleştirmem gerekir mi?

  SSL/TLS kullanmıyorsanız hiçbir eylem gerekmez.

### <a name="2-if-im-using-ssltls-do-i-need-to-restart-my-database-server-to-update-the-root-ca"></a>2. SSL/TLS kullanıyorum, kök CA 'yı güncelleştirmek için veritabanı sunucusunu yeniden başlatmem gerekir mi?

Hayır, yeni sertifikayı kullanmaya başlamak için veritabanı sunucusunu yeniden başlatmanız gerekmez. Bu kök sertifika, istemci tarafı bir değişiklik ve gelen istemci bağlantılarının veritabanı sunucusuna bağlanabildiklerinden emin olmak için yeni sertifikayı kullanması gerekir.

### <a name="3-how-do-i-know-if-im-using-ssltls-with-root-certificate-verification"></a>3. Nasıl yaparım? kök sertifika doğrulaması ile SSL/TLS kullanıp kullandığımı öğrenin.

Bağlantı dizenizi inceleyerek bağlantılarınızın kök sertifikayı doğrulayıp doğrulamayıp belirleyemeyeceğini belirleyebilirsiniz.

- Bağlantı dizeniz veya içeriyorsa `sslmode=verify-ca` `sslmode=verify-identity` , sertifikayı güncelleştirmeniz gerekir.
- Bağlantı dizeniz,, `sslmode=disable` veya içerdiğinde, `sslmode=allow` `sslmode=prefer` `sslmode=require` sertifikaları güncelleştirmeniz gerekmez.
- Bağlantı dizeniz sslmode 'u belirtmezse, sertifikaları güncelleştirmeniz gerekmez.

Bağlantı dizesini soyutlayan bir istemci kullanıyorsanız, sertifikaları doğrulayıp doğrulamadığını anlamak için istemci belgelerini gözden geçirin.

### <a name="4-what-is-the-impact-if-using-app-service-with-azure-database-for-mysql"></a>4. MySQL için Azure veritabanı ile App Service kullanılıyorsa ne etkiler?

MySQL için Azure veritabanı 'na bağlanan Azure Uygulama Hizmetleri için, uygulama ile SSL kullanma yönteminize bağlı olarak iki olası senaryo vardır.

* Bu yeni sertifika platform düzeyinde App Service eklendi. Uygulamanızdaki App Service platforma dahil olan SSL sertifikalarını kullanıyorsanız hiçbir işlem gerekmez. En yaygın senaryo budur. 
* Kodunuzda SSL sertifika dosyasının yolunu açıkça dahil ediyorsanız, yeni sertifikayı indirmeniz ve yukarıda belirtilen şekilde birleştirilmiş bir sertifika oluşturmanız ve sertifika dosyasını kullanmanız gerekir. Bu senaryonun iyi bir örneği, [App Service belgelerinde](../app-service/tutorial-multi-container-app.md#configure-database-variables-in-wordpress)paylaşıldığından App Service özel kapsayıcılar kullandığınızda oluşur. Bu, yaygın olmayan bir senaryodur ancak bunu kullanan bazı kullanıcıları gördük.

### <a name="5-what-is-the-impact-if-using-azure-kubernetes-services-aks-with-azure-database-for-mysql"></a>5. MySQL için Azure veritabanı ile Azure Kubernetes Hizmetleri (AKS) kullanılıyorsa etkisi nedir?

Azure Kubernetes Services (AKS) kullanarak MySQL için Azure veritabanı 'na bağlanmaya çalışıyorsanız, adanmış bir müşterilerin ana bilgisayar ortamından erişime de benzer. [Buradaki](../aks/ingress-own-tls.md)adımlara bakın.

### <a name="6-what-is-the-impact-if-using-azure-data-factory-to-connect-to-azure-database-for-mysql"></a>6. MySQL için Azure veritabanı 'na bağlanmak üzere Azure Data Factory kullanıyorsanız etkisi nedir?

Bağlayıcı, Azure Integration Runtime kullanan bir bağlayıcı için, Azure 'da barındırılan ortamdaki Windows sertifika deposundaki sertifikalar üzerinden faydalanır. Bu sertifikalar zaten yeni uygulanan sertifikalarla uyumludur ve bu nedenle herhangi bir eylem gerekmez.

Şirket içinde barındırılan Integration Runtime kullanan bir bağlayıcı için, bağlantı dizinizdeki SSL sertifikası dosyasının yolunu açıkça eklediğiniz yerde, [yeni sertifikayı](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) indirmeniz ve bağlantı dizesini kullanmak için güncelleştirmeniz gerekir.

### <a name="7-do-i-need-to-plan-a-database-server-maintenance-downtime-for-this-change"></a>7. bu değişiklik için bir veritabanı sunucusu bakım kapalı kalma süresi planlamanız gerekir mi?

Hayır. Buradaki değişiklik yalnızca istemci tarafında, veritabanı sunucusuna bağlanmak için olduğundan, bu değişiklik için veritabanı sunucusu için gerekli bakım kapalı kalma süresi yoktur.

### <a name="8-if-i-create-a-new-server-after-february-15-2021-02152021-will-i-be-impacted"></a>8.15 Şubat 2021 (02/15/2021) sonrasında yeni bir sunucu oluştururum, bundan etkilenecek mıyım?

15 Şubat 2021 (02/15/2021) ' den sonra oluşturulan sunucular için, uygulamalarınızın SSL kullanarak bağlanması için [Baltimorecyıbertrustroot](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) 'yi kullanmaya devam edersiniz.

### <a name="9-how-often-does-microsoft-update-their-certificates-or-what-is-the-expiry-policy"></a>9. Microsoft sertifikalarını ne sıklıkla güncelleştiriyor veya süre sonu ilkesi nedir?

MySQL için Azure veritabanı tarafından kullanılan bu sertifikalar, güvenilen sertifika yetkilileri (CA) tarafından sağlanır. Bu nedenle, bu sertifikaların desteklenmesi CA tarafından bu sertifikaların desteğine bağlıdır. [Baltimorecerbertrustroot](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) sertifikası 2025 ' de sona ermek üzere zamanlandı. bu nedenle, Microsoft 'un süre sonu öncesinde bir sertifika değişikliği gerçekleştirmesi gerekir. Ayrıca, bu önceden tanımlanmış sertifikalarda öngörülemeyen hatalar varsa, Microsoft 'un her zaman güvenli ve uyumlu olduğundan emin olmak için, 15 Şubat 2021 ' de gerçekleştirilen değişikliğe benzer şekilde Microsoft 'un sertifika döndürmesini yapması gerekir.

### <a name="10-if-im-using-read-replicas-do-i-need-to-perform-this-update-only-on-source-server-or-the-read-replicas"></a>10. okuma çoğaltmaları kullanıyorsam, bu güncelleştirmeyi yalnızca kaynak sunucuda veya okuma çoğaltmalarıyla gerçekleştirmem gerekir mi?

Bu güncelleştirme istemci tarafı değişikliği olduğundan, istemci Çoğaltma sunucusundan veri okumak için kullanılıyorsa, bu istemciler için değişiklikleri de uygulamanız gerekir.

### <a name="11-if-im-using-data-in-replication-do-i-need-to-perform-any-action"></a>11. veri çoğaltma 'yı kullanıyorsam herhangi bir eylem gerçekleştirmem gerekir mi?

MySQL için Azure veritabanı 'na bağlanmak üzere [veri çoğaltma](concepts-data-in-replication.md) 'yı kullanıyorsanız, göz önünde bulundurmanız gereken iki şey vardır:

* Veri çoğaltma bir sanal makineden (Şirket içi veya Azure sanal makinesi) MySQL için Azure veritabanı 'na ait ise, çoğaltmayı oluşturmak için SSL 'nin kullanıldığını denetlemeniz gerekir. **BAĞıMLı durumu göster** ' i çalıştırın ve aşağıdaki ayarı denetleyin.  

    ```azurecli-interactive
    Master_SSL_Allowed            : Yes
    Master_SSL_CA_File            : ~\azure_mysqlservice.pem
    Master_SSL_CA_Path            :
    Master_SSL_Cert               : ~\azure_mysqlclient_cert.pem
    Master_SSL_Cipher             :
    Master_SSL_Key                : ~\azure_mysqlclient_key.pem
    ```

    CA_file, SSL_Cert ve SSL_Key için sertifikanın sağlandığını görürseniz, [yeni sertifikayı](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) ekleyerek ve birleştirilmiş bir sertifika dosyası oluşturarak dosyayı güncelleştirmeniz gerekir.

* Veri çoğaltma, MySQL için iki Azure veritabanı arasındaysa, **çağrı MySQL.az_replication_change_master** yürüterek çoğaltmayı sıfırlamanız ve son parametre olarak yeni ikili kök sertifikayı sağlamanız gerekir [master_ssl_ca](howto-data-in-replication.md#4-link-source-and-replica-servers-to-start-data-in-replication)

### <a name="12-do-we-have-server-side-query-to-verify-if-ssl-is-being-used"></a>12. SSL 'nin kullanıldığını doğrulamak için sunucu tarafı sorgumuz var mı?

Sunucuya bağlanmak için SSL bağlantısı kullanıp kullandığınızı doğrulamak için [SSL doğrulaması](howto-configure-ssl.md#step-4-verify-the-ssl-connection)' na başvurun.

### <a name="13-is-there-an-action-needed-if-i-already-have-the-digicertglobalrootg2-in-my-certificate-file"></a>13. sertifika dosyasında DigiCertGlobalRootG2 zaten varsa gerekli bir eylem var mı?

Hayır. Sertifika dosyanızda zaten **DigiCertGlobalRootG2** varsa herhangi bir işlem yapmanız gerekmez.

### <a name="14-what-if-i-have-further-questions"></a>14. daha fazla sorunuz varsa ne yapmalıyım?

Sorularınız varsa, [Microsoft Q&A](mailto:AzureDatabaseforMySQL@service.microsoft.com)'daki topluluk uzmanlarının yanıtlarını alın. Destek planınız varsa ve teknik yardıma ihtiyacınız varsa [bizimle iletişime geçin](mailto:AzureDatabaseforMySQL@service.microsoft.com).
