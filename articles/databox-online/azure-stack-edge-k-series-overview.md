---
title: Azure Stack Edge Mini e-Özeti | Microsoft Docs
description: Azure 'a Wi-Fi üzerinden aktarım için pili olan taşınabilir fiziksel bir cihaz kullanan askeri bir depolama çözümü olan Azure Stack Edge Mini R ' yi açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 09/10/2020
ms.author: alkohli
ms.openlocfilehash: 1ea04633708e866b6f200bef0eb6e331c06873e9
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948246"
---
# <a name="what-is-the-azure-stack-edge-mini-r"></a>Azure Stack Edge Mini R nedir?

Azure Stack Edge Mini R, bir hizmet olarak donanım çözümüdür. Microsoft, hızlandırılmış AI-ınrugged ve bir ağ depolama ağ geçidi 'nin tüm özelliklerine sahip olan yerleşik bir görüntü Işleme birimi (VPU) ile birlikte, bulut tarafından yönetilen ve ultra taşınabilir bir cihaz sunar. Bu cihazlar, en zorlu ortamlarda kullanılmak üzere uygundur ve AI, analiz ve sunucusuz bilgi işlem için iyileştirilmiştir.

Bu makalede, Azure Stack Edge Mini R çözümüne, önemli yeteneklere ve bu cihazı dağıtabileceğiniz senaryolara ilişkin bir genel bakış sunulmaktadır.


## <a name="key-capabilities"></a>Temel işlevler

Azure Stack Edge Mini R aşağıdaki yeteneklere sahiptir:

|Özellik |Açıklama  |
|---------|---------|
|Rugged donanımı| Rugged, en zorlu ortamlar için tasarlanan donanım.|
|Ultra taşınabilir| Ultra taşınabilir, pille çalıştırılan form faktörü.|
|Bulutta yönetilen|Cihaz ve hizmet, Azure portal aracılığıyla yönetilir.|
|Edge işlem iş yükleri|Verilerin analizine, işlenmesine, filtrelenmesine olanak tanır.<br>VM 'Leri ve Kapsayıcılı iş yüklerini destekler. |
|Hızlandırılmış AI ınkrii| Intel Movidius Myriad X VPU tarafından etkinleştirilir.|
|Kablolu ve kablosuz | Kablolu ve kablosuz veri aktarımlarına izin verir.|
|Veri erişimi     | Bulutta ek veri işleme için bulut API'lerini kullanarak Azure Depolama Blobları ve Azure Dosyaları'ndan doğrudan veri erişimi. Cihazdaki yerel önbellek, en son kullanılan dosyalara hızlı erişim için kullanılır.|
|Bağlantısı kesik mod| Cihaz ve hizmet, isteğe bağlı olarak Azure Stack aracılığıyla yönetilebilir. <br> Uygulamaları çevrimdışı modda dağıtın, çalıştırın, yönetin. <br> Bağlantısız mod, çevrimdışı karşıya yükleme senaryolarını destekler.|
|Desteklenen protokoller     |Veri alımı için standart SMB, NFS ve REST protokollerini destekler. <br> Desteklenen sürümler hakkında daha fazla bilgi için [Azure Stack Edge Mini R sistem gereksinimleri](azure-stack-edge-gpu-system-requirements.md)' ne gidin.|
|Veri yenileme     | Yerel dosyaları buluttaki en son sürümle yenileme olanağı.|
|Çift şifreleme    | Kendi kendine şifrelenen sürücü kullanımı, ilk şifreleme katmanını sağlar. VPN ikinci şifreleme katmanını sağlar. Verileri yerel olarak şifrelemek ve *https* üzerinden buluta veri aktarımını güvenli hale getirmek için BitLocker desteği.|
|Bant genişliği azaltma| Yoğun saatlerde bant genişliği kullanımını sınırlandırmaya kısıtlama.|

## <a name="use-cases"></a>Uygulama alanları

Azure Stack Edge Mini R 'nin, en uçta hızlı Machine Learning (ML) için kullanılabileceği ve verileri Azure 'a göndermeden önce ön işlemesi için kullanabileceğiniz çeşitli senaryolar aşağıda verilmiştir.

