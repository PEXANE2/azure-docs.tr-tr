---
title: Azure DNS ile DNS kayıt kümelerini ve kayıtlarını yönetme
description: Azure DNS, etki alanınızı barındırırken DNS kayıt kümelerini ve kayıtlarını yönetme olanağı sağlar.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 10/6/2018
ms.author: rohink
ms.openlocfilehash: 4012b32eb2684126b8dc64b9e86bf35a016e9fba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76936834"
---
# <a name="manage-dns-records-and-record-sets-by-using-the-azure-portal"></a>Azure portalını kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme

Bu makalede, Azure portalını kullanarak DNS bölgenizdeki kayıt kümelerini ve kayıtları nasıl yönetebildiğinizi gösterir.

DNS kayıt kümeleri ile tek tek DNS kayıtları arasındaki farkı anlamak önemlidir. Kayıt kümesi, aynı ada sahip ve aynı türdeki bir bölgede bulunan kayıtların koleksiyonudur. Daha fazla bilgi için Azure [portalını kullanarak DNS kayıt kümeleri ve kayıtları oluştur'a](dns-getstarted-create-recordset-portal.md)bakın.

## <a name="create-a-new-record-set-and-record"></a>Yeni bir kayıt kümesi oluşturma ve kaydetme

Azure portalında ayarlanmış bir kayıt oluşturmak için Azure [portalını kullanarak DNS kayıtları oluştur'a](dns-getstarted-create-recordset-portal.md)bakın.

## <a name="view-a-record-set"></a>Kayıt kümesini görüntüleme

1. Azure portalında **DNS bölge** bıçağına gidin.
2. Kayıt kümesini arayın ve seçin. Bu, kayıt kümesi özelliklerini açar.

    ![Kayıt kümesini arama](./media/dns-operations-recordsets-portal/searchset500.png)

## <a name="add-a-new-record-to-a-record-set"></a>Kayıt kümesine yeni bir kayıt ekleme

Herhangi bir kayıt kümesine en fazla 20 kayıt ekleyebilirsiniz. Bir kayıt kümesi iki özdeş kayıt içeremez. Boş kayıt kümeleri (sıfır kayıtla) oluşturulabilir, ancak Azure DNS ad sunucularında görünmez. CNAME türünün kayıt kümeleri en fazla bir kayıt içerebilir.

1. DNS bölgeniz için **Kayıt kümesi özellikleri** bıçağında, kayıt eklemek istediğiniz kayıt kümesini tıklatın.

    ![Kayıt kümesi ni seçme](./media/dns-operations-recordsets-portal/selectset500.png)

2. Alanları doldurarak kayıt kümesi özelliklerini belirtin.

    ![Kayıt ekleme](./media/dns-operations-recordsets-portal/addrecord500.png)

3. Ayarlarınızı kaydetmek için bıçağın üst **kısmındakaydet'i** tıklatın. O zaman bıçağı kapat.
4. Köşede, kaydın kaydedildiğini göreceksiniz.

    ![Kayıt kümesini kaydetme](./media/dns-operations-recordsets-portal/saving150.png)

Kayıt kaydedildikten sonra, **DNS bölge** bıçak üzerindeki değerler yeni kaydı yansıtır.

## <a name="update-a-record"></a>Kaydı güncelleştirme

Varolan bir kayıt kümesinde bir kaydı güncelleştirdiğinizde, güncelleştirebileceğiniz alanlar, çalıştığınız kaydın türüne bağlıdır.

1. Kayıt **seti nizde,** kayıt setiniz için özellikler blade'inde, kaydı arayın.
2. Kaydı değiştirin. Bir kaydı değiştirdiğinizde, kayıt için kullanılabilir ayarları değiştirebilirsiniz. Aşağıdaki örnekte, **IP adresi** alanı seçilir ve IP adresi değiştirilme sürecindedir.

    ![Bir kaydı değiştirme](./media/dns-operations-recordsets-portal/modifyrecord500.png)

3. Ayarlarınızı kaydetmek için bıçağın üst **kısmındakaydet'i** tıklatın. Sağ üst köşede, kaydın kaydedildiğine dair bildirimi görürsünüz.

    ![Kaydedilen kayıt kümesi](./media/dns-operations-recordsets-portal/saved150.png)

Kayıt kaydedildikten sonra, **DNS bölge** bıçak üzerinde ayarlanan kayıt için değerler güncelleştirilmiş kaydı yansıtır.

## <a name="remove-a-record-from-a-record-set"></a>Kayıt kümesinden kayıt kaldırma

Kayıtları bir kayıt kümesinden kaldırmak için Azure portalını kullanabilirsiniz. Son kaydı bir kayıt kümesinden kaldırmanın kayıt kümesini silmediğini unutmayın.

1. Kayıt **seti nizde,** kayıt setiniz için özellikler blade'inde, kaydı arayın.
2. Kaldırmak istediğiniz kaydı tıklatın. Ardından **Kaldır'ı**seçin.

    ![Kaydı kaldırma](./media/dns-operations-recordsets-portal/removerecord500.png)

3. Ayarlarınızı kaydetmek için bıçağın üst **kısmındakaydet'i** tıklatın.
4. Kayıt kaldırıldıktan sonra, **DNS bölge** bıçak üzerindeki kayıt değerleri kaldırma yansıtacak.

## <a name="delete-a-record-set"></a><a name="delete"></a>Kayıt kümesini silme

1. Kayıt **seti özellikleri** bıçak üzerinde kayıt kümesi için, **Sil'i**tıklatın.

    ![Kayıt kümesini silme](./media/dns-operations-recordsets-portal/deleterecordset500.PNG)

2. Kayıt kümesini silmek isteyip istemediğiniz soran bir ileti görüntülenir.
3. Adın silmek istediğiniz kayıt kümesiyle eşleştiğini doğrulayın ve ardından **Evet'i**tıklatın.
4. **DNS bölge** bıçaklarında, kayıt kümesinin artık görünür olmadığını doğrulayın.

## <a name="work-with-ns-and-soa-records"></a>NS ve SOA kayıtlarıyla çalışma

Otomatik olarak oluşturulan NS ve SOA kayıtları diğer kayıt türlerinden farklı olarak yönetilir.

### <a name="modify-soa-records"></a>SOA kayıtlarını değiştirme

Bölge tepe noktasında otomatik olarak oluşturulan SOA kaydından kayıt ekleyemez veya\@kaldıramazsınız (ad = " "). Ancak, SOA kaydı ("Ana Bilgisayar" hariç) ve TTL kayıt kümesindeki parametrelerden herhangi birini değiştirebilirsiniz.

### <a name="modify-ns-records-at-the-zone-apex"></a>Bölge tepe noktasındaki NS kayıtlarını değiştirme

Bölge tepe noktasında ayarlanan NS kaydı, her DNS bölgesiyle otomatik olarak oluşturulur. Bölgeye atanan Azure DNS ad sunucularının adlarını içerir.

Birden fazla DNS sağlayıcısıyla birlikte barındırılabilen etki alanlarını desteklemek için bu NS kayıt kümesine ek ad sunucuları ekleyebilirsiniz. Bu kayıt kümesi için TTL ve meta verilerini de değiştirebilirsiniz. Ancak, önceden doldurulmuş Azure DNS ad sunucularını kaldıramaz veya değiştiremezsiniz.

Bunun yalnızca bölge tepe noktasında ayarlanan NS kaydı için geçerli olduğunu unutmayın. Bölgenizdeki diğer NS kayıt kümeleri (alt bölgeleri temsilcilendirmek için kullanılır) kısıtlama olmaksızın değiştirilebilir.

### <a name="delete-soa-or-ns-record-sets"></a>SOA veya NS kayıt kümelerini silme

Bölge oluşturulduğunda otomatik olarak oluşturulan SOA ve NS kayıt\@kümelerini bölge tepe noktasında (ad = " ") silemezsiniz. Bölgeyi sildiğinizde otomatik olarak silinirler.

## <a name="next-steps"></a>Sonraki adımlar

* Azure DNS hakkında daha fazla bilgi için [Azure DNS genel bakış](dns-overview.md)bilgisine bakın.
* DNS'yi otomatikleştirmek hakkında daha fazla bilgi için [.NET SDK'yı kullanarak DNS bölgeleri ve kayıt kümeleri oluşturma'ya](dns-sdk.md)bakın.
* Ters DNS kayıtları hakkında daha fazla bilgi için [Azure'da ters DNS'ye genel bakış ve destek](dns-reverse-dns-overview.md)bölümüne bakın.
* Azure DNS diğer ad kayıtları hakkında daha fazla bilgi için Azure [DNS diğer adı kayıtlarına genel bakış'a](dns-alias.md)bakın.
