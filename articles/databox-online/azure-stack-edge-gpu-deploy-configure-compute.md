---
title: Azure Stack Edge Pro GPU üzerinde işlem ile verileri filtreleme, analiz etme öğreticisi | Microsoft Docs
description: Azure Stack Edge Pro GPU üzerinde işlem rolünü yapılandırmayı ve Azure 'a göndermeden önce verileri dönüştürmek için kullanmayı öğrenin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 08/28/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Azure Stack Edge Pro so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: 711da24b3edf08f4867109d0d70165955236c39a
ms.sourcegitcommit: a07a01afc9bffa0582519b57aa4967d27adcf91a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91743429"
---
# <a name="tutorial-configure-compute-on-azure-stack-edge-pro-gpu-device"></a>Öğretici: Azure Stack Edge Pro GPU cihazında işlem yapılandırma

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

Bu öğreticide, Azure Stack Edge Pro cihazınızda bir işlem rolünün nasıl yapılandırılacağı ve bir Kubernetes kümesinin nasıl oluşturulacağı açıklanmaktadır. 

Bu yordamın tamamlanması 20 ila 30 dakika kadar sürebilir.


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * İşlem yapılandırma
> * Kubernetes uç noktalarını al

 
## <a name="prerequisites"></a>Önkoşullar

Azure Stack Edge Pro cihazınızda bir işlem rolü ayarlamadan önce şunları yaptığınızdan emin olun:

- Azure Stack Edge Pro cihazınızı [etkinleştirme Azure Stack Edge Pro 'Yu etkinleştir](azure-stack-edge-gpu-deploy-activate.md)bölümünde açıklandığı gibi etkinleştirdiniz.
- [İşlem ağını etkinleştir](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md#enable-compute-network) bölümündeki yönergeleri izlediğinizden emin olun ve şunları yapın:
    - İşlem için bir ağ arabirimi etkinleştirildi.
    - Kubernetes düğüm IP 'Leri ve Kubernetes dış hizmet IP 'Leri atandı.

## <a name="configure-compute"></a>İşlem yapılandırma

Azure Stack Edge Pro ortamınızda işlem yapılandırmak için Azure portal aracılığıyla bir IoT Hub kaynağı oluşturacaksınız.

1. Azure Stack Edge kaynağınızın Azure portal **Genel Bakış ' a**gidin. Sağ bölmede, **işlem** kutucuğunda **başlayın**' ı seçin.

    ![İşlem ile çalışmaya başlama](./media/azure-stack-edge-j-series-deploy-configure-compute/configure-compute-1.png)

2. **Uç Işlem yapılandırma** kutucuğunda, **işlem Yapılandır**' ı seçin.

    ![İşlem yapılandırma](./media/azure-stack-edge-j-series-deploy-configure-compute/configure-compute-2.png)

3. **Uç Işlem yapılandırma** dikey penceresinde aşağıdakileri girin:

   
    |Alan  |Değer  |
    |---------|---------|
    |IoT Hub     | **Yeni** veya **mevcut**seçeneklerinden birini belirleyin. <br> Varsayılan olarak IoT kaynağı oluşturulurken Standart katmanı (S1) kullanılır. Bir ücretsiz katman IoT kaynağı kullanmak için kaynağı oluşturun ve sonra da mevcut kaynağı seçin. <br> Her durumda IoT Hub kaynak, Azure Stack Edge kaynağı tarafından kullanılan aynı abonelik ve kaynak grubunu kullanır.     |
    |Name     |IoT Hub kaynağınız için bir ad girin.         |

    ![İşlem 2 ile çalışmaya başlama](./media/azure-stack-edge-j-series-deploy-configure-compute/configure-compute-3.png)

4. **Oluştur**’u seçin. IoT Hub kaynak oluşturma birkaç dakika sürer. IoT Hub kaynağı oluşturulduktan sonra, işlem yapılandırmasını göstermek için işlem kutucuğunu **Yapılandır** ' ı güncelleştirir. 

    ![İşlem 3 ' ü kullanmaya başlama](./media/azure-stack-edge-j-series-deploy-configure-compute/configure-compute-4.png)

5. Edge işlem rolünün yapılandırıldığını doğrulamak için, **Işlem yapılandırma** kutucuğunda işlemi **görüntüle** ' yi seçin.
    
    ![İşlem 4 ' ü kullanmaya başlama](./media/azure-stack-edge-j-series-deploy-configure-compute/configure-compute-5.png)

    > [!NOTE]
    > IoT Hub Azure Stack Edge Pro cihazı ile ilişkilendirilmeden önce **Işlem Yapılandır** iletişim kutusu kapatılırsa IoT Hub oluşturulur ancak işlem yapılandırmasında gösterilmez. 
    
Edge cihazında Edge hesaplama rolü ayarlandığında, iki cihaz oluşturur: bir IoT cihazı ve bir IoT Edge cihaz. Her iki cihaz de IoT Hub kaynağında görüntülenebilir. Bu IoT Edge cihazında aynı zamanda bir IoT Edge çalışma zamanı çalışıyor. Bu noktada, IoT Edge cihazınız için yalnızca Linux platformu kullanılabilir.

Arka planda, sanal makinelerin ve Kubernetes kümesinin oluşturulmasından bu yana işlem yapılandırmak 20-30 dakika sürebilir. 

Azure portal içinde işlem başarıyla yapılandırıldıktan sonra, bir Kubernetes kümesi ve IoT ad alanıyla ilişkili bir varsayılan kullanıcı (Azure Stack Edge Pro tarafından denetlenen bir sistem ad alanı) mevcuttur. 

## <a name="get-kubernetes-endpoints"></a>Kubernetes uç noktalarını al

Bir istemciyi Kubernetes kümesine erişecek şekilde yapılandırmak için Kubernetes uç noktasına ihtiyacınız olacaktır. Azure Stack Edge Pro cihazınızın yerel kullanıcı arabiriminden Kubernetes API uç noktasını almak için aşağıdaki adımları izleyin.

1. Cihazınızın yerel Web Kullanıcı arabiriminde, **cihazlar** sayfasına gidin.
2. **Cihaz uç noktaları**altında **Kubernetes API hizmeti** uç noktasını kopyalayın. Bu uç nokta aşağıdaki biçimde bir dizedir: `https://compute.<device-name>.<DNS-domain>[Kubernetes-cluster-IP-address]` . 

    ![Yerel Kullanıcı arabirimindeki cihaz sayfası](./media/azure-stack-edge-j-series-create-kubernetes-cluster/device-kubernetes-endpoint-1.png)

3. Uç nokta dizesini kaydedin. Bu daha sonra, kubectl aracılığıyla Kubernetes kümesine erişmek üzere bir istemciyi yapılandırırken kullanacaksınız.

4. Yerel Web Kullanıcı arabirimdeyken şunları yapabilirsiniz:

    - Kubernetes API 'sine gidin, **Gelişmiş ayarlar** ' ı seçin ve Kubernetes için gelişmiş bir yapılandırma dosyası indirin. 

        ![Yerel Kullanıcı arabirimi 1 ' deki cihaz sayfası](./media/azure-stack-edge-gpu-deploy-configure-compute/download-advanced-config-1.png)

        Microsoft 'tan bir anahtar sağladıysanız (Select Users bu olabilir), bu yapılandırma dosyasını kullanabilirsiniz.

        ![Yerel Kullanıcı arabirimi 2 ' deki cihaz sayfası](./media/azure-stack-edge-gpu-deploy-configure-compute/download-advanced-config-2.png)

    - Ayrıca, **Kubernetes Pano** uç noktasına gidebilir ve bir `aseuser` yapılandırma dosyası indirebilirsiniz. 
    
        ![Yerel Kullanıcı arabirimi 3 ' te cihaz sayfası](./media/azure-stack-edge-gpu-deploy-configure-compute/download-aseuser-config-1.png)

        Bu yapılandırma dosyasını Kubernetes panosunda oturum açmak veya Kubernetes kümenizdeki sorunları ayıklamak için kullanabilirsiniz. Daha fazla bilgi için bkz. [Kubernetes panosuna erişme](azure-stack-edge-gpu-monitor-kubernetes-dashboard.md#access-dashboard). 


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * İşlem yapılandırma
> * Kubernetes uç noktalarını al


Azure Stack Edge Pro cihazınızı yönetme hakkında bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Azure Stack Edge Pro 'Yu yönetmek için yerel Web Kullanıcı arabirimini kullanma](azure-stack-edge-manage-access-power-connectivity-mode.md)
