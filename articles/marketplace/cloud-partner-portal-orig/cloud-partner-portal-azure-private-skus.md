---
title: Özel SK'ler ve planlar | Azure Marketi
description: Teklifin kullanılabilirliğini yönetmek için özel SNU'lar nasıl kullanılır?
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/15/2019
ms.author: dsindona
ms.openlocfilehash: ee3ab7be4d15b13a3c0bb014a3ca4d4096299b4c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280397"
---
<a name="private-skus-and-plans"></a>Özel SKS ve Planları
============

Özel SNU'lar, SNU'ların kullanılabilirliğini belirli müşterilerle sınırlamanızı sağlar. Bir SKU özel olarak işaretlendiğinde, [Azure Marketi](https://azuremarketplace.microsoft.com) ve [Azure portalı](https://portal.azure.com)da dahil olmak üzere hiçbir genel katalogda kullanılamaz. Azure portalında, yalnızca SKU erişimi olan müşteriler bunu görebilir. Ayrıca, özel tekliflere erişimleri de istenenir.

>[!NOTE]
>Özel SKU'ların, kamu SKU'larınızla herhangi bir çatışmayı önlemek için yeni benzersiz SKU/Plan Id'lerine sahip olması gerekir.

Aşağıdaki senaryoları işlemek için özel SNU'ları kullanabilirsiniz:

1.  Yalnızca belirli müşteriler tarafından herkese açık olmasını istediğiniz ve herkese açık olmayan yazılımları yayımlayın.
2.  Belirli müşteriler için özelleştirilmiş bir fiyatla ortak yazılım varyasyonları yayımlayın.
3.  Özelleştirilmiş bir açıklama ve terimle (yeni teklif yoluyla) ortak yazılımın varyasyonlarını yayımlayın.

Yalnızca fiyatı değiştirmek istiyorsanız, aynı teklifte başka bir SKU'daki diskleri yeniden kullanabilirsiniz. Özel SK'ler ile SK'ler arasında diskleri yeniden göndermeniz gerekmez.

<a name="mark-a-sku-private"></a>Mark bir SKU özel
---------------------

Bir SKU'yu özel olarak işaretlemek için, SKU'nun özel olup olmadığını sorma seçeneğini belirleyin:

![Özel olarak bir SKU işaretleyin](./media/cloud-partner-portal-publish-virtual-machine/markingskuprivate.png)

Diskleri başka bir SKU'da yeniden kullanabilir ve fiyatlandırmayı veya açıklamayı değiştirebilirsiniz. Diskleri yeniden kullanmak **için,"Bu** SKU görüntüleri genel bir SKU'dan yeniden kullanır mı" istemlerine yanıt olarak Evet'i seçin.

SKU özel olarak işaretlenmişse ve teklifte yeniden kullanılabilir disklere sahip başka SKU'lar varsa, SKU'nun diskleri başka bir SKU'dan yeniden kullandığını belirtmeniz gerekir. Ayrıca özel SKU için hedef kitleyi belirtmeniz gerekir.

>[!NOTE]
>Yayımlandıktan sonra, halka açık bir SKU özel hale getirilemeyecek.

<a name="select-an-image"></a>Resim seçin
------------------

Özel SKU için yeni diskler sağlayabilir veya başka bir SKU'da zaten sağlanan aynı diskleri yeniden kullanabilir, yalnızca fiyatlandırmayı veya açıklamayı değiştirebilirsiniz. Diskleri yeniden kullanmak **için,"Bu** SKU resmi ortak bir SKU'dan yeniden kullanır mı" istemlerine yanıt olarak Evet'i seçin.

![Görüntünün yeniden kullanımını göster](./media/cloud-partner-portal-publish-virtual-machine/selectimage1.png)

SKU'nun görüntüleri yeniden kullandığını doğruladıktan sonra, görüntüler için kaynak veya *temel* SKU'yu seçin:

![Resim seçin](./media/cloud-partner-portal-publish-virtual-machine/selectimage2.png)

Teklifi yayınladığınızda, seçilen SKU'dan görüntüler özel sku kimliği altında özel fiyatlar/terimlerle birlikte kullanıma sunulacaktır. Özel SKU sadece hedef kitle tarafından görülebilir.

Görüntü güncelleştirmeleri için yalnızca temel SKU'nun görüntüsünü güncelleştirmeniz gerekir. Perde arkasında, özel SKU için görüntü de otomatik olarak güncellenecektir. Benzer şekilde, görüntüyü altta yatan SKU'dan silerseniz, görüntü özel SKU'dan da kaldırılır.

<a name="restricting-the-audience"></a>İzleyiciyi kısıtlama
------------------------

Özel teklifler yalnızca hedeflenen kullanıcılar tarafından bulunabilir ve dağıtılabilir.
Şu anda abonelik kimlikleri kullanarak kullanıcıları hedeflemeyi destekliyoruz.

Bu **abonelikler, 10 abonelik için**manuel giriş formu aracılığıyla veya **20.000'e kadar abonelik**sağlayan bir CSV dosyası yükleyerek girilebilir.

Kısıtlı hedef kitle için Manuel Giriş:

![Hedef kitleyi el ile sınırlandırın](./media/cloud-partner-portal-publish-virtual-machine/restrictaudience1.png)

Kısıtlı hedef kitle için CSV Yükleme:

![Hedef kitleyi kısıtlamak için CSV'yi kullanın](./media/cloud-partner-portal-publish-virtual-machine/restrictaudience2.png)

Örnek CSV dosya içeriği:

            Type,Id,Description
            SubscriptionId,7738d703-3135-4e8d-8b81-1e70379abd9d,Private Customer

Manuel girişten CSV yükleme görünümüne veya CSV'den manuel girişe geçtiğiniz zaman, SKU erişimi olan eski abonelik kimlikleri listesi tutulmaz. Bir uyarı görüntülenir ve liste yalnızca teklifi kaydedinden sonra üzerine yazılır.

<a name="managing-private-audiences"></a>Özel hedef kitlelerini yönetme
-------------------------

**Teklifi yeniden yayınlamadan hedef kitleyi güncelleştirmek için istediğiniz hedef kitle değişikliklerini yaparsınız (Kullanıcı UI veya API'yi kullanarak) ve ardından "Özel Hedef Kitlelerini Eşitle" eylemini başlatırsınız.**

Hedef kitleniz 10 veya daha az abonelikse, cpp ue'yi kullanarak bu aboneliği tamamen yönetebilirsiniz.

Hedef kitleniz 10'dan fazla abonelikse, bunu CPP UI'ye yükleyebileceğiniz bir CSV dosyasını veya API'yi kullanarak yönetebilirsiniz.

API kullanıyorsanız ve bir CSV dosyasını korumak istemiyorsanız, aşağıdaki talimatlara göre doğrudan API kullanarak hedef kitleyi yönetebilirsiniz.

> [!NOTE]
> Özel teklifinize bir hedef kitle eklemek için Azure abonelik kimliğini (Planlar ve SK'ler) veya Kiracı Kimliği'ni (yalnızca Planlar) kullanın.

###  <a name="managing-subscriptions-with-the-api"></a>API ile abonelikleri yönetme

API'yi csv yüklemek veya hedef kitlenizi doğrudan yönetmek için (CSV kullanmadan) kullanabilirsiniz. Genel olarak, yalnızca teklifinizi almanız, `restrictedAudience` nesneyi güncellemeniz ve ardından hedef kitle üyelerini eklemek veya kaldırmak için bu değişiklikleri teklifinize geri göndermeniz yeterlidir.

Hedef kitle listenizi şu şekilde programa göre güncelleştirebilirsiniz:

1. [Teklif verilerinizi alın:](cloud-partner-portal-api-retrieve-specific-offer.md)

    ```
    GET https://cloudpartner.azure.com/api/publishers//offers/?api-version=2017-10-31&includeAllPricing=true
    ```

2. Bu JPath sorgusunu kullanarak teklifin her SKU'sunda kısıtlı hedef kitle nesnelerini bulun:

    ```
    $.definition.plans[*].restrictedAudience
    ```
3. Teklifiniz için kısıtlanmış hedef kitle nesnesini güncelleştirin.

    **CSV dosyasından özel teklifiniz için abonelik listesini ilk olarak yüklediyseniz:**

    *KısıtlıHedef* nesneniz (ler) bu şekilde görünecektir.
    ```
    "restrictedAudience": {
                  "uploadedCsvUri": "{SasUrl}"
    }
    ```

    Her sınırlı hedef kitle nesnesi için:

    a. İçeriği indirin. `restrictedAudience.uploadedCsvUri` İçerik sadece üstbilgi içeren bir CSV dosyasıdır. Örnek:

        type,id,description
        subscriptionId,541a269f-3df2-486e-8fe3-c8f9dcf28205,sub1
        subscriptionId,c0da499c-25ec-4e4b-a42a-6e75635253b9,sub2

    b. Gerektiğinde indirilen CSV dosyasına abonelik ekleme veya silme.

    c. Güncelleştirilmiş CSV dosyasını [Azure Blob depolama](../../storage/blobs/storage-blobs-overview.md) veya [OneDrive](https://onedrive.live.com)gibi bir konuma yükleyin ve dosyanıza salt okunur bağlantı oluşturun. Bu yeni *SasUrl*olacak .

    d. `restrictedAudience.uploadedCsvUri` Yeni *SasUrl*ile anahtarı güncelleyin.

    **Cloud Partner Portal'dan özel teklifiniz için orijinal abonelik listesini el ile girdiyseniz:**

    *KısıtlıHedef* nesneniz (ler) şuna benzer:

    ```
    "restrictedAudience": {
        "manualEntries": [{
            "type": "subscriptionId",
            "id": "541a269f-3df2-486e-8fe3-c8f9dcf28205",
            "description": "sub1"
            }, {
            "type": "subscriptionId",
            "id": "c0da499c-25ec-4e4b-a42a-6e75635253b9",
            "description": "sub2"
            }
        ]}
    ```

    a. Her kısıtlı hedef kitle nesnesi `restrictedAudience.manualEntries` için, gerektiğinde listedeki girişleri ekleyin veya silin.

4. Özel teklifinizin her SKU'su için tüm *kısıtlı Hedef Kitle* nesnelerini güncellemeyi bitirdiğinde, teklifi [güncelleyin:](cloud-partner-portal-api-creating-offer.md)

    ```
    PUT https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>?api-version=2017-10-31
    ```
    Bununla birlikte, güncelleştirilmiş hedef kitle listeniz artık geçerli olur.

<a name="previewing-private-offers"></a>Özel tekliflerin önizlemesi
-------------------------

Önizleme/evreleme adımı sırasında, SKU'ya yalnızca teklif düzeyi önizleme abonelikleri erişebilir. Bu test aşamasında, teklifi hedef müşterilerinize göründüğü gibi önizleyebilirsiniz.

Aşamalı tekliflere erişmek için Teklif Düzeyi Önizleme Abonelikleri:

![Abonelik Kimliklerini Önizleme](./media/cloud-partner-portal-publish-virtual-machine/previewoffer1.png)

Teklif canlı olarak dağıtıldıktan sonra, özel SKU'yu yalnızca kısıtlı hedef kitle abonelikleri (manuel giriş veya CSV ile girilebilir) görüntüleyebilir ve dağıtabilecektir. Doğrulama amacıyla özel SKU için **her zaman kendi aboneliklerinizi kısıtlı hedef kitleye eklemenizi** öneririz.

>[!NOTE]
>Hata ayıklama amacıyla, Microsoft destek ve mühendislik ekipleri de bu özel tekliflere erişebilir.
