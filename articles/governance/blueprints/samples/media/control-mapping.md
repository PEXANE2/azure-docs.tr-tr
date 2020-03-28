---
title: Ortam planı örnek denetimleri
description: Medya plan örneklerinin kontrol eşlemi. Her denetim, değerlendirmeye yardımcı olan bir veya daha fazla Azure İlkesiyle eşlenir.
ms.date: 02/24/2020
ms.topic: sample
ms.openlocfilehash: bdc4ef65c6d61a9d4217d7873bb29bd787842e71
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78201909"
---
# <a name="control-mapping-of-the-media-blueprint-sample"></a>Medya planı örneğinin kontrol eşlemi

Aşağıdaki makalede, Azure Blueprints Media planı örnek haritaların Medya denetimleri nasıl açıklanmaktadır. Denetimler hakkında daha fazla bilgi için [Medya'ya](https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html)bakın.

Aşağıdaki eşlemeler **Medya** denetimleri içindir. Doğrudan belirli bir denetim eşleleme sine atlamak için sağdaki gezinmeyi kullanın. Eşlenen denetimlerin çoğu bir Azure [İlkesi](../../../policy/overview.md) girişimiyle uygulanır. Girişimin tamamını incelemek için Azure portalında **İlke'yi** açın ve **Tanımlar** sayfasını seçin. Ardından ** \[\]Önizleme:Audit Media denetimleri** yerleşik ilke girişimini bulun ve seçin.

> [!IMPORTANT]
> Aşağıdaki her denetim bir veya daha fazla [Azure İlkesi](../../../policy/overview.md) tanımıyla ilişkilidir. Bu ilkeler, denetime [uygunluğu değerlendirmenize](../../../policy/how-to/get-compliance-data.md) yardımcı olabilir; ancak, genellikle bir denetim ve bir veya daha fazla ilke arasında 1:1 veya tam bir eşleşme yoktur. Bu nedenle, Azure İlkesinde **Uyumlu olmak** yalnızca ilkelerin kendileri için başvurur; bu, bir denetimin tüm gereksinimlerine tam olarak uymanızı sağlamaz. Ayrıca, uyumluluk standardı şu anda herhangi bir Azure İlkesi tanımı tarafından ele alınmayan denetimleri içerir. Bu nedenle, Azure İlkesi'ndeki uyumluluk, genel uyumluluk durumunuza yalnızca kısmi bir görünümdür. Bu uyumluluk planı örneği için denetimler ve Azure İlkesi tanımları arasındaki ilişkilendirmeler zaman içinde değişebilir. Değişiklik geçmişini görüntülemek için [GitHub Commit History'ye](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/media/control-mapping.md)bakın.

## <a name="access-control"></a>Erişim Denetimi

### <a name="ac-11--ensure-no-root-access-key-exists"></a>AC-1.1- Kök erişim anahtarının bulunmadığından emin olun

- \[Önizleme\]: Güvenilen Kök'te belirtilen sertifikaları içermeyen Windows VM'lerini denetlemek için ön koşulları dağıtma

### <a name="ac-12---passwords-pins-and-tokens-must-be-protected"></a>AC-1.2 - Parolalar, PIN'ler ve Jetonlar korunmalı

- \[Önizleme\]: Minimum parola uzunluğunu 14 karakterle sınırlamayan Windows VM'lerini denetlemek için ön koşulları dağıtma

### <a name="ac-18---shared-account-access-is-prohibited"></a>AC-1.8 - Paylaşılan hesap erişimi yasaktır

- RootManageSharedAccessKey dışındaki tüm yetkilendirme kuralları Hizmet Veri Günü ad alanından kaldırılmalıdır

### <a name="ac-19--system-must-restrict-access-to-authorized-users"></a>AC-1.9 -Sistem yetkili kullanıcılara erişimi kısıtlamalıdır.

- Depolama hesaplarına sınırsız ağ erişimini denetleme

### <a name="ac--114--system-must-enforce-access-rights"></a>AC- 1.14 -Sistem erişim haklarını uygulamalıdır.

- \[Önizleme\]: 'Kullanıcı Hakları Ataması'nda Windows VM yapılandırmalarını denetlemek için ön koşulları dağıtma

### <a name="ac--115--prevent-unauthorized-access-to-security-relevant-information-or-functions"></a>AC- 1.15 -Güvenlikle ilgili bilgilere veya işlevlere yetkisiz erişimi engelleyin.

- \[Önizleme\]: Windows VM yapılandırmalarından gelen denetim sonuçlarını 'Güvenlik Seçenekleri - Sistem ayarları' adlı dosyada göster

