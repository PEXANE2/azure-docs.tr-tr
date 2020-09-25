---
title: GPU 'ya genel bakış ile Microsoft Azure Stack Edge Pro | Microsoft Docs
description: Azure 'a ağ tabanlı Aktarım için fiziksel bir cihaz kullanan bir depolama çözümü olan GPU ile Azure Stack Edge Pro açıklanmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 09/23/2020
ms.author: alkohli
ms.openlocfilehash: 34dc0717b13cb38c4477cc93f23d57d0a8023dae
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91320753"
---
# <a name="what-is-azure-stack-edge-pro-with-gpu"></a>GPU ile Azure Stack Edge Pro nedir?

GPU ile Azure Stack Edge Pro, ağ veri aktarımı özelliklerine sahip bir AI özellikli Edge bilgi işlem aygıtıdır. Bu makalede, Azure Stack Edge Pro çözümü, avantajları, önemli özellikleri ve bu cihazı dağıtabileceğiniz senaryolar hakkında bir genel bakış sunulmaktadır.

GPU ile Azure Stack Edge Pro, bir hizmet olarak donanım çözümüdür. Microsoft, ağ depolama ağ geçidi olarak davranan ve hızlandırılmış AI sağlayan yerleşik bir grafik Işleme birimi (GPU) olan, bulut tarafından yönetilen bir cihaz sunar. 

## <a name="use-cases"></a>Uygulama alanları

Azure Stack Edge Pro 'nun, en uçta hızlı Machine Learning (ML) için kullanılabileceği ve verileri Azure 'a göndermeden önce ön işlemesi için kullanabileceğiniz çeşitli senaryolar aşağıda verilmiştir.

