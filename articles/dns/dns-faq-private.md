---
title: Azure Özel DNS hakkında SSS
description: Azure Özel DNS hakkında sık sorulan sorular
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 09/20/2019
ms.author: victorh
ms.openlocfilehash: fca7359f9fa54899bb72be3b939e1a1839dbfbd1
ms.sourcegitcommit: 116bc6a75e501b7bba85e750b336f2af4ad29f5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71155698"
---
# <a name="azure-private-dns-faq"></a>Azure Özel DNS hakkında SSS

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="does-azure-dns-support-private-domains"></a>Özel etki alanlarını desteklemek Azure DNS mı?

Özel etki alanları için destek, Azure Özel DNS bölgeleri özelliği kullanılarak desteklenir. Özel DNS bölgeler, internet 'e yönelik Azure DNS bölgeleriyle aynı araçlarla yönetilir. Bunlar yalnızca belirtilen sanal ağlarınız içinden çözülebilir. Daha fazla bilgi için bkz. [genel bakış](private-dns-overview.md).

Azure 'daki diğer iç DNS seçenekleri hakkında daha fazla bilgi için bkz. [VM 'ler ve rol örnekleri Için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name="will-azure-dns-private-zones-work-across-azure-regions"></a>Azure DNS Özel Bölgeleri, Azure bölgelerinde çalışacak mı?

Evet. Özel bölgeler, Azure bölgelerindeki sanal ağlar arasındaki DNS çözümlemesi için desteklenir. Özel bölgeler, sanal ağları açıkça eşlemeye gerek kalmadan bile çalışmaktadır. Tüm sanal ağların özel bölge için çözümleme sanal ağları olarak belirtilmesi gerekir. TCP/HTTP trafiği için sanal ağların bir bölgeden diğerine akacak şekilde kullanılması gerekebilir.

## <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>Özel bölgeler için gereken sanal ağlardan Internet bağlantısı var mı?

Hayır. Özel bölgeler, sanal ağlarla birlikte çalışır. Sanal makineler veya sanal ağlar içindeki ve içindeki diğer kaynaklar için etki alanlarını yönetmek üzere bunları kullanabilirsiniz. Ad çözümlemesi için Internet bağlantısı gerekli değildir.

## <a name="can-the-same-private-zone-be-used-for-several-virtual-networks-for-resolution"></a>Aynı özel bölge, çözümlenmek üzere birkaç sanal ağ için de kullanılabilir mi?

Evet. Tek bir özel bölge ile 1000 sanal ağı ilişkilendirebilirsiniz.

## <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-added-as-a-linked-virtual-network-to-a-private-zone"></a>Farklı bir aboneliğe ait bir sanal ağ, bir özel bölgeye bağlı bir sanal ağ olarak eklenebilir mi?

Evet. Sanal ağlarda ve özel DNS bölgesinde yazma işlemi izninizin olması gerekir. Yazma izni birkaç RBAC rolüne verilebilir. Örneğin, klasik ağ katılımcısı RBAC rolü sanal ağlara yazma izinlerine sahiptir. RBAC rolleri hakkında daha fazla bilgi için bkz. [rol tabanlı erişim denetimi](../role-based-access-control/overview.md).

## <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-you-delete-the-virtual-machine"></a>Sanal makineyi sildiğinizde özel bir bölgedeki otomatik olarak kaydettirilen sanal makine DNS kayıtları otomatik olarak silinir mi?

Evet. Otomatik kayıt etkin olan bağlı bir sanal ağ içindeki bir sanal makineyi silerseniz, kayıtlı kayıtlar otomatik olarak silinir.

## <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-linked-virtual-network-be-deleted-manually"></a>Bağlı bir sanal ağdan özel bir bölgede otomatik olarak kaydedilmiş bir sanal makine kaydı el ile silinebilir mi?

Evet. Otomatik olarak kayıtlı DNS kayıtlarının, bölgede el ile oluşturulan bir DNS kaydıyla üzerine yazabilirsiniz. Aşağıdaki soru ve yanıt bu konuyu ele almaktadır.

## <a name="what-happens-when-i-try-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-linked-virtual-network"></a>Bağlı bir sanal ağda otomatik olarak kaydedilmiş var olan bir sanal makine ile aynı ana bilgisayar adına sahip özel bir bölgede yeni bir DNS kaydını el ile oluşturmayı denediğimde ne olur?

Bağlı bir sanal ağda, mevcut, otomatik olarak kaydedilmiş bir sanal makineyle aynı ana bilgisayar adına sahip özel bir bölgede el ile yeni bir DNS kaydı oluşturmaya çalışırsınız. Bunu yaptığınızda, yeni DNS kaydı otomatik olarak kaydedilen sanal makine kaydının üzerine yazar. Bu elle oluşturulan DNS kaydını bölgeden yeniden silmeye çalışırsanız, silme işlemi başarılı olur. Otomatik kayıt, sanal makine hala varolduğu ve kendisine bağlı özel bir IP 'si olduğu sürece yeniden gerçekleşir. DNS kaydı, bölgede otomatik olarak yeniden oluşturulur.

