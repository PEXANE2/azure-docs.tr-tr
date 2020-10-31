---
title: Azure Market 'te VM hakkında sık sorulan sorular
description: Azure Marketi 'nde bir sanal makine oluşturulurken karşılaşılan ortak sorularla karşılaşıldı.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: guide
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 10/15/2020
ms.openlocfilehash: 88ec58ca9bcfa5c64036d3b65e77f3248e26357d
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93124960"
---
# <a name="common-questions-about-vm-in-azure-marketplace"></a>Azure Market 'te VM hakkında sık sorulan sorular

Bu sık sorulan sorular (SSS), Azure Marketi 'nde bir sanal makine (VM) teklifi oluştururken karşılaşabileceğiniz yaygın sorunları ele alır.

## <a name="how-do-i-configure-a-virtual-private-network-vpn-to-work-with-my-vms"></a>Sanal özel ağ (VPN), VM 'lerimi çalışacak şekilde mi Nasıl yaparım??

Azure Resource Manager dağıtım modelini kullanıyorsanız, üç seçeneğiniz vardır:

- [Azure portal kullanarak rota tabanlı bir VPN ağ geçidi oluşturma](../vpn-gateway/tutorial-create-gateway-portal.md)
- [Azure PowerShell kullanarak rota tabanlı bir VPN ağ geçidi oluşturma](../vpn-gateway/create-routebased-vpn-gateway-powershell.md)
- [CLı kullanarak rota temelli VPN ağ geçidi oluşturma](../vpn-gateway/create-routebased-vpn-gateway-cli.md)

## <a name="what-are-microsoft-support-policies-for-running-microsoft-server-software-on-azure-based-vms"></a>Azure tabanlı VM 'lerde Microsoft sunucu yazılımını çalıştırmaya yönelik Microsoft destek ilkeleri nelerdir?

[Microsoft Azure sanal makineler Için Microsoft sunucu yazılımı desteği](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)' nde ayrıntıları bulabilirsiniz.

## <a name="in-a-vm-how-do-i-manage-the-custom-script-extension-in-the-startup-task"></a>Bir VM 'de, başlangıç görevinde Özel Betik uzantısını nasıl yönetebilirim?

Windows sistemlerinde Azure PowerShell modülünü, Azure Resource Manager şablonları ve sorun giderme adımlarını kullanarak özel betik uzantısının kullanımıyla ilgili ayrıntılar için, bkz. [Windows Için özel Betik uzantısı](../virtual-machines/extensions/custom-script-windows.md).

## <a name="are-32-bit-applications-or-services-supported-in-azure-marketplace"></a>32 bitlik uygulamalar veya hizmetler Azure Marketi 'nde destekleniyor mu?

Hayır. Azure VM 'Leri için desteklenen işletim sistemleri ve standart hizmetler 64 bittir. 64 bitlik işletim sistemleri, geriye dönük uyumluluk için uygulamaların 32 bit sürümlerini desteklese de, sanal makine çözümünüzün bir parçası olarak 32-bit uygulamalar kullanılması desteklenmez ve kesinlikle önerilmez. Uygulamanızı 64 bitlik bir proje olarak yeniden oluşturun.

Daha fazla bilgi için şu makalelere bakın:

- [32 bitlik uygulamalar çalıştırma](/windows/desktop/WinProg64/running-32-bit-applications)
- [Azure sanal makinelerinde 32 bit işletim sistemleri desteği](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines)
- [Microsoft Azure sanal makineleri için Microsoft sunucu yazılımı desteği](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)

## <a name="error-vhd-is-already-registered-with-image-repository-as-the-resource"></a>Hata: VHD, kaynak olarak görüntü deposuna zaten kayıtlı

VHD 'lerimin içinden bir görüntü oluşturmaya her seferinde, bu Azure PowerShell "VHD, kaynak olarak görüntü deposuna zaten kaydedildi" hatasını alıyorum. Azure 'da bu ada sahip herhangi bir görüntü oluşturmadım veya bu adı taşıyan bir resim bulmadı. Bu sorunu nasıl çözebilirim?

Bu sorun genellikle kilidi olan bir VHD 'den bir VM oluşturduysanız görüntülenir. Bu VHD 'den ayrılmış bir VM olmadığından emin olun ve işlemi yeniden deneyin. Bu sorun devam ederse bir destek bileti açın. Bkz. [Iş ortağı merkezi desteği](support.md).

## <a name="next-steps"></a>Sonraki adımlar

- [VM sertifikasyonu sorunlarını giderme](azure-vm-create-certification-faq.md)