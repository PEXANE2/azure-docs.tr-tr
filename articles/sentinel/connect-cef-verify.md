---
title: Azure Sentinel'e bağlantı doğrulama | Microsoft Dokümanlar
description: CEF iletilerinin Azure Sentinel'e iletildiğinden emin olmak için güvenlik çözümünüzün bağlantılarını doğrulayın.
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
ms.openlocfilehash: 6b91e36ee09aa855c119add2c0eb268cf8b97393
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81731824"
---
# <a name="step-3-validate-connectivity"></a>ADIM 3: Bağlantıyı doğrulama

Günlük iletmecinizi (Adım 1'de) dağıttıktan ve güvenlik çözümünüzü CEF iletileri (Adım 2'de) gönderecek şekilde yapılandırdıktan sonra, güvenlik çözümünüzle Azure Sentinel arasındaki bağlantıyı doğrulamak için aşağıdaki yönergeleri izleyin. 

## <a name="prerequisites"></a>Ön koşullar

- Günlük ileti makinenizde yüksek izinlere (sudo) sahip olmalısınız.

- Günlük forwarder makinenizde Python yüklü olmalıdır.<br>
Kontrol `python –version` etmek için komutu kullanın.

## <a name="how-to-validate-connectivity"></a>Bağlantı nasıl doğrulanır?

1. Azure Sentinel gezinti menüsünden **Günlükler'i**açın. Güvenlik çözümünüzden günlük alıp almadığınızı görmek için **CommonSecurityLog** şemasını kullanarak bir sorgu çalıştırın.<br>
**Günlüklerinizin Log Analytics'te**görünmeye başlamasının yaklaşık 20 dakika sürebileceğini unutmayın. 

1. Sorgudan herhangi bir sonuç görmüyorsanız, olayların güvenlik çözümünüzden oluşturulduğunu doğrulayın veya bazılarını oluşturmayı deneyin ve bunların belirlediğiniz Syslog iletme makinesine iletildiğini doğrulayın. 

1. Güvenlik çözümünüz, günlük iletmeciniz ve Azure Sentinel arasındaki bağlantıyı denetlemek için günlük iletmecinde aşağıdaki komut dosyasını çalıştırın. Bu komut dosyası, daemon'un doğru bağlantı noktalarında dinlediğini, iletmenin düzgün yapılandırıldığından ve daemon ile Log Analytics aracısı arasındaki iletişimi hiçbir şeyin engellemediğini denetler. Ayrıca uçtan uca bağlantıyı denetlemek için sahte mesajlar 'TestCommonEventFormat' gönderir. <br>
 `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_troubleshoot.py&&sudo python cef_troubleshoot.py [WorkspaceID]`

## <a name="validation-script-explained"></a>Doğrulama komut dosyası açıklandı

Doğrulama komut dosyası aşağıdaki denetimleri gerçekleştirir:

# <a name="rsyslog-daemon"></a>[rsyslog daemon](#tab/rsyslog)

1. Dosyanın<br>
    `/etc/opt/microsoft/omsagent/[WorkspaceID]/conf/omsagent.d/security_events.conf`<br>
    vardır ve geçerlidir.

1. Dosyanın aşağıdaki metni içermelerini denetler:

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

1. Makinede ağ trafiğini engelleyebilecek (ana bilgisayar güvenlik duvarı gibi) güvenlik geliştirmeleri olup olmadığını denetler.

1. Syslog daemon'un (rsyslog) CEF (regex kullanarak) olarak tanımladığı iletileri TCP bağlantı noktası 25226'daki Log Analytics temsilcisine gönderecek şekilde düzgün bir şekilde yapılandırıldığından kontrol edilir:

    - Yapılandırma dosyası:`/etc/rsyslog.d/security-config-omsagent.conf`

            :rawmsg, regex, "CEF\|ASA" ~
            *.* @@127.0.0.1:25226

1. Syslog daemon port 514 veri alıyor denetler

1. Gerekli bağlantıların kurulup kurulmadığını denetler: veri almak için tcp 514, syslog daemon ile Log Analytics aracısı arasındaki iç iletişim için tcp 25226

1. Mock verilerini localhost'taki 514 bağlantı noktasına gönderir. Bu veriler, aşağıdaki sorguyu çalıştırarak Azure Sentinel çalışma alanında gözlemlenebilir olmalıdır:

        CommonSecurityLog
        | where DeviceProduct == "MOCK"

# <a name="syslog-ng-daemon"></a>[syslog-ng daemon](#tab/syslogng)

1. Dosyanın<br>
    `/etc/opt/microsoft/omsagent/[WorkspaceID]/conf/omsagent.d/security_events.conf`<br>
    vardır ve geçerlidir.

1. Dosyanın aşağıdaki metni içermelerini denetler:

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

1. Makinede ağ trafiğini engelleyebilecek (ana bilgisayar güvenlik duvarı gibi) güvenlik geliştirmeleri olup olmadığını denetler.

1. Syslog daemon'un (syslog-ng) CEF (regex kullanarak) olarak tanımladığı iletileri TCP bağlantı noktası 25226'daki Log Analytics temsilcisine gönderecek şekilde düzgün bir şekilde yapılandırıldığından kontrol edilir:

    - Yapılandırma dosyası:`/etc/syslog-ng/conf.d/security-config-omsagent.conf`

            filter f_oms_filter {match(\"CEF\|ASA\" ) ;};
            destination oms_destination {tcp(\"127.0.0.1\" port("25226"));};
            log {source(s_src);filter(f_oms_filter);destination(oms_destination);};

1. Syslog daemon port 514 veri alıyor denetler

1. Gerekli bağlantıların kurulup kurulmadığını denetler: veri almak için tcp 514, syslog daemon ile Log Analytics aracısı arasındaki iç iletişim için tcp 25226

1. Mock verilerini localhost'taki 514 bağlantı noktasına gönderir. Bu veriler, aşağıdaki sorguyu çalıştırarak Azure Sentinel çalışma alanında gözlemlenebilir olmalıdır:

        CommonSecurityLog
        | where DeviceProduct == "MOCK"

---

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, CEF cihazlarını Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)

