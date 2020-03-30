---
title: Azure Sentinel'de DNS verilerini bağlayın| Microsoft Dokümanlar
description: Azure Sentinel'de DNS verilerini nasıl bağlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 77af84f9-47bc-418e-8ce2-4414d7b58c0c
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/24/2019
ms.author: yelevin
ms.openlocfilehash: 6d43b82ecd13ac5e082d270ee44ce61fef763d2c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588323"
---
# <a name="connect-your-domain-name-server"></a>Etki alanı adı sunucunuzu bağlama

> [!IMPORTANT]
> Azure Sentinel'deki DNS veri bağlayıcısı şu anda genel önizlemededir.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Windows'da çalışan herhangi bir Alan Adı Sunucusu'nü (DNS) Azure Sentinel'e bağlayabilirsiniz. Bu, DNS makinesine bir aracı yüklenerek yapılır. DNS günlüklerini kullanarak, DNS sunucularından analitik ve denetim günlüklerini ve diğer ilgili verileri toplayarak, analiz ederek ve ilişkilendirerek kuruluşunuzun DNS altyapısına ilişkin güvenlik, performans ve işlemlerle ilgili öngörüler elde edebilirsiniz.

DNS günlük bağlantısını etkinleştirdiğinizde şunları yapabilirsiniz:
- Kötü amaçlı alan adlarını çözmeye çalışan istemcileri tanımlama
- Eski kaynak kayıtlarını tanımlama
- Sık sorgulanmış alan adlarını ve konuşkan DNS istemcilerini belirleme
- DNS sunucularında istek yükünü görüntüleme
- Dinamik DNS kayıt hatalarını görüntüleme

## <a name="connected-sources"></a>Bağlı kaynaklar

Aşağıdaki tabloda, bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır:

| **Bağlı kaynak** | **Destek** | **Açıklama** |
| --- | --- | --- |
| [Windows aracıları](../azure-monitor/platform/agent-windows.md) | Evet | Çözüm, Windows aracılarından DNS bilgileri toplar. |
| [Linux aracıları](../azure-monitor/learn/quick-collect-linux-computer.md) | Hayır | Çözüm, doğrudan Linux ajanlarından DNS bilgileri toplamaz. |
| [System Center Operations Manager yönetim grubu](../azure-monitor/platform/om-agents.md) | Evet | Çözüm, bağlı bir Operasyon Yöneticisi yönetim grubundaki aracılardan DNS bilgileri toplar. Operations Manager aracısından Azure Monitor'a doğrudan bağlantı gerekmez. Veriler yönetim grubundan Log Analytics çalışma alanına iletilir. |
| [Azure depolama hesabı](../azure-monitor/platform/collect-azure-metrics-logs.md) | Hayır | Azure depolama çözümü tarafından kullanılmaz. |

### <a name="data-collection-details"></a>Veri toplama ayrıntıları

Çözüm, Log Analytics aracısının yüklü olduğu DNS sunucularından DNS envanteri ve DNS olayla ilgili verileri toplar. DNS sunucularının, bölgelerinin ve kaynak kayıtlarının sayısı gibi stokla ilgili veriler, DNS PowerShell cmdlets çalıştırılarak toplanır. Veriler iki günde bir güncellenir. Etkinlikle ilgili veriler, Windows Server 2012 R2'de geliştirilmiş DNS günlüğü ve tanılama tarafından sağlanan [analitik ve denetim günlüklerinden](https://technet.microsoft.com/library/dn800669.aspx#enhanc) gerçek zamanlı olarak toplanır.


## <a name="connect-your-dns-appliance"></a>DNS cihazınızı bağlayın

1. Azure Sentinel portalında **Veri bağlayıcılarını** seçin ve **DNS (Önizleme)** döşemesini seçin.
1. DNS makineleriniz Azure'daysa:
    1. **Azure Windows sanal makinesinde Yükle aracısı'nı**tıklatın.
    1. Sanal **makineler** listesinde, Azure Sentinel'e aktarmak istediğiniz DNS makinesini seçin. Bunun bir Windows VM olduğundan emin olun.
    1. Bu VM için açılan pencerede **Bağlan'ı**tıklatın.  
    1. **DNS bağlayıcı** penceresinde **Etkinleştir'i** tıklatın. 

2. DNS makineniz Azure VM değilse:
    1. **Azure olmayan makinelerde Yükle aracıyı**tıklatın.
    1. Doğrudan **aracı** penceresinde, **Windows aracısını indir (64 bit)** veya **Windows aracısını (32 bit) indir'i**seçin.
    1. Aracıyı DNS makinenize yükleyin. Çalışma **Alanı Kimliği,** **Birincil anahtar**ve **İkincil anahtarı** kopyalayın ve yükleme sırasında istendiğinde bunları kullanın.

3. DNS günlükleri için Log Analytics'teki ilgili şemayı kullanmak için **DnsEvents'i**arayın.

## <a name="validate"></a>Doğrulama 

Log Analytics'te şema **DnsEvents'i** arayın ve etkinlikler olduğundan emin olun.

## <a name="troubleshooting"></a>Sorun giderme

Arama Sorguları Azure Sentinel'de görünmüyorsa, sorguların düzgün görüntülenmesi için aşağıdaki adımları izleyin:
1. [Sunucularınızdaki DNS Analytics günlüklerini](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn800669(v=ws.11))açın.
2. DNSEvents'in Günlük Analizi toplama listenizde göründüğünden emin olun.
3. Azure [DNS Analytics'i](../azure-monitor/insights/dns-analytics.md)açın.
4. Azure DNS Analytics'te Configuration **altında,** ayarlardan herhangi birini değiştirin, kaydedin, gerekirse geri değiştirin ve sonra yeniden kaydedin.
5. Sorguların görüntülendiğinden emin olmak için Azure DNS analitiğini kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, DNS şirket içi cihazları Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
