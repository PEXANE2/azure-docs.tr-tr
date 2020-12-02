---
title: Microsoft Azure Stack Edge Pro 'ya genel bakış | Microsoft Docs
description: Azure 'a ağ tabanlı Aktarım için fiziksel bir cihaz kullanan bir depolama çözümü olan Azure Stack Edge Pro 'Yu açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 09/09/2020
ms.author: alkohli
ms.openlocfilehash: 51469c23574f55c102f0c6fb98e5aa5178b59a4a
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96455749"
---
# <a name="what-is-azure-stack-edge-pro-with-fpga"></a>FPGA ile Azure Stack Edge Pro nedir?

[!INCLUDE [data-box-edge-gateway-rename-note](../../includes/data-box-edge-gateway-rename-note.md)]

FPGA ile Azure Stack Edge Pro, ağ veri aktarımı özelliklerine sahip bir AI özellikli Edge bilgi işlem aygıtıdır. Bu makale, Azure Stack Edge Pro 'Yu FPGA çözümü, avantajları, önemli özellikler ve bu cihazı dağıtabileceğiniz senaryolar hakkında bir genel bakış sağlar.

Azure Stack Edge Pro, FPGA ile bir hizmet olarak donanım çözümüdür. Microsoft, hızlandırılmış AI-ınlili sağlayan ve bir ağ depolama ağ geçidi 'nin tüm özelliklerine sahip olan yerleşik bir alan programlanabilir geçit dizisi (FPGA) ile birlikte bulut tarafından yönetilen bir cihaz sunar. 

## <a name="use-cases"></a>Uygulama alanları

Azure Stack Edge Pro 'nun, en uçta hızlı Machine Learning (ML) için kullanılabileceği ve verileri Azure 'a göndermeden önce ön işlemesi için kullanabileceğiniz çeşitli senaryolar aşağıda verilmiştir.

