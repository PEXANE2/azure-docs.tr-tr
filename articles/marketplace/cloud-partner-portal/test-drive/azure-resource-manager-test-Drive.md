---
title: Azure Kaynak Yöneticisi Test Sürüşü | Azure Marketi
description: Azure Kaynak Yöneticisi'ni kullanarak Bir Pazar Test Sürücüsü Oluşturma
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 6125aa010d8676518b84f866343b01f95246160f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275943"
---
# <a name="azure-resource-manager-test-drive"></a>Azure Resource Manager Test Sürüşü

Bu makale, Azure Marketi'nde teklifi olan veya AppSource'ta bulunan ancak Test Sürüşlerini yalnızca Azure kaynaklarıyla oluşturmak isteyen Yayıncılar içindir.

Azure Kaynak Yöneticisi (Kaynak Yöneticisi) şablonu, çözümünüzü en iyi şekilde temsil edecek şekilde tasarladığınız kodlanmış azure kaynakları kapsayıcısır. Kaynak Yöneticisi şablonunu bilmiyorsanız, kendi şablonlarınızı nasıl oluşturup test edeceğinden emin olmak için [Kaynak Yöneticisi şablonlarını anlama](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) ve Kaynak Yöneticisi [şablonları yazma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) hakkında bilgi edinin.

Test Çalışması'nın yaptığı, sağlanan Kaynak Yöneticisi şablonuna alınması ve bu Kaynak Yöneticisi şablonundan gerekli tüm kaynakların bir kaynak grubuna dağıtılmasıdır.

Bir Azure Kaynak Yöneticisi Test Sürüşü oluşturmayı seçerseniz, aşağıdakileri yapmanızı n için gerekli olan gereksinimler şunlardır:

- Test Sürüşü Kaynak Yöneticisi şablonunuzu oluşturun, test edin ve yükleyin.
- Test Sürüşünüzü etkinleştirmek için gerekli tüm meta verileri ve ayarları yapılandırın.
- Test Sürüşü etkinken teklifinizi yeniden yayınlayın.

## <a name="how-to-build-an-azure-resource-manager-test-drive"></a>Azure Kaynak Yöneticisi Test Sürüşü nasıl oluşturur?

Azure Kaynak Yöneticisi Test Sürüşü oluşturma işlemi aşağıda veda edinilir:

1. Müşterilerinizin akış diyagramında ne yapmasını istediğinizi tasarla.
1. Müşterilerinizin hangi deneyimleri oluşturmasını istediğinizi tanımlayın.
1. Yukarıdaki tanımlara dayanarak, müşterilerin bu tür bir deneyimi gerçekleştirmesi için hangi parçalara ve kaynaklara ihtiyaç duyulduğuna karar verin: örneğin, D365 örneği veya veritabanı olan bir web sitesi.
1. Tasarımı yerel olarak oluşturun ve deneyimi test edin.
1. Deneyimi ARM şablon dağıtımında ve oradan paketleyin:
    1. Kaynakların hangi bölümlerinin giriş parametreleri olduğunu tanımlayın;
    1. Değişkenler nelerdir;
    1. Müşteri deneyimine hangi çıktılar verilir.
1. Yayınlayın, test edin ve yayınlayın.

Azure Kaynak Yöneticisi Test Sürüşü oluşturmanın en önemli kısmı, müşterilerinizin hangi senaryoyu yaşamasını istediğinizi tanımlamaktır. Bir güvenlik duvarı ürünü ve ne kadar iyi komut dosyası enjeksiyon saldırıları ele demo istiyorum? Bir depolama ürünü müyseniz ve çözümünüzün dosyaları ne kadar hızlı ve kolay sıkıştırdığını nida etmek ister misiniz?

Ürününüzü göstermenin en iyi yollarını değerlendirerek yeterli miktarda zaman harcadığınızdan emin olun. Kaynak Yöneticisi şablonunun yeterince kolay paketlenmesi nden, özellikle ihtiyacınız olan tüm kaynakların etrafında.

Güvenlik duvarı örneğimize devam etmek için mimari, hizmetiniz için genel bir IP URL'ye ve güvenlik duvarınızın koruduğu web sitesi için başka bir genel IP URL'sine ihtiyacınız olabilir. Her IP sanal makinede dağıtılır ve bir ağ güvenlik grubu + ağ arabirimi ile birlikte bağlanır.

