---
title: Ticari Market iş ortağı ve müşteri kullanımı attributıon
description: Azure Marketi çözümleri için müşteri kullanımını izlemeye genel bir bakış alın.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: dsindona
ms.openlocfilehash: 4dd42c6eabf02bc7a4eb956b9cd5ab2aec28176b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82186212"
---
# <a name="commercial-marketplace-partner-and-customer-usage-attribution"></a>Ticari Market iş ortağı ve müşteri kullanımı attributıon

Müşteri kullanım atımı, çözümünüzü çalıştırmak için dağıtılan ve iş ortağı olarak, müşteri aboneliklerinde çalışan Azure kaynaklarını ilişkilendirmek için kullanılan bir yöntemdir. Bu ilişkilerin iç Microsoft sistemlerinde oluşi, yazılımınızı çalıştıran Azure ayak izine daha fazla görünürlük getirir. Bu izleme özelliğini benimsediğinizde Microsoft satış ekipleriyle hizalanır ve Microsoft iş ortağı programları için kredi elde edersiniz.

İlişkiyi Azure Marketi, hızlı başlangıç deposu, özel GitHub depoları ve dayanıklı IP (bir uygulama geliştirme gibi) oluşturan 1:1 müşteri görevlendirmeleri aracılığıyla oluşturabilirsiniz.

Müşteri kullanım attributıon üç dağıtım seçeneğini destekler:

- Azure Resource Manager şablonlar: Iş ortakları, iş ortağının yazılımlarını çalıştırmak üzere Azure hizmetlerini dağıtmak için Kaynak Yöneticisi şablonları kullanabilir. İş ortakları, Azure çözümünün altyapısını ve yapılandırmasını tanımlamak için bir Kaynak Yöneticisi şablonu oluşturabilir. Bir Kaynak Yöneticisi şablonu, size ve müşterilerinizin kendi yaşam döngüsünün tamamında çözümünüzü dağıtmasına olanak sağlar. Kaynaklarınızın tutarlı bir durumda dağıtıldığından emin olabilirsiniz.
- Azure Resource Manager API 'Leri: Iş ortakları, doğrudan bir Kaynak Yöneticisi şablonu dağıtmak veya doğrudan Azure hizmetlerini sağlamak üzere API çağrıları oluşturmak için Kaynak Yöneticisi API 'Lerini çağırabilir.
- Terrayform: Iş ortakları Terrayform 'u bir Kaynak Yöneticisi şablonu dağıtmak veya doğrudan Azure hizmetlerini dağıtmak için kullanabilir.

>[!IMPORTANT]
>- Müşteri kullanımı attributıon, Azure 'da çalışan yazılımları dağıtmak ve yönetmek için tasarlanan sistem tümleştiricileri, yönetilen hizmet sağlayıcılarının veya araçların çalışmasını izlemek üzere tasarlanmamıştır.
>
>- Müşteri kullanım attributıon Yeni dağıtımlar içindir ve zaten dağıtılmış olan mevcut kaynakların etiketlemesini desteklemez.
>
>- Azure Market 'Te yayımlanan [Azure Uygulama](./partner-center-portal/create-new-azure-apps-offer.md) teklifleri için müşteri kullanım atısyonu gereklidir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="create-guids"></a>GUID 'Ler oluştur

GUID, 32 onaltılık basamak içeren benzersiz bir başvuru tanımlayıcısıdır. İzleme için GUID 'Ler oluşturmak üzere bir GUID Oluşturucu kullanmanız gerekir. Azure depolama ekibi, doğru biçimin bir GUID 'sine e-posta sağlayacak bir [GUID Oluşturucu formu](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR3i8TQB_XnRAsV3-7XmQFpFUMVRVVFFLTDFLS0E2QzNYSkFZR1U3WVJCTSQlQCN0PWcu) oluşturdu ve farklı izleme sistemleri arasında yeniden kullanılabilir.

