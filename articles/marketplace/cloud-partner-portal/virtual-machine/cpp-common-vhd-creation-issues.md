---
title: Azure Marketi için VHD oluşturma (SSS) sırasında sık karşılaşılan sorunlar
description: VHD oluşturma ve ilişkili sorunlar hakkında sık sorulan sorular.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: dsindona
ms.reviewer: v-divte; v-miclar
ms.openlocfilehash: 192fea4101fb972ec66bb5f21a2a83f9903f0855
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278118"
---
# <a name="common-issues-during-vhd-creation-faq"></a>VHD oluşturma sırasında sık karşılaşılan sorunlar (SSS)

Aşağıdaki sık sorulan sorular (SSS) sanal sabit disk (VHD) ve Sanal makine (VM) VM teklifleri için oluşturulması sırasında karşılaşılan sık karşılaşılan sorunları kapsamaktadır. 

## <a name="how-do-you-create-a-vm-from-the-azure-portal-using-the-vhd-that-is-uploaded-to-premium-storage"></a>Premium depolama alanına yüklenen VHD'yi kullanarak Azure portalından nasıl bir VM oluşturursunuz?

Azure Marketi şu anda yönetilen depolama da veya Azure Premium Depolama'da bulunan resimlerden VM teklifleri oluşturmayı desteklemiyor.  Bu depolama seçenekleri hakkında daha fazla bilgi için [Azure Yönetilen Disklere Genel Bakış'a](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview)bakın.


## <a name="can-you-use-generation-2-vms-for-offers"></a>Teklifler için generation 2 VM'leri kullanabilir misiniz?

