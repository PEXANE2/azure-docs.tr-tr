---
title: Sanal makinelerde MATLAB kümeleri
description: İşlem yoğun paralel MATLAB iş yüklerinizi çalıştırmak için MATLAB Distributed Computing Server kümelerini oluşturmak için Microsoft Azure sanal makinelerini kullanın
author: mscurrell
ms.service: virtual-machines-windows
ms.subservice: workloads
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 05/09/2016
ms.author: markscu
ms.openlocfilehash: d34481587fd48e2eddfd268c39f6bc4f7c4e0c76
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869413"
---
# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>Azure VM'lerde MATLAB Dağıtılmış Bilgi İşlem Sunucusu kümeleri oluşturma
İşlem yoğun paralel MATLAB iş yüklerinizi çalıştırmak için bir veya daha fazla MATLAB Dağıtılmış Bilgi İşlem Sunucusu kümeleri oluşturmak için Microsoft Azure sanal makinelerini kullanın. MATLAB Distributed Computing Server yazılımınızı temel görüntü olarak kullanmak üzere bir VM'ye yükleyin ve kümeyi dağıtmak ve yönetmek için Azure quickstart şablonu veya Azure PowerShell komut dosyası [(GitHub'da](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)kullanılabilir) kullanın. Dağıtımdan sonra, iş yüklerinizi çalıştırmak için kümeye bağlanın.

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>MATLAB ve MATLAB Distributed Computing Server Hakkında
[MATLAB](https://www.mathworks.com/products/matlab/) platformu mühendislik ve bilimsel sorunları çözmek için optimize edin. Büyük ölçekli simülasyonlara ve veri işleme görevlerine sahip MATLAB kullanıcıları, bilgi işlem kümeleri ve ızgara hizmetlerinden yararlanarak bilgi işlem yoğun iş yüklerini hızlandırmak için MathWorks paralel bilgi işlem ürünlerini kullanabilir. [Paralel Bilgi İşlem Araç Kutusu,](https://www.mathworks.com/products/parallel-computing/) MATLAB kullanıcılarının uygulamaları paralelleştirmesini ve çok çekirdekli işlemciler, GPU'lar ve bilgi işlem kümelerinden yararlanmalarını sağlar. [MATLAB Distributed Computing Server,](https://www.mathworks.com/products/distriben/) MATLAB kullanıcılarının bir bilgi işlem kümesinde birçok bilgisayarı kullanmalarını sağlar.

Azure sanal makinelerini kullanarak, etkileşimli işler, toplu iş işleri, bağımsız görevler ve iletişim görevleri gibi şirket içi kümeler halinde paralel çalışma göndermek için aynı mekanizmalara sahip MATLAB Dağıtılmış Bilgi İşlem Sunucusu kümeleri oluşturabilirsiniz. Azure'un MATLAB platformuyla birlikte kullanılmasının, geleneksel şirket içi donanımı sağlama ve kullanmayla karşılaştırıldığında birçok faydası vardır: bir dizi sanal makine boyutu, isteğe bağlı kümeler oluşturulması, böylece yalnızca kullandığınız bilgi işlem kaynakları için ödeme yapma ve modelleri ölçekte test etme olanağı.  

## <a name="prerequisites"></a>Önkoşullar
* **İstemci bilgisayar** - Dağıtımdan sonra Azure ve MATLAB Distributed Computing Server kümesiyle iletişim kurmak için Windows tabanlı bir istemci bilgisayara ihtiyacınız vardır.
* **Azure PowerShell** - Azure [PowerShell'i istemci bilgisayarınıza yüklemek için nasıl yükleyip yapılandırılabildiğini](/powershell/azure/overview) öğrenin.
* **Azure aboneliği** - Aboneliğiniz yoksa, birkaç dakika içinde ücretsiz bir [hesap](https://azure.microsoft.com/free/) oluşturabilirsiniz. Daha büyük kümeler için, istediğiniz kadar öde aboneliği veya diğer satın alma seçeneklerini göz önünde bulundurun.
* **vCPUs kotası** - Büyük bir kümeyi veya birden fazla MATLAB Dağıtılmış Bilgi İşlem Sunucusu kümesini dağıtmak için vCPU kotasını artırmanız gerekebilir. Kotayı artırmak için ücretsiz [olarak çevrimiçi müşteri destek isteği açın.](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
* **MATLAB, Parallel Computing Toolbox ve MATLAB Distributed Computing Server lisansları** - Komut dosyaları [MathWorks Hosted License Manager'ın](https://www.mathworks.com/help/install/license-management.html) tüm lisanslar için kullanıldığını varsayar.  
* **MATLAB Distributed Computing Server yazılımı** - Küme VM'leri için temel VM görüntüsü olarak kullanılacak bir VM'ye yüklenir.

## <a name="high-level-steps"></a>Üst düzey adımlar
MATLAB Distributed Computing Server kümeleriniz için Azure sanal makinelerini kullanmak için aşağıdaki üst düzey adımlar gereklidir. Ayrıntılı [talimatlar, GitHub'daki](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)hızlı başlatma şablonu ve komut dosyalarıyla birlikte belgelerde bulunmaktadır.

1. **Temel VM görüntüsü oluşturma**  

   * MATLAB Distributed Computing Server yazılımını bu VM'ye indirin ve yükleyin.

     > [!NOTE]
     > Bu işlem birkaç saat sürebilir, ancak kullandığınız MATLAB her sürümü için sadece bir kez yapmanız gerekir.   
     >
     >
2. **Bir veya daha fazla küme oluşturma**  

   * Sağlanan PowerShell komut dosyasını kullanın veya temel VM görüntüsünden bir küme oluşturmak için hızlı başlangıç şablonuna kullanın.   
   * Kümeleri listelemenize, duraklatmanıza, devam ettirmenize ve silmenize olanak tanıyan powershell komut dosyasını kullanarak kümeleri yönetin.

## <a name="cluster-configurations"></a>Küme yapılandırmaları
Şu anda, küme oluşturma komut dosyası ve şablon tek bir MATLAB Distributed Computing Server topolojisi oluşturmanıza olanak sağlar. İstersenizle, her kümefarklı VM boyutları kullanarak farklı sayıda alt VM'ye sahip olan bir veya daha fazla ek küme oluşturun.

### <a name="matlab-client-and-cluster-in-azure"></a>Azure'da MATLAB istemcisi ve küme
MATLAB istemci düğümü, MATLAB İş Zamanlayıcısı düğümü ve MATLAB Distributed Computing Server "worker" düğümlerinin tümü, aşağıdaki şekilde gösterildiği gibi sanal ağdaki Azure VM'ler olarak yapılandırılır.


* Kümeyi kullanmak için Uzak Masaüstü'ne bağlayın. İstemci düğümü MATLAB istemcisini çalıştırZ.
* İstemci düğümü, tüm çalışanlar tarafından erişilebilen bir dosya paylaşımına sahiptir.
* MathWorks Hosted Lisans Yöneticisi, tüm MATLAB yazılımları için lisans denetimleri için kullanılır.
* Varsayılan olarak, vCPU başına bir MATLAB Distributed Computing Server çalışanı, işçi VM'lerinde oluşturulur, ancak herhangi bir sayı belirtebilirsiniz.

## <a name="use-an-azure-based-cluster"></a>Azure tabanlı küme kullanma
Diğer MATLAB Dağıtılmış Bilgi İşlem Sunucusu kümelerinde olduğu gibi, MATLAB istemcisinde (istemci VM'de) Küme Profil Yöneticisi'ni kullanarak Bir MATLAB İş Zamanlayıcısı küme profili oluşturmanız gerekir.

![Küme Profil Yöneticisi](./media/matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Sonraki adımlar
* Azure'da MATLAB Dağıtılmış Bilgi İşlem Sunucusu kümelerini dağıtmak ve yönetmek için ayrıntılı talimatlar için şablonları ve komut dosyalarını içeren [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) deposuna bakın.
* MATLAB ve MATLAB Distributed Computing Server için ayrıntılı belgeler için [MathWorks sitesine](https://www.mathworks.com/) gidin.
