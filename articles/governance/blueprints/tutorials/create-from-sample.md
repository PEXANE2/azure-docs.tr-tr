---
title: 'Öğretici: Yeni ortama blueprint örneği'
description: Bu öğreticide, iki kaynak grubu oluşturan ve her biri için bir rol ataması yapılandıran bir plan tanımı oluşturmak için bir plan örneği kullanırsınız.
ms.date: 03/25/2020
ms.topic: tutorial
ms.openlocfilehash: 79928822b41dfe0583afbeaf2f5f1a4a87dd3202
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80677084"
---
# <a name="tutorial-create-an-environment-from-a-blueprint-sample"></a>Öğretici: Plan örneğinden bir ortam oluşturma

Örnek planlar, Azure Planları kullanılarak neler yapılabileceğinin örneklerini sağlar. Her biri belirli bir amaç veya amaca sahip bir örnektir, ancak tek başlarına tam bir ortam oluşturmaz. Her biri, azure planlarını n içindahil olan yapı, tasarım ve parametrelerin çeşitli kombinasyonları ile birlikte kullanarak keşfetmek için bir başlangıç noktası olarak tasarlanmıştır.

Aşağıdaki öğretici, Azure Planları hizmetinin farklı yönlerini sergilemek için RBAC plan örneğine **sahip Kaynak Gruplarını** kullanır. Aşağıdaki adımlar kapsanmaktadır:

> [!div class="checklist"]
> - Örnekten yeni bir plan tanımı oluşturma
> - Örnek kopyanızı **Yayımlanmış** olarak işaretleme
> - Planın kopyasını varolan bir aboneye atama
> - Atama için dağıtılan kaynakları denetleme
> - Kilitleri kaldırmak için planı atama

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için bir Azure aboneliği gerekir. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="create-blueprint-definition-from-sample"></a>Örnekten plan tanımı oluşturma

İlk olarak, plan örneğini uygulayın. İçe aktarma, örneğine dayalı olarak ortamınızda yeni bir plan oluşturur.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Başlat'tan plan** _Oluştur'un_altındaki **Oluştur** düğmesini seçin.

1. _Diğer Örnekler_ altında RBAC plan örneği içeren **Kaynak Gruplarını** bulun ve seçin.

1. Plan örneğinin _Temellerini_ girin:

   - **Plan adı**: Plan örneğinin kopyası için bir ad verin. Bu öğretici _için, iki rgs-rol-atamaları ile_adını kullanacağız.
   - **Tanım yeri**: Elipsleri kullanın ve numunenin kopyasını kaydetmek için yönetim grubunu veya aboneliği seçin.

1. Sayfanın üst kısmındaki _Eserler_ sekmesini veya **Sonraki: Sayfanın** altındaki Eserler sekmesini seçin.

1. Plan örneğini oluşturan yapıtların listesini gözden geçirin. Bu örnek, _ProdRG_ ve _PreProdRG_ekran adlarını içeren iki kaynak grubu tanımlar. Her kaynak grubunun son adı ve konumu plan ataması sırasında ayarlanır. _ProdRG_ kaynak grubuna _Katılımcı_ rolü atanır ve _PreProdRG_ kaynak grubuna _Sahibi_ ve _Okuyucuları_ rolleri atanır. Tanımda atanan roller statiktir, ancak rol atanan kullanıcı, uygulama veya grup plan ataması sırasında ayarlanır.

1. Plan örneğini gözden geçirmeyi bitirdiğinizde **Taslağı Kaydet'i** seçin.

Bu adım, seçili yönetim grubunda veya abonelikte örnek plan tanımının bir kopyasını oluşturur. Kaydedilen plan tanımı sıfırdan oluşturulan herhangi bir plan gibi yönetilir. Örneği yönetim grubunuzun veya aboneliğinize gerektiği kadar kaydedebilirsiniz. Ancak, her kopyaya benzersiz bir ad verilmelidir.

