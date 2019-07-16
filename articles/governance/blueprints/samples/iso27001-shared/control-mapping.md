---
title: Örnek - ISO 27001 paylaşılan hizmetler şema - denetim eşleme
description: Azure İlkesi ve RBAC ISO 27001 paylaşılan hizmetler şema örnek eşleme denetimi.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 5c225d76e1822d42500713578a7159eed0699a66
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68225952"
---
# <a name="control-mapping-of-the-iso-27001-shared-services-blueprint-sample"></a>ISO 27001 paylaşılan hizmetler şema örnek denetimi eşleme

Aşağıdaki makalede nasıl Azure şemaları ISO 27001 paylaşılan hizmetler ISO 27001 denetim örnek eşlenir blueprint ayrıntıları. Denetimler hakkında daha fazla bilgi için bkz. [ISO 27001](https://www.iso.org/isoiec-27001-information-security.html).

Şu eşlemeler üzeresiniz **ISO 27001: 2013** kontrol eder. Gezinti, doğrudan bir özel denetim eşlemesi atlamak için sağ taraftaki kullanın. Eşlenen denetimleri birçoğu ile uygulanan bir [Azure İlkesi](../../../policy/overview.md) girişim. Tam girişim gözden geçirmek için açık **ilke** Azure portal ve select **tanımları** sayfası. Daha sonra bulmak ve seçmek  **\[Önizleme\] denetim ISO 27001: 2013 denetler ve denetim gereksinimlerini desteklemek için belirli VM uzantılarını dağıtma** yerleşik ilke girişimi.

## <a name="a612-segregation-of-duties"></a>Görev ayrımı prensibini A.6.1.2

Tek bir Azure aboneliği sahibi olan Yönetim yedeklilik için izin vermez. Buna karşılık, çok fazla Azure aboneliğine sahip olan, güvenliği aşılmış sahip hesabı aracılığıyla bir ihlal olasılığını artırabilir. Bu şema iki atayarak Azure abonelik sahipleri uygun sayıda tutmanıza yardımcı olur. [Azure İlkesi](../../../policy/overview.md) denetim sahipleri Azure aboneliklerinin sayısını tanımlar. Abonelik sahibi izinleri yönetme, uygun görev ayrımı uygulamak yardımcı olabilir.

- \[Önizleme\]: Denetim sahip bir abonelik için en düşük sayısı
- \[Önizleme\]: Denetim sahip bir abonelik için en yüksek sayısı

## <a name="a821-classification-of-information"></a>A.8.2.1 sınıflandırması bilgileri

Azure'nın [SQL güvenlik açığı değerlendirme hizmeti](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) veritabanınızda depolanan hassas verileri keşfetmenize yardımcı olabilir ve bu verileri sınıflandırmak için öneriler içerir. Bu şema atar bir [Azure İlkesi](../../../policy/overview.md) SQL güvenlik açığı değerlendirmesi taraması sırasında güvenlik açıkları düzeltildiğinden denetleme tanımı.

- \[Önizleme\]: SQL güvenlik açığı değerlendirme sonuçları Azure Güvenlik Merkezi'nde izleme

## <a name="a912-access-to-networks-and-network-services"></a>Ağ ve ağ hizmetlerini A.9.1.2 erişimi

Azure uygular [rol tabanlı erişim denetimi](../../../../role-based-access-control/overview.md) Azure kaynaklarına kimlerin erişebildiğini yönetmek için (RBAC). Bu şema yardımcı olan yedi atayarak Azure kaynaklarına erişimi denetlemek [Azure İlkesi](../../../policy/overview.md) tanımlar. Bu ilkeler kaynak türlerinin kullanımını denetleyin ve daha esnek olarak izin verebileceği yapılandırmaları kaynaklarına erişim.
Yetkili kullanıcıların Azure kaynaklarına erişim sağlamak için düzeltici eylemleri Yardım kısıtlanmış, bu ilkeleri can ihlal anlama kaynaklar.

- \[Önizleme\]: Hiçbir parola ile Linux VM hesapları denetlemek için VM uzantısı dağıtma
- \[Önizleme\]: Linux VM hiçbir parola sahip hesaplardan gelen uzak bağlantılara izin vermeden denetlemek için VM uzantısı dağıtma
- \[Önizleme\]: Hiçbir parola ile Linux VM hesapları denetleme
- \[Önizleme\]: Linux VM hiçbir parola sahip hesaplardan gelen uzak bağlantılara izin vermeden denetleme
- Klasik depolama hesapları kullanımını denetleme
- Klasik sanal makinelerin kullanımını denetleme
- Yönetilen disk kullanmayan Vm'leri denetle

## <a name="a923-management-of-privileged-access-rights"></a>A.9.2.3 Yönetimi ayrıcalıklı erişim hakları

Bu şema kısıtlamak ve ayrıcalıklı erişim hakları dört atayarak denetlemenize yardımcı olur. [Azure İlkesi](../../../policy/overview.md) sahip dış hesapların denetim ve/veya izinleri ve hesapları sahip yazma ve/veya yazma izinleri tanımları Bu, çok faktörlü kimlik doğrulaması etkin yok. Azure rol tabanlı erişim denetimi (RBAC) kimlerin erişebildiğini yönetmek için Azure kaynaklarına uygular. Bu şema SQL sunucuları ve Service Fabric için Azure Active Directory kimlik doğrulaması kullanımını denetlemek için üç Azure İlkesi tanım ayrıca atar. Azure Active Directory'yi kullanarak kimlik doğrulaması, Basitleştirilmiş izin yönetimi ve veritabanı kullanıcıları ve diğer Microsoft hizmetlerinde merkezi kimlik yönetimi sağlar. Bu plan, ayrıca özel bir RBAC kurallar kullanımını denetlemek için bir Azure İlkesi tanım atar. Özel bir RBAC kurallar hata yapmaya açık olarak özel bir RBAC kuralları uygulamak nerede anlama, gereksinim ve uygun uygulama doğrulamanıza yardımcı olabilir.

- \[Önizleme\]: Mfa'yı bir abonelikte etkin olmayan sahip izinleri ile hesapları denetleme
- \[Önizleme\]: Mfa'yı bir abonelikte etkin olmayan yazma izinleri ile hesapları denetleme
- \[Önizleme\]: Bir abonelikte sahip izinleri olan dış hesapları denetleme
- \[Önizleme\]: Bir abonelikteki yazma izinleri olan dış hesapları denetleyin
- SQL server için Azure Active Directory Yöneticisi sağlama denetleme
- Service fabric'te istemci kimlik doğrulaması için Azure Active Directory kullanımını denetleme
- Özel bir RBAC kurallarının kullanımını denetleme

## <a name="a924-management-of-secret-authentication-information-of-users"></a>Kullanıcıların gizli kimlik bilgilerinin A.9.2.4 Yönetimi

Bu plan üç atar [Azure İlkesi](../../../policy/overview.md) çok faktörlü kimlik doğrulaması etkin olmayan hesaplar denetlemek için tanımları. Çok faktörlü kimlik doğrulaması, kimlik doğrulama bilgilerini bir parça tehlikede olsa bile hesapları korunmasına yardımcı olur. Çok faktörlü kimlik doğrulama etkin olmadan hesapları izleyerek, büyük olasılıkla tehlikeye hesapları tanımlayabilirsiniz. Bu şema, ayrıca Linux VM denetim iki Azure ilke tanımları yanlış ayarladıysanız, sizi uyarmak için parola dosya izinleri atar. Bu ayar, kimlik doğrulayıcılar tehlikede olmayan emin olmak için düzeltici eylemlerde sağlar.

- \[Önizleme\]: Mfa'yı bir abonelikte etkin olmayan sahip izinleri ile hesapları denetleme
- \[Önizleme\]: Mfa'yı bir abonelikte etkin olmayan okuma izinleri olan hesaplar denetleme
- \[Önizleme\]: Mfa'yı bir abonelikte etkin olmayan yazma izinleri ile hesapları denetleme
- \[Önizleme\]: Linux VM parola dosya izinleri denetlemek için VM uzantısı dağıtma
- \[Önizleme\]: Denetim Linux VM/etc/passwd dosya izinleri 0644 için ayarlanır.

## <a name="a925-review-of-user-access-rights"></a>Kullanıcı erişim haklarını A.9.2.5 gözden geçirme

Azure uygular [rol tabanlı erişim denetimi](../../../../role-based-access-control/overview.md) Azure'da kimlerin erişebileceğini kaynakları yönetmenize yardımcı olmak için (RBAC). Azure portalını kullanarak, kimlerin erişebileceğini Azure kaynaklarını ve izinlerini gözden geçirebilirsiniz. Bu şema dört atar [Azure İlkesi](../../../policy/overview.md) tanımlarını gözden geçirme için amorti edilmiş ve yükseltilmiş izinleri olan dış hesapları dahil olmak üzere, öncelik verilmesi gerektiği hesapları denetleme.

- \[Önizleme\]: Bir Abonelikteki kullanım dışı bırakılmış hesapları denetleme
- \[Önizleme\]: Bir abonelikte sahip izinleri ile kullanım dışı bırakılmış hesapları denetleme
- \[Önizleme\]: Bir abonelikte sahip izinleri olan dış hesapları denetleme
- \[Önizleme\]: Bir abonelikteki yazma izinleri olan dış hesapları denetleyin

## <a name="a926-removal-or-adjustment-of-access-rights"></a>A.9.2.6 kaldırma veya erişim haklarını ayarlama

Azure uygular [rol tabanlı erişim denetimi](../../../../role-based-access-control/overview.md) Azure'da kimlerin erişebileceğini kaynakları yönetmenize yardımcı olmak için (RBAC). Kullanarak [Azure Active Directory](../../../../active-directory/fundamentals/active-directory-whatis.md) ve RBAC, kullanıcı rolleri, kuruluş değişiklikleri yansıtacak şekilde güncelleştirebilirsiniz. Gerektiğinde, hesapları oturum açma engellendi (veya kaldırıldı) hangi Azure kaynakları için erişim haklarını hemen kaldırır. Bu şema iki atar [Azure İlkesi](../../../policy/overview.md) kaldırılmak üzere davranılacak amorti edilmiş hesabı denetlemek için tanımları.

- \[Önizleme\]: Bir Abonelikteki kullanım dışı bırakılmış hesapları denetleme
- \[Önizleme\]: Bir abonelikte sahip izinleri ile kullanım dışı bırakılmış hesapları denetleme

## <a name="a942-secure-log-on-procedures"></a>Oturum açma A.9.4.2 güvenli yordamları

Bu plan, çok faktörlü kimlik doğrulaması etkin olmayan hesaplar denetlemek için üç Azure İlkesi tanım atar. Azure multi-Factor Authentication ikinci bir form kimlik doğrulaması gerektirerek ek güvenlik sağlar ve güçlü kimlik doğrulaması sunar. Çok faktörlü kimlik doğrulama etkin olmadan hesapları izleyerek, büyük olasılıkla tehlikeye hesapları tanımlayabilirsiniz.

- \[Önizleme\]: Mfa'yı bir abonelikte etkin olmayan sahip izinleri ile hesapları denetleme
- \[Önizleme\]: Mfa'yı bir abonelikte etkin olmayan okuma izinleri olan hesaplar denetleme
- \[Önizleme\]: Mfa'yı bir abonelikte etkin olmayan yazma izinleri ile hesapları denetleme

## <a name="a943-password-management-system"></a>A.9.4.3 parola yönetimi sistemi

Bu şema 10 atayarak güçlü parolalar zorunlu yardımcı olan [Azure İlkesi](../../../policy/overview.md) en az gücü ve diğer parola gereksinimlerini zorlamaz; Windows Vm'leri denetle tanımlar. Parola gücü ilkeyi ihlal VM'lerin tanıma tüm VM kullanıcı hesapları için parola ilkesiyle uyumlu olduğundan emin olmak için düzeltici eylemleri yardımcı olur.

- \[Önizleme\]: Denetim için VM uzantısı dağıtma Windows VM parola karmaşıklık gereksinimlerini zorunlu kılar
- \[Önizleme\]: Windows VM en fazla parola geçerlilik süresi 70 gün denetlemek için VM uzantısı dağıtma
- \[Önizleme\]: Windows VM en az parola geçerlilik süresi 1 gün denetlemek için VM uzantısı dağıtma
- \[Önizleme\]: VM dağıtma uzantısı Windows VM parolalarını denetlemek için en az 14 karakter olmalıdır
- \[Önizleme\]: Denetim için VM uzantısı dağıtma Windows VM önceki 24 parolaların izin
- \[Önizleme\]: Denetim Windows VM parola karmaşıklık gereksinimlerini zorunlu kılar.
- \[Önizleme\]: Windows VM en fazla parola geçerlilik süresi 70 gün denetle
- \[Önizleme\]: Windows VM en az parola geçerlilik süresi 1 gün denetle
- \[Önizleme\]: Denetim Windows VM parolalar en az 14 karakter olmalıdır
- \[Önizleme\]: Denetim Windows VM önceki 24 parolaların izin vermemelidir

## <a name="a1011-policy-on-the-use-of-cryptographic-controls"></a>A.10.1.1 İlkesi kullanımı için şifreleme denetimleri

Bu şema 13 atayarak, politikasını cryptograph denetimlerin kullanımına yardımcı olan [Azure İlkesi](../../../policy/overview.md) belirli cryptograph denetimlerini ve denetim zorunlu tanımları zayıf şifreleme ayarları kullanın.
Burada Azure kaynaklarınıza uygun olmayan şifreleme yapılandırmaları olabilir kaynakların, bilgi Güvenlik ilkenize uygun şekilde yapılandırıldığından emin olmak için düzeltici eylemleri yardımcı olabileceğini anlama. Özellikle, bu şema tarafından atanan ilkeler, blob depolama hesapları ve data lake depolama hesapları için şifreleme gerektiren; SQL veritabanlarında saydam veri şifrelemesi; Depolama hesapları, SQL veritabanları, sanal makine disklerini ve Otomasyon hesabı değişkenleri eksik şifrelemesini denetle; Depolama hesapları, işlev uygulamaları, Web uygulaması, API Apps ve Redis Cache için güvenli bağlantıları denetim; sanal makine zayıf parola şifrelemesini denetler; ve Service Fabric şifrelenmemiş iletişimin denetleyebilirsiniz.

- \[Önizleme\]: Bir işlev uygulaması için yalnızca HTTPS erişimi denetleme
- \[Önizleme\]: Bir Web uygulaması için yalnızca HTTPS erişimi denetleme
- \[Önizleme\]: Bir API uygulaması için yalnızca HTTPS erişimi denetleme
- \[Önizleme\]: Depolama hesapları için eksik blob şifrelemesini denetler
- \[Önizleme\]: Denetim için VM uzantısı dağıtma Windows VM parolaları ters çevrilebilir şifreleme kullanarak değil saklamak
- \[Önizleme\]: Parolaları, ters çevrilebilir şifreleme kullanarak Windows VM denetim depolanmamalıdır
- \[Önizleme\]: Azure Güvenlik Merkezi'nde şifrelenmemiş SQL veritabanını İzle
- \[Önizleme\]: Azure Güvenlik Merkezi'nde şifrelenmemiş VM disklerini izleyin
- Otomasyon hesabı değişkenlerin şifreleme denetim etkinleştirme
- Yalnızca güvenli bağlantılar Redis Cache'inizi etkinleştirme denetleme
- Güvenli aktarım depolama hesapları için Denetim
- Service fabric'te EncryptAndSign özelliğini ClusterProtectionLevel ayarını denetle
- Saydam veri şifreleme durumunu denetle

## <a name="a1241-event-logging"></a>A.12.4.1 olay günlüğü

Bu şema sistem olaylarını yedi atayarak kaydedilir olmanıza yardımcı olur. [Azure İlkesi](../../../policy/overview.md) Azure kaynaklarını denetim günlüğü ayarlarını tanımlar.
Tanılama günlükleri, Azure kaynaklarını içinde gerçekleştirilen işlemler hakkında bilgi sağlar.

- \[Önizleme\]: Denetim bağımlılık Aracısı dağıtımı - VM görüntüsü (OS) listeden kaldırıldı
- \[Önizleme\]: Bağımlılık Aracısı VMSS - VM görüntüsü (OS) listelenmemiş dağıtımda denetleme
- \[Önizleme\]: Denetim günlüğü analiz aracı dağıtımı - VM görüntüsü (OS) listeden kaldırıldı
- \[Önizleme\]: Denetim günlüğü analiz aracı dağıtım VMSS - VM görüntüsü (OS) listeden kaldırıldı
- \[Önizleme\]: Azure Güvenlik Merkezi'nde denetlenmeyen SQL veritabanını izleme
- Tanılama ayarını denetle
- SQL sunucu düzeyi denetimi ayarları denetle

## <a name="a1243-administrator-and-operator-logs"></a>Günlükleri A.12.4.3 yönetici and işleci

Bu şema sistem olaylarını, Azure kaynaklarını denetim günlüğü ayarlarını yedi Azure ilke tanımları atayarak oturum açmış olmanıza yardımcı olur. Tanılama günlükleri, Azure kaynaklarını içinde gerçekleştirilen işlemler hakkında bilgi sağlar.

- \[Önizleme\]: Denetim bağımlılık Aracısı dağıtımı - VM görüntüsü (OS) listeden kaldırıldı
- \[Önizleme\]: Bağımlılık Aracısı VMSS - VM görüntüsü (OS) listelenmemiş dağıtımda denetleme
- \[Önizleme\]: Denetim günlüğü analiz aracı dağıtımı - VM görüntüsü (OS) listeden kaldırıldı
- \[Önizleme\]: Denetim günlüğü analiz aracı dağıtım VMSS - VM görüntüsü (OS) listeden kaldırıldı
- \[Önizleme\]: Azure Güvenlik Merkezi'nde denetlenmeyen SQL veritabanını izleme
- Tanılama ayarını denetle
- SQL sunucu düzeyi denetimi ayarları denetle

## <a name="a1244-clock-synchronization"></a>A.12.4.4 saati eşitleme

Bu şema sistem olaylarını, Azure kaynaklarını denetim günlüğü ayarlarını yedi Azure ilke tanımları atayarak oturum açmış olmanıza yardımcı olur. Azure günlüklerini eşitlenmiş iç saat kaynakları genelinde olayların süresi bağıntılı bir kayıt oluşturmak için kullanır.

- \[Önizleme\]: Denetim bağımlılık Aracısı dağıtımı - VM görüntüsü (OS) listeden kaldırıldı
- \[Önizleme\]: Bağımlılık Aracısı VMSS - VM görüntüsü (OS) listelenmemiş dağıtımda denetleme
- \[Önizleme\]: Denetim günlüğü analiz aracı dağıtımı - VM görüntüsü (OS) listeden kaldırıldı
- \[Önizleme\]: Denetim günlüğü analiz aracı dağıtım VMSS - VM görüntüsü (OS) listeden kaldırıldı
- \[Önizleme\]: Azure Güvenlik Merkezi'nde denetlenmeyen SQL veritabanını izleme
- Tanılama ayarını denetle
- SQL sunucu düzeyi denetimi ayarları denetle

## <a name="a1251-installation-of-software-on-operational-systems"></a>İşletim sistemlerine yazılım A.12.5.1 yüklemesi

Uyarlamalı uygulama denetimlerini, Azure Güvenlik Merkezi'nden, Azure'da yer alan sanal makineleriniz uygulamaları denetlemenize yardımcı çalıştırabilirsiniz çözümüdür. Bu plan için izin verilen uygulamalar yapılan değişiklikleri izleyen bir Azure İlkesi tanım atar. Bu özellik, yardımcı olur. Denetim, yazılım yüklemesini ve Azure vm'lerdeki uygulamalar.

- \[Önizleme\]: Olası uygulama beyaz listesini Azure Güvenlik Merkezi'nde izleme

## <a name="a1261-management-of-technical-vulnerabilities"></a>Teknik güvenlik açıklarının A.12.6.1 Yönetimi

Bu şema beş atayarak bilgi sistemi güvenlik açıkları yönetmenize yardımcı olan [Azure İlkesi](../../../policy/overview.md) eksik sistem güncelleştirmeleri, işletim sistemi güvenlik açıkları, SQL güvenlik açıkları ve sanal makine izleyen tanımları Azure Güvenlik Merkezi'nde güvenlik açıklarını. Azure Güvenlik Merkezi, dağıtılan Azure kaynaklarınızın güvenlik durumunu gerçek zamanlı öngörülere sahip olmanıza olanak sağlayan bir raporlama yetenekleri sağlar.

- \[Önizleme\]: Azure Güvenlik Merkezi'nde eksik Endpoint Protection'ı izleme
- \[Önizleme\]: Azure Güvenlik Merkezi'nde eksik sistem güncelleştirmelerini izleme
- \[Önizleme\]: Azure Güvenlik Merkezi'nde işletim sistemi güvenlik açıklarını izleyin
- \[Önizleme\]: SQL güvenlik açığı değerlendirme sonuçları Azure Güvenlik Merkezi'nde izleme
- \[Önizleme\]: Azure Güvenlik Merkezi'nde izleme VM güvenlik açıklarını

## <a name="a1262-restrictions-on-software-installation"></a>Yazılım yükleme A.12.6.2 kısıtlamaları

Uyarlamalı uygulama denetimlerini, Azure Güvenlik Merkezi'nden, Azure'da yer alan sanal makineleriniz uygulamaları denetlemenize yardımcı çalıştırabilirsiniz çözümüdür. Bu plan için izin verilen uygulamalar yapılan değişiklikleri izleyen bir Azure İlkesi tanım atar. Yazılım yükleme kısıtlamaları yazılım güvenlik açıklarından giriş olasılığını azaltmaya yardımcı olabilir.

- \[Önizleme\]: Olası uygulama beyaz listesini Azure Güvenlik Merkezi'nde izleme

## <a name="a1311-network-controls"></a>A.13.1.1 ağ denetimleri

Bu şema yönetmenize ve denetlemenize ağları atayarak yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) , esnek kurallara sahip ağ güvenlik grupları izleyen tanımı. Çok esnek olan kuralları istenmeyen ağ erişimini izin verebilir ve gözden geçirilmesi gerekir. Bu şema korumasız uç noktalar, uygulamaları ve depolama hesaplarına izleyen üç Azure İlkesi tanım ayrıca atar. Uç noktaları ve güvenlik duvarı ve depolama hesapları ile sınırsız erişim tarafından korunmayan uygulamalar bilgi sistemi içinde yer alan bilgileri istenmeyen erişime izin verebilir.

