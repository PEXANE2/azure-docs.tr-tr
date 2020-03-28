---
title: 'Öğretici: Kilitlerle yeni kaynakları koruyun'
description: Bu eğitimde, yeni dağıtılan kaynakları korumak için Salt Planlar kaynak kilitleri yalnızca oku ve Silme yetmez seçeneklerini kullanırsınız.
ms.date: 11/21/2019
ms.topic: tutorial
ms.openlocfilehash: ee57ff0c08f4fb8aa710dd2fa4dcef664484973d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74327454"
---
# <a name="tutorial-protect-new-resources-with-azure-blueprints-resource-locks"></a>Öğretici: Azure Blueprints kaynak kilitleriyle yeni kaynakları koruyun

Azure Blueprints [kaynak kilitleri](../concepts/resource-locking.md)ile, yeni dağıtılan _kaynakları, Sahibi_ rolü olan bir hesap tarafından bile değiştirilmekten koruyabilirsiniz. Bu korumayı, Kaynak Yöneticisi şablonu yapı tarafından oluşturulan kaynakların plan tanımlarına ekleyebilirsiniz.

Bu öğreticide, şu adımları tamamlayaceksiniz:

> [!div class="checklist"]
> - Plan tanımı oluşturma
> - Plan tanımınızı **Yayımlanmış** olarak işaretleme
> - Plan tanımınızı varolan bir aboneye atama
> - Yeni kaynak grubunu inceleyin
> - Kilitleri kaldırmak için planı atama

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free) bir hesap oluşturun.

## <a name="create-a-blueprint-definition"></a>Plan tanımı oluşturma

İlk olarak, plan tanımını oluşturun.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Başlat'ı Başlat** sayfasında, **plan oluştur**altında **Oluştur'u** seçin.

1. **Sayfanın** üst kısmında boş plan planı örneğini bulun. **Boş planla Başlat'ı**seçin.

1. **Temel bilgiler** sekmesine bu bilgileri girin:

   - **Plan adı**: Plan örneğinin kopyası için bir ad verin. Bu öğretici için kilitli depolama **hesabı**adını kullanacağız.
   - **Plan açıklaması**: Plan tanımı için açıklama ekleyin. **Dağıtılan kaynaklara plan kaynağı kilitlemeyi test etmek için**kullanın.
   - **Tanım konumu**: Elips düğmesini (...) seçin ve ardından plan tanımınızı kaydetmek için yönetim grubunu veya aboneliği seçin.

1. Sayfanın üst kısmındaki **Eserler** sekmesini veya sayfanın altındaki **Sonraki: Eserler** sekmesini seçin.

1. Abonelik düzeyinde bir kaynak grubu ekleyin:
   1. **Abonelik**altında **yapı ekle** satırını seçin.
   1. Yapı türü altında **Kaynak** **Grubu'ni**seçin.
   1. **Artifakı ekran adını** **RGtoLock**olarak ayarlayın.
   1. Kaynak **Grubu Adı** ve **Konum** kutularını boş bırakın, ancak **dinamik parametreler**yapmak için her özellikte onay kutusunun seçildiğinden emin olun.
   1. Yapıyı plana eklemek için **Ekle'yi** seçin.

