---
title: Çok faktörlü AAD kimlik doğrulamasını kullanma
description: Azure SQL Veritabanı ve Azure Synapse, Active Directory Universal Authentication'ı kullanarak SQL Server Management Studio'dan (SSMS) bağlantıları destekler.
services: sql-database
ms.service: sql-database
ms.subservice: security
titleSuffix: Azure SQL Database and Azure Synapse
ms.custom: seoapril2019
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 02/06/2020
tags: azure-synapse
ms.openlocfilehash: e9a4aa5b54cf7ed48daf1899bb5801c609dfbf32
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255879"
---
# <a name="using-multi-factor-aad-authentication-with-azure-sql-database-and-azure-synapse-analytics-ssms-support-for-mfa"></a>Azure SQL Veritabanı ve Azure Synapse Analytics ile çok faktörlü AAD kimlik doğrulamasını kullanma (MFA için SSMS desteği)
Azure SQL Veritabanı ve Azure Synapse, *Active Directory Universal Authentication'ı*kullanarak SQL Server Management Studio'dan (SSMS) bağlantıları destekler. Bu makalede, çeşitli kimlik doğrulama seçenekleri arasındaki farklar ve ayrıca Evrensel Kimlik Doğrulama kullanarak ilişkili sınırlamalar açıklanın. 

**En son SSMS indirin** - Istemci bilgisayarda, Download [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)dan, SSMS en son sürümünü indirin. 


Bu makalede tartışılan tüm özellikler için, en az Temmuz 2017, sürüm 17.2 kullanın.  En son bağlantı iletişim kutusu, aşağıdaki görüntüye benzer olmalıdır:
 
  ![1mfa-evrensel-bağlantı](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png "Kullanıcı adı kutusunu tamamlar.")  

## <a name="the-five-authentication-options"></a>Beş kimlik doğrulama seçeneği  

Active Directory Universal Authentication, etkileşimli olmayan iki kimlik doğrulama yöntemini destekler:
    - `Active Directory - Password`Kimlik doğrulama
    - `Active Directory - Integrated`Kimlik doğrulama

Birçok farklı uygulamada kullanılabilen etkileşimli olmayan iki kimlik doğrulama modeli de vardır (ADO.NET, JDCB, ODC, vb.). Bu iki yöntem hiçbir zaman açılır iletişim kutularıyla sonuçlanmaz: 
- `Active Directory - Password` 
- `Active Directory - Integrated` 

Etkileşimli yöntem, Azure çok faktörlü kimlik doğrulamasını (MFA) da destekleyen yöntemdir: 
- `Active Directory - Universal with MFA` 


Azure MFA, kullanıcıların oturum açmaya yönelik basit işlem taleplerini karşılarken, verilere ve uygulamalara erişimi korumaya da yardımcı olur. Bir dizi kolay doğrulama seçeneğiyle (telefon görüşmesi, kısa mesaj, pinli akıllı kartlar veya mobil uygulama bildirimi) güçlü bir kimlik doğrulaması sağlayarak kullanıcıların tercih ettikleri yöntemi seçmelerine olanak tanır. Azure AD ile etkileşimli MFA, doğrulama için açılır iletişim kutusuyla sonuçlanabilir.

Çok Faktörlü Kimlik Doğrulama'nın açıklaması için, [Bkz. Çok Faktörlü Kimlik Doğrulama.](../active-directory/authentication/multi-factor-authentication.md)
Yapılandırma adımları [için, SQL Server Management Studio için Azure SQL Veritabanı'nı çok faktörlü kimlik doğrulamayı yapılandırma'ya](sql-database-ssms-mfa-authentication-configure.md)bakın.

### <a name="azure-ad-domain-name-or-tenant-id-parameter"></a>Azure AD etki alanı adı veya kiracı kimliği parametresi   

