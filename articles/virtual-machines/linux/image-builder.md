---
title: Azure Image Builder ile Bir Linux VM oluşturma (önizleme)
description: Azure Image Builder ile bir Linux VM oluşturun.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.subservice: imaging
ms.openlocfilehash: 0d36d7db4d85ece8de77040925c535305951562b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066677"
---
# <a name="preview-create-a-linux-vm-with-azure-image-builder"></a>Önizleme: Azure Image Builder ile Linux VM oluşturun

Bu makalede, Azure Image Builder ve Azure CLI'yi kullanarak nasıl özelleştirilmiş bir Linux görüntüsü oluşturabileceğiniz gösterilmektedir. Bu makaledeki örnek, görüntüyü özelleştirmek için üç farklı [özelleştirici](image-builder-json.md#properties-customize) kullanır:

- Shell (ScriptUri) - indirir ve bir [kabuk komut dosyası](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/customizeScript.sh)çalışır.
- Shell (satır içi) - belirli komutları çalıştırın. Bu örnekte, satır satır komutları bir dizin oluşturma ve işletim sistemi güncelleştirmeiçerir.
- Dosya - [GitHub'daki](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/exampleArtifacts/buildArtifacts/index.html) bir dosyayı VM'deki bir dizine kopyalar.

Ayrıca bir `buildTimeoutInMinutes`. Varsayılan değer 240 dakikadır ve daha uzun çalışan yapılara izin vermek için yapı süresini artırabilirsiniz.

Görüntüyü yapılandırmak için bir örnek .json şablonu kullanıyor olacağız. Kullandığımız .json dosyası burada: [helloImageTemplateLinux.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Linux_Managed_Image/helloImageTemplateLinux.json). 

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

## <a name="setup-example-variables"></a>Kurulum örnek değişkenleri

Bazı bilgileri tekrar tekrar kullanarak bu bilgileri depolamak için bazı değişkenler oluşturacağız.


```console
# Resource group name - we are using myImageBuilderRG in this example
imageResourceGroup=myImageBuilerRGLinux
# Datacenter location - we are using West US 2 in this example
location=WestUS2
# Name for the image - we are using myBuilderImage in this example
imageName=myBuilderImage
# Run output name
runOutputName=aibLinux
```

Abonelik kimliğiniz için bir değişken oluşturun. Bunu kullanarak `az account show | grep id`elde edebilirsiniz.

```console
subscriptionID=<Your subscription ID>
```

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturun.
Bu, görüntü yapılandırma şablonu artifakı ve görüntüyü depolamak için kullanılır.

```azurecli-interactive
az group create -n $imageResourceGroup -l $location
```

## <a name="set-permissions-on-the-resource-group"></a>Kaynak grubunda izinleri ayarlama
Kaynak grubundaki görüntüyü oluşturmak için Görüntü Oluşturucuya 'katkıda bulunan' izni verin. Uygun izinler olmadan, görüntü oluşturma başarısız olur. 

Değer, `--assignee` Image Builder hizmetinin uygulama kayıt kimliğidir. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```

## <a name="download-the-template-example"></a>Şablon örneğini indirin

Kullanmanız için parametreli örnek görüntü yapılandırma şablonu oluşturuldu. Örnek .json dosyasını indirin ve daha önce ayarladığınız değişkenlerle yapılandırın.

```bash
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Linux_Managed_Image/helloImageTemplateLinux.json -o helloImageTemplateLinux.json

sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateLinux.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateLinux.json
sed -i -e "s/<region>/$location/g" helloImageTemplateLinux.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateLinux.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateLinux.json
```

Bu örneği gerektiği gibi .json olarak değiştirebilirsiniz. Örneğin, daha uzun çalışan `buildTimeoutInMinutes` yapılara izin verme değerini artırabilirsiniz. Bulut Bulut'ta dosyayı ' gibi `vi`bir metin düzenleyicisi kullanarak edinebilirsiniz.

```bash
vi helloImageTemplateLinux.json
```

> [!NOTE]
> Kaynak görüntü için, her zaman bir sürüm `latest` [belirtmeniz](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#image-version-failure)gerekir, kullanamazsınız.
>
> Görüntünün dağıtıldığı kaynak grubunu ekler veya değiştirirseniz, [izinlerin kaynak grubu için ayarlandığınızdan](#set-permissions-on-the-resource-group)emin olmanız gerekir.


## <a name="submit-the-image-configuration"></a>Görüntü yapılandırmasını gönderme
Görüntü yapılandırmasını VM Image Builder hizmetine gönderme

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateLinux.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

Başarıyla tamamlanırsa, bir başarı iletisi döndürür ve $imageResourceGroup bir görüntü oluşturucu yapılandırma şablonu yapı oluşturur. 'Gizli türleri göster'i etkinleştirirseniz, portaldaki kaynak grubunu görebilirsiniz.

Ayrıca, arka planda, Image Builder aboneliğinizde bir hazırlama kaynak grubu oluşturur. Image Builder, görüntü oluşturma için hazırlama kaynak grubunu kullanır. Kaynak grubunun adı bu biçimde olacaktır: `IT_<DestinationResourceGroup>_<TemplateName>`.

> [!IMPORTANT]
> Evreleme kaynak grubunu doğrudan silmeyin. Resim şablonu artifakını silerseniz, hazırlama kaynak grubunu otomatik olarak siler. Daha fazla bilgi için, bu makalenin sonundaki [Temizleme](#clean-up) bölümüne bakın.

Hizmet, görüntü yapılandırma şablonu gönderimi sırasında bir hata bildiriyorsa, [sorun giderme](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#template-submission-errors--troubleshooting) adımlarını görün. Ayrıca, yapıyı yeniden göndermeye çalışmadan önce şablonu silmeniz gerekir. Şablonu silmek için:

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

## <a name="start-the-image-build"></a>Görüntü oluşturmayı başlatın

Görüntü oluşturmayı başlatın.


```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateLinux01 \
     --action Run 
```

Yapı tamamlanana kadar bekleyin, bu örnek için, 10-15 dakika sürebilir.

Herhangi bir hatayla karşılaşırsanız, lütfen bu [sorun giderme](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#image-build-errors--troubleshooting) adımlarını gözden geçirin.


## <a name="create-the-vm"></a>Sanal makine oluşturma

Oluşturduğunuz görüntüyü kullanarak VM'yi oluşturun.

```azurecli-interactive
az vm create \
  --resource-group $imageResourceGroup \
  --name myVM \
  --admin-username azureuser \
  --image $imageName \
  --location $location \
  --generate-ssh-keys
```

IP adresini VM oluşturma çıktısından alın ve SSH'den VM'ye kullanın.

```bash
ssh azureuser@<pubIp>
```

SSH bağlantınız kurulur kurulmaz görüntünün Günün Mesajı ile özelleştirilmelerini görmelisiniz!

```output

*******************************************************
**            This VM was built from the:            **
**      !! AZURE VM IMAGE BUILDER Custom Image !!    **
**         You have just been Customized :-)         **
*******************************************************
```

SSH bağlantısını kapatmak için yaptığınız da yazın. `exit`

## <a name="check-the-source"></a>Kaynağı kontrol edin

Görüntü Oluşturucu Şablonu'nda, 'Özellikler'de kaynak görüntüyü, çalıştırdığı özelleştirme komut dosyasını ve nerede dağıtıldığını görürsünüz.

```bash
cat helloImageTemplateLinux.json
```

Bu .json dosyası hakkında daha ayrıntılı bilgi için [Bkz. Resim oluşturucu şablonu başvurusu](image-builder-json.md)

## <a name="clean-up"></a>Temizleme

İşi bittiğinde, kaynakları silebilirsiniz.

Resim oluşturucu şablonu silin.

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

Resim kaynak grubunu silin.

```azurecli
az group delete -n $imageResourceGroup
```


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan .json dosyasının bileşenleri hakkında daha fazla bilgi edinmek için [Bkz. Resim Oluşturucu şablonu başvurusu.](image-builder-json.md)
