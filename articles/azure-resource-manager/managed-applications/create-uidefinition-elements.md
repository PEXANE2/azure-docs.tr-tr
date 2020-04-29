---
title: UI tanımı öğeleri oluşturma
description: Azure portal için Kullanıcı arabirimi tanımları oluştururken kullanılacak öğeleri açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 02/06/2020
ms.author: tomfitz
ms.openlocfilehash: 12bf5d131001d95a3f4327f95c24125dbf3fb510
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77086712"
---
# <a name="createuidefinition-elements"></a>Createuıdefinition öğeleri

Bu makalede bir Createuıdefinition öğesinin tüm desteklenen öğelerinin şeması ve özellikleri açıklanmaktadır. 

## <a name="schema"></a>Şema

Çoğu öğenin şeması aşağıdaki gibidir:

```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "my value",
  "toolTip": "Provide a descriptive name.",
  "constraints": {},
  "options": {},
  "visible": true
}
```

| Özellik | Gerekli | Açıklama |
| -------- | -------- | ----------- |
| ad | Yes | Bir öğenin belirli bir örneğine başvurmak için bir iç tanımlayıcı. Öğe adının `outputs`en yaygın kullanımı, belirtilen öğelerin çıkış değerlerinin şablonun parametreleriyle eşlendiği yerdir. Ayrıca, bir öğenin `defaultValue` çıkış değerini başka bir öğenin öğesine bağlamak için de kullanabilirsiniz. |
| type | Yes | Öğesi için işlenecek Kullanıcı arabirimi denetimi. Desteklenen türlerin listesi için bkz. [öğeleri](#elements). |
| etiket | Yes | Öğenin görüntüleme metni. Bazı öğe türleri birden çok etiket içerir, bu nedenle değer birden fazla dize içeren bir nesne olabilir. |
| Değerinin | Hayır | Öğesinin varsayılan değeri. Bazı öğe türleri karmaşık varsayılan değerleri destekler, bu yüzden değer bir nesne olabilir. |
| Ipucuna | Hayır | Öğenin araç ipucunda görüntülenecek metin. Benzer şekilde `label`, bazı öğeler birden çok araç ipucu dizesini destekler. Satır içi bağlantılar markın sözdizimi kullanılarak gömülebilir.
| kısıtlamaları | Hayır | Öğesinin doğrulama davranışını özelleştirmek için kullanılan bir veya daha fazla özellik. Kısıtlamaların desteklenen özellikleri öğe türüne göre farklılık gösterir. Bazı öğe türleri doğrulama davranışının özelleştirilmesini desteklemez ve bu nedenle kısıtlama özelliğine sahip değildir. |
| seçenekler | Hayır | Öğesinin davranışını özelleştiren ek özellikler. Benzer şekilde `constraints`, desteklenen özellikler öğe türüne göre farklılık gösterir. |
| görüne | Hayır | Öğenin görüntülenip görüntülenmeyeceğini gösterir. İse `true`, öğesi ve ilgili alt öğeleri görüntülenir. Varsayılan değer: `true`. Bu özelliğin değerini dinamik olarak denetlemek için [mantıksal işlevleri](create-uidefinition-functions.md#logical-functions) kullanın.

## <a name="elements"></a>Öğeler

Her bir öğe için belge, Kullanıcı arabirimi örneği, şema, öğe (genellikle doğrulama ve desteklenen özelleştirme ile ilgili) ve örnek çıkış gibi bir kullanıcı arabirimi örneği, şema, açıklamalar içerir.

- [Microsoft.Common.DropDown](microsoft-common-dropdown.md)
- [Microsoft.Common.FileUpload](microsoft-common-fileupload.md)
- [Microsoft. Common. Infobox](microsoft-common-infobox.md)
- [Microsoft.Common.OptionsGroup](microsoft-common-optionsgroup.md)
- [Microsoft.Common.PasswordBox](microsoft-common-passwordbox.md)
- [Microsoft.Common.Section](microsoft-common-section.md)
- [Microsoft. Common. TagsByResource](microsoft-common-tagsbyresource.md)
- [Microsoft. Common. TextBlock](microsoft-common-textblock.md)
- [Microsoft.Common.TextBox](microsoft-common-textbox.md)
- [Microsoft.Compute.CredentialsCombo](microsoft-compute-credentialscombo.md)
- [Microsoft.Compute.SizeSelector](microsoft-compute-sizeselector.md)
- [Microsoft.Compute.UserNameTextBox](microsoft-compute-usernametextbox.md)
- [Microsoft. Managedıdentity. ıdentityselector](microsoft-managedidentity-identityselector.md)
- [Microsoft.Network.PublicIpAddressCombo](microsoft-network-publicipaddresscombo.md)
- [Microsoft.Network.VirtualNetworkCombo](microsoft-network-virtualnetworkcombo.md)
- [Microsoft.Storage.MultiStorageAccountCombo](microsoft-storage-multistorageaccountcombo.md)
- [Microsoft.Storage.StorageAccountSelector](microsoft-storage-storageaccountselector.md)

## <a name="next-steps"></a>Sonraki adımlar

UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
