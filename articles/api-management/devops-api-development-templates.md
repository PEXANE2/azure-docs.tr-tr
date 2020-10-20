---
title: ARM şablonları kullanarak Azure API Management DevOps
description: Bir CI/CD işlem hattındaki API dağıtımlarını yönetmek için Azure Resource Manager şablonları kullanarak Azure API Management ile API DevOps 'a giriş
services: api-management
author: miaojiang
ms.service: api-management
ms.topic: conceptual
ms.date: 10/09/2020
ms.author: apimpm
ms.openlocfilehash: 144baa0944451adaf08f0df84325b58079e19d65
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92210422"
---
# <a name="cicd-for-api-management-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonları kullanarak API Management için CI/CD

Bu makalede, Azure Resource Manager şablonları kullanarak Azure API Management ile API DevOps tanıtılmıştır. API 'lerin stratejik değeri sayesinde bir sürekli tümleştirme (CI) ve sürekli dağıtım (CD) işlem hattı, API geliştirmenin önemli bir yönüdür. Kuruluşların, hataya açık el ile adımlar olmadan API değişikliklerinin dağıtımını otomatik hale getirme, daha önce sorunları algılaması ve son kullanıcılara daha hızlı değer sunma olanağı sağlar. 

Bu makalede açıklanan DevOps yaklaşımını uygulamak için Ayrıntılar, Araçlar ve kod örnekleri için GitHub 'daki açık kaynaklı [Azure API Management DevOps kaynak seti](https://github.com/Azure/azure-api-management-devops-resource-kit) ' ne bakın. Müşteriler çok çeşitli mühendislik kültürleri ve var olan otomasyon çözümlerini getirtiğinden, yaklaşım tek boyutlu bir-hepsi çözümü değildir.

## <a name="the-problem"></a>Sorun

Günümüzde kuruluşlar, genellikle birden fazla dağıtım ortamına (geliştirme, test, üretim gibi) sahiptir ve her ortam için ayrı API Management örnekleri kullanır. Bazı örnekler, farklı sürüm 'Lere sahip farklı API 'lerden sorumlu olan birden çok geliştirme ekipleri tarafından paylaşılır.

Sonuç olarak, müşteriler aşağıdaki güçlüklere sahiptir:

* API 'lerin API Management ' a dağıtımını otomatikleştirme
* Yapılandırma bir ortamdan diğerine nasıl geçirilir?
* Aynı API Management örneğini paylaşan farklı geliştirme takımları arasında bir girişimden kaçının mi?

## <a name="manage-configurations-in-resource-manager-templates"></a>Kaynak Yöneticisi şablonlarındaki konfigürasyonları yönetme

Önerilen yaklaşım aşağıdaki görüntüde gösterilmiştir. 

:::image type="content" source="media/devops-api-development-templates/apim-devops.png" alt-text="API Management ile DevOps":::

Bu örnekte, iki dağıtım ortamı vardır: *geliştirme* ve *Üretim*. Her birinin kendi API Management örneği vardır. 

* API geliştiricilerin geliştirme örneğine erişimi vardır ve API 'Lerini geliştirmek ve test etmek için kullanabilir. 
* Üretim örneği, *API yayımcıları*adında belirlenmiş bir takım tarafından yönetilir.

Bu önerilen yaklaşımdaki anahtar, tüm API Management yapılandırmalarının [Azure Resource Manager şablonlarda](../azure-resource-manager/resource-group-authoring-templates.md)tutulmasını kullanmaktır. Bu şablonlar git gibi bir kaynak denetimi sisteminde tutulmalıdır. Görüntüde gösterildiği gibi, bir şablon koleksiyonundaki üretim API Management örneğinin tüm yapılandırmasını içeren bir yayımcı deposu vardır:

