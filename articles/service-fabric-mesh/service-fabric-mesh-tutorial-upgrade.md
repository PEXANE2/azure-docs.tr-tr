---
title: Öğretici-Azure Service Fabric kafes uygulamasını yükseltme
description: Bu öğretici bir serinin dördüncü bölümüdür ve bir Azure Service Fabric kafes uygulamasının doğrudan Visual Studio 'dan nasıl yükseltileceğini gösterir.
author: dkkapur
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: 7cdb8868f760ef0f35ab90c06b411110f871738c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75351711"
---
# <a name="tutorial-learn-how-to-upgrade-a-service-fabric-application-using-visual-studio"></a>Öğretici: Visual Studio kullanarak Service Fabric uygulamasını nasıl yükselteceğinizi öğrenin

Bu öğretici bir serinin dördüncü bölümüdür ve bir Azure Service Fabric kafes uygulamasının doğrudan Visual Studio 'dan nasıl yükseltileceğini gösterir. Yükseltme hem bir kod güncelleştirmesi hem de bir yapılandırma güncelleştirmesi içerir. Visual Studio içinden yükseltme ve yayımlama adımlarının aynı olduğunu görürsünüz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Visual Studio 'Yu kullanarak Service Fabric bir kafes hizmetini yükseltme

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

## <a name="upgrade-a-service-fabric-mesh-service-by-using-visual-studio"></a>Visual Studio 'Yu kullanarak Service Fabric bir kafes hizmetini yükseltme

Bu makalede bir uygulamanın içindeki bir mikro hizmetin nasıl yükseltileceğini gösterir. Bu örnekte, `WebFrontEnd` hizmeti bir görev kategorisi görüntüleyecek ve verilen CPU miktarını artıracak şekilde değiştireceksiniz. Sonra dağıtılan hizmeti yükseltiyoruz.

## <a name="modify-the-config"></a>Yapılandırmayı değiştirme

Service Fabric bir kafes uygulaması oluşturduğunuzda, Visual Studio her dağıtım ortamı (bulut ve yerel) için bir **Parameters. YAML** dosyası ekler. Bu dosyalarda, Service. YAML veya Network. YAML gibi ağ *. YAML dosyalarından başvurulabilen parametreleri ve değerlerini tanımlayabilirsiniz.  Visual Studio, hizmetin ne kadar CPU kullanabileceği gibi bazı değişkenler sağlar.

**Web ön uç** hizmetinin `WebFrontEnd_cpu` daha yoğun bir şekilde `1.5` kullanılması için CPU kaynaklarını olasılığına olarak güncelleştirmek üzere parametresini güncelleştireceğiz.

1. **Todolistapp** projesinde, **ortamlar** > **bulutu**altında **Parameters. YAML** dosyasını açın. `WebFrontEnd_cpu`Değerini olarak `1.5`değiştirin. Parametre adı, farklı hizmetlere uygulanan aynı ada sahip parametrelerden `WebFrontEnd_` ayırt edilebilmesi için en iyi uygulama olarak hizmet adı ile önceden başlatılacaktır.

    ```xml
    WebFrontEnd_cpu: 1.5
    ```

2. Webön **uç** > **hizmeti kaynakları**altında **webön uç** projesinin **Service. YAML** dosyasını açın.

    İçindeki `resources:` bölümünün `cpu:` olarak ayarlandığını unutmayın `"[parameters('WebFrontEnd_cpu')]"`. Proje bulut için derleniyorsa, için `'WebFrontEnd_cpu` değeri,**bulut** > **parametreleri. YAML** dosyası **ortamlarından** > alınır ve olur. `1.5` Proje yerel olarak çalışmak üzere derleniyorsa, bu değer **ortamlar** > **Yerel** > **Parametreler. YAML** dosyasından alınır ve ' 0,5 ' olur.

