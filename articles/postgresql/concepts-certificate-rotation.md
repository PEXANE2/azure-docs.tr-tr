---
title: PostgreSQL için Azure veritabanı tek sunucu için sertifika döndürme
description: PostgreSQL için Azure veritabanı tek sunucu için Azure veritabanı 'nı etkileyecek olan kök sertifika değişikliklerinin yakında değişiklikler hakkında bilgi edinin
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/02/2020
ms.openlocfilehash: c9bc6e3822ac6c014b9ff00e9cd81bbe707628fe
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101736075"
---
# <a name="understanding-the-changes-in-the-root-ca-change-for-azure-database-for-postgresql-single-server"></a>PostgreSQL için Azure veritabanı 'nın tek sunucu değişikliği için kök CA 'daki değişiklikleri anlama

PostgreSQL için Azure veritabanı tek sunucu, standart bakım ve güvenlik en iyi uygulamalarının bir parçası olarak **15 şubat 2021 (02/15/2021)** tarihinde kök sertifika değişikliğini başarıyla tamamladı. Bu makalede, değişiklikler hakkında daha fazla ayrıntı, etkilenen kaynaklar ve uygulamanızın veritabanı sunucunuza bağlantıyı korumasından emin olmak için gereken adımlar verilmektedir.

## <a name="why-root-certificate-update-is-required"></a>Kök sertifika güncelleştirmesi neden gereklidir?

