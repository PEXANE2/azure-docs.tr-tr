---
title: Geçişten sonra tek ve birleştirilmiş veritabanlarını yönetme
description: Azure SQL Veritabanı'na geçişten sonra veritabanınızı nasıl yöneteceklerini öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: joesackmsft
ms.author: josack
ms.reviewer: sstein
ms.date: 02/13/2019
ms.openlocfilehash: ebb512fee0186bed3cc7f49f0525dac43e57da3a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256191"
---
# <a name="new-dba-in-the-cloud--managing-your-single-and-pooled-databases-in-azure-sql-database"></a>Bulutta Yeni DBA - Azure SQL Veritabanı'nda tek ve birleştirilmiş veritabanlarınızı yönetme

Geleneksel kendi kendini yöneten, kendi kendini kontrol eden ortamdan PaaS ortamına geçmek ilk başta biraz bunaltıcı görünebilir. Bir uygulama geliştiricisi veya DBA olarak, uygulamanızı her zaman kullanılabilir, performanslı, güvenli ve esnek tutmanıza yardımcı olacak platformun temel özelliklerini bilmek istersiniz. Bu makale tam olarak bunu yapmayı amaçlamaktadır. Makale, kaynakları kısaca düzenler ve uygulamanızı verimli bir şekilde yönetmek ve tutmak ve bulutta en iyi sonuçları elde etmek için SQL Veritabanı'nın tek ve havuzlu veritabanlarıile temel yeteneklerini en iyi şekilde nasıl kullanacağınız konusunda size bazı rehberlik sağlar. Bu makale için tipik hedef kitle olanlar olacaktır:

- Uygulama(lar)ın Azure SQL Veritabanına geçişini değerlendiriyoruz – uygulamanızı(lar) modernize ediyoruz.
- Onların uygulama(lar) - Devam eden geçiş senaryosu geçirme sürecinde misiniz.
- Bulutta Azure SQL DB - Yeni DBA'ya geçişi yakın zamanda tamamladınız.

Bu makalede, azure SQL Veritabanı'nın bazı temel özellikleri, tek veritabanları ve esnek havuzlarda birleştirilmiş veritabanları ile çalışırken kolayca yararlanabileceğiniz bir platform olarak açıklanmaktadır. Bunlar şunlardır:

- Azure portalını kullanarak veritabanını izleme
- İş sürekliliği ve olağanüstü durum kurtarma (BCDR)
- Güvenlik ve uyumluluk
- Akıllı veritabanı izleme ve bakım
- Veri taşıma

> [!NOTE]
> Bu makale, Azure SQL Veritabanı'nda aşağıdaki dağıtım seçenekleri için geçerlidir: tek veritabanları ve esnek havuzlar. SQL Veritabanı'nda yönetilen örnek dağıtım seçeneği için geçerli değildir.

## <a name="monitor-databases-using-the-azure-portal"></a>Azure portalını kullanarak veritabanlarını izleme

