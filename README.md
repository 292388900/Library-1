#  Library
A microservice project using .NET Core 2.2, DDD, CQRS, Event Sourcing, Redis and RabbitMQ.

## Components
![Components](https://github.com/lamondlu/BookLibrary/blob/master/docs/Architecture/20180108201702.png)

## System Architecture
![System Architecture](https://github.com/lamondlu/BookLibrary/blob/master/docs/Architecture/20171107104353.png)

## Prerequisites
- Visual Studio 2017 (For development and debug propose)
- RabbitMQ
- Redis 
- Consul (Service Discovery and registeration)
- Consult Template (Update and Restart Nginx)
- Nginx (For Load Balance)
- MongoDB (For Event Store)

## EDA 
![EDA](https://github.com/lamondlu/BookLibrary/blob/master/docs/Architecture/20171108152513.png)

## Service Discovery 
We will use the Nginx, Consul, Consul Template to create an service discovery and service registeration mechanism.

![Service Discovery](https://github.com/lamondlu/BookLibrary/blob/master/docs/Architecture/20180108211340.png)

- The nginx will do the load balance work.
- All the web or api instances will be registered in the consul server with the `SelfRegister` method.
- Consul template will listen the consul server, if there is new instance, consul template will update the nginx.conf with assigned template and restart the nginx server, so the new instance will be connected with the nginx server correctly


Use the Log service as an example.


    public void SelfRegister()
    {
        var serviceDiscovery = InjectContainer.GetInstance<IServiceDiscovery>();
        serviceDiscovery.RegisterService(new Infrastructure.Operation.Core.Models.Service
        {
            Port = 5003,
            ServiceName = "LogService",
            Tag = "Microservice API"
        });

        Console.WriteLine("Register to consul successfully.");
    }