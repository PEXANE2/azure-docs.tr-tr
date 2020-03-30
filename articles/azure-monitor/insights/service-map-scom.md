---
title: VMs Haritası için Azure Monitörünü Operasyon Yöneticisi ile Tümleştir | Microsoft Dokümanlar
description: Sanal Ayarlar için Azure Monitor, Windows ve Linux sistemlerindeki uygulama bileşenlerini otomatik olarak keşfeder ve hizmetler arasındaki iletişimi eşler. Bu makalede, Operations Manager'da otomatik olarak dağıtılmış uygulama diyagramları oluşturmak için Harita özelliğinin kullanılması anlatılmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/12/2019
ms.openlocfilehash: 112413720f969474369555a74bc89846666e2ef9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77663462"
---
# <a name="integrate-system-center-operations-manager-with-azure-monitor-for-vms-map-feature"></a>VMs Map özelliği için Sistem Merkezi Operasyon Yöneticisini Azure Monitörile Tümleştir

Sanal Ayarlar için Azure Monitörü'nde, Azure'da veya çevrenizde çalışan Windows ve Linux sanal makinelerinde (VM) keşfedilen uygulama bileşenlerini görüntüleyebilirsiniz. Harita özelliği ve Sistem Merkezi Operasyon Yöneticisi arasındaki bu tümleştirme yle, İşlem Yöneticisi'nde, VM'ler için Azure Monitörü'ndeki dinamik bağımlılık eşlemlerini temel alan dağıtılmış uygulama diyagramları otomatik olarak oluşturabilirsiniz. Bu makalede, sistem merkezi operasyon yöneticisi yönetim grubunuzun bu özelliği destekleyecek şekilde nasıl yapılandırılabildiğini açıklanmaktadır.

