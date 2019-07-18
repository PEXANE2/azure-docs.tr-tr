---
title: Microsoft Azure Data Box Edge'e genel bakış | Microsoft Docs
description: Azure 'a ağ tabanlı Aktarım için fiziksel bir cihaz kullanan bir depolama çözümü olan Azure Data Box Edge açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 07/17/2019
ms.author: alkohli
ms.openlocfilehash: 69580f956b603423ef302353953a45ad5d00391e
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68305430"
---
# <a name="what-is-azure-data-box-edge"></a>Azure Data Box Edge nedir? 

Azure Data Box Edge ağ veri aktarımı yetenekleri olan, AI özellikli bir uç bilgi işlem cihazıdır. Bu makalede Data Box Edge çözümüne, avantajlarına, önemli özelliklerine ve bu cihazı dağıtabileceğiniz senaryolara genel bir bakış sağlanır. 

Data Box Edge bir hizmet olarak donanım çözümüdür. Microsoft, hızlandırılmış AI-ıncapatıes sağlayan ve bir depolama ağ geçidi 'nin tüm özelliklerine sahip olan, yerleşik bir alan programlanabilir geçit dizisi (FPGA) ile birlikte bulut tarafından yönetilen bir cihaz sunar. 

## <a name="use-cases"></a>Uygulama alanları

Data Box Edge, en kenarda bulunan hızlı Machine Learning (ML) için kullanılabilen ve verileri Azure 'a göndermeden önce ön işleme için kullanılabilecek çeşitli senaryolar aşağıda verilmiştir.

