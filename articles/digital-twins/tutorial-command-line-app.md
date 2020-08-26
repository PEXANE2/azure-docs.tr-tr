---
title: 'Öğretici: örnek bir istemci uygulamasıyla ilgili temel bilgileri araştırma'
titleSuffix: Azure Digital Twins
description: Örnek bir komut satırı uygulaması kullanarak Azure dijital TWINS SDK 'larını keşfetmeye yönelik öğretici
author: baanders
ms.author: baanders
ms.date: 5/8/2020
ms.topic: tutorial
ms.service: digital-twins
ms.openlocfilehash: 58013ba84e1ede36ed54284af4d20b943d63436f
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88855865"
---
# <a name="tutorial-explore-azure-digital-twins-with-a-sample-client-app"></a>Öğretici: örnek bir istemci uygulamasıyla Azure dijital TWINS 'i araştırma

Bu öğreticide, bir Azure dijital TWINS örneğiyle etkileşim kurmak için bir komut satırı istemci uygulaması uygulayan örnek bir uygulama tanıtılmıştır. İstemci uygulaması, [*öğretici: istemci uygulaması kodu*](tutorial-code.md)olarak yazılmış birine benzer.

Bu örneği, model yükleme, TWINS oluşturma ve değiştirme ve ilişki oluşturma gibi temel Azure dijital TWINS eylemlerini gerçekleştirmek için kullanabilirsiniz. Ayrıca, Azure dijital TWINS API 'Leri hakkında bilgi edinmek ve örnek projeyi istediğiniz gibi değiştirerek kendi komutlarınızı uygulamayı yapmak için örneğin koduna bakabilirsiniz.

Bu öğreticide,...
1. Azure dijital TWINS örneği ayarlama
2. Örnek komut satırı uygulamasını örnekle etkileşim kuracak şekilde yapılandırma
3. **Modeller**, **dijital TWINS**, **ilişkiler**ve **sorgular** dahil olmak üzere Azure dijital TWINS 'i araştırmak için komut satırı uygulamasını kullanın

[!INCLUDE [Azure Digital Twins tutorial: sample prerequisites](../../includes/digital-twins-tutorial-sample-prereqs.md)]

[!INCLUDE [Azure Digital Twins tutorial: configure the sample project](../../includes/digital-twins-tutorial-sample-configure.md)]

## <a name="explore-with-the-sample-solution"></a>Örnek çözümle araştırma

Örnek ve örnek uygulama yapılandırıldıktan sonra, temel bir Azure dijital TWINS çözümü oluşturmak ve araştırmak için örnek projeyi ve önceden yazılmış bazı örnek kodları kullanacaksınız. Ana çözüm bileşenleri **modellerdir**, **dijital TWINS**ve **ilişkilerdir**ve bir ortamın sorgulanabilir **ikizi grafiğine** neden olur.

### <a name="model-a-physical-environment-with-dtdl"></a>DTDL ile fiziksel ortam modelleme

Azure dijital TWINS çözümü oluşturmanın ilk adımı, ortamınız için ikizi [**modellerini**](concepts-models.md) tanımlıyor. 

Modeller, nesne odaklı programlama dillerinde sınıflarla benzerdir; Bunlar, daha sonra izlemek ve örneklendirilecek [dijital TWINS](concepts-twins-graph.md) için Kullanıcı tanımlı şablonlar sağlar. Bunlar, **dijital TWINS tanım dili (DTDL)** adlı JSON benzeri bir dilde yazılır ve bir ikizi *özellikleri*, *telemetri*, *ilişki*ve *bileşen*tanımlayabilir.

> [!NOTE]
> DTDL Ayrıca dijital TWINS 'teki *komutların* tanımına izin verir. Ancak, şu anda Azure Digital TWINS hizmetinde komutlar desteklenmemektedir.

_**AdtE2ESample**_ projesinin açık olduğu Visual Studio pencerenizde, *AdtSampleApp\SampleClientApp\Models* klasörüne gitmek için *Çözüm Gezgini* bölmesini kullanın. Bu klasör örnek modeller içerir.

Düzenle penceresinde açmak için *Room.js* seçin ve aşağıdaki yollarla değiştirin:

