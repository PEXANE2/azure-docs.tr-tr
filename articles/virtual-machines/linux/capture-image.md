---
title: Azure CLI kullanarak bir Linux VM görüntüsünü yakalayın
description: Azure CLI'yi kullanarak toplu dağıtımlarda kullanmak üzere bir Azure VM görüntüsünü yakalayın.
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 10/08/2018
ms.author: cynthn
ms.openlocfilehash: 77f6244651551763f5460432655d66267775a256
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250406"
---
# <a name="how-to-create-an-image-of-a-virtual-machine-or-vhd"></a>Sanal makine veya VHD görüntüsü oluşturma

Azure'da kullanılmak üzere sanal bir makinenin (VM) birden çok kopyasını oluşturmak için VM veya OS VHD'nin bir görüntüsünü yakalayın. Dağıtım için bir resim oluşturmak için kişisel hesap bilgilerini kaldırmanız gerekir. Aşağıdaki adımlarda, varolan bir VM'yi deprovision, anlaşma yerini saptave bir görüntü oluşturursunuz. Aboneliğinizdeki herhangi bir kaynak grubunda VM oluşturmak için bu resmi kullanabilirsiniz.

Yedekleme veya hata ayıklama için mevcut Linux VM'nizin bir kopyasını oluşturmak veya şirket içi bir VM'den özel bir Linux VHD yüklemek [için, Upload'a bakın ve özel disk görüntüsünden bir Linux VM oluşturun.](upload-vhd.md)  

**Azure VM Image Builder (Genel Önizleme)** hizmetini, özel resminizi oluşturmak için kullanabilirsiniz, herhangi bir araç veya kurulum yapı denetim hatları öğrenmenize gerek yok, yalnızca bir görüntü yapılandırması sağlar ve Görüntü Oluşturucu görüntü oluşturur. Daha fazla bilgi için Azure [VM Image Builder ile Başlarken'e](https://docs.microsoft.com/azure/virtual-machines/linux/image-builder-overview)bakın.

Bir resim oluşturmadan önce aşağıdaki öğelere ihtiyacınız olacak:

* Yönetilen diskleri kullanan Kaynak Yöneticisi dağıtım modelinde oluşturulan bir Azure VM. Henüz bir Linux VM oluşturmadıysanız, [Portalı](quick-create-portal.md) [(Azure CLI)](quick-create-cli.md)veya Kaynak [Yöneticisi şablonlarını](create-ssh-secured-vm-from-template.md)kullanabilirsiniz. VM'yi gerektiği gibi yapılandırın. Örneğin, [veri diskleri ekleyin,](add-disk.md)güncelleştirmeleri uygulayın ve uygulamaları yükleyin. 

* En son [Azure CLI](/cli/azure/install-az-cli2) yüklendi ve az giriş ile bir Azure [hesabına](/cli/azure/reference-index#az-login)giriş yaptı.

## <a name="prefer-a-tutorial-instead"></a>Bunun yerine bir öğretici tercih?

Bu makalenin basitleştirilmiş bir sürümü için ve Azure'daki VM'leri test etmek, değerlendirmek veya öğrenmek için [Bkz. CLI'yi kullanarak Azure VM'nin özel bir görüntüsünü oluştur.](tutorial-custom-images.md)  Aksi takdirde, tam resmi almak için burada okumaya devam edin.


## <a name="step-1-deprovision-the-vm"></a>Adım 1: VM'nin hükmünü bozun
Önce, makineye özgü dosyaları ve verileri silmek için Azure VM aracısını kullanarak VM'yi etkisiz olarak kullanırsınız. Kaynağınızdaki `waagent` Linux `-deprovision+user` VM parametresi ile komutu kullanın. Daha fazla bilgi için bkz. [Azure Linux Aracısı kullanıcı kılavuzu](../extensions/agent-linux.md).

1. Bir SSH istemcisi ile Linux VM'nize bağlanın.
2. SSH penceresinde aşağıdaki komutu girin:
   
    ```bash
    sudo waagent -deprovision+user
    ```
   > [!NOTE]
   > Bu komutu yalnızca görüntü olarak yakaladığınız bir VM'de çalıştırın. Bu komut, görüntünün tüm hassas bilgilerden temizlendiğini veya yeniden dağıtım için uygun olduğunu garanti etmez. `+user` Parametre, son sağlanan kullanıcı hesabını da kaldırır. Kullanıcı hesabı kimlik bilgilerini VM'de `-deprovision`tutmak için yalnızca .
 
3. Devam etmek için **y** girin. Bu onay `-force` adımını önlemek için parametreyi ekleyebilirsiniz.
4. Komut tamamlandıktan sonra, SSH istemcisini kapatmak için **çıkışgirin.**  VM hala bu noktada çalışıyor olacak.

## <a name="step-2-create-vm-image"></a>Adım 2: VM görüntüsü oluşturma
VM'yi genelleştirilmiş olarak işaretlemek ve görüntüyü yakalamak için Azure CLI'yi kullanın. Aşağıdaki örneklerde, örnek parametre adlarını kendi değerlerinizle değiştirin. Örnek parametre adları *myResourceGroup,* *myVnet*ve *myVM'i*içerir.

1. Deallocate [az vm deallocate](/cli/azure/vm)ile deprovisioned VM . Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myVM* adlı VM'yi yerle bir eder.  
   
    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myVM
    ```
    
    VM'nin devam etmeden önce tamamen anlaşmasını bekleyin. Bu işlem tamamlanması birkaç dakika sürebilir.  VM ayırma sırasında kapatılır.

2. [Az vm genelleme](/cli/azure/vm)ile genelleştirilmiş olarak VM işaretleyin. Aşağıdaki örnekte, *myResourceGroup* adlı kaynak grubunda *myVM* adlı VM genelleştirilmiş olarak işaretleneişarettir.
   
    ```azurecli
    az vm generalize \
      --resource-group myResourceGroup \
      --name myVM
    ```

    Genelleştirilmiş bir VM artık yeniden başlatılamaz.

3. Az görüntü oluşturmak ile VM kaynağının bir [görüntü oluşturun.](/cli/azure/image#az-image-create) Aşağıdaki örnek, *myVM*adlı VM kaynağını kullanarak *myResourceGroup* adlı kaynak grubunda *myImage* adlı bir resim oluşturur.
   
    ```azurecli
    az image create \
      --resource-group myResourceGroup \
      --name myImage --source myVM
    ```
   
   > [!NOTE]
   > Görüntü, kaynağınız VM ile aynı kaynak grubunda oluşturulur. Aboneliğinizdeki herhangi bir kaynak grubunda bu resimden VM'ler oluşturabilirsiniz. Yönetim açısından, VM kaynaklarınız ve resimleriniz için belirli bir kaynak grubu oluşturmak isteyebilirsiniz.
   >
   > Resminizi bölgeye dayanıklı depolama alanında depolamak istiyorsanız, görüntünüzkullanılabilirlik [bölgelerini](../../availability-zones/az-overview.md) destekleyen ve `--zone-resilient true` parametreyi içeren bir bölgede oluşturmanız gerekir.
   
Bu komut, VM görüntüsünü açıklayan JSON'ı döndürür. Bu çıktıyı daha sonraki başvuruiçin kaydedin.

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Adım 3: Yakalanan görüntüden VM oluşturma
[Az vm create](/cli/azure/vm)ile oluşturduğunuz görüntüyü kullanarak bir VM oluşturun. Aşağıdaki örnek, *myImage*adlı görüntüden *myVMDeployed* adlı bir VM oluşturur.

```azurecli
az vm create \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --image myImage\
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```

### <a name="creating-the-vm-in-another-resource-group"></a>Başka bir kaynak grubunda VM oluşturma 

Aboneliğinizdeki herhangi bir kaynak grubundaki bir görüntüden VM'ler oluşturabilirsiniz. Görüntüden farklı bir kaynak grubunda VM oluşturmak için resminize tam kaynak kimliği belirtin. Görüntülerin listesini görüntülemek için [az resim listesini](/cli/azure/image#az-image-list) kullanın. Çıktı aşağıdaki örneğe benzerdir.

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

Aşağıdaki örnek, görüntü kaynağı kimliğini belirterek kaynak görüntü dışındaki bir kaynak grubunda VM oluşturmak için [az vm oluşturmayı](/cli/azure/vm#az-vm-create) kullanır.

```azurecli
az vm create \
   --resource-group myOtherResourceGroup \
   --name myOtherVMDeployed \
   --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage" \
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```


## <a name="step-4-verify-the-deployment"></a>Adım 4: Dağıtımı doğrulayın

Dağıtımı doğrulamak ve yeni VM'yi kullanmaya başlamak için oluşturduğunuz sanal makineye SSH. SSH üzerinden bağlanmak [için, az vm show](/cli/azure/vm#az-vm-show)ile VM IP adresini veya FQDN'yi bulun.

```azurecli
az vm show \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --show-details
```

## <a name="next-steps"></a>Sonraki adımlar
Kaynak VM resminizden birden çok VM oluşturabilirsiniz. Resminizde değişiklik yapmak için: 

- Resminizden bir VM oluşturun.
- Herhangi bir güncelleştirme veya yapılandırma değişikliği yapın.
- Bir görüntüyü dağıtmak, dağıtmak, genelleştirmek ve oluşturmak için adımları yeniden izleyin.
- Gelecekteki dağıtımlar için bu yeni görüntüyü kullanın. Orijinal resmi silebilirsiniz.

CLI ile VM'lerinizi yönetme hakkında daha fazla bilgi için [Azure CLI'ye](/cli/azure)bakın.
