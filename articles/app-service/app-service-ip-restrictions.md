---
title: Azure App Service erişim kısıtlamaları
description: Erişim kısıtlamalarını ayarlayarak Azure App Service uygulamanızın güvenliğini nasıl sağlayacağınızı öğrenin.
author: ccompy
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.topic: article
ms.date: 12/17/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: fea189952b1452c680255ceb99e38609775a8bd6
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102502697"
---
# <a name="set-up-azure-app-service-access-restrictions"></a>Azure App Service erişim kısıtlamalarını ayarlama

Erişim kısıtlamalarını ayarlayarak, uygulamanıza ağ erişimini denetleyen, öncelik sırasına sahip bir izin verme/reddetme listesi tanımlayabilirsiniz. Listede IP adresleri veya Azure sanal ağ alt ağları bulunabilir. Bir veya daha fazla giriş olduğunda, listenin sonunda örtülü *reddetme tümünün* vardır.

Erişim kısıtlama özelliği, tüm Azure App Service barındırılan iş yükleri ile birlikte kullanılabilir. İş yükleri Web Apps, API Apps, Linux uygulamaları, Linux kapsayıcı uygulamaları ve Işlevler içerebilir.

Uygulamanıza bir istek yapıldığında, KIMDEN adresi erişim kısıtlama listenizdeki kurallara göre değerlendirilir. KIMDEN adresi Microsoft. Web 'e hizmet uç noktaları ile yapılandırılmış bir alt ağdaysa, kaynak alt ağ erişim kısıtlama listenizdeki sanal ağ kurallarına göre karşılaştırılır. Listedeki kurallara göre adrese erişime izin verilmiyorsa, hizmet bir [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) durum koduyla yanıt verir.

Erişim kısıtlaması özelliği, kodunuzun çalıştırıldığı çalışanların ana bilgisayarlarının yukarı akış olan App Service ön uç rollerinde uygulanır. Bu nedenle, erişim kısıtlamaları etkin bir ağ erişim denetimi listeleridir (ACL).

Bir Azure sanal ağından Web uygulamanıza erişimi kısıtlama özelliği, [hizmet uç noktaları][serviceendpoints]tarafından etkinleştirilir. Hizmet uç noktaları ile, seçilen alt ağlardan çok kiracılı bir hizmete erişimi kısıtlayabilirsiniz. App Service Ortamı barındırılan uygulamalarla trafiği kısıtlamak işe yaramamaktadır. Bir App Service Ortamı kullanıyorsanız, IP adresi kurallarını uygulayarak uygulamanıza erişimi kontrol edebilirsiniz.

> [!NOTE]
> Hizmet uç noktaları hem ağ tarafında hem de üzerinde etkinleştirildikleri Azure hizmeti için etkinleştirilmelidir. Hizmet uç noktalarını destekleyen Azure hizmetlerinin bir listesi için bkz. [sanal ağ hizmeti uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md).
>

:::image type="content" source="media/app-service-ip-restrictions/access-restrictions-flow.png" alt-text="Erişim kısıtlamaları akışının diyagramı.":::

## <a name="manage-access-restriction-rules-in-the-portal"></a>Portalda erişim kısıtlama kurallarını yönetme

Uygulamanıza bir erişim kısıtlama kuralı eklemek için aşağıdakileri yapın:

1. Azure portalında oturum açın.

1. Sol bölmede **ağ iletişimi**' ni seçin.

1. **Ağ** bölmesinde, **erişim kısıtlamaları**' nın altında, **erişim kısıtlamalarını Yapılandır**' ı seçin.

    :::image type="content" source="media/app-service-ip-restrictions/access-restrictions.png" alt-text="Azure portal App Service ağ seçenekleri bölmesinin ekran görüntüsü.":::

