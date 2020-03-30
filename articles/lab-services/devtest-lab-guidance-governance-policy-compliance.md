---
title: Azure DevTest Labs'da şirket ilkesi ve uyumluluğu
description: Bu makalede, Azure DevTest Labs altyapısı için şirket ilkesi ve uyumluluk yönetimi hakkında kılavuz verilmektedir.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 81ca2a90b1940d70e170cab3f8d18144a5d5e5a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74560515"
---
# <a name="governance-of-azure-devtest-labs-infrastructure---company-policy-and-compliance"></a>Azure DevTest Labs altyapısının yönetimi - Şirket ilkesi ve uyumluluğu
Bu makalede, Azure DevTest Labs altyapısı için şirket ilkesi ve uyumluluk yönetimi hakkında kılavuz verilmektedir. 

## <a name="public-vs-private-artifact-repository"></a>Genel ve özel eser deposu

### <a name="question"></a>Soru
Bir kuruluş DevTest Labs'da ortak yapı deposu ve özel eser deposu ne zaman kullanmalıdır?

### <a name="answer"></a>Yanıt
[Ortak yapı deposu,](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) en sık kullanılan bir ilk yazılım paketi kümesi sağlar. Ortak geliştirici araçlarını ve eklentilerini yeniden oluşturmak için zaman yatırım yapmak zorunda kalmadan hızlı dağıtıma yardımcı olur. Kendi özel depolarını dağıtmayı seçebilirsiniz. Ortak ve özel bir depoyı paralel olarak kullanabilirsiniz. Ayrıca, genel depoyu devre dışı bırakmayı da seçebilirsiniz. Özel bir depo dağıtmak için ölçütler aşağıdaki sorular ve hususlar tarafından yönlendirilmelidir:

- Kuruluşun DevTest Labs teklifinin bir parçası olarak kurumsal lisanslı yazılıma sahip olması gerekliliği var mı? Yanıt evet ise, özel bir depo oluşturulmalıdır.
- Kuruluş, genel sağlama sürecinin bir parçası olarak gerekli olan belirli bir işlem sağlayan özel yazılım lar geliştiriyor mu? Yanıt evet ise, özel bir depo dağıtılmalıdır.
- Kuruluşun yönetim ilkesi yalıtım gerektiriyorsa ve dış depolar kuruluş tarafından doğrudan yapılandırma yönetimi altında değilse, özel bir yapı deposu dağıtılmalıdır. Bu işlemin bir parçası olarak, kamu deposunun ilk kopyası kopyalanabilir ve özel depoile tümleştirilebilir. Daha sonra, kuruluş içindeki hiç kimsenin artık bu depoya erişemesi için ortak depo devre dışı tutulabilir. Bu yaklaşım, kuruluş içindeki tüm kullanıcıları kuruluş tarafından onaylanan tek bir depoya sahip olmaya ve yapılandırma sürüklenmesini en aza indirmeye zorlar.

### <a name="single-repository-or-multiple-repositories"></a>Tek depo veya birden fazla depo 

### <a name="question"></a>Soru
Bir kuruluş tek bir depo için plan yapmalı veya birden çok depoya izin vermeli mi?

### <a name="answer"></a>Yanıt
Kuruluşunuzun genel yönetim ve yapılandırma yönetimi stratejisinin bir parçası olarak, merkezi bir depo kullanmanızı öneririz. Birden çok depo kullandığınızda, bunlar zaman içinde yönetilmeyen yazılım siloları haline gelebilir. Merkezi bir depo ile, birden çok takım projeleri için bu depodaki yapıları tüketebilir. Standardizasyon, güvenlik, yönetim kolaylığı ve çabaların çoğaltılmasını ortadan kaldırır. Merkezileştirmenin bir parçası olarak, aşağıdaki eylemler uzun vadeli yönetim ve sürdürülebilirlik için tavsiye edilen uygulamalardır:

- Azure Repos'u, Azure aboneliğinin kimlik doğrulama ve yetkilendirme için kullandığı Aynı Azure Etkin Dizin kiracısıyla ilişkilendirin.
- Azure Etkin Dizin'de merkezi olarak yönetilen **Tüm DevTest Labs Geliştiricileri** adında bir grup oluşturun. Yapı geliştirmeye katkıda bulunan herhangi bir geliştirici bu gruba yerleştirilmelidir.
- Aynı Azure Etkin Dizin grubu, Azure Deposu deposuna ve laboratuvara erişim sağlamak için kullanılabilir.
- Azure Deposu'nda, dallanma veya forama, birincil üretim deposundan ayrı bir geliştirme deposu için kullanılmalıdır. İçerik yalnızca uygun bir kod incelemesinden sonra çekme isteğiyle ana dala eklenir. Kod gözden geçiren değişikliği onayladıktan sonra, ana dalı bakımından sorumlu bir müşteri adayı geliştirici, güncelleştirilmiş kodu birleştirir. 

