---
title: Azure Kaynak Yöneticisi şablon örnekleri
description: Azure Kapsayıcı Örneklerini farklı yapılandırmalarda dağıtmak için Azure Kaynak Yöneticisi şablon örneklerini bulma
ms.topic: article
ms.date: 03/07/2019
ms.openlocfilehash: a8f3c81c539562a3c56e4822cf6e4df77d04928f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75981660"
---
# <a name="azure-resource-manager-templates-for-azure-container-instances"></a>Azure Kapsayıcı Örnekleri için Azure Kaynak Yöneticisi şablonları

Aşağıdaki örnek şablonlar kapsayıcı örneklerini çeşitli yapılandırmalarda dağıtMaktadır.

Dağıtım seçenekleri için [Dağıtım](#deployment) bölümüne bakın. Kendi şablonlarınızı oluşturmak istiyorsanız, Azure Kapsayıcı Örnekleri [Kaynak Yöneticisi şablon başvuru][ref] bilgileri şablon biçimi ve kullanılabilir özellikler.

## <a name="sample-templates"></a>Örnek şablonlar

| | |
|-|-|
| **Uygulamalar** ||
| [WordPress][app-wp] | Bir kapsayıcı grubunda bir WordPress web sitesi ve MySQL veritabanı oluşturur. WordPress site içeriği ve MySQL veritabanı, Azure Dosyaları paylaşımına devam etti. Ayrıca WordPress için ortak ağ erişimi ortaya çıkarmak için bir uygulama ağ geçidi oluşturur. |
| [SQL Server ve IIS ile MS NAV][app-nav] | Tam özellikli bağımsız Dynamics NAV / Dynamics 365 Business Central ortamına sahip tek bir Windows kapsayıcısı dağıtır. |
| **Birim** ||
| [emptyDir][vol-emptydir] | BoşDir hacmini paylaşan iki Linux kapsayıcısı dağıtır. |
| [gitRepo][vol-gitrepo] | Bir GitHub repo klonlar ve bir hacim olarak bağlar bir Linux kapsayıcı dağıtır. |
| [Gizli][vol-secret] | Gizli bir ses olarak monte edilmiş PFX sertifikasına sahip bir Linux kapsayıcısı dağıtır. |
| **Ağ Oluşturma** ||
| [UDP maruz konteyner][net-udp] | UDP bağlantı noktasını ortaya çıkaran bir Windows veya Linux kapsayıcısı dağıtır. |
| [Kamu IP ile Linux konteyner][net-publicip] | Herkese açık bir IP üzerinden erişilebilen tek bir Linux kapsayıcısı dağıtıyor. |
| [Sanal ağa sahip bir kapsayıcı grubunu dağıtma (önizleme)][net-vnet] | Yeni bir sanal ağ, alt ağ, ağ profili ve kapsayıcı grubu dağıtıyor. |
| **Azure kaynakları** ||
| [Azure Depolama hesabı oluşturma ve Dosyalar paylaşımı][az-files] | Bir depolama hesabı ve Azure Dosyaları paylaşımı oluşturmak için kapsayıcı örneğinde Azure CLI'yi kullanır.

## <a name="deployment"></a>Dağıtım

Kaynak Yöneticisi şablonlarıyla kaynak dağıtmak için çeşitli seçenekleriniz vardır:

[Azure CLI][deploy-cli]

[Azure PowerShell][deploy-powershell]

[Azure portalında][deploy-portal]

[REST API][deploy-rest]

<!-- LINKS - External -->
[app-nav]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-dynamicsnav
[app-wp]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-wordpress
[az-files]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-storage-file-share
[net-publicip]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-linuxcontainer-public-ip
[net-udp]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-udp
[net-vnet]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-vnet
[repo]: https://github.com/Azure/azure-quickstart-templates
[vol-emptydir]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-linuxcontainer-volume-emptydir
[vol-gitrepo]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-linuxcontainer-volume-gitrepo
[vol-secret]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-linuxcontainer-volume-secret

<!-- LINKS - Internal -->
[deploy-cli]: ../azure-resource-manager/templates/deploy-cli.md
[deploy-portal]: ../azure-resource-manager/templates/deploy-portal.md
[deploy-powershell]: ../azure-resource-manager/templates/deploy-powershell.md
[deploy-rest]: ../azure-resource-manager/templates/deploy-rest.md
[ref]: /azure/templates/microsoft.containerinstance/containergroups