- **Azure Machine Learning Ile çıkarım** -Azure Stack Edge Pro ile, veri buluta gönderilmeden önce üzerinde işlem yapılabilir hızlı sonuçlar almak için ml modellerini çalıştırabilirsiniz. Tam veri kümesi, ML modellerinizi yeniden eğitmeye ve artırmaya devam etmek için isteğe bağlı olarak aktarılabilir. Azure Stack Edge Pro cihazında Azure ML donanım hızlandırılmış modellerini kullanma hakkında daha fazla bilgi için, bkz. [Azure Stack Edge Pro 'Da Azure ML donanım hızlandırılmış modellerini dağıtma](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-fpga-web-service#deploy-to-a-local-edge-server).

- Daha **önceden** uygulanabilir bir veri kümesi oluşturmak için Kapsayıcılı iş yükleri ve sanal makineler gibi işlem seçenekleri aracılığıyla verileri Azure 'a göndermeden önce veri dönüştürme. Önceden işleme şu amaçlarla kullanılabilir: 

    - Verileri toplama.
    - Verileri değiştirin, örneğin kişisel verileri kaldırmak için.
    - Depolama ve bant genişliğini iyileştirmek veya daha fazla analiz için verileri alt kümele.
    - IoT Olaylarını analiz etme ve bunlara yanıt verme. 

- **Ağ üzerinden Azure 'a veri aktarma** -daha fazla bilgi işlem ve analiz sağlamak veya arşivleme amacıyla verileri kolayca ve hızlı bir şekilde Azure 'a aktarmak Için Azure Stack Edge Pro 'yu kullanın. 

## <a name="key-capabilities"></a>Temel işlevler

Azure Stack Edge Pro aşağıdaki yeteneklere sahiptir:

|Özellik |Description  |
|---------|---------|
|Hızlandırılmış AI ınkrii| Yerleşik GPU (modele bağlı olarak bir veya ikisi) tarafından etkinleştirilir.|
|Edge bilgi işlem      |Veri çözümleme, işleme ve filtrelemeye olanak tanımak için VM ve Kapsayıcılı iş yüklerini destekler. |
|Veri erişimi     | Bulutta ek veri işleme için bulut API'lerini kullanarak Azure Depolama Blobları ve Azure Dosyaları'ndan doğrudan veri erişimi. Cihazdaki yerel önbellek, en son kullanılan dosyalara hızlı erişim için kullanılır.|
|Bulutta yönetilen     |Cihaz ve hizmet, Azure portal aracılığıyla yönetilir.  |
|Çevrimdışı karşıya yükleme     | Bağlantısız mod, çevrimdışı karşıya yükleme senaryolarını destekler.|
|Desteklenen dosya aktarımı protokolleri      | Veri alımı için standart SMB, NFS ve REST protokolleri desteği. <br> Desteklenen sürümler hakkında daha fazla bilgi için bkz. [Edge Pro sistem gereksinimleri Azure Stack](azure-stack-edge-system-requirements.md).|
|Veri yenileme     | Yerel dosyaları buluttaki en son sürümle yenileme olanağı.|
|Şifreleme    | Verileri yerel olarak şifrelemek ve *https*üzerinden buluta veri aktarımını güvenli hale getirmek için BitLocker desteği.|
|Bant genişliği azaltma| Yoğun saatlerde bant genişliği kullanımını sınırlandırmaya kısıtlama.|
<!--|ExpressRoute | ExpressRoute aracılığıyla güvenlik eklendi. Yerel cihazlardan gelen trafiğin, ExpressRoute üzerinden dolaşmasına neden olan eşleme yapılandırmasını kullanın. Daha fazla bilgi için bkz. [ExpressRoute 'a genel bakış](../expressroute/expressroute-introduction.md).-->

## <a name="components"></a>Bileşenler

Azure Stack Edge Pro çözümü, Azure Stack Edge kaynağı, Azure Stack Edge Pro fiziksel cihazından ve yerel bir Web kullanıcı arabiriminden oluşur.

* **Azure Stack Edge Pro fiziksel cihazı** -Azure 'a veri gönderecek şekilde yapılandırılabilen, Microsoft tarafından sağlanan bir 1U takılabilir sunucu.
    
* **Azure Stack Edge kaynağı** : bir Azure Stack Edge Pro cihazını, farklı coğrafi konumlardan erişebileceğiniz bir web arabiriminden yönetmenizi sağlayan Azure Portal bir kaynaktır. Kaynak oluşturmak ve yönetmek, cihazları ve uyarıları görüntülemek ve yönetmek ve paylaşımları yönetmek için Azure Stack Edge kaynağını kullanın.  

    Daha fazla bilgi için [Azure Stack Edge Pro cihazınız için sipariş oluşturma](azure-stack-edge-gpu-deploy-prep.md#create-a-new-resource)bölümüne gidin.

* **Azure Stack Edge Pro Local Web UI** -öncelikle cihazın ilk yapılandırması için tasarlanan Azure Stack Edge Pro cihazınızda tarayıcı tabanlı bir yerel kullanıcı arabirimi. Yerel Web Kullanıcı arabirimini kullanarak da tanılamayı çalıştırın, Azure Stack Edge Pro cihazını kapatıp yeniden başlatın, kopyalama günlüklerini görüntüleyin ve bir hizmet isteği dosyası için Microsoft Desteği başvurun.

    Web tabanlı kullanıcı arabirimini kullanma hakkında daha fazla bilgi için, [Azure Stack Edge Pro 'yu yönetmek üzere Web tabanlı kullanıcı arabirimini kullanma](azure-stack-edge-manage-access-power-connectivity-mode.md)bölümüne gidin.

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure Stack Edge Pro fiziksel cihazı, Azure kaynağı ve veri aktarımı yaptığınız hedef depolama hesabı, tümünün aynı bölgede olması gerekmez.

- **Kaynak kullanılabilirliği** -bu önizleme sürümü için kaynak Doğu ABD, Batı AB ve Güney Doğu Asya bölgelerinde kullanılabilir.

- **Cihaz kullanılabilirliği** -Azure Stack Edge Pro cihazının kullanılabildiği tüm ülkelerin listesi için, [Azure Stack Edge pro fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-stack/edge/#azureStackEdgePro)için **Azure Stack Edge Pro** sekmesinde **kullanılabilirlik** bölümüne gidin.
    
- **Hedef Depolama hesapları**: Verilerin depolandığı depolama hesapları, tüm Azure bölgelerinde sağlanır. Depolama hesaplarının Azure Stack Edge Pro verilerini depolayan bölgeler, cihazın en iyi performans için bulunduğu yere yakın yerleştirilmelidir. Cihazdan uzağa konumlandırılan depolama hesabı uzun gecikme sürelerine ve daha yavaş bir performansa yol açar.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Edge Pro sistem gereksinimlerini](azure-stack-edge-gpu-system-requirements.md)gözden geçirin.

- [Azure Stack Edge Pro sınırlarını](azure-stack-edge-limits.md)anlayın.
- Azure portal [Azure Stack Edge Pro 'yu](azure-stack-edge-gpu-deploy-prep.md) dağıtın.
