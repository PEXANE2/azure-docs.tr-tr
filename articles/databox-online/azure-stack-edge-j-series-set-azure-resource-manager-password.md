---
title: Azure Stack Edge GPU cihazınızda Azure Resource Manager parola ayarlama
description: Azure PowerShell kullanarak Azure Stack Edge GPU 'unuzda çalışan Azure Resource Manager nasıl bağlanabileceğinizi açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: 6a59510b342f7ebd3969a4bb4fcfd75fffd04804
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89254160"
---
# <a name="set-azure-resource-manager-password-on-azure-stack-edge-gpu-device"></a>Azure Stack Edge GPU cihazında Azure Resource Manager parolayı ayarla

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

Bu makalede Azure Resource Manager parolanızın nasıl ayarlanacağı açıklanır. Azure Resource Manager aracılığıyla cihaz yerel API 'Lerine bağlanırken bu parolayı ayarlamanız gerekir.

Azure portal veya PowerShell cmdlet 'lerini kullanmanıza bağlı olarak parolayı ayarlama yordamı farklı olabilir. Bu yordamların her biri aşağıdaki bölümlerde açıklanmıştır.


## <a name="reset-password-via-the-azure-portal"></a>Azure portal aracılığıyla parolayı sıfırlama

1. Azure portal, cihazınızı yönetmek için oluşturduğunuz Azure Stack Edge kaynağına gidin. **Edge compute >** başlayın ' a gidin.

2. Sağ bölmede, komut çubuğundan **Edge ARM parolasını sıfırla**' yı seçin. 

    ![EdgeARM Kullanıcı parolasını sıfırla 1](media/azure-stack-edge-j-series-set-azure-resource-manager-password/set-edgearm-password-1.png)

3. **EdgeArm Kullanıcı parolasını sıfırla** dikey penceresinde, Azure Resource Manager aracılığıyla cihaz yerel API 'nize bağlanmak için bir parola girin. Parolayı onaylayın ve **Sıfırla**' yı seçin.

    ![EdgeARM Kullanıcı parolasını sıfırla 2](media/azure-stack-edge-j-series-set-azure-resource-manager-password/set-edgearm-password-2.png)



## <a name="reset-password-via-powershell"></a>PowerShell aracılığıyla parolayı sıfırlama

1. Azure portalında, cihazınızı yönetmek için oluşturduğunuz Azure Stack Edge kaynağına gidin. **Genel bakış** sayfasında aşağıdaki parametreleri bir yere göz atın.

    - Azure Stack Edge kaynak adı
    - Abonelik Kimliği

2. **Ayarlar > Özellikler**' e gidin. **Özellikler** sayfasında aşağıdaki parametreleri bir yere getirin.

    - Kaynak grubu
    - CıK şifreleme anahtarı: Görünüm ' ü seçin ve ardından **şifreleme anahtarını**kopyalayın.

    ![CıK şifreleme anahtarı al](media/azure-stack-edge-j-series-set-azure-resource-manager-password/get-cik-portal.png)
 
3. Azure Resource Manager bağlanmak için kullanacağınız bir parola belirler.

