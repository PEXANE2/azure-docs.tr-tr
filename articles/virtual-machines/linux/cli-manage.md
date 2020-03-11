---
title: Ortak Azure CLı komutları
description: Azure Resource Manager modunda sanal makinelerinizi yönetmeye başlamanızı sağlamak için genel Azure CLı komutlarından bazılarını öğrenin
author: RicksterCDN
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 05/12/2017
ms.author: rclaus
ms.openlocfilehash: 253f2ab1b192d22f43e4082766adf4ec4f86fe71
ms.sourcegitcommit: 5f39f60c4ae33b20156529a765b8f8c04f181143
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "78969245"
---
# <a name="common-azure-cli-commands-for-managing-azure-resources"></a>Azure kaynaklarını yönetmeye yönelik genel Azure CLı komutları

Azure CLı, Azure kaynaklarınızı macOS, Linux ve Windows üzerinde oluşturmanıza ve yönetmenize olanak sağlar. Bu makalede, sanal makineler (VM 'Ler) oluşturmak ve yönetmek için en sık kullanılan komutların bazıları ayrıntılı olarak açıklanır.

Bu makale, Azure CLı sürüm 2.0.4 veya üstünü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltmeniz gerekiyorsa bkz. [Azure CLI 'Yı yüklemek](/cli/azure/install-azure-cli). Ayrıca, tarayıcınızdan [Cloud Shell](/azure/cloud-shell/quickstart) de kullanabilirsiniz.

## <a name="basic-azure-resource-manager-commands-in-azure-cli"></a>Azure CLI’daki Temel Azure Resource Manager komutları
Belirli komut satırı anahtarları ve seçenekleriyle ilgili daha ayrıntılı yardım için `az <command> <subcommand> --help`yazarak çevrimiçi komut yardımı ve seçeneklerini kullanabilirsiniz.

### <a name="create-vms"></a>VM oluşturma
| Görev | Azure CLI komutları |
| --- | --- |
| Kaynak grubu oluşturma | `az group create --name myResourceGroup --location eastus` |
| Linux VM oluşturma | `az vm create --resource-group myResourceGroup --name myVM --image ubuntults` |
| Windows VM oluşturma | `az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter` |

### <a name="manage-vm-state"></a>VM durumunu yönetme
| Görev | Azure CLI komutları |
| --- | --- |
| VM başlatma | `az vm start --resource-group myResourceGroup --name myVM` |
| VM durdurma | `az vm stop --resource-group myResourceGroup --name myVM` |
| Bir VM’yi serbest bırakma | `az vm deallocate --resource-group myResourceGroup --name myVM` |
| Bir VM’yi yeniden başlatma | `az vm restart --resource-group myResourceGroup --name myVM` |
| Bir VM’yi yeniden dağıtma | `az vm redeploy --resource-group myResourceGroup --name myVM` |
| VM silme | `az vm delete --resource-group myResourceGroup --name myVM` |

### <a name="get-vm-info"></a>VM bilgilerini al
| Görev | Azure CLI komutları |
| --- | --- |
| VM'leri listeleme | `az vm list` |
| VM hakkında bilgi alma | `az vm show --resource-group myResourceGroup --name myVM` |
| VM kaynaklarının kullanımını alma | `az vm list-usage --location eastus` |
| Tüm kullanılabilir VM boyutlarını alma | `az vm list-sizes --location eastus` |

## <a name="disks-and-images"></a>Diskler ve görüntüler
| Görev | Azure CLI komutları |
| --- | --- |
| Bir VM’ye veri diski ekleme | `az vm disk attach --resource-group myResourceGroup --vm-name myVM --disk myDataDisk --size-gb 128 --new` |
| Bir VM’den veri diski kaldırma | `az vm disk detach --resource-group myResourceGroup --vm-name myVM --disk myDataDisk` |
| Bir diski yeniden boyutlandırma | `az disk update --resource-group myResourceGroup --name myDataDisk --size-gb 256` |
| Bir diskin anlık görüntüsünü alma | `az snapshot create --resource-group myResourceGroup --name mySnapshot --source myDataDisk` |
| VM görüntüsü oluşturma | `az image create --resource-group myResourceGroup --source myVM --name myImage` |
| Görüntüden VM oluştur | `az vm create --resource-group myResourceGroup --name myNewVM --image myImage` |


## <a name="next-steps"></a>Sonraki adımlar
CLı komutlarına ek örnekler için [Azure CLI Ile Linux VM 'Leri oluşturma ve yönetme](tutorial-manage-vm.md) öğreticisine bakın.



