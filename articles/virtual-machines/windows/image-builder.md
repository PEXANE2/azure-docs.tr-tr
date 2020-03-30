---
title: Azure Image Builder ile Windows VM oluşturma (önizleme)
description: Azure Image Builder ile bir Windows VM oluşturun.
author: cynthn
ms.author: cynthn
ms.date: 07/31/2019
ms.topic: article
ms.service: virtual-machines-windows
manager: gwallace
ms.openlocfilehash: e82d82dac833f7455e3d83d7e11c0c57c4eea816
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80238796"
---
# <a name="preview-create-a-windows-vm-with-azure-image-builder"></a>Önizleme: Azure Image Builder ile Windows VM oluşturma

Bu makale, Azure VM Image Builder'ı kullanarak nasıl özelleştirilmiş bir Windows görüntüsü oluşturabileceğinizi göstermek içindir. Bu makaledeki örnek, görüntüyü özelleştirmek için [özelleştiriciler](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#properties-customize) kullanır:
- PowerShell (ScriptUri) - indirin ve bir [PowerShell komut dosyası](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/testPsScript.ps1)çalıştırın.
- Windows Yeniden Başlat - VM'yi yeniden başlatır.
- PowerShell (satır içi) - belirli bir komut çalıştırın. Bu örnekte, VM'de bir dizin `mkdir c:\\buildActions`oluşturur.
- Dosya - GitHub'daki bir dosyayı VM'ye kopyalayın. Bu örnek, [index.md](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/exampleArtifacts/buildArtifacts/index.html) VM'de `c:\buildArtifacts\index.html` index.md kopyalar.

Ayrıca bir `buildTimeoutInMinutes`. Varsayılan değer 240 dakikadır ve daha uzun çalışan yapılara izin vermek için yapı süresini artırabilirsiniz.

Görüntüyü yapılandırmak için bir örnek .json şablonu kullanıyor olacağız. Kullandığımız .json dosyası burada: [helloImageTemplateWin.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json). 


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

## <a name="set-variables"></a>Değişkenleri ayarlama

Bazı bilgileri tekrar tekrar kullanarak bu bilgileri depolamak için bazı değişkenler oluşturacağız.


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

Abonelik kimliğiniz için bir değişken oluşturun. Bunu kullanarak `az account show | grep id`elde edebilirsiniz.

```azurecli-interactive
subscriptionID=<Your subscription ID>
```
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Bu kaynak grubu, görüntü yapılandırma şablonu artmasını ve görüntüyü depolamak için kullanılır.


```azurecli-interactive
az group create -n $imageResourceGroup -l $location
```

## <a name="set-permissions-on-the-resource-group"></a>Kaynak grubunda izinleri ayarlama

Kaynak grubundaki görüntüyü oluşturmak için Görüntü Oluşturucuya 'katkıda bulunan' izni verin. Bu olmadan, görüntü oluşturma başarısız olur. 

Değer, `--assignee` Image Builder hizmetinin uygulama kayıt kimliğidir. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```


## <a name="download-the-image-configuration-template-example"></a>Resim yapılandırma şablonu örneğini indirin

Denemeniz için parametreli bir görüntü yapılandırma şablonu oluşturuldu. .json örneğini indirin ve daha önce ayarladığınız değişkenlerle yapılandırın.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json -o helloImageTemplateWin.json

sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateWin.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateWin.json
sed -i -e "s/<region>/$location/g" helloImageTemplateWin.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateWin.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateWin.json

```

Bu örneği, terminalde bir metin düzenleyicisi kullanarak `vi`değiştirebilirsiniz.

```azurecli-interactive
vi helloImageTemplateLinux.json
```

> [!NOTE]
> Kaynak görüntü için, her zaman bir sürüm `latest` [belirtmeniz](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#image-version-failure)gerekir, kullanamazsınız.
> Resmin dağıtıldığı kaynak grubunu ekler veya değiştirirseniz, izinlerin kaynak grubunda [ayarlanmasını](#set-permissions-on-the-resource-group) sağlamalısınız.
 
## <a name="create-the-image"></a>Görüntü oluşturma

Görüntü yapılandırmasını VM Image Builder hizmetine gönderme

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateWin.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
```

Tamamlandığında, bu konsola bir başarı iletisi geri `Image Builder Configuration Template` döndürür ve bir . `$imageResourceGroup` 'Gizli türleri göster'i etkinleştirirseniz, bu kaynağı Azure portalındaki kaynak grubunda görebilirsiniz.

Arka planda, Image Builder aboneliğinizde bir hazırlama kaynak grubu da oluşturur. Bu kaynak grubu görüntü oluşturmak için kullanılır. Bu biçimde olacaktır:`IT_<DestinationResourceGroup>_<TemplateName>`

> [!Note]
> Evreleme kaynak grubunu doğrudan silmemelisiniz. Önce resim şablonu artifakı silin, bu evreleme kaynak grubunun silinmesine neden olur.

Hizmet, görüntü yapılandırma şablonu gönderimi sırasında bir hata bildiriyorsa:
-  Bu [sorun giderme](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#template-submission-errors--troubleshooting) adımlarını gözden geçirin. 
- Gönderiyi yeniden denemeden önce aşağıdaki parçacığı kullanarak şablonu silmeniz gerekir.

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

## <a name="start-the-image-build"></a>Görüntü oluşturmayı başlatın
Az kaynak çağırma [eylemi](/cli/azure/resource#az-resource-invoke-action)kullanarak görüntü oluşturma işlemini başlatın.

```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateWin01 \
     --action Run 
```

Yapı tamamlanana kadar bekleyin. Bu işlem yaklaşık 15 dakika sürebilir.

Herhangi bir hatayla karşılaşırsanız, lütfen bu [sorun giderme](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#image-build-errors--troubleshooting) adımlarını gözden geçirin.


## <a name="create-the-vm"></a>Sanal makine oluşturma

Oluşturduğunuz görüntüyü kullanarak VM'yi oluşturun. Parola `aibuser` * \<>* VM'deki parolanızla değiştirin.

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

VM'yi oluştururken belirlediğiniz kullanıcı adı ve parolayı kullanarak VM'ye Uzak Masaüstü bağlantısı oluşturun. VM'nin içinde cmd istemi ni açın ve yazın:

```console
dir c:\
```

Görüntü özelleştirme sırasında oluşturulan bu iki dizingörmeniz gerekir:
- buildActions
- buildArtifacts

## <a name="clean-up"></a>Temizleme

İşi bittiğinde, kaynakları silin.

### <a name="delete-the-image-builder-template"></a>Resim oluşturucu şablonu silme

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
```

### <a name="delete-the-image-resource-group"></a>Resim kaynak grubunu silme

```azurecli-interactive
az group delete -n $imageResourceGroup
```


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan .json dosyasının bileşenleri hakkında daha fazla bilgi edinmek için [Bkz. Resim oluşturucu şablonu başvurusu.](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
