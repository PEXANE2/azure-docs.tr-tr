---
title: Syslog verilerini Azure Sentinel'e bağlayın | Microsoft Dokümanlar
description: Syslog verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
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
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 73fd55fc24fd94dc88bba2f591c32480f77c7d5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588085"
---
# <a name="connect-your-external-solution-using-syslog"></a>Syslog'u kullanarak harici çözümünüzü bağlayın

Syslog'u destekleyen tüm şirket içi aletleri Azure Sentinel'e bağlayabilirsiniz. Bu, cihaz ve Azure Sentinel arasındaki Linux makinesini temel alan bir aracı kullanılarak yapılır. Linux makineniz Azure'daysa, günlükleri cihazınızdan veya uygulamanızdan Azure'da oluşturduğunuz özel bir çalışma alanına aktarabilir ve bağlayabilirsiniz. Linux makineniz Azure'da değilse, günlükleri cihazınızdan Linux için Aracı'yı yüklediğiniz özel bir şirket içi VM'ye veya makineye aktarabilirsiniz. 

> [!NOTE]
> Cihazınız Syslog CEF'i destekliyorsa, bağlantı daha eksiksizdir ve bu seçeneği seçmeli ve [CEF'den veri bağlama](connect-common-event-format.md)yönergelerini izlemelisiniz.

## <a name="how-it-works"></a>Nasıl çalışır?

Syslog, Linux'ta yaygın olan bir olay günlüğü protokolüdür. Uygulamalar, yerel makinede depolanabilecek veya bir Syslog toplayıcısına teslim edilebilecek iletiler gönderir. Linux için Log Analytics aracısı yüklendiğinde, iletileri temsilciye iletmek için yerel Syslog daemon'u yapılandırır. Aracı, daha sonra ilgili kaydın oluşturulduğu Azure Monitor'a iletiyi gönderir.