* Bu modelin daha güncel bir sürümünü sağladıklarını belirtmek için **sürüm numarasını güncelleştirin**. Değerin sonundaki *1* `@id` değerini *2*olarak değiştirerek bunu yapın. Geçerli sürüm numarasından daha büyük bir sayı de çalışacaktır.
* **Bir özelliği düzenleyin**. `Humidity`Özelliğin adını *Humidtylevel* (veya isterseniz farklı bir şey) olarak değiştirin. *Humidtylevel*' dan farklı bir şey kullanırsanız, kullandığınız şeyi hatırlayın ve öğretici genelinde *Humidtylevel* yerine bunu kullanmaya devam edin).
* **Özellik ekleyin**. 15. `HumidityLevel` satırda sonlanan özelliğin altında, Odaya bir özellik eklemek için aşağıdaki kodu yapıştırın `RoomName` :

    ```json
    ,
    {
      "@type": "Property",
      "name": "RoomName",
      "schema": "string"
    }
    ```
* **Ilişki ekleyin**. `RoomName`Az önce eklediğiniz özelliğin altında, bu tür bir ikizi oluşturmak için aşağıdaki kodu yapıştırın. *contains*

    ```json
    ,
    {
      "@type": "Relationship",
      "name": "contains",
    }
    ```

İşiniz bittiğinde, güncelleştirilmiş modelin şöyle görünmesi gerekir:

:::image type="content" source="media/tutorial-command-line-app/room-model.png" alt-text="Güncelleştirilmiş sürüm numarasıyla birlikte Room.jsdüzenlendi, Humidtylevel ve RoomName özellikleri ve ilişki içerir" border="false":::

Taşımadan önce dosyayı kaydettiğinizden emin olun.

> [!TIP]
> Kendi modelinizi oluşturmayı denemek istiyorsanız, *Oda* modeli kodunu *AdtSampleApp\SampleClientApp\Models* klasöründe bir *. JSON* uzantısıyla kaydedeceğiniz yeni bir dosyaya yapıştırabilirsiniz. Ardından, istediğiniz şeyi temsil etmek üzere Özellikler ve ilişkiler ekleyerek bir adım adım oynayın. Ayrıca, bu klasördeki diğer örnek modellere fikir için göz atabilirsiniz.

> [!TIP] 
> DTDL 'nin geçerli olduğundan emin olmak için model belgelerini denetlemek için kullanabileceğiniz dilden bağımsız bir [Dtdl doğrulayıcısı örneği](https://docs.microsoft.com/samples/azure-samples/dtdl-validator/dtdl-validator) vardır. Bu, [*nasıl yapılır: modellerinizi ayrıştırma ve doğrulama*](how-to-parse-models.md)hakkında daha fazla bilgi edinmek için dtdl ayrıştırıcısı kitaplığı üzerine kurulmuştur.

### <a name="get-started-with-the-command-line-app"></a>Komut satırı uygulaması ile çalışmaya başlama

Bir model tanımladığınıza göre, kalan adımlar Azure dijital TWINS örneğiniz ile etkileşim kurmak için örnek uygulamayı kullanmayı içerir. Araç çubuğunda Bu düğmeyle projeyi Çalıştır:

:::image type="content" source="media/tutorial-command-line-app/start-button-sample.png" alt-text="Visual Studio Başlangıç düğmesi (SampleClientApp Projesi)":::

Bir konsol penceresi açılır, kimlik doğrulama işlemi gerçekleştirebilir ve bir komut için bekler. 
* Kimlik doğrulaması tarayıcı aracılığıyla gerçekleştirilir: varsayılan Web tarayıcınız, bir kimlik doğrulama istemiyle açılır. Azure kimlik bilgilerinizle oturum açmak için bu istemi kullanın. Ardından tarayıcı sekmesini veya penceresini kapatabilirsiniz.

Proje konsolunun neye benzediğinin ekran görüntüsü aşağıda verilmiştir:

:::image type="content" source="media/tutorial-command-line-app/command-line-app.png" alt-text="Komut satırı uygulamasından hoş geldiniz iletisi":::

