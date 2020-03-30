---
title: Azure veritabanı güvenliğine genel bakış| Microsoft Dokümanlar
description: Bu makalede, Azure veritabanı güvenlik özelliklerine genel bir bakış sağlar.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: TomSh
ms.openlocfilehash: e5ed60ea59dc8cf19b8f9ca7e96777dbc6980171
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69906051"
---
# <a name="azure-database-security-overview"></a>Azure veritabanı güvenliğine genel bakış

Güvenlik, veritabanlarını yönetmek için en önemli sorundur ve Azure SQL Veritabanı için her zaman bir öncelik olmuştur. Azure SQL Veritabanı, güvenlik duvarı kuralları ve bağlantı şifrelemesi ile bağlantı güvenliğini destekler. Kullanıcı adı ve parola ile kimlik doğrulamayı ve Azure Active Directory tarafından yönetilen kimlikleri kullanan Azure Active Directory (Azure AD) kimlik doğrulamasını destekler. Yetkilendirme, rol tabanlı erişim denetimini kullanır.

Azure SQL Veritabanı, uygulamada değişiklik gerektirmeden veritabanlarının, ilişkili yedeklemelerin ve işlem günlüğü dosyalarının gerçek zamanlı şifrelemesini ve şifresini çözerek şifrelemeyi destekler.

Microsoft kurumsal verileri şifrelemek için ek yollar sağlar:

-   Hücre düzeyinde şifreleme, belirli sütunları ve hatta veri hücrelerini farklı şifreleme anahtarlarıyla şifrelemek için kullanılabilir.
-   Bir donanım güvenlik modülüne veya şifreleme anahtarı hiyerarşinizin merkezi yönetimine ihtiyacınız varsa, Azure sanal makinede (VM) SQL Server ile Azure Key Vault'u kullanmayı düşünün.
-   Her zaman Şifrelenmiş (şu anda önizlemede) şifrelemeyi uygulamalar için saydam hale getirir. Ayrıca, istemcilerin şifreleme anahtarlarını SQL Veritabanı ile paylaşmadan istemci uygulamaları içindeki hassas verileri şifrelemelerine olanak tanır.

Azure SQL Veritabanı Denetimi, işletmelerin olayları Azure Depolama'daki bir denetim günlüğüne kaydetmesini sağlar. SQL Veritabanı Denetimi ayrıca Microsoft Power BI ile tümleştirilerek ayrıntılı raporlar ve analizler oluşturulmasını sağlar.

