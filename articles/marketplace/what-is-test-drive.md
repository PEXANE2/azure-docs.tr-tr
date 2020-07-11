---
title: Test sürüşü nedir? Microsoft ticari Market
description: Market test sürücüsü özelliğinin açıklaması
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: keferna
ms.author: keferna
ms.date: 06/19/2020
ms.openlocfilehash: abad72145b095b4da77ec499f936c6912fd970a6
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86229457"
---
# <a name="what-is-a-test-drive"></a>Test sürüşü nedir?

Test sürüşü, *satın almadan önce deneme*seçeneğini sunarak olası müşterilere teklifinizi göstermek için harika bir yoldur. böylece, yüksek oranda nitelikli müşteri adaylarının dönüştürülmesine ve oluşturulmasına neden olur. Bir sınama sürücüsü, ürününüzü gerçek dünyada bir uygulama senaryosunda hayata geçirin ve ayrıca yüksek düzeyde nitelikli müşteri adayları da sunar.

Test sürücüleri, çözümünüzü veya uygulamanızı isteyen müşteriler için talep üzerine dağıtan yönetilen örneklerdir. Bir test sürücü örneği atandıktan sonra, bir süre için kullanılabilir ve daha sonra başka bir müşterinin odasını oluşturmak için silinir.

Yayımcı olarak, Iş Ortağı Merkezi 'nde test sürücüsü ayarlarını yönetip yapılandırırsınız. Teknik yapılandırma ayrıntıları, çalıştığınız teklifin türüne bağlı olarak değişiklik gösterir. Ayrıntılı yönergeler için bu konunun sonundaki [sonraki adım](#next-step) altındaki bağlantıya bakın.

Potansiyel müşteriler, ticari Market 'teki test sürücünüzü keşfeder. Bunlar, iletişim bilgilerini sağlar ve teklifinizin hüküm ve gizlilik ilkesini kabul edebilir ve ardından sabit bir süre boyunca denemek için önceden yapılandırılmış ortamınıza erişin. Müşteriler, ürününüzün temel özelliklerine ve avantajlarına yönelik uygulamalı, kendinden destekli bir deneme alır ve değerli bir müşteri adayı elde edersiniz.

## <a name="how-does-it-work"></a>Nasıl çalışır?

Yayımcı olarak, Iş Ortağı Merkezi 'nde test sürücüsü ayarlarını yönetip yapılandırırsınız. Kurulumdan sonra, isteği isteyen müşteri için isteğe bağlı dağıtılacak yönetilen bir örnek haline gelir. Bir test sürücü örneği atandıktan sonra, bir süre için kullanılabilir ve daha sonra başka bir müşterinin odasını oluşturmak için silinir.

## <a name="types-of-test-drives"></a>Test sürücü türleri

Sunmakta olduğunuz ürün, senaryo ve Market türüne bağlı olarak, select teklifleri için ticari Market 'te bulunan farklı test sürücüleri mevcuttur:

- Azure Resource Manager
- Barındırılan test sürücüsü
    - Dynamics 365 for Business Central
    - Müşteri Etkileşimi için Dynamics 365
    - Dynamics 365 for Operations
- Mantıksal uygulama
- Power BI

