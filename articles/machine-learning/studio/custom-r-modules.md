---
title: Özel R modüllerini tanımlayın
titleSuffix: ML Studio (classic) - Azure
description: Bu konu, özel bir R Studio'nun (klasik) nasıl yazılabildiğini ve dağıtılabildiğini açıklar. Özel R modüllerinin ne olduğunu ve bunları tanımlamak için hangi dosyaların kullanıldığını açıklar.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 11/29/2017
ms.openlocfilehash: 5b8dab14a9416795eccef1f71988a048c8bedb48
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218161"
---
# <a name="define-custom-r-modules-for-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio (klasik) için özel R modüllerini tanımlayın

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Bu konu, özel bir R Studio'nun (klasik) nasıl yazılabildiğini ve dağıtılabildiğini açıklar. Özel R modüllerinin ne olduğunu ve bunları tanımlamak için hangi dosyaların kullanıldığını açıklar. Bir modülü tanımlayan dosyaların nasıl oluşturedileceğini ve makine öğrenimi çalışma alanında dağıtım modülünün nasıl kaydedileceğini gösterir. Özel modülün tanımında kullanılan öğeler ve öznitelikler daha sonra daha ayrıntılı olarak açıklanır. Yardımcı işlevsellik ve dosyaların ve birden çok çıktının nasıl kullanılacağı da tartışılır. 



## <a name="what-is-a-custom-r-module"></a>Özel R modülü nedir?
**Özel modül,** çalışma alanınıza yüklenebilen ve Azure Machine Learning Studio (klasik) deneyinin bir parçası olarak yürütülebilen kullanıcı tanımlı bir modüldür. **Özel R modülü,** kullanıcı tanımlı Bir R işlevini yürüten özel bir modüldür. **R,** istatistikçiler ve veri bilimciler tarafından algoritmaları uygulamak için yaygın olarak kullanılan istatistiksel bilgi işlem ve grafikler için bir programlama dilidir. Şu anda, R özel modüllerde desteklenen tek dildir, ancak sonraki sürümler için ek diller için destek zamanlanır.

Özel modüller, azure machine learning studio'da (klasik) diğer modüller gibi kullanılabilme leri açısından **birinci sınıf bir konuma** sahiptir. Bunlar, yayınlanmış denemelerde veya görselleştirmelerde yer alan diğer modüllerle birlikte yürütülebilir. Modül tarafından uygulanan algoritma, kullanılacak giriş ve çıkış bağlantı noktaları, modelleme parametreleri ve diğer çeşitli çalışma zamanı davranışları üzerinde denetiminiz vardır. Özel modüller içeren bir deneme, kolay paylaşım için Azure AI Galerisi'nde de yayınlanabilir.

## <a name="files-in-a-custom-r-module"></a>Özel Bir R modülündeki dosyalar
Özel Bir R modülü, en az iki dosya içeren bir .zip dosyası ile tanımlanır:

* Modülün maruz kaçtığı R işlevini uygulayan bir **kaynak dosya**
* Özel modül arabirimini açıklayan bir **XML tanım dosyası**

Özel modülden erişilebilen işlevsellik sağlayan .zip dosyasına ek yardımcı dosyalar da eklenebilir. Bu seçenek, hızlı başlatma örneğini izleyen **XML tanım dosyasındaki** başvuru bölümü Öğeleri'nin **Bağımsız Değişkenler** bölümünde tartışılır.

