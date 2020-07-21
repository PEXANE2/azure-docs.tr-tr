---
title: PostgreSQL için Azure veritabanı-Azure Güvenlik kıyaslaması için hiper ölçek güvenlik temeli
description: PostgreSQL için Azure veritabanı-Hyperscale güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: postgresql
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: ca08ac30c5d8e925197a0df1d327bf9a0923b046
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86532075"
---
# <a name="azure-database-for-postgresql---hyperscale-security-baseline-for-azure-security-benchmark"></a>PostgreSQL için Azure veritabanı-Azure Güvenlik kıyaslaması için hiper ölçek güvenlik temeli

PostgreSQL için Azure veritabanı için Azure Güvenlik temeli-hiper ölçek, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: sanal ağlar içindeki Azure kaynaklarını koruma

**Rehberlik**: PostgreSQL Için Azure veritabanı sunucu güvenlik duvarı, hangi bilgisayarların izne sahip olduğunu belirtene kadar hyperscale (Citus) düzenleyici düğümüne tüm erişimi engeller. Güvenlik Duvarı, her isteğin kaynak IP adresini temel alarak sunucuya erişim izni verir. Güvenlik duvarınızı yapılandırmak için kabul edilebilir IP adreslerinin aralıklarını belirten güvenlik duvarı kuralları oluşturun. Sunucu düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz.

