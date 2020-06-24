---
title: İsteğe bağlı SQL (Önizleme)
description: Azure SYNAPSE Analytics 'te SYNAPSE SQL isteğe bağlı hakkında bilgi edinin.
services: synapse analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 3f020abd4d207a49068ccffb9bb7f57b88bc87df
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85206402"
---
# <a name="sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SQL isteğe bağlı (Önizleme) 

Her Azure SYNAPSE Analytics çalışma alanı (Önizleme), Gölü verileri sorgulamak için kullanabileceğiniz, SQL isteğe bağlı (Önizleme) uç noktalarıyla gelir.

İsteğe bağlı SQL, Data Lake 'inizdeki veriler üzerinde bir sorgu hizmetidir. Verilerinize aşağıdaki işlevleri kullanarak erişmenizi sağlar:
 
- Verileri özelleştirilmiş bir depoya kopyalama veya yüklemeye gerek kalmadan verileri sorgulamak için tanıdık bir T-SQL söz dizimi. 
- En popüler sürücüler dahil olmak üzere çok çeşitli iş zekası ve geçici sorgulama araçları sunan T-SQL arabirimi aracılığıyla tümleşik bağlantı. 

İsteğe bağlı SQL, büyük ölçekli veri ve işlem için oluşturulmuş bir dağıtılmış veri işleme sistemidir. SQL isteğe bağlı olarak, büyük verilerinizi iş yüküne bağlı olarak birkaç dakika içinde analiz etmenizi sağlar. Yerleşik sorgu yürütme hata toleransı sayesinde sistem, büyük veri kümeleri içeren uzun süreli sorgularda bile yüksek güvenilirlik ve başarı oranları sağlar.

İsteğe bağlı SQL Server sunucusuz, bu nedenle, bakım yapmak için kurulum veya kümelerin bir altyapısı yoktur. Bu hizmet için varsayılan bir uç nokta her Azure SYNAPSE çalışma alanı içinde sağlanır, bu sayede çalışma alanı oluşturulduktan hemen sonra verileri sorgulamaya başlayabilirsiniz. Ayrılan kaynaklar için ücret alınmaz, yalnızca çalıştırdığınız sorgular tarafından Taranan veriler için ücretlendirilirsiniz, bu nedenle bu modelin kullanım başına doğru bir ödeme modeli olması gerekir.  

Veri işlem hattınızda Azure SYNAPSE için Apache Spark kullanıyorsanız, veri hazırlama, temizleme veya zenginleştirme için, işlem içinde oluşturduğunuz [Harici Spark tablolarını](develop-storage-files-spark-tables.md) doğrudan SQL isteğe bağlı olarak sorgulayabilirsiniz. SQL isteğe bağlı uç noktanızı [yönetilen çalışma alanı sanal](../security/synapse-workspace-managed-vnet.md)ağınıza taşımak Için [özel bağlantıyı](../security/how-to-connect-to-workspace-with-private-links.md) kullanın.  

## <a name="who-is-sql-on-demand-for"></a>İçin SQL isteğe bağlı kim

Veri Gölü verileri araştırmanıza, bundan Öngörüler elde etmeniz veya mevcut veri dönüştürme işlem hattınızı iyileştirmeniz gerekiyorsa, SQL 'i isteğe bağlı olarak kullanmaya yarar sağlayabilirsiniz. Aşağıdaki senaryolar için uygundur:

- Temel bulma ve araştırmayla ilgili veri Gölü biçimlerdeki veriler hakkında hızlı bir neden (Parquet, CSV, JSON), bu sayede öngörüleri nasıl ayıklayabileceğinizi planlayabileceksiniz.
- Mantıksal veri ambarı: verilerin üzerine konumlandırmadan ve dönüştürmeden verileri her zaman güncel görüntülemeye izin vererek ham veya farklı verilerin üzerinde ilişkisel bir soyutlama sağlar.
- Veri dönüştürme-T-SQL ' y i kullanarak Gölü verileri dönüştürmenin basit, ölçeklenebilir ve performanslı yolu, bı ve diğer araçlara geçirilebilir veya bir ilişkisel veri deposuna (SYNAPSE SQL veritabanları, Azure SQL veritabanı vb.) yüklenebilir.

Farklı profesyonel roller, isteğe bağlı SQL 'den faydalanabilir:

- Veri mühendisleri gölü keşfedebilir, bu hizmeti kullanarak verileri dönüştürebilir ve hazırlayabilir ve veri dönüştürme işlem hatlarını basitleştirir. Daha fazla bilgi için bu [öğreticiye](tutorial-data-analyst.md)bakın.
- Veri bilimcileri, OPENROWSET ve otomatik Şema çıkarımı gibi özellikler sayesinde Gölü verilerin içerikleri ve yapısı hakkında hızlı bir neden olabilir.
- Veri analistleri, tanıdık T-SQL dili veya en sevdiğiniz araçları kullanarak veri bilimcileri veya veri mühendisleri tarafından oluşturulan [veri ve Spark dış tablolarını](develop-storage-files-spark-tables.md) , Isteğe bağlı SQL 'e bağlanabilecek şekilde keşfedebilir.
- Bı uzmanları, Gölü ve Spark tablolarındaki verilerin üzerine hızlı [bir şekilde Power BI raporlar oluşturabilir](tutorial-connect-power-bi-desktop.md) .