1. Kaynak grubunun altına şablon ekleyin:
   1. **RGtoLock** girişinin altındaki **artefakt ekle** satırını seçin.
   1. Yapı türü altında **Azure Kaynak Yöneticisi şablonu** seçin, **Artefakt ekran adını** Depolama **Hesabı'na**ayarlayın ve **Açıklamayı** boş bırakın. **Artifact type**
   1. **Şablon** sekmesinde, aşağıdaki Kaynak Yöneticisi şablonunu düzenleyici kutusuna yapıştırın.
      Şablona yapıştırdıktan sonra, yapıyı plana eklemek için **Ekle'yi** seçin.

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
           }
       },
       "variables": {
           "storageAccountName": "[concat('store', uniquestring(resourceGroup().id))]"
       },
       "resources": [{
           "type": "Microsoft.Storage/storageAccounts",
           "name": "[variables('storageAccountName')]",
           "location": "[resourceGroup().location]",
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

1. Sayfanın altındaki **Taslağı Kaydet'i** seçin.

Bu adım, seçili yönetim grubunda veya abonelikte plan tanımını oluşturur.

**Planını Kaydetme tanımı başarılı** portal bildirimi göründükten sonra bir sonraki adıma geçin.

## <a name="publish-the-blueprint-definition"></a>Plan tanımını yayımla

Plan tanımınız artık ortamınızda oluşturuldu. **Taslak** modunda oluşturulur ve atanıp dağıtılmadan önce yayımlanmalıdır.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Blueprint tanımları** sayfasını seçin. **Kilitli depolama hesabı** planı tanımını bulmak için filtreleri kullanın ve ardından seçin.

1. Sayfanın üst kısmında **ki planı Yayımla'yı** seçin. Sağdaki yeni bölmeye Sürüm olarak **1.0** **girin.** Bu özellik, daha sonra bir değişiklik yaparsanız yararlıdır. Plan dağıtılan kaynakları **kilitlemek için yayınlanan İlk sürüm**gibi Değişiklik **notları**girin. Ardından sayfanın altındaki **Yayımla'yı** seçin.

Bu adım, planı bir aboneye atamayı mümkün kılar. Plan tanımı yayımlandıktan sonra, yine de değişiklik yapabilirsiniz. Değişiklik yaparsanız, aynı plan tanımının sürümleri arasındaki farkları izlemek için tanımı yeni bir sürüm değeriyle yayımlamanız gerekir.

**Yayımlama planı tanımı başarılı** portal bildirimi göründükten sonra bir sonraki adıma geçin.

## <a name="assign-the-blueprint-definition"></a>Plan tanımını atama

Plan tanımı yayımlandıktan sonra, onu kaydettiğiniz yönetim grubu içindeki bir aboneye atayabilirsiniz. Bu adımda, plan tanımının her dağıtımını benzersiz kılmak için parametreler sağlarsınız.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Blueprint tanımları** sayfasını seçin. **Kilitli depolama hesabı** planı tanımını bulmak için filtreleri kullanın ve ardından seçin.

1. Plan tanımı sayfasının üst **kısmındaki plan atay'ı** seçin.

1. Plan ataması için parametre değerlerini sağlayın:

   - **Temel Bilgiler**

     - **Abonelikler**: Plan tanımınızı kaydettiğiniz yönetim grubunda bulunan aboneliklerden birini veya birkaçını seçin. Birden fazla abonelik seçerseniz, girdiğiniz parametreleri kullanarak her abonelik için bir atama oluşturulur.
     - **Atama adı**: Plan tanımının adına göre ad önceden doldurulur. Bu atamanın yeni kaynak grubunu kilitlemeyi temsil etmesini istiyoruz, bu nedenle atama adını **atama kilitli-depolama hesabı-TestBPLocks**olarak değiştirin.
     - **Konum**: Yönetilen kimliği oluşturmak için bir bölge seçin. Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır. Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikler](../../../active-directory/managed-identities-azure-resources/overview.md).
       Bu öğretici için **Doğu ABD 2'yi**seçin.
     - **Blueprint tanımı sürümü**: Plan tanımının yayınlanmış sürüm **1.0'ını** seçin.

   - **Kilit Ataması**

     Salt **Okunur** plan kilidi modunu seçin. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../concepts/resource-locking.md).

   - **Yönetilen Kimlik**

     Varsayılan seçeneği kullanın: **Sistem atanmış**. Daha fazla bilgi için [yönetilen kimliklere](../../../active-directory/managed-identities-azure-resources/overview.md)bakın.

   - **Yapı parametreleri**

     Bu bölümde tanımlanan parametreler, tanımlandıkları yapı için geçerlidir. Bu [parametreler dinamik parametrelerdir](../concepts/parameters.md#dynamic-parameters) çünkü planın atanması sırasında tanımlanırlar. Her yapı için parametre değerini **Değer** sütununda gördükleriniz ine ayarlayın.

     |Artefakt adı|Artefakt türü|Parametre adı|Değer|Açıklama|
     |-|-|-|-|-|
     |RGtoLock kaynak grubu|Kaynak grubu|Adı|TestBPLocks|Plan kilitlerini uygulamak için yeni kaynak grubunun adını tanımlar.|
     |RGtoLock kaynak grubu|Kaynak grubu|Konum|Batı ABD 2|Plan kilitlerini uygulamak için yeni kaynak grubunun konumunu tanımlar.|
     |StorageAccount|Resource Manager şablonu|storageAccountType (StorageAccount)|Standard_GRS|Depolama SKU. Varsayılan değer _Standard_LRS._|

1. Tüm parametreleri girdikten sonra sayfanın alt kısmında **Ata'yı** seçin.

Bu adım, tanımlanan kaynakları dağıtır ve seçili **Kilit Atamasını**yapılandırır. Plan kilitlerini uygulamak 30 dakika kadar sürebilir.

Atama **planı tanımı başarılı** portal bildirimi göründükten sonra, bir sonraki adıma geçin.

## <a name="inspect-resources-deployed-by-the-assignment"></a>Atama tarafından dağıtılan kaynakları denetleme

Atama kaynak grubu _TestingBPLocks_ ve Depolama hesabı Kaynak Yöneticisi şablon yapı tarafından dağıtılan oluşturur. Yeni kaynak grubu ve seçili kilit durumu atama ayrıntıları sayfasında gösterilir.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Atanan planlar** sayfasını seçin. **Atama kilitli-depolama hesabı-TestBPLocks** plan atamasını bulmak için filtreleri kullanın ve ardından seçin.

   Bu sayfadan, atamanın başarılı olduğunu ve kaynakların yeni plan kilidi durumuyla dağıtıldığını görebiliriz. Atama güncelleştirilirse, **Atama işlemi** açılır, her tanım sürümünün dağıtımıyla ilgili ayrıntıları gösterir. Özellik sayfasını açmak için kaynak grubunu seçebilirsiniz.

1. **TestingBPLocks** kaynak grubunu seçin.

1. Soldaki **Access denetimi (IAM)** sayfasını seçin. Ardından **Rol atamaları** sekmesini seçin.

   Burada _atama kilitli-depolama hesabı-TestBPLocks_ plan atama _Sahibi_ rolü olduğunu görüyoruz. Bu rol kaynak grubunu dağıtmak ve kilitlemek için kullanıldığından bu role sahiptir.

1. Atamaları **Reddet** sekmesini seçin.

   Plan ataması, **Yalnızca Salt Plan** kilidi modunu zorlamak için dağıtılan kaynak grubunda bir [reddet ataması](../../../role-based-access-control/deny-assignments.md) oluşturdu. Reddet ataması, **Rol atamaları** sekmesinde uygun haklara sahip birinin belirli eylemlerde durmasını engeller. Reddet _atamatüm ilkeleri_etkiler.

   Bir anaparanın reddi atamasından hariç tevkici hakkında bilgi için, [planlar kaynak kilitleme'ye](../concepts/resource-locking.md#exclude-a-principal-from-a-deny-assignment)bakın.

1. Reddet atamasını seçin ve ardından soldaki **Reddedilen İzinler** sayfasını seçin.

   Reddet **\*** ataması ve **Eylem** yapılandırması ile tüm işlemleri engelliyor, ancak **NotActions**üzerinden ** \*/read'i** hariç tutarak okuma erişimine izin veriyor.

1. Azure portalı kırıntısında **TestingBPLocks - Access denetimi (IAM)** seçeneğini belirleyin. Ardından soldaki **Genel Bakış** sayfasını ve ardından **kaynak grup** larını sil düğmesini seçin. Silmeyi onaylamak için **TestingBPLocks** adını girin ve ardından bölmenin altındaki **Sil'i** seçin.

   Portal bildirimi **Kaynak grubu Delete TestBPLocks başarısız** görünür. Hata, hesabınızda kaynak grubunu silme izni olmasına rağmen, erişimin plan ataması tarafından reddedildiğini belirtir. Plan ataması sırasında **Salt Okunur** plan kilidi modunu seçtiğimizi unutmayın. Plan kilidi, kaynak silme izni olan bir hesabı, hatta _Sahibin_bile olmasını engeller. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../concepts/resource-locking.md).

Bu adımlar, dağıtılan kaynaklarımızın artık kaynakları silme izni olan bir hesaptan bile istenmeyen silmeyi engelleyen plan kilitleriyle korunduğunu gösterir.

## <a name="unassign-the-blueprint"></a>Planı atama

Son adım, plan tanımının atamasını kaldırmaktır. Atamayı kaldırmak ilişkili yapıları kaldırmaz.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Atanan planlar** sayfasını seçin. **Atama kilitli-depolama hesabı-TestBPLocks** plan atamasını bulmak için filtreleri kullanın ve ardından seçin.

1. Sayfanın üst kısmında **Kiremit'i** seçin. Onay iletişim kutusundaki uyarıyı okuyun ve ardından **Tamam'ı**seçin.

   Plan ataması kaldırıldığında, plan kilitleri de kaldırılır. Kaynaklar, uygun izinlere sahip bir hesap tarafından bir kez daha silinebilir.

1. Azure menüsünden **Kaynak grupları** seçin ve ardından **TestingBPLocks'u**seçin.

1. Soldaki **Access denetimi (IAM)** sayfasını seçin ve ardından **Rol atamaları** sekmesini seçin.

Kaynak grubunun güvenliği, plan atamasının artık _Sahip erişimine_ sahip olmadığını gösterir.

Blueprint **atama kaldırma başarılı** portal bildirimi görünür sonra, bir sonraki adıma gidin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyi bitirdikten sonra şu kaynakları silin:

- Kaynak grubu _TestingBPLocks_
- Plan tanımı _kilitli depolama hesabı_

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure Planları ile dağıtılan yeni kaynakların nasıl korunabileceğinizi öğrendiniz. Azure Planları hakkında daha fazla bilgi edinmek için, plan yaşam döngüsü makalesine devam edin.

> [!div class="nextstepaction"]
> [Plan yaşam döngüsü hakkında bilgi edinin](../concepts/lifecycle.md)