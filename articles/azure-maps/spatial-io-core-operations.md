---
title: Çekirdek IO operasyonları | Microsoft Azure Haritaları
description: Uzamsal IO modülünden çekirdek kitaplıkları kullanarak XML'i verimli bir şekilde nasıl okuyup yazarak verileri sınırlandırmayı öğrenin.
author: philmea
ms.author: philmea
ms.date: 03/03/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 0b8fe1b319dc480879944d28f10645025a8cb38e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371452"
---
# <a name="core-io-operations"></a>Çekirdek IO operasyonları

Uzamsal veri dosyalarını okumak için araçlar sağlamanın yanı sıra, uzamsal IO modülü XML okumak ve yazmak için temel temel kitaplıkları ortaya çıkarır ve verileri hızlı ve verimli bir şekilde sınırlandırır.

Ad `atlas.io.core` alanı, CSV ve XML verilerini hızla okuyup yazabilen iki alt düzey sınıf içerir. Bu temel sınıflar, Uzamsal IO modülündeki uzamsal veri okuyucularına ve yazarlara güç sağlar. CSV veya XML dosyaları için ek okuma ve yazma desteği eklemek için bunları kullanmaktan çekinmeyin.
 
## <a name="read-delimited-files"></a>Sınırlı dosyaları okuma

Sınıf, `atlas.io.core.CsvReader` sınırlı veri kümeleri içeren dizeleri okur. Bu sınıf verileri okumak için iki yöntem sağlar:

- İşlev `read` tam veri kümesini okur ve sınırlı veri kümesinin tüm hücrelerini temsil eden iki boyutlu bir dize dizisini döndürür.
- İşlev, `getNextRow` sınırlı bir veri kümesindeki her metin satırını okur ve bu veri kümesi satırındaki tüm hücreleri temsil eden bir dize dizisini döndürür. Kullanıcı satırı işleyebilir ve bir sonraki satırı işlemeden önce bu satırdan herhangi bir gereksiz bellek atabilir. Yani, fonksiyon daha fazla bellek verimlidir.

Varsayılan olarak, okuyucu delimiter olarak virgül karakterini kullanır. Ancak, sınırlayıcı herhangi bir tek karakter veya `'auto'`ayarlanabilir. `'auto'`Ayarlandığında, okuyucu dizedeki ilk metin satırını analiz edecektir. Daha sonra, delimiter olarak kullanmak için aşağıdaki tablodan en yaygın karakteri seçecektir.

| | |
| :-- | :-- |
| Virgül | `,` |
| Tab | `\t` |
| Boru | `|` |

Bu okuyucu, sınırlayıcı karakterini içeren hücreleri işlemek için kullanılan metin niteleyicilerini de destekler. Teklif (`'"'`) karakteri varsayılan metin niteleyicisidir, ancak herhangi bir tek karakterle değiştirilebilir.

## <a name="write-delimited-files"></a>Sınırlı dosyaları yazma

Bir `atlas.io.core.CsvWriter` dizi nesne sınırlı bir dize olarak yazar. Herhangi bir tek karakter bir sınırlayıcı veya metin niteleyici olarak kullanılabilir. Varsayılan delimiter virgül`','`( ) ve varsayılan metin`'"'`niteleyici teklif ( ) karakteridir.

Bu sınıfı kullanmak için aşağıdaki adımları izleyin:

- Sınıfın bir örneğini oluşturun ve isteğe bağlı olarak özel bir sınırlayıcı veya metin niteleyici ayarlayın.
- `write` İşlev veya `writeRow` işlevi kullanarak sınıfa veri yazın. `write` İşlev için, birden çok satırı ve hücreyi temsil eden iki boyutlu bir nesne dizisini geçirin. `writeRow` İşlevi kullanmak için, birden çok sütunlu bir veri satırını temsil eden bir dizi nesne geçirin.
- Sınırlı `toString` dize almak için işlevi çağırın. 
- İsteğe bağlı `clear` olarak, yazarı yeniden kullanılabilir hale getirmek ve kaynak `delete` tahsisini azaltmak için yöntemi arayın veya yazar örneğini elden çıkarmak için yöntemi arayın.

