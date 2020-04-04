---
title: Azure Veri Bilimi Sanal Makine Nedir
titleSuffix: Azure Data Science Virtual Machine
description: Azure Veri Bilimi Sanal Makinesine Genel Bakış - Veri bilimi yapmak ve akıllı uygulamalar geliştirmek için önceden yüklenmiş ve yapılandırılmış araçlar ve kitaplıklarla Azure bulut platformunda sanal makine oluşturması ve kullanması kolay.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: overview
ms.date: 04/02/2020
ms.openlocfilehash: 074b547917b65a411d2b1d1b2f094156828eef78
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631387"
---
# <a name="what-is-the-azure-data-science-virtual-machine-for-linux-and-windows"></a>Linux ve Windows için Azure Veri Bilimi Sanal Makinesi nedir?

Data Science Virtual Machine (DSVM), Azure bulut platformunda veri bilimi yapmak için özel olarak oluşturulmuş özelleştirilmiş bir VM görüntüsüdür. Gelişmiş analitik için akıllı uygulamalar oluşturmak için önceden yüklenmiş ve önceden yapılandırılmış birçok popüler veri bilimi aracı vardır. 

DSVM şu konularda kullanılabilir:

+ **Windows Server 2019**
+ **Ubuntu 18.04 LTS**
+ Windows Server 2016
+ Ubuntu 16.04 LTS ve CentOS 7.4

> [!NOTE]
> Derin öğrenme için tüm VM araçları Veri Bilimi Sanal Makine içine katlanmış. 

## <a name="why-choose-the-dsvm"></a>Neden DSVM'yi seçmelisiniz?

Veri Bilimi Sanal Makine'nin amacı, tüm beceri düzeylerinde ve endüstriler arasında sürtünmesiz, önceden yapılandırılmış veri bilimi ortamı yla veri profesyonellerine sunmaktır. Kendi karşılaştırılabilir bir çalışma alanı kullanıma çıkarmak yerine, bir DSVM sağlayabilirsiniz. Bu seçim, yükleme, yapılandırma ve paket yönetimi süreçlerinde günler, hatta _haftalar_ calısabilir. DSVM ayrıldıktan sonra veri bilimi projenizin üzerinde çalışmaya hemen başlayabilirsiniz.

## <a name="sample-use-cases"></a>Örnek Kullanım Örnekleri

Aşağıda, DSVM müşterileri için bazı yaygın kullanım örneklerini göstereceğiz.

### <a name="moving-data-science-workloads-to-the-cloud"></a>Veri bilimi iş yüklerini buluta taşıma

DSVM, yerel masaüstü bilgisayarlarını yönetilen bir bulut masaüstüyle değiştirmek isteyen veri bilimi ekipleri için temel bir yapılandırma sağlayarak, ekipteki tüm veri bilimcilerin denemeleri doğrulamak ve işbirliğini teşvik etmek için tutarlı bir kuruluma sahip olmasını sağlar. Ayrıca sysadmin yükünü azaltarak maliyetleri düşürür. Bu yük azaltma, gelişmiş analitik için yazılım paketlerini değerlendirmek, yüklemek ve korumak için gereken zamandan tasarruf sağlar.

### <a name="data-science-training-and-education"></a>Veri bilimi eğitimi

Veri bilimi dersleri veren kurumsal eğitmenler ve eğitimciler genellikle sanal bir makine görüntüsü sağlar. Görüntü, öğrencilerin tutarlı bir kuruluma sahip olmasını ve örneklerin tahmin edilebilir şekilde çalışmasını sağlar. 

DSVM, destek ve uyumsuzluk zorluklarını hafifleten tutarlı bir kurulumla isteğe bağlı bir ortam oluşturur. Başta daha kısa eğitim sınıfları olmak üzere bu ortamların sıklıkla oluşturulması gereken durumlar önemli ölçüde avantajlıdır.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Büyük ölçekli projeler için isteğe bağlı esnek kapasite

Veri bilimi hackathons / yarışmalar veya büyük ölçekli veri modelleme ve keşif genellikle kısa bir süre için, ölçekli-out donanım kapasitesi gerektirir. DSVM, yüksek güçlü bilgi işlem kaynaklarının çalıştırabileceği denemelere izin veren ölçeklenmiş sunucularda, veri bilimi ortamının isteğe bağlı olarak hızlı bir şekilde çoğaltılmasında yardımcı olabilir.

