---
title: Azure Sentinel 'e bağlantıyı doğrulama | Microsoft Docs
description: CEF iletilerinin Azure Sentinel 'e iletildiğinden emin olmak için güvenlik çözümünüzün bağlantısını doğrulayın.
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
ms.openlocfilehash: 07a6b84569fe0356267440e38b31ac738b2659d6
ms.sourcegitcommit: 635114a0f07a2de310b34720856dd074aaf4f9cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85260840"
---
# <a name="step-3-validate-connectivity"></a>3. Adım: bağlantıyı doğrulama

Günlük ileticinizi dağıttıktan sonra (adım 1 ' de) ve Güvenlik çözümünüzü BT CEF iletileri gönderecek şekilde yapılandırdıysanız (adım 2 ' de), güvenlik çözümünüz ile Azure Sentinel arasındaki bağlantıyı doğrulamak için bu yönergeleri izleyin. 

## <a name="prerequisites"></a>Ön koşullar

- Günlük ileticisi makinenizde yükseltilmiş izinleriniz (sudo) olmalıdır.

- Günlük ileticisi makinenizde Python yüklü olmalıdır.<br>
`python –version`Denetlemek için komutunu kullanın.

## <a name="how-to-validate-connectivity"></a>Bağlantıyı doğrulama

1. Azure Sentinel gezinti menüsünde **Günlükler**' i açın. Güvenlik çözümünüzdeki günlükleri alıp almadığınızı görmek için **Commonsecuritylog** şemasını kullanarak bir sorgu çalıştırın.<br>
Günlüklerinizin **Log Analytics**görünmeye başlaması için 20 dakika sürecağına dikkat edin. 

1. Sorgudan herhangi bir sonuç görmüyorsanız, olayların güvenlik çözümünden oluşturulduğunu doğrulayın veya bir süre oluşturmayı deneyin ve belirlediğiniz Syslog iletici makinesine iletildiğini doğrulayın. 

1. Güvenlik çözümünüz, günlük ileticisi ve Azure Sentinel arasındaki bağlantıyı denetlemek için günlük ileticisinde aşağıdaki betiği çalıştırın. Bu betik, arka plan programının doğru şekilde yapılandırıldığını ve arka plan programı ile Log Analytics Aracısı arasındaki iletişimi engellemediğini kontrol eder. Ayrıca, uçtan uca bağlantıyı denetlemek için ' TestCommonEventFormat ' adlı sahte iletiler gönderir. <br>
 `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_troubleshoot.py&&sudo python cef_troubleshoot.py [WorkspaceID]`

## <a name="validation-script-explained"></a>Doğrulama betiği açıklanmıştı

Doğrulama betiği aşağıdaki denetimleri gerçekleştirir:

# <a name="rsyslog-daemon"></a>[rsyslog Daemon](#tab/rsyslog)

1. Dosyanın<br>
    `/etc/opt/microsoft/omsagent/[WorkspaceID]/conf/omsagent.d/security_events.conf`<br>
    var ve geçerli.

1. Dosyanın aşağıdaki metni içerip içermediğini denetler:

        <source>
            type syslog
            port 25226
            bind 127.0.0.1
            protocol_type tcp
            tag oms.security
            format /(?<time>(?:\w+ +){2,3}(?:\d+:){2}\d+|\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}.[\w\-\:\+]{3,12}):?\s*(?:(?<host>[^: ]+) ?:?)?\s*(?<ident>.*CEF.+?(?=0\|)|%ASA[0-9\-]{8,10})\s*:?(?<message>0\|.*|.*)/
            <parse>
                message_format auto
            </parse>
        </source>

        <filter oms.security.**>
            type filter_syslog_security
        </filter>

1. Makinede ağ trafiğini (örneğin, bir ana bilgisayar güvenlik duvarı) engelliyor olabilecek herhangi bir güvenlik geliştirmesi olup olmadığını denetler.

1. Syslog Daemon (rsyslog), TCP bağlantı noktası 25226 ' deki Log Analytics aracısına CEF (Regex kullanılarak) olarak tanımladığı iletileri göndermek için düzgün şekilde yapılandırıldığını denetler:

    - Yapılandırma dosyası:`/etc/rsyslog.d/security-config-omsagent.conf`

            :rawmsg, regex, "CEF"|"ASA"
            *.* @@127.0.0.1:25226

1. Syslog arka plan programının 514 numaralı bağlantı noktasında veri aldığını denetler

1. Gerekli bağlantıların kurulu olduğunu denetler: veri almak için TCP 514, Syslog Daemon ve Log Analytics Aracısı arasındaki iç iletişim için TCP 25226

1. SAHTE verileri, localhost üzerinde 514 numaralı bağlantı noktasına gönderir. Bu veriler, aşağıdaki sorguyu çalıştırarak Azure Sentinel çalışma alanında observable olmalıdır:

        CommonSecurityLog
        | where DeviceProduct == "MOCK"

# <a name="syslog-ng-daemon"></a>[Syslog-ng Daemon](#tab/syslogng)

1. Dosyanın<br>
    `/etc/opt/microsoft/omsagent/[WorkspaceID]/conf/omsagent.d/security_events.conf`<br>
    var ve geçerli.

1. Dosyanın aşağıdaki metni içerip içermediğini denetler:

        <source>
            type syslog
            port 25226
            bind 127.0.0.1
            protocol_type tcp
            tag oms.security
            format /(?<time>(?:\w+ +){2,3}(?:\d+:){2}\d+|\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}.[\w\-\:\+]{3,12}):?\s*(?:(?<host>[^: ]+) ?:?)?\s*(?<ident>.*CEF.+?(?=0\|)|%ASA[0-9\-]{8,10})\s*:?(?<message>0\|.*|.*)/
            <parse>
                message_format auto
            </parse>
        </source>

        <filter oms.security.**>
            type filter_syslog_security
        </filter>

1. Makinede ağ trafiğini (örneğin, bir ana bilgisayar güvenlik duvarı) engelliyor olabilecek herhangi bir güvenlik geliştirmesi olup olmadığını denetler.

1. Syslog Daemon (syslog-ng), TCP bağlantı noktası 25226 ' deki Log Analytics aracısına CEF (Regex kullanılarak) olarak tanımladığı iletileri göndermek için düzgün şekilde yapılandırıldığını denetler:

    - Yapılandırma dosyası:`/etc/syslog-ng/conf.d/security-config-omsagent.conf`

            filter f_oms_filter {match(\"CEF\|ASA\" ) ;};
            destination oms_destination {tcp(\"127.0.0.1\" port("25226"));};
            log {source(s_src);filter(f_oms_filter);destination(oms_destination);};

1. Syslog arka plan programının 514 numaralı bağlantı noktasında veri aldığını denetler

1. Gerekli bağlantıların kurulu olduğunu denetler: veri almak için TCP 514, Syslog Daemon ve Log Analytics Aracısı arasındaki iç iletişim için TCP 25226

1. SAHTE verileri, localhost üzerinde 514 numaralı bağlantı noktasına gönderir. Bu veriler, aşağıdaki sorguyu çalıştırarak Azure Sentinel çalışma alanında observable olmalıdır:

        CommonSecurityLog
        | where DeviceProduct == "MOCK"

---

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede CEF gereçlerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .

