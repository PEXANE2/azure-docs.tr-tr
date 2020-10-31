---
title: Hızlı başlangıç-Red Hat Enterprise Linux (RHEL) üzerinde Azure VM ve sanal makine ölçek kümesi ile Jpatron Enterprise uygulaması (EAP)
description: Azure RHEL VM ve sanal makine ölçek kümesi üzerinde Red Hat Jpatron EAP kullanarak kurumsal Java uygulamalarını dağıtma.
author: theresa-nguyen
ms.author: bicnguy
ms.topic: quickstart
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.assetid: 8a4df7bf-be49-4198-800e-db381cda98f5
ms.date: 10/30/2020
ms.openlocfilehash: d97148393d3158e38f9740d4a8f8e17dd04326d5
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93135455"
---
# <a name="deploy-enterprise-java-applications-to-azure-with-jboss-eap-on-red-hat-enterprise-linux"></a>Red Hat Enterprise Linux 'de Jpatron EAP ile Kurumsal Java uygulamalarını Azure 'a dağıtma

Bu hızlı başlangıç şablonları, Azure sanal makineleri (VM) ve sanal makine ölçek [kümeleri Ile](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux) [jpatron EAP](https://www.redhat.com/en/technologies/jboss-middleware/application-platform) dağıtma sürecini gösterir. Dağıtımı doğrulamak için dağıtımda örnek bir Java uygulamanız olacaktır. Jpatron EAP açık kaynaklı bir uygulama sunucusu platformudur. Java uygulamalarınız için kurumsal düzeyde güvenlik, ölçeklenebilirlik ve performans sunar. RHEL, açık kaynaklı bir işletim sistemi (OS) platformudur. Mevcut uygulamaların ölçeklendirilmesine ve tüm ortamlarda gelişen teknolojilerde kullanıma sunulmaya olanak tanır. Jpatron EAP ve RHEL her türlü ortamda kurumsal Java uygulamaları derlemek, çalıştırmak, dağıtmak ve yönetmek için gereken her şeyi içerir. Şirket içi, sanal ortamlar ve özel, genel ya da karma bulutları için harika bir açık kaynak çözümüdür.

## <a name="prerequisite"></a>Önkoşul 

* Etkin aboneliği olan bir Azure hesabı. Bir Azure aboneliği almak için, [Visual Studio aboneleri Için Azure kredilerinizi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirin veya [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/pricing/free-trial).

* Jpatron EAP yüklemesi-Jpatron EAP için Red Hat abonelik yönetimi (RHSM) yetkilendirimiyle Red Hat hesabına sahip olmanız gerekir. Bu yetkilendirme, Red Hat sınanmış ve sertifikalı Jpatron EAP sürümünü indirmenizi sağlayacak.  EAP yetkilendirmeleri yoksa, başlamadan önce bir [Jpatron EAP değerlendirme aboneliği](https://access.redhat.com/products/red-hat-jboss-enterprise-application-platform/evaluation) alın. Yeni bir Red Hat aboneliği oluşturmak için [Red Hat müşteri portalı](https://access.redhat.com/) ' na gidin ve bir hesap ayarlayın.
F
* [Azure Command-Line arabirimi](https://docs.microsoft.com/cli/azure/overview).

* RHEL seçenekleri-Kullandıkça Öde (PAYG) veya kendi aboneliğinizi getir (KCG) arasında seçim yapın. KCG sayesinde, hızlı başlangıç şablonunu dağıtılmadan önce [Red Hat bulut erişimi](https://access.redhat.com/) RHEL Gold görüntünüzü etkinleştirmeniz gerekir.

## <a name="java-ee--jakarata-ee-application-migration"></a>Java EE/Jakarata EE uygulama geçişi

### <a name="migrate-to-jboss-eap"></a>Jpatron EAP 'ye geçiş
Jpatron EAP 7,2 ve 7,3, Java Enterprise Edition (Java EE) 8 ve Jakarta EE 8 belirtimlerinin sertifikalı uygulamalarıdır. Jpatron EAP, yüksek kullanılabilirliğe sahip (HA) kümeleme, mesajlaşma ve dağıtılmış önbelleğe alma gibi özellikler için önceden yapılandırılmış seçenekler sağlar. Ayrıca, kullanıcıların Jpatron EAP 'nin sağladığı çeşitli API 'Leri ve Hizmetleri kullanarak uygulama yazmasına, dağıtmasına ve çalıştırmasına olanak tanır.  Jpatron EAP hakkında daha fazla bilgi için, [JPATRON eap 7,2 'ye giriş](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html-single/introduction_to_jboss_eap/index) veya [jpatron EAP 7,3](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/introduction_to_jboss_eap/index)' e giriş sayfasını ziyaret edin.

 #### <a name="applications-on-jboss-eap"></a>Jpatron EAP üzerinde uygulamalar

* Web Hizmetleri uygulamaları-Web Hizmetleri, farklı yazılım uygulamaları arasında standart bir çalışma yöntemi sağlar. Her uygulama, farklı platformlar ve çerçeveler üzerinde çalışabilir. Bu Web Hizmetleri, iç ve heterojen alt sistem iletişimini kolaylaştırır. Daha fazla bilgi edinmek için [eap 7,2 üzerinde Web Hizmetleri uygulamaları geliştirme](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/developing_web_services_applications/index) veya [EAP 7,3 üzerinde Web Hizmetleri uygulamaları](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/developing_web_services_applications/index)geliştirme makalesini ziyaret edin.

* Enterprise Java Beans (EJB) uygulamaları-EJB 3,2, dağıtılmış, işlemsel, güvenli ve taşınabilir Java EE ve Jakarta EE uygulamaları geliştirmeye yönelik bir API 'dir. EJB, bir uygulamanın iş mantığını yeniden kullanımını teşvik eden bir şekilde uygulamak için kurumsal Beans adlı sunucu tarafı bileşenlerini kullanır. Daha fazla bilgi edinmek için [eap 7,2 ÜZERINDE EJB uygulamaları geliştirme](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/developing_ejb_applications/index) veya [EAP 7,3 üzerinde EJB uygulamaları geliştirme](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/developing_ejb_applications/index)makalesini ziyaret edin.

* Hazırda Beklet uygulamaları-geliştiriciler ve Yöneticiler, Jpatron EAP ile Java Kalıcılık API (JPA)/hazırda beklet uygulamaları geliştirebilir ve dağıtabilir. Hazırda bekletme çekirdeği, Java dili için nesne ilişkisel bir eşleme çerçevesidir. Nesne yönelimli bir etki alanı modelini ilişkisel bir veritabanına eşlemek için bir çerçeve sağlar ve uygulamaların veritabanıyla doğrudan etkileşime engel olmasını sağlar. Hazırda Beklet varlık Yöneticisi, [JPA 2,1 belirtimi](https://www.jcp.org/en/jsr/overview)tarafından tanımlanan programlama arabirimlerini ve yaşam döngüsü kurallarını uygular. Bu sarmalayıcı, hazırda bekleme ek açıklamalarıyla birlikte, çok sayıda hazırda bekleme moduna sahip bir tamamen (ve tek başına) JPA çözümü uygular. Hazırda bekletme hakkında daha fazla bilgi edinmek için EAP 7,2 ' de [JPA 'yi](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/development_guide/java_persistence_api) veya  [EAP 7,3 üzerinde Cakarta kalıcılığı](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/development_guide/java_persistence_api)ziyaret edin

#### <a name="red-hat-migration-toolkit-for-applications-mta"></a>Uygulamalar için Red Hat geçiş araç seti (MTA)
[Uygulamalar Için Red Hat geçiş araç seti (MTA)](https://developers.redhat.com/products/mta/overview) , Java uygulama sunucuları için bir geçiş aracıdır. Başka bir uygulama sunucusundan Jpatron EAP 'ye geçiş yapmak için bu aracı kullanın. Bu, Java için IDE eklentileri [IDE](https://www.eclipse.org/ide/), [Red Hat CodeReady çalışma alanları](https://developers.redhat.com/products/codeready-workspaces/overview)ve [Visual Studio Code (vs Code)](https://code.visualstudio.com/docs/languages/java) ile birlikte kullanılabilir.  MTA, ücretsiz ve açık kaynaklı bir araçtır:
* Uygulama analizini otomatikleştirin
* Destek efor tahmini
* Kod geçişini hızlandırma
* Kapsayıcılama desteği
* Azure Iş yükü Oluşturucu ile tümleşir

### <a name="migrate-jboss-eap-from-on-premises-to-azure"></a>Jpatron EAP 'yi Şirket içinden Azure 'a geçirme
RHEL 'de Jpatron EAP 'nin Azure Marketi teklifi, Azure VM 'lerinde 20 dakikadan kısa bir süre içinde yüklenir ve temin edilir. Bu tekliflere [Azure Marketi](https://azuremarketplace.microsoft.com/) 'nden erişebilirsiniz

Bu market teklifi, gereksinimlerinizi desteklemek için çeşitli EAP ve RHEL sürümlerinin birleşimlerini içerir. Jpatron EAP her zaman kendi aboneliğinizi getir (BYOS), ancak RHEL OS için KCG veya Kullandıkça Öde (PAYG) arasında seçim yapabilirsiniz. Azure Marketi teklifi, RHEL 'de tek başına veya kümelenmiş VM 'Ler olarak Jpatron EAP için plan seçeneklerini içerir:

* Jpatron EAP 7,2 RHEL 7,7 VM (PAYG)
* Jpatron EAP 7,2 RHEL 8,0 VM (PAYG)
* Jpatron EAP 7,3 RHEL 8,0 VM (PAYG)
* Jpatron EAP 7,2 RHEL 7,7 VM (BYOS)
* Jpatron EAP 7,2 RHEL 8,0 VM (BYOS)
* Jpatron EAP 7,3 RHEL 8,0 VM (BYOS)

Azure Market teklifleriyle birlikte, Azure geçiş yolculuğuna başlamanıza olanak sağlayan hızlı başlangıç şablonları mevcuttur. Bu hızlı başlangıç, çeşitli yapılandırmalarda ve sürüm birleşimlerinde Jpatron EAP 'yi RHEL üzerinde dağıtmak için önceden oluşturulmuş Azure Resource Manager (ARM) şablonları ve betiği içerir. Şunları yapmanız gerekir:

* Load Balancer (LB)
* Yük Dengeleme ve VM 'Ler için özel IP
* Tek bir alt ağa sahip sanal ağ (VNET)
* VM yapılandırması (küme veya tek başına)
* Örnek bir Java uygulaması

Bu şablonlar için çözüm mimarisi şunları içerir:

* Tek başına RHEL VM 'de Jpatron EAP
* Birden çok RHEL VM arasında kümelenmiş bir Jpatron EAP
* Azure sanal makine ölçek kümesini kullanan Jpatron EAP kümelenmiş

#### <a name="linux-workload-migration-for-jboss-eap"></a>Jpatron EAP için Linux Iş yükü geçişi
Azure Iş yükü Oluşturucu, şirket içi Java uygulamalarına yönelik kavram kanıtı (POC), değerlendirme ve geçiş sürecini Azure 'a basitleştirir. Iş yükü Oluşturucu, Jpatron EAP sunucularını belirlemek için Azure geçişi bulma aracı ile tümleşir. Daha sonra, Jpatron EAP sunucusu dağıtımı için dinamik olarak erişilebilir bir PlayBook oluşturur. Diğer uygulama sunucularından Jpatron EAP 'ye sunucu geçirmek için Red Hat MTA aracından yararlanır. Geçişi basitleştirecek adımlar şunlardır:
* Değerlendirme-Azure VM veya sanal makine ölçek kümesi kullanan Jpatron EAP kümeleri
* Değerlendirme-uygulamaları ve altyapıyı tarar
* Altyapı yapılandırması-iş yükü profili oluşturur
* Dağıtım ve test-iş yükünü dağıtma, geçirme ve test etme
* Dağıtım sonrası yapılandırması-veriler, izleme, güvenlik, yedekleme ve daha fazlası ile tümleşir

## <a name="server-configuration-choice"></a>Sunucu yapılandırma seçimi

RHEL VM dağıtımı için, PAYG ya da KCG arasında seçim yapabilirsiniz. [Azure Marketi](https://azuremarketplace.microsoft.com) 'ndeki görüntülerin varsayılan değeri PAYG olur. Kendi RHEL işletim sistemi görüntünüz varsa, bir KCG türü RHEL VM dağıtın. VM 'Leri veya sanal makine ölçek kümesini dağıtmadan önce RHSM hesabınızın F bulut erişimi aracılığıyla KCG yetkilendirmelerine sahip olduğundan emin olun.

Jpatron EAP güçlü yönetim özelliklerine sahiptir ve uygulamalarına işlevsellik ve API 'Ler sağlar. Bu yönetim özellikleri, Jpatron EAP 'yi başlatmak için kullanılan işletim moduna göre farklılık gösterir. RHEL ve Windows Server 'da desteklenir. Jpatron EAP, ayrık örnekleri yönetmek için tek başına sunucu işletim modu sunar. Ayrıca, tek bir denetim noktasından örnek gruplarını yönetmek için yönetilen bir etki alanı işletim modu sağlar. Note: yüksek kullanılabilirlik (HA) özelliği Azure altyapı hizmetleri tarafından yönetildiğinden bu yana, Jpatron EAP tarafından yönetilen etki alanları Microsoft Azure desteklenmez. *EAP_HOME* adlı ortam değişkeni, JPATRON EAP yüklemesinin yolunu belirtmek için kullanılır.

**Jpatron EAP 'Yi tek başına sunucu olarak başlatma** -aşağıdaki komut, EAP hizmetini tek başına modda nasıl başlataöğreneceksiniz.

```
$EAP_HOME/bin/standalone.sh
```
    
Bu başlangıç betiği, JVM seçenekleri gibi bazı varsayılan tercihleri ayarlamak için EAP_HOME/bin/standalone.conf dosyasını kullanır. Ayarlar bu dosyada özelleştirilebilir. Jpatron EAP, tek başına modu varsayılan olarak başlatmak için standalone.xml yapılandırma dosyasını kullanır, ancak farklı bir mod kullanılarak başlatılabilir. Kullanılabilir tek başına yapılandırma dosyaları ve bunların nasıl kullanılacağı hakkında ayrıntılar için, EAP [7,2 Için tek başına sunucu yapılandırma dosyalarına](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/configuration_guide/jboss_eap_management#standalone_server_configuration_files) veya [EAP 7,3 Için tek başına sunucu yapılandırma dosyalarına](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/configuration_guide/jboss_eap_management#standalone_server_configuration_files) bakın. Jpatron EAP 'yi farklı bir yapılandırmayla başlatmak için--Server-config bağımsız değişkenini kullanın. Örneğin:
    
 ```
 $EAP_HOME/bin/standalone.sh --server-config=standalone-full.xml
 ```
    
Tüm kullanılabilir başlangıç betiği bağımsız değişkenlerinin tam listesi ve bunların amaçları için,--Help bağımsız değişkenini kullanın veya EAP [7,2 ' deki sunucu çalışma](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/configuration_guide/reference_material#reference_of_switches_and_arguments_to_pass_at_server_runtime) zamanı bağımsız DEĞIŞKENLERI veya [EAP 7,3 bölümünde sunucu çalışma zamanı](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/configuration_guide/reference_material#reference_of_switches_and_arguments_to_pass_at_server_runtime) bağımsız değişkenlerini görüntüleyin.
    
Jpatron EAP, küme modunda da çalışabilir. Daha fazla bilgi edinmek için EAP 7,2 veya [kümelere 7,3 genel](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/configuring_messaging/clusters_overview) bakışa [göre kümelere genel bakış](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/configuring_messaging/clusters_overview) konusuna bakın. Jpatron EAP küme mesajlaşma işlemi, Jpatron EAP ileti sunucularının gruplanarak ileti işleme yükünü paylaşmasına izin verir. Kümedeki her etkin düğüm, kendi iletilerini yöneten ve kendi bağlantılarını işleyen etkin bir Jpatron EAP mesajlaşma sunucusudur.

## <a name="support-and-subscription-notes"></a>Destek ve abonelik notları
Bu hızlı başlangıç şablonları şu şekilde sunulmaktadır: 

- RHEL OS, Red Hat Gold görüntü modeli aracılığıyla PAYG veya KCG olarak sunulur
- Jpatron EAP yalnızca BYOS olarak sunulur

#### <a name="using-rhel-os-with-payg-model"></a>RHEL işletim sistemini PAYG modeliyle kullanma

Varsayılan olarak, bu hızlı başlangıç şablonları Azure galerisinden Isteğe bağlı RHEL 7,7 veya 8,0 PAYG görüntüsünü kullanır. PAYG görüntülerinin normal işlem, ağ ve depolama maliyetlerinin üzerine ek saatlik RHEL abonelik ücreti alınacaktır. Aynı zamanda, örnek Red Hat aboneliğinize kaydedilir. Bu, yetkilendirmelerinizin birini kullandığınız anlamına gelir. Bu PAYG görüntüsü "çift Faturalandırma" ile sonuçlanacaktır. Kendi RHEL görüntünüzü oluşturarak bu sorundan kaçınabilirsiniz. Daha fazla bilgi edinmek için bu Red Hat Bilgi Bankası (KB) makalesini okuyun [Microsoft Azure için BIR RHEL VM sağlama](https://access.redhat.com/articles/uploading-rhel-image-to-azure). Veya [Red Hat bulut erişimi](https://access.redhat.com/) RHEL Gold görüntünüzü etkinleştirin.

PAYG VM fiyatlandırmasıyla ilgili ayrıntılar için [Red Hat Enterprise Linux fiyatlandırmaya](https://azure.microsoft.com/pricing/details/virtual-machines/red-hat/) göz atın. PAYG modelinde RHEL kullanmak için, [rhel 7,7 Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/RedHat.RedHatEnterpriseLinux77-ARM) veya [rhel 8,0 Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/RedHat.RedHatEnterpriseLinux80-ARM)için belirtilen ödeme yöntemiyle bir Azure aboneliğine sahip olmanız gerekir. Bu teklifler, Azure aboneliğinde bir ödeme yönteminin belirtilmesini gerektirir.

#### <a name="using-rhel-os-with-byos-model"></a>Kyos modeliyle RHEL OS kullanma

RHEL işletim sistemi için KCG 'yi kullanmak için Azure 'da RHEL OS kullanma yetkilendirmelerine sahip geçerli bir Red Hat aboneliğine sahip olmanız gerekir. Bu hızlı başlangıç şablonunu dağıtmadan önce aşağıdaki önkoşulları doldurun:

1. Red Hat aboneliğinize bağlı RHEL OS ve Jpatron EAP yetkilendirmelerine sahip olduğunuzdan emin olun.
2. RHEL BYOS görüntülerini kullanmak için Azure abonelik KIMLIĞINIZI yetkilendirin. Aşağıdaki adımları içeren işlemi gerçekleştirmek için [Red Hat abonelik yönetimi (RHSM) belgelerini](https://access.redhat.com/documentation/en/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/con-enable-subs) izleyin:

    2,1 Red Hat bulut erişim panonuzda bir sağlayıcı olarak Microsoft Azure etkinleştirin.

    2,2 Azure abonelik kimliklerinizi ekleyin.

    2,3 Microsoft Azure bulut erişimi için yeni ürünleri etkinleştirin.
    
    2,4 Azure aboneliğiniz için Red Hat Gold görüntülerini etkinleştirin. Daha fazla bilgi için, daha fazla ayrıntı için [abonelik erişimi aboneliklerini etkinleştirme ve sürdürme](https://access.redhat.com/documentation/en/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/using_red_hat_gold_images#con-gold-image-azure) bölümünü okuyun.

    2,5, Azure aboneliğinizde Red Hat Gold görüntülerinin kullanılabilir olmasını bekleyin. Bu altın görüntüler genellikle 3 saat içinde kullanılabilir.
    
3. RHEL BYOS görüntüleri için Azure Marketi hüküm ve koşullarını kabul edin. Aşağıda verilen şekilde Azure Command-Line Interface (CLı) komutlarını çalıştırarak bu işlemi tamamlayabilirsiniz. Daha fazla bilgi için bkz. [Azure 'Daki RHEL BYOS Gold resimleri](https://docs.microsoft.com/azure/virtual-machines/workloads/redhat/byos) daha fazla ayrıntı için. En son Azure CLı sürümünü çalıştırıyor olmanız önemlidir.

    3,1 bir Azure CLı oturumu başlatın ve Azure hesabınızla kimlik doğrulaması yapın. Yardım almak için [Azure CLI Ile oturum açma](https://docs.microsoft.com/cli/azure/authenticate-azure-cli) bölümüne bakın. Taşımadan önce en son Azure CLı sürümünü çalıştırdığınızdan emin olun.

    3,2 aşağıdaki CLı komutunu çalıştırarak, aboneliğinizde RHEL BYOS görüntülerinin kullanılabildiğini doğrulayın. Burada herhangi bir sonuç alamazsanız, #2 başvurun ve Azure aboneliğinizin RHEL BYOS görüntüleri için etkinleştirildiğinden emin olun.
   
   ```
   az vm image list --offer rhel-byos --all
   ```

    3,3, sırasıyla RHEL 7,7 BYOS ve RHEL 8,0 BYOS için Market şartlarını kabul etmek üzere aşağıdaki komutu çalıştırın.
   ```
   az vm image terms accept --publisher redhat --offer rhel-byos --plan rhel-lvm77
   ``` 

   ```
   az vm image terms accept --publisher redhat --offer rhel-byos --plan rhel-lvm8
   ``` 
   
4. Aboneliğiniz artık Azure sanal makinelerinde RHEL 7,7 veya 8,0 KCG 'yi dağıtmaya hazırdır.

#### <a name="using-jboss-eap-with-byos-model"></a>Kyos modeliyle Jpatron EAP kullanma

Jpatron EAP yalnızca KCG modeli aracılığıyla Azure 'da kullanılabilir. Bu şablonu dağıttığınızda, geçerli EAP yetkilendirmelerine sahip RHSM havuz KIMLIĞI ile birlikte RHSM kimlik bilgilerinizi sağlamanız gerekir. EAP yetkilendirmeleri yoksa, başlamadan önce bir [Jpatron EAP değerlendirme aboneliği](https://access.redhat.com/products/red-hat-jboss-enterprise-application-platform/evaluation) alın.

## <a name="how-to-consume"></a>Nasıl tüketiliyor

Şablonu aşağıdaki üç şekilde dağıtabilirsiniz:

- Aşağıdaki komutları çalıştırarak PowerShell 'i kullanın-şablonu dağıtın: (Azure PowerShell yükleme ve yapılandırma hakkında bilgi için [Azure PowerShell](https://docs.microsoft.com/powershell/azure/) kullanıma alma).

  ```
  New-AzResourceGroup -Name <resource-group-name> -Location <resource-group-location> #use this command when you need to create a new resource group for your deployment
  ```

  ```
  New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateUri <raw link to the template which can be obtained from github>
  ```
    
- Azure CLı 'yi kullanma-aşağıdaki komutları çalıştırarak şablonu dağıtın: (Azure CLı 'yi yükleme ve yapılandırma hakkında ayrıntılı bilgi için bkz. Azure [CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) 'Yi gözden geçirin).

  ```
  az group create --name <resource-group-name> --location <resource-group-location> #use this command when you need to create a new resource group for your deployment
  ```

  ```
  az group deployment create --resource-group <my-resource-group> --template-uri <raw link to the template which can be obtained from github>
  ```

- Azure portal kullanma-aşağıdaki bölümde belirtildiği gibi *Azure hızlı başlangıç şablonlarına* giderek Azure Portal dağıtabilirsiniz. Hızlı başlangıçta, **Azure 'A dağıt** ' a tıklayın veya GitHub ' a **gidin** .

## <a name="azure-quickstart-templates"></a>Azure hızlı başlangıç şablonları

Jpatron EAP 'nin hızlı başlangıç şablonlarından biriyle, dağıtım hedefiniz ile eşleşen RHEL birleşimini kullanabilirsiniz. Kullanılabilir hızlı başlangıç şablonlarının listesi aşağıda verilmiştir.

* <a href="https://azure.microsoft.com/resources/templates/jboss-eap-standalone-rhel/"> RHEL üzerinde JPATRON EAP (tek BAŞıNA VM)</a> -bu, Azure 'da jpatron-EAP adlı bir Web uygulamasını rhel 7,7 veya 8,0 VM üzerinde çalışan jpatron EAP 7,2 veya 7,3 olarak dağıtır.

* <a href="https://azure.microsoft.com/resources/templates/jboss-eap-clustered-multivm-rhel/"> RHEL 'de Jpatron EAP (kümelenmiş, çoklu VM 'ler)</a> -bu, ' n ' numarası rhel 7,7 veya 8,0 VM 'Lerde çalışan jpatron EAP 7,2 veya 7,3 kümesinde EAP-Session-Replication adlı bir Web uygulaması dağıtır. ' N ' değeri Kullanıcı tarafından karar verdi. Tüm VM 'Ler bir Load Balancer arka uç havuzuna eklenir.

* <a href="https://azure.microsoft.com/en-us/resources/templates/jboss-eap-clustered-vmss-rhel/"> RHEL 'de Jpatron EAP (kümelenmiş, sanal makine ölçek kümesi)</a> -bu, rhel 7,7 veya 8,0 sanal makine ölçek kümesi örnekleri üzerinde çalışan jpatron EAP 7,2 veya 7,3 kümesinde EAP-Session-Replication adlı bir Web uygulaması dağıtır.

## <a name="resource-links"></a>Kaynak bağlantıları:

* [Azure hibrit avantajları](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing)
* [Azure App Service için bir Java uygulaması yapılandırma](https://docs.microsoft.com/azure/app-service/configure-language-java)
* [Azure Red Hat OpenShift üzerinde Jpatron EAP](https://azure.microsoft.com/services/openshift/)
* [Azure App Service Linux üzerinde Jpatron EAP](https://docs.microsoft.com/azure/app-service/quickstart-java) Hızlı başlangıç
* [Jpatron EAP 'yi Azure App Service](https://github.com/JasonFreeberg/jboss-on-app-service) öğreticisine dağıtma

## <a name="next-steps"></a>Sonraki adımlar

* [Jpatron EAP 7,2](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.2/) hakkında daha fazla bilgi edinin
* [Jpatron EAP 7,3](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.3/) hakkında daha fazla bilgi edinin
* [Red Hat abonelik yönetimi](https://access.redhat.com/products/red-hat-subscription-management) hakkında daha fazla bilgi edinin
* [Azure 'Da Red Hat](https://aka.ms/rhel-docs) için Microsoft docs
* [Azure Marketi 'nden RHEL VM/sanal makine ölçek kümesi üzerinde Jpatron EAP](https://aka.ms/AMP-JBoss-EAP) dağıtma
* [Azure hızlı başlangıcı 'ndan RHEL VM/sanal makine ölçek kümesi üzerinde Jpatron EAP](https://aka.ms/Quickstart-JBoss-EAP) dağıtma
