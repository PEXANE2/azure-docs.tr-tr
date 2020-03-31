---
title: IoT aracısı için Azure Güvenlik Merkezi'nin Linux C aracısını yükleme ve dağıtma kılavuzu| Microsoft Dokümanlar
description: IoT aracısı için Azure Güvenlik Merkezi'ni hem 32 bit hem de 64 bit Linux'a nasıl yükleyin öğrenin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 3ccf2aec-106a-4d2c-8079-5f3e8f2afdcb
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 7578811c37cd0bbe47821dadacce5fa5974f56cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68812737"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Linux için IoT için Azure Güvenlik Merkezi C tabanlı güvenlik aracısını dağıtma

Bu kılavuz, IoT C tabanlı güvenlik aracısı için Azure Güvenlik Merkezi'nin Linux'a nasıl yüklenir ve dağıtılanın.

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz: 
> [!div class="checklist"]
> * Yükleme
> * Dağıtımı doğrulama
> * Aracıyı kaldırın
> * Sorun giderme 

## <a name="prerequisites"></a>Ön koşullar

Diğer platformlar ve aracı tatları için [bkz.](how-to-deploy-agent.md)

1. Güvenlik aracısını dağıtmak için, yüklemek istediğiniz makinede (sudo) yerel yönetici hakları gereklidir.

1. Aygıt için [bir güvenlik modülü oluşturun.](quickstart-create-security-twin.md)

## <a name="installation"></a>Yükleme 

Güvenlik aracısını yüklemek ve dağıtmak için aşağıdaki iş akışını kullanın:


1. [GitHub'dan](https://aka.ms/iot-security-github-c)makinenize en son sürümü indirin.

1. Paketin içeriğini ayıklayın ve _/src/installation_ klasörüne gidin.

1. Aşağıdaki komutu çalıştırarak **InstallSecurityAgent komut dosyasına** çalışan izinler ekleyin:
    
   ```
   chmod +x InstallSecurityAgent.sh
   ```

1. Sonra, çalıştırın: 

   ```
   ./InstallSecurityAgent.sh -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -i
   ```
   
   Kimlik doğrulama parametreleri hakkında daha fazla bilgi için [kimlik doğrulamayı nasıl yapılandırılabilirsiniz.](concept-security-agent-authentication-methods.md)

Bu komut dosyası aşağıdaki işlevi gerçekleştirir:

1. Ön koşulları yükler.

2. Bir hizmet kullanıcısı ekler (etkileşimli oturum açma devre dışı).

3. Aracıyı **Daemon** olarak yükler - aygıtın servis yönetimi için **sistemli** kullandığını varsayar.

4. Aracıyı sağlanan kimlik doğrulama parametreleriyle yapılandırır. 

Ek yardım için komut dosyasını –yardım parametresi ile çalıştırın: 
    
    ./InstallSecurityAgent.sh --help

### <a name="uninstall-the-agent"></a>Aracıyı kaldırın

Aracıyı kaldırmak için komut dosyasını –-kaldır parametresi ile çalıştırın:

    ./InstallSecurityAgent.sh -–uninstall

## <a name="troubleshooting"></a>Sorun giderme
Çalıştırarak dağıtım durumunu denetleyin:

    systemctl status ASCIoTAgent.service


## <a name="next-steps"></a>Sonraki adımlar
- IoT hizmetine [Genel Bakış](overview.md) için Azure Güvenlik Merkezi'ni okuyun
- IoT [Mimarisi](architecture.md) için Azure Güvenlik Merkezi hakkında daha fazla bilgi edinin
- [Hizmeti](quickstart-onboard-iot-hub.md) etkinleştirme
- [SSS'yi](resources-frequently-asked-questions.md) okuyun
- [Güvenlik uyarılarını](concept-security-alerts.md) anlama
