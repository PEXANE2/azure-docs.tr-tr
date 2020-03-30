---
title: Azure Güvenlik Denetimi - Ağ Güvenliği
description: Güvenlik Kontrol Ağı Güvenliği
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 7916bbb28602d64e0916fce7badf16a65c242227
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77251881"
---
# <a name="security-control-network-security"></a>Güvenlik Denetimi: Ağ Güvenliği

Ağ güvenliği önerileri, hangi ağ protokollerinin, TCP/UDP bağlantı noktalarının ve ağa bağlı hizmetlerin Azure hizmetlerine erişime izin verildiğini veya bu hizmetlere erişiminin engellenici olduğunu belirtmeye odaklanır.

## <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Sanal Ağınızdaki Ağ Güvenlik Grupları veya Azure Güvenlik Duvarı'nı kullanarak kaynakları koruyun

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.1 | 9.2, 9.4, 14.1-14.3 | Müşteri |

Tüm Sanal Ağ alt ağ dağıtımlarının, uygulamanızın güvenilir bağlantı noktalarına ve kaynaklarına özgü ağ erişim denetimleriyle birlikte uygulanan bir Ağ Güvenlik Grubu olduğundan emin olun. Azure Hizmetlerini Özel Bağlantı etkinleştirilmiş olarak kullanın, hizmeti Vnet'inizin içine dağıtın veya Özel Uç Noktaları'nı kullanarak özel olarak bağlanın. Hizmete özel gereksinimler için lütfen söz konusu hizmetiçin güvenlik önerisine bakın.

Alternatif olarak, belirli bir kullanım örneğiniz varsa, gereksinimleri Azure Güvenlik Duvarı uygulanarak karşılanabilir.

Özel Bağlantı Hakkında Genel Bilgiler:

https://docs.microsoft.com/azure/private-link/private-link-overview

Sanal Ağ nasıl oluşturulur:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Güvenlik yapılandırması olan bir NSG nasıl oluşturulur:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

## <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Vnet, Subnet ve NIC'lerin yapılandırmasını ve trafiğini izleyin ve kaydedin

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.2 | 9.3, 12.2 | Müşteri |

Azure Güvenlik Merkezi'ni kullanın ve Azure'da ağ kaynaklarınızın güvenliğini sağlamaya yardımcı olmak için ağ koruma önerilerine uyun. NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir Depolama Hesabına gönderin.

NSG Akış Günlükleri Nasıl Etkinleştirilir:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Azure Güvenlik Merkezi tarafından sağlanan Ağ Güvenliğini Anlayın:

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

## <a name="13-protect-critical-web-applications"></a>1.3: Kritik web uygulamalarını koruyun

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.3 | 9.5 | Müşteri |

Gelen trafiğin ek denetimi için kritik web uygulamalarının önüne Azure Web Uygulaması Güvenlik Duvarı 'nı (WAF) dağıtın. WAF için Tanılama Ayarını etkinleştirin ve günlükleri bir Depolama Hesabı, Olay Hub'ı veya Log Analytics Çalışma alanına alın.

Azure WAF nasıl dağıtılır:

https://docs.microsoft.com/azure/web-application-firewall/ag/create-waf-policy-ag

## <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Bilinen kötü amaçlı IP adresleriyle iletişimi reddetme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.4 | 12.3 | Müşteri |

DDoS saldırılarına karşı korunmak için Azure Sanal Ağlarınızda DDoS Standart korumasını etkinleştirin. Bilinen kötü amaçlı IP adresleriyle iletişimi reddetmek için Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı kullanın.

Tehdit İstihbaratı etkinleştirilmiş ve Kötü amaçlı ağ trafiği için Uyarı &quot;ve reddedilecek&quot; şekilde yapılandırılmışolmasıyla kuruluşun ağ sınırlarının her birinde Azure Güvenlik Duvarı'nı dağıtın.

Son noktaların sınırlı bir süre için onaylanan IP adresleriyle maruz kalmasını sınırlamak için NSG'leri yapılandırmak için Azure Güvenlik Merkezi Just In Time Network erişimini kullanın.

Gerçek trafik ve tehdit istihbaratına dayalı bağlantı noktalarını ve kaynak IP'leri sınırlayan NSG yapılandırmalarını önermek için Azure Güvenlik Merkezi Uyarlamalı Ağ Sertleştirme'yi kullanın.

DDoS koruması nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

Azure Güvenlik Duvarı nasıl dağıtılır:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı Anlayın:

https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer

Azure Güvenlik Merkezi Uyarlamalı Ağ Sertleştirmeyi Anlayın:

https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening

Azure Güvenlik Merkezi'ni tam zamanında ağ erişimi denetiminde anlayın:

https://docs.microsoft.com/azure/security-center/security-center-just-in-time

