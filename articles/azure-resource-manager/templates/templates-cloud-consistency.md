---
title: Şablonları bulutlar arasında yeniden kullanma
description: Farklı bulut ortamları için tutarlı bir şekilde çalışan Azure Kaynak Yöneticisi şablonları geliştirin. Azure Yığını için yeni şablonlar oluşturun veya varolan şablonları güncelleştirin.
author: marcvaneijk
ms.topic: conceptual
ms.date: 12/09/2018
ms.author: mavane
ms.custom: seodec18
ms.openlocfilehash: c5095efef5d4bef44993bdd9cd52dbdef17378a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80156115"
---
# <a name="develop-arm-templates-for-cloud-consistency"></a>Bulut tutarlılığı için ARM şablonları geliştirin

[!INCLUDE [requires-azurerm](../../../includes/requires-azurerm.md)]

Azure'un önemli bir avantajı tutarlılıktır. Bir yer için geliştirme yatırımları başka bir yeniden kullanılabilir. Azure Kaynak Yöneticisi (ARM) şablonu, dağıtımlarınızı genel Azure, Azure egemen bulutlar ve Azure Yığını gibi ortamlarda tutarlı ve yinelenebilir hale getirir. Ancak, şablonları bulutlar arasında yeniden kullanmak için, bu kılavuzun açıkladığı gibi buluta özgü bağımlılıkları göz önünde bulundurmanız gerekir.

Microsoft, birçok konumda akıllı, kuruluşa hazır bulut hizmetleri sunar:

* Dünyanın dört bir yanındaki bölgelerde Microsoft tarafından yönetilen veri merkezlerinden oluşan büyüyen bir ağ tarafından desteklenen küresel Azure platformu.
* Azure Almanya, Azure Devlet ve Azure China 21Vianet gibi yalıtılmış egemen bulutlar. Egemen bulutlar, küresel Azure müşterilerinin erişebilen aynı harika özelliklerin çoğuyla tutarlı bir platform sağlar.
* Azure Yığını, kuruluşunuzun veri merkezinden Azure hizmetleri sunmanıza olanak tanıyan karma bir bulut platformudur. Kuruluşlar Azure Yığını'nı kendi veri merkezlerinde ayarlayabilir veya tesislerinde (bazen barındırılan bölgeler olarak da bilinir) Azure Yığını çalıştıran hizmet sağlayıcılarının Azure Hizmetlerini tüketebilir.

Azure Kaynak Yöneticisi, tüm bu bulutların özünde, çok çeşitli kullanıcı arabirimlerinin Azure platformuyla iletişim kurmasına olanak tanıyan bir API sağlar. Bu API, kod olarak güçlü altyapı özellikleri sağlar. Azure bulut platformunda kullanılabilen her tür kaynak Azure Kaynak Yöneticisi ile dağıtılabilir ve yapılandırılabilir. Tek bir şablonla, tüm uygulamanızı operasyonel bir son durumuna dağıtabilir ve yapılandırabilirsiniz.

![Azure ortamları](./media/templates-cloud-consistency/environments.png)

Veri merkezinizdeki küresel Azure, egemen bulutlar, barındırılan bulutlar ve bulutun tutarlılığı Azure Kaynak Yöneticisi'nden yararlanmanıza yardımcı olur. Şablon tabanlı kaynak dağıtımı ve yapılandırmasını ayarladığınızda geliştirme yatırımlarınızı bu bulutlar da yeniden kullanabilirsiniz.

Ancak, küresel, egemen, barındırılan ve hibrit bulutlar tutarlı hizmetler sağlasa da, tüm bulutlar aynı değildir. Sonuç olarak, yalnızca belirli bir bulutta kullanılabilen özelliklere bağlı bağımlılıkları olan bir şablon oluşturabilirsiniz.

Bu kılavuzun geri kalanı, Azure Yığını için yeni veya güncellenmiş ARM şablonları geliştirmeyi planlarken göz önünde bulundurulması gereken alanları açıklar. Genel olarak, denetim listeniz aşağıdaki öğeleri içermelidir:

* Şablonunuzdaki işlevlerin, uç noktaların, hizmetlerin ve diğer kaynakların hedef dağıtım konumlarında kullanılabildiğini doğrulayın.
* İç içe şablonları ve yapılandırma yapılarını erişilebilir konumlarda depolayın ve bulutlar arasında erişim sağlayın.
* Sabit kodlama bağlantıları ve öğeleri yerine dinamik başvurular kullanın.
* Hedef bulutlarda kullandığınız şablon parametrelerinin çalışmasını sağlayın.
* Kaynağa özgü özelliklerin hedef bulutlarda kullanılabildiğini doğrulayın.

ARM şablonlarına giriş için [Bkz. Şablon dağıtımı.](overview.md)

## <a name="ensure-template-functions-work"></a>Şablon işlevlerinin çalışmasını sağlayın

ARM şablonunun temel sözdizimi JSON'dur. Şablonlar, sözdizimini ifadeler ve işlevlerle genişleten bir JSON kümesi kullanır. Şablon dil işlemcisi ek şablon işlevlerini desteklemek için sık sık güncelleştirilir. Kullanılabilir şablon işlevlerinin ayrıntılı bir açıklaması için [ARM şablon işlevlerine](template-functions.md)bakın.

Azure Kaynak Yöneticisi'ne tanıtılan yeni şablon işlevleri egemen bulutlarda veya Azure Yığını'nda hemen kullanılamaz. Bir şablonu başarıyla dağıtmak için, şablonda başvurulan tüm işlevlerin hedef bulutta kullanılabilir olması gerekir.

Azure Kaynak Yöneticisi yetenekleri her zaman önce global Azure'a tanıtılır. Azure Yığını'nda yeni tanıtılan şablon işlevlerinin kullanılabilir olup olmadığını doğrulamak için aşağıdaki PowerShell komut dosyasını kullanabilirsiniz:

