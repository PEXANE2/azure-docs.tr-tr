---
title: Makineleri ve Azure Güvenlik Merkezi'nde uygulamalarınızı koruma | Microsoft Docs
description: Bu belgede ele yardımcı olacak öneriler Güvenlik Merkezi'nde sanal makinelerinizi ve bilgisayarlar ve web uygulamaları ve App Service ortamları koruma.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/20/2019
ms.author: v-mohabe
ms.openlocfilehash: c8f381a3440d742cca880f44b73cbc22bde92ecc
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910556"
---
# <a name="protecting-your-machines-and-applications-in-azure-security-center"></a>Makineleri ve Azure Güvenlik Merkezi'nde uygulamalarınızı koruma
Azure Güvenlik Merkezi, Azure kaynaklarınızın, Azure olmayan sunucularınızın ve sanal makinelerinizin güvenlik durumunu analiz eder. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, gerekli denetimlerin yapılandırılması işlemi boyunca size rehberlik öneriler oluşturur. Öneriler, Azure kaynak türleri için geçerlidir: sanal makineleri (VM'ler) ve bilgisayarlar, uygulamalar, ağ, SQL ve kimlik ve erişim.

Bu makalede makineleri ve uygulamaları için uygulama önerileri ele alır.

## <a name="monitoring-security-health"></a>Güvenlik durumunu izleme
Kaynaklarınızın güvenlik durumunu izleyebilirsiniz **Güvenlik Merkezi-genel bakış** Pano. **Kaynakları** bölüm, her kaynak türü için tanımlanan sorunların sayısını ve güvenlik durumunu sağlar.

Seçerek, tüm sorunların bir listesini görüntüleyebilirsiniz **önerileri**. Önerileri uygulama hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md).