1. **Erişim kısıtlamaları** sayfasında, uygulamanız için tanımlanan erişim kısıtlama kuralları listesini gözden geçirin.

   :::image type="content" source="media/app-service-ip-restrictions/access-restrictions-browse.png" alt-text="Seçili uygulama için tanımlanan erişim kısıtlama kurallarının listesini gösteren Azure portal erişim kısıtlamaları sayfasının ekran görüntüsü.":::

   Listede, uygulamaya uygulanan tüm geçerli kısıtlamalar görüntülenir. Uygulamanızda bir sanal ağ kısıtlaması varsa, tablo, hizmet uç noktalarının Microsoft. Web için etkinleştirilip etkinleştirilmediğini gösterir. Uygulamanızda tanımlı bir kısıtlama yoksa, uygulamaya her yerden erişilebilir.

### <a name="add-an-access-restriction-rule"></a>Erişim kısıtlama kuralı ekleme

Uygulamanıza bir erişim kısıtlama kuralı eklemek için, **erişim kısıtlamaları** bölmesinde **Kural Ekle**' yi seçin. Bir kural ekledikten sonra, hemen etkili olur. 

Kurallar öncelik sütunundaki en düşük sayıdan **başlayarak öncelik sırasına** göre zorlanır. Tek bir kural ekledikten sonra örtülü *reddetme tümünün* etkin olması gerekir.

**Erişim kısıtlaması Ekle** bölmesinde, bir kural oluşturduğunuzda şunları yapın:

1. **Eylem** altında **Izin ver** veya **Reddet**' i seçin.  

   :::image type="content" source="media/app-service-ip-restrictions/access-restrictions-ip-add.png?v2" alt-text="' Erişim kısıtlaması Ekle ' bölmesinin ekran görüntüsü.":::

1. İsteğe bağlı olarak, kuralın adını ve açıklamasını girin.
1. **Öncelik** kutusuna bir öncelik değeri girin.
1. **Tür** açılan listesinde, kural türünü seçin.

Farklı kural türleri aşağıdaki bölümlerde açıklanmıştır.

> [!NOTE]
> - 512 erişim kısıtlama kuralları sınırlaması vardır. 512 ' den fazla erişim kısıtlama kuralına ihtiyacınız varsa, Azure ön kapısı, Azure App Gateway veya alternatif bir WAF gibi tek başına bir güvenlik ürünü yüklemenizi öneririz.
>
#### <a name="set-an-ip-address-based-rule"></a>IP adresi tabanlı kural ayarlama

Önceki bölümde özetlenen, ancak aşağıdaki ek bilgilerle yordamı izleyin:
* Adım 4 ' te, **tür** açılan listesinde **IPv4** veya **IPv6**' yı seçin. 

**IP adresi bloğunu** , hem IPv4 hem de IPv6 adresleri için sınıfsız Inter-Domain YÖNLENDIRME (CIDR) gösteriminde belirtin. Bir adres belirtmek için, *1.2.3.4/32* gibi bir şey kullanabilirsiniz; burada ilk dört sekizli IP adresinizi ve */32* ' in maskesini temsil eder. Tüm adresler için IPv4 CıDR gösterimi 0.0.0.0/0 ' dır. CıDR gösterimi hakkında daha fazla bilgi edinmek için bkz. [sınıfsız Inter-Domain yönlendirme](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing). 

#### <a name="set-a-service-endpoint-based-rule"></a>Hizmet uç noktası tabanlı kural ayarlama

* Adım 4 ' te, **tür** açılan listesinde **sanal ağ**' ı seçin.

   :::image type="content" source="media/app-service-ip-restrictions/access-restrictions-vnet-add.png?v2" alt-text="Sanal ağ türü seçili olan ' kısıtlama Ekle ' bölmesinin ekran görüntüsü.":::

Erişimi kısıtlamak istediklerinizi eşleşen **abonelik**, **sanal ağ** ve **alt ağ** açılan listelerini belirtin.

