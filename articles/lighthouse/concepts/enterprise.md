---
title: Kurumsal senaryolarda Azure Lighthouse
description: Azure Deniz Feneri'nin özellikleri, birden çok Azure AD kiracısı kullanan bir kuruluştaki kiracılar arası yönetimi basitleştirmek için kullanılabilir.
ms.date: 09/25/2019
ms.topic: conceptual
ms.openlocfilehash: 91089a6fb1a965191489e87027ef508c7ebe2aa2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75749214"
---
# <a name="azure-lighthouse-in-enterprise-scenarios"></a>Kurumsal senaryolarda Azure Lighthouse

[Azure Deniz Feneri](../overview.md) için en yaygın senaryo, müşterilerinin Azure Etkin Dizin (Azure AD) kiracılarında kaynakları yöneten bir hizmet sağlayıcısıdır. Ancak, Azure Deniz Feneri'nin özellikleri, birden çok Azure AD kiracısı kullanan bir kuruluştaki kiracılar arası yönetimi basitleştirmek için de kullanılabilir.

## <a name="single-vs-multiple-tenants"></a>Tek ve birden çok kiracı

Çoğu kuruluş için yönetim, tek bir Azure AD kiracısıyla daha kolaydır. Tüm kaynakların tek bir kiracı içinde olması, yönetim görevlerinin, bu kiracı içindeki atanmış kullanıcılar, kullanıcı grupları veya hizmet ilkeleri tarafından merkezileştirilmesine olanak tanır. Kuruluşunuz için mümkün olduğunca tek bir kiracı kullanmanızı öneririz.

