---
title: CEF verilerini Azure Sentinel'e bağlamak için günlük iletmesini dağıtma | Microsoft Dokümanlar
description: CEF verilerini Azure Sentinel'e bağlamak için aracıyı nasıl dağıtılayabilirsiniz öğrenin.
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
ms.openlocfilehash: 5a8b97e5bef57b29f388c86628f0af5d05e1724a
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81731653"
---
# <a name="step-1-deploy-the-log-forwarder"></a>Adım 1: Günlük iletmesini dağıtma


Bu adımda, güvenlik çözümünüzdeki günlükleri Azure Sentinel çalışma alanınıza iletecek Linux makinesini belirleyecek ve yapılandıracaksınız. Bu makine, şirket içi ortamınızda fiziksel veya sanal bir makine, başka bir bulutta Azure VM veya VM olabilir. Sağlanan bağlantıyı kullanarak, aşağıdaki görevleri gerçekleştiren atanmış makinede bir komut dosyası çalıştırın:
- Linux için Log Analytics aracısını (OMS aracısı olarak da bilinir) yükler ve aşağıdaki amaçlarla yapılandırır:
    - TCP bağlantı noktası 25226 üzerinde yerleşik Linux Syslog daemon CEF mesajları dinleme
    - iletileri TLS üzerinden güvenli bir şekilde, ayrıştırılmış ve zenginleştirilmiş Azure Sentinel çalışma alanınıza gönderme

- Yerleşik Linux Syslog daemon (rsyslog.d/syslog-ng) aşağıdaki amaçlar için yapılandırır:
    - TCP bağlantı noktası 514'teki güvenlik çözümlerinizden Syslog mesajlarını dinleme
    - Yalnızca CEF olarak tanımladığı iletileri TCP portu 25226'yı kullanarak localhost'taki Log Analytics temsilcisine iletme
 
## <a name="prerequisites"></a>Ön koşullar

- Belirlediğiniz Linux makinenizde yüksek izinlere (sudo) sahip olmalısınız.
- Linux makinesine python yüklü olmalıdır.<br>Kontrol `python -version` etmek için komutu kullanın.
- Log Analytics aracısını yüklemeden önce Linux makinesihiçbir Azure çalışma alanlarına bağlı olmamalıdır.

## <a name="run-the-deployment-script"></a>Dağıtım komut dosyasını çalıştırma
 
1. Azure Sentinel gezinti menüsünden **Veri bağlayıcılarını**tıklatın. Bağlayıcılar listesinden, **Ortak Olay Biçimi (CEF)** döşemesini ve ardından sağ alttaki Açık bağlayıcı **sayfası** düğmesini tıklatın. 

1. **1.2 Altında Linux makinesine CEF toplayıcı yükleyin, CEF** **toplayıcı yüklemek ve uygulamak için aşağıdaki komut dosyası çalıştır**altında sağlanan bağlantıyı kopyalamak, ya da aşağıdaki metinden:

     `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_installer.py&&sudo python cef_installer.py [WorkspaceID] [Workspace Primary Key]`

1. Komut dosyası çalışırken, herhangi bir hata veya uyarı iletisi almadığınızdan emin olun.

ADIM 2'ye devam [et: Güvenlik çözümünüzü CEF iletilerini iletmek için yapılandırın.](connect-cef-solution-config.md)

## <a name="deployment-script-explained"></a>Dağıtım komut dosyası açıklandı

Aşağıda, dağıtım komut dosyasının eylemlerinin komut la komut açıklaması veremistir.

Uygun açıklamayı görmek için bir syslog daemon seçin.

# <a name="rsyslog-daemon"></a>[rsyslog daemon](#tab/rsyslog)

1. **Log Analytics aracısını indirme ve yükleme:**

    - Log Analytics (OMS) Linux aracısı için yükleme komut dosyasını karşıdan yükler<br>
        `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh`

    - Log Analytics aracısını yükler<br>
        `sh onboard_agent.sh -w [workspaceID] -s [Primary Key] -d opinsights.azure.com`

1. **Syslog daemon yapılandırılması:**

    1. Syslog yapılandırma dosyasını `/etc/rsyslog.conf`kullanarak TCP iletişimi için 514 bağlantı noktasını açar.

    1. Syslog daemon dizinine özel bir yapılandırma dosyası `security-config-omsagent.conf` ekleyerek CEF iletilerini TCP bağlantı noktası 25226'daki Log Analytics `/etc/rsyslog.d/`temsilcisine iletmek için daemon'u yapılandırır.

        Dosyanın `security-config-omsagent.conf` içeriği:

            :rawmsg, regex, "CEF\|ASA" ~
            *.* @@127.0.0.1:25226

1. **Syslog daemon yeniden başlatma**

    `service rsyslog restart`

1. **Giriş Analizi aracıyapılandırmasını bağlantı noktası 25226'da dinleyecek şekilde ayarlama ve CEF mesajlarını Azure Sentinel'e iletme**

    1. Yapılandırmayı Log Analytics aracısı GitHub deposundan karşıdan yükle<br>
        `wget -o /etc/opt/microsoft/omsagent/[workspaceID]/conf/omsagent.d/security_events.conf https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/installer/conf/omsagent.d/security_events.conf`


    1. Log Analytics aracısını yeniden başlatır<br>
        `/opt/microsoft/omsagent/bin/service_control restart [workspaceID]`

# <a name="syslog-ng-daemon"></a>[syslog-ng daemon](#tab/syslogng)

1. **Log Analytics aracısını indirme ve yükleme:**

    - Log Analytics (OMS) Linux aracısı için yükleme komut dosyasını karşıdan yükler<br>`wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh`

    - Log Analytics aracısını yükler<br>`sh onboard_agent.sh -w [workspaceID] -s [Primary Key] -d opinsights.azure.com`

1. **Syslog daemon yapılandırılması:**

    1. Syslog yapılandırma dosyasını `/etc/syslog-ng/syslog-ng.conf`kullanarak TCP iletişimi için 514 bağlantı noktasını açar.

    1. Syslog daemon dizinine özel bir yapılandırma dosyası `security-config-omsagent.conf` ekleyerek CEF iletilerini TCP bağlantı noktası 25226'daki Log Analytics `/etc/syslog-ng/conf.d/`temsilcisine iletmek için daemon'u yapılandırır.

        Dosyanın `security-config-omsagent.conf` içeriği:

            filter f_oms_filter {match(\"CEF\|ASA\" ) ;};
            destination oms_destination {tcp(\"127.0.0.1\" port("25226"));};
            log {source(s_src);filter(f_oms_filter);destination(oms_destination);};

1. **Syslog daemon yeniden başlatma**

    `service syslog-ng restart`

1. **Giriş Analizi aracıyapılandırmasını bağlantı noktası 25226'da dinleyecek şekilde ayarlama ve CEF mesajlarını Azure Sentinel'e iletme**

    1. Yapılandırmayı Log Analytics aracısı GitHub deposundan karşıdan yükle<br>
        `wget -o /etc/opt/microsoft/omsagent/[workspaceID]/conf/omsagent.d/security_events.conf https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/installer/conf/omsagent.d/security_events.conf`


    1. Log Analytics aracısını yeniden başlatır<br>
        `/opt/microsoft/omsagent/bin/service_control restart [workspaceID]`

---

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, CEF cihazlarını Azure Sentinel'e bağlamak için Log Analytics aracısını nasıl dağıtabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.