> [!NOTE]
> GUID 'nizi oluşturmak için [Azure Storage 'un GUID Oluşturucu formunu](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR3i8TQB_XnRAsV3-7XmQFpFUMVRVVFFLTDFLS0E2QzNYSkFZR1U3WVJCTSQlQCN0PWcu) kullanmanız önemle tavsiye edilir. Daha fazla bilgi için bkz. [SSS](#faq).

Her ürün için her bir teklif ve dağıtım kanalı için benzersiz bir GUID oluşturmanız önerilir. Raporlama 'nın bölünmesini istemiyorsanız, ürünün birden çok dağıtım kanalı için tek bir GUID kullanmayı tercih edebilirsiniz.

Bir ürünü bir şablon kullanarak dağıtırsanız ve hem Azure Marketi hem de GitHub üzerinde mevcutsa, iki ayrı GUID oluşturup kaydedebilirsiniz:

- Azure Market 'te A ürünü
- GitHub 'da A ürünü

Raporlama Microsoft İş Ortağı Ağı ID ve GUID tarafından yapılır.

Ayrıca, ek GUID 'ler kaydederek ve, planlar arasında GUID 'Leri değiştirerek ve planların bir teklifin varyantları olduğu durumlarda kullanımı daha ayrıntılı bir şekilde izleyebilirsiniz.

## <a name="register-guids"></a>GUID 'Leri Kaydet

GUID 'lerin, müşteri kullanımı atısyonu sağlamak için Iş Ortağı Merkezi 'ne kayıtlı olması gerekir.

Şablonunuza veya kullanıcı aracısına bir GUID ekleyin ve GUID 'yi Iş Ortağı Merkezi 'ne kaydettikten sonra gelecek dağıtımlar izlenir.

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard)' nde oturum açın.

