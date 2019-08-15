---
title: Azure Güvenlik Merkezi ve Azure SQL veritabanı hizmeti | Microsoft Docs
description: Bu makalede Güvenlik Merkezi 'nin, Azure SQL veritabanı 'nda veritabanlarınızı güvenli hale getirmenize nasıl yardımcı olduğu gösterilmektedir.
services: sql-database
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f109adfd-daed-4257-9692-2042a1399480
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: rkarlin
ms.openlocfilehash: 0a889de79b6a5921007614dac8d610c1be0222d2
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "60704620"
---
# <a name="azure-security-center-and-azure-sql-database-service"></a>Azure Güvenlik Merkezi ve Azure SQL veritabanı hizmeti
[Azure Güvenlik Merkezi](https://azure.microsoft.com/documentation/services/security-center/), tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Aboneliklerinizde, tümleşik güvenlik izleme ve ilke yönetimi sağlar; normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

Bu makalede Güvenlik Merkezi 'nin, Azure SQL veritabanı 'nda veritabanlarınızı güvenli hale getirmenize nasıl yardımcı olduğu gösterilmektedir.

## <a name="why-use-security-center"></a>Güvenlik Merkezi neden kullanılır?
Güvenlik Merkezi, tüm sunucularınızın ve veritabanlarınızın güvenliğine ilişkin görünürlük sağlayarak SQL veritabanındaki verileri korumanıza yardımcı olur. Güvenlik Merkezi ile şunları yapabilirsiniz:

* SQL veritabanı şifreleme ve denetleme ilkelerini tanımlayın.
* Tüm aboneliklerinizde SQL veritabanı kaynaklarının güvenliğini izleyin.
* Güvenlik sorunlarını hızlıca belirleyip düzeltin.
* Uyarıları [Azure SQL veritabanı tehdit](../sql-database/sql-database-threat-detection.md)algılamalarından tümleştirin.

Güvenlik Merkezi, SQL veritabanı kaynaklarınızı korumaya yardımcı olmaya ek olarak Azure sanal makineleri, Cloud Services, App Services, sanal ağlar ve daha fazlası için güvenlik izleme ve yönetimi de sağlar. [Burada](security-center-intro.md)Güvenlik Merkezi hakkında daha fazla bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Güvenlik Merkezi 'nin ücretsiz katmanı, aboneliğiniz ile etkinleştirilir. Güvenlik Merkezi 'nin ücretsiz ve standart katmanları hakkında daha fazla bilgi için bkz. [Güvenlik Merkezi fiyatlandırması](https://azure.microsoft.com/pricing/details/security-center/).

Güvenlik Merkezi rol tabanlı erişimi destekler. Azure 'da rol tabanlı erişim denetimi (RBAC) hakkında daha fazla bilgi edinmek için bkz. [rol tabanlı Access Control Azure Active Directory](../role-based-access-control/role-assignments-portal.md). Güvenlik Merkezi SSS, [Izinlerin Güvenlik Merkezi 'nde nasıl işlendiği](security-center-faq.md#permissions)hakkında bilgi sağlar.

## <a name="access-security-center"></a>Güvenlik Merkezi'ne erişme
Güvenlik Merkezi'ne [Azure portalından](https://azure.microsoft.com/features/azure-portal/) erişebilirsiniz. [Portalda oturum açın](https://portal.azure.com/) ve **Güvenlik Merkezi seçeneğini**belirleyin.

![Güvenlik Merkezi seçeneği][1]

**Güvenlik Merkezi** dikey penceresi açılır.
![Güvenlik Merkezi dikey penceresi][2]

## <a name="set-security-policy"></a>Güvenlik ilkesi ayarlama
Bir güvenlik ilkesi, belirtilen abonelik veya kaynak grubundaki kaynaklar için önerilen denetim kümesini tanımlar. Güvenlik Merkezi 'nde, aboneliğinizin veya kaynak gruplarınızın ilkelerini, şirketinizin güvenlik ihtiyaçlarına ve uygulama türüne ve her abonelikteki verilerin duyarlılığına göre tanımlarsınız.

SQL denetimi ve SQL saydam veri şifrelemesi (TDE) için öneriler göstermek üzere bir ilke ayarlayabilirsiniz.

* **SQL denetimi ve tehdit algılamayı**açtığınızda, Güvenlik Merkezi, uyumluluk, gelişmiş algılama ve Araştırma amaçları Için Azure veritabanı erişimi denetiminin etkinleştirilmesini önerir.
* **SQL saydam veri şifrelemesini**açtığınızda, Güvenlik Merkezi, Azure SQL veritabanınız, ilişkili yedeklemeler ve işlem günlük dosyaları için bekleyen şifrelemenin etkinleştirilmesini önerir.

Bir güvenlik ilkesi ayarlamak için, güvenlik merkezi dikey penceresinde **ilke** kutucuğunu seçin. **Güvenlik ilkesi** dikey penceresinde, güvenlik ilkesini etkinleştirmek istediğiniz aboneliği seçin. **Önleme ilkesini** seçin ve bu abonelikte kullanmak istediğiniz güvenlik önerilerini açın.
![Güvenlik ilkesi][3]

Daha fazla bilgi için bkz. [güvenlik Ilkelerini ayarlama](tutorial-security-policy.md).

## <a name="manage-security-recommendation"></a>Güvenlik önerisini yönetme
Güvenlik Merkezi düzenli aralıklarla Azure kaynaklarınızın güvenlik durumunu çözümler. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde öneriler oluşturur. Gerekli denetimlerin yapılandırılması işlemi boyunca öneriler size rehberlik eder.

Bir güvenlik ilkesi ayarladıktan sonra, güvenlik merkezi olası güvenlik açıklarını belirlemek için kaynaklarınızın güvenlik durumunu analiz eder. Öneriler, her satırın belirli bir öneriyi temsil ettiği bir tablo biçiminde görüntülenir. Azure SQL veritabanı 'nın kullanılabilir önerilerini anlamanıza yardımcı olmak için bir başvuru olarak aşağıdaki tabloyu kullanın. Öneri seçilirse, güvenlik merkezi 'nde öneriyi nasıl uygulayacağınızı açıklayan bir makaleye gidersiniz.

| Öneri | Açıklama |
| --- | --- |
| [SQL sunucularında denetim ve tehdit algılamayı etkinleştirme](security-center-enable-auditing-on-sql-servers.md) |SQL veritabanı sunucuları için denetim ve tehdit algılamayı etkinleştirmenizi önerir. (Yalnızca SQL veritabanı hizmeti. , Sanal makinelerinizde çalışan Microsoft SQL Server içermez.) |
| [SQL veritabanlarında denetim ve tehdit algılamayı etkinleştirme](security-center-enable-auditing-on-sql-databases.md) |SQL veritabanı veritabanları için denetim ve tehdit algılamayı etkinleştirmenizi önerir. (Yalnızca SQL veritabanı hizmeti. , Sanal makinelerinizde çalışan Microsoft SQL Server içermez.) |
| [Saydam Veri Şifrelemesini Etkinleştirme](security-center-enable-transparent-data-encryption.md) |SQL veritabanları için şifrelemeyi etkinleştirmenizi önerir. (Yalnızca SQL veritabanı hizmeti.) |

Azure kaynaklarınızın önerilerini görmek için Güvenlik Merkezi dikey penceresinde **öneriler** kutucuğunu seçin. **Öneriler** dikey penceresinde, ayrıntıları görmek için bir öneri seçin. Bu örnekte, **SQL Server 'lar üzerinde tehdit algılamayı & denetimi etkinleştir**' i seçelim.

![Öneriler][4]

Aşağıda gösterildiği gibi, Güvenlik Merkezi, denetim ve tehdit algılama özelliğinin etkinleştirilmediği SQL sunucularını gösterir. Denetimi etkinleştirdikten sonra, tehdit algılama ayarlarını ve e-posta ayarlarını güvenlik uyarılarını alacak şekilde yapılandırabilirsiniz. Tehdit algılama, veritabanına ilişkin olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algıladığında sizi uyarır. Uyarılar, güvenlik merkezi panosunda görüntülenir.
![Denetim ve tehdit algılama][5]

Tehdit algılamayı açıp yapılandırmak ve anormal etkinliklerin algılanması sırasında güvenlik uyarılarını alacak e-postaların listesini yapılandırmak için [Azure Portal SQL veritabanı tehdit algılama](../sql-database/sql-database-threat-detection-portal.md) içindeki adımları izleyin.

Öneriler hakkında daha fazla bilgi edinmek için bkz. [güvenlik önerilerini yönetme](security-center-recommendations.md).

## <a name="monitor-security-health"></a>Güvenlik durumunu izleyin
Bir aboneliğin kaynakları için [güvenlik ilkelerini](tutorial-security-policy.md) etkinleştirmenizin ardından, Güvenlik Merkezi olası güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın güvenliğini analiz eder.  **Kaynak güvenlik durumu** kutucuğunda kaynaklarınızın güvenlik durumunu görüntüleyebilirsiniz. **Kaynak güvenlik durumu** kutucuğunda **veriler** ' e tıkladığınızda, **veri kaynakları** dikey penceresi, denetim ve saydam VERI şifrelemesi etkinleştirilmemiş gibi sorunlar için SQL önerileriyle açılır. Ayrıca, veritabanının genel sağlık durumu için öneriler içerir.
![Kaynak güvenlik durumu][6]

Daha fazla bilgi için bkz. [güvenlik durumu izleme](security-center-monitoring.md).

## <a name="manage-and-respond-to-security-alerts"></a>Güvenlik uyarılarını yönetin ve yanıtlayın
Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için [Azure SQL tehdit](../sql-database/sql-database-threat-detection.md)algılamalarından ve diğer Azure kaynaklarından günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir. Öncelikli güvenlik uyarıları listesi, sorunu hızlıca araştırmanız gereken bilgiler ve saldırıyı düzeltme hakkındaki önerilerle birlikte Güvenlik Merkezi'nde gösterilir.

Uyarıları görmek için Güvenlik Merkezi dikey penceresinde **güvenlik uyarıları** kutucuğunu seçin. **Güvenlik uyarıları** dikey penceresinde, uyarıyı tetikleyen olaylar hakkında daha fazla bilgi edinmek için bir uyarı seçin ve bir saldırının düzeltilmesi için yapmanız gereken adımları uygulayın. Bu örnekte **OLASı SQL ekleme**işlemini seçelim.
![Güvenlik uyarıları][7]

Aşağıda gösterildiği gibi, Güvenlik Merkezi, uyarının tetiklendiği, hedef kaynağın, kaynak IP adresinin uygulanabildiği ve nasıl düzeltileceği ile ilgili öneriler sunan ek ayrıntılar sağlar.
![Olası SQL ekleme][8]

Daha fazla bilgi edinmek için bkz. [güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenlik MERKEZI SSS](security-center-faq.md) — hizmeti kullanma hakkında sık sorulan soruları bulun.
* [Güvenlik merkezi planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md) -kuruluşunuzun güvenlik gereksinimlerine ve bulut yönetimi modeline göre güvenlik merkezi kullanımını iyileştirmek için bir dizi adımı ve görevi izleyin.
* [Güvenlik Merkezi veri güvenliği](security-center-data-security.md) – yapılandırma bilgileri, meta veriler, olay günlükleri, kilitlenme dökümü dosyaları ve daha fazlası dahil, güvenlik merkezi 'nin Azure kaynaklarınızla ilgili verileri nasıl toplayıp işleyeceğinizi öğrenin.
* [Güvenlik olaylarını işleme](security-center-incident.md) -Güvenlik Merkezi 'ndeki güvenlik uyarısı özelliğini kullanarak güvenlik olaylarını işleme konusunda size yardımcı olun.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
