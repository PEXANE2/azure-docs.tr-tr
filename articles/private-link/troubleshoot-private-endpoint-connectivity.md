---
title: Azure Özel Uç Nokta bağlantı sorunlarını giderme
description: Özel uç nokta bağlantısını tanılamaya yönelik adım adım yönergeler
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77538543"
---
# <a name="troubleshoot-azure-private-endpoint-connectivity-problems"></a>Azure Özel Uç Nokta bağlantı sorunlarını giderme

Bu makalede, Azure özel uç nokta bağlantısı kurulumunuzu doğrulamak ve tanılamak için adım adım yönergeler sağlanmaktadır.

Azure özel uç noktası, özel bir bağlantı hizmetine özel ve güvenli bir şekilde bağlanan bir ağ arabirimidir. Bu çözüm, sanal ağınızdan Azure hizmet kaynaklarınıza özel bağlantı sağlayarak iş yüklerinizi Azure 'da korumanıza yardımcı olur. Bu çözüm bu hizmetleri sanal ağınıza etkin bir şekilde getirir.

Özel uç nokta ile kullanılabilen bağlantı senaryoları şunlardır:

- Aynı bölgedeki sanal ağ
- Region, eşlenen sanal ağlar
- Genel eşlenmiş sanal ağlar
- VPN veya Azure ExpressRoute devreleri üzerinden şirket içi müşteri

## <a name="diagnose-connectivity-problems"></a>Bağlantı sorunlarını Tanıla 

Tüm olağan yapılandırmaların, Özel uç nokta kurulumlarınızla bağlantı sorunlarını gidermek için beklendiğinden emin olmak için bu adımları gözden geçirin.

1. Kaynağa göz atarak özel uç nokta yapılandırmasını gözden geçirin.

    a. **Özel bağlantı merkezine**gidin.

      ![Özel bağlantı merkezi](./media/private-endpoint-tsg/private-link-center.png)

    b. Sol bölmede **Özel uç noktalar**' ı seçin.
    
      ![Özel uç noktalar](./media/private-endpoint-tsg/private-endpoints.png)

    c. Tanılama yapmak istediğiniz özel uç noktayı filtreleyin ve seçin.

    d. Sanal ağ ve DNS bilgilerini gözden geçirin.
     - Bağlantı durumunun **onaylandığını**doğrulayın.
     - VM 'nin özel uç noktaları barındıran sanal ağa bağlantısı olduğundan emin olun.
     - FQDN bilgilerinin (kopya) ve özel IP adresinin atandığından emin olun.
    
       ![Sanal ağ ve DNS yapılandırması](./media/private-endpoint-tsg/vnet-dns-configuration.png)
    
1. Verilerin akan olup olmadığını görmek için [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview) 'yi kullanın.

    a. Özel uç nokta kaynağında, **İzle**' yi seçin.
     - **Veri Içindeki verileri** veya **verileri**seçin. 
     - Özel uç noktaya bağlanmaya çalıştığınızda verilerin akışa alındığını görün. Yaklaşık 10 dakikalık bir gecikme süresi bekler.
    
       ![Özel uç nokta telemetrisini doğrulama](./media/private-endpoint-tsg/private-endpoint-monitor.png)

1.  Azure ağ Izleyicisi 'nde **VM bağlantısı sorunlarını giderme** .

    a. İstemci sanal makinesini seçin.

    b. **Bağlantı sorunlarını giderin**' ı seçin ve **giden bağlantılar** sekmesini seçin.
    
      ![Ağ Izleyicisi-giden bağlantıları test etme](./media/private-endpoint-tsg/network-watcher-outbound-connection.png)
    
    c. **Ayrıntılı bağlantı izleme Için ağ Izleyicisi kullan**' ı seçin.
    
      ![Ağ Izleyicisi-bağlantı sorunlarını giderme](./media/private-endpoint-tsg/network-watcher-connection-troubleshoot.png)

    d. **FQDN Ile test**' i seçin.
     - FQDN 'yi özel uç nokta kaynağından yapıştırın.
     - Bir bağlantı noktası belirtin. Genellikle, Azure depolama için 443 veya SQL için Azure Cosmos DB ve 1336 kullanın.

    e. **Test**' i seçin ve test sonuçlarını doğrulayın.
    
      ![Ağ Izleyicisi-test sonuçları](./media/private-endpoint-tsg/network-watcher-test-results.png)
    
        
1. Test sonuçlarından DNS çözümlemesi, Özel uç noktaya atanmış aynı özel IP adresine sahip olmalıdır.

    a. DNS ayarları yanlışsa, şu adımları izleyin:
     - Özel bir bölge kullanıyorsanız: 
       - İstemci VM sanal ağının özel Bölgeyle ilişkili olduğundan emin olun.
       - Özel DNS bölgesi kaydının mevcut olup olmadığını denetleyin. Mevcut değilse, oluşturun.
     - Özel DNS kullanıyorsanız:
       - Özel DNS ayarlarınızı gözden geçirin ve DNS yapılandırmasının doğru olduğunu doğrulayın.
       Rehberlik için bkz. [Özel uç noktaya genel bakış: DNS yapılandırması](https://docs.microsoft.com/azure/private-link/private-endpoint-overview#dns-configuration).

    b. Ağ güvenlik grupları (NSG 'ler) veya Kullanıcı tanımlı rotalar nedeniyle bağlantı başarısız olursa:
     - NSG giden kurallarını gözden geçirin ve trafiğe izin vermek için uygun giden kuralları oluşturun.
    
       ![NSG giden kuralları](./media/private-endpoint-tsg/nsg-outbound-rules.png)

1. Bağlantıda sonuçlar doğrulandıktan sonra bağlantı sorunu, uygulama katmanındaki gizli diziler, belirteçler ve parolalar gibi diğer yönlerle ilgili olabilir.
   - Bu durumda, Özel uç noktayla ilişkili özel bağlantı kaynağının yapılandırmasını gözden geçirin. Daha fazla bilgi için bkz. [Azure özel bağlantısı sorun giderme kılavuzu](troubleshoot-private-link-connectivity.md).

1. Sorununuz hala çözülmedi ve bir bağlantı sorunu hala varsa [Azure destek](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) ekibine başvurun.

## <a name="next-steps"></a>Sonraki adımlar

 * [Güncelleştirilmiş alt ağda özel bir uç nokta oluşturma (Azure portal)](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal)
 * [Azure özel bağlantısı sorun giderme kılavuzu](troubleshoot-private-link-connectivity.md)
