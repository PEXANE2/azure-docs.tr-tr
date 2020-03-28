---
title: 'İlk ilişkisel veritabanınızı tasarla C #'
description: İlk ilişkisel veritabanınızı Azure SQL Veritabanı'nda C# ile tek bir veritabanında ADO.NET kullanarak tasarlamayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: seo-lt-2019
ms.topic: tutorial
author: MightyPen
ms.author: genemi
ms.reviewer: carlrab
ms.date: 07/29/2019
ms.openlocfilehash: 0f1140bbefc7508666e763fcd4f1a04ba48cdfdd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75354955"
---
# <a name="tutorial-design-a-relational-database-in-a-single-database-within-azure-sql-database-cx23-and-adonet"></a>Öğretici: Azure SQL Veritabanı C&#x23; ve ADO.NET içinde tek bir veritabanında ilişkisel bir veritabanı tasarlar

Azure SQL Veritabanı, Microsoft Cloud'da (Azure) hizmet olarak ilişkisel bir veritabanıdır (DBaaS). Bu öğreticide, Visual Studio ile ADO.NET ve Azure portalını kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

> [!div class="checklist"]
> * Azure portalını kullanarak tek bir veritabanı oluşturun*
> * Azure portalını kullanarak sunucu düzeyinde BIR IP güvenlik duvarı kuralı ayarlama
> * ADO.NET ve Visual Studio ile veritabanına bağlanma
> * ADO.NET ile tablo oluşturma
> * ADO.NET ile veri ekleme, güncelleştirme ve silme
> * ADO.NET ile veri sorgulama

*Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

