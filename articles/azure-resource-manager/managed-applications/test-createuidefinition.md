---
title: U-UI tanım dosyasını test edin
description: Azure Yönetilen Uygulamanızı oluşturmak için kullanıcı deneyimini portal üzerinden nasıl sınayarak test ediletilir.
author: tfitzmac
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: tomfitz
ms.openlocfilehash: e2d075a58872f9337c7d1faa642a48047e2f9ddf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78250192"
---
# <a name="test-your-portal-interface-for-azure-managed-applications"></a>Azure Yönetilen Uygulamalar için portal arabiriminizi test edin

Yönetilen uygulamanız için [createUiDefinition.json dosyasını oluşturduktan](create-uidefinition-overview.md) sonra, kullanıcı deneyimini test etmeniz gerekir. Testi kolaylaştırmak için, dosyanızı portala yükleyen bir kum havuzu ortamı kullanın. Yönetilen uygulamanızı gerçekten dağıtmanız gerekmez. Kum havuzu, kullanıcı arabiriminizi geçerli tam ekran portal deneyiminde sunar. Veya arabirimi sınamak için bir komut dosyası kullanabilirsiniz. Her iki yaklaşım da bu makalede gösterilmiştir. Zonk alanı, arabirimi önizlemenin önerilen yoludur.

## <a name="prerequisites"></a>Ön koşullar

* **CreateUiDefinition.json** dosyası. Bu dosya yoksa, [örnek dosyayı](https://github.com/Azure/azure-quickstart-templates/blob/master/100-marketplace-sample/createUiDefinition.json)kopyalayın.

* Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="use-sandbox"></a>Kum havuzu kullanma

1. [UI Tanımı Oluştur Sandbox'ı](https://portal.azure.com/?feature.customPortal=false&#blade/Microsoft_Azure_CreateUIDef/SandboxBlade)açın.

   ![Kum havuzugöster](./media/test-createuidefinition/show-sandbox.png)

1. Boş tanımı createUiDefinition.json dosyanızın içeriğiyle değiştirin. **Önizleme'yi**seçin.

   ![Önizlemeyi seçin](./media/test-createuidefinition/select-preview.png)

1. Oluşturduğunuz form görüntülenir. Kullanıcı deneyimini gözden geçirip değerleri doldurabilirsiniz.

   ![Formu göster](./media/test-createuidefinition/show-ui-form.png)

### <a name="troubleshooting"></a>Sorun giderme

Önizleme'yi **seçtikten**sonra formunuz görüntülemezse, sözdizimi hatanız olabilir. Sağ kaydırma çubuğundaki kırmızı göstergeye bakın ve ona gidin.

![Sözdizimi hatasini göster](./media/test-createuidefinition/show-syntax-error.png)

Formunuz görüntülenmiyorsa ve bunun yerine gözyaşı damlası olan bir bulut simgesi görüyorsanız, formunuzda eksik özellik gibi bir hata vardır. Tarayıcınızda Web Geliştirici Araçlarını açın. **Konsol** arabiriminiz hakkında önemli iletiler görüntüler.

![Hatayı göster](./media/test-createuidefinition/show-error.png)

## <a name="use-test-script"></a>Test komut dosyalarını kullanma

Arabiriminizi portalda test etmek için, aşağıdaki komut dosyalarından birini yerel makinenize kopyalayın:

* [PowerShell yan yük komut dosyası - Az Modülü](https://github.com/Azure/azure-quickstart-templates/blob/master/SideLoad-AzCreateUIDefinition.ps1)
* [PowerShell yan yükleme komut dosyası - Azure Modülü](https://github.com/Azure/azure-quickstart-templates/blob/master/SideLoad-CreateUIDefinition.ps1)
* [Azure CLI yan yükleme komut dosyası](https://github.com/Azure/azure-quickstart-templates/blob/master/sideload-createuidef.sh)

Arayüz dosyanızı portalda görmek için indirilen komut dosyanızı çalıştırın. Komut dosyası, Azure aboneliğinizde bir depolama hesabı oluşturur ve createUiDefinition.json dosyanızı depolama hesabına yükler. Depolama hesabı, komut dosyasını ilk çalıştırdığınızda veya depolama hesabı silinmişse oluşturulur. Azure aboneliğinizde depolama hesabı zaten varsa, komut dosyası hesabı yeniden kullanır. Komut dosyası portalı açar ve dosyanızı depolama hesabından yükler.

Depolama hesabı için bir konum sağlayın ve createUiDefinition.json dosyanızın bulunduğu klasörü belirtin.

PowerShell için şunu kullanın:

```powershell
.\SideLoad-AzCreateUIDefinition.ps1 `
  -StorageResourceGroupLocation southcentralus `
  -ArtifactsStagingDirectory .\100-Marketplace-Sample
```

Azure CLI için şunu kullanın:

```bash
./sideload-createuidef.sh \
  -l southcentralus \
  -a .\100-Marketplace-Sample
```

createUiDefinition.json dosyanız komut dosyasıyla aynı klasördeyse ve depolama hesabını zaten oluşturduysanız, bu parametreleri sağlamanız gerekmez.

PowerShell için şunu kullanın:

```powershell
.\SideLoad-AzCreateUIDefinition.ps1
```

Azure CLI için şunu kullanın:

```bash
./sideload-createuidef.sh
```

Komut dosyası tarayıcınızda yeni bir sekme açar. Yönetilen uygulamayı oluşturmak için portalı arabiriminiz ile görüntüler.

Alanlar için değerler sağlayın. Bittiğinde, tarayıcınızın geliştirici araçları konsolunda bulunabilecek şablona geçirilen değerleri görürsünüz.

![Değerleri göster](./media/test-createuidefinition/show-json.png)

Dağıtım şablonunuzu test etmek için bu değerleri parametre dosyası olarak kullanabilirsiniz.

Portal özet ekranında asılıysa, çıktı bölümünde bir hata olabilir. Örneğin, var olmayan bir denetime başvuruvermiş olabilirsiniz. Çıktıdaki bir parametre boşsa, parametre var olmayan bir özelliğe başvuruyor olabilir. Örneğin, denetime başvuru geçerlidir, ancak özellik başvurusu geçerli değildir.

## <a name="test-your-solution-files"></a>Çözüm dosyalarınızı test edin

Portal arabiriminizin beklendiği gibi çalıştığını doğruladığınıza göre, createUiDefinition dosyanızın mainTemplate.json dosyanızla düzgün bir şekilde entegre olduğunu doğrulamanın zamanı doldu. CreateUiDefinition dosyası da dahil olmak üzere çözüm dosyalarınızın içeriğini sınamak için bir doğrulama komut dosyası testi çalıştırabilirsiniz. Komut dosyası JSON sözdizimini doğrular, metin alanlarındaki regex ifadelerini denetler ve portal arabiriminin çıktı değerlerinin şablonunuzun parametreleri ile eşleştirdiğinden emin olmasını sağlar. Bu komut dosyalarını çalıştırma hakkında bilgi için [şablonlar için statik doğrulama denetimlerini çalıştır'a](https://github.com/Azure/azure-quickstart-templates/tree/master/test)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Portal arabiriminizi doğruladıktan sonra, [Azure yönetilen uygulamanızı Market'te kullanıma sunhakkında](publish-marketplace-app.md)bilgi edinin.