### <a name="ac-1-21---separation-of-duties-must-be-enforced-through-appropriate-assignment-of-role"></a>AC-1-21 - Görevlerin ayrılması, uygun görev ataması ile yapılmalıdır.

- [Önizleme\]: Rol Tabanlı Erişim Kontrolü (RBAC) Kubernetes Hizmetlerinde kullanılmalıdır

### <a name="ac-140--ensure-that-systems-are-not-connecting-trusted-network-and-untrusted-networks-at-the-same-time"></a>AC-1.40- Sistemlerin güvenilir ağı ve güvenilmeyen ağları aynı anda bağlamadığından emin olun.

- \[Önizleme\]: 'Güvenlik Seçenekleri - Ağ Erişimi' (Güvenlik Seçenekleri - Ağ Erişimi) 'de Windows VM yapılandırmalarını denetlemek için ön koşulları dağıtma

### <a name="ac-142--ac--143---remote-access-for-non-employees-must-be-restricted-to-allow-access-only-to-specifically-approved-information-systems"></a>AC-1.42 & AC- 1.43 - Çalışan olmayanlar için uzaktan erişim, yalnızca özel olarak onaylanmış bilgi sistemlerine erişime izin verecek şekilde sınırlandırılmalıdır

- \[Önizleme\]: Linux VM'lerinden gelen ve parolasız hesaplardan uzaktan bağlantı sağlayan denetim sonuçlarını göster

### <a name="ac-150--log-security-related-events-for-all-information-system-components"></a>AC-1.50- Tüm bilgi sistemi bileşenleri için güvenlikle ilgili olayları günlüğe kaydedin.

- Logic Apps'taki tanılama günlükleri etkinleştirilmeli

### <a name="ac-154--ensure-multi-factor-authentication-mfa-is-enabled-for-all-cloud-console-users"></a>AC-1.54- Tüm bulut konsolu kullanıcıları için çok faktörlü kimlik doğrulamanın (MFA) etkin olduğundan emin olun.

- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir
- Çok Faktörlü Kimlik Doğrulama (MFA), hesap veya kaynak ihlalini önlemek için yazma ayrıcalığına sahip tüm abonelik hesapları için etkinleştirilmelidir.

## <a name="auditing--logging"></a>Denetim & Günlük

### <a name="al-21--successful-and-unsuccessful-events-must-be-logged"></a>AL-2.1- Başarılı ve başarısız olaylar günlüğe kaydedilmelidir.

- Arama hizmetlerinde tanılama günlükleri etkinleştirilmelidir

### <a name="al--216---network-devicesinstances-must-log-any-event-classified-as-a-critical-security-event-by-that-network-deviceinstance-elbs-web-application-firewalls-etc"></a>AL -2.16 - Ağ aygıtları/örnekleri, kritik bir güvenlik olayı olarak sınıflandırılan her olayı bu ağ aygıtı/örneği (ELB'ler, web uygulaması güvenlik duvarları, vb.) tarafından kaydetmelidir.

- \[Önizleme\]: Windows VM yapılandırmalarından gelen denetim sonuçlarını 'Güvenlik Seçenekleri - Hesaplar' adlı nda göster

### <a name="al-217--serversinstances-must-log-any-event-classified-as-a-critical-security-event-by-that-serverinstance"></a>AL-2.17- Sunucular/örnekler, kritik güvenlik olayı olarak sınıflandırılan her türlü olayı o sunucu/örnek tarafından

- \[Önizleme\]: Windows VM yapılandırmalarından gelen denetim sonuçlarını 'Güvenlik Seçenekleri - Hesaplar' adlı nda göster

### <a name="al-219---domain-events-must-log-any-event-classified-as-a-critical-or-high-security-event-by-the-domain-management-software"></a>AL-2.19 - Etki alanı olayları, etki alanı yönetimi yazılımı tarafından kritik veya yüksek güvenlikli bir olay olarak sınıflandırılan tüm olayları kaydetmelidir

- \[Önizleme\]: Windows VM yapılandırmalarından gelen denetim sonuçlarını 'Güvenlik Seçenekleri - Hesaplar' adlı nda göster
- \[Önizleme\]: 'Güvenlik Seçenekleri - Microsoft Network Client' 'da Windows VM yapılandırmalarını denetlemek için ön koşulları dağıtma

