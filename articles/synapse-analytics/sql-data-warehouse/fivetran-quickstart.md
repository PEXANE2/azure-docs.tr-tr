---
title: 'Hızlı başlangıç: Fivetran ve veri ambarı'
description: Fivetran ve bir Azure SYNAPSE Analytics veri ambarı ile çalışmaya başlayın.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 10/12/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 96e679c0b284cc649dbde3fba1b640f4e09df05e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85201659"
---
# <a name="quickstart-fivetran-with-data-warehouse"></a>Hızlı başlangıç: Fivetle veri ambarı ile çalıştırıldı 

Bu hızlı başlangıçta, bir SQL havuzuyla sağlanan bir Azure SYNAPSE Analytics veri ambarı ile çalışmak üzere yeni bir Fivetran kullanıcısının nasıl ayarlanacağı açıklanır. Makalesinde, mevcut bir veri ambarınız olduğunu varsaymaktadır.

## <a name="set-up-a-connection"></a>Bağlantı kurma

1. Veri ambarınıza bağlanmak için kullandığınız tam sunucu adını ve veritabanı adını bulun.
    
    Bu bilgileri bulmak için yardıma ihtiyacınız varsa bkz. [veri ambarınıza bağlanma](../sql/connect-overview.md).

2. Kurulum sihirbazında, veritabanınıza doğrudan veya bir SSH tüneli kullanarak bağlanıp bağlanmayacağını seçin.

   Doğrudan veritabanınıza bağlanmayı seçerseniz, erişime izin vermek için bir güvenlik duvarı kuralı oluşturmanız gerekir. Bu yöntem, en basit ve en güvenli yöntemdir.

   Bir SSH tüneli kullanarak bağlanmayı seçerseniz, Fivetran, ağınızdaki ayrı bir sunucuya bağlanır. Sunucu, veritabanınıza bir SSH tüneli sağlar. Veritabanınız bir sanal ağ üzerinde erişilemeyen bir alt ağdaysa bu yöntemi kullanmanız gerekir.

3. Fivetran 'dan veri ambarı örneğinize gelen bağlantılara izin vermek için **52.0.2.4** IP adresini sunucu düzeyi güvenlik duvarınıza ekleyin.

   Daha fazla bilgi için bkz. [sunucu düzeyinde güvenlik duvarı kuralı oluşturma](create-data-warehouse-portal.md#create-a-server-level-firewall-rule).

## <a name="set-up-user-credentials"></a>Kullanıcı kimlik bilgilerini ayarlama

1. SQL Server Management Studio (SSMS) veya tercih ettiğiniz aracı kullanarak veri ambarınıza bağlanın. Sunucu Yöneticisi kullanıcısı olarak oturum açın. Ardından, aşağıdaki SQL komutlarını çalıştırarak Fivetran için bir kullanıcı oluşturun:

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

    Kullanıcı, PolyBase kullanarak Azure Blob depolamadan dosya yüklediğinde kullanılan veritabanı kapsamlı kimlik bilgileri oluşturmak için DENETIM izni gereklidir.

3. Fivetran kullanıcısına uygun bir kaynak sınıfı ekleyin. Kullandığınız kaynak sınıfı, bir columnstore dizini oluşturmak için gereken belleğe bağlıdır. Örneğin, Marketo ve Salesforce gibi ürünlerle tümleştirmeler, çok sayıda sütun ve ürünlerin kullanacağı daha büyük veri hacmi nedeniyle daha yüksek bir kaynak sınıfı gerektirir. Daha yüksek bir kaynak sınıfı, columnstore dizinleri oluşturmak için daha fazla bellek gerektirir.

    Statik kaynak sınıfları kullanmanızı öneririz. `staticrc20`Kaynak sınıfıyla başlayabilirsiniz. `staticrc20`Kaynak sınıfı, kullandığınız performans düzeyinden bağımsız olarak her kullanıcı için 200 MB ayırır. Columnstore dizin oluşturma ilk kaynak sınıfı düzeyinde başarısız olursa, kaynak sınıfını arttırın.

    ```sql
    EXEC sp_addrolemember '<resource_class_name>', 'fivetran';
    ```

    Daha fazla bilgi için [bellek ve eşzamanlılık sınırları](memory-concurrency-limits.md) ve [kaynak sınıfları](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md#ways-to-allocate-more-memory)hakkında bilgi edinin.


## <a name="connect-from-fivetran"></a>Fivetran 'dan Bağlan

Fivetran hesabınızdan veri ambarınıza bağlanmak için, veri ambarınıza erişmek için kullandığınız kimlik bilgilerini girin: 

* Ana bilgisayar (sunucunuzun adı).
* Bağ.
* Veritabanınızı.
* Kullanıcı (Kullanıcı adı, *SERVER_NAME* Azure ana bilgisayar URI 'sinin bir parçası olduğu için **fivetran \@ _SERVER_NAME_ ** olmalıdır: ** _sunucu \_ adı_. Database.Windows.net**).
* Parolayı.
