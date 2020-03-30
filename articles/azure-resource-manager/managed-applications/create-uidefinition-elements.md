---
title: UI tanım öğeleri oluşturma
description: Azure portalı için ui tanımları yaparken kullanılacak öğeleri açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 02/06/2020
ms.author: tomfitz
ms.openlocfilehash: 12bf5d131001d95a3f4327f95c24125dbf3fb510
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77086712"
---
# <a name="createuidefinition-elements"></a>CreateUiDefinition öğeleri

Bu makalede, createuiDefinition tüm desteklenen öğeler için şema ve özellikleri açıklanır. 

## <a name="schema"></a>Şema

Çoğu öğe için şema aşağıdaki gibidir:

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
| ad | Evet | Bir öğenin belirli bir örneğine başvurmak için bir iç tanımlayıcı. Öğe adının en yaygın kullanımı, `outputs`belirtilen öğelerin çıkış değerlerinin şablonparametrelerine eşlendiği yerdir. Bir öğenin çıktı değerini başka bir öğenin `defaultValue` çıktı değerine bağlamak için de kullanabilirsiniz. |
| type | Evet | Öğe için işlemek için UI denetimi. Desteklenen türlerin listesi için [Öğeler'e](#elements)bakın. |
| etiket | Evet | Öğenin görüntü metni. Bazı öğe türleri birden çok etiket içerdiğinden, değer birden çok dize içeren bir nesne olabilir. |
| defaultDeğer | Hayır | Öğenin varsayılan değeri. Bazı öğe türleri karmaşık varsayılan değerleri destekler, bu nedenle değer bir nesne olabilir. |
| Araç ipucu | Hayır | Öğenin araç ucunda görüntülenecek metin. Benzer `label`şekilde, bazı öğeler birden çok araç uç dizeleri destekler. Satır çizgisi bağlantılar Markdown sözdizimi kullanılarak katışdırılabilir.
| Kısıtlama -ları | Hayır | Öğenin doğrulama davranışını özelleştirmek için kullanılan bir veya daha fazla özellik. Kısıtlamalar için desteklenen özellikler öğe türüne göre değişir. Bazı öğe türleri doğrulama davranışının özelleştirmesini desteklemez ve bu nedenle hiçbir kısıtlama özelliği vardır. |
| seçenekler | Hayır | Öğenin davranışını özelleştiren ek özellikler. Benzer, `constraints`desteklenen özellikleri öğe türüne göre değişir. |
| Görünür | Hayır | Öğenin görüntülenip görüntülenmediğini gösterir. , `true`öğe ve uygulanabilir alt öğeleri görüntülenirse. Varsayılan değer: `true`. Bu özelliğin değerini dinamik olarak denetlemek için [mantıksal işlevleri](create-uidefinition-functions.md#logical-functions) kullanın.

## <a name="elements"></a>Öğeler

Her öğe için belgeler bir Kullanıcı Arabirimi örneği, şema, öğenin davranışı (genellikle doğrulama ve desteklenen özelleştirme ile ilgili açıklamalar) ve örnek çıktı içerir.

- [Microsoft.Common.DropDown](microsoft-common-dropdown.md)
- [Microsoft.Common.FileUpload](microsoft-common-fileupload.md)
- [Microsoft.Common.InfoBox](microsoft-common-infobox.md)
- [Microsoft.Common.OptionsGroup](microsoft-common-optionsgroup.md)
- [Microsoft.Common.PasswordBox](microsoft-common-passwordbox.md)
- [Microsoft.Common.Section](microsoft-common-section.md)
- [Microsoft.Common.tagsByResource](microsoft-common-tagsbyresource.md)
- [Microsoft.Common.TextBlock](microsoft-common-textblock.md)
- [Microsoft.Common.TextBox](microsoft-common-textbox.md)
- [Microsoft.Compute.CredentialsCombo](microsoft-compute-credentialscombo.md)
- [Microsoft.Compute.SizeSelector](microsoft-compute-sizeselector.md)
- [Microsoft.Compute.UserNameTextBox](microsoft-compute-usernametextbox.md)
- [Microsoft.ManagedIdentity.IdentitySelector](microsoft-managedidentity-identityselector.md)
- [Microsoft.Network.PublicIpAddressCombo](microsoft-network-publicipaddresscombo.md)
- [Microsoft.Network.VirtualNetworkCombo](microsoft-network-virtualnetworkcombo.md)
- [Microsoft.Storage.MultiStorageAccountCombo](microsoft-storage-multistorageaccountcombo.md)
- [Microsoft.Storage.StorageAccountSelector](microsoft-storage-storageaccountselector.md)

## <a name="next-steps"></a>Sonraki adımlar

UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
