---
title: Açık kaynak teknolojileri SSS
description: Azure Uygulama Hizmeti'nde açık kaynak teknolojileri hakkında sık sorulan soruların yanıtlarını alın.
author: genlin
manager: dcscontentpm
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.custom: seodec18
ms.openlocfilehash: 33590f9fc7e6c4d46123cbc7088086a3197d52ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74672437"
---
# <a name="open-source-technologies-faqs-for-web-apps-in-azure"></a>Azure'daki Web Apps için açık kaynak teknolojileri SSS'leri

Bu makalede, [Azure Uygulama Hizmeti'nin Web Apps özelliği](https://azure.microsoft.com/services/app-service/web/)için açık kaynak teknolojileri ile ilgili sorunlar hakkında sık sorulan soruların (SSS) yanıtları bulunmaktadır.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="how-do-i-turn-on-php-logging-to-troubleshoot-php-issues"></a>PHP sorunlarını gidermek için PHP günlüğe kaydetmeyi nasıl açarım?

PHP günlüğe kaydetmeyi açmak için:

1. [Kudu web sitenizde](https://*yourwebsitename*.scm.azurewebsites.net)oturum açın.
2. Üst menüde **Hata Ayıklama Konsolu** > **CMD'yi**seçin.
3. **Site** klasörünü seçin.
4. **wwwroot** klasörünü seçin.
5. Simgeyi **+** seçin ve ardından **Yeni Dosya'yı**seçin.
6. Dosya adını **.user.ini**olarak ayarlayın.
7. **.user.ini'nin**yanındaki kalem simgesini seçin.
8. Dosyaya şu kodu ekleyin:`log_errors=on`
9. **Kaydet'i**seçin.
10. **wp-config.php'nin**yanındaki kalem simgesini seçin.
11. Metni aşağıdaki kodla değiştirin:
    ```php
    //Enable WP_DEBUG modedefine('WP_DEBUG', true);//Enable debug logging to /wp-content/debug.logdefine('WP_DEBUG_LOG', true);
    //Suppress errors and warnings to screendefine('WP_DEBUG_DISPLAY', false);//Suppress PHP errors to screenini_set('display_errors', 0);
    ```
12. Azure portalında, web uygulaması menüsünde web uygulamanızı yeniden başlatın.

Daha fazla bilgi için [WordPress hata günlüklerini etkinleştir'e](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/)bakın.

## <a name="how-do-i-log-python-application-errors-in-apps-that-are-hosted-in-app-service"></a>App Service'de barındırılan uygulamalarda Python uygulama hatalarını nasıl kaydederim?
[!INCLUDE [web-sites-python-troubleshooting-wsgi-error-log](../../includes/web-sites-python-troubleshooting-wsgi-error-log.md)]

## <a name="how-do-i-change-the-version-of-the-nodejs-application-that-is-hosted-in-app-service"></a>App Service'de barındırılan Düğüm.js uygulamasının sürümünü nasıl değiştirebilirim?

Düğüm.js uygulamasının sürümünü değiştirmek için aşağıdaki seçeneklerden birini kullanabilirsiniz:

* Azure portalında **Uygulama ayarlarını**kullanın.
  1. Azure portalında web uygulamanıza gidin.
  2. **Ayarlar** bıçağında **Uygulama ayarlarını**seçin.
  3. **Uygulama ayarlarında,** WEBSITE_NODE_DEFAULT_VERSION'ı anahtar olarak ve değer olarak istediğiniz Node.js sürümünü ekleyebilirsiniz.
  4. [Kudu konsoluna](https://*yourwebsitename*.scm.azurewebsites.net)git.
  5. Düğüm.js sürümünü denetlemek için aşağıdaki komutu girin:  
     ```
     node -v
     ```
* iisnode.yml dosyasını değiştirin. iisnode.yml dosyasındaki Düğüm.js sürümünü değiştirmek yalnızca iisnod'un kullandığı çalışma zamanı ortamını ayarlar. Kudu cmd'niz ve diğerleri hala Azure portalındaki **Uygulama ayarlarında** ayarlanan Düğüm.js sürümünü kullanmaya devam eder.

  iisnode.yml'yi el ile ayarlamak için uygulama kök klasörünüzde bir iisnode.yml dosyası oluşturun. Dosyada aşağıdaki satırı ekleyin:
  ```yml
  nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
  ```
   
* Kaynak denetimi dağıtımı sırasında package.json kullanarak iisnode.yml dosyasını ayarlayın.
  Azure kaynak denetimi dağıtım işlemi aşağıdaki adımları içerir:
  1. İçeriği Azure web uygulamasına taşır.
  2. Web uygulaması kök klasöründe bir tane (deploy.cmd, .deployment files) yoksa varsayılan dağıtım komut dosyası oluşturur.
  3. paket.json dosyasında Node.js sürümünden > motorunda bahsederseniz, iisnode.yml dosyası oluşturduğu bir dağıtım komut dosyası çalıştırır`"engines": {"node": "5.9.1","npm": "3.7.3"}`
  4. iisnode.yml dosyasında aşağıdaki kod satırı vardır:
      ```yml
      nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
      ```

## <a name="i-see-the-message-error-establishing-a-database-connection-in-my-wordpress-app-thats-hosted-in-app-service-how-do-i-troubleshoot-this"></a>App Service'de barındırılan WordPress uygulamamda "Veritabanı bağlantısı kurma hatası" mesajını görüyorum. Bu sorunu nasıl giderebilirim?

Azure WordPress uygulamanızda bu hatayı görürseniz, php_errors.log ve debug.log'u etkinleştirmek için [WordPress hata günlüklerini etkinleştir'de](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/)ayrıntılı olarak belirtilen adımları tamamlayın.

Günlükler etkinleştirildiğinde, hatayı yeniden üretin ve bağlantılarınız tükenip tükenmediğinizi görmek için günlükleri denetleyin:
```
[09-Oct-2015 00:03:13 UTC] PHP Warning: mysqli_real_connect(): (HY000/1226): User ‘abcdefghijk79' has exceeded the ‘max_user_connections’ resource (current value: 4) in D:\home\site\wwwroot\wp-includes\wp-db.php on line 1454
```

Bu hatayı hata ayıklama.log veya php_errors.log dosyalarınızda görürseniz, uygulamanız bağlantı sayısını aşıyor. ClearDB'de barındırma yapıyorsanız, [hizmet planınızda](https://www.cleardb.com/pricing.view)bulunan bağlantı sayısını doğrulayın.

## <a name="how-do-i-debug-a-nodejs-app-thats-hosted-in-app-service"></a>App Service'de barındırılan bir Düğüm.js uygulamasını nasıl hataayıka ait olurum?

1.  [Kudu konsoluna](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole)git.
2.  Uygulama günlükleri klasörüne gidin (D:\home\LogFiles\Application).
3.  logging_errors.txt dosyasında içeriği kontrol edin.

## <a name="how-do-i-install-native-python-modules-in-an-app-service-web-app-or-api-app"></a>Bir App Service web uygulamasına veya API uygulamasına yerel Python modüllerini nasıl yüklerim?

Bazı paketler Azure'da pip kullanarak yüklenmeyebilir. Paket Python Paket Dizini'nde kullanılamayabilir veya derleyici gerekebilir (App Service'te web uygulamasını çalıştıran bilgisayarda derleyici kullanılamaz). App Service web uygulamalarına ve API uygulamalarına yerel modülyükleme hakkında daha fazla bilgi için [Bkz.](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/)

## <a name="how-do-i-deploy-a-django-app-to-app-service-by-using-git-and-the-new-version-of-python"></a>Git'i ve Python'un yeni sürümünü kullanarak Bir Django uygulamasını Uygulama Hizmetine nasıl dağıtıyorum?

Django'nun kurulumu hakkında daha fazla bilgi için, [Uygulama Hizmetine Bir Django uygulaması dağıtma 'ya](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/)bakın.

## <a name="where-are-the-tomcat-log-files-located"></a>Tomcat günlük dosyaları nerede bulunur?

Azure Marketi ve özel dağıtımlar için:

* Klasör konumu: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs
* İlgi çekici dosyalar:
    * Catalina. *yyyy-mm-dd*.log
    * ev sahibi-yönetici. *yyyy-mm-dd*.log
    * Localhost. *yyyy-mm-dd*.log
    * Yöneticisi. *yyyy-mm-dd*.log
    * site_access_log. *yyyy-mm-dd*.log


Portal **Uygulama ayarları** dağıtımları için:

* Klasör konumu: D:\home\LogFiles
* İlgi çekici dosyalar:
    * Catalina. *yyyy-mm-dd*.log
    * ev sahibi-yönetici. *yyyy-mm-dd*.log
    * Localhost. *yyyy-mm-dd*.log
    * Yöneticisi. *yyyy-mm-dd*.log
    * site_access_log. *yyyy-mm-dd*.log

## <a name="how-do-i-troubleshoot-jdbc-driver-connection-errors"></a>JDBC sürücü bağlantı hatalarını nasıl giderebilirim?

Tomcat günlüklerinizde aşağıdaki iletiyi görebilirsiniz:

```
The web application[ROOT] registered the JDBC driver [com.mysql.jdbc.Driver] but failed to unregister it when the web application was stopped. To prevent a memory leak,the JDBC Driver has been forcibly unregistered
```

Hatayı gidermek için:

1. Sqljdbc*.jar dosyasını uygulama/lib klasörünüzden kaldırın.
2. Özel Tomcat veya Azure Marketi Tomcat web sunucusunu kullanıyorsanız, bu .jar dosyasını Tomcat lib klasörüne kopyalayın.
3. Azure portalından Java'yı etkinleştiriyorsanız **(Java 1.8** > **Tomcat sunucusunu**seçin), uygulamanızla paralel klasördeki sqljdbc.* jar dosyasını kopyalayın. Ardından, web.config dosyasına aşağıdaki classpath ayarını ekleyin:

    ```xml
    <httpPlatform>
    <environmentVariables>
    <environmentVariablename ="JAVA_OPTS" value=" -Djava.net.preferIPv4Stack=true
    -Xms128M -classpath %CLASSPATH%;[Path to the sqljdbc*.jarfile]" />
    </environmentVariables>
    </httpPlatform>
    ```

## <a name="why-do-i-see-errors-when-i-attempt-to-copy-live-log-files"></a>Canlı günlük dosyalarını kopyalamaya çalışırken neden hatalar görüyorum?

Bir Java uygulaması için canlı günlük dosyalarını kopyalamaya çalışırsanız (örneğin, Tomcat), bu FTP hatasını görebilirsiniz:

```
Error transferring file [filename] Copying files from remote side failed.
    
The process cannot access the file because it is being used by another process.
```

Hata iletisi FTP istemcisi bağlı olarak değişebilir.

Tüm Java uygulamalarında bu kilitleme sorunu var. Uygulama çalışırken yalnızca Kudu bu dosyayı indirmeyi destekler.

Uygulamayı durdurmak FTP'nin bu dosyalara erişmesine olanak tanır.

Başka bir geçici çözüm, zamanlamada çalışan ve bu dosyaları farklı bir dizine kopyalayan bir WebJob yazmaktır. Örnek bir proje için [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) projesine bakın.

## <a name="where-do-i-find-the-log-files-for-jetty"></a>İskele'nin günlük dosyalarını nerede bulabilirim?

Market ve özel dağıtımlar için günlük dosyası D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs klasöründedir. Klasör konumunun kullandığınız İskele sürümüne bağlı olduğunu unutmayın. Örneğin, burada sağlanan yol İskele 9.1.2 içindir. *jetty_ YYYY_MM_DD*.stderrout.log arayın.

Portal Uygulama Ayarlama dağıtımları için günlük dosyası D:\home\LogFiles'dadır. jetty_*YYYY_MM_DD*.stderrout.log arayın

## <a name="can-i-send-email-from-my-azure-web-app"></a>Azure web uygulamamdan e-posta gönderebilir miyim?

Uygulama Hizmeti'nin yerleşik bir e-posta özelliği yoktur. Uygulamanızdan e-posta göndermek için bazı iyi alternatifler için, bu [Stack Overflow tartışmasına](https://stackoverflow.com/questions/17666161/sending-email-from-azure)bakın.

## <a name="why-does-my-wordpress-site-redirect-to-another-url"></a>WordPress sitem neden başka bir URL'ye yönlendirilir?

Yakın zamanda Azure'a geçtiyseniz, WordPress eski etki alanı URL'sine yönlendirebilir. Bunun nedeni MySQL veritabanındaki bir ayardır.

WordPress Buddy+, yeniden yönlendirme URL'sini doğrudan veritabanında güncelleştirmek için kullanabileceğiniz bir Azure Site Uzantısıdır. WordPress Buddy+ kullanma hakkında daha fazla bilgi için [WordPress araçları ve WordPress Buddy+ ile MySQL geçişine](https://sharepointforum.org/threads/wordpress-tools-and-mysql-migration-with-wordpress-buddy.82929/)bakın.

Alternatif olarak, SQL sorguları veya PHPMyAdmin kullanarak yeniden yönlendirme URL'sini el ile güncelleştirmeyi tercih ederseniz, [WordPress: Yanlış URL'ye yönlendirme'](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/)ye bakın.

## <a name="how-do-i-change-my-wordpress-sign-in-password"></a>WordPress oturum açma şifremi nasıl değiştiririm?

WordPress oturum açma şifrenizi unuttuysanız, güncellemek için WordPress Buddy+'ı kullanabilirsiniz. Parolanızı sıfırlamak için WordPress Buddy+ Azure Site Uzantısı'nı yükleyin ve [WordPress araçları ve MySQL geçişinde](https://sharepointforum.org/threads/wordpress-tools-and-mysql-migration-with-wordpress-buddy.82929/)açıklanan adımları WordPress Buddy+ ile tamamlayın.

## <a name="i-cant-sign-in-to-wordpress-how-do-i-resolve-this"></a>WordPress'e kaydolamam. Bu sorunu nasıl çözebilirim?

Son zamanlarda bir eklenti yükledikten sonra kendinizi WordPress kilitli bulursanız, hatalı bir eklenti olabilir. WordPress Buddy+, WordPress'teki eklentileri devre dışı dışı bekleyebilirsiniz. Daha fazla bilgi için [WordPress araçları ve WordPress Buddy + ile MySQL geçiş](https://sharepointforum.org/threads/wordpress-tools-and-mysql-migration-with-wordpress-buddy.82929/)bakın.

## <a name="how-do-i-migrate-my-wordpress-database"></a>WordPress veritabanımı nasıl geçirebilirim?

WordPress web sitenize bağlı MySQL veritabanını geçirmek için birden çok seçeneğiniz vardır:

* Geliştiriciler: [Komut istemi veya PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/) kullanın
* Geliştirici olmayanlar: [WordPress Buddy+](https://sharepointforum.org/threads/wordpress-tools-and-mysql-migration-with-wordpress-buddy.82929/) kullanın

## <a name="how-do-i-help-make-wordpress-more-secure"></a>WordPress'in daha güvenli hale getirilen hale nasıl yardımcı olabilirim?

WordPress için güvenlik en iyi uygulamaları hakkında bilgi edinmek [için Azure'da WordPress güvenliği için en iyi uygulamalara](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/)bakın.

## <a name="i-am-trying-to-use-phpmyadmin-and-i-see-the-message-access-denied-how-do-i-resolve-this"></a>PHPMyAdmin kullanmaya çalışıyorum ve "Erişim reddedildi" iletisini görüyorum. Bu sorunu nasıl çözebilirim?

MySQL uygulama içi özelliği bu Uygulama Hizmeti örneğinde henüz çalışmıyorsa bu sorunla karşılaşabilirsiniz. Sorunu çözmek için web sitenize erişmeye çalışın. Bu, MySQL uygulama içi işlem de dahil olmak üzere gerekli işlemleri başlatır. MySQL in-app'inin çalıştığını doğrulamak için Process Explorer'da mysqld.exe'nin işlemlerde listelenmiş olduğundan emin olun.

MySQL in-app'inin çalıştığından emin olduktan sonra PHPMyAdmin'i kullanmayı deneyin.

## <a name="i-get-an-http-403-error-when-i-try-to-import-or-export-my-mysql-in-app-database-by-using-phpmyadmin-how-do-i-resolve-this"></a>PHPMyadmin kullanarak MySQL uygulama içi veritabanımı içe aktarmaya veya dışa aktarmaya çalıştığımda HTTP 403 hatası alıyorum. Bu sorunu nasıl çözebilirim?

Chrome'un eski bir sürümünü kullanıyorsanız, bilinen bir hatayla karşılaşabilirsiniz. Sorunu gidermek için Chrome'un daha yeni bir sürümüne yükseltin. Ayrıca, sorunun oluşmadığı Internet Explorer veya Microsoft Edge gibi farklı bir tarayıcı kullanmayı deneyin.
