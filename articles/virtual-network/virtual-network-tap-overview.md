---
title: Azure sanal ağ TAP'a genel bakış | Microsoft Dokümanlar
description: Sanal ağ TAP hakkında bilgi edinin. Sanal ağ TAP, bir paket toplayıcısına aktarılabilen sanal makine ağı trafiğinin derin bir kopyasını sağlar.
services: virtual-network
documentationcenter: na
author: karthikananth
manager: ganesr
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/14/2019
ms.author: kaanan
ms.openlocfilehash: 47db03460ad3c5194a5445f0b25cb8e742e60c21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279617"
---
# <a name="virtual-network-tap"></a>Sanal ağ TAP

Azure sanal ağ TAP (Terminal Erişim Noktası), sanal makine ağ trafiğinizi sürekli olarak bir ağ paketi toplayıcısına veya analiz aracına aktarmanızı sağlar. Toplayıcı veya analiz aracı bir [ağ sanal cihaz](https://azure.microsoft.com/solutions/network-appliances/) ortağı tarafından sağlanır. Sanal ağ TAP ile çalışmak üzere doğrulanmış iş ortağı çözümlerinin listesi için [iş ortağı çözümlerine](#virtual-network-tap-partner-solutions)bakın.

> [!IMPORTANT]
> Sanal ağ TAP şu anda tüm Azure bölgelerinde önizlemededir. Sanal ağ TAP'ı kullanmak için, abonelik kimliğinizle <azurevnettap@microsoft.com> birlikte bir e-posta göndererek önizlemeye kaydolmalısınız. Aboneliğiniz kaydedildiğinde siz de bir e-posta alırsınız. Bir onay e-postası alana kadar bu özelliği kullanamazsınız. Bu önizleme, hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar [için Microsoft Azure Önizlemeleri](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) için Ek Kullanım Koşulları'na bakın.

## <a name="virtual-network-tap-partner-solutions"></a>Sanal ağ TAP iş ortağı çözümleri

### <a name="network-packet-brokers"></a>Ağ paketi brokerları

- [Büyük Anahtar Büyük İzleme Kumaşı](https://www.bigswitch.com/products/big-monitoring-fabric/public-cloud/microsoft-azure)
- [Gigamon GigaSECURE](https://blog.gigamon.com/2018/09/13/why-microsofts-new-vtap-service-works-even-better-with-gigasecure-for-azure)
- [Ixia CloudLens](https://www.ixiacom.com/cloudlens/cloudlens-azure)
- [Nubeva Prizmaları](https://www.nubeva.com/azurevtap)

### <a name="security-analytics-networkapplication-performance-management"></a>Güvenlik analitiği, ağ/uygulama performans yönetimi

- [Uyanık Güvenlik](https://awakesecurity.com/technology-partners/microsoft-azure/)
- [Cisco Stealthwatch Bulut](https://blogs.cisco.com/security/cisco-stealthwatch-cloud-and-microsoft-azure-reliable-cloud-infrastructure-meets-comprehensive-cloud-security)
- [Karanlık eser](https://www.darktrace.com/en/azure/)
- [ExtraHop Reveal(x)](https://www.extrahop.com/partners/tech-partners/microsoft/)
- [Fidelis Siber Güvenlik](https://www.fidelissecurity.com/technology-partners/microsoft-azure )
- [Flowmon](https://www.flowmon.com/blog/azure-vtap)
- [NetFort LANGuardian](https://www.netfort.com/languardian/solutions/visibility-in-azure-network-tap/)
- [Netscout vSTREAM]( https://www.netscout.com/technology-partners/microsoft/azure-vtap)
- [Riverbed SteelCentral AppResponse]( https://www.riverbed.com/products/steelcentral/steelcentral-appresponse-11.html)
- [RSA NetWitness® Platformu](https://www.rsa.com/azure)
- [Vectra Cognito](https://vectra.ai/microsoftazure)

Aşağıdaki resim, sanal ağ TAP'ın nasıl çalıştığını gösterir. Sanal ağınızda dağıtılan sanal bir makineye bağlı bir [ağ arabirimine](virtual-network-network-interface.md) TAP yapılandırması ekleyebilirsiniz. Hedef, izlenen ağ arabirimi veya [eşlenen sanal](virtual-network-peering-overview.md) ağ ile aynı sanal ağdaki sanal ağ IP adresidir. Sanal ağ TAP için toplayıcı çözümü, yüksek kullanılabilirlik için bir Azure Dahili Yük dengeleyicisinin arkasına dağıtılabilir. Tek tek çözüm için dağıtım seçeneklerini değerlendirmek için [iş ortağı çözümlerine](#virtual-network-tap-partner-solutions)bakın.

![Sanal ağ TAP nasıl çalışır?](./media/virtual-network-tap/architecture.png)

## <a name="prerequisites"></a>Ön koşullar

Sanal ağ TAP oluşturmadan önce, önizlemeye kaydolduğunuza dair bir onay postası almış ve Azure [Kaynak Yöneticisi](../azure-resource-manager/management/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) dağıtım modeli ve aynı azure bölgesinde TAP trafiğini bir araya getiren bir iş ortağı çözümü kullanılarak oluşturulmuş bir veya daha fazla sanal makineye sahip olmalısınız. Sanal ağınızda bir iş ortağı çözümünüz yoksa, dağıtmak için [iş ortağı çözümlerine](#virtual-network-tap-partner-solutions) bakın. Aynı veya farklı aboneliklerde birden çok ağ arabiriminden gelen trafiği toplamak için aynı sanal ağ TAP kaynağını kullanabilirsiniz. İzlenen ağ arabirimleri farklı aboneliklerdeyse, aboneliklerin aynı Azure Etkin Dizin kiracıyla ilişkilendirilmesi gerekir. Ayrıca, izlenen ağ arabirimleri ve TAP trafiğini toplama hedef bitiş noktası aynı bölgedeki eşlenmiş sanal ağlarda bulunabilir. Bu dağıtım modelini kullanıyorsanız, sanal ağ TAP'ı yapılandırmadan önce [sanal ağ eşlemesinin](virtual-network-peering-overview.md) etkin olduğundan emin olun.

## <a name="permissions"></a>İzinler

Ağ arabirimlerinde TAP yapılandırmasını uygulamak için kullandığınız hesapların [ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolüne veya aşağıdaki tablodan gerekli eylemleri atanan özel bir [role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanması gerekir:

| Eylem | Adı |
|---|---|
| Microsoft.Network/virtualNetworkTaps/* | Sanal ağ TAP kaynağı oluşturmak, güncellemek, okumak ve silmek için gereklidir |
| Microsoft.Network/networkInterfaces/read | TAP'ın yapılandırılacak ağ arabirimi kaynağını okumak için gerekli |
| Microsoft.Network/tapConfigurations/* | Ağ arabiriminde TAP yapılandırmasını oluşturmak, güncellemek, okumak ve silmek için gereklidir |

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal ağ TAP'ı nasıl oluşturup oluşturabilirsiniz](tutorial-tap-virtual-network-cli.md)öğrenin.