Azure [portalında,](https://portal.azure.com/)veritabanınızı seçip **İzleme** grafiğini tıklatarak tek bir veritabanları kullanımını izleyebilirsiniz. Bu işlem sonrasında bir **Ölçüm** penceresi görüntülenir. **Grafiği düzenle** düğmesine tıklayarak değişiklik yapabilirsiniz. Şu ölçümleri ekleyin:

- CPU yüzdesi
- DTU yüzdesi
- Veri G/Ç yüzdesi
- Veri boyutu yüzdesi

Bu ölçümleri ekledikten sonra, **Metrik** penceresinde daha fazla bilgi yle bunları **İzleme** grafiğinde görüntülemeye devam edebilirsiniz. Dört ölçümün tümü de veritabanınızın ortalama **DTU** kullanım yüzdesini gösterir. Hizmet katmanları hakkında daha fazla bilgi için [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [vCore tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) makalelerine bakın.  

![Hizmet katmanına göre veritabanı performansını izleme.](./media/sql-database-single-database-monitoring/sqldb_service_tier_monitoring.png)

Performans ölçümlerine ilişkin uyarıları da yapılandırabilirsiniz. **Ölçüm** penceresindeki **Uyarı ekle** düğmesine tıklayın. Uyarınızı yapılandırmak için sihirbazı takip edin. Ölçümlerin belirli bir eşiği aşması veya belirli bir eşiğin altına düşmesi halinde uyarı alabilirsiniz.

Örneğin, veritabanınızdaki bir iş yükünün artmasını bekliyorsanız bir e-posta uyarısı yapılandırarak veritabanınızın herhangi bir performans ölçümünde %80 sınırına ulaşması halinde uyarı alabilirsiniz. Bunu, bir sonraki en yüksek işlem boyutuna ne zaman geçmeniz gerekebileceğini anlamak için erken bir uyarı olarak kullanabilirsiniz.

Performans ölçümleri, daha düşük bir işlem boyutuna indirip düşürebildiğinizi belirlemenize de yardımcı olabilir. Standart S2 veritabanını kullandığınızı ve tüm performans ölçümlerinin, veritabanının belirli bir zaman için ortalama %10'dan daha fazla kullanımda bulunmadığını gösterdiğini varsayın. Bu, veritabanının Standart S1'de de düzgün şekilde çalışabileceğini gösterir. Ancak, daha düşük bir bilgi işlem boyutuna geçme kararı almadan önce ani veya dalgalanan iş yüklerinin farkında olun.

## <a name="business-continuity-and-disaster-recovery-bcdr"></a>İş sürekliliği ve olağanüstü durum kurtarma (BCDR)

İş sürekliliği ve olağanüstü durum kurtarma yetenekleri, bir felaket durumunda, her zamanki gibi işinize devam etmenizi sağlar. Felaket bir veritabanı düzeyinde olay (örneğin, birisi yanlışlıkla önemli bir tablo düşer) veya bir veri merkezi düzeyinde olay (bölgesel felaket, örneğin bir tsunami) olabilir.

### <a name="how-do-i-create-and-manage-backups-on-sql-database"></a>SQL Veritabanı'nda yedeklemeleri nasıl oluşturur ve yönetirim?

Azure SQL DB'de yedekleme oluşturmuyorsunuz, bunun nedeni de bunu yapmak zorunda değilsiniz. SQL Database sizin için veritabanlarını otomatik olarak yedekler, böylece yedeklemeleri zamanlama, alma ve yönetme konusunda artık endişelenmenize gerek yoktur. Platform her hafta tam bir yedekleme, birkaç saatte bir diferansiyel yedekleme ve olağanüstü durum kurtarma verimli ve veri kaybı en az sağlamak için her 5 dakikada bir günlük yedekleme alır. İlk tam yedekleme, bir veritabanı oluşturur oluşturmaz gerçekleşir. Bu yedeklemeler sizin için "Bekletme Süresi" adı verilen belirli bir süre için kullanılabilir ve seçtiğiniz hizmet katmanına göre değişir. SQL Veritabanı, [Point in Time Recovery (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore)kullanarak bu bekletme süresi içinde herhangi bir noktaya geri yükleme olanağı sağlar.

|Hizmet katmanı|Gün içinde bekletme süresi|
|---|:---:|
|Temel|7|
|Standart|35|
|Premium|35|
|||

Buna ek olarak, [Uzun Süreli Bekletme (LTR)](sql-database-long-term-retention.md) özelliği, yedekleme dosyalarınızı özellikle 10 yıla kadar çok daha uzun bir süre boyunca tutmanızı ve bu süre içinde herhangi bir noktada bu yedeklemelerden gelen verileri geri yüklemenize olanak tanır. Ayrıca, veritabanı yedeklemeleri bölgesel felaketten esneklik sağlamak için coğrafi olarak çoğaltılmış depolama da tutulur. Bu yedeklemeleri, bekletme süresi içinde herhangi bir zaman diliminde herhangi bir Azure bölgesinde geri yükleyebilirsiniz. İş [sürekliliğine genel bakışa](sql-database-business-continuity.md)bakın.

### <a name="how-do-i-ensure-business-continuity-in-the-event-of-a-datacenter-level-disaster-or-regional-catastrophe"></a>Veri merkezi düzeyinde bir felaket veya bölgesel bir felaket durumunda iş sürekliliğini nasıl sağlayabilirim?

Veritabanı yedeklemeleriniz, bölgesel bir felaket durumunda yedeklemeyi başka bir Azure bölgesine geri yükleyebildiğinizden emin olmak için coğrafi olarak çoğaltılmış depolama alanında depolanır. Buna coğrafi geri yükleme denir. Bunun için RPO (Kurtarma Noktası Hedefi) genellikle 1 saat < ve ERT (Tahmini Kurtarma Süresi – saat birkaç dakika).

Kritik görev veritabanları için Azure SQL DB etkin coğrafi çoğaltma sunar. Bunun temelde yaptığı şey, özgün veritabanınızın başka bir bölgede coğrafi olarak çoğaltılmış ikincil bir kopyasını oluşturmaktır. Örneğin, veritabanınız başlangıçta Azure Batı ABD bölgesinde barındırılıyorsa ve bölgesel felakete dayanıklılık istiyorsanız. Doğu ABD demek için Batı ABD'de veritabanıaktif bir coğrafi kopya oluşturmak istiyorum. Felaket Batı ABD'ye vurduğunda, Doğu ABD bölgesine başarısız olabilirsiniz. Bu veritabanı otomatik olarak doğu ABD'de ikincil bir felaket durumunda başarısız olmasını sağlar, çünkü bir otomatik failover Grubu bunları yapılandırmak daha iyidir. Bunun için RPO 5 saniye < ve ERT 30 saniye <.

Otomatik arıza grubu yapılandırılmamışsa, uygulamanızın bir felaketi etkin olarak izlemesi ve ikincil bir hata başlatması gerekir. Farklı Azure bölgelerinde bu tür en fazla 4 etkin coğrafi yineleme oluşturabilirsiniz. Daha da iyi oluyor. Salt okunur erişim için bu ikincil etkin coğrafi yinelemelere de erişebilirsiniz. Bu, coğrafi olarak dağıtılan bir uygulama senaryosunun gecikme süresini azaltmak için çok kullanışlı dır.

### <a name="how-does-my-disaster-recovery-plan-change-from-on-premises-to-sql-database"></a>Olağanüstü durum kurtarma planım şirket içi SQL Veritabanı'na nasıl değişir?

Özetle, geleneksel şirket içi SQL Server kurulumu, Failover Clustering, Database Mirroring, Transaction Replication veya Log Shipping gibi özellikleri kullanarak Kullanılabilirliğinizi etkin bir şekilde yönetmenizi ve yedeklemeleri korumanızı ve yönetmenizi zorunlu kılmasını gerektirmektedir. İş Sürekliliği. SQL Database ile platform bunları sizin için yönetir, böylece veritabanı uygulamanızı geliştirmeye ve en iyi duruma göre değerlendirmeye odaklanabilir ve felaket yönetimi konusunda çok fazla endişelenmeyin. Yedekleme ve olağanüstü durum kurtarma planlarının azure portalında (veya PowerShell API'lerini kullanarak birkaç komutla) yalnızca birkaç tıklamayla yapılandırılmasına ve çalışılmasına sahip olabilirsiniz.

Olağanüstü Durum kurtarma hakkında daha fazla bilgi için bkz: [Azure SQL Db Olağanüstü Durum Kurtarma 101](https://azure.microsoft.com/blog/azure-sql-databases-disaster-recovery-101/)

## <a name="security-and-compliance"></a>Güvenlik ve uyumluluk

SQL Veritabanı Güvenlik ve Gizlilik'i çok ciddiye alır. SQL Veritabanı içinde Güvenlik veritabanı düzeyinde ve platform düzeyinde kullanılabilir ve en iyi birkaç katmana kategorize edildiğinde anlaşılır. Her katmanda, uygulamanız için en iyi güvenliği kontrol eder ve sağlarsınız. Katmanlar şunlardır:

- Kimlik & kimlik doğrulama ([SQL kimlik doğrulama ve Azure Etkin Dizin [AAD] kimlik doğrulama).](sql-database-manage-logins.md)
- İzleme faaliyeti ([Denetim](sql-database-auditing.md) ve [tehdit tespiti](sql-database-threat-detection.md)).
- Gerçek verilerin korunması ([Saydam Veri Şifreleme [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) ve [Her Zaman Şifrelenmiş [AE]](/sql/relational-databases/security/encryption/always-encrypted-database-engine)).
- Hassas ve ayrıcalıklı verilere erişimi denetleme[(Satır Düzeyi güvenliği](/sql/relational-databases/security/row-level-security) ve [Dinamik Veri Maskeleme).](/sql/relational-databases/security/dynamic-data-masking)

[Azure Güvenlik Merkezi,](https://azure.microsoft.com/services/security-center/) Azure'da, şirket içinde ve diğer bulutlarda çalışan iş yüklerinde merkezi güvenlik yönetimi sunar. [Denetim](sql-database-auditing.md) ve [Saydam Veri Şifreleme [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) gibi temel SQL Veritabanı korumasının tüm kaynaklarda yapılandırılıp yapılandırılmadığını görebilir ve kendi gereksinimlerinize göre ilkeler oluşturabilirsiniz.

### <a name="what-user-authentication-methods-are-offered-in-sql-database"></a>SQL Veritabanı'nda hangi kullanıcı kimlik doğrulama yöntemleri sunulur?

SQL Veritabanı'nda sunulan iki kimlik doğrulama yöntemi vardır:

- [Azure Active Directory Kimlik Doğrulaması](sql-database-aad-authentication.md)
- [SQL kimlik doğrulaması](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode#connecting-through-sql-server-authentication)

Geleneksel windows kimlik doğrulaması desteklenmez. Azure Active Directory (AD), merkezi bir kimlik ve erişim yönetimi hizmetidir. Bununla, kuruluşunuzdaki tüm personele tek oturum açma erişimi (SSO) sağlayabilirsiniz. Bunun anlamı, kimlik bilgilerinin daha basit kimlik doğrulaması için tüm Azure hizmetlerinde paylaşılmasıdır. AAD [MFA (Multi Factor Authentication)](sql-database-ssms-mfa-authentication.md) destekler ve [birkaç tıklama](../active-directory/hybrid/how-to-connect-install-express.md) ile AAD Windows Server Active Directory ile entegre edilebilir. SQL Kimlik Doğrulaması, geçmişte kullandığınız gibi çalışır. Bir kullanıcı adı/parola sağlarsınız ve kullanıcıları belirli bir SQL Veritabanı sunucusundaki herhangi bir veritabanına doğrulayabilirsiniz. Bu, SQL Veritabanı ve SQL Veri Ambarı'nın bir Azure AD etki alanında çok faktörlü kimlik doğrulama ve konuk kullanıcı hesapları sunmasına da olanak tanır. Zaten şirket içinde Bir Active Directory'niz varsa, dizininizi Azure'a genişletmek için Dizini Azure Etkin Dizini ile fesatlayabilirsiniz.

|**Eğer ...**|**SQL Veritabanı / SQL Veri Ambarı**|
|---|---|
|Azure'da Azure Etkin Dizini (AD) kullanmamayı tercih etme|[SQL kimlik doğrulamasını](sql-database-security-overview.md) kullanma|
|SQL Server'da kullanılan AD|[Azure AD ile Federate AD](../active-directory/hybrid/whatis-hybrid-identity.md)ve Azure AD kimlik doğrulaması kullanın. Bununla Tek Oturum Açma'yı kullanabilirsiniz.|
|Çok faktörlü kimlik doğrulama (MFA) zorlamak gerekir|[Microsoft Koşullu Erişim](sql-database-conditional-access.md)aracılığıyla MFA'yı bir ilke olarak zorunlu kılmasını ve [MFA desteğiyle Azure AD Evrensel kimlik doğrulamasını](sql-database-ssms-mfa-authentication.md)kullanın.|
|Microsoft hesaplarından (live.com, outlook.com) veya diğer etki alanlarından (gmail.com) konuk hesaplarına sahip olun|[Azure AD B2B İşbirliği'ni](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md)kullanan SQL Veritabanı/Veri Ambarı'nda [Azure AD Evrensel kimlik doğrulamasını](sql-database-ssms-mfa-authentication.md) kullanın.|
|Federe bir etki alanından Azure AD kimlik bilgilerinizi kullanarak Windows'da oturum açtınız|[Azure AD tümleşik kimlik doğrulamasını](sql-database-aad-authentication-configure.md)kullanın.|
|Azure ile birlikte federe olmayan bir etki alanından kimlik bilgilerini kullanarak Windows'da oturum açtılar|[Azure AD tümleşik kimlik doğrulamasını](sql-database-aad-authentication-configure.md)kullanın.|
|SQL Veritabanı'na veya SQL Veri Ambarı'na bağlanması gereken orta katman hizmetlerine sahip|[Azure AD tümleşik kimlik doğrulamasını](sql-database-aad-authentication-configure.md)kullanın.|
|||

### <a name="how-do-i-limit-or-control-connectivity-access-to-my-database"></a>Veritabanıma bağlantı erişimini nasıl sınırlandırıyorum veya denetlerim?

Uygulamanız için en iyi bağlantı organizasyonuna ulaşmak için kullanabileceğiniz birden çok teknik vardır.

- Güvenlik Duvarı Kuralları
- VNet Hizmet Bitiş Noktaları
- Ayrılmış IP’ler

#### <a name="firewall"></a>Güvenlik duvarı

Güvenlik duvarı, yalnızca belirli varlıkların SQL Veritabanı sunucunuza erişmesine izin vererek sunucunuza harici bir varlıktan erişmesini engeller. Varsayılan olarak, diğer Azure Hizmetlerinden gelen bağlantılar dışında, SQL Veritabanı sunucusundaki tüm bağlantılara ve veritabanlarına izin verilmez. Güvenlik duvarı kuralıyla, sunucunuza erişimi yalnızca onayladığınız varlıklara (örneğin, bir geliştirici makinesi) açarak, bilgisayarın IP adresinin güvenlik duvarından geçmesine izin verebilirsiniz. Ayrıca, SQL Veritabanı sunucusuna erişime izin vermek istediğiniz bir IP aralığı belirtmenize de olanak tanır. Örneğin, kuruluşunuzdaki geliştirici makinesi IP adresleri, Güvenlik Duvarı ayarları sayfasında bir aralık belirterek aynı anda eklenebilir.

Sunucu düzeyinde veya veritabanı düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz. Sunucu düzeyindeki IP güvenlik duvarı kuralları Azure portalı kullanılarak veya SSMS ile oluşturulabilir. Sunucu düzeyi ve veritabanı düzeyinde güvenlik duvarı kuralının nasıl ayarlan olduğu hakkında daha fazla bilgi edinmek için bkz: [SQL Veritabanı'nda IP güvenlik duvarı kuralları oluşturun.](sql-database-security-tutorial.md#create-firewall-rules)

#### <a name="service-endpoints"></a>Hizmet uç noktaları

Varsayılan olarak, SQL veritabanınız "Azure hizmetlerinin sunucuya erişmesine izin ver" olarak yapılandırılır , bu da Azure'daki herhangi bir Sanal Makinenin veritabanınıza bağlanmayı deneyebileceği anlamına gelir. Bu girişimlerin hala doğrulaması gerekiyor. Ancak, veritabanınızın herhangi bir Azure IP'si tarafından erişilebilmesini istemiyorsanız, "Azure hizmetlerinin sunucuya erişmesine izin ver"i devre dışı kullanabilirsiniz. Ayrıca, [VNet Hizmet Bitiş Noktalarını](sql-database-vnet-service-endpoint-rule-overview.md)yapılandırabilirsiniz.

Hizmet bitiş noktaları (SE), kritik Azure kaynaklarınızı yalnızca Azure'daki kendi özel sanal ağınıza maruz bırakmanızı sağlar. Bunu yaparak, kaynaklarına genel erişimi ortadan kaldırırsınız. Sanal ağınız ile Azure arasındaki trafik Azure omurga ağında kalır. SE olmadan zorunlu tünel paket yönlendirme olsun. Sanal ağınız, internet trafiğini kuruluşunuz ve Azure Hizmeti trafiğini aynı rotayı aşmaya zorlar. Hizmet Bitiş Noktaları ile paketler doğrudan sanal ağınızdan Azure omurga ağındaki hizmete aktığı için bunu optimize edebilirsiniz.

![Sanal Ağ hizmet uç noktaları](./media/sql-database-manage-after-migration/vnet-service-endpoints.png)

#### <a name="reserved-ips"></a>Ayrılmış IP’ler

Başka bir seçenek, VM'leriniz için [ayrılmış IP'leri](../virtual-network/virtual-networks-reserved-public-ip.md) sağlamak ve sunucu güvenlik duvarı ayarlarına bu özel VM IP adreslerini eklemektir. Ayrılmış IP'ler atayarak, IP adreslerini değiştirerek güvenlik duvarı kurallarını güncelleştirmek zorunda kalma zahmetini kaydetmiş olursunuz.

### <a name="what-port-do-i-connect-to-sql-database-on"></a>SQL Veritabanına hangi bağlantı noktasında bağlanıyorum

Bağlantı noktası 1433. SQL Veritabanı bu bağlantı noktası üzerinden iletişim kurar. Bir şirket ağındabağlanmak için kuruluşunuzun güvenlik duvarı ayarlarına giden bir kural eklemeniz gerekir. Kılavuz olarak, 1433 bağlantı noktasını Azure sınırının dışına çıkarmaktan kaçının.

### <a name="how-can-i-monitor-and-regulate-activity-on-my-server-and-database-in-sql-database"></a>SQL Veritabanı'ndaki sunucum ve veritabanımdaki etkinliği nasıl izleyebilir ve düzenleyebilirim?

#### <a name="sql-database-auditing"></a>SQL Veritabanı Denetimi

SQL Database ile veritabanı olaylarını izlemek için On Auditing'i açabilirsiniz. [SQL Veritabanı Denetimi](sql-database-auditing.md) veritabanı olaylarını kaydeder ve bunları Azure Depolama Hesabınızdaki bir denetim günlüğü dosyasına yazar. Denetim, özellikle olası güvenlik ve politika ihlalleri hakkında bilgi edinmek, mevzuata uygunluğu korumak vb. için yararlıdır. Denetime ihtiyaç duyduğunuz belirli olay kategorilerini tanımlamanıza ve yapılandırmanıza olanak tanır ve veritabanınızda meydana gelen olaylara genel bir bakış elde etmek için önceden yapılandırılmış raporlar ve bir pano elde edebilirsiniz. Bu denetim ilkelerini veritabanı düzeyinde veya sunucu düzeyinde uygulayabilirsiniz. Sunucunuz/veritabanınız için denetimi nasıl [etkinleştirin,](sql-database-security-tutorial.md#enable-security-features)bkz.

#### <a name="threat-detection"></a>Tehdit algılama

[Tehdit algılama](sql-database-threat-detection.md)ile, çok kolay Denetim tarafından keşfedilen güvenlik veya politika ihlalleri üzerine hareket etme yeteneğine sahip olursunuz. Sisteminizdeki olası tehditleri veya ihlalleri gidermek için bir güvenlik uzmanı olmanız gerekmez. Tehdit algılama da SQL Injection algılama gibi bazı yerleşik yetenekleri vardır. SQL Injection, verileri değiştirme veya tehlikeye atma girişimidir ve genel olarak bir veritabanı uygulamasına saldırmak için oldukça yaygın bir yoldur. Tehdit algılama, olası güvenlik açıklarını ve SQL enjeksiyon saldırılarını algılayan birden çok algoritma kümesinin yanı sıra anormal veritabanı erişim desenleri (olağandışı bir konumdan veya yabancı bir müdür tarafından erişim gibi) çalıştırın. Veritabanında bir tehdit algılanırsa, güvenlik görevlileri veya diğer atanmış yöneticilere bir e-posta bildirimi gönderilir. Her bildirim, şüpheli etkinliğin ayrıntılarını ve tehdidin nasıl daha fazla araştırılabildiğini ve azat edilebildiğini anlatan öneriler sağlar. Tehdit algılamayı nasıl açabilirsiniz öğrenmek için bkz: [Tehdit algılamayı etkinleştirin.](sql-database-security-tutorial.md#enable-security-features)

### <a name="how-do-i-protect-my-data-in-general-on-sql-database"></a>SQL Veritabanı'nda verilerimi genel olarak nasıl korurum?

Şifreleme, hassas verilerinizi davetsiz misafirlerden korumak ve korumak için güçlü bir mekanizma sağlar. Şifre çözme anahtarı olmadan şifreçözücünün hiçbir faydası yoktur. Böylece, SQL Veritabanı'nda yerleşik güvenlik varolan katmanlarının üstüne ek bir koruma katmanı ekler. SQL Veritabanı'nda verilerinizi korumanın iki yönü vardır:

- Veri ve günlük dosyalarında dinlendirilen verileriniz
- Uçuş halindeolan verileriniz

SQL Veritabanı'nda, varsayılan olarak, depolama alt sistemindeki veri ve günlük dosyalarında bulunan verileriniz tamamen ve her zaman [Saydam Veri Şifreleme [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)ile şifrelenir. Yedekleriniz de şifrelenir. TDE ile uygulama tarafınızda bu verilere erişen herhangi bir değişiklik gerekmez. Şifreleme ve şifre çözme saydam olur; bu nedenle adı.
Sql Veritabanı, hassas verilerinizi uçuş sırasında ve istirahatte korumak için [Her Zaman Şifrelenmiş (AE)](/sql/relational-databases/security/encryption/always-encrypted-database-engine)adlı bir özellik sağlar. AE, veritabanınızdaki hassas sütunları şifreleyen istemci tarafı şifreleme biçimidir (bu nedenle veritabanı yöneticilerine ve yetkisiz kullanıcılara şifreli olarak bulunurlar). Sunucu başlangıçta şifrelenmiş verileri alır. Her Zaman Şifrelenmiş'in anahtarı istemci tarafında da depolanır, böylece hassas sütunların şifresini yalnızca yetkili istemciler çözebilir. Şifreleme anahtarları istemcide depolandığından, sunucu ve veri yöneticileri hassas verileri göremez. AE, yetkisiz istemcilerden fiziksel diske kadar tablodaki hassas sütunları uçtan uca şifreler. AE bugün eşitlik karşılaştırmalarını destekler, böylece DAB'ler SQL komutlarının bir parçası olarak şifrelenmiş sütunları sorgulamaya devam edebilir. Her zaman Şifrelenmiş, [Azure Key Vault,](sql-database-always-encrypted-azure-key-vault.md)Windows sertifika mağazası ve yerel donanım güvenlik modülleri gibi çeşitli önemli mağaza seçenekleriyle kullanılabilir.

|**Özellikler**|**Always Encrypted**|**Şeffaf Veri Şifreleme**|
|---|---|---|
|**Şifreleme açıklığı**|Uçuç|Istirahat verisi|
|**Veritabanı sunucusu hassas verilere erişebilir**|Hayır|Evet, şifreleme veri için olduğu için|
|**İzin verilen T-SQL işlemleri**|Eşitlik karşılaştırması|Tüm T-SQL yüzey alanı mevcuttur|
|**Özelliği kullanmak için gereken uygulama değişiklikleri**|En az|Çok Minimal|
|**Şifreleme parçalı**|Sütun düzeyi|Veritabanı düzeyinde Kimlik Bilgileri belirleme seçeneği|
||||

### <a name="how-can-i-limit-access-to-sensitive-data-in-my-database"></a>Veritabanımdaki hassas verilere erişimi nasıl sınırlandırabilirim?

Her uygulamanın veritabanında herkes tarafından görülebilmesiiçin korunması gereken belirli bir hassas veri parçası vardır. Kuruluş içindeki belirli personelin bu verileri görüntülemesi gerekir, ancak diğerleri bu verileri görüntüleyememelidir. Bir örnek çalışan ücretleriolduğunu. Bir yöneticinin doğrudan raporları için ücret bilgilerine erişmesi gerekir, ancak bireysel ekip üyelerinin akranlarının ücret bilgilerine erişememeleri gerekir. Başka bir senaryo geliştirme aşamalarında veya test sırasında hassas verilerle etkileşim olabilir veri geliştiricileri , örneğin, müşterilerin SSN'leri. Bu bilgilerin yine geliştiriciye açık olması gerekmez. Bu gibi durumlarda, hassas verilerinizin maskelemesi veya hiç açığa çıkmaması gerekir. SQL Veritabanı, yetkisiz kullanıcıların hassas verileri görüntüleyebilmelerini önlemek için iki tür yaklaşım sunar:

[Dinamik Veri Maskeleme,](sql-database-dynamic-data-masking-get-started.md) uygulama katmanındaki ayrıcalıklı olmayan kullanıcılarla maskeleyerek hassas veri maruziyetini sınırlamanızı sağlayan bir veri maskeleme özelliğidir. Maskeleme deseni oluşturabilecek bir maskeleme kuralı tanımlarsınız (örneğin, ulusal bir Kimlik SSN'nin son dört hanesini göstermek için: XXX-XX-0000 ve çoğunu X olarak işaretlersiniz) ve hangi kullanıcıların maskeleme kuralından dışlanacaklarını belirlersiniz. Maskeleme anında olur ve çeşitli veri kategorileri için çeşitli maskeleme işlevleri vardır. Dinamik veri maskeleme, veritabanınızdaki hassas verileri otomatik olarak algılamanızı ve maskeleme uygulamanızı sağlar.

[Satır Düzeyi güvenliği,](/sql/relational-databases/security/row-level-security) satırı düzeyinde erişimi denetlemenizi sağlar. Anlamı, sorgu (grup üyeliği veya yürütme bağlamı) çalıştıran kullanıcı dayalı bir veritabanı tablosunda belirli satırlar gizlidir. Erişim kısıtlaması, uygulama mantığınızı basitleştirmek için uygulama katmanı yerine veritabanı katmanında yapılır. Bir filtre yüklemi oluşturarak, açıkta olmayan satırları filtreleyerek ve bu satırlara kimlerin erişeceğini tanımlayan güvenlik ilkesini sonraki olarak başlatın. Son olarak, son kullanıcı sorgularını çalıştırıyor ve kullanıcının ayrıcalığına bağlı olarak, bu kısıtlı satırları görüntüleyebilir veya hiç göremiyor.

### <a name="how-do-i-manage-encryption-keys-in-the-cloud"></a>Buluttaki şifreleme anahtarlarını nasıl yönetirim?

Hem Her Zaman Şifrelenmiş (istemci tarafı şifreleme) hem de Saydam Veri Şifrelemesi (şifreleme) için anahtar yönetim seçenekleri vardır. Şifreleme anahtarlarını düzenli olarak döndürmeniz önerilir. Döndürme sıklığı hem dahili kuruluş düzenlemelerinizle hem de uyumluluk gereksinimlerinize uygun olmalıdır.

#### <a name="transparent-data-encryption-tde"></a>Saydam Veri Şifrelemesi (TDE)

TDE'de iki anahtarlı bir hiyerarşi vardır – her kullanıcı veritabanındaki veriler, sunucuya özgü asimetrik RSA 2048 ana anahtarı yla şifrelenen simetrik Bir AES-256 veritabanına özgü veritabanı şifreleme anahtarı (DEK) ile şifrelenir. Ana anahtar aşağıdakilerden biri de yönetilebilir:

- Otomatik olarak platform - SQL Veritabanı.
- Veya anahtar deposu olarak [Azure Key Vault'u](sql-database-always-encrypted-azure-key-vault.md) kullanarak.

Varsayılan olarak, Saydam Veri Şifreleme için ana anahtar kolaylık sağlamak için SQL Veritabanı hizmeti tarafından yönetilir. Kuruluşunuz ana anahtar üzerinde denetim yapmak istiyorsa, anahtar deposu olarak Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md) kullanma seçeneği vardır. Kuruluşunuz Azure Key Vault'u kullanarak anahtar sağlama, döndürme ve izin denetimleri üzerinde denetimi üstlenir. [TDE ana anahtarının türünü döndürme veya değiştirme](/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql-key-rotation) hızlıdır, çünkü yalnızca DEK'i yeniden şifreler. Güvenlik ve veri yönetimi arasında rollerin ayrılması olan kuruluşlar için, bir güvenlik yöneticisi Azure Key Vault'taki TDE ana anahtarıiçin anahtar malzemeyi sağlayabilir ve veritabanı yöneticisine kullanmak üzere bir Azure Key Vault anahtar tanımlayıcısı sağlayabilir bir sunucuda dinlenme şifreleme. Key Vault, Microsoft'un herhangi bir şifreleme anahtarını göremeyecek veya ayıklamayacak şekilde tasarlanmıştır. Ayrıca, kuruluşunuz için anahtarların merkezi bir yönetimini de elde elabilirsiniz.

#### <a name="always-encrypted"></a>Always Encrypted

Her Zaman [Şifrelenmiş'te iki anahtarlı](/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted) bir hiyerarşi de vardır - hassas veri sütunu, sırayla bir sütun ana anahtarı (CMK) tarafından şifrelenen Bir AES 256 sütunlu şifreleme anahtarı (CEK) tarafından şifrelenir. Always Encrypted için sağlanan istemci sürücülerin CMK'ların uzunluğu yla ilgili herhangi bir sınırlaması yoktur. CEK'nin şifrelenmiş değeri veritabanında depolanır ve CMK, Windows Certificate Store, Azure Key Vault veya donanım güvenlik modülü gibi güvenilir bir anahtar deposunda depolanır.

- Hem [CEK hem](/sql/relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell) de CMK döndürülebilir.
- CEK döndürme, veri işleminin boyutudur ve şifrelenmiş sütunları içeren tabloların boyutuna bağlı olarak zaman yoğun olabilir. Bu nedenle CEK rotasyonlarını buna göre planlamak ihtiyatlı dır.
- Ancak CMK döndürme veritabanı performansını engellemez ve ayrılmış rollerle yapılabilir.

Aşağıdaki diyagram, Her Zaman Şifrelenmiş sütun ana anahtarları için anahtar deposu seçeneklerini gösterir

![Her zaman şifreli CMK mağaza sağlayıcıları](./media/sql-database-manage-after-migration/always-encrypted.png)

### <a name="how-can-i-optimize-and-secure-the-traffic-between-my-organization-and-sql-database"></a>Kuruluşum ve SQL Veritabanım arasındaki trafiği nasıl optimize edebilir ve güvenli hale getirebilirim?

Kuruluşunuz ve SQL Veritabanı arasındaki ağ trafiği genellikle ortak ağ üzerinden yönlendirilir. Ancak, bu yolu en iyi duruma getirmeyi ve daha güvenli hale getirmeyi seçerseniz, Express Route'a bakabilirsiniz. Ekspres rota, şirket ağınızı özel bir bağlantı üzerinden Azure platformuna genişletmenize olanak tanır. Bunu yaparak, genel Internet üzerinden gitmez. Ayrıca, normalde genel internet üzerinden giderek deneyim olacağını daha düşük ağ gecikmeleri ve çok daha hızlı çevirir daha yüksek güvenlik, güvenilirlik ve yönlendirme optimizasyonu olsun. Kuruluşunuz ve Azure arasında önemli miktarda veri aktarmayı planlıyorsanız, Express Route'u kullanmak maliyet avantajları sağlayabilir. Kuruluşunuzdan Azure'a bağlantı için üç farklı bağlantı modeli arasından seçim yapabilirsiniz:

- [Bulut Exchange Ortak konumu](../expressroute/expressroute-connectivity-models.md#CloudExchange)
- [Herhangi biri için](../expressroute/expressroute-connectivity-models.md#IPVPN)
- [Noktadan noktaya](../expressroute/expressroute-connectivity-models.md#Ethernet)

Express Route ayrıca ek ücret ödemeden satın aldığınız bant genişliği limitinin 2 kata kadar patlamanızı sağlar. Express rotasını kullanarak çapraz bölge bağlantısını yapılandırmak da mümkündür. ER bağlantı sağlayıcılarının listesini görmek için bkz: [Ekspres Rota Ortakları ve Bakan Konumlar.](../expressroute/expressroute-locations.md) Aşağıdaki makaleler Express Route'u daha ayrıntılı olarak açıklayınız:

- [Ekspres Rotaya Giriş](../expressroute/expressroute-introduction.md)
- [Ön koşullar](../expressroute/expressroute-prerequisites.md)
- [İş akışları](../expressroute/expressroute-workflows.md)

### <a name="is-sql-database-compliant-with-any-regulatory-requirements-and-how-does-that-help-with-my-own-organizations-compliance"></a>SQL Veritabanı herhangi bir düzenleyici gereklilikle uyumlu mudur ve bu benim kendi kuruluşumun uyumluluğuna nasıl yardımcı olur?

SQL Veritabanı, bir dizi düzenleyici uyumlulukla uyumludur. SQL Veritabanı tarafından karşılanan en son uyumluluk kümesini görüntülemek için [Microsoft Güven Merkezi'ni](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) ziyaret edin ve SQL Veritabanı'nın uyumlu Azure hizmetlerine dahil edilip edilemediğini görmek için kuruluşunuz için önemli olan derlemeleri inceleyin. SQL Veritabanı uyumlu bir hizmet olarak sertifikalandırılabilir, ancak kuruluşunuzun hizmetinin uyumluluğuna yardımcı olur, ancak otomatik olarak garanti etmez dikkat etmek önemlidir.

## <a name="intelligent-database-monitoring-and-maintenance-after-migration"></a>Geçiş sonrası akıllı veritabanı izleme ve bakım

Veritabanınızı SQL Veritabanı'na geçirdikten sonra veritabanınızı izlemek (örneğin, kaynak kullanımının nasıl olduğunu veya DBCC denetimlerini kontrol etmek) ve düzenli bakım (örneğin, dizinleri, istatistikleri yeniden oluşturma veya yeniden düzenleme vb.) yapmak isteyeceksiniz. Neyse ki, SQL Veritabanı, uygulamanızın her zaman en iyi şekilde çalışması için veritabanınızı proaktif olarak izlemenize ve korumanıza yardımcı olmak için geçmiş eğilimleri ve kaydedilmiş ölçümleri ve istatistikleri kullanması açısından akıllıdır. Bazı durumlarda, Azure SQL DB yapılandırma kurulumunuza bağlı olarak bakım görevlerini otomatik olarak gerçekleştirebilir. Veritabanınızı SQL Veritabanı'nda izlemek için üç yönü vardır:

- Performans izleme ve optimizasyon.
- Güvenlik optimizasyonu.
- Maliyet optimizasyonu.

### <a name="performance-monitoring-and-optimization"></a>Performans izleme ve optimizasyon

Sorgu Performans Öngörüleri ile, uygulamalarınızın her zaman en uygun düzeyde çalışmaya devam edebilmeleri için veritabanı iş yükünüz için özel leştirilmiş öneriler alabilirsiniz. Ayrıca, bu önerilerin otomatik olarak uygulanması ve bakım görevlerini yerine getirme zahmetine girmeniz gerekmeyecek şekilde ayarlayabilirsiniz. Index Advisor ile, iş yükünüze bağlı olarak dizin önerilerini otomatik olarak uygulayabilirsiniz - buna Otomatik Ayar denir. Öneriler, uygulama iş yükünüz değiştikçe size en alakalı önerileri sunmak için gelişir. Ayrıca, bu önerileri el ile gözden geçirme ve kendi takdirinize bağlı olarak uygulama seçeneğine de elde elabilirsiniz.  

### <a name="security-optimization"></a>Güvenlik optimizasyonu

SQL Veritabanı, veritabanında olası bir iş parçacığı oluşturabilecek şüpheli veritabanı etkinliklerini tanımlamak ve araştırmak için verilerinizi ve tehdit algılamanızı güvenli hale getirmek için eyleme geçirilebilir güvenlik önerileri sağlar. [Güvenlik açığı değerlendirmesi,](sql-vulnerability-assessment.md) veritabanlarınızın güvenlik durumunu ölçekte izlemenize ve güvenlik risklerini belirlemenize ve sizin tanımladığınız bir güvenlik taban çizgisinden uzaklaşmanıza olanak tanıyan bir veritabanı tarama ve raporlama hizmetidir. Her taradan sonra, işlem yapılabilir adımlar ve düzeltme komut dosyalarının özelleştirilmiş bir listesi nin yanı sıra uyumluluk gereksinimlerini karşılamaya yardımcı olmak için kullanılabilecek bir değerlendirme raporu sağlanır.

Azure Güvenlik Merkezi ile, güvenlik önerilerini tüm yönetim kurulunda tanımlar ve bunları tek bir tıklamayla uygularsınız.

### <a name="cost-optimization"></a>Maliyet iyileştirmesi

Azure SQL platformu, sizin için maliyet optimizasyonu seçeneklerini değerlendirmek ve önermek için bir sunucudaki veritabanlarındaki kullanım geçmişini analiz eder. Bu çözümleme genellikle analiz etmek ve eyleme geçirilebilir öneriler oluşturmak için iki hafta sürer. Elastik havuz böyle bir seçenektir. Öneri, portalda bir başlık olarak görünür:

![elastik havuz önerileri](./media/sql-database-manage-after-migration/elastic-pool-recommendations.png)

Bu analizi "Danışman" bölümünde de görüntüleyebilirsiniz:

![elastik havuz önerileri-danışman](./media/sql-database-manage-after-migration/advisor-section.png)

### <a name="how-do-i-monitor-the-performance-and-resource-utilization-in-sql-database"></a>SQL Veritabanı'nda performans ve kaynak kullanımını nasıl izleyebilirim?

SQL Veritabanı'nda performansı izlemek ve buna göre ayarlamak için platformun akıllı öngörülerinden yararlanabilirsiniz. SQL Veritabanı'nda performans ve kaynak kullanımını aşağıdaki yöntemleri kullanarak izleyebilirsiniz:

#### <a name="azure-portal"></a>Azure portalında

Azure portalı, veritabanını seçerek ve Genel Bakış bölmesinde grafiği tıklatarak veritabanının kullanımını gösterir. Cpu yüzdesi, DTU yüzdesi, Veri IO yüzdesi, Oturumlar yüzdesi ve Veritabanı boyutu yüzdesi dahil olmak üzere birden çok ölçüm gösterecek şekilde grafiği değiştirebilirsiniz.

![İzleme grafiği](./media/sql-database-manage-after-migration/monitoring-chart.png)

![İzleme grafiği2](./media/sql-database-manage-after-migration/chart.png)

Bu grafikten, uyarıları kaynağa göre de yapılandırabilirsiniz. Bu uyarılar, kaynak koşullarına bir e-postayla yanıt vermenize, https/HTTP bitiş noktasına yazmanıza veya bir eylem gerçekleştirmenize olanak sağlar. Daha fazla bilgi için [bkz.](sql-database-insights-alerts-portal.md)

#### <a name="dynamic-management-views"></a>Dinamik Yönetim Görünümleri

Kaynak tüketim istatistikleri geçmişini son saatten döndürmek için [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) dinamik yönetim görünümünü ve son 14 günün geçmişini döndürmek için [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) sistem kataloğu görünümünü sorgulayabilirsiniz.

#### <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri

[Sorgu Performans Öngörüsü,](sql-database-query-performance.md) belirli bir veritabanı için en çok kaynak tüketen sorguların ve uzun süren sorguların geçmişini görmenizi sağlar. Kaynak kullanımına, süresine ve yürütme sıklığına göre TOP sorgularını hızla tanımlayabilirsiniz. Sorguları izleyebilir ve gerilemeyi algılayabilirsiniz. Bu özellik, [Sorgu Deposu'nun](/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) veritabanı için etkin ve etkin olmasını gerektirir.

![Sorgu Performansı İçgörüleri](./media/sql-database-manage-after-migration/query-performance-insight.png)

#### <a name="azure-sql-analytics-preview-in-azure-monitor-logs"></a>Azure Monitor günlüklerinde Azure SQL Analytics (Önizleme)

[Azure Monitor günlükleri,](../azure-monitor/insights/azure-sql.md) çalışma alanı başına 150.000'e kadar SQL Veritabanı ve 5.000 SQL Elastik havuzu destekleyerek önemli Azure SQL veritabanı performans ölçümlerini toplamanızı ve görselleştirmenizi sağlar. Bildirimleri izlemek ve almak için kullanabilirsiniz. Sql Veritabanı'nı ve elastik havuz ölçümlerini birden çok Azure aboneliği ve elastik havuzda izleyebilir ve uygulama yığınının her katmanındaki sorunları tanımlamak için kullanılabilir.

### <a name="i-am-noticing-performance-issues-how-does-my-sql-database-troubleshooting-methodology-differ-from-sql-server"></a>Performans sorunları fark ediyorum: SQL Veritabanı sorun giderme metodolojimin SQL Server'dan farkı nedir?

Sorgu ve veritabanı performans sorunlarını tanılamak için kullanacağınız sorun giderme tekniklerinin önemli bir bölümü aynı kalır. Sonuçta aynı SQL Server motoru bulut güçler. Ancak, platform - Azure SQL DB 'istihbarat' inşa etti. Performans sorunlarını daha kolay gidermenize ve tanılamanıza yardımcı olabilir. Ayrıca, bu düzeltici eylemlerin bazılarını sizin adınıza gerçekleştirebilir ve bazı durumlarda bunları proaktif olarak düzeltebilir - otomatik olarak.

Sorun giderme performansı sorunlarına yaklaşımınız, Sorgu [Performans Öngörüsü (QPI)](sql-database-query-performance.md) ve [Veritabanı Danışmanı](sql-database-advisor.md) gibi akıllı özellikleri birlikte kullanarak önemli ölçüde fayda sağlayabilir ve böylece metodoloji farkı bu açıdan farklılık gösterir – artık sorunu gidermenize yardımcı olabilecek temel ayrıntıları öğütme zahmetine girmeniz gerekmez. Platform sizin için zor işi yapar. Bunun bir örneği QPI olduğunu. QPI ile, sorgu düzeyine kadar sondaj yapabilir ve geçmiş eğilimlere bakabilir ve sorgunun tam olarak ne zaman gerilediğini anlayabilirsiniz. Veritabanı Danışmanı, genel olarak genel performansınızı artırmanıza yardımcı olabilecek öneriler sunar - eksik dizinler, dizinbırakma, sorgularınızı parametreleme vb.

Performans sorun giderme de, uygulama performansınızı etkileyen sadece uygulama veya veritabanı onu destekleyen olup olmadığını belirlemek önemlidir. Genellikle performans sorunu uygulama katmanında yatıyor. Mimari ya da veri erişim deseni olabilir. Örneğin, ağ gecikmesine duyarlı geveze bir uygulamanız olduğunu göz önünde bulundurun. Bu durumda, uygulamanız, uygulama ile sunucu arasında ve sıkışık bir ağda ileri geri giden birçok kısa istek olacağı için uygulamanız zarar görür, bu tur yolculuklar hızlı bir şekilde toplanır. Bu durumda performansı artırmak için [Toplu Sorgular'ı](sql-database-performance-guidance.md#batch-queries)kullanabilirsiniz. Toplu iş kullanmak size muazzam ölçüde yardımcı olur, çünkü artık istekleriniz toplu olarak işlenir; böylece, gidiş dönüş gecikmesini azaltmanıza ve uygulama performansınızı artırmanıza yardımcı olur.

Ayrıca, veritabanınızın genel performansında bir bozulma fark ederseniz, CPU, IO ve bellek tüketimini anlamak için [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) ve [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) dinamik yönetim görünümlerini izleyebilirsiniz. Veritabanınız kaynak açlığı nedeniyle performansınız etkilenmiş olabilir. Büyüyen ve azalan iş yükü taleplerine bağlı olarak işlem boyutunu ve/veya hizmet katmanını değiştirmeniz gerekebilir.

Performans sorunlarını ayarlamak için kapsamlı bir öneri kümesi için bkz: [Veritabanınızı ayarlayın.](sql-database-performance-guidance.md#tune-your-database)

### <a name="how-do-i-ensure-i-am-using-the-appropriate-service-tier-and-compute-size"></a>Uygun hizmet katmanını ve işlem boyutunu kullandığımdan nasıl emin olabilirim?

SQL Veritabanı, Temel, Standart ve Premium olmak üzere çeşitli hizmet katmanları sunar. Her hizmet katmanı, o hizmet katmanına bağlı olarak garantili öngörülebilir bir performans elde elabilirsiniz. İş yükünüze bağlı olarak, kaynak kullanımınızın bulunduğunuz geçerli bilgi işlem boyutunun tavanına çarpabileceği etkinlik patlamaları olabilir. Bu gibi durumlarda, öncelikle herhangi bir alemenin yardımcı olup olmadığını değerlendirerek başlamak yararlıdır (örneğin, bir dizin ekleme veya değiştirme vb.). Hala sınır sorunlarıyla karşılaşırsanız, daha yüksek bir hizmet katmanına veya işlem boyutuna taşınmayı düşünün.

|**Hizmet katmanı**|**Yaygın Kullanım Örneği Senaryoları**|
|---|---|
|**Temel**|Bir avuç kullanıcıya ve yüksek eşzamanlılık, ölçek ve performans gereksinimlerine sahip olmayan bir veritabanına sahip uygulamalar. |
|**Standart**|Önemli bir eşzamanlılık, ölçek ve performans gereksinimleri ne kadar düşük ve orta IO talepleri ile birleştiğinde uygulamalar. |
|**Premium**|Çok sayıda eşzamanlı kullanıcı, yüksek CPU/bellek ve yüksek IO talebi olan uygulamalar. Yüksek eşzamanlılık, yüksek iş seviyesi ve gecikmeye duyarlı uygulamalar Premium düzeyinden yararlanabilir. |
|||

Doğru bilgi işlem boyutunda olduğunuzdan emin olmak için, "SQL Veritabanı'nda performans ve kaynak kullanımını nasıl izleyebilirim" adlı yöntemle sorgunuzu ve veritabanı kaynak tüketiminizi izleyebilirsiniz. Sorgularınızın/veritabanlarınızın CPU/Memory vb. üzerinde sürekli olarak sıcak çalıştığını fark ederseniz, daha yüksek bir bilgi işlem boyutuna kadar ölçeklemeyi düşünebilirsiniz. Benzer şekilde, yoğun saatlerde bile kaynakları çok fazla kullanmadığınızı fark ederseniz; geçerli işlem boyutundan aşağı ölçekleme düşünün.

SaaS uygulama denizmesi veya veritabanı birleştirme senaryonuz varsa, maliyet optimizasyonu için Elastik bir havuz kullanmayı düşünün. Elastik havuz veritabanı konsolidasyonu ve maliyet optimizasyonu elde etmek için harika bir yoldur. Elastik havuzu kullanarak birden çok veritabanını yönetme hakkında daha fazla bilgi için bkz: [Havuzları ve veritabanlarını yönetin.](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases)

### <a name="how-often-do-i-need-to-run-database-integrity-checks-for-my-database"></a>Veritabanım için veritabanı bütünlüğü denetimlerini ne sıklıkta çalıştırmam gerekir?

SQL Veritabanı, belirli veri bozulması sınıflarını otomatik olarak ve herhangi bir veri kaybı olmadan işlemesine olanak tanıyan bazı akıllı teknikler kullanır. Bu teknikler hizmete dahil edilir ve gerektiğinde hizmet tarafından yararlanılır. Düzenli olarak, hizmet genelinde veritabanı yedeklemeleri bunları geri ve üzerinde DBCC CHECKDB çalıştırarak sınanılır. Sorunlar varsa, SQL Veritabanı bunları proaktif olarak giderir. [Otomatik sayfa onarımı](/sql/sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring) bozuk veya veri bütünlüğü sorunları olan sayfaları düzeltmek için kullanılır. Veritabanı sayfaları her zaman sayfanın bütünlüğünü doğrulayan varsayılan CHECKSUM ayarı ile doğrulanır. SQL Veritabanı, veritabanınızın veri bütünlüğünü proaktif olarak izler ve inceler ve sorunlar ortaya çıkarsa bunları en yüksek önceliğe sahip olarak ele alar. Bunlara ek olarak, isteğe bağlı olarak kendi bütünlük denetimlerinizi istediğiniz zaman çalıştırmayı seçebilirsiniz.  Daha fazla bilgi için [SQL Veritabanı'nda Veri Bütünlüğü'ne](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/) bakın

## <a name="data-movement-after-migration"></a>Geçişten sonra veri hareketi

### <a name="how-do-i-export-and-import-data-as-bacpac-files-from-sql-database"></a>SQL Veritabanından BACPAC dosyaları olarak verileri nasıl dışa aktarıp aktarım?

- **Dışa Aktarma**: Azure SQL veritabanınızı Azure portalından BACPAC dosyası olarak dışa aktarabilirsiniz

   ![veritabanı dışa aktarma](./media/sql-database-export/database-export1.png)

- **Alma**: Azure portalını kullanarak veri tabanına BACPAC dosyası olarak da veri aktarabilirsiniz.

   ![veritabanı alma](./media/sql-database-import/import1.png)

### <a name="how-do-i-synchronize-data-between-sql-database-and-sql-server"></a>SQL Veritabanı ve SQL Server arasındaki verileri nasıl senkronize edebilirim?

Bunu başarmak için birkaç yolunuz vardır:

- **[Veri Eşitleme](sql-database-sync-data.md)** – Bu özellik, verileri birden çok şirket içi SQL Server veritabanları ve SQL Veritabanı arasında iki yönlü olarak eşitlemenize yardımcı olur. Şirket içi SQL Server veritabanlarıyla eşitlemek için, eşitleme aracısını yerel bir bilgisayara yüklemeniz ve yapılandırmanız ve giden TCP bağlantı noktası 1433'ün açılmasını zedebilirsiniz.
- **[İşlem Çoğaltma](https://azure.microsoft.com/blog/transactional-replication-to-azure-sql-database-is-now-generally-available/)** – İşlem çoğaltma ile verilerinizi şirket içinde Azure SQL DB'ye, şirket içinde yayıncı ve Azure SQL DB abone olarak senkronize edebilirsiniz. Şimdilik yalnızca bu kurulum desteklenir. Verilerinizi şirket içinde en az kapalı kalma süresiyle Azure SQL'e nasıl geçirebilirsiniz hakkında daha fazla bilgi için bkz: [İşlem Çoğaltma'yı kullanın](sql-database-single-database-migrate.md#method-2-use-transactional-replication)

## <a name="next-steps"></a>Sonraki adımlar

SQL [Veritabanı](sql-database-technical-overview.md)hakkında bilgi edinin.
