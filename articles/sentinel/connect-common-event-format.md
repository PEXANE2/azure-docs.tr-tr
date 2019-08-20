---
title: CEF verilerini Azure Sentinel önizlemesine bağlama | Microsoft Docs
description: CEF verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: cbf5003b-76cf-446f-adb6-6d816beca70f
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/19/2019
ms.author: rkarlin
ms.openlocfilehash: 28def73926294a025d70844e535a0856153ae30a
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69611939"
---
# <a name="connect-your-external-solution-using-common-event-format"></a>Ortak olay biçimini kullanarak dış çözümünüzü bağlama

> [!IMPORTANT]
> Azure Sentinel Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Sentinel 'e, günlük dosyalarını Syslog dosyasına kaydetmenizi sağlayan bir dış çözümle bağlanabilirsiniz. Gereciniz günlükleri Syslog ortak olay biçimi (CEF) olarak kaydetmenizi sağladığından Azure Sentinel ile tümleştirme, veri genelinde analiz ve sorguları kolayca çalıştırmanıza olanak sağlar.

> [!NOTE] 
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="how-it-works"></a>Nasıl çalışır?

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

## <a name="security-considerations"></a>Güvenlik konuları

Makinenin güvenliğini kuruluşunuzun güvenlik ilkesine göre yapılandırdığınızdan emin olun. Örneğin, ağınızı kurumsal ağ güvenlik ilkenize göre olacak şekilde yapılandırabilir ve gereksinimlerinize göre uyum sağlamak için arka plan programındaki bağlantı noktalarını ve protokolleri değiştirmelisiniz. Aşağıdaki yönergeleri kullanarak makinenizin güvenlik yapılandırmasını geliştirebilirsiniz:  [Azure 'Da GÜVENLI VM](../virtual-machines/linux/security-policy.md), [ağ güvenliği için en iyi uygulamalar](../security/fundamentals/network-best-practices.md).

Güvenlik çözümü ve Syslog makinesi arasındaki TLS iletişimini kullanmak için, Syslog Daemon 'u (rsyslog veya Syslog-ng) TLS 'de iletişim kurmak üzere yapılandırmanız gerekir: [Syslog TRAFIĞINI TLS-rsyslog Ile şifreleme](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html), [günlük iletilerini TLS – Syslog-NG ile şifreleme](https://support.oneidentity.com/technical-documents/syslog-ng-open-source-edition/3.22/administration-guide/60#TOPIC-1209298).


## <a name="step-1-configure-your-syslog-vm"></a>1\. adım: Syslog VM 'nizi yapılandırma

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
            
       2. 25226 numaralı bağlantı noktasını dinlemek için Syslog aracısını yapılandıran [security_events yapılandırma dosyasını](https://aka.ms/asi-syslog-config-file-linux) indirip yükleyin. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"`{0} Çalışma alanınızın GUID 'iyle değiştirilmelidir.
            
       1. Syslog Daemon 'u yeniden başlatma`sudo service rsyslog restart`<br> Daha fazla bilgi için bkz. [rsyslog belgeleri](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html)
           
    - Syslog-ng ' i seçtiyseniz:
       1. Local_4 ve 25226 numaralı bağlantı noktasını kullanarak syslog iletilerini Azure Sentinel aracısına dinlemek için Syslog Daemon 'a söyleyin. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
       2. 25226 numaralı bağlantı noktasını dinlemek için Syslog aracısını yapılandıran [security_events yapılandırma dosyasını](https://aka.ms/asi-syslog-config-file-linux) indirip yükleyin. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"`{0} Çalışma alanınızın GUID 'iyle değiştirilmelidir.

        3. Syslog Daemon 'u yeniden başlatma`sudo service syslog-ng restart` <br>Daha fazla bilgi için bkz. [Syslog-ng belgeleri](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/mutual-authentication-using-tls/2)
1. Bu komutu kullanarak Syslog aracısını yeniden başlatın:`sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
1. Şu komutu çalıştırarak aracı günlüğünde hata olmadığını doğrulayın:`tail /var/opt/microsoft/omsagent/log/omsagent.log`

CEF olayları için Log Analytics ilgili şemayı kullanmak için, araması `CommonSecurityLog`yapın.

## <a name="step-2-forward-common-event-format-cef-logs-to-syslog-agent"></a>2\. adım: Ortak olay biçimi (CEF) günlüklerini Syslog aracısına ilet

Güvenlik çözümünüzü Syslog aracısına CEF biçiminde Syslog iletileri gönderecek şekilde ayarlayın. Aracı yapılandırmanızda görüntülenen aynı parametreleri kullandığınızdan emin olun. Bunlar genellikle şunlardır:

- Bağlantı noktası 514
- Tesis local4

## <a name="step-3-validate-connectivity"></a>3\. adım: Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünene kadar 20 dakikadan bu kadar bir zaman çıkabilir. 

1. Doğru tesis kullandığınızdan emin olun. Tesis, Gereç ve Azure Sentinel 'de aynı olmalıdır. Azure Sentinel 'de hangi tesis dosyasını kullandığınızı denetleyebilir ve dosyada `security-config-omsagent.conf`değişiklik yapabilirsiniz. 

2. Günlüklerinizin Syslog aracısında doğru bağlantı noktasına sahip olduğundan emin olun. Bu komutu Syslog Aracısı makinesinde çalıştırın: `tcpdump -A -ni any  port 514 -vv`Bu komut, cihazdan Syslog makinesine akış yapan günlükleri gösterir. Sağ bağlantı noktası ve sağ tesis üzerindeki kaynak gerecinden günlüklerin alındığından emin olun.

3. Göndereceğiniz günlüklerin [RFC 3164](https://tools.ietf.org/html/rfc3164)ile uyumlu olduğundan emin olun.

4. Syslog Aracısı 'nı çalıştıran bilgisayarda, bu bağlantı noktalarının 514, 25226, komutunu `netstat -a -n:`kullanarak açık ve dinleme olduğundan emin olun. Bu komutu kullanma hakkında daha fazla bilgi için bkz. [netstat (8)-Linux man sayfası](https://linux.die.net/man/8/netstat). Düzgün şekilde dinliyorsa şunu görürsünüz:

   ![Azure Sentinel bağlantı noktaları](./media/connect-cef/ports.png) 

5. Arka plan programının, günlükleri gönderdiğiniz 514 numaralı bağlantı noktasında dinlemek için ayarlandığından emin olun.
    - Rsyslog için:<br>Dosyanın `/etc/rsyslog.conf` bu yapılandırmayı içerdiğinden emin olun:

           # provides UDP syslog reception
           module(load="imudp")
           input(type="imudp" port="514")
        
           # provides TCP syslog reception
           module(load="imtcp")
           input(type="imtcp" port="514")

      Daha fazla bilgi için bkz [. ımudp: UDP Syslog giriş modülü](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imudp.html#imudp-udp-syslog-input-module) ve [ıtcp: TCP Syslog giriş modülü](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imtcp.html#imtcp-tcp-syslog-input-module)

   - Syslog-NG için:<br>Dosyanın `/etc/syslog-ng/syslog-ng.conf` bu yapılandırmayı içerdiğinden emin olun:

           # source s_network {
            network( transport(UDP) port(514));
             };
     Daha fazla bilgi için bkz. [Syslog-ng açık kaynak sürüm 3,16-Yönetim Kılavuzu](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/administration-guide/19#TOPIC-956455).

1. Syslog Daemon ve aracı arasında iletişim olup olmadığını denetleyin. Bu komutu Syslog Aracısı makinesinde çalıştırın: `tcpdump -A -ni any  port 25226 -vv`Bu komut, cihazdan Syslog makinesine akış yapan günlükleri gösterir. Günlüklerin aracıda alındığından emin olun.

6. Bu komutlardan her ikisi de başarılı bir şekilde sağlanmışsa, günlüklerinizin elde olup olmadığını görmek için Log Analytics işaretleyin. Bu gereçlerden akan tüm olaylar, tür altında `CommonSecurityLog` Log Analytics ham biçimde görüntülenir.

7. Hatalar olup olmadığını denetlemek veya günlüklere ulaşan bir sorun olup olmadığını denetlemek için bölümüne bakın `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`. Günlük biçimi uyuşmazlığı hatalar olduğunu söyyorsa, adresine `/etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` gidin ve dosyaya `security_events.conf`bakın ve günlüklerinizin bu dosyada gördüğünüz Regex biçimiyle eşleştiğinden emin olun.

8. Syslog iletinizin varsayılan boyutunun 2048 bayt (2 KB) ile sınırlı olduğundan emin olun. Günlükler çok uzunsa, bu komutu kullanarak security_events. conf dosyasını güncelleştirin:`message_length_limit 4096`


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede CEF gereçlerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.

