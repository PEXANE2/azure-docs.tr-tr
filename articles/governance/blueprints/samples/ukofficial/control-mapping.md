---
title: İngiltere'de RESMI & İngiltere NHS planı örnek kontrolleri
description: İngiltere RESMİ ve İngiltere NHS plan örneklerinin kontrol haritalama. Her denetim, değerlendirmeye yardımcı olan bir veya daha fazla Azure İlkesiyle eşlenir.
ms.date: 12/04/2019
ms.topic: sample
ms.openlocfilehash: 5bef590013a9ef06b791e58dc6c82e74dffe1a17
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74851375"
---
# <a name="control-mapping-of-the-uk-official-and-uk-nhs-blueprint-samples"></a>İngiltere RESMİ ve İngiltere NHS plan örneklerinin kontrol haritalama

Aşağıdaki makalede, Birleşik Krallık RESMİ ve İngiltere NHS planı örneklerinin Birleşik Krallık RESMİ ve İngiltere NHS kontrollerine nasıl harita landığı ayrıntılı olarak açıklanmıştır. Denetimler hakkında daha fazla bilgi [için, İngiltere RESMİ](https://www.gov.uk/government/publications/government-security-classifications)bakın.

Aşağıdaki **haritalamalar İngiltere RESMİ** ve **İngiltere NHS** kontrolleri içindir. Doğrudan belirli bir denetim eşleleme sine atlamak için sağdaki gezinmeyi kullanın. Eşlenen denetimlerin çoğu bir Azure [İlkesi](../../../policy/overview.md) girişimiyle uygulanır. Girişimin tamamını incelemek için Azure portalında **İlke'yi** açın ve **Tanımlar** sayfasını seçin. Ardından, ** \[Preview\] Audit UK OFFICIAL ve UK NHS denetimlerini** bulun ve seçin ve yerleşik politika girişimini desteklemek için belirli VM Uzantılarını dağıtın.

> [!IMPORTANT]
> Aşağıdaki her denetim bir veya daha fazla [Azure İlkesi](../../../policy/overview.md) tanımıyla ilişkilidir. Bu ilkeler, denetime [uygunluğu değerlendirmenize](../../../policy/how-to/get-compliance-data.md) yardımcı olabilir; ancak, genellikle bir denetim ve bir veya daha fazla ilke arasında 1:1 veya tam bir eşleşme yoktur. Bu nedenle, Azure İlkesinde **Uyumlu olmak** yalnızca ilkelerin kendileri için başvurur; bu, bir denetimin tüm gereksinimlerine tam olarak uymanızı sağlamaz. Ayrıca, uyumluluk standardı şu anda herhangi bir Azure İlkesi tanımı tarafından ele alınmayan denetimleri içerir. Bu nedenle, Azure İlkesi'ndeki uyumluluk, genel uyumluluk durumunuza yalnızca kısmi bir görünümdür. Bu uyumluluk planı örneği için denetimler ve Azure İlkesi tanımları arasındaki ilişkilendirmeler zaman içinde değişebilir. Değişiklik geçmişini görüntülemek için [GitHub Commit History'ye](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/ukofficial/control-mapping.md)bakın.

## <a name="1-data-in-transit-protection"></a>1 Transit koruma verileri

Plan, depolama hesaplarına ve Redis Önbelleğine güvenli olmayan bağlantıları denetleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak Azure hizmetleriyle bilgi aktarımının güvenli olmasını sağlamanıza yardımcı olur.

- Yalnızca Redis Önbelleğinize güvenli bağlantılar etkinleştirilmelidir
- Depolama hesaplarına güvenli aktarım etkinleştirilmelidir
- Güvenli iletişim protokolleri kullanmayan Windows web sunucularından gelen denetim sonuçlarını göster
- Güvenli iletişim protokolleri kullanmayan Windows web sunucularını denetlemek için ön koşulları dağıtma
- API Uygulamanızda en son TLS sürümü kullanılmalıdır
- Web Uygulamanızda en son TLS sürümü kullanılmalıdır
- Fonksiyon Uygulamanızda en son TLS sürümü kullanılmalıdır

## <a name="23-data-at-rest-protection"></a>2.3 Dinlenme korumasındaki veriler

