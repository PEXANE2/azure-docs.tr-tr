---
title: FedRAMP Yüksek plan örnek kontrolleri
description: FedRAMP Yüksek plan örneğinin kontrol haritalaması. Her denetim, değerlendirmeye yardımcı olan bir veya daha fazla Azure İlkesiyle eşlenir.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: cceca23e4bdc749c553eaf41b5f9599be3c9bf7d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77150621"
---
# <a name="control-mapping-of-the-fedramp-high-blueprint-sample"></a>FedRAMP Yüksek plan örneğinin kontrol haritalaması

Aşağıdaki makalede, Azure Blueprints FedRAMP Yüksek plan örnek haritalarının FedRAMP High denetimlerine nasıl eşlendiği ayrıntılı olarak anlatılıyor. Denetimler hakkında daha fazla bilgi için [FedRAMP Güvenlik Denetimleri Taban Çizgisi'ne](https://www.fedramp.gov/)bakın.

Aşağıdaki haritalamalar **FedRAMP Yüksek** kontrolleri içindir. Doğrudan belirli bir denetim eşleleme sine atlamak için sağdaki gezinmeyi kullanın. Eşlenen denetimlerin çoğu bir Azure [İlkesi](../../../policy/overview.md) girişimiyle uygulanır. Girişimin tamamını incelemek için Azure portalında **İlke'yi** açın ve **Tanımlar** sayfasını seçin. Ardından, ** \[Önizleme'yi\]bulun ve seçin: Audit FedRAMP Yüksek denetimleri ve yerleşik politika girişimini desteklemek için belirli VM Uzantıları dağıtın.**

