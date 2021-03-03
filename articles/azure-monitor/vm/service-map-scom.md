---
title: VM Insights haritasını Operations Manager ile tümleştirin | Microsoft Docs
description: VM öngörüleri Windows ve Linux sistemlerindeki uygulama bileşenlerini otomatik olarak bulur ve hizmetler arasındaki iletişimi eşler. Bu makalede, Operations Manager ' de otomatik olarak dağıtılmış uygulama diyagramları oluşturmak için harita özelliğinin kullanılması anlatılmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/12/2019
ms.openlocfilehash: a39f40c2a284a743db258a49f36cb4f13c2a4d1c
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101725501"
---
# <a name="integrate-system-center-operations-manager-with-vm-insights-map-feature"></a>VM öngörüleri eşleme özelliği ile System Center Operations Manager tümleştirme

VM öngörüleri ' nde, bulunan uygulama bileşenlerini Azure 'da veya ortamınızda çalışan Windows ve Linux sanal makinelerinde (VM) görüntüleyebilirsiniz. Eşleme özelliği ve System Center Operations Manager arasındaki bu tümleştirmeyle, sanal makine öngörülerinde dinamik bağımlılık haritalarını temel alan Operations Manager otomatik olarak dağıtılmış uygulama diyagramları oluşturabilirsiniz. Bu makalede, System Center Operations Manager yönetim grubunuzun bu özelliği destekleyecek şekilde nasıl yapılandırılacağı açıklanmaktadır.

>[!NOTE]
>Daha önce Hizmet Eşlemesi dağıttıysanız, sanal makine öngörülerini, VM sistem durumunu ve performansını izlemek için ek özellikler içeren VM içgörüler ' de görüntüleyebilirsiniz. VM öngörülerinin harita özelliği, tek başına Hizmet Eşlemesi çözümünü değiştirme amaçlıdır. Daha fazla bilgi için bkz. [VM öngörülerine genel bakış](../vm/vminsights-overview.md).

## <a name="prerequisites"></a>Önkoşullar

