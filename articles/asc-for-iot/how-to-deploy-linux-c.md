---
title: Linux C Aracısı 'nı Install & Deploy
description: IoT Aracısı için Azure Güvenlik Merkezi 'ni 32-bit ve 64 bit Linux 'ta yüklemeyi öğrenin.
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
ms.openlocfilehash: d9f9602a19a266c70b17422e90566f72de2978f6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81311201"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Linux için IoT için Azure Güvenlik Merkezi C tabanlı güvenlik aracısını dağıtma

Bu kılavuzda, Linux üzerinde IoT C tabanlı güvenlik aracısına yönelik Azure Güvenlik Merkezi 'nin nasıl yükleneceği ve dağıtılacağı açıklanmaktadır.

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Yükleme
> * Dağıtımı doğrulama
> * Aracıyı kaldırın
> * Sorun giderme

## <a name="prerequisites"></a>Ön koşullar

Diğer platformlar ve aracı türleri için bkz. [doğru güvenlik aracısını seçme](how-to-deploy-agent.md).

1. Güvenlik aracısını dağıtmak için, yüklemek istediğiniz makinede (sudo) yerel yönetici hakları gereklidir.

1. Cihaz için [bir güvenlik modülü oluşturun](quickstart-create-security-twin.md) .

## <a name="installation"></a>Yükleme

Güvenlik aracısını yüklemek ve dağıtmak için aşağıdaki iş akışını kullanın:

1. [GitHub](https://aka.ms/iot-security-github-c)'dan makinenize en son sürümü indirin.

1. Paketin içeriğini ayıklayın ve _/src/installation_ klasörüne gidin.

1. Aşağıdaki komutu çalıştırarak **ınstallsecurityagent betiğine** çalışan izinleri ekleyin:

   ```
   chmod +x InstallSecurityAgent.sh
   ```

1. Sonra, şunu çalıştırın:

   ```
   ./InstallSecurityAgent.sh -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -i
   ```

   Kimlik doğrulama parametreleri hakkında daha fazla bilgi için bkz. [kimlik doğrulamasını yapılandırma](concept-security-agent-authentication-methods.md) .

Bu betik aşağıdaki işlevi gerçekleştirir:

1. Önkoşulları kurar.

1. Bir hizmet kullanıcısı ekler (etkileşimli oturum açma devre dışı).

1. Aracıyı bir **Daemon** olarak yükleme-cihazın hizmet yönetimi için **systemd** 'yi kullandığını varsayar.

1. Aracıyı, belirtilen kimlik doğrulama parametreleriyle yapılandırır.

Ek Yardım için, – help parametresiyle betiği çalıştırın:

```./InstallSecurityAgent.sh --help```

### <a name="uninstall-the-agent"></a>Aracıyı kaldırın

Aracıyı kaldırmak için--Uninstall parametresiyle betiği çalıştırın:

```./InstallSecurityAgent.sh -–uninstall```

## <a name="troubleshooting"></a>Sorun giderme

Aşağıdakileri çalıştırarak dağıtım durumunu denetleyin:

```systemctl status ASCIoTAgent.service```

## <a name="next-steps"></a>Sonraki adımlar

- IoT hizmetine [genel bakış](overview.md) Için Azure Güvenlik Merkezi 'ni okuyun
- IoT [mimarisi](architecture.md) Için Azure Güvenlik Merkezi hakkında daha fazla bilgi edinin
- [Hizmeti](quickstart-onboard-iot-hub.md) etkinleştirme
- [SSS](resources-frequently-asked-questions.md) 'yi okuyun
- [Güvenlik uyarılarını](concept-security-alerts.md) anlama
