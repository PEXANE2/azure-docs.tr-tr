---
title: CEF verilerini Azure Sentinel önizlemesine bağlama | Microsoft Docs
description: CEF verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: e74dd54403ed599aa95e8fc8a94c2bd7a3ca41d8
ms.sourcegitcommit: a19f4b35a0123256e76f2789cd5083921ac73daf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71719121"
---
# <a name="connect-your-external-solution-using-common-event-format"></a>Ortak olay biçimini kullanarak dış çözümünüzü bağlama

Azure Sentinel 'e, günlük dosyalarını Syslog dosyasına kaydetmenizi sağlayan bir dış çözümle bağlanabilirsiniz. Gereciniz günlükleri Syslog ortak olay biçimi (CEF) olarak kaydetmenizi sağladığından Azure Sentinel ile tümleştirme, veri genelinde analiz ve sorguları kolayca çalıştırmanıza olanak sağlar.

> [!NOTE] 
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="how-it-works"></a>Nasıl çalışır

Azure Sentinel ve CEF gereç arasındaki bağlantı üç adımda gerçekleşir:

1. Gereç üzerinde, bu değerleri, gerecin gerekli günlük dosyalarını, Microsoft Monitoring Agent bağlı olarak Azure Sentinel Syslog aracısına göndermesi için ayarlamanız gerekir. Bu parametreleri Azure Sentinel aracısında Syslog arka plan programı 'nda da değiştirdiğiniz sürece, aracısında değiştirebilirsiniz.
    - Protokol = UDP
    - Bağlantı noktası = 514
    - Tesis = Local4
    - Biçim = CEF
2. Syslog Aracısı verileri toplar ve bunları ayrıştırılıp zenginleştirilmiş Log Analytics güvenli bir şekilde gönderir.
3. Aracı verileri bir Log Analytics çalışma alanında depolar, böylece analiz, bağıntı kuralları ve panolar kullanılarak gerektiğinde sorgulanabilir.

