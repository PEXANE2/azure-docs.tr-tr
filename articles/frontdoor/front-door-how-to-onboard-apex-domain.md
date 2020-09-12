---
title: Var olan bir ön kapıya bir kök veya tepesinde etki alanı ekleme-Azure portal
description: Azure portal kullanarak var olan bir ön kapıya bir kök veya tepesinde etki alanı eklemeyi öğrenin.
services: front-door
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 5/21/2019
ms.author: duau
ms.openlocfilehash: 05267ad43f6e7f89ec50b1765d2475a02fae1702
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89399605"
---
# <a name="onboard-a-root-or-apex-domain-on-your-front-door"></a>Front Door’unuza kök veya apex etki alanı ekleme
Azure ön kapısı, özel etki alanlarını eklemek için etki alanı sahipliğini doğrulamak üzere CNAME kayıtlarını kullanır. Ayrıca, ön kapı, ön kapı profilinizle ilişkili ön uç IP adresini kullanıma sunmaz ve amaç Azure ön kapısına eklemek için tepesinde etki alanınızı bir IP adresiyle eşleyemezsiniz.

DNS protokolü, CNAME kayıtlarının tepesinde bölgesinde atanmasını engeller. Örneğin, etki alanınız ise `contoso.com` ; IÇIN CNAME kayıtları oluşturabilirsiniz, `somelabel.contoso.com` ancak kendısı için CNAME oluşturamazsınız `contoso.com` . Bu kısıtlama, Azure ön kapısının arkasında yük dengeli uygulamalara sahip olan uygulama sahipleri için bir sorun gösterir. Bir ön kapı profili kullanmak için bir CNAME kaydı oluşturulması gerektiğinden, bölge tepesinde ön kapı profilini işaret etmek mümkün değildir.

Bu sorun Azure DNS diğer ad kayıtları kullanılarak çözülebilir. CNAME kayıtlarının aksine, diğer ad kayıtları bölge tepesinde kayıtlarını ortak uç noktalara sahip bir ön kapılı profile işaret etmek için kullanabilir. Uygulama sahipleri, DNS bölgesi içindeki diğer etki alanı için kullanılan aynı ön kapı profilini işaret. Örneğin, `contoso.com` ve `www.contoso.com` aynı ön kapı profilini işaret edebilir. 

Tepesinde veya kök etki alanınızı ön kapı profilinize eşleyerek, DNS sağlayıcının bir IP adresi ile eşleşene kadar CNAME girişini yinelemeli olarak çözümlediği bir mekanizma olan CNAME düzleştirme veya DNS oluşturma gerekir. Bu işlevsellik, ön kapı uç noktaları için Azure DNS desteklenir. 

> [!NOTE]
> CNAME düzleştirme veya DNS ile izleme desteği sunan başka DNS sağlayıcıları da vardır, ancak Azure ön kapısı, kendi etki alanlarını barındırmak için müşterilerinin Azure DNS kullanılmasını önerir.

Ön kapıya bir tepesinde etki alanı eklemek ve bunu TLS sonlandırmasına yönelik sertifikayla ilişkilendirerek HTTPS 'yi etkinleştirmek için Azure portal kullanabilirsiniz. Tepesinde etki alanları, kök veya Naked etki alanları olarak da adlandırılır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Ön kapı profilinize işaret eden bir diğer ad kaydı oluşturma
> * Kök etki alanını ön kapıya ekleme
> * Kök etki alanında HTTPS kurulumu

> [!NOTE]
> Bu öğreticide, önceden oluşturulmuş bir ön kapı profili olması gerekir. Başlamak için [hızlı başlangıç: ön kapı oluşturma](./quickstart-create-front-door.md) veya [http ile https yönlendirmesi arasında ön kapı oluşturma](./front-door-how-to-redirect-https.md) gibi diğer öğreticiler hakkında daha fazla öğreticilere bakın.

## <a name="create-an-alias-record-for-zone-apex"></a>Bölge tepesinde için diğer ad kaydı oluşturma

1. Etki alanının eklendi olması için **Azure DNS** yapılandırma açın.
2. Bölge tepesinde için kayıt oluşturun veya düzenleyin.
3. Kayıt **türünü** _bir_ kayıt olarak seçin ve ardından **diğer ad kayıt kümesi**için _Evet_ ' i seçin. **Diğer ad türü** _Azure kaynağı_olarak ayarlanmalıdır.
4. Ön kapı profilinizin barındırıldığı Azure aboneliğini seçin ve ardından **Azure Kaynak** açılan listesinden ön kapı kaynağını seçin.
5. Değişikliklerinizi göndermek için **Tamam** ' ı tıklatın.

    ![Bölge tepesinde için diğer ad kaydı](./media/front-door-apex-domain/front-door-apex-alias-record.png)

6. Yukarıdaki adım, ön kapı kaynağınızı ve ayrıca `afdverify.contosonews.com` `afdverify.<name>.azurefd.net` ön kapı profilinizde etki alanını eklemek IÇIN kullanılacak CNAME kaydı eşlemesi ' afdverify ' (örnek-) olan bir bölge tepesinde kaydı oluşturacaktır.

## <a name="onboard-the-custom-domain-on-your-front-door"></a>Özel etki alanını ön kapıya ekleme

1. Ön kapı Tasarımcısı sekmesinde, ön uç konaklar bölümünde ' + ' simgesine tıklayarak yeni bir özel etki alanı ekleyin.
2. Özel ana bilgisayar adı alanına kök veya tepesinde etki alanı adını girin, örneğin `contosonews.com` .
3. Etki alanından ön kapıya yönelik CNAME eşlemesi doğrulandıktan sonra, özel etki alanını eklemek için **Ekle** ' ye tıklayın.
4. Değişiklikleri göndermek için **Kaydet** ' e tıklayın.

![Özel etki alanı menüsü](./media/front-door-apex-domain/front-door-onboard-apex-domain.png)

## <a name="enable-https-on-your-custom-domain"></a>Özel etki alanınız üzerinde HTTPS 'yi etkinleştirme

1. Eklenen özel etki alanına tıklayın ve **özel etki alanı https**bölümünde, durumu **etkin**olarak değiştirin.
2. _' Kendi sertifikamı kullan '_ olarak **sertifika yönetimi türünü** seçin.

> [!WARNING]
> Ön kapı tarafından yönetilen sertifika yönetimi türü, tepesinde veya kök etki alanları için şu anda desteklenmiyor. Ön kapıya yönelik bir tepesinde veya kök etki alanında HTTPS 'yi etkinleştirmek için kullanılabilen tek seçenek, Azure Key Vault barındırılan özel TLS/SSL sertifikanızı kullanmaktır.

3. Bir sonraki adıma geçmeden önce, ön kapıya Kullanıcı arabiriminde belirtildiği şekilde anahtar kasanıza erişmek için doğru izinleri oluşturduğunuzdan emin olun.
4. Geçerli aboneliğinizden bir **Key Vault hesabı** seçin ve ardından doğru sertifikayla eşlenecek uygun **gizli** ve **gizli sürümü** seçin.
5. Seçimi kaydetmek için **Güncelleştir** ' e tıklayın ve ardından **Kaydet**' e tıklayın.
6. Birkaç dakika sonra **Yenile** ' ye tıklayın ve ardından sertifika sağlama ilerleme durumunu görmek için özel etki alanı ' na tıklayın. 

> [!WARNING]
> Tepesinde etki alanınız için uygun yönlendirme kuralları oluşturduğunuzdan veya etki alanını mevcut yönlendirme kurallarına eklemiş olduğunuzdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.