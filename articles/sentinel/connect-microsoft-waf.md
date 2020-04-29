---
title: Web uygulaması güvenlik duvarı verilerini Azure Sentinel 'e bağlama
description: Microsoft Web uygulaması güvenlik duvarı verilerini Azure Sentinel 'e bağlamayı öğrenin.
author: yelevin
manager: rkarlin
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: a5cef16694fa2cfae036152d22cfa4473956fc72
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77588187"
---
# <a name="connect-data-from-microsoft-web-application-firewall"></a>Microsoft Web uygulaması güvenlik duvarındaki verileri bağlama



Azure Application Gateway Microsoft Web uygulaması güvenlik duvarı 'ndan (WAF) günlükleri akışla aktarabilirsiniz. Bu WAF, uygulamalarınızı SQL ekleme ve siteler arası betik oluşturma gibi yaygın web açıklarına karşı korur ve hatalı pozitif sonuçları azaltmak için kuralları özelleştirmenizi sağlar. Microsoft Web uygulaması güvenlik duvarı günlüklerinizi Azure Sentinel 'e aktarmak için bu yönergeleri izleyin.


## <a name="prerequisites"></a>Ön koşullar

- Mevcut bir Application Gateway kaynağı

## <a name="connect-to-microsoft-web-application-firewall"></a>Microsoft Web uygulaması güvenlik duvarı 'na Bağlan

Zaten Microsoft Web uygulaması güvenlik duvarınız varsa, mevcut bir ağ geçidi kaynağınız olduğundan emin olun.
Microsoft Web uygulaması güvenlik duvarınız dağıtıldıktan ve veriler alındıktan sonra, uyarı verileri kolayca Azure Sentinel 'e akışla eklenebilir.
    
1. Azure Sentinel portalında **veri bağlayıcıları**' nı seçin.
1. Veri bağlayıcıları sayfasında **WAF** kutucuğunu seçin.
1. [Application Gateway kaynağına](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAllResourcesBlade/resourceType/Microsoft.Network%2FapplicationGateways) gidin ve WAF 'nizi seçin.
    1. **Tanılama ayarları**' nı seçin.
    1. Tablo altında **+ Tanılama ayarı Ekle** ' yi seçin.
    1. **Tanılama ayarları** sayfasında, bir **ad** yazın ve **Log Analytics gönder**' i seçin.
    1. **Log Analytics çalışma alanı** altında Azure Sentinel çalışma alanını seçin.
    1. Çözümlemek istediğiniz günlük türlerini seçin. Şunları öneririz: ApplicationGatewayAccessLog ve ApplicationGatewayFirewallLog.
1. Microsoft Web uygulaması güvenlik duvarı uyarıları için Log Analytics ilgili şemayı kullanmak için, **AzureDiagnostics**aratın.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Microsoft Web uygulaması güvenlik duvarını Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
