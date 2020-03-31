---
title: Azure Hizmet Kumaşı kümesini yükseltme
description: Bir Azure Hizmet Dokusu kümesinin sürümünü veya yapılandırmasını yükseltme hakkında bilgi edinin- küme güncelleştirme modu ayarlama, sertifikaları yükseltme, uygulama bağlantı noktaları ekleme, işletim sistemi yamaları yapma ve yükseltmeler yapıldığında neler bekleyebileceğiniz hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 11/12/2018
ms.custom: sfrev
ms.openlocfilehash: 6897854820339fc78dd9083c82147dce95ab68b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258661"
---
# <a name="upgrading-and-updating-an-azure-service-fabric-cluster"></a>Azure Service Fabric kümesini yükseltme ve güncelleştirme

Herhangi bir modern sistem için, yükseltme için tasarım ürününüzün uzun vadeli başarı elde etmek için anahtardır. Azure Hizmet Kumaşı kümesi, sahip olduğunuz ancak kısmen Microsoft tarafından yönetilen bir kaynaktır. Bu makalede, neyin otomatik olarak yönetildiği ve kendinizi neleri yapılandırabileceğiniz açıklanmaktadır.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Kümenizde çalışan kumaş sürümünü denetleme

Kümenizin her zaman [desteklenen](service-fabric-versions.md)bir kumaş sürümünü çalıştırdığından emin olun. Microsoft, Service Fabric'in yeni bir sürümünü her duyurduğunda, önceki sürüm, bu tarihten itibaren en az 60 gün sonra desteğin sonu için işaretlenir. Yeni sürümler [Service Fabric takım blogunda](https://techcommunity.microsoft.com/t5/azure-service-fabric/bg-p/Service-Fabric)duyurulur.

Kümenizin çalıştırdığı sürümün sona ermesinden 14 gün önce, kümenizi uyarı durumu durumuna sokan bir sistem durumu oluşturulur. Desteklenen kumaş sürümüne yükseltilene kadar küme uyarı durumunda kalır.

