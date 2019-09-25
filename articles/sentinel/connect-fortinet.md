---
title: Fortinet verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Fortinet verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: add92907-0d7c-42b8-a773-f570f2d705ff
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 20079fd0c95da3e3aec9518f194ea39561a5e662
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240691"
---
# <a name="connect-your-fortinet-appliance"></a>Fortinet gerecinizi bağlama



Günlük dosyalarını Syslog ortak olay biçimi (CEF) olarak kaydederek, Azure Sentinel 'i herhangi bir Fortinet gerecine bağlayabilirsiniz. Azure Sentinel ile tümleştirmeyle, Fortinet 'teki günlük dosyası verileri genelinde analiz ve sorguları kolayca çalıştırabilirsiniz. Azure 'u Sentinel CEF verilerinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Connect CEF gereçileri](connect-common-event-format.md).

> [!NOTE]
> Veriler, Azure Sentinel 'i çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="step-1-connect-your-fortinet-appliance-by-using-an-agent"></a>1\. adım: Bir aracı kullanarak Fortinet gerecinizi bağlama

Fortinet gerecinizi Azure Sentinel 'e bağlamak için, Gereç ve Azure Sentinel arasındaki iletişimi desteklemek üzere adanmış bir VM 'de veya şirket içi makinede bir aracı dağıtın. 

Aracıyı Ayrıca, mevcut bir Azure VM 'de, başka bir buluttaki bir VM 'de veya şirket içi bir makinede el ile dağıtabilirsiniz.

> [!NOTE]
> Makinenin güvenliğini kuruluşunuzun güvenlik ilkesine göre yapılandırdığınızdan emin olun. Örneğin, ağınızı kurumsal ağ güvenlik ilkenize göre olacak şekilde yapılandırabilir ve gereksinimlerinize göre uyum sağlamak için arka plan programındaki bağlantı noktalarını ve protokolleri değiştirmelisiniz. 