> [!IMPORTANT]
> Aşağıdaki her denetim bir veya daha fazla [Azure İlkesi](../../../policy/overview.md) tanımıyla ilişkilidir. Bu ilkeler, denetime [uygunluğu değerlendirmenize](../../../policy/how-to/get-compliance-data.md) yardımcı olabilir; ancak, genellikle bir denetim ve bir veya daha fazla ilke arasında 1:1 veya tam bir eşleşme yoktur. Bu nedenle, Azure İlkesinde **Uyumlu olmak** yalnızca ilkelerin kendileri için başvurur; bu, bir denetimin tüm gereksinimlerine tam olarak uymanızı sağlamaz. Ayrıca, uyumluluk standardı şu anda herhangi bir Azure İlkesi tanımı tarafından ele alınmayan denetimleri içerir. Bu nedenle, Azure İlkesi'ndeki uyumluluk, genel uyumluluk durumunuza yalnızca kısmi bir görünümdür. Bu uyumluluk planı örneği için denetimler ve Azure İlkesi tanımları arasındaki ilişkilendirmeler zaman içinde değişebilir.
> Değişiklik geçmişini görüntülemek için [GitHub Commit History'ye](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/fedramp-h/control-mapping.md)bakın.

## <a name="ac-2-account-management"></a>AC-2 Hesap Yönetimi

Bu plan, kuruluşunuzun hesap yönetimi gereksinimlerine uymayan hesapları gözden geçirmenize yardımcı olur. Bu plan, bir abonelik ve amortismana alınan hesaplarda okuma, yazma ve sahip izinleriyle dış hesapları denetleyen [Azure İlkesi](../../../policy/overview.md) tanımları atar. Bu ilkeler tarafından denetlenen hesapları gözden geçirerek, hesap yönetimi gereksinimlerinin karşılandığından emin olmak için uygun eylemi gerçekleştirebilirsiniz.

- Amortismana alınan hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan amortismana alınmış hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Okuma izinleri olan harici hesaplar aboneliğinizden kaldırılmalıdır
- Yazma izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

## <a name="ac-2-7-account-management--role-based-schemes"></a>AC-2 (7) Hesap Yönetimi | Rol Tabanlı Şemalar

Azure, [Azure'daki](../../../../role-based-access-control/overview.md) kaynaklara kimlerin erişebilenleri yönetmenize yardımcı olmak için rol tabanlı erişim denetimi (RBAC) uygular. Azure portalını kullanarak, Azure kaynaklarına ve izinlerine kimlerin erişimi olduğunu inceleyebilirsiniz. Bu plan ayrıca, SQL Sunucuları ve Hizmet Dokusu için Azure Active Directory kimlik doğrulamasını denetlemek için [Azure İlkesi](../../../policy/overview.md) tanımlarını da atar. Azure Active Directory kimlik doğrulaması kullanmak, veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin basitleştirilmiş izin yönetimini ve merkezi kimlik yönetimini sağlar. Ayrıca, bu plan, özel RBAC kurallarının kullanımını denetlemek için bir Azure İlkesi tanımı atar. Özel RBAC kurallarının nerede uygulandığını anlamak, özel RBAC kuralları hataya açık olduğundan, gereksinimi ve doğru uygulamayı doğrulamanıza yardımcı olabilir.

- SQL sunucuları için bir Azure Active Directory yöneticisi sağlanmalıdır
- Özel RBAC kurallarının denetim kullanımı
- Hizmet Kumaş kümeleri yalnızca müşteri kimlik doğrulaması için Azure Etkin Dizini kullanmalıdır

## <a name="ac-2-12-account-management--account-monitoring--atypical-usage"></a>AC-2 (12) Hesap Yönetimi | Hesap İzleme / Atipik Kullanım

Tam zamanında (JIT) sanal makine erişimi, Gelen Trafiği Azure sanal makinelerine kilitleyerek saldırılara maruz kalmamayı azaltırken gerektiğinde VM'lere bağlanmak için kolay erişim sağlar. Sanal makinelere erişmek için tüm JIT istekleri, atipik kullanımı izlemenize olanak tanıyan Etkinlik Günlüğü'nde günlüğe kaydedilir. Bu plan, tam zamanında erişimi destekleyebilen ancak henüz yapılandırılmamış sanal makineleri izlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Sanal makinelere anlık ağ erişim denetimi uygulanmalıdır

## <a name="ac-4-information-flow-enforcement"></a>AC-4 Bilgi Akışı Uygulama

Çapraz kaynak paylaşımı (CORS), Uygulama Hizmetleri kaynaklarının dışarıdan bir etki alanından istenmesine izin verebilir. Microsoft, yalnızca gerekli etki alanlarının API' n, işleviniz ve web uygulamalarınızla etkileşimkurabilmesine izin vermenizi önerir. Bu plan, Azure Güvenlik Merkezi'ndeki CORS kaynaklarına erişim kısıtlamalarını izlemenize yardımcı olmak için bir [Azure İlkesi](../../../policy/overview.md) tanımı atar. CORS uygulamalarını anlamak, bilgi akışı denetimlerinin uygulandığını doğrulamanıza yardımcı olabilir.

- CORS her kaynağın Web Uygulamanıza erişmesine izin vermemelidir

## <a name="ac-5-separation-of-duties"></a>AC-5 Görev Ayrılığı

Yalnızca bir Azure abonelik sahibine sahip olmak, yönetim fazlalığına izin vermez. Tam tersine, çok fazla Azure abonelik sahibine sahip olmak, gizliliği ihlal edilen bir sahip hesabı aracılığıyla ihlal potansiyelini artırabilir. Bu plan, Azure aboneliklerinin sahip sayısını denetleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak uygun sayıda Azure abonelik sahibini korumanıza yardımcı olur. Bu plan ayrıca, Windows sanal makinelerde Yöneticiler grubunun üyeliğini denetlemenize yardımcı olan Azure İlkesi tanımları da atar. Abonelik sahibinin ve sanal makine yöneticisi izinlerinin yönetilmesi, uygun görev ayrımını uygulamanıza yardımcı olabilir.

- Aboneliğiniz için en fazla 3 sahip belirlenmelidir
- Yöneticiler grubunun belirtilen üyelerden herhangi birini içerdiği Windows VM'lerini denetleme
- Yöneticiler grubunun belirtilen üyelerin tümlerini içermediği Windows VM'lerini denetleme
- Yöneticiler grubunun belirtilen üyelerden herhangi birini içerdiği Windows VM'lerini denetlemek için gereksinimleri dağıtma
- Yöneticiler grubunun belirtilen üyelerin tümlerini içermediği Windows VM'lerini denetlemek için gereksinimleri dağıtma
- Aboneliğinize birden fazla sahip atanmalıdır

## <a name="ac-6-7-least-privilege--review-of-user-privileges"></a>AC-6 (7) En Az Ayrıcalık | Kullanıcı Ayrıcalıklarının Gözden Geçirilmesi

Azure, [Azure'daki](../../../../role-based-access-control/overview.md) kaynaklara kimlerin erişebilenleri yönetmenize yardımcı olmak için rol tabanlı erişim denetimi (RBAC) uygular. Azure portalını kullanarak, Azure kaynaklarına ve izinlerine kimlerin erişimi olduğunu inceleyebilirsiniz. Bu plan, gözden geçirilmesi için öncelik verilmesi gereken denetim hesaplarına [Azure İlkesi](../../../policy/overview.md) tanımları atar. Bu hesap göstergelerinin gözden geçirilmesi, en az ayrıcalık denetimlerinin uygulanmasını sağlamanıza yardımcı olabilir.

- Aboneliğiniz için en fazla 3 sahip belirlenmelidir
- Yöneticiler grubunun belirtilen üyelerden herhangi birini içerdiği Windows VM'lerini denetleme
- Yöneticiler grubunun belirtilen üyelerin tümlerini içermediği Windows VM'lerini denetleme
- Yöneticiler grubunun belirtilen üyelerden herhangi birini içerdiği Windows VM'lerini denetlemek için gereksinimleri dağıtma
- Yöneticiler grubunun belirtilen üyelerin tümlerini içermediği Windows VM'lerini denetlemek için gereksinimleri dağıtma
- Aboneliğinize birden fazla sahip atanmalıdır

## <a name="ac-17-1-remote-access--automated-monitoring--control"></a>AC-17 (1) Uzaktan Erişim | Otomatik İzleme / Kontrol

Bu plan, Azure Uygulama Hizmeti uygulaması için uzaktan hata ayıklamanın kapalı olduğunu ve parolasız hesaplardan uzaktan bağlantı sağlayan Linux sanal makinelerini denetleyen ilke tanımlarını izlemek için [Azure İlkesi](../../../policy/overview.md) tanımları atayarak uzaktan erişimi izlemenize ve denetlemenize yardımcı olur. Bu plan, depolama hesaplarına sınırsız erişimi izlemenize yardımcı olan bir Azure İlkesi tanımı da atar. Bu göstergelerin izlenmesi, uzaktan erişim yöntemlerinin güvenlik politikanıza uygun olmasını sağlamanıza yardımcı olabilir.

- \[Önizleme\]: Parolasız hesaplardan uzaktan bağlantı sağlayan Linux VM'lerini denetle
- \[Önizleme\]: Parola olmayan hesaplardan uzaktan bağlantı sağlayan Linux VM'lerini denetlemek için gereksinimleri dağıtma
- Depolama hesaplarına sınırsız ağ erişimini denetleme
- API Uygulaması için uzaktan hata ayıklama kapatılmalıdır
- İşlev Uygulaması için uzaktan hata ayıklama kapatılmalıdır
- Web Uygulaması için uzaktan hata ayıklama kapatılmalıdır

## <a name="au-3-2-content-of-audit-records--centralized-management-of-planned-audit-record-content"></a>AU-3 (2) Denetim Kayıtlarının İçeriği | Planlı Denetim Kayıt İçeriğinin Merkezi Yönetimi

Azure Monitor tarafından toplanan günlük verileri, merkezi yapılandırma ve yönetimi sağlayan bir Log Analytics çalışma alanında depolanır. Bu plan, Azure sanal makinelerinde Log Analytics aracısını denetleyen ve uygulayan [Azure İlkesi](../../../policy/overview.md) tanımları atayarak olayların günlüğe kaydedilmesini sağlamanıza yardımcı olur.

- \[Önizleme\]: Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- \[Önizleme\]: VMSS'de Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- \[Önizleme\]: VM için Denetim Günlüğü Analitik Çalışma Alanı - Rapor Uyuşmazlığı
- \[Önizleme\]: Linux VM Ölçek Setleri (VMSS) için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Windows VM Ölçek Setleri (VMSS) için Günlük Analizi Aracısı Dağıt
- \[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt

## <a name="au-5-response-to-audit-processing-failures"></a>AU-5 Denetim İşleme Hatalarına Yanıt

Bu plan, denetim ve olay günlüğe kaydetme yapılandırmalarını izleyen [Azure İlkesi](../../../policy/overview.md) tanımları atar. Bu yapılandırmaların izlenmesi, bir denetim sistemi hatasının veya yanlış yapılandırmanın bir göstergesi olabilir ve düzeltici eylemde bulunmanıza yardımcı olabilir.

- Tanılama ayarını denetle
- SQL Server'da gelişmiş veri güvenliği ayarlarında denetim etkinleştirilmelidir
- Yönetilen örneklerinizde gelişmiş veri güvenliği etkinleştirilmelidir
- SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir

## <a name="au-6-4-audit-review-analysis-and-reporting--central-review-and-analysis"></a>AU-6 (4) Denetim İnceleme, Analiz ve Raporlama | Merkezi İnceleme ve Analiz

Azure Monitor tarafından toplanan günlük verileri, merkezi raporlama ve analiz sağlayan bir Log Analytics çalışma alanında depolanır. Bu plan, Azure sanal makinelerinde Log Analytics aracısını denetleyen ve uygulayan [Azure İlkesi](../../../policy/overview.md) tanımları atayarak olayların günlüğe kaydedilmesini sağlamanıza yardımcı olur.

- \[Önizleme\]: Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- \[Önizleme\]: VMSS'de Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- \[Önizleme\]: VM için Denetim Günlüğü Analitik Çalışma Alanı - Rapor Uyuşmazlığı
- \[Önizleme\]: Linux VM Ölçek Setleri (VMSS) için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Windows VM Ölçek Setleri (VMSS) için Günlük Analizi Aracısı Dağıt
- \[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt

## <a name="au-6-5-audit-review-analysis-and-reporting--integration--scanning-and-monitoring-capabilities"></a>AU-6 (5) Denetim İnceleme, Analiz ve Raporlama | Entegrasyon / Tarama ve İzleme Yetenekleri

Bu plan, sanal makinelerde, sanal makine ölçek kümelerinde, SQL yönetilen örneklerinde ve SQL sunucularında güvenlik açığı değerlendirmesinin analiziyle kayıtları denetleyen ilke tanımları sağlar.
Bu ilke tanımları, Azure kaynakları içinde gerçekleştirilen işlemler hakkında bilgi sağlamak için tanılama günlüklerinin yapılandırmasını da denetler. Bu öngörüler, dağıtılmış kaynaklarınızın güvenlik durumu hakkında gerçek zamanlı bilgiler sağlar ve düzeltme eylemlerine öncelik vermenize yardımcı olabilir.
Ayrıntılı güvenlik açığı tarama ve izleme için Azure Sentinel ve Azure Güvenlik Merkezi'ni de kullanasınız öneririz.

- \[Önizleme\]: Sanal Makinelerde Güvenlik Açığı Değerlendirmesi etkinleştirilmelidir
- \[Önizleme\]: VM'ler için Azure Monitörünü Etkinleştir
- \[Önizleme\]: VM Ölçek Setleri (VMSS) için Azure Monitörünü etkinleştir
- SQL sunucularınızda güvenlik açığı değerlendirmesi etkinleştirilmelidir
- Tanılama ayarını denetle
- SQL yönetilen örneklerinizde güvenlik açığı değerlendirmesi etkinleştirilmelidir
- SQL sunucularınızda güvenlik açığı değerlendirmesi etkinleştirilmelidir
- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir
- Güvenlik açıkları, Güvenlik Açığı Değerlendirmesi çözümü yle düzeltilmelidir
- Sanal makine ölçeği kümelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir

## <a name="au-12-audit-generation"></a>AU-12 Denetim Üretimi

Bu plan, Log Analytics aracısının Azure sanal makinelerde dağıtımını ve diğer Azure kaynak türleri için denetim ayarlarının yapılandırmasını denetleyen ve zorlayan ilke tanımları sağlar.
Bu ilke tanımları, Azure kaynakları içinde gerçekleştirilen işlemler hakkında bilgi sağlamak için tanılama günlüklerinin yapılandırmasını da denetler. Ayrıca, denetim ve Gelişmiş Veri Güvenliği SQL sunucularında yapılandırılır.

- \[Önizleme\]: Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- \[Önizleme\]: VMSS'de Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- \[Önizleme\]: VM için Denetim Günlüğü Analitik Çalışma Alanı - Rapor Uyuşmazlığı
- \[Önizleme\]: Linux VM Ölçek Setleri (VMSS) için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Windows VM Ölçek Setleri (VMSS) için Günlük Analizi Aracısı Dağıt
- \[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt
- Tanılama ayarını denetle
- SQL Server'da gelişmiş veri güvenliği ayarlarında denetim etkinleştirilmelidir
- Yönetilen örneklerinizde gelişmiş veri güvenliği etkinleştirilmelidir
- SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir
- Gelişmiş Veri Güvenliğini SQL sunucularında dağıtma
- DENETIMI SQL sunucularında dağıtma
- Ağ Güvenlik Grupları için Tanılama Ayarlarını Dağıtma

## <a name="au-12-01-audit-generation--system-wide--time-correlated-audit-trail"></a>AU-12 (01) Denetim Üretimi | Sistem Genelinde / Zaman Bağlaçlı Denetim İzi

Bu plan, Azure kaynaklarında günlük ayarlarını denetleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak sistem olaylarının günlüğe kaydedilmesini sağlamanıza yardımcı olur.
Bu yerleşik ilke, tanılama ayarlarının etkin olup olmadığını denetlemek için bir dizi kaynak türü belirtmenizi gerektirir.

- Tanılama ayarını denetle

## <a name="cm-7-2-least-functionality--prevent-program-execution"></a>CM-7 (2) En Az İşlevsellik | Program Yürütülmesini Önleme

Azure Güvenlik Merkezi'ndeki uyarlanabilir uygulama denetimi, belirli yazılımların sanal makinelerinizde çalışmasını engelleyen veya engelleyen akıllı, uçtan uca uygulama beyaz liste çözümüdür. Uygulama denetimi, onaylanmamış uygulamanın çalışmasını yasaklayan bir uygulama modunda çalıştırılabilir. Bu plan, uygulama beyaz listesinin önerildiği ancak henüz yapılmadığını sanal makineleri izlemenize yardımcı olan bir Azure İlkesi tanımı atar.

- Uyarlanabilir Uygulama Denetimleri sanal makinelerde etkinleştirilmelidir

## <a name="cm-7-5-least-functionality--authorized-software--whitelisting"></a>CM-7 (5) En Az İşlevsellik | Yetkili Yazılım / Whitelisting

Azure Güvenlik Merkezi'ndeki uyarlanabilir uygulama denetimi, belirli yazılımların sanal makinelerinizde çalışmasını engelleyen veya engelleyen akıllı, uçtan uca uygulama beyaz liste çözümüdür. Uygulama denetimi, sanal makineleriniz için onaylanmış uygulama listeleri oluşturmanıza yardımcı olur. Bu plan, uygulama beyaz listesinin önerildiği ancak henüz yapılmadığını sanal makineleri izlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Uyarlanabilir Uygulama Denetimleri sanal makinelerde etkinleştirilmelidir

## <a name="cm-11-user-installed-software"></a>CM-11 Kullanıcı Yüklü Yazılım

Azure Güvenlik Merkezi'ndeki uyarlanabilir uygulama denetimi, belirli yazılımların sanal makinelerinizde çalışmasını engelleyen veya engelleyen akıllı, uçtan uca uygulama beyaz liste çözümüdür. Uygulama denetimi, yazılım kısıtlama ilkeleriyle uyumluluğu zorlamanıza ve izlemenize yardımcı olabilir. Bu plan, uygulama beyaz listesinin önerildiği ancak henüz yapılmadığını sanal makineleri izlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Uyarlanabilir Uygulama Denetimleri sanal makinelerde etkinleştirilmelidir

## <a name="cp-7-alternate-processing-site"></a>CP-7 Alternatif İşleme Sitesi

Azure Site Kurtarma, sanal makinelerde çalışan iş yüklerini birincil konumdan ikincil bir konuma çoğaltır. Birincil sitede bir kesinti oluşursa, iş yükü ikincil konum üzerinde başarısız olur. Bu plan, olağanüstü durum kurtarma yapılandırılması olmadan sanal makineleri denetleyen bir [Azure İlkesi](../../../policy/overview.md) tanımı atar. Bu göstergenin izlenmesi, gerekli acil durum denetimlerinin yerinde olduğundan emin olabilirsiniz.

- Olağanüstü durum kurtarma yapılandırması yapılmadan sanal makineleri denetleme

## <a name="cp-9-05--information-system-backup--transfer-to-alternate-storage-site"></a>CP-9 (05) Bilgi Sistemi Yedekleme | Alternatif Depolama Sitesine Aktarma

Bu plan, kuruluşun sistem yedekleme bilgilerini elektronik olarak alternatif depolama sitesine denetleyen Azure İlkesi tanımlarını atar. Depolama meta verilerinin fiziksel olarak sevkiyatı için Azure Veri Kutusu'ni kullanmayı düşünün.

- Depolama Hesapları için coğrafi yedekli depolama etkinleştirilmelidir
- PostgreSQL için Azure Veritabanı için coğrafi yedekli yedekleme etkinleştirilmelidir
- MySQL için Azure Veritabanı için coğrafi yedekli yedekleme etkinleştirilmelidir
- MariaDB için Azure Veritabanı için coğrafi yedekli yedekleme etkinleştirilmelidir
- Azure SQL Veritabanları için uzun vadeli coğrafi yedekli yedekleme etkinleştirilmelidir

## <a name="ia-2-1-identification-and-authentication-organizational-users--network-access-to-privileged-accounts"></a>IA-2 (1) Kimlik ve Kimlik Doğrulama (Organizasyonel Kullanıcılar) | Ayrıcalıklı Hesaplara Ağ Erişimi

Bu plan, çok faktörlü kimlik doğrulaması etkin olmayan sahip ve/veya yazma izinlerine [Azure İlkesi](../../../policy/overview.md) tanımları atayarak ayrıcalıklı erişimi kısıtlamanıza ve denetlemenize yardımcı olur. Çok faktörlü kimlik doğrulama, bir parça kimlik doğrulama bilgisi tehlikeye atsa bile hesapların güvende tutulmasına yardımcı olur. Çok faktörlü kimlik doğrulaması etkin leştirilmeden hesapları izleyerek, gizliliği ihlal edilme olasılığı daha yüksek hesapları tanımlayabilirsiniz.

- MFA, aboneliğinizde sahip izni olan hesaplarda etkinleştirilmelidir
- MFA, aboneliğinizde yazma izinleri olan hesaplarda etkinleştirilmelidir

## <a name="ia-2-2-identification-and-authentication-organizational-users--network-access-to-non-privileged-accounts"></a>IA-2 (2) Kimlik ve Kimlik Doğrulama (Kurumsal Kullanıcılar) | İmtiyazsız Hesaplara Ağ Erişimi

Bu plan, çok faktörlü kimlik doğrulaması etkin olmayan okuma izinleriyle denetim hesaplarına bir [Azure İlkesi](../../../policy/overview.md) tanımı atayarak erişimi kısıtlamanıza ve denetlemenize yardımcı olur. Çok faktörlü kimlik doğrulama, bir parça kimlik doğrulama bilgisi tehlikeye atsa bile hesapların güvende tutulmasına yardımcı olur. Çok faktörlü kimlik doğrulaması etkin leştirilmeden hesapları izleyerek, gizliliği ihlal edilme olasılığı daha yüksek hesapları tanımlayabilirsiniz.

- MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir

## <a name="ia-5-authenticator-management"></a>IA-5 Kimlik Doğrulama Yönetimi

Bu plan, parolasız hesaplardan uzaktan bağlantılara izin veren ve/veya passwd dosyasında yanlış izinler ayarlanan Linux sanal makinelerini denetleyen [Azure İlkesi](../../../policy/overview.md) tanımları atar. Bu plan ayrıca, Windows sanal makineleri için parola şifreleme türünün yapılandırmasını denetleyen ilke tanımları da atar. Bu göstergeleri izlemek, sistem kimlik doğrulayıcılarının kuruluşunuzun kimlik doğrulama ilkesine uymasını sağlamanıza yardımcı olur.

- \[Önizleme\]: 0644 olarak ayarlanmış passwd dosya izinleri olmayan Linux VM'leri denetle
- \[Önizleme\]: Parolasız hesapları olan Linux VM'lerini denetle
- \[Önizleme\]: Parolaları ters çevrilebilir şifreleme kullanarak depolamayan Windows VM'lerini denetleme
- \[Önizleme\]: 0644 olarak ayarlanmış passwd dosya izinleri olmayan Linux VM'leri denetlemek için gereksinimleri dağıtma
- \[Önizleme\]: Parolaları olmayan hesapları olan Linux VM'lerini denetlemek için gereksinimleri dağıtma
- \[Önizleme\]: Parolaları ters çevrilebilir şifreleme kullanarak depolamayan Windows VM'lerini denetlemek için gereksinimleri dağıtma

## <a name="ia-5-1-authenticator-management--password-based-authentication"></a>IA-5 (1) Kimlik Doğrulama Yönetimi | Parola Tabanlı Kimlik Doğrulama

Bu plan, minimum mukavemet ve diğer parola gereksinimlerini zorlamayan Windows sanal makinelerini denetleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak güçlü parolalar uygulamanıza yardımcı olur. Parola gücü ilkesini ihlal eden sanal makinelerin farkında olmak, tüm sanal makine kullanıcı hesaplarının parolalarının kuruluşunuzun parola ilkesine uygun olmasını sağlamak için düzeltici eylemlerde yardımcı olur.

- \[Önizleme\]: Önceki 24 parolanın yeniden kullanımına izin veren Windows VM'lerini denetleme
- \[Önizleme\]: En fazla 70 günlük parola yaşı olmayan Windows VM'lerini denetleyin
- \[Önizleme\]: Minimum parola yaşı 1 gün olmayan Windows VM'lerini denetleyin
- \[Önizleme\]: Parola karmaşıklığı ayarı etkin olmayan Windows VM'lerini denetleme
- \[Önizleme\]: Minimum parola uzunluğunu 14 karakterle sınırlamayan Windows VM'lerini denetleme
- \[Önizleme\]: Parolaları ters çevrilebilir şifreleme kullanarak depolamayan Windows VM'lerini denetleme
- \[Önizleme\]: Önceki 24 parolanın yeniden kullanılmasına izin veren Windows VM'lerini denetlemek için gereksinimleri dağıtma
- \[Önizleme\]: En fazla 70 günlük parola yaşı olmayan Windows VM'lerini denetlemek için gereksinimleri dağıtma
- \[Önizleme\]: En az parola yaşı 1 gün olmayan Windows VM'lerini denetlemek için gereksinimleri dağıtma
- \[Önizleme\]: Parola karmaşıklığı ayarı etkin olmayan Windows VM'lerini denetlemek için gereksinimleri dağıtma
- \[Önizleme\]: Minimum parola uzunluğunu 14 karakterle sınırlamayan Windows VM'lerini denetlemek için gereksinimleri dağıtma
- \[Önizleme\]: Parolaları ters çevrilebilir şifreleme kullanarak depolamayan Windows VM'lerini denetlemek için gereksinimleri dağıtma

## <a name="ra-5-vulnerability-scanning"></a>RA-5 Güvenlik Açığı Tarama

Bu plan, Azure Güvenlik Merkezi'ndeki işletim sistemi güvenlik açıklarını, SQL güvenlik açıklarını ve sanal makine güvenlik açıklarını izleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak bilgi sistemi güvenlik açıklarını yönetmenize yardımcı olur. Azure Güvenlik Merkezi, dağıtılan Azure kaynaklarının güvenlik durumu hakkında gerçek zamanlı bilgi sahibi olmasını sağlayan raporlama özellikleri sağlar. Bu plan ayrıca SQL sunucularında Gelişmiş Veri Güvenliğini denetleyen ve uygulayan ilke tanımları da atar. Gelişmiş veri güvenliği, dağıtılan kaynaklarınızdaki güvenlik açıklarını anlamanıza yardımcı olacak güvenlik açığı değerlendirmesi ve gelişmiş tehdit koruması özelliklerini içeriyordu.

- Yönetilen örneklerinizde gelişmiş veri güvenliği etkinleştirilmelidir
- SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir
- Gelişmiş Veri Güvenliğini SQL sunucularında dağıtma
- Sanal makine ölçeği kümelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Sanal makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir
- Güvenlik açıkları, Güvenlik Açığı Değerlendirmesi çözümü yle düzeltilmelidir

## <a name="sc-5-denial-of-service-protection"></a>SC-5 Hizmet Korumasının Reddi

Azure'un dağıtılmış hizmet reddi (DDoS) standart katmanı, temel hizmet katmanıüzerinde ek özellikler ve azaltma özellikleri sağlar. Bu ek özellikler arasında Azure Monitor tümleştirmesi ve saldırı sonrası azaltma raporlarını gözden geçirme olanağı yer almaktadır. Bu plan, DDoS standart katmanı etkinse denetleyen bir [Azure İlkesi](../../../policy/overview.md) tanımı atar. Hizmet katmanları arasındaki yetenek farkını anlamak, Azure ortamınız için hizmet reddi korumalarını gidermek için en iyi çözümü seçmenize yardımcı olabilir.

- DDoS Koruma Standardı etkinleştirilmelidir

## <a name="sc-7-boundary-protection"></a>SC-7 Sınır Koruması

Bu plan, Azure Güvenlik Merkezi'nde ağ güvenliği grubu sertleştirme önerilerini izleyen bir [Azure İlkesi](../../../policy/overview.md) tanımı atayarak sistem sınırını yönetmenize ve denetlemenize yardımcı olur. Azure Güvenlik Merkezi, Internet'in karşı karşıya olduğu sanal makinelerin trafik modellerini analiz eder ve olası saldırı yüzeyini azaltmak için ağ güvenliği grubu kuralı önerileri sağlar.
Ayrıca, bu plan, korumasız uç noktaları, uygulamaları ve depolama hesaplarını izleyen ilke tanımları da atar. Güvenlik duvarı tarafından korunmayan uç noktalar ve uygulamalar ve sınırsız erişime sahip depolama hesapları, bilgi sisteminde bulunan bilgilere istenmeyen erişime izin verebilir.

- Internet'in sanal makinelerle karşı karşıya olması için Ağ Güvenliği Grubu Kuralları sertleştirilmiş olmalıdır
- Internet'ten erişim bitiş noktasına dönük olarak kısıtlanmalıdır
- Web bağlantı noktaları, VM'nizle ilişkili Ağ Güvenlik Gruplarında kısıtlanmalıdır
- Depolama hesaplarına sınırsız ağ erişimini denetleme

## <a name="sc-7-3-boundary-protection--access-points"></a>SC-7 (3) Sınır Koruması | Erişim Noktaları

Tam zamanında (JIT) sanal makine erişimi, Gelen Trafiği Azure sanal makinelerine kilitleyerek saldırılara maruz kalmamayı azaltırken gerektiğinde VM'lere bağlanmak için kolay erişim sağlar. JIT sanal makine erişimi, Azure'daki kaynaklarınızla harici bağlantı sayısını sınırlamanıza yardımcı olur. Bu plan, tam zamanında erişimi destekleyebilen ancak henüz yapılandırılmamış sanal makineleri izlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Sanal makinelere anlık ağ erişim denetimi uygulanmalıdır

## <a name="sc-7-4-boundary-protection--external-telecommunications-services"></a>SC-7 (4) Sınır Koruması | Dış Telekomünikasyon Hizmetleri

Tam zamanında (JIT) sanal makine erişimi, Gelen Trafiği Azure sanal makinelerine kilitleyerek saldırılara maruz kalmamayı azaltırken gerektiğinde VM'lere bağlanmak için kolay erişim sağlar. JIT sanal makine erişimi, erişim isteği ve onay işlemlerini kolaylaştırarak trafik akışı politikanızın istisnalarını yönetmenize yardımcı olur. Bu plan, tam zamanında erişimi destekleyebilen ancak henüz yapılandırılmamış sanal makineleri izlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Sanal makinelere anlık ağ erişim denetimi uygulanmalıdır

## <a name="sc-8-1-transmission-confidentiality-and-integrity--cryptographic-or-alternate-physical-protection"></a>SC-8 (1) İletim Gizliliği ve Bütünlüğü | Şifreleme veya Alternatif Fiziksel Koruma

Bu plan, iletişim protokolleri için uygulanan şifreleme mekanizmasını izlemenize yardımcı olan [Azure İlkesi](../../../policy/overview.md) tanımları atayarak aktarılan bilgilerin gizli ve bütünlüğünü korumanıza yardımcı olur. İletişimin düzgün şekilde şifrelenmiş olmasını sağlamak, kuruluşunuzun gereksinimlerini karşılamanıza veya bilgileri yetkisiz ifşa ve değişikliklere karşı korumanıza yardımcı olabilir.

- API Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır
- Güvenli iletişim protokolleri kullanmayan Windows web sunucularını denetleme
- Güvenli iletişim protokolleri kullanmayan Windows web sunucularını denetlemek için gereksinimleri dağıtma
- Fonksiyon Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır
- Yalnızca Redis Önbelleğinize güvenli bağlantılar etkinleştirilmelidir
- Depolama hesaplarına güvenli aktarım etkinleştirilmelidir
- Web Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır

## <a name="sc-28-1-protection-of-information-at-rest--cryptographic-protection"></a>SC-28 (1) Istirahatte Bilginin Korunması | Şifreleme Koruması

Bu plan, belirli şifreleme denetimlerini ve zayıf şifreleme ayarlarının denetim kullanımını zorunlu kılarak [Azure İlkesi](../../../policy/overview.md) tanımları atayarak bilgileri korumak için şifreleme denetimlerinin kullanımına ilişkin ilkenizi uygulamanıza yardımcı olur. Azure kaynaklarınızın en iyi olmayan şifreleme yapılandırmalarına sahip olabileceğini anlamak, kaynakların bilgi güvenliği politikanıza uygun şekilde yapılandırıldığından emin olmak için düzeltici eylemlerde yardımcı olabilir. Özellikle, bu plan tarafından atanan ilke tanımları veri gölü depolama hesapları için şifreleme gerektirir; SQL veritabanlarında saydam veri şifreleme gerektirir; ve SQL veritabanlarında, sanal makine disklerinde ve otomasyon hesabı değişkenlerinde eksik şifrelemeyi denetleyin.

- Yönetilen örneklerinizde gelişmiş veri güvenliği etkinleştirilmelidir
- SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir
- Gelişmiş Veri Güvenliğini SQL sunucularında dağıtma
- SQL DB saydam veri şifrelemesini dağıtma
- Disk şifreleme sanal makinelerde uygulanmalıdır
- Data Lake Store hesaplarında şifreleme gerektirme
- SQL veritabanlarında Saydam Veri Şifrelemesi etkinleştirilmelidir

## <a name="si-2-flaw-remediation"></a>SI-2 Kusur Düzeltme

Bu plan, Eksik sistem güncelleştirmelerini, işletim sistemi açıklarını, SQL güvenlik açıklarını ve Azure Güvenlik Merkezi'ndeki sanal makine güvenlik açıklarını izleyen [Azure İlkesi](../../../policy/overview.md) tanımlarını atayarak bilgi sistemi hatalarını yönetmenize yardımcı olur. Azure Güvenlik Merkezi, dağıtılan Azure kaynaklarının güvenlik durumu hakkında gerçek zamanlı bilgi sahibi olmasını sağlayan raporlama özellikleri sağlar. Bu plan ayrıca, sanal makine ölçek kümeleri için işletim sisteminin yama sağlayan bir ilke tanımı atar.

- Sanal Makine Ölçeği Kümelerinde otomatik işletim sistemi görüntü yaması gerektirme
- Sanal makine ölçek kümelerinde sistem güncellemeleri yüklenmelidir
- Sistem güncellemeleri sanal makinelerinize yüklenmeli
- Sanal makine ölçeği kümelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Sanal makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir
- Güvenlik açıkları, Güvenlik Açığı Değerlendirmesi çözümü yle düzeltilmelidir

## <a name="si-3-malicious-code-protection"></a>SI-3 Kötü Amaçlı Kod Koruması

Bu plan, Azure Güvenlik Merkezi'ndeki sanal makinelerde eksik uç nokta korumasını izleyen ve Windows sanal makinelerde Microsoft kötü amaçlı yazılımdan koruma çözümünü uygulayan [Azure İlkesi](../../../policy/overview.md) tanımları atayarak kötü amaçlı kod koruması da dahil olmak üzere uç nokta korumayı yönetmenize yardımcı olur.

- Windows Server için varsayılan Microsoft IaaaSAntimalware uzantısı dağıtma
- Uç nokta koruma çözümü sanal makine ölçek kümeleri üzerine kurulmalıdır
- Azure Güvenlik Merkezi'nde eksik Uç Nokta Koruması'nı izleme

## <a name="si-3-1-malicious-code-protection--central-management"></a>SI-3 (1) Kötü Amaçlı Kod Koruması | Merkezi Yönetim

Bu plan, Azure Güvenlik Merkezi'ndeki sanal makinelerde eksik uç nokta korumayı izleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak kötü amaçlı kod koruması da dahil olmak üzere uç nokta korumayı yönetmenize yardımcı olur. Azure Güvenlik Merkezi, dağıtılan Azure kaynaklarının güvenlik durumu hakkında gerçek zamanlı bilgi sahibi olmak için merkezi yönetim ve raporlama özellikleri sağlar.

- Uç nokta koruma çözümü sanal makine ölçek kümeleri üzerine kurulmalıdır
- Azure Güvenlik Merkezi'nde eksik Uç Nokta Koruması'nı izleme

## <a name="si-4-information-system-monitoring"></a>SI-4 Bilgi Sistemi İzleme

Bu plan, Azure kaynaklarında günlüğe kaydetme ve veri güvenliğini denetleyerek ve uygulayarak sisteminizi izlemenize yardımcı olur. Özellikle, ilkeler denetim atanmış ve Log Analytics aracısı dağıtım zorlamak ve SQL veritabanları, depolama hesapları ve ağ kaynakları için gelişmiş güvenlik ayarları. Bu özellikler, uygun eylemi gerçekleştirebilmeniz için anormal davranışları ve saldırı göstergelerini algılamanıza yardımcı olabilir.

- \[Önizleme\]: Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- \[Önizleme\]: VMSS'de Denetim Günlüğü Analytics Agent Deployment - VM Image (OS) listelenmemiş
- \[Önizleme\]: VM için Denetim Günlüğü Analitik Çalışma Alanı - Rapor Uyuşmazlığı
- \[Önizleme\]: Linux VM Ölçek Setleri (VMSS) için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın
- \[Önizleme\]: Windows VM Ölçek Setleri (VMSS) için Günlük Analizi Aracısı Dağıt
- \[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt
- Yönetilen örneklerinizde gelişmiş veri güvenliği etkinleştirilmelidir
- SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir
- Gelişmiş Veri Güvenliğini SQL sunucularında dağıtma
- Depolama Hesaplarına Gelişmiş Tehdit Koruması Dağıtma
- DENETIMI SQL sunucularında dağıtma
- Sanal ağlar oluşturulduğunda ağ izleyicisini dağıtma
- SQL sunucularında Tehdit Algılamayı Dağıtma
- İzin verilen konumlar
- Kaynak grupları için izin verilen konumlar

## <a name="si-4-18-information-system-monitoring--analyze-traffic--covert-exfiltration"></a>SI-4 (18) Bilgi Sistemi İzleme | Trafiği Analiz Et / Gizli Exfiltration

Azure Depolama için Gelişmiş Tehdit Koruması, depolama hesaplarına erişmek veya bu hesaplardan yararlanmak için olağandışı ve zararlı olabilecek girişimleri algılar. Koruma uyarıları anormal erişim desenleri, anormal ekstreler/yüklemeler ve şüpheli depolama etkinlikleriiçerir. Bu göstergeler, gizli bilgi sızma nızı tespit etmenize yardımcı olabilir.

- Depolama Hesaplarına Gelişmiş Tehdit Koruması Dağıtma

> [!NOTE]
> Azure İlkesi tanımlarının kullanılabilirliği Azure Kamu ve diğer ulusal bulutlarda farklılık gösterebilir. 

## <a name="next-steps"></a>Sonraki adımlar

FedRAMP Yüksek planının kontrol haritasını gözden geçirdiğiniz için, plan ve bu örneğin nasıl dağıtılabildiğini öğrenmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [FedRAMP Yüksek planı - Genel Bakış](./index.md)
> [FedRAMP Yüksek planı - Dağıtım adımları](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Plan yaşam döngüsü](../../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../../how-to/update-existing-assignments.md)öğrenin.