---
title: Microsoft Azure Stack kenara genel bakış | Microsoft Docs
description: Azure 'a ağ tabanlı Aktarım için fiziksel bir cihaz kullanan bir depolama çözümü olan Azure Stack Edge açıklanmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 08/27/2020
ms.author: alkohli
ms.openlocfilehash: e3caf4e0e60b8ef9fb0b30b599b22e0f07d8998c
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89088416"
---
# <a name="what-is-azure-stack-edge-with-gpu-preview"></a>GPU ile Azure Stack Edge nedir (Önizleme)?

GPU ile Azure Stack Edge, ağ veri aktarımı özelliklerine sahip bir AI özellikli Edge bilgi işlem aygıtıdır. Bu makalede, Azure Stack Edge çözümüne, avantajlara, önemli yeteneklere ve bu cihazı dağıtabileceğiniz senaryolara ilişkin bir genel bakış sunulmaktadır.

GPU ile Azure Stack Edge, bir hizmet olarak donanım çözümüdür. Microsoft, ağ depolama ağ geçidi olarak davranan ve hızlandırılmış AI sağlayan yerleşik bir grafik Işleme birimi (GPU) olan, bulut tarafından yönetilen bir cihaz sunar. 

> [!IMPORTANT]
> GPU ile Azure Stack Edge Şu anda önizleme aşamasındadır. Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="use-cases"></a>Uygulama alanları

Azure Stack Edge 'in Edge 'de hızlı Machine Learning (ML) için kullanılabileceği ve verileri Azure 'a göndermeden önce ön işlemesi için kullanabileceğiniz çeşitli senaryolar aşağıda verilmiştir.

