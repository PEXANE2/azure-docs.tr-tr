---
title: Azure görüntü Oluşturucu (Önizleme) kullanarak var olan bir görüntü sürümünden yeni bir VM görüntü sürümü oluşturma
description: Azure görüntü Oluşturucu kullanarak var olan bir görüntü sürümünden yeni bir VM görüntüsü sürümü oluşturun.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.subservice: imaging
manager: gwallace
ms.openlocfilehash: 5766e91dc6a17d50c46d396dd8a68d17081e0926
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80246815"
---
# <a name="preview-create-a-new-vm-image-version-from-an-existing-image-version-using-azure-image-builder"></a>Önizleme: Azure görüntü Oluşturucu kullanarak var olan bir görüntü sürümünden yeni bir VM görüntüsü sürümü oluşturma

Bu makalede, [paylaşılan görüntü galerisinde](shared-image-galleries.md)var olan bir görüntü sürümü alma, güncelleştirme ve Galeri için yeni bir görüntü sürümü olarak yayımlama işlemlerinin nasıl yapılacağı gösterilir.

Görüntüyü yapılandırmak için bir Sample. JSON şablonu kullanacağız. Kullandığımız. JSON dosyası şu şekildedir: [Helloımagetemplateforsigfromsig. JSON](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/2_Creating_a_Custom_Linux_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromSIG.json). 


## <a name="register-the-features"></a>Özellikleri kaydetme
Önizleme sırasında Azure Image Builder 'ı kullanmak için yeni özelliği kaydetmeniz gerekir.

```azurecli-interactive
az feature register --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview
```

Özellik kaydının durumunu denetleyin.

```azurecli-interactive
az feature show --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview | grep state
```

Kaydınızı denetleyin.

```azurecli-interactive
az provider show -n Microsoft.VirtualMachineImages | grep registrationState

az provider show -n Microsoft.Storage | grep registrationState
```

Kayıtlı değilse, aşağıdakileri çalıştırın:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages

az provider register -n Microsoft.Storage
```


## <a name="set-variables-and-permissions"></a>Değişkenleri ve izinleri ayarla

Paylaşılan görüntü galerinizi oluşturmak için [görüntü oluştur ve paylaşılan görüntü galerisine dağıt](image-builder-gallery.md) ' ı kullandıysanız, ihtiyacımız olan değişkenlerden bazılarını zaten oluşturdunuz. Aksi takdirde, lütfen bu örnek için kullanılacak bazı değişkenleri ayarlayın.

Önizleme için, görüntü Oluşturucu yalnızca kaynak yönetilen görüntüyle aynı kaynak grubunda özel görüntüler oluşturmayı destekleyecektir. Bu örnekteki kaynak grubu adını kaynak yönetilen yansımanız ile aynı kaynak grubu olacak şekilde güncelleştirin.


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

Abonelik KIMLIĞINIZ için bir değişken oluşturun. Bunu kullanarak `az account show | grep id`edinebilirsiniz.

```console
subscriptionID=<Subscription ID>
```

Güncelleştirmek istediğiniz görüntü sürümünü alın.

```azurecli
sigDefImgVersionId=$(az sig image-version list \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID --query [].'id' -o json | grep 0. | tr -d '"' | tr -d '[:space:]')
```


Zaten kendi paylaşılan görüntü galeriniz varsa ve önceki örneği izmediyseniz, kaynak grubuna erişmek için görüntü Oluşturucu için izinler atamanız gerekir, bu nedenle galeriye erişebilir.


```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup
```


## <a name="modify-helloimage-example"></a>Merhaba görüntü örneğini değiştirme
Burada. json dosyasını açarak kullanmak üzere olduğumuz örneği gözden geçirebilirsiniz: [Helloımagetemplateforsigfromsig. JSON](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/2_Creating_a_Custom_Linux_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromSIG.json) Ile birlikte [Görüntü Oluşturucu şablon başvurusu](image-builder-json.md). 


. JSON örneğini indirin ve değişkenlerinizi yapılandırın. 

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

Görüntü yapılandırmasını VM görüntü Oluşturucu hizmetine gönderme.

```azurecli-interactive
az resource create \
    --resource-group $sigResourceGroup \
    --properties @helloImageTemplateforSIGfromSIG.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforSIGfromSIG01
```

Görüntü derlemesini başlatın.

```azurecli-interactive
az resource invoke-action \
     --resource-group $sigResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateforSIGfromSIG01 \
     --action Run 
```

Sonraki adıma geçmeden önce görüntünün oluşturulup çoğaltılıncaya kadar bekleyin.


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

VM 'nin genel IP adresini kullanarak VM 'ye bir SSH bağlantısı oluşturun.

```console
ssh azureuser@<pubIp>
```

SSH bağlantınız kurulduunda görüntünün "günün Iletisi" ile özelleştirildiğini görmeniz gerekir.

```output
*******************************************************
**            This VM was built from the:            **
**      !! AZURE VM IMAGE BUILDER Custom Image !!    **
**         You have just been Customized :-)         **
*******************************************************
```

SSH `exit` bağlantısını kapatmak için yazın.

Ayrıca, galerinizin artık kullanabildiği görüntü sürümlerini de listeleyebilirsiniz.

```azurecli-interactive
az sig image-version list -g $sigResourceGroup -r $sigName -i $imageDefName -o table
```


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan. json dosyasının bileşenleri hakkında daha fazla bilgi edinmek için bkz. [Görüntü Oluşturucu şablonu başvurusu](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).