Bu plan, belirli şifreleme denetimlerini zorlayan [Azure İlkesi](../../../policy/overview.md) tanımları atayarak ve zayıf şifreleme ayarlarının denetim kullanımını kullanarak şifreleme denetimlerinin kullanımına ilişkin ilkenizi uygulamanıza yardımcı olur.
Azure kaynaklarınızın en iyi olmayan şifreleme yapılandırmalarına sahip olabileceğini anlamak, kaynakların bilgi güvenliği politikanıza uygun şekilde yapılandırıldığından emin olmak için düzeltici eylemlerde yardımcı olabilir. Özellikle, bu plan tarafından atanan ilkeler veri gölü depolama hesapları için şifreleme gerektirir; SQL veritabanlarında saydam veri şifreleme gerektirir; depolama hesapları, SQL veritabanları, sanal makine diskleri ve otomasyon hesabı değişkenlerinde eksik şifrelemeyi denetlemek; depolama hesaplarına ve Redis Cache'ye güvenli olmayan bağlantıları denetlemek; denetim zayıf sanal makine şifre şifreleme; ve şifrelenmemiş Hizmet Kumaşı iletişimini denetleyin.

- Disk şifreleme sanal makinelerde uygulanmalıdır
- Otomasyon hesabı değişkenleri şifrelenmelidir
- Service Fabric kümeleri ClusterProtectionLevel özelliğini EncryptAndSign olarak ayarlamalıdır
- SQL veritabanlarında Saydam Veri Şifrelemesi etkinleştirilmelidir
- SQL DB saydam veri şifrelemesini dağıtma
- Data Lake Store hesaplarında şifreleme gerektirme
- İzin verilen konumlar ("UK SOUTH" ve "UK WEST" olarak kodlanmıştır)
- Kaynak grupları için izin verilen konumlar ("UK SOUTH" ve "UK WEST" olarak kodlanmıştır)

## <a name="52-vulnerability-management"></a>5.2 Güvenlik Açığı Yönetimi

Bu plan, eksik uç nokta korumayı, eksik sistem güncelleştirmelerini, işletim sistemi güvenlik açıklarını, SQL güvenlik açıklarını ve sanal makine güvenlik açıklarını izleyen [Azure İlkesi](../../../policy/overview.md) tanımlarını atayarak bilgi sistemi güvenlik açıklarını yönetmenize yardımcı olur. Bu öngörüler, dağıtılmış kaynaklarınızın güvenlik durumu hakkında gerçek zamanlı bilgiler sağlar ve düzeltme eylemlerine öncelik vermenize yardımcı olabilir.

- Azure Güvenlik Merkezi'nde eksik Uç Nokta Koruması'nı izleme
- Makinelerinize sistem güncelleştirmeleri yüklenmelidir
- Sanal makine ölçek kümelerinde sistem güncellemeleri yüklenmelidir
- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir
- Güvenlik açıkları, Güvenlik Açığı Değerlendirmesi çözümü yle düzeltilmelidir
- SQL sunucularınızda güvenlik açığı değerlendirmesi etkinleştirilmelidir
- SQL yönetilen örneklerinizde güvenlik açığı değerlendirmesi etkinleştirilmelidir
- Sanal makine ölçeği kümelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- SQL yönetilen örneklerinizde gelişmiş veri güvenliği etkinleştirilmelidir
- SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir

## <a name="53-protective-monitoring"></a>5.3 Koruyucu İzleme

Bu plan, sınırsız erişimde koruyucu izleme sağlayan, liste etkinliğine ve tehditlere izin veren [Azure İlkesi](../../../policy/overview.md) tanımları atayarak bilgi sistemi varlıklarını korumanıza yardımcı olur.

- Depolama hesaplarına sınırsız ağ erişimini denetleme
- Uyarlanabilir Uygulama Denetimleri sanal makinelerde etkinleştirilmelidir
- Olağanüstü durum kurtarma yapılandırması yapılmadan sanal makineleri denetleme
- DDoS Koruma Standardı etkinleştirilmelidir
- Gelişmiş Tehdit Koruması türleri SQL yönetilen örnek Gelişmiş Veri Güvenliği ayarlarında 'Tümü' olarak ayarlanmalıdır
- Gelişmiş Tehdit Koruması türleri SQL sunucusu Gelişmiş Veri Güvenliği ayarlarında 'Tümü' olarak ayarlanmalıdır
- SQL sunucularında Tehdit Algılamayı Dağıtma
- Windows Server için varsayılan Microsoft IaaaSAntimalware uzantısı dağıtma

