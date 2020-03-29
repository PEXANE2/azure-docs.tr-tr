---
title: Eklenmemiş Azure NIC'lerini bulma ve silme
description: Azure CLI ile VM'lere bağlı olmayan Azure NIC'lerini bulma ve silme
author: cynthn
ms.service: virtual-machines
ms.subservice: networking
ms.workload: infrastructure-services
ms.topic: article
ms.date: 04/10/2018
ms.author: cynthn
ms.openlocfilehash: 8142b95ee666e205a8328eafd5930f1f386e49af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78945133"
---
# <a name="how-to-find-and-delete-unattached-network-interface-cards-nics-for-azure-vms"></a>Azure VM'ler için iliştirilmemiş ağ arabirim kartlarını (NIC' ler) bulma ve silme
Azure'da sanal bir makineyi (VM) sildiğinizde, ağ arabirimi kartları (NIC' ler) varsayılan olarak silinmez. Birden çok VM oluşturur ve silerseniz, kullanılmayan NIC'ler dahili IP adresi kiralamalarını kullanmaya devam eder. Diğer VM NIC'leri oluşturduğunuzda, alt ağın adres alanında IP kirası alamayabilirler. Bu makalede, iliştirilmemiş NIC'leri nasıl bulabileceğiniz ve sildiğiniz gösterilmektedir.

## <a name="find-and-delete-unattached-nics"></a>Eklenmemiş NIC’leri bulma ve silme

NIC için *virtualMachine* özelliği, NIC'nin bağlı olduğu VM'nin kimliğini ve kaynak grubunu depolar. Aşağıdaki komut dosyası, bir abonelikteki tüm NIC'leri döngüye sokuyor ve *virtualMachine* özelliğinin null olup olmadığını denetler. Bu özellik null ise, NIC bir VM bağlı değildir.

Tüm eklenmemiş NIC'leri görüntülemek için, komut dosyasını ilk olarak *deleteUnattachedNics* değişkeni *ile 0'a*çalıştırmanızı şiddetle tavsiye ederim. Liste çıktısını gözden geçirdikten sonra tüm eklenmemiş NIC'leri silmek için komut dosyasını *deleteUnattachedNics* ile *1'e*çalıştırın.

```azurecli
# Set deleteUnattachedNics=1 if you want to delete unattached NICs
# Set deleteUnattachedNics=0 if you want to see the Id(s) of the unattached NICs
deleteUnattachedNics=0

unattachedNicsIds=$(az network nic list --query '[?virtualMachine==`null`].[id]' -o tsv)
for id in ${unattachedNicsIds[@]}
do
   if (( $deleteUnattachedNics == 1 ))
   then

       echo "Deleting unattached NIC with Id: "$id
       az network nic delete --ids $id
       echo "Deleted unattached NIC with Id: "$id
   else
       echo $id
   fi
done
```

## <a name="next-steps"></a>Sonraki adımlar

Azure'da sanal ağların nasıl oluşturulup yönetilenhakkında daha fazla bilgi için [VM ağları oluşturma ve yönetme](tutorial-virtual-network.md)hakkında bilgi edinin.
