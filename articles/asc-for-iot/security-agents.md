---
title: IoT güvenlik aracıları için Azure Güvenlik Merkezi'ni kullanmaya başlayın| Microsoft Dokümanlar
description: IoT aygıtlarınızda IoT güvenlik hizmeti aracıları için Azure Güvenlik Merkezi'ni anlamaya, yapılandırmaya, dağıtmaya ve kullanmaya başlayın.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 55c8d3b6-3126-4246-8d07-ef88fe5ea84f
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 1ed890d9d3602de24e6f85f6f0ae7f59849f3df2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74664206"
---
# <a name="get-started-with-azure-security-center-for-iot-device-security-agents"></a>IoT aygıt güvenlik aracıları için Azure Güvenlik Merkezi ile başlayın

Azure Güvenlik Merkezi ioT güvenlik aracıları için uzak bağlantıları, etkin uygulamaları, oturum açma olaylarını ve işletim sistemi yapılandırması en iyi uygulamaları izleme gibi gelişmiş güvenlik özellikleri sunar. Tek bir hizmetle cihazınızın alan tehdit koruması ve güvenlik duruşunu kontrol altına alın. 

C# ve C'de hem Linux hem de Windows güvenlik aracıları için referans mimarisi sağlanır.

IoT güvenlik aracıları için Azure Güvenlik Merkezi, aygıt işletim sisteminden ham olay toplama, maliyeti azaltmak için olay toplama ve aygıt modülü ikizi aracılığıyla yapılandırma işlemlerini gerçekleştirir. Güvenlik iletileri IoT Hub'ınız aracılığıyla Azure Güvenlik Merkezi'ne IoT analiz hizmetleri için gönderilir.

IoT güvenlik aracıları için Azure Güvenlik Merkezinizi dağıtmak ve test etmek için aşağıdaki iş akışını kullanın: 

1. [IoT Hub'ınıza Azure Güvenlik Merkezi'ni IoT hizmeti için etkinleştirme](quickstart-onboard-iot-hub.md)
1. IoT Hub'ınızda kayıtlı aygıt yoksa, [yeni bir aygıt kaydedin.](https://docs.microsoft.com/azure/iot-accelerators/quickstart-device-simulation-deploy)
1. Aygıtlarınız için [azureiotsecurity güvenlik modülü oluşturun.](quickstart-create-security-twin.md)
1. Aracıyı gerçek bir aygıta yüklemek yerine Azure simüle edilmiş bir aygıta yüklemek için kullanılabilir bir bölgede [yeni bir Azure Sanal Makine (VM) döndürün.](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) 
1. IoT aygıtınızda veya yeni VM'nizde [IoT güvenlik aracısı için bir Azure Güvenlik Merkezi dağıtın.](how-to-deploy-linux-cs.md)
1. Bir saldırının zararsız bir simülasyon çalıştırmak için [trigger_events](https://aka.ms/iot-security-github-trigger-events) için yönergeleri izleyin.
1. Önceki adımdaki benzetimli saldırıya yanıt olarak Azure Güvenlik Merkezi'ni IoT uyarıları için doğrulayın. Komut dosyasını çalıştırdıktan beş dakika sonra doğrulamaya başlayın.
1. IoT Hub'ı [kullanarak Log Analytics'i kullanarak](how-to-security-data-access.md) [uyarıları,](concept-security-alerts.md) [önerileri](concept-recommendations.md)ve derin dalışı keşfedin. 


## <a name="next-steps"></a>Sonraki adımlar

- [Çözümünüzü](quickstart-configure-your-solution.md) yapılandırın
- [Güvenlik modülleri oluşturma](quickstart-create-security-twin.md)
- Özel [uyarıları](quickstart-create-custom-alerts.md) yapılandırma
- [Bir güvenlik aracısı dağıtma](how-to-deploy-agent.md)
