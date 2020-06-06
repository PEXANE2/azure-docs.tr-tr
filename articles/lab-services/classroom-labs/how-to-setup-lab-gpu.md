---
title: Azure Lab Services GPU 'Lar ile laboratuvar ayarlama | Microsoft Docs
description: GPU (grafik işleme birimi) sanal makinelerle laboratuvar ayarlamayı öğrenin.
services: lab-services
documentationcenter: na
author: nicolela
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/28/2020
ms.author: nicolela
ms.openlocfilehash: 9f4bc77366aaeaef9c66467e540219763228d317
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84450776"
---
# <a name="set-up-a-lab-with-gpu-graphics-processing-unit-virtual-machine-size"></a>GPU ile laboratuvar ayarlama (grafik işleme birimi) sanal makine boyutu
Bu makalede aşağıdaki görevlerin nasıl yapılacağı gösterilmektedir:

- Sanallaştırma ve işlem GPU 'ları arasında seçim yapın
- Uygun GPU sürücülerinin yüklü olduğundan emin olun
- GPU sanal makinesine (VM) bağlanmak için RDP ayarlarını yapılandırma

## <a name="choose-between-virtualization-and-compute-gpu-sizes"></a>Sanallaştırma ve işlem GPU boyutları arasında seçim yapın
Laboratuvar oluşturma Sihirbazı 'nın ilk sayfasında sınıfınız için gereken **sanal makinelerin boyutunu** seçersiniz.  

![VM boyutunu seçin](../media/how-to-setup-gpu/lab-gpu-selection.png)

Burada **görselleştirme** ve **işlem** GPU 'ları arasından seçim yapma seçeneğiniz vardır.  Öğrencilerinizin kullanacağı yazılıma göre uygun GPU türünü seçmeniz önemlidir.  

Aşağıdaki tabloda açıklandığı gibi, bilgi **işlem** GPU boyutu yoğun işlem gücü uygulamalarına yöneliktir.  Örneğin, [doğal dil işleme sınıfı türünde derin öğrenme](./class-type-deep-learning-natural-language-processing.md) , **küçük GPU (işlem)** boyutunu kullanmayı gösterir.  Öğrenciler, büyük veri kümeleriyle derin öğrenme modellerini eğmek üzere [veri bilimi sanal makinesi görüntüsü](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.ubuntu-1804) tarafından sunulan derin öğrenme çerçeveleri ve araçları kullandığından, bu tür bir sınıf için **işlem** GPU 'su uygundur.

