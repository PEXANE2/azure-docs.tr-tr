---
title: ISO 27001 plan örnek kontrolleri
description: ISO 27001 plan örneğinin kontrol eşlemi. Her denetim, değerlendirmeye yardımcı olan bir veya daha fazla Azure İlkesiyle eşlenir.
ms.date: 01/13/2020
ms.topic: sample
ms.openlocfilehash: 501884491dbef85cdf8a29cb5fdcef44a68235a9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75920564"
---
# <a name="control-mapping-of-the-iso-27001-blueprint-sample"></a>ISO 27001 plan örneğinin kontrol eşlemi

Aşağıdaki makalede, Azure Planları ISO 27001 plan örnek haritalarının ISO 27001 denetimlerine nasıl ait olduğu ayrıntılı olarak açıklanmaktadır. Denetimler hakkında daha fazla bilgi için [ISO 27001'e](https://www.iso.org/isoiec-27001-information-security.html)bakın.

Aşağıdaki eşlemeler **ISO 27001:2013** kontrollerine ait. Doğrudan belirli bir denetim eşleleme sine atlamak için sağdaki gezinmeyi kullanın. Eşlenen denetimlerin çoğu bir Azure [İlkesi](../../../policy/overview.md) girişimiyle uygulanır. Girişimin tamamını incelemek için Azure portalında **İlke'yi** açın ve **Tanımlar** sayfasını seçin. Ardından, ** \[Önizleme\] Denetimi ISO 27001:2013 denetimlerini** bulun ve seçin ve yerleşik politika girişimini denetlemek gereksinimlerini desteklemek için belirli VM Uzantıları dağıtın.

> [!IMPORTANT]
> Aşağıdaki her denetim bir veya daha fazla [Azure İlkesi](../../../policy/overview.md) tanımıyla ilişkilidir. Bu ilkeler, denetime [uygunluğu değerlendirmenize](../../../policy/how-to/get-compliance-data.md) yardımcı olabilir; ancak, genellikle bir denetim ve bir veya daha fazla ilke arasında 1:1 veya tam bir eşleşme yoktur. Bu nedenle, Azure İlkesinde **Uyumlu olmak** yalnızca ilkelerin kendileri için başvurur; bu, bir denetimin tüm gereksinimlerine tam olarak uymanızı sağlamaz. Ayrıca, uyumluluk standardı şu anda herhangi bir Azure İlkesi tanımı tarafından ele alınmayan denetimleri içerir. Bu nedenle, Azure İlkesi'ndeki uyumluluk, genel uyumluluk durumunuza yalnızca kısmi bir görünümdür. Bu uyumluluk planı örneği için denetimler ve Azure İlkesi tanımları arasındaki ilişkilendirmeler zaman içinde değişebilir. Değişiklik geçmişini görüntülemek için [GitHub Commit History'ye](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/iso27001/control-mapping.md)bakın.

## <a name="a612-segregation-of-duties"></a>A.6.1.2 Görevlerin ayrılması

Yalnızca bir Azure abonelik sahibine sahip olmak, yönetim fazlalığına izin vermez. Tam tersine, çok fazla Azure abonelik sahibine sahip olmak, gizliliği ihlal edilen bir sahip hesabı aracılığıyla ihlal potansiyelini artırabilir. Bu plan, Azure aboneliklerinin sahip sayısını denetleyen iki [Azure İlkesi](../../../policy/overview.md) tanımı atayarak uygun sayıda Azure abonelik sahibini korumanıza yardımcı olur. Abonelik sahibi izinlerini yönetmek, uygun görev ayrımını uygulamanıza yardımcı olabilir.

- \[Önizleme\]: Abonelik için en az sahip sayısını denetleme
- \[Önizleme\]: Abonelik için maksimum sahip sayısını denetleme

## <a name="a821-classification-of-information"></a>A.8.2.1 Bilgilerin sınıflandırılması

Azure'un [SQL Güvenlik Açığı Değerlendirmesi hizmeti,](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) veritabanlarınızda depolanan hassas verileri keşfetmenize yardımcı olabilir ve bu verileri sınıflandırmak için öneriler içerir. Bu plan, SQL Güvenlik Açığı Değerlendirmesi tetkiki sırasında tanımlanan güvenlik açıklarının düzeltileni denetlemek için bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- \[Önizleme\]: Azure Güvenlik Merkezi'nde SQL güvenlik açığı değerlendirme sonuçlarını izleyin

