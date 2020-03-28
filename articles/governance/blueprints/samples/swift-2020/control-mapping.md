---
title: SWIFT CSP-CSCF v2020 plan örnek kontrolleri
description: SWIFT CSP-CSCF v2020 plan örneğinin kontrol haritalaması. Her denetim, değerlendirmeye yardımcı olan bir veya daha fazla Azure İlkesiyle eşlenir.
ms.date: 09/24/2019
ms.topic: sample
ms.openlocfilehash: 1826e4820f06fc3a858ce6cd5f8906baaa9808cc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74546522"
---
# <a name="control-mapping-of-the-swift-csp-cscf-v2020-blueprint-sample"></a>SWIFT CSP-CSCF v2020 plan numunesinin kontrol haritalaması

Aşağıdaki makalede, Azure Blueprints SWIFT CSP-CSCF v2020 plan örnek haritalarının SWIFT CSP-CSCF v2020 denetimleri nasıl olduğu ayrıntılı olarak açıklanmaktadır. Denetimler hakkında daha fazla bilgi için [SWIFT CSP-CSCF v2020'ye](https://www.swift.com/myswift/customer-security-programme-csp)bakın.

Aşağıdaki eşlemeler **SWIFT CSP-CSCF v2020** kontrollerine ait. Doğrudan belirli bir denetim eşleleme sine atlamak için sağdaki gezinmeyi kullanın. Eşlenen denetimlerin çoğu bir Azure [İlkesi](../../../policy/overview.md) girişimiyle uygulanır. Girişimin tamamını incelemek için Azure portalında **İlke'yi** açın ve **Tanımlar** sayfasını seçin. Ardından, ** \[Önizleme'yi\]bulun ve seçin : SWIFT CSP-CSCF v2020 denetimlerini denetleyin ve yerleşik politika girişimini denetlemek için belirli VM Uzantıları** dağıtın.

> [!IMPORTANT]
> Aşağıdaki her denetim bir veya daha fazla [Azure İlkesi](../../../policy/overview.md) tanımıyla ilişkilidir. Bu ilkeler, denetime [uygunluğu değerlendirmenize](../../../policy/how-to/get-compliance-data.md) yardımcı olabilir; ancak, genellikle bir denetim ve bir veya daha fazla ilke arasında 1:1 veya tam bir eşleşme yoktur. Bu nedenle, Azure İlkesinde **Uyumlu olmak** yalnızca ilkelerin kendileri için başvurur; bu, bir denetimin tüm gereksinimlerine tam olarak uymanızı sağlamaz. Ayrıca, uyumluluk standardı şu anda herhangi bir Azure İlkesi tanımı tarafından ele alınmayan denetimleri içerir. Bu nedenle, Azure İlkesi'ndeki uyumluluk, genel uyumluluk durumunuza yalnızca kısmi bir görünümdür. Bu uyumluluk planı örneği için denetimler ve Azure İlkesi tanımları arasındaki ilişkilendirmeler zaman içinde değişebilir. Değişiklik geçmişini görüntülemek için [GitHub Commit History'ye](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/swift-2020/control-mapping.md)bakın.

## <a name="12-and-51-account-management"></a>1.2 ve 5.1 Hesap Yönetimi

Bu plan, kuruluşunuzun hesap yönetimi gereksinimlerine uymayan hesapları gözden geçirmenize yardımcı olur. Bu plan, bir abonelik ve amortismana alınan hesaplarda okuma, yazma ve sahip izinleriyle dış hesapları denetleyen [Azure İlkesi](../../../policy/overview.md) tanımları atar. Bu ilkeler tarafından denetlenen hesapları gözden geçirerek, hesap yönetimi gereksinimlerinin karşılandığından emin olmak için uygun eylemi gerçekleştirebilirsiniz.

- Amortismana alınan hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan amortismana alınmış hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Okuma izinleri olan harici hesaplar aboneliğinizden kaldırılmalıdır
- Yazma izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

## <a name="26-51-64-and-65a-account-management--role-based-schemes"></a>2.6, 5.1, 6.4 ve 6.5A Hesap Yönetimi | Rol Tabanlı Şemalar

