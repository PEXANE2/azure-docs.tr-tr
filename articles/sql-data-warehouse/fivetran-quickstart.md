---
title: 'Hızlı başlangıç: Fivetran ve veri ambarı'
description: Fivetran ve bir Azure SYNAPSE Analytics veri ambarı ile çalışmaya başlayın.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: integration
ms.date: 10/12/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: b068b2436aaa1df22e3c83a54fb384f925149cc2
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78194623"
---
# <a name="quickstart-fivetran-with-data-warehouse"></a>Hızlı başlangıç: Fivetle veri ambarı ile çalıştırıldı 

Bu hızlı başlangıçta, bir SQL havuzuyla sağlanan bir Azure SYNAPSE Analytics veri ambarı ile çalışmak üzere yeni bir Fivetran kullanıcısının nasıl ayarlanacağı açıklanır. Makalesinde, mevcut bir veri ambarınız olduğunu varsaymaktadır.

## <a name="set-up-a-connection"></a>Bağlantı kurma

1. Veri ambarınıza bağlanmak için kullandığınız tam sunucu adını ve veritabanı adını bulun.
    
    Bu bilgileri bulmak için yardıma ihtiyacınız varsa bkz. [veri ambarınıza bağlanma](sql-data-warehouse-connect-overview.md).

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

    Statik kaynak sınıfları kullanmanızı öneririz. `staticrc20` kaynak sınıfıyla başlayabilirsiniz. `staticrc20` kaynak sınıfı, kullandığınız performans düzeyinden bağımsız olarak her kullanıcı için 200 MB ayırır. Columnstore dizin oluşturma ilk kaynak sınıfı düzeyinde başarısız olursa, kaynak sınıfını arttırın.

    ```sql
    EXEC sp_addrolemember '<resource_class_name>', 'fivetran';
    ```

    Daha fazla bilgi için [bellek ve eşzamanlılık sınırları](memory-concurrency-limits.md) ve [kaynak sınıfları](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md#ways-to-allocate-more-memory)hakkında bilgi edinin.


## <a name="sign-in-to-fivetran"></a>Fivetran 'da oturum açın

Fivetran 'da oturum açmak için, veri ambarınıza erişmek için kullandığınız kimlik bilgilerini girin: 

* Ana bilgisayar (sunucunuzun adı).
* Bağ.
* Veritabanınızı.
* Kullanıcı ( *SERVER_NAME* Azure ana bilgisayar URI 'nizin parçası olan  **_SERVER_NAME_\@fivetran** olmalıdır:  **_sunucu\_Name_. Database.Windows.net**).
* Parolayı.
