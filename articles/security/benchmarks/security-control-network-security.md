---
title: Azure Güvenlik denetimi-ağ güvenliği
description: Azure Güvenlik denetimi ağ güvenliği
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: cc52d1f6eec5f1aedb0db37b3945f7be6d9f62a8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "100595642"
---
# <a name="security-control-network-security"></a>Güvenlik denetimi: ağ güvenliği

Ağ güvenliği önerileri, hangi ağ protokollerini, TCP/UDP bağlantı noktalarını ve ağa bağlı hizmetleri Azure hizmetlerine erişime izin verildiğini ve bu hizmetlere erişimi reddedildiğini belirtmeye odaklanmaktadır.

## <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: sanal ağlar içindeki Azure kaynaklarını koruma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.1 | 9,2, 9,4, 14,1, 14,2, 14,3 | Müşteri |

Tüm sanal ağ alt ağı dağıtımlarının, uygulamanızın güvenilen bağlantı noktalarına ve kaynaklarına özel ağ erişim denetimleriyle uygulanmış bir ağ güvenlik grubu olduğundan emin olun. Kullanılabilir olduğunda, VNet kimliğini hizmete genişleterek Azure hizmet kaynaklarınızı sanal ağınıza güvenli hale getirmek için özel bir bağlantı ile özel uç noktalar kullanın. Özel uç noktalar ve özel bağlantı kullanılabilir olmadığında, hizmet uç noktalarını kullanın. Hizmete özgü gereksinimler için lütfen ilgili hizmetin güvenlik önerisi bölümüne bakın. 

Alternatif olarak, belirli bir kullanım durumu varsa, Azure Güvenlik Duvarı 'Nı uygulayarak gereksinim karşılamayabilir.

- [Sanal ağ hizmeti uç noktalarını anlama](../../virtual-network/virtual-network-service-endpoints-overview.md)

- [Azure özel bağlantısını anlama](../../private-link/private-link-overview.md)

- [Sanal ağ oluşturma](../../virtual-network/quick-create-portal.md)

- [Güvenlik Yapılandırması ile NSG oluşturma](../../virtual-network/tutorial-filter-network-traffic.md)

- [Azure Güvenlik duvarını dağıtma ve yapılandırma](../../firewall/tutorial-firewall-deploy-portal.md)

## <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1,2: sanal ağların, alt ağların ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.2 | 9,3, 12,2, 12,8 | Müşteri |

Azure Güvenlik Merkezi 'ni kullanarak ağ kaynaklarınızı Azure 'da güvenli hale getirmeye yardımcı olması için ağ koruma önerilerini izleyin. Trafik denetimi için NSG akış günlüklerini etkinleştirin ve günlükleri bir depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

- [NSG akış günlüklerini etkinleştirme](../../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Trafik Analizi etkinleştirme ve kullanma](../../network-watcher/traffic-analytics.md)

- [Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlama](../../security-center/security-center-network-recommendations.md)

## <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.3 | 9,5 | Müşteri |

Azure Web uygulaması güvenlik duvarını (WAF), gelen trafiğin ek incelemesi için kritik Web uygulamaları önünde dağıtın. WAF ve alma günlükleri için tanılama ayarını bir depolama hesabı, Olay Hub 'ı veya Log Analytics çalışma alanında etkinleştirin.

- [Azure WAF dağıtma](../../web-application-firewall/ag/create-waf-policy-ag.md)

## <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.4 | 12,3 | Müşteri |

DDoS saldırılarına karşı koruma sağlamak için Azure sanal ağlarınızda DDoS standart korumasını etkinleştirin. Bilinen kötü amaçlı IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.

Tehdit zekası etkinleştirilmiş ve kötü amaçlı ağ trafiği için "uyarı ve reddetme" olarak yapılandırılmış bir kuruluşun ağ sınırlarının her birinde Azure Güvenlik Duvarı 'Nı dağıtın.

NSG 'leri, sınırlı bir süre için bitiş noktaları onaylı IP adresleriyle sınırlamak üzere Azure Güvenlik Merkezi 'ni tam zamanında ağ erişimi ile kullanın.

Bağlantı noktalarını ve kaynak IP 'Leri gerçek trafik ve tehdit zeka göre sınırlayan NSG yapılandırmalarının kullanılması için Azure Güvenlik Merkezi Uyarlamalı ağ sağlamlaştırma kullanın.

- [DDoS korumasını yapılandırma](../../ddos-protection/manage-ddos-protection.md)

- [Azure Güvenlik duvarını dağıtma](../../firewall/tutorial-firewall-deploy-portal.md)

- [Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama](../../security-center/azure-defender.md)