Hizmet uç noktalarını kullanarak, seçili Azure sanal ağ alt ağlarına erişimi kısıtlayabilirsiniz. Hizmet uç noktaları, seçtiğiniz alt ağ için Microsoft. Web ile zaten etkin değilse, **eksik Microsoft. Web hizmeti uç noktalarını yoksay** onay kutusunu seçmediğiniz takdirde bunlar otomatik olarak etkinleştirilir. Uygulama üzerinde hizmet uç noktalarını etkinleştirmek isteyebileceğiniz, ancak alt ağda olmayan bir senaryo, temel olarak bunları alt ağda etkinleştirme izinlerine sahip olmanıza bağlı olarak değişir. 

Alt ağda hizmet uç noktalarını etkinleştirmek için başka bir kişiye ihtiyacınız varsa, **yok sayma Microsoft. Web hizmeti uç noktaları** onay kutusunu seçin. Uygulamanız, olasılığına ' deki hizmet uç noktaları için, alt ağda daha sonra etkinleştirilmesini sağlayacak şekilde yapılandırılır. 

App Service Ortamı çalıştıran uygulamalara erişimi kısıtlamak için hizmet uç noktalarını kullanamazsınız. Uygulamanız bir App Service Ortamı olduğunda, IP erişim kuralları uygulayarak bu erişime erişimi denetleyebilirsiniz. 

Hizmet uç noktaları ile uygulamanızı uygulama ağ geçitleri veya diğer Web uygulaması güvenlik duvarı (WAF) cihazları ile yapılandırabilirsiniz. Ayrıca, güvenli arka uçlar ile çok katmanlı uygulamalar da yapılandırabilirsiniz. Daha fazla bilgi için bkz. [ağ özellikleri ve App Service](networking-features.md) ve [hizmet uç noktalarıyla tümleştirme Application Gateway](networking/app-gateway-with-service-endpoints.md).

> [!NOTE]
> - Hizmet uç noktaları şu anda IP Güvenli Yuva Katmanı (SSL) sanal IP (VIP) kullanan Web uygulamaları için desteklenmemektedir.
>
#### <a name="set-a-service-tag-based-rule-preview"></a>Hizmet etiketi tabanlı kural ayarlama (Önizleme)

* 4. adım 'da, **tür** açılan listesinde **hizmet etiketi (Önizleme)** öğesini seçin.

   :::image type="content" source="media/app-service-ip-restrictions/access-restrictions-service-tag-add.png" alt-text="Hizmet etiketi türü seçili olan ' kısıtlama Ekle ' bölmesinin ekran görüntüsü.":::

Her hizmet etiketi, Azure hizmetlerinden gelen IP aralıklarının listesini temsil eder. Bu hizmetlerin bir listesi ve belirli aralıklara yönelik bağlantılar [hizmet etiketi belgelerinde][servicetags]bulunabilir.

Aşağıdaki hizmet etiketleri listesi, önizleme aşamasında erişim kısıtlama kurallarında desteklenir:
* ActionGroup
* AzureCloud
* Azurecognivesearch
* AzureConnectors
* AzureEventGrid
* Azurefrontkapısı. arka uç
* AzureMachineLearning
* AzureSignalR
* AzureTrafficManager
* LogicApps
* ServiceFabric

### <a name="edit-a-rule"></a>Bir kuralı düzenleme

1. Var olan bir erişim kısıtlama kuralını düzenlemeye başlamak için, **erişim kısıtlamaları** sayfasında, düzenlemek istediğiniz kuralı seçin.

1. **Erişim kısıtlaması Düzenle** bölmesinde, değişikliklerinizi yapın ve ardından **kuralı Güncelleştir**' i seçin. Düzenlemeler, öncelik Sıralamalı değişiklikler de dahil olmak üzere hemen etkili olur.

   :::image type="content" source="media/app-service-ip-restrictions/access-restrictions-ip-edit.png?v2" alt-text="Var olan bir erişim kısıtlama kuralına ait alanları gösteren Azure portal, ' erişim kısıtlamasını Düzenle ' bölmesinin ekran görüntüsü.":::

   > [!NOTE]
   > Bir kuralı düzenlediğinizde, kural türleri arasında geçiş yapamazsınız. 

