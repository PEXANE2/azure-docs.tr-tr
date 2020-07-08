---
title: Linux C# aracısını yükler & dağıtın
description: IoT Aracısı için Azure Güvenlik Merkezi 'ni 32-bit ve 64 bit Linux 'ta yüklemeyi öğrenin.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81311137"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Linux için IoT için Azure Güvenlik Merkezi C# tabanlı güvenlik aracısını dağıtma

Bu kılavuzda, Linux üzerinde IoT C# tabanlı güvenlik aracısına yönelik Azure Güvenlik Merkezi 'nin nasıl yükleneceği ve dağıtılacağı açıklanmaktadır.

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Yükleme
> * Dağıtımı doğrulama
> * Aracıyı kaldırın
> * Sorun giderme

## <a name="prerequisites"></a>Ön koşullar

Diğer platformlar ve aracı türleri için bkz. [doğru güvenlik aracısını seçme](how-to-deploy-agent.md).

1. Güvenlik aracısını dağıtmak için, yüklemek istediğiniz makinede yerel yönetici hakları gereklidir.

1. Cihaz için [bir güvenlik modülü oluşturun](quickstart-create-security-twin.md) .

## <a name="installation"></a>Yükleme

Güvenlik aracısını dağıtmak için aşağıdaki adımları kullanın:

1. [GitHub](https://aka.ms/iot-security-github-cs)'dan makinenize en son sürümü indirin.

1. Paketin içeriğini ayıklayın ve _/install_ klasörüne gidin.

1. Çalıştırılarak **ınstallsecurityagent betiğine** çalışan izinleri ekleme`chmod +x InstallSecurityAgent.sh`

1. Ardından, **kök ayrıcalıklarıyla**aşağıdaki komutu çalıştırın:

   ```
   ./InstallSecurityAgent.sh -i -aui <authentication identity>  -aum <authentication method> -f <file path> -hn <host name>  -di <device id> -cl <certificate location kind>
   ```

   kimlik doğrulama parametreleri hakkında daha fazla bilgi için bkz. [kimlik doğrulamasını yapılandırma](concept-security-agent-authentication-methods.md).

Bu betik aşağıdaki eylemleri gerçekleştirir:

- Önkoşulları kurar.

- Bir hizmet kullanıcısı ekler (etkileşimli oturum açma devre dışı).

- Aracıyı bir **Daemon** olarak yükleme-cihazın klasik dağıtım modeli için **systemd** kullandığını varsayar.

- Aracının belirli görevleri kök olarak yapmasına izin vermek için **sudocıları** yapılandırır.

- Aracıyı, belirtilen kimlik doğrulama parametreleriyle yapılandırır.

Ek Yardım için, – help parametresiyle betiği çalıştırın:`./InstallSecurityAgent.sh --help`

### <a name="uninstall-the-agent"></a>Aracıyı kaldırın

Aracıyı kaldırmak için betiği – u parametresiyle çalıştırın: `./InstallSecurityAgent.sh -u` .

> [!NOTE]
> Kaldırma, yükleme sırasında yüklenmiş eksik önkoşulları kaldırmaz.

## <a name="troubleshooting"></a>Sorun giderme

1. Aşağıdakileri çalıştırarak dağıtım durumunu denetleyin:

    `systemctl status ASCIoTAgent.service`

1. Günlüğe kaydetmeyi etkinleştirin.
   Aracı başlatılamazsa, daha fazla bilgi almak için günlüğü açın.

   Günlük kaydını açmak için:

   1. Yapılandırma dosyasını herhangi bir Linux düzenleyicisinde düzenleme için açın:

        `vi /var/ASCIoTAgent/General.config`

   1. Aşağıdaki değerleri düzenleyin:

      ```
      <add key="logLevel" value="Debug"/>
      <add key="fileLogLevel" value="Debug"/>
      <add key="diagnosticVerbosityLevel" value="Some" />
      <add key="logFilePath" value="IotAgentLog.log"/>
      ```

       **LogFilePath** değeri yapılandırılabilir.

       > [!NOTE]
       > Sorun giderme işlemi tamamlandıktan sonra oturumu **kapatmayı** öneririz. Günlüğe kaydetmeyi **bırakmak günlük dosyası** boyutunu ve veri kullanımını artırır.

   1. Şunu çalıştırarak aracıyı yeniden başlatın:

       `systemctl restart ASCIoTAgent.service`

   1. Hata hakkında daha fazla bilgi için günlük dosyasını görüntüleyin.

       Günlük dosyası konumu:`/var/ASCIoTAgent/IotAgentLog.log`

       Dosya konumu yolunu, 2. adımdaki **LogFilePath** için seçtiğiniz ada göre değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

- IoT hizmetine [genel bakış](overview.md) Için Azure Güvenlik Merkezi 'ni okuyun
- IoT [mimarisi](architecture.md) Için Azure Güvenlik Merkezi hakkında daha fazla bilgi edinin
- [Hizmeti](quickstart-onboard-iot-hub.md) etkinleştirme
- [SSS](resources-frequently-asked-questions.md) 'yi okuyun
- [Uyarıları](concept-security-alerts.md) anlama
