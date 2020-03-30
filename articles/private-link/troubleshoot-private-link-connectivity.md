---
title: Azure Özel Bağlantı hizmeti bağlantı sorunlarını giderme
description: Özel bağlantı bağlantısını tanılamak için adım adım kılavuz
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
ms.openlocfilehash: 1e5253d617c87d5869cebc817da6d265ebfdfa7e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77539476"
---
# <a name="troubleshoot-azure-private-link-connectivity-problems"></a>Azure Özel Bağlantı hizmeti bağlantı sorunlarını giderme

Bu makalede, Azure Özel Bağlantı kurulumunuz için bağlantı nın doğrulanması ve tanılanması için adım adım kılavuzluyat sağlanmaktadır.

Azure Özel Bağlantı ile Azure Platformu'na hizmet (PaaS) hizmetleri olarak erişebilirsiniz (Azure Depolama, Azure Cosmos DB ve Azure SQL Veritabanı gibi) ve Azure barındırılan müşteri veya iş ortağı hizmetlerine sanal ağınızdaki özel bir bitiş noktası üzerinden erişebilirsiniz. Sanal ağınızla hizmet arasındaki trafik, genel internetten açığa geçişi ortadan kaldıran Microsoft omurga ağı üzerinden geçer. Ayrıca sanal ağınızda kendi özel bağlantı hizmetinizi oluşturabilir ve müşterilerinize özel olarak sunabilirsiniz.

Özel Bağlantı erişimi için Azure Yük Bakiyeleyicisinin Standart katmanının arkasında çalışan hizmetinizi etkinleştirebilirsiniz. Hizmetinizin tüketicileri sanal ağları nın içinde özel bir bitiş noktası oluşturabilir ve özel olarak erişmek için bu hizmete eşleyebilir.

Private Link ile kullanılabilen bağlantı senaryoları şunlardır:

- Aynı bölgeden sanal ağ
- Bölgesel olarak bakılmış sanal ağlar
- Küresel olarak eşlenen sanal ağlar
- VPN veya Azure ExpressRoute devreleri üzerinden müşteri şirket içi

## <a name="deployment-troubleshooting"></a>Dağıtım sorun giderme