| Boyut | Çekirdekler | RAM | Açıklama | 
| ---- | ----- | --- | ----------- | 
| Küçük GPU (Işlem) | <ul><li>6 çekirdek</li><li>56 GB RAM</li></ul>  | [Standard_NC6](https://docs.microsoft.com/azure/virtual-machines/nc-series) |Bu boyut, yapay zeka ve derin öğrenme gibi bilgisayar yoğunluklu uygulamalar için idealdir. |

**Görselleştirme** boyutları grafik yoğun uygulamalar için tasarlanmıştır.  Örneğin, [Solidçalışmamühendisliği sınıf türü](./class-type-solidworks.md) **küçük GPU (görselleştirme)** boyutunu kullanmayı gösterir.  Öğrenciler, düz nesnelerin modellenmesi ve görselleştirilmesi için, ' 3B bilgisayar destekli tasarım (CAD) ortamıyla etkileşimde bulunduğundan, **görselleştirme** GPU 'su bu tür bir sınıfa uygundur.

| Boyut | Çekirdekler | RAM | Açıklama | 
| ---- | ----- | --- | ----------- | 
| Küçük GPU (görselleştirme) | <ul><li>6 çekirdek</li><li>56 GB RAM</li>  | [Standard_NV6](https://docs.microsoft.com/azure/virtual-machines/nv-series) | Bu boyut, OpenGL ve DirectX gibi çerçeveleri kullanarak uzaktan görselleştirme, akış, oyun ve kodlama için idealdir. |
| Orta ölçekli GPU (görselleştirme) | <ul><li>12 çekirdek</li><li>112 GB RAM</li></ul>  | [Standard_NV12](https://docs.microsoft.com/azure/virtual-machines/nv-series?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json) | Bu boyut, OpenGL ve DirectX gibi çerçeveleri kullanarak uzaktan görselleştirme, akış, oyun ve kodlama için idealdir. |

## <a name="ensure-the-appropriate-gpu-drivers-are-installed"></a>Uygun GPU sürücülerinin yüklü olduğundan emin olun
Laboratuvar sanal makinelerinizin GPU yeteneklerini avantajlarından yararlanmak için uygun GPU sürücülerinin yüklü olduğundan emin olmanız gerekir.  Laboratuvar oluşturma Sihirbazı 'nda bir GPU VM boyutu seçtiğinizde, **GPU sürücülerini yüklemek**için seçenek vardır.  

![GPU sürücülerini yükler](../media/how-to-setup-gpu/lab-gpu-drivers.png)

Bu seçenek varsayılan olarak **etkindir** ve seçtiğiniz GPU ve görüntü türü için *en son* sürücülerin yüklü olmasını sağlar:
- **İşlem** GPU boyutunu seçtiğinizde, laboratuvar sanal makinelerinizin [NVIDIA Tesla K80](https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/tesla-product-literature/Tesla-K80-BoardSpec-07317-001-v05.pdf) GPU 'su tarafından gücü vardır.  Bu durumda, en son [CUDA](https://www.nvidia.com/object/io_69526.html) sürücüleri yüksek performanslı bilgi işlem sağlamak için yüklenir.
- **Görselleştirme** GPU boyutunu seçtiğinizde, laboratuvar sanal makineleriniz [NVIDIA Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU ve [kılavuz teknolojisi](https://www.nvidia.com/content/dam/en-zz/Solutions/design-visualization/solutions/resources/documents1/NVIDIA_GRID_vPC_Solution_Overview.pdf)tarafından desteklenir.  Bu durumda, grafik yoğun uygulamaları etkinleştirmek için en son KıLAVUZ sürücüleri yüklenir.

> [!IMPORTANT]
> **Görselleştirme** GPU 'ları ile en iyi kullanıcı deneyimine sahip olmak için, *her iki sürücünün de* yüklü olduğundan *ve* GPU 'nun RDP bağlantıları üzerinden etkinleştirildiğinden emin olmanız gerekir.  [Windows VM 'LERINE RDP bağlantısı üzerinden GPU 'Yu etkinleştir](how-to-setup-lab-gpu.md#enable-gpu-over-rdp-connection-to-windows-vms)adlı bölümdeki adımlara bakın.

### <a name="manually-install-drivers"></a>Sürücüleri el ile yükler
Sürücülerin farklı bir sürümünü en son sürümden yüklemeniz gereken senaryolar olabilir.  Bu bölümdeki adımlarda, bir **işlem** veya **görselleştirme** GPU kullanıyorsanız, uygun sürücülerin el ile nasıl yükleneceği gösterilmektedir.

#### <a name="compute-gpu-drivers"></a>İşlem GPU sürücüleri
**İşlem** GPU boyutu için sürücüleri el ile yüklemek için aşağıdaki adımları izleyin:
1. [Laboratuvarınızı oluştururken](./how-to-manage-classroom-labs.md)Laboratuvar oluşturma SIHIRBAZı 'nda **GPU sürücülerini yüklemeyi** **devre dışı bırakın** .
1. Laboratuvarınız oluşturulduktan sonra, uygun sürücüleri yüklemek için şablon VM 'ye bağlanın.
   ![NVıDıA sürücülerini Indirme](../media/how-to-setup-gpu/nvidia-driver-download.png) 
   1. Tarayıcınızda, [NVIDIA sürücü indirmeleri sayfasını](https://www.nvidia.com/Download/index.aspx)açın.
   2. **Ürün türünü** **Tesla**olarak ayarlayın.
   3. **Ürün serisini** **K serisi**olarak ayarlayın.
   4. **Ürünü** **Tesla K80**olarak ayarlayın.
   5. Laboratuvarınızı oluştururken seçtiğiniz temel görüntünün türüne göre **Işletim sistemini** ayarlayın.
   6. **CUDA araç setini** , ihtiyacınız olan CUDA sürücülerinin sürümüne ayarlayın.
   7. Sürücülerinizi bulmak için **Ara** ' yı tıklatın.
   8. Yükleyiciyi indirmek için **İndir** ' e tıklayın.
   9. Sürücüleri şablon VM 'de yüklü olacak şekilde yükleyiciyi çalıştırın.  
2. [Yüklü sürücüleri doğrula](how-to-setup-lab-gpu.md#validate-installed-drivers)adlı bölümdeki adımları izleyerek doğru yüklü sürücüleri doğrulayın. 
3. Sınıfınız için gereken sürücüleri ve diğer yazılımları yükledikten sonra, öğrencilerinizi sanal makinelerinizi oluşturmak için **Yayımla** ' ya tıklayabilirsiniz.

> [!NOTE]
> Linux görüntüsü kullanıyorsanız, yükleyiciyi yükledikten sonra sürücüleri yüklemek için aşağıdaki adımlara de bakın: [Linux üzerinde CUDA sürücülerini yükleme](https://docs.microsoft.com/azure/virtual-machines/linux/n-series-driver-setup?toc=/azure/virtual-machines/linux/toc.json#install-cuda-drivers-on-n-series-vms).

#### <a name="visualization-gpu-drivers"></a>Görselleştirme GPU sürücüleri
**Görselleştirme** GPU boyutları için sürücüleri el ile yüklemek için aşağıdaki adımları izleyin:
1. [Laboratuvarınızı oluştururken](./how-to-manage-classroom-labs.md)Laboratuvar oluşturma SIHIRBAZı 'nda **GPU sürücülerini yüklemeyi** **devre dışı bırakın** .
1. Laboratuvarınız oluşturulduktan sonra, uygun sürücüleri yüklemek için şablon VM 'ye bağlanın.
1. Aşağıdaki adımları izleyerek, şablon VM 'de Microsoft tarafından sunulan KıLAVUZ sürücülerini yükler:
   -  [Windows NVıDıA KıLAVUZ sürücüleri](https://docs.microsoft.com/azure/virtual-machines/windows/n-series-driver-setup#nvidia-grid-drivers)
   -  [Linux NVıDıA KıLAVUZ sürücüleri](https://docs.microsoft.com/azure/virtual-machines/linux/n-series-driver-setup?toc=/azure/virtual-machines/linux/toc.json#nvidia-grid-drivers)
  
1. Şablon VM 'yi yeniden başlatın.
1. [Yüklü sürücüleri doğrula](how-to-setup-lab-gpu.md#validate-installed-drivers)adlı bölümdeki adımları izleyerek doğru yüklü sürücüleri doğrulayın.
1. [Windows VM 'LERINE RDP bağlantısı üzerinden GPU 'Yu etkinleştir](how-to-setup-lab-gpu.md#enable-gpu-over-rdp-connection-to-windows-vms)adlı bölümdeki adımları izleyerek GPU 'yu ETKINLEŞTIRMEK için RDP ayarlarını yapılandırın.
1. Sınıfınız için gereken sürücüleri ve diğer yazılımları yükledikten sonra, öğrencilerinizi sanal makinelerinizi oluşturmak için **Yayımla** ' ya tıklayabilirsiniz.

### <a name="validate-installed-drivers"></a>Yüklü sürücüleri doğrulama
Bu bölümdeki adımlarda, GPU sürücülerinizin düzgün şekilde yüklendiği nasıl doğrulanacağı açıklanır.

#### <a name="windows-images"></a>Windows görüntüleri
1.  [Windows 'da sürücü yüklemesinin nasıl doğrulanaldığını](https://docs.microsoft.com/azure/virtual-machines/windows/n-series-driver-setup#verify-driver-installation)gösteren makaledeki adımları izleyin.
1.  Bir **görselleştirme** GPU kullanıyorsanız, şunları da yapabilirsiniz:
    - GPU ayarlarınızı görüntülemek ve ayarlamak için **NVIDIA Denetim masasına** erişin.  Bu ayarlara erişmek için **Denetim masası > donanımı** açın ve **NVIDIA Denetim masasını**seçin.
      ![NVıDıA denetim masasını aç](../media/how-to-setup-gpu/control-panel-nvidia-settings.png) 
     - **Görev Yöneticisi 'ni**kullanarak GPU performansınızı görüntüleyin.  Bunu görüntülemek için, **Görev Yöneticisi 'ni** açın ve **performans** sekmesini seçin. ![ Görev Yöneticisi GPU performansı](../media/how-to-setup-gpu/task-manager-gpu.png) 

      > [!IMPORTANT]
      > **NVIDIA Denetim Masası** ayarlarına yalnızca **görselleştirme** GPU 'ları için erişilebilir.  Bir **işlem** GPU 'Su Için **NVIDIA Denetim masasını** açmaya çalışırsanız şu hatayı görürsünüz: **NVIDIA ekran ayarları kullanılamıyor.  Şu anda bir NVıDıA GPU 'ya bağlı olan bir görüntü kullanmamaktadır.**  Benzer şekilde, **Görev Yöneticisi** 'Ndeki GPU performans bilgileri yalnızca **görselleştirme** GPU 'ları için sağlanır.

#### <a name="linux-images"></a>Linux görüntüleri
[Linux 'ta sürücü yüklemesinin nasıl doğrulanaldığını](https://docs.microsoft.com/azure/virtual-machines/linux/n-series-driver-setup#verify-driver-installation)gösteren makaledeki adımları izleyin.

## <a name="enable-gpu-over-rdp-connection-to-windows-vms"></a>Windows VM 'lerine RDP bağlantısı üzerinden GPU 'YU etkinleştirme
Bir **görselleştirme** GPU 'su tarafından desteklenen BIR Windows sanal makinesine bağlanmak için RDP (Uzak Masaüstü Protokolü) kullanırken, daha fazla yapılandırma yapmanız gerekir, böylece GPU grafik işleme için kullanılır (aksı takdirde CPU kullanılır).

Aşağıdaki adımların şablon VM 'de tamamlanması gerekir.

1. İlk olarak, GPU 'YU kullanmaya yönelik RDP ayarlarını yapılandırın:

   1. [GPU hızlandırmalı uygulama işlemesini nasıl yapılandıracağınızı](https://docs.microsoft.com/azure/virtual-desktop/configure-vm-gpu#configure-gpu-accelerated-app-rendering)gösteren bu makaledeki adımları izleyin.

   2. [GPU hızlandırmalı çerçeve kodlamasının nasıl yapılandırılacağını](https://docs.microsoft.com/azure/virtual-desktop/configure-vm-gpu#configure-gpu-accelerated-frame-encoding)gösteren bu makaledeki adımları izleyin.

2. Sonra, yapılandırmayı doğrulayın: 

   1. [GPU hızlandırmalı uygulama işlemesini nasıl doğrulayabildiğinizi](https://docs.microsoft.com/azure/virtual-desktop/configure-vm-gpu#verify-gpu-accelerated-app-rendering)gösteren bu makaledeki adımları izleyin.

   2.  [GPU hızlandırmalı çerçeve kodlamasının nasıl doğrulanalınacağını](https://docs.microsoft.com/azure/virtual-desktop/configure-vm-gpu#verify-gpu-accelerated-frame-encoding)gösteren bu makaledeki adımları izleyin.

3. Son olarak, artık sürücüleri yüklüdür ve GPU 'larınızı kullanmak için RDP ayarlarının yapılandırılmış olması gerekir.  Sınıfınız için gereken diğer yazılımları yükledikten sonra, öğrencilerinizin VM 'lerini oluşturmak için **Yayımla** ' ya tıklayabilirsiniz.  Öğrencileriniz, RDP kullanarak VM 'lerine bağlandığında, Masaüstü, sanal makinelerinin GPU 'SU kullanılarak işlenir.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Sınıf laboratuvarları oluşturma ve yönetme](how-to-manage-classroom-labs.md)
- [Solidçalışmalar bilgisayar destekli tasarım (CAD) sınıfı türü](class-type-solidworks.md)
- [MATLAB sınıf türü](class-type-matlab.md)



