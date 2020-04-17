---
title: Güvenlik Genel Bakış
description: Bulut ve SQL Server arasındaki farklar da dahil olmak üzere Azure SQL Veritabanı ve SQL Server güvenliği hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
ms.reviewer: vanto, carlrab, emlisa
ms.date: 05/14/2019
ms.openlocfilehash: 1d08770d81ade2d976142b2ce1fd94dae044cf32
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81461404"
---
# <a name="an-overview-of-azure-sql-database-security-capabilities"></a>Azure SQL Veritabanı güvenlik özelliklerine genel bakış

Bu makalede, Azure SQL Veritabanı kullanarak bir uygulamanın veri katmanını güvence altına almanın temelleri özetlanmaktadır. Açıklanan güvenlik stratejisi aşağıdaki resimde gösterildiği gibi katmanlı savunma-derinlemesine yaklaşım izler ve dışarıdan hareket eder:

![sql-security-layer.png](media/sql-database-security-overview/sql-security-layer.png)

## <a name="network-security"></a>Ağ güvenliği

Microsoft Azure SQL Veritabanı, bulut ve kurumsal uygulamalar için ilişkisel bir veritabanı hizmeti sağlar. Müşteri verilerinin korunmasına yardımcı olmak için, güvenlik duvarları IP adresine veya Azure Sanal ağ trafiği kaynağına bağlı olarak erişim açıkça verilene kadar veritabanı sunucusuna ağ erişimini engeller.

### <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları

IP güvenlik duvarı kuralları, her isteğin menşeli IP adresine dayalı olarak veritabanlarına erişim sağlar. Daha fazla bilgi için Azure [SQL Veritabanına Genel Bakış ve SQL Veri Ambarı güvenlik duvarı kurallarına](sql-database-firewall-configure.md)bakın.

### <a name="virtual-network-firewall-rules"></a>Sanal ağ güvenlik duvarı kuralları

[Sanal ağ hizmeti bitiş noktaları](../virtual-network/virtual-network-service-endpoints-overview.md) sanal ağ bağlantınızı Azure omurgası üzerinden genişletir ve Azure SQL Veritabanı'nın trafiğin kaynağı olduğu sanal ağ alt netini tanımlamasını sağlar. Trafiğin Azure SQL Veritabanı'na erişmesine izin vermek için, Ağ Güvenlik Grupları üzerinden giden trafiğine izin vermek için SQL [hizmet etiketlerini](../virtual-network/security-overview.md) kullanın.

[Sanal ağ kuralları,](sql-database-vnet-service-endpoint-rule-overview.md) Azure SQL Veritabanı'nın yalnızca sanal ağ içindeki seçili alt ağlardan gönderilen iletişimleri kabul etmesini sağlar.

> [!NOTE]
> Güvenlik duvarı kurallarıyla erişimi denetleme **yönetilen bir örnek**için geçerli *değildir.* Gerekli ağ yapılandırması hakkında daha fazla bilgi için [yönetilen bir örne bağlanma](sql-database-managed-instance-connect-app.md)

## <a name="access-management"></a>Erişim yönetimi

> [!IMPORTANT]
> Azure'daki veritabanlarını ve veritabanı sunucularını yönetmek, portal kullanıcı hesabınızın rol atamaları tarafından denetlenir. Bu makale hakkında daha fazla bilgi için [Azure portalında Rol tabanlı erişim denetimine](../role-based-access-control/overview.md)bakın.

### <a name="authentication"></a>Kimlik Doğrulaması

Kimlik doğrulama, kullanıcının iddia ettikleri kişi olduğunu kanıtlama işlemidir. Azure SQL Veritabanı iki tür kimlik doğrulamayı destekler:

