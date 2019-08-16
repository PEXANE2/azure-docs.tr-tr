---
title: Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme | Microsoft Docs
description: Bu belge, güvenlik uyarılarını yönetmek ve yanıtlamak için Azure Güvenlik Merkezi işlevlerini kullanmanıza yardımcı olur.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/22/2018
ms.author: v-mohabe
ms.openlocfilehash: 39849514d772f128434daad590de22f941245af7
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69516102"
---
# <a name="managing-and-responding-to-security-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama
Bu belge, güvenlik uyarılarını yönetmeniz ve yanıtlamanız için Azure Güvenlik Merkezi’ni kullanmanıza yardımcı olur.

> [!NOTE]
> Gelişmiş algılamaları etkinleştirmek için Azure Güvenlik Merkezi Standart sürümüne yükseltme yapın. Ücretsiz deneme sürümü mevcuttur. Yükseltmek için [Güvenlik İlkesi](tutorial-security-policy.md)’nde Fiyatlandırma Katmanı’nı seçin. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi fiyatlandırması](security-center-pricing.md).
>
>

## <a name="what-are-security-alerts"></a>Güvenlik uyarıları nedir?
Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için Azure kaynaklarınızdan, ağınızdan ve güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir. Öncelikli güvenlik uyarıları listesi, sorunu hızlıca araştırmanız gereken bilgiler ve saldırıyı düzeltme hakkındaki önerilerle birlikte Güvenlik Merkezi'nde gösterilir.


> [!NOTE]
> Güvenlik Merkezi algılama özelliklerinin nasıl çalıştığı hakkında daha fazla bilgi için [Azure Güvenlik Merkezi Algılama Özellikleri](security-center-detection-capabilities.md) konusunu okuyun.
>
>

## <a name="managing-security-alerts"></a>Güvenlik ilkelerini yönetme
**Güvenlik uyarıları** kutucuğuna bakarak mevcut uyarılarınızı gözden geçirebilirsiniz. Uyarılar hakkında daha fazla ayrıntıya ulaşmak için şu adımları izleyin:

