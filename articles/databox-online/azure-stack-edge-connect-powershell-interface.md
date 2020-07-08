---
title: Windows PowerShell arabirimi aracılığıyla Microsoft Azure Stack Edge cihazını bağlama ve yönetme | Microsoft Docs
description: Windows PowerShell arabirimi aracılığıyla Azure Stack Edge 'e bağlanmayı ve bunu yönetmeyi açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 06/25/2019
ms.author: alkohli
ms.openlocfilehash: 973c618b46d1b6be902d9629ca63ee120cae6855
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85313210"
---
# <a name="manage-an-azure-stack-edge-device-via-windows-powershell"></a>Windows PowerShell aracılığıyla Azure Stack Edge cihazını yönetme

Azure Stack Edge çözümü, verileri işlemenize ve ağ üzerinden Azure 'a göndermenize olanak tanır. Bu makalede Azure Stack Edge cihazınız için yapılandırma ve yönetim görevlerinin bazıları açıklanmaktadır. Cihazınızı yönetmek için Azure portal, yerel Web Kullanıcı arabirimini veya Windows PowerShell arabirimini kullanabilirsiniz.

Bu makale, PowerShell arabirimini kullanarak yaptığınız görevlere odaklanır. 

Bu makale aşağıdaki yordamları içerir:

- PowerShell arabirimine bağlanma
- Destek paketi oluşturma
- Sertifikayı karşıya yükleme
- Cihazı sıfırlama
- Cihaz bilgilerini görüntüle
- İşlem günlüklerini al
- İşlem modüllerini izleme ve sorun giderme

## <a name="connect-to-the-powershell-interface"></a>PowerShell arabirimine bağlanma

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="create-a-support-package"></a>Destek paketi oluşturma

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

## <a name="upload-certificate"></a>Sertifikayı karşıya yükleme

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

Ayrıca, IoT Edge cihazınız ile bağlanabilir olan aşağı akış cihazları arasında güvenli bir bağlantı sağlamak için IoT Edge sertifikaları karşıya yükleyebilirsiniz. Yüklemeniz gereken üç IoT Edge sertifikası (*. pek* biçimi) vardır:

- Kök CA sertifikası veya sahip CA
- Cihaz CA sertifikası
- Cihaz anahtarı sertifikası

Aşağıdaki örnek, IoT Edge sertifikaları yüklemek için bu cmdlet 'in kullanımını gösterir:

```
Set-HcsCertificate -Scope IotEdge -RootCACertificateFilePath "\\hcfs\root-ca-cert.pem" -DeviceCertificateFilePath "\\hcfs\device-ca-cert.pem\" -DeviceKeyFilePath "\\hcfs\device-key-cert.pem" -Credential "username"
```
Bu cmdlet 'i çalıştırdığınızda, ağ paylaşımının parolasını girmeniz istenir.

Sertifikalar hakkında daha fazla bilgi için [Azure IoT Edge sertifikalara](https://docs.microsoft.com/azure/iot-edge/iot-edge-certs) gidin veya [bir ağ geçidine sertifika yüklemeyi](https://docs.microsoft.com/azure/iot-edge/how-to-create-transparent-gateway)yapın.

## <a name="view-device-information"></a>Cihaz bilgilerini görüntüle
 
[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="reset-your-device"></a>Cihazınızı sıfırlama

[!INCLUDE [Reset your device](../../includes/data-box-edge-gateway-deactivate-device.md)]

## <a name="get-compute-logs"></a>İşlem günlüklerini al

İşlem rolü cihazınızda yapılandırılmışsa, işlem günlüklerini PowerShell arabirimi aracılığıyla da alabilirsiniz.

1. [PowerShell arabirimine bağlanın](#connect-to-the-powershell-interface).
2. `Get-AzureDataBoxEdgeComputeRoleLogs`Cihazınızın işlem günlüklerini almak için kullanın.

    Aşağıdaki örnek, bu cmdlet 'in kullanımını gösterir:

    ```powershell
    Get-AzureDataBoxEdgeComputeRoleLogs -Path "\\hcsfs\logs\myacct" -Credential "username" -FullLogCollection
    ```

    Cmdlet 'i için kullanılan parametrelerin açıklaması aşağıdadır:
    - `Path`: İşlem günlüğü paketini oluşturmak istediğiniz paylaşıma yönelik bir ağ yolu sağlayın.
    - `Credential`: Ağ paylaşımının Kullanıcı adını sağlayın. Bu cmdlet 'i çalıştırdığınızda, paylaşma parolasını sağlamanız gerekir.
    - `FullLogCollection`: Bu parametre, günlük paketinin tüm işlem günlüklerini içermesini sağlar. Varsayılan olarak, günlük paketi yalnızca bir Günlükler alt kümesi içerir.

## <a name="monitor-and-troubleshoot-compute-modules"></a>İşlem modüllerini izleme ve sorun giderme

[!INCLUDE [Monitor and troubleshoot compute modules](../../includes/azure-stack-edge-monitor-troubleshoot-compute.md)]

## <a name="exit-the-remote-session"></a>Uzak oturumdan çık

Uzak PowerShell oturumundan çıkmak için PowerShell penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure portal [Azure Stack Edge](azure-stack-edge-deploy-prep.md) dağıtın.
