---
title: Varolan bir Ön Kapı - Azure portalına bir kök veya tepe etki alanı
description: Azure portalını kullanarak varolan bir Ön Kapı'ya kök veya tepe etki alanında nasıl bindirilen öğrenin.
services: front-door
author: sharad4u
ms.service: frontdoor
ms.topic: article
ms.date: 5/21/2019
ms.author: sharadag
ms.openlocfilehash: 4b74338f22a82d76ef13126ee0862b841bd89a99
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878893"
---
# <a name="onboard-a-root-or-apex-domain-on-your-front-door"></a>Ön Kapınızda bir kök veya tepe etki alanı
Azure Ön Kapı, özel etki alanlarının bindirilmesi için etki alanı sahipliğini doğrulamak için CNAME kayıtlarını kullanır. Ayrıca, Ön Kapı, Ön Kapı profilinizle ilişkili ön uç IP adresini ortaya çıkarmaz ve bu nedenle amaç Azure Ön Kapı'ya dahil etmekse, apeks etki alanınızı bir IP adresiyle eşlemezsiniz.

DNS protokolü, bölge tepe noktasında CNAME kayıtlarının atanmasını engeller. Örneğin, etki `contoso.com`alanınız; için `somelabel.contoso.com`CNAME kayıtları oluşturabilirsiniz; ancak kendisi için `contoso.com` CNAME oluşturamaz. Bu kısıtlama, Azure Ön Kapı'nın arkasında yük dengeli uygulamaları olan uygulama sahipleri için bir sorun teşkil eder. Ön Kapı profili kullanmak bir CNAME kaydı oluşturulmasını gerektirdiğinden, bölge tepesinden Ön Kapı profilini işaret etmek mümkün değildir.

Bu sorun, Azure DNS'deki diğer ad kayıtları kullanılarak çözülür. CNAME kayıtlarının aksine, bölge tepe noktasında takma ad kayıtları oluşturulur ve uygulama sahipleri bölge uçlarını ortak uç noktaları olan bir Ön Kapı profiline işaret etmek için bunu kullanabilir. Uygulama sahipleri, DNS bölgesi içindeki herhangi bir başka etki alanı için kullanılan aynı Ön Kapı profiline işaret eder. Örneğin, `contoso.com` ve `www.contoso.com` aynı Ön Kapı profiline işaret edebilir. 

Ön Kapı profilinize tepe veya kök etki alanınızı eşleme temel olarak CNAME düzleşme veya DNS takibi gerektirir, bu da DNS sağlayıcısında bir IP adresine ulaşana kadar CNAME girişini özyinelemeli şekilde çözen bir mekanizmadır. Bu işlevsellik, Ön Kapı uç noktaları için Azure DNS tarafından desteklenir. 

> [!NOTE]
> CNAME düzlemesi veya DNS takibini destekleyen başka DNS sağlayıcıları da vardır, ancak Azure Ön Kapı, etki alanlarını barındırmak için müşterileri için Azure DNS kullanmanızı önerir.

Azure portalını Ön Kapınızdaki bir tepe etki alanında kullanmak ve TLS sonlandırma sertifikasıyla ilişkilendirerek https'yi etkinleştirebilirsiniz. Apex etki alanları kök veya çıplak etki alanları olarak da adlandırılır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Ön Kapı profilinizi gösteren bir takma ad kaydı oluşturma
> * Kök etki alanını Ön Kapıya ekleme
> * Kök etki alanında HTTPS kurulumu

> [!NOTE]
> Bu öğretici, zaten oluşturulan bir Ön Kapı profili olması gerekir. [Quickstart:](./quickstart-create-front-door.md) Create a Front Door veya http [ile HTTPS yeniden yönlendirmesi ile](./front-door-how-to-redirect-https.md) bir Ön Kapı oluşturun gibi diğer öğreticilere bakın.

## <a name="create-an-alias-record-for-zone-apex"></a>Bölge apeksi için takma ad kaydı oluşturma

1. Etki alanının yerleşik olması için **Azure DNS** yapılandırmasını açın.
2. Bölge tepe cihazı için kayıt oluşturun veya edin.
3. Kayıt **türünü** _A_ kaydı olarak seçin ve ardından Diğer Ad **kayıt kümesi**için _Evet'i_ seçin. **Diğer ad türü** _Azure kaynağına_ayarlanmalıdır.
4. Ön Kapı profilinizin barındırıldığı Azure aboneliğini seçin ve ardından **Azure kaynak** açılır düşüşünden Ön Kapı kaynağını seçin.
5. Değişikliklerinizi göndermek için **Tamam'ı** tıklatın.

    ![Bölge tepe alanı için takma ad kaydı](./media/front-door-apex-domain/front-door-apex-alias-record.png)

6. Yukarıdaki adım, Ön Kapı kaynağınızı gösteren bir bölge apeksi kaydı ve ayrıca Ön Kapı `afdverify.contosonews.com`profilinizdeki `afdverify.<name>.azurefd.net` etki alanının onboarding için kullanılacak bir CNAME kaydı 'afdverify' (örnek - ) eşleme oluşturur.

## <a name="onboard-the-custom-domain-on-your-front-door"></a>Ön Kapınızdaki özel etki alanında yerleşik

1. Ön Kapı tasarımcısı sekmesinde, yeni bir özel etki alanı eklemek için Frontend ana bilgisayarları bölümündeki '+' simgesine tıklayın.
2. Özel ana bilgisayar adı alanına kök veya tepe `contosonews.com`etki alanı adını girin, örnek .
3. Etki alanından Ön Kapınıza cname eşleme doğrulandıktan sonra, özel etki alanını eklemek için **Ekle'yi** tıklatın.
4. Değişiklikleri göndermek için **Kaydet'i** tıklatın.

![Özel etki alanı menüsü](./media/front-door-apex-domain/front-door-onboard-apex-domain.png)

## <a name="enable-https-on-your-custom-domain"></a>Özel etki alanınızda HTTPS'yi etkinleştirme

1. Eklenen özel etki alanına tıklayın ve **Özel etki alanı HTTPS**bölümünün altında, durumu **Etkin**olarak değiştirin.
2. _'Kendi sertifikamı kullan'_ için **Sertifika yönetim türünü** seçin.

> [!WARNING]
> Ön Kapı yönetilen sertifika yönetim türü şu anda tepe veya kök etki alanları için desteklenmez. Ön Kapı için bir tepe veya kök etki alanında HTTPS'yi etkinleştirmek için kullanılabilen tek seçenek, Azure Key Vault'ta barındırılan kendi özel TLS/SSL sertifikanızı kullanmaktır.

3. Bir sonraki adıma geçmeden önce, Web-Üfe'de belirtildiği gibi, Ön Kapı'nın anahtar Vault'unuza erişmesi için doğru izinlere sahip olduğundan emin olun.
4. Geçerli aboneliğinizden bir **Key Vault hesabı** seçin ve ardından doğru sertifikayla eşlenecek uygun **Gizli** ve **Gizli sürümünü** seçin.
5. Seçimi kaydetmek için **Güncelleştir'e** tıklayın ve ardından **Kaydet'i**tıklatın.
6. Birkaç dakika sonra **Yenile'yi** tıklatın ve ardından sertifika sağlamanın ilerlemesini görmek için özel etki alanını yeniden tıklatın. 

> [!WARNING]
> Apeks etki alanınız için uygun yönlendirme kuralları oluşturduğunuzdan veya etki alanını varolan yönlendirme kurallarına eklediğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.