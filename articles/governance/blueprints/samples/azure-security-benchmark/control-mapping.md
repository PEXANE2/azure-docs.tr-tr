---
title: Azure Güvenlik Kıyaslama planı örnek denetimleri
description: Azure Güvenlik Kıyaslaması planı örneğinin Azure Politikası için eşlemesini denetleme.
ms.date: 04/16/2020
ms.topic: sample
ms.openlocfilehash: 0f2d24d7d8c9f7ce2568f11b15e65ed1fcd02afb
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81538736"
---
# <a name="control-mapping-of-the-azure-security-benchmark-blueprint-sample"></a>Azure Güvenlik Kıyaslama planı örneğinin denetim eşlemi

Aşağıdaki makalede, Azure Planları Azure Güvenlik Kıyaslaması planı örnek lemlerinin Azure Güvenlik Kıyaslaması denetimleri için nasıl eşlenebildiğini ayrıntılı olarak açıklanmaktadır. Denetimler hakkında daha fazla bilgi için [Azure Güvenlik Kıyaslama'ya](https://docs.microsoft.com/azure/security/benchmarks/overview)bakın.

Aşağıdaki eşlemeler **Azure Güvenlik Kıyaslaması** denetimleri içindir. Doğrudan belirli bir denetim eşleleme sine atlamak için sağdaki gezinmeyi kullanın. Eşlenen denetimler bir Azure [İlkesi](../../../policy/overview.md) girişimiyle uygulanır. Girişimin tamamını incelemek için Azure portalında **İlke'yi** açın ve **Tanımlar** sayfasını seçin. Ardından, ** \[Önizleme:\]Azure Güvenlik Kıyaslama önerilerini denetleyin ve belirli destekleyici VM Uzantıları** yerleşik ilke girişimini dağıtın ve bulun.

> [!IMPORTANT]
> Aşağıdaki her denetim bir veya daha fazla [Azure İlkesi](../../../policy/overview.md) tanımıyla ilişkilidir. Bu ilkeler, denetime [uygunluğu değerlendirmenize](../../../policy/how-to/get-compliance-data.md) yardımcı olabilir; ancak, genellikle bir denetim ve bir veya daha fazla ilke arasında 1:1 veya tam bir eşleşme yoktur. Bu nedenle, Azure İlkesinde **Uyumlu olmak** yalnızca ilkelerin kendileri için başvurur; bu, bir denetimin tüm gereksinimlerine tam olarak uymanızı sağlamaz. Ayrıca, uyumluluk standardı şu anda herhangi bir Azure İlkesi tanımı tarafından ele alınmayan denetimleri içerebilir. Bu nedenle, Azure İlkesi'ndeki uyumluluk, genel uyumluluk durumunuza yalnızca kısmi bir görünümdür. Bu uyumluluk planı örneği için denetimler ve Azure İlkesi tanımları arasındaki ilişkilendirmeler zaman içinde değişebilir. Değişiklik geçmişini görüntülemek için [GitHub Commit History'ye](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/azure-security-benchmark/control-mapping.md)bakın.

## <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1 Sanal Ağınızdaki Ağ Güvenlik Grupları veya Azure Güvenlik Duvarı'nı kullanarak kaynakları koruyun

- Alt ağlar bir Ağ Güvenlik Grubu ile ilişkilendirilmelidir
- Adaptif Ağ Sertleştirme önerileri sanal makinelere bakan internet üzerinde uygulanmalıdır
- Sanal makineler onaylanmış bir sanal ağa bağlanmalıdır
- Internet'e bakan sanal makineler Ağ Güvenlik Grupları ile korunmalıdır
- Hizmet Veri Servisi sanal ağ hizmeti bitiş noktası kullanmalıdır
- Uygulama Hizmeti sanal ağ hizmeti bitiş noktası kullanmalıdır
- SQL Server sanal ağ hizmeti bitiş noktası kullanmalıdır
- Olay Hub sanal ağ hizmeti bitiş noktası kullanmalıdır
- Cosmos DB sanal ağ hizmeti bitiş noktası kullanmalıdır
- Key Vault sanal ağ hizmeti bitiş noktası kullanmalıdır
- Depolama hesaplarına sınırsız ağ erişimini denetleme
- Depolama Hesapları sanal ağ hizmeti bitiş noktası kullanmalıdır
- Kapsayıcı Kayıt Defteri sanal ağ hizmeti bitiş noktası kullanmalıdır
- Sanal ağlar belirtilen sanal ağ ağ ağ geçidi kullanmalıdır
- Yetkili IP aralıkları Kubernetes Hizmetlerinde tanımlanmalıdır
- \[Önizleme\]: Sanal makinenizdeki IP Yönlendirme devre dışı bırakılmalı
- Internet'e bakan sanal makineler Ağ Güvenlik Grupları ile korunmalıdır
- Sanal makinelere anlık ağ erişim denetimi uygulanmalıdır
- Yönetim bağlantı noktaları sanal makinelerinizde kapatılmalıdır