PostgreSQL için Azure veritabanı kullanıcıları, [burada](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem)bulunan PostgreSQL sunucusuna bağlanmak için önceden tanımlanmış sertifikayı kullanabilir. Bununla birlikte, [sertifika yetkilisi (CA) tarayıcı Forumu](https://cabforum.org/),   CA satıcıları tarafından verilen birden çok sertifikanın uyumsuz olması için en son yayımlanmış raporlar.

Sektörün uyumluluk gereksinimlerine göre, CA satıcıları uyumlu olmayan CA 'Lar için CA sertifikalarını iptal etmeyi, sunucuların uyumlu CA 'lar tarafından verilen sertifikaları kullanmalarını ve bu uyumlu CA 'lardan CA sertifikaları kullanmasını gerektirir. MySQL için Azure veritabanı, bu uyumlu olmayan sertifikalardan birini kullandığından, MySQL sunucularınızda olası tehdidi en aza indirmek için sertifikayı uyumlu sürüme döndürmemiz gerekiyordu.

Yeni sertifika alınır ve 15 Şubat 2021 (02/15/2021) tarihinden itibaren geçerli olur. 

## <a name="what-change-was-performed-on-february-15-2021-02152021"></a>15 Şubat 2021 ' de hangi değişiklik yapıldı (02/15/2021)?

15 Şubat 2021 ' de, [baltimorecerbertrustroot kök](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) sertifikası aynı [baltimorecerbertrustroot kök sertifikası](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) ile değiştirilmiştir ve bu da mevcut müşterilerin sunucu bağlantılarının hiçbir etkisi olmadığından emin olun.  Bu değişiklik sırasında, [Baltimorecyıbertrustroot kök sertifikası](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) [DigiCertGlobalRootG2](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) ile **değiştirilmez** ve bu değişiklik, müşterilerin değişikliği yapmasına daha fazla zaman tanımak için ertelenir.

## <a name="do-i-need-to-make-any-changes-on-my-client-to-maintain-connectivity"></a>Bağlantıyı sürdürmek için istemcimde herhangi bir değişiklik yapmam gerekiyor mu?

İstemci tarafında bir değişiklik yapılması gerekmez. Aşağıdaki önceki önerimizi izlediyseniz, **Baltimorecyıbertrustroot sertifikası** birleştirilmiş CA sertifikasından kaldırılmadığı sürece yine de bağlanmaya devam edersiniz. **Bağlantıyı sürdürmeye yönelik daha fazla bildirimde bulununcaya kadar, birleştirilmiş CA sertifikanızdan Baltimorecyıbertrustroot 'yi kaldırmadık.**

### <a name="previous-recommendation"></a>Önceki öneri

*   Aşağıdaki bağlantılardan BaltimoreCyberTrustRoot & DigiCertGlobalRootG2 kök CA 'sını indirin:
    *   https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem
    *   https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem

*   Hem **Baltimorecyıbertrustroot** hem de **DigiCertGlobalRootG2** sertifikaları dahil olmak üzere bir birleştirilmiş CA sertifika deposu oluşturun.
    *   DefaultJavaSSLFactory kullanan Java (PostgreSQL JDBC) kullanıcıları için şunu çalıştırın:

          ```console
          keytool -importcert -alias PostgreSQLServerCACert  -file D:\BaltimoreCyberTrustRoot.crt.pem  -keystore truststore -storepass password -noprompt
          ```

          ```console
          keytool -importcert -alias PostgreSQLServerCACert2  -file D:\DigiCertGlobalRootG2.crt.pem -keystore truststore -storepass password  -noprompt
          ```

          Ardından orijinal anahtar deposu dosyasını yeni oluşturulan yeni bir anahtar ile değiştirin:
        *   System. setProperty ("javax. net. SSL. trustStore", "path_to_truststore_file"); 
        *   System. setProperty ("javax. net. SSL. trustStorePassword", "Password");
        
    *   Windows 'daki .NET (Npgsql) kullanıcıları için, Windows sertifika deposunda, güvenilen kök sertifika yetkilileri ' nde hem **Baltimore CyberTrust kökünün** hem de **DigiCert genel kökünün G2** olduğundan emin olun. Herhangi bir sertifika yoksa, eksik sertifikayı içeri aktarın.

        ![PostgreSQL için Azure veritabanı .net sertifikası](media/overview/netconnecter-cert.png)

    *   Linux üzerinde SSL_CERT_DIR kullanarak .NET (Npgsql) kullanıcıları için, SSL_CERT_DIR tarafından belirtilen dizinde **Baltimorecyıbertrustroot** ve **DigiCertGlobalRootG2** 'ın aynı olduğundan emin olun. Herhangi bir sertifika yoksa, eksik sertifika dosyasını oluşturun.

    *   Diğer PostgreSQL istemci kullanıcıları için aşağıdaki biçim gibi iki CA sertifika dosyasını birleştirebilirsiniz

        </br>-----BAŞLANGıÇ SERTIFIKASı-----
 </br>(Root CA1: BaltimoreCyberTrustRoot. CRT. pek)
 </br>-----SON SERTIFIKA-----
 </br>-----BAŞLANGıÇ SERTIFIKASı-----
 </br>(Root CA2: DigiCertGlobalRootG2. CRT. pek)
 </br>-----SON SERTIFIKA-----

*   Özgün kök CA ped dosyasını birleştirilmiş kök CA dosyası ile değiştirin ve uygulamanızı/istemcinizi yeniden başlatın.
*    Daha sonra, yeni sertifika sunucu tarafında dağıtıldıktan sonra, CA PEI dosyanızı DigiCertGlobalRootG2. CRT. ped olarak değiştirebilirsiniz.

> [!NOTE]
> Lütfen sertifika değişikliği yapılıncaya kadar **Baltidaha fazla sertifikayı** kaldırmayın veya değiştirmeyin. Değişiklik yapıldıktan sonra, Baltidaha fazla sertifikayı bırakması için güvenli olacak şekilde bir iletişim göndereceğiz. 

## <a name="why-was-baltimorecybertrustroot-certificate-not-replaced-to-digicertglobalrootg2-during-this-change-on-february-15-2021"></a>15 Şubat 2021 ' de bu değişiklik sırasında Baltimorecyıbertrustroot sertifikası neden DigiCertGlobalRootG2 ile değiştirilmedi?

Bu değişiklik için müşteri hazırlığını değerlendirdik ve çok sayıda müşteri bu değişikliği yönetmek için ek sağlama süresi arıyor. Müşterilere hazırlık sağlamak için daha fazla müşteri adayı süresi sunmanın yanı sıra, müşterilere ve son kullanıcılara yeterli sağlama süresi sağlayan en az bir yılda sertifika değişikliğini DigiCertGlobalRootG2 olarak ertelemenizi karardık. 

Kullanıcılara yönelik önerilerimiz, birleştirilmiş bir sertifika oluşturmak ve sunucunuza bağlanmak için bir iletişim gönderene kadar Baltimoreconbertrustroot sertifikasını kaldırmayın. 

## <a name="what-if-we-removed-the-baltimorecybertrustroot-certificate"></a>Baltimorecyıbertrustroot sertifikasını kaldırdık ne olursa?

PostgreSQL için Azure veritabanı sunucusuna bağlanırken hatalarla bağlantı kurma ile başlayacaksınız. Bağlantıyı sürdürmek için [Baltimorecyıbertrustroot](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) SERTIFIKASı ile [SSL 'yi yapılandırmanız](howto-configure-ssl.md) gerekir.


## <a name="frequently-asked-questions"></a>Sık sorulan sorular

###    <a name="1-if-i-am-not-using-ssltls-do-i-still-need-to-update-the-root-ca"></a>1. SSL/TLS kullanmıyorum, yine de kök CA 'yı güncelleştirmem gerekir mi?
SSL/TLS kullanmıyorsanız hiçbir eylem gerekmez. 

### <a name="2-if-i-am-using-ssltls-do-i-need-to-restart-my-database-server-to-update-the-root-ca"></a>2. SSL/TLS kullanıyorum, kök CA 'yı güncelleştirmek için veritabanı sunucusunu yeniden başlatmem gerekir mi?
Hayır, yeni sertifikayı kullanmaya başlamak için veritabanı sunucusunu yeniden başlatmanız gerekmez. Bu bir istemci tarafı değişikdir ve gelen istemci bağlantılarının veritabanı sunucusuna bağlanabildiklerinden emin olmak için yeni sertifikayı kullanması gerekir.

### <a name="3-how-do-i-know-if-im-using-ssltls-with-root-certificate-verification"></a>3. Nasıl yaparım? kök sertifika doğrulaması ile SSL/TLS kullanıp kullandığımı öğrenin.

Bağlantı dizenizi inceleyerek bağlantılarınızın kök sertifikayı doğrulayıp doğrulamayıp belirleyemeyeceğini belirleyebilirsiniz.
-    Bağlantı dizeniz veya içeriyorsa `sslmode=verify-ca` `sslmode=verify-full` , sertifikayı güncelleştirmeniz gerekir.
-    Bağlantı dizeniz,, `sslmode=disable` veya içerdiğinde, `sslmode=allow` `sslmode=prefer` `sslmode=require` sertifikaları güncelleştirmeniz gerekmez. 
-    Bağlantı dizeniz sslmode belirtmezse, sertifikaları güncelleştirmeniz gerekmez.

Bağlantı dizesini soyutlayan bir istemci kullanıyorsanız, sertifikaları doğrulayıp doğrulamadığını anlamak için istemci belgelerini gözden geçirin. PostgreSQL sslmode ' i anlamak için PostgreSQL belgelerindeki [SSL modu açıklamalarını](https://www.postgresql.org/docs/11/libpq-ssl.html#ssl-mode-descriptions) gözden geçirin.

### <a name="4-what-is-the-impact-if-using-app-service-with-azure-database-for-postgresql"></a>4. PostgreSQL için Azure veritabanı ile App Service kullanılıyorsa etki nedir?
Azure Uygulama Hizmetleri için PostgreSQL için Azure veritabanı 'na bağlanmak üzere iki olası senaryo olabilir ve bu, uygulamanızla birlikte SSL kullanma şeklinize bağlıdır.
*   Bu yeni sertifika platform düzeyinde App Service eklendi. Uygulamanızdaki App Service platforma dahil olan SSL sertifikalarını kullanıyorsanız hiçbir işlem gerekmez.
*   Kodunuzda SSL sertifika dosyasının yolunu açıkça dahil ediyorsanız, yeni sertifikayı indirmeniz ve kodu yeni sertifikayı kullanacak şekilde güncelleştirmeniz gerekir. Bu senaryonun iyi bir örneği, [App Service belgelerinde](../app-service/tutorial-multi-container-app.md#configure-database-variables-in-wordpress) paylaşıldığından App Service özel kapsayıcılar kullandığınızda oluşur

### <a name="5-what-is-the-impact-if-using-azure-kubernetes-services-aks-with-azure-database-for-postgresql"></a>5. PostgreSQL için Azure veritabanı ile Azure Kubernetes Hizmetleri (AKS) kullanılıyorsa etkisi nedir?
Azure Kubernetes Services (AKS) kullanarak PostgreSQL için Azure veritabanı 'na bağlanmaya çalışıyorsanız, adanmış bir müşterilerin ana bilgisayar ortamından erişime benzer. [Buradaki](../aks/ingress-own-tls.md)adımlara bakın.

### <a name="6-what-is-the-impact-if-using-azure-data-factory-to-connect-to-azure-database-for-postgresql"></a>6. PostgreSQL için Azure veritabanı 'na bağlanmak üzere Azure Data Factory kullanılıyorsa etkisi nedir?
Bağlayıcı, Azure Integration Runtime kullanan bağlayıcı için Azure 'da barındırılan ortamdaki Windows sertifika deposundaki sertifikalardan yararlanır. Bu sertifikalar zaten yeni uygulanan sertifikalarla uyumludur ve bu nedenle herhangi bir eylem gerekmez.

Şirket içinde barındırılan Integration Runtime kullanan bağlayıcı için, bağlantı dizinizdeki SSL sertifikası dosyasının yolunu açıkça eklediğiniz yerde, [yeni sertifikayı](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) indirip kullanılacak bağlantı dizesini güncelleştirmeniz gerekir.

### <a name="7-do-i-need-to-plan-a-database-server-maintenance-downtime-for-this-change"></a>7. bu değişiklik için bir veritabanı sunucusu bakım kapalı kalma süresi planlamanız gerekir mi?
Hayır. Buradaki değişiklik yalnızca istemci tarafında, veritabanı sunucusuna bağlanmak için olduğundan, bu değişiklik için veritabanı sunucusu için gerekli bakım kapalı kalma süresi yoktur.

### <a name="8-if-i-create-a-new-server-after-february-15-2021-02152021-will-i-be-impacted"></a>8.15 Şubat 2021 (02/15/2021) sonrasında yeni bir sunucu oluştururum, bundan etkilenecek mıyım?
15 Şubat 2021 (02/15/2021) ' den sonra oluşturulan sunucular için, uygulamalarınızın SSL kullanarak bağlanması için [Baltimorecyıbertrustroot](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) 'yi kullanmaya devam edersiniz.

### <a name="9-how-often-does-microsoft-update-their-certificates-or-what-is-the-expiry-policy"></a>9. Microsoft sertifikalarını ne sıklıkla güncelleştiriyor veya süre sonu ilkesi nedir?
PostgreSQL için Azure veritabanı tarafından kullanılan bu sertifikalar, güvenilen sertifika yetkilileri (CA) tarafından sağlanır. Bu nedenle, bu sertifikaların desteklenmesi CA tarafından bu sertifikaların desteğine bağlıdır. [Baltimorecerbertrustroot](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) sertifikası 2025 ' de sona ermek üzere zamanlandı. bu nedenle, Microsoft 'un süre sonu öncesinde bir sertifika değişikliği gerçekleştirmesi gerekir. Ayrıca, bu önceden tanımlanmış sertifikalarda öngörülemeyen hatalar varsa, Microsoft 'un her zaman güvenli ve uyumlu olduğundan emin olmak için, 15 Şubat 2021 ' de gerçekleştirilen değişikliğe benzer şekilde Microsoft 'un sertifika döndürmesini yapması gerekir.

### <a name="10-if-i-am-using-read-replicas-do-i-need-to-perform-this-update-only-on-the-primary-server-or-the-read-replicas"></a>10. okuma çoğaltmaları kullanıyorum, bu güncelleştirmeyi yalnızca birincil sunucuda veya okuma çoğaltmalarıyla gerçekleştirmem gerekir mi?
Bu güncelleştirme bir istemci tarafı değişikliği olduğundan, istemci Çoğaltma sunucusundan veri okumak için kullanılıyorsa, bu istemciler için değişiklikleri de uygulamanız gerekecektir. 

### <a name="11-do-we-have-server-side-query-to-verify-if-ssl-is-being-used"></a>11. SSL kullanıldığını doğrulamak için sunucu tarafı sorgumuz var mı?
Sunucuya bağlanmak için SSL bağlantısı kullanıp kullandığınızı doğrulamak için [SSL doğrulaması](concepts-ssl-connection-security.md#applications-that-require-certificate-verification-for-tls-connectivity)' na başvurun.

### <a name="12-is-there-an-action-needed-if-i-already-have-the-digicertglobalrootg2-in-my-certificate-file"></a>12. DigiCertGlobalRootG2 zaten sertifika dosyasında varsa gerekli bir eylem var mı?
Hayır. Sertifika dosyanızda zaten **DigiCertGlobalRootG2** varsa herhangi bir eylem gerekmez.

### <a name="13-what-if-you-are-using-docker-image-of-pgbouncer-sidecar-provided-by-microsoft"></a>13. Microsoft tarafından sunulan pgbouncer sepet 'ın Docker görüntüsünü kullanıyorsunuz?
Hem [**Baltimore**](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) hem de [**DigiCert**](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) 'yi destekleyen yeni bir Docker görüntüsü [burada aşağıda verilmiştir](https://hub.docker.com/_/microsoft-azure-oss-db-tools-pgbouncer-sidecar) (en son etiket). 15 Şubat 2021 ' den itibaren bağlantının kesintiye uğramasını önlemek için bu yeni görüntüyü çekebilirsiniz. 

### <a name="14-what-if-i-have-further-questions"></a>14. daha fazla sorunuz varsa ne yapmalıyım?
Sorularınız varsa, [Microsoft Q&A](mailto:AzureDatabaseforPostgreSQL@service.microsoft.com)'daki topluluk uzmanlarının yanıtlarını alın. Destek planınız varsa ve teknik yardıma ihtiyacınız varsa  [bizimle iletişime geçin](mailto:AzureDatabaseforPostgreSQL@service.microsoft.com)