Azure, [Azure'daki](../../../../role-based-access-control/overview.md) kaynaklara kimlerin erişebilenleri yönetmenize yardımcı olmak için rol tabanlı erişim denetimi (RBAC) uygular. Azure portalını kullanarak, Azure kaynaklarına ve izinlerine kimlerin erişimi olduğunu inceleyebilirsiniz. Bu plan ayrıca, SQL Sunucuları ve Hizmet Dokusu için Azure Active Directory kimlik doğrulamasını denetlemek için [Azure İlkesi](../../../policy/overview.md) tanımlarını da atar. Azure Active Directory kimlik doğrulaması kullanmak, veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin basitleştirilmiş izin yönetimini ve merkezi kimlik yönetimini sağlar. Ayrıca, bu plan, özel RBAC kurallarının kullanımını denetlemek için bir Azure İlkesi tanımı atar. Özel RBAC kurallarının nerede uygulandığını anlamak, özel RBAC kuralları hataya açık olduğundan, gereksinimi ve doğru uygulamayı doğrulamanıza yardımcı olabilir.

- SQL sunucuları için bir Azure Active Directory yöneticisi sağlanmalıdır
- Özel RBAC kurallarının denetim kullanımı
- Hizmet Kumaş kümeleri yalnızca müşteri kimlik doğrulaması için Azure Etkin Dizini kullanmalıdır

## <a name="29a--account-management--account-monitoring--atypical-usage"></a>2.9A Hesap Yönetimi | Hesap İzleme / Atipik Kullanım

Tam zamanında (JIT) sanal makine erişimi, Gelen Trafiği Azure sanal makinelerine kilitleyerek saldırılara maruz kalmamayı azaltırken gerektiğinde VM'lere bağlanmak için kolay erişim sağlar. Sanal makinelere erişmek için tüm JIT istekleri, atipik kullanımı izlemenize olanak tanıyan Etkinlik Günlüğü'nde günlüğe kaydedilir. Bu plan, tam zamanında erişimi destekleyebilen ancak henüz yapılandırılmamış sanal makineleri izlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Sanal makinelere anlık ağ erişim denetimi uygulanmalıdır

## <a name="13-51-and-64-separation-of-duties"></a>1.3, 5.1 ve 6.4 Görev ayrılığı

Yalnızca bir Azure abonelik sahibine sahip olmak, yönetim fazlalığına izin vermez. Tam tersine, çok fazla Azure abonelik sahibine sahip olmak, gizliliği ihlal edilen bir sahip hesabı aracılığıyla ihlal potansiyelini artırabilir. Bu plan, Azure aboneliklerinin sahip sayısını denetleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak uygun sayıda Azure abonelik sahibini korumanıza yardımcı olur. Bu plan ayrıca, Windows sanal makinelerde Yöneticiler grubunun üyeliğini denetlemenize yardımcı olan Azure İlkesi tanımları da atar. Abonelik sahibinin ve sanal makine yöneticisi izinlerinin yönetilmesi, uygun görev ayrımını uygulamanıza yardımcı olabilir.

- Aboneliğiniz için en fazla 3 sahip belirlenmelidir
- Yöneticiler grubunun belirtilen üyelerin tümünü içermediği Windows VM'lerinden gelen denetim sonuçlarını göster
- Yöneticiler grubunun belirtilen üyelerin tümlerini içermediği Windows VM'lerini denetlemek için ön koşulları dağıtma
- Aboneliğinize birden fazla sahip atanmalıdır

## <a name="13-51-and-64-least-privilege--review-of-user-privileges"></a>1.3, 5.1 ve 6.4 En Az Ayrıcalık | Kullanıcı Ayrıcalıklarının Gözden Geçirilmesi

Azure, [Azure'daki](../../../../role-based-access-control/overview.md) kaynaklara kimlerin erişebilenleri yönetmenize yardımcı olmak için rol tabanlı erişim denetimi (RBAC) uygular. Azure portalını kullanarak, Azure kaynaklarına ve izinlerine kimlerin erişimi olduğunu inceleyebilirsiniz. Bu plan, gözden geçirilmesi için öncelik verilmesi gereken denetim hesaplarına [Azure İlkesi](../../../policy/overview.md) tanımları atar. Bu hesap göstergelerinin gözden geçirilmesi, en az ayrıcalık denetimlerinin uygulanmasını sağlamanıza yardımcı olabilir.

- Aboneliğiniz için en fazla 3 sahip belirlenmelidir
- Yöneticiler grubunun belirtilen üyelerin tümünü içermediği Windows VM'lerinden gelen denetim sonuçlarını göster
- Yöneticiler grubunun belirtilen üyelerin tümlerini içermediği Windows VM'lerini denetlemek için ön koşulları dağıtma
- Aboneliğinize birden fazla sahip atanmalıdır

## <a name="22-and-27-security-attributes"></a>2.2 ve 2.7 Güvenlik Özellikleri

Azure SQL Veritabanı için gelişmiş veri güvenliğinin veri bulma ve sınıflandırma yeteneği, veritabanlarınızdaki hassas verileri bulma, sınıflandırma, etiketleme ve koruma özellikleri sağlar. Veri sınıflandırma durumunuz için görünürlük sağlamanın yanı sıra veritabanı içindeki ve dışındaki hassas verilere erişimin izlenmesi için kullanılabilir. Gelişmiş veri güvenliği, kuruluşunuz için uygun güvenlik özellikleriyle ilişkili bilgileri sağlamanıza yardımcı olabilir. Bu plan, SQL sunucusunda gelişmiş veri güvenliğinin kullanımını izlemek ve uygulamak için [Azure İlkesi](../../../policy/overview.md) tanımları atar. 

- SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir
- Gelişmiş Veri Güvenliğini SQL sunucularında dağıtma

## <a name="22-27-41-and-61-remote-access--automated-monitoring--control"></a>2.2, 2.7, 4.1 ve 6.1 Uzaktan Erişim | Otomatik İzleme / Kontrol

Bu plan, Azure Uygulama Hizmeti uygulaması için uzaktan hata ayıklamanın kapalı olduğunu ve parolasız hesaplardan uzaktan bağlantı sağlayan Linux sanal makinelerini denetleyen ilke tanımlarını izlemek için [Azure İlkesi](../../../policy/overview.md) tanımları atayarak uzaktan erişimi izlemenize ve denetlemenize yardımcı olur. Bu plan, depolama hesaplarına sınırsız erişimi izlemenize yardımcı olan bir Azure İlkesi tanımı da atar. Bu göstergelerin izlenmesi, uzaktan erişim yöntemlerinin güvenlik politikanıza uygun olmasını sağlamanıza yardımcı olabilir.

- \[Önizleme\]: Linux VM'lerinden gelen ve parolasız hesaplardan uzaktan bağlantı sağlayan denetim sonuçlarını göster
- \[Önizleme\]: Parola olmayan hesaplardan uzaktan bağlantı sağlayan Linux VM'lerini denetlemek için ön koşulları dağıtma
- Depolama hesaplarına sınırsız ağ erişimini denetleme
- API Uygulaması için uzaktan hata ayıklama kapatılmalıdır
- İşlev Uygulaması için uzaktan hata ayıklama kapatılmalıdır
- Web Uygulaması için uzaktan hata ayıklama kapatılmalıdır

## <a name="13-and-64-content-of-audit-records--centralized-management-of-planned-audit-record-content"></a>1.3 ve 6.4 Denetim Kayıtlarının İçeriği | Planlı Denetim Kayıt İçeriğinin Merkezi Yönetimi

Azure Monitor tarafından toplanan günlük verileri, merkezi yapılandırma ve yönetimi sağlayan bir Log Analytics çalışma alanında depolanır. Bu plan, Azure sanal makinelerinde Log Analytics aracısını denetleyen ve uygulayan [Azure İlkesi](../../../policy/overview.md) tanımları atayarak olayların günlüğe kaydedilmesini sağlamanıza yardımcı olur.

- \[Önizleme\]: Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- \[Önizleme\]: Linux VM Ölçek Setleri (VMSS) için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Windows VM Ölçek Setleri (VMSS) için Günlük Analizi Aracısı Dağıt
- \[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt

## <a name="22-27-and-64-response-to-audit-processing-failures"></a>2.2, 2.7 ve 6.4 Denetim İşleme Hatalarına Yanıt

Bu plan, denetim ve olay günlüğe kaydetme yapılandırmalarını izleyen [Azure İlkesi](../../../policy/overview.md) tanımları atar. Bu yapılandırmaların izlenmesi, bir denetim sistemi hatasının veya yanlış yapılandırmanın bir göstergesi olabilir ve düzeltici eylemde bulunmanıza yardımcı olabilir.

- SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir
- Tanılama ayarını denetle
- DENETIMI SQL sunucusunda dağıtma

## <a name="13-and-64-audit-review-analysis-and-reporting--central-review-and-analysis"></a>1.3 ve 6.4 Denetim İnceleme, Analiz ve Raporlama | Merkezi İnceleme ve Analiz

Azure Monitor tarafından toplanan günlük verileri, merkezi raporlama ve analiz sağlayan bir Log Analytics çalışma alanında depolanır. Bu plan, Azure sanal makinelerinde Log Analytics aracısını denetleyen ve uygulayan [Azure İlkesi](../../../policy/overview.md) tanımları atayarak olayların günlüğe kaydedilmesini sağlamanıza yardımcı olur.

- \[Önizleme\]: Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- \[Önizleme\]: Linux VM Ölçek Setleri (VMSS) için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Windows VM Ölçek Setleri (VMSS) için Günlük Analizi Aracısı Dağıt
- \[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt

## <a name="13-22-27-64-and-65a-audit-generation"></a>1.3, 2.2, 2.7, 6.4 ve 6.5A Denetim Üretimi

Bu plan, Azure kaynaklarında günlük ayarlarını denetleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak sistem olaylarının günlüğe kaydedilmesini sağlamanıza yardımcı olur. Bu ilke tanımları, Log Analytics aracısının Azure sanal makinelerde dağıtımını ve diğer Azure kaynak türleri için denetim ayarlarının yapılandırmasını denetler ve zorlar. Bu ilke tanımları, Azure kaynakları içinde gerçekleştirilen işlemler hakkında bilgi sağlamak için tanılama günlüklerinin yapılandırmasını da denetler. Ayrıca, denetim ve Gelişmiş Veri Güvenliği SQL sunucularında yapılandırılır.

- \[Önizleme\]: Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- \[Önizleme\]: Linux VM Ölçek Setleri (VMSS) için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Windows VM Ölçek Setleri (VMSS) için Günlük Analizi Aracısı Dağıt
- \[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt
- Tanılama ayarını denetle
- DENETIM SQL sunucu düzeyi Denetim ayarları
- SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir
- Gelişmiş Veri Güvenliğini SQL sunucularında dağıtma
- DENETIMI SQL sunucularında dağıtma
- Ağ Güvenlik Grupları için Tanılama Ayarlarını Dağıtma

## <a name="11-least-functionality--prevent-program-execution"></a>1.1 En Az İşlevsellik | Program Yürütülmesini Önleme

Azure Güvenlik Merkezi'ndeki uyarlanabilir uygulama denetimi, belirli yazılımların sanal makinelerinizde çalışmasını engelleyen veya engelleyen akıllı, uçtan uca uygulama beyaz liste çözümüdür. Uygulama denetimi, onaylanmamış uygulamanın çalışmasını yasaklayan bir uygulama modunda çalıştırılabilir. Bu plan, uygulama beyaz listesinin önerildiği ancak henüz yapılmadığını sanal makineleri izlemenize yardımcı olan bir Azure İlkesi tanımı atar.

- Uyarlanabilir Uygulama Denetimleri sanal makinelerde etkinleştirilmelidir

## <a name="11-least-functionality--authorized-software--whitelisting"></a>1.1 En Az İşlevsellik | Yetkili Yazılım / Whitelisting

Azure Güvenlik Merkezi'ndeki uyarlanabilir uygulama denetimi, belirli yazılımların sanal makinelerinizde çalışmasını engelleyen veya engelleyen akıllı, uçtan uca uygulama beyaz liste çözümüdür. Uygulama denetimi, sanal makineleriniz için onaylanmış uygulama listeleri oluşturmanıza yardımcı olur. Bu plan, uygulama beyaz listesinin önerildiği ancak henüz yapılmadığını sanal makineleri izlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Uyarlanabilir Uygulama Denetimleri sanal makinelerde etkinleştirilmelidir

## <a name="11-user-installed-software"></a>1.1 Kullanıcı Yüklü Yazılımlar

Azure Güvenlik Merkezi'ndeki uyarlanabilir uygulama denetimi, belirli yazılımların sanal makinelerinizde çalışmasını engelleyen veya engelleyen akıllı, uçtan uca uygulama beyaz liste çözümüdür. Uygulama denetimi, yazılım kısıtlama ilkeleriyle uyumluluğu zorlamanıza ve izlemenize yardımcı olabilir. Bu plan, uygulama beyaz listesinin önerildiği ancak henüz yapılmadığını sanal makineleri izlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Uyarlanabilir Uygulama Denetimleri sanal makinelerde etkinleştirilmelidir

## <a name="42-identification-and-authentication-organizational-users--network-access-to-privileged-accounts"></a>4.2 Kimlik ve Kimlik Doğrulama (Organizasyonel Kullanıcılar) | Ayrıcalıklı Hesaplara Ağ Erişimi

Bu plan, çok faktörlü kimlik doğrulaması etkin olmayan sahip ve/veya yazma izinlerine [Azure İlkesi](../../../policy/overview.md) tanımları atayarak ayrıcalıklı erişimi kısıtlamanıza ve denetlemenize yardımcı olur. Çok faktörlü kimlik doğrulama, bir parça kimlik doğrulama bilgisi tehlikeye atsa bile hesapların güvende tutulmasına yardımcı olur. Çok faktörlü kimlik doğrulaması etkin leştirilmeden hesapları izleyerek, gizliliği ihlal edilme olasılığı daha yüksek hesapları tanımlayabilirsiniz.

- MFA, aboneliğinizde sahip izni olan hesaplarda etkinleştirilmelidir
- MFA, aboneliğinizde yazma izinleri olan hesaplarda etkinleştirilmelidir

## <a name="42-identification-and-authentication-organizational-users--network-access-to-non-privileged-accounts"></a>4.2 Kimlik ve Kimlik Doğrulama (Organizasyonel Kullanıcılar) | İmtiyazsız Hesaplara Ağ Erişimi

Bu plan, çok faktörlü kimlik doğrulaması etkin olmayan okuma izinleriyle denetim hesaplarına bir [Azure İlkesi](../../../policy/overview.md) tanımı atayarak erişimi kısıtlamanıza ve denetlemenize yardımcı olur. Çok faktörlü kimlik doğrulama, bir parça kimlik doğrulama bilgisi tehlikeye atsa bile hesapların güvende tutulmasına yardımcı olur. Çok faktörlü kimlik doğrulaması etkin leştirilmeden hesapları izleyerek, gizliliği ihlal edilme olasılığı daha yüksek hesapları tanımlayabilirsiniz.

- MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir

## <a name="23-and-41-authenticator-management"></a>2.3 ve 4.1 Kimlik Doğrulama Yönetimi

Bu plan, parolasız hesaplardan uzaktan bağlantılara izin veren ve/veya passwd dosyasında yanlış izinler ayarlanan Linux sanal makinelerini denetleyen [Azure İlkesi](../../../policy/overview.md) tanımları atar. Bu plan ayrıca, Windows sanal makineleri için parola şifreleme türünün yapılandırmasını denetleyen ilke tanımları da atar. Bu göstergeleri izlemek, sistem kimlik doğrulayıcılarının kuruluşunuzun kimlik doğrulama ilkesine uymasını sağlamanıza yardımcı olur.

- \[Önizleme\]: 0644 olarak ayarlanmış passwd dosya izinleri olmayan Linux VM'lerinden gelen denetim sonuçlarını göster
- \[Önizleme\]: 0644 olarak ayarlanmış passwd dosya izinleri olmayan Linux VM'leri denetlemek için gereksinimleri dağıtma
- \[Önizleme\]: Parolaları olmayan hesapları olan Linux VM'lerinin denetim sonuçlarını göster
- \[Önizleme\]: Parolaları olmayan hesapları olan Linux VM'lerini denetlemek için gereksinimleri dağıtma
- \[Önizleme\]: Geri döndürülebilir şifreleme kullanarak parolaları depolamayan Windows VM'lerinden gelen denetim sonuçlarını göster
- \[Önizleme\]: Parolaları ters çevrilebilir şifreleme kullanarak depolamayan Windows VM'lerini denetlemek için gereksinimleri dağıtma

## <a name="23-and-41-authenticator-management--password-based-authentication"></a>2.3 ve 4.1 Kimlik Doğrulama Yönetimi | Parola Tabanlı Kimlik Doğrulama

Bu plan, minimum mukavemet ve diğer parola gereksinimlerini zorlamayan Windows sanal makinelerini denetleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak güçlü parolalar uygulamanıza yardımcı olur. Parola gücü ilkesini ihlal eden sanal makinelerin farkında olmak, tüm sanal makine kullanıcı hesaplarının parolalarının kuruluşunuzun parola ilkesine uygun olmasını sağlamak için düzeltici eylemlerde yardımcı olur.

- \[Önizleme\]: Önceki 24 parolanın yeniden kullanılmasına izin veren Windows VM'lerinden gelen denetim sonuçlarını göster
- \[Önizleme\]: En fazla 70 günlük parola yaşı olmayan Windows VM'lerinden gelen denetim sonuçlarını göster
- \[Önizleme\]: En az parola yaşı 1 gün olmayan Windows VM'lerinden gelen denetim sonuçlarını göster
- \[Önizleme\]: Parola karmaşıklığı ayarı etkin olmayan Windows VM'lerinden gelen denetim sonuçlarını göster
- \[Önizleme\]: En az parola uzunluğunu 14 karakterle sınırlamayan Windows VM'lerinden gelen denetim sonuçlarını göster
- \[Önizleme\]: Geri döndürülebilir şifreleme kullanarak parolaları depolamayan Windows VM'lerinden gelen denetim sonuçlarını göster
- \[Önizleme\]: Önceki 24 parolanın yeniden kullanılmasına izin veren Windows VM'lerini denetlemek için ön koşulları dağıtma
- \[Önizleme\]: En fazla 70 günlük parola yaşı olmayan Windows VM'lerini denetlemek için ön koşulları dağıtma
- \[Önizleme\]: Minimum parola yaşı 1 gün olmayan Windows VM'lerini denetlemek için ön koşulları dağıtma
- \[Önizleme\]: Parola karmaşıklığı ayarı etkin olmayan Windows VM'lerini denetlemek için ön koşulları dağıtma
- \[Önizleme\]: Minimum parola uzunluğunu 14 karakterle sınırlamayan Windows VM'lerini denetlemek için ön koşulları dağıtma
- \[Önizleme\]: Parolaları ters çevrilebilir şifreleme kullanarak depolamayan Windows VM'lerini denetlemek için ön koşulları dağıtma

## <a name="22-and-27-vulnerability-scanning"></a>2.2 ve 2.7 Güvenlik Açığı Taraması

Bu plan, Azure Güvenlik Merkezi'ndeki işletim sistemi güvenlik açıklarını, SQL güvenlik açıklarını ve sanal makine güvenlik açıklarını izleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak bilgi sistemi güvenlik açıklarını yönetmenize yardımcı olur. Azure Güvenlik Merkezi, dağıtılan Azure kaynaklarının güvenlik durumu hakkında gerçek zamanlı bilgi sahibi olmasını sağlayan raporlama özellikleri sağlar. Bu plan ayrıca SQL sunucularında Gelişmiş Veri Güvenliğini denetleyen ve uygulayan ilke tanımları da atar. Gelişmiş veri güvenliği, dağıtılan kaynaklarınızdaki güvenlik açıklarını anlamanıza yardımcı olacak güvenlik açığı değerlendirmesi ve gelişmiş tehdit koruması özelliklerini içeriyordu.

- SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir
- Gelişmiş Veri Güvenliğini SQL sunucularında dağıtma
- Sanal makine ölçeği kümelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir 
- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir

## <a name="13-denial-of-service-protection"></a>1.3 Hizmet Korumasının Reddi

Azure'un dağıtılmış hizmet reddi (DDoS) standart katmanı, temel hizmet katmanıüzerinde ek özellikler ve azaltma özellikleri sağlar. Bu ek özellikler arasında Azure Monitor tümleştirmesi ve saldırı sonrası azaltma raporlarını gözden geçirme olanağı yer almaktadır. Bu plan, DDoS standart katmanı etkinse denetleyen bir [Azure İlkesi](../../../policy/overview.md) tanımı atar. Hizmet katmanları arasındaki yetenek farkını anlamak, Azure ortamınız için hizmet reddi korumalarını gidermek için en iyi çözümü seçmenize yardımcı olabilir.

- DDoS Koruma Standardı etkinleştirilmelidir

## <a name="11-and-61-boundary-protection"></a>1.1 ve 6.1 Sınır Koruması

Bu plan, Azure Güvenlik Merkezi'nde ağ güvenliği grubu sertleştirme önerilerini izleyen bir [Azure İlkesi](../../../policy/overview.md) tanımı atayarak sistem sınırını yönetmenize ve denetlemenize yardımcı olur. Azure Güvenlik Merkezi, Internet'in karşı karşıya olduğu sanal makinelerin trafik modellerini analiz eder ve olası saldırı yüzeyini azaltmak için ağ güvenliği grubu kuralı önerileri sağlar.
Ayrıca, bu plan, korumasız uç noktaları, uygulamaları ve depolama hesaplarını izleyen ilke tanımları da atar. Güvenlik duvarı tarafından korunmayan uç noktalar ve uygulamalar ve sınırsız erişime sahip depolama hesapları, bilgi sisteminde bulunan bilgilere istenmeyen erişime izin verebilir.

- Internet'in sanal makinelerle karşı karşıya olması için Ağ Güvenliği Grubu Kuralları sertleştirilmiş olmalıdır
- Internet'ten erişim bitiş noktasına dönük olarak kısıtlanmalıdır
- Depolama hesaplarına sınırsız ağ erişimini denetleme

## <a name="29a-boundary-protection--access-points"></a>2.9A Sınır Koruması | Erişim Noktaları

Tam zamanında (JIT) sanal makine erişimi, Gelen Trafiği Azure sanal makinelerine kilitleyerek saldırılara maruz kalmamayı azaltırken gerektiğinde VM'lere bağlanmak için kolay erişim sağlar. JIT sanal makine erişimi, Azure'daki kaynaklarınızla harici bağlantı sayısını sınırlamanıza yardımcı olur. Bu plan, tam zamanında erişimi destekleyebilen ancak henüz yapılandırılmamış sanal makineleri izlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Sanal makinelere anlık ağ erişim denetimi uygulanmalıdır

## <a name="29a-boundary-protection--external-telecommunications-services"></a>2.9A Sınır Koruması | Dış Telekomünikasyon Hizmetleri

Tam zamanında (JIT) sanal makine erişimi, Gelen Trafiği Azure sanal makinelerine kilitleyerek saldırılara maruz kalmamayı azaltırken gerektiğinde VM'lere bağlanmak için kolay erişim sağlar. JIT sanal makine erişimi, erişim isteği ve onay işlemlerini kolaylaştırarak trafik akışı politikanızın istisnalarını yönetmenize yardımcı olur. Bu plan, tam zamanında erişimi destekleyebilen ancak henüz yapılandırılmamış sanal makineleri izlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Sanal makinelere anlık ağ erişim denetimi uygulanmalıdır

## <a name="21-24-24a-25a-and-26-transmission-confidentiality-and-integrity--cryptographic-or-alternate-physical-protection"></a>2.1, 2.4, 2.4A, 2.5A ve 2.6 İletim Gizliliği ve Bütünlüğü | Şifreleme veya Alternatif Fiziksel Koruma

Bu plan, iletişim protokolleri için uygulanan şifreleme mekanizmasını izlemenize yardımcı olan [Azure İlkesi](../../../policy/overview.md) tanımları atayarak aktarılan bilgilerin gizli ve bütünlüğünü korumanıza yardımcı olur. İletişimin düzgün şekilde şifrelenmiş olmasını sağlamak, kuruluşunuzun gereksinimlerini karşılamanıza veya bilgileri yetkisiz ifşa ve değişikliklere karşı korumanıza yardımcı olabilir.

- API Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır
- Güvenli iletişim protokolleri kullanmayan Windows web sunucularından gelen denetim sonuçlarını göster
- Güvenli iletişim protokolleri kullanmayan Windows web sunucularını denetlemek için ön koşulları dağıtma
- Fonksiyon Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır
- Yalnızca Redis Önbelleğinize güvenli bağlantılar etkinleştirilmelidir
- Depolama hesaplarına güvenli aktarım etkinleştirilmelidir
- Web Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır

## <a name="22-23-25-41-and-27-protection-of-information-at-rest--cryptographic-protection"></a>2.2, 2.3, 2.5, 4.1 ve 2.7 Dinlenme De Bilginin Korunması | Şifreleme Koruması

Bu plan, belirli şifreleme denetimlerini ve zayıf şifreleme ayarlarının denetim kullanımını zorunlu kılarak [Azure İlkesi](../../../policy/overview.md) tanımları atayarak bilgileri korumak için şifreleme denetimlerinin kullanımına ilişkin ilkenizi uygulamanıza yardımcı olur. Azure kaynaklarınızın en iyi olmayan şifreleme yapılandırmalarına sahip olabileceğini anlamak, kaynakların bilgi güvenliği politikanıza uygun şekilde yapılandırıldığından emin olmak için düzeltici eylemlerde yardımcı olabilir. Özellikle, bu plan tarafından atanan ilke tanımları veri gölü depolama hesapları için şifreleme gerektirir; SQL veritabanlarında saydam veri şifreleme gerektirir; ve SQL veritabanlarında, sanal makine disklerinde ve otomasyon hesabı değişkenlerinde eksik şifrelemeyi denetleyin.

- SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir
- Gelişmiş Veri Güvenliğini SQL sunucularında dağıtma
- SQL DB saydam veri şifrelemesini dağıtma
- SQL veritabanlarında Saydam Veri Şifrelemesi etkinleştirilmelidir

## <a name="13-22-and-27-flaw-remediation"></a>1.3, 2.2 ve 2.7 Kusur Düzeltme

Bu plan, Eksik sistem güncelleştirmelerini, işletim sistemi açıklarını, SQL güvenlik açıklarını ve Azure Güvenlik Merkezi'ndeki sanal makine güvenlik açıklarını izleyen [Azure İlkesi](../../../policy/overview.md) tanımlarını atayarak bilgi sistemi hatalarını yönetmenize yardımcı olur. Azure Güvenlik Merkezi, dağıtılan Azure kaynaklarının güvenlik durumu hakkında gerçek zamanlı bilgi sahibi olmasını sağlayan raporlama özellikleri sağlar. Bu plan ayrıca, sanal makine ölçek kümeleri için işletim sisteminin yama sağlayan bir ilke tanımı atar.

- Sanal Makine Ölçeği Kümelerinde otomatik işletim sistemi görüntü yaması gerektirme
- Sanal makine ölçek kümelerinde sistem güncellemeleri yüklenmelidir
- Sistem güncellemeleri sanal makinelerinize yüklenmeli
- Sanal makine ölçeği kümelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Sanal makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir

## <a name="61-malicious-code-protection"></a>6.1 Kötü Amaçlı Kod Koruması

Bu plan, Azure Güvenlik Merkezi'ndeki sanal makinelerde eksik uç nokta korumasını izleyen ve Windows sanal makinelerde Microsoft kötü amaçlı yazılımdan koruma çözümünü uygulayan [Azure İlkesi](../../../policy/overview.md) tanımları atayarak kötü amaçlı kod koruması da dahil olmak üzere uç nokta korumayı yönetmenize yardımcı olur.

- Windows Server için varsayılan Microsoft IaaaSAntimalware uzantısı dağıtma
- Uç nokta koruma çözümü sanal makine ölçek kümeleri üzerine kurulmalıdır
- Azure Güvenlik Merkezi'nde eksik Uç Nokta Koruması'nı izleme

## <a name="61-malicious-code-protection--central-management"></a>6.1 Kötü Amaçlı Kod Koruması | Merkezi Yönetim

Bu plan, Azure Güvenlik Merkezi'ndeki sanal makinelerde eksik uç nokta korumayı izleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak kötü amaçlı kod koruması da dahil olmak üzere uç nokta korumayı yönetmenize yardımcı olur. Azure Güvenlik Merkezi, dağıtılan Azure kaynaklarının güvenlik durumu hakkında gerçek zamanlı bilgi sahibi olmak için merkezi yönetim ve raporlama özellikleri sağlar.

- Uç nokta koruma çözümü sanal makine ölçek kümeleri üzerine kurulmalıdır
- Azure Güvenlik Merkezi'nde eksik Uç Nokta Koruması'nı izleme

## <a name="11-13-22-27-28-and-64-information-system-monitoring"></a>1.1, 1.3, 2.2, 2.7, 2.8 ve 6.4 Bilgi Sistemi İzleme

Bu plan, Azure kaynaklarında günlüğe kaydetme ve veri güvenliğini denetleyerek ve uygulayarak sisteminizi izlemenize yardımcı olur. Özellikle, ilkeler denetim atanmış ve Log Analytics aracısı dağıtım zorlamak ve SQL veritabanları, depolama hesapları ve ağ kaynakları için gelişmiş güvenlik ayarları. Bu özellikler, uygun eylemi gerçekleştirebilmeniz için anormal davranışları ve saldırı göstergelerini algılamanıza yardımcı olabilir.

- \[Önizleme\]: Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- \[Önizleme\]: Linux VM Ölçek Setleri (VMSS) için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Windows VM Ölçek Setleri (VMSS) için Günlük Analizi Aracısı Dağıt
- \[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt
- SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir
- SQL sunucusu için gelişmiş veri güvenliği ayarları, güvenlik uyarıları almak için bir e-posta adresi içermelidir
- Azure Akış Analizi'ndeki tanılama günlükleri etkinleştirilmelidir
- Gelişmiş Veri Güvenliğini SQL sunucularında dağıtma
- DENETIMI SQL sunucularında dağıtma
- Sanal ağlar oluşturulduğunda ağ izleyicisini dağıtma
- SQL sunucularında Tehdit Algılamayı Dağıtma

## <a name="22-and-28-information-system-monitoring--analyze-traffic--covert-exfiltration"></a>2.2 ve 2.8 Bilgi Sistemi İzleme | Trafiği Analiz Et / Gizli Exfiltration

Azure Depolama için Gelişmiş Tehdit Koruması, depolama hesaplarına erişmek veya bu hesaplardan yararlanmak için olağandışı ve zararlı olabilecek girişimleri algılar. Koruma uyarıları anormal erişim desenleri, anormal ekstreler/yüklemeler ve şüpheli depolama etkinlikleriiçerir. Bu göstergeler, gizli bilgi sızma nızı tespit etmenize yardımcı olabilir.

- SQL sunucularında Tehdit Algılamayı Dağıtma

> [!NOTE]
> Azure İlkesi tanımlarının kullanılabilirliği Azure Kamu ve diğer ulusal bulutlarda farklılık gösterebilir.

## <a name="next-steps"></a>Sonraki adımlar

SWIFT CSP-CSCF v2020 planının kontrol eşleğini incelediğiniz için, plan ve bu örneğin nasıl dağıtılabildiğini öğrenmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [SWIFT CSP-CSCF v2020 planı - Hızlı](./index.md)
> [CSP-CSCF v2020 planı](./deploy.md) genel bakış - Dağıtım adımları

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Plan yaşam döngüsü](../../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../../how-to/update-existing-assignments.md)öğrenin.
