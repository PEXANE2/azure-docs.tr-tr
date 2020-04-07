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
ms.openlocfilehash: fcd9607cd630f41c67c655c2e8016254600b52e2
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80745384"
---
# <a name="authenticate-to-azure-synapse-analytics"></a>Azure Synapse Analytics'e kimlik doğrulama

Azure Active Directory (AAD) veya SQL Server kimlik doğrulaması kullanarak Azure Sinaps'ta SQL Analytics'e nasıl kimlik doğrulaması yapılacağını öğrenin.

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

## <a name="azure-active-directory-aad-authentication"></a>Azure Etkin Dizin (AAD) kimlik doğrulaması

[Azure Etkin Dizin](../../active-directory/fundamentals/active-directory-whatis.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) kimlik doğrulaması, Azure Etkin Dizin'de (Azure AD) kimlikleri kullanarak SQL havuzuna bağlanma mekanizmasıdır. Azure Active Directory kimlik doğrulaması ile veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin kimliklerini merkezi olarak tek bir merkezi konumda yönetebilirsiniz. Merkezi kimlik yönetimi, Azure Synapse kullanıcılarını yönetmek için tek bir yer sağlar ve izin yönetimini kolaylaştırır.

### <a name="benefits"></a>Yararları

Azure Etkin Dizin avantajları şunlardır:

* SQL Server kimlik doğrulamasına alternatif sağlar.
* Veritabanı sunucuları arasında kullanıcı kimliklerinin çoğalmasını durdurmaya yardımcı olur.
* Tek konumda parola dönüşüne olanak sağlar
* Dış (Azure AD) gruplarını kullanarak veritabanı izinlerini yönetin.
* Azure Active Directory tarafından desteklenen tümleşik Windows kimlik doğrulaması ve diğer kimlik doğrulama biçimlerini etkinleştirerek parola depolamayı ortadan kaldırır.
* Veritabanı düzeyinde kimlikleri doğrulamak için veritabanı kullanıcıları içeren kullanır.
* SQL havuzuna bağlanan uygulamalar için belirteç tabanlı kimlik doğrulamasını destekler.
* [SQL Server Management Studio](../../sql-database/sql-database-ssms-mfa-authentication.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) ve SQL Server Data [Tools](/sql/ssdt/azure-active-directory?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)gibi çeşitli araçlar için Active Directory Universal Authentication aracılığıyla Çoklu Faktörlü kimlik doğrulamayı destekler.

> [!NOTE]
> Azure Etkin Dizin ilerki hala nispeten yenidir ve bazı sınırlamaları vardır. Azure Etkin Dizininin ortamınız için uygun olduğundan emin olmak için Azure [REKLAM özelliklerine ve sınırlamalarına](../../sql-database/sql-database-aad-authentication.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#azure-ad-features-and-limitations)(özellikle Ek hususlara) bakın.

### <a name="configuration-steps"></a>Yapılandırma adımları

Azure Etkin Dizin kimlik doğrulamasını yapılandırmak için aşağıdaki adımları izleyin.

1. Azure Etkin Dizin oluşturma ve doldurma
2. İsteğe bağlı: Azure Aboneliğinizle ilişkili olan etkin dizinini ilişkilendirme veya değiştirme
3. Azure Synapse için bir Azure Etkin Dizin yöneticisi oluşturma
4. İstemci bilgisayarlarınızı yapılandırma
5. Veritabanınızda Azure AD kimliklerine eşlenen veritabanında bulunan veritabanı kullanıcıları oluşturma
6. Azure AD kimliklerini kullanarak SQL havuzunuza bağlanın

Şu anda Azure Etkin Dizin kullanıcıları SSDT Object Explorer'da gösterilmez. Geçici çözüm olarak, [sys.database_principals'deki](/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest#azure-ad-features-and-limitations)kullanıcıları görüntüleyin.

### <a name="find-the-details"></a>Ayrıntıları bulun

* Azure Active Directory kimlik doğrulamasını yapılandırma ve kullanma adımları, Azure Sinaps'taki Azure SQL Veritabanı ve SQL Analytics için hemen hemen aynıdır. [Azure Active Directory Authentication'ı kullanarak SQL Veritabanına veya SQL Havuzuna Bağlanma](../../sql-database/sql-database-aad-authentication.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)konusundaki ayrıntılı adımları izleyin.
* Özel veritabanı rolleri oluşturun ve rolleri kullanıcılarekleyin. Ardından rollere ayrıntılı izinler tanıyın. Daha fazla bilgi için veritabanı [altyapısı izinleri ile başlarken](/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio ve diğer uygulamalarla sorgulamaya başlamak için [Visual Studio ile Sorgula'ya](sql-data-warehouse-query-visual-studio.md)bakın.
