---
title: 'Quickstart: Windows Veri Bilimi Sanal Makine Oluşturma'
titleSuffix: Azure Data Science Virtual Machine
description: Analitik ve makine öğrenimi için Azure'da bir Veri Bilimi Sanal Makinesi'ni yapılandırın ve oluşturun.
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: quickstart
ms.date: 12/31/2019
ms.openlocfilehash: afcb676f68e7be9d3ebef11ea2c6876a86bbd062
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80281791"
---
# <a name="quickstart-set-up-the-data-science-virtual-machine-for-windows"></a>Quickstart: Windows için Veri Bilimi Sanal Makine'yi ayarlama

Windows Server 2019 Data Science Virtual Machine ile çalışmaya devam edin.

## <a name="prerequisite"></a>Önkoşul

Windows Veri Bilimi Sanal Makinesi oluşturmak için bir Azure aboneliğiniz olması gerekir. [Azure'u ücretsiz deneyin.](https://azure.com/free)
Azure ücretsiz hesaplarının GPU özellikli sanal makine SKU'larını desteklemediğini lütfen unutmayın.

## <a name="create-your-dsvm"></a>DSVM'nizi oluşturun

Bir DSVM örneği oluşturmak için:

1. [Azure portalına](https://portal.azure.com) gidin, zaten oturum açmadıysanız Azure hesabınızda oturum açmanız istenebilir.
1. "Veri bilimi sanal makine" yazarak ve "Data Science Virtual Machine - Windows 2019" seçeneğini seçerek sanal makine girişini bulun.

1. Alttaki **Oluştur** düğmesini seçin.

1. "Sanal bir makine oluştur" bıçağına yönlendirilmelisiniz.

1. **Temel Bilgiler** sekmesini doldurun:
      * **Abonelik**: Birden fazla aboneliğiniz varsa, makinenin oluşturulacağı ve faturalandırılacağı aboneliği seçin. Bu abonelikte kaynak oluşturma ayrıcalıklarına sahip olmanız gerekir.
      * **Kaynak grubu**: Yeni bir grup oluşturun veya varolan bir grup kullanın.
      * **Sanal makine adı**: Sanal makinenin adını girin. Azure portalınızda bu şekilde görünür.
      * **Konum**: En uygun veri merkezini seçin. En hızlı ağ erişimi için, verilerinizin çoğuna sahip olan veya fiziksel konumunuza en yakın olan veri merkezidir. [Azure Bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)hakkında daha fazla bilgi edinin.
      * **Resim**: Varsayılan değeri bırakın.
      * **Boyut**: Bu, genel iş yükleri için uygun bir boyutla otomatik doldurmalıdır. [Azure'da Windows VM boyutları](../../virtual-machines/windows/sizes.md)hakkında daha fazla bilgi edinin.
      * **Kullanıcı Adı**: Yönetici kullanıcı adını girin. Bu, sanal makinenize giriş yapmak için kullanacağınız kullanıcı adıdır ve Azure kullanıcı adınız ile aynı olması gerekmez.
      * **Şifre**: Sanal makinenize giriş yapmak için kullanacağınız şifreyi girin.    
1. **İncele ve oluştur**’u seçin.
1. **İnceleme+oluştur**
   * Girdiğiniz tüm bilgilerin doğru olduğundan doğrulayın. 
   * **Oluştur'u**seçin.


> [!NOTE]
> * Sanal makineye önceden yüklenmiş olarak gelen yazılımlar için lisans ücreti ödemezsiniz. **Boyut** adımında seçtiğiniz sunucu boyutu için bilgi işlem maliyetini ödersiniz.
> * Sağlama 10 ila 20 dakika sürer. Azure portalında VM'nizin durumunu görüntüleyebilirsiniz.

## <a name="access-the-dsvm"></a>DSVM'ye erişin

VM oluşturulduktan ve sağlandıktan sonra, [Azure tabanlı sanal makinenize bağlanmak](../../marketplace/cloud-partner-portal/virtual-machine/cpp-connect-vm.md)için listelenen adımları izleyin. Sanal makine oluşturmak için **Temeller** adımında yapılandırdığınız yönetici hesabı kimlik bilgilerini kullanın. 

VM'de yüklenen ve yapılandırılan araçları kullanmaya başlamaya hazırsınız. Araçların çoğuna **Başlat** menüsü kutucukları ve masaüstü simgeleri aracılığıyla erişilebilir.

Ayrıca, VM'de Jupyter dizüstü bilgisayarları çalıştırmak ve ücretsiz hizmet katmanının sınırlamalarını atlamak için Azure Not Defterlerine bir DSVM ekleyebilirsiniz. Daha fazla bilgi için Bkz. [Not Defterleri projelerini yönet ve yapılandırma.](../../notebooks/configure-manage-azure-notebooks-projects.md#manage-and-configure-projects)

<a name="tools"></a>


## <a name="next-steps"></a>Sonraki adımlar

* **Başlat** menüsünü açarak DSVM'deki araçları keşfedin.
* [Azure Machine Learning nedir](../overview-what-is-azure-ml.md) okuyarak ve [öğreticileri](../index.yml)deneyerek Azure Machine Learning hakkında bilgi edinin.
* Makaleyi okuyun [Veri Bilimi Sanal Makine üzerinde yapabileceğiniz on şey](https://aka.ms/dsvmtenthings).
* Azure Makine [Öğrenimi](https://gallery.cortanaintelligence.com) ve Azure'daki ilgili veri hizmetlerini kullanan makine öğrenimi ve veri analizi örnekleri için Azure Yapay Değer Galerisi'ni ziyaret edin. Ayrıca **Başlat** menüsünde ve sanal makinenin masaüstünde bu galeri için bir simge sağladık.

