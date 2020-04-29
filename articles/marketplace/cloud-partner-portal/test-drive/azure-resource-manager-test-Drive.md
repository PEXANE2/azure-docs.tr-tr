---
title: Test sürücüsüne Azure Resource Manager | Azure Marketi
description: Azure Resource Manager kullanarak Market test sürücüsü oluşturma
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 6125aa010d8676518b84f866343b01f95246160f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80275943"
---
# <a name="azure-resource-manager-test-drive"></a>Azure Resource Manager Test Sürüşü

Bu makale, Azure Marketi 'nde teklifleri olan veya AppSource 'ta çalışan ancak yalnızca Azure kaynaklarıyla test sürücüleri oluşturmak isteyen yayımcılar için geçerlidir.

Azure Resource Manager (Kaynak Yöneticisi) şablonu, çözümünüzü en iyi şekilde temsil edecek şekilde tasarlamanızı sağlayan, Azure kaynakları 'nın kodlanmış bir kapsayıcısıdır. Kaynak Yöneticisi şablonun ne olduğunu bilmiyorsanız, kendi şablonlarınızı nasıl derleyip test etmek için [Kaynak Yöneticisi şablonları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) ve [yazma Kaynak Yöneticisi şablonları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) hakkında bilgi edinin.

Hangi test sürücüsü, belirtilen Kaynak Yöneticisi şablonunu alır ve bu Kaynak Yöneticisi şablonundan gereken tüm kaynakların bir kaynak grubuna dağıtımını yapar.

Bir Azure Resource Manager test sürücüsü derlemeyi seçerseniz, gereksinimler şunları yapmanız gerekir:

- Test sürücünüzü Kaynak Yöneticisi şablonu oluşturun, test edin ve karşıya yükleyin.
- Test sürücünüzü etkinleştirmek için tüm gerekli meta verileri ve ayarları yapılandırın.
- Test sürücüsü etkinken teklifinizi yeniden yayımlayın.

## <a name="how-to-build-an-azure-resource-manager-test-drive"></a>Azure Resource Manager test sürücüsü oluşturma

Azure Resource Manager test sürücüsü oluşturma işlemi aşağıda verilmiştir:

1. Müşterilerinizin akış diyagramında ne olmasını istediğinizi tasarlayın.
1. Müşterilerinizin hangi deneyimleri oluşturmasını istediğinizi tanımlayın.
1. Yukarıdaki tanımları temel alarak, müşteriler için bu tür deneyimleri yerine getirmek için hangi parçaların ve kaynakların gerekli olduğuna karar verin: Örneğin, D365 örneği veya veritabanı içeren bir Web sitesi.
1. Tasarımı yerel olarak derleyin ve deneyimi test edin.
1. Deneyimi bir ARM şablon dağıtımında ve buradan paketleyin:
    1. Kaynakların hangi bölümlerinin giriş parametreleri olduğunu tanımlayın;
    1. Hangi değişkenler vardır?
    1. Müşteri deneyimine hangi çıktılar verilir.
1. Yayımlayın, test edin ve canlı olun.

Azure Resource Manager test sürücüsü oluşturmanın en önemli bölümü, müşterilerinizin hangi senaryolarınızı deneydiklerine tanımlamaktır. Bir güvenlik duvarı ürünü misiniz ve betik ekleme saldırılarını ne kadar iyi işleyebilmek istiyorsunuz? Bir depolama ürünüdür ve çözümünüzün dosyaları ne kadar hızlı ve kolay sıkıştırır?

Ürününüzü görüntülemenin en iyi yollarını değerlendirmek için yeterli miktarda süre harcadığınızdan emin olun. Özellikle ihtiyaç duyduğunuz tüm gerekli kaynakları, Kaynak Yöneticisi şablonu paketlemeyi yeterince daha kolay hale getirir.

