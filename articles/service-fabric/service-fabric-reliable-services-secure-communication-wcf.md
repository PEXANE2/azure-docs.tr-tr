---
title: Güvenli WCF tabanlı hizmet iletişimi
description: Azure Hizmet Kumaşı kümesinde çalışan güvenilir hizmetler için WCF tabanlı iletişimleri nasıl güvence altına alabildiğini öğrenin.
author: suchiagicha
ms.topic: conceptual
ms.date: 04/20/2017
ms.author: pepogors
ms.openlocfilehash: ca5eafa4612503a13f80b7f238e4827979c0358b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614171"
---
# <a name="secure-wcf-based-communications-for-a-service"></a>Bir hizmet için WCF tabanlı güvenli iletişim
Güvenlik iletişimin en önemli yönlerinden biridir. Güvenilir Hizmetler uygulama çerçevesi, güvenliği artırmak için kullanabileceğiniz birkaç önceden oluşturulmuş iletişim yığını ve aracı sağlar. Bu makalede, hizmet remoting kullanırken güvenliği artırmak için nasıl hakkında konuşuyor.

Güvenilir hizmetler için WCF tabanlı bir iletişim yığınının nasıl ayarlanacağını açıklayan varolan bir [örnek](service-fabric-reliable-services-communication-wcf.md) kullanıyoruz. WCF tabanlı bir iletişim yığını kullanırken bir hizmetin güvenliğini sağlamak için aşağıdaki adımları izleyin:

1. Hizmet için, oluşturduğunuz WCF iletişim dinleyicisinin`WcfCommunicationListener`güvenliğini sağlamanız gerekir. Bunu yapmak için `CreateServiceReplicaListeners` yöntemi değiştirin.

    ```csharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        return new[]
        {
            new ServiceReplicaListener(
                this.CreateWcfCommunicationListener)
        };
    }

    private WcfCommunicationListener<ICalculator> CreateWcfCommunicationListener(StatefulServiceContext context)
    {
       var wcfCommunicationListener = new WcfCommunicationListener<ICalculator>(
            serviceContext:context,
            wcfServiceObject:this,
            // For this example, we will be using NetTcpBinding.
            listenerBinding: GetNetTcpBinding(),
            endpointResourceName:"WcfServiceEndpoint");

        // Add certificate details in the ServiceHost credentials.
        wcfCommunicationListener.ServiceHost.Credentials.ServiceCertificate.SetCertificate(
            StoreLocation.LocalMachine,
            StoreName.My,
            X509FindType.FindByThumbprint,
            "9DC906B169DC4FAFFD1697AC781E806790749D2F");
        return wcfCommunicationListener;
    }

    private static NetTcpBinding GetNetTcpBinding()
    {
        NetTcpBinding b = new NetTcpBinding(SecurityMode.TransportWithMessageCredential);
        b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
        return b;
    }
    ```
2. İstemcide, `WcfCommunicationClient` önceki [örnekte](service-fabric-reliable-services-communication-wcf.md) oluşturulan sınıf değişmeden kalır. Ama `CreateClientAsync` yöntemi geçersiz kılmak `WcfCommunicationClientFactory`gerekir:

    ```csharp
    public class SecureWcfCommunicationClientFactory<TServiceContract> : WcfCommunicationClientFactory<TServiceContract> where TServiceContract : class
    {
        private readonly Binding clientBinding;
        private readonly object callbackObject;
        public SecureWcfCommunicationClientFactory(
            Binding clientBinding,
            IEnumerable<IExceptionHandler> exceptionHandlers = null,
            IServicePartitionResolver servicePartitionResolver = null,
            string traceId = null,
            object callback = null)
            : base(clientBinding, exceptionHandlers, servicePartitionResolver,traceId,callback)
        {
            this.clientBinding = clientBinding;
            this.callbackObject = callback;
        }

        protected override Task<WcfCommunicationClient<TServiceContract>> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
        {
            var endpointAddress = new EndpointAddress(new Uri(endpoint));
            ChannelFactory<TServiceContract> channelFactory;
            if (this.callbackObject != null)
            {
                channelFactory = new DuplexChannelFactory<TServiceContract>(
                this.callbackObject,
                this.clientBinding,
                endpointAddress);
            }
            else
            {
                channelFactory = new ChannelFactory<TServiceContract>(this.clientBinding, endpointAddress);
            }
            // Add certificate details to the ChannelFactory credentials.
            // These credentials will be used by the clients created by
            // SecureWcfCommunicationClientFactory.  
            channelFactory.Credentials.ClientCertificate.SetCertificate(
                StoreLocation.LocalMachine,
                StoreName.My,
                X509FindType.FindByThumbprint,
                "9DC906B169DC4FAFFD1697AC781E806790749D2F");
            var channel = channelFactory.CreateChannel();
            var clientChannel = ((IClientChannel)channel);
            clientChannel.OperationTimeout = this.clientBinding.ReceiveTimeout;
            return Task.FromResult(this.CreateWcfCommunicationClient(channel));
        }
    }
    ```

    Bir `SecureWcfCommunicationClientFactory` WCF iletişim istemcisi oluşturmak için kullanın (`WcfCommunicationClient`). Hizmet yöntemlerini çağırmak için istemciyi kullanın.

    ```csharp
    IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();

    var wcfClientFactory = new SecureWcfCommunicationClientFactory<ICalculator>(clientBinding: GetNetTcpBinding(), servicePartitionResolver: partitionResolver);

    var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
        wcfClientFactory,
        ServiceUri,
        ServicePartitionKey.Singleton);

    var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
        client => client.Channel.Add(2, 3)).Result;
    ```

Bir sonraki adım olarak, [Güvenilir Hizmetler OWIN ile Web API](service-fabric-reliable-services-communication-webapi.md)okuyun.
