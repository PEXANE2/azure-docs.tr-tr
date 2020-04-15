---
title: Linux C# aracısı & yükleyin
description: IoT aracısı için Azure Güvenlik Merkezi'ni hem 32 bit hem de 64 bit Linux'a nasıl yükleyin öğrenin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: b0982203-c3c8-4a0b-8717-5b5ac4038d8c
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2019
ms.author: mlottner
ms.openlocfilehash: 40c6ea91fd84a0f088ed770cd7c4c3ea7b8b1c91
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81311137"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Linux için IoT için Azure Güvenlik Merkezi C# tabanlı güvenlik aracısını dağıtma

Bu kılavuz, IoT C# tabanlı güvenlik aracısı için Azure Güvenlik Merkezi'nin Linux'a nasıl yüklenir ve dağıtılanın.

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Yükleme
> * Dağıtımı doğrulama
> * Aracıyı kaldırın
> * Sorun giderme

## <a name="prerequisites"></a>Ön koşullar

Diğer platformlar ve aracı tatları için [bkz.](how-to-deploy-agent.md)

1. Güvenlik aracısını dağıtmak için, yüklemek istediğiniz makinede yerel yönetici hakları gereklidir.

1. Aygıt için [bir güvenlik modülü oluşturun.](quickstart-create-security-twin.md)

## <a name="installation"></a>Yükleme

Güvenlik aracısını dağıtmak için aşağıdaki adımları kullanın:

1. [GitHub'dan](https://aka.ms/iot-security-github-cs)makinenize en son sürümü indirin.

1. Paketin içeriğini ayıklayın ve _/Install_ klasörüne gidin.

1. Çalıştırarak **InstallSecurityAgent komut dosyasına** çalışan izinler ekleme`chmod +x InstallSecurityAgent.sh`

1. Ardından, **kök ayrıcalıkları**ile aşağıdaki komutu çalıştırın:

   ```
   ./InstallSecurityAgent.sh -i -aui <authentication identity>  -aum <authentication method> -f <file path> -hn <host name>  -di <device id> -cl <certificate location kind>
   ```

   kimlik doğrulama parametreleri hakkında daha fazla bilgi için kimlik [doğrulamasını nasıl yapılandırılatırınız.](concept-security-agent-authentication-methods.md)

Bu komut dosyası aşağıdaki eylemleri gerçekleştirir:

- Ön koşulları yükler.

- Bir hizmet kullanıcısı ekler (etkileşimli oturum açma devre dışı).

- Aracıyı **Daemon** olarak yükler - aygıtın klasik dağıtım modeli için **sistemli** kullandığını varsayar.

- Aracının belirli görevleri kök olarak yapmasına izin verecek şekilde **sudoerleri** yapılandırır.

- Aracıyı sağlanan kimlik doğrulama parametreleriyle yapılandırır.

Ek yardım için komut dosyasını –yardım parametresi ile çalıştırın:`./InstallSecurityAgent.sh --help`

### <a name="uninstall-the-agent"></a>Aracıyı kaldırın

Aracıyı kaldırmak için komut dosyasını –u parametresi ile çalıştırın: `./InstallSecurityAgent.sh -u`.

> [!NOTE]
> Kaldırma yükleme sırasında yüklenen eksik ön koşulları kaldırmaz.

## <a name="troubleshooting"></a>Sorun giderme

1. Çalıştırarak dağıtım durumunu denetleyin:

    `systemctl status ASCIoTAgent.service`

1. Günlüğe kaydetmeyi etkinleştirin.
   Aracı başlatamazsa, daha fazla bilgi almak için günlüğe kaydetmeyi açın.

   Günlüğe kaydetmeyi açın:

   1. Yapılandırma dosyasını herhangi bir Linux düzenleyicisinde düzenleme için açın:

        `vi /var/ASCIoTAgent/General.config`

   1. Aşağıdaki değerleri edin:

      ```
      <add key="logLevel" value="Debug"/>
      <add key="fileLogLevel" value="Debug"/>
      <add key="diagnosticVerbosityLevel" value="Some" />
      <add key="logFilePath" value="IotAgentLog.log"/>
      ```

       **logFilePath** değeri yapılandırılabilir.

       > [!NOTE]
       > Sorun giderme tamamlandıktan sonra oturumu **kapatmanızı** öneririz. **Oturum** açma, günlük dosya boyutunu ve veri kullanımını artırır.

   1. Çalıştırarak aracıyı yeniden başlatın:

       `systemctl restart ASCIoTAgent.service`

   1. Hata hakkında daha fazla bilgi için günlük dosyasını görüntüleyin.

       Günlük dosya konumu:`/var/ASCIoTAgent/IotAgentLog.log`

       Dosya konumu yolunu, 2. **logFilePath**

## <a name="next-steps"></a>Sonraki adımlar

- IoT hizmetine [Genel Bakış](overview.md) için Azure Güvenlik Merkezi'ni okuyun
- IoT [Mimarisi](architecture.md) için Azure Güvenlik Merkezi hakkında daha fazla bilgi edinin
- [Hizmeti](quickstart-onboard-iot-hub.md) etkinleştirme
- [SSS'yi](resources-frequently-asked-questions.md) okuyun
- [Uyarıları](concept-security-alerts.md) anlama
