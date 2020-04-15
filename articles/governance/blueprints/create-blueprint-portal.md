---
title: 'Quickstart: Portalda bir plan oluşturma'
description: Bu hızlı başlangıçta, Azure portalı üzerinden yapı tadilleri oluşturmak, tanımlamak ve dağıtmak için Azure Planları'nı kullanırsınız.
ms.date: 03/25/2020
ms.topic: quickstart
ms.openlocfilehash: dd50b1833f16d364a4494483fcccfee017bb982b
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81381901"
---
# <a name="quickstart-define-and-assign-a-blueprint-in-the-portal"></a>Quickstart: Portalda bir plan tanımlayın ve atayın

Planları oluşturmayı ve atamayı öğrendiğiniz zaman, Azure Kaynak Yöneticisi şablonları, ilke, güvenlik ve daha fazlasını temel alan yeniden kullanılabilir ve hızla dağıtılabilir yapılandırmalar geliştirmek için ortak desenler tanımlayabilirsiniz. Bu eğitimde, kuruluşunuz içinde bir plan oluşturma, yayımlama ve atama yla ilgili ortak görevlerden bazılarını yapmak için Azure Planları'nı kullanmayı öğrenirsiniz. Bu görevler aşağıdakileri içerir:

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free) bir hesap oluşturun.

## <a name="create-a-blueprint"></a>Şema oluşturma

Uyumluluk için standart desen tanımlamanın ilk adımı kullanılabilir durumdaki kaynaklardan bir şema oluşturmaktır. Bu örnekte, aboneliğin rol ve ilke atamalarını yapılandırmak için **MyBlueprint** adında yeni bir plan oluşturun. Ardından yeni bir kaynak grubu ekleyin ve yeni kaynak grubunda bir Kaynak Yöneticisi şablonu ve rol ataması oluşturun.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki sayfadan **Plan tanımlarını** seçin ve sayfanın üst kısmındaki **+ Plan Oluştur** düğmesini seçin.

   Veya, doğrudan bir plan oluşturmak için **Başlangıç** sayfasından **Oluştur'u** seçin.

   :::image type="content" source="./media/create-blueprint-portal/create-blueprint-button.png" alt-text="Blueprint tanımları sayfasından bir plan oluşturma" border="false":::

1. Yerleşik planlar listesinin en üstündeki karttan **boş planla Başlat'ı** seçin.

1. **MyBlueprint**gibi bir **Plan adı** sağlayın. (En fazla 48 harf ve sayı kullanın, ancak boşluk veya özel karakter kullanmayın). **Blueprint açıklamasını** şimdilik boş bırakın.

1. Tanım **konum** kutusunda, sağdaki elipsleri seçin, planı kaydetmek istediğiniz [yönetim grubunu](../management-groups/overview.md) veya aboneliği seçin ve **Seç'i**seçin.

1. Bilgilerin doğru olduğundan doğrulayın. **Blueprint adı** ve **Tanım konum** alanları daha sonra değiştirilemez. Ardından **İleri :** Sayfanın altındaki Eserler veya sayfanın üst kısmındaki **Eserler** sekmesini seçin.

1. Abonelik düzeyinde bir rol ataması ekleyin:

   1. **Abonelik**altında **+ Yapı** ekle satırını seçin. **Yapı ekle** penceresi tarayıcının sağ tarafında açılır.

   1. **Artefakt türü**için **Rol ataması** seçin.

   1. **Rolü**altında, **Katkıda Bulunan'ı**seçin. Kullanıcı, **uygulama veya grup** kutusunu dinamik bir parametreyi gösteren onay kutusuyla birlikte bırak.

   1. Bu yapıyı plana eklemek için **Ekle'yi** seçin.

   :::image type="content" source="./media/create-blueprint-portal/add-role-assignment.png" alt-text="Bir plan artifakı için rol ataması" border="false":::

   > [!NOTE]
   > Çoğu yapıt, parametreleri destekler. Plan oluşturma sırasında değer atanan bir parametre statik bir _parametredir._ Plan ataması sırasında parametre atanmışsa, dinamik bir _parametredir._ Daha fazla bilgi için bkz. [Şema parametreleri](./concepts/parameters.md).