- **Azure Machine Learning Ile çıkarım** -Azure Stack Edge Pro ile, veri buluta gönderilmeden önce üzerinde işlem yapılabilir hızlı sonuçlar almak için ml modellerini çalıştırabilirsiniz. Tam veri kümesi, ML modellerinizi yeniden eğitmeye ve artırmaya devam etmek için isteğe bağlı olarak aktarılabilir. Azure Stack Edge Pro cihazında Azure ML donanım hızlandırılmış modellerini kullanma hakkında daha fazla bilgi için, bkz. [Azure Stack Edge Pro 'Da Azure ML donanım hızlandırılmış modellerini dağıtma](../machine-learning/how-to-deploy-fpga-web-service.md#deploy-to-a-local-edge-server).

- **Önceden işlem** yapılabilir bir veri kümesi oluşturmak Için verileri Azure 'a göndermeden önce veri dönüştürme. Önceden işleme şu amaçlarla kullanılabilir: 

    - Verileri toplama.
    - Verileri değiştirin, örneğin kişisel verileri kaldırmak için.
    - Depolama ve bant genişliğini iyileştirmek veya daha fazla analiz için verileri alt kümele.
    - IoT Olaylarını analiz etme ve bunlara yanıt verme. 

- **Ağ üzerinden Azure 'a veri aktarma** -daha fazla bilgi işlem ve analiz sağlamak veya arşivleme amacıyla verileri kolayca ve hızlı bir şekilde Azure 'a aktarmak Için Azure Stack Edge Pro 'yu kullanın. 

## <a name="key-capabilities"></a>Temel işlevler

Azure Stack Edge Pro aşağıdaki yeteneklere sahiptir:

|Özellik |Açıklama  |
|---------|---------|
|Hızlandırılmış AI ınkrii| Yerleşik FPGA tarafından etkinleştirildi.|
|Bilgi İşlem       |Verilerin analizine, işlenmesine, filtrelenmesine olanak tanır.|
|Yüksek performans | Yüksek performanslı işlem ve veri aktarımları.|
|Veri erişimi     | Bulutta ek veri işleme için bulut API'lerini kullanarak Azure Depolama Blobları ve Azure Dosyaları'ndan doğrudan veri erişimi. Cihazdaki yerel önbellek, en son kullanılan dosyalara hızlı erişim için kullanılır.|
|Bulutta yönetilen     |Cihaz ve hizmet, Azure portal aracılığıyla yönetilir.  |
|Çevrimdışı karşıya yükleme     | Bağlantısız mod, çevrimdışı karşıya yükleme senaryolarını destekler.|
|Desteklenen protokoller     | Veri alımında standart SMB ve NFS protokolleri için destek. <br> Desteklenen sürümler hakkında daha fazla bilgi için bkz. [Edge Pro sistem gereksinimleri Azure Stack](azure-stack-edge-system-requirements.md).|
|Veri yenileme     | Yerel dosyaları buluttaki en son sürümle yenileme olanağı.|
|Şifreleme    | Verileri yerel olarak şifrelemek ve *https* üzerinden buluta veri aktarımını güvenli hale getirmek için BitLocker desteği.|
|Bant genişliği azaltma| Yoğun saatlerde bant genişliği kullanımını sınırlandırmaya kısıtlama.|
|ExpressRoute | ExpressRoute aracılığıyla güvenlik eklendi. Yerel cihazlardan gelen trafiğin, ExpressRoute üzerinden dolaşmasına neden olan eşleme yapılandırmasını kullanın. Daha fazla bilgi için bkz. [ExpressRoute'a genel bakış](../expressroute/expressroute-introduction.md).

## <a name="components"></a>Bileşenler

Azure Stack Edge Pro çözümü, Azure Stack Edge kaynağı, Azure Stack Edge Pro fiziksel cihazından ve yerel bir Web kullanıcı arabiriminden oluşur.

* **Azure Stack Edge Pro fiziksel cihazı** -Azure 'a veri gönderecek şekilde yapılandırılabilen, Microsoft tarafından sağlanan bir 1U takılabilir sunucu.
    
* **Azure Stack Edge kaynağı** : bir Azure Stack Edge Pro cihazını, farklı coğrafi konumlardan erişebileceğiniz bir web arabiriminden yönetmenizi sağlayan Azure Portal bir kaynaktır. Kaynak oluşturmak ve yönetmek, cihazları ve uyarıları görüntülemek ve yönetmek ve paylaşımları yönetmek için Azure Stack Edge kaynağını kullanın.  

    <!--![The Azure Stack Edge service in Azure portal](media/data-box-overview/data-box-Edge-service1.png)-->

    Daha fazla bilgi için [Azure Stack Edge Pro cihazınız için sipariş oluşturma](azure-stack-edge-deploy-prep.md#create-a-new-resource)bölümüne gidin.

* **Azure Stack Edge Pro yerel Web Kullanıcı arabirimi** -tanılamayı çalıştırmak için yerel Web Kullanıcı arabirimini kullanın, Azure Stack Edge Pro cihazını kapatıp yeniden başlatın, kopyalama günlüklerini görüntüleyin ve hizmet isteği dosyasını dosya dosyasına Microsoft desteği başvurun.

    <!--![The Azure Stack Edge Pro local web UI](media/data-box-Edge-overview/data-box-Edge-local-web-ui.png)-->

    Web tabanlı kullanıcı arabirimini kullanma hakkında daha fazla bilgi için, [Azure Stack Edge Pro 'yu yönetmek üzere Web tabanlı kullanıcı arabirimini kullanma](azure-stack-edge-manage-access-power-connectivity-mode.md)bölümüne gidin.

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure Stack Edge Pro fiziksel cihazı, Azure kaynağı ve veri aktarımı yaptığınız hedef depolama hesabı, tümünün aynı bölgede olması gerekmez.

- **Kaynak kullanılabilirliği** -Azure Stack Edge kaynağının kullanılabildiği tüm bölgelerin listesi için bkz. [bölgeye göre kullanılabilir Azure ürünleri](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). Azure Stack Edge Pro, Azure Kamu Bulutu 'nda da dağıtılabilir. Daha fazla bilgi için bkz. [Azure Kamu nedir?](../azure-government/documentation-government-welcome.md).
    
- **Hedef Depolama hesapları**: Verilerin depolandığı depolama hesapları, tüm Azure bölgelerinde sağlanır. Depolama hesaplarının Azure Stack Edge Pro verilerini depolayan bölgeler, cihazın en iyi performans için bulunduğu yere yakın yerleştirilmelidir. Cihazdan uzağa konumlandırılan depolama hesabı uzun gecikme sürelerine ve daha yavaş bir performansa yol açar.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Edge Pro sistem gereksinimlerini](azure-stack-edge-system-requirements.md)gözden geçirin.
- [Azure Stack Edge Pro sınırlarını](azure-stack-edge-limits.md)anlayın.
- Azure portal [Azure Stack Edge Pro 'yu](azure-stack-edge-deploy-prep.md) dağıtın.