* System Center Operations Manager yönetim grubu (2012 R2 veya üzeri).
* VM öngörülerini desteklemek için yapılandırılmış bir Log Analytics çalışma alanı.
* Operations Manager tarafından izlenen bir veya daha fazla Windows ve Linux sanal makinesi veya fiziksel bilgisayar Log Analytics çalışma alanınıza veri gönderiyor. Bir Operations Manager yönetim grubuna raporlayan Linux sunucularının Azure Izleyici 'ye doğrudan bağlanacak şekilde yapılandırılması gerekir. Daha fazla bilgi için [Log Analytics aracısıyla günlük verilerini toplama](../agents/log-analytics-agent.md)bölümündeki genel bakışı gözden geçirin.
* Log Analytics çalışma alanıyla ilişkili Azure aboneliğine erişimi olan bir hizmet sorumlusu. Daha fazla bilgi için [hizmet sorumlusu oluşturma](#create-a-service-principal)bölümüne gidin.

## <a name="install-the-service-map-management-pack"></a>Hizmet Eşlemesi yönetim paketini yükler

Operations Manager ile eşleme özelliği arasında tümleştirmeyi etkinleştirerek Microsoft.SystemCenter. ServiceMap yönetim paketi paketini (Microsoft.SystemCenter. ServiceMap. mpb) içeri aktarabilirsiniz. Yönetim Paketi grubunu [Microsoft Indirme merkezi](https://www.microsoft.com/download/details.aspx?id=55763)' nden indirebilirsiniz. Paket aşağıdaki yönetim paketlerini içerir:

* Microsoft Hizmet Eşlemesi uygulama görünümleri
* Microsoft System Center Hizmet Eşlemesi Iç
* Microsoft System Center Hizmet Eşlemesi geçersiz kılmaları
* Microsoft System Center Hizmet Eşlemesi

## <a name="configure-integration"></a>Tümleştirmeyi yapılandırma

Hizmet Eşlemesi yönetim paketini yükledikten sonra, **hizmet eşlemesi** yeni bir düğüm, Operations Manager işletim konsolunuzun **Yönetim** bölmesinde **Operations Management Suite** altında görüntülenir.

>[!NOTE]
>[Operations Management Suite, Log Analytics dahil olan hizmetlerden oluşan bir koleksiyondur](../terminology.md#april-2018---retirement-of-operations-management-suite-brand) . artık [Azure izleyici](../overview.md)'nin bir parçasıdır.

VM öngörüleri eşleme tümleştirmesini yapılandırmak için şunları yapın:

1. Yapılandırma Sihirbazı 'nı açmak için **hizmet eşlemesi genel bakış** bölmesinde **çalışma alanı Ekle**' ye tıklayın.  

    ![Hizmet Eşlemesi Genel bakış bölmesi](media/service-map-scom/scom-configuration.png)

2. **Bağlantı yapılandırması** penceresinde kiracı adını veya kimliğini, uygulama kimliğini (Kullanıcı adı veya ClientID olarak da bilinir) ve hizmet sorumlusu parolasını girin ve ardından **İleri**' ye tıklayın. Daha fazla bilgi için hizmet sorumlusu oluşturma bölümüne gidin.

    ![Bağlantı yapılandırma penceresi](media/service-map-scom/scom-config-spn.png)

3. **Abonelik seçimi** penceresinde Azure aboneliği, Azure Kaynak grubu (Log Analytics çalışma alanını içeren) ve Log Analytics çalışma alanı ' nı seçin ve ardından **İleri**' ye tıklayın.

    ![Operations Manager yapılandırma çalışma alanı](media/service-map-scom/scom-config-workspace.png)

4. **Makine grubu seçim** penceresinde, Operations Manager eşitlemek Istediğiniz hizmet eşlemesi makine gruplarını seçersiniz. **Makine gruplarını Ekle/Kaldır**' a tıklayın, **kullanılabilir makine grupları** listesinden gruplar ' ı seçin ve **Ekle**' ye tıklayın.  Grupları seçmeyi tamamladığınızda, **Tamam** ' a tıklayarak işlemi sona erdirin.

    ![Operations Manager yapılandırma makinesi grupları](media/service-map-scom/scom-config-machine-groups.png)

5. **Sunucu seçimi** penceresinde, hizmet eşlemesi servers grubunu Operations Manager ile eşleme özelliği arasında eşitlemek istediğiniz sunucularla yapılandırırsınız. **Sunucu Ekle/Kaldır**' a tıklayın.

    Bir sunucu için dağıtılmış uygulama diyagramı oluşturmak amacıyla tümleştirme için sunucu şu şekilde olmalıdır:

   * Operations Manager tarafından izlenen
   * VM öngörüleri ile yapılandırılan Log Analytics çalışma alanına raporlamak için yapılandırılır
   * Hizmet Eşlemesi Servers grubunda listelenmiştir

     ![Operations Manager yapılandırma grubu](media/service-map-scom/scom-config-group.png)

6. İsteğe bağlı: Log Analytics iletişim kurmak için tüm yönetim sunucuları kaynak havuzunu seçin ve ardından **çalışma alanı Ekle**' ye tıklayın.

    ![Tüm yönetim sunucuları kaynak havuzu seçiliyken Microsoft Operations Management Suite çalışma alanı Ekle içindeki sunucu havuzu ekranının ekran görüntüsü.](media/service-map-scom/scom-config-pool.png)

    Log Analytics çalışma alanını yapılandırmak ve kaydettirmek bir dakika sürebilir. Yapılandırıldıktan sonra, Operations Manager ilk eşleme eşitlemesini başlatır.

    ![Çalışma alanının eklendiğini onaylayan Microsoft Operations Management Suite Ekle çalışma alanındaki tamamlama ekranının ekran görüntüsü.](media/service-map-scom/scom-config-success.png)

## <a name="monitor-integration"></a>İzleme tümleştirmesi

Log Analytics çalışma alanı bağlandıktan sonra, Operations Manager Işletim konsolunun **izleme** bölmesinde yeni bir klasör hizmet eşlemesi görüntülenir.

![Operations Manager Izleme bölmesi](media/service-map-scom/scom-monitoring.png)

Hizmet Eşlemesi klasörü dört düğüme sahiptir:

* **Etkin uyarılar**: Operations Manager Ile Azure izleyici arasındaki iletişimle ilgili tüm etkin uyarıları listeler.  

  >[!NOTE]
  >Bu uyarılar Operations Manager ile eşitlenen Log Analytics uyarıları değil, Hizmet Eşlemesi yönetim paketinde tanımlanan iş akışlarına göre yönetim grubunda oluşturulur.

* **Sunucular**: VM öngörüleri eşleme özelliğinden eşitlenecek şekilde yapılandırılan izlenen sunucuları listeler.

    ![Operations Manager Izleme sunucuları bölmesi](media/service-map-scom/scom-monitoring-servers.png)

* **Makine grubu bağımlılığı görünümleri**: harita özelliğinden eşitlenen tüm makine gruplarını listeler. Dağıtılmış uygulama diyagramını görüntülemek için herhangi bir gruba tıklayabilirsiniz.

    ![Her makine grubu için görüntü içeren bir diyagramı gösteren Hizmet Eşlemesi ekran görüntüsü ve aralarındaki bağımlılıkları belirten satırlar.](media/service-map-scom/scom-group-dad.png)

* **Sunucu bağımlılığı görünümleri**: harita özelliğinden eşitlenen tüm sunucuları listeler. Dağıtılmış uygulama diyagramını görüntülemek için herhangi bir sunucu ' ya tıklayabilirsiniz.

    ![Her sunucu için görüntü içeren bir diyagramı gösteren Hizmet Eşlemesi ekran görüntüsü ve aralarındaki bağımlılıkları belirten satırlar.](media/service-map-scom/scom-dad.png)

## <a name="edit-or-delete-the-workspace"></a>Çalışma alanını düzenleme veya silme

Yapılandırılan çalışma alanını **hizmet eşlemesi genel bakış** bölmesinde düzenleyebilir veya silebilirsiniz (**Yönetim** bölmesi > **Operations Management Suite**  >  **hizmet eşlemesi**).

> [!NOTE]
> Operations Management Suite, artık [Azure izleyici](../overview.md)'nin bir parçası olan Log Analytics dahil olan [hizmetlerden oluşan bir koleksiyondur](../terminology.md#april-2018---retirement-of-operations-management-suite-brand) .

Bu geçerli sürümde yalnızca bir Log Analytics çalışma alanı yapılandırabilirsiniz.

![Operations Manager düzenleme çalışma alanı bölmesi](media/service-map-scom/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a>Kuralları ve geçersiz kılmaları yapılandırma

Bir kural, *Microsoft.SystemCenter. Servicemapımport. Rule*, düzenlı aralıklarla VM öngörüleri eşleme özelliğinden bilgi getirir. Eşitleme aralığını değiştirmek için kuralı geçersiz kılabilir ve değeri **ınterinterval** parametresi için değiştirebilirsiniz.

![Operations Manager Özellikler penceresi geçersiz kılınır](media/service-map-scom/scom-overrides.png)

* **Etkin**: otomatik güncelleştirmeleri etkinleştirin veya devre dışı bırakın.
* **Interınminutes**: güncelleştirmeler arasındaki süreyi belirtir. Varsayılan Aralık bir saattir. Haritaları daha sık eşitlemek istiyorsanız, değeri değiştirebilirsiniz.
* **TimeoutSeconds**: istek zaman aşımına uğramadan önceki sürenin uzunluğunu belirtir.
* **Timewindowminutes**: verileri sorgulamak için zaman penceresini belirtir. Varsayılan değer, izin verilen en fazla Aralık olan 60 dakikadır.

## <a name="known-issues-and-limitations"></a>Bilinen sorunlar ve sınırlamalar

Geçerli tasarımda aşağıdaki sorunlar ve sınırlamalar sunulmaktadır:

* Yalnızca tek bir Log Analytics çalışma alanına bağlanabilirsiniz.
* **Yazma** bölmesi aracılığıyla hizmet eşlemesi Servers grubuna el ile sunucu ekleyebilseniz de, bu sunucuların haritaları hemen eşitlenmez. Bir sonraki eşitleme çevrimi sırasında VM öngörüleri eşleme özelliğinden eşitlenecek.
* Yönetim paketi tarafından oluşturulan dağıtılmış uygulama diyagramlarında herhangi bir değişiklik yaparsanız, bu değişikliklerin büyük olasılıkla VM öngörülerine sahip bir sonraki eşitlemede üzerine yazılır.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Hizmet sorumlusu oluşturma hakkında resmi Azure belgeleri için bkz.:

* [PowerShell kullanarak hizmet sorumlusu oluşturma](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)
* [Azure CLı kullanarak hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli)
* [Azure portal kullanarak bir hizmet sorumlusu oluşturma](../../active-directory/develop/howto-create-service-principal-portal.md)

### <a name="suggestions"></a>Öneriler

VM öngörüleri eşleme özelliği veya bu belgeler hakkında bizimle ilgili geri bildiriminiz var mı? Özellik önerdiğiniz veya mevcut önerilere oy oluşturabileceğiniz [Kullanıcı ses](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map)sayfamızı ziyaret edin.

