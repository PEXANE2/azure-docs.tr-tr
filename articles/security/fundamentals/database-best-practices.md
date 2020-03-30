---
title: Veritabanı güvenliği en iyi uygulamalar - Microsoft Azure
description: Bu makalede, Azure veritabanı güvenliği için en iyi uygulamalar kümesi sağlar.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/06/2019
ms.author: terrylan
ms.openlocfilehash: 0f2e0257c5bf855b0d9be61c43b68b4e30b3d80d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80125098"
---
# <a name="azure-database-security-best-practices"></a>Azure güvenliği için en iyi yöntemler
Bu makalede, veritabanı güvenliği için en iyi uygulamaları açıklanır.

En iyi uygulamalar fikir birliğine dayanır ve geçerli Azure platformu özellikleri ve özellik kümeleriyle çalışır. Görüşler ve teknolojiler zaman içinde değişir ve bu makale, bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

## <a name="secure-databases"></a>Güvenli veritabanları
Güvenlik veritabanlarını yönetmek için en önemli konudur ve [Azure SQL Veritabanı](../../sql-database/index.yml)için her zaman bir öncelik olmuştur. Veritabanlarınız, HIPAA, ISO 27001/27002 ve PCI DSS Düzey 1 dahil olmak üzere çoğu düzenleyici veya güvenlik gereksinimini karşılamaya yardımcı olmak için sıkı bir şekilde güvenli hale getirilebilir. [Microsoft Trust Center sitesinde](https://azure.microsoft.com/support/trust-center/services/)geçerli bir güvenlik uyumluluğu sertifikaları listesi mevcuttur. Ayrıca, veritabanlarınızı düzenleyici gereksinimlere dayalı olarak belirli Azure veri merkezlerine yerleştirmeyi de seçebilirsiniz.

## <a name="use-firewall-rules-to-restrict-database-access"></a>Veritabanı erişimini kısıtlamak için güvenlik duvarı kurallarını kullanma
Microsoft Azure SQL Veritabanı, Azure ve diğer internet tabanlı uygulamalar için ilişkisel veritabanı hizmeti sağlar. Erişim güvenliğini sağlamak için SQL Veritabanı erişimi şu şekilde denetler:

- IP adresine göre bağlantıyı sınırlayan güvenlik duvarı kuralları.
- Kullanıcıların kimliklerini kanıtlamalarını gerektiren kimlik doğrulama mekanizmaları.
- Kullanıcıları belirli eylemlerle ve verilerle sınırlayan yetkilendirme mekanizmaları.

Güvenlik duvarları, siz hangi bilgisayarların izni olduğunu belirtene kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.

Aşağıdaki şekilde SQL Veritabanı'nda bir sunucu güvenlik duvarı ayarladığınızı gösterir:

![Güvenlik duvarı kuralları](./media/database-best-practices/azure-database-security-best-practices-Fig1.png)

Azure SQL Veritabanı hizmeti yalnızca TCP bağlantı noktası 1433 üzerinden kullanılabilir. Bilgisayarınızdan bir SQL veritabanına erişmek için istemci bilgisayarınızın güvenlik duvarının TCP bağlantı noktası 1433'te giden TCP iletişimine izin verdiğinden emin olun. Diğer uygulamalar için bu bağlantılara ihtiyacınız yoksa, güvenlik duvarı kurallarını kullanarak TCP bağlantı noktası 1433'teki gelen bağlantıları engelleyin.

Bağlantı sürecinin bir parçası olarak, Azure sanal makinelerinden gelen bağlantılar, her çalışan rolü için benzersiz olan bir IP adresine ve bağlantı noktasına yönlendirilir. Bağlantı noktası numarası 11000 ile 11999 arasındadır. TCP bağlantı noktaları hakkında daha fazla bilgi için, [ADO.NET 4,5 için 1433'ün ötesindeki bağlantı noktalarına](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md)bakın.

SQL Veritabanındaki güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz. [SQL Veritabanı güvenlik duvarı kuralları](../../sql-database/sql-database-firewall-configure.md).

> [!Note]
> IP kurallarına ek olarak, güvenlik duvarı sanal ağ kurallarını yönetir. Sanal ağ kuralları, sanal ağ hizmeti uç noktalarına dayanır. Sanal ağ kuralları bazı durumlarda IP kurallarına tercih edilebilir. Daha fazla bilgi edinmek [için Azure SQL Veritabanı için Sanal ağ hizmeti bitiş noktalarına ve kurallarına](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md)bakın.

## <a name="enable-database-authentication"></a>Veritabanı kimlik doğrulamasını etkinleştirme
SQL Veritabanı iki tür kimlik doğrulamayı, SQL Server kimlik doğrulamasını ve Azure AD kimlik doğrulamasını destekler.

### <a name="sql-server-authentication"></a>*SQL Server kimlik doğrulaması*

Avantajları şunlardır:

- SQL Veritabanı'nın, tüm kullanıcıların bir Windows etki alanı tarafından kimlik doğrulaması olmadığı karma işletim sistemlerine sahip ortamları desteklemesine olanak tanır.
- SQL Veritabanı'nın, SQL Server kimlik doğrulaması gerektiren eski uygulamaları ve iş ortağı tarafından sağlanan uygulamaları desteklemesine izin verir.
- Kullanıcıların bilinmeyen veya güvenilmeyen etki alanlarından bağlanmasını sağlar. Bir örnek, kurulan müşterilerin siparişlerinin durumunu almak için atanan SQL Server girişlerine bağlandığı bir uygulamadır.
- SQL Veritabanı'nın, kullanıcıların kendi kimliklerini oluşturdukları web tabanlı uygulamaları desteklemesine izin verir.
- Yazılım geliştiricilerin, bilinen, önceden ayarlanmış SQL Server girişlerine dayalı karmaşık bir izin hiyerarşisi kullanarak uygulamalarını dağıtmalarına olanak tanır.

> [!NOTE]
> SQL Server kimlik doğrulaması Kerberos güvenlik protokolünü kullanamaz.

SQL Server kimlik doğrulamasını kullanıyorsanız, şunları

- Güçlü kimlik bilgilerini kendiniz yönetin.
- Bağlantı dizesindeki kimlik bilgilerini koruyun.
- (Potansiyel olarak) web sunucusundan veritabanına ağ üzerinden geçirilen kimlik bilgilerini korumak. Daha fazla bilgi için [bkz: 2.0](/previous-versions/msp-n-p/ff648340(v=pandp.10))ASP.NET SQL Kimlik Doğrulamasını Kullanarak SQL Server'a Bağlanma .

### <a name="azure-active-directory-ad-authentication"></a>*Azure Etkin Dizin (AD) kimlik doğrulaması*
Azure AD kimlik doğrulaması, Azure AD'deki kimlikleri kullanarak Azure SQL Veritabanı'na ve [SQL Veri Ambarı'na](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) bağlanma mekanizmasıdır. Azure AD kimlik doğrulaması ile veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin kimliklerini tek bir merkezi konumda yönetebilirsiniz. Merkezi kimlik yönetimi, tüm veritabanı kullanıcılarını aynı yerden yönetmenizi sağlar ve izin yönetimini kolaylaştırır.

> [!NOTE]
> SQL Server kimlik doğrulaması kullanımı yerine Azure AD kimlik doğrulamasını kullanmanızı öneririz.

Avantajları şunlardır:

- SQL Server kimlik doğrulamasına alternatif sağlar.
- Veritabanı sunucuları arasında kullanıcı kimliklerinin çoğalmasını durdurmaya yardımcı olur.
- Parola döndürmeyi tek bir yerde sağlar.
- Müşteriler dış (Azure AD) gruplarını kullanarak veritabanı izinlerini yönetebilir.
- Azure Active Directory tarafından desteklenen tümleşik Windows kimlik doğrulaması ve diğer kimlik doğrulama biçimlerini etkinleştirerek parola depolamayı ortadan kaldırabilir.
- Veritabanı düzeyinde kimlikleri doğrulamak için içerdiği veritabanı kullanıcılarını kullanır.
- SQL Veritabanı'na bağlanan uygulamalar için belirteç tabanlı kimlik doğrulamasını destekler.
- Etki alanı eşitlemesi olmadan yerel bir Azure Etkin Dizin örneği için AD FS (etki alanı federasyonu) veya yerel kullanıcı/parola kimlik doğrulamasını destekler.
- Azure AD, SQL Server Management Studio'da Çok Faktörlü Kimlik Doğrulama'yı içeren Active Directory Universal Authentication kullanan bağlantıları destekler. Çok Faktörlü Kimlik Doğrulama, telefon görüşmesi, kısa mesaj, PIN'li akıllı kartlar veya mobil uygulama bildirimi gibi çeşitli doğrulama seçenekleriyle güçlü bir kimlik doğrulaması sağlar. Daha fazla bilgi [için, SQL Veritabanı ve SQL Veri Ambarı ile Azure AD Çok Faktörlü Kimlik Doğrulama için SSMS desteğine](../../sql-database/sql-database-ssms-mfa-authentication.md)bakın.

Yapılandırma adımları, Azure AD kimlik doğrulamasını yapılandırmak ve kullanmak için aşağıdaki yordamları içerir:

- Azure AD oluşturun ve doldurun.
- İsteğe bağlı: Şu anda Azure aboneliğinizle ilişkili olan Etkin Dizin örneğini ilişkilendirin veya değiştirin.
- Azure SQL Veritabanı veya Azure SQL [Veri Ambarı](https://azure.microsoft.com/services/sql-data-warehouse/)için bir Azure Etkin Dizin yöneticisi oluşturun.
- İstemci bilgisayarlarınızı yapılandırın.
- Veritabanınızda Azure AD kimlikkimliklerine eşlenen veritabanında bulunan veritabanı kullanıcıları oluşturun.
- Azure AD kimliklerini kullanarak veritabanınıza bağlanın.

[SQL Veritabanı, Yönetilen Örnek veya SQL Veri Ambarı ile kimlik doğrulama için Azure Active Directory kimlik doğrulamasını kullan'da](../../sql-database/sql-database-aad-authentication.md)ayrıntılı bilgileri bulabilirsiniz.

## <a name="protect-your-data-by-using-encryption-and-row-level-security"></a>Şifreleme ve satır düzeyinde güvenlik kullanarak verilerinizi koruyun
[Azure SQL Veritabanı saydam veri](../../sql-database/transparent-data-encryption-azure-sql.md) şifrelemesi, diskteki verilerin korunmasına yardımcı olur ve donanıma yetkisiz erişime karşı korur. Bu özellik bütün bir veritabanı, yedekleri ve işlem günlüğü dosyaları için gerçek zamanlı şifreleme ve şifre çözme işlemlerini gerçekleştirir ve uygulamada değişiklik yapmayı gerektirmez. Saydam veri şifreleme, veritabanı şifreleme anahtarı adı verilen simetrik bir anahtar kullanarak tüm veritabanının depolanmasını şifreler.

Tüm depolama alanı şifrelendiğinde bile, veritabanının kendisini şifrelemek de önemlidir. Bu, veri koruma için derinlemesine savunma yaklaşımının bir uygulamasıdır. Azure SQL Veritabanı kullanıyorsanız ve hassas verileri (kredi kartı veya sosyal güvenlik numaraları gibi) korumak istiyorsanız, veritabanlarını FIPS 140-2 doğrulanmış 256 bit AES şifrelemesiyle şifreleyebilirsiniz. Bu şifreleme birçok endüstri standardının (örneğin, HIPAA ve PCI) gereksinimlerini karşılar.

Bir veritabanını saydam veri şifreleme skullanarak şifrelediğinizde [arabellek havuzu uzantısı (BPE)](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) ile ilgili dosyalar şifrelenmez. BPE ile ilgili dosyalar için [BitLocker](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732774(v=ws.11)) veya [Şifreleme Dosya Sistemi (EFS)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc749610(v%3dws.10)) gibi dosya sistemi düzeyinde şifreleme araçlarını kullanmanız gerekir.

Güvenlik yöneticisi veya veritabanı yöneticisi gibi yetkili bir kullanıcı, veritabanı saydam veri şifrelemesi ile şifrelenmiş olsa bile verilere erişebildiği için, aşağıdaki önerileri de izlemeniz gerekir:

- Veritabanı düzeyinde SQL Server kimlik doğrulamasını etkinleştirin.
- [RBAC rollerini](/azure/role-based-access-control/overview)kullanarak Azure AD kimlik doğrulamasını kullanın.
- Kullanıcıların ve uygulamaların kimlik doğrulaması için ayrı hesaplar kullandığından emin olun. Bu şekilde, kullanıcılara ve uygulamalara verilen izinleri sınırlayabilir ve kötü amaçlı etkinlik riskini azaltabilirsiniz.
- Sabit veritabanı rolleri (db_datareader veya db_datawriter gibi) kullanarak veritabanı düzeyinde güvenlik uygulayın. Veya seçtiğiniz veritabanı nesnelerine açık izinler vermek için uygulamanız için özel roller oluşturabilirsiniz.

Verilerinizi korumanın diğer yolları için şunları göz önünde bulundurun:

- [Hücre düzeyinde şifreleme](/sql/relational-databases/security/encryption/encrypt-a-column-of-data): Belirli sütunları hatta veri hücrelerini farklı şifreleme anahtarlarıyla şifrelemenizi sağlar.
- [Her zaman Şifrelenmiş,](/sql/relational-databases/security/encryption/always-encrypted-database-engine)istemciler istemci uygulamaları içinde hassas verileri şifrelemek ve Veritabanı Motoru (SQL Database veya SQL Server) için şifreleme anahtarlarını asla sağlar. Sonuç olarak, Her Zaman Şifrelenmiş, verilere sahip olanlar (ve görüntüleyebilir) ile verileri yöneten (ancak erişimi olmayan) arasında bir ayrım sağlar.
- [Satır Düzey Güvenlik](/sql/relational-databases/security/row-level-security), müşterilerin bir sorgu yürüten kullanıcının özelliklerine göre bir veritabanı tablosunda satırlara erişimi denetlemek sağlar. (Örnek özellikler grup üyeliği ve yürütme bağlamıdır.)

Veritabanı düzeyinde şifreleme kullanmayan kuruluşlar, SQL veritabanlarında bulunan verileri tehlikeye atan saldırılara karşı daha duyarlı olabilir.

[Azure SQL Veritabanı ile Saydam Veri Şifreleme](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx)makalesini okuyarak SQL Veritabanı saydam veri şifreleme hakkında daha fazla bilgi edinebilirsiniz.

## <a name="enable-database-auditing"></a>Veritabanı denetimini etkinleştirme
SQL Server Database Engine veya tek bir veritabanının bir örneğini denetlemek, olayları izlemeyi ve günlüğe kaydetmeyi içerir. SQL Server için, sunucu düzeyindeki olaylar için belirtimler ve veritabanı düzeyindeki olaylar için belirtimler içeren denetimler oluşturabilirsiniz. Denetlenen olaylar olay günlüklerine veya denetim dosyalarına yazılabilir.

Yüklemeniz için devlet veya standart gereksinimlerine bağlı olarak, SQL Server için çeşitli denetim düzeyleri vardır. SQL Server denetimi, çeşitli sunucu ve veritabanı nesnelerinde denetimleri etkinleştirmek, depolamak ve görüntülemek için araçlar ve işlemler sağlar.

[Azure SQL Veritabanı denetimi](/azure/sql-database/sql-database-auditing) veritabanı olaylarını izler ve Bunları Azure depolama hesabınızdaki bir denetim günlüğüne yazar.

Denetim, mevzuata uygunluğu korumanıza, veritabanı etkinliğini anlamanıza ve iş kaygılarına veya güvenlik ihlallerine işaret edebilecek tutarsızlıklar ve anormallikler bulmanıza yardımcı olabilir. Denetim, uyumluluk standartlarına bağlılığı kolaylaştırır, ancak uyumluluğu garanti etmez.

Veritabanı denetimi ve nasıl etkinleştirilen hakkında daha fazla bilgi edinmek için [bkz.](/azure/sql-database/sql-database-auditing)

## <a name="enable-database-threat-detection"></a>Veritabanı tehdit algılamayı etkinleştirme
Tehdit koruması tespit edilmenin ötesine geçer. Veritabanı tehdit koruması şunları içerir:

- Verilerinizi koruyabilmek için en hassas verilerinizi keşfetmek ve sınıflandırmak.
- Veritabanınızı koruyabilmeniz için veritabanınızda güvenli yapılandırmalar uygulama.
- Olası tehditleri tespit etmek ve bunlar ortaya çıkarken yanıt vermek, böylece hızlı bir şekilde yanıt verebilir ve düzeltebilirsiniz.

**En iyi yöntem**: Veritabanlarınızdaki hassas verileri keşfedin, sınıflandırın ve etiketleyin.   
**Ayrıntı**: Azure SQL Veritabanı'nda Veri [Bulma ve Sınıflandırmayı](/azure/sql-database/sql-database-data-discovery-and-classification) etkinleştirerek SQL veritabanınızdaki verileri sınıflandırın. Azure panosunda hassas verilerinize erişimi izleyebilir veya raporları indirebilirsiniz.

**En iyi yöntem**: Veritabanı güvenliğinizi proaktif olarak geliştirebilmeniz için veritabanı güvenlik açıklarını izleyin.   
**Ayrıntı**: Olası veritabanı güvenlik açıklarını tarayan Azure SQL Veritabanı [Güvenlik Açığı Değerlendirmesi](/azure/sql-database/sql-vulnerability-assessment) hizmetini kullanın. Hizmet, güvenlik açıklarını işaretleyen ve yanlış yapılandırmalar, aşırı izinler ve korunmasız hassas veriler gibi en iyi uygulamalardan sapmalar gösteren bir bilgi tabanı kullanır.

Kurallar, Microsoft'un en iyi uygulamalarına dayanır ve veritabanınız ve değerli verileri için en büyük riskleri sunan güvenlik sorunlarına odaklanır. Hem veritabanı düzeyindeki sorunları hem de sunucu güvenlik duvarı ayarları ve sunucu düzeyinde izinler gibi sunucu düzeyindegüvenlik sorunlarını kapsar. Bu kurallar aynı zamanda düzenleyici kurumların uyumluluk standartlarını karşılama gerekliliklerinin çoğunu temsil eder.

**En iyi uygulama**: Tehdit algılamayı etkinleştirin.  
**Ayrıntı**: Tehditlerin nasıl araştırılabildiğini ve azaltılabildiğini zedelemek için güvenlik uyarıları ve öneriler almak için Azure SQL Veritabanı [Tehdit Algılamasını](/azure/sql-database/sql-database-threat-detection) etkinleştirin. Şüpheli veritabanı etkinlikleri, olası güvenlik açıkları ve SQL enjeksiyon saldırılarının yanı sıra anormal veritabanı erişimi ve sorgu desenleri hakkında uyarılar alırsınız.

[Gelişmiş Tehdit Koruması](/azure/sql-database/sql-advanced-threat-protection) gelişmiş SQL güvenlik yetenekleri için birleşik bir pakettir. Daha önce bahsedilen hizmetleri içerir: Veri Bulma ve Sınıflandırma, Güvenlik Açığı Değerlendirmesi ve Tehdit Algılama. Bu yetenekleri etkinleştirmek ve yönetmek için tek bir konum sağlar.

Bu özellikleri etkinleştirmek şunları yardımcı olur:

- Veri gizliliği standartlarını ve mevzuata uygunluk gerekliliklerini karşılayın.
- Veritabanlarınıza erişimi kontrol edin ve güvenliklerini sertleştirir.
- Değişikliklerin izlenmesinin zor olduğu dinamik bir veritabanı ortamını izleyin.
- Olası tehditleri algılayın ve yanıtlayın.

Buna ek olarak, Tehdit Algılama, tüm Azure kaynaklarınızın güvenlik durumunun merkezi bir görünümü için uyarıları Azure Güvenlik Merkezi ile tümleştirir.

## <a name="next-steps"></a>Sonraki adımlar
Azure'u kullanarak bulut çözümlerinizi tasarlarken, dağıtırken ve yönetirken kullanabileceğiniz daha fazla güvenlik en iyi uygulamaları için Azure güvenlik en iyi uygulamaları ve [desenleri](best-practices-and-patterns.md) görün.

Azure güvenliği ve ilgili Microsoft hizmetleri hakkında daha genel bilgi sağlamak için aşağıdaki kaynaklar kullanılabilir:
* [Azure Güvenlik Ekibi Blogu](https://blogs.msdn.microsoft.com/azuresecurity/) - Azure Güvenlik'teki en son bilgiler için
* [Microsoft Güvenlik Yanıt Merkezi](https://technet.microsoft.com/library/dn440717.aspx) - Azure ile ilgili sorunlar da dahil olmak üzere Microsoft güvenlik açıklarının rapor edilebildiği veya e-posta yoluylasecure@microsoft.com