> [!TIP]
> Bu projeyle kullanabileceğiniz tüm olası komutların listesi için, `help` Proje konsoluna girip Return tuşuna basın.
> :::image type="content" source="media/tutorial-command-line-app/command-line-app-help.png" alt-text="Yardım komutunun çıkışı":::

Bu öğreticideki adımların geri kalanında proje konsolunu çalışır durumda tutun.

#### <a name="upload-models-to-azure-digital-twins"></a>Azure dijital TWINS 'e modelleri yükleme

Modelleri tasarladıktan sonra Azure dijital TWINS örneğinizi yüklemeniz gerekir. Bu, Azure dijital TWINS hizmeti örneğinizi kendi özel etki alanı sözlüğinizle yapılandırır. Modelleri karşıya yükledikten sonra, bunları kullanan ikizi örnekleri oluşturabilirsiniz.

Proje konsolu penceresinde, güncelleştirilmiş *Oda* modelinizi karşıya yüklemek için aşağıdaki komutu çalıştırın ve farklı türlerde TWINS oluşturmak için bir sonraki bölümde kullanacağınız bir *kat* modeli de vardır.

```cmd/sh
CreateModels Room Floor
```

Çıktı, modellerin başarıyla oluşturulduğunu göstermelidir.

> [!TIP]
> Kendi modelinizi daha önce tasarladıktan sonra, Yukarıdaki komutta bulunan dosya adını (uzantıyı da bırakabilirsiniz) ekleyerek buraya yükleyebilirsiniz `Room Floor` .

Komutu çalıştırarak modellerin oluşturulduğunu doğrulayın `GetModels true` . Bu, karşıya yüklenen tüm modeller için Azure dijital TWINS örneğini sorgular ve tam bilgilerini yazdırır. Sonuçlarda düzenlenmiş *Oda* modelini arayın:

:::image type="content" source="media/tutorial-command-line-app/output-get-models.png" alt-text="Güncelleştirilmiş Oda modelini gösteren GetModel sonuçları":::

#### <a name="errors"></a>Hatalar

Örnek uygulama, hizmetten gelen hataları da işler. 

`CreateModels`İkinci bir kez karşıya yüklediğiniz modellerden birini yeniden karşıya yüklemeyi denemek için komutu yeniden çalıştırın:

```cmd/sh
CreateModels Room
```

Modellerin üzerine yazılamaz, bu işlem artık bir hizmet hatası döndürüyor.
Mevcut modellerin nasıl silineceği hakkındaki ayrıntılar için bkz. [*nasıl yapılır: özel modelleri yönetme*](how-to-manage-model.md).
```cmd/sh
Response 409: Service request failed.
Status: 409 (Conflict)

Content:
{"error":{"code":"ModelAlreadyExists","message":"Could not add model dtmi:example:Room;2 as it already exists. Use Model_List API to view models that already exist. See the Swagger example.(http://aka.ms/ModelListSwSmpl)"}}

Headers:
Strict-Transport-Security: REDACTED
Date: Wed, 20 May 2020 00:53:49 GMT
Content-Length: 223
Content-Type: application/json; charset=utf-8
```

### <a name="create-digital-twins"></a>Dijital TWINS oluşturma

Artık bazı modeller Azure dijital TWINS örneğinizi karşıya yüklediğinize göre, model tanımlarına göre [**dijital TWINS**](concepts-twins-graph.md) oluşturabilirsiniz. Dijital TWINS, iş ortamınızdaki varlıkları, bir gruptaki algılayıcılar gibi şeyler, bir bina içindeki odalar veya bir otomobilde ışıklar temsil eder. 

Dijital bir ikizi oluşturmak için `CreateDigitalTwin` komutunu kullanın. İkizi 'in temel aldığı modele başvurmanız gerekir ve isteğe bağlı olarak modeldeki tüm özellikler için başlangıç değerlerini tanımlayabilir. Bu aşamada herhangi bir ilişki bilgisi iletmeniz gerekmez.

Daha önce güncelleştirdiğiniz *Oda* modeline ve başka bir model, *kata*göre birkaç TWINS oluşturmak için bu kodu çalışan proje konsolunda çalıştırın. *Odanın* üç özelliği olduğunu anımsayın, bu nedenle bunlar için başlangıç değerleriyle bağımsız değişken sağlayabilirsiniz.

