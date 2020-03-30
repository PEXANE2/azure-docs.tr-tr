---
title: Şablonlar için en iyi uygulamalar
description: Azure Kaynak Yöneticisi şablonları yazarken önerilen yaklaşımları açıklar. Şablonları kullanırken sık karşılaşılan sorunları önlemek için öneriler sunar.
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 870636d6457d842c89f261c2537644c17a335294
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80156421"
---
# <a name="arm-template-best-practices"></a>ARM şablonu en iyi uygulamalar

Bu makalede, Azure Kaynak Yöneticisi (ARM) şablonunuzu nasıl oluşturacağÝ hakkında öneriler verilmiştir. Bu öneriler, bir çözüm dağıtmak için arm şablonu kullanırken sık karşılaşılan sorunları önlemenize yardımcı olur.

Azure aboneliklerinizi nasıl yöneteceğimize ilişkin öneriler için Bkz. [Azure kurumsal iskelesi: Açıklayıcı abonelik yönetimi.](/azure/architecture/cloud-adoption/appendix/azure-scaffold?toc=%2Fen-us%2Fazure%2Fazure-resource-manager%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)

Tüm Azure bulut ortamlarında çalışan şablonların nasıl oluşturulacaklarına ilişkin öneriler için bulut [tutarlılığı için Azure Kaynak Yöneticisi şablonları geliştir'e](templates-cloud-consistency.md)bakın.

## <a name="template-limits"></a>Şablon sınırları

Şablonunuzun boyutunu 4 MB ve her parametre dosyasını 64 KB ile sınırlayın. 4 MB sınırı, yinelemeli kaynak tanımları ve değişkenler ve parametreler için değerlerle genişletildikten sonra şablonun son durumuna uygulanır. 

Ayrıca şu ile sınırlısınız:

* 256 parametre
* 256 değişken
* 800 kaynak (kopya sayısı dahil)
* 64 çıkış değeri
* Şablon ifadesinde 24.576 karakter

İç içe bir şablon kullanarak bazı şablon sınırlarını aşabilirsiniz. Daha fazla bilgi için bkz: [Azure kaynaklarını dağıtırken bağlantılı şablonları kullanma.](linked-templates.md) Parametrelerin, değişkenlerin veya çıktıların sayısını azaltmak için, bir nesneye birkaç değer birleştirebilirsiniz. Daha fazla bilgi [için, parametreler olarak Nesneler'e](/azure/architecture/building-blocks/extending-templates/objects-as-parameters)bakın.

## <a name="resource-group"></a>Kaynak grubu

Kaynakları bir kaynak grubuna dağıtdığınızda, kaynak grubu kaynaklar la ilgili meta verileri depolar. Meta veriler kaynak grubunun konumunda depolanır.

Kaynak grubunun bölgesi geçici olarak kullanılamıyorsa, meta veriler kullanılamadığı ndan kaynak grubundaki kaynakları güncelleştiremezsiniz. Diğer bölgelerdeki kaynaklar beklendiği gibi çalışmaya devam eder, ancak bunları güncelleştiremezsiniz. Riski en aza indirmek için kaynak grubunuzu ve kaynaklarınızı aynı bölgede bulun.

## <a name="parameters"></a>Parametreler

Bu bölümdeki bilgiler [parametrelerle](template-parameters.md)çalışırken yararlı olabilir.

### <a name="general-recommendations-for-parameters"></a>Parametreler için genel öneriler

* Parametreleri kullanımınızı en aza indirin. Bunun yerine, dağıtım sırasında belirtilmesi gerekmeyen özellikler için değişkenler veya gerçek değerler kullanın.

* Parametre adları için deve kılıfı kullanın.

* SKU, boyut veya kapasite gibi ortama göre değişen ayarlar için parametreleri kullanın.

* Kolay tanımlama için belirtmek istediğiniz kaynak adları için parametreleri kullanın.

* Meta verilerdeki her parametrenin açıklamasını sağlayın:

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "The type of the new storage account created to store the VM disks."
           }
       }
   }
   ```

* Hassas olmayan parametreler için varsayılan değerleri tanımlayın. Varsayılan bir değer belirterek, şablonu dağıtmak daha kolaydır ve şablonunuzun kullanıcıları uygun bir değer örneğini görür. Bir parametre için varsayılan değer, varsayılan dağıtım yapılandırmasındaki tüm kullanıcılar için geçerli olmalıdır. 
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "The type of the new storage account created to store the VM disks."
            }
        }
   }
   ```

* İsteğe bağlı bir parametre belirtmek için boş bir dizeyi varsayılan değer olarak kullanmayın. Bunun yerine, bir değer oluşturmak için gerçek bir değer veya dil ifadesi kullanın.

   ```json
   "storageAccountName": {
     "type": "string",
     "defaultValue": "[concat('storage', uniqueString(resourceGroup().id))]",
     "metadata": {
       "description": "Name of the storage account"
     }
   },
   ```

* Kaynak türü için API sürümü için parametre kullanmayın. Kaynak özellikleri ve değerleri sürüm numarasına göre değişebilir. Api sürümü bir parametreye ayarlandığında, kod düzenleyicisindeki IntelliSense doğru şemayı belirleyemez. Bunun yerine, şablondaki API sürümünü sabit kodlayın.

* Dikkatli `allowedValues` kullanın. Yalnızca bazı değerlerin izin verilen seçeneklere dahil olmadığından emin olmak istediğinizde kullanın. Çok geniş `allowedValues` kullanım yapıyorsanız, listenizi güncel tutmayarak geçerli dağıtımları engelleyebilirsiniz.

* Şablonunuzdaki bir parametre adı PowerShell dağıtım komutundaki bir parametreyle eşleştiğinde, Kaynak Yöneticisi şablon parametresine **FromTemplate** postfix'ini ekleyerek bu adlandırma çakışmasını çözer. Örneğin, şablonunuza **ResourceGroupName** adlı bir parametre eklerseniz, [Yeni Kaynak GrubuDağıtım](/powershell/module/az.resources/new-azresourcegroupdeployment) cmdlet'indeki **ResourceGroupName** parametresi ile çakışır. Dağıtım sırasında, **ResourceGroupNameFromTemplate**için bir değer sağlamanız istenir.

### <a name="security-recommendations-for-parameters"></a>Parametreler için güvenlik önerileri

* Her zaman kullanıcı adları ve parolalar (veya sırlar) için parametreleri kullanın.

* Tüm `securestring` parolalar ve sırlar için kullanın. Bir JSON nesnesinde hassas verileri geçirirseniz, `secureObject` türü kullanın. Güvenli dize veya güvenli nesne türlerine sahip şablon parametreleri kaynak dağıtımından sonra okuamaz. 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "The value of the secret to store in the vault."
           }
       }
   }
   ```

* Kullanıcı adları, parolalar veya tür gerektiren herhangi bir değer `secureString` için varsayılan değerler sağlamayın.

* Uygulamanın saldırı yüzey alanını artıran özellikler için varsayılan değerler sağlamayın.

### <a name="location-recommendations-for-parameters"></a>Parametreler için konum önerileri

* Kaynakların konumunu belirtmek için bir parametre kullanın ve `resourceGroup().location`varsayılan değeri . Konum parametresi sağlamak, şablon kullanıcılarının dağıtmak için izin verdikleri bir konum belirtmelerini sağlar.

   ```json
   "parameters": {
     "location": {
       "type": "string",
       "defaultValue": "[resourceGroup().location]",
       "metadata": {
         "description": "The location in which the resources should be deployed."
       }
     }
   },
   ```

* Konum parametresi için belirtmeyin. `allowedValues` Belirttiğiniz konumlar tüm bulutlarda kullanılamayabilir.

* Aynı konumda olması muhtemel kaynaklar için konum parametre değerini kullanın. Bu yaklaşım, kullanıcıların konum bilgilerini sağlamalarının istenme sayısını en aza indirir.

* Tüm konumlarda kullanılamayan kaynaklar için ayrı bir parametre kullanın veya gerçek bir konum değeri belirtin.

## <a name="variables"></a>Değişkenler

[Değişkenlerle](template-variables.md)çalışırken aşağıdaki bilgiler yararlı olabilir:

* Değişken adlar için deve kılıfı kullanın.

* Şablonda birden fazla kez kullanmanız gereken değerler için değişkenleri kullanın. Bir değer yalnızca bir kez kullanılırsa, sabit kodlanmış bir değer şablonunuzun okunmasını kolaylaştırır.

* Şablon işlevlerinin karmaşık bir düzenlemesinden oluşturabileceğiniz değerler için değişkenleri kullanın. Karmaşık ifade yalnızca değişkenlerde göründüğünde şablonunuzun okunması daha kolaydır.

* Bir kaynak `apiVersion` için değişkenler kullanmayın. API sürümü kaynağın şema sını belirler. Genellikle, kaynağın özelliklerini değiştirmeden sürümü değiştiremezsiniz.

* Şablonun **değişkenler** [bölümündeki başvuru](template-functions-resource.md#reference) işlevini kullanamazsınız. **Başvuru** işlevi değerini kaynağın çalışma zamanı durumundan türeter. Ancak, değişkenler şablonun ilk ayrışma sırasında çözülür. Başvuru **işlevine** doğrudan şablonun **kaynaklar** veya **çıktılar** bölümünde gereken değerler oluşturma.

* Benzersiz olması gereken kaynak adları için değişkenler ekleyin.

* JSON nesnelerinin yinelenen bir deseni oluşturmak için [değişkenlerde](copy-variables.md) bir kopyalama döngüsü kullanın.

* Kullanılmayan değişkenleri kaldırın.

## <a name="resource-dependencies"></a>Kaynak bağımlılıkları

Hangi [bağımlılıkların](define-resource-dependency.md) ayarlanmasına karar verirken aşağıdaki yönergeleri kullanın:

* Bir özelliği paylaşması gereken kaynaklar arasında örtük bir bağımlılık ayarlamak için **kaynak adındaki başvuru** işlevini ve geçişini kullanın. Örtük bir bağımlılık `dependsOn` tanımlamışsanız açık bir öğe eklemeyin. Bu yaklaşım, gereksiz bağımlılıklara sahip olma riskini azaltır.

* Alt kaynağı ana kaynağına bağımlı olarak ayarlayın.

* [Koşul öğesi](conditional-resource-deployment.md) false olarak ayarlanmış kaynaklar bağımlılık siparişinden otomatik olarak kaldırılır. Bağımlılıkları, kaynak her zaman dağıtılmış gibi ayarlayın.

* Bağımlılıkları açıkça ayarlamadan basamaklamasına izin verin. Örneğin, sanal makineniz sanal ağ arabirimine, sanal ağ arabirimi ise sanal ağa ve genel IP adreslerine bağlıdır. Bu nedenle, sanal makine her üç kaynaktan sonra dağıtılır, ancak sanal makineyi açıkça her üç kaynağa bağlı olarak ayarlamayın. Bu yaklaşım, bağımlılık sırasını açıklar ve şablonu daha sonra değiştirmeyi kolaylaştırır.

* Dağıtımdan önce bir değer belirlenebiliyorsa, kaynağı bağımlılık olmadan dağıtmayı deneyin. Örneğin, yapılandırma değerinin başka bir kaynağın adına ihtiyacı varsa, bir bağımlıya ihtiyacınız olmayabilir. Bazı kaynaklar diğer kaynağın varlığını doğruladığı için bu kılavuz her zaman çalışmaz. Bir hata alırsanız, bir bağımlılık ekleyin.

## <a name="resources"></a>Kaynaklar

[Kaynaklarla](template-syntax.md#resources)çalışırken aşağıdaki bilgiler yararlı olabilir:

* Diğer katkıda bulunanların kaynağın amacını anlamalarına yardımcı olmak için, şablondaki her kaynak için **açıklamalar** belirtin:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2019-06-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used to store the VM disks.",
         ...
     }
   ]
   ```

