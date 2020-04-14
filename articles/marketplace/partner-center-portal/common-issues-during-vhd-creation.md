---
title: VHD oluşturma sırasında sık karşılaşılan sorunlar (SSS)
description: Sanal sabit disk (VHD) oluştururken sık sorulan sık sorulan sorular.
author: emuench
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: guide
ms.date: 04/09/2020
ms.openlocfilehash: a47bece4df7263dbc29ec19510c3ea422bb3f4cf
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81266272"
---
# <a name="common-issues-during-vhd-creation"></a>VHD oluşturma sırasında sık karşılaşılan sorunlar

> [!NOTE]
> Azure VM tekliflerinizin yönetimini Bulut İş Ortağı Portalı'ndan İş Ortağı Merkezi'ne taşıyoruz. Tekliflerin izne gelene kadar, lütfen tekliflerinizi yönetmek için Cloud İş Ortağı Portalı'ndaki [VHD oluşturma (SSS) sırasında sık karşılaşılan yönergeleri](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-common-vhd-creation-issues) izlemeye devam edin.

Sık sorulan bu sorular (SSS) Azure Sanal Makine teklifiniz için sanal bir sabit disk (VHD) oluştururken karşılaşabileceğiniz sık karşılaşılan sorunları kapsar.

## <a name="how-do-i-create-a-vm-from-the-azure-portal-using-a-vhd-in-premium-storage"></a>Premium depolamada VHD kullanarak Azure portalından nasıl bir VM oluşturabilirim?

Azure Marketi şu anda yönetilen depolama daki resimlerden veya Azure Premium Depolama'dan VM teklifleri oluşturmayı desteklemiyor. Ayrıntılar için Azure [Yönetilen Disklere Genel Bakış'a](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview)bakın.

## <a name="can-i-use-generation-2-vms-for-offers"></a>Teklifler için Generation 2 VM'leri kullanabilir miyim?