## <a name="quickstart-example-define-package-and-register-a-custom-r-module"></a>Hızlı başlangıç örneği: özel bir R modül tanımlayın, paketlenin ve kaydettirin
Bu örnek, özel bir R modülü tarafından gerekli dosyaları oluşturmak için, bir zip dosyası içine paket ve sonra Makine Öğrenimi çalışma alanına modül kayıt nasıl göstermektedir. Örnek zip paketi ve örnek dosyaları [Download CustomAddRows.zip dosyasından](https://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409)indirilebilir.

## <a name="the-source-file"></a>Kaynak dosya
İki veri kümesinden (veri çerçeveleri) satırları (gözlemleri) birleştirmek için kullanılan **Satır Ekle** modülünün standart uygulamasını değiştiren **Özel Satır Ekle** modülü örneğini düşünün. Standart **Satır Ekle** modülü, `rbind` algoritmayı kullanarak ikinci giriş veri kümesinin satırlarını ilk giriş veri kümesinin sonuna ekler. Özelleştirilmiş `CustomAddRows` işlev benzer şekilde iki veri kümesini kabul eder, ancak ek bir giriş olarak Boolean takas parametresini de kabul eder. Takas parametresi **FALSE**olarak ayarlanmışsa, standart uygulamayla aynı veri kümesini döndürür. Ancak değiştirme parametresi **TRUE**ise, işlev yerine ikinci veri kümesinin sonuna ilk giriş veri kümesi satırlarını ekler. **Custom Add Rows** modülü tarafından maruz kalan `CustomAddRows` R işlevinin uygulanmasını içeren CustomAddRows.R dosyası aşağıdaki R koduna sahiptir.

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) 
    {
        if (swap)
        {
            return (rbind(dataset2, dataset1));
        }
        else
        {
            return (rbind(dataset1, dataset2));
        } 
    } 

### <a name="the-xml-definition-file"></a>XML tanım dosyası
Bu `CustomAddRows` işlevi Azure Machine Learning Studio (klasik) modülü olarak göstermek için, Özel **Satır Ekle** modülünün nasıl görünmesi ve nasıl görünmesi gerektiğini belirtmek için bir XML tanım dosyası oluşturulması gerekir. 

    <!-- Defined a module using an R Script -->
    <Module name="Custom Add Rows">
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset to another. Dataset 2 is concatenated to Dataset 1 when Swap is FALSE, and vice versa when Swap is TRUE.</Description>

    <!-- Specify the base language, script file and R function to use for this module. -->        
        <Language name="R" 
         sourceFile="CustomAddRows.R" 
         entryPoint="CustomAddRows" />  

    <!-- Define module input and output ports -->
    <!-- Note: The values of the id attributes in the Input and Arg elements must match the parameter names in the R Function CustomAddRows defined in CustomAddRows.R. -->
        <Ports>
            <Input id="dataset1" name="Dataset 1" type="DataTable">
                <Description>First input dataset</Description>
            </Input>
            <Input id="dataset2" name="Dataset 2" type="DataTable">
                <Description>Second input dataset</Description>
            </Input>
            <Output id="dataset" name="Dataset" type="DataTable">
                <Description>The combined dataset</Description>
            </Output>
        </Ports>

    <!-- Define module parameters -->
        <Arguments>
            <Arg id="swap" name="Swap" type="bool" >
                <Description>Swap input datasets.</Description>
            </Arg>
        </Arguments>
    </Module>


XML dosyasındaki **Giriş** ve **Arg** öğelerinin **kimlik** özniteliklerinin değerinin CustomAddRows.R dosyasındaki R kodunun işlev parametre adlarıyla tam olarak eşleşmesi gerektiğini belirtmek önemlidir: (*dataset1*, *dataset2*, ve örnekte *takas).* Benzer şekilde, **Dil** öğesinin **entryPoint** özniteliğinin değeri, R komut dosyasındaki işlevin adıyla tam olarak eşleşmelidir: ( Örnekteki*CustomAddRows).* 

Buna karşılık, **Çıktı** öğesi için **id** özniteliği R komut dosyasında herhangi bir değişkene karşılık gelmez. Birden fazla çıktı gerektiğinde, **XML** dosyasında Çıktı öğeleri nin beyan edildiği *sırada aynı sırada* yerleştirilen sonuçlarla Birlikte R işlevinden bir liste döndürmenız yeterlidir.

### <a name="package-and-register-the-module"></a>Modülü paketleyip kaydedin
*CustomAddRows.R* ve *CustomAddRows.xml* olarak bu iki dosyayı kaydedin ve sonra birlikte bir *CustomAddRows.zip* dosyası içine iki dosya zip.

Bunları Machine Learning çalışma alanınıza kaydetmek için Azure Machine Learning Studio'da (klasik) çalışma alanınıza gidin, alttaki **+Yenİ** düğmesini tıklayın ve yeni **Özel Satır Ekle** modüllerini yüklemek için ZIP **PAKETİnDEN MODÜL ->'yi** seçin.