[SSMS sürüm 17](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)ile başlayarak, konuk kullanıcı olarak diğer Azure Etkin Dizinlerinden geçerli Active Directory'ye aktarılan kullanıcılar, bağlandıklarında Azure AD etki alanı adını veya kiracı kimliğini sağlayabilir. Konuk kullanıcılar, diğer Azure AD'lerinden davet edilen kullanıcıları, outlook.com, hotmail.com, live.com gibi Microsoft hesaplarını veya gmail.com gibi diğer hesapları içerir. Bu bilgiler, **MFA Kimlik Doğrulaması ile Active Directory Universal'ın** doğru kimlik doğrulama yetkisini tanımlamasına olanak tanır. Bu seçenek, outlook.com, hotmail.com, live.com veya MSA olmayan hesaplar gibi Microsoft hesaplarını (MSA) desteklemek için de gereklidir. Evrensel Kimlik Doğrulama kullanarak kimlik doğrulaması olmak isteyen tüm bu kullanıcıların Azure AD etki alanı adlarını veya kiracı kimliklerini girmeleri gerekir. Bu parametre, Azure Server'ın bağlı olduğu geçerli Azure AD etki alanı adını/kiracı kimliğini temsil eder. Örneğin, `contosotest.onmicrosoft.com` Azure Server, kullanıcının `joe@contosodev.onmicrosoft.com` Azure AD etki alanından `contosodev.onmicrosoft.com`alınan bir kullanıcı olarak barındırıldığı Azure AD etki `contosotest.onmicrosoft.com`alanıyla ilişkiliyse, bu kullanıcının kimliğini doğrulamak için gereken etki alanı adı. Kullanıcı Azure Ad'ın Azure Server'a bağlı yerel kullanıcısı ysa ve MSA hesabı değilse, etki alanı adı veya kiracı kimliği gerekmez. Veritabanına **Bağlan** iletişim kutusuna parametre (SSMS sürümü 17.2 ile başlayarak) girmek için, Iletişim kutusunu tamamlayın, **Active Directory - MFA** kimlik doğrulaması ile Evrensel'i seçin, **Seçenekler'i**tıklatın, **Kullanıcı adı** kutusunu tıklatın ve ardından Bağlantı **Özellikleri** sekmesini tıklatın. AD etki alanı adını veya **kiracı kimliği** kutusunu işaretleyin ve alan adı (**contosotest.onmicrosoft.com**) veya kiracı kimliğinin GUID'i gibi kimlik doğrulama yetkilisini sağlayın.  
   ![mfa-kiracı-ssms](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)

SSMS 18.x veya daha sonra çalıştırıyorsanız, 18.x veya daha sonra otomatik olarak tanıdığı ndan, konuk kullanıcılar için artık AD alan adı veya kiracı kimliğine gerek kalmaz.

   ![mfa-kiracı-ssms](./media/sql-database-ssms-mfa-auth/mfa-no-tenant-ssms.png)

### <a name="azure-ad-business-to-business-support"></a>Azure AD işletmesiile iş desteği   
Azure AD B2B senaryoları için desteklenen Azure AD kullanıcıları (bkz. [Azure B2B işbirliği nedir)](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md)SQL Veritabanı'na ve Azure Sinaps'a yalnızca geçerli Azure AD'sinde `CREATE USER` oluşturulan ve belirli bir veritabanındaki Transact-SQL bildirimini kullanarak el ile eşlenen bir grubun üyesinin bir parçası olarak bağlanabilir. Örneğin, Azure `steve@gmail.com` AD'ye `contosotest` (Azure Reklam etki `contosotest.onmicrosoft.com`alanıyla) davet edilirse, `usergroup` `steve@gmail.com` üyeyi içeren Azure AD'de oluşturulması gereken bir Azure REKLAM grubu olmalıdır. Daha sonra, bu grup, Bir Transact-SQL `CREATE USER [usergroup] FROM EXTERNAL PROVIDER` deyimi yürütülerek Azure AD SQL yöneticisi veya Azure AD DBO tarafından belirli bir veritabanı (yani MyDatabase) için oluşturulmalıdır. Veritabanı kullanıcısı oluşturulduktan sonra, `steve@gmail.com` kullanıcı SSMS `MyDatabase` kimlik doğrulama seçeneğini `Active Directory – Universal with MFA support`kullanarak oturum açabilir. Kullanıcı grubu, varsayılan olarak, yalnızca bağlantı iznine ve normal şekilde verilmesi gereken başka veri erişimine sahiptir. Konuk kullanıcı `steve@gmail.com` olarak kullanıcının kutuyu denetlemesi ve `contosotest.onmicrosoft.com` SSMS **Connection Property** iletişim kutusuna AD etki alanı adını eklemesi gerektiğini unutmayın. **AD etki alanı adı veya kiracı kimliği** seçeneği yalnızca MFA bağlantı seçenekleriyle Evrensel için desteklenir, aksi takdirde solukludur.

