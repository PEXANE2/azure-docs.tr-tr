---
title: Azure Image Builder ile Windows VM oluşturma (Önizleme)
description: Azure görüntü Oluşturucu ile bir Windows sanal makinesi oluşturun.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-windows
manager: gwallace
ms.openlocfilehash: 103ec3c9ee4bd6b3b83408b0f9958a22d3a22ae1
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68261055"
---
# <a name="preview-create-a-windows-vm-with-azure-image-builder"></a>Önizleme: Azure Image Builder ile Windows VM oluşturma

Bu makale, Azure VM görüntü Oluşturucusu 'nu kullanarak özelleştirilmiş bir Windows görüntüsünü nasıl oluşturabileceğiniz hakkında sizi gösterir. Bu makaledeki örnek, görüntüyü özelleştirmek için üç farklı [özelleştiriciler](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#properties-customize) kullanır:
- PowerShell (ScriptUri)- [PowerShell betiğini](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/testPsScript.ps1)indirip çalıştırın.
- Windows yeniden başlatma-VM 'yi yeniden başlatır.
- PowerShell (satır içi)-belirli bir komut çalıştırın. Bu örnekte, kullanarak `mkdir c:\\buildActions`VM üzerinde bir dizin oluşturur.
- Dosya-GitHub 'dan sanal makineye dosya kopyalama. Bu örnek, [index.MD](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/exampleArtifacts/buildArtifacts/index.html) `c:\buildArtifacts\index.html` öğesini sanal makineye kopyalar.

Görüntüyü yapılandırmak için bir Sample. JSON şablonu kullanacağız. Kullandığımız. JSON dosyası şurada: [Helloımagetemplatewin. JSON](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json). 


> [!IMPORTANT]
> Azure görüntü Oluşturucu Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


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

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bazı bilgi parçalarını sürekli olarak kullanacağız. bu nedenle, bu bilgileri depolamak için bazı değişkenler oluşturacağız.

```azurecli-interactive
# Resource group name - we are using myImageBuilderRG in this example
imageResourceGroup=myWinImgBuilderRG
# Region location 
location=WestUS2
# Name for the image 
imageName=myWinBuilderImage
# Run output name
runOutputName=aibWindows
# name of the image to be created
imageName=aibWinImage
```

Abonelik KIMLIĞINIZ için bir değişken oluşturun. Bunu kullanarak `az account show | grep id`edinebilirsiniz.

```azurecli-interactive
subscriptionID=<Your subscription ID>
```

Kaynak grubunu oluşturun.

```azurecli-interactive
az group create -n $imageResourceGroup -l $location
```

Bu kaynak grubundaki kaynakları oluşturmak için görüntü Oluşturucu iznini verin. `--assignee` Değer, görüntü Oluşturucu hizmeti için uygulama kayıt kimliğidir. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```


## <a name="download-the-json-example"></a>. JSON örneğini indirin

Örnek. json dosyasını indirin ve oluşturduğunuz değişkenlerle yapılandırın.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json -o helloImageTemplateWin.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateWin.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateWin.json
sed -i -e "s/<region>/$location/g" helloImageTemplateWin.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateWin.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateWin.json

```

## <a name="create-the-image"></a>Görüntü oluşturma

Görüntü yapılandırmasını VM görüntü Oluşturucu hizmetine gönderme

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateWin.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
```

Görüntü derlemesini başlatın.

```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateWin01 \
     --action Run 
```

Yapı tamamlanana kadar bekleyin. Bu, yaklaşık 15 dakika sürebilir.

## <a name="create-the-vm"></a>Sanal makine oluşturma

Oluşturduğunuz görüntüyü kullanarak VM 'yi oluşturun. *\<Parola >* , `aibuser` VM üzerindeki için kendi parolanızla değiştirin.

```azurecli-interactive
az vm create \
  --resource-group $imageResourceGroup \
  --name aibImgWinVm00 \
  --admin-username aibuser \
  --admin-password <password> \
  --image $imageName \
  --location $location
```

## <a name="verify-the-customization"></a>Özelleştirmeyi doğrulama

VM 'yi oluştururken ayarladığınız Kullanıcı adını ve parolayı kullanarak VM 'ye bir Uzak Masaüstü bağlantısı oluşturun. VM 'nin içinde bir komut istemi açın ve şunu yazın:

```console
dir c:\
```

Görüntü özelleştirmesi sırasında oluşturulan bu iki dizini görmeniz gerekir:
- Builkactions
- Buildartifdavranır

## <a name="clean-up"></a>Temizleme

İşiniz bittiğinde, kaynakları silin.

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
az group delete -n $imageResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan. json dosyasının bileşenleri hakkında daha fazla bilgi edinmek için bkz. [Görüntü Oluşturucu şablonu başvurusu](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

