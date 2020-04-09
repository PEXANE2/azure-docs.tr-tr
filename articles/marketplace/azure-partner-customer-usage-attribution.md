---
title: Azure iş ortağı ve müşteri kullanımı atıf | Azure Marketi
description: Azure Marketi çözümleri için müşteri kullanımının nasıl izleyebilirsiniz
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 9/23/2019
ms.author: dsindona
ms.openlocfilehash: 2895944dea6417949488076186135680523e19db
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80874979"
---
# <a name="azure-partner-customer-usage-attribution"></a>Azure iş ortağı müşteri kullanımı ilişkilendirmesi

Azure için bir yazılım ortağı olarak, çözümleriniz Azure bileşenlerigerektirir veya doğrudan Azure altyapısına dağıtılmalı. İş ortağı çözümlerini dağıtan ve kendi Azure kaynaklarını tedarik eden müşteriler, dağıtımın durumu hakkında görünürlük elde etmekte zorlanabilir ve Azure büyümesi üzerindeki etkiyi optik olarak ele alabilir. Daha yüksek bir görünürlük düzeyi eklediğinizde, Microsoft satış ekipleriyle hizalanır ve Microsoft iş ortağı programları için kredi kazanırsınız.

Microsoft artık iş ortaklarının yazılımlarının Azure'daki müşteri dağıtımlarının Azure kullanımını daha iyi izlemelerine yardımcı olacak bir yöntem sunuyor. Yeni yöntem, Azure hizmetlerinin dağıtımını düzenlemek için Azure Kaynak Yöneticisi'ni kullanır.

Microsoft iş ortağı olarak, Azure kullanımını müşteri adına sağladığınız tüm Azure kaynaklarıyla ilişkilendirebilirsiniz. İlişkilendirmeyi Azure Marketi, Quickstart deposu, özel GitHub depoları ve bire bir müşteri etkileşimi aracılığıyla oluşturabilirsiniz. Müşteri kullanımı atıf üç dağıtım seçeneğini destekler:

- Azure Kaynak Yöneticisi şablonları: İş ortakları, iş ortağının yazılımını çalıştırmak için Azure hizmetlerini dağıtmak için Kaynak Yöneticisi şablonlarını kullanabilir. İş ortakları, Azure çözümlerinin altyapısını ve yapılandırmasını tanımlamak için bir Kaynak Yöneticisi şablonu oluşturabilir. Kaynak Yöneticisi şablonu, sizin ve müşterilerinizin çözümünüzü yaşam döngüsü boyunca dağıtmanızı sağlar. Kaynaklarınızın tutarlı bir durumda dağıtıldığınızdan emin olabilirsiniz.
- Azure Kaynak Yöneticisi API'leri: İş ortakları, Kaynak Yöneticisi şablonu dağıtmak veya Doğrudan Azure hizmetlerini sağlamak için API çağrıları oluşturmak için Doğrudan Kaynak Yöneticisi API'lerini arayabilir.
- Terraform: İş ortakları, Bir Kaynak Yöneticisi şablonu dağıtmak veya Azure hizmetlerini doğrudan dağıtmak için Terraform gibi bulut orkestratörlerini kullanabilir.

Müşteri kullanımı atıf yeni dağıtım içindir ve zaten dağıtılan varolan kaynakları etiketlemeyi desteklemez.

[Azure Uygulaması:](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/azure-applications/cpp-azure-app-offer)Azure Marketi'nde yayınlanan çözüm şablonu teklifinde müşteri kullanımı atıfı gereklidir.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="use-resource-manager-templates"></a>Resource Manager şablonlarını kullanma
Kaynak Yöneticisi şablonları kullanılarak müşterinin aboneliğinde birçok iş ortağı çözümü dağıtılır. Azure Marketi'nde, GitHub'da veya Quickstart olarak kullanılabilen bir Kaynak Yöneticisi şablonunuz varsa, müşteri kullanımı ilişkilendirmesini etkinleştirmek için şablonunuzu değiştirme işlemi doğrudan olmalıdır.

Çözüm Şablonları oluşturma ve yayımlama hakkında daha fazla bilgi için bkz.

