---
title: İsteğe bağlı SQL (önizleme)
description: Azure Synapse Analytics'te isteğe bağlı Synapse SQL hakkında bilgi edinin.
services: synapse analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 8d4de424d5d4d6da1ee80e04b35e63ae29df57c8
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424912"
---
# <a name="sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te isteğe bağlı SQL (önizleme) 

Her Azure Synapse Analytics çalışma alanı (önizleme), göldeki verileri sorgulamak için kullanabileceğiniz isteğe bağlı (önizleme) uç noktalarıyla birlikte gelir.

SQL isteğe bağlı, veri gölünüzdeki veriler üzerinde bir sorgu hizmetidir. Verilerinize aşağıdaki işlevler aracılığıyla erişmenizi sağlar:
 
- Verileri özel bir depoya kopyalamaya veya yüklemeye gerek kalmadan verileri yerinde sorgulamak için tanıdık bir T-SQL sözdizimi. 
- En popüler sürücüler de dahil olmak üzere çok çeşitli iş zekası ve geçici sorgulama araçları sunan T-SQL arabirimi üzerinden entegre bağlantı. 

İsteğe bağlı SQL, büyük ölçekli veri ve işlem için oluşturulmuş dağıtılmış bir veri işleme sistemidir. İsteğe bağlı OLARAK SQL isteğe bağlı olarak Büyük Verilerinizi saniyesaniye analiz etmenizi sağlar. Yerleşik sorgu yürütme hatası toleransı sayesinde, sistem büyük veri kümelerini içeren uzun süren sorgular için bile yüksek güvenilirlik ve başarı oranları sağlar.

Sql on-demand sunucusuzdur, bu nedenle kurulum veya kümeleri korumak için hiçbir altyapı yoktur. Bu hizmet için varsayılan bir bitiş noktası her Azure Synapse çalışma alanında sağlanır, böylece çalışma alanı oluşturulur oluşturulmaz verileri sorgulamaya başlayabilirsiniz. Ayrılmış kaynaklar için herhangi bir ücret yoktur, yalnızca çalıştırdığınız sorgular tarafından taranan veriler için ücretlendirilirsiniz, bu nedenle bu model gerçek bir kullanım başına ödeme modelidir.  

