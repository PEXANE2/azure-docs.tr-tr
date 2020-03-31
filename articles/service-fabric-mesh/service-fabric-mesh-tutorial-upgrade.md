---
title: Öğretici- Azure Hizmeti Kumaş Örgü uygulamasını yükseltme
description: Bu öğretici, bir serinin dördüncü bölümüdür ve azure hizmeti kumaş örgü uygulamasını doğrudan Visual Studio'dan nasıl yükseltersiniz gösterir.
author: dkkapur
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: 7cdb8868f760ef0f35ab90c06b411110f871738c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75351711"
---
# <a name="tutorial-learn-how-to-upgrade-a-service-fabric-application-using-visual-studio"></a>Öğretici: Visual Studio kullanarak bir Hizmet Kumaş ı uygulamasını nasıl yükselteceklerini öğrenin

Bu öğretici, bir serinin dördüncü bölümüdür ve azure hizmeti kumaş örgü uygulamasını doğrudan Visual Studio'dan nasıl yükseltersiniz gösterir. Yükseltme, hem kod güncelleştirmesi hem de config güncelleştirmesi içerir. Visual Studio içinden yükseltme ve yayımlama adımlarının aynı olduğunu görürsünüz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Visual Studio kullanarak Bir Hizmet Kumaş Örgü hizmeti yükseltme

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Visual Studio’da Service Fabric Mesh uygulaması oluşturma](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Yerel geliştirme kümenizde çalışan bir Service Fabric Mesh uygulamasının hatalarını ayıklama](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * [Service Fabric Mesh uygulaması dağıtma](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)
> * Service Fabric Mesh uygulamasını yükseltme
> * [Service Fabric Mesh kaynaklarını temizleme](service-fabric-mesh-tutorial-cleanup-resources.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* To-Do uygulamasını dağıtmadıysanız, [Service Fabric Mesh web uygulamasını yayımlama](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md) başlığı altında verilen yönergeleri izleyin.

## <a name="upgrade-a-service-fabric-mesh-service-by-using-visual-studio"></a>Visual Studio kullanarak Bir Hizmet Kumaş Örgü hizmeti yükseltme

Bu makalede, bir uygulama içinde bir microservice yükseltmek için nasıl gösterir. Bu örnekte, `WebFrontEnd` bir görev kategorisi görüntülemek ve verilen CPU miktarını artırmak için hizmeti değiştireceğiz. Sonra dağıtılan hizmeti yükselteceğiz.

## <a name="modify-the-config"></a>Config değiştirin

Bir Service Fabric Mesh uygulaması oluşturduğunuzda, Visual studio her dağıtım ortamı (bulut ve yerel) için bir **parameters.yaml** dosyası ekler. Bu dosyalarda, daha sonra service.yaml veya network.yaml gibi Mesh *.yaml dosyalarınızdan başvurulabilen parametreleri ve değerlerini tanımlayabilirsiniz.  Visual Studio, hizmetin ne kadar CPU kullanabileceği gibi bazı değişkenler sağlar.

**WebFrontEnd** hizmetinin `WebFrontEnd_cpu` daha yoğun kullanılacağı `1.5` beklentisiyle işlemci kaynaklarını güncelleştirmek için parametreyi güncelleştireceğiz.

1. **Todolistapp** projesinde, **Ortam** > **bulutu**altında, **parametreleri açın.yaml** dosyası. `WebFrontEnd_cpu`, değerini ' `1.5`' ye değiştirin. Parametre adı, farklı hizmetler için `WebFrontEnd_` geçerli olan aynı adıtaşıyan parametrelerden ayırt etmek için en iyi yöntem olarak hizmet adı ile önceden yüzlenir.

    ```xml
    WebFrontEnd_cpu: 1.5
    ```

2. **WebFrontEnd** projesinin **service.yaml** dosyasını **WebFrontEnd** > **Hizmet Kaynakları**altında açın.

    Bölüm deki, `resources:` `cpu:` `"[parameters('WebFrontEnd_cpu')]"`ayarlanır unutmayın. Proje bulut için oluşturuluyorsa, değer `'WebFrontEnd_cpu` **Ortambulut** > **Cloud** > **parametreleri.yaml** dosyasından alınır ve `1.5`. Proje yerel olarak çalışacak şekilde oluşturuluyorsa, değer **Ortamlar** > **Yerel** > **parametreleri.yaml** dosyasından alınır ve '0,5' olur.

> [!Tip]
> Varsayılan olarak, profilin eş olduğu parametre dosyası.yaml dosyası bu profil.yaml dosyasının değerlerini sağlamak için kullanılır.
> Örneğin, Bulut > > parametreleri >.yaml, Bulut > profili.yaml > Ortamlar için parametre değerlerini sağlar.
>
> Profile.yaml dosyasına aşağıdakileri ekleyerek bunu geçersiz`parametersFilePath=”relative or full path to the parameters file”` kılabilirsiniz: `parametersFilePath=”C:\MeshParms\CustomParameters.yaml”` Örneğin,`parametersFilePath=”..\CommonParameters.yaml”`

## <a name="modify-the-model"></a>Modeli değiştirme

Kod değişikliği tanıtmak için `Category` `ToDoItem.cs` dosyadaki `ToDoItem` sınıfa bir özellik ekleyin.

```csharp
public class ToDoItem
{
    public string Category { get; set; }
    ...
}
```

Ardından, `Load()` kategoriyi varsayılan dize olarak ayarlamak için yöntemi aynı dosyada güncelleştirin:

```csharp
public static ToDoItem Load(string description, int index, bool completed)
{
    ToDoItem newItem = new ToDoItem(description)
    {
        Index = index,
        Completed = completed, 
        Category = "none"    // <-- add this line
    };

    return newItem;
}
```

## <a name="modify-the-service"></a>Hizmeti değiştirme

Proje, `WebFrontEnd` yapılacaklar listesi öğelerini gösteren bir web sayfası olan bir ASP.NET Core uygulamasıdır. `WebFrontEnd` Projede, görevin kategorisini görüntülemek için aşağıdaki iki satırı açın `Index.cshtml` ve ekleyin:

```HTML
<div>
    <table class="table-bordered">
        <thead>
            <tr>
                <th>Description</th>
                <th>Category</th>           @*add this line*@
                <th>Done?</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var item in Model.Items)
            {
                <tr>
                    <td>@item.Description</td>
                    <td>@item.Category</td>           @*add this line*@
                    <td>@item.Completed</td>
                </tr>
            }
        </tbody>
    </table>
</div>
```

Web sayfasında görevleri listeleyen yeni bir kategori sütunu gördüğünüzden emin olmak için uygulamayı oluşturun ve çalıştırın.

## <a name="upgrade-the-app-from-visual-studio"></a>Uygulamayı Visual Studio'dan yükseltme

İster kod yükseltmesi yapın, ister config yükseltmesi (bu durumda her ikisini de yapıyoruz), Visual Studio'da **todolistapp'a** sağ tıklayarak Azure'daki Service Fabric Mesh uygulamanızı yükseltin ve ardından **Yayımla'yı seçin...**

Ardından **Service Fabric Uygulamasını Yayımla** iletişim kutusunu göreceksiniz.

Bu dağıtım için kullanılacak profile.yaml dosyasını seçmek için **Hedef profil** açılır resmini kullanın. Uygulamayı bulutta yükseltiyoruz, bu yüzden açılır dosyadaki **cloud.yaml'yi** seçiyoruz `WebFrontEnd_cpu` ve bu dosyada tanımlanan 1.0 değerini kullanıyor.

![Visual Studio Service Fabric Mesh yayımla iletişim kutusu](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Azure hesabınızı ve aboneliğinizi seçin. **Yapılacaklar** uygulamasını Azure'da ilk kez yayınladığınızda Konum'u kullandığınız konuma ayarlayın. Bu makalede **Doğu ABD**kullanılır.

**Kaynak grubunu,** yapılacaklar uygulamasını Azure'da ilk kez yayınladığınızda kullandığınız kaynak grubuna ayarlayın.

**Azure Kapsayıcı Kayıt Defteri'ni,** yapılacaklar uygulamasını Azure'da ilk kez yayınladığınızda oluşturduğunuz azure kapsayıcı kayıt defteri adı olarak ayarlayın.

Yayımlama iletişim kutusunda, Azure'da yapılacaklar uygulamasını yükseltmek için **Yayımla** düğmesine basın.

Visual Studio **Output** penceresinde Service **Fabric Tools** bölmesini seçerek yükseltmenin ilerlemesini izleyin. 

Resim oluşturulup Azure Kapsayıcı Kayıt Defteri'ne itildikten sonra, Azure portalındaki dağıtımı izlemek için tıklatabileceğiniz çıktıda **Bir Durum İçin** bağlantısı görüntülenir.

Yükseltme tamamlandıktan sonra, **Service Fabric Tools** çıktısı UYGULAMANIZIN IP adresini ve bağlantı noktasını bir URL şeklinde görüntüler.

```json
The application was deployed successfully and it can be accessed at http://10.000.38.000:20000.
```

Bir web tarayıcısı açıp URL'ye giderek Azure'da çalışan web sitenizi görebilirsiniz. Şimdi bir kategori sütunu içeren bir web sayfası görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde şunları öğrendiniz:
> [!div class="checklist"]
> * Visual Studio kullanarak Bir Service Fabric Mesh uygulaması yükseltme

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Service Fabric Mesh kaynaklarını temizleme](service-fabric-mesh-tutorial-cleanup-resources.md)