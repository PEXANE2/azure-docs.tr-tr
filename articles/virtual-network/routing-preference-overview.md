---
title: Azure 'da yönlendirme tercihi
description: Yönlendirme tercihi ile trafiğinizi Azure ile Internet arasında nasıl yönlendirdiğini nasıl seçebileceğinizi öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
Customer intent: As an Azure customer, I want to learn more about routing choices for my internet egress traffic.
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2020
ms.author: mnayak
ms.openlocfilehash: ba01b89b54dc7209449490059555f531f7616720
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85193371"
---
# <a name="what-is-routing-preference-preview"></a>Yönlendirme tercihi nedir (Önizleme)?

Azure yönlendirme tercihi, trafiğinizin Azure ile Internet arasında nasıl yönlendirdiğini seçmenizi sağlar. Trafiği Microsoft ağı aracılığıyla ya da ISS ağı (genel İnternet) aracılığıyla yönlendirmeyi seçebilirsiniz. Bu seçenekler aynı zamanda *soğuk patates* yönlendirme ve *sık erişimli yönlendirme* olarak da adlandırılır. Çıkış veri aktarımı fiyatı, yönlendirme seçimine göre farklılık gösterir. Genel IP adresi oluştururken yönlendirme seçeneğini belirleyebilirsiniz. Genel IP adresi, sanal makine, sanal makine ölçek kümeleri, internet 'e yönelik yük dengeleyici vb. gibi kaynaklarla ilişkilendirilebilir. Blob, dosya, Web ve Azure Veri Gölü gibi Azure depolama kaynakları için yönlendirme tercihini de ayarlayabilirsiniz. Varsayılan olarak, trafik tüm Azure hizmetleri için Microsoft Global ağı aracılığıyla yönlendirilir.

> [!IMPORTANT]
> Yönlendirme tercihi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="routing-via-microsoft-global-network"></a>Microsoft Global ağı aracılığıyla yönlendirme

Trafiğinizi *Microsoft Global Network*üzerinden yönlendirdiğinizde, trafik 160.000 ' den fazla Fiber ' a yayılan en büyük ağlardan biri üzerinden, 165 ' den fazla iletişim kutusu (pop) ile dağıtılır. Ağ, aşırı yüksek güvenilirlik ve kullanılabilirlik sağlamak için birden fazla yedekli fiber yol ile birlikte sağlanmış olur. Trafik Mühendisliği, trafiğiniz için düşük gecikmeli yol seçimini sağlayan, yazılım tanımlı bir WAN denetleyicisi tarafından yönetilir ve Premium ağ performansı sunar.

![Microsoft Global ağı aracılığıyla yönlendirme](media/routing-preference-overview/route-via-microsoft-global-network.png)

Giriş **trafiği:** Küresel BGP her noktaya yayın duyurusu, giriş trafiğinin kullanıcıya en yakın Microsoft ağını girdiğini sağlar. Örneğin, Singapur 'deki bir Kullanıcı Chicago 'de barındırılan Azure kaynaklarına eriştiğinde, ABD 'de Microsoft Global ağı 'nda Singapur Edge POP 'daki trafik girilir ve Microsoft Network 'e Chicago 'de barındırılan hizmet ile seyahat edilir.

**Çıkış trafiği:** Çıkış trafiği aynı prensibi izler. Trafik, Microsoft küresel ağı 'na ait yolculuğun büyük bir bölümünü hareket eder ve kullanıcıya en yakın çıkış yapar. Örneğin, Azure Chicago 'nin trafiği Singapur 'dan bir kullanıcıya gidiyor ise trafik Microsoft ağı 'na Chicago 'den Singapur 'a, Microsoft ağı ise Singapur Edge POP 'da çıkar.

Hem giriş hem de çıkış trafiği Microsoft Global Network 'teki seyahatin toplu olarak kalır. Bu, *soğuk patates yönlendirme*olarak da bilinir.


## <a name="routing-over-public-internet-isp-network"></a>Ortak Internet üzerinden yönlendirme (ISS ağı)

Yeni yönlendirme seçimi *Internet yönlendirme* , Microsoft Global Network 'teki yolculuğu en aza indirir ve trafiğinizi yönlendirmek IÇIN transit ISS ağını kullanır. Bu maliyet için iyileştirilmiş yönlendirme seçeneği, diğer bulut sağlayıcılarına benzer ağ performansı sunar.

![Genel Internet üzerinden yönlendirme](media/routing-preference-overview/route-via-isp-network.png)

Giriş **trafiği:** Giriş yolu, trafiğin barındırılan hizmet bölgesine en yakın Microsoft ağını girdiği anlamına gelen, *etkin patates yönlendirme* kullanır. Örneğin, Singapur 'deki bir Kullanıcı Chicago 'de barındırılan Azure kaynaklarına eriştiğinde trafik genel İnternet üzerinden geçer ve Microsoft Global ağı Chicago 'ye girer.