1. GitHub deposunun bir klonu yapın: [https://github.com/marcvaneijk/arm-template-functions](https://github.com/marcvaneijk/arm-template-functions).

1. Deponun yerel bir klonunu aldıktan sonra PowerShell ile hedefin Azure Kaynak Yöneticisi'ne bağlanın.

1. psm1 modüllerini içeri aktarın ve Test-AzureRmTemplateFunctions cmdlet'i çalıştırın:

   ```powershell
   # Import the module
   Import-module <path to local clone>\AzTemplateFunctions.psm1

   # Execute the Test-AzureRmTemplateFunctions cmdlet
   Test-AzureRmTemplateFunctions -path <path to local clone>
   ```

Komut dosyası, her biri yalnızca benzersiz şablon işlevleri içeren birden çok, en aza indirgenen şablonları dağır. Komut dosyasının çıktısı desteklenen ve kullanılamayan şablon işlevlerini bildirir.

## <a name="working-with-linked-artifacts"></a>Bağlantılı eserlerle çalışma

Şablon, bağlı yapılara başvurular içerebilir ve başka bir şablona bağlantı veren bir dağıtım kaynağı içerebilir. Bağlantılı şablonlar (iç içe şablon olarak da adlandırılır) çalışma zamanında Kaynak Yöneticisi tarafından alınır. Şablon, sanal makine (VM) uzantıları için yapılan yapılara yapılan başvuruları da içerebilir. Bu yapılar, şablon dağıtımı sırasında VM uzantısı yapılandırması için VM içinde çalışan VM uzantısı tarafından alınır.

Aşağıdaki bölümlerde, ana dağıtım şablonuna dışarıdan gelen yapılar içeren şablonlar geliştirirken bulut tutarlılığı yla ilgili hususlar açıklanmaktadır.

### <a name="use-nested-templates-across-regions"></a>Bölgeler arasında iç içe kullanılan şablonları kullanma

Şablonlar, her biri belirli bir amaca sahip olan ve dağıtım senaryoları arasında yeniden kullanılabilen küçük, yeniden kullanılabilir şablonlara ayrıştırılabilir. Dağıtımı yürütmek için, ana veya ana şablon olarak bilinen tek bir şablon belirtirsiniz. Sanal ağlar, Sanal Kaynaklar ve web uygulamaları gibi dağıtılacak kaynakları belirtir. Ana şablon, başka bir şablona bağlantı da içerebilir, yani şablonları iç içe çekebilirsiniz. Aynı şekilde, iç içe geçen bir şablon diğer şablonlara bağlantılar içerebilir. En fazla beş kat derin yuva yapabilirsiniz.

Aşağıdaki kod, templateLink parametresinin iç içe bir şablona nasıl atıfta bulunduğunu gösterir:

```json
"resources": [
  {
     "type": "Microsoft.Resources/deployments",
     "apiVersion": "2017-05-10",
     "name": "linkedTemplate",
     "properties": {
       "mode": "incremental",
       "templateLink": {
          "uri":"https://mystorageaccount.blob.core.windows.net/AzureTemplates/vNet.json",
          "contentVersion":"1.0.0.0"
       }
     }
  }
]
```

Azure Kaynak Yöneticisi çalışma zamanında ana şablonu değerlendirir ve iç içe geçen her şablonu alır ve değerlendirir. İç içe geçirilen tüm şablonlar alındıktan sonra şablon düzleştirilmiş ve daha fazla işleme başlatılır.

### <a name="make-linked-templates-accessible-across-clouds"></a>Bağlantılı şablonları bulutlar arasında erişilebilir hale getirme

Kullandığınız bağlantılı şablonların nerede ve nasıl depolandığınızı göz önünde bulundurun. Çalışma zamanında Azure Kaynak Yöneticisi, bağlantılı şablonlara doğrudan erişim gerektirir ve bu nedenle de doğrudan erişim gerektirir. Sık karşılaşılan bir uygulama, iç içe olan şablonları depolamak için GitHub'ı kullanmaktır. GitHub deposu, bir URL üzerinden herkese açık olarak erişilebilen dosyalar içerebilir. Bu teknik genel bulut ve egemen bulutlar için iyi çalışsa da, bir Azure Yığını ortamı bir şirket ağında veya bağlantısı kesilmiş bir uzak konumda, giden Internet erişimi olmadan bulunabilir. Bu gibi durumlarda, Azure Kaynak Yöneticisi iç içe olan şablonları almakta başarısız olur.

Bulutlar arası dağıtımlar için daha iyi bir uygulama, bağlantılı şablonlarınızı hedef bulut için erişilebilir bir konumda depolamaktır. İdeal olarak tüm dağıtım yapıları sürekli tümleştirme/sürekli geliştirme (CI/CD) ardışık boru hattında korunur ve dağıtılır. Alternatif olarak, iç içe olan şablonları Azure Kaynak Yöneticisi'nin bunları alabileceği bir blob depolama kapsayıcısında depolayabilirsiniz.

Her buluttaki blob depolama alanı farklı bir bitiş noktası tam nitelikli etki alanı adı (FQDN) kullandığından, şablonu bağlantılı şablonların konumuyla iki parametreyle yapılandırın. Parametreler dağıtım zamanında kullanıcı girişini kabul edebilir. Şablonlar genellikle birden çok kişi tarafından yazar ve paylaşılır, bu nedenle en iyi yöntem bu parametreler için standart bir ad kullanmaktır. Adlandırma kuralları, şablonların bölgeler, bulutlar ve yazarlar arasında daha kullanılabilir hale getirilebilir hale getirmeye yardımcı olur.

Aşağıdaki kodda, `_artifactsLocation` dağıtımla ilgili tüm yapıları içeren tek bir konumu işaret etmek için kullanılır. Varsayılan değer sağlandı. Dağıtım zamanında, giriş `_artifactsLocation`değeri belirtilmemişse, varsayılan değer kullanılır. Giriş `_artifactsLocationSasToken` olarak `sasToken`kullanılır. Varsayılan değer, güvenli olmayan senaryolar `_artifactsLocation` için boş bir dize olmalıdır — örneğin, genel bir GitHub deposu.

```json
"parameters": {
  "_artifactsLocation": {
    "type": "string",
    "metadata": {
      "description": "The base URI where artifacts required by this template are located."
    },
    "defaultValue": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-custom-script-windows/"
  },
  "_artifactsLocationSasToken": {
    "type": "securestring",
    "metadata": {
      "description": "The sasToken required to access _artifactsLocation."
    },
    "defaultValue": ""
  }
}
```

Şablon boyunca, bağlantılar temel URI `_artifactsLocation` (parametre) bir artefakt göreli yol `_artifactsLocationSasToken`ve . Aşağıdaki kod, uri şablon işlevini kullanarak iç içe geçme şablonuna bağlantının nasıl belirtilen olduğunu gösterir:

```json
"resources": [
  {
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "name": "shared",
    "properties": {
      "mode": "Incremental",
      "templateLink": {
        "uri": "[uri(parameters('_artifactsLocation'), concat('nested/vnet.json', parameters('_artifactsLocationSasToken')))]",
        "contentVersion": "1.0.0.0"
      }
    }
  }
]
```

Bu yaklaşım `_artifactsLocation` kullanılarak, parametre için varsayılan değer kullanılır. Bağlantılı şablonların farklı bir konumdan alınması gerekiyorsa, parametre girişi varsayılan değeri geçersiz kılmak için dağıtım zamanında kullanılabilir ve şablonun kendisinde değişiklik gerekmez.

### <a name="use-_artifactslocation-instead-of-hardcoding-links"></a>Hardcoding bağlantıları yerine _artifactsLocation kullanın

İç içe şablonlar için kullanılmasının yanı `_artifactsLocation` sıra, parametredeki URL dağıtım şablonunun ilgili tüm yapıları için temel olarak kullanılır. Bazı VM uzantıları, şablonun dışında depolanan bir komut dosyasına bağlantı içerir. Bu uzantılar için, bağlantıları kodlamamalısınız. Örneğin, Özel Komut Dosyası ve PowerShell DSC uzantıları, gösterildiği gibi GitHub'daki harici bir komut dosyasına bağlanabilir:

```json
"properties": {
  "publisher": "Microsoft.Compute",
  "type": "CustomScriptExtension",
  "typeHandlerVersion": "1.9",
  "autoUpgradeMinorVersion": true,
  "settings": {
    "fileUris": [
      "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
    ]
  }
}
```

Komut dosyasına bağlantılar hardcoding potansiyel olarak şablonun başka bir konuma başarıyla dağıtılmasını engeller. VM kaynağının yapılandırması sırasında, VM içinde çalışan VM aracısı VM uzantısına bağlı tüm komut dosyalarının karşıdan yükleminin karşıdan yükleniyor ve ardından komut dosyalarını VM'nin yerel diskinde depolar. Bu yaklaşım, "Bölgeler arasında iç içe şablonlar kullan" bölümünde daha önce açıklanan iç içe geçirilmiş şablon bağlantıları gibi işlev görür.

Kaynak Yöneticisi çalışma zamanında iç içe olan şablonları alır. VM uzantıları için, herhangi bir dış yapının alınması VM aracısı tarafından gerçekleştirilir. Yapı alma işleminin farklı başlatıcısının yanı sıra, şablon tanımındaki çözüm aynıdır. Tüm yapıların depolandığı temel yolun varsayılan değeri (VM uzantılı komut dosyaları dahil) ve `_artifactsLocationSasToken` sasToken girişi için parametre ile _artifactsLocation parametresini kullanın.

```json
"parameters": {
  "_artifactsLocation": {
    "type": "string",
    "metadata": {
      "description": "The base URI where artifacts required by this template are located."
    },
    "defaultValue": "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/"
  },
  "_artifactsLocationSasToken": {
    "type": "securestring",
    "metadata": {
      "description": "The sasToken required to access _artifactsLocation."
    },
    "defaultValue": ""
  }
}
```

Bir yapının mutlak URI'sini oluşturmak için tercih edilen yöntem, concat şablon işlevi yerine uri şablon işlevini kullanmaktır. VM uzantısındaki komut dosyalarına sabit kodlu bağlantılar uri şablon işlevi yle değiştirilerek, şablondaki bu işlevsellik bulut tutarlılığı için yapılandırılır.

```json
"properties": {
  "publisher": "Microsoft.Compute",
  "type": "CustomScriptExtension",
  "typeHandlerVersion": "1.9",
  "autoUpgradeMinorVersion": true,
  "settings": {
    "fileUris": [
      "[uri(parameters('_artifactsLocation'), concat('scripts/configure-music-app.ps1', parameters('_artifactsLocationSasToken')))]"
    ]
  }
}
```

Bu yaklaşımla, yapılandırma komut dosyaları da dahil olmak üzere tüm dağıtım yapıları şablonun kendisiyle aynı konumda depolanabilir. Tüm bağlantıların konumunu değiştirmek için, _yalnızca yapılarKonum parametreleri_için farklı bir temel URL belirtmeniz gerekir.

## <a name="factor-in-differing-regional-capabilities"></a>Farklı bölgesel yeteneklerde faktör

Azure'a sunulan güncelleştirmelerin ve yeni hizmetlerin çevik gelişimi ve sürekli akışı yla, bölgelerin hizmet veya güncelleştirme kullanılabilirliğinde [farklılık olabilir.](https://azure.microsoft.com/regions/services/) Zorlu dahili sınamalardan sonra, yeni hizmetler veya varolan hizmetlere yapılan güncelleştirmeler genellikle bir doğrulama programına katılan küçük bir müşteri kitlesine tanıtılır. Başarılı müşteri doğrulaması sonrasında, hizmetler veya güncelleştirmeler Azure bölgelerinin bir alt kümesinde kullanıma sunulur, daha sonra daha fazla bölgeye sunulur, egemen bulutlara dağıtılır ve Azure Yığını müşterileri için de kullanılabilir hale getirilir.

Azure bölgelerinin ve bulutların kullanılabilir hizmetlerinde farklılık olabileceğini bilerek, şablonlarınız hakkında bazı proaktif kararlar verebilirsiniz. Başlamak için iyi bir yer, bir bulut için kullanılabilir kaynak sağlayıcılarını incelemektir. Bir kaynak sağlayıcısı, bir Azure hizmeti için kullanılabilen kaynak ve işlem kümesini size bildirir.

Şablon kaynakları dağıtır ve yapılandırır. Kaynak türü bir kaynak sağlayıcısı tarafından sağlanır. Örneğin, bilgi işlem kaynak sağlayıcısı (Microsoft.Compute), virtualMachines ve kullanılabilirlik Kümeleri gibi birden çok kaynak türü sağlar. Her kaynak sağlayıcısı, azure kaynak yöneticisine ortak bir sözleşmeyle tanımlanan bir API sağlayarak tüm kaynak sağlayıcılar arasında tutarlı ve birleşik bir yazma deneyimi sağlar. Ancak, genel Azure'da kullanılabilen bir kaynak sağlayıcısı egemen bir bulutta veya Azure Yığını bölgesinde kullanılamayabilir.

![Kaynak sağlayıcıları](./media/templates-cloud-consistency/resource-providers.png)

Belirli bir bulutta kullanılabilen kaynak sağlayıcılarını doğrulamak için Azure komut satırı arabiriminde[(CLI)](/cli/azure/install-azure-cli)aşağıdaki komut dosyasını çalıştırın:

```azurecli-interactive
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Ayrıca, kullanılabilir kaynak sağlayıcılarını görmek için aşağıdaki PowerShell cmdlet'i kullanabilirsiniz:

```azurepowershell-interactive
Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

### <a name="verify-the-version-of-all-resource-types"></a>Tüm kaynak türlerinin sürümünü doğrulama

Özellikler kümesi tüm kaynak türleri için ortaktır, ancak her kaynağın kendi özel özellikleri de vardır. Yeni bir API sürümü aracılığıyla zaman zaman varolan kaynak türlerine yeni özellikler ve ilgili özellikler eklenir. Şablondaki bir kaynağın kendi API `apiVersion`sürümü özelliği vardır - . Bu sürüm, şablondaki varolan bir kaynak yapılandırmasının platformdaki değişikliklerden etkilenmemesini sağlar.

Genel Azure'daki varolan kaynak türlerine tanıtılan yeni API sürümleri tüm bölgelerde, egemen bulutlarda veya Azure Yığını'nda hemen kullanılamayabilir. Bir bulut için kullanılabilir kaynak sağlayıcıları, kaynak türleri ve API sürümlerinin listesini görüntülemek için Azure portalında Kaynak Gezgini'ni kullanabilirsiniz. Tüm Hizmetler menüsünde Kaynak Gezgini'ni arayın. Kullanılabilir tüm kaynak sağlayıcılarını, kaynak türlerini ve API sürümlerini bu bulutta döndürmek için Kaynak Gezgini'ndeki Sağlayıcılar düğümini genişletin.

Azure CLI'de belirli bir buluttaki tüm kaynak türleri için kullanılabilir API sürümünü listelemek için aşağıdaki komut dosyasını çalıştırın:

```azurecli-interactive
az provider list --query "[].{namespace:namespace, resourceType:resourceType[]}"
```

Ayrıca aşağıdaki PowerShell cmdlet kullanabilirsiniz:

```azurepowershell-interactive
Get-AzureRmResourceProvider | select-object ProviderNamespace -ExpandProperty ResourceTypes | ft ProviderNamespace, ResourceTypeName, ApiVersions
```

### <a name="refer-to-resource-locations-with-a-parameter"></a>Parametresi olan kaynak konumlarına bakın

Şablon her zaman bir bölgede bulunan bir kaynak grubuna dağıtılır. Dağıtımın kendisi dışında, şablondaki her kaynağın, dağıtılacak bölgeyi belirtmek için kullandığınız bir konum özelliği de vardır. Bulut tutarlılığı şablonunuzu geliştirmek için kaynak konumlarına başvurmak için dinamik bir yol gerekir, çünkü her Azure Yığını benzersiz konum adları içerebilir. Genellikle kaynaklar kaynak grubuyla aynı bölgede dağıtılır, ancak bölgeler arası uygulama kullanılabilirliği gibi senaryoları desteklemek için, kaynakları bölgelere yaymak yararlı olabilir.

Şablondaki kaynak özelliklerini belirtirken bölge adlarını kodlayabilirsiniz, ancak bu yaklaşım şablonun diğer Azure Yığını ortamlarına dağıtılabileceğini garanti etmez, çünkü bölge adı büyük olasılıkla orada bulunmaz.

Farklı bölgeleri barındırmak için, varsayılan değeri olan şablona bir giriş parametre konumu ekleyin. Dağıtım sırasında değer belirtilmemişse varsayılan değer kullanılır.

Şablon işlevi `[resourceGroup()]` aşağıdaki anahtar/değer çiftlerini içeren bir nesne döndürür:

```json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
  "tags": {
  },
  "properties": {
    "provisioningState": "{status}"
  }
}
```

Azure Kaynak Yöneticisi, giriş parametresinin varsayılan Değeri'nde nesnenin konum anahtarına başvurarak, çalışma `[resourceGroup().location]` zamanında şablon işlevini, şablonun dağıtılan kaynak grubunun konumuyla değiştirir.

```json
"parameters": {
  "location": {
    "type": "string",
    "metadata": {
      "description": "Location the resources will be deployed to."
    },
    "defaultValue": "[resourceGroup().location]"
  }
},
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2015-06-15",
    "name": "storageaccount1",
    "location": "[parameters('location')]",
    ...
```

Bu şablon işlevi yle, bölge adlarını önceden bilmeden şablonunuzu herhangi bir buluta dağıtabilirsiniz. Ayrıca, şablondaki belirli bir kaynağın konumu kaynak grubu konumundan farklı olabilir. Bu durumda, söz konusu kaynak için ek giriş parametreleri kullanarak yapılandırabilirsiniz, aynı şablondaki diğer kaynaklar hala ilk konum giriş parametresini kullanır.

### <a name="track-versions-using-api-profiles"></a>API profillerini kullanarak sürümleri izleme

Azure Yığını'nda bulunan tüm kullanılabilir kaynak sağlayıcılarını ve ilgili API sürümlerini izlemek çok zor olabilir. Örneğin, yazım sırasında, **Microsoft.Compute/availabilitySets** için Azure'daki en son `2018-04-01`API sürümü, Azure ve Azure Yığını'nda yaygın olan kullanılabilir API sürümü ise. `2016-03-30` **Microsoft.Storage/storageAccounts'un** tüm Azure ve Azure Yığını konumları `2016-01-01`arasında paylaşılan yaygın API `2018-02-01`sürümü, Azure'daki en son API sürümü ise.

Bu nedenle, Kaynak Yöneticisi şablonlara API profilleri kavramını tanıttı. API profilleri olmadan, şablondaki her kaynak, `apiVersion` belirli bir kaynağın API sürümünü açıklayan bir öğeyle yapılandırılır.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string",
      "metadata": {
          "description": "Location the resources will be deployed to."
      },
      "defaultValue": "[resourceGroup().location]"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-01-01",
      "name": "mystorageaccount",
      "location": "[parameters('location')]",
      "properties": {
        "accountType": "Standard_LRS"
      }
    },
    {
      "type": "Microsoft.Compute/availabilitySets",
      "apiVersion": "2016-03-30",
      "name": "myavailabilityset",
      "location": "[parameters('location')]",
      "properties": {
        "platformFaultDomainCount": 2,
        "platformUpdateDomainCount": 2
      }
    }
  ],
  "outputs": {}
}
```

API profil sürümü, Azure ve Azure Yığını'nda yaygın olan kaynak türü başına tek bir API sürümü için takma ad görevi görür. Şablondaki her kaynak için bir API sürümü belirtmek yerine, çağrılan `apiProfile` yeni bir kök öğesinde yalnızca `apiVersion` API profil sürümünü belirtir ve tek tek kaynaklar için öğeyi atlarsınız.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "apiProfile": "2018–03-01-hybrid",
    "parameters": {
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location the resources will be deployed to."
            },
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "mystorageaccount",
            "location": "[parameters('location')]",
            "properties": {
                "accountType": "Standard_LRS"
            }
        },
        {
            "type": "Microsoft.Compute/availabilitySets",
            "name": "myavailabilityset",
            "location": "[parameters('location')]",
            "properties": {
                "platformFaultDomainCount": 2,
                "platformUpdateDomainCount": 2
            }
        }
    ],
    "outputs": {}
}
```

