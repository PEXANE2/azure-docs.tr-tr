---
title: Azure Güvenlik Denetimi - Ağ Güvenliği
description: Azure Güvenlik Denetimi Ağı Güvenliği
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: f508de35470043cc71bfc8607367f28c04440b57
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81408312"
---
# <a name="security-control-network-security"></a>Güvenlik Denetimi: Ağ Güvenliği

Ağ güvenliği önerileri, hangi ağ protokollerinin, TCP/UDP bağlantı noktalarının ve ağa bağlı hizmetlerin Azure hizmetlerine erişime izin verildiğini veya bu hizmetlere erişiminin engellenici olduğunu belirtmeye odaklanır.

## <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Azure kaynaklarını sanal ağlarda koruyun

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.1 | 9.2, 9.4, 14.1, 14.2, 14.3 | Müşteri |

Tüm Sanal Ağ alt ağ dağıtımlarının, uygulamanızın güvenilir bağlantı noktalarına ve kaynaklarına özgü ağ erişim denetimleriyle birlikte uygulanan bir Ağ Güvenlik Grubu olduğundan emin olun. Kullanılabilir olduğunda, VNet kimliğini hizmete genişleterek Azure hizmet kaynaklarınızı sanal ağınıza sabitlemek için Özel Bağlantı ile Özel Uç Noktaları'nı kullanın. Özel Bitiş Noktaları ve Özel Bağlantı kullanılamıyorsa, Hizmet Bitiş Noktaları'nı kullanın. Hizmete özel gereksinimler için lütfen söz konusu hizmetiçin güvenlik önerisine bakın. 

Alternatif olarak, belirli bir kullanım örneğine sahipseniz, Azure Güvenlik Duvarı'nı uygulayarak gereksinim karşılanabilir.

- [Sanal Ağ Hizmeti Bitiş Noktalarını Anlayın](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)

