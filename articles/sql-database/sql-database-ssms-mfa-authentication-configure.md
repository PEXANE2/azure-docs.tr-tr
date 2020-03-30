---
title: Multi-Factor Authentication’ı yapılandırma
description: SQL Veritabanı ve SQL Veri Ambarı için SSMS ile Çok Faktörlü Kimlik Doğrulama'yı nasıl kullanacağınızı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 08/27/2019
ms.openlocfilehash: 5d4d410f6fca566dab14e601972952b5996c331a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124881"
---
# <a name="configure-multi-factor-authentication-for-sql-server-management-studio-and-azure-ad"></a>SQL Server Management Studio ve Azure AD için çok faktörlü kimlik doğrulamayı yapılandırma

Bu konu, SQL Server Management Studio ile Azure Active Directory çok faktörlü kimlik doğrulamasını (MFA) nasıl kullanacağınızı gösterir. Azure AD MFA, SSMS veya SqlPackage.exe'yi Azure [SQL Veritabanı](sql-database-technical-overview.md) ve SQL [Veri Ambarı'na](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md)bağlarken kullanılabilir. Azure SQL Veritabanı çok faktörlü kimlik doğrulamagenel bakış için, [SQL Veritabanı ve SQL Veri Ambarı (MFA için SSMS desteği) ile Evrensel Kimlik Doğrulama bölümüne](sql-database-ssms-mfa-authentication.md)bakın.

> [!NOTE]
> Bu konu başlığı, Azure SQL sunucusunun yanı sıra Azure SQL sunucusu üzerinde oluşturulmuş olan SQL Veritabanı ve SQL Veri Ambarı veritabanları için de geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.

## <a name="configuration-steps"></a>Yapılandırma adımları