## <a name="a912-access-to-networks-and-network-services"></a>A.9.1.2 Ağlara ve ağ hizmetlerine erişim

Azure, Azure kaynaklarına kimlerin erişebilenleri yönetmek için [rol tabanlı erişim denetimi](../../../../role-based-access-control/overview.md) (RBAC) uygular. Bu plan, yedi [Azure İlkesi](../../../policy/overview.md) tanımı atayarak Azure kaynaklarına erişimi denetlemenize yardımcı olur. Bu ilkeler, kaynaklara daha fazla izin veren erişime izin verebilecek kaynak türlerinin ve yapılandırmaların kullanımını denetler.
Bu ilkeleri ihlal eden kaynakları anlamak, Azure kaynaklarına erişimin yetkili kullanıcılarla sınırlı olduğundan emin olmak için düzeltici eylemlerde yardımcı olabilir.

- \[Önizleme\]: Linux VM hesaplarını parolasız denetlemek için VM uzantısını dağıtın
- \[Önizleme\]: Linux VM'yi denetlemek için VM uzantısını dağıtarak parolası olmayan hesaplardan uzaktan bağlantı sağlar
- \[Önizleme\]: Linux VM hesaplarını şifresiz denetle
- \[Önizleme\]: Linux VM'yi denetleyerek parolasız hesaplardan uzaktan bağlantı sağlar
- Klasik depolama hesaplarının denetim kullanımı
- Klasik sanal makinelerin denetim kullanımı
- Yönetilen diskleri kullanmayan denetim VM'leri

## <a name="a923-management-of-privileged-access-rights"></a>A.9.2.3 Ayrıcalıklı erişim haklarının yönetimi

Bu plan, sahip sahibi ve/veya çok faktörlü kimlik doğrulaması etkin olmayan sahibi ve/veya yazma izinleri olan dış hesapları denetlemek için dört [Azure İlkesi](../../../policy/overview.md) tanımı atayarak ayrıcalıklı erişim haklarını kısıtlamanıza ve denetlemenize yardımcı olur. Azure, Azure kaynaklarına kimlerin erişebilenleri yönetmek için rol tabanlı erişim denetimi (RBAC) uygular. Bu plan ayrıca, SQL Sunucuları ve Hizmet Dokusu için Azure Active Directory kimlik doğrulamasını denetlemek için üç Azure İlkesi tanımı atar. Azure Active Directory kimlik doğrulaması kullanmak, veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin basitleştirilmiş izin yönetimini ve merkezi kimlik yönetimini sağlar. Bu plan, özel RBAC kurallarının kullanımını denetlemek için bir Azure İlkesi tanımı da atar. Özel RBAC kurallarının nerede uygulandığını anlamak, özel RBAC kuralları hataya açık olduğundan, gereksinimi ve doğru uygulamayı doğrulamanıza yardımcı olabilir.

- \[Önizleme\]: Abonelikte MFA etkin olmayan sahip izinlerine sahip hesapları denetleme
- \[Önizleme\]: Abonelikte MFA etkin olmayan yazma izinleri olan hesapları denetleme
- \[Önizleme\]: Abonelikte sahip izinleri olan dış hesapları denetleme
- \[Önizleme\]: Abonelik te yazma izinleri ile dış hesapları denetleme
- SQL sunucusu için bir Azure Active Directory yöneticisinin denetim sağlama
- Hizmet Kumaşında müşteri kimlik doğrulaması için Azure Active Directory'nin denetim kullanımı
- Özel RBAC kurallarının denetim kullanımı

## <a name="a924-management-of-secret-authentication-information-of-users"></a>A.9.2.4 Kullanıcıların gizli kimlik doğrulama bilgilerinin yönetimi

