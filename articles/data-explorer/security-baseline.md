---
title: Veri Gezgini için Azure Güvenlik Taban Çizgisi
description: Veri Gezgini için Azure Güvenlik Taban Çizgisi
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 92a607e4c17a16d07f07c97f626ed98a213d509b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80289725"
---
# <a name="azure-security-baseline-for-data-explorer"></a>Veri Gezgini için Azure Güvenlik Taban Çizgisi

Veri Gezgini için Azure Güvenlik Taban Çizgisi, dağıtımınızın güvenlik duruşunu geliştirmenize yardımcı olacak öneriler içerir.

Bu hizmetin temeli, en iyi uygulamalar kılavuzumuzla Azure'da bulut çözümlerinizi nasıl güvenebileceğinize ilişkin öneriler sunan [Azure Güvenlik Kıyaslama sürümü 1.0'dan](https://docs.microsoft.com/azure/security/benchmarks/overview)alınmıştır.

Daha fazla bilgi için [Azure Güvenlik Taban Çizgilerini genel olarak](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)görün.

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için [Güvenlik Denetimi: Ağ Güvenliği'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security)bakın.*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Sanal Ağınızdaki Ağ Güvenlik Grupları veya Azure Güvenlik Duvarı'nı kullanarak kaynakları koruyun

**Yol gösterici**: Azure Veri Gezgini, sanal ağınızdaki bir kümeyi bir alt ağa dağıtmayı destekler. Bu özellik, Azure Veri Gezgini küme trafiğinizde ağ güvenlik grubu (NSG) kurallarını uygulamanızı, şirket içi ağınızı Azure Veri Gezgini kümesinin alt ağına bağlamanızı ve veri bağlantı kaynaklarınızı (Olay Hub'ı ve Olay İzni) hizmet bitiş noktaları.

Azure Veri Gezgini kümenizi sanal ağa nasıl dağıtabilirsiniz:https://docs.microsoft.com/azure/data-explorer/vnet-deployment


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Vnets, Subnets ve NICS'in yapılandırmasını ve trafiğini izleyin ve kaydedin

**Kılavuz**: Ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri Bir Depolama Hesabına gönderin.

NSG Akış Günlükleri Nasıl Etkinleştirilir:https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Azure Güvenlik Merkezi tarafından sağlanan Ağ Güvenliğini Anlama:https://docs.microsoft.com/azure/security-center/security-center-network-recommendations


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="13-protect-critical-web-applications"></a>1.3: Kritik Web Uygulamalarını Koruyun

**Rehberlik**: Geçerli değildir; Öneri, Azure Uygulama Hizmeti veya bilgi işlem kaynakları üzerinde çalışan web uygulamaları için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Bilinen Kötü Amaçlı IP Adresleriyle İletişimi İnkar Etme

**Yönerge**: DDoS saldırılarına karşı korunmak için Veri Gezgini kümelerinizi koruyan sanal ağda Azure DDoS Koruma Standardını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimi reddetmek için Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı kullanın.

DDoS koruması nasıl yapılandırılabilen:https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı Anlayın:https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1.5: Ağ Paketlerini ve Akış Günlüklerini Kaydetme

**Yönerge**: Azure Veri Gezgini kümenizi korumak için kullanılan ağ güvenlik gruplarındaki (NSG) Akış Günlüklerini etkinleştirin ve günlükleri trafik denetimi için bir Depolama Hesabına gönderin.

NSG Akış Günlükleri Nasıl Etkinleştirilir:https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems"></a>1.6: Ağ Tabanlı Saldırı Algılama/Saldırı Önleme Sistemleri

**Rehberlik**: Geçerli değildir; Bu denetim bitiş noktasında veya güvenlik duvarında yapılır.


**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="17-manage-traffic-to-your-web-applications"></a>1.7: Web uygulamalarınızdaki trafiği yönetme

**Rehberlik**: Geçerli değildir; Bu öneri, Azure Uygulama Hizmeti'nde çalışan web uygulamaları veya bilgi işlem kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Ağ güvenliği kurallarının karmaşıklığını ve yönetim ek lerini en aza indirmek

**Kılavuz**: Ağ Güvenlik Grupları'ndaki ağ erişim denetimlerini veya Azure Veri Gezgini kümelerinizle ilişkili Azure Güvenlik Duvarlarını tanımlamak için Sanal Ağ Hizmeti Etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında servis etiketi adını (örn. ApiManagement) belirterek, ilgili hizmetin trafiğine izin verebilir veya reddedebilirsiniz. Microsoft, hizmet etiketinin kapsadığı adres öneklerini yönetir ve adresler değiştikçe servis etiketini otomatik olarak güncelleştirir.

Hizmet Etiketlerini Anlama ve Kullanma:https://docs.microsoft.com/azure/virtual-network/service-tags-overview

Azure Veri Gezgini için Hizmet Etiketleri yapılandırma gereksinimleri:https://docs.microsoft.com/azure/data-explorer/vnet-deployment#dependencies-for-vnet-deployment


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Ağ Aygıtları için Standart Güvenlik Yapılandırmalarını Koruyun

**Yönerge**: Azure İlkesi ile ağ kaynakları için standart güvenlik yapılandırmalarını tanımlamak ve uygulamak için müşteri.

Müşteri, ARM şablonları, RBAC denetimleri ve ilkeleri gibi önemli ortam yapılarını tek bir plan tanımında paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure Planları'nı da kullanabilir. Planı yeni aboneliklere ve ortamlara kolayca uygulayın ve sürüm leme yoluyla denetim ve yönetime ince ayar uygulayın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Blueprint nasıl oluşturulur:https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal


**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="110-document-traffic-configuration-rules"></a>1.10: Belge Trafiği Yapılandırma Kuralları

**Kılavuz**: Veri Gezgini kümeleriniz için ağ güvenliği grupları (NSG) ve ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için etiketleri kullanın. Tek tek NSG kuralları için, bir ağa/aktan trafiğin alınmasına izin veren kurallar için iş gereksinimini ve/veya süresini (vb.) belirtmek için "Açıklama" alanını kullanın.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags


**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Ağ Kaynak Yapılandırmalarını İzlemek ve Değişiklikleri Algılamak için Otomatik Araçlar Kullanın

**Yönerge**: Ağ kaynakları için yapılandırmayı doğrulamak (ve/veya düzeltmek için) Azure İlkesi'ni kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için [Güvenlik Denetimi: Günlüğe kaydetme ve izleme.](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring)*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Onaylı Zaman Senkronizasyon Kaynaklarını Kullanma

**Kılavuz**: Microsoft Azure kaynakları için zaman kaynakları tutar, müşteriler müşteriye ait bilgi işlem dağıtımları için zaman eşitlemesi güncelleyebilir.

Azure bilgi işlem kaynakları için zaman eşitleme yapılandırması nasıl yapılandırılır:https://docs.microsoft.com/azure/virtual-machines/windows/time-sync

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2: Merkezi Güvenlik Günlüğü Yönetimini Yapılandırma

**Kılavuz :** Azure Veri Gezgini, yutma başarıları ve hataları hakkında öngörüler için tanılama günlüklerini kullanır. İşlem durumunu izlemek için işlem günlüklerini Azure Depolama, Etkinlik Hub'ı veya Log Analytics'e dışa aktarabilirsiniz.

Azure Veri Gezgini işlemleri nasıl izlenir:

https://docs.microsoft.com/azure/data-explorer/using-diagnostic-logs

Azure Veri Gezgini'nde izleme verilerini alma ve sorgulama:

https://docs.microsoft.com/azure/data-explorer/ingest-data-no-code

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Azure Kaynakları için denetim günlüğe kaydetmeyi etkinleştirme

**Yönerge**: Belirli işlemlere ve günlüğe erişmek için Azure Veri Gezgini tanılama ayarlarını etkinleştirin. Azure Etkinliği, kaynak hakkında üst düzey günlüğe kaydetmeyi içeren Azure Etkinliği günlükleri varsayılan olarak etkinleştirilir.

Azure Veri Gezgini işlemleri nasıl izlenir:https://docs.microsoft.com/azure/data-explorer/using-diagnostic-logs

Azure Monitor ile platform günlükleri ve ölçümleri nasıl toplanır:https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings

Azure platform günlüklerine genel bakış:https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="24-collect-security-logs-from-operating-system"></a>2.4: İşletim Sisteminden Güvenlik Günlüklerini Topla

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2.5: Güvenlik Günlüğü Depolama Tutma Yapılandırma

**Kılavuz**: Azure Monitor'da, Log Analytics Çalışma Alanı saklama sürenizi kuruluşunuzun uyumluluk yönetmeliklerine göre ayarlayın. Uzun vadeli/arşivdepolama için Azure Depolama Hesapları'nı kullanın.

Log Analytics Çalışma Alanları için günlük bekletme parametreleri nasıl ayarlanır:https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="26-monitor-and-review-logs"></a>2.6: İzleme ve İnceleme Günlükleri

**Rehberlik**: Anormal davranışlar için günlükleri analiz edin ve izleyin ve sonuçları düzenli olarak gözden geçirin. Azure Veri Gezgini için Tanı Lama Ayarlarını etkinleştirdikten sonra, günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Monitor'un Log Analytics Çalışma Alanını kullanın.

Günlük Analizi Çalışma Alanını Anlama:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal

Azure Monitor'da özel sorgular nasıl gerçekleştirililir:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7: Anormal Etkinlik Uyarıları Etkinleştir

**Rehberlik**: Geçerli değildir; Azure Veri Gezgini bu yeteneğe sahip değildir.


**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="28-centralize-anti-malware-logging"></a>2.8: Kötü Amaçlı Yazılımdan Koruma Günlüğü'ne Merkezileştirme

**Rehberlik**: Geçerli değildir; Azure Veri Gezgini kötü amaçlı yazılımdan koruma günlüğe kaydetmeişlemini işlemez.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="29-enable-dns-query-logging"></a>2.9: DNS Sorgu Günlüğe Kaydetmeyi Etkinleştir

**Kılavuz**: Geçerli değildir: DNS sorgusu Azure Veri Gezgini'nin bir işlevi değildir.


**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2.10: Komut Satırı Denetim Günlüğe Kaydetmeyi Etkinleştir

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için [güvenlik denetimi: kimlik ve erişim denetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control)bakın.*

### <a name="31-maintain-inventory-of-administrative-accounts"></a>3.1: İdari Hesapların Envanterini Koruyun

**Kılavuz :** Azure Veri Gezgini'nde Güvenlik rolleri, hangi güvenlik ilkelerinin (kullanıcılar ve uygulamalar) veritabanı veya tablo gibi güvenli bir kaynakta çalışma iznine sahip olduğunu ve hangi işlemlere izin verilip verilmeye izin verilebileni tanımlar.  Azure Veri Gezgini kümeleri ve veritabanları için yönetici rolündeki ilkeleri listelemek için Kusto sorgusundan yararlanabilirsiniz.
Kusto sorgusukullanarak Azure Veri Gezgini'nde güvenlik rolleri yönetimi:https://docs.microsoft.com/azure/kusto/management/security-roles



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Geçerli olduğu Durumlarda Varsayılan Parolaları Değiştir

**Yol gösterici**: Azure AD varsayılan parola kavramına sahip değildir. Parola gerektiren diğer Azure kaynakları, hizmete bağlı olarak farklılık gösteren karmaşıklık gereksinimleri ve minimum parola uzunluğuyla bir parola oluşturulmasını zorlar. Varsayılan parolaları kullanabilen üçüncü taraf uygulamalardan ve pazar yeri hizmetlerinden siz sorumlusunuz.


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="33-ensure-the-use-of-dedicated-administrative-accounts"></a>3.3: Özel İdari Hesapların Kullanımını Sağlamak

**Rehberlik**: Müşteri, özel yönetim hesaplarının kullanımı etrafında standart işletim prosedürleri oluşturmak için. Yönetim hesabı sayısını izlemek için Azure Güvenlik Merkezi Kimliği ve Erişim Yönetimi'ni kullanın.

Müşteriler ayrıca Microsoft Hizmetleri ve Azure ARM için Azure AD Ayrıcalıklı Kimlik Yönetimi Ayrıcalıklı Rolleri kullanarak Tam Zamanında / Yeterli Erişime olanak sağlayabilir. 

Azure AD Ayrıcalıklı Kimlik Yönetimi nedir?:https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="34-utilize-single-sign-on-sso-with-azure-active-directory"></a>3.4: Azure Active Directory ile Tek Oturum Açma (SSO) kullanımı

**Yönlendirme**: Mümkün olan her yerde, müşteri hizmet başına tek tek kimlik bilgilerini yapılandırmak yerine SSO'yı Azure Active Directory (Azure AD) ile kullanabilir. Azure Güvenlik Merkezi Kimlik ve Erişim Yönetimi önerilerini kullanın.

Azure AD ile SSO'nun anlaşılması:https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="35-use-multifactor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Tüm Azure Active Directory tabanlı erişim için Çok Faktörlü Kimlik Doğrulama'yı kullanın.

**Kılavuz :** Azure Etkin Dizin (Azure AD) çok faktörlü kimlik doğrulamasını (MFA) etkinleştirin ve Azure Güvenlik Merkezi Kimlik ve Erişim Yönetimi önerilerini izleyin.

Azure'da MFA nasıl etkinleştirilir:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted 

Azure Güvenlik Merkezi'nde kimlik ve erişim nasıl izlenir:https://docs.microsoft.com/azure/security-center/security-center-identity-access


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="36-use-of-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Tüm İdari Görevler için Özel Makinelerin (Ayrıcalıklı Erişim İş İstasyonları) Kullanımı

**Yönlendirme**: Azure kaynaklarına giriş yapmak ve yapılandırmak üzere yapılandırılan çok faktörlü kimlik doğrulama (MFA) ile PAW'ları (ayrıcalıklı erişim iş istasyonları) kullanın.

Ayrıcalıklı Erişim İş İstasyonları hakkında bilgi edinin:https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Azure'da MFA nasıl etkinleştirilir:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted


**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="37-log-and-alert-on-suspicious-activity-on-administrative-accounts"></a>3.7: İdari Hesaplarda Şüpheli Faaliyette Oturum Açma ve Uyarı

**Kılavuz :** Çevrede şüpheli veya güvensiz etkinlik oluştuğunda günlük ve uyarı oluşturma için Azure Active Directory (Azure AD) güvenlik raporlarını kullanın. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi'ni kullanın

Riskli etkinlikler için işaretlenen Azure AD kullanıcılarını nasıl tanımlarım:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk

Azure Güvenlik Merkezi'nde kullanıcıların kimlik ve erişim etkinliğini izleme:https://docs.microsoft.com/azure/security-center/security-center-identity-access


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="38-manage-azure-resource-from-only-approved-locations"></a>3.8: Azure Kaynağını yalnızca Onaylanan Konumlardan Yönetme

**Yönerge**: Müşteri, IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplandırmalarından erişime izin vermek için Koşullu Erişim adlı konumları kullanır.

Azure'da adlandırılmış konumlar nasıl yapılandırılabilen:https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="39-utilize-azure-active-directory"></a>3.9: Azure Etkin Dizininden Yararlanın

**Kılavuz :** Azure Active Directory (Azure AD), Azure Veri Gezgini'ne kimlik doğrulama için tercih edilen yöntemdir. Bir dizi kimlik doğrulama senaryosudestekler:

Kullanıcı kimlik doğrulaması (etkileşimli oturum açma): İnsan ilkelerini doğrulamak için kullanılır.

Uygulama kimlik doğrulaması (etkileşimli olmayan oturum açma): Hiçbir insan kullanıcı bulunmadan çalıştırılması/kimlik doğrulaması yapmak zorunda olan hizmetlerin ve uygulamaların doğrulanması için kullanılır.

Azure Veri Gezgini Erişim Denetimine Genel Bakış:https://docs.microsoft.com/azure/kusto/management/access-control

Azure Etkin Dizini ile kimlik doğrulaması:https://docs.microsoft.com/azure/kusto/management/access-control/aad



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Kullanıcı Erişimini Düzenli Olarak Gözden Geçirin ve Uzlaştırın

**Rehberlik**: Azure Active Directory (Azure AD), eski hesapları keşfetmeye yardımcı olmak için günlükler sağlar. Ayrıca, grup üyeliklerini, kurumsal uygulamalara erişimi ve rol atamalarını verimli bir şekilde yönetmek için Azure Kimlik Erişim İncelemeleri'ni kullanın. Yalnızca doğru Kullanıcıların sürekli erişime sahip olduğundan emin olmak için kullanıcıerişimi düzenli olarak gözden geçirilebilir. 

Azure Veri Gezgini Erişimi için Azure AD ile Kimlik Doğrulaması:https://docs.microsoft.com/azure/kusto/management/access-control/how-to-authenticate-with-aad

Azure REKLAM Raporlama:https://docs.microsoft.com/azure/active-directory/reports-monitoring/

Azure Kimlik Erişim İncelemeleri nasıl kullanılır:https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: Devre Dışı Bırakılan Hesaplara Erişim Denemelerini İzle

**Kılavuz**: Herhangi bir Güvenlik Bilgisi ve Olay Yönetimi (SIEM) / İzleme aracıyla tümleştirmenize olanak tanıyan izleme için Etkinlik, Denetim ve Risk Olayı günlüğünde Oturum Açma'yı kullanabilirsiniz.

 Azure Active Directory kullanıcı hesapları için Tanılama Ayarları oluşturarak, denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics Çalışma alanına göndererek bu işlemi kolaylaştırabilirsiniz. Log Analytics Çalışma Alanı içinde istenilen Uyarıları yapılandırmak için müşteri.

Azure Etkinlik Günlükleri Azure Monitör'e nasıl entegre eleştirilir:https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Hesap Giriş Davranış Sapması Uyarısı

**Kılavuz :** Kullanıcı kimlikleri ile ilgili algılanan şüpheli eylemlere otomatik yanıtları yapılandırmak için Azure Active Directory (Azure AD) Risk Algılamaları ve Kimlik Koruması özelliğini kullanın. Ayrıca, daha fazla araştırma için Azure Sentinel'e veri sindirebilirsiniz.

Azure AD riskli oturum açmaları nasıl görüntülenebilen:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

Kimlik Koruması risk ilkelerinin nasıl yapılandırılabilen ve etkinleştirilir:https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Destek senaryoları sırasında Microsoft'a ilgili müşteri verilerine erişim sağlama

**Kılavuz**: Microsoft'un müşteri verilerine erişmesi gereken destek senaryolarında, Customer Lockbox müşterilerin müşteri veri erişim isteklerini gözden geçirmesi ve onaylaması veya reddetmesi için bir arabirim sağlar.

Müşteri Kilit Kutusunu Anlama:

https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için [güvenlik denetimi: veri koruma.](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection)*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Hassas Bilgilerin Envanterini Koruyun

**Yönerge**: Hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olmak için etiketleri kullanın.

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Hassas bilgileri depolama veya işleme sistemlerini yalıtma

**Rehberlik**: Geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Azure Veri Gezgini kümeleri sanal ağ/alt ağ tarafından diğer kaynaklardan ayrılmalı, uygun şekilde etiketlenmelidir ve bir ağ güvenlik grubu (NSG) veya Azure Güvenlik Duvarı içinde güvenli hale edilmelidir. Veri Gezgini kümeleri, hassas verileri depolamak veya işlemek için yeterince izole edilmelidir.

Ek Azure abonelikleri oluşturma:https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları nasıl oluşturulur:https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Azure Veri Gezgini kümenizi sanal ağa nasıl dağıtabilirsiniz:https://docs.microsoft.com/azure/data-explorer/vnet-deployment

Güvenlik Config ile bir NSG oluşturmak için nasıl:https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic


**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Hassas bilgilerin yetkisiz aktarılmasını izleme ve engelleme

**Rehberlik**: Geçerli değildir; Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data


**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Transit Tüm Hassas Bilgileri Şifreleme

**Yönerge**: Azure Veri Gezgini kümesi TLS 1.2'yi varsayılan olarak görüşür. Azure kaynaklarınıza bağlanan tüm istemcilerin TLS 1,2 veya daha büyük bir anlaşma sağlayabilmesini sağlayın.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Paylaşılan

### <a name="45-divuse-an-active-discovery-tool-to-identify-sensitive-datadiv"></a>4.5: <div>Hassas Verileri Tanımlamak için Etkin Bir Bulma Aracı nı Kullanma</div>

**Kılavuz :** Azure Veri Gezgini için veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz kullanılamıyor. Uyumluluk amacıyla gerekirse üçüncü taraf çözümlerini uygulayın.

Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data


**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="46-divuse-azure-rbac-to-control-access-to-resourcesdiv"></a>4.6: <div>Kaynaklara erişimi denetlemek için Azure RBAC'ı kullanın</div>

**Yönlendirme**: Azure Veri Gezgini, rol tabanlı erişim denetimi (RBAC) modelini kullanarak veritabanlarına ve tablolara erişimi denetlemenizi sağlar. Bu modelde, ilkeler (kullanıcılar, gruplar ve uygulamalar) rollere eşlenir. Müdürler, atandıkları rollere göre kaynaklara erişebilirler.

Azure Veri Gezgini için RBAC'ın nasıl yapılandırılabildiğini anlatan rollerin ve izinlerin ve yönergelerin listesi:https://docs.microsoft.com/azure/data-explorer/manage-database-permissions


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Erişim denetimini sağlamak için ana bilgisayar tabanlı Veri Kaybı Önleme'yi kullanın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

Microsoft, Azure Veri Gezgini'nin altında yatan altyapıyı yönetir ve müşteri verilerinin kaybolmasını veya açığa dökülmesini önlemek için sıkı denetimler uygulamıştır.

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data


**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Hassas Bilgileri Istirahatte Şifreleme

**Yönlendirme**: Azure Disk Şifreleme, kuruluş güvenliği ve uyumluluk taahhütlerinizi yerine getirmek için verilerinizin korunmasına ve korunmasına yardımcı olur. Küme sanal makinelerinizin işletim sistemi ve veri diskleri için birim şifreleme sağlar. Ayrıca, disk şifreleme anahtarlarını ve sırlarını kontrol etmemize ve yönetmemize ve VM diskleri üzerindeki tüm verilerin Azure Depolama'dayken istirahatte şifrelenmemizi sağlayan Azure Key Vault ile entegre olur.

Azure Veri Gezgini kümeleri için şifrelemeyi istirahatte etkinleştirme:https://docs.microsoft.com/azure/data-explorer/manage-cluster-security


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Kritik Azure kaynaklarında yapılan değişiklikleri kaydedin ve uyarı

**Yönerge**: Azure Veri Gezgini kümelerinizde kaynak düzeyinde değişiklikler ne zaman gerçekleşecek uyarıları oluşturmak için Azure Etkinlik Günlüğü ile Azure Monitörünü kullanın.

Azure Etkinlik Günlüğü etkinlikleri için uyarılar oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

Azure Etkinlik Günlüğü etkinlikleri için uyarılar oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için [Güvenlik Denetimi: Güvenlik Açığı Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)bakın.*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Otomatik Güvenlik Açığı Tarama Araçlarını Çalıştırın

**Yönerge**: Azure Veri Gezgini kaynaklarınızı güvence altına alma konusunda Azure Güvenlik Merkezi'nin önerilerine uyun.

Microsoft, Azure Veri Gezgini'ni destekleyen temel sistemlerde güvenlik açığı yönetimi de gerçekleştirir.

Azure Güvenlik Merkezi önerilerini anlayın:https://docs.microsoft.com/azure/security-center/recommendations-reference

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Microsoft

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Otomatik İşletim Sistemi Yama Yönetimi Çözümü Dağıtmak

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3: Otomatik Üçüncü Taraf Yazılım Yama Yönetimi Çözümü Dağıt

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Arka arkaya Güvenlik Açığı Taramalarını Karşılaştırın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="55-utilize-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Keşfedilen güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işleminden yararlanın.

**Yönerge**: Azure Güvenlik Merkezi tarafından sağlanan varsayılan risk derecelendirmelerini (Güvenli Puan) kullanın.
Azure Güvenlik Merkezi Güvenli Puanı Anlayın:https://docs.microsoft.com/azure/security-center/security-center-secure-score


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için [güvenlik denetimi: Envanter ve Varlık Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management)bakın.*

### <a name="61-utilize-azure-asset-discovery"></a>6.1: Azure Varlık Bulma'yı kullanma

**Yönerge**: Aboneliğinizdeki tüm kaynakları (bilgi işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın. Kiracınızda uygun (okundu) izinler sağlayın ve aboneliklerinizdeki tüm Azure aboneliklerinin yanı sıra kaynakları sırala.

Kaynak Grafiği aracılığıyla klasik Azure kaynakları keşfedilebilir, ancak ileriye dönük Olarak Azure Kaynak Yöneticisi kaynaklarının oluşturulması ve kullanılması önerilir.

Azure Kaynak Grafiği ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure Aboneliklerinizi görüntüleme:https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Azure RBAC'ı anlayın:https://docs.microsoft.com/azure/role-based-access-control/overview



**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="62-maintain-asset-metadata"></a>6.2: Varlık Meta Verilerini Koruyun

**Yönerge**: Meta verileri mantıksal olarak taksonomi olarak düzenlemek için meta veriler veren Azure kaynaklarına etiketler uygulayın.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags


**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Yetkisiz Azure Kaynaklarını Silme

**Rehberlik**: Varlıkları düzenlemek ve izlemek için uygun adlandırma kurallarını, etiketlemeyi, yönetim gruplarını veya uygun olduğu durumlarda ayrı abonelikleri kullanabilirsiniz. Envanteri düzenli olarak uzlaştırmak ve yetkisiz kaynakların abonelikten zamanında silinmesini sağlamak için Azure Kaynak Grafiği'ni kullanabilirsiniz. 

Ek Azure abonelikleri oluşturma:https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları nasıl oluşturulur:https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler nasıl oluşturulur ve kullanılır:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Azure Kaynak Grafiği ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal



**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="64-maintain-inventory-of-approved-azure-resources-and-software-titles"></a>6.4: Onaylanan Azure kaynaklarının ve yazılım başlıklarının envanterini koruyun.

**Yönerge**: Kuruluş gereksinimlerinize göre, onaylı Azure kaynaklarının ve onaylı yazılımların bir envanterini oluşturmanız gerekir.  


**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Onaylanmamış Azure Kaynakları için Monitör

**Yönerge**: Azure ilkelerini, aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için kullanabilirsiniz:

    - İzin verilmeyen kaynak türleri

    - İzin verilen kaynak türleri

Politika oluşturulan olayları 

Azure Monitor kullanılarak izlenebilen etkinlik günlükleri.

Ayrıca, abonelik(ler) içindeki kaynakları sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanabilirsiniz.

Öğretici: Uyumluluğu uygulamak için ilkeler oluşturun ve yönetin:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Hızlı başlatma: Azure Kaynak Grafiği Gezgini'ni kullanarak ilk Kaynak Grafiği sorgunuzu çalıştırın:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure Monitör'ü kullanarak etkinlik günlüğü uyarıları oluşturun, görüntüleyin ve yönetin:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log



**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Bilgi İşlem Kaynakları İçinde onaylanmamış Yazılım Uygulamaları için Monitör

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Onaylanmamış Azure Kaynaklarını ve Yazılım Uygulamalarını Kaldırma

**Rehberlik**: Geçerli değildir; bu öneri, bir bütün olarak hesaplama kaynakları ve Azure için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="68-utilize-only-approved-applications"></a>6.8: Yalnızca onaylı uygulamalardan yararlanın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="69-utilize-only-approved-azure-services"></a>6.9: Yalnızca onaylanmış Azure Hizmetlerinden yararlanın

**Yönerge**: Azure ilkelerini, aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için kullanabilirsiniz:

    - İzin verilmeyen kaynak türleri

    - İzin verilen kaynak türleri

Öğretici: Uyumluluğu uygulamak için ilkeler oluşturun ve yönetin:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlkesi Örnekleri:https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types



**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="610-implement-approved-application-list"></a>6.10: Onaylı başvuru listesini uygulama

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azure-resources-manager-via-scripts"></a>6.11: Kullanıcıların komut dosyaları aracılığıyla Azure Kaynakları Yöneticisi ile etkileşim deşmelerini sınırlandırın

**Yönerge**: "Microsoft Azure Yönetimi" Uygulaması için "Erişimi engelle" yapılandırmayı yaparak kullanıcıların Azure Kaynak Yöneticisi ile etkileşim edebilmelerini sınırlamak için Azure Koşullu Erişimi kullanın. Bu, Azure aboneliklerinizdeki kaynakların oluşturulmasını ve kaynakların değiştirilmesini önler. 

Koşullu Erişim ile Azure yönetimine erişimi yönetin:https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Kullanıcıların Bilgi İşlem Kaynakları İçinde Ki Komut Dosyalarını Yürütme Yeteneğini Sınırlayın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Yüksek Riskli Uygulamaları Fiziksel veya Mantıksal Olarak Ayırın

**Rehberlik**: Geçerli değildir; Bu öneri, Azure Uygulama Hizmeti'nde çalışan web uygulamaları veya bilgi işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için güvenlik [denetimi: Güvenli Yapılandırma'ya](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration)bakın.*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Tüm Azure Kaynakları için Güvenli Yapılandırmalar Oluşturma

**Yönerge**: Azure kaynaklarınızın yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak için Azure İlkesi takma adlarını kullanın. Yerleşik Azure İlkesi tanımlarını da kullanabilirsiniz.

Ayrıca, Azure Kaynak Yöneticisi, yapılandırmaların kuruluşunuz için güvenlik gereksinimlerini karşılamasını veya aştığından emin olmak için gözden geçirilmesi gereken JavaScript Nesne Gösterimi'nde (JSON) şablonu dışa aktarma yeteneğine sahiptir.

Azure kaynaklarınız için güvenli bir yapılandırma temeli olarak Azure Güvenlik Merkezi'nden gelen önerileri de kullanabilirsiniz.

Kullanılabilir Azure İlkesi Diğer Adları nasıl görüntüleyebilirsiniz:https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Öğretici: Uyumluluğu uygulamak için ilkeler oluşturun ve yönetin:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure portalındaki şablona tek ve çok kaynak lı dışa aktarma:https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal

Güvenlik önerileri - bir başvuru kılavuzu:https://docs.microsoft.com/azure/security-center/recommendations-reference



**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="72-establish-secure-configurations-for-your-operating-system"></a>7.2: İşletim Sisteminiz için Güvenli Yapılandırmalar Kurun

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="73-maintain-secure-configurations-for-all-azure-resources"></a>7.3: Tüm Azure Kaynakları için Güvenli Yapılandırmaları Koruyun

**Yol gösterici**: Azure kaynaklarınız arasında güvenli ayarlar uygulamak için Azure ilkesini [reddet] ve [varsa dağıt] kullanın.  Çevrenizdeki güvenlik değişikliklerini kolayca tanımlamak için İzlemeyi Değiştir, İlke uyumluluk panosu veya özel bir çözüm gibi çözümleri kullanabilirsiniz.

Azure İlkesi efektlerini anlayın:https://docs.microsoft.com/azure/governance/policy/concepts/effects

Uyumluluğu uygulamak için ilkeler oluşturun ve yönetin:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

İzlemeyi Değiştir çözümüyle ortamınızdaki değişiklikleri izleyin:https://docs.microsoft.com/azure/automation/change-tracking

Azure kaynaklarının uyumluluk verilerini alın:https://docs.microsoft.com/azure/governance/policy/how-to/get-compliance-data



**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="74-maintain-secure-configurations-for-operating-systems"></a>7.4: İşletim Sistemleri için Güvenli Yapılandırmaları Koruyun

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Azure Kaynaklarının Yapılandırmayı Güvenli Bir Şekilde Depolama

**Yönerge**: Özel Azure ilkeleri, Azure Kaynak Yöneticisi şablonları, İstenilen Durum Yapılandırma komut dosyaları vb. gibi kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos'unu kullanın. Azure DevOps'te yönettiğiniz kaynaklara erişmek için, Azure DevOps ile entegre edilmişse Azure Etkin Dizini'nde (Azure AD) tanımlanan belirli kullanıcılara, yerleşik güvenlik gruplarına veya gruplara izin verebilir veya reddedebilir veya TFS ile entegre edildiyseniz Active Directory'yi.

Azure DevOps'lerde kod depolama:https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure DevOps'teki izinler ve gruplar hakkında:https://docs.microsoft.com/azure/devops/organizations/security/about-permissions



**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Özel İşletim Sistemi Görüntülerini Güvenli Bir Şekilde Saklayın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7.7: Sistem Yapılandırma Yönetim Araçlarını Dağıtma

**Yönerge**: Azure İlkesi'ni kullanarak Azure kaynakları için standart güvenlik yapılandırmaları tanımlayın ve uygulayın. Azure kaynaklarınızın ağ yapılandırmasını denetlemek veya uygulamak için özel ilkeler oluşturmak için Azure İlkesi takma adlarını kullanın. Ayrıca, belirli kaynaklarınızla ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz.  Ayrıca, yapılandırma değişikliklerini dağıtmak için Azure Otomasyonu'ni kullanabilirsiniz.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Diğer Adları nasıl kullanılır:https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases



**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: İşletim Sistemleri için Sistem Yapılandırma Yönetim Araçlarını Dağıtma

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Azure Hizmetleri için Otomatik Yapılandırma İzleme uygulaması

**Yönerge**: Sistem yapılandırmalarını uyarmak, denetlemek ve uygulamak için özel ilkeler oluşturmak için Azure İlkesi takma adlarını kullanın. Azure kaynaklarınız için yapılandırmaları otomatik olarak uygulamak için Azure ilkesi [denetim], [reddet], [varsa dağıt] kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: İşletim Sistemleri için Otomatik Yapılandırma İzleme uygulaması

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="711-securely-manage-azure-secrets"></a>7.11: Azure sırlarını güvenli bir şekilde yönetin

**Kılavuz :** Azure Disk Şifreleme, Azure Veri Gezgini küme sanal makinelerinizin işletim sistemi ve veri diskleri için birim şifreleme sağlar. Ayrıca, disk şifreleme anahtarlarını ve sırlarını kontrol etmenizi ve yönetmeniz ve VM diskleri üzerindeki tüm verilerin Azure Depolama'dayken dinlenirken şifrelenmenizi sağlayan Azure Key Vault ile entegre olur.

Azure Veri Gezgini'nde kümenizi nasıl güvene alabilen:https://docs.microsoft.com/azure/data-explorer/manage-cluster-security


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Microsoft

### <a name="712-securely-and-automatically-manage-identities"></a>7.12: Kimlikleri güvenli ve otomatik olarak yönetin

**Yol gösterici**: Azure hizmetlerine Azure AD'de otomatik olarak yönetilen bir kimlik sağlamak için Yönetilen Kimlikler'i kullanın. Yönetilen Kimlikler, anahtarınızda herhangi bir kimlik belgesi olmadan Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlere kimlik doğrulamanızı sağlar. Yönetilen Kimlikler nasıl yapılandırılır:https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

Azure Veri Gezgini kümeniz için yönetilen kimlikleri yapılandırın:https://docs.microsoft.com/azure/data-explorer/managed-identities


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: İstenmeden kimlik bilgisi maruziyetini ortadan kaldırın

**Yönerge**: Kod içindeki kimlik bilgilerini tanımlamak için Kimlik Bilgisi Tarayıcısı'nı uygulayın. Kimlik Bilgisi Tarayıcısı, keşfedilen kimlik bilgilerinin Azure Anahtar Kasası gibi daha güvenli konumlara taşınmasını da teşvik edecektir. 

Kimlik Bilgisi Tarayıcı nasıl kurulturur:https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için [Güvenlik Kontrolü: Malware Defense](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense)' e bakın.*

### <a name="81-utilize-centrally-managed-anti-malware-software"></a>8.1: Merkezi Yönetilen Kötü Amaçlı Yazılımdan Koruma Yazılımından Yararlanın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda etkindir (örneğin, Azure Veri Gezgini), ancak müşteri içeriğinde çalışmaz.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: İşlem yapmayan Azure kaynaklarına yüklenecek dosyaları önceden taraya

**Yönlendirme**: Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure Veri Gezgini) Microsoft kötü amaçlı yazılımdan koruma etkindir, ancak müşteri içeriğinde çalışmaz.

Azure Veri Gezgini, Veri Gölü Depolama, Blob Depolama, PostgreSQL için Azure Veritabanı gibi bilgi işlem dışı Azure kaynaklarına yüklenen tüm içerikleri önceden tarayın. Microsoft bu gibi durumlarda verilerinize erişemez.



**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: Kötü Amaçlı Yazılımdan Koruma Yazılımı ve İmzalarının Güncelleştirilmesini Sağlayın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda etkindir, ancak müşteri içeriğinde çalışmaz.

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Düzenli Otomatik Yedekleme'leri Sağlayın

**Kılavuz**: Veri Gezgini kümeniz tarafından kullanılan Microsoft Azure depolama hesabınızdaki veriler, dayanıklılık ve yüksek kullanılabilirlik sağlamak için her zaman çoğaltılır. Azure Depolama, verilerinizi geçici donanım arızaları, ağ veya elektrik kesintileri ve büyük doğal afetler gibi planlı ve planlanmamış olaylara karşı korunacak şekilde kopyalar. Verilerinizi aynı veri merkezi içinde, aynı bölgedeki bölge veri merkezlerinde veya coğrafi olarak ayrılmış bölgelerde çoğaltmayı seçebilirsiniz.

Azure Depolama artıklığını ve Hizmet Düzeyi Sözleşmelerini Anlama:https://docs.microsoft.com/azure/storage/common/storage-redundancy

Verileri depolamaya dışa aktarma:https://docs.microsoft.com/azure/kusto/management/data-export/export-data-to-storage



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Komple Sistem Yedeklemeleri Gerçekleştirin ve Müşteri Yönetilen Anahtarları Yedekleme

**Kılavuz :** Azure Veri Gezgini, bir depolama hesabındaki tüm verileri istirahat halinde şifreler. Varsayılan olarak, veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için, veri şifreleme için kullanmak üzere müşteri tarafından yönetilen anahtarları sağlayabilirsiniz. Müşteri tarafından yönetilen anahtarlar Azure Anahtar Kasası'nda depolanmalıdır. 

C# kullanarak müşteri tarafından yönetilen anahtarları yapılandırın:https://docs.microsoft.com/azure/data-explorer/customer-managed-keys-csharp

Azure Kaynak Yöneticisi şablonu kullanarak müşteri tarafından yönetilen anahtarları yapılandırın:https://docs.microsoft.com/azure/data-explorer/customer-managed-keys-resource-manager

Azure Anahtar Kasası sertifikaları nasıl yedekilir:https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultcertificate?view=azurermps-6.13.0


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Müşteri Yönetilen Anahtarlar dahil tüm Yedeklemeleri doğrulayın

Kılavuz : Azure Key Vault sırlarınızın veri geri yüklemesini periyodik olarak test **edin.**

Azure Anahtar Kasası sertifikalarını geri yükleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultcertificate?view=azurermps-6.13.0 

C# kullanarak müşteri tarafından yönetilen anahtarları yapılandırın:https://docs.microsoft.com/azure/data-explorer/customer-managed-keys-csharp

Azure Kaynak Yöneticisi şablonu kullanarak müşteri tarafından yönetilen anahtarları yapılandırın:https://docs.microsoft.com/azure/data-explorer/customer-managed-keys-resource-manager



**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Yedeklerin ve Müşteri Yönetilen Anahtarların Korunmasını Sağlayın

**Kılavuz**: Anahtar Kasası'nda Soft-Delete'i etkinleştirerek anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korur.  Ayrıca, silinen durumda bir kasa veya bir nesne varsa, bekletme süresi geçene kadar tasfiye edilemez, böylece temizleme koruması etkinleştirebilirsiniz.  

Key Vault'ta Soft-Delete ve Temizleme koruması nasıl etkinleştirilir:https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete

C# kullanarak müşteri tarafından yönetilen anahtarları yapılandırın:https://docs.microsoft.com/azure/data-explorer/customer-managed-keys-csharp

Azure Kaynak Yöneticisi şablonu kullanarak müşteri tarafından yönetilen anahtarları yapılandırın:https://docs.microsoft.com/azure/data-explorer/customer-managed-keys-resource-manager



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için [Güvenlik Denetimi: Olay Yanıtı'na](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response)bakın.*

### <a name="101-create-incident-response-guide"></a>10.1: Olay yanıt kılavuzu oluşturma

**Rehberlik**: Kuruluşunuz için bir olay yanıt kılavuzu oluşturun. Personelin tüm rollerini ve olay işleme/yönetiminin algılamadan olay sonrası incelemeye kadar olan aşamalarını tanımlayan yazılı olay yanıt planları olduğundan emin olun.
    

    Guidance on building your own security incident response process: https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

    

    Microsoft Security Response Center's Anatomy of an Incident: https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/

    

    Customer may also leverage NIST's Computer Security Incident Handling Guide to aid in the creation of their own incident response plan: https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final

    



**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="102-create-incident-scoring-and-prioritization-procedure"></a>10.2: Olay Puanlama ve Öncelik Lendirme Prosedürü Oluşturma

**Rehberlik**: Güvenlik Merkezi, öncelikle hangi uyarıların araştırılması gerektiğine öncelik vermenize yardımcı olmak için her uyarıya önem verir. Önem derecesi, Güvenlik Merkezi'nin bulguda ne kadar emin olduğuna veya uyarıyı vermek için kullanılan analize ve uyarıya yol açan etkinliğin arkasında kötü amaçlı niyet olduğuna dair güven düzeyine dayanır. 
    

    Additionally, clearly mark subscriptions (for ex. production, non-prod) using tags and create a naming system to clearly identify and categorize Azure resources, especially those processing sensitive data.  It is your responsibility to prioritize the remediation of alerts based on the criticality of the Azure resources and environment where the incident occurred.

    

    Security alerts in Azure Security Center: https://docs.microsoft.com/azure/security-center/security-center-alerts-overview

    

    Use tags to organize your Azure resources: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="103-test-security-response-procedures"></a>10.3: Test Güvenliği Yanıt Prosedürleri

**Kılavuz :** Azure kaynaklarınızın korunmasına yardımcı olmak için sistemlerinizin olay yanıt yeteneklerini düzenli olarak test etmek için alıştırmalar yapın. Zayıf noktaları ve boşlukları belirleyin ve planı gerektiği gibi gözden geçirin.
    

    Refer to NIST's publication: Guide to Test, Training, and Exercise Programs for IT Plans and Capabilities: https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf



**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-nbspfor-security-incidents"></a>10.4: Güvenlik Olayı İletişim Bilgilerini Sağlayın &nbsp;ve Güvenlik Olayları Için Uyarı Bildirimlerini Yapılandırın

**Kılavuz :** Microsoft Güvenlik Yanıt Merkezi (MSRC) verilerinize yasadışı veya yetkisiz bir taraf tarafından erişildiğini tespit ederse, güvenlik olayı iletişim bilgileri Microsoft tarafından sizinle iletişim kurmak için kullanılır. Sorunların çözülmesini sağlamak için olaydan sonraki olayları gözden geçirin.
    

Azure Güvenlik Merkezi Güvenlik Kişisi nasıl ayarlanır:https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Olay yanıt sisteminize güvenlik uyarıları dahil edin

**Kılavuz**: Azure kaynaklarına yönelik riskleri belirlemeye yardımcı olmak için Sürekli DışAkverme özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışa aktarın. Sürekli Dışa Aktarma, uyarı ve önerileri el ile veya sürekli olarak dışa aktarmanıza olanak tanır. Uyarıları Azure Sentinel'e aktarmak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz.
    

    How to configure continuous export: https://docs.microsoft.com/azure/security-center/continuous-export

    

    How to stream alerts into Azure Sentinel: https://docs.microsoft.com/azure/sentinel/connect-azure-security-center



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Güvenlik uyarılarına yanıtı otomatikleştirin

**Yönerge**: Azure kaynaklarınızı korumak için güvenlik uyarıları ve önerilerinde "Mantık Uygulamaları" aracılığıyla yanıtları otomatik olarak tetiklemek için Azure Güvenlik Merkezi'ndeki İş Akışı Otomasyonu özelliğini kullanın.
    

    How to configure Workflow Automation and Logic Apps: https://docs.microsoft.com/azure/security-center/workflow-automation



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için [Güvenlik Kontrolü: Penetrasyon Testleri ve Kırmızı Takım Egzersizleri'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises)bakın.*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-to-remediate-all-critical-security-findings-within-60-days"></a>11.1: Azure kaynaklarınızın düzenli Penetrasyon Testini gerçekleştirin ve tüm kritik güvenlik bulgularını 60 gün içinde düzeltin

**Kılavuz**: Penetrasyon Testlerinizin Microsoft ilkelerini ihlal etmediğini sağlamak https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1için lütfen Microsoft Etkileşim Kuralları'nı izleyin: .

Microsoft'un stratejisi ve Red Teaming ve Microsoft yönetilen bulut altyapısı, hizmetleri ve uygulamalarına karşı canlı site penetrasyon testi nin uygulanması hakkında daha fazla bilgiyi burada bulabilirsiniz:https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Güvenlik Kıyaslama'ya](https://docs.microsoft.com/azure/security/benchmarks/overview) bakın
- [Azure Güvenlik Taban Çizgileri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
