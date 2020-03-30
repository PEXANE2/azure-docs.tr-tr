---
title: Microsoft Azure Bulut Hizmetleri SSS| Microsoft Dokümanlar
description: Bu makalede, Microsoft Azure Bulut Hizmetleri dağıtımı yla ilgili sık sorulan sorular listelanmaktadır.
services: cloud-services
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 63a219078927e9001a8eb4085c722e7ec8d2fac9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75980639"
---
# <a name="deployment-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Azure Bulut Hizmetleri için dağıtım sorunları: Sık sorulan sorular (SSS)

Bu makalede, Microsoft Azure Bulut [Hizmetleri](https://azure.microsoft.com/services/cloud-services)için dağıtım sorunları hakkında sık sorulan sorular yer almaktadır. Boyut bilgileri için [Bulut Hizmetleri VM Boyut sayfasına](cloud-services-sizes-specs.md) da başvurabilirsiniz.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-does-deploying-a-cloud-service-to-the-staging-slot-sometimes-fail-with-a-resource-allocation-error-if-there-is-already-an-existing-deployment-in-the-production-slot"></a>Üretim yuvasında zaten varolan bir dağıtım varsa, bir bulut hizmetini hazırlama yuvasına dağıtmak bazen kaynak ayırma hatasıyla başarısız oluyor?
Bir bulut hizmetinin her iki yuvada da dağıtımı varsa, tüm bulut hizmeti belirli bir kümeye sabitlenir. Bu, üretim yuvasında zaten bir dağıtım varsa, yeni bir hazırlama dağıtımının yalnızca üretim yuvasıyla aynı kümede ayrılabileceği anlamına gelir.

Bulut hizmetinizin bulunduğu küme, dağıtım isteğinizi karşılamak için yeterli fiziksel bilgi işlem kaynağına sahip olmadığında ayırma hataları oluşur.

Bu tür ayırma hatalarını azaltma konusunda yardım için Bulut [Hizmeti ayırma hatası: Çözümler'](cloud-services-allocation-failures.md#solutions)e bakın.

## <a name="why-does-scaling-up-or-scaling-out-a-cloud-service-deployment-sometimes-result-in-allocation-failure"></a>Bulut hizmeti dağıtımını ölçekleme veya ölçekleme neden bazen ayırma hatasına neden oluyor?
Bir bulut hizmeti dağıtıldığında, genellikle belirli bir kümeye sabitlenir. Bu, varolan bir bulut hizmetini ölçeklemenin/çıkarmanın aynı kümede yeni örnekler ayırması gerektiği anlamına gelir. Küme kapasiteye yaklaşıyorsa veya istenen VM boyutu/türü kullanılamıyorsa, istek başarısız olabilir.

Bu tür ayırma hatalarını azaltma konusunda yardım için Bulut [Hizmeti ayırma hatası: Çözümler'](cloud-services-allocation-failures.md#solutions)e bakın.

## <a name="why-does-deploying-a-cloud-service-into-an-affinity-group-sometimes-result-in-allocation-failure"></a>Bulut hizmetini bir yakınlık grubuna dağıtmak neden bazen ayırma hatasına neden olur?
Boş bir bulut hizmetine yeni bir dağıtım, bulut hizmeti bir yakınlık grubuna sabitlenmedikçe, o bölgedeki herhangi bir kümedeki kumaş tarafından tahsis edilebilir. Aynı yakınlık grubuna dağıtımlar aynı kümede denenir. Küme kapasiteye yaklaşıyorsa, istek başarısız olabilir.

Bu tür ayırma hatalarını azaltma konusunda yardım için Bulut [Hizmeti ayırma hatası: Çözümler'](cloud-services-allocation-failures.md#solutions)e bakın.

## <a name="why-does-changing-vm-size-or-adding-a-new-vm-to-an-existing-cloud-service-sometimes-result-in-allocation-failure"></a>VM boyutunu niçin değiştirmek veya varolan bir bulut hizmetine yeni bir VM eklemek bazen tahsis atamasına neden oluyor?
Veri merkezindeki kümeler makine türlerinin farklı yapılandırmalarına sahip olabilir (örneğin, A serisi, Av2 serisi, D serisi, Dv2 serisi, G serisi, H serisi, vb.). Ancak tüm kümeler mutlaka VM'lerin her türlü olurdu. Örneğin, yalnızca A serisinde zaten dağıtılan bir bulut hizmetine D serisi VM eklemeye çalışırsanız, bir ayırma hatası yla karşılaşırsınız. VM SKU boyutlarını değiştirmeye çalışırsanız (örneğin, A serisinden D serisine geçiş) bu durum da gerçekleşir.

Bu tür ayırma hatalarını azaltma konusunda yardım için Bulut [Hizmeti ayırma hatası: Çözümler'](cloud-services-allocation-failures.md#solutions)e bakın.

Bölgenizde kullanılabilen boyutları denetlemek için [Bkz. Microsoft Azure: Bölgeye göre kullanılabilen ürünler.](https://azure.microsoft.com/regions/services)

## <a name="why-does-deploying-a-cloud-service-sometime-fail-due-to-limitsquotasconstraints-on-my-subscription-or-service"></a>Aboneliğimdeki veya hizmetimdeki sınırlar/kotalar/kısıtlamalar nedeniyle bir bulut hizmetini dağıtmak neden bazen başarısız oluyor?
Bir bulut hizmetinin dağıtımı, tahsis edilmesi gereken kaynakların bölge/veri merkezi düzeyinde hizmetiniz için izin verilen varsayılan veya maksimum kotayı aşması durumunda başarısız olabilir. Daha fazla bilgi için [Bulut Hizmetleri sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-cloud-services-limits)bakın.

Ayrıca, aboneliğiniz için geçerli kullanımı/kotayı portalda da izleyebilirsiniz: \<Azure portalı => Abonelikler = uygun abonelik>> => "Kullanım + kota".

Kaynak kullanımı/tüketimiyle ilgili bilgiler Azure Faturalandırma API'leri aracılığıyla da alınabilir. Bkz. [Azure Kaynak Kullanımı API 'si (Önizleme)](../cost-management-billing/manage/usage-rate-card-overview.md#azure-resource-usage-api-preview).

## <a name="how-can-i-change-the-size-of-a-deployed-cloud-service-vm-without-redeploying-it"></a>Yeniden dağıtmadan dağıtılan bir bulut hizmeti VM'nin boyutunu nasıl değiştirebilirim?
Dağıtılan bir bulut hizmetinin VM boyutunu yeniden dağıtmadan değiştiremezsiniz. VM boyutu CSDEF'te yerleşiktir ve bu da yalnızca yeniden dağıtılımla güncellenebilir.

Daha fazla bilgi için [bulut hizmetini nasıl güncelleştirin.](cloud-services-update-azure-service.md)

## <a name="why-am-i-not-able-to-deploy-cloud-services-through-service-management-apis-or-powershell-when-using-azure-resource-manager-storage-account"></a>Azure Kaynak Yöneticisi Depolama hesabını kullanırken neden Hizmet Yönetimi API'leri veya PowerShell aracılığıyla Bulut Hizmetlerini dağıtamıyorum? 

Bulut Hizmeti, Azure Kaynak Yöneticisi modeliyle doğrudan uyumlu olmayan klasik bir kaynak olduğundan, bu kaynağı Azure Kaynak Yöneticisi Depolama hesaplarıyla ilişkilendiremezsiniz. Birkaç seçenek şunlardır: 

- REST API üzerinden dağıtım.

    Service Management REST API aracılığıyla dağıtırken, hem Klasik hem de Azure Kaynak Yöneticisi Depolama hesabıyla çalışacak blob depolamasına bir SAS URL'si belirterek sınırlamayı aşabilirsiniz. [Burada](/previous-versions/azure/reference/ee460813(v=azure.100))'PackageUrl' özelliği hakkında daha fazla bilgi edinin.

- [Azure portalı](https://portal.azure.com)üzerinden dağıtım.

    Bu, arama Azure Kaynak Yöneticisi ve Klasik kaynaklar arasında iletişim sağlayan bir proxy/şim'den geçerken [Azure portalında](https://portal.azure.com) çalışır. 

## <a name="why-does-azure-portal-require-me-to-provide-a-storage-account-for-deployment"></a>Azure portalı neden dağıtım için bir depolama hesabı sağlamamı gerektiriyor?

Klasik portalda, paket doğrudan yönetim API katmanına yüklendi ve ardından API katmanı paketi geçici olarak bir dahili depolama hesabına koydu.  API katmanı bir dosya yükleme hizmeti olarak tasarlanmadığından, bu işlem performans ve ölçeklenebilirlik sorunlarına neden olur.  Azure portalında (Kaynak Yöneticisi dağıtım modeli), API katmanına ilk yüklemenin geçici adımını atlayarak daha hızlı ve daha güvenilir dağıtımlar elde ettik.

Maliyete gelince, çok küçüktür ve tüm dağıtımlarda aynı depolama hesabını yeniden kullanabilirsiniz. Hizmet paketini (CSPKG) yükleme maliyetini belirlemek, CSPKG'yi indirmek ve ardından CSPKG'yi silmek için [depolama maliyeti hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/#storage1) kullanabilirsiniz.