**Planını Kaydetme tanımı başarılı** portal bildirimi göründükten sonra bir sonraki adıma geçin.

## <a name="publish-the-sample-copy"></a>Örnek kopyayı yayımla

Plan örneğinin kopyası artık ortamınızda oluşturuldu. **Taslak** modunda oluşturulur ve atanıp dağıtılmadan önce **yayımlanmalıdır.** Plan örneğinin kopyası ortamınıza ve gereksinimlerinize göre özelleştirilebilir. Bu öğretici için herhangi bir değişiklik yapmayacağız.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Blueprint tanımları** sayfasını seçin. _İki rgs-rol atamaları_ plan tanımını bulmak ve sonra seçmek için filtreleri kullanın.

1. Sayfanın üst kısmında **ki planı Yayımla'yı** seçin. Sağdaki yeni bölmede, plan örneğinin kopyası için _Sürüm'ü 1.0_ olarak sağlayın. **Version** Bu özellik, daha sonra bir değişiklik yaparsanız için yararlıdır. "RBAC plan örneğine sahip kaynak gruplarından yayınlanan ilk sürüm" gibi **Değişiklik notları** sağlayın. Ardından sayfanın altındaki **Yayımla'yı** seçin.

Bu adım, planı bir aboneye atamayı mümkün kılar. Yayımlandıktan sonra değişiklikler yapılabilir. Ek değişiklikler, aynı plan tanımının farklı sürümleri arasındaki farkları izlemek için yeni bir **Sürüm** değeriyle yayımlama gerektirir.

**Yayımlama planı tanımı başarılı** portal bildirimi göründükten sonra bir sonraki adıma geçin.

## <a name="assign-the-sample-copy"></a>Örnek kopyayı atama

Plan örneğinin kopyası başarıyla **Yayımlandıktan**sonra, kaydedildiği yönetim grubu içindeki bir aboneye atanabilir. Bu adım, plan örneğinin her kopyasını benzersiz hale getirmek için parametrelerin sağlandığı adımdır.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Blueprint tanımları** sayfasını seçin. _İki rgs-rol atamaları_ plan tanımını bulmak ve sonra seçmek için filtreleri kullanın.

1. Plan tanımı sayfasının üst **kısmındaki plan atay'ı** seçin.