> [!Note]
> Yazılan sütun sayısı, yazara aktarılan verilerin ilk satırındaki hücre sayısıyla sınırlandırılacaktır.

## <a name="read-xml-files"></a>XML dosyalarını okuma

Sınıf, `atlas.io.core.SimpleXmlReader` XML dosyalarını ayrıştırmada .'dan daha `DOMParser`hızlıdır. Ancak, `atlas.io.core.SimpleXmlReader` sınıf XML dosyalarının iyi biçimlendirilmiş olmasını gerektirir. İyi biçimlendirilmemiş XML dosyaları (örneğin kapanış etiketlerinin eksik olması) büyük olasılıkla bir hataya neden olur.

Aşağıdaki kod, bir XML `SimpleXmlReader` dizesini Bir JSON nesnesine ayrıştmak ve istenilen biçimde seri hale getirmek için sınıfın nasıl kullanılacağını gösterir.

```javascript
//Create an instance of the SimpleXmlReader and parse an XML string into a JSON object.
var xmlDoc = new atlas.io.core.SimpleXmlReader().parse(xmlStringToParse);

//Verify that the root XML tag name of the document is the file type your code is designed to parse.
if (xmlDoc && xmlDoc.root && xmlDoc.root.tagName && xmlDoc.root.tagName === '<Your desired root XML tag name>') {

    var node = xmlDoc.root;

    //Loop through the child node tree to navigate through the parsed XML object.
    for (var i = 0, len = node.childNodes.length; i < len; i++) {
        childNode = node.childNodes[i];

        switch (childNode.tagName) {
            //Look for tag names, parse and serialized as desired.
        }
    }
}
```

## <a name="write-xml-files"></a>XML dosyalarını yazma

Sınıf `atlas.io.core.SimpleXmlWriter` iyi biçimlendirilmiş XML'i bellek etkin bir şekilde yazar.

Aşağıdaki kod, iyi biçimlendirilmiş `SimpleXmlWriter` bir XML dizesi oluşturmak için sınıfın nasıl kullanılacağını gösterir.

```javascript
//Create an instance of the SimpleXmlWriter class.
var writer = new atlas.io.core.SimpleXmlWriter();

//Start writing the document. All write functions return a reference to the writer, making it easy to chain the function calls to reduce the code size.
writer.writeStartDocument(true)
    //Specify the root XML tag name, in this case 'root'
    .writeStartElement('root', {
        //Attributes to add to the root XML tag.
        'version': '1.0',
        'xmlns': 'http://www.example.com',
         //Example of a namespace.
        'xmlns:abc': 'http://www.example.com/abc'
    });

//Start writing an element that has the namespace abc and add other XML elements as children.
writer.writeStartElement('abc:parent');

//Write a simple XML element like <title>Azure Maps is awesome!</title>
writer.writeElement('title', 'Azure Maps is awesome!');

//Close the element that we have been writing children to.
writer.writeEndElement();

//Finish writing the document by closing the root tag and the document.
writer.writeEndElement().writeEndDocument();

//Get the generated XML string from the writer.
var xmlString = writer.toString();
```

Yukarıdaki koddan oluşturulan XML aşağıdaki gibi görünür.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<root version="1.0" xmlns="http://www.example.com" xmlns:abc="http://www.example.com/abc">
    <abc:parent>
        <title>Azure Maps is awesome!</title>
    </abc:parent>
</root>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [CsvReader](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.io.core.csvreader)

> [!div class="nextstepaction"]
> [CsvYazar](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.io.core.csvwriter)

> [!div class="nextstepaction"]
> [SimpleXmlReader](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.io.core.simplexmlreader)

> [!div class="nextstepaction"]
> [SimpleXmlWriter](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.io.core.simplexmlwriter)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Desteklenen veri biçimi ayrıntıları](spatial-io-supported-data-format-details.md)