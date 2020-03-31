---
title: 'Quickstart: Bir Geo AI Veri Bilimi Sanal Makine oluşturma'
titleSuffix: Azure Data Science Virtual Machine
description: Jeouzamsal analitik ve makine öğrenimi için Azure'da bir Geo AI Data Science Virtual Machine yapılandırın ve oluşturun.
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: quickstart
ms.date: 09/13/2019
ms.openlocfilehash: f3ff9bd64f54d8f83fd1889078e8a4c01827d135
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77525898"
---
# <a name="quickstart-set-up-a-geo-artificial-intelligence-virtual-machine-on-azure"></a>Quickstart: Azure'da Bir Geo Yapay Zeka Sanal Makinesi Ayarlama 

Geo AI Veri Bilimi Sanal Makine (Geo-DSVM), yapay bilgi ve jeouzamsal analitiği birleştirmek üzere özel olarak yapılandırılan popüler [Azure Veri Bilimi Sanal Makinesi'nin](https://aka.ms/dsvm) bir uzantısıdır. VM jeouzamsal analitik [ArcGIS Pro](https://www.arcgis.com/features/index.html)tarafından desteklenmektedir. Data Science Virtual Machine (DSVM), makine öğrenimi ve hatta derin öğrenme modellerinin hızlı bir şekilde eğitilmesini sağlar. Bu modelleri geliştirmek için, coğrafi bilgilerle zenginleştirilmiş verileri kullanır. Geo-DSVM yalnızca Windows 2016 DSVM'de desteklenir. 

Geo-DSVM'de yer alan AI araçları şunlardır:

- Microsoft Cognitive Toolkit, TensorFlow, Keras, Caffe2 ve Chainer gibi popüler derin öğrenme çerçevelerinin GPU sürümleri
- Görüntü ve metinsel veri elde etme ve önişleme araçları
- Microsoft Machine Learning Server Developer Edition, Anaconda Python, Python ve R için Jupyter dizüstü bilgisayarlar, Python ve R için IDA'lar ve SQL veritabanları gibi geliştirme etkinlikleri için araçlar
- AI uygulamalarınızdaki jeouzamsal verilerle çalışabilen Python ve R arayüzleri ile birlikte ESRI'nın ArcGIS Pro masaüstü yazılımı
 

## <a name="create-your-geo-ai-data-science-vm"></a>Geo AI Veri Bilimi VM'nizi oluşturun

Geo AI Data Science VM'nin bir örneğini oluşturmak için aşağıdaki adımları izleyin:

1. [Azure portalındaki](https://ms.portal.azure.com/#create/microsoft-ads.geodsvmwindows)sanal makine listesine gidin.
1. Sihirbaz oluşturmak için en altta **Oluştur'u** seçin:

   ![oluşturmak-geo-ai-dsvm](./media/provision-geo-ai-dsvm/Create-Geo-AI.png)

1. Sihirbaz, dört adımın her biri için giriş gerektirir. Bu giriş hakkında ayrıntılı bilgi için aşağıdaki bölüme bakın.

### <a name="wizard-details"></a>Sihirbaz ayrıntıları ###

**Temel Bilgiler**:

- **Adı**: Oluşturduğunuz veri bilimi sunucusunun adı.
    
- **Kullanıcı adı**: Yönetici hesabı oturum açma kimliği.
    
- **Şifre**: Yönetici hesap şifresi.
    
- **Abonelik**: Birden fazla aboneliğiniz varsa, makinenin oluşturulup faturalandırılacak olan ını seçin.
    
- **Kaynak grubu**: Yeni bir grup oluşturabilir veya aboneliğinizde **boş** bir varolan Azure kaynak grubu kullanabilirsiniz.
    
- **Konum**: En uygun veri merkezini seçin. Genellikle, verilerinizin çoğuna sahip olan veya en hızlı ağ erişimi için fiziksel konumunuza en yakın olandır. GPU'da derin öğrenme yi planlıyorsanız, Azure'da NC Serisi GPU VM örneklerine sahip konumlardan birini seçmeniz gerekir. Şu anda bu yerler şunlardır: **Doğu ABD, Kuzey Orta ABD, Güney Orta ABD, Batı ABD 2, Kuzey Avrupa, Batı Avrupa**. En son liste [için, Bölgeye göre Azure Ürünleri](https://azure.microsoft.com/regions/services/) sayfasını kontrol edin ve **Compute**altında **NC-Serisi'ni** arayın. 
    
    
**Ayarlar**: Geo DSVM'nizde gpu üzerinde derin öğrenme yapmayı planlıyorsanız, NC Serisi GPU sanal makine boyutlarından birini seçin. Aksi takdirde, CPU tabanlı örneklerden birini seçebilirsiniz. VM'niz için bir depolama hesabı oluşturun. 
       
**Özet**: Girdiğiniz tüm bilgilerin doğru olduğunu doğrulayın.
    
**Satın Al**: Sağlama işlemini başlatmak için **Satın Al'ı**tıklatın. Hizmet koşullarına bir bağlantı sağlanır. VM' nin **Boyut** adımında seçtiğiniz sunucu boyutu için bilgi işlem ücretlerinin ötesinde ek ücretleri yoktur. 
 
 >[!NOTE]
 > Sağlama yaklaşık 20 ila 30 dakika sürer. Sağlamanın durumu Azure portalında görüntülenir.

 
## <a name="how-to-access-the-geo-ai-data-science-virtual-machine"></a>Geo AI Data Science Virtual Machine'e nasıl erişilir?

 VM'niz oluşturulduktan sonra, üzerine yüklenmiş ve önceden yapılandırılmış araçları kullanmaya başlayabilirsiniz. Birçok araç için Başlat menüsü kutucukları ve masaüstü simgeleri vardır. **Temel Bilgiler** bölümünde yapılandırdığınız Yönetici hesap kimlik bilgilerini kullanarak uzak masaüstünden VM'ye erişebilirsiniz.

 
## <a name="using-arcgis-pro-installed-in-the-vm"></a>VM yüklü ArcGIS Pro kullanma

Geo-DSVM'de ArcGIS Pro masaüstü önceden yüklenir ve ortam DSVM'deki tüm araçlarla çalışacak şekilde önceden yapılandırılmıştır. ArcGIS'i başlattığınızda, ArcGIS hesabınıza kimlik bilgileri istenir. Zaten bir ArcGIS hesabınız varsa ve yazılım için lisanslarınız varsa, varolan kimlik bilgilerinizi kullanabilirsiniz.  

![Arc-GIS-Logon](./media/provision-geo-ai-dsvm/ArcGISLogon.png)

Aksi takdirde, yeni bir ArcGIS hesabı ve lisansı için kaydolabilir veya ücretsiz bir [deneme](https://www.arcgis.com/features/free-trial.html)sürümü alabilirsiniz. 

![ArcGIS-Free-Deneme](./media/provision-geo-ai-dsvm/ArcGIS-Free-Trial.png)

Standart bir ArcGIS hesabına veya ücretsiz deneme sürümüne kaydolduktan sonra, [ArcGIS Pro ile başlarken](https://www.esri.com/library/brochures/getting-started-with-arcgis-pro.pdf)talimatları izleyerek ArcGIS Pro'yu hesabınız için yetkilendirebilirsiniz.

ArcGIS hesabınız üzerinden ArcGIS Pro masaüstünde oturum açtıktan sonra, coğrafi analizve makine öğrenimi projeleriniz için VM'de yüklenen ve yapılandırılan veri bilimi araçlarını kullanmaya başlamaya hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki kaynaktan kılavuzla Geo AI Data Science VM'yi kullanmaya başlayın:

* [Geo AI Veri Bilimi VM'yi kullanma](use-geo-ai-dsvm.md)