```cmd/sh
CreateDigitalTwin dtmi:example:Room;2 room0 RoomName string Room0 Temperature double 70 HumidityLevel double 30
CreateDigitalTwin dtmi:example:Room;2 room1 RoomName string Room1 Temperature double 80 HumidityLevel double 60
CreateDigitalTwin dtmi:example:Floor;1 floor0
CreateDigitalTwin dtmi:example:Floor;1 floor1
```

> [!TIP]
> Kendi modelinizi daha önce karşıya yüklediyseniz, kendi `CreateDigitalTwin` model türünden bir ikizi eklemek için yukarıdaki komutlara göre kendi komutlarınızı yapmayı deneyin.

Bu komutların çıktısı, TWINS 'nin başarıyla oluşturulduğunu göstermelidir. 

:::image type="content" source="media/tutorial-command-line-app/output-create-digital-twin.png" alt-text="Floor0, Floor1, room0 ve room1 gösteren CreateDigitalTwin komutlarının sonuçlarından alıntı yapın":::

Ayrıca, komutunu çalıştırarak TWINS 'nin oluşturulduğunu doğrulayabilirsiniz `Query` . Bu komut, içerdiği tüm dijital TWINS için Azure dijital TWINS örneğinizi sorgular. Sonuçlarda *floor0*, *Floor1*, *room0*ve *room1* TWINS ' i arayın.

#### <a name="modify-a-digital-twin"></a>Dijital ikizi değiştirme

Ayrıca, oluşturduğunuz bir ikizi özelliklerini de değiştirebilirsiniz. *Room0*'In RoomName öğesini *Room0* ' den *PresidentialSuite*' ye değiştirmek için bu komutu çalıştırmayı deneyin:

```cmd/sh
UpdateDigitalTwin room0 add /RoomName string PresidentialSuite
```

Çıktı, ikizi başarıyla güncelleştirildiğini göstermelidir.

Ayrıca, *room0*'un bilgilerini görmek için bu komutu çalıştırarak da doğrulayabilirsiniz:

```cmd/sh
GetDigitalTwin room0
```

Çıktı, güncelleştirilmiş adı yansıtmalıdır.

> [!NOTE]
> Temel REST API, bir ikizi güncelleştirmelerini tanımlamak için JSON Patch kullanır. Komut satırı uygulaması bu biçimi yansıtır, böylece temeldeki API 'Lerin gerçekten beklendiğini deneyebilirsiniz.

### <a name="create-a-graph-by-adding-relationships"></a>İlişkiler ekleyerek bir grafik oluşturma

Daha sonra, bu TWINS arasında bir [**ikizi grafiğine**](concepts-twins-graph.md)bağlanmak için bazı **ilişkiler** oluşturabilirsiniz. İkizi grafikleri tüm ortamı temsil etmek için kullanılır. 

Bir ilişki eklemek için `CreateRelationship` komutunu kullanın. İlişkinin geldiği ikizi, eklenecek ilişkinin türü ve ilişkinin bağlandığı ikizi ' i belirtin. Son olarak, ilişki için bir ad (KIMLIK) sağlayın.

Daha önce oluşturduğunuz her bir *tabandan* bir "Contains" ilişkisini karşılık gelen *odaya* ikizi eklemek için aşağıdaki kodu çalıştırın. Bunun mümkün olması için, *taban* modelde tanımlı bir ilişki *içeren* bir ilişki olması gerektiğini unutmayın.

```cmd/sh
CreateRelationship floor0 contains room0 relationship0
CreateRelationship floor1 contains room1 relationship1
```

Bu komutların çıktısı, ilişkilerin başarıyla oluşturulduğunu onaylar:

:::image type="content" source="media/tutorial-command-line-app/output-create-relationship.png" alt-text="Relationship0 ve relationship1 gösteren CreateRelationship komutlarının sonuçlarından alıntı":::

Ayrıca, Azure dijital TWINS örneğiniz ilişkilerini sorgulayan aşağıdaki komutlardan herhangi biriyle ilişkileri doğrulayabilirsiniz.
* Her bir tabandan gelen tüm ilişkileri görmek için (ilişkiyi bir taraftan görüntüleme),
    ```cmd/sh
    GetRelationships floor0
    GetRelationships floor1
    ```
