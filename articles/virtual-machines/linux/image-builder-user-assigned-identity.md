---
title: Sanal Makine görüntüsü oluşturun ve Azure Depolama'daki dosyalara erişmek için kullanıcı tarafından atanmış yönetilen bir kimlik kullanın (önizleme)
description: Azure Image Builder kullanarak, kullanıcı tarafından atanan yönetilen kimliği kullanarak Azure Depolama'da depolanan dosyalara erişebilen sanal makine görüntüsü oluşturun.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.subservice: imaging
manager: gwallace
ms.openlocfilehash: 27f4073efc8647d331faa14afbda0e15f92b8d50
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060741"
---
# <a name="create-an-image-and-use-a-user-assigned-managed-identity-to-access-files-in-azure-storage"></a>Azure Depolama'daki dosyalara erişmek için bir resim oluşturun ve kullanıcı tarafından atanmış yönetilen bir kimlik kullanın 

Azure Image Builder komut dosyalarını kullanmayı veya GitHub ve Azure depolama vb. gibi birden çok konumdan dosya kopyalamayı destekler. Bunları kullanmak için Azure Image Builder tarafından dışarıdan erişilebiliyor olmaları gerekir, ancak SAS Belirteçleri'ni kullanarak Azure Depolama lekelerini koruyabilirsiniz.

Bu makalede, dosyaları herkese açık hale getirmenize veya SAS belirteçlerini ayarlamanıza gerek kalmadan, hizmetin görüntü özelleştirme için Azure depolamasundaki dosyalara erişmek için [Kullanıcı tarafından atanmış yönetilen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) bir kimlik kullanacağı Azure VM Image Builder'ı kullanarak özelleştirilmiş bir görüntünün nasıl oluşturulacağı gösterilmektedir.

Aşağıdaki örnekte, biri özel görüntü için kullanılacak, diğeri komut dosyası dosyası dosyası içeren bir Azure Depolama Hesabı barındıracak iki kaynak grubu oluşturacaksınız. Bu, Resim Oluşturucu'nun dışında, farklı depolama hesaplarında yapı yapıları veya görüntü dosyaları oluşturabileceğiniz gerçek bir senaryoyu simüle eder. Kullanıcı tarafından atanmış bir kimlik oluşturur, ardından komut dosyası dosyasında okuma izinleri verirsiniz, ancak bu dosyaya genel erişim ayarlamazsınız. Daha sonra bu komut dosyasını depolama hesabından indirmek ve çalıştırmak için Shell özelleyicisini kullanırsınız.


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


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bazı bilgileri tekrar tekrar kullanarak bu bilgileri depolamak için bazı değişkenler oluşturacağız.


```console
# Image resource group name 
imageResourceGroup=aibmdimsi
# storage resource group
strResourceGroup=aibmdimsistor
# Location 
location=WestUS2
# name of the image to be created
imageName=aibCustLinuxImgMsi01
# image distribution metadata reference name
runOutputName=u1804ManImgMsiro
```

Abonelik kimliğiniz için bir değişken oluşturun. Bunu kullanarak `az account show | grep id`elde edebilirsiniz.

```console
subscriptionID=<Your subscription ID>
```

Hem görüntü hem de komut dosyası depolama alanı için kaynak grupları oluşturun.

```console
# create resource group for image template
az group create -n $imageResourceGroup -l $location
# create resource group for the script storage
az group create -n $strResourceGroup -l $location
```


Depolama alanını oluşturun ve örnek komut dosyasını GitHub'dan kopyalayın.

```azurecli-interactive
# script storage account
scriptStorageAcc=aibstorscript$(date +'%s')

# script container
scriptStorageAccContainer=scriptscont$(date +'%s')

# script url
scriptUrl=https://$scriptStorageAcc.blob.core.windows.net/$scriptStorageAccContainer/customizeScript.sh

# create storage account and blob in resource group
az storage account create -n $scriptStorageAcc -g $strResourceGroup -l $location --sku Standard_LRS

az storage container create -n $scriptStorageAccContainer --fail-on-exist --account-name $scriptStorageAcc

# copy in an example script from the GitHub repo 
az storage blob copy start \
    --destination-blob customizeScript.sh \
    --destination-container $scriptStorageAccContainer \
    --account-name $scriptStorageAcc \
    --source-uri https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/customizeScript.sh
```



