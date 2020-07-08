---
title: Test sürücüleri türleri, Microsoft ticari Market
description: Ticari Market 'teki test sürücü türleri
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 06/19/2020
ms.author: dsindona
ms.openlocfilehash: a2d12a372f773a4a389ea7e7f2b3d2811ba889d3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85801232"
---
# <a name="azure-resource-manager-test-drive"></a>Test sürüşü Azure Resource Manager

Azure Market veya AppSource 'ta bir teklifiniz varsa ancak yalnızca Azure kaynaklarıyla bir test sürücüsü oluşturmak istiyorsanız bu türü kullanın. Azure Resource Manager (ARM) şablonu, çözümünüzü en iyi şekilde temsil edecek şekilde tasarlamanızı sağlayan, Azure kaynaklarının kodlanmış bir kapsayıcısıdır. Test sürücüsü, sunulan ARM şablonunu alır ve gereken tüm kaynakları bir kaynak grubuna dağıtır. Bu, sanal makine veya Azure Uygulama teklifleri için tek sınama sürücüsü seçeneğidir.

ARM şablonunun ne olduğunu bilmiyorsanız, [Azure Resource Manager nedir?](../azure-resource-manager/resource-group-overview.md) seçeneğini okuyun ve kendi şablonlarınızı nasıl oluşturup test etmek daha iyi anlamak için [ARM şablonlarının yapısını ve söz dizimini anlayın](../azure-resource-manager/resource-group-authoring-templates.md) .

**Barındırılan** veya **mantıksal uygulama** test sürücüsü hakkında bilgi için bkz. [test sürücüsü nedir?](what-is-test-drive.md)

## <a name="technical-configuration"></a>Teknik yapılandırma

Dağıtım şablonu, çözümünüzü oluşturan tüm Azure kaynaklarını içerir. Bu senaryoya uyan ürünler yalnızca Azure kaynaklarını kullanır. Iş Ortağı Merkezi 'nde aşağıdaki özellikleri ayarlayın:

- **Bölgeler** (gerekli) – Şu anda, test sürücünüzün kullanılabilir hale getirilbileceği 26 Azure tarafından desteklenen bölge vardır. Genellikle, en iyi performans için en yakın bölgeyi seçebilmeniz için, en fazla sayıda müşteriyi tahmin ettiğiniz bölgelerde test sürücünüzü kullanılabilir hale getirmek isteyeceksiniz. Aboneliğinizin seçtiğiniz her bölgede gereken tüm kaynakları dağıtmasına izin verildiğinden emin olmanız gerekir.

- **Örnekler** : teklifin kullanılabildiği bölge sayısıyla çarpılacak olan türü (sıcak veya soğuk) ve kullanılabilir örneklerin sayısını seçin.

  - **Sık** erişimli – bu örnek türü dağıtılır ve seçilen bölge başına erişim bekleniyor. Müşteriler, bir dağıtımı beklemek yerine bir test sürücüsünün *sık* erişimli örneklerine anında erişebilir. Zorunluluğunu getirir, bu örneklerin her zaman Azure aboneliğinizde çalışıyor olması, bu nedenle daha büyük bir çalışma süresi maliyeti doğuracaktır. En az bir *sıcak* örnek olması önemle önerilir, çünkü çoğu müşteri tam dağıtımları beklemek Istemelidir ve *etkin* örnek yoksa, müşteri kullanımındaki bir bırakmaya neden olur.

  - **Soğuk** – Bu örnek türü, bölge başına büyük olasılıkla dağıtılabilecek örneklerin toplam sayısını temsil eder. Soğuk örnekler, bir müşteri test sürücüsünü istediğinde tüm test sürücüsü Kaynak Yöneticisi şablonun dağıtılmasını gerektirir, böylece *soğuk* örnekler, *etkin* örneklere göre yüklenmeye çok daha yavaştır. Zorunluluğunu getirir, yalnızca test sürücüsünün süresi için ödeme yapmanız gereken *bir deyişle, Azure aboneliğinizde her zaman* bir *sıcak* örnekle birlikte çalışmıyor.

- **Test sürücüsü Azure Resource Manager şablonu** – Azure Resource Manager şablonunuzu içeren. zip ' i karşıya yükleyin. Hızlı başlangıç makalesinde Azure Resource Manager şablonu oluşturma hakkında daha fazla bilgi edinin [Azure Portal kullanarak Azure Resource Manager şablonları oluşturun ve dağıtın](../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md).

