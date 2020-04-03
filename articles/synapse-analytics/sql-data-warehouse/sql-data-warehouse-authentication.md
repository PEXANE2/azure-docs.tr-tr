---
title: Kimlik Doğrulaması
description: Azure Active Directory (Azure AD) veya SQL Server kimlik doğrulaması kullanarak Azure Synapse Analytics'e nasıl kimlik doğrulaması yapılacağını öğrenin.
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/02/2019
ms.author: jrasnick
ms.reviewer: igorstan
ms.custom: seo-lt-2019
tag: azure-synapse
ms.openlocfilehash: ccc5db828a03c37d3fc4f49b13883ac3eeda2368
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80584226"
---
# <a name="authenticate-to-azure-synapse-analytics"></a>Azure Synapse Analytics'e kimlik doğrulama

Azure Active Directory (Azure AD) veya SQL Server kimlik doğrulaması kullanarak Azure Synapse'de Synapse SQL poool'a nasıl kimlik doğrulaması yapılacağını öğrenin.

BIR SQL havuzuna bağlanmak için kimlik doğrulama amacıyla güvenlik kimlik bilgilerini geçmeniz gerekir. Bağlantı kurduktan sonra, sorgu oturumunuzu kurmanın bir parçası olarak belirli bağlantı ayarları yapılandırılır.  

Güvenlik ve veri ambarınıza bağlantıları etkinleştirme hakkında daha fazla bilgi için [bir veritabanı belgesinin güvenliğini sağlama](sql-data-warehouse-overview-manage-security.md)bilgisine bakın.

## <a name="sql-authentication"></a>SQL kimlik doğrulaması

SQL havuzuna bağlanmak için aşağıdaki bilgileri sağlamanız gerekir:

* Tam nitelikli sunucu adı
* SQL kimlik doğrulamasını belirtin
* Kullanıcı adı
* Parola
* Varsayılan veritabanı (isteğe bağlı)

Varsayılan olarak, bağlantınız kullanıcı veritabanınıza *değil, ana* veritabanına bağlanır. Kullanıcı veritabanınıza bağlanmak için iki şeyden birini yapmayı seçebilirsiniz:

* Sunucunuzu SSDT, SSMS veya uygulama bağlantı dizenizde SQL Server Object Explorer'a kaydederken varsayılan veritabanını belirtin. Örneğin, Bir ODBC bağlantısı için Başlangıç Kataloğu parametresini ekleyin.
* SSDT'de oturum oluşturmadan önce kullanıcı veritabanını vurgulayın.

> [!NOTE]
> Transact-SQL deyimi **USE MyDatabase;** bir bağlantı için veritabanını değiştirmek için desteklenmez. SSDT içeren bir SQL havuzuna bağlanan kılavuzlar için [Visual Studio makalesini sorgula](sql-data-warehouse-query-visual-studio.md) makalesine bakın.
> 
> 

## <a name="azure-active-directory-azure-ad-authentication"></a>Azure Etkin Dizin (Azure AD) kimlik doğrulaması

[Azure Etkin Dizin](../../active-directory/fundamentals/active-directory-whatis.md) kimlik doğrulaması, Azure Etkin Dizin'de (Azure AD) kimlikleri kullanarak SQL havuzuna bağlanma mekanizmasıdır. Azure Active Directory kimlik doğrulaması ile veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin kimliklerini merkezi olarak tek bir merkezi konumda yönetebilirsiniz. Merkezi kimlik yönetimi, Azure Synapse kullanıcılarını yönetmek için tek bir yer sağlar ve izin yönetimini kolaylaştırır. 

### <a name="benefits"></a>Avantajlar

Azure Etkin Dizin avantajları şunlardır:

* SQL Server kimlik doğrulamasına alternatif sağlar.
* Veritabanı sunucuları arasında kullanıcı kimliklerinin çoğalmasını durdurmaya yardımcı olur.
* Tek konumda parola dönüşüne olanak sağlar
* Dış (Azure AD) gruplarını kullanarak veritabanı izinlerini yönetin.
* Azure Active Directory tarafından desteklenen tümleşik Windows kimlik doğrulaması ve diğer kimlik doğrulama biçimlerini etkinleştirerek parola depolamayı ortadan kaldırır.
* Veritabanı düzeyinde kimlikleri doğrulamak için veritabanı kullanıcıları içeren kullanır.
* SQL havuzuna bağlanan uygulamalar için belirteç tabanlı kimlik doğrulamasını destekler.
* [SQL Server Management Studio](../../sql-database/sql-database-ssms-mfa-authentication.md) ve SQL Server Data [Tools](https://docs.microsoft.com/sql/ssdt/azure-active-directory?toc=/azure/sql-data-warehouse/toc.json)gibi çeşitli araçlar için Active Directory Universal Authentication aracılığıyla Çoklu Faktörlü kimlik doğrulamayı destekler.

> [!NOTE]
> Azure Etkin Dizin ilerki hala nispeten yenidir ve bazı sınırlamaları vardır. Azure Etkin Dizininin ortamınız için uygun olduğundan emin olmak için Azure [REKLAM özelliklerine ve sınırlamalarına](../../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations)(özellikle Ek hususlara) bakın.
> 
> 

### <a name="configuration-steps"></a>Yapılandırma adımları

Azure Etkin Dizin kimlik doğrulamasını yapılandırmak için aşağıdaki adımları izleyin.

1. Azure Etkin Dizin oluşturma ve doldurma
2. İsteğe bağlı: Azure Aboneliğinizle ilişkili olan etkin dizinini ilişkilendirme veya değiştirme
3. Azure Synapse için bir Azure Etkin Dizin yöneticisi oluşturma
4. İstemci bilgisayarlarınızı yapılandırma
5. Veritabanınızda Azure AD kimliklerine eşlenen veritabanında bulunan veritabanı kullanıcıları oluşturma
6. Azure AD kimliklerini kullanarak SQL havuzunuza bağlanın

Şu anda Azure Etkin Dizin kullanıcıları SSDT Object Explorer'da gösterilmez. Geçici çözüm olarak, [sys.database_principals'deki](https://msdn.microsoft.com/library/ms187328.aspx)kullanıcıları görüntüleyin.

### <a name="find-the-details"></a>Ayrıntıları bulun

* Azure Active Directory kimlik doğrulamasını yapılandırma ve kullanma adımları, Azure Sinaps'taki Azure SQL Veritabanı ve Synapse SQL havuzu için hemen hemen aynıdır. [Azure Active Directory Authentication'ı kullanarak SQL Veritabanına veya SQL Havuzuna Bağlanma](../../sql-database/sql-database-aad-authentication.md)konusundaki ayrıntılı adımları izleyin.
* Özel veritabanı rolleri oluşturun ve rolleri kullanıcılarekleyin. Ardından rollere ayrıntılı izinler tanıyın. Daha fazla bilgi için veritabanı [altyapısı izinleri ile başlarken](https://msdn.microsoft.com/library/mt667986.aspx)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio ve diğer uygulamalarla sorgulamaya başlamak için [Visual Studio ile Sorgula'ya](sql-data-warehouse-query-visual-studio.md)bakın.
