---
title: Troubleshooting guide - Azure DNS
description: In this learning path, get started troubleshooting common issues with Azure DNS
services: dns
author: asudbring
ms.service: dns
ms.topic: article
ms.date: 09/20/2019
ms.author: allensu
ms.openlocfilehash: b5fedba7b739c07a37f3aabf75ddd8ca465ba73b
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74210932"
---
# <a name="azure-dns-troubleshooting-guide"></a>Azure DNS troubleshooting guide

This article provides troubleshooting information for common Azure DNS questions.

If these steps don't resolve your issue, you can also search for or post your issue on our [community support forum on MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork). Or, you can open an Azure support request.


## <a name="i-cant-create-a-dns-zone"></a>I can't create a DNS zone

Sık karşılaşılan sorunları çözmek için aşağıdaki adımlardan birini veya daha fazlasını deneyin:

1.  Review the Azure DNS audit logs to determine the failure reason.
2.  Her DNS bölge adı, kaynak grubu içinde benzersiz olmalıdır. That is, two DNS zones with the same name can't share a resource group. Farklı bir bölge adı veya farklı bir kaynak grubu kullanmayı deneyin.
3.  "{subscription id} aboneliğinde izin verilen en fazla bölge sayısına ulaştınız veya bu sayıyı aştınız." şeklinde bir hata görebilirsiniz. Farklı bir Azure aboneliği kullanın, bazı bölgeleri silin veya Azure Desteği ile iletişim kurarak abonelik sınırınızı yükseltin.
4.  "'{zone name}' bölgesi mevcut değil." şeklinde bir hata görebilirsiniz. Bu hata Azure DNS’in bu DNS bölgesi için ad sunucularını ayıramadığı anlamına gelir. Farklı bir bölge adı kullanmayı deneyin. Or, if you are the domain name owner you can contact Azure support to allocate name servers for you.


### <a name="recommended-articles"></a>Recommended articles

* [DNS bölgeleri ve kayıtları](dns-zones-records.md)
* [DNS bölgesi oluşturma](dns-getstarted-create-dnszone-portal.md)

## <a name="i-cant-create-a-dns-record"></a>DNS kaydı oluşturamıyorum

Sık karşılaşılan sorunları çözmek için aşağıdaki adımlardan birini veya daha fazlasını deneyin:

1.  Review the Azure DNS audit logs to determine the failure reason.
2.  Kayıt kümesi zaten var mı?  Azure DNS, kayıt *kümelerini* kullanarak kayıtları yönetir. Bu kümeler, aynı ada ve aynı türe sahip kayıtların koleksiyonudur. Zaten aynı ada ve türe sahip bir kayıt varsa buna benzer bir kayıt daha eklemek için mevcut kayıt kümesini düzenlemeniz gerekir.
3.  DNS bölgesinin tepesinde (bölgenin “kökünde”) kayıt oluşturmaya mı çalışıyorsunuz? Böyle bir kayıt oluşturmaya çalışıyorsanız DNS kuralına göre, kayıt adı olarak ‘@’ karakterini kullanmanız gerekir. Also note that the DNS standards don't permit CNAME records at the zone apex.
4.  CNAME çakışmanız mı var?  The DNS standards don't allow a CNAME record with the same name as a record of any other type. Bir CNAME’iniz varsa aynı ada sahip farklı türden bir kayıt oluşturamazsınız.  Benzer şekilde, ad farklı türden mevcut bir kayıtla eşleşiyorsa CNAME oluşturulamaz. Diğer kaydı kaldırarak veya farklı bir kayıt adı seçerek çakışmayı giderin.
5.  Bir DNS bölgesinde izin verilen kayıt kümesi sayısının sınırına mı ulaştınız? Kayıt kümelerinin geçerli sayısı ve kayıt kümelerinin maksimum sayısı, Azure portalda bölgenin 'Özellikler' seçeneğinin altında gösterilir. If you've reached this limit, then either delete some record sets or contact Azure Support to raise your record set limit for this zone, then try again. 


### <a name="recommended-articles"></a>Recommended articles

* [DNS bölgeleri ve kayıtları](dns-zones-records.md)
* [DNS bölgesi oluşturma](dns-getstarted-create-dnszone-portal.md)



## <a name="i-cant-resolve-my-dns-record"></a>DNS kaydımı çözümleyemiyorum