## <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2 Vnet, Subnet ve NIC'lerin yapılandırmasını ve trafiğini izleyin ve kaydedin

- Ağ İzleyicisi etkinleştirilmelidir

## <a name="13-protect-critical-web-applications"></a>1.3 Kritik web uygulamalarını koruyun

- WEB uygulamasının 'Müşteri Sertifikaları (Gelen istemci sertifikaları)' 'A)' olarak ayarlandığından emin olun
- CORS her kaynağın Web Uygulamalarınıza erişmesine izin vermemelidir
- CORS, her kaynağın İşlev Uygulamalarınıza erişmesine izin vermemelidir
- CORS, her kaynağın API Uygulamanıza erişmesine izin vermemelidir
- Web Uygulamaları için uzaktan hata ayıklama kapatılmalıdır
- İşlev Uygulamaları için uzaktan hata ayıklama kapatılmalıdır
- API Uygulamaları için uzaktan hata ayıklama kapatılmalıdır

## <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4 Bilinen kötü amaçlı IP adresleriyle iletişimi reddetme

- DDoS Koruma Standardı etkinleştirilmelidir
- Sanal makinelere anlık ağ erişim denetimi uygulanmalıdır
- Adaptif Ağ Sertleştirme önerileri sanal makinelere bakan internet üzerinde uygulanmalıdır

## <a name="15-record-network-packets-and-flow-logs"></a>1.5 Ağ paketlerini ve akış günlüklerini kaydetme

- Ağ İzleyicisi etkinleştirilmelidir

## <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11 Ağ kaynağı yapılandırmalarını izlemek ve değişiklikleri algılamak için otomatik araçlar kullanın

- 'Güvenlik Seçenekleri - Ağ Erişimi' (Güvenlik Seçenekleri - Ağ Erişimi) yapılandırmalarını denetlemek için ön koşulları dağıtma
- Windows VM yapılandırmalarından gelen denetim sonuçlarını 'Güvenlik Seçenekleri - Microsoft Ağ İstemci' adlı nda göster
- 'Güvenlik Seçenekleri - Ağ Güvenliği' (Güvenlik Seçenekleri - Ağ Güvenliği) yapılandırmalarını denetlemek için ön koşulları dağıtma
- Windows VM yapılandırmalarından gelen denetim sonuçlarını 'Güvenlik Seçenekleri - Ağ Güvenliği' adlı dizide göster
- 'Güvenlik Seçenekleri - Microsoft Network Server' 'da Windows VM yapılandırmalarını denetlemek için ön koşulları dağıtma
- Windows VM yapılandırmalarından gelen denetim sonuçlarını 'Güvenlik Seçenekleri - Microsoft Ağ Sunucusu' nda göster
- 'Yönetim şablonları - Ağ'daki Windows VM yapılandırmalarını denetlemek için ön koşulları dağıtma
- Windows VM yapılandırmalarından gelen denetim sonuçlarını 'Yönetim şablonları - Ağ'da göster

## <a name="22-configure-central-security-log-management"></a>2.2 Merkezi güvenlik günlüğü yönetimini yapılandırma

- Log Analytics aracısı sanal makinelere yüklenmeli
- Log Analytics aracısı Sanal Makine Ölçeği Kümelerine yüklenmelidir
- Log Analytics aracısının beklendiği gibi bağlı olmadığı Windows VM'lerini denetlemek için ön koşulları dağıtma
- Log Analytics aracısının beklendiği gibi bağlı olmadığı Windows VM'lerinden gelen denetim sonuçlarını göster
- Azure Monitor günlük profili 'yazma', 'silme' ve 'eylem' kategorileri için günlükleri toplamalı
- Azure Monitor tüm bölgelerden etkinlik günlükleri toplamalıdır
- Log Analytics izleme aracısının otomatik olarak sağlanması aboneliğinizde etkinleştirilmelidir

