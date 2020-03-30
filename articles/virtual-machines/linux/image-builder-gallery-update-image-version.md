---
title: Azure Image Builder 'ı kullanarak varolan bir resim sürümünden yeni bir VM resim sürümü oluşturma (önizleme)
description: Azure Image Builder'ı kullanarak varolan bir resim sürümünden yeni bir VM resim sürümü oluşturun.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.subservice: imaging
manager: gwallace
ms.openlocfilehash: 5766e91dc6a17d50c46d396dd8a68d17081e0926
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246815"
---
# <a name="preview-create-a-new-vm-image-version-from-an-existing-image-version-using-azure-image-builder"></a>Önizleme: Azure Image Builder'ı kullanarak varolan bir resim sürümünden yeni bir VM resim sürümü oluşturun

Bu makalede, [Paylaşılan Resim Galerisi'nde](shared-image-galleries.md)varolan bir resim sürümünü nasıl alacağınızı, nasıl güncelleştirdiğinizi ve galeriye yeni bir resim sürümü olarak nasıl yayımlayacağınızı gösterir.

Görüntüyü yapılandırmak için bir örnek .json şablonu kullanıyor olacağız. Kullandığımız .json dosyası burada: [helloImageTemplateforSIGfromSIG.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/2_Creating_a_Custom_Linux_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromSIG.json). 


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
```

Kayıtlı demiyorlarsa, aşağıdakileri çalıştırın:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages

az provider register -n Microsoft.Storage
```


## <a name="set-variables-and-permissions"></a>Değişkenleri ve izinleri ayarlama

Paylaşılan Resim Galerisi'nizi oluşturmak için [bir resim oluştur ve Paylaşılan Resim Galerisi'ne dağıtım](image-builder-gallery.md) kullandıysanız, zaten ihtiyacımız olan değişkenlerden bazılarını oluşturdunuz. Değilse, lütfen bu örnek için kullanılacak bazı değişkenleri kurulum.

Önizleme için, görüntü oluşturucu yalnızca kaynak yönetilen görüntüyle aynı Kaynak Grubunda özel görüntüler oluşturmayı destekler. Kaynak yönetilen görüntünüzle aynı kaynak grubu olacak şekilde bu örnekteki kaynak grubu adını güncelleştirin.


```console
# Resource group name 
sigResourceGroup=ibLinuxGalleryRG
# Gallery location 
location=westus2
# Additional region to replicate the image version to 
additionalregion=eastus
# Name of the shared image gallery 
sigName=myIbGallery
# Name of the image definition to use
imageDefName=myIbImageDef
# image distribution metadata reference name
runOutputName=aibSIGLinuxUpdate
```

Abonelik kimliğiniz için bir değişken oluşturun. Bunu kullanarak `az account show | grep id`elde edebilirsiniz.

```console
subscriptionID=<Subscription ID>
```

Güncellemek istediğiniz resim sürümünü alın.

```azurecli
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
Kullanmak üzere olduğumuz örneği burada .json dosyasını açarak inceleyebilirsiniz: [helloImageTemplateforSIGfromSIG.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/2_Creating_a_Custom_Linux_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromSIG.json) ile birlikte [Image Builder şablonu başvurusu.](image-builder-json.md) 


.json örneğini indirin ve değişkenlerinizle yapılandırın. 

```console
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/8_Creating_a_Custom_Linux_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromSIG.json -o helloImageTemplateforSIGfromSIG.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateforSIGfromSIG.json
sed -i -e "s/<rgName>/$sigResourceGroup/g" helloImageTemplateforSIGfromSIG.json
sed -i -e "s/<imageDefName>/$imageDefName/g" helloImageTemplateforSIGfromSIG.json
sed -i -e "s/<sharedImageGalName>/$sigName/g" helloImageTemplateforSIGfromSIG.json
sed -i -e "s%<sigDefImgVersionId>%$sigDefImgVersionId%g" helloImageTemplateforSIGfromSIG.json
sed -i -e "s/<region1>/$location/g" helloImageTemplateforSIGfromSIG.json
sed -i -e "s/<region2>/$additionalregion/g" helloImageTemplateforSIGfromSIG.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateforSIGfromSIG.json
```

## <a name="create-the-image"></a>Görüntü oluşturma

Görüntü yapılandırmasını VM Image Builder Service'e gönderin.

```azurecli-interactive
az resource create \
    --resource-group $sigResourceGroup \
    --properties @helloImageTemplateforSIGfromSIG.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforSIGfromSIG01
```

Görüntü oluşturmayı başlatın.

```azurecli-interactive
az resource invoke-action \
     --resource-group $sigResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateforSIGfromSIG01 \
     --action Run 
```

Bir sonraki adıma geçmeden önce görüntü nün oluşturulmasını ve çoğaltılmasını bekleyin.


## <a name="create-the-vm"></a>Sanal makine oluşturma

```azurecli-interactive
az vm create \
  --resource-group $sigResourceGroup \
  --name aibImgVm001 \
  --admin-username azureuser \
  --location $location \
  --image "/subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup/providers/Microsoft.Compute/galleries/$sigName/images/$imageDefName/versions/latest" \
  --generate-ssh-keys
```

VM'nin genel IP adresini kullanarak VM'ye Bir SSH bağlantısı oluşturun.

```console
ssh azureuser@<pubIp>
```

SSH bağlantınız kurulur kurulmaz görüntünün "Günün Mesajı" ile özelleştirilmenizi görmeniz gerekir.

```output
*******************************************************
**            This VM was built from the:            **
**      !! AZURE VM IMAGE BUILDER Custom Image !!    **
**         You have just been Customized :-)         **
*******************************************************
```

SSH bağlantısını kapatmak için yazın. `exit`

Galerinizde bulunan resim sürümlerini de listeleyebilirsiniz.

```azurecli-interactive
az sig image-version list -g $sigResourceGroup -r $sigName -i $imageDefName -o table
```


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan .json dosyasının bileşenleri hakkında daha fazla bilgi edinmek için [Bkz. Resim oluşturucu şablonu başvurusu.](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)