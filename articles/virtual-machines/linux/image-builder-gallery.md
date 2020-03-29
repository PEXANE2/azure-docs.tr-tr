---
title: Linux VM'leri için bir resim galerisiyle Azure Image Builder'ı kullanın (önizleme)
description: Azure Image Builder ve Paylaşılan Resim Galerisi ile Linux VM görüntüleri oluşturun.
author: cynthn
ms.author: cynthn
ms.date: 04/20/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.subservice: imaging
ms.openlocfilehash: bf1dca61ec6b39e52d4f76c1c77cd3def6973ab8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78945002"
---
# <a name="preview-create-a-linux-image-and-distribute-it-to-a-shared-image-gallery"></a>Önizleme: Bir Linux görüntüsü oluşturun ve Paylaşılan Resim Galerisi'ne dağıtın 

Bu makalede, [Paylaşılan Resim Galerisi'nde](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries)bir resim sürümü oluşturmak ve ardından görüntüyü genel olarak dağıtmak için Azure Image Builder ve Azure CLI'yi nasıl kullanabileceğinizi gösterir. Bunu [Azure PowerShell'i](../windows/image-builder-gallery.md)kullanarak da yapabilirsiniz.


Görüntüyü yapılandırmak için bir örnek .json şablonu kullanıyor olacağız. Kullandığımız .json dosyası burada: [helloImageTemplateforSIG.json](https://github.com/danielsollondon/azvmimagebuilder/blob/master/quickquickstarts/1_Creating_a_Custom_Linux_Shared_Image_Gallery_Image/helloImageTemplateforSIG.json). 

Görüntüyü Paylaşılan Resim Galerisi'ne dağıtmak için şablon, `distribute` şablonun bölümü için değer olarak [sharedImage](image-builder-json.md#distribute-sharedimage) kullanır.

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
```

Kayıtlı demiyorlarsa, aşağıdakileri çalıştırın:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages

az provider register -n Microsoft.Storage
```

## <a name="set-variables-and-permissions"></a>Değişkenleri ve izinleri ayarlama 

Bazı bilgileri tekrar tekrar kullanarak bu bilgileri depolamak için bazı değişkenler oluşturacağız.

Önizleme için, görüntü oluşturucu yalnızca kaynak yönetilen görüntüyle aynı Kaynak Grubunda özel görüntüler oluşturmayı destekler. Kaynak yönetilen görüntünüzle aynı kaynak grubu olacak şekilde bu örnekteki kaynak grubu adını güncelleştirin.

```azurecli-interactive
# Resource group name - we are using ibLinuxGalleryRG in this example
sigResourceGroup=ibLinuxGalleryRG
# Datacenter location - we are using West US 2 in this example
location=westus2
# Additional region to replicate the image to - we are using East US in this example
additionalregion=eastus
# name of the shared image gallery - in this example we are using myGallery
sigName=myIbGallery
# name of the image definition to be created - in this example we are using myImageDef
imageDefName=myIbImageDef
# image distribution metadata reference name
runOutputName=aibLinuxSIG
```

Abonelik kimliğiniz için bir değişken oluşturun. Bunu kullanarak `az account show | grep id`elde edebilirsiniz.

```azurecli-interactive
subscriptionID=<Subscription ID>
```

Kaynak grubunu oluşturun.

```azurecli-interactive
az group create -n $sigResourceGroup -l $location
```


Azure Image Builder'a bu kaynak grubunda kaynak oluşturması için izin verin. Değer, `--assignee` Image Builder hizmetinin uygulama kayıt kimliğidir. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup
```





## <a name="create-an-image-definition-and-gallery"></a>Resim tanımı ve galeri oluşturma

Paylaşılan bir resim galerisiyle Image Builder'ı kullanmak için varolan bir resim galerisine ve resim tanımına sahip olmanız gerekir. Image Builder sizin için resim galerisi ve resim tanımı oluşturmaz.

Zaten kullanmak için bir galeri ve resim tanımı yoksa, bunları oluşturarak başlayın. İlk olarak, bir resim galerisi oluşturun.

```azurecli-interactive
az sig create \
    -g $sigResourceGroup \
    --gallery-name $sigName
```

Ardından, bir resim tanımı oluşturun.

```azurecli-interactive
az sig image-definition create \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --publisher myIbPublisher \
   --offer myOffer \
   --sku 18.04-LTS \
   --os-type Linux
```


## <a name="download-and-configure-the-json"></a>.json'u indirin ve yapılandırın

.json şablonunu indirin ve değişkenlerinizle yapılandırın.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Linux_Shared_Image_Gallery_Image/helloImageTemplateforSIG.json -o helloImageTemplateforSIG.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateforSIG.json
sed -i -e "s/<rgName>/$sigResourceGroup/g" helloImageTemplateforSIG.json
sed -i -e "s/<imageDefName>/$imageDefName/g" helloImageTemplateforSIG.json
sed -i -e "s/<sharedImageGalName>/$sigName/g" helloImageTemplateforSIG.json
sed -i -e "s/<region1>/$location/g" helloImageTemplateforSIG.json
sed -i -e "s/<region2>/$additionalregion/g" helloImageTemplateforSIG.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateforSIG.json
```

## <a name="create-the-image-version"></a>Resim sürümünü oluşturma

Bu sonraki bölümde galeride görüntü sürümü oluşturacaktır. 

Görüntü yapılandırmasını Azure Image Builder hizmetine gönderin.

```azurecli-interactive
az resource create \
    --resource-group $sigResourceGroup \
    --properties @helloImageTemplateforSIG.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforSIG01
```

Görüntü oluşturmayı başlatın.

```azurecli-interactive
az resource invoke-action \
     --resource-group $sigResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateforSIG01 \
     --action Run 
```

Görüntüyü oluşturma ve her iki bölgeye çoğaltmak biraz zaman alabilir. VM oluşturmaya geçmeden önce bu bölümün bitmesini bekleyin.


## <a name="create-the-vm"></a>Sanal makine oluşturma

Azure Image Builder tarafından oluşturulan resim sürümünden bir VM oluşturun.

```azurecli-interactive
az vm create \
  --resource-group $sigResourceGroup \
  --name myAibGalleryVM \
  --admin-username aibuser \
  --location $location \
  --image "/subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup/providers/Microsoft.Compute/galleries/$sigName/images/$imageDefName/versions/latest" \
  --generate-ssh-keys
```

VM ile SSH bağlantısı kurun.

```azurecli-interactive
ssh aibuser@<publicIpAddress>
```

SSH bağlantınız kurulur kurulmaz *görüntünün Günün Mesajı* ile özelleştirilmelerini görmelisiniz!

```console
*******************************************************
**            This VM was built from the:            **
**      !! AZURE VM IMAGE BUILDER Custom Image !!    **
**         You have just been Customized :-)         **
*******************************************************
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Şimdi aynı görüntünün yeni bir sürümünü oluşturmak için resim sürümünü yeniden özelleştirmeyi denemek istiyorsanız, sonraki adımları atlayın ve [başka bir resim sürümü oluşturmak için Azure Image Builder'ı kullanın'](image-builder-gallery-update-image-version.md)a gidin.


Bu, oluşturulan tüm diğer kaynak dosyalarıyla birlikte oluşturulan görüntüyü siler. Kaynakları silmeden önce bu dağıtımı bitirdiğinden emin olun.

Resim galerisi kaynaklarını silerken, bunları oluşturmak için kullanılan resim tanımını silmeden önce tüm resim sürümlerini silmeniz gerekir. Bir galeriyi silmek için öncelikle galerideki tüm resim tanımlarını silmiş olmanız gerekir.

Resim oluşturucu şablonu silin.

```azurecli-interactive
az resource delete \
    --resource-group $sigResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforSIG01
```

Görüntü oluşturucu tarafından oluşturulan görüntü sürümü `0.`alın, bu her zaman ile başlar , ve sonra görüntü sürümünü silmek

```azurecli-interactive
sigDefImgVersion=$(az sig image-version list \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID --query [].'name' -o json | grep 0. | tr -d '"')
az sig image-version delete \
   -g $sigResourceGroup \
   --gallery-image-version $sigDefImgVersion \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID
```   


Resim tanımını silin.

```azurecli-interactive
az sig image-definition delete \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID
```

Galeriyi silin.

```azurecli-interactive
az sig delete -r $sigName -g $sigResourceGroup
```

Kaynak grubunu silin.

```azurecli-interactive
az group delete -n $sigResourceGroup -y
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Paylaşılan Resim Galerileri](shared-image-galleries.md)hakkında daha fazla bilgi edinin.