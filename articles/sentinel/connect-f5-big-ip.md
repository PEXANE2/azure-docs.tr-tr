---
title: F5 BIG-IP verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: F5 BIG-IP verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: d068223f-395e-46d6-bb94-7ca1afd3503c
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: ae361c74b261bdd6a5673040f868392282b573ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588289"
---
# <a name="connect-your-f5-big-ip-appliance"></a>F5 BIG-IP cihazınızı bağlayın 

> [!IMPORTANT]
> Azure Sentinel'deki F5 BIG-IP veri bağlayıcısı şu anda genel önizlemededir.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

F5 BIG-IP konektörü, tüm F5 BIG-IP günlüklerinizi Azure Sentinel ile kolayca bağlamanızı, çalışma kitaplarını görüntülemenizi, özel uyarılar oluşturmanızı ve araştırmayı geliştirmenizi sağlar. Bu, kuruluşunuzun ağı hakkında daha fazla bilgi verir ve güvenlik işlem yeteneklerinizi geliştirir. F5 BIG-IP ve Azure Sentinel arasındaki tümleştirme, REST API'den yararlanır.


> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-f5-big-ip"></a>F5 BIG-IP'yi yapılandırma ve bağlama 

F5 BIG-IP günlükleri doğrudan Azure Sentinel'e entegre edebilir ve dışa aktarabilir.

1. Azure Sentinel portalında **Veri bağlayıcılarını** tıklatın ve **F5 BIG-IP'yi** seçin ve ardından **bağlayıcı sayfasını açın.** 
1. F5 BIG-IP'nizi bağlamak için sistemin API bitiş noktasına bir JSON bildirimi göndermeniz gerekir. Bunun nasıl yapılacağını gösteren talimatlar için [F5 BIG-IP'yi Azure Sentinel ile tümleştirme](https://devcentral.f5.com/s/articles/Integrating-the-F5-BIGIP-with-Azure-Sentinel)'ye bakın.
8. F5 BIG-IP bağlayıcı sısayfasından, Çalışma Alanı Kimliği ve Birincil Anahtarı kopyalayın ve Akış verileri altında belirtildiği gibi [Azure Log Analytics'e](https://devcentral.f5.com/s/articles/Integrating-the-F5-BIGIP-with-Azure-Sentinel#streaming-data-to-azure-log-analytics)yapıştırın.
1. F5 BIG-IP yönergelerini tamamladıktan sonra Azure Sentinel bağlayıcı sayfasında bağlı veri türlerini görürsünüz.
1. F5 BIG-IP etkinlikleri için Log Analytics'teki ilgili şemayı kullanmak **için, F5Telemetry_LTM_CL,** **F5Telemetry_system_CL**ve **F5Telemetry_ASM_CL**arama yapın.


## <a name="validate-connectivity"></a>Bağlantıyı doğrulama

Günlüklerinizin Log Analytics'te görünmeye başlaması 20 dakikadan fazla sürebilir. 



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, F5 BIG-IP'yi Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)


