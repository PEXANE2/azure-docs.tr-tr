---
title: VHD oluşturma sırasında yaygın sorunlar (SSS)
description: Bir sanal sabit disk (VHD) oluştururken yaygın sorunlar hakkında sık sorulan sorular.
author: emuench
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: guide
ms.date: 04/09/2020
ms.openlocfilehash: e96118e3c570faaaffb4cc7ef64e6d2d0e075e47
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85954185"
---
# <a name="common-issues-during-vhd-creation"></a>VHD oluşturma sırasında sık karşılaşılan sorunlar

Bu sık sorulan sorular (SSS), Azure sanal makine teklifiniz için bir sanal sabit disk (VHD) oluştururken karşılaşabileceğiniz yaygın sorunları ele alır.

## <a name="how-do-i-create-a-vm-from-the-azure-portal-using-a-vhd-in-premium-storage"></a>Premium depolamada bulunan bir VHD 'yi kullanarak Azure portal bir sanal makine oluşturmak Nasıl yaparım?.

Azure Marketi Şu anda yönetilen depolama veya Azure Premium Depolama 'daki görüntülerden sanal makine teklifleri oluşturmayı desteklemez. Ayrıntılar için bkz. [Azure yönetilen disklere genel bakış](../../virtual-machines/windows/managed-disks-overview.md).

## <a name="can-i-use-generation-2-vms-for-offers"></a>Teklifler için 2. nesil VM 'Leri kullanabilir miyim?

Hayır, yalnızca nesil 1 VHD 'ler desteklenir. Ancak, 2. nesil VM 'Lerin desteğini araştırmak için Microsoft Azure platform ekibiyle çalışıyoruz. Ayrıntılar için bkz. [Hyper-V ' d a 1. kuşak veya 2 sanal makine oluşturmalıyım?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)

## <a name="how-do-i-change-the-name-of-the-host"></a>Nasıl yaparım? konağın adı değiştirilsin mi?

Yapamazsınız. Bir VM oluşturulduktan sonra kullanıcılar (sahipler dahil) ana bilgisayar adını güncelleştiremez.

## <a name="how-do-i-reset-the-remote-desktop-service-or-its-sign-in-password"></a>Uzak Masaüstü hizmetinin veya oturum açma parolasının Nasıl yaparım? sıfırlansın mı?

Bu makalelerde, Windows ve Linux tabanlı VM 'Ler için RDS sıfırlamaları gerçekleştirme açıklanmaktadır:

* [Windows VM’sinde Uzak Masaüstü hizmetini veya oturum açma parolasını sıfırlama](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
* [Bir Linux VM parolasını veya SSH anahtarını sıfırlama, SSH yapılandırmasını çözme ve VMAccess uzantısını kullanarak disk tutarlılığını denetleme](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)

## <a name="how-do-i-generate-new-ssh-certificates"></a>Nasıl yaparım? yeni SSH sertifikaları mı oluşturulsun?

Sertifikaların oluşturulması, [Azure VM görüntüsü sertifikalarında](https://aks.ms/CertifyVMimage)açıklanır.

## <a name="how-do-i-configure-a-virtual-private-network-vpn-to-work-with-my-vms"></a>Sanal özel ağ (VPN), VM 'lerimi çalışacak şekilde mi Nasıl yaparım??

Azure Resource Manager dağıtım modelini kullanıyorsanız, üç seçeneğiniz vardır:

* [Azure portal kullanarak rota tabanlı bir VPN ağ geçidi oluşturma](../../vpn-gateway/create-routebased-vpn-gateway-portal.md)
* [Azure PowerShell kullanarak rota tabanlı bir VPN ağ geçidi oluşturma](../../vpn-gateway/create-routebased-vpn-gateway-powershell.md)
* [CLı kullanarak rota temelli VPN ağ geçidi oluşturma](../../vpn-gateway/create-routebased-vpn-gateway-cli.md)

## <a name="what-are-microsoft-support-policies-for-running-microsoft-server-software-on-azure-based-vms"></a>Azure tabanlı VM 'lerde Microsoft sunucu yazılımını çalıştırmaya yönelik Microsoft destek ilkeleri nelerdir?

[Microsoft Azure sanal makineler Için Microsoft sunucu yazılımı desteği](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)' nde ayrıntıları bulabilirsiniz.

## <a name="do-virtual-machines-have-unique-identifiers-associated-with-them"></a>Sanal makinelerin kendileriyle ilişkili benzersiz tanımlayıcıları var mı?

Evet, Azure üzerinde barındırılıyorsa. Azure, oluşturulan her yeni VM kaynağına [Azure sanal makinesi BENZERSIZ kimliği](https://blogs.msdn.microsoft.com/wasimbloch/2016/10/20/azure-virtual-machine-unique-id/)olarak adlandırılan benzersiz bir tanımlayıcı atar. Ayrıntılar için bkz. Azure sanal makinesi benzersiz KIMLIĞI. Bu tanımlayıcıyı [liste API 'si](https://docs.microsoft.com/rest/api/compute/virtualmachines/list)aracılığıyla da alabilirsiniz.

## <a name="in-a-vm-how-do-i-manage-the-custom-script-extension-in-the-startup-task"></a>Bir VM 'de, başlangıç görevinde Özel Betik uzantısını nasıl yönetebilirim?

Windows sistemlerinde Azure PowerShell modülünü, Azure Resource Manager şablonları ve sorun giderme adımlarını kullanarak özel betik uzantısının kullanımıyla ilgili ayrıntılar için, bkz. [Windows Için özel Betik uzantısı](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/).

## <a name="are-32-bit-applications-or-services-supported-in-azure-marketplace"></a>32 bitlik uygulamalar veya hizmetler Azure Marketi 'nde destekleniyor mu?

Genellikle yapamazsınız. Azure VM 'Leri için desteklenen işletim sistemleri ve standart hizmetler 64 bittir. 64 bitlik işletim sistemleri, geriye dönük uyumluluk için uygulamaların 32 bit sürümlerini desteklese de, sanal makine çözümünüzün bir parçası olarak 32-bit uygulamalar kullanılması desteklenmez ve kesinlikle önerilmez. Uygulamanızı 64 bitlik bir proje olarak yeniden oluşturun.

Daha fazla bilgi için şu makalelere bakın:

* [32 bitlik uygulamalar çalıştırma](https://docs.microsoft.com/windows/desktop/WinProg64/running-32-bit-applications)
* [Azure sanal makinelerinde 32 bit işletim sistemleri desteği](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines)
* [Microsoft Azure sanal makineleri için Microsoft sunucu yazılımı desteği](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)

## <a name="error-vhd-is-already-registered-with-image-repository-as-the-resource"></a>Hata: VHD, kaynak olarak görüntü deposuna zaten kayıtlı

VHD 'lerimin içinden bir görüntü oluşturmaya her seferinde, bu Azure PowerShell "VHD, kaynak olarak görüntü deposuna zaten kaydedildi" hatasını alıyorum. Azure 'da bu ada sahip herhangi bir görüntü oluşturmadım veya bu adı taşıyan bir resim bulmadı. Bu sorunu nasıl çözebilirim?

Bu sorun genellikle kilidi olan bir VHD 'den bir VM oluşturduysanız görüntülenir. Bu VHD 'den ayrılmış bir VM olmadığından emin olun ve işlemi yeniden deneyin. Bu sorun devam ederse bir destek bileti açın. Bkz. [Iş ortağı merkezi desteği](support.md).
