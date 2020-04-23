---
title: Örnek Azure Altyapı Walkthrough
description: Azure'da örnek bir altyapı dağıtmaya yönelik temel tasarım ve uygulama yönergeleri hakkında bilgi edinin.
author: cynthn
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: example-scenario
ms.date: 12/15/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 43e96b891e60dfcf8bc3c29b202bb60213905372
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869464"
---
# <a name="example-azure-infrastructure-walkthrough-for-windows-vms"></a>Windows VM'ler için örnek Azure altyapısı gözden geçirme
Bu makalede, örnek bir uygulama altyapısı oluşturma yoluyla yürür. Kuralları, kullanılabilirlik kümelerini, sanal ağları ve yük dengeleyicilerini adlandırma ve sanal makinelerinizi (VM) dağıtma yla ilgili tüm yönergeleri ve kararları bir araya getiren basit bir çevrimiçi mağaza için bir altyapı tasarlamayı ayrıntılarıyla anlatıyoruz.

## <a name="example-workload"></a>Örnek iş yükü
Adventure Works Cycles, Azure'da aşağıdakilerden oluşan bir çevrimiçi mağaza uygulaması oluşturmak istiyor:

* Bir web katmanında istemciön uç çalışan iki IIS sunucusu
* Bir uygulama katmanında veri ve siparişleri işleyen iki IIS sunucusu
* Ürün verilerini ve siparişleri bir veritabanı katmanında depolamak için AlwaysOn kullanılabilirlik gruplarına (iki SQL Sunucusu ve çoğunluk düğümü tanığı) sahip iki Microsoft SQL Server örneği
* Kimlik doğrulama katmanındaki müşteri hesapları ve tedarikçiler için iki Active Directory etki alanı denetleyicisi
* Tüm sunucular iki alt ağda bulunur:
  * web sunucuları için bir ön uç alt ağ 
  * uygulama sunucuları, SQL kümesi ve etki alanı denetleyicileri için bir arka uç alt ağı

![Uygulama altyapısı için farklı katmanların diyagramı](./media/infrastructure-example/example-tiers.png)

Müşteriler çevrimiçi mağazaya göz atarken, gelen güvenli web trafiği web sunucuları arasında yük dengeli olmalıdır. Web sunucularından HTTP istekleri şeklinde sipariş işleme trafiği uygulama sunucuları arasında dengelenmelidir. Ayrıca, altyapı yüksek kullanılabilirlik için tasarlanmalıdır.

Ortaya çıkan tasarım şunları dahil etmelidir:

* Azure aboneliği ve hesabı
* Tek bir kaynak grubu
* Azure Yönetilen Diskleri
* İki alt ağa sahip bir sanal ağ
* Benzer bir role sahip VM'ler için kullanılabilirlik kümeleri
* Sanal makineler

Yukarıdakilerin tümü şu adlandırma kurallarını takip eder:

* Adventure Works Döngüleri önek olarak **[BT iş yükü]-[konum]-[Azure kaynağını]** kullanır
  * Bu örnekiçin , "**azos**" (Azure Online Store) BT iş yükü adıdır ve "**use**" (Doğu ABD 2)
* Sanal ağlar AZOS-USE-VN **[sayı]** kullanır
* Kullanılabilirlik setleri azos-use-as-**[rol]** kullanın
* Sanal makine adları azos-use-vm-**[vmname]** kullanır

## <a name="azure-subscriptions-and-accounts"></a>Azure abonelikleri ve hesapları
Adventure Works Cycles, bu BT iş yükü için faturalandırma sağlamak için Adventure Works Enterprise Subscription adlı Kurumsal aboneliklerini kullanıyor.

## <a name="storage"></a>Depolama
Adventure Works Döngüleri, Azure Yönetilen Diskler kullanmaları gerektiğini belirledi. VM oluştururken, kullanılabilir depolama katmanlarının her ikisi de kullanılır:

* Web sunucuları, uygulama sunucuları ve etki alanı denetleyicileri ve bunların veri diskleri için **standart depolama.**
* SQL Server VM'leri ve veri diskleri için **premium depolama.**

## <a name="virtual-network-and-subnets"></a>Sanal ağ ve alt ağlar
Sanal ağın Adventure Work Cycles şirket içi ağına sürekli bağlantı ya da şirket içi bağlantısına ihtiyacı olmadığından, yalnızca buluta yönelik bir sanal ağa karar verirler.

Azure portalını kullanarak aşağıdaki ayarlara sahip yalnızca buluta özel bir sanal ağ oluşturdular:

* Adı: AZOS-USE-VN01
* Yer: Doğu ABD 2
* Sanal ağ adres alanı: 10.0.0.0/8
* İlk alt ağ:
  * Adı: FrontEnd
  * Adres alanı: 10.0.1.0/24
* İkinci alt ağ:
  * Adı: BackEnd
  * Adres alanı: 10.0.2.0/24

## <a name="availability-sets"></a>Kullanılabilirlik kümeleri
Adventure Works Cycles, çevrimiçi mağazalarının dört katmanının da yüksek kullanılabilirliğini sağlamak için dört kullanılabilirlik kümesine karar verdi:

* web sunucuları için **azos-kullanımı-as-web**
* uygulama sunucuları için **azos-kullanım-as-app**
* SQL Sunucular için **azos-use-as-sql**
* etki alanı denetleyicileri için **azos-use-as-dc**

## <a name="virtual-machines"></a>Sanal makineler
Adventure Works Cycles, Azure VM'leri için aşağıdaki adlara karar verdi:

* ilk web sunucusu için **azos-use-vm-web01**
* ikinci web sunucusu için **azos-use-vm-web02**
* ilk uygulama sunucusu için **azos-use-vm-app01**
* ikinci uygulama sunucusu için **azos-use-vm-app02**
* kümedeki ilk SQL Server sunucusu için **azos-use-vm-sql01**
* kümedeki ikinci SQL Server sunucusu için **azos-use-vm-sql02**
* ilk etki alanı denetleyicisi için **azos-use-vm-dc01**
* ikinci etki alanı denetleyicisi için **azos-use-vm-dc02**

İşte ortaya çıkan yapılandırma.

![Azure'da dağıtılan son uygulama altyapısı](./media/infrastructure-example/example-config.png)

Bu yapılandırma şunları içerir:

* İki alt ağa sahip yalnızca buluta özel sanal ağ (FrontEnd ve BackEnd)
* Standart ve Premium disklere sahip Azure Yönetilen Diskler
* Çevrimiçi mağazanın her katmanı için bir tane olmak üzere dört kullanılabilirlik kümesi
* Dört katman için sanal makineler
* Internet'ten web sunucularına HTTPS tabanlı web trafiği için harici yük dengeli bir ayar
* Web sunucularından uygulama sunucularına şifrelenmemiş web trafiği için dahili yük dengeli bir ayar
* Tek bir kaynak grubu