![Zip Yükle](./media/custom-r-modules/upload-from-zip-package.png)

**Özel Satır Ekle** modülü artık Machine Learning deneyleriniz tarafından erişilmeye hazırdır.

## <a name="elements-in-the-xml-definition-file"></a>XML tanım dosyasındaki öğeler
### <a name="module-elements"></a>Modül elemanları
**Modül** öğesi XML dosyasında özel bir modül tanımlamak için kullanılır. Birden çok modül, birden çok **modül** öğesi kullanılarak bir XML dosyasında tanımlanabilir. Çalışma alanınızdaki her modülün benzersiz bir adı olmalıdır. Varolan özel bir modülle aynı ada sahip özel bir modül kaydettirin ve varolan modülü niçin yenisiyle değiştirir. Ancak özel modüller, mevcut bir Azure Machine Learning Studio (klasik) modülüyle aynı ada kaydedilebilir. Bu durumda, modül paletinin **Özel** kategorisinde görünürler.

    <Module name="Custom Add Rows" isDeterministic="false"> 
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset to another...</Description>/> 


**Modül** öğesi içinde, iki ek isteğe bağlı öğe belirtebilirsiniz:

* modülün içine gömülü bir **Sahip** öğesi  
* modül için hızlı yardım olarak görüntülenen ve Makine Öğrenimi Kullanıcı Arabirimi'nde modülün üzerinde gezinirken görüntülenen bir **Açıklama** öğesi.

Modül elemanlarındaki karakter sınırları için kurallar:

* **Modül** öğesindeki **ad** özniteliğinin değeri 64 karakteri geçmemelidir. 
* **Açıklama** öğesinin içeriği 128 karakteri geçmemelidir.
* **Sahibi** öğesinin içeriği uzunluğu 32 karakter geçmemelidir.

Bir modülün sonuçları deterministic veya nondeterministic olabilir.** Varsayılan olarak, tüm modüller deterministic olarak kabul edilir. Diğer bir deyişle, değişmeyen bir giriş parametreleri ve veri kümesi göz önüne alındığında, modül aynı sonuçları eacRAND veya çalıştırılandığı bir işlev süresini döndürmelidir. Bu davranış göz önüne alındığında, Azure Machine Learning Studio (klasik) yalnızca bir parametre veya giriş verileri değiştiyse deterministic olarak işaretlenmiş modülleri yeniden çalıştırın. Önbelleğe alınmış sonuçları döndürmek de denemelerin çok daha hızlı yürütülmesini sağlar.

RAND veya geçerli tarih veya saati döndüren bir işlev gibi belirsiz işlevler vardır. Modülünüzün nondeterministic bir işlev kullanıyorsa, isteğe bağlı **isDeterministic** özniteliği **FALSE**ayarlayarak modülün deterministik olmadığını belirtebilirsiniz. Bu, modül girişi ve parametreleri değişmemiş olsa bile, deneme çalıştırıldığında modülün yeniden çalıştırıldığından emin olur. 

### <a name="language-definition"></a>Dil Tanımı
XML tanım dosyanızdaki **Dil** öğesi, özel modül dilini belirtmek için kullanılır. Şu an için yalnızca R dili desteklenmektedir. **SourceFile** özniteliğinin değeri, modül çalıştırıldığında çağrılacak işlevi içeren R dosyasının adı olmalıdır. Bu dosya zip paketinin bir parçası olmalıdır. **entryPoint** özniteliğinin değeri çağrılan işlevin adıdır ve kaynak dosyada tanımlanan geçerli bir işlevle eşleşmelidir.

    <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />


### <a name="ports"></a>Bağlantı Noktaları
Özel bir modülün giriş ve çıkış bağlantı noktaları, XML tanım dosyasının **Bağlantı Noktaları** bölümünün alt öğelerinde belirtilir. Bu öğelerin sırası, kullanıcılar tarafından karşılaşılan düzeni (UX) belirler. XML dosyasının **Bağlantı Noktaları** öğesinde listelenen ilk alt **giriş** veya **çıktı,** Machine Learning UX'deki en sol giriş bağlantı noktası olur.
Her giriş ve çıkış bağlantı noktası, Fare imlecini Machine Learning UI'deki bağlantı noktasının üzerinde gezinirken gösterilen metni belirten isteğe bağlı bir **Açıklama** alt öğesine sahip olabilir.

