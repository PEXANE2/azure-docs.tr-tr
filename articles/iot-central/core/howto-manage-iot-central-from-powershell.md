---
title: Azure PowerShell'den IoT Central'ı yönetin | Microsoft Dokümanlar
description: Bu makalede, Azure PowerShell'deki IoT Central uygulamalarınızın nasıl oluşturulup yönetilen anlatılmaktadır.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 03/27/2020
ms.topic: how-to
manager: philmea
ms.openlocfilehash: 42d853c9cf53c1c6921fbd1816ec2298c9c3583e
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365531"
---
# <a name="manage-iot-central-from-azure-powershell"></a>Azure PowerShell’den IoT Central’ı yönetme

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

[Azure IoT Central uygulama yöneticisi](https://aka.ms/iotcentral) web sitesinde IoT Central uygulamalarını oluşturmak ve yönetmek yerine, uygulamalarınızı yönetmek için Azure [PowerShell'i](https://docs.microsoft.com/powershell/azure/overview) kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Azure PowerShell'i yerel makinenizde çalıştırmayı tercih ediyorsanız, [bkz.](https://docs.microsoft.com/powershell/azure/install-az-ps) Azure PowerShell'i yerel olarak çalıştırdığınızda, bu makaledeki cmdlets'i denemeden önce Azure'da oturum kurmak için **Connect-AzAccount** cmdlet'ini kullanın.

> [!TIP]
> PowerShell komutlarınızı farklı bir Azure aboneliğinde çalıştırmanız gerekiyorsa, [bkz.](/powershell/azure/manage-subscriptions-azureps?view=azps-3.4.0#change-the-active-subscription)

## <a name="install-the-iot-central-module"></a>IoT Merkezi modüllerini yükleyin

PowerShell ortamınızda yüklü [olan IoT Central modülünün](https://docs.microsoft.com/powershell/module/az.iotcentral/) yüklü olup olmadığını kontrol etmek için aşağıdaki komutu çalıştırın:

```powershell
Get-InstalledModule -name Az.I*
```

Yüklü modüllerin listesi **Az.IotCentral**içermiyorsa, aşağıdaki komutu çalıştırın:

```powershell
Install-Module Az.IotCentral
```

## <a name="create-an-application"></a>Uygulama oluşturma

Azure aboneliğinizde bir IoT Central uygulaması oluşturmak için [New-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/New-AzIotCentralApp) cmdlet'i kullanın. Örnek:

```powershell
# Create a resource group for the IoT Central application
New-AzResourceGroup -ResourceGroupName "MyIoTCentralResourceGroup" `
  -Location "East US"
```

```powershell
# Create an IoT Central application
New-AzIotCentralApp -ResourceGroupName "MyIoTCentralResourceGroup" `
  -Name "myiotcentralapp" -Subdomain "mysubdomain" `
  -Sku "ST1" -Template "iotc-pnp-preview@1.0.0" `
  -DisplayName "My Custom Display Name"
```

Komut dosyası ilk uygulama için doğu ABD bölgesinde bir kaynak grubu oluşturur. Aşağıdaki **tabloda New-AzIotCentralApp** komutu ile kullanılan parametreler açıklanmaktadır:

|Parametre         |Açıklama |
|------------------|------------|
|ResourceGroupName |Uygulamayı içeren kaynak grubu. Bu kaynak grubunun aboneliğinizde zaten bulunması gerekir. |
|Konum |Varsayılan olarak, bu cmdlet kaynak grubundan konumu kullanır. Şu anda, **Avustralya,** **Asya Pasifik,** **Avrupa,** **Amerika Birleşik Devletleri,** **İngiltere**ve **Japonya** coğrafyalarında bir IoT Merkezi uygulama oluşturabilirsiniz. |
|Adı              |Azure portalındaki uygulamanın adı. |
|Alt etki alanı         |Uygulamanın URL'sindeki alt etki alanı. Örnekte, uygulama URL'si. `https://mysubdomain.azureiotcentral.com` |
|Sku               |Şu anda, **ST1** veya **ST2**kullanabilirsiniz. Bkz. [Azure IoT Merkezi fiyatlandırması.](https://azure.microsoft.com/pricing/details/iot-central/) |
|Şablon          | Kullanılacak uygulama şablonu. Daha fazla bilgi için aşağıdaki tabloya bakın. |
|DisplayName       |Kullanıcı Arabirimi'nde görüntülenen uygulamanın adı. |

[!INCLUDE [iot-central-template-list](../../../includes/iot-central-template-list.md)]

## <a name="view-your-iot-central-applications"></a>IoT Central uygulamalarınızı görüntüleyin

IoT Central uygulamalarınızı listelemek ve meta verileri görüntülemek için [Get-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/Get-AzIotCentralApp) cmdlet'ini kullanın.

## <a name="modify-an-application"></a>Uygulamayı değiştirme

Bir IoT Central uygulamasının meta verilerini güncellemek için [Set-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/set-aziotcentralapp) cmdlet'ini kullanın. Örneğin, uygulamanızın görüntü adını değiştirmek için:

```powershell
Set-AzIotCentralApp -Name "myiotcentralapp" `
  -ResourceGroupName "MyIoTCentralResourceGroup" `
  -DisplayName "My new display name"
```

## <a name="remove-an-application"></a>Uygulamayı kaldırma

Bir IoT Central uygulamasını silmek için [Remove-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/Remove-AzIotCentralApp) cmdlet'ini kullanın. Örnek:

```powershell
Remove-AzIotCentralApp -ResourceGroupName "MyIoTCentralResourceGroup" `
 -Name "myiotcentralapp"
```

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell'den Azure IoT Merkezi uygulamalarını nasıl yönetdiğinizi öğrendiğiniz egöre, önerilen bir sonraki adım şudur:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetme](howto-administer.md)
