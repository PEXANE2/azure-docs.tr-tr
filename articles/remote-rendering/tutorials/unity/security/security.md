---
title: Azure Remote Rendering ve model depolamanın güvenliğini sağlama
description: İçeriği güvenli hale getirmek için uzaktan Işleme uygulamasını sağlamlaştırma
author: florianborn71
ms.author: flborn
ms.date: 06/15/2020
ms.topic: tutorial
ms.custom: devx-track-csharp
ms.openlocfilehash: 403a5b68e3320700e275c744210f480be2c88e84
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89021332"
---
# <a name="tutorial-securing-azure-remote-rendering-and-model-storage"></a>Öğretici: Azure uzaktan Işleme ve model depolamanın güvenliğini sağlama

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Azure uzaktan Işleme modellerini içeren güvenli Azure Blob depolama
> * Azure uzaktan Işleme örneğinize erişmek için Azure AD ile kimlik doğrulama
> * Azure uzaktan Işleme kimlik doğrulaması için Azure kimlik bilgilerini kullanma

## <a name="prerequisites"></a>Önkoşullar

* Bu öğretici [öğretici: malzemeleri, aydınlatmayı ve etkileri Iyileştirirken](..\materials-lighting-effects\materials-lighting-effects.md)Bu öğreticide oluşturulur.

## <a name="why-additional-security-is-needed"></a>Neden ek güvenlik gerekir?

Uygulamanın geçerli durumu ve Azure kaynaklarınıza erişimi şöyle görünür:

![İlk güvenlik](./media/security-one.png)

Hem "AccountID + AccountKey" hem de "URL + SAS token", temelde bir Kullanıcı adı ve parolayı birlikte depolarsınız. Örneğin, "AccountID + AccountKey" gösterilirdi, bu, bir saldırganın sizin de kendi harcamanıza izniniz olmadan ARR kaynaklarınızı kullanması önemsiz bir durum olabilir.

## <a name="securing-your-content-in-azure-blob-storage"></a>Azure Blob depolamada içeriğinizi güvenli hale getirme

