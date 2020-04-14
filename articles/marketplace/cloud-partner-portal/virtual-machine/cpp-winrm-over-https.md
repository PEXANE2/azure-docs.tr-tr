---
title: Azure için HTTPS üzerinden Windows Uzaktan Yönetimi | Azure Marketi
description: PowerShell ile uzaktan yönetilebilmek için Azure barındırılan Windows tabanlı bir VM'in nasıl yapılandırılabildiğini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: dsindona
ms.openlocfilehash: b2a4bb107309894a7180e0a4585cdba6f04d1bee
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273044"
---
# <a name="windows-remote-management-over-https"></a>HTTPS üzerinden Windows Uzak Yönetimi

> [!IMPORTANT]
> 13 Nisan 2020'den itibaren Azure Sanal Makine tekliflerinizin yönetimini İş Ortağı Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure Sanal Makine Oluştur teklifindeki](https://aka.ms/CreateAzureVMoffer) yönergeleri izleyin.

Bu bölümde, PowerShell ile uzaktan yönetilebilmek ve dağıtılabilmek için Azure barındırılan, Windows tabanlı bir VM'nin nasıl yapılandırılabildiği açıklanmaktadır.  PowerShell remotingini etkinleştirmek için hedef VM'nin bir Windows Uzaktan Yönetimi (WinRM) HTTPS bitiş noktasını ortaya çıkarması gerekir.  PowerShell remoting hakkında daha fazla bilgi için, [Uzaktan Kumanda komutları çalıştırın](https://docs.microsoft.com/powershell/scripting/learn/remoting/running-remote-commands)bakın.  WinRM hakkında daha fazla bilgi için [Windows Remote Management'a](https://docs.microsoft.com/windows/desktop/WinRM/portal)bakın.

Azure Hizmet Yöneticisi Portalı veya amortismana naltı lanse edilmiş [Azure Hizmet Yönetimi API'si](https://docs.microsoft.com/previous-versions/azure/ee460799(v=azure.100))gibi "klasik" Azure yaklaşımlarından birini kullanarak bir VM oluşturduysanız, otomatik olarak bir WinRM bitiş noktasıyla yapılandırılır.  Ancak, aşağıdaki "modern" Azure yaklaşımlarından herhangi birini kullanarak bir VM *not* oluşturursanız, VM'niz HTTPS üzerinden WinRM için yapılandırılmaz.

- Azure [portalının](https://portal.azure.com/)(genellikle onaylanmış bir tabandan) kullanılması, azure [uyumlu bir VHD oluşturma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-create-vhd) bölümünde açıklandığı gibi
- [Azure Kaynak Yöneticisi şablonlarını kullanma](https://docs.microsoft.com/azure/virtual-machines/windows/ps-template)
- Azure PowerShell veya Azure CLI komut kabuğunu kullanma.  Örneğin, [Bkz. Quickstart: PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell) ve Quickstart ile Azure'da bir Windows sanal makinesi [oluşturun: Azure CLI ile bir Linux sanal makinesi oluşturun.](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-cli)

Bu WinRM bitiş noktası, [VM görüntünüzde](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-certify-vm)açıklandığı gibi VM'ye binmek için Sertifika aracı kitini çalıştırmak için de gereklidir.

Buna karşılık, genellikle Linux VM'leri bir SSH konsolundaki [Azure CLI](https://docs.microsoft.com/cli/azure) veya Linux komutları kullanılarak uzaktan yönetilir.  Azure ayrıca [Linux VM'nizde komut dosyalarını çalıştırmak](https://docs.microsoft.com/azure/virtual-machines/linux/run-scripts-in-vm)için çeşitli alternatif yöntemler de sağlar.  Daha karmaşık senaryolar için, Windows veya Linux tabanlı VM'ler için kullanılabilir bir dizi otomasyon ve tümleştirme çözümü vardır.


## <a name="configure-and-deploy-with-winrm"></a>WinRM ile yapılandırma ve dağıtma

Windows tabanlı bir VM için WinRM bitiş noktası, geliştirmenin iki farklı aşamasında yapılandırılabilir:

- Oluşturma sırasında - varolan bir VHD'ye VM dağıtımı sırasında.  Bu yeni teklifler için tercih edilen bir yaklaşımdır.  Bu yaklaşım, sağlanan Azure Kaynak Yöneticisi şablonlarını kullanarak ve özelleştirilmiş PowerShell komut dosyaları çalıştırarak bir Azure sertifikası oluşturulmasını gerektirir.
- Dağıtımdan sonra , Azure'da barındırılan mevcut bir VM'de.  Azure'da zaten dağıtılan bir VM çözümünüz varsa ve bunun için Pencere Uzaktan Yönetimi'ni etkinleştirmeniz gerekiyorsa bu yaklaşımı kullanın.  Bu yaklaşım, Azure portalında el ile değişiklikler ve hedef VM'de bir komut dosyasının yürütülmesini gerektirir.


## <a name="next-steps"></a>Sonraki adımlar
Yeni bir VM oluşturuyorsanız, [VM'nizin VHD'lerinden dağıtımı](./cpp-deploy-vm-vhd.md)sırasında WinRM'i etkinleştirebilirsiniz.  Aksi takdirde, WinRM varolan bir VM'de etkinleştirilebilir
