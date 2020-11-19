---
title: DDoS konuma telemetrisini görüntüleme ve yapılandırma
description: DDoS koruması telemetrisini görüntülemeyi ve yapılandırmayı öğrenin.
services: ddos-protection
documentationcenter: na
author: yitoh
ms.service: ddos-protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/08/2020
ms.author: yitoh
ms.openlocfilehash: eefb658c689128c1d91858ac906c09e71d05cda6
ms.sourcegitcommit: 230d5656b525a2c6a6717525b68a10135c568d67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94888955"
---
# <a name="view-and-configure-ddos-protection-telemetry"></a>DDoS konuma telemetrisini görüntüleme ve yapılandırma

Azure DDoS koruması standardı, DDoS saldırı analiziyle ayrıntılı saldırı öngörüleri ve görselleştirmeleri sağlar. Sanal ağlarını DDoS saldırılarına karşı koruyan müşteriler, saldırı saldırılarına karşı saldırı ve risk azaltma & raporları aracılığıyla saldırının etkilerini azaltmak için gerçekleştirilen saldırı ve eylemler hakkında ayrıntılı görünürlük sağlar. Zengin telemetri, DDoS saldırısının süresi boyunca ayrıntılı ölçümler dahil olmak üzere Azure Izleyici aracılığıyla sunulur. DDoS koruması tarafından sunulan Azure Izleyici ölçümlerinden herhangi biri için uyarı yapılandırılabilir. Günlüğe kaydetme, Azure Izleme tanılama arabirimi aracılığıyla [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-ddos-protection), splunk (Azure Event Hubs), OMS Log Analytics ve gelişmiş analiz Için Azure depolama ile daha da tümleştirilebilir.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * DDoS koruma ölçümleri için uyarıları yapılandırma
> * DDoS koruması telemetrisini kullanma
> * DDoS risk azaltma ilkelerini görüntüleme
> * Azure Güvenlik Merkezi 'nde DDoS koruma uyarılarını görüntüleme

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
- Bu öğreticideki adımları tamamlayabilmeniz için önce bir [Azure DDoS standart koruma planı](manage-ddos-protection.md)oluşturmanız gerekir.

## <a name="configure-alerts-for-ddos-protection-metrics"></a>DDoS koruma ölçümleri için uyarıları yapılandırma

Azure Izleyici uyarı yapılandırması kullanılarak saldırı sırasında etkin bir risk azaltma olduğunda sizi uyarmak için kullanılabilir DDoS koruma ölçümlerinden herhangi birini seçebilirsiniz. Koşullar karşılandığında, belirtilen adres bir uyarı e-postası alır:

