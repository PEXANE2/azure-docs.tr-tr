---
title: Multi-Factor AAD kimlik doğrulamasını kullanma
description: Active Directory evrensel kimlik doğrulaması kullanarak SQL Server Management Studio (SSMS) üzerinden SQL destek bağlantılarını SYNAPSE.
services: synapse-analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick
ms.openlocfilehash: 8e8f17f54f52aef6f552b7c211aa8f0e498b94df
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81424926"
---
# <a name="using-multi-factor-aad-authentication-with-synapse-sql-ssms-support-for-mfa"></a>Multi-Factor AAD kimlik doğrulamasını SYNAPSE SQL ile kullanma (MFA için SSMS desteği)

*Active Directory evrensel kimlik doğrulaması*kullanarak SQL Server Management Studio (SSMS) üzerinden SQL destek bağlantılarını SYNAPSE. 

Bu makalede çeşitli kimlik doğrulama seçenekleri arasındaki farklar ve ayrıca evrensel kimlik doğrulaması kullanımıyla ilişkili sınırlamalar ele alınmaktadır. 

**En son SSMS 'Yi indirin** -istemci bilgisayarda, [Download SQL Server Management Studio 'ten (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)en son SSMS sürümünü indirin. 

**En son SSMS 'Yi indirin** -istemci bilgisayarda, [Download SQL Server Management Studio 'ten (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)en son SSMS sürümünü indirin.

Bu makalede ele alınan tüm özellikler için en az 2017 Temmuz sürüm 17,2 kullanın.  En son bağlantı iletişim kutusu aşağıdaki görüntüye benzer görünmelidir:

![1mfa-Universal-Connect](./media/mfa-authentication/1mfa-universal-connect.png "Kullanıcı adı kutusunu tamamlar.")  

## <a name="the-five-authentication-options"></a>Beş kimlik doğrulama seçeneği  

Active Directory evrensel kimlik doğrulaması, etkileşimli olmayan iki kimlik doğrulama yöntemini destekler:
    - `Active Directory - Password`yetkilendirmesi
    - `Active Directory - Integrated`yetkilendirmesi

Birçok farklı uygulamada (ADO.NET, JDCB, ODC vb.) kullanılabilecek iki etkileşimli olmayan kimlik doğrulama modeli vardır. Bu iki yöntem hiçbir koşulda açılan iletişim kutularına neden olmaz:

- `Active Directory - Password`
- `Active Directory - Integrated`

Etkileşimli Yöntem de Azure Multi-Factor Authentication (MFA) hizmetini de destekler:

- `Active Directory - Universal with MFA`

Azure MFA, kullanıcıların oturum açmaya yönelik basit işlem taleplerini karşılarken, verilere ve uygulamalara erişimi korumaya da yardımcı olur. Kolay doğrulama seçenekleriyle (telefon araması, SMS mesajı, PIN ile akıllı kartlar veya mobil uygulama bildirimi), kullanıcıların tercih ettikleri yöntemi seçebilmesine olanak tanıyan güçlü kimlik doğrulaması sağlar. Azure AD ile etkileşimli MFA, doğrulama için bir açılır iletişim kutusu ile sonuçlanabilir.

