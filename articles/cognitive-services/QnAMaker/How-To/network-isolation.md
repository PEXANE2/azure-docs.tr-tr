---
title: Ağ yalıtımı
description: Kullanıcılar, Soru-Cevap Oluşturma kaynaklarına genel erişimi kısıtlayabilir.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/09/2020
ms.openlocfilehash: 4e9b45c72e5fb4cbd9e548727faf8946e3e5ba8f
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102125151"
---
# <a name="recommended-settings-for-network-isolation"></a>Ağ yalıtımı için önerilen ayarlar

Soru-Cevap Oluşturma kaynaklarına genel erişimi kısıtlamak için aşağıdaki adımları izleyin. Bilişsel hizmetler kaynağını [, sanal ağı yapılandırarak](../../cognitive-services-virtual-networks.md?tabs=portal)ortak erişimden koruyun.

## <a name="restrict-access-to-cognitive-search-resource"></a>Bilişsel Arama kaynağına erişimi kısıtla

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

Bilişsel Arama sanal ağın içinde özel bir uç nokta olarak yapılandırma. Bir Soru-Cevap Oluşturma kaynağı oluşturma sırasında bir arama örneği oluşturulduğunda, Bilişsel Arama bir müşterinin sanal ağı içinde oluşturulan özel bir uç nokta yapılandırmasını desteklemeye zorlayabilirsiniz.

Özel bir uç nokta kullanmak için tüm kaynakların aynı bölgede oluşturulması gerekir.

* Soru-Cevap Oluşturma kaynağı
* Yeni Bilişsel Arama kaynağı
* Yeni sanal ağ kaynağı