- **Azure Machine Learning Ile çıkarım** -Azure Stack Edge ile, veri buluta gönderilmeden önce üzerinde işlem yapılabilir hızlı sonuçlar almak için ml modellerini çalıştırabilirsiniz. Tam veri kümesi, ML modellerinizi yeniden eğitmeye ve artırmaya devam etmek için isteğe bağlı olarak aktarılabilir. Azure Stack Edge cihazında Azure ML donanım hızlandırılmış modellerini kullanma hakkında daha fazla bilgi için bkz. [Azure Stack Edge üzerinde Azure ML Hardware hızlandırılmış modellerini dağıtma](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-fpga-web-service#deploy-to-a-local-edge-server).

- **Önceden işlem** yapılabilir bir veri kümesi oluşturmak Için verileri Azure 'a göndermeden önce veri dönüştürme. Önceden işleme şu amaçlarla kullanılabilir: 

    - Verileri toplama.
    - Verileri değiştirin, örneğin kişisel verileri kaldırmak için.
    - Depolama ve bant genişliğini iyileştirmek veya daha fazla analiz için verileri alt kümele.
    - IoT Olaylarını analiz etme ve bunlara yanıt verme. 

- **Ağ üzerinden Azure 'a veri aktarma** -daha fazla bilgi işlem ve analiz sağlamak veya arşivleme amacıyla verileri kolayca ve hızlı bir şekilde Azure 'a aktarmak Için Azure Stack Edge kullanın. 

## <a name="key-capabilities"></a>Temel işlevler

Azure Stack Edge aşağıdaki yeteneklere sahiptir:

|Özellik |Açıklama  |
|---------|---------|
|Hızlandırılmış AI ınkrii| Yerleşik GPU (modele bağlı olarak bir veya ikisi) tarafından etkinleştirilir.|
|Edge bilgi işlem      |Verilerin analizine, işlenmesine, filtrelenmesine olanak tanır. VM 'Leri ve Kubernetes kümelerini destekler.|
|Yüksek performans | Yüksek performanslı işlem ve veri aktarımları.|
|Veri erişimi     | Bulutta ek veri işleme için bulut API'lerini kullanarak Azure Depolama Blobları ve Azure Dosyaları'ndan doğrudan veri erişimi. Cihazdaki yerel önbellek, en son kullanılan dosyalara hızlı erişim için kullanılır.|
|Bulutta yönetilen     |Cihaz ve hizmet, Azure portal aracılığıyla yönetilir.  |
|Çevrimdışı karşıya yükleme     | Bağlantısız mod, çevrimdışı karşıya yükleme senaryolarını destekler.|
|Desteklenen protokoller     | Veri alımı için standart SMB, NFS ve REST protokolleri desteği. <br> Desteklenen sürümler hakkında daha fazla bilgi için bkz. [Azure Stack Edge sistem gereksinimleri](azure-stack-edge-system-requirements.md).|
|Veri yenileme     | Yerel dosyaları buluttaki en son sürümle yenileme olanağı.|
|Şifreleme    | Verileri yerel olarak şifrelemek ve *https*üzerinden buluta veri aktarımını güvenli hale getirmek için BitLocker desteği.|
|Bant genişliği azaltma| Yoğun saatlerde bant genişliği kullanımını sınırlandırmaya kısıtlama.|
|ExpressRoute | ExpressRoute aracılığıyla güvenlik eklendi. Yerel cihazlardan gelen trafiğin, ExpressRoute üzerinden dolaşmasına neden olan eşleme yapılandırmasını kullanın. Daha fazla bilgi için bkz. [ExpressRoute'a genel bakış](../expressroute/expressroute-introduction.md).

## <a name="components"></a>Bileşenler

Azure Stack Edge çözümü, Azure Stack Edge kaynağı, Azure Stack Edge fiziksel cihazından ve yerel bir Web kullanıcı arabiriminden oluşur.

* **Azure Stack Edge fiziksel cihaz** -Azure 'a veri gönderecek şekilde yapılandırılabilen, Microsoft tarafından sağlanan bir 1U takılabilir sunucu.
    
* **Azure Stack Edge kaynağı** : bir Azure Stack Edge cihazını, farklı coğrafi konumlardan erişebileceğiniz bir web arabiriminden yönetmenizi sağlayan Azure Portal bir kaynaktır. Kaynak oluşturmak ve yönetmek, cihazları ve uyarıları görüntülemek ve yönetmek ve paylaşımları yönetmek için Azure Stack Edge kaynağını kullanın.  

    Daha fazla bilgi için [Azure Stack Edge cihazınız için sipariş oluşturma](azure-stack-edge-gpu-deploy-prep.md#create-a-new-resource)bölümüne gidin.

* **Azure Stack Edge Yerel Web Kullanıcı arabirimi** -tanılamayı çalıştırmak için yerel Web Kullanıcı arabirimini kullanın, Azure Stack Edge cihazını kapatıp yeniden başlatın, kopyalama günlüklerini görüntüleyin ve hizmet isteği dosyası için Microsoft desteği başvurun.

    Web tabanlı kullanıcı arabirimini kullanma hakkında daha fazla bilgi için, [Azure Stack kenarını yönetmek üzere Web tabanlı kullanıcı arabirimini kullanma](azure-stack-edge-manage-access-power-connectivity-mode.md)bölümüne gidin.

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure Stack Edge fiziksel cihazı, Azure kaynağı ve verileri aktardığınız hedef depolama hesabının hepsi aynı bölgede olmalıdır.

- **Kaynak kullanılabilirliği** -bu önizleme sürümü için kaynak Doğu ABD, Batı AB ve Güney Doğu Asya bölgelerinde kullanılabilir.
    
- **Hedef Depolama hesapları**: Verilerin depolandığı depolama hesapları, tüm Azure bölgelerinde sağlanır. Depolama hesaplarının Azure Stack uç verilerini depolayan bölgeler, cihazın en iyi performans için bulunduğu yere yakın yerleştirilmelidir. Cihazdan uzağa konumlandırılan depolama hesabı uzun gecikme sürelerine ve daha yavaş bir performansa yol açar.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Edge sistem gereksinimlerini](azure-stack-edge-gpu-system-requirements.md)gözden geçirin.
- [Azure Stack Edge sınırlarını](azure-stack-edge-limits.md)anlayın.
- Azure portal [Azure Stack Edge](azure-stack-edge-gpu-deploy-prep.md) dağıtın.