Hayır, yalnızca Generation 1 VHD'ler desteklenir. Ancak, şu anda Generation 2 VM'ler için desteği araştırmak için Microsoft Azure Platform Ekibi ile birlikte çalışıyoruz. Ayrıntılar için bkz. [Hyper-V'de bir nesil 1 veya 2 sanal makine oluşturmalı mıyım?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)

## <a name="how-do-i-change-the-name-of-the-host"></a>Ev sahibinin adını nasıl değiştiririm?

Bunu yapamazsınız. Bir VM oluşturulduktan sonra, kullanıcılar (sahipleri dahil) ana bilgisayar adını güncelleştiremez.

## <a name="how-do-i-reset-the-remote-desktop-service-or-its-sign-in-password"></a>Uzak Masaüstü hizmetini veya oturum açma parolasını nasıl sıfırlarım?

Bu makaleler, Windows ve Linux tabanlı VM'ler için RDS sıfırlamalarının nasıl gerçekleştirildirilebildiğini açıklar:

* [Windows VM’sinde Uzak Masaüstü hizmetini veya oturum açma parolasını sıfırlama](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
* [Linux VM parolası veya SSH anahtarı sıfırlama, SSH yapılandırmasını düzeltme ve VMAccess uzantısını kullanarak disk tutarlılığını denetleme](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)

## <a name="how-do-i-generate-new-ssh-certificates"></a>Yeni SSH sertifikalarını nasıl oluştururum?

Sertifikaların üretimi Azure [VM görüntü sertifikasında](https://aks.ms/CertifyVMimage)açıklanmıştır.

## <a name="how-do-i-configure-a-virtual-private-network-vpn-to-work-with-my-vms"></a>Sanal özel ağı (VPN) Sanal Cihazlarımla çalışacak şekilde nasıl yapılandırıyorum?

Azure Kaynak Yöneticisi dağıtım modelini kullanıyorsanız, üç seçeneğiniz vardır:

* [Azure portalını kullanarak rota tabanlı BIR VPN ağ geçidi oluşturma](https://docs.microsoft.com/azure/vpn-gateway/create-routebased-vpn-gateway-portal)
* [Azure PowerShell'i kullanarak rota tabanlı VPN ağ geçidi oluşturma](https://docs.microsoft.com/azure/vpn-gateway/create-routebased-vpn-gateway-powershell)
* [CLI kullanarak rota tabanlı VPN ağ geçidi oluşturma](https://docs.microsoft.com/azure/vpn-gateway/create-routebased-vpn-gateway-cli)

## <a name="what-are-microsoft-support-policies-for-running-microsoft-server-software-on-azure-based-vms"></a>Microsoft sunucu yazılımlarını Azure tabanlı VM'lerde çalıştırmak için Microsoft destek ilkeleri nelerdir?

[Microsoft Azure sanal makineleri için Microsoft sunucu yazılım desteğinde](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)ayrıntıları bulabilirsiniz.

## <a name="do-virtual-machines-have-unique-identifiers-associated-with-them"></a>Sanal makinelerin kendileriyle ilişkili benzersiz tanımlayıcıları var mı?

Evet, Azure'da barındırılıyorsa. Azure, oluşturulan her yeni VM kaynağına [Azure Sanal Makine Benzersiz Kimliği](https://blogs.msdn.microsoft.com/wasimbloch/2016/10/20/azure-virtual-machine-unique-id/)adı verilen benzersiz bir tanımlayıcı atar. Ayrıntılar için Azure Virtual Machine Benzersiz Kimliği'ne bakın. Ayrıca bu tanımlayıcıyı [Liste API'si](https://docs.microsoft.com/rest/api/compute/virtualmachines/list)aracılığıyla da alabilirsiniz.

## <a name="in-a-vm-how-do-i-manage-the-custom-script-extension-in-the-startup-task"></a>VM'de, başlangıç görevinde özel komut dosyası uzantısını nasıl yönetirim?

Azure PowerShell modülasyonundaki Özel Komut Dosyası Uzantısını, Azure Kaynak Yöneticisi şablonlarını ve Windows sistemlerindeki sorun giderme adımlarını kullanma hakkında ayrıntılı bilgi için [Windows için Özel Komut Dosyası Uzantısı'na](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)bakın.

## <a name="are-32-bit-applications-or-services-supported-in-azure-marketplace"></a>Azure Marketi'nde 32 bit uygulamalar veya hizmetler desteklendi mi?

Genellikle yapamazsınız. Azure VM'ler için desteklenen işletim sistemleri ve standart hizmetlerin tümü 64 bit'tir. Çoğu 64 bit işletim sistemi geriye dönük uyumluluk için uygulamaların 32 bit sürümlerini desteklese de, VM çözümünüzün bir parçası olarak 32 bit uygulamaları kullanmak desteklenmez ve son derece cesareti kırılır. Uygulamanızı 64 bit proje olarak yeniden oluşturun.

Daha fazla bilgi için şu makalelere bakın:

* [32 bit uygulamaları çalıştırma](https://docs.microsoft.com/windows/desktop/WinProg64/running-32-bit-applications)
* [Azure sanal makinelerinde 32 bit işletim sistemleri desteği](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines)
* [Microsoft Azure sanal makineleri için Microsoft sunucu yazılımı desteği](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)

## <a name="error-vhd-is-already-registered-with-image-repository-as-the-resource"></a>Hata: VHD zaten kaynak olarak görüntü deposu ile kayıtlı

VHD'lerimden bir görüntü oluşturmaya her çalıştığımda, Azure PowerShell'de "VHD zaten kaynak olarak görüntü deposuna kayıtlıdır" hatası alıyorum. Daha önce herhangi bir resim oluşturmadım ve Azure'da bu ada sahip herhangi bir resim buldum. Bu sorunu nasıl çözebilirim?

Bu sorun genellikle üzerinde kilit olan bir VHD'den bir VM oluşturduysanız görünür. Bu VHD'den ayrılan VM olmadığını doğrulayın ve işlemi yeniden deneyin. Bu sorun devam ederse, bir destek bileti açın. [İş Ortağı Merkezi desteğine](https://docs.microsoft.com/azure/marketplace/partner-center-portal/support)bakın.