|Şablon  |Açıklama  |
|---------|---------|
|Hizmet şablonu     | API Management örneğinin fiyatlandırma katmanı ve özel etki alanları gibi hizmet düzeyi yapılandırması         |
|Paylaşılan Şablonlar     |  Gruplar, ürünler ve Günlükçüler gibi API Management bir örnek genelinde paylaşılan kaynaklar    |
|API şablonları     |  API 'lerin ve alt kaynaklarının yapılandırması: işlemler, ilkeler, Tanılama ayarları        |
|Ana (ana) şablon     |   Tüm şablonlara bağlanarak ve bunları sırayla dağıtarak her şeyi birbirine [bağlama](../azure-resource-manager/resource-group-linked-templates.md) . Tüm konfigürasyonları bir API Management örneğine dağıtmak için, ana şablonu dağıtın. Her şablon tek tek de dağıtılabilir.       |

API geliştiricileri, yayımcı deposunu bir geliştirici deposuna çatalla ve API 'lerinde değişiklikler üzerinde çalışacaktır. Çoğu durumda, API 'Ler için API şablonlarına odaklanırlar ve paylaşılan veya hizmet şablonlarını değiştirmenize gerek kalmaz.

## <a name="migrate-configurations-to-templates"></a>Yapılandırma şablonlarını şablonlara geçirme
Kaynak Yöneticisi şablonlarıyla çalışırken API geliştiricileri için sorunlar vardır:

* API geliştiricileri genellikle [Openapı belirtimiyle](https://github.com/OAI/OpenAPI-Specification) çalışır ve Kaynak Yöneticisi şemaları hakkında bilgi sahibi olmayabilir. Yazma şablonları el ile hata olabilir. 

   Resource Kit 'te [**Oluşturucu**](https://github.com/Azure/azure-api-management-devops-resource-kit/blob/master/src/APIM_ARMTemplate/README.md#Creator) adlı bir yardımcı program aracı, açık bir API belirtim dosyasını temel alan API şablonlarının oluşturulmasını otomatikleştirmenize yardımcı olabilir. Ayrıca, geliştiriciler bir API için XML biçiminde API Management ilkeleri sağlayabilir. 

* Zaten API Management kullanan müşteriler için, mevcut yapılandırmaların Kaynak Yöneticisi şablonlarına ayıklanmasına yönelik başka bir zorluk vardır. Bu müşteriler için Resource Kit 'te [**Extractor**](https://github.com/Azure/azure-api-management-devops-resource-kit/blob/master/src/APIM_ARMTemplate/README.md#extractor) adlı bir araç, API Management örneklerinden yapılandırmaların ayıklanarak şablon oluşturmaya yardımcı olabilir.  

## <a name="workflow"></a>İş akışı

* API geliştiricileri bir API geliştirmeyi ve test etmeyi tamamladıktan ve API şablonlarını ürettikten sonra, değişiklikleri yayımcı deposunda birleştirmek için bir çekme isteği gönderebilirler. 

* API yayımcıları çekme isteğini doğrulayabilir ve değişikliklerin güvenli ve uyumlu olmasını sağlayabilir. Örneğin, API ile yalnızca HTTPS iletişim kurmasına izin verilip verilmeyeceğini kontrol edebilirler. Çoğu doğrulama, CI/CD işlem hattındaki bir adım olarak otomatikleştirilebilir.

* Değişiklikler onaylandıktan ve başarıyla birleştirildikten sonra, API yayımcıları bunları zamanlamaya göre ya da isteğe bağlı olarak üretim örneğine dağıtmayı seçebilirler. Şablonların dağıtımı, [GitHub eylemleri](https://github.com/Azure/apimanagement-devops-samples), [Azure Pipelines](/devops/pipelines/), [Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md), [Azure CLI](../azure-resource-manager/templates/deploy-cli.md)veya diğer araçları kullanarak otomatikleştirilebilir.

Bu yaklaşımla, API değişikliklerinin API Management örneklerine dağıtılması otomatikleştirilebilir ve değişiklikleri bir ortamdan diğerine yükseltmek kolaydır. Farklı API geliştirme takımları farklı API şablonu ve dosya kümelerinde çalışacağından, farklı takımlar arasında girişim yapılmasını önler.

## <a name="video"></a>Video

> [!VIDEO https://www.youtube.com/embed/4Sp2Qvmg6j8]

## <a name="next-steps"></a>Sonraki adımlar

- Ek bilgi, araç ve örnek şablonlar için bkz. açık kaynaklı [Azure API Management DevOps kaynak seti](https://github.com/Azure/azure-api-management-devops-resource-kit) .