### <a name="delete-a-rule"></a>Kuralı silme

Bir kuralı silmek için, **erişim kısıtlamaları** sayfasında, silmek istediğiniz kuralın yanındaki üç nokta (**...**) simgesini seçin ve ardından **Kaldır**' ı seçin.

:::image type="content" source="media/app-service-ip-restrictions/access-restrictions-delete.png" alt-text="' Erişim kısıtlamaları ' sayfasının, silinecek erişim kısıtlama kuralının yanındaki ' Kaldır ' üç nokta simgesini gösteren ekran görüntüsü.":::

## <a name="access-restriction-advanced-scenarios"></a>Erişim kısıtlaması gelişmiş senaryolar
Aşağıdaki bölümlerde, erişim kısıtlamalarını kullanan bazı gelişmiş senaryolar açıklanır.
### <a name="block-a-single-ip-address"></a>Tek bir IP adresini engelle

İlk erişim kısıtlama kuralınızı eklediğinizde, hizmet önceliği 2147483647 olan bir açık *Tümünü reddetme* kuralı ekler. Uygulamada, *tüm açık reddetme* kuralı yürütülecek son kuraldır ve *izin verme* kuralı tarafından açıkça ızın verilmeyen herhangi bir IP adresine erişimi engeller.

Tek bir IP adresini veya bir IP adresi bloğunu açıkça engellemek istediğiniz, ancak başka her şeye erişime izin veren bir senaryo için, bir açık *Izin verme* kuralına ekleyin.

:::image type="content" source="media/app-service-ip-restrictions/block-single-address.png" alt-text="Tek bir engellenen IP adresini gösteren Azure portal ' erişim kısıtlamaları ' sayfasının ekran görüntüsü.":::

### <a name="restrict-access-to-an-scm-site"></a>Bir SCM sitesine erişimi kısıtlama 

Uygulamanıza erişimi denetleyebilmenin yanı sıra, uygulamanız tarafından kullanılan SCM sitesine erişimi kısıtlayabilirsiniz. SCM sitesi hem Web dağıtımı uç noktası hem de kudu konsoludur. Uygulamadan SCM sitesine erişim kısıtlamaları atayabilir veya hem uygulama hem de SCM sitesi için aynı kısıtlama kümesini kullanabilirsiniz. Onay kutusuyla **aynı kısıtlamaları \<app name>** seçtiğinizde, her şey göz aşımına uğrar. Onay kutusunun işaretini kaldırırsanız SCM sitenizin ayarları yeniden uygulanır. 

:::image type="content" source="media/app-service-ip-restrictions/access-restrictions-scm-browse.png" alt-text="SCM sitesi veya uygulama için hiçbir erişim kısıtlaması ayarlanmediğini gösteren Azure portal ' erişim kısıtlamaları ' sayfasının ekran görüntüsü.":::

### <a name="restrict-access-to-a-specific-azure-front-door-instance-preview"></a>Belirli bir Azure ön kapısı örneğine erişimi kısıtla (Önizleme)
Azure ön kapılarından uygulamanıza giden trafik, Azurefrontkapı. arka uç hizmeti etiketinde tanımlanan iyi bilinen bir IP aralığı kümesinden kaynaklanır. Bir hizmet etiketi kısıtlama kuralı kullanarak, trafiği yalnızca Azure ön kapısından kaynaklanacak şekilde kısıtlayabilirsiniz. Trafiğin yalnızca belirli örnekten kaynaklandığını sağlamak için, Azure ön kapısının gönderdiği benzersiz http üstbilgisine göre gelen istekleri daha fazla filtrelemeniz gerekir. Önizleme süresince bunu PowerShell veya REST/ARM ile elde edebilirsiniz. 

* PowerShell örneği (ön kapı KIMLIĞI Azure portal):

   ```azurepowershell-interactive
    $frontdoorId = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    Add-AzWebAppAccessRestrictionRule -ResourceGroupName "ResourceGroup" -WebAppName "AppName" `
      -Name "Front Door example rule" -Priority 100 -Action Allow -ServiceTag AzureFrontDoor.Backend `
      -HttpHeader @{'x-azure-fdid' = $frontdoorId}
    ```
