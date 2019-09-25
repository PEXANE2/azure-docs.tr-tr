---
title: Microsoft Web uygulaması güvenlik duvarı verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Microsoft Web uygulaması güvenlik duvarı verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 9d85b5a72c2e37719348d61250d167eb9a5688a1
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240036"
---
# <a name="connect-data-from-microsoft-web-application-firewall"></a>Microsoft Web uygulaması güvenlik duvarındaki verileri bağlama



Azure Application Gateway Microsoft Web uygulaması güvenlik duvarı 'ndan (WAF) günlükleri akışla aktarabilirsiniz. Bu WAF, uygulamalarınızı SQL ekleme ve siteler arası betik oluşturma gibi yaygın web açıklarına karşı korur ve hatalı pozitif sonuçları azaltmak için kuralları özelleştirmenizi sağlar. Microsoft Web uygulaması güvenlik duvarı günlüklerinizi Azure Sentinel 'e aktarmak için bu yönergeleri izleyin.


## <a name="prerequisites"></a>Önkoşullar

- Mevcut bir Application Gateway kaynağı

## <a name="connect-to-microsoft-web-application-firewall"></a>Microsoft Web uygulaması güvenlik duvarı 'na Bağlan

Zaten Microsoft Web uygulaması güvenlik duvarınız varsa, mevcut bir ağ geçidi kaynağınız olduğundan emin olun.
Microsoft Web uygulaması güvenlik duvarınız dağıtıldıktan ve veriler alındıktan sonra, uyarı verileri kolayca Azure Sentinel 'e akışla eklenebilir.
    
1. Azure Sentinel portalında **veri bağlayıcıları**' nı seçin.
1. Veri bağlayıcıları sayfasında **WAF** kutucuğunu seçin.
1. [Application Gateway kaynağına](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAllResourcesBlade/resourceType/Microsoft.Network%2FapplicationGateways) gidin ve WAF 'nizi seçin.
    1.  **Tanılama ayarları**' nı seçin.
    1. Tablo altında **+ Tanılama ayarı Ekle** ' yi seçin.
    1.  **Tanılama ayarları** sayfasında, bir **ad** yazın ve **Log Analytics gönder**' i seçin.
    1. **Log Analytics çalışma alanı** altında Azure Sentinel çalışma alanını seçin.
    1. Çözümlemek istediğiniz günlük türlerini seçin. Önerdiğimiz: ApplicationGatewayAccessLog ve ApplicationGatewayFirewallLog.
1. Microsoft Web uygulaması güvenlik duvarı uyarıları için Log Analytics ilgili şemayı kullanmak için, **AzureDiagnostics**aratın.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Microsoft Web uygulaması güvenlik duvarını Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
