---
title: Bulut Dökümhanesi Azure ile Nasıl Bütünleşir | Microsoft Dokümanlar
description: Cloud Foundry'nin Kurumsal deneyimi geliştirmek için Azure hizmetlerini nasıl kullanabileceğini açıklar
services: virtual-machines-linux
documentationcenter: ''
author: ningk
tags: Cloud-Foundry
ms.assetid: 00c76c49-3738-494b-b70d-344d8efc0853
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2018
ms.author: ningk
ms.openlocfilehash: 04ef72f7ec70b370305395ae8de8180f4594b43b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277337"
---
# <a name="integrate-cloud-foundry-with-azure"></a>Cloud Foundry ile Azure’ı tümleştirme

[Cloud Foundry,](https://docs.cloudfoundry.org/) bulut sağlayıcılarının IaaS platformunun üzerinde çalışan bir PaaS platformudur. Bulut sağlayıcıları arasında tutarlı uygulama dağıtım deneyimi sunar. Ayrıca, kurumsal sınıf HA, ölçeklenebilirlik ve maliyet tasarrufu ile çeşitli Azure hizmetleriyle de entegre olabilir.
Yönlendirme, Kimlik Doğrulama, Uygulama yaşam döngüsü yönetimi, Hizmet yönetimi, Mesajlaşma ve İzleme: [Cloud Foundry,](https://docs.cloudfoundry.org/concepts/architecture/)esnek ölçekli online olabilir 6 alt sistemleri vardır. Alt sistemlerin her biri için, Bulut Dökümhane'yi muhabir Azure hizmetini kullanacak şekilde yapılandırabilirsiniz. 

![Azure Tümleştirme Mimarisinde Bulut Dökümhanesi](media/CFOnAzureEcosystem-colored.png)

## <a name="1-high-availability-and-scalability"></a>1. Yüksek Kullanılabilirlik ve Ölçeklenebilirlik
### <a name="managed-disk"></a>Yönetilen Disk
Bosh, disk oluşturma ve yordamları silen için Azure CPI (Bulut Sağlayıcı Arabirimi) kullanır. Varsayılan olarak, yönetilmeyen diskler kullanılır. Müşterinin depolama hesaplarını el ile oluşturmasını ve sonra hesapları CF bildirim dosyalarında yapılandırmasını gerektirir. Bunun nedeni, depolama hesabı başına disk sayısının sınırlandırılmasıdır.
Şimdi [Yönetilen Disk](https://azure.microsoft.com/services/managed-disks/) kullanılabilir, sanal makineler için yönetilen güvenli ve güvenilir disk depolama sunuyor. Müşteri artık ölçek ve HA için depolama hesabı ile uğraşmak gerekir. Azure diskleri otomatik olarak düzenler. İster yeni ister varolan bir dağıtım olsun, Azure CPI, bir CF dağıtımı sırasında yönetilen diskin oluşturulmasını veya geçişini işler. PCF 1.11 ile desteklenir. Ayrıca, başvuru için açık kaynak Cloud Foundry [Yönetilen Disk kılavuzunu](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/managed-disks) da keşfedebilirsiniz. 
### <a name="availability-zone-"></a>Kullanılabilirlik Bölgesi *
Bulut tabanlı bir uygulama platformu olarak Cloud Foundry dört [yüksek kullanılabilirlik düzeyiyle](https://docs.pivotal.io/pivotalcf/2-1/concepts/high-availability.html)tasarlanmıştır. Yazılım hatalarının ilk üç düzeyi CF sisteminin kendisi tarafından ele alınabilirken, platform hata toleransı bulut sağlayıcıları tarafından sağlanır. Önemli CF bileşenleri, bir bulut sağlayıcısının platformu HA çözümü ile korunmalıdır. Bu GoRouters, Diego Brains, CF veritabanı ve hizmet karoları içerir. Varsayılan olarak, [Azure Kullanılabilirlik Kümesi,](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/deploy-cloudfoundry-with-availability-sets) veri merkezindeki kümeler arasında hata toleransı için kullanılır.
İyi olan yeni, [Azure Kullanılabilirlik Bölgesi](https://docs.microsoft.com/azure/availability-zones/az-overview ) şimdi piyasaya sürülerek hata toleransını veri merkezleri arasında düşük gecikme gecikmesi artıklığıyla bir sonraki seviyeye getiriyor.
Azure Kullanılabilirlik Bölgesi, 2+ veri merkezine bir vm kümesi yerleştirerek HA'ya ulaşır ve her bir VM kümesi diğer kümelere göre gereksizdir. Eğer bir Bölge aşağıdaysa, diğer kümeler hala canlı, felaketten izole edilmiş.
> [!NOTE] 
> Azure Kullanılabilirlik Bölgesi henüz tüm bölgelere sunulmadı, desteklenen bölgelerin listesi için en son [duyuruyu](https://docs.microsoft.com/azure/availability-zones/az-overview)kontrol edin. Açık Kaynak Bulut Dökümhanesi [için, açık kaynak Bulut Dökümhanesi kılavuzu için Azure Kullanılabilirlik Bölgesi'ni](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/availability-zone)denetleyin.

## <a name="2-network-routing"></a>2. Ağ Yönlendirme
Varsayılan olarak, Azure temel yük dengeleyicisi gelen CF API/uygulama istekleri için kullanılır ve bunları Gorouters'a iletir. Diego Brain, MySQL, ERT gibi CF bileşenleri de HA için trafik dengelemek için yük dengeleyici kullanabilirsiniz. Azure ayrıca tam olarak yönetilen bir dizi yük dengeleme çözümü de sağlar. TLS sonlandırma ("SSL boşaltma") veya HTTP/HTTPS istek uygulama katmanı işleme başına arıyorsanız, Uygulama Ağ Geçidi'ni düşünün. Katman 4'te yüksek kullanılabilirlik ve ölçeklenebilirlik yükü dengelemesi için standart yük dengeleyiciyi göz önünde bulundurun.
### <a name="azure-application-gateway-"></a>Azure Uygulama Ağ Geçidi *
[Azure Application Gateway,](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) SSL boşaltma, uçtan uca SSL, Web Application Güvenlik Duvarı, çerez tabanlı oturum yakınlığı ve daha fazlası dahil olmak üzere çeşitli katman 7 yük dengeleme özellikleri sunar. Açık [Kaynak Bulut Dökümhanesi'nde Uygulama Ağ Geçidi'ni yapılandırabilirsiniz.](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/application-gateway) PCF için, POC testi için [PCF 2.1 sürüm notlarını](https://docs.pivotal.io/pivotalcf/2-1/pcf-release-notes/opsmanager-rn.html#azure-application-gateway) kontrol edin.

### <a name="azure-standard-load-balancer-"></a>Azure Standart Yük Dengeleyicisi *
Azure Yük Dengeleyici, Katman 4 yük dengeleyicidir. Trafiği yük dengeli bir kümedeki hizmetler arasında dağıtmak için kullanılır. Standart sürüm, temel sürümün üstünde [gelişmiş özellikler](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) sağlar. Örneğin 1. Arka uç havuzu maksimum limiti 100'den 1000 VM'ye yükseltilir.  2. Uç noktalar artık tek kullanılabilirlik kümesi yerine birden çok kullanılabilirlik kümesini destekler.  3. HA bağlantı noktaları, daha zengin izleme verileri ve benzeri ek özellikler. Azure Kullanılabilirlik Bölgesi'ne taşınıyorsanız, standart yük dengeleyicisi gereklidir. Yeni bir dağıtım için Azure Standart Yük Dengeleyicisi ile başlamanızı öneririz. 

## <a name="3-authentication"></a>3. Kimlik doğrulama 
[Cloud Foundry Kullanıcı Hesabı ve Kimlik Doğrulama,](https://docs.cloudfoundry.org/concepts/architecture/uaa.html) CF ve çeşitli bileşenleri için merkezi kimlik yönetimi hizmetidir. [Azure Active Directory,](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) Microsoft'un çok kiracılı, bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Varsayılan olarak, UAA Cloud Foundry kimlik doğrulaması için kullanılır. Gelişmiş bir seçenek olarak UAA, Azure AD'yi harici bir kullanıcı deposu olarak da destekler. Azure AD kullanıcıları, Bulut Dökümhanesi hesabı olmadan LDAP kimliklerini kullanarak Bulut Dökümhanesi'ne erişebilir. [PCF'de UAA için Azure REKLAMını yapılandırmak için](https://docs.pivotal.io/p-identity/1-6/azure/index.html)aşağıdaki adımları izleyin.

## <a name="4-data-storage-for-cloud-foundry-runtime-system"></a>4. Cloud Foundry Runtime System için veri depolama
Cloud Foundry, uygulama çalışma zamanı sistem depolaması için Azure blobstore veya Azure MySQL/PostgreSQL hizmetlerini kullanmak için mükemmel genişletilebilirlik sunar.
### <a name="azure-blobstore-for-cloud-foundry-cloud-controller-blobstore"></a>Cloud Foundry Cloud Controller blobstore için Azure Blobstore
Bulut Denetleyicisi blobstore, yapı paketleri, damlacıklar, paketler ve kaynak havuzları için kritik bir veri deposudur. Varsayılan olarak, NFS sunucusu Cloud Controller blobstore için kullanılır. Tek bir hata noktasını önlemek için Azure Blob Depolama'yı harici depo olarak kullanın. [Özet Bulut Dökümhanesi'ndeki](https://docs.pivotal.io/pivotalcf/2-0/customizing/azure.html)arka plan ve seçenekler için [Bulut Dökümhanesi belgelerine](https://docs.cloudfoundry.org/deploying/common/cc-blobstore-config.html) göz atın.

### <a name="mysqlpostgresql-as-cloud-foundry-elastic-run-time-database-"></a>MySQL/PostgreSQL - Cloud Foundry Elastic Run Time Database *
CF Elastic Runtime iki ana sistem veritabanı gerektirir:
#### <a name="ccdb"></a>CCDB 
Bulut Denetleyici veritabanı.  Bulut Denetleyici, istemcilerin sisteme erişebilmek için REST API uç noktaları sağlar. CCDB, orglar, alanlar, hizmetler, kullanıcı rolleri ve bulut denetleyicisi için tablolar saklar.
#### <a name="uaadb"></a>UAADB 
Kullanıcı Hesabı ve Kimlik Doğrulama veritabanı. Şifreli kullanıcı adları ve parolalar gibi kullanıcı kimlik doğrulamasıyla ilgili verileri depolar.

Varsayılan olarak, yerel bir sistem veritabanı (MySQL) kullanılabilir. HA ve ölçeklendirmek için Azure tarafından yönetilen MySQL veya PostgreSQL hizmetinden yararlanın. Burada [Açık Kaynak Bulut Döküm ile CCDB, UAADB ve diğer sistem veritabanları için Azure MySQL / PostgreSQL etkinleştirme](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/configure-cf-external-databases-using-azure-mysql-postgres-service)talimatı.

## <a name="5-open-service-broker"></a>5. Açık Servis Komisyoncusu
Azure servis aracısı, uygulamanın Azure hizmetlerine erişimini yönetmek için tutarlı bir arayüz sunar. Azure için yeni [Açık Hizmet Broker'ı projesi,](https://github.com/Azure/open-service-broker-azure) Cloud Foundry, OpenShift ve Kubernetes genelindeki uygulamalara hizmet sunmanın tek ve basit bir yolunu sunar. PCF'de dağıtım yönergeleri [için PCF döşemesi için Azure Açık Servis](https://pivotal.io/platform/services-marketplace/data-management/microsoft-azure) Aracısı'na bakın.

## <a name="6-metrics-and-logging"></a>6. Ölçümler ve Günlük
Azure Log Analytics Nozzle, [Bulut Döküm kutusu loggregator firehose'undaki](https://docs.cloudfoundry.org/loggregator/architecture.html) ölçümleri [Azure Monitor günlüklerine](https://azure.microsoft.com/services/log-analytics/)ileten bir Bulut Dökümhanesi bileşenidir. With the Nozzle, you can collect, view, and analyze your CF system health and performance metrics across multiple deployments.
Azure Log Analytics Nozul'u hem Açık Kaynak hem de Önemli Bulut Dökümhanesi ortamına nasıl dağıtılacağınızı öğrenmek ve ardından Azure Monitor günlükleri konsolundan verilere nasıl eriştiğinizi öğrenmek için [burayı](https://docs.microsoft.com/azure/cloudfoundry/cloudfoundry-oms-nozzle) tıklatın. 
> [!NOTE]
> PCF 2.0'dan, VM'ler için BOSH sistem durumu ölçümleri varsayılan olarak Loggregator Firehose'a iletilir ve Azure Monitor günlükleri konsoluna entegre edilir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="7-cost-saving"></a>7. Maliyet Tasarrufu
### <a name="cost-saving-for-devtest-environments"></a>Geliştirme/Test Ortamları için Maliyet Tasarrufu
#### <a name="b-series-"></a>B Serisi: *
F ve D VM serisi Pivotal Cloud Foundry üretim ortamı için yaygın olarak önerilirken, yeni "patlayabilir" [B serisi](https://azure.microsoft.com/blog/introducing-b-series-our-new-burstable-vm-size/) yeni seçenekler getiriyor. B serisi patlamalı VM'ler, web sunucuları, küçük veritabanları ve geliştirme ve test ortamları gibi CPU'nun tam performansına sürekli olarak ihtiyaç duymamış iş yükleri için idealdir. Bu iş yükleri genellikle burstable performans gereksinimleri vardır. $0.012/hour (B1) $0.05/hour (F1) ile karşılaştırıldığında, [vm boyutlarının](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general) tam listesine bakın ve ayrıntılar için [fiyatlar.](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) 
#### <a name="managed-standard-disk"></a>Yönetilen Standart Disk: 
Üretimde güvenilir performans için premium diskler önerilir.  [Yönetilen Disk](https://azure.microsoft.com/services/managed-disks/)ile standart depolama da farklı performans ile, benzer güvenilirlik sunabilir. Geliştirme/Test veya kritik olmayan ortam gibi performansa duyarlı olmayan iş yükü için yönetilen standart diskler daha düşük maliyetli alternatif bir seçenek sunar.  
### <a name="cost-saving-in-general"></a>Genel olarak maliyet tasarrufu 
#### <a name="significant-vm-cost-saving-with-azure-reservations"></a>Azure rezervasyonlarında önemli VM Maliyet Tasarrufu: 
Bugün tüm CF VM'ler, ortamlar genellikle süresiz olarak ayakta kalsa da "isteğe bağlı" fiyatlandırma kullanılarak faturalandırılır. Artık VM kapasitesini 1 veya 3 yıllık bir süre için rezerve edebilir ve %45-65 indirim elde edebilirsiniz. İndirimler, ortamınızda herhangi bir değişiklik olmadan faturalandırma sisteminde uygulanır. Ayrıntılar için [Azure rezervasyonlarının nasıl çalıştığını](https://azure.microsoft.com/pricing/reserved-vm-instances/)öğrenin. 
#### <a name="managed-premium-disk-with-smaller-sizes"></a>Daha Küçük Boyutlarda Yönetilen Premium Disk: 
Yönetilen diskler, hem premium hem de standart diskler için P4(32 GB) ve P6 (64 GB) gibi daha küçük disk boyutlarını destekler. Küçük iş yüklerin varsa, standart premium disklerden yönetilen premium disklere geçiş yaparken maliyetten tasarruf edebilirsiniz.
#### <a name="use-azure-first-party-services"></a>Azure Birinci Taraf Hizmetlerini kullanın: 
Azure'un ilk taraf hizmetinden yararlanmak, ha ve yukarıdaki bölümlerde belirtilen güvenilirlike ek olarak uzun vadeli yönetim maliyetini düşürür. 

Pivotal PCF müşterileri için [küçük bir ayak izi ERT](https://docs.pivotal.io/pivotalcf/2-0/customizing/small-footprint.html) başlattı, bileşenleri 2500 uygulama örnekleri kadar çalışan, sadece 4 VMs içine ortak bulunmaktadır. Deneme sürümü artık [Azure Market yeri](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry)üzerinden kullanılabilir.

## <a name="next-steps"></a>Sonraki Adımlar
Azure tümleştirme özellikleri, Pivotal [Cloud Foundry'de](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/)kullanıma sunulmadan önce ilk olarak Açık Kaynak Bulut Dökümhanesi'nde kullanılabilir. * ile işaretlenmiş özellikler PCF aracılığıyla hala kullanılamaz. Azure Yığını ile Cloud Foundry tümleştirmesi de bu belgede yer almıyor.
*veya Azure Stack ile Cloud Foundry tümleştirmesi ile işaretlenmiş özelliklerdeki PCF desteği için, en son durum için Pivotal ve Microsoft hesap yöneticinize başvurun. 

