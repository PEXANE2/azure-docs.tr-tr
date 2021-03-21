---
title: Azure portal Azure Stack Edge Mini R 'ye bağlanma öğreticisi
description: Yerel Web Kullanıcı arabirimini kullanarak Azure Stack Edge Mini R cihazınıza nasıl bağlanacağınızı öğrenin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/20/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Azure Stack Edge Mini R so I can use it to transfer data to Azure.
ms.openlocfilehash: fe76391a5cfce8d7d39e47131db108ab87e5aed5
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96468935"
---
# <a name="tutorial-connect-to-azure-stack-edge-mini-r"></a>Öğretici: Azure Stack Edge Mini R 'ye bağlanma

Bu öğreticide, yerel Web Kullanıcı arabirimini kullanarak Azure Stack Edge Mini R cihazınıza nasıl bağlanabileceğinizi açıklanmaktadır.

Bağlantı işleminin tamamlanması 5 dakika kadar sürebilir.

Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
>
> * Önkoşullar
> * Fiziksel bir cihaza bağlanma



## <a name="prerequisites"></a>Önkoşullar

Azure Stack Edge cihazınızı yapılandırmadan ve ayarlamadan önce şunları yaptığınızdan emin olun:

* Fiziksel cihazı [yükleme Azure Stack Edge](azure-stack-edge-mini-r-deploy-install.md)bölümünde ayrıntılı olarak yüklediniz.


## <a name="connect-to-the-local-web-ui-setup"></a>Yerel Web Kullanıcı arabirimi kurulumuna bağlanma

1. Bilgisayarınızda Ethernet bağdaştırıcısını, 192.168.100.5 ve alt ağ 255.255.255.0 statik IP adresiyle Azure Stack Edge Pro cihazına bağlanacak şekilde yapılandırın.

2. Bilgisayarınızı cihazınızdaki PORT 1'e bağlayın. Bilgisayar cihaza doğrudan bağlanıyorsa (anahtar olmadan) çapraz bağlantılı kablo veya USB Ethernet bağdaştırıcısı kullanın. Cihazınızda PORT 1'i belirlemek için aşağıdaki çizimi kullanın.

    ![Wi-Fi için kablolama](./media/azure-stack-edge-mini-r-deploy-install/wireless-cabled.png)

[!INCLUDE [azure-stack-edge-gateway-delpoy-connect](../../includes/azure-stack-edge-gateway-deploy-connect.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Fiziksel bir cihaza bağlanma


Cihazınızda ağ ayarlarını yapılandırma hakkında bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Ağı yapılandırma](./azure-stack-edge-mini-r-deploy-configure-network-compute-web-proxy.md)