## <a name="corporate-security-policies"></a>Kurumsal güvenlik politikaları

### <a name="question"></a>Soru
Bir kuruluş kurumsal güvenlik ilkelerinin yerinde olmasını nasıl sağlayabilir?

### <a name="answer"></a>Yanıt
Bir kuruluş bunu aşağıdaki eylemleri yaparak elde edebilir:

1. Kapsamlı bir güvenlik politikası geliştirmek ve yayınlamak. İlke, yazılım, bulut varlıkları nın kullanımıyla ilişkili kabul edilebilir kullanım kurallarını ifade eder. Ayrıca, politikayı açıkça ihlal eden şeyi tanımlar. 
2. Etkin dizinle tanımlanan güvenlik alanı içinde düzenlemeye olanak tanıyan özel bir görüntü, özel yapılar ve dağıtım işlemi geliştirin. Bu yaklaşım, kurumsal sınırı zorlar ve ortak bir çevresel denetim kümesi belirler. Bir geliştirici, genel süreçlerinin bir parçası olarak güvenli bir geliştirme yaşam döngüsü geliştirir ken ve takip ederken, bir geliştiricinin düşünebileceği ortama karşı bu denetimler. Amaç aynı zamanda, gelişimi engelleyebilecek aşırı kısıtlayıcı olmayan, ancak makul bir denetim kümesi sağlayan bir ortam sağlamaktır. Laboratuvar sanal makinelerini içeren kuruluş birimindeki (OU) grup ilkeleri, üretimde bulunan toplam grup ilkelerinin bir alt kümesi olabilir. Alternatif olarak, tanımlanan riskleri düzgün bir şekilde azaltmak için ek bir küme olabilir.

## <a name="data-integrity"></a>Veri bütünlüğü

### <a name="question"></a>Soru
Bir kuruluş, remoting geliştiricilerinin kodu kaldıramamasını veya kötü amaçlı yazılım veya onaylanmamış yazılım ları tanıtamamasını sağlamak için veri bütünlüğünü nasıl sağlayabilir?

### <a name="answer"></a>Yanıt
DevTest Labs'da işbirliği yapmak için yeniden motive olan harici danışmanlar, yükleniciler veya çalışanların tehdidini azaltmak için çeşitli denetim katmanları vardır. 

Daha önce de belirtildiği gibi, ilk adım, birisi politikayı ihlal ettiğinde sonuçlarını açıkça özetleyen kabul edilebilir bir kullanım politikası hazırlamış ve tanımlanmalıdır. 

Uzaktan erişim için ilk denetim katmanı, doğrudan laboratuvara bağlı olmayan bir VPN bağlantısı üzerinden uzaktan erişim ilkesi uygulamaktır. 

Denetimlerin ikinci katmanı, uzak masaüstü üzerinden kopyalanıp yapıştırmayı engelleyen bir grup ilkesi nesnesi kümesi uygulamaktır. FTP ve RDP hizmetleri gibi çevreden giden hizmetlerin çevredışına çıkmasına izin vermemek için bir ağ ilkesi uygulanabilir. Kullanıcı tanımlı yönlendirme tüm Azure ağ trafiğini şirket başına geri dönmeye zorlayabilir, ancak yönlendirme, içeriği ve oturumları taramaya yönelik bir proxy aracılığıyla denetlenmedikçe verilerin yüklenmesine izin verebilecek tüm URL'leri hesaba katamadı. Genel IP'ler, DevTest Labs'ı destekleyen sanal ağ içinde harici bir ağ kaynağının köprülemesine izin vermeyecek şekilde kısıtlanabilir.

Sonuç olarak, aynı tür kısıtlamaların kuruluş genelinde uygulanması gerekir, bu da bir içerik gönderisini kabul edebilecek çıkarılabilir ortam veya harici URL'lerin olası tüm yöntemlerini de hesaba katmak zorunda kalacaktır. Bir güvenlik ilkesini gözden geçirmek ve uygulamak için güvenlik uzmanınızla danışın. Daha fazla öneri için [Microsoft Siber Güvenlik'e](https://www.microsoft.com/security/default.aspx?&WT.srch=1&wt.mc_id=AID623240_SEM_sNYnsZDs)bakın.


## <a name="next-steps"></a>Sonraki adımlar
Bkz. [Uygulama geçişi ve tümleştirme.](devtest-lab-guidance-governance-application-migration-integration.md)
