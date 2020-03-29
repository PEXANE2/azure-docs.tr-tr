---
title: FileUpload UI öğesi
description: Azure portalı için Microsoft.Common.FileUpload UI öğesini açıklar. Yönetilen bir uygulamayı dağıtırken kullanıcıların dosya yüklemesi gerekir.
author: tfitzmac
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: tomfitz
ms.openlocfilehash: 61e1c9fe07fdd29ebc00e7e3491472d073bc4e5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75652495"
---
# <a name="microsoftcommonfileupload-ui-element"></a>Microsoft.Common.FileUpload UI öğesi

Kullanıcının yüklenmesi için bir veya daha fazla dosya belirtmesine olanak tanıyan denetim.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Common.FileUpload](./media/managed-application-elements/microsoft.common.fileupload.png)

## <a name="schema"></a>Şema

```json
{
  "name": "element1",
  "type": "Microsoft.Common.FileUpload",
  "label": "Some file upload",
  "toolTip": "",
  "constraints": {
    "required": true,
    "accept": ".doc,.docx,.xml,application/msword"
  },
  "options": {
    "multiple": false,
    "uploadMode": "file",
    "openMode": "text",
    "encoding": "UTF-8"
  },
  "visible": true
}
```

## <a name="sample-output"></a>Örnek çıktı

options.multiple yanlışsa ve options.uploadMode dosyaise, çıktıda json dizesi olarak dosyanın içeriği vardır:

```json
"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."
```

options.multiple doğruysa ve'options.uploadMode dosyaise, çıktı da dosyaların içeriğini JSON dizisi olarak sunar:

```json
[
  "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
  "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
  "Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.",
  "Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."
]
```

options.multiple yanlış sayılsa ve options.uploadMode url ise, çıktının JSON dizesi olarak bir URL'si vardır:

```json
"https://myaccount.blob.core.windows.net/pictures/profile.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
```

options.multiple doğruysa ve options.uploadMode url ise, çıktıda JSON dizisi olarak URL'lerin bir listesi vardır:
```json
[
  "https://myaccount.blob.core.windows.net/pictures/profile1.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile2.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile3.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
]
```

CreateUiDefinition'ı sınarken, bazı tarayıcılar (Google Chrome gibi) tarayıcı konsolundaki Microsoft.Common.FileUpload öğesi tarafından oluşturulan URL'leri kesemez. URL'lerin tamamını kopyalamak için tek tek bağlantıları sağ tıklatmanız gerekebilir.

## <a name="remarks"></a>Açıklamalar

- `constraints.accept`tarayıcının dosya iletişim kutusunda gösterilen dosya türlerini belirtir. İzin verilen değerler için [HTML5 belirtimine](https://html.spec.whatwg.org/multipage/input.html#attr-input-accept) bakın. Varsayılan değer **null'** dur.
- Doğru `options.multiple` olarak **true**ayarlanmışsa, kullanıcı tarayıcının dosya iletişim kutusunda birden fazla dosya seçebilir. Varsayılan değer **false** şeklindedir.
- Bu öğe, `options.uploadMode`'nin değerine göre iki modda dosya yüklemeyi destekler. **Dosya** belirtilirse, çıktı da dosyanın içeriğini bir blob olarak alır. **Url** belirtilirse, dosya geçici bir konuma yüklenir ve çıktıblob URL'sine sahiptir. Geçici lekeler 24 saat sonra temizlenecek. Varsayılan değer **dosyadır.**
- Yüklenen bir dosya korunur. Çıktı URL'si, dağıtım sırasında dosyaya erişmek için bir [SAS belirteci](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) içerir.
- Dosyanın `options.openMode` nasıl okunduğunu belirler. Dosyanın düz metin olması bekleniyorsa, **metin**belirtin; else, **ikili**belirtin. Varsayılan değer **metindir.**
- Dosya ya **file** da `options.openMode` dosya olarak ayarlanmışsa ve ikili olarak ayarlanmışsa, çıktı base64-kodlanır. **binary** `options.uploadMode`
- `options.encoding`dosyayı okurken kullanılacak kodlamayı belirtir. Varsayılan değer **UTF-8'dir**ve yalnızca `options.openMode` **metne**ayarlandığında kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
* UI öğelerindeki ortak özelliklerin açıklaması için [createUiDefinition öğelerine](create-uidefinition-elements.md)bakın.
