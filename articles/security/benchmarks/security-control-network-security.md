---
title: Azure Güvenlik denetimi-ağ güvenliği
description: Güvenlik denetimi ağ güvenliği
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 7916bbb28602d64e0916fce7badf16a65c242227
ms.sourcegitcommit: 79cbd20a86cd6f516acc3912d973aef7bf8c66e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77251881"
---
# <a name="security-control-network-security"></a>Güvenlik denetimi: ağ güvenliği

Ağ güvenliği önerileri, hangi ağ protokollerini, TCP/UDP bağlantı noktalarını ve ağa bağlı hizmetleri Azure hizmetlerine erişime izin verildiğini ve bu hizmetlere erişimi reddedildiğini belirtmeye odaklanmaktadır.

## <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.1 | 9,2, 9,4, 14.1-14.3 | Müşteri |

Tüm sanal ağ alt ağı dağıtımlarının, uygulamanızın güvenilen bağlantı noktalarına ve kaynaklarına özel ağ erişim denetimleriyle uygulanmış bir ağ güvenlik grubu olduğundan emin olun. Özel bağlantı etkinken Azure hizmetlerini kullanın, hizmeti VNET 'iniz içinde dağıtın veya özel uç noktaları kullanarak özel olarak bağlayın. Hizmete özgü gereksinimler için lütfen ilgili hizmetin güvenlik önerisi bölümüne bakın.

Alternatif olarak, belirli bir kullanım durumu varsa, gereksinimler Azure Güvenlik Duvarı kullanılarak karşılanır.

Özel bağlantıyla ilgili genel bilgiler:

https://docs.microsoft.com/azure/private-link/private-link-overview

Sanal ağ oluşturma:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Güvenlik Yapılandırması ile NSG oluşturma:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

Azure Güvenlik duvarını dağıtma ve yapılandırma:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

## <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2: VNet, alt ağlar ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.2 | 9,3, 12,2 | Müşteri |

Azure Güvenlik Merkezi 'ni kullanarak ağ kaynaklarınızı Azure 'da güvenli hale getirmeye yardımcı olması için ağ koruma önerilerini izleyin. Trafik denetimi için NSG akış günlüklerini etkinleştirin ve günlükleri bir depolama hesabına gönderin.

NSG akış günlüklerini etkinleştirme:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlayın:

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

## <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.3 | 9.5 | Müşteri |

Azure Web uygulaması güvenlik duvarını (WAF), gelen trafiğin ek incelemesi için kritik Web uygulamaları önünde dağıtın. WAF ve alma günlükleri için tanılama ayarını bir depolama hesabı, Olay Hub 'ı veya Log Analytics çalışma alanında etkinleştirin.

Azure WAF 'yi dağıtma:

https://docs.microsoft.com/azure/web-application-firewall/ag/create-waf-policy-ag

## <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.4 | 12,3 | Müşteri |

DDoS saldırılarına karşı koruma sağlamak için Azure sanal ağlarınızda DDoS standart korumasını etkinleştirin. Bilinen kötü amaçlı IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.

Tehdit zekası etkinleştirilmiş ve kötü amaçlı ağ trafiği için &quot;uyarı ve reddetme&quot; üzere yapılandırılmış bir kuruluşun ağ sınırlarının her birinde Azure Güvenlik Duvarı 'Nı dağıtın.

NSG 'leri, sınırlı bir süre için bitiş noktaları onaylı IP adresleriyle sınırlamak üzere Azure Güvenlik Merkezi 'ni tam zamanında ağ erişimi ile kullanın.

Bağlantı noktalarını ve kaynak IP 'Leri gerçek trafik ve tehdit zeka göre sınırlayan NSG yapılandırmalarının kullanılması için Azure Güvenlik Merkezi Uyarlamalı ağ sağlamlaştırma kullanın.

DDoS korumasını yapılandırma:

https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

Azure Güvenlik duvarını dağıtma:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama:

https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer

Azure Güvenlik Merkezi Uyarlamalı ağ sağlamlaştırma 'yi anlama:

https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening

Azure Güvenlik Merkezi 'Ni, tam zamanında ağ Access Control anlayın:

https://docs.microsoft.com/azure/security-center/security-center-just-in-time

## <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1,5 | 12,5, 15,8 | Müşteri |

Akış kayıtları oluşturmak için NSG akış günlüklerini bir depolama hesabına kaydedin. Anormal etkinlikleri araştırmak için gerekliyse, ağ Izleyicisi paket yakalamayı etkinleştirin.

NSG akış günlüklerini etkinleştirme:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Ağ Izleyicisi 'ni etkinleştirme:

https://docs.microsoft.com/azure/network-watcher/network-watcher-create

## <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.6 | 12,6, 12,7 | Müşteri |

Tehdit zekası etkinleştirilmiş ve kötü amaçlı ağ trafiği için &quot;uyarı ve reddetme&quot; üzere yapılandırılmış bir kuruluşun ağ sınırlarının her birinde Azure Güvenlik Duvarı 'Nı dağıtın.

Azure Güvenlik Duvarı 'Nı dağıtma: https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Azure Güvenlik Duvarı ile uyarıları yapılandırma: https://docs.microsoft.com/azure/firewall/threat-intel

## <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.7 | 12,9, 12,10 | Müşteri |

Güvenilen sertifikalar için HTTPS/SSL özellikli Web uygulamaları için Azure Application Gateway dağıtın.

Application Gateway dağıtma:

https://docs.microsoft.com/azure/application-gateway/quick-create-portal

Application Gateway HTTPS kullanacak şekilde yapılandırma:

https://docs.microsoft.com/azure/application-gateway/create-ssl-portal

Azure Web uygulaması ağ geçitleri ile katman 7 yük dengelemesini anlayın:

https://docs.microsoft.com/azure/application-gateway/overview

## <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1,8 | 1,5 | Müşteri |

Ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örn., Apimanaya) belirterek, ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

Hizmet etiketlerini anlayın ve kullanın:

https://docs.microsoft.com/azure/virtual-network/service-tags-overview

## <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.9 | 11,1 | Müşteri |

Azure Ilkesiyle ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın.

Ayrıca, tek bir şema tanımında Azure Resource Manager şablonları, RBAC denetimleri ve ilkeleri gibi anahtar ortam yapıtlarını paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanabilirsiniz. Şema ' i yeni aboneliklere uygulayabilir ve sürüm oluşturma aracılığıyla denetim ve yönetime ince ayar yapabilirsiniz.

Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Ağ için Azure Ilke örnekleri:

https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#network

Azure Blueprint oluşturma:

https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

## <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1.1 | 11.2 | Müşteri |

NSG 'ler ve ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için Etiketler kullanın. Bireysel NSG kuralları için, bir ağa/veya ağ üzerinden gelen trafiğe izin veren tüm kurallar için iş gereksinimini ve/veya süre (vb.) belirtmek üzere &quot;Description&quot; alanını kullanın.

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Sanal ağ oluşturma:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Güvenlik Yapılandırması ile NSG oluşturma:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

## <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 1,11 | 11,3 | Müşteri |

Ağ kaynakları için yapılandırmayı doğrulamak (ve/veya düzeltmek) için Azure Ilkesini kullanın.

Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Ağ için Azure Ilke örnekleri:

https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#network

## <a name="next-steps"></a>Sonraki adımlar

- Sonraki güvenlik denetimine bakın: [günlüğe kaydetme ve izleme](security-control-logging-monitoring.md)