Veri hazırlama, temizleme veya zenginleştirme için veri ardışık alanınızda Spark kullanıyorsanız, işlemde oluşturduğunuz [Spark tablolarını](develop-storage-files-spark-tables.md) doğrudan isteğe bağlı SQL'den sorgulayabilirsiniz. SQL isteğe bağlı bitiş noktanızı yönetilen [çalışma alanı VNet'inize](../security/synapse-workspace-managed-vnet.md)getirmek için [Özel Bağlantı'yı](../security/how-to-connect-to-workspace-with-private-links.md) kullanın.  

## <a name="who-is-sql-on-demand-for"></a>Sql isteğe bağlı olarak kimler

Veri gölündeki verileri keşfetmeniz, ondan öngörüler elde etmeniz veya mevcut veri dönüştürme ardışık alanınızın optimizasyonu yapmanız gerekiyorsa, isteğe bağlı SQL'i kullanarak yararlanabilirsiniz. Aşağıdaki senaryolar için uygundur:

- Temel keşif ve keşif - Veri gölünüzdeki çeşitli biçimlerde (Parke, CSV, JSON) veriler hakkında hızlı bir şekilde neden olun, böylece ondan nasıl içgörü elde edebileceğinizi planlayabilirsiniz.
- Mantıksal veri ambarı – Verilerinizin her zaman güncel bir şekilde görüntülemesine olanak tanıyan, verilerinizin taşınması ve dönüştürülmesi olmadan ham veya birbirinden farklı verilerin üstüne ilişkisel bir soyutlama sağlayın.
- Veri dönüşümü - T-SQL kullanarak göldeki verileri dönüştürmenin basit, ölçeklenebilir ve performant yolu, böylece BI ve diğer araçlara aktarılabilir veya ilişkisel veri deposuna yüklenebilir (Synapse SQL veritabanları, Azure SQL Veritabanı, vb.).

Farklı profesyonel roller, isteğe bağlı SQL'den yararlanabilir:

- Veri Mühendisleri bu hizmeti kullanarak gölü keşfedebilir, verileri dönüştürebilir ve hazırlayabilir ve veri dönüşüm boru hatlarını basitleştirebilir. Daha fazla bilgi için bu öğreticiyi kontrol [edin.](tutorial-data-analyst.md)
- Data Scientists, OPENROWSET ve otomatik şema çıkarımı gibi özellikler sayesinde göldeki verilerin içeriği ve yapısı hakkında hızlı bir şekilde muhakeme edebilirler.
- Veri Analistleri, tanıdık T-SQL dilini veya isteğe bağlı SQL'e bağlanabilen en sevdikleri araçları kullanarak Veri Bilimcileri veya Veri Mühendisleri tarafından oluşturulan [verileri ve Kıvılcım tablolarını keşfedebilir.](develop-storage-files-spark-tables.md)
- BI Uzmanları, göl ve Spark [tablolarındaki verilerin üzerine güç bi raporları](tutorial-connect-power-bi-desktop.md) oluşturabilir.

## <a name="what-do-i-need-to-do-to-start-using-it"></a>Kullanmaya başlamak için ne yapmam gerekiyor?

Her Azure Synapse çalışma alanında SQL isteğe bağlı bitiş noktası sağlanır. Bir çalışma alanı oluşturabilir ve aşina olduğunuz araçları kullanarak verileri anında sorgulamaya başlayabilirsiniz.

## <a name="client-tools"></a>İstemci araçları

İsteğe bağlı SQL, varolan SQL geçici sorgulama ve iş zekası araçlarının veri gölüne girmesini sağlar. Tanıdık T-SQL sözdizimi sağladığından, TDS bağlantısı SQL teklifleri oluşturabilen herhangi bir araç [Synapse SQL'e isteğe bağlı olarak bağlanabilir ve sorgulayabilir.](connect-overview.md) Azure Veri Stüdyosu'na bağlanabilir ve birkaç dakika içinde öngörüler elde etmek için geçici sorgular çalıştırabilir veya Power BI'ye bağlanabilirsiniz.

## <a name="is-full-t-sql-supported"></a>Tam T-SQL desteklendi mi?

SQL isteğe bağlı olarak, yarı yapılandırılmış ve yapılandırılmamış verileri sorgulama konusundaki deneyimleri karşılamak için bazı yönlerden biraz geliştirilmiş/genişletilmiş T-SQL sorgu yüzey alanı sunar. Ayrıca, T-SQL dilinin bazı yönleri, isteğe bağlı SQL tasarımı nedeniyle desteklenmez, örnek olarak, DML işlevselliği şu anda desteklenmez.

- İş yükü tanıdık kavramlar kullanılarak düzenlenebilir:
- Veritabanları - SQL on-demand endpoint birden çok veritabanı olabilir.
- Şemalar - Bir veritabanı içinde şema adı verilen bir veya çok sayıda nesne sahiplik grubu olabilir.
- Görünümler
- Dış kaynaklar – veri kaynakları, dosya biçimleri ve tablolar

Güvenlik şu şekilde zorlanabilir:

- Oturum açma bilgileri ve kullanıcılar
- Depolama hesaplarına erişimi denetlemek için kimlik bilgileri
- Nesne düzeyi başına izinleri verme, reddetme ve iptal etme
- Azure Active Directory tümleştirmesi

Desteklenen T-SQL:

- SQL fonksiyonlarının çoğunluğu da dahil olmak üzere tam [SELECT](/sql/t-sql/queries/select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) yüzey alanı desteklenir
- ÇETAŞ - SELECT OLARAK Dış TABLO OLUŞTUR
- Yalnızca görünümler ve güvenlikle ilgili DDL ifadeleri

İsteğe bağlı SQL'de yerel depolama alanı yoktur, yalnızca meta veri nesneleri veritabanlarında depolanır. Bu nedenle, Aşağıdaki kavramlarla ilgili T-SQL desteklenmez:

- Tablolar
- Tetikleyiciler
- Somutlaştırılmış görünümler
- Görünümler ve güvenlikle ilgili olanlar dışındaki DDL ifadeleri
- DML deyimleri

### <a name="extensions"></a>Uzantıları

Sql, veri gölündeki dosyalarda bulunan verilerin yerinde sorgulanması için sorunsuz bir deneyim sağlamak için, isteğe bağlı olarak aşağıdaki yetenekleri ekleyerek varolan [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) işlevini genişletir:

[Birden çok dosya veya klasörü sorgula](develop-storage-files-overview.md#query-multiple-files-or-folders)

[PARKE dosya formatı](develop-storage-files-overview.md#parquet-file-format)

[Sınırlı metinle çalışmak için ek seçenekler (alan sonlandırıcısı, satır sonlandırıcısı, kaçış char)](develop-storage-files-overview.md#additional-options-for-working-with-delimited-text)

[Sütunların seçilen bir alt kümesini okuma](develop-storage-files-overview.md#read-a-chosen-subset-of-columns)

[Şema çıkarımı](develop-storage-files-overview.md#schema-inference)

[dosya adı fonksiyonu](develop-storage-files-overview.md#filename-function)

[filepath fonksiyonu](develop-storage-files-overview.md#filepath-function)

[Karmaşık türleri ve iç içe veya yinelenen veri yapıları ile çalışma](develop-storage-files-overview.md#work-with-complex-types-and-nested-or-repeated-data-structures)

## <a name="security"></a>Güvenlik

İsteğe bağlı SQL, verilerinize erişimi güvence altına almak için mekanizmalar sunar.

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory tümleştirmesi ve çok faktörlü kimlik doğrulaması

İsteğe bağlı SQL, [Azure Active Directory tümleştirmesi](../../sql-database/sql-database-Azure AD-authentication.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)ile veritabanı kullanıcısının ve diğer Microsoft hizmetlerinin kimliklerini merkezi olarak yönetmenize olanak tanır. Bu özellik, izin yönetimini kolaylaştırırken güvenliği artırır. Azure Etkin Dizin (Azure AD), tek bir oturum açma işlemini desteklerken veri ve uygulama güvenliğini artırmak için [çok faktörlü kimlik doğrulamayı](../../sql-database/sql-database-ssms-mfa-authentication-configure.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) (MFA) destekler.

#### <a name="authentication"></a>Kimlik Doğrulaması

İsteğe bağlı SQL kimlik doğrulaması, kullanıcıların bitiş noktasına bağlanırken kimliklerini nasıl kanıtladığını ifade eder. İki tür kimlik doğrulama desteklenir:

- **SQL Kimlik Doğrulama**

  Bu kimlik doğrulama yöntemi bir kullanıcı adı ve parola kullanır.

- **Azure Etkin Dizin Kimlik Doğrulaması**:

  Bu kimlik doğrulama yöntemi, Azure Etkin Dizini tarafından yönetilen kimlikleri kullanır. Azure AD kullanıcıları için çok faktörlü kimlik doğrulama etkinleştirilebilir. [Mümkün olduğunda](/sql/relational-databases/security/choose-an-authentication-mode?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)Etkin Dizin kimlik doğrulaması (tümleşik güvenlik) kullanın.

#### <a name="authorization"></a>Yetkilendirme

Yetkilendirme, kullanıcının isteğe bağlı bir SQL veritabanı içinde neler yapabileceğini ifade eder ve kullanıcı hesabınızın veritabanı rol üyelikleri ve nesne düzeyindeizinler tarafından denetlenir.

SQL Kimlik Doğrulaması kullanılırsa, SQL kullanıcısı yalnızca isteğe bağlı OLARAK SQL'de bulunur ve izinler isteğe bağlı OLARAK SQL'deki nesnelere yöneliktir. Diğer hizmetlerdeki (Azure Depolama gibi) değerli nesnelere erişim, yalnızca isteğe bağlı SQL kapsamında olduğundan doğrudan SQL kullanıcısına verilemez. SQL kullanıcısının dosyalara erişmek için [desteklenen yetkilendirme türlerinden](develop-storage-files-storage-access-control.md#supported-storage-authorization-types) birini kullanması gerekir.

Azure AD kimlik doğrulaması kullanılırsa, kullanıcı isteğe bağlı OLARAK SQL'de oturum açabilir ve Azure Depolama gibi diğer hizmetlerde oturum açabilir ve Azure AD kullanıcısına izin verebilir.

### <a name="access-to-storage-accounts"></a>Depolama hesaplarına erişim

Sql isteğe bağlı hizmetine giriş yapan bir kullanıcının Azure Depolama'daki dosyalara erişme ve sorgu yetkisi ne kadar yetkilendirilebilir. İsteğe bağlı SQL aşağıdaki yetkilendirme türlerini destekler:

- **Paylaşılan erişim imzası (SAS),** depolama hesabındaki kaynaklara temsilci erişimi sağlar. SAS ile, müşterilere hesap anahtarlarını paylaşmadan depolama hesabındaki kaynaklara erişim izni verebilirsiniz. SAS, SAS'a sahip istemcilere sağladığınız erişim türü üzerinde ayrıntılı denetim sağlar: geçerlilik aralığı, verilen izinler, kabul edilebilir IP adresi aralığı, kabul edilebilir protokol (https/http).

- **Kullanıcı Kimliği** ("geçiş" olarak da bilinir), sql'e isteğe bağlı olarak giriş yapan Azure AD kullanıcısının kimliğinin verilere erişimi yetkilendirmek için kullanıldığı bir yetkilendirme türüdür. Verilere erişmeden önce Azure Depolama yöneticisinin verilere erişmek için Azure AD kullanıcısına izin vermesi gerekir. Bu yetkilendirme türü, isteğe bağlı OLARAK SQL'e giriş yapan Azure AD kullanıcısını kullanır, bu nedenle SQL kullanıcı türleri için desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
Uç nokta bağlantısı ve sorgu dosyaları hakkında ek bilgiler aşağıdaki makalelerde bulunabilir: 
- [Bitiş noktanıza bağlanın](connect-overview.md)
- [Dosyalarınızı sorgula](develop-storage-files-overview.md)
