---
title: 'Quickstart: Fivetran ve veri ambarı'
description: Fivetran ve Azure Synapse Analytics veri ambarı ile başlayın.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 10/12/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 8f164232a3b1782511758f93a9e9b8d17d3714d5
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414281"
---
# <a name="quickstart-fivetran-with-data-warehouse"></a>Quickstart: Veri ambarı ile Fivetran 

Bu hızlı başlangıç, SQL Havuzu ile birlikte sağlanan bir Azure Synapse Analytics veri ambarıyla çalışacak yeni bir Fivetran kullanıcısının nasıl ayarlanış edilebildiğini açıklar. Makale, varolan bir veri ambarı olduğunu varsayar.

## <a name="set-up-a-connection"></a>Bağlantı kurma

1. Veri ambarınıza bağlanmak için kullandığınız tam nitelikli sunucu adını ve veritabanı adını bulun.
    
    Bu bilgileri bulmak için yardıma ihtiyacınız varsa, [bkz.](../sql/connect-overview.md)

2. Kurulum sihirbazında, veritabanınızı doğrudan mı yoksa Bir SSH tüneli kullanarak mı bağlayıp bağlamayyacağınızı seçin.

   Doğrudan veritabanınıza bağlanmayı seçerseniz, erişime izin vermek için bir güvenlik duvarı kuralı oluşturmanız gerekir. Bu yöntem en basit ve en güvenli yöntemdir.

   Bir SSH tüneli kullanarak bağlanmayı seçerseniz, Fivetran ağınızdaki ayrı bir sunucuya bağlanır. Sunucu veritabanınıza bir SSH tüneli sağlar. Veritabanınız sanal ağda erişilemeyen bir alt ağdaysa bu yöntemi kullanmanız gerekir.

3. Fivetran'dan veri ambarı örneğinize gelen bağlantılara izin vermek için sunucu düzeyindeki güvenlik duvarınıza **52.0.2.4** IP adresini ekleyin.

   Daha fazla bilgi için [bkz.](create-data-warehouse-portal.md#create-a-server-level-firewall-rule)

## <a name="set-up-user-credentials"></a>Kullanıcı kimlik bilgilerini ayarlama

1. SQL Server Management Studio 'yı (SSMS) veya tercih ettiğiniz aracı kullanarak veri ambarınıza bağlanın. Sunucu yöneticisi kullanıcısı olarak oturum açın. Ardından, Fivetran için bir kullanıcı oluşturmak için aşağıdaki SQL komutlarını çalıştırın:

    - Ana veritabanında: 
    
      ```sql
      CREATE LOGIN fivetran WITH PASSWORD = '<password>'; 
      ```

    - Veri ambarı veritabanında:

      ```sql
      CREATE USER fivetran_user_without_login without login;
      CREATE USER fivetran FOR LOGIN fivetran;
      GRANT IMPERSONATE on USER::fivetran_user_without_login to fivetran;
      ```

2. Fivetran kullanıcısına veri ambarınıza aşağıdaki izinleri verin:

    ```sql
    GRANT CONTROL to fivetran;
    ```

    Bir kullanıcı Dosyaları Azure Blob depolama alanından PolyBase kullanarak yüklerken kullanılan veritabanı kapsamlı kimlik bilgilerini oluşturmak için CONTROL izni gereklidir.

3. Fivetran kullanıcısına uygun bir kaynak sınıfı ekleyin. Kullandığınız kaynak sınıfı, sütun deposu dizini oluşturmak için gereken belleğe bağlıdır. Örneğin, Marketo ve Salesforce gibi ürünlerle tümleştirmeler, çok sayıda sütun ve ürünlerin kullandığı daha büyük veri hacmi nedeniyle daha yüksek bir kaynak sınıfı gerektirir. Daha yüksek bir kaynak sınıfı, sütun deposu dizinleri oluşturmak için daha fazla bellek gerektirir.

    Statik kaynak sınıfları kullanmanızı öneririz. `staticrc20` Kaynak sınıfıyla başlayabilirsiniz. Kaynak `staticrc20` sınıfı, kullandığınız performans düzeyine bakılmaksızın her kullanıcı için 200 MB ayırır. Sütun deposu dizini ilk kaynak sınıfı düzeyinde başarısız olursa, kaynak sınıfını artırın.

    ```sql
    EXEC sp_addrolemember '<resource_class_name>', 'fivetran';
    ```

    Daha fazla bilgi için [bellek ve eşzamanlılık sınırları ve](memory-concurrency-limits.md) kaynak [sınıfları](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md#ways-to-allocate-more-memory)hakkında bilgi edinin.


## <a name="connect-from-fivetran"></a>Fivetran'dan bağlan

Fivetran hesabınızdan veri ambarınıza bağlanmak için, veri ambarınıza erişmek için kullandığınız kimlik bilgilerini girin: 

* Ana bilgisayar (sunucu adınız).
* Bağlantı noktası.
* Veritabanı.
* Kullanıcı (kullanıcı adı, *server_name* Azure ana bilgisayar URI'nizin bir parçası olduğu **fivetran\@server_name** olmalıdır: sunucu ** _\_adı_.database.windows.net).**
* Parola.
