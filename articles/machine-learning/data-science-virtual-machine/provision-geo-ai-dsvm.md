---
title: 'Hızlı başlangıç: coğrafi bir AI Veri Bilimi Sanal Makinesi oluşturma'
titleSuffix: Azure Data Science Virtual Machine
description: Jeo-uzamsal analiz ve makine öğrenimi için Azure 'da coğrafi bir AI Veri Bilimi Sanal Makinesi oluşturun ve oluşturun.
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: quickstart
ms.date: 09/13/2019
ms.openlocfilehash: f3ff9bd64f54d8f83fd1889078e8a4c01827d135
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77525898"
---
# <a name="quickstart-set-up-a-geo-artificial-intelligence-virtual-machine-on-azure"></a>Hızlı başlangıç: Azure 'da coğrafi yapay zeka sanal makinesi ayarlama 

Coğrafi AI Veri Bilimi Sanal Makinesi (coğrafi-DSVM), AI ve jeo-uzamsal analizlerini birleştirmek için özel olarak yapılandırılmış popüler [Azure veri bilimi sanal makinesi](https://aka.ms/dsvm) bir uzantısıdır. VM 'deki Jeo-uzamsal analizler, [argıs Pro](https://www.arcgis.com/features/index.html)ile desteklenir. Veri Bilimi Sanal Makinesi (DSVM), makine öğrenimi ve hatta derin öğrenme modelleri için hızlı eğitim sunar. Bu modelleri geliştirmek için, coğrafi bilgilerle zenginleştirilmiş verileri kullanır. Coğrafi-DSVM yalnızca Windows 2016 DSVM 'de desteklenir. 

Coğrafi-DSVM 'ye dahil edilen AI araçları şunları içerir:

- Microsoft Cognitive Toolkit, TensorFlow, keras, Caffe2 ve Chainer gibi popüler derin öğrenme çerçevelerinin GPU sürümleri
- Görüntü ve metin verileri alma ve ön işleme araçları
- Microsoft Machine Learning Server Geliştirici sürümü, Anaconda Python, Python ve R için JUPITER Not defterleri, Python ve R için Ides ve SQL veritabanları gibi geliştirme etkinliklerine yönelik araçlar
- AI uygulamalarınızdaki Jeo-uzamsal verilerle birlikte çalışan Python ve R arabirimlerinin yanı sıra ESRI 'nin Pro Desktop yazılımını argıs.
 

## <a name="create-your-geo-ai-data-science-vm"></a>Coğrafi AI Veri Bilimi VM'si oluşturma

Coğrafi AI Veri Bilimi VM'si bir örneğini oluşturmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://ms.portal.azure.com/#create/microsoft-ads.geodsvmwindows)sanal makine listesine gidin.
1. Sihirbaz oluşturmak için en altta **Oluştur** ' u seçin:

   ![-coğrafi-AI-dsvm oluşturma](./media/provision-geo-ai-dsvm/Create-Geo-AI.png)

1. Sihirbaz dört adımdan her biri için giriş gerektirir. Bu girdi hakkında ayrıntılı bilgi için aşağıdaki bölüme bakın.

### <a name="wizard-details"></a>Sihirbaz ayrıntıları ###

**Temel bilgiler**:

- **Ad**: oluşturmakta olduğunuz veri bilimi sunucusunun adı.
    
- **Kullanıcı adı**: yönetici hesabı oturum açma kimliği.
    
- **Parola**: yönetici hesabı parolası.
    
- **Abonelik**: birden fazla aboneliğiniz varsa, makinenin oluşturulup faturalandırılabileceği bir tane seçin.
    
- **Kaynak grubu**: yeni bir tane oluşturabilir veya aboneliğinizde **boş** bir mevcut Azure Kaynak grubu kullanabilirsiniz.
    
- **Konum**: en uygun veri merkezini seçin. Genellikle bu, verilerinizin büyük bir kısmının veya en hızlı ağ erişimi için fiziksel konumunuza en yakın olan biridir. Bir GPU üzerinde derin öğrenme çalıştırmayı planlıyorsanız, NC serisi GPU VM örnekleri içeren Azure 'daki konumlardan birini seçmeniz gerekir. Şu anda bu konumlar şunlardır: **Doğu ABD, Orta Kuzey ABD, Orta Güney ABD, Batı ABD 2, Kuzey Avrupa, Batı Avrupa**. En son liste için [bölgeye göre Azure ürünleri](https://azure.microsoft.com/regions/services/) sayfasına bakın ve **Işlem**altında **NC serisi** ' ni arayın. 
    
    
**Ayarlar**: coğrafi BIR bir GPU üzerinde derin öğrenme çalıştırmayı planlıyorsanız, bir NC serisi GPU sanal makine boyutundan birini seçin. Aksi takdirde, CPU tabanlı örneklerden birini seçebilirsiniz. VM 'niz için bir depolama hesabı oluşturun. 
       
**Özet**: girdiğiniz tüm bilgilerin doğru olduğundan emin olun.
    
**Satın al**: sağlama işlemini başlatmak Için **satın al**' a tıklayın. Hizmet koşullarına bir bağlantı sağlanır. VM 'de, **Boyut** adımında seçtiğiniz sunucu boyutu için işlem ücretlerinin ötesinde ek ücret yoktur. 
 
 >[!NOTE]
 > Sağlama, yaklaşık 20 ila 30 dakika sürer. Sağlama durumu Azure portal görüntülenir.

 
## <a name="how-to-access-the-geo-ai-data-science-virtual-machine"></a>Coğrafi AI Veri Bilimi Sanal Makinesi erişme

 VM 'niz oluşturulduktan sonra, üzerine yüklenmiş ve önceden yapılandırılmış araçları kullanmaya başlamaya hazırsınız demektir. Araçların birçoğu için başlangıç menüsü kutucukları ve Masaüstü simgeleri vardır. **Temel bilgiler** bölümünde yapılandırdığınız Yönetici hesabı kimlik BILGILERINI kullanarak sanal makineye uzak masaüstü ile erişebilirsiniz.

 
## <a name="using-arcgis-pro-installed-in-the-vm"></a>VM 'de yüklü olan argıs Pro kullanma

Coğrafi-DSVM 'de, argıs Pro Desktop önceden yüklenir ve ortam, DSVM 'deki tüm araçlarla çalışacak şekilde önceden yapılandırılmıştır. Argıs 'yi başlattığınızda, argıs hesabınızda kimlik bilgileri istenir. Zaten bir ArcGIS hesabınız varsa ve yazılım için lisanslarınız varsa, mevcut kimlik bilgilerinizi kullanabilirsiniz.  

![Yay-GıS-oturum açma](./media/provision-geo-ai-dsvm/ArcGISLogon.png)

Aksi takdirde, yeni bir ArcGIS hesabı ve lisansına kaydolabilir veya [ücretsiz bir deneme](https://www.arcgis.com/features/free-trial.html)edinebilirsiniz. 

![ArcGIS-ücretsiz-deneme](./media/provision-geo-ai-dsvm/ArcGIS-Free-Trial.png)

Standart bir ArcGIS hesabına veya ücretsiz bir denemeye kaydolduktan sonra, [argıs Pro 'yu kullanmaya](https://www.esri.com/library/brochures/getting-started-with-arcgis-pro.pdf)başlama yönergelerini izleyerek hesabınız için argıs Pro 'yu yetkilendirebilirsiniz.

Argıs hesabınız aracılığıyla argıs Pro Desktop 'ta oturum açtıktan sonra, Jeo-uzamsal analizlerinizin ve makine öğrenimi projelerinizin VM 'de yüklü ve yapılandırılmış olan veri bilimi araçlarını kullanmaya başlamaya hazırsınız demektir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki kaynaktaki kılavuzlarla coğrafi AI Veri Bilimi VM'si kullanmaya başlayın:

* [Coğrafi AI Veri Bilimi VM'si kullanın](use-geo-ai-dsvm.md)