1. [Ticari Market yayımcısı](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/azureisv)olarak kaydolun.

   * İş ortaklarının [Iş Ortağı Merkezi 'nde bir profili olması](https://docs.microsoft.com/azure/marketplace/become-publisher)gerekir. Teklifi Azure Market veya AppSource 'ta listeliyoruz.
   * İş ortakları birden çok GUID kaydedebilir.
   * İş ortakları, Market olmayan çözüm şablonları ve teklifleri için GUID 'Leri kaydedebilir.

1. Sağ üst köşede, ayarlar dişli simgesini seçin ve ardından **Geliştirici ayarları**' nı seçin.

1. **Hesap ayarları sayfasında**, **Izleme GUID 'si Ekle** ' yi seçin.

1. **GUID** kutusuna izleme GUID 'nizi girin. **PID** ön eki olmadan yalnızca GUID girin. **Açıklama** kutusuna teklif adınızı veya açıklamasını girin.

1. Birden fazla GUID kaydetmek için, **izleme GUID 'i yeniden Ekle** ' yi seçin. Sayfada ek kutular görüntülenir.

1. **Kaydet**’i seçin.

## <a name="use-resource-manager-templates"></a>Resource Manager şablonlarını kullanma
Birçok iş ortağı çözümü Azure Resource Manager şablonları kullanılarak dağıtılır. Azure Marketi 'nde, GitHub 'da veya hızlı başlangıç olarak kullanılabilen bir Kaynak Yöneticisi şablonunuz varsa, müşteri kullanımı atısyonu 'nı etkinleştirmek için şablonunuzu değiştirme işlemi sorunsuz bir şekilde ileriye doğru.

> [!NOTE]
> Çözüm şablonları oluşturma ve yayımlama hakkında daha fazla bilgi için bkz.
> * [İlk kaynak yöneticisi şablonunuzu oluşturun ve dağıtın](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).
>* [Azure Uygulama teklifi](./partner-center-portal/create-new-azure-apps-offer.md).
>* Video: [Azure Marketi Için çözüm şablonları ve yönetilen uygulamalar oluşturma](https://channel9.msdn.com/Events/Build/2018/BRK3603).


Bir genel benzersiz tanımlayıcı (GUID) eklemek için, ana şablon dosyasında tek bir değişiklik yaparsınız:

1. Önerilen yöntemi kullanarak [BIR GUID oluşturun](#create-guids) ve [GUID 'yi kaydedin](#register-guids).

1. Kaynak Yöneticisi şablonunu açın.

1. Ana şablon dosyasına yeni bir kaynak ekleyin. Kaynak, iç içe veya bağlı şablonlarda değil yalnızca **Maintemplate. JSON** veya **azuredeploy. JSON** dosyasında olmalıdır.

1. **PID** ön ekiyle (örn., pid-eb7927c8-dd66-43e1-b0cf-c346a422063) sonra GUID değerini girin.

1. Şablonda hata olup olmadığını denetleyin.

1. Şablonu uygun depolarda yeniden yayımlayın.

1. [Şablon DAĞıTıMıNDA GUID başarısını doğrulayın](#verify-the-guid-deployment).

### <a name="sample-resource-manager-template-code"></a>Örnek Kaynak Yöneticisi Şablon kodu

Şablonunuz için izleme kaynaklarını etkinleştirmek üzere kaynaklar bölümüne aşağıdaki ek kaynağı eklemeniz gerekir. Lütfen ana şablon dosyasına eklediğinizde aşağıdaki örnek kodu kendi girdunuzla değiştirdiğinizden emin olun.
Kaynak, iç içe veya bağlı şablonlarda değil yalnızca **Maintemplate. JSON** veya **azuredeploy. JSON** dosyasına eklenmelidir.

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

## <a name="use-the-resource-manager-apis"></a>Kaynak Yöneticisi API 'Lerini kullanma

Bazı durumlarda, Azure hizmetlerini dağıtmak için Kaynak Yöneticisi REST API 'Lerine doğrudan çağrı yapmayı tercih edebilirsiniz. Azure, bu çağrıları etkinleştirmek için [birden çok SDK 'yi destekler](https://docs.microsoft.com/azure/?pivot=sdkstools) . SDK 'Lardan birini kullanabilir veya doğrudan kaynak dağıtmak için REST API 'Lerini çağırabilirsiniz.

Kaynak Yöneticisi şablonu kullanıyorsanız, daha önce açıklanan yönergeleri izleyerek çözümünüzü etiketlemelisiniz. Kaynak Yöneticisi şablonu kullanmıyorsanız ve doğrudan API çağrıları yaparsanız, Azure kaynaklarının kullanımını ilişkilendirmek için dağıtımınızı etiketleyerek yine de kullanabilirsiniz.

### <a name="tag-a-deployment-with-the-resource-manager-apis"></a>Kaynak Yöneticisi API 'Leriyle bir dağıtımı etiketleme

Müşteri kullanımı atısyonu 'nı etkinleştirmek için, API aramalarınızı tasarlarken, istekteki Kullanıcı Aracısı başlığına bir GUID ekleyin. Her teklif veya SKU için GUID 'YI ekleyin. Dizeyi **PID** ön ekiyle biçimlendirin ve iş ortağı tarafından oluşturulan GUID 'yi ekleyin. Kullanıcı aracısına eklemek için GUID biçimine bir örnek aşağıda verilmiştir:

![Örnek GUID biçimi](media/marketplace-publishers-guide/tracking-sample-guid-for-lu-2.PNG)

> [!NOTE]
> Dizenin biçimi önemlidir. **PID** ön eki dahil edilmemişse, verileri sorgulamak mümkün değildir. Farklı SDK 'lar farklı şekilde izler. Bu yöntemi uygulamak için tercih ettiğiniz Azure SDK 'niz için destek ve izleme yaklaşımını gözden geçirin.

#### <a name="example-the-python-sdk"></a>Örnek: Python SDK 'Sı

Python için, **config** özniteliğini kullanın. Özniteliği yalnızca bir UserAgent öğesine ekleyebilirsiniz. Bir örneği aşağıda verilmiştir:

![Özniteliği bir kullanıcı aracısına ekleyin](media/marketplace-publishers-guide/python-for-lu.PNG)

> [!NOTE]
> Her istemci için özniteliğini ekleyin. Genel statik yapılandırma yoktur. Her istemcinin izlenmesini sağlamak için bir istemci fabrikası etiketleyebilir. Daha fazla bilgi için [GitHub 'daki bu istemci fabrikası örneğine](https://github.com/Azure/azure-cli/blob/7402fb2c20be2cdbcaa7bdb2eeb72b7461fbcc30/src/azure-cli-core/azure/cli/core/commands/client_factory.py#L70-L79)bakın.

#### <a name="tag-a-deployment-by-using-the-azure-powershell"></a>Azure PowerShell kullanarak bir dağıtımı etiketleme

Kaynakları Azure PowerShell aracılığıyla dağıtırsanız, aşağıdaki yöntemi kullanarak GUID 'nizi ekleyin:

```powershell
[Microsoft.Azure.Common.Authentication.AzureSession]::ClientFactory.AddUserAgent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")
```

#### <a name="tag-a-deployment-by-using-the-azure-cli"></a>Azure CLı kullanarak bir dağıtımı etiketleme

GUID 'nizi eklemek için Azure CLı kullandığınızda **AZURE_HTTP_USER_AGENT** ortam değişkenini ayarlayın. Bu değişkeni bir komut dosyasının kapsamı içinde ayarlayabilirsiniz. Değişkeni kabuk kapsamı için de genel olarak ayarlayabilirsiniz:

```
export AZURE_HTTP_USER_AGENT='pid-eb7927c8-dd66-43e1-b0cf-c346a422063'
```
Daha fazla bilgi için bkz. [Go için Azure SDK](https://docs.microsoft.com/azure/go/).

## <a name="use-terraform"></a>Terrayform kullanma

Terrayform desteği, Azure sağlayıcısı 'nın 1.21.0 sürümü ile kullanılabilir: [https://github.com/terraform-providers/terraform-provider-azurerm/blob/master/CHANGELOG.md#1210-january-11-2019](https://github.com/terraform-providers/terraform-provider-azurerm/blob/master/CHANGELOG.md#1210-january-11-2019).  Bu destek, çözümünü Terrayform aracılığıyla dağıtan tüm iş ortakları ve Azure sağlayıcısı tarafından dağıtılan ve ölçülen tüm kaynaklar için geçerlidir (sürüm 1.21.0 veya üzeri).

Terrayform için Azure sağlayıcısı, çözümünüz için kullandığınız izleme GUID 'sini belirttiğiniz [*partner_id*](https://www.terraform.io/docs/providers/azurerm/#partner_id) adlı yeni bir isteğe bağlı alan ekledi. Bu alanın değeri, *ARM_PARTNER_ID* ortam değişkeninden de kaynak oluşturulabilir.

```
provider "azurerm" {
          subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          client_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          ……
          # new stuff for ISV attribution
          partner_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"}
```
Müşteri kullanımı attributıon tarafından izlenen Terrayform aracılığıyla dağıtımını almak isteyen iş ortakları şunları yapmanız gerekir:

* GUID oluşturma (her teklif veya SKU için GUID eklenmelidir)
* *Partner_id* değerini GUID olarak ayarlamak Için Azure sağlayıcısını GÜNCELLEŞTIRIN (GUID 'yi "pid-" ile ön düzelmeyin, bunu gerçek GUID olarak ayarlamanız yeterlidir)


## <a name="verify-the-guid-deployment"></a>GUID dağıtımını doğrulama

Şablonunuzu değiştirdikten ve bir test dağıtımı çalıştırdıktan sonra, dağıttığınız ve etiketlediğiniz kaynakları almak için aşağıdaki PowerShell betiğini kullanın.

GUID 'nin Kaynak Yöneticisi şablonunuza başarıyla eklendiğini doğrulamak için betiği kullanabilirsiniz. Betik Kaynak Yöneticisi API 'sine veya Tercaform dağıtımına uygulanmaz.

Azure'da oturum açın. Betiği çalıştırmadan önce doğrulamak istediğiniz dağıtıma sahip aboneliği seçin. Betiği, dağıtımın abonelik bağlamı içinde çalıştırın.

Dağıtımın **GUID** ve **resourceGroup** adı gerekli parametrelerdir.

[Asıl betiği](https://gist.github.com/bmoore-msft/ae6b8226311014d6e7177c5127c7eba1#file-verify-deploymentguid-ps1) GitHub ' da alabilirsiniz.

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

Iş Ortağı Merkezi panonuzda ([https://partner.microsoft.com/dashboard/mpn/analytics/CPP/MicrosoftAzure](https://partner.microsoft.com/dashboard/mpn/analytics/CPP/MicrosoftAzure)) müşteri kullanımı atısyonu için raporu bulabilirsiniz. Raporu görmek için Iş ortağı merkezi kimlik bilgilerinizi kullanarak oturum açın. Raporla veya oturum açma ile ilgili herhangi bir sorunla karşılaşırsanız destek al bölümündeki yönergeyi izleyerek bir destek isteği oluşturun.

Raporu görmek için Iş ortağı Ilişki türü açılan listesinde Izlenen şablon ' u seçin.

![Müşteri kullanım attributıon için rapor](media/marketplace-publishers-guide/customer-usage-attribution-report.png)

## <a name="notify-your-customers"></a>Müşterilerinize bildirme

İş ortakları, müşterilerine müşteri kullanımı attributıon kullanan dağıtımlar hakkında bilgi sağlamalıdır. Microsoft, bu dağıtımlarla ilişkili olan Azure kullanımını iş ortaklarına bildirir. Aşağıdaki örneklerde, bu dağıtımlar hakkında müşterilerinizi bilgilendirmek için kullanabileceğiniz içerikler yer alır. Örneklerde, iş ortağı> \<değerini şirket adınızla değiştirin. İş ortakları, kullanıcıların izlemenin dışında bırakılmasını sağlama seçenekleri de dahil olmak üzere, veri gizliliğiyle ve koleksiyon ilkeleriyle hizalandığından emin olmalıdır.

### <a name="notification-for-resource-manager-template-deployments"></a>Kaynak Yöneticisi Şablon dağıtımları için bildirim

Bu şablonu dağıttığınızda, Microsoft, dağıtılan Azure kaynaklarıyla \<iş ortağı> yazılımını yüklemeyi tanımlayabilir. Microsoft, yazılımı desteklemek için kullanılan Azure kaynaklarını ilişkilendirebiliyor. Microsoft bu bilgileri, ürünleriyle ilgili en iyi deneyimleri sağlamak ve işlerini işletmek için toplar. Veriler, Microsoft 'un adresinde https://www.microsoft.com/trustcenterbulunan gizlilik ilkelerine göre toplanır ve yönetilir.

### <a name="notification-for-sdk-or-api-deployments"></a>SDK veya API dağıtımları için bildirim

İş ortağı> \<yazılımını dağıtırken, Microsoft, dağıtılan Azure kaynaklarıyla \<iş ortağı> yazılımının yüklenmesini tanımlayabilir. Microsoft, yazılımı desteklemek için kullanılan Azure kaynaklarını ilişkilendirebiliyor. Microsoft bu bilgileri, ürünleriyle ilgili en iyi deneyimleri sağlamak ve işlerini işletmek için toplar. Veriler, Microsoft 'un adresinde https://www.microsoft.com/trustcenterbulunan gizlilik ilkelerine göre toplanır ve yönetilir.

## <a name="get-support"></a>Destek alma

Karşılaştığınız sorunlara bağlı olarak iki destek kanalı vardır.

Iş Ortağı Merkezi 'nde müşteri kullanımı attributıon raporunu görmek veya oturum açmak gibi herhangi bir sorunla karşılaşırsanız, Iş ortağı merkezi destek ekibi ile bir destek isteği oluşturun:[https://partner.microsoft.com/support](https://partner.microsoft.com/support)

![](./media/marketplace-publishers-guide/partner-center-log-in-support.png)

Müşteri kullanımı attribuini ayarlama gibi Market ekleme ve/veya müşteri kullanım atısyonu için yardıma ihtiyacınız varsa, aşağıdaki adımları izleyin:

1. [Destek sayfasına](https://go.microsoft.com/fwlink/?linkid=844975)gidin.

1. **Sorun türü**altında **Market ekleme**' yi seçin.

1. Sorununuz için **Kategori** seçin:

   - Kullanım ilişkisi sorunları için **diğer**' i seçin.
   - Azure Marketi ile ilgili erişim sorunları için **erişim sorunu**' nı seçin.

     ![Sorun kategorisini seçin](media/marketplace-publishers-guide/lu-article-incident.png)

1. **Isteği Başlat**' ı seçin.

1. Sonraki sayfada gerekli değerleri girin. **Devam**'ı seçin.

1. Sonraki sayfada gerekli değerleri girin.

   > [!IMPORTANT]
   > **Olay başlığı** kutusuna **ISV kullanımı izleme**girin. Sorununuzu ayrıntılı olarak tanıtın.

   ![Olay başlığı için ISV kullanımı Izleme girin](media/marketplace-publishers-guide/guid-dev-center-help-hd%201.png)

1. Formu doldurun ve **Gönder**' i seçin.

Ayrıca, teknik satış, dağıtım ve uygulama geliştirme senaryolarına yönelik olarak bir Microsoft Iş ortağı teknik danışmanında, müşteri kullanımı atışının anlaşılması ve dahil olması için teknik kılavuzluk de alabilirsiniz.

### <a name="how-to-submit-a-technical-consultation-request"></a>Teknik bir danışmandaki istek gönderme

1. [Iş ortağı teknik hizmetlerini](https://partner.microsoft.com/training/partner-technical-services)ziyaret edin.
1. Bulut altyapısı ve Yönetimi ' ni seçin ve teknik yolculuğun görüntüleneceği yeni bir sayfa açılır.
1. Dağıtım Hizmetleri altında istek gönder düğmesine tıklayın
1. MSA (MPN hesabı) veya AAD 'nizi (Iş ortağı Pano hesabı) kullanarak oturum açın; oturum açma kimlik bilgileriniz temelinde bir çevrimiçi istek formu açılır:
    * İletişim bilgilerini doldurun/gözden geçirin.
    * Danışmanın ayrıntıları önceden doldurulmuş veya açılan kutudan seçim olabilir.
    * Sorun için bir başlık ve açıklama girin (mümkün olduğunca fazla ayrıntı sağlayın).
1. Gönder’e tıklayın

[Teknik satış ve dağıtım hizmetlerini kullanırken](https://support.microsoft.com/help/3121537/using-technical-presales-and-deployment-services#request%20a%20technical%20consultation)ekran görüntüleriyle birlikte adım adım yönergeleri görüntüleyin.

### <a name="whats-next"></a>Sırada ne var?

İhtiyaçlarınızı karşılamak için bir çağrı kurmak üzere Microsoft Iş ortağı teknik danışman ile iletişim kurulacaksınız.

## <a name="faq"></a>SSS

**GUID 'yi şablona eklemenin avantajı nedir?**

Microsoft, iş ortakları, çözümlerinin ve öngörülerinin müşterilerin etkilenme kullanımlarıyla ilgili bir görünümünü sunar. Hem Microsoft hem de iş ortağı, satış takımları arasında daha yakın bir katılım sağlamak için bu bilgileri kullanabilir. Hem Microsoft hem de iş ortağı, tek bir iş ortağının Azure büyümesi üzerindeki etkisinden daha tutarlı bir görünüm sağlamak için verileri kullanabilir.

**Bir GUID eklendikten sonra değişiklik yapılabilir mi?**

Evet, bir müşteri veya uygulama ortağı şablonu özelleştirebilir ve GUID 'YI değiştirebilir veya kaldırabilir. Bu iş ortaklarının, GUID 'de kaldırma veya düzenleme yapılmasını önleyen, kaynak ve GUID 'nin, müşterileri ve iş ortakları için kaynak ve GUID rolünü etkin bir şekilde tanımlamasını öneririz. GUID 'nin değiştirilmesi yalnızca yeni, var olan dağıtımları ve kaynakları etkiler.

**GitHub gibi Microsoft olmayan bir depodan dağıtılan şablonları izleyebilir miyim?**

Evet, şablon dağıtıldığında GUID mevcut olduğu sürece kullanım izlenir. İş ortakları GUID 'Leri kaydetmeye devam etmelidir.

**Müşteri Raporlama da alıyor mu?**

Müşteriler, Azure portal içinde bireysel kaynakların veya müşteri tanımlı kaynak gruplarının kullanımını izleyebilir. Müşteriler GUID tarafından kesilen kullanımı görmez.

**Bu metodolojinin dijital Iş ortağı (DPOR) ile aynı mı?**

Dağıtım ve kullanımı bir iş ortağının çözümüne bağlamak için bu yeni yöntem bir iş ortağı çözümünü Azure kullanımına bağlama mekanizması sağlar. DPOR, bir danışmanlık (Sistem Tümleştirici) veya yönetim (yönetilen hizmet sağlayıcısı) ortağını bir müşterinin Azure aboneliğiyle ilişkilendirmek üzere tasarlanmıştır.

**Azure depolama 'nın GUID Oluşturucu formunu kullanmanın avantajı nedir?**

Azure depolama 'nın GUID Oluşturucu formu, gerekli biçimdeki bir GUID oluşturmak için garanti edilir. Ayrıca, Azure Storage 'ın veri düzlemi izleme yöntemlerinden herhangi birini kullanıyorsanız, Market denetim düzlemi izleme için aynı GUID 'den yararlanabilirsiniz. Bu, ayrı GUID 'LERI sürdürmenize gerek kalmadan, Iş ortağı için birleştirilmiş bir Birleşik GUID özelliğinden yararlanmanızı sağlar.

**Azure Marketi 'nde bir çözüm şablonu teklifi için özel, özel bir VHD kullanabilir miyim?**

Hayır, şu yapılamıyor. Sanal makine görüntüsünün Azure Marketi 'nden gelmesi gerekir, bkz.: [https://docs.microsoft.com/azure/marketplace/marketplace-virtual-machines](https://docs.microsoft.com/azure/marketplace/marketplace-virtual-machines).

Özel VHD 'nizi kullanarak Market 'te bir VM teklifi oluşturabilir ve bunu hiçbir kimse görememesi için özel olarak işaretleyebilirsiniz. Sonra çözüm şablonunuzda bu VM 'ye başvurun.

**Ana şablon için *Contentversion* özelliği güncelleştirilemedi mi?**

Büyük olasılıkla bir hata, şablon, daha eski contentVersion 'ı bir nedenden dolayı daha önce bekleyen başka bir şablondan TemplateLink kullanılarak dağıtıldığında oluşan bir hatadır. Geçici çözüm, meta veri özelliğini kullanmaktır:

```
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "metadata": {
        "contentVersion": "1.0.1.0"
    },
    "parameters": {
```
