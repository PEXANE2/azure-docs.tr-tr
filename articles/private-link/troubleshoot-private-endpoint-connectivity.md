---
title: Azure Özel Uç Nokta bağlantı sorunlarını giderme
description: Özel uç nokta bağlantısını tanılamak için adım adım kılavuz
services: private-endpoint
documentationcenter: na
author: rdhillon
manager: narayan
editor: ''
ms.service: private-link
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2020
ms.author: rdhillon
ms.openlocfilehash: fcc482e6231bbd925fd500a37989052765dede58
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77538543"
---
# <a name="troubleshoot-azure-private-endpoint-connectivity-problems"></a>Azure Özel Uç Nokta bağlantı sorunlarını giderme

Bu makalede, Azure Özel Bitiş Noktası bağlantı kurulumunuzu doğrulamak ve tanılamak için adım adım kılavuzluyat sağlanmaktadır.

Azure Private Endpoint, sizi özel ve güvenli bir şekilde özel bir bağlantı hizmetine bağlayan bir ağ arabirimidir. Bu çözüm, sanal ağınızdan Azure hizmet kaynaklarınıza özel bağlantı sağlayarak Azure'da iş yüklerinizi güvence altına almanıza yardımcı olur. Bu çözüm, bu hizmetleri sanal ağınıza etkin bir şekilde getirir.

Private Endpoint ile kullanılabilen bağlantı senaryoları şunlardır:

- Aynı bölgeden sanal ağ
- Bölgesel olarak bakılmış sanal ağlar
- Küresel olarak eşlenen sanal ağlar
- VPN veya Azure ExpressRoute devreleri üzerinden müşteri şirket içi

## <a name="diagnose-connectivity-problems"></a>Bağlantı sorunlarını tanılama 

Özel bitiş noktası kurulumunuzla bağlantı sorunlarını çözmek için tüm olağan yapılandırmaların beklendiği gibi olduğundan emin olmak için bu adımları gözden geçirin.

1. Kaynağa göz atarak Özel Bitiş Noktası yapılandırmasını gözden geçirin.

    a. Özel **Bağlantı Merkezi'ne**gidin.

      ![Özel Bağlantı Merkezi](./media/private-endpoint-tsg/private-link-center.png)

    b. Sol bölmede Özel **uç noktaları**seçin.
    
      ![Özel uç noktalar](./media/private-endpoint-tsg/private-endpoints.png)

    c. Tanılamak istediğiniz özel bitiş noktasını filtreleyin ve seçin.

    d. Sanal ağı ve DNS bilgilerini gözden geçirin.
     - Bağlantı durumunun **Onaylandığını**doğrulayın.
     - VM'nin özel uç noktaları barındıran sanal ağa bağlantı verdiğinden emin olun.
     - FQDN bilgilerinin (kopya) ve Özel IP adresinin atanmış olup olmadığını kontrol edin.
    
       ![Sanal ağ ve DNS yapılandırması](./media/private-endpoint-tsg/vnet-dns-configuration.png)
    
1. Verilerin [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp

    a. Özel bitiş noktası **kaynağında, Monitör'ü**seçin.
     - **Veri İçini** veya **Veri Çıkış'ı**seçin. 
     - Özel bitiş noktasına bağlanmaya çalıştığınızda verilerin akıp akmayacak olmadığını görün. Yaklaşık 10 dakikalık bir gecikme bekleyin.
    
       ![Özel uç nokta telemetrisini doğrulama](./media/private-endpoint-tsg/private-endpoint-monitor.png)

1.  Azure Ağ İzleyicisi'nden **VM Bağlantısı sorun giderme işini** kullanın.

    a. VM istemcisini seçin.

    b. **Bağlantı sorun giderme**seçeneğini belirleyin ve ardından **Giden bağlantılar** sekmesini seçin.
    
      ![Ağ İzleyicisi - Giden bağlantıları test edin](./media/private-endpoint-tsg/network-watcher-outbound-connection.png)
    
    c. **Ayrıntılı bağlantı izleme için Ağ İzleyicisini Kullan'ı**seçin.
    
      ![Ağ İzleyicisi - Bağlantı sorun giderme](./media/private-endpoint-tsg/network-watcher-connection-troubleshoot.png)

    d. **FQDN ile Test'i**seçin.
     - Özel bitiş noktası kaynağından FQDN yapıştırın.
     - Bir bağlantı noktası sağlayın. Genellikle, Azure Depolama veya Azure Cosmos DB için 443 ve SQL için 1336 kullanın.

    e. **Test'i**seçin ve test sonuçlarını doğrulayın.
    
      ![Ağ İzleyicisi - Test sonuçları](./media/private-endpoint-tsg/network-watcher-test-results.png)
    
        
1. Test sonuçlarından DNS çözünürlüğü, özel bitiş noktasına atanmış aynı özel IP adresine sahip olmalıdır.

    a. DNS ayarları yanlışsa aşağıdaki adımları izleyin:
     - Özel bir bölge kullanıyorsanız: 
       - İstemci VM sanal ağının özel bölgeyle ilişkili olduğundan emin olun.
       - Özel DNS bölge kaydının var olup olmadığını denetleyin. Eğer yoksa, yarat.
     - Özel DNS kullanıyorsanız:
       - Özel DNS ayarlarınızı gözden geçirin ve DNS yapılandırmasının doğru olduğunu doğrulayın.
       Kılavuz için [bkz: Özel bitiş noktası genel bakışı: DNS yapılandırması.](https://docs.microsoft.com/azure/private-link/private-endpoint-overview#dns-configuration)

    b. Ağ güvenlik grupları (NSG' ler) veya kullanıcı tanımlı yollar nedeniyle bağlantı başarısız oluyorsa:
     - NSG giden kuralları gözden geçirin ve trafiğe izin vermek için uygun giden kuralları oluşturun.
    
       ![NSG giden kurallar](./media/private-endpoint-tsg/nsg-outbound-rules.png)

1. Bağlantı sonuçları doğrulanmışsa, bağlantı sorunu uygulama katmanındaki sırlar, belirteçler ve parolalar gibi diğer yönleriyle ilgili olabilir.
   - Bu durumda, özel bitiş noktası ile ilişkili özel bağlantı kaynağıyapılandırmasını gözden geçirin. Daha fazla bilgi için [Azure Özel Bağlantı sorun giderme kılavuzuna](troubleshoot-private-link-connectivity.md)bakın.

1. Sorununuzun hala çözülmemiş olması ve bağlantı sorunu hala varsa [Azure Destek](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) ekibine başvurun.

## <a name="next-steps"></a>Sonraki adımlar

 * [Güncelleştirilmiş alt ağda özel bir bitiş noktası oluşturma (Azure portalı)](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal)
 * [Azure Özel Bağlantı sorun giderme kılavuzu](troubleshoot-private-link-connectivity.md)