>[!NOTE]
>Hizmet Haritası'nı zaten dağıttıysanız, haritalarınızı VM'ler için Azure Monitörü'nde görüntüleyebilirsiniz ve bu özellikler VM durumunu ve performansını izlemek için ek özellikler içerir. VM'ler için Azure Monitor'un Harita özelliği, bağımsız Hizmet Haritası çözümünü değiştirmek için tasarlanmıştır. Daha fazla bilgi edinmek [için VM'lere genel bakış için Azure Monitor'a](vminsights-overview.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Bir Sistem Merkezi Operasyon Yöneticisi yönetim grubu (2012 R2 veya sonrası).
* VM'ler için Azure Monitor'u destekleyecek şekilde yapılandırılan bir Günlük Analizi çalışma alanı.
* Operations Manager tarafından izlenen ve Log Analytics çalışma alanınıza veri gönderen bir veya daha fazla Windows ve Linux sanal makinesi veya fiziksel bilgisayar. Bir Operasyon Yöneticisi yönetim grubuna rapor eden Linux sunucuları, doğrudan Azure Monitor'a bağlanacak şekilde yapılandırılmalıdır. Daha fazla bilgi için, [Log Analytics aracısıyla günlük verilerini topla'daki](../platform/log-analytics-agent.md)genel bakışı inceleyin.
* Log Analytics çalışma alanıyla ilişkili Azure aboneliğine erişimi olan bir hizmet ilkesi. Daha fazla bilgi için [bir hizmet ilkesi oluştur'a](#create-a-service-principal)gidin.

## <a name="install-the-service-map-management-pack"></a>Hizmet Haritası yönetim paketini yükleme

Microsoft.SystemCenter.ServiceMap yönetim paketi paketini (Microsoft.SystemCenter.ServiceMap.mpb) içeri girerek Operations Manager ve Harita özelliği arasındaki tümleştirmeyi etkinleştirebilirsiniz. Yönetim paketi paketini Microsoft [Download Center'dan](https://www.microsoft.com/download/details.aspx?id=55763)indirebilirsiniz. Paket aşağıdaki yönetim paketlerini içerir:

* Microsoft Hizmet Haritası Uygulama Görünümleri
* Microsoft System Center Hizmet Haritası Dahili
* Microsoft System Center Hizmet Haritası Geçersiz Kılar
* Microsoft System Center Hizmet Haritası

## <a name="configure-integration"></a>Tümleştirmeyi yapılandırma

Hizmet Haritası yönetim paketini yükledikten sonra, Operasyon Yöneticisi İşlemleri konsolunuzun **Yönetim** bölmesinde **Operasyon Yönetimi Paketi** altında yeni bir düğüm, Hizmet **Haritası**görüntülenir.

>[!NOTE]
>Operations Management Suite, Log Analytics'i içeren [bir hizmet topluluğuydu,](../terminology.md#april-2018---retirement-of-operations-management-suite-brand) artık [Azure Monitor'un](../overview.md)bir parçasıdır.

VMs Map tümleştirmesi için Azure Monitor'u yapılandırmak için aşağıdakileri yapın:

1. Yapılandırma sihirbazını açmak **için, Hizmet Haritasıgenel Bakış** bölmesinde **çalışma alanı ekle'yi**tıklatın.  

    ![Hizmet Haritası Genel Bakış bölmesi](media/service-map-scom/scom-configuration.png)

2. Bağlantı **Yapılandırması** penceresinde, kiracı adı veya kimliği, uygulama kimliği (kullanıcı adı veya istemci kimliği olarak da bilinir) ve hizmet sorumlusunun parolasını girin ve **sonra İleri'yi**tıklatın. Daha fazla bilgi için bir hizmet ilkesi oluştur'a gidin.

    ![Bağlantı Yapılandırma penceresi](media/service-map-scom/scom-config-spn.png)

3. Abonelik **Seçimi** penceresinde, Azure aboneliğini, Azure kaynak grubunu (Günlük Analitiği çalışma alanını içeren grubu) ve Günlük Analitiği çalışma alanını seçin ve ardından **İleri'yi**tıklatın.

    ![Operasyon Yöneticisi Yapılandırma Çalışma Alanı](media/service-map-scom/scom-config-workspace.png)

4. Makine **Grubu Seçimi** penceresinde, Operasyon Yöneticisi ile eşitlemek istediğiniz Servis Haritası Makine Gruplarını seçersiniz. **Makine Grupları Ekle/Kaldır'ı**tıklatın, **Kullanılabilir Makine Grupları**listesinden gruplar seçin ve **Ekle'yi**tıklatın.  Grupları seçmeyi bitirdiğinizde, bitirmek için **Tamam'ı** tıklatın.

    ![Operasyon Yöneticisi Yapılandırma Makine Grupları](media/service-map-scom/scom-config-machine-groups.png)

5. Sunucu **Seçimi** penceresinde, Hizmet Haritası Sunucular Grubu'nu Operations Manager ve Harita özelliği arasında eşitlemek istediğiniz sunucularla yapılandırAbilirsiniz. **Sunucu Ekle/Kaldır'ı**tıklatın.

    Tümleştirmenin bir sunucu için dağıtılmış bir uygulama diyagramı oluşturması için sunucunun aşağıdakiler olması gerekir:

   * Operasyon Müdürü tarafından izlenir
   * VM'ler için Azure Monitor ile yapılandırılan Log Analytics çalışma alanına rapor vermek üzere yapılandırıldı
   * Hizmet Haritası Sunucular Grubunda Listelenmiştir

     ![Operasyon Yöneticisi Yapılandırma Grubu](media/service-map-scom/scom-config-group.png)

6. İsteğe bağlı: Günlük Analitiği ile iletişim kurmak için Tüm Yönetim Sunucuları Kaynak **Havuzu'nu**seçin ve ardından Çalışma Alanı Ekle'yi tıklatın.

    ![Operasyon Yöneticisi Yapılandırma Kaynak Havuzu](media/service-map-scom/scom-config-pool.png)

    Log Analytics çalışma alanını yapılandırmak ve kaydetmek bir dakika sürebilir. Yapılandırıldıktan sonra, Operations Manager ilk Harita eşitlemeyi başlatır.

    ![Operasyon Yöneticisi Yapılandırma Kaynak Havuzu](media/service-map-scom/scom-config-success.png)

## <a name="monitor-integration"></a>Tümleştirmeyi izleyin

Log Analytics çalışma alanı bağlandıktan sonra, Operasyon Yöneticisi İşlemleri konsolunun **İzleme** bölmesinde yeni bir klasör, Hizmet Haritası görüntülenir.

![Operasyon Yöneticisi İzleme bölmesi](media/service-map-scom/scom-monitoring.png)

Hizmet Haritası klasöründe dört düğüm vardır:

* **Etkin Uyarılar**: Operations Manager ve Azure Monitor arasındaki iletişimle ilgili tüm etkin uyarıları listeler.  

  >[!NOTE]
  >Bu uyarılar, Operasyon Yöneticisi ile senkronize edilen Log Analytics uyarıları değildir, Hizmet Haritası yönetim paketinde tanımlanan iş akışlarına göre yönetim grubunda oluşturulur.

* **Sunucular**: VMs Map özelliği için Azure Monitor'dan eşitlenecek şekilde yapılandırılan izlenen sunucuları listeler.

    ![Operasyon Yöneticisi İzleme Sunucuları bölmesi](media/service-map-scom/scom-monitoring-servers.png)

* **Makine Grubu Bağımlılık Görünümleri**: Harita özelliğinden eşitlenen tüm makine gruplarını listeler. Dağıtılmış uygulama diyagramını görüntülemek için herhangi bir grubu tıklatabilirsiniz.

    ![Operasyon Yöneticisi dağıtılan uygulama diyagramı](media/service-map-scom/scom-group-dad.png)

* **Sunucu Bağımlılık Görünümleri**: Harita özelliğinden eşitlenen tüm sunucuları listeler. Dağıtılmış uygulama diyagramını görüntülemek için herhangi bir sunucuyu tıklatabilirsiniz.

    ![Operasyon Yöneticisi dağıtılan uygulama diyagramı](media/service-map-scom/scom-dad.png)

## <a name="edit-or-delete-the-workspace"></a>Çalışma alanını düzenle veya silme

Yapılandırılan çalışma alanını **Hizmet Haritasıgenel bakış** bölmesi **(Yönetim** bölmesi > **Operations Management Suite** > **Service Map)** aracılığıyla düzenleyebilir veya silebilirsiniz.

>[!NOTE]
>Operations Management Suite, artık [Azure Monitor'un](https://github.com/MicrosoftDocs/azure-docs-pr/pull/azure-monitor/overview.md)bir parçası olan Log Analytics'i içeren [bir hizmet topluluğudur.](https://github.com/MicrosoftDocs/azure-docs-pr/pull/azure-monitor/azure-monitor-rebrand.md#retirement-of-operations-management-suite-brand)

Bu geçerli sürümde yalnızca bir Log Analytics çalışma alanını yapılandırabilirsiniz.

![Operasyon Yöneticisi Çalışma Alanı bölmesini edit](media/service-map-scom/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a>Kuralları ve geçersiz kılmaları yapılandır

*Microsoft.SystemCenter.ServiceMapImport.Rule*adlı bir kural, VMs Map özelliği için Azure Monitor'dan düzenli olarak bilgi getirir. Eşitleme aralığını değiştirmek için kuralı geçersiz kılabilir ve **IntervalMinutes**parametresi için değeri değiştirebilirsiniz.

![İşlem yöneticisi özellik penceresini geçersiz kılar](media/service-map-scom/scom-overrides.png)

* **Etkin**: Otomatik güncelleştirmeleri etkinleştirin veya devre dışı.
* **IntervalMinutes**: Güncelleştirmeler arasındaki süreyi belirtir. Varsayılan aralık bir saattir. Haritaları daha sık eşitlemek istiyorsanız, değeri değiştirebilirsiniz.
* **TimeoutSeconds**: İstek zaman ları dolmadan önce süreyi belirtir.
* **TimeWindowMinutes**: Verileri sorgulamak için zaman penceresini belirtir. Varsayılan değer 60 dakikadır ve bu da izin verilen en yüksek aralıktır.

## <a name="known-issues-and-limitations"></a>Bilinen sorunlar ve sınırlamalar

Geçerli tasarım aşağıdaki sorunları ve sınırlamaları sunar:

* Yalnızca tek bir Log Analytics çalışma alanına bağlanabilirsiniz.
* Hizmet Haritası Sunucular Grubu'na **Yazma** bölmesi aracılığıyla el ile sunucu lar ekleyebilirsiniz, ancak bu sunucular için haritalar hemen eşitlenmez. Bunlar, bir sonraki eşitleme döngüsü sırasında VMs Map özelliği için Azure Monitor'dan senkronize edilir.
* Yönetim paketi tarafından oluşturulan Dağıtılmış Uygulama Diyagramlarında herhangi bir değişiklik yaparsanız, bu değişiklikler büyük olasılıkla Bir sonraki eşitlemede VM'ler için Azure Monitor ile birlikte üzerine yazılır.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Hizmet ilkesi oluşturma yla ilgili resmi Azure belgeleri için bkz:

* [PowerShell'i kullanarak bir hizmet ilkesi oluşturma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Azure CLI'yi kullanarak bir hizmet ilkesi oluşturma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
* [Azure portalını kullanarak bir hizmet ilkesi oluşturma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### <a name="feedback"></a>Geri Bildirim
VMs Map özelliği için Azure Monitor ile tümleştirme veya bu dokümantasyon hakkında herhangi bir geri bildiriminiz var mı? Özellikler önerebileceğiniz veya mevcut önerileri oylayabileceğiniz [Kullanıcı Sesi sayfamızı](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map)ziyaret edin.