- **SQL kimlik doğrulama:**

    SQL veritabanı kimlik doğrulaması, kullanıcı adı ve parola kullanarak [Azure SQL Veritabanı'na](sql-database-technical-overview.md) bağlanırken kullanıcının kimlik doğrulaması anlamına gelir. Veritabanı için veritabanı sunucusu oluşturma sırasında, bir kullanıcı adı ve parola ile bir "Server admin" giriş belirtilmelidir. Bu kimlik bilgilerini kullanarak, bir "sunucu yöneticisi" veritabanı sahibi olarak bu veritabanı sunucusunda herhangi bir veritabanına kimlik doğrulaması yapabilir. Bundan sonra, ek SQL girişleri ve kullanıcılar, kullanıcıların kullanıcı adı ve parola kullanarak bağlanmalarını sağlayan sunucu yöneticisi tarafından oluşturulabilir.

- **Azure Active Directory kimlik doğrulaması:**

    Azure Active Directory kimlik doğrulaması, Azure Active Directory (Azure AD) kimliklerini kullanarak [Azure SQL Veritabanı'na](sql-database-technical-overview.md) ve [SQL Veri Ambarı'na](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) bağlanma mekanizmasıdır. Azure AD kimlik doğrulaması, yöneticilerin veritabanı kullanıcılarının kimliklerini ve izinlerini diğer Microsoft hizmetleriyle birlikte tek bir merkezi konumda merkezi olarak yönetmesine olanak tanır. Bu, parola depolamanın en aza inmesini içerir ve merkezi parola döndürme ilkelerini etkinleştirin.

     SQL Veritabanı ile Azure AD kimlik doğrulamasını kullanmak için **Active Directory yöneticisi** adlı bir sunucu yöneticisi oluşturulmalıdır. Daha fazla bilgi için bkz: [Azure Active Directory Authentication'ı kullanarak SQL Veritabanına Bağlanma.](sql-database-aad-authentication.md) Azure AD kimlik doğrulaması hem yönetilen hem de federe hesapları destekler. Federe hesaplar, Azure AD ile beslenen bir müşteri etki alanı için Windows kullanıcılarını ve gruplarını destekler.

    Kullanılabilir ek Azure AD kimlik doğrulama seçenekleri, [Çoklu Faktörlü Kimlik Doğrulama](../active-directory/authentication/concept-mfa-howitworks.md) ve [Koşullu Erişim](sql-database-conditional-access.md)dahil olmak üzere SQL Server Management Studio bağlantıları için Active [Directory Universal Authentication'dır.](sql-database-ssms-mfa-authentication.md)

> [!IMPORTANT]
> Azure'daki veritabanlarını ve sunucuları yönetmek, portal kullanıcı hesabınızın rol atamaları tarafından denetlenir. Bu makale hakkında daha fazla bilgi için [Azure portalında Rol tabanlı erişim denetimine](../role-based-access-control/overview.md)bakın. Güvenlik duvarı kurallarıyla erişimi denetleme **yönetilen bir örnek**için geçerli *değildir.* Gereken ağ yapılandırması hakkında daha fazla bilgi için [yönetilen bir örne bağlanma](sql-database-managed-instance-connect-app.md) yla ilgili aşağıdaki makaleye bakın.

## <a name="authorization"></a>Yetkilendirme

Yetkilendirme, Azure SQL Veritabanı'nda bir kullanıcıya atanan izinleri ifade eder ve kullanıcının ne yapmasına izin verilip verilmediğini belirler. İzinler, kullanıcı hesaplarını [veritabanı rollerine](/sql/relational-databases/security/authentication-access/database-level-roles) ekleyerek ve bu rollere veritabanı düzeyinde izinler atayarak veya kullanıcıya belirli [nesne düzeyinde izinler](/sql/relational-databases/security/permissions-database-engine)vererek denetlenir. Daha fazla bilgi için [Bkz. Girişler ve Kullanıcılar](sql-database-manage-logins.md)

En iyi uygulama olarak, gerektiğinde özel roller oluşturun. İş işlevlerini yapmak için gereken en az ayrıcalıkla kullanıcıları role ekleyin. İzinleri doğrudan kullanıcılara atamayın. Sunucu yöneticisi hesabı, kapsamlı izinlere sahip olan yerleşik db_owner rolün bir üyesidir ve yalnızca yönetim görevleri olan birkaç kullanıcıya verilmelidir. Azure SQL Veritabanı uygulamaları [için,](/sql/t-sql/statements/execute-as-clause-transact-sql) çağrılan modülün yürütme bağlamını belirtmek için EXECUTE AS'yi kullanın veya sınırlı izinlerle [Uygulama Rolleri'ni](/sql/relational-databases/security/authentication-access/application-roles) kullanın. Bu uygulama, veritabanına bağlanan uygulamanın uygulama tarafından gereken en az ayrıcalıklara sahip olmasını sağlar. Bu en iyi uygulamaları takiben de görevlerin ayrılmasını teşvik eder.

### <a name="row-level-security"></a>Satır düzeyi güvenlik

Satır Düzey Güvenliği, müşterilerin bir sorguyu yürüten kullanıcının özelliklerine (örneğin, grup üyeliği veya yürütme bağlamı) dayalı olarak veritabanı tablosundaki satırlara erişimi denetlemesini sağlar. Satır Düzey Güvenlik, özel Etiket tabanlı güvenlik kavramlarını uygulamak için de kullanılabilir. Daha fazla bilgi için bkz. [Satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security).

![azure-veritabanı-rls.png](media/sql-database-security-overview/azure-database-rls.png)

## <a name="threat-protection"></a>Tehdit koruması

SQL Veritabanı, denetim ve tehdit algılama özellikleri sağlayarak müşteri verilerini güvence altına alır.

### <a name="sql-auditing-in-azure-monitor-logs-and-event-hubs"></a>Azure Monitor günlüklerinde ve Etkinlik Hub'larında SQL denetimi

SQL Veritabanı denetimi veritabanı etkinliklerini izler ve veritabanı olaylarını müşteriye ait bir Azure depolama hesabındaki bir denetim günlüğüne kaydederek güvenlik standartlarıyla uyumluluğu korumaya yardımcı olur. Denetim, kullanıcıların devam eden veritabanı etkinliklerini izlemelerine ve olası tehditleri veya şüpheli kötüye kullanım ve güvenlik ihlallerini belirlemek için geçmiş faaliyetleri analiz etmelerine ve araştırmalarına olanak tanır. Daha fazla bilgi için [bkz.](sql-database-auditing.md)  

### <a name="advanced-threat-protection"></a>Gelişmiş Tehdit Koruması

Gelişmiş Tehdit Koruması, olağandışı davranışları ve veritabanlarına erişmek veya veritabanlarından yararlanmak için zararlı olabilecek girişimleri algılamak için SQL Server günlüklerinizi çözümlüyor. Uyarılar, SQL enjeksiyonu, olası veri sızma ve kaba kuvvet saldırıları gibi şüpheli etkinlikler veya ayrıcalık artışlarını ve ihlal edilmiş kimlik bilgilerini yakalamak için erişim desenlerinde anormallikler için oluşturulur. Uyarılar, şüpheli etkinliklerin ayrıntılarının sağlandığı ve tehdidi azaltmak için eylemlerle birlikte daha fazla araştırma için önerilerin sunulduğu [Azure Güvenlik Merkezi'nden](https://azure.microsoft.com/services/security-center/)görüntülenir. Gelişmiş Tehdit Koruması ek bir ücret karşılığında sunucu başına etkinleştirilebilir. Daha fazla bilgi için sql [veritabanı gelişmiş tehdit koruması ile başlayın](sql-database-threat-detection.md)bakın.

![azure-veritabanı-td.jpg](media/sql-database-security-overview/azure-database-td.jpg)

## <a name="information-protection-and-encryption"></a>Bilgi koruma ve şifreleme

### <a name="transport-layer-security-tls-encryption-in-transit"></a>Aktarım Katmanı Güvenliği TLS (Şifreleme-in-aktarım)

SQL [Veritabanı, Transport Layer Security](https://support.microsoft.com/help/3135244/tls-1-2-support-for-microsoft-sql-server)ile hareket halindeki verileri şifreleyerek müşteri verilerini güvence altına alar.

Sql Server tüm bağlantılar için her zaman şifreleme (SSL/TLS) uygular. Bu, bağlantı dizesinde **Şifreveya** **TrustServerCertificate** ayarına bakılmaksızın istemci ve sunucu arasında tüm verilerin "aktarım halinde" şifrelenmesini sağlar.

En iyi yöntem olarak, uygulamanızın bağlantı dizesinde şifreli bir bağlantı belirtmenizi ve sunucu sertifikasına _**güvenmemenizi**_ öneririz. Bu, uygulamanızı sunucu sertifikasını doğrulamaya zorlar ve böylece uygulamanızın orta tür saldırılarda adama karşı savunmasız olmasını önler.

Örneğin ADO.NET sürücüsünü kullanırken bu işlem **Encrypt=True** ve **TrustServerCertificate=False**ile gerçekleştirilir. Bağlantı dizenizi Azure portalından alırsanız, doğru ayarlara sahip olur.

> [!IMPORTANT]
> Microsoft'a ait olmayan bazı sürücülerin varsayılan olarak TLS'yi kullanamayabileceğini veya çalışması için TLS'nin (<1,2) eski bir sürümüne güvenebileceğini unutmayın. Bu durumda SQL Server yine de veritabanınıza bağlanmanızı sağlar. Ancak, özellikle hassas verileri depolıyorsanız, bu tür sürücülerin ve uygulamaların SQL Veritabanı'na bağlanmasına izin vermenin güvenlik risklerini değerlendirmenizi öneririz.
>
> TLS ve bağlantı hakkında daha fazla bilgi için [TLS'nin göz önünde bulundurulması gerekenler](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity)

### <a name="transparent-data-encryption-encryption-at-rest"></a>Saydam Veri Şifreleme (Şifreleme-at-rest)

[Azure SQL Veritabanı için Saydam Veri Şifreleme (TDE),](transparent-data-encryption-azure-sql.md) verileri ham dosyalara veya yedeklemelere yetkisiz veya çevrimdışı erişimden korumaya yardımcı olmak için bir güvenlik katmanı ekler. Yaygın senaryolar arasında veri merkezi hırsızlığı veya disk sürücüleri ve yedekleme teypleri gibi donanım veya ortamın güvenli olmayan şekilde atılması yer almaktadır.TDE, uygulama geliştiricilerin varolan uygulamalarda herhangi bir değişiklik yapmalarını gerektirmeyen bir AES şifreleme algoritması kullanarak tüm veritabanını şifreler.

Azure'da, yeni oluşturulan tüm SQL veritabanları varsayılan olarak şifrelenir ve veritabanı şifreleme anahtarı yerleşik bir sunucu sertifikası tarafından korunur.  Sertifika bakım ve döndürme hizmeti tarafından yönetilir ve kullanıcıdan hiçbir giriş gerektirmez. Şifreleme anahtarlarının denetimini ele almayı tercih eden müşteriler, Tuşları [Azure Anahtar Kasası'nda](../key-vault/general/secure-your-key-vault.md)yönetebilir.

### <a name="key-management-with-azure-key-vault"></a>Azure Key Vault ile anahtar yönetimi

 [Şeffaf Veri Şifreleme](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE) için Kendi [Anahtarınızı Getir](transparent-data-encryption-byok-azure-sql.md) (BYOK) desteği, müşterilerin [Azure'un](../key-vault/general/secure-your-key-vault.md)bulut tabanlı dış anahtar yönetim sistemi olan Azure Key Vault'u kullanarak anahtar yönetimi ve döndürme ye sahip olmasını sağlar. Veritabanının anahtar kasasına erişimi iptal edilirse, veritabanının şifresi çözülemez ve belleğe okunamaz. Azure Key Vault merkezi bir anahtar yönetim platformu sağlar, sıkı bir şekilde izlenen donanım güvenlik modüllerinden (HSM) yararlanır ve güvenlik uyumluluk gereksinimlerini karşılamaya yardımcı olmak için anahtarların yönetimi ile verilerin yönetimi arasında görev ayrılmasını sağlar.

### <a name="always-encrypted-encryption-in-use"></a>Her Zaman Şifrelenmiş (Kullanımda Şifreleme)

![azure-database-ae.png](media/sql-database-security-overview/azure-database-ae.png)

[Her Zaman Şifrelenmiş,](/sql/relational-databases/security/encryption/always-encrypted-database-engine) belirli veritabanı sütunlarında depolanan hassas verileri erişimden (örneğin, kredi kartı numaraları, ulusal kimlik numaraları veya _bilinmesi gereken_ veriler) korumak için tasarlanmış bir özelliktir. Buna, yönetim görevlerini gerçekleştirmek için veritabanına erişmeye yetkili veritabanı yöneticileri veya diğer ayrıcalıklı kullanıcılar dahildir, ancak şifrelenmiş sütunlarda belirli verilere erişmeye gerek yoktur. Veriler her zaman şifrelenir, bu da şifrelenmiş verilerin yalnızca şifreleme anahtarına erişimi olan istemci uygulamaları tarafından işlenmesi için şifresinin çözüldüği anlamına gelir.  Şifreleme anahtarı hiçbir zaman SQL'e maruz kalmaz ve [Windows Certificate Store'da](sql-database-always-encrypted.md) veya [Azure Key Vault'ta](sql-database-always-encrypted-azure-key-vault.md)depolanabilir.

### <a name="dynamic-data-masking"></a>Dinamik veri maskeleme

![azure-veritabanı-ddm.png](media/sql-database-security-overview/azure-database-ddm.png)

SQL Veritabanı dinamik veri maskeleme, hassas verilerin görünürlüğünü ayrıcalık sahibi olmayan kullanıcılardan gizleyerek sınırlar. Dinamik veri maskeleme, Azure SQL Veritabanı'nda hassas olabilecek verileri otomatik olarak keşfeder ve uygulama katmanı üzerinde en az etkiyle bu alanları maskelemek için işlem uygulanabilir öneriler sağlar. Bu özellik, hassas verileri belirlenen veritabanı alanlarına yapılan sorgunun sonuç kümesinde karartır ancak veritabanındaki veriler değişmez. Daha fazla bilgi için bkz: [SQL Veritabanı dinamik veri maskeleme ile başlayın.](sql-database-dynamic-data-masking-get-started.md)

## <a name="security-management"></a>Güvenlik yönetimi

### <a name="vulnerability-assessment"></a>Güvenlik açığı değerlendirmesi

[Güvenlik açığı değerlendirmesi,](sql-vulnerability-assessment.md) genel veritabanı güvenliğini proaktif olarak iyileştirmek amacıyla olası veritabanı açıklarını keşfedebilen, izleyebilen ve düzeltmeye yardımcı olabilecek kolay bir hizmeti yapılandırmaktır. Güvenlik açığı değerlendirmesi (VA), gelişmiş SQL güvenlik yetenekleri için birleşik bir paket olan gelişmiş veri güvenliği (ADS) teklifinin bir parçasıdır. Güvenlik açığı değerlendirmesine merkezi SQL ADS portalı üzerinden erişilebilir ve yönetilebilir.

### <a name="data-discovery--classification"></a>Veri bulma ve sınıflandırma

Veri bulma & sınıflandırması (şu anda önizlemede), veritabanlarınızdaki hassas verileri keşfetmek, sınıflandırmak, etiketlemek ve korumak için Azure SQL Veritabanı'nda yerleşik gelişmiş özellikler sağlar. Son derece hassas verilerinizi (iş/finans, sağlık, kişisel veriler, vb.) keşfetmek ve sınıflandırmak, organizasyonel Bilgi koruma statunuzda önemli bir rol oynayabilir. Şunlara altyapı sağlayabilir:

- Hassas verilere anormal erişim konusunda izleme (denetleme) ve uyarı gibi çeşitli güvenlik senaryoları.
- Son derece hassas veriler içeren veritabanlarına erişimi denetleme ve güvenliği sertleştirme.
- Veri gizliliği standartlarına uymaya ve mevzuat uyumluluğu gereksinimlerini karşılamaya yardımcı olma.

Daha fazla bilgi için bkz. [veri bulma & sınıflandırması ile başlayın.](sql-database-data-discovery-and-classification.md)

### <a name="compliance"></a>Uyumluluk

Azure SQL Veritabanı, uygulamanızın çeşitli güvenlik gereksinimlerini karşılamasına yardımcı olabilecek yukarıdaki özellikler ve işlevlere ek olarak, düzenli denetimlere de katılır ve bir dizi uyumluluk standardına göre sertifikalandırılır. Daha fazla bilgi için, SQL Veritabanı uyumluluk sertifikalarının en güncel listesini bulabileceğiniz [Microsoft Azure Güven Merkezi'ne](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) bakın.

## <a name="next-steps"></a>Sonraki adımlar

- SQL Veritabanı'nda oturum açma, kullanıcı hesapları, veritabanı rolleri ve izinlerin kullanımı yla ilgili bir tartışma [için](sql-database-manage-logins.md)bkz.
- Veritabanı denetimi tartışması için [SQL Veritabanı denetimine](sql-database-auditing.md)bakın.
- Tehdit algılama tartışması için [SQL Veritabanı tehdit algılama bölümüne](sql-database-threat-detection.md)bakın.