## <a name="universal-authentication-limitations-for-sql-database-and-azure-synapse"></a>SQL Veritabanı ve Azure Sinapsiçin Evrensel Kimlik Doğrulama sınırlamaları
- SSMS ve SqlPackage.exe şu anda Active Directory Universal Authentication aracılığıyla MFA için etkinleştirilen tek araçlardır.
- SSMS sürüm 17.2, MFA ile Evrensel Kimlik Doğrulama kullanarak çok kullanıcılı eşzamanlı erişimi destekler. Sürüm 17.0 ve 17.1, Evrensel Kimlik Doğrulaması kullanarak SSMS örneği için girişini tek bir Azure Etkin Dizin hesabıyla sınırlandırdı. Başka bir Azure REKLAM hesabı olarak oturum açmak için başka bir SSMS örneği kullanmanız gerekir. (Bu kısıtlama Active Directory Universal Authentication ile sınırlıdır; Active Directory Password Authentication, Active Directory Integrated Authentication veya SQL Server Authentication kullanarak farklı sunucularda oturum açabilirsiniz).
- SSMS, Object Explorer, Query Editor ve Query Store görselleştirmesi için Active Directory Universal Authentication'ı destekler.
- SSMS sürüm 17.2, Veri İhraç/Ayıkla/Dağıt veri tabanı için DacFx Sihirbazı desteği sağlar. Evrensel Kimlik Doğrulama'yı kullanarak belirli bir kullanıcının ilk kimlik doğrulama iletişim kutusunda kimlik doğrulaması alındıktan sonra, DacFx Sihirbazı diğer tüm kimlik doğrulama yöntemleriyle aynı şekilde çalışır.
- SSMS Tablo Tasarımcısı Evrensel Kimlik Doğrulaması desteklemez.
- Active Directory Universal Authentication için, SSMS'in desteklenen bir sürümünü kullanmanız dışında ek yazılım gereksinimleri yoktur.  
- Evrensel kimlik doğrulaması için Active Directory Authentication Library (ADAL) sürümü en son ADAL.dll 3.13.9 kullanılabilir sürümüne güncelleştirildi. [Bkz. Etkin Dizin Kimlik Doğrulama Kitaplığı 3.14.1.](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)  


## <a name="next-steps"></a>Sonraki adımlar

- Yapılandırma adımları [için, SQL Server Management Studio için Azure SQL Veritabanı'nı çok faktörlü kimlik doğrulamayı yapılandırma'ya](sql-database-ssms-mfa-authentication-configure.md)bakın.
- Diğerlerine veritabanınıza erişim izni: [SQL Veritabanı Kimlik Doğrulama ve Yetkilendirme: Erişim verme](sql-database-manage-logins.md)  
- Başkalarının güvenlik duvarı üzerinden bağlanadığından emin olun: [Azure portalını kullanarak Bir Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralını yapılandırın](sql-database-configure-firewall-settings.md)  
- [Azure Active Directory kimlik doğrulamasını SQL Veritabanı veya Azure Sinaps'ı ile yapılandırma ve yönetme](sql-database-aad-authentication-configure.md)  
- [Microsoft SQL Server Veri Katmanı Uygulama Çerçevesi (17.0.0 GA)](https://www.microsoft.com/download/details.aspx?id=55088)  
- [SQLPackage.exe](https://docs.microsoft.com/sql/tools/sqlpackage)  
- [BACPAC dosyasını yeni bir Azure SQL Veritabanı’na içeri aktarma](../sql-database/sql-database-import.md)  
- [Azure SQL Veritabanı’nı bir BACPAC dosyasına dışarı aktarma](../sql-database/sql-database-export.md)  
- C# arabirimi [IUniversalAuthSağlayıcı Arabirimi](https://msdn.microsoft.com/library/microsoft.sqlserver.dac.iuniversalauthprovider.aspx)  
- Active **Directory-Universal ile MFA** kimlik doğrulaması kullanırken, [SSMS 17.3](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)ile başlayan ADAL izleme kullanılabilir. Varsayılan olarak, **Azure Hizmetleri**, **Azure Bulut**, ADAL Çıkış Penceresi İzleme Düzeyi altında **Araçlar**, **Seçenekler** menüsünü ve ardından **Görünüm** menüsünde **Çıktı'yı** etkinleştirerek **ADAL**izlemeyi açabilirsiniz. Azure **Active Directory seçeneğini**seçerken izlemeler çıktı penceresinde kullanılabilir.  