1. Abonelik düzeyinde bir ilke ataması ekleyin:

   1. Rol atama artefaktAltında **+ Yapı** ekle satırını seçin.

   1. Yapı türü için **İlke** **ataması**seçin.

   1. **Türü** **Yerleşik**olarak değiştirin. **Arama'da**, **etiketi**girin.

   1. Filtrelemenin yapılması için **Ara**’ya tıklayın. **Ek etiketini ve kaynak gruplarına varsayılan değerini**seçin.

   1. Bu yapıyı plana eklemek için **Ekle'yi** seçin.

1. İlke atama Sıtkı etiketinin satırını **ve kaynak gruplarına varsayılan değerini**seçin.

1. Şema tanımı kapsamında yapıta parametreleri sağlayacak pencere açılır ve atama sırasında parametre ayarlamak (dinamik parametreler) yerine bu şemaya göre tüm atamalar için parametreleri ayarlamaya (statik parametreler) olanak tanır. Bu örnek, plan ataması sırasında dinamik parametreler kullanır, bu nedenle varsayılanları bırakın ve **İptal'i**seçin.

1. Abonelik düzeyinde bir kaynak grubu ekleyin:

   1. **Abonelik**altında **+ Yapı** ekle satırını seçin.

   1. **Yapı türü**için **Kaynak grubu** seçin.

   1. Yapı **görüntü adı,** **Kaynak Grubu Adı**ve **Konum** kutularını boş bırakın, ancak onay kutusunun dinamik parametreler yapmak için her parametre özelliği için işaretli olduğundan emin olun.

   1. Bu yapıyı plana eklemek için **Ekle'yi** seçin.

1. Kaynak grubunun altına şablon ekleyin:

   1. **Kaynak Grubu** girişinin altındaki **yapı tonu ekle satırını** seçin.

   1. Yapı türü için **Azure Kaynak Yöneticisi şablonu** seçin, **Artefakt ekran adını** Depolama **Hesabı'na**ayarlayın ve **Açıklama'yı** boş bırakın. **Artifact type**

   1. Düzenleyici kutusundaki **Şablon** sekmesinde aşağıdaki Resource Manager şablonunu yapıştırın.
      Şablonu yapıştırdıktan sonra **Parametreler** sekmesini seçin ve şablon parametreleri **depolamaAccountType** ve **konumunun** algılandığını unutmayın. Her parametre otomatik olarak algılandı ve dolduruldu, ancak dinamik bir parametre olarak yapılandırıldı.

      > [!IMPORTANT]
      > Şablonu alıyorsanız, dosyanın yalnızca JSON olduğundan ve HTML içermediğinden emin olun. GitHub'da bir URL'yi işaret ederken, GitHub'da görüntülenmek üzere HTML ile sarılmış dosyayı değil, saf JSON dosyasını almak için **RAW'ı** seçtiğinizden emin olun. İçeri aktarılan şablon saf JSON değilse bir hata oluşur.

      ```json
      {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
              "storageAccountType": {
                  "type": "string",
                  "defaultValue": "Standard_LRS",
                  "allowedValues": [
                      "Standard_LRS",
                      "Standard_GRS",
                      "Standard_ZRS",
                      "Premium_LRS"
                  ],
                  "metadata": {
                      "description": "Storage Account type"
                  }
              },
              "location": {
                  "type": "string",
                  "defaultValue": "[resourceGroup().location]",
                  "metadata": {
                      "description": "Location for all resources."
                  }
              }
          },
          "variables": {
              "storageAccountName": "[concat('store', uniquestring(resourceGroup().id))]"
          },
          "resources": [{
              "type": "Microsoft.Storage/storageAccounts",
              "name": "[variables('storageAccountName')]",
              "location": "[parameters('location')]",
              "apiVersion": "2018-07-01",
              "sku": {
                  "name": "[parameters('storageAccountType')]"
              },
              "kind": "StorageV2",
              "properties": {}
          }],
          "outputs": {
              "storageAccountName": {
                  "type": "string",
                  "value": "[variables('storageAccountName')]"
              }
          }
      }
      ```

   1. **storageAccountType** onay kutusunu temizleyin ve açılır listenin yalnızca Kaynak Yöneticisi şablonuna **izin verilen Değerler**altında dahil edilen değerleri içerdiğini unutmayın. Dinamik bir parametreye geri ayarlamak için kutuyu seçin.

   1. Bu yapıyı plana eklemek için **Ekle'yi** seçin.

   :::image type="content" source="./media/create-blueprint-portal/add-resource-manager-template.png" alt-text="Plan artifakı için Kaynak Yöneticisi şablonu" border="false":::