1. Güvenlik Merkezi panosunda **Güvenlik uyarıları** kutucuğunu görürsünüz.

    ![Güvenlik Merkezi'nde güvenlik uyarıları kutucuğu](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-ga.png)

2. Uyarılar hakkında daha fazla ayrıntı içeren **Güvenlik uyarıları** sayfasını açmak için kutucuğa tıklayın.

   ![Güvenlik Merkezi'nde Güvenlik uyarıları](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-ga.png)

Bu sayfanın alt bölümünde her bir uyarı için ayrıntılar bulunur. Sıralamak için hangi sütuna göre sıralamak istediğinizi belirtin. Her sütunun tanımı aşağıda verilmiştir:

* **Açıklama**: Uyarının kısa bir açıklaması.
* **Sayı**: Belirli bir gün üzerinde algılanan bu belirli türdeki tüm uyarıların listesi.
* **Tespit eden**: Uyarıyı tetiklemeden sorumlu olan hizmet.
* **Tarih**: Olayın oluştuğu tarih.
* **Durum**: Bu uyarının geçerli durumu. İki tür durum mevcuttur:
  * **Etkin**: Güvenlik Uyarısı algılandı.
  * **Çıkarıldı**: Güvenlik uyarısı kullanıcı tarafından kapatıldı. Bu durum genellikle araştırılan ve hafiftılan ya da gerçek bir saldırı olmayan uyarılar için kullanılır.
* **Önem derecesi**: Yüksek, orta veya düşük olabilen önem derecesi düzeyi.

> [!NOTE]
> Güvenlik Merkezi tarafından oluşturulan güvenlik uyarıları, Azure Etkinlik Günlüğü altında ayrıca görünür. Azure Etkinlik Günlüğü’ne erişim hakkında daha fazla bilgi için bkz. [Kaynaklara uygulanan eylemleri denetlemek için etkinlik günlüklerini görüntüle](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
>


### <a name="alert-severity"></a>Uyarı önem derecesi

-   **Yüksek**: Kaynağınızın güvenliğinin tehlikeye girdiği büyük bir olasılık vardır. Hemen bir yere bakmanız gerekir. Güvenlik Merkezi, hem kötü amaçlı amaç hem de uyarı vermek için kullanılan bulgularda yüksek güvenilirliğe sahiptir. Örneğin, Mimikatz gibi bilinen kötü amaçlı bir aracın yürütülmesini algılayan bir uyarı, kimlik bilgilerinin çalınması için kullanılan ortak bir araçtır. 
-   **Orta**: Bu, bir kaynağın güvenliğinin aşıldığını gösterebilen şüpheli bir etkinlikten kaynaklanıyor olabilir.
Güvenlik Merkezi 'nin analitik veya bulma 'daki güvenilirliği orta ve kötü amaçlı amaç 'nin güvenilirliği orta ile yüksektir. Bunlar genellikle makine öğrenimi veya anomali tabanlı algılamalar olur. Örneğin, anormal bir konumdan oturum açma girişimi.
-   **Düşük**: Bu bir zararsız veya engellenmiş saldırı olabilir. 
    - Güvenlik Merkezi, amacın kötü amaçlı olduğu ve etkinliğin masum olabileceğinden emin olmak için yeterli değildir. Örneğin, günlük Temizleme, bir saldırgan parçaları gizlemeyi denediğinde gerçekleşemeyen bir eylemdir, ancak çoğu durumda Yöneticiler tarafından gerçekleştirilen bir rutin işlemdir.
    - Güvenlik Merkezi, baktığımız ilginç bir durum olmadığı takdirde saldırı ne zaman engellendiğine ilişkin bilgi vermez. 
-   **Bilgi amaçlı**: Yalnızca bir güvenlik olayının detayına veya belirli bir uyarı KIMLIĞIYLE REST API kullandığınızda bilgilendirme uyarıları görürsünüz. Genellikle bir olay bir dizi uyarıdan oluşur, bazıları yalnızca bilgilendirici olarak görünebilir, ancak diğer uyarıların bağlamında daha yakından bir görünüme sahip olabilir.  

> [!NOTE]
> **2015-06-01-Preview** API sürümünü kullanıyorsanız, yukarıda listelenenden itibaren alarm önem derecesi türlerinin hangi senaryolara uygulanabileceğini gösteren farklılıklar vardır.  

### <a name="filtering-alerts"></a>Uyarıları filtreleme
Tarihe, duruma ve önem derecesine göre uyarıları filtreleyebilirsiniz. Filtreleme uyarıları, güvenlik uyarıları gösterimi kapsamını daraltmanızın gerektiği senaryolar için faydalı olabilir. Örneğin, sistemde olası bir ihlali araştırdığınız için son 24 saatte oluşan güvenlik uyarılarını ele almak isteyebilirsiniz.

1. **Güvenlik Uyarıları**'ndaki **Filtreleme**'ye tıklayın. **Filtreleme** açılır ve görmek istediğiniz tarih, durum ve önem derecesi değerlerini seçersiniz.

    ![Güvenlik Merkezi'nde uyarıları filtreleme](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-2017.png)

### <a name="respond-to-security-alerts"></a>Güvenlik uyarılarını yanıtlama
Uyarıyı tetikleyen olay(lar) ve saldırıyı düzeltmek için (varsa) hangi adımları atmanız gerektiği hakkında daha fazla bilgi edinmek için bir güvenlik uyarısı seçin. Güvenlik uyarıları, türe ve tarihe göre gruplandırılır. Güvenlik uyarısına tıklandığında gruplanan uyarıların listesini içeren sayfa açılır.

![Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

Bu durumda, tetiklenen uyarılar şüpheli Uzak Masaüstü Protokolü (RDP) etkinliğine işaret eder. Birinci sütunda hangi kaynakların saldırıya uğradığı; ikinci sütunda kaynağın kaç kez saldırıya uğradığı; üçüncü sütunda saldırının zamanı; dördüncü sütunda uyarının durumu ve beşinci sütunda saldırının önem derecesi gösterilir. Bu bilgileri gözden geçirdikten sonra, saldırıya uğrayan kaynağa tıklayın.

![Azure Güvenlik Merkezi'nde güvenlik uyarıları hakkında ne yapılacağına ilişkin öneriler](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

**Açıklama** alanında, bu olayla ilgili daha fazla ayrıntı bulabilirsiniz. Bu ek ayrıntılar, güvenlik uyarısını neyin tetiklediği, hedef kaynak, uygulanabilirse kaynak IP adresi ve düzeltmeye ilişkin öneriler hakkında öngörüler sunar.  Windows güvenlik olayı günlüklerinin tümü IP adresi içermediğinden, bazı durumlarda kaynak IP adresi boştur (yoktur).

Güvenlik Merkezi tarafından önerilen düzeltme, güvenlik uyarısına göre farklılık gösterir. Bazı durumlarda, önerilen düzeltmeyi uygulamak için diğer Azure işlevlerini kullanmak zorunda kalabilirsiniz. Örneğin, bu saldırının düzeltilmesi, [ağ ACL 'si](../virtual-network/virtual-networks-acl.md) veya [ağ güvenlik grubu](../virtual-network/security-overview.md#security-rules) kuralı kullanarak bu saldırıyı oluşturan IP adresine izin vermemelidir. Farklı Uyarı türleri hakkında daha fazla bilgi için [güvenlik uyarıları türlerini](security-center-alerts-overview.md#security-alert-types)okuyun.

> [!NOTE]
> Güvenlik Merkezi Linux makinelerdeki kötü amaçlı davranışlarını algılamak için denetim kayıtlarını kullanan yeni bir algılama kümesi ve ortak denetim çerçevesi içeren sınırlı bir önizleme sürümüyle yayımlanmıştır. Önizlemeye katılmak için lütfen abonelik kimliklerinizi [bize](mailto:ASC_linuxdetections@microsoft.com) e-posta ile gönderin.


## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde Güvenlik Olayını İşleme](security-center-incident.md)
* [Azure Güvenlik Merkezi Algılama Özellikleri](security-center-detection-capabilities.md)
* [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md)
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.
