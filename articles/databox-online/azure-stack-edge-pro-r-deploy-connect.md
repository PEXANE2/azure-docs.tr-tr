---
title: Azure portal 'de Azure Stack Edge Pro R cihazını bağlamak, yapılandırmak, etkinleştirmek için öğretici | Microsoft Docs
description: Yerel Web Kullanıcı arabirimini kullanarak Azure Stack Edge Pro R cihazınıza nasıl bağlanacağınızı öğrenin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/15/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Azure Stack Edge Pro R so I can use it to transfer data to Azure.
ms.openlocfilehash: ce97c22cf4bfbe5cca01183574597706a8c239e7
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96468731"
---
# <a name="tutorial-connect-to-azure-stack-edge-pro-r"></a>Öğretici: Azure Stack Edge Pro R 'ye bağlanma

Bu öğreticide, yerel Web Kullanıcı arabirimini kullanarak Azure Stack Edge Pro R cihazınıza nasıl bağlanabileceğinizi açıklanmaktadır.

Bağlantı işleminin tamamlanması 5 dakika sürebilir.

Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
>
> * Önkoşullar
> * Fiziksel bir cihaza bağlanma


## <a name="prerequisites"></a>Önkoşullar

Azure Stack Edge Pro R cihazınızı yapılandırmadan ve ayarlamadan önce şunları yaptığınızdan emin olun:

* Fiziksel cihazı [yükleme Azure Stack Edge Pro R](azure-stack-edge-pro-r-deploy-install.md)' de ayrıntılı olarak yüklediniz.


## <a name="connect-to-the-local-web-ui-setup"></a>Yerel Web Kullanıcı arabirimi kurulumuna bağlanma

1. Bilgisayarınızda Ethernet bağdaştırıcısını, 192.168.100.5 ve alt ağ 255.255.255.0 statik IP adresiyle Azure Stack Edge Pro R cihazına bağlanacak şekilde yapılandırın.

2. Bilgisayarınızı cihazınızda bağlantı noktası 1 ' e bağlayın. Bilgisayarı doğrudan cihaza (anahtar olmadan) bağlıyorsanız, bir çapraz kablo veya USB Ethernet bağdaştırıcısı kullanın. Cihazınızda bağlantı noktası 1 ' i tanımlamak için aşağıdaki çizimi kullanın.

    ![Kabloları takılmış bir cihazın arka yüzü](./media/azure-stack-edge-pro-r-deploy-install/backplane-cabled.png)


3. Tarayıcı penceresini açın ve `https://192.168.100.10` adresinden cihazın yerel web kullanıcı arabirimine erişin.  
    Siz cihazı açtıktan sonra bu eylem birkaç dakika sürebilir.

    Web sitesinin güvenlik sertifikasında sorun olduğunu belirten bir hata veya uyarı görürsünüz. 
   
    ![Web sitesi güvenlik sertifikası hata iletisi](media/azure-stack-edge-pro-r-deploy-connect/connect-web-ui-1.png)


4. **Bu Web sayfasına devam et**' i seçin.  
    Bu adımlar kullandığınız tarayıcıya bağlı olarak değişebilir.

5. Cihazınızın web kullanıcı arabiriminde oturum açın. Varsayılan parola *Parola1*' dir. 
   
    ![Azure Stack Edge cihaz oturum açma sayfası](media/azure-stack-edge-pro-r-deploy-connect/connect-web-ui-3.png)

6. İstem sırasında cihaz yönetici parolasını değiştirin.  
    Yeni parola 8 ile 16 arasında karakter içermelidir. Şu karakterlerden üçünü içermelidir: büyük harf, küçük harf, sayısal ve özel karakterler.

Artık cihazınızın **genel bakış** sayfasında olursunuz. Bir sonraki adım, cihazınızın ağ ayarlarını yapılandırmaktır.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Fiziksel bir cihaza bağlanma


Azure Stack Edge Pro R cihazınızda ağ ayarlarını yapılandırma hakkında bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Ağı yapılandırma](./azure-stack-edge-pro-r-deploy-configure-network-compute-web-proxy.md)