4. Cloud Shell 'i başlatın. Sağ üst köşedeki simgeyi seçin:

    ![Cloud Shell 'i Başlat](media/azure-stack-edge-j-series-set-azure-resource-manager-password/start-cloud-shell.png) 

    Cloud Shell başlatıldıktan sonra PowerShell 'e geçmeniz gerekebilir.

    ![Cloud shell](media/azure-stack-edge-j-series-set-azure-resource-manager-password/cloud-shell.png)   


5. Bağlam ayarla. Şunu yazın:

    `Set-AzContext -SubscriptionId <Subscription ID>`

    Örnek çıktı aşağıdaki gibidir:

    
    ```azurepowershell
    PS Azure:\> Set-AzContext -SubscriptionId 8eb87630-972c-4c36-a270-f330e6c063df
    
        Name        Account   SubscriptionName   Environment  TenantId
        ----       -------    ----------------   -----------  --------
        DataBox_Edge_Test (8eb87630-972c-4c36-a… MSI@50342 DataBox_Edge_Tes AzureCloud           72f988bf-86f1-41af-91ab-2d7…
    
        PS Azure:/
    ```
    
5.  Eski PS modülleriniz varsa, bunları yüklemeniz gerekir.

    `Remove-Module  Az.DataBoxEdge -force`

    Örnek bir çıktı aşağıda verilmiştir. Bu örnekte, yüklenecek eski modül yoktu.

    
    ```azurepowershell
        PS Azure:\> Remove-Module  Az.DataBoxEdge -force
        Remove-Module : No modules were removed. Verify that the specification of modules to remove is correct and those modules exist in the runspace.
        At line:1 char:1
        + Remove-Module  Az.DataBoxEdge -force
        + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ResourceUnavailable: (:) [Remove-Module], InvalidOperationException
        + FullyQualifiedErrorId : Modules_NoModulesRemoved,Microsoft.PowerShell.Commands.RemoveModuleCommand
    
        PS Azure:\
    ```

6. Sonraki komut kümesi, PowerShell modüllerini yüklemek için bir komut dosyası indirir ve çalıştırır.
    
    ```azurepowershell
        cd ~/clouddrive
        wget https://aka.ms/dbe-cmdlet-beta -O Az.DataBoxEdge.zip
        unzip ./Az.DataBoxEdge.zip
        Import-Module ~/clouddrive/Az.DataBoxEdge/Az.DataBoxEdge.psd1 -Force
    ```

7. Sonraki komut kümesinde kaynak adı, kaynak grubu adı, şifreleme anahtarı ve önceki adımda belirlediğiniz parolayı sağlamanız gerekir.

    ```azurepowershell
    $devicename = "<Azure Stack Edge resource name>"
    $resourceGroup = "<Resource group name>"
    $cik = "<Encryption key>"
    $password = "<Password>"
    ```
    Parola ve şifreleme anahtarı parametrelerinin güvenli dizeler olarak geçirilmesi gerekir. Parolayı ve şifreleme anahtarını güvenli dizeler için dönüştürmek üzere aşağıdaki cmdlet 'leri kullanın.

    ```azurepowershell
    $pass = ConvertTo-SecureString $password -AsPlainText -Force
    $key = ConvertTo-SecureString $cik -AsPlainText -Force
    ```
    Parolayı sıfırlamak için yukarıdaki oluşturulan güvenli dizeleri set-AzDataBoxEdgeUser cmdlet 'inde parametre olarak kullanın. Azure Stack Edge/Data Box Gateway kaynağını oluştururken kullandığınız kaynak grubunu kullanın.

    ```azurepowershell
    Set-AzDataBoxEdgeUser -ResourceGroupName $resourceGroup -DeviceName $devicename -Name EdgeARMUser  -Password $pass -EncryptionKey $key
    ```
    Örnek çıktı aşağıda verilmiştir.
    
    ```azurepowershell
    PS /home/aseuser/clouddrive> $devicename = "myaseresource"
    PS /home/aseuser/clouddrive> $resourceGroup = "myaserg"
    PS /home/aseuser/clouddrive> $cik = "54a7450fd7b3c506e10efea4e0c88a9390f37e299fbf43e01fb5dfe483ac036b6d0f85a6246e1926e297f98c0ff84c20a57348c689eff179ce31571fc787ac0a"
    PS /home/aseuser/clouddrive> $password = "Password2"
    PS /home/aseuser/clouddrive> $pass = ConvertTo-SecureString $password -AsPlainText -Force
    PS /home/aseuser/clouddrive> $key = ConvertTo-SecureString $cik -AsPlainText -Force
    PS /home/aseuser/clouddrive> Set-AzDataBoxEdgeUser -ResourceGroupName $resourceGroup -DeviceName $devicename -Name EdgeARMUser  -Password $pass -EncryptionKey $key
    
        User name   Type ResourceGroupName DeviceName
    ---------   ---- ----------------- ----------
        EdgeARMUser ARM  myaserg        myaseresource
    
        PS /home/aseuser/clouddrive>
    ```
Azure Resource Manager bağlanmak için yeni parolayı kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Resource Manager Bağlan](azure-stack-edge-j-series-connect-resource-manager.md)
