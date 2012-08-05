oglabs-springhelpers
====================

Useful classes extending Spring libraries.

Currently all are for Spring WS

- XslTransformingMarshaller extends Jaxb2Marshaller to transform incoming XML via an XSL before unmarshalling. It is a drop-in replacement for the standard Jaxb2Marshaller. This class is useful if you are consuming a webservice that has an ugly response that results in objects you don't want. You can use XSLT to transform the response into an XML structure more to your liking and it will then unmarshal into the objects you want.
- PeoplesoftFaultMessageResolver is built to provide sensible exceptions when you're consuming a Peoplesoft web service and it throws a fault. Rather than getting really unhelpful fault messages like "null", this pulls the first MessageID, StatusCode, and DefaultMessage it runs across and puts it in an IOException. This allows the exception to provide at least basic info about the fault regardless of whether it is a custom fault or one of the low level faults (like password is wrong). Simply configure this class as the faultMessageResolver on your Spring WebServiceTemplate.
- FaultObjectResolver is an interface that can be used in conjunction with PeoplesoftFaultMessageResolver so that you can get custom fault objects unmarshalled by JAXB rather than having to deal with the XML of the fault message. It also handles the situation where you get back a low level fault (like wrong password) that doesn't provide the custom fault objects. In this case, it just throws a standard IOException but with the MessageID, Status Code, and DefaultMessage. When you configure this properly on the PeoplesoftFaultMessageResolver, your implementation of this interface will be handed the custom fault objects specified by the Peoplesoft service's fault XSD. To use, you must first implement this interface in your own class that will use the unmarshalled fault objects. You configure your FaultObjectResolver with a Jaxb2Marshaller that can handle your custom fault objects and hook it up to PeoplesoftFaultMessageResolver. Presto: fault objects.

These have been tested with the webservices provided by PeopleTools 8.52 but should be used as examples, not production-ready code. No guarantees or warantees.