* [İlk Kaynak Yöneticisi şablonunuzu oluşturun ve dağıtın.](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal)
* [Azure Uygulama teklifi.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/azure-applications/cpp-azure-app-offer)
* Video: [Azure Marketi için Çözüm Şablonları Oluşturma ve Yönetilen Uygulamalar.](https://channel9.msdn.com/Events/Build/2018/BRK3603)


## <a name="add-a-guid-to-your-template"></a>Şablonunuza GUID ekleme

Genel olarak benzersiz bir tanımlayıcı (GUID) eklemek için, ana şablon dosyasında tek bir değişiklik yaparsınız:

1. Önerilen yöntemi kullanarak [bir GUID oluşturun](#create-guids) ve [GUID'i kaydedin.](#register-guids-and-offers)

1. Kaynak Yöneticisi şablonu açın.

1. Ana şablon dosyasına yeni bir kaynak ekleyin. Kaynağın **yalnızca mainTemplate.json** veya **azuredeploy.json** dosyasında olması gerekir ve iç içe veya bağlı şablonlarda bulunmamalıdır.

1. **Pid önekinden** sonra GUID değerini girin (örneğin, pid-eb7927c8-dd66-43e1-b0cf-c346a422063).

1. Herhangi bir hata için şablonu denetleyin.

1. Şablonu uygun depolarda yeniden yayımlayın.

1. [Şablon dağıtımında GUID başarısını doğrulayın.](#verify-the-guid-deployment)

### <a name="sample-resource-manager-template-code"></a>Örnek Kaynak Yöneticisi şablon kodu

Şablonunuz için izleme kaynaklarını etkinleştirmek için, kaynaklar bölümüne aşağıdaki ek kaynağı eklemeniz gerekir. Lütfen ana şablon dosyasına eklediğinizde aşağıdaki örnek kodu kendi girişlerinizle değiştirdiğinizden emin olun.
Kaynağın **yalnızca mainTemplate.json** veya **azuredeploy.json** dosyasına eklenmesi gerekir ve iç içe veya bağlı şablonlarda bulunmamalıdır.

```
// Make sure to modify this sample code with your own inputs where applicable

{ // add this resource to the resources section in the mainTemplate.json (do not add the entire file)
    "apiVersion": "2018-02-01",
    "name": "pid-XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX", // use your generated GUID here
    "type": "Microsoft.Resources/deployments",
    "properties": {
        "mode": "Incremental",
        "template": {
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "resources": []
        }
    }
} // remove all comments from the file when complete
```

## <a name="use-the-resource-manager-apis"></a>Kaynak Yöneticisi API'lerini kullanma

Bazı durumlarda, Azure hizmetlerini dağıtmak için doğrudan Kaynak Yöneticisi REST API'lerine karşı arama yapmayı tercih edebilirsiniz. Azure, bu çağrıları etkinleştirmek için [birden çok SDK'yı destekler.](https://docs.microsoft.com/azure/?pivot=sdkstools) SDK'lardan birini kullanabilir veya kaynakları dağıtmak için doğrudan REST API'lerini arayabilirsiniz.

Kaynak Yöneticisi şablonu kullanıyorsanız, daha önce açıklanan yönergeleri izleyerek çözümünüzü etiketlemeniz gerekir. Kaynak Yöneticisi şablonu kullanmıyorsanız ve doğrudan API çağrıları yapmıyorsanız, dağıtımınızı Azure kaynaklarının kullanımını ilişkilendirmek için etiketlemeye devam edebilirsiniz.

### <a name="tag-a-deployment-with-the-resource-manager-apis"></a>Kaynak Yöneticisi API'leri ile dağıtımı etiketleme

Müşteri kullanımı ilişkilendirmesini etkinleştirmek için, API çağrılarınızı tasarlarken, isteğe kullanıcı aracısı üstbilgisine bir GUID ekleyin. Her teklif veya SKU için GUID'i ekleyin. Dizeyi **pid önekiyle** biçimlendirin ve ortak tarafından oluşturulan GUID'i ekleyin. Kullanıcı aracısına ekleme için GUID biçiminin bir örneği aşağıda verilmiştir:

![Örnek GUID biçimi](media/marketplace-publishers-guide/tracking-sample-guid-for-lu-2.PNG)

> [!Note]
> Dize biçimi önemlidir. Pid **öneki** dahil değilse, verileri sorgulamak mümkün değildir. Farklı SDK'lar farklı şekilde izler. Bu yöntemi uygulamak için, tercih ettiğiniz Azure SDK'sı için destek ve izleme yaklaşımını gözden geçirin.

#### <a name="example-the-python-sdk"></a>Örnek: Python SDK

Python için **config** özniteliğini kullanın. Özniteliği yalnızca bir UserAgent'a ekleyebilirsiniz. Bir örneği aşağıda verilmiştir:

![Bir kullanıcı aracısına öznitelik ekleme](media/marketplace-publishers-guide/python-for-lu.PNG)

> [!Note]
> Her istemci için öznitelik ekleyin. Küresel statik yapılandırma yok. Her istemcinin izlediğinden emin olmak için bir istemci fabrikasını etiketleyebilirsiniz. Daha fazla bilgi için [GitHub'daki](https://github.com/Azure/azure-cli/blob/7402fb2c20be2cdbcaa7bdb2eeb72b7461fbcc30/src/azure-cli-core/azure/cli/core/commands/client_factory.py#L70-L79)bu istemci fabrika örneğine bakın.

#### <a name="tag-a-deployment-by-using-the-azure-powershell"></a>Azure PowerShell'i kullanarak dağıtımı etiketleme

Kaynakları Azure PowerShell üzerinden dağıtıyorsanız, aşağıdaki yöntemi kullanarak GUID'inizi ekleyerek aşağıdaki leri ekler:

```powershell
[Microsoft.Azure.Common.Authentication.AzureSession]::ClientFactory.AddUserAgent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")
```

#### <a name="tag-a-deployment-by-using-the-azure-cli"></a>Azure CLI'yi kullanarak dağıtımı etiketleme

GUID'inizi eklemek için Azure CLI'yi **kullandığınızda, AZURE_HTTP_USER_AGENT** ortamı değişkenini ayarlayın. Bu değişkeni bir komut dosyası kapsamında ayarlayabilirsiniz. Ayrıca kabuk kapsamı için genel değişkenayarlayabilirsiniz:

```
export AZURE_HTTP_USER_AGENT='pid-eb7927c8-dd66-43e1-b0cf-c346a422063'
```
Daha fazla bilgi için [Azure SDK for Go 'ya](https://docs.microsoft.com/azure/go/)bakın.

## <a name="use-terraform"></a>Terraform kullanın

Terraform desteği, Azure Sağlayıcısı'nın 1.21.0 sürümü [https://github.com/terraform-providers/terraform-provider-azurerm/blob/master/CHANGELOG.md#1210-january-11-2019](https://github.com/terraform-providers/terraform-provider-azurerm/blob/master/CHANGELOG.md#1210-january-11-2019)aracılığıyla kullanılabilir: .  Bu destek, çözümlerini Terraform üzerinden dağıtan tüm iş ortakları ve Azure Sağlayıcısı tarafından dağıtılan ve ölçülen tüm kaynaklar için geçerlidir (sürüm 1.21.0 veya sonraki sürüm).

Terraform için Azure sağlayıcısı, [*çözümünüz*](https://www.terraform.io/docs/providers/azurerm/#partner_id) için kullandığınız izleme GUID'ini belirttiğiniz partner_id adında yeni bir isteğe bağlı alan ekledi. Bu alanın *değeri, ARM_PARTNER_ID* Çevre Değişkeni'nden de kaynaklanabilir.

```
provider "azurerm" {
          subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          client_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          ……
          # new stuff for ISV attribution
          partner_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"}
```
Dağıtımlarını Terraform üzerinden müşteri kullanımı atıfları tarafından izlenen bir şekilde almak isteyen iş ortaklarının aşağıdakileri yapması gerekir:

* Guid oluşturma (GUID her Teklif veya SKU için eklenmelidir)
* *partner_id* değerini GUID'e ayarlamak için Azure Sağlayıcılarını güncelleştirin (GUID'i "pid-" ile önceden düzeltmeyin, gerçek GUID'e ayarlayın)

## <a name="create-guids"></a>GUID'ler oluşturma

GUID, 32 hexadecimal basamaklı benzersiz bir başvuru numarasıdır. İzleme için GUID'ler oluşturmak için bir GUID jeneratörü kullanmanız gerekir. Azure Depolama ekibi, size doğru biçimde bir GUID e-postagönderebilen ve farklı izleme sistemlerinde yeniden kullanılabilen bir [GUID jeneratör formu](https://aka.ms/StoragePartners) oluşturmuştur.

> [!Note]
> GUID'inizi oluşturmak için [Azure Depolama'nın GUID jeneratör formunu](https://aka.ms/StoragePartners) kullanmanız önerilir. Daha fazla bilgi için [SSS](#faq)bölümüne bakın.

Her ürün için her teklif ve dağıtım kanalı için benzersiz bir GUID oluşturmanızı öneririz. Raporlamanın bölünmesini istemiyorsanız, ürünün birden çok dağıtım kanalı için tek bir GUID kullanmayı tercih edebilirsiniz.

Bir ürünü bir şablon kullanarak dağıtıyorsanız ve hem Azure Marketi'nde hem de GitHub'da kullanılabilirse, 2 farklı GUIDS oluşturabilir ve kaydedebilirsiniz:

*   Azure Marketinde Ürün A
*   Ürün A GitHub üzerinde

Raporlama, iş ortağı değeri (Microsoft İş Ortağı Kimliği) ve GUID'ler tarafından yapılır.

Ayrıca, SKU'ların bir teklifin varyantları olduğu SKU gibi daha ayrıntılı bir düzeyde GUID'leri de izleyebilirsiniz.

## <a name="register-guids-and-offers"></a>GUID'leri ve teklifleri kaydedin

GUI'ler müşteri kullanımı ilişkilendirmesini etkinleştirmek için kaydedilmelidir.

Şablon GUID'ler için tüm kayıtlar Ortak Merkezi içinde yapılır.

GUID'i şablonunuza veya kullanıcı aracısına ekledikten ve GUID'i İş Ortağı Merkezi'ne kaydettirdikten sonra tüm dağıtımlar izlenir.

1. Ticari bir [pazar yayıncısı](https://aka.ms/JoinMarketplace)olarak kaydolun.

   * İş ortaklarının [Partner Center'da bir profili olması](https://docs.microsoft.com/azure/marketplace/become-publisher)gerekmektedir. Teklifi Azure Marketi veya AppSource'da listele etmeniz tavsiye ediliyor.
   * Ortaklar birden fazla GUID kaydedebilir.
   * İş ortakları, Pazar Yeri olmayan çözüm şablonları ve teklifleri için bir GUID kaydedebilir.

1. [Ortak Merkezi'nde](https://partner.microsoft.com/dashboard)oturum açın.

1. Sağ üst köşede, ayarlar dişli simgesini seçin ve ardından **Geliştirici ayarlarını**seçin.

1. Hesap **ayarları sayfasında** **İzleme GUID ekle'yi seçin.**

1. **GUID** kutusuna, izleme GUID'inizi girin. **Pid öneki** olmadan sadece GUID'i girin. **Açıklama** kutusuna teklif adınızı veya açıklamanızı girin.

1. Birden fazla GUID kaydetmek için, **İzleme GUID ekle'yi** yeniden seçin. Sayfada ek kutular görünür.

1. **Kaydet'i**seçin.


## <a name="verify-the-guid-deployment"></a>GUID dağıtımını doğrulama

Şablonunuzu değiştirip bir test dağıtımı çalıştırdıktan sonra, dağıttığınız ve etiketlediğiniz kaynakları almak için aşağıdaki PowerShell komut dosyasını kullanın.

GUID'nin Kaynak Yöneticisi şablonunuza başarıyla eklettiğini doğrulamak için komut dosyasını kullanabilirsiniz. Komut dosyası Kaynak Yöneticisi API veya Terraform dağıtımları için geçerli değildir.

Azure'da oturum açın. Komut dosyasını çalıştırmadan önce doğrulamak istediğiniz dağıtımla birlikte aboneliği seçin. Komut dosyasını dağıtımın abonelik bağlamında çalıştırın.

Dağıtımın **GUID** ve **kaynak Grubu** adı gerekli parametrelerdir.

[Orijinal komut dosyasını GitHub'dan](https://gist.github.com/bmoore-msft/ae6b8226311014d6e7177c5127c7eba1#file-verify-deploymentguid-ps1) alabilirsiniz.

```powershell
Param(
    [GUID][Parameter(Mandatory=$true)]$guid,
    [string][Parameter(Mandatory=$true)]$resourceGroupName
)

# Get the correlationId of the pid deployment

$correlationId = (Get-AzResourceGroupDeployment -ResourceGroupName
$resourceGroupName -Name "pid-$guid").correlationId

# Find all deployments with that correlationId

$deployments = Get-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName | Where-Object{$_.correlationId -eq $correlationId}

# Find all deploymentOperations in a deployment by name
# PowerShell doesn't surface outputResources on the deployment
# or correlationId on the deploymentOperation

foreach ($deployment in $deployments){

# Get deploymentOperations by deploymentName
# then the resourceId for any create operation

($deployment | Get-AzResourceGroupDeploymentOperation | Where-Object{$_.properties.provisioningOperation -eq "Create" -and $_.properties.targetResource.resourceType -ne "Microsoft.Resources/deployments"}).properties.targetResource.id

}
```

## <a name="report"></a>Rapor

İş Ortağı Merkezi panonuzda müşteri kullanımı ile ilgili[https://partner.microsoft.com/dashboard/mpn/analytics/CPP/MicrosoftAzure](https://partner.microsoft.com/dashboard/mpn/analytics/CPP/MicrosoftAzure)atıf raporunu bulabilirsiniz ( ). Raporu görmek için oturum açabilmek için Ortak Merkezi kimlik bilgilerinizi kullanmanız gerekir. Rapor veya oturum açma yla ilgili herhangi bir sorunla karşılaşırsanız, destek al bölümündeki yönergeyi izleyerek bir destek isteği oluşturun.

Raporu görmek için Ortak İlişkilendirme Türü açılır listesinde Izlenen Şablon'u seçin.

![Müşteri kullanımı atıf raporu](media/marketplace-publishers-guide/customer-usage-attribution-report.png)

## <a name="notify-your-customers"></a>Müşterilerinize bildirin

İş ortakları, müşteri kullanımı atıfını kullanan dağıtımlar hakkında müşterilerini bilgilendirmelidir. Microsoft, bu dağıtımlarla ilişkili Azure kullanımını iş ortağına bildirir. Aşağıdaki örnekler, müşterilerinizi bu dağıtımlar hakkında bilgilendirmek için kullanabileceğiniz içeriği içerir. Örneklerde, PARTNER \<>'ı şirket adınız ile değiştirin. İş ortakları, müşterilerin izleme dışında tutulabilmesini sağlayan seçenekler de dahil olmak üzere bildirimin veri gizliliği ve toplama politikalarıyla uyumlu olduğundan emin olmalıdır.

### <a name="notification-for-resource-manager-template-deployments"></a>Kaynak Yöneticisi şablon dağıtımları için bildirim

Bu şablonu dağıttığınızda, Microsoft, dağıtılan Azure kaynaklarıyla ORTAK> yazılımının \<yüklenmesini belirleyebilir. Microsoft, yazılımı desteklemek için kullanılan Azure kaynaklarını ilişkilendirebilir. Microsoft bu bilgileri, ürünleriyle en iyi deneyimleri sağlamak ve işlerini yürütmek için toplar. Veriler toplanır ve Microsoft'un gizlilik politikaları tarafından yönetilir, hangi https://www.microsoft.com/trustcenterbulunabilir.

### <a name="notification-for-sdk-or-api-deployments"></a>SDK veya API dağıtımları için bildirim

MICROSOFT, \<İŞ> yazılımını dağıttığınızda, İş \<ortağı> yazılımının yüklenmesini dağıtılan Azure kaynaklarıyla belirleyebilir. Microsoft, yazılımı desteklemek için kullanılan Azure kaynaklarını ilişkilendirebilir. Microsoft bu bilgileri, ürünleriyle en iyi deneyimleri sağlamak ve işlerini yürütmek için toplar. Veriler toplanır ve Microsoft'un gizlilik politikaları tarafından yönetilir, hangi https://www.microsoft.com/trustcenterbulunabilir.

## <a name="get-support"></a>Destek alın

Karşılaştığınız sorunlara bağlı olarak iki destek kanalı vardır.

İş Ortağı Merkezi'nde müşteri kullanımı atıf raporunu görmek veya oturum açma gibi sorunlarla karşılaşırsanız, Burada İş Ortağı Merkezi destek ekibiyle bir destek isteği oluşturun:[https://partner.microsoft.com/support](https://partner.microsoft.com/support)

![](./media/marketplace-publishers-guide/partner-center-log-in-support.png)

Müşteri kullanımı atfını nasıl ayarlanız gerektiği gibi genel olarak Market Onboarding ve/veya müşteri kullanımı atıfı için yardıma ihtiyacınız varsa aşağıdaki adımları izleyin:

1. [Destek sayfasına](https://go.microsoft.com/fwlink/?linkid=844975)gidin.

1. **Sorun türü**altında, **Market Onboarding'i**seçin.

1. Sorununuzun **Kategorisini** seçin:

   - Kullanım ilişkilendirme sorunları için **Diğer'i**seçin.
   - Azure Marketi ile ilgili erişim sorunları için **Access Problem'i**seçin.

     ![Sorun kategorisini seçin](media/marketplace-publishers-guide/lu-article-incident.png)

1. **Başlat İsteği'ni**seçin.

1. Sonraki sayfada, gerekli değerleri girin. **Devam**'ı seçin.

1. Sonraki sayfada, gerekli değerleri girin.

   > [!Important]
   > Olay **başlık** kutusuna **ISV Kullanım İzleme'yi**girin. Sorununuzu ayrıntılı olarak açıklayın.

   ![Olay başlığı için ISV Kullanım İzleme'yi girin](media/marketplace-publishers-guide/guid-dev-center-help-hd%201.png)

1. Formu doldurun ve **gönder'i**seçin.

Ayrıca, müşteri kullanımı atamasını anlamak ve dahil etmek için teknik satış öncesi, dağıtım ve uygulama geliştirme senaryoları için bir Microsoft İş Ortağı Teknik Danışmanı'ndan teknik rehberlik de alabilirsiniz.

### <a name="how-to-submit-a-technical-consultation-request"></a>Teknik danışma isteği nasıl gönderilir?

1. Ziyaret [https://aka.ms/TechnicalJourney](https://aka.ms/TechnicalJourney)edin.
1. Bulut altyapısını ve yönetimini seçin ve teknik yolculuğu görüntülemeniz için yeni bir sayfa açılacaktır.
1. Dağıtım Hizmetleri altında, istek gönder düğmesini tıklatın
1. MSA (MPN hesabınızı) veya AAD (İş Ortağı Panosu hesabınızı) kullanarak oturum açın; oturum açma kimlik bilgilerinize bağlı olarak, çevrimiçi bir istek formu açılır:
    * İletişim bilgilerini tamamlayın/gözden geçirin.
    * Danışma ayrıntıları önceden doldurulmuş olabilir veya açılır düşüşlerden seçilebilir.
    * Bir başlık ve sorunun açıklamasını girin (mümkün olduğunca fazla ayrıntı sağlayın).
1. Gönder’e tıklayın

[https://aka.ms/TechConsultInstructions](https://aka.ms/TechConsultInstructions)Ekran görüntüleriyle adım adım yönergeleri görüntüleyin.

### <a name="whats-next"></a>Sırada ne var?

İhtiyaçlarınızı karşılamak için bir çağrı ayarlamak için bir Microsoft İş Ortağı Teknik Danışmanı sizinle irtibata geçecektir.

## <a name="faq"></a>SSS

**GUID'i şablona eklemenin yararı nedir?**

Microsoft, iş ortaklarına, çözümlerinin müşteri dağıtımları ve etkilenmiş kullanımlarıyla ilgili öngörüleri hakkında bir görünüm sağlar. Hem Microsoft hem de iş ortağı, satış ekipleri arasında daha yakın etkileşim ilerletmek için bu bilgileri kullanabilir. Hem Microsoft hem de iş ortağı, tek bir iş ortağının Azure büyümesi üzerindeki etkisini daha tutarlı bir şekilde görmek için verileri kullanabilir.

**GUID eklendikten sonra değiştirilebilir mi?**

Evet, bir müşteri veya uygulama ortağı şablonu özelleştirebilir ve GUID'i değiştirebilir veya kaldırabilir. İş ortaklarının GUID'in kaldırılmasını veya yapılan ları önlemek için kaynağın ve GUID'nin rolünü müşterilerine ve iş ortaklarına proaktif bir şekilde açıklamalarını öneririz. GUID'i değiştirmek yalnızca yeni, varolan değil, dağıtımları ve kaynakları etkiler.

**GitHub gibi Microsoft'a ait olmayan bir depodan dağıtılan şablonları izleyebilir miyim?**

Evet, şablon dağıtıldığında GUID olduğu sürece, kullanım izlenir. İş ortaklarının, Azure Marketi dışında dağıtım için kullanılan GUID'leri kaydetmek için İş Merkezi'ndeki ticari pazar kaydında bir profile sahip olması gerekir.

**Müşteri de raporlama alıyor mu?**

Müşteriler Azure portalında tek tek kaynakları veya müşteri tanımlı kaynak gruplarını izleyebilir.

**Bu metodoloji Dijital Kayıt Ortağı'na (DPOR) benzer mi?**

Dağıtım ve kullanımı bir iş ortağının çözümüne bağlamanın bu yeni yöntemi, iş ortağı çözümünü Azure kullanımına bağlayacak bir mekanizma sağlar. DPOR, bir danışmanlık (Sistem Entegratör) veya yönetim (Yönetilen Hizmet Sağlayıcısı) iş ortağını müşterinin Azure aboneliğiyle ilişkilendirmeyi amaçlamaktadır.

**Azure Depolama'nın GUID Generator formunu kullanmanın ne yararı var?**

Azure Depolama'nın GUID Generator formu, gerekli biçimde bir GUID oluşturmak için garanti edilir. Ayrıca, Azure Depolama'nın veri düzlemi izleme yöntemlerinden herhangi birini kullanıyorsanız, Pazar Yeri denetimi düzlemi izleme için aynı GUID'den yararlanabilirsiniz. Bu, ayrı GUIDS korumak zorunda kalmadan Ortak atıf için tek bir birleşik GUID kaldıraç sağlar.

**Azure Marketi'ndeki çözüm şablonu teklifi için özel, özel bir VHD kullanabilir miyim?**

Hayır, olmaz. Sanal makine görüntüsü Azure Marketi'nden [https://docs.microsoft.com/azure/marketplace/marketplace-virtual-machines](https://docs.microsoft.com/azure/marketplace/marketplace-virtual-machines)gelmelidir, bkz: .

Özel VHD'nizi kullanarak pazarda bir VM teklifi oluşturabilir ve kimsenin göremeyebileceği özel olarak işaretleyebilirsiniz. Ardından çözüm şablonunuzda bu VM'ye başvurun.

**Ana şablon için *içerikSürüm* özelliğini güncelleştirmek için başarısız oldu?**

Şablon, bazı nedenlerden dolayı eski içerikSürüm bekleyen başka bir şablondan TemplateLink kullanılarak dağıtılırken büyük olasılıkla bir hatadır. Geçici çözüm meta veri özelliğini kullanmaktır:

```
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "metadata": {
        "contentVersion": "1.0.1.0"
    },
    "parameters": {
```