Azure uzaktan Işleme, Azure Blob depolama verilerinizin içeriğine doğru yapılandırmayla güvenli bir şekilde erişebilir. Azure uzaktan Işleme örneğinizi BLOB depolama hesaplarınızla yapılandırmak için bkz. [nasıl yapılır: depolama hesaplarını bağlama](../../../how-tos/create-an-account.md#link-storage-accounts) .

Bağlı bir BLOB depolama kullanılırken, modelleri yüklemek için biraz farklı yöntemler kullanacaksınız:

```csharp
var loadModelParams = new LoadModelFromSASParams(modelPath, modelEntity);
var loadModelAsync = ARRSessionService.CurrentActiveSession.Actions.LoadModelFromSASAsync(loadModelParams);
```

Yukarıdaki satırlar, `FromSAS` params ve Session eyleminin sürümünü kullanır. Bunlar SAS olmayan sürümlere dönüştürülmelidir:

```csharp
var loadModelParams = new LoadModelParams(storageAccountPath, blobContainerName, modelPath, modelEntity);
var loadModelAsync = ARRSessionService.CurrentActiveSession.Actions.LoadModelAsync(loadModelParams);
```

Şimdi, bir bağlı BLOB depolama hesabından bir özel model yüklemek için **Remoterenderingcoordinator** 'ı değiştirelim.

1. Henüz yapmadıysanız, ARR örneğiniz için BLOB depolama örneğinizi erişim iznini vermek üzere, [nasıl yapılır: bağlantı depolama hesaplarını](../../../how-tos/create-an-account.md#link-storage-accounts) doldurun.
1. Aşağıdaki değiştirilmiş **LoadModel** yöntemini geçerli **LoadModel** yönteminin hemen altındaki **remoterenderingcoordinator** öğesine ekleyin:

    ```csharp
    /// <summary>
    /// Loads a model from blob storage that has been linked to the ARR instance
    /// </summary>
    /// <param name="storageAccountName">The storage account name, this contains the blob containers </param>
    /// <param name="blobContainerName">The blob container name, i.e. arroutput</param>
    /// <param name="modelPath">The relative path inside the container to the model, i.e. test/MyCustomModel.arrAsset</param>
    /// <param name="parent">The parent Transform for this remote entity</param>
    /// <param name="progress">A call back method that accepts a float progress value [0->1]</param>
    /// <returns></returns>
    public async Task<Entity> LoadModel(string storageAccountName, string blobContainerName, string modelPath, Transform parent = null, ProgressHandler progress = null)
    {
        //Create a root object to parent a loaded model to
        var modelEntity = ARRSessionService.CurrentActiveSession.Actions.CreateEntity();

        //Get the game object representation of this entity
        var modelGameObject = modelEntity.GetOrCreateGameObject(UnityCreationMode.DoNotCreateUnityComponents);

        //Ensure the entity will sync its transform with the server
        var sync = modelGameObject.GetComponent<RemoteEntitySyncObject>();
        sync.SyncEveryFrame = true;

        //Parent the new object under the defined parent
        if (parent != null)
        {
            modelGameObject.transform.SetParent(parent, false);
            modelGameObject.name = parent.name + "_Entity";
        }

    #if UNITY_WSA
        //Anchor the model in the world, prefer anchoring parent if there is one
        if (parent != null)
        {
            parent.gameObject.AddComponent<WorldAnchor>();
        }
        else
        {
            modelGameObject.AddComponent<WorldAnchor>();
        }
    #endif

        //Load a model that will be parented to the entity
        var loadModelParams = new LoadModelParams($"{storageAccountName}.blob.core.windows.net", blobContainerName, modelPath, modelEntity);
        var loadModelAsync = ARRSessionService.CurrentActiveSession.Actions.LoadModelAsync(loadModelParams);
        if (progress != null)
            loadModelAsync.ProgressUpdated += progress;
        var result = await loadModelAsync.AsTask();
        return modelEntity;
    }
    ```

    Çoğu bölümde, bu kod özgün `LoadModel` yöntemle aynıdır, ancak yöntem ÇAĞRıLARıNıN SAS sürümünü SAS olmayan sürümlerle değiştirdik.

    Ek girişler `storageAccountName` ve `blobContainerName` bağımsız değişkenlere de eklenmiştir. Bu yeni **LoadModel** yöntemini, ilk öğreticide oluşturduğumuz çok Ilk **loadtestmodel** yöntemine benzer bir yöntemle çağıracağız.

1. Yalnızca **Loadtestmodel** sonrasında **Remoterenderingcoordinator** öğesine aşağıdaki yöntemi ekleyin

    ```csharp
    private bool loadingLinkedCustomModel = false;

    [SerializeField]
    private string storageAccountName;
    public string StorageAccountName {
        get => storageAccountName.Trim();
        set => storageAccountName = value;
    }

    [SerializeField]
    private string blobContainerName;
    public string BlobContainerName {
        get => blobContainerName.Trim();
        set => blobContainerName = value;
    }

    [SerializeField]
    private string modelPath;
    public string ModelPath {
        get => modelPath.Trim();
        set => modelPath = value;
    }

    [ContextMenu("Load Linked Custom Model")]
    public async void LoadLinkedCustomModel()
    {
        if (CurrentCoordinatorState != RemoteRenderingState.RuntimeConnected)
        {
            Debug.LogError("Please wait for the runtime to connect before loading the test model. Try again later.");
            return;
        }
        if (loadingLinkedCustomModel)
        {
            Debug.Log("Linked Test model already loading or loaded!");
            return;
        }
        loadingLinkedCustomModel = true;

        // Create a parent object to use for positioning
        GameObject testParent = new GameObject("LinkedCustomModel");
        testParent.transform.position = new Vector3(0f, 0f, 3f);

        await LoadModel(StorageAccountName, BlobContainerName, ModelPath, testParent.transform, (progressValue) => Debug.Log($"Loading Test Model progress: {Math.Round(progressValue * 100, 2)}%"));
    }
    ```

    Bu kod, **Remoterenderingcoordinator** bileşenine üç ek dize değişkeni ekler.
    ![Bağlantılı model](./media/storage-account-linked-model.png)

1. Değerlerinizi **Remoterenderingcoordinator** bileşenine ekleyin. [Model dönüştürme Için hızlı başlangıç](../../../quickstarts/convert-model.md)sonrasında değerlerinizin şu olması gerekir:

    * Depolama hesabı **adı**: depolama hesabınız için seçtiğiniz genel benzersiz ad depolama hesabı adıdır. Bu hızlı başlangıçta *arrtutorialstorage*idi, sizin değer farklı olacaktır.
    * **BLOB kapsayıcısı adı**: arroutput, BLOB depolama kapsayıcısı
    * **Model yolu**: dosyada *arrconfig.js* tanımlanan "Outputfolderpath" ve "outputAssetFileName" birleşimi. Hızlı başlangıçta "outputFolderPath" idi: "dönüştürülmüş/robot", "outputAssetFileName": "robot. arrAsset". Bu, "dönüştürülmüş/robot/robot. arrAsset" model yolu değeriyle sonuçlanır, ancak değer farklı olacaktır.

    >[!TIP]
    > "-UseContainerSas" bağımsız değişkeni olmadan [ **Conversion.ps1** betiğini çalıştırırsanız](../../../quickstarts/convert-model.md#run-the-conversion) , komut dosyası, SAS belirteci yerine sizin için yukarıdaki tüm değerleri çıktı olarak çıkar. ![Bağlantılı model](./media/converted-output.png)
1. Bu süre boyunca, özel modelinizin yüklenmesi için yer açmak amacıyla GameObject **Testmodel**' i kaldırın veya devre dışı bırakın.
1. Sahneyi yürütün ve uzak bir oturuma bağlanın.
1. **Remoterenderingcoordinator** ' a sağ tıklayıp **bağlı özel model yükle**' yi seçin.
    ![Bağlı modeli yükle](./media/load-linked-model.png)

Bu adımlar, yerel uygulamadan SAS belirtecini kaldırarak uygulamanın güvenliğini artırmıştır.

Şimdi uygulamanın şu andaki durumu ve Azure kaynaklarınıza erişimi şöyle görünür:

![Daha iyi güvenlik](./media/security-two.png)

Yerel uygulamadan kaldırmak için, daha önce bir "parola", AccountKey. Bu, Azure Active Directory (AAD) kimlik doğrulaması kullanılarak yapılabilir.

## <a name="azure-active-directory-azure-ad-authentication"></a>Azure Active Directory (Azure AD) kimlik doğrulaması

AAD kimlik doğrulaması, ARR 'yi daha denetimli bir şekilde hangi bireylerin veya grupların kullandığını belirlemenizi sağlar. ARR, hesap anahtarı kullanmak yerine [erişim belirteçlerini](https://docs.microsoft.com/azure/active-directory/develop/access-tokens) kabul etmek için yerleşik desteğe sahiptir. Erişim belirteçlerini, zaman sınırlı, kullanıcıya özgü bir anahtar olarak düşünebilirsiniz. Bu, yalnızca istediği belirli kaynakların belirli bölümlerinin kilidini açabilir.

**Remoterenderingcoordinator** betiği, uzak oturum yönetimini yapılandırmak için kullanılan bir **Azurefrontendaccountınfo** nesnesi döndüren bir yöntemi tutan **ARRCredentialGetter**adlı bir temsilciye sahiptir. **ARRCredentialGetter**'e farklı bir yöntem atayabiliriz. Azure erişim belirteci Içeren bir **Azurefrontendaccountınfo** nesnesi oluşturarak Azure oturum açma akışı kullanmamızı sağlar. Bu erişim belirteci, oturum açan kullanıcıya özgü olur.

1. [Nasıl yapılır: dağıtılan uygulamalar için kimlik doğrulama-kimlik doğrulamasını yapılandırma](../../../how-tos/authentication.md#authentication-for-deployed-applications), özellikle Azure uzamsal bağlayıcılarında listelenen YÖNERGELERI [Azure AD Kullanıcı kimlik doğrulaması](https://docs.microsoft.com/azure/spatial-anchors/concepts/authentication?tabs=csharp#azure-ad-user-authentication)' nda takip edersiniz. Bu, yeni bir Azure Active Directory uygulamasının kaydolmasını ve ARR örneğinizin erişimini yapılandırmayı içerir.
1. Yeni AAD uygulamasını yapılandırdıktan sonra AAD uygulamanızın aşağıdaki görüntüler gibi göründüğünü denetleyin:

    **AAD uygulama-> kimlik doğrulaması** ![ Uygulama kimlik doğrulaması](./media/app-authentication-public.png)

    **AAD uygulama-> API izinleri** ![ Uygulama API 'Leri](./media/request-api-permissions-step-five.png)

1. Uzaktan Işleme Hesabınızı yapılandırdıktan sonra, yapılandırmanızın aşağıdaki görüntüde olduğu gibi göründüğünü denetleyin:

    **AAR-> AccessControl (IAM)** ![ ARR rolü](./media/azure-remote-rendering-role-assignment-complete.png)

    >[!NOTE]
    > *Sahip* rolü, istemci uygulaması aracılığıyla oturumları yönetmek için yeterli değildir. Oturumları yönetme olanağı vermek istediğiniz her kullanıcı için, rol **Uzaktan Işleme istemcisi**sağlamalısınız. Oturumları yönetmek ve modelleri dönüştürmek istediğiniz her kullanıcı için, rol **Uzaktan Işleme Yöneticisi**sağlamanız gerekir.

Azure 'un her şeyi sunduğumuz için, kodunuzun AAR hizmetine nasıl bağlandığını değiştirmeniz gerekir. Bunu, yeni bir **Azurefrontendaccountınfo** nesnesi döndürecek bir **BaseARRAuthentication**örneğini uygulayarak yapacağız. Bu durumda, hesap bilgileri Azure erişim belirteciyle yapılandırılır.

1. **Aadauthentication** adlı yeni bir betik oluşturun ve kodunu aşağıdaki kodla değiştirin:

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License. See LICENSE in the project root for license information.

    using Microsoft.Azure.RemoteRendering;
    using Microsoft.Identity.Client;
    using System;
    using System.Linq;
    using System.Threading;
    using System.Threading.Tasks;
    using UnityEngine;

    public class AADAuthentication : BaseARRAuthentication
    {
        [SerializeField]
        private string accountDomain;
        public string AccountDomain
        {
            get => accountDomain.Trim();
            set => accountDomain = value;
        }

        [SerializeField]
        private string activeDirectoryApplicationClientID;
        public string ActiveDirectoryApplicationClientID
        {
            get => activeDirectoryApplicationClientID.Trim();
            set => activeDirectoryApplicationClientID = value;
        }

        [SerializeField]
        private string azureTenantID;
        public string AzureTenantID
        {
            get => azureTenantID.Trim();
            set => azureTenantID = value;
        }

        [SerializeField]
        private string azureRemoteRenderingAccountID;
        public string AzureRemoteRenderingAccountID
        {
            get => azureRemoteRenderingAccountID.Trim();
            set => azureRemoteRenderingAccountID = value;
        }

        public override event Action<string> AuthenticationInstructions;

        string authority => "https://login.microsoftonline.com/" + AzureTenantID;

        string redirect_uri = "https://login.microsoftonline.com/common/oauth2/nativeclient";

        string[] scopes => new string[] { "https://sts.mixedreality.azure.com/mixedreality.signin" };

        public void OnEnable()
        {
            RemoteRenderingCoordinator.ARRCredentialGetter = GetAARCredentials;
            this.gameObject.AddComponent<ExecuteOnUnityThread>();
        }

        public async override Task<AzureFrontendAccountInfo> GetAARCredentials()
        {
            var result = await TryLogin();
            if (result != null)
            {
                Debug.Log("Account signin successful " + result.Account.Username);

                var AD_Token = result.AccessToken;

                return await Task.FromResult(new AzureFrontendAccountInfo(AccountDomain, AzureRemoteRenderingAccountID, "", AD_Token, ""));
            }
            else
            {
                Debug.LogError("Error logging in");
            }
            return default;
        }

        private Task DeviceCodeReturned(DeviceCodeResult deviceCodeDetails)
        {
            //Since everything in this task can happen on a different thread, invoke responses on the main Unity thread
            ExecuteOnUnityThread.Enqueue(() =>
            {
                // Display instructions to the user for how to authenticate in the browser
                Debug.Log(deviceCodeDetails.Message);
                AuthenticationInstructions?.Invoke(deviceCodeDetails.Message);
            });

            return Task.FromResult(0);
        }

        public override async Task<AuthenticationResult> TryLogin()
        {
            var clientApplication = PublicClientApplicationBuilder.Create(ActiveDirectoryApplicationClientID).WithAuthority(authority).WithRedirectUri(redirect_uri).Build();
            AuthenticationResult result = null;
            try
            {
                var accounts = await clientApplication.GetAccountsAsync();

                if (accounts.Any())
                {
                    result = await clientApplication.AcquireTokenSilent(scopes, accounts.First()).ExecuteAsync();

                    return result;
                }
                else
                {
                    try
                    {
                        result = await clientApplication.AcquireTokenWithDeviceCode(scopes, DeviceCodeReturned).ExecuteAsync(CancellationToken.None);
                        return result;
                    }
                    catch (MsalUiRequiredException ex)
                    {
                        Debug.LogError("MsalUiRequiredException");
                        Debug.LogException(ex);
                    }
                    catch (MsalServiceException ex)
                    {
                        Debug.LogError("MsalServiceException");
                        Debug.LogException(ex);
                    }
                    catch (MsalClientException ex)
                    {
                        Debug.LogError("MsalClientException");
                        Debug.LogException(ex);
                        // Mitigation: Use interactive authentication
                    }
                    catch (Exception ex)
                    {
                        Debug.LogError("Exception");
                        Debug.LogException(ex);
                    }
                }
            }
            catch (Exception ex)
            {
                Debug.LogError("GetAccountsAsync");
                Debug.LogException(ex);
            }

            return null;
        }
    }
    ```

>[!NOTE]
> Bu kod, hayır anlamına gelir ve ticari bir uygulama için henüz hazırlanmaz. Örneğin, en azından, muhtemelen oturumu kapatma olanağını da eklemek isteyeceksiniz. Bu, `Task RemoveAsync(IAccount account)` istemci uygulaması tarafından sunulan yöntemi kullanılarak yapılabilir. Bu kod yalnızca öğretici kullanımına yöneliktir, uygulamanız uygulamanıza özgüdür.

Kod önce **AquireTokenSilent**kullanarak belirteci sessizce almaya çalışır. Bu, Kullanıcı daha önce bu uygulamayı doğrulamışsa bu başarılı olur. Başarılı olmazsa, daha fazla kullanıcı ile ilgili stratejiye geçin.

Bu kod için, bir erişim belirteci almak üzere [cihaz kod akışını](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code) kullanıyoruz. Bu akış, kullanıcının bir bilgisayar veya mobil cihazda Azure hesabında oturum açmasını ve elde edilen belirtecin HoloLens uygulamasına geri gönderilmesini sağlar.

Bu sınıfın bir ARR perspektifinden en önemli bölümü bu satırdır:

```csharp
return await Task.FromResult(new AzureFrontendAccountInfo(AccountDomain, AzureRemoteRenderingAccountID, "", AD_Token, ""));
```

Burada hesap etki alanını, hesap KIMLIĞINI ve erişim belirtecini kullanarak yeni bir **Azurefrontendaccountınfo** nesnesi oluşturacağız. Bu belirteç daha sonra, Kullanıcı daha önce yapılandırılan rol tabanlı izinlere göre yetkilendirildiği sürece, uzak işleme oturumlarını sorgulamak, oluşturmak ve birleştirmek için ARR hizmeti tarafından kullanılır.

Bu değişiklik ile uygulamanın geçerli durumu ve Azure kaynaklarınıza erişimi şöyle görünür:

![Daha da iyi güvenlik](./media/security-three.png)

Kullanıcı kimlik bilgileri cihazda depolanmadığından (veya cihaza girilse bile bu durumda), bunların etkilenme riski çok düşüktür. Artık cihaz, blob depolamaya erişmek için erişim denetimi (ıAM) kullanan ARR 'ye erişmek için kullanıcıya özgü, zaman sınırlı bir erişim belirteci kullanıyor. Bu iki adım, kaynak koddan "parolalar" tamamen kaldırılmıştır ve güvenlik önemli ölçüde artar. Ancak, bu en fazla güvenlik kullanılabilir değildir ve model ve oturum yönetiminin bir Web hizmetine taşınması, güvenliği daha da iyileştirir. Ek güvenlik konuları [ticari hazırlık](../commercial-ready/commercial-ready.md) bölümünde ele alınmıştır.

### <a name="testing-aad-auth"></a>AAD kimlik doğrulamasını test etme

Unity düzenleyicisinde, AAD kimlik doğrulaması etkin olduğunda, uygulamayı her başlattığınızda kimlik doğrulaması yapmanız gerekir. Cihazda, kimlik doğrulama adımı ilk kez gerçekleşir ve yalnızca belirtecin süresi dolarsa veya geçersiz kılındıktan sonra gerekli olur.

1. **Aadauthentication** bileşenini **Remoterenderingcoordinator** oyunobject öğesine ekleyin.

    ![AAD kimlik doğrulama bileşeni](./media/azure-active-directory-auth-component.png)

1. Istemci KIMLIĞI ve kiracı KIMLIĞI için değerlerinizi girin. Bu değerler, uygulama kaydınızın Genel Bakış sayfasında bulunabilir:

    * **Hesap etki alanı** , **Remoterenderingcoordinator**'ın hesap etki alanında kullandığınız etki alanıdır.
    * **Active Directory Uygulama ISTEMCI kimliği** , AAD uygulamanızın kaydında bulunan *uygulama (istemci) kimliğidir* (aşağıdaki resme bakın).
    * **Azure KIRACı kimliği** , AAD uygulama kaydınızda bulunan *Dizin (kiracı) kimliğidir* (aşağıdaki resme bakın).
    * **Azure uzaktan Işleme hesap kimliği** , **Remoterenderingcoordinator**IÇIN kullanmakta olduğunuz **Hesap kimliğidir** .

    ![AAD kimlik doğrulama bileşeni](./media/app-overview-data.png)

1. Unity düzenleyicisinde oynat ' a ve bir oturumu çalıştırmak için onay düğmesine basın.
    **Aadauthentication** bileşeninin bir görünüm denetleyicisi olduğundan, oturum yetkilendirme kalıcı panelinden sonra bir istem görüntülemek için otomatik olarak bağlanır.
1. **Appmenu**'in sağındaki panelde bulunan yönergeleri izleyin.
    Bu şuna benzer bir şey görmeniz gerekir: ![ AAD kimlik doğrulama bileşeni ](./media/device-flow-instructions.png) , ikincil cihazınıza (veya aynı cihazdaki gözatıcıya) girilen kodlanmış öğeyi girdikten ve kimlik bilgilerinizi kullanarak oturum açtıktan sonra, istekte bulunan uygulamaya bir erişim belirteci döndürülür, bu durumda Unity Düzenleyicisi olur.
1. Bu noktadan sonra, uygulamadaki her şey normal olarak devam etmelidir. Evrelerin beklendiği gibi ilerlemeden devam ediyorsanız, Unity konsolunda herhangi bir hata olup olmadığını denetleyin.

## <a name="build-to-device"></a>Cihaza oluştur

MSAL to Device kullanarak bir uygulama oluşturuyorsanız projenizin **varlıklar** klasörüne bir dosya eklemeniz gerekir. Bu, derleyicinin **öğretici varlıklarına**dahil olan *Microsoft.Identity.Client.dll* kullanarak uygulamayı doğru şekilde oluşturmasına yardımcı olur.

1. **link.xml** adlı **varlıklara** yeni bir dosya ekleyin
1. Dosyasına aşağıdakileri ekleyin:

    ```xml
    <linker>
        <assembly fullname="Microsoft.Identity.Client" preserve="all"/>
        <assembly fullname="System.Runtime.Serialization" preserve="all"/>
        <assembly fullname="System.Core">
            <type fullname="System.Linq.Expressions.Interpreter.LightLambda" preserve="all" />
        </assembly>
    </linker>
    ```

1. Değişiklikleri Kaydet

Hızlı Başlangıç bölümünde bulunan adımları izleyin: HoloLens 'te [Unity örneği dağıtma-](../../../quickstarts/deploy-to-hololens.md#build-the-sample-project)HoloLens 'e derlemek Için örnek projeyi derleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici kümesinin geri kalanı, Azure uzaktan Işleme kullanan üretime yönelik bir uygulama oluşturmaya yönelik kavramsal konular içerir.

> [!div class="nextstepaction"]
> [Sonraki: ticari hazırlık](../commercial-ready/commercial-ready.md)