### <a name="al-220--domain-events-must-log-any-event-classified-as-a-critical-security-event-by-domain-security-controls"></a>AL-2.20- Etki alanı olayları, etki alanı güvenlik denetimleri tarafından kritik güvenlik olayı olarak sınıflandırılan tüm olayları kaydetmelidir

- \[Önizleme\]: Windows VM yapılandırmalarından gelen denetim sonuçlarını 'Güvenlik Seçenekleri - Hesaplar' adlı nda göster

### <a name="al-221--domain-events-must-log-any-access-or-changes-to-the-domain-log"></a>AL-2.21- Etki alanı olayları, etki alanı günlüğüne herhangi bir erişimi veya değişiklikleri kaydetmelidir

- \[Önizleme\]: Windows VM yapılandırmalarından gelen denetim sonuçlarını 'Güvenlik Seçenekleri - Kurtarma konsolu' nda göster

## <a name="cryptographic-controls"></a>Şifreleme Denetimleri

### <a name="cc-42--applications-and-systems-must-use-current-cryptographic-solutions-for-protecting-data"></a>CC-4.2- Uygulamalar ve sistemler verileri korumak için güncel şifreleme çözümlerini kullanmalıdır.

- SQL veritabanlarında Saydam Veri Şifrelemesi etkinleştirilmelidir
- Veri-istirahati korumak ve uyumluluk gereksinimlerini karşılamak için saydam veri şifreleme etkinleştirilmelidir

### <a name="cc-45--digital-certificates-must-be-signed-by-an-approved-certificate-authority"></a>CC-4.5- Dijital Sertifikalar onaylı bir Sertifika Yetkilisi tarafından imzalanmalıdır.

- \[Önizleme\]: Belirtilen gün sayısı içinde süresi dolan sertifikaları içeren Windows VM'lerinden gelen denetim sonuçlarını göster

### <a name="cc-46--digital-certificates-must-be-uniquely-assigned-to-a-user-or-device"></a>CC-4.6- Dijital Sertifikalar bir kullanıcıya veya cihaza benzersiz bir şekilde atanmalıdır.

- \[Önizleme\]: Belirtilen gün sayısı içinde süresi dolan sertifikaları içeren Windows VM'lerini denetlemek için ön koşulları dağıtma

### <a name="cc-47--cryptographic-material-must-be-stored-to-enable-decryption-of-the-records-for-the-length-of-time-the-records-are-retained"></a>CC-4.7- Kayıtların saklandığı süre boyunca kayıtların şifresinin çözülmesini sağlamak için şifreleme materyali saklanmalıdır.

- Disk şifreleme sanal makinelerde uygulanmalıdır
- Etkin bir disk şifrelemesi olmayan VM'ler Azure Güvenlik Merkezi tarafından öneriler olarak izlenir

### <a name="cc-48--secret-and-private-keys-must-be-stored-securely"></a>CC-4.8- Gizli ve özel anahtarlar güvenli bir şekilde saklanmalıdır.

- SQL veritabanlarında Saydam Veri Şifrelemesi etkinleştirilmelidir
- Veri-istirahati korumak ve uyumluluk gereksinimlerini karşılamak için saydam veri şifreleme etkinleştirilmelidir

## <a name="change--config-management"></a>Config Yönetimi & Değişim

### <a name="cm-52--only-authorized-users-may-implement-approved-changes-on-the-system"></a>CM-5.2- Sistemde yalnızca yetkili kullanıcılar onaylı değişiklikleri uygulayabilir.

- Makinelerinize sistem güncelleştirmeleri yüklenmelidir
- Sunucularınızdaki eksik güvenlik sistemi güncelleştirmeleri, öneriler olarak Azure Güvenlik Merkezi tarafından izlenir

### <a name="cm-512--maintain-an-up-to-date-complete-accurate-and-readily-available-baseline-configuration-of-the-information-system"></a>CM-5.12- Bilgi sisteminin güncel, eksiksiz, doğru ve hazır bir temel yapılandırmasını koruyun.

- Makinelerinize sistem güncelleştirmeleri yüklenmelidir
- Sunucularınızdaki eksik güvenlik sistemi güncelleştirmeleri, öneriler olarak Azure Güvenlik Merkezi tarafından izlenir

### <a name="cm-513--employ-automated-tools-to-maintain-a-baseline-configuration-of-the-information-system"></a>CM-5.13- Bilgi sisteminin temel yapılandırmasını korumak için otomatik araçlar dan sıyrık kullanır.

- Makinelerinize sistem güncelleştirmeleri yüklenmelidir
- Sunucularınızdaki eksik güvenlik sistemi güncelleştirmeleri, öneriler olarak Azure Güvenlik Merkezi tarafından izlenir

