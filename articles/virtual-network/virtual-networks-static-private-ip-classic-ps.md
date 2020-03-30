---
title: VM'ler için özel IP adreslerini yapılandırma (Klasik) - Azure PowerShell | Microsoft Dokümanlar
description: PowerShell'i kullanarak sanal makineler (Klasik) için özel IP adreslerini nasıl yapılandırıştırmayı öğrenin.
services: virtual-network
documentationcenter: na
author: genlin
manager: dcscontentpm
editor: tysonn
tags: azure-service-management
ms.assetid: 60c7b489-46ae-48af-a453-2b429a474afd
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 00a4f282da3a943516ffce584247ac20935526b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71058717"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-powershell"></a>PowerShell kullanarak sanal bir makine (Klasik) için özel IP adreslerini yapılandırın

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır. [Kaynak Yöneticisi dağıtım modelinde statik bir özel IP adresini](virtual-networks-static-private-ip-arm-ps.md)de yönetebilirsiniz.

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Aşağıdaki örnek PowerShell komutları zaten oluşturulmuş basit bir ortam bekliyor. Komutları bu belgede görüntülenen komutları çalıştırmak istiyorsanız, önce [VNet Oluştur'da](virtual-networks-create-vnet-classic-netcfg-ps.md)açıklanan test ortamını oluşturun.

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Belirli bir IP adresinin kullanılabilir olup olmadığını doğrulama
IP adresi *192.168.1.101* *TestVNet*adlı bir VNet kullanılabilir olup olmadığını doğrulamak için, aşağıdaki PowerShell komutunu çalıştırın ve *IsAvailable*için değeri doğrulamak:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 192.168.1.101 

Beklenen çıktı:

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>VM oluştururken statik özel IP adresi nasıl belirtilir?
Aşağıdaki PowerShell komut dosyası *TestService*adında yeni bir bulut hizmeti oluşturur, ardından Azure'dan bir görüntü alır, alınan görüntüyü kullanarak yeni bulut hizmetinde *DNS01* adında bir VM oluşturur, VM'yi *FrontEnd*adlı bir alt ağda ayarlar ve VM için statik özel IP adresi olarak *192.168.1.7* ayarlar:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage | where {$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name DNS01 -InstanceSize Small -ImageName $image.ImageName |
      Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! |
      Set-AzureSubnet –SubnetNames FrontEnd |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      New-AzureVM -ServiceName TestService –VNetName TestVNet

Beklenen çıktı:

    WARNING: No deployment found in service: 'TestService'.
    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    New-AzureService     fcf705f1-d902-011c-95c7-b690735e7412 Succeeded      
    New-AzureVM          3b99a86d-84f8-04e5-888e-b6fc3c73c4b9 Succeeded  

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Bir VM için statik özel IP adresi bilgileri nasıl alınır?
Yukarıdaki komut dosyasıyla oluşturulan VM'nin statik özel IP adresi bilgilerini görüntülemek için aşağıdaki PowerShell komutunu çalıştırın ve *IpAddress*değerlerini gözlemleyin:

    Get-AzureVM -Name DNS01 -ServiceName TestService

Beklenen çıktı:

    DeploymentName              : TestService
    Name                        : DNS01
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 192.168.1.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : DNS01
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Statik özel IP adresi VM'den nasıl kaldırılır?
Yukarıdaki komut dosyasında VM'ye eklenen statik özel IP adresini kaldırmak için aşağıdaki PowerShell komutunu çalıştırın:

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Remove-AzureStaticVNetIP |
      Update-AzureVM

Beklenen çıktı:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       052fa6f6-1483-0ede-a7bf-14f91f805483 Succeeded

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Varolan bir VM'ye statik özel IP adresi ekleme
Yukarıdaki komut dosyası kullanılarak oluşturulan VM'ye statik özel BIR IP adresi eklemek için aşağıdaki komutu çalıştırın:

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      Update-AzureVM

Beklenen çıktı:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       77d8cae2-87e6-0ead-9738-7c7dae9810cb Succeeded 

## <a name="set-ip-addresses-within-the-operating-system"></a>İşletim sistemi içindeki IP adreslerini ayarlama

Gerekli olmadıkça, Bir VM işletim sistemi içinde Azure sanal makinesine atanan özel IP'yi statik olarak atamamanız önerilir. İşletim sistemi içindeki özel IP adresini el ile ayarlarsanız, bunun Azure VM'ye atanan özel IP adresiyle aynı adres olduğundan emin olun veya sanal makineye bağlantınızı kaybedebilirsiniz. Sanal makinenin işletim sistemi içinde bir Azure sanal makinesine atanan genel IP adresini asla el ile atamamalısınız.

## <a name="next-steps"></a>Sonraki adımlar
* Ayrılmış [genel IP](virtual-networks-reserved-public-ip.md) adresleri hakkında bilgi edinin.
* Örnek [düzeyindeki genel IP (ILPIP)](virtual-networks-instance-level-public-ip.md) adresleri hakkında bilgi edinin.
* Ayrılmış [IP REST API'leri](https://msdn.microsoft.com/library/azure/dn722420.aspx)başvurun.

