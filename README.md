# Programming Model

Version 1: https://docs.spring.io/spring-cloud-stream/docs/1.0.0.RC2/reference/html/_programming_model.html
* Bound interfaces
* Bound channels

Version 2: https://docs.spring.io/spring-cloud-stream/docs/Elmhurst.RELEASE/reference/html/_programming_model.html 
* bindings
* bindable components - MessageChannel for outbound and SubscribableChannel for inbound (PollableMessageSource for a pollable consumer)
* @EnableBinding, @Qualifier, @StreamListener