- **Azure Machine Learning Ile çıkarım** -Data Box Edge, veri buluta gönderilmeden önce üzerinde işlem yapılabilir hızlı sonuçlar almak için ml modellerini çalıştırabilirsiniz. Tam veri kümesi, ML modellerinizi yeniden eğitmeye ve artırmaya devam etmek için isteğe bağlı olarak aktarılabilir. Data Box Edge cihazında Azure ML donanım hızlandırılmış modellerini kullanma hakkında daha fazla bilgi için, bkz. [Data Box Edge Azure ML Hardware hızlandırılmış modellerini dağıtma](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-fpga-web-service#deploy-to-a-local-edge-server).

- **Önceden işlem** yapılabilir bir veri kümesi oluşturmak Için verileri Azure 'a göndermeden önce veri dönüştürme. Önceden işleme şu amaçlarla kullanılabilir: 

    - Verileri toplama.
    - Verileri değiştirin, örneğin kişisel verileri kaldırmak için.
    - Depolama ve bant genişliğini iyileştirmek veya daha fazla analiz için verileri alt kümele.
    - IoT Olaylarını analiz etme ve bunlara yanıt verme. 

- **Verileri ağ üzerinden Azure'a aktarma** - Daha fazla işlem ve analiz yapmak için veya arşiv amacıyla Data Box Edge kullanarak verileri kolayca ve hızla Azure'a aktarın. 


## <a name="key-capabilities"></a>Temel işlevler

Data Box Edge'in şöyle özellikleri vardır:

|Özellik |Açıklama  |
|---------|---------|
|Hızlandırılmış AI ınkrii| Yerleşik FPGA tarafından etkinleştirildi.|
|Bilgi işlem       |Verilerin analizine, işlenmesine, filtrelenmesine olanak tanır.|
|Yüksek performans | Yüksek performanslı işlem ve veri aktarımları.|
|Veri erişimi     | Bulutta ek veri işleme için bulut API'lerini kullanarak Azure Depolama Blobları ve Azure Dosyaları'ndan doğrudan veri erişimi. Cihazdaki yerel önbellek, en son kullanılan dosyalara hızlı erişim için kullanılır.|
|Bulutta yönetilen     |Cihaz ve hizmet, Azure portal aracılığıyla yönetilir.  |
|Çevrimdışı karşıya yükleme     | Bağlantısız mod, çevrimdışı karşıya yükleme senaryolarını destekler.|
|Desteklenen protokoller     | Veri alımında standart SMB ve NFS protokolleri için destek. <br> Desteklenen sürümler hakkında daha fazla bilgi için bkz. [Data Box Edge sistem gereksinimleri](data-box-edge-system-requirements.md).|
|Veri yenileme     | Yerel dosyaları buluttaki en son sürümle yenileme olanağı.|
|Şifreleme    | Verileri yerel olarak şifrelemek ve *https* üzerinden buluta veri aktarımının güvenliğini sağlamak için BitLocker desteği.|
|Bant genişliği azaltma| Yoğun saatlerde bant genişliği kullanımını sınırlandırmaya kısıtlama.|


## <a name="components"></a>Bileşenler

Data Box Edge çözümü Data Box Edge kaynağından, Data Box Edge fiziksel cihazından ve yerel bir web kullanıcı arabiriminden oluşur.

* **Data Box Edge fiziksel cihazı** - Microsoft tarafından sağlanan ve Azure'a veri gönderecek şekilde yapılandırılabilen 1U rafa takılan sunucu. 
    
* **Data Box Edge kaynağı** – Azure portalında, farklı coğrafi konumlardan erişebildiğiniz bir web arabiriminde Data Box Edge cihazını yönetmenize olanak tanıyan bir kaynak. Data Box Edge kaynağını kullanarak kaynakları oluşturun ve yönetin, cihazlarla uyarıları görüntüleyin ve yönetin, paylaşımları yönetin.  

    <!--![The Data Box Edge service in Azure portal](media/data-box-overview/data-box-Edge-service1.png)-->

    Daha fazla bilgi için [Data Box Edge cihazınız için sipariş oluşturma](data-box-edge-deploy-prep.md#create-a-new-resource)bölümüne gidin.

* **Data Box yerel web kullanıcı arabirimi** - Tanılama çalıştırmak, Data Box Edge cihazını kapatmak ve yeniden başlatmak, kopya günlüklerini görüntülemek ve hizmet isteğinde bulunmak üzere Microsoft Desteği'ne başvurmak için yerel web kullanıcı arabirimini kullanın.

    <!--![The Data Box Edge local web UI](media/data-box-Edge-overview/data-box-Edge-local-web-ui.png)-->

    Web tabanlı kullanıcı arabirimini kullanma hakkında bilgi için, [Data Box'ınızı yönetmek için web tabanlı kullanıcı arabirimini kullanma](data-box-edge-manage-access-power-connectivity-mode.md) konusuna gidin.


## <a name="region-availability"></a>Bölge kullanılabilirliği

Data Box Edge fiziksel cihazı, Azure kaynağı ve verileri aktardığınız hedef depolama hesabının üçünün de aynı bölgede bulunması gerekmez.

- **Kaynak kullanılabilirliği** -Data Box Edge kaynağın kullanılabildiği tüm bölgelerin listesi için, [bölgeye göre kullanılabilir Azure ürünlerine](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all)gidin. Data Box Edge, Azure Kamu Bulutu 'nda da dağıtılabilir. Daha fazla bilgi için bkz. [Azure Kamu nedir?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome).
    
- **Hedef Depolama hesapları**: Verilerin depolandığı depolama hesapları, tüm Azure bölgelerinde sağlanır. Depolama hesaplarının Data Box Edge verilerini depolayan bölgeler, cihazın en iyi performans için bulunduğu yere yakın yerde yerleştirilmelidir. Cihazdan uzağa konumlandırılan depolama hesabı uzun gecikme sürelerine ve daha yavaş bir performansa yol açar. 


## <a name="next-steps"></a>Sonraki adımlar

- [Data Box Edge sistem gereksinimlerini](data-box-edge-system-requirements.md) gözden geçirin.
- [Data Box Edge sınırlarını](data-box-edge-limits.md) anlayın.
- Azure portalda [Azure Data Box Edge](data-box-edge-deploy-prep.md)’i dağıtın.