Kümenizi, Microsoft tarafından yayımlandıkça otomatik kumaş yükseltmeleri alacak şekilde ayarlayabilir veya kümenizin üzerinde olmasını istediğiniz desteklenen kumaş sürümünü seçebilirsiniz.  Daha fazla bilgi için [kümenizin Service Fabric sürümünü yükseltin' i](service-fabric-cluster-upgrade-version-azure.md)okuyun.

## <a name="fabric-upgrade-behavior-during-automatic-upgrades"></a>Otomatik yükseltmeler sırasında kumaş yükseltme davranışı

Microsoft, Azure kümesinde çalışan kumaş kodunu ve yapılandırmasını korur. Yazılıma gerektiğinde otomatik olarak izlenen yükseltmeler gerçekleştiriyoruz. Bu yükseltmeler kod, yapılandırma veya her ikisi de olabilir. Uygulamanızın bu yükseltmeler nedeniyle herhangi bir etki veya en az etkiye maruz kalmadığından emin olmak için yükseltmeler aşağıdaki aşamalarda gerçekleştirilir:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Aşama 1: Yükseltme, tüm küme durumu ilkeleri kullanılarak gerçekleştirilir

Bu aşamada, yükseltmeler aynı anda bir yükseltme etki alanına devam eder ve kümede çalışan uygulamalar herhangi bir kapalı kalma süresi olmadan çalışmaya devam eder. Küme sistem durumu ilkeleri (düğüm durumu ve uygulama durumu için) yükseltme sırasında yapıştırılır.

Küme durumu ilkeleri karşılanmazsa, yükseltme geri alınır ve aboneliğin sahibine bir e-posta gönderilir. E-posta aşağıdaki bilgileri içerir:

* Bir küme yükseltmesini geri almamız gerektiğine dair bildirim.
* Varsa, önerilen düzeltici eylemler.
* Aşama 2'yi çalıştırana kadar gün sayısı (*n*).

Altyapı nedenleriyle herhangi bir yükseltmenin başarısız olması durumunda aynı yükseltmeyi birkaç kez daha yürütmeye çalışıyoruz. E-postanın gönderildiği tarihten itibaren *n* gün sonra, Aşama 2'ye devam ediyoruz.

Küme durumu ilkeleri karşılanırsa, yükseltme başarılı ve tamamlanmış olarak işaretlenir. Bu, ilk yükseltme sırasında veya bu aşamada yükseltme yeniden herhangi biri sırasında gerçekleşebilir. Başarılı bir çalıştırmanın e-posta onayı yoktur. Bu size çok fazla e-posta göndermemek için; bir e-posta alma normal bir istisna olarak görülmelidir. Küme yükseltmelerinin çoğunun uygulama kullanılabilirliğinizi etkilemeden başarılı olmasını bekliyoruz.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Aşama 2: Yükseltme yalnızca varsayılan sistem durumu ilkeleri kullanılarak gerçekleştirilir

Bu aşamadaki sistem durumu ilkeleri, yükseltme nin başlangıcında sağlıklı olan uygulama sayısıyükseltme işlemi süresince aynı kalacak şekilde ayarlanır. Aşama 1'de olduğu gibi, Aşama 2 yükseltmeleri aynı anda bir yükseltme etki alanına devam eder ve kümede çalışan uygulamalar herhangi bir kapalı kalma süresi olmadan çalışmaya devam eder. Küme sistem durumu ilkeleri (düğüm durumu ve kümede çalışan tüm uygulamaların sistem durumu birleşimi) yükseltme süresince yapıştırılır.

Yürürlükteki küme durumu ilkeleri karşılanmazsa, yükseltme geri alınır. Ardından aboneliğin sahibine bir e-posta gönderilir. E-posta aşağıdaki bilgileri içerir:

* Bir küme yükseltmesini geri almamız gerektiğine dair bildirim.
* Varsa, önerilen düzeltici eylemler.
* Aşama 3'ü çalıştırana kadar ki gün sayısı (n).

Altyapı nedenleriyle herhangi bir yükseltmenin başarısız olması durumunda aynı yükseltmeyi birkaç kez daha yürütmeye çalışıyoruz. N gün dolmadan birkaç gün önce bir hatırlatma e-postası gönderilir. E-postanın gönderildiği tarihten itibaren n gün sonra, Aşama 3'e devam ediyoruz. Faz 2'de size gönderdiğimiz e-postalar ciddiye alınmalı ve düzeltici önlemler alınmalıdır.

Küme durumu ilkeleri karşılanırsa, yükseltme başarılı ve tamamlanmış olarak işaretlenir. Bu, ilk yükseltme sırasında veya bu aşamada yükseltme yeniden herhangi biri sırasında gerçekleşebilir. Başarılı bir çalıştırmanın e-posta onayı yoktur.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Aşama 3: Yükseltme, agresif sistem durumu ilkeleri kullanılarak gerçekleştirilir

Bu aşamadaki bu sağlık politikaları, uygulamaların sağlığından çok yükseltmenin tamamlanmasına yöneliktir. Birkaç küme yükseltmeleri bu aşamada sona erer. Kümeniz bu aşamaya gelirse, uygulamanızın sağlıksız hale gelmesi ve/veya kullanılabilirliğini kaybetme olasılığı yüksektir.

Diğer iki aşamaya benzer şekilde, Aşama 3 yükseltmeleri bir defada bir yükseltme etki alanına devam eder.

Küme sistem durumu ilkeleri karşılanmazsa, yükseltme geri alınır. Altyapı nedenleriyle herhangi bir yükseltmenin başarısız olması durumunda aynı yükseltmeyi birkaç kez daha yürütmeye çalışıyoruz. Bundan sonra küme sabitlenir, böylece artık destek ve/veya yükseltme almaz.

Bu bilgileri içeren bir e-posta, düzeltici eylemlerle birlikte abonelik sahibine gönderilir. Herhangi bir kümenin Aşama 3'ün başarısız olduğu bir duruma girmesini beklemiyoruz.

Küme durumu ilkeleri karşılanırsa, yükseltme başarılı ve tamamlanmış olarak işaretlenir. Bu, ilk yükseltme sırasında veya bu aşamada yükseltme yeniden herhangi biri sırasında gerçekleşebilir. Başarılı bir çalıştırmanın e-posta onayı yoktur.

## <a name="manage-certificates"></a>Sertifikaları yönetme

Service Fabric, küme düğümleri ve kimlik doğrulama istemcileri arasındaki iletişimi güvenli hale getirmek için bir küme oluşturduğunuzda belirttiğiniz [X.509 sunucu sertifikalarını](service-fabric-cluster-security.md) kullanır. [Azure portalındaki](https://portal.azure.com) küme ve istemci için veya PowerShell/Azure CLI'yi kullanarak sertifika ekleyebilir, güncelleyebilir veya silebilirsiniz.  Daha fazla bilgi edinmek için [sertifika ekle veya kaldır'ı](service-fabric-cluster-security-update-certs-azure.md) okuyun

## <a name="open-application-ports"></a>Uygulama bağlantı noktalarını aç

Düğüm türüyle ilişkili Yük Dengeleyici kaynak özelliklerini değiştirerek uygulama bağlantı noktalarını değiştirebilirsiniz. Azure portalını kullanabilir veya PowerShell/Azure CLI'yi kullanabilirsiniz. Daha fazla bilgi için [küme için aç uygulama bağlantı noktalarını](create-load-balancer-rule.md)okuyun.

## <a name="define-node-properties"></a>Düğüm özelliklerini tanımlama

Bazen belirli iş yüklerinin kümedeki belirli düğüm türlerinde çalıştığından emin olmak isteyebilirsiniz. Örneğin, bazı iş yükleri GPU'lar veya SSD'ler gerektirebilirken, diğerleri gerekmeyebilir. Kümedeki düğüm türlerinin her biri için küme düğümlerine özel düğüm özellikleri ekleyebilirsiniz. Yerleşim kısıtlamaları, bir veya daha fazla düğüm özelliği için seçen tek tek hizmetlere eklenen deyimlerdir. Yerleşim kısıtlamaları, hizmetlerin nerede çalışması gerektiğini tanımlar.

Yerleşim kısıtlamaları, düğüm özellikleri ve bunları nasıl tanımlayacakayrıntıları için [düğüm özelliklerini ve yerleşim kısıtlamalarını](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)okuyun.

## <a name="add-capacity-metrics"></a>Kapasite ölçümleri ekleme

Düğüm türlerinin her biri için, yüklemeyi bildirmek için uygulamalarınızda kullanmak istediğiniz özel kapasite ölçümleri ekleyebilirsiniz. Yükü bildirmek için kapasite ölçümlerinin kullanımı yla ilgili ayrıntılar için, Kümenizi ve [Ölçümlerinizi](service-fabric-cluster-resource-manager-metrics.md)ve [Yüklemenizi Açıklayan](service-fabric-cluster-resource-manager-cluster-description.md) Hizmet Kumaş Kümesi Kaynak Yöneticisi Belgelerine bakın.

## <a name="set-health-policies-for-automatic-upgrades"></a>Otomatik yükseltmeler için sistem durumu ilkelerini ayarlama

Kumaş yükseltmesi için özel sistem durumu ilkeleri belirtebilirsiniz. Kümenizi Otomatik kumaş yükseltmelerine ayarladıysanız, bu ilkeler otomatik kumaş yükseltmelerinin Faz-1'ine uygulanır.
Kümenizi El ile kumaş yükseltmeleri için ayarladıysanız, bu ilkeler kümenizdeki kumaş yükseltmesini başlatmak için sistemi tetikleyen yeni bir sürümü her seçtiğinizde uygulanır. İlkeleri geçersiz kılmazsanız, varsayılanlar kullanılır.

Gelişmiş yükseltme ayarlarını seçerek özel sistem durumu ilkelerini belirtebilir veya "kumaş yükseltme" bıçağı altındaki geçerli ayarları gözden geçirebilirsiniz. Nasıl yapılacağını aşağıdaki resmi gözden geçirin.

![Özel sistem durumu ilkelerini yönetme][HealthPolices]

## <a name="customize-fabric-settings-for-your-cluster"></a>Kümeniz için Kumaş ayarlarını özelleştirin

Kümeve düğüm özelliklerinin güvenilirlik düzeyi gibi birçok farklı yapılandırma ayarı kümeüzerinde özelleştirilebilir. Daha fazla bilgi için [Service Fabric küme kumaş ayarlarını](service-fabric-cluster-fabric-settings.md)okuyun.

## <a name="patch-the-os-in-the-cluster-nodes"></a>Küme düğümlerinde işletim sistemi yama

Yama orkestrasyon uygulaması (POA), bir Servis Kumaşı kümesinde çalışma sistemi yamasını kesinti olmadan otomatikleştiren bir Service Fabric uygulamasıdır. [Windows için Patch Orchestration Application,](service-fabric-patch-orchestration-application.md) hizmetleri her zaman kullanılabilir tutarken yamaları düzenlenmiş bir şekilde yüklemek için kümenizde dağıtılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* Bazı hizmet kumaş küme [kumaş ayarlarını](service-fabric-cluster-fabric-settings.md) özelleştirmek öğrenin
* [Kümenizi nasıl ölçeklendirecek ve dışarı çıkarmayı](service-fabric-cluster-scale-up-down.md) öğrenin
* Uygulama [yükseltmeleri](service-fabric-application-upgrade.md) hakkında bilgi edinin

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