- [Azure Güvenlik Merkezi Uyarlamalı ağ sağlamlaştırma 'yi anlama](../../security-center/security-center-adaptive-network-hardening.md)

- [Azure Güvenlik Merkezi 'Ni tam zamanında ağ Access Control anlama](../../security-center/security-center-just-in-time.md)

## <a name="15-record-network-packets"></a>1,5: ağ paketlerini kaydetme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1,5 | 12,5 | Müşteri |

Anormal etkinlikleri araştırmak için ağ Izleyicisi paket yakalamayı etkinleştirin.

- [Ağ İzleyicisini etkinleştirme](../../network-watcher/network-watcher-create.md)

## <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.6 | 12,6, 12,7 | Müşteri |

Azure Marketi 'nden, yük İnceleme özelliklerine sahip KIMLIKLER/IP 'leri işlevselliğini destekleyen bir teklif seçin.  Yük incelemesini temel alan yetkisiz giriş algılama ve/veya önleme bir gereksinim değilse, tehdit zekasıyla Azure Güvenlik Duvarı kullanılabilir. Azure Güvenlik Duvarı tehdit zekası tabanlı filtreleme, bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen ve giden trafiği uyarabilir ve reddedebilir. IP adresleri ve etki alanları, Microsoft Tehdit Analizi akışından alınır.

Kötü amaçlı trafiği algılamak ve/veya reddetmek için kuruluşunuzun ağ sınırlarının her birinde tercih ettiğiniz güvenlik duvarı çözümünü dağıtın.

- [Azure Market](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

- [Azure Güvenlik duvarını dağıtma](../../firewall/tutorial-firewall-deploy-portal.md)

- [Azure Güvenlik Duvarı ile uyarıları yapılandırma](../../firewall/threat-intel.md)

## <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.7 | 12,9, 12,10 | Müşteri |

Güvenilen sertifikalara yönelik olarak HTTPS/TLS özellikli Web uygulamaları için Azure Application Gateway dağıtın.

- [Application Gateway dağıtma](../../application-gateway/quick-create-portal.md)

- [Application Gateway HTTPS kullanacak şekilde yapılandırma](../../application-gateway/create-ssl-portal.md)

- [Azure Web uygulaması ağ geçitleri ile katman 7 yük dengelemesini anlama](../../application-gateway/overview.md)

## <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.8 | 1,5 | Müşteri |

Ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örn., Apimanaya) belirterek, ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

Ayrıca, karmaşık güvenlik yapılandırmasını basitleştirmeye yardımcı olması için uygulama güvenlik gruplarını kullanabilirsiniz. Uygulama güvenlik grupları ağ güvenliğini uygulamanın yapısının doğal bir uzantısı olarak yapılandırmanıza imkan vererek sanal makineleri gruplamanızı ve ağ güvenlik ilkelerini bu gruplara göre tanımlamanızı sağlar.

- [Hizmet etiketlerini anlama ve kullanma](../../virtual-network/service-tags-overview.md)

- [Uygulama güvenlik gruplarını anlama ve kullanma](../../virtual-network/network-security-groups-overview.md#application-security-groups)

## <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.9 | 11.1 | Müşteri |

Azure Ilkesiyle ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın.

Azure şemaları, Azure kaynakları Yöneticisi şablonları, Azure RBAC denetimleri ve ilkeleri gibi tek bir şema tanımında paket ortam yapıtları sunarak büyük ölçekli Azure dağıtımlarını basitleştirecek şekilde de kullanabilirsiniz. Şema 'i yeni aboneliklere uygulayabilir ve sürüm oluşturma aracılığıyla denetim ve yönetimi ayarlayabilirsiniz.

- [Azure İlkesi'ni yapılandırma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

- [Ağ için Azure Ilke örnekleri](../../governance/policy/samples/built-in-policies.md#network)

- [Azure Blueprint oluşturma](../../governance/blueprints/create-blueprint-portal.md)

## <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.10 | 11,2 | Müşteri |

NSG 'ler ve ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için Etiketler kullanın. Bireysel NSG kuralları için "Açıklama" alanını kullanarak ağa giden/giden trafiğe izin veren kuralların iş gereksinimini ve/veya süresini (vs.) belirtin.

Tüm kaynakların etiketlerle oluşturulmasını ve mevcut etiketlenmemiş kaynakları bilgilendirmesini sağlamak için etiketlemeyle ilgili yerleşik Azure Ilke tanımlarından herhangi birini ("etiket ve onun değeri gerektir" gibi) kullanın.

Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynaklar üzerinde arama yapabilir veya eylemler gerçekleştirebilirsiniz.

- [Etiketler oluşturma ve kullanma](../../azure-resource-manager/management/tag-resources.md)

- [Sanal ağ oluşturma](../../virtual-network/quick-create-portal.md)

- [Güvenlik Yapılandırması ile NSG oluşturma](../../virtual-network/tutorial-filter-network-traffic.md)

## <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1,11 | 11.3 | Müşteri |

Azure etkinlik günlüğü 'nü kullanarak kaynak yapılandırmalarının yanı sıra Azure kaynaklarınızda yapılan değişiklikleri tespit edin. Kritik kaynaklardaki değişiklikler gerçekleşirken tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

- [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](../../azure-monitor/essentials/activity-log.md#view-the-activity-log)

- [Azure Izleyici 'de uyarı oluşturma](../../azure-monitor/alerts/alerts-activity-log.md)

## <a name="next-steps"></a>Sonraki adımlar

- Sonraki güvenlik denetimine bakın: [günlüğe kaydetme ve izleme](security-control-logging-monitoring.md)