Multi-Factor Authentication açıklaması için bkz. [Multi-Factor Authentication](../../active-directory/authentication//concept-mfa-howitworks.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="azure-ad-domain-name-or-tenant-id-parameter"></a>Azure AD etki alanı adı veya kiracı KIMLIĞI parametresi

[SSMS sürümü 17](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)' den başlayarak, Konuk Kullanıcı olarak diğer Azure Active Directory 'lerden Geçerli Active Directory içeri aktarılan kullanıcılar, BAĞLANDıKLARıNDA Azure AD etki alanı adını veya kiracı kimliğini sağlayabilir. 

Konuk kullanıcılar, diğer Azure reklamları, outlook.com, hotmail.com, live.com gibi Microsoft hesaplarından veya gmail.com gibi diğer hesaplarla davet edilen kullanıcıları içerir. Bu bilgiler, **MFA kimlik doğrulamasıyla evrensel Active Directory** , doğru kimlik doğrulama yetkilisini belirlemesine izin verir. Bu seçenek ayrıca, outlook.com, hotmail.com, live.com veya MSA olmayan hesaplar gibi Microsoft hesaplarını (MSA) desteklemek için de gereklidir. 

Evrensel kimlik doğrulaması kullanarak kimlik doğrulaması yapmak isteyen tüm bu kullanıcıların Azure AD etki alanı adını veya kiracı KIMLIĞINI girmesi gerekir. Bu parametre, Azure Server ile bağlantılı olan geçerli Azure AD etki alanı adını/kiracı KIMLIĞINI temsil eder. 

Örneğin, Azure `contosotest.onmicrosoft.com` Server, kullanıcının `joe@contosodev.onmicrosoft.com` Azure AD etki `contosodev.onmicrosoft.com`alanından Içeri AKTARıLMıŞ bir kullanıcı olarak barındırıldığı Azure AD etki alanı ile ilişkiliyse, bu kullanıcının kimliğini doğrulamak için gereken etki alanı adı. `contosotest.onmicrosoft.com` 

Kullanıcı Azure AD 'nin Azure Server 'a bağlı olduğu yerel bir kullanıcı olduğunda ve bir MSA hesabı olmadığında, etki alanı adı veya kiracı KIMLIĞI gerekmez. 

Parametreyi (SSMS sürüm 17,2 ' den başlayarak) girmek için, **veritabanına Bağlan** iletişim kutusunda, iletişim kutusunu doldurun, MFA kimlik doğrulamasıyla **Active Directory-Universal** ' i seçin, **Seçenekler**' e tıklayın, **Kullanıcı adı** kutusunu doldurun ve ardından **bağlantı özellikleri** sekmesine tıklayın. 

**Ad etki alanı adı veya KIRACı kimliği** kutusunu işaretleyin ve kimlik doğrulaması yetkilisini (etki alanı adı (**contosotest.onmicrosoft.com**) veya kiracı kimliğinin GUID 'si gibi) belirtin.  

   ![MFA-kiracı-SSMS](./media/mfa-authentication/mfa-tenant-ssms.png)

SSMS 18. x veya sonraki bir sürümünü çalıştırıyorsanız, bu AD etki alanı adı veya kiracı KIMLIĞI artık Konuk kullanıcılar için gerekli değildir çünkü 18. x veya daha sonraki bir sürümü otomatik olarak tanır.

   ![MFA-kiracı-SSMS](./media/mfa-authentication/mfa-no-tenant-ssms.png)

### <a name="azure-ad-business-to-business-support"></a>Azure AD iş 'e iş desteği   
Azure AD B2B senaryolarında Konuk Kullanıcı olarak desteklenen Azure AD kullanıcıları (bkz. [Azure B2B Işbirliği nedir,](../../active-directory/b2b/what-is-b2b.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) GEÇERLI Azure AD 'de oluşturulan bir grubun üyelerinin parçası olarak yalnızca SYNAPSE SQL 'e bağlanabilir ve belirli bir veritabanındaki Transact-SQL `CREATE USER` ifadesiyle el ile eşleştirilir. 

Örneğin, Azure AD `steve@gmail.com` 'ye `contosotest` (Azure AD etki alanı `contosotest.onmicrosoft.com`ile) davet edildiyseniz, `usergroup` `steve@gmail.com` üyeyi içeren Azure AD 'de, gibi bir Azure AD grubunun oluşturulması gerekir. Ardından, bir Transact-SQL `CREATE USER [usergroup] FROM EXTERNAL PROVIDER` ekstresi yürüterek, bu grup Azure ad SQL Yöneticisi veya Azure AD dbo tarafından belirli bir veritabanı (yani, MyDatabase) için oluşturulmalıdır. 

Veritabanı kullanıcısı oluşturulduktan sonra, Kullanıcı `steve@gmail.com` SSMS kimlik doğrulama seçeneğini `MyDatabase` `Active Directory – Universal with MFA support`kullanarak oturum açabilir. 

UserGroup, varsayılan olarak yalnızca Connect iznine ve normal şekilde verilmesi gereken daha fazla veri erişimine sahiptir. 

Konuk Kullanıcı olarak `steve@gmail.com` kullanıcının, SSMS **bağlantı özelliği** iletişim kutusunda kutuyu denetlemesi ve ad etki alanı `contosotest.onmicrosoft.com` adını eklemesi gerektiğini unutmayın. **Ad etki alanı adı veya KIRACı kimliği** SEÇENEĞI yalnızca MFA bağlantı seçenekleriyle Universal için desteklenir, aksi halde gri olur.

## <a name="universal-authentication-limitations-for-synapse-sql"></a>SYNAPSE SQL için evrensel kimlik doğrulama sınırlamaları

- SSMS ve SqlPackage. exe, şu anda Active Directory evrensel kimlik doğrulaması aracılığıyla MFA için etkinleştirilen tek araçlardır.
- SSMS sürüm 17,2, MFA ile evrensel kimlik doğrulaması kullanarak çok kullanıcılı eşzamanlı erişimi destekler. Sürüm 17,0 ve 17,1, evrensel kimlik doğrulaması kullanan SSMS örnekleri için tek bir Azure Active Directory hesabıyla bir oturum açmayı kısıtlamalıdır. Başka bir Azure AD hesabı olarak oturum açmak için SSMS 'nin başka bir örneğini kullanmanız gerekir. (Bu kısıtlama Active Directory evrensel kimlik doğrulaması ile sınırlıdır; Active Directory parola kimlik doğrulaması, Active Directory tümleşik kimlik doğrulaması veya SQL Server kimlik doğrulaması kullanarak farklı sunucularda oturum açabilirsiniz).
- SSMS, Nesne Gezgini, sorgu Düzenleyicisi ve sorgu deposu görselleştirmesi için Active Directory evrensel kimlik doğrulamasını destekler.
- SSMS sürüm 17,2, veri veritabanını dışa/ayıklamayı/dağıtmayı sağlayan DacFx Sihirbazı desteği sağlar. Belirli bir kullanıcının kimliği, evrensel kimlik doğrulaması kullanılarak ilk kimlik doğrulama iletişim kutusu aracılığıyla doğrulandıktan sonra, DacFx Sihirbazı diğer tüm kimlik doğrulama yöntemleri için aynı şekilde çalışır.
- SSMS Tablo Tasarımcısı evrensel kimlik doğrulamasını desteklemez.
- Active Directory evrensel kimlik doğrulaması için ek yazılım gereksinimi yoktur, ancak desteklenen bir SSMS sürümü kullanmanız gerekir.  
- Evrensel kimlik doğrulaması için Active Directory Authentication Library (ADAL) sürümü en son ADAL. dll 3.13.9 kullanılabilir yayımlanmış sürümü olarak güncelleştirildi. Bkz. [Active Directory Authentication Library 3.14.1](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/).  

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz. [SYNAPSE SQL Ile bağlanma SQL Server Management Studio](get-started-ssms.md) makalesi. 

