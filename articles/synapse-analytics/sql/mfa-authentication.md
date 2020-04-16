---
title: Çok faktörlü AAD kimlik doğrulamasını kullanma
description: Synapse SQL, Active Directory Universal Authentication kullanarak SQL Server Management Studio'dan (SSMS) bağlantıları destekler.
services: synapse-analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick
ms.openlocfilehash: 8e8f17f54f52aef6f552b7c211aa8f0e498b94df
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424926"
---
# <a name="using-multi-factor-aad-authentication-with-synapse-sql-ssms-support-for-mfa"></a>Synapse SQL ile çok faktörlü AAD kimlik doğrulamasını kullanma (MFA için SSMS desteği)

Synapse SQL, *Active Directory Universal Authentication*kullanarak SQL Server Management Studio'dan (SSMS) bağlantıları destekler. 

Bu makalede, çeşitli kimlik doğrulama seçenekleri arasındaki farklar ve ayrıca Evrensel Kimlik Doğrulama kullanarak ilişkili sınırlamalar açıklanın. 

**En son SSMS indirin** - Istemci bilgisayarda, Download [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)dan, SSMS en son sürümünü indirin. 

**En son SSMS indirin** - Istemci bilgisayarda, Download [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)dan, SSMS en son sürümünü indirin.

Bu makalede tartışılan tüm özellikler için, en az Temmuz 2017, sürüm 17.2 kullanın.  En son bağlantı iletişim kutusu, aşağıdaki görüntüye benzer olmalıdır:

![1mfa-evrensel-bağlantı](./media/mfa-authentication/1mfa-universal-connect.png "Kullanıcı adı kutusunu tamamlar.")  

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

Çok Faktörlü Kimlik Doğrulama'nın açıklaması için, [Bkz. Çok Faktörlü Kimlik Doğrulama.](../../active-directory/authentication//concept-mfa-howitworks.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

### <a name="azure-ad-domain-name-or-tenant-id-parameter"></a>Azure AD etki alanı adı veya kiracı kimliği parametresi