## <a name="what-do-i-need-to-do-to-start-using-it"></a>Kullanmaya başlamak için ne yapmam gerekir?

SQL isteğe bağlı uç noktası her Azure SYNAPSE çalışma alanı içinde sağlanır. Bir çalışma alanı oluşturabilir ve tanıdığınız araçları kullanarak verileri anında sorgulamaya başlayabilirsiniz.

## <a name="client-tools"></a>İstemci araçları

SQL isteğe bağlı, mevcut SQL geçici sorgulama ve iş zekası araçlarının Veri Gölü içine dokunmasına olanak sağlar. Tanıdık T-SQL söz dizimini sağladığından, TDS bağlantısı SQL teklifleri oluşturmaya yönelik herhangi bir araç, isteğe bağlı [SYNAPSE SQL 'e bağlanabilir ve sorgu](connect-overview.md) oluşturabilir. Azure Data Studio ile bağlanabilir, geçici sorgular çalıştırabilir veya birkaç dakika içinde Öngörüler elde etmek için Power BI ile bağlanabilirsiniz.

## <a name="is-full-t-sql-supported"></a>Tam T-SQL destekleniyor mu?

SQL isteğe bağlı, yarı yapılandırılmış ve yapılandırılmamış verileri sorgulama konusunda deneyimlere uyum sağlamak için bazı yönlerde biraz gelişmiş/genişletilmiş olan T-SQL sorgulama yüzey alanı sunar. Ayrıca, SQL isteğe bağlı SQL 'in tasarımı nedeniyle, örnek olarak DML işlevselliği Şu anda desteklenmediğinden T-SQL dilinin bazı yönleri desteklenmez.

- İş yükü tanıdık kavramlar kullanılarak düzenlenebilir:
- Veritabanları-isteğe bağlı SQL uç noktası birden çok veritabanına sahip olabilir.
- Şemalar-bir veritabanı Içinde, şemalar adlı bir veya daha fazla nesne sahiplik grubu olabilir.
- Görünümler
- Dış kaynaklar – veri kaynakları, dosya biçimleri ve tablolar

Şu kullanılarak güvenlik zorlanabilir:

- Oturum açma bilgileri ve kullanıcılar
- Depolama hesaplarına erişimi denetlemek için kimlik bilgileri
- Nesne düzeyinde verme, reddetme ve iptal etme izinleri
- Azure Active Directory tümleştirmesi

Desteklenen T-SQL:

- Tam [seçme](/sql/t-sql/queries/select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) yüzeyı alanı SQL işlevlerinin çoğunluğu dahil desteklenir
- CETAS-DıŞ TABLOYU SEÇ OLARAK OLUŞTUR
- Yalnızca görünümler ve güvenlikle ilgili DDL deyimleri

İsteğe bağlı SQL 'de yerel depolama alanı yoktur, veritabanlarında yalnızca meta veri nesneleri depolanır. Bu nedenle, aşağıdaki kavramlarla ilgili T-SQL desteklenmez:

- Tablolar
- Tetikleyiciler
- Gerçekleştirilmiş görünümler
- Görünümler ve güvenlikle ilgili olanlar dışındaki DDL deyimleri
- DML deyimleri

### <a name="extensions"></a>Uzantılar

Data Lake 'teki dosyalarda bulunan verilerin yerinde sorgulanmasında sorunsuz bir deneyim sağlamak için, SQL isteğe bağlı, aşağıdaki özellikleri ekleyerek var olan [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) işlevini genişletir:

[Birden çok dosyayı veya klasörü sorgulama](develop-storage-files-overview.md#query-multiple-files-or-folders)

[PARQUET dosya biçimi](develop-storage-files-overview.md#parquet-file-format)

[Sınırlandırılmış metinle çalışmak için ek seçenekler (alan Sonlandırıcı, satır Sonlandırıcı, kaçış karakteri)](develop-storage-files-overview.md#additional-options-for-working-with-delimited-text)

[Seçili sütunların bir alt kümesini okuyun](develop-storage-files-overview.md#read-a-chosen-subset-of-columns)

[Şema çıkarımı](develop-storage-files-overview.md#schema-inference)

[filename işlevi](develop-storage-files-overview.md#filename-function)

[FilePath işlevi](develop-storage-files-overview.md#filepath-function)

[Karmaşık türlerle ve iç içe veya yinelenen veri yapıları ile çalışma](develop-storage-files-overview.md#work-with-complex-types-and-nested-or-repeated-data-structures)

## <a name="security"></a>Güvenlik

Verilerinize güvenli bir şekilde erişmek için SQL isteğe bağlı, mekanizmalar sunmaktadır.

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory tümleştirmesi ve çok faktörlü kimlik doğrulaması

İsteğe bağlı SQL, veritabanı kullanıcısı ve diğer Microsoft hizmetlerinin kimliklerini [Azure Active Directory tümleştirmeyle](../../azure-sql/database/authentication-aad-configure.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)merkezi olarak yönetmenize olanak sağlar. Bu özellik, izin yönetimini kolaylaştırırken güvenliği artırır. Azure Active Directory (Azure AD), çoklu bir oturum açma işlemini desteklerken veri ve uygulama güvenliğini artırmak için [çok faktörlü kimlik doğrulamasını](../../azure-sql/database/authentication-mfa-ssms-configure.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) (MFA) destekler.

#### <a name="authentication"></a>Kimlik doğrulaması

İsteğe bağlı SQL kimlik doğrulaması, kullanıcıların uç noktaya bağlanırken kimliklerini nasıl kanıtlayacağına başvurur. İki tür kimlik doğrulaması desteklenir:

- **SQL kimlik doğrulaması**

  Bu kimlik doğrulama yöntemi bir Kullanıcı adı ve parola kullanır.

- **Azure Active Directory kimlik doğrulaması**:

  Bu kimlik doğrulama yöntemi, Azure Active Directory tarafından yönetilen kimlikleri kullanır. Azure AD kullanıcıları için Multi-Factor Authentication etkinleştirilebilir. [Mümkün olan her durumda](/sql/relational-databases/security/choose-an-authentication-mode?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)Active Directory kimlik doğrulaması (tümleşik güvenlik) kullanın.

#### <a name="authorization"></a>Yetkilendirme

Yetkilendirme, bir kullanıcının bir SQL isteğe bağlı veritabanı içinde neler yapabileceğini belirtir ve Kullanıcı hesabınızın veritabanı rolü üyelikleri ve nesne düzeyi izinleri tarafından kontrol edilir.

SQL kimlik doğrulaması kullanılırsa, SQL kullanıcısı yalnızca isteğe bağlı SQL 'de bulunur ve izinler SQL isteğe bağlı olarak kapsamlandırılır. Diğer hizmetlerde (Azure depolama gibi) güvenli kılınabilir nesnelere erişim, yalnızca SQL isteğe bağlı SQL kapsamında bulunduğundan SQL kullanıcısına doğrudan verilemez. SQL kullanıcısının dosyalara erişmek için [Desteklenen yetkilendirme türlerinden](develop-storage-files-storage-access-control.md#supported-storage-authorization-types) birini kullanması gerekir.

Azure AD kimlik doğrulaması kullanılırsa, bir Kullanıcı Azure depolama gibi isteğe bağlı SQL ve diğer hizmetlerde oturum açabilir ve Azure AD kullanıcısına izin verebilir.

### <a name="access-to-storage-accounts"></a>Depolama hesaplarına erişim

İsteğe bağlı SQL hizmetinde oturum açan bir Kullanıcı, Azure depolama 'daki dosyalara erişme ve bunları sorgulama yetkisine sahip olmalıdır. İsteğe bağlı SQL aşağıdaki yetkilendirme türlerini destekler:

- **Paylaşılan erişim imzası (SAS)** , depolama hesabındaki kaynaklara temsilci erişimi sağlar. SAS ile, istemcilere, hesap anahtarlarını paylaşmadan depolama hesabındaki kaynaklara erişim izni verebilirsiniz. SAS: geçerlilik aralığı, izin verilen izinler, kabul edilebilir IP adresi aralığı, kabul edilebilir protokol (https/http) olan istemcilere verdiğiniz erişim türü üzerinde ayrıntılı denetim elde etmenizi sağlar.

- **Kullanıcı kimliği** ("doğrudan geçiş" olarak da bilinir), Isteğe bağlı SQL 'de oturum açan Azure AD kullanıcısının kimliğinin verilere erişim yetkisi vermek için kullanıldığı bir yetkilendirme türüdür. Verilere erişmeden önce Azure depolama yöneticisinin verilere erişmek için Azure AD kullanıcısına izin vermesi gerekir. Bu yetkilendirme türü, isteğe bağlı SQL 'de oturum açan Azure AD kullanıcısını kullanır, bu nedenle SQL kullanıcı türleri için desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
Uç nokta bağlantısı ve dosya sorgulama hakkında ek bilgiler aşağıdaki makalelerde bulunabilir: 
- [Uç noktanıza bağlanma](connect-overview.md)
- [Dosyalarınızı sorgulama](develop-storage-files-overview.md)
