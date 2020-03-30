---
title: Azure'da PaaS Veritabanlarını Güvence Altına Alma | Microsoft Dokümanlar
description: 'PaaS web ve mobil uygulamalarınızın güvenliğini sağlamak için Azure SQL Veritabanı ve SQL Veri Ambarı güvenliği en iyi uygulamaları hakkında bilgi edinin. '
services: security
documentationcenter: na
author: techlake
manager: barbkess
editor: ''
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2018
ms.author: terrylan
ms.openlocfilehash: c73f585e3102618cea378716491f9354810a6db8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80125003"
---
# <a name="best-practices-for-securing-paas-databases-in-azure"></a>Azure'da PaaS veritabanlarını güvence altına almak için en iyi uygulamalar

Bu makalede, hizmet olarak platform (PaaS) web ve mobil uygulamalarınızı güvence altına almak için [Azure SQL Veritabanı](../../sql-database/sql-database-technical-overview.md) ve SQL Veri [Ambarı](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) güvenlik uygulamalarının bir koleksiyonunu tartışıyoruz. Bu en iyi uygulamalar Azure ile olan deneyimlerimizden ve sizin gibi müşterilerin deneyimlerinden elde edilir.

Azure SQL Veritabanı ve SQL Veri Ambarı, internet tabanlı uygulamalarınız için ilişkisel bir veritabanı hizmeti sağlar. Bir PaaS dağıtımında Azure SQL Veritabanı ve SQL Veri Ambarı kullanırken uygulamalarınızın ve verilerinizin korunmasına yardımcı olan hizmetlere bakalım:

- Azure Active Directory kimlik doğrulaması (SQL Server kimlik doğrulaması yerine)
- Azure SQL güvenlik duvarı
- Saydam Veri Şifrelemesi (TDE)

## <a name="use-a-centralized-identity-repository"></a>Merkezi kimlik deposu kullanma
Azure SQL veritabanları iki kimlik doğrulama türünün birini kullanacak şekilde yapılandırılabilir:

- **SQL kimlik doğrulaması** bir kullanıcı adı ve parola kullanır. Veritabanınıza ait mantıksal sunucuyu oluşturduktan sonra kullanıcı adı ve parola belirleyerek "sunucu yöneticisi" oturum açma bilgisi oluşturdunuz. Bu kimlik bilgilerini kullanarak, veritabanı sahibi olarak o sunucudaki herhangi bir veritabanına kimlik doğrulaması yapabilirsiniz.

- **Azure Active Directory kimlik doğrulaması,** Azure Active Directory tarafından yönetilen kimlikleri kullanır ve yönetilen ve tümleşik etki alanları için desteklenir. Azure Etkin Dizin Kimlik Doğrulaması'nı kullanmak için, Azure AD kullanıcılarını ve gruplarını yönetmesine izin verilen "Azure AD yöneticisi" adlı başka bir sunucu yöneticisi oluşturmanız gerekir. Bu yönetici normal bir sunucu yöneticisinin gerçekleştirebileceği tüm işlemleri yapabilir.

[Azure Active Directory kimlik doğrulaması,](../../active-directory/develop/authentication-scenarios.md) Azure Active Directory (AD) kimliklerini kullanarak Azure SQL Veritabanı'na ve SQL Veri Ambarı'na bağlanma mekanizmasıdır. Azure AD, veritabanı sunucuları arasında kullanıcı kimliklerinin yayılmasını durdurabilmeniz için SQL Server kimlik doğrulamasına alternatif sağlar. Azure AD kimlik doğrulaması, veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin kimliklerini tek bir merkezi konumda merkezi olarak yönetmenize olanak tanır. Merkezi kimlik yönetimi, tüm veritabanı kullanıcılarını aynı yerden yönetmenizi sağlar ve izin yönetimini kolaylaştırır.  

### <a name="benefits-of-using-azure-ad-instead-of-sql-authentication"></a>SQL kimlik doğrulaması yerine Azure AD kullanmanın avantajları
- Parola döndürmeyi tek bir yerde sağlar.
- Dış Azure REKLAM gruplarını kullanarak veritabanı izinlerini yönetir.
- Azure AD tarafından desteklenen tümleşik Windows kimlik doğrulaması ve diğer kimlik doğrulama biçimlerini etkinleştirerek parola depolamayı ortadan kaldırır.
- Veritabanı düzeyinde kimlikleri doğrulamak için veritabanı kullanıcıları içeren kullanır.
- SQL Veritabanı'na bağlanan uygulamalar için belirteç tabanlı kimlik doğrulamasını destekler.
- Etki alanı eşitlemesi olmadan yerel bir Azure REKLAMı için Etkin Dizin Federasyonu Hizmetleri (ADFS) veya yerel kullanıcı/parola kimlik doğrulaması ile etki alanı federasyonunu destekler.
- SQL Server Management Studio'nun [Çok Faktörlü Kimlik Doğrulama (MFA)](/azure/active-directory/authentication/multi-factor-authentication)içeren Active Directory Universal Authentication kullanan bağlantılarını destekler. MFA telefon görüşmesi, SMS mesajı, pin ile akıllı kart veya mobil uygulama bildirimi gibi çok sayıda kolay doğrulama seçeneğine sahiptir. Daha fazla bilgi için, [SQL Veritabanı ve SQL Veri Ambarı ile Evrensel Kimlik Doğrulama'ya](../../sql-database/sql-database-ssms-mfa-authentication.md)bakın.

Azure AD kimlik doğrulaması hakkında daha fazla bilgi edinmek için bkz:

