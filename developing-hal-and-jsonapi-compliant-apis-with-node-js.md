
# Developing HAL and JSON:API Compliant APIs with Node.js
As a backend developer at [Hybrid Web Agency](https://hybridwebagency.com/), focused on crafting maintainable and intuitive APIs, I'm continually challenging myself to learn new techniques and standards. My latest project at Hybrid Web Agency pushed me well outside my typical work - building a hypermedia-driven API from the ground up in Node.js.

My goal was both ambitious and practical: implement the HATEOAS architectural style according to the HAL and JSON:API specifications through dedicated experimentation and testing. This would provide extensive hands-on experience applying emerging best practices.

Over six months, I developed a fully-featured order management system designed around hypermedia principles. Responses included links between related resources, allowing deep evaluation of the two specifications. Along the way, I encountered challenges that revealed nuanced differences in approaches.

In this post, I will discuss the insights uncovered during my research and implementation. Code examples will demonstrate how each spec structures interconnected order, product and customer resources.

Most importantly, sharing my learnings from this journey aims to benefit fellow API designers exploring representational state transfer techniques. It also allows me to continually expand my understanding of how to craft elegant yet future-proof 






### What is HATEOAS?

HATEOAS (Hypermedia as the Engine of Application State) defines a RESTful architectural style centered on the use of hyperlinks to drive client-server communication. Rather than clients relying on out-of-band documentation to understand an API, HATEOAS dictates that all interaction metadata should be contained within or accessible from responses.

By including hypermedia controls like links, forms, and embedded URIs, HATEOAS-compliant APIs provide the interactive context needed for clients to navigate the API dynamically. For example, retrieving an order may disclose hyperlinks to applicable invoices, products, statuses, or other related resources.

This decouples clients from static knowledge of implementation specifics like endpoints or relationship mappings. The API is free to evolve independently through structural changes without breaking existing integrations. Clients simply follow the hypermedia links to discover updated or expanded functionality.

Adopting HATEOAS establishes interfaces that are inherently explorable, flexible to change, and independent of any predefined usage schema. Resources and controls contained in responses effectively steer clients through valid state transitions and available actions.

In summary, the HATEOAS approach defines RESTful systems entirely driven by opaque hyperlinks within transmissions. This hypermedia-led design cultivates resilience, extensibility and “self-documenting” APIs that convey their own interactive context through each request-response exchange.



### Benefits of HATEOAS

APIs designed according to HATEOAS principles gain several important advantages:

**Self-Describing Dialogue** - Each response describes its own interactive context through embedded hypermedia links, removing the need for external documentation. 

** Independence** - Clients depend solely on the hypermedia format, uncoupling them from invariant resources or request patterns. This allows the API to evolve autonomously.

**Resilience to Change** - Augmenting the domain model through new relations or endpoints is seamlessly discovered by pre-existing clients following hypertext prompts. 

**Progressive Capabilities** - Hypermedia responses facilitate expanding functionality organically over time. Links signal extensibility points for future growth beyond initial designs.  

**Serendipitous Discovery** - By dynamically learning valid transitions, clients can leverage little-known properties or operations without advance notices.

**Adaptable Traversal** - HATEOAS frees clients from rigid usage directives, allowing data-driven routes tailored for distinct operational needs.

Overall, hypermedia-driven design imbues REST interfaces with natural pliability, teachability and longevity through an emphasize on self-describing resource interactions instead of static contracts.






## Choosing a Hypermedia Specification

When developing HATEOAS-compliant Node APIs, two common options for representing hypermedia links are Hypertext Application Language (HAL) and JSON:API.

### Hypertext Application Language

HAL provides a basic yet straightforward hypermedia format using a top-level "_links" object to associate resources via URLs. 

```json
{
  "_links": {
    "self": {"href": "/orders/1"},
    "items": {"href": "/orders/1/items"}
  }
}
```

It keeps implementations simple while meeting core hypermedia needs.

### JSON:API

JSON:API establishes a robust standard, connecting resources through a "relationships" object containing related IDs and explicitly linked reference data.

```
  "relationships": {
    "items": {  
      "links": {
        "related": "/orders/1/relationships/items"  
      },
      "data": [...]
    }
  }
```

This fully-featured format supports advanced use cases but introduces more complexity.

### Key Factors

For lightweight needs, HAL's streamlined approach may suffice. JSON:API acts as a comprehensive framework well-suited to large, standardized systems. Consider an API's scope, scale and hypermedia purpose to determine the best-fitting specification. Both enable the HATEOAS design approach.





## Creating a HAL API in Node.js

Setting up an API that follows the HAL specification using Express.

### Adding Dependencies

Bringing in the `express-hal-builder` module to generate HAL representations. 

```
npm install express-hal-builder
```

### Defining Resource Schemas

Creating schema objects for Order and OrderItem resources.

```js
const Order = {/*...*/};  
const OrderItem = {/*...*/};
```

### Generating Hypermedia

Rendering resources and including embedded links using the builder:

```js 
app.get('/orders/:id', (req, res) => {

  const order = /*...*/;
  const builder = new HalBuilder();

  builder.representEntity(order)
    .childLink(order.items, '/orders/'+id+'/items');

  res.json(builder.get());

})
```

This establishes the fundamental structure for a HAL-based API.


## Discovering Resources and Relationships in a HAL API

By making initial requests and parsing responses:

- Map out the exposed endpoints  
- Identify supported resource and link types
- Extract URL values used to navigate between related resources
- Gradually reveal the domain model by traversing embedded links

## Building a Connected Data API with JSON:API

This guide demonstrates how to develop a backend service exposing the relationships between resources using JSON:API with Node and Express.

### Importing Required Modules

The `jsonapi-serializer` package helps generate standardized JSON:API documents.

### Modeling Core Entity Models

Schemas define the attributes and structure of resources like orders and line items.  

### Generating JSON:API Response Documents

To relate data across resources:

1. Represent a root or parent resource 

2. Embed links to associated child resources

```js
app.get('/orders/:id', async (req, res) => {

  const order = await Order.findById(req.params.id);

  res.json(order.serialize({
    include: 'lineItems'
  }));

});
```

This provides a starting point for building connected REST APIs according to the JSON:API specification through standardized resource and relationship representations.

# Conclusion

In summary, developing REST APIs using established specifications provides structure, discoverability, and future-proofs applications. Following standards helps ensure intuitive and explorable APIs for consumers.

Building specification-driven, connected data services can be complex, requiring varied expertise. This is where partnering with an experienced Node.js firm can help expedite completion.

As a full-service Node.js development company located in Anaheim, we offer [Node.js Development Services in Anaheim](https://hybridwebagency.com/anaheim-ca/custom-laravel-development-services/) to assist with robust, specification-driven API implementation. Our senior engineers have extensive experience building HAL, JSON:API and other standard-compliant APIs. Whether for POC development, scaling existing services, or managing full projects, we aim to deliver production-ready REST endpoints on-time and on-budget.

By leveraging our Node.js and framework proficiency, alongside best practices for API architecture, your team can prioritize core work while we ensure technical execution meets quality and performance standards. Contact us to discuss how our Anaheim-based services could support your project goals.

## References

- https://www.halspec.org/
The official specification website for HAL (Hypertext Application Language), detailing the standard.

- https://jsonapi.org/
The official website for the JSON:API specification, including documentation on compliant API design. 

- https://expressjs.com/en/advanced/best-practice-performance.html
Best practices guide from the Express.js website covering performance, security, and standard compliance for Node.js APIs.  

- https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design
Documentation from Microsoft on API design best practices, including modeling relationships and using standards.

- https://swagger.io/resources/building-blocks/hypermedia/
Swagger overview of hypermedia APIs and industry standards like HAL and JSON:API for enabling discoverability.