* Şablonunuzda ortak bir *bitiş noktası* kullanıyorsanız (Azure Blob depolama ortak bitiş noktası gibi), ad alanını *sabit kodlamayın.* Ad alanını dinamik olarak almak için **başvuru** işlevini kullanın. Şablondaki bitiş noktasını el ile değiştirmeden şablonu farklı ortak ad alanı ortamlarına dağıtmak için bu yaklaşımı kullanabilirsiniz. API sürümünü şablonunuzdaki depolama hesabı için kullandığınız sürümle ayarlayın:
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')), '2019-06-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   Depolama hesabı oluşturduğunuz şablonda dağıtılırsa ve depolama hesabının adı şablondaki başka bir kaynakla paylaşılmazsa, kaynağa başvururken sağlayıcı ad alanını veya apiVersion'u belirtmeniz gerekmez. Aşağıdaki örnekbasitleştirilmiş sözdizimini gösterir:
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
       }
   }
   ```
     
   Ayrıca, farklı bir kaynak grubunda bulunan varolan bir depolama hesabına da başvuruda bulunabilirsiniz:

   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('existingStorageAccountName')), '2019-06-01').primaryEndpoints.blob]"
       }
   }
   ```

* Genel IP adreslerini yalnızca bir uygulama gerektirdiğinde sanal bir makineye atayın. Hata ayıklama veya yönetim veya yönetim amacıyla sanal bir makineye (VM) bağlanmak için gelen NAT kurallarını, sanal ağ ağ ağ geçidini veya atlama kutusunu kullanın.
   
     Sanal makinelere bağlanma hakkında daha fazla bilgi için bkz:
   
   * [Azure'da N katmanlı mimari için VM'ler çalıştırma](../../guidance/guidance-compute-n-tier-vm.md)
   * [Azure Kaynak Yöneticisi'nde VM'ler için WinRM erişimi ayarlama](../../virtual-machines/windows/winrm.md)
   * [Azure portalını kullanarak VM'nize harici erişime izin verin](../../virtual-machines/windows/nsg-quickstart-portal.md)
   * [PowerShell'i kullanarak VM'inize harici erişime izin verin](../../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [Azure CLI'yi kullanarak Linux VM'nize harici erişime izin verin](../../virtual-machines/virtual-machines-linux-nsg-quickstart.md)

* Ortak IP adresleri için **domainNameLabel** özelliği benzersiz olmalıdır. **DomainNameLabel** değeri 3 ile 63 karakter uzunluğunda olmalı ve bu normal ifadede belirtilen kurallara uymalıdır: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`. **uniqueString** işlevi 13 karakter uzunluğunda bir dize oluşturduğundan, **dnsPrefixString** parametresi 50 karakterle sınırlıdır:

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* Özel bir komut dosyası uzantısına parola eklediğinizde, **protectedSettings** özelliğindeki **ToExecute özelliğini** kullanın:
   
   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```
   
   > [!NOTE]
   > Sırların VM'lere ve uzantılara parametre olarak aktarıldığında şifrelendiğinden emin olmak için, ilgili uzantıların **protectedSettings** özelliğini kullanın.
   > 
   > 

## <a name="next-steps"></a>Sonraki adımlar

* Şablon dosyasının yapısı hakkında bilgi için [bkz.](template-syntax.md)
* Tüm Azure bulut ortamlarında çalışan şablonların nasıl oluşturulacaklarına ilişkin öneriler için bulut [tutarlılığı için ARM şablonları geliştir'e](templates-cloud-consistency.md)bakın.