Hayır, yalnızca nesil 1 VHD'ler desteklenir.  Ancak, şu anda nesil 2 VM'ler için desteği araştırmak için Microsoft Azure Platform Ekibi ile birlikte çalışıyoruz.  Farklar hakkında daha fazla bilgi için bkz. [Hyper-V'de bir nesil 1 veya 2 sanal makine oluşturmalı mıyım?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)


## <a name="how-do-you-change-the-name-of-the-host"></a>Ev sahibinin adını nasıl değiştirirsiniz?

Yapamazsınız.  VM oluşturulduktan sonra, kullanıcılar (sahipleri dahil) ana bilgisayar adını güncelleştiremez.


## <a name="how-do-you-reset-the-remote-desktop-service-or-its-sign-in-password"></a>Uzak Masaüstü hizmetini veya oturum açma parolasını nasıl sıfırlarsınız?

Aşağıdaki makaleler, Windows ve Linux tabanlı VM'ler için RDS sıfırlamalarının nasıl gerçekleştirildirilebildiğini açıklar:   

- [Windows VM’sinde Uzak Masaüstü hizmetini veya oturum açma parolasını sıfırlama](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
- [Linux VM parolası veya SSH anahtarı sıfırlama, SSH yapılandırmasını düzeltme ve VMAccess uzantısını kullanarak disk tutarlılığını denetleme](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)


## <a name="how-do-you-generate-new-ssh-certificates"></a>Yeni SSH sertifikalarını nasıl oluşturursunuz?

Sertifikaların oluşturulması makalede açıklanmıştır Sonraki bölümde [VM görüntünüz için paylaşılan erişim imzası URI](./cpp-get-sas-uri.md) alın [VM teklifi için teknik varlıkları oluşturun.](./cpp-create-technical-assets.md)


## <a name="how-do-you-configure-a-virtual-private-network-vpn-to-work-with-my-vms"></a>Sanal özel ağı (VPN) Sanal Cihazlarımla çalışacak şekilde nasıl yapılandırabilirsiniz?

Azure Kaynak Yöneticisi dağıtım modelini kullanıyorsanız, VPN kurmanın üç ortak seçeneğiniz vardır:
- [Azure portalını kullanarak rota tabanlı BIR VPN ağ geçidi oluşturma](https://docs.microsoft.com/azure/vpn-gateway/create-routebased-vpn-gateway-portal)
- [PowerShell'i kullanarak rota tabanlı VPN ağ geçidi oluşturma](https://docs.microsoft.com/azure/vpn-gateway/create-routebased-vpn-gateway-powershell)
- [CLI kullanarak rota tabanlı VPN ağ geçidi oluşturma](https://docs.microsoft.com/azure/vpn-gateway/create-routebased-vpn-gateway-cli)


## <a name="what-are-microsoft-support-policies-for-running-microsoft-server-software-on-azure-based-vms"></a>Microsoft sunucu yazılımlarını Azure tabanlı VM'lerde çalıştırmak için Microsoft destek ilkeleri nelerdir?

Bu destek ilkeleri, Microsoft [Azure sanal makineleri için Microsoft sunucu yazılım desteği](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)makalesinde ayrıntılı olarak açıklanmıştır.


## <a name="do-virtual-machines-have-unique-identifiers-associated-with-them"></a>Sanal makinelerin kendileriyle ilişkili benzersiz tanımlayıcıları var mı?

Evet, Azure'da barındırılıyorsa.  Azure, oluşturulan her yeni VM kaynağına Azure Sanal Makine Benzersiz Kimliği adında benzersiz bir tanımlayıcı atar.  Daha fazla bilgi için [Azure Sanal Makine Benzersiz Kimliği](https://blogs.msdn.microsoft.com/wasimbloch/2016/10/20/azure-virtual-machine-unique-id/)adlı blog gönderisini okuyun.  Ayrıca bu tanımlayıcıyı [Liste API'si](https://docs.microsoft.com/rest/api/compute/virtualmachines/list)aracılığıyla programlı olarak elde edebilirsiniz.


## <a name="in-a-vm-how-do-you-manage-the-custom-script-extension-in-the-startup-task"></a>VM'de, başlangıç görevinde özel komut dosyası uzantısını nasıl yönetirsiniz?

Aşağıdaki makalede Azure PowerShell modüllerini kullanarak Özel Komut Dosyası Uzantısı'nın nasıl kullanılacağı, Azure Kaynak Yöneticisi şablonları ve Windows sistemlerindeki sorun giderme adımları ayrıntıları: [Windows için Özel Komut Dosyası Uzantısı](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)


## <a name="are-32-bit-applications-or-services-supported-in-the-azure-marketplace"></a>Azure Marketi'nde 32 bit uygulamalar veya hizmetler desteklendi mi?

Genellikle yapamazsınız.  Azure VM'ler için desteklenen işletim sistemleri ve standart hizmetlerin tümü 64 bit'tir.  Ancak, teknik açıdan, çoğu 64 bit işletim sistemleri geriye dönük uyumluluk için uygulamaların 32 bit sürümlerini çalıştırmayı destekler.  Ancak, VM çözümünüzün bir parçası olarak 32 bit uygulamaların kullanımı desteklenmez ve bu nedenle *son derece önerilmez.*  Bunun yerine, uygulamanızı 64 bit proje olarak yeniden derle....

Daha fazla bilgi için aşağıdaki makalelere bakın:
- [32 bit uygulamaları çalıştırma](https://docs.microsoft.com/windows/desktop/WinProg64/running-32-bit-applications)
- [Azure sanal makinelerinde 32 bit işletim sistemleri desteği](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines)
- [Microsoft Azure sanal makineleri için Microsoft sunucu yazılımı desteği](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)


## <a name="every-time-i-try-to-create-an-image-from-my-vhds-i-get-the-error-vhd-is-already-registered-with-image-repository-as-the-resource-in-powershell-i-did-not-create-any-image-before-nor-did-i-find-any-image-with-this-name-in-azure-how-do-i-resolve-this-issue"></a>VHD'lerimden bir görüntü oluşturmaya her çalıştığımda PowerShell'de hata `.VHD is already registered with image repository as the resource` alıyorum. Daha önce herhangi bir resim oluşturmadım ve Azure'da bu ada sahip herhangi bir resim buldum. Bu sorunu nasıl çözebilirim?

Bu sorun genellikle kullanıcı üzerinde kilit olan bir VHD vm sağlanan oluşur.  Bu VHD'den vm ayrılmadığını doğrulayın ve işlemi yeniden deneyin.  Bu sorun devam ederse, [Bulut İş Ortağı Portalı Için Destek'te](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-support-for-cloud-partner-portal)açıklandığı gibi bir destek bileti açın. 

