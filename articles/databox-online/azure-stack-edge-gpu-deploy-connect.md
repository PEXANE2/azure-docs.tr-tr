---
title: Azure portal ' de GPU ile Azure Stack Edge cihazını bağlama, yapılandırma, etkinleştirme öğreticisi | Microsoft Docs
description: Azure Stack Edge dağıtımı öğreticisi, fiziksel cihazınızı bağlanmanızı, ayarlamanıza ve etkinleştirmenizi sağlar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 05/19/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Azure Stack Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: c98d6644dba0b3ca51f07f01ec27fee697369f2b
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89088261"
---
# <a name="tutorial-connect-to-azure-stack-edge-with-gpu"></a>Öğretici: GPU ile Azure Stack Edge 'e bağlanma

Bu öğreticide, yerel Web Kullanıcı arabirimini kullanarak, yerleşik bir GPU ile Azure Stack Edge cihazınıza nasıl bağlanabileceğinizi açıklanmaktadır.

Bağlantı işleminin tamamlanması 5 dakika sürebilir.

Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
>
> * Önkoşullar
> * Fiziksel bir cihaza bağlanma


## <a name="prerequisites"></a>Önkoşullar

Azure Stack Edge cihazınızı GPU ile yapılandırmadan ve ayarlamadan önce şunları yaptığınızdan emin olun:

* Fiziksel cihazı [yükleme Azure Stack Edge](azure-stack-edge-gpu-deploy-install.md)bölümünde ayrıntılı olarak yüklediniz.


## <a name="connect-to-the-local-web-ui-setup"></a>Yerel Web Kullanıcı arabirimi kurulumuna bağlanma

1. Bilgisayarınızda Ethernet bağdaştırıcısını, 192.168.100.5 ve alt ağ 255.255.255.0 statik IP adresiyle Azure Stack Edge cihazına bağlanacak şekilde yapılandırın.

2. Bilgisayarınızı cihazınızda bağlantı noktası 1 ' e bağlayın. Cihazınızda bağlantı noktası 1 ' i tanımlamak için aşağıdaki çizimi kullanın.

    ![Kabloları takılmış bir cihazın arka yüzü](./media/azure-stack-edge-gpu-deploy-install/ase-two-pci-slots.png)


3. Bir tarayıcı penceresi açın ve konumundaki cihazın yerel Web Kullanıcı arabirimine erişin `https://192.168.100.10` .  
    Bu eylem, cihazı etkinleştirdikten sonra birkaç dakika sürebilir. 

    Bir hata veya Web sitesinin güvenlik sertifikasıyla ilgili bir sorun olduğunu belirten bir uyarı görürsünüz. 
   
    ![Web sitesi güvenlik sertifikası hata iletisi](./media/azure-stack-edge-deploy-connect-setup-activate/image2.png)

4. **Bu Web sayfasına devam et**' i seçin.  
    Bu adımlar, kullanmakta olduğunuz tarayıcıya bağlı olarak farklılık gösterebilir.

5. Cihazınızın Web Kullanıcı arabiriminde oturum açın. Varsayılan parola *Parola1*' dir. 
   
    ![Azure Stack Edge cihaz oturum açma sayfası](./media/azure-stack-edge-deploy-connect-setup-activate/image3.png)

6. İstem sırasında cihaz yönetici parolasını değiştirin.  
    Yeni parola 8 ile 16 arasında karakter içermelidir. Şu karakterlerden üçünü içermelidir: büyük harf, küçük harf, sayısal ve özel karakterler.

Artık cihazınızın **genel bakış** sayfasında olursunuz. Bir sonraki adım, cihazınızın ağ ayarlarını yapılandırmaktır.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Fiziksel bir cihaza bağlanma


Azure Stack Edge cihazınızda ağ ayarlarını yapılandırma hakkında bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Ağı yapılandırma](./azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md)