> [!TIP]
> Aşağıdaki Microsoft Learn modülü, basit bir veritabanı nın oluşturulması da dahil olmak üzere [bir Azure SQL Veritabanını sorgulayan bir ASP.NET uygulamasının nasıl geliştirilip yapılandırılabildiğini](https://docs.microsoft.com/learn/modules/develop-app-that-queries-azure-sql/)ücretsiz olarak öğrenmenize yardımcı olur.

## <a name="prerequisites"></a>Ön koşullar

[Visual Studio 2019](https://www.visualstudio.com/downloads/) veya sonrası bir kurulum.

## <a name="create-a-blank-single-database"></a>Boş bir tek veritabanı oluşturma

Azure SQL Veritabanı'nda tanımlanan bir işlem ve depolama kaynakları kümesiyle tek bir veritabanı oluşturulur. Veritabanı bir Azure [kaynak grubu](../azure-resource-manager/management/overview.md) içinde oluşturulur ve bir [veritabanı sunucusu](sql-database-servers.md)kullanılarak yönetilir.

Boş bir tek veritabanı oluşturmak için aşağıdaki adımları izleyin.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **Yeni** sayfasında, Azure Market bölümünde **Veritabanları**’nı seçin ve ardından **Öne Çıkan** bölümünde **SQL Veritabanı**’na tıklayın.

   ![create empty-database](./media/sql-database-design-first-database/create-empty-database.png)

3. **Sql Veritabanı** formunu, önceki resimde gösterildiği gibi aşağıdaki bilgilerle doldurun:

    | Ayar       | Önerilen değer | Açıklama |
    | ------------ | ------------------ | ------------------------------------------------- |
    | **Veritabanı adı** | *sizin Veritabanınız* | Geçerli veritabanı adları için [Veritabanı tanımlayıcıları'na](/sql/relational-databases/databases/database-identifiers)bakın. |
    | **Abonelik** | *yourSubscription*  | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
    | **Kaynak grubu** | *sizin ResourceGroup* | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming). |
    | **Kaynağı seçin** | Boş veritabanı | Boş bir veritabanı oluşturulması gerektiğini belirtir. |

4. Varolan bir veritabanı sunucusu kullanmak veya yeni bir veritabanı sunucusu oluşturmak ve yapılandırmak için **Sunucu'ya** tıklayın. Varolan bir sunucuseçin veya **yeni bir sunucu oluştur'u** tıklatın ve **Yeni sunucu** formunu aşağıdaki bilgilerle doldurun:

    | Ayar       | Önerilen değer | Açıklama |
    | ------------ | ------------------ | ------------------------------------------------- |
    | **Sunucu adı** | Genel olarak benzersiz bir ad | Geçerli sunucu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming). |
    | **Sunucu admin giriş** | Geçerli bir ad | Geçerli giriş adları için [Veritabanı tanımlayıcıları'na](/sql/relational-databases/databases/database-identifiers)bakın. |
    | **Parola** | Geçerli bir parola | Parolanızın en az sekiz karakteri olmalı ve aşağıdaki kategorilerden üçündeki karakterleri kullanmalıdır: büyük harf karakterleri, küçük harf karakterleri, sayılar ve alfasayısal olmayan karakterler. |
    | **Konum** | Geçerli bir konum | Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/). |

    ![create database-server](./media/sql-database-design-first-database/create-database-server.png)

5. **Seç'i**tıklatın.
6. Hizmet katmanını, DTU veya sanal çekirdek sayısını ve depolama alanı miktarını belirtmek için **Fiyatlandırma katmanı**’na tıklayın. Her hizmet katmanı için kullanabileceğiniz DTUs/vCore sayının ve depolama alanının seçeneklerini keşfedebilirsiniz.

    Hizmet katmanını, DT'lerin veya vCore'ların sayısını ve depolama miktarını seçtikten sonra **Uygula'yı**tıklatın.

7. Boş veritabanı için **bir Collation** girin (bu öğretici için varsayılan değeri kullanın). Harmanlamalar hakkında daha fazla bilgi için bkz. [Harmanlamalar](/sql/t-sql/statements/collations)

8. **SQL Veritabanı** formunu tamamladığınızda, tek veritabanını sağlamak için **Oluştur'u** tıklatın. Bu adım birkaç dakika sürebilir.

9. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.

   ![bildirim](./media/sql-database-design-first-database/notification.png)

## <a name="create-a-server-level-ip-firewall-rule"></a>Sunucu düzeyinde IP güvenlik duvarı kuralı oluşturma

SQL Veritabanı hizmeti sunucu düzeyinde bir IP güvenlik duvarı oluşturur. Bu güvenlik duvarı, bir güvenlik duvarı kuralı IP'lerinin güvenlik duvarından geçmesine izin vermedikçe harici uygulamaların ve araçların sunucuya ve sunucudaki veritabanlarına bağlanmasını engeller. Tek veritabanınıza harici bağlantı sağlamak için, öncelikle IP adresiniz (veya IP adresi aralığı) için bir IP güvenlik duvarı kuralı eklemeniz gerekir. Bir SQL Veritabanı [sunucu düzeyinde IP güvenlik duvarı kuralı](sql-database-firewall-configure.md)oluşturmak için aşağıdaki adımları izleyin.

> [!IMPORTANT]
> SQL Veritabanı hizmeti port 1433 üzerinden iletişim kurar. Bu hizmete bir şirket ağı içinden bağlanmaya çalışıyorsanız, 1433 bağlantı noktası üzerindeki giden trafiğe ağınızın güvenlik duvarı izin vermeyebilir. Bu nedenle, yöneticiniz 1433 bağlantı noktasını açmadıkça tek veritabanınıza bağlanamazsınız.

1. Dağıtım tamamlandıktan sonra, sol menüden **SQL veritabanlarını** tıklatın ve ardından **SQL veritabanları** sayfasında *Veritabanınızı* tıklatın. Veritabanınız için genel bakış sayfası açılır ve size tam nitelikli **Sunucu adını** *(yourserver.database.windows.net*gibi) gösterir ve daha fazla yapılandırma için seçenekler sunar.

2. SQL Server Management Studio'dan sunucunuza ve veritabanlarınıza bağlanmak için bu tam nitelikli sunucu adını kopyalayın.

   ![sunucu adı](./media/sql-database-design-first-database/server-name.png)

3. Araç çubuğunda **Sunucu güvenlik duvarını ayarla**’ya tıklayın. SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır.

   ![sunucu düzeyinde IP güvenlik duvarı kuralı](./media/sql-database-design-first-database/server-firewall-rule.png)

4. Geçerli IP adresinizi yeni bir IP güvenlik duvarı kuralına eklemek için araç çubuğuna **istemci IP ekle'yi** tıklatın. IP güvenlik duvarı kuralı, tek bir IP adresi veya çeşitli IP adresleri için 1433 bağlantı noktasını açabilir.

5. **Kaydet**'e tıklayın. SQL Veritabanı sunucusunda geçerli IP adresiniz 1433 açma bağlantı noktası için sunucu düzeyinde BIR IP güvenlik duvarı kuralı oluşturulur.

6. **Tamam**’a tıklayın ve sonra **Güvenlik duvarı ayarları** sayfasını kapatın.

IP adresiniz artık IP güvenlik duvarından geçebilir. Artık SQL Server Management Studio'u veya seçtiğiniz başka bir aracı kullanarak tek veritabanınıza bağlanabilirsiniz. Daha önce oluşturduğunuz sunucu yöneticisi hesabını kullandığınızdan emin olun.

> [!IMPORTANT]
> Varsayılan olarak, tüm Azure hizmetleri için SQL Database IP güvenlik duvarı üzerinden erişim sağlanır. Tüm Azure hizmetleri için devre dışı bırakmak isterseniz bu sayfadaki **KAPALI** öğesine tıklayın.

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, veritabanı ve tablolar oluşturma, veritabanına bağlanma, veri yükleme ve sorgu çalıştırma gibi temel veritabanı görevlerini öğrendiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Veritabanı oluşturma
> * Güvenlik duvarı kuralı ayarlama
> * [Visual Studio ve C#](sql-database-connect-query-dotnet-visual-studio.md) ile veritabanına bağlanma
> * Tablo oluşturma
> * Veri ekleme, güncelleştirme, silme ve sorgu

Veri geçişi hakkında bilgi edinmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [DMS kullanarak SQL Server'ı çevrimdışı Azure SQL Veritabanına geçirme](../dms/tutorial-sql-server-to-azure-sql.md)
