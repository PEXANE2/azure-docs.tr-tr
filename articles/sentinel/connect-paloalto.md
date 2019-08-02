---
title: Palo Alto Networks verilerini Azure Sentinel Preview 'a bağlama | Microsoft Docs
description: Palo Alto Networks verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: a4b21d67-1a72-40ec-bfce-d79a8707b4e1
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2019
ms.author: rkarlin
ms.openlocfilehash: 5860e1b1b817985aafd95f6f63d39553489482b9
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68679225"
---
# <a name="connect-your-palo-alto-networks-appliance"></a>Palo Alto Networks gerecinizi bağlama

> [!IMPORTANT]
> Azure Sentinel Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Günlük dosyalarını Syslog ortak hata biçimi (CEF) olarak kaydederek, Azure Sentinel 'e herhangi bir Palo Alto Networks gerecine bağlanabilirsiniz. Azure Sentinel ile tümleştirme, Palo Alto ağlarındaki günlük dosyası verilerinde analiz ve sorguları kolayca çalıştırmanıza olanak sağlar. Azure 'u Sentinel CEF verilerinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Connect CEF](connect-common-event-format.md)gereçileri.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="step-1-connect-your-palo-alto-networks-appliance-using-an-agent"></a>1\. adım: Bir aracı kullanarak Palo Alto Networks gerecinizi bağlama

Palo Alto Networks gerecinizi Azure Sentinel 'e bağlamak için, Gereç ve Azure Sentinel arasındaki iletişimi desteklemek üzere adanmış bir makineye (VM veya şirket içi) bir aracı dağıtmanız gerekir. 

Alternatif olarak, aracıyı mevcut bir Azure VM 'de, başka bir buluttaki VM 'de veya şirket içi bir makinede el ile dağıtabilirsiniz.

> [!NOTE]
> Makinenin güvenliğini kuruluşunuzun güvenlik ilkesine göre yapılandırdığınızdan emin olun. Örneğin, ağınızı kurumsal ağ güvenlik ilkenize göre olacak şekilde yapılandırabilir ve gereksinimlerinize göre uyum sağlamak için arka plan programındaki bağlantı noktalarını ve protokolleri değiştirmelisiniz. 

Her iki seçenek için de bir ağ diyagramı görmek için bkz. [veri kaynaklarını bağlama](connect-data-sources.md#agent-options).


### <a name="deploy-the-agent"></a>Aracıyı dağıtma 

1. Azure Sentinel portalında, **veri bağlayıcıları** ' na tıklayın ve **Palo Alto ağları** ' nı seçin ve ardından **bağlayıcı sayfası**' nı açın. 

1. **Syslog aracısını indirme ve yükleme**altında, Azure ya da şirket içi makine türünü seçin. 
1. Açılan **sanal makineler** ekranında, kullanmak istediğiniz makineyi seçin ve **Bağlan**' a tıklayın.
1. **Azure Linux sanal makineleri için aracıyı indir ve yükle**seçeneğini belirlerseniz makineyi seçin ve **Bağlan**' a tıklayın. **Azure dışı Linux sanal makineleri için aracı indir ve yükle**' yi seçerseniz, **doğrudan aracı** ekranında, **Linux için indirme ve ekleme Aracısı**altında betiği çalıştırın.      1. Bağlayıcı ekranında, **yapılandırma ve Iletme Syslog**altında, Syslog Daemon 'ınızın **rsyslog. d** veya **Syslog-ng**olduğunu ayarlayın. 
1. Bu komutları kopyalayıp gereç üzerinde çalıştırın:
     - Rsyslog. d öğesini seçtiyseniz:
              
       1. Local_4 ve 25226 numaralı bağlantı noktasını kullanarak syslog iletilerini Azure Sentinel aracısına dinlemek için Syslog Daemon 'a söyleyin. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
       2. 25226 numaralı bağlantı noktasını dinlemek için Syslog aracısını yapılandıran [security_events yapılandırma dosyasını](https://aka.ms/asi-syslog-config-file-linux) indirip yükleyin. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"`{0} Çalışma alanınızın GUID 'iyle değiştirilmelidir.
            
       1. Syslog Daemon 'u yeniden başlatma`sudo service rsyslog restart`
             
    - Syslog-ng ' i seçtiyseniz:

         1. Local_4 ve 25226 numaralı bağlantı noktasını kullanarak syslog iletilerini Azure Sentinel aracısına dinlemek için Syslog Daemon 'a söyleyin. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
         2. 25226 numaralı bağlantı noktasını dinlemek için Syslog aracısını yapılandıran [security_events yapılandırma dosyasını](https://aka.ms/asi-syslog-config-file-linux) indirip yükleyin. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"`{0} Çalışma alanınızın GUID 'iyle değiştirilmelidir.

         3. Syslog Daemon 'u yeniden başlatma`sudo service syslog-ng restart`
 1. Bu komutu kullanarak Syslog aracısını yeniden başlatın:`sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
 1. Şu komutu çalıştırarak aracı günlüğünde hata olmadığını doğrulayın:`tail /var/opt/microsoft/omsagent/log/omsagent.log`


 
## <a name="step-2-forward-palo-alto-networks-logs-to-the-syslog-agent"></a>2\. adım: Palo Alto Networks günlüklerini Syslog aracısına ilet

Azure çalışma alanınıza Syslog Aracısı aracılığıyla syslog iletilerini CEF biçiminde iletmek için Palo Alto ağlarını yapılandırın:
1.  [Ortak olay biçimi (CEF) yapılandırma kılavuzlarından](https://docs.paloaltonetworks.com/resources/cef) gidin ve gereç türü için PDF 'yi indirin. CEF olaylarını toplamak üzere Palo Alto Networks gerecinizi ayarlamak için kılavuzdaki tüm yönergeleri izleyin. 

1.  [Syslog Izlemesini yapılandırma](https://aka.ms/asi-syslog-paloalto-forwarding) bölümüne gidin ve Azure Sentinel 'e Palo Alto Networks GERECINDEN CEF olay iletmeyi yapılandırmak için adım 2 ve 3 ' ü izleyin.

    1. **Syslog Sunucu biçimini** **BSD**olarak ayarladığınızdan emin olun.
    1. **Tesis numarasını** Syslog aracısında ayarladığınız aynı değere ayarladığınızdan emin olun.

       > [!NOTE]
       > PDF 'den Kopyala/yapıştır işlemleri metni değiştirebilir ve rastgele karakterler ekleyebilir. Bunu önlemek için, metni bir düzenleyiciye kopyalayın ve bu örnekte görebileceğiniz gibi, yapıştırmadan önce günlük biçimini bozabilecek tüm karakterleri kaldırın.
 
        ![CEF metin kopyalama sorunu](./media/connect-cef/paloalto-text-prob1.png)

2. Palo Alto Networks olayları için Log Analytics ilgili şemayı kullanmak için, **Commonsecuritylog**' u arayın.

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
Bu belgede, Palo Alto Networks gereçlerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.