## <a name="what-happens-when-we-unlink-a-linked-virtual-network-from-a-private-zone-will-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-too"></a>Bağlı bir sanal ağın özel bir bölgeden bağlantısını kaldırdığınızda ne olur? Sanal ağdan otomatik olarak kaydedilen sanal makine kayıtları bölgeden da kaldırılacak mı?

Evet. Bağlı bir sanal ağın özel bir bölgeden bağlantısını kaldırmak için, DNS bölgesini, ilişkili sanal ağ bağlantısını kaldıracak şekilde güncelleştirin. Bu işlemde, otomatik olarak kaydedilen sanal makine kayıtları bölgeden kaldırılır.

## <a name="what-happens-when-we-delete-a-linked-virtual-network-thats-linked-to-a-private-zone-do-we-have-to-manually-update-the-private-zone-to-unlink-the-virtual-network-as-a-linked-virtual-network-from-the-zone"></a>Özel bir bölgeyle bağlantılı bağlı bir sanal ağı silediğimiz zaman ne olur? Sanal ağın bölge üzerinden bağlı bir sanal ağ olarak bağlantısını kaldırmak için özel bölgeyi el ile güncelleştirmemiz gerekir mi?

Evet. Bağlı bir sanal ağı, önce özel bir bölgeden kaldırmadan silerseniz, silme işlemi başarılı olur. Ancak, sanal ağ, varsa özel bölgenize otomatik olarak bağlantısız değildir. Sanal ağın özel bölge bağlantısını el ile kaldırmanız gerekir. Bu nedenle, silmeden önce sanal ağınızın bağlantısını özel bölgenize kaldırın.

## <a name="will-dns-resolution-by-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-privatecontosocom-is-linked-to-a-virtual-network"></a>Özel bir bölge (örneğin, private.contoso.com) bir sanal ağla bağlantılı olduğunda bile varsayılan FQDN (internal.cloudapp.net) kullanılarak DNS çözümlemesi devam eder mi?

Evet. Özel bölgeler, Azure tarafından sunulan internal.cloudapp.net bölgesini kullanarak varsayılan DNS çözümlerini değiştirmez. Ek bir özellik veya geliştirme olarak sunulur. Azure tarafından sağlanmış internal.cloudapp.net veya kendi özel bölgeniz olsun, çözümlemek istediğiniz bölgenin FQDN 'sini kullanın.

## <a name="will-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>Bağlı bir sanal ağ içindeki sanal makinelerdeki DNS son eki özel bölgeye göre değiştirilsin mi?

Hayır. Bağlı sanal ağınızdaki sanal makinelerdeki DNS son eki, varsayılan olarak Azure tarafından belirtilen sonek ("*. internal.cloudapp.net") olarak kalır. Sanal makinelerinizdeki bu DNS sonekini özel bölgeye el ile değiştirebilirsiniz.

## <a name="what-are-the-usage-limits-for-azure-private-dns"></a>Azure Özel DNS kullanım sınırları nelerdir?

Azure Özel DNS kullandığınızda aşağıdaki varsayılan sınırlar geçerlidir.

| Resource | Varsayılan sınır |
| --- | --- |
|Abonelik başına Özel DNS bölgeleri|1000|
|Özel DNS bölge başına kayıt kümeleri|25,000|
|Kayıt kümesi başına kayıt sayısı|20|
|Özel DNS bölgesi başına sanal ağ bağlantıları|1000|
|Otomatik kayıt özellikli özel DNS bölgeleri başına sanal ağ bağlantıları|100|
|Bir sanal ağın otomatik kayıt etkin ile bağlantılandırılan özel DNS bölgesi sayısı|1\.|
|Bir sanal ağın bağlı olduğu özel DNS bölgesi sayısı|1000|

## <a name="is-there-portal-support-for-private-zones"></a>Özel bölgeler için portal desteği var mı?

Evet ve API 'Ler, PowerShell, CLı ve SDK 'Lar aracılığıyla zaten oluşturulan özel bölgeler Azure portal görünür.

## <a name="why-dont-my-existing-private-dns-zones-show-up-in-new-portal-experience"></a>Var olan özel DNS bölgelerimin yeni Portal deneyiminde neden gösterilmeliyim?

Önizleme yenileme sürümünün bir parçası olarak özel DNS bölgeleri için yeni bir kaynak modeli sevk ettik. Mevcut özel DNS bölgelerinizin yeni Portal deneyiminde görünmesi için yeni kaynak modeline geçirilmesi gerekir. Yeni kaynak modeline geçiş hakkında yönergeler için aşağıya bakın.

## <a name="how-do-i-migrate-my-existing-private-dns-zones-to-the-new-model"></a>Var olan özel DNS bölgelerinizi yeni modele geçirmek Nasıl yaparım? mi?
Yeni kaynak modeline mümkün olan en kısa sürede geçiş yapmanızı önemle tavsiye ederiz. Ancak, eski kaynak modeli desteklenecektir, ancak daha fazla özellik bu modelin üzerine geliştirilecektir. Gelecekte bunu yeni kaynak modeli yerine kullanımdan kaldırmayı amaçlıyoruz. Mevcut özel DNS bölgelerinizi yeni kaynak modeline geçirme hakkında yönergeler için bkz.[Azure DNS özel bölgeler için geçiş kılavuzu](private-dns-migration-guide.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Özel DNS hakkında daha fazla bilgi](private-dns-overview.md)