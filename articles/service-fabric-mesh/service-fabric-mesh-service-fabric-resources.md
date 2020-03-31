---
title: Azure Hizmet Kumaş Kaynak Modeline Giriş
description: Service Fabric Mesh uygulamalarını tanımlamak için basitleştirilmiş bir yaklaşım olan Service Fabric Resource Model hakkında bilgi edinin.
author: vturecek
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: vturecek
ms.custom: mvc, devcenter
ms.openlocfilehash: 0ae2ed163560aee4c0c3525ab31910e37afaa5b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259142"
---
# <a name="introduction-to-service-fabric-resource-model"></a>Hizmet kumaş kaynak modeline giriş

Service Fabric Resource Model, Service Fabric Mesh uygulamasını oluşturan kaynakları tanımlamak için basit bir yaklaşım açıklar. Tek tek kaynaklar herhangi bir Hizmet Kumaşı ortamına dağıtılabilir.  Hizmet Kumaş Kaynak Modeli de Azure Kaynak Yöneticisi Modeli ile uyumludur. Aşağıdaki kaynak türleri şu anda bu modelde desteklenir:

- Uygulamalar ve hizmetler
- Ağlar
- Ağ geçitleri
- Sırlar ve Sırlar/Değerler
- Birimler

Her kaynak, Mesh Uygulamasını açıklayan basit bir YAML veya JSON belgesi olan ve Service Fabric platformu tarafından sağlanan bir kaynak dosyasında bildirimsel olarak açıklanır.

## <a name="applications-and-services"></a>Uygulamalar ve Hizmetler

Uygulama kaynağı, Mesh uygulamasının dağıtım, sürüm ve kullanım ömrü birimidir. Bir mikro hizmeti temsil eden bir veya daha fazla Hizmet kaynaklarından oluşur. Her Hizmet kaynağı, kod paketiyle ilişkili kapsayıcı görüntüsünü çalıştırmak için gereken her şeyi açıklayan bir veya daha fazla kod paketinden oluşur.

![Uygulamalar ve hizmetler][Image1]

Bir Hizmet kaynağı aşağıdakileri bildirir:

- Kapsayıcı adı, sürümü ve kayıt defteri
- Her kapsayıcı için gerekli CPU ve bellek kaynakları
- Ağ uç noktaları
- Ağlar, birimler ve sırlar gibi diğer kaynaklara başvurular 

Hizmet kaynağının parçası olarak tanımlanan tüm kod paketleri dağıtılır ve grup olarak birlikte etkinleştirilir. Hizmet kaynağı ayrıca, hizmetin kaç örneğinin çalıştırılabildiğini ve bağlı olduğu diğer Kaynaklara (örneğin, Ağ kaynağı) başvurur.

Mesh Uygulaması birden fazla Hizmetten oluşuyorsa, aynı düğümüzerinde birlikte çalışması garanti edilmez. Ayrıca, Uygulamanın yükseltilmesi sırasında, tek bir Hizmeti yükseltmenin başarısız olması, tüm Hizmetlerin önceki sürümüne geri dönmesine neden olur.

Daha önce de belirtildiği gibi, her Uygulama örneğinin yaşam döngüsü bağımsız olarak yönetilebilir. Örneğin, bir Uygulama örneği diğer Uygulama örneklerinden bağımsız olarak yükseltilebilir. Genellikle, bir uygulamada hizmet sayısını oldukça küçük tutarsanız, bir uygulamaya ne kadar çok hizmet koyarsanız, her hizmeti bağımsız olarak yönetmek o kadar zorlaşır.

## <a name="networks"></a>Ağlar

Ağ kaynağı, bağımlılıkları olarak adlandırılabilecek bir Uygulama veya Hizmet kaynağından bağımsız olarak tek tek dağıtılabilir kaynaktır. Uygulamalarınız için bir ağ oluşturmak için kullanılır. Farklı Uygulamalardan Gelen Birden Çok Hizmet aynı ağın parçası olabilir.  Daha fazla bilgi [için, Hizmet Kumaş Örgü uygulamalarında ağ](service-fabric-mesh-networks-and-gateways.md)hakkında okuyun.

> [!NOTE]
> Geçerli önizleme yalnızca uygulamalar ve ağlar arasında bire bir eşleme destekler

![Ağ ve ağ geçidi][Image2]