1. **Azure Etkin Dizinini Yapılandırın** - Daha fazla bilgi için Azure [REKLAM dizininizi yönetme](https://msdn.microsoft.com/library/azure/hh967611.aspx), [şirket içi kimliklerinizi Azure Etkin Dizini ile tümleştirme](../active-directory/hybrid/whatis-hybrid-identity.md), [Azure AD'ye kendi etki alanı adınızı ekleme](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/), Microsoft Azure artık Windows Server Active [Directory ile federasyonu destekliyor](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/)ve Windows [PowerShell'i kullanarak Azure AD'yi yönetin.](https://msdn.microsoft.com/library/azure/jj151815.aspx)
2. **MFA'yı yapılandırın** - Adım adım talimatlar için Azure [Çok Faktörlü Kimlik Doğrulama nedir?](../active-directory/authentication/multi-factor-authentication.md) [Conditional Access (MFA) with Azure SQL Database and Data Warehouse](sql-database-conditional-access.md) (Tam Koşullu Erişim için Premium Azure Etkin Dizini (Azure AD) gerektirir. Sınırlı MFA standart bir Azure AD ile kullanılabilir.)
3. **Azure AD Kimlik Doğrulaması için SQL Veritabanını veya SQL Veri Ambarı'nı yapılandırın** - Adım adım talimatlar için Azure [Active Directory Authentication'ı kullanarak SQL Veritabanına bağlanma veya SQL Veri Ambarı'na](sql-database-aad-authentication.md)bakın.
4. **Download SSMS** - Istemci bilgisayarda, [download SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)en son SSMS indirin. Bu konudaki tüm özellikler için en az Temmuz 2017 sürüm 17.2'yi kullanın.  

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>SSMS ile evrensel kimlik doğrulaması kullanarak bağlanma

Aşağıdaki adımlar, en son SSMS'yi kullanarak SQL Veritabanı'na veya SQL Veri Ambarı'na nasıl bağlanışyapılacağını gösterir.

1. Evrensel Kimlik Doğrulama'yı kullanarak Bağlanmak **için Sunucuya Bağlan** iletişim kutusunda **Active Directory - Universal with MFA desteğini**seçin. (Active **Directory Universal Authentication'ı** görürseniz SSMS'in en son sürümünde yoksunuz.)  
   ![1mfa-evrensel-bağlantı][1]  
2. Azure Active Directory kimlik bilgileriyle **Kullanıcı adı** `user_name@domain.com`kutusunu biçimde tamamlayın.  
   ![1mfa-universal-connect-kullanıcı](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect-user.png)   
3. Konuk kullanıcı olarak bağlanıyorsanız, SSMS 18.x veya daha sonra otomatik olarak tanıdığı için konuk kullanıcılar için artık AD alan adı veya kiracı kimliği alanını tamamlamanız gerekmez. Daha fazla bilgi için, [SQL Veritabanı ve SQL Veri Ambarı (MFA için SSMS desteği) ile Evrensel Kimlik Doğrulama bölümüne](sql-database-ssms-mfa-authentication.md)bakın.
   ![mfa-no-kiracı-ssms](./media/sql-database-ssms-mfa-auth/mfa-no-tenant-ssms.png)

   Ancak, SSMS 17.x veya daha büyük kullanarak konuk kullanıcı olarak bağlanıyorsanız, **Seçenekler'i**ve **Bağlantı Özelliği** iletişim kutusunda ve **AD alan adı veya kiracı kimliği** kutusunu tamamlamanız gerekir.
   ![mfa-kiracı-ssms](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)

4. SQL Veritabanı ve SQL Veri Ambarı için her zamanki gibi Seçenekler'i **tıklatmalı** ve **Seçenekler** iletişim kutusunda veritabanını belirtmelisiniz. (Bağlı kullanıcı konuk kullanıcıysa (örn. joe@outlook.com) kutuyu işaretleyip Seçenekler'in bir parçası olarak geçerli AD etki alanı adını veya kiracı kimliğini eklemeniz gerekir. Bkz. [SQL Veritabanı ve SQL Veri Ambarı ile Evrensel Kimlik Doğrulama (MFA için SSMS desteği)](sql-database-ssms-mfa-authentication.md). Ardından **Bağlan**’a tıklayın.  
5. Hesap iletişim **kutunuzda Oturum Aç** göründüğünde, Azure Etkin Dizin kimliğinizin hesabını ve parolasını sağlayın. Bir kullanıcı Azure AD ile beslenen bir etki alanının parçasıysa parola gerekmez.  
   ![2mfa-oturum açma][2]  

   > [!NOTE]
   > MFA gerektirmeyen bir hesapla Evrensel Kimlik Doğrulama için bu noktada bağlanırsınız. MFA gerektiren kullanıcılar için aşağıdaki adımları ile devam edin:
   >  
   
6. İki MFA kurulum iletişim kutusu görünebilir. Bu tek seferlik işlem MFA yönetici ayarına bağlıdır ve bu nedenle isteğe bağlı olabilir. MFA etkin leştirilmiş bir etki alanı için bu adım bazen önceden tanımlanır (örneğin, etki alanı kullanıcıların akıllı kart ve pin kullanmasını gerektirir).  
   ![3mfa-kurulum][3]  
7. İkinci olası bir kez iletişim kutusu, kimlik doğrulama yönteminizin ayrıntılarını seçmenize olanak tanır. Olası seçenekler yöneticiniz tarafından yapılandırılır.  
   ![4mfa-doğrula-1][4]  
8. Azure Etkin Dizin, doğrulayan bilgileri size gönderir. Doğrulama kodunu aldığınızda, **doğrulama kodu** girin kutusuna girin ve **Oturum Aç'ı**tıklatın.  
   ![5mfa-doğrula-2][5]  

Doğrulama tamamlandığında, SSMS normalde geçerli kimlik bilgilerini ve güvenlik duvarı erişimini varsayarak bağlanır.

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL Veritabanı çok faktörlü kimlik doğrulamagenel bakış için, [SQL Veritabanı ve SQL Veri Ambarı (MFA için SSMS desteği)](sql-database-ssms-mfa-authentication.md)ile Evrensel Kimlik Doğrulama bölümüne bakın.  
- Diğerlerine veritabanınıza erişim izni: [SQL Veritabanı Kimlik Doğrulama ve Yetkilendirme: Erişim verme](sql-database-manage-logins.md)  
- Başkalarının güvenlik duvarı üzerinden bağlanadığından emin olun: [Azure portalını kullanarak Bir Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralını yapılandırın](sql-database-configure-firewall-settings.md)  
- Active **Directory-Universal ile MFA** kimlik doğrulaması kullanırken, [SSMS 17.3](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)ile başlayan ADAL izleme kullanılabilir. Varsayılan olarak, **Azure Hizmetleri**, **Azure Bulut**, ADAL Çıkış Penceresi İzleme Düzeyi altında **Araçlar**, **Seçenekler** menüsünü ve ardından **Görünüm** menüsünde **Çıktı'yı** etkinleştirerek **ADAL**izlemeyi açabilirsiniz. Azure **Active Directory seçeneğini**seçerken izlemeler çıktı penceresinde kullanılabilir.   


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