* Her odaya ulaşan tüm ilişkileri görmek için ("diğer" taraftan ilişkiyi görüntüleme),
    ```cmd/sh
    GetIncomingRelationships room0
    ```
* Bu ilişkileri tek tek sorgulamak için 
    ```cmd/sh
    GetRelationship floor0 contains relationship0
    GetRelationship floor1 contains relationship1
    ```

Bu öğreticide ayarladığınız TWINS ve ilişkiler aşağıdaki kavramsal grafiği oluşturur:

:::image type="content" source="media/tutorial-command-line-app/sample-graph.png" alt-text="Relationship0 ile room0 arasında bağlanan floor0 ve Floor1 ile relationship1 arasında bağlanan bir grafik" border="false":::

### <a name="query-the-twin-graph-to-answer-environment-questions"></a>Ortam sorularını yanıtlamak için ikizi grafiğini sorgulama

Azure dijital TWINS 'in ana özelliği, ortamınız hakkında soruları yanıtlamak için ikizi grafınızı kolayca ve verimli bir şekilde [sorgulayabilir](concepts-query-language.md) . Bunun nasıl olduğuna ilişkin bir fikir edinmek için çalışan proje konsolunda aşağıdaki komutları çalıştırın.

* **Ortamınızdaki Azure dijital TWINS 'te gösterilen tüm varlıklar nelerdir?** (tümünü sorgula)

    ```cmd/sh
    Query
    ```

    Bu, ortamınızı bir bakışta almanıza olanak sağlar ve her şeyin Azure dijital TWINS 'in içinde olmasını istediğiniz şekilde gösterildiğinden emin olun. Bunun sonucu, her dijital ikizi ayrıntılarını içeren bir çıktıdır. Aşağıda bir alıntı verilmiştir:

    :::image type="content" source="media/tutorial-command-line-app/output-query-all.png" alt-text="İkizi sorgusunun kısmi sonuçları, room0 ve Floor1 gösteriliyor":::

    >[!NOTE]
    >`Query`Ek bağımsız değişken içermeyen komut, ' nin eşdeğeridir `Query SELECT * FROM DIGITALTWINS` .

* **Ortammdaki tüm odalar nelerdir?** (modele göre sorgu)

    ```cmd/sh
    Query SELECT * FROM DIGITALTWINS T WHERE IS_OF_MODEL(T, 'dtmi:example:Room;2')
    ```

    Hangi bilgilerin temsil edildiği hakkında daha ayrıntılı bilgi edinmek için sorgunuzu belirli bir türden TWINS ile kısıtlayabilirsiniz. Bunun sonucu, *room0* ve *room1*gösterir, ancak *floor0* veya *Floor1* **göstermez (** Odalar, odaların değil, bu nedenle).
    
    :::image type="content" source="media/tutorial-command-line-app/output-query-model.png" alt-text="Yalnızca room0 ve room1 gösteren model sorgusunun sonuçları":::

* ***Floor0*üzerindeki tüm odalar nelerdir?** (ilişkiye göre sorgulama)

    ```cmd/sh
    Query SELECT room FROM DIGITALTWINS floor JOIN room RELATED floor.contains where floor.$dtId = 'floor0'
    ```

    TWINS 'in nasıl bağlı olduğu hakkında bilgi edinmek veya sorgunuzu belirli bir alanla kısıtlamak için grafiğinizde ilişkiler temelinde sorgulama yapabilirsiniz. Yalnızca *room0* *floor0*, bu nedenle sonuçtaki tek yer.

    :::image type="content" source="media/tutorial-command-line-app/output-query-relationship.png" alt-text="İlişki sorgusunun sonuçları, room0 gösteriliyor":::

