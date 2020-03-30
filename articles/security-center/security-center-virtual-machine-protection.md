---
title: Makinelerinizi ve uygulamalarınızı koruyun
description: Bu belge, Güvenlik Merkezi'nde sanal makinelerinizi ve bilgisayarlarınızı, web uygulamalarınızı ve Uygulama Hizmeti ortamlarınızı korumanıza yardımcı olan önerileri ele alır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/11/2020
ms.author: memildin
ms.openlocfilehash: bcf92838483fbb6b54802cc0d44cc44ea086d705
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282646"
---
# <a name="protect-your-machines-and-applications"></a>Makinelerinizi ve uygulamalarınızı koruyun
Azure Güvenlik Merkezi olası güvenlik açıklarını tanımladığında, kaynaklarınızı sertleştirmek ve korumak için gerekli denetimleri yapılandırma sürecinde size rehberlik eden öneriler oluşturur.

Bu makalede, Güvenlik Merkezi'nin kaynak güvenliği bölümünün **Bilgi İşlem ve Uygulamalar** sayfası açıklanmaktadır.

Bu sayfada görebileceğiniz önerilerin tam listesi için [Bkz. İşlem ve uygulama önerileri.](recommendations-reference.md#recs-computeapp)


## <a name="view-the-security-of-your-compute-and-apps-resources"></a>İşlem ve uygulama kaynaklarınızın güvenliğini görüntüleme

[![Güvenlik Merkezi panosu](./media/security-center-virtual-machine-recommendations/compute-and-apps-recs-overview.png)](./media/security-center-virtual-machine-recommendations/compute-and-apps-recs-overview.png#lightbox)

Güvenlik Merkezi'ndeki sol bölmeden işlem ve uygulama kaynaklarınızın durumunu görüntülemek **için, & Hesaplama uygulamalarını**seçin. Aşağıdaki sekmeler kullanılabilir:

* **Genel Bakış**: tüm bilgi işlem ve uygulama kaynaklarının yanı sıra mevcut güvenlik durumları için önerileri listeler 

* [**VM'leriniz ve Sunucular**](#vms-and-computers): Her birinin VM'leriniz, bilgisayarlarınız ve geçerli güvenlik durumunuz için önerileri listeler

* [**VM ölçek kümeleri:**](#vmscale-sets)ölçek kümeleriniz için önerileri listeler, 

* [**Bulut hizmetleri:**](#cloud-services)Güvenlik Merkezi tarafından izlenen web ve çalışan rolleriniz için önerileri listeler

* [**Uygulama hizmetleri:**](#app-services)Uygulama hizmet ortamlarınız için önerileri listeler ve her birinin mevcut güvenlik durumu

* [**Kapsayıcılar**](#containers): kapsayıcılarınız için önerileri listeler ve yapılandırmalarının güvenlik değerlendirmesi

* **Hesaplama kaynakları:** Service Fabric kümeleri ve Olay hub'ları gibi işlem kaynaklarınız için önerileri listeler

### <a name="whats-in-each-tab"></a>Her sekmede ne var?

Her sekmede birden çok bölüm vardır ve her bölümde, gösterilen öğe yle ilgili ek ayrıntıları görmek için ayrıntıya inebilirsiniz.

Her sekmede, izlenen ortamınızdaki ilgili kaynaklarla ilgili öneriler de görürsünüz. İlk sütunda öneri listeleniyor, ikincisi etkilenen toplam kaynak sayısını, üçüncüsü ise sorunun önem derecesini gösterir.

Her önerinin, seçtikten sonra gerçekleştirebileceğiniz bir dizi eylemi vardır. Örneğin, **Eksik sistem güncelleştirmelerini**seçerseniz, eksik olan VM ve bilgisayar sayısı ve eksik güncelleştirmenin önem derecesi görüntülenir.

> [!NOTE]
> Güvenlik önerileri **Öneriler** sayfasındakilerle aynıdır, ancak burada seçtiğiniz belirli kaynak türüne göre filtrelenirler. Önerilerin nasıl çözüleceği hakkında daha fazla bilgi için [bkz.](security-center-recommendations.md)
>




### <a name="vms-and-servers"></a><a name="vms-and-computers"></a>VM'ler ve Sunucular
VM'ler ve bilgisayarlar bölümü, VM'leriniz ve bilgisayarlarınız için tüm güvenlik önerilerine genel bir bakış sağlar. Dört tür makine dahildir:

![Azure dışı bilgisayar](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png) Azure olmayan bilgisayar.

![Azure Kaynak Yöneticisi VM](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png) Azure Kaynak Yöneticisi VM.

![Azure Klasik VM](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png) Azure Klasik VM.

![Çalışma alanından tanımlanan VM'ler](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png) VM'ler yalnızca görüntülenen abonelik parçası olan çalışma alanından tanımlanır. Buna, bu abonelikteki çalışma alanına rapor veren diğer aboneliklerden gelen VM'ler ve Operations Manager doğrudan aracısı ile yüklenen ve kaynak kimliği olmayan VM'ler dahildir.

Her öneri altında görünen simge, dikkat edilmesi gereken VM'yi ve bilgisayarı ve tavsiye türünü hızlı bir şekilde belirlemenize yardımcı olur. Ayrıca, kaynak **türüne** ve **Önem Derecesine**göre listeyi aramak için filtreleri de kullanabilirsiniz.

Her VM için güvenlik önerilerini detaylandırmak için VM'yi tıklatın.
Burada VM veya bilgisayarın güvenlik ayrıntılarını görürsünüz. En altta, önerilen eylemi ve her sorunun önem derecesini görebilirsiniz.

[![Bulut hizmetleri](./media/security-center-virtual-machine-recommendations/recommendation-list.png)](./media/security-center-virtual-machine-recommendations/recommendation-list.png#lightbox)




### <a name="virtual-machine-scale-sets"></a><a name="vmscale-sets"></a>Sanal makine ölçek setleri
Güvenlik Merkezi, ölçek kümeniz olup olmadığını otomatik olarak keşfeder ve microsoft izleme aracısını yüklemenizi önerir.

Microsoft İzleme Aracısını yüklemek için: 

1. Öneriyi seçin **İzleme aracısını sanal makine ölçeği kümesine yükleyin.** İzlenmeyen ölçek kümelerinin bir listesini alırsınız.

1. Sağlıksız bir ölçek kümesi seçin. Varolan bir kalabalık çalışma alanı kullanarak izleme aracısını yüklemek veya yeni bir çalışma alanı oluşturmak için yönergeleri izleyin. Ayarlı değilse çalışma alanı [fiyatlandırma katmanını](security-center-pricing.md) ayarladıktan emin olun.

   ![MMS'yi Yükleyin](./media/security-center-virtual-machine-recommendations/install-mms.png)

Microsoft İzleme Aracısını otomatik olarak yüklemek için yeni ölçek kümeleri ayarlamak için:
1. Azure İlkesi'ne gidin ve **Tanımlar'ı**tıklatın.

1. Windows sanal makine ölçek setleri için ilke **Dağıt Günlük Analizi aracısını** arayın ve üzerine tıklayın.

1. **Ata**'ya tıklayın.

1. **Kapsam** ve **Günlük Analizi çalışma alanını** ayarlayın ve **Ata'yı**tıklatın.

Microsoft İzleme Aracısı'nı Azure İlkesi'ne yüklenecek şekilde varolan tüm ölçek kümelerini ayarlamak istiyorsanız, **Düzeltme'ye** gidin ve varolan ilkeyi varolan ölçek kümelerine uygulayın.





### <a name="cloud-services"></a><a name="cloud-services"></a>Bulut hizmetleri
Bulut hizmetleri için, işletim sistemi sürümü güncel olmadığında bir öneri oluşturulur.

![Bulut hizmetleri](./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png)

Bir önerinizin olduğu bir senaryoda, işletim sistemini güncelleştirmek için önerideki adımları izleyin. Bir güncelleştirme kullanılabilir olduğunda, sorunun önem derecesine bağlı olarak bir uyarı (kırmızı veya turuncu) olacaktır. Bu önerinin tam açıklaması için, **DESCRIPTION** sütununun altındaki **Os sürümünü güncelleştir'i** tıklatın.






### <a name="app-services"></a><a name="app-services"></a>Uygulama hizmetleri
Uygulama Hizmeti bilgilerini görüntülemek için Security Center'ın Standart fiyatlandırma katmanında olması ve aboneliğinizde Uygulama Hizmeti'ni etkinleştirmeniz gerekir. Bu özelliği etkinleştirme yle ilgili talimatlar için Azure [Güvenlik Merkezi ile Uygulama Hizmetini Koru'ya](security-center-app-services.md)bakın.

**Uygulama hizmetleri**altında, Uygulama hizmet ortamlarınızın bir listesini ve Güvenlik Merkezi'nin gerçekleştirdiği değerlendirmeye dayalı sağlık özetini bulabilirsiniz.

![Uygulama hizmetleri](./media/security-center-virtual-machine-recommendations/app-services.png)

Gösterilen üç tür uygulama hizmeti vardır:

![Uygulama hizmetleri ortamı](./media/security-center-virtual-machine-recommendations/ase.png) Uygulama hizmetleri ortamı

![Web uygulaması](./media/security-center-virtual-machine-recommendations/web-app.png) Web uygulaması

![Fonksiyon uygulaması](./media/security-center-virtual-machine-recommendations/function-app.png) Fonksiyon uygulaması

Bir web uygulaması seçerseniz, özet görünümü üç sekmeyle açılır:

   - **Öneriler**: Güvenlik Merkezi tarafından yapılan ve başarısız olan değerlendirmelere dayanmaktadır.
   - **Geçirilen değerlendirmeler**: Güvenlik Merkezi tarafından gerçekleştirilen değerlendirmelerin listesi geçti.
   - **Kullanılamayan değerlendirmeler**: bir hata nedeniyle çalıştırılamayan veya öneribelirli Uygulama hizmetiyle ilgili olmayan değerlendirmeler listesi

   **Öneriler** altında seçilen web uygulaması ve her önerinin önem derecesi için önerilerin bir listesidir.

   ![Uygulama Hizmetleri önerileri](./media/security-center-virtual-machine-recommendations/app-services-rec.png)

Önerinin açıklamasını ve sağlıksız kaynakların, sağlıklı kaynakların ve taranmamış kaynakların listesini görmek için bir öneri seçin.

   - **Geçirilen değerlendirmeler** sütunu, geçirilen değerlendirmelerin bir listesini gösterir. Bu değerlendirmelerin şiddeti her zaman yeşildir.

   - Değerlendirmenin açıklaması, sağlıksız ve sağlıklı kaynakların listesi ve taranmamış kaynakların listesi için listeden geçirilen bir değerlendirme seçin. Sağlıksız kaynaklar için bir sekme vardır, ancak değerlendirme geçtiğinden beri bu liste her zaman boştur.





### <a name="containers"></a><a name="containers"></a>Kapsayıcılar

Ortamınıza bağlı olarak **Kapsayıcılar** sekmesini açtığınızda, üç kaynak türünden birini görebilirsiniz:

![Konteyner ana bilgisayar](./media/security-center-virtual-machine-recommendations/icon-container-host-rec.png) Konteyner ana bilgisayarları - Docker çalıştıran VM'ler 

![Kubernetes servisi](./media/security-center-virtual-machine-recommendations/icon-kubernetes-service-rec.png) Azure Kubernetes Service (AKS) kümeleri

![Kapsayıcı kayıt defteri](./media/security-center-virtual-machine-recommendations/icon-container-registry-rec.png) Azure Kapsayıcı Kayıt Defteri (ACR) kayıt defterleri - Yalnızca standart fiyatlandırma katmanında olduğunuzda ve Azure Konteyner Kayıt Defteri paketini etkinleştirdiğinizde gösterilir.

Kapsayıcı güvenlik özelliklerinin nasıl kullanılacağına ilişkin talimatlar için [bkz.](monitor-container-security.md)

Azure Konteyner Kayıt Defteri paketinin avantajları [burada](azure-container-registry-integration.md) açıklanmıştır

Kubernetes Hizmetleri paketinin faydaları [burada](azure-kubernetes-service-integration.md) açıklanmıştır

[![Kapsayıcılar sekmesi](./media/security-center-virtual-machine-recommendations/container-recommendations-all-types.png)](./media/security-center-virtual-machine-recommendations/container-recommendations-all-types.png#lightbox)

Listedeki belirli bir kaynak için önerileri görmek için bu kaynağı tıklatın.

#### <a name="visibility-into-container-registries"></a>Konteyner kayıtlarına görünürlük

Örneğin, yukarıdaki grafikte gösterilen listeden asc-demo ACR kayıt defterine tıkladığınızda bu ayrıntılar sayfasına yol açar:

[![Belirli bir ACR kayıt defteri için öneriler](./media/security-center-virtual-machine-recommendations/acr-registry-recs-list.png)](./media/security-center-virtual-machine-recommendations/acr-registry-recs-list.png#lightbox)


#### <a name="visibility-into-containers-hosted-on-iaas-linux-machines"></a>IaaS Linux makinelerinde barındırılan konteynerlere görünürlük

Docker çalıştıran VM'lerden birini tıklattığınızda, docker sürümü ve ana bilgisayarda çalışan görüntü sayısı gibi makinedeki kapsayıcılarla ilgili bilgilerin yer aldığı ayrıntılar sayfasını görürsünüz.

![VM çalışan docker için öneriler](./media/security-center-virtual-machine-recommendations/docker-recommendation.png)


#### <a name="security-recommendations-based-on-cis-benchmark-for-docker"></a>Docker için BDT kıyaslama dayalı güvenlik önerileri

Güvenlik Merkezi, Docker yapılandırmalarınızı tarar ve değerlendirilen başarısız kuralların listesini sunarak hatalı yapılandırmalar konusunda görünürlük sağlar. Güvenlik Merkezi, bu sorunları hızlı bir şekilde çözmenize ve zamandan tasarruf etmenizi sağlamak için yönergeler sağlar. Güvenlik Merkezi, Docker yapılandırmalarını sürekli değerlendirir ve son durumlar hakkında bilgi verir.

![konteyner sekmesi](./media/security-center-container-recommendations/container-cis-benchmark.png)


## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türleri için geçerli olan öneriler hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi'nin güvenlik önerilerinin tam başvuru listesi](recommendations-reference.md)
* [Azure Güvenlik Merkezi'nde kimliği ve erişimi izleme](security-center-identity-access.md)
* [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)
* [Azure SQL hizmetinizi Azure Güvenlik Merkezi'nde koruma](security-center-sql-service-recommendations.md)