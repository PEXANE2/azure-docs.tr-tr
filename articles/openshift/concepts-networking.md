---
title: Kavramlar-Azure Red Hat 'te OpenShift 4 üzerinde ağ diyagramı
description: Azure Red Hat OpenShift ağı için ağ diyagramı ve genel bakış
author: sakthi-vetrivel
ms.author: suvetriv
ms.topic: tutorial
ms.service: container-service
ms.date: 06/22/2020
ms.openlocfilehash: fb81405e85d6e2653e0cf6c007c363493992161a
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87419980"
---
# <a name="networking-in-azure-red-hat-on-openshift-4"></a>OpenShift 4 üzerinde Azure Red Hat 'te ağ oluşturma

Bu kılavuz, OpenShift 4 kümelerinde Azure Red Hat 'teki ağa genel bir bakış ve bir diyagram ve önemli uç noktalar listesi içerir.

Core OpenShift ağ kavramları hakkında daha fazla bilgi için bkz. [Azure Red Hat OpenShift 4 ağ belgeleri](https://docs.openshift.com/aro/4/networking/understanding-networking.html).

## <a name="networking-concepts-in-azure-red-hat-openshift"></a>Azure Red Hat OpenShift 'teki ağ kavramları

![Azure Red Hat OpenShift 4 ağ diyagramı](./media/concepts-networking/aro4-networking-diagram.png)

OpenShift 4 ' te Azure Red Hat dağıttığınızda, tüm kümeniz bir sanal ağ içinde yer alır. Bu sanal ağ içinde, her biri kendi alt ağında yaşayan ana düğümleriniz ve çalışan düğümleriniz. Her alt ağ, ortak ve iç yük dengeleyici kullanır.

## <a name="explanation-of-endpoints"></a>Uç noktaların açıklaması

Aşağıdaki liste, bir Azure Red Hat OpenShift kümesindeki önemli uç noktaları içerir.

* **Aro-pls**
    * Bu, Microsoft ve Red Hat site güvenilirliği mühendisleri tarafından kümenin yönetilmesine yardımcı olmak için kullanılan bir Azure özel bağlantı uç noktasıdır.
* **Aro-iç-lb**
    * Bu uç nokta, trafiği API sunucusuyla dengeler. Bu yük dengeleyici için ana düğümler arka uç havuzudur.
* **Aro-genel-lb**
    * API genel ise, bu uç nokta trafiği API sunucusuyla yönlendirir ve dengeler. Bu uç nokta genel bir giden IP atar ve bu sayede ana bilgisayarlar Azure Resource Manager ve rapor durumu üzerinde yeniden rapor verebilir.
* **Aro-iç**
    * Bu uç nokta iç hizmet trafiğini dengeler. Bu yük dengeleyici için çalışan düğümleri arka uç havuzudur.
    * Bu yük dengeleyici varsayılan olarak oluşturulmaz. Bu yük dengeleyici, doğru ek açıklamalarla yük dengeleyici türünde bir hizmet oluşturduğunuzda oluşturulur. Örneğin: service.beta.kubernetes.io/azure-load-balancer-internal: "true".
* **Ağ Ilkeleri (giriş)**
    * OpenShift SDN 'nin bir parçası olarak desteklenir
    * Varsayılan olarak etkin, müşteriler tarafından gerçekleştirilen zorlama
    * V1 NetworkPolicy uyumlu, ancak "çıkış ve IpBlock" türleri henüz desteklenmiyor
    * **Aro**
    * Bu uç nokta, trafiği API sunucusuyla dengeler. Bu yük dengeleyici için ana düğümler arka uç havuzudur.
  * **Aro-iç-lb**
    * Bu uç nokta tüm ortak trafik için kullanılır. Ve uygulama ve bir yol oluşturduğunuzda bu, giriş trafiğinin yoludur.
    * Bu yük dengeleyici Ayrıca, Azure Load Balancer giden kuralları aracılığıyla çalışan düğümlerinde çalışan herhangi bir pod 'tan çıkış Internet bağlantısını da içerir.
        * Şu anda giden kurallar yapılandırılamaz. Her düğüme 1.024 TCP bağlantı noktası ayırır.
        * DisableOutboundSnat, LB kurallarında yapılandırılmamış, bu nedenle bu ALB 'de yapılandırılmış çıkış IP 'leri genel IP olarak alabilir.
        * Önceki iki noktasının bir sonucu olarak, kısa ömürlü SNAT bağlantı noktaları eklemenin tek yolu, ARO 'ye ortak yük dengeleyici-tür hizmetleri eklemektir.
* **Ağ Ilkeleri (çıkış)**
    * Çıkış Ilkeleri, OpenShift 'teki çıkış güvenlik duvarı özelliği kullanılarak desteklenir.
    * Ad alanı/proje başına yalnızca bir tane.
    * Çıkış ilkeleri "varsayılan" ad alanında desteklenmez.
    * Çıkış ilke kuralları sırayla değerlendirilir (ilk olarak en son).
    * **Aro-giden-PIP**
        * Bu uç nokta, çalışan düğümleri için genel IP (PıP) görevi görür.
        * Bu uç nokta, hizmetlerin bir Azure Red Hat OpenShift kümesinden izin verilenler listesine gelen belirli bir IP eklemesini sağlar.
* **Aro-node-NSG**
    * Bir hizmeti kullanıma sundığınızda, API bu ağ güvenlik grubunda bir kural oluşturarak trafiğin düğümlere akışını ve düğümlere ulaşmasını sağlayın.
    * Varsayılan olarak, bu ağ güvenlik grubu tüm giden trafiğe izin verir. Şu anda giden trafik yalnızca Azure Red Hat OpenShift denetim düzlemi ile kısıtlanabilir.
* **Aro-controldüzlemi-NSG**
    * Bu uç nokta yalnızca ana düğümler için bağlantı noktası 6443 üzerinden trafiğe izin verir.
* **Azure Container Registry**
    * Bu, Microsoft tarafından dahili olarak sağlanmış ve kullanılan bir kapsayıcı kayıt defteridir.
        * Bu kayıt defteri, konak platformu görüntülerini ve küme bileşenlerini sağlar. Örneğin, kapsayıcıları izleme veya günlüğe kaydetme.
        * Azure Red Hat OpenShift müşterileri tarafından kullanılmaya yönelik değildir.  
        * Salt okunurdur.
        * Bu kayıt defterine yönelik bağlantılar hizmet uç noktası üzerinden (Azure hizmetleri arasında iç bağlantı) oluşur.
        * Bu iç kayıt defteri varsayılan olarak küme dışında kullanılamaz.
* **Özel Bağlantı**
    * Yönetim düzlemine küme yönetimi için bir kümeye Ağ bağlantısının yapılmasına izin verir.
    * Kümenizin yönetilmesine yardımcı olmak için Microsoft ve Red Hat site güvenilirliği mühendisleri.

## <a name="networking-basics-in-openshift"></a>OpenShift 'te Ağ temelleri

OpenShift yazılım tanımlı ağ (SDN), kapsayıcı ağ arabirimi (CNı) belirtimine dayalı bir OpenFlow uygulamasının açık vSwitch (OVS) kullanılarak bir yer paylaşımı ağını yapılandırmak için kullanılır. SDN farklı eklentileri destekler ve ağ Ilkesi, OpenShift 4 üzerinde Azure Red Hat 'te kullanılan eklentidir. Tüm ağ iletişimleri SDN tarafından yönetiliyor, bu yüzden Pod 'e Pod iletişimine ulaşmak için sanal ağlarınızda ek bir yol gerekli değildir.

## <a name="azure-red-hat-openshift-networking-specifics"></a>Azure Red Hat OpenShift ağ özellikleri

Aşağıdaki özellikler Azure Red Hat OpenShift 'e özgüdür:
* Kendi sanal ağınızı getir işlemi desteklenir.
* Pod ve hizmet ağı Cıdrs yapılandırılabilir.
* Düğümler ve ana bilgisayarlar farklı alt ağlardır.
* Düğümler ve ana sanal ağ alt ağları en az/27 olmalıdır.
* Pod CıDR değeri minimum/18 boyutunda olmalıdır (pod ağı yönlendirilemeyen IP 'Ler ve yalnızca OpenShift SDN içinde kullanılır).
* Her düğüm, IP 'Leri için ayrılan/23 alt ağı (512 IP). Bu değer değiştirilemez.
* Birden çok ağa Pod ekleyemezsiniz.
* Çıkış statik IP 'yi yapılandıramazsınız. (Bu bir OpenShift özelliğidir. Bilgi için bkz. [Çıkış IP 'lerini yapılandırma](https://docs.openshift.com/aro/4/networking/openshift_sdn/assigning-egress-ips.html)).

## <a name="network-settings"></a>Ağ ayarları

Aşağıdaki ağ ayarları Azure Red Hat OpenShift 4 ' te kullanılabilir:

* **API görünürlüğü** - [az Aro Create komutunu](tutorial-create-cluster.md#create-the-cluster)çalıştırırken API görünürlüğünü ayarlayın.
    * "Genel"-API sunucusuna dış ağlar tarafından erişilebilir.
    * "Özel"-API sunucusu, ana alt ağdan özel bir IP atamıştır, yalnızca bağlı ağlar (eşlenen sanal ağlar, kümedeki diğer alt ağlar) kullanılarak erişilebilir. Müşterinin adına özel bir DNS bölgesi oluşturulacaktır.
* Giriş **görünürlüğü** - [az Aro Create komutunu](tutorial-create-cluster.md#create-the-cluster)çalıştırırken API görünürlüğünü ayarlayın.
    * "Genel" yollar varsayılan olarak genel Azure Standart Load Balancer (Bu değiştirilebilir).
    * "Özel" yollar varsayılan olarak iç yük dengeleyiciye (bu değişebilir).

## <a name="network-security-groups"></a>Ağ güvenlik grupları
Ağ güvenlik grupları, düğümün kilitli olduğu düğümün kaynak grubunda oluşturulacaktır. Ağ güvenlik grupları, düğümün NIC 'Leri üzerinde değil, doğrudan alt ağlara atanır. Ağ güvenlik grupları sabittir, bu da bunları değiştirme izinlerinizin olmadığı anlamına gelir. 

Ancak, herkese açık bir API sunucusu ile ağ güvenlik grupları oluşturamaz ve bunları NIC 'lere atayamazsınız.

## <a name="domain-forwarding"></a>Etki alanı iletme
Azure Red Hat Openshıft, CoreDNS kullanır. Etki alanı iletme yapılandırılabilir (daha fazla ayrıntı için [DNS iletmeyi kullanma](https://docs.openshift.com/aro/4/networking/dns-operator.html#nw-dns-forward_dns-operator) hakkındaki belgelere bakın).

Şu anda, kendi DNS 'nizi sanal ağlarınıza getiremezsiniz.


Giden trafik hakkında daha fazla bilgi ve çıkış için Azure Red Hat OpenShift 'i destekleme hakkında daha fazla bilgi için bkz. [destek ilkeleri](support-policies-v4.md) belgeleri.
