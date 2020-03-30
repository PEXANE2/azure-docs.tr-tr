---
title: Web Uygulaması Güvenlik Duvarı verilerini Azure Sentinel'e bağlama
description: Microsoft web uygulaması güvenlik duvarı verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
author: yelevin
manager: rkarlin
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: a5cef16694fa2cfae036152d22cfa4473956fc72
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588187"
---
# <a name="connect-data-from-microsoft-web-application-firewall"></a>Microsoft web uygulaması güvenlik duvarından veri bağlama



Azure Application Gateway'in Microsoft web uygulama güvenlik duvarından (WAF) günlükleri alabilirsiniz. Bu WAF, uygulamalarınızı SQL enjeksiyonu ve site ler arası komut dosyası komut dosyası gibi yaygın web güvenlik açıklarından korur ve yanlış pozitif leri azaltmak için kuralları özelleştirmenize olanak tanır. Microsoft Web uygulama güvenlik duvarı günlüklerinizi Azure Sentinel'e aktarmak için aşağıdaki yönergeleri izleyin.


## <a name="prerequisites"></a>Ön koşullar

- Varolan bir uygulama ağ geçidi kaynağı

## <a name="connect-to-microsoft-web-application-firewall"></a>Microsoft web uygulaması güvenlik duvarına bağlanın

Microsoft web uygulaması güvenlik duvarınız zaten varsa, varolan bir ağ geçidi kaynağınız olduğundan emin olun.
Microsoft web uygulaması güvenlik duvarınız dağıtıldıktan ve veri aldıktan sonra uyarı verileri kolayca Azure Sentinel'e aktarılabilir.
    
1. Azure Sentinel portalında **Veri bağlayıcılarını**seçin.
1. Veri bağlayıcıları sayfasında **WAF** döşemesini seçin.
1. Uygulama [Ağ Geçidi kaynağına](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAllResourcesBlade/resourceType/Microsoft.Network%2FapplicationGateways) gidin ve WAF'ınızı seçin.
    1. **Tanılama ayarlarını**seçin.
    1. + Tablonun altına **tanı ayarını ekle'yi** seçin.
    1. **Tanılama ayarları** sayfasında bir **Ad** yazın ve **Günlük Analitiğine Gönder'i**seçin.
    1. **Log Analytics Workspace** altında Azure Sentinel çalışma alanını seçin.
    1. Çözümlemek istediğiniz günlük türlerini seçin. Biz önerilir: ApplicationGatewayAccessLog ve ApplicationGatewayFirewallLog.
1. Microsoft web uygulaması güvenlik duvarı uyarıları için Log Analytics'teki ilgili şemayı kullanmak için **AzureDiagnostics'i**arayın.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Microsoft web uygulaması güvenlik duvarını Azure Sentinel' e nasıl bağlayacağını ünü öğrendim. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