**Liman Kuralları**:

* Her biri için maksimum **giriş ve çıkış bağlantı noktası** sayısı 8'dir.

### <a name="input-elements"></a>Giriş öğeleri
Giriş bağlantı noktaları, verileri R işlevinize ve çalışma alanınıza aktarmanızı sağlar. Giriş bağlantı noktaları için desteklenen **veri türleri** aşağıdaki gibidir: 

**DataTable:** Bu tür, veri çerçevesi olarak R işlevinize aktarılır. Aslında, Machine Learning tarafından desteklenen ve **DataTable** ile uyumlu olan tüm türler (örneğin, CSV dosyaları veya ARFF dosyaları) otomatik olarak data.frame'e dönüştürülür. 

        <Input id="dataset1" name="Input 1" type="DataTable" isOptional="false">
            <Description>Input Dataset 1</Description>
           </Input>

Her **DataTable** giriş bağlantı noktasıyla ilişkili **id** özniteliğinin benzersiz bir değeri olmalıdır ve bu değer, R işlevinizdeki karşılık gelen adlandırılmış parametreyle eşleşmelidir.
Bir denemede giriş olarak geçirilemeyen isteğe bağlı **DataTable** bağlantı noktaları, **NULL'un** R işlevine geçtiği değere sahiptir ve giriş bağlı değilse isteğe bağlı zip bağlantı noktaları yoksayılır. **isteğe bağlı** öznitelik hem **DataTable** ve **Zip** türleri için isteğe bağlıdır ve varsayılan olarak *yanlıştır.*

**Zip:** Özel modüller bir zip dosyayı giriş olarak kabul edebilir. Bu giriş, işlevinizin R çalışma dizinine çözülr

        <Input id="zippedData" name="Zip Input" type="Zip" IsOptional="false">
            <Description>Zip files to be extracted to the R working directory.</Description>
           </Input>

Özel R modülleri için Zip bağlantı noktası nın kimliğinin R fonksiyonunun herhangi bir parametresini eşleştirmesi gerekmez. Bunun nedeni, zip dosyasının otomatik olarak R çalışma dizinine ayıklanmasıdır.

**Giriş Kuralları:**

* **Giriş** öğesinin **id** özniteliğinin değeri geçerli bir R değişken adı olmalıdır.
* **Giriş** öğesinin **id** özniteliğinin değeri 64 karakterden uzun olmamalıdır.
* **Giriş** öğesinin **ad** özniteliğinin değeri 64 karakterden uzun olmamalıdır.
* **Açıklama** öğesinin içeriği 128 karakterden uzun olmamalıdır
* **Giriş** öğesinin **tür** özniteliğinin değeri *Zip* veya *DataTable*olmalıdır.
* **Giriş** öğesinin **isteğe bağlı** özniteliğinin değeri gerekli değildir (ve belirtilmediğinde varsayılan olarak *yanlıştır);* ama belirtilirse, *doğru* veya *yanlış*olmalıdır.

### <a name="output-elements"></a>Çıkış elemanları
**Standart çıkış bağlantı noktaları:** Çıkış bağlantı noktaları, r işlevinizden gelen dönüş değerlerine eşlenir ve sonraki modüller tarafından kullanılabilir. *DataTable,* şu anda desteklenen tek standart çıkış bağlantı noktası türüdür. *(Öğrenenler* ve *Dönüşümler* için destek geliyor.) *DataTable* çıktısı şu şekilde tanımlanır:

    <Output id="dataset" name="Dataset" type="DataTable">
        <Description>Combined dataset</Description>
    </Output>