1. Plan ataması için parametre değerlerini sağlayın:

   - Temel Bilgiler

     - **Abonelikler**: Plan örneğinin kopyasını kaydettiğiniz yönetim grubundaki aboneliklerden birini veya birkaçını seçin. Birden fazla abonelik seçerseniz, girilen parametreleri kullanarak her biri için bir atama oluşturulur.
     - **Atama adı**: Plan tanımının adına göre ad sizin için önceden doldurulur.
     - **Konum**: Oluşturulan yönetilen kimlik için bir bölge seçin. Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır. Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikler](../../../active-directory/managed-identities-azure-resources/overview.md).
       Bu öğretici için _Doğu ABD 2'yi_seçin.
     - **Blueprint tanımı sürümü**: Örnek plan tanımının kopyanızın **Yayınlanmış** sürüm _1.0'ını_ seçin.

   - Kilit Ataması

     Salt _Okunur_ plan kilidi modunu seçin. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../concepts/resource-locking.md).

   - Yönetilen Kimlik

     Varsayılan _Sistem atanmış_ seçeneği bırakın. Daha fazla bilgi için [yönetilen kimliklere](../../../active-directory/managed-identities-azure-resources/overview.md)bakın.

   - Yapı parametreleri

     Bu bölümde tanımlanan parametreler, tanımlandığı yapı için geçerlidir. Bu parametreler, planın atanması sırasında tanımlandıkları için [dinamik parametrelerdir.](../concepts/parameters.md#dynamic-parameters) Her yapı için parametre değerini **Değer** sütununda tanımlanan değere ayarlayın. Bunun `{Your ID}`için Azure kullanıcı hesabınızı seçin.

     |Artefakt adı|Artefakt türü|Parametre adı|Değer|Açıklama|
     |-|-|-|-|-|
     |ProdRG kaynak grubu|Kaynak grubu|Adı|ProductionRG|İlk kaynak grubunun adını tanımlar.|
     |ProdRG kaynak grubu|Kaynak grubu|Konum|Batı ABD 2|İlk kaynak grubunun konumunu ayarlar.|
     |Katılımcı|Rol ataması|Kullanıcı veya Grup|{Kimliğiniz}|İlk kaynak grubunda _Katılımcı_ rol ataması vermek için hangi kullanıcı veya grup tanımlar.|
     |PreProdRG kaynak grubu|Kaynak grubu|Adı|Üretim ÖncesiRG|İkinci kaynak grubunun adını tanımlar.|
     |PreProdRG kaynak grubu|Kaynak grubu|Konum|Batı ABD|İkinci kaynak grubunun konumunu ayarlar.|
     |Sahip|Rol ataması|Kullanıcı veya Grup|{Kimliğiniz}|İkinci kaynak grubunda _Sahibine_ rol ataması verecek kullanıcı veya grubu tanımlar.|
     |Okuyucular|Rol ataması|Kullanıcı veya Grup|{Kimliğiniz}|İkinci kaynak grubu içinde _Okuyuculara_ rol ataması verecek kullanıcı veya grubu tanımlar.|

1. Tüm parametreler girildikten sonra, sayfanın alt kısmında **Ata'yı** seçin.

Bu adım, tanımlanan kaynakları dağıtır ve seçili **Kilit Atamasını**yapılandırır. Plan kilitleri uygulamak için 30 dakika kadar sürebilir.

Atama **planı tanımı başarılı** portal bildirimi göründükten sonra bir sonraki adıma geçin.

## <a name="inspect-resources-deployed-by-the-assignment"></a>Atama tarafından dağıtılan kaynakları denetleme

Plan ataması, plan tanımında tanımlanan yapıları oluşturur ve izler. Planların atama sayfasından ve kaynaklara doğrudan bakarak kaynakların durumunu görebiliriz.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Atanan planlar** sayfasını seçin. _Atama-iki-rgs-rol atamaları_ plan atamasını bulmak ve sonra seçmek için filtreleri kullanın.

   Bu sayfadan, atamanın başarılı olduğunu ve oluşturulan kaynakların listesini plan kilit durumuyla birlikte görebiliriz. Atama güncelleştirilirse, **Atama işlemi** açılır, her tanım sürümünün dağıtımıyla ilgili ayrıntıları gösterir. Oluşturulan listelenen her kaynak tıklanabilir ve bu kaynaklar özelliği sayfasını açar.

1. **ProductionRG** kaynak grubunu seçin.

   Kaynak grubunun adının _ProdRG_değil, artefakt görüntü adı **ProductionRG** olduğunu görüyoruz. Bu ad, plan ataması sırasında ayarlanan değerle eşleşir.

1. Soldaki **Access denetimi (IAM)** sayfasını ve ardından **Rol atamaları** sekmesini seçin.

   Burada, hesabınıza _Bu kaynak_kapsamında _Katılımcı_ rolü verildiğini görüyoruz. _Atama-iki-rgs-rol atamaları_ ile plan atama kaynak grubu oluşturmak için kullanılan olarak _Sahibi_ rolü vardır. Bu izinler, yapılandırılmış plan kilitleri ile kaynakları yönetmek için de kullanılır.

1. Azure portalı kırıntısından, bir sayfa geriye gitmek için **Atama-iki rgs-rol atamaları'nı** seçin ve ardından **PreProductionRG** kaynak grubunu seçin.

1. Soldaki **Access denetimi (IAM)** sayfasını ve ardından **Rol atamaları** sekmesini seçin.

   Burada, hesabınızın hem Bu _kaynak_kapsamında hem _Sahibi_ hem de _Okuyucu_ rolleri verildiğini görüyoruz. Plan ataması, ilk kaynak grubu gibi _Sahibi_ rolüne de sahiptir.

1. Atamaları **Reddet** sekmesini seçin.

   Plan ataması, _Yalnızca Salt Plan_ kilidi modunu zorlamak için dağıtılan kaynak grubunda bir [reddet ataması](../../../role-based-access-control/deny-assignments.md) oluşturdu. Reddet ataması, _Rol atamaları_ sekmesinde uygun haklara sahip birinin belirli eylemlerde durmasını engeller. Reddet _atamatüm ilkeleri_etkiler.

1. Reddet atamasını seçin ve ardından soldaki **Reddedilen İzinler** sayfasını seçin.

   Reddet **\*** ataması ve **Eylem** yapılandırması ile tüm işlemleri engelliyor, ancak **NotActions**üzerinden ** \*/read'i** hariç tutarak okuma erişimine izin veriyor.

1. Azure portalı kırıntısından **PreProductionRG - Access denetimi (IAM) seçeneğini**belirleyin. Ardından soldaki **Genel Bakış** sayfasını ve ardından **kaynak grup** larını sil düğmesini seçin. Silmeyi onaylamak için _PreProductionRG_ adını girin ve bölmenin altındaki **Sil'i** seçin.

   Portal bildirimi **Delete kaynak grubu PreProductionRG başarısız** görüntülenir. Hata, hesabınızda kaynak grubunu silme izni olsa da, erişimin plan ataması tarafından reddedildiğini belirtir. Plan ataması sırasında _Salt Okunur_ plan kilidi modunu seçtiğimizi unutmayın. Plan kilidi, kaynak silme izni olan bir hesabı, hatta _Sahibin_bile olmasını engeller. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../concepts/resource-locking.md).

