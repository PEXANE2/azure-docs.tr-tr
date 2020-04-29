---
title: Azure Active Directory
description: SQL veritabanı, yönetilen örnek ve Azure SYNAPSE Analytics ile kimlik doğrulaması için Azure Active Directory kullanma hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: azure-synapse
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto, carlrab
ms.date: 03/27/2020
ms.openlocfilehash: 58f40bc7406048df2b052bea799bcbf6466fbbae
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80421100"
---
# <a name="use-azure-active-directory-authentication-for-authentication-with-sql"></a>SQL ile kimlik doğrulaması için Azure Active Directory kimlik doğrulaması kullanma

Azure Active Directory kimlik doğrulaması, Azure [SQL veritabanı](sql-database-technical-overview.md), [yönetilen örnek](sql-database-managed-instance.md)ve [Azure SYNAPSE Analytics (eskiden Azure sql veri ambarı)](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) ile Azure Active Directory (Azure AD) kimlikleri kullanılarak bağlantı kurma mekanizmasıdır. 

> [!NOTE]
> Bu makale, Azure SQL Server ve hem SQL veritabanı hem de Azure SYNAPSE için geçerlidir. Basitlik için SQL veritabanı hem SQL veritabanı hem de Azure SYNAPSE 'a başvurulduğunda kullanılır.

Azure AD kimlik doğrulamasıyla, veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin kimliklerini tek bir konumda merkezi olarak yönetebilirsiniz. Merkezi kimlik yönetimi, tüm veritabanı kullanıcılarını aynı yerden yönetmenizi sağlar ve izin yönetimini kolaylaştırır. Avantajları şunlardır:

- SQL Server kimlik doğrulamasına bir alternatif sağlar
- Veritabanı sunucuları arasında kullanıcı kimliklerinin uzamasını durdurmaya yardımcı olur.
- Tek konumda parola dönüşüne olanak sağlar
- Müşteriler dış (Azure AD) grupları kullanarak veritabanı izinlerini yönetebilir
- Tümleşik Windows kimlik doğrulamasına ve Azure Active Directory tarafından desteklenen diğer kimlik doğrulama biçimlerine olanak sağlayarak parola depolamayı ortadan kaldırabilir
- Azure AD kimlik doğrulaması, kimlikleri veritabanı düzeyinde doğrulamak için bağımsız veritabanı kullanıcılarını kullanır
- Azure AD SQL Veritabanı'na bağlanan uygulamalar için belirteç tabanlı kimlik doğrulamasını destekler
- Azure AD kimlik doğrulaması şunları destekler:
  - Yalnızca Azure AD bulutu kimlikleri
  - Azure AD karma kimlikleri şunları destekler:
    - Sorunsuz çoklu oturum açma (SSO) ile birlikte iki seçenekten oluşan bulut kimlik doğrulaması: **doğrudan** kimlik doğrulama ve **Parola karması** kimlik doğrulaması
    - Federal kimlik doğrulaması
  - Azure AD kimlik doğrulama yöntemleri ve aralarından seçim yapabileceğiniz daha fazla bilgi için aşağıdaki makaleye bakın:
    - [Azure Active Directory karma kimlik çözümünüz için doğru kimlik doğrulama yöntemini seçin](../active-directory/hybrid/choose-ad-authn.md)
- Azure AD, Multi-Factor Authentication (MFA) içeren Active Directory Evrensel Kimlik Doğrulaması'nın kullanıldığı SQL Server Management Studio'dan gelen bağlantıları destekler.  MFA telefon görüşmesi, SMS mesajı, pin ile akıllı kart veya mobil uygulama bildirimi gibi çok sayıda kolay doğrulama seçeneğine sahiptir. Daha fazla bilgi için bkz. [SQL veritabanı ve Azure SYNAPSE Ile Azure AD MFA Için SSMS desteği](sql-database-ssms-mfa-authentication.md)
- Azure AD, Active Directory Etkileşimli Kimlik Doğrulaması'nın kullanıldığı SQL Server Veri Araçları'ndan (SSDT) gelen benzer bağlantıları destekler. Daha fazla bilgi için bkz. [SQL Server Veri Araçları Azure Active Directory desteği (SSDT)](/sql/ssdt/azure-active-directory)

> [!NOTE]  
> Bir Azure VM üzerinde çalışan SQL Server bağlanmak Azure Active Directory hesabı kullanılarak desteklenmez. Bunun yerine bir etki alanı Active Directory hesabını kullanın.  

