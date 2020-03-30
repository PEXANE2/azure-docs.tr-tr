---
title: Olağanüstü durum kurtarma için güvenliği yapılandırma
description: Veritabanı geri yüklemesi veya ikincil bir sunucuya bir hata dan sonra güvenliği yapılandırmak ve yönetmek için güvenlik hususlarını öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 12/18/2018
ms.openlocfilehash: 9d628583168883276e67d9e2f2fcafdce292769e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73807491"
---
# <a name="configure-and-manage-azure-sql-database-security-for-geo-restore-or-failover"></a>Azure SQL Veritabanı güvenliğini coğrafi geri yükleme veya başarısızlık için yapılandırma ve yönetme

Bu makalede, yapılandırma ve [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) ve otomatik [failover grupları](sql-database-auto-failover-group.md)denetlemek için kimlik doğrulama gereksinimleri açıklanır. Ayrıca, ikincil veritabanına kullanıcı erişimi ayarlamak için gereken adımları da sağlar. Son olarak, [coğrafi geri yükleme](sql-database-recovery-using-backups.md#geo-restore)kullandıktan sonra kurtarılan veritabanına erişimi etkinleştirmek için nasıl açıklanır. Kurtarma seçenekleri hakkında daha fazla bilgi için İş [Sürekliliğine Genel Bakış'a](sql-database-business-continuity.md)bakın.

## <a name="disaster-recovery-with-contained-users"></a>İçerdiği kullanıcılarla olağanüstü durum kurtarma

Ana veritabanında oturum açmak için eşlenen geleneksel kullanıcıların aksine, içerdiği kullanıcı tamamen veritabanının kendisi tarafından yönetilir. Bunun iki faydası var. Olağanüstü durum kurtarma senaryosunda, veritabanı kullanıcıları yönettiğinden, kullanıcılar yeni birincil veritabanına veya ek yapılandırma olmadan coğrafi geri yükleme kullanılarak kurtarılan veritabanına bağlanmaya devam edebilir. Bu yapılandırmadan giriş açısından potansiyel ölçeklenebilirlik ve performans avantajları da vardır. Daha fazla bilgi için bkz. [Bağımsız Veritabanı Kullanıcıları - Veritabanınızı Taşınabilir Hale Getirme](https://msdn.microsoft.com/library/ff929188.aspx).

Ana denge, olağanüstü durum kurtarma sürecini ölçekte yönetmenin daha zor olmasıdır. Aynı girişi kullanan birden çok veritabanınız varsa, birden çok veritabanında bulunan kullanıcıları kullanarak kimlik bilgilerini korumak, içerdiği kullanıcıların yararlarını inkâr edebilir. Örneğin, parola döndürme ilkesi, ana veritabanında oturum açma parolasını değiştirmek yerine sürekli olarak birden çok veritabanında değişiklik yapılmasını gerektirir. Bu nedenle, aynı kullanıcı adı ve parolayı kullanan birden çok veritabanınız varsa, içerdiği kullanıcıları kullanmanız önerilmez.

## <a name="how-to-configure-logins-and-users"></a>Oturum açma ve kullanıcıları yapılandırma

Girişler ve kullanıcılar kullanıyorsanız (içerdiği kullanıcılar yerine), ana veritabanında aynı girişlerin olduğundan emin olmak için ek adımlar atmalısınız. Aşağıdaki bölümlerde ilgili adımlar ve ek hususlar sıralar.

  >[!NOTE]
  > Veritabanlarınızı yönetmek için Azure Active Directory (AAD) girişlerini de kullanabilirsiniz. Daha fazla bilgi için [Azure SQL girişleri ve kullanıcıları](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins)için bkz.

### <a name="set-up-user-access-to-a-secondary-or-recovered-database"></a>İkincil veya kurtarılan bir veritabanına kullanıcı erişimi ayarlama

İkincil veritabanının salt okunur ikincil veritabanı olarak kullanılabilir olması ve yeni birincil veritabanına veya coğrafi geri yükleme kullanılarak kurtarılan veritabanına doğru erişimi sağlayabilmek için, hedef sunucunun ana veritabanının uygun güvenliğe sahip olması gerekir kurtarma dan önce yerinde yapılandırma.

Her adım için belirli izinler daha sonra bu konuda açıklanmıştır.

Bir coğrafi çoğaltma ikincil kullanıcı erişimi hazırlama parçası coğrafi çoğaltma yapılandırma olarak yapılmalıdır. Coğrafi olarak geri yüklenen veritabanlarına kullanıcı erişiminin hazırlanması, orijinal sunucu çevrimiçi olduğunda (örneğin DR matkabının bir parçası olarak) herhangi bir zamanda yapılmalıdır.

> [!NOTE]
> Düzgün yapılandırılmış oturum açmaları olmayan bir sunucuda başarısız olurveya coğrafi olarak geri yüklenirseniz, sunucu yöneticisi hesabıyla sınırlı olacaktır.

Hedef sunucuda oturum açma ların ayarlanması aşağıda özetlenen üç adımdan oluşur:

#### <a name="1-determine-logins-with-access-to-the-primary-database"></a>1. Birincil veritabanına erişim ile girişleri belirleme

İşlemin ilk adımı, hedef sunucuda hangi girişlerin çoğaltılması gerektiğini belirlemektir. Bu, biri kaynak sunucudaki mantıksal ana veritabanında ve diğeri birincil veritabanının kendisinde olmak üzere bir çift SELECT deyimiyle gerçekleştirilir.

Kaynak sunucudaki girişleri aşağıdaki SELECT deyimiyle yalnızca sunucu yöneticisi veya **LoginManager** sunucu rolünün bir üyesi belirleyebilir.

    SELECT [name], [sid]
    FROM [sys].[sql_logins]
    WHERE [type_desc] = 'SQL_Login'

Yalnızca db_owner veritabanı rolünün bir üyesi, dbo kullanıcısı veya sunucu yöneticisi, birincil veritabanındaki tüm veritabanı kullanıcı ilkelerini belirleyebilir.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

#### <a name="2-find-the-sid-for-the-logins-identified-in-step-1"></a>2. Adım 1'de tanımlanan oturum açmalar için SID'yi bulun

Önceki bölümdeki sorguçıktılarını karşılaştırarak ve SID'lerle eşleştirerek, sunucu oturum açmayı veritabanı kullanıcısıyla eşleyebilirsiniz. Eşleşen bir SID'ye sahip bir veritabanı kullanıcısı olan oturum açmalar, veritabanı kullanıcı ilkesi olarak bu veritabanına kullanıcı erişimine sahiptir.

Aşağıdaki sorgu, tüm kullanıcı ilkelerini ve SIT'lerini bir veritabanında görmek için kullanılabilir. Bu sorguyu yalnızca db_owner veritabanı rolünün bir üyesi veya sunucu yöneticisi çalıştırabilir.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

> [!NOTE]
> **INFORMATION_SCHEMA** ve **sys** kullanıcıları *NULL* SID'lere sahiptir ve **konuk** SID **0x00'dir.** **Dbo** SID *0x010600000000001648000000048454*ile başlayabilir , veritabanı oluşturucu **dbManager**üyesi yerine sunucu yöneticisi ise .

#### <a name="3-create-the-logins-on-the-target-server"></a>3. Hedef sunucudaki girişleri oluşturma

Son adım, hedef sunucuya veya sunuculara gitmek ve uygun SID'lerle giriş yapmaktır. Temel sözdizimi aşağıdaki gibidir.

    CREATE LOGIN [<login name>]
    WITH PASSWORD = <login password>,
    SID = <desired login SID>

> [!NOTE]
> Kullanıcıya ikincil erişim vermek istiyorsanız, ancak birincil ekive vermek istemiyorsanız, bunu aşağıdaki sözdizimini kullanarak birincil sunucudaki kullanıcı oturum açmasını değiştirerek yapabilirsiniz.
>
> ```sql
> ALTER LOGIN <login name> DISABLE
> ```
>
> DISABLE parolayı değiştirmez, bu nedenle gerekirse her zaman etkinleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Veritabanı erişimini ve oturum açmaları yönetme hakkında daha fazla bilgi için [SQL Veritabanı güvenliği: Veritabanı erişimini ve oturum açma güvenliğini yönetin.](sql-database-manage-logins.md)
* İçerdiği veritabanı kullanıcıları hakkında daha fazla bilgi için, [İçe Bilgi Verilen Veritabanı Kullanıcıları - Veritabanınızı Taşınabilir Yapma](https://msdn.microsoft.com/library/ff929188.aspx)' ya bakın.
* Etkin coğrafi çoğaltma hakkında bilgi edinmek için [Etkin coğrafi çoğaltma'ya](sql-database-active-geo-replication.md)bakın.
* Otomatik hata grupları hakkında bilgi edinmek için Otomatik [başarısız grupları'na](sql-database-auto-failover-group.md)bakın.
* Coğrafi geri yükleme kullanımı hakkında bilgi için [bkz.](sql-database-recovery-using-backups.md#geo-restore)
