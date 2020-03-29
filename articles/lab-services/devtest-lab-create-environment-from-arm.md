---
title: Şablonlarla çoklu VM ortamları ve PaaS kaynakları oluşturun
description: Azure Kaynak Yöneticisi şablonundan Azure DevTest Labs'da çoklu VM ortamları ve PaaS kaynakları oluşturma yı öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: 1385b20847cf90c212a13591389dfb6cda08432a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169642"
---
# <a name="create-multi-vm-environments-and-paas-resources-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturma

Azure DevTest Labs ortamları, kullanıcıların karmaşık altyapıları laboratuvar sınırları içinde tutarlı bir şekilde kolayca dağıtmalarına olanak tanır. DevTest Labs'da kaynak kümeleri içeren ortamlar oluşturmak için [Azure Kaynak Yöneticisi şablonlarını](../azure-resource-manager/templates/template-syntax.md) kullanabilirsiniz. Bu ortamlar, Kaynak Yöneticisi şablonlarının oluşturabileceği Azure kaynaklarını içerebilir.

[Azure portalını](https://portal.azure.com)kullanarak laboratuvara aynı anda kolayca bir [sanal makine (VM) ekleyebilirsiniz.](devtest-lab-add-vm.md) Ancak, çok katmanlı web uygulamaları veya SharePoint çiftliği gibi senaryoların tek bir adımda birden çok VM oluşturmak için bir mekanizmaya ihtiyacı vardır. Azure Kaynak Yöneticisi şablonlarını kullanarak, Azure çözümünüzün altyapısını ve yapılandırmasını tanımlayabilir ve tutarlı bir durumda sürekli olarak birden çok VM dağıtabilirsiniz.

Azure Kaynak Yöneticisi şablonları da aşağıdaki avantajları sağlar:

- Azure Kaynak Yöneticisi şablonları doğrudan GitHub veya Azure Repos kaynak denetim deposunuzdan yüklenir.
- Kullanıcılarınız, diğer [VM taban](devtest-lab-comparing-vm-base-image-types.md)türlerinde olduğu gibi Azure portalından yapılandırılmış bir Azure Kaynak Yöneticisi şablonu seçerek bir ortam oluşturabilir.
- Azure Kaynak Yöneticisi şablonundan bir ortamda Azure PaaS kaynaklarının yanı sıra IaaS VM'leri de sağlayabilirsiniz.
- Diğer üs türleri tarafından oluşturulan tek tek VM'lere ek olarak, laboratuardaki ortamların maliyetini de izleyebilirsiniz. PaaS kaynakları oluşturulur ve maliyet izleme görünür. Ancak, VM otomatik kapatma PaaS kaynakları için geçerli değildir.

Tek bir işlemde birçok laboratuvar kaynağını dağıtmak, güncelleştirmek veya silmek için Kaynak Yöneticisi şablonlarını kullanmanın yararları hakkında daha fazla bilgi edinmek için Kaynak [Yöneticisi şablonlarını kullanmanın yararları](../azure-resource-manager/management/overview.md#the-benefits-of-using-resource-manager)bölümüne bakın.

> [!NOTE]
> Laboratuvar VM'leri oluşturmak için taban olarak kaynak yöneticisi şablonu kullandığınızda, birden çok VM veya tek bir VM oluşturma arasında bazı farklar vardır. Daha fazla bilgi için [bkz.](devtest-lab-use-resource-manager-template.md)
>

## <a name="use-devtest-labs-public-environments"></a>DevTest Labs ortak ortamlarını kullanma
Azure DevTest Labs, harici bir GitHub kaynağına kendiniz bağlanmak zorunda kalmadan ortam oluşturmak için kullanabileceğiniz [Azure Kaynak Yöneticisi şablonlarının genel deposuna](https://github.com/Azure/azure-devtestlab/tree/master/Environments) sahiptir. Bu genel depo, oluşturduğunuz her laboratuvar için Azure portalında bulunan yapıtların genel deposuna benzer. Ortam deposu, az sayıda giriş parametresi olan önceden yazılmış ortam şablonlarıyla hızla başlamanızı sağlar. Bu şablonlar, laboratuvarlar içindeki PaaS kaynakları için sorunsuz bir başlangıç deneyimi sağlar.

Genel depoda, DevTest Labs ekibi ve diğerleri Azure Web Uygulamaları, Hizmet Kumaş Kümesi ve geliştirme SharePoint Farm ortamı gibi sık kullanılan şablonlar oluşturmuş ve paylaşmıştır. Bu şablonları doğrudan kullanabilir veya gereksinimlerinize uyacak şekilde özelleştirebilirsiniz. Daha fazla bilgi için, [DevTest Labs'daki ortak ortamları yapılandırın ve kullanın.](devtest-lab-configure-use-public-environments.md) Kendi şablonlarınızı oluşturduktan sonra, bunları başkalarıyla paylaşmak için bu depoda saklayabilir veya kendi Git deponuzu ayarlayabilirsiniz.

<a name="configure-your-own-template-repositories"></a>
## <a name="create-your-own-template-repositories"></a>Kendi şablon depolarınızı oluşturun

Kod olarak altyapı ve yapılandırma kodu yla en iyi uygulamalardan biri olarak, kaynak denetiminde ortam şablonlarını yönetmeniz gerekir. Azure DevTest Labs bu uygulamayı izler ve tüm Azure Kaynak Yöneticisi şablonlarını doğrudan GitHub veya Azure Depo depolarından yükler. Sonuç olarak, test ortamından üretim ortamına kadar tüm sürüm döngüsü boyunca Kaynak Yöneticisi şablonlarını kullanabilirsiniz.

Azure Kaynak Yöneticisi şablonlarınızı bir depoda düzenlemek için izninizlen birkaç kural vardır:

- Asıl şablon dosyasını *azuredeploy.json*olarak adlandırmanız gerekir.

- Parametre dosyasında tanımlanan parametre değerlerini kullanmak istiyorsanız, parametre *dosyasıazuredeploy.parameters.json*olarak adlandırılmalıdır.

  Parametreleri `_artifactsLocation` kullanabilir ve `_artifactsLocationSasToken` DevTest Labs'ın iç içe olan şablonları otomatik olarak yönetmesine izin vererek parametreleri Link URI değerini oluşturabilirsiniz. Daha fazla bilgi için [bkz.](deploy-nested-template-environments.md)

- *Metadata.json*adlı bir dosyada şablon görüntü adını ve açıklamasını belirtmek için meta verileri tanımlayabilirsiniz, aşağıdaki gibi:

  ```json
  {
    "itemDisplayName": "<your template name>",
    "description": "<description of the template>"
  }
  ```

![Önemli Azure Kaynak Yöneticisi şablon dosyaları](./media/devtest-lab-create-environment-from-arm/master-template.png)

## <a name="add-template-repositories-to-the-lab"></a>Şablon depolarını laboratuvara ekleme

Deponuzu oluşturduktan ve yapılandırdıktan sonra, Azure portalını kullanarak laboratuvarınıza ekleyebilirsiniz:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.
1. Laboratuvarlar listesinden istediğiniz laboratuarı seçin.
1. Laboratuvarın Genel **Bakış** bölmesine Yapılandırma **ve ilkeler'i**seçin.

   ![Yapılandırma ve ilkeler](./media/devtest-lab-create-environment-from-arm/configuration-and-policies-menu.png)

1. Yapılandırma **ve ilke** ayarları listesinden **Depolar'ı**seçin. **Ortak Artifakı Repo** deposu tüm laboratuvarlar için otomatik olarak oluşturulur ve [DevTest Labs public GitHub deposuna](https://github.com/Azure/azure-devtestlab)bağlanır.

1. Azure Kaynak Yöneticisi şablon deponuzu eklemek için **Ekle'yi**seçin.

   ![Genel repo](./media/devtest-lab-create-environment-from-arm/public-repo.png)

1. **Depolar** bölmesine aşağıdaki bilgileri girin:

   - **Adı**: Laboratuvarda kullanılacak bir depo adı girin.
   - **Git klon URL'si**: GitHub veya Azure Repos'tan Git HTTPS klon URL'sini girin.
   - **Dal** (isteğe bağlı): Azure Kaynak Yöneticisi şablon tanımlarınıza erişmek için şube adını girin.
   - **Kişisel erişim belirteci**: Deponuza güvenli bir şekilde erişmek için kullanılan kişisel erişim jetonuna girin.
     - Profilinizin altındaki Azure Repos'tan belirteçlerinizi almak için **Kullanıcı ayarları** > **Güvenlik** > **Kişisel erişim belirteçlerini**seçin.
     - Profilinizin altındaki GitHub'dan belirteçlerinizi almak için **Ayarlar** > **Geliştirici Ayarları** > **Kişisel erişim belirteçlerini**seçin.
   - **Klasör yolları**: Yapı tanımlarınız veya Azure Kaynak Yöneticisi şablon tanımlarınız için Git klonu URI'nize göre klasör yolunu girin.

1. **Kaydet'i**seçin.

   ![Yeni depo ekleme](./media/devtest-lab-create-environment-from-arm/repo-values.png)

Laboratuvara bir Azure Kaynak Yöneticisi şablonu ekledikten sonra, laboratuvar kullanıcılarınız şablonu kullanarak ortamlar oluşturabilir.

## <a name="configure-access-rights-for-lab-users"></a>Laboratuvar kullanıcıları için erişim haklarını yapılandırma

Laboratuvar kullanıcıları varsayılan olarak **Reader** rolüne sahiptir, bu nedenle ortam kaynak grubundaki kaynakları değiştiremezler. Örneğin, kaynaklarını durduramıyor veya başlatamıyor.

Laboratuvar kullanıcılarınıza ortamlarındaki kaynakları edinebilmeleri için **Katılımcı** rolü vermek için aşağıdaki adımları izleyin:

1. Azure [portalında,](https://portal.azure.com)laboratuvarınızın **Genel Bakış** bölmesinde Yapılandırma ve ilkeleri seçin **ve**ardından **Laboratuvar ayarlarını**seçin.

1. Laboratuvar **ayarları** bölmesinde **Katılımcı'yı**seçin ve ardından laboratuvar kullanıcılarına yazma izni vermek için **Kaydet'i** seçin.

   ![Laboratuvar kullanıcı erişim haklarını yapılandırma](./media/devtest-lab-create-environment-from-arm/config-access-rights.png)

Bir sonraki bölümde, Azure Kaynak Yöneticisi şablonundan ortamlar oluşturma yoluyla yürür.

## <a name="create-environments-from-templates-in-the-azure-portal"></a>Azure portalındaki şablonlardan ortamlar oluşturma

Laboratuvara bir Azure Kaynak Yöneticisi şablonu ekledikten sonra, laboratuvar kullanıcılarınız aşağıdaki adımları izleyerek Azure portalında ortamlar oluşturabilir:

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.

1. Laboratuvarlar listesinden istediğiniz laboratuarı seçin.

1. Laboratuvarın sayfasında **Ekle'yi**seçin.

1. Temel bölme **seç,** önce Azure Kaynak Yöneticisi şablonları listelenmiş olarak kullanabileceğiniz temel görüntüleri görüntüler. İstediğiniz Azure Kaynak Yöneticisi şablonuna seçin.

   ![Bir temel seçin](./media/devtest-lab-create-environment-from-arm/choose-a-base.png)

1. **Ekle** bölmesine, ortam kullanıcılarına görüntülemek için bir **Ortam adı** değeri girin.

   Azure Kaynak Yöneticisi şablonu, giriş alanlarının geri kalanını tanımlar. Şablon *azuredeploy.parameter.json* dosyası varsayılan değerleri tanımlıyorsa, giriş alanları bu değerleri gösterir.

   Tür güvenli *dize*parametreleri için Azure Anahtar Kasası'nızdaki sırları kullanabilirsiniz. Sırları önemli bir kasada depolama ve laboratuvar kaynakları oluştururken kullanma hakkında bilgi edinmek için Azure [Anahtar Kasası'nda sırlar saklayın.](devtest-lab-store-secrets-in-key-vault.md)  

   ![Bölme ekle](./media/devtest-lab-create-environment-from-arm/add.png)

   > [!NOTE]
   > Şablon bunları belirtse bile, aşağıdaki parametre değerleri giriş alanlarında görünmez. Form, ortamı oluştururken laboratuar kullanıcılarının değerleri girmeleri gereken boş giriş alanlarını gösterir.
   >
   > - GEN-UNIQUE
   > - GEN-UNIQUE-[N]
   > - GEN-SSH-PUB-ANAHTAR
   > - GEN-ŞIFRE

1. Ortamı oluşturmak için **Ekle'yi** seçin.

   Ortam, durumu **sanal makineler listemde** görüntüleyerek hemen sağlama başlar. Laboratuvar, Azure Kaynak Yöneticisi şablonunda tanımlanan tüm kaynakları sağlamak için otomatik olarak yeni bir kaynak grubu oluşturur.

1. Ortam oluşturulduktan sonra, kaynak grubu bölmesini açmak ve ortamın sağlanmış tüm kaynaklarına göz atmak için **Sanal Makineler listemdeki** ortamı seçin.

   ![Çevre kaynakları](./media/devtest-lab-create-environment-from-arm/all-environment-resources.png)

   Ayrıca, sağlanan ortamın sadece VM listesini görüntülemek için ortamı genişletebilirsiniz.

   ![Sanal makineler listem](./media/devtest-lab-create-environment-from-arm/my-vm-list.png)

1. Yapı teşkezlikleri uygulama, veri diskleri ekleme, otomatik kapatma süresini değiştirme ve daha fazlası gibi kullanılabilir eylemleri görüntülemek için ortamlardan herhangi birini seçin.

   ![Çevre eylemleri](./media/devtest-lab-create-environment-from-arm/environment-actions.png)

<a name="automate-deployment-of-environments"></a>
## <a name="automate-environment-creation-with-powershell"></a>PowerShell ile ortam oluşturmayı otomatikleştirin

Bir laboratuvara tek bir ortam eklemek için Azure portalını kullanmak mümkündür, ancak bir geliştirme veya test senaryosu birden çok ortam oluşturması gerektiğinde, otomatik dağıtım daha iyi bir deneyimdir.

Devam etmeden önce, oluşturulacak kaynakları tanımlayan bir Azure Kaynak Yöneticisi şablonunuz olduğundan emin olun. [Şablonu Git deposuna ekleyin ve yapılandırın](#configure-your-own-template-repositories)ve [depoyu laboratuvara ekleyin.](#add-template-repositories-to-the-lab)

Aşağıdaki örnek komut dosyası laboratuarınızda bir ortam oluşturur. Yorumlar, komut dosyasını daha iyi anlamanıza yardımcı olur.

1. Aşağıdaki örnek PowerShell komut dosyasını sabit sürücünüze *deployenv.ps1*olarak kaydedin.

   [!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

   ```powershell
   #Requires -Module Az.Resources

   [CmdletBinding()]

   param (
   # ID of the Azure Subscription for the lab
   [string] [Parameter(Mandatory=$true)] $SubscriptionId,

   # Name of the existing lab in which to create the environment
   [string] [Parameter(Mandatory=$true)] $LabName,

   # Name of the connected repository in the lab
   [string] [Parameter(Mandatory=$true)] $RepositoryName,

   # Name of the template (folder name in the Git repository)
   [string] [Parameter(Mandatory=$true)] $TemplateName,

   # Name of the environment to be created in the lab
   [string] [Parameter(Mandatory=$true)] $EnvironmentName,

   # The parameters to be passed to the template. Each parameter is prefixed with "-param_".
   # For example, if the template has a parameter named "TestVMName" with a value of "MyVMName",
   # the string in $Params will have the form: -param_TestVMName MyVMName.
   # This convention allows the script to dynamically handle different templates.
   [Parameter(ValueFromRemainingArguments=$true)]
       $Params
   )

   # Sign in to Azure.
   # Comment out the following statement to completely automate the environment creation.
   Connect-AzAccount

   # Select the subscription that has the lab.  
   Set-AzContext -SubscriptionId $SubscriptionId | Out-Null

   # Get information about the user, specifically the user ID, which is used later in the script.  
   $UserId = $((Get-AzADUser -UserPrincipalName (Get-AzContext).Account).Id.Guid)

   # Get information about the lab, such as lab location.
   $lab = Get-AzResource -ResourceType "Microsoft.DevTestLab/labs" -Name $LabName -ResourceGroupName $ResourceGroupName
   if ($lab -eq $null) { throw "Unable to find lab $LabName in subscription $SubscriptionId." }

   # Get information about the repository in the lab.
   $repository = Get-AzResource -ResourceGroupName $lab.ResourceGroupName `
       -ResourceType 'Microsoft.DevTestLab/labs/artifactsources' `
       -ResourceName $LabName `
       -ApiVersion 2016-05-15 `
       | Where-Object { $RepositoryName -in ($_.Name, $_.Properties.displayName) } `
       | Select-Object -First 1
   if ($repository -eq $null) { throw "Unable to find repository $RepositoryName in lab $LabName." }

   # Get information about the Resource Manager template base for the environment.
   $template = Get-AzResource -ResourceGroupName $lab.ResourceGroupName `
       -ResourceType "Microsoft.DevTestLab/labs/artifactSources/armTemplates" `
       -ResourceName "$LabName/$($repository.Name)" `
       -ApiVersion 2016-05-15 `
       | Where-Object { $TemplateName -in ($_.Name, $_.Properties.displayName) } `
       | Select-Object -First 1
   if ($template -eq $null) { throw "Unable to find template $TemplateName in lab $LabName." }

   # Build the template parameters with parameter name and values.  
   $parameters = Get-Member -InputObject $template.Properties.contents.parameters -MemberType NoteProperty | Select-Object -ExpandProperty Name
   $templateParameters = @()

   # Extract the custom parameters from $Params and format as name/value pairs.
   $Params | ForEach-Object {
       if ($_ -match '^-param_(.*)' -and $Matches[1] -in $parameters) {
           $name = $Matches[1]                
       } elseif ( $name ) {
           $templateParameters += @{ "name" = "$name"; "value" = "$_" }
           $name = $null #reset name variable
       }
   }

   # Once name/value pairs are isolated, create an object to hold the necessary template properties.
   $templateProperties = @{ "deploymentProperties" = @{ "armTemplateId" = "$($template.ResourceId)"; "parameters" = $templateParameters }; }

   # Now, create or deploy the environment in the lab by using the New-AzResource command.
   New-AzResource -Location $Lab.Location `
       -ResourceGroupName $lab.ResourceGroupName `
       -Properties $templateProperties `
       -ResourceType 'Microsoft.DevTestLab/labs/users/environments' `
       -ResourceName "$LabName/$UserId/$EnvironmentName" `
       -ApiVersion '2016-05-15' -Force

   Write-Output "Environment $EnvironmentName completed."
   ```

1. SubscriptionId, LabName, ResourceGroupName, RepositoryName, TemplateName (Git repo'daki klasör) ve EnvironmentName için özel değerlerinizi kullanarak komut dosyasını aşağıdaki gibi çalıştırın.

   ```powershell
   ./deployenv.ps1 -SubscriptionId "000000000-0000-0000-0000-0000000000000" -LabName "mydevtestlab" -ResourceGroupName "mydevtestlabRG000000" -RepositoryName "myRepository" -TemplateName "My Environment template name" -EnvironmentName "myGroupEnv"
   ```

Kaynak Yöneticisi şablonlarıyla kaynakları dağıtmak için Azure CLI'yi de kullanabilirsiniz. Daha fazla bilgi için Kaynak [Yöneticisi şablonları ve Azure CLI ile kaynakları dağıt'a](../azure-resource-manager/templates/deploy-cli.md)bakın.

> [!NOTE]
> Azure PowerShell'i kullanarak Kaynak Yöneticisi şablonundan yalnızca laboratuvar sahibi izinlerine sahip bir kullanıcı VM'ler oluşturabilir. Bir Kaynak Yöneticisi şablonu kullanarak VM oluşturma otomatikleştirmek istiyorsanız ve sadece kullanıcı izinleri var, CLI [komutu az lab vm oluştur](/cli/azure/lab/vm#az-lab-vm-create)kullanabilirsiniz.

## <a name="resource-manager-template-limitations-in-devtest-labs"></a>DevTest Labs'da Kaynak Yöneticisi şablon sınırlamaları

DevTest Labs'da Kaynak Yöneticisi şablonlarını kullanırken bu sınırlamaları göz önünde bulundurun:

- Kaynak Yöneticisi şablonları varolan kaynakların çoğuna başvuramaz. Yalnızca yeni kaynaklar oluşturabilirler. Varolan kaynaklara başvuran DevTest Labs dışında kullandığınız Kaynak Yöneticisi şablonlarınız varsa, bunları DevTest Labs'da kullanamazsınız. Bunun tek istisnası, varolan bir sanal ağa başvuruda bulunabiliyor olmasıdır.

- Kaynak Yöneticisi şablonundan oluşturulan laboratuvar VM'lerinden formüller veya özel görüntüler oluşturamazsınız.

- Kaynak Yöneticisi şablonlarını dağıttığınızda çoğu ilke değerlendirilmez.

  Örneğin, bir kullanıcının yalnızca beş VM oluşturabileceği bir laboratuvar ilkeniz olabilir. Ancak, bir kullanıcı düzinelerce VM oluşturan bir Kaynak Yöneticisi şablonu dağıtabilir. Değerlendirilmeyan ilkeler şunlardır:

  - Kullanıcı başına VM sayısı

  - Laboratuvar kullanıcısı başına premium VM sayısı

  - Laboratuvar kullanıcısı başına premium disk sayısı

## <a name="next-steps"></a>Sonraki adımlar
- Bir VM oluşturduktan sonra, VM'nin yönetim bölmesindeki **Bağlan'ı** seçerek VM'ye bağlanabilirsiniz.
- Laboratuarınızdaki **Sanal Makineler listemdeki** ortamı seçerek bir ortamdaki kaynakları görüntüleyin ve yönetin.
- Azure [Quickstart şablon galerisinden Azure Kaynak Yöneticisi şablonlarını](https://github.com/Azure/azure-quickstart-templates)keşfedin.