Bu plan, çok faktörlü kimlik doğrulaması etkin olmayan denetim hesaplarına üç [Azure İlkesi](../../../policy/overview.md) tanımı atar. Çok faktörlü kimlik doğrulama, bir parça kimlik doğrulama bilgisi tehlikeye atsa bile hesapların güvende tutulmasına yardımcı olur. Çok faktörlü kimlik doğrulaması etkin leştirilmeden hesapları izleyerek, gizliliği ihlal edilme olasılığı daha yüksek hesapları tanımlayabilirsiniz. Bu plan ayrıca, yanlış ayarlanmışsa uyarmak için Linux VM parola dosyası izinlerini denetleyen iki Azure İlkesi tanımı da atar. Bu kurulum, kimlik doğrulayıcıların tehlikeye girmediğinden emin olmak için düzeltici eylemde bulunmanızı sağlar.

- \[Önizleme\]: Abonelikte MFA etkin olmayan sahip izinlerine sahip hesapları denetleme
- \[Önizleme\]: Abonelikte MFA etkin olmayan okuma izinleri olan hesapları denetleme
- \[Önizleme\]: Abonelikte MFA etkin olmayan yazma izinleri olan hesapları denetleme
- \[Önizleme\]: Linux VM passwd dosya izinlerini denetlemek için VM uzantısı dağıtma
- \[Önizleme\]: Denetim Linux VM /etc/passwd dosya izinleri 0644 olarak ayarlanır

## <a name="a925-review-of-user-access-rights"></a>A.9.2.5 Kullanıcı erişim haklarının gözden geçirilmesi