1. Portalın sol tarafındaki **tüm hizmetler** ' i seçin.
2. **Filtre** kutusuna *izleyiciyi* girin. Sonuçlarda **izleyici** göründüğünde seçin.
3. **PAYLAŞıLAN hizmetler** altında **ölçümler** ' i seçin.
4. Kendi değerlerinizi girin veya seçin veya aşağıdaki örnek değerleri girip, kalan Varsayılanları kabul edin ve ardından **Tamam**' ı seçin:

    |Ayar                  |Değer                                                                                               |
    |---------                |---------                                                                                           |
    |Ad                     | _Myddosalert_ yazın.                                                                                |
    |Abonelik             | Uyarılarını almak istediğiniz genel IP adresini içeren aboneliği seçin.        |
    |Kaynak grubu           | Uyarılarını almak istediğiniz genel IP adresini içeren kaynak grubunu seçin.      |
    |Kaynak                 | Uyarıları almak istediğiniz genel IP adresini içeren genel IP adresini seçin. DDoS, bir sanal ağ içindeki kaynaklara atanan genel IP adreslerini izler. Sanal ağda genel IP adresleri olan kaynaklarınız yoksa, önce genel IP adresine sahip bir kaynak oluşturmanız gerekir. Azure App Service ortamları ve Azure VPN Gateway dışında [Azure hizmetleri Için sanal ağda](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services#services-that-can-be-deployed-into-a-virtual-network)listelenen Kaynak Yöneticisi (klasik değil) aracılığıyla dağıtılan tüm KAYNAKLARıN genel IP adresini izleyebilirsiniz. Bu öğreticiye devam etmek için hızlı bir şekilde [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makinesi oluşturabilirsiniz.                   |
    |Ölçüm                   | **DDoS saldırısı altında** öğesini seçin.                                                                |
    |Eşik                | 1- **1** saldırı altında olduğunuz anlamına gelir. **0** saldırı altında olmayan anlamına gelir.                         |
    |Dönem                   | Seçtiğiniz değeri seçin.                                                                   |
    |E-postayla bildir         | Onay kutusunu işaretleyin.                                                                                 |
    |Ek yönetici | Abonelik için bir e-posta sahibi, katkıda bulunan veya okuyucu değilseniz e-posta adresinizi girin. |

    Birkaç dakika içinde saldırı algılama sırasında, Azure Izleyici ölçümlerinden aşağıdaki resme benzer bir e-posta alırsınız:

    ![Saldırı Uyarısı](./media/manage-ddos-protection/ddos-alert.png)


Uyarı doğrulamanızı sağlamak için DDoS saldırısının benzetimini yapmak için bkz. [DDoS algılamayı doğrulama](test-through-simulations.md).

Ayrıca, uyarı oluşturmak için [Web kancaları](../azure-monitor/platform/alerts-webhooks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Logic Apps](../logic-apps/logic-apps-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) yapılandırma hakkında daha fazla bilgi edinebilirsiniz.

## <a name="use-ddos-protection-telemetry"></a>DDoS koruması telemetrisini kullanma

Bir saldırının telemetrisi, Azure Izleyici aracılığıyla gerçek zamanlı olarak sağlanır. Telemetri yalnızca genel bir IP adresinin risk altında olduğu süre boyunca kullanılabilir. Bir saldırının etkilerinin azaltılmadan önce veya sonra telemetri görmezsiniz.

1. Portalın sol tarafındaki **tüm hizmetler** ' i seçin.
2. **Filtre** kutusuna *izleyiciyi* girin. Sonuçlarda **izleyici** göründüğünde seçin.
3. **PAYLAŞıLAN hizmetler** altında **ölçümler**' i seçin.
4. Telemetriyi istediğiniz genel IP adresini içeren **aboneliği** ve **kaynak grubunu** seçin.
5. **Kaynak türü** IÇIN **genel IP adresi** ' ni seçin ve ardından Telemetriyi istediğiniz belirli genel IP adresini seçin.
6. Ekranın sol tarafında bir dizi **kullanılabilir ölçüm** görüntülenir. Bu ölçümler seçildiğinde, genel bakış ekranında **Azure Izleyici ölçümleri grafiğinde** grafiği kaldırılır.
7. **Toplama** türünü **en fazla** olarak seçin

Ölçüm adları farklı paket türleri ve bayt ve paketlerin yanı sıra, her ölçüm üzerinde aşağıdaki gibi temel bir etiket adı vardır:

- **Bırakılan etiket adı** (örneğin, **gönderilen gelen paketler**): DDoS koruma sistemi tarafından bırakılan/itilen paketlerin sayısı.
- **İletilen etiket adı** (örneğin, **iletilen gelen paketler**): DDoS sistemi tarafından, filtrelenmemiş hedef VIP 'ye iletilen paketlerin sayısı.
- **Etiket adı** (örneğin, **gelen paketler DDoS**): temizleme sistemine gelen toplam paket sayısı – bırakılan ve iletilen paketlerin toplamını temsil eder.

Bu [Azure izleyici uyarı kuralı](https://github.com/Azure/Azure-Network-Security/tree/master/Azure%20DDoS%20Protection/Azure%20Monitor%20Alert%20-%20DDoS%20Mitigation%20Started) , etkin bir DDoS risk azaltma ne zaman meydana geldiğini algılamak için basit bir sorgu çalıştırır. Telemetrinin doğrulanması için DDoS saldırısının benzetimini yapmak için bkz. [DDoS algılamayı doğrulama](test-through-simulations.md). 

## <a name="view-ddos-mitigation-policies"></a>DDoS risk azaltma ilkelerini görüntüleme

DDoS koruması standardı, DDoS 'ın etkinleştirildiği sanal ağdaki korumalı kaynağın her genel IP adresi için otomatik olarak ayarlanmış üç hafifletme ilkesi (TCP SYN, TCP & UDP) uygular. Aşağıdaki resimde gösterildiği gibi, **toplama** türü olan DDoS azaltma ölçümlerini ' Max ' olarak tetiklemek Için, DDoS azaltma ve **gelen UDP paketlerini** **tetiklemek üzere gelen TCP paketlerini** seçerek ilke eşiklerini görüntüleyebilirsiniz:

![Risk azaltma ilkelerini görüntüleme](./media/manage-ddos-protection/view-mitigation-policies.png)

İlke eşikleri, Azure Machine Learning tabanlı ağ trafiği profili oluşturma yöntemiyle otomatik olarak yapılandırılır. Yalnızca ilke eşiğine ulaşıldığında, saldırı altındaki IP adresi için DDoS risk azaltma meydana gelir.

## <a name="view-ddos-protection-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde DDoS koruma uyarılarını görüntüleme

Azure Güvenlik Merkezi, sorunları araştırmaya ve düzeltmeye yardımcı olacak bilgiler içeren [güvenlik uyarılarının](/azure/security-center/security-center-managing-and-responding-alerts)bir listesini sağlar. Bu özellikle, DDoS saldırılarıyla ilgili uyarılar ve saldırıyı neredeyse güncel olarak azaltmak için gerçekleştirilecek eylemler dahil olmak üzere uyarıların birleştirilmiş bir görünümünü alırsınız.
Tüm DDoS saldırıları algılama ve risk azaltma için göreceğiniz iki özel uyarı vardır:

- **Genel IP Için DDoS saldırısı algılandı**: Bu uyarı, DDoS Koruması hizmeti, genel IP adreslerinizin bir DDoS saldırısının hedefi olduğunu algıladığında üretilir.
- **Genel IP Için DDoS saldırısı azaltılmıştır**: Bu uyarı, genel IP adresindeki bir saldırı azaltıldığında oluşturulur.
Uyarıları görüntülemek için Azure portal **Güvenlik Merkezi** 'ni açın. **Tehdit koruması** altında **güvenlik uyarıları**' nı seçin. Aşağıdaki ekran görüntüsünde, DDoS saldırı uyarılarının bir örneği gösterilmektedir.

![Azure Güvenlik Merkezi 'nde DDoS uyarısı](./media/manage-ddos-protection/ddos-alert-asc.png)

Uyarılar, saldırı altında genel IP adresi, coğrafi ve tehdit bilgileri ve düzeltmeler adımları hakkında genel bilgiler içerir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

- DDoS koruma ölçümleri için uyarıları yapılandırma
- DDoS koruması telemetrisini kullanma
- DDoS risk azaltma ilkelerini görüntüleme
- Azure Güvenlik Merkezi 'nde DDoS koruma uyarılarını görüntüleme

Saldırı risk azaltma raporlarının ve akış günlüklerinin nasıl yapılandırılacağı hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [DDoS saldırı azaltma raporları ve akış günlükleri](reports-and-flow-logs.md)