## <a name="9-secure-user-management"></a>9 Güvenli Kullanıcı Yönetimi 

Azure, Azure'daki kaynaklara kimlerin erişebildiğinizi yönetmenize yardımcı olmak için rol tabanlı erişim denetimi (RBAC) uygular. Azure portalını kullanarak, Azure kaynaklarına ve izinlerine kimlerin erişimi olduğunu inceleyebilirsiniz. Bu plan, azure [ilkesi](../../../policy/overview.md) tanımlarını, sahibi ve/veya okuma/yazma izinleri ve çok faktörlü kimlik doğrulaması etkin olmayan sahibi, okuma ve/yazma izinleri olan dış hesapları denetlemek için atayarak erişim haklarını kısıtlamanıza ve denetlemenize yardımcı olur.

- MFA, aboneliğinizde sahip izni olan hesaplarda etkinleştirilmelidir
- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir
- MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Yazma izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Okuma izinleri olan harici hesaplar aboneliğinizden kaldırılmalıdır

## <a name="10-identity-and-authentication"></a>10 Kimlik ve Kimlik Doğrulama

Bu plan, azure [ilkesi](../../../policy/overview.md) tanımlarını, sahibi ve/veya okuma/yazma izinleri ve çok faktörlü kimlik doğrulaması etkin olmayan sahibi, okuma ve/yazma izinleri olan dış hesapları denetlemek için atayarak erişim haklarını kısıtlamanıza ve denetlemenize yardımcı olur.

- MFA, aboneliğinizde sahip izni olan hesaplarda etkinleştirilmelidir
- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir
- MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Yazma izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Okuma izinleri olan harici hesaplar aboneliğinizden kaldırılmalıdır

Bu plan, SQL sunucuları ve Hizmet Dokusu için Azure Active Directory kimlik doğrulamasını denetlemek için Azure İlkesi tanımlarını atar. Azure Active Directory kimlik doğrulaması kullanmak, veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin basitleştirilmiş izin yönetimini ve merkezi kimlik yönetimini sağlar.

- SQL sunucuları için bir Azure Active Directory yöneticisi sağlanmalıdır
- Hizmet Kumaş kümeleri yalnızca müşteri kimlik doğrulaması için Azure Etkin Dizini kullanmalıdır

Bu plan ayrıca, amortismana tabi hesaplar ve dış hesaplar da dahil olmak üzere gözden geçirilmesi için öncelik verilmesi gereken denetim hesaplarına Azure İlkesi tanımları atar. Gerektiğinde, hesapların oturum açması (veya kaldırılması) engellenebilir ve bu da Azure kaynaklarına erişim haklarını hemen kaldırır. Bu plan, amortismana tabi hesabı denetlemek için kaldırılması gereken iki Azure İlkesi tanımı atar.

- Amortismana alınan hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan amortismana alınmış hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Yazma izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

Bu plan ayrıca, yanlış ayarlanmışsa uyarmak için Linux VM parola dosyası izinlerini denetleyen bir Azure İlkesi tanımı da atar. Bu tasarım, kimlik doğrulayıcıların tehlikeye girmediğinden emin olmak için düzeltici eylemde bulunmanızı sağlar.

- \[Önizleme\]: 0644 olarak ayarlanmış passwd dosya izinleri olmayan Linux VM'lerinden gelen denetim sonuçlarını göster

Bu plan, minimum güç ve diğer parola gereksinimlerini zorlamayan Windows VM'lerini denetleyen Azure İlkesi tanımları atayarak güçlü parolalar uygulamanıza yardımcı olur. Parola gücü ilkesini ihlal eden VM'lerin farkında olmak, tüm VM kullanıcı hesaplarının parolalarının ilkeyle uyumlu olduğundan emin olmak için düzeltici eylemlerde yardımcı olur.