### <a name="custom-compute-power-for-azure-notebooks"></a>Azure Dizüstü Bilgisayarlar için özel işlem gücü

[Azure Dizüstü Bilgisayarlar,](../../notebooks/azure-notebooks-overview.md) Jupyter dizüstü bilgisayarlarını yükleme olmadan bulutta geliştirmek, çalıştırmak ve paylaşmak için ücretsiz barındırılan bir hizmettir. Ücretsiz hizmet katmanı 4 GB bellek ve 1 GB veri ile sınırlıdır. 

Tüm sınırları serbest bırakmak için, Bir DSVM'ye veya Jupyter sunucusunda çalışan başka bir VM'ye Dizüstü Bilgisayar projesi ekleyebilirsiniz. Azure Active Directory (şirket hesabı gibi) kullanarak bir hesapla Azure Not Defterleri'nde oturum açtığınızda, Not Defterleri bu hesapla ilişkili aboneliklerde DSVM'leri otomatik olarak gösterir. Kullanılabilir işlem gücünü genişletmek [için Azure Not Defterlerine Bir DSVM ekleyebilirsiniz.](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier)

### <a name="short-term-experimentation-and-evaluation"></a>Kısa süreli deneme ve değerlendirme

DSVM'yi, özellikle yayınlanmış [örneklerimizden ve gözden geçirmelerimizden](./dsvm-samples-and-walkthroughs.md)bazılarını inceleyerek, yeni veri bilimi [araçlarını](./tools-included.md)değerlendirmek veya öğrenmek için kullanabilirsiniz.


### <a name="deep-learning-with-gpus"></a>GPU'larla derin öğrenme

DSVM'de, eğitim modelleriniz grafik işleme birimlerine (GPU) dayalı donanımlarda derin öğrenme algoritmaları kullanabilir. DSVM, Azure platformunun VM ölçekleme özelliklerinden yararlanarak, bulutta GPU tabanlı donanımı ihtiyaçlarınıza göre kullanmanıza yardımcı olur. Büyük modelleri eğitirken veya aynı işletim sistemi diskini tutarken yüksek hızlı hesaplamalara ihtiyaç duyduğunuzda GPU tabanlı bir VM'ye geçebilirsiniz. DSVM özellikli N serisi GPU özellikli sanal makine SKU'lardan herhangi birini seçebilirsiniz. Azure ücretsiz hesaplarının GPU özellikli sanal makine SKU'larını desteklemediğini lütfen unutmayın.

DSVM'nin Windows Server 2016 sürümü, derin öğrenme çerçevelerinin GPU sürücüleri, çerçeveleri ve GPU sürümleriyle önceden yüklenmiş olarak gelir. Linux sürümünde, Hem CentOS hem de Ubuntu DSVM'lerde GPU'larda derin öğrenme etkindir. 

DSVM'nin Ubuntu, CentOS veya Windows 2016 sürümünü GPU'lara dayanmayan bir Azure sanal makinesine de dağıtabilirsiniz. Bu durumda, tüm derin öğrenme çerçeveleri CPU moduna geri düşecek.

[Mevcut derin öğrenme ve AI çerçeveleri hakkında daha fazla bilgi edinin.](dsvm-tools-deep-learning-frameworks.md)

<a name="included"></a>
## <a name="whats-included-on-the-dsvm"></a>DSVM'de neler var?

Windows ve Linux DSVM's hem araçların tam [listesine bakın burada](tools-included.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerle daha fazla bilgi edinin:

+ Windows:
  + [Windows DSVM’si ayarlama](provision-vm.md)
  + [Windows DSVM'de yapabileceğiniz on şey](vm-do-ten-things.md)

+ Linux:
  + [Linux DSVM (Ubuntu) ayarlama](dsvm-ubuntu-intro.md)
  + [Linux DSVM (CentOS) ayarlama](linux-dsvm-intro.md)
  + [Linux DSVM'de veri bilimi](linux-dsvm-walkthrough.md)
