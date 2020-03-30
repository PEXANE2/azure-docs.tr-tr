---
title: Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma
description: Tek ve havuzlu veritabanları için SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralı oluşturma
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: vanto, carlrab
ms.date: 02/11/2019
ms.openlocfilehash: ff2508952b75bad88ff8ff92388c20ba52f50f42
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73818251"
---
# <a name="quickstart-create-a-server-level-firewall-rule-for-single-and-pooled-databases-using-the-azure-portal"></a>Hızlı başlatma: Azure portalını kullanarak tek ve havuzlu veritabanları için sunucu düzeyinde güvenlik duvarı kuralı oluşturma

Bu hızlı başlangıç, veritabanı sunucularına, tek veritabanlarına ve elastik havuzlara ve veritabanlarına bağlanmanızı sağlamak için Azure portalını kullanarak Azure SQL Veritabanı'ndaki tek ve havuzlu veritabanları için sunucu düzeyinde bir [güvenlik duvarı kuralı](sql-database-firewall-configure.md) oluşturma yı geçer. Diğer Azure kaynaklarından ve şirket içi kaynaklardan bağlanmak için bir güvenlik duvarı kuralı gerekir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlatma, başlangıç noktası olarak [Azure portalını kullanarak tek bir veritabanı oluştur'da](sql-database-single-database-get-started.md) oluşturulan kaynakları kullanır.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="create-a-server-level-ip-firewall-rule"></a>Sunucu düzeyinde IP güvenlik duvarı kuralı oluşturma

SQL Veritabanı hizmeti, tek ve havuzlu veritabanları için veritabanı sunucusu düzeyinde bir güvenlik duvarı oluşturur. Bu güvenlik duvarı, güvenlik duvarını açmak için bir IP güvenlik duvarı kuralı oluşturmadığınız sürece istemci uygulamalarının sunucuya veya tek veya birleştirilmiş veritabanlarından herhangi biriyle bağlanmasını engeller. Azure dışındaki bir IP adresinden bağlantı için, bağlanmak istediğiniz belirli bir IP adresi veya adres aralığı için bir güvenlik duvarı kuralı oluşturun. Sunucu düzeyi ve veritabanı düzeyindeki IP güvenlik duvarı kuralları hakkında daha fazla bilgi için [SQL Veritabanı sunucu düzeyi ve veritabanı düzeyindeIP güvenlik duvarı kurallarına](sql-database-firewall-configure.md)bakın.

> [!NOTE]
> SQL Veritabanı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Bir şirket ağından bağlanmaya çalışıyorsanız ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu nedenle, BT departmanınız 1433 bağlantı noktasını açmadıkça Azure SQL Veritabanı sunucunuza bağlanamazsınız.
> [!IMPORTANT]
> 0.0.0.0'lık bir güvenlik duvarı kuralı, tüm Azure hizmetlerinin sunucu düzeyindeki güvenlik duvarı kuralından geçmesini ve sunucu üzerinden tek veya havuzlanmış bir veritabanına bağlanmaya çalışmasını sağlar. 

İstemcinizin IP adresi için sunucu düzeyinde bir IP güvenlik duvarı kuralı oluşturmak ve yalnızca IP adresiniz için SQL Veritabanı güvenlik duvarı üzerinden harici bağlantı sağlamak için aşağıdaki adımları izleyin.

1. Ön [koşul Azure SQL veritabanı](#prerequisites) dağıtımı tamamlandıktan sonra, sol menüden **SQL veritabanlarını** seçin ve ardından **SQL veritabanları** sayfasında **mySampleDatabase'i** seçin. Veritabanınız için genel bakış sayfası açılır ve size tam nitelikli sunucu adını **(mynewserver-20170824.database.windows.net**gibi) gösterir ve daha fazla yapılandırma için seçenekler sunar.

2. Sunucunuza ve veritabanlarına diğer hızlı başlangıçlarda bağlanırken kullanmak üzere bu tam nitelikli sunucu adını kopyalayın.

   ![sunucu adı](./media/sql-database-get-started-portal/server-name.png)

3. Araç çubuğunda **sunucu güvenlik duvarını** ayarla'yı seçin. Veritabanı sunucusuiçin **Güvenlik Duvarı ayarları** sayfası açılır.

   ![sunucu düzeyinde IP güvenlik duvarı kuralı](./media/sql-database-get-started-portal/server-firewall-rule.png)

4. Geçerli IP adresinizi sunucu düzeyindeki yeni bir IP güvenlik duvarı kuralına eklemek için araç çubuğuna **istemci IP ekle'yi** seçin. Sunucu düzeyindeki BIR IP güvenlik duvarı kuralı, tek bir IP adresi veya çeşitli IP adresleri için 1433 bağlantı noktasını açabilir.

   > [!IMPORTANT]
   > Varsayılan olarak, SQL Veritabanı güvenlik duvarı üzerinden erişim tüm Azure hizmetleri için devre dışı bırakılır. Tüm Azure hizmetlerine erişimi etkinleştirmek istiyorsanız bu sayfada **AÇI'yı** seçin.
   >

5. **Kaydet'i**seçin. SQL Veritabanı sunucusunda geçerli IP adresiniz 1433 açma bağlantı noktası için sunucu düzeyinde BIR IP güvenlik duvarı kuralı oluşturulur.

6. Güvenlik **Duvarı ayarları** sayfasını kapatın.

SQL Server Management Studio'yu veya seçtiğiniz başka bir aracı kullanarak, daha önce oluşturulan sunucu yöneticisi hesabını kullanarak bu IP adresinden SQL Database sunucusuna ve veritabanlarına bağlanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[Sonraki adımlar](#next-steps)’a geçip farklı yöntemler kullanarak veritabanını sorgulama hakkında bilgi edinmek istiyorsanız bu kaynakları kaydedin. Ancak, bu hızlı başlatmada oluşturduğunuz kaynakları silmek istiyorsanız, aşağıdaki adımları kullanın.

1. Azure portalındaki sol menüden Kaynak **gruplarını** seçin ve ardından **myResourceGroup'u**seçin.
2. Kaynak grubu sayfanızda **Sil,** metin kutusuna **myResourceGroup** yazın ve sonra **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Artık bir veritabanınız olduğuna göre, şunlar dahil tercih ettiğiniz araçlar ya da diller ile veritabanında [bağlanma ve sorgulama](sql-database-connect-query.md) işlemleri yapabilirsiniz:
  - [SQL Server Management Studio kullanarak bağlanma ve sorgulama](sql-database-connect-query-ssms.md)
  - [Azure Data Studio kullanarak bağlanma ve sorgulama](/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- İlk veritabanınızı tasarlamayı, tablolar oluşturmayı ve veri eklemeyi öğrenmek için, şu öğreticilerden birine bakın:
  - [Azure SQL Veritabanı'ndaki ilk tek veritabanınızı SSMS kullanarak tasarla](sql-database-design-first-database.md)
  - [Azure SQL Veritabanı'nda tek bir veritabanı tasarlayın ve C# ve ADO.NET ile bağlantı kurun](sql-database-design-first-database-csharp.md)