- **Sınama sürücüsü süresi** (zorunlu) – test sürücüsünün etkin kalacağı saat sayısını girin. Bu süre dolduktan sonra test sürücüsü otomatik olarak sona erer. Yalnızca tam sayılar kullanın (örneğin, "2" saatleri geçerlidir, "1,5" değildir).

## <a name="write-the-test-drive-template"></a>Test sürücüsü şablonunu yazma

İstenen kaynak paketini tasarladıktan sonra, test sürücüsü ARM şablonunu yazın ve oluşturun. Test sürücüsü dağıtımları tam otomatik modda çalıştırdığı için, test sürücü şablonlarında bazı kısıtlamalar vardır:

### <a name="parameters"></a>Parametreler

Çoğu şablon, kaynak adlarını, kaynak boyutlarını (depolama hesabı türleri veya sanal makine boyutları gibi), Kullanıcı adlarını ve parolaları, DNS adlarını vb. tanımlayan bir dizi parametreye sahiptir. Azure portal kullanarak çözümler dağıttığınızda, tüm bu parametreleri el ile doldurabilir, kullanılabilir DNS adlarını veya depolama hesabı adlarını seçebilir ve bu şekilde devam edebilirsiniz.

![Azure Resource Manager parametrelerinin listesi](media/test-drive/resource-manager-parameters.png)

Ancak, test sürücüsü insan etkileşimi olmadan otomatik olarak çalışarak yalnızca sınırlı sayıda parametre kategorisi kümesini destekler. Test sürücüsü ARM şablonundaki bir parametre desteklenen kategorilerden birine denk değilse, bu parametreyi bir değişken veya sabit değer ile değiştirmelisiniz.

Parametreleriniz için geçerli herhangi bir ad kullanabilirsiniz; test sürücüsü, meta veri türü değeri kullanarak parametre kategorisini tanır. Her şablon parametresi için meta veri türü belirtin, aksi takdirde şablonunuz doğrulamadan geçmeyecektir:

```JSON
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

> [!NOTE]
> Tüm parametreler isteğe bağlıdır, bu nedenle herhangi birini kullanmak istemiyorsanız, yapmanız gerekmez.

### <a name="accepted-parameter-metadata-types"></a>Kabul edilen parametre meta veri türleri

| Meta veri türü   | Parametre türü  | Açıklama     | Örnek değer    |
|---|---|---|---|
| **BaseUri**     | string          | Dağıtım paketinizin temel URI 'SI| `https:\//\<\..\>.blob.core.windows.net/\<\..\>` |
| **nitelen**    | string          | Yeni rastgele Kullanıcı adı.| admin68876      |
| **parola**    | güvenli dize    | Yeni rastgele parola | LP! ACS \^ 2kh     |
| **oturum kimliği**   | string          | Benzersiz test sürücüsü oturum KIMLIĞI (GUID)    | b8c8693e-5673-449c-badd-257a405a6dee |

#### <a name="baseuri"></a>BaseUri

Test sürücüsü bu parametreyi dağıtım paketinizin **temel URI** 'siyle başlatır, böylece paketinize dahil olan herhangi bir dosyanın URI 'sini oluşturmak için bu parametreyi kullanabilirsiniz.

```JSON
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

Test sürücüsü dağıtım paketinizden herhangi bir dosyanın URI 'Sini oluşturmak için bu parametreyi şablonunuz içinde kullanın. Aşağıdaki örnek, bağlantılı şablonun URI 'Sini oluşturmayı gösterir:

```JSON
"templateLink": {
  "uri": "[concat(parameters('baseuri'),'templates/solution.json')]",
  "contentVersion": "1.0.0.0"
}
```

#### <a name="username"></a>kullanıcı adı

Test sürücüsü bu parametreyi yeni bir rastgele Kullanıcı adı ile başlatır:

```JSON
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

Örnek değer:`admin68876`

Çözümünüz için rastgele veya sabit Kullanıcı adlarını kullanabilirsiniz.

#### <a name="password"></a>password

Test sürücüsü bu parametreyi yeni bir rastgele parolayla başlatır:

```JSON
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

Örnek değer:`Lp!ACS^2kh`

Çözümünüz için rastgele veya sabit parolalar kullanabilirsiniz.

#### <a name="session-id"></a>oturum KIMLIĞI

Test sürücüsü bu parametreyi test sürücüsü oturum KIMLIĞINI temsil eden benzersiz bir GUID ile başlatır:

```JSON
"parameters": {
  ...
  "sessionid": {
    "type": "string",
    "metadata": {
      "type": "sessionid",
      "description": "Unique test drive session id."
    }
  },
  ...
}
```

Örnek değer:`b8c8693e-5673-449c-badd-257a405a6dee`

Bu parametreyi, gerekirse test sürücüsü oturumunu benzersiz şekilde tanımlamak için kullanabilirsiniz.

### <a name="unique-names"></a>Benzersiz adlar

Depolama hesapları veya DNS adları gibi bazı Azure kaynakları, genel olarak benzersiz adlar gerektirir. Bu, her bir test sürücüsü ARM şablonunu dağıttığı anlamına gelir, bu, tüm kaynakları için benzersiz bir ada sahip yeni bir kaynak grubu oluşturur. Bu nedenle, rastgele benzersiz değerler oluşturmak için kaynak grubu kimliklerinde değişken adlarınızla birleştirilmiş [uniquestring](../azure-resource-manager/templates/template-functions.md) işlevini kullanmanız gerekir:

```JSON
"variables": {
  ...
  "domainNameLabel": "[concat('contosovm',uniquestring(resourceGroup().id))]",
  "storageAccountName": "[concat('contosodisk',uniquestring(resourceGroup().id))]",
  ...
}
```

Parametre/değişken dizelerinizi ( `contosovm` ) benzersiz bir dize çıkışı () ile birleştirmeniz `resourceGroup().id` , bu nedenle her bir değişkenin benzersizliğini ve güvenilirliğini garanti ettiğinden emin olun.

Örneğin, çoğu kaynak adı bir basamakla başlayamaz, ancak benzersiz dize işlevi bir sayıyla başlayan bir dize döndürebilir. Bu nedenle, ham benzersiz dize çıkışı kullanırsanız dağıtımlarınız başarısız olur.

[Bu makaledeki](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging)kaynak adlandırma kuralları ve kısıtlamaları hakkında daha fazla bilgi bulabilirsiniz.

### <a name="deployment-location"></a>Dağıtım Konumu

Test sürücünüzü farklı Azure bölgelerinde kullanılabilir hale getirebilirsiniz. Fikir, bir kullanıcının Beast Kullanıcı deneyimiyle birlikte sağlaması için en yakın bölgeyi seçmesine izin versağlamaktır.

Test sürücüsü laboratuvarın bir örneğini oluşturduğunda, her zaman bir kullanıcı tarafından seçilen bölgede bir kaynak grubu oluşturur ve ardından bu grup bağlamında dağıtım şablonunuzu yürütür. Bu nedenle, şablonunuz kaynak grubundan dağıtım konumunu seçmelidir:

```JSON
"variables": {
  ...
  "location": "[resourceGroup().location]",
  ...
}
```

Ardından, belirli bir laboratuvar örneği için bu konumu her kaynak için kullanın:

```JSON
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

Aboneliğinizin seçtiğiniz her bölgede istediğiniz tüm kaynakları dağıtmasına izin verildiğinden emin olun. Ayrıca, sanal makine görüntülerinizin etkinleştirebildiğiniz tüm bölgelerde kullanılabilir olduğundan emin olun, aksi takdirde dağıtım şablonunuz bazı bölgelerde çalışmaz.

### <a name="outputs"></a>Çıkışlar

Normalde Kaynak Yöneticisi şablonlar ile herhangi bir çıkış üretmeksizin dağıtım yapabilirsiniz. Bunun nedeni, şablon parametrelerini doldurmak için kullandığınız tüm değerleri bilmeniz ve herhangi bir kaynağın özelliklerini her zaman el ile inceleyebilirsiniz.

