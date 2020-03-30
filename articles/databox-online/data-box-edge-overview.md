---
title: Microsoft Azure Yığın Kenarı genel bakış | Microsoft Dokümanlar
description: Azure'a ağ tabanlı aktarım için fiziksel bir aygıt kullanan bir depolama çözümü olan Azure Yığını Kenarı'nı açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 11/04/2019
ms.author: alkohli
ms.openlocfilehash: f463e8883efd5e2dfc4d7fff80912c193665b850
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78399793"
---
# <a name="what-is-azure-stack-edge"></a>Azure Stack Edge nedir?

[!INCLUDE [data-box-edge-gateway-rename-note](../../includes/data-box-edge-gateway-rename-note.md)]

Azure Stack Edge, ağ veri aktarım özelliklerine sahip, AI etkin kenar bilgi işlem aygıtıdır. Bu makalede, Azure Yığını Kenarı çözümüne, avantajlara, anahtar özelliklerine ve bu aygıtı dağıtabileceğiniz senaryolara genel bir bakış sunulur.

Azure Stack Edge, hizmet olarak donanım çözümüdür. Microsoft, hızlandırılmış AI-inferencing sağlayan ve bir ağ depolama ağ geçidinin tüm özelliklerine sahip yerleşik Alan Programlanabilir Kapı Dizisi (FPGA) ile bulut tarafından yönetilen bir aygıt göndermiştir.

## <a name="use-cases"></a>Uygulama alanları

Azure Stack Edge'in, azure'a göndermeden önce hızlı Makine Öğrenimi (ML) tarafından sınırda çıkarılan ve verileri önceden işleme de kullanılabildiği çeşitli senaryolar aşağıda verilmiştir.

- **Azure Machine Learning ile çıkarım** - Azure Stack Edge ile, veriler buluta gönderilmeden önce harekete geçilebilen hızlı sonuçlar elde etmek için ML modellerini çalıştırabilirsiniz. Ml modellerinizi yeniden eğitmek ve geliştirmek için tam veri seti isteğe bağlı olarak aktarılabilir. Azure Stack Edge aygıtındaki Azure ML donanım hızlandırılmış modellerinin nasıl kullanılacağı hakkında daha fazla bilgi için Azure [Stack Edge'deki Azure ML donanım hızlandırılmış modelleri dağıt'a](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-fpga-web-service#deploy-to-a-local-edge-server)bakın.

- **İşlem öncesi veriler** - Daha işlem yapılabilir bir veri kümesi oluşturmak için verileri Azure'a göndermeden önce dönüştürün. Önceden işleme şu amaçlarla kullanılabilir: 

    - Verileri toplama.
    - Örneğin kişisel verileri kaldırmak için verileri değiştirin.
    - Depolama ve bant genişliğini optimize etmek veya daha fazla analiz için verileri alt kümele.
    - IoT Olaylarını analiz etme ve bunlara yanıt verme. 

- **Ağ üzerinden Azure'a veri aktarımı** - Daha fazla bilgi işlem ve analiz sağlamak veya arşivleme amacıyla verileri azure'a kolayca ve hızlı bir şekilde aktarmak için Azure Stack Edge'i kullanın. 

## <a name="key-capabilities"></a>Temel işlevler

Azure Stack Edge aşağıdaki özelliklere sahiptir:

|Özellik |Açıklama  |
|---------|---------|
|Hızlandırılmış AI inferencing| Dahili FPGA tarafından etkinleştirilir.|
|Bilgi işlem       |Verilerin analizine, işlenmesine, filtrelenmesine olanak tanır.|
|Yüksek performans | Yüksek performanslı bilgi işlem ve veri aktarımları.|
|Veri erişimi     | Bulutta ek veri işleme için bulut API'lerini kullanarak Azure Depolama Blobları ve Azure Dosyaları'ndan doğrudan veri erişimi. Aygıttaki yerel önbellek, en son kullanılan dosyalara hızlı erişim için kullanılır.|
|Bulut yönetimi     |Cihaz ve hizmet Azure portalı üzerinden yönetilir.  |
|Çevrimdışı karşıya yükleme     | Bağlantısız mod, çevrimdışı karşıya yükleme senaryolarını destekler.|
|Desteklenen protokoller     | Veri alımında standart SMB ve NFS protokolleri için destek. <br> Desteklenen sürümler hakkında daha fazla bilgi için [Azure Yığını Kenarı sistem gereksinimlerine](data-box-edge-system-requirements.md)bakın.|
|Veri yenileme     | Yerel dosyaları buluttaki en son sürümle yenileme olanağı.|
|Şifreleme    | Verileri yerel olarak şifrelemek ve *https*üzerinden buluta veri aktarımLarını güvenli hale getirmek için BitLocker desteği.|
|Bant genişliği azaltma| Yoğun saatlerde bant genişliği kullanımını sınırlamak için gaz.|
|ExpressRoute | ExpressRoute ile güvenlik eklendi. Yerel aygıtlardan bulut depolama bitiş noktalarına giden trafiğin ExpressRoute üzerinden geçtiği gözleme yapılandırmasını kullanın. Daha fazla bilgi için bkz. [ExpressRoute'a genel bakış](../expressroute/expressroute-introduction.md).

## <a name="components"></a>Bileşenler

The Azure Stack Edge solution comprises of Azure Stack Edge resource, Azure Stack Edge physical device, and a local web UI.

* **Azure Stack Edge fiziksel aygıtı** - Microsoft tarafından sağlanan ve Azure'a veri gönderecek şekilde yapılandırılabilen 1U rafa monte edilmiş bir sunucu.
    
* **Azure Stack Edge kaynağı** – Azure portalında, farklı coğrafi konumlardan erişebileceğiniz bir web arabiriminden Azure Stack Edge aygıtını yönetmenize olanak tanıyan bir kaynaktır. Kaynakları oluşturmak ve yönetmek, aygıtları ve uyarıları görüntülemek ve yönetmek ve paylaşımları yönetmek için Azure Yığını Kenarı kaynağını kullanın.  

    <!--![The Azure Stack Edge service in Azure portal](media/data-box-overview/data-box-Edge-service1.png)-->

    Daha fazla bilgi için [Azure Stack Edge aygıtınız için sipariş oluştur'a](data-box-edge-deploy-prep.md#create-a-new-resource)gidin.

* **Azure Stack Edge local web UI** - Use the local web UI to run diagnostics, shut down and restart the Azure Stack Edge device, view copy logs, and contact Microsoft Support to file a service request.

    <!--![The Azure Stack Edge local web UI](media/data-box-Edge-overview/data-box-Edge-local-web-ui.png)-->

    Web tabanlı Kullanıcı Arabirimi'ni kullanma hakkında bilgi için [Azure Yığın Kenarı'nızı yönetmek için web tabanlı Kullanıcı](data-box-edge-manage-access-power-connectivity-mode.md)Arabirimi'ni kullanın'a gidin.

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure Yığını Kenarı fiziksel aygıtı, Azure kaynağı ve veri aktardığınız hedef depolama hesabının tümü aynı bölgede olmak zorunda değildir.

- **Kaynak kullanılabilirliği** - Azure Yığını Kenarı kaynağının bulunduğu tüm bölgelerin listesi [için, bölgeye göre kullanılabilen Azure ürünlerine](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all)bakın. Azure Yığını Kenarı, Azure Kamu Bulutu'nda da dağıtılabilir. Daha fazla bilgi için azure [devlet nedir?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome)
    
- **Hedef Depolama hesapları**: Verilerin depolandığı depolama hesapları, tüm Azure bölgelerinde sağlanır. Depolama hesaplarının depoladığı bölgeler, optimum performans için aygıtın bulunduğu yere yakın olmalıdır. Cihazdan uzağa konumlandırılan depolama hesabı uzun gecikme sürelerine ve daha yavaş bir performansa yol açar.

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Yığın Kenarı sistem gereksinimlerini](data-box-edge-system-requirements.md)gözden geçirin.
- Azure [Yığın Kenarı sınırlarını](data-box-edge-limits.md)anlayın.
- Azure portalında [Azure Azure Yığın Kenarı'nı](data-box-edge-deploy-prep.md) dağıtın.