## <a name="23-enable-audit-logging-for-azure-resources"></a>2.3 Azure kaynakları için denetim günlüğe kaydetmeyi etkinleştirme

- Azure Veri Gölü Deposu'ndaki tanılama günlükleri etkinleştirilmelidir
- Logic Apps'taki tanılama günlükleri etkinleştirilmeli
- IoT Hub'daki tanılama günlükleri etkinleştirilmelidir
- Toplu iş hesaplarındaki tanılama günlükleri etkinleştirilmeli
- Sanal Makine Ölçek Kümelerinde tanılama günlükleri etkinleştirilmelidir
- Event Hub'daki tanılama günlükleri etkinleştirilmeli
- Arama hizmetlerinde tanılama günlükleri etkinleştirilmelidir
- Uygulama Hizmetlerinde tanılama günlükleri etkinleştirilmelidir
- Data Lake Analytics'teki tanılama günlükleri etkinleştirilmelidir
- Key Vault'taki tanılama günlükleri etkinleştirilmeli
- Hizmet Veri Servisi'ndeki tanılama günlükleri etkinleştirilmeli
- Azure Akış Analizi'ndeki tanılama günlükleri etkinleştirilmelidir
- SQL sunucusunda denetim etkinleştirilmelidir
- Tanılama ayarını denetle

## <a name="24-collect-security-logs-from-operating-systems"></a>2.4 İşletim sistemlerinden güvenlik günlüklerini toplama

- Log Analytics izleme aracısının otomatik olarak sağlanması aboneliğinizde etkinleştirilmelidir
- Log Analytics aracısı sanal makinelere yüklenmeli
- Log Analytics aracısı Sanal Makine Ölçeği Kümelerine yüklenmelidir
- Log Analytics aracısının beklendiği gibi bağlı olmadığı Windows VM'lerini denetlemek için ön koşulları dağıtma
- Log Analytics aracısının beklendiği gibi bağlı olmadığı Windows VM'lerinden gelen denetim sonuçlarını göster

## <a name="27-enable-alerts-for-anomalous-activity"></a>2.7 Anormal aktivite için uyarıları etkinleştirme

- Güvenlik Merkezi standart fiyatlandırma katmanı seçilmelidir
- SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir
- SQL yönetilen örneklerinizde gelişmiş veri güvenliği etkinleştirilmelidir
- Gelişmiş Tehdit Koruması türleri SQL sunucusu Gelişmiş Veri Güvenliği ayarlarında 'Tümü' olarak ayarlanmalıdır
- Gelişmiş Tehdit Koruması türleri SQL yönetilen örnek Gelişmiş Veri Güvenliği ayarlarında 'Tümü' olarak ayarlanmalıdır

## <a name="28-centralize-anti-malware-logging"></a>2.8 Kötü amaçlı yazılımdan koruma günlüğe kaydetmeyi merkezileştirin

- Azure için Microsoft Antimalware, koruma imzalarını otomatik olarak güncelleştirmek üzere yapılandırılmalıdır
- Azure Güvenlik Merkezi'nde eksik Uç Nokta Koruması'nı izleme
- Uç nokta koruma çözümü sanal makine ölçek kümeleri üzerine kurulmalıdır

## <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1 İdari hesapların envanterini tutma

- Aboneliğiniz için en fazla 3 sahip belirlenmelidir
- Aboneliğinize birden fazla sahip atanmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan amortismana alınmış hesaplar aboneliğinizden kaldırılmalıdır

## <a name="33-use-dedicated-administrative-accounts"></a>3.3 Özel yönetim hesaplarını kullanma

- Yöneticiler grubunun yalnızca belirtilen üyeleri içermediği Windows VM'lerini denetlemek için ön koşulları dağıtma
- Yöneticiler grubunun yalnızca belirtilen üyeleri içermediği Windows VM'lerinden gelen denetim sonuçlarını göster
- Yöneticiler grubunun belirtilen üyelerin tümlerini içermediği Windows VM'lerini denetlemek için ön koşulları dağıtma
- Yöneticiler grubunun belirtilen üyelerin tümünü içermediği Windows VM'lerinden gelen denetim sonuçlarını göster
- Yöneticiler grubunun belirtilen üyelerden herhangi birini içerdiği Windows VM'lerini denetlemek için ön koşulları dağıtma
- Yöneticiler grubunun belirtilen üyelerden herhangi birini içerdiği Windows VM'lerinden gelen denetim sonuçlarını göster
- Aboneliğiniz için en fazla 3 sahip belirlenmelidir
- Aboneliğinize birden fazla sahip atanmalıdır

