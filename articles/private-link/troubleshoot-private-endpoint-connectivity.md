---
title: Azure özel uç nokta bağlantı sorunlarını giderme
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
ms.openlocfilehash: 1c3fc4ebaffe2347a9c46b626b94042d1c9cfe11
ms.sourcegitcommit: f0f73c51441aeb04a5c21a6e3205b7f520f8b0e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77031861"
---
# <a name="troubleshoot-private-endpoint-connectivity-problems"></a>Özel uç nokta bağlantı sorunlarını giderme

Bu kılavuzda, Özel uç nokta bağlantısı kurulumunuzu doğrulamak ve tanılamak için adım adım yönergeler sağlanmaktadır. 

Azure özel uç noktası, özel bir bağlantı hizmetine özel ve güvenli bir şekilde bağlanan bir ağ arabirimidir. Bu çözüm, sanal ağınızdan Azure hizmet kaynaklarınıza özel bağlantı sağlayarak iş yüklerinizi Azure 'da korumanıza yardımcı olur. Bu, bu hizmetleri sanal ağınıza etkin bir şekilde getiriyor. 

Özel uç noktalarla kullanılabilen bağlantı senaryoları şunlardır 
- aynı bölgedeki sanal ağ 
- Region, eşlenen sanal ağlar
- Genel eşlenmiş sanal ağlar
- VPN veya Express Route devreleri üzerinden şirket içi müşteri

## <a name="diagnosing-connectivity-problems"></a>Bağlantı sorunlarını tanılama 
Tüm olağan yapılandırmaların, Özel uç nokta kurulumunuzda bağlantı sorunlarını gidermek için beklenen şekilde olduğundan emin olmak için aşağıda listelenen adımları izleyin.

1. Kaynağa göz atarak özel uç nokta yapılandırmasını gözden geçirin 

    a) **özel bağlantı merkezine** git

      ![Özel bağlantı merkezi](./media/private-endpoint-tsg/private-link-center.png)

    b) sol gezinti bölmesinden özel uç noktaları seçin
    
      ![Özel uç noktalar](./media/private-endpoint-tsg/private-endpoints.png)

    c), tanımak istediğiniz özel uç noktayı filtreleyip seçin

    d) sanal ağı ve DNS bilgilerini gözden geçirin
    
     - Bağlantı durumunun **onaylandığını** doğrula
     - VM 'nin özel uç noktaları barındıran VNet 'e bağlantısı olduğundan emin olun
     - FQDN bilgileri (kopya) ve özel IP adresi atandı
    
       ![VNet ve DNS yapılandırması](./media/private-endpoint-tsg/vnet-dns-configuration.png)    
    
2. Verilerin akışını gözden geçirmek için [**Azure izleyici**](https://docs.microsoft.com/azure/azure-monitor/overview) 'yi kullanma

    a) özel uç nokta kaynağında **izleyici** ' yi seçin.
     - Verileri veya veri aşımını seçin ve özel uç noktaya bağlanmaya çalışırken verilerin akışını inceleyin. Yaklaşık 10 dakikalık bir gecikme süresi beklenir.
    
       ![Özel uç nokta telemetrisini doğrulama](./media/private-endpoint-tsg/private-endpoint-monitor.png)

3. **Ağ** IZLEYICISINDEN VM bağlantısı sorunlarını giderme

    a) istemci VM 'sini seçin

    b) **bağlantı sorunlarını giderme** bölümünü seçin, **giden bağlantı** sekmesi
    
      ![Ağ Izleyicisi-giden bağlantıları test etme](./media/private-endpoint-tsg/network-watcher-outbound-connection.png)
    
    c) **ayrıntılı bağlantı izleme Için ağ Izleyicisini kullan** ' ı seçin.
    
      ![Ağ Izleyicisi-bağlantı sorunlarını giderme](./media/private-endpoint-tsg/network-watcher-connection-troubleshoot.png)

    d) **FQDN 'ye göre test** seçin
     - FQDN 'yi özel uç nokta kaynağından Yapıştır
     - Bir bağlantı noktası sağlayın (*genellikle Azure Storage veya COSMOS için 443, SQL için 1336...* )

    e) **Sına** tıklayın ve test sonuçlarını doğrulayın
    
      ![Ağ Izleyicisi-test sonuçları](./media/private-endpoint-tsg/network-watcher-test-results.png)
    
        
4. Test sonuçlarından DNS çözümlemesi, Özel uç noktaya atanmış aynı özel IP adresine sahip olmalıdır

    a) DNS ayarları yanlışsa, aşağıdakileri yapın
     - Özel bölge kullanma: 
       - İstemci VM VNet 'in özel bölge ile ilişkili olduğundan emin olun
       - Özel DNS bölge kaydını gözden geçir, mevcut değilse oluştur
    
     - Özel DNS kullanma:
       - Müşteri DNS ayarlarınızı gözden geçirin ve DNS yapılandırmasının doğru olduğunu doğrulayın.
       Kılavuza yönelik [Özel uç noktaya genel bakış-DNS yapılandırması](https://docs.microsoft.com/azure/private-link/private-endpoint-overview#dns-configuration) bölümüne bakın.

    b) NSG/UDRs nedeniyle bağlantı başarısız olursa
     - NSG giden kurallarını gözden geçirin ve trafiğe izin vermek için uygun giden kuralları oluşturun
    
       ![NSG giden kuralları](./media/private-endpoint-tsg/nsg-outbound-rules.png)

5. Bağlantıda sonuçlar doğrulandıktan sonra bağlantı sorunu, uygulama katmanındaki gizli dizileri, belirteçleri, parolaları gibi diğer yönlerle ilgili olabilir.
   - Bu durumda, Özel uç noktayla ilişkili özel bağlantı kaynağının yapılandırmasını gözden geçirin. [Özel bağlantı sorun giderme kılavuzu](https://docs.microsoft.com/azure/private-link/private-link-connectivity-troubleshooting)' na bakın. 

6. Sorununuz çözülmedi ve bağlantı sorunu hala varsa [Azure destek](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) ekibine başvurun. 

## <a name="next-steps"></a>Sonraki adımlar

 * [Güncelleştirilmiş alt ağda özel bir uç nokta oluşturma (Azure portal)](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal)

 * [Özel bağlantı sorun giderme kılavuzu](https://docs.microsoft.com/azure/private-link/private-link-connectivity-troubleshooting)
