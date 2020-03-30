---
title: Yeniden yönlendirme yle bağlan - MySQL için Azure Veritabanı
description: Bu makalede, yeniden yönlendirme ile MySQL için Azure Veritabanı'na bağlanmak için uygulamanızı nasıl yapılandırabileceğiniz açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: f987d5d9640c3bfef61320df379a68eae2f4712b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246344"
---
# <a name="connect-to-azure-database-for-mysql-with-redirection"></a>Yeniden yönlendirme ile MySQL için Azure Veritabanına bağlanın

Bu konu, mysql sunucusu için Azure Veritabanınızın yeniden yönlendirme moduyla nasıl bağlanılsüreceğini açıklar. Yeniden yönlendirme, uygulamaların doğrudan arka uç sunucu düğümlerine bağlanmasına izin vererek istemci uygulamaları ve MySQL sunucuları arasındaki ağ gecikmesini azaltmayı amaçlar.

## <a name="before-you-begin"></a>Başlamadan önce
[Azure portalında](https://portal.azure.com)oturum açın. MySQL sunucusu için 5.6, 5.7 veya 8.0 motor sürümüiçeren bir Azure Veritabanı oluşturun. Ayrıntılar için, [Portal'dan MySQL sunucusu için Azure Veritabanı nasıl oluşturulur](quickstart-create-mysql-server-database-using-azure-portal.md) veya [CLI kullanarak MySQL sunucusu için Azure Veritabanı nasıl oluşturulur'a](quickstart-create-mysql-server-database-using-azure-cli.md)bakın.

Yeniden yönlendirme şu anda yalnızca MySQL sunucusu için Azure Veritabanınızda **SSL etkinleştirildiğinde** desteklenir. SSL'nin nasıl yapılandırılabildiğini öğrenmek [için MySQL için Azure Veritabanı ile SSL kullanma bilgisine](howto-configure-ssl.md#step-3--enforcing-ssl-connections-in-azure)bakın.

## <a name="php"></a>PHP

PHP uygulamalarında yeniden yönlendirme desteği, Microsoft tarafından geliştirilen [mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure) uzantısı aracılığıyla kullanılabilir. 

mysqlnd_azure uzantısı PECL üzerinden PHP uygulamalarına eklemek için kullanılabilir ve son derece yüklemek ve resmi olarak yayınlanan [PECL paketi](https://pecl.php.net/package/mysqlnd_azure)ile uzantısı yapılandırmak için tavsiye edilir.

> [!IMPORTANT]
> PHP [mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure) uzantısı yeniden yönlendirme desteği şu anda önizleme de.

### <a name="redirection-logic"></a>Yeniden yönlendirme mantığı

>[!IMPORTANT]
> Yönlendirme mantığı/davranış başlangıç sürümü 1.1.0 güncelleştirildi ve **sürüm 1.1.0+ kullanılması önerilir.**

Yeniden yönlendirme `mysqlnd_azure.enableRedirect`davranışı. Aşağıdaki **tabloda, sürüm 1.1.0+** ile başlayan bu parametrenin değerine göre yeniden yönlendirme davranışı özetlenir.

mysqlnd_azure uzantının (sürüm 1.0.0-1.0.3) eski bir sürümünü kullanıyorsanız, yeniden yönlendirme `mysqlnd_azure.enabled`davranışı . Geçerli değerler `off` (aşağıdaki tabloda özetlenen davranış gibi davranır) `on` ve `preferred` (aşağıdaki tabloda gibi davranır).  

|**mysqlnd_azure.enableRedirect değeri**| **Davranış**|
|----------------------------------------|-------------|
|`off` veya `0`|Yeniden yönlendirme kullanılmaz. |
|`on` veya `1`|- MySQL sunucusu için Azure Veritabanı'nda SSL etkin değilse, bağlantı yapılmaz. Aşağıdaki hata döndürülür: *"mysqlnd_azure.enableRedirect açık, ancak SSL seçeneği bağlantı dizesinde ayarlanmaz. Yeniden yönlendirme sadece SSL ile mümkündür."*<br>- MySQL sunucusunda SSL etkinse, ancak sunucuda yeniden yönlendirme desteklenmiyorsa, ilk bağlantı iptal edilir ve aşağıdaki hata döndürülür: *"MySQL sunucusunda yeniden yönlendirme etkinleştirilemediği veya ağ paketi yeniden yönlendirme protokolüne uymadığı* için bağlantı iptal edilir."<br>- MySQL sunucusu yeniden yönlendirmeyi destekliyorsa, ancak yeniden yönlendirilen bağlantı herhangi bir nedenle başarısız olursa, ilk proxy bağlantısını da iptal edin. Yeniden yönlendirilen bağlantının hatasını döndürün.|
|`preferred` veya `2`<br> (varsayılan değer)|- mysqlnd_azure mümkünse yeniden yönlendirme kullanır.<br>- Bağlantı SSL kullanmıyorsa, sunucu yeniden yönlendirmeyi desteklemez veya proxy bağlantısı hala geçerli yken, yeniden yönlendirilen bağlantı herhangi bir önemli olmayan nedenle bağlanamazsa, ilk proxy bağlantısına geri döner.|

Belgenin sonraki bölümleri, uzantının `mysqlnd_azure` PECL kullanarak nasıl yüklenir ve bu parametrenin değerini nasıl ayarlayacaklarını ana hatlar.

### <a name="ubuntu-linux"></a>Ubuntu Linux

#### <a name="prerequisites"></a>Ön koşullar 
- PHP versiyonları 7.2.15+ ve 7.3.2+
- PHP ARMUT 
- php-mysql
- SSL etkin MySQL sunucusu için Azure Veritabanı

1. PECL ile [mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure) [yükleyin.](https://pecl.php.net/package/mysqlnd_azure) Sürüm 1.1.0+ kullanılması önerilir.

    ```bash
    sudo pecl install mysqlnd_azure
    ```

2. Aşağıdaki leri çalıştırarak`extension_dir`uzantı dizini ( ) bulun:

    ```bash
    php -i | grep "extension_dir"
    ```

3. Dizinleri döndürülen klasöre `mysqlnd_azure.so` değiştirin ve bu klasörde bulunduğundan emin olun. 

4. .ini dosyaları nın klasörünü aşağıdakileri çalıştırarak bulun: 

    ```bash
    php -i | grep "dir for additional .ini files"
    ```

5. Dizinleri bu döndürülen klasörde değiştirin. 

6. Için `mysqlnd_azure`yeni bir .ini dosyası oluşturun. Modüller ini dosyalarının ad sırasına göre yüklendiğinden, ismin alfabe sırasının mysqnld'den sonra olduğundan emin olun. Örneğin, .ini adlı `mysqlnd` `10-mysqlnd.ini`ise, mysqlnd ini `20-mysqlnd-azure.ini`olarak adlandırın .

7. Yeni .ini dosyasında, yeniden yönlendirmeyi etkinleştirmek için aşağıdaki satırları ekleyin.

    ```bash
    extension=mysqlnd_azure
    mysqlnd_azure.enableRedirect = on/off/preferred
    ```

### <a name="windows"></a>Windows

#### <a name="prerequisites"></a>Ön koşullar 
- PHP versiyonları 7.2.15+ ve 7.3.2+
- php-mysql
- SSL etkin MySQL sunucusu için Azure Veritabanı

1. Aşağıdaki komutu çalıştırarak PHP'nin x64 veya x86 sürümünü çalıştırıp çalıştırmadığınızı belirleyin:

    ```cmd
    php -i | findstr "Thread"
    ```

2. PHP sürümünüzle eşleşen mysqlnd_azure [mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure) DLL'nin ilgili x64 veya x86 sürümünü [PECL'den](https://pecl.php.net/package/mysqlnd_azure) indirin. Sürüm 1.1.0+ kullanılması önerilir.

3. Zip dosyasını ayıklayın ve `php_mysqlnd_azure.dll`DLL adlı bulun.

4. Aşağıdaki komutu çalıştırarak uzantı dizini (`extension_dir`) bulun:

    ```cmd
    php -i | find "extension_dir"
    ```

5. Dosyayı `php_mysqlnd_azure.dll` adım 4'te döndürülen dizine kopyalayın. 

6. Aşağıdaki komutu kullanarak `php.ini` dosyayı içeren PHP klasörünü bulun:

    ```cmd
    php -i | find "Loaded Configuration File"
    ```

7. Yeniden `php.ini` yönlendirmeyi etkinleştirmek için dosyayı değiştirin ve aşağıdaki ek satırları ekleyin. 

    Dinamik Uzantılar bölümü altında: 
    ```cmd
    extension=mysqlnd_azure
    ```
    
    Modül Ayarları bölümünde:     
    ```cmd 
    [mysqlnd_azure]
    mysqlnd_azure.enableRedirect = on/off/preferred
    ```

### <a name="confirm-redirection"></a>Yeniden yönlendirmeyi onaylayın

Ayrıca, yeniden yönlendirmenin aşağıdaki örnek PHP koduyla yapılandırıldığı da onaylayabilirsiniz. Çağrılan `mysqlConnect.php` bir PHP dosyası oluşturun ve aşağıdaki kodu yapıştırın. Sunucu adını, kullanıcı adını ve parolayı kendi adınızile güncelleştirin. 
 
 ```php
<?php
$host = '<yourservername>.mysql.database.azure.com';
$username = '<yourusername>@<yourservername>';
$password = '<yourpassword>';
$db_name = 'testdb';
  echo "mysqlnd_azure.enableRedirect: ", ini_get("mysqlnd_azure.enableRedirect"), "\n";
  $db = mysqli_init();
  //The connection must be configured with SSL for redirection test
  $link = mysqli_real_connect ($db, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL);
  if (!$link) {
     die ('Connect error (' . mysqli_connect_errno() . '): ' . mysqli_connect_error() . "\n");
  }
  else {
    echo $db->host_info, "\n"; //if redirection succeeds, the host_info will differ from the hostname you used used to connect
    $res = $db->query('SHOW TABLES;'); //test query with the connection
    print_r ($res);
    $db->close();
  }
?>
 ```

## <a name="next-steps"></a>Sonraki adımlar
Bağlantı dizeleri hakkında daha fazla bilgi için [Bağlantı Dizeleri'ne](howto-connection-string.md)bakın.
