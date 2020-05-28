---
title: Multi-Factor Authentication’ı yapılandırma
titleSuffix: Azure SQL Database & SQL Managed Instance & Azure Synapse Analytics
description: Azure SQL veritabanı, Azure SQL yönetilen örneği ve Azure SYNAPSE Analytics için SSMS ile çok yönlü bir kimlik doğrulamasını nasıl kullanacağınızı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: has-adal-ref, sqldbrb=3
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 08/27/2019
ms.openlocfilehash: ed263b71621e0eb82171cca76a143bd0bb133e66
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84048913"
---
# <a name="configure-multi-factor-authentication-for-sql-server-management-studio-and-azure-ad"></a>SQL Server Management Studio ve Azure AD için Multi-Factor Authentication 'ı yapılandırma
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

Bu makalede, Azure Active Directory Multi-Factor Authentication 'ın (MFA) SQL Server Management Studio birlikte nasıl kullanılacağı gösterilmektedir. SSMS veya SqlPackage. exe ' yi [Azure SQL veritabanı](sql-database-paas-overview.md), [Azure SQL yönetilen örneği](../managed-instance/sql-managed-instance-paas-overview.md) ve [Azure SYNAPSE ANALYTICS (eski adıyla SQL DW)](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md)ile bağlarken Azure AD MFA kullanılabilir. Multi-Factor Authentication 'a genel bakış için bkz. [SQL veritabanı Ile evrensel kimlik doğrulaması, SQL yönetilen örneği ve Azure Synapse (MFA IÇIN SSMS desteği)](../database/authentication-mfa-ssms-overview.md).

> [!IMPORTANT]
> Azure SQL veritabanı, Azure SQL yönetilen örneği ve Azure Synapse (eski adıyla Azure SQL veri ambarı) veritabanları, bu makalenin geri kalanında veritabanı olarak ve sunucu, Azure SQL veritabanı ve Azure SYNAPSE için veritabanları barındıran [sunucuya](logical-servers.md) başvurmaktadır.

## <a name="configuration-steps"></a>Yapılandırma adımları

