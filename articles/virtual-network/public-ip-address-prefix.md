---
title: Azure Genel IP adresi öneki | Microsoft Dokümanlar
description: Azure genel IP adresi önekinin ne olduğu ve kaynaklarına öngörülebilir genel IP adresleri atamanıza nasıl yardımcı olabileceği hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: anavin
ms.openlocfilehash: 12fb7e03062600745cd8511d37b439ce44f2ef78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75640729"
---
# <a name="public-ip-address-prefix"></a>Genel IP adresi ön eki

Genel IP adresi öneki, Azure'daki ortak uç noktalarınız için ayrılmış bir IP adresi aralığıdır. Azure, belirtme nizsayısına bağlı olarak aboneliğinize bitişik bir adres aralığı ayırır. Ortak adresleri bilmiyorsanız, Genel IP [adreslerine bakın.](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)

Ortak IP adresleri, her Azure bölgesindeki bir adres havuzundan atanır. Azure'un her bölge için kullandığı aralıkların listesini [indirebilirsiniz.](https://www.microsoft.com/download/details.aspx?id=56519) Örneğin, 40.121.0.0/16, Azure'un Doğu ABD bölgesinde kullandığı 100'den fazla aralıktan biridir. Aralık 40.121.0.1 - 40.121.255.254 kullanılabilir adresleri içerir.

Bir ad belirterek bir Azure bölgesinde ve abonelikte genel bir IP adresi öneki oluşturursunuz ve önek kaç adres eklemek istediğinizi. Örneğin, /28'in genel bir IP adresi öneki oluşturursanız, Azure sizin için aralıklarından birinden 16 adres ayırır. Siz aralığı oluşturana kadar Azure'un hangi aralığı ataacağını bilemezsiniz, ancak adresler bitişiktir. Genel IP adresi önekleri ücretlidir. Ayrıntılar için [genel IP adresi](https://azure.microsoft.com/pricing/details/ip-addresses)fiyatlandırması'na bakın.

## <a name="why-create-a-public-ip-address-prefix"></a>Neden genel bir IP adresi öneki oluştur?

Ortak IP adresi kaynakları oluşturduğunuzda, Azure bölgede kullanılan aralıklardan herhangi birinden kullanılabilir bir genel IP adresi atar. Azure adresi atadıktan sonra, adresin ne olduğunu bilirsiniz, ancak Azure adresi atayana kadar hangi adresin atanabileceğini bilemezsiniz. Bu durum, örneğin siz veya iş ortaklarınız belirli IP adreslerine izin veren güvenlik duvarı kuralları nı kurduğunda sorun yaratabilir. Bir kaynağa her yeni bir genel IP adresi atadığınızda, adresin güvenlik duvarı kuralına eklenmesi gerekir. Genel bir IP adresi önekinden kaynaklarınıza adres ler atadığınızda, tüm aralık bir kurala eklenebileceğinden, adreslerden birini her atadığınızda güvenlik duvarı kurallarının güncelleştirilmesi gerekmez.

## <a name="benefits"></a>Avantajlar

- Bilinen bir aralıktan genel IP adresi kaynakları oluşturabilirsiniz.
- Siz veya iş ortaklarınız, şu anda atadığınız genel IP adreslerinin yanı sıra henüz atadığınız adresleri içeren aralıklarla güvenlik duvarı kuralları oluşturabilir. Bu, IP adreslerini yeni kaynaklara atadıkça güvenlik duvarı kurallarını değiştirme gereksinimini ortadan kaldırır.
- Oluşturabileceğiniz bir aralığın varsayılan boyutu /28 veya 16 IP adresidir.
- Kaç aralık oluşturabileceğinizkonusunda herhangi bir sınırlama yoktur, ancak bir Azure aboneliğinde sahip olabileceğiniz maksimum statik genel IP adresi sayısında sınırlamalar vardır. Sonuç olarak, oluşturduğunuz aralıkların sayısı, aboneliğinizde sahip olabileceğiniz den daha fazla statik genel IP adresini kapsayamaz. Daha fazla bilgi için [Azure sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)bakın.
- Önekteki adresleri kullanarak oluşturduğunuz adresler, herkese açık bir IP adresi atayabileceğiniz herhangi bir Azure kaynağına atanabilir.
- Aralık içinde henüz tahsis edilmemiş ip adreslerinin ne olduğunu kolayca görebilirsiniz.

## <a name="scenarios"></a>Senaryolar
Aşağıdaki kaynakları bir önek teki statik genel IP adresiyle ilişkilendirebilirsiniz:

|Kaynak|Senaryo|Adımlar|
|---|---|---|
|Virtual Machines| Azure'da genel IP'leri bir önek teki sanal makinelerinize ilişkilendirmek, bir güvenlik duvarında IP'leri beyaz listeye alma söz konusu olduğunda yönetim ek lerini azaltır. Tek bir güvenlik duvarı kuralıyla tüm önek'i beyaz listeye alabilirsiniz. Azure'daki sanal makinelerle ölçeklendikçe, IP'leri aynı önek maliyet, zaman ve yönetim ek yükünden ilişkilendirebilirsiniz.| IP'leri bir önek ile sanal makinenize ilişkilendirmek için: 1. [Bir önek oluşturun.](manage-public-ip-address-prefix.md) 2. [Önek bir IP oluşturun.](manage-public-ip-address-prefix.md) 3. [IP'yi sanal makinenizin ağ arabirimiyle ilişkilendirin.](virtual-network-network-interface-addresses.md#add-ip-addresses) [IP'leri Sanal Makine Ölçeği Kümesi](https://azure.microsoft.com/resources/templates/101-vmms-with-public-ip-prefix/)ile de ilişkilendirebilirsiniz.
| Standart Yük Dengeleyicileri | Genel IP'leri bir önek ten önek veya Yük Dengeleyicisi'nin giden kuralına ilişkilendirmek, Azure genel IP adres alanınızın basitleştirilmesini sağlar. Genel IP öneki tarafından tanımlanan bir dizi bitişik IP adresinden kaynaklanan giden bağlantıları tımar ederek senaryonuzu basitleştirebilirsiniz. | IP'leri bir önek ile Yük bakiyeniz le ilişkilendirmek için: 1. [Bir önek oluşturun.](manage-public-ip-address-prefix.md) 2. [Önek bir IP oluşturun.](manage-public-ip-address-prefix.md) 3. Yük Dengeleyicisini oluştururken, yukarıda adım 2'de oluşturulan IP'yi Yük Dengeleyicinizin ön uç IP'si olarak seçin veya güncelleyin. |
| Azure Güvenlik Duvarı | Giden SNAT için öneki bir ortak IP kullanabilirsiniz. Bu, tüm giden sanal ağ trafiğinin [Azure Güvenlik Duvarı](../firewall/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) genel IP'sine çevrildiği anlamına gelir. Bu IP önceden belirlenmiş bir önek geldiğinden, Azure'daki genel IP ayak izinizin nasıl görüneceğini önceden bilmek çok kolaydır. | 1. [Bir önek oluşturun.](manage-public-ip-address-prefix.md) 2. [Önek bir IP oluşturun.](manage-public-ip-address-prefix.md) 3. Azure [güvenlik duvarını dağıttığınızda,](../firewall/tutorial-firewall-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-the-firewall)önekten daha önce tahsis ettiğiniz IP'yi seçtiğinizden emin olun.|
| Uygulama Ağ Geçidi v2 | Otomatik ölçekleme ve bölge yedekli Uygulama ağ geçidi v2 için öneki bir ortak IP kullanabilirsiniz. Bu IP önceden belirlenmiş bir önek geldiğinden, Azure'daki genel IP ayak izinizin nasıl görüneceğini önceden bilmek çok kolaydır. | 1. [Bir önek oluşturun.](manage-public-ip-address-prefix.md) 2. [Önek bir IP oluşturun.](manage-public-ip-address-prefix.md) 3. Uygulama [Ağ Geçidi'ni dağıttığınızda,](../application-gateway/quick-create-portal.md#create-an-application-gateway)önekten daha önce tahsis ettiğiniz IP'yi seçtiğinizden emin olun.|

## <a name="constraints"></a>Kısıtlamalar

- Önek için IP adreslerini belirtemezsiniz. Azure, belirttiğiniz boyuta bağlı olarak önek için IP adreslerini ayırır.
- En fazla 16 IP adresi veya /28 öneki oluşturabilirsiniz. Daha fazla bilgi için [Azure sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)bakın.
- Önek oluşturduktan sonra aralığı değiştiremezsiniz.
- Yalnızca Standart SKU ile oluşturulan statik genel IP adresleri öneek aralığından atanabilir. Ortak IP adresi SK'leri hakkında daha fazla bilgi edinmek için [genel IP adresine](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)bakın.
- Aralıktaki adresler yalnızca Azure Kaynak Yöneticisi kaynaklarına atanabilir. Adresler klasik dağıtım modelindeki kaynaklara atanamıyor.
- Önek te oluşturulan tüm genel IP adreslerinin önek ile aynı Azure bölgesinde ve abonelikte bulunması ve aynı bölgedeki kaynaklara ve aboneliklere atanması gerekir.
- Bir kaynakla ilişkili genel IP adresi kaynaklarına atanmış bir önek silemezsiniz. İlk önek itibaren IP adresleri atanan tüm genel IP adresi kaynaklarını ayrıştırın.


## <a name="next-steps"></a>Sonraki adımlar

- Genel IP adresi öneki [oluşturma](manage-public-ip-address-prefix.md)