[SSMS sürüm 17](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ile başlayarak, konuk kullanıcı olarak diğer Azure Etkin Dizinlerinden geçerli Active Directory'ye aktarılan kullanıcılar, bağlandıklarında Azure AD etki alanı adını veya kiracı kimliğini sağlayabilir. 

Konuk kullanıcılar, diğer Azure AD'lerinden davet edilen kullanıcıları, outlook.com, hotmail.com, live.com gibi Microsoft hesaplarını veya gmail.com gibi diğer hesapları içerir. Bu bilgiler, **MFA Kimlik Doğrulaması ile Active Directory Universal'ın** doğru kimlik doğrulama yetkisini tanımlamasına olanak tanır. Bu seçenek, outlook.com, hotmail.com, live.com veya MSA olmayan hesaplar gibi Microsoft hesaplarını (MSA) desteklemek için de gereklidir. 

Evrensel Kimlik Doğrulama kullanarak kimlik doğrulaması olmak isteyen tüm bu kullanıcıların Azure AD etki alanı adlarını veya kiracı kimliklerini girmeleri gerekir. Bu parametre, Azure Server'ın bağlı olduğu geçerli Azure AD etki alanı adını/kiracı kimliğini temsil eder. 

Örneğin, `contosotest.onmicrosoft.com` Azure Server, kullanıcının `joe@contosodev.onmicrosoft.com` Azure AD etki alanından `contosodev.onmicrosoft.com`alınan bir kullanıcı olarak barındırıldığı Azure AD etki `contosotest.onmicrosoft.com`alanıyla ilişkiliyse, bu kullanıcının kimliğini doğrulamak için gereken etki alanı adı. 

Kullanıcı Azure Ad'ın Azure Server'a bağlı yerel kullanıcısı ysa ve MSA hesabı değilse, etki alanı adı veya kiracı kimliği gerekmez. 

Parametreyi (SSMS sürüm 17.2 ile başlayarak) **Veritabanına Bağlan** iletişim kutusuna girmek için, Active **Directory - Universal with MFA** kimlik doğrulaması seçeneğini seçerek iletişim kutusunu tamamlayın, **Seçenekler'i**tıklatın, **Kullanıcı adı** kutusunu tamamlayın ve ardından **Bağlantı Özellikleri** sekmesini tıklatın. 

AD **etki alanı adını veya kiracı kimliği** kutusunu denetleyin ve alan adı **(contosotest.onmicrosoft.com)** veya kiracı kimliğinin GUID'i gibi kimlik doğrulama yetkisi sağlayın.  

   ![mfa-kiracı-ssms](./media/mfa-authentication/mfa-tenant-ssms.png)

SSMS 18.x veya daha sonra çalıştırıyorsanız, 18.x veya daha sonra otomatik olarak tanıdığı ndan, konuk kullanıcılar için artık AD alan adı veya kiracı kimliğine gerek kalmaz.

   ![mfa-kiracı-ssms](./media/mfa-authentication/mfa-no-tenant-ssms.png)

### <a name="azure-ad-business-to-business-support"></a>Azure AD işletmesiile iş desteği   
Azure AD B2B senaryoları için konuk kullanıcı olarak desteklenen Azure AD kullanıcıları (bkz. [What is Azure B2B collaboration](../../active-directory/b2b/what-is-b2b.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) `CREATE USER` 

Örneğin, Azure `steve@gmail.com` AD'ye `contosotest` (Azure Reklam etki `contosotest.onmicrosoft.com`alanıyla) davet edilirse, `usergroup` `steve@gmail.com` üyeyi içeren Azure AD'de oluşturulması gereken bir Azure REKLAM grubu olmalıdır. Daha sonra, bu grup, Bir Transact-SQL `CREATE USER [usergroup] FROM EXTERNAL PROVIDER` deyimi yürütülerek Azure AD SQL yöneticisi veya Azure AD DBO tarafından belirli bir veritabanı (yani MyDatabase) için oluşturulmalıdır. 

Veritabanı kullanıcısı oluşturulduktan sonra, `steve@gmail.com` kullanıcı SSMS `MyDatabase` kimlik doğrulama seçeneğini `Active Directory – Universal with MFA support`kullanarak oturum açabilir. 

Kullanıcı grubu, varsayılan olarak, yalnızca bağlantı iznine ve normal şekilde verilmesi gereken başka veri erişimine sahiptir. 

Konuk kullanıcı `steve@gmail.com` olarak kullanıcının kutuyu denetlemesi ve `contosotest.onmicrosoft.com` SSMS **Connection Property** iletişim kutusuna AD etki alanı adını eklemesi gerektiğini unutmayın. **AD etki alanı adı veya kiracı kimliği** seçeneği yalnızca MFA bağlantı seçenekleriyle Evrensel için desteklenir, aksi takdirde solukludur.

## <a name="universal-authentication-limitations-for-synapse-sql"></a>Synapse SQL için Evrensel Kimlik Doğrulama sınırlamaları

- SSMS ve SqlPackage.exe şu anda Active Directory Universal Authentication aracılığıyla MFA için etkinleştirilen tek araçlardır.
- SSMS sürüm 17.2, MFA ile Evrensel Kimlik Doğrulama kullanarak çok kullanıcılı eşzamanlı erişimi destekler. Sürüm 17.0 ve 17.1, Evrensel Kimlik Doğrulaması kullanarak SSMS örneği için girişini tek bir Azure Etkin Dizin hesabıyla sınırlandırdı. Başka bir Azure REKLAM hesabı olarak oturum açmak için başka bir SSMS örneği kullanmanız gerekir. (Bu kısıtlama Active Directory Universal Authentication ile sınırlıdır; Active Directory Password Authentication, Active Directory Integrated Authentication veya SQL Server Authentication kullanarak farklı sunucularda oturum açabilirsiniz).
- SSMS, Object Explorer, Query Editor ve Query Store görselleştirmesi için Active Directory Universal Authentication'ı destekler.
- SSMS sürüm 17.2, Veri İhraç/Ayıkla/Dağıt veri tabanı için DacFx Sihirbazı desteği sağlar. Evrensel Kimlik Doğrulama'yı kullanarak belirli bir kullanıcının ilk kimlik doğrulama iletişim kutusunda kimlik doğrulaması alındıktan sonra, DacFx Sihirbazı diğer tüm kimlik doğrulama yöntemleriyle aynı şekilde çalışır.
- SSMS Tablo Tasarımcısı Evrensel Kimlik Doğrulaması desteklemez.
- Active Directory Universal Authentication için, SSMS'in desteklenen bir sürümünü kullanmanız dışında ek yazılım gereksinimleri yoktur.  
- Evrensel kimlik doğrulaması için Active Directory Authentication Library (ADAL) sürümü en son ADAL.dll 3.13.9 kullanılabilir sürümüne güncelleştirildi. [Bkz. Etkin Dizin Kimlik Doğrulama Kitaplığı 3.14.1.](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)  

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için SQL Server Management Studio makalesi [ile Synapse SQL'e Bağlan'a](get-started-ssms.md) bakın. 

