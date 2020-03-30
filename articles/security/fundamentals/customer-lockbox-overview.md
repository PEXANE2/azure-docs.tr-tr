---
title: Microsoft Azure için Müşteri Kilit Kutusu
description: Microsoft'un müşteri verilerine erişmesi gerektiğinde bulut sağlayıcısı erişimi üzerinde denetim sağlayan Microsoft Azure için Müşteri Kilit Kutusu'na teknik genel bakış.
author: TerryLanfear
ms.service: security
ms.subservice: security-fundamentals
ms.topic: article
ms.author: terrylan
manager: rkarlin
ms.date: 11/04/2019
ms.openlocfilehash: abc16ae7f7ab8bf15173248a6e7668e689e127de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77561978"
---
# <a name="customer-lockbox-for-microsoft-azure"></a>Microsoft Azure için Müşteri Kilit Kutusu

> [!NOTE]
> Bu özelliği kullanmak için kuruluşunuzun en az **geliştirici**düzeyine sahip bir [Azure destek planına](https://azure.microsoft.com/support/plans/) sahip olması gerekir.

Microsoft Azure için Müşteri Kilit Kutusu, müşterilerin müşteri veri erişim isteklerini gözden geçirmesi ve onaylaması veya reddetmesi için bir arayüz sağlar. Bir Microsoft mühendisinin destek isteği sırasında müşteri verilerine erişmesi gereken durumlarda kullanılır.

Bu makale, Müşteri Kilit Kutusu isteklerinin sonraki incelemeler ve denetimler için nasıl başlatıldığını, izlenir ve depolanır.)

Müşteri Lockbox artık genel olarak kullanılabilir ve şu anda sanal makinelere uzak masaüstü erişimi için etkinleştirilir.

## <a name="workflow"></a>İş akışı

Aşağıdaki adımlar, Müşteri Kilit Kutusu isteği için tipik bir iş akışını ana hatlar.

1. Kuruluştaki bir kişinin Azure iş yüküyle ilgili bir sorunu vardır.

2. Bu kişi sorunu giderdikten, ancak düzeltemedikten [sonra, Azure portalından](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac)bir destek bileti açar. Bilet bir Azure Müşteri Destek Mühendisine atanır.

3. Azure Destek Mühendisi hizmet isteğini inceler ve sorunu gidermek için sonraki adımları belirler.

4. Destek mühendisi standart araçlar ve telemetri kullanarak sorunu çözemezse, bir sonraki adım, Bir Just-In-Time (JIT) erişim hizmeti kullanarak yükseltilmiş izinler istemektir. Bu istek orijinal destek mühendisinden olabilir. Veya sorun Azure DevOps ekibine iletildiçünkü farklı bir mühendisten olabilir.

5. Erişim isteği Azure Mühendisi tarafından gönderildikten sonra, Tam Zamanında hizmet, aşağıdakiler gibi faktörleri dikkate alarak isteği değerlendirir:
    - Kaynağın kapsamı
    - İsteklinin yalıtılmış bir kimlik olup olmadığı veya çok faktörlü kimlik doğrulaması kullanılarak
    - İzin düzeyleri

    JIT kuralına bağlı olarak, bu istek Dahili Microsoft Onaylayıcıları'nın onayını da içerebilir. Örneğin, onaylayan Müşteri destek müşteri adayı veya DevOps Yöneticisi olabilir.

6. İstek müşteri verilerine doğrudan erişim gerektirdiğinde, müşteri kilit kutusu isteği başlatılır. Örneğin, müşterinin sanal makinesine uzak masaüstü erişimi.

    İstek artık Müşteri **Tarafından Bildirilen** bir durumda dır ve erişim izni vermeden önce müşterinin onayını bekler.