Bununla birlikte test sürücüsü Kaynak Yöneticisi şablonları için, test sürücüsüne (Web sitesi URI 'Leri, sanal makine ana bilgisayar adları, Kullanıcı adları ve parolalar) erişim sağlamak için gereken tüm bilgiler için geri dönmek önemlidir. Bu değişkenler müşteriye sunulduğundan tüm çıkış adlarınızın okunabilir olduğundan emin olun.

Şablon çıkışları ile ilgili herhangi bir kısıtlama yoktur. Test sürücüsü tüm çıktı değerlerini dizelere dönüştürür, bu nedenle çıktıya bir nesne gönderirseniz, bir Kullanıcı JSON dizesini görür.

Örnek:

```JSON
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

Abonelik ve hizmet limitlerini unutmayın. Örneğin, on 4 çekirdekli sanal makine dağıtmak istiyorsanız, laboratuvarınız için kullandığınız aboneliğin 40 çekirdek kullanmanıza izin verdiğinden emin olmanız gerekir. Azure aboneliği ve hizmet limitleri hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md). Aynı anda birden çok test sürücüsü alınacağından, aboneliğinizin alınabilecek toplam eşzamanlı test sürücüsü sayısıyla çarpıldığı çekirdek sayısını işleyebildiğini doğrulayın.

### <a name="what-to-upload"></a>Karşıya yüklenecek

Test sürücüsü ARM şablonu, çeşitli dağıtım yapıtları içerebilen bir ZIP dosyası olarak karşıya yüklenir, ancak adında bir dosya olmalıdır `main-template.json` . Bu, test sürücüsünün bir laboratuvarın örneğini oluşturmak için kullandığı ARM dağıtım şablonudur. Bu dosyanın ötesinde ek kaynaklarınız varsa, bunlara şablon içinde dış kaynaklar olarak başvurabilir veya bunları ZIP dosyasına ekleyebilirsiniz.

Yayımlama sertifikası sırasında, dağıtım paketinizi IP 'leri test edin ve içeriğini dahili bir test sürücüsü blob kapsayıcısına koyar. Kapsayıcı yapısı, dağıtım paketinizin yapısını yansıtır:

| package.zip                       | Test sürücüsü blob kapsayıcısı         |
|---|---|
| `main-template.json`                | `https:\//\<\...\>.blob.core.windows.net/\<\...\>/main-template.json`  |
| `templates/solution.json`           | `https:\//\<\...\>.blob.core.windows.net/\<\...\>/templates/solution.json` |
| `scripts/warmup.ps1`                | `https:\//\<\...\>.blob.core.windows.net/\<\...\>/scripts/warmup.ps1`  |
|||

Bu blob kapsayıcı temel URI 'sinin bir URI 'Sini çağırdık. Laboratuvarınızın her düzeltmesinin kendi BLOB kapsayıcısı olduğundan, laboratuvarınızın her düzeltmesinin kendi temel URI 'Si vardır. Test sürücüsü, daraltılmış dağıtım paketinizin temel bir URI 'Sini şablon parametreleri aracılığıyla şablonunuza geçirebilir.

### <a name="transform-template-examples-for-test-drive"></a>Test sürücüsü için dönüştürme şablonu örnekleri

Bir kaynak mimarisini bir test sürücüsüne açma işlemi Kaynak Yöneticisi şablonu olabilir. Ek Yardım için, geçerli dağıtım şablonlarının en iyi şekilde dönüştürülmesiyle ilgili şu örneklere bakın: [test sürücüsü nedir?](what-is-test-drive.md#transforming-examples).

## <a name="test-drive-deployment-subscription-details"></a>Test sürücüsü dağıtım aboneliği ayrıntıları

Tamamlanacak son bölüm, Azure aboneliğinizi ve Azure Active Directory (AD) bağlantısını kurarak test sürücüleri otomatik olarak dağıtabilmelidir.

![Test sürücüsü dağıtım aboneliği ayrıntıları](media/test-drive/deployment-subscription-details.png)

1. **Azure ABONELIK kimliği**alın. Bu, Azure hizmetlerine ve Azure portal erişim izni verir. Abonelik, kaynak kullanımının raporlandığı ve hizmetlerin faturalandırılabildiği yerdir. Yalnızca test sürücüleri için ayrı bir Azure aboneliğiniz yoksa, bir tane yapın. `1a83645ac-1234-5ab6-6789-1h234g764ghty1`Azure Portal için oturum açarak ve sol gezinti menüsünden **abonelikler** ' i seçerek Azure abonelik kimliklerini (örneğin,) bulabilirsiniz.

   ![Azure Abonelikleri](media/test-drive/azure-subscriptions.png)

2. **Azure AD KIRACı kimliği**edinin. Zaten kullanılabilir bir kiracı Kimliğiniz varsa, **Azure Active Directory**  >  **Özellikler**  >  **dizin kimliği**' nde bulabilirsiniz.

   ![Azure Active Directory özellikleri](media/test-drive/azure-active-directory-properties.png)

   Kiracı KIMLIĞINIZ yoksa Azure Active Directory yeni bir tane oluşturun. Kiracı ayarlama konusunda yardım için bkz. [hızlı başlangıç: kiracı ayarlama](../active-directory/develop/quickstart-create-new-tenant.md).

3. **Azure AD UYGULAMASı kimliği** – yeni bir uygulama oluşturun ve kaydedin. Bu uygulamayı, test sürücü örneğiniz üzerinde işlem gerçekleştirmek için kullanacağız.

   1. Yeni oluşturulan dizine veya zaten var olan dizine gidin ve filtre bölmesinde Azure Active Directory ' yi seçin.
   2. **Uygulama kayıtları** arayın ve **Ekle**' yi seçin.
   3. Bir uygulama adı girin.
   4. **Web uygulaması/API** **türünü** seçin.
   5. Oturum açma URL 'sinde herhangi bir değer sağlayın, bu alan kullanılmaz.
   6. **Oluştur**'u seçin.
   7. Uygulama oluşturulduktan sonra **Özellikler**' i,  >  **uygulamayı çok kiracılı olarak ayarla** ve sonra **Kaydet**' i seçin.

4. **Kaydet**'i seçin.

5. Bu kayıtlı uygulamanın uygulama KIMLIĞINI kopyalayın ve test sürücüsü alanına yapıştırın.

   ![Azure AD uygulama KIMLIĞI ayrıntısı](media/test-drive/azure-ad-application-id-detail.png)

6. Aboneliğe dağıtılacak uygulamayı kullandığımızdan, uygulamayı aboneliğe katkıda bulunan olarak eklememiz gerekir:

   1. Test sürücüsü için kullanmakta olduğunuz **aboneliğin** türünü seçin.
   1. **Erişim denetimi (IAM)** öğesini seçin.
   1. **Rol atamaları** sekmesini seçin ve **rol ataması ekleyin**.

      ![Yeni Access Control sorumlusu ekleme](media/test-drive/access-control-principal.jpg)

   1. **Rolü** ayarlayın ve gösterilen şekilde **erişimi atayın** . **Seç** ALANıNA Azure AD uygulamasının adını girin. **Katkıda** bulunan rolünü atamak istediğiniz uygulamayı seçin.

      ![İzinleri ekleme](media/test-drive/access-control-permissions.jpg)

   1. **Kaydet**'i seçin.

7. **Azure AD uygulaması** bir kimlik doğrulama anahtarı oluşturun. **Anahtarlar**' ın altında, bir **anahtar açıklaması**ekleyin, süreyi **süresiz** olarak ayarlayın (süresi dolan bir anahtar, üretimde test sürücünüzü keser) ve ardından **Kaydet**' i seçin. Bu değeri kopyalayın ve gerekli Test Drive alanına yapıştırın.

![Azure AD uygulaması için anahtarları gösterir](media/test-drive/azure-ad-app-keys.png)

## <a name="republish"></a>Yeniden yayımlama

Tüm test sürücüsü alanlarınız tamamlandığına göre, teklifinizi yeniden **yayımlayın** . Sınama sürücünüz sertifikayı geçtikten sonra müşteri deneyimini teklifinizin önizlemesine göre test edin:

1. Kullanıcı arabiriminde bir test sürücüsü başlatın.
1. Azure aboneliğinizi Azure portal içinde açın.
1. Test sürücünüzün doğru bir şekilde dağıtmakta olduğunu doğrulayın.

   ![Azure portal](media/test-drive/azure-portal.png)

Müşterileriniz için sağlanan test sürücü örneklerini silmeyin; test sürücüsü hizmeti, bir müşteri bittikten sonra bu kaynak gruplarını otomatik olarak temizler.

Önizleme teklifinizi rahat bir şekilde kullanmaya başladıktan sonra **canlı çalışmaya devam**edersiniz! Uçtan uca deneyimin tamamını iki kez kontrol etmek için son bir gözden geçirme süreci vardır. Teklifi reddettireceğiz, teklifinizin ne kadar düzeltilmesi gerektiğini belirten mühendisin kişileriyle ilgili olarak e-posta göndereceğiz.

## <a name="next-steps"></a>Sonraki adımlar

- Iş Ortağı Merkezi 'nde Teklifinizi oluşturma yönergelerini takip ediyorsanız, bu konuya dönmek için geri okunu kullanın.
- [Test sürücüsü olan](what-is-test-drive.md)diğer test sürücüsü türleri hakkında daha fazla bilgi edinin.
