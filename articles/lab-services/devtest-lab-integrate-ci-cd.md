---
title: Azure DevTest Laboratuvarlarını Azure Ardışık Hatlarınıza Entegre Edin
description: Azure DevTest Laboratuvarlarını Azure Ardışık Hatları sürekli tümleştirme ve teslim boru hattınıza nasıl entegre edebilirsiniz öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: a26df85e-2a00-462b-aac1-dd3539532569
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: 9604da5252254120ac7bd3fca3f0cc97324aef92
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76293224"
---
# <a name="integrate-azure-devtest-labs-into-your-azure-pipelines-cicd-pipeline"></a>Azure DevTest Laboratuvarlarını Azure Ardışık Hatları CI/CD ardışık sisteminize entegre edin

Azure Ardışık Hatlar'ınızı sürekli tümleştirme ve sürekli teslim (CI/CD) oluşturma ve serbest bırakma altyapılarını Azure DevTest Labs ile entegre etmek için *Azure DevTest Labs Görevleri* uzantısını kullanabilirsiniz. Uzantı, aşağıdakiler dahil olmak üzere çeşitli görevler yükler: 

- Sanal makine (VM) oluşturma
- VM’den özel görüntü oluşturma
- VM silme 

Bu görevler, örneğin, belirli bir test görevi için *altın bir görüntü* VM'yi hızla dağıtmayı ve test tamamlandığında VM'yi silmeyi kolaylaştırır.

Bu makalede, bir VM oluşturmak ve dağıtmak, özel bir resim oluşturmak ve sonra VM'yi tek bir sürüm ardışık işlem alanı olarak silmek için Azure DevTest Labs Görevleri'nin nasıl kullanılacağı gösterilmektedir. Normalde görevleri kendi özel yapı, test ve dağıtım ardışık hatlarınızda tek tek gerçekleştirirsiniz.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="prerequisites"></a>Ön koşullar

