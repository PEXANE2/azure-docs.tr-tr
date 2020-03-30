---
title: PowerShell cmdlet kullanarak Azure IoT Hub'ı oluşturma | Microsoft Dokümanlar
description: Kaynak grubu oluşturmak ve kaynak grubunda bir IoT hub'ı oluşturmak için PowerShell cmdlets'i nasıl kullanacağınızı öğrenin. Ayrıca hub'ı nasıl kaldırılacağa da değinin.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: robinsh
ms.openlocfilehash: 9c49f7ac744ee516aefc1571d50264132035ba8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73890600"
---
# <a name="create-an-iot-hub-using-the-new-aziothub-cmdlet"></a>New-AzIotHub cmdlet'i kullanarak bir IoT hub'ı oluşturun

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Giriş

Azure IoT hub'ları oluşturmak ve yönetmek için Azure PowerShell cmdlets'i kullanabilirsiniz. Bu öğretici, PowerShell ile nasıl bir IoT hub'ı oluşturabileceğinizi gösterir.

Bu nasıl yapılacağını tamamlamak için bir Azure aboneliğine ihtiyacınız var. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="connect-to-your-azure-subscription"></a>Azure aboneliğinize bağlanma

Bulut Kabuğu'nu kullanıyorsanız, aboneliğinizde zaten oturum açmışsınızdır. Bunun yerine PowerShell'i yerel olarak çalıştırıyorsanız, Azure aboneliğinizde oturum açmak için aşağıdaki komutu girin:

```powershell
# Log into Azure account.
Login-AzAccount
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir IoT hub'ı dağıtmak için bir kaynak grubuna ihtiyacınız var. Varolan bir kaynak grubunu kullanabilir veya yeni bir kaynak oluşturabilirsiniz.

IoT hub'ınız için bir kaynak grubu oluşturmak için [Yeni Kaynak Grubu](https://docs.microsoft.com/powershell/module/az.Resources/New-azResourceGroup) komutunu kullanın. Bu örnek, **Doğu ABD** bölgesinde **MyIoTRG1** adlı bir kaynak grubu oluşturur:

```azurepowershell-interactive
New-AzResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki adımda oluşturduğunuz kaynak grubunda bir IoT hub'ı oluşturmak için [Yeni-AzIotHub](https://docs.microsoft.com/powershell/module/az.IotHub/New-azIotHub) komutunu kullanın. Bu örnek, **Doğu ABD** bölgesinde **MyTestIoTHub** adında bir **S1** hub'ı oluşturur:

```azurepowershell-interactive
New-AzIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub `
    -SkuName S1 -Units 1 `
    -Location "East US"
```

IoT hub'ının adı genel olarak benzersiz olmalıdır.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

Aboneliğinizdeki tüm IoT hub'larını [Get-AzIotHub](https://docs.microsoft.com/powershell/module/az.IotHub/Get-azIotHub) komutunu kullanarak listeleyebilirsiniz:

```azurepowershell-interactive
Get-AzIotHub
```

Bu örnek, önceki adımda oluşturduğunuz S1 Standart IoT Hub'ını gösterir.

[Kaldır-AzIotHub](https://docs.microsoft.com/powershell/module/az.iothub/remove-aziothub) komutunu kullanarak IoT hub'ını silebilirsiniz:

```azurepowershell-interactive
Remove-AzIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub
```

Alternatif olarak, Bir kaynak grubunu ve içerdiği tüm kaynakları [Kaldır-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.Resources/Remove-azResourceGroup) komutunu kullanarak kaldırabilirsiniz:

```azurepowershell-interactive
Remove-AzResourceGroup -Name MyIoTRG1
```

## <a name="next-steps"></a>Sonraki adımlar

Şimdi bir PowerShell cmdlet kullanarak bir IoT hub'ı dağıttınız, daha fazla keşfetmek istiyorsanız, aşağıdaki makalelere göz atın:

* [IoT göbeğinizle çalışmak için PowerShell cmdlets.](https://docs.microsoft.com/powershell/module/az.iothub/)

* [IoT Hub kaynak sağlayıcısı REST API.](https://docs.microsoft.com/rest/api/iothub/iothubresource)

IoT Hub için geliştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [C SDK'ya Giriş](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDK’ları](iot-hub-devguide-sdks.md)

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