API profili, API sürümlerinin konumlar arasında kullanılabilir olmasını sağlar, böylece belirli bir konumda bulunan apiVersions'ları el ile doğrulamanız gerekmez. API profiliniz tarafından başvurulan API sürümlerinin Azure Yığını ortamında bulunduğundan emin olmak için, Azure Yığını işleçlerinin destek ilkesine göre çözümü güncel tutması gerekir. Bir sistem altı aydan fazla güncel değilse, uyumluluğu dışında kabul edilir ve ortamın güncellenmesi gerekir.

API profili şablonda gerekli bir öğe değildir. Öğeyi ekleseniz bile, yalnızca hiçbirinin `apiVersion` belirtilmediği kaynaklar için kullanılır. Bu öğe aşamalı değişikliklere izin verir, ancak varolan şablonlarda değişiklik gerektirmez.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "apiProfile": "2018–03-01-hybrid",
    "parameters": {
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location the resources will be deployed to."
            },
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2016-01-01",
            "name": "mystorageaccount",
            "location": "[parameters('location')]",
            "properties": {
                "accountType": "Standard_LRS"
            }
        },
        {
            "type": "Microsoft.Compute/availabilitySets",
            "name": "myavailabilityset",
            "location": "[parameters('location')]",
            "properties": {
                "platformFaultDomainCount": 2,
                "platformUpdateDomainCount": 2
            }
        }
    ],
    "outputs": {}
}
```

## <a name="check-endpoint-references"></a>Bitiş noktası başvurularını denetleme

Kaynakların platformdaki diğer hizmetlere başvuruları olabilir. Örneğin, ortak bir IP'nin herkese açık bir DNS adı atanabilir. Genel bulut, egemen bulutlar ve Azure Yığını çözümlerinin kendi bitiş noktası ad alanları vardır. Çoğu durumda, bir kaynak şablonda giriş olarak yalnızca bir önek gerektirir. Çalışma zamanı sırasında Azure Kaynak Yöneticisi, bitiş noktası değerini ekler. Bazı uç nokta değerlerinin şablonda açıkça belirtilmesi gerekir.

> [!NOTE]
> Bulut tutarlılığı için şablonlar geliştirmek için bitiş noktası ad alanlarını kodlamayın.

Aşağıdaki iki örnek, kaynak oluştururken açıkça belirtilmesi gereken ortak uç nokta ad alanlarıdır:

* Depolama hesapları (blob, sıra, tablo ve dosya)
* Veritabanları için bağlantı dizeleri ve Redis için Azure Önbelleği

Bitiş noktası ad alanları, dağıtım tamamlandığında kullanıcı için bilgi olarak şablon çıktısında da kullanılabilir. Sık karşılaşılan örnekler şunlardır:

* Depolama hesapları (blob, sıra, tablo ve dosya)
* Bağlantı dizeleri (MySql, SQLServer, SQLAzure, Custom, NotificationHub, ServiceBus, EventHub, ApiHub, DocDb, RedisCache, PostgreSQL)
* Traffic Manager
* ortak bir IP adresinin domainNameLabel
* Bulut hizmetleri

Genel olarak, şablonda kodlanmış uç noktalardan kaçının. En iyi yöntem, uç noktaları dinamik olarak almak için başvuru şablonu işlevini kullanmaktır. Örneğin, en sık hardcoded bitiş noktası depolama hesapları için bitiş noktası ad alanıdır. Her depolama hesabı, depolama hesabının adını bitiş noktası ad alanıyla biraraya getiren benzersiz bir FQDN'ye sahiptir. Mystorageaccount1 adlı bir blob depolama hesabı, buluta bağlı olarak farklı FQDN'lerle sonuçlanır:

* **mystorageaccount1.blob.core.windows.net,** genel Azure bulutu üzerinde oluşturulduğunda.
* Azure China 21Vianet bulutu içinde oluşturulduğunda **mystorageaccount1.blob.core.chinacloudapi.cn.**

Aşağıdaki başvuru şablonu işlevi, depolama kaynak sağlayıcısından bitiş noktası ad alanını alır:

```json
"diskUri":"[concat(reference(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))).primaryEndpoints.blob, 'container/myosdisk.vhd')]"
```

Depolama hesabı bitiş noktasının sabit kodlanmış değerini `reference` şablon işleviyle değiştirerek, bitiş noktası referansında herhangi bir değişiklik yapmadan farklı ortamlara başarılı bir şekilde dağıtmak için aynı şablonu kullanabilirsiniz.

### <a name="refer-to-existing-resources-by-unique-id"></a>Varolan kaynaklara benzersiz kimlikle bakın

Aynı veya başka bir kaynak grubundan ve aynı abonelik veya başka bir abonelik içinde, aynı bulutta aynı kiracı içinde varolan bir kaynağa da başvurabilirsiniz. Kaynak özelliklerini almak için, kaynağın kendisi için benzersiz tanımlayıcıyı kullanmanız gerekir. Şablon `resourceId` işlevi, aşağıdaki kodun gösterdiği gibi SQL Server gibi bir kaynağın benzersiz kimliğini alır:

```json
"outputs": {
  "resourceId":{
    "type": "string",
    "value": "[resourceId('otherResourceGroup', 'Microsoft.Sql/servers', parameters('serverName'))]"
  }
}
```

Daha sonra bir `resourceId` veritabanının `reference` özelliklerini almak için şablon işlevi içinde işlevi kullanabilirsiniz. İade nesnesi, tam uç nokta değerini tutan `fullyQualifiedDomainName` özelliği içerir. Bu değer çalışma zamanında alınır ve bulut ortamına özgü uç nokta ad alanı sağlar. Bitiş noktası ad alanını zorlamadan bağlantı dizesini tanımlamak için, dönüş nesnesinin özelliğine gösterildiği gibi doğrudan bağlantı dizesinde başvurabilirsiniz:

```json
"[concat('Server=tcp:', reference(resourceId('sql', 'Microsoft.Sql/servers', parameters('test')), '2015-05-01-preview').fullyQualifiedDomainName, ',1433;Initial Catalog=', parameters('database'),';User ID=', parameters('username'), ';Password=', parameters('pass'), ';Encrypt=True;')]"
```

## <a name="consider-resource-properties"></a>Kaynak özelliklerini göz önünde bulundurun

Azure Yığını ortamlarında belirli kaynaklar şablonunuzda göz önünde bulundurmanız gereken benzersiz özelliklere sahiptir.

### <a name="ensure-vm-images-are-available"></a>VM görüntülerinin kullanılabilir olduğundan emin olun

Azure, zengin bir VM görüntü seçkisi sunar. Bu görüntüler Microsoft ve iş ortakları tarafından oluşturulup dağıtıma hazırlanmıştır. Görüntüler platformdaki VM'lerin temelini oluşturur. Ancak bulut tutarlı bir şablon yalnızca kullanılabilir parametrelere (özellikle, genel Azure, Azure egemen bulutlar veya Azure Yığını çözümü için kullanılabilen VM görüntülerinin yayıncısı, teklifi ve SKU'su) başvurmalıdır.

Bir konumda bulunan kullanılabilir VM görüntülerinin listesini almak için aşağıdaki Azure CLI komutunu çalıştırın:

```azurecli-interactive
az vm image list -all
```

Azure PowerShell cmdlet [Get-AzureRmVMImagePublisher](/powershell/module/az.compute/get-azvmimagepublisher) ile aynı listeyi alabilir ve `-Location` parametreyle istediğiniz konumu belirtebilirsiniz. Örnek:

```azurepowershell-interactive
Get-AzureRmVMImagePublisher -Location "West Europe" | Get-AzureRmVMImageOffer | Get-AzureRmVMImageSku | Get-AzureRmVMImage
```

Bu komutun, küresel Azure bulutunun Batı Avrupa bölgesinde bulunan tüm görüntüleri döndürmesi birkaç dakika sürer.

Bu VM görüntülerini Azure Stack'te kullanıma sunulduysanız, kullanılabilir tüm depolama alanı tüketilir. Azure Yığını, en küçük ölçekli birimi bile barındırmak için ortama eklemek istediğiniz resimleri seçmenize olanak tanır.

Aşağıdaki kod örneği, ARM şablonlarınızdaki yayımcı, teklif ve SKU parametrelerine başvurmak için tutarlı bir yaklaşım gösterir:

```json
"storageProfile": {
    "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "2016-Datacenter",
    "version": "latest"
    }
}
```

### <a name="check-local-vm-sizes"></a>Yerel VM boyutlarını kontrol edin

Bulut tutarlılığı için şablonunuzu geliştirmek için, istediğiniz VM boyutunun tüm hedef ortamlarda kullanılabilir olduğundan emin olmanız gerekir. VM boyutları performans özellikleri ve yetenekleri bir gruplandırma vardır. Bazı VM boyutları, VM'nin çalıştığı donanıma bağlıdır. Örneğin, GPU için optimize edilmiş bir VM dağıtmak istiyorsanız, hipervizörü çalıştıran donanımın donanım GPU'larına sahip olması gerekir.

Microsoft belirli donanım bağımlılıklarına sahip yeni bir VM boyutu sunduğunda, VM boyutu genellikle önce Azure bulutundaki küçük bir bölge alt kümesinde kullanılabilir hale getirilir. Daha sonra, diğer bölgeler ve bulutlar için kullanılabilir hale getirilir. Dağıttıklarınız her bulutta VM boyutunun bulunduğundan emin olmak için, kullanılabilir boyutları aşağıdaki Azure CLI komutuyla alabilirsiniz:

```azurecli-interactive
az vm list-sizes --location "West Europe"
```

Azure PowerShell için şunları kullanın:

```azurepowershell-interactive
Get-AzureRmVMSize -Location "West Europe"
```

Mevcut hizmetlerin tam listesi [için, bölgeye göre kullanılabilen Ürünler](https://azure.microsoft.com/global-infrastructure/services/?cdn=disable)bölümüne bakın.

### <a name="check-use-of-azure-managed-disks-in-azure-stack"></a>Azure Yığını'nda Azure Yönetilen Disklerin kullanımını denetleme

Yönetilen diskler, bir Azure kiracısının depolama alanını işler. Bir depolama hesabı açıkça oluşturmak ve sanal bir sabit disk (VHD) için URI'yi belirtmek yerine, bir VM dağıtırken bu eylemleri dolaylı olarak gerçekleştirmek için yönetilen diskleri kullanabilirsiniz. Yönetilen diskler, VM'lerden gelen tüm diskleri farklı depolama birimlerine ayarlanmış aynı kullanılabilirliğe yerleştirerek kullanılabilirliği artırır. Ayrıca, mevcut VHD'ler önemli ölçüde daha az kapalı kalma süresiyle Standart depolamadan Premium depolama alanına dönüştürülebilir.

Yönetilen diskler Azure Yığını'nın yol haritasında olmasına rağmen, şu anda desteklenmez. Bunlar olana kadar, VM kaynağının şablonundaki öğeyi `vhd` kullanarak VHD'leri açıkça belirterek Azure Yığını için bulut tutarlı şablonlar geliştirebilirsiniz:

```json
"storageProfile": {
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "[parameters('windowsOSVersion')]",
    "version": "latest"
  },
  "osDisk": {
    "name": "osdisk",
    "vhd": {
      "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2015-06-15').primaryEndpoints.blob, 'vhds/osdisk.vhd')]"
    },
    "caching": "ReadWrite",
    "createOption": "FromImage"
  }
}
```

Bunun aksine, bir şablonda yönetilen disk yapılandırmasını belirtmek için öğeyi `vhd` disk yapılandırmasından kaldırın.

```json
"storageProfile": {
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "[parameters('windowsOSVersion')]",
    "version": "latest"
  },
  "osDisk": {
    "caching": "ReadWrite",
    "createOption": "FromImage"
  }
}
```

Aynı değişiklikler [de veri diskleri](../../virtual-machines/windows/using-managed-disks-template-deployments.md)geçerlidir.

### <a name="verify-that-vm-extensions-are-available-in-azure-stack"></a>Azure Yığını'nda VM uzantılarının kullanılabildiğini doğrulayın

Bulut tutarlılığı için dikkat edilmesi gereken bir diğer husus da, vm içindeki kaynakları yapılandırmak için [sanal makine uzantılarının](../../virtual-machines/windows/extensions-features.md) kullanılmasıdır. Tüm VM uzantıları Azure Yığını'nda kullanılamaz. Şablon, VM uzantısına adanmış kaynakları belirterek şablon içindeki bağımlılıkları ve koşulları oluşturabilir.

Örneğin, Microsoft SQL Server çalıştıran bir VM yapılandırmak istiyorsanız, VM uzantısı şablon dağıtımının bir parçası olarak SQL Server'ı yapılandırabilir. Dağıtım şablonu, SQL Server çalıştıran VM'de bir veritabanı oluşturmak üzere yapılandırılmış bir uygulama sunucusu da içeriyorsa neler olacağını düşünün. Uygulama sunucuları için bir VM uzantısı kullanmanın yanı sıra, SQL Server VM uzantı kaynağının başarılı dönüşünde uygulama sunucusunun bağımlılığını yapılandırabilirsiniz. Bu yaklaşım, uygulama sunucusuveritabanı oluşturmak için talimat verildiğinde VM çalışan SQL Server yapılandırılır ve kullanılabilir sağlar.

Şablonun bildirimsel yaklaşımı, kaynakların ve bunların bağımlılıklarının son durumunu tanımlamanıza olanak sağlarken, platform bağımlılıklar için gerekli olan mantığı dikkate alır.

#### <a name="check-that-vm-extensions-are-available"></a>VM uzantılarının kullanılabilir olup olmadığını kontrol edin

Birçok VM uzantısı türü vardır. Bulut tutarlılığı için şablon geliştirirken, yalnızca şablonun hedeflediği tüm bölgelerde kullanılabilen uzantıları kullandığınızdan emin olun.

Belirli bir bölge için kullanılabilen VM uzantılarının listesini almak için `myLocation`(bu örnekte), aşağıdaki Azure CLI komutunu çalıştırın:

```azurecli-interactive
az vm extension image list --location myLocation
```

Ayrıca Azure PowerShell [Get-AzureRmVmImagePublisher](/powershell/module/az.compute/get-azvmimagepublisher) cmdlet'i `-Location` çalıştırabilir ve sanal makine görüntüsünün konumunu belirtmek için kullanabilirsiniz. Örnek:

```azurepowershell-interactive
Get-AzureRmVmImagePublisher -Location myLocation | Get-AzureRmVMExtensionImageType | Get-AzureRmVMExtensionImage | Select Type, Version
```

#### <a name="ensure-that-versions-are-available"></a>Sürümlerin kullanılabilir olduğundan emin olun

VM uzantıları birinci taraf Kaynak Yöneticisi kaynakları olduğundan, kendi API sürümleri vardır. Aşağıdaki kodun gösterdiği gibi, VM uzantı türü Microsoft.Compute kaynak sağlayıcısında iç içe bir kaynaktır.

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "apiVersion": "2015-06-15",
    "name": "myExtension",
    "location": "[parameters('location')]",
    ...
```