Azure SQL veritabanları, HIPAA, ISO 27001/27002 ve PCI DSS Düzey 1 dahil olmak üzere çoğu düzenleyici veya güvenlik gereksinimini karşılamak için sıkı bir şekilde güvenli hale getirilebilir. [Microsoft Azure Güven Merkezi sitesinde](https://azure.microsoft.com/support/trust-center/services/)geçerli bir güvenlik uyumluluğu sertifikası listesi mevcuttur.

Bu makalede, yapılandırılmış, tabular ve ilişkisel veriler için Microsoft Azure SQL veritabanlarını güvence altına alma temelleri ele allanmıştır. Bu makalede özellikle veri koruma, erişim denetimi ve öngörülebilir izleme kaynakları için başlangıç bilgileri sunulmaktadır.

## <a name="protection-of-data"></a>Verilerin korunması

SQL Veritabanı şifreleme sağlayarak verilerinizin güvenliğini sağlamaya yardımcı olur:

- [Taşıma Katmanı Güvenliği (TLS)](https://support.microsoft.com/kb/3135244)aracılığıyla hareket halindeki veriler için.
- [Saydam veri şifreleme](https://go.microsoft.com/fwlink/?LinkId=526242)yoluyla veri istirahatiçin.
- [Her Zaman Şifrelenmiş](https://msdn.microsoft.com/library/mt163865.aspx)üzerinden kullanılan veriler için.

Verilerinizi şifrelemek için kullanabileceğiniz diğer yöntemler şunlardır:

-   [Hücre düzeyinde şifreleme](https://msdn.microsoft.com/library/ms179331.aspx): Belirli sütunları hatta veri hücrelerini farklı şifreleme anahtarlarıyla şifrelemenizi sağlar.
-   [Bir Azure VM'de SQL Server ile Azure Key Vault](https://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx), bir donanım güvenlik modülüne veya şifreleme anahtarı hiyerarşinizin merkezi yönetimine ihtiyacınız varsa.

### <a name="encryption-in-motion"></a>Hareket halindeşifreleme

Tüm istemci/sunucu uygulamaları için yaygın bir sorun, veriler genel ve özel ağlar üzerinde hareket ettikçe gizlilik gereksinimidir. Ağ üzerinde hareket eden veriler şifrelenmezse, yetkisiz kullanıcılar tarafından yakalanıp çalınma olasılığı vardır. Veritabanı hizmetleriyle uğraşırken, verilerin veritabanı istemcisi ve sunucu arasında şifrelendiğinden emin olun. Ayrıca, verilerin birbiriyle iletişim kurandaki veritabanı sunucuları ve orta katman uygulamaları arasında şifrelenmiş olduğundan emin olun.

Bir ağı yönetirken sorun, güvenilmeyen bir ağ üzerinden uygulamalar arasında gönderilen verileri güvence altına almaktır. Sunucuların ve istemcilerin kimliğini doğrulamak için [TLS/SSL'yi](/windows-server/security/tls/transport-layer-security-protocol) kullanabilir ve ardından kimlik doğrulaması yapılan taraflar arasındaki iletileri şifrelemek için kullanabilirsiniz.

Kimlik doğrulama işleminde, bir TLS/SSL istemcisi TLS/SSL sunucusuna ileti gönderir. Sunucu, sunucunun kendisini doğrulamak için ihtiyaç duyduğu bilgilerle yanıt verir. İstemci ve sunucu, oturum anahtarlarının ek değişimini gerçekleştirir ve kimlik doğrulama iletişimi sona erer. Kimlik doğrulama tamamlandığında, kimlik doğrulama işlemi sırasında oluşturulan simetrik şifreleme anahtarları aracılığıyla sunucu ve istemci arasında SSL tarafından güvenli iletişim başlayabilir.

Azure SQL Veritabanı'na yapılan tüm bağlantılar, veriler veritabanına ve veritabanından "aktarımhalinde" yken her zaman şifreleme (TLS/SSL) gerektirir. SQL Veritabanı, sunucuların ve istemcilerin kimliğini doğrulamak ve kimlik doğrulaması yapılan taraflar arasındaki iletileri şifrelemek için TLS/SSL kullanır. 

Uygulamanızın bağlantı dizesinde, bağlantıyı şifrelemek ve sunucu sertifikasına güvenmemek için parametreler belirtmeniz gerekir. (Bağlantı dizenizi Azure portalından kopyalarsanız bu sizin için yapılır.) Aksi takdirde, bağlantı sunucunun kimliğini doğrulamaz ve "ortadaki adam" saldırılarına karşı duyarlı olacaktır. Örneğin, ADO.NET sürücü için bu bağlantı `Encrypt=True` dize parametreleri ve `TrustServerCertificate=False`.

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Veritabanının güvenliğini sağlamak için çeşitli önlemler alabilirsiniz. Örneğin, güvenli bir sistem tasarlayın, gizli varlıkları şifreleyin ve veritabanı sunucuları etrafında bir güvenlik duvarı oluşturun. Ancak fiziksel ortamın (sürücüler veya yedekleme teypleri gibi) çalındığı bir senaryoda, kötü amaçlı bir taraf veritabanını geri yükleyebilir veya ekleyebilir ve verilere göz atabilir.

Bir çözüm veritabanında hassas verileri şifrelemek ve bir sertifika ile veri şifrelemek için kullanılan anahtarları korumaktır. Bu çözüm, anahtarları olmayan herkesin verileri kullanmasını engeller, ancak bu tür bir koruma planlanmalıdır.

Bu sorunu çözmek için SQL Server ve SQL Database [saydam veri şifrelemesini](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql?view=azuresqldb-current&viewFallbackFrom=sql-server-2017)destekler. Saydam veri şifreleme, SQL Server ve SQL Database veri dosyalarını şifreler, şifreleme verileri olarak bilinir.

Saydam veri şifreleme, kötü amaçlı etkinlik tehdidine karşı koruma sağlar. Bu özellik bütün bir veritabanı, yedekleri ve işlem günlüğü dosyaları için gerçek zamanlı şifreleme ve şifre çözme işlemlerini gerçekleştirir ve uygulamada değişiklik yapmayı gerektirmez.  

Saydam veri şifreleme, veritabanı şifreleme anahtarı adı verilen simetrik bir anahtar kullanarak tüm veritabanının depolanmasını şifreler. SQL Veritabanı'nda veritabanı şifreleme anahtarı yerleşik bir sunucu sertifikası tarafından korunur. Yerleşik sunucu sertifikası her SQL Veritabanı sunucusu için benzersizdir.

Bir veritabanı Geo-DR ilişkisindeyse, her sunucuda farklı bir anahtarla korunur. İki veritabanı aynı sunucuya bağlıysa, aynı yerleşik sertifikayı paylaşırlar. Microsoft bu sertifikaları en az 90 günde bir otomatik olarak döndürür. 

Daha fazla bilgi için [bkz.](/sql/relational-databases/security/encryption/transparent-data-encryption-tde)

### <a name="encryption-in-use-client"></a>Kullanımda şifreleme (istemci)

Veri ihlallerinin çoğu, kredi kartı numaraları veya kişisel olarak tanımlanabilir bilgiler gibi kritik verilerin çalınmasını içerir. Veritabanları hassas bilgilerin hazineleri olabilir. Müşterilerin kişisel verilerini (ulusal kimlik numaraları, gizli rekabet bilgileri ve fikri mülkiyet gibi) içerebilirler. Kayıp veya çalıntı veriler, özellikle müşteri verileri, marka hasarına, rekabet dezavantajına ve ciddi para cezalarına, hatta davalara neden olabilir.

![Her Zaman Şifrelenmiş özellik bir kilit ve anahtar olarak gösterilmiştir](./media/database-security-overview/azure-database-fig1.png)

[Her Zaman Şifrelenmiş,](https://msdn.microsoft.com/library/mt163865.aspx) Azure SQL Veritabanı veya SQL Server veritabanlarında depolanan hassas verileri korumak için tasarlanmış bir özelliktir. Her Zaman Şifrelenmiş istemcilerin istemci uygulamaları içinde hassas verileri şifrelemek ve veritabanı altyapısı (SQL Database veya SQL Server) şifreleme anahtarlarını asla ortaya sağlar.

Her Zaman Şifrelenmiş, verilere sahip olan (ve görüntüleyebilen) kişilerle verileri yöneten (ancak erişimi olmaması gereken) kişiler arasında bir ayrım sağlar. Şirket içi veritabanı yöneticilerinin, bulut veritabanı operatörlerinin veya diğer yüksek ayrıcalıklı ancak yetkisiz kullanıcıların şifrelenmiş verilere erişememesini sağlamaya yardımcı olur.

Buna ek olarak, Her Zaman Şifrelenmiş şifrelemeyi uygulamalariçin saydam hale getirir. İstemci uygulamasındaki hassas verileri otomatik olarak şifreleyip şifresini çözebilmek için istemci bilgisayara her zaman şifrelenmiş bir sürücü yüklenir. Sürücü, verileri veritabanı altyapısına geçirmeden önce verileri hassas sütunlarda şifreler. Sürücü, uygulamanın anlamtlarının korunması için sorguları otomatik olarak yeniden yazar. Benzer şekilde, sürücü sorgu sonuçlarında bulunan şifreli veritabanı sütunlarında depolanan verilerin şifresini saydam olarak çözer.

## <a name="access-control"></a>Erişim denetimi

Güvenliği sağlamak için SQL Veritabanı erişimi şu şekilde denetler:

- IP adresine göre bağlantıyı sınırlayan güvenlik duvarı kuralları.
- Kullanıcıların kimliklerini kanıtlamalarını gerektiren kimlik doğrulama mekanizmaları.
- Kullanıcıları belirli eylemlerle ve verilerle sınırlayan yetkilendirme mekanizmaları.

### <a name="database-access"></a>Veritabanı erişimi

Veri koruması, verilerinize erişimi denetlemeyle başlar. Verilerinizi barındıran veri merkezi fiziksel erişimi yönetir. Ağ katmanındaki güvenliği yönetmek için bir güvenlik duvarı yapılandırabilirsiniz. Ayrıca, kimlik doğrulama için oturum açma işlemlerini yapılandırarak ve sunucu ve veritabanı rolleri için izinleri tanımlayarak erişimi de denetlersiniz.

#### <a name="firewall-and-firewall-rules"></a>Güvenlik duvarı ve güvenlik duvarı kuralları

[Azure SQL Veritabanı,](https://azure.microsoft.com/services/sql-database/) Azure ve diğer internet tabanlı uygulamalar için ilişkisel bir veritabanı hizmeti sağlar. Güvenlik duvarları, verilerinizin korunmasına yardımcı olmak üzere, hangi bilgisayarların izinli olduğunu belirtmenize kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir. Daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanı güvenlik duvarı kurallarına genel bakış](/azure/sql-database/sql-database-firewall-configure).

Azure SQL Veritabanı hizmeti yalnızca TCP bağlantı noktası 1433 üzerinden kullanılabilir. Bilgisayarınızdan bir SQL veritabanına erişmek için istemci bilgisayarınızın güvenlik duvarının TCP bağlantı noktası 1433'te giden TCP iletişimine izin verdiğinden emin olun. Diğer uygulamalar için gelen bağlantılar gerekli değilse, bunları TCP bağlantı noktası 1433'te engelleyin.

#### <a name="authentication"></a>Kimlik doğrulaması

Kimlik doğrulaması, veritabanına bağlanırken kimliğinizi nasıl kanıtlayacağınızı belirtir. SQL Veritabanı iki kimlik doğrulaması türünü destekler:

-   **SQL Server kimlik doğrulaması**: SQL Veritabanı Abone Hesabı adı verilen mantıksal bir SQL örneği oluşturulduğunda tek bir giriş hesabı oluşturulur. Bu hesap SQL [Server kimlik doğrulaması](/azure/sql-database/sql-database-security-overview) (kullanıcı adı ve parola) kullanarak bağlanır. Bu hesap mantıksal sunucu örneğinde ve bu örneğe ekli tüm kullanıcı hesaplarında yöneticidir. Abone hesabının izinleri kısıtlanamaz. Bu hesaplardan yalnızca biri mevcut olabilir.
-   **Azure Etkin Dizin kimlik doğrulaması**: [Azure AD kimlik doğrulaması,](/azure/sql-database/sql-database-aad-authentication) Azure AD'deki kimlikleri kullanarak Azure SQL Veritabanı'na ve Azure SQL Veri Ambarı'na bağlanma mekanizmasıdır. Veritabanı kullanıcılarının kimliklerini merkezi olarak yönetmek için kullanabilirsiniz.

![SQL Veritabanı ile Azure AD kimlik doğrulaması](./media/database-security-overview/azure-database-fig2.png)

 Azure AD kimlik doğrulamasının avantajları şunlardır:
  - SQL Server kimlik doğrulamasına alternatif sağlar.
  - Veritabanı sunucularında kullanıcı kimliklerinin yayılmasını durdurmaya yardımcı olur ve parola döndürmeyi tek bir yerde sağlar.
  - Dış (Azure AD) gruplarını kullanarak veritabanı izinlerini yönetebilirsiniz.
  - Azure AD'nin desteklediği tümleşik Windows kimlik doğrulaması ve diğer kimlik doğrulama biçimlerini etkinleştirerek parola depolamayı ortadan kaldırabilir.

#### <a name="authorization"></a>Yetkilendirme

[Yetkilendirme,](/azure/sql-database/sql-database-manage-logins) bir kullanıcının Azure SQL veritabanında neler yapabileceğini ifade eder. Kullanıcı hesabınızın veritabanı [rol üyelikleri](https://msdn.microsoft.com/library/ms189121) ve [nesne düzeyindeizinler](https://msdn.microsoft.com/library/ms191291.aspx)tarafından denetler. Yetkilendirme, bir anabilgisayarın hangi önemli kaynaklara erişebileceğini ve bu kaynaklar için hangi işlemlere izin verildiğini belirleme işlemidir.

### <a name="application-access"></a>Uygulama erişimi

#### <a name="dynamic-data-masking"></a>Dinamik veri maskeleme

Çağrı merkezindeki bir servis temsilcisi, arayanların sosyal güvenlik numaralarının veya kredi kartı numaralarının birkaç hanesiyle kimliğini belirleyebilir. Ancak bu veri öğeleri hizmet temsilcisine tam olarak maruz kalınmamalıdır.

Herhangi bir sorgunun sonuç kümesinde bir sosyal güvenlik numarasının veya kredi kartı numarasının son dört hanesi hariç tümünü maskeleyen bir maskeleme kuralı tanımlayabilirsiniz.

![SQL veritabanı ve iş uygulamaları arasında gönderilen bir numarayı maskeleme](./media/database-security-overview/azure-database-fig3.png)

Başka bir örnek olarak, kişisel olarak tanımlanabilir bilgileri korumak için uygun bir veri maskesi tanımlanabilir. Daha sonra bir geliştirici, uyumluluk düzenlemelerini ihlal etmeden sorun giderme amacıyla üretim ortamlarını sorgulayabilir.

[SQL Veritabanı dinamik veri maskeleme](/azure/sql-database/sql-database-dynamic-data-masking-get-started), hassas verilerin görünürlüğünü ayrıcalık sahibi olmayan kullanıcılardan gizleyerek sınırlar. Azure SQL Veritabanı'nın V12 sürümü için dinamik veri maskeleme desteklenir.

[Dinamik veri maskeleme,](/sql/relational-databases/security/dynamic-data-masking) hassas verilerin ne kadarının uygulama katmanı üzerinde en az etkiyle ortaya çıkaracağını belirlemenize olanak sağlayarak hassas verilere yetkisiz erişimi önlemeye yardımcı olur. Bu özellik, hassas verileri belirlenen veritabanı alanlarına yapılan sorgunun sonuç kümesinde gizleyen ancak veritabanındaki verileri değiştirmeyen ilke tabanlı bir güvenlik özelliğidir.

> [!Note]
> Dinamik veri maskeleme, Azure Veritabanı yöneticisi, sunucu yöneticisi veya güvenlik görevlisi rolleri tarafından yapılandırılabilir.

#### <a name="row-level-security"></a>Satır Düzeyi Güvenlik

Çok kiracıveritabanları için başka bir ortak güvenlik gereksinimi [Satır Düzey Güvenlik.](https://msdn.microsoft.com/library/dn765131.aspx) Bu özelliği, sorguyu yürüten kullanıcının özelliklerine göre veritabanı tablosundaki satırlara erişimi denetlemek için kullanabilirsiniz. (Örnek özellikler grup üyeliği ve yürütme bağlamıdır.)

![Kullanıcının istemci uygulaması aracılığıyla tablodaki satırlara erişmesine izin veren Satır Düzeyinde Güvenlik](./media/database-security-overview/azure-database-fig4.png)

Erişim kısıtlaması mantığı, başka bir uygulama katmanındaki verilerden uzakta değil, veritabanı katmanında bulunur. Veritabanı sistemi, herhangi bir katmandan veri erişimi denendiği her zaman erişim kısıtlamalarını uygular. Bu, güvenlik sisteminizin yüzey alanını azaltarak güvenlik sisteminizi daha güvenilir ve sağlam hale getirir.

Satır Düzey Güvenlik yüklem tabanlı erişim denetimini tanıtır. Meta verileri veya yöneticinin uygun olarak belirlediği diğer ölçütleri dikkate alabilecek esnek ve merkezi bir değerlendirme içerir. Yüklem, kullanıcının kullanıcı özniteliklerine dayalı verilere uygun erişime sahip olup olmadığını belirlemek için bir ölçüt olarak kullanılır. Yüklem tabanlı erişim denetimini kullanarak etiket tabanlı erişim denetimi uygulayabilirsiniz.

## <a name="proactive-monitoring"></a>Öngörülebilir izleme

SQL *Veritabanı, denetim* ve *tehdit algılama* özellikleri sağlayarak verilerinizin güvenliğini sağlamaya yardımcı olur.

### <a name="auditing"></a>Denetim

[Azure SQL Veritabanı denetimi,](/azure/sql-database/sql-database-auditing-get-started) veritabanında meydana gelen olaylar ve değişiklikler hakkında bilgi edinme yeteneğinizi artırır. Örnekler, verilere karşı güncelleştirmeler ve sorgulardır.

SQL Veritabanı denetimi veritabanı olaylarını izler ve bunları Azure depolama hesabınızdaki bir denetim günlüğüne yazar. Denetim, mevzuata uygunluğu korumanıza, veritabanı etkinliğini anlamanıza ve iş kaygıları veya şüpheli güvenlik ihlallerini gösterebilecek tutarsızlıklar ve anormallikler hakkında bilgi edinmenize yardımcı olabilir. Denetim, uyumluluk standartlarına uygunluğu sağlar ve kolaylaştırır, ancak uyumluluğu garanti etmez.

SQL Veritabanı denetimini şu şekilde kullanabilirsiniz:

- Seçili olayların denetim izini **koruyun.** Denetlenecek veritabanı eylemi kategorilerini tanımlayabilirsiniz.
- Veritabanı etkinliği hakkında **rapor.** Etkinlik ve olay raporlamaya hızlı bir başlangıç yapmak için önceden yapılandırılmış raporları ve bir panoyu kullanabilirsiniz.
- Raporları **analiz edin.** Şüpheli olayları, alışılmışın dışındaki etkinlikleri ve eğilimleri bulabilirsiniz.

İki denetim yöntemi vardır:

-   **Blob denetimi**: Günlükler Azure Blob depolama alanına yazılır. Bu yeni bir denetim yöntemidir. Daha yüksek performans sağlar, daha yüksek parçalı nesne düzeyinde denetimi destekler ve daha uygun maliyetlidir.
-   **Tablo denetimi**: Günlükler Azure Tablo depolama alanına yazılır.

### <a name="threat-detection"></a>Tehdit algılama

[Azure SQL Veritabanı için Gelişmiş Tehdit Koruması,](/azure/sql-database/sql-advanced-threat-protection) olası güvenlik tehditlerini gösteren şüpheli etkinlikleri algılar. Veritabanındaki SQL enjeksiyonları gibi şüpheli olaylara yanıt vermek için tehdit algılamayı kullanabilirsiniz. Uyarılar sağlar ve şüpheli olayları keşfetmek için Azure SQL Veritabanı denetiminin kullanılmasına izin verir.

![Harici bir saldırgan ve kötü niyetli bir içeriden sql veritabanı ve bir web uygulaması için tehdit algılama](./media/database-security-overview/azure-database-fig5.jpg)

SQL Advanced Threat Protection (ATP), Veri Bulma & Sınıflandırması, Güvenlik Açığı Değerlendirmesi ve Tehdit Algılama dahil olmak üzere bir dizi gelişmiş SQL güvenlik yeteneği sağlar. 

- [Veri Bulma & Sınıflandırması](/azure/sql-database/sql-database-data-discovery-and-classification)
- [Güvenlik Açığı Değerlendirmesi](/azure/sql-database/sql-vulnerability-assessment)  
- [Tehdit Algılama](/azure/sql-database/sql-database-threat-detection)

[PostgreSQL Gelişmiş Tehdit Koruması için Azure Veritabanı,](/azure/postgresql/concepts-data-access-and-security-threat-protection) anormal etkinliklerde güvenlik uyarıları sağlayarak olası tehditleri algılamanızı ve bunlara yanıt vermenizi sağlayan yeni bir güvenlik katmanı sağlar. Kullanıcılar, şüpheli veritabanı etkinlikleri ve olası güvenlik açıklarının yanı sıra anormal veritabanı erişimi ve sorgu desenleri hakkında bir uyarı alır. PostgreSQL için Azure Veritabanı için Gelişmiş Tehdit Koruması, uyarıları Azure Güvenlik Merkezi ile tümleştirir. Uyarı türü şunlardır:

- Olağandışı konumdan erişim
- Olağandışı Azure veri merkezinden erişim 
- Yabancı müdürden erişim 
- Zararlı olabilecek bir uygulamadan erişim 
- PostgreSQL kimlik bilgileri için Brute force Azure veritabanı 

[MySQL Gelişmiş Tehdit Koruması için Azure Veritabanı,](/azure/mysql/concepts-data-access-and-security-threat-protection) PostgreSQL Gelişmiş Koruması'na benzer koruma sağlar.  

## <a name="centralized-security-management"></a>Merkezi güvenlik yönetimi

[Azure Güvenlik Merkezi](https://azure.microsoft.com/documentation/services/security-center/), tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar. Aksi takdirde fark edilmeden gidebilecek tehditleri algılamaya yardımcı olur ve geniş bir güvenlik çözümleri ekosistemi ile çalışır.

[Güvenlik Merkezi,](../../security-center/security-center-alerts-data-services.md) tüm sunucularınızın ve veritabanlarınızın güvenliğinde görünürlük sağlayarak SQL Veritabanı'ndaki verileri korumanıza yardımcı olur. Güvenlik Merkezi ile şunları yapabilirsiniz:

- SQL Veritabanı şifreleme ve denetimi için ilkeler tanımlayın.
- Tüm aboneliklerinizde SQL Veritabanı kaynaklarının güvenliğini izleyin.
- Güvenlik sorunlarını hızla tanımlayın ve düzeltin.
- Azure SQL [Veritabanı tehdit algılama](/azure/sql-database/sql-database-threat-detection)uyarılarını tümleştirin.

Güvenlik Merkezi rol tabanlı erişimi destekler.

## <a name="sql-information-protection"></a>SQL Bilgi Koruma

[SQL Bilgi Koruması,](/azure/sql-database/sql-database-data-discovery-and-classification) hassas olabilecek verileri otomatik olarak keşfeder ve sınıflar, hassas verileri sınıflandırma öznitelikleriyle sürekli etiketlemek için bir etiketleme mekanizması sağlar ve veritabanının sınıflandırma durumunu gösteren ayrıntılı bir pano sağlar.  

Ayrıca, sql sorgularının sonuç kümesi duyarlılığını hesaplar, böylece hassas verileri ayıklayan sorgular açıkça denetlenebilir ve veriler korunabilir. SQL Bilgi Koruması hakkında daha fazla bilgi için Azure SQL Veritabanı Veri Bulma ve Sınıflandırma'ya bakın.

Azure Güvenlik Merkezi'nde [SQL Bilgi Koruma ilkelerini](/azure/security-center/security-center-info-protection-policy) yapılandırabilirsiniz.

## <a name="azure-marketplace"></a>Azure Market

Azure Marketi, yeni başlayanlar ve bağımsız yazılım satıcılarının (ISV) çözümlerini dünyanın dört bir yanındaki Azure müşterilerine sunmasını sağlayan çevrimiçi bir uygulama ve hizmet pazarıdır.
Azure Marketi, müşterilere ve iş ortaklarına daha iyi hizmet verebilmek için Microsoft Azure iş ortağı ekosistemlerini birleşik bir platformda birleştirir. Azure Marketi'nde bulunan veritabanı güvenlik ürünlerini görüntülemek için [bir arama çalıştırabilirsiniz.](https://azuremarketplace.microsoft.com/marketplace/apps?search=Database%20Security&page=1)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL veritabanınızı güvenli hale](/azure/sql-database/sql-database-security-tutorial)
- [Azure Güvenlik Merkezi ve Azure SQL Veritabanı hizmeti](/azure/security-center/security-center-sql-database)
- [SQL Veritabanı tehdit algılama](/azure/sql-database/sql-database-threat-detection)
- [SQL veritabanı performansını artırma](/azure/sql-database/sql-database-performance-tutorial)