Aynı zamanda, bir kuruluşun birden çok Azure AD kiracısını korumasını gerektirebilecek durumlar da vardır. Bazı durumlarda, satın almalar gerçekleştiği ve uzun vadeli kiracı konsolidasyon stratejisinin tanımlanması biraz zaman alacağı için bu geçici bir durum olabilir. Bir kuruluşun sürekli olarak birden fazla kiracı tutması da gerekebilir (tamamen bağımsız yan kuruluşlar, coğrafi veya yasal gereksinimler vb. nedeniyle). Çok kiracılı mimarinin gerekli olduğu durumlarda, Azure temsilcikaynak yönetimi, yönetim işlemlerini merkezileştirmek ve kolaylaştırmak için kullanılabilir. Birden çok kiracıdan gelen abonelikler, [Azure'un devraldığı kaynak yönetimi](azure-delegated-resource-management.md)için kullanılabilir ve yönetici kiracıdaki atanmış kullanıcıların çapraz kiracı yönetim [işlevlerini](cross-tenant-management-experience.md) merkezi ve ölçeklenebilir bir şekilde gerçekleştirmesine olanak tanır.

## <a name="tenant-management-architecture"></a>Kiracı yönetim mimarisi

Yönetim işlemlerini birden çok kiracı arasında merkezileştirdiğinizde, hangi kiracının diğer kiracılar için yönetim işlemleri gerçekleştiren kullanıcıları içereceğini belirlemeniz gerekir. Başka bir deyişle, diğer kiracılar için yönetici kiracı olacak hangi kiracı belirlemek gerekir.

Örneğin, kuruluşunuzun *Kiracı A*adını vereceğimiz tek bir kiracısı olduğunu varsayabiliriz. Kuruluşunuz daha sonra iki ek kiracı, *Kiracı B* ve *Kiracı C*satın aldı ve bunları ayrı kiracı olarak korumanızı gerektiren iş nedenleriniz vardır.

Kuruluşunuz tüm kiracılar arasında aynı ilke tanımlarını, yedekleme uygulamalarını ve güvenlik işlemlerini kullanmak istiyor. Kiracı A'da bu görevleri gerçekleştirmekten sorumlu kullanıcılar (kullanıcı grupları ve hizmet ilkeleri dahil) olduğundan, Kiracı A'daki aynı kullanıcıların bunları gerçekleştirebilmeleri için Kiracı B ve Kiracı C içindeki tüm abonelikleri Görev.

![Kiracı A'daki kullanıcılar Kiracı B ve Kiracı C'deki kaynakları yönetiyor](../media/enterprise-azure-lighthouse.jpg)

## <a name="security-and-access-considerations"></a>Güvenlik ve erişim konuları

Çoğu kurumsal senaryoda, bir abonelik içinde yalnızca belirli kaynak gruplarını da devredebilirsiniz, ancak Azure temsilci kaynak yönetimi için tam bir abonelik vermek isteyebilirsiniz.

Her iki şekilde de, [hangi kullanıcıların kaynaklara erişeceğini tanımlarken en az ayrıcalık ilkesine uymayı](recommended-security-practices.md#assign-permissions-to-groups-using-the-principle-of-least-privilege)unutmayın. Bunu yapmak, kullanıcıların yalnızca gerekli görevleri gerçekleştirmek için gereken izinlere sahip olmasını sağlamaya yardımcı olur ve yanlışlıkla hata yapma olasılığını azaltır.

Azure Deniz Feneri ve Azure temsilci kaynak yönetimi, verileri veya kaynakları fiziksel olarak taşımak yerine yalnızca yönetici kiracı ile yönetilen kiracılar arasında mantıksal bağlantılar sağlar. Ayrıca, erişim her zaman yönetici kiracıdan yönetilen kiracılara kadar tek bir yöne gider.  Yönetici kiracıdaki kullanıcılar ve gruplar, yönetilen kiracı kaynakları üzerinde yönetim işlemleri gerçekleştirirken çok faktörlü kimlik doğrulaması kullanmaya devam etmelidir.

Dahili veya harici yönetişim ve uyumluluk korkuluklarına sahip işletmeler, şeffaflık gereksinimlerini karşılamak için [Azure Etkinlik günlüklerini](../../azure-monitor/platform/platform-logs-overview.md) kullanabilir. Kurumsal kiracılar yönetici ve yönetilen kiracı ilişkilerini kurduğunda, her kiracıdaki kullanıcılar günlüğe kaydedilmiş etkinliği görüntüleyerek diğer kiracıdaki kullanıcılar tarafından gerçekleştirilen eylemleri izleyebilir ve görünürlük kazanabilir.

## <a name="onboarding-process-considerations"></a>Onboarding süreci hususlar

Abonelikler (veya abonelik içindeki kaynak grupları), Azure Kaynak Yöneticisi şablonlarını dağıtarak veya Azure Marketi'nde yayınlanan Yönetilen Hizmetler teklifleri aracılığıyla Azure'a devredilen kaynak yönetimine dahil edilebilir Halka.

Kurumsal kullanıcılar normalde işletmenin kiracılarına doğrudan erişim elde edebileceklerinden ve bir yönetim teklifini pazarlamaya veya tanıtmaya gerek olmadığından, genellikle Azure Kaynak Yöneticisi şablonlarıyla doğrudan dağıtmak daha hızlı ve daha kolaydır. Biz [onboarding rehberlik](../how-to/onboard-customer.md)hizmet sağlayıcılar ve müşteriler bakın iken, işletmeler aynı süreçleri kullanabilirsiniz.

İsterseniz, bir kuruluştaki kiracılar, [Yönetilen Hizmetler teklifini Azure Marketi'nde yayımlayarak](../how-to/publish-managed-services-offers.md)gemiye binebilir. Teklifin yalnızca uygun kiracılar tarafından kullanılabildiğinden emin olmak için, planlarınızın özel olarak işaretli olduğundan emin olun. Özel bir planla, gemide yapmayı planladığınız her kiracı için abonelik eklerini sağlayabilirsiniz ve başka hiç kimse teklifinizi alamaz.

## <a name="terminology-notes"></a>Terminoloji notları

Kuruluş içinde çapraz kiracı yönetimi için, Azure Deniz Feneri belgelerindeki hizmet sağlayıcılarına yapılan atıfların, bir kuruluş taki yönetici kiracıya, yani kaynakları yönetecek kullanıcıları içeren kiracıya geçerli olduğu anlaşılabilir Azure temsilci kaynak yönetimi aracılığıyla diğer kiracılarda. Benzer şekilde, müşterilere yapılan başvuruların, yönetici kiracıdaki kullanıcılar aracılığıyla yönetilecek kaynakları devredeleyen kiracılara da geçerli olduğu anlaşılabilir.

Örneğin, yukarıda açıklanan örnekte, Kiracı A servis sağlayıcı kiracı (yönetici kiracı) ve Kiracı B ve Kiracı C müşteri kiracı olarak düşünülebilir.

Bu örnekte, Uygun izinlere sahip Kiracı A kullanıcıları, Azure portalının **müşterilerim** sayfasında [temsilci kaynakları görüntüleyebilir ve yönetebilir.](../how-to/view-manage-customers.md) Benzer şekilde, uygun izinlere sahip Kiracı B ve Kiracı C kullanıcıları, Azure portalının **Hizmet sağlayıcıları** sayfasında Kiracı A'ya [devredilen kaynakları görüntüleyebilir ve yönetebilir.](../how-to/view-manage-service-providers.md)

## <a name="next-steps"></a>Sonraki adımlar

- Kiracılar [arası yönetim deneyimleri](cross-tenant-management-experience.md)hakkında bilgi edinin.
- [Azure tarafından atanan temsilcinin kaynak yönetimi](azure-delegated-resource-management.md) hakkında bilgi edinin.