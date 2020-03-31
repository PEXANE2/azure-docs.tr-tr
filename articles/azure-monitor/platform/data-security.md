---
title: Log Analytics veri güvenliği | Microsoft Dokümanlar
description: Log Analytics'in gizliliğinizi nasıl koruduğu ve verilerinizi nasıl koruduğu hakkında bilgi edinin.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/04/2019
ms.openlocfilehash: 63d8d8d3701a9adca4bd01e6e061877f5d0bd245
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80333354"
---
# <a name="log-analytics-data-security"></a>Log Analytics veri güvenliği
Bu belge, [Azure Güven Merkezi'ndeki](../../security/fundamentals/trust-center.md)bilgileri tamamlamak için Azure Monitor'un bir özelliği olan Log Analytics'e özgü bilgiler sağlamayı amaçlamaktadır.  

Bu makalede verilerin Log Analytics tarafından nasıl toplandığı, işlendiği ve güvenlik altına alındığı açıklanır. Aracıları web hizmetine bağlanmak, operasyonel verileri toplamak için System Center Operations Manager'ı kullanmak veya Log Analytics tarafından kullanılmak üzere Azure tanılama larından veri almak için aracılar kullanabilirsiniz. 

Log Analytics hizmeti, bulut tabanlı verilerinizi aşağıdaki yöntemleri kullanarak güvenli bir şekilde yönetir:

* Veri ayırma
* Veri saklama
* Fiziksel güvenlik
* Olay yönetimi
* Uyumluluk
* Güvenlik standartları sertifikaları

[Azure destek seçeneklerindeki](https://azure.microsoft.com/support/options/)güvenlik ilkelerimiz de dahil olmak üzere aşağıdaki bilgilerden herhangi biriyle ilgili sorularınız, önerileriniz veya sorunlarla ilgili olarak bizimle iletişime geçin.

## <a name="sending-data-securely-using-tls-12"></a>TLS 1.2 kullanarak güvenli bir şekilde veri gönderme 

Log Analytics'e aktarılan verilerin güvenliğini sağlamak için, aracıyı en az Taşıma Katmanı Güvenliği (TLS) 1.2 kullanacak şekilde yapılandırmanızı önemle öneririz. TLS/Secure Sockets Layer'ın (SSL) eski sürümlerinin savunmasız olduğu tespit edilmiştir ve hala geriye dönük uyumluluğa izin vermek için çalışırken, bu sürümler **önerilmez**ve endüstri bu eski protokoller için desteği bırakmak için hızla hareket etmektedir. 

[PCI Güvenlik Standartları Konseyi,](https://www.pcisecuritystandards.org/) TLS/SSL'nin eski sürümlerini devre dışı bırakıp daha güvenli protokollere yükseltmek için [30 Haziran 2018 tarihine kadar süre](https://www.pcisecuritystandards.org/pdfs/PCI_SSC_Migrating_from_SSL_and_Early_TLS_Resource_Guide.pdf) verdi. Azure eski desteği bıraktığında, aracılarınız en az TLS 1.2 üzerinden iletişim kuramazsa, Log Analytics'e veri gönderemezsiniz. 

Aracınızı, platform düzeyindeki güvenlik özelliklerini otomatik olarak algılayabiliyor ve kullanılabilir olduklarında daha güvenli protokollerden yararlanabiliyor, kesinlikle gerekli olmadıkça yalnızca TLS 1.2'yi kullanacak şekilde ayarlamanızı açıkça önermiyoruz. TLS 1.3 olarak. 

### <a name="platform-specific-guidance"></a>Platforma özel rehberlik

|Platform / Dil | Destek | Daha Fazla Bilgi |
| --- | --- | --- |
|Linux | Linux dağıtımları TLS 1.2 desteği için [OpenSSL'ye](https://www.openssl.org) güvenme eğilimindedir.  | OpenSSL sürümünüzün desteklenir olduğunu doğrulamak için [OpenSSL Changelog'u](https://www.openssl.org/news/changelog.html) kontrol edin.|
| Windows 8.0 - 10 arası | Desteklenen ve varsayılan olarak etkinleştirildi. | [Varsayılan ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings)hala kullandığınızı doğrulamak için.  |
| Windows Server 2012 - 2016 | Desteklenen ve varsayılan olarak etkinleştirildi. | [Varsayılan ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) hala kullandığınızı doğrulamak için |
| Windows 7 SP1 ve Windows Server 2008 R2 SP1 | Desteklenen, ancak varsayılan olarak etkin değil. | Nasıl etkinleştirilene ilişkin ayrıntılar için [Aktarım Katmanı Güvenliği (TLS) kayıt defteri ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) sayfasına bakın.  |

## <a name="data-segregation"></a>Veri ayırma
Verileriniz Log Analytics hizmeti tarafından alındıktan sonra, veriler hizmet boyunca her bileşende mantıksal olarak ayrı tutulur. Tüm veriler çalışma alanı başına etiketlenir. Bu etiketleme, veri yaşam döngüsü boyunca devam eder ve her bir hizmet katmanında uygulanır. Verileriniz seçtiğiniz bölgedeki depolama kümesinde özel bir veritabanında depolanır.

## <a name="data-retention"></a>Veri saklama
Dizine eklenmiş günlük arama verileri fiyatlandırma planınıza göre depolanır ve saklanır. Daha fazla bilgi için Bkz. [Log Analytics Fiyatlandırması.](https://azure.microsoft.com/pricing/details/log-analytics/)

[Abonelik sözleşmenizin](https://azure.microsoft.com/support/legal/subscription-agreement/)bir parçası olarak, Microsoft verilerinizi sözleşme koşullarına göre saklar.  Müşteri verileri kaldırıldığında, hiçbir fiziksel sürücü yok olmaz.  

Aşağıdaki tablo, kullanılabilir çözümlerden bazılarını listeler ve topladıkları veri türüne örnekler sağlar.

| **Çözüm** | **Veri türleri** |
| --- | --- |
| Kapasite ve Performans |Performans verileri ve meta veriler |
| Güncelleştirme Yönetimi |Meta veriler ve durum verileri |
| Günlük Yönetimi |Kullanıcı tanımlı olay günlükleri, Windows Olay Günlükleri ve/veya IIS Günlükleri |
| Değişiklik İzleme |Yazılım envanteri, Windows hizmeti ve Linux daemon meta verileri ve Windows/Linux dosya meta verileri |
| SQL ve Aktif Dizin Değerlendirmesi |WMI verileri, kayıt defteri verileri, performans verileri ve SQL Server dinamik yönetim görünümü sonuçları |

Aşağıdaki tabloda veri türlerinin örnekleri gösterilmektedir:

| **Veri türü** | **Alanlar** |
| --- | --- |
| Uyarı |Uyarı Adı, Uyarı Açıklaması, BaseManagedEntityId, Problem Kimliği, IsMonitorAlert, RuleId, ResolutionState, Öncelik, Önem, Kategori, Sahibi, ResolvedBy, TimeRaised, TimeAdded, LastModi, LastModifiedBy, LastModifiedExceptRepeatCount, TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Yapılandırma |CustomerID, AgentID, EntityID, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate |
| Olay |EventId, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, Number, Category, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Not:** Windows etkinlik günlüğüne özel alanlara sahip etkinlikler yazdığınızda, Log Analytics bunları toplar. |
| Meta Veriler |BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IPAddress, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP Adres, NetbiosDomainName, LogicalProcessors, DNSName, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Performans |ObjectName, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Durum |StatechangeEventid, stateid, newhealthstate, oldhealthstate, bağlam, timegenerated, timeadded, stateid2, basemanagedentityid, monitorid, healthstate, lastmodified, lastgreenalertgenerated, databasetimemodified |

## <a name="physical-security"></a>Fiziksel güvenlik
Log Analytics hizmeti Microsoft personeli tarafından yönetilir ve tüm etkinlikler günlüğe kaydedilir ve denetlenebilir. Log Analytics bir Azure Hizmeti olarak işletilmektedir ve tüm Azure Uyumluluk ve Güvenlik gereksinimlerini karşılar. [Microsoft Azure Güvenlik Genel Bakış](https://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf)sayfa 18'de Azure varlıklarının fiziksel güvenliği yle ilgili ayrıntıları görüntüleyebilirsiniz. Güvenli alanlara fiziksel erişim hakları, transfer ve fesih de dahil olmak üzere Log Analytics hizmetinden artık sorumlu olmayan herkes için bir iş günü içinde değiştirilir. [Microsoft Datacenters'da](https://azure.microsoft.com/global-infrastructure/)kullandığımız küresel fiziksel altyapı hakkında bilgi edinebilirsiniz.

## <a name="incident-management"></a>Olay yönetimi
Log Analytics, tüm Microsoft hizmetlerinin bağlı olduğu bir olay yönetimi işlemine sahiptir. Özetlemek gerekirse, biz:

* Güvenlik sorumluluğunun bir kısmının Microsoft'a, bir kısmının müşteriye ait olduğu paylaşılan bir sorumluluk modelini kullanma
* Azure güvenlik olaylarını yönetme:
  * Bir olayın algılanması üzerine soruşturma başlatın
  * Bir olaydaki olay müdahale ekibi üyesi tarafından bir olayın etkisini ve şiddetini değerlendirin. Kanıtlara dayanarak, değerlendirme güvenlik müdahale ekibinin daha da yükselmesine neden olabilir veya olmayabilir.
  * Teknik veya adli soruşturmayı yürütmek, çevreleme, azaltma ve geçici çözüm stratejilerini belirlemek için güvenlik müdahale uzmanları tarafından bir olayı tanılayın. Güvenlik ekibi müşteri verilerinin yasadışı veya yetkisiz bir kişiye maruz kalmış olabileceğine inanıyorsa, Müşteri Olayı Bildirimi işleminin paralel yürütülmesi paralel olarak başlar.  
  * Olayı stabilize edin ve atlatın. Olay yanıt ekibi, sorunu azaltmak için bir kurtarma planı oluşturur. Etkilenen sistemlerin karantinaya alınması gibi kriz önleme adımları hemen ve tanıya paralel olarak ortaya çıkabilir. Acil risk geçtikten sonra ortaya çıkan uzun vadeli azaltıcı etkenler planlanabilir.  
  * Olayı kapatın ve otopsi yap. Olay müdahale ekibi, olayın tekrarını önlemek için politikaları, prosedürleri ve işlemleri gözden geçirmek amacıyla olayın ayrıntılarını özetleyen bir otopsi oluşturur.
* Müşterilere güvenlik olaylarını bildirin:
  * Etkilenen müşterilerin kapsamını belirlemek ve etkilenen herkese mümkün olduğunca ayrıntılı bir bildirim de sunmak
  * Müşterilere, kendi başlarına bir araştırma gerçekleştirebilmeleri ve bildirim işlemini gereksiz yere geciktirmeden son kullanıcılarına verdikleri taahhütleri yerine getirebilmeleri için yeterli ayrıntılı bilgi sağlamak için bir bildirim oluşturun.
  * Gerektiğinde olayı doğrulayın ve bildirin.
  * Makul olmayan bir gecikme olmaksızın ve herhangi bir yasal veya sözleşme taahhüdüuyarınca bir olay bildirimi ile müşterilerine bildirin. Güvenlik olayları yla ilgili bildirimler, Microsoft'un e-posta yoluyla da dahil olmak üzere seçtiği herhangi bir yolla bir veya daha fazla müşteri yöneticisine teslim edilir.
* Ekip hazırlığı ve eğitimini gerçekleştirin:
  * Microsoft personelinin, şüpheli güvenlik sorunlarını belirlemelerine ve bildirmelerine yardımcı olan güvenlik ve bilinirlik eğitimini tamamlaması gerekir.  
  * Microsoft Azure hizmetinde çalışan operatörler, müşteri verilerini barındıran hassas sistemlere erişimlerini çevreleyen ek eğitim yükümlülüklerine sahiptir.
  * Microsoft güvenlik yanıt personeli rolleri için özel eğitim alır

Herhangi bir müşteri verisi kaybı meydana gelirse, her müşteriye bir gün içinde bildirimde bulunacağız. Ancak, müşteri veri kaybı hizmet ile oluştu asla. 

Microsoft'un güvenlik olaylarına nasıl tepki verdiği hakkında daha fazla bilgi için [Bulut'ta Microsoft Azure Güvenlik Yanıtı'na](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/4/Microsoft%20Azure%20Security%20Response%20in%20the%20cloud.pdf)bakın.

## <a name="compliance"></a>Uyumluluk
Log Analytics yazılım geliştirme ve hizmet ekibinin bilgi güvenliği ve yönetim programı iş gereksinimlerini destekler ve [Microsoft Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/) ve Microsoft Güven Merkezi [Uyumluluğu'nda](https://www.microsoft.com/en-us/trustcenter/compliance/default.aspx)açıklandığı şekilde yasalara ve düzenlemelere uyar. Log Analytics'in güvenlik gereksinimlerini nasıl oluşturduğu, güvenlik denetimlerini nasıl tanımladığı, riskleri nasıl yönettiği ve riskleri nasıl izlediği de burada açıklanmıştır. Her yıl, polisleri, standartları, prosedürleri ve yönergeleri gözden geçiriyoruz.

Her geliştirme ekibi üyesi resmi uygulama güvenliği eğitimi alır. Dahili olarak, yazılım geliştirme için bir sürüm kontrol sistemi kullanırız. Her yazılım projesi sürüm kontrol sistemi tarafından korunmaktadır.

Microsoft'un, Microsoft'taki tüm hizmetleri denetleyen ve değerlendiren bir güvenlik ve uyumluluk ekibi vardır. Ekibi bilgi güvenliği görevlileri yapar ve Log Analytics'i geliştiren mühendislik ekipleriyle ilişkili değildir. Güvenlik görevlilerinin kendi yönetim zincirleri vardır ve güvenlik ve uyumu sağlamak için ürün ve hizmetlerin bağımsız değerlendirmeleri yapar.

Microsoft'un yönetim kurulu, Microsoft'taki tüm bilgi güvenliği programları hakkında yıllık bir rapor la bildirilir.

Log Analytics yazılım geliştirme ve hizmet ekibi, çeşitli sertifikalar almak için Microsoft Yasal ve Uyumluluk ekipleri ve diğer endüstri ortaklarıile aktif olarak çalışmaktadır.

## <a name="certifications-and-attestations"></a>Sertifikalar ve attestation
Azure Log Analytics aşağıdaki gereksinimleri karşılar:

* [ISO/IEC 27001](https://www.iso.org/iso/home/standards/management-standards/iso27001.htm)
* [ISO/IEC 27018:2014](https://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498)
* [ISO 22301](https://azure.microsoft.com/blog/iso22301/)
* PCI Güvenlik Standartları Konseyi tarafından [Ödeme Kartı Endüstrisi (PCI Uyumlu) Veri Güvenliği Standardı (PCI DSS).](https://www.microsoft.com/en-us/TrustCenter/Compliance/PCI)
* [Servis Organizasyonu Denetimleri (SOC) 1 Tip 1 ve SOC 2 Tip 1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) uyumlu
* HIPAA İş Ortağı Sözleşmesi olan şirketler için [HIPAA ve HITECH](https://www.microsoft.com/en-us/TrustCenter/Compliance/hipaa)
* Windows Ortak Mühendislik Kriterleri
* Microsoft Güvenilir Bilgi İşlem
* Azure hizmeti olarak, Log Analytics'in kullandığı bileşenler Azure uyumluluk gereksinimlerine bağlıdır. [Microsoft Trust Center Compliance](https://www.microsoft.com/en-us/trustcenter/compliance/default.aspx)'da daha fazla bilgi edinebilirsiniz.

> [!NOTE]
> Bazı sertifikalarda/attestation'larda Log Analytics, eski *Operasyonel Öngörüler*adı altında listelenmiştir.
>
>

## <a name="cloud-computing-security-data-flow"></a>Bulut bilgi işlem güvenliği veri akışı
Aşağıdaki diyagram, şirketinizden gelen bilgi akışı olarak bir bulut güvenlik mimarisini ve azure portalında sizin gördüğünüz Log Analytics hizmetine taşınırken nasıl güvence altına alınır gibi güvenli olduğunu gösterir. Her adım hakkında daha fazla bilgi diyagramı izler.

![Log Analytics veri toplama ve güvenlik görüntüsü](./media/data-security/log-analytics-data-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. Log Analytics'e kaydolun ve veri toplayın
Kuruluşunuzun Log Analytics'e veri göndermesi için, Azure sanal makinelerinde veya ortamınızdaki veya diğer bulut sağlayıcınızdaki sanal veya fiziksel bilgisayarlarda çalışan bir Windows veya Linux aracısı yapılandırabilirsiniz.  Operations Manager'ı kullanırsanız, yönetim grubundan Operations Manager aracısını yapılandırabilirsiniz. Kullanıcılar (siz, diğer bireysel kullanıcılar veya bir grup kişi) bir veya daha fazla Log Analytics çalışma alanı oluşturur ve aşağıdaki hesaplardan birini kullanarak aracıları kaydeder:

* [Kuruluş Kimliği](../../active-directory/fundamentals/sign-up-organization.md)
* [Microsoft Account - Outlook, Office Live, MSN](https://account.microsoft.com/account)

Günlük Analizi çalışma alanı, verilerin toplandığı, toplandığı, analiz edildiği ve sunulduğu yerdir. Çalışma alanı öncelikle verileri bölmek için bir araç olarak kullanılır ve her çalışma alanı benzersizdir. Örneğin, üretim verilerinizin bir çalışma alanıyla, test verilerinizin de başka bir çalışma alanıyla yönetilmesini isteyebilirsiniz. Çalışma alanları, yöneticinin verilere kullanıcı erişimini denetlemesine de yardımcı olur. Her çalışma alanıyla ilişkili birden çok kullanıcı hesabı olabilir ve her kullanıcı hesabı birden çok Log Analytics çalışma alanına erişebilir. Veri merkezi bölgesini temel alan çalışma alanları oluşturursunuz.

Operasyon Yöneticisi için, Operasyon Yöneticisi yönetim grubu Log Analytics hizmetiyle bir bağlantı kurar. Daha sonra, yönetim grubundaki hangi aracı tarafından yönetilen sistemlerin hizmete veri toplamasına ve göndermesine izin verildiğini yapılandırırsınız. Etkinleştirdiğiniz çözüme bağlı olarak, bu çözümlerden elde edilen veriler doğrudan bir Operasyon Yöneticisi yönetim sunucusundan Log Analytics hizmetine gönderilir veya aracı tarafından yönetilen sistem tarafından toplanan veri hacmi nedeniyle doğrudan hizmete aracı. Operations Manager tarafından izlenmeyen sistemler için her biri doğrudan Log Analytics hizmetine güvenli bir şekilde bağlanır.

Bağlı sistemler ve Log Analytics hizmeti arasındaki tüm iletişim şifrelenir. TLS (HTTPS) protokolü şifreleme için kullanılır.  Log Analytics'in şifreleme protokollerinde en son gelişmelerden haberdar olduğundan emin olmak için Microsoft SDL işlemi izlenir.

Her tür aracı, Log Analytics için veri toplar. Toplanan veri türü kullanılan çözüm türlerine bağlıdır. [Çözüm Galerisi'nden Veri Toplama İşlemi Çözümlerini Ekle çözümlerinde](../../azure-monitor/insights/solutions.md)veri toplamanın bir özetini görebilirsiniz. Ayrıca, çoğu çözüm için daha ayrıntılı koleksiyon bilgileri mevcuttur. Çözüm, önceden tanımlanmış görünümler, günlük arama sorguları, veri toplama kuralları ve işleme mantığından oluşan bir pakettir. Bir çözümü almak için Log Analytics'i yalnızca yöneticiler kullanabilir. Çözüm alındıktan sonra, Operasyon Yöneticisi yönetim sunucularına (kullanılırsa) ve sonra seçtiğiniz aracılara taşınır. Daha sonra, aracılar verileri toplar.

## <a name="2-send-data-from-agents"></a>2. Aracılardan veri gönderme
Tüm aracı türlerini bir kayıt anahtarıyla kaydedersiniz ve temsilci ile Log Analytics hizmeti arasında sertifika tabanlı kimlik doğrulaması ve 443 portlu TLS kullanılarak güvenli bir bağlantı kurulur. Log Analytics, anahtarları oluşturmak ve korumak için gizli bir mağaza kullanır. Özel anahtarlar her 90 günde bir döndürülür ve Azure'da depolanır ve katı düzenleme ve uyumluluk uygulamalarını izleyen Azure işlemleri tarafından yönetilir.

Operations Manager ile, Log Analytics çalışma alanına kayıtlı yönetim grubu, Bir İşlem Yöneticisi yönetim sunucusuyla güvenli bir HTTPS bağlantısı kurar.

Azure sanal makinelerinde çalışan Windows veya Linux aracıları için, Azure tablolarında tanılama olaylarını okumak için salt okunur depolama anahtarı kullanılır.  

Log Analytics ile entegre olan bir Operasyon Yöneticisi yönetim grubuna rapor veren herhangi bir aracı, yönetim sunucusu herhangi bir nedenle hizmetle iletişim kuramıyorsa, toplanan veriler yönetim üzerinde geçici bir önbellekte yerel olarak depolanır Sunucu.   İki saat boyunca her sekiz dakikada bir verileri yeniden göndermeye çalışıyorlar.  Yönetim sunucusunu atlayan ve doğrudan Log Analytics'e gönderilen veriler için davranış, Windows aracısıyla tutarlıdır.  

Önbelleğe alınan Windows veya yönetim sunucusu aracısı, işletim sisteminin kimlik bilgisi deposu tarafından korunur. Hizmet iki saat sonra verileri işleyemezse, aracılar verileri sıraya alır. Sıra dolduğunda, aracı performans verilerinden başlayarak veri türlerini düşürmeye başlar. Aracı sıra sınırı, gerekirse değiştirebilmeniz için bir kayıt defteri anahtarıdır. Toplanan veriler sıkıştırılır ve Operasyon Yöneticisi yönetim grubu veritabanlarıatılarak hizmete gönderilir, bu nedenle bunlara herhangi bir yük eklemez. Toplanan veriler gönderildikten sonra önbellekten kaldırılır.

Yukarıda açıklandığı gibi, yönetim sunucusundan veya doğrudan bağlı aracılardan gelen veriler TLS üzerinden Microsoft Azure veri merkezlerine gönderilir. İsteğe bağlı olarak, veriler için ek güvenlik sağlamak için ExpressRoute'u kullanabilirsiniz. ExpressRoute, bir ağ servis sağlayıcısı tarafından sağlanan çok protokoletiket anahtarlama (MPLS) VPN gibi mevcut WAN ağınızdan Azure'a doğrudan bağlanmanın bir yoludur. Daha fazla bilgi için [ExpressRoute'a](https://azure.microsoft.com/services/expressroute/)bakın.

## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. Log Analytics hizmeti veri alır ve işler
Log Analytics hizmeti, sertifikaları ve veri bütünlüğünü Azure kimlik doğrulamasıyla doğrulayarak gelen verilerin güvenilir bir kaynaktan gelmesini sağlar. İşlenmemiş ham veriler daha sonra bölgedeki bir Azure Etkinlik Hub'ında depolanır ve veriler sonunda istirahatte depolanır. Depolanan veri türü, veri toplamak için alınan ve kullanılan çözüm türlerine bağlıdır. Daha sonra, Log Analytics hizmeti ham verileri işler ve veritabanına alır.

Veritabanında depolanan toplanan verilerin bekletme süresi, seçili fiyatlandırma planına bağlıdır. *Ücretsiz* katman için toplanan veriler yedi gün boyunca kullanılabilir. *Ücretli* katman için toplanan veriler varsayılan olarak 31 gün kullanılabilir, ancak 730 güne uzatılabilir. Veriler, veri gizliliğini sağlamak için Azure depolama alanında şifrelenir ve veriler yerel olarak yedekli depolama (LRS) kullanılarak yerel bölgede çoğaltılır. Son iki haftalık veriler DeSD tabanlı önbellekte depolanır ve bu önbellek şifrelenir.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. Verilere erişmek için Log Analytics'i kullanın
Log Analytics çalışma alanınıza erişmek için, daha önce ayarladığınız kuruluş hesabını veya Microsoft hesabını kullanarak Azure portalında oturum açarsınız. Portal ve Log Analytics hizmeti arasındaki tüm trafik güvenli bir HTTPS kanalı üzerinden gönderilir. Portalı kullanırken, kullanıcı istemcisinde (web tarayıcısı) bir oturum kimliği oluşturulur ve oturum sonlandırılana kadar veriler yerel bir önbellekte depolanır. Sonlandırıldığında önbellek silinir. Kişisel olarak tanımlayıcı bilgiler içermeyen istemci tarafı çerezleri otomatik olarak kaldırılmaz. Oturum tanımlama bilgileri HTTPOnly olarak işaretlenir ve güvenli hale alınır. Önceden belirlenmiş bir boşta kalan dönemden sonra Azure portal oturumu sonlandırılır.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure VM hızlı başlatılmasını](../../azure-monitor/learn/quick-collect-azurevm.md)takiben Azure Sanal Tom'larınız için Günlük Analizi ile nasıl veri topladığınızı öğrenin.  

*  Ortamınızdaki fiziksel veya sanal Windows veya Linux bilgisayarlarından veri toplamak istiyorsanız, [Linux bilgisayarlar için Hızlı Başlatma](../../azure-monitor/learn/quick-collect-linux-computer.md) veya Windows bilgisayarlar için Hızlı [Başlatma'ya](../../azure-monitor/learn/quick-collect-windows-computer.md) bakın