**Çıkış trafiği:** Çıkış trafiği aynı prensibi izler. Trafik, hizmetin barındırıldığı bölgedeki Microsoft ağından çıkar. Örneğin, Azure Chicago 'deki hizmetinizin trafiği Singapur 'dan bir kullanıcıya gidiyor ise trafik, Microsoft ağını Chicago 'da çıkar ve genel İnternet üzerinden Singapur 'daki Kullanıcı üzerinden dolaşır.

## <a name="supported-services"></a>Desteklenen hizmetler

"Microsoft Global Network" yönlendirme tercihi seçimine sahip genel IP, herhangi bir Azure hizmeti ile ilişkilendirilebilir. Ancak, yönlendirme tercihi seçimi **Internet** Ile genel IP aşağıdaki Azure kaynaklarıyla ilişkilendirilebilir:

* Sanal makine
* Sanal makine ölçek kümesi
* Azure Kubernetes Hizmeti (AKS)
* Internet 'e yönelik yük dengeleyici
* Application Gateway
* Azure Güvenlik Duvarı

Depolama için birincil uç noktalar her zaman **Microsoft Global ağını**kullanır. Trafik yönlendirme için tercih ettiğiniz ikincil uç noktaları **Internet** ile etkinleştirebilirsiniz. Desteklenen depolama hizmetleri şunlardır:

* Bloblar
* Dosyalar
* Web
* Azure DataLake

## <a name="pricing"></a>Fiyatlandırma
Her iki seçenek arasındaki fiyat farkı, Internet çıkış veri aktarımı fiyatlandırmasına yansıtılır. **Microsoft Global Network** veri aktarımı fiyatı üzerinden yönlendirme, geçerli internet çıkış fiyatıyla aynı. En son fiyatlandırma bilgileri için [Azure bant genişliği fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/bandwidth/) ziyaret edin. Aşağıdaki tabloda gösterildiği gibi, **genel Internet** üzerinden yönlendirme daha düşük fiyatlandırılır:

| Çıkış kaynak bölgesi | 0-5 GB/ay | 5 GB-10 TB/ay | 10-50 TB/ay | 50-150 TB/ay | 150-500 TB/ay |
| --- | --- | --- | --- | --- | --- |
| Bölge 1 | $0/GB | $0.085/GB | $0.065/GB | $0.06/GB | $0.04/GB |
| Bölge 2 | $0/GB | $0.11/GB | $0.075/GB | $0,07/GB | $0.06/GB  |

500 TB üzerinden aylık hacim için [bizimle Iletişim kurun](https://azure.microsoft.com/overview/sales-number/) .
* Bölge 1 — Avustralya Orta, Avustralya Orta 2, Kanada Orta, Kanada Doğu, Kuzey Avrupa, Batı Avrupa, Fransa Orta, Fransa Güney, Almanya Kuzey (genel), Almanya Orta Batı (genel), Norveç Doğu, Norveç Batı, İsviçre Kuzey, İsviçre Batı, UK Güney, UK Batı, Orta ABD, Doğu ABD, Doğu ABD 2, Orta Kuzey ABD, Orta Güney ABD, Batı ABD, Batı ABD 2 ve Orta Batı ABD.

* Bölge 2 — Doğu Asya, Güneydoğu Asya, Avustralya Doğu, Avustralya Güneydoğu, Orta Hindistan, Güney Hindistan, Batı Hindistan, Japonya Doğu, Japonya Batı, Kore Orta ve Kore Güney.

* Bölge 3 — Brezilya Güney, Güney Afrika Kuzey, Güney Afrika Batı, BAE Orta ve BAE Kuzey.

## <a name="availability"></a>Kullanılabilirlik

Yönlendirme tercihi desteği, internet çıkış-Kuzey Avrupa, Batı Avrupa, Fransa Güney, UK Güney, Doğu ABD, Orta Kuzey ABD, Orta Güney ABD, Batı ABD, Orta Batı ABD, Güneydoğu Asya, Almanya Orta Batı, İsviçre Batı, Japonya Doğu ve Japonya Batı için genel bir IP kullanan sanal makine ve internet 'e yönelik yük dengeleyici gibi hizmetler için aşağıdaki bölgelerde kullanılabilir.

Depolama hesabı için yönlendirme tercihi desteği, aşağıdaki Azure bölgelerinde bulunur-Fransa Güney, Orta Kuzey ABD, Orta Batı ABD, Japonya Doğu, Japonya Batı, Almanya Orta Batı ve İsviçre Batı.
## <a name="limitations"></a>Sınırlamalar

* Yönlendirme tercihi yalnızca standart genel IP adresi SKU 'SU ile uyumludur. Temel genel IP adresi SKU 'SU desteklenmiyor.
* Yönlendirme tercihi şu anda yalnızca IPv4 Genel IP adreslerini desteklemektedir. IPv6 genel IP adresleri desteklenmez.
* Birden çok NIC içeren sanal makineler yalnızca bir yönlendirme tercihi türüne sahip olabilir.


## <a name="next-steps"></a>Sonraki adımlar

* [Azure PowerShell kullanarak bir VM için yönlendirme tercihini yapılandırma](configure-routing-preference-virtual-machine-powershell.md)
* [Azure CLı kullanarak bir VM için yönlendirme tercihini yapılandırma](configure-routing-preference-virtual-machine-cli.md)