VM uzantı kaynağının API sürümü, şablonunuzla hedeflemeyi planladığınız tüm konumlarda bulunmalıdır. Konum bağımlılığı, daha önce "Tüm kaynak türlerinin sürümünü doğrula" bölümünde tartışılan kaynak sağlayıcısı API sürümü kullanılabilirliği gibi çalışır.

VM uzantıkaynağı için kullanılabilir API sürümlerinin listesini almak için, gösterildiği gibi **Microsoft.Compute** kaynak sağlayıcısıyla [Get-AzureRmResourceProvider](/powershell/module/az.resources/get-azresourceprovider) cmdlet'ini kullanın:

```azurepowershell-interactive
Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Compute" | Select-Object -ExpandProperty ResourceTypes | Select ResourceTypeName, Locations, ApiVersions | where {$_.ResourceTypeName -eq "virtualMachines/extensions"}
```

Sanal makine ölçek kümelerinde VM uzantılarını da kullanabilirsiniz. Aynı konum koşulları geçerlidir. Bulut tutarlılığı şablonunuzu geliştirmek için, API sürümlerinin dağıtmayı planladığınız tüm konumlarda kullanılabilir olduğundan emin olun. Ölçek kümeleri için VM uzantı kaynağının API sürümlerini almak için, öncekiyle aynı cmdlet'i kullanın, ancak sanal makine ölçeğikaynak türünü gösterildiği gibi ayarlar:

```azurepowershell-interactive
Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Compute" | Select-Object -ExpandProperty ResourceTypes | Select ResourceTypeName, Locations, ApiVersions | where {$_.ResourceTypeName -eq "virtualMachineScaleSets/extensions"}
```

Her özel uzantı da sürülür. Bu sürüm VM `typeHandlerVersion` uzantısı özelliği gösterilir. Şablonunuzun VM uzantılarının `typeHandlerVersion` öğesinde belirtilen sürümün şablonu dağıtmayı planladığınız konumlarda kullanılabilir olduğundan emin olun. Örneğin, aşağıdaki kod sürüm 1.7'yi belirtir:

```json
{
    "type": "extensions",
    "apiVersion": "2016-03-30",
    "name": "MyCustomScriptExtension",
    "location": "[parameters('location')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]"
    ],
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.7",
        ...
```

Belirli bir VM uzantısı için kullanılabilir sürümlerin listesini almak için [Get-AzureRmVMExtensionImage](/powershell/module/az.compute/get-azvmextensionimage) cmdlet'ini kullanın. Aşağıdaki örnek, PowerShell DSC (İstenilen Durum Yapılandırması) VM uzantısı için kullanılabilir sürümleri **myLocation'tan**alır:

```azurepowershell-interactive
Get-AzureRmVMExtensionImage -Location myLocation -PublisherName Microsoft.PowerShell -Type DSC | FT
```