- \[Önizleme\]: Parola karmaşıklığı ayarı etkin olmayan Windows VM'lerini denetlemek için ön koşulları dağıtma
- \[Önizleme\]: En fazla 70 günlük parola yaşı olmayan Windows VM'lerini denetlemek için ön koşulları dağıtma
- \[Önizleme\]: Minimum parola yaşı 1 gün olmayan Windows VM'lerini denetlemek için ön koşulları dağıtma
- \[Önizleme\]: Minimum parola uzunluğunu 14 karakterle sınırlamayan Windows VM'lerini denetlemek için ön koşulları dağıtma
- \[Önizleme\]: Önceki 24 parolanın yeniden kullanılmasına izin veren Windows VM'lerini denetlemek için ön koşulları dağıtma
- \[Önizleme\]: Parola karmaşıklığı ayarı etkin olmayan Windows VM'lerinden gelen denetim sonuçlarını göster
- \[Önizleme\]: En fazla 70 günlük parola yaşı olmayan Windows VM'lerinden gelen denetim sonuçlarını göster
- \[Önizleme\]: En az parola yaşı 1 gün olmayan Windows VM'lerinden gelen denetim sonuçlarını göster
- \[Önizleme\]: En az parola uzunluğunu 14 karakterle sınırlamayan Windows VM'lerinden gelen denetim sonuçlarını göster
- \[Önizleme\]: Önceki 24 parolanın yeniden kullanılmasına izin veren Windows VM'lerinden gelen denetim sonuçlarını göster

Bu plan, Azure İlkesi tanımları atayarak Azure kaynaklarına erişimi denetlemenize de yardımcı olur. Bu ilkeler, kaynaklara daha fazla izin veren erişime izin verebilecek kaynak türlerinin ve yapılandırmaların kullanımını denetler. Bu ilkeleri ihlal eden kaynakları anlamak, Azure kaynaklarına erişimin yetkili kullanıcılarla sınırlı olduğundan emin olmak için düzeltici eylemlerde yardımcı olabilir.

- \[Önizleme\]: Parolaları olmayan hesapları olan Linux VM'lerini denetlemek için gereksinimleri dağıtma
- \[Önizleme\]: Parola olmayan hesaplardan uzaktan bağlantı sağlayan Linux VM'lerini denetlemek için gereksinimleri dağıtma
- \[Önizleme\]: Parolasız hesapları olan Linux VM'lerini denetle
- \[Önizleme\]: Parolasız hesaplardan uzaktan bağlantı sağlayan Linux VM'lerini denetle
- Depolama hesapları yeni Azure Kaynak Yöneticisi kaynaklarına geçirilmelidir
- Sanal makineler yeni Azure Kaynak Yöneticisi kaynaklarına geçirilmelidir
- Yönetilen diskleri kullanmayan denetim VM'leri

## <a name="11-external-interface-protection"></a>11 Dış Arayüz Koruması

Bu plan, uygun güvenli kullanıcı yönetimi için 25'ten fazla ilke kullanmanın dışında, sınırsız depolama hesaplarını izleyen bir [Azure İlkesi](../../../policy/overview.md) tanımı atayarak hizmet arabirimlerini yetkisiz erişime karşı korumanıza yardımcı olur. Sınırsız erişime sahip depolama hesapları, bilgi sisteminde bulunan bilgilere istenmeyen erişime izin verebilir. Bu plan ayrıca sanal makinelerde uyarlanabilir uygulama denetimleri sağlayan bir ilke atar.

- Depolama hesaplarına sınırsız ağ erişimini denetleme
- Uyarlanabilir Uygulama Denetimleri sanal makinelerde etkinleştirilmelidir
- IaaS'deki web uygulamaları için NSGs kuralları sertleştirilmiş olmalıdır
- Internet'ten erişim bitiş noktasına dönük olarak kısıtlanmalıdır
- Internet'in sanal makinelerle karşı karşıya olması için Ağ Güvenliği Grubu Kuralları sertleştirilmiş olmalıdır
- Uç nokta koruma çözümü sanal makine ölçek kümeleri üzerine kurulmalıdır
- Sanal makinelere anlık ağ erişim denetimi uygulanmalıdır
- Depolama hesaplarına sınırsız ağ erişimini denetleme
- İşlev Uygulaması için uzaktan hata ayıklama kapatılmalıdır
- Web Uygulaması için uzaktan hata ayıklama kapatılmalıdır
- API Uygulaması için uzaktan hata ayıklama kapatılmalıdır
- Web Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır
- Fonksiyon Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır
- API Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır

## <a name="12-secure-service-administration"></a>12 Güvenli Servis İdaresi