- [Azure Özel Bağlantısını Anlayın](https://docs.microsoft.com/azure/private-link/private-link-overview)

- [Sanal Ağ nasıl oluşturulur?](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

- [Güvenlik yapılandırması olan bir NSG nasıl oluşturulur?](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

- [Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

## <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1.2: Sanal ağların, alt ağların ve NIC'lerin yapılandırmasını ve trafiğini izleyin ve kaydedin

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.2 | 9.3, 12.2, 12.8 | Müşteri |

Azure Güvenlik Merkezi'ni kullanın ve Azure'da ağ kaynaklarınızın güvenliğini sağlamaya yardımcı olmak için ağ koruma önerilerine uyun. NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir Depolama Hesabına gönderin. Ayrıca NSG akış günlüklerini bir Log Analytics Çalışma Alanına gönderebilir ve Azure bulutunuzdaki trafik akışıhakkında bilgi sağlamak için Trafik Analitiği'ni kullanabilirsiniz. Traffic Analytics'in bazı avantajları, ağ etkinliğini görselleştirme ve etkin noktaları belirleme, güvenlik tehditlerini belirleme, trafik akışı desenlerini anlama ve ağ yanlış yapılandırmalarını saptabilme yeteneğidir.

- [NSG Akış Günlükleri Nasıl Etkinleştirilir?](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

- [Trafik Analizi ni etkinleştirme ve kullanma](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

- [Azure Güvenlik Merkezi tarafından sağlanan Ağ Güvenliğini Anlama](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

## <a name="13-protect-critical-web-applications"></a>1.3: Kritik web uygulamalarını koruyun

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.3 | 9.5 | Müşteri |

Gelen trafiğin ek denetimi için kritik web uygulamalarının önüne Azure Web Uygulaması Güvenlik Duvarı 'nı (WAF) dağıtın. WAF için Tanılama Ayarını etkinleştirin ve günlükleri bir Depolama Hesabı, Olay Hub'ı veya Log Analytics Çalışma alanına alın.

- [Azure WAF nasıl dağıtılır?](https://docs.microsoft.com/azure/web-application-firewall/ag/create-waf-policy-ag)

## <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Bilinen kötü amaçlı IP adresleriyle iletişimi reddetme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.4 | 12.3 | Müşteri |

DDoS saldırılarına karşı korunmak için Azure Sanal Ağlarınızda DDoS Standart korumasını etkinleştirin. Bilinen kötü amaçlı IP adresleriyle iletişimi reddetmek için Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı kullanın.

Tehdit İstihbaratı etkin leştirilmiş ve kötü amaçlı ağ trafiği için "Uyarı ve reddet" olarak yapılandırılan kuruluşun ağ sınırlarının her birinde Azure Güvenlik Duvarı'nı dağıtın.

Son noktaların sınırlı bir süre için onaylanan IP adresleriyle maruz kalmasını sınırlamak için NSG'leri yapılandırmak için Azure Güvenlik Merkezi Just In Time Network erişimini kullanın.

Gerçek trafik ve tehdit istihbaratına dayalı bağlantı noktalarını ve kaynak IP'leri sınırlayan NSG yapılandırmalarını önermek için Azure Güvenlik Merkezi Uyarlamalı Ağ Sertleştirme'yi kullanın.

- [DDoS koruması nasıl yapılandırılır?](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

- [Azure Güvenlik Duvarı nasıl dağıtılır?](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

- [Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı Anlayın](https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer)

- [Azure Güvenlik Merkezi Uyarlamalı Ağ Sertleştirmeyi Anlayın](https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening)

- [Azure Güvenlik Merkezi'ni Tam Zamanında Ağ Erişim Denetiminde Anlayın](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)

## <a name="15-record-network-packets"></a>1.5: Ağ paketlerini kaydetme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1,5 | 12,5 | Müşteri |

Anormal etkinlikleri araştırmak için Ağ İzleyici paket yakalamayı etkinleştirin.

- [Ağ İzleyicisi nasıl etkinleştirilir?](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

## <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Ağ tabanlı izinsiz giriş algılama/saldırı önleme sistemlerini (IDS/IPS) dağıtma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.6 | 12.6, 12.7 | Müşteri |

Azure Marketi'nden, yük denetimi özellikleriyle IDS/IPS işlevselliğini destekleyen bir teklif seçin.  Yük denetimine dayalı izinsiz giriş algılama ve/veya önleme bir gereklilik değilse, Tehdit İstihbaratı içeren Azure Güvenlik Duvarı kullanılabilir. Azure Güvenlik Duvarı Tehdit İstihbarat tabanlı filtreleme, bilinen kötü amaçlı IP adreslerine ve etki alanlarındaki trafiği uyarabilir ve reddedebilir. IP adresleri ve etki alanları Microsoft Threat Intelligence akışından kaynaklanmaktadır.

Kötü amaçlı trafiği algılamak ve/veya reddetmek için kuruluşunuzun ağ sınırlarının her birinde seçtiğiniz güvenlik duvarı çözümünün dağıtılanın.

- [Azure Market](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

- [Azure Güvenlik Duvarı nasıl dağıtılır?](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

- [Azure Güvenlik Duvarı ile uyarıları yapılandırma](https://docs.microsoft.com/azure/firewall/threat-intel)

## <a name="17-manage-traffic-to-web-applications"></a>1.7: Web uygulamalarına trafik yönetme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.7 | 12.9, 12.10 | Müşteri |

GÜVENLİ SERTIFIKALAR İçİn HTTPS/SSL etkinleştirilmiş web uygulamaları için Azure Uygulama Ağ Geçidi'ni dağıtın.

- [Uygulama Ağ Geçidi nasıl dağıtılır?](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)

- [HTTPS'yi kullanmak için Uygulama Ağ Geçidi nasıl yapılandırılır?](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal)

- [Azure web uygulama ağ geçitleri ile katman 7 yük dengelemeyi anlama](https://docs.microsoft.com/azure/application-gateway/overview)

## <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Ağ güvenliği kurallarının karmaşıklığını ve yönetim ek lerini en aza indirmek

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1,8 | 1,5 | Müşteri |

Ağ Güvenlik Grupları veya Azure Güvenlik Duvarı'ndaki ağ erişim denetimlerini tanımlamak için Sanal Ağ Hizmeti Etiketleri'ni kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında servis etiketi adını (örn. ApiManagement) belirterek, ilgili hizmetin trafiğine izin verebilir veya reddedebilirsiniz. Microsoft, hizmet etiketinin kapsadığı adres öneklerini yönetir ve adresler değiştikçe servis etiketini otomatik olarak güncelleştirir.

Karmaşık güvenlik yapılandırmasını basitleştirmeye yardımcı olmak için Uygulama Güvenlik Grupları'nı da kullanabilirsiniz. Uygulama güvenlik grupları ağ güvenliğini uygulamanın yapısının doğal bir uzantısı olarak yapılandırmanıza imkan vererek sanal makineleri gruplamanızı ve ağ güvenlik ilkelerini bu gruplara göre tanımlamanızı sağlar.

- [Hizmet Etiketlerini anlama ve kullanma](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

- [Uygulama Güvenlik Gruplarını anlama ve kullanma](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups)

## <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Ağ aygıtları için standart güvenlik yapılandırmalarını koruyun

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.9 | 11.1 | Müşteri |

Azure İlkesi ile ağ kaynakları için standart güvenlik yapılandırmaları tanımlayın ve uygulayın.

Azure Kaynakları Yöneticisi şablonları, RBAC denetimleri ve ilkeleri gibi önemli ortam yapılarını tek bir plan tanımında paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure Planları'nı da kullanabilirsiniz. Planı yeni aboneliklere uygulayabilir ve sürüm üzerinden denetim ve yönetime ince ayar yapabilirsiniz.

- [Azure İlkesi nasıl yapılandırılatır ve yönetilir?](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Ağ için Azure İlkesi örnekleri](https://docs.microsoft.com/azure/governance/policy/samples/#network)

- [Azure Blueprint nasıl oluşturulur?](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

## <a name="110-document-traffic-configuration-rules"></a>1.10: Belge trafiği yapılandırma kuralları

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.10 | 11.2 | Müşteri |

NSG'ler ve ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için Etiketleri kullanın. Tek tek NSG kuralları için, bir ağa/aktan trafiğin alınmasına izin veren kurallar için iş gereksinimini ve/veya süresini (vb.) belirtmek için "Açıklama" alanını kullanın.

Tüm kaynakların Etiketler ile oluşturulduğundan emin olmak ve varolan etiketlenmemiş kaynakları size bildirmek için etiketlemeyle ilgili "Etiket ve değeri gerektir" gibi yerleşik Azure ilke tanımlarından herhangi birini kullanın.

Azure PowerShell veya Azure CLI'yi, Etiketleri'ni temel alan kaynaklara bakmak veya eylemlergerçekleştirmek için kullanabilirsiniz.

- [Etiketler nasıl oluşturulur ve kullanılır](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

- [Sanal Ağ nasıl oluşturulur?](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

- [Güvenlik Config ile NSG nasıl oluşturulur?](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

## <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Ağ kaynağı yapılandırmalarını izlemek ve değişiklikleri algılamak için otomatik araçlar kullanın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.11 | 11.3 | Müşteri |

Kaynak yapılandırmalarını izlemek ve Azure kaynaklarınızdaki değişiklikleri algılamak için Azure Etkinlik Günlüğü'ni kullanın. Azure Monitor'da kritik kaynaklarda değişiklikler olduğunda tetikleyecek uyarılar oluşturun.

- [Azure Etkinlik Günlüğü etkinliklerini görüntüleme ve alma](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

- [Azure Monitor'da uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

## <a name="next-steps"></a>Sonraki adımlar

- Sonraki Güvenlik Denetimine bakın: [Günlük ve İzleme](security-control-logging-monitoring.md)