Yayıncıların listesini almak için [Get-AzureRmVmImagePublisher](/powershell/module/az.compute/get-azvmimagepublisher) komutunu kullanın. Tür istemek için [Get-AzureRmVMExtensionImageType](/powershell/module/az.compute/get-azvmextensionimagetype) övgüsi'ni kullanın.

## <a name="tips-for-testing-and-automation"></a>Test ve otomasyon için ipuçları

Şablon yazarken ilgili tüm ayarları, yetenekleri ve sınırlamaları izlemek zor bir iş. Ortak yaklaşım, diğer konumlar hedeflenmeden önce şablonları tek bir buluta karşı geliştirmek ve sınamaktır. Ancak, yazma işleminde testler ne kadar erken gerçekleştirilirse, geliştirme ekibinizin yeniden yazmayı o kadar az sorun giderme ve kod yeniden yazma işlemi gerekir. Konum bağımlılıkları nedeniyle başarısız olan dağıtımlar, sorun giderme için zaman alabilir. Bu nedenle, yazma döngüsünde mümkün olduğunca erken otomatik test yapmanızı öneririz. Sonuç olarak, daha az geliştirme süresine ve daha az kaynağa ihtiyacınız olacak ve bulut tutarlı yapılarınızın daha da değerli hale gelmesi.

