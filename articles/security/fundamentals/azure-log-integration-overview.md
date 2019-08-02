---
title: Günlükleri Azure kaynaklarından SıEM sistemlerinizle tümleştirin | Microsoft Docs
description: Azure günlük tümleştirmesi, temel özellikleri ve nasıl çalıştığı hakkında bilgi edinin.
services: security
documentationcenter: na
author: TomShinder
manager: barbkess
editor: TerryLanfear
ms.assetid: 9c1346e1-baf8-4975-b2f2-42ae05b2dc0a
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/28/2019
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 9ba4a64268bcc57f04e92be83edb2ac71f18bcaf
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727604"
---
# <a name="introduction-to-azure-log-integration"></a>Azure günlük tümleştirmesine giriş

>[!IMPORTANT]
> Azure günlük tümleştirme özelliği 06/15/2019 tarafından kullanım dışı bırakılacak. AzLog İndirmeleri 27 Haz 2018 ' de devre dışı bırakıldı. İleriye doğru iletme hakkında yönergeler için gönderiyi [SıEM araçlarıyla tümleştirme Için Azure Izleyicisini kullanma](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

Azure günlük tümleştirmesi, Azure günlüklerini şirket içi güvenlik bilgileriniz ve olay yönetimi (SıEM) sisteminizle tümleştirme görevini basitleştirmek için kullanılabilir hale getirilir.

 Azure günlüklerini tümleştirmek için önerilen yöntem, SıEM satıcınızın bağlayıcılarınızı kullanmaktır. Azure Izleyici, günlükleri Olay Hub 'larına akışa alma özelliği sağlar ve SıEM satıcıları, olayları Olay Hub 'ından SıEM 'ye daha fazla bütünleştirmek için bağlayıcılar yazabilir.  Bunun nasıl çalıştığına ilişkin bir açıklama için, [veri olay hub 'ları için akış izlemeyi](/azure/azure-monitor/platform/stream-monitoring-data-event-hubs)izleme bölümündeki yönergeleri izleyin. Bu makalede, doğrudan Azure bağlayıcılarının zaten kullanılabildiği Sıems de listelenir.  

> [!IMPORTANT]
> Birincil İlginiz sanal makine günlüklerini topluyorsa, en SıEM satıcıları çözümünde bu seçeneği içerir. SıEM satıcısının bağlayıcısının kullanılması her zaman tercih edilen alternatiftir.

Azure günlük tümleştirme özelliğindeki belgeler, özellik kullanım dışı bırakılıncaya kadar hala tutulmaktadır.

Azure günlük tümleştirme özelliği hakkında daha fazla bilgi edinmek için daha fazla bilgi edinin:

Azure günlük tümleştirmesi, Windows Olay Görüntüleyicisi günlüklerinden, [Azure etkinlik günlüklerinden](/azure/azure-monitor/platform/activity-logs-overview), [Azure Güvenlik Merkezi uyarılarından](/azure/security-center/security-center-intro)ve Azure kaynaklarından [Azure tanılama günlüklerinden](/azure/azure-monitor/platform/diagnostic-logs-overview) Windows olaylarını toplar. Tümleştirme, SıEM çözümünüzün şirket içinde veya bulutta olsun, tüm varlıklarınızda birleştirilmiş bir pano sağlamasına yardımcı olur. Güvenlik olaylarına yönelik uyarıları almak, toplamak, ilişkilendirmek ve analiz etmek için bir pano kullanabilirsiniz.

> [!NOTE]
> Şu anda Azure günlük tümleştirmesi yalnızca Azure ticari ve Azure Kamu bulutlarını destekler. Diğer bulutlar desteklenmez.

![Azure günlük tümleştirme işlemi](media/azure-log-integration-overview/azure-log-integration.png)

## <a name="what-logs-can-i-integrate"></a>Hangi günlükleri tümleştirebilirim?

Azure, her Azure hizmeti için kapsamlı günlük oluşturur. Günlükler üç günlük türünü temsil eder:

* **Denetim/Yönetim günlükleri**: [Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview) oluşturma, GÜNCELLEŞTIRME ve silme işlemlerine görünürlük sağlar. Azure etkinlik günlüğü, bu tür bir günlüğe ait bir örnektir.
* **Veri düzlemi günlükleri**: Bir Azure kaynağı kullandığınızda oluşan olaylara görünürlük sağlar. Bu tür bir günlüğe bir örnek Windows Olay Görüntüleyicisi **sistem**, **güvenlik**ve Windows sanal makinesindeki **uygulama** kanallarıdır. Azure Izleyici aracılığıyla yapılandırdığınız Azure Tanılama günlüğe kaydetme işlemi başka bir örnektir.
* **İşlenen olaylar**: Sizin için işlenen çözümlenmiş olay ve uyarı bilgilerini sağlayın. Bu tür bir olaya örnek olarak Azure Güvenlik Merkezi uyarıları vardır. Azure Güvenlik Merkezi, geçerli güvenlik duruşınızla ilgili uyarılar sağlamak için aboneliğinizi işler ve analiz eder.

Azure günlük tümleştirmesi, Arctıma, QRadar ve splunk 'yi destekler. Satıcının yerel bağlayıcıya sahip olup olmadığını değerlendirmek için SıEM satıcınızla görüşün. Yerel bağlayıcı kullanılabiliyorsa Azure günlük tümleştirmesi kullanmayın.

Başka seçenek yoksa, Azure günlük tümleştirmesi kullanmayı göz önünde bulundurun. Aşağıdaki tabloda önerilerinizi verilmiştir:

|SIEM | Müşteri zaten Azure günlük tümleştiricisi kullanıyor | Müşteri, SıEM tümleştirme seçeneklerini araştırmaktadır|
|---------|--------------------------|-------------------------------------------|
|**Splunk** | [Splunk Için Azure izleyici eklentisini](https://splunkbase.splunk.com/app/3534/)geçirmeye başlayın. | [Splunk bağlayıcısını](https://splunkbase.splunk.com/app/3534/)kullanın. |
|**QRadar** | [Azure izleme verilerinin bir dış araç tarafından tüketimine yönelik bir olay hub 'ına akışını](/azure/azure-monitor/platform/stream-monitoring-data-event-hubs)sağlayan QRadar bağlayıcısını kullanın veya kullanmaya başlayın. | [Azure izleme verilerinin bir dış araç tarafından tüketimine yönelik bir olay hub 'ına akışını](/azure/azure-monitor/platform/stream-monitoring-data-event-hubs)sağlayan QRadar bağlayıcısını kullanın. |
|**Arcgörüş** | Bir bağlayıcı kullanılabilir olana kadar Azure günlük tümleştirici kullanmaya devam edin ve bağlayıcı tabanlı çözüme geçirin.  | Azure Izleyici günlüklerini alternatif olarak kullanmayı göz önünde bulundurun. Bağlayıcı kullanılabilir hale geldiğinde geçiş işlemini yapmak istemediğiniz sürece Azure günlük tümleştirmenize eklemeyin. |

> [!NOTE]
> Azure günlük tümleştirmesi ücretsiz bir çözüm olsa da, günlük dosyası bilgi depolama ile ilişkili Azure depolama maliyetleri vardır.

Yardıma ihtiyacınız varsa, bir [destek isteği](/azure/azure-supportability/how-to-create-azure-support-request)oluşturabilirsiniz. Hizmet için **günlük tümleştirmesi**' ni seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makale sizi Azure günlük tümleştirmesi ' ni tanıtmaktadır. Azure günlük tümleştirmesi ve desteklenen günlük türleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure günlük tümleştirmesi ile çalışmaya](azure-log-integration-get-started.md)başlayın. Bu öğretici, Azure günlük tümleştirmesi yüklemesinde size kılavuzluk eder. Ayrıca, Windows Azure Tanılama (WAD) depolama, Azure etkinlik günlükleri, Azure Güvenlik Merkezi uyarıları ve Azure Active Directory Denetim günlükleriyle günlüklerin nasıl tümleştirileceğini açıklar.
* [Azure günlük tümleştirmesi hakkında sık sorulan sorular (SSS)](azure-log-integration-faq.md). Bu SSS, Azure günlük tümleştirmesi hakkında sık sorulan soruları yanıtlar.
* [Azure izleme verilerinin bir dış araç tarafından tüketimine yönelik bir olay hub 'ına nasıl akışının](/azure/azure-monitor/platform/stream-monitoring-data-event-hubs)yapılacağı hakkında daha fazla bilgi edinin.