DNS ad çözümlemesi, çeşitli nedenlerle başarısız olabilecek çok adımlı bir işlemdir. Aşağıdaki adımlar, Azure DNS’de barındırılan bir bölgede DNS kaydı için DNS çözümlemenin neden başarısız olduğunu araştırmanıza yardımcı olacaktır.

1.  DNS kayıtlarının Azure DNS'de doğru şekilde yapılandırıldığını onaylayın. Azure portalda DNS kayıtlarını gözden geçirin ve bölge adının, kayıt adının ve kayıt türünün doğru olduğunu kontrol edin.
2.  DNS kayıtlarının, Azure DNS ad sunucularında doğru şekilde çözümlendiğini onaylayın.
    - Yerel bilgisayarınızdan DNS sorguları yaparsanız ad sunucularının geçerli durumunu yansıtmayan önbelleğe alınmış sonuçlar görebilirsiniz.  Ayrıca, kurumsal ağlar çoğu zaman DNS sorgularının belirli ad sunucularına yönlendirilmesini önleyen DNS ara sunucuları kullanır.  Bu sorunları önlemek için [digwebinterface](https://digwebinterface.com) gibi web tabanlı ad çözümlemesi hizmeti kullanın.
    - DNS bölgeniz için Azure portalda gösterildiği gibi doğru ad sunucularını belirttiğinizden emin olun.
    - DNS adının (bölge adı da dahil tam adı belirtmeniz gerekir) ve kayıt türünün doğru olduğunu kontrol edin
3.  DNS etki alanı adının, [Azure DNS ad sunucularına doğru şekilde atandığını](dns-domain-delegation.md) onaylayın. [DNS temsilci seçme doğrulaması hizmeti sunan birçok 3. taraf web sitesi](https://www.bing.com/search?q=dns+check+tool) vardır. Bu test bir *bölge* temsilcisi seçme testidir, bu nedenle yalnızca DNS bölge adını girmeli, tam kayıt adını girmemelisiniz.
4.  Yukarıdakiler tamamlandıktan sonra, DNS kaydınızın artık doğru şekilde çözümlenmesi gerekir. Doğrulamak için [digwebinterface](https://digwebinterface.com)’i, bu kez varsayılan ad sunucusu ayarlarından yararlanarak yeniden kullanabilirsiniz.


### <a name="recommended-articles"></a>Recommended articles

* [Azure DNS'e bir etki alanı atama](dns-domain-delegation.md)



## <a name="how-do-i-specify-the-service-and-protocol-for-an-srv-record"></a>SRV kaydı için ‘hizmet’ ve ‘protokol’ü nasıl belirtebilirim?

Azure DNS, DNS kayıtlarını kayıt kümeleri olarak yönetir. Bu kümeler, aynı ada ve aynı türe sahip kayıtların koleksiyonudur. SRV kayıt kümesi için 'hizmet' ve 'protokol'ün, kayıt kümesi adının parçası olarak belirtilmesi gerekir. Diğer SRV parametreleri ('öncelik', 'ağırlık', 'bağlantı noktası' ve 'hedef'), kayıt kümesindeki her kayıt için ayrı olarak belirtilir.

Örnek SRV kayıt adları (hizmet adı 'sip', protokol 'tcp'):

- \_sip.\_tcp (bölge tepesinde kayıt kümesi oluşturur)
- \_sip.\_tcp.sipservice ('sipservice' adlı bir kayıt kümesi oluşturur)

### <a name="recommended-articles"></a>Recommended articles

* [DNS bölgeleri ve kayıtları](dns-zones-records.md)
* [Azure portalını kullanarak DNS kayıt kümeleri ve kayıtları oluşturma](dns-getstarted-create-recordset-portal.md)
* [SRV kayıt türü (Wikipedia)](https://en.wikipedia.org/wiki/SRV_record)


## <a name="next-steps"></a>Sonraki adımlar

* Learn about [Azure DNS zones and records](dns-zones-records.md)
* To start using Azure DNS, learn how to [create a DNS zone](dns-getstarted-create-dnszone-portal.md) and [create DNS records](dns-getstarted-create-recordset-portal.md).
* To migrate an existing DNS zone, learn how to [import and export a DNS zone file](dns-import-export.md).

