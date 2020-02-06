---
title: Azure özel bağlantı bağlantı sorunlarını giderme
description: Özel bağlantı bağlantısını tanılamaya yönelik adım adım yönergeler
services: private-link
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
ms.openlocfilehash: 667fa1c85c63ffb87e49c4bf99112f57d0c85a72
ms.sourcegitcommit: f0f73c51441aeb04a5c21a6e3205b7f520f8b0e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77031926"
---
# <a name="troubleshoot-private-link-service-connectivity-problems"></a>Özel bağlantı hizmeti bağlantı sorunlarını giderme

Bu kılavuzda, özel bağlantı hizmeti kuruluma yönelik bağlantıyı doğrulamak ve tanılamak için adım adım yönergeler sağlanmaktadır. 

Azure özel bağlantısı, Azure PaaS hizmetlerine (örn. Azure depolama, Azure Cosmos DB ve SQL veritabanı) ve Azure 'da barındırılan müşteri/iş ortağı hizmetlerine sanal ağınızdaki özel bir uç nokta üzerinden erişmenizi sağlar. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Ayrıca, kendi özel bağlantı hizmetinizi sanal ağınızda (VNet) oluşturabilir ve müşterilerinize özel olarak iletebilirsiniz. 

Özel bağlantı erişimi için Azure Standart Load Balancer 'in arkasında çalışan hizmetinizi etkinleştirebilirsiniz. Hizmetinizin tüketicileri, sanal ağı içinde özel bir uç nokta oluşturabilir ve bu hizmete özel olarak erişmek için bu hizmetle eşlenir.

Özel bağlantı hizmeti ile kullanılabilen bağlantı senaryoları şunlardır
- aynı bölgedeki sanal ağ 
- Region, eşlenen sanal ağlar
- Genel eşlenmiş sanal ağlar
- VPN veya Express Route devreleri üzerinden şirket içi müşteri

## <a name="deployment-troubleshooting"></a>Dağıtım sorunlarını giderme

Özel bağlantı hizmetiniz için tercih ettiğiniz alt ağdan kaynak IP adresini seçmemekte olduğunuz, sorun giderme durumları için [özel bağlantı hizmetinde ağ Ilkelerini devre dışı bırakma](https://docs.microsoft.com/azure/private-link/disable-private-link-service-network-policy) hakkındaki bilgileri gözden geçirin.

Kaynak IP adresini seçtiğiniz alt ağ için **Privatelinkservicenetworkpolicies** ayarının devre dışı bırakıldığından emin olun.

## <a name="diagnosing-connectivity-problems"></a>Bağlantı sorunlarını tanılama

Özel bağlantı kurulumunuzda bağlantı sorunlarıyla karşılaşıyorsanız, tüm olağan yapılandırmaların beklenen şekilde olduğundan emin olmak için aşağıda listelenen adımları izleyin.

1. Kaynağa göz atarak özel bağlantı hizmeti yapılandırmasını gözden geçirin 

    a) **özel bağlantı merkezine** git

      ![Özel bağlantı merkezi](./media/private-link-tsg/private-link-center.png)

    b) sol gezinti bölmesinden **özel bağlantı Hizmetleri** ' ni seçin

      ![Özel bağlantı Hizmetleri](./media/private-link-tsg/private-link-service.png)

    c) tanılama yapmak istediğiniz özel bağlantı hizmetini filtreleyip seçin

    d) özel uç nokta bağlantılarını gözden geçirin
     - Bağlantısını Aradığınız özel uç noktanın **onaylanan** bağlantı durumuyla listelendiğinden emin olun. 
     - **Bekliyor**ise, seçin ve onaylayın. 

       ![Özel uç nokta bağlantıları](./media/private-link-tsg/pls-private-endpoint-connections.png)

     - Ada tıklayarak, bağlandığınız özel uç noktaya gidin. Bağlantı durumunun **onaylanan**göründüğünden emin olun.

       ![Özel uç nokta bağlantısına genel bakış](./media/private-link-tsg/pls-private-endpoint-overview.png)

     - Her iki taraf onaylandıktan sonra bağlantıyı yeniden deneyin.

    e) Özellikler sekmesinden Genel Bakış sekmesi ve **kaynak kimliği** ' nden **diğer adı** gözden geçirin 
     - **Diğer ad/kaynak kimliğinin** , bu hizmete özel bir uç nokta oluşturmak Için kullandığınız **diğer ad/kaynak kimliğiyle** eşleştiğinden emin olun. 

       ![Diğer adı doğrula](./media/private-link-tsg/pls-overview-pane-alias.png)

       ![Kaynak KIMLIĞINI doğrula](./media/private-link-tsg/pls-properties-pane-resourceid.png)

    f) görünürlüğü gözden geçirme (genel bakış bölümü) bilgileri
     - Aboneliğinizin **görünürlük** kapsamı altında olduğundan emin olun

       ![Görünürlüğü doğrula](./media/private-link-tsg/pls-overview-pane-visibility.png)

    g) Load Balancer (genel bakış) bilgilerini gözden geçirin
     - Yük dengeleyici bağlantısına tıklayarak yük dengeleyiciye gidebilirsiniz

       ![Load Balancer doğrula](./media/private-link-tsg/pls-overview-pane-ilb.png)

     - Load Balancer ayarlarının beklentileriniz uyarınca yapılandırıldığından emin olun
       - Ön uç IP yapılandırmasını gözden geçir
       - Arka uç havuzlarını gözden geçirme
       - Yük Dengeleme kurallarını gözden geçirme

       ![Load Balancer özelliklerini doğrulama](./media/private-link-tsg/pls-ilb-properties.png)

     - Load Balancer, yukarıdaki ayarlara göre çalıştığından emin olun
       - Load Balancer arka uç havuzunun kullanılabildiği alt ağ dışında herhangi bir alt ağda bir VM seçin
       - Yukarıdaki VM 'den yük dengeleyici ön ucuna erişmeyi deneyin
       - Bağlantı, arka uç havuzunda yük dengeleme kurallarına göre yapılır, yük dengeleyici çalışır durumda olur
       - Ayrıca, verilerin yük dengeleyiciye akışının akışını görmek için Azure Izleyici aracılığıyla Load Balancer ölçümünü gözden geçirebilirsiniz

2. Verilerin akışını gözden geçirmek için [**Azure izleyici**](https://docs.microsoft.com/azure/azure-monitor/overview) 'yi kullanma

    a) özel bağlantı hizmeti kaynağında **ölçümler** ' i seçin.
     - **Bayt/gelen** veya **Giden bayt** seçin
     - Özel bağlantı hizmetine bağlanmaya çalışırken İnceleme verileri akar. Yaklaşık 10 dakikalık bir gecikme süresi beklenir.

       ![Özel bağlantı hizmeti ölçümlerini doğrulama](./media/private-link-tsg/pls-metrics.png)

3. Sorununuz çözülmedi ve bağlantı sorunu hala varsa [Azure destek](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) ekibine başvurun. 

## <a name="next-steps"></a>Sonraki adımlar

 * [Özel bağlantı hizmeti (CLı) oluşturma](https://docs.microsoft.com/azure/private-link/create-private-link-service-cli)

 * [Özel uç nokta sorun giderme kılavuzu](https://docs.microsoft.com/azure/private-link/private-endpoint-connectivity-troubleshooting)