[Azure Portal](https://portal.azure.com)aşağıdaki adımları uygulayın:

1. [Soru-cevap oluşturma kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker)oluşturun.
2. Uç nokta bağlantısı (veri) ile _özel_ olarak ayarlanmış yeni bir bilişsel arama kaynağı oluşturun. Kaynağı, 1. adımda oluşturulan Soru-Cevap Oluşturma kaynağıyla aynı bölgede oluşturun. [Bilişsel arama kaynağı oluşturma](../../../search/search-create-service-portal.md)hakkında daha fazla bilgi edinin ve bu bağlantıyı kullanarak doğrudan [kaynağın oluşturma sayfasına](https://ms.portal.azure.com/#create/Microsoft.Search)gidebilirsiniz.
3. Yeni bir [sanal ağ kaynağı](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM)oluşturun.
4. Bu yordamın 1. adımında oluşturulan App Service kaynağında VNET 'i yapılandırın. 2. adımda oluşturulan yeni Bilişsel Arama kaynağı için VNET 'te yeni bir DNS girişi oluşturun. Bilişsel Arama IP adresine gidin.
5. App Service 'i adım 2 ' de oluşturulan [yeni bilişsel arama kaynağıyla ilişkilendirin](../how-to/set-up-qnamaker-service-azure.md) . Ardından, 1. adımda oluşturulan özgün Bilişsel Arama kaynağını silebilirsiniz.
    
[Soru-cevap oluşturma portalında](https://www.qnamaker.ai/)ilk bilgi tabanınızı oluşturun.

#  <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

Azure Search kaynağında [Özel uç noktalar oluşturun](../reference-private-endpoint.md) .

---

## <a name="restrict-access-to-app-service-qna-runtime"></a>App Service erişimi kısıtla (QnA Runtime)

Erişimi kısıtlamak veya App Service environemnt 'yi soru-cevap oluşturma App Service barındıracak şekilde yapılandırmak için App Service izin öğesine IP ekleyebilirsiniz.

#### <a name="add-ips-to-app-service-allowlist"></a>App Service izin öğesine IP ekleme

1. Yalnızca bilişsel hizmetler IP 'lerinden gelen trafiğe izin verin. Bunlar zaten hizmet etiketine dahil edilmiştir `CognitiveServicesManagement` . Bu, App Service 'i çağırmak ve Azure Search hizmeti 'ni uygun şekilde güncelleştirmek için API 'Leri yazma (oluşturma/güncelleştirme KB) için gereklidir. [Hizmet etiketleri hakkında daha fazla bilgi edinin.](../../../virtual-network/service-tags-overview.md)
2. Azure bot hizmeti, Soru-Cevap Oluşturma portalı vb. gibi diğer giriş noktalarına da izin verdiğinizden emin olun. tahmin için "GenerateAnswer" API erişimi.
3. IP adresi aralıklarını bir allowlist öğesine eklemek için lütfen şu adımları izleyin:

   1. [Tüm hizmet etiketlerinin IP aralıklarını](https://www.microsoft.com/download/details.aspx?id=56519)indirin.
   2. "Biliveservicesmanagement" öğesinin IP 'lerini seçin.
   3. App Service kaynağınızın ağ bölümüne gidin ve IP 'Leri bir allowlist öğesine eklemek için "erişim kısıtlaması Yapılandır" seçeneğini tıklayın.

Ayrıca, App Service için aynı olacak otomatikleştirilmiş bir betiğimiz de vardır. GitHub 'da [bir izin yapılandırmak için PowerShell betiğini](https://github.com/pchoudhari/QnAMakerBackupRestore/blob/master/AddRestrictedIPAzureAppService.ps1) bulabilirsiniz. Abonelik kimliği, kaynak grubu ve gerçek App Service adını betik parametreleri olarak girbilmeniz gerekir. Betiği çalıştırmak, IP 'Leri App Service allowlist dosyasına otomatik olarak ekler.

#### <a name="configure-app-service-environment-to-host-qna-maker-app-service"></a>App Service Ortamı Soru-Cevap Oluşturma barındırmak için yapılandırın App Service
    
App Service Ortamı (Ao), Soru-Cevap Oluşturma App Service 'i barındırmak için kullanılabilir. Lütfen aşağıdaki adımları izleyin:

1. Bir App Service Ortamı oluşturun ve "dış" olarak işaretleyin. Yönergeler için lütfen [öğreticiyi](../../../app-service/environment/create-external-ase.md) izleyin.
2.  App Service Ortamı içinde bir App Service oluşturun.
    1. App Service 'in yapılandırmasını denetleyin ve bir uygulama ayarı olarak ' BID Yendpointkey ' ekleyin. ' BID Yendpointkey ' değeri "-BID \<app-name\> yendpointkey" olarak ayarlanmalıdır. Uygulama adı App Service URL 'sinde tanımlanmıştır. Örneğin, uygulama hizmeti URL 'SI "mywebsite.myase.p.azurewebsite.net" ise, uygulama adı "mywebsite" olur. Bu durumda, ' bıı Yendpointkey ' değeri "mywebsite-bıı Yendpointkey" olarak ayarlanmalıdır.
    2. Azure Search hizmeti oluşturun.
    3. Azure Search ve uygulama ayarlarının uygun şekilde yapılandırıldığından emin olun. 
          Lütfen bu [öğreticiyi](../reference-app-service.md?tabs=v1#app-service)izleyin.
3.  App Service Ortamı ilişkili ağ güvenlik grubunu güncelleştirin
    1. Gereksinimlerinize göre önceden oluşturulmuş gelen güvenlik kurallarını güncelleştirin.
    2. ' Hizmet etiketi ' ve kaynak hizmet etiketi ' Biliveservicesmanagement ' olarak kaynağa sahip yeni bir gelen güvenlik kuralı ekleyin.
4.  Azure Resource Manager kullanarak Soru-Cevap Oluşturma bilişsel hizmet örneği (Microsoft. Biliveservices/hesaplar) oluşturun; burada Soru-Cevap Oluşturma uç noktası yukarıda oluşturulan App Service uç noktaya ayarlanır (https://mywebsite.myase.p.azurewebsite.net).
    
---