## <a name="gateways"></a>Ağ geçitleri
Ağ Geçidi kaynağı iki ağı bağlar ve trafiği yönlendirir.  Bir ağ geçidi, hizmetlerinizin harici istemcilerle iletişim kurmasını sağlar ve hizmet(ler) hizmetinize giriş sağlar.  Ağ geçidi, Mesh uygulamanızı kendi varolan sanal ağınıza bağlamak için de kullanılabilir. Daha fazla bilgi [için, Hizmet Kumaş Örgü uygulamalarında ağ](service-fabric-mesh-networks-and-gateways.md)hakkında okuyun.

![Ağ ve ağ geçidi][Image2]

## <a name="secrets"></a>Gizli Diziler

Sırlar kaynakları, bağımlılıkları olarak adlandırılabilecek bir uygulama veya hizmet kaynağından bağımsız olarak dağıtılabilir. Uygulamalarınıza Sırlar'ı güvenli bir şekilde sunmak için kullanılır. Farklı uygulamalardan gelen birden çok hizmet, aynı Gizli'nin değerlerine başvurulabilir.

## <a name="volumes"></a>Birimler

Kapsayıcılar genellikle geçici diskleri kullanılabilir hale getirir. Geçici diskler geçicidir, ancak, yeni bir geçici disk alırsınız ve bir kapsayıcı çöktürüldüğünde bilgileri kaybedersiniz. Geçici diskler hakkındaki bilgileri diğer kapsayıcılarla paylaşmak da zordur. Birimler, durumu sürdürmek için kullanabileceğiniz kapsayıcı örneklerinin içine monte edilmiş dizinlerdir. Birimler size genel amaçlı dosya depolama sağlar ve normal disk G/Ç dosya API'lerini kullanarak dosyaları okuma/yazmanızı sağlar. Birim kaynağı, bir dizinin nasıl monte edilip destek deposuolduğunu (Azure Dosyaları Birimi veya Hizmet Kumaşı Güvenilir Ses Düzeyi) açıklamak için bildirimsel bir yoldur.  Daha fazla bilgi için, [depolama durumunu](service-fabric-mesh-storing-state.md#volumes)okuyun.

![Birimler][Image3]

## <a name="programming-models"></a>Programlama modelleri
Hizmet kaynağı yalnızca kaynakla ilişkili kod paketinde başvurulan bir kapsayıcı görüntüsü gerektirir. Service Fabric Mesh'e özgü API'leri bilmeye veya kullanmaya gerek kalmadan kapsayıcının içindeki herhangi bir çerçeveyi kullanarak herhangi bir dilde yazılmış herhangi bir kodu çalıştırabilirsiniz. 

Uygulama kodunuz Service Fabric Mesh dışında bile taşınabilir kalır ve uygulama dağıtımlarınız, hizmetlerinizi uygulamak için kullanılan dil veya çerçeveden bağımsız olarak tutarlı kalır. Uygulamanız ister Core, Go, ister yalnızca bir dizi işlem ve komut dosyası ASP.NET olsun, Service Fabric Mesh Kaynak dağıtım modeli aynı kalır. 

## <a name="packaging-and-deployment"></a>Paketleme ve dağıtım

Kaynak modeline dayalı Servis Kumaş Örgü uygulamaları Docker konteynerolarak paketlenir.  Service Fabric Mesh paylaşılan, çok kiracılı bir ortamdır ve konteynerler size yüksek düzeyde izolasyon sağlar.  Bu uygulamalar JSON biçimi veya YAML biçimi kullanılarak tanımlanır (daha sonra JSON dönüştürülür). Bir Mesh uygulamasını Azure Hizmet Kumaş Kafesi'ne dağıtırken, uygulamayı açıklamak için kullanılan JSON bir Azure Kaynak Yöneticisi şablonudur. Kaynaklar Azure kaynaklarına eşlenir.  Bir Hizmet Dokusu kümesine (bağımsız veya Azure barındırılan) Bir Kafes uygulaması dağıtırken, uygulamayı açıklamak için kullanılan JSON, Azure Kaynak Yöneticisi şablonuna benzer bir biçimdir.  Mesh uygulamaları dağıtıldıktan sonra HTTP arabirimleri veya Azure CLI üzerinden yönetilebilir. 


## <a name="next-steps"></a>Sonraki adımlar 
Service Fabric Mesh hakkında daha fazla bilgi edinmek için genel bakışı okuyun:
- [Hizmet Kumaş Örgü genel bakış](service-fabric-mesh-overview.md)

[Image1]: media/service-fabric-mesh-service-fabric-resources/AppsAndServices.png
[Image2]: media/service-fabric-mesh-service-fabric-resources/NetworkAndGateway.png
[Image3]: media/service-fabric-mesh-service-fabric-resources/volumes.png