Özel bağlantı hizmetiniz için seçtiğiniz alt ağdan kaynak IP adresini seçemediğiniz sorun giderme durumları için [özel bağlantı hizmetindeki ağ ilkelerini devre dışı bırakma](https://docs.microsoft.com/azure/private-link/disable-private-link-service-network-policy) hakkındaki bilgileri gözden geçirin.

Kaynak IP adresini seçtiğiniz alt ağ için **privateLinkServiceNetworkPolicies** ayarı devre dışı bırakıldığından emin olun.

## <a name="diagnose-connectivity-problems"></a>Bağlantı sorunlarını tanılama

Özel bağlantı kurulumunuzla ilgili bağlantı sorunları yla karşılaşırsanız, tüm olağan yapılandırmaların beklendiği gibi olduğundan emin olmak için bu adımları gözden geçirin.

1. Kaynağa göz atarak Özel Bağlantı yapılandırmasını gözden geçirin.

    a. Özel **Bağlantı Merkezi'ne**gidin.

      ![Özel Bağlantı Merkezi](./media/private-link-tsg/private-link-center.png)

    b. Sol bölmede Özel **bağlantı hizmetlerini**seçin.

      ![Özel bağlantı hizmetleri](./media/private-link-tsg/private-link-service.png)

    c. Tanılamak istediğiniz özel bağlantı hizmetini filtreleyin ve seçin.

    d. Özel bitiş noktası bağlantılarını gözden geçirin.
     - Bağlantı aradığınız özel bitiş noktasının **Onaylanmış** bağlantı durumuyla listelenmiş olduğundan emin olun.
     - Durum **Beklemedeyse,** seçin ve onaylayın.

       ![Özel uç nokta bağlantıları](./media/private-link-tsg/pls-private-endpoint-connections.png)

     - Adı seçerek bağlandığınız özel bitiş noktasına gidin. Bağlantı durumunun **Onaylandı**olarak gösterdiğinden emin olun.

       ![Özel uç nokta bağlantısına genel bakış](./media/private-link-tsg/pls-private-endpoint-overview.png)

     - Her iki taraf da onaylandıktan sonra bağlantıyı yeniden deneyin.

    e. **Özellikler** sekmesinde **Genel Bakış** sekmesinde **Diğer Adları** ve **Kaynak Kimliği'ni** gözden geçirin.
     - **Diğer Ad** ve **Kaynak Kimliği** bilgilerinin, bu hizmetiçin özel bir bitiş noktası oluşturmak için kullandığınız Diğer **Ad** ve **Kaynak Kimliği** ile eşleştiğinden emin olun.

       ![Diğer Ad bilgilerini doğrula](./media/private-link-tsg/pls-overview-pane-alias.png)

       ![Kaynak Kimliği bilgilerini doğrula](./media/private-link-tsg/pls-properties-pane-resourceid.png)

    f. **Genel Bakış** sekmesinde **Görünürlük** bilgilerini gözden geçirin.
     - Aboneliğinizin **Görünürlük** kapsamına düştüğünden emin olun.

       ![Görünürlük bilgilerini doğrula](./media/private-link-tsg/pls-overview-pane-visibility.png)

    g. **Genel Bakış** sekmesinde **Yük bakiyesi** bilgilerini gözden geçirin.
     - Yük dengeleyici bağlantısını seçerek yük dengeleyicisine gidebilirsiniz.

       ![Yük dengeleyici bilgilerini doğrula](./media/private-link-tsg/pls-overview-pane-ilb.png)

     - Yük dengeleyici ayarlarının beklentilerinize göre yapılandırıldığından emin olun.
       - **Frontend IP yapılandırmasına**inceleyin.
       - **Arka uç havuzlarını**gözden geçirin.
       - Yük dengeleme kurallarını gözden **geçirin.**

       ![Yük dengeleyici özelliklerini doğrulama](./media/private-link-tsg/pls-ilb-properties.png)

     - Yük dengeleyicisinin önceki ayarlara göre çalıştığından emin olun.
       - Yük bakiyesi arka uç havuzunun bulunduğu alt ağ dışında herhangi bir alt ağda bir VM seçin.
       - Önceki VM'den yük dengeleyiciön ucuna erişebİlIn.
       - Bağlantı, yük dengeleme kurallarına göre arka uç havuzuna ulaştırırsa, yük dengeleyiciniz çalışır durumdadır.
       - Verilerin yük dengeleyicisi aracılığıyla akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp aktığını görmek için Azure Monitor aracılığıyla yük dengeleyici ölçümünü de inceleyebilirsiniz.

1. Verilerin [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp akıp

    a. Özel bağlantı hizmeti kaynağında **Ölçümler'i**seçin.
     - **Bayt Lar Veya** **Bayt Lar Çıkış'ı**seçin.
     - Özel bağlantı hizmetine bağlanmaya çalıştığınızda verilerin akıp akmayacak olmadığını görün. Yaklaşık 10 dakikalık bir gecikme bekleyin.

       ![Özel bağlantı hizmeti ölçümlerini doğrulama](./media/private-link-tsg/pls-metrics.png)

1. Sorununuzun hala çözülmemiş olması ve bağlantı sorunu hala varsa [Azure Destek](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) ekibine başvurun.

## <a name="next-steps"></a>Sonraki adımlar

 * [Özel bağlantı hizmeti (CLI) oluşturma](https://docs.microsoft.com/azure/private-link/create-private-link-service-cli)
 * [Azure Özel Bitiş Noktası sorun giderme kılavuzu](troubleshoot-private-endpoint-connectivity.md)