1. Tamamladığınız şema aşağıdakine benzer olmalıdır. Her yapının **Parametreler** sütununda **doldurulan _y_ parametrelerinin _x'i_ ** olduğuna dikkat edin. Dinamik parametreler şemanın her atamasında ayarlanır.

   :::image type="content" source="./media/create-blueprint-portal/completed-blueprint.png" alt-text="Tamamlanmış plan tanımı" border="false":::

1. Artık tüm planlanan yapılar eklendiğiniz için sayfanın altındaki **Taslak'ı Kaydet'i** seçin.

## <a name="edit-a-blueprint"></a>Şema düzenleme

[Plan](#create-a-blueprint)Oluştur'da, bir açıklama sağlamadın veya rol atamasını yeni kaynak grubuna eklemedin. Aşağıdaki adımları izleyerek her ikisini de düzeltebilirsiniz:

1. Soldaki sayfadan **Blueprint tanımlarını** seçin.

1. Planlar listesinde, daha önce oluşturduğunuz planı sağ tıklatın ve **planı edit'i**seçin.

1. **Blueprint açıklamasında,** plan ve onu oluşturan eserler hakkında bazı bilgiler sağlayın. Bu durumda, aşağıdaki gibi bir şey girin: **Bu plan, abonelikte etiket ilkesi ni ve rol atamasını ayarlar, bir Kaynak Grubu oluşturur ve söz konusu Kaynak Grubu'na bir kaynak şablonu ve rol ataması dağıtır.**

1. **Sonrakini** Seçin : Sayfanın altındaki eserler veya sayfanın üst kısmındaki **Eserler** sekmesi.

1. Kaynak grubuna bir rol ataması ekleyin:

   1. Kaynak **Grubu** girişinin hemen altındaki **yapı tonu ekle satırını** seçin.

   1. **Artefakt türü**için **Rol ataması** seçin.

   1. **Role**altında, **Sahibi'ni**seçin ve **kullanıcı, uygulama veya grup** kutusunu ekle'nin altındaki onay kutusunu temizleyin.

   1. Eklenecek bir kullanıcıyı, uygulamayı veya grubu arayın ve seçin. Yapıt, bu şemanın tüm atamalarında aynı statik parametreyi kullanır.

   1. Bu yapıyı plana eklemek için **Ekle'yi** seçin.

   :::image type="content" source="./media/create-blueprint-portal/add-role-assignment-2.png" alt-text="Plan artifakı için ikinci rol ataması" border="false":::

1. Tamamladığınız şema aşağıdakine benzer olmalıdır. Yeni eklenen rol atamasının **doldurulan 1 parametreden 1'ini**gösterdiğine dikkat edin. Yani statik bir parametre.

   :::image type="content" source="./media/create-blueprint-portal/completed-blueprint-2.png" alt-text="Tamamlanan plan için ikinci tanım" border="false":::

1. Güncelleştirildikten sonra **Taslağı Kaydet'i** seçin.

## <a name="publish-a-blueprint"></a>Şemayı yayımlama

Planlanan tüm yapıtları ekledikten sonra şemayı yayımlayabilirsiniz.
Yayımlama, planı bir aboneye atanmak üzere kullanılabilir hale getirir.

1. Soldaki sayfadan **Blueprint tanımlarını** seçin.

1. Planlar listesinde, daha önce oluşturduğunuz planı sağ tıklatın ve **Planı Yayımla'yı**seçin.

1. Açılan bölmede, **v1**gibi en fazla 20 karakter uzunluğunda bir **Sürüm** (harfler, sayılar ve tireler) sağlayın. İsteğe bağlı olarak, **İlk yayımlama**gibi **Değiştir notlarına**metin girin.

1. Sayfanın altındaki **Yayımla'yı** seçin.

## <a name="assign-a-blueprint"></a>Şema atama

Bir plan yayımlandıktan sonra, bir aboneye atanabilir. Oluşturduğunuz planı yönetim grubu hiyerarşiniz altındaki aboneliklerden birine atayın. Plan bir aboneye kaydedilirse, yalnızca bu aboneye atanabilir.

1. Soldaki sayfadan **Blueprint tanımlarını** seçin.

1. Planlar listesinde, daha önce oluşturduğunuz (veya elipsleri seçin) sağ tıklatın ve **planı atla'yı**seçin.

1. Plan **Atla** sayfasında, **Abonelik** açılır listesinde, bu planı dağıtmak istediğiniz abonelikleri seçin.

   [Azure Faturalandırma'da](../../billing/index.md)desteklenen Kurumsal teklifler varsa, **Abonelik** kutusunun altında yeni bir **bağlantı oluştur** etkinleştirilir. Şu adımları uygulayın:

   1. Varolanları seçmek yerine yeni bir abonelik oluşturmak için **yeni** bağlantı oluştur'u seçin.

   1. Yeni abonelik için bir **Görüntü adı** sağlayın.

   1. Açılan listeden kullanılabilir **Teklifi** seçin.

   1. Aboneliğin alt grubu olacağı [yönetim grubunu](../management-groups/overview.md) seçmek için elipsleri kullanın.

   1. Sayfanın altındaki **Oluştur'u** seçin.

      :::image type="content" source="./media/create-blueprint-portal/assignment-create-subscription.png" alt-text="Plan atama aboneliği için abonelik oluşturma" border="false":::

      > [!IMPORTANT]
      > Yeni abonelik, **Oluştur'u**seçtikten hemen sonra oluşturulur.

   > [!NOTE]
   > Seçtiğiniz her abonelik için bir atama oluşturulur. Seçili aboneliklerin geri kalanında değişiklik zorlamadan daha sonraki bir zamanda tek bir abonelik atamada değişiklik yapabilirsiniz.

1. **Atama adı**için, bu atama için benzersiz bir ad sağlayın.

1. **Konum'da,** oluşturulacak yönetilen kimlik ve abonelik dağıtım nesnesi için bir bölge seçin. Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır. Daha fazla bilgi edinmek [için Azure kaynakları için Yönetilen kimlikler'e](../../active-directory/managed-identities-azure-resources/overview.md)bakın.

1. **V1** girişinde **Yayınlanan** sürümlerin **Blueprint tanımı sürüm** açılır seçimini bırakın. (Varsayılan değer en son yayınlanan sürümüdür.)

1. **Atamayı Kilitle** seçeneği için varsayılan **Kilitleme** ayarını değiştirmeyin. Daha fazla bilgi için [Blueprints kaynak kilitleme'ye](./concepts/resource-locking.md)bakın.

   :::image type="content" source="./media/create-blueprint-portal/assignment-locking-mi.png" alt-text="Atama için kilitleme ve yönetilen kimlikler" border="false":::

1. **Yönetilen Kimlik**altında , **atanan Sistemin**varsayılan bırakın.

1. Abonelik düzeyinde rol ataması **[Kullanıcı grubu veya uygulama adı]:Katkıda bulunan** için bir kullanıcı, uygulama veya grup arayıp seçin.

1. Abonelik düzeyi ilkesi ataması için **Etiket** **Adı'nı CostCenter'a,** Etiket **Değerini** de **ContosoIT'e**ayarlayın.

1. **ResourceGroup**için, açılan listeden **Bir Depolama Hesabı** **Adı** ve Doğu **ABD 2** **konumu** sağlayın.

   > [!NOTE]
   > Plan tanımı sırasında kaynak grubunun altına eklediğiniz her yapı için, bu yapı, dağıtacağınız kaynak grubu veya nesneyle hizalamak üzere girintilir.
   > Parametreleri almayan veya atamada tanımlanacak parametreleri olmayan yapılar yalnızca bağlamsal bilgiler için listelenir.

1. Azure Kaynak Yöneticisi şablonu **StorageAccount'ta** **storageAccount** türü için **Standard_GRS'yi** seçin.

1. Sayfanın altındaki bilgi kutusunu okuyun ve sonra **Atla'yı**seçin.

## <a name="track-deployment-of-a-blueprint"></a>Şema dağıtımını izleme

Bir şema bir veya daha fazla aboneliğe atandığında iki şey gerçekleşir:

- Plan, her abonelik için **Atanan planlar** sayfasına eklenir.
- Plan tarafından tanımlanan tüm yapıları dağıtma işlemi başlar.

Plan bir aboneye atandığı için, dağıtımın ilerlemesini doğrulayın:

1. Soldaki sayfadan **Atanmış planları** seçin.

1. Planlar listesinde, daha önce atadığınız planı sağ tıklatın ve **atama ayrıntılarını görüntüle'yi**seçin.

   :::image type="content" source="./media/create-blueprint-portal/view-assignment-details.png" alt-text="Atanan planlar sayfasından atama ayrıntılarını görüntüleme" border="false":::

1. Blueprint **atama** sayfasında, tüm yapıtların başarıyla dağıtıldığını ve dağıtım sırasında hata olmadığını doğrulayın. Hatalar oluştuysa, neyin yanlış gittiğini belirlemek için adımlar için [Sorun Giderme planlarına](./troubleshoot/general.md) bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

### <a name="unassign-a-blueprint"></a>Şema atamasını kaldırma

Artık bir plan atamasına ihtiyacınız yoksa, bunu abonelikten kaldırın. Plan, güncelleştirilmiş desenler, ilkeler ve tasarımlar içeren yeni bir planla değiştirilmiş olabilir. Bir şema kaldırıldığında o şemanın bir parçası olarak atanan yapıtlar geride kalır. Şema atamasını kaldırmak için aşağıdaki adımları izleyin:

1. Soldaki sayfadan **Atanmış planları** seçin.

1. Planlar listesinde, atamak istediğiniz planı seçin. Ardından sayfanın üst kısmındaki **Atama zat düğmesini** seçin.

1. Onay iletisini okuyun ve **ardından Tamam'ı**seçin.

### <a name="delete-a-blueprint"></a>Şema silme

1. Soldaki sayfadan **Blueprint tanımlarını** seçin.

1. Silmek istediğiniz plana sağ tıklayın ve **planı sil'i**seçin. Ardından onay iletişim kutusunda **Evet'i** seçin.

> [!NOTE]
> Bu yöntemdeki bir planı silmek, seçili planın tüm yayınlanmış sürümlerini de siler.
> Tek bir sürümü silmek için planı açın, **Yayınlanan sürümler** sekmesini seçin, silmek istediğiniz sürümü seçin ve ardından **Bu Sürümü Sil'i**seçin. Ayrıca, plan tanımının tüm plan ataması silinmeden bir planı silemezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure portalı ile bir plan oluşturdunuz, adediverdiniz ve kaldırdınız. Azure Planları hakkında daha fazla bilgi edinmek için, plan yaşam döngüsü makalesine devam edin.

> [!div class="nextstepaction"]
> [Plan yaşam döngüsü hakkında bilgi edinin](./concepts/lifecycle.md)