### <a name="cm-514--identify-and-disable-unnecessary-andor-non-secure-functions-ports-protocols-and-services"></a>CM-5.14- Gereksiz ve/veya güvenli olmayan işlevleri, bağlantı noktalarını, protokolleri ve hizmetleri tanımlayın ve devre dışı kılabilir.

- Ağ arabirimleri IP iletmedevreni devre dışı
- \[Önizleme\]: Sanal makinenizdeki IP Yönlendirme devre dışı bırakılmalı

### <a name="cm-519--monitor-changes-to-the-security-configuration-settings"></a>CM-5.19- Güvenlik yapılandırma ayarlarındaki değişiklikleri izleyin.

- Ağ Güvenlik Grupları için Tanılama Ayarlarını Dağıtma

### <a name="cm-522--ensure-that-only-authorized-software-and-updates-are-installed-on-company-systems"></a>CM-5.22- Şirket sistemlerine yalnızca yetkili yazılım ve güncellemelerin yüklendiğinden emin olun.

- Makinelerinize sistem güncelleştirmeleri yüklenmelidir
- Sunucularınızdaki eksik güvenlik sistemi güncelleştirmeleri, öneriler olarak Azure Güvenlik Merkezi tarafından izlenir

## <a name="identity--authentication"></a>Kimlik & Kimlik Doğrulama

### <a name="ia-71--user-accounts-must-be-uniquely-assigned-to-individuals-for-access-to-information-that-is-not-classified-as-public-account-ids-must-be-constructed-using-a-standardized-logical-format"></a>IA-7.1- Kullanıcı hesapları, Genel olarak sınıflandırılmamış bilgilere erişmek için bireylere benzersiz bir şekilde atanmalıdır. Hesap işleri standart bir mantıksal biçim kullanılarak oluşturulmalıdır.

- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- İzlenmeyen erişimi önlemek için, sahip izinlerine sahip harici hesaplar aboneliğinizden kaldırılmalıdır.

## <a name="network-security"></a>Ağ Güvenliği

### <a name="ns-92--access-to-network-device-management-functionality-is-restricted-to-authorized-users"></a>NS-9.2- Ağ aygıtı yönetimi işlevine erişim yetkili kullanıcılarla sınırlıdır.

- \[Önizleme\]: 'Güvenlik Seçenekleri - Ağ Erişimi' (Güvenlik Seçenekleri - Ağ Erişimi) 'de Windows VM yapılandırmalarını denetlemek için ön koşulları dağıtma

### <a name="ns-93--all-network-devices-must-be-configured-using-their-most-secure-configurations"></a>NS-9.3- Tüm ağ aygıtları en güvenli yapılandırmaları kullanılarak yapılandırılmalıdır.

- \[Önizleme\]: 'Güvenlik Seçenekleri - Ağ Erişimi' (Güvenlik Seçenekleri - Ağ Erişimi) 'de Windows VM yapılandırmalarını denetlemek için ön koşulları dağıtma

### <a name="ns-95--all-network-connections-to-a-system-through-a-firewall-must-be-approved-and-audited-on-a-regular-basis"></a>NS-9.5- Güvenlik duvarı aracılığıyla bir sisteme yapılan tüm ağ bağlantılarının düzenli olarak onaylanması ve denetlenilmesi gerekir.

- \[Önizleme\]: Windows VM yapılandırmalarından gelen denetim sonuçlarını 'Windows Güvenlik Duvarı Özellikleri'nde göster

### <a name="ns-97--appropriate-controls-must-be-present-at-any-boundary-between-a-trusted-network-and-any-untrusted-or-public-network"></a>NS-9.7- Uygun denetimler, güvenilir bir ağ ile güvenilmeyen veya ortak ağ arasındaki herhangi bir sınırda bulunmalıdır.

- \[Önizleme\]: 'Windows Güvenlik Duvarı Özellikleri'ndeki Windows VM yapılandırmalarını denetlemek için ön koşulları dağıtma

## <a name="security-planning"></a>Güvenlik Planlama

### <a name="sp-113--threats-must-be-identified-that-could-negatively-impact-the-confidentiality-integrity-or-availability-of-company-information-and-content-along-with-the-likelihood-of-their-occurrence"></a>SP-11.3- Şirket bilgilerinin ve içeriğinin gizliliğini, bütünlüğünü veya kullanılabilirliğini olumsuz etkileyebilecek tehditler ve bunların oluşma olasılığı belirlenmelidir.