## <a name="manage-access-restriction-rules-programmatically"></a>Erişim kısıtlama kurallarını programlı olarak yönetme

Aşağıdakilerden birini yaparak erişim kısıtlamalarını programlı bir şekilde ekleyebilirsiniz: 

* [Azure CLI](/cli/azure/webapp/config/access-restriction)'yi kullanın. Örnek:
   
  ```azurecli-interactive
  az webapp config access-restriction add --resource-group ResourceGroup --name AppName \
    --rule-name 'IP example rule' --action Allow --ip-address 122.133.144.0/24 --priority 100
  ```

* [Azure PowerShell](/powershell/module/Az.Websites/Add-AzWebAppAccessRestrictionRule)kullanın. Örnek:


  ```azurepowershell-interactive
  Add-AzWebAppAccessRestrictionRule -ResourceGroupName "ResourceGroup" -WebAppName "AppName"
      -Name "Ip example rule" -Priority 100 -Action Allow -IpAddress 122.133.144.0/24
  ```
   > [!NOTE]
   > Hizmet etiketleri, http üstbilgileri veya çok kaynaklı kurallarla çalışma en az sürüm 5.1.0 gerektirir. Yüklü modülün sürümünü,: **Get-ınstalınstallatem-Name az** ile doğrulayabilirsiniz.

Ayrıca, aşağıdakilerden birini yaparak değerleri el ile de ayarlayabilirsiniz:

* Azure Resource Manager içindeki uygulama yapılandırmasına bir [Azure REST API](/rest/api/azure/) Put işlemi kullanın. Bu bilgilerin Azure Resource Manager konumu:

  management.azure.com/subscriptions/**ABONELIK kimliği**/ResourceGroups/**kaynak grupları**/Providers/Microsoft.Web/Sites/**Web uygulaması adı**/config/Web? api-Version = 2020-06-01

* Kaynak Yöneticisi şablonu kullanın. Örnek olarak, gerekli JSON 'u eklemek için resources.azure.com kullanabilir ve ıpsecurityrestrictions bloğunu düzenleyebilirsiniz.

  Önceki örnek için JSON sözdizimi şöyledir:

  ```json
  {
    "properties": {
      "ipSecurityRestrictions": [
        {
          "ipAddress": "122.133.144.0/24",
          "action": "Allow",
          "priority": 100,
          "name": "IP example rule"
        }
      ]
    }
  }
  ```
  Hizmet etiketi ve http üstbilgisi kısıtlaması kullanan gelişmiş bir örnek için JSON sözdizimi şöyledir:
  ```json
  {
    "properties": {
      "ipSecurityRestrictions": [
        {
          "ipAddress": "AzureFrontDoor.Backend",
          "tag": "ServiceTag",
          "action": "Allow",
          "priority": 100,
          "name": "Azure Front Door example",
          "headers": {
            "x-azure-fdid": [
              "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
            ]
          }
        }
      ]
    }
  }
  ```
## <a name="set-up-azure-functions-access-restrictions"></a>Azure Işlevleri erişim kısıtlamalarını ayarlama

App Service planlarla aynı işlevselliğe sahip işlev uygulamaları için de erişim kısıtlamaları vardır. Erişim kısıtlamalarını etkinleştirdiğinizde, izin verilmeyen IP 'Ler için Azure portal kod düzenleyicisini de devre dışı bırakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Işlevleri için erişim kısıtlamaları](../azure-functions/functions-networking-options.md#inbound-access-restrictions)  
[Hizmet uç noktaları ile Application Gateway tümleştirme](networking/app-gateway-with-service-endpoints.md)

<!--Links-->
[serviceendpoints]: ../virtual-network/virtual-network-service-endpoints-overview.md
[servicetags]: ../virtual-network/service-tags-overview.md