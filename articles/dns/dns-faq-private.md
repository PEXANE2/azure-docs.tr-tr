---
title: Azure Özel DNS hakkında SSS
description: Bu makalede, Azure Özel DNS hakkında sık sorulan soruları öğrenin
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 10/05/2019
ms.author: rohink
ms.openlocfilehash: 4de585a965cfefa6399b0c0929a8f732d0712617
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76939417"
---
# <a name="azure-private-dns-faq"></a>Azure Özel DNS hakkında SSS

Aşağıda Azure özel DNS ile ilgili sık sorulan sorular yer almaktadır.

## <a name="does-azure-dns-support-private-domains"></a>Azure DNS özel etki alanlarını destekliyor mu?

Azure Özel DNS bölgeleri özelliği kullanılarak özel etki alanları desteklenir. Özel DNS bölgeleri yalnızca belirtilen sanal ağların içinden çözülebilir. Daha fazla bilgi için [genel bakışa](private-dns-overview.md)bakın.

Azure'daki diğer dahili DNS seçenekleri hakkında bilgi [için, VM'ler ve rol örnekleri için Ad çözünürlüğüne](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)bakın.

## <a name="will-azure-private-dns-zones-work-across-azure-regions"></a>Azure Özel DNS bölgeleri Azure bölgelerinde çalışır mı?

Evet. Azure bölgelerindeki sanal ağlar arasındaki DNS çözümü için Özel Bölgeler desteklenir. Özel Bölgeler, sanal ağlara açıkça bakmadan bile çalışır. Tüm sanal ağlar özel DNS bölgesine bağlı olmalıdır.

## <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>Özel bölgeler için sanal ağlardan Internet bağlantısı gerekli midir?

Hayır. Özel bölgeler sanal ağlarla birlikte çalışır. Bunları, sanal ağlar içinde ve dışında sanal makineler veya diğer kaynaklar için etki alanlarını yönetmek için kullanırsınız. Ad çözümlemesi için Internet bağlantısı gerekmez.

## <a name="can-the-same-private-zone-be-used-for-several-virtual-networks-for-resolution"></a>Aynı özel bölge çözüm için birkaç sanal ağ için kullanılabilir mi?