- Gelişmiş Tehdit Koruması türleri SQL yönetilen örnek Gelişmiş Veri Güvenliği ayarlarında 'Tümü' olarak ayarlanmalıdır

### <a name="security-continuity"></a>Güvenlik Sürekliliği

## <a name="sc-125--data-in-long-term-storage-must-be-accessible-throughout-the-retention-period-and-protected-against-media-degradation-and-technology-changes"></a>SC-12.5- Uzun süreli depolamadaki verilere saklama süresi boyunca erişilebilir ve ortam bozulması ve teknoloji değişikliklerine karşı korunmalıdır.

- SQL sunucuları, 90 günden uzun denetim bekletme günleriyle yapılandırılmalıdır.
- Denetim SQL sunucuları 90 günden az bir denetim bekletme süresi ile yapılandırıldı.

## <a name="system-integrity"></a>Sistem Bütünlüğü

### <a name="si-143--only-authorized-personnel-may-monitor-network-and-user-activities"></a>SI-14.3- Sadece yetkili personel ağ ve kullanıcı faaliyetlerini izleyebilir.

- SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir
- Güvenlik Açığı Değerlendirmesini izleyin, veritabanı güvenlik açıklarını nasıl düzeltebileceğinize ilişkin sonuçları ve önerileri tarayın.

### <a name="si-144--internet-facing-systems-must-have-intrusion-detection"></a>SI-14.4- İnternete bakan sistemlerde izinsiz giriş tespiti yapılmalıdır.

- SQL sunucularında Tehdit Algılamayı Dağıtma

### <a name="si-1413--standardized-centrally-managed-anti-malware-software-should-be-implemented-across-the-company"></a>SI-14.13- Standart laştırılmış merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı şirket genelinde uygulanmalıdır.

- Windows Server için varsayılan Microsoft IaaaSAntimalware uzantısı dağıtma

### <a name="si-1414--anti-malware-software-must-scan-computers-and-media-weekly-at-a-minimum"></a>SI-14.14- Kötü amaçlı yazılımdan koruma yazılımları bilgisayarları ve medyayı haftalık olarak en az tarasıalmalıdır.

- Windows Server için varsayılan Microsoft IaaaSAntimalware uzantısı dağıtma

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

### <a name="vm-154--ensure-that-applications-are-scanned-for-vulnerabilities-on-a-monthly-basis"></a>VM-15.4- Uygulamaların aylık olarak güvenlik açıklarına karşı tarandığından emin olun.

- Sanal makine ölçeği kümelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Sanal makine ölçeğinizdeki işletim sistemi açıklarını denetimiz ve bunları saldırılardan koruyun.

### <a name="vm-155--ensure-that-vulnerabilities-are-identified-paired-to-threats-and-evaluated-for-risk"></a>VM-15.5- Güvenlik açıklarının tanımlandığından, tehditlerle eşleştirilmesini ve risk açısından değerlendirilmesini sağlayın.

- Sanal makine ölçeği kümelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Sanal makine ölçeğinizdeki işletim sistemi açıklarını denetimiz ve bunları saldırılardan koruyun.

### <a name="vm-156--ensure-that-identified-vulnerabilities-have-been-remediated-within-a-mutually-agreed-upon-timeline"></a>VM-15.6- Tanımlanan güvenlik açıklarının, üzerinde anlaştığımız zaman çizelgesi içinde düzeltildiğini sağlayın.

- Sanal makine ölçeği kümelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Sanal makine ölçeğinizdeki işletim sistemi açıklarını denetimiz ve bunları saldırılardan koruyun.

### <a name="vm-157--access-to-and-use-of-vulnerability-management-systems-must-be-restricted-to-authorized-personnel"></a>VM-15.7- Güvenlik açığı yönetim sistemlerine erişim ve bunların kullanımı yetkili personelle sınırlandırılmalıdır.

- Sanal makine ölçeği kümelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Sanal makine ölçeğinizdeki işletim sistemi açıklarını denetimiz ve bunları saldırılardan koruyun.

> [!NOTE]
> Azure İlkesi tanımlarının kullanılabilirliği Azure Kamu ve diğer ulusal bulutlarda farklılık gösterebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Medya planı örneğinin kontrol eşlemi'ni incelediniz. Ardından, genel bakış ve bu örneğin nasıl dağıtılacak hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="next step action"]
> [Medya planı - Genel Bakış](./control-mapping.md)
> [Medya planı - Dağıtım adımları](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Plan yaşam döngüsü](../../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../../how-to/update-existing-assignments.md)öğrenin.