Bu test sürücülerinden birini yapılandırma hakkında ayrıntılı bilgi için bu konunun sonundaki [sonraki adımda](#next-step) yer alan bağlantıya bakın.

### <a name="azure-resource-manager-test-drive"></a>Test sürüşü Azure Resource Manager

Bu dağıtım şablonu, çözümünüzü oluşturan tüm Azure kaynaklarını içerir. Bu senaryoya uyan ürünler yalnızca Azure kaynaklarını kullanır. Bu, sanal makine veya Azure Uygulama teklifleri için tek sınama sürücüsü seçeneğidir.

### <a name="hosted-test-drive"></a>Barındırılan test sürücüsü

Barındırılan bir test sürücüsü, Microsoft ana bilgisayarına izin vererek ve Kullanıcı hazırlama, dağıtma ve devre dışı bırakma sınama sürücüsünü gerçekleştiren hizmeti sürdürerek kurulum karmaşıklığını ortadan kaldırır. Microsoft AppSource bir teklifiniz varsa, bir Dynamics AX/CRM örneğine veya yalnızca Azure dışındaki diğer kaynaklara bağlanmak için test sürücünüzü oluşturun. Bu Dynamics 365 tekliflerle bağlantı kurmak için AppSource teklifleri için bu türü kullanın:

- Finans, işlemler, tedarik zinciri ve CRM gibi Iş Merkezi kurumsal kaynak planlama sistemi için [Dynamics 365 for Business Central](partner-center-portal/create-new-operations-offer.md) 'ı kullanın.
- Sales, Service, Project Service ve Field hizmeti gibi bir müşteri katılımı sistemi için [Dynamics 365](partner-center-portal/create-new-customer-engagement-offer.md) ' i kullanın.
- Finans, işlemler ve üretim, tedarik zinciri gibi finans ve Işlemler kurumsal kaynak planlama sistemine yönelik [işlemler Için Dynamics 365](partner-center-portal/create-new-operations-offer.md) kullanın.

### <a name="logic-app-test-drive"></a>Mantıksal uygulama sınama sürücüsü

Bu tür bir test sürücüsü Microsoft tarafından barındırılmamaktadır. Dynamics 365 teklifiyle veya diğer özel kaynakla bağlanmak için bunu kullanın.

### <a name="power-bi-test-drive"></a>Test sürüşü Power BI

Bu yalnızca özel olarak oluşturulmuş bir panoya eklenmiş bir bağlantıdır. Yalnızca etkileşimli bir Power BI görseli gösteren tüm ürünler bu test sürücüsü türünü kullanmalıdır.

## <a name="transforming-examples"></a>Örnekleri dönüştürme

Bir kaynak mimarisini bir test sürücüsüne çevirme işlemi, korunal olabilir. Geçerli mimarilerin en iyi şekilde dönüştürülmesiyle bu örneklere göz atın.

[Web sitesi şablonunu test sürücüsüne dönüştürme](https://github.com/Azure/AzureTestDrive/wiki/Transforming-Website-Deployment-Template-for-Test-Drive)

[Bir sanal makine şablonunu test sürücüsüne dönüştürme](https://github.com/Azure/AzureTestDrive/wiki/Transforming-Virtual-Machine-Deployment-Template-for-Test-Drive)

[Varolan bir Kaynak Yöneticisi şablonunu test sürücüsüne Dönüştür](https://github.com/Azure/AzureTestDrive/wiki/Deploying-Existing-Solutions)

## <a name="generate-leads-from-your-test-drive"></a>Test sürücünüzden müşteri adayları oluşturma

Ticari Market test sürüşü, pazarlamalara yönelik harika bir araçtır. İşletmeniz için daha fazla müşteri adayı oluşturmaya başladığınızda, bunu pazara sunma çabalarınıza dahil etmenizi öneririz. Ayrıntılı kılavuz için bkz. [ticari Market teklifinizin müşteri adayları](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/marketplace/partner-center-portal/commercial-marketplace-get-customer-leads.md).

Bir test sürücüsü lideri ile bir anlaşmayı kapatırsanız, [Microsoft Iş ortağı Sales Connect](https://support.microsoft.com/help/3155788/getting-started-with-microsoft-partner-sales-connect)'e kaydettiğinizden emin olun. Ayrıca, bir test sürücüsünün bir rol oynadığı müşterilerin WINS 'niz hakkında bilgi almak için sevdiğimiz bir duyuyoruz.

## <a name="other-resources"></a>Diğer kaynaklar

Ek test sürücüsü kaynakları:

- [En iyi teknik uygulamalar](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Genel bakış](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (PDF; açılır pencere engelleyicinizin kapalı olduğundan emin olun)

## <a name="next-step"></a>Sonraki adım

- [Test sürüşü teknik yapılandırması](test-drive-technical-configuration.md)
