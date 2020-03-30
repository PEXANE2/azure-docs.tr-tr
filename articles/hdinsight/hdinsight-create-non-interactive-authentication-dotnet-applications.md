---
title: Etkileşimli olmayan kimlik doğrulama .NET uygulaması - Azure HDInsight
description: Azure HDInsight'ta etkileşimli olmayan kimlik doğrulama Microsoft .NET uygulamalarını nasıl oluşturabilirsiniz öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/23/2019
ms.openlocfilehash: 5e6a0586bc750f8972586920c15dbb297295aa20
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371282"
---
# <a name="create-a-non-interactive-authentication-net-hdinsight-application"></a>Etkileşimli olmayan bir kimlik doğrulama .NET HDInsight uygulaması oluşturma

Microsoft .NET Azure HDInsight uygulamanızı uygulamanın kendi kimliği (etkileşimli olmayan) veya uygulamanın oturum açmış kullanıcısı (etkileşimli) kimliği altında çalıştırın. Bu makalede, Azure'a bağlanmak ve HDInsight'ı yönetmek için etkileşimli olmayan bir kimlik doğrulama .NET uygulaması nasıl oluşturulabileceğinizgösterilmektedir. Etkileşimli bir uygulama örneği [için](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight)bkz.

Etkileşimli olmayan .NET uygulamanızdan şunları yapmanız gerekir:

* Azure abonelik kiracı kimliğiniz *(dizin kimliği*olarak da adlandırılır). Bkz. [Kiracı kimliğini alın.](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in)
* Azure Etkin Dizin (Azure AD) uygulama istemci kimliği. Bkz. [Azure Etkin Dizin uygulaması oluşturun](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) ve uygulama kimliği [alın.](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in)
* Azure AD uygulaması gizli anahtarı. Bkz. [Uygulama kimlik doğrulama anahtarını alın.](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in)

## <a name="prerequisites"></a>Ön koşullar

Bir HDInsight kümesi. Başlangıç [öğretici](hadoop/apache-hadoop-linux-tutorial-get-started.md)bakın.

## <a name="assign-a-role-to-the-azure-ad-application"></a>Azure AD uygulamasına rol atama

Azure AD uygulamanıza eylem gerçekleştirme izni vermek için bir [rol](../role-based-access-control/built-in-roles.md)atayın. Kapsamı abonelik, kaynak grubu veya kaynak düzeyinde ayarlayabilirsiniz. İzinler daha düşük kapsam düzeylerine devralınır. Örneğin, bir kaynak grubu için Okuyucu rolüne bir uygulama eklemek, uygulamanın kaynak grubunu ve içinde bulunan tüm kaynakları okuyabileceği anlamına gelir. Bu makalede, kapsamı kaynak grubu düzeyinde ayarlarsınız. Daha fazla bilgi için Azure [abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullan'a](../role-based-access-control/role-assignments-portal.md)bakın.

**Azure AD uygulamasına Sahip rolünü eklemek için**

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Bu makalede daha sonra Kovan sorgunuzu çalıştıracağınız HDInsight kümesine sahip kaynak grubuna gidin. Çok sayıda kaynak grubunuz varsa, istediğinizi bulmak için filtreyi kullanabilirsiniz.
1. Kaynak grubu menüsünde **Access denetimi (IAM)** seçeneğini belirleyin.
1. Geçerli rol **atamalarını** görmek için Rol atamaları sekmesini seçin.
1. Sayfanın üst kısmında **+ Ekle'yi**seçin.
1. Azure REKLAM uygulamanıza Kurucu rolünü eklemek için yönergeleri izleyin. Rolü başarıyla ekledikten sonra, uygulama Sahibi rolü altında listelenir.

## <a name="develop-an-hdinsight-client-application"></a>HDInsight istemci uygulaması geliştirme

1. Bir C# konsol uygulaması oluşturun.
2. Aşağıdaki [NuGet](https://www.nuget.org/) paketlerini ekleyin:

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. Aşağıdaki kodu çalıştırın:

    ```csharp
    using System;
    using System.Security;
    using Microsoft.Azure;
    using Microsoft.Azure.Common.Authentication;
    using Microsoft.Azure.Common.Authentication.Factories;
    using Microsoft.Azure.Common.Authentication.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.HDInsight;
    
    namespace CreateHDICluster
    {
        internal class Program
        {
            private static HDInsightManagementClient _hdiManagementClient;
    
            private static Guid SubscriptionId = new Guid("<Enter your Azure subscription ID>");
            private static string tenantID = "<Enter your tenant ID (also called directory ID)>";
            private static string applicationID = "<Enter your application ID>";
            private static string secretKey = "<Enter the application secret key>";
    
            private static void Main(string[] args)
            {
                var key = new SecureString();
                foreach (char c in secretKey) { key.AppendChar(c); }
    
                var tokenCreds = GetTokenCloudCredentials(tenantID, applicationID, key);
                var subCloudCredentials = GetSubscriptionCloudCredentials(tokenCreds, SubscriptionId);
    
                var resourceManagementClient = new ResourceManagementClient(subCloudCredentials);
                resourceManagementClient.Providers.Register("Microsoft.HDInsight");
    
                _hdiManagementClient = new HDInsightManagementClient(subCloudCredentials);
    
                var results = _hdiManagementClient.Clusters.List();
                foreach (var name in results.Clusters)
                {
                    Console.WriteLine("Cluster Name: " + name.Name);
                    Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
                    Console.WriteLine("\t Cluster location: " + name.Location);
                    Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
                }
                Console.WriteLine("Press Enter to continue");
                Console.ReadLine();
            }
    
            /// Get the access token for a service principal and provided key.          
            public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
            {
                var authFactory = new AuthenticationFactory();
                var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
                var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
                var accessToken =
                    authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
    
                return new TokenCloudCredentials(accessToken);
            }
    
            public static SubscriptionCloudCredentials GetSubscriptionCloudCredentials(SubscriptionCloudCredentials creds, Guid subId)
            {
                return new TokenCloudCredentials(subId.ToString(), ((TokenCloudCredentials)creds).Token);
            }
        }
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portalında bir Azure Etkin Dizin uygulaması ve hizmet ilkesi oluşturun.](../active-directory/develop/howto-create-service-principal-portal.md)
* Azure Kaynak Yöneticisi ile bir hizmet yöneticisinin kimliğini nasıl [doğrulayacaä](../active-directory/develop/howto-authenticate-service-principal-powershell.md)ını öğrenin.
* Azure [Role Tabanlı Erişim Denetimi (RBAC)](../role-based-access-control/role-assignments-portal.md)hakkında bilgi edinin.