Aşağıdaki resim, tümleşik bir geliştirme ortamı (IDE) kullanan bir takım için geliştirme sürecinin tipik bir örneğini gösterir. Zaman çizelgesinin farklı aşamalarında, farklı test türleri yürütülür. Burada, iki geliştirici aynı çözüm üzerinde çalışıyor, ancak bu senaryo tek bir geliştirici veya büyük bir ekip için eşit olarak geçerlidir. Her geliştirici genellikle merkezi bir deponun yerel bir kopyasını oluşturur ve her birinin aynı dosyalar üzerinde çalışan diğer lerini etkilemeden yerel kopya üzerinde çalışmasını sağlar.

![İş akışı](./media/templates-cloud-consistency/workflow.png)

Test ve otomasyon için aşağıdaki ipuçlarını göz önünde bulundurun:

* Test araçlarından yararlanın. Örneğin, Visual Studio Code ve Visual Studio, IntelliSense ve şablonlarınızı doğrulamanıza yardımcı olabilecek diğer özellikleri içerir.
* Yerel IDE'nin geliştirilmesi sırasında kod kalitesini artırmak için birim testleri ve tümleştirme testleri ile statik kod çözümlemesi yapın.
* İlk geliştirme sırasında daha da iyi bir deneyim için, birim testleri ve tümleştirme testleri yalnızca bir sorun bulunduğunda uyarmalı ve testlere devam etmelidir. Bu şekilde, ele alınacak sorunları belirleyebilir ve test tabanlı dağıtım (TDD) olarak da adlandırılan değişikliklerin sırasını önceliklendirmek.
* Azure Kaynak Yöneticisi'ne bağlı olmadan bazı testlerin gerçekleştirilebileceğini unutmayın. Şablon dağıtımı sınamak gibi diğerleri, Kaynak Yöneticisi'nin çevrimdışı gerçekleştirilemeyen belirli eylemleri gerçekleştirmesini gerektirir.
* Bir dağıtım şablonunu doğrulama API'sine karşı sınamak, gerçek bir dağıtıma eşit değildir. Ayrıca, yerel bir dosyadan bir şablon dağıtsanız bile, şablondaki iç içe geçirilmiş şablonlara yapılan başvurular doğrudan Kaynak Yöneticisi tarafından alınır ve VM uzantıları tarafından başvurulan yapılar, dağıtılan VM'nin içinde çalışan VM aracısı tarafından alınır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Kaynak Yöneticisi şablonu hususları](/azure-stack/user/azure-stack-develop-templates)
* [ARM şablonları için en iyi uygulamalar](template-syntax.md)