- \[Önizleme\]: Azure Güvenlik Merkezi'nde esnek ağın genelini erişimi izleme
- \[Önizleme\]: Azure Güvenlik Merkezi'nde korumasız ağ uç noktalarını izleme
- \[Önizleme\]: Azure Güvenlik Merkezi'nde korumasız web uygulamasını izleme
- Depolama hesapları sınırsız ağ erişimi denetleyin

## <a name="a1321-information-transfer-policies-and-procedures"></a>A.13.2.1 bilgi aktarımı politikaları ve yordamları

Blueprint bilgi aktarımı Azure Hizmetleri ile güvenli iki atayarak olmanıza yardımcı olur. [Azure İlkesi](../../../policy/overview.md) depolama hesapları ve Redis önbelleği için güvenli bağlantıları denetlemek için tanımları.

- Yalnızca güvenli bağlantılar Redis Cache'inizi etkinleştirme denetleme
- Güvenli aktarım depolama hesapları için Denetim

## <a name="next-steps"></a>Sonraki adımlar

ISO 27001 paylaşılan hizmetler blueprint'in denetimi eşleme gözden geçirdikten sonra mimari ve bu örneğin nasıl dağıtılacağını öğrenmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [ISO 27001 paylaşılan hizmetler şema - genel bakış](./index.md)
> [ISO 27001 paylaşılan hizmetler blueprint - adımları dağıtma](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.
