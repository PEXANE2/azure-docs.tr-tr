---
title: Olağanüstü durum kurtarma için güvenliği yapılandırma
description: Bir veritabanı geri yüklendikten sonra güvenliği yapılandırmak ve yönetmek için güvenlik konularını veya ikincil bir sunucuya yük devretmeyi öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 12/18/2018
ms.openlocfilehash: 7db83535b7e6257159e0a0eb363e6d05c5e916b9
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84047912"
---
# <a name="configure-and-manage-azure-sql-database-security-for-geo-restore-or-failover"></a>Coğrafi geri yükleme veya yük devretme için Azure SQL veritabanı güvenliğini yapılandırma ve yönetme
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Bu makalede [etkin coğrafi çoğaltma](active-geo-replication-overview.md) ve [otomatik yük devretme gruplarını](auto-failover-group-overview.md)yapılandırmak ve denetlemek için kimlik doğrulama gereksinimleri açıklanmaktadır. Ayrıca, ikincil veritabanına Kullanıcı erişimini ayarlamak için gerekli adımları sağlar. Son olarak, [coğrafi geri yükleme](recovery-using-backups.md#geo-restore)kullanıldıktan sonra kurtarılan veritabanına erişimin nasıl etkinleştirileceğini de açıklar. Kurtarma seçenekleri hakkında daha fazla bilgi için bkz. [Iş sürekliliği genel bakış](business-continuity-high-availability-disaster-recover-hadr-overview.md).

## <a name="disaster-recovery-with-contained-users"></a>İçerilen kullanıcılarla olağanüstü durum kurtarma

Ana veritabanında oturum açma işlemlerine eşlenmesi gereken geleneksel kullanıcıların aksine, kapsanan bir kullanıcı tamamen veritabanının kendisi tarafından yönetilir. Bunun iki avantajı vardır. Olağanüstü durum kurtarma senaryosunda, kullanıcılar yeni birincil veritabanına bağlanmaya devam edebilir veya veritabanı kullanıcıları yönettiği için ek bir yapılandırma olmadan coğrafi geri yükleme kullanarak Kurtarılan veritabanı. Bu yapılandırmadan bir oturum açma perspektifinden de potansiyel ölçeklenebilirlik ve performans avantajları vardır. Daha fazla bilgi için bkz. [Bağımsız Veritabanı Kullanıcıları - Veritabanınızı Taşınabilir Hale Getirme](https://msdn.microsoft.com/library/ff929188.aspx).

Ana ticaret, olağanüstü durum kurtarma işleminin ölçekteki yönetiminde daha zor bir işlemdir. Aynı oturum açma kullanan birden fazla veritabanınız varsa, birden fazla veritabanında kapsanan kullanıcıları kullanarak kimlik bilgilerinin saklanması, kapsanan kullanıcıların avantajlarından faydalanmayabilir. Örneğin, parola döndürme ilkesi, ana veritabanında oturum açma parolasını değiştirmek yerine birden çok veritabanında sürekli olarak değişiklikler yapılmasını gerektirir. Bu nedenle, aynı kullanıcı adını ve parolayı kullanan birden fazla veritabanınız varsa, içerilen kullanıcıların kullanılması önerilmez.

## <a name="how-to-configure-logins-and-users"></a>Oturum açmaları ve kullanıcıları yapılandırma

Oturumlar ve kullanıcılar (kapsanan kullanıcılar yerine) kullanıyorsanız, ana veritabanında aynı oturum açma bilgilerinin mevcut olduğundan emin olmak için ek adımlar gerçekleştirmeniz gerekir. Aşağıdaki bölümlerde, ilgili adımlar ve ek konular ana hatlarıyla verilmiştir.

  >[!NOTE]
  > Veritabanlarınızı yönetmek için Azure Active Directory (AAD) oturum açmaları kullanmak da mümkündür. Daha fazla bilgi için bkz. [Azure SQL oturum açmaları ve kullanıcıları](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).

### <a name="set-up-user-access-to-a-secondary-or-recovered-database"></a>İkincil veya kurtarılan bir veritabanına Kullanıcı erişimini ayarlama

İkincil veritabanının salt okunurdur bir ikincil veritabanı olarak kullanılabilmesi ve yeni birincil veritabanına veya coğrafi geri yükleme kullanılarak kurtarılan veritabanına doğru erişimin sağlandığından emin olmak için hedef sunucunun ana veritabanı, kurtarma işleminden önce uygun güvenlik yapılandırmasına sahip olmalıdır.

Her adımın belirli izinleri, bu konunun ilerleyen kısımlarında açıklanmıştır.

Coğrafi çoğaltma için Kullanıcı erişiminin hazırlanması, coğrafi çoğaltma yapılandırma bölümü olarak gerçekleştirilmelidir. Coğrafi olarak geri yüklenen veritabanlarına Kullanıcı erişiminin hazırlanması, özgün sunucu çevrimiçi olduğunda (örneğin, DR detayının bir parçası olarak) herhangi bir zamanda gerçekleştirilmelidir.

> [!NOTE]
> Yük devretmek veya düzgün şekilde yapılandırılmamış bir sunucuya coğrafi geri yükleme yaparsanız, sunucuya erişim sunucu yöneticisi hesabıyla sınırlı olur.

Hedef sunucuda oturum açma ayarları aşağıda belirtilen üç adımdan oluşur:

#### <a name="1-determine-logins-with-access-to-the-primary-database"></a>1. birincil veritabanına erişimi olan oturum açmaları belirleme

İşlemin ilk adımı, hedef sunucuda hangi oturum açmaları tekrarlamak gerektiğini belirlemektir. Bu, biri kaynak sunucuda ve diğeri birincil veritabanında bulunan mantıksal ana veritabanında bulunan bir çift SELECT deyimi ile gerçekleştirilir.

Yalnızca sunucu yöneticisi veya **loginmanager** sunucu rolünün bir üyesi, kaynak sunucudaki oturum AÇMALARı aşağıdaki SELECT ifadesiyle belirleyebilir.

    SELECT [name], [sid]
    FROM [sys].[sql_logins]
    WHERE [type_desc] = 'SQL_Login'

Yalnızca db_owner veritabanı rolünün bir üyesi, dbo kullanıcısı veya Sunucu Yöneticisi, birincil veritabanındaki tüm veritabanı kullanıcı sorumlularını tespit edebilir.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

#### <a name="2-find-the-sid-for-the-logins-identified-in-step-1"></a>2. Adım 1 ' de tanımlanan oturum açma SID 'sini bulun

Önceki bölümden ve SID 'Lerle eşleşen sorguların çıkışını karşılaştırarak sunucu oturum açma bilgilerini veritabanı kullanıcısına eşleyebilirsiniz. Eşleşen SID 'si olan bir veritabanı kullanıcısına sahip olan oturumlarda, bu veritabanı kullanıcı sorumlusu olarak bu veritabanına Kullanıcı erişimi vardır.

Aşağıdaki sorgu, tüm kullanıcı sorumlularını ve bunların SID 'Lerini bir veritabanında görmek için kullanılabilir. Bu sorguyu yalnızca db_owner veritabanı rolü veya Sunucu Yöneticisi üyesi çalıştırabilir.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

> [!NOTE]
> **INFORMATION_SCHEMA** ve **sys** kullanıcıları *null* SID 'lere sahiptir ve **Konuk** SID değeri **0x00**olur. Veritabanı Oluşturucu **DBManager**üyesi yerine sunucu yöneticisi ise, **dbo** SID 'si *0x01060000000001648000000000048454*ile başlayabilir.

#### <a name="3-create-the-logins-on-the-target-server"></a>3. oturum açma bilgilerini hedef sunucuda oluşturun

Son adım hedef sunucuya veya sunuculara gidececektir ve uygun SID 'Ler ile oturum açmaları oluşturacaktır. Temel sözdizimi aşağıdaki gibidir.

    CREATE LOGIN [<login name>]
    WITH PASSWORD = <login password>,
    SID = <desired login SID>

> [!NOTE]
> Birincil sunucuya kullanıcı erişimi vermek, ancak birinciye değil ise, birincil sunucudaki kullanıcı oturum açma bilgilerini aşağıdaki sözdizimini kullanarak değiştirerek bunu yapabilirsiniz.
>
> ```sql
> ALTER LOGIN <login name> DISABLE
> ```
>
> DEVRE DıŞı bırak, parolayı değiştirmez, bu nedenle gerekirse her zaman etkinleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Veritabanı erişimini ve oturum açma bilgilerini yönetme hakkında daha fazla bilgi için bkz. [SQL veritabanı güvenliği: veritabanı erişimini yönetme ve oturum açma güvenliği](logins-create-manage.md).
* Kapsanan veritabanı kullanıcıları hakkında daha fazla bilgi için bkz. [Kapsanan Veritabanı kullanıcıları-veritabanınızı taşınabilir hale getirme](https://msdn.microsoft.com/library/ff929188.aspx).
* Etkin coğrafi çoğaltma hakkında bilgi edinmek için bkz. [etkin coğrafi çoğaltma](active-geo-replication-overview.md).
* Otomatik yük devretme grupları hakkında bilgi edinmek için bkz. [otomatik yük devretme grupları](auto-failover-group-overview.md).
* Coğrafi geri yükleme kullanma hakkında daha fazla bilgi için bkz. [coğrafi geri yükleme](recovery-using-backups.md#geo-restore)