## <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5 Tüm Azure Active Directory tabanlı erişim için çok faktörlü kimlik doğrulamayı kullanın

- MFA, aboneliğinizde sahip izni olan hesaplarda etkinleştirilmelidir
- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir
- MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir

## <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7 İdari hesaplardan şüpheli etkinliklere ilişkin günlük ve uyarı

- Güvenlik Merkezi standart fiyatlandırma katmanı seçilmelidir

## <a name="39-use-azure-active-directory"></a>3.9 Azure Etkin Dizini Kullanma

- SQL sunucuları için bir Azure Active Directory yöneticisi sağlanmalıdır
- Hizmet Kumaş kümeleri yalnızca müşteri kimlik doğrulaması için Azure Etkin Dizini kullanmalıdır
- API uygulamasında Azure Active Directory ile Kaydolun etkinleştirildiğinden emin olun
- AZURE Active Directory ile Kaydol'un WEB Uygulamasında etkinleştirildiğinden emin olun
- İşlev Uygulamasında Azure Active Directory ile Kaydolun etkinleştirildiğinden emin olun

## <a name="310-regularly-review-and-reconcile-user-access"></a>3.10 Kullanıcı erişimini düzenli olarak gözden geçirin ve uzlaştırın

- Amortismana alınan hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan amortismana alınmış hesaplar aboneliğinizden kaldırılmalıdır
- Okuma izinleri olan harici hesaplar aboneliğinizden kaldırılmalıdır
- Yazma izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

## <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1 Hassas Bilgilerin envanterini koruyun

- SQL veritabanlarınızdaki hassas veriler sınıflandırılmalıdır

## <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4 Transit tüm hassas bilgileri şifreleme

- Depolama hesaplarına güvenli aktarım etkinleştirilmelidir
- API Uygulamanızda en son TLS sürümü kullanılmalıdır
- Web Uygulamanızda en son TLS sürümü kullanılmalıdır
- Fonksiyon Uygulamanızda en son TLS sürümü kullanılmalıdır
- Fonksiyon Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır
- Web Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır
- API Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır
- MySQL veritabanı sunucuları için SSL bağlantısını zorla
- PostgreSQL veritabanı sunucuları için SSL bağlantısını uygulayın etkinleştirilmelidir
- Yalnızca Redis Önbelleğinize güvenli bağlantılar etkinleştirilmelidir

## <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5 Hassas verileri tanımlamak için etkin bir keşif aracı kullanın

- SQL veritabanlarınızdaki hassas veriler sınıflandırılmalıdır
- SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir
- SQL yönetilen örneklerinizde gelişmiş veri güvenliği etkinleştirilmelidir

## <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6 Kaynaklara erişimi denetlemek için Azure RBAC'ı kullanın

- Rol Tabanlı Erişim Kontrolü (RBAC) Kubernetes Hizmetlerinde kullanılmalıdır
- Özel RBAC kurallarının denetim kullanımı

## <a name="48-encrypt-sensitive-information-at-rest"></a>4.8 Hassas bilgileri istirahatte şifreleme

- SQL veritabanlarında Saydam Veri Şifrelemesi etkinleştirilmelidir
- Disk şifreleme sanal makinelerde uygulanmalıdır
- Eklenmemiş diskler şifrelenmelidir
- SQL server TDE koruyucusu kendi anahtarınızla şifrelenmelidir
- SQL yönetilen örnek TDE koruyucukendi anahtarı ile şifrelenmelidir
- Otomasyon hesabı değişkenleri şifrelenmelidir
- Service Fabric kümeleri ClusterProtectionLevel özelliğini EncryptAndSign olarak ayarlamalıdır

## <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9 Kritik Azure kaynaklarında yapılan değişikliklerde oturum açma ve uyarı

- Azure Monitor tüm bölgelerden etkinlik günlükleri toplamalıdır

## <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1 Otomatik güvenlik açığı tarama araçlarını çalıştırın

- SQL sunucularınızda güvenlik açığı değerlendirmesi etkinleştirilmelidir
- SQL yönetilen örneklerinizde güvenlik açığı değerlendirmesi etkinleştirilmelidir
- \[Sanal\] Makinelerde Önizleme Güvenlik Açığı Değerlendirmesi etkinleştirilmelidir
- SQL sunucusu için Güvenlik Açığı Değerlendirme ayarları, tarar raporları almak için bir e-posta adresi içermelidir