Özel R modüllerinde çıktılar **için, id** özniteliğinin değeri R komut dosyasındaki herhangi bir şeyle karşılık gelmek zorunda değildir, ancak benzersiz olmalıdır. Tek bir modül çıkışı için, R işlevinden gelen geri dönüş değeri bir *data.frame*olmalıdır. Desteklenen veri türünün birden fazla nesnesini çıktırabilmek için, XML tanım dosyasında uygun çıktı bağlantı noktalarının belirtilmesi ve nesnelerin liste olarak döndürülmesi gerekir. Çıktı nesneleri, nesnelerin döndürülen ler listesine yerleştirilme sırasını yansıtan, soldan sağa çıkış bağlantı noktalarına atanır.

Örneğin, yeni birleştirilmiş veri kümesine ek olarak, *dataset1* ve dataset2 olmak üzere, orijinal iki veri kümesini, *dataset1*ve *dataset2'yi*çıktıolarak **Özel Satır Ekle** modüllerini değiştirmek istiyorsanız, (bir sırada, soldan sağa, aşağıdaki gibi: *dataset*, *dataset1*, *dataset2*), sonra CustomAddRows.xml dosyasındaki çıkış bağlantı noktalarını aşağıdaki gibi tanımlayın:

    <Ports> 
        <Output id="dataset" name="Dataset Out" type="DataTable"> 
            <Description>New Dataset</Description> 
        </Output> 
        <Output id="dataset1_out" name="Dataset 1 Out" type="DataTable"> 
            <Description>First Dataset</Description> 
        </Output> 
        <Output id="dataset2_out" name="Dataset 2 Out" type="DataTable"> 
            <Description>Second Dataset</Description> 
        </Output> 
        <Input id="dataset1" name="Dataset 1" type="DataTable"> 
            <Description>First Input Table</Description>
        </Input> 
        <Input id="dataset2" name="Dataset 2" type="DataTable"> 
            <Description>Second Input Table</Description> 
        </Input> 
    </Ports> 


Ve 'CustomAddRows.R' doğru sırada bir listedeki nesnelerin listesini döndürün:

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) { 
        if (swap) { dataset <- rbind(dataset2, dataset1)) } 
        else { dataset <- rbind(dataset1, dataset2)) 
        } 
    return (list(dataset, dataset1, dataset2)) 
    } 

**Görselleştirme çıktısı:** Ayrıca, R grafik aygıtından ve konsol çıkışından çıktıyı görüntüleyen *tür Görselleştirme'nin*çıkış bağlantı noktasını da belirtebilirsiniz. Bu bağlantı noktası R işlev çıkışının bir parçası değildir ve diğer çıkış bağlantı noktası türlerinin sırasını engellemez. Özel modüllere görselleştirme bağlantı noktası eklemek için, **tür** özniteliği için *Görselleştirme* değerine sahip bir **Çıktı** öğesi ekleyin:

    <Output id="deviceOutput" name="View Port" type="Visualization">
      <Description>View the R console graphics device output.</Description>
    </Output>

**Çıktı Kuralları:**

* **Çıktı** öğesinin **id** özniteliğinin değeri geçerli bir R değişken adı olmalıdır.
* **Çıktı** öğesinin **id** özniteliğinin değeri 32 karakterden uzun olmamalıdır.
* **Çıktı** öğesinin **ad** özniteliğinin değeri 64 karakterden uzun olmamalıdır.
* **Çıktı** öğesinin **tür** özniteliğinin değeri *Görselleştirme*olmalıdır.