- [Azure DevOps](https://dev.azure.com) kuruluşunuza kaydolun veya oturum açın ve kuruluşta [bir proje oluşturun.](/vsts/organizations/projects/create-project) 
  
- Visual Studio Marketplace'ten Azure DevTest Labs Görevleri uzantısını yükleyin.
  
  1. Azure [DevTest Labs Görevleri'ne](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks)gidin.
  1. **Ücretsiz alın'ı**seçin.
  1. Açılan dosyadan Azure DevOps kuruluşunuzu seçin ve **Yükle'yi**seçin. 
  
## <a name="create-the-template-to-build-an-azure-vm"></a>Azure VM oluşturmak için şablonu oluşturma 

Bu bölümde, isteğe bağlı olarak bir Azure VM oluşturmak için kullandığınız Azure Kaynak Yöneticisi şablonu nasıl oluşturulabileceğiniz açıklanmaktadır.

1. Aboneliğinizde Kaynak Yöneticisi şablonu oluşturmak için [Kaynak Yöneticisi şablonunu kullan](devtest-lab-use-resource-manager-template.md)yordamını izleyin.
   
1. Kaynak Yöneticisi şablonu oluşturmadan önce, VM oluşturmanın bir parçası olarak [WinRM artifakı](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-winrm) ekleyin. Hedef Makinelerdeki *Azure Dosya Kopyalama* ve *PowerShell* gibi dağıtım görevlerinin WinRM erişimine ihtiyacı vardır. WinRM artifakı, VM'nin tam nitelikli etki alanı adı (FQDN) olması gereken bir parametre olarak bir ana ad gerektirir. 
   
   > [!NOTE]
   > Paylaşılan bir IP adresiyle WinRM kullandığınızda, WinRM bağlantı noktasına harici bir bağlantı noktasını eşlemek için bir NAT kuralı eklemeniz gerekir. Ortak bir IP adresiyle VM oluşturursanız NAT kuralına gerek yoktur.
   
   
1. Şablonu *CreateVMTemplate.json*adlı bir dosya olarak bilgisayarınıza kaydedin.
   
1. Şablonu kaynak kontrol sisteminize iade edin.

## <a name="create-a-script-to-get-vm-properties"></a>VM özelliklerini almak için komut dosyası oluşturma

Sürüm ardışık alanında Hedef Makinelerde *Azure Dosya Kopyalama* veya *PowerShell* gibi görev adımlarını çalıştırdığınızda, aşağıdaki komut dosyası bir uygulamayı VM'ye dağıtmak için gereken değerleri toplar. Uygulamanızı bir Azure VM'ye dağıtmak için normalde bu görevleri kullanırsınız. Görevler VM kaynak grubu adı, IP adresi ve FQDN gibi değerler gerektirir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Komut dosyası dosyasını oluşturmak için:

1. Bir metin düzenleyicisi açın ve aşağıdaki komut dosyasını içine yapıştırın.
   
   ```powershell
   Param( [string] $labVmId)

   $labVmComputeId = (Get-AzResource -Id $labVmId).Properties.ComputeId

   # Get lab VM resource group name
   $labVmRgName = (Get-AzResource -Id $labVmComputeId).ResourceGroupName

   # Get the lab VM Name
   $labVmName = (Get-AzResource -Id $labVmId).Name

   # Get lab VM public IP address
   $labVMIpAddress = (Get-AzPublicIpAddress -ResourceGroupName $labVmRgName
                   -Name $labVmName).IpAddress

   # Get lab VM FQDN
   $labVMFqdn = (Get-AzPublicIpAddress -ResourceGroupName $labVmRgName
              -Name $labVmName).DnsSettings.Fqdn

   # Set a variable labVmRgName to store the lab VM resource group name
   Write-Host "##vso[task.setvariable variable=labVmRgName;]$labVmRgName"

   # Set a variable labVMIpAddress to store the lab VM Ip address
   Write-Host "##vso[task.setvariable variable=labVMIpAddress;]$labVMIpAddress"

   # Set a variable labVMFqdn to store the lab VM FQDN name
   Write-Host "##vso[task.setvariable variable=labVMFqdn;]$labVMFqdn"
   ```

1. *GetLabVMParams.ps1*gibi bir adla dosyayı kaydedin ve kaynak kontrol sisteminize iade edin. 

## <a name="create-a-release-pipeline-in-azure-pipelines"></a>Azure Pipelines’da yayın işlem hattı oluşturma

Yeni bir sürüm ardışık hattı oluşturmak için:

1. Azure DevOps proje sayfanızdan, sol gezintiden **Boru Hattı** > **Sürümleri'ni** seçin.
1. **Yeni İşlem Hattı**’nı seçin.
1. **Şablon seç'in**altında, aşağı kaydırın ve **Boş iş'i**seçin ve ardından **Uygula'yı**seçin.

### <a name="add-and-set-variables"></a>Değişkenleri ekleme ve ayarlama

Azure portalında Kaynak Yöneticisi şablonu oluşturduğunuzda, ardışık iş aksı görevleri VM'ye atadığınız değerleri kullanır. 

Değerler için değişken eklemek için: 

1. Ardışık hatlar sayfasında **Değişkenler** sekmesini seçin.
   
1. Her değişken için **Ekle'yi** seçin ve adı ve değeri girin:
   
   |Adı|Değer|
   |---|---|
   |*vmName*|Kaynak Yöneticisi şablonuna atadığınız VM adı|
   |*Username*|VM'ye erişmek için kullanıcı adı|
   |*parola*|Kullanıcı adı için parola. Parolayı gizlemek ve güvenli hale getirmek için kilit simgesini seçin.

### <a name="create-a-devtest-labs-vm"></a>DevTest Labs VM oluşturma

Bir sonraki adım, gelecekteki dağıtımlar için kullanılacak altın görüntü VM'yi oluşturmaktır. *Azure DevTest Labs Oluşturma VM* görevini kullanarak Azure DevTest Labs örneğiniz içinde VM oluşturursunuz.

1. Sürüm ardışık **etki hattı pipeline** sekmesinde, Aşama **1'den** **Aşama'ya**görünüm **+** eki görevlerdeki köprü bağlantılı metni seçin ve ardından **Aracı işinin**yanındaki artı işaretini seçin. 
   
1. **Görevler Ekle'nin**altında, **Azure Devtest Labs'ı**seçin VM oluştur ve **Ekle'yi**seçin. 
   
1. Sol bölmede **Azure DevTest Labs VM oluştur'u** seçin. 

1. Sağ bölmede, formu aşağıda gösterildiği gibi doldurun:
   
   |Alan|Değer|
   |---|---|
   |**Azure RM Aboneliği**|Açılan açılır yolda **Kullanılabilir Azure Hizmet Bağlantılarından** veya Kullanılabilir Azure **Aboneliklerinden** bir hizmet bağlantısı veya abonelik seçin ve gerekirse **Yetkilendirme'yi** seçin.<br /><br />**Not:** Azure aboneliğinizde daha kısıtlı izinbağlantısı oluşturma hakkında bilgi için [Azure Kaynak Yöneticisi hizmeti bitiş noktası'na](/azure/devops/pipelines/library/service-endpoints#sep-azure-resource-manager)bakın.|
   |**Laboratuvar Adı**|Laboratuvar VM'sinin oluşturulacağı varolan bir laboratuvarın adını seçin.|
   |**Şablon Adı**|Kaynak kod deponuza kaydettiğiniz şablon dosyasının tam yolunu ve adını girin. Örneğin, yolu basitleştirmek için yerleşik özellikleri kullanabilirsiniz:<br /><br />`$(System.DefaultWorkingDirectory)/Templates/CreateVMTemplate.json`|
   |**Şablon Parametreleri**|Daha önce tanımladığınız değişkenlerin parametrelerini girin:<br /><br />`-newVMName '$(vmName)' -userName '$(userName)' -password (ConvertTo-SecureString -String '$(password)' -AsPlainText -Force)`|
   |**Çıkış Değişkenleri** > **Lab VM ID**|Oluşturulan laboratuvar VM Kimliği için değişkeni girin. Varsayılan **labVMId**kullanıyorsanız, sonraki görevlerde değişkene *$(labVMId)* olarak başvurabilirsiniz.<br /><br />Varsayılan dan başka bir ad oluşturabilirsiniz, ancak sonraki görevlerde doğru adı kullanmayı unutmayın. Lab VM KIMLIĞINI aşağıdaki şekilde yazabilirsiniz:<br /><br />`/subscriptions/{subscription Id}/resourceGroups/{resource group Name}/providers/Microsoft.DevTestLab/labs/{lab name}/virtualMachines/{vmName}`|

### <a name="collect-the-details-of-the-devtest-labs-vm"></a>DevTest Labs VM ayrıntılarını toplayın

DevTest Labs VM'nin ayrıntılarını toplamak için daha önce oluşturduğunuz komut dosyasını çalıştırın. 

1. Sürüm ardışık **etki hattı pipeline** sekmesinde, Aşama **1'den** **Aşama'ya**görünüm **+** eki görevlerdeki köprü bağlantılı metni seçin ve ardından **Aracı işinin**yanındaki artı işaretini seçin. 
   
1. **Görevler Ekle'nin**altında **Azure PowerShell'i**seçin ve **Ekle'yi**seçin. 
   
1. **Azure PowerShell komut dosyası: FilePath** sol bölmede seçin. 
   
1. Sağ bölmede, formu aşağıda gösterildiği gibi doldurun:
   
   |Alan|Değer|
   |---|---|
   |**Azure Bağlantı Türü**|**Azure Kaynak Yöneticisi'ni**seçin.|
   |**Azure Aboneliği**|Servis bağlantınızı veya aboneliğinizi seçin.| 
   |**Komut Dosyası Türü**|**Komut Dosyası Dosya Yolu'nu**seçin.|
   |**Komut Dosyası Yolu**|Kaynak kodu deponuza kaydettiğiniz PowerShell komut dosyasının tam yolunu ve adını girin. Örneğin, yolu basitleştirmek için yerleşik özellikleri kullanabilirsiniz:<br /><br />`$(System.DefaultWorkingDirectory/Scripts/GetLabVMParams.ps1`|
   |**Komut Dosyası Bağımsız Değişkenleri**|Örneğin, önceki görevtarafından doldurulan *labVmId* değişkeninin adını girin:<br /><br />`-labVmId '$(labVMId)'`|

Komut dosyası gerekli değerleri toplar ve bunları sürüm ardışık alanı içindeki ortam değişkenlerinde saklar, böylece sonraki adımlarda bunlara kolayca başvurabilirsiniz.

### <a name="create-a-vm-image-from-the-devtest-labs-vm"></a>DevTest Labs VM'den VM görüntüsü oluşturma

Bir sonraki görev, Azure DevTest Labs örneğinde yeni dağıtılan VM'nin bir görüntüsünü oluşturmaktır. Daha sonra, bir dev görevi yürütmek veya bazı testler çalıştırmak istediğinizde isteğe bağlı Olarak VM kopyalarını oluşturmak için görüntüyü kullanabilirsiniz. 

1. Sürüm ardışık **etki hattı pipeline** sekmesinde, Aşama **1'den** **Aşama'ya**görünüm **+** eki görevlerdeki köprü bağlantılı metni seçin ve ardından **Aracı işinin**yanındaki artı işaretini seçin. 
   
1. **Görevler Ekle'nin**altında, **Azure DevTest Labs'ı**seçin Özel Resim Oluştur'u seçin ve **Ekle'yi**seçin. 
   
1. Görevi aşağıdaki gibi yapılandırın:
   
   |Alan|Değer|
   |---|---|
   |**Azure RM Aboneliği**|Servis bağlantınızı veya aboneliğinizi seçin.|
   |**Laboratuvar Adı**|Görüntünün oluşturulacağı varolan bir laboratuvarın adını seçin.|
   |**Özel Resim Adı**|Özel görüntü için bir ad girin.|
   |**Açıklama** (isteğe bağlı)|Daha sonra doğru görüntüyü seçmeyi kolaylaştırmak için bir açıklama girin.|
   |**Kaynak Lab VM** > **Kaynak Lab VM Kimliği**|LabVMId değişkeninin varsayılan adını değiştirdiyseniz, buraya girin. Varsayılan değer **$(labVMId)**'dir.|
   |**Çıktı Değişkenleri** > **Özel Resim Kimliği**|Gerekirse değişkenin varsayılan adını atabilirsiniz.|
   
### <a name="deploy-your-app-to-the-devtest-labs-vm-optional"></a>Uygulamanızı DevTest Labs VM'ye dağıtın (isteğe bağlı)

Uygulamanızı yeni DevTest Labs VM'ye dağıtmak için görevler ekleyebilirsiniz. Uygulamayı dağıtmak için genellikle kullandığınız görevler, Hedef Makinelerde *Azure Dosya Kopyalama* ve *PowerShell'dir.*

Bu görevlerin parametreleri için ihtiyacınız olan VM bilgileri, release pipeline içinde **labVmRgName**, **labVMIpAddress**ve **labVMFqdn** adlı üç yapılandırma değişkeninde depolanır. Yalnızca bir DevTest Labs VM ve özel bir görüntü oluşturma denemeyapmak istiyorsanız, bir uygulama dağıtmadan, bu adımı atlayabilirsiniz.

### <a name="delete-the-vm"></a>VM’yi silin

Son görev, Azure DevTest Labs örneğinde dağıttığınız VM'yi silmektir. Dev görevlerini yürüttükten veya dağıtılan VM'de gereksinim duyduğunuz testleri çalıştırdıktan sonra normalde VM'yi silersiniz. 

1. Sürüm ardışık **etki hattı pipeline** sekmesinde, Aşama **1'den** **Aşama'ya**görünüm **+** eki görevlerdeki köprü bağlantılı metni seçin ve ardından **Aracı işinin**yanındaki artı işaretini seçin. 
   
1. **Görevler Ekle'nin**altında, **Azure DevTest Labs'ı seçin VM'yi sil**ve **Ekle'yi**seçin. 
   
1. Görevi aşağıdaki gibi yapılandırın:
   
   - **Azure RM Aboneliği**altında servis bağlantınızı veya aboneliğinizi seçin. 
   - **Lab VM ID**için, LabVMId değişkeninin varsayılan adını değiştirdiyseniz, buraya girin. Varsayılan değer **$(labVMId)**'dir.
   
### <a name="save-the-release-pipeline"></a>Sürüm ardışık hattını kaydet

Yeni sürüm ardışık hattını kaydetmek için:

1. Sürüm ardışık lık sayfasında **Yeni sürüm ardışık adını** seçin ve ardışık hatlar için yeni bir ad girin. 
   
1. Sağ üstteki **Kaydet** simgesini seçin. 

## <a name="create-and-run-a-release"></a>Sürüm oluşturma ve çalıştırma

Yeni ardışık hattı kullanarak bir sürüm oluşturmak ve çalıştırmak için:

1. Sürüm ardışık alan sayfasında sağ üstkısımda **sürüm oluştur'u** seçin. 
   
1. **Artefaktlar**altında, en son yapıyı seçin ve sonra **Oluştur'u**seçin.
   
1. Her sürüm aşamasında, VM oluşturma, görüntü oluşturma ve VM silme işlemini görüntülemek için Azure portalındaki DevTest Labs örneğinizin görünümünü yenileyin.

İhtiyacınız olduğunda VM'ler oluşturmak için özel görüntüyü kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [Kaynak Yöneticisi şablonları ile çoklu VM ortamları](devtest-lab-create-environment-from-arm.md)oluşturmayı öğrenin.
- [Kamu DevTest Labs GitHub repo](https://github.com/Azure/azure-quickstart-templates)DevTest Labs otomasyon için daha hızlı başlat Kaynak Yöneticisi şablonları keşfedin.
- Gerekirse Azure [DevOps sorun giderme sayfasına](https://docs.microsoft.com/azure/devops/pipelines/troubleshooting) bakın.
 