## <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2 Otomatik işletim sistemi yama yönetimi çözümlerini dağıtma

- Makinelerinize sistem güncelleştirmeleri yüklenmelidir
- Sanal makine ölçek kümelerinde sistem güncellemeleri yüklenmelidir
- İşlev Uygulamasının bir parçası olarak kullanıldığında '.NET Framework' sürümünün en son sürüm olduğundan emin olun
- Web uygulamasının bir parçası olarak kullanıldığında '.NET Framework' sürümünün en son sürüm olduğundan emin olun
- API uygulamasının bir parçası olarak kullanıldığında '.NET Framework' sürümünün en son sürüm olduğundan emin olun

## <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3 Otomatik üçüncü taraf yazılım yama yönetimi çözümlerini dağıtma

- Api uygulamasının bir parçası olarak kullanıldığında 'PHP sürümü'nün en son sürüm olduğundan emin olun
- WEB uygulamasının bir parçası olarak kullanıldığında 'PHP sürümü'nün en son sürüm olduğundan emin olun
- Fonksiyon uygulamasının bir parçası olarak kullanıldığında 'PHP sürümü'nün en son sürüm olduğundan emin olun
- Web uygulamasının bir parçası olarak kullanılıyorsa, 'Java sürümü'nün en son sürüm olduğundan emin olun
- İşlev uygulamasının bir parçası olarak kullanıldığında 'Java sürümü'nün en son sürüm olduğundan emin olun
- Api uygulamasının bir parçası olarak kullanıldığında 'Java sürümü'nün en son sürüm olduğundan emin olun
- Web uygulamasının bir parçası olarak kullanılıyorsa ,'Python sürümü'nün en son sürüm olduğundan emin olun
- İşlev uygulamasının bir parçası olarak kullanılıyorsa ,'Python sürümü'nün en son sürüm olduğundan emin olun
- Api uygulamasının bir parçası olarak kullanılıyorsa ,'Python sürümü'nün en son sürüm olduğundan emin olun
- Kubernetes Hizmetleri savunmasız olmayan bir Kubernetes sürümüne yükseltilmelidir

## <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5 Keşfedilen güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemini kullanma

- Güvenlik açıkları, Güvenlik Açığı Değerlendirmesi çözümü yle düzeltilmelidir
- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Sanal makine ölçeği kümelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Kapsayıcı güvenlik yapılandırmalarında güvenlik açıkları düzeltilmelidir
- SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir

## <a name="68-use-only-approved-applications"></a>6.8 Yalnızca onaylı uygulamaları kullanın

- Güvenlik Merkezi standart fiyatlandırma katmanı seçilmelidir
- Uyarlanabilir Uygulama Denetimleri sanal makinelerde etkinleştirilmelidir

## <a name="69-use-only-approved-azure-services"></a>6.9 Yalnızca onaylanmış Azure hizmetlerini kullanma

- Sanal makineler yeni Azure Kaynak Yöneticisi kaynaklarına geçirilmelidir
- Depolama hesapları yeni Azure Kaynak Yöneticisi kaynaklarına geçirilmelidir

## <a name="610-implement-approved-application-list"></a>6.10 Onaylı başvuru listesini uygulayın

- Güvenlik Merkezi standart fiyatlandırma katmanı seçilmelidir
- Uyarlanabilir Uygulama Denetimleri sanal makinelerde etkinleştirilmelidir

## <a name="73-maintain-secure-azure-resource-configurations"></a>7.3 Güvenli Azure kaynak yapılandırmalarını koruyun

- \[Önizleme\]: Pod Güvenlik İlkeleri Kubernetes Hizmetlerinde tanımlanmalıdır

## <a name="74-maintain-secure-operating-system-configurations"></a>7.4 Güvenli işletim sistemi yapılandırmalarını koruyun

- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Kapsayıcı güvenlik yapılandırmalarında güvenlik açıkları düzeltilmelidir
- Sanal makine ölçeği kümelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir

## <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9 Azure hizmetleri için otomatik yapılandırma izleme uygulaması

- \[Önizleme\]: Pod Güvenlik İlkeleri Kubernetes Hizmetlerinde tanımlanmalıdır

## <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10 İşletim sistemleri için otomatik yapılandırma izleme uygulayın

- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Kapsayıcı güvenlik yapılandırmalarında güvenlik açıkları düzeltilmelidir
- Sanal makine ölçeği kümelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir

## <a name="711-manage-azure-secrets-securely"></a>7.11 Azure sırlarını güvenli bir şekilde yönetme

- Key Vault nesneleri kurtarılabilir olmalıdır

## <a name="712-manage-identities-securely-and-automatically"></a>7.12 Kimlikleri güvenli ve otomatik olarak yönetme

- İşlev Uygulamanızda yönetilen kimlik kullanılmalıdır
- Yönetilen kimlik Web Uygulamanızda kullanılmalıdır
- Yönetilen kimlik API Uygulamanızda kullanılmalıdır

## <a name="81-use-centrally-managed-anti-malware-software"></a>8.1 Merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımlarını kullanma

- Azure Güvenlik Merkezi'nde eksik Uç Nokta Koruması'nı izleme
- Uç nokta koruma çözümü sanal makine ölçek kümeleri üzerine kurulmalıdır

## <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2 İşlemyapmayan Azure kaynaklarına yüklenecek ön taraya dosyaları

- Güvenlik Merkezi standart fiyatlandırma katmanı seçilmelidir

## <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3 Kötü amaçlı yazılımdan koruma yazılımının ve imzalarının güncelleştirilmesini sağlamak

- Azure için Microsoft Antimalware, koruma imzalarını otomatik olarak güncelleştirmek üzere yapılandırılmalıdır

## <a name="91-ensure-regular-automated-back-ups"></a>9.1 Düzenli otomatik yedeklemelerden emin olun

- Azure SQL Veritabanları için uzun vadeli coğrafi yedekli yedekleme etkinleştirilmelidir
- MySQL için Azure Veritabanı için coğrafi yedekli yedekleme etkinleştirilmelidir
- PostgreSQL için Azure Veritabanı için coğrafi yedekli yedekleme etkinleştirilmelidir
- MariaDB için Azure Veritabanı için coğrafi yedekli yedekleme etkinleştirilmelidir
- Sanal Makineler için Azure Yedekleme etkinleştirilmelidir

## <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2 Komple sistem yedeklemelerini gerçekleştirin ve müşteri yönetilen anahtarları yedekleme

- Azure SQL Veritabanları için uzun vadeli coğrafi yedekli yedekleme etkinleştirilmelidir
- MySQL için Azure Veritabanı için coğrafi yedekli yedekleme etkinleştirilmelidir
- PostgreSQL için Azure Veritabanı için coğrafi yedekli yedekleme etkinleştirilmelidir
- MariaDB için Azure Veritabanı için coğrafi yedekli yedekleme etkinleştirilmelidir
- Sanal Makineler için Azure Yedekleme etkinleştirilmelidir

## <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4 Yedeklerin ve müşteri yönetilen anahtarlarının korunmasını sağlayın

- Key Vault nesneleri kurtarılabilir olmalıdır

## <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2 Olay puanlama ve öncelik lendirme prosedürü oluşturma

- Güvenlik Merkezi standart fiyatlandırma katmanı seçilmelidir

## <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4 Güvenlik olayı iletişim bilgilerini sağlayın ve güvenlik olayları için uyarı bildirimlerini yapılandırma

- Aboneliğiniz için bir güvenlik kişisi e-posta adresi sağlanmalıdır
- Aboneliğiniz için bir güvenlik kişi telefon numarası sağlanmalıdır
- SQL sunucusu için gelişmiş veri güvenliği ayarları, güvenlik uyarıları almak için bir e-posta adresi içermelidir
- SQL yönetilen örnek için gelişmiş veri güvenliği ayarları güvenlik uyarıları almak için bir e-posta adresi içermelidir
- Yöneticilere ve abonelik sahiplerine e-posta bildirimleri SQL sunucusu gelişmiş veri güvenliği ayarlarında etkinleştirilmelidir
- Yöneticilere ve abonelik sahiplerine e-posta bildirimleri SQL yönetilen örnek gelişmiş veri güvenliği ayarlarında etkinleştirilmelidir

## <a name="next-steps"></a>Sonraki adımlar

Azure Güvenlik Kıyaslama planının denetim eşlemini gözden geçirdiğiniz için, girişimi atamak için Azure portalında Azure Politikası'nı ziyaret edin:

> [!div class="nextstepaction"]
> [Azure İlkesi](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyMenuBlade/Definitions)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.