## <a name="15-record-network-packets-and-flow-logs"></a>1.5: Ağ paketlerini ve akış günlüklerini kaydetme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1,5 | 12.5, 15.8 | Müşteri |

Akış kayıtları oluşturmak için NSG akışını bir depolama hesabına kaydedin. Anormal etkinliği araştırmak için gerekirse, Ağ İzleyicipaket yakalamayı etkinleştirin.

NSG Akış Günlükleri Nasıl Etkinleştirilir:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Ağ İzleyicisi nasıl etkinleştirilir:

https://docs.microsoft.com/azure/network-watcher/network-watcher-create

## <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Ağ tabanlı izinsiz giriş algılama/saldırı önleme sistemlerini (IDS/IPS) dağıtma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.6 | 12.6, 12.7 | Müşteri |

Tehdit İstihbaratı etkinleştirilmiş ve Kötü amaçlı ağ trafiği için Uyarı &quot;ve reddedilecek&quot; şekilde yapılandırılmışolmasıyla kuruluşun ağ sınırlarının her birinde Azure Güvenlik Duvarı'nı dağıtın.

Azure Güvenlik Duvarı nasıl dağıtılır:https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Azure Güvenlik Duvarı ile uyarıları yapılandırma:https://docs.microsoft.com/azure/firewall/threat-intel

## <a name="17-manage-traffic-to-web-applications"></a>1.7: Web uygulamalarına trafik yönetme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.7 | 12.9, 12.10 | Müşteri |

GÜVENLİ SERTIFIKALAR İçİn HTTPS/SSL etkinleştirilmiş web uygulamaları için Azure Uygulama Ağ Geçidi'ni dağıtın.

Uygulama Ağ Geçidi nasıl dağıtılır:

https://docs.microsoft.com/azure/application-gateway/quick-create-portal

HTTPS'yi kullanmak için Uygulama Ağ Geçidi nasıl yapılandırılasınız:

https://docs.microsoft.com/azure/application-gateway/create-ssl-portal

Azure web uygulama ağ geçitleri ile katman 7 yük dengelemeyi anlayın:

https://docs.microsoft.com/azure/application-gateway/overview

## <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Ağ güvenliği kurallarının karmaşıklığını ve yönetim ek lerini en aza indirmek

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1,8 | 1,5 | Müşteri |

Ağ Güvenlik Grupları veya Azure Güvenlik Duvarı'ndaki ağ erişim denetimlerini tanımlamak için Sanal Ağ Hizmeti Etiketleri'ni kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında servis etiketi adını (örn. ApiManagement) belirterek, ilgili hizmetin trafiğine izin verebilir veya reddedebilirsiniz. Microsoft, hizmet etiketinin kapsadığı adres öneklerini yönetir ve adresler değiştikçe servis etiketini otomatik olarak güncelleştirir.

Hizmet Etiketlerini anlama ve kullanma:

https://docs.microsoft.com/azure/virtual-network/service-tags-overview

## <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Ağ aygıtları için standart güvenlik yapılandırmalarını koruyun

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.9 | 11.1 | Müşteri |

Azure İlkesi ile ağ kaynakları için standart güvenlik yapılandırmaları tanımlayın ve uygulayın.

Azure Kaynak Yöneticisi şablonları, RBAC denetimleri ve ilkeleri gibi önemli ortam yapılarını tek bir plan tanımında paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure Planları'nı da kullanabilirsiniz. Sürümü aracılığıyla planı yeni aboneliklere uygulayabilir ve denetim ve yönetime ince ayar yapabilirsiniz.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Ağ oluşturmak için Azure İlkesi örnekleri:

https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#network

Azure Blueprint nasıl oluşturulur:

https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

## <a name="110-document-traffic-configuration-rules"></a>1.10: Belge trafiği yapılandırma kuralları

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.1 | 11.2 | Müşteri |

NSG'ler ve ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için Etiketleri kullanın. Tek tek NSG kuralları &quot;&quot; için, bir ağa/aktan trafiğin alınmasına izin veren kurallar için iş gereksinimini ve/veya süresini (vb.) belirtmek için Açıklama alanını kullanın.

Etiketler nasıl oluşturulur ve kullanılır:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Sanal Ağ nasıl oluşturulur:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Güvenlik Config ile bir NSG oluşturmak için nasıl:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

## <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Ağ kaynağı yapılandırmalarını izlemek ve değişiklikleri algılamak için otomatik araçlar kullanın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 1.11 | 11.3 | Müşteri |

Ağ kaynakları için yapılandırmayı doğrulamak (ve/veya düzeltmek) için Azure İlkesi'ni kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Ağ oluşturmak için Azure İlkesi örnekleri:

https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#network

## <a name="next-steps"></a>Sonraki adımlar

- Bir sonraki güvenlik denetimine bakın: [Günlük ve İzleme](security-control-logging-monitoring.md)