### <a name="arguments"></a>Bağımsız Değişkenler
**Bağımsız değişkenler** öğesinde tanımlanan modül parametreleri aracılığıyla Ek veriler R işlevine aktarılabilir. Bu parametreler, modül seçildiğinde Machine Learning UI'nin en sağdaki özellikleri bölmesinde görünür. Bağımsız değişkenler desteklenen türlerden herhangi biri olabilir veya gerektiğinde özel bir numaralandırma oluşturabilirsiniz. **Bağımsız Değişkenler** öğelerine benzer şekilde, **Bağımsız Değişkenler'de** de farenin parametre adının üzerinde gezinirken görünen metni belirten isteğe bağlı bir **Açıklama** öğesi olabilir.
Varsayılan Değer, minValue ve maxValue gibi bir modülün isteğe bağlı özellikleri, **özellikler** öğesine öznitelikleri olarak herhangi bir bağımsız değişkene eklenebilir. **Özellikler** öğesi için geçerli özellikleri bağımsız değişken türüne bağlıdır ve sonraki bölümde desteklenen bağımsız değişken türleri ile açıklanır. **"True"** olarak ayarlanmış **isteğe bağlı** özellik içeren bağımsız değişkenler, kullanıcının bir değer girmesini gerektirmez. Bağımsız değişkene bir değer sağlanmadıysa, bağımsız değişken giriş noktası işlevine geçirilmeyecek. İsteğe bağlı giriş noktası işlevinin bağımsız değişkenlerinin işlev tarafından açıkça ele alınması gerekir, örneğin giriş noktası işlevi tanımında NULL varsayılan değeri atanmıştır. İsteğe bağlı bir bağımsız değişken, kullanıcı tarafından bir değer sağlanıyorsa, yalnızca diğer bağımsız değişken kısıtlamalarını, yani min veya max'i zorlar.
Girdi ve çıktılarda olduğu gibi, parametrelerin her birinin kendileriyle ilişkili benzersiz kimlik değerlerine sahip olması önemlidir. Hızlı başlangıç örneğimizde ilişkili id/parametre *takas*oldu.

### <a name="arg-element"></a>Arg öğesi
Bir modül parametresi XML tanım dosyasının **Bağımsız Değişkenler** bölümünün **Arg** alt öğesi kullanılarak tanımlanır. **Bağımsız Değişkenler** bölümündeki alt öğelerde olduğu gibi, **Bağımsızlar** bölümündeki parametrelerin sıralanması UX'de karşılaşılan düzeni tanımlar. Parametreler, UI'de yukarıdan aşağıya, XML dosyasında tanımlandığı sırada görünür. Parametreler için Machine Learning tarafından desteklenen türler burada listelenmiştir. 

**int** – bir İnteger (32-bit) türü parametresi.

    <Arg id="intValue1" name="Int Param" type="int">
        <Properties min="0" max="100" default="0" />
        <Description>Integer Parameter</Description>
    </Arg>


* *İsteğe Bağlı Özellikler*: **min**, **max**, **varsayılan** ve **isteğe bağlı**

**çift** - çift tip parametre.

    <Arg id="doubleValue1" name="Double Param" type="double">
        <Properties min="0.000" max="0.999" default="0.3" />
        <Description>Double Parameter</Description>
    </Arg>


* *İsteğe Bağlı Özellikler*: **min**, **max**, **varsayılan** ve **isteğe bağlı**

**bool** - UX bir onay kutusu ile temsil edilen bir Boolean parametresi.

    <Arg id="boolValue1" name="Boolean Param" type="bool">
        <Properties default="true" />
        <Description>Boolean Parameter</Description>
    </Arg>



* *İsteğe Bağlı Özellikler*: **varsayılan** - ayarlı değilse false

**string**: standart dize

    <Arg id="stringValue1" name="My string Param" type="string">
        <Properties isOptional="true" />
        <Description>String Parameter 1</Description>
    </Arg>    

* *İsteğe Bağlı Özellikler*: **varsayılan** ve **isteğe bağlı**

**ColumnPicker**: sütun seçim parametresi. Bu tür UX'de sütun seçici olarak işlenir. **Özellik** öğesi burada, sütunların seçildiği, hedef bağlantı noktası türünün *DataTable*olması gereken bağlantı noktasının kimliğini belirtmek için kullanılır. Sütun seçiminin sonucu, seçilen sütun adlarını içeren dizeleri listesi olarak R işlevine geçirilir. 

        <Arg id="colset" name="Column set" type="ColumnPicker">      
          <Properties portId="datasetIn1" allowedTypes="Numeric" default="NumericAll"/>
          <Description>Column set</Description>
        </Arg>