* [PostgreSQL için Azure veritabanı 'nda güvenlik duvarı kuralları yapılandırma-hiper ölçek (Citus)](https://docs.microsoft.com/azure/postgresql/concepts-hyperscale-firewall-rules)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1,2: sanal ağların, alt ağların ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Rehberlik**: uygulanamaz; PostgreSQL için Azure veritabanı-hiper ölçek (Citus) bir sanal ağ, alt ağ veya ağ güvenlik grubuyla ilişkilendirilemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: uygulanamaz; PostgreSQL için Azure veritabanı-Hyperscale (Citus) tarafından kullanılan uç noktalar Microsoft tarafından yönetilir. Şirket içi sistemlerinize dağıtmak istediğiniz ek denetimlerden siz sorumlusunuz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="15-record-network-packets"></a>1,5: ağ paketlerini kaydetme

**Rehberlik**: uygulanamaz; PostgreSQL için Azure veritabanı-hiper ölçek (Citus) bir sanal ağ, alt ağ veya ağ güvenlik grubuyla ilişkilendirilemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Rehberlik**: uygulanamaz; PostgreSQL için Azure veritabanı-Hyperscale (Citus) tarafından kullanılan uç noktalar Microsoft tarafından yönetilir. Şirket içi sistemlerinize dağıtmak istediğiniz ek denetimlerden siz sorumlusunuz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: uygulanamaz; PostgreSQL için Azure veritabanı-hiper ölçek (Citus) bir sanal ağ, alt ağ veya ağ güvenlik grubuyla ilişkilendirilemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Rehberlik**: Azure Ilkesiyle PostgreSQL Için Azure veritabanı örnekleri ile ilişkili ağ ayarları ve ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. PostgreSQL için Azure veritabanı örneklerine yönelik ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Network" ad alanındaki Azure Ilke diğer adlarını kullanın.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Ağ için Azure Ilke örnekleri](https://docs.microsoft.com/azure/governance/policy/samples/#network)

* [Azure Blueprint oluşturma](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: uygulanamaz; PostgreSQL için Azure veritabanı-hiper ölçek (Citus) bir sanal ağ, alt ağ veya ağ güvenlik grubuyla ilişkilendirilemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Rehberlik**: uygulanamaz; PostgreSQL için Azure veritabanı-hiper ölçek (Citus) bir sanal ağ, alt ağ veya ağ güvenlik grubuyla ilişkilendirilemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, günlüklerdeki zaman damgaları Için PostgreSQL Için Azure veritabanı-hiper ölçek (Citus) gibi Azure kaynakları için kullanılan zaman kaynağını korur.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: denetim düzlemi denetim günlüğü Için Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. Azure etkinlik günlüğü verilerini kullanarak, Azure kaynaklarınızın denetim düzlemi düzeyinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, SILME) için "ne, kim ve ne zaman" seçeneğini belirleyebilirsiniz.

Ayrıca, Hiperscale tarafından oluşturulan güvenlik verilerini toplamak için Azure Izleyici aracılığıyla günlük alma (Citus). Azure Izleyici 'de, Log Analytics çalışma alanı (ler) kullanarak Analizi sorgulayın ve gerçekleştirin ve uzun süreli/arşiv depolama için depolama hesapları kullanın. Alternatif olarak, Azure Sentinel veya bir üçüncü taraf güvenlik olayı ve olay yönetimi (SıEM) üzerinde verileri etkinleştirebilir ve bu verilere izin verebilir.

* [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Hiper ölçekte ölçümler (Citus)](https://docs.microsoft.com/azure/postgresql/concepts-hyperscale-monitoring)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: hiper ölçek (Citus), bir sunucu grubundaki her bir düğüm için ölçümler sağlar. Ölçümler, destekleyici kaynakların davranışına ilişkin öngörüler sunar. Her ölçüm bir dakikalık sıklıkta yayılır ve 30 güne kadar geçmiş olur.

Denetim düzlemi denetim günlüğü için, Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. Azure etkinlik günlüğü verilerini kullanarak, Azure kaynaklarınızın denetim düzlemi düzeyinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, SILME) için "ne, kim ve ne zaman" seçeneğini belirleyebilirsiniz.

Ayrıca, Hiperscale tarafından oluşturulan güvenlik verilerini toplamak için Azure Izleyici aracılığıyla günlük alma (Citus). Azure Izleyici 'de, Log Analytics çalışma alanı (ler) kullanarak Analizi sorgulayın ve gerçekleştirin ve uzun süreli/arşiv depolama için depolama hesapları kullanın. Alternatif olarak, Azure Sentinel veya bir üçüncü taraf güvenlik olayı ve olay yönetimi (SıEM) üzerinde verileri etkinleştirebilir ve bu verilere izin verebilir.

* [Hiper ölçekte ölçümler (Citus)](https://docs.microsoft.com/azure/postgresql/concepts-hyperscale-monitoring)

* [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de, hiper ölçek (Citus) günlüklerinizi tutmak için kullanılan Log Analytics çalışma alanı için, saklama süresini kuruluşunuzun uyumluluk düzenlemelerine göre ayarlayın. Uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

* [Log Analytics çalışma alanları için günlük saklama parametrelerini ayarlama](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

* [Kaynak günlüklerini bir Azure depolama hesabında depolama](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-collect-storage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: anormal davranışlar Için hiperscale (Citus) örneklerinizin günlüklerini çözümleyin ve izleyin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Izleyici Log Analytics kullanın. Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [Log Analytics hakkında daha fazla bilgi için](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)

* [Azure Izleyici 'de özel sorgular gerçekleştirme](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: anormal etkinlikler için uyarıları etkinleştir

**Rehberlik**: hiper ölçek (Citus) için tanılama ayarlarını etkinleştirebilir ve bir Log Analytics çalışma alanına Günlükler gönderebilirsiniz. Azure hizmetleriniz için izleme ölçümlerine göre bir uyarı yapılandırabilir ve alabilirsiniz. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Izleyici Log Analytics kullanın. Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.

Log Analytics çalışma alanınızı Azure Sentinel 'e ekleyin. Bu, bir güvenlik Orchestration otomatik yanıtı (SOAR) çözümü sağlar. Bu, güvenlik sorunlarını gidermek için PlayBook 'ları (otomatikleştirilmiş çözümlerin) oluşturulmasına ve kullanılmasına olanak tanır.

* [Hiper ölçekte ölçümler (Citus)](https://docs.microsoft.com/azure/postgresql/howto-hyperscale-alert-on-metric)

* [Azure etkinlik günlüğü için tanılama ayarlarını yapılandırma](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: uygulanamaz; PostgreSQL için Azure veritabanı-Hyperscale (Citus) kötü amaçlı yazılımdan koruma ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; PostgreSQL için Azure veritabanı, DNS ile ilgili günlükleri işlemez veya oluşturmuyor.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve erişim denetimi](/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: hiper ölçek (Citus) örneklerinizin denetim düzlemi (ör. Azure Portal) üzerinde yönetici erişimi olan kullanıcı hesaplarının envanterini saklayın. Ayrıca, hiper ölçek (Citus) örneklerinizin veri düzlemine erişimi olan yönetim hesaplarının envanterini saklayın (veritabanının kendisi içinde).

Hiper ölçek (Citus), yerleşik rol tabanlı erişim denetimini desteklemez, ancak belirli kaynak sağlayıcısı işlemlerine göre özel roller oluşturabilirsiniz.

Ayrıca, PostgreSQL altyapısı, veritabanı nesnelerine erişimi denetlemek için roller kullanır ve yeni oluşturulan bir hiper ölçek (Citus) sunucu grubu önceden tanımlanmış birçok rol ile gelir. Kullanıcı ayrıcalıklarını değiştirmek için, PgAdmin veya psql gibi bir araç kullanarak standart PostgreSQL komutlarını kullanın.

* [Azure aboneliği için özel rolleri anlama](https://docs.microsoft.com/azure/role-based-access-control/custom-roles)

* [PostgreSQL için Azure veritabanı kaynak sağlayıcısı işlemlerini anlama](https://docs.microsoft.com/azure/role-based-access-control/resource-provider-operations#microsoftdbforpostgresql)

* [PostgreSQL için Azure veritabanı erişim yönetimini anlama](https://docs.microsoft.com/azure/postgresql/concepts-security#access-management])

* [PostgreSQL için Azure veritabanı 'nda Kullanıcı oluşturma-hiper ölçek (Citus)](https://docs.microsoft.com/azure/postgresql/howto-hyperscale-create-users)

* [Psql kullanarak PostgreSQL-Hyperscale 'e (Citus) bağlanma](https://docs.microsoft.com/azure/postgresql/quickstart-create-hyperscale-portal#connect-to-the-database-using-psql)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Kılavuz**: Azure AD varsayılan parola kavramına sahip değildir. Parola gerektiren diğer Azure kaynakları, karmaşıklık gereksinimleriyle ve hizmete bağlı olarak, en düşük parola uzunluğuna sahip bir parola oluşturulmasını zorlar. Varsayılan parolaları kullanbilen üçüncü taraf uygulamalardan ve Market hizmetlerinden siz sorumlusunuz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: hyperscale (Citus) örneklerine erişmek için kullanılan adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Azure kaynağını yönetmeye yönelik yönetici hesapları Azure Active Directory bağlıdır, Ayrıca, veritabanı erişim izinlerinin yönetilmesi için Hiperscale (Citus) sunucu grubunda bulunan yerel sunucu yöneticisi hesapları da vardır. Azure Active Directory içindeki yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

* [Azure Güvenlik Merkezi kimlik ve erişimini anlama](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

* [PostgreSQL için Azure veritabanı 'nda Kullanıcı oluşturma-hiper ölçek (Citus)](https://docs.microsoft.com/azure/postgresql/howto-hyperscale-create-users)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: PostgreSQL Için Azure veritabanı 'nda oturum açma-hiper ölçek (Citus), hem veritabanında doğrudan yapılandırılan Kullanıcı adı/parola hem de bir Azure ACTIVE DIRECTORY (ad) kimliği kullanarak ve bağlanmak Için BIR Azure AD belirteci kullanılarak desteklenir. Bir Azure AD belirteci kullanırken, bir Azure AD kullanıcısı, bir Azure AD grubu veya veritabanına bağlanan bir Azure AD uygulaması gibi farklı yöntemler desteklenir.

* [PostgreSQL için Azure veritabanı 'nda kimlik doğrulaması için Azure Active Directory kullanın](https://docs.microsoft.com/azure/postgresql/howto-configure-sign-in-aad-authentication)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure Active Directory MULTI-Factor AUTHENTICATION (MFA) etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin. Veritabanınızda oturum açmak için Azure AD belirteçlerini kullanırken, veritabanı oturum açma işlemleri için çok faktörlü kimlik doğrulaması gerektirmesini sağlar.

* [Azure 'da MFA 'yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

* [PostgreSQL için Azure veritabanı 'nda kimlik doğrulaması için Azure Active Directory kullanın](https://docs.microsoft.com/azure/postgresql/howto-configure-sign-in-aad-authentication)

* [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure kaynaklarını açmak ve yapılandırmak için yapılandırılmış MULTI-Factor AUTHENTICATION (MFA) Ile ayrıcalıklı erişim iş Istasyonları (Paw) kullanın.

* [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [Azure 'da MFA 'yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-alert-on-account-login-behavior-deviation"></a>3,7: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik oluştuğunda günlükler ve uyarılar oluşturmak için Azure ACTIVE DIRECTORY (AD) PRIVILEGED IDENTITY Management (PIM) kullanın.

Riskli Kullanıcı davranışında uyarıları ve raporları görüntülemek için Azure AD risk algılamalarını kullanın.

* [Privileged Identity Management dağıtma (PıM)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan)

* [Azure AD risk algılamalarını anlama](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: portala izin vermek ve IP adresi aralıklarının ya da ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişim Azure Resource Manager Için, koşullu erişim adlı konum kullanın.

* [Azure 'da adlandırılmış konumları yapılandırma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (ad) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

Azure AD kimlik bilgileri ayrıca, PostgreSQL yönetici hesaplarını denetlemek için yönetim düzlemi düzeyinde (ör. Azure portal) yönetim için de kullanılabilir.

* [PostgreSQL için Azure veritabanı 'nda kimlik doğrulaması için Azure Active Directory kullanın](https://docs.microsoft.com/azure/postgresql/howto-configure-sign-in-aad-authentication)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: hem yerel veritabanına hem de Azure Active Directory erişim sahibi olan kullanıcılar için erişimi gözden geçirin ve karşılaştırın.

Hiper ölçek (Citus) yönetim rolleriyle bunları içerebilen eski hesapların keşfedilmesine yardımcı olması için Azure Active Directory (AD) günlüklerini gözden geçirin. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek için Azure kimlik erişimi Incelemelerini kullanın, hiper ölçekte (Citus) ve rol atamalarına erişmek için kullanılabilecek kurumsal uygulamalara erişin. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişiminin her 90 gün gibi düzenli aralıklarla gözden geçirilmesi gerekir.

* [PostgreSQL kullanıcılarını ve atanan rolleri gözden geçirin](https://www.postgresql.org/docs/current/database-roles.html)

* [Azure AD raporlamayı anlama](https://docs.microsoft.com/azure/active-directory/reports-monitoring/)

* [Azure kimlik erişimi Incelemelerini kullanma](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: devre dışı bırakılmış kimlik bilgilerine erişme girişimlerini izleme

**Rehberlik**: Azure ACTIVE DIRECTORY (ad) IÇINDE Azure AD oturum açma etkinliğine, denetim ve risk olay günlüğü kaynaklarına erişerek herhangi bir SIEM/izleme aracıyla tümleştirmenize imkan tanır.

Azure Active Directory Kullanıcı hesapları için Tanılama ayarları oluşturarak ve denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics çalışma alanına göndererek bu işlemi kolaylaştırabilirsiniz. İstenen uyarıları Log Analytics çalışma alanı içinde yapılandırabilirsiniz.

* [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapması üzerinde uyarı

**Rehberlik**: Azure ACTIVE DIRECTORY (ad) düzeyinde otomatik yanıtları algılanan şüpheli eylemlere yapılandırmak Için Azure Active Directory kimlik koruması ve risk algılama özelliklerini kullanın. Kuruluşunuzun güvenlik yanıtlarını uygulamak için Azure Sentinel aracılığıyla otomatikleştirilmiş yanıtları etkinleştirebilirsiniz.

Ayrıca, daha fazla araştırma için günlükleri Azure Sentinel 'e aktarabilirsiniz.

* [Azure AD Kimlik Koruması genel bakış](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection)

* [Azure AD riskli oturum açma işlemlerini görüntüleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: Şu anda kullanılamıyor; Müşteri Kasası henüz hiper ölçek için desteklenmiyor (Citus).

* [Desteklenen Müşteri Kasası hizmetleri listesi](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Şu anda kullanılamıyor

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: gizli bilgileri depolayan veya Işleyen hiper ölçek (Citus) örneklerini veya ilgili kaynakları izlemeye yardımcı olması için etiketleri kullanın.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. PostgreSQL örnekleri için Azure veritabanınıza ağ erişimini yalıtmak ve sınırlamak için yönetim rolleri ve Güvenlik Duvarı kurallarının bir birleşimini kullanın.

* [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

* [PostgreSQL için Azure veritabanı 'nda güvenlik duvarı kurallarını anlama-hiper ölçek (Citus)](https://docs.microsoft.com/azure/postgresql/concepts-hyperscale-firewall-rules)

* [Hiper ölçekte rolleri anlama (Citus)](https://docs.microsoft.com/azure/postgresql/howto-hyperscale-create-users)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: Microsoft, PostgreSQL Için Azure veritabanı-hiper ölçek (Citus) için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: hiper ölçek (Citus) düzenleyici düğümüne istemci uygulama bağlantıları, aktarım katmanı GÜVENLIĞI (TLS) 1,2 gerektirir. Veritabanı sunucunuz ile istemci uygulamalarınız arasında TLS bağlantılarının uygulanması, sunucu ile uygulamanız arasındaki veri akışını şifreleyerek "ortadaki adam" saldırılarına karşı korunmaya yardımcı olur.

Azure portal aracılığıyla sağlanan PostgreSQL için Azure veritabanı sunucuları için, TLS bağlantılarının uygulanması varsayılan olarak etkindir.

Bazı durumlarda, üçüncü taraf uygulamalar güvenli bir şekilde bağlanmak için güvenilir bir sertifika yetkilisi (CA) sertifika dosyasından (. cer) oluşturulan yerel bir sertifika dosyası gerektirir.

* [PostgreSQL için Azure veritabanı 'nda TLS 'yi yapılandırma-hiper ölçek (Citus)](https://docs.microsoft.com/azure/postgresql/concepts-hyperscale-ssl-connection-security)

* [TLS bağlantısı için sertifika doğrulaması gerektiren uygulamalar](https://docs.microsoft.com/azure/postgresql/concepts-hyperscale-ssl-connection-security)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz hiper ölçekte kullanılamıyor (Citus).

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için rol tabanlı erişim denetimi kullanma

**Rehberlik**: hiper ölçek (Citus) denetim düzlemine erişimi denetlemek için Azure rol tabanlı erişim denetimi 'NI (RBAC) kullanın (ör. Azure Portal). RBAC, veritabanı içindeki kullanıcı izinlerini etkilemez.

Veritabanı düzeyinde kullanıcı ayrıcalıklarını değiştirmek için, PgAdmin veya psql gibi bir araç kullanarak standart PostgreSQL komutlarını kullanın.

* [Azure 'da RBAC 'yi yapılandırma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

* [PostgreSQL için Azure veritabanı için SQL ile Kullanıcı erişimini yapılandırma](https://docs.microsoft.com/azure/postgresql/howto-hyperscale-create-users)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft, hiper ölçek (Citus) için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: günde en az bir kez, PostgreSQL Için Azure veritabanı hiper ölçek (Citus), veri dosyalarının anlık görüntü yedeklemelerini ve veritabanı işlem günlüğünü alır. Yedeklemeler, bir sunucuyu Bekletme dönemi içinde herhangi bir zaman noktasına geri yüklemenize olanak tanır. (Bekletme dönemi Şu anda tüm kümeler için 35 gündür.) Tüm yedeklemeler AES 256 bit şifreleme kullanılarak şifrelenir. PostgreSQL hiper ölçek (Citus) teklifi, şifreleme için Microsoft tarafından yönetilen anahtarları kullanır.

* [Azure PostgreSQL-Hyperscale (Citus) yedeklemeleri için şifrelemeyi anlama](https://docs.microsoft.com/azure/postgresql/concepts-hyperscale-backup)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure Izleyici 'Yi Azure etkinlik günlüğü ile birlikte kullanarak, hiper ölçek (Citus) ve diğer kritik veya ilgili kaynakların üretim örneklerine yapılan değişikliklerin ne zaman gerçekleştiği hakkında uyarılar oluşturun.

* [Azure etkinlik günlüğü olayları için uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: Şu anda kullanılamıyor; Azure Güvenlik Merkezi, PostgreSQL için Azure veritabanı-hiper ölçek (Citus) için güvenlik açığı değerlendirmesini henüz desteklememektedir.

* [Azure Güvenlik Merkezi 'nde Azure PaaS hizmetleri için özellik kapsamı](https://docs.microsoft.com/azure/security-center/features-paas)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Şu anda kullanılamıyor

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Kılavuz**: Microsoft tarafından yönetilen temel platform Için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve pozlamaya karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5,3: üçüncü taraf yazılım başlıkları için otomatik düzeltme eki yönetimi çözümünü dağıtma

**Kılavuz**: Microsoft tarafından yönetilen temel platform Için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve pozlamaya karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Kılavuz**: Microsoft tarafından yönetilen temel platform Için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve pozlamaya karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: Microsoft, PostgreSQL Için Azure veritabanı 'Nı (Citus) destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir. Veritabanı kümenizin ve ilgili kaynaklarınızın yapılandırmasıyla ilgili güvenlik açıkları için, ortak bir risk Puanlama programını (örneğin, ortak güvenlik açığı Puanlama sistemi) veya üçüncü taraf tarama aracınız tarafından sunulan varsayılan risk derecelendirmelerini kullanın.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: otomatik varlık bulma çözümünü kullanma

**Rehberlik**: abonelikleriniz içindeki tüm kaynakları (Hiperscale (Citus) örnekleri dahil) sorgulamak ve öğrenmek Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinleriniz olduğundan ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerinin listesini belirleyebildiğinizden emin olun.

* [Azure Graph ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

* [Azure aboneliklerinizi görüntüleme](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [Azure RBAC 'yi anlama](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Rehberlik**: hiper ölçek (Citus) örneklerine ve diğer ilgili kaynaklara Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler sağlar.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: hiper ölçek (Citus) örneklerini ve ilgili kaynakları düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

* [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynaklarının envanterini tanımlayın ve saklayın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkesini kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

Ayrıca, Azure Kaynak grafiğini kullanarak abonelikler içindeki kaynakları sorgular/bulur.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Graph ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkesini kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

Ayrıca, Azure Kaynak grafiğini kullanarak abonelikler içindeki kaynakları sorgular/bulur.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Graph ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: uygulanamaz; Bu öneri, bir bütün olarak işlem kaynaklarına ve Azure 'a yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkesini kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Ilkesiyle belirli bir kaynak türünü reddetme](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6,10: onaylanan yazılım başlıkları envanterini koruyun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak Için Azure koşullu erişimini kullanın. Bu, hassas bilgiler içeren hiper ölçek (Citus) örnekleri gibi yüksek bir güvenlik ortamındaki kaynaklarda oluşturma ve değişiklik yapılmasını önleyebilir.

* [Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Kılavuz**: Azure Ilkesiyle hiperscale (Citus) örnekleri için standart güvenlik yapılandırması tanımlayın ve uygulayın. PostgreSQL için Azure veritabanı örneklerine yönelik ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için Azure Ilkesi 'ni kullanın.

Ayrıca, Azure Resource Manager, yapılandırmanın kuruluşunuzun güvenlik gereksinimlerini karşıladığından/aştığından emin olmak için gözden geçirilmesi gereken JavaScript Nesne Gösterimi (JSON) içinde şablonu dışarı aktarma özelliğine sahiptir.

* [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure portal bir şablona tek ve çoklu kaynak verme](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın. Ayrıca, kuruluşunuz tarafından gerekli olan Azure kaynaklarınızın güvenlik yapılandırmasını sürdürmek için Azure Resource Manager şablonları kullanabilirsiniz.

* [Azure Ilke efektlerini anlama](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

* [Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Resource Manager şablonlarına genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Rehberlik**: hyperscale (Citus) örneklerinizin ve ilgili kaynaklarınızın özel Azure ilke tanımlarını kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos kullanın.

* [Azure DevOps 'da kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Azure Repos belgeleri](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Azure kaynakları için yapılandırma yönetimi araçları dağıtma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın. Ayrıca, kuruluşunuz tarafından gerekli olan Azure kaynaklarınızın güvenlik yapılandırmasını sürdürmek için Azure Resource Manager şablonları kullanabilirsiniz.

* [Azure Ilke efektlerini anlama](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

* [Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Resource Manager şablonlarına genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Azure kaynakları için otomatik yapılandırma izlemeyi uygulama

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak için özel ilkeler oluşturmak üzere "Microsoft. DBforPostgreSQL" ad alanındaki Azure ilke diğer adlarını kullanın. PostgreSQL için Azure veritabanı örneklerine ve ilgili kaynaklara yönelik konfigürasyonları otomatik olarak zorlamak için [Denetim], [reddetme] ve [dağıtım yok] Azure ilkesini kullanın.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: uygulanamaz; Azure Key Vault kullanarak veritabanı yönetici parolanızı güvenle yönetebilirsiniz. Veritabanı parolanma erişimi kısıtlamak Key Vault gizli kapsamlı aşağı izinleri atayın. Kod depolarınızda herhangi bir bağlantı dizesini veya veritabanı gizli dizilerini saklamaktan kaçının.

* [PostgreSQL veritabanınıza bağlanma](https://docs.microsoft.com/azure/postgresql/quickstart-create-hyperscale-portal#connect-to-the-database-using-psql)

* [Key Vault oluşturma](https://docs.microsoft.com/azure/key-vault/quick-create-portal)

* [Yönetilen kimlik ile Key Vault kimlik doğrulaması sağlama](https://docs.microsoft.com/azure/key-vault/managed-identity)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: PostgreSQL Için Azure veritabanı sunucusu, veritabanlarına erişmek için Azure Active Directory kimlik doğrulamasını destekler. PostgreSQL için Azure veritabanı sunucusu oluştururken, yönetici kullanıcı için kimlik bilgilerini sağlarsınız. Bu yönetici, ek veritabanı kullanıcıları oluşturmak için kullanılabilir.

PostgreSQL için Azure veritabanı sunucusuna erişmek üzere kullanılan Azure App Service üzerinde çalışan Azure sanal makineleri veya Web uygulamaları için, PostgreSQL için Azure veritabanı sunucusu için kimlik bilgilerini depolamak ve almak üzere Azure Key Vault ile birlikte Yönetilen Hizmet Kimliği kullanın. Key Vault geçici silmenin etkinleştirildiğinden emin olun.

Azure Active Directory (AD) içinde otomatik olarak yönetilen bir kimlik ile Azure hizmetleri sağlamak için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar.

* [Yönetilen kimlikleri yapılandırma](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm)

* [Azure yönetilen kimliklerle tümleştirme](https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Şu anda kullanılamıyor

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

* [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü amaçlı yazılımdan koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure App Service) etkinleştirilir, ancak müşteri içeriklerde çalıştırılmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetlerini destekleyen temel ana bilgisayar üzerinde etkindir--Örneğin, hiper ölçek (Citus), ancak müşteri içeriği üzerinde çalışmaz.

App Service, Data Lake Storage, BLOB depolama, PostgreSQL için Azure veritabanı vb. gibi işlem dışı Azure kaynaklarına karşıya yüklenen tüm içerikleri önceden tarayın. Microsoft bu örneklerdeki verilerinize erişemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetlerini destekleyen temel bir konakta (örneğin, hiper ölçek (Citus) etkinleştirilir, ancak müşteri içeriğinde çalışmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: PostgreSQL Için Azure veritabanı – hyperscale (Citus), her bir düğümün yedeklemesini otomatik olarak oluşturur ve yerel olarak yedekli depolamada depolar. Yedeklemeler, hiper ölçek (Citus) kümenizi belirtilen bir zamana geri yüklemek için kullanılabilir.

* [PostgreSQL için Azure veritabanı 'nda yedekleme ve geri yükleme-hiper ölçek (Citus)](https://docs.microsoft.com/azure/postgresql/concepts-hyperscale-backup)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tam sistem yedeklemeleri gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: günde en az bir kez, PostgreSQL Için Azure veritabanı, veri dosyalarının anlık görüntü yedeklemelerini ve veritabanı işlem günlüğünü alır. Yedeklemeler, bir sunucuyu Bekletme dönemi içinde herhangi bir zaman noktasına geri yüklemenize olanak tanır. Saklama dönemi Şu anda tüm kümeler için 35 gündür. Tüm yedeklemeler AES 256 bit şifreleme kullanılarak şifrelenir.

Kullanılabilirlik alanlarını destekleyen Azure bölgelerinde, yedekleme anlık görüntüleri üç kullanılabilirlik alanında depolanır. En az bir kullanılabilirlik bölgesi çevrimiçi olduğu sürece, hiper ölçek (Citus) kümesi yeniden geri yüklenebilen.

* [PostgreSQL için Azure veritabanı 'nda yedekleme ve geri yükleme-hiper ölçek (Citus)](https://docs.microsoft.com/azure/postgresql/concepts-hyperscale-backup)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşteri tarafından yönetilen anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: PostgreSQL Için Azure veritabanı 'Nda, hiper ölçek (Citus) kümesini geri yüklemek, özgün düğümlerin yedeklemelerinden yeni bir küme oluşturur. Bir kümeyi, son 35 gün içinde herhangi bir zaman noktasına geri yükleyebilirsiniz. Geri yükleme işlemi, orijinal olarak aynı Azure bölgesinde, abonelikte ve kaynak grubunda yeni bir küme oluşturur. Yeni küme yapılandırması, özgün küme yapılandırması ile aynıdır: aynı düğüm sayısı, sanal çekirdek sayısı, depolama boyutu ve Kullanıcı rolleri.

Güvenlik Duvarı ayarları ve PostgreSQL sunucu parametreleri özgün sunucu grubundan korunmaz; varsayılan değerlere sıfırlanır. Güvenlik Duvarı tüm bağlantıları engeller. Geri yükleme sonrasında bu ayarları el ile ayarlamanız gerekir.

* [PostgreSQL için Azure veritabanı 'nda yedekleme ve geri yükleme-hiper ölçek (Citus)](https://docs.microsoft.com/azure/postgresql/concepts-hyperscale-backup)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Kılavuz**: silinen hyperscale (Citus) kümeleri geri yüklenemez. Kümeyi silerseniz, kümeye ait olan tüm düğümler silinir ve kurtarılamaz. Küme kaynaklarını yanlışlıkla silinmeye veya beklenmeyen değişikliklerden korumak için Yöneticiler yönetim kilitlerinin faydalanabilir.

* [PostgreSQL için Azure veritabanı 'nda yedekleme ve geri yükleme-hiper ölçek (Citus)](https://docs.microsoft.com/azure/postgresql/concepts-hyperscale-backup)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

* [Azure Güvenlik Merkezi 'nde Iş akışı otomasyonlarını yapılandırma](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)

* [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Müşteri, kendi olay yanıt planının oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu da kullanabilir](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

* [NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), müşterinin verilerine izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

* [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: sürekli dışa aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Uyarılar Sentinel 'i akışa almak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz.

* [Sürekli dışarı aktarmayı yapılandırma](https://docs.microsoft.com/azure/security-center/continuous-export)

* [Uyarıları Azure Sentinel 'e akış](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: güvenlik uyarılarında ve önerilerinde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek Için Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanın.

* [Iş akışı otomasyonu ve Logic Apps yapılandırma](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: * [Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

* [Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına göre kırmızı ekip oluşturma ve canlı site sızma testini yürütme hakkında daha fazla bilgi edinebilirsiniz.](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
