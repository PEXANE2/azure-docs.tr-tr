---
title: Azure'da SAP HANA için ek ağ gereksinimleri (büyük örnekler) | Microsoft Dokümanlar
description: Azure'da SAP HANA için ek ağ gereksinimleri (büyük örnekler).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 66bbd8b462ac35756be0fae6eba940ba0aba6c4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77614577"
---
# <a name="additional-network-requirements-for-large-instances"></a>Büyük örnekler için ek ağ gereksinimleri

Azure'da büyük SAP HANA örneklerinin dağıtımının bir parçası olarak ek ağ gereksinimleriniz olabilir.

## <a name="add-more-ip-addresses-or-subnets"></a>Daha fazla IP adresi veya alt ağ ekleme

Daha fazla IP adresi veya alt ağ eklerken Azure portalını, PowerShell'i veya Azure CLI'yi kullanın.

Yeni bir toplu aralık oluşturmak yerine, sanal ağ adresi alanına yeni bir aralık olarak yeni bir IP adresi aralığı ekleyin. Bu değişikliği Microsoft'a gönderin. Bu, yeni IP adresi aralığından istemcinizdeki HANA büyük örnek birimlerine bağlanmanızı sağlar. Yeni sanal ağ adresi alanının eklenmesini sağlamak için bir Azure destek isteği açabilirsiniz. Onay aldıktan sonra, sonraki adımları gerçekleştirin.

Azure portalından ek bir alt ağ oluşturmak [için](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network)bkz. PowerShell'den bir tane oluşturmak için [powershell'i kullanarak sanal ağ oluşturma'ya](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network)bakın.

## <a name="add-virtual-networks"></a>Sanal ağlar ekleme

Başlangıçta bir veya daha fazla Azure sanal abuna bağlandıktan sonra, Azure'da SAP HANA'ya erişen ek ağları (büyük örnekler) bağlamak isteyebilirsiniz. İlk olarak, bir Azure destek isteği gönderin. Bu istekte, belirli Azure dağıtımını tanımlayan belirli bilgileri ekleyin. Ayrıca, Azure sanal ağ adres alanının IP adresi alanı aralığını veya aralıklarını da ekleyin. Microsoft Service Management'daki SAP HANA, ek sanal ağları ve Azure ExpressRoute'u bağlamak için gereken gerekli bilgileri sağlar. Her sanal ağ için, ExpressRoute devresine HANA'nın büyük örneklerine bağlanmak için benzersiz bir yetkilendirme anahtarına ihtiyacınız vardır.

## <a name="increase-expressroute-circuit-bandwidth"></a>ExpressRoute devre bant genişliğini artırın

Microsoft Hizmet Yönetimi konusunda SAP HANA'ya danışın. Azure (büyük örnekler) ExpressRoute devresi üzerindeki SAP HANA'nın bant genişliğini artırmanızı tavsiye ederlerse, bir Azure destek isteği oluşturun. (Tek bir devre bant genişliği için maksimum 10 Gbps'ye kadar bir artış talep edebilirsiniz.) Daha sonra işlem tamamlandıktan sonra bildirim alırsınız; Azure'da bu yüksek hızı sağlamak için başka bir şey yapmanız gerekmez.

## <a name="add-an-additional-expressroute-circuit"></a>Ek bir ExpressRoute devresi ekleme

Microsoft Hizmet Yönetimi konusunda SAP HANA'ya danışın. Ek bir ExpressRoute devresi eklemenizi tavsiye ederlerse, bir Azure destek isteği oluşturun (yeni devreye bağlanmak için yetkilendirme bilgileri alma isteği de dahil). İstekte bulunmadan önce, sanal ağlarda kullanılan adres alanını tanımlamanız gerekir. Microsoft Service Management'daki SAP HANA daha sonra yetkilendirme sağlayabilir.

Yeni devre oluşturulduğunda ve Microsoft Hizmet Yönetimi yapılandırması üzerindeki SAP HANA tamamlandığında, devam etmek için gereken bilgileri içeren bir bildirim alırsınız. Azure sanal ağlarını, aynı Azure bölgesindeki Azure'daki başka bir SAP HANA'ya (büyük örnek) ExpressRoute devresine zaten bağlıysa, bu ek devreye bağlayamadığınızda, bu ek devreye bağlayamadığınızda, bu ek devreye bağlayamadığınızda, bu ek devreye bağlanamazsınız.

## <a name="delete-a-subnet"></a>Bir alt ağı silme

Sanal ağ alt ağını kaldırmak için Azure portalını, PowerShell'i veya Azure CLI'yi kullanabilirsiniz. Azure sanal ağ IP adres aralığınız veya adres alanınız toplu bir aralıksa, Microsoft ile sizin için herhangi bir takip yoktur. (Ancak, sanal ağın silinen alt ağı içeren BGP rota adresi alanını hala yaydığını unutmayın.) Azure sanal ağ adres aralığını veya adres alanını, silinen alt ağınıza atanan birden çok IP adresi aralığı olarak tanımlamış olabilirsiniz. Bunu sanal ağ adres alanınızdan sildiğinizden emin olun. Ardından SAP HANA'yı Microsoft Hizmet Yönetimi'nden, Azure'daki SAP HANA'nın iletişim kurmasına izin verilen aralıklardan kaldırması için bilgilendirin.

Daha fazla bilgi için [bkz.](../../../virtual-network/virtual-network-manage-subnet.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-subnet)

## <a name="delete-a-virtual-network"></a>Sanal ağı silme

Bilgi için [bkz.](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-virtual-network)

Microsoft Service Management'daki SAP HANA, Azure'daki SAP HANA'daki varolan yetkilendirmeleri (büyük örnekler) ExpressRoute devresi üzerinde kaldırır. Ayrıca, HANA'nın büyük örnekleriyle iletişim için Azure sanal ağ IP adres aralığını veya adres alanını da kaldırır.

Sanal ağı kaldırdıktan sonra, IP adresi alanı aralığını veya kaldırılacak aralıkları sağlamak için bir Azure destek isteği açın.

Her şeyi kaldırdığınızdan emin olmak için ExpressRoute bağlantısını, sanal ağ ağ ağ geçidini, sanal ağ ağ geçidi ni ve sanal ağı silin.

## <a name="delete-an-expressroute-circuit"></a>Bir ExpressRoute devresi silme

Azure (büyük örnekler) ExpressRoute devresindeki ek bir SAP HANA'yı kaldırmak için Microsoft Service Management'ta SAP HANA ile bir Azure destek isteği açın. Devrenin silinmesini isteyin. Azure aboneliği nde, gerektiğinde sanal ağı silebilir veya saklayabilirsiniz. Ancak, HANA büyük örnekleri ExpressRoute devresi ve bağlantılı sanal ağ ağ geçidi arasındaki bağlantıyı silmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [SAP HANA'yı (büyük örnekler) Azure'a yükleme ve yapılandırma](hana-installation.md)
