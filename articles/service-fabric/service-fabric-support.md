---
title: Azure Hizmet Kumaş ı Destek seçenekleri hakkında bilgi edinin
description: Azure Hizmet Kumaşı küme sürümleri desteklenen ve dosya destek biletlerine bağlantılar
author: pkcsf
ms.topic: troubleshooting
ms.date: 8/24/2018
ms.author: pkc
ms.openlocfilehash: 7494f0072f27f2c9b00db7070f19dfc05627eacf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282100"
---
# <a name="azure-service-fabric-support-options"></a>Azure Hizmet Kumaşı destek seçenekleri

Uygulama çalışma yüklerinizi çalıştırdığınız Service Fabric kümeleriniz için uygun desteği sağlamak için sizin için çeşitli seçenekler belirledik. Gereken destek düzeyine ve sorunun önem derecesine bağlı olarak, doğru seçenekleri seçebilirsiniz. 

## <a name="report-production-issues-or-request-paid-support-for-azure"></a>Azure için üretim sorunlarını bildirme veya ücretli destek isteme

Azure'da dağıtılan Hizmet Dokusu kümenizdeki sorunları bildirmek için [Azure portalında](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) veya Microsoft destek [portalında](https://support.microsoft.com/oas/default.aspx?prid=16146)destek için bir bilet açın.

Aşağıdakiler hakkında daha fazla bilgi edinin:
 
- [Azure için Microsoft'tan destek.](https://azure.microsoft.com/support/plans/?b=16.44)
- [Microsoft premier desteği](https://support.microsoft.com/en-us/premier).

> [!Note]
> Bronz güvenilirlik katmanı veya Tek Düğüm Kümesi üzerinde çalışan kümeler yalnızca test iş yüklerini çalıştırmanızı sağlar. Bronz güvenilirlik veya Tek Düğüm Kümesi üzerinde çalışan bir kümeyle ilgili sorunlarla karşılaşırsanız, Microsoft destek ekibi sorunu azaltmada size yardımcı olur, ancak Root Cause Çözümlemesi gerçekleştirmez. Daha fazla bilgi için lütfen [kümenin güvenilirlik özelliklerine](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-reliability-characteristics-of-the-cluster) bakın.
>
> Üretime hazır küme için gerekenler hakkında daha fazla bilgi için lütfen [üretime hazırlık kontrol listesine](https://docs.microsoft.com/azure/service-fabric/service-fabric-production-readiness-checklist)bakın.

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Tek başına Hizmet Kumaş kümeleri için üretim sorunlarını bildirme veya ücretli destek isteme

Şirket içinde veya diğer bulutlarda dağıtılan Service Fabric kümenizdeki sorunları bildirmek için [Microsoft destek portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)profesyonel destek için bir bilet açın.

Aşağıdakiler hakkında daha fazla bilgi edinin:

- [Şirket içi için Microsoft'tan Profesyonel Destek.](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0)
- [Microsoft premier desteği](https://support.microsoft.com/en-us/premier).

## <a name="report-azure-service-fabric-issues"></a>Azure Hizmet Dokusu sorunlarını bildirin

Service Fabric sorunlarını bildirmek için bir GitHub repo'su hazırladık.  Ayrıca aşağıdaki forumları da aktif olarak izliyoruz.

### <a name="github-repo"></a>GitHub repo 

Hizmet-Kumaş sorunları [git repo](https://github.com/Azure/service-fabric-issues)üzerinde Azure Hizmet Kumaş Sorunları bildirin. Bu repo, Azure Hizmet Kumaşı ile ilgili sorunları raporlamak ve izlemek ve küçük özellik istekleri yapmak için tasarlanmıştır. **Bunu canlı site sorunlarını bildirmek için kullanmayın.**

### <a name="stackoverflow-and-msdn-forums"></a>StackOverflow ve MSDN forumları

[StackOverflow'daki Service Fabric etiketi][stackoverflow] ve [MSDN'deki Service Fabric forumu,][msdn-forum] platformun nasıl çalıştığı ve belirli görevleri nasıl yerine getirebileceğin hakkında sorular sormak için en iyi şekilde kullanılır.

### <a name="azure-feedback-forum"></a>Azure Geri Bildirim forumu

[Hizmet Kumaşı için Azure Geri Bildirim Forumu,][uservoice-forum] en popüler istekleri gözden aldığımız için, orta ve uzun vadeli planlamamızın bir parçasıdır, ürün için sahip olduğunuz büyük özellik fikirlerini göndermek için en iyi yerdir. Toplum içindeki önerileriniz için destek toplamanızı öneririz.

## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Servis Kumaş Önizleme Sürümleri - üretim kullanımı için desteklenmeyen

Zaman zaman, geri bildirim istediğimiz ve önizleme olarak yayımlanan önemli özelliklere sahip sürümler yayınlıyoruz. Bu önizleme sürümleri yalnızca test amacıyla kullanılmalıdır. Üretim kümeniz her zaman desteklenen, kararlı, Service Fabric sürümünü çalıştırmalıdır. Önizleme sürümü her zaman 255'lik büyük ve küçük sürüm numarasıyla başlar. Örneğin, Service Fabric sürümü 255.255.5703.949'u görürseniz, bu sürüm yalnızca test kümelerinde kullanılır ve önizlemededir. Bu önizleme sürümleri de [Service Fabric takım blogunda](https://blogs.msdn.microsoft.com/azureservicefabric) duyurulur ve dahil özellikleri hakkında ayrıntılı bilgi olacaktır.
Bu önizleme sürümleri için ücretli destek seçeneği yoktur. Soru sormak veya geri bildirim sağlamak için [Azure Hizmet Dokusu Nulamı Bildir altında](https://docs.microsoft.com/azure/service-fabric/service-fabric-support#report-azure-service-fabric-issues) listelenen seçeneklerden birini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[Desteklenen Hizmet Kumaşı sürümleri](service-fabric-versions.md)

<!--references-->
[msdn-forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureServiceFabric
[stackoverflow]: https://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: https://aka.ms/servicefabricdocs
[sample-repos]: https://aka.ms/servicefabricsamples