- **Azure Machine Learning Ile çıkarım** -Azure Stack Edge Mini R ile, veri buluta gönderilmeden önce üzerinde işlem yapılabilir hızlı sonuçlar almak için ml modellerini çalıştırabilirsiniz. Tam veri kümesi, ML modellerinizi yeniden eğitmeye ve artırmaya devam etmek için isteğe bağlı olarak aktarılabilir. Azure Stack Edge Mini R cihazında Azure ML donanım hızlandırılmış modellerini kullanma hakkında daha fazla bilgi için bkz. [Azure Stack Edge Mini r üzerinde Azure ML donanım hızlandırılmış modellerini dağıtma](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-fpga-web-service#deploy-to-a-local-edge-server).

- Daha önceden uygulanabilir bir veri kümesi oluşturmak üzere **verileri** Azure 'a göndermeden önce kapsayıcılar veya sanal makineler gibi işlem seçenekleri aracılığıyla veri dönüştürme. Önceden işleme şu amaçlarla kullanılabilir:

    - Verileri toplama.
    - Verileri değiştirin, örneğin kişisel verileri kaldırmak için.
    - Depolama ve bant genişliğini iyileştirmek veya daha fazla analiz için verileri alt kümele.
    - IoT Olaylarını analiz etme ve bunlara yanıt verme.

- **Ağ üzerinden Azure 'a veri aktarma** -daha fazla bilgi işlem ve analiz sağlamak veya arşivleme amacıyla verileri kolayca ve hızlı bir şekilde Azure 'a aktarmak Için Azure Stack Edge Mini R 'yi kullanın.

## <a name="components"></a>Bileşenler

Azure Stack Edge Mini R çözümü, bir Azure Stack Edge kaynağı, Azure Stack Edge Mini R Rugged, ultra taşınabilir fiziksel cihaz ve yerel bir Web kullanıcı arabiriminden oluşur.

* **Azure Stack Edge Mini R fiziksel cihaz** -yerleşik bir pil ve Microsoft tarafından sağlanan ve Azure 'a veri gönderecek şekilde yapılandırılabilen bir Rugged Me ile ultra taşınabilir bir cihaz. Pili 7 ' den az olan cihaz.

    ![Azure Stack Edge Mini R cihazı](media/azure-stack-edge-k-series-overview/perspective-view-1.png)

* **Azure Stack Edge kaynağı** : bir rugged, Azure Stack Edge Mini R cihazını farklı coğrafi konumlardan erişebileceğiniz bir web arabiriminden yönetmenize olanak tanıyan bir Azure Portal kaynaktır. Kaynak oluşturmak ve yönetmek, cihazları ve uyarıları görüntülemek ve yönetmek ve paylaşımları yönetmek için Azure Stack Edge kaynağını kullanın.  

* **Azure Stack Edge Mini R yerel Web Kullanıcı arabirimi** -ilk cihaz yapılandırması için yerel Web Kullanıcı arabirimini kullanın, tanılamayı çalıştırmak Için Azure Stack Edge Mini R cihazını kapatıp yeniden başlatın, kopyalama günlüklerini görüntüleyin ve hizmet isteği dosyasına Microsoft desteği başvurun.


## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure Stack Edge Mini R fiziksel cihazı, Azure kaynağı ve veri aktarımı yaptığınız hedef depolama hesabı, tümünün aynı bölgede olması gerekmez.

- **Kaynak kullanılabilirliği** -Azure Stack Edge kaynağının kullanılabildiği tüm bölgelerin listesi için [bölgeye göre kullanılabilir Azure ürünlerine](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all)gidin. 

- **Hedef Depolama hesapları**: Verilerin depolandığı depolama hesapları, tüm Azure bölgelerinde sağlanır. Depolama hesaplarının Azure Stack Edge Mini R verileri, cihazın en iyi performans için bulunduğu konuma yakın olması gerekir. Cihazdan uzağa konumlandırılan depolama hesabı uzun gecikme sürelerine ve daha yavaş bir performansa yol açar.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Edge Mini R sistem gereksinimlerini](azure-stack-edge-gpu-system-requirements.md)gözden geçirin.