> [!NOTE]
> Aracı birden çok kaynaktan Günlükler toplayabilir, ancak adanmış makineye yüklenmelidir.


 ![Azure 'da CEF](./media/connect-cef/cef-syslog-azure.png)

Alternatif olarak, aracıyı mevcut bir Azure VM 'de, başka bir buluttaki VM 'de veya şirket içi bir makinede el ile dağıtabilirsiniz. 

 ![Şirket içi CEF](./media/connect-cef/cef-syslog-onprem.png)

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Makinenin güvenliğini kuruluşunuzun güvenlik ilkesine göre yapılandırdığınızdan emin olun. Örneğin, ağınızı kurumsal ağ güvenlik ilkenize göre olacak şekilde yapılandırabilir ve gereksinimlerinize göre uyum sağlamak için arka plan programındaki bağlantı noktalarını ve protokolleri değiştirmelisiniz. Aşağıdaki yönergeleri kullanarak makinenizin güvenlik yapılandırmasını geliştirebilirsiniz:  [Azure 'Da GÜVENLI VM](../virtual-machines/linux/security-policy.md), [ağ güvenliği için en iyi uygulamalar](../security/fundamentals/network-best-practices.md).

Güvenlik çözümü ve Syslog makinesi arasındaki TLS iletişimini kullanmak için Syslog Daemon (rsyslog veya Syslog-ng) ' i TLS ile iletişim kurmak üzere yapılandırmanız gerekir: [Syslog TRAFIĞINI TLS-rsyslog Ile şifreleme](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html), [günlük iletilerini TLS ile şifreleme – Syslog-ng](https://support.oneidentity.com/technical-documents/syslog-ng-open-source-edition/3.22/administration-guide/60#TOPIC-1209298).


## <a name="step-1-configure-your-syslog-vm"></a>1\. Adım: Syslog sanal makinesini yapılandırma

Gereç ve Azure Sentinel arasındaki iletişimi desteklemek için adanmış bir Linux makinesine (VM veya şirket içi) bir aracı dağıtmanız gerekir. 

> [!NOTE]
> Makinenin güvenliğini kuruluşunuzun güvenlik ilkesine göre yapılandırdığınızdan emin olun. Örneğin, ağınızı kurumsal ağ güvenlik ilkenize göre olacak şekilde yapılandırabilir ve gereksinimlerinize göre uyum sağlamak için arka plan programındaki bağlantı noktalarını ve protokolleri değiştirmelisiniz. 


1. Azure Sentinel portalında, **veri bağlayıcıları** ' na tıklayın ve **ortak olay biçimi (CEF)** öğesini seçin ve ardından **bağlayıcı sayfasını açın**. 

1. **Syslog aracısını indirme ve yükleme**altında, Azure ya da şirket içi makine türünü seçin. 
1. Açılan **sanal makineler** ekranında, kullanmak istediğiniz makineyi seçin ve **Bağlan**' a tıklayın.
1. **Azure Linux sanal makineleri için aracıyı indir ve yükle**seçeneğini belirlerseniz makineyi seçin ve **Bağlan**' a tıklayın. **Azure dışı Linux sanal makineleri için aracı indir ve yükle**' yi seçerseniz, **doğrudan aracı** ekranında, **Linux için indirme ve ekleme Aracısı**altında betiği çalıştırın.
1. CEF Bağlayıcısı ekranında, **yapılandırma ve Iletme Syslog**altında, Syslog Daemon 'ınızın **rsyslog. d** veya **Syslog-ng**olduğunu ayarlayın. 
1. Bu komutları kopyalayıp gereç üzerinde çalıştırın:
    - Rsyslog. d öğesini seçtiyseniz:
              
       1. Local_4 ve 25226 numaralı bağlantı noktasını kullanarak syslog iletilerini Azure Sentinel aracısına dinlemek için Syslog Daemon 'a söyleyin. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
       2. 25226 numaralı bağlantı noktasını dinlemek için Syslog aracısını yapılandıran [security_events yapılandırma dosyasını](https://aka.ms/asi-syslog-config-file-linux) indirip yükleyin. {0} ' in, çalışma alanınızın GUID 'iyle değiştirildiği .
            
       1. Syslog Daemon 'u yeniden başlatın `sudo service rsyslog restart`<br> Daha fazla bilgi için bkz. [rsyslog belgeleri](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html)
           
    - Syslog-ng ' i seçtiyseniz:
       1. Local_4 ve 25226 numaralı bağlantı noktasını kullanarak syslog iletilerini Azure Sentinel aracısına dinlemek için Syslog Daemon 'a söyleyin. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
       2. 25226 numaralı bağlantı noktasını dinlemek için Syslog aracısını yapılandıran [security_events yapılandırma dosyasını](https://aka.ms/asi-syslog-config-file-linux) indirip yükleyin. {0} ' in, çalışma alanınızın GUID 'iyle değiştirildiği .

        3. Syslog Daemon 'u yeniden başlatın `sudo service syslog-ng restart` <br>Daha fazla bilgi için bkz. [Syslog-ng belgeleri](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/mutual-authentication-using-tls/2)
1. Şu komutu kullanarak Syslog aracısını yeniden başlatın: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
1. Şu komutu çalıştırarak aracı günlüğünde hata olmadığından emin olun: `tail /var/opt/microsoft/omsagent/log/omsagent.log`

CEF olayları için Log Analytics ilgili şemayı kullanmak için, `CommonSecurityLog` ' ı arayın.

## <a name="step-2-forward-common-event-format-cef-logs-to-syslog-agent"></a>2\. Adım: ortak olay biçimi (CEF) günlüklerini Syslog aracısına Ilet

Güvenlik çözümünüzü Syslog aracısına CEF biçiminde Syslog iletileri gönderecek şekilde ayarlayın. Aracı yapılandırmanızda görüntülenen aynı parametreleri kullandığınızdan emin olun. Bunlar genellikle şunlardır:

- Bağlantı noktası 514
- Tesis local4

## <a name="step-3-validate-connectivity"></a>3\. Adım: bağlantıyı doğrulama

Günlüklerinizin Log Analytics görünene kadar 20 dakikadan bu kadar bir zaman çıkabilir. 

1. Doğru tesis kullandığınızdan emin olun. Tesis, Gereç ve Azure Sentinel 'de aynı olmalıdır. Azure Sentinel 'de hangi tesis dosyasını kullandığınızı denetleyebilir ve dosyada `security-config-omsagent.conf` ' a değiştirirsiniz. 

2. Günlüklerinizin Syslog aracısında doğru bağlantı noktasına sahip olduğundan emin olun. Bu komutu Syslog Aracısı makinesinde çalıştırın: `tcpdump -A -ni any  port 514 -vv` Bu komutta cihazdan Syslog makinesine akış yapan Günlükler gösterilir. Sağ bağlantı noktası ve sağ tesis üzerindeki kaynak gerecinden günlüklerin alındığından emin olun.

3. Göndereceğiniz günlüklerin [RFC 3164](https://tools.ietf.org/html/rfc3164)ile uyumlu olduğundan emin olun.

4. Syslog Aracısı 'nı çalıştıran bilgisayarda, bu bağlantı noktalarının 514, 25226, `netstat -a -n:` komutunu kullanarak açık ve dinleme olduğundan emin olun. Bu komutu kullanma hakkında daha fazla bilgi için bkz. [netstat (8)-Linux man sayfası](https://linux.die.net/man/8/netstat). Düzgün şekilde dinliyorsa şunu görürsünüz:

   ![Azure Sentinel bağlantı noktaları](./media/connect-cef/ports.png) 

5. Arka plan programının, günlükleri gönderdiğiniz 514 numaralı bağlantı noktasında dinlemek için ayarlandığından emin olun.
    - Rsyslog için:<br>@No__t-0 dosyasında bu yapılandırmayı içerdiğinden emin olun:

           # provides UDP syslog reception
           module(load="imudp")
           input(type="imudp" port="514")
        
           # provides TCP syslog reception
           module(load="imtcp")
           input(type="imtcp" port="514")

      Daha fazla bilgi için bkz [. ımudp: UDP Syslog giriş modülü](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imudp.html#imudp-udp-syslog-input-module) ve [ıMTCP: TCP Syslog giriş modülü](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imtcp.html#imtcp-tcp-syslog-input-module)

   - Syslog-NG için:<br>@No__t-0 dosyasında bu yapılandırmayı içerdiğinden emin olun:

           # source s_network {
            network( transport(UDP) port(514));
             };
     Daha fazla bilgi için bkz. [Syslog-ng açık kaynak sürüm 3,16-Yönetim Kılavuzu](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/administration-guide/19#TOPIC-956455).

1. Syslog Daemon ve aracı arasında iletişim olup olmadığını denetleyin. Bu komutu Syslog Aracısı makinesinde çalıştırın: `tcpdump -A -ni any  port 25226 -vv` Bu komutta cihazdan Syslog makinesine akış yapan Günlükler gösterilir. Günlüklerin aracıda alındığından emin olun.

6. Bu komutlardan her ikisi de başarılı bir şekilde sağlanmışsa, günlüklerinizin elde olup olmadığını görmek için Log Analytics işaretleyin. Bu gereçlerden akan tüm olaylar, `CommonSecurityLog` türü altında Log Analytics ham biçimde görüntülenir.

7. Hatalar olup olmadığını denetlemek veya Günlükler yoksa, `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log` ' a bakın. Günlük biçimi uyuşmazlığı hatalar varsa, `/etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` ' a gidin ve `security_events.conf`dosyasına bakın ve günlüklerinizin bu dosyada gördüğünüz Regex biçimiyle eşleştiğinden emin olun.

8. Syslog iletinizin varsayılan boyutunun 2048 bayt (2 KB) ile sınırlı olduğundan emin olun. Günlükler çok uzunsa, şu komutu kullanarak security_events. conf dosyasını güncelleştirin: `message_length_limit 4096`


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede CEF gereçlerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

