---
title: Yönlendirme ile MySQL için Azure veritabanı 'na bağlanma
description: Bu makalede, uygulamanızı yeniden yönlendirme ile MySQL için Azure veritabanı 'na bağlanmak üzere nasıl yapılandırabileceğiniz açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 11/15/2019
ms.openlocfilehash: eed52c444bbcad60b4e629cf476c285f53c9f515
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74233746"
---
# <a name="connect-to-azure-database-for-mysql-with-redirection"></a>Yönlendirme ile MySQL için Azure veritabanı 'na bağlanma

Bu konuda, MySQL için Azure veritabanı sunucusuna yeniden yönlendirme moduyla bir uygulamanın nasıl bağlanacağı açıklanmaktadır. Uygulamaların doğrudan arka uç sunucu düğümlerine bağlanmasına izin vererek istemci uygulamaları ve MySQL sunucuları arasındaki ağ gecikmesini azaltmak için amaçlar 'yi yeniden yönlendirme.

> [!IMPORTANT]
> PHP [mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure) yeniden yönlendirme desteği şu anda önizleme aşamasındadır.

## <a name="before-you-begin"></a>Başlamadan önce
[Azure portalında](https://portal.azure.com) oturum açın. Altyapı sürümü 5,6, 5,7 veya 8,0 olan MySQL için Azure veritabanı sunucusu oluşturun. Ayrıntılar için [Portal 'Dan MySQL Için Azure veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md) veya [CLI kullanarak MySQL için Azure veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-cli.md)bölümüne bakın.

Yeniden yönlendirme özelliği şu anda yalnızca SSL etkinleştirildiğinde desteklenir. SSL 'yi yapılandırma hakkında ayrıntılı bilgi için bkz. [MySQL Için Azure veritabanı Ile SSL kullanma](https://docs.microsoft.com/azure/mysql/howto-configure-ssl#step-3-enforcing-ssl-connections-in-azure). 

## <a name="php"></a>PHP

### <a name="ubuntu-linux"></a>Ubuntu Linux

#### <a name="prerequisites"></a>Önkoşullar 
- PHP sürümleri 7.2.15 + ve 7.3.2 +
- PHP PEAR 
- PHP-MySQL
- SSL özellikli MySQL için Azure veritabanı sunucusu

1. [Mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure) [,](https://pecl.php.net/package/mysqlnd_azure)

    ```bash
    sudo pecl install mysqlnd_azure
    ```

2. Aşağıdaki adımları çalıştırarak uzantı dizinini (`extension_dir`) bulun:

    ```bash
    php -i | grep "extension_dir"
    ```

3. Dizini döndürülen klasöre değiştirin ve `mysqlnd_azure.so` bu klasörde bulunduğundan emin olun. 

4. Aşağıdaki çalıştırarak. ini dosyaları klasörünü bulun: 

    ```bash
    php -i | grep "dir for additional .ini files"
    ```

5. Dizinleri bu döndürülen klasör olarak değiştirin. 

6. `mysqlnd_azure`için yeni bir. ini dosyası oluşturun. Modüller, INI dosyalarının ad sırasına göre yüklendiğinden, bu adın alfabetik sırasının mysqnld ' den sonra olduğundan emin olun. Örneğin, `mysqlnd`. ini `10-mysqlnd.ini`olarak adlandırılmışsa, mysqlnd ini `20-mysqlnd-azure.ini`olarak adlandırın.

7. Yeni. ini dosyası içinde yeniden yönlendirmeyi etkinleştirmek için aşağıdaki satırları ekleyin.

    ```bash
    extension=mysqlnd_azure
    mysqlnd_azure.enabled=on
    ```

### <a name="windows"></a>Windows

#### <a name="prerequisites"></a>Önkoşullar 
- PHP sürümleri 7.2.15 + ve 7.3.2 +
- PHP-MySQL
- SSL özellikli MySQL için Azure veritabanı sunucusu

1. Aşağıdaki komutu çalıştırarak PHP 'nin x64 veya x86 sürümünü çalıştırıp çalıştırmadığını belirleyebilirsiniz:

    ```cmd
    php -i | findstr "Thread"
    ```

2. PHP sürümünüz ile eşleşen bir [mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure) [DLL 'nin karşılık](https://pecl.php.net/package/mysqlnd_azure) gelen x64 veya x86 sürümünü indirin. 

3. ZIP dosyasını ayıklayın ve `php_mysqlnd_azure.dll`adlı DLL dosyasını bulun.

4. Aşağıdaki komutu çalıştırarak uzantı dizinini (`extension_dir`) bulun:

    ```cmd
    php -i | find "extension_dir"s
    ```

5. `php_mysqlnd_azure.dll` dosyasını adım 4 ' te döndürülen dizine kopyalayın. 

6. Aşağıdaki komutu kullanarak `php.ini` dosyasını içeren PHP klasörünü bulun:

    ```cmd
    php -i | find "Loaded Configuration File"
    ```

7. `php.ini` dosyasını değiştirin ve yeniden yönlendirmeyi etkinleştirmek için aşağıdaki ek satırları ekleyin. 

    Dinamik uzantılar bölümünde: 
    ```cmd
    extension=mysqlnd_azure
    ```
    
    Modül ayarları bölümünde:     
    ```cmd 
    [mysqlnd_azure]
    mysqlnd_azure.enabled=on
    ```

### <a name="confirm-redirection"></a>Yeniden yönlendirmeyi Onayla

Yeniden yönlendirmeyi de aşağıdaki örnek PHP kodu ile yapılandırıldığını doğrulayabilirsiniz. `mysqlConnect.php` adlı bir PHP dosyası oluşturun ve aşağıdaki kodu yapıştırın. Sunucu adını, Kullanıcı adını ve parolayı kendi kendinize güncelleştirin. 
 
 ```php
<?php
$host = '<yourservername>.mysql.database.azure.com';
$username = '<yourusername>@<yourservername>';
$password = '<yourpassword>';
$db_name = 'testdb';
  echo "mysqlnd_azure.enabled: ", ini_get("mysqlnd_azure.enabled") == true?"On":"Off", "\n";
  $db = mysqli_init();
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
Bağlantı dizeleri hakkında daha fazla bilgi için, [bağlantı dizelerine](howto-connection-string.md)bakın.

