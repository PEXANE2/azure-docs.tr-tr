---
title: Azure Web Uygulaması Güvenlik Duvarına Giriş
description: Bu makale, Azure Web Uygulaması Güvenlik Duvarı'na (WAF) genel bir bakış sağlar
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 03/18/2020
ms.author: victorh
ms.topic: overview
ms.openlocfilehash: 49024c86d09f5cdd9e8b04d5a49f60021660b0c4
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79475796"
---
# <a name="what-is-azure-web-application-firewall"></a>Azure Web Uygulaması Güvenlik Duvarı nedir?

Web Uygulaması Güvenlik Duvarı (WAF) web uygulamalarınız için yaygın güvenlik açıklarına ve açıklardan yararlanma işlemlerine karşı merkezi bir koruma sağlar. Web uygulamaları, yaygın olarak bilinen güvenlik açıklarından yararlanan kötü amaçlı saldırılar tarafından giderek daha fazla hedef alınır. SQL enjeksiyonve çapraz site komut dosyası en yaygın saldırılar arasında yer alıyor.

![WAF'a genel bakış](media/overview/wafoverview.png)

Uygulama kodunda bu tür saldırıları önlemek zordur. Uygulama topolojisinin birden fazla katmanında sıkı bakım, yama ve izleme gerektirebilir. Merkezi bir web uygulaması güvenlik duvarı, güvenlik yönetimini çok daha basit hale getirmeye yardımcı olur. WAF ayrıca uygulama yöneticilerine tehditlere ve izinsiz girişlere karşı daha iyi koruma güvencesi sağlar.

WAF çözümü, her bir web uygulamasını güvence altına almak yerine bilinen bir güvenlik açığını merkezi olarak yamalayarak bir güvenlik tehdidine daha hızlı tepki verebilir.

## <a name="supported-service"></a>Desteklenen hizmet

WAF, Microsoft'un Azure Uygulama Ağ Geçidi, Azure Ön Kapı ve Azure İçerik Dağıtım Ağı (CDN) hizmetiyle dağıtılabilir. Azure CDN'deki WAF şu anda genel önizleme altında.  WAF, her bir hizmet için özelleştirilmiş özelliklere sahiptir. Her hizmet için WAF özellikleri hakkında daha fazla bilgi için her hizmete genel bakış alameti görün.

## <a name="next-steps"></a>Sonraki adımlar

- Uygulama Ağ Geçidi'ndeki Web Uygulaması Güvenlik Duvarı hakkında daha fazla bilgi için [Azure Uygulama Ağ Geçidi'ndeki Web Uygulaması Güvenlik Duvarı'na](./ag/ag-overview.md)bakın.
- Azure Ön Kapı Hizmeti'nde Web Uygulaması Güvenlik Duvarı hakkında daha fazla bilgi için [Azure Ön Kapı Hizmeti'nde Web Uygulaması Güvenlik Duvarı'na](./afds/afds-overview.md)bakın.
- Azure CDN Hizmeti'nde Web Uygulaması Güvenlik Duvarı hakkında daha fazla bilgi için [Azure CDN Hizmeti'nde Web Uygulaması Güvenlik Duvarı'na](./cdn/cdn-overview.md) bakın