Görüntü oluşturucuya görüntü kaynak grubunda kaynak oluşturmak için izin verin. Değer, `--assignee` Image Builder hizmetinin uygulama kayıt kimliğidir. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```


## <a name="create-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma

Komut dosyası depolama hesabı için kimlik oluşturun ve izinler atayın. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm#user-assigned-managed-identity)

```azurecli-interactive
# Create the user assigned identity 
identityName=aibBuiUserId$(date +'%s')
az identity create -g $imageResourceGroup -n $identityName
# assign the identity permissions to the storage account, so it can read the script blob
imgBuilderCliId=$(az identity show -g $imageResourceGroup -n $identityName | grep "clientId" | cut -c16- | tr -d '",')
az role assignment create \
    --assignee $imgBuilderCliId \
    --role "Storage Blob Data Reader" \
    --scope /subscriptions/$subscriptionID/resourceGroups/$strResourceGroup/providers/Microsoft.Storage/storageAccounts/$scriptStorageAcc/blobServices/default/containers/$scriptStorageAccContainer 
# create the user identity URI
imgBuilderId=/subscriptions/$subscriptionID/resourcegroups/$imageResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/$identityName
```


## <a name="modify-the-example"></a>Örneği değiştirme

.json örneğini indirin ve oluşturduğunuz değişkenlerle yapılandırın.

```console
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage/helloImageTemplateMsi.json -o helloImageTemplateMsi.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateMsi.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateMsi.json
sed -i -e "s/<region>/$location/g" helloImageTemplateMsi.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateMsi.json
sed -i -e "s%<scriptUrl>%$scriptUrl%g" helloImageTemplateMsi.json
sed -i -e "s%<imgBuilderId>%$imgBuilderId%g" helloImageTemplateMsi.json
sed -i -e "s%<runOutputName>%$runOutputName%g" helloImageTemplateMsi.json
```

## <a name="create-the-image"></a>Görüntü oluşturma

Görüntü yapılandırmasını Azure Image Builder hizmetine gönderin.

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateMsi.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateMsi01
```

Görüntü oluşturmayı başlatın.

```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateMsi01 \
     --action Run 
```

Yapının tamamlanmasını bekleyin. Bu işlem yaklaşık 15 dakika sürebilir.

## <a name="create-a-vm"></a>VM oluşturma

Görüntüden bir VM oluşturun. 

```azurecli
az vm create \
  --resource-group $imageResourceGroup \
  --name aibImgVm00 \
  --admin-username aibuser \
  --image $imageName \
  --location $location \
  --generate-ssh-keys
```

VM oluşturulduktan sonra, VM ile bir SSH oturumu başlatın.

```console
ssh aibuser@<publicIp>
```

SSH bağlantınız kurulur kurulmaz görüntünün Günün Mesajı ile özelleştirilmelerini görmelisiniz!

```output

*******************************************************
**            This VM was built from the:            **
**      !! AZURE VM IMAGE BUILDER Custom Image !!    **
**         You have just been Customized :-)         **
*******************************************************
```

## <a name="clean-up"></a>Temizleme

İşi bittiğinde, artık ihtiyaç duyulmadığında kaynakları silebilirsiniz.

```azurecli-interactive
az identity delete --ids $imgBuilderId
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateMsi01
az group delete -n $imageResourceGroup
az group delete -n $strResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Image Builder ile çalışırken herhangi bir sorun yaşıyorsanız, [Sorun Giderme'ye](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md?toc=%2fazure%2fvirtual-machines%context%2ftoc.json)bakın.