Bu adımlar, kaynaklarımızın tanımlandığı şekilde oluşturulduğunu ve plan kilitlerinin izinli bir hesaptan bile istenmeyen silinmesini engellediğini gösterir.

## <a name="unassign-the-blueprint"></a>Planı atama

Son adım, planın atamasını ve dağıtdığı kaynakları kaldırmaktır.
Atamayı kaldırmak, dağıtılan yapıları kaldırmaz.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Atanan planlar** sayfasını seçin. _Atama-iki-rgs-rol atamaları_ plan atamasını bulmak ve sonra seçmek için filtreleri kullanın.

1. Sayfanın üst kısmındaki **Atama zat ını** seçin. Onay iletişim kutusundaki uyarıyı okuyun ve **ardından Tamam'ı**seçin.

   Plan ataması kaldırıldığında, plan kilitleri de kaldırılır. Oluşturulan kaynaklar, izinleri olan bir hesap tarafından bir kez daha silinebilir.

1. Azure menüsünden **Kaynak grupları** seçin ve ardından **ProductionRG'yi**seçin.

1. Soldaki **Access denetimi (IAM)** sayfasını ve ardından **Rol atamaları** sekmesini seçin.

Her kaynak gruplarının güvenliği hala dağıtılan rol atamalarına sahiptir, ancak plan ataması artık _Sahip erişimine_ sahip değildir.

Blueprint **atamasını kaldırma** başarılı portal bildirimi göründükten sonra bir sonraki adıma geçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici yle tamamlandığında, aşağıdaki kaynakları silin:

- Kaynak grubu _ProductionRG_
- Kaynak grubu _PreProductionRG_
- Plan tanımı _iki rgs-rol-atamaları ile_

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, örnek bir tanımdan yeni bir plan oluşturmayı öğrendiniz. Azure Planları hakkında daha fazla bilgi edinmek için, plan yaşam döngüsü makalesine devam edin.

> [!div class="nextstepaction"]
> [Plan yaşam döngüsü hakkında bilgi edinin](../concepts/lifecycle.md)