Azure, Azure'daki kaynaklara kimlerin erişebildiğinizi yönetmenize yardımcı olmak için rol tabanlı erişim denetimi (RBAC) uygular. Azure portalını kullanarak, Azure kaynaklarına ve izinlerine kimlerin erişimi olduğunu inceleyebilirsiniz. Bu plan, sahip sahibi ve/veya sahibi olan izinleri ve hesaplarını denetlemek ve/veya çok faktörlü kimlik doğrulaması etkin olmayan izinler yazmak için beş [Azure İlkesi](../../../policy/overview.md) tanımı atayarak ayrıcalıklı erişim haklarını kısıtlamanıza ve denetlemenize yardımcı olur.

Bir bulut hizmetinin yönetimi için kullanılan sistemler, bu hizmete son derece ayrıcalıklı erişime sahip olacaktır. Bu uzlaşma, güvenlik denetimlerini atlama ve büyük hacimli verileri çalma veya işleme araçları da dahil olmak üzere önemli bir etkiye sahip olacaktır. Hizmet sağlayıcının yöneticileri tarafından operasyonel hizmeti yönetmek için kullanılan yöntemler, hizmetin güvenliğini zedeleyebilecek herhangi bir yararlanma riskini azaltmak üzere tasarlanmalıdır. Bu ilke uygulanmazsa, saldırgan güvenlik denetimlerini atlamak ve büyük hacimli verileri çalmak veya işlemek için araçlara sahip olabilir.

- MFA, aboneliğinizde sahip izni olan hesaplarda etkinleştirilmelidir
- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Yazma izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

Bu plan, SQL sunucuları ve Hizmet Dokusu için Azure Active Directory kimlik doğrulamasını denetlemek için Azure İlkesi tanımlarını atar. Azure Active Directory kimlik doğrulaması kullanmak, veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin basitleştirilmiş izin yönetimini ve merkezi kimlik yönetimini sağlar.

- SQL sunucuları için bir Azure Active Directory yöneticisi sağlanmalıdır
- Hizmet Kumaş kümeleri yalnızca müşteri kimlik doğrulaması için Azure Etkin Dizini kullanmalıdır

Bu plan ayrıca, amortismana tabi hesaplar ve yüksek izinlere sahip dış hesaplar da dahil olmak üzere gözden geçirilmesi için öncelik verilmesi gereken denetim hesaplarına Azure İlkesi tanımlarını da atar. Gerektiğinde, hesapların oturum açması (veya kaldırılması) engellenebilir ve bu da Azure kaynaklarına erişim haklarını hemen kaldırır. Bu plan, amortismana tabi hesabı denetlemek için kaldırılması gereken iki Azure İlkesi tanımı atar.

- Amortismana alınan hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan amortismana alınmış hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Yazma izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

Bu plan ayrıca, yanlış ayarlanmışsa uyarmak için Linux VM parola dosyası izinlerini denetleyen bir Azure İlkesi tanımı da atar. Bu tasarım, kimlik doğrulayıcıların tehlikeye girmediğinden emin olmak için düzeltici eylemde bulunmanızı sağlar.

- \[Önizleme\]: Denetim Linux VM /etc/passwd dosya izinleri 0644 olarak ayarlanır

## <a name="13-audit-information-for-users"></a>13 Kullanıcılar için Denetim Bilgileri

Bu plan, Azure kaynaklarında günlük ayarlarını denetleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak sistem olaylarının günlüğe kaydedilmesini sağlamanıza yardımcı olur. Atanan bir ilke, sanal makineler günlükleri belirli bir günlük analizi çalışma alanına göndermiyorsa da denetler.

- SQL Server'da gelişmiş veri güvenliği ayarlarında denetim etkinleştirilmelidir
- Tanılama ayarını denetle
- \[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt
- Sanal ağlar oluşturulduğunda ağ izleyicisini dağıtma

## <a name="next-steps"></a>Sonraki adımlar

Şimdi İngiltere'de RESMİ ve İngiltere NHS planları kontrol haritalama inceledik, genel bakış ve nasıl bu örnek dağıtmak hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [İngiltere RESMİ ve İngiltere NHS planları - Genel Bakış](./index.md)
> [İngiltere RESMİ ve İngiltere NHS planları - Dağıtım adımları](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Plan yaşam döngüsü](../../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../../how-to/update-existing-assignments.md)öğrenin.
