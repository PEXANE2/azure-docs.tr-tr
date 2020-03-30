---
title: Windows PowerShell arabirimi üzerinden Microsoft Azure Veri Kutusu Edge cihazına bağlanma ve yönetme | Microsoft Dokümanlar
description: Windows PowerShell arabirimi üzerinden Data Box Edge'e nasıl bağlanıp yönetilmeye devam edilebildiğini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 06/25/2019
ms.author: alkohli
ms.openlocfilehash: f49396331a31f7ca9eaf453dc8bf6880da2e0da8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265486"
---
# <a name="manage-an-azure-data-box-edge-device-via-windows-powershell"></a>Windows PowerShell üzerinden Azure Veri Kutusu Kenarı aygıtını yönetme

Azure Veri Kutusu Kenarı çözümü, verileri işlemenizi ve ağ üzerinden Azure'a göndermenizi sağlar. Bu makalede, Veri Kutusu Edge aygıtınızın yapılandırma ve yönetim görevlerinden bazıları açıklanmaktadır. Cihazınızı yönetmek için Azure portalını, yerel web ui'sini veya Windows PowerShell arabirimini kullanabilirsiniz.

Bu makalede, PowerShell arabirimini kullanarak yaptığınız görevler üzerinde duruluyor.

Bu makalede aşağıdaki yordamlar içerir:

- PowerShell arabirimine bağlanın
- Destek paketi oluşturma
- Sertifikayı karşıya yükleme
- Cihazı sıfırlama
- Cihaz bilgilerini görüntüleme
- İşlem günlüklerini alın
- Bilgi işlem modüllerini izleme ve sorun giderme

## <a name="connect-to-the-powershell-interface"></a>PowerShell arabirimine bağlanın

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="create-a-support-package"></a>Destek paketi oluşturma

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

## <a name="upload-certificate"></a>Sertifikayı karşıya yükleme

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

Ayrıca, IoT Edge aygıtınızla ona bağlanabilen aşağı akım aygıtları arasında güvenli bir bağlantı sağlamak için IoT Edge sertifikaları da yükleyebilirsiniz. Yüklemeniz gereken üç IoT Edge sertifikası *(.pem* biçimi) vardır:

- Kök CA sertifikası veya sahibi CA
- Cihaz CA sertifikası
- Aygıt anahtarı sertifikası

Aşağıdaki örnek, IoT Edge sertifikalarını yüklemek için bu cmdlet kullanımını gösterir:

```
Set-HcsCertificate -Scope IotEdge -RootCACertificateFilePath "\\hcfs\root-ca-cert.pem" -DeviceCertificateFilePath "\\hcfs\device-ca-cert.pem\" -DeviceKeyFilePath "\\hcfs\device-key-cert.pem" -Credential "username"
```
Bu cmdlet çalıştırdığınızda, ağ payı için parolayı sağlamanız istenir.

Sertifikalar hakkında daha fazla bilgi için [Azure IoT Edge sertifikalarına](https://docs.microsoft.com/azure/iot-edge/iot-edge-certs) gidin veya [bir ağ geçidine sertifika yükleyin.](https://docs.microsoft.com/azure/iot-edge/how-to-create-transparent-gateway)

## <a name="view-device-information"></a>Cihaz bilgilerini görüntüleme
 
[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="reset-your-device"></a>Cihazınızı sıfırlama

[!INCLUDE [Reset your device](../../includes/data-box-edge-gateway-deactivate-device.md)]

## <a name="get-compute-logs"></a>İşlem günlüklerini alın

İşlem rolü cihazınızda yapılandırılırsa, PowerShell arabirimi üzerinden bilgi işlem günlüklerini de alabilirsiniz.

1. [PowerShell arabirimine bağlanın.](#connect-to-the-powershell-interface)
2. Cihazınızın `Get-AzureDataBoxEdgeComputeRoleLogs` işlem günlüklerini almak için kullanın.

    Aşağıdaki örnek, bu cmdlet kullanımını gösterir:

    ```powershell
    Get-AzureDataBoxEdgeComputeRoleLogs -Path "\\hcsfs\logs\myacct" -Credential "username" -FullLogCollection
    ```

    Cmdlet için kullanılan parametrelerin açıklaması aşağıda verilmiştir:
    - `Path`: İşlem günlüğü paketini oluşturmak istediğiniz paylaşıma bir ağ yolu sağlayın.
    - `Credential`: Ağ paylaşımı için kullanıcı adı sağlayın. Bu cmdlet çalıştırdığınızda, paylaşım parolası sağlamanız gerekir.
    - `FullLogCollection`: Bu parametre, günlük paketinin tüm işlem günlüklerini içermesini sağlar. Varsayılan olarak, günlük paketi yalnızca günlüklerin bir alt kümesini içerir.

## <a name="monitor-and-troubleshoot-compute-modules"></a>Bilgi işlem modüllerini izleme ve sorun giderme

[!INCLUDE [Monitor and troubleshoot compute modules](../../includes/data-box-edge-monitor-troubleshoot-compute.md)]

## <a name="exit-the-remote-session"></a>Uzak oturumdan çıkın

Uzak PowerShell oturumundan çıkmak için PowerShell penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure portalda [Azure Data Box Edge](data-box-edge-deploy-prep.md)’i dağıtın.
