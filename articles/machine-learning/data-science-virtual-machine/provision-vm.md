---
title: 'Hızlı Başlangıç: Windows DSVM’si oluşturma'
description: Yapılandırma ve analiz için Azure'da bir veri bilimi sanal makinesi oluşturma ve makine öğrenimi.
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: quickstart
ms.date: 09/10/2019
ms.openlocfilehash: f543db0240d59cf99f3bd793e059b7985f1ffe22
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71170615"
---
# <a name="quickstart-set-up-the-data-science-virtual-machine-for-windows"></a>Hızlı Başlangıç: Windows için Veri Bilimi Sanal Makinesi ayarlama

Windows Veri Bilimi Sanal Makinesi ile çalışmaya başlayın.

## <a name="prerequisite"></a>Önkoşul

Bir Windows Veri Bilimi Sanal Makinesi oluşturmak için bir Azure aboneliğinizin olması gerekir. [Azure 'ı ücretsiz deneyin](https://azure.com/free).

## <a name="create-your-dsvm"></a>DSVM 'nizi oluşturma

DSVM örneği oluşturmak için:

1. Daha önce oturum açmadıysanız Azure hesabınızda oturum açmanız istenebilir [Azure Portal](https://portal.azure.com) gidin.
1. "Veri bilimi sanal makinesi" ni yazarak ve "Veri Bilimi Sanal Makinesi-Windows 2016" öğesini seçerek sanal makine listesini bulun.
    ![Windows VM listesi](./media/provision-vm/search-windows.png)
1. Alt kısımdaki **Oluştur** düğmesini seçin.

   ![Azure portal, Oluştur düğmesi ile, sanal makine listesi](./media/provision-vm/create-windows.png)

1. "Sanal makine oluşturma" dikey penceresine yönlendirilmelisiniz.
   ![Windows sanal makinesine karşılık gelen temel bilgiler sekmesi](./media/provision-vm/review-create-windows.png)

1. **Temel bilgiler** sekmesini doldur:
      * **Abonelik**: Birden fazla aboneliğiniz varsa makinenin oluşturulacağı ve faturalandırılabileceği bir tane seçin. Bu abonelikte kaynak oluşturma ayrıcalıklarına sahip olmanız gerekir.
      * **Kaynak grubu**: Yeni bir grup oluşturun veya var olan bir grubu kullanın.
      * **Sanal makine adı**: Sanal makinenin adını girin. Azure portal bu şekilde görünür.
      * **Konum**: En uygun veri merkezini seçin. En hızlı ağ erişimi için bu, verilerinizin çoğunu veya fiziksel konumunuza en yakın olan veri merkezdir. [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)hakkında daha fazla bilgi edinin.
      * **Görüntü**: Varsayılan değeri değiştirmeyin.
      * **Boyutu**: Bu, genel iş yükleri için uygun bir boyutla otomatik olarak doldurulur. [Azure 'Da WINDOWS VM boyutları](../../virtual-machines/windows/sizes.md)hakkında daha fazla bilgi edinin.
      * **Kullanıcı adı**: Yönetici Kullanıcı adını girin. Bu, sanal makinenizde oturum açmak için kullanacağınız Kullanıcı adı ve Azure Kullanıcı adınızla aynı olmaması gerekir.
      * **Parola**: Sanal makinenizde oturum açmak için kullanacağınız parolayı girin.    
1. **İncele ve oluştur**’u seçin.
1. **Gözden geçir + oluştur**
   * Girdiğiniz tüm bilgilerin doğru olduğundan emin olun. 
   * **Oluştur**’u seçin.


> [!NOTE]
> * Sanal makinede önceden yüklenmiş olarak gelen yazılımlar için lisans ücretleri ödemezsiniz. **Boyut** adımında seçtiğiniz sunucu boyutu için işlem maliyetini ödeyin.
> * Sağlama 10 ila 20 dakika sürer. Azure portal sanal makinenizin durumunu görüntüleyebilirsiniz.

## <a name="access-the-dsvm"></a>DSVM 'ye erişme

VM oluşturulup sağlandıktan sonra, [Azure tabanlı sanal makinenize bağlanmak](../../marketplace/cloud-partner-portal/virtual-machine/cpp-connect-vm.md)için listelenen adımları izleyin. Bir sanal makine oluşturmanın **temelleri** adımında yapılandırdığınız Yönetici hesabı kimlik bilgilerini kullanın. 

Yüklenmiş ve yapılandırılmış VM'de araçları kullanmaya başlamak hazırsınız demektir. Araçların çoğuna **Başlangıç** menüsü kutucukları ve Masaüstü simgeleri aracılığıyla erişilebilir.

Ayrıca, sanal makine üzerinde jupi not defterlerini çalıştırmak ve ücretsiz hizmet katmanının sınırlamalarını atlamak için Azure Notebooks bir DSVM ekleyebilirsiniz. Daha fazla bilgi için bkz. [Not defteri projelerini yönetme ve yapılandırma](../../notebooks/configure-manage-azure-notebooks-projects.md#manage-and-configure-projects).

<a name="tools"></a>


## <a name="next-steps"></a>Sonraki adımlar

* **Başlat** menüsünü açarak dsvm 'deki araçları araştırın.
* [Azure Machine Learning hizmetin ne olduğunu](../service/overview-what-is-azure-ml.md) okuyarak Azure Machine Learning hizmeti hakkında bilgi edinin ve [öğreticiler](../index.yml)deneyin.
* Dosya Gezgini 'nde, kurumsal ölçekte veri analizlerini destekleyen R 'deki Iptal edilmiş bir kitaplığı kullanan örnekler için C:\Program Files\Microsoft\ML Server\r_server\library\unsıscaler\demoscripts konumuna göz atın. 
* [Veri bilimi sanal makinesi için kullanabileceğiniz on şey](https://aka.ms/dsvmtenthings)makalesini okuyun.
* Sistematik olarak kullanarak uçtan uca analitik çözümler oluşturmayı öğrenin [Team Data Science Process](../team-data-science-process/index.yml).
* Ziyaret [Azure AI Gallery](https://gallery.cortanaintelligence.com) Azure'da Azure Machine Learning ve ilgili verileri kullanan makine öğrenimi ve veri analizi için örnekleri Hizmetleri. Ayrıca, bu Galeri için **Başlat** menüsünde ve sanal makinenin masaüstüne bir simge sağladık.
* Bu sanal makine için uygun [başvuru belgelerine](./reference-windows-vm.md) bakın.