Yapılandırma adımları Azure Active Directory kimlik doğrulaması yapılandırmak ve kullanmak için aşağıdaki yordamları içerir.

1. Azure AD 'yi oluşturun ve doldurun.
2. İsteğe bağlı: Azure aboneliğinizle ilişkili olan Active Directory 'yi Ilişkilendirin veya değiştirin.
3. Azure SQL veritabanı sunucusu, yönetilen örnek veya [Azure SYNAPSE](https://azure.microsoft.com/services/sql-data-warehouse/)için Azure Active Directory Yöneticisi oluşturun.
4. İstemci bilgisayarlarınızı yapılandırın.
5. Veritabanınızda Azure AD kimlikleriyle eşlenmiş kapsanan veritabanı kullanıcıları oluşturun.
6. Azure AD kimliklerini kullanarak veritabanınıza bağlanın.

> [!NOTE]
> Azure AD 'yi oluşturma ve doldurma hakkında bilgi edinmek ve Azure AD 'yi Azure SQL veritabanı, yönetilen örnek ve Azure SYNAPSE ile yapılandırma hakkında bilgi edinmek için bkz. Azure [ad 'Yi Azure SQL veritabanı Ile yapılandırma](sql-database-aad-authentication-configure.md).

## <a name="trust-architecture"></a>Güven mimarisi

- Azure AD yerel kullanıcı parolasını desteklemek için yalnızca bulut bölümü ve Azure AD/Azure SQL veritabanı kabul edilir.
- Windows çoklu oturum açma kimlik bilgilerini (veya Windows kimlik bilgileri için Kullanıcı/parola) desteklemek için, geçişli ve parola karması kimlik doğrulaması için sorunsuz çoklu oturum açma için yapılandırılmış bir Federasyon veya yönetilen etki alanından Azure Active Directory kimlik bilgilerini kullanın. Daha fazla bilgi için bkz. [kesintisiz çoklu oturum açma Azure Active Directory](../active-directory/hybrid/how-to-connect-sso.md).
- Federal kimlik doğrulamasını (veya Windows kimlik bilgileri için Kullanıcı/parola) desteklemek için, ADFS bloğundan iletişim gerekir.

Azure AD karma kimlikleri, kurulum ve eşitleme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- Parola karması kimlik doğrulaması- [Azure AD Connect eşitleme ile parola karması eşitlemesi uygulama](../active-directory/hybrid/how-to-connect-password-hash-synchronization.md)
- Geçişli kimlik doğrulaması- [Azure Active Directory geçişli kimlik doğrulaması](../active-directory/hybrid/how-to-connect-pta-quick-start.md)
- Federal kimlik doğrulaması- [Azure 'da Active Directory Federasyon Hizmetleri (AD FS) dağıtma](/windows-server/identity/ad-fs/deployment/how-to-connect-fed-azure-adfs) ve [Azure AD Connect ve Federasyon](../active-directory/hybrid/how-to-connect-fed-whatis.md)

ADFS altyapısıyla (veya Windows kimlik bilgileri için Kullanıcı/parola) örnek bir Federasyon kimlik doğrulaması için aşağıdaki diyagrama bakın. Oklar, iletişim yollarını gösterir.

![aad kimlik doğrulama diyagramı][1]

Aşağıdaki diyagramda, bir istemcinin bir belirteci göndererek bir veritabanına bağlanmasına izin veren Federasyon, güven ve barındırma ilişkileri belirtilmektedir. Belirtecin kimliği bir Azure AD tarafından doğrulanır ve veritabanı tarafından güvenilir. Müşteri 1, yerel kullanıcılar veya bir Azure AD ile federe kullanıcılarla bir Azure Active Directory temsil edebilir. Müşteri 2, içeri aktarılan kullanıcılar dahil olası bir çözümü temsil eder; Bu örnekte, ADFS ile Azure Active Directory eşitlenmiş bir federal Azure Active Directory geliyor. Azure AD kimlik doğrulaması kullanarak bir veritabanına erişimin, barındırma aboneliğinin Azure AD ile ilişkilendirilmesini gerektirdiğini anlamak önemlidir. Azure SQL veritabanını veya Azure SYNAPSE 'i barındıran SQL Server oluşturmak için aynı aboneliğin kullanılması gerekir.

![Abonelik ilişkisi][2]

## <a name="administrator-structure"></a>Yönetici yapısı

Azure AD kimlik doğrulaması kullanırken, SQL veritabanı sunucusu ve yönetilen örnek için iki yönetici hesabı bulunur; özgün SQL Server Yöneticisi ve Azure AD yöneticisi. Aynı kavramlar Azure SYNAPSE için de geçerlidir. Yalnızca bir Azure AD hesabını temel alan yönetici, bir kullanıcı veritabanında ilk Azure AD kapsanan veritabanı kullanıcısını oluşturabilir. Azure AD yönetici oturumu, bir Azure AD kullanıcısı veya bir Azure AD grubu olabilir. Yönetici bir grup hesabı olduğunda, SQL Server örneği için birden çok Azure AD yöneticisini etkinleştirerek herhangi bir grup üyesi tarafından kullanılabilir. Grup hesabını yönetici olarak kullanmak, SQL veritabanında kullanıcıları veya izinleri değiştirmeden Azure AD 'de grup üyelerini merkezi olarak eklemenize ve kaldırmanıza olanak tanıyarak yönetilebilirlik geliştirir. Herhangi bir anda yalnızca bir Azure AD Yöneticisi (bir kullanıcı veya grup) yapılandırılabilir.

![Yönetici yapısı][3]

## <a name="permissions"></a>İzinler

Yeni kullanıcılar oluşturmak için, veritabanında `ALTER ANY USER` izninizin olması gerekir. Herhangi `ALTER ANY USER` bir veritabanı kullanıcısına izin verilebilir. Ayrıca `ALTER ANY USER` , bu izin Sunucu Yöneticisi hesapları tarafından ve bu veritabanı için `CONTROL ON DATABASE` veya `ALTER ON DATABASE` iznine sahip veritabanı kullanıcıları ve `db_owner` veritabanı rolünün üyeleri tarafından tutulur.

Azure SQL veritabanı, yönetilen örnek veya Azure SYNAPSE içinde kapsanan bir veritabanı kullanıcısı oluşturmak için, bir Azure AD kimliği kullanarak veritabanına veya örneğe bağlanmanız gerekir. İlk kapsanan veritabanı kullanıcısını oluşturmak için, bir Azure AD Yöneticisi (veritabanının sahibi olan) kullanarak veritabanına bağlanmanız gerekir. Bu, [Azure Active Directory kimlik DOĞRULAMASıNı SQL veritabanı veya Azure SYNAPSE Ile yapılandırma ve yönetme](sql-database-aad-authentication-configure.md)bölümünde gösterilmiştir. Tüm Azure AD kimlik doğrulaması yalnızca Azure AD yöneticisi Azure SQL veritabanı veya Azure SYNAPSE için oluşturulduysa mümkündür. Azure Active Directory Yöneticisi sunucudan kaldırılmışsa, daha önce SQL Server içinde oluşturulan mevcut Azure Active Directory Kullanıcılar artık Azure Active Directory kimlik bilgilerini kullanarak veritabanına bağlanamaz.

## <a name="azure-ad-features-and-limitations"></a>Azure AD özellikleri ve sınırlamaları

- Azure AD 'nin aşağıdaki üyeleri Azure SQL Server veya Azure SYNAPSE 'da sağlanabilir:

  - Yerel Üyeler: yönetilen etki alanında veya bir müşteri etki alanında Azure AD 'de oluşturulan bir üye. Daha fazla bilgi için bkz. [Azure AD 'ye kendi etki alanı adınızı ekleme](../active-directory/active-directory-domains-add-azure-portal.md).
  - Yönetilen bir etki alanında, geçişli veya parola karması kimlik doğrulamasıyla sorunsuz çoklu oturum açma için yapılandırılmış Azure Active Directory ile federe Active Directory etki alanı üyeleri. Daha fazla bilgi için bkz. [Microsoft Azure artık Windows Server Active Directory ile Federasyonu destekliyor](https://azure.microsoft.com/blog/windows-azure-now-supports-federation-with-windows-server-active-directory//) ve [sorunsuz çoklu oturum açma Azure Active Directory](../active-directory/hybrid/how-to-connect-sso.md).
  - Diğer Azure AD 'den, yerel veya Federasyon etki alanı üyesi olan Üyeler içeri aktarıldı.
  - Güvenlik grupları olarak oluşturulan grupları Active Directory.

- Sunucu rolüne sahip bir grubun parçası olan Azure AD kullanıcıları, `db_owner` Azure SQL veritabanı ve Azure SYNAPSE 'de **[VERITABANı kapsamlı kimlik bilgisi oluştur](/sql/t-sql/statements/create-database-scoped-credential-transact-sql)** sözdizimini kullanamaz. Aşağıdaki hatayı görürsünüz:

    `SQL Error [2760] [S0001]: The specified schema name 'user@mydomain.com' either does not exist or you do not have permission to use it.`

    `db_owner` **VERITABANı kapsamlı kimlik bilgileri oluşturma** sorununu hafifletmek için role doğrudan tek bir Azure AD kullanıcısına izin verin.

- Bu sistem işlevleri, Azure AD sorumluları altında yürütüldüğünde NULL değerler döndürüyor:

  - `SUSER_ID()`
  - `SUSER_NAME(<admin ID>)`
  - `SUSER_SNAME(<admin SID>)`
  - `SUSER_ID(<admin name>)`
  - `SUSER_SID(<admin name>)`

### <a name="managed-instances"></a>Yönetilen örnekler

- Azure AD Server sorumluları (oturumlar) ve kullanıcılar, [yönetilen örnekler](sql-database-managed-instance.md)için Önizleme özelliği olarak desteklenir.
- Bir Azure AD grubuna eşlenen Azure AD Server sorumlularını (oturum açma) veritabanı sahibi olarak ayarlama, [yönetilen örneklerde](sql-database-managed-instance.md)desteklenmez.
    - Bunun bir uzantısı, bir grup `dbcreator` sunucu rolünün bir parçası olarak eklendiğinde, bu gruptaki kullanıcılar yönetilen örneğe bağlanabilir ve yeni veritabanları oluşturabilir, ancak veritabanına erişemeyecektir. Bunun nedeni, yeni veritabanı sahibinin Azure AD kullanıcısı değil, SA. Bu sorun, bireysel kullanıcı `dbcreator` sunucu rolüne eklenirse bildirim yapmaz.
- SQL Aracısı yönetimi ve işlerin yürütülmesi, Azure AD Server sorumluları (oturumlar) için desteklenir.
- Veritabanı yedekleme ve geri yükleme işlemleri, Azure AD Server sorumluları (oturumlar) tarafından yürütülebilir.
- Azure AD Server sorumluları (oturum açmalar) ve kimlik doğrulama olayları ile ilgili tüm deyimlerin denetlenmesi desteklenir.
- Sysadmin sunucu rolünün üyeleri olan Azure AD Server sorumluları (oturumlar) için adanmış yönetici bağlantısı desteklenir.
    - SQLCMD yardımcı programı ve SQL Server Management Studio aracılığıyla desteklenir.
- Oturum açma Tetikleyicileri, Azure AD Server sorumlularına (oturum açmalar) gelen oturum açma olayları için desteklenir.
- Hizmet Aracısı ve DB Mail, bir Azure AD sunucusu sorumlusu (oturum açma) kullanılarak ayarlanabilir.


## <a name="connecting-using-azure-ad-identities"></a>Azure AD kimliklerini kullanarak bağlanma

Azure Active Directory kimlik doğrulaması, Azure AD kimliklerini kullanarak bir veritabanına bağlanmak için aşağıdaki yöntemleri destekler:

- Azure Active Directory parolası
- Azure Active Directory tümleşik
- MFA ile evrensel Azure Active Directory
- Uygulama belirteci kimlik doğrulamasını kullanma

Aşağıdaki kimlik doğrulama yöntemleri Azure AD Server sorumluları (oturum açmalar) için desteklenir:

- Azure Active Directory parolası
- Azure Active Directory tümleşik
- MFA ile evrensel Azure Active Directory


### <a name="additional-considerations"></a>Diğer konular

- Yönetilebilirlik geliştirmek için, bir yönetici olarak adanmış bir Azure AD grubu sağlamanızı öneririz.   
- Azure SQL veritabanı sunucusu veya Azure SYNAPSE için herhangi bir anda yalnızca bir Azure AD Yöneticisi (bir kullanıcı veya grup) yapılandırılabilir.
  - Yönetilen örnekler için Azure AD Server sorumlularını (oturum açma) ekleme, `sysadmin` role eklenebilen birden çok Azure AD Server sorumlusu (oturum açma) oluşturma olasılığa izin verir.
- SQL Server için yalnızca bir Azure AD yöneticisi, Azure Active Directory bir hesabı kullanarak başlangıçta Azure SQL veritabanı sunucusuna, yönetilen örneğe veya Azure SYNAPSE 'e bağlanabilir. Active Directory Yöneticisi, sonraki Azure AD veritabanı kullanıcılarını yapılandırabilir.   
- Bağlantı zaman aşımını 30 saniyeye ayarlamayı öneririz.   
- SQL Server 2016 Management Studio ve SQL Server Veri Araçları Visual Studio 2015 için (sürüm 14.0.60311.1 Nisan 2016 veya üzeri) Azure Active Directory kimlik doğrulamasını destekler. (Azure AD kimlik doğrulaması, **SqlServer için .NET Framework veri sağlayıcısı**tarafından desteklenir; en az sürüm .NET Framework 4,6). Bu nedenle, bu araçların ve veri katmanı uygulamalarının en yeni sürümleri (DAC ve. BACPAC), Azure AD kimlik doğrulaması kullanabilir.   
- Version 15.0.1 sürümünden itibaren [sqlcmd yardımcı](/sql/tools/sqlcmd-utility) programı ve [bcp](/sql/tools/bcp-utility) ile etkileşimli kimlik doğrulama desteği Active Directory.
- Visual Studio 2015 için SQL Server Veri Araçları, en az veri araçları 'nın 2016 Nisan sürümü (sürüm 14.0.60311.1) gerektirir. Şu anda Azure AD kullanıcıları SSDT Nesne Gezgini gösterilmez. Geçici bir çözüm olarak, [sys. database_principals](https://msdn.microsoft.com/library/ms187328.aspx)içindeki kullanıcıları görüntüleyin.   
- [SQL Server Için MICROSOFT JDBC sürücüsü 6,0](https://www.microsoft.com/download/details.aspx?id=11774) , Azure AD kimlik doğrulamasını destekler. Ayrıca bkz. [bağlantı özelliklerini ayarlama](https://msdn.microsoft.com/library/ms378988.aspx).   
- PolyBase, Azure AD kimlik doğrulamasını kullanarak kimlik doğrulaması yapamaz.   
- Azure AD kimlik doğrulaması, SQL veritabanı için Azure portal **Içeri aktarma veritabanı** ve **dışarı aktarma veritabanı** dikey pencereleri tarafından desteklenir. Azure AD kimlik doğrulaması kullanılarak içeri ve dışarı aktarma, PowerShell komutundan de desteklenir.   
- CLı kullanarak SQL veritabanı, yönetilen örnek ve Azure SYNAPSE için Azure AD kimlik doğrulaması desteklenir. Daha fazla bilgi için bkz. [SQL veritabanı veya Azure SYNAPSE ile Azure Active Directory kimlik doğrulamasını yapılandırma ve yönetme](sql-database-aad-authentication-configure.md) ve [SQL Server-az SQL Server](https://docs.microsoft.com/cli/azure/sql/server).

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD oluşturma ve doldurma hakkında bilgi edinmek ve Azure AD 'yi Azure SQL veritabanı veya Azure SYNAPSE ile yapılandırmak için bkz. [SQL veritabanı, yönetilen örnek veya Azure SYNAPSE ile Azure Active Directory kimlik doğrulamasını yapılandırma ve yönetme](sql-database-aad-authentication-configure.md).
- Yönetilen örneklerle Azure AD Server sorumlularını (oturum açma) kullanmanın bir öğreticisi için bkz. [yönetilen örneklerle Azure ad sunucu sorumluları (oturumlar)](sql-database-managed-instance-aad-security-tutorial.md)
- SQL veritabanında oturum açma bilgileri, kullanıcılar, veritabanı rolleri ve izinlere genel bakış için bkz. [oturum açma bilgileri, kullanıcılar, veritabanı rolleri ve izinler](sql-database-manage-logins.md).
- Veritabanı sorumluları hakkında daha fazla bilgi için bkz. [Sorumlular](https://msdn.microsoft.com/library/ms181127.aspx).
- Veritabanı rolleri hakkında daha fazla bilgi için bkz. [Veritabanı rolleri](https://msdn.microsoft.com/library/ms189121.aspx).
- Yönetilen örnekler için Azure AD Server sorumlularını (oturum açma) oluşturma hakkında sözdizimi için bkz. [oturum oluşturma](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current).
- SQL Veritabanındaki güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz. [SQL Veritabanı güvenlik duvarı kuralları](sql-database-firewall-configure.md).

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png