- [SQL Veritabanı, Yönetilen Örnek veya SQL Veri Ambarı ile kimlik doğrulama için Azure Active Directory Authentication'ı kullanma](../../sql-database/sql-database-aad-authentication.md)
- [Azure SQL Veri Ambarı’nda kimlik doğrulama](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-authentication.md)
- [Azure AD kimlik doğrulamasını kullanarak Azure SQL DB için belirteç tabanlı kimlik doğrulama desteği](../../sql-database/sql-database-aad-authentication.md)

> [!NOTE]
> Azure Etkin Dizininin ortamınız için uygun olduğundan emin olmak için [Azure REKLAM özelliklerine ve sınırlamalarına](../../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations)bakın.
>
>

## <a name="restrict-access-based-on-ip-address"></a>IP adresine göre erişimi kısıtlama
Kabul edilebilir IP adresleriaralıklarını belirten güvenlik duvarı kuralları oluşturabilirsiniz. Bu kurallar hem sunucu hem de veritabanı düzeylerini hedefleyebilir. Güvenliği artırmak ve veritabanınızı daha taşınabilir hale getirmek için mümkün olduğunda veritabanı düzeyinde güvenlik duvarı kurallarını kullanmanızı öneririz. Sunucu düzeyinde güvenlik duvarı kuralları en iyi yöneticiler için kullanılır ve aynı erişim gereksinimlerine sahip ancak her veritabanını ayrı ayrı yapılandırmak için zaman harcamak istemiyorsanız birçok veritabanınız varsa.

SQL Veritabanı varsayılan kaynak IP adresi kısıtlamaları, diğer abonelikler ve kiracılar da dahil olmak üzere herhangi bir Azure adresinden erişime izin verir. Bunu yalnızca IP adreslerinizin örne erişmesine izin vermek için kısıtlayabilirsiniz. SQL güvenlik duvarı nız ve IP adresi kısıtlamalarınızla bile güçlü kimlik doğrulamanız gereklidir. Bu makalede daha önce yapılan önerilere bakın.

Azure SQL Güvenlik Duvarı ve IP kısıtlamaları hakkında daha fazla bilgi edinmek için bkz:

- [Azure SQL Veritabanı ve SQL Veri Ambarı erişim denetimi](../../sql-database/sql-database-manage-logins.md)
- [Azure SQL Veritabanı ve SQL Veri Ambarı güvenlik duvarı kuralları](../../sql-database/sql-database-firewall-configure.md)


## <a name="encrypt-data-at-rest"></a>Bekleyen verileri şifreleme
[Saydam Veri Şifreleme (TDE)](/sql/relational-databases/security/encryption/transparent-data-encryption) varsayılan olarak etkinleştirilir. TDE, SQL Server, Azure SQL Veritabanı ve Azure SQL Veri Ambarı verilerini ve günlük dosyalarını saydam olarak şifreler. TDE, dosyalara veya bunların yedeğine doğrudan erişim inancıya karşı koruma sağlar. Bu, varolan uygulamaları değiştirmeden verileri istirahatte şifrelemenizi sağlar. TDE her zaman etkin kalmalıdır; ancak, bu normal erişim yolunu kullanarak bir saldırganın durmaz. TDE, çeşitli sektörlerde oluşturulan birçok yasa, yönetmelik ve yönergeye uyma olanağı sağlar.

Azure SQL, TDE ile ilgili önemli sorunları yönetir. TDE'de olduğu gibi, kurtarılabilirliği sağlamak ve veritabanlarını hareket ettirirken şirket içinde özel özen denilmelidir. Daha karmaşık senaryolarda, anahtarlar genişletilebilir anahtar yönetimi aracılığıyla Azure Anahtar Kasası'nda açıkça yönetilebilir. Bkz. [EKM kullanarak SQL Server'da TDE'yi etkinleştir.](/sql/relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm) Bu aynı zamanda Azure Key Vaults BYOK özelliği aracılığıyla Kendi Anahtarınızı Getir 'e (BYOK) olanak tanır.

Azure [SQL, Her Zaman Şifrelenmiş](/sql/relational-databases/security/encryption/always-encrypted-database-engine)sütunlar için şifreleme sağlar. Bu, yalnızca yetkili uygulamaların hassas sütunlara erişmesine olanak tanır. Bu tür şifreleme kullanmak, şifrelenmiş sütunlar için SQL sorgularını eşitlik tabanlı değerlerle sınırlar.

Uygulama düzeyi şifreleme sevesi verileri için de kullanılmalıdır. Veri egemenliği endişeleri bazen doğru ülkede/bölgede tutulan bir anahtarla verilerin şifrelenerek azaltılabilir. Bu, güçlü bir algoritma kullanıldığını varsayarak (AES 256 gibi) anahtar olmadan verilerin şifresini çözmek imkansız olduğundan, yanlışlıkla veri aktarımını bile soruna neden olmaktan çıkarır.

Güvenli bir sistem tasarlamak, gizli varlıkları şifrelemek ve veritabanı sunucuları etrafında bir güvenlik duvarı oluşturmak gibi veritabanının güvenliğini sağlamaya yardımcı olmak için ek önlemler kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, PaaS web ve mobil uygulamalarınızı güvence altına almak için SQL Veritabanı ve SQL Veri Ambarı güvenlik en iyi uygulamaları bir koleksiyon tanıttı. PaaS dağıtımlarınızın güvenliğini sağlama hakkında daha fazla bilgi edinmek için bkz:

- [PaaS dağıtımlarının güvenliğini sağlama](paas-deployments.md)
- [Azure Uygulama Hizmetlerini Kullanarak PaaS web ve mobil uygulamaları güvence altına alma](paas-applications-using-app-services.md)
