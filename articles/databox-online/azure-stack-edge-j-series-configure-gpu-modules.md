---
title: Microsoft Azure Stack Edge GPU cihazında GPU modülünü çalıştırma | Microsoft Docs
description: Azure portal aracılığıyla bir Azure Stack Edge cihazında GPU üzerinde bir modülün nasıl yapılandırılacağını ve çalıştırılacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/25/2020
ms.author: alkohli
ms.openlocfilehash: 419b8beb866711e80b4366df4398eb248256021b
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89266961"
---
# <a name="configure-and-run-a-module-on-gpu-on-azure-stack-edge-device"></a>Azure Stack Edge cihazında GPU üzerinde bir modül yapılandırma ve çalıştırma

Azure Stack Edge cihazınız bir veya daha fazla grafik Işleme birimi (GPU) içerir. GPU 'Lar, paralel işleme özellikleri sundukları ve merkezi Işleme birimlerinden (CPU) görüntü işlemeye daha hızlı bir şekilde, AI hesaplamaları için popüler bir seçimdir. Azure Stack Edge cihazında bulunan GPU hakkında daha fazla bilgi için [Azure Stack Edge cihazı teknik özellikleri](azure-stack-edge-gpu-technical-specifications-compliance.md)' ne gidin.

Bu makalede, Azure Stack Edge cihazınızda GPU üzerinde bir modülün nasıl yapılandırılacağı ve çalıştırılacağı açıklanır. Bu makalede, NVIDIA T4 GPU 'ları için yazılmış genel olarak kullanılabilir kapsayıcı modülü **basamaklarını** kullanacaksınız. Bu yordam, bu GPU 'Lar için NVIDIA tarafından yayımlanan diğer modülleri yapılandırmak üzere kullanılabilir.


## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakilerden emin olun:

1. GPU etkin 1 düğümlü Azure Stack Edge cihazına erişirsiniz. Bu cihaz Azure 'da bir kaynakla etkinleştirildi.  

## <a name="configure-module-to-use-gpu"></a>Modülü GPU 'YU kullanacak şekilde yapılandırma

Modül çalıştırmak için Azure Stack Edge cihazınızda GPU 'YU kullanmak üzere bir modül yapılandırmak için aşağıdaki adımları izleyin.

1. Azure portal, aygıtınızla ilişkili kaynağa gidin. 

2. **Edge compute >** başlayın ' a gidin. **Uç Işlem yapılandırma** kutucuğunda Yapılandır ' ı seçin.

    ![Modülü GPU 1 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-compute-1.png)

3. **Uç Işlem yapılandırma** dikey penceresinde:

    1. **IoT Hub**Için **Yeni oluştur**' u seçin.
    2. Cihazınız için oluşturmak istediğiniz IoT Hub kaynağı için bir ad sağlayın. Ücretsiz katmanı kullanmak IÇIN var olan bir kaynağı seçin. 
    3. IoT Hub kaynakla oluşturulan IoT Edge cihazını ve IoT Ağ Geçidi cihazını bir yere unutmayın. Bu bilgileri sonraki adımlarda kullanacaksınız.

    ![Modülü GPU 2 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-compute-2.png)

4. IoT Hub kaynağını oluşturmak birkaç dakika sürer. Kaynak oluşturulduktan sonra, IoT Hub kaynağın ayrıntılarını görüntülemek için **uç Işlem yapılandırma** kutucuğunda **yapılandırma görüntüle** ' yi seçin.

    ![Modülü GPU 4 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-compute-4.png)

5. **Otomatik cihaz yönetimi > IoT Edge**gidin.

    ![Modülü GPU 6 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-2.png)

    Sağ bölmede, Azure Stack Edge cihazlarınızın ilişkilendirildiği IoT Edge cihazını görürsünüz. Bu, IoT Hub kaynağı oluştururken önceki adımda oluşturduğunuz IoT Edge cihazına karşılık gelir. 
    
6. Bu cihaz IoT Edge seçin.

   ![Modülü GPU 7 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-3.png)

7.  **Modül ayarla**' yı seçin.

    ![Modülü GPU 8 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-4.png)

8. **+ Ekle** ' yi seçin ve **+ IoT Edge modülünü**seçin. 

    ![Modülü GPU 9 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-5.png)

9. **IoT Edge modülü Ekle** sekmesinde:

    1. **Görüntü URI**'sini sağlayın. Genel olarak kullanılabilen NVIDIA modül **rakamlarını** burada kullanacaksınız. 
    
    2. **Yeniden başlatma ilkesini** **her zaman**olarak ayarlayın.
    
    3. **İstenen durumu** **çalışıyor**olarak ayarlayın.
    
    ![Modülü GPU 10 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-6.png)

10. **Ortam değişkenleri** sekmesinde, değişkenin adını ve karşılık gelen değeri belirtin. 

    1. Geçerli modülün bu cihazda tek bir GPU kullanmasını sağlamak için NVIDIA_VISIBLE_DEVICES kullanın. 

    2. Değeri 0 veya 1 olarak ayarlayın. Bu, cihaz tarafından bu modül için en az bir GPU kullanılmasını sağlar. Bu değeri 0, 1 olarak ayarlarsanız, cihazınızdaki GPU 'Ların bu modül tarafından kullanılmakta olduğu anlamına gelir.

        ![Modülü GPU 11 ' i kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-7.png)

        NVIDIA GPU ile kullanabileceğiniz ortam değişkenleri hakkında daha fazla bilgi için, [NVIDIA kapsayıcı çalışma zamanına](https://github.com/NVIDIA/nvidia-container-runtime#environment-variables-oci-spec)gidin.

    > [!NOTE]
    > GPU yalnızca bir modüle eşleştirilebilir. Modül, her ikisini de veya hiçbir GPU 'yu kullanabilir. 

11. Modülünüzün adını girin. Bu noktada kapsayıcı oluşturma seçeneği sağlayıp modül ikizi ayarlarını değiştirebilir veya işiniz bittiğinde **Ekle**' yi seçin. 

    ![Modülü GPU 12 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-8.png)

12. Modülün çalıştığından emin olun ve **gözden geçir + oluştur**' u seçin.    

    ![Modülü GPU 13 ' ü kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-9.png)

13. **Gözden geçir + oluştur** sekmelerinde, seçtiğiniz dağıtım seçenekleri görüntülenir. Seçenekleri gözden geçirin ve **Oluştur**' u seçin.
    
    ![Modülü GPU 14 ' ü kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-10.png)

14. Modülün **çalışma zamanı durumunu** bir yere getirin. 
    
    ![Modülü GPU 15 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-11.png)

    Modülün dağıtılması birkaç dakika sürer. **Yenile** ' yi seçin ve **çalışma zamanı durumu** güncelleştirmesini **çalışıyor**olarak görmeniz gerekir.

    ![Modülü GPU 16 kullanacak şekilde yapılandırma](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-12.png)


## <a name="next-steps"></a>Sonraki adımlar

- [NVıDıA GPU ile kullanabileceğiniz ortam değişkenleri](https://github.com/NVIDIA/nvidia-container-runtime#environment-variables-oci-spec)hakkında daha fazla bilgi edinin.