Her iki seçenek için de bir ağ diyagramı görmek için bkz. [veri kaynaklarını bağlama](connect-data-sources.md#agent-options).

### <a name="deploy-the-agent"></a>Aracıyı dağıtma

1. Azure Sentinel portalında, **veri bağlayıcıları** ' na tıklayın ve **Fortinet** ' ı seçin ve ardından **bağlayıcı sayfası**' nı açın. 

1. **Syslog aracısını indirme ve yükleme**altında, Azure ya da şirket içi makine türünü seçin. 
1. Açılan **sanal makineler** ekranında, kullanmak istediğiniz makineyi seçin ve **Bağlan**' a tıklayın.
1. **Azure Linux sanal makineleri için aracıyı indir ve yükle**seçeneğini belirlerseniz makineyi seçin ve **Bağlan**' a tıklayın. **Azure dışı Linux sanal makineleri için aracı indir ve yükle**' yi seçerseniz, **doğrudan aracı** ekranında, **Linux için indirme ve ekleme Aracısı**altında betiği çalıştırın.
1. Bağlayıcı ekranında, **yapılandırma ve Iletme Syslog**altında, Syslog Daemon 'ınızın **rsyslog. d** veya **Syslog-ng**olduğunu ayarlayın. 
1. Bu komutları kopyalayıp gereç üzerinde çalıştırın:
   - Rsyslog. d öğesini seçtiyseniz:
            
     1. Local_4 ve 25226 numaralı bağlantı noktasını kullanarak syslog iletilerini Azure Sentinel aracısına dinlemek için Syslog Daemon 'a söyleyin. Şu komutu kullanın:`sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Fortinet\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
     1. 25226 numaralı bağlantı noktasını dinlemek için Syslog aracısını yapılandıran [security_events yapılandırma dosyasını](https://aka.ms/asi-syslog-config-file-linux) indirip yükleyin. Bu komutu kullanın: `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` nerede {0} , çalışma alanınızın GUID 'iyle değiştirilmelidir.
     1. Bu komutu kullanarak Syslog Daemon programını yeniden başlatın:`sudo service rsyslog restart`
            
   - Syslog-ng ' i seçtiyseniz:

      1. Local_4 ve 25226 numaralı bağlantı noktasını kullanarak syslog iletilerini Azure Sentinel aracısına dinlemek için Syslog Daemon 'a söyleyin. Şu komutu kullanın:`sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Fortinet\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
      1. 25226 numaralı bağlantı noktasını dinlemek için Syslog aracısını yapılandıran [security_events yapılandırma dosyasını](https://aka.ms/asi-syslog-config-file-linux) indirip yükleyin. Bu komutu kullanın: `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` nerede {0} , çalışma alanınızın GUID 'iyle değiştirilmelidir.
      1. Bu komutu kullanarak Syslog Daemon programını yeniden başlatın:`sudo service syslog-ng restart`
1. Bu komutu kullanarak Syslog aracısını yeniden başlatın:`sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
1. Şu komutu çalıştırarak aracı günlüğünde hata olmadığını doğrulayın:`tail /var/opt/microsoft/omsagent/log/omsagent.log`

 
## <a name="step-2-forward-fortinet-logs-to-the-syslog-agent"></a>2\. adım: Fortinet günlüklerini Syslog aracısına ilet

Fortinet Agent aracılığıyla syslog iletilerini CEF biçiminde Azure çalışma alanınıza iletmek için Fortinet 'ı yapılandırın.

1. Fortinet gerecinizde CLı 'yı açın ve aşağıdaki komutları çalıştırın:

        config log syslogd setting
        set format cef
        set facility <facility_name>
        set port 514
        set reliable disable
        set server <ip_address_of_Receiver>
        set status enable
        end

    - Sunucu **IP adresini** aracının IP adresiyle değiştirin.
    - **Facility_name** 'yi, aracıda yapılandırdığınız tesis kullanacak şekilde ayarlayın. Varsayılan olarak, aracı bunu local4 olarak ayarlar.
    - **Syslog bağlantı noktasını** **514** veya aracıda ayarlanan bağlantı noktası olarak ayarlayın.
    - Erken FortiOS sürümlerinde CEF biçimini etkinleştirmek için, **CSV 'yi devre dışı bırak**komut kümesini çalıştırmanız gerekebilir.
 
   > [!NOTE] 
   > Daha fazla bilgi için, [Fortinet belge kitaplığına](https://aka.ms/asi-syslog-fortinet-fortinetdocumentlibrary)gidin. Sürümünüzü seçin ve **el kitabı** ve **log ileti başvurusunu**kullanın.

 Azure Izleyici 'de ilgili şemayı, Fortinet olayları için Log Analytics kullanmak için arama `CommonSecurityLog`yapın.


## <a name="step-3-validate-connectivity"></a>3\. adım: Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünmeye başlaması 20 dakikaya kadar sürebilir. 

1. Doğru tesis kullandığınızdan emin olun. Tesis, Gereç ve Azure Sentinel 'de aynı olmalıdır. Azure Sentinel 'de hangi tesis dosyasını kullandığınızı denetleyebilir ve dosyada `security-config-omsagent.conf`değişiklik yapabilirsiniz. 

2. Günlüklerinizin Syslog aracısında doğru bağlantı noktasına sahip olduğundan emin olun. Bu komutu Syslog Aracısı makinesinde çalıştırın: `tcpdump -A -ni any  port 514 -vv`. Bu komut, cihazdan Syslog makinesine akış yapan günlükleri gösterir. Günlüklerin doğru bağlantı noktasında ve doğru tesisde kaynak gerecinden alındığından emin olun.

3. Göndereceğiniz günlüklerin [RFC 3164](https://tools.ietf.org/html/rfc3164)ile uyumlu olduğundan emin olun.

4. Syslog Aracısı 'nı çalıştıran bilgisayarda, 514 ve 25226 bağlantı noktalarının açık olduğundan ve komutunu `netstat -a -n:`kullanarak dinlediğinden emin olun. Bu komutu kullanma hakkında daha fazla bilgi için bkz. [netstat (8)-Linux man sayfası](https://linux.die.net/man/8/netstat). Düzgün şekilde dinliyorsa şunu görürsünüz:

   ![Azure Sentinel bağlantı noktaları](./media/connect-cef/ports.png) 

5. Arka plan programının, günlükleri gönderdiğiniz 514 numaralı bağlantı noktasında dinlemek için ayarlandığından emin olun.
    - Rsyslog için:<br>Dosyanın `/etc/rsyslog.conf` bu yapılandırmayı içerdiğinden emin olun:

           # provides UDP syslog reception
           module(load="imudp")
           input(type="imudp" port="514")
        
           # provides TCP syslog reception
           module(load="imtcp")
           input(type="imtcp" port="514")

      Daha fazla bilgi için bkz [. ımudp: UDP Syslog giriş modülü](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imudp.html#imudp-udp-syslog-input-module) ve [ıtcp: TCP Syslog giriş modülü](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imtcp.html#imtcp-tcp-syslog-input-module).

   - Syslog-NG için:<br>Dosyanın `/etc/syslog-ng/syslog-ng.conf` bu yapılandırmayı içerdiğinden emin olun:

           # source s_network {
            network( transport(UDP) port(514));
             };
     Daha fazla bilgi için bkz [. ımudp: UDP Syslog giriş modülü](https://rsyslog.readthedocs.io/en/latest/configuration/modules/imudp.html) ve [Syslog-ng açık kaynak sürüm 3,16-Yönetim Kılavuzu](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/administration-guide/19#TOPIC-956455).

1. Syslog Daemon ile aracı arasındaki iletişim olup olmadığını denetleyin. Bu komutu Syslog Aracısı makinesinde çalıştırın: `tcpdump -A -ni any  port 25226 -vv`. Bu komut, cihazdan Syslog makinesine akış yapan günlükleri gösterir. Günlüklerin aracıda alındığından emin olun.

6. Bu komutlardan her ikisi de başarılı bir şekilde sağlanmışsa, günlüklerinizin elde olup olmadığını görmek için Log Analytics işaretleyin. Bu gereçlerden akan tüm olaylar, tür altında `CommonSecurityLog` Log Analytics ham biçimde görüntülenir.

7. Hatalar olup olmadığını denetlemek veya günlüklere ulaşan bir sorun olup olmadığını denetlemek için bölümüne bakın `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`. Günlük biçimi uyuşmazlığı hatalar olduğunu söyyorsa, adresine `/etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` gidin ve dosyaya `security_events.conf`bakın. Günlüklerinizin bu dosyada gördüğünüz Regex biçimiyle eşleştiğinden emin olun.

8. Syslog iletinizin varsayılan boyutunun 2048 bayt (2 KB) ile sınırlı olduğundan emin olun. Günlükler çok uzunsa, bu komutu kullanarak security_events. conf dosyasını güncelleştirin:`message_length_limit 4096`

10. Fortinet günlüklerinizin aracı tarafından alınmıyorsa, bu komutu kullanarak, kullandığınız Syslog Daemon türüne bağlı olarak, Tesis ayarlamak ve günlükleri günlüklerde Fortinet sözcüğünü arayacak şekilde ayarlamak için bu komutu çalıştırın:
       - rsyslog. d:`sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Fortinet\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`

     Bu komutu kullanarak Syslog Daemon programını yeniden başlatın:`sudo service rsyslog restart`
       - Syslog-NG:`sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Fortinet\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
      
     Bu komutu kullanarak Syslog Daemon programını yeniden başlatın:`sudo service syslog-ng restart`

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Fortinet gereçlerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