Tam listesi işlem ve App services önerileri için bkz: [önerileri](security-center-virtual-machine-protection.md#compute-and-app-recommendations).

Devam etmek için seçin **işlem ve uygulamalar** altında **kaynakları** veya Güvenlik Merkezi ana menüsünde.
![Güvenlik Merkezi Panosu](./media/security-center-virtual-machine-recommendations/overview.png)

## <a name="monitor-compute-and-app-services"></a>İşlem ve uygulama hizmetlerini izleme
**İşlem & uygulamalar**altında aşağıdaki sekmeler vardır:

- **Genel Bakış**: Güvenlik Merkezi tarafından belirlenen izleme ve öneriler.
- **Sanal makineler ve bilgisayarlar**: Sanal makinelerinizin, bilgisayarlarınızın ve her birinin geçerli güvenlik durumunun listesi.
- **Cloud Services**: Güvenlik Merkezi tarafından izlenen web ve çalışan rollerinizin listesi.
- **Uygulama hizmetleri**: App Service ortamlarınızın ve geçerli güvenlik durumunun listesi.
- **Kapsayıcılar (Önizleme)** : IaaS Linux makinelerinde barındırılan kapsayıcılarınızın listesi ve Docker yapılandırmalarının güvenlik değerlendirmesi.
- **İşlem kaynakları (Önizleme)** : Service Fabric kümeleri ve Olay Hub 'ları gibi işlem kaynaklarınız için önerilerin listesi.

Devam etmek için, **kaynak güvenliği durumu**altında **işlem & uygulamalar** ' ı seçin.

![İşlem](./media/security-center-virtual-machine-recommendations/compute.png)

Her sekmede birden fazla bölüm olabilir ve her bölümde, sorunu çözmek üzere önerilen adımlarla ilgili daha fazla ayrıntı görmek için ayrı ayrı seçenekler belirleyebilirsiniz.

### İzlenmeyen VM'ler ve bilgisayarlar <a name="unmonitored-vms-and-computers"></a>
Microsoft Monitoring Agent uzantısını makine çalışmıyorsa bir VM'yi veya bilgisayarı Güvenlik Merkezi tarafından izlenmez. Bir makinede yerel bir aracı zaten yüklü olabilir, örneğin OMS doğrudan Aracısı veya System Center Operations Manager Aracısı. Bu aracıları makinelerle olarak tanımlandığında bu aracılar Güvenlik Merkezi'nde tam olarak desteklenmediğinden izlenmeyen. Güvenlik Merkezi’nin tüm özelliklerinden tam olarak faydalanmak için, Microsoft Monitoring Agent uzantısı gereklidir.

İzlenmeyen VM'de veya bilgisayarda zaten yüklü olan yerel aracıya ek olarak, uzantı yükleyebilirsiniz. İki aracıyı da aynı çalışma alanına bağlayarak aynı şekilde yapılandırın. Bu, Güvenlik Merkezi’nin Microsoft Monitoring Agent uzantısıyla etkileşim kurup veri toplamasını sağlar. Microsoft Monitoring Agent uzantısını nasıl yükleyeceğiniz hakkında yönergeler için bkz. [VM uzantısını etkinleştir](../azure-monitor/learn/quick-collect-azurevm.md).

Güvenlik Merkezi’nin otomatik hazırlama için başlatılan VM’leri ve bilgisayarları başarılı bir şekilde izleyememe nedenleri hakkında daha fazla bilgi edinmek için bkz. [Aracı durumu sorunlarını izleme](security-center-troubleshooting-guide.md#mon-agent).

### <a name="recommendations"></a>Öneriler
Bu bölümde, her VM ve bilgisayar, web ve çalışan rolleri, Azure App Service Web Apps ve Güvenlik Merkezi'nin izlediği Azure App Service ortamı için önerilerin kümesine sahiptir. İlk sütunda öneriler listelenmiştir. İkinci sütunda, bu önerinin etkilediği kaynakların toplam sayısı gösterilmektedir. Üçüncü sütun, sorunun önem derecesini gösterir.

Her önerinin gerçekleştirebileceğiniz bir eylemler kümesi bulunur seçtikten sonra. Örneğin, **eksik sistem güncelleştirmeleri**' ni seçerseniz, yamaları eksik olan VM 'ler ve bilgisayar sayısı ve eksik güncelleştirmenin önem derecesi görüntülenir.

**Sistem güncelleştirmelerini uygulayın** grafik biçiminde, bir Windows ve Linux için kritik güncelleştirmelerin özeti vardır. İkinci bölümde, aşağıdaki bilgileri içeren bir tablo vardır:

- **AD**: Eksik güncelleştirmenin adı.
- **VM’LER VM 'Ler & BILGISAYARLAR**: Bu güncelleştirme eksik olan sanal makinelerin ve bilgisayarların toplam sayısı.
- **GÜNCELLEŞTİRME ÖNEM DERECESİ**: Bu belirli bir önerinin önem derecesini açıklar:

    - **Kritik**: Anlamlı bir kaynak (uygulama, sanal makine veya ağ güvenlik grubu) ile bir güvenlik açığı bulunur ve dikkat edilmesi gerekir.
    - **Önemli**: Bir işlemi gerçekleştirmek veya bir güvenlik açığını ortadan kaldırmak için kritik olmayan veya ek adımlar gerekir.
    - **Orta**: Bir güvenlik açığına değinilmesi gerekir, ancak hemen ilgilenilmesi gerekmez. (Varsayılan olarak, düşük öneriler sunulmaz ancak bunları görüntülemek istiyorsanız düşük öneriler filtresini kullanabilirsiniz.)


- **DURUM**: Önerinin geçerli durumu:

    - Şunu **Aç**: Öneri henüz sağlanmadı.
    - **Devam ediyor**: Öneri şu anda bu kaynaklara uygulanıyor ve sizin için herhangi bir eylem gerekmiyor.
    - **Çözümlendi**: Öneri zaten tamamlandı. (Sorun çözüldüğünde girdi soluklaşır).

Öneri ayrıntılarını görüntülemek için listeden eksik güncelleştirmenin adına tıklayın.


> [!NOTE]
> Buradaki güvenlik önerileri altında aynıdır **önerileri** Döşe. Bkz: [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md) önerileri çözümleme hakkında daha fazla bilgi için.
>
>

### <a name="vms-and-computers"></a>VM'ler ve bilgisayarlar
VM'ler ve bilgisayarlar bölümü, tüm VM ve bilgisayar önerilerini genel bir bakış sağlar. Her sütunda bir dizi öneri sunulur.

![VM ve bilgisayar önerileri](./media/security-center-virtual-machine-recommendations/vm-computers.png)

Simgeler bu listede gösterilen dört tür vardır:

![Azure olmayan bilgisayar](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png) Azure olmayan bilgisayar.

![Azure Resource Manager VM](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png) Azure Resource Manager sanal makinesi.

![Azure Klasik VM](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png) Azure Klasik VM.


![Vm'leri çalışma alanından tanımlanır](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png) VM'ler yalnızca görüntülenen abonelik parçası olan çalışma alanından tanımlanır. Bu, Bu abonelikteki çalışma alanına rapor veren diğer aboneliklerden gelen VM 'Leri ve Operations Manager doğrudan aracıyla yüklenen ve kaynak KIMLIĞI olmayan VM 'leri içerir.

Her önerinin altında görüntülenen simge VM ile dikkat ve önerinin türünü gerektiren bilgisayar hızlıca tanımlamanıza yardımcı olur. Ayrıca, listeyi **kaynak türüne** göre ve **önem derecesine**göre aramak için filtreleri de kullanabilirsiniz.

Her VM 'nin güvenlik önerilerinin ayrıntılarına gitmek için VM 'ye tıklayın.
![Bulut hizmetleri](./media/security-center-virtual-machine-recommendations/recommendation-list.png)

### <a name="cloud-services"></a>Bulut hizmetleri
Bulut hizmetleri için, işletim sistemi sürümü güncel olmadığında bir öneri oluşturulur.

![Bulut hizmetleri](./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png)

(Bu durum önceki örnek için geçerli değildir) bir öneri sahip olduğu bir senaryoda, işletim sistemi sürümünü güncelleştirmek için Önerideki adımları izlemeniz gerekir. Bir güncelleştirme mevcut olduğunda uyarı alırsınız (sorunun önem derecesine bağlı olarak kırmızı veya turuncu). Bu uyarıyı WebRole1 (Web uygulamanızı IIS 'e otomatik olarak dağıtılan Windows Server çalıştırır) veya WorkerRole1 (Web uygulamanızı IIS 'e otomatik olarak dağıtarak Windows Server çalıştırır) satırlarına seçtiğinizde, bu öneriye ilişkin daha fazla ayrıntı görürsünüz.

Bu öneriyle ilgili daha kesin bir açıklama görmek için **AÇIKLAMA** sütunu altındaki **İşletim sistemi sürümünü güncelleştir**’e tıklayın.



![İşletim sistemi sürümünü güncelleştirme](./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new4.png)

### <a name="app-services"></a>Uygulama hizmetleri
App Service bilgilerini görüntülemek için aboneliğinizdeki App Service etkinleştirmeniz gerekir. Bu özelliği etkinleştirme hakkında yönergeler için bkz. [Azure Güvenlik Merkezi ile App Service koruma](security-center-app-services.md).
[!NOTE]
> App Service'ı izleme önizlemede ve Güvenlik Merkezi'nin standart katmanında yalnızca kullanılabilir.


Altında **uygulama hizmetleri**, App service ortamları listesini bulmak ve Güvenlik Merkezi değerlendirmesini temel alan durum özeti gerçekleştirilir.

![Uygulama hizmetleri](./media/security-center-virtual-machine-recommendations/app-services.png)

Üç tür bu listede gösterilen simge vardır:

![App services ortamı](./media/security-center-virtual-machine-recommendations/ase.png) App services ortamı.

![Web uygulaması](./media/security-center-virtual-machine-recommendations/web-app.png) Web uygulaması.

![İşlev uygulaması](./media/security-center-virtual-machine-recommendations/function-app.png) İşlev uygulaması.

1. Bir web uygulaması'nı seçin. Özet görünümü ile üç sekme açar:

   - **Öneriler**: Değerlendirme başarısız oldu Güvenlik Merkezi tarafından gerçekleştirilen göre.
   - **Değerlendirmeler geçirilen**: Güvenlik Merkezi tarafından geçirilen gerçekleştirdiği değerlendirmeler listesi.
   - **Kullanılamayan iç değerlendirmeler**: bir hata veya öneri nedeniyle çalıştırılamadı değerlendirmelerinin listesini belirli bir App service için uygun değil

   Altında **önerileri** Seçili web uygulaması için önerilerin bir listesi ve önerilerin önem derecesi.

   ![Uygulama Hizmetleri önerileri](./media/security-center-virtual-machine-recommendations/app-services-rec.png)

2. Önerinin açıklamasını ve sağlıksız kaynakların, sağlıklı kaynakların ve taranmamış kaynakların bir listesini görmek için bir öneri seçin.

   - **Geçirilen değerlendirmeler** sütununun altında, geçirilen değerlendirmelerin listesi bulunur.  Bu değerlendirmeler önemini her zaman büyük/küçük harf yeşildir.

   - Geçirilen bir değerlendirme, listenin değerlendirme açıklaması için sağlıksız ve iyi durumda kaynakların bir listesini ve Taranmayan kaynaklar listesini seçin. İyi durumda olmayan kaynaklar için bir sekme yoktur ancak değerlendirmede başarılı olduğundan bu liste her zaman boştur.

     ![Düzeltme App Service](./media/security-center-virtual-machine-recommendations/app-service-remediation.png)

## <a name="virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri
Güvenlik Merkezi, ölçek kümelerine sahip olup olmadığını otomatik olarak bulur ve Microsoft Monitoring Agent bu ölçek kümelerine yüklemenizi önerir. 

Microsoft Monitoring Agent yüklemek için: 

1. **İzleme aracısını sanal makine ölçek kümesine yükler**önerisini seçin. İzlenmeyen ölçek kümelerinin bir listesini alırsınız.
2. Sağlıksız bir ölçek kümesi seçin. İzleme aracısını mevcut doldurulmuş bir çalışma alanını kullanarak yüklemek veya yeni bir tane oluşturmak için yönergeleri izleyin. Ayarlı değilse, çalışma alanı [fiyatlandırma katmanını](security-center-pricing.md) ayarladığınızdan emin olun.

   ![MMS 'yi yükler](./media/security-center-virtual-machine-recommendations/install-mms.png)

Yeni ölçek kümelerini Microsoft Monitoring Agent otomatik olarak yükleyecek şekilde ayarlamak için:
1. Azure Ilkesi ' ne gidin ve **tanımlar**' a tıklayın.
2. **Windows sanal makine ölçek kümeleri için Log Analytics aracısını dağıtma** ilkesi için arama yapın ve üzerine tıklayın.
3. **Ata**'ya tıklayın.
4. **Kapsamı** ve **Log Analytics çalışma alanını** ayarlayıp **ata**' ya tıklayın.

Microsoft Monitoring Agent yüklemek için tüm mevcut ölçek kümelerini ayarlamak istiyorsanız, Azure Ilkesinde, **Düzeltme** ' ye gidin ve mevcut ilkeyi mevcut ölçek kümelerine uygulayın.


## <a name="compute-and-app-recommendations"></a>İşlem ve uygulama önerileri
|Kaynak türü|Güvenlik puanı|Öneri|Açıklama|
|----|----|----|----|
|App Service|20|Web uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır|Web uygulamalarının erişim yalnızca HTTPS üzerinden sınırlayın.|
|App Service|20|İşlev uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır|İşlev uygulamaları, erişim yalnızca HTTPS üzerinden sınırlayın.|
|App Service|5|Uygulama hizmetlerindeki tanılama günlükleri etkinleştirilmelidir|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|App Service|10|Uzaktan hata ayıklama için Web uygulaması kapalı olmaları|Artık bunu kullanmanız gerekiyorsa, Web uygulamaları için hata ayıklama devre dışı bırakın. Uzaktan hata ayıklama, gelen bağlantı noktası üzerinde bir işlev uygulaması açılmasını gerektirir.|
|App Service|10|Uzaktan hata ayıklama işlev uygulaması için kapatılmalıdır|Artık bunu kullanmanız gerekiyorsa, işlev uygulaması için hata ayıklama devre dışı bırakın. Uzaktan hata ayıklama, gelen bağlantı noktası üzerinde bir işlev uygulaması açılmasını gerektirir.|
|App Service|10|Tüm izin verme ('* ') uygulamanıza erişmek için kaynaklar| WEBSITE_LOAD_CERTIFICATES parametre kümesi izin verme "". Parametresini ayarlayarak '' tüm sertifikalar, web uygulamaları kişisel sertifika deposuna yüklendiğini anlamına gelir. Site çalışma zamanında tüm sertifikalara erişim gerektiğini olası olmadığından bu en düşük öncelik ilkesini kötüye neden olabilir.|
|App Service|20|CORS her kaynağın Web uygulamalarınıza erişmek izin vermemelidir|Web uygulamanızla etkileşim kurmak yalnızca gerekli etki alanı sağlar. Kaynak kaynak paylaşımı (CORS) tüm etki alanları web uygulamanıza erişmek izin vermemelisiniz.|
|App Service|20|CORS her kaynağın işlev uygulamanıza erişmek izin vermemelidir| İşlev uygulamanızla etkileşim kurmak yalnızca gerekli etki alanı sağlar. Kaynak kaynak paylaşımı (CORS) tüm etki alanlarının işlev uygulamanıza erişmek izin vermemelisiniz.|
|İşlem kaynakları (toplu)|1\.|Toplu Iş hesaplarında ölçüm uyarısı kuralları yapılandırılmalıdır|Batch hesabında ölçüm uyarı kuralları yapılandırın ve havuzu Silme Tamamlandı olayları ve havuz silme başlangıç olayları ölçümlerini etkinleştir|
|İşlem kaynakları (service fabric)|10|Service Fabric kümeler yalnızca istemci kimlik doğrulaması için Azure Active Directory kullanmalıdır|İstemci kimlik doğrulaması yalnızca Azure Active Directory aracılığıyla Service Fabric'te gerçekleştirin.|
|İşlem kaynakları (Otomasyon hesabı)|5|Otomasyon hesabı değişkenleri şifrelenmelidir|Otomasyon hesabı değişken varlıkları şifrelenmesi, hassas verileri depolarken etkinleştirin.|
|İşlem kaynakları (arama)|5|Arama Hizmetleri için tanılama günlüklerini etkinleştirme denetimi|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (hizmet veri yolu)|5|Service Bus tanılama günlükleri etkinleştirilmelidir|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (akış analizi)|5|Azure Stream Analytics tanılama günlükleri etkinleştirilmelidir|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (toplu)|5|Batch hesapları, tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (olay hub'ı)|5|Olay Hub 'ındaki tanılama günlükleri etkinleştirilmelidir|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (mantıksal uygulamalar)|5|Logic apps'teki tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (service fabric)|15|Service fabric'te EncryptAndSign ClusterProtectionLevel özelliğini ayarlayın|Service Fabric, düğümden düğüme iletişim için bir birincil küme sertifikası kullanarak koruma (None, oturum ve EncryptAndSign) üç düzeyleri sağlar.  Tüm düğümler için iletileri şifrelenir ve dijital olarak imzalanmış emin olmak için koruma düzeyini ayarlayın. |
|İşlem kaynakları (hizmet veri yolu)|1|Service Bus ad alanındaki tüm yetkilendirme kurallarını RootManageSharedAccessKey dışında Kaldır |Hizmet veri yolu istemcileri tüm kuyrukları ve konuları ad alanında erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamanız gerekir. En az ayrıcalık güvenlik modeliyle uyum sağlamak için, yalnızca belirli varlığa erişim sağlamak üzere kuyruklar ve konular için varlık düzeyinde erişim ilkeleri oluşturmanız gerekir.|
|İşlem kaynakları (olay hub'ı)|1\.|RootManageSharedAccessKey hariç tüm yetkilendirme kuralları, Olay Hub 'ı ad alanından kaldırılmalıdır|Olay hub'ı istemcilerin tüm kuyrukları ve konuları ad alanında erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamanız gerekir. En az ayrıcalık güvenlik modeliyle uyum sağlamak için, yalnızca belirli varlığa erişim sağlamak üzere kuyruklar ve konular için varlık düzeyinde erişim ilkeleri oluşturmanız gerekir.|
|İşlem kaynakları (olay hub'ı)|5|Olay Hub 'ı varlığındaki yetkilendirme kuralları tanımlanmalıdır|Yetkilendirme kuralları en az ayrıcalıklı erişim vermek için olay hub'ı varlık üzerinde denetim.|
|Machine|50|Makinelerinize izleme aracısı yükleyin|Veri toplama, tarama, temel tarama ve her makineye bir uç nokta koruma güncelleştirmeleri etkinleştirmek için izleme aracısını yükleyin.|
|Makine|50|Otomatik sağlama ve Abonelikleriniz için veri toplamayı etkinleştir |Otomatik sağlama ve veri toplamayı etkinleştirmek için aboneliklerinizi tarama, temel tarama ve aboneliklerinize eklenen her makineye bir uç nokta koruma güncelleştirmeleri makineler için veri toplamayı etkinleştirin.|
|Makine|40|Makinelerinizin izleme aracısı sistem durumu sorunlarını çözün|Tam Güvenlik Merkezi koruma için sorun giderme Kılavuzu'ndaki yönergeleri takip ederek makinelerinizdeki izleme aracı sorunlarını çözün.| 
|Makine|40|Makinelerinizin uç nokta koruması sistem durumu sorunlarını çözün|Tüm Güvenlik Merkezi koruma için sorun giderme Kılavuzu'ndaki yönergeleri takip ederek makinelerinizi izleme aracı sorunları çözün.|
|Makine|40|Makinelerinizde eksik tarama verileri sorunlarını giderin|Sanal makineler ve bilgisayarlar üzerinde eksik tarama verileri sorunlarını giderin. Tarama, temel tarama ve uç nokta koruma çözümü tarama eksik güvenlik değerlendirmeleri gibi eksik, makineleri sonuçları üzerinde eksik tarama verileri güncelleştirin.|
|Makine|40|Sistem güncelleştirmelerinin makinelerinizde yüklü olması gerekir|Eksik sistem güvenliği ve güvenli Windows ve Linux sanal makineleri ve bilgisayarlar için kritik güncelleştirmeleri yükleyin
|Machine|15|Web uygulaması güvenlik duvarı ekleme| Web uygulamalarınızın güvenliğini sağlamak için bir web uygulaması Güvenlik Duvarı (WAF) çözümünü dağıtın. |
|Machine|40|Bulut hizmeti rolleriniz için işletim sistemi sürümünü güncelleştirin|Bulut hizmeti rolleriniz için işletim sistemi sürümünü, işletim sistemi ailenizdeki en yeni sürüm ile güncelleştirin.|
|Makine|35|Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir|Güvenlik Yapılandırması saldırılarına karşı korunacak makinelerinizde güvenlik açıklarını düzeltin.|
|Makine|35|Kapsayıcılarınızı güvenlik yapılandırması, güvenlik açıklarını düzeltin|Docker yüklü makineleri korumak için bunların güvenlik yapılandırmasındaki güvenlik açıklarını düzeltin.|
|Makine|25|Uyarlamalı Uygulama Denetimlerini etkinleştirin|Uygulamalar, Azure'da yer alan Vm'leriniz üzerinde çalışabilen uygulama denetime etkinleştirin. Bu, sanal makinelerinizi kötü amaçlı yazılımlara karşı sağlamlaştırma yardımcı olur. Güvenlik Merkezi, her sanal makinede çalışan uygulamaların çözümlemek için machine learning kullanır ve bu bilgileri kullanarak kuralları yardımcı olur, uygulamanızı sağlar. Bu özellik yapılandırma işlemi basitleştirir ve izin verme kurallarını uygulama koruma.|
|Makine|20|Makinelerinize uç nokta koruma çözümü yükleyin|Tehditleri ve güvenlik açıklarından korumak için sanal makineler, bir uç nokta koruma çözümüne yükleyin.|
|Makine|20|Sistem güncelleştirmelerini uygulamak için makinelerinizi yeniden başlatın|Sistem güncelleştirmelerini uygulamak ve makinenizi güvenlik açıklarına karşı korumak için makinelerinizi yeniden başlatın.|
|Machine|15|Disk şifrelemesi sanal makinelere uygulanmalıdır|Windows ve Linux sanal makineleri için Azure Disk şifrelemesi hem kullanılarak sanal makine disklerinizi şifreleyin. Azure Disk şifrelemesi (ADE) sektörde standart BitLocker özelliğini Windows ve Linux korumak ve verilerinizi koruyun ve kurumsal güvenlik ve uyumluluk karşılamanıza yardımcı olmak için işletim sistemi ve veri disk şifreleme sağlamak için DM-Crypt özelliğini kullanır. Taahhüt müşteri Azure anahtar kasası. Ne zaman, uyumluluk ve güvenlik gereksinimleri, kullanım Azure disk şifrelemesi kısa ömürlü (yerel olarak bağlı geçici) disk şifreleme gibi şifreleme anahtarlarınızı kullanarak uca verileri şifrelemek gerektirir. Alternatif olarak, Microsoft tarafından yönetilen anahtarları azure'da şifreleme anahtarları nerede Azure depolama hizmeti şifrelemesi kullanarak varsayılan olarak varsayılan olarak, yönetilen diskler bekleme durumundayken şifrelenir. Bu, uyumluluk ve güvenlik gereksinimlerini karşılıyorsa, gereksinimlerinizi karşılamak için varsayılan yönetilen disk şifrelemesi yararlanabilirsiniz.|
|Machine|30|Sanal makinelerinize bir güvenlik açığı değerlendirme çözümü yükleyin|Sanal makinelerinize bir güvenlik açığı değerlendirme çözümü yükleyin|
|Makine|15|Web uygulaması güvenlik duvarı ekleme| Web uygulamalarınızın güvenliğini sağlamak için bir web uygulaması Güvenlik Duvarı (WAF) çözümünü dağıtın. |
|Makine|30|Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir|Kendisi için bir güvenlik açığı değerlendirme 3 taraf çözümü dağıtılan sanal makinelerin sürekli olarak uygulama ve işletim sistemi güvenlik açıklarını karşı incelenen. Tür güvenlik açıklarına bulunduğunda, bunlar öneri bir parçası olarak daha fazla bilgi için kullanılabilir.|
|Machine|30|Sanal makinelerinize bir güvenlik açığı değerlendirme çözümü yükleyin|Sanal makinelerinize bir güvenlik açığı değerlendirme çözümü yükleyin|
|Machine|1\.|Sanal makinelerin yeni Azurerd kaynaklarına geçirilmesi gerekir|Sanal makineleriniz için Azure Resource Manager kullanarak: daha güçlü erişim denetimi (RBAC), daha iyi denetim, Kaynak Yöneticisi tabanlı dağıtım ve idare, yönetilen kimliklere erişim, gizli dizi için anahtar kasasına erişim, Daha kolay güvenlik yönetimi için Azure AD tabanlı kimlik doğrulaması ve Etiketler ve kaynak grupları desteği. |
|Machine|30|Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir|Kendisi için bir güvenlik açığı değerlendirme 3 taraf çözümü dağıtılan sanal makinelerin sürekli olarak uygulama ve işletim sistemi güvenlik açıklarını karşı incelenen. Tür güvenlik açıklarına bulunduğunda, bunlar öneri bir parçası olarak daha fazla bilgi için kullanılabilir.|
|Sanal makine ölçek kümesi |4|Sanal makine ölçek kümelerindeki tanılama günlükleri etkinleştirilmelidir|Günlükleri etkinleştirin ve bir yıla kadar saklayın. Bu, araştırma amaçları için etkinlik izlerini yeniden oluşturmayı sağlar. Bu, bir güvenlik olayı gerçekleştiğinde veya ağınızın güvenliği tehlikeye girerse yararlıdır.|
|Sanal makine ölçek kümesi|35|Sanal makine ölçek kümelerinizin güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir|Saldırılara karşı korumak için sanal makine ölçek kümelerinizin güvenlik yapılandırmasındaki güvenlik açıklarını düzeltin. |
|Sanal makine ölçek kümesi|5|Sanal makine ölçek kümelerinde Endpoint Protection sistem durumu başarısızlıklarını düzelt|Sanal makine ölçek kümelerinizi tehdit ve güvenlik açıklarına karşı korumak için Endpoint Protection sistem durumu başarısızlıklarını düzeltin. |
|Sanal makine ölçek kümesi|10|Endpoint Protection 'ın sanal makinelere yüklenmesi gerekir|Sanal makine ölçek kümelerinizi tehdit ve güvenlik açıklarına karşı korumak için bir uç nokta koruma çözümü yüklemek. |
|Sanal makine ölçek kümesi|40|Sanal makine ölçek kümelerindeki sistem güncelleştirmeleri yüklenmelidir|Windows ve Linux sanal makine ölçek kümelerinizin güvenliğini sağlamak için eksik sistem güvenliği ve kritik güncelleştirmeleri yükler. |
 





## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türü için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:


* [Azure Güvenlik Merkezi'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md)
* [Kimlik ve erişim Azure Güvenlik Merkezi'nde izleme](security-center-identity-access.md)
* [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)
* [Azure Güvenlik Merkezi'nde Azure SQL hizmetinizi koruma](security-center-sql-service-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.

