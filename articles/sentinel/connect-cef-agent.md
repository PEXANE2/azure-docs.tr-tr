---
title: CEF verilerini Azure Sentinel 'e bağlamak için günlük ileticisini dağıtın | Microsoft Docs
description: CEF verilerini Azure Sentinel 'e bağlamak için aracıyı dağıtmayı öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2020
ms.author: yelevin
ms.openlocfilehash: 502fbe3bc7b1de2038bc444ae5daf180cfc80203
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85298999"
---
# <a name="step-1-deploy-the-log-forwarder"></a>1. Adım: günlük ileticisini dağıtma


Bu adımda, günlükleri güvenlik çözümünüzden Azure Sentinel çalışma alanınıza ileten Linux makinesini belirlersiniz ve yapılandıracaksınız. Bu makine, şirket içi ortamınızda, Azure VM 'de veya başka bir buluttaki bir sanal makinede bulunan fiziksel veya sanal bir makine olabilir. Belirtilen bağlantıyı kullanarak, belirlenen makinede aşağıdaki görevleri gerçekleştiren bir komut dosyası çalıştırırsınız:
- , Linux için Log Analytics aracısını (OMS Aracısı olarak da bilinir) yükleyerek aşağıdaki amaçlarla yapılandırır:
    - TCP bağlantı noktası 25226 ' deki yerleşik Linux Syslog arka plan programından CEF iletilerini dinleme
    - Bunların ayrıştırılabildiği ve zenginleştirdiği Azure Sentinel çalışma alanınıza TLS üzerinden güvenli bir şekilde gönderme

- Yerleşik Linux Syslog cinini (rsyslog. d/syslog-ng) aşağıdaki amaçlarla yapılandırır:
    - TCP bağlantı noktası 514 ' deki güvenlik çözümlerinizde syslog iletilerini dinleme
    - TCP bağlantı noktası 25226 kullanarak yalnızca, CEF olarak tanımladığı iletileri localhost üzerinde Log Analytics aracısına iletme
 
## <a name="prerequisites"></a>Ön koşullar

- Belirlenen Linux makinenizde yükseltilmiş izinleriniz (sudo) olmalıdır.
- Linux makinesinde Python yüklü olmalıdır.<br>`python -version`Denetlemek için komutunu kullanın.
- Log Analytics aracısını yüklemeden önce Linux makinenin herhangi bir Azure çalışma alanına bağlı olmaması gerekir.

## <a name="run-the-deployment-script"></a>Dağıtım betiğini çalıştırma
 
1. Azure Sentinel gezinti menüsünde, **veri bağlayıcıları**' na tıklayın. Bağlayıcılar listesinden **ortak olay biçimi (CEF)** kutucuğuna ve ardından sağ alt köşedeki **bağlayıcı sayfası aç** düğmesine tıklayın. 

1. **1,2 altına Linux MAKINESINE CEF toplayıcısı**'nı yüklemek için aşağıdaki betiği çalıştırın altında belirtilen bağlantıyı kopyalayın **ve CEF toplayıcısı 'nı yüklemek ve uygulamak için aşağıdaki**metni:

     `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_installer.py&&sudo python cef_installer.py [WorkspaceID] [Workspace Primary Key]`

1. Betik çalışırken, herhangi bir hata veya uyarı iletisi aldığınızdan emin olmak için kontrol edin.

2. [Adım: CEF iletilerini iletmek için Güvenlik çözümünüzü yapılandırma](connect-cef-solution-config.md) .

## <a name="deployment-script-explained"></a>Dağıtım betiği açıklanmıştı

Aşağıda dağıtım betiğinin eylemlerinin komut satırı açıklaması verilmiştir.

Uygun açıklamayı görmek için bir Syslog Daemon seçin.

# <a name="rsyslog-daemon"></a>[rsyslog Daemon](#tab/rsyslog)

1. **Log Analytics aracısını indirme ve yükleme:**

    - Log Analytics (OMS) Linux Aracısı için yükleme betiğini indirir<br>
        `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh`

    - Log Analytics aracısını yükleme<br>
        `sh onboard_agent.sh -w [workspaceID] -s [Primary Key] -d opinsights.azure.com`

1. **Syslog Daemon 'ı yapılandırma:**

    1. Syslog yapılandırma dosyasını kullanarak TCP iletişimi için 514 bağlantı noktasını açar `/etc/rsyslog.conf` .

    1. , Syslog Daemon dizinine özel bir yapılandırma dosyası ekleyerek CEF iletilerini TCP bağlantı noktası 25226 ' deki Log Analytics aracısına iletmek için arka plan programını yapılandırır `security-config-omsagent.conf` `/etc/rsyslog.d/` .

        `security-config-omsagent.conf`Dosyanın içeriği:

            :rawmsg, regex, "CEF"|"ASA"
            *.* @@127.0.0.1:25226

1. **Syslog Daemon 'u yeniden başlatma**

    `service rsyslog restart`

1. **25226 bağlantı noktasını dinlemek için Log Analytics Aracısı yapılandırmasını ayarlama ve CEF iletilerini Azure Sentinel 'e iletme**

    1. Log Analytics Agent GitHub deposundan yapılandırmayı indirir<br>
        `wget -o /etc/opt/microsoft/omsagent/[workspaceID]/conf/omsagent.d/security_events.conf https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/installer/conf/omsagent.d/security_events.conf`


    1. Log Analytics aracısını yeniden başlatır<br>
        `/opt/microsoft/omsagent/bin/service_control restart [workspaceID]`

# <a name="syslog-ng-daemon"></a>[Syslog-ng Daemon](#tab/syslogng)

1. **Log Analytics aracısını indirme ve yükleme:**

    - Log Analytics (OMS) Linux Aracısı için yükleme betiğini indirir<br>`wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh`

    - Log Analytics aracısını yükleme<br>`sh onboard_agent.sh -w [workspaceID] -s [Primary Key] -d opinsights.azure.com`

1. **Syslog Daemon 'ı yapılandırma:**

    1. Syslog yapılandırma dosyasını kullanarak TCP iletişimi için 514 bağlantı noktasını açar `/etc/syslog-ng/syslog-ng.conf` .

    1. , Syslog Daemon dizinine özel bir yapılandırma dosyası ekleyerek CEF iletilerini TCP bağlantı noktası 25226 ' deki Log Analytics aracısına iletmek için arka plan programını yapılandırır `security-config-omsagent.conf` `/etc/syslog-ng/conf.d/` .

        `security-config-omsagent.conf`Dosyanın içeriği:

            filter f_oms_filter {match(\"CEF\|ASA\" ) ;};
            destination oms_destination {tcp(\"127.0.0.1\" port("25226"));};
            log {source(s_src);filter(f_oms_filter);destination(oms_destination);};

1. **Syslog Daemon 'u yeniden başlatma**

    `service syslog-ng restart`

1. **25226 bağlantı noktasını dinlemek için Log Analytics Aracısı yapılandırmasını ayarlama ve CEF iletilerini Azure Sentinel 'e iletme**

    1. Log Analytics Agent GitHub deposundan yapılandırmayı indirir<br>
        `wget -o /etc/opt/microsoft/omsagent/[workspaceID]/conf/omsagent.d/security_events.conf https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/installer/conf/omsagent.d/security_events.conf`


    1. Log Analytics aracısını yeniden başlatır<br>
        `/opt/microsoft/omsagent/bin/service_control restart [workspaceID]`

---

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, CEF gereçlerini Azure Sentinel 'e bağlamak için Log Analytics aracısının nasıl dağıtılacağını öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.

