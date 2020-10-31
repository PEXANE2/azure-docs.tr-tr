---
title: Citrix WAF verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Citrix WAF veri bağlayıcısını kullanarak günlüklerini Azure Sentinel 'e çekmek için nasıl kullanacağınızı öğrenin. Çalışma kitaplarında Citrix WAF verilerini görüntüleyin, uyarılar oluşturun ve araştırmayı geliştirebilirsiniz.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0001cad6-699c-4ca9-b66c-80c194e439a5
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/25/2020
ms.author: yelevin
ms.openlocfilehash: 475e04ad3dd8c7cc51d6345b4b51dd68cff5b597
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93103129"
---
# <a name="connect-your-citrix-waf-to-azure-sentinel"></a>Citrix WAF 'nizi Azure Sentinel 'e bağlama

> [!IMPORTANT]
> Azure Sentinel 'deki Citrix Web uygulaması güvenlik duvarı (WAF) veri Bağlayıcısı Şu anda genel önizlemededir. Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sunulmaktadır. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, Citrix Web uygulaması güvenlik duvarı (WAF) gerecinizi Azure Sentinel 'e nasıl bağlayabileceğiniz açıklanır. Citrix WAF veri Bağlayıcısı, Azure Sentinel ile Citrix WAF günlüklerinizi kolayca bağlamanıza, panoları görüntülemenize, özel uyarılar oluşturmanıza ve araştırmaya olanak tanır. Citrix WAF CEF günlüklerini Azure Sentinel 'e bağlayarak, her günlük için arama ve bağıntı, uyarı ve tehdit zekası zenginleştirme özelliğinden yararlanabilirsiniz.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="forward-citrix-waf-logs-to-the-syslog-agent"></a>Citrix WAF günlüklerini Syslog aracısına ilet  

Citrix WAF, günlükleri Azure Sentinel çalışma alanınıza ileten Log Analytics aracı ile bir Linux tabanlı günlük iletme sunucusuna (rsyslog veya Syslog-ng çalıştıran), CEF biçiminde Syslog iletileri gönderir.

1. Bu tür bir günlük iletme sunucunuz yoksa, bir tane çalışır duruma getirmek için [Bu yönergelere](connect-cef-agent.md) bakın.

1. [WAF 'yi yapılandırmak](https://support.citrix.com/article/CTX234174), [CEF günlüğünü yapılandırmak](https://support.citrix.com/article/CTX136146)ve [günlükleri günlük Ileticiye göndermeyi yapılandırmak](https://docs.citrix.com/en-us/citrix-adc/13/system/audit-logging/configuring-audit-logging.html)için Citrix 'in sağladığı yönergeleri izleyin. Günlükleri, günlük ileticisi makinesinin IP adresinde TCP bağlantı noktası 514 ' e gönderdiğinizden emin olun.

1. Bağlantınızı doğrulayın ve [Bu yönergeleri](connect-cef-verify.md)kullanarak veri alımını doğrulayın. Günlüklerinizin Log Analytics görünmeye başlaması 20 dakikaya kadar sürebilir.

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler **günlüklerde** , **Azure Sentinel** bölümünde, *commonsecuritylog* tablosunda görünür.

Citrix WAF günlüklerini Log Analytics sorgulamak için, `CommonSecurityLog` sorgu penceresinin üst kısmına girin.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Citrix WAF 'yi Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .