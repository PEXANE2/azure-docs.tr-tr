---
title: Microsoft Azure Stack Edge Pro R 'ye Genel Bakış | Microsoft Docs
description: Azure 'a ağ tabanlı Aktarım için fiziksel bir cihaz kullanan askerlik uygulamalar için bir depolama çözümü olan Azure Stack Edge Pro R cihazlarını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 09/09/2020
ms.author: alkohli
ms.openlocfilehash: 8bf9bb172d559dbb1f92b376b60a914ec24769d1
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948252"
---
# <a name="what-is-the-azure-stack-edge-pro-r"></a>Azure Stack Edge Pro R nedir?

Azure Stack Edge Pro R, bir hizmet olarak donanım çözümüdür. Microsoft, Azure 'a ağ veri aktarımı üzerinden için sürekli bir Rugged, sunucu sınıfı ve taşınabilir bir uç cihaz sağlar. Grafik Işleme birimi (GPU) ile donatılmış bu cihazlar AI, analiz ve sunucusuz bilgi işlem için iyileştirilmiştir. Rugged cihazları, en zorlu ortamlarda kullanım için uygundur.

Bu makalede, Azure Stack Edge Pro R çözümüne, önemli yeteneklere ve bu cihazı dağıtabileceğiniz senaryolara ilişkin bir genel bakış sunulmaktadır.


## <a name="key-capabilities"></a>Temel işlevler

Azure Stack Edge Pro R aşağıdaki yeteneklere sahiptir:

|Özellik |Açıklama  |
|---------|---------|
|Rugged, taşınabilir donanım| Rugged 85 lb. Server sınıfı donanım, en zorlu ortamlar için tasarlanmıştır. 2 kişi, büyük/küçük bir taşıma durumunda cihaz taşınabilir. |
|Bulutta yönetilen     |Cihaz ve hizmet, Azure portal aracılığıyla yönetilir.|
|Edge işlem iş yükleri   |Verilerin analizine, işlenmesine, filtrelenmesine olanak tanır. VM 'Leri ve Kapsayıcılı iş yüklerini destekler.|
|Hızlandırılmış AI ınkrii| NVIDIA T4 GPU tarafından etkinleştirildi.|
|Yüksek performans | Yüksek performanslı işlem ve veri aktarımları.|
|Veri erişimi     | Bulutta ek veri işleme için bulut API'lerini kullanarak Azure Depolama Blobları ve Azure Dosyaları'ndan doğrudan veri erişimi. Cihazdaki yerel önbellek, en son kullanılan dosyalara hızlı erişim için kullanılır.|
|Bağlantısı kesik mod| Cihaz ve hizmet, isteğe bağlı olarak Azure Stack aracılığıyla yönetilebilir. <br> Uygulamaları çevrimdışı modda dağıtın, çalıştırın, yönetin. <br> Bağlantısız mod, çevrimdışı karşıya yükleme senaryolarını destekler.|
|Desteklenen protokoller     |Veri alımı için standart SMB, NFS ve REST protokolleri desteği. <br> Desteklenen sürümler hakkında daha fazla bilgi için [Azure Stack Edge Pro R sistem gereksinimleri](azure-stack-edge-gpu-system-requirements.md)' ne gidin.|
|Veri yenileme     | Yerel dosyaları buluttaki en son sürümle yenileme olanağı.|
|Çift şifreleme    | Otomatik olarak şifrelenen sürücülerin kullanımı, ilk şifreleme katmanını sağlar. VPN ikinci şifreleme katmanını sağlar. Verileri yerel olarak şifrelemek ve *https* üzerinden buluta veri aktarımını güvenli hale getirmek için BitLocker desteği.|
|Bant genişliği azaltma| Yoğun saatlerde bant genişliği kullanımını sınırlandırmaya kısıtlama.|

<!--|Scale out file server| Available as 1-node and 4-node cluster configurations|-->

## <a name="use-cases"></a>Uygulama alanları

Azure Stack Edge Pro R 'nin, Edge 'de hızlı Machine Learning (ML) için kullanılabileceği ve verileri Azure 'a göndermeden önce ön işlemesi için kullanabileceğiniz çeşitli senaryolar aşağıda verilmiştir.

- **Azure Machine Learning Ile çıkarım** -Azure Stack Edge Pro R ile, veri buluta gönderilmeden önce üzerinde işlem yapılabilir hızlı sonuçlar almak için ml modellerini çalıştırabilirsiniz. Tam veri kümesi, ML modellerinizi yeniden eğitmeye ve artırmaya devam etmek için isteğe bağlı olarak aktarılabilir. Azure Stack Edge Pro R cihazında Azure ML donanım hızlandırılmış modellerini kullanma hakkında daha fazla bilgi için, bkz. [Azure Stack Edge Pro r üzerinde Azure ML donanım hızlandırılmış modellerini dağıtma](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-fpga-web-service#deploy-to-a-local-edge-server).

- **Önceden işlem** yapılabilir bir veri kümesi oluşturmak Için verileri Azure 'a göndermeden önce veri dönüştürme. Önceden işleme şu amaçlarla kullanılabilir:

    - Verileri toplama.
    - Verileri değiştirin, örneğin kişisel verileri kaldırmak için.
    - Depolama ve bant genişliğini iyileştirmek veya daha fazla analiz için verileri alt kümele.
    - IoT Olaylarını analiz etme ve bunlara yanıt verme.

- **Ağ üzerinden Azure 'a veri aktarma** -daha fazla bilgi işlem ve analiz sağlamak veya arşivleme amacıyla verileri kolayca ve hızlı bir şekilde Azure 'a aktarmak Için Azure Stack Edge Pro R 'yi kullanın.

## <a name="components"></a>Bileşenler

Azure Stack Edge Pro R çözümü, bir Azure Stack Edge kaynağı, Azure Stack Edge Pro R Rugged, fiziksel cihaz ve yerel Web kullanıcı arabiriminden oluşur.

- **Azure Stack Edge Pro r fiziksel cihazı** -Azure Stack Edge Pro r, Azure 'a veri gönderecek şekilde yapılandırılabilecek bir 1 düğümlü cihazdır. Cihaz, Microsoft tarafından sağlanan bir Rugged Enme ile bir 1U sunucusudur. İsteğe bağlı olarak, sunucu bir UPS (Ayrıca 1U) ile kullanılabilir.

    ![Azure Stack Edge Pro R 1-node cihazı](media/azure-stack-edge-j-series-overview/device-image-1.png)

- **Azure Stack Edge kaynağı** : bir rugged, Azure Stack Edge Pro R cihazını farklı coğrafi konumlardan erişebileceğiniz bir web arabiriminden yönetmenize olanak tanıyan bir Azure Portal kaynaktır. Kaynak oluşturmak ve yönetmek, cihazları ve uyarıları görüntülemek ve yönetmek ve paylaşımları yönetmek için Azure Stack Edge kaynağını kullanın.  

- **Azure Stack Edge Pro R yerel Web Kullanıcı arabirimi** -ilk cihaz yapılandırması için yerel Web Kullanıcı arabirimini kullanın, tanılamayı çalıştırmak Için Azure Stack Edge Pro R cihazını kapatıp yeniden başlatın, kopyalama günlüklerini görüntüleyin ve hizmet isteği dosyasını dosya dosyasına Microsoft desteği başvurun.


## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure Stack Edge Pro R fiziksel cihazı, Azure kaynağı ve veri aktarımı yaptığınız hedef depolama hesabı, tümünün aynı bölgede olması gerekmez.

- **Kaynak kullanılabilirliği** -Azure Stack Edge kaynağının kullanılabildiği tüm bölgelerin listesi için [bölgeye göre kullanılabilir Azure ürünlerine](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all)gidin. 

- **Hedef Depolama hesapları**: Verilerin depolandığı depolama hesapları, tüm Azure bölgelerinde sağlanır. Depolama hesaplarının Azure Stack Edge Pro R verilerini depolayan bölgeler, cihazın en iyi performans için bulunduğu yere yakın olmalıdır. Cihazdan uzağa konumlandırılan depolama hesabı uzun gecikme sürelerine ve daha yavaş bir performansa yol açar.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Edge Pro R sistem gereksinimlerini](azure-stack-edge-gpu-system-requirements.md)gözden geçirin.
- [Azure Stack Edge Pro R sınırlarını](azure-stack-edge-limits.md)anlayın.