Daha fazla bilgi için [Azure Monitor'daki Syslog veri kaynaklarına](../azure-monitor/platform/data-sources-syslog.md)bakın.

> [!NOTE]
> - Aracı, birden çok kaynaktan günlükleri toplayabilir, ancak özel proxy makinesine yüklenmesi gerekir.
> - Aynı VM'de hem CEF hem de Syslog için bağlayıcıları desteklemek istiyorsanız, verileri çoğaltmayı önlemek için aşağıdaki adımları gerçekleştirin:
>    1. [CEF'inizi bağlamak](connect-common-event-format.md)için yönergeleri izleyin.
>    2. Syslog verilerini bağlamak için **Ayarlar** > **Çalışma Alanı ayarları** > **Gelişmiş ayarlar** > **Veri** > **Syslog'a** gidin ve Tesisleri ve önceliklerini CEF yapılandırmanızda kullandığınız tesis ve özelliklerle aynı olmayacak şekilde ayarlayın. <br></br>**Makinelerime aşağıda uygula yapılandırmasını**seçerseniz, bu ayarlar bu çalışma alanına bağlı tüm VM'lere uygulanır.


## <a name="connect-your-syslog-appliance"></a>Syslog cihazınızı bağlayın

1. Azure Sentinel'de **Veri bağlayıcılarını** seçin ve ardından **Syslog** bağlayıcısını seçin.

2. **Syslog** bıçağında **Açık bağlayıcı sayfasını**seçin.

3. Linux aracısını yükleyin:
    
    - Linux sanal makineniz Azure'daysa, **Azure Linux sanal makinesine karşı indir ve yükle aracıyı**seçin. Sanal **makineler** bıçağında, aracıyı yüklemek için sanal makineleri seçin ve sonra **Bağlan'ı**tıklatın.
    - Linux makineniz Azure'da değilse, **Linux Azure olmayan makineye karşı İndir ve yükle aracısını**seçin. Direct **agent** blade'de, **LINUX İçİn İnDİr VE ONBOARD AGENT** komutunu kopyalayın ve bilgisayarınızda çalıştırın. 
    
   > [!NOTE]
   > Bu bilgisayarların güvenlik ayarlarını kuruluşunuzun güvenlik ilkesine göre yapılandırdığınızdan emin olun. Örneğin, ağ ayarlarını kuruluşunuzun ağ güvenlik ilkesiyle uyumlu olacak şekilde yapılandırabilir ve daemon'daki bağlantı noktalarını ve protokollerini güvenlik gereksinimleriyle uyumlu olacak şekilde değiştirebilirsiniz.

4. **Çalışma alanı gelişmiş ayarlar yapılandırmanızı aç'ı**seçin.

5. Gelişmiş **ayarlar** bıçağında **Data** > **Syslog'u**seçin. Sonra konektör için toplamak için tesisleri ekleyin.
    
    Syslog cihazınızın günlük başlıklarına dahil olduğu tesisleri ekleyin. Bu yapılandırmayı **Syslog-d** `/etc/rsyslog.d/security-config-omsagent.conf` klasörünüzde ve **r-Syslog'da** `/etc/syslog-ng/security-config-omsagent.conf`görebilirsiniz.
    
    Eğer topladığınız verilerle anormal SSH giriş algılama kullanmak istiyorsanız, **auth** ve **authpriv**ekleyin. Ek ayrıntılar için [aşağıdaki bölüme](#configure-the-syslog-connector-for-anomalous-ssh-login-detection) bakın.

6. İzlemek istediğiniz tüm tesisleri eklediyseniz ve her biri için önem seçenekleri ayarladığınızda, **makinelerime aşağıdaki yapılandırmayı uygula**onay kutusunu seçin.

7. **Kaydet'i**seçin. 

8. Syslog cihazınızda, belirttiğiniz tesisleri gönderdiğinizden emin olun.

9. Syslog günlükleri için Azure Monitor'daki ilgili şemayı kullanmak için **Syslog'u**arayın.

10. Syslog iletilerinizi ayrıştırmak için [Azure Monitor günlük sorgularında işlevleri kullanma'da](../azure-monitor/log-query/functions.md) açıklanan Kusto işlevini kullanabilirsiniz. Daha sonra bunları yeni bir veri türü olarak kullanmak üzere yeni bir Log Analytics işlevi olarak kaydedebilirsiniz.

### <a name="configure-the-syslog-connector-for-anomalous-ssh-login-detection"></a>Anormal SSH giriş algılaması için Syslog konektörünü yapılandırın

> [!IMPORTANT]
> Anormal SSH giriş algılama şu anda genel önizleme de.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Azure Sentinel, anormal Secure Shell (SSH) giriş etkinliğini belirlemek için sislog verilerine makine öğrenimi (ML) uygulayabilir. Senaryolar şunlardır:

- İmkansız seyahat – iki başarılı oturum açma olayı, iki oturum açma olayının zaman dilimi içinde ulaşılması imkansız olan iki konumdan meydana geldiğinde.
- Beklenmeyen konum – başarılı bir oturum açma olayının gerçekleştiği yer şüphelidir. Örneğin, konum son zamanlarda görülmedi.
 
Bu algılama, Syslog veri bağlayıcısının belirli bir yapılandırmasını gerektirir: 

1. Önceki yordamda adım 5 için, hem **auth** hem de **authpriv** izlemek için tesisler olarak seçilmiş olduğundan emin olun. Önem derecesi seçenekleri için varsayılan ayarları tutun, böylece hepsi seçili. Örnek:
    
    > [!div class="mx-imgBorder"]
    > ![Anormal SSH giriş tespiti için gerekli tesisler](./media/connect-syslog/facilities-ssh-detection.png)

2. Syslog bilgilerinin toplanması için yeterli zaman bekleyin. Ardından, **Azure Sentinel - Günlükler adresine**gidin ve aşağıdaki sorguyu kopyalayıp yapıştırın:
    
        Syslog |  where Facility in ("authpriv","auth")| extend c = extract( "Accepted\\s(publickey|password|keyboard-interactive/pam)\\sfor ([^\\s]+)",1,SyslogMessage)| where isnotempty(c) | count 
    
    Gerekirse **Zaman aralığını** değiştirin ve **Çalıştır'ı**seçin.
    
    Elde edilen sayı sıfırsa, bağlayıcının yapılandırmasını onaylayın ve izlenen bilgisayarların sorgunuz için belirttiğiniz süre için başarılı giriş etkinliği olduğunu onaylayın.
    
    Elde edilen sayım sıfırdan büyükse, sislog verileriniz anormal SSH giriş algılaması için uygundur. Bu algılamayı **Analytics** >  **Rule şablonlarından** > **(Önizleme) Anormal SSH Giriş Algılama'dan**etkinleştirin.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Syslog şirket içi cihazları Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)