Azure, [Azure'daki](../../../../role-based-access-control/overview.md) kaynaklara kimlerin erişebilenleri yönetmenize yardımcı olmak için rol tabanlı erişim denetimi (RBAC) uygular. Azure portalını kullanarak, Azure kaynaklarına ve izinlerine kimlerin erişimi olduğunu inceleyebilirsiniz. Bu plan, amortismana tabi hesaplar ve yüksek izinlere sahip dış hesaplar da dahil olmak üzere gözden geçirilmesi için öncelik verilmesi gereken denetim hesaplarına dört [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- \[Önizleme\]: Bir abonelikte amortismana niçin amortismana netti
- \[Önizleme\]: Abonelikte sahip izinleri olan amortismana dayalı hesapları denetleme
- \[Önizleme\]: Abonelikte sahip izinleri olan dış hesapları denetleme
- \[Önizleme\]: Abonelik te yazma izinleri ile dış hesapları denetleme

## <a name="a926-removal-or-adjustment-of-access-rights"></a>A.9.2.6 Erişim haklarının kaldırılması veya ayarı

Azure, [Azure'daki](../../../../role-based-access-control/overview.md) kaynaklara kimlerin erişebilenleri yönetmenize yardımcı olmak için rol tabanlı erişim denetimi (RBAC) uygular. [Azure Etkin Dizini](../../../../active-directory/fundamentals/active-directory-whatis.md) ve RBAC'ı kullanarak, kullanıcı rollerini kuruluş değişikliklerini yansıtacak şekilde güncelleştirebilirsiniz. Gerektiğinde, hesapların oturum açması (veya kaldırılması) engellenebilir ve bu da Azure kaynaklarına erişim haklarını hemen kaldırır. Bu plan, amortismana tabi hesabı denetlemek için kaldırılması gereken iki [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- \[Önizleme\]: Bir abonelikte amortismana niçin amortismana netti
- \[Önizleme\]: Abonelikte sahip izinleri olan amortismana dayalı hesapları denetleme

## <a name="a942-secure-log-on-procedures"></a>A.9.4.2 Güvenli oturum açma prosedürleri

Bu plan, çok faktörlü kimlik doğrulaması etkin olmayan denetim hesaplarına üç Azure İlkesi tanımı atar. Azure Çok Faktörlü Kimlik Doğrulaması, ikinci bir kimlik doğrulama biçimi gerektirerek ek güvenlik sağlar ve güçlü kimlik doğrulaması sağlar. Çok faktörlü kimlik doğrulaması etkin leştirilmeden hesapları izleyerek, gizliliği ihlal edilme olasılığı daha yüksek hesapları tanımlayabilirsiniz.

- \[Önizleme\]: Abonelikte MFA etkin olmayan sahip izinlerine sahip hesapları denetleme
- \[Önizleme\]: Abonelikte MFA etkin olmayan okuma izinleri olan hesapları denetleme
- \[Önizleme\]: Abonelikte MFA etkin olmayan yazma izinleri olan hesapları denetleme

## <a name="a943-password-management-system"></a>A.9.4.3 Şifre yönetim sistemi

Bu plan, minimum güç ve diğer parola gereksinimlerini zorlamayan Windows VM'lerini denetleyen 10 [Azure İlkesi](../../../policy/overview.md) tanımı atayarak güçlü parolalar uygulamanıza yardımcı olur. Parola gücü ilkesini ihlal eden VM'lerin farkında olmak, tüm VM kullanıcı hesaplarının parolalarının ilkeyle uyumlu olduğundan emin olmak için düzeltici eylemlerde yardımcı olur.

- \[Önizleme\]: Windows VM denetimi için VM uzantısı dağıtma parola karmaşıklığı gereksinimlerini zorlar
- \[Önizleme\]: Windows VM maksimum parola yaşı 70 gün denetlemek için VM uzantısı dağıtma
- \[Önizleme\]: Windows VM minimum parola yaşı 1 gün denetlemek için VM uzantısı dağıtma
- \[Önizleme\]: Windows VM parolalarını denetlemek için VM uzantısını dağıtmaen en az 14 karakter olmalıdır
- \[Önizleme\]: Windows VM denetlemek için VM uzantısı dağıtma önceki 24 parolaya izin vermemelidir
- \[Önizleme\]: Denetim Windows VM parola karmaşıklığı gereksinimlerini zorlar
- \[Önizleme\]: Denetim Windows VM maksimum şifre yaş 70 gün
- \[Önizleme\]: Denetim Windows VM minimum şifre yaş 1 gün
- \[Önizleme\]: Denetim Windows VM parolaları en az 14 karakter olmalıdır
- \[Önizleme\]: Denetim Windows VM önceki 24 parolaya izin vermemelidir

## <a name="a1011-policy-on-the-use-of-cryptographic-controls"></a>A.10.1.1 Şifreleme kontrollerinin kullanımına ilişkin politika

Bu plan, belirli şifreleme denetimlerini ve zayıf şifreleme ayarlarının denetim kullanımını zorunlu kılarak 13 [Azure Politikası](../../../policy/overview.md) tanımı atayarak şifreleme denetimlerinin kullanımına ilişkin ilkenizi uygulamanıza yardımcı olur.
Azure kaynaklarınızın en iyi olmayan şifreleme yapılandırmalarına sahip olabileceğini anlamak, kaynakların bilgi güvenliği politikanıza uygun şekilde yapılandırıldığından emin olmak için düzeltici eylemlerde yardımcı olabilir. Özellikle, bu plan tarafından atanan ilkeler blob depolama hesapları ve veri gölü depolama hesapları için şifreleme gerektirir; SQL veritabanlarında saydam veri şifreleme gerektirir; depolama hesapları, SQL veritabanları, sanal makine diskleri ve otomasyon hesabı değişkenlerinde eksik şifrelemeyi denetlemek; depolama hesaplarına, İşlev Uygulamalarına, Web Uygulamasına, API Uygulamalarına ve Redis Önbelleğine güvenli olmayan bağlantıları denetlemek; denetim zayıf sanal makine şifre şifreleme; ve şifrelenmemiş Hizmet Kumaşı iletişimini denetleyin.

- \[Önizleme\]: Yalnızca Bir İşlev Uygulaması için HTTPS denetimini denetle
- \[Önizleme\]: Denetim HTTPS yalnızca bir Web Uygulaması için erişim
- \[Önizleme\]: Denetim HTTPS yalnızca bir API Uygulaması için erişim
- \[Önizleme\]: Depolama hesapları için eksik blob şifrelemesini denetleme
- \[Önizleme\]: Windows VM'yi denetlemek için VM uzantısını dağıtma, parolaları ters çevrilebilir şifreleme kullanarak depolamamalıdır
- \[Önizleme\]: Denetim Windows VM, parolaları ters çevrilebilir şifreleme kullanarak depolamamalıdır
- \[Önizleme\]: Azure Güvenlik Merkezi'nde şifrelenmemiş VM Diskleri izleyin
- Otomasyon hesap değişkenlerinin şifrelemesinin denetim etkinleştirme
- Redis Önbelleğinize yalnızca güvenli bağlantıların etkinleştirilmesini denetleme
- Depolama hesaplarına güvenli transferi denetleme
- ClusterProtectionLevel özelliğinin Ayarını Şifrelemeye Ve Hizmet Kumaşına İmzala
- Saydam veri şifreleme durumunu denetle
- SQL veritabanlarında Saydam Veri Şifrelemesi etkinleştirilmelidir

## <a name="a1241-event-logging"></a>A.12.4.1 Olay günlüğü

Bu plan, Azure kaynaklarında günlük ayarlarını denetleyen yedi [Azure İlkesi](../../../policy/overview.md) tanımı atayarak sistem olaylarının günlüğe kaydedilmesini sağlamanıza yardımcı olur.
Tanılama günlükleri, Azure kaynakları içinde gerçekleştirilen işlemlerhakkında bilgi sağlar.

- \[Önizleme\]: Denetim Bağımlılık Aracısı Dağıtımı - VM Image (OS) listelenmemiş
- \[Önizleme\]: VMSS'de Denetim Bağımlılık Aracısı Dağıtımı - VM Image (OS) listelenmemiş
- \[Önizleme\]: Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- \[Önizleme\]: VMSS'de Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- Tanılama ayarını denetle
- DENETIM SQL sunucu düzeyi Denetim ayarları
- SQL Server'da gelişmiş veri güvenliği ayarlarında denetim etkinleştirilmelidir

## <a name="a1243-administrator-and-operator-logs"></a>A.12.4.3 Yönetici ve operatör günlükleri

Bu plan, Azure kaynaklarında günlük ayarlarını denetleyen yedi Azure İlkesi tanımı atayarak sistem olaylarının günlüğe kaydedilmesini sağlamanıza yardımcı olur. Tanılama günlükleri, Azure kaynakları içinde gerçekleştirilen işlemlerhakkında bilgi sağlar.

- \[Önizleme\]: Denetim Bağımlılık Aracısı Dağıtımı - VM Image (OS) listelenmemiş
- \[Önizleme\]: VMSS'de Denetim Bağımlılık Aracısı Dağıtımı - VM Image (OS) listelenmemiş
- \[Önizleme\]: Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- \[Önizleme\]: VMSS'de Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- Tanılama ayarını denetle
- DENETIM SQL sunucu düzeyi Denetim ayarları
- SQL Server'da gelişmiş veri güvenliği ayarlarında denetim etkinleştirilmelidir

## <a name="a1244-clock-synchronization"></a>A.12.4.4 Saat senkronizasyonu

Bu plan, Azure kaynaklarında günlük ayarlarını denetleyen yedi Azure İlkesi tanımı atayarak sistem olaylarının günlüğe kaydedilmesini sağlamanıza yardımcı olur. Azure günlükleri, kaynaklar arasında olayların zamanla ilişkili bir kaydını oluşturmak için senkronize iç saatlere güvenir.

- \[Önizleme\]: Denetim Bağımlılık Aracısı Dağıtımı - VM Image (OS) listelenmemiş
- \[Önizleme\]: VMSS'de Denetim Bağımlılık Aracısı Dağıtımı - VM Image (OS) listelenmemiş
- \[Önizleme\]: Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- \[Önizleme\]: VMSS'de Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- Tanılama ayarını denetle
- DENETIM SQL sunucu düzeyi Denetim ayarları
- SQL Server'da gelişmiş veri güvenliği ayarlarında denetim etkinleştirilmelidir

## <a name="a1251-installation-of-software-on-operational-systems"></a>A.12.5.1 Operasyonel sistemlere yazılım kurulumu

Uyarlanabilir uygulama denetimi, Azure Güvenlik Merkezi'nin azure'da bulunan VM'lerinizde hangi uygulamaların çalıştırılamayabileceğini denetlemenize yardımcı olan bir çözümdür. Bu plan, izin verilen uygulamalar kümesindeki değişiklikleri izleyen bir Azure İlkesi tanımı atar. Bu özellik, Azure VM'lerinde yazılım ve uygulamaların yüklenmesini denetlemenize yardımcı olur.

- \[Önizleme\]: Azure Güvenlik Merkezi'nde olası uygulama Beyaz Liste'yi izleyin

## <a name="a1261-management-of-technical-vulnerabilities"></a>A.12.6.1 Teknik güvenlik açıklarının yönetimi

Bu plan, Eksik sistem güncelleştirmelerini, işletim sistemi güvenlik açıklarını, SQL güvenlik açıklarını ve Azure Güvenlik Merkezi'ndeki sanal makine güvenlik açıklarını izleyen beş [Azure İlkesi](../../../policy/overview.md) tanımı atayarak bilgi sistemi güvenlik açıklarını yönetmenize yardımcı olur. Azure Güvenlik Merkezi, dağıtılan Azure kaynaklarının güvenlik durumu hakkında gerçek zamanlı bilgi sahibi olmasını sağlayan raporlama özellikleri sağlar.

- \[Önizleme:\]Azure Güvenlik Merkezi'nde eksik Uç Nokta Koruma'yı izleyin
- \[Önizleme\]: Azure Güvenlik Merkezi'nde eksik sistem güncelleştirmelerini izleme
- \[Önizleme:\]Azure Güvenlik Merkezi'ndeki işletim sistemi güvenlik açıklarını izleyin
- \[Önizleme\]: Azure Güvenlik Merkezi'nde SQL güvenlik açığı değerlendirme sonuçlarını izleyin
- \[Önizleme:\]Azure Güvenlik Merkezi'ndeki VM Güvenlik Açıklarını Izleyin

## <a name="a1262-restrictions-on-software-installation"></a>A.12.6.2 Yazılım yüklemesi ile ilgili kısıtlamalar

Uyarlanabilir uygulama denetimi, Azure Güvenlik Merkezi'nin azure'da bulunan VM'lerinizde hangi uygulamaların çalıştırılamayabileceğini denetlemenize yardımcı olan bir çözümdür. Bu plan, izin verilen uygulamalar kümesindeki değişiklikleri izleyen bir Azure İlkesi tanımı atar. Yazılım yüklemesi ile ilgili kısıtlamalar, yazılım açıklarına sahip olma olasılığını azaltmanıza yardımcı olabilir.

- \[Önizleme\]: Azure Güvenlik Merkezi'nde olası uygulama Beyaz Liste'yi izleyin

## <a name="a1311-network-controls"></a>A.13.1.1 Ağ denetimleri

Bu plan, ağ güvenlik gruplarını izin kurallarıyla izleyen bir [Azure İlkesi](../../../policy/overview.md) tanımı atayarak ağları yönetmenize ve denetlemenize yardımcı olur. Çok izin verilen kurallar istenmeyen ağ erişimine izin verebilir ve gözden geçirilmelidir. Bu plan ayrıca korumasız uç noktaları, uygulamaları ve depolama hesaplarını izleyen üç Azure İlkesi tanımı da atar. Güvenlik duvarı tarafından korunmayan uç noktalar ve uygulamalar ve sınırsız erişime sahip depolama hesapları, bilgi sisteminde bulunan bilgilere istenmeyen erişime izin verebilir.

- \[Önizleme\]: Azure Güvenlik Merkezi'nde izin verilen ağ erişimini izleyin
- \[Önizleme:\]Azure Güvenlik Merkezi'nde korumasız ağ uçnoktalarını izleme
- \[Önizleme:\]Azure Güvenlik Merkezi'nde korumasız web uygulamasını izleyin
- Depolama hesaplarına sınırsız ağ erişimini denetleme

## <a name="a1321-information-transfer-policies-and-procedures"></a>A.13.2.1 Bilgi aktarım politikaları ve prosedürleri

Plan, depolama hesaplarına ve Redis Önbelleği'ne güvenli olmayan bağlantıları denetlemek için iki [Azure İlkesi](../../../policy/overview.md) tanımı atayarak Azure hizmetleriyle bilgi aktarımının güvenli olduğundan emin olmanıza yardımcı olur.

- Redis Önbelleğinize yalnızca güvenli bağlantıların etkinleştirilmesini denetleme
- Depolama hesaplarına güvenli transferi denetleme

## <a name="next-steps"></a>Sonraki adımlar

ISO 27001 planının kontrol eşlemesini gözden geçirdiğiniz için, mimari ve bu örneğin nasıl dağıtılabildiğini öğrenmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [ISO 27001 planı - Genel Bakış](./index.md)
> [ISO 27001 planı - Dağıtım adımları](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Plan yaşam döngüsü](../../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../../how-to/update-existing-assignments.md)öğrenin.