7. Müşteri kuruluşunda, Azure aboneliğiiçin [Sahip rolüne](../../role-based-access-control/rbac-and-directory-admin-roles.md#azure-rbac-roles) sahip olan kullanıcı, bekleyen erişim isteği hakkında bilgi vermek üzere Microsoft'tan bir e-posta alır. Müşteri Kilit Kutusu istekleri için, bu kişi atanmış onaylayıcıdır.

    Örnek e-posta:

    ![Azure Müşteri Kilit Kutusu - e-posta bildirimi](./media/customer-lockbox-overview/customer-lockbox-email-notification.png)

8. E-posta bildirimi, Azure portalındaki **Müşteri Kilit Kutusu** bıçağına bağlantı sağlar. Bu bağlantıyı kullanarak, atanan onaylayıcı, kuruluşlarının Customer Lockbox için sahip olduğu bekleyen istekleri görüntülemek için Azure portalında imzalar:

    ![Azure Müşteri Kilit Kutusu - açılış sayfası](./media/customer-lockbox-overview/customer-lockbox-landing-page.png)

   İstek dört gün boyunca müşteri kuyruğunda kalır. Bu süre sonunda, erişim isteği otomatik olarak sona erer ve Microsoft mühendislerine erişim izni verilmez.

9. Bekleyen isteğin ayrıntılarını almak için, atanan onaylayıcı **Bekleyen İsteklerden**kilit kutusu isteğini seçebilir:

    ![Azure Müşteri Kilit Kutusu - bekleyen isteği görüntüleyin](./media/customer-lockbox-overview/customer-lockbox-pending-requests.png)

10. Atanan onaylayıcı, özgün kullanıcı tarafından oluşturulan destek bileti isteğini görüntülemek için **SERVICE REQUEST ID'yi** de seçebilir. Bu bilgiler, Microsoft Destek'in neden devreye girdiği ve bildirilen sorunun geçmişi için bağlam sağlar. Örnek:

    ![Azure Müşteri Kilit Kutusu - destek bileti isteğini görüntüleyin](./media/customer-lockbox-overview/customer-lockbox-support-ticket.png)

11. İsteği inceledikten sonra, atanan onaylayıcı Onayla veya **Reddet'i** seçer: **Deny**

    ![Azure Müşteri Kilit Kutusu - Onayla veya Reddet'i seçin](./media/customer-lockbox-overview/customer-lockbox-approval.png)

    Seçim sonucunda:
    - **Approve**: Access Microsoft mühendisine verilir. Erişim, varsayılan olarak sekiz saatlik bir süre için verilir.
    - **Reddet**: Microsoft mühendisi tarafından yapılan yüksek erişim isteği reddedilir ve başka bir işlem yapılmaz.

Denetim amacıyla, bu iş akışında gerçekleştirilen eylemler [Müşteri Kilit Kutusu istek günlüklerinde](#auditing-logs)günlüğe kaydedilir.

## <a name="auditing-logs"></a>Denetim günlükleri

Müşteri Lockbox günlükleri etkinlik günlüklerinde depolanır. Azure portalında, Müşteri Kilit Kutusu istekleriyle ilgili denetim bilgilerini görüntülemek için **Etkinlik Günlükleri'ni** seçin. Şu gibi belirli eylemlere filtre uygulayabilirsiniz:
- **Lockbox İsteğini Reddet**
- **Lockbox İsteği Oluştur**
- **Lockbox İsteğini Onayla**
- **Lockbox İstek Son Kullanma Süresi**

Örneğin:

![Azure Müşteri Kilit Kutusu - etkinlik günlükleri](./media/customer-lockbox-overview/customer-lockbox-activitylogs.png)

## <a name="supported-services-and-scenarios-in-general-availability"></a>Genel kullanılabilirlik te desteklenen hizmetler ve senaryolar

Aşağıdaki hizmetler ve senaryolar şu anda Müşteri Kilit Kutusu için genel kullanılabilirlik teyiz.

### <a name="remote-desktop-access-to-virtual-machines"></a>Sanal makinelere uzak masaüstü erişimi

Müşteri Lockbox şu anda sanal makinelere uzak masaüstü erişim istekleri için etkindir. Aşağıdaki iş yükleri desteklenir:
- Hizmet olarak platform (PaaS) - Azure Bulut Hizmetleri (web rolü ve çalışan rolü)
- Hizmet olarak altyapı (IaaS) - Windows ve Linux (yalnızca Azure Kaynak Yöneticisi)
- Sanal makine ölçek seti - Windows ve Linux

> [!NOTE]
> IaaS Classic örnekleri Customer Lockbox tarafından desteklenmez. IaaS Classic örneklerinde çalışan iş yükleriniz varsa, bunları Klasik Kaynak Yöneticisi dağıtım modellerinden kaynak yöneticisi dağıtım modellerine geçirmenizi öneririz. Yönergeler için, [IaaS kaynaklarının klasikten Azure Kaynak Yöneticisi'ne Platform destekli geçişine](../../virtual-machines/windows/migration-classic-resource-manager-overview.md)bakın.

#### <a name="detailed-audit-logs"></a>Ayrıntılı denetim günlükleri

Uzak masaüstü erişimi içeren senaryolar için, Microsoft mühendisi tarafından gerçekleştirilen eylemleri gözden geçirmek için Windows olay günlüklerini kullanabilirsiniz. Etkinlik günlüklerinizi toplamak ve verileri analiz için çalışma alanınıza kopyalamak için Azure Güvenlik Merkezi'ni kullanmayı düşünün. Daha fazla bilgi için [Azure Güvenlik Merkezi'nde Veri toplama](../../security-center/security-center-enable-data-collection.md)bilgisine bakın.

## <a name="supported-services-and-scenarios-in-preview"></a>Önizlemede desteklenen hizmetler ve senaryolar

Aşağıdaki hizmetler şu anda Müşteri Kilit Kutusu için önizlemededir:

- Azure Storage

- Azure SQL DB

- Azure Veri Gezgini

- Sanal makineler (artık bellek dökümlerine ve yönetilen disklere erişimi de kapsamaktadır)

- Azure abonelik aktarımları

Kuruluşunuz için bu önizleme teklifleri için Müşteri Kilit Kutusu'yu etkinleştirmek [için Azure Genel Önizleme için Customer Lockbox'a](https://aka.ms/customerlockbox/insiderprogram)kaydolun.


## <a name="exclusions"></a>Dışlamalar

Müşteri Kilit Kutusu istekleri aşağıdaki mühendislik destek senaryolarında tetiklenmez:

- Bir Microsoft mühendisinin standart çalışma yordamlarının dışında kalan bir etkinlik yapması gerekir. Örneğin, beklenmeyen veya öngörülemeyen senaryolarda hizmetleri kurtarmak veya geri yüklemek için.

- Bir Microsoft mühendisi sorun giderme nin bir parçası olarak Azure platformuna erişir ve istemeden müşteri verilerine erişebilir. Örneğin, Azure Ağ Ekibi, bir ağ aygıtında paket yakalamayla sonuçlanan sorun giderme gerçekleştirir. Ancak, müşteri verileri aktarım sırasında şifrelediyse, mühendis verileri okuyamaz.

## <a name="next-steps"></a>Sonraki adımlar

Müşteri Kilit Kutusu, en az **geliştirici**düzeyine sahip bir [Azure destek planına](https://azure.microsoft.com/support/plans/) sahip tüm müşteriler için otomatik olarak kullanılabilir.

Uygun bir destek planınız olduğunda, Müşteri Kilit Kutusu'yu etkinleştirmek için herhangi bir işlem yapmanızı gerektirmez. Bu eylem, kuruluşunuzdaki birinden gelen bir destek biletini kaydetmek için gerekliyse, müşteri Kilit Kutusu istekleri bir Microsoft mühendisi tarafından başlatılır.