Güvenlik Duvarı örneğimize devam etmek için, mimariniz için genel bir IP URL 'SI ve güvenlik duvarınızın koruduğu Web sitesi için başka bir genel IP URL 'si gerekiyor olabilir. Her IP bir sanal makinede dağıtılır ve bir ağ güvenlik grubu + ağ arabirimiyle birlikte bağlanır.

İstenen kaynak paketini tasarladıktan sonra, şimdi test sürücüsü Kaynak Yöneticisi şablonu yazma ve oluşturma.

## <a name="writing-test-drive-resource-manager-templates"></a>Test sürücüsü Kaynak Yöneticisi şablonları yazma

Test sürücüsü dağıtımları tam otomatik modda çalıştırır ve bu nedenle test sürücü şablonlarının aşağıda açıklanan bazı kısıtlamaları vardır.

### <a name="parameters"></a>Parametreler

Çoğu şablon bir dizi parametreye sahiptir. Parametreler, kaynak adlarını, kaynak boyutlarını (örneğin, depolama hesabı türleri veya sanal makine boyutları), Kullanıcı adlarını ve parolaları, DNS adlarını vb. tanımlar. Azure portal kullanarak çözümler dağıttığınızda, tüm bu parametreleri el ile doldurabilir, kullanılabilir DNS adlarını veya depolama hesabı adlarını seçebilir ve bu şekilde devam edebilirsiniz.

![Azure Resource Manager parametrelerinin listesi](./media/azure-resource-manager-test-drive/param1.png)

Ancak, test sürücüsü, insan etkileşimi olmadan tamamen otomatik modda çalışarak yalnızca sınırlı sayıda parametre kategorisi kümesini destekler. Test Drive Kaynak Yöneticisi şablonundaki bir parametre desteklenen kategorilerden birine denk düşmezse, **Bu parametreyi bir değişken veya sabit değer ile değiştirmeniz gerekir.**

Parametreleriniz için geçerli herhangi bir ad kullanabilirsiniz. test sürücüsü, meta veri türü değerini kullanarak parametre kategorisini tanır. **Her şablon parametresi için meta veri türü belirtmeniz gerekir**, aksi takdirde şablonunuz doğrulamadan geçmeyecektir:

```json
"parameters": {
  ...
  "username": {
    "type": "string",
    "metadata": {
      "type": "username"
    }
  },
  ...
}
```

**Tüm parametrelerin isteğe bağlı**olduğunu unutmamak için de önemlidir. bu nedenle\', herhangi birini kullanmak istemiyorsanız,\'bunu yapmanız gerekmez.

### <a name="accepted-parameter-metadata-types"></a>Kabul edilen parametre meta veri türleri

| Meta veri türü   | Parametre türü  | Açıklama     | Örnek değer    |
|---|---|---|---|
| **BaseUri**     | string          | Dağıtım paketinizin temel URI 'SI| https:\//\<.\. \>. blob.Core.Windows.net/\<\..\> |
| **nitelen**    | string          | Yeni rastgele Kullanıcı adı.| admin68876      |
| **parola**    | güvenli dize    | Yeni rastgele parola | LP! ACS\^2kh     |
| **oturum kimliği**   | string          | Benzersiz test sürücüsü oturum KIMLIĞI (GUID)    | b8c8693e-5673-449c-badd-257a405a6dee |

#### <a name="baseuri"></a>BaseUri

Test sürücüsü bu parametreyi dağıtım paketinizin **temel URI** 'siyle başlatır, bu nedenle paketinize dahil olan herhangi bir dosyanın URI 'sini oluşturmak için bu parametreyi kullanabilirsiniz.

```json
"parameters": {
  ...
  "baseuri": {
    "type": "string",
    "metadata": {
      "type": "baseuri",
      "description": "Base Uri of the deployment package."
    }
  },
  ...
}
```

Şablonunuzun içinde, bu parametreyi test sürücüsü dağıtım paketinizdeki herhangi bir dosyanın URI 'Sini oluşturmak için kullanabilirsiniz. Aşağıdaki örnekte, bağlantılı şablonun URI 'Sini oluşturma gösterilmektedir:

```json
"templateLink": {
  "uri": "[concat(parameters('baseuri'),'templates/solution.json')]",
  "contentVersion": "1.0.0.0"
}
```

#### <a name="username"></a>kullanıcı adı

Test sürücüsü bu parametreyi yeni bir rastgele Kullanıcı adı ile başlatır:

```json
"parameters": {
  ...
  "username": {
    "type": "string",
    "metadata": {
      "type": "username",
      "description": "Solution admin name."
    }
  },
  ...
}
```

Örnek değer:

    admin68876

Çözümünüz için rastgele veya sabit Kullanıcı adlarını kullanabilirsiniz.

#### <a name="password"></a>password

Test sürücüsü bu parametreyi yeni bir rastgele parolayla başlatır:

```json
"parameters": {
  ...
  "password": {
    "type": "securestring",
    "metadata": {
      "type": "password",
      "description": "Solution admin password."
    }
  },
  ...
}
```

Örnek değer:

    Lp!ACS^2kh

Çözümünüz için rastgele veya sabit parolalar kullanabilirsiniz.

#### <a name="session-id"></a>oturum KIMLIĞI

Test sürücüsü bu parametreyi, test sürücüsü oturum KIMLIĞINI temsil eden benzersiz bir GUID ile başlatın:

```json
"parameters": {
  ...
  "sessionid": {
    "type": "string",
    "metadata": {
      "type": "sessionid",
      "description": "Unique Test Drive session id."
    }
  },
  ...
}
```

Örnek değer:

    b8c8693e-5673-449c-badd-257a405a6dee

Bu parametreyi, gerekirse test sürücüsü oturumunu benzersiz şekilde tanımlamak için kullanabilirsiniz.

### <a name="unique-names"></a>Benzersiz adlar

Depolama hesapları veya DNS adları gibi bazı Azure kaynakları, genel olarak benzersiz adlar gerektirir.

Bu, her bir test sürücüsünün Kaynak Yöneticisi şablonu dağıttığı anlamına gelir. Bu, tüm\' kaynakları için benzersiz bir **ada sahip yeni bir kaynak grubu** oluşturur. Bu nedenle, rastgele benzersiz değerler oluşturmak için kaynak grubu kimliklerinde değişken adlarınızla birleştirilmiş [uniquestring](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions#uniquestring) işlevini kullanmanız gerekir:

```json
"variables": {
  ...
  "domainNameLabel": "[concat('contosovm',uniquestring(resourceGroup().id))]",
  "storageAccountName": "[concat('contosodisk',uniquestring(resourceGroup().id))]",
  ...
}
```

Parametre/değişken dizelerinizi\'(contosovm\') benzersiz bir dize çıkışı (\'resourceGroup (). ID\') ile eklediğinizden emin olun çünkü bu, her değişkenin benzersizlik ve güvenilirliğini garanti eder.

Örneğin, çoğu kaynak adı bir basamakla başlayamaz, ancak benzersiz dize işlevi bir sayıyla başlayan bir dize döndürebilir. Bu nedenle, ham benzersiz dize çıkışı kullanırsanız dağıtımlarınız başarısız olur. 

[Bu makaledeki](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging)kaynak adlandırma kuralları ve kısıtlamaları hakkında daha fazla bilgi bulabilirsiniz.

### <a name="deployment-location"></a>Dağıtım Konumu

Test sürücünüzü farklı Azure bölgelerinde kullanılabilir hale getirebilirsiniz. Fikir, bir kullanıcının Beast Kullanıcı deneyimiyle birlikte sağlaması için en yakın bölgeyi seçmesine izin versağlamaktır.

Test sürücüsü laboratuvarın bir örneğini oluşturduğunda, her zaman bir kullanıcı tarafından seçilen bölgede bir kaynak grubu oluşturur ve ardından bu grup bağlamında dağıtım şablonunuzu yürütür. Bu nedenle, şablonunuz kaynak grubundan dağıtım konumunu seçmelidir:

```json
"variables": {
  ...
  "location": "[resourceGroup().location]",
  ...
}
```

Ardından, belirli bir laboratuvar örneği için bu konumu her kaynak için kullanın:

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "location": "[variables('location')]",
    ...
  },
  {
    "type": "Microsoft.Network/publicIPAddresses",
    "location": "[variables('location')]",
    ...
  },
  {
    "type": "Microsoft.Network/virtualNetworks",
    "location": "[variables('location')]",
    ...
  },
  {
    "type": "Microsoft.Network/networkInterfaces",
    "location": "[variables('location')]",
    ...
  },
  {
    "type": "Microsoft.Compute/virtualMachines",
    "location": "[variables('location')]",
    ...
  }
]
```

Aboneliğinizin seçtiğiniz her bölgede dağıtmak istediğiniz tüm kaynakları dağıtmasına izin verildiğinden emin olmanız gerekir. Ayrıca, sanal makine görüntülerinizin etkinleştirebildiğiniz tüm bölgelerde kullanılabilir olduğundan emin olmanız gerekir, aksi halde dağıtım şablonunuz bazı bölgelerde çalışmayacaktır.

### <a name="outputs"></a>Çıkışlar

Normalde Kaynak Yöneticisi şablonları ile herhangi bir çıkış üretmeksizin dağıtım yapabilirsiniz. Bunun nedeni, şablon parametrelerini doldurmak için kullandığınız tüm değerleri bilmeniz ve herhangi bir kaynağın özelliklerini her zaman el ile inceleyebilirsiniz.

Test sürücüsü Kaynak Yöneticisi şablonları için, test sürücüsüne\'geri dönmek önemlidir, bu da laboratuvara (Web sitesi URI 'Leri, sanal makine ana bilgisayar adları, Kullanıcı adları ve parolalar) erişim sağlamak için gereklidir. Bu değişkenler müşteriye sunulduğundan tüm çıkış adlarınızın okunabilir olduğundan emin olun.

Şablon çıkışları ile ilgili herhangi bir kısıtlama yoktur. Yalnızca unutmayın, test sürücüsü tüm çıktı değerlerini **dizelere**dönüştürür, yani çıktıya bir nesne gönderirseniz, BIR kullanıcı JSON dizesini görür.

Örnek:

```json
"outputs": {
  "Host Name": {
    "type": "string",
    "value": "[reference(variables('pubIpId')).dnsSettings.fqdn]"
  },
  "User Name": {
    "type": "string",
    "value": "[parameters('adminName')]"
  },
  "Password": {
    "type": "string",
    "value": "[parameters('adminPassword')]"
  }
}
```

### <a name="subscription-limits"></a>Abonelik limitleri

Göz önünde bulundurmanız gereken bir şey, abonelik ve hizmet limitlerdir. Örneğin, on 4 çekirdekli sanal makine dağıtmak istiyorsanız, laboratuvarınız için kullandığınız aboneliğin 40 çekirdek kullanmasına izin verdiğinden emin olmanız gerekir.

[Bu makaledeki](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits)Azure aboneliği ve hizmet limitleri hakkında daha fazla bilgi edinebilirsiniz. Aynı anda birden çok test sürücüsü alınacağından, aboneliğinizin alınabilecek çekirdek sayısını işleyebildiğini ve alınabilecek toplam eşzamanlı \# test sürücüsü sayısına göre işleyebildiğini doğrulayın.

### <a name="what-to-upload"></a>Karşıya yüklenecek

Test sürücüsü Kaynak Yöneticisi şablonu, çeşitli dağıtım yapıtları içerebilen, ancak **ana şablon. JSON**adlı bir dosya olması gereken bir zip dosyası olarak karşıya yüklenir. Bu dosya Azure Resource Manager dağıtım şablonudur ve test sürücüsü bunu bir laboratuvar oluşturmak için kullanır.

Bu dosyanın ötesinde ek kaynaklarınız varsa, bu dosyaya şablonun içinde bir dış kaynak olarak başvurabilirsiniz veya kaynağı ZIP dosyasına dahil edebilirsiniz.

Yayımlama sertifikası sırasında, dağıtım paketinizi IP 'leri test edin ve içeriğini dahili bir test sürücüsü blob kapsayıcısına koyar. Kapsayıcı yapısı, dağıtım paketinizin yapısını yansıtır:

| Package. zip                       | Test sürücüsü blob kapsayıcısı         |
|---|---|
| ana şablon. JSON                | \//\<https:\... \>. blob.Core.Windows.net/\<\... \>/Main-Template.exe  |
| Templates/Solution. JSON           | \//\<https:\... \>. blob.Core.Windows.net/\<\... \>/Templates/Solution.exe JSON |
| betikler/Warmup. ps1                | \//\<https:\... \>. blob.Core.Windows.net/\<\... \>/Scripts/Warmup.exe  |


Bu blob kapsayıcı temel URI 'sinin bir URI 'Sini çağırdık. Laboratuvarınızın her düzeltmesi kendi BLOB kapsayıcısına sahiptir ve bu nedenle, laboratuvarınızın her düzeltmesinin kendi temel URI 'Si vardır. Test sürücüsü, daraltılmış dağıtım paketinizin temel bir URI 'Sini şablon parametreleri aracılığıyla şablonunuza geçirebilir.

## <a name="transforming-template-examples-for-test-drive"></a>Test sürücüsü için şablon örnekleri dönüştürülüyor

Bir kaynak mimarisini bir test sürücüsüne (Kaynak Yöneticisi şablonu) açma işlemi, bozuk olabilir. Bu işlemi daha kolay hale getirmek için,\' [geçerli dağıtım şablonlarının](./transforming-examples-for-test-drive.md)en iyi şekilde nasıl dönüştürüyoruz konusunda örnekler yaptık.

## <a name="how-to-publish-a-test-drive"></a>Test sürücüsü yayımlama

Test sürücünüz oluşturuldığına göre, bu bölümde test sürücünüzü başarıyla yayımlamanız için gereken her bir alan gösterilmektedir.

![Kullanıcı arabiriminde test sürücüsü etkinleştiriliyor](./media/azure-resource-manager-test-drive/howtopub1.png)

İlk ve en önemli alan, test sürücüsünün teklifiniz için etkin olmasını isteyip istemediğinizi değişmemelidir. **Evet ' i seçtiğinizde,** tüm gerekli alanları içeren formun geri kalanı, doldurmanız için sunulur. **Hayır ' ı seçtiğinizde,** form devre dışı bırakılır ve test sürücüsü devre dışı olarak yeniden yayımlarsanız, test sürücünüz üretimden kaldırılır.

Note: kullanıcılar tarafından etkin olarak kullanılan herhangi bir test sürücüsü varsa, bu test sürücüleri oturumunun süresi dolana kadar çalışmaya devam eder.

### <a name="details"></a>Ayrıntılar

Doldurulacak sonraki bölüm, test sürücünüzün Teklifinizle ilgili ayrıntılardır.

![Sınama sürücüsü ayrıntılı bilgileri](./media/azure-resource-manager-test-drive/howtopub2.png)

**Açıklama-** bu, test sürücünüzde olanlar hakkında ana açıklama yazmanız *gereken* yerdir. Müşteri, test sürücünüzün ürününüz hakkında hangi senaryolarda ele alınacaktır? bölümünü okumak için buraya gelir. 

**Kullanıcı el ile-** *Bu,* test sürücü deneyiminizin derinlemesine bir yönergedir. Müşteri bunu açar ve kendi test sürücüleri boyunca ne yapmak istediğinizi tam olarak ele alabilir. Bu içeriğin anlaşılması ve izlenmesi oldukça önemlidir! (Bir. PDF dosyası olmalıdır)

**Test sürücüsü tanıtım videosu-** Kullanıcı kılavuzuna benzer şekilde *önerilir* , test sürücüsü deneyiminizin video öğreticisini eklemek en iyisidir. Müşteri, test sürüşü öncesinde veya sırasında bunu izleyebilir ve test sürücüleri genelinde ne yapmak istediğinizi tam olarak ele alabilir. Bu içeriğin anlaşılması ve izlenmesi oldukça önemlidir!

- Videonuzun **ad** başlığı
- **Bağlantı** -tüp veya videoınızdan gömülü bir URL olmalıdır. Gömülü URL 'yi alma hakkında örnek aşağıda verilmiştir:
- **Küçük resim** -yüksek kaliteli bir görüntü (533x324) piksel olmalıdır. Buradaki test sürücüsü deneyiminizin bir kısmının ekran görüntüsünü almanız önerilir.

Aşağıda, bu alanların test sürücüsü deneyimi sırasında müşteriniz için nasıl gösterileceği gösterilmektedir.

![Market teklifinde test sürücüsü alanlarının konumu](./media/azure-resource-manager-test-drive/howtopub4.png)

### <a name="technical-configuration"></a>Teknik yapılandırma

Doldurmanız gereken sonraki bölüm, test sürücünüzün Kaynak Yöneticisi şablonunu karşıya yüklediğiniz ve özel olarak test sürücü örneklerinizin nasıl çalıştığını tanımlayacaksınız.

![](./media/azure-resource-manager-test-drive/howtopub5.png)

**Örnekler-** *Bu,* ne kadar örnek istediğinizi, hangi bölgelere ve müşterilerinizin test sürücüsünü ne kadar hızlı bir şekilde edindiklerinizi yapılandırdığınız yerdir.

- **Örnekler** -seçim bölgeleri, Test sürücünüzün Kaynak Yöneticisi şablonunun dağıtıldığı yeri seçtiğiniz yerdir. Yalnızca müşterilerinizin en fazla bir yerde bulunmasını istediğiniz bölgeyi seçmeniz önerilir.
- Daha önce dağıtılan ve seçilen bölge başına erişim bekleyen test sürücüsü örneklerinin **sık** erişimli sayısı. Müşteriler, bir dağıtımı beklemek yerine bu test sürücülerine anında erişebilir. Zorunluluğunu getirir, bu örneklerin her zaman Azure aboneliğinizde çalışıyor olması, bu nedenle daha büyük bir çalışma süresi maliyeti doğuracaktır. En **az bir etkin örnek**olması önerilir, çünkü müşterilerinizin çoğu tam dağıtımın bitmesini beklemek istemezsiniz ve müşterinin kullanımı için bir iade vardır.
- **Isınma** -dağıtılan bölge başına test sürücüsü örneklerinin sayısı ve ardından VM durdurulur ve Azure Storage 'da depolanır. Sıcak örnekler için bekleme süresi, etkin örneklerden daha yavaştır, ancak depolama alanının kullanım maliyeti de daha pahalı olur.
- **Soğuk** -dağıtılabilir olabilecek, bölge başına test sürücüsü örneklerinin sayısı. Soğuk örnekler, test sürücüsünü talep eden bir müşterinin bir dağıtım üzerinden gitmesini sağlamak için tüm test sürücüsü Kaynak Yöneticisi şablonu gerektirir, bu nedenle sık erişimli veya sıcak örneklerden daha yavaştır. Ancak, zorunluluğunu getirir yalnızca test sürücüsünün süresi için ödeme yapmanız gerekir.

Bu sırada, kullanılabilir hale getirmek istediğiniz olası eşzamanlı test sürücülerinin toplam sayısını hesaplar ve aboneliğiniz için kota sınırınızı Bu eşzamanlı miktarı işleyebildiğini doğrulayın:

**(X sıcak örnek Seçili bölge sayısı) + (Seçili bölge sayısı x sıcak örnek) + (Seçili bölge sayısı x soğuk örnekleri)**

**Sınama sürücüsü süresi (saat)-** *Required* test sürücüsünün etkin \# kalacağı süre (saat) için gerekli süre. Bu süre dolduktan sonra test sürücüsü otomatik olarak sona erer.

**Test sürücüsü kaynak yöneticisi şablonu-** *gerekli* Kaynak Yöneticisi şablonunuzu buraya yükleyin. Yukarıdaki yukarıdaki bölümde oluşturulan dosya budur. Ana şablon dosyasını adlandırın: "Main-Template. JSON" ve Kaynak Yöneticisi şablonunuz gereken anahtar değişkenleri için çıkış parametreleri içerdiğinden emin olun. (Bir. zip dosyası olmalıdır)

**Erişim bilgileri-** bir müşteri test sürücüsünü aldıktan sonra *gerekli* erişim bilgileri bu bilgilere sunulur. Bu yönergeler, test sürücünüzün Kaynak Yöneticisi şablonundaki yararlı çıkış parametrelerini paylaşmaktır. Çıkış parametrelerini dahil etmek için çift küme ayraçları kullanın (örneğin, **{{outputname}}**) ve konuma doğru eklenecektir. (Bu, ön uçta işlemek için HTML dize biçimlendirmesi önerilir).

### <a name="test-drive-deployment-subscription-details"></a>Test sürücüsü dağıtım aboneliği ayrıntıları

Doldurulacak son bölüm, Azure aboneliğinizi ve Azure Active Directory (AD) bağlantısını kurarak test sürücüleri otomatik olarak dağıtabilmelidir.

![Test sürücüsü dağıtım aboneliği ayrıntıları](./media/azure-resource-manager-test-drive/subdetails1.png)

**Azure ABONELIK kimliği-** *gerekli* bu, Azure hizmetlerine ve Azure Portal erişim izni verir. Abonelik, kaynak kullanımının raporlandığı ve hizmetlerin faturalandırılabildiği yerdir. Yalnızca test sürücüleri için **ayrı** bir Azure aboneliğiniz yoksa, devam edin ve bir tane yapın. Azure portal ' de oturum açarak ve sol taraftaki menüdeki aboneliklerde gezinerek Azure abonelik kimliklerini bulabilirsiniz. (Örnek: "a83645ac-1234-5AB6-345-1h234g764ghty")

![Azure Abonelikleri](./media/azure-resource-manager-test-drive/subdetails2.png)

**Azure AD KIRACı kimliği-** zaten kullanılabilir BIR kiracı *Kimliğiniz varsa,* AŞAĞıDAKI Özellikler-\> Dizin kimliğinde bulabilirsiniz.

![Azure Active Directory özellikleri](./media/azure-resource-manager-test-drive/subdetails3.png)

Aksi takdirde, Azure Active Directory yeni bir kiracı oluşturun.

![Azure Active Directory kiracılar listesi](./media/azure-resource-manager-test-drive/subdetails4.png)

![Azure AD kiracısı için kuruluş, etki alanı ve ülke/bölge tanımlama](./media/azure-resource-manager-test-drive/subdetails5.png)

![Seçimi onaylayın](./media/azure-resource-manager-test-drive/subdetails6.png)

**Azure AD UYGULAMASı kimliği-** *gerekli* sonraki adım yeni bir uygulama oluşturmak ve kaydetmek. Bu uygulamayı, test sürücü örneğiniz üzerinde işlem gerçekleştirmek için kullanacağız.

1. Yeni oluşturulan dizine veya zaten var olan dizine gidin ve filtre bölmesinde Azure Active Directory ' yi seçin.
2. "Uygulama kayıtları" araması yapın ve "Ekle" ye tıklayın
3. Bir uygulama adı girin.
4. "Web uygulaması/API" olarak türü seçin
5. Oturum açma URL 'SI içinde herhangi bir değer sağlayın, bu\'alanı kullandık.
6. Oluştur ' a tıklayın.
7. Uygulama oluşturulduktan sonra Özellikler ' e gidin-\> uygulamayı çok kiracılı olarak ayarlayın ve Kaydet ' e basın.

Kaydet’e tıklayın. Son adım, bu kayıtlı uygulamanın uygulama KIMLIĞINI alıp buradaki test sürücüsü alanına yapıştırmaktır.

![Azure AD uygulama KIMLIĞI ayrıntısı](./media/azure-resource-manager-test-drive/subdetails7.png)

Aboneliğe dağıtmak üzere uygulamayı kullandığımızda, uygulamayı aboneliğe katkıda bulunan olarak eklememiz gerekiyor. Bunlar için yönergeler aşağıda verilmiştir:

1. Abonelikler dikey penceresine gidin ve yalnızca test sürücüsü için kullandığınız uygun aboneliği seçin.
1. **Erişim denetimi (IAM)** öğesine tıklayın.
1. **Rol atamaları** sekmesine tıklayın.  ![Yeni Access Control sorumlusu ekleme](./media/azure-resource-manager-test-drive/SetupSub7_1.jpg)
1. **Rol ataması Ekle**' ye tıklayın.
1. Rolü **katkıda bulunan**olarak ayarlayın.
1. Azure AD uygulamasının adını yazın ve rolü atamak için uygulamayı seçin.
    ![İzinleri ekleme](./media/azure-resource-manager-test-drive/SetupSub7_2.jpg)
1. **Kaydet**’e tıklayın.

**Azure AD uygulaması anahtar-** son alan, bir kimlik doğrulama anahtarı oluşturmak için *gereklidir* . Anahtarlar ' ın altında bir anahtar açıklaması ekleyin, süresi hiçbir zaman dolmayacak şekilde ayarlayın ve ardından Kaydet ' i seçin. Bir zaman aşımına uğramaması **önemlidir** , bu, üretim ortamında test sürücünüzü bozacaktır. Bu değeri kopyalayın ve gerekli test sürücüsü alanına yapıştırın.

![Azure AD uygulaması için anahtarları gösterir](./media/azure-resource-manager-test-drive/subdetails8.png)

## <a name="next-steps"></a>Sonraki adımlar

Tüm test sürücüsü alanlarınızı doldurduktan sonra, teklifinizi gözden geçirin ve yeniden **yayımlayın** . Sınama sürücünüz sertifikayı geçtikten sonra teklifinizin **Önizlemedeki** müşteri deneyimini kapsamlı bir şekilde test etmeniz gerekir. Kullanıcı arabiriminde bir test sürücüsü başlatın ve Azure aboneliğinizi Azure portal içinde açın ve test sürücülerinizin tamamen doğru şekilde dağıtıldığını doğrulayın.

![Azure portal](./media/azure-resource-manager-test-drive/subdetails9.png)

Müşterilerinize sağlandıklarında test sürücü örneklerinin hiçbirini silmeyin, bu nedenle test sürücüsü hizmeti bu kaynak gruplarını bir müşteri tamamlandıktan sonra otomatik olarak temizler.

Önizleme teklifinizi rahat hissettiğinizde, şimdi **canlı çalışmaya devam**edersiniz! Teklif yayımlandıktan sonra, uçtan uca deneyimle tamamen emin olmak için Microsoft 'tan son bir gözden geçirme süreci vardır. Teklifin reddedilme bir nedeni varsa, teklifiniz için ne yapmanız gerektiğini açıklayan, mühendisiniz için mühendislik kişiye bir bildirim göndereceğiz.

Daha fazla sorunuz varsa, sorun giderme önerisi aranıyor veya test sürücünüzü daha başarılı hale getirmek istiyorsanız lütfen [SSS, sorun giderme & En Iyi Yöntemler](./marketing-and-best-practices.md)bölümüne gidin.