> [!Tip]
> Varsayılan olarak, profile. YAML dosyasının bir eşi olan parametre dosyası, bu profile. YAML dosyasının değerlerini sağlamak için kullanılacaktır.
> Örneğin, bulut > parametreleri > ortamlar. YAML, bulut > profili. YAML > ortamları için parametre değerleri sağlar.
>
> Bunu profile. YAML dosyasına aşağıdakileri ekleyerek geçersiz kılabilirsiniz:`parametersFilePath=”relative or full path to the parameters file”` örneğin veya `parametersFilePath=”C:\MeshParms\CustomParameters.yaml”``parametersFilePath=”..\CommonParameters.yaml”`

## <a name="modify-the-model"></a>Modeli değiştirme

Bir kod değişikliği tanıtmak için, `Category` `ToDoItem` `ToDoItem.cs` dosyadaki sınıfına bir özellik ekleyin.

```csharp
public class ToDoItem
{
    public string Category { get; set; }
    ...
}
```

Sonra, kategoriyi `Load()` varsayılan bir dizeye ayarlamak için aynı dosyada yöntemi güncelleştirin:

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

`WebFrontEnd` Proje, yapılacaklar listesi öğelerini gösteren bir Web sayfası olan ASP.NET Core bir uygulamadır. `WebFrontEnd` Projede, görevin kategorisini göstermek `Index.cshtml` için aşağıda gösterildiği gibi aşağıdaki iki satırı açın ve ekleyin:

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

Görevleri listeleyen Web sayfasında yeni bir kategori sütunu gördiğinizi doğrulamak için uygulamayı derleyin ve çalıştırın.

## <a name="upgrade-the-app-from-visual-studio"></a>Uygulamayı Visual Studio 'dan yükseltme

Bir kod yükseltmesi veya bir yapılandırma yükseltmesi yapıyor musunuz (Bu durumda her ikisini de yaptığımız), Visual Studio 'da **todolistapp** ' ye sağ tıklayıp **Yayımla...** öğesini seçerek Azure 'da Service Fabric kafes uygulamanızı yükseltin.

Ardından **Service Fabric Uygulamasını Yayımla** iletişim kutusunu göreceksiniz.

Bu dağıtım için kullanılacak profile. YAML dosyasını seçmek için **hedef profili** açılan listesini kullanın. Uygulamayı bulutta yükseltiyoruz, bu nedenle açılan menüde, bu dosyada tanımlanan 1,0 `WebFrontEnd_cpu` değerini kullanacak olan **Cloud. YAML** 'yi seçeceğiz.

![Visual Studio Service Fabric Mesh yayımla iletişim kutusu](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Azure hesabınızı ve aboneliğinizi seçin. **Konumu** , Ilk olarak Azure 'da yapılacak-do uygulamasını yayımladığınızda kullandığınız konuma ayarlayın. Bu makale **Doğu ABD**kullanıldı.

**Kaynak grubunu** , ilk olarak yapılacak BT uygulamasını Azure 'a yayımladığınızda kullandığınız kaynak grubuna ayarlayın.

Azure 'da yapılacak do uygulamasını ilk kez yayımladığınızda oluşturduğunuz Azure Container Registry adına **Azure Container Registry** ayarlayın.

Yayımla iletişim kutusunda, Azure 'da Yapılacaklar uygulamasını yükseltmek için **Yayımla** düğmesine basın.

Visual Studio **çıktı** penceresinde **Service Fabric araçları** bölmesini seçerek yükseltmenin ilerlemesini izleyin. 

Görüntü oluşturulup Azure Container Registry gönderildikten sonra, çıktıda Azure portal dağıtımı izlemek için tıklabileceğiniz çıktıda bir **durum** bağlantısı görüntülenir.

Yükseltme tamamlandıktan sonra, **Service Fabric araçları** çıktısı uygulamanızın IP adresini ve bağlantı noktasını bir URL biçiminde görüntüler.

```json
The application was deployed successfully and it can be accessed at http://10.000.38.000:20000.
```

Bir web tarayıcısı açıp URL'ye giderek Azure'da çalışan web sitenizi görebilirsiniz. Artık Kategori sütunu içeren bir Web sayfası görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde şunları öğrendiniz:
> [!div class="checklist"]
> * Service Fabric bir kafes uygulamasını Visual Studio kullanarak yükseltme

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Service Fabric Mesh kaynaklarını temizleme](service-fabric-mesh-tutorial-cleanup-resources.md)