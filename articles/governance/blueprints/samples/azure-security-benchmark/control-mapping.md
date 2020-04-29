---
title: Azure Güvenlik kıyaslama şeması örnek denetimleri
description: Azure Güvenlik kıyaslaması şeması örneği ile Azure Ilkesi arasındaki eşlemeyi denetleyin.
ms.date: 04/16/2020
ms.topic: sample
ms.openlocfilehash: 0f2d24d7d8c9f7ce2568f11b15e65ed1fcd02afb
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81538736"
---
# <a name="control-mapping-of-the-azure-security-benchmark-blueprint-sample"></a>Azure Güvenlik kıyaslaması şeması örneğinin denetim eşlemesi

Aşağıdaki makalede Azure Güvenlik kıyaslaması, Azure Güvenlik kıyaslama denetimleri ile Azure Güvenlik kıyaslaması şeması örneği hakkında ayrıntılı bilgi verilmiştir. Denetimler hakkında daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması](https://docs.microsoft.com/azure/security/benchmarks/overview).

Aşağıdaki eşlemeler **Azure Güvenlik kıyaslama** denetimlerine göre yapılır. Sağ taraftaki gezinmeyi kullanarak doğrudan belirli bir denetim eşlemesine atlayın. Eşlenen denetimler bir [Azure Policy](../../../policy/overview.md) girişimi ile uygulanır. Tüm girişimi gözden geçirmek için Azure portal **ilkeyi** açın ve **tanımlar** sayfasını seçin. Ardından, ** \[önizlemeyi\]bulun ve seçin: Azure Güvenlik kıyaslama önerilerini denetleyin ve belirli destekleyici VM uzantıları** yerleşik ilke girişim 'yı dağıtın.

> [!IMPORTANT]
> Aşağıdaki her denetim bir veya daha fazla [Azure ilke](../../../policy/overview.md) tanımı ile ilişkilidir. Bu ilkeler, denetimiyle [uyumluluğu değerlendirmenize](../../../policy/how-to/get-compliance-data.md) yardımcı olabilir; Ancak, bir denetim ve bir veya daha fazla ilke arasında genellikle bir 1:1 veya bir eşleşme yoktur. Bu nedenle, Azure Ilkesi ile **uyumlu** , yalnızca ilkelerin kendilerine başvurur; Bu, bir denetimin tüm gereksinimleriyle tamamen uyumlu olduğunuzdan emin değildir. Buna ek olarak, uyumluluk standardı şu anda herhangi bir Azure Ilke tanımı tarafından değinilmemiş denetimleri içerebilir. Bu nedenle, Azure Ilkesinde uyumluluk, genel uyumluluk durumunuzu yalnızca kısmi görünümüdür. Bu uyumluluk şeması örneği için denetimler ve Azure Ilke tanımları arasındaki ilişkilendirmeler zaman içinde değişebilir. Değişiklik geçmişini görüntülemek için [GitHub kayıt geçmişine](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/azure-security-benchmark/control-mapping.md)bakın.

## <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1 sanal ağınızdaki ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruma

- Alt ağlar bir ağ güvenlik grubuyla ilişkilendirilmelidir
- Uyarlamalı ağ sağlamlaştırma önerileri internet 'e yönelik sanal makinelere uygulanmalıdır
- Sanal makinelerin onaylanan bir sanal ağa bağlanması gerekir
- Internet 'e yönelik sanal makineler ağ güvenlik gruplarıyla korunmalıdır
- Service Bus bir sanal ağ hizmeti uç noktası kullanmalıdır
- App Service bir sanal ağ hizmeti uç noktası kullanmalıdır
- SQL Server bir sanal ağ hizmeti uç noktası kullanmalıdır
- Olay Hub 'ı bir sanal ağ hizmeti uç noktası kullanmalıdır
- Cosmos DB bir sanal ağ hizmeti uç noktası kullanmalıdır
- Key Vault bir sanal ağ hizmeti uç noktası kullanmalıdır
- Depolama hesaplarına Kısıtlanmamış ağ erişimini denetleme
- Depolama hesapları bir sanal ağ hizmeti uç noktası kullanmalıdır
- Container Registry bir sanal ağ hizmeti uç noktası kullanmalıdır
- Sanal ağlar belirtilen sanal ağ geçidini kullanmalıdır
- Yetkili IP aralıkları, Kubernetes hizmetlerinde tanımlanmalıdır
- \[Önizleme\]: sanal makinenizde IP iletimi devre dışı bırakılmalıdır
- Internet 'e yönelik sanal makineler ağ güvenlik gruplarıyla korunmalıdır
- Sanal makinelere anlık ağ erişim denetimi uygulanmalıdır
- Yönetim bağlantı noktaları sanal makinelerinizde kapatılmalıdır

## <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2 sanal ağlar, alt ağlar ve NIC 'lerin yapılandırma ve trafiğini izleme ve günlüğe kaydetme

- Ağ Izleyicisi etkinleştirilmelidir

## <a name="13-protect-critical-web-applications"></a>1,3 kritik Web uygulamalarını koruma

- WEB uygulamasının ' Istemci sertifikaları (gelen istemci sertifikaları) ' olarak ' on ' olarak ayarlandığından emin olun
- CORS, her kaynağın Web uygulamalarınıza erişmesine izin vermemelidir
- CORS, her kaynağın Işlev uygulamalarınıza erişmesine izin vermemelidir
- CORS, her kaynağın API uygulamanıza erişmesine izin vermemelidir
- Web uygulamaları için uzaktan hata ayıklama kapatılmalıdır
- Işlev uygulamaları için uzaktan hata ayıklama kapatılmalıdır
- API Apps için uzaktan hata ayıklama kapatılmalıdır

## <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4 bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

- DDoS koruma standardı etkinleştirilmelidir
- Sanal makinelere anlık ağ erişim denetimi uygulanmalıdır
- Uyarlamalı ağ sağlamlaştırma önerileri internet 'e yönelik sanal makinelere uygulanmalıdır

## <a name="15-record-network-packets-and-flow-logs"></a>1,5 ağ paketlerini ve akış günlüklerini kaydetme

- Ağ Izleyicisi etkinleştirilmelidir

## <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11 ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanma

- ' Güvenlik seçenekleri-ağ erişimi 'nde Windows VM 'Leri yapılandırmasını denetlemek için önkoşulları dağıtın
- ' Güvenlik seçenekleri-Microsoft ağ Istemcisi ' içindeki Windows VM yapılandırmalarından denetim sonuçlarını göster
- ' Güvenlik seçenekleri-ağ güvenliği ' içindeki Windows VM 'Leri yapılandırmasını denetlemek için önkoşulları dağıtın
- ' Güvenlik seçenekleri-ağ güvenliği ' içindeki Windows VM yapılandırmalarından denetim sonuçlarını göster
- ' Güvenlik seçenekleri-Microsoft ağ sunucusu ' 'ndaki Windows VM 'Leri yapılandırmasını denetlemek için önkoşulları dağıtın
- ' Güvenlik seçenekleri-Microsoft ağ sunucusu ' 'ndaki Windows VM yapılandırmalarından denetim sonuçlarını göster
- ' Yönetim Şablonları-Network ' içindeki Windows VM 'Leri yapılandırmasını denetlemek için önkoşulları dağıtın
- ' Yönetim Şablonları-Network ' içindeki Windows VM yapılandırmalarından denetim sonuçlarını göster

## <a name="22-configure-central-security-log-management"></a>2,2 Merkezi güvenlik günlüğü yönetimini yapılandırma

- Log Analytics Aracısı sanal makinelere yüklenmelidir
- Log Analytics Aracısı sanal makine ölçek kümelerine yüklenmelidir
- Log Analytics aracısının beklendiği gibi bağlı olmadığı Windows sanal makinelerini denetlemek için önkoşulları dağıtın
- Log Analytics aracısının beklendiği gibi bağlı olmadığı Windows VM 'lerinden denetim sonuçlarını göster
- Azure Izleyici günlük profili, ' Write, ' ' DELETE, ' ve ' Action ' kategorilerinin günlüklerini toplamalıdır
- Azure Izleyici, tüm bölgelerdeki etkinlik günlüklerini toplamalıdır
- Log Analytics izleme aracısının otomatik olarak sağlanması aboneliğinizde etkinleştirilmelidir

## <a name="23-enable-audit-logging-for-azure-resources"></a>2,3 Azure kaynakları için denetim günlüğünü etkinleştirme

- Azure Data Lake Store tanılama günlükleri etkinleştirilmelidir
- Logic Apps tanılama günlükleri etkinleştirilmelidir
- IoT Hub tanılama günlükleri etkinleştirilmelidir
- Batch hesaplarındaki tanılama günlükleri etkinleştirilmelidir
- Sanal makine ölçek kümelerindeki tanılama günlükleri etkinleştirilmelidir
- Olay Hub 'ındaki tanılama günlükleri etkinleştirilmelidir
- Arama hizmetlerindeki tanılama günlükleri etkinleştirilmelidir
- Uygulama hizmetlerindeki tanılama günlükleri etkinleştirilmelidir
- Data Lake Analytics tanılama günlükleri etkinleştirilmelidir
- Key Vault tanılama günlükleri etkinleştirilmelidir
- Service Bus tanılama günlükleri etkinleştirilmelidir
- Azure Stream Analytics tanılama günlükleri etkinleştirilmelidir
- SQL Server üzerinde denetim etkinleştirilmelidir
- Tanılama ayarını denetle

## <a name="24-collect-security-logs-from-operating-systems"></a>2,4 işletim sistemlerinden güvenlik günlüklerini topla

- Log Analytics izleme aracısının otomatik olarak sağlanması aboneliğinizde etkinleştirilmelidir
- Log Analytics Aracısı sanal makinelere yüklenmelidir
- Log Analytics Aracısı sanal makine ölçek kümelerine yüklenmelidir
- Log Analytics aracısının beklendiği gibi bağlı olmadığı Windows sanal makinelerini denetlemek için önkoşulları dağıtın
- Log Analytics aracısının beklendiği gibi bağlı olmadığı Windows VM 'lerinden denetim sonuçlarını göster

## <a name="27-enable-alerts-for-anomalous-activity"></a>2,7 anormal etkinlik için uyarıları etkinleştir

- Güvenlik Merkezi Standart fiyatlandırma katmanı seçilmelidir
- Gelişmiş veri güvenliği SQL sunucularınızda etkinleştirilmelidir
- Gelişmiş veri güvenliği, SQL yönetilen örneklerinizin üzerinde etkinleştirilmelidir
- Gelişmiş tehdit koruması türleri SQL Server Gelişmiş veri güvenliği ayarları 'nda ' tümü ' olarak ayarlanmalıdır
- Gelişmiş tehdit koruması türleri SQL yönetilen örneği gelişmiş veri güvenliği ayarları içinde ' All ' olarak ayarlanmalıdır

## <a name="28-centralize-anti-malware-logging"></a>2,8 bir kötü amaçlı yazılımdan koruma oluşturmayı merkezileştirme

- Azure için Microsoft kötü amaçlı yazılımdan koruma imzaları otomatik olarak güncelleştirilecek şekilde yapılandırılmalıdır
- Azure Güvenlik Merkezi 'nde eksik Endpoint Protection izleme
- Uç nokta koruma çözümü, sanal makine ölçek kümelerine yüklenmelidir

## <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1 Yönetim hesaplarının envanterini tutma

- Aboneliğiniz için en fazla 3 sahip belirtilmelidir
- Aboneliğinize birden fazla sahip atanmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan kullanım dışı hesaplar aboneliğinizden kaldırılmalıdır

## <a name="33-use-dedicated-administrative-accounts"></a>3,3 adanmış yönetim hesapları kullan

- Yöneticiler grubunun yalnızca belirtilen üyeleri içermediği Windows sanal makinelerini denetlemek için önkoşulları dağıtın
- Yöneticiler grubunun yalnızca belirtilen üyeleri içermediği Windows VM 'lerinden denetim sonuçlarını göster
- Yöneticiler grubunun belirtilen tüm üyeleri içermediği Windows sanal makinelerini denetlemek için önkoşulları dağıtın
- Yöneticiler grubunun belirtilen tüm üyeleri içermediği Windows VM 'lerinden denetim sonuçlarını göster
- Yöneticiler grubunun belirtilen üyelerden birini içerdiği Windows sanal makinelerini denetlemek için önkoşulları dağıtın
- Yöneticiler grubunun belirtilen üyelerden birini içerdiği Windows VM 'lerinden denetim sonuçlarını göster
- Aboneliğiniz için en fazla 3 sahip belirtilmelidir
- Aboneliğinize birden fazla sahip atanmalıdır

## <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5 tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

- MFA, aboneliğinizde sahip izinleri olan hesaplarda etkinleştirilmelidir
- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir
- MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir

## <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7 Yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

- Güvenlik Merkezi Standart fiyatlandırma katmanı seçilmelidir

## <a name="39-use-azure-active-directory"></a>3,9 Azure Active Directory kullanımı

- SQL sunucuları için bir Azure Active Directory Yöneticisi sağlanmalıdır
- Service Fabric kümeler yalnızca istemci kimlik doğrulaması için Azure Active Directory kullanmalıdır
- Azure Active Directory ile kaydolma özelliğinin API uygulamasında etkinleştirildiğinden emin olun
- Azure Active Directory kaydetme özelliğinin WEB uygulamasında etkinleştirildiğinden emin olun
- Azure Active Directory kayıt İşlev Uygulaması etkinleştirildiğinden emin olun

## <a name="310-regularly-review-and-reconcile-user-access"></a>3,10 Kullanıcı erişimini düzenli olarak gözden geçirin ve mutabık kıl

- Kullanım dışı bırakılan hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan kullanım dışı hesaplar aboneliğinizden kaldırılmalıdır
- Okuma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir
- Yazma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

## <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1 hassas bilgilerin envanterini tutma

- SQL veritabanlarınızdaki hassas verilerin sınıflandırılmış olması gerekir

## <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4 tüm hassas bilgileri iletimde şifreleyin

- Depolama hesaplarına güvenli aktarım etkinleştirilmelidir
- API uygulamanızda en son TLS sürümü kullanılmalıdır
- Web uygulamanızda en son TLS sürümü kullanılmalıdır
- İşlev Uygulaması en son TLS sürümü kullanılmalıdır
- İşlev Uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır
- Web uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır
- API uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır
- MySQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir
- PostgreSQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir
- Yalnızca Redis Cache güvenli bağlantılar etkinleştirilmelidir

## <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5 hassas verileri belirlemek için etkin bir keşif aracı kullanma

- SQL veritabanlarınızdaki hassas verilerin sınıflandırılmış olması gerekir
- Gelişmiş veri güvenliği SQL sunucularınızda etkinleştirilmelidir
- Gelişmiş veri güvenliği, SQL yönetilen örneklerinizin üzerinde etkinleştirilmelidir

## <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6 kaynaklara erişimi denetlemek için Azure RBAC kullanma

- Rol tabanlı Access Control (RBAC), Kubernetes hizmetlerinde kullanılmalıdır
- Özel RBAC kurallarının kullanımını denetleme

## <a name="48-encrypt-sensitive-information-at-rest"></a>4,8 Rest 'de hassas bilgileri şifreleyin

- SQL veritabanlarındaki Saydam Veri Şifrelemesi etkinleştirilmelidir
- Disk şifrelemesi sanal makinelere uygulanmalıdır
- Eklenmemiş diskler şifrelenmelidir
- SQL Server TDE koruyucusu kendi anahtarınızla şifrelenmelidir
- SQL yönetilen örnek TDE koruyucusu kendi anahtarınızla şifrelenmelidir
- Otomasyon hesabı değişkenleri şifrelenmelidir
- Service Fabric kümelerinde ClusterProtectionLevel özelliği EncryptAndSign olarak ayarlanmalıdır

## <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>önemli Azure kaynaklarında yapılan değişikliklerle 4,9 günlük ve uyarı

- Azure Izleyici, tüm bölgelerdeki etkinlik günlüklerini toplamalıdır

## <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1 otomatik güvenlik açığı tarama araçlarını Çalıştır

- Güvenlik açığı değerlendirmesi SQL sunucularınızda etkinleştirilmelidir
- SQL yönetilen örneklerinizin üzerinde güvenlik açığı değerlendirmesi etkinleştirilmelidir
- \[Önizleme\] güvenlik açığı değerlendirmesi sanal makinelerde etkinleştirilmelidir
- SQL Server için güvenlik açığı değerlendirmesi ayarları, tarama raporlarını alacak bir e-posta adresi içermelidir

## <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2 otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

- Makinelerinize sistem güncelleştirmeleri yüklenmelidir
- Sanal makine ölçek kümelerindeki sistem güncelleştirmeleri yüklenmelidir
- İşlev Uygulaması bir parçası olarak kullanılıyorsa ' .NET Framework ' sürümünün en son sürümü olduğundan emin olun
- Web uygulamasının bir parçası olarak kullanılıyorsa ' .NET Framework ' sürümünün en son sürümü olduğundan emin olun
- API uygulamasının bir parçası olarak kullanılıyorsa ' .NET Framework ' sürümünün en son sürümü olduğundan emin olun

## <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3 otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

- API uygulamasının bir parçası olarak kullanılıyorsa ' PHP Version ' öğesinin en son sürümü olduğundan emin olun
- WEB uygulamasının bir parçası olarak kullanılıyorsa, ' PHP Version ' öğesinin en son sürümü olduğundan emin olun
- Işlev uygulamasının bir parçası olarak kullanılıyorsa, ' PHP Version ' öğesinin en son sürümü olduğundan emin olun
- Web uygulamasının bir parçası olarak kullanılıyorsa ' Java sürümü 'nin en son sürümü olduğundan emin olun
- Işlev uygulamasının bir parçası olarak kullanılıyorsa ' Java sürümü ' nin en son sürümü olduğundan emin olun
- API uygulamasının bir parçası olarak kullanılıyorsa ' Java sürümü ' nin en son sürümü olduğundan emin olun
- Web uygulamasının bir parçası olarak kullanılıyorsa ' Python sürümü 'nin en son bir sürüm olduğundan emin olun
- Işlev uygulamasının bir parçası olarak kullanılıyorsa ' Python sürümü 'nin en son bir sürüm olduğundan emin olun
- API uygulamasının bir parçası olarak kullanılıyorsa ' Python sürümü 'nin en son bir sürüm olduğundan emin olun
- Kubernetes Hizmetleri, güvenlik açığı olmayan bir Kubernetes sürümüne yükseltilmelidir

## <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5 bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

- Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir
- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Sanal makine ölçek kümelerinizin güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Kapsayıcı güvenlik yapılandırmalarında güvenlik açıkları düzeltilmelidir
- SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir

## <a name="68-use-only-approved-applications"></a>6,8 yalnızca onaylanan uygulamaları kullan

- Güvenlik Merkezi Standart fiyatlandırma katmanı seçilmelidir
- Uyarlamalı uygulama denetimleri sanal makinelerde etkinleştirilmelidir

## <a name="69-use-only-approved-azure-services"></a>6,9 yalnızca onaylanan Azure hizmetlerini kullan

- Sanal makinelerin yeni Azure Resource Manager kaynaklara geçirilmesi gerekir
- Depolama hesaplarının yeni Azure Resource Manager kaynaklarına geçirilmesi gerekir

## <a name="610-implement-approved-application-list"></a>6,10 onaylanan uygulama listesini Uygula

- Güvenlik Merkezi Standart fiyatlandırma katmanı seçilmelidir
- Uyarlamalı uygulama denetimleri sanal makinelerde etkinleştirilmelidir

## <a name="73-maintain-secure-azure-resource-configurations"></a>7,3 güvenli Azure Kaynak yapılandırmalarının bakımını yapma

- \[Önizleme\]: Kubernetes hizmetlerinde Pod güvenlik ilkeleri tanımlanmalıdır

## <a name="74-maintain-secure-operating-system-configurations"></a>7,4 güvenli işletim sistemi yapılandırmalarının bakımını yapma

- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Kapsayıcı güvenlik yapılandırmalarında güvenlik açıkları düzeltilmelidir
- Sanal makine ölçek kümelerinizin güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir

## <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9 Azure hizmetleri için otomatik yapılandırma izlemeyi uygulama

- \[Önizleme\]: Kubernetes hizmetlerinde Pod güvenlik ilkeleri tanımlanmalıdır

## <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10 işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Kapsayıcı güvenlik yapılandırmalarında güvenlik açıkları düzeltilmelidir
- Sanal makine ölçek kümelerinizin güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir

## <a name="711-manage-azure-secrets-securely"></a>7,11 Azure gizli dizilerini güvenli bir şekilde yönetme

- Key Vault nesneler kurtarılabilir olmalıdır

## <a name="712-manage-identities-securely-and-automatically"></a>7,12 kimlikleri güvenli ve otomatik olarak yönetme

- Yönetilen kimlik İşlev Uygulaması kullanılmalıdır
- Yönetilen kimliğin Web uygulamanızda kullanılması gerekir
- Yönetilen kimlik API uygulamanızda kullanılmalıdır

## <a name="81-use-centrally-managed-anti-malware-software"></a>8,1 merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

- Azure Güvenlik Merkezi 'nde eksik Endpoint Protection izleme
- Uç nokta koruma çözümü, sanal makine ölçek kümelerine yüklenmelidir

## <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2 işlem dışı Azure kaynaklarına yüklenecek ön tarama dosyaları

- Güvenlik Merkezi Standart fiyatlandırma katmanı seçilmelidir

## <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3 kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

- Azure için Microsoft kötü amaçlı yazılımdan koruma imzaları otomatik olarak güncelleştirilecek şekilde yapılandırılmalıdır

## <a name="91-ensure-regular-automated-back-ups"></a>9,1 düzenli otomatik yedeklemeler sağlayın

- Azure SQL veritabanları için uzun vadeli coğrafi olarak yedekli yedeklemenin etkinleştirilmesi gerekir
- MySQL için Azure veritabanı 'nda coğrafi olarak yedekli yedekleme etkinleştirilmelidir
- PostgreSQL için Azure veritabanı için coğrafi olarak yedekli yedekleme etkinleştirilmelidir
- MariaDB için Azure veritabanı 'nda coğrafi olarak yedekli yedekleme etkinleştirilmelidir
- Sanal makineler için Azure Backup etkinleştirilmelidir

## <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2 tüm sistem yedeklemeleri gerçekleştirme ve müşterinin yönettiği tüm anahtarları yedekleme

- Azure SQL veritabanları için uzun vadeli coğrafi olarak yedekli yedeklemenin etkinleştirilmesi gerekir
- MySQL için Azure veritabanı 'nda coğrafi olarak yedekli yedekleme etkinleştirilmelidir
- PostgreSQL için Azure veritabanı için coğrafi olarak yedekli yedekleme etkinleştirilmelidir
- MariaDB için Azure veritabanı 'nda coğrafi olarak yedekli yedekleme etkinleştirilmelidir
- Sanal makineler için Azure Backup etkinleştirilmelidir

## <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4 yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

- Key Vault nesneler kurtarılabilir olmalıdır

## <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2 olay Puanlama ve öncelik belirleme yordamı oluşturma

- Güvenlik Merkezi Standart fiyatlandırma katmanı seçilmelidir

## <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4 güvenlik olayları iletişim ayrıntılarını sağlar ve güvenlik olayları için uyarı bildirimleri yapılandırın

- Aboneliğiniz için bir güvenlik iletişim e-posta adresi belirtilmelidir
- Aboneliğiniz için bir güvenlik ilgili kişisi telefon numarası belirtilmelidir
- SQL Server için gelişmiş veri güvenliği ayarları, güvenlik uyarılarını almak için bir e-posta adresi içermelidir
- SQL yönetilen örneği için gelişmiş veri güvenliği ayarları, güvenlik uyarılarını almak için bir e-posta adresi içermelidir
- SQL Server Gelişmiş veri güvenliği ayarları 'nda Yöneticiler ve abonelik sahiplerine e-posta bildirimleri etkinleştirilmelidir
- SQL yönetilen örnek gelişmiş veri güvenliği ayarları 'nda yöneticilere ve abonelik sahiplerine e-posta bildirimleri etkinleştirilmelidir

## <a name="next-steps"></a>Sonraki adımlar

Azure Güvenlik kıyaslaması şeması 'nın denetim eşlemesini gözden geçirdiğinize göre, girişimi atamak için Azure portal Azure Ilkesi ' ni ziyaret edin:

> [!div class="nextstepaction"]
> [Azure İlkesi](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyMenuBlade/Definitions)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.