Evet. Özel bir DNS bölgesini binlerce sanal ağa bağlayabilirsiniz. Daha fazla bilgi için Azure [DNS Sınırları'na](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-dns-limits) bakın

## <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-linked-to-a-private-zone"></a>Farklı bir aboneye ait sanal ağ özel bir bölgeye bağlanabilir mi?

Evet. Sanal ağlarda ve özel DNS bölgesinde işlem izni yazmanız gerekir. Yazma izni birkaç RBAC rolüne verilebilir. Örneğin, Klasik Ağ Katılımcısı RBAC rolü sanal ağlara yazma izinleri ve Özel DNS bölgeleri Katılımcı sıfatı özel DNS bölgelerine yazma izinleri vardır. RBAC rolleri hakkında daha fazla bilgi için [Rol tabanlı erişim denetimine](../role-based-access-control/overview.md)bakın.

## <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-you-delete-the-virtual-machine"></a>Sanal makineyi sildiğinizde özel bölgedeki otomatik olarak kaydedilmiş sanal makine DNS kayıtları otomatik olarak silinir mi?

Evet. Otomatik kayıt etkinleştirilmiş bağlantılı bir sanal ağ içinde sanal bir makineyi silerseniz, kayıtlı kayıtlar otomatik olarak silinir.

## <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-linked-virtual-network-be-deleted-manually"></a>Bağlı bir sanal ağdan özel bir bölgede otomatik olarak kaydedilmiş bir sanal makine kaydı el ile silinebilir mi?

Evet. Bölgede el ile oluşturulan bir DNS kaydıyla otomatik olarak kaydedilmiş DNS kayıtlarının üzerine yazabilirsiniz. Aşağıdaki soru ve cevap adresi bu konu.

## <a name="what-happens-when-i-try-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-linked-virtual-network"></a>Bağlı bir sanal ağda otomatik olarak kaydedilmiş bir sanal makineyle aynı ana bilgisayar ada sahip özel bir bölgeye yeni bir DNS kaydını el ile oluşturmaya çalıştığımda ne olur?

Bağlı bir sanal ağda varolan, otomatik olarak kayıtlı bir sanal makineyle aynı ana bilgisayar ada sahip özel bir bölgeye yeni bir DNS kaydını el ile oluşturmaya çalışırsınız. Bunu yaptığınızda, yeni DNS kaydı otomatik olarak kaydedilen sanal makine kaydının üzerine yazar. El ile oluşturulan bu DNS kaydını bölgeden yeniden silmeye çalışırsanız, silme işlemi başarılı olur. Otomatik kayıt, sanal makine hala var olduğu ve ona bağlı özel bir IP'si olduğu sürece tekrar olur. DNS kaydı bölgede otomatik olarak yeniden oluşturulur.

## <a name="what-happens-when-we-unlink-a-linked-virtual-network-from-a-private-zone-will-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-too"></a>Bağlantılı bir sanal ağın bağlantısını özel bir bölgeden çözersek ne olur? Sanal ağdan otomatik olarak kaydedilmiş sanal makine kayıtları da bölgeden kaldırılacak mı?

Evet. Bağlı bir sanal ağın bağlantısını özel bir bölgeden kaldırmak için, ilişkili sanal ağ bağlantısını kaldırmak için DNS bölgesini güncelleştirebilirsiniz. Bu işlemde, otomatik olarak kaydedilmiş sanal makine kayıtları bölgeden kaldırılır.

## <a name="what-happens-when-we-delete-a-linked-virtual-network-thats-linked-to-a-private-zone-do-we-have-to-manually-update-the-private-zone-to-unlink-the-virtual-network-as-a-linked-virtual-network-from-the-zone"></a>Özel bir bölgeye bağlı bağlantılı bir sanal ağı sildiğimizde ne olur? Sanal ağı bölgeden bağlantılı bir sanal ağ olarak bağlamak için özel bölgeyi el ile güncelleştirmemiz gerekiyor mu?

Hayır. Bağlı bir sanal ağı önce özel bir bölgeden bağlamadan sildiğinizde, silme işleminiz başarılı olur ve DNS bölgesine bağlantılar otomatik olarak temizlenir.

## <a name="will-dns-resolution-by-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-privatecontosocom-is-linked-to-a-virtual-network"></a>Varsayılan FQDN 'yi (internal.cloudapp.net) kullanarak DNS çözümü, özel bir bölge (örneğin, private.contoso.com) sanal bir ağa bağlı olsa bile çalışmaya devam eder mi?

Evet. Özel Bölgeler, Varsayılan Azure sağlanan internal.cloudapp.net bölgesinin yerini alamaz. İster Azure tarafından sağlanan internal.cloudapp.net ister kendi özel bölgenize güvenin, çözüme kavuşturmak istediğiniz bölgenin FQDN'sini kullanın.

## <a name="will-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>Bağlı bir sanal ağ daki sanal makinelerdeki DNS soneki özel bölgeninkiyle değiştirilecek mi?

Hayır. Bağlı sanal ağınızdaki sanal makinelerdeki DNS soneki varsayılan Azure tarafından sağlanan sonek ("*.internal.cloudapp.net") olarak kalır. Sanal makinelerinizdeki bu DNS ekini el ile özel bölgeninkiyle değiştirebilirsiniz.
Bu sonek nasıl değiştirilen hakkında rehberlik için [kendi DNS sunucunuzda ana bilgisayar adlarını kaydetmek için dinamik DNS kullanın](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-ddns#windows-clients) bakın

## <a name="what-are-the-usage-limits-for-azure-dns-private-zones"></a>Azure DNS Özel bölgeleri için kullanım sınırları nelerdir?

Azure DNS özel bölgeleri için kullanım sınırları yla ilgili ayrıntılar için [Azure DNS sınırlarına](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-dns-limits) bakın.

## <a name="why-dont-my-existing-private-dns-zones-show-up-in-new-portal-experience"></a>Neden mevcut özel DNS bölgelerim yeni portal deneyiminde görünmüyor?

Varolan özel DNS bölgeniz önizleme API'si kullanılarak oluşturulduysa, bu bölgeleri yeni kaynak modeline geçirmeniz gerekir. Önizleme API'si kullanılarak oluşturulan özel DNS bölgeleri yeni portal deneyiminde gösterilmez. Yeni kaynak modeline nasıl geçirilir yönergeleri için aşağıya bakın.

## <a name="how-do-i-migrate-my-existing-private-dns-zones-to-the-new-model"></a>Varolan özel DNS bölgelerimi yeni modele nasıl geçirebilirim?

En kısa zamanda yeni kaynak modeline geçiş yapmanızı şiddetle öneririz. Eski kaynak modeli desteklenir, ancak bu modelin üstünde daha fazla özellik geliştirilmeyecektir. Gelecekte, yeni kaynak modeli lehine amortismanniyetindeyiz. Varolan özel DNS bölgelerinizi yeni kaynak modeline nasıl geçirteceklerine ilişkin kılavuz için[Azure DNS özel bölgeleri için geçiş kılavuzuna](private-dns-migration-guide.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Özel DNS hakkında daha fazla bilgi edinin](private-dns-overview.md)