* **75 üzerinde sıcaklık olan ortammdaki tüm TWINS nedir?** (özelliğe göre sorgu)

    ```cmd/sh
    Query SELECT * FROM DigitalTwins T WHERE T.Temperature > 75
    ```

    Ortamınızda dikkat etmeniz gerekebilecek aykırı değerleri bulma dahil olmak üzere çeşitli soruları yanıtlamak için, özellikleri temel alarak grafik sorgulama yapabilirsiniz. Diğer karşılaştırma işleçleri ( *<* , *>* , *=* veya *! =*) de desteklenir. *room1* , 80 sıcaklığını içerdiğinden burada sonuçlarda görüntülenir.

    :::image type="content" source="media/tutorial-command-line-app/output-query-property.png" alt-text="Yalnızca room1 gösteren özellik sorgusunun sonuçları":::

* **75 üzerindeki sıcaklığa sahip *floor0* üzerindeki tüm odalar nelerdir?** (bileşik sorgu)

    ```cmd/sh
    Query SELECT room FROM DIGITALTWINS floor JOIN room RELATED floor.contains where floor.$dtId = 'floor0' AND IS_OF_MODEL(room, 'dtmi:example:Room;2') AND room.Temperature > 75
    ```

    SQL 'de olduğu gibi önceki sorguları,, gibi Birleşik işleçler kullanarak da birleştirebilirsiniz `AND` `OR` `NOT` . Bu sorgu `AND` , önceki ikizi sıcaklıklar hakkında daha belirgin bir sorgu oluşturmak için kullanır. Sonuç artık yalnızca, *floor0*üzerinde olan 75 üzerinde sıcaklık olan odaları (Bu durumda, bunlardan hiçbiri) içerir. Sonuç kümesi boş.

    :::image type="content" source="media/tutorial-command-line-app/output-query-compound.png" alt-text="Bileşik sorgunun sonuçları, sonuç yok gösteriliyor":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticideki proje, bir sonraki öğreticinin temelini oluşturur, [*öğretici: uçtan uca bir çözümü bağlama*](tutorial-end-to-end.md). Sonraki öğreticiye devam etmeyi planlıyorsanız, bu Azure dijital TWINS örneğini ve yapılandırılmış örnek uygulamayı kullanmaya devam etmek için ayarladığınız kaynakları koruyabilirsiniz.
* Bu durumda, örnek uygulama `DeleteAllTwins` ve `DeleteAllModels` komutlarını kullanarak, örneğiniz içindeki TWINS 'leri ve modelleri temizleyebilirsiniz. Bu, bir sonraki öğreticiye yönelik bir temiz kurşun işlem sağlar.

Bu öğreticide oluşturulan kaynaklara artık ihtiyacınız yoksa, bunları silmek için aşağıdaki adımları izleyin.

[Azure Cloud Shell](https://shell.azure.com)kullanarak, [az Group Delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) komutuyla bir kaynak grubundaki tüm Azure kaynaklarını silebilirsiniz. Bu, kaynak grubunu ve Azure dijital TWINS örneğini kaldırır.

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. 

Bir Azure Cloud Shell açın ve kaynak grubunu ve içerdiği her şeyi silmek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name <your-resource-group>
```

Ardından, aşağıdaki komutla istemci uygulamanız için oluşturduğunuz Azure Active Directory Uygulama kaydını silin:

```azurecli
az ad app delete --id <your-application-ID>
```

Son olarak, indirdiğiniz proje örnek klasörünü yerel makinenize silin.

## <a name="next-steps"></a>Sonraki adımlar 

Bu öğreticide, örnekle etkileşimde bulunmak üzere bir örnek ve bir istemci uygulaması ayarlayarak Azure dijital TWINS ile çalışmaya başladıysanız. Azure dijital TWINS 'i incelemek, modeller, dijital WINS 'leri ve ilişkiler oluşturmak için istemci uygulamasını kullandınız. Ayrıca, Azure dijital TWINS 'in bir ortam hakkında ne tür sorular yanıtlayabileceklerini öğrenmek için çözümde bazı sorgular da çalıştırdınız.

Veri odaklı, uçtan uca bir senaryoyu tamamlamaya yönelik örnek komut satırı uygulamasını diğer Azure hizmetleriyle birlikte kullanmak için sonraki öğreticiye geçin:
> [!div class="nextstepaction"]
> [*Öğretici: uçtan uca bir çözümü bağlama*](tutorial-end-to-end.md)