İstenilen kaynak paketini tasarladıktan sonra, test sürüşü kaynak yöneticisi şablonunun yazımı ve oluşturulması gelir.

## <a name="writing-test-drive-resource-manager-templates"></a>Test Sürücüsü Kaynak Yöneticisi şablonları yazma

Test Sürüşü dağıtımları tam otomatik modda çalıştırır ve bu nedenle Test Drive şablonlarının aşağıda açıklanan bazı kısıtlamaları vardır.

### <a name="parameters"></a>Parametreler

Çoğu şablonun bir dizi parametresi vardır. Parametreler kaynak adlarını, kaynak boyutlarını (örneğin, depolama hesabı türleri veya sanal makine boyutları), kullanıcı adları ve parolalar, DNS adları vb. tanımlar. Azure portalLarını kullanarak çözümleri dağıttığınızda, tüm bu parametreleri el ile doldurabilir, kullanılabilir DNS adlarını veya depolama hesabı adlarını seçebilir ve benzeri şeyler yapabilirsiniz.

![Azure Kaynak Yöneticisi'ndeki parametrelerin listesi](./media/azure-resource-manager-test-drive/param1.png)

Ancak, Test Sürüşü insan etkileşimi olmadan tam otomatik modda çalışır, bu nedenle yalnızca sınırlı bir parametre kategorisini destekler. Test Sürücüsü Kaynak Yöneticisi şablonundaki bir parametre desteklenen kategorilerden birine düşmüyorsa, **bu parametreyi değişken veya sabit değerle değiştirmeniz gerekir.**

Parametreleriniz için herhangi bir geçerli ad kullanabilirsiniz, Test Drive meta veri türü değeri kullanarak parametre kategorisini tanır. **Her şablon parametresi için meta veri türü belirtmeniz gerekir,** aksi takdirde şablonunuz doğrulamayı geçemez:

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

**Ayrıca tüm parametrelerin isteğe bağlı olduğunu**unutmayın,\'bu nedenle herhangi bir\'kullanmak istemiyorsanız, bunu yapmak zorunda değilsiniz.

### <a name="accepted-parameter-metadata-types"></a>Kabul Edilen Parametre Meta veri türleri

| Meta veri türü   | Parametre Türü  | Açıklama     | Örnek Değer    |
|---|---|---|---|
| **Baseuri**     | string          | Dağıtım paketinizin Temel URI'si| \//\<https:\.. \>.blob.core.windows.net/\<\..\> |
| **Username**    | string          | Yeni rasgele kullanıcı adı.| admin68876      |
| **parola**    | güvenli dize    | Yeni rasgele parola | Lp!ACS\^2kh     |
| **oturum kimliği**   | string          | Benzersiz Test Sürüşü oturum kimliği (GUID)    | b8c8693e-5673-449c-badd-257a405a6dee |

#### <a name="baseuri"></a>Baseuri

Test Sürüşü bu parametreyi dağıtım paketinizin **Base Uri'si** ile açar, böylece paketinizde yer alan herhangi bir dosyanın Uri'sini oluşturmak için bu parametreyi kullanabilirsiniz.

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

Şablonunuzun içinde, Test Sürüşü dağıtım paketinizden herhangi bir dosyanın Uri'sini oluşturmak için bu parametreyi kullanabilirsiniz. Aşağıdaki örnek, bağlantılı şablonun Uri'sinin nasıl inşa edilebildiğini gösterir:

```json
"templateLink": {
  "uri": "[concat(parameters('baseuri'),'templates/solution.json')]",
  "contentVersion": "1.0.0.0"
}
```

#### <a name="username"></a>kullanıcı adı

Test Sürücüsü bu parametreyi yeni bir rasgele kullanıcı adı ile başharfe sunar:

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

Çözümünüz için rasgele veya sabit kullanıcı adlarını kullanabilirsiniz.

#### <a name="password"></a>password

Test Sürücüsü bu parametreyi yeni bir rasgele parolayla başharfe ait hale leştirir:

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

Çözümünüz için rasgele veya sabit parolalar kullanabilirsiniz.

#### <a name="session-id"></a>oturum kimliği

Test Sürüşü, test sürüşü oturum kimliğini temsil eden benzersiz bir GUID ile bu parametreyi başlatmayı

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

Gerekirse Test Sürüşü oturumunu benzersiz olarak tanımlamak için bu parametreyi kullanabilirsiniz.

### <a name="unique-names"></a>Benzersiz İsimler

Depolama hesapları veya DNS adları gibi bazı Azure kaynakları, genel olarak benzersiz adlar gerektirir.

Bu, Test Drive Kaynak Yöneticisi şablonunu her dağıtınca, tüm\' kaynakları için benzersiz bir **ada sahip yeni bir kaynak grubu** oluşturduğu anlamına gelir. Bu nedenle rasgele benzersiz değerler oluşturmak için kaynak grubu dislerinde değişken adlarınız ile birlikte [benzersiz dize](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions#uniquestring) işlevini kullanmak gerekir:

```json
"variables": {
  ...
  "domainNameLabel": "[concat('contosovm',uniquestring(resourceGroup().id))]",
  "storageAccountName": "[concat('contosodisk',uniquestring(resourceGroup().id))]",
  ...
}
```

Parametre/değişken\'dizelerinizi (contosovm)\'benzersiz bir dize çıkışıyla\'(resourceGroup().id)\'birleştirirsiniz, çünkü bu her değişkenin benzersizliğini ve güvenilirliğini garanti eder.

Örneğin, çoğu kaynak adı bir basamakla başlayamaz, ancak benzersiz dize işlevi bir basamakla başlayan bir dize döndürebilir. Bu nedenle, ham benzersiz dize çıktısı kullanırsanız, dağıtımlarınız başarısız olur. 

Kaynak adlandırma kuralları ve kısıtlamaları hakkında ek bilgileri [bu makalede](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging)bulabilirsiniz.

### <a name="deployment-location"></a>Dağıtım Konumu

Test Sürüşü'ni farklı Azure bölgelerinde kullanılabilir hale getirebilirsiniz. Fikir, bir kullanıcının en yakın bölgeyi seçmesine izin vermek, canavar kullanıcı deneyimini sağlamaktır.

Test Çalışması Laboratuvar'ın bir örneğini oluşturduğunda, her zaman bir kullanıcı tarafından seçtiği bölgede bir kaynak grubu oluşturur ve dağıtım şablonunuzu bu grup bağlamında yürütür. Bu nedenle, şablonunuz kaynak grubundan dağıtım konumunu seçmelidir:

```json
"variables": {
  ...
  "location": "[resourceGroup().location]",
  ...
}
```

Ve sonra belirli bir Laboratuvar örneği için her kaynak için bu konumu kullanın:

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

Aboneliğinizin, dağıtmak istediğiniz tüm kaynakları seçtiğiniz bölgelerin her birinde dağıtmasına izin verdiğinden emin olmanız gerekir. Ayrıca, sanal makine görsellerinizin etkinleştireceğiniz tüm bölgelerde kullanılabilir olduğundan emin olmanız gerekir, aksi takdirde dağıtım şablonunuz bazı bölgelerde çalışmaz.

### <a name="outputs"></a>Çıkışlar

Normalde Kaynak Yöneticisi şablonları ile, herhangi bir çıktı üretmeden dağıtabilirsiniz. Bunun nedeni, şablon parametrelerini doldurmak için kullandığınız tüm değerleri bilmeniz ve herhangi bir kaynağın özelliklerini her zaman el ile inceleyebilmenizdir.

Ancak Test Drive Kaynak Yöneticisi\'şablonları için, laboratuvara erişim sağlamak için gereken tüm bilgileri Test Drive'a (Web Sitesi URL'leri, Sanal Makine ana bilgisayar adları, kullanıcı adları ve parolalar) döndürmek önemlidir. Bu değişkenler müşteriye sunulduğundan, tüm çıktı adlarınızın okunabilir olduğundan emin olun.

Şablon çıktıları ile ilgili herhangi bir kısıtlama yoktur. Unutmayın, Test Sürüşü tüm çıktı değerlerini **dizeleri**dönüştürür, bu nedenle çıktıya bir nesne gönderirseniz, kullanıcı JSON dizesini görür.

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

### <a name="subscription-limits"></a>Abonelik Limitleri

Dikkate almalısınız bir şey daha abonelik ve hizmet sınırlarıdır. Örneğin, en fazla on adet 4 çekirdekli sanal makine dağıtmak istiyorsanız, Laboratuvarınız için kullandığınız aboneliğin 40 çekirdek kullanmanıza izin verdiğinden emin olmanız gerekir.

Azure aboneliği ve hizmet sınırları hakkında daha fazla bilgiyi [bu makalede](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits)bulabilirsiniz. Aynı anda birden çok Test Sürücüsü alınabildiği için, \# aboneliğinizin alınabilecek toplam eşzamanlı Test Sürücüsü sayısıyla çarpılarak çekirdeklerin sayısını işleyebilir.

### <a name="what-to-upload"></a>Ne yüklemek için

Test Drive Kaynak Yöneticisi şablonu, çeşitli dağıtım yapıları içerebilir bir zip dosyası olarak yüklenir, ancak **main-template.json**adlı bir dosya olması gerekir. Bu dosya Azure Kaynak Yöneticisi dağıtım şablonudur ve Test Drive bu dosyayı bir Laboratuarı anında oluşturmak için kullanır.

Bu dosyanın ötesinde ek kaynaklarınız varsa, şablonun içinde dış kaynak olarak başvuruda bulunabilir veya kaynağı zip dosyasına ekleyebilirsiniz.

Yayımlama sertifikası sırasında Test Sürüşü dağıtım paketinizin zip'ini çıkarır ve içeriğini dahili bir Test Drive blob kapsayıcısına koyar. Kapsayıcı yapısı dağıtım paketinizin yapısını yansıtır:

| paket.zip                       | Test Drive blob konteyner         |
|---|---|
| ana-template.json                | \//\<https:\... \>.blob.core.windows.net/\<\... \>/main-template.json  |
| şablonlar/solution.json           | \//\<https:\... \>.blob.core.windows.net/\<\... \>/templates/solution.json |
| komut dosyaları/ısınma.ps1                | \//\<https:\... \>.blob.core.windows.net/\<\... \>/komut dosyaları/ısınma.ps1  |


Bu blob konteynerinuri'si olan Bir Uri'ye Base Uri diyoruz. Laboratuarınızın her revizyonunun kendi blob kabı vardır ve bu nedenle, Laboratuarınızın her revizyonunun kendi Base Uri'si vardır. Test Sürüşü, şablon parametreleri aracılığıyla şablonunuza sıkıştırılmamış dağıtım paketinizin Base Uri'sini geçirebilir.

## <a name="transforming-template-examples-for-test-drive"></a>Test Sürüşü için Şablon Örneklerini Dönüştürme

Kaynakların mimarisini Test Drive Kaynak Yöneticisi şablonuna dönüştürme işlemi göz korkutucu olabilir. Bu işlemin kolaylaşmasına yardımcı\'olmak için, mevcut [dağıtım şablonlarını](./transforming-examples-for-test-drive.md)en iyi şekilde nasıl dönüştürebileceğimize ilişkin örnekler verdik.

## <a name="how-to-publish-a-test-drive"></a>Test Sürüşü nasıl yayımlanır?

Artık Test Sürüşünüzü oluşturduğuna göre, bu bölüm Test Sürüşünüzü başarıyla yayımlamanız için gereken alanların her birinde gezinir.

![Kullanıcı arabiriminde Test Sürücüsü'nün etkinleştirilmesi](./media/azure-resource-manager-test-drive/howtopub1.png)

İlk ve en önemli alan, teklifiniz için Test Sürüşü'nün etkin olmasını isteyip istemediğiniz konusunda geçiş yapmaktır. **Evet'i seçtiğinizde,** formun geri kalanı gerekli alanların doldurulması için sunulur. **Hayır'ı seçtiğinizde,** form devre dışı bırakılır ve Test Sürüşü devre dışı bırakılmışsa, Test Sürücünüz üretimden kaldırılır.

Not: Kullanıcılar tarafından etkin olarak kullanılan herhangi bir Test Sürücüleri varsa, bu Test Sürücüleri oturumları sona erene kadar çalışmaya devam eder.

### <a name="details"></a>Ayrıntılar

Doldurulması gereken bir sonraki bölüm, Test Sürüşü teklifiniz hakkındaki ayrıntılardır.

![Test Sürüşü detaylı bilgi](./media/azure-resource-manager-test-drive/howtopub2.png)

**Açıklama -** *Gerekli* Bu test sürücünüzde ne olduğu hakkında ana açıklama yazmak yerdir. Müşteri, Test Sürüşünüzün ürününizle ilgili hangi senaryoları ele alacağınızı okumak için buraya gelecektir. 

**Kullanım Kılavuzu -** *Gerekli* Bu, Test Sürüşü deneyiminizin derinlemesine gözden geçirimidir. Müşteri bunu açar ve Test Sürüşü boyunca tam olarak ne yapmalarını istediğinizi gözden geçirebilirsiniz. Bu içeriğin anlaşılması ve takip edilebilen kolay olması önemlidir! (Bir .pdf dosyası olmalıdır)

**Test Sürüşü Demo Videosu -** *Önerilen* Kullanım Kılavuzu'na benzer şekilde, Test Sürüşü deneyiminizi içeren bir video eğitimi eklemek en iyisidir. Müşteri bunu test sürüşünden önce veya sırasında izleyecek ve Test Sürüşü boyunca tam olarak ne yapmalarını istediğinizi gözden geçirebilecektir. Bu içeriğin anlaşılması ve takip edilebilen kolay olması önemlidir!

- **Adı** - Videonuzun Adı
- **Bağlantı** - Tüpünüzden veya videonuzdan gömülü bir URL olmalıdır. Gömülü url'nin nasıl alınabildiğini anlatan örnek aşağıdadır:
- **Küçük resim** - Yüksek kaliteli görüntü (533x324) piksel olmalıdır. Test Sürüşü deneyiminizin bir bölümünün ekran görüntüsünü burada almanız önerilir.

Aşağıda, bu alanların Test Sürüşü deneyimi sırasında müşteriniz için nasıl gösteriş yaptığı aşağıda verilmiştir.

![Test Sürüşü alanlarının Pazar Daki Konumu teklifi](./media/azure-resource-manager-test-drive/howtopub4.png)

### <a name="technical-configuration"></a>Teknik Yapılandırma

Doldurulması gereken bir sonraki bölüm, Test Sürücüsü Kaynak Yöneticisi şablonunuzu yüklediğiniz ve Test Sürüşü örneklerinizin özel olarak nasıl çalıştığını tanımladığınız bölümdür.

![](./media/azure-resource-manager-test-drive/howtopub5.png)

**Örnekler -** *Gerekli* Bu, istediğiniz kaç örnek, hangi bölgede (ler) ve müşterilerinizin Test Drive'ı ne kadar hızlı alabileceğinizi yapılandırdığınız yerdir.

- **Örnekler** - Seç bölgeleri, Test Sürücüsü Kaynak Yöneticisi şablonunuzun dağıtıldığı yeri seçtiğiniz yerdir. Müşterilerinizin en çok bulunduğu bölgeyi seçmeniz önerilir.
- **Sıcak** - Zaten dağıtılan ve seçili bölge başına erişim bekleyen Test Sürüşü örneklerinin sayısı. Müşteriler dağıtım beklemek yerine bu Test Sürücülerine anında erişebilir. Bunun amacı, bu örneklerin Azure aboneliğinizde her zaman çalışıyor olmasıdır, bu nedenle daha büyük bir çalışma süresi maliyetine neden olurlar. Müşterilerinizin çoğu tam dağıtımların tamamlanmasını beklemek istemediğinden ve böylece müşteri kullanımında bir düşüş olduğundan, en az bir **Sıcak örneğinin**olması önerilir.
- **Sıcak** - Dağıtılan ve ardından VM durdurulan ve Azure depolama alanında depolanan bölge başına Test Sürüşü örneklerinin sayısı. Sıcak örnekleri için bekleme süresi Sıcak örneklerden daha yavaştır, ancak depolamanın çalışma süresi maliyeti de daha düşüktür.
- **Soğuk** - Dağıtılabilir bölge başına Test Sürüşü örneklerinin sayısı. Soğuk örnekler, bir müşterinin Test Sürücüsü'nü istediği sırada tüm Test Sürücüsü Kaynak Yöneticisi şablonunun dağıtımdan geçmesini gerektirir, bu nedenle Sıcak veya Sıcak örneklerden daha yavaştır. Ancak, denge, yalnızca Test Sürüşü süresi için ödeme yapmak zorunda olmasıdır.

Şu anda, kullanıma sunacağınız potansiyel eşzamanlı Test Sürücülerinin toplam sayısını hesaplar ve aboneliğiniz için kota limitinizin bu eşzamanlı tutarı işleyeceğini doğrular:

**(Seçilen Bölge Sayısı x Sıcak örnekler) + (Seçilen Bölge Sayısı x Sıcak örnekler) + (Seçilen Bölge Sayısı x Soğuk örnekler)**

**Test Sürüşsüresi (saat) -** *Required* Test Sürüşünün saat içinde \# etkin kalacağı süre için Gerekli Süre. Bu süre sona erdikten sonra Test Sürüşü otomatik olarak sona erer.

**Test Drive Kaynak Yöneticisi şablonu -** Gerekli Kaynak Yöneticisi şablonunuzu buraya yüklemeniz *gerekir.* Bu, yukarıdaki önceki bölümde oluşturabileceğiniz dosyadır. Ana şablon dosyasını adlandırın: "main-template.json" ve Kaynak Yöneticisi şablonunuzun gerekli olan anahtar değişkenler için çıkış parametreleri içerdiğinden emin olun. (Bir .zip dosyası olmalıdır)

**Erişim Bilgileri -** *Bir* müşteri Test Sürüşü aldıktan sonra erişim bilgileri onlara sunulur. Bu yönergeler, Test Sürücüsü Kaynak Yöneticisi şablonunuzdaki yararlı çıktı parametrelerini paylaşmak içindir. Çıktı parametrelerini eklemek için çift kıvırcık köşeli ayraç (örneğin, **{{outputname}}** kullanın ve bunlar konuma doğru şekilde eklenir. (HTML dize biçimlendirme ön ucunda işlemek için burada önerilir).

### <a name="test-drive-deployment-subscription-details"></a>Test Sürüşü Dağıtım Abonelik Ayrıntıları

Doldurulması gereken son bölüm, Azure Aboneliğinizi ve Azure Etkin Dizininizi (AD) bağlayarak Test Sürücülerini otomatik olarak dağıtabilmektir.

![Test Drive dağıtım abonelik ayrıntıları](./media/azure-resource-manager-test-drive/subdetails1.png)

**Azure Abonelik Kimliği -** *Gerekli* Bu, Azure hizmetlerine ve Azure portalına erişim izni verir. Abonelik, kaynak kullanımının raporlandığı ve hizmetlerin faturalandırıldığı yerdir. Yalnızca Test Sürücüleri için **ayrı** bir Azure Aboneliğiniz yoksa, devam edin ve bir tane yapın. Azure portalına giriş yaparak ve sol taraftaki menüde Aboneliklere gezinerek Azure Abonelik Kimlikleri'ni bulabilirsiniz. (Örnek: "a83645ac-1234-5ab6-6789-1h234g764ghty")

![Azure Abonelikleri](./media/azure-resource-manager-test-drive/subdetails2.png)

**Azure AD Kiracı Kimliği -** *Gerekli* Kiracı Kimliğiniz zaten mevcutsa,\> aşağıda Özellikler - Dizin Kimliği'nde bulabilirsiniz.

![Azure Etkin Dizin özellikleri](./media/azure-resource-manager-test-drive/subdetails3.png)

Aksi takdirde, Azure Etkin Dizini'nde yeni bir Kiracı oluşturun.

![Azure Etkin Dizin kiracıları listesi](./media/azure-resource-manager-test-drive/subdetails4.png)

![Azure AD kiracısı için kuruluş, etki alanı ve ülke/bölge tanımlayın](./media/azure-resource-manager-test-drive/subdetails5.png)

![Seçimi onaylama](./media/azure-resource-manager-test-drive/subdetails6.png)

**Azure AD Uygulama Kimliği -** *Gerekli* Sonraki adım yeni bir uygulama oluşturmak ve kaydetmektir. Bu uygulamayı Test Sürüşü örneğinde işlemleri gerçekleştirmek için kullanacağız.

1. Yeni oluşturulan dizine veya zaten varolan dizine gidin ve filtre bölmesinde Azure Etkin dizinini seçin.
2. "Uygulama kayıtları"nda arama yapın ve "Ekle"ye tıklayın
3. Bir uygulama adı sağlayın.
4. "Web uygulaması / API" olarak Türünü seçin
5. Oturum Aç URL'sinde herhangi bir\'değer sağlayın, bu alanı kullanmayacağız.
6. Oluştur'u tıklatın.
7. Uygulama oluşturulduktan sonra Özellikler' e\> gidin - Uygulamayı çok kiracı olarak ayarlayın ve Kaydet'e çarptı.

Kaydet’e tıklayın. Son adım, bu kayıtlı uygulamanın Uygulama Kimliğini kapmak ve test sürüşü alanına yapıştırmaktır.

![Azure AD uygulama kimliği detayı](./media/azure-resource-manager-test-drive/subdetails7.png)

Uygulamayı aboneliğe dağıtmak için kullandığımızı göz önüne alırsak, uygulamayı aboneliğe katkıda bulunan olarak eklememiz gerekir. Bunlar için talimatlar aşağıdaki gibidir:

1. Abonelikler bıçağına gidin ve yalnızca Test Sürüşü için kullandığınız uygun aboneliği seçin.
1. **Erişim denetimi (IAM)** öğesine tıklayın.
1. Rol **atamaları** sekmesini tıklatın.  ![Yeni bir Erişim Denetimi ilkesi ekleme](./media/azure-resource-manager-test-drive/SetupSub7_1.jpg)
1. **Rol Atama ekle'yi**tıklatın.
1. **Katılımcı**olarak rolü ayarlayın.
1. Azure AD uygulamasının adını yazın ve rolü atamak için uygulamayı seçin.
    ![İzinleri ekleme](./media/azure-resource-manager-test-drive/SetupSub7_2.jpg)
1. **Kaydet**'e tıklayın.

**Azure AD Uygulama Anahtarı -** *Gerekli* Son alan bir kimlik doğrulama anahtarı oluşturmaktır. Tuşların altında, Bir Anahtar Açıklaması ekleyin, süresi asla sona ermeyecek şekilde ayarlayın ve sonra kaydet'i seçin. Üretimde test sürüşünüzü kıracak, süresi dolmuş bir anahtara sahip olmaktan kaçınmak **önemlidir.** Bu değeri kopyalayın ve gerekli Test Sürüşü alanına yapıştırın.

![Azure AD uygulamasıiçin Anahtarları gösterir](./media/azure-resource-manager-test-drive/subdetails8.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık tüm Test Sürüşü alanlarınızın doldurulduğuna göre, teklifinizi gözden geçirin ve **yeniden yayınlayın.** Test Sürüşünuz sertifikayı geçtikten sonra, teklifinizin **önizlemesinde** müşteri deneyimini kapsamlı bir şekilde test etmelisiniz. UI'de bir Test Sürüşü başlatın ve Azure portalında Azure Aboneliğinizi açın ve Test Sürücülerinizin tam olarak dağıtıldığını doğrulayın.

![Azure portalında](./media/azure-resource-manager-test-drive/subdetails9.png)

Müşterileriniz için sağlanmış olan test sürüşü örneklerini silmediğinizi unutmayın, bu nedenle Test Drive hizmeti bu Kaynak Gruplarını müşteriyle birlikte bitirdikten sonra otomatik olarak temizler.

Bir kez Önizleme sunan rahat hissediyorum, şimdi **canlı gitmek**zamanı! Teklif yayımlandıktan sonra Microsoft'tan son bir gözden geçirme işlemi vardır. Teklif inreddedilirse, teklifiniz için mühendislik ilgiliye nelerin düzeltilmesi gerektiğini açıklayan bir bildirim göndeririz.

Daha fazla sorunuz varsa, sorun giderme önerileri arıyorsanız veya Test Sürüşünüzü daha başarılı hale getirmek istiyorsanız, lütfen [SSS, Sorun Giderme ve En İyi Uygulamalar &](./marketing-and-best-practices.md)gidin.