* *Gerekli Özellikler*: **portId** - veri *tablosu*türü ile bir Giriş öğesinin kimliği eşleşir.
* *İsteğe Bağlı Özellikler*:
  
  * **allowedTypes** - Seçebileceğiniz sütun türlerini filtreler. Geçerli değerler şunlardır: 
    
    * Sayısal
    * Boole
    * Kategorik
    * Dize
    * Etiketle
    * Özellik
    * Puan
    * Tümü
  * **varsayılan** - Sütun seçici için geçerli varsayılan seçimler şunlardır: 
    
    * None
    * Sayısal Özellik
    * Sayısal Etiket
    * Sayısal Puan
    * SayısalTüm
    * BooleanÖzelliği
    * BooleanEtiket
    * BooleanPuan
    * BooleanTümü
    * Kategorik Özellik
    * CategoricalLabel
    * Kategoripuanı
    * CategoricalTümü
    * StringFeature
    * StringLabel
    * StringScore
    * StringAll
    * AllLabel
    * AllFeature
    * AllScore
    * Tümü

**DropDown**: kullanıcı tarafından numaralandırılmış (açılır) listesi. Açılan öğeler, **Madde** öğesi kullanılarak **Özellikler** öğesi içinde belirtilir. Her **Öğenin** **kimliği** benzersiz ve geçerli bir R değişkeni olmalıdır. Bir **Öğenin** **adının** değeri hem gördüğünüz metin hem de R işlevine geçirilen değer olarak hizmet verir.

    <Arg id="color" name="Color" type="DropDown">
      <Properties default="red">
        <Item id="red" name="Red Value"/>
        <Item id="green" name="Green Value"/>
        <Item id="blue" name="Blue Value"/>
      </Properties>
      <Description>Select a color.</Description>
    </Arg>    

* *İsteğe Bağlı Özellikler*:
  * **varsayılan** - Varsayılan özelliğin **değeri, Madde** öğelerinden birinin kimlik değeriyle karşılık vermelidir.

### <a name="auxiliary-files"></a>Yardımcı Dosyalar
Özel modül ZIP dosyanıza yerleştirilen herhangi bir dosya yürütme süresi boyunca kullanılabilir olacaktır. Mevcut dizin yapıları korunur. Bu, dosya kaynağının yerel olarak ve Azure Machine Learning Studio (klasik) yürütmede aynı şekilde çalıştığı anlamına gelir. 

> [!NOTE]
> Tüm dosyaların 'src' dizinine ayıklanmasına dikkat edin, böylece tüm yollar 'src/' öneki olmalıdır.
> 
> 

Örneğin, veri kümesinden NAS içeren satırları kaldırmak istediğinizi ve ayrıca yinelenen satırları CustomAddRows'a çıkarmadan önce kaldırmak istediğinizi ve bunu bir dosyada RemoveDupNARows.R'de bunu yapan bir R işlevi zaten yazdığınızu varsa:

    RemoveDupNARows <- function(dataFrame) {
        #Remove Duplicate Rows:
        dataFrame <- unique(dataFrame)
        #Remove Rows with NAs:
        finalDataFrame <- dataFrame[complete.cases(dataFrame),]
        return(finalDataFrame)
    }
Size CustomAddRows işlevinde yardımcı dosya RemoveDupNARows.R kaynak olabilir:

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) {
        source("src/RemoveDupNARows.R")
            if (swap) { 
                dataset <- rbind(dataset2, dataset1))
             } else { 
                  dataset <- rbind(dataset1, dataset2)) 
             } 
        dataset <- removeDupNARows(dataset)
        return (dataset)
    }

Ardından, özel bir R modülü olarak 'CustomAddRows.R', 'CustomAddRows.xml' ve 'RemoveDupNARows.R' içeren bir zip dosyası yükleyin.

## <a name="execution-environment"></a>Yürütme Ortamı
R komut dosyasının yürütme ortamı, R **Komut Dosyası** modülüyle R'nin aynı sürümünü kullanır ve aynı varsayılan paketleri kullanabilir. Ayrıca özel modül zip paketine ekleyerek özel modülünüze ek R paketleri ekleyebilirsiniz. R komut dosyanıza kendi R ortamınızda olduğu gibi yükleyin. 

**Yürütme ortamının sınırlamaları** şunlardır:

* Kalıcı olmayan dosya sistemi: Özel modül çalıştırıldığında yazılan dosyalar aynı modülün birden fazla çalıştırılmasında kalıcı değildir.
* Ağ erişimi yok

