---
title: Azure Veri Kutusu Ağ Geçidi aygıtına bağlanmak ve bunları yönetmek için Windows PowerShell'i kullanın
description: Windows PowerShell arabirimi üzerinden Data Box Ağ Geçidi'ne nasıl bağlanıp yönetilmeye devam edilebildiğini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 08/02/2019
ms.author: alkohli
ms.openlocfilehash: 6c9f3455a07001a8d1b9b8a1e84f2af3392b5690
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260221"
---
# <a name="manage-an-azure-data-box-gateway-device-via-windows-powershell"></a>Windows PowerShell üzerinden Azure Veri Kutusu Ağ Geçidi aygıtını yönetme

Azure Veri Kutusu Ağ Geçidi çözümü, ağ üzerinden Azure'a veri göndermenize olanak tanır. Bu makalede, Veri Kutusu Ağ Geçidi aygıtınızın yapılandırma ve yönetim görevlerinden bazıları açıklanmaktadır. Cihazınızı yönetmek için Azure portalını, yerel web ui'sini veya Windows PowerShell arabirimini kullanabilirsiniz.

Bu makalede, PowerShell arabirimini kullanarak yaptığınız görevler üzerinde duruluyor.

Bu makalede aşağıdaki yordamlar içerir:

- PowerShell arabirimine bağlanın
- Destek paketi oluşturma
- Sertifikayı karşıya yükleme
- DHCP olmayan ortamda önyükleme
- Cihaz bilgilerini görüntüleme

## <a name="connect-to-the-powershell-interface"></a>PowerShell arabirimine bağlanın

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="create-a-support-package"></a>Destek paketi oluşturma

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

## <a name="upload-certificate"></a>Sertifikayı karşıya yükleme

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

## <a name="boot-up-in-non-dhcp-environment"></a>DHCP olmayan ortamda önyükleme

[!INCLUDE [Boot up in non-DHCP environment](../../includes/data-box-edge-gateway-boot-non-dhcp.md)]

## <a name="view-device-information"></a>Cihaz bilgilerini görüntüleme

[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Azure portalında [Azure Data Box Gateway](data-box-gateway-deploy-prep.md)’i dağıtın.
