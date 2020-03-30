---
title: Azure Image Builder 'ı kullanarak varolan bir resim sürümünden yeni bir resim sürümü oluşturma (önizleme)
description: Azure Image Builder'ı kullanarak varolan bir resim sürümünden yeni bir VM resim sürümü oluşturun.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-windows
manager: gwallace
ms.openlocfilehash: 60a37588ddcac339d9545fb6f597ef7bdc17ccb4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247376"
---
# <a name="preview-create-a-new-vm-image-version-from-an-existing-image-version-using-azure-image-builder"></a>Önizleme: Azure Image Builder'ı kullanarak varolan bir resim sürümünden yeni bir VM resim sürümü oluşturun

Bu makalede, [Paylaşılan Resim Galerisi'nde](shared-image-galleries.md)varolan bir resim sürümünü nasıl alacağınızı, nasıl güncelleştirdiğinizi ve galeriye yeni bir resim sürümü olarak nasıl yayımlayacağınızı gösterir.

Görüntüyü yapılandırmak için bir örnek .json şablonu kullanıyor olacağız. Kullandığımız .json dosyası burada: [helloImageTemplateforSIGfromWinSIG.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/2_Creating_a_Custom_Win_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromWinSIG.json). 

> [!IMPORTANT]
> Azure Image Builder şu anda genel önizlemede.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="register-the-features"></a>Özellikleri kaydedin
Önizleme sırasında Azure Image Builder'ı kullanmak için yeni özelliği kaydetmeniz gerekir.

```azurecli-interactive
az feature register --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview
```

Özellik kaydının durumunu denetleyin.

```azurecli-interactive
az feature show --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview | grep state
```

Kaydınızı kontrol edin.

```azurecli-interactive
az provider show -n Microsoft.VirtualMachineImages | grep registrationState
az provider show -n Microsoft.Storage | grep registrationState
az provider show -n Microsoft.Compute | grep registrationState
```

Kayıtlı demiyorlarsa, aşağıdakileri çalıştırın:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages
az provider register -n Microsoft.Storage
az provider register -n Microsoft.Compute
```


## <a name="set-variables-and-permissions"></a>Değişkenleri ve izinleri ayarlama

Paylaşılan Resim Galerinizi oluşturmak için [bir resim oluştur ve Paylaşılan Resim Galerisi'ne dağıtıkullandıysanız,](image-builder-gallery.md) ihtiyacımız olan değişkenleri zaten oluşturdunuz. Değilse, lütfen bu örnek için kullanılacak bazı değişkenleri kurulum.

Önizleme için, görüntü oluşturucu yalnızca kaynak yönetilen görüntüyle aynı Kaynak Grubunda özel görüntüler oluşturmayı destekler. Kaynak yönetilen görüntünüzle aynı kaynak grubu olacak şekilde bu örnekteki kaynak grubu adını güncelleştirin.

```azurecli-interactive
# Resource group name - we are using ibsigRG in this example
sigResourceGroup=myIBWinRG
# Datacenter location - we are using West US 2 in this example
location=westus
# Additional region to replicate the image to - we are using East US in this example
additionalregion=eastus
# name of the shared image gallery - in this example we are using myGallery
sigName=my22stSIG
# name of the image definition to be created - in this example we are using myImageDef
imageDefName=winSvrimages
# image distribution metadata reference name
runOutputName=w2019SigRo
# User name and password for the VM
username="user name for the VM"
vmpassword="password for the VM"
```

Abonelik kimliğiniz için bir değişken oluşturun. Bunu kullanarak `az account show | grep id`elde edebilirsiniz.

```azurecli-interactive
subscriptionID=<Subscription ID>
```

Güncellemek istediğiniz resim sürümünü alın.

```azurecli-interactive
sigDefImgVersionId=$(az sig image-version list \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID --query [].'id' -o json | grep 0. | tr -d '"' | tr -d '[:space:]')
```


Zaten kendi Paylaşılan Resim Galerisi'niz varsa ve önceki örneği izlemediyseniz, Galeri'ye erişebilmek için Resim Oluşturucu'nun Kaynak Grubuna erişmesi için izinler atamanız gerekir.


```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup
```


## <a name="modify-helloimage-example"></a>helloImage örneğini değiştirin
Kullanmak üzere olduğumuz örneği burada .json dosyasını açarak inceleyebilirsiniz: [helloImageTemplateforSIGfromSIG.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/2_Creating_a_Custom_Linux_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromSIG.json) ile birlikte [Image Builder şablonu başvurusu.](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) 


.json örneğini indirin ve değişkenlerinizle yapılandırın. 

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/8_Creating_a_Custom_Win_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromWinSIG.json -o helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<rgName>/$sigResourceGroup/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<imageDefName>/$imageDefName/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<sharedImageGalName>/$sigName/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s%<sigDefImgVersionId>%$sigDefImgVersionId%g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<region1>/$location/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<region2>/$additionalregion/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateforSIGfromWinSIG.json
```

## <a name="create-the-image"></a>Görüntü oluşturma

Görüntü yapılandırmasını VM Image Builder Service'e gönderin.

```azurecli-interactive
az resource create \
    --resource-group $sigResourceGroup \
    --properties @helloImageTemplateforSIGfromWinSIG.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n imageTemplateforSIGfromWinSIG01
```

Görüntü oluşturmayı başlatın.

```azurecli-interactive
az resource invoke-action \
     --resource-group $sigResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n imageTemplateforSIGfromWinSIG01 \
     --action Run 
```

Bir sonraki adıma geçmeden önce görüntü nün oluşturulmasını ve çoğaltılmasını bekleyin.


## <a name="create-the-vm"></a>Sanal makine oluşturma

```azurecli-interactive
az vm create \
  --resource-group $sigResourceGroup \
  --name aibImgWinVm002 \
  --admin-username $username \
  --admin-password $vmpassword \
  --image "/subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup/providers/Microsoft.Compute/galleries/$sigName/images/$imageDefName/versions/latest" \
  --location $location
```

## <a name="verify-the-customization"></a>Özelleştirmeyi doğrulama
VM'yi oluştururken belirlediğiniz kullanıcı adı ve parolayı kullanarak VM'ye Uzak Masaüstü bağlantısı oluşturun. VM'nin içinde cmd istemi ni açın ve yazın:

```console
dir c:\
```

Şimdi iki dizin görmelisiniz:
- `buildActions`bu ilk görüntü sürümünde oluşturuldu.
- `buildActions2`ikinci resim sürümünü oluşturmak için ilk görüntü sürümünü güncelleme parçası olarak oluşturuldu.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan .json dosyasının bileşenleri hakkında daha fazla bilgi edinmek için [Bkz. Resim oluşturucu şablonu başvurusu.](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)