1. **Azure Active Directory yapılandırma** -daha fazla bilgi için bkz. [Azure AD dizininizi yönetme](https://msdn.microsoft.com/library/azure/hh967611.aspx), [Şirket içi kimliklerinizi Azure ACTIVE DIRECTORY ile tümleştirme](../../active-directory/hybrid/whatis-hybrid-identity.md), [Azure AD 'ye kendi etki alanı adınızı ekleme](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure artık Windows Server ACTIVE DIRECTORY ile Federasyonu destekliyor](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/)ve [Windows PowerShell kullanarak Azure AD 'yi yönetme](https://msdn.microsoft.com/library/azure/jj151815.aspx).
2. **MFA 'Yı yapılandırma** -adım adım yönergeler için bkz. Azure [SQL veritabanı ve veri ambarı ile](conditional-access-configure.md) [Azure Multi-Factor Authentication nedir?](../../active-directory/authentication/multi-factor-authentication.md), koşullu erişim (MFA). (Tam koşullu erişim bir Premium Azure Active Directory (Azure AD) gerektirir. Standart bir Azure AD ile sınırlı MFA kullanılabilir.)
3. **Azure AD kimlik doğrulamasını yapılandırma** -adım adım yönergeler Için [Azure Active Directory KIMLIK doğrulaması kullanarak SQL veritabanı, SQL yönetilen örneği veya Azure SYNAPSE 'a bağlanma](authentication-aad-overview.md)konusuna bakın.
4. **SSMS 'Yi indirme** -istemci bilgisayarda, [indirme SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)adresinden en son SSMS 'yi indirin.

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>SSMS ile evrensel kimlik doğrulaması kullanarak bağlanma

Aşağıdaki adımlarda, en son SSMS kullanılarak nasıl bağlanacağı gösterilmektedir.

1. Evrensel kimlik doğrulaması kullanarak bağlanmak için, SQL Server Management Studio (SSMS) içindeki **sunucuya Bağlan** iletişim kutusunda, **MFA desteği olan Active Directory-Universal**' i seçin. ( **Active Directory evrensel kimlik doğrulaması** ' nı görürseniz, en son SSMS sürümünde değilsiniz.)  
   ! [1mfa-Universal-Connect] 1  
2. Azure Active Directory kimlik bilgileriyle **Kullanıcı adı** kutusunu, biçimde doldurun `user_name@domain.com` .  
   ![1mfa-evrensel-Bağlan-Kullanıcı](./media/authentication-mfa-ssms-configure/1mfa-universal-connect-user.png)
3. Konuk Kullanıcı olarak bağlanıyorsanız, SMS 18. x veya daha sonraki bir sürümü tarafından otomatik olarak tanıyacağından, Konuk kullanıcılar için AD etki alanı adını veya kiracı KIMLIĞI alanını doldurmanız artık gerekmez. Daha fazla bilgi için bkz. [SQL veritabanı, SQL yönetilen örneği ve Azure Synapse (MFA IÇIN SSMS desteği) Ile evrensel kimlik doğrulaması](../database/authentication-mfa-ssms-overview.md).
   ![MFA-kiracı-SSMS](./media/authentication-mfa-ssms-configure/mfa-no-tenant-ssms.png)

   Ancak, SSMS 17. x veya daha eski bir sürümle Konuk Kullanıcı olarak bağlanıyorsanız, **Seçenekler**' e tıklamanız ve **bağlantı özelliği** iletişim kutusunda, **ad etkı alanı adını veya Kiracı kimliği** kutusunu doldurmanız gerekir.
   ![MFA-kiracı-SSMS](./media/authentication-mfa-ssms-configure/mfa-tenant-ssms.png)

4. **Seçenekler** ' i seçin ve **Seçenekler** iletişim kutusunda veritabanını belirtin. (Bağlı Kullanıcı bir Konuk Kullanıcı (yani joe@outlook.com ), kutuyu denetlemeniz ve geçerlI ad etki alanı adını ya da KIRACı kimliğini seçeneklerin bir parçası olarak eklemeniz gerekir. Bkz. [SQL veritabanı ve SQL veri ambarı Ile evrensel kimlik doğrulaması (MFA IÇIN SSMS desteği)](../database/authentication-mfa-ssms-overview.md). Ardından **Bağlan**’a tıklayın.  
5. **Hesapta oturum açma** iletişim kutusu göründüğünde, Azure Active Directory kimliğinizin hesabını ve parolasını girin. Bir Kullanıcı Azure AD ile federe bir etki alanının parçasıysa parola gerekli değildir.  
   ! [2mfa-oturum açma] iki  

   > [!NOTE]
   > MFA gerektirmeyen bir hesapla evrensel kimlik doğrulaması için bu noktaya bağlanırsınız. MFA gerektiren kullanıcılar için aşağıdaki adımlarla devam edin:
   >  

6. İki MFA kurulum iletişim kutusu görünebilir. Bu bir kerelik işlem MFA yönetici ayarına bağlıdır ve bu nedenle isteğe bağlı olabilir. MFA etkin bir etki alanı için bu adım bazen önceden tanımlanmıştır (örneğin, etki alanı kullanıcıların bir akıllı kart ve PIN kullanmasını gerektirir).  
   ! [3mfa-kurulum] 03  
7. İkinci olası bir zaman iletişim kutusu, kimlik doğrulama yönteminizin ayrıntılarını seçmenize olanak sağlar. Olası seçenekler yöneticiniz tarafından yapılandırılır.  
   ! [4mfa-Verify-1] 4  
8. Azure Active Directory onaylama bilgilerini size gönderir. Doğrulama kodunu aldığınızda, **doğrulama kodunu girin** kutusuna girin ve **oturum aç**' a tıklayın.  
   ! [5mfa-doğrula-2] e  

Doğrulama tamamlandığında SSMS, genellikle geçerli kimlik bilgilerini ve güvenlik duvarı erişimini önceden izleyerek bağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Multi-Factor Authentication 'a genel bakış için bkz. [SQL veritabanı Ile evrensel kimlik doğrulaması, SQL yönetilen örneği ve Azure Synapse (MFA IÇIN SSMS desteği)](../database/authentication-mfa-ssms-overview.md).  
- Veritabanınıza başkalarının erişmesine izin verme: [SQL veritabanı kimlik doğrulaması ve yetkilendirme: erişim verme](logins-create-manage.md)  
- Başkalarının güvenlik duvarından bağlanmasına emin olun: [Azure Portal kullanarak sunucu düzeyinde güvenlik duvarı kuralı yapılandırma](/database/firewall-configure.md)  
- MFA kimlik doğrulamasıyla **Active Directory Universal** KULLANıRKEN, adal Izleme [SSMS 17,3](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)ile başlayarak kullanılabilir. Varsayılan olarak, **Azure hizmetleri**, **Azure Cloud**, **adal çıkış penceresi izleme düzeyi**altındaki **Araçlar**, **Seçenekler** menüsünü ve ardından **Görünüm** menüsünde **çıktıyı** etkinleştirerek, adal izlemeyi açabilirsiniz. İzlemeler **Azure Active Directory seçenek**belirlendiğinde çıkış